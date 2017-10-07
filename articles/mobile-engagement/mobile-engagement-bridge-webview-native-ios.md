---
title: "aaaBridge iOS webes nézet natív a Mobile Engagement IOS SDK"
description: "Ismerteti, hogyan toocreate hidat között, Javascript és hello natív a Mobile Engagement iOS SDK fut webes nézet"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: e1d6ff6f-cd67-4131-96eb-c3d6318de752
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 089ed8484722cb5ba624e5dce0e670ab56de514d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a><span data-ttu-id="bb288-103">A Helykapcsolathíd iOS webes nézet natív a Mobile Engagement IOS SDK</span><span class="sxs-lookup"><span data-stu-id="bb288-103">Bridge iOS WebView with native Mobile Engagement iOS SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bb288-104">Android híd</span><span class="sxs-lookup"><span data-stu-id="bb288-104">Android Bridge</span></span>](mobile-engagement-bridge-webview-native-android.md)
> * [<span data-ttu-id="bb288-105">iOS híd</span><span class="sxs-lookup"><span data-stu-id="bb288-105">iOS Bridge</span></span>](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

<span data-ttu-id="bb288-106">Egyes mobil alkalmazások úgy lettek kialakítva, ahol maga hello alkalmazás fejlesztett Objective-C natív iOS-fejlesztések, de néhány vagy akár az összes hello képernyők megjelennek-e egy webes nézet iOS belül hibrid alkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="bb288-106">Some mobile apps are designed as a hybrid app where hello app itself is developed using native iOS Objective-C development but some or even all of hello screens are rendered within an iOS WebView.</span></span> <span data-ttu-id="bb288-107">Ön továbbra is használhatja a Mobile Engagement iOS SDK ilyen alkalmazásokon belül, és ez az oktatóanyag leírja, hogyan toogo ezzel kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bb288-107">You can still consume Mobile Engagement iOS SDK within such apps and this tutorial describes how toogo about doing this.</span></span> 

<span data-ttu-id="bb288-108">Nincsenek két megközelítés tooachieve Ez abban az esetben, ha mindkettő nem dokumentált:</span><span class="sxs-lookup"><span data-stu-id="bb288-108">There are two approaches tooachieve this though both are undocumented:</span></span>

* <span data-ttu-id="bb288-109">Először egy leírását itt [hivatkozás](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) amely magában foglalja a regisztrálása a `UIWebViewDelegate` a webes nézet és a catch-és-azonnal-Mégse gombra a hely módosítása Javascript végezheti el.</span><span class="sxs-lookup"><span data-stu-id="bb288-109">First one is described on this [link](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) which involves registering a `UIWebViewDelegate` on your web view and catch-and-immediatly-cancel a location change done in Javascript.</span></span> 
* <span data-ttu-id="bb288-110">Második egy alapján ez [WWDC 2013 munkamenet](https://developer.apple.com/videos/play/wwdc2013/615), mint hello tisztító először, és amely azt követi a jelen útmutató egy módszert.</span><span class="sxs-lookup"><span data-stu-id="bb288-110">Second one is based on this [WWDC 2013 session](https://developer.apple.com/videos/play/wwdc2013/615), an approach which is cleaner than hello first and which we will follow for this guide.</span></span> <span data-ttu-id="bb288-111">Vegye figyelembe, hogy ez a módszer csak akkor működik ios7-en és újabb verziók.</span><span class="sxs-lookup"><span data-stu-id="bb288-111">Note that this approach only works on iOS7 and above.</span></span> 

<span data-ttu-id="bb288-112">A hello iOS híd minta, kövesse a hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bb288-112">Follow hello steps below for hello iOS bridge sample:</span></span>

1. <span data-ttu-id="bb288-113">Első lépésként szüksége, amely keresztül megtettünk mindent tooensure a [bevezető oktatóanyag](mobile-engagement-ios-get-started.md) toointegrate hello a Mobile Engagement iOS SDK a hibrid alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bb288-113">First of all, you need tooensure that you have gone through our [Getting Started tutorial](mobile-engagement-ios-get-started.md) toointegrate hello Mobile Engagement iOS SDK in your hybrid app.</span></span> <span data-ttu-id="bb288-114">Szükség esetén is engedélyezheti, hogy láthassa hello SDK módszerek, mivel azt elindítani őket a hello webes nézet teszt naplózás az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="bb288-114">Optionally, you can also enable test logging as follows so that you can see hello SDK methods as we trigger them from hello webview.</span></span> 
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
             [EngagementAgent setTestLogEnabled:YES];
           ....
        }
2. <span data-ttu-id="bb288-115">Most győződjön meg arról, hogy rendelkezik-e a hibrid app egy webes nézet egy képernyőt rajta.</span><span class="sxs-lookup"><span data-stu-id="bb288-115">Now make sure that your hybrid app has a screen with a webview on it.</span></span> <span data-ttu-id="bb288-116">Toohello adhat hozzá `Main.storyboard` hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bb288-116">You can add it toohello `Main.storyboard` of hello app.</span></span> 
3. <span data-ttu-id="bb288-117">Társítsa ezt a webes nézet a **ViewController** kattintással és húzással hello webes nézet a hello View vezérlő helyszín toohello `ViewController.h` képernyőn helyezés hello alatt szerkesztése `@interface` sor.</span><span class="sxs-lookup"><span data-stu-id="bb288-117">Associate this webview with your **ViewController** by clicking and dragging hello webview from hello View Controller Scene toohello `ViewController.h` edit screen, placing it just below hello `@interface` line.</span></span> 
4. <span data-ttu-id="bb288-118">Ezután a, egy párbeszédpanel jelenik kérő nevét.</span><span class="sxs-lookup"><span data-stu-id="bb288-118">Once you do this, a dialog box will pop up asking for a name.</span></span> <span data-ttu-id="bb288-119">Adja meg a hello néven **webes nézet**.</span><span class="sxs-lookup"><span data-stu-id="bb288-119">Provide hello name as **webView**.</span></span> <span data-ttu-id="bb288-120">A `ViewController.h` fájl hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="bb288-120">Your `ViewController.h` file should look like hello following:</span></span>
   
        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
   
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
   
        @end
5. <span data-ttu-id="bb288-121">Frissítjük a hello `ViewController.m` később fájlt, de először létrehozunk hello híd fájlt, amely egy burkolót hoz keresztül néhány gyakran használt Mobile Engagement iOS SDK módszerek.</span><span class="sxs-lookup"><span data-stu-id="bb288-121">We will update hello `ViewController.m` file later but first we will create hello bridge file which creates a wrapper over some commonly used Mobile Engagement iOS SDK methods.</span></span> <span data-ttu-id="bb288-122">Hozzon létre egy új fejlécfájlt nevű **EngagementJsExports.h** hello használó `JSExport` hello fent ismertetett mechanizmus [munkamenet](https://developer.apple.com/videos/play/wwdc2013/615) tooexpose hello natív iOS módszerek.</span><span class="sxs-lookup"><span data-stu-id="bb288-122">Create a new header file called **EngagementJsExports.h** which uses hello `JSExport` mechanism described in hello aforementioned [session](https://developer.apple.com/videos/play/wwdc2013/615) tooexpose hello native iOS methods.</span></span> 
   
        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
   
        @protocol EngagementJsExports <JSExport>
   
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
   
        @end
   
        @interface EngagementJs : NSObject <EngagementJsExports>
   
        @end
6. <span data-ttu-id="bb288-123">Ezután létrehozunk hello hello híd fájl második részét.</span><span class="sxs-lookup"><span data-stu-id="bb288-123">Next we will create hello second part of hello bridge file.</span></span> <span data-ttu-id="bb288-124">Hozzon létre egy nevű fájlt **EngagementJsExports.m** fog tartalmazó hello megvalósítása által hello a Mobile Engagement iOS SDK metódusok meghívása tényleges burkolók hello létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bb288-124">Create a file called **EngagementJsExports.m** which will contain hello implementation creating hello actual wrappers by calling hello Mobile Engagement iOS SDK methods.</span></span> <span data-ttu-id="bb288-125">Is vegye figyelembe, hogy azt a rendszer elemzése hello `extras` hello webes nézet JavaScript átadott, és azokat, amelyek üzembe egy `NSMutableDictionary` hello Engagement SDK-metódussal hívások objektum toobe átadni.</span><span class="sxs-lookup"><span data-stu-id="bb288-125">Also note that we are parsing hello `extras` being passed from hello webview javascript and putting that into an `NSMutableDictionary` object toobe passed with hello Engagement SDK method calls.</span></span>  
   
        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
   
        @implementation EngagementJs
   
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
   
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
   
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
   
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
   
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
   
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
   
           return extras;
        }
   
        @end
7. <span data-ttu-id="bb288-126">Most azt visszatérhet toohello **ViewController.m** és hello kód a következő frissítést:</span><span class="sxs-lookup"><span data-stu-id="bb288-126">Now we come back toohello **ViewController.m** and update it with hello following code:</span></span> 
   
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
   
        @interface ViewController ()
   
        @end
   
        @implementation ViewController
   
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
   
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
   
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
   
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
   
           context[@"EngagementJs"] = [EngagementJs class];
        }
   
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
   
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
   
        @end
