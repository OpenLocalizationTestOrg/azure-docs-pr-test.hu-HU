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
# <a name="windows-universal-apps-reach-sdk-integration"></a><span data-ttu-id="9bc36-103">Univerzális Windows-alkalmazások Reach SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="9bc36-103">Windows Universal Apps Reach SDK Integration</span></span>
<span data-ttu-id="9bc36-104">Hello ismertetett hello integrációs eljárást kell követni, [Windows Universal Engagement SDK-integráció](mobile-engagement-windows-store-integrate-engagement.md) Ez az útmutató követése előtt.</span><span class="sxs-lookup"><span data-stu-id="9bc36-104">You must follow hello integration procedure described in hello [Windows Universal Engagement SDK Integration](mobile-engagement-windows-store-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-universal-project"></a><span data-ttu-id="9bc36-105">A univerzális Windows-projektben Engagement Reach SDK hello beágyazása</span><span class="sxs-lookup"><span data-stu-id="9bc36-105">Embed hello Engagement Reach SDK into your Windows Universal project</span></span>
<span data-ttu-id="9bc36-106">Nincs semmi tooadd.</span><span class="sxs-lookup"><span data-stu-id="9bc36-106">You do not have anything tooadd.</span></span> <span data-ttu-id="9bc36-107">`EngagementReach`hivatkozások és erőforrások még a projektben.</span><span class="sxs-lookup"><span data-stu-id="9bc36-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="9bc36-108">Testre szabhatja a hello található képek `Resources` mappa a projekt, különösen akkor hello márka ikon (adott alapértelmezett toohello Engagement ikon).</span><span class="sxs-lookup"><span data-stu-id="9bc36-108">You can customize images located in hello `Resources` folder of your project, especially hello brand icon (that default toohello Engagement icon).</span></span> <span data-ttu-id="9bc36-109">Univerzális alkalmazások is áthelyezheti hello `Resources` a benne lévő tartalom alkalmazásokat, de közötti lesz megosztott projekt tooshare mappájába tookeep hello `Resources\EngagementConfiguration.xml` alapértelmezett helyére, a fájlt, mert függő platformra.</span><span class="sxs-lookup"><span data-stu-id="9bc36-109">On Universal Apps you can also move hello `Resources` folder on your shared project tooshare its content between apps, but you will have tookeep hello `Resources\EngagementConfiguration.xml` file on its default location as it is platform dependent.</span></span>
> 
> 

## <a name="enable-hello-windows-notification-service"></a><span data-ttu-id="9bc36-110">Hello Windows értesítési szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9bc36-110">Enable hello Windows Notification Service</span></span>
### <a name="windows-8x-and-windows-phone-81-only"></a><span data-ttu-id="9bc36-111">A Windows 8.x és Windows Phone 8.1 esetén</span><span class="sxs-lookup"><span data-stu-id="9bc36-111">Windows 8.x and Windows Phone 8.1 only</span></span>
<span data-ttu-id="9bc36-112">A sorrend toouse hello **Windows értesítési szolgáltatásával** (WNS néven) az a `Package.appxmanifest` fájlja `Application UI` kattintson a `All Image Assets` hello bal oldali botot mezőbe.</span><span class="sxs-lookup"><span data-stu-id="9bc36-112">In order toouse hello **Windows Notification Service** (referred as WNS) in your `Package.appxmanifest` file on `Application UI` click on `All Image Assets` in hello left bot box.</span></span> <span data-ttu-id="9bc36-113">Jobb listájának hello hello: `Notifications`, módosítsa `toast capable` a `(not set)` túl`(Yes)`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-113">At hello right of hello box in `Notifications`, change `toast capable` from `(not set)` too`(Yes)`.</span></span>

### <a name="all-platforms"></a><span data-ttu-id="9bc36-114">Összes platform</span><span class="sxs-lookup"><span data-stu-id="9bc36-114">All platforms</span></span>
<span data-ttu-id="9bc36-115">Az alkalmazás tooyour Microsoft fiók és toohello engagement platform kell toosynchronize.</span><span class="sxs-lookup"><span data-stu-id="9bc36-115">You need toosynchronize your app tooyour Microsoft account and toohello engagement platform.</span></span> <span data-ttu-id="9bc36-116">Ez a fiók toocreate kell, vagy jelentkezzen be [windows fejlesztői központ](https://dev.windows.com).</span><span class="sxs-lookup"><span data-stu-id="9bc36-116">For this you need toocreate an account or log on [windows dev center](https://dev.windows.com).</span></span> <span data-ttu-id="9bc36-117">Miután, hozzon létre egy új alkalmazást, és található hello biztonsági AZONOSÍTÓT és a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="9bc36-117">After that create a new application and find hello SID and secret key.</span></span> <span data-ttu-id="9bc36-118">A hello engagement előtér, nyissa meg az Alkalmazásbeállítás a `native push` , majd illessze be a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="9bc36-118">On hello engagement frontend, go on your app setting in `native push` and paste your credentials.</span></span> <span data-ttu-id="9bc36-119">Ezt követően kattintson a jobb gombbal a projektre, válassza `store` és `Associate App with hello Store...`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-119">After that, right click on your project, select `store` and `Associate App with hello Store...`.</span></span> <span data-ttu-id="9bc36-120">Egyszerűen tooselect hello alkalmazás rendelkezik létrehozása előtt toosynchronize.</span><span class="sxs-lookup"><span data-stu-id="9bc36-120">You just need tooselect hello application you have create before toosynchronize it.</span></span>

## <a name="initialize-hello-engagement-reach-sdk"></a><span data-ttu-id="9bc36-121">Hello Engagement Reach SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="9bc36-121">Initialize hello Engagement Reach SDK</span></span>
<span data-ttu-id="9bc36-122">Módosítsa a hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="9bc36-122">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="9bc36-123">Helyezze be `EngagementReach.Instance.Init` után csak `EngagementAgent.Instance.Init` a a `InitEngagement` módszert:</span><span class="sxs-lookup"><span data-stu-id="9bc36-123">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in your `InitEngagement` method:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  <span data-ttu-id="9bc36-124">Hello `EngagementReach.Instance.Init` egy dedikált szálat futtat.</span><span class="sxs-lookup"><span data-stu-id="9bc36-124">hello `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="9bc36-125">Nem rendelkezik toodo azt saját maga.</span><span class="sxs-lookup"><span data-stu-id="9bc36-125">You do not have toodo it yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="9bc36-126">Ha máshol használják a leküldéses értesítések a az alkalmazás, akkor túl van[megosztani a leküldéses csatorna](#push-channel-sharing) az Engagement Reach.</span><span class="sxs-lookup"><span data-stu-id="9bc36-126">If you are using push notifications elsewhere in your application then you have too[share your push channel](#push-channel-sharing) with Engagement Reach.</span></span>
> 
> 

## <a name="integration"></a><span data-ttu-id="9bc36-127">Integráció</span><span class="sxs-lookup"><span data-stu-id="9bc36-127">Integration</span></span>
<span data-ttu-id="9bc36-128">Bevonási kétféleképpen tooadd hello Reach alkalmazásbeli szalagok és közbeszúrt kínál a hirdetmények és szavazások az alkalmazásban: hello átfedő integrációs és hello webes nézetek manuális integráció.</span><span class="sxs-lookup"><span data-stu-id="9bc36-128">Engagement provides two ways tooadd hello Reach in-app banners and interstitial views for announcements and polls in your application: hello overlay integration and hello web views manual integration.</span></span> <span data-ttu-id="9bc36-129">Mindkét megközelítés a hello ötvözze nem ugyanazon az oldalon.</span><span class="sxs-lookup"><span data-stu-id="9bc36-129">You should not combine both approach on hello same page.</span></span>

<span data-ttu-id="9bc36-130">sikerült hello két integrációs hello választást összesíthető ily módon:</span><span class="sxs-lookup"><span data-stu-id="9bc36-130">hello choice between hello two integration could be summarized this way:</span></span>

* <span data-ttu-id="9bc36-131">Hello átfedéses integráció úgy is dönthet, ha a lapok már örököl hello ügynök `EngagementPage`, csak cseréje kérdése `EngagementPage` által `EngagementPageOverlay` és `xmlns:engagement="using:Microsoft.Azure.Engagement"` által `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` a lapokon.</span><span class="sxs-lookup"><span data-stu-id="9bc36-131">You may choose hello overlay integration if your pages already inherits from hello Agent `EngagementPage`, it's just a matter of replacing `EngagementPage` by `EngagementPageOverlay` and `xmlns:engagement="using:Microsoft.Azure.Engagement"` by `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in your pages.</span></span>
* <span data-ttu-id="9bc36-132">Dönthet hello webes nézetek manuális integrációs Ha tooprecisely hely hello UI elérni a lapokon belül, vagy ha nem szeretné, hogy tooadd egy másik öröklési szintű tooyour lapokat.</span><span class="sxs-lookup"><span data-stu-id="9bc36-132">You may choose hello web views manual integration if you want tooprecisely place hello Reach UI within your pages or if you don't want tooadd another inheritance level tooyour pages.</span></span> 

### <a name="overlay-integration"></a><span data-ttu-id="9bc36-133">Átfedéses integráció</span><span class="sxs-lookup"><span data-stu-id="9bc36-133">Overlay integration</span></span>
<span data-ttu-id="9bc36-134">hello Engagement átfedő dinamikusan kerülnek be a hello felhasználói felületi elemei toodisplay Reach-kampányokat szerepel a oldalon.</span><span class="sxs-lookup"><span data-stu-id="9bc36-134">hello Engagement overlay dynamically adds hello UI elements used toodisplay Reach campaigns in your page.</span></span> <span data-ttu-id="9bc36-135">Ha hello átfedő nem felelnek meg a elrendezést érdemes hello webes nézetek manuális integrációs helyette.</span><span class="sxs-lookup"><span data-stu-id="9bc36-135">If hello overlay doesn't suit your layout you should consider hello web views manual integration instead.</span></span>

<span data-ttu-id="9bc36-136">A .xaml fájlban változásával `EngagementPage` túl hivatkozik`EngagementPageOverlay`</span><span class="sxs-lookup"><span data-stu-id="9bc36-136">In your .xaml file change `EngagementPage` reference too`EngagementPageOverlay`</span></span>

* <span data-ttu-id="9bc36-137">Tooyour névtér-deklarációk hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="9bc36-137">Add tooyour namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* <span data-ttu-id="9bc36-138">Cserélje le `engagement:EngagementPage` rendelkező `engagement:EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="9bc36-138">Replace `engagement:EngagementPage` with `engagement:EngagementPageOverlay`:</span></span>

<span data-ttu-id="9bc36-139">**A EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="9bc36-139">**With EngagementPage:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

<span data-ttu-id="9bc36-140">**A EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="9bc36-140">**With EngagementPageOverlay:**</span></span>

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

<span data-ttu-id="9bc36-141">Ezután a .cs fájlban a lap a címke `EngagementPageOverlay` helyett `EngagementPage` , majd importálja `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-141">Then in your .cs file tag your page in `EngagementPageOverlay` instead of `EngagementPage` and import `Microsoft.Azure.Engagement.Overlay`.</span></span>

            using Microsoft.Azure.Engagement.Overlay;

* <span data-ttu-id="9bc36-142">Cserélje le `EngagementPage` rendelkező `EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="9bc36-142">Replace `EngagementPage` with `EngagementPageOverlay`:</span></span>

<span data-ttu-id="9bc36-143">**A EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="9bc36-143">**With EngagementPage:**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

<span data-ttu-id="9bc36-144">**A EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="9bc36-144">**With EngagementPageOverlay:**</span></span>

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


<span data-ttu-id="9bc36-145">hello Engagement átfedő ad hozzá egy `Grid` elem felett a lap a elrendezés és a két hello álló `WebView` hello elemei egy szalag, és más hello közbeszúrt nézet egy hello.</span><span class="sxs-lookup"><span data-stu-id="9bc36-145">hello Engagement overlay adds a `Grid` element on top of your page composed of your layout and hello two `WebView` elements one for hello banner and hello other one for hello interstitial view.</span></span>

<span data-ttu-id="9bc36-146">Testre szabhatja a hello átfedő elemet közvetlenül a hello `EngagementPageOverlay.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9bc36-146">You can customize hello overlay elements directly in hello `EngagementPageOverlay.cs` file.</span></span>

### <a name="web-views-manual-integration"></a><span data-ttu-id="9bc36-147">Webes nézetek manuális integráció</span><span class="sxs-lookup"><span data-stu-id="9bc36-147">Web views manual integration</span></span>
<span data-ttu-id="9bc36-148">Reach keresnek a lapokat a hello két `WebView` felelős hello Szalagcím és hello közbeszúrt nézet megjelenítése elemet.</span><span class="sxs-lookup"><span data-stu-id="9bc36-148">Reach will be searching your pages for hello two `WebView` elements responsible for displaying hello banner and hello interstitial view.</span></span> <span data-ttu-id="9bc36-149">csak annyi teendője van, toodo tooadd hello e két `WebView` elemek valahol a lapokon Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="9bc36-149">hello only thing you have toodo is tooadd those two `WebView` elements somewhere in your pages, here is an example:</span></span>

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


<span data-ttu-id="9bc36-150">Az ebben a példában hello `WebView` elemek felhőbe archivált toofit vannak a tároló, amely automatikusan újra méretezi őket képernyő elforgatásának vagy ablak méretének módosítása esetén.</span><span class="sxs-lookup"><span data-stu-id="9bc36-150">In this example hello `WebView` elements are stretched toofit their container which automatically re-sizes them in case of screen rotation or window size change.</span></span>

> [!WARNING]
> <span data-ttu-id="9bc36-151">Fontos tookeep hello azonos elnevezési `engagement_notification_content` és `engagement_announcement_content` a hello `WebView` elemek.</span><span class="sxs-lookup"><span data-stu-id="9bc36-151">It is important tookeep hello same naming `engagement_notification_content` and `engagement_announcement_content` for hello `WebView` elements.</span></span> <span data-ttu-id="9bc36-152">Reach azonosítja azokat a nevük számára.</span><span class="sxs-lookup"><span data-stu-id="9bc36-152">Reach is identifying them by their name.</span></span> 
> 
> 

## <a name="handle-datapush-optional"></a><span data-ttu-id="9bc36-153">Leíró datapush (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="9bc36-153">Handle datapush (optional)</span></span>
<span data-ttu-id="9bc36-154">Ha azt szeretné, hogy az alkalmazás toobe képes tooreceive Reach adatleküldések, két események tooimplement hello EngagementReach osztály van:</span><span class="sxs-lookup"><span data-stu-id="9bc36-154">If you want your application toobe able tooreceive Reach data pushes, you have tooimplement two events of hello EngagementReach class:</span></span>

<span data-ttu-id="9bc36-155">Hello App() konstruktor az App.xaml.cs fájlban adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="9bc36-155">In App.xaml.cs in hello App() constructor add:</span></span>

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

<span data-ttu-id="9bc36-156">Láthatja, hogy az egyes metódus értéket ad vissza egy logikai érték hello a visszahívás.</span><span class="sxs-lookup"><span data-stu-id="9bc36-156">You can see that hello callback of each method returns a boolean.</span></span> <span data-ttu-id="9bc36-157">Engagement küld egy visszajelzés tooits háttér-után hello adatleküldés terjesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="9bc36-157">Engagement sends a feedback tooits back-end after dispatching hello data push.</span></span> <span data-ttu-id="9bc36-158">Hello visszahívási hamis értéket ad vissza, ha hello `exit` visszajelzés küldése lesz.</span><span class="sxs-lookup"><span data-stu-id="9bc36-158">If hello callback returns false, hello `exit` feedback will be send.</span></span> <span data-ttu-id="9bc36-159">Ellenkező esetben lesz `action`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-159">Otherwise, it will be `action`.</span></span> <span data-ttu-id="9bc36-160">Ha nincs visszahívás hello események, hello `drop` visszajelzés visszaadott tooEngagement.</span><span class="sxs-lookup"><span data-stu-id="9bc36-160">If no callback is set for hello events, hello `drop` feedback will be returned tooEngagement.</span></span>

> [!WARNING]
> <span data-ttu-id="9bc36-161">Bevonási nem tud tooreceive Többszörösök visszajelzése van, az adatokat.</span><span class="sxs-lookup"><span data-stu-id="9bc36-161">Engagement is not able tooreceive multiples feedbacks for a data push.</span></span> <span data-ttu-id="9bc36-162">Ha azt tervezi, tooset több kezelők egy olyan eseményre, vegye figyelembe, hogy hello visszajelzés felel meg toohello legutóbb elküldött.</span><span class="sxs-lookup"><span data-stu-id="9bc36-162">If you plan tooset several handlers on an event, be aware that hello feedback will correspond toohello last one sent.</span></span> <span data-ttu-id="9bc36-163">Ebben az esetben ajánlott tooalways értéket ad vissza hello zavaró visszajelzés rendelkező előtér-hello azonos érték tooavoid.</span><span class="sxs-lookup"><span data-stu-id="9bc36-163">In this case, we recommend tooalways returns hello same value tooavoid having confusing feedback on hello front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="9bc36-164">(Választható) felhasználói felület testreszabása</span><span class="sxs-lookup"><span data-stu-id="9bc36-164">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="9bc36-165">Első lépés</span><span class="sxs-lookup"><span data-stu-id="9bc36-165">First step</span></span>
<span data-ttu-id="9bc36-166">Azt teszik toocustomize hello reach felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="9bc36-166">We allow you toocustomize hello reach UI.</span></span>

<span data-ttu-id="9bc36-167">toodo úgy, hogy toocreate hello alosztályát `EngagementReachHandler` osztály.</span><span class="sxs-lookup"><span data-stu-id="9bc36-167">toodo so, you have toocreate a subclass of hello `EngagementReachHandler` class.</span></span>

<span data-ttu-id="9bc36-168">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="9bc36-168">**Sample Code :**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

<span data-ttu-id="9bc36-169">Állítsa hello hello tartalmának `EngagementReach.Instance.Handler` mező található az egyéni objektum a `App.xaml.cs` hello osztály `App()` metódust.</span><span class="sxs-lookup"><span data-stu-id="9bc36-169">Then, set hello content of hello `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within hello `App()` method.</span></span>

<span data-ttu-id="9bc36-170">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="9bc36-170">**Sample Code :**</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> <span data-ttu-id="9bc36-171">Alapértelmezés szerint az Engagement használja a saját végrehajtásának `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-171">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span>
> <span data-ttu-id="9bc36-172">Nincs toocreate a saját, és ha így tesz, akkor nem kell toooverride minden metódus.</span><span class="sxs-lookup"><span data-stu-id="9bc36-172">You don't have toocreate your own, and if you do so, you don't have toooverride every method.</span></span> <span data-ttu-id="9bc36-173">hello alapértelmezés tooselect hello Engagement alapobjektum lesz.</span><span class="sxs-lookup"><span data-stu-id="9bc36-173">hello default behavior is tooselect hello Engagement base object.</span></span>
> 
> 

### <a name="web-view"></a><span data-ttu-id="9bc36-174">Webes nézet</span><span class="sxs-lookup"><span data-stu-id="9bc36-174">Web View</span></span>
<span data-ttu-id="9bc36-175">Alapértelmezés szerint a Reach hello beágyazott erőforrások hello DLL toodisplay hello értesítések és lapokat fogja használni.</span><span class="sxs-lookup"><span data-stu-id="9bc36-175">By default, Reach will use hello embedded resources of hello DLL toodisplay hello notifications and pages.</span></span>

<span data-ttu-id="9bc36-176">teljes tooprovide testreszabási lehetőségek webes nézet csak használjuk.</span><span class="sxs-lookup"><span data-stu-id="9bc36-176">tooprovide a full customization possibilities we only use web view.</span></span> <span data-ttu-id="9bc36-177">Ha azt szeretné, hogy toocustomize elrendezések, bírálja felül közvetlenül hello erőforrások fájlok `EngagementAnnouncement.html` és `EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-177">If you want toocustomize layouts, override directly hello resources files `EngagementAnnouncement.html` and `EngagementNotification.html`.</span></span> <span data-ttu-id="9bc36-178">Bevonási kell az összes kódot `<body></body>` toorun megfelelően.</span><span class="sxs-lookup"><span data-stu-id="9bc36-178">Engagement needs all code in `<body></body>` toorun correctly.</span></span> <span data-ttu-id="9bc36-179">Külső címkét adhat hozzá, de `engagement_webview_area`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-179">But you can add tag outer `engagement_webview_area`.</span></span>

<span data-ttu-id="9bc36-180">Azonban dönthet úgy is toouse saját erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="9bc36-180">However, you can decide toouse your own resources.</span></span>

<span data-ttu-id="9bc36-181">Ha szeretné felülbírálni az `EngagementReachHandler` az alosztály tootell Engagement toouse módszerek az elrendezést, de igénybe vehet gondot tooembedded hello engagement mechanizmus:</span><span class="sxs-lookup"><span data-stu-id="9bc36-181">You can override `EngagementReachHandler` methods in your subclass tootell Engagement toouse your layouts, but take care tooembedded hello engagement mechanism:</span></span>

<span data-ttu-id="9bc36-182">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="9bc36-182">**Sample Code :**</span></span>

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


<span data-ttu-id="9bc36-183">Alapértelmezés szerint AnnouncementHTML van `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-183">By default, AnnouncementHTML is `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span></span> <span data-ttu-id="9bc36-184">Azt jelenti, hogy hello html-fájlba (szöveges közlemény, webes anoucement és lekérdezési közlemény) leküldéses üzenet hello tartalmának tervezése.</span><span class="sxs-lookup"><span data-stu-id="9bc36-184">It represents hello html file that design hello content of a push message (Text announcement, Web anoucement and Poll announcement).</span></span> <span data-ttu-id="9bc36-185">AnnouncementName van `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-185">AnnouncementName is `engagement_announcement_content`.</span></span> <span data-ttu-id="9bc36-186">Az xaml-oldal hello webes nézet tervéhez hello nevét is.</span><span class="sxs-lookup"><span data-stu-id="9bc36-186">It is hello name of hello webview design in your xaml page.</span></span>

<span data-ttu-id="9bc36-187">NotfificationHTML van `ms-appx-web:///Resources/EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-187">NotfificationHTML is `ms-appx-web:///Resources/EngagementNotification.html`.</span></span> <span data-ttu-id="9bc36-188">Azt jelenti, hogy hello html-fájl, amely a leküldéses üzenet hello értesítési tervezése.</span><span class="sxs-lookup"><span data-stu-id="9bc36-188">It represents hello html file that design hello notification of a push message.</span></span> <span data-ttu-id="9bc36-189">NotfificationName van `engagement_notification_content`.</span><span class="sxs-lookup"><span data-stu-id="9bc36-189">NotfificationName is `engagement_notification_content`.</span></span> <span data-ttu-id="9bc36-190">Az xaml-oldal hello webes nézet tervéhez hello nevét is.</span><span class="sxs-lookup"><span data-stu-id="9bc36-190">It is hello name of hello webview design in your xaml page.</span></span>

### <a name="customization"></a><span data-ttu-id="9bc36-191">Testreszabás</span><span class="sxs-lookup"><span data-stu-id="9bc36-191">Customization</span></span>
<span data-ttu-id="9bc36-192">Értesítés és a webes nézet azt szeretné, hogy ha megőrizheti a bevonási objektum rendelkezik közlemény személyre is szabhatja.</span><span class="sxs-lookup"><span data-stu-id="9bc36-192">You can customize notification and announcement web view has you want if you preserve Engagement object.</span></span> <span data-ttu-id="9bc36-193">Mi gondoskodunk, hogy webes nézet objektum ismertetett három alkalommal – hello először az XAML-kódban, második időben hello "setwebview()" metódusban a .cs fájlban, és harmadik idő hello HTML-fájlban.</span><span class="sxs-lookup"><span data-stu-id="9bc36-193">Take care that webview object is described three times - hello first time in your xaml, second time in your .cs file in hello "setwebview()" method, and third time in hello html file.</span></span>

* <span data-ttu-id="9bc36-194">Az xaml hello aktuális grafikus elrendezés webes nézet összetevő ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9bc36-194">In your xaml you describe hello current graphical layout webview component.</span></span>
* <span data-ttu-id="9bc36-195">A .cs fájlban definiálhat "setwebview()" beállította hello két webes nézet (értesítés, bejelentés) hello dimenziója.</span><span class="sxs-lookup"><span data-stu-id="9bc36-195">In your .cs file you can define "setwebview()" in which you set hello dimension of hello two webview (notification, announcement).</span></span> <span data-ttu-id="9bc36-196">Akkor nagyon hatékony hello alkalmazás átméretezésekor.</span><span class="sxs-lookup"><span data-stu-id="9bc36-196">It is very effective when hello application is resizing.</span></span>
* <span data-ttu-id="9bc36-197">Hello Engagement HTML-fájlban azt leíró hello webes nézet tartalom, tervezési és hello elemek pozíciók egymás között.</span><span class="sxs-lookup"><span data-stu-id="9bc36-197">In hello Engagement html file we describe hello webview content, design and hello elements positions between each other.</span></span>

### <a name="launch-message"></a><span data-ttu-id="9bc36-198">Indítsa el az üzenet</span><span class="sxs-lookup"><span data-stu-id="9bc36-198">Launch message</span></span>
<span data-ttu-id="9bc36-199">Ha a felhasználó kattint, a rendszer értesítést (egy bejelentési), Engagement elindítja hello alkalmazást, hello hello tartalmának betöltése leküldéses üzenetek, hello megfelelő kampány hello lap megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9bc36-199">When a user clicks on a system notification (a toast), Engagement launches hello application, load hello content of hello push messages, and display hello page for hello corresponding campaign.</span></span>

<span data-ttu-id="9bc36-200">Nincs késleltetés hello indítási hello alkalmazás és hello megjelenítés hello lap (attól függően, hogy a hálózat sebességétől hello) között.</span><span class="sxs-lookup"><span data-stu-id="9bc36-200">There is a delay between hello launch of hello application and hello display of hello page (depending on hello speed of your network).</span></span>

<span data-ttu-id="9bc36-201">valami tölt toohello felhasználói tooindicate, kell biztosítania egy visual adatok, például egy folyamatjelző vagy egy folyamatjelző.</span><span class="sxs-lookup"><span data-stu-id="9bc36-201">tooindicate toohello user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="9bc36-202">Bevonási nem tudja kezelni, hogy saját magát, de itt néhány kezelők meg.</span><span class="sxs-lookup"><span data-stu-id="9bc36-202">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="9bc36-203">tooimplement hello visszahívási "Nyilvános App() megkeresése" App.xaml.cs fájlban adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="9bc36-203">tooimplement hello callback, in App.xaml.cs in "Public App(){}" add:</span></span>

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

<span data-ttu-id="9bc36-204">Hello visszahívási állíthatja be a "Nyilvános App() {}" metódusában a `App.xaml.cs` fájl, lehetőleg előtt hello `EngagementReach.Instance.Init()` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="9bc36-204">You can set hello callback in your "Public App(){}" method of your `App.xaml.cs` file, preferably before hello `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="9bc36-205">Minden egyes kezelő felhasználói felület szálából hello hívja.</span><span class="sxs-lookup"><span data-stu-id="9bc36-205">Each handler is called by hello UI Thread.</span></span> <span data-ttu-id="9bc36-206">Önnek nincs tooworry MessageBox vagy valami felhasználói felület kapcsolatos használatakor.</span><span class="sxs-lookup"><span data-stu-id="9bc36-206">You do not have tooworry when using a MessageBox or something UI-related.</span></span>
> 
> 

## <span data-ttu-id="9bc36-207"><a id="push-channel-sharing"></a>Leküldéses csatorna megosztása</span><span class="sxs-lookup"><span data-stu-id="9bc36-207"><a id="push-channel-sharing"></a> Push channel sharing</span></span>
<span data-ttu-id="9bc36-208">Használata leküldéses értesítések más célra az alkalmazás ezután rendelkezik toouse hello leküldéses csatorna megosztása a hello Engagement SDK szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9bc36-208">If you are using push notifications for another purpose in your application then you have toouse hello push channel sharing feature of hello Engagement SDK.</span></span> <span data-ttu-id="9bc36-209">Ez az elmulasztott tooavoid leküldéses.</span><span class="sxs-lookup"><span data-stu-id="9bc36-209">This is tooavoid missed push.</span></span>

* <span data-ttu-id="9bc36-210">Megadhatja a saját leküldéses csatorna toohello Engagement Reach inicializálása.</span><span class="sxs-lookup"><span data-stu-id="9bc36-210">You can provide your own push channel toohello Engagement Reach initialization.</span></span> <span data-ttu-id="9bc36-211">hello SDK helyett egy új kérő használja.</span><span class="sxs-lookup"><span data-stu-id="9bc36-211">hello SDK will use it instead of requesting a new one.</span></span>

<span data-ttu-id="9bc36-212">Frissítse a leküldéses csatorna hello hello Engagement Reach inicializálási `InitEngagement` hello metódusnak `App.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="9bc36-212">Update hello Engagement Reach initialization with your push channel in hello `InitEngagement` method from hello `App.xaml.cs` file:</span></span>

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* <span data-ttu-id="9bc36-213">Azt is megteheti Ha csak tooconsume hello leküldéses csatorna hello Reach inicializálás után majd beállíthatja egy visszahívási Engagement Reach tooget hello leküldéses csatornán hello SDK által létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="9bc36-213">Alternatively, if you just want tooconsume hello push channel after hello Reach initialization then you can set a callback on Engagement Reach tooget hello push channel once it is created by hello SDK.</span></span>

<span data-ttu-id="9bc36-214">Állítsa be a visszahívási bárhol **után** Reach inicializálási hello:</span><span class="sxs-lookup"><span data-stu-id="9bc36-214">Set your callback at any place **after** hello Reach initialization :</span></span>

    /* Set action on hello SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* hello forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a><span data-ttu-id="9bc36-215">Egyéni séma tipp</span><span class="sxs-lookup"><span data-stu-id="9bc36-215">Custom scheme tip</span></span>
<span data-ttu-id="9bc36-216">Azt adja meg az egyéni séma használja.</span><span class="sxs-lookup"><span data-stu-id="9bc36-216">We provide custom scheme use.</span></span> <span data-ttu-id="9bc36-217">A bevonási előtér toobe az engagement alkalmazásban használt URI különböző típusú küldhet.</span><span class="sxs-lookup"><span data-stu-id="9bc36-217">You can send different type of URI from engagement frontend toobe used in your engagement application.</span></span> <span data-ttu-id="9bc36-218">Alapértelmezett séma például `http, ftp, ...` vannak kezelése Windows, illetve egy ablak lesz kérdezzen rá, ha telepítve az eszközön alapértelmezés szerinti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9bc36-218">Default scheme like `http, ftp, ...` are manage by Windows, a window will prompt if they are no default application installed on device.</span></span> <span data-ttu-id="9bc36-219">Az alkalmazás a saját egyéni séma is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="9bc36-219">You can also create your own custom scheme for your application.</span></span>

<span data-ttu-id="9bc36-220">hello egyszerűen tooset egy egyéni séma az alkalmazásban tooopen a `Package.appxmanifest` nyissa meg a `Declarations` panel.</span><span class="sxs-lookup"><span data-stu-id="9bc36-220">hello simple way tooset a custom scheme in your application is tooopen your `Package.appxmanifest` go in `Declarations` panel.</span></span> <span data-ttu-id="9bc36-221">Válassza ki `Protocol` hello elérhető nyilatkozatok görgessen a mezőbe, majd vegye fel azt.</span><span class="sxs-lookup"><span data-stu-id="9bc36-221">Select `Protocol` in hello Available Declarations scroll box and add it.</span></span> <span data-ttu-id="9bc36-222">Hello szerkesztése `Name` mező az új protokoll a kívánt nevet.</span><span class="sxs-lookup"><span data-stu-id="9bc36-222">Edit hello `Name` field with your new protocol desired name.</span></span>

<span data-ttu-id="9bc36-223">Most toouse Ez a protokoll szerkesztése a `App.xaml.cs` a hello `OnActivated` metódust, és ne feledkezzen meg is tooinitialize engagement itt:</span><span class="sxs-lookup"><span data-stu-id="9bc36-223">Now toouse this protocol, edit your `App.xaml.cs` with hello `OnActivated` method, and don't forget tooinitialize engagement here also:</span></span>

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

