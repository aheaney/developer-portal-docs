---

title: MagicLeapXrProviderNativeBindings.cs

---


# MagicLeapXrProviderNativeBindings.cs









## Source code

```csharp
// %BANNER_BEGIN%
// ---------------------------------------------------------------------
// %COPYRIGHT_BEGIN%
// Copyright (c) (2021-2022) Magic Leap, Inc. All Rights Reserved.
// Use of this file is governed by the Software License Agreement, located here: https://www.magicleap.com/software-license-agreement-ml2
// Terms and conditions applicable to third-party materials accompanying this distribution may also be found in the top-level NOTICE file appearing herein.
// %COPYRIGHT_END%
// ---------------------------------------------------------------------
// %BANNER_END%

using UnityEngine;

namespace UnityEngine.XR.MagicLeap
{
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Runtime.InteropServices;
    using Native;
    using UnityEngine.XR.ARSubsystems;
    using UnityEngine.XR.InteractionSubsystems;

    public sealed class MagicLeapXrProviderNativeBindings : Native.MagicLeapNativeBindings
    {
        internal const string MagicLeapXrProviderDll = "MagicLeapXrProvider";

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl, CharSet = CharSet.Ansi)]
        internal static extern void MagicLeapXrProviderAddLibraryPath([MarshalAs(UnmanagedType.LPStr)] string path);

        [DllImport(UnityMagicLeapDll, CallingConvention = CallingConvention.Cdecl, CharSet = CharSet.Ansi)]
        internal static extern void UnityMagicLeap_SetLibraryPath([MarshalAs(UnmanagedType.LPStr)] string path);

#if UNITY_EDITOR
        [DllImport(MLSdkLoaderDll, CallingConvention = CallingConvention.Cdecl, CharSet = CharSet.Ansi)]
        internal static extern void MLSdkLoaderResetLibraryPaths();

        [DllImport(MLSdkLoaderDll, CallingConvention = CallingConvention.Cdecl, CharSet = CharSet.Ansi)]
        internal static extern void MLSdkLoaderUnloadAllLibraries();

        [DllImport(MLSdkLoaderDll, CallingConvention = CallingConvention.Cdecl, CharSet = CharSet.Ansi)]
        internal static extern MLResult.Code MLZIIsServerConfigured([MarshalAs(UnmanagedType.I1)] out bool isConfigured);
#endif

        public enum LogLevel : uint
        {
            Fatal,
            Error,
            Warning,
            Info,
            Debug,
            Verbose,
        }

        private static ulong DebugHandle;
        private static ulong loaderDebugHandle;
        public const string SettingsKey = "com.magicleap.unitysdk.settings";

        MagicLeapXrProviderSettings GetSettings()
        {
            MagicLeapXrProviderSettings settings = null;
            // When running in the Unity Editor, we have to load user's customization of configuration data directly from
            // EditorBuildSettings. At runtime, we need to grab it from the static instance field instead.
#if UNITY_EDITOR
            UnityEditor.EditorBuildSettings.TryGetConfigObject(SettingsKey, out settings);
#else
            //settings = SampleSettings.s_RuntimeInstance;
#endif
            return settings;
        }

        public delegate void OnDebugMessageDelegate(LogLevel logLevel, [MarshalAs(UnmanagedType.LPStr)] string message, IntPtr context);

        internal delegate void CallOnPerceptionShutdownDelegate();

        internal delegate void CreateBlockRequestsDelegate(ref MeshingSubsystem.Extensions.MLMeshing.NativeBindings.MLMeshingMeshInfo meshInfo, ref MeshingSubsystem.Extensions.MLMeshing.NativeBindings.MLMeshingMeshRequest data);

        internal delegate void CallFreeBlockRequestPointerDelegate();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        internal static extern bool StartEyeTracking();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        internal static extern void StopEyeTracking();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        internal static extern void StartHandTracking();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        internal static extern void StopHandTracking();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        internal static extern void StartGestureTracking();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        internal static extern void StopGestureTracking();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        public static extern ulong GetHandTrackerHandle();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        public static extern ulong GetControllerTrackerHandle();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        public static extern ulong GetHeadTrackerHandle();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        public static extern ulong GetInputHandle();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        public static extern MLResult.Code GetUnityPose(MLCoordinateFrameUID cfuid, out Pose pose);

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        public static extern MLResult.Code StartHapticsPattern(uint eventType, IntPtr buffer);

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        public static extern MLResult.Code StopHaptics();

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        internal static extern void InputSetOnPerceptionShutdownCallback(CallOnPerceptionShutdownDelegate createOnPerceptionShutdown);

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        internal static extern void MeshingSetMeshRequestCallback(CreateBlockRequestsDelegate createBlockRequest);

        [DllImport(MagicLeapXrProviderDll, CallingConvention = CallingConvention.Cdecl)]
        internal static extern void MeshingSetFreeBlockRequestPointerCallback(CallFreeBlockRequestPointerDelegate createFreePointer);

        [AOT.MonoPInvokeCallback(typeof(OnDebugMessageDelegate))]
        private static void OnDebugMessage(LogLevel logLevel, [MarshalAs(UnmanagedType.LPStr)] string message, IntPtr context)
        {
            if (logLevel == LogLevel.Fatal)
            {
                throw new Exception(message);
            }
            else if (logLevel == LogLevel.Error)
            {
                Debug.LogError("[LuminXr] " + message);
            }
            else if (logLevel == LogLevel.Warning)
            {
                Debug.LogWarning("[LuminXr] " + message);
            }
            else
            {
                Debug.Log("[LuminXr] " + message);
            }
        }

        [StructLayout(LayoutKind.Sequential)]
        public struct MagicLeapXrProviderDebugUtils
        {
            public IntPtr Context;

            public LogLevel LogLevel;

            public OnDebugMessageDelegate OnDebugMessage;

            public static MagicLeapXrProviderDebugUtils Create()
            {
                MagicLeapXrProviderDebugUtils debugUtils = new MagicLeapXrProviderDebugUtils
                {
                    Context = IntPtr.Zero,
                    LogLevel = LogLevel.Error,
                    OnDebugMessage = MagicLeapXrProviderNativeBindings.OnDebugMessage
                };

                return debugUtils;
            }
        }
    }
}
```



