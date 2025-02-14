---

title: MLCameraBaseInternal.cs

---


# MLCameraBaseInternal.cs









## Source code

```csharp
// %BANNER_BEGIN%
// ---------------------------------------------------------------------
// %COPYRIGHT_BEGIN%
// Copyright (c) (2018-2022) Magic Leap, Inc. All Rights Reserved.
// Use of this file is governed by the Software License Agreement, located here: https://www.magicleap.com/software-license-agreement-ml2
// Terms and conditions applicable to third-party materials accompanying this distribution may also be found in the top-level NOTICE file appearing herein.
// %COPYRIGHT_END%
// ---------------------------------------------------------------------
// %BANNER_END%

// ReSharper disable All

namespace UnityEngine.XR.MagicLeap
{
    using System;
    using System.Runtime.InteropServices;
    using UnityEngine.XR.MagicLeap.Native;


    public partial class MLCameraBase
    {
        #region properties

        protected static bool cameraInited = false;

        protected static object rawVideoFrameQueueLockObject = new object();

        protected MLCamera.Renderer previewRenderer;

        protected GCHandle gcHandle;

        protected RenderTexture previewTexture;

        protected bool cameraConnectionEstablished = false;

        protected long previousCaptureTimestamp;

        protected float currentFPS;

        protected bool isCapturingVideo = false;

        protected bool isCapturingPreview = false;

        protected Texture2D previewTexture2D = null;

        protected bool resumeConnect = false;

        protected bool resumePreviewCapture = false;

        protected bool resumeVideoCapture = false;

        protected MLCamera.ConnectContext cameraConnectContext;

        protected MLCamera.CaptureConfig cameraCaptureConfig;

        private bool wasCapturingVideo = false;

        private Texture2D PreviewTexture2D
        {
            get => isCapturingPreview ? previewTexture2D : null;

            set => previewTexture2D = value;
        }

        protected byte[][] byteArrays;

        #endregion

        protected MLCameraBase()
        {
            gcHandle = GCHandle.Alloc(this, GCHandleType.Weak);
            Handle = Native.MagicLeapNativeBindings.InvalidHandle;
            byteArrays = new byte[MLCamera.NativeBindings.MLCameraMaxImagePlanes][];
        }

        protected void CreatePreviewTexture()
        {
            int width = cameraCaptureConfig.StreamConfigs[0].Width;
            int height = cameraCaptureConfig.StreamConfigs[0].Height;

            if (previewTexture != null && (previewTexture.width != width || previewTexture.height != height))
            {
                ClearPreviewTexture();
            }
            if (previewTexture == null)
            {
                previewTexture = new RenderTexture(width, height, 0, RenderTextureFormat.ARGB32);
            }

            // preview rendering not supported under Magic Leap App Simulator
#if !UNITY_EDITOR
            previewRenderer?.Cleanup();
            previewRenderer = new MLCamera.Renderer();
            previewRenderer.SetRenderBuffer(previewTexture);
#endif
        }

        private void ReleaseCaptureBuffers()
        {
            for (int i = 0; i < byteArrays.Length; ++i)
            {
                byteArrays[i] = null;
            }
        }

        protected void GLPluginEvent()
        {
            // preview rendering not supported under Magic Leap App Simulator
#if !UNITY_EDITOR
            previewRenderer?.Render();
#endif
        }

        private MLResult.Code DisconnectNative()
        {
            var resultCode = MLCamera.NativeBindings.MLCameraDisconnect(Handle);

            // preview rendering not supported under Magic Leap App Simulator
            MLThreadDispatch.ScheduleMain(() => previewRenderer?.Cleanup());
            MLResult.DidNativeCallSucceed(resultCode, nameof(MLCamera.NativeBindings.MLCameraDisconnect));

            return resultCode;
        }

        protected static MLResult InternalInitialize()
        {
            MLResult.Code resultCode = MLResult.Code.Ok;

            if (!cameraInited)
            {
                MLCamera.NativeBindings.MLCameraDeviceAvailabilityStatusCallbacks callbacks =
                    MLCamera.NativeBindings.MLCameraDeviceAvailabilityStatusCallbacks.Create();
                resultCode = MLCamera.NativeBindings.MLCameraInit(ref callbacks, IntPtr.Zero);
                cameraInited = MLResult.DidNativeCallSucceed(resultCode, nameof(MLCamera.NativeBindings.MLCameraInit));
            }


            return MLResult.Create(resultCode);
        }
        protected static MLResult InternalUninitialize()
        {
            if (!cameraInited)  // already uninited
                return MLResult.Create(MLResult.Code.Ok);

            cameraInited = false;
            MLCamera.NativeBindings.MLCameraDeviceAvailabilityStatusCallbacks callbacks =
                MLCamera.NativeBindings.MLCameraDeviceAvailabilityStatusCallbacks.CreateUninitialized();
            var resultCode = MLCamera.NativeBindings.MLCameraDeInit();
            MLResult.DidNativeCallSucceed(resultCode, nameof(MLCamera.NativeBindings.MLCameraDeInit));
            return MLResult.Create(resultCode);
        }

        protected static MLResult.Code InternalCheckCameraPermission()
        {
            var check = MLPermissions.CheckPermission(MLPermission.Camera);
            if (!MLResult.DidNativeCallSucceed(check.Result, nameof(MLPermissions.CheckPermission)))
            {
                return MLResult.Code.PermissionDenied;
            }
            return MLResult.Code.Ok;
        }

        protected MLResult.Code InternalConnect(MLCamera.ConnectContext cameraConnectContext)
        {
            this.cameraConnectContext = cameraConnectContext;

            MLCamera.NativeBindings.MLCameraConnectContext context = MLCamera.NativeBindings.MLCameraConnectContext.Create(cameraConnectContext);

            MLResult.Code resultCode = MLResult.Code.Ok;


            if (!cameraConnectionEstablished)
            {
                resultCode = MLCamera.NativeBindings.MLCameraConnect(ref context, out Handle);

                if (!MLResult.DidNativeCallSucceed(resultCode, nameof(MLCamera.NativeBindings.MLCameraConnect)))
                {
                    Debug.LogErrorFormat("MLCamera.Connect failed connecting to the camera. Reason: {0}", resultCode);
                    MLCamera.connectPerfMarker.End();
                    return resultCode;
                }

                MLCamera.NativeBindings.MLCameraCaptureCallbacks captureCallbacks = MLCamera.NativeBindings.MLCameraCaptureCallbacks.Create();

                if (!MLResult.DidNativeCallSucceed(MLCamera.NativeBindings.MLCameraSetCaptureCallbacks(Handle, ref captureCallbacks, GCHandle.ToIntPtr(gcHandle)), nameof(MLCamera.NativeBindings.MLCameraSetCaptureCallbacks)))
                {
                    MLPluginLog.ErrorFormat("MLCamera.CaptureCallbacks failed seting the capture callbacks. Reason: {0}", resultCode);
                    MLCamera.connectPerfMarker.End();
                    return resultCode;
                }

                MLCamera.NativeBindings.MLCameraDeviceStatusCallbacks deviceCallbacks = MLCamera.NativeBindings.MLCameraDeviceStatusCallbacks.Create();

                if (!MLResult.DidNativeCallSucceed(MLCamera.NativeBindings.MLCameraSetDeviceStatusCallbacks(Handle, ref deviceCallbacks, GCHandle.ToIntPtr(gcHandle)), nameof(MLCamera.NativeBindings.MLCameraSetDeviceStatusCallbacks)))
                {
                    MLPluginLog.ErrorFormat("MLCamera.DeviceStatusCallbacks failed setting the device callbacks. Reason: {0}", resultCode);
                    MLCamera.connectPerfMarker.End();
                    return resultCode;
                }

                cameraConnectionEstablished = MLResult.IsOK(resultCode);
            }

            if (!MLResult.IsOK(resultCode))
            {
                MLPluginLog.ErrorFormat("MLCamera.InternalConnect failed to populate characteristics for MLCamera. Reason: {0}", resultCode);
                MLCamera.connectPerfMarker.End();
                return resultCode;
            }

            MLCamera.connectPerfMarker.End();
            return resultCode;
        }

        protected MLResult.Code InternalDisconnect(bool flagsOnly = false)
        {
            MLCamera.disconnectPerfMarker.Begin();

            if (this.cameraConnectionEstablished)
            {
                if (this.isCapturingVideo && !flagsOnly)
                {
                    var stopResult = CaptureVideoStop();
                    if (stopResult.IsOk)
                    {
                        this.isCapturingVideo = false;
                    }
                }

                if (this.isCapturingPreview && !flagsOnly)
                {
                    var stopResult = CapturePreviewStop();
                    if (stopResult.IsOk)
                    {
                        isCapturingPreview = false;
                    }
                }

                MLResult.Code result = DisconnectNative();

                // Assume that connection is no longer established
                // even if there is some failure disconnecting.
                this.cameraConnectionEstablished = false;
            }

            ReleaseCaptureBuffers();
            MLCamera.disconnectPerfMarker.Begin();

            return MLResult.Code.Ok;
        }

        protected MLResult InternalGetStreamCapabilities(out MLCamera.StreamCapabilitiesInfo[] streamCapabilities)
        {
            streamCapabilities = null;

            var resultCode = MLCamera.NativeBindings.MLCameraGetNumSupportedStreams(Handle, out uint supportedStreams);
            if (!MLResult.DidNativeCallSucceed(resultCode, nameof(MLCamera.NativeBindings.MLCameraGetNumSupportedStreams)))
            {
                return MLResult.Create(resultCode);
            }

            streamCapabilities = new MLCamera.StreamCapabilitiesInfo[supportedStreams];
            int sizeOfStruct = Marshal.SizeOf(typeof(MLCamera.NativeBindings.MLCameraCaptureStreamCaps));

            for (uint i = 0; i < supportedStreams; i++)
            {
                streamCapabilities[i] = new MLCamera.StreamCapabilitiesInfo();
                uint streamCapsMax = 0;

                resultCode = MLCamera.NativeBindings.MLCameraGetStreamCaps(Handle, i, ref streamCapsMax, IntPtr.Zero);
                if (!MLResult.DidNativeCallSucceed(resultCode, nameof(MLCamera.NativeBindings.MLCameraGetStreamCaps)))
                {
                    return MLResult.Create(resultCode);
                }

                streamCapabilities[i].StreamCapabilities = new MLCamera.StreamCapability[streamCapsMax];
                IntPtr pointer = Marshal.AllocHGlobal((int)streamCapsMax *
                    Marshal.SizeOf(typeof(MLCamera.NativeBindings.MLCameraCaptureStreamCaps)));
                resultCode = MLCamera.NativeBindings.MLCameraGetStreamCaps(Handle, i, ref streamCapsMax, pointer);
                if (!MLResult.DidNativeCallSucceed(resultCode, nameof(MLCamera.NativeBindings.MLCameraGetStreamCaps)))
                {
                    Marshal.FreeHGlobal(pointer);
                    return MLResult.Create(resultCode);
                }

                for (int j = 0; j < streamCapsMax; j++)
                {
                    MLCamera.NativeBindings.MLCameraCaptureStreamCaps nativeStreamCap =
                        Marshal.PtrToStructure<MLCamera.NativeBindings.MLCameraCaptureStreamCaps>(pointer);

                    streamCapabilities[i].StreamCapabilities[j] = new MLCamera.StreamCapability()
                    {
                        Width = nativeStreamCap.Width,
                        Height = nativeStreamCap.Height,
                        CaptureType = nativeStreamCap.CaptureType
                    };

                    pointer += sizeOfStruct;
                }
            }

            return MLResult.Create(resultCode);
        }

        protected MLResult.Code InternalPrepareCapture(MLCamera.CaptureConfig captureConfig, out MLCamera.Metadata cameraMetadata)
        {
            cameraCaptureConfig = captureConfig;
            cameraMetadata = null;

            MLResult.Code resultCode = MLResult.Code.Ok;
            MLCamera.NativeBindings.MLCameraCaptureConfig nativeCaptureConfig = MLCamera.NativeBindings.MLCameraCaptureConfig.Create(captureConfig);
            resultCode = MLCamera.NativeBindings.MLCameraPrepareCapture(Handle, ref nativeCaptureConfig, out ulong metadataHandle);
            if (MLResult.DidNativeCallSucceed(resultCode, nameof(MLCamera.NativeBindings.MLCameraPrepareCapture)))
            {
                cameraMetadata = new MLCamera.Metadata(metadataHandle);
            }
            return resultCode;
        }
    }
}
```



