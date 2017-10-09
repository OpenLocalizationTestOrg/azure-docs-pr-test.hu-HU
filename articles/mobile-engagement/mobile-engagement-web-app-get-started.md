---
title: "a Web Apps aaaGet elindítva, az Azure Mobile Engagement |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések a Web Apps."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04afe53a-4caf-4c80-bd75-20cc630cd75c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: hero-article
ms.date: 06/01/2016
ms.author: piyushjo
ms.openlocfilehash: a84c96cac13bf3b85e72aef55da5c91693e1766c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a><span data-ttu-id="ef0b1-103">Ismerkedés az Azure Mobile Engagement Web webalkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="ef0b1-103">Get started with Azure Mobile Engagement for Web Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="ef0b1-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand a webes alkalmazások használatáról.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your Web App usage.</span></span>

> [!NOTE]
> <span data-ttu-id="ef0b1-105">hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-105">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="ef0b1-106">További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="ef0b1-106">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="ef0b1-107">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="ef0b1-107">This tutorial requires hello following:</span></span>

* <span data-ttu-id="ef0b1-108">Visual Studio 2015 vagy bármely másik választott szerkesztő</span><span class="sxs-lookup"><span data-stu-id="ef0b1-108">Visual Studio 2015 or any other editor of your choice</span></span>
* [<span data-ttu-id="ef0b1-109">Web SDK</span><span class="sxs-lookup"><span data-stu-id="ef0b1-109">Web SDK</span></span>](http://aka.ms/P7b453)

<span data-ttu-id="ef0b1-110">A webszolgáltatási SDK jelenleg előzetes verzióban érhető és csak hello pillanatban támogatja az elemzés és még nem támogatja a küldő böngésző vagy alkalmazásbeli leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-110">This Web SDK is in Preview and only supports Analytics at hello moment and doesn't support sending browser or in-app push notifications yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="ef0b1-111">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-111">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="ef0b1-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ef0b1-113">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).</span><span class="sxs-lookup"><span data-stu-id="ef0b1-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).</span></span>
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a><span data-ttu-id="ef0b1-114">A Mobile Engagement beállítása a webalkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="ef0b1-114">Setup Mobile Engagement for your Web app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="ef0b1-115"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="ef0b1-115"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="ef0b1-116">Ez az oktatóanyag egy "alapszintű integrációt," hello minimálisan szükséges toocollect adatok pedig mutat.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-116">This tutorial presents a "basic integration," which is hello minimal set required toocollect data.</span></span>

<span data-ttu-id="ef0b1-117">Létre fogunk hozni egy alapszintű webalkalmazást a Visual Studio toodemonstrate hello integrációja, ha a Visual Studio kívül is létre webes alkalmazásokkal együtt hello lépések végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-117">We will create a basic web app with Visual Studio toodemonstrate hello integration though you can follow hello steps with any web application created outside of Visual Studio also.</span></span> 

### <a name="create-a-new-web-app"></a><span data-ttu-id="ef0b1-118">Új webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef0b1-118">Create a new Web App</span></span>
<span data-ttu-id="ef0b1-119">hello következő lépések azt feltételezik hello Visual Studio 2015 használatát, ha hello lépések hasonlóak a Visual Studio korábbi verzióiban.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-119">hello following steps assume hello use of Visual Studio 2015 though hello steps are similar in earlier versions of Visual Studio.</span></span> 

1. <span data-ttu-id="ef0b1-120">Indítsa el a Visual Studio és a hello **Home** képernyőn válassza ki **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-120">Start Visual Studio, and in hello **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="ef0b1-121">Hello előugró ablakban válassza **webes** -> **ASP.Net Web Application**.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-121">In hello pop-up, select **Web** -> **ASP.Net Web Application**.</span></span> <span data-ttu-id="ef0b1-122">Töltse ki a hello app **neve**, **hely** és **megoldás neve**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-122">Fill in hello app **Name**, **Location** and  **Solution name**, and then click **OK**.</span></span>
3. <span data-ttu-id="ef0b1-123">A hello **válasszon olyan sablont,** előugró ablakban válasszon **üres** alatt **ASP.Net 4.5 sablonok** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-123">In hello **Select a template** popup, select **Empty** under **ASP.Net 4.5 Templates** and click **OK**.</span></span> 

