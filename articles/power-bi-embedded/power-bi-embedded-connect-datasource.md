---
title: "a Power BI Embedded - csatlakozás tooa adatforrás aaaMicrosoft"
description: "A Power BI Embedded, csatlakozás toodata források"
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
ms.openlocfilehash: b1aad6e638104716d90f7e1d060eefcbc9daedbc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-data-source"></a><span data-ttu-id="fb754-103">Tooa adatforrás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="fb754-103">Connect tooa data source</span></span>
<span data-ttu-id="fb754-104">A **Power BI Embedded**, jelentés beágyazása az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="fb754-104">With **Power BI Embedded**, you can embed reports into your own app.</span></span> <span data-ttu-id="fb754-105">Power BI-jelentés beágyazása az alkalmazásba, hello jelentés csatlakozik-e az alapul szolgáló adatokat toohello **importálása** hello adatok vagy az **közvetlenül csatlakozó** toohello használhatja  **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="fb754-105">When you embed a Power BI report into your app, hello report connects toohello underlying data by **importing** a copy of hello data or by **connecting directly** toohello data source using **DirectQuery**.</span></span>

<span data-ttu-id="fb754-106">Az alábbiakban közti különbségeket hello **importálási** és **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="fb754-106">Here are hello differences between using **Import** and **DirectQuery**.</span></span>

| <span data-ttu-id="fb754-107">Importálás</span><span class="sxs-lookup"><span data-stu-id="fb754-107">Import</span></span> | <span data-ttu-id="fb754-108">DirectQuery</span><span class="sxs-lookup"><span data-stu-id="fb754-108">DirectQuery</span></span> |
| --- | --- |
| <span data-ttu-id="fb754-109">A táblákat, oszlopokat *és az adatok* importálhatja vagy másolhatja a hello jelentés adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="fb754-109">Tables, columns, *and data* are imported or copied into hello report's dataset.</span></span> <span data-ttu-id="fb754-110">toosee történt toohello alapjául szolgáló adatok megváltozik, frissítse, vagy importálja, a teljes aktuális adatkészletet újra.</span><span class="sxs-lookup"><span data-stu-id="fb754-110">toosee changes that occurred toohello underlying data, you must refresh, or import, a complete, current dataset again.</span></span> |<span data-ttu-id="fb754-111">Csak *táblákat és oszlopokat* importálhatja vagy másolhatja a hello jelentés adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="fb754-111">Only *tables and columns* are imported or copied into hello report's dataset.</span></span> <span data-ttu-id="fb754-112">Bármikor megtekintheti a legfrissebb adatok hello.</span><span class="sxs-lookup"><span data-stu-id="fb754-112">You always view hello most current data.</span></span> |

<span data-ttu-id="fb754-113">Power BI Embedded akkor felhő adatforrások DirectQuery használható, de nem a helyi adatforrások jelenleg.</span><span class="sxs-lookup"><span data-stu-id="fb754-113">With Power BI Embedded, you can use DirectQuery with cloud data sources but not on-premises data sources at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="fb754-114">hello helyszíni adatátjáró nem támogatott a Power BI Embedded most.</span><span class="sxs-lookup"><span data-stu-id="fb754-114">hello On-Premises Data Gateway is not supported with Power BI Embedded at this time.</span></span> <span data-ttu-id="fb754-115">Ez azt jelenti, hogy a helyszíni adatforrások DirectQuery nem használhat.</span><span class="sxs-lookup"><span data-stu-id="fb754-115">This means you cannot use DirectQuery with on-premises data sources.</span></span>

## <a name="supported-data-sources"></a><span data-ttu-id="fb754-116">Támogatott adatforrások</span><span class="sxs-lookup"><span data-stu-id="fb754-116">Supported data sources</span></span>

<span data-ttu-id="fb754-117">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="fb754-117">**DirectQuery**</span></span>
* <span data-ttu-id="fb754-118">Az Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="fb754-118">Azure SQL database</span></span>
* <span data-ttu-id="fb754-119">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="fb754-119">Azure SQL Data Warehouse</span></span>

