---
title: "hello JavaScript API használatával jelentéseket aaaInteract |} Microsoft Docs"
description: "A Power BI Embedded, kezelheti a jelentéseket hello JavaScript API használatával"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: bdd885d3-1b00-4dcf-bdff-531eb1f97bfb
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: 657e4d5cee031bdda173ab3f451cc19b93ddb17b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="interact-with-power-bi-reports-using-hello-javascript-api"></a><span data-ttu-id="e023d-103">Kezelheti a Power BI-jelentéseket hello JavaScript API használatával</span><span class="sxs-lookup"><span data-stu-id="e023d-103">Interact with Power BI reports using hello JavaScript API</span></span>
<span data-ttu-id="e023d-104">hello Power BI JavaScript API lehetővé teszi, hogy Ön tooeasily Power BI-jelentéseket beágyazása az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e023d-104">hello Power BI JavaScript API enables you tooeasily embed Power BI reports into your applications.</span></span> <span data-ttu-id="e023d-105">A hello API az alkalmazások programozott módon működhetnek együtt a különböző jelentési elemek, például a lapok és a szűrők.</span><span class="sxs-lookup"><span data-stu-id="e023d-105">With hello API, your applications can programmatically interact with different report elements like pages and filters.</span></span> <span data-ttu-id="e023d-106">Ez az interaktivitás a Power BI-jelentéseket az alkalmazás még szervesebb részévé teszi.</span><span class="sxs-lookup"><span data-stu-id="e023d-106">This interactivity makes Power BI reports a more integrated part of your application.</span></span>

<span data-ttu-id="e023d-107">Az alkalmazás Power BI-jelentés beágyazása iframe üzemeltetett hello alkalmazás részeként használatával.</span><span class="sxs-lookup"><span data-stu-id="e023d-107">You embed a Power BI report in your application by using an iframe that is hosted as part of hello application.</span></span> <span data-ttu-id="e023d-108">hello iframe között az alkalmazás- és hello jelentés határaként szolgálnak, mint a kép a következő hello látható.</span><span class="sxs-lookup"><span data-stu-id="e023d-108">hello iframe acts as a boundary between your application and hello report, as you can see in hello following image.</span></span> 

![A Power BI beágyazott iframe eleme JavaScript API nélkül](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-1.png)

<span data-ttu-id="e023d-110">hello JavaScript API hello nélkül jelentés és az alkalmazás nem kommunikál egymással, de hello iframe hello beágyazás folyamat sokkal könnyebb lesz.</span><span class="sxs-lookup"><span data-stu-id="e023d-110">hello iframe makes hello embedding process a lot easier, but without hello JavaScript API hello report and your application can't interact with each other.</span></span> <span data-ttu-id="e023d-111">Interakció hiánya teheti érzi hello jelentés valóban nem hello alkalmazás részét.</span><span class="sxs-lookup"><span data-stu-id="e023d-111">This lack of interaction can make it feel like hello report is not really part of hello application.</span></span> <span data-ttu-id="e023d-112">hello jelentés és az alkalmazás valóban szükség van a toocommunicate oda-vissza, úgy, ahogy a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="e023d-112">hello report and application really need toocommunicate back and forth, as in hello following image.</span></span>

![A Power BI beágyazott iframe eleme JavaScript API-val](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-2.png)

<span data-ttu-id="e023d-114">hello Power BI JavaScript API lehetővé teszi toowrite kódot, amely képes biztonságosan továbbítása hello iframe-határ.</span><span class="sxs-lookup"><span data-stu-id="e023d-114">hello Power BI JavaScript API enables you toowrite code that can securely pass through hello iframe boundary.</span></span> <span data-ttu-id="e023d-115">Ez lehetővé teszi, hogy az alkalmazás tooprogrammatically valamilyen műveletet hajt végre egy jelentést, és toolisten azon eseményeit, hello jelentésen belül felhasználók hajthatnak végre műveleteket.</span><span class="sxs-lookup"><span data-stu-id="e023d-115">This enables your application tooprogrammatically perform an action in a report, and toolisten for events from actions that users make within hello report.</span></span>

