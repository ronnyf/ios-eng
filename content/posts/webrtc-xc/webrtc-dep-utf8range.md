---
title: "UTF-8 Range"
date: 2023-07-23T21:53:44-07:00
draft: true
_build:
    list: false
---

[Back to Article]({{< ref "/posts/webrtc-xc/main">}})

## Fast UTF-8 validation with Range algorithm (NEON+SSE4+AVX2)

This is a brand new algorithm to leverage SIMD for fast UTF-8 string validation. Both **NEON**(armv8a) and **SSE4** versions are implemented. **AVX2** implementation contributed by [ioioioio](https://github.com/ioioioio).

## Swift Package Manager

A new Swift package was created to build static and dynamic libraries with Xcode / SwiftPM.

https://github.com/ronnyf/utf8_range/blob/release/webrtc/Package.swift
