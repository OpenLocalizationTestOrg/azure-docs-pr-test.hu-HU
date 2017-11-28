---
title: "Az Azure Mobile Engagement webes SDK-integráció |} Microsoft Docs"
description: "A legújabb frissítéseket és az Azure Mobile Engagement webes SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: b5daa2a2-942b-489d-aa1d-568c3b25e56f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 02/29/2016
ms.author: piyushjo
ms.openlocfilehash: 7d8eaa180e277741a583522ee62d68f5247b92bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a><span data-ttu-id="4e254-103">Integrálása az Azure Mobile Engagement egy webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="4e254-103">Integrate Azure Mobile Engagement in a web application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4e254-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="4e254-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="4e254-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="4e254-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="4e254-106">iOS</span><span class="sxs-lookup"><span data-stu-id="4e254-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="4e254-107">Android</span><span class="sxs-lookup"><span data-stu-id="4e254-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="4e254-108">Az ebben a cikkben szereplő eljárásokat a legegyszerűbben úgy, hogy aktiválja a elemzés és a webalkalmazás az Azure Mobile Engagement figyelési funkciók.</span><span class="sxs-lookup"><span data-stu-id="4e254-108">The procedures in this article describe the simplest way to activate the analytics and monitoring functions in Azure Mobile Engagement in your web application.</span></span>

<span data-ttu-id="4e254-109">Kövesse a jelentések, amelyek szükségesek ahhoz, hogy a felhasználók, munkamenetek, tevékenységekhez, összeomlik és technicals összes statisztikája számítási aktiválásához.</span><span class="sxs-lookup"><span data-stu-id="4e254-109">Follow the steps to activate the log reports that are needed to compute all statistics about users, sessions, activities, crashes, and technicals.</span></span> <span data-ttu-id="4e254-110">Az alkalmazás-függő statisztikákat, pl.: eseményeket, hibákat és feladatokat aktiválnia kell a jelentések manuálisan az Azure Mobile Engagement API használatával.</span><span class="sxs-lookup"><span data-stu-id="4e254-110">For application-dependent statistics, such as events, errors, and jobs, you must activate log reports manually by using the Azure Mobile Engagement API.</span></span> <span data-ttu-id="4e254-111">További információt a további [használata a speciális API webalkalmazás szerinti címkézését a Mobile Engagement](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="4e254-111">For more information, learn [how to use the advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="4e254-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="4e254-112">Introduction</span></span>
<span data-ttu-id="4e254-113">[Töltse le az Azure Mobile Engagement SDK webes](http://aka.ms/P7b453).</span><span class="sxs-lookup"><span data-stu-id="4e254-113">[Download the Azure Mobile Engagement Web SDK](http://aka.ms/P7b453).</span></span>
<span data-ttu-id="4e254-114">A Mobile Engagement webes SDK egyetlen JavaScript fájlként van kiadva, hogy a hely vagy a webes alkalmazás egyes lapján szerepeljenek azure-engagement.js.</span><span class="sxs-lookup"><span data-stu-id="4e254-114">The Mobile Engagement Web SDK is shipped as a single JavaScript file, azure-engagement.js, which you have to include in each page of your site or web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4e254-115">Ez a parancsfájl futtatása előtt futtatnia kell egy parancsfájl vagy a kód részlet, hogy írni a Mobile Engagement beállítása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4e254-115">Before you run this script, you must run a script or code snippet that you write to configure Mobile Engagement for your application.</span></span>
> 
> 

## <a name="browser-compatibility"></a><span data-ttu-id="4e254-116">Böngészőkompatibilitás</span><span class="sxs-lookup"><span data-stu-id="4e254-116">Browser compatibility</span></span>
<span data-ttu-id="4e254-117">A Mobile Engagement webes SDK-t használ a natív JSON kódolási és dekódolási, tartományokon átívelő AJAX-kérelmek (a W3C CORS megadását hagyatkoznia) mellett.</span><span class="sxs-lookup"><span data-stu-id="4e254-117">The Mobile Engagement Web SDK uses native JSON encoding and decoding, in addition to cross-domain AJAX requests (relying on the W3C CORS specification).</span></span> <span data-ttu-id="4e254-118">Az alábbi böngészők kompatibilis:</span><span class="sxs-lookup"><span data-stu-id="4e254-118">It's compatible with the following browsers:</span></span>

* <span data-ttu-id="4e254-119">A Microsoft Edge 12 +</span><span class="sxs-lookup"><span data-stu-id="4e254-119">Microsoft Edge 12+</span></span>
* <span data-ttu-id="4e254-120">Az Internet Explorer 10 +</span><span class="sxs-lookup"><span data-stu-id="4e254-120">Internet Explorer 10+</span></span>
* <span data-ttu-id="4e254-121">Firefox 3.5 +</span><span class="sxs-lookup"><span data-stu-id="4e254-121">Firefox 3.5+</span></span>
* <span data-ttu-id="4e254-122">Chrome 4 +</span><span class="sxs-lookup"><span data-stu-id="4e254-122">Chrome 4+</span></span>
* <span data-ttu-id="4e254-123">Safari 6 +</span><span class="sxs-lookup"><span data-stu-id="4e254-123">Safari 6+</span></span>
* <span data-ttu-id="4e254-124">Opera 12 +</span><span class="sxs-lookup"><span data-stu-id="4e254-124">Opera 12+</span></span>

## <a name="configure-mobile-engagement"></a><span data-ttu-id="4e254-125">Mobilmarketing konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4e254-125">Configure Mobile Engagement</span></span>
<span data-ttu-id="4e254-126">Írni egy parancsfájlt, amely létrehoz egy globális `azureEngagement` JavaScript object, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="4e254-126">Write a script that creates a global `azureEngagement` JavaScript object, as in the following example.</span></span> <span data-ttu-id="4e254-127">A hely rendelkezhet olyan Többszörösök lapokat, mert ez a példa feltételezi, hogy ez a parancsfájl minden oldalon szerepel-e.</span><span class="sxs-lookup"><span data-stu-id="4e254-127">Because your site might have multiples pages, this example assumes that this script is included in every page.</span></span> <span data-ttu-id="4e254-128">Ebben a példában a JavaScript-objektum neve `azure-engagement-conf.js`.</span><span class="sxs-lookup"><span data-stu-id="4e254-128">In this example, the JavaScript object is named `azure-engagement-conf.js`.</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

<span data-ttu-id="4e254-129">A `connectionString` érték, az alkalmazás az Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4e254-129">The `connectionString` value for your application is displayed in the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="4e254-130">`appVersionName`és `appVersionCode` megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="4e254-130">`appVersionName` and `appVersionCode` are optional.</span></span> <span data-ttu-id="4e254-131">Azt javasoljuk azonban, hogy konfigurálja azokat, hogy analytics képes a fájlverzió-információkat.</span><span class="sxs-lookup"><span data-stu-id="4e254-131">However, we recommend that you configure them so that analytics can process version information.</span></span>
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a><span data-ttu-id="4e254-132">A Mobile Engagement parancsfájlok szerepeljenek a lapokon</span><span class="sxs-lookup"><span data-stu-id="4e254-132">Include Mobile Engagement scripts in your pages</span></span>
<span data-ttu-id="4e254-133">A Mobile Engagement parancsfájlok hozzáadása a lap a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="4e254-133">Add Mobile Engagement scripts to your pages in one of the following ways:</span></span>

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

<span data-ttu-id="4e254-134">Vagy ezt:</span><span class="sxs-lookup"><span data-stu-id="4e254-134">Or this:</span></span>

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a><span data-ttu-id="4e254-135">Alias</span><span class="sxs-lookup"><span data-stu-id="4e254-135">Alias</span></span>
<span data-ttu-id="4e254-136">Miután a Mobile Engagement webes SDK-parancsprogramot be van töltve, létrehozza a **engagement** alias az SDK API-k elérésére.</span><span class="sxs-lookup"><span data-stu-id="4e254-136">After the Mobile Engagement Web SDK script is loaded, it creates the **engagement** alias to access the SDK APIs.</span></span> <span data-ttu-id="4e254-137">Az alias nem használható SDK konfigurációjának meghatározása.</span><span class="sxs-lookup"><span data-stu-id="4e254-137">You cannot use this alias to define the SDK configuration.</span></span> <span data-ttu-id="4e254-138">Ez az alias lesz a jelen dokumentációban lévő hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="4e254-138">This alias is used as a reference in this documentation.</span></span>

<span data-ttu-id="4e254-139">Vegye figyelembe, hogy ha az alapértelmezett alias ütközik egy másik globális változó a lapról, akkor újra meg lehet adni azt a konfigurációban az alábbiak szerint ahhoz, hogy betöltve a Mobile Engagement webes SDK:</span><span class="sxs-lookup"><span data-stu-id="4e254-139">Note that if the default alias conflicts with another global variable from your page, you can redefine it in the configuration as follows before you load the Mobile Engagement Web SDK:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a><span data-ttu-id="4e254-140">Alapvető jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="4e254-140">Basic reporting</span></span>
<span data-ttu-id="4e254-141">A Mobile Engagement alapvető jelentéskészítési munkamenet szintű statisztika, például a felhasználók, a munkamenetek, a tevékenységek és az összeomlások statisztikája ismerteti.</span><span class="sxs-lookup"><span data-stu-id="4e254-141">Basic reporting in Mobile Engagement covers session-level statistics, such as statistics about users, sessions, activities, and crashes.</span></span>

### <a name="session-tracking"></a><span data-ttu-id="4e254-142">Munkamenet-követés</span><span class="sxs-lookup"><span data-stu-id="4e254-142">Session tracking</span></span>
<span data-ttu-id="4e254-143">A Mobile Engagement munkamenet tevékenységek sorrendje oszlik, minden egyes névvel azonosíthatók.</span><span class="sxs-lookup"><span data-stu-id="4e254-143">A Mobile Engagement session is divided into a sequence of activities, each identified by a name.</span></span>

<span data-ttu-id="4e254-144">Klasszikus webhelyen azt javasoljuk, hogy a hely minden oldalon egy másik tevékenység deklarálja.</span><span class="sxs-lookup"><span data-stu-id="4e254-144">In a classic website, we recommend that you declare a different activity on each page of your site.</span></span> <span data-ttu-id="4e254-145">Egy webhelyre vagy webalkalmazásra alkalmazást, amelyben az aktuális oldalon soha nem változik érdemes nyomon követheti a tevékenységek kisebb méretű, például a lapon belül.</span><span class="sxs-lookup"><span data-stu-id="4e254-145">For a website or web application in which the current page never changes, you might want to track the activities on a smaller scale, such as within the page.</span></span>

<span data-ttu-id="4e254-146">Mindkét esetben indítsa el, vagy módosítsa az aktuális felhasználói tevékenység, hívja meg a `engagement.agent.startActivity` függvény.</span><span class="sxs-lookup"><span data-stu-id="4e254-146">Either way, to start or change the current user activity, call the `engagement.agent.startActivity` function.</span></span> <span data-ttu-id="4e254-147">Példa:</span><span class="sxs-lookup"><span data-stu-id="4e254-147">For example:</span></span>

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

<span data-ttu-id="4e254-148">A Mobile Engagement-kiszolgáló automatikusan az alkalmazás lap bezárása után három percen belül munkamenetekkel karakterlánccal végződik-e.</span><span class="sxs-lookup"><span data-stu-id="4e254-148">The Mobile Engagement server automatically ends an open session within three minutes after the application page is closed.</span></span>

<span data-ttu-id="4e254-149">Azt is megteheti, hogy is munkamenet befejezése manuálisan meghívásával `engagement.agent.endActivity`.</span><span class="sxs-lookup"><span data-stu-id="4e254-149">Alternatively, you can end a session manually by calling `engagement.agent.endActivity`.</span></span> <span data-ttu-id="4e254-150">Ez beállítja, hogy az aktuális felhasználói tevékenységet "Inaktív".</span><span class="sxs-lookup"><span data-stu-id="4e254-150">This sets the current user activity to 'Idle.'</span></span>  <span data-ttu-id="4e254-151">A munkamenet véget ér 10 másodperc múlva, kivéve, ha új hívása `engagement.agent.startActivity` folytatja a munkamenet.</span><span class="sxs-lookup"><span data-stu-id="4e254-151">The session will end 10 seconds later unless a new call to `engagement.agent.startActivity` resumes the session.</span></span>

<span data-ttu-id="4e254-152">A globális bevonási objektumot, az alábbiak szerint konfigurálhatja a 10 másodperces késleltetési:</span><span class="sxs-lookup"><span data-stu-id="4e254-152">You can configure the 10-second delay in the global engagement object, as follows:</span></span>

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [!NOTE]
> <span data-ttu-id="4e254-153">Nem használhat `engagement.agent.endActivity` a a `onunload` visszahívási mert AJAX-hívások nem hajtható végre ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="4e254-153">You cannot use `engagement.agent.endActivity` in the `onunload` callback because you cannot make AJAX calls at this stage.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="4e254-154">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="4e254-154">Advanced reporting</span></span>
<span data-ttu-id="4e254-155">Szükség esetén szeretne jelentést készíteni, az alkalmazás-specifikus eseményeket, hibákat és feladatok, ha szeretné a Mobile Engagement API-t használja.</span><span class="sxs-lookup"><span data-stu-id="4e254-155">Optionally, if you want to report application-specific events, errors, and jobs, you need to use the Mobile Engagement API.</span></span> <span data-ttu-id="4e254-156">A Mobile Engagement API keresztül hozzáférhet a `engagement.agent` objektum.</span><span class="sxs-lookup"><span data-stu-id="4e254-156">You access the Mobile Engagement API through the `engagement.agent` object.</span></span>

<span data-ttu-id="4e254-157">A speciális funkciók a Mobile Engagement a Mobile Engagement API érheti el.</span><span class="sxs-lookup"><span data-stu-id="4e254-157">You can access all of the advanced capabilities in Mobile Engagement in the Mobile Engagement API.</span></span> <span data-ttu-id="4e254-158">Az API-cikkben található részletes [használata a speciális API webalkalmazás szerinti címkézését a Mobile Engagement](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="4e254-158">The API is detailed in the article [How to use the advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="customize-the-urls-used-for-ajax-calls"></a><span data-ttu-id="4e254-159">Az AJAX-hívások használt URL-címek testreszabása</span><span class="sxs-lookup"><span data-stu-id="4e254-159">Customize the URLs used for AJAX calls</span></span>
<span data-ttu-id="4e254-160">Testre szabhatja a Mobile Engagement webes SDK-t használó URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="4e254-160">You can customize URLs that the Mobile Engagement Web SDK uses.</span></span> <span data-ttu-id="4e254-161">Például a napló URL-címe (az SDK végpont naplózás) újra, konfigurációt lehet felülbírálni a ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="4e254-161">For example, to redefine the log URL (the SDK endpoint for logging), you can override the configuration like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

<span data-ttu-id="4e254-162">Ha az URL-címet adja vissza, amely kezdődik `/`, `//`, `http://`, vagy `https://`, az alapértelmezett séma nem használatos.</span><span class="sxs-lookup"><span data-stu-id="4e254-162">If your URL functions return a string that begins with `/`, `//`, `http://`, or `https://`, the default scheme is not used.</span></span> <span data-ttu-id="4e254-163">Alapértelmezés szerint a `https://` séma az URL-címeket szolgál.</span><span class="sxs-lookup"><span data-stu-id="4e254-163">By default, the `https://` scheme is used for those URLs.</span></span> <span data-ttu-id="4e254-164">Ha szeretné testre szabni az alapértelmezett séma, bírálja felül a konfigurációt, ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="4e254-164">If you want to customize the default scheme, override the configuration, like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