## <a name="what-can-you-do-with-hello-power-bi-javascript-api"></a><span data-ttu-id="e023d-116">Mire szolgál a Power BI JavaScript API hello?</span><span class="sxs-lookup"><span data-stu-id="e023d-116">What can you do with hello Power BI JavaScript API?</span></span>
<span data-ttu-id="e023d-117">A JavaScript API hello készült jelentések kezelése, keresse meg a jelentésekben toopages, a jelentések szűréséhez és beágyazás események kezelésére.</span><span class="sxs-lookup"><span data-stu-id="e023d-117">With hello JavaScript API you can manage reports, navigate toopages in a report, filter a report, and handle embedding events.</span></span> <span data-ttu-id="e023d-118">hello alábbi ábrán látható hello hello API struktúráját.</span><span class="sxs-lookup"><span data-stu-id="e023d-118">hello following diagram shows hello structure of hello API.</span></span>

![A Power BI JavaScript API-t bemutató ábra](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-3.png)

### <a name="manage-reports"></a><span data-ttu-id="e023d-120">Jelentések kezelése</span><span class="sxs-lookup"><span data-stu-id="e023d-120">Manage reports</span></span>
<span data-ttu-id="e023d-121">hello Javascript API lehetővé teszi hello jelentés és a lap szintjén toomanage viselkedést:</span><span class="sxs-lookup"><span data-stu-id="e023d-121">hello Javascript API enables you toomanage behavior at hello report and page level:</span></span>

* <span data-ttu-id="e023d-122">Egy adott Power BI-jelentés biztonságos beágyazása az alkalmazás - hello próbálja [bemutató alkalmazás beágyazása](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)</span><span class="sxs-lookup"><span data-stu-id="e023d-122">Embed a specific Power BI Report securely in your application - try hello [embed demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)</span></span>
  * <span data-ttu-id="e023d-123">Hozzáférési jogkivonat beállítása</span><span class="sxs-lookup"><span data-stu-id="e023d-123">Set access token</span></span>
* <span data-ttu-id="e023d-124">Hello jelentés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e023d-124">Configure hello report</span></span>
  * <span data-ttu-id="e023d-125">Engedélyezése és letiltása hello szűrő és a lap navigációs ablaktábla – hello próbálja [beállításait bemutató alkalmazás frissítése](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)</span><span class="sxs-lookup"><span data-stu-id="e023d-125">Enable and disable hello filter pane and page navigation pane - try hello [update settings demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)</span></span>
  * <span data-ttu-id="e023d-126">Lapok és a szűrők alapértelmezett értékeinek beállítása – hello próbálja [készlet alapértelmezett bemutató](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)</span><span class="sxs-lookup"><span data-stu-id="e023d-126">Set defaults for pages and filters - try hello [set defaults demo](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)</span></span>
* <span data-ttu-id="e023d-127">Teljes képernyős mód vagy kilépés a teljes képernyős módból</span><span class="sxs-lookup"><span data-stu-id="e023d-127">Enter and exit full screen mode</span></span>

[<span data-ttu-id="e023d-128">További információ jelentések beágyazásáról</span><span class="sxs-lookup"><span data-stu-id="e023d-128">Learn more about embedding a report</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)

### <a name="navigate-toopages-in-a-report"></a><span data-ttu-id="e023d-129">Keresse meg a jelentésekben toopages</span><span class="sxs-lookup"><span data-stu-id="e023d-129">Navigate toopages in a report</span></span>
<span data-ttu-id="e023d-130">hello JavaScript API enbales meg toodiscover összes lapok jelentés és tooset hello aktuális lapon.</span><span class="sxs-lookup"><span data-stu-id="e023d-130">hello JavaScript API enbales you toodiscover all pages in a report and tooset hello current page.</span></span> <span data-ttu-id="e023d-131">Próbálja meg hello [navigációs bemutató alkalmazás](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).</span><span class="sxs-lookup"><span data-stu-id="e023d-131">Try hello [navigation demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).</span></span>

