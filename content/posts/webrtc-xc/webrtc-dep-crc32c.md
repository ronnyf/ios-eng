---
title: "CRC32c"
date: 2023-08-20T18:00:00-07:00
draft: false
_build:
    list: false
---

[Back to Article]({{< ref "/posts/webrtc-xc/main">}})

## CRC32c

This project collects a few CRC32C implementations under an umbrella that
dispatches to a suitable implementation based on the host computer's hardware
capabilities.

## Swift Package Manager

A new Swift package was created to build static and dynamic libraries with Xcode / SwiftPM.

https://github.com/ronnyf/crc32c/blob/release/webrtc/Package.swift
