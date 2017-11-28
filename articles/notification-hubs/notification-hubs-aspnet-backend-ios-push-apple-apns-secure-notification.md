---
title: "Notification Hubs biztonságos leküldéses aaaAzure"
description: "Tudnivalók a biztonságos toosend a leküldéses értesítések tooan iOS-alkalmazás az Azure-ból. Kódminták Objective-C és C#."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 17d42b0a-2c80-4e35-a1ed-ed510d19f4b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 86dd8d7042e5b9e55d2d7ff41cb42f23831fc575
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="c44b5-104">Biztonságos Azure Notification Hubs leküldéses</span><span class="sxs-lookup"><span data-stu-id="c44b5-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c44b5-105">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="c44b5-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="c44b5-106">iOS</span><span class="sxs-lookup"><span data-stu-id="c44b5-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="c44b5-107">Android</span><span class="sxs-lookup"><span data-stu-id="c44b5-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="c44b5-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c44b5-108">Overview</span></span>
<span data-ttu-id="c44b5-109">Leküldéses értesítési támogatása a Microsoft Azure lehetővé teszi tooaccess egy könnyen használható, többplatformos, kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok.</span><span class="sxs-lookup"><span data-stu-id="c44b5-109">Push notification support in Microsoft Azure enables you tooaccess an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="c44b5-110">Tooregulatory vagy biztonsági korlátozások miatt néha egy alkalmazás érdemes tooinclude valamit a hello szabványos leküldéses értesítési infrastruktúrát keresztül nem továbbítható hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="c44b5-110">Due tooregulatory or security constraints, sometimes an application might want tooinclude something in hello notification that cannot be transmitted through hello standard push notification infrastructure.</span></span> <span data-ttu-id="c44b5-111">Ez az oktatóanyag leírja, hogyan tooachieve hello úgy, hogy a bizalmas adatokat hello ügyféleszköz- és hello háttéralkalmazás közötti biztonságos és hitelesített kapcsolaton keresztül küld ugyanazt a felhasználói élményt.</span><span class="sxs-lookup"><span data-stu-id="c44b5-111">This tutorial describes how tooachieve hello same experience by sending sensitive information through a secure, authenticated connection between hello client device and hello app backend.</span></span>

<span data-ttu-id="c44b5-112">Magas szinten hello folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="c44b5-112">At a high level, hello flow is as follows:</span></span>

1. <span data-ttu-id="c44b5-113">hello app háttér:</span><span class="sxs-lookup"><span data-stu-id="c44b5-113">hello app back-end:</span></span>
   * <span data-ttu-id="c44b5-114">Háttér-adatbázisban tárolja biztonságos hasznos.</span><span class="sxs-lookup"><span data-stu-id="c44b5-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="c44b5-115">Küldi hello azonosítója (nem biztonságos információk küldése) értesítési toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="c44b5-115">Sends hello ID of this notification toohello device (no secure information is sent).</span></span>
2. <span data-ttu-id="c44b5-116">hello alkalmazást hello eszközön, hello értesítés fogadása közben:</span><span class="sxs-lookup"><span data-stu-id="c44b5-116">hello app on hello device, when receiving hello notification:</span></span>
   * <span data-ttu-id="c44b5-117">hello eszköz kapcsolatba lép a hello háttér-kérelmező hello biztonságos hasznos.</span><span class="sxs-lookup"><span data-stu-id="c44b5-117">hello device contacts hello back-end requesting hello secure payload.</span></span>
   * <span data-ttu-id="c44b5-118">hello hasznos mutatja az hello app értesítésként hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="c44b5-118">hello app can show hello payload as a notification on hello device.</span></span>

