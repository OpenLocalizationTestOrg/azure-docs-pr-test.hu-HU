---
title: "aaaGet lépések az Azure Mobile Engagement a Windows Phone Silverlight-alkalmazásokhoz"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement Windows Phone Silverlight-alkalmazásokhoz kapcsolódó elemzések és leküldéses értesítések."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: aa34692f-87f7-47c6-a20c-a1972750bc25
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b39a838ab03217b2dc845cbf59d7bf8b094dac1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a><span data-ttu-id="7976c-103">Ismerkedés az Azure Mobile Engagement Windows Phone Silverlight-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="7976c-103">Get started with Azure Mobile Engagement for Windows Phone Silverlight apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="7976c-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók Windows Phone Silverlight-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7976c-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users of a Windows Phone Silverlight application.</span></span>
<span data-ttu-id="7976c-105">Ez az oktatóanyag bemutatja, hogyan hello használó egyszerű küldési forgatókönyvet a Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="7976c-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="7976c-106">Az oktatóanyagban létrehoz egy üres Windows Phone Silverlight-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad a Microsoft leküldéses értesítéseket kezelő szolgáltatása (MPNS) használatával.</span><span class="sxs-lookup"><span data-stu-id="7976c-106">In it, you create a blank Windows Phone Silverlight app that collects basic data and receives push notifications using Microsoft Push Notification Service (MPNS).</span></span>

> [!NOTE]
> <span data-ttu-id="7976c-107">hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="7976c-107">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="7976c-108">További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="7976c-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

> [!NOTE]
> <span data-ttu-id="7976c-109">A Windows Phone 8.1-es és korábbi verziójú projektek nem támogatottak a Visual Studio 2017-ben.</span><span class="sxs-lookup"><span data-stu-id="7976c-109">Windows Phone 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="7976c-110">További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="7976c-110">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

> [!NOTE]
> <span data-ttu-id="7976c-111">Ha a Windows Phone 8.1 (nem Silverlight) céloz meg, tekintse meg a toohello [univerzális Windows-oktatóanyag](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7976c-111">If you are targeting Windows Phone 8.1 (non-Silverlight), refer toohello [Windows Universal tutorial](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>
> 
> 

<span data-ttu-id="7976c-112">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="7976c-112">This tutorial requires hello following:</span></span>

* <span data-ttu-id="7976c-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7976c-113">Visual Studio 2013</span></span>
* <span data-ttu-id="7976c-114">[MicrosoftAzure.MobileEngagement] NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="7976c-114">[MicrosoftAzure.MobileEngagement] Nuget package</span></span>

> [!NOTE]
> <span data-ttu-id="7976c-115">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="7976c-115">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="7976c-116">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="7976c-116">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="7976c-117">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).</span><span class="sxs-lookup"><span data-stu-id="7976c-117">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).</span></span>
> 
> 

## <span data-ttu-id="7976c-118"><a id="setup-azme"></a>A Mobile Engagement beállítása a Windows Phone-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="7976c-118"><a id="setup-azme"></a>Setup Mobile Engagement for your Windows Phone app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="7976c-119"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="7976c-119"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="7976c-120">Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="7976c-120">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="7976c-121">hello teljes integrációs dokumentáció itt található a hello [Mobile Engagement Windows Phone SDK-integráció](mobile-engagement-windows-phone-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="7976c-121">hello complete integration documentation can be found in hello [Mobile Engagement Windows Phone SDK integration](mobile-engagement-windows-phone-sdk-overview.md)</span></span>

<span data-ttu-id="7976c-122">Létre fogunk hozni egy alapszintű alkalmazást a Visual Studio toodemonstrate hello integrációja.</span><span class="sxs-lookup"><span data-stu-id="7976c-122">We will create a basic app with Visual Studio toodemonstrate hello integration.</span></span>

### <a name="create-a-new-windows-phone-silverlight-project"></a><span data-ttu-id="7976c-123">Új Windows Phone Silverlight-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="7976c-123">Create a new Windows Phone Silverlight project</span></span>
<span data-ttu-id="7976c-124">hello következő lépések azt feltételezik hello Visual Studio 2015 használatát, ha hello lépések hasonlóak a Visual Studio korábbi verzióiban.</span><span class="sxs-lookup"><span data-stu-id="7976c-124">hello following steps assume hello use of Visual Studio 2015 though hello steps are similar in earlier versions of Visual Studio.</span></span> 

