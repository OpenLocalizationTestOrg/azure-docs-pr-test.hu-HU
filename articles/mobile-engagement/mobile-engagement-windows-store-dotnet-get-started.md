---
title: "aaaGet Windows univerzális alkalmazások Azure Mobile Engagement használatába"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az univerzális Windows-alkalmazásokhoz kapcsolódó elemzések és leküldéses értesítéseket."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48103867-7f64-4646-b019-42bd797d38e2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 8224a6d3789cfe4784bbc9472005f9eddb94a8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a><span data-ttu-id="a56b1-103">Ismerkedés az Azure Mobile Engagement univerzális Windows-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="a56b1-103">Get started with Azure Mobile Engagement for Windows Universal Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="a56b1-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók egy univerzális Windows-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a56b1-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users of a Windows Universal application.</span></span>
<span data-ttu-id="a56b1-105">Ez az oktatóanyag bemutatja, hogyan hello használó egyszerű küldési forgatókönyvet a Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a56b1-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="a56b1-106">Ennek során létrehoz egy üres univerzális Windows-alkalmazást, amely alapszintű alkalmazáshasználati adatokat gyűjt, és leküldéses értesítéseket fogad a Windows értesítési szolgáltatása (WNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="a56b1-106">You create a blank Windows Universal App that collects basic app usage data and receives push notifications using Windows Notification Service (WNS).</span></span>

> [!NOTE]
> <span data-ttu-id="a56b1-107">hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="a56b1-107">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="a56b1-108">További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="a56b1-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a56b1-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a56b1-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a><span data-ttu-id="a56b1-110">A Mobile Engagement beállítása az univerzális Windows-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a56b1-110">Set up Mobile Engagement for your Windows Universal app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="a56b1-111"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="a56b1-111"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="a56b1-112">Ez az oktatóanyag egy "alapszintű integrációt," hello minimális határozza meg a szükséges toocollect dátumát és leküldéses értesítés küldéséhez mutat.</span><span class="sxs-lookup"><span data-stu-id="a56b1-112">This tutorial presents a "basic integration," which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="a56b1-113">hello teljes integrációs dokumentáció itt található a hello [Mobile Engagement univerzális Windows SDK-integráció](mobile-engagement-windows-store-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a56b1-113">hello complete integration documentation can be found in hello [Mobile Engagement Windows Universal SDK integration](mobile-engagement-windows-store-sdk-overview.md).</span></span>

<span data-ttu-id="a56b1-114">Létrehozhat egy alapszintű alkalmazást a Visual Studio toodemonstrate hello integrációja.</span><span class="sxs-lookup"><span data-stu-id="a56b1-114">You create a basic app with Visual Studio toodemonstrate hello integration.</span></span>

### <a name="create-a-windows-universal-app-project"></a><span data-ttu-id="a56b1-115">Univerzális Windows-alkalmazás projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a56b1-115">Create a Windows Universal App project</span></span>
<span data-ttu-id="a56b1-116">hello következő lépések azt feltételezik hello Visual Studio 2015 használatát, ha hello lépések hasonlóak a Visual Studio korábbi verzióiban.</span><span class="sxs-lookup"><span data-stu-id="a56b1-116">hello following steps assume hello use of Visual Studio 2015 though hello steps are similar in earlier versions of Visual Studio.</span></span>

1. <span data-ttu-id="a56b1-117">Indítsa el a Visual Studio és a hello **Home** képernyőn válassza ki **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="a56b1-117">Start Visual Studio, and in hello **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="a56b1-118">Hello előugró ablakban válassza **Windows** -> **univerzális** -> **üres alkalmazás (univerzális Windows)**.</span><span class="sxs-lookup"><span data-stu-id="a56b1-118">In hello pop-up, select **Windows** -> **Universal** -> **Blank App (Universal Windows)**.</span></span> <span data-ttu-id="a56b1-119">Töltse ki a hello app **neve** és **megoldásnév**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a56b1-119">Fill in hello app **Name** and **Solution name**, and then click **OK**.</span></span>

    ![][1]

<span data-ttu-id="a56b1-120">Most létrehozott egy univerzális Windows-alkalmazás projektet, amelybe mellett integrálása hello Azure Mobile Engagement SDK-t.</span><span class="sxs-lookup"><span data-stu-id="a56b1-120">You have now created a Windows Universal App project into which you next integrate hello Azure Mobile Engagement SDK.</span></span>

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="a56b1-121">Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="a56b1-121">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="a56b1-122">Telepítse a hello [MicrosoftAzure.MobileEngagement] Nuget-csomagot a projektben.</span><span class="sxs-lookup"><span data-stu-id="a56b1-122">Install hello [MicrosoftAzure.MobileEngagement] Nuget package in your project.</span></span> <span data-ttu-id="a56b1-123">Windows és Windows Phone platformot céloz meg, ha szüksége toodo ezt mindkét projekt.</span><span class="sxs-lookup"><span data-stu-id="a56b1-123">If you are targeting both Windows and Windows Phone platforms, you need toodo this for both projects.</span></span> <span data-ttu-id="a56b1-124">A Windows 8.x és Windows Phone 8.1, hello azonos Nuget csomag helyek hello megfelelő platformspecifikus bináris fájlokat mindkét projektben.</span><span class="sxs-lookup"><span data-stu-id="a56b1-124">For Windows 8.x and Windows Phone 8.1, hello same Nuget package places hello correct platform-specific binaries in each project.</span></span>
2. <span data-ttu-id="a56b1-125">Nyissa meg **Package.appxmanifest** és győződjön meg arról, hogy a következő képességet hello hiba jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a56b1-125">Open **Package.appxmanifest** and make sure that hello following capability is added there:</span></span>

        Internet (Client)

    ![][2]
3. <span data-ttu-id="a56b1-126">Most másolja, amely korábban a Mobile Engagement-alkalmazáshoz kimásolt hello kapcsolati karakterláncot, és illessze be hello `Resources\EngagementConfiguration.xml` fájl közötti hello `<connectionString>` és `</connectionString>` címkék:</span><span class="sxs-lookup"><span data-stu-id="a56b1-126">Now copy hello connection string that you copied earlier for your Mobile Engagement App and paste it in hello `Resources\EngagementConfiguration.xml` file, between hello `<connectionString>` and `</connectionString>` tags:</span></span>

    ![][3]

    > [!TIP]
    > <span data-ttu-id="a56b1-127">Ha az alkalmazása a Windows és a Windows Phone platformot is célozza, hozzon létre két Mobile Engagement-alkalmazást – egyet-egyet mindegyik támogatott platformhoz.</span><span class="sxs-lookup"><span data-stu-id="a56b1-127">If your App targets both Windows and Windows Phone platforms, you should still create two Mobile Engagement Applications - one for each supported platform.</span></span> <span data-ttu-id="a56b1-128">Két alkalmazások biztosítja, hogy helyes szegmentálását hello célközönség hozhat létre, és az egyes platformokon megfelelően célzott értesítéseket küldhet.</span><span class="sxs-lookup"><span data-stu-id="a56b1-128">Having two apps ensures that you can create correct segmentation of hello audience and can send appropriately targeted notifications for each platform.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a56b1-129">NuGet nem automatikusan hello SDK-erőforrások másolása a Windows 10 UWP-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a56b1-129">NuGet does not automatically copy hello SDK resources in your Windows 10 UWP application.</span></span> <span data-ttu-id="a56b1-130">Hogy toodo, manuális lépések hello amely jelenik meg (readme.txt), ha hello Nuget-csomagja telepítve van.</span><span class="sxs-lookup"><span data-stu-id="a56b1-130">You have toodo it manually following hello steps which show up (readme.txt) when hello Nuget package is installed.</span></span>  

1. <span data-ttu-id="a56b1-131">A hello `App.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="a56b1-131">In hello `App.xaml.cs` file:</span></span>

    <span data-ttu-id="a56b1-132">a.</span><span class="sxs-lookup"><span data-stu-id="a56b1-132">a.</span></span> <span data-ttu-id="a56b1-133">Adja hozzá a hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="a56b1-133">Add hello `using` statement:</span></span>

            using Microsoft.Azure.Engagement;

    <span data-ttu-id="a56b1-134">b.</span><span class="sxs-lookup"><span data-stu-id="a56b1-134">b.</span></span> <span data-ttu-id="a56b1-135">Inicializálja az hello Engagement metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="a56b1-135">Add a method that initializes hello Engagement:</span></span>

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of hello code
           }

    <span data-ttu-id="a56b1-136">c.</span><span class="sxs-lookup"><span data-stu-id="a56b1-136">c.</span></span> <span data-ttu-id="a56b1-137">A hello SDK hello inicializálása **OnLaunched** módszert:</span><span class="sxs-lookup"><span data-stu-id="a56b1-137">Initialize hello SDK in hello **OnLaunched** method:</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

    <span data-ttu-id="a56b1-138">c.</span><span class="sxs-lookup"><span data-stu-id="a56b1-138">c.</span></span> <span data-ttu-id="a56b1-139">Helyezze be a következő hello hello **OnActivated** metódust, és adja hozzá a hello metódust, ha még nincs jelen:</span><span class="sxs-lookup"><span data-stu-id="a56b1-139">Insert hello following in hello **OnActivated** method and add hello method if it is not already present:</span></span>

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

## <span data-ttu-id="a56b1-140"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a56b1-140"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="a56b1-141">adatküldés, és győződjön meg arról, hogy hello felhasználók aktívak, el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello toostart.</span><span class="sxs-lookup"><span data-stu-id="a56b1-141">toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="a56b1-142">A hello **MainPage.xaml.cs**, adja hozzá a következő hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="a56b1-142">In hello **MainPage.xaml.cs**, add hello following `using` statement:</span></span>

    <span data-ttu-id="a56b1-143">using Microsoft.Azure.Engagement.Overlay;</span><span class="sxs-lookup"><span data-stu-id="a56b1-143">using Microsoft.Azure.Engagement.Overlay;</span></span>
2. <span data-ttu-id="a56b1-144">Módosítsa az alaposztály alaposztályát hello **MainPage** a **lap** túl**EngagementPageOverlay**:</span><span class="sxs-lookup"><span data-stu-id="a56b1-144">Change hello base class of **MainPage** from **Page** too**EngagementPageOverlay**:</span></span>

        class MainPage : EngagementPageOverlay
3. <span data-ttu-id="a56b1-145">A hello `MainPage.xaml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="a56b1-145">In hello `MainPage.xaml` file:</span></span>

    <span data-ttu-id="a56b1-146">a.</span><span class="sxs-lookup"><span data-stu-id="a56b1-146">a.</span></span> <span data-ttu-id="a56b1-147">Tooyour névtér-deklarációk hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="a56b1-147">Add tooyour namespaces declarations:</span></span>

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    <span data-ttu-id="a56b1-148">b.</span><span class="sxs-lookup"><span data-stu-id="a56b1-148">b.</span></span> <span data-ttu-id="a56b1-149">Cserélje le a hello **lap** hello XML-címkenév a **engagement: EngagementPageOverlay**</span><span class="sxs-lookup"><span data-stu-id="a56b1-149">Replace hello **Page** in hello XML tag name with **engagement:EngagementPageOverlay**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a56b1-150">Ha az oldala felülírja hello `OnNavigatedTo` módszer, lehet, hogy toocall `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="a56b1-150">If your page overrides hello `OnNavigatedTo` method, be sure toocall `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="a56b1-151">Ellenkező esetben hello tevékenység nem jelentett `EngagementPage` hívások `StartActivity` belül a `OnNavigatedTo` metódus).</span><span class="sxs-lookup"><span data-stu-id="a56b1-151">Otherwise, hello activity is not reported `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span> <span data-ttu-id="a56b1-152">Ez különösen fontos a Windows Phone-projektben ahol hello alapértelmezett sablon rendelkezik egy `OnNavigatedTo` metódust.</span><span class="sxs-lookup"><span data-stu-id="a56b1-152">This is especially important in a Windows Phone project where hello default template has an `OnNavigatedTo` method.</span></span>
>
> <span data-ttu-id="a56b1-153">A **univerzális Windows 10-alkalmazások**, hello módszer ajánlott a hello "javasolt módszer: a lap osztályok túlterhelés" szakasza [speciális jelentéskészítés hello Windows univerzális alkalmazások Engagement SDK-t a](mobile-engagement-windows-store-advanced-reporting.md) , ahelyett, hogy hello egyet fent említett.</span><span class="sxs-lookup"><span data-stu-id="a56b1-153">For **Windows 10 Universal apps**, use hello method recommended in hello "Recommended method: overload your Page classes" section of [Advanced Reporting with hello Windows Universal Apps Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md), rather than hello one mentioned above.</span></span>

## <span data-ttu-id="a56b1-154"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="a56b1-154"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="a56b1-155"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a56b1-155"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="a56b1-156">A Mobile Engagement lehetővé teszi a toointeract, és a felhasználók elérését a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="a56b1-156">Mobile Engagement allows you toointeract and reach your users with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="a56b1-157">Ez a modul REACH neve hello a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="a56b1-157">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="a56b1-158">hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.</span><span class="sxs-lookup"><span data-stu-id="a56b1-158">hello following sections set up your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-wns-push-notifications"></a><span data-ttu-id="a56b1-159">Az alkalmazás tooreceive WNS leküldéses értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a56b1-159">Enable your app tooreceive WNS Push Notifications</span></span>
1. <span data-ttu-id="a56b1-160">A hello `Package.appxmanifest` hello-fájlban **alkalmazás** lap **értesítések**, beállíthatja **képes bejelentési:** túl**Igen**</span><span class="sxs-lookup"><span data-stu-id="a56b1-160">In hello `Package.appxmanifest` file, in hello **Application** tab, under **Notifications**, set **Toast capable:** too**Yes**</span></span>

    ![][5]

### <a name="initialize-hello-reach-sdk"></a><span data-ttu-id="a56b1-161">Hello REACH SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="a56b1-161">Initialize hello REACH SDK</span></span>
<span data-ttu-id="a56b1-162">A `App.xaml.cs`, hívja **Initengagement** a hello **Engagementreach.instance.Init(e)** függvény hello ügynök inicializálása után:</span><span class="sxs-lookup"><span data-stu-id="a56b1-162">In `App.xaml.cs`, call **EngagementReach.Instance.Init(e);** in hello **InitEngagement** function right after hello agent initialization:</span></span>

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

<span data-ttu-id="a56b1-163">Most készen áll a toosend egy bejelentési.</span><span class="sxs-lookup"><span data-stu-id="a56b1-163">You're ready toosend a toast.</span></span> <span data-ttu-id="a56b1-164">A következő lépésben ellenőrizni fogjuk, hogy ezt az alapszintű integrációt megfelelően végezte-e el.</span><span class="sxs-lookup"><span data-stu-id="a56b1-164">Next we verify that you have correctly carried out this basic integration.</span></span>

### <a name="grant-access-toomobile-engagement-toosend-notifications"></a><span data-ttu-id="a56b1-165">Adjon hozzáférést tooMobile Engagement toosend értesítések</span><span class="sxs-lookup"><span data-stu-id="a56b1-165">Grant access tooMobile Engagement toosend notifications</span></span>
1. <span data-ttu-id="a56b1-166">Nyissa meg a [Windows fejlesztői központot] a webböngészőjében, és jelentkezzen be, vagy ha szükséges, hozzon létre egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="a56b1-166">Open [Windows Store Dev Center] in your web browser, login, and create an account if necessary.</span></span>
2. <span data-ttu-id="a56b1-167">Kattintson a **irányítópult** : hello jobb felső sarokban, és kattintson a **hozzon létre egy új alkalmazást** hello bal oldali panelmenüben.</span><span class="sxs-lookup"><span data-stu-id="a56b1-167">Click **Dashboard** at hello top right corner and then click **Create a new app** from hello left panel menu.</span></span>

    ![][9]
3. <span data-ttu-id="a56b1-168">Hozza létre az alkalmazást a neve lefoglalásával.</span><span class="sxs-lookup"><span data-stu-id="a56b1-168">Create your app by reserving its name.</span></span>

    ![][10]
4. <span data-ttu-id="a56b1-169">Hello alkalmazás létrehozása után, nyissa meg túl**szolgáltatások -> leküldéses értesítések** hello bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="a56b1-169">Once hello app has been created, navigate too**Services -> Push notifications** from hello left menu.</span></span>

    ![][11]
5. <span data-ttu-id="a56b1-170">Hello a leküldéses értesítések szakaszban, kattintson a hello **Live Services webhely** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a56b1-170">In hello Push notifications section, click hello **Live Services site** link.</span></span>

    ![][12]
6. <span data-ttu-id="a56b1-171">Navigálás toohello leküldési hitelesítő adatok szakasz.</span><span class="sxs-lookup"><span data-stu-id="a56b1-171">You navigate toohello Push credentials section.</span></span> <span data-ttu-id="a56b1-172">Győződjön meg arról, hogy a hello **Alkalmazásbeállítások** szakaszt, és másolja a **CSOMAGAZONOSÍTÓT** és **ügyfélkulcs**</span><span class="sxs-lookup"><span data-stu-id="a56b1-172">Make sure you are in hello **App Settings** section and then copy your **Package SID** and **Client secret**</span></span>

    ![][13]
7. <span data-ttu-id="a56b1-173">Keresse meg a toohello **beállítások** a Mobile Engagement portál hello kattintson **natív leküldés** hello bal oldali szakasz.</span><span class="sxs-lookup"><span data-stu-id="a56b1-173">Navigate toohello **Settings** of your Mobile Engagement portal, and click hello **Native Push** section on hello left.</span></span> <span data-ttu-id="a56b1-174">Kattintson a hello **szerkesztése** gomb tooenter a **csomag biztonsági azonosítóját (SID)** és a **titkos kulcs** látható módon:</span><span class="sxs-lookup"><span data-stu-id="a56b1-174">Then, click hello **Edit** button tooenter your **Package security identifier (SID)** and your **Secret Key** as shown:</span></span>

    ![][6]
8. <span data-ttu-id="a56b1-175">Végül győződjön meg arról, hogy ezzel az alkalmazással létrehozott hello App Store-ból a Visual Studio alkalmazás társítva.</span><span class="sxs-lookup"><span data-stu-id="a56b1-175">Finally make sure that you have associated your Visual Studio app with this created app in hello App store.</span></span> <span data-ttu-id="a56b1-176">A Visual Studióban kattintson a következőkre: **Associate app with the store** (Alkalmazás társítása az Áruházzal).</span><span class="sxs-lookup"><span data-stu-id="a56b1-176">Click **Associate App with Store** in Visual Studio.</span></span>

    ![][7]

## <span data-ttu-id="a56b1-177"><a id="send"></a>Egy értesítési tooyour app küldése</span><span class="sxs-lookup"><span data-stu-id="a56b1-177"><a id="send"></a>Send a notification tooyour app</span></span>
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

<span data-ttu-id="a56b1-178">Hello webalkalmazás működik, ha egy alkalmazásbeli értesítés megjelenik.</span><span class="sxs-lookup"><span data-stu-id="a56b1-178">If hello app is running, you see an in-app notification.</span></span> <span data-ttu-id="a56b1-179">Ellenkező esetben ha hello app le van zárva, megjelenik egy bejelentési értesítést.</span><span class="sxs-lookup"><span data-stu-id="a56b1-179">otherwise if hello app is closed, you see a toast notification.</span></span>
<span data-ttu-id="a56b1-180">Ha egy alkalmazásbeli értesítés, de a bejelentési értesítés nem látja, és hello alkalmazást a Visual Studio hibakeresési módban futtatja, majd próbálja meg **életciklus-események -> Suspend** a hello eszköztár tooensure hello alkalmazást fel van függesztve.</span><span class="sxs-lookup"><span data-stu-id="a56b1-180">If you see an in-app notification but not a toast notification, and you are running hello app in debug mode in Visual Studio, then try **Lifecycle events -> Suspend** in hello toolbar tooensure that hello app is suspended.</span></span> <span data-ttu-id="a56b1-181">A Visual Studio hello alkalmazás hibakeresése során hello kezdőképernyő gombra kattintott, majd azt nem mindig lesz felfüggesztve, és alkalmazáson belüli hello értesítési akkor látható, amíg azt nem jelenik a bejelentési értesítés.</span><span class="sxs-lookup"><span data-stu-id="a56b1-181">If you clicked hello Home button while debugging hello application in Visual Studio, then it doesn't always get suspended and while you see hello notification as in-app, it doesn't show up as a toast notification.</span></span>  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[Windows fejlesztői központot]: https://dev.windows.com
[Windows Universal Apps - Overlay integration]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png
