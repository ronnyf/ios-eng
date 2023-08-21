---
title: "WebRTC configuration"
date: 2023-07-23T21:53:44-07:00
draft: true
_build:
    list: false
---
[Back to Article]({{< ref "/posts/webrtc-xc/main">}})

### RTC configuration values set via `webrtc.gni`

The build system uses those configuration values to determine which `#define` to declare.

#### Configuration Values

`rtc_enable_symbol_export` = `true`

Setting this to true will make RTC_EXPORT (see rtc_base/system/rtc_export.h) expand to code that will manage symbols visibility.

```
#define WEBRTC_ENABLE_SYMBOL_EXPORT
```

---

`rtc_enable_objc_symbol_export` = `true`

Setting this to true will make RTC_OBJC_EXPORT expand to code that will manage symbols visibility. By default, Obj-C/Obj-C++ symbols are exported if C++ symbols are but setting this arg to true while keeping `rtc_enable_symbol_export`= `false` will only export `RTC_OBJC_EXPORT` annotated symbols.

```
#define WEBRTC_ENABLE_OBJC_SYMBOL_EXPORT
```

---

`rtc_dlog_always_on` = `false`

Setting this to true, will make RTC_DLOG() expand to log statements instead of being removed by the preprocessor.
This is useful for example to be able to get *RTC_DLOGs* on a release build.

```
 #define DLOG_ALWAYS_ON
 ```

---

`rtc_enable_google_benchmarks` = `false`

Enables additional build targets that rely on `//third_party/google_benchmarks`.

---

`rtc_exclude_field_trial_default` = `false`

Setting this to true will define `WEBRTC_EXCLUDE_FIELD_TRIAL_DEFAULT` which will tell the pre-processor to remove the default definition of symbols needed to use field_trial. In that case a new implementation needs to be provided.

When WebRTC is built as part of Chromium it should exclude the default implementation of field_trial unless it is building for NACL or Chromecast.

---

`rtc_exclude_metrics_default` = `false`

Setting this to true will define `WEBRTC_EXCLUDE_METRICS_DEFAULT` which will tell the pre-processor to remove the default definition of symbols needed to use metrics. In that case a new implementation needs to be provided.

---

`rtc_exclude_system_time` = `false`

Setting this to true will define `WEBRTC_EXCLUDE_SYSTEM_TIME` which will tell the pre-processor to remove the default definition of the SystemTimeNanos() which is defined in `rtc_base/system_time.cc`. In that case a new implementation needs to be provided.

---

`rtc_builtin_ssl_root_certificates` = `true`

Setting this to `false` will require the API user to pass in their own `SSLCertificateVerifier` to verify the certificates presented from a TLS-TURN server. In return disabling this **saves around 100kb** in the binary.

```
#define WEBRTC_EXCLUDE_BUILT_IN_SSL_ROOT_CERTS
```

---

`rtc_include_ilbc` = `true`

Include the iLBC audio codec?

---

`rtc_include_opus` = `true`

Disable this to avoid building the Opus audio codec.

---

`rtc_opus_support_120ms_ptime` = `true`

Enable this if the Opus version upon which WebRTC is built supports direct encoding of 120 ms packets.

---

`rtc_opus_variable_complexity` = `false`

Enable this to let the Opus audio codec change complexity on the fly.

---

`rtc_enable_external_auth` = `false`

Enable when an external authentication mechanism is used for performing packet authentication for RTP packets instead of libsrtp.

```
#ifdef ENABLE_EXTERNAL_AUTH
#undef ENABLE_EXTERNAL_AUTH
#endif
```

---

`apm_debug_dump` = `false`

Selects whether debug dumps for the audio processing module should be generated.

---

`rtc_exclude_audio_processing_module` = `false`

Selects whether the audio processing module should be excluded.

```
#ifdef WEBRTC_EXCLUDE_AUDIO_PROCESSING_MODULE
#undef WEBRTC_EXCLUDE_AUDIO_PROCESSING_MODULE
#endif
```

---

`rtc_enable_bwe_test_logging` = `false`

Set this to true to enable BWE test logging.

---

`rtc_build_examples` = `false`

Set this to true to build examples.

---

`rtc_build_tools` = `true`

Set this to false to skip building tools.

---

`rtc_use_x11` = `false`

Set this to false to skip building code that requires X11.

---

`rtc_use_pipewire` = 'false'

Set this to use PipeWire on the Wayland display server. By default it's only enabled on desktop Linux (excludes ChromeOS) and only when using the sysroot as PipeWire is not available in older and supported Ubuntu and Debian distributions.

---

`rtc_link_pipewire` = `false`

Set this to link PipeWire and required libraries directly instead of using the dlopen.

---

`build_with_mozilla` = `false`

Enable to use the Mozilla internal settings.

---

`rtc_enable_android_aaudio` = `false`

Experimental: enable use of Android AAudio which requires Android SDK 26 or above and NDK r16 or above.

---

`rtc_sanitize_coverage` = `""`

Set to `func`, `block`, `edge` for coverage generation. At unit test runtime set `UBSAN_OPTIONS`=`"coverage=1"`. It is recommend to set `include_examples=0`. Use llvm's `sancov -html-report` for human readable reports. See http://clang.llvm.org/docs/SanitizerCoverage.html .

---

`rtc_prefer_fixed_point` = current_cpu == `arm` || current_cpu == `arm64`

Selects fixed-point code where possible (`true` for arm and arm64).

---

`rtc_build_with_neon` = `true`

Determines whether NEON code will be built.

---

`rtc_use_h264` = `false`