8. <span data-ttu-id="bb288-127">Megjegyzés: hello következő mutat kapcsolatos hello **ViewController.m** fájlt:</span><span class="sxs-lookup"><span data-stu-id="bb288-127">Note hello following points about hello **ViewController.m** file:</span></span>
   
   * <span data-ttu-id="bb288-128">A hello `loadWebView` módszer, azt letöltése folyamatban van egy helyi HTML-fájl neve **LocalPage.html** azt a következő felülvizsgálja azonosítójú.</span><span class="sxs-lookup"><span data-stu-id="bb288-128">In hello `loadWebView` method, we are loading a local HTML file called **LocalPage.html** whose code we will review next.</span></span> 
   * <span data-ttu-id="bb288-129">A hello `webViewDidFinishLoad` módszer, azt is grabbing hello `JsContext` és a burkoló osztályt társít.</span><span class="sxs-lookup"><span data-stu-id="bb288-129">In hello `webViewDidFinishLoad` method, we are grabbing hello `JsContext` and associating our wrapper class with it.</span></span> <span data-ttu-id="bb288-130">Ez lehetővé teszi a a burkoló hello leíró használatával SDK metódusok meghívása **EngagementJs** a hello webes nézet.</span><span class="sxs-lookup"><span data-stu-id="bb288-130">This will allow calling our wrapper SDK methods using hello handle **EngagementJs** from hello webView.</span></span> 
