---
title: "Data Representation"
date: 2018-12-03T11:30:00+08:00
categories: [Foundations of Computer Science]
tags: [2's complement, bcd, gray code, excess, text, audio, image, video]
toc: true
math: false
---

# Data Representation

## Units
- bit, byte, kilobytes, megabytes, gigabytes, terabytes, petabytes
- 1000 base, MB
- 1024 base, MiB
- b vs B

## Numerical

### Encoding
- Excess 3
- BCD
![](https://qph.fs.quoracdn.net/main-qimg-f6778dabbaff0f1dd82925cea591e3ac.webp)
- Exercise: base 10, 9's complement vs 10's complement

### Integers(fixed point)
- 2's complement: mainly used
- 1's complement: postive & negative 0
- Sign and magnitude
- Gray code
    - Benefit: filp 1 bit at a time, has a [good pattern](https://en.wikipedia.org/wiki/Gray_code)

### Floating point
- Sign and magnitude
- [Float](https://en.wikipedia.org/wiki/Single-precision_floating-point_format)(32bits, 1 + 8 + 23), excess-127
- Double(64bits, 1 + 11 + 48)
![](https://i.imgur.com/lGJG12B.png)

### Overflow & Underflow

### Conversion
- 10110.0011~(2)~
- 1101~(2)~
    - unsigned
    - signed
- float: 1 01111111 00000 00000 00000 00000 001 = -(1+2^-23^)

## Text

- [ASCII](https://zh.wikipedia.org/wiki/ASCII)
- [Unicode](https://stackoverflow.com/questions/5290182/how-many-bytes-does-one-unicode-character-take#answer-33349765)
    - [Implementation](https://stackoverflow.com/questions/21737906/how-to-read-write-utf8-text-files-in-c#answer-21745211)

## Audio

1. Sampling
    - Sample rate(e.g. 44100Hz)
    - Duration(e.g. 3s)
3. Quantization
    - Bit per sample(bit depth, e.g. 24 bit)
4. Encoding
    - Lossy: mp3, ape...
    - Lossless: wav, flac...

- Bit rate = sample rate * bit depth (e.g. 1033kbps)

## Images

- Raster(bitmap) graphics: bmp, jpeg, gif...
- Vector graphics: svg...
- Resolution
- Color depth
    - True color: 24 bits, 3 channels
    - Indexed color: a subset color of true color

## Videos

- Mpeg4, avi, mkv...

## Questions
1. Convert the 1010.01011~(2)~ to decimal and octal.
2. Convert the 734.2371~(8)~ to decimal and binary.
3. Convert the 31.96~(10)~ to octal and binary.
4. Represent 31.96~(10)~ with single precision floating point.
5. When will overflow and underflow happen?
6. Give 1 example of overflowing and 1 example of underflowing.
7. If a piece of audio has 500 bytes, duration of 5s, bit depth is 10bit. What's its bit rate and sampling rate?
8. How do resolution and color depth affect an image?
