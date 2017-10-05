---
title: "Microsoft Power BI Embedded – egy adatforráshoz való kapcsolódáshoz"
description: "A Power BI Embedded, kapcsolódás az adatforrásokhoz"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: 9f614bbc63eae788aa52132c8f0e42ad8963559a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-a-data-source"></a><span data-ttu-id="8ba12-103">Kapcsolódás adatforráshoz</span><span class="sxs-lookup"><span data-stu-id="8ba12-103">Connect to a data source</span></span>
<span data-ttu-id="8ba12-104">A **Power BI Embedded**, jelentés beágyazása az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="8ba12-104">With **Power BI Embedded**, you can embed reports into your own app.</span></span> <span data-ttu-id="8ba12-105">A Power BI-jelentés beágyazása az alkalmazásba, a jelentés csatlakozik-e az alapul szolgáló adatokat által **importálása** egy másolatot, az adatok vagy a **közvetlenül csatlakozó** a forrás-történő **DirectQuery** .</span><span class="sxs-lookup"><span data-stu-id="8ba12-105">When you embed a Power BI report into your app, the report connects to the underlying data by **importing** a copy of the data or by **connecting directly** to the data source using **DirectQuery**.</span></span>

<span data-ttu-id="8ba12-106">Az alábbiakban a két módszer (**importálás** és **DirectQuery**) közti különbségeket láthatja.</span><span class="sxs-lookup"><span data-stu-id="8ba12-106">Here are the differences between using **Import** and **DirectQuery**.</span></span>

| <span data-ttu-id="8ba12-107">Importálás</span><span class="sxs-lookup"><span data-stu-id="8ba12-107">Import</span></span> | <span data-ttu-id="8ba12-108">DirectQuery</span><span class="sxs-lookup"><span data-stu-id="8ba12-108">DirectQuery</span></span> |
| --- | --- |
| <span data-ttu-id="8ba12-109">A táblákat, oszlopokat *és az adatok* importálhatja vagy másolhatja be a jelentésben adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="8ba12-109">Tables, columns, *and data* are imported or copied into the report's dataset.</span></span> <span data-ttu-id="8ba12-110">A mögöttes adatok módosulásainak megtekintéséhez frissítse, vagy importáljon egy teljes aktuális adatkészletet újra.</span><span class="sxs-lookup"><span data-stu-id="8ba12-110">To see changes that occurred to the underlying data, you must refresh, or import, a complete, current dataset again.</span></span> |<span data-ttu-id="8ba12-111">Csak *táblákat és oszlopokat* importálhatja vagy másolhatja be a jelentésben adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="8ba12-111">Only *tables and columns* are imported or copied into the report's dataset.</span></span> <span data-ttu-id="8ba12-112">Mindig a legfrissebb adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="8ba12-112">You always view the most current data.</span></span> |

<span data-ttu-id="8ba12-113">Power BI Embedded akkor felhő adatforrások DirectQuery használható, de nem a helyi adatforrások jelenleg.</span><span class="sxs-lookup"><span data-stu-id="8ba12-113">With Power BI Embedded, you can use DirectQuery with cloud data sources but not on-premises data sources at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="8ba12-114">Az On-Premises átjáró jelenleg nem támogatott a Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="8ba12-114">The On-Premises Data Gateway is not supported with Power BI Embedded at this time.</span></span> <span data-ttu-id="8ba12-115">Ez azt jelenti, hogy a helyszíni adatforrások DirectQuery nem használhat.</span><span class="sxs-lookup"><span data-stu-id="8ba12-115">This means you cannot use DirectQuery with on-premises data sources.</span></span>

## <a name="supported-data-sources"></a><span data-ttu-id="8ba12-116">Támogatott adatforrások</span><span class="sxs-lookup"><span data-stu-id="8ba12-116">Supported data sources</span></span>

<span data-ttu-id="8ba12-117">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="8ba12-117">**DirectQuery**</span></span>
* <span data-ttu-id="8ba12-118">Az Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="8ba12-118">Azure SQL database</span></span>
* <span data-ttu-id="8ba12-119">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="8ba12-119">Azure SQL Data Warehouse</span></span>

