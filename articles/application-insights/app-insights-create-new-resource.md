---
title: "Hozzon létre egy új Azure Application Insights-erőforrást |} Microsoft Docs"
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
ms.openlocfilehash: 5f8814ee943424c1c278ab3732129d4459f83819
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-insights-resource"></a><span data-ttu-id="1a135-103">Application Insights-erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a135-103">Create an Application Insights resource</span></span>
<span data-ttu-id="1a135-104">Azure Application Insights az alkalmazással kapcsolatos adatokat jeleníti meg a Microsoft Azure *erőforrás*.</span><span class="sxs-lookup"><span data-stu-id="1a135-104">Azure Application Insights displays data about your application in a Microsoft Azure *resource*.</span></span> <span data-ttu-id="1a135-105">Új erőforrás létrehozása része ezért [Application Insights beállítása egy új alkalmazás figyelésére][start].</span><span class="sxs-lookup"><span data-stu-id="1a135-105">Creating a new resource is therefore part of [setting up Application Insights to monitor a new application][start].</span></span> <span data-ttu-id="1a135-106">Sok esetben erőforrás létrehozása automatikusan úgy teheti az IDE.</span><span class="sxs-lookup"><span data-stu-id="1a135-106">In many cases, creating a resource can be done automatically by the IDE.</span></span> <span data-ttu-id="1a135-107">De néhány esetben hoz létre egy erőforrás manuálisan - például számára elkülönített erőforrások fejlesztési és éles az alkalmazás létrehozza.</span><span class="sxs-lookup"><span data-stu-id="1a135-107">But in some cases, you create a resource manually - for example, to have separate resources for development and production builds of your application.</span></span>

<span data-ttu-id="1a135-108">Az erőforrás létrehozása után annak instrumentation kulcs lekérése, és azt használja az alkalmazás az SDK konfigurálására.</span><span class="sxs-lookup"><span data-stu-id="1a135-108">After you have created the resource, you get its instrumentation key and use that to configure the SDK in the application.</span></span> <span data-ttu-id="1a135-109">Az erőforráskulcs telemetriai adatok az erőforrás hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1a135-109">The resource key links the telemetry to the resource.</span></span>

## <a name="sign-up-to-microsoft-azure"></a><span data-ttu-id="1a135-110">Jelentkezzen a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="1a135-110">Sign up to Microsoft Azure</span></span>
<span data-ttu-id="1a135-111">Ha még nem kapott egy [Microsoft fiókot, egy letöltése](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="1a135-111">If you haven't got a [Microsoft account, get one now](http://live.com).</span></span> <span data-ttu-id="1a135-112">(Például Outlook.com, OneDrive, Windows Phone vagy XBox Live-szolgáltatást használ, ha már rendelkezik Microsoft-fiók.)</span><span class="sxs-lookup"><span data-stu-id="1a135-112">(If you use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, you already have a Microsoft account.)</span></span>

<span data-ttu-id="1a135-113">Emellett szükség van egy előfizetés [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="1a135-113">You also need a subscription to [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="1a135-114">Ha a csapat vagy szervezet Azure-előfizetéssel, a tulajdonos adhat hozzá, a Windows Live ID azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="1a135-114">If your team or organization has an Azure subscription, the owner can add you to it, using your Windows Live ID.</span></span> <span data-ttu-id="1a135-115">Most csak felszámított a valóban használt funkciókért.</span><span class="sxs-lookup"><span data-stu-id="1a135-115">You're only charged for what you use.</span></span> <span data-ttu-id="1a135-116">Az alapértelmezett alapszintű csomag lehetővé teszi a kísérleti díjmentesen használható bizonyos mennyiségű.</span><span class="sxs-lookup"><span data-stu-id="1a135-116">The default basic plan allows for a certain amount of experimental use free of charge.</span></span>

<span data-ttu-id="1a135-117">Előfizetés hozzáférést van, amikor jelentkezzen be az Application Insights részére, [http://portal.azure.com](https://portal.azure.com), és a Live ID bejelentkezési használja.</span><span class="sxs-lookup"><span data-stu-id="1a135-117">When you've got access to a subscription, log in to Application Insights at [http://portal.azure.com](https://portal.azure.com), and use your Live ID to login.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="1a135-118">Application Insights-erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a135-118">Create an Application Insights resource</span></span>
<span data-ttu-id="1a135-119">Az a [portal.azure.com](https://portal.azure.com), vegyen fel egy Application Insights-erőforrást:</span><span class="sxs-lookup"><span data-stu-id="1a135-119">In the [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Kattintson az Új, majd az Application Insights lehetőségre](./media/app-insights-create-new-resource/01-new.png)

* <span data-ttu-id="1a135-121">**Az alkalmazástípus** elemnél mi jelenik a áttekintése panel megnyitásához, és a tulajdonságok érhetők el a [metrika explorer][metrics].</span><span class="sxs-lookup"><span data-stu-id="1a135-121">**Application type** affects what you see on the overview blade and the properties available in [metric explorer][metrics].</span></span> <span data-ttu-id="1a135-122">Ha nem látja a típusú alkalmazást, válassza az általános.</span><span class="sxs-lookup"><span data-stu-id="1a135-122">If you don't see your type of app, choose General.</span></span>
* <span data-ttu-id="1a135-123">**Előfizetés** a fizetési fiók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="1a135-123">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="1a135-124">**Erőforráscsoport** a könnyebb tulajdonságainak kezelésére van, például hozzáférés-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="1a135-124">**Resource group** is a convenience for managing properties like access control.</span></span> <span data-ttu-id="1a135-125">Ha már létrehozott más Azure-erőforrások, válassza ki az új erőforrás helyezze ugyanabba a csoportba.</span><span class="sxs-lookup"><span data-stu-id="1a135-125">If you have already created other Azure resources, you can choose to put this new resource in the same group.</span></span>
* <span data-ttu-id="1a135-126">**Hely** van, ahol azt megőrizni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="1a135-126">**Location** is where we keep your data.</span></span>
* <span data-ttu-id="1a135-127">**Rögzítés az irányítópulton** helyezi az erőforrás egy gyors elérést csempe az Azure kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="1a135-127">**Pin to dashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> <span data-ttu-id="1a135-128">Ajánlott.</span><span class="sxs-lookup"><span data-stu-id="1a135-128">Recommended.</span></span>

<span data-ttu-id="1a135-129">Ha az alkalmazás létrehozása után egy új panelen nyitja meg.</span><span class="sxs-lookup"><span data-stu-id="1a135-129">When your app has been created, a new blade opens.</span></span> <span data-ttu-id="1a135-130">Ezen a panelen, ahol látható teljesítmény- és használati adatokat az alkalmazásra vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="1a135-130">This blade is where you see performance and usage data about your app.</span></span> 

<span data-ttu-id="1a135-131">Kattintva visszatérhet, amikor legközelebb bejelentkezik Azure, keresse meg az alkalmazás gyors üzembe helyezési csempe a start táblán (kezdőképernyő).</span><span class="sxs-lookup"><span data-stu-id="1a135-131">To get back to it next time you log in to Azure, look for your app's quick-start tile on the start board (home screen).</span></span> <span data-ttu-id="1a135-132">Vagy kattintson a Tallózás gombra, és keresse meg.</span><span class="sxs-lookup"><span data-stu-id="1a135-132">Or click Browse to find it.</span></span>

## <a name="copy-the-instrumentation-key"></a><span data-ttu-id="1a135-133">A rendszerállapot-kulcs másolása</span><span class="sxs-lookup"><span data-stu-id="1a135-133">Copy the instrumentation key</span></span>
<span data-ttu-id="1a135-134">A instrumentation kulcs azonosítja az erőforrás, amelyet létrehozott.</span><span class="sxs-lookup"><span data-stu-id="1a135-134">The instrumentation key identifies the resource that you created.</span></span> <span data-ttu-id="1a135-135">Esetleg szükség lenne rá, hogy biztosítsa az SDK-t a.</span><span class="sxs-lookup"><span data-stu-id="1a135-135">You need it to give to the SDK.</span></span>

![Essentials kattintson, majd a Instrumentation kulcsot, a CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a><span data-ttu-id="1a135-137">Az alkalmazás az SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="1a135-137">Install the SDK in your app</span></span>
<span data-ttu-id="1a135-138">Telepítse az Application Insights SDK az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1a135-138">Install the Application Insights SDK in your app.</span></span> <span data-ttu-id="1a135-139">Ezt a lépést az alkalmazás fokozottan függ.</span><span class="sxs-lookup"><span data-stu-id="1a135-139">This step depends heavily on the type of your application.</span></span> 

<span data-ttu-id="1a135-140">A rendszerállapot-kulcsot használ konfigurálása [az SDK-t, hogy az alkalmazás telepítése][start].</span><span class="sxs-lookup"><span data-stu-id="1a135-140">Use the instrumentation key to configure [the SDK that you install in your application][start].</span></span>

<span data-ttu-id="1a135-141">Az SDK magában foglalja a telemetriai adatokat küldhet anélkül, hogy a kód írása modulban.</span><span class="sxs-lookup"><span data-stu-id="1a135-141">The SDK includes standard modules that send telemetry without you having to write any code.</span></span> <span data-ttu-id="1a135-142">Nyomon követheti a felhasználói műveletek vagy eseményadatokat részletesen, [API-t használó] [ api] saját telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="1a135-142">To track user actions or diagnose issues in more detail, [use the API][api] to send your own telemetry.</span></span>

## <span data-ttu-id="1a135-143"><a name="monitor"></a>Lásd: a telemetriai adatok</span><span class="sxs-lookup"><span data-stu-id="1a135-143"><a name="monitor"></a>See telemetry data</span></span>
<span data-ttu-id="1a135-144">Zárja be a gyors üzembe helyezési panel az alkalmazás panel az Azure-portálon való visszatéréshez.</span><span class="sxs-lookup"><span data-stu-id="1a135-144">Close the quick start blade to return to your application blade in the Azure portal.</span></span>

<span data-ttu-id="1a135-145">Kattintson a keresés csempe megtekintéséhez [diagnosztikai keresési][diagnostic], ahol az első események jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="1a135-145">Click the Search tile to see [Diagnostic Search][diagnostic], where the first events appear.</span></span> 

<span data-ttu-id="1a135-146">Ha több adatot várt, kattintson a **frissítése** néhány másodperc múlva.</span><span class="sxs-lookup"><span data-stu-id="1a135-146">If you're expecting more data, click **Refresh** after a few seconds  .</span></span>

## <a name="creating-a-resource-automatically"></a><span data-ttu-id="1a135-147">Erőforrás automatikus létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a135-147">Creating a resource automatically</span></span>
<span data-ttu-id="1a135-148">Írhat egy [PowerShell-parancsfájl](app-insights-powershell.md) erőforrás automatikus létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1a135-148">You can write a [PowerShell script](app-insights-powershell.md) to create a resource automatically.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a135-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1a135-149">Next steps</span></span>
* [<span data-ttu-id="1a135-150">Irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a135-150">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="1a135-151">Diagnosztikai keresés</span><span class="sxs-lookup"><span data-stu-id="1a135-151">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="1a135-152">Metrikák böngészése</span><span class="sxs-lookup"><span data-stu-id="1a135-152">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="1a135-153">Analytics-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="1a135-153">Write Analytics queries</span></span>](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

