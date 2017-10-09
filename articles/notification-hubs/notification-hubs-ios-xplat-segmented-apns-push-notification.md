---
title: "aaaNotification hubok Megtörje hírek oktatóanyag – iOS"
description: "Megtudhatja, hogyan toouse Azure Service Bus Notification Hubs toosend megtörje hírek értesítések tooiOS eszközök."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 763b80b5ffed238b351d95bd3d6a96cb914f53cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a>Használja a Notification Hubs toosend legfrissebb hírek
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Áttekintés
Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toobroadcast breaking news értesítések tooan iOS-alkalmazás. Amikor végzett, akkor tudja tooregister számára megtörje hírek kategóriák érdekli, és ezen kategóriák csak leküldéses értesítések fogadásához. Ebben a forgatókönyvben számos alkalmazás általános felépítését, amelyben értesítések vannak küldött toobe toogroups, amely rendelkezik deklarálva érdeklődési rajtuk, például az RSS-olvasóval, az alkalmazások zene ventilátorok stb.

Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* hello értesítési központ regisztráció létrehozásakor. Ha az értesítések küldése tooa címke hello címke regisztrált minden eszköz hello értesítést kap. Mivel a címkékkel egyszerűen csak karakterláncok, nincs kiépítve előzetes toobe. Címkékkel kapcsolatos további információkért tekintse meg túl[Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Előfeltételek
Ebben a témakörben megfogalmazott célra épül létrehozott hello app [Ismerkedés a Notification Hubs][get-started]. Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs][get-started].

## <a name="add-category-selection-toohello-app"></a>Kategória kiválasztása toohello alkalmazás hozzáadása
első lépés hello tooadd hello felhasználói felületi elemek tooyour meglévő storyboard, amelyek lehetővé teszik a hello felhasználói tooselect kategóriák tooregister. felhasználó által kijelölt hello kategóriák hello eszközön tárolja. Hello alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ kijelölt hello kategóriákhoz jön létre.

1. Adja hozzá a MainStoryboard_iPhone.storyboard hello összetevők hello objektum könyvtárból a következő:
   
   * A címkék "Megtörje hírek" szóra,
   * Címkék kategória szöveget a "World", "Politika", "Vállalati", "Technológia", "Tudományos", "Sport",
   * Egy kategóriát, egy hat kapcsolók beállítása minden kapcsoló **állapot** toobe **ki** alapértelmezés szerint.
   * Egy gomb "Előfizetés" címkével
     
     A storyboard fájlt a következőképpen kell kinéznie:
     
     ![][3]
2. Hello Segéd-szerkesztőben kimeneteket összes hello kapcsolók létrehozása, és hívja meg őket "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"
3. Az "előfizetés" gombra a művelet létrehozása. A ViewController.h hello következőket kell tartalmaznia:
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. Hozzon létre egy új **Cocoa Touch osztály** nevű `Notifications`. Másolja a következő kód hello fájl Notifications.h hello felület szakaszában hello:
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. Adja hozzá a következő importálási irányelv tooNotifications.m hello:
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. A következő kód hello implementation szakaszban hello fájl Notifications.m hello másolja.
   
        SBNotificationHub* hub;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{
   
            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];
   
            return self;
        }
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
   
            [self subscribeWithCategories:categories completion:completion];
        }

        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Ez az osztály toostore helyi tárhelyet használ, és az eszköz fogadó hírek hello kategóriák lekérdezése. Tartalmaz egy metódus tooregister ezen kategóriák segítségével az is, egy [sablon](notification-hubs-templates-cross-platform-push-messages.md) regisztrációs.

1. Hello AppDelegate.h fájlban adja hozzá az importálási utasítást Notifications.h és hello értesítések osztály egy példányának tulajdonság hozzáadása:
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. A hello **didFinishLaunchingWithOptions** metódus AppDelegate.m, a hello kód tooinitialize hello értesítések példány hozzáadása hello metódus hello elején.  
   
    `HUBNAME`és `HUBLISTENACCESS` (hubinfo.h-ban meghatározott) már rendelkezik hello `<hub name>` és `<connection string with listen access>` helyőrzők helyett az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultListenSharedAccessSignature*korábban beszerzett
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > Ügyfél alkalmazáshoz elosztott hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni figyelési hozzáférési kulcs hello ügyfél alkalmazása. Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás tooregister az értesítéseket, de a meglévő regisztrációját nem lehet módosítani, és értesítések nem küldhető el. hello teljes körű hozzáférési kulcsot használnak a következő biztonságos háttérszolgáltatás értesítések küldését, és meglévő regisztrációk módosítása.
   > 
   > 
3. A hello **didRegisterForRemoteNotificationsWithDeviceToken** metódus AppDelegate.m, a hello kód hello metódusban cserélje le a következő kód toopass hello token toohello értesítések eszközosztályt hello. hello értesítések osztály fogja elvégezni az értesítések küldése a hello kategóriák regisztrálásakor hello. Hello felhasználói kategória beállításokat módosítja, ha hívása hello `subscribeWithCategories` metódus a válasz toohello **előfizetés** gomb tooupdate őket.
   
   > [!NOTE]
   > Hello eszköz jogkivonatát hello Apple Push Notification (APN) szolgáltatás által hozzárendelt is alkalommal bármikor, mert regisztrálnia kell az értesítésekhez gyakran tooavoid értesítés sikertelen. Ebben a példában regisztrál az értesítési hello alkalmazás minden indításakor. Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációs toopreserve sávszélesség Ha kevesebb mint egy nappal hello előző regisztráció óta eltelt.
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves hello categories from local storage and requests a registration for these categories
        // each time hello app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    Vegye figyelembe, hogy ezen a ponton kell nincs más kód a hello **didRegisterForRemoteNotificationsWithDeviceToken** metódust.