<span data-ttu-id="8ba12-120">**Importálás**</span><span class="sxs-lookup"><span data-stu-id="8ba12-120">**Import**</span></span>

<span data-ttu-id="8ba12-121">Az összes az elérhető adatforrásokhoz Power BI Desktop segítségével importálhatja.</span><span class="sxs-lookup"><span data-stu-id="8ba12-121">You can import using all of the available data sources within Power BI Desktop.</span></span> <span data-ttu-id="8ba12-122">Lehetővé teszi a **nem** kell lehet csatlakozni a Power BI Embedded belül.</span><span class="sxs-lookup"><span data-stu-id="8ba12-122">You will **not** be able to refresh that data within Power BI Embedded.</span></span> <span data-ttu-id="8ba12-123">A PBIX-fájl a Power BI Embedded feltölteni a módosításokat fog.</span><span class="sxs-lookup"><span data-stu-id="8ba12-123">You will have to upload changes to your PBIX file to Power BI Embedded.</span></span> <span data-ttu-id="8ba12-124">Rendelkezésre álló átjáró okozza.</span><span class="sxs-lookup"><span data-stu-id="8ba12-124">This is due to no available gateway.</span></span> 

## <a name="benefits-of-using-directquery"></a><span data-ttu-id="8ba12-125">DirectQuery használatának előnyei</span><span class="sxs-lookup"><span data-stu-id="8ba12-125">Benefits of using DirectQuery</span></span>
<span data-ttu-id="8ba12-126">Nincsenek két elsődleges előnnyel jár, használatakor **DirectQuery**:</span><span class="sxs-lookup"><span data-stu-id="8ba12-126">There are two primary benefits when using **DirectQuery**:</span></span>

* <span data-ttu-id="8ba12-127">**DirectQuery** is létrehozhat képi megjelenítések alatt a nagy adatkészleteknél, ahol egyébként lenne, amelyeknek első importálandó összes adatot.</span><span class="sxs-lookup"><span data-stu-id="8ba12-127">**DirectQuery** lets you build visualizations over very large datasets, where it otherwise would be unfeasible to first import all of the data.</span></span>
* <span data-ttu-id="8ba12-128">Az alapul szolgáló adatváltozásokat lehet szükség, adatok frissítését, és az egyes jelentések aktuális adatok megjelenítéséhez szükség lehet szükség nagy adatátvitelt, és amelyeknek újraimportálásának adatok.</span><span class="sxs-lookup"><span data-stu-id="8ba12-128">Underlying data changes can require a refresh of data, and for some reports, the need to display current data can require large data transfers, making re-importing data unfeasible.</span></span> <span data-ttu-id="8ba12-129">Ezzel szemben **DirectQuery** jelentések mindig aktuális adatokat használják.</span><span class="sxs-lookup"><span data-stu-id="8ba12-129">By contrast, **DirectQuery** reports always use current data.</span></span>

## <a name="limitations-of-directquery"></a><span data-ttu-id="8ba12-130">DirectQuery korlátozásai</span><span class="sxs-lookup"><span data-stu-id="8ba12-130">Limitations of DirectQuery</span></span>
   <span data-ttu-id="8ba12-131">Néhány korlátozást érdemes használatával **DirectQuery**:</span><span class="sxs-lookup"><span data-stu-id="8ba12-131">There are a few limitations to using **DirectQuery**:</span></span>

