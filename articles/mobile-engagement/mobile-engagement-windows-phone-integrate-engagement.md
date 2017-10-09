---
title: "aaaWindows Phone Silverlight Engagement SDK-integráció"
description: "Hogyan tooIntegrate Azure Mobile Engagement Windows Phone Silverlight-alkalmazásokhoz"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 447fea8d-f4e3-4ad4-8ec0-8e3cf1ad3ab0
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f65683a62e5256cea469a3a73d99ade4331cb6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight Engagement SDK-integráció
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Ez az eljárás ismerteti a hello legegyszerűbb módja tooactivate Azure Mobile Engagement elemzés és Monitorozási funkciók a Windows Phone Silverlight alkalmazás.

a lépéseket követve hello a naplók elegendő tooactivate hello jelentés szükséges toocompute felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals minden statisztikai adatok. hello jelentés naplók szükséges toocompute más statisztika hasonló eseményeket, hibákat és feladatokat kell elvégezni hello Engagement API segítségével manuálisan (lásd: [hogyan toouse hello speciális API szerinti címkézését a Windows Phone Silverlight-alkalmazást a Mobile Engagement](mobile-engagement-windows-phone-use-engagement-api.md) az alábbi) mert a statisztikák függő alkalmazás.

## <a name="supported-versions"></a>Támogatott verziók
Mobile Engagement SDK a Windows Silverlight hello csak integrálhatók célcsoport-kezelési alkalmazások:

* Windows Phone 8.0
* Windows Phone 8.1 Silverlight

> [!NOTE]
> Ha a Windows Phone 8.1 (nem Silverlight) céloz meg, majd tekintse meg a toohello [univerzális Windows-integrációs eljárás](mobile-engagement-windows-store-integrate-engagement.md).
> 
> 

## <a name="install-hello-mobile-engagement-silverlight-sdk"></a>Hello Mobile Engagement Silverlight SDK telepítése
Mobile Engagement SDK a Windows Silverlight hello nevű Nuget-csomagként érhető el *MicrosoftAzure.MobileEngagement*. A Visual Studio Nuget-Csomagkezelő hello telepítheti. 

## <a name="add-hello-capabilities"></a>Hello képességek hozzáadása
hello Engagement SDK megfelelően kell néhány rendelés toowork a Windows Phone Silverlight SDK hello képességeit.

Nyissa meg a `WMAppManifest.xml` fájlt, és ne feledje, hogy a következő képességeket hello táblákon hello `Capabilities` panel:

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-hello-engagement-sdk"></a>Hello Engagement SDK inicializálása
### <a name="engagement-configuration"></a>Bevonási konfiguráció
hello Engagement konfigurációs rendszer központosított hello `Resources\EngagementConfiguration.xml` fájlt a projekt.

A fájl toospecify szerkesztése:

* Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.

Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

hello kapcsolati karakterlánc az alkalmazás megjelenik a klasszikus Azure portál hello.

### <a name="engagement-initialization"></a>Bevonási inicializálása
Amikor létrehoz egy új projektet egy `App.xaml.cs` fájl jön létre. Ez az osztály örökli `Application` és számos fontos metódust tartalmaz. Azt is használt tooinitialize hello Engagement SDK-t.

Módosítsa a hello `App.xaml.cs`:

* Adja hozzá a tooyour `using` utasításokat:
  
      using Microsoft.Azure.Engagement;
* Helyezze be `EngagementAgent.Instance.Init` a hello `Application_Launching` módszert:
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* Helyezze be `EngagementAgent.Instance.OnActivated` a hello `Application_Activated` módszert:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> Kifejezetten nem ajánlott, tooadd hello Engagement inicializálására az alkalmazás egy másik helyen. Azonban figyelembe, hogy hello `EngagementAgent.Instance.Init` metódus egy dedikált szálon, és nem a felhasználói felület szálán hello futtatja.
> 
> 

## <a name="basic-reporting"></a>Alapvető jelentéskészítési
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Ajánlott módszer: túlterhelés a `PhoneApplicationPage` osztályok
Rendelés tooactivate hello jelentésben az összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, egyszerűen végezheti el az összes a `PhoneApplicationPage` alosztályokat örökölhet hello `EngagementPage` osztályok.

Íme egy példa bemutatja, hogyan toodo Ez az alkalmazás egy lap. Mindent hello ugyanaz az alkalmazás összes lapja esetében.

#### <a name="c-source-file"></a>C# forrásfájl
A lap módosítása `.xaml.cs` fájlt:

* Adja hozzá a tooyour `using` utasításokat:
  
      using Microsoft.Azure.Engagement;
* Cserélje le `PhoneApplicationPage` rendelkező `EngagementPage` :

**Nélkül Engagement:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Az Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> Ha az oldala örököl hello `OnNavigatedTo` módszer, gondos toolet hello kell `base.OnNavigatedTo(e)` hívható meg. Ellenkező esetben hello tevékenység nem fog szerepelni. Valóban hello `EngagementPage` hívja a következő `StartActivity` belül hello `OnNavigatedTo` metódust.
> 
> 

#### <a name="xaml-file"></a>XAML-fájl
A lap módosítása `.xaml` fájlt:

* Tooyour névtér-deklarációk hozzáadása:
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* Cserélje le `phone:PhoneApplicationPage` rendelkező `engagement:EngagementPage` :

**Nélkül Engagement:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Az Engagement:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-hello-default-behavior"></a>Hello alapértelmezett viselkedés felülbírálásához
Alapértelmezés szerint hello osztálynév hello lap hello tevékenység nevét, nem extra akkor számít. Ha hello osztály hello "Lap" utótagot használ, Engagement is törlődik.

Ha hello neve toooverride hello alapértelmezett viselkedését, egyszerűen adja hozzá a tooyour kódot:

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Ha tooreport néhány további információt szeretne a tevékenységet, a tooyour kódot is hozzáadhat:

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Ezek a módszerek is meghívhatók hello `OnNavigatedTo` módszer a lap.

### <a name="alternate-method-call-startactivity-manually"></a>Másodlagos módszer: hívja `StartActivity()` manuálisan
Ha nem, vagy nem toooverload a `PhoneApplicationPage` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent` módszerek közvetlenül.

Azt javasoljuk, hogy hívása `StartActivity` belül a `OnNavigatedTo` a PhoneApplicationPage metódusában.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> Gondoskodjon arról, hogy megfelelően a munkamenet befejezéséhez.
> 
> hello SDK automatikusan meghívja a hello `EndActivity` metódus hello alkalmazás bezárásakor. Így **magas** toocall hello ajánlott `StartActivity` metódus hello tevékenység hello felhasználó módosítja, amikor és túl**soha** hívás hello `EndActivity` metódust. Ez a módszer küld message toohello Engagement kiszolgáló, hogy hello aktuális felhasználó kilépett hello alkalmazást, és ez hatással van az összes alkalmazás naplóiban.
> 
> 

## <a name="advanced-reporting"></a>Speciális jelentéskészítés
Szükség esetén érdemes lehet tooreport adott eseményeket, hibákat és feladatokat, toodo így, használjon más módszereket található hello hello `EngagementAgent` osztály. hello Engagement API lehetővé teszi, hogy toouse Engagement speciális funkciók.

További információkért lásd: [hogyan toouse hello speciális API szerinti címkézését a Windows Phone Silverlight-alkalmazást a Mobile Engagement](mobile-engagement-windows-phone-use-engagement-api.md).

## <a name="advanced-configuration"></a>Speciális konfiguráció
### <a name="disable-automatic-crash-reporting"></a>Automatikus összeomlási jelentések letiltása
Bármikor letilthatja hello automatikus összeomlási jelentéskészítési funkció bekapcsolása. Ezután amikor nem kezelt kivétel lép fel, Engagement nem semmit.

> [!WARNING]
> Ha azt tervezi, toodisable ezt a szolgáltatást, vegye figyelembe, hogy ha egy nem kezelt összeomlási történik, az alkalmazásban, Engagement nem küld hello összeomlási **és** nem bezárul hello munkamenet és feladatok.
> 
> 

toodisable automatikus összeomlási reporting, attól függően, hogy hello módja, deklarált, konfiguráció csak testreszabása:

#### <a name="from-engagementconfigurationxml-file"></a>A `EngagementConfiguration.xml` fájl
Állítsa be a jelentés túl összeomlik miattuk`false` közötti `<reportCrash>` és `</reportCrash>` címkék.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>A `EngagementConfiguration` futási időben objektum
Állítsa be a jelentés összeomlási toofalse a EngagementConfiguration objektum használatával.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Nagy sebességű átvitel
Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben. Ha az alkalmazás naplók nagyon gyakran, jelenti jobb toobuffer hello naplók és tooreport őket egyszerre egy rendszeres időközönként alap (Ez a lehetőség hello "kapacitásnövelés mód").

toodo tehát hello metódus hívása:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

hello argumentum értéke a **ezredmásodperc**. Bármikor Ha azt szeretné, hogy tooreactivate hello valós idejű naplózási, csak hívja hello metódus bármely paraméter nélkül vagy hello 0 értékkel.

hello kapacitásnövelés mód némileg növeli hello akkumulátor élettartama hello Engagement figyelő azonban hatással van: az összes munkamenetek és feladatok is lesznek kerekített toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható). Az ajánlott toouse egy mint 30000 (30s) kapacitásnövelés küszöbértéket. Vegye figyelembe a mentett naplókat még korlátozott too300 elemek toobe rendelkezik. Ha a küldő túl hosszú néhány naplók elveszhet.

> [!WARNING]
> hello kapacitásnövelés küszöbértéke nem lehet konfigurálni tooa időszak kevesebb mint egy második. Ha úgy próbálja toodo, hello SDK hello hiba: a nyomkövetési bemutatják és automatikusan alaphelyzetbe toohello alapértelmezett érték, ez azt jelenti, hogy nulla másodperc. Ez akkor indul el, hello SDK tooreport hello naplók valós időben.
> 
> 

