---
title: "aaaGet elindítva az Azure Mobile Engagement Unity iOS üzemelő"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések Unity-alkalmazásokhoz tooiOS eszközök telepítése."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7ddfbac3-8d13-4ebe-b061-c865f357297f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f4247b0a9240cbe2bf1a6618388919d3554c07fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a><span data-ttu-id="c5b7e-103">Ismerkedés az Azure Mobile Engagement Unity iOS üzemelő példánnyal való használatával</span><span class="sxs-lookup"><span data-stu-id="c5b7e-103">Get Started with Azure Mobile Engagement for Unity iOS deployment</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="c5b7e-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és toosend a leküldéses értesítések toosegmented felhasználók Unity-alkalmazás tooan iOS-eszköz telepítése során.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of a Unity application when deploying tooan iOS device.</span></span>
<span data-ttu-id="c5b7e-105">A oktatóanyag használ hello Unity klasszikus Roll golyó oktatóanyag, hello kiindulási pontjaként.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-105">This tutorial uses hello classic Unity Roll a Ball tutorial as hello starting point.</span></span> <span data-ttu-id="c5b7e-106">Ezen hello lépéseket kell követnie [oktatóanyag](mobile-engagement-unity-roll-a-ball.md) hello a Mobile Engagement-integrációs oktatóanyaggal az alábbi hello oktatóanyag folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-106">You should follow hello steps in this [tutorial](mobile-engagement-unity-roll-a-ball.md) before proceeding with hello Mobile Engagement integration we showcase in hello tutorial below.</span></span> 

<span data-ttu-id="c5b7e-107">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="c5b7e-107">This tutorial requires hello following:</span></span>

