---	
description: Learn how to debug WebView2 controls.	
title: Debugging WebView2	
author: MSEdgeTeam	
ms.author: msedgedevrel, jasteph	
ms.date: 07/15/2020	
ms.topic: how-to	
ms.prod: microsoft-edge	
ms.technology: webview	
keywords: IWebView2, IWebView2WebView, webview2, webview, win32 apps, win32, edge, ICoreWebView2, ICoreWebView2Host, browser control, edge html	
---	

# How to Debug with WebView2  	

The goal of the Microsoft Edge WebView2 control is combining the best of both the web and native application development features and developer tools.  The following page outlines the different tools to use when developing with WebView2 controls.  	

## Microsoft Edge DevTools  	

Use [Microsoft Edge (Chromium) Developer Tools][DevtoolsMain] to debug web content displayed in WebView2 controls, in the same way that you would if you were using Microsoft Edge.  To open the DevTools, set focus on the WebView window and then use any of the following actions.  	
*   Press `F12`.  	
*   Press `Ctrl`+`Shift`+`I`.  	
*   Open the context menu \(right-click\) > select `Inspect`.  	


> [!IMPORTANT]	
> When you debug your application in Visual Studio with the native debugger attached, pressing `F12` may trigger the native debugger instead of Developer Tools.  Use `Ctrl`+`Shift`+`I`, or use the context menu \(right-click\) to avoid the situation.  	
## Visual Studio  	
:::image type="complex" source="./media/f12.png" alt-text="Microsoft Edge DevTools" lightbox="../media/f12.png":::
   Microsoft Edge DevTools  
:::image-end:::  

You may use Visual Studio to develop application code and debug scripts at the same time.  	

Keep the following things in mind.  	

*   Visual Studio only supports debugging scripts when the app is launched from within Visual Studio.  \(Attaching a running process for debugging is not supported.\)  	
*   The targeted WebView debugging scenario is not supported.  	

> [!NOTE]
> This method of debugging is currently restricted to Win32 applications and Office add-ins.  

Use the script debugger in Visual Studio 2019 version 16.4 Preview 2 or later to debug your script in Visual Studio. 

To Begin:

*   Verify the **JavaScript diagnostics** component in **Desktop development with C++** workload is installed.  
    
    1.  Navigate to **Apps & features** settings in Windows.  Search for it using the Windows task bar.  
    1.  Choose **Modify**.  
    
            :::image type="complex" source="./media/appsandfeatures.png" alt-text="Apps & Features" lightbox="../media/appsandfeatures.png":::
           Apps & Features  
        :::image-end:::  

    1.  Verify the **Javascript diagnostics** and **Desktop Development in C++** checkboxes are selected.  
        

        
*   Enable WebView2 script debugging.  
    1.  Hover on your project, open the context menu \(right-click\), and select **Properties**.  
    1.  On **Configuration Properties**, select **Debugging**.  
    1.  On the **Debugger Type** property, search the the list of options, and select **JavaScript (WebView2)**.  
        
        :::image type="complex" source="./media/enbjs.png" alt-text="Visual Studio JavaScript Debugger" lightbox="/media/enbjs.png":::
           Visual Studio JavaScript Debugger  
        :::image-end:::  

You are ready to debug! Now you may:

1.  Set Breakpoints  
    *   Open the file you are trying to debug and set a breakpoint by clicking left on the line number.  
        
        :::image type="complex" source="./media/breakpoint.png" alt-text="Visual Studio Code Adding Breakpoints" lightbox="/media/breakpoint.png":::
           Visual Studio Adding Breakpoints  
        :::image-end::: 

        > [!NOTE]
        > The JS/TS debug adapter does not do source path mapping.  You must open the exact same path associated with your WebView2.  
        
1.  Run Code  
    *   Select the appropriate build flavor and runtime environment and then run the local windows debugger by clicking the green play button.

        :::image type="complex" source="./media/run.png" alt-text=" Visual Studio Code Run" lightbox="/media/run.png":::
            Visual Studio Code Run  
        :::image-end:::   
1.  View Debug Console  
    *   The application will run and the debugger will connect after the first webview2 is created.  Any pending debug output will be displayed.  

                :::image type="complex" source="./media/console.png" alt-text=" Visual Studio Code Debug Output" lightbox="/media/console.png":::
            Visual Studio Code Debug Output  
        :::image-end:::


        
## Visual Studio Code  
  
There are 5 basic steps for debugging within VSCode:

1. **Install the debug adapter**
    1. Navigate to VSCode's Extensions
    1. Search for JavaScript Debugger (shown in screenshot)
    1. Install any version of the adapter later than 2020.6.208
    
        :::image type="complex" source="./media/jsdebugger.png" alt-text=" Visual Studio Code Debug Output" lightbox="/media/jsdebugger.png":::
            Visual Studio Code Debug Output  
        :::image-end:::
    1. Navigate to VSCode's Settings Tab > Extensions > JavaScript Debugger
    1. Ensure the "Use the new in-preview JavaScript debugger for Node.js and Chrome." box is checked.
    
        :::image type="complex" source="./media/previewbox.png" alt-text=" Visual Studio Code Debug Output" lightbox="/media/previewbox.png":::
        Visual Studio Code Debug Output  
        :::image-end:::

1. **Configure the debug adapter**
    1. Select the Run tab on the left side of VS Code.
    
        :::image type="complex" source="./media/runbutton.png" alt-text=" Visual Studio Code Debug Output" lightbox="/media/runbutton.png":::
        Visual Studio Code Debug Output  
        :::image-end:::

    1. Within the dropdown menu select a project to run
    
        :::image type="complex" source="./media/scenario.png" alt-text=" Visual Studio Code Debug Output" lightbox="/media/scenario.png":::
        Visual Studio Code Debug Output  
        :::image-end:::

    1. Ensure you have a VSCode Launch.json file. If not create a Launch.json file with the following metadata: (This step is required to debug a WebView Control)
    
        ```csharp
        {
            "name": "Scenario 1: Script debugging (first) (old adapter)",  
            "type": "pwa-msedge”   
            "port": 9222, // Optional defaults to 9222  
            "request": "launch", // optionally “attach”  
            "runtimeExecutable": "E:/YourPath/YourApplication.exe",  
            "env": {  
                    // customize for your build location if needed  
                    "Path": "%path%;e:/YourNeededPath; "  
            }  
            "useWebView": true, // optionally “advanced”  
            //"urlFilter": "*DebugTrigger", // Only used when useWebView == “Advanced”  
        } 
        ```  

1. **Set breakpoints**
    1. Open the file you want to debug in VSCode.  And then set the breakpoint (select the line and press F9, or click the breakpoint column in the editor).  

        :::image type="complex" source="./media/breakpointvs.png" alt-text=" Visual Studio Code Debug Output" lightbox="/media/breakpointvs.png":::
        Visual Studio Code Debug Output  
        :::image-end:::

    > [!NOTE]
        >  VSCode does not do source mapping you so MUST ensure you have opened and set breakpoints in the same file path as the WebView will be using.  If the paths are not exact VSCode can’t resolve the breakpoint. 
1. **Run code**
    1. Select the launch configuration from the dropdown menu.
    1. Start debugging by clicking the green run button.        

        :::image type="complex" source="./media/runvs.png" alt-text=" Visual Studio Code Debug Output" lightbox="/media/runvs.png":::
        Visual Studio Code Debug Output  
        :::image-end:::

1. **View Results**

        :::image type="complex" source="./media/resultsvs.png" alt-text=" Visual Studio Code Debug Output" lightbox="/media/resultsvs.png":::
        Visual Studio Code Debug Output  
        :::image-end:::

> [!NOTE]
> If your application has more than one WebView hosted within it, connecting to the first created might not be what you need. To solve this, there is an "Advanced" mode that lets you select a targeted WebView to connect to.

To turn on Advanced mode:
* Change the useWebview configuration parameter to “Advanced” and add the urlFilter parameter. The value of the urlFilter is used as a string comparison parameter on the URL that each WebView is navigated to.  

Congratulations, you have completed this guide.

For a list of other JavaScript Debugging features in Visual Studio Code, visit the [Visual Studio JS Adapter](https://github.com/microsoft/vscode-js-debug/#whats-new) Github Page.

## Getting in touch with the Microsoft Edge WebView team  	

Help build a richer WebView2 experience by sharing your feedback.  Visit the [feedback repo][GithubMicrosoftedgeWebviewfeedback] to submit feature requests or bug reports.  	

<!-- links -->  	

[DevtoolsMain]: /microsoft-edge/devtools-guide-chromium "Microsoft Edge (Chromium) Developer Tools | Microsoft Docs"  	

[GithubMicrosoftVscodeEdgeDebug2MainChromiumWebviewApplications]: https://github.com/microsoft/vscode-edge-debug2/blob/master/README.md#microsoft-edge-chromium-webview-applications "Microsoft Edge (Chromium) WebView applications - VS Code - Debugger for Microsoft Edge | GitHub"  	

[GithubMicrosoftedgeWebviewfeedback]: https://github.com/MicrosoftEdge/WebViewFeedback "WebView Feedback - MicrosoftEdge/WebViewFeedback | GitHub"  