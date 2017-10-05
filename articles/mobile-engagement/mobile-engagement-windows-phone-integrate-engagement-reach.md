---
title: "Windows Phone Silverlight Reach SDK-integráció"
description: "Windows Phone Silverlight-alkalmazásokhoz az Azure Mobile Engagement Reach integrálása"
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
ms.openlocfilehash: 0738f33df94d14fbb393bfaaf09e94c6560213cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="windows-phone-silverlight-reach-sdk-integration"></a><span data-ttu-id="e24b7-103">Windows Phone Silverlight Reach SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="e24b7-103">Windows Phone Silverlight Reach SDK Integration</span></span>
<span data-ttu-id="e24b7-104">Az integráció az ismertetett eljárást kell követni a [Windows Phone Silverlight Engagement SDK-integráció](mobile-engagement-windows-phone-integrate-engagement.md) Ez az útmutató követése előtt.</span><span class="sxs-lookup"><span data-stu-id="e24b7-104">You must follow the integration procedure described in the [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a><span data-ttu-id="e24b7-105">Az Engagement Reach SDK beágyazása a Windows Phone Silverlight-projekt</span><span class="sxs-lookup"><span data-stu-id="e24b7-105">Embed the Engagement Reach SDK into your Windows Phone Silverlight project</span></span>
<span data-ttu-id="e24b7-106">Nincs olyan hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="e24b7-106">You do not have anything to add.</span></span> <span data-ttu-id="e24b7-107">`EngagementReach`hivatkozások és erőforrások még a projektben.</span><span class="sxs-lookup"><span data-stu-id="e24b7-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="e24b7-108">Lemezképek található, testreszabhatja a `Resources` mappa a projekt, különösen a márka ikon (hogy az Engagement ikonra az alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="e24b7-108">You can customize images located in the `Resources` folder of your project, especially the brand icon (that default to the Engagement icon).</span></span>
> 
> 

## <a name="add-the-capabilities"></a><span data-ttu-id="e24b7-109">A képességek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e24b7-109">Add the capabilities</span></span>
<span data-ttu-id="e24b7-110">Az Engagement Reach SDK kell néhány további képességeket.</span><span class="sxs-lookup"><span data-stu-id="e24b7-110">The Engagement Reach SDK needs some additional capabilities.</span></span>

<span data-ttu-id="e24b7-111">Nyissa meg a `WMAppManifest.xml` fájlt, és ne feledje, hogy a következő lehetőségei vannak deklarálva:</span><span class="sxs-lookup"><span data-stu-id="e24b7-111">Open your `WMAppManifest.xml` file and be sure that the following capabilities are declared:</span></span>

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

<span data-ttu-id="e24b7-112">Az első egy bejelentési értesítés megjelenítésének használják az MPNS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="e24b7-112">The first one is used by the MPNS service to allow the display of toast notification.</span></span> <span data-ttu-id="e24b7-113">A második egy böngésző feladat beágyazása az SDK szolgál.</span><span class="sxs-lookup"><span data-stu-id="e24b7-113">The second one is used to embed a browser task into the SDK.</span></span>

<span data-ttu-id="e24b7-114">Szerkessze a `WMAppManifest.xml` fájlt, és belül a `<Capabilities />` címke:</span><span class="sxs-lookup"><span data-stu-id="e24b7-114">Edit the `WMAppManifest.xml` file and add inside the `<Capabilities />` tag :</span></span>

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-the-microsoft-push-notification-service"></a><span data-ttu-id="e24b7-115">A Microsoft leküldéses értesítési szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e24b7-115">Enable the Microsoft Push Notification Service</span></span>
<span data-ttu-id="e24b7-116">Ahhoz, hogy a **a Microsoft leküldéses értesítéseket kezelő szolgáltatásában** (MPNS néven) a `WMAppManifest.xml` fájl rendelkeznie kell egy `<App />` rendelkező címke egy `Publisher` attribútum értékének beállítása a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="e24b7-116">In order to use the **Microsoft Push Notification Service** (referred as MPNS) your `WMAppManifest.xml` file must have an `<App />` tag with a `Publisher` attribute set to the name of your project.</span></span>

## <a name="initialize-the-engagement-reach-sdk"></a><span data-ttu-id="e24b7-117">Az Engagement Reach SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="e24b7-117">Initialize the Engagement Reach SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="e24b7-118">Bevonási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="e24b7-118">Engagement configuration</span></span>
<span data-ttu-id="e24b7-119">A bevonási konfigurációs központosított a `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="e24b7-119">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="e24b7-120">Ezt a fájlt adja meg a reach-konfiguráció szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="e24b7-120">Edit this file to specify reach configuration :</span></span>

* <span data-ttu-id="e24b7-121">*Nem kötelező*, jelzik, hogy a natív leküldéses (MPNS) aktiválva van, vagy nem közötti `<enableNativePush>` és `</enableNativePush>` címkék, (`true` alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="e24b7-121">*Optional*, indicate whether the native push (MPNS) is activated or not between `<enableNativePush>` and `</enableNativePush>` tags, (`true` by default).</span></span>
* <span data-ttu-id="e24b7-122">*Nem kötelező*, közötti leküldéses csatorna jelzésére `<channelName>` és `</channelName>` címkék, adja meg, hogy jelenleg lehet-e használni az alkalmazás, vagy hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="e24b7-122">*Optional*, indicate the name of the push channel between `<channelName>` and `</channelName>` tags, provide the same that your application may currently use or leave it empty.</span></span>

<span data-ttu-id="e24b7-123">Ha azt szeretné, ehelyett meg futásidőben, hívása előtt az Engagement ügynök inicializálása a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="e24b7-123">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization :</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */

    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [!TIP]
> <span data-ttu-id="e24b7-124">Megadhatja az MPNS leküldéses csatorna az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="e24b7-124">You can specify the name of the MPNS push channel of your application.</span></span> <span data-ttu-id="e24b7-125">Alapértelmezés szerint az Engagement egy nevet a appId alapján hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e24b7-125">By default, Engagement creates a name based on the appId.</span></span> <span data-ttu-id="e24b7-126">Adja meg a nevét, kivéve, ha szeretné-e használni a leküldéses csatorna Engagement kívül nincs szükség van.</span><span class="sxs-lookup"><span data-stu-id="e24b7-126">You have no need to specify the name yourself, except if you plan to use the push channel outside of Engagement.</span></span>
> 
> 

### <a name="engagement-initialization"></a><span data-ttu-id="e24b7-127">Bevonási inicializálása</span><span class="sxs-lookup"><span data-stu-id="e24b7-127">Engagement initialization</span></span>
<span data-ttu-id="e24b7-128">Módosítsa a `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="e24b7-128">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="e24b7-129">Adja hozzá a `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="e24b7-129">Add to your `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="e24b7-130">Helyezze be `EngagementReach.Instance.Init` után csak `EngagementAgent.Instance.Init` a `Application_Launching` :</span><span class="sxs-lookup"><span data-stu-id="e24b7-130">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in `Application_Launching` :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* <span data-ttu-id="e24b7-131">Helyezze be `EngagementReach.Instance.OnActivated` a a `Application_Activated` módszert:</span><span class="sxs-lookup"><span data-stu-id="e24b7-131">Insert `EngagementReach.Instance.OnActivated` in the `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> <span data-ttu-id="e24b7-132">A `EngagementReach.Instance.Init` egy dedikált szálat futtat.</span><span class="sxs-lookup"><span data-stu-id="e24b7-132">The `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="e24b7-133">Nem kell saját kezűleg elvégezni.</span><span class="sxs-lookup"><span data-stu-id="e24b7-133">You do not have to do it yourself.</span></span>
> 
> 

## <a name="app-store-submission-considerations"></a><span data-ttu-id="e24b7-134">Tároló küldésének szempontjai</span><span class="sxs-lookup"><span data-stu-id="e24b7-134">App store submission considerations</span></span>
<span data-ttu-id="e24b7-135">Microsoft néhány szabály írja elő, amikor a leküldéses értesítések használata:</span><span class="sxs-lookup"><span data-stu-id="e24b7-135">Microsoft imposes some rules when using the push notifications:</span></span>

<span data-ttu-id="e24b7-136">A Microsoft [alkalmazás-házirendek] -dokumentáció, szakasz 2.9:</span><span class="sxs-lookup"><span data-stu-id="e24b7-136">From the Microsoft [Application Policies] documentation, section 2.9:</span></span>

1) <span data-ttu-id="e24b7-137">Meg kell kérnie a felhasználót, hogy fogadja el a leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="e24b7-137">You must ask the user to accept to receive push notifications.</span></span> <span data-ttu-id="e24b7-138">A beállításokat, majd adja hozzá egy módszerre, amellyel a leküldéses értesítések letiltása.</span><span class="sxs-lookup"><span data-stu-id="e24b7-138">Then, in your settings, add a way to disable the push notifications.</span></span>

<span data-ttu-id="e24b7-139">A EngagementReach objektum kezelheti a megújításra-a/lemondásra, két módszert kínál a `EnableNativePush()` és `DisableNativePush()`.</span><span class="sxs-lookup"><span data-stu-id="e24b7-139">The EngagementReach object provides two methods to manage the opt-in/opt-out, `EnableNativePush()` and `DisableNativePush()`.</span></span> <span data-ttu-id="e24b7-140">Például létrehozhat egy beállítást a beállítások a Váltás az MPNS engedélyezése vagy letiltása.</span><span class="sxs-lookup"><span data-stu-id="e24b7-140">You could, for example, create an option in the settings with a toggle to disable or enable MPNS.</span></span>

<span data-ttu-id="e24b7-141">Úgy is dönt, hogy MPNS inaktiválása az Engagement konfigurálással\<windows phone-sdk-reach-konfiguráció\>.</span><span class="sxs-lookup"><span data-stu-id="e24b7-141">You can also decide to deactivate MPNS through the Engagement configuration\<windows-phone-sdk-reach-configuration\>.</span></span>

> <span data-ttu-id="e24b7-142">2.9.1) az alkalmazás kell először írja le az értesítéseket meg kell adni, és **szerezze be a felhasználó engedélye (részt)**, és **kell egy olyan mechanizmust, amelyen keresztül a felhasználó is tilthatják le az leküldéses fogadása értesítések**.</span><span class="sxs-lookup"><span data-stu-id="e24b7-142">2.9.1) The application must first describe the notifications to be provided and **obtain the user’s express permission (opt-in)**, and **must provide a mechanism through which the user can opt out of receiving push notifications**.</span></span> <span data-ttu-id="e24b7-143">Az értesítések a Microsoft leküldéses értesítéseket kezelő szolgáltatással megadott a leírás, a felhasználó számára biztosított konzisztensnek kell lennie, és meg kell felelnie az összes alkalmazható [alkalmazás-házirendek] [ Content Policies] és [Adott alkalmazás esetében további követelmények].</span><span class="sxs-lookup"><span data-stu-id="e24b7-143">All notifications provided using the Microsoft Push Notification Service must be consistent with the description provided to the user and must comply with all applicable [Application Policies][Content Policies] and [Additional Requirements for Specific Application Types].</span></span>
> 
> 