Enable this to build OpenH264 encoder/FFmpeg decoder. This is supported on all platforms except Android and iOS.

```
#ifdef WEBRTC_USE_H264
#undef WEBRTC_USE_H264
#endif
```

---

`rtc_use_absl_mutex` = `false`

Enable this flag to make webrtc::Mutex be implemented by absl::Mutex.

```
#ifdef WEBRTC_ABSL_MUTEX
#undef WEBRTC_ABSL_MUTEX
#endif
```

---

`rtc_ios_use_opengl_rendering` = `is_ios` && target_environment != `catalyst`

Determines whether OpenGL is available on iOS.

---

`rtc_include_builtin_audio_codecs` = `true`

When set to false, builtin audio encoder/decoder factories and all the audio codecs they depend on will not be included in libwebrtc.{a|lib} (they will still be included in libjingle_peerconnection_so.so and WebRTC.framework)

---
   
`rtc_include_dav1d_in_internal_decoder_factory` = `false`

Includes the dav1d decoder in the internal decoder factory when set to true.

```
#define RTC_DAV1D_IN_INTERNAL_DECODER_FACTORY
```

---

`rtc_strict_field_trials` = `""`

When enabled, a run-time check will make sure that all field trial keys have been registered in accordance with the field trial policy, see g3doc/field-trials.md. The value can be set to the following:

- "dcheck"
> RTC_DCHECKs that the field trial has been registered. RTC_DCHECK must be enabled separately.

- "warn" 
> RTC_LOGs a message with LS_WARNING severity if the field trial hasn't been registered.

```
#define WEBRTC_STRICT_FIELD_TRIALS 0 // ""
#define WEBRTC_STRICT_FIELD_TRIALS 1 // "dcheck"
#define WEBRTC_STRICT_FIELD_TRIALS 2 // "warn"
```

---

`rtc_enable_protobuf` = `true`

Enables the use of protocol buffers for debug recordings.

```
#define WEBRTC_ENABLE_PROTOBUF 1
```

---

`rtc_enable_sctp` = `true`

Set this to disable building with support for SCTP data channels.

```
#define WEBRTC_HAVE_SCTP
```

---

`rtc_build_json` = `true`

`rtc_build_libsrtp` = `true`

`rtc_build_libvpx` = `true`

---

`rtc_libvpx_build_vp9` = `true`

```
#define RTC_ENABLE_VP9
```
---

`rtc_build_opus` = `true`

`rtc_build_ssl` = `true`

Disable these to not build components which can be externally provided.

---

`rtc_enable_libevent` = `false`

`rtc_build_libevent` = `false`

Enable libevent task queues on platforms that support it.

---

`rtc_include_pulse_audio` = `false`

Excluded in Chromium since its prerequisites don't require Pulse Audio.

---

`rtc_include_internal_audio_device` = `true`

Chromium uses its own IO handling, so the internal ADM is only built for standalone WebRTC.

```
#define WEBRTC_INCLUDE_INTERNAL_AUDIO_DEVICE
```

---

`rtc_enable_avx2` = `false`

Set this to true to enable the avx2 support in webrtc.

```
#define WEBRTC_ENABLE_AVX2
```

---

`rtc_include_tests` = `false`
 
Set this to true to build the unit tests. Disabled when building with Chromium or Mozilla.

```
#define WEBRTC_NON_STATIC_TRACE_EVENT_HANDLERS 1
```

---

`rtc_use_x11_extensions` = `false`

Set this to false to skip building code that also requires X11 extensions such as Xdamage, Xfixes.

---

`rtc_disable_logging` = `false`

Set this to true to fully remove logging from WebRTC.

```
#ifdef RTC_DISABLE_LOGGING
#undef RTC_DISABLE_LOGGING
#endif
```

---

`rtc_disable_trace_events` = `false`

Set this to true to disable trace events.

```
#ifdef RTC_DISABLE_TRACE_EVENTS
#undef RTC_DISABLE_TRACE_EVENTS
#endif
```

---

`rtc_disable_check_msg` = `false`
   
Set this to true to disable detailed error message and logging for RTC_CHECKs.

```
#define RTC_DISABLE_CHECK_MSG
```

---

`rtc_disable_metrics` = `false`

Set this to true to disable webrtc metrics.

```
#ifdef RTC_DISABLE_METRICS
#undef RTC_DISABLE_METRICS
#endif
```

---

`rtc_exclude_transient_suppressor` = `false`

Set this to true to exclude the transient suppressor in the audio processing module from the build.

```
#ifdef WEBRTC_EXCLUDE_TRANSIENT_SUPPRESSOR
#undef WEBRTC_EXCLUDE_TRANSIENT_SUPPRESSOR
#endif
```

---

`rtc_desktop_capture_supported` = `is_mac`

Desktop capturer is supported only on Windows, OSX and Linux.

---

### Additional defines based on `BUILDCONFIG.gn`

`is_ios` = current_os == `ios`

```
#define WEBRTC_IOS
#define WEBRTC_MAC
```

---

`is_mac` = current_os == `mac`

```
#define WEBRTC_MAC
```

---

`is_posix` = !`is_win` && !`is_fuchsia`

```
#define WEBRTC_POSIX
```

---

current_cpu == `arm64`

```
#define WEBRTC_ARCH_ARM64
#define WEBRTC_HAS_NEON
```

---

current_cpu == `arm`

```
#define WEBRTC_ARCH_ARM
#define WEBRTC_HAS_NEON
```

---

arm_version >= `7`

```
#define WEBRTC_ARCH_ARM_V7
```

---

arm_use_neon == `true`

```
#define WEBRTC_HAS_NEON
```

---