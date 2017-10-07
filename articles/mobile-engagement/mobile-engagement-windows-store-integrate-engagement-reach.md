---
title: "aaaWindows univerzális alkalmazások Reach SDK-integráció"
description: "Hogyan tooIntegrate az Azure Mobile Engagement jut, ahol a univerzális Windows-alkalmazások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a31ca1d6-856f-4aec-898a-07969ae5f7ec
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: af311c65940014083333853875a00173b8d6783e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a>Univerzális Windows-alkalmazások Reach SDK-integráció
Hello ismertetett hello integrációs eljárást kell követni, [Windows Universal Engagement SDK-integráció](mobile-engagement-windows-store-integrate-engagement.md) Ez az útmutató követése előtt.

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-universal-project"></a>A univerzális Windows-projektben Engagement Reach SDK hello beágyazása
Nincs semmi tooadd. `EngagementReach`hivatkozások és erőforrások még a projektben.

> [!TIP]
> Testre szabhatja a hello található képek `Resources` mappa a projekt, különösen akkor hello márka ikon (adott alapértelmezett toohello Engagement ikon). Univerzális alkalmazások is áthelyezheti hello `Resources` a benne lévő tartalom alkalmazásokat, de közötti lesz megosztott projekt tooshare mappájába tookeep hello `Resources\EngagementConfiguration.xml` alapértelmezett helyére, a fájlt, mert függő platformra.
> 
> 

## <a name="enable-hello-windows-notification-service"></a>Hello Windows értesítési szolgáltatás engedélyezése
### <a name="windows-8x-and-windows-phone-81-only"></a>A Windows 8.x és Windows Phone 8.1 esetén
A sorrend toouse hello **Windows értesítési szolgáltatásával** (WNS néven) az a `Package.appxmanifest` fájlja `Application UI` kattintson a `All Image Assets` hello bal oldali botot mezőbe. Jobb listájának hello hello: `Notifications`, módosítsa `toast capable` a `(not set)` túl`(Yes)`.

