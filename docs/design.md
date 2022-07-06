# S/BLOW5 Design Goals

*S/BLOW5* is a file format designed for the bioinformatics research community and hovers around three major design principles:

1. Simplicity and usability
2. Reading performance for common analysis workloads
3. Efficient file size


## Simplicity and usability

- Follows the seminal *SAM/BAM* philosophy the community is already familiar with. *SLOW5 ASCII* is for human readability, and *BLOW5* binary with optional compression is for efficient performance and file size. [*slow5lib*](https://github.com/hasindu2008/slow5lib), and [*slow5tools*](https://github.com/hasindu2008/slow5tools) also follows a similar philosophy the community is already familiar with. There is no need to follow a completely different approach just for the sake of showing that it is novel when something has already worked so far. Thus, minimal learning curve.
- Simple format specification and thus library implementation is simple.
- *slow5lib*, the reference library designed for *S/BLOW5*, has minimal dependencies (only *zlib* is required)  and thus, compiles with a single `make` command (inspired by [*minimap2*](https://github.com/lh3/minimap2) compilation).  
Only a C compiler that supports C99 standard with X/Open 7 POSIX 2008 extensions is required. 
Thus, supports numerous platforms/architectures.
- Purpose-built for nanopore data and thus unnecessary convoluted features are not present like in a general data format.
- Well documented and a [detailed specification](https://hasindu2008.github.io/slow5specs/) is available. Versioning is properly thought out and changes will be backward compatible. No ad-hoc changes will be done and thus will be stable.
- Only a few lines of code are required to write a program, rather than using a generic format like [*FAST5*](https://hasindu2008.github.io/slow5specs/fast5_demystified.pdf). 
- Community developed tools can support a *SLOW5* file in their project in no time compared to the effort required to handle existing *FAST5*.

We believe that simplicity and usability translate to improved scientist efficiency and thus are as important as computational performance. This also makes *BLOW5* an ideal choice for archiving as no adhoc changes will be done and even after 5 or 10 years it can be compiled on a system as long as a c compiler is available. In a general-purpose format, especially if dependent on multiple heavyweight libraries, a deprecation of a single dependency down the chain by that time would cause a nightmare to compile an earlier version.

## Reading performance for common analysis workloads
- Conforms to the [principle of temporal and spatial locality](https://en.wikipedia.org/wiki/Locality_of_reference) in computer architecture that is the foundation for a modern computing system comprised of a memory hierarchy. *S/BLOW5* format is primarily for reading nanopore raw signal data. All per-read fields are kept in a contiguous block along with the raw signal to preserve the locality of access. That is if the raw signal is accessed it is highly likely that all fields relevant to the pico-ampere conversion are accessed soon after. Thus, they are kept continuously so that seek operations are reduced (and cache-friendly as well).
- *S/BLOW5* can be sequential read (streamed), which is the fastest mode of disk reading for any type of disk (this is more visible in HDD than SSD). Sequential access is extremely beneficial for HPC workloads where dozens of terabytes of SSD are typically not available. Suits best for operations like base-calling, signal mapping, etc. Streaming mode incurs no seek operations.
- *S/BLOW5* also supports random access for cases where streaming is not possible (e.g, [*Nanopolish*](https://github.com/jts/nanopolish)), still is designed to minimise seeks as much as possible. One read record requires only one seek operation. Slow5lib implements random access using  pread64 so that multiple threads can issue parallel requests when multiple cores are available to exploit the throughput of RAID disk systems.
- *S/BLOW5* is very lightweight and puts minimal strain on a system. Even works on a phone.


## Efficient file size
- As files are written sequentially, no fragmented space is wasted like in *FAST5*.
- Global metadata is not redundantly stored and stored only once in the header.
- Specification supports multiple compression methods both at signal level and record level. In *slow5lib* we have implemented *zlib* and *zstd* for record compression and [streamvbyte zig-zag delta](https://github.com/lemire/streamvbyte/) for signal compression. If necessary, the community can plug in any custom compression method.

Note that simplicity has been sometimes favoured over performance. For instance, slow5lib uses the well-established and widely available system calls (*read*, *write*, *pread64*) rather than using the latest asynchronous/iuring interfaces. This is because compatibility is more important, we believe. Also, we do not use the latest compiler features despite performance gains, for the sake of compatibility and portability. 

---

In addition to the above, *S/BLOW5* also features extendability and flexibility.

## Extendibility
-	Reserved bytes in the header allow adding features without breaking the backward compatibility. For instance, block-wise compression of the signal or partial decompression can be added if necessary.
-	Designed to be future proof as long as it is nanopore raw signal data, despite any ad-hoc changes done to *FAST5*. How a multitude of cases is handled are in the specification document.

## Flexibility
-	Depending on the user's needs, it can be a single *S/BLOW5* file per sample or multiple files per sample.
-	Also supports having multiple read groups so that software can easily handle samples with multiple sequencing runs. 
-	slow5tools provides an interface similar to the familiar samtools and supports operations such as merge, split and view.
-	slow5tools supports converting back and forth from *FAST5* and whatever replacement formats that come in future to replace *FAST5*. Binaries for slow5tools are provided so that the users can safely do the conversion on the computer attached to the sequencer. 

Also, with the demise of Moore’s law, the computer architectures are moving toward domain-specific systems (See the [Turing lecture from Turing award Winners in 2018 Hennessy and Patterson](https://www.acm.org/hennessy-patterson-turing-lecture) who are the pioneers in computer architecture). So, we believe that having a domain-specific format rather than a general-purpose format will be the first step necessary for that.


## Limitations

- *SLOW5* lays down the read sequentially. So, multiple reads cannot be written to a single BLOW5 file simultaneously. For an application that requires writing in parallel, it must write to separate files in parallel and merge them afterwards., similar to *SAM/BAM*, and falls well within the *EXT4* file system principles. This is a deliberate choice as random multiple writes will cause performance issues and impact file size efficiency and thus the merging overhead will be worth it.
- *slow5lib* library does not support big-endian processors. This can be addressed easily if there is a need, but we did not put effort as such systems are rare.
-	*slow5lib* cannot be natively compiled on Windows (needs *WSL* on *windows*). This also can be addressed if there is a need, but we haven’t put effort as the community is not using Windows frequently.
- *slow5lib* is a reference implementation written in C and no advanced profiling optimisations have been done as it is already super-fast. If necessary, there is room for many more improvements and optimisations.

As a final note, *S/BLOW5* is a community developed format and thus any [comments, suggestions, contributions, critiques and questions are welcome](https://github.com/hasindu2008/slow5specs/issues). 

Last, but not least, see [Heng Li's comment](https://twitter.com/lh3lh3/status/1480937811171811331):
![slow5_lh3](img/Screenshot-2022-07-06-103208.png)