2) <span data-ttu-id="e24b7-144">Ne használjon túl sok leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="e24b7-144">You should not use too many push notifications.</span></span> <span data-ttu-id="e24b7-145">Bevonási értesítéseket meg fogja kezelni.</span><span class="sxs-lookup"><span data-stu-id="e24b7-145">Engagement will handle notifications for you.</span></span>

> <span data-ttu-id="e24b7-146">2.9.2) az alkalmazás és annak a Microsoft leküldéses értesítéseket kezelő szolgáltatásában kell túlzottan használjon hálózati kapacitás vagy a Microsoft leküldéses értesítéseket kezelő szolgáltatásában, vagy más módon jogosulatlanul terheket egy Windows Phone-vagy más Microsoft eszköz vagy szolgáltatást, amely a sávszélesség túl sok leküldéses értesítések alapján a Microsoft méltányosan, és nem sérüléséhez vagy bármely Microsoft Networkshöz vagy kiszolgálók, vagy bármely harmadik felek kiszolgálóival vagy hálózatokhoz kapcsolódik a Microsoft leküldéses értesítéseket kezelő szolgáltatásában.</span><span class="sxs-lookup"><span data-stu-id="e24b7-146">2.9.2) The application and its use of the Microsoft Push Notification Service must not excessively use network capacity or bandwidth of the Microsoft Push Notification Service, or otherwise unduly burden a Windows Phone or other Microsoft device or service with excessive push notifications, as determined by Microsoft in its reasonable discretion, and must not harm or interfere with any Microsoft networks or servers or any third party servers or networks connected to the Microsoft Push Notification Service.</span></span>
> 
> 

3) <span data-ttu-id="e24b7-147">Ne használja az MPNS criticals információk küldése.</span><span class="sxs-lookup"><span data-stu-id="e24b7-147">Do not rely on MPNS to send criticals information.</span></span> <span data-ttu-id="e24b7-148">Engagement használ MPNS, ezért ez a szabály a kampányok előtér az Engagement létrehozott is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e24b7-148">Engagement uses MPNS, so this rule also applies for the campaigns created inside the Engagement front-end.</span></span>

> <span data-ttu-id="e24b7-149">2.9.3) a Microsoft leküldéses értesítéseket kezelő szolgáltatásában nem lehet, amelyek kritikus vagy egyéb kritikus értesítések befolyásolhatja a fontos információk vagy halál küldéséhez használt, kritikus értesítések korlátozás nélkül ideértve kapcsolódó orvosi eszköz vagy az állapot.</span><span class="sxs-lookup"><span data-stu-id="e24b7-149">2.9.3) The Microsoft Push Notification Service may not be used to send notifications that are mission critical or otherwise could affect matters of life or death, including without limitation critical notifications related to a medical device or condition.</span></span> <span data-ttu-id="e24b7-150">A MICROSOFT KIFEJEZETTEN ELHÁRÍT SEMMIFÉLE, HOGY A MICROSOFT LEKÜLDÉSES ÉRTESÍTÉSI SZOLGÁLTATÁS HASZNÁLATÁT VAGY A MICROSOFT LEKÜLDÉSES ÉRTESÍTÉSI SZOLGÁLTATÁSHOZ ÉRTESÍTÉST KÉZBESÍTÉSI FOLYAMATOS, SZABAD VAGY EGYÉB HIBA VALÓSZÍNŰ, HOGY A VALÓS IDEJŰ ALAPON TÖRTÉNIK.</span><span class="sxs-lookup"><span data-stu-id="e24b7-150">MICROSOFT EXPRESSLY DISCLAIMS ANY WARRANTIES THAT THE USE OF THE MICROSOFT PUSH NOTIFICATION SERVICE OR DELIVERY OF MICROSOFT PUSH NOTIFICATION SERVICE NOTIFICATIONS WILL BE UNINTERRUPTED, ERROR FREE, OR OTHERWISE GUARANTEED TO OCCUR ON A REAL-TIME BASIS.</span></span>
> 
> 

<span data-ttu-id="e24b7-151">**A Microsoft nem garantálja, hogy az alkalmazás az érvényesítési folyamat fogja továbbítani, ha ezek a javaslatok nem veszik figyelembe.**</span><span class="sxs-lookup"><span data-stu-id="e24b7-151">**We cannot guarantee that your application will pass the validation process if you do not respect these recommendations.**</span></span>

## <a name="handle-data-push-optional"></a><span data-ttu-id="e24b7-152">Kezeli az adatleküldés (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="e24b7-152">Handle data push (optional)</span></span>
<span data-ttu-id="e24b7-153">Ha azt szeretné, hogy az alkalmazás fogadhat Reach adatleküldések, két esemény EngagementReach osztály végrehajtásához rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e24b7-153">If you want your application to be able to receive Reach data pushes, you have to implement two events of the EngagementReach class:</span></span>

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

<span data-ttu-id="e24b7-154">Láthatja, hogy az egyes módszerek visszahívási olyan logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e24b7-154">You can see that the callback of each method returns a boolean.</span></span> <span data-ttu-id="e24b7-155">Bevonási egy visszajelzést küld a a háttér-után az adatleküldés terjesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="e24b7-155">Engagement sends a feedback to its back-end after dispatching the data push.</span></span> <span data-ttu-id="e24b7-156">A visszahívási hamis értéket ad vissza, ha a `exit` visszajelzés küldése lesz.</span><span class="sxs-lookup"><span data-stu-id="e24b7-156">If the callback returns false, the `exit` feedback will be send.</span></span> <span data-ttu-id="e24b7-157">Ellenkező esetben lesz `action`.</span><span class="sxs-lookup"><span data-stu-id="e24b7-157">Otherwise, it will be `action`.</span></span> <span data-ttu-id="e24b7-158">Ha nem visszahívási események, be van állítva a `drop` visszajelzést az eredmény engagement.</span><span class="sxs-lookup"><span data-stu-id="e24b7-158">If no callback is set for the events, the `drop` feedback will be returned to Engagement.</span></span>

> [!WARNING]
> <span data-ttu-id="e24b7-159">Bevonási nincs Többszörösök visszajelzése van, az adatokat fogadhat.</span><span class="sxs-lookup"><span data-stu-id="e24b7-159">Engagement is not able to receive multiples feedbacks for a data push.</span></span> <span data-ttu-id="e24b7-160">Ha azt tervezi, hogy több kezelők be egy eseményt, vegye figyelembe, hogy a visszajelzés utolsó felel meg egyik küldött.</span><span class="sxs-lookup"><span data-stu-id="e24b7-160">If you plan to set several handlers on an event, be aware that the feedback will correspond to the last one sent.</span></span> <span data-ttu-id="e24b7-161">Ebben az esetben ajánlott mindig adja meg ugyanazt az értéket ne használjon egyértelmű visszajelzést az előtér-a.</span><span class="sxs-lookup"><span data-stu-id="e24b7-161">In this case, we recommend to always returns the same value to avoid having confusing feedback on the front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="e24b7-162">(Választható) felhasználói felület testreszabása</span><span class="sxs-lookup"><span data-stu-id="e24b7-162">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="e24b7-163">Első lépés</span><span class="sxs-lookup"><span data-stu-id="e24b7-163">First step</span></span>
<span data-ttu-id="e24b7-164">Azt teszik lehetővé a reach felhasználói felület testreszabása.</span><span class="sxs-lookup"><span data-stu-id="e24b7-164">We allow you to customize the reach UI.</span></span>

<span data-ttu-id="e24b7-165">Ehhez létre kell hoznia egy alosztálya a `EngagementReachHandler` osztály.</span><span class="sxs-lookup"><span data-stu-id="e24b7-165">To do so, you have to create a subclass of the `EngagementReachHandler` class.</span></span>

<span data-ttu-id="e24b7-166">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="e24b7-166">**Sample Code :**</span></span>

    using Microsoft.Azure.Engagement;

    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

