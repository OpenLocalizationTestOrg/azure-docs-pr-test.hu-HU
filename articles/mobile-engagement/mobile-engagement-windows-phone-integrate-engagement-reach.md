---
title: "aaaWindows Phone Silverlight Reach SDK-integráció"
description: "Hogyan tooIntegrate az Azure Mobile Engagement jut, ahol a Windows Phone Silverlight-alkalmazásokhoz"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d3516a6b-db9f-4cdb-a475-4148edf81af1
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 09c8767216e11963c5c600755ab8d4d11cd92034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reach SDK-integráció
Hello ismertetett hello integrációs eljárást kell követni, [Windows Phone Silverlight Engagement SDK-integráció](mobile-engagement-windows-phone-integrate-engagement.md) Ez az útmutató követése előtt.

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>A Windows Phone Silverlight-projektben Engagement Reach SDK hello beágyazása
Nincs semmi tooadd. `EngagementReach`hivatkozások és erőforrások még a projektben.

> [!TIP]
> Testre szabhatja a hello található képek `Resources` mappa a projekt, különösen akkor hello márka ikon (adott alapértelmezett toohello Engagement ikon).
> 
> 

## <a name="add-hello-capabilities"></a>Hello képességek hozzáadása
hello Engagement Reach SDK-t kell néhány további képességeket.

Nyissa meg a `WMAppManifest.xml` fájlt, és ne feledje, hogy a következő képességeket hello deklarált:

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

hello először egy használják hello MPNS szolgáltatás tooallow hello megjelenítési bejelentési értesítés. hello második használatban tooembed hello SDK be egy böngésző feladatot.

Hello szerkesztése `WMAppManifest.xml` fájlt, és hello belül `<Capabilities />` címke:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-hello-microsoft-push-notification-service"></a>A Microsoft leküldéses értesítéseket kezelő szolgáltatásában hello engedélyezése
A sorrend toouse hello **a Microsoft leküldéses értesítéseket kezelő szolgáltatásában** (MPNS néven) a `WMAppManifest.xml` fájl rendelkeznie kell egy `<App />` rendelkező címke egy `Publisher` attribútum a projekt toohello nevének beállítása.

## <a name="initialize-hello-engagement-reach-sdk"></a>Hello Engagement Reach SDK inicializálása
### <a name="engagement-configuration"></a>Bevonási konfiguráció
hello Engagement konfigurációs rendszer központosított hello `Resources\EngagementConfiguration.xml` fájlt a projekt.

A fájl toospecify reach konfigurációjának a szerkesztésével:

* *Nem kötelező*, jelzik, hogy a natív leküldéses hello (MPNS) aktiválva van, vagy nem közötti `<enableNativePush>` és `</enableNativePush>` címkék, (`true` alapértelmezés szerint).
* *Nem kötelező*, hello leküldéses csatorna közötti hello nevét jelzi `<channelName>` és `</channelName>` címkék, adja meg a hello azonos, hogy jelenleg lehet-e használni az alkalmazást, vagy hagyja üresen.

Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether hello native push (MPNS) is activated or not. */

    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide hello same channel name that your application may currently use. */

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [!TIP]
> Megadhatja, hogy hello hello MPNS leküldéses csatorna az alkalmazás nevét. Alapértelmezés szerint az Engagement hello appId alapján nevét hoz létre. Hogy nincs szükség toospecify hello neve, kivéve, ha azt tervezi, toouse hello leküldéses csatorna Engagement kívül.
> 
> 

### <a name="engagement-initialization"></a>Bevonási inicializálása
Módosítsa a hello `App.xaml.cs`:

* Adja hozzá a tooyour `using` utasításokat:
  
      using Microsoft.Azure.Engagement;
* Helyezze be `EngagementReach.Instance.Init` után csak `EngagementAgent.Instance.Init` a `Application_Launching` :
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* Helyezze be `EngagementReach.Instance.OnActivated` a hello `Application_Activated` módszert:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> Hello `EngagementReach.Instance.Init` egy dedikált szálat futtat. Nem rendelkezik toodo azt saját maga.
> 
> 

## <a name="app-store-submission-considerations"></a>Tároló küldésének szempontjai
Microsoft néhány szabály írja elő, amikor hello leküldéses értesítésekkel:

A hello Microsoft [alkalmazás-házirendek] -dokumentáció, szakasz 2.9:

1) Meg kell kérnie hello felhasználói tooaccept tooreceive leküldéses értesítéseket. Ezután a beállítások egy módon toodisable hello leküldéses értesítések hozzáadása.

hello EngagementReach objektum biztosít két módszer toomanage hello az/opt-lemondáshoz, `EnableNativePush()` és `DisableNativePush()`. Ön nem sikerült, például hozzon létre egy beállítást a váltógomb toodisable hello-beállítások vagy MPNS engedélyezése.

Azt is eldöntheti, toodeactivate MPNS hello Engagement konfigurálással\<windows phone-sdk-reach-konfiguráció\>.

> 2.9.1) hello alkalmazás először le kell írnia a megadott hello értesítések toobe és **hello felhasználó engedélye (részt) beszerzése**, és **kell egy olyan mechanizmus biztosítása mely hello keresztül felhasználó kérheti fogadás kívül leküldéses értesítések**. Az értesítések használatával a Microsoft leküldéses értesítéseket kezelő szolgáltatásában hello megadott hello megadott leírás toohello felhasználói konzisztensnek kell lennie, és meg kell felelnie az összes alkalmazható [alkalmazás-házirendek] [ Content Policies]és [adott alkalmazás esetében további követelmények].
> 
> 

2) Ne használjon túl sok leküldéses értesítéseket. Bevonási értesítéseket meg fogja kezelni.