1. <span data-ttu-id="7976c-125">Indítsa el a Visual Studio és a hello **Home** képernyőn válassza ki **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="7976c-125">Start Visual Studio, and in hello **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="7976c-126">Hello előugró ablakban válassza **Windows 8** -> **Windows Phone** -> **üres alkalmazás (Windows Phone Silverlight)**.</span><span class="sxs-lookup"><span data-stu-id="7976c-126">In hello pop-up, select **Windows 8** -> **Windows Phone** -> **Blank App (Windows Phone Silverlight)**.</span></span> <span data-ttu-id="7976c-127">Töltse ki a hello app **neve** és **megoldásnév**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="7976c-127">Fill in hello app **Name** and **Solution name**, and then click **OK**.</span></span>
   
    ![][1]
3. <span data-ttu-id="7976c-128">Dönthet úgy tootarget vagy **Windows Phone 8.0** vagy **Windows Phone 8.1**.</span><span class="sxs-lookup"><span data-stu-id="7976c-128">You can choose tootarget either **Windows Phone 8.0** or **Windows Phone 8.1**.</span></span>

<span data-ttu-id="7976c-129">Most létrehozott egy új Windows Phone Silverlight-alkalmazást, amelybe integrálni fogjuk a hello Azure Mobile Engagement SDK-t.</span><span class="sxs-lookup"><span data-stu-id="7976c-129">You have now created a new Windows Phone Silverlight app into which we will integrate hello Azure Mobile Engagement SDK.</span></span>

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a><span data-ttu-id="7976c-130">Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="7976c-130">Connect your app toohello Mobile Engagement backend</span></span>
1. <span data-ttu-id="7976c-131">Telepítse a hello [MicrosoftAzure.MobileEngagement] nuget-csomagot a projektben.</span><span class="sxs-lookup"><span data-stu-id="7976c-131">Install hello [MicrosoftAzure.MobileEngagement] nuget package in your project.</span></span>
2. <span data-ttu-id="7976c-132">Nyissa meg `WMAppManifest.xml` (hello Properties mappában), és győződjön meg arról, hogy hello következő deklarált (hozzá, ha azok nem) a hello `<Capabilities />` címke:</span><span class="sxs-lookup"><span data-stu-id="7976c-132">Open `WMAppManifest.xml` (under hello Properties folder) and make sure hello following are declared (add them if they are not) in hello `<Capabilities />` tag:</span></span>
   
        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />
   
    ![][2]
3. <span data-ttu-id="7976c-133">Most illessze be a hello korábban a Mobile Engagement-alkalmazáshoz kimásolt kapcsolati karakterláncot, és illessze be hello `Resources\EngagementConfiguration.xml` fájl közötti hello `<connectionString>` és `</connectionString>` címkék:</span><span class="sxs-lookup"><span data-stu-id="7976c-133">Now paste hello connection string that you copied earlier for your Mobile Engagement app and paste it in hello `Resources\EngagementConfiguration.xml` file, between hello `<connectionString>` and `</connectionString>` tags:</span></span>
   
    ![][3]
