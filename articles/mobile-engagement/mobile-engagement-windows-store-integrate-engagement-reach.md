---
title: "Univerzális Windows-alkalmazások Reach SDK-integráció"
description: "Az Azure Mobile Engagement Reach integrálása univerzális Windows-alkalmazások"
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
ms.openlocfilehash: 9311e998e67d8d0d56da68fc9460df32ce7ce5a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a><span data-ttu-id="c8145-103">Univerzális Windows-alkalmazások Reach SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="c8145-103">Windows Universal Apps Reach SDK Integration</span></span>
<span data-ttu-id="c8145-104">Az integráció az ismertetett eljárást kell követni a [Windows Universal Engagement SDK-integráció](mobile-engagement-windows-store-integrate-engagement.md) Ez az útmutató követése előtt.</span><span class="sxs-lookup"><span data-stu-id="c8145-104">You must follow the integration procedure described in the [Windows Universal Engagement SDK Integration](mobile-engagement-windows-store-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a><span data-ttu-id="c8145-105">Az Engagement Reach SDK beágyazása a univerzális Windows-projekt</span><span class="sxs-lookup"><span data-stu-id="c8145-105">Embed the Engagement Reach SDK into your Windows Universal project</span></span>
<span data-ttu-id="c8145-106">Nincs olyan hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="c8145-106">You do not have anything to add.</span></span> <span data-ttu-id="c8145-107">`EngagementReach`hivatkozások és erőforrások még a projektben.</span><span class="sxs-lookup"><span data-stu-id="c8145-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="c8145-108">Lemezképek található, testreszabhatja a `Resources` mappa a projekt, különösen a márka ikon (hogy az Engagement ikonra az alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="c8145-108">You can customize images located in the `Resources` folder of your project, especially the brand icon (that default to the Engagement icon).</span></span> <span data-ttu-id="c8145-109">Univerzális alkalmazások át is helyezheti a `Resources` közötti alkalmazásokat, de a tartalmak megosztása a megosztott projekt mappájába kell tartani a `Resources\EngagementConfiguration.xml` alapértelmezett helyére, a fájlt, mert függő platformra.</span><span class="sxs-lookup"><span data-stu-id="c8145-109">On Universal Apps you can also move the `Resources` folder on your shared project to share its content between apps, but you will have to keep the `Resources\EngagementConfiguration.xml` file on its default location as it is platform dependent.</span></span>
> 
> 

## <a name="enable-the-windows-notification-service"></a><span data-ttu-id="c8145-110">A Windows értesítési szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c8145-110">Enable the Windows Notification Service</span></span>
### <a name="windows-8x-and-windows-phone-81-only"></a><span data-ttu-id="c8145-111">A Windows 8.x és Windows Phone 8.1 esetén</span><span class="sxs-lookup"><span data-stu-id="c8145-111">Windows 8.x and Windows Phone 8.1 only</span></span>
<span data-ttu-id="c8145-112">Ahhoz, hogy a **Windows értesítési szolgáltatásával** (WNS néven) az a `Package.appxmanifest` fájlja `Application UI` kattintson a `All Image Assets` a bal oldali botot mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c8145-112">In order to use the **Windows Notification Service** (referred as WNS) in your `Package.appxmanifest` file on `Application UI` click on `All Image Assets` in the left bot box.</span></span> <span data-ttu-id="c8145-113">A mező a jobb `Notifications`, módosítsa `toast capable` a `(not set)` való `(Yes)`.</span><span class="sxs-lookup"><span data-stu-id="c8145-113">At the right of the box in `Notifications`, change `toast capable` from `(not set)` to `(Yes)`.</span></span>

### <a name="all-platforms"></a><span data-ttu-id="c8145-114">Összes platform</span><span class="sxs-lookup"><span data-stu-id="c8145-114">All platforms</span></span>
<span data-ttu-id="c8145-115">Az alkalmazás a Microsoft-fiókjával, és az engagement platform szinkronizálni kell.</span><span class="sxs-lookup"><span data-stu-id="c8145-115">You need to synchronize your app to your Microsoft account and to the engagement platform.</span></span> <span data-ttu-id="c8145-116">Ehhez hozzon létre egy fiókot, vagy jelentkezzen be szüksége [windows fejlesztői központ](https://dev.windows.com).</span><span class="sxs-lookup"><span data-stu-id="c8145-116">For this you need to create an account or log on [windows dev center](https://dev.windows.com).</span></span> <span data-ttu-id="c8145-117">Ezután hozzon létre egy új alkalmazást, és keresse meg a biztonsági AZONOSÍTÓT és titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="c8145-117">After that create a new application and find the SID and secret key.</span></span> <span data-ttu-id="c8145-118">A bevonási előtér nyissa meg az Alkalmazásbeállítás a `native push` , majd illessze be a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c8145-118">On the engagement frontend, go on your app setting in `native push` and paste your credentials.</span></span> <span data-ttu-id="c8145-119">Ezt követően kattintson a jobb gombbal a projektre, válassza `store` és `Associate App with the Store...`.</span><span class="sxs-lookup"><span data-stu-id="c8145-119">After that, right click on your project, select `store` and `Associate App with the Store...`.</span></span> <span data-ttu-id="c8145-120">Egyszerűen jelölje be az alkalmazás rendelkezik létrehozása előtt a szinkronizáláshoz.</span><span class="sxs-lookup"><span data-stu-id="c8145-120">You just need to select the application you have create before to synchronize it.</span></span>

## <a name="initialize-the-engagement-reach-sdk"></a><span data-ttu-id="c8145-121">Az Engagement Reach SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="c8145-121">Initialize the Engagement Reach SDK</span></span>
<span data-ttu-id="c8145-122">Módosítsa a `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="c8145-122">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="c8145-123">Helyezze be `EngagementReach.Instance.Init` után csak `EngagementAgent.Instance.Init` a a `InitEngagement` módszert:</span><span class="sxs-lookup"><span data-stu-id="c8145-123">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in your `InitEngagement` method:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  <span data-ttu-id="c8145-124">A `EngagementReach.Instance.Init` egy dedikált szálat futtat.</span><span class="sxs-lookup"><span data-stu-id="c8145-124">The `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="c8145-125">Nem kell saját kezűleg elvégezni.</span><span class="sxs-lookup"><span data-stu-id="c8145-125">You do not have to do it yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="c8145-126">Ha máshol használják a leküldéses értesítések a az alkalmazás, akkor el kell [megosztani a leküldéses csatorna](#push-channel-sharing) az Engagement Reach.</span><span class="sxs-lookup"><span data-stu-id="c8145-126">If you are using push notifications elsewhere in your application then you have to [share your push channel](#push-channel-sharing) with Engagement Reach.</span></span>
> 
> 

## <a name="integration"></a><span data-ttu-id="c8145-127">Integráció</span><span class="sxs-lookup"><span data-stu-id="c8145-127">Integration</span></span>
<span data-ttu-id="c8145-128">Engagement két módot biztosít a Reach alkalmazásbeli szalagok és a hirdetmények és szavazások esetén közbeszúrt nézetek hozzáadása az alkalmazásban: a átfedéses integráció és a webes nézetek manuális integráció.</span><span class="sxs-lookup"><span data-stu-id="c8145-128">Engagement provides two ways to add the Reach in-app banners and interstitial views for announcements and polls in your application: the overlay integration and the web views manual integration.</span></span> <span data-ttu-id="c8145-129">Nem ötvözze az mindkét megközelítés ugyanazon az oldalon.</span><span class="sxs-lookup"><span data-stu-id="c8145-129">You should not combine both approach on the same page.</span></span>

<span data-ttu-id="c8145-130">A két integrációs közötti választás sikerült összesíthető ily módon:</span><span class="sxs-lookup"><span data-stu-id="c8145-130">The choice between the two integration could be summarized this way:</span></span>

* <span data-ttu-id="c8145-131">A átfedéses integráció úgy is dönthet, ha a lapok már örököl az ügynök `EngagementPage`, csak cseréje kérdése `EngagementPage` által `EngagementPageOverlay` és `xmlns:engagement="using:Microsoft.Azure.Engagement"` által `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` a lapokon.</span><span class="sxs-lookup"><span data-stu-id="c8145-131">You may choose the overlay integration if your pages already inherits from the Agent `EngagementPage`, it's just a matter of replacing `EngagementPage` by `EngagementPageOverlay` and `xmlns:engagement="using:Microsoft.Azure.Engagement"` by `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in your pages.</span></span>
* <span data-ttu-id="c8145-132">Választhatja, hogy a webes nézetek manuális integrációs Ha azt szeretné, hogy pontosan helyezhető el a Reach felhasználói felületén a lapokon belül, vagy ha nem szeretne egy másik öröklési szint hozzáadása a lapokon.</span><span class="sxs-lookup"><span data-stu-id="c8145-132">You may choose the web views manual integration if you want to precisely place the Reach UI within your pages or if you don't want to add another inheritance level to your pages.</span></span> 

### <a name="overlay-integration"></a><span data-ttu-id="c8145-133">Átfedéses integráció</span><span class="sxs-lookup"><span data-stu-id="c8145-133">Overlay integration</span></span>
<span data-ttu-id="c8145-134">Az Engagement átfedés dinamikusan kerülnek be a Reach-kampányokat a lapon való megjelenítéséhez használt felhasználói felületi elemeket.</span><span class="sxs-lookup"><span data-stu-id="c8145-134">The Engagement overlay dynamically adds the UI elements used to display Reach campaigns in your page.</span></span> <span data-ttu-id="c8145-135">Ha az átmeneti területre nem felelnek meg az elrendezésben vegye figyelembe a webes nézetek manuális integrációs helyette.</span><span class="sxs-lookup"><span data-stu-id="c8145-135">If the overlay doesn't suit your layout you should consider the web views manual integration instead.</span></span>

<span data-ttu-id="c8145-136">A .xaml fájlban változásával `EngagementPage` hivatkozás`EngagementPageOverlay`</span><span class="sxs-lookup"><span data-stu-id="c8145-136">In your .xaml file change `EngagementPage` reference to `EngagementPageOverlay`</span></span>

* <span data-ttu-id="c8145-137">Adja hozzá a következőt a névtér-deklarációkhoz:</span><span class="sxs-lookup"><span data-stu-id="c8145-137">Add to your namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* <span data-ttu-id="c8145-138">Cserélje le `engagement:EngagementPage` rendelkező `engagement:EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="c8145-138">Replace `engagement:EngagementPage` with `engagement:EngagementPageOverlay`:</span></span>

<span data-ttu-id="c8145-139">**A EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="c8145-139">**With EngagementPage:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

<span data-ttu-id="c8145-140">**A EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="c8145-140">**With EngagementPageOverlay:**</span></span>

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

<span data-ttu-id="c8145-141">Ezután a .cs fájlban a lap a címke `EngagementPageOverlay` helyett `EngagementPage` , majd importálja `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="c8145-141">Then in your .cs file tag your page in `EngagementPageOverlay` instead of `EngagementPage` and import `Microsoft.Azure.Engagement.Overlay`.</span></span>

            using Microsoft.Azure.Engagement.Overlay;

* <span data-ttu-id="c8145-142">Cserélje le `EngagementPage` rendelkező `EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="c8145-142">Replace `EngagementPage` with `EngagementPageOverlay`:</span></span>

<span data-ttu-id="c8145-143">**A EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="c8145-143">**With EngagementPage:**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

<span data-ttu-id="c8145-144">**A EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="c8145-144">**With EngagementPageOverlay:**</span></span>

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


<span data-ttu-id="c8145-145">Az Engagement átfedés ad hozzá egy `Grid` elem felett a lap a elrendezés és a két állnak `WebView` elemei egy szalagcím, a másik a közbeszúrt nézet.</span><span class="sxs-lookup"><span data-stu-id="c8145-145">The Engagement overlay adds a `Grid` element on top of your page composed of your layout and the two `WebView` elements one for the banner and the other one for the interstitial view.</span></span>

<span data-ttu-id="c8145-146">Testre szabhatja az átmeneti területre elemek közvetlenül a `EngagementPageOverlay.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="c8145-146">You can customize the overlay elements directly in the `EngagementPageOverlay.cs` file.</span></span>

### <a name="web-views-manual-integration"></a><span data-ttu-id="c8145-147">Webes nézetek manuális integráció</span><span class="sxs-lookup"><span data-stu-id="c8145-147">Web views manual integration</span></span>
<span data-ttu-id="c8145-148">Reach keresnek a lapokon a két `WebView` elemek megjelenítése a szalagcím, valamint a közbeszúrt nézet felelős.</span><span class="sxs-lookup"><span data-stu-id="c8145-148">Reach will be searching your pages for the two `WebView` elements responsible for displaying the banner and the interstitial view.</span></span> <span data-ttu-id="c8145-149">Meg kell nyitnia egyedül az, hogy adja hozzá ezeket a két `WebView` elemek valahol a lapokon Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="c8145-149">The only thing you have to do is to add those two `WebView` elements somewhere in your pages, here is an example:</span></span>

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


<span data-ttu-id="c8145-150">Ebben a példában a `WebView` megfelelően a tároló, amely automatikusan újra méretezi őket képernyő elforgatásának vagy ablak méretének módosítása esetén elemek archiválva a felhőbe.</span><span class="sxs-lookup"><span data-stu-id="c8145-150">In this example the `WebView` elements are stretched to fit their container which automatically re-sizes them in case of screen rotation or window size change.</span></span>

> [!WARNING]
> <span data-ttu-id="c8145-151">Fontos, hogy az azonos elnevezési tartsa `engagement_notification_content` és `engagement_announcement_content` a a `WebView` elemek.</span><span class="sxs-lookup"><span data-stu-id="c8145-151">It is important to keep the same naming `engagement_notification_content` and `engagement_announcement_content` for the `WebView` elements.</span></span> <span data-ttu-id="c8145-152">Reach azonosítja azokat a nevük számára.</span><span class="sxs-lookup"><span data-stu-id="c8145-152">Reach is identifying them by their name.</span></span> 
> 
> 

## <a name="handle-datapush-optional"></a><span data-ttu-id="c8145-153">Leíró datapush (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="c8145-153">Handle datapush (optional)</span></span>
<span data-ttu-id="c8145-154">Ha azt szeretné, hogy az alkalmazás fogadhat Reach adatleküldések, két esemény EngagementReach osztály végrehajtásához rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="c8145-154">If you want your application to be able to receive Reach data pushes, you have to implement two events of the EngagementReach class:</span></span>

<span data-ttu-id="c8145-155">A App() konstruktorban App.xaml.cs fájlban adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="c8145-155">In App.xaml.cs in the App() constructor add:</span></span>

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

<span data-ttu-id="c8145-156">Láthatja, hogy az egyes módszerek visszahívási olyan logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c8145-156">You can see that the callback of each method returns a boolean.</span></span> <span data-ttu-id="c8145-157">Bevonási egy visszajelzést küld a a háttér-után az adatleküldés terjesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="c8145-157">Engagement sends a feedback to its back-end after dispatching the data push.</span></span> <span data-ttu-id="c8145-158">A visszahívási hamis értéket ad vissza, ha a `exit` visszajelzés küldése lesz.</span><span class="sxs-lookup"><span data-stu-id="c8145-158">If the callback returns false, the `exit` feedback will be send.</span></span> <span data-ttu-id="c8145-159">Ellenkező esetben lesz `action`.</span><span class="sxs-lookup"><span data-stu-id="c8145-159">Otherwise, it will be `action`.</span></span> <span data-ttu-id="c8145-160">Ha nem visszahívási események, be van állítva a `drop` visszajelzést az eredmény engagement.</span><span class="sxs-lookup"><span data-stu-id="c8145-160">If no callback is set for the events, the `drop` feedback will be returned to Engagement.</span></span>

> [!WARNING]
> <span data-ttu-id="c8145-161">Bevonási nincs Többszörösök visszajelzése van, az adatokat fogadhat.</span><span class="sxs-lookup"><span data-stu-id="c8145-161">Engagement is not able to receive multiples feedbacks for a data push.</span></span> <span data-ttu-id="c8145-162">Ha azt tervezi, hogy több kezelők be egy eseményt, vegye figyelembe, hogy a visszajelzés utolsó felel meg egyik küldött.</span><span class="sxs-lookup"><span data-stu-id="c8145-162">If you plan to set several handlers on an event, be aware that the feedback will correspond to the last one sent.</span></span> <span data-ttu-id="c8145-163">Ebben az esetben ajánlott mindig adja meg ugyanazt az értéket ne használjon egyértelmű visszajelzést az előtér-a.</span><span class="sxs-lookup"><span data-stu-id="c8145-163">In this case, we recommend to always returns the same value to avoid having confusing feedback on the front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="c8145-164">(Választható) felhasználói felület testreszabása</span><span class="sxs-lookup"><span data-stu-id="c8145-164">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="c8145-165">Első lépés</span><span class="sxs-lookup"><span data-stu-id="c8145-165">First step</span></span>
<span data-ttu-id="c8145-166">Azt teszik lehetővé a reach felhasználói felület testreszabása.</span><span class="sxs-lookup"><span data-stu-id="c8145-166">We allow you to customize the reach UI.</span></span>

<span data-ttu-id="c8145-167">Ehhez létre kell hoznia egy alosztálya a `EngagementReachHandler` osztály.</span><span class="sxs-lookup"><span data-stu-id="c8145-167">To do so, you have to create a subclass of the `EngagementReachHandler` class.</span></span>

<span data-ttu-id="c8145-168">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="c8145-168">**Sample Code :**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

<span data-ttu-id="c8145-169">Állítsa a tartalmát a `EngagementReach.Instance.Handler` mező található az egyéni objektum a `App.xaml.cs` belül osztály a `App()` metódus.</span><span class="sxs-lookup"><span data-stu-id="c8145-169">Then, set the content of the `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within the `App()` method.</span></span>

<span data-ttu-id="c8145-170">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="c8145-170">**Sample Code :**</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> <span data-ttu-id="c8145-171">Alapértelmezés szerint az Engagement használja a saját végrehajtásának `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="c8145-171">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span>
> <span data-ttu-id="c8145-172">Nem kell létrehoznia a saját, és ha így tesz, nem kell minden metódus felülbírálására.</span><span class="sxs-lookup"><span data-stu-id="c8145-172">You don't have to create your own, and if you do so, you don't have to override every method.</span></span> <span data-ttu-id="c8145-173">Az alapértelmezett viselkedés, jelölje be a bevonási objektum.</span><span class="sxs-lookup"><span data-stu-id="c8145-173">The default behavior is to select the Engagement base object.</span></span>
> 
> 

### <a name="web-view"></a><span data-ttu-id="c8145-174">Webes nézet</span><span class="sxs-lookup"><span data-stu-id="c8145-174">Web View</span></span>
<span data-ttu-id="c8145-175">Alapértelmezés szerint Reach az értesítések és a lap megjelenítése fogja használni a beágyazott erőforrások a dll-fájl.</span><span class="sxs-lookup"><span data-stu-id="c8145-175">By default, Reach will use the embedded resources of the DLL to display the notifications and pages.</span></span>

<span data-ttu-id="c8145-176">Adjon meg egy teljes testreszabási lehetőségek a webes nézet csak használjuk.</span><span class="sxs-lookup"><span data-stu-id="c8145-176">To provide a full customization possibilities we only use web view.</span></span> <span data-ttu-id="c8145-177">Ha szeretné testre szabni elrendezések, bírálja felül közvetlenül az erőforrások fájlok `EngagementAnnouncement.html` és `EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="c8145-177">If you want to customize layouts, override directly the resources files `EngagementAnnouncement.html` and `EngagementNotification.html`.</span></span> <span data-ttu-id="c8145-178">Bevonási kell az összes kódot `<body></body>` működéséhez.</span><span class="sxs-lookup"><span data-stu-id="c8145-178">Engagement needs all code in `<body></body>` to run correctly.</span></span> <span data-ttu-id="c8145-179">Külső címkét adhat hozzá, de `engagement_webview_area`.</span><span class="sxs-lookup"><span data-stu-id="c8145-179">But you can add tag outer `engagement_webview_area`.</span></span>

<span data-ttu-id="c8145-180">Azonban dönthet úgy is használja a saját erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="c8145-180">However, you can decide to use your own resources.</span></span>

<span data-ttu-id="c8145-181">Ha szeretné felülbírálni az `EngagementReachHandler` módszereket a alosztály állapítható meg, Engagement is használható a elrendezések, de ügyeljen arra, hogy a beágyazott az engagement mechanizmus:</span><span class="sxs-lookup"><span data-stu-id="c8145-181">You can override `EngagementReachHandler` methods in your subclass to tell Engagement to use your layouts, but take care to embedded the engagement mechanism:</span></span>

<span data-ttu-id="c8145-182">**Mintakód:**</span><span class="sxs-lookup"><span data-stu-id="c8145-182">**Sample Code :**</span></span>

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


<span data-ttu-id="c8145-183">Alapértelmezés szerint AnnouncementHTML van `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span><span class="sxs-lookup"><span data-stu-id="c8145-183">By default, AnnouncementHTML is `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span></span> <span data-ttu-id="c8145-184">A HTML-fájl, amely a leküldéses üzenet (szöveges közlemény, webes anoucement és lekérdezési közlemény) tartalma tervezése képviseli.</span><span class="sxs-lookup"><span data-stu-id="c8145-184">It represents the html file that design the content of a push message (Text announcement, Web anoucement and Poll announcement).</span></span> <span data-ttu-id="c8145-185">AnnouncementName van `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="c8145-185">AnnouncementName is `engagement_announcement_content`.</span></span> <span data-ttu-id="c8145-186">A webes nézet tervéhez az xaml-oldal neve.</span><span class="sxs-lookup"><span data-stu-id="c8145-186">It is the name of the webview design in your xaml page.</span></span>

<span data-ttu-id="c8145-187">NotfificationHTML van `ms-appx-web:///Resources/EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="c8145-187">NotfificationHTML is `ms-appx-web:///Resources/EngagementNotification.html`.</span></span> <span data-ttu-id="c8145-188">Azt jelenti, hogy a HTML-fájl, amely a leküldéses üzenet értesítésének megtervezésére.</span><span class="sxs-lookup"><span data-stu-id="c8145-188">It represents the html file that design the notification of a push message.</span></span> <span data-ttu-id="c8145-189">NotfificationName van `engagement_notification_content`.</span><span class="sxs-lookup"><span data-stu-id="c8145-189">NotfificationName is `engagement_notification_content`.</span></span> <span data-ttu-id="c8145-190">A webes nézet tervéhez az xaml-oldal neve.</span><span class="sxs-lookup"><span data-stu-id="c8145-190">It is the name of the webview design in your xaml page.</span></span>

### <a name="customization"></a><span data-ttu-id="c8145-191">Testreszabás</span><span class="sxs-lookup"><span data-stu-id="c8145-191">Customization</span></span>
<span data-ttu-id="c8145-192">Értesítés és a webes nézet azt szeretné, hogy ha megőrizheti a bevonási objektum rendelkezik közlemény személyre is szabhatja.</span><span class="sxs-lookup"><span data-stu-id="c8145-192">You can customize notification and announcement web view has you want if you preserve Engagement object.</span></span> <span data-ttu-id="c8145-193">Ügyelni kell arra, hogy a webes nézet objektum leírt három alkalommal – az első alkalommal a xaml-kódban, még egyszer a .cs fájlban a "setwebview()" metódusban és harmadik idő a HTML-fájlban.</span><span class="sxs-lookup"><span data-stu-id="c8145-193">Take care that webview object is described three times - the first time in your xaml, second time in your .cs file in the "setwebview()" method, and third time in the html file.</span></span>

* <span data-ttu-id="c8145-194">Az XAML-kódot az aktuális grafikus elrendezés webes nézet összetevő ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c8145-194">In your xaml you describe the current graphical layout webview component.</span></span>
* <span data-ttu-id="c8145-195">A .cs fájlban definiálhat "setwebview()" beállította a két webes nézet (értesítés, bejelentés) dimenziója.</span><span class="sxs-lookup"><span data-stu-id="c8145-195">In your .cs file you can define "setwebview()" in which you set the dimension of the two webview (notification, announcement).</span></span> <span data-ttu-id="c8145-196">Akkor nagyon hatékony az alkalmazás átméretezésekor.</span><span class="sxs-lookup"><span data-stu-id="c8145-196">It is very effective when the application is resizing.</span></span>
* <span data-ttu-id="c8145-197">A bevonási html-fájlba azt írják le, a webes nézet tartalmat, tervezési és az elemek pozíciók egymás között.</span><span class="sxs-lookup"><span data-stu-id="c8145-197">In the Engagement html file we describe the webview content, design and the elements positions between each other.</span></span>

### <a name="launch-message"></a><span data-ttu-id="c8145-198">Indítsa el az üzenet</span><span class="sxs-lookup"><span data-stu-id="c8145-198">Launch message</span></span>
<span data-ttu-id="c8145-199">Amikor egy felhasználó egy Rendszerértesítő (egy bejelentési) kattint, az Engagement elindítja az alkalmazást, a leküldéses üzenetek a tartalom betöltése, és a megfelelő kampány lap megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c8145-199">When a user clicks on a system notification (a toast), Engagement launches the application, load the content of the push messages, and display the page for the corresponding campaign.</span></span>

<span data-ttu-id="c8145-200">A késleltetés van a indítsa el az alkalmazás és a lap (attól függően, hogy a hálózat sebességétől) megjelenítési között.</span><span class="sxs-lookup"><span data-stu-id="c8145-200">There is a delay between the launch of the application and the display of the page (depending on the speed of your network).</span></span>

<span data-ttu-id="c8145-201">Jelzi a felhasználóknak, hogy valami tölt, egy visual adatok, például egy folyamatjelző vagy egy folyamatjelző kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="c8145-201">To indicate to the user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="c8145-202">Bevonási nem tudja kezelni, hogy saját magát, de itt néhány kezelők meg.</span><span class="sxs-lookup"><span data-stu-id="c8145-202">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="c8145-203">A visszahívás megvalósításához, "Nyilvános App() megkeresése" App.xaml.cs fájlban adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="c8145-203">To implement the callback, in App.xaml.cs in "Public App(){}" add:</span></span>

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

<span data-ttu-id="c8145-204">A visszahívási állíthatja be a "Nyilvános App() {}" metódusában a `App.xaml.cs` fájlt, mielőtt lehetőleg a `EngagementReach.Instance.Init()` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="c8145-204">You can set the callback in your "Public App(){}" method of your `App.xaml.cs` file, preferably before the `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="c8145-205">A felhasználói felület szálából minden kezelő hívja.</span><span class="sxs-lookup"><span data-stu-id="c8145-205">Each handler is called by the UI Thread.</span></span> <span data-ttu-id="c8145-206">Nincs a MessageBox vagy a felhasználói felület kapcsolatos valami használatakor foglalkoznia.</span><span class="sxs-lookup"><span data-stu-id="c8145-206">You do not have to worry when using a MessageBox or something UI-related.</span></span>
> 
> 

## <span data-ttu-id="c8145-207"><a id="push-channel-sharing"></a>Leküldéses csatorna megosztása</span><span class="sxs-lookup"><span data-stu-id="c8145-207"><a id="push-channel-sharing"></a> Push channel sharing</span></span>
<span data-ttu-id="c8145-208">Ha használ a leküldéses értesítések más célra az alkalmazás akkor rendelkezik a leküldéses csatorna megosztása Engagement SDK-szolgáltatás használatára.</span><span class="sxs-lookup"><span data-stu-id="c8145-208">If you are using push notifications for another purpose in your application then you have to use the push channel sharing feature of the Engagement SDK.</span></span> <span data-ttu-id="c8145-209">Ez a kihagyott leküldéses elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="c8145-209">This is to avoid missed push.</span></span>

* <span data-ttu-id="c8145-210">Megadhatja a saját az Engagement Reach inicializálás leküldéses csatornát.</span><span class="sxs-lookup"><span data-stu-id="c8145-210">You can provide your own push channel to the Engagement Reach initialization.</span></span> <span data-ttu-id="c8145-211">Az SDK helyett egy új kérő használja.</span><span class="sxs-lookup"><span data-stu-id="c8145-211">The SDK will use it instead of requesting a new one.</span></span>

<span data-ttu-id="c8145-212">Frissítse az Engagement Reach inicializálása a leküldéses csatorna a `InitEngagement` metódust a `App.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="c8145-212">Update the Engagement Reach initialization with your push channel in the `InitEngagement` method from the `App.xaml.cs` file:</span></span>

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* <span data-ttu-id="c8145-213">Azt is megteheti Ha szeretné a leküldéses csatorna felhasználását követően a Reach inicializálás, akkor beállíthatja az egy visszahívási Engagement Reach egyszer lekérni a leküldéses csatorna létrejön az SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="c8145-213">Alternatively, if you just want to consume the push channel after the Reach initialization then you can set a callback on Engagement Reach to get the push channel once it is created by the SDK.</span></span>

<span data-ttu-id="c8145-214">Állítsa be a visszahívási bárhol **után** Reach inicializálása:</span><span class="sxs-lookup"><span data-stu-id="c8145-214">Set your callback at any place **after** the Reach initialization :</span></span>

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a><span data-ttu-id="c8145-215">Egyéni séma tipp</span><span class="sxs-lookup"><span data-stu-id="c8145-215">Custom scheme tip</span></span>
<span data-ttu-id="c8145-216">Azt adja meg az egyéni séma használja.</span><span class="sxs-lookup"><span data-stu-id="c8145-216">We provide custom scheme use.</span></span> <span data-ttu-id="c8145-217">A bevonási frontend alhálózatból az engagement alkalmazásban használható különböző típusú URI küldhet.</span><span class="sxs-lookup"><span data-stu-id="c8145-217">You can send different type of URI from engagement frontend to be used in your engagement application.</span></span> <span data-ttu-id="c8145-218">Alapértelmezett séma például `http, ftp, ...` vannak kezelése Windows, illetve egy ablak lesz kérdezzen rá, ha telepítve az eszközön alapértelmezés szerinti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c8145-218">Default scheme like `http, ftp, ...` are manage by Windows, a window will prompt if they are no default application installed on device.</span></span> <span data-ttu-id="c8145-219">Az alkalmazás a saját egyéni séma is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="c8145-219">You can also create your own custom scheme for your application.</span></span>

<span data-ttu-id="c8145-220">Az egyéni séma beállítása az alkalmazás legegyszerűbb megnyitásához a `Package.appxmanifest` nyissa meg a `Declarations` panel.</span><span class="sxs-lookup"><span data-stu-id="c8145-220">The simple way to set a custom scheme in your application is to open your `Package.appxmanifest` go in `Declarations` panel.</span></span> <span data-ttu-id="c8145-221">Válassza ki `Protocol` a rendelkezésre álló deklarációjában görgessen a mezőbe, majd vegye fel azt.</span><span class="sxs-lookup"><span data-stu-id="c8145-221">Select `Protocol` in the Available Declarations scroll box and add it.</span></span> <span data-ttu-id="c8145-222">Szerkessze a `Name` mező az új protokoll a kívánt nevet.</span><span class="sxs-lookup"><span data-stu-id="c8145-222">Edit the `Name` field with your new protocol desired name.</span></span>

<span data-ttu-id="c8145-223">Ez a protokoll használatához szerkesztésével a `App.xaml.cs` rendelkező a `OnActivated` metódust, és ne feledkezzen meg itt az engagement inicializálására is:</span><span class="sxs-lookup"><span data-stu-id="c8145-223">Now to use this protocol, edit your `App.xaml.cs` with the `OnActivated` method, and don't forget to initialize engagement here also:</span></span>

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action to do when app is launch

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

