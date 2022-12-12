# Summary of SLOW5 ASCII format 

This is just a summary of the latest version of SLOW5 ASCII file format (.slow5). For the full specification and information on SLOW5 binary (called BLOW5) format, refer to the PDF links [here](https://github.com/hasindu2008/slow5specs/blob/main/docs/index.md#latest-version).

A SLOW5 ASCII file is a plain text file that uses the American Standard Code for Information Interchange (ASCII) encoding (locale: C/POSIX, code set: US-ASCII).  The file extension is .slow5. A SLOW5 file contains a header followed by the sequencing data. An example structure of a SLOW5 ASCII file with a single read group is and an example structure of a SLOW5 ASCII with multiple read groups - i.e., multiple sequencing runs - is provided below. The column/row borders, spacing and cell colours are added to increase the readability. The actual format uses tabs (‘\t’) and newlines (‘\n’) as delimiters

Example of a SLOW5 ASCII file with a single read group:


| <sub>#slow5\_version</sub>    | <sub>1.0.0</sub>                ||||||||
| ------------------ | -------------------- |-|-|-|-|-|-|-|
| <sub>#num\_read\_groups</sub> | <sub>1</sub>                    |
| <sub>@asic\_id</sub>          | <sub>0004A30B00232BEC</sub>     |
| <sub>@exp\_start\_time</sub>  | <sub>2020-01-01T00:00:00Z</sub> |
| <sub>@flow\_cell\_id</sub>    | <sub>FAH00000</sub>             |
| <sub>@run\_id</sub>           | <sub>855cdb</sub>               |
| <sub>…</sub>                  | <sub>…</sub>                    |
| <sub>#char\*</sub>            | <sub>uint32\_t</sub>            | <sub>double | <sub>double | <sub>double | <sub>double | <sub>uint64\_t</sub>   | <sub>int16\_t\*</sub>   | <sub>...</sub>   |
| <sub>#read\_id</sub>          | <sub>read\_group</sub>          | <sub>digitisation</sub>   | <sub>offset</sub>   | <sub>range</sub>   | <sub>sampling\_rate</sub>   | <sub>len\_raw\_signal | <sub>raw\_signal | <sub>...</sub>   |
| <sub>read0</sub>              | <sub>0</sub>                    | <sub>8192</sub>   | <sub>6</sub>   | <sub>1467.6</sub>   | <sub>4000</sub>   | <sub>123456 | <sub>498,492,... | <sub>... |
| <sub>read1</sub>              | <sub>0 </sub>                   | <sub>8192</sub>   | <sub>5</sub>   | <sub>1467.6</sub>   | <sub>4000</sub>   | <sub>2000 | <sub>491,491,... | <sub>... |
| <sub>…</sub>                  | <sub>… </sub>                   | <sub>…</sub>   | <sub>…</sub>   | <sub>…</sub>   | <sub>…</sub>   | <sub>… </sub>  | <sub>…</sub>   | <sub>...</sub>   |
| <sub>readN</sub>              | <sub>0 </sub>                   | <sub>8192</sub>   | <sub>3</sub>   | <sub>1467.6</sub>   | <sub>4000</sub>   | <sub>3000</sub>   | <sub>400,400,...</sub>   | <sub>...</sub>   |


Example of a SLOW5 ASCII file with multiple read groups:

| <sub>#slow5\_version</sub>    | <sub>1.0.0</sub>                ||||||||
| ------------------ | -------------------- |-|-|-|-|-|-|-|
| <sub>#num\_read\_groups</sub> | <sub>3     </sub>               |
| <sub>@asic\_id </sub>         | <sub>0004A30B00232BEC </sub>    | <sub>1004A30B00232BEC</sub> | <sub>2004A30B00232BEC</sub> |
| <sub>@exp\_start\_time</sub>  | <sub>2020-01-01T00:00:00Z </sub>| <sub>2020-01-01T00:00:00Z</sub> | <sub>2020-01-01T00:00:00Z</sub> |
| <sub>@flow\_cell\_id </sub>   | <sub>FAH00000</sub>             | <sub>FAH00001</sub> | <sub>FAH00002</sub> |
| <sub>@run\_id  </sub>         | <sub>855cdb </sub>              | <sub>855cd1</sub> | <sub>855cdc</sub> |
| <sub>…     </sub>             |   <sub>…     </sub>             | <sub>…</sub> | <sub>…</sub> |
| <sub>#char\*   </sub>         | <sub>uint32\_t </sub>           | <sub>double</sub> | <sub>double</sub> | <sub>double</sub> | <sub>double</sub> | <sub>uint64\_t</sub> | <sub>int16\_t\* | <sub>...</sub> |
| <sub>#read\_id   </sub>       | <sub>read\_group </sub>         | <sub>digitisation</sub> | <sub>offset</sub> | <sub>range</sub> | <sub>sampling\_rate</sub> | <sub>len\_raw\_signal</sub> | <sub>raw\_signal</sub> | <sub>... </sub>|
| <sub>read-0</sub>     | <sub>1    </sub>                | <sub>8192</sub> | <sub>6</sub> | <sub>1467.6</sub> | <sub>4000</sub> | <sub>4000</sub> | <sub>498,492,...</sub> | <sub>...</sub> |
| <sub>read-1</sub>             | <sub>0  </sub>                  | <sub>8192</sub> | <sub>5</sub> | <sub>1467.6</sub> | <sub>4000</sub> | <sub>2000</sub> | <sub>491,491,...</sub> | <sub>...</sub> |
| <sub>…</sub>                 | <sub>…  </sub>                  | <sub>…</sub> | <sub>…</sub> | <sub>…</sub> | <sub>…</sub> | <sub>…</sub> | <sub>…</sub> | <sub>...</sub> |
| <sub>read-N</sub>             | <sub>2  </sub>                  | <sub>8192</sub> | <sub>3</sub> | <sub>1467.6</sub> | <sub>4000</sub> | <sub>3000</sub> | <sub>400,400,...</sub> | <sub>...</sub> |

## SLOW5 Header

The SLOW5 header stores metadata regarding the experiment. Header lines start with either ‘#’ or ‘@’. The header contains two parts: the global header  and the data header.

### Global header

lines starting with ‘#’ form the global header.

- The first line of a SLOW5 ASCII file is a key-value pair that specifies the SLOW5 version. The key is separated from the value using a tab ‘\t’.
- The second line specifies the number of read groups in the file. Observe that in the single read group file example (Table 1), the value for num_read_groups is set to 1. In the second example with three read groups (Table 2) the value is set to 3.
- The last line of the header is always the field names for the subsequent per-read records.
- The second last line of the header specifies the data types of each field for the subsequent per-read records (i.e., for the fields named in the last line of the header). Further information about the fields is provided in the SLOW5 Data section below.

### Data header

The header lines that start with ‘@’ form the data header. These header lines contain ONT data attributes that are shared across multiple reads in a sequencing run (read group). For instance, the run_id and the flow_cell_id are common to all the reads in the read group and are therefore stored in the data header.

## SLOW5 Data

After the SLOW5 header, the actual data is encoded. Each line contains information about a single read and we refer to this as a record.

### Primary fields

 These fields are mandatory and must be arranged in the order that they appear below:

|Col | Field name       | Data type  | Description                                                                                                                                                           | Example value                        |
|-- | ---------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
|1| <sub>read\_id</sub>         | <sub>char\*</sub>     | <sub>A unique identifier for the read.</sub>  | <sub>00592138-f120-4ab5-9916-c5567adb8e29</sub>
|2| <sub>read\_group</sub>      | <sub>uint32\_t</sub>  | <sub>Read group identifier.</sub>                                                                                                      | <sub>0                                    |
|3| <sub>digitisation</sub>     | <sub>double</sub>     | <sub>Number of quantisation levels in the Analog to Digital Converter (ADC). That is, if the ADC is 12 bit, digitisation is 4096 (2<sup>12</sup>).</sub> | <sub>8192</sub>                                 |
|4| <sub>offset</sub>           | <sub>double</sub>     | <sub>The ADC offset error. This value is added when converting the signal to pico ampere.</sub>                                                                                  | <sub>10</sub>                                   |
|5| <sub>range</sub>            | <sub>double</sub>     | <sub>The full scale measurement range in pico amperes.</sub>                                                                                                                     | <sub>1441.389893</sub>                          |
|6| <sub>sampling\_rate</sub>   | <sub>double</sub>     | <sub>Sampling frequency of the ADC, i.e., the number of data points collected per second. </sub>                                                                                 | <sub>4000</sub>                                 |
|7| <sub>len\_raw\_signal</sub> | <sub>uint64\_t</sub>  | <sub>The number of samples in the raw signal (length of the raw\_signal vector below). </sub>                                                                                    | <sub>59676</sub>                                |
|8| <sub>raw\_signal</sub>      | <sub>int16\_t\*</sub> | <sub>The raw signal which are the direct acquisition values from the ADC and are comma separated.   </sub>                                                                       | <sub>1039,588,588,593,586….</sub>               |

Primary fields contain all the information required for a typical nanopore signal-level analysis. The raw signal can be converted to pico-ampere using the following equation:

  ```
signal_in_pico_ampere = (raw_signal + offset) * range / digitisation
```

### Auxiliary fields

These fields are optional and not bound by any strict order. Following are some common auxiliary data fields in SLOW5 format:
  
 | Field name      | Data type | Description                                                                                                                                                                                                                                                                                                                       | Example value      |
| --------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| <sub>channel\_number</sub> | <sub>char\*</sub>    | <sub>The channel number. A flow cell has multiple channels allowing multiple DNA/RNA strands to be sequenced in parallel. For instance, a MinION flow cell has 512 channels and thus can sequence 512 strands in parallel. </sub>                                                                                                             | <sub>504 </sub>                |
| <sub>median\_before</sub>  | <sub>double</sub>    | <sub>The estimated median current level immediately preceding the read. In most cases this can be used as an estimate of the open pore level. The open-pore state is when there is no strand inside the pore.</sub>                                                                                                                           | <sub>238.78225708007812 |
| <sub>read\_number</sub>    | <sub>int32\_t</sub>  | <sub>A unique number within each channel counted upwards from zero. Note that not all reads generated are “strand” reads, but only strand reads are written to the final fast5 file, so some read numbers may be absent.                                                                                                               | <sub>17981 </sub>              |
| <sub>start\_mux</sub>      | <sub>uint8\_t</sub>  | <sub>The MUX setting for the channel when the read began. Each channel contains one or more wells. For instance, a MinION flow cell has 4 wells per channel. The wells within a channel are connected to a multiplexer (MUX), a switch that controls which of the four wells in the channel is controlled and read out for sequencing.</sub>  | <sub>4 </sub>                  |
| <sub>start\_time</sub>     | <sub>uint64\_t</sub> | <sub>The start time of the read. The unit for start\_time is ‘number of signal samples’, so start\_time has to be divided by sampling rate (sampling\_rate) to get the start time in seconds (i.e. the time since the run was started) </sub>                                                                                                 | <sub>335845487 </sub>          |
  
  
  
  
___
  
Please cite the following in your publications when using *S/BLOW5* file format:

> Gamaarachchi, H., Samarakoon, H., Jenner, S.P. et al. Fast nanopore sequencing data analysis with SLOW5. Nat Biotechnol 40, 1026-1029 (2022). https://doi.org/10.1038/s41587-021-01147-4

```
@article{gamaarachchi2022fast,
  title={Fast nanopore sequencing data analysis with SLOW5},
  author={Gamaarachchi, Hasindu and Samarakoon, Hiruna and Jenner, Sasha P and Ferguson, James M and Amos, Timothy G and Hammond, Jillian M and Saadat, Hassaan and Smith, Martin A and Parameswaran, Sri and Deveson, Ira W},
  journal={Nature biotechnology},
  pages={1--4},
  year={2022},
  publisher={Nature Publishing Group}
}
```
