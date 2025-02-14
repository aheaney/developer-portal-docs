---

title: MLCVCamera.cs

---


# MLCVCamera.cs









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

namespace UnityEngine.XR.MagicLeap
{
    using System;
    using System.Runtime.InteropServices;
    using System.Threading;
    using UnityEngine.XR.MagicLeap.Native;

    [RequireXRLoader]
    public sealed partial class MLCVCamera : MLAutoAPISingleton<MLCVCamera>
    {
        public static MLResult GetFramePose(MLTime vcamTimestamp, out Matrix4x4 outTransform)
        {
            getFramePosePerfMarker.Begin();
            MLResult result = Instance.InternalGetFramePose(NativeBindings.CameraID.ColorCamera, vcamTimestamp, out outTransform);
            getFramePosePerfMarker.End();

            return result;
        }

        protected override MLResult.Code StartAPI()
        {
            if (!MLDevice.IsReady())
            {
                MLPluginLog.WarningFormat("MLCamera API is attempting to start before the MagicLeap XR Loader has been initialiazed, this could cause issues with MLCVCamera features. If your application needs these features please wait to start API until Monobehavior.Start and if issue persists make sure ProjectSettings/XR/Initialize On Startup is enabled.");
            }

            nativeMLCVCameraTrackingCreatePerfMarker.Begin();
            MLResult.Code code = NativeBindings.MLCVCameraTrackingCreate(ref Handle);
            MLResult.DidNativeCallSucceed(code, nameof(NativeBindings.MLCVCameraTrackingCreate));
            nativeMLCVCameraTrackingCreatePerfMarker.End();

            return code;
        }

        protected override MLResult.Code StopAPI()
        {
            nativeMLCVCameraTrackingDestroyPerfMarker.Begin();
            MLResult.Code code = NativeBindings.MLCVCameraTrackingDestroy(Handle);
            nativeMLCVCameraTrackingDestroyPerfMarker.End();

            return code;
        }

        private MLResult InternalGetFramePose(NativeBindings.CameraID cameraId, MLTime vcamTimestamp, out Matrix4x4 outTransform)
        {
            MagicLeapNativeBindings.MLTransform outInternalTransform = new MagicLeapNativeBindings.MLTransform();

            MLResult.Code resultCode = NativeBindings.MLCVCameraGetFramePose(Handle, MagicLeapXrProviderNativeBindings.GetHeadTrackerHandle(), cameraId, vcamTimestamp.Value, ref outInternalTransform);
            MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLCVCameraGetFramePose));
            MLResult poseResult = MLResult.Create(resultCode);
            if (!poseResult.IsOk)
            {
                MLPluginLog.ErrorFormat("MLCamera.InternalGetFramePose failed to get camera frame pose. Reason: {0}", poseResult);
                outTransform = new Matrix4x4();
            }
            else
            {
                outTransform = MLConvert.ToUnity(outInternalTransform);
            }

            return poseResult;
        }
    }
}
```