<span data-ttu-id="c44b5-119">Fontos, hogy megelőző folyamata hello (és az oktatóanyag) feltételezzük, hogy hello eszköz toonote tárol egy hitelesítési jogkivonatot helyi tárhelyre – hello felhasználó bejelentkezése után.</span><span class="sxs-lookup"><span data-stu-id="c44b5-119">It is important toonote that in hello preceding flow (and in this tutorial), we assume that hello device stores an authentication token in local storage, after hello user logs in.</span></span> <span data-ttu-id="c44b5-120">Ez biztosítja, hogy teljesen zökkenőmentes élményt, mivel hello eszköz hello értesítési biztonságos hasznos a token használatával kérheti le.</span><span class="sxs-lookup"><span data-stu-id="c44b5-120">This guarantees a completely seamless experience, as hello device can retrieve hello notification’s secure payload using this token.</span></span> <span data-ttu-id="c44b5-121">Ha az alkalmazás nem tárolja a hitelesítési tokenek hello eszköz, vagy ezeket a jogkivonatokat is járhatott, hello eszközalkalmazás hello értesítés fogadásakor megjelenjen-e arra kéri a hello felhasználói toolaunch hello alkalmazás általános értesítést.</span><span class="sxs-lookup"><span data-stu-id="c44b5-121">If your application does not store authentication tokens on hello device, or if these tokens can be expired, hello device app, upon receiving hello notification should display a generic notification prompting hello user toolaunch hello app.</span></span> <span data-ttu-id="c44b5-122">hello app majd hello felhasználó hitelesíti, és hello értesítési tartalom jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c44b5-122">hello app then authenticates hello user and shows hello notification payload.</span></span>

<span data-ttu-id="c44b5-123">Ez biztonságos leküldéses az oktatóanyag bemutatja, hogyan toosend leküldéses értesítés biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="c44b5-123">This Secure Push tutorial shows how toosend a push notification securely.</span></span> <span data-ttu-id="c44b5-124">hello oktatóanyag épít, hello [felhasználók értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyag, ezért el kell végeznie hello lépéseket, hogy az oktatóanyagban először.</span><span class="sxs-lookup"><span data-stu-id="c44b5-124">hello tutorial builds on hello [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial, so you should complete hello steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="c44b5-125">Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c44b5-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-ios-project"></a><span data-ttu-id="c44b5-126">IOS-projekt hello módosítása</span><span class="sxs-lookup"><span data-stu-id="c44b5-126">Modify hello iOS project</span></span>
<span data-ttu-id="c44b5-127">Most, hogy az alkalmazás háttér-toosend csak hello módosított *azonosító* egy értesítés, hogy toochange az iOS app toohandle, értesítések és a visszahívás a háttér-tooretrieve hello megjelenített üzenet toobe biztonságos.</span><span class="sxs-lookup"><span data-stu-id="c44b5-127">Now that you modified your app back-end toosend just hello *id* of a notification, you have toochange your iOS app toohandle that notification and call back your back-end tooretrieve hello secure message toobe displayed.</span></span>

<span data-ttu-id="c44b5-128">tooachieve toowrite hello logika tooretrieve hello biztonságos tartalmat app háttér-hello tudunk ezen cél esetében.</span><span class="sxs-lookup"><span data-stu-id="c44b5-128">tooachieve this goal, we have toowrite hello logic tooretrieve hello secure content from hello app back-end.</span></span>

1. <span data-ttu-id="c44b5-129">A **AppDelegate.m**, győződjön meg arról, hogy hello app regiszterekben csendes értesítések úgy dolgozza fel a küldött hello háttérrendszerből hello értesítés azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c44b5-129">In **AppDelegate.m**, make sure hello app registers for silent notifications so it processes hello notification id sent from hello backend.</span></span> <span data-ttu-id="c44b5-130">Adja hozzá a hello **UIRemoteNotificationTypeNewsstandContentAvailability** didFinishLaunchingWithOptions beállítást:</span><span class="sxs-lookup"><span data-stu-id="c44b5-130">Add hello **UIRemoteNotificationTypeNewsstandContentAvailability** option in didFinishLaunchingWithOptions:</span></span>
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. <span data-ttu-id="c44b5-131">Az a **AppDelegate.m** adja hozzá az implementációs szakaszban hello felső hello deklaráció a következő:</span><span class="sxs-lookup"><span data-stu-id="c44b5-131">In your **AppDelegate.m** add an implementation section at hello top with hello following declaration:</span></span>
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. <span data-ttu-id="c44b5-132">Adja meg a hello megvalósítási szakaszban hello a következő kódot, és hello helyőrző `{back-end endpoint}` a háttér-korábban beszerzett hello-végponthoz:</span><span class="sxs-lookup"><span data-stu-id="c44b5-132">Then add in hello implementation section hello following code, substituting hello placeholder `{back-end endpoint}` with hello endpoint for your back-end obtained previously:</span></span>

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end tooretrieve hello notification content using hello credentials stored in hello shared preferences.

1. <span data-ttu-id="c44b5-133">Most azt toohandle hello bejövő értesítési rendelkezik, és fent tooretrieve hello tartalom toodisplay hello módszert használja.</span><span class="sxs-lookup"><span data-stu-id="c44b5-133">Now we have toohandle hello incoming notification and use hello method above tooretrieve hello content toodisplay.</span></span> <span data-ttu-id="c44b5-134">Először azt kell tooenable az iOS app toorun hello háttérben leküldéses értesítés fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="c44b5-134">First, we have tooenable your iOS app toorun in hello background when receiving a push notification.</span></span> <span data-ttu-id="c44b5-135">A **XCode**, az alkalmazás projekt hello bal oldali panelen, majd kattintson a fő cél a hello **célok** szakasz hello központi panelről.</span><span class="sxs-lookup"><span data-stu-id="c44b5-135">In **XCode**, select your app project on hello left panel, then click your main app target in hello **Targets** section from hello central pane.</span></span>
2. <span data-ttu-id="c44b5-136">Kattintson a **képességek** a központi ablaktábla felső hello fülre, és ellenőrizze a hello **távoli értesítések** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c44b5-136">Then click your **Capabilities** tab at hello top of your central pane, and check hello **Remote Notifications** checkbox.</span></span>
   
    ![][IOS1]
3. <span data-ttu-id="c44b5-137">A **AppDelegate.m** hello a következő metódus toohandle leküldéses értesítések hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="c44b5-137">In **AppDelegate.m** add hello following method toohandle push notifications:</span></span>
   
        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);
   
            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];
   
                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];
   
        }
   
    <span data-ttu-id="c44b5-138">Ne feledje, hogy hiányzó hitelesítési fejléc tulajdonság vagy elutasíthatják hello háttér-érdemes toohandle hello esetekben.</span><span class="sxs-lookup"><span data-stu-id="c44b5-138">Note that it is preferable toohandle hello cases of missing authentication header property or rejection by hello back-end.</span></span> <span data-ttu-id="c44b5-139">Ezekben az esetekben hello adott kezelésének legtöbbször a cél felhasználói élmény függ.</span><span class="sxs-lookup"><span data-stu-id="c44b5-139">hello specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="c44b5-140">Egy elem toodisplay hello tooauthenticate tooretrieve hello tényleges Felhasználóértesítés általános kérése az értesítést.</span><span class="sxs-lookup"><span data-stu-id="c44b5-140">One option is toodisplay a notification with a generic prompt for hello user tooauthenticate tooretrieve hello actual notification.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="c44b5-141">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="c44b5-141">Run hello Application</span></span>
