---
title: "aaaGet elindítva az Azure Mobile Engagement Unity Android üzemelő"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések Unity-alkalmazásokhoz tooiOS eszközök telepítése."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: d5f0ef79-be00-4cec-97a5-a0b2fdaa380e
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c4d34691daeb7544b11c2d6895b2474af0f902b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a><span data-ttu-id="b4c7e-103">Ismerkedés az Azure Mobile Engagement Unity Android üzemelő példánnyal való használatával</span><span class="sxs-lookup"><span data-stu-id="b4c7e-103">Get Started with Azure Mobile Engagement for Unity Android deployment</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="b4c7e-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és toosend a leküldéses értesítések toosegmented felhasználók Unity-alkalmazás tooan Android-eszköz telepítése során.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of a Unity application when deploying tooan Android device.</span></span>
<span data-ttu-id="b4c7e-105">A oktatóanyag használ hello Unity klasszikus Roll golyó oktatóanyag, hello kiindulási pontjaként.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-105">This tutorial uses hello classic Unity Roll a Ball tutorial as hello starting point.</span></span> <span data-ttu-id="b4c7e-106">Ezen hello lépéseket kell követnie [oktatóanyag](mobile-engagement-unity-roll-a-ball.md) hello a Mobile Engagement-integrációs oktatóanyaggal az alábbi hello oktatóanyag folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-106">You should follow hello steps in this [tutorial](mobile-engagement-unity-roll-a-ball.md) before proceeding with hello Mobile Engagement integration we showcase in hello tutorial below.</span></span> 

<span data-ttu-id="b4c7e-107">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="b4c7e-107">This tutorial requires hello following:</span></span>

