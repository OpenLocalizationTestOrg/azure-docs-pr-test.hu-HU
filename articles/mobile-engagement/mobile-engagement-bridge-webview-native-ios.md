---
title: "A Helykapcsolathíd iOS webes nézet natív a Mobile Engagement IOS SDK"
description: "Hidat között, Javascript és a natív a Mobile Engagement iOS SDK szolgáltatást futtató webes nézet létrehozása"
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
ms.openlocfilehash: 35f7bdbeb480122513ae2a0b04a6d8cfd426802a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a><span data-ttu-id="0045b-103">A Helykapcsolathíd iOS webes nézet natív a Mobile Engagement IOS SDK</span><span class="sxs-lookup"><span data-stu-id="0045b-103">Bridge iOS WebView with native Mobile Engagement iOS SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0045b-104">Android híd</span><span class="sxs-lookup"><span data-stu-id="0045b-104">Android Bridge</span></span>](mobile-engagement-bridge-webview-native-android.md)
> * [<span data-ttu-id="0045b-105">iOS híd</span><span class="sxs-lookup"><span data-stu-id="0045b-105">iOS Bridge</span></span>](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

<span data-ttu-id="0045b-106">Egyes mobil alkalmazások úgy lettek kialakítva, ahol maga az alkalmazás fejlesztett Objective-C natív iOS-fejlesztések, de még a képernyők listáját megjelennek-e egy webes nézet iOS belül hibrid alkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="0045b-106">Some mobile apps are designed as a hybrid app where the app itself is developed using native iOS Objective-C development but some or even all of the screens are rendered within an iOS WebView.</span></span> <span data-ttu-id="0045b-107">Ön továbbra is használhatja a Mobile Engagement iOS SDK ilyen alkalmazásokon belül, és ez az oktatóanyag leírja, hogyan érhetők el ezzel.</span><span class="sxs-lookup"><span data-stu-id="0045b-107">You can still consume Mobile Engagement iOS SDK within such apps and this tutorial describes how to go about doing this.</span></span> 

<span data-ttu-id="0045b-108">Ennek ellenére nem dokumentált mindkettő elérése kétféleképpen történhet:</span><span class="sxs-lookup"><span data-stu-id="0045b-108">There are two approaches to achieve this though both are undocumented:</span></span>

* <span data-ttu-id="0045b-109">Először egy leírását itt [hivatkozás](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) amely magában foglalja a regisztrálása a `UIWebViewDelegate` a webes nézet és a catch-és-azonnal-Mégse gombra a hely módosítása Javascript végezheti el.</span><span class="sxs-lookup"><span data-stu-id="0045b-109">First one is described on this [link](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) which involves registering a `UIWebViewDelegate` on your web view and catch-and-immediatly-cancel a location change done in Javascript.</span></span> 
* <span data-ttu-id="0045b-110">Második egy alapján ez [WWDC 2013 munkamenet](https://developer.apple.com/videos/play/wwdc2013/615), tisztító, mint az első, amely azt követi a jelen útmutató, amely egy módszert.</span><span class="sxs-lookup"><span data-stu-id="0045b-110">Second one is based on this [WWDC 2013 session](https://developer.apple.com/videos/play/wwdc2013/615), an approach which is cleaner than the first and which we will follow for this guide.</span></span> <span data-ttu-id="0045b-111">Vegye figyelembe, hogy ez a módszer csak akkor működik ios7-en és újabb verziók.</span><span class="sxs-lookup"><span data-stu-id="0045b-111">Note that this approach only works on iOS7 and above.</span></span> 

<span data-ttu-id="0045b-112">Az iOS minta hidat képeznek a, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0045b-112">Follow the steps below for the iOS bridge sample:</span></span>

1. <span data-ttu-id="0045b-113">Első lépésként gondoskodnia kell arról, hogy elvégezték-a [bevezető oktatóanyag](mobile-engagement-ios-get-started.md) integrálható a Mobile Engagement iOS SDK a hibrid alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0045b-113">First of all, you need to ensure that you have gone through our [Getting Started tutorial](mobile-engagement-ios-get-started.md) to integrate the Mobile Engagement iOS SDK in your hybrid app.</span></span> <span data-ttu-id="0045b-114">Szükség esetén is engedélyezheti, hogy az SDK módszerek látható módon azt hajtható végre ezeket a webes nézet teszt naplózás az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="0045b-114">Optionally, you can also enable test logging as follows so that you can see the SDK methods as we trigger them from the webview.</span></span> 
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
             [EngagementAgent setTestLogEnabled:YES];
           ....
        }
2. <span data-ttu-id="0045b-115">Most győződjön meg arról, hogy rendelkezik-e a hibrid app egy webes nézet egy képernyőt rajta.</span><span class="sxs-lookup"><span data-stu-id="0045b-115">Now make sure that your hybrid app has a screen with a webview on it.</span></span> <span data-ttu-id="0045b-116">Hozzáadhat, úgy, hogy a `Main.storyboard` az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0045b-116">You can add it to the `Main.storyboard` of the app.</span></span> 
3. <span data-ttu-id="0045b-117">Ez a webes nézet társítsa a **ViewController** kattintással és húzással a webes nézet a View vezérlő leképezni kívánt jelenetben a a `ViewController.h` képernyőn helyezés szerkesztése csak az alábbiakban a `@interface` sor.</span><span class="sxs-lookup"><span data-stu-id="0045b-117">Associate this webview with your **ViewController** by clicking and dragging the webview from the View Controller Scene to the `ViewController.h` edit screen, placing it just below the `@interface` line.</span></span> 
4. <span data-ttu-id="0045b-118">Ezután a, egy párbeszédpanel jelenik kérő nevét.</span><span class="sxs-lookup"><span data-stu-id="0045b-118">Once you do this, a dialog box will pop up asking for a name.</span></span> <span data-ttu-id="0045b-119">Adja meg a nevet a következőként **webes nézet**.</span><span class="sxs-lookup"><span data-stu-id="0045b-119">Provide the name as **webView**.</span></span> <span data-ttu-id="0045b-120">A `ViewController.h` fájl a következő hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="0045b-120">Your `ViewController.h` file should look like the following:</span></span>
   
        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
   
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
   
        @end
5. <span data-ttu-id="0045b-121">A Microsoft frissíti a `ViewController.m` később fájlt, de először létrehozunk a híd fájlt, amely egy burkolót hoz keresztül néhány gyakran használt Mobile Engagement iOS SDK módszerek.</span><span class="sxs-lookup"><span data-stu-id="0045b-121">We will update the `ViewController.m` file later but first we will create the bridge file which creates a wrapper over some commonly used Mobile Engagement iOS SDK methods.</span></span> <span data-ttu-id="0045b-122">Hozzon létre egy új fejlécfájlt nevű **EngagementJsExports.h** használó a `JSExport` a fent ismertetett mechanizmus [munkamenet](https://developer.apple.com/videos/play/wwdc2013/615) teszi közzé a natív iOS módszerek.</span><span class="sxs-lookup"><span data-stu-id="0045b-122">Create a new header file called **EngagementJsExports.h** which uses the `JSExport` mechanism described in the aforementioned [session](https://developer.apple.com/videos/play/wwdc2013/615) to expose the native iOS methods.</span></span> 
   
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
6. <span data-ttu-id="0045b-123">Ezután létre fogunk hozni a híd fájl második részét.</span><span class="sxs-lookup"><span data-stu-id="0045b-123">Next we will create the second part of the bridge file.</span></span> <span data-ttu-id="0045b-124">Hozzon létre egy nevű fájlt **EngagementJsExports.m** tartalmazó fogja a tényleges burkolók létrehozása a Mobile Engagement iOS SDK metódusok meghívása végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="0045b-124">Create a file called **EngagementJsExports.m** which will contain the implementation creating the actual wrappers by calling the Mobile Engagement iOS SDK methods.</span></span> <span data-ttu-id="0045b-125">Is vegye figyelembe, hogy azt elemzése a `extras` átadta-e a webes nézet JavaScript, és azokat, amelyek üzembe egy `NSMutableDictionary` objektumot az Engagement SDK metódushívások átadni.</span><span class="sxs-lookup"><span data-stu-id="0045b-125">Also note that we are parsing the `extras` being passed from the webview javascript and putting that into an `NSMutableDictionary` object to be passed with the Engagement SDK method calls.</span></span>  
   
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
7. <span data-ttu-id="0045b-126">Most azt térjen vissza a **ViewController.m** és frissíti a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="0045b-126">Now we come back to the **ViewController.m** and update it with the following code:</span></span> 
   
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
8. <span data-ttu-id="0045b-127">A következő szempontokat vegye figyelembe a kapcsolatos a **ViewController.m** fájlt:</span><span class="sxs-lookup"><span data-stu-id="0045b-127">Note the following points about the **ViewController.m** file:</span></span>
   
   * <span data-ttu-id="0045b-128">Az a `loadWebView` módszer, azt letöltése folyamatban van egy helyi HTML-fájl neve **LocalPage.html** azt a következő felülvizsgálja azonosítójú.</span><span class="sxs-lookup"><span data-stu-id="0045b-128">In the `loadWebView` method, we are loading a local HTML file called **LocalPage.html** whose code we will review next.</span></span> 
   * <span data-ttu-id="0045b-129">Az a `webViewDidFinishLoad` metódust, hogy vannak grabbing a `JsContext` és a burkoló osztályt társít.</span><span class="sxs-lookup"><span data-stu-id="0045b-129">In the `webViewDidFinishLoad` method, we are grabbing the `JsContext` and associating our wrapper class with it.</span></span> <span data-ttu-id="0045b-130">Ez lehetővé teszi a a burkoló a leíró használatával SDK metódusok meghívása **EngagementJs** a a webes nézet.</span><span class="sxs-lookup"><span data-stu-id="0045b-130">This will allow calling our wrapper SDK methods using the handle **EngagementJs** from the webView.</span></span> 
9. <span data-ttu-id="0045b-131">Hozzon létre egy nevű fájlt **LocalPage.html** az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="0045b-131">Create a file called **LocalPage.html** with the following code:</span></span>
   
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
                       // Example of how extras info can be passed with the Engagement logs
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
10. <span data-ttu-id="0045b-132">A fenti HTML-fájl a következő szempontokat vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="0045b-132">Note the following points about the HTML file above:</span></span>
    
    * <span data-ttu-id="0045b-133">A beviteli mezőbe, ahol megadhatja a esemény, a feladat, a hiba, a AppInfo névként használt adatok készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0045b-133">It contains a set of input boxes where you can provide data to be used as names for your Event, Job, Error, AppInfo.</span></span> <span data-ttu-id="0045b-134">Amikor a látható gombra kattint, egy kérés érkezett a Javascript, ami végül az olyan módszereket hív meg felelt meg a Mobile Engagement iOS SDK hívása híd fájlból.</span><span class="sxs-lookup"><span data-stu-id="0045b-134">When you click on the button next to it, a call is made to the Javascript which eventually calls the methods from the bridge file to pass this call to the Mobile Engagement iOS SDK.</span></span> 
    * <span data-ttu-id="0045b-135">Azt a néhány statikus további információt az események, feladatok és bemutatják, hogyan ezt megteheti a még akkor is, hibákat vannak címkézést.</span><span class="sxs-lookup"><span data-stu-id="0045b-135">We are tagging on some static extra info to the events, jobs and even errors to demonstrate how this could be done.</span></span> <span data-ttu-id="0045b-136">A felesleges adatokat küldött, egy JSON karakterlánc, amely jelenik meg a `EngagementJsExports.m` fájl elemzése, amely átadott események, feladatok, hibák küldése mellett.</span><span class="sxs-lookup"><span data-stu-id="0045b-136">This extra info is sent as a JSON string which, if you look in the `EngagementJsExports.m` file, is parsed and passed along with sending Events, Jobs, Errors.</span></span> 
    * <span data-ttu-id="0045b-137">A Mobile Engagement feladat kezdődött el a nevet, adjon meg a beviteli mezőbe, futtassa a 10 másodperc, és állítsa le.</span><span class="sxs-lookup"><span data-stu-id="0045b-137">A Mobile Engagement Job is kicked off with the name you specify in the input box, run for 10 seconds and shut down.</span></span> 
    * <span data-ttu-id="0045b-138">A Mobile Engagement appinfo vagy címke átadása "customer_name" statikus kulcs és a megadott bemeneti értékeként a címke értéke.</span><span class="sxs-lookup"><span data-stu-id="0045b-138">A Mobile Engagement appinfo or tag is passed with 'customer_name' as the static key and the value that you entered in the input as the value for the tag.</span></span> 
11. <span data-ttu-id="0045b-139">Futtassa az alkalmazást, és a következő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0045b-139">Run the app and you will see the following.</span></span> <span data-ttu-id="0045b-140">Most adja meg a következőhöz hasonló vizsgálati esemény néhány nevét, majd kattintson **küldése** látható.</span><span class="sxs-lookup"><span data-stu-id="0045b-140">Now provide some name for a test event like the following and click **Send** next to it.</span></span> 
    
     ![][1]
12. <span data-ttu-id="0045b-141">Ha most a **figyelő** fülre az alkalmazás, és kattintson duplán a **események -> részletek**, láthatja, hogy ez az esemény jelenik meg a statikus app-info azt küldő együtt.</span><span class="sxs-lookup"><span data-stu-id="0045b-141">Now if you go to the **Monitor** tab of your app and look under **Events -> Details**, you will see this event show up along with the static app-info that we are sending.</span></span> 
    
    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
