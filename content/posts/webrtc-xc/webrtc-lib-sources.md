---
title: "libWebRTC Source Files"
date: 2023-08-20T18:00:00-07:00
draft: false
_build:
    list: false
---
[Back to Article]({{< ref "/posts/webrtc-xc/main#libwebrtc">}})

{{< adsense-feed >}}

- sdk/objc/api/logging/RTCCallbackLogger.mm
- sdk/objc/api/peerconnection/RTCAudioSource.mm
- sdk/objc/api/peerconnection/RTCAudioTrack.mm
- sdk/objc/api/peerconnection/RTCCertificate.mm
- sdk/objc/api/peerconnection/RTCConfiguration.mm
- sdk/objc/helpers/UIDevice+RTCDevice.m
- sdk/objc/api/peerconnection/RTCCryptoOptions.mm
- sdk/objc/api/peerconnection/RTCDataChannel.mm
- sdk/objc/api/peerconnection/RTCDataChannelConfiguration.mm
- sdk/objc/api/peerconnection/RTCDtmfSender.mm
- sdk/objc/api/peerconnection/RTCEncodedImage+Private.mm
- sdk/objc/api/peerconnection/RTCFieldTrials.mm
- sdk/objc/api/peerconnection/RTCFileLogger.mm
- sdk/objc/api/peerconnection/RTCIceCandidate.mm
- sdk/objc/api/peerconnection/RTCIceCandidateErrorEvent.mm
- sdk/objc/helpers/NSString+StdString.mm
- sdk/objc/api/peerconnection/RTCIceServer.mm
- sdk/objc/api/peerconnection/RTCLegacyStatsReport.mm
- sdk/objc/api/peerconnection/RTCMediaConstraints.mm
- sdk/objc/api/peerconnection/RTCMediaSource.mm
- sdk/objc/api/peerconnection/RTCMediaStream.mm
- sdk/objc/api/peerconnection/RTCMediaStreamTrack.mm
- sdk/objc/api/peerconnection/RTCMetrics.mm
- sdk/objc/api/peerconnection/RTCMetricsSampleInfo.mm
- sdk/objc/api/peerconnection/RTCPeerConnection.mm
- sdk/objc/api/peerconnection/RTCPeerConnection+DataChannel.mm
- sdk/objc/api/peerconnection/RTCPeerConnection+Stats.mm
- sdk/objc/api/peerconnection/RTCPeerConnectionFactory.mm
- sdk/objc/api/peerconnection/RTCPeerConnectionFactoryBuilder.mm
- sdk/objc/api/peerconnection/RTCPeerConnectionFactoryBuilder+DefaultComponents.mm
- sdk/objc/api/peerconnection/RTCPeerConnectionFactoryOptions.mm
- sdk/objc/api/peerconnection/RTCRtcpParameters.mm
- sdk/objc/api/peerconnection/RTCRtpCodecParameters.mm
- sdk/objc/api/peerconnection/RTCRtpEncodingParameters.mm
- sdk/objc/api/peerconnection/RTCRtpHeaderExtension.mm
- sdk/objc/api/peerconnection/RTCRtpParameters.mm
- sdk/objc/api/peerconnection/RTCRtpReceiver.mm
- sdk/objc/api/peerconnection/RTCRtpSender.mm
- sdk/objc/api/peerconnection/RTCRtpTransceiver.mm
- sdk/objc/api/peerconnection/RTCSessionDescription.mm
- sdk/objc/api/peerconnection/RTCSSLAdapter.mm
- sdk/objc/api/peerconnection/RTCStatisticsReport.mm
- sdk/objc/api/peerconnection/RTCTracing.mm
- sdk/objc/api/peerconnection/RTCVideoCodecInfo+Private.mm
- sdk/objc/components/video_codec/nalu_rewriter.cc
- sdk/objc/api/peerconnection/RTCVideoEncoderSettings+Private.mm
- sdk/objc/api/peerconnection/RTCVideoSource.mm
- sdk/objc/api/peerconnection/RTCVideoTrack.mm
- sdk/objc/api/RTCVideoRendererAdapter.mm
- sdk/objc/api/video_codec/RTCVideoCodecConstants.mm
- sdk/objc/api/video_codec/RTCVideoDecoderAV1.mm
- sdk/objc/api/video_codec/RTCVideoDecoderVP8.mm
- sdk/objc/api/video_codec/RTCVideoDecoderVP9.mm
- sdk/objc/api/video_codec/RTCVideoEncoderAV1.mm
- sdk/objc/api/video_codec/RTCVideoEncoderVP8.mm
- sdk/objc/api/video_codec/RTCVideoEncoderVP9.mm
- sdk/objc/api/video_codec/RTCWrappedNativeVideoDecoder.mm
- sdk/objc/api/video_codec/RTCWrappedNativeVideoEncoder.mm
- sdk/objc/api/video_frame_buffer/RTCNativeI420Buffer.mm
- sdk/objc/api/video_frame_buffer/RTCNativeMutableI420Buffer.mm
- sdk/objc/base/RTCEncodedImage.m
- sdk/objc/base/RTCLogging.mm

