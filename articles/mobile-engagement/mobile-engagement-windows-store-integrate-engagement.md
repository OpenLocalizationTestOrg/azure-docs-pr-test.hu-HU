---
title: "aaaWindows univerzális alkalmazások Engagement SDK-integráció"
description: "Hogyan tooIntegrate Azure Mobile Engagement az univerzális Windows-alkalmazások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 71236b68-5ebd-44aa-8c82-c7ca8098ea05
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 18543798099c233dbe55cc387ba0216e369c8596
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a>Windows univerzális alkalmazások Engagement SDK-integráció
> [!div class="op_single_selector"]
> * [Univerzális Windows](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Ez az eljárás ismerteti a hello legegyszerűbb módja tooactivate Engagement elemzés és Monitorozási funkciók az univerzális Windows-alkalmazás.

a lépéseket követve hello a naplók elegendő tooactivate hello jelentés szükséges toocompute felhasználók, a munkamenetek, a tevékenységek, az összeomlásokat és a Technicals minden statisztikai adatok. hello jelentés naplók szükséges toocompute más statisztika hasonló eseményeket, hibákat és feladatokat kell elvégezni hello Engagement API segítségével manuálisan (lásd: [hogyan toouse hello speciális a Mobile Engagement az univerzális Windows-alkalmazás API címkézés](mobile-engagement-windows-store-use-engagement-api.md) óta Ezek a statisztikákat a függő alkalmazást.

## <a name="supported-versions"></a>Támogatott verziók
Mobile Engagement SDK Windows Universal alkalmazásokkal való hello csak integrálhatók a Windows-futtatókörnyezet és célzó univerzális Windows Platform-alkalmazások:

* Windows 8
* Windows 8.1
* Windows Phone 8.1
* Windows 10 (asztali és mobil családok)

> [!NOTE]
> Ha a Windows Phone Silverlight céloz meg, majd tekintse meg a toohello [Windows Phone Silverlight-integráció eljárás](mobile-engagement-windows-phone-integrate-engagement.md).
> 
> 

## <a name="install-hello-mobile-engagement-universal-apps-sdk"></a>Hello Mobile Engagement univerzális alkalmazások SDK telepítése
### <a name="all-platforms"></a>Összes platform
Mobile Engagement SDK az univerzális Windows-alkalmazás hello nevű Nuget-csomagként érhető el *MicrosoftAzure.MobileEngagement*. A Visual Studio Nuget-Csomagkezelő hello telepítheti.

### <a name="windows-8x-and-windows-phone-81"></a>A Windows 8.x és Windows Phone 8.1
NuGet automatikusan telepíti a hello SDK erőforrások hello `Resources` mappát a hello a projekt gyökerében.

### <a name="windows-10-universal-windows-platform-applications"></a>Windows 10 univerzális Windows Platform-alkalmazások
NuGet nem automatikus központi telepítése még az UWP-alkalmazás hello SDK-erőforrások. Az erőforrások telepítése amíg manuálisan újra bevezették NuGet toodo közül választhat:

1. Nyissa meg a Fájlkezelőben.
2. Keresse meg a következő helyen toohello (**x.x.x** telepíti Engagement hello verziója): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\*  *x.x.x**\\content\win81*
3. A fogd és vidd hello **erőforrások** hello fájl explorer toohello a projekt gyökerében a Visual Studio mappát.
4. A Visual Studio válassza ki a projektet, és aktiválja a hello **megjelenítése minden fájl** ikonja felett hello **Megoldáskezelőben**.
5. Egyes fájlok nem szerepelnek a hello projekt. őket egyszerre kattintson jobb gombbal a hello tooimport **erőforrások** mappa, **zárja ki a projekt** majd egy másik kattintson jobb gombbal a hello **erőforrások** mappa, **belefoglalása a projekt** toore-hello egész mappa tartalmazza. Minden fájl a hello **erőforrások** mappa mostantól beletartoznak a projekthez.

## <a name="add-hello-capabilities"></a>Hello képességek hozzáadása
hello Engagement SDK megfelelően kell néhány hello Windows SDK-t a rendelés toowork képességeit.

Nyissa meg a `Package.appxmanifest` fájlt, és ne feledje, hogy a következő képességeket hello deklarált:

* `Internet (Client)`

## <a name="initialize-hello-engagement-sdk"></a>Hello Engagement SDK inicializálása
### <a name="engagement-configuration"></a>Bevonási konfiguráció
hello Engagement konfigurációs rendszer központosított hello `Resources\EngagementConfiguration.xml` fájlt a projekt.

A fájl toospecify szerkesztése:

* Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.

Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

hello kapcsolati karakterlánc az alkalmazás megjelenik a klasszikus Azure portál hello.

### <a name="engagement-initialization"></a>Bevonási inicializálása
Amikor létrehoz egy új projektet egy `App.xaml.cs` fájl jön létre. Ez az osztály örökli `Application` és számos fontos metódust tartalmaz. Azt is használt tooinitialize hello Engagement SDK-t.

Módosítsa a hello `App.xaml.cs`:

* Adja hozzá a tooyour `using` utasításokat:
  
      using Microsoft.Azure.Engagement;
* Adja meg a metódus tooshare hello Engagement inicializálására egyszer minden hívásoknál:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* Hívás `InitEngagement` a hello `OnLaunched` módszert:
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* Az alkalmazás indításakor egyéni megjelenítve, egy másik alkalmazás vagy hello parancssori majd hello `OnActivated` metódust. Az alkalmazás aktiválásakor is kell tooinitiate hello Engagement SDK-t. tehát felülbírálása toodo `OnActivated` módszert:
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> Kifejezetten nem ajánlott, tooadd hello Engagement inicializálására az alkalmazás egy másik helyen.
> 
> 

## <a name="basic-reporting"></a>Alapvető jelentéskészítési
### <a name="recommended-method-overload-your-page-classes"></a>Ajánlott módszer: túlterhelés a `Page` osztályok
Rendelés tooactivate hello jelentésben az összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, egyszerűen végezheti el az összes a `Page` alosztályokat örökölhet hello `EngagementPage` osztályok.

Íme egy példa bemutatja, hogyan toodo Ez az alkalmazás egy lap. Mindent hello ugyanaz az alkalmazás összes lapja esetében.

#### <a name="c-source-file"></a>C# forrásfájl
A lap módosítása `.xaml.cs` fájlt:

* Adja hozzá a tooyour `using` utasításokat:
  
      using Microsoft.Azure.Engagement;
* Cserélje le `Page` rendelkező `EngagementPage`:

**Nélkül Engagement:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Az Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> Ha az oldala felülírja hello `OnNavigatedTo` módszer, lehet, hogy toocall `base.OnNavigatedTo(e)`. Ellenkező esetben hello tevékenység nem készíthető jelentés (hello `EngagementPage` hívások `StartActivity` belül a `OnNavigatedTo` metódus).
> 
> 

#### <a name="xaml-file"></a>XAML-fájl
A lap módosítása `.xaml` fájlt:

* Tooyour névtér-deklarációk hozzáadása:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* Cserélje le `Page` rendelkező `engagement:EngagementPage`:

**Nélkül Engagement:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Az Engagement:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-hello-default-behaviour"></a>Bírálja felül a hello alapértelmezett viselkedése
Alapértelmezés szerint hello osztálynév hello lap hello tevékenység nevét, nem extra akkor számít. Ha hello osztály hello "Lap" utótagot használ, Engagement is törlődik.

Ha toooverride hello alapértelmezett viselkedése hello neveként, egyszerűen adja hozzá a tooyour kódot:

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Ha azt szeretné tooreport néhány további informations a tevékenységet, a tooyour kódot is hozzáadhat:

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Ezek a módszerek is meghívhatók hello `OnNavigatedTo` módszer a lap.

### <a name="alternate-method-call-startactivity-manually"></a>Másodlagos módszer: hívja `StartActivity()` manuálisan
Ha nem, vagy nem toooverload a `Page` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent` módszerek közvetlenül.

Azt javasoljuk, hogy toocall `StartActivity` belül a `OnNavigatedTo` metódus a lap.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Gondoskodjon arról, hogy megfelelően a munkamenet befejezéséhez.
> 
> Univerzális Windows SDK hello automatikusan meghívja a hello `EndActivity` metódus hello alkalmazás bezárásakor. Így **magas** toocall hello ajánlott `StartActivity` metódus hello tevékenység hello felhasználó módosítja, amikor és túl**soha** hívás hello `EndActivity` módszer, ez a módszer küld tooEngagement kiszolgáló, hogy az aktuális felhasználó rendelkezik-e hagyja hello alkalmazás, a rendszer minden alkalmazásnaplók hatással van.
> 
> 

## <a name="advanced-reporting"></a>Speciális jelentéskészítés
Szükség esetén érdemes lehet tooreport adott eseményeket, hibákat és feladatokat, toodo így, használjon más módszereket található hello hello `EngagementAgent` osztály. hello Engagement API lehetővé teszi, hogy toouse Engagement speciális funkciók.

További információkért lásd: [hogyan toouse hello speciális a Mobile Engagement az univerzális Windows-alkalmazás API címkézés](mobile-engagement-windows-store-use-engagement-api.md).

## <a name="advanced-configuration"></a>Speciális konfiguráció
### <a name="disable-automatic-crash-reporting"></a>Automatikus összeomlási jelentések letiltása
Bármikor letilthatja hello automatikus összeomlási jelentéskészítési funkció bekapcsolása. Ezután amikor nem kezelt kivétel lép fel, Engagement nem semmit.

> [!WARNING]
> Ha azt tervezi, toodisable ezt a szolgáltatást, vegye figyelembe, hogy ha egy nem kezelt összeomlási történik, az alkalmazásban, Engagement nem küld hello összeomlási **és** hello munkamenet és feladatok nem zárható be.
> 
> 

toodisable automatikus összeomlási reporting, attól függően, hogy hello módja, deklarált, konfiguráció csak testreszabása:

#### <a name="from-engagementconfigurationxml-file"></a>A `EngagementConfiguration.xml` fájl
Állítsa be a jelentés túl összeomlik miattuk`false` közötti `<reportCrash>` és `</reportCrash>` címkék.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>A `EngagementConfiguration` futási időben objektum
Állítsa be a jelentés összeomlási toofalse a EngagementConfiguration objektum használatával.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Nagy sebességű átvitel
Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben. Ha az alkalmazás naplók nagyon gyakran, jelenti jobb toobuffer hello naplók és tooreport őket egyszerre egy rendszeres időközönként alap (Ez a lehetőség hello "kapacitásnövelés mód").

toodo tehát hello metódus hívása:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

hello argumentum értéke a **ezredmásodperc**. Bármikor Ha azt szeretné, hogy tooreactivate hello valós idejű naplózási, csak hívja hello metódus bármely paraméter nélkül vagy hello 0 értékkel.

hello kapacitásnövelés mód némileg növeli hello akkumulátor élettartama hello Engagement figyelő azonban hatással van: az összes munkamenetek és feladatok is lesznek kerekített toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható). Az ajánlott toouse egy mint 30000 (30s) kapacitásnövelés küszöbértéket. Vegye figyelembe a mentett naplókat még korlátozott too300 elemek toobe rendelkezik. Ha a küldő túl hosszú néhány naplók elveszhet.

> [!WARNING]
> hello kapacitásnövelés küszöbértéke kisebb, mint 1s tooa időszak nem lehet konfigurálni. Ha úgy próbálja toodo, hello SDK hello hiba: a nyomkövetési bemutatják és automatikusan toohello alapértelmezett értékét, azaz 0s alaphelyzetbe állítása. Ez akkor indul el, hello SDK tooreport hello naplók valós időben.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

