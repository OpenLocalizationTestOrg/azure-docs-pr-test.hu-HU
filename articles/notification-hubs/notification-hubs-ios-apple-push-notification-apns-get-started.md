---
title: "aaaSending leküldéses értesítések tooiOS az Azure Notification hubs használatával |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooan iOS-alkalmazás."
services: notification-hubs
documentationcenter: ios
keywords: "leküldéses értesítés,leküldéses értesítések,ios leküldéses értesítések"
author: ysxu
manager: erikre
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: d8bb47fee4c229b3ed2a7a4dbff25a56a7a7d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooios-with-azure-notification-hubs"></a>Az Azure Notification Hubs leküldéses értesítések tooiOS küldése
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).
> 
> 

Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooan iOS-alkalmazás. Létre fog hozni egy üres iOS-alkalmazást, amely leküldéses értesítéseket fogad hello segítségével [Apple Push Notification szolgáltatás (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató.

## <a name="before-you-begin"></a>Előkészületek
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

az oktatóanyag kódjának befejeződött hello található [a Githubon](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag hello következő szükséges:

* [Mobile Services iOS SDK 1.2.4-es]
* Az [Xcode] legújabb verziója
* Az iOS 8-cal (vagy újabb verzióval) kompatibilis eszköz
* Tagság az [Apple fejlesztői programjában](https://developer.apple.com/programs/).
  
  > [!NOTE]
  > A leküldéses értesítések konfigurációs követelményei miatt kell telepítenie és hello iOS-szimulátor helyett egy fizikai iOS-eszközön (iPhone vagy iPad) leküldéses értesítések teszteléséhez.
  > 
  > 

Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, iOS-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Az értesítési központ konfigurálása iOS leküldéses értesítésekhez
Ez a szakasz végigvezeti egy új értesítési központ létrehozása és konfigurálása a hitelesítés az apns-sel hello segítségével **.p12** létrehozott leküldéses tanúsítvány. Ha azt szeretné, hogy toouse egy már létrehozott értesítési központot, kihagyhatja toostep 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Hello kattintson <b>értesítési szolgáltatások</b> hello gombjára <b>beállítások</b> panelen, majd válassza ki <b>Apple (APNS)</b>. Kattintson a <b>tanúsítvány feltöltése</b> és select hello <b>.p12</b> korábban exportált fájl. Győződjön meg arról, hogy hello helyes jelszót is meg.</p>

<p>Győződjön meg arról, hogy tooselect <b>védőfal</b> módot, mivel ez a fejlesztés. Csak a hello használata <b>éles</b> Ha azt szeretné, hogy toosend leküldéses értesítések toousers akik megvásárolták az alkalmazást hello áruházból.</p>
</li>
</ol>
&emsp;&emsp;&emsp;&emsp;![APN szolgáltatás konfigurálása az Azure-portálon](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;&emsp;&emsp;![APNS-tanúsítvány konfigurálása az Azure Portalon](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

Az értesítési központ most már konfigurált toowork az apns-sel, és rendelkezik hello kapcsolati karakterláncok tooregister az alkalmazást, és leküldéses értesítések küldéséhez.

## <a name="connect-your-ios-app-toonotification-hubs"></a>Csatlakoztassa az iOS app tooNotification hubok
1. Az Xcode-ban hozzon létre egy új iOS-projektet, és válassza ki a hello **egyetlen nézetben alkalmazás** sablont.
   
    ![Xcode – Egynézetes alkalmazás][8]
    
2. Az új projekt hello-beállítások megadásakor, ellenőrizze, hogy toouse hello azonos **Terméknév** és **Organization Identifier** használhatók, ha az Apple Developer hello korábban beállítva hello alkalmazásköteg-azonosító portál.
   
    ![Xcode – projektbeállítások][11]
    
3. A **célok**, kattintson a projektnévre, majd a hello **Build Settings** lapra, és bontsa ki a **Code aláíró identitásának**, majd a **Debug**, állítsa be a kódaláíró identitását. Váltás **szintek** a **alapvető** túl**összes**, és állítsa be **Provisioning Profile** toohello létesítési profilt, amelyet korábban hozott létre .
   
    Ha nem látja az új létesítési profilt, amely az xcode-ban létrehozott hello, frissítse az aláíró identitása hello-profilok. Kattintson a **Xcode** hello menüsávjában kattintson **beállítások**, hello kattintson **fiók** lapra, majd hello **részleteinek megtekintése** gombra, majd a aláíró identitása, és kattintson a jobb alsó sarok hello hello frissítés gombra.
   
    ![Xcode – Létesítési profil][9]
4. Töltse le a hello [Mobile Services iOS SDK 1.2.4-es] és hello fájlt csomagolja ki. Az xcode-ban, kattintson jobb gombbal a projektre, majd kattintson a hello **fájlok hozzáadása a** beállítás tooadd hello **WindowsAzureMessaging.framework** mappa tooyour Xcode projekt. Válassza a **Copy items if needed** (Elemek másolása, ha szükséges) lehetőséget, majd kattintson az **Add** (Hozzáadás) gombra.
   
   > [!NOTE]
   > hello notification hubs SDK jelenleg nem támogatja a bitcode az Xcode 7.  Meg kell adni **Bitcode engedélyezése** túl**nem** a hello **Build beállítások** a projekthez.
   > 
   > 
   
    ![Az Azure SDK kicsomagolása][10]
5. Adja hozzá egy új fejléc fájl tooyour nevű projekt `HubInfo.h`. Ez a fájl tárolja majd hello állandókat az értesítési központban.  Adja hozzá a következő definíciókat hello, és cserélje le a hello szövegkonstans helyőrzőit a *központnév* és hello *DefaultListenSharedAccessSignature* korábban feljegyzett.
   
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter hello name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
6. Nyissa meg a `AppDelegate.h` fájlt adja hozzá a következő importálási irányelveket hello:
   
         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
7. Az a `AppDelegate.m file`, adja hozzá a következő kódot a hello hello `didFinishLaunchingWithOptions` metódus az iOS verziójától függően. Ez a kód regisztrálja az eszközleíróját az APNs szolgáltatással:
   
    iOS 8 esetén:
   
         UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
   
    Az iOS korábbi verziói-too8:
   
         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
8. A hello ugyanazt a fájlt, adja hozzá a következő módszerek hello. Ez a kód a hubinfo.h-ban megadott hello kapcsolati információk használatával toohello értesítési központ csatlakozik. Ezután megadja hello eszköz token toohello értesítési központot, így hello értesítési képes értesítéseket küldeni:
   
        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }
   
        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
9. A hello ugyanazt a fájlt, adja hozzá a következő metódus toodisplay hello egy **UIAlert** Ha hello értesítés érkezik, amíg aktív hello alkalmazást:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

1. Hozza létre és hello alkalmazást futtatnak az eszköz tooverify, hogy vannak-e hibák.

## <a name="send-test-push-notifications"></a>Teszt leküldéses értesítések küldése
Értesítések fogadásának az alkalmazásban való leküldéses értesítések küldése a hello tesztelheti [Azure Portal] keresztül hello **hibaelhárítás** hello hub panel szakasz (hello használata *Küldéstesztelése* lehetőséget).

![Azure portál – Küldés tesztelése][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-from-hello-app"></a>(Választható) Leküldéses értesítések küldéséhez hello alkalmazásból
> [!IMPORTANT]
> Ebben a példában a hello ügyfélalkalmazás értesítések küldésére szolgáló jelleggel valósul meg. Mivel ezt a beállítást hello `DefaultFullSharedAccessSignature` toobe hello ügyfélalkalmazás megtalálható, azt mutatja meg az értesítési központ toohello kockázata, hogy a felhasználó szerezhetnek hozzáférés nem engedélyezett toosend értesítések tooyour ügyfelek.
> 
> 

Ha azt szeretné, hogy toosend leküldéses értesítéseket az alkalmazáson belül, ez a témakör példa bemutatja, hogyan toodo a használatával hello REST-felületen.

1. Az Xcode-ban nyissa meg a `Main.storyboard` , és adja hozzá a felhasználói felületi összetevőket hello objektum könyvtár tooallow hello felhasználói toosend leküldéses értesítések hello alkalmazásban a következő hello:
   
   * Egy címke címkeszöveg nélkül. Az értesítések küldése használt tooreport hibák lesz. Hello **sorok** tulajdonságot kell beállítani, túl**0** , hogy automatikusan átméreteződik korlátozott toohello jobb és bal oldali margókhoz és hello hello nézet tetejéhez.
   * A szövegmező **helyőrző** szöveg túl beállítása**értesítési üzenet beírása**. A hozzáférési hello mezőt pont hello címke alább látható módon. Állítsa be a hello nézetvezérlő hello kilépő delegált.
   * Egy című gomb pont **értesítés küldése** hello szövegmező alá és vízszintesen középre hello korlátozott.
     
     hello nézet a következőképpen kell kinéznie:
     
     ![Xcode-tervező][32]
2. [Adja hozzá kimeneteket](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) a hello címke és a szöveges nézethez kapcsolt, és frissítse a `interface` definition toosupport `UITextFieldDelegate` és `NSXMLParserDelegate`. Adja hozzá a hello három tulajdonságdeklarációt alatt toohelp támogatási hello REST API hívása és hello-válasz elemzése.
   
    A ViewController.h fájlnak az alábbihoz kell hasonlítania:
   
        #import <UIKit/UIKit.h>
   
        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }
   
        // Make sure these outlets are connected tooyour UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;
   
        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;
   
        @end
3. Nyissa meg `HubInfo.h` , és adja hozzá a következő állandókat, amelyek az értesítésközpontról tooyour küldéséhez használható hello. Hello helyőrző karakterláncot cserélje le a tényleges *DefaultFullSharedAccessSignature* kapcsolati karakterláncot.
   
        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
4. Adja hozzá a következő hello `#import` utasítások tooyour `ViewController.h` fájlt.
   
        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"
5. A `ViewController.m` adja hozzá a következő kód toohello felhasználói felület megvalósításához hello. Ez a kód fogja elemezni a *DefaultFullSharedAccessSignature* kapcsolati karakterláncot. A hello [REST API-referenciában](http://msdn.microsoft.com/library/azure/dn495627.aspx), ezzel az elemzett információval lesz használt toogenerate egy SaS-jogkivonat hello **engedélyezési** kérelemfejlécet.
   
        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;
   
        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;
   
            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];
   
                @throw parseException;
            }
   
            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }
6. A `ViewController.m`, frissítés hello `viewDidLoad` metódus tooparse hello kapcsolati karakterlánc hello nézet betöltésekor. Az alábbi hello segédprogram-metódusokat is hozzáadhat toohello felhasználói felület megvalósításához.  

        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





1. A `ViewController.m`, adja hozzá a következő kód toohello illesztőfelület megvalósítása toogenerate hello SaS-hitelesítő jogkivonatának hello a biztosított hello **engedélyezési** fejléc, ahogyan az hello [REST API-n Hivatkozás](http://msdn.microsoft.com/library/azure/dn495627.aspx).
   
        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;
   
            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];
   
                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }
   
            return token;
        }
2. CTRL + a húzás hello **értesítés küldése** túl gomb`ViewController.m` tooadd nevű művelet **SendNotificationMessage** a hello **Touch Down** esemény. Frissítse a metódust a következő kód toosend hello értesítést hello REST API használatával hello.
   
        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }
   
        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];
   
            // Apple Notification format of hello notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];
   
            // Construct hello message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];
   
            // Generate hello token toobe used in hello authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create hello request tooadd hello APNs notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send hello REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                       [xmlParser parse];
                }
            }];
            [dataTask resume];
        }