<span data-ttu-id="c44b5-142">toorun hello alkalmazás, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c44b5-142">toorun hello application, do hello following:</span></span>

1. <span data-ttu-id="c44b5-143">Az XCode-ban hello alkalmazás futtatása egy fizikai iOS-eszközön (leküldéses értesítések hello szimulátor fog működni).</span><span class="sxs-lookup"><span data-stu-id="c44b5-143">In XCode, run hello app on a physical iOS device (push notifications will not work in hello simulator).</span></span>
2. <span data-ttu-id="c44b5-144">A hello iOS-alkalmazás felhasználói felületén írja be a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="c44b5-144">In hello iOS app UI, enter a username and password.</span></span> <span data-ttu-id="c44b5-145">Ezek karakterlánc lehet, de kell hello ugyanazt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c44b5-145">These can be any string, but they must be hello same value.</span></span>
3. <span data-ttu-id="c44b5-146">Hello iOS-alkalmazás felhasználói felületén, kattintson **jelentkezzen be**.</span><span class="sxs-lookup"><span data-stu-id="c44b5-146">In hello iOS app UI, click **Log in**.</span></span> <span data-ttu-id="c44b5-147">Kattintson a **leküldéses küldése**.</span><span class="sxs-lookup"><span data-stu-id="c44b5-147">Then click **Send push**.</span></span> <span data-ttu-id="c44b5-148">Nem jelenik meg az értesítési központ a következő hello biztonságos értesítésnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="c44b5-148">You should see hello secure notification being displayed in your notification center.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
