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
# <a name="azure-notification-hubs-secure-push"></a>Biztonságos Azure Notification Hubs leküldéses
> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Áttekintés
Leküldéses értesítési támogatása a Microsoft Azure lehetővé teszi tooaccess egy könnyen használható, többplatformos, kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok.

Tooregulatory vagy biztonsági korlátozások miatt néha egy alkalmazás érdemes tooinclude valamit a hello szabványos leküldéses értesítési infrastruktúrát keresztül nem továbbítható hello értesítést. Ez az oktatóanyag leírja, hogyan tooachieve hello úgy, hogy a bizalmas adatokat hello ügyféleszköz- és hello háttéralkalmazás közötti biztonságos és hitelesített kapcsolaton keresztül küld ugyanazt a felhasználói élményt.

Magas szinten hello folyamat a következőképpen történik:

1. hello app háttér:
   * Háttér-adatbázisban tárolja biztonságos hasznos.
   * Küldi hello azonosítója (nem biztonságos információk küldése) értesítési toohello eszköz.
2. hello alkalmazást hello eszközön, hello értesítés fogadása közben:
   * hello eszköz kapcsolatba lép a hello háttér-kérelmező hello biztonságos hasznos.
   * hello hasznos mutatja az hello app értesítésként hello eszközön.

Fontos, hogy megelőző folyamata hello (és az oktatóanyag) feltételezzük, hogy hello eszköz toonote tárol egy hitelesítési jogkivonatot helyi tárhelyre – hello felhasználó bejelentkezése után. Ez biztosítja, hogy teljesen zökkenőmentes élményt, mivel hello eszköz hello értesítési biztonságos hasznos a token használatával kérheti le. Ha az alkalmazás nem tárolja a hitelesítési tokenek hello eszköz, vagy ezeket a jogkivonatokat is járhatott, hello eszközalkalmazás hello értesítés fogadásakor megjelenjen-e arra kéri a hello felhasználói toolaunch hello alkalmazás általános értesítést. hello app majd hello felhasználó hitelesíti, és hello értesítési tartalom jeleníti meg.

Ez biztonságos leküldéses az oktatóanyag bemutatja, hogyan toosend leküldéses értesítés biztonságos helyen. hello oktatóanyag épít, hello [felhasználók értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyag, ezért el kell végeznie hello lépéseket, hogy az oktatóanyagban először.

> [!NOTE]
> Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-ios-project"></a>IOS-projekt hello módosítása
Most, hogy az alkalmazás háttér-toosend csak hello módosított *azonosító* egy értesítés, hogy toochange az iOS app toohandle, értesítések és a visszahívás a háttér-tooretrieve hello megjelenített üzenet toobe biztonságos.

tooachieve toowrite hello logika tooretrieve hello biztonságos tartalmat app háttér-hello tudunk ezen cél esetében.

1. A **AppDelegate.m**, győződjön meg arról, hogy hello app regiszterekben csendes értesítések úgy dolgozza fel a küldött hello háttérrendszerből hello értesítés azonosítója. Adja hozzá a hello **UIRemoteNotificationTypeNewsstandContentAvailability** didFinishLaunchingWithOptions beállítást:
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. Az a **AppDelegate.m** adja hozzá az implementációs szakaszban hello felső hello deklaráció a következő:
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. Adja meg a hello megvalósítási szakaszban hello a következő kódot, és hello helyőrző `{back-end endpoint}` a háttér-korábban beszerzett hello-végponthoz:

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

1. Most azt toohandle hello bejövő értesítési rendelkezik, és fent tooretrieve hello tartalom toodisplay hello módszert használja. Először azt kell tooenable az iOS app toorun hello háttérben leküldéses értesítés fogadásakor. A **XCode**, az alkalmazás projekt hello bal oldali panelen, majd kattintson a fő cél a hello **célok** szakasz hello központi panelről.
2. Kattintson a **képességek** a központi ablaktábla felső hello fülre, és ellenőrizze a hello **távoli értesítések** jelölőnégyzetet.
   
    ![][IOS1]
3. A **AppDelegate.m** hello a következő metódus toohandle leküldéses értesítések hozzáadása:
   
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
   
    Ne feledje, hogy hiányzó hitelesítési fejléc tulajdonság vagy elutasíthatják hello háttér-érdemes toohandle hello esetekben. Ezekben az esetekben hello adott kezelésének legtöbbször a cél felhasználói élmény függ. Egy elem toodisplay hello tooauthenticate tooretrieve hello tényleges Felhasználóértesítés általános kérése az értesítést.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
toorun hello alkalmazás, a következő hello:

1. Az XCode-ban hello alkalmazás futtatása egy fizikai iOS-eszközön (leküldéses értesítések hello szimulátor fog működni).
2. A hello iOS-alkalmazás felhasználói felületén írja be a felhasználónevet és jelszót. Ezek karakterlánc lehet, de kell hello ugyanazt az értéket.
3. Hello iOS-alkalmazás felhasználói felületén, kattintson **jelentkezzen be**. Kattintson a **leküldéses küldése**. Nem jelenik meg az értesítési központ a következő hello biztonságos értesítésnek kell megjelennie.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