<span data-ttu-id="fb754-120">**Importálás**</span><span class="sxs-lookup"><span data-stu-id="fb754-120">**Import**</span></span>

<span data-ttu-id="fb754-121">Az összes elérhető adatforrások hello Power BI Desktop segítségével importálhatja.</span><span class="sxs-lookup"><span data-stu-id="fb754-121">You can import using all of hello available data sources within Power BI Desktop.</span></span> <span data-ttu-id="fb754-122">Lehetővé teszi a **nem** kell tudni toorefresh belül a Power BI Embedded adatokat.</span><span class="sxs-lookup"><span data-stu-id="fb754-122">You will **not** be able toorefresh that data within Power BI Embedded.</span></span> <span data-ttu-id="fb754-123">Tooupload módosításokat követően tooyour PBIX-fájl BI Embedded tooPower.</span><span class="sxs-lookup"><span data-stu-id="fb754-123">You will have tooupload changes tooyour PBIX file tooPower BI Embedded.</span></span> <span data-ttu-id="fb754-124">Ez a rendelkezésre álló átjáró toono miatt.</span><span class="sxs-lookup"><span data-stu-id="fb754-124">This is due toono available gateway.</span></span> 

## <a name="benefits-of-using-directquery"></a><span data-ttu-id="fb754-125">DirectQuery használatának előnyei</span><span class="sxs-lookup"><span data-stu-id="fb754-125">Benefits of using DirectQuery</span></span>
<span data-ttu-id="fb754-126">Nincsenek két elsődleges előnnyel jár, használatakor **DirectQuery**:</span><span class="sxs-lookup"><span data-stu-id="fb754-126">There are two primary benefits when using **DirectQuery**:</span></span>

* <span data-ttu-id="fb754-127">**DirectQuery** segítségével készít képi megjelenítések alatt a nagy adatkészleteknél, ahol egyébként lenne, amelyeknek toofirst importálása összes hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="fb754-127">**DirectQuery** lets you build visualizations over very large datasets, where it otherwise would be unfeasible toofirst import all of hello data.</span></span>
* <span data-ttu-id="fb754-128">Az alapul szolgáló adatváltozásokat is van szükség adatok frissítését, és az egyes jelentések hello kell toodisplay aktuális adatokat is nagy adatátvitelt, és amelyeknek újraimportálásának adatokat.</span><span class="sxs-lookup"><span data-stu-id="fb754-128">Underlying data changes can require a refresh of data, and for some reports, hello need toodisplay current data can require large data transfers, making re-importing data unfeasible.</span></span> <span data-ttu-id="fb754-129">Ezzel szemben **DirectQuery** jelentések mindig aktuális adatokat használják.</span><span class="sxs-lookup"><span data-stu-id="fb754-129">By contrast, **DirectQuery** reports always use current data.</span></span>

## <a name="limitations-of-directquery"></a><span data-ttu-id="fb754-130">DirectQuery korlátozásai</span><span class="sxs-lookup"><span data-stu-id="fb754-130">Limitations of DirectQuery</span></span>
   <span data-ttu-id="fb754-131">Van néhány korlátozások toousing **DirectQuery**:</span><span class="sxs-lookup"><span data-stu-id="fb754-131">There are a few limitations toousing **DirectQuery**:</span></span>

