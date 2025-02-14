---

title: MLMediaParserCEA608NativeBindingsEditModeTests.cs

---


# MLMediaParserCEA608NativeBindingsEditModeTests.cs









## Source code

```csharp
using System.Reflection;
using NUnit.Framework;

namespace Tests.Editor
{
    public class MLMediaParserCEA608NativeBindingsEditModeTests : NativeBindingsTests
    {
        private MlSdkDllLoader lib;
    
        [OneTimeSetUp]
        public void Init()
        {
            lib = new MlSdkDllLoader();
            lib.Load("media_ccparser.magicleap");
        }

        [OneTimeTearDown]
        public void Cleanup()
        {
            lib.Free();
        } 
    
        [SetUp]
        public void SetupNativeBindings()
        {
            var apiType = typeof(UnityEngine.XR.MagicLeap.MLMedia.ParserCEA608);
            nativeBindings = apiType.GetNestedType("NativeBindings", BindingFlags.Public);
        }

        [Test]
        public void NativeBinding_MLMediaCCParserCreate_Exists()
        {
            AssertThatMethodExists("MLMediaCCParserCreate");
        }

        [Test]
        public void NativeBinding_MLMediaCCParserGetDisplayableEx_Exists()
        {
            AssertThatMethodExists("MLMediaCCParserGetDisplayableEx");
        }

        [Test]
        public void NativeBinding_MLMediaCCParserDestroy_Exists()
        {
            AssertThatMethodExists("MLMediaCCParserDestroy");
        }

        [Test]
        public void NativeBinding_MLMediaCCParserSetDisplayChangedCallback_Exists()
        {
            AssertThatMethodExists("MLMediaCCParserSetDisplayChangedCallback");
        }

        [Test]
        public void NativeBinding_MLMediaCCParserParse_Exists()
        {
            AssertThatMethodExists("MLMediaCCParserParse");
        }
    }
}
```