* <span data-ttu-id="8ba12-132">Minden táblának egyetlen adatbázisra kell származnia.</span><span class="sxs-lookup"><span data-stu-id="8ba12-132">All tables must come from a single database.</span></span>
* <span data-ttu-id="8ba12-133">Ha a lekérdezés túl összetett, hiba történik.</span><span class="sxs-lookup"><span data-stu-id="8ba12-133">If the query is overly complex, an error will occur.</span></span> <span data-ttu-id="8ba12-134">A hiba elhárításához a lekérdezést, hogy kevésbé összetett kell refactor.</span><span class="sxs-lookup"><span data-stu-id="8ba12-134">To remedy the error you must refactor the query so it is less complex.</span></span> <span data-ttu-id="8ba12-135">A lekérdezés összetett kell lennie, ha szüksége lesz az adatimportálás használata helyett **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="8ba12-135">If the query must be complex, you will need to import the data instead of using **DirectQuery**.</span></span>
* <span data-ttu-id="8ba12-136">Kapcsolat szűrés csak egy irányban ahelyett, hogy mindkét irányban korlátozva.</span><span class="sxs-lookup"><span data-stu-id="8ba12-136">Relationship filtering is limited to a single direction, rather than both directions.</span></span>
* <span data-ttu-id="8ba12-137">Egy oszlop adattípusa nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="8ba12-137">You cannot change the data type of a column.</span></span>
* <span data-ttu-id="8ba12-138">Alapértelmezés szerint engedélyezett a mértékek DAX-kifejezések korlátozások kerülnek.</span><span class="sxs-lookup"><span data-stu-id="8ba12-138">By default, limitations are placed on DAX expressions allowed in measures.</span></span> <span data-ttu-id="8ba12-139">Lásd: [DirectQuery és intézkedések](#measures).</span><span class="sxs-lookup"><span data-stu-id="8ba12-139">See [DirectQuery and measures](#measures).</span></span>

<a name="measures"/>

## <a name="directquery-and-measures"></a><span data-ttu-id="8ba12-140">DirectQuery és intézkedések</span><span class="sxs-lookup"><span data-stu-id="8ba12-140">DirectQuery and measures</span></span>
<span data-ttu-id="8ba12-141">Az alapul szolgáló adatforrásban küldött lekérdezések rendelkezik a megfelelő teljesítmény érdekében az korlátozások intézkedések a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="8ba12-141">To ensure queries sent to the underlying data source have acceptable performance, limitations are imposed on measures.</span></span> <span data-ttu-id="8ba12-142">Használata esetén **Power BI Desktop**, speciális felhasználók engedélyezze a Ez a korlátozás kiválasztásával **fájl > lehetőségek és beállítások > Beállítások**.</span><span class="sxs-lookup"><span data-stu-id="8ba12-142">When using **Power BI Desktop**, advanced users can choose to bypass this limitation by choosing **File > Options and settings > Options**.</span></span> <span data-ttu-id="8ba12-143">Az a **beállítások** párbeszédpanelen válasszon **DirectQuery**, válassza ki, **nem korlátozott mértékek engedélyezése DirectQuery módban**.</span><span class="sxs-lookup"><span data-stu-id="8ba12-143">In the **Options** dialog, choose **DirectQuery**, and select the option **Allow unrestricted measures in DirectQuery mode**.</span></span> <span data-ttu-id="8ba12-144">Ez a lehetőség van kiválasztva, ha bármely mérték érvényes DAX-kifejezés használható.</span><span class="sxs-lookup"><span data-stu-id="8ba12-144">When that option is selected, any DAX expression that is valid for a measure can be used.</span></span> <span data-ttu-id="8ba12-145">Lehet, hogy a felhasználók tisztában; azonban, hogy néhány kifejezés, amely jól végre, ha az adatok importálása azt eredményezheti, nagyon lassú lekérdezi a háttérrendszer forrás, amikor a **DirectQuery** mód.</span><span class="sxs-lookup"><span data-stu-id="8ba12-145">Users must be aware; however, that some expressions that perform very well when the data is imported may result in very slow queries to the backend source when in **DirectQuery** mode.</span></span> 

## <a name="see-also"></a><span data-ttu-id="8ba12-146">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8ba12-146">See Also</span></span>
* [<span data-ttu-id="8ba12-147">Ismerkedés a Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="8ba12-147">Get started with Microsoft Power BI Embedded</span></span>](power-bi-embedded-get-started.md)
* [<span data-ttu-id="8ba12-148">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="8ba12-148">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

<span data-ttu-id="8ba12-149">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="8ba12-149">More questions?</span></span> [<span data-ttu-id="8ba12-150">Tegye próbára a Power BI közösségét</span><span class="sxs-lookup"><span data-stu-id="8ba12-150">Try the Power BI Community</span></span>](http://community.powerbi.com/)