<span data-ttu-id="e24b7-167">Állítsa a tartalmát a `EngagementReach.Instance.Handler` mező található az egyéni objektum a `App.xaml.cs` belül osztály a `Application_Launching` metódus.</span><span class="sxs-lookup"><span data-stu-id="e24b7-167">Then, set the content of the `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within the `Application_Launching` method.</span></span>

<span data-ttu-id="e24b7-168">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="e24b7-168">**Sample Code :**</span></span>

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> <span data-ttu-id="e24b7-169">Alapértelmezés szerint az Engagement használja a saját végrehajtásának `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="e24b7-169">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span> <span data-ttu-id="e24b7-170">Nem kell létrehoznia a saját, és ha így tesz, nem kell minden metódus felülbírálására.</span><span class="sxs-lookup"><span data-stu-id="e24b7-170">You don't have to create your own, and if you do so, you don't have to override every method.</span></span> <span data-ttu-id="e24b7-171">Az alapértelmezett viselkedés, jelölje be a bevonási objektum.</span><span class="sxs-lookup"><span data-stu-id="e24b7-171">The default behavior is to select the Engagement base object.</span></span>
> 
> 

### <a name="layouts"></a><span data-ttu-id="e24b7-172">Elrendezés</span><span class="sxs-lookup"><span data-stu-id="e24b7-172">Layouts</span></span>
<span data-ttu-id="e24b7-173">Alapértelmezés szerint Reach az értesítések és a lap megjelenítése fogja használni a beágyazott erőforrások a dll-fájl.</span><span class="sxs-lookup"><span data-stu-id="e24b7-173">By default, Reach will use the embedded resources of the DLL to display the notifications and pages.</span></span>

<span data-ttu-id="e24b7-174">Azonban ha dönt, hogy saját erőforrásokat használnak, a márka, ezen összetevők megfelelően.</span><span class="sxs-lookup"><span data-stu-id="e24b7-174">However, you can decide to use your own resources to reflect your brand in these components.</span></span>

<span data-ttu-id="e24b7-175">Ha szeretné felülbírálni az `EngagementReachHandler` a alosztályának kell tudniuk a bevonási a elrendezések használandó módszerek:</span><span class="sxs-lookup"><span data-stu-id="e24b7-175">You can override `EngagementReachHandler` methods in your subclass to tell Engagement to use your layouts :</span></span>

<span data-ttu-id="e24b7-176">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="e24b7-176">**Sample Code :**</span></span>

    // In your subclass of EngagementReachHandler

    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetPollUri()
    {
       // return the path of your own xaml
    }

    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [!TIP]
> <span data-ttu-id="e24b7-177">A `CreateNotification` metódus adhat vissza null értékű.</span><span class="sxs-lookup"><span data-stu-id="e24b7-177">The `CreateNotification` method can return null.</span></span> <span data-ttu-id="e24b7-178">Az értesítés nem jelennek meg, és a reach-kampány a rendszer eldobja.</span><span class="sxs-lookup"><span data-stu-id="e24b7-178">The notification won't be displayed and the reach campaign will be dropped.</span></span>
> 
> 

<span data-ttu-id="e24b7-179">Az elrendezés megvalósítási leegyszerűsítése is biztosítunk saját XAML-kódot, amely a kódot is szolgálhatnak.</span><span class="sxs-lookup"><span data-stu-id="e24b7-179">To simplify your layout implementation, we also provide our own xaml which can serve as a basis for your code.</span></span> <span data-ttu-id="e24b7-180">Az Engagement SDK archívumban találhatók (/ src/reach /).</span><span class="sxs-lookup"><span data-stu-id="e24b7-180">They are located in the Engagement SDK archive (/src/reach/).</span></span>

> [!WARNING]
> <span data-ttu-id="e24b7-181">Az általunk biztosított forrásai pontos ugyanazokat a portokat, amelyek jelenleg használják.</span><span class="sxs-lookup"><span data-stu-id="e24b7-181">The sources that we provide are the exact same ones that we use.</span></span> <span data-ttu-id="e24b7-182">Ezért ha azokat közvetlenül módosítani szeretné, ne feledje módosítani a névtér és a neve.</span><span class="sxs-lookup"><span data-stu-id="e24b7-182">So if you want to modify them directly, don't forget to change the namespace and the name.</span></span>
> 
> 

