---
title: "aaaNotification hubok honosított Megtörje hírek oktatóanyag iOS-hez"
description: "Ismerje meg, hogyan toouse Azure Service Bus Notification Hubs toosend honosított legfrissebb híreket tartalmazó értesítések (iOS)."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 9fe88c0440e93b72d349574160ddcd85a7ba0be0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-localized-breaking-news-tooios-devices"></a>Használja a Notification Hubs honosított toosend breaking news tooiOS eszközök
> [!div class="op_single_selector"]
> * [Windows áruház C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Áttekintés
Ez a témakör bemutatja, hogyan toouse hello [sablonok](notification-hubs-templates-cross-platform-push-messages.md) az Azure Notification Hubs toobroadcast nyelv és az eszköz honosított katalóguselemekről híreket tartalmazó értesítések megtörje szolgáltatása. Ez az oktatóanyag a kiindulási pont hello iOS-alkalmazás létrehozása [legfrissebb hírek Notification Hubs használata toosend]. Amikor végzett, képes tooregister érdekli kategóriákban fogja, adjon meg egy nyelvi mely tooreceive hello értesítések, és csak a kiválasztott hello kategóriák leküldéses értesítések fogadásához az adott nyelveken.

Nincsenek két részből toothis forgatókönyv:

* iOS-alkalmazás lehetővé teszi, hogy ügyfél eszközök toospecify egy nyelvet, és toosubscribe toodifferent megtörje hírek kategóriák;
* hello háttér-közzéteszi hello értesítéseket, hello **címke** és **sablon** az Azure Notification Hubs feautres.

## <a name="prerequisites"></a>Előfeltételek
Már végrehajtotta hello [legfrissebb hírek Notification Hubs használata toosend] oktatóanyag és hello kód érhető el, mert ez az oktatóanyag közvetlenül épít, hogy a kód.

A Visual Studio 2012 vagy újabb nem kötelező megadni.

## <a name="template-concepts"></a>Sablon fogalmak
A [legfrissebb hírek Notification Hubs használata toosend] olyan alkalmazás, amelynek használt parancsfájlkezelő **címkék** toosubscribe toonotifications hírek különböző kategóriákban.
Számos alkalmazás, azonban több piacok célként, és honosítási igényelnek. Ez azt jelenti, hogy maguk hello értesítések hello tartalmának rendelkezik honosított toobe kézbesített toohello, javítsa ki az eszközök.
Ebben a témakörben bemutatjuk a hogyan toouse hello **sablon** tooeasily biztosítanak a Notification Hubs szolgáltatása honosított legfrissebb híreket tartalmazó értesítések.

Megjegyzés: egyirányú toosend honosított értesítések toocreate minden címke több verziója van. Például toosupport angol, francia és Mandarin, lenne szükséges három különböző címkék híreket: "world_en", "world_fr" és "world_ch". A Microsoft majd kellene toosend hello world hírek tooeach címkéket honosított verzióját. Ez a témakör használjuk, a címkéket a sablonok tooavoid hello elterjedése és hello követelmény több üzenetet küldeni.

Magas szinten, a sablonok olyan módon toospecify hogyan egy adott eszközhöz egy értesítést kell kapnia. hello sablon hello pontos az adattartalom formátuma hello által küldött üzenet az alkalmazás háttér-részét képező tooproperties alapján határozza meg. Ebben az esetben az összes támogatott nyelvek tartalmazó területibeállítás-független üzenetet küldünk:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Ezután azt fogja győződjön meg arról, hogy az eszközök regisztrálása a sablont, amely hivatkozik a toohello megfelelő tulajdonság. Például egy iOS-alkalmazást, amely a francia hírek tooregister szeretne regisztrálni fogja hello következő:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Sablonok egyik újdonsága nagyon hatékony többet is megtudhat arról a a [sablonok](notification-hubs-templates-cross-platform-push-messages.md) cikk.

## <a name="hello-app-user-interface"></a>hello alkalmazás felhasználói felülete
Most módosítja, a Microsoft hello Megtörje hírek app hello a témakör a [legfrissebb hírek Notification Hubs használata toosend] toosend honosított legfrissebb hírek sablonok használatával.

A MainStoryboard_iPhone.storyboard hello három nyelv, amely támogatjuk szegmentált vezérlőt hozzá: angol, francia és Mandarin.

![][13]

Győződjön meg arról, hogy tooadd egy IBOutlet a ViewController.h az alább látható módon:

![][14]

## <a name="building-hello-ios-app"></a>Épület hello iOS-alkalmazás
1. A Notification.h hozzá hello *retrieveLocale* módszer módosításához hello tárolóban, és az előfizetés módszerek alább látható módon:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    Módosítsa a Notification.m hello *storeCategoriesAndSubscribe* metódus hello területi beállítással rendelkezik, és tárolja őket hello felhasználók alapértelmezett beállításait:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    Módosítsa a hello *előfizetés* metódus tooinclude hello területi beállítás:
   
        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];
   
            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }
   
            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];
   
            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }
   
    Vegye figyelembe, hogyan most használjuk hello metódus *registerTemplateWithDeviceToken*, ahelyett, hogy *registerNativeWithDeviceToken*. Ha a sablon regisztráljuk tudunk tooprovide hello json-sablon, és hello sablon nevét (az alkalmazásnak érdemes tooregister különböző sablonok). Győződjön meg arról, hogy tooregister címkéket, mint a kategóriák, az adott hírek szeretnénk toomake meg arról, hogy tooreceive hello notifciations.
   
    Adja hozzá a metódus tooretrieve hello területi hello felhasználó alapértelmezett beállításai közül:
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. Most, hogy az értesítések osztály módosított azt kell arról, hogy a ViewController teszi toomake hello használata új UISegmentControl. Adja hozzá a következő sort a hello hello *viewDidLoad* metódus toomake meg arról, hogy tooshow hello területi beállítás a kiválasztott:
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    Ezt követően a a *előfizetés* módszer, módosítsa a hívás toohello *storeCategoriesAndSubscribe* toohello következő:
   
        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];
3. Végül ott tooupdate hello *didRegisterForRemoteNotificationsWithDeviceToken* a AppDelegate.m metódust, hogy helyesen, is frissítheti a regisztrációt, az alkalmazás indításakor. Módosítsa a hívás toohello *előfizetés* hello következőre értesítések módszert:
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(választható) .NET-Konzolalkalmazás honosított sablonhoz értesítésküldést.
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-hello-device"></a>(választható) Honosított sablonhoz értesítésküldést hello eszköz
Ha nem access tooVisual Studio, esetleg toojust teszt honosított hello sablon értesítések küldése közvetlenül hello alkalmazásból hello eszközön.  Egyszerű is vegye fel a honosított hello sablon paraméterek toohello `SendNotificationRESTAPI` hello az oktatóanyag előző meghatározott metódus.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct hello messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated hello token toobe used in hello authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create hello request tooadd hello template notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add hello category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send hello REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a>Következő lépések
Sablonok használatával kapcsolatos további információkért lásd:

* [Értesítse a felhasználókat a Notification hubs használatával: ASP.NET]
* [Értesítse a felhasználókat a Notification hubs használatával: Mobile Services]

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[legfrissebb hírek Notification Hubs használata toosend]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Értesítse a felhasználókat a Notification hubs használatával: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Értesítse a felhasználókat a Notification hubs használatával: Mobile Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications tooapp users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
