---

title: MLAudioOutputPluginNativeBindingsEditModeTests.cs

---


# MLAudioOutputPluginNativeBindingsEditModeTests.cs









## Source code

```csharp
using NUnit.Framework;
using System.Reflection;

namespace Tests.Editor
{
    public class MLAudioOutputPluginNativeBindingsEditModeTests : NativeBindingsTests
    {
        [SetUp]
        public void SetupNativeBindings()
        {
            var apiType = typeof(UnityEngine.XR.MagicLeap.MLAudioPlayback);
            nativeBindings = apiType.GetNestedType("NativeBindings", BindingFlags.Public);
        }

        [Test]
        public void NativeBinding_CreateAudioOutput_Exists()
        {
            AssertThatMethodExists("CreateAudioOutput");
        }

        [Test]
        public void NativeBinding_DestroyAudioOutput_Exists()
        {
            AssertThatMethodExists("DestroyAudioOutput");
        }

        [Test]
        public void NativeBinding_OnUnityAudio_Exists()
        {
            AssertThatMethodExists("OnUnityAudio");
        }

        [Test]
        public void NativeBinding_CreateOutputBuffer_Exists()
        {
            AssertThatMethodExists("CreateOutputBuffer");
        }
    }
}
```




