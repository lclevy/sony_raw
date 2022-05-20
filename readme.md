# Description of Sony RAW file formats (SRF, SR2 and ARW)

Version 0.51 (20may2022)

(this is work in progress)

## Introduction

This document describes RAW file formats used by Sony cameras, with extensions SRF, SR2 and ARW, from 2004 to today.

All 3 formats are based on TIFF format, with Intel (little) endianess ('II' marker). SRF appeared in January 2004 (DSC-F828), SR2 in December 2005 (DSC-R1) and ARW in July 2006 with Alpha DSLR-A100.

## Evolution of Sony RAW formats

SRF likely stands for Sony Raw File, SR2 is version 2 of SRF, and ARW likely means Alpha RAW.

RAW versions and formats had additional features over time:

- SRF, SR2 and ARW 1.0 : 12 bits uncompressed 

- ARW 2.1 : 12 bits cRAW (11 bits + 7 bits). **Lossy**. 2008

- ARW 2.3 : 14 bits. Lossy ?  (2012)

- ARW 2.3.1 :  14 uncompressed (2015)

- ARW 4.0 : **lossless** compression (2021)
  
  

Here is a more detailed view of evolution:

| Model       | Release date | RAW format                                     |
| ----------- | ------------ | ---------------------------------------------- |
| DSC-F828    | jan2004      | **SRF**.<br>First CyberShot with RAW           |
| DSC-V3      | dec2004      | RAW?                                           |
| DSC-R1      | dec2005      | **SRF2**                                       |
| A100        | jul2006      | **ARW 1.0**, 12bits                            |
| A700        | dec2007      | ARW 2.0                                        |
| A900        | oct2008      | RAW, cRAW. ARW 2.1                             |
| NEX-3/NEX-5 | jun2010      | ARW 2.2                                        |
| SLT-A99     | dec2012      | ARW 2.3, 14 bits<br/>**first 14bits sensor**   |
| RX1         | feb2013      | ARW 2.3, 14 bits                               |
| A7          | jan2014      | ARW 2.3, 12 bits only?                         |
| A7R         | fev2014      | ARW 2.3.1                                      |
| A7R II      | jun2015      | ARW 2.3.1, 14 bits<br/>**uncompressed 14bits** |
| RX10 III    | jul2016      | ARW 2.3.2                                      |
| A9          | apr2017      | ARW 2.3.3                                      |
| A7R III     | nov2017      | ARW 2.3.3<br/>can create **ARQ**               |
| A7 III      | fev2018      | ARW 2.3.3                                      |
| A9 II       | aug2020      | ARW 2.3.5                                      |
| A1          | apr2021      | ARW 4.0. <br>First ILC with Lossless RAW       |

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

- SRF : 16bits aligned, 12 bits data, big endian. Some data must be decrypted first

- ARW1 : 12 bits (starting with A100)

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

- Sony : https://imagingedge.sony.net

- Phase One

- Camera Raw

- DNGlab : [dnglab/arw.rs at main · dnglab/dnglab · GitHub](https://github.com/dnglab/dnglab/blob/main/rawler/src/decoders/arw.rs)
  
  

## Firmware hacking

- 2022, https://github.com/ma1co/fwtool.py
- Nex-Hack wiki, 14may2016 : https://web.archive.org/web/20160514055435/http://www.nex-hack.info/wiki/development/general
- May2014 : http://www.personal-view.com/faqs/sony-hack/fwtool

## RAW samples

- http://www.rawsamples.ch/index.php/en/sony

- https://www.photographyblog.com/pages/reviews/reviews_sony_a900_3.php

- dpreview.com

- https://raw.pixls.us/
  
  
