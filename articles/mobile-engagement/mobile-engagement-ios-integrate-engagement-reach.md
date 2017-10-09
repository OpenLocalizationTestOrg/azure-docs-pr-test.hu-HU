---
title: "a Mobile Engagement iOS SDK-integráció elérni aaaAzure |} Microsoft Docs"
description: "Legújabb frissítések és az Azure Mobile Engagement SDK iOS eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f5f5857-867c-40c5-9d76-675a343a0296
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 40c9bfbdb475ab0b97bdbc9cea798a59cb8a71ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-ios"></a>Hogyan tooIntegrate Engagement Reach IOS rendszerű eszközökön
Hello ismertetett hello integrációs eljárást kell követni, [hogyan tooIntegrate Engagement iOS dokumentumon](mobile-engagement-ios-integrate-engagement.md) Ez az útmutató követése előtt.

Ebben a dokumentációban XCode 8 igényel. Ha valóban függenek XCode 7, akkor előfordulhat, hogy használja hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Nincs egy ismert hiba az előző verzió iOS 10-eszközök futtatása közben: nincsenek műveletet kiváltó rendszerértesítéseket. toofix ezt követően tooimplement hello elavult API `application:didReceiveRemoteNotification:` az alkalmazás delegálása az alábbiak szerint:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Ez a megoldás nem ajánlott** , ez a viselkedés a jövőbeli (még akkor is kisebb) iOS verzió frissítése is módosítható, mivel az iOS API elavult. Amint lehetséges tooXCode 8 kell váltani.
>
>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a>Az alkalmazás tooreceive csendes leküldéses értesítések engedélyezése
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## <a name="integration-steps"></a>Integrációs lépések
### <a name="embed-hello-engagement-reach-sdk-into-your-ios-project"></a>Az iOS-projekthez az Engagement Reach SDK hello beágyazása
* Hello Reach sdk hozzáadása az Xcode projektjében. Az Xcode-ban lépjen túl**projekt \> tooproject hozzáadása** , és válassza a hello `EngagementReach` mappát.

### <a name="modify-your-application-delegate"></a>Az alkalmazás delegáltjának módosítása
* Hello felső a fájlt importálja az Engagement Reach modulját hello:

      [...]
      #import "AEReachModule.h"
* Módszer belül `applicationDidFinishLaunching:` vagy `application:didFinishLaunchingWithOptions:`, hozzon létre egy reach modult, és adja át tooyour Engagement meglévő inicializációs sorának:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]

        return YES;
      }
* Módosítsa **"icon.png"** hello kép neve, amelyet az értesítési ikon karakterlánc.
* Ha azt szeretné, hogy toouse hello beállítás *frissítés jelvény érték* Reach-kampányokat, vagy ha azt szeretné, toouse natív leküldéses \<SaaS/Reach API/kampány formátum/natív leküldéses\> rendszeren végrehajtott kampány segítségével hello Reach-modul kezelése hello jelvény ikon magát (emellett automatikusan törli a hello alkalmazás jelvény és is alaphelyzetbe állíthatja az Engagement által tárolt minden alkalommal, amikor hello alkalmazás elindítva, vagy foregrounded hello érték). Ehhez adja hozzá a következő sor Reach-modul inicializálása után hello:

      [reach setAutoBadgeEnabled:YES];
* Ha azt szeretné, hogy toohandle Reach adatleküldés, az alkalmazás delegáltjának felel meg a toohello segítségével `AEReachDataPushDelegate` protokoll. Adja hozzá a sor Reach-modul inicializálása után a következő hello:

      [reach setDataPushDelegate:self];
* Akkor is létrehozható hello módszerek `onDataPushStringReceived:` és `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` a az alkalmazás delegáltjának:

      -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
      {
         NSLog(@"String data push message with category <%@> received: %@", category, body);
         return YES;
      }

      -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
      {
         NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
         // Do something useful with decodedBody like updating an image view
         return YES;
      }

### <a name="category"></a>Kategória
hello kategória paraméter nem kötelező, amikor adatokat leküldéses kampány létrehozása, és lehetővé teszi, hogy toofilter adatok leküldéses értesítések. Ez akkor hasznos, ha azt szeretné, hogy toopush különböző a `Base64` adatok és a kívánt tooidentify típusuk őket elemzése előtt.

**Az alkalmazás most már készen áll a tooreceive és megjelenítési elérni a tartalom van.**

## <a name="how-tooreceive-announcements-and-polls-at-any-time"></a>Hogyan tooreceive mutató hirdetmények és szavazások bármikor
Engagement Reach értesítéseket küldhet tooyour a végfelhasználók bármikor hello Apple Push Notification szolgáltatás használatával.