* [<span data-ttu-id="b4c7e-108">Unity Editor</span><span class="sxs-lookup"><span data-stu-id="b4c7e-108">Unity Editor</span></span>](http://unity3d.com/get-unity)
* [<span data-ttu-id="b4c7e-109">Mobile Engagement Unity SDK</span><span class="sxs-lookup"><span data-stu-id="b4c7e-109">Mobile Engagement Unity SDK</span></span>](https://aka.ms/azmeunitysdk)
* <span data-ttu-id="b4c7e-110">Google Android SDK</span><span class="sxs-lookup"><span data-stu-id="b4c7e-110">Google Android SDK</span></span>

> [!NOTE]
> <span data-ttu-id="b4c7e-111">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-111">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="b4c7e-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="b4c7e-113">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="b4c7e-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).</span></span>
> 
> 

## <span data-ttu-id="b4c7e-114"><a id="setup-azme"></a>A Mobile Engagement beállítása az Android-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="b4c7e-114"><a id="setup-azme"></a>Setup Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="b4c7e-115"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="b4c7e-115"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
### <a name="import-hello-unity-package"></a><span data-ttu-id="b4c7e-116">Hello Unity-csomag importálása</span><span class="sxs-lookup"><span data-stu-id="b4c7e-116">Import hello Unity package</span></span>
1. <span data-ttu-id="b4c7e-117">Töltse le a hello [Mobile Engagement Unity-csomagot](https://aka.ms/azmeunitysdk) , és mentse tooyour helyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-117">Download hello [Mobile Engagement Unity package](https://aka.ms/azmeunitysdk) and save it tooyour local machine.</span></span> 
2. <span data-ttu-id="b4c7e-118">Nyissa meg túl**eszközök -> csomag importálása -> egyéni csomag** és a fenti lépés hello letöltött válassza hello csomagot.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-118">Go too**Assets -> Import Package -> Custom Package** and select hello package you downloaded in hello above step.</span></span> 
   
    ![][70] 
3. <span data-ttu-id="b4c7e-119">Ügyeljen arra, hogy minden fájl ki legyen választva, és kattintson az **Import** (Importálás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-119">Make sure all files are selected and click **Import** button.</span></span> 
   
    ![][71] 
4. <span data-ttu-id="b4c7e-120">Ha sikeres importálás, látni fogja hello importált SDK-fájlok a projektben.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-120">Once Import is successful, you will see hello imported SDK files in your project.</span></span>  
   
    ![][72] 

### <a name="update-hello-engagementconfiguration"></a><span data-ttu-id="b4c7e-121">Hello EngagementConfiguration frissítése</span><span class="sxs-lookup"><span data-stu-id="b4c7e-121">Update hello EngagementConfiguration</span></span>
1. <span data-ttu-id="b4c7e-122">Nyissa meg hello **EngagementConfiguration** hello SDK mappából, és frissítse hello parancsfájlt **ANDROID\_kapcsolat\_karakterlánc** kapott hello kapcsolati karakterlánccal korábban a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-122">Open up hello **EngagementConfiguration** script file from hello SDK folder and update hello **ANDROID\_CONNECTION\_STRING** with hello connection string you obtained earlier from hello Azure portal.</span></span>  
   
    ![][73]
2. <span data-ttu-id="b4c7e-123">Hello fájl mentése</span><span class="sxs-lookup"><span data-stu-id="b4c7e-123">Save hello file</span></span> 
3. <span data-ttu-id="b4c7e-124">Futtassa a következőt: **File -> Engagement -> Generate Android Manifest** (Fájl -> Engagement -> Android-jegyzék létrehozása).</span><span class="sxs-lookup"><span data-stu-id="b4c7e-124">Execute **File -> Engagement -> Generate Android Manifest**.</span></span> <span data-ttu-id="b4c7e-125">Ez a hello Mobile Engagement SDK által hozzáadott hello beépülő modul, és azt automatikusan frissíti a projektbeállításokat.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-125">This is hello plugin added by hello Mobile Engagement SDK and clicking on it will automatically update your project settings.</span></span> 
   
    ![][74]

> [!IMPORTANT]
> <span data-ttu-id="b4c7e-126">Győződjön meg arról, hogy tooexecute Ez minden egyes hello frissítése **EngagementConfiguration** fájlból, egyébként pedig a módosítások nem tükröződnek hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-126">Make sure tooexecute this every time you update hello **EngagementConfiguration** file otherwise your changes will not be reflected in hello app.</span></span> 
> 
> 

### <a name="configure-hello-app-for-basic-tracking"></a><span data-ttu-id="b4c7e-127">Alapszintű nyomkövetéshez hello alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4c7e-127">Configure hello app for basic tracking</span></span>
1. <span data-ttu-id="b4c7e-128">Nyissa meg hello **PlayerController** toohello Player objektum szerkesztésre csatolt parancsprogram.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-128">Open up hello **PlayerController** script attached toohello Player object for editing.</span></span> 
2. <span data-ttu-id="b4c7e-129">Adja hozzá hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="b4c7e-129">Add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Unity;
3. <span data-ttu-id="b4c7e-130">Adja hozzá a következő toohello hello `Start()` módszer</span><span class="sxs-lookup"><span data-stu-id="b4c7e-130">Add hello following toohello `Start()` method</span></span>
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a><span data-ttu-id="b4c7e-131">Regisztrálhat és futtathat hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b4c7e-131">Deploy and run hello app</span></span>
<span data-ttu-id="b4c7e-132">Győződjön meg arról, hogy rendelkezik-e telepítve a számítógépre, mielőtt megpróbálná toodeploy a Unity-alkalmazás tooyour eszköz Android SDK-t.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-132">Make sure that you have Android SDK installed on your machine before attempting toodeploy this Unity app tooyour device.</span></span> 

1. <span data-ttu-id="b4c7e-133">Csatlakoztassa az Android-eszköz tooyour gépet.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-133">Connect an Android device tooyour machine.</span></span> 
2. <span data-ttu-id="b4c7e-134">Nyissa meg a **File -> Build Settings** (Fájl -> Létrehozási beállítások) menüpontot.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-134">Open up **File -> Build Settings**</span></span> 
   
    ![][40]
3. <span data-ttu-id="b4c7e-135">Válassza az **Android** lehetőséget, majd kattintson a **Switch Platform** (Platformváltás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-135">Select **Android** and then click on **Switch Platform**</span></span>
   
    ![][51]
   
    ![][52]
4. <span data-ttu-id="b4c7e-136">Kattintson a **Player settings** (Lejátszó beállításai) lehetőségre, és adjon meg egy érvényes értéket a Bundle Identifier (Csomagazonosító) számára.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-136">Click on **Player settings** and provide a valid Bundle Identifier.</span></span> 
   
    ![][53]
5. <span data-ttu-id="b4c7e-137">Végül kattintson a **Build And Run** (Létrehozás és futtatás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-137">Finally click on **Build And Run**</span></span>
   
    ![][54]
6. <span data-ttu-id="b4c7e-138">Előfordulhat, hogy ismételt tooprovide mappa neve toostore hello Android csomagot.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-138">You may be asked tooprovide a folder name toostore hello Android package.</span></span> 
7. <span data-ttu-id="b4c7e-139">Ha minden megfelelően konfigurálva, akkor hello csomag lesz csatlakoztatva telepített tooyour eszközt, és megjelenik a Unity-játék a telefonon!</span><span class="sxs-lookup"><span data-stu-id="b4c7e-139">If everything goes fine, then hello package will be deployed tooyour connected device and you should see your Unity game on your phone!</span></span> 

## <span data-ttu-id="b4c7e-140"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="b4c7e-140"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="b4c7e-141"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b4c7e-141"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### <a name="update-hello-engagementconfiguration"></a><span data-ttu-id="b4c7e-142">Hello EngagementConfiguration frissítése</span><span class="sxs-lookup"><span data-stu-id="b4c7e-142">Update hello EngagementConfiguration</span></span>
1. <span data-ttu-id="b4c7e-143">Hello nyissa meg **EngagementConfiguration** hello SDK mappából, és frissítse hello parancsfájlt **ANDROID\_GOOGLE\_szám** a hello **Google-projekt Szám** hello Google Cloud Developer portálról korábban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-143">Open up hello **EngagementConfiguration** script file from hello SDK folder and update hello **ANDROID\_GOOGLE\_NUMBER** with hello **Google Project Number** you obtained earlier from hello Google Cloud Developer portal.</span></span> <span data-ttu-id="b4c7e-144">Ez egy értéket, ezért győződjön meg arról, hogy tooenclose legyen idézőjelek közé foglalt.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-144">This is a string value so make sure tooenclose it in double quotes.</span></span> 
   
    ![][75]
2. <span data-ttu-id="b4c7e-145">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-145">Save hello file.</span></span> 
3. <span data-ttu-id="b4c7e-146">Futtassa a következőt: **File -> Engagement -> Generate Android Manifest** (Fájl -> Engagement -> Android-jegyzék létrehozása).</span><span class="sxs-lookup"><span data-stu-id="b4c7e-146">Execute **File -> Engagement -> Generate Android Manifest**.</span></span> <span data-ttu-id="b4c7e-147">Ez a hello Mobile Engagement SDK által hozzáadott hello beépülő modul, és azt automatikusan frissíti a projektbeállításokat.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-147">This is hello plugin added by hello Mobile Engagement SDK and clicking on it will automatically update your project settings.</span></span> 
   
    ![][74]

### <a name="configure-hello-app-tooreceive-notifications"></a><span data-ttu-id="b4c7e-148">Hello app tooreceive értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4c7e-148">Configure hello app tooreceive notifications</span></span>
1. <span data-ttu-id="b4c7e-149">Nyissa meg hello **PlayerController** toohello Player objektum szerkesztésre csatolt parancsprogram.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-149">Open up hello **PlayerController** script attached toohello Player object for editing.</span></span> 
2. <span data-ttu-id="b4c7e-150">Adja hozzá a következő toohello hello `Start()` módszer</span><span class="sxs-lookup"><span data-stu-id="b4c7e-150">Add hello following toohello `Start()` method</span></span>
   
        EngagementReachAgent.Initialize();
3. <span data-ttu-id="b4c7e-151">Most, hogy hello app frissül, telepítése és hello alkalmazást futtatják egy eszközön / hello alábbi utasítások szerint.</span><span class="sxs-lookup"><span data-stu-id="b4c7e-151">Now that hello app is updated, deploy and run hello app on a device per hello instructions provided below.</span></span> 

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
