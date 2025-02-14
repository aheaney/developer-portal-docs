---

title: MLMediaPlayerTrackNativeBindingsEditModeTests.cs

---


# MLMediaPlayerTrackNativeBindingsEditModeTests.cs









## Source code

```csharp
using System.Reflection;
using NUnit.Framework;

namespace Tests.Editor
{
    public class MLMediaPlayerTrackNativeBindingsEditModeTests : NativeBindingsTests
    {
        private MlSdkDllLoader lib;
    
        [OneTimeSetUp]
        public void Init()
        {
            lib = new MlSdkDllLoader();
            lib.Load("media_player.magicleap");
        }

        [OneTimeTearDown]
        public void Cleanup()
        {
            lib.Free();
        } 
    
        [SetUp]
        public void SetupNativeBindings()
        {
            var apiType = typeof(UnityEngine.XR.MagicLeap.MLMedia.Player.Track);
            nativeBindings = apiType.GetNestedType("NativeBindings", BindingFlags.NonPublic);
        }

        [Test]
        public void NativeBinding_MLMediaPlayerGetTrackLanguage_Exists()
        {
            AssertThatMethodExists("MLMediaPlayerGetTrackLanguage");
        }

        [Test]
        public void NativeBinding_MLMediaPlayerGetTrackType_Exists()
        {
            AssertThatMethodExists("MLMediaPlayerGetTrackType");
        }

        [Test]
        public void NativeBinding_MLMediaPlayerGetTrackMediaFormat_Exists()
        {
            AssertThatMethodExists("MLMediaPlayerGetTrackMediaFormat");
        }
    }
}
```




