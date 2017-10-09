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
# <a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>A Helykapcsolathíd natív Mobile Engagement Android SDK az Android webes nézet
> [!div class="op_single_selector"]
> * [Android híd](mobile-engagement-bridge-webview-native-android.md)
> * [iOS híd](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

Egyes mobil alkalmazások úgy lettek kialakítva, ahol maga hello alkalmazás fejlesztett natív Android-fejlesztések, de néhány vagy akár az összes hello képernyők jelennek meg, egy Android webes nézet belül hibrid alkalmazásként. Ilyen alkalmazásokon belül továbbra is használhatja a Mobile Engagement Android SDK-t, és ez az oktatóanyag leírja, hogyan toogo ezzel kapcsolatban. hello mintakódot az alábbi hello Android dokumentáció alapján [Itt](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Azt ismerteti, hogyan a dokumentált módszer használható tooimplement hello ugyanaz a Mobile Engagement Android SDK általánosan használt módszerek, úgy, hogy a webes nézet egy hibrid alkalmazásból is kezdeményezhet kérelmek tootrack események, feladatok, hibák, app-info közben ismertetett őket keresztül az Android SDK-t. 

1. Első lépésként szüksége, amely keresztül megtettünk mindent tooensure a [bevezető oktatóanyag](mobile-engagement-android-get-started.md) toointegrate hello Mobile Engagement Android SDK a hibrid alkalmazásban. Ezután, a `OnCreate` metódus hello hasonlóan fog kinézni.  
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }
2. Most győződjön meg arról, hogy rendelkezik-e a hibrid app egy webes nézet egy képernyőt rajta. hello kód hozzá lesz hasonló toohello következő ahol betöltése azt egy helyi HTML-fájlba **Sample.html** a webes nézet hello a hello `onCreate` módszer a képernyőn. 
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }
   
        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }
3. Most létrehoz egy híd nevű fájlt **WebAppInterface** amely egy burkolót hoz keresztül néhány gyakran használt Mobile Engagement Android SDK módszerek használatával hello `@JavascriptInterface` hello megközelítésnek [Android dokumentáció ](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):
   
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
4. Miután létrehoztunk hello híd fájl újabb, igazolnia kell, hogy társítva a webes nézet tooensure. A toohappen tooedit kell a `SetWebview` metódus úgy, hogy az informatikai következőhöz hasonló hello:
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }
5. A fenti részlet hello, hívtuk `addJavascriptInterface` tooassociate a híd a webes nézet az osztály és is létrejön, úgynevezett leírót **EngagementJs** toocall hello módszerek hello híd fájlból. 
6. Most létrehozza a következő nevű fájl hello **Sample.html** a projekt egy mappát a nevű **eszközök** hello webes nézet betöltődik, és ahol fog nevezzük hello módszerek hello híd fájlból.
   
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
7. Megjegyzés: hello következő fenti hello HTML-fájllal kapcsolatos mutat:
   
   * A beviteli mezőbe, ahol megadhatja a esemény, a feladat, a hiba, a AppInfo névként használt adatok toobe készletét tartalmazza. Amikor hello gomb következő tooit kattint, a rendszer telefonhívást indít toohello Javascript, ami végül hello módszerek hívásait hello híd fájl toopass a hívás toohello Mobile Engagement Android SDK-t. 
   * Azt is címkézés néhány statikus további információ toohello események, feladatok és hibák toodemonstrate még hogyan ezt megteheti. A felesleges adatokat küldött, egy JSON karakterlánc, amely hello jelenik meg `WebAppInterface` fájl elemzése, amely egy Android be `Bundle` és közlekednek az események, feladatok, hibák küldése mellett. 
   * A Mobile Engagement feladat hello beviteli mezőbe, adjon meg 10 másodperces futtatása, és állítsa le hello nevű kezdődött el. 
   * A Mobile Engagement appinfo vagy címke van át a "customer_name" hello statikus adatcserekulcsokat és hello megadott hello bemeneti hello értékként hello címkével. 
8. Hello következő futtatási hello alkalmazást, és akkor jelenik meg. Most adja meg például a következő hello vizsgálati esemény néhány nevét, majd kattintson **küldése** alá. 
   
    ![][1]
9. Jelenleg Ha toohello **figyelő** fülre az alkalmazás, és kattintson duplán a **események -> részletek**, láthatja, hogy ez az esemény hello statikus app-info azt küldő együtt jelenik meg. 
   
   ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
