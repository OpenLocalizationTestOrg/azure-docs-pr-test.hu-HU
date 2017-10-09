---
title: "Mobile Engagement webes SDK-integráció aaaAzure |} Microsoft Docs"
description: "hello a legújabb frissítésekről és Azure Mobile Engagement webes SDK hello eljárásai"
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
ms.openlocfilehash: 99613b68b615bec4ddcfcc8e4e0133ce9d887bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a><span data-ttu-id="caba9-103">Integrálása az Azure Mobile Engagement egy webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="caba9-103">Integrate Azure Mobile Engagement in a web application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="caba9-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="caba9-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="caba9-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="caba9-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="caba9-106">iOS</span><span class="sxs-lookup"><span data-stu-id="caba9-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="caba9-107">Android</span><span class="sxs-lookup"><span data-stu-id="caba9-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="caba9-108">Ez a cikk hello eljárásokat mutatunk be, hello legegyszerűbb módja tooactivate hello elemzés és a monitorozási funkciók az Azure Mobile Engagement a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="caba9-108">hello procedures in this article describe hello simplest way tooactivate hello analytics and monitoring functions in Azure Mobile Engagement in your web application.</span></span>

<span data-ttu-id="caba9-109">Hello lépéseket tooactivate hello a jelentések, amelyek a szükséges toocompute kövesse a felhasználók, munkamenetek, tevékenységekhez, összeomlik és technicals összes statisztikája.</span><span class="sxs-lookup"><span data-stu-id="caba9-109">Follow hello steps tooactivate hello log reports that are needed toocompute all statistics about users, sessions, activities, crashes, and technicals.</span></span> <span data-ttu-id="caba9-110">Az alkalmazás-függő statisztikákat, pl.: eseményeket, hibákat és feladatokat aktiválnia kell a jelentések manuálisan hello Azure Mobile Engagement API használatával.</span><span class="sxs-lookup"><span data-stu-id="caba9-110">For application-dependent statistics, such as events, errors, and jobs, you must activate log reports manually by using hello Azure Mobile Engagement API.</span></span> <span data-ttu-id="caba9-111">További információt a további [hogyan toouse hello speciális API webalkalmazás szerinti címkézését a Mobile Engagement](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="caba9-111">For more information, learn [how toouse hello advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="caba9-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="caba9-112">Introduction</span></span>
<span data-ttu-id="caba9-113">[Töltse le az Azure Mobile Engagement webes SDK hello](http://aka.ms/P7b453).</span><span class="sxs-lookup"><span data-stu-id="caba9-113">[Download hello Azure Mobile Engagement Web SDK](http://aka.ms/P7b453).</span></span>
<span data-ttu-id="caba9-114">Mobile Engagement webes SDK-t egyetlen JavaScript-fájlként van kiadva hello, hogy rendelkezik azure-engagement.js tooinclude a hely vagy a webes alkalmazás egyes lapon.</span><span class="sxs-lookup"><span data-stu-id="caba9-114">hello Mobile Engagement Web SDK is shipped as a single JavaScript file, azure-engagement.js, which you have tooinclude in each page of your site or web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="caba9-115">Ez a parancsfájl futtatása előtt kell parancsfájllal vagy, hogy az alkalmazás írása a Mobile Engagement tooconfigure részlet kód.</span><span class="sxs-lookup"><span data-stu-id="caba9-115">Before you run this script, you must run a script or code snippet that you write tooconfigure Mobile Engagement for your application.</span></span>
> 
> 

## <a name="browser-compatibility"></a><span data-ttu-id="caba9-116">Böngészőkompatibilitás</span><span class="sxs-lookup"><span data-stu-id="caba9-116">Browser compatibility</span></span>
<span data-ttu-id="caba9-117">Mobile Engagement webes SDK hello natív JSON kódolási és dekódolási, továbbá toocross-tartomány AJAX-kérelmek (hello W3C CORS specification hagyatkoznia) használja.</span><span class="sxs-lookup"><span data-stu-id="caba9-117">hello Mobile Engagement Web SDK uses native JSON encoding and decoding, in addition toocross-domain AJAX requests (relying on hello W3C CORS specification).</span></span> <span data-ttu-id="caba9-118">A következő böngészőkben hello kompatibilis:</span><span class="sxs-lookup"><span data-stu-id="caba9-118">It's compatible with hello following browsers:</span></span>

* <span data-ttu-id="caba9-119">A Microsoft Edge 12 +</span><span class="sxs-lookup"><span data-stu-id="caba9-119">Microsoft Edge 12+</span></span>
* <span data-ttu-id="caba9-120">Az Internet Explorer 10 +</span><span class="sxs-lookup"><span data-stu-id="caba9-120">Internet Explorer 10+</span></span>
* <span data-ttu-id="caba9-121">Firefox 3.5 +</span><span class="sxs-lookup"><span data-stu-id="caba9-121">Firefox 3.5+</span></span>
* <span data-ttu-id="caba9-122">Chrome 4 +</span><span class="sxs-lookup"><span data-stu-id="caba9-122">Chrome 4+</span></span>
* <span data-ttu-id="caba9-123">Safari 6 +</span><span class="sxs-lookup"><span data-stu-id="caba9-123">Safari 6+</span></span>
* <span data-ttu-id="caba9-124">Opera 12 +</span><span class="sxs-lookup"><span data-stu-id="caba9-124">Opera 12+</span></span>

## <a name="configure-mobile-engagement"></a><span data-ttu-id="caba9-125">Mobilmarketing konfigurálása</span><span class="sxs-lookup"><span data-stu-id="caba9-125">Configure Mobile Engagement</span></span>
<span data-ttu-id="caba9-126">Írni egy parancsfájlt, amely létrehoz egy globális `azureEngagement` JavaScript-objektum, mint például a következő hello.</span><span class="sxs-lookup"><span data-stu-id="caba9-126">Write a script that creates a global `azureEngagement` JavaScript object, as in hello following example.</span></span> <span data-ttu-id="caba9-127">A hely rendelkezhet olyan Többszörösök lapokat, mert ez a példa feltételezi, hogy ez a parancsfájl minden oldalon szerepel-e.</span><span class="sxs-lookup"><span data-stu-id="caba9-127">Because your site might have multiples pages, this example assumes that this script is included in every page.</span></span> <span data-ttu-id="caba9-128">Ebben a példában hello JavaScript objektum neve `azure-engagement-conf.js`.</span><span class="sxs-lookup"><span data-stu-id="caba9-128">In this example, hello JavaScript object is named `azure-engagement-conf.js`.</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

<span data-ttu-id="caba9-129">Hello `connectionString` értéket az alkalmazás megjelenik hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="caba9-129">hello `connectionString` value for your application is displayed in hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="caba9-130">`appVersionName`és `appVersionCode` megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="caba9-130">`appVersionName` and `appVersionCode` are optional.</span></span> <span data-ttu-id="caba9-131">Azt javasoljuk azonban, hogy konfigurálja azokat, hogy analytics képes a fájlverzió-információkat.</span><span class="sxs-lookup"><span data-stu-id="caba9-131">However, we recommend that you configure them so that analytics can process version information.</span></span>
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a><span data-ttu-id="caba9-132">A Mobile Engagement parancsfájlok szerepeljenek a lapokon</span><span class="sxs-lookup"><span data-stu-id="caba9-132">Include Mobile Engagement scripts in your pages</span></span>
<span data-ttu-id="caba9-133">Adja hozzá a Mobile Engagement parancsfájlok tooyour lapok hello a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="caba9-133">Add Mobile Engagement scripts tooyour pages in one of hello following ways:</span></span>

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

<span data-ttu-id="caba9-134">Vagy ezt:</span><span class="sxs-lookup"><span data-stu-id="caba9-134">Or this:</span></span>

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a><span data-ttu-id="caba9-135">Alias</span><span class="sxs-lookup"><span data-stu-id="caba9-135">Alias</span></span>
<span data-ttu-id="caba9-136">Miután a Mobile Engagement webes SDK parancsfájl hello be van töltve, hello létrehoz **engagement** alias tooaccess hello SDK API-k.</span><span class="sxs-lookup"><span data-stu-id="caba9-136">After hello Mobile Engagement Web SDK script is loaded, it creates hello **engagement** alias tooaccess hello SDK APIs.</span></span> <span data-ttu-id="caba9-137">Az alias toodefine hello SDK konfigurációs nem használható.</span><span class="sxs-lookup"><span data-stu-id="caba9-137">You cannot use this alias toodefine hello SDK configuration.</span></span> <span data-ttu-id="caba9-138">Ez az alias lesz a jelen dokumentációban lévő hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="caba9-138">This alias is used as a reference in this documentation.</span></span>

<span data-ttu-id="caba9-139">Vegye figyelembe, hogy ha hello alapértelmezett alias ütközik egy másik globális változó a lapról, akkor újra meg lehet adni azt hello konfigurációban az alábbiak szerint előtt betölteni a Mobile Engagement webes SDK hello:</span><span class="sxs-lookup"><span data-stu-id="caba9-139">Note that if hello default alias conflicts with another global variable from your page, you can redefine it in hello configuration as follows before you load hello Mobile Engagement Web SDK:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a><span data-ttu-id="caba9-140">Alapvető jelentéskészítési</span><span class="sxs-lookup"><span data-stu-id="caba9-140">Basic reporting</span></span>
<span data-ttu-id="caba9-141">A Mobile Engagement alapvető jelentéskészítési munkamenet szintű statisztika, például a felhasználók, a munkamenetek, a tevékenységek és az összeomlások statisztikája ismerteti.</span><span class="sxs-lookup"><span data-stu-id="caba9-141">Basic reporting in Mobile Engagement covers session-level statistics, such as statistics about users, sessions, activities, and crashes.</span></span>

### <a name="session-tracking"></a><span data-ttu-id="caba9-142">Munkamenet-követés</span><span class="sxs-lookup"><span data-stu-id="caba9-142">Session tracking</span></span>
<span data-ttu-id="caba9-143">A Mobile Engagement munkamenet tevékenységek sorrendje oszlik, minden egyes névvel azonosíthatók.</span><span class="sxs-lookup"><span data-stu-id="caba9-143">A Mobile Engagement session is divided into a sequence of activities, each identified by a name.</span></span>

<span data-ttu-id="caba9-144">Klasszikus webhelyen azt javasoljuk, hogy a hely minden oldalon egy másik tevékenység deklarálja.</span><span class="sxs-lookup"><span data-stu-id="caba9-144">In a classic website, we recommend that you declare a different activity on each page of your site.</span></span> <span data-ttu-id="caba9-145">Egy webhelyre vagy webalkalmazásra alkalmazáshoz, mely hello az aktuális lap soha nem változik érdemes lehet tootrack hello tevékenységek kisebb méretű, például hello oldalán.</span><span class="sxs-lookup"><span data-stu-id="caba9-145">For a website or web application in which hello current page never changes, you might want tootrack hello activities on a smaller scale, such as within hello page.</span></span>

<span data-ttu-id="caba9-146">Vagy módon toostart, vagy módosítsa hello aktuális felhasználói tevékenység, hívás hello `engagement.agent.startActivity` függvény.</span><span class="sxs-lookup"><span data-stu-id="caba9-146">Either way, toostart or change hello current user activity, call hello `engagement.agent.startActivity` function.</span></span> <span data-ttu-id="caba9-147">Példa:</span><span class="sxs-lookup"><span data-stu-id="caba9-147">For example:</span></span>

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

<span data-ttu-id="caba9-148">hello a Mobile Engagement-kiszolgáló automatikusan munkamenetekkel hello alkalmazáslap bezárása után három percen belül befejeződik.</span><span class="sxs-lookup"><span data-stu-id="caba9-148">hello Mobile Engagement server automatically ends an open session within three minutes after hello application page is closed.</span></span>

<span data-ttu-id="caba9-149">Azt is megteheti, hogy is munkamenet befejezése manuálisan meghívásával `engagement.agent.endActivity`.</span><span class="sxs-lookup"><span data-stu-id="caba9-149">Alternatively, you can end a session manually by calling `engagement.agent.endActivity`.</span></span> <span data-ttu-id="caba9-150">Hello aktuális felhasználói tevékenység too'Idle állít be. "</span><span class="sxs-lookup"><span data-stu-id="caba9-150">This sets hello current user activity too'Idle.'</span></span>  <span data-ttu-id="caba9-151">hello munkamenet véget ér 10 másodperc múlva, kivéve, ha egy új hívása túl`engagement.agent.startActivity` hello-munkamenet folytatása.</span><span class="sxs-lookup"><span data-stu-id="caba9-151">hello session will end 10 seconds later unless a new call too`engagement.agent.startActivity` resumes hello session.</span></span>

<span data-ttu-id="caba9-152">Konfigurálhatja hello 10 másodperces késleltetési hello globális bevonási objektum, a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="caba9-152">You can configure hello 10-second delay in hello global engagement object, as follows:</span></span>

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end hello session as soon as endActivity is called

> [!NOTE]
> <span data-ttu-id="caba9-153">Nem használhat `engagement.agent.endActivity` a hello `onunload` visszahívási mert AJAX-hívások nem hajtható végre ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="caba9-153">You cannot use `engagement.agent.endActivity` in hello `onunload` callback because you cannot make AJAX calls at this stage.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="caba9-154">Speciális jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="caba9-154">Advanced reporting</span></span>
<span data-ttu-id="caba9-155">Nem kötelező Ha azt szeretné, tooreport alkalmazásspecifikus eseményeket, hibákat és feladatok, kell toouse hello Mobile Engagement API.</span><span class="sxs-lookup"><span data-stu-id="caba9-155">Optionally, if you want tooreport application-specific events, errors, and jobs, you need toouse hello Mobile Engagement API.</span></span> <span data-ttu-id="caba9-156">Hello keresztül érhető el a Mobile Engagement API hello `engagement.agent` objektum.</span><span class="sxs-lookup"><span data-stu-id="caba9-156">You access hello Mobile Engagement API through hello `engagement.agent` object.</span></span>

<span data-ttu-id="caba9-157">Az összes speciális lehetőségeket a Mobile Engagement a Mobile Engagement API hello hello érheti el.</span><span class="sxs-lookup"><span data-stu-id="caba9-157">You can access all of hello advanced capabilities in Mobile Engagement in hello Mobile Engagement API.</span></span> <span data-ttu-id="caba9-158">hello API hello cikkben található részletes [hogyan toouse hello speciális API webalkalmazás szerinti címkézését a Mobile Engagement](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="caba9-158">hello API is detailed in hello article [How toouse hello advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="customize-hello-urls-used-for-ajax-calls"></a><span data-ttu-id="caba9-159">Az AJAX-hívások használt URL-címek hello testreszabása</span><span class="sxs-lookup"><span data-stu-id="caba9-159">Customize hello URLs used for AJAX calls</span></span>
<span data-ttu-id="caba9-160">Testre szabható URL-címeket, hogy hello Mobile Engagement webes SDK-t használ.</span><span class="sxs-lookup"><span data-stu-id="caba9-160">You can customize URLs that hello Mobile Engagement Web SDK uses.</span></span> <span data-ttu-id="caba9-161">Például tooredefine hello napló URL-címe (hello SDK végpont naplózás), hello konfigurációt lehet felülbírálni módja:</span><span class="sxs-lookup"><span data-stu-id="caba9-161">For example, tooredefine hello log URL (hello SDK endpoint for logging), you can override hello configuration like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

<span data-ttu-id="caba9-162">Ha az URL-címet adja vissza, amely kezdődik `/`, `//`, `http://`, vagy `https://`, hello alapértelmezett séma nem használatos.</span><span class="sxs-lookup"><span data-stu-id="caba9-162">If your URL functions return a string that begins with `/`, `//`, `http://`, or `https://`, hello default scheme is not used.</span></span> <span data-ttu-id="caba9-163">Alapértelmezés szerint hello `https://` sémát az URL-címeket szolgál.</span><span class="sxs-lookup"><span data-stu-id="caba9-163">By default, hello `https://` scheme is used for those URLs.</span></span> <span data-ttu-id="caba9-164">Ha azt szeretné, hogy toocustomize hello alapértelmezett séma, bírálja felül a hello konfigurálása ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="caba9-164">If you want toocustomize hello default scheme, override hello configuration, like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
