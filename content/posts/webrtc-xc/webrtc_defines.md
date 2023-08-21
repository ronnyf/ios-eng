---
title: "WebRTC defines"
date: 2023-07-23T21:53:44-07:00
draft: true
_build:
    list: false
---
[Back to Article]({{< ref "/posts/webrtc-xc/main">}})

## Creating a config header for all iOS/macOS builds

1: Make sure that we have Apple's `TargetConditionals` header.

```
#ifdef __APPLE__
#include <TargetConditionals.h>
#else
#error The purpose of this is to build WebRTC with Xcode
#endif // __APPLE__
```

### Platform defines

Defining `WEBRTC_POSIX` and `WEBRTC_MAC` and `WEBRTC_IOS`. Since Xcode is the build tool of choice for this, `WEBRTC_POSIX` and `WEBRTC_MAC` are always defined by default.

```
#ifndef WEBRTC_POSIX
#define WEBRTC_POSIX
#endif
```

```
#ifndef WEBRTC_MAC
#define WEBRTC_MAC
#endif
```
```
#if TARGET_OS_IOS || TARGET_OS_SIMULATOR
#define WEBRTC_IOS
#else
#undef WEBRTC_IOS
#endif
```

### CPU architecture based definitions 

#### ARM / ARM64
```
#if TARGET_CPU_ARM64 && !defined(WEBRTC_ARCH_ARM64)
#define WEBRTC_ARCH_ARM64
#endif
```
```
#if TARGET_CPU_ARM && !defined(WEBRTC_ARCH_ARM)
#define WEBRTC_ARCH_ARM
#endif
```
```
#if __ARM_ARCH == 7 && !defined(WEBRTC_ARCH_ARM_V7)
#define WEBRTC_ARCH_ARM_V7
#endif
```
```
#if __ARM_ARCH_7S__ && !defined(WEBRTC_ARCH_ARM_V7S)
#define WEBRTC_ARCH_ARM_V7S
#endif
```
```
#if __ARM_NEON && !defined(WEBRTC_HAS_NEON)
#define WEBRTC_HAS_NEON
#endif
```

#### X86 / X86_64

```
#if TARGET_CPU_X86 && !defined(WEBRTC_ARCH_X86_FAMILY)
#define WEBRTC_ARCH_X86_FAMILY
#endif
```
```
#if TARGET_CPU_X86_64 && !defined(WEBRTC_ARCH_X86_FAMILY)
#define WEBRTC_ARCH_X86_FAMILY
#endif
```
```
#if TARGET_CPU_X86_64 && !defined(WEBRTC_ARCH_X86_64)
#define WEBRTC_ARCH_X86_64
#endif
```

#### Additional Vector Extensions for X86

```
#if __MMX__
#define WEBRTC_HAS_MMX
#endif
```
```
#if __SSE2__
#define WEBRTC_HAS_SSE2
#endif
```
```
#if __SSE3__
#define WEBRTC_HAS_SSE3
#endif
```
```
#if __SSE4_1__
#define WEBRTC_HAS_SSE4_1
#endif
```
```
#if __SSSE3__
#define WEBRTC_HAS_SSSE3
#endif
```
```
#if __AVX2__
#define WEBRTC_HAS_AVX2
#define WEBRTC_ENABLE_AVX2
#endif
```

##### AVX and Xcode

The following Xcode setting allows enabling of vector extensions:

```
CLANG_X86_VECTOR_INSTRUCTIONS = $(DEFAULT_SSE_LEVEL_4_2_$(GCC_ENABLE_SSE42_EXTENSIONS))
```

By default, Xcode enables `SSE4.2` extensions, which include `MMX`, `SSE2`, `SSE3` but **NOT** `AVX2`nor `FMA`. Those are enabled via the `-m` compiler flag `(-mavx2 -mfma)` in the Google build.

Run the following command in your favorite terminal app on `macOS` with Xcode + Command Line Tools installed to get a list of supported compiler flags.

For example, on a M1 MacBook Pro, with Xcode 14.3 installed, Apple's clang compiler indeed supports all those `SSE` instructions ... and also something called `SSSE3`. 

```
g++ -arch x86_64 -dM -E - </dev/null | egrep -i '(sse|avx|fma|x86)'

#define __SSE2_MATH__ 1
#define __SSE2__ 1
#define __SSE3__ 1
#define __SSE4_1__ 1
#define __SSE_MATH__ 1
#define __SSE__ 1
#define __SSSE3__ 1
#define __x86_64 1
#define __x86_64__ 1
```

Judging by the output, `AVX2` support is not enabled by default. The compiler definitely has the ability to create `AVX2` instructions when those are enabled explicitly.

```
g++ -arch x86_64 -mavx2 -mfma -dM -E - </dev/null | egrep -i '(sse|avx|fma|x86)'

#define __AVX2__ 1
#define __AVX__ 1
#define __FMA__ 1
#define __SSE2_MATH__ 1
#define __SSE2__ 1
#define __SSE3__ 1
#define __SSE4_1__ 1
#define __SSE4_2__ 1
#define __SSE_MATH__ 1
#define __SSE__ 1
#define __SSSE3__ 1
#define __x86_64 1
#define __x86_64__ 1
```

When building for `arm64`, some other features are of interest.

```
g++ -arch arm64 -dM -E - </dev/null | egrep -i '(arm|crc)'

#define __ARM64_ARCH_8__ 1
#define __ARM_64BIT_STATE 1
#define __ARM_ACLE 200
#define __ARM_ALIGN_MAX_STACK_PWR 4
#define __ARM_ARCH 8
#define __ARM_ARCH_8_3__ 1
#define __ARM_ARCH_8_4__ 1
#define __ARM_ARCH_8_5__ 1
#define __ARM_ARCH_ISA_A64 1
#define __ARM_ARCH_PROFILE 'A'
#define __ARM_FEATURE_AES 1
#define __ARM_FEATURE_ATOMICS 1
#define __ARM_FEATURE_CLZ 1
#define __ARM_FEATURE_COMPLEX 1
#define __ARM_FEATURE_CRC32 1
#define __ARM_FEATURE_CRYPTO 1
#define __ARM_FEATURE_DIRECTED_ROUNDING 1
#define __ARM_FEATURE_DIV 1
#define __ARM_FEATURE_DOTPROD 1
#define __ARM_FEATURE_FMA 1
#define __ARM_FEATURE_FP16_FML 1
#define __ARM_FEATURE_FP16_SCALAR_ARITHMETIC 1
#define __ARM_FEATURE_FP16_VECTOR_ARITHMETIC 1
#define __ARM_FEATURE_FRINT 1
#define __ARM_FEATURE_IDIV 1
#define __ARM_FEATURE_JCVT 1
#define __ARM_FEATURE_LDREX 0xF
#define __ARM_FEATURE_NUMERIC_MAXMIN 1
#define __ARM_FEATURE_QRDMX 1
#define __ARM_FEATURE_SHA2 1
#define __ARM_FEATURE_SHA3 1
#define __ARM_FEATURE_SHA512 1
#define __ARM_FEATURE_SM3 1
#define __ARM_FEATURE_SM4 1
#define __ARM_FEATURE_UNALIGNED 1
#define __ARM_FP 0xE
#define __ARM_FP16_ARGS 1
#define __ARM_FP16_FORMAT_IEEE 1
#define __ARM_NEON 1
#define __ARM_NEON_FP 0xE
#define __ARM_NEON__ 1
#define __ARM_PCS_AAPCS64 1
#define __ARM_SIZEOF_MINIMAL_ENUM 4
#define __ARM_SIZEOF_WCHAR_T 4
#define __arm64 1
#define __arm64__ 1
```

### WebRTC Feature Defines

The following defines seem to be reasonable choice and result in a functional library.

```
#define HAVE_WEBRTC_VIDEO
#define OPENSSL_IS_BORINGSSL
#define WEBRTC_APM_DEBUG_DUMP 1
#define WEBRTC_AUDIOPROC_FIXED_PROFILE
#define WEBRTC_ENABLE_OBJC_SYMBOL_EXPORT
#define WEBRTC_ENABLE_PROTOBUF 1
#define WEBRTC_ENABLE_RTC_EVENT_LOG
#define WEBRTC_ENABLE_SYMBOL_EXPORT
#define WEBRTC_HAVE_DCSCTP
#define WEBRTC_HAVE_SCTP
#define WEBRTC_INCLUDE_INTERNAL_AUDIO_DEVICE
#define WEBRTC_LIBRARY_IMPL
#define WEBRTC_STRICT_FIELD_TRIALS 0
#define WEBRTC_USE_BUILTIN_ILBC 1
#define WEBRTC_USE_BUILTIN_OPUS 1
```
```
#ifdef RTC_USE_LIBAOM_AV1_ENCODER
#undef RTC_USE_LIBAOM_AV1_ENCODER
#endif
```
```
#ifdef WEBRTC_USE_H264
#error Let's use the built-in HW encoder/decoder.
#endif
```

It should be noted that WebRTC is defaulting to its `DummyVideoDevice` without setting:

```
#define HAVE_WEBRTC_VIDEO
```

#### Disabled Features

```
//#define RTC_DAV1D_IN_INTERNAL_DECODER_FACTORY
//#define RTC_ENABLE_VP8
//#define RTC_ENABLE_VP9
```
```
//#define AECM_WITH_ABS_APPROX
//#define WEBRTC_DUMMY_FILE_DEVICES
//#define WEBRTC_EXCLUDE_AUDIO_PROCESSING_MODULE
//#define WEBRTC_EXCLUDE_TRANSIENT_SUPPRESSOR
//#define WEBRTC_IOS_ENABLE_COVERAGE
//#define WEBRTC_UNIT_TEST
//#define WEBRTC_USE_CRYPTO_BUFFER_CALLBACK
//#define WEBRTC_USE_GIO
```
