---
title: "a Mobile Engagement iOS SDK-integráció aaaAzure |} Microsoft Docs"
description: "Legújabb frissítések és az Azure Mobile Engagement SDK iOS eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 947ea44b-00c1-450f-9a3b-74437954dc56
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 66ce34efabede7d882caa8a91431a8df71e4fb59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-ios"></a>Hogyan tooIntegrate Engagement IOS rendszerű eszközökön
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
>
>

Ez az eljárás ismerteti a hello legegyszerűbb módja tooactivate Engagement elemzés és Monitorozási funkciók az iOS-alkalmazásban.

hello Engagement SDK iOS7 + és Xcode 8 + van szükség: az alkalmazás központi telepítés célját hello verziójúnak kell lennie iOS 7-es.

> [!NOTE]
> Ha valóban függenek XCode 7, akkor előfordulhat, hogy használja hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Nincs egy ismert hiba hello Reach-modul korábbi verzióhoz iOS 10 eszközök lásd futtatott [reach-modul integrációs hello](mobile-engagement-ios-integrate-engagement-reach.md) további részleteket. Ha toouse hello SDK v3.2.4 válassza ki, akkor csak a hello kihagyása `UserNotifications.framework` hello következő lépésben importálni.
>
>

a lépéseket követve hello a naplók elegendő tooactivate hello jelentés szükséges toocompute felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals minden statisztikai adatok. hello jelentés naplók szükséges toocompute más statisztika hasonló eseményeket, hibákat és feladatokat kell elvégezni hello Engagement API segítségével manuálisan (lásd: [hogyan toouse hello speciális a Mobile Engagement az iOS-alkalmazás szerinti címkézését API](mobile-engagement-ios-use-engagement-api.md) óta statisztikák alkalmazás függenek.

## <a name="embed-hello-engagement-sdk-into-your-ios-project"></a>Az iOS-projekthez az Engagement SDK hello beágyazása
* Hello iOS SDK letöltése a [Itt](http://aka.ms/qk2rnj).
* Hozzáadás hello Engagement SDK tooyour iOS-projekt: az Xcode-ban kattintson a jobb gombbal a projekt, és válassza ki **"fájlok túl hozzáadása..."** , és válassza a hello `EngagementSDK` mappa.
* Bevonási szükséges további keretrendszerek toowork: hello project Explorer, nyissa meg a projekt ablaktáblán, és válassza ki a megfelelő cél hello. Ezután nyissa meg a hello **"Összeállítási fázisok"** lapon és a hello **"Bináris rendelkező Kódtárakon"** menüben adja hozzá a keretrendszereket:

  * `UserNotifications.framework`-készlet hello hivatkozásokat`Optional`
  * `AdSupport.framework`-készlet hello hivatkozásokat`Optional`
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> hello AdSupport keretrendszer távolítható el. Bevonási kell a keretrendszer toocollect hello idfa-JÁT. Azonban le kell tiltani IDFA-gyűjtést \<ios-sdk-engagement-idfa-ját\> toocomply hello új Apple házirenddel kapcsolatos ezt.
>
>

## <a name="initialize-hello-engagement-sdk"></a>Hello Engagement SDK inicializálása
Az alkalmazás delegáltjának kell toomodify:

* A fájlt a hello tetején hello Engagement ügynök importálása:

      [...]
      #import "EngagementAgent.h"
* Engagement inicializálására belül hello metódus "**applicationDidFinishLaunching:**"vagy"**alkalmazás: didFinishLaunchingWithOptions:**":

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a>Alapvető jelentéskészítési
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Ajánlott módszer: túlterhelés a `UIViewController` osztályok
Rendelés tooactivate hello jelentésben az összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, egyszerűen végezheti el az összes a `UIViewController` alosztályokat örökölhet hello `EngagementViewController` osztályok (ugyanaz a szabály a `UITableViewController`  - \> `EngagementTableViewController`).

**Nélkül Engagement:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Az Engagement:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Másodlagos módszer: hívja `startActivity()` manuálisan
Ha nem, vagy nem toooverload a `UIViewController` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent`tartozó módszerek közvetlenül.

> [!IMPORTANT]
> hello iOS SDK automatikusan meghívja a hello `endActivity()` metódus hello alkalmazás bezárásakor. Így *magas* toocall hello ajánlott `startActivity` metódus hello tevékenység hello felhasználó módosítja, amikor és túl*soha* hívás hello `endActivity` metódus kényszeríti a metódus hívása óta hello aktuális munkamenet toobe véget ért.
>
>

## <a name="location-reporting"></a>Helyi jelentéskészítés
Szolgáltatási feltételek Apple nem teszik lehetővé az alkalmazások toouse hely csak a statisztika célra nyomon követése. Ebből kifolyólag javasoljuk tooenable hely jelentések csak akkor, ha az alkalmazás is használhatnak hello más okból nyomon követése.

Az iOS 8-től kezdődően meg kell adnia egy hogyan alkalmazása használja-e helyszolgáltatás úgy, hogy egy karakterláncot hello kulcs leírását [NSLocationWhenInUseUsageDescription] vagy [NSLocationAlwaysUsageDescription]az alkalmazás Info.plist fájljában. Ha azt szeretné, hogy az Engagement hello háttérben tooreport helyét, adja hozzá a hello kulcs NSLocationAlwaysUsageDescription. Minden más esetben hello kulcsot NSLocationWhenInUseUsageDescription hozzáadni. Ne feledje, hogy NSLocationAlwaysAndWhenInUseUsageDescription és a NSLocationWhenInUseUsageDescription tooreport háttér hely IOS 11.

### <a name="lazy-area-location-reporting"></a>Helymeghatározást
Helymeghatározást lehetővé teszi, hogy tooreport hello ország, valamint régió és Helység társított toodevices. A hely jelentéskészítési típusú csak használ a hálózati helyek (cella azonosító vagy Wi-Fi alapján). hello eszköz terület munkamenetenként legfeljebb egyszer jelenti. hello GPS a rendszer nem használja, és ezáltal az ilyen típusú hely jelentés nagyon kevés (nem toosay nem) hello akkumulátor hatása.

Jelentett területek a következők felhasználók, munkamenetek, kapcsolódó események és hibák földrajzi statisztikája használt toocompute. Ezek használhatók a Reach-kampányokat feltételként. utolsó ismert lehet lekérdezése Köszönjük toohello jelentett terület hello [Device API-val].

tooenable Lusta terület helye reporting, adja hozzá a sor hello Engagement ügynök inicializálása után a következő hello:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Valós idejű hely jelentéskészítési
Valós idejű hely jelentéskészítési lehetővé teszi, hogy tooreport hello szélességi és hosszúsági társított toodevices. Alapértelmezés szerint a hely jelentéskészítési csak használja a hálózati helyek (cella azonosító vagy Wi-Fi alapján), és hello reporting csak aktív alkalmazás futásakor, hello előtérben (azaz egy munkamenetben).

Valós idejű helyek *nem* toocompute statisztika használt. A csak célja a geokerítések valós idejű tooallow hello használata \<Reach-célközönségre-geokerítések\> Reach-kampányokat a feltételnek.

tooenable valós idejű hely jelentéskészítési, adja hozzá a sor hello Engagement ügynök inicializálása után a következő hello:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS-alapú jelentés
Alapértelmezés szerint valós idejű helyét jelentő csak funkció alapú hálózati helyek. tooenable hello GPS alapuló (amelyek pontos sokkal) helyét, adja hozzá:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Háttér-jelentés
Alapértelmezés szerint valós idejű hely jelentéskészítési az csak aktív alkalmazás futásakor, hello előtérben (azaz egy munkamenetben). Adja hozzá a háttérben, is reporting tooenable hello:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> Hello alkalmazás fut a háttérben, amikor csak alapú hálózati helyek küld jelentést, még akkor is, ha engedélyezte a hello GPS.
>
>

Ez a függvény végrehajtása fel fogja hívni [startMonitoringSignificantLocationChanges] amikor az alkalmazás a hello háttér állapotba kerül. Vegye figyelembe, hogy azt fogja automatikusan indítsa újra az alkalmazás hello háttér Ha új helyet esemény érkezik.

## <a name="advanced-reporting"></a>Speciális jelentéskészítés
Nem kötelező, ha tooreport adott eseményeket, hibákat és feladatok, kell toouse hello Engagement API keresztül hello hello módszerek `EngagementAgent` osztály. Ez az osztály egy objektumának lekérhetők hívó hello `[EngagementAgent shared]` statikus metódus.

hello Engagement API lehetővé teszi, hogy toouse Engagement speciális funkciók és részleteit a hello hogyan tooUse az Engagement API IOS (valamint hello hello műszaki dokumentációját az `EngagementAgent` osztály).

## <a name="disable-idfa-collection"></a>Tiltsa le a IDFA-gyűjtést
Alapértelmezés szerint a bevonási használandó hello [idfa-JÁT] toouniquely felhasználó azonosításának. De ha máshol hirdetési hello alkalmazás nem használja, akkor előfordulhat, hogy elutasítja hello App Store felülvizsgálati folyamat. IDFA-gyűjtést letiltható hello előfeldolgozó makró hozzáadásával `ENGAGEMENT_DISABLE_IDFA` a pch fájlban (vagy a hello `Build Settings` az alkalmazás). Ezzel biztosíthatja, hogy nincs nem túl`ASIdentifierManager`, `advertisingIdentifier` vagy `isAdvertisingTrackingEnabled` hello alkalmazás építés.

Integráció a hello **prefix.pch** fájlt:

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Ellenőrizheti, hogy hello IDFA-gyűjtést megfelelően letiltotta az alkalmazás hello Engagement vizsgálati naplók ellenőrzése. Lásd: Integráció tesztelése hello\<ios-sdk-engagement-teszt-idfa-ját\> dokumentációja további információkat.

## <a name="disable-log-reporting"></a>Naplózási jelentések letiltásához
### <a name="using-a-method-call"></a>Metódushívások használatával
Ha azt szeretné, hogy a naplók küldése Engagement toostop, hívása:

    [[EngagementAgent shared] setEnabled:NO];

Ez a hívás az állandó: használ `NSUserDefaults` toostore hello információkat.

Engedélyezheti újra reporting működik az azonos hello meghívásával napló `YES`.

### <a name="integration-in-your-settings-bundle"></a>A beállítások nyalábban integráció
Helyett ezt a funkciót, integrálhatja ezt a beállítást a közvetlenül a meglévő `Settings.bundle` fájlt. karakterlánc hello `engagement_agent_enabled` kell használni egy hello preferencia azonosítót, és társított tooa váltása kapcsoló kell lennie (`PSToggleSwitchSpecifier`).

Példa következő hello `Settings.bundle` bemutatja, hogyan tooimplement azt:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Device API-val]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[idfa-JÁT]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