<span data-ttu-id="ef0b1-124">Most létrehozott egy új üres webalkalmazás projekt amelybe integrálni fogjuk a hello Azure Mobile Engagement webes SDK-t.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-124">You have now created a new blank Web App project into which we will integrate hello Azure Mobile Engagement Web SDK.</span></span>

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="ef0b1-125">Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="ef0b1-125">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="ef0b1-126">Hozzon létre egy új nevű **javascript** a megoldásban, és adja hozzá a hello webes SDK JS fájl **azure-engagement.js** bele.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-126">Create a new folder called **javascript** in your solution and add hello Web SDK JS file **azure-engagement.js** into it.</span></span> 
2. <span data-ttu-id="ef0b1-127">Adjon hozzá egy új fájlt nevű **main.js** hello a következő kódot a javascript mappában.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-127">Add a new file called **main.js** in this javascript folder with hello following code.</span></span> <span data-ttu-id="ef0b1-128">Győződjön meg arról, hogy tooupdate hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-128">Make sure tooupdate hello connection string.</span></span> <span data-ttu-id="ef0b1-129">Ez `azureEngagement` objektum használt tooaccess webes SDK módszerek lesz.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-129">This `azureEngagement` object will be used tooaccess Web SDK methods.</span></span> 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio .js fájlokkal][1]

## <a name="enable-real-time-monitoring"></a><span data-ttu-id="ef0b1-131">Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ef0b1-131">Enable real-time monitoring</span></span>
<span data-ttu-id="ef0b1-132">Rendelés toostart adatküldés és annak biztosítására, hogy hello felhasználók aktív, a el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy tevékenység toohello.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-132">In order toostart sending data and ensuring that hello users are active, you must send at least one Activity toohello Mobile Engagement backend.</span></span> <span data-ttu-id="ef0b1-133">A webalkalmazás kontextusában hello tevékenység egy olyan weblap.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-133">An activity in hello context of a web app is a web page.</span></span> 

1. <span data-ttu-id="ef0b1-134">Hozzon létre egy új lapot nevű **home.html** a megoldás és a webalkalmazás azt hello kiindulási lapon.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-134">Create a new page called **home.html** in your solution and set it as hello starting page for your web app.</span></span> 
2. <span data-ttu-id="ef0b1-135">Hello két JavaScript-kódok hello következő hello törzs címkén belül hozzáadásával a lap korábbi részében hozzáadott tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ef0b1-135">Include hello two javascripts we added earlier in this page by adding hello following within hello body tag.</span></span> 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. <span data-ttu-id="ef0b1-136">Hello törzs címke toocall EngagementAgent tartozó frissítése `startActivity` módszer</span><span class="sxs-lookup"><span data-stu-id="ef0b1-136">Update hello body tag toocall EngagementAgent's `startActivity` method</span></span>
   
        <body onload="engagement.agent.startActivity('Home')">
4. <span data-ttu-id="ef0b1-137">A **home.html** lapnak így kell kinéznie</span><span class="sxs-lookup"><span data-stu-id="ef0b1-137">Here is what your **home.html** will look like</span></span>
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="ef0b1-138">Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="ef0b1-138">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a><span data-ttu-id="ef0b1-139">Az elemzés kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="ef0b1-139">Extend analytics</span></span>
<span data-ttu-id="ef0b1-140">Itt érhetők el minden hello módszerek jelenleg elemzéséhez használható webes SDK-val:</span><span class="sxs-lookup"><span data-stu-id="ef0b1-140">Here are all hello methods currently available with Web SDK that you can use for analytics:</span></span>

1. <span data-ttu-id="ef0b1-141">Tevékenységek/weblapok:</span><span class="sxs-lookup"><span data-stu-id="ef0b1-141">Activities/Web pages:</span></span>
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. <span data-ttu-id="ef0b1-142">Események</span><span class="sxs-lookup"><span data-stu-id="ef0b1-142">Events</span></span>
   
        engagement.agent.sendEvent(name, extras);
3. <span data-ttu-id="ef0b1-143">Hibák</span><span class="sxs-lookup"><span data-stu-id="ef0b1-143">Errors</span></span>
   
        engagement.agent.sendError(name, extras);
4. <span data-ttu-id="ef0b1-144">Feladatok</span><span class="sxs-lookup"><span data-stu-id="ef0b1-144">Jobs</span></span>
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