### <a name="notification-position"></a><span data-ttu-id="e24b7-183">Értesítési pozíciója</span><span class="sxs-lookup"><span data-stu-id="e24b7-183">Notification position</span></span>
<span data-ttu-id="e24b7-184">Alapértelmezés szerint bal oldalán található az alkalmazás alján egy alkalmazásbeli értesítés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e24b7-184">By default, an in-app notification is displayed at the bottom left side of the application.</span></span> <span data-ttu-id="e24b7-185">Ez a viselkedés felülbírálható módosíthatja a `GetNotificationPosition` metódusában a `EngagementReachHandler` objektum.</span><span class="sxs-lookup"><span data-stu-id="e24b7-185">You can change this behavior by overriding the `GetNotificationPosition` method of the `EngagementReachHandler` object.</span></span>

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

<span data-ttu-id="e24b7-186">Jelenleg, választhat a `BOTTOM` (alapértelmezett) és `TOP` pozíciók.</span><span class="sxs-lookup"><span data-stu-id="e24b7-186">Currently, you can choose between the `BOTTOM` (default) and `TOP` positions.</span></span>

### <a name="launch-message"></a><span data-ttu-id="e24b7-187">Indítsa el az üzenet</span><span class="sxs-lookup"><span data-stu-id="e24b7-187">Launch message</span></span>
<span data-ttu-id="e24b7-188">Amikor egy felhasználó egy Rendszerértesítő (egy bejelentési) kattint, az Engagement elindítja az alkalmazást, a leküldéses üzenetek a tartalom betöltése, és a megfelelő kampány lap megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="e24b7-188">When a user clicks on a system notification (a toast), Engagement launches the app, load the content of the push messages, and display the page for the corresponding campaign.</span></span>

<span data-ttu-id="e24b7-189">A késleltetés van a indítsa el az alkalmazás és a lap (attól függően, hogy a hálózat sebességétől) megjelenítési között.</span><span class="sxs-lookup"><span data-stu-id="e24b7-189">There is a delay between the launch of the application and the display of the page (depending on the speed of your network).</span></span>

<span data-ttu-id="e24b7-190">Jelzi a felhasználóknak, hogy valami tölt, egy visual adatok, például egy folyamatjelző vagy egy folyamatjelző kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="e24b7-190">To indicate to the user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="e24b7-191">Bevonási nem tudja kezelni, hogy saját magát, de itt néhány kezelők meg.</span><span class="sxs-lookup"><span data-stu-id="e24b7-191">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="e24b7-192">A visszahívás megvalósításához, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="e24b7-192">To implement the callback, do:</span></span>

    /* The application has launched and the content is loading.
     * You should display an indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

    /* The application has finished loading the content and the page
     * is about to be displayed.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

    /* The content has been loaded, but an error has occurred.
     * You can provide an information to the user.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

<span data-ttu-id="e24b7-193">Állíthatja be a visszahívást a `Application_Launching` metódusában a `App.xaml.cs` fájlt, mielőtt lehetőleg a `EngagementReach.Instance.Init()` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="e24b7-193">You can set the callback in your `Application_Launching` method of your `App.xaml.cs` file, preferably before the `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="e24b7-194">A felhasználói felület szálából minden kezelő hívja.</span><span class="sxs-lookup"><span data-stu-id="e24b7-194">Each handler is called by the UI Thread.</span></span> <span data-ttu-id="e24b7-195">Nincs a MessageBox vagy a felhasználói felület kapcsolatos valami használatakor foglalkoznia.</span><span class="sxs-lookup"><span data-stu-id="e24b7-195">You do not have to worry when using a MessageBox or something UI-related.</span></span>
> 
> 

<span data-ttu-id="e24b7-196">[alkalmazás-házirendek]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="e24b7-196">[Application Policies]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx</span></span>
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
<span data-ttu-id="e24b7-197">[Adott alkalmazás esetében további követelmények]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="e24b7-197">[Additional Requirements for Specific Application Types]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx</span></span>

