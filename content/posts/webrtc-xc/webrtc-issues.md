---
title: "Build Issues"
date: 2023-07-23T21:53:44-07:00
draft: true
_build:
    list: false
---

[Back to Article]({{< ref "/posts/webrtc-xc/main">}})

## All Build Warnings

It is currently only possible to specify additional linker settings with `unsafeFlags` in `Package.swift`. The use of this flag has the following side effect:
> As the usage of the word “unsafe” implies, Swift Package Manager can't safely determine if the build flags have any negative side effect on the build since certain flags can change the behavior of how it performs a build. 
>
> As some build flags can be exploited for unsupported or malicious behavior, the use of unsafe flags makes the products containing this target ineligible for use by other packages.

This applies to all third party dependencies. While the build produces a multitude of warnings, most of which are 

    Implicit conversion loses integer precision: 'uint64_t' (aka 'unsigned long long') to 'int32_t' (aka 'int')

### WebRTC Build Warnings

`Variable '...' may be uninitialized when used here`
- async_tcp_socket.cc:151:7
- physical_socket_server.cc:1496:30
- vad_audio_proc.cc:223:64
- frame_buffer.cc:267:31
---
`'stream_size' is deprecated`
- rtc_event_log_parser.cc:2485:24
---
`Possible misuse of comma operator here`
- echo_canceller3.cc:785:72 
---
`OpenGLES API deprecated.`
- RTCDefaultShader
- RTCEAGLVideoView
- RTCI420TextureCache
- RTCNV12TextureCache
- RTCNV12TextureCache
- RTCOpenGLDefines
- RTCShader

## Static Analysis

### bugprone-move-forwarding-reference issues

    Forwarding reference passed to std::move(), which may unexpectedly cause lvalues to be moved; use std::forward() instead

- In file included from sdk/objc/api/peerconnection/RTCAudioTrack.mm:15:
- In file included from sdk/objc/api/peerconnection/RTCPeerConnectionFactory+Private.h:13:
- In file included from api/peer_connection_interface.h:129:
- In file included from p2p/base/port_allocator.h:22:
- In file included from p2p/base/port.h:28:
- In file included from api/transport/field_trial_based_config.h:16:
- In file included from api/field_trials_registry.h:18:
- In file included from rtc_base/containers/flat_set.h:19:

```
rtc_base/containers/flat_tree.h:84:9: warning: Forwarding reference passed to std::move(), which may unexpectedly cause lvalues to be moved; use std::forward() instead [bugprone-move-forwarding-reference]
  lhs = std::move(rhs);
        ^
1 warning generated.
```

For a total of **224** build warnings, the static analyzer produced an additional **603** results, which includes all third party dependencies. Some of those seem interesting enough to look into.