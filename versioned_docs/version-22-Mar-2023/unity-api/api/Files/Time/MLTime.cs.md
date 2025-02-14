---

title: MLTime.cs

---


# MLTime.cs









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

using System;
using System.Runtime.InteropServices;

namespace UnityEngine.XR.MagicLeap
{
    public partial class MLTime
    {
        public long Value { get; private set; }

        private MLTime(long convertedNs)
        {
            Value = convertedNs;
        }

        public static implicit operator long(MLTime mltime) => mltime.Value;
        public static implicit operator MLTime(long timestamp) => new MLTime(timestamp);

        public static MLResult ConvertSystemTimeToMLTime(long timestampNs, out MLTime mlTime) => MLResult.Create(InternalConvertSystemTimeToMLTime(timestampNs, out mlTime));

        public static MLResult ConvertMLTimeToSystemTime(MLTime mlTime, out long timestampNs) => MLResult.Create(InternalConvertMLTimeToSystemTime(mlTime, out timestampNs));

        private static MLResult.Code InternalConvertSystemTimeToMLTime(long timestampNs, out MLTime mlTime)
        {
            NativeBindings.TimeSpec timeSpec;
            timeSpec.Seconds = timestampNs / 1000000000;
            timeSpec.Nanoseconds = timestampNs - timeSpec.Seconds;

            MLResult.Code resultCode = MLResult.Code.UnspecifiedFailure;

            IntPtr ptr = Marshal.AllocHGlobal(Marshal.SizeOf(typeof(NativeBindings.TimeSpec)));
            try
            {
                Marshal.StructureToPtr(timeSpec, ptr, false);

                resultCode = NativeBindings.MLTimeConvertSystemTimeToMLTime(ptr, out long nanoseconds);
                if (MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLTimeConvertSystemTimeToMLTime)))
                {
                    mlTime = new MLTime(nanoseconds);
                }
                else
                {
                    mlTime = new MLTime(0);
                }
            }
            finally
            {
                Marshal.FreeHGlobal(ptr);
            }

            return resultCode;  

        }

        private static MLResult.Code InternalConvertMLTimeToSystemTime(MLTime mlTime, out long timestampNs)
        {
            IntPtr timeSpecPtr = Marshal.AllocHGlobal(Marshal.SizeOf(typeof(NativeBindings.TimeSpec)));

            MLResult.Code resultCode = MLResult.Code.UnspecifiedFailure;
            try
            {
                resultCode = NativeBindings.MLTimeConvertMLTimeToSystemTime(mlTime.Value, timeSpecPtr);
                if (MLResult.DidNativeCallSucceed(resultCode, nameof(NativeBindings.MLTimeConvertMLTimeToSystemTime)))
                {
                    var timeSpec = (NativeBindings.TimeSpec)Marshal.PtrToStructure(timeSpecPtr, typeof(NativeBindings.TimeSpec));
                    timestampNs = (long)((timeSpec.Seconds * 1000000000) + timeSpec.Nanoseconds);
                }
                else
                {
                    timestampNs = 0;
                }

            }
            finally
            {
                Marshal.FreeHGlobal(timeSpecPtr);
            }
            return resultCode;
        }

        public override string ToString()
        {
            return Value.ToString();
        }
    }
}
```