* [<span data-ttu-id="c5b7e-108">Unity Editor</span><span class="sxs-lookup"><span data-stu-id="c5b7e-108">Unity Editor</span></span>](http://unity3d.com/get-unity)
* [<span data-ttu-id="c5b7e-109">Mobile Engagement Unity SDK</span><span class="sxs-lookup"><span data-stu-id="c5b7e-109">Mobile Engagement Unity SDK</span></span>](https://aka.ms/azmeunitysdk)
* <span data-ttu-id="c5b7e-110">XCode Editor</span><span class="sxs-lookup"><span data-stu-id="c5b7e-110">XCode Editor</span></span>

> [!NOTE]
> <span data-ttu-id="c5b7e-111">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-111">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="c5b7e-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c5b7e-113">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="c5b7e-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).</span></span>
> 
> 

## <span data-ttu-id="c5b7e-114"><a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="c5b7e-114"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="c5b7e-115"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="c5b7e-115"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
### <a name="import-hello-unity-package"></a><span data-ttu-id="c5b7e-116">Hello Unity-csomag importálása</span><span class="sxs-lookup"><span data-stu-id="c5b7e-116">Import hello Unity package</span></span>
1. <span data-ttu-id="c5b7e-117">Töltse le a hello [Mobile Engagement Unity-csomagot](https://aka.ms/azmeunitysdk) , és mentse tooyour helyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-117">Download hello [Mobile Engagement Unity package](https://aka.ms/azmeunitysdk) and save it tooyour local machine.</span></span> 
2. <span data-ttu-id="c5b7e-118">Nyissa meg túl**eszközök -> csomag importálása -> egyéni csomag** és a fenti lépés hello letöltött válassza hello csomagot.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-118">Go too**Assets -> Import Package -> Custom Package** and select hello package you downloaded in hello above step.</span></span> 
   
    ![][70] 
3. <span data-ttu-id="c5b7e-119">Ügyeljen arra, hogy minden fájl ki legyen választva, és kattintson az **Import** (Importálás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-119">Make sure all files are selected and click **Import** button.</span></span> 
   
    ![][71] 
4. <span data-ttu-id="c5b7e-120">Ha sikeres importálás, látni fogja hello importált SDK-fájlok a projektben.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-120">Once Import is successful, you will see hello imported SDK files in your project.</span></span>  
   
    ![][72] 

### <a name="update-hello-engagementconfiguration"></a><span data-ttu-id="c5b7e-121">Hello EngagementConfiguration frissítése</span><span class="sxs-lookup"><span data-stu-id="c5b7e-121">Update hello EngagementConfiguration</span></span>
1. <span data-ttu-id="c5b7e-122">Nyissa meg hello **EngagementConfiguration** hello SDK mappából, és frissítse hello parancsfájlt **IOS\_kapcsolat\_karakterlánc** korábban beszerzett hello kapcsolati karakterlánccal a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-122">Open up hello **EngagementConfiguration** script file from hello SDK folder and update hello **IOS\_CONNECTION\_STRING** with hello connection string you obtained earlier from hello Azure portal.</span></span>  
   
    ![][73]
2. <span data-ttu-id="c5b7e-123">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-123">Save hello file.</span></span> 

### <a name="configure-hello-app-for-basic-tracking"></a><span data-ttu-id="c5b7e-124">Alapszintű nyomkövetéshez hello alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c5b7e-124">Configure hello app for basic tracking</span></span>
1. <span data-ttu-id="c5b7e-125">Nyissa meg hello **PlayerController** toohello Player objektum szerkesztésre csatolt parancsprogram.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-125">Open up hello **PlayerController** script attached toohello Player object for editing.</span></span> 
2. <span data-ttu-id="c5b7e-126">Adja hozzá hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="c5b7e-126">Add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Unity;
3. <span data-ttu-id="c5b7e-127">Adja hozzá a következő toohello hello `Start()` módszer</span><span class="sxs-lookup"><span data-stu-id="c5b7e-127">Add hello following toohello `Start()` method</span></span>
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a><span data-ttu-id="c5b7e-128">Regisztrálhat és futtathat hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c5b7e-128">Deploy and run hello app</span></span>
1. <span data-ttu-id="c5b7e-129">Csatlakoztassa az iOS eszköz tooyour gépet.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-129">Connect an iOS device tooyour machine.</span></span> 
2. <span data-ttu-id="c5b7e-130">Nyissa meg a **File -> Build Settings** (Fájl -> Létrehozási beállítások) menüpontot.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-130">Open up **File -> Build Settings**</span></span> 
   
    ![][40]
3. <span data-ttu-id="c5b7e-131">Válassza az **iOS** lehetőséget, majd kattintson a **Switch Platform** (Platformváltás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-131">Select **iOS** and then click on **Switch Platform**</span></span>
   
    ![][41]
   
    ![][42]
4. <span data-ttu-id="c5b7e-132">Kattintson a **Player settings** (Lejátszó beállításai) lehetőségre, és adjon meg egy érvényes értéket a Bundle Identifier (Csomagazonosító) számára.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-132">Click on **Player settings** and provide a valid Bundle Identifier.</span></span> 
   
    ![][53]
5. <span data-ttu-id="c5b7e-133">Végül kattintson a **Build And Run** (Létrehozás és futtatás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-133">Finally click on **Build And Run**</span></span>
   
    ![][54]
6. <span data-ttu-id="c5b7e-134">Előfordulhat, hogy ismételt tooprovide mappa neve toostore hello iOS csomagot.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-134">You may be asked tooprovide a folder name toostore hello iOS package.</span></span> 
   
    ![][43]
7. <span data-ttu-id="c5b7e-135">Ha a művelet sikeres, hello projekt le lesz fordítva, és hogy megnyitható az XCode-alkalmazással a.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-135">If everything goes fine, then hello project will be compiled and you should open it up on your XCode application.</span></span> 
8. <span data-ttu-id="c5b7e-136">Győződjön meg arról, hogy hello **csomagazonosítót** helyes hello projektben.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-136">Make sure that hello **Bundle identifier** is correct in hello project.</span></span>  
   
    ![][75]
9. <span data-ttu-id="c5b7e-137">Most futtassa hello alkalmazást az xcode-ban, hogy hello csomag telepített tooyour csatlakoztatott eszközön, és a Unity-játék megjelenik a telefonon!</span><span class="sxs-lookup"><span data-stu-id="c5b7e-137">Now run hello app in XCode so that hello package is deployed tooyour connected device and you should see your Unity game on your phone!</span></span> 

## <span data-ttu-id="c5b7e-138"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="c5b7e-138"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="c5b7e-139"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c5b7e-139"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="c5b7e-140">Mobile Engagement lehetővé teszi a felhasználókkal toointeract és elérése révén a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-140">Mobile Engagement allows you toointeract with your users and REACH with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="c5b7e-141">Ez a modul REACH neve hello a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-141">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="c5b7e-142">Nincs toodo további konfiguráció a alkalmazáson belüli tooreceive értesítések, és már állítva.</span><span class="sxs-lookup"><span data-stu-id="c5b7e-142">You don't have toodo any additional configuration in your app tooreceive notifications and it is already setup for it.</span></span>

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
