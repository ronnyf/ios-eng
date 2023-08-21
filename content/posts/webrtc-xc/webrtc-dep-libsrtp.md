---
title: "libSRTP"
date: 2023-08-20T18:00:00-07:00
draft: false
_build:
    list: false
---

[Back to Article]({{< ref "/posts/webrtc-xc/main">}})

## Introduction to libSRTP

This package provides an implementation of the Secure Real-time
Transport Protocol (SRTP), the Universal Security Transform (UST), and
a supporting cryptographic kernel. The SRTP API is documented in include/srtp.h,
and the library is in libsrtp2.a (after compilation).

## Swift Package Manager

A new Swift package was created to build static and dynamic libraries with Xcode / SwiftPM.

https://github.com/ronnyf/libsrtp/blob/release/webrtc/Package.swift