> 2.9.2) hello alkalmazás és a Microsoft leküldéses értesítéseket kezelő szolgáltatásában hello használatát kell nem túl hálózati kapacitás vagy a Microsoft leküldéses értesítéseket kezelő szolgáltatásában hello sávszélesség, vagy más módon jogosulatlanul terheljék a Windows Phone vagy más Microsoft-eszköz vagy szolgáltatás túl sok a leküldéses értesítések alapján a Microsoft méltányosan, és nem sérüléséhez vagy bármely Microsoft Networkshöz vagy kiszolgálók, vagy bármely harmadik felek kiszolgálóihoz vagy hálózatok csatlakoztatott toohello a Microsoft leküldéses értesítéseket kezelő szolgáltatása.
> 
> 

3) Ne használja az MPNS toosend criticals információra. Bevonási használ MPNS, ezért ez a szabály is létre hello Engagement előtér-hello kampányok vonatkozik.

> 2.9.3) a Microsoft leküldéses értesítéseket kezelő szolgáltatásában használt toosend értesítések, amelyek akkreditált képviseletének kritikus vagy más módon nem lehet hello hatással lehet a kérdések vagy halál, beleértve a korlátozás kritikus értesítések kapcsolódó tooa orvosi eszköz vagy a feltétel nélkül. A MICROSOFT kifejezetten elhárít minden garanciát, hogy hello használata a hello MICROSOFT LEKÜLDÉSES értesítési szolgáltatás vagy KÉZBESÍTÉSI a MICROSOFT LEKÜLDÉSES értesítési szolgáltatás ÉRTESÍTÉSEKET fog kell folyamatos, hiba szabad vagy más módon garantált tooOCCUR ON A valós idejű alapján.
> 
> 

**A Microsoft nem garantálja, hogy az alkalmazás továbbítják hello érvényesítési folyamata, ha ezek a javaslatok nem veszik figyelembe.**

## <a name="handle-data-push-optional"></a>Kezeli az adatleküldés (nem kötelező)
Ha azt szeretné, hogy az alkalmazás toobe képes tooreceive Reach adatleküldések, két események tooimplement hello EngagementReach osztály van:

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

Állítsa hello hello tartalmának `EngagementReach.Instance.Handler` mező található az egyéni objektum a `App.xaml.cs` hello osztály `Application_Launching` metódust.

**Mintakód:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> Alapértelmezés szerint az Engagement használja a saját végrehajtásának `EngagementReachHandler`. Nincs toocreate a saját, és ha így tesz, akkor nem kell toooverride minden metódus. hello alapértelmezés tooselect hello Engagement alapobjektum lesz.
> 
> 

### <a name="layouts"></a>Elrendezés
Alapértelmezés szerint a Reach hello beágyazott erőforrások hello DLL toodisplay hello értesítések és lapokat fogja használni.

Azonban dönthet úgy is toouse saját erőforrások tooreflect a márka, ezen összetevők.

Felülbírálható `EngagementReachHandler` az alosztály tootell Engagement toouse módszerek a elrendezések:

**Mintakód:**

    // In your subclass of EngagementReachHandler

    public override string GetTextViewAnnouncementUri()
    {
       // return hello path of your own xaml
    }

    public override string GetWebViewAnnouncementUri()
    {
       // return hello path of your own xaml
    }

    public override string GetPollUri()
    {
       // return hello path of your own xaml
    }

    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [!TIP]
> Hello `CreateNotification` metódus adhat vissza null értékű. hello értesítési nem jelennek meg, és hello reach-kampány a rendszer eldobja.
> 
> 

toosimplify elrendezés Önnél is biztosítunk saját XAML-kódot, amely a kódot is szolgálhatnak. Hello Engagement SDK archívumban találhatók (/ src/reach /).

> [!WARNING]
> hello szerzik be a Microsoft hello pontos azonos gazdarendszerhez használjuk. Ezért ha azt szeretné, hogy toomodify azokat közvetlenül, nem elfelejti toochange hello névtér és hello nevét.
> 
> 

### <a name="notification-position"></a>Értesítési pozíciója
Alapértelmezés szerint egy alkalmazásbeli értesítés hello alsó bal oldalán található hello alkalmazás jelenik meg. Ez a viselkedés felülbírálható hello módosítható `GetNotificationPosition` hello metódusában `EngagementReachHandler` objektum.

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of hello EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Jelenleg, választhat a hello `BOTTOM` (alapértelmezett) és `TOP` pozíciók.

### <a name="launch-message"></a>Indítsa el az üzenet
Ha a felhasználó kattint, a rendszer értesítést (egy bejelentési), Engagement elindítja hello app, hello hello tartalmának betöltése leküldéses üzeneteket, és a megfelelő kampány hello hello lap megjelenítéséhez.

Nincs késleltetés hello indítási hello alkalmazás és hello megjelenítés hello lap (attól függően, hogy a hálózat sebességétől hello) között.

valami tölt toohello felhasználói tooindicate, kell biztosítania egy visual adatok, például egy folyamatjelző vagy egy folyamatjelző. Bevonási nem tudja kezelni, hogy saját magát, de itt néhány kezelők meg.

tooimplement hello visszahívási, tegye:

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

A hello visszahívási beállíthatja a `Application_Launching` metódusában a `App.xaml.cs` fájl, lehetőleg előtt hello `EngagementReach.Instance.Init()` hívható meg.

> [!TIP]
> Minden egyes kezelő felhasználói felület szálából hello hívja. Önnek nincs tooworry MessageBox vagy valami felhasználói felület kapcsolatos használatakor.
> 
> 

[alkalmazás-házirendek]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[adott alkalmazás esetében további követelmények]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx

