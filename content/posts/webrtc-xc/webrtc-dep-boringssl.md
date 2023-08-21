---
title: "BoringSSL"
date: 2023-07-23T21:53:44-07:00
draft: true
_build:
    list: false
---

[Back to Article]({{< ref "/posts/webrtc-xc/main">}})

## BoringSSL

BoringSSL is a fork of OpenSSL that is designed to meet Google's needs.

Although BoringSSL is an open source project, it is not intended for general
use, as OpenSSL is. We don't recommend that third parties depend upon it. Doing
so is likely to be frustrating because there are no guarantees of API or ABI
stability.

## Swift Package Manager

A new Swift package was created to build static and dynamic libraries with Xcode / SwiftPM.

https://github.com/ronnyf/boringssl/blob/release/webrtc/Package.swift
