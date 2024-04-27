# Description of Sony RAW file formats (SRF, SR2, ARW and ARQ)

Version 0.52 (30may2022)

(this is work in progress)

## Introduction

This document describes RAW file formats used by Sony cameras, with extensions SRF, SR2, ARW and ARQ, from 2004 to today.

All formats are based on TIFF format, with Intel (little) endianess ('II' marker). SRF appeared in January 2004 (DSC-F828), SR2 in December 2005 (DSC-R1) and ARW in July 2006 with Alpha DSLR-A100. 

ARQ, pixel shifting, appeared in November 2017 (A7R III).

## Evolution of Sony RAW formats

SRF likely stands for Sony Raw File, SR2 is version 2 of SRF, and ARW means Alpha RAW.

RAW versions and formats had additional features over time:

- SRF, SR2 and ARW 1.0 : 12 bits uncompressed 

- ARW 2.1 : 12 bits cRAW (11 bits + 7 bits). **Lossy**. 2008

- ARW 2.3 : 14 bits.  (2012)

- ARW 4.0 : **lossless** compression (2021). A1 also introduces YUV420 burst format, which is **lossy**.





Here is a more detailed view of evolution, and links to implementations:

| Model       | Release date | RAW format                                     | dcraw / libraw support                                                                                                                                                                                                                                            |
| ----------- | ------------ | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DSC-F828    | jan2004      | **SRF**.<br>First CyberShot with RAW           | [parseSonySRF()](https://github.com/LibRaw/LibRaw/blob/2a9a4de21ea7f5d15314da8ee5f27feebf239655/src/metadata/sony.cpp#L2173)                                                                                                                                      |
| DSC-V3      | dec2004      | **SRF**                                        | [sony_load_raw()](https://github.com/LibRaw/LibRaw/blob/01a2b7f3545705f38cfd4e9a3eee152ea8d1f967/src/decoders/decoders_dcraw.cpp#L1395)                                                                                                                           |
| DSC-R1      | dec2005      | **SRF2**                                       | [1.283, 7sep2005](https://github.com/ncruces/dcraw/commit/d476e291f39da55c0beb03061a56b13cb8021bbd)<br/>[parseSonySR2()](https://github.com/LibRaw/LibRaw/blob/2a9a4de21ea7f5d15314da8ee5f27feebf239655/src/metadata/sony.cpp#L2032)                              |
| A100        | jul2006      | **ARW 1.0**, 12bits                            | [1.331, 21jun2006](https://github.com/ncruces/dcraw/commit/6bdeee0d353335e483d8c5f7108d19131d49188f)<br/>[sony_arw_load_raw()](https://github.com/LibRaw/LibRaw/blob/01a2b7f3545705f38cfd4e9a3eee152ea8d1f967/src/decoders/decoders_dcraw.cpp#L1427)              |
| A700        | dec2007      | ARW 2.0                                        | [1.393, 8.78, 30oct2007, arw2](https://github.com/ncruces/dcraw/commit/0d7914e221897bc0674e863ad320107033a3fa3c)<br/>[sony_arw2_load_raw()](https://github.com/LibRaw/LibRaw/blob/01a2b7f3545705f38cfd4e9a3eee152ea8d1f967/src/decoders/decoders_dcraw.cpp#L1455) |
| A900        | oct2008      | RAW, cRAW. ARW 2.1                             | [1.405, 8.88, 15sep2008](https://github.com/ncruces/dcraw/commit/9418537b93a415972022372ad69d764c9fcaf044)                                                                                                                                                        |
| NEX-3/NEX-5 | jun2010      | ARW 2.2                                        | [1.434, 9.01, 30may2010](https://github.com/ncruces/dcraw/commit/db71a929e08ac1005b92ded3c7ac77b411df9419)                                                                                                                                                        |
| RX100       | jun2012      | ARW 2.3, 12 bits                               | [1.451, 9.16, 5jul2012](https://github.com/ncruces/dcraw/commit/a3f1bdd18523ee19bff610d21de968cb1051a791)                                                                                                                                                         |
| SLT-A99     | dec2012      | ARW 2.3, 14 bits<br/>**first 14bits sensor**   | [1.453, 9.17, 23dec2012](https://github.com/ncruces/dcraw/commit/65e6c8605de44cdb271ed0e196181f0c4eb34229)                                                                                                                                                        |
| RX1         | feb2013      | ARW 2.3, 14 bits                               | [1.453, 9.17, 23dec2012](https://github.com/ncruces/dcraw/commit/65e6c8605de44cdb271ed0e196181f0c4eb34229)                                                                                                                                                        |
| A7          | jan2014      | ARW 2.3, 12 bits only?                         |                                                                                                                                                                                                                                                                   |
| A7R         | fev2014      | ARW 2.3.1                                      |                                                                                                                                                                                                                                                                   |
| A7R II      | jun2015      | ARW 2.3.1, 14 bits<br/>**uncompressed 14bits** | uncompressed: [unpacked_load_raw()](https://github.com/LibRaw/LibRaw/blob/1dbed6b7e65ef2ebd0c3f82722a0b7f3990845f7/src/decoders/generic.cpp#L21)                                                                                                                  |
| RX10 III    | jul2016      | ARW 2.3.2                                      |                                                                                                                                                                                                                                                                   |
| A9          | apr2017      | ARW 2.3.3                                      |                                                                                                                                                                                                                                                                   |
| A7R III     | nov2017      | ARW 2.3.3<br/>can create **ARQ**               | [sony_arq_load_raw](https://github.com/LibRaw/LibRaw/blob/2a9a4de21ea7f5d15314da8ee5f27feebf239655/src/decoders/decoders_libraw.cpp#L17)                                                                                                                          |
| A7 III      | fev2018      | ARW 2.3.3                                      |                                                                                                                                                                                                                                                                   |
| A9 II       | aug2020      | ARW 2.3.5                                      |                                                                                                                                                                                                                                                                   |
| A1          | apr2021      | ARW 4.0. <br>First ILC with Lossless RAW<br>       | 30fps RAW seems YUV420 : https://github.com/dnglab/dnglab/pull/269/commits (jan2023)                                                                                                                                                                                                                                                                 |
| A7 IV       | fev2022      | ARW 4.0                                        |                                                                                                                                                                                                                                                                   |

You can extract format information with Exiftool with **-FileFormat** :

```
> exiftool -FileFormat -SonyModelID -RAWFileType RAW_SONY_A100.ARW
File Format : ARW 1.0
Sony Model ID : DSLR-A100

> exiftool -FileFormat -SonyModelID -RAWFileType A1_DSC02244.ARW
File Format : ARW 4.0
Sony Model ID : ILCE-1
RAW File Type : Uncompressed RAW
```

## Compression

Based on dcraw and dnglab code source, there are 4 different kind of compression:

- SRF : 16bits aligned, 12 bits data, big endian. Some data must be decrypted first. See *sony_load_raw()* in libraw

- ARW1 : 12 bits (starting with A100). See *sony_arw_load_raw()*

- ARW2 (first with A700, A850 and A900)
  
  - either uncompressed 12bits. Uncompressed 14 bits since A7R II
  
  - or cRAW for "compressed Raw". This is lossy : "11 bit 'base' values, 7-bit 'deltas', 11 to 14 bit tone curve" say Rawdigger

- LossLess Jpeg92 (starting with Alpha 1), in 2021
  
  

"Compressed" is used by Sony, where this is "lossy". 

"Uncompressed" is used when it is "lossless". 

But there is lossless compression ! They waited 2021 to adopt lossless jpeg when Canon use it since 2004 !

## TIFF Structure

Below are examples of TIFF structure for SRF, SR2 and some ARW versions.

### SRF (jan 2004)

- IFD0 (18 entries)
  - ImageWidth = 3360
  - ImageHeight = 2460
  - EXIF Subdir (20 entries)
  - SRF MakerNote Subdir
    - SRF0 (1 entry)
    - SRF1 (encrypted, 2 entries)
    - SRF2 (encrypted, 130 entries)
    - SRF3 (130 entries)
    - SRF4 (130 entries)
    - SRF5 (130 entries)
    - SRF6 (9 entries)
- IFD1 (6 entries)

from http://rawsamples.ch/raws/sony/RAW_SONY_DSC-F828.SRF (Exiftool output: [RAW_SONY_DSC-F828.SRF.txt](RAW_SONY_DSC-F828.SRF.txt))

### SR2 (dec 2005)

- IFD0 (15 entries)
  
  - SubfileType = 1
  
  - SubIFD (19 entries)
    
    - SubfileType = 0
    - ImageWidth = 3984
    - ImageHeight = 2608
    - BitsPerSample = 14
  
  - EXIF Subdir (30 entries)
    
    - MakerNote Subdir
  
  - SR2Private Subdir ( 6 entries)
    
    - SR2SubIFD (encrypted, 101 entries)
      - SR2SubIFD (132 entries)
      - SR2DataIFD1 (132 entries)
      - SR2DataIFD2 (132 entries)

- IFD1  (11 entries)

See http://www.rawsamples.ch/raws/sony/r1/RAW_SONY_R1.SR2 (Exiftool : [RAW_SONY_R1.txt](RAW_SONY_R1.txt))

### ARW

##### version 1.0

- IFD0 (19 entries)
  - EXIFIFD (36 entries)
    - MakerNoteSony (28 entries)
      - MakerNoteMinolta
  - SR2Private
    - MRW PRD segment
    - MRW WBG segment
    - MRW RIF segment
- IFD1 (11 entries)

See http://www.rawsamples.ch/raws/sony/a100/RAW_SONY_A100.ARW (Exiftool: [RAW_SONY_A100.txt](RAW_SONY_A100.txt))

#### version 4.0

Dated July 2021

- IFD0 (18 entries)
  - SubfileType = 1
  - SubIFD (35 entries)
    - SubfileType = 0
    - ImageWidth = 6048
    - ImageHeight = 4024
    - BitsPerSample = 14
  - XMP directory
  - ExifIFD directory (41 entries)
    - MakerNoteSony5 (99 entries)
      - Deciphered Tag9400c directory
      - Deciphered Tag9401 directory
  - PrintIM (SubDirectory)
  - SR2Private (9 entries)
    - SR2SubIFD (209 entries)
      - SR2DataIFD SubDirectory (124 entries)
      - SR2DataIFD1 directory with 124 entries
      - SR2DataIFD2 directory with 124 entries
      - SR2DataIFD3 directory with 124 entries
      - SR2DataIFD4 directory with 124 entries
      - SR2DataIFD5 directory with 124 entries
      - SR2DataIFD6 directory with 124 entries
      - SR2DataIFD7 directory with 124 entries
      - SR2DataIFD8 directory with 124 entries
      - SR2DataIFD9 directory with 124 entries
      - SR2DataIFD10 directory with 124 entries
      - SR2DataIFD11 directory with 124 entries
      - SR2DataIFD12 directory with 124 entries
- IFD1 directory with 14 entries
  - SubfileType = 1

Exiftool : [SONY_A9_DSC04458.txt](SONY_A9_DSC04458.txt)

## ARQ

ARQ likely means Alpha Raw Quad shots, which uses pixel shifting to capture high resolution pictures by combining 4 ARW pictures. This features is available with A7R III and requires Sony software Image Edge. See also [Pixel Shift Multi Shooting | SONY](https://support.d-imaging.sony.co.jp/support/ilc/psms/ilce7rm4/en/index.html). 

Supported by RawDigger 1.2.24 and Lightroom Classic CC 7.3.

```
>exiftool -FileFormat -RAWFileType -BitsPerSample -PixelShiftInfo "Sony-ILCE-7RM3.tif"
File Format                     : ARW 2.3.5
RAW File Type                   : Uncompressed RAW
Bits Per Sample                 : 14
Pixel Shift Info                : Group 12094102, Composed 4-shot (0x3c3)
```

## 

## References

- [RawDigger: detecting posterization in SONY cRAW/ARW2 files | RawDigger](https://www.rawdigger.com/howtouse/sony-craw-arw2-posterization-detection), Iliah Borg, June 2014

- Sony ARW2 Compression: Artifacts And Credible Repair, Henry Gordon Dietz, Kentucky Univ, 2016, https://library.imaging.org/admin/apis/public/api/sandbox/website/downloadArticle/ei/28/2/art00020

- Sony, [What shooting settings affect RAW image bit sizes (14 bits to 12bits)?](https://www.sony.co.uk/electronics/support/articles/00229990)

- Rawdigger, 23jan2018, [SonyPixelShift2DNG (Beta 0.8): Convert Sony A7R-III Pixel Shift Mode Shots to DNG | FastRawViewer](https://www.fastrawviewer.com/blog/SonyPixelShift2DNG-converter-Beta)



## Tools support

- ExifTool, Phil Harvey, https://exiftool.org/TagNames/Sony.html

- Libraw : [LibRaw/decoders_dcraw.cpp at master · LibRaw/LibRaw · GitHub](https://github.com/LibRaw/LibRaw/blob/master/src/decoders/decoders_dcraw.cpp)

- DNGlab : [dnglab/arw.rs at main · dnglab/dnglab · GitHub](https://github.com/dnglab/dnglab/blob/main/rawler/src/decoders/arw.rs)

- Sony : https://imagingedge.sony.net

- Phase One

- Camera Raw



## Firmware hacking

- 2022, https://github.com/ma1co/fwtool.py
- Nex-Hack wiki, 14may2016 : https://web.archive.org/web/20160514055435/http://www.nex-hack.info/wiki/development/general
- May2014 : http://www.personal-view.com/faqs/sony-hack/fwtool



## RAW samples

- http://www.rawsamples.ch/index.php/en/sony

- https://www.photographyblog.com/pages/reviews/reviews_sony_a900_3.php

- dpreview.com

- https://raw.pixls.us/