1. hello következőkkel már jelen kell lenniük AppDelegate.m hello befejezését [Ismerkedés a Notification Hubs] [ get-started] oktatóanyag.  Ha nem, adja hozzá.
   
    -(void) MessageBox:(NSString *) cím üzenet:(NSString *) ÜzenetSzövege {
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    }
   
   * (a "void") alkalmazás:(UIApplication *) alkalmazás didReceiveRemoteNotification: (NSDictionary *) userInfo {NSLog (@"% @", userInfo);   [a saját MessageBox:@"Notification" üzenetet: [valueForKey:@"alert [userInfo objectForKey:@"aps"]"]]; }
   
   Ez a metódus kezeli a bejelentések hello alkalmazás futtatásakor egy egyszerű megjelenítésével **UIAlert**.
2. A ViewController.m, adjon hozzá egy importálási utasítást, a következő kódot a hello AppDelegate.h, és másolja hello XCode által létrehozott **előfizetés** metódust. Ez a kód frissíti a hello értesítési regisztrációs toouse hello új kategória címkék hello felhasználó által választott hello felhasználói felületen.
   
       ```
       #import "Notifications.h"
       ```
   
       NSMutableArray* categories = [[NSMutableArray alloc] init];
   
       if (self.WorldSwitch.isOn) [categories addObject:@"World"];
       if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
       if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
       if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
       if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
       if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];
   
       Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];
   
       [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
           if (!error) {
               [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
           } else {
               NSLog(@"Error subscribing: %@", error);
           }
       }];
   
   Ezzel a módszerrel hoz létre egy **NSMutableArray** a kategóriák és a hello **értesítések** osztály toostore hello listájában hello helyi tároló, és regisztrál hello megfelelő címkéket az értesítési központban. Kategóriák megváltozásakor hello regisztrációs hello új kategóriák újra létrejön.
3. ViewController.m, adja hozzá a következő kódot a hello hello **viewDidLoad** metódus tooset hello felhasználói felülete a korábban mentett hello kategóriák alapján.

        // This updates hello UI on startup based on hello status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



hello app most tárolhat kategóriák készlete hello eszköz használt helyi tárhely tooregister hello értesítési központban amikor hello alkalmazás indítása.  hello felhasználói kategóriák futásidőben hello beállítás módosítható, és kattintson hello **előfizetés** metódus tooupdate hello regisztrációs hello eszközhöz. A következő később frissíteni hello app toosend hello híreket tartalmazó értesítések megtörje közvetlenül a hello alkalmazás funkcióit.

## <a name="optional-sending-tagged-notifications"></a>(választható) Címkézett értesítések küldése
Ha még nem rendelkezik hozzáféréssel tooVisual Studio, toohello következő szakaszt kihagyhatja, és értesítések küldése hello alkalmazás. Hello megfelelő sablon értesítési is elküldheti a hello [klasszikus Azure portál] az értesítési központ hibakeresési lapján hello használatával. 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-hello-device"></a>(választható) Értesítések küldése hello eszköz
Általában akkor kell értesítéseket egy háttér-szolgáltatás, de hello alkalmazásból közvetlenül is elküldheti a legfrissebb híreket tartalmazó értesítések. toodo ez frissítjük a hello `SendNotificationRESTAPI` módszerrel, amely a hello meghatározott [Ismerkedés a Notification Hubs] [ get-started] oktatóanyag.

1. A frissítés hello ViewController.m `SendNotificationRESTAPI` módszert az követi, hogy egy hello kategória címke paramétert fogad és küld a megfelelő hello [sablon](notification-hubs-templates-cross-platform-push-messages.md) értesítést.
   
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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];
   
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
2. A frissítés hello ViewController.m **értesítés küldése** művelet a következő hello kódban látható módon. Hogy az egyes címkék használatával külön-külön hello értesítések küldéséhez, és toomultiple platformok küldése.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send hello message as breaking news for each category tooWNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



1. A projekt újraépítéséhez, és győződjön meg arról, hogy nincs összeállítási hiba.

## <a name="run-hello-app-and-generate-notifications"></a>Hello alkalmazás futtatását, és értesítések
1. Nyomja le az hello futtassa gomb toobuild hello projektet, és indítsa el hello alkalmazást. Jelölje ki az egyes breaking news beállítások toosubscribe tooand, majd nyomja le az ENTER hello **előfizetés** gombra. Már előfizetett értesítések hello utaló üzenet jelenik meg.
   
    ![][1]
   
    Ha úgy dönt, **előfizetés**, app alakítja hello kiválasztott kategóriák hello címkék be, és egy új eszközök regisztrációja kijelölt hello címkék hello értesítési központ érkező kérelmeket.
2. Adjon meg egy elküldött, legfrissebb hírek nyomja meg az üdvözlő üzenet toobe **értesítés küldése** gombra. Alternatív megoldásként futtassa hello .NET konzol app toogenerate értesítések.
   
    ![][2]
3. Minden eszköz előfizetett toobreaking hírek hello legfrissebb híreket tartalmazó értesítések imént a telefonjára küldött fog kapni.

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag azt megtanulta, hogyan toobroadcast legfrissebb hírek kategória szerint. Vegye figyelembe, hogy befejezése hello oktatóprogramot kínál, amelyek más speciális Notification Hubs forgatókönyvek jelölje ki a következő egyikét:

* **[Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]**
  
    Ismerje meg, hogyan megtörje hírek app tooenable küldése tooexpand hello honosított értesítések.

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[klasszikus Azure portál]: https://manage.windowsazure.com
