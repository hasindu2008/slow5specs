# Summary of SLOW5 format 

This is just a summary of the data fields in latest version of SLOW5 ASCII file format (.slow5). For the full specification, refer to the PDF links [here](https://github.com/hasindu2008/slow5specs/blob/main/docs/index.md#latest-version).


## Primary fields

 These fields are mandatory and must be arranged in the order that they appear below:

|Col | Field name       | Data type  | Description                                                                                                                                                           | Example value                        |
|-- | ---------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
|1| read\_id         | char\*     | <sub>A unique identifier for the read.</sub>  | <sub>00592138-f120-4ab5-9916-c5567adb8e29</sub>
|2| read\_group      | uint32\_t  | <sub>Read group identifier.</sub>.                                                                                                       | <sub>0                                    |
|3| digitisation     | double     | <sub>Number of quantisation levels in the Analog to Digital Converter (ADC). That is, if the ADC is 12 bit, digitisation is 4096 (2<sup>12</sup>).</sub> | <sub>8192</sub>                                 |
|4| offset           | double     | <sub>The ADC offset error. This value is added when converting the signal to pico ampere.</sub>                                                                                  | <sub>10</sub>                                   |
|5| range            | double     | <sub>The full scale measurement range in pico amperes.</sub>                                                                                                                     | <sub>1441.389893</sub>                          |
|6| sampling\_rate   | double     | <sub>Sampling frequency of the ADC, i.e., the number of data points collected per second. </sub>                                                                                 | <sub>4000</sub>                                 |
|7| len\_raw\_signal | uint64\_t  | <sub>The number of samples in the raw signal (length of the raw\_signal vector below). </sub>                                                                                    | <sub>59676</sub>                                |
|8| raw\_signal      | int16\_t\* | <sub>The raw signal which are the direct acquisition values from the ADC and are comma separated.   </sub>                                                                       | <sub>1039,588,588,593,586….</sub>               |

Primary fields contain all the information required for a typical nanopore signal-level analysis. The raw signal can be converted to pico-ampere using the following equation:

  ```
signal_in_pico_ampere = (raw_signal + offset) * range / digitisation
```

## Auxiliary fields

These fields are optional and not bound by any strict order. Following are some common auxiliary data fields in SLOW5 format:
  
 | Field name      | Data type | Description                                                                                                                                                                                                                                                                                                                       | Example value      |
| --------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| channel\_number | char\*    | <sub>The channel number. A flow cell has multiple channels allowing multiple DNA/RNA strands to be sequenced in parallel. For instance, a MinION flow cell has 512 channels and thus can sequence 512 strands in parallel. </sub>                                                                                                             | <sub>504 </sub>                |
| median\_before  | double    | <sub>The estimated median current level immediately preceding the read. In most cases this can be used as an estimate of the open pore level. The open-pore state is when there is no strand inside the pore.</sub>                                                                                                                           | <sub>238.78225708007812 |
| read\_number    | int32\_t  | <sub>A unique number within each channel counted upwards from zero. Note that not all reads generated are “strand” reads, but only strand reads are written to the final fast5 file, so some read numbers may be absent.                                                                                                               | <sub>17981 </sub>              |
| start\_mux      | uint8\_t  | <sub>The MUX setting for the channel when the read began. Each channel contains one or more wells. For instance, a MinION flow cell has 4 wells per channel. The wells within a channel are connected to a multiplexer (MUX), a switch that controls which of the four wells in the channel is controlled and read out for sequencing.</sub>  | <sub>4 </sub>                  |
| start\_time     | uint64\_t | <sub>The start time of the read. The unit for start\_time is ‘number of signal samples’, so start\_time has to be divided by sampling rate (sampling\_rate) to get the start time in seconds (i.e. the time since the run was started) </sub>                                                                                                 | <sub>335845487 </sub>          |
  
  
  
  
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