9. <span data-ttu-id="bb288-131">Hozzon létre egy nevű fájlt **LocalPage.html** a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="bb288-131">Create a file called **LocalPage.html** with hello following code:</span></span>
   
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
                   alert('window.onerror: ' + err);
               }
   
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with hello Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
   
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
   
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
   
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
   
           </head>
           <body>
               <h1>Bridge Tester</h1>
   
               <div id='engagement'>
   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
   
               </div>
           </body>
        </html>
10. <span data-ttu-id="bb288-132">Megjegyzés: hello következő fenti hello HTML-fájllal kapcsolatos mutat:</span><span class="sxs-lookup"><span data-stu-id="bb288-132">Note hello following points about hello HTML file above:</span></span>
    
    * <span data-ttu-id="bb288-133">A beviteli mezőbe, ahol megadhatja a esemény, a feladat, a hiba, a AppInfo névként használt adatok toobe készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bb288-133">It contains a set of input boxes where you can provide data toobe used as names for your Event, Job, Error, AppInfo.</span></span> <span data-ttu-id="bb288-134">Amikor hello gomb következő tooit kattint, a rendszer telefonhívást indít toohello Javascript, ami végül hello módszerek hívásait hello híd fájl toopass a hívás toohello a Mobile Engagement iOS SDK.</span><span class="sxs-lookup"><span data-stu-id="bb288-134">When you click on hello button next tooit, a call is made toohello Javascript which eventually calls hello methods from hello bridge file toopass this call toohello Mobile Engagement iOS SDK.</span></span> 
    * <span data-ttu-id="bb288-135">Azt is címkézés néhány statikus további információ toohello események, feladatok és hibák toodemonstrate még hogyan ezt megteheti.</span><span class="sxs-lookup"><span data-stu-id="bb288-135">We are tagging on some static extra info toohello events, jobs and even errors toodemonstrate how this could be done.</span></span> <span data-ttu-id="bb288-136">A felesleges adatokat küldött, egy JSON karakterlánc, amely hello jelenik meg `EngagementJsExports.m` fájl elemzése, amely átadott események, feladatok, hibák küldése mellett.</span><span class="sxs-lookup"><span data-stu-id="bb288-136">This extra info is sent as a JSON string which, if you look in hello `EngagementJsExports.m` file, is parsed and passed along with sending Events, Jobs, Errors.</span></span> 
    * <span data-ttu-id="bb288-137">A Mobile Engagement feladat hello beviteli mezőbe, adjon meg 10 másodperces futtatása, és állítsa le hello nevű kezdődött el.</span><span class="sxs-lookup"><span data-stu-id="bb288-137">A Mobile Engagement Job is kicked off with hello name you specify in hello input box, run for 10 seconds and shut down.</span></span> 
    * <span data-ttu-id="bb288-138">A Mobile Engagement appinfo vagy címke van át a "customer_name" hello statikus adatcserekulcsokat és hello megadott hello bemeneti hello értékként hello címkével.</span><span class="sxs-lookup"><span data-stu-id="bb288-138">A Mobile Engagement appinfo or tag is passed with 'customer_name' as hello static key and hello value that you entered in hello input as hello value for hello tag.</span></span> 
11. <span data-ttu-id="bb288-139">Hello következő futtatási hello alkalmazást, és akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bb288-139">Run hello app and you will see hello following.</span></span> <span data-ttu-id="bb288-140">Most adja meg például a következő hello vizsgálati esemény néhány nevét, majd kattintson **küldése** következő tooit.</span><span class="sxs-lookup"><span data-stu-id="bb288-140">Now provide some name for a test event like hello following and click **Send** next tooit.</span></span> 
    
     ![][1]
12. <span data-ttu-id="bb288-141">Jelenleg Ha toohello **figyelő** fülre az alkalmazás, és kattintson duplán a **események -> részletek**, láthatja, hogy ez az esemény hello statikus app-info azt küldő együtt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bb288-141">Now if you go toohello **Monitor** tab of your app and look under **Events -> Details**, you will see this event show up along with hello static app-info that we are sending.</span></span> 
    
    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