[<span data-ttu-id="e023d-132">További információ a lapok közötti navigálásról</span><span class="sxs-lookup"><span data-stu-id="e023d-132">Learn more about page navigation</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a><span data-ttu-id="e023d-133">Jelentés szűrése</span><span class="sxs-lookup"><span data-stu-id="e023d-133">Filter a report</span></span>
<span data-ttu-id="e023d-134">hello JavaScript API biztosít az alapszintű és speciális szűrési lehetőségek beágyazott jelentések és jelentés oldalán.</span><span class="sxs-lookup"><span data-stu-id="e023d-134">hello JavaScript API provides basic and advanced filtering capabilities for embedded reports and report pages.</span></span> <span data-ttu-id="e023d-135">Próbálja hello [bemutató alkalmazás szűrés](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), és tekintse át az egyes bevezető kódot.</span><span class="sxs-lookup"><span data-stu-id="e023d-135">Try hello [filtering demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), and review some introductory code here.</span></span>  

#### <a name="basic-filters"></a><span data-ttu-id="e023d-136">Alapszintű szűrők</span><span class="sxs-lookup"><span data-stu-id="e023d-136">Basic filters</span></span>
<span data-ttu-id="e023d-137">Alapszintű szűrő egy oszlop vagy a hierarchia szint helyezkedik el, és értékek tooinclude vagy kizárási listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e023d-137">A basic filter is placed on a column or hierarchy level and contains a list of values tooinclude or exclude.</span></span>

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a><span data-ttu-id="e023d-138">Speciális szűrők</span><span class="sxs-lookup"><span data-stu-id="e023d-138">Advanced filters</span></span>
<span data-ttu-id="e023d-139">Speciális szűrők hello logikai operátor használata és vagy vagy, és el kell fogadnia egy vagy két feltételt, egyenként a saját operátor és értéke.</span><span class="sxs-lookup"><span data-stu-id="e023d-139">Advanced filters use hello logical operator AND or OR, and accept one or two conditions, each with their own operator and value.</span></span> <span data-ttu-id="e023d-140">Támogatott feltételek:</span><span class="sxs-lookup"><span data-stu-id="e023d-140">Supported conditions are:</span></span>

* <span data-ttu-id="e023d-141">None</span><span class="sxs-lookup"><span data-stu-id="e023d-141">None</span></span>
* <span data-ttu-id="e023d-142">LessThan</span><span class="sxs-lookup"><span data-stu-id="e023d-142">LessThan</span></span>
* <span data-ttu-id="e023d-143">LessThanOrEqual</span><span class="sxs-lookup"><span data-stu-id="e023d-143">LessThanOrEqual</span></span>
* <span data-ttu-id="e023d-144">GreaterThan</span><span class="sxs-lookup"><span data-stu-id="e023d-144">GreaterThan</span></span>
* <span data-ttu-id="e023d-145">GreaterThanOrEqual</span><span class="sxs-lookup"><span data-stu-id="e023d-145">GreaterThanOrEqual</span></span>
* <span data-ttu-id="e023d-146">Contains</span><span class="sxs-lookup"><span data-stu-id="e023d-146">Contains</span></span>
* <span data-ttu-id="e023d-147">DoesNotContain</span><span class="sxs-lookup"><span data-stu-id="e023d-147">DoesNotContain</span></span>
* <span data-ttu-id="e023d-148">StartsWith</span><span class="sxs-lookup"><span data-stu-id="e023d-148">StartsWith</span></span>
* <span data-ttu-id="e023d-149">DoesNotStartWith</span><span class="sxs-lookup"><span data-stu-id="e023d-149">DoesNotStartWith</span></span>
* <span data-ttu-id="e023d-150">Is</span><span class="sxs-lookup"><span data-stu-id="e023d-150">Is</span></span>
* <span data-ttu-id="e023d-151">IsNot</span><span class="sxs-lookup"><span data-stu-id="e023d-151">IsNot</span></span>
* <span data-ttu-id="e023d-152">IsBlank</span><span class="sxs-lookup"><span data-stu-id="e023d-152">IsBlank</span></span>
* <span data-ttu-id="e023d-153">IsNotBlank</span><span class="sxs-lookup"><span data-stu-id="e023d-153">IsNotBlank</span></span>

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[<span data-ttu-id="e023d-154">További információk a szűrésről</span><span class="sxs-lookup"><span data-stu-id="e023d-154">Learn more about filtering</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)

### <a name="handling-events"></a><span data-ttu-id="e023d-155">Események kezelése</span><span class="sxs-lookup"><span data-stu-id="e023d-155">Handling events</span></span>
<span data-ttu-id="e023d-156">Ezenkívül hello iframe toosending adatait, az alkalmazás akkor is jelentkezhet hello hello iframe érkező események a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="e023d-156">In addition toosending information into hello iframe, your application can also receive information on hello following events coming from hello iframe:</span></span>

* <span data-ttu-id="e023d-157">Beágyazás</span><span class="sxs-lookup"><span data-stu-id="e023d-157">Embed</span></span>
  * <span data-ttu-id="e023d-158">loaded</span><span class="sxs-lookup"><span data-stu-id="e023d-158">loaded</span></span>
  * <span data-ttu-id="e023d-159">error</span><span class="sxs-lookup"><span data-stu-id="e023d-159">error</span></span>
* <span data-ttu-id="e023d-160">Jelentések</span><span class="sxs-lookup"><span data-stu-id="e023d-160">Reports</span></span>
  * <span data-ttu-id="e023d-161">pageChanged</span><span class="sxs-lookup"><span data-stu-id="e023d-161">pageChanged</span></span>
  * <span data-ttu-id="e023d-162">dataSelected (hamarosan)</span><span class="sxs-lookup"><span data-stu-id="e023d-162">dataSelected (coming soon)</span></span>

[<span data-ttu-id="e023d-163">További tudnivalók az események kezeléséről</span><span class="sxs-lookup"><span data-stu-id="e023d-163">Learn more about handling events</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)

## <a name="next-steps"></a><span data-ttu-id="e023d-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e023d-164">Next steps</span></span>
<span data-ttu-id="e023d-165">A Power BI JavaScript API hello kapcsolatos további információkért tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="e023d-165">For more information about hello Power BI JavaScript API, check out hello following links:</span></span>

* [<span data-ttu-id="e023d-166">JavaScript API wiki</span><span class="sxs-lookup"><span data-stu-id="e023d-166">JavaScript API Wiki</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
* [<span data-ttu-id="e023d-167">Objektummodell-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="e023d-167">Object model reference</span></span>](https://microsoft.github.io/powerbi-models/modules/_models_.html)
* <span data-ttu-id="e023d-168">Példák</span><span class="sxs-lookup"><span data-stu-id="e023d-168">Samples</span></span>
  * [<span data-ttu-id="e023d-169">Angular</span><span class="sxs-lookup"><span data-stu-id="e023d-169">Angular</span></span>](http://azure-samples.github.io/powerbi-angular-client)
  * [<span data-ttu-id="e023d-170">Ember</span><span class="sxs-lookup"><span data-stu-id="e023d-170">Ember</span></span>](https://github.com/Microsoft/powerbi-ember)
* [<span data-ttu-id="e023d-171">Élő bemutató</span><span class="sxs-lookup"><span data-stu-id="e023d-171">Live demo</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)

