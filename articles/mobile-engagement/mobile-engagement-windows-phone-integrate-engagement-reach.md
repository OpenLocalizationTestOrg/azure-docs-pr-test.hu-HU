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
# <a name="windows-phone-silverlight-reach-sdk-integration"></a><span data-ttu-id="44913-103">Windows Phone Silverlight Reach SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="44913-103">Windows Phone Silverlight Reach SDK Integration</span></span>
<span data-ttu-id="44913-104">Hello ismertetett hello integrációs eljárást kell követni, [Windows Phone Silverlight Engagement SDK-integráció](mobile-engagement-windows-phone-integrate-engagement.md) Ez az útmutató követése előtt.</span><span class="sxs-lookup"><span data-stu-id="44913-104">You must follow hello integration procedure described in hello [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a><span data-ttu-id="44913-105">A Windows Phone Silverlight-projektben Engagement Reach SDK hello beágyazása</span><span class="sxs-lookup"><span data-stu-id="44913-105">Embed hello Engagement Reach SDK into your Windows Phone Silverlight project</span></span>
<span data-ttu-id="44913-106">Nincs semmi tooadd.</span><span class="sxs-lookup"><span data-stu-id="44913-106">You do not have anything tooadd.</span></span> <span data-ttu-id="44913-107">`EngagementReach`hivatkozások és erőforrások még a projektben.</span><span class="sxs-lookup"><span data-stu-id="44913-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="44913-108">Testre szabhatja a hello található képek `Resources` mappa a projekt, különösen akkor hello márka ikon (adott alapértelmezett toohello Engagement ikon).</span><span class="sxs-lookup"><span data-stu-id="44913-108">You can customize images located in hello `Resources` folder of your project, especially hello brand icon (that default toohello Engagement icon).</span></span>
> 
> 

## <a name="add-hello-capabilities"></a><span data-ttu-id="44913-109">Hello képességek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="44913-109">Add hello capabilities</span></span>
<span data-ttu-id="44913-110">hello Engagement Reach SDK-t kell néhány további képességeket.</span><span class="sxs-lookup"><span data-stu-id="44913-110">hello Engagement Reach SDK needs some additional capabilities.</span></span>

<span data-ttu-id="44913-111">Nyissa meg a `WMAppManifest.xml` fájlt, és ne feledje, hogy a következő képességeket hello deklarált:</span><span class="sxs-lookup"><span data-stu-id="44913-111">Open your `WMAppManifest.xml` file and be sure that hello following capabilities are declared:</span></span>

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

<span data-ttu-id="44913-112">hello először egy használják hello MPNS szolgáltatás tooallow hello megjelenítési bejelentési értesítés.</span><span class="sxs-lookup"><span data-stu-id="44913-112">hello first one is used by hello MPNS service tooallow hello display of toast notification.</span></span> <span data-ttu-id="44913-113">hello második használatban tooembed hello SDK be egy böngésző feladatot.</span><span class="sxs-lookup"><span data-stu-id="44913-113">hello second one is used tooembed a browser task into hello SDK.</span></span>

<span data-ttu-id="44913-114">Hello szerkesztése `WMAppManifest.xml` fájlt, és hello belül `<Capabilities />` címke:</span><span class="sxs-lookup"><span data-stu-id="44913-114">Edit hello `WMAppManifest.xml` file and add inside hello `<Capabilities />` tag :</span></span>

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-hello-microsoft-push-notification-service"></a><span data-ttu-id="44913-115">A Microsoft leküldéses értesítéseket kezelő szolgáltatásában hello engedélyezése</span><span class="sxs-lookup"><span data-stu-id="44913-115">Enable hello Microsoft Push Notification Service</span></span>
<span data-ttu-id="44913-116">A sorrend toouse hello **a Microsoft leküldéses értesítéseket kezelő szolgáltatásában** (MPNS néven) a `WMAppManifest.xml` fájl rendelkeznie kell egy `<App />` rendelkező címke egy `Publisher` attribútum a projekt toohello nevének beállítása.</span><span class="sxs-lookup"><span data-stu-id="44913-116">In order toouse hello **Microsoft Push Notification Service** (referred as MPNS) your `WMAppManifest.xml` file must have an `<App />` tag with a `Publisher` attribute set toohello name of your project.</span></span>

## <a name="initialize-hello-engagement-reach-sdk"></a><span data-ttu-id="44913-117">Hello Engagement Reach SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="44913-117">Initialize hello Engagement Reach SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="44913-118">Bevonási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="44913-118">Engagement configuration</span></span>
<span data-ttu-id="44913-119">hello Engagement konfigurációs rendszer központosított hello `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="44913-119">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="44913-120">A fájl toospecify reach konfigurációjának a szerkesztésével:</span><span class="sxs-lookup"><span data-stu-id="44913-120">Edit this file toospecify reach configuration :</span></span>

* <span data-ttu-id="44913-121">*Nem kötelező*, jelzik, hogy a natív leküldéses hello (MPNS) aktiválva van, vagy nem közötti `<enableNativePush>` és `</enableNativePush>` címkék, (`true` alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="44913-121">*Optional*, indicate whether hello native push (MPNS) is activated or not between `<enableNativePush>` and `</enableNativePush>` tags, (`true` by default).</span></span>
* <span data-ttu-id="44913-122">*Nem kötelező*, hello leküldéses csatorna közötti hello nevét jelzi `<channelName>` és `</channelName>` címkék, adja meg a hello azonos, hogy jelenleg lehet-e használni az alkalmazást, vagy hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="44913-122">*Optional*, indicate hello name of hello push channel between `<channelName>` and `</channelName>` tags, provide hello same that your application may currently use or leave it empty.</span></span>

<span data-ttu-id="44913-123">Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:</span><span class="sxs-lookup"><span data-stu-id="44913-123">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization :</span></span>

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
> <span data-ttu-id="44913-124">Megadhatja, hogy hello hello MPNS leküldéses csatorna az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="44913-124">You can specify hello name of hello MPNS push channel of your application.</span></span> <span data-ttu-id="44913-125">Alapértelmezés szerint az Engagement hello appId alapján nevét hoz létre.</span><span class="sxs-lookup"><span data-stu-id="44913-125">By default, Engagement creates a name based on hello appId.</span></span> <span data-ttu-id="44913-126">Hogy nincs szükség toospecify hello neve, kivéve, ha azt tervezi, toouse hello leküldéses csatorna Engagement kívül.</span><span class="sxs-lookup"><span data-stu-id="44913-126">You have no need toospecify hello name yourself, except if you plan toouse hello push channel outside of Engagement.</span></span>
> 
> 

### <a name="engagement-initialization"></a><span data-ttu-id="44913-127">Bevonási inicializálása</span><span class="sxs-lookup"><span data-stu-id="44913-127">Engagement initialization</span></span>
<span data-ttu-id="44913-128">Módosítsa a hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="44913-128">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="44913-129">Adja hozzá a tooyour `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="44913-129">Add tooyour `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="44913-130">Helyezze be `EngagementReach.Instance.Init` után csak `EngagementAgent.Instance.Init` a `Application_Launching` :</span><span class="sxs-lookup"><span data-stu-id="44913-130">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in `Application_Launching` :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* <span data-ttu-id="44913-131">Helyezze be `EngagementReach.Instance.OnActivated` a hello `Application_Activated` módszert:</span><span class="sxs-lookup"><span data-stu-id="44913-131">Insert `EngagementReach.Instance.OnActivated` in hello `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> <span data-ttu-id="44913-132">Hello `EngagementReach.Instance.Init` egy dedikált szálat futtat.</span><span class="sxs-lookup"><span data-stu-id="44913-132">hello `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="44913-133">Nem rendelkezik toodo azt saját maga.</span><span class="sxs-lookup"><span data-stu-id="44913-133">You do not have toodo it yourself.</span></span>
> 
> 

## <a name="app-store-submission-considerations"></a><span data-ttu-id="44913-134">Tároló küldésének szempontjai</span><span class="sxs-lookup"><span data-stu-id="44913-134">App store submission considerations</span></span>
<span data-ttu-id="44913-135">Microsoft néhány szabály írja elő, amikor hello leküldéses értesítésekkel:</span><span class="sxs-lookup"><span data-stu-id="44913-135">Microsoft imposes some rules when using hello push notifications:</span></span>

<span data-ttu-id="44913-136">A hello Microsoft [alkalmazás-házirendek] -dokumentáció, szakasz 2.9:</span><span class="sxs-lookup"><span data-stu-id="44913-136">From hello Microsoft [Application Policies] documentation, section 2.9:</span></span>

1) <span data-ttu-id="44913-137">Meg kell kérnie hello felhasználói tooaccept tooreceive leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="44913-137">You must ask hello user tooaccept tooreceive push notifications.</span></span> <span data-ttu-id="44913-138">Ezután a beállítások egy módon toodisable hello leküldéses értesítések hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="44913-138">Then, in your settings, add a way toodisable hello push notifications.</span></span>

<span data-ttu-id="44913-139">hello EngagementReach objektum biztosít két módszer toomanage hello az/opt-lemondáshoz, `EnableNativePush()` és `DisableNativePush()`.</span><span class="sxs-lookup"><span data-stu-id="44913-139">hello EngagementReach object provides two methods toomanage hello opt-in/opt-out, `EnableNativePush()` and `DisableNativePush()`.</span></span> <span data-ttu-id="44913-140">Ön nem sikerült, például hozzon létre egy beállítást a váltógomb toodisable hello-beállítások vagy MPNS engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="44913-140">You could, for example, create an option in hello settings with a toggle toodisable or enable MPNS.</span></span>

<span data-ttu-id="44913-141">Azt is eldöntheti, toodeactivate MPNS hello Engagement konfigurálással\<windows phone-sdk-reach-konfiguráció\>.</span><span class="sxs-lookup"><span data-stu-id="44913-141">You can also decide toodeactivate MPNS through hello Engagement configuration\<windows-phone-sdk-reach-configuration\>.</span></span>

> <span data-ttu-id="44913-142">2.9.1) hello alkalmazás először le kell írnia a megadott hello értesítések toobe és **hello felhasználó engedélye (részt) beszerzése**, és **kell egy olyan mechanizmus biztosítása mely hello keresztül felhasználó kérheti fogadás kívül leküldéses értesítések**.</span><span class="sxs-lookup"><span data-stu-id="44913-142">2.9.1) hello application must first describe hello notifications toobe provided and **obtain hello user’s express permission (opt-in)**, and **must provide a mechanism through which hello user can opt out of receiving push notifications**.</span></span> <span data-ttu-id="44913-143">Az értesítések használatával a Microsoft leküldéses értesítéseket kezelő szolgáltatásában hello megadott hello megadott leírás toohello felhasználói konzisztensnek kell lennie, és meg kell felelnie az összes alkalmazható [alkalmazás-házirendek] [ Content Policies]és [adott alkalmazás esetében további követelmények].</span><span class="sxs-lookup"><span data-stu-id="44913-143">All notifications provided using hello Microsoft Push Notification Service must be consistent with hello description provided toohello user and must comply with all applicable [Application Policies][Content Policies] and [Additional Requirements for Specific Application Types].</span></span>
> 
> 

2) <span data-ttu-id="44913-144">Ne használjon túl sok leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="44913-144">You should not use too many push notifications.</span></span> <span data-ttu-id="44913-145">Bevonási értesítéseket meg fogja kezelni.</span><span class="sxs-lookup"><span data-stu-id="44913-145">Engagement will handle notifications for you.</span></span>

> <span data-ttu-id="44913-146">2.9.2) hello alkalmazás és a Microsoft leküldéses értesítéseket kezelő szolgáltatásában hello használatát kell nem túl hálózati kapacitás vagy a Microsoft leküldéses értesítéseket kezelő szolgáltatásában hello sávszélesség, vagy más módon jogosulatlanul terheljék a Windows Phone vagy más Microsoft-eszköz vagy szolgáltatás túl sok a leküldéses értesítések alapján a Microsoft méltányosan, és nem sérüléséhez vagy bármely Microsoft Networkshöz vagy kiszolgálók, vagy bármely harmadik felek kiszolgálóihoz vagy hálózatok csatlakoztatott toohello a Microsoft leküldéses értesítéseket kezelő szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="44913-146">2.9.2) hello application and its use of hello Microsoft Push Notification Service must not excessively use network capacity or bandwidth of hello Microsoft Push Notification Service, or otherwise unduly burden a Windows Phone or other Microsoft device or service with excessive push notifications, as determined by Microsoft in its reasonable discretion, and must not harm or interfere with any Microsoft networks or servers or any third party servers or networks connected toohello Microsoft Push Notification Service.</span></span>
> 
> 

3) <span data-ttu-id="44913-147">Ne használja az MPNS toosend criticals információra.</span><span class="sxs-lookup"><span data-stu-id="44913-147">Do not rely on MPNS toosend criticals information.</span></span> <span data-ttu-id="44913-148">Bevonási használ MPNS, ezért ez a szabály is létre hello Engagement előtér-hello kampányok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="44913-148">Engagement uses MPNS, so this rule also applies for hello campaigns created inside hello Engagement front-end.</span></span>

> <span data-ttu-id="44913-149">2.9.3) a Microsoft leküldéses értesítéseket kezelő szolgáltatásában használt toosend értesítések, amelyek akkreditált képviseletének kritikus vagy más módon nem lehet hello hatással lehet a kérdések vagy halál, beleértve a korlátozás kritikus értesítések kapcsolódó tooa orvosi eszköz vagy a feltétel nélkül.</span><span class="sxs-lookup"><span data-stu-id="44913-149">2.9.3) hello Microsoft Push Notification Service may not be used toosend notifications that are mission critical or otherwise could affect matters of life or death, including without limitation critical notifications related tooa medical device or condition.</span></span> <span data-ttu-id="44913-150">A MICROSOFT kifejezetten elhárít minden garanciát, hogy hello használata a hello MICROSOFT LEKÜLDÉSES értesítési szolgáltatás vagy KÉZBESÍTÉSI a MICROSOFT LEKÜLDÉSES értesítési szolgáltatás ÉRTESÍTÉSEKET fog kell folyamatos, hiba szabad vagy más módon garantált tooOCCUR ON A valós idejű alapján.</span><span class="sxs-lookup"><span data-stu-id="44913-150">MICROSOFT EXPRESSLY DISCLAIMS ANY WARRANTIES THAT hello USE OF hello MICROSOFT PUSH NOTIFICATION SERVICE OR DELIVERY OF MICROSOFT PUSH NOTIFICATION SERVICE NOTIFICATIONS WILL BE UNINTERRUPTED, ERROR FREE, OR OTHERWISE GUARANTEED tooOCCUR ON A REAL-TIME BASIS.</span></span>
> 
> 

<span data-ttu-id="44913-151">**A Microsoft nem garantálja, hogy az alkalmazás továbbítják hello érvényesítési folyamata, ha ezek a javaslatok nem veszik figyelembe.**</span><span class="sxs-lookup"><span data-stu-id="44913-151">**We cannot guarantee that your application will pass hello validation process if you do not respect these recommendations.**</span></span>

## <a name="handle-data-push-optional"></a><span data-ttu-id="44913-152">Kezeli az adatleküldés (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="44913-152">Handle data push (optional)</span></span>
<span data-ttu-id="44913-153">Ha azt szeretné, hogy az alkalmazás toobe képes tooreceive Reach adatleküldések, két események tooimplement hello EngagementReach osztály van:</span><span class="sxs-lookup"><span data-stu-id="44913-153">If you want your application toobe able tooreceive Reach data pushes, you have tooimplement two events of hello EngagementReach class:</span></span>

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

<span data-ttu-id="44913-154">Láthatja, hogy az egyes metódus értéket ad vissza egy logikai érték hello a visszahívás.</span><span class="sxs-lookup"><span data-stu-id="44913-154">You can see that hello callback of each method returns a boolean.</span></span> <span data-ttu-id="44913-155">Engagement küld egy visszajelzés tooits háttér-után hello adatleküldés terjesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="44913-155">Engagement sends a feedback tooits back-end after dispatching hello data push.</span></span> <span data-ttu-id="44913-156">Hello visszahívási hamis értéket ad vissza, ha hello `exit` visszajelzés küldése lesz.</span><span class="sxs-lookup"><span data-stu-id="44913-156">If hello callback returns false, hello `exit` feedback will be send.</span></span> <span data-ttu-id="44913-157">Ellenkező esetben lesz `action`.</span><span class="sxs-lookup"><span data-stu-id="44913-157">Otherwise, it will be `action`.</span></span> <span data-ttu-id="44913-158">Ha nincs visszahívás hello események, hello `drop` visszajelzés visszaadott tooEngagement.</span><span class="sxs-lookup"><span data-stu-id="44913-158">If no callback is set for hello events, hello `drop` feedback will be returned tooEngagement.</span></span>

> [!WARNING]
> <span data-ttu-id="44913-159">Bevonási nem tud tooreceive Többszörösök visszajelzése van, az adatokat.</span><span class="sxs-lookup"><span data-stu-id="44913-159">Engagement is not able tooreceive multiples feedbacks for a data push.</span></span> <span data-ttu-id="44913-160">Ha azt tervezi, tooset több kezelők egy olyan eseményre, vegye figyelembe, hogy hello visszajelzés felel meg toohello legutóbb elküldött.</span><span class="sxs-lookup"><span data-stu-id="44913-160">If you plan tooset several handlers on an event, be aware that hello feedback will correspond toohello last one sent.</span></span> <span data-ttu-id="44913-161">Ebben az esetben ajánlott tooalways értéket ad vissza hello zavaró visszajelzés rendelkező előtér-hello azonos érték tooavoid.</span><span class="sxs-lookup"><span data-stu-id="44913-161">In this case, we recommend tooalways returns hello same value tooavoid having confusing feedback on hello front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="44913-162">(Választható) felhasználói felület testreszabása</span><span class="sxs-lookup"><span data-stu-id="44913-162">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="44913-163">Első lépés</span><span class="sxs-lookup"><span data-stu-id="44913-163">First step</span></span>
<span data-ttu-id="44913-164">Azt teszik toocustomize hello reach felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="44913-164">We allow you toocustomize hello reach UI.</span></span>

<span data-ttu-id="44913-165">toodo úgy, hogy toocreate hello alosztályát `EngagementReachHandler` osztály.</span><span class="sxs-lookup"><span data-stu-id="44913-165">toodo so, you have toocreate a subclass of hello `EngagementReachHandler` class.</span></span>

<span data-ttu-id="44913-166">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="44913-166">**Sample Code :**</span></span>

    using Microsoft.Azure.Engagement;

    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

<span data-ttu-id="44913-167">Állítsa hello hello tartalmának `EngagementReach.Instance.Handler` mező található az egyéni objektum a `App.xaml.cs` hello osztály `Application_Launching` metódust.</span><span class="sxs-lookup"><span data-stu-id="44913-167">Then, set hello content of hello `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within hello `Application_Launching` method.</span></span>

<span data-ttu-id="44913-168">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="44913-168">**Sample Code :**</span></span>

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> <span data-ttu-id="44913-169">Alapértelmezés szerint az Engagement használja a saját végrehajtásának `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="44913-169">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span> <span data-ttu-id="44913-170">Nincs toocreate a saját, és ha így tesz, akkor nem kell toooverride minden metódus.</span><span class="sxs-lookup"><span data-stu-id="44913-170">You don't have toocreate your own, and if you do so, you don't have toooverride every method.</span></span> <span data-ttu-id="44913-171">hello alapértelmezés tooselect hello Engagement alapobjektum lesz.</span><span class="sxs-lookup"><span data-stu-id="44913-171">hello default behavior is tooselect hello Engagement base object.</span></span>
> 
> 

### <a name="layouts"></a><span data-ttu-id="44913-172">Elrendezés</span><span class="sxs-lookup"><span data-stu-id="44913-172">Layouts</span></span>
<span data-ttu-id="44913-173">Alapértelmezés szerint a Reach hello beágyazott erőforrások hello DLL toodisplay hello értesítések és lapokat fogja használni.</span><span class="sxs-lookup"><span data-stu-id="44913-173">By default, Reach will use hello embedded resources of hello DLL toodisplay hello notifications and pages.</span></span>

<span data-ttu-id="44913-174">Azonban dönthet úgy is toouse saját erőforrások tooreflect a márka, ezen összetevők.</span><span class="sxs-lookup"><span data-stu-id="44913-174">However, you can decide toouse your own resources tooreflect your brand in these components.</span></span>

<span data-ttu-id="44913-175">Felülbírálható `EngagementReachHandler` az alosztály tootell Engagement toouse módszerek a elrendezések:</span><span class="sxs-lookup"><span data-stu-id="44913-175">You can override `EngagementReachHandler` methods in your subclass tootell Engagement toouse your layouts :</span></span>

<span data-ttu-id="44913-176">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="44913-176">**Sample Code :**</span></span>

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
> <span data-ttu-id="44913-177">Hello `CreateNotification` metódus adhat vissza null értékű.</span><span class="sxs-lookup"><span data-stu-id="44913-177">hello `CreateNotification` method can return null.</span></span> <span data-ttu-id="44913-178">hello értesítési nem jelennek meg, és hello reach-kampány a rendszer eldobja.</span><span class="sxs-lookup"><span data-stu-id="44913-178">hello notification won't be displayed and hello reach campaign will be dropped.</span></span>
> 
> 

<span data-ttu-id="44913-179">toosimplify elrendezés Önnél is biztosítunk saját XAML-kódot, amely a kódot is szolgálhatnak.</span><span class="sxs-lookup"><span data-stu-id="44913-179">toosimplify your layout implementation, we also provide our own xaml which can serve as a basis for your code.</span></span> <span data-ttu-id="44913-180">Hello Engagement SDK archívumban találhatók (/ src/reach /).</span><span class="sxs-lookup"><span data-stu-id="44913-180">They are located in hello Engagement SDK archive (/src/reach/).</span></span>

> [!WARNING]
> <span data-ttu-id="44913-181">hello szerzik be a Microsoft hello pontos azonos gazdarendszerhez használjuk.</span><span class="sxs-lookup"><span data-stu-id="44913-181">hello sources that we provide are hello exact same ones that we use.</span></span> <span data-ttu-id="44913-182">Ezért ha azt szeretné, hogy toomodify azokat közvetlenül, nem elfelejti toochange hello névtér és hello nevét.</span><span class="sxs-lookup"><span data-stu-id="44913-182">So if you want toomodify them directly, don't forget toochange hello namespace and hello name.</span></span>
> 
> 

### <a name="notification-position"></a><span data-ttu-id="44913-183">Értesítési pozíciója</span><span class="sxs-lookup"><span data-stu-id="44913-183">Notification position</span></span>
<span data-ttu-id="44913-184">Alapértelmezés szerint egy alkalmazásbeli értesítés hello alsó bal oldalán található hello alkalmazás jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="44913-184">By default, an in-app notification is displayed at hello bottom left side of hello application.</span></span> <span data-ttu-id="44913-185">Ez a viselkedés felülbírálható hello módosítható `GetNotificationPosition` hello metódusában `EngagementReachHandler` objektum.</span><span class="sxs-lookup"><span data-stu-id="44913-185">You can change this behavior by overriding hello `GetNotificationPosition` method of hello `EngagementReachHandler` object.</span></span>

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of hello EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

<span data-ttu-id="44913-186">Jelenleg, választhat a hello `BOTTOM` (alapértelmezett) és `TOP` pozíciók.</span><span class="sxs-lookup"><span data-stu-id="44913-186">Currently, you can choose between hello `BOTTOM` (default) and `TOP` positions.</span></span>

### <a name="launch-message"></a><span data-ttu-id="44913-187">Indítsa el az üzenet</span><span class="sxs-lookup"><span data-stu-id="44913-187">Launch message</span></span>
<span data-ttu-id="44913-188">Ha a felhasználó kattint, a rendszer értesítést (egy bejelentési), Engagement elindítja hello app, hello hello tartalmának betöltése leküldéses üzeneteket, és a megfelelő kampány hello hello lap megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="44913-188">When a user clicks on a system notification (a toast), Engagement launches hello app, load hello content of hello push messages, and display hello page for hello corresponding campaign.</span></span>

<span data-ttu-id="44913-189">Nincs késleltetés hello indítási hello alkalmazás és hello megjelenítés hello lap (attól függően, hogy a hálózat sebességétől hello) között.</span><span class="sxs-lookup"><span data-stu-id="44913-189">There is a delay between hello launch of hello application and hello display of hello page (depending on hello speed of your network).</span></span>

<span data-ttu-id="44913-190">valami tölt toohello felhasználói tooindicate, kell biztosítania egy visual adatok, például egy folyamatjelző vagy egy folyamatjelző.</span><span class="sxs-lookup"><span data-stu-id="44913-190">tooindicate toohello user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="44913-191">Bevonási nem tudja kezelni, hogy saját magát, de itt néhány kezelők meg.</span><span class="sxs-lookup"><span data-stu-id="44913-191">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="44913-192">tooimplement hello visszahívási, tegye:</span><span class="sxs-lookup"><span data-stu-id="44913-192">tooimplement hello callback, do:</span></span>

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

<span data-ttu-id="44913-193">A hello visszahívási beállíthatja a `Application_Launching` metódusában a `App.xaml.cs` fájl, lehetőleg előtt hello `EngagementReach.Instance.Init()` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="44913-193">You can set hello callback in your `Application_Launching` method of your `App.xaml.cs` file, preferably before hello `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="44913-194">Minden egyes kezelő felhasználói felület szálából hello hívja.</span><span class="sxs-lookup"><span data-stu-id="44913-194">Each handler is called by hello UI Thread.</span></span> <span data-ttu-id="44913-195">Önnek nincs tooworry MessageBox vagy valami felhasználói felület kapcsolatos használatakor.</span><span class="sxs-lookup"><span data-stu-id="44913-195">You do not have tooworry when using a MessageBox or something UI-related.</span></span>
> 
> 

[alkalmazás-házirendek]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[adott alkalmazás esetében további követelmények]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx

