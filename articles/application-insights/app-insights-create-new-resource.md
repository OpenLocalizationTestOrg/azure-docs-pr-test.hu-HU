---
title: "egy új Azure Application Insights-erőforrás aaaCreate |} Microsoft Docs"
description: "Manuálisan állítsa be az Application Insights egy új élő alkalmazás figyelését."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: bwren
ms.openlocfilehash: 3aba7045e1f8fe43d473f0be01dd52106ab992a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-insights-resource"></a><span data-ttu-id="e339b-103">Application Insights-erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e339b-103">Create an Application Insights resource</span></span>
<span data-ttu-id="e339b-104">Azure Application Insights az alkalmazással kapcsolatos adatokat jeleníti meg a Microsoft Azure *erőforrás*.</span><span class="sxs-lookup"><span data-stu-id="e339b-104">Azure Application Insights displays data about your application in a Microsoft Azure *resource*.</span></span> <span data-ttu-id="e339b-105">Új erőforrás létrehozása része ezért [beállítása az Application Insights toomonitor egy új alkalmazás][start].</span><span class="sxs-lookup"><span data-stu-id="e339b-105">Creating a new resource is therefore part of [setting up Application Insights toomonitor a new application][start].</span></span> <span data-ttu-id="e339b-106">Sok esetben erőforrás létrehozása automatikusan úgy teheti hello IDE.</span><span class="sxs-lookup"><span data-stu-id="e339b-106">In many cases, creating a resource can be done automatically by hello IDE.</span></span> <span data-ttu-id="e339b-107">De néhány esetben erőforrás-létrehozhat egy manuálisan – például külön erőforrások toohave fejlesztési és éles az alkalmazás létrehozza.</span><span class="sxs-lookup"><span data-stu-id="e339b-107">But in some cases, you create a resource manually - for example, toohave separate resources for development and production builds of your application.</span></span>

<span data-ttu-id="e339b-108">Hello erőforrás létrehozása után annak instrumentation kulcs lekérése, és adott tooconfigure hello SDK hello alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="e339b-108">After you have created hello resource, you get its instrumentation key and use that tooconfigure hello SDK in hello application.</span></span> <span data-ttu-id="e339b-109">hello erőforrás kulcs hivatkozások hello telemetriai toohello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="e339b-109">hello resource key links hello telemetry toohello resource.</span></span>

## <a name="sign-up-toomicrosoft-azure"></a><span data-ttu-id="e339b-110">Regisztráljon az Azure tooMicrosoft</span><span class="sxs-lookup"><span data-stu-id="e339b-110">Sign up tooMicrosoft Azure</span></span>
<span data-ttu-id="e339b-111">Ha még nem kapott egy [Microsoft fiókot, egy letöltése](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="e339b-111">If you haven't got a [Microsoft account, get one now](http://live.com).</span></span> <span data-ttu-id="e339b-112">(Például Outlook.com, OneDrive, Windows Phone vagy XBox Live-szolgáltatást használ, ha már rendelkezik Microsoft-fiók.)</span><span class="sxs-lookup"><span data-stu-id="e339b-112">(If you use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, you already have a Microsoft account.)</span></span>

<span data-ttu-id="e339b-113">Emellett szükség van egy előfizetési túl[Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="e339b-113">You also need a subscription too[Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="e339b-114">Ha a csapat vagy szervezet Azure-előfizetéssel, hello tulajdonosa adhat hozzá, tooit, a Windows Live ID azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="e339b-114">If your team or organization has an Azure subscription, hello owner can add you tooit, using your Windows Live ID.</span></span> <span data-ttu-id="e339b-115">Most csak felszámított a valóban használt funkciókért.</span><span class="sxs-lookup"><span data-stu-id="e339b-115">You're only charged for what you use.</span></span> <span data-ttu-id="e339b-116">hello alapértelmezett alapszintű kísérleti díjmentesen használható bizonyos mennyiségű teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="e339b-116">hello default basic plan allows for a certain amount of experimental use free of charge.</span></span>

<span data-ttu-id="e339b-117">Amikor hozzáférési tooa előfizetése van, jelentkezzen be, tooApplication Insights [http://portal.azure.com](https://portal.azure.com), és a Live ID toologin használja.</span><span class="sxs-lookup"><span data-stu-id="e339b-117">When you've got access tooa subscription, log in tooApplication Insights at [http://portal.azure.com](https://portal.azure.com), and use your Live ID toologin.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="e339b-118">Application Insights-erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e339b-118">Create an Application Insights resource</span></span>
<span data-ttu-id="e339b-119">A hello [portal.azure.com](https://portal.azure.com), vegyen fel egy Application Insights-erőforrást:</span><span class="sxs-lookup"><span data-stu-id="e339b-119">In hello [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Kattintson az Új, majd az Application Insights lehetőségre](./media/app-insights-create-new-resource/01-new.png)

* <span data-ttu-id="e339b-121">**Az alkalmazástípus** elemnél mi jelenik hello áttekintése panel és hello tulajdonságok érhetők el [metrika explorer][metrics].</span><span class="sxs-lookup"><span data-stu-id="e339b-121">**Application type** affects what you see on hello overview blade and hello properties available in [metric explorer][metrics].</span></span> <span data-ttu-id="e339b-122">Ha nem látja a típusú alkalmazást, válassza az általános.</span><span class="sxs-lookup"><span data-stu-id="e339b-122">If you don't see your type of app, choose General.</span></span>
* <span data-ttu-id="e339b-123">**Előfizetés** a fizetési fiók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e339b-123">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="e339b-124">**Erőforráscsoport** a könnyebb tulajdonságainak kezelésére van, például hozzáférés-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="e339b-124">**Resource group** is a convenience for managing properties like access control.</span></span> <span data-ttu-id="e339b-125">Ha már létrehozott más Azure-erőforrások, választhatja ki tooput az új erőforrás hello ugyanabban a csoportban.</span><span class="sxs-lookup"><span data-stu-id="e339b-125">If you have already created other Azure resources, you can choose tooput this new resource in hello same group.</span></span>
* <span data-ttu-id="e339b-126">**Hely** van, ahol azt megőrizni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="e339b-126">**Location** is where we keep your data.</span></span>
* <span data-ttu-id="e339b-127">**PIN-kód toodashboard** helyezi az erőforrás egy gyors elérést csempe az Azure kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="e339b-127">**Pin toodashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> <span data-ttu-id="e339b-128">Ajánlott.</span><span class="sxs-lookup"><span data-stu-id="e339b-128">Recommended.</span></span>

<span data-ttu-id="e339b-129">Ha az alkalmazás létrehozása után egy új panelen nyitja meg.</span><span class="sxs-lookup"><span data-stu-id="e339b-129">When your app has been created, a new blade opens.</span></span> <span data-ttu-id="e339b-130">Ezen a panelen, ahol látható teljesítmény- és használati adatokat az alkalmazásra vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="e339b-130">This blade is where you see performance and usage data about your app.</span></span> 

<span data-ttu-id="e339b-131">tooget hátsó tooit következő bejelentkezéskor tooAzure, keresse meg az alkalmazás gyors üzembe helyezési csempe hello indítsa el board (kezdőképernyő).</span><span class="sxs-lookup"><span data-stu-id="e339b-131">tooget back tooit next time you log in tooAzure, look for your app's quick-start tile on hello start board (home screen).</span></span> <span data-ttu-id="e339b-132">Vagy kattintson a Tallózás toofind azt.</span><span class="sxs-lookup"><span data-stu-id="e339b-132">Or click Browse toofind it.</span></span>

## <a name="copy-hello-instrumentation-key"></a><span data-ttu-id="e339b-133">Hello instrumentation kulcs másolása</span><span class="sxs-lookup"><span data-stu-id="e339b-133">Copy hello instrumentation key</span></span>
<span data-ttu-id="e339b-134">hello instrumentation kulcs hello erőforrás létrehozott azonosítja.</span><span class="sxs-lookup"><span data-stu-id="e339b-134">hello instrumentation key identifies hello resource that you created.</span></span> <span data-ttu-id="e339b-135">Esetleg szükség lenne rá toogive toohello SDK.</span><span class="sxs-lookup"><span data-stu-id="e339b-135">You need it toogive toohello SDK.</span></span>

![Essentials kattintson, majd hello Instrumentation kulcs CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-hello-sdk-in-your-app"></a><span data-ttu-id="e339b-137">Az alkalmazás hello SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="e339b-137">Install hello SDK in your app</span></span>
<span data-ttu-id="e339b-138">Hello Application Insights SDK telepítése az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e339b-138">Install hello Application Insights SDK in your app.</span></span> <span data-ttu-id="e339b-139">Ezt a lépést az alkalmazás hello típusú fokozottan függ.</span><span class="sxs-lookup"><span data-stu-id="e339b-139">This step depends heavily on hello type of your application.</span></span> 

<span data-ttu-id="e339b-140">Használja a hello instrumentation kulcs tooconfigure [, amely az alkalmazás telepítése SDK hello][start].</span><span class="sxs-lookup"><span data-stu-id="e339b-140">Use hello instrumentation key tooconfigure [hello SDK that you install in your application][start].</span></span>

<span data-ttu-id="e339b-141">hello SDK magában foglalja a telemetriai adatokat küldhet anélkül, hogy toowrite kódok modulban.</span><span class="sxs-lookup"><span data-stu-id="e339b-141">hello SDK includes standard modules that send telemetry without you having toowrite any code.</span></span> <span data-ttu-id="e339b-142">tootrack felhasználói műveletek vagy eseményadatokat részletesen, [hello API-t használó] [ api] toosend saját telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="e339b-142">tootrack user actions or diagnose issues in more detail, [use hello API][api] toosend your own telemetry.</span></span>

## <span data-ttu-id="e339b-143"><a name="monitor"></a>Lásd: a telemetriai adatok</span><span class="sxs-lookup"><span data-stu-id="e339b-143"><a name="monitor"></a>See telemetry data</span></span>
<span data-ttu-id="e339b-144">Gyors Bezárás hello hello Azure-portál panel tooreturn tooyour alkalmazás panel elindítása.</span><span class="sxs-lookup"><span data-stu-id="e339b-144">Close hello quick start blade tooreturn tooyour application blade in hello Azure portal.</span></span>

<span data-ttu-id="e339b-145">Kattintson a hello keresési csempe toosee [diagnosztikai keresési][diagnostic], ahol a hello első események jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="e339b-145">Click hello Search tile toosee [Diagnostic Search][diagnostic], where hello first events appear.</span></span> 

<span data-ttu-id="e339b-146">Ha több adatot várt, kattintson a **frissítése** néhány másodperc múlva.</span><span class="sxs-lookup"><span data-stu-id="e339b-146">If you're expecting more data, click **Refresh** after a few seconds  .</span></span>

## <a name="creating-a-resource-automatically"></a><span data-ttu-id="e339b-147">Erőforrás automatikus létrehozása</span><span class="sxs-lookup"><span data-stu-id="e339b-147">Creating a resource automatically</span></span>
<span data-ttu-id="e339b-148">Írhat egy [PowerShell-parancsfájl](app-insights-powershell.md) toocreate erőforrás automatikusan.</span><span class="sxs-lookup"><span data-stu-id="e339b-148">You can write a [PowerShell script](app-insights-powershell.md) toocreate a resource automatically.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e339b-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e339b-149">Next steps</span></span>
* [<span data-ttu-id="e339b-150">Irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="e339b-150">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="e339b-151">Diagnosztikai keresés</span><span class="sxs-lookup"><span data-stu-id="e339b-151">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="e339b-152">Metrikák böngészése</span><span class="sxs-lookup"><span data-stu-id="e339b-152">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="e339b-153">Analytics-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="e339b-153">Write Analytics queries</span></span>](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

