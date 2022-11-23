# SLOW5 Specification


## Latest version

[0.2.0](slow5-v0.2.0.pdf)



## Archieved versions

[0.1.0](slow5-v0.1.0.pdf)


___

SLOW5 primary design goals are simplicity and usability; reading performance; and, file size. They are elaborated [here](design.md).

What we uncovered about the structure and fields of FAST5 format for the purpose of developing SLOW5 is available in the [fast5 demystied document](fast5_demystified.pdf).

___

A summary of the latest SLOW5 specification is given below for convienience. For full specifications, refer to the links above.

Primary data fields in SLOW5 format:

| Field name       | Data type  | Description                                                                                                                                                           | Example value                        |
| ---------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| read\_id         | char\*     | A unique identifier for the read. This is a Universally unique identifier (UUID) version 4, and should be unique for any read from any device.                        | 00592138-f120-4ab5-9916-c5567adb8e29 |
| read\_group      | uint32\_t  | Read group identifier. More information in the subsequent text.                                                                                                       | 0                                    |
| digitisation     | double     | The digitisation is the number of quantisation levels in the Analog to Digital Converter (ADC). That is, if the ADC is 12 bit, digitisation is 4096 (2<sup>12</sup>). | 8192                                 |
| offset           | double     | The ADC offset error. This value is added when converting the signal to pico ampere.                                                                                  | 10                                   |
| range            | double     | The full scale measurement range in pico amperes.                                                                                                                     | 1441.389893                          |
| sampling\_rate   | double     | Sampling frequency of the ADC, i.e., the number of data points collected per second.                                                                                  | 4000                                 |
| len\_raw\_signal | uint64\_t  | The number of samples in the raw signal (length of the raw\_signal vector below).                                                                                     | 59676                                |
| raw\_signal      | int16\_t\* | The raw signal which are the direct acquisition values from the ADC and are comma separated.                                                                          | 1039,588,588,593,586….               |

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
