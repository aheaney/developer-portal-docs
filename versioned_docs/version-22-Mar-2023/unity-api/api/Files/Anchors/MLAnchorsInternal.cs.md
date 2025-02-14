---

title: MLAnchorsInternal.cs

---


# MLAnchorsInternal.cs









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
    using Native;

    public partial class MLAnchors
    {
        private MLResult.Code CreateQuery(Request.Params requestParams, out ulong queryHandle, out uint resultsCount) => NativeBindings.MLSpatialAnchorQueryFilter.Create(requestParams, out queryHandle, out resultsCount);

        private MLResult.Code GetQueryResult(Request.Params requestParams, ulong queryHandle, uint firstIndex, uint lastIndex, NativeBindings.MLSpatialAnchor[] anchors)
        {
            var resultCode = NativeBindings.MLSpatialAnchorQueryGetResult(this.Handle, queryHandle, firstIndex, lastIndex, anchors);
            MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLSpatialAnchorQueryGetResult));
            return resultCode;
        }

        private MLResult.Code DestroyQuery(ulong queryHandle)
        {
            var resultCode = NativeBindings.MLSpatialAnchorQueryDestroy(this.Handle, queryHandle);
            MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLSpatialAnchorQueryDestroy));
            return resultCode;
        }

        private MLResult.Code GetLocalizationInformation(out MLAnchors.LocalizationInfo info)
        {
            var nativeInfo = NativeBindings.MLSpatialAnchorLocalizationInfo.Create();
            var resultCode = NativeBindings.MLSpatialAnchorGetLocalizationInfo(this.Handle, ref nativeInfo);
            MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLSpatialAnchorGetLocalizationInfo));
            info = new LocalizationInfo(nativeInfo);
            return resultCode;
        }

        private MLResult.Code CreateAnchor(NativeBindings.MLSpatialAnchorCreateInfo createInfo, out NativeBindings.MLSpatialAnchor anchor)
        {
            var resultCode = NativeBindings.MLSpatialAnchorCreate(this.Handle, in createInfo, out anchor);
            MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLSpatialAnchorCreate));
            return resultCode;
        }

        private MLResult.Code PublishAnchor(Anchor anchor)
        {
            var resultCode = NativeBindings.MLSpatialAnchorPublish(this.Handle, anchor.id);
            MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLSpatialAnchorPublish));
            return resultCode;
        }

        private MLResult.Code UpdateAnchor(Anchor anchor, long expirationTimeStamp)
        {
            var unixTimestamp = (DateTime.UtcNow.AddSeconds(expirationTimeStamp)).Subtract(new DateTime(1970, 1, 1)).TotalSeconds;
            var nativeAnchor = new NativeBindings.MLSpatialAnchor(anchor, (ulong)unixTimestamp);
            var resultCode = NativeBindings.MLSpatialAnchorUpdate(this.Handle, in nativeAnchor);
            MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLSpatialAnchorUpdate));
            return resultCode;
        }

        private MLResult.Code DeleteAnchor(Anchor anchor)
        {
            var resultCode = NativeBindings.MLSpatialAnchorDelete(this.Handle, anchor.id);
            MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLSpatialAnchorDelete));
            return resultCode;
        }
        private MLResult.Code DeleteAnchor(string anchorId)
        {
            var resultCode = NativeBindings.MLSpatialAnchorDelete(this.Handle, new MagicLeapNativeBindings.MLUUIDBytes(anchorId));
            MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLSpatialAnchorDelete));
            return resultCode;
        }

    }
}
```




