---

title: MLWebViewTabBehavior.cs

---


# MLWebViewTabBehavior.cs









## Source code

```csharp
using System;
using System.Runtime.InteropServices;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.XR.MagicLeap;
using UnityEngine.XR.MagicLeap.Native;

namespace MagicLeap.Core
{
    [RequireComponent(typeof(Toggle))]
    public class MLWebViewTabBehavior : MonoBehaviour
    {
        public MLWebView WebView
        {
            get; private set;
        }
        
        public event Action<MLWebViewTabBehavior> OnTabSelected;

        private MLWebViewTabBarBehavior tabBar;
        private MLWebViewScreenBehavior webViewScreen;
        private InputField addressBar;

        private Toggle toggle;
        private Text text;

        private bool loadOnServiceConnected = false;
        private bool isPaused;

        public string tabUrl
        {
            get; private set;
        }

        public bool IsPaused => isPaused;

        void Awake()
        {
            toggle = GetComponent<Toggle>();
            toggle.onValueChanged.AddListener(OnValueChanged);

            text = GetComponentInChildren<Text>();
        }

        void OnDestroy()
        {
            DestroyTab();
        }

        public bool CreateTab(MLWebViewTabBarBehavior tabBar, MLWebViewScreenBehavior webViewScreen, InputField addressBar)
        {
            if (!MLPermissions.CheckPermission(MLPermission.WebView).IsOk)
            {
                Debug.LogError($"You must include {MLPermission.WebView} in AndroidManifest.xml to run this example");
                Destroy(this);
                return false;
            }

            this.tabBar = tabBar;
            this.webViewScreen = webViewScreen;
            this.addressBar = addressBar;
            webViewScreen.GetWebViewSize(out uint width, out uint height);

            WebView = MLWebView.Create(width, height);
            if (WebView == null)
            {
                Debug.LogError("Failed to create new web view tab");
                Destroy(this);
                return false;
            }

            WebView.OnLoadEnded += OnLoadEnded;
            WebView.OnServiceConnected += OnServiceConnected;
            return true;
        }

        public void SelectTab()
        {
            if (webViewScreen != null)
            {
                webViewScreen.WebView = WebView;
            }
            if (tabBar != null)
            {
                tabBar.SelectTab(this);
            }
            toggle.isOn = true;
        }

        public void UnselectTab()
        {
            toggle.isOn = false;
        }

        private void OnValueChanged(bool selected)
        {
            if (selected)
            {
                SelectTab();
            }
        }

        public void DestroyTab()
        {
            if (webViewScreen != null)
            {
                if (webViewScreen.WebView == WebView)
                {
                    webViewScreen.WebView = null;
                }
            }

            if (WebView != null)
            {
                WebView.OnLoadEnded -= OnLoadEnded;
                WebView.OnServiceConnected -= OnServiceConnected;

                // manually call ServiceDisconnected because webview could be garbage collected
                // prior to the callback coming back from platform
                webViewScreen.ServiceDisconnected();

                if (!WebView.Destroy().IsOk)
                {
                    Debug.LogError("Failed to destroy webview tab " + WebView.WebViewHandle);
                }
                else
                {
                    WebView = null;
                    Destroy(gameObject);
                }
            }
        }

        public void GoToUrl(string url)
        {
            if (WebView != null)
            {
                if (webViewScreen.IsConnected)
                {
                    if (!WebView.GoTo(url).IsOk)
                    {
                        Debug.LogError("Failed to navigate to url " + url);
                    }
                    else
                    {
                        tabUrl = url;
                    }
                }
                else
                {
                    tabUrl = url;
                    loadOnServiceConnected = true;
                }

            }
        }

        public void UpdateTabLabel()
        {
            if (text != null && WebView != null)
            {
                tabUrl = WebView.GetURL();
                System.Uri uri = new System.Uri(tabUrl);
                text.text = uri.Host;
            }
        }

        private void OnServiceConnected(MLWebView webView)
        {
            if (webViewScreen != null)
            {
                webViewScreen.ServiceConnected();
            }

            if (loadOnServiceConnected)
            {
                loadOnServiceConnected = false;
                if (!String.IsNullOrEmpty(tabUrl))
                {
                    if (!WebView.GoTo(tabUrl).IsOk)
                    {
                        Debug.LogError("Failed to navigate to url " + tabUrl);
                    }
                }
            }
        }

        private void OnLoadEnded(MLWebView webView, bool isMainFrame, int httpStatusCode)
        {
            UpdateTabLabel();

            if (WebView != null && webViewScreen.WebView == WebView)
            {
                if (addressBar != null)
                {
                    addressBar.text = WebView.GetURL();
                }
            }
        }

        public void Pause()
        {
            isPaused = true;
        }
        
        public void Resume()
        {
            isPaused = false;
        }
    }
}
```