* <span data-ttu-id="fb754-132">Minden táblának egyetlen adatbázisra kell származnia.</span><span class="sxs-lookup"><span data-stu-id="fb754-132">All tables must come from a single database.</span></span>
* <span data-ttu-id="fb754-133">Ha hello lekérdezés túl összetett, hiba történik.</span><span class="sxs-lookup"><span data-stu-id="fb754-133">If hello query is overly complex, an error will occur.</span></span> <span data-ttu-id="fb754-134">tooremedy hello hiba, hogy kevésbé összetett, meg kell refactor hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="fb754-134">tooremedy hello error you must refactor hello query so it is less complex.</span></span> <span data-ttu-id="fb754-135">Hello lekérdezés összetett kell lennie, ha szüksége lesz a tooimport hello adatok használata helyett **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="fb754-135">If hello query must be complex, you will need tooimport hello data instead of using **DirectQuery**.</span></span>
* <span data-ttu-id="fb754-136">Szűrés kapcsolat, korlátozott tooa irányra ahelyett, hogy mindkét irányban.</span><span class="sxs-lookup"><span data-stu-id="fb754-136">Relationship filtering is limited tooa single direction, rather than both directions.</span></span>
* <span data-ttu-id="fb754-137">Egy oszlop hello adatok típusa nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="fb754-137">You cannot change hello data type of a column.</span></span>
* <span data-ttu-id="fb754-138">Alapértelmezés szerint engedélyezett a mértékek DAX-kifejezések korlátozások kerülnek.</span><span class="sxs-lookup"><span data-stu-id="fb754-138">By default, limitations are placed on DAX expressions allowed in measures.</span></span> <span data-ttu-id="fb754-139">Lásd: [DirectQuery és intézkedések](#measures).</span><span class="sxs-lookup"><span data-stu-id="fb754-139">See [DirectQuery and measures](#measures).</span></span>

<a name="measures"/>

## <a name="directquery-and-measures"></a><span data-ttu-id="fb754-140">DirectQuery és intézkedések</span><span class="sxs-lookup"><span data-stu-id="fb754-140">DirectQuery and measures</span></span>
<span data-ttu-id="fb754-141">küldött toohello alapul szolgáló adatforrásban tooensure lekérdezések elfogadható teljesítmény, a mértékek a korlátozást.</span><span class="sxs-lookup"><span data-stu-id="fb754-141">tooensure queries sent toohello underlying data source have acceptable performance, limitations are imposed on measures.</span></span> <span data-ttu-id="fb754-142">Használata esetén **Power BI Desktop**, speciális felhasználók maguk dönthetik toobypass Ez a korlátozás kiválasztásával **fájl > lehetőségek és beállítások > Beállítások**.</span><span class="sxs-lookup"><span data-stu-id="fb754-142">When using **Power BI Desktop**, advanced users can choose toobypass this limitation by choosing **File > Options and settings > Options**.</span></span> <span data-ttu-id="fb754-143">A hello **beállítások** párbeszédpanelen válasszon **DirectQuery**, és hello beállítást **nem korlátozott mértékek engedélyezése DirectQuery módban**.</span><span class="sxs-lookup"><span data-stu-id="fb754-143">In hello **Options** dialog, choose **DirectQuery**, and select hello option **Allow unrestricted measures in DirectQuery mode**.</span></span> <span data-ttu-id="fb754-144">Ez a lehetőség van kiválasztva, ha bármely mérték érvényes DAX-kifejezés használható.</span><span class="sxs-lookup"><span data-stu-id="fb754-144">When that option is selected, any DAX expression that is valid for a measure can be used.</span></span> <span data-ttu-id="fb754-145">Lehet, hogy a felhasználók tisztában; azonban, hogy egyes kifejezések, amelyek végrehajtása jól hello adatok importálása azt eredményezheti, nagyon lelassul lekérdezések toohello háttér forrás, amikor a **DirectQuery** mód.</span><span class="sxs-lookup"><span data-stu-id="fb754-145">Users must be aware; however, that some expressions that perform very well when hello data is imported may result in very slow queries toohello backend source when in **DirectQuery** mode.</span></span> 

## <a name="see-also"></a><span data-ttu-id="fb754-146">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="fb754-146">See Also</span></span>
* [<span data-ttu-id="fb754-147">Ismerkedés a Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="fb754-147">Get started with Microsoft Power BI Embedded</span></span>](power-bi-embedded-get-started.md)
* [<span data-ttu-id="fb754-148">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="fb754-148">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

<span data-ttu-id="fb754-149">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="fb754-149">More questions?</span></span> [<span data-ttu-id="fb754-150">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="fb754-150">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