### <a name="all-platforms"></a>Összes platform
Az alkalmazás tooyour Microsoft fiók és toohello engagement platform kell toosynchronize. Ez a fiók toocreate kell, vagy jelentkezzen be [windows fejlesztői központ](https://dev.windows.com). Miután, hozzon létre egy új alkalmazást, és található hello biztonsági AZONOSÍTÓT és a titkos kulcsot. A hello engagement előtér, nyissa meg az Alkalmazásbeállítás a `native push` , majd illessze be a hitelesítő adatait. Ezt követően kattintson a jobb gombbal a projektre, válassza `store` és `Associate App with hello Store...`. Egyszerűen tooselect hello alkalmazás rendelkezik létrehozása előtt toosynchronize.

## <a name="initialize-hello-engagement-reach-sdk"></a>Hello Engagement Reach SDK inicializálása
Módosítsa a hello `App.xaml.cs`:

* Helyezze be `EngagementReach.Instance.Init` után csak `EngagementAgent.Instance.Init` a a `InitEngagement` módszert:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  Hello `EngagementReach.Instance.Init` egy dedikált szálat futtat. Nem rendelkezik toodo azt saját maga.

> [!NOTE]
> Ha máshol használják a leküldéses értesítések a az alkalmazás, akkor túl van[megosztani a leküldéses csatorna](#push-channel-sharing) az Engagement Reach.
> 
> 

## <a name="integration"></a>Integráció
Bevonási kétféleképpen tooadd hello Reach alkalmazásbeli szalagok és közbeszúrt kínál a hirdetmények és szavazások az alkalmazásban: hello átfedő integrációs és hello webes nézetek manuális integráció. Mindkét megközelítés a hello ötvözze nem ugyanazon az oldalon.

sikerült hello két integrációs hello választást összesíthető ily módon:

* Hello átfedéses integráció úgy is dönthet, ha a lapok már örököl hello ügynök `EngagementPage`, csak cseréje kérdése `EngagementPage` által `EngagementPageOverlay` és `xmlns:engagement="using:Microsoft.Azure.Engagement"` által `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` a lapokon.
* Dönthet hello webes nézetek manuális integrációs Ha tooprecisely hely hello UI elérni a lapokon belül, vagy ha nem szeretné, hogy tooadd egy másik öröklési szintű tooyour lapokat. 

### <a name="overlay-integration"></a>Átfedéses integráció
hello Engagement átfedő dinamikusan kerülnek be a hello felhasználói felületi elemei toodisplay Reach-kampányokat szerepel a oldalon. Ha hello átfedő nem felelnek meg a elrendezést érdemes hello webes nézetek manuális integrációs helyette.

A .xaml fájlban változásával `EngagementPage` túl hivatkozik`EngagementPageOverlay`

* Tooyour névtér-deklarációk hozzáadása:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* Cserélje le `engagement:EngagementPage` rendelkező `engagement:EngagementPageOverlay`:

**A EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

**A EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Ezután a .cs fájlban a lap a címke `EngagementPageOverlay` helyett `EngagementPage` , majd importálja `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

* Cserélje le `EngagementPage` rendelkező `EngagementPageOverlay`:

**A EngagementPage:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**A EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


hello Engagement átfedő ad hozzá egy `Grid` elem felett a lap a elrendezés és a két hello álló `WebView` hello elemei egy szalag, és más hello közbeszúrt nézet egy hello.

Testre szabhatja a hello átfedő elemet közvetlenül a hello `EngagementPageOverlay.cs` fájlt.

### <a name="web-views-manual-integration"></a>Webes nézetek manuális integráció
Reach keresnek a lapokat a hello két `WebView` felelős hello Szalagcím és hello közbeszúrt nézet megjelenítése elemet. csak annyi teendője van, toodo tooadd hello e két `WebView` elemek valahol a lapokon Íme egy példa:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


Az ebben a példában hello `WebView` elemek felhőbe archivált toofit vannak a tároló, amely automatikusan újra méretezi őket képernyő elforgatásának vagy ablak méretének módosítása esetén.

> [!WARNING]
> Fontos tookeep hello azonos elnevezési `engagement_notification_content` és `engagement_announcement_content` a hello `WebView` elemek. Reach azonosítja azokat a nevük számára. 
> 
> 

## <a name="handle-datapush-optional"></a>Leíró datapush (nem kötelező)
Ha azt szeretné, hogy az alkalmazás toobe képes tooreceive Reach adatleküldések, két események tooimplement hello EngagementReach osztály van:

Hello App() konstruktor az App.xaml.cs fájlban adja hozzá:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };

            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Láthatja, hogy az egyes metódus értéket ad vissza egy logikai érték hello a visszahívás. Engagement küld egy visszajelzés tooits háttér-után hello adatleküldés terjesztéséhez. Hello visszahívási hamis értéket ad vissza, ha hello `exit` visszajelzés küldése lesz. Ellenkező esetben lesz `action`. Ha nincs visszahívás hello események, hello `drop` visszajelzés visszaadott tooEngagement.

> [!WARNING]
> Bevonási nem tud tooreceive Többszörösök visszajelzése van, az adatokat. Ha azt tervezi, tooset több kezelők egy olyan eseményre, vegye figyelembe, hogy hello visszajelzés felel meg toohello legutóbb elküldött. Ebben az esetben ajánlott tooalways értéket ad vissza hello zavaró visszajelzés rendelkező előtér-hello azonos érték tooavoid.
> 
> 

## <a name="customize-ui-optional"></a>(Választható) felhasználói felület testreszabása
### <a name="first-step"></a>Első lépés
Azt teszik toocustomize hello reach felhasználói felületén.

toodo úgy, hogy toocreate hello alosztályát `EngagementReachHandler` osztály.

**Mintakód:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Állítsa hello hello tartalmának `EngagementReach.Instance.Handler` mező található az egyéni objektum a `App.xaml.cs` hello osztály `App()` metódust.

**Mintakód:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> Alapértelmezés szerint az Engagement használja a saját végrehajtásának `EngagementReachHandler`.
> Nincs toocreate a saját, és ha így tesz, akkor nem kell toooverride minden metódus. hello alapértelmezés tooselect hello Engagement alapobjektum lesz.
> 
> 

### <a name="web-view"></a>Webes nézet
Alapértelmezés szerint a Reach hello beágyazott erőforrások hello DLL toodisplay hello értesítések és lapokat fogja használni.

teljes tooprovide testreszabási lehetőségek webes nézet csak használjuk. Ha azt szeretné, hogy toocustomize elrendezések, bírálja felül közvetlenül hello erőforrások fájlok `EngagementAnnouncement.html` és `EngagementNotification.html`. Bevonási kell az összes kódot `<body></body>` toorun megfelelően. Külső címkét adhat hozzá, de `engagement_webview_area`.

Azonban dönthet úgy is toouse saját erőforrásokat.

Ha szeretné felülbírálni az `EngagementReachHandler` az alosztály tootell Engagement toouse módszerek az elrendezést, de igénybe vehet gondot tooembedded hello engagement mechanizmus:

**Mintakód:**

            // In your subclass of EngagementReachHandler

            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Alapértelmezés szerint AnnouncementHTML van `ms-appx-web:///Resources/EngagementAnnouncement.html`. Azt jelenti, hogy hello html-fájlba (szöveges közlemény, webes anoucement és lekérdezési közlemény) leküldéses üzenet hello tartalmának tervezése. AnnouncementName van `engagement_announcement_content`. Az xaml-oldal hello webes nézet tervéhez hello nevét is.

NotfificationHTML van `ms-appx-web:///Resources/EngagementNotification.html`. Azt jelenti, hogy hello html-fájl, amely a leküldéses üzenet hello értesítési tervezése. NotfificationName van `engagement_notification_content`. Az xaml-oldal hello webes nézet tervéhez hello nevét is.

### <a name="customization"></a>Testreszabás
Értesítés és a webes nézet azt szeretné, hogy ha megőrizheti a bevonási objektum rendelkezik közlemény személyre is szabhatja. Mi gondoskodunk, hogy webes nézet objektum ismertetett három alkalommal – hello először az XAML-kódban, második időben hello "setwebview()" metódusban a .cs fájlban, és harmadik idő hello HTML-fájlban.

* Az xaml hello aktuális grafikus elrendezés webes nézet összetevő ismerteti.
* A .cs fájlban definiálhat "setwebview()" beállította hello két webes nézet (értesítés, bejelentés) hello dimenziója. Akkor nagyon hatékony hello alkalmazás átméretezésekor.
* Hello Engagement HTML-fájlban azt leíró hello webes nézet tartalom, tervezési és hello elemek pozíciók egymás között.

### <a name="launch-message"></a>Indítsa el az üzenet
Ha a felhasználó kattint, a rendszer értesítést (egy bejelentési), Engagement elindítja hello alkalmazást, hello hello tartalmának betöltése leküldéses üzenetek, hello megfelelő kampány hello lap megjelenítéséhez.

Nincs késleltetés hello indítási hello alkalmazás és hello megjelenítés hello lap (attól függően, hogy a hálózat sebességétől hello) között.

valami tölt toohello felhasználói tooindicate, kell biztosítania egy visual adatok, például egy folyamatjelző vagy egy folyamatjelző. Bevonási nem tudja kezelni, hogy saját magát, de itt néhány kezelők meg.

tooimplement hello visszahívási "Nyilvános App() megkeresése" App.xaml.cs fájlban adja hozzá:

            /* hello application has launched and hello content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

            /* hello application has finished loading hello content and hello page
             * is about toobe displayed.
             * You should hide hello indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

            /* hello content has been loaded, but an error has occurred.
             * You can provide an information toohello user.
             * You should hide hello indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Hello visszahívási állíthatja be a "Nyilvános App() {}" metódusában a `App.xaml.cs` fájl, lehetőleg előtt hello `EngagementReach.Instance.Init()` hívható meg.

> [!TIP]
> Minden egyes kezelő felhasználói felület szálából hello hívja. Önnek nincs tooworry MessageBox vagy valami felhasználói felület kapcsolatos használatakor.
> 
> 

## <a id="push-channel-sharing"></a>Leküldéses csatorna megosztása
Használata leküldéses értesítések más célra az alkalmazás ezután rendelkezik toouse hello leküldéses csatorna megosztása a hello Engagement SDK szolgáltatás. Ez az elmulasztott tooavoid leküldéses.

* Megadhatja a saját leküldéses csatorna toohello Engagement Reach inicializálása. hello SDK helyett egy új kérő használja.

Frissítse a leküldéses csatorna hello hello Engagement Reach inicializálási `InitEngagement` hello metódusnak `App.xaml.cs` fájlt:

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* Azt is megteheti Ha csak tooconsume hello leküldéses csatorna hello Reach inicializálás után majd beállíthatja egy visszahívási Engagement Reach tooget hello leküldéses csatornán hello SDK által létrehozása után.

Állítsa be a visszahívási bárhol **után** Reach inicializálási hello:

    /* Set action on hello SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* hello forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Egyéni séma tipp
Azt adja meg az egyéni séma használja. A bevonási előtér toobe az engagement alkalmazásban használt URI különböző típusú küldhet. Alapértelmezett séma például `http, ftp, ...` vannak kezelése Windows, illetve egy ablak lesz kérdezzen rá, ha telepítve az eszközön alapértelmezés szerinti alkalmazás. Az alkalmazás a saját egyéni séma is létrehozhat.

hello egyszerűen tooset egy egyéni séma az alkalmazásban tooopen a `Package.appxmanifest` nyissa meg a `Declarations` panel. Válassza ki `Protocol` hello elérhető nyilatkozatok görgessen a mezőbe, majd vegye fel azt. Hello szerkesztése `Name` mező az új protokoll a kívánt nevet.

Most toouse Ez a protokoll szerkesztése a `App.xaml.cs` a hello `OnActivated` metódust, és ne feledkezzen meg is tooinitialize engagement itt:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action toodo when app is launch

              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;

                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion

