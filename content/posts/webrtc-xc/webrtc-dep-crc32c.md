---
title: "CRC32c"
date: 2023-07-23T21:53:44-07:00
draft: true
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
