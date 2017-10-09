---
title: "a natív Mobile Engagement Android SDK Android webes nézet aaaBridge"
description: "Ismerteti, hogyan toocreate hidat webes nézet között Javascript fut, valamint a natív Mobile Engagement Android SDK hello"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: cf272f3f-2b09-41b1-b190-944cdca8bba2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: a7a09bcc156490fe69ad29a67809745dcfc22da6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a><span data-ttu-id="b3782-103">A Helykapcsolathíd natív Mobile Engagement Android SDK az Android webes nézet</span><span class="sxs-lookup"><span data-stu-id="b3782-103">Bridge Android WebView with native Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b3782-104">Android híd</span><span class="sxs-lookup"><span data-stu-id="b3782-104">Android Bridge</span></span>](mobile-engagement-bridge-webview-native-android.md)
> * [<span data-ttu-id="b3782-105">iOS híd</span><span class="sxs-lookup"><span data-stu-id="b3782-105">iOS Bridge</span></span>](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

<span data-ttu-id="b3782-106">Egyes mobil alkalmazások úgy lettek kialakítva, ahol maga hello alkalmazás fejlesztett natív Android-fejlesztések, de néhány vagy akár az összes hello képernyők jelennek meg, egy Android webes nézet belül hibrid alkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="b3782-106">Some mobile apps are designed as a hybrid app where hello app itself is developed using native Android development but some or even all of hello screens are rendered within an Android WebView.</span></span> <span data-ttu-id="b3782-107">Ilyen alkalmazásokon belül továbbra is használhatja a Mobile Engagement Android SDK-t, és ez az oktatóanyag leírja, hogyan toogo ezzel kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b3782-107">You can still consume Mobile Engagement Android SDK within such apps and this tutorial describes how toogo about doing this.</span></span> <span data-ttu-id="b3782-108">hello mintakódot az alábbi hello Android dokumentáció alapján [Itt](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript).</span><span class="sxs-lookup"><span data-stu-id="b3782-108">hello sample code below is based on hello Android documentation [here](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript).</span></span> <span data-ttu-id="b3782-109">Azt ismerteti, hogyan a dokumentált módszer használható tooimplement hello ugyanaz a Mobile Engagement Android SDK általánosan használt módszerek, úgy, hogy a webes nézet egy hibrid alkalmazásból is kezdeményezhet kérelmek tootrack események, feladatok, hibák, app-info közben ismertetett őket keresztül az Android SDK-t.</span><span class="sxs-lookup"><span data-stu-id="b3782-109">It describes how this documented approach could be used tooimplement hello same for Mobile Engagement Android SDK's commonly used methods such that a Webview from a hybrid app can also initiate requests tootrack events, jobs, errors, app-info while piping them via our Android SDK.</span></span> 

1. <span data-ttu-id="b3782-110">Első lépésként szüksége, amely keresztül megtettünk mindent tooensure a [bevezető oktatóanyag](mobile-engagement-android-get-started.md) toointegrate hello Mobile Engagement Android SDK a hibrid alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b3782-110">First of all, you need tooensure that you have gone through our [Getting Started tutorial](mobile-engagement-android-get-started.md) toointegrate hello Mobile Engagement Android SDK in your hybrid app.</span></span> <span data-ttu-id="b3782-111">Ezután, a `OnCreate` metódus hello hasonlóan fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="b3782-111">Once you do that, your `OnCreate` method will look like hello following.</span></span>  
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }
2. <span data-ttu-id="b3782-112">Most győződjön meg arról, hogy rendelkezik-e a hibrid app egy webes nézet egy képernyőt rajta.</span><span class="sxs-lookup"><span data-stu-id="b3782-112">Now make sure that your hybrid app has a screen with a WebView on it.</span></span> <span data-ttu-id="b3782-113">hello kód hozzá lesz hasonló toohello következő ahol betöltése azt egy helyi HTML-fájlba **Sample.html** a webes nézet hello a hello `onCreate` módszer a képernyőn.</span><span class="sxs-lookup"><span data-stu-id="b3782-113">hello code for it will be similar toohello following where we are loading a local HTML file **Sample.html** in hello Webview in hello `onCreate` method of your screen.</span></span> 
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }
   
        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }
3. <span data-ttu-id="b3782-114">Most létrehoz egy híd nevű fájlt **WebAppInterface** amely egy burkolót hoz keresztül néhány gyakran használt Mobile Engagement Android SDK módszerek használatával hello `@JavascriptInterface` hello megközelítésnek [Android dokumentáció ](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):</span><span class="sxs-lookup"><span data-stu-id="b3782-114">Now create a bridge file called **WebAppInterface** which creates a wrapper over some commonly used Mobile Engagement Android SDK methods using hello `@JavascriptInterface` approach described in hello [Android documentation](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):</span></span>
   
        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
   
        import com.microsoft.azure.engagement.EngagementAgent;
   
        import org.json.JSONArray;
        import org.json.JSONObject;
   
        public class WebAppInterface {
            Context mContext;
   
            /** Instantiate hello interface and set hello context */
            WebAppInterface(Context c) {
                mContext = c;
            }
   
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
   
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
   
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
   
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  
4. <span data-ttu-id="b3782-115">Miután létrehoztunk hello híd fájl újabb, igazolnia kell, hogy társítva a webes nézet tooensure.</span><span class="sxs-lookup"><span data-stu-id="b3782-115">Once we have created hello above bridge file, we need tooensure that it is associated with our Webview.</span></span> <span data-ttu-id="b3782-116">A toohappen tooedit kell a `SetWebview` metódus úgy, hogy az informatikai következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="b3782-116">For this toohappen, you need tooedit your `SetWebview` method so that it looks like hello following:</span></span>
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }
5. <span data-ttu-id="b3782-117">A fenti részlet hello, hívtuk `addJavascriptInterface` tooassociate a híd a webes nézet az osztály és is létrejön, úgynevezett leírót **EngagementJs** toocall hello módszerek hello híd fájlból.</span><span class="sxs-lookup"><span data-stu-id="b3782-117">In hello above snippet, we called `addJavascriptInterface` tooassociate our bridge class with our Webview and also created a handle called **EngagementJs** toocall hello methods from hello bridge file.</span></span> 
6. <span data-ttu-id="b3782-118">Most létrehozza a következő nevű fájl hello **Sample.html** a projekt egy mappát a nevű **eszközök** hello webes nézet betöltődik, és ahol fog nevezzük hello módszerek hello híd fájlból.</span><span class="sxs-lookup"><span data-stu-id="b3782-118">Now create hello following file called **Sample.html** in your project in a folder called **assets** which is loaded into hello Webview and where we will call hello methods from hello bridge file.</span></span>
   
        <!doctype html>
        <html>
            <head>
                <style type='text/css'>
                    html { font-family:Helvetica; color:#222; }
                    h1 { color:steelblue; font-size:22px; margin-top:16px; }
                    h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
                </style>
   
                <script type="text/javascript">
   
                    window.onerror = function(err)
                    {
                      log('window.onerror: ' + err);
                    }
   
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with hello Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
   
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
   
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
   
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
   
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
   
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
   
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
   
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>
7. <span data-ttu-id="b3782-119">Megjegyzés: hello következő fenti hello HTML-fájllal kapcsolatos mutat:</span><span class="sxs-lookup"><span data-stu-id="b3782-119">Note hello following points about hello HTML file above:</span></span>
   
   * <span data-ttu-id="b3782-120">A beviteli mezőbe, ahol megadhatja a esemény, a feladat, a hiba, a AppInfo névként használt adatok toobe készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b3782-120">It contains a set of input boxes where you can provide data toobe used as names for your Event, Job, Error, AppInfo.</span></span> <span data-ttu-id="b3782-121">Amikor hello gomb következő tooit kattint, a rendszer telefonhívást indít toohello Javascript, ami végül hello módszerek hívásait hello híd fájl toopass a hívás toohello Mobile Engagement Android SDK-t.</span><span class="sxs-lookup"><span data-stu-id="b3782-121">When you click on hello button next tooit, a call is made toohello Javascript which eventually calls hello methods from hello bridge file toopass this call toohello Mobile Engagement Android SDK.</span></span> 
   * <span data-ttu-id="b3782-122">Azt is címkézés néhány statikus további információ toohello események, feladatok és hibák toodemonstrate még hogyan ezt megteheti.</span><span class="sxs-lookup"><span data-stu-id="b3782-122">We are tagging on some static extra info toohello events, jobs and even errors toodemonstrate how this could be done.</span></span> <span data-ttu-id="b3782-123">A felesleges adatokat küldött, egy JSON karakterlánc, amely hello jelenik meg `WebAppInterface` fájl elemzése, amely egy Android be `Bundle` és közlekednek az események, feladatok, hibák küldése mellett.</span><span class="sxs-lookup"><span data-stu-id="b3782-123">This extra info is sent as a JSON string which, if you look in hello `WebAppInterface` file, is parsed and put in an Android `Bundle` and is passed along with sending Events, Jobs, Errors.</span></span> 
   * <span data-ttu-id="b3782-124">A Mobile Engagement feladat hello beviteli mezőbe, adjon meg 10 másodperces futtatása, és állítsa le hello nevű kezdődött el.</span><span class="sxs-lookup"><span data-stu-id="b3782-124">A Mobile Engagement Job is kicked off with hello name you specify in hello input box, run for 10 seconds and shut down.</span></span> 
   * <span data-ttu-id="b3782-125">A Mobile Engagement appinfo vagy címke van át a "customer_name" hello statikus adatcserekulcsokat és hello megadott hello bemeneti hello értékként hello címkével.</span><span class="sxs-lookup"><span data-stu-id="b3782-125">A Mobile Engagement appinfo or tag is passed with 'customer_name' as hello static key and hello value that you entered in hello input as hello value for hello tag.</span></span> 
8. <span data-ttu-id="b3782-126">Hello következő futtatási hello alkalmazást, és akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b3782-126">Run hello app and you will see hello following.</span></span> <span data-ttu-id="b3782-127">Most adja meg például a következő hello vizsgálati esemény néhány nevét, majd kattintson **küldése** alá.</span><span class="sxs-lookup"><span data-stu-id="b3782-127">Now provide some name for a test event like hello following and click **Send** below it.</span></span> 
   
    ![][1]
9. <span data-ttu-id="b3782-128">Jelenleg Ha toohello **figyelő** fülre az alkalmazás, és kattintson duplán a **események -> részletek**, láthatja, hogy ez az esemény hello statikus app-info azt küldő együtt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b3782-128">Now if you go toohello **Monitor** tab of your app and look under **Events -> Details**, you will see this event show up along with hello static app-info that we are sending.</span></span> 
   
   ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
