---
title: "Building WebRTC for iOS/macOS with Xcode"
date: 2023-07-23T21:53:44-07:00
draft: true
---
## Introduction

WebRTC (Web Real-Time Communication) is a powerful open-source technology that enables real-time communication capabilities in web and mobile applications. While WebRTC is well-supported in web browsers, incorporating it into iOS applications often requires (re)building a binary framework to ensure seamless integration and improved performance. In this article, we will provide a comprehensive step-by-step guide on how to integrate WebRTC into any iOS/macOS application and building it from source entirely in Xcode.

Wait! What? Why?

But one can build this easily as a framework with Google's depot tools, you might say. That is entirely correct. It might even be more convenient to run someone's clever build script, grab your favorite (non)cafeinated drink and voila. But what if maybe there's another way?

## Motivation

The main motiviation for this endeavor is to create the ability for developers to not only live-debug into the framework with familiar tools (Xcode, Instruments) but to also build a greater variety of targets. This then opens up a lot more opportunities to optimize and modernize some parts of WebRTC for iOS. With Swift-C++ interopability coming with the Swift 5.9 toolchain, some very interesting engineering might happen that could yield benefits not only for Apple platforms. Let's get started.

## Creating an Xcode project for WebRTC

A basic familiarity with Xcode and how to create projects, targets and their basic configuration is assumed. 

The final product is available in [this github respsitory](https://github.com/ronnyf/WebRTC.git)

### Obtaining the sources

Refer to [Google's instructions](https://webrtc.github.io/webrtc-org/native-code/ios/ "Google's instructions") for fetching the WebRTC sources. At the time of writing this, the full sync would take around **20 GB** of disk space. In contrast, the "Built-With-Xcode" variant occupies **788 MB**.

### Creating the targets

While Google's build configuration defines several dozens of individual targets that are very neatly organized into an efficient dependency graph, this is not something that is going to be replicated for the Xcode build. The build graph is something that Xcode manages internally. Rather than taking on those responsibilities manually, Xcode is entrusted with this. Please refer to the paragraph about [build performance]({{< relref "#build-performance" >}}) for a breakdown of build times of Xcode and Ninja.

The targets that are being created are the following:
- WebRTC (framework)
- libWebRTC (static library)
- CoreRTC (static library)

#### CoreRTC

The *CoreRTC* static library will contain all objects (minus the unit tests and mock objects) from the following folders:
- api
- audio
- call
- common_audio
- common_video
- logging
- media
- modules
- net
- p2p
- pc
- resources
- rtc_base
- rtc_tools
- stats
- system_wrappers
- test
- testing
- third_party*
- video

[All Source Files]({{< relref "webrtc-core-sources" >}})

Google engineers follow the pattern of storing header files near the location of the source files and refer to them by thir relative path to the source root. There should be a name for this 🤷‍♂️.

Setting the `User Header Search Paths` to `$(SRCROOT)` in Xcode's `Build Settings`.

```
USER_HEADER_SEARCH_PATHS = $(SRCROOT)
```

#### libWebRTC

The static `WebRTC` library links the `CoreRTC` static library and builds the following sources.

[All Source Files]({{< relref "webrtc-lib-sources" >}})

Additionally, the public headers need to be made available for other libraries to link this one. This is achieved with a `Copy Files Phase` where all relevent headers are copied to the `Products Directory` at subpath `include/WebRTC`.

[All Header Files]({{< relref "webrtc-lib-headers" >}})

THe following SDK classes / protocols / objects were moved into the `modules/audio_device/ios/components/audio` location because the `audio_device_ios.h` header is included in `modules/audio_device/audio_device_impl.cc`. The change was introduced with [this commit](https://webrtc.googlesource.com/src/+/8d95e3b2114e25977af93599a32b941d4e627364). While this probably made sense when using `Blaze/Bazel`. In this configuration here, the SDK libraries are linking the core library and this is a unidirectional path. In iOS SDK terms, this would be something like `QuartzCore` linking `UIKit`. 

So this approach has to be reversed but without making code changes. This can easily be achieved by re-exporting the symbols:

* RTCAudioDevice
* RTCAudioSession
* RTCAudioSessionDelegate
* RTCAudioSessionActivationDelegate
* RTCAudioSessionConfiguration

from `CoreRTC` in the `libWebRTC` and the `WebRTC (framework)` targets. One minor issue is that the implementation of those references SDK includes, such as `RTCMacros.h` and `RTCLogging.h`. 

A new header is created and added to the `rtc_base` directory: **rtc_export_bridge.h**. The content borrows from those defines that are declared in the SDK.

    #ifndef rtc_export_bridge_h
    #define rtc_export_bridge_h

    #include "rtc_base/system/rtc_export.h"

    #ifndef RTC_OBJC_TYPE
    #define RTC_OBJC_TYPE(t) t
    #endif

    #ifndef RTC_OBJC_EXPORT
    #define RTC_OBJC_EXPORT RTC_EXPORT
    #endif

    #ifndef RTC_EXTERN
    #if defined(__cplusplus)
    #define RTC_EXTERN extern "C" RTC_OBJC_EXPORT
    #else
    #define RTC_EXTERN extern RTC_OBJC_EXPORT
    #endif
    #endif

    #ifndef RTC_FWD_DECL_OBJC_CLASS
    #ifdef __OBJC__
    #define RTC_FWD_DECL_OBJC_CLASS(classname) @class classname
    #else
    #define RTC_FWD_DECL_OBJC_CLASS(classname) typedef struct objc_object classname
    #endif
    #endif

This allows the symbols that are already annotated with `RTC_OBJC_EXPORT` do be re-exported. Another small caveat... the `RTCAudioDevice.h` header must include the newly defined `rtc_export_bridge.h` header. This include needs to be hidden from the public interface however. For that purpose, the `reexported` folder was created, containing the original header files that are part of the public API definition. The actual headers are hidden from the public interfaces of `libWebRTC` and `WebRTC`.

#### WebRTC

The framework target builds a similar set of sources as the static library, the public and internal headers however are organized using Xcode's `Build Phases` configuration.

#### Defines

Looking at the `BUILD.gn` and `webrtc.gni` file(s), there are a lot of preprocessor macro definitions that seem extremely relevant. There are several options to provide those to the build system. A `config.h` file seems to be generally used for provisioning relevant macros.

[Default configuration values for iOS builds]({{< relref "webrtc_config" >}})

Adding a new header file to the `rtc_base` directory: `rtc_base/rtc_defines.h`.

[Content of *rtc_defines.h*]({{< relref "webrtc_defines" >}})

In order to take advantage of this configuration, the header needs to be included in a lot of source files. As a rule of thumb, if a `#define` is checked, the include statement `#include "rtc_base/rtc_defines.h"` should be added.

```
#ifndef COMMON_AUDIO_FIR_FILTER_AVX2_H_
#define COMMON_AUDIO_FIR_FILTER_AVX2_H_

#include "rtc_base/rtc_defines.h"

#if defined(WEBRTC_ARCH_X86_FAMILY) && defined(WEBRTC_HAS_AVX2)

#include <stddef.h>

#include <memory>

#include "common_audio/fir_filter.h"
#include "rtc_base/memory/aligned_malloc.h"

namespace webrtc {

class FIRFilterAVX2 : public FIRFilter {...}

}  // namespace webrtc

#endif // defined(WEBRTC_ARCH_X86_FAMILY) && defined(WEBRTC_HAS_AVX2)
#endif  // COMMON_AUDIO_FIR_FILTER_AVX2_H_
```

On a platform without `AVX2` support, this would be compiled into an (empty) `.o` file, which adds a small amount of overhead but should be stripped out of any optimized binary that is being distributed.

#### Generated Files

##### Protobufs

Use your favorite `protoc` build, or try out [this one](https://github.com/ronnyf/protobuf/tree/release/webrtc) that is part of WebRTC's dependency list. Executable `protoc` binaries can be built with SwiftPM for iOS/macOS on arm64/x86_64.

##### Registered Field Trials

The `registered_field_trials.h` file is generated by a script during Google's build process. Depending on the user's needs, the individual variants could be added manually to Xcode.

```
// This file was automatically generated. Do not edit.

#ifndef GEN_REGISTERED_FIELD_TRIALS_H_
#define GEN_REGISTERED_FIELD_TRIALS_H_

#include "absl/strings/string_view.h"

namespace webrtc {

inline constexpr absl::string_view kRegisteredFieldTrials[] = {
    "",
};

}  // namespace webrtc

#endif  // GEN_REGISTERED_FIELD_TRIALS_H_
```
For that purpose, a dedicated `generated` folder is created and relevant headers / sources are copied into it.

### Third Party Dependencies

The following repositories were forked from their origin and a `Package.swift` file was added to allow building static and dynamic libraries with Xcode / SwiftPM. The branch `release/webrtc` has been created where the package definiton is available and also potential (required/useful) changes were made.

- [Abseil-C++]({{< relref "webrtc-dep-absl" >}})

- [CRC32c]({{< relref "webrtc-dep-crc32c" >}})

- [BoringSSL]({{< relref "webrtc-dep-boringssl" >}})

- [JSONcpp]({{< relref "webrtc-dep-jsoncpp" >}})

- [libSRTP]({{< relref "webrtc-dep-libsrtp" >}})

- [libVPX]({{< relref "webrtc-dep-libvpx" >}})

- [libYuv]({{< relref "webrtc-dep-libyuv" >}})

- [OPUS]({{< relref "webrtc-dep-libopus" >}})

- [Protobuf]({{< relref "webrtc-dep-protobuf" >}})

- [RNNoise]({{< relref "webrtc-dep-rnnoise" >}})

- [utf8_range]({{< relref "webrtc-dep-utf8range" >}})

## Build issues

Not considering implicit `int32` to `int64` conversions, Xcode has revealed a small [list of warnings]({{< relref "webrtc-issues" >}})

## Build performance

All measurements were taken on a 2021 16" MacBook Pro M1-Max running macOS Ventura 13.4.1 (c), build: 22F770820d

The WebRTC framework build with `Ninja`:

```
time ninja -C out/ios_64 framework_objc
ninja: Entering directory `out/ios_64'
[3640/3640] STAMP obj/sdk/framework_objc.stamp
ninja -C out/ios_64 framework_objc  892.88s user 189.84s system 623% cpu 2:53.54 total
```

Three Xcode builds using the `Product > Perform Action > Build with Timing Summary` were done with the following results:

1: Build succeeded    7/26/23, 10:40 AM    `171.6 seconds`

2: Build succeeded    7/26/23, 10:43 AM    `192.5 seconds`

3: Build succeeded    7/26/23, 11:00 AM    `187.9 seconds`

## Conclusion

Judging from those preliminary (and unarguably unverified) results, the following allegations are made:

* The Xcode build of the WebRTC iOS framework (arm64 target) using the methods described in this article is performed in similar time compared to the `Ninja` build, [described by Google](https://webrtc.github.io/webrtc-org/native-code/ios/).
* The build graph created by `xcodebuild` is **NOT** less efficient than the (allegedly) manually created and highly optimized build graph produced by Google's `Bazel/Blaze` toolchain. 
* The complexity of managing all files in the build targets is significantly lower (lower is better) with Xcode.
* There are no performance concerns managing several thousand source and header files with Xcode, when run on an APFS file system on a current generation SSD drive. It is reasonable to assume that the same statement would not apply when the Xcode project file is located in a user space mounted network file system.
