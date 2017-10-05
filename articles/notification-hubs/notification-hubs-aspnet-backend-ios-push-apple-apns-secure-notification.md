---
title: "Biztonságos Azure Notification Hubs leküldéses"
description: "Megtudhatja, hogyan biztonságos leküldéses értesítések küldéséhez iOS-alkalmazásokhoz az Azure-ból. Kódminták Objective-C és C#."
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
ms.openlocfilehash: e5f09fb3716303bb21fe7442aa6fa8832174838e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="7e7cc-104">Biztonságos Azure Notification Hubs leküldéses</span><span class="sxs-lookup"><span data-stu-id="7e7cc-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7e7cc-105">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="7e7cc-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="7e7cc-106">iOS</span><span class="sxs-lookup"><span data-stu-id="7e7cc-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="7e7cc-107">Android</span><span class="sxs-lookup"><span data-stu-id="7e7cc-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="7e7cc-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7e7cc-108">Overview</span></span>
<span data-ttu-id="7e7cc-109">Leküldéses értesítési támogatása a Microsoft Azure lehetővé teszi egy könnyen használható, többplatformos, kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűsíti a leküldéses értesítések mobil platformokhoz fogyasztói, valamint a vállalati alkalmazások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-109">Push notification support in Microsoft Azure enables you to access an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="7e7cc-110">Szabályozó miatt vagy biztonsági korlátozások néha egy alkalmazás előfordulhat, hogy szeretne valamit az értesítés, amely a szabványos leküldéses értesítési infrastruktúrát keresztül nem lehet megadni.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-110">Due to regulatory or security constraints, sometimes an application might want to include something in the notification that cannot be transmitted through the standard push notification infrastructure.</span></span> <span data-ttu-id="7e7cc-111">Ez az oktatóanyag ismerteti, hogyan által a biztonságos és hitelesített kapcsolatot az ügyféleszközön és a háttéralkalmazás keresztül érzékeny adatokat küld a felhasználói élmény eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-111">This tutorial describes how to achieve the same experience by sending sensitive information through a secure, authenticated connection between the client device and the app backend.</span></span>

<span data-ttu-id="7e7cc-112">Magas szinten a folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="7e7cc-112">At a high level, the flow is as follows:</span></span>

1. <span data-ttu-id="7e7cc-113">Az alkalmazás háttér:</span><span class="sxs-lookup"><span data-stu-id="7e7cc-113">The app back-end:</span></span>
   * <span data-ttu-id="7e7cc-114">Háttér-adatbázisban tárolja biztonságos hasznos.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="7e7cc-115">Az értesítés Azonosítójának elküldi az eszköznek (nem biztonságos információk küldése).</span><span class="sxs-lookup"><span data-stu-id="7e7cc-115">Sends the ID of this notification to the device (no secure information is sent).</span></span>
2. <span data-ttu-id="7e7cc-116">Az alkalmazást az eszközön, az értesítés fogadása közben:</span><span class="sxs-lookup"><span data-stu-id="7e7cc-116">The app on the device, when receiving the notification:</span></span>
   * <span data-ttu-id="7e7cc-117">Az eszköz kapcsolatot létesít a háttér-kérő a biztonságos tartalom.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-117">The device contacts the back-end requesting the secure payload.</span></span>
   * <span data-ttu-id="7e7cc-118">Az alkalmazás a tartalom megjeleníthető értesítésként az eszközön.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-118">The app can show the payload as a notification on the device.</span></span>

<span data-ttu-id="7e7cc-119">Fontos megjegyezni, hogy az előző folyamatában (és ebben az oktatóanyagban), feltételezzük, hogy az eszköz tárol egy hitelesítési jogkivonatot helyi tároló, a felhasználó bejelentkezése után.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-119">It is important to note that in the preceding flow (and in this tutorial), we assume that the device stores an authentication token in local storage, after the user logs in.</span></span> <span data-ttu-id="7e7cc-120">Ez biztosítja, hogy teljesen zökkenőmentes élményt, mivel az eszköz le az értesítési biztonságos tartalom a tokent.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-120">This guarantees a completely seamless experience, as the device can retrieve the notification’s secure payload using this token.</span></span> <span data-ttu-id="7e7cc-121">Ha az alkalmazás nem tárolja a hitelesítési tokenek az eszközön, vagy ezeket a jogkivonatokat is lejárt, az eszköz alkalmazást, és az értesítés fogadásakor megjelenjen-e a felhasználó megkérdezése általános értesítési indíthatja el az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-121">If your application does not store authentication tokens on the device, or if these tokens can be expired, the device app, upon receiving the notification should display a generic notification prompting the user to launch the app.</span></span> <span data-ttu-id="7e7cc-122">Az alkalmazás majd hitelesíti a felhasználót, és az értesítési tartalom jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-122">The app then authenticates the user and shows the notification payload.</span></span>

<span data-ttu-id="7e7cc-123">Biztonságos leküldéses az oktatóanyag bemutatja, hogyan biztonságosan leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-123">This Secure Push tutorial shows how to send a push notification securely.</span></span> <span data-ttu-id="7e7cc-124">Az oktatóanyag épít, a [felhasználók értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyag, ezért el kell végeznie a lépéseket, hogy az oktatóanyagban először.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-124">The tutorial builds on the [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial, so you should complete the steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="7e7cc-125">Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7e7cc-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a><span data-ttu-id="7e7cc-126">Az iOS-projektre módosítása</span><span class="sxs-lookup"><span data-stu-id="7e7cc-126">Modify the iOS project</span></span>
<span data-ttu-id="7e7cc-127">Most, hogy módosította, a app háttér küldése csak a *azonosító* egy értesítés, módosítania kell az iOS-alkalmazás az értesítés, és a háttér-letölteni a biztonságos üzenetet megjelenítendő visszahívási.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-127">Now that you modified your app back-end to send just the *id* of a notification, you have to change your iOS app to handle that notification and call back your back-end to retrieve the secure message to be displayed.</span></span>

<span data-ttu-id="7e7cc-128">E cél eléréséhez, igazolnia kell a biztonságos tartalmat lekérjen a app háttér-logika írását.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-128">To achieve this goal, we have to write the logic to retrieve the secure content from the app back-end.</span></span>

1. <span data-ttu-id="7e7cc-129">A **AppDelegate.m**, győződjön meg arról, hogy az alkalmazás regisztrálása a beavatkozás nélküli értesítések, akkor feldolgozza a háttérrendszer által küldött értesítés-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-129">In **AppDelegate.m**, make sure the app registers for silent notifications so it processes the notification id sent from the backend.</span></span> <span data-ttu-id="7e7cc-130">Adja hozzá a **UIRemoteNotificationTypeNewsstandContentAvailability** didFinishLaunchingWithOptions beállítást:</span><span class="sxs-lookup"><span data-stu-id="7e7cc-130">Add the **UIRemoteNotificationTypeNewsstandContentAvailability** option in didFinishLaunchingWithOptions:</span></span>
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. <span data-ttu-id="7e7cc-131">Az a **AppDelegate.m** adja hozzá az implementációs szakaszban a következő nyilatkozattal felső:</span><span class="sxs-lookup"><span data-stu-id="7e7cc-131">In your **AppDelegate.m** add an implementation section at the top with the following declaration:</span></span>
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. <span data-ttu-id="7e7cc-132">Majd adja hozzá a megvalósítási szakaszban a következő kódra, és a helyőrző `{back-end endpoint}` a végponthoz, a háttér-korábban beszerzett esetében:</span><span class="sxs-lookup"><span data-stu-id="7e7cc-132">Then add in the implementation section the following code, substituting the placeholder `{back-end endpoint}` with the endpoint for your back-end obtained previously:</span></span>

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

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

1. <span data-ttu-id="7e7cc-133">Most van a bejövő értesítést, és a fenti metódus használatával a tartalom megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-133">Now we have to handle the incoming notification and use the method above to retrieve the content to display.</span></span> <span data-ttu-id="7e7cc-134">Először azt kell ahhoz, hogy az iOS-alkalmazás fut a háttérben leküldéses értesítés fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-134">First, we have to enable your iOS app to run in the background when receiving a push notification.</span></span> <span data-ttu-id="7e7cc-135">A **XCode**, a app projektet, a bal oldali panelen, majd kattintson a fő cél a a **célok** szakasz a központi panelről.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-135">In **XCode**, select your app project on the left panel, then click your main app target in the **Targets** section from the central pane.</span></span>
2. <span data-ttu-id="7e7cc-136">Kattintson a **képességek** a központi ablaktábla tetején fülre, és ellenőrizze a **távoli értesítések** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-136">Then click your **Capabilities** tab at the top of your central pane, and check the **Remote Notifications** checkbox.</span></span>
   
    ![][IOS1]
3. <span data-ttu-id="7e7cc-137">A **AppDelegate.m** adja hozzá leküldéses értesítések kezeléséhez a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="7e7cc-137">In **AppDelegate.m** add the following method to handle push notifications:</span></span>
   
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
   
    <span data-ttu-id="7e7cc-138">Ne feledje, hogy az esetek hiányzó hitelesítési fejléc tulajdonság vagy elutasítása kezelésére által a háttér-érdemes.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-138">Note that it is preferable to handle the cases of missing authentication header property or rejection by the back-end.</span></span> <span data-ttu-id="7e7cc-139">Ezekben az esetekben adott kezelésének legtöbbször a cél felhasználói élményét függ.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-139">The specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="7e7cc-140">Egy lehetőség egy értesítés megjelenítése egy általános kérdéshez a felhasználó hitelesítésére a tényleges értesítési beolvasása.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-140">One option is to display a notification with a generic prompt for the user to authenticate to retrieve the actual notification.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="7e7cc-141">Futtassa az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="7e7cc-141">Run the Application</span></span>
<span data-ttu-id="7e7cc-142">Futtassa az alkalmazást, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="7e7cc-142">To run the application, do the following:</span></span>

1. <span data-ttu-id="7e7cc-143">Az xcode-ban az alkalmazás futtatása egy fizikai iOS-eszközön (leküldéses értesítések nem fog működni a szimulátor).</span><span class="sxs-lookup"><span data-stu-id="7e7cc-143">In XCode, run the app on a physical iOS device (push notifications will not work in the simulator).</span></span>
2. <span data-ttu-id="7e7cc-144">Az iOS-alkalmazás felhasználói felületén adja meg egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-144">In the iOS app UI, enter a username and password.</span></span> <span data-ttu-id="7e7cc-145">Bármilyen karakterlánc is lehetnek, de ugyanarra az értékre kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-145">These can be any string, but they must be the same value.</span></span>
3. <span data-ttu-id="7e7cc-146">Kattintson az iOS-alkalmazás felhasználói felületén, **jelentkezzen be**.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-146">In the iOS app UI, click **Log in**.</span></span> <span data-ttu-id="7e7cc-147">Kattintson a **leküldéses küldése**.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-147">Then click **Send push**.</span></span> <span data-ttu-id="7e7cc-148">A következő nem jelenik meg az értesítési központ a biztonságos értesítésnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="7e7cc-148">You should see the secure notification being displayed in your notification center.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