tooenable ezt a funkciót, hogy lesz tooprepare az alkalmazás az Apple leküldéses értesítések és az alkalmazás delegáltjának módosítása.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Az Apple leküldéses értesítések az alkalmazás előkészítése
Kövesse a hello Útmutató: [hogyan tooPrepare az alkalmazás az Apple leküldéses értesítésekhez](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-hello-necessary-client-code"></a>Hello szükséges Ügyfélkód hozzáadása
*Az alkalmazás ezen a ponton hello Engagement előtér kell rendelkezniük a regisztrált Apple leküldéses tanúsítvány.*

Ha nem már elkészült, akkor tooregister az alkalmazás tooreceive leküldéses értesítéseket.

* Importálás hello `User Notification` keretrendszer:

        #import <UserNotifications/UserNotifications.h>
* Adja hozzá az alkalmazás indításakor a sorban következő hello (általában a `application:didFinishLaunchingWithOptions:`):

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

Ezt követően tooprovide tooEngagement hello eszköz jogkivonatát az Apple-kiszolgálók által visszaadott van szüksége. Ezt nevű hello metódust `application:didRegisterForRemoteNotificationsWithDeviceToken:` a az alkalmazás delegáltjának:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Végül tooinform hello Engagement SDK között, ha az alkalmazás távoli kapcsolatos értesítőt kap. Ezt követően hívható toodo hello metódus `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` a az alkalmazás delegáltjának:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [!IMPORTANT]
> Alapértelmezés szerint az Engagement Reach szabályozza hello completionHandler. Ha azt szeretné, hogy toomanually válaszoljon toohello `handler` letiltása a kódban, a hello üres átadhatók `handler` argumentum és vezérlés hello befejezési blokkolása magát. Lásd: hello `UIBackgroundFetchResult` típusát a lehetséges értékek listáját.
>
>

### <a name="full-example"></a>Teljes példa
Ez az integráció teljes példa:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter delegált ütközések feloldása

*Ha az alkalmazás és a harmadik féltől származó tárak egyike nem valósítja meg a `UNUserNotificationCenterDelegate` akkor kihagyhatja ezt a részt.*

A `UNUserNotificationCenter` delegált hello SDK toomonitor hello életciklusa az Engagement értesítések 10 vagy újabb IOS-es eszközökön használják. hello SDK rendelkezik saját hello végrehajtásának `UNUserNotificationCenterDelegate` protokoll, de lehet csak az egyik `UNUserNotificationCenter` alkalmazásonként delegálni. Bármely más delegált hozzáadott toohello `UNUserNotificationCenter` objektum hello egy bevonási ütközik. Ha a hello SDK a vagy bármely más harmadik fél delegált észlel, akkor nem fogja használni a saját megvalósítási toogive, egy alkalommal tooresolve hello ütközések. Hogy tooadd hello bevonási programot tooyour saját sorrendben delegált tooresolve hello ütközések.

Nincsenek két módon tooachieve ez.

Javaslat: 1, a delegált működve egyszerűen meghívja toohello SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Vagy javaslat 2, hello való öröklődés `AEUserNotificationHandler` osztály

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [!NOTE]
> Azt is meghatározhatja, hogy értesítést az engagement szolgáltatásból, vagy nem úgy, hogy rendelkezik-e a `userInfo` szótár toohello ügynök `isEngagementPushPayload:` metódus osztályban.

Győződjön meg arról, hogy hello `UNUserNotificationCenter` objektum delegált tooyour delegált belül vagy hello beállítása `application:willFinishLaunchingWithOptions:` vagy hello `application:didFinishLaunchingWithOptions:` az alkalmazás delegáltjának metódusában.
Például ha megvalósította a fent javaslat 1 hello:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="how-toocustomize-campaigns"></a>Hogyan toocustomize kampányok
### <a name="notifications"></a>Értesítések
Az értesítések két típusa van: rendszer és az alkalmazáson belüli értesítések.

Rendszer értesítések iOS kezeli, és nem szabható testre.

Alkalmazásbeli értesítéseket a toohello aktuális alkalmazás ablak dinamikusan hozzáadott nézetek. Ez a lehetőség egy értesítési területre. Értesítési átfedések rendkívül gyors integrációs, mivel azok nem szükséges, toomodify az alkalmazás nézeten.

#### <a name="layout"></a>Elrendezés
Az alkalmazásbeli értesítések toomodify hello megjelenését, egyszerűen módosíthatja hello fájlt `AENotificationView.xib` tooyour van szüksége, mindaddig, amíg megőrzi hello előfizetéscímkék értékeit és hello meglévő subviews típusú.

Alapértelmezés szerint az alkalmazásbeli értesítésekben üdvözlő képernyőt hello alján jelennek meg. Ha jobban szeret toodisplay azokat a képernyő, Szerkesztés hello hello tetejére megadott `AENotificationView.xib` , és módosítsa a hello `AutoSizing` tulajdonság hello fő nézet, így is megmarad a superview hello tetején.

#### <a name="categories"></a>Kategóriák
Ha módosítja a megadott elrendezés hello, az értesítések hello megjelenésének módosítása. Kategóriák engedélyezése toodefine, különböző megcélzott értesítések (valószínűleg viselkedések) keresi. A kategória a Reach-kampány létrehozásakor adható meg. Ne feledje, hogy kategóriák is segítségével testre szabható mutató hirdetmények és szavazások, a dokumentum későbbi szakaszában ismertetett.

az értesítések kategória leíró tooregister, kell tooadd hívása után hello reach-modul inicializálása.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`egy olyan objektum, amely megfelel a toohello protokoll példányának kell lennie `AENotifier`.

Megvalósíthat hello protokoll módszerek önállóan, vagy dönthet úgy tooreimplement hello meglévő osztály `AEDefaultNotifier` melyik már teljesít hello munka nagyobb része.

Például ha egy adott konkrét kategóriával tooredefine hello értesítési nézet használni szeretne, ebben a példában követheti:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Egyszerű példa a kategória azt feltételezik, hogy rendelkezik-e egy fájlt `MyNotificationView.xib` az alkalmazás fő kötegben. Ha hello metódus nem tudja a megfelelő toofind `.xib`, hello értesítés nem fog megjelenni, és Engagement kimeneteként hello konzolon üzenet.

a megadott hello nib fájl tiszteletben kell szabályainak hello:

* Csak egy nézetet kell tartalmaznia.
* Subviews hello azonos típusokat, hello kiépítettektől eltérő nevű fájlban található a megadott hello nib belül kell lennie`AENotificationView.xib`
* Subviews hello azonos címkéket hello megadott hello nib fájl nevű kiépítettektől eltérő módon kell rendelkeznie.`AENotificationView.xib`

> [!TIP]
> Csak a megadott hello nib fájlt, másolja `AENotificationView.xib`, és ott munka megkezdéséhez. De legyen óvatos, a nib fájl belső tartozik toohello osztály nézet hello `AENotificationView`. Ez az osztály újradefiniálása hello metódus `layoutSubViews` toomove és annak megfelelően toocontext subviews átméretezése. Érdemes lehet tooreplace azt egy `UIView` vagy egyéni nézet osztály.
>
>

Ha módosítania kell az értesítések (Ha azt kívánja, például tooload a nézet közvetlenül a kódból hello) mélyebb testreszabása, ajánlott hello tekintse meg a kódot és az osztály dokumentációban forrás megadott tootake `Protocol ReferencesDefaultNotifier` és `AENotifier`.

Vegye figyelembe, hogy is használhat ugyanazon bejelentő több kategóriákban hello.

A kiválasztott is hello alapértelmezett bejelentő ehhez hasonló a következőket teheti:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Értesítés kezelése
Hello alapértelmezett kategóriát használatakor a hello egyes életciklus metódusok nevezik `AEReachContent` objektum tooreport statisztikák és a frissítés hello kampány állapota:

* Alkalmazás hello értesítés jelenik meg, amikor hello `displayNotification` módszer neve (amely statisztikáit) által `AEReachModule` Ha `handleNotification:` adja vissza `YES`.
* Ha hello értesítési eltűnése hello `exitNotification` metódus lehívásra kerül, statisztika jelez, és tovább kampányok most már tudja feldolgozni.
* Ha hello értesítési kattint, `actionNotification` van nevű statisztika jelez, és hello tartozó művelet elvégzése.

Ha a végrehajtásának `AENotifier` Mellőzés hello alapértelmezett viselkedés telepítette, toocall életciklus módszerekhez önállóan. hello a következő példák bemutatják, egyes esetekben, ahol hello alapértelmezett viselkedés elmarad:

* Nem terjeszti ki `AEDefaultNotifier`, pl. megvalósította a teljesen új kategória kezelését.
* Ön overrode `prepareNotificationView:forContent:`, nem lehet rövidebb meg arról, hogy toomap `onNotificationActioned` vagy `onNotificationExited` tooone a U.I vezérlők.

> [!WARNING]
> Ha `handleNotification:` jelez kivétel, hello tartalom törlése és `drop` van neve, ez statisztika jelez, és tovább kampányok most már tudja feldolgozni.
>
>

#### <a name="include-notification-as-part-of-an-existing-view"></a>Egy meglévő nézethez részeként értesítési tartalmazza
Átfedések ideális az egy gyors integrációt, de lehet néha nem kényelmes, vagy nemkívánatos hatásai lehetnek.

Ha nem elégedett hello átfedő rendszer egyes a nézetek, testre szabhatja, ezek a nézetek.

Dönthet úgy is tooinclude az értesítési elrendezés a meglévő nézetekben. toodo Igen, van két megvalósítási stílusok:

1. Adja hozzá a felület builder használatával hello értesítési nézet

   * Nyissa meg *jelentéskészítő csatoló*
   * Helyezze a 320 x 60 (vagy ha iPad 768 x 60) `UIView` hello értesítési tooappear, ahová
   * Ebben a nézetben hello címke értékének túl beállítása: **36822491**
2. Programozott módon vegye fel a hello értesítési nézet. A nézet inicializálásakor a következő kód hello hozzá:

       UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values tooyour needs.
       notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
       [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`makró található `AEDefaultNotifier.h`.

> [!NOTE]
> hello alapértelmezett bejelentő automatikusan észleli, hogy hello értesítési elrendezés Ez a nézet tartalmazza, és nem adja hozzá egy átmeneti területre az.
>
>

### <a name="announcements-and-polls"></a>Hirdetmények és szavazások esetén
#### <a name="layouts"></a>Elrendezés
Módosíthatja a hello fájlok `AEDefaultAnnouncementView.xib` és `AEDefaultPollView.xib` mindaddig, amíg megőrzi hello előfizetéscímkék értékeit és hello meglévő subviews típusú.

#### <a name="categories"></a>Kategóriák
##### <a name="alternate-layouts"></a>Alternatív elrendezések
Értesítések, például a hello kampány kategória lehet használt toohave alternatív elrendezések a hirdetmények és szavazások esetén.

toocreate bejelentés kategóriáját, ki kell terjesztenie **AEAnnouncementViewController** , és regisztrálhatja azt hello reach-modul inicializálása után:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [!NOTE]
> Minden alkalommal, amikor a felhasználó fog kattintson a közlemény hello kategória az értesítés "a\_kategória", a regisztrált nézetvezérlő (ebben az esetben `MyCustomAnnouncementViewController`) fog hello metódus meghívásával inicializálható `initWithAnnouncement:` és hello lesz a hozzáadott toohello aktuális alkalmazás ablak.
>
>

Hello példányában `AEAnnouncementViewController` tooread hello tulajdonság követően osztály `announcement` tooinitialize a subviews. Tekintse meg az alábbi, hello példát, ahol két címkék használatával inicializálása `title` és `body` hello tulajdonságainak `AEReachAnnouncement` osztály:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Ha nem szeretné, hogy tooload a nézetek önállóan, de csak kívánt tooreuse hello alapértelmezett bejelentés nézet elrendezését, egyszerűen elvégezheti a egyéni nézetvezérlő kiterjeszti a megadott hello osztály `AEDefaultAnnouncementViewController`. Ebben az esetben a hello nib fájl ismétlődő `AEDefaultAnnouncementView.xib` , és nevezze át, az egyéni nézetvezérlő betölthető legyen (nevű tartományvezérlő `CustomAnnouncementViewController`, meg kell hívnia a nib fájl `CustomAnnouncementView.xib`).

tooreplace hello alapértelmezett kategória hirdetmények, egyszerűen regisztrálja a hello kategória definiált egyéni nézetvezérlő `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Szavazások lehet testreszabott hello ugyanúgy:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Az idő, a megadott hello `MyCustomPollViewController` kell terjesztenie `AEPollViewController`. Vagy tooextend hello alapértelmezett vezérlő közül választhat: `AEDefaultPollViewController`.

> [!IMPORTANT]
> Ne feledkezzen meg vagy toocall `action` (`submitAnswers:` az egyéni lekérdezési nézet vezérlők) vagy `exit` metódus hello nézetvezérlő eltűnése előtt. Ellenkező esetben statisztika nem küldhető el (azaz nem elemzés hello kampány), és további fontos következő kampányok nem kap értesítést hello alkalmazás folyamatának újraindításáig.
>
>

##### <a name="implementation-example"></a>Megvalósítási példa
Ebben a megvalósításban hello egyéni bejelentés nézet be van töltve egy külső xib fájlból.

Például a speciális értesítési testreszabáshoz ajánlott: hello forráskódját hello szabványos implementációjától toolook.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
