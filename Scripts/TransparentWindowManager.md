```csharp
using System;
using System.Runtime.InteropServices;
using UnityEngine;

public class TransparentWindowManager : SingletonMonoBehaviour<TransparentWindowManager>
{
    #region Enum

    internal enum WindowCompositionAttribute
    {
        WCA_ACCENT_POLICY = 19
    }

    internal enum AccentState
    {
        ACCENT_DISABLED = 0,
        ACCENT_ENABLE_GRADIENT = 1,
        ACCENT_ENABLE_TRANSPARENTGRADIENT = 2,
        ACCENT_ENABLE_BLURBEHIND = 3,
        ACCENT_INVALID_STATE = 4
    }

    #endregion Enum

    #region Struct

    private struct MARGINS
    {
        public int cxLeftWidth;
        public int cxRightWidth;
        public int cyTopHeight;
        public int cyBottomHeight;
    }

    internal struct WindowCompositionAttributeData
    {
        public WindowCompositionAttribute Attribute;
        public IntPtr Data;
        public int SizeOfData;
    }

    internal struct AccentPolicy
    {
        public AccentState AccentState;
        public int AccentFlags;
        public int GradientColor;
        public int AnimationId;
    }

    #endregion Struct

    #region DLL Import

    [DllImport("user32.dll")]
    private static extern IntPtr GetActiveWindow();

    [DllImport("user32.dll")]
    private static extern int SetWindowLong(IntPtr hWnd, int nIndex, uint dwNewLong);

    [DllImport("Dwmapi.dll")]
    private static extern uint DwmExtendFrameIntoClientArea(IntPtr hWnd, ref MARGINS margins);

    #endregion DLL Import

    #region Method

    // CAUTION:
    // To control enable or disable, use Start method instead of Awake.
    protected virtual void Start()
    {
#if !UNITY_EDITOR && UNITY_STANDALONE_WIN

        const int GWL_STYLE = -16;
        const uint WS_POPUP = 0x80000000;
        const uint WS_VISIBLE = 0x10000000;

        // Get the current window handle
        var windowHandle = GetActiveWindow();

        // Set window style (popup + visible)
        SetWindowLong(windowHandle, GWL_STYLE, WS_POPUP | WS_VISIBLE);

        // Extend the frame into the client area
        MARGINS margins = new MARGINS()
        {
            cxLeftWidth = -1,
            cxRightWidth = -1,
            cyTopHeight = -1,
            cyBottomHeight = -1
        };

        DwmExtendFrameIntoClientArea(windowHandle, ref margins);

#endif
    }

    #endregion Method
}
```