3. A `ViewController.m`, adja hozzá a következő delegált metódust toosupport hello billentyűzet hello szövegmezőt a záró hello. CTRL + hello szöveg mező toohello nézetvezérlő ikonra hello felület Tervező tooset hello közben húzza megtekintése a vezérlő hello kilépő delegált aláírásával.
   
        //===[ Implement UITextFieldDelegate methods ]===
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
4. A `ViewController.m`, adja hozzá a hello következő delegálása módszerek toosupport elemzési hello válasz használatával `NSXMLParser`.
   
       //===[ Implement NSXMLParserDelegate methods ]===
   
       -(void)parserDidStartDocument:(NSXMLParser *)parser
       {
           self.statusResult = @"";
       }
   
       -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
           namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
           attributes:(NSDictionary *)attributeDict
       {
           NSString * element = [elementName lowercaseString];
           NSLog(@"*** New element parsed : %@ ***",element);
   
           if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
           {
               self.currentElement = element;
           }
       }
   
       -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
       {
           self.statusResult = [self.statusResult stringByAppendingString:
               [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
       }
   
       -(void)parserDidEndDocument:(NSXMLParser *)parser
       {
           // Set hello status label text on hello UI thread
           dispatch_async(dispatch_get_main_queue(),
           ^{
               [self.sendResults setText:self.statusResult];
           });
       }
5. Hello projekt buildjének elkészítéséhez, és győződjön meg arról, hogy nincsenek-e hibák.

> [!NOTE]
> Ha egy összeállítási hibát Xcode7 bitcode-támogatással kapcsolatos, módosítania kell a hello **Build Settings** > **engedélyezése Bitcode (ENABLE_BITCODE)** túl**nem** az xcode-ban. hello Notification Hubs SDK jelenleg nem támogatja a bitcode. 
> 
> 

Minden hello értesítés lehetséges hasznos adatot megtalálja az hello Apple [helyi és leküldéses értesítések programozásával foglalkozó útmutatójában].

## <a name="checking-if-your-app-can-receive-push-notifications"></a>Annak ellenőrzése, hogy az alkalmazás képes-e leküldéses értesítéseket fogadni
tootest leküldéses értesítések IOS rendszerű eszközökön, telepítenie kell a hello app tooa fizikai iOS-eszközön. Az Apple leküldéses értesítések hello iOS-szimulátor használatával nem lehet elküldeni.

1. Hello alkalmazás futtatását, és győződjön meg arról, hogy a regisztráció sikeres, és nyomja le az **OK**.
   
    ![iOS-alkalmazás leküldésesértesítés-regisztrációs tesztje][33]
2. Teszt leküldéses értesítést küldhet a hello [Azure Portal]fent leírtak szerint. Ha hozzáadott kódot hello alkalmazásban leküldéses értesítések küldéséhez, érintse meg hello szöveg mező tooenter egy értesítési üzenetet. Nyomja le az hello **küldése** gombra hello billentyűzet vagy hello **értesítés küldése** hello értesítési nézet toosend üdvözlőüzenetére gombjára.
   
    ![iOS-alkalmazás leküldésesértesítés-küldési tesztje][34]
3. hello leküldéses értesítést küld tooall eszközök regisztrált tooreceive hello értesítéseket hello adott értesítési központból.
   
    ![iOS-alkalmazás leküldésesértesítés-fogadási tesztje][35]

## <a name="next-steps"></a>Következő lépések
Ez az egyszerű példában küldött leküldéses értesítések tooall regisztrált iOS-eszközre. Azt javasoljuk, hogy folytatja-e toohello a tanulás következő lépéseként, [Azure Notification Hubs – felhasználók értesítése iOS rendszerhez .NET-háttérrendszerrel] oktatóanyag, amely végigvezeti a létrehozása olyan háttér toosend leküldéses értesítések címkék használatával. 

Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, továbbléphet a toohello [legfrissebb hírek Notification Hubs használata toosend] oktatóanyag. 

A Notification Hubs használatával kapcsolatban a [Notification Hubs használatával] foglalkozó témakörben tekinthet meg további általános információt.

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Services iOS SDK 1.2.4-es]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs használatával]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs – felhasználók értesítése iOS rendszerhez .NET-háttérrendszerrel]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[legfrissebb hírek Notification Hubs használata toosend]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[helyi és leküldéses értesítések programozásával foglalkozó útmutatójában]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com