4. <span data-ttu-id="7976c-134">A hello `App.xaml.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="7976c-134">In hello `App.xaml.cs` file:</span></span>
   
    <span data-ttu-id="7976c-135">a.</span><span class="sxs-lookup"><span data-stu-id="7976c-135">a.</span></span> <span data-ttu-id="7976c-136">Adja hozzá a hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="7976c-136">Add hello `using` statement:</span></span>
   
            using Microsoft.Azure.Engagement;
   
    <span data-ttu-id="7976c-137">b.</span><span class="sxs-lookup"><span data-stu-id="7976c-137">b.</span></span> <span data-ttu-id="7976c-138">A hello SDK hello inicializálása `Application_Launching` módszert:</span><span class="sxs-lookup"><span data-stu-id="7976c-138">Initialize hello SDK in hello `Application_Launching` method:</span></span>
   
            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }
   
    <span data-ttu-id="7976c-139">c.</span><span class="sxs-lookup"><span data-stu-id="7976c-139">c.</span></span> <span data-ttu-id="7976c-140">Helyezze be a következő hello hello `Application_Activated`:</span><span class="sxs-lookup"><span data-stu-id="7976c-140">Insert hello following in hello `Application_Activated`:</span></span>
   
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

## <span data-ttu-id="7976c-141"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7976c-141"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="7976c-142">Rendelés toostart adatküldés és annak biztosítására, hogy hello felhasználók aktív, a el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello.</span><span class="sxs-lookup"><span data-stu-id="7976c-142">In order toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="7976c-143">A hello MainPage.xaml.cs, adja hozzá a hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="7976c-143">In hello MainPage.xaml.cs, add hello `using` statement:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="7976c-144">Cserélje le az alaposztály alaposztályát hello **MainPage**, amely előtt van **PhoneApplicationPage**, a **EngagementPage**.</span><span class="sxs-lookup"><span data-stu-id="7976c-144">Replace hello base class of **MainPage**, which is before **PhoneApplicationPage**, with **EngagementPage**.</span></span>
   
        class MainPage : EngagementPage 
3. <span data-ttu-id="7976c-145">A `MainPage.xml` fájlban:</span><span class="sxs-lookup"><span data-stu-id="7976c-145">In your `MainPage.xml` file:</span></span>
   
    <span data-ttu-id="7976c-146">a.</span><span class="sxs-lookup"><span data-stu-id="7976c-146">a.</span></span> <span data-ttu-id="7976c-147">Tooyour névtér-deklarációk hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="7976c-147">Add tooyour namespaces declarations:</span></span>
   
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
   
    <span data-ttu-id="7976c-148">b.</span><span class="sxs-lookup"><span data-stu-id="7976c-148">b.</span></span> <span data-ttu-id="7976c-149">Cserélje le `phone:PhoneApplicationPage` hello XML-címkenév a `engagement:EngagementPage`.</span><span class="sxs-lookup"><span data-stu-id="7976c-149">Replace `phone:PhoneApplicationPage` in hello XML tag name with `engagement:EngagementPage`.</span></span>

## <span data-ttu-id="7976c-150"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="7976c-150"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="7976c-151"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7976c-151"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="7976c-152">A Mobile Engagement lehetővé teszi a toointeract, és a felhasználókat leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel hello kampányok elérni.</span><span class="sxs-lookup"><span data-stu-id="7976c-152">Mobile Engagement allows you toointeract and reach your users with Push Notifications and in-app Messaging in hello context of campaigns.</span></span> <span data-ttu-id="7976c-153">Ez a modul REACH neve hello a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="7976c-153">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="7976c-154">hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.</span><span class="sxs-lookup"><span data-stu-id="7976c-154">hello following sections set up your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-mpns-push-notifications"></a><span data-ttu-id="7976c-155">Az alkalmazás tooreceive MPNS leküldéses értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7976c-155">Enable your app tooreceive MPNS Push Notifications</span></span>
<span data-ttu-id="7976c-156">Adja hozzá az új képességek tooyour `WMAppManifest.xml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="7976c-156">Add new Capabilities tooyour `WMAppManifest.xml` file:</span></span>

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

### <a name="initialize-hello-reach-sdk"></a><span data-ttu-id="7976c-157">Hello REACH SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="7976c-157">Initialize hello REACH SDK</span></span>
1. <span data-ttu-id="7976c-158">A `App.xaml.cs`, hívja `EngagementReach.Instance.Init();` a hello **Application_Launching** függvény hello ügynök inicializálása után:</span><span class="sxs-lookup"><span data-stu-id="7976c-158">In `App.xaml.cs`, call `EngagementReach.Instance.Init();` in hello **Application_Launching** function, right after hello agent initialization:</span></span>
   
        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }
2. <span data-ttu-id="7976c-159">A `App.xaml.cs`, hívja `EngagementReach.Instance.OnActivated(e);` a hello **Application_Activated** függvény hello ügynök inicializálása után:</span><span class="sxs-lookup"><span data-stu-id="7976c-159">In `App.xaml.cs`, call `EngagementReach.Instance.OnActivated(e);` in hello **Application_Activated** function, right after hello agent initialization:</span></span>
   
        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

<span data-ttu-id="7976c-160">Készen is van.</span><span class="sxs-lookup"><span data-stu-id="7976c-160">You're all set.</span></span> <span data-ttu-id="7976c-161">Most ellenőrizzük, hogy ezt az alapszintű integrációt megfelelően végezte-e el.</span><span class="sxs-lookup"><span data-stu-id="7976c-161">Now we will verify that you have correctly cried out this basic integration.</span></span>

## <span data-ttu-id="7976c-162"><a id="send"></a>Egy értesítési tooyour app küldése</span><span class="sxs-lookup"><span data-stu-id="7976c-162"><a id="send"></a>Send a notification tooyour app</span></span>
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

<span data-ttu-id="7976c-163">Ekkor megjelenik egy értesítés az eszközön, amely megjelenik az alkalmazáson belüli értesítések, ha hello alkalmazás meg nyitva egy bejelentési értesítést hello hasonló, ellenkező esetben:</span><span class="sxs-lookup"><span data-stu-id="7976c-163">You should now see a notification on your device which will show up as an in-app notification if hello app is open otherwise as a toast notification like hello following:</span></span> 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