{{< adsense-feed >}}

- sdk/objc/base/RTCVideoCapturer.m
- sdk/objc/base/RTCVideoCodecInfo.m
- sdk/objc/base/RTCVideoEncoderQpThresholds.m
- sdk/objc/base/RTCVideoEncoderSettings.m
- sdk/objc/base/RTCVideoFrame.mm
- sdk/objc/components/capturer/RTCCameraVideoCapturer.m
- sdk/objc/components/capturer/RTCFileVideoCapturer.m
- sdk/objc/helpers/AVCaptureSession+DevicePosition.m
- sdk/objc/components/video_codec/helpers.cc
- sdk/objc/helpers/RTCCameraPreviewView.m
- sdk/objc/helpers/RTCDispatcher.m
- sdk/objc/native/api/audio_device_module.mm
- sdk/objc/components/video_codec/UIDevice+H264Profile.mm
- sdk/objc/native/api/native_network_monitor_factory.mm
- sdk/objc/native/api/objc_audio_device_module.mm
- sdk/objc/native/api/ssl_certificate_verifier.mm
- sdk/objc/native/api/video_capturer.mm
- sdk/objc/native/api/objc_to_native_video_decoder_factory.mm
- sdk/objc/native/api/objc_to_native_video_encoder_factory.mm
- sdk/objc/native/api/video_frame_buffer.mm
- sdk/objc/native/api/native_to_objc_video_frame.mm
- sdk/objc/native/api/video_renderer.mm
- sdk/objc/native/src/objc_audio_device_delegate.mm
- sdk/objc/native/src/objc_audio_device.mm
- sdk/objc/native/src/objc_frame_buffer.mm
- sdk/objc/native/src/objc_network_monitor.mm
- sdk/objc/native/src/objc_video_decoder_factory.mm
- sdk/objc/native/src/objc_video_encoder_factory.mm
- sdk/objc/native/src/objc_video_frame.mm
- sdk/objc/native/src/objc_video_renderer.mm
- sdk/objc/native/src/objc_video_track_source.mm
- sdk/objc/components/network/RTCNetworkMonitor.mm
- sdk/objc/components/renderer/metal/RTCMTLNSVideoView.m
- sdk/objc/components/renderer/metal/RTCMTLVideoView.m
- sdk/objc/components/renderer/metal/RTCMTLI420Renderer.mm
- sdk/objc/components/renderer/metal/RTCMTLNV12Renderer.mm
- sdk/objc/components/renderer/metal/RTCMTLRenderer.mm
- sdk/objc/components/renderer/metal/RTCMTLRGBRenderer.mm
- sdk/objc/components/renderer/opengl/RTCDisplayLinkTimer.m
- sdk/objc/components/renderer/opengl/RTCEAGLVideoView.m
- sdk/objc/components/renderer/opengl/RTCNV12TextureCache.m
- sdk/objc/components/renderer/opengl/RTCDefaultShader.mm
- sdk/objc/components/renderer/opengl/RTCI420TextureCache.mm
- sdk/objc/components/renderer/opengl/RTCShader.mm
- sdk/objc/components/video_codec/RTCDefaultVideoDecoderFactory.m
- sdk/objc/components/video_codec/RTCDefaultVideoEncoderFactory.m
- sdk/objc/components/video_codec/RTCVideoDecoderFactoryH264.m
- sdk/objc/components/video_codec/RTCVideoEncoderFactoryH264.m
- sdk/objc/components/video_codec/RTCCodecSpecificInfoH264.mm
- sdk/objc/components/video_codec/RTCH264ProfileLevelId.mm
- sdk/objc/components/video_codec/RTCVideoDecoderH264.mm
- sdk/objc/components/video_codec/RTCVideoEncoderH264.mm
- sdk/objc/components/video_frame_buffer/RTCCVPixelBuffer.mm

{{< adsense-feed >}}