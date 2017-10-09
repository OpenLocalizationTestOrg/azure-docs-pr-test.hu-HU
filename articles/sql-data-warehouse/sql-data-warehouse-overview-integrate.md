---
title: "aaaBuild megoldások integrálva az SQL Data Warehouse |} Microsoft Docs"
description: "Eszközök és a partnerek, amelyekbe beépül az SQL Data Warehouse-megoldás. "
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: c8a4202dd84305bea4e4c2faf0e4791d026e794f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a><span data-ttu-id="6fc86-103">Használja ki az egyéb szolgáltatásokat az SQL Data Warehouse szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="6fc86-103">Leverage other services with SQL Data Warehouse</span></span>
<span data-ttu-id="6fc86-104">Ezenkívül tooits alapvető funkcióit, az SQL Data Warehouse lehetővé teszi, hogy a felhasználók tooleverage számos hello más szolgáltatásokkal együtt, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6fc86-104">In addition tooits core functionality, SQL Data Warehouse enables users tooleverage many of hello other services in Azure alongside it.</span></span>  <span data-ttu-id="6fc86-105">Pontosabban jelenleg a rendszer átirányította toodeeply integrálása hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6fc86-105">Specifically, we have currently taken steps toodeeply integrate with hello following:</span></span>

* <span data-ttu-id="6fc86-106">Power BI</span><span class="sxs-lookup"><span data-stu-id="6fc86-106">Power BI</span></span>
* <span data-ttu-id="6fc86-107">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="6fc86-107">Azure Data Factory</span></span>
* <span data-ttu-id="6fc86-108">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6fc86-108">Azure Machine Learning</span></span>
* <span data-ttu-id="6fc86-109">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="6fc86-109">Azure Stream Analytics</span></span>

<span data-ttu-id="6fc86-110">Dolgozunk a további szolgáltatások tooconnect hello Azure ökoszisztéma között.</span><span class="sxs-lookup"><span data-stu-id="6fc86-110">We are working tooconnect with more services across hello Azure ecosystem.</span></span>

## <a name="power-bi"></a><span data-ttu-id="6fc86-111">Power BI</span><span class="sxs-lookup"><span data-stu-id="6fc86-111">Power BI</span></span>
<span data-ttu-id="6fc86-112">A Power BI-integráció lehetővé teszi tooleverage hello számítási teljesítményt az SQL Data Warehouse hello dinamikus jelentéskészítéssel és a Power BI a képi megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="6fc86-112">Power BI integration allows you tooleverage hello compute power of SQL Data Warehouse with hello dynamic reporting and visualization of Power BI.</span></span> <span data-ttu-id="6fc86-113">A Power BI integrációs jelenleg tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6fc86-113">Power BI integration currently includes:</span></span>

* <span data-ttu-id="6fc86-114">**Közvetlen csatlakozás**: egy speciális logikai leküldéses közzététele elleni SQL Data Warehouse-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="6fc86-114">**Direct Connect**: A more advanced connection with logical pushdown against SQL Data Warehouse.</span></span>  <span data-ttu-id="6fc86-115">Magasabb szinten gyorsabb elemzése biztosít.</span><span class="sxs-lookup"><span data-stu-id="6fc86-115">This provides faster analysis on a larger scale.</span></span>
* <span data-ttu-id="6fc86-116">**Nyissa meg a Power BI**: hello "Megnyitás Power BI-ban" gombra példány információk tooPower BI, lehetővé téve a zökkenőmentesebb kapcsolat továbbítja.</span><span class="sxs-lookup"><span data-stu-id="6fc86-116">**Open in Power BI**: hello 'Open in Power BI' button passes instance information tooPower BI, allowing for a more seamless connection.</span></span>

<span data-ttu-id="6fc86-117">Lásd: [integráció a Power BI](sql-data-warehouse-integrate-power-bi.md) vagy hello [Power BI-dokumentáció](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="6fc86-117">See [Integrate with Power BI](sql-data-warehouse-integrate-power-bi.md) or hello [Power BI documentation](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) for more information.</span></span>

## <a name="azure-data-factory"></a><span data-ttu-id="6fc86-118">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="6fc86-118">Azure Data Factory</span></span>
<span data-ttu-id="6fc86-119">Az Azure Data Factory lehetőséget ad a felhasználók a felügyelt platform toocreate összetett kivonat-betöltési folyamatok.</span><span class="sxs-lookup"><span data-stu-id="6fc86-119">Azure Data Factory gives users a managed platform toocreate complex Extract-Load pipelines.</span></span>  <span data-ttu-id="6fc86-120">Az SQL Data Warehouse-integráció az Azure Data Factoryvel hello következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6fc86-120">SQL Data Warehouse's integration with Azure Data Factory includes hello following:</span></span>

* <span data-ttu-id="6fc86-121">**Tárolt eljárások**: az SQL Data Warehouse a tárolt eljárások végrehajtása hello Levezényelni.</span><span class="sxs-lookup"><span data-stu-id="6fc86-121">**Stored Procedures**: Orchestrate hello execution of stored procedures on SQL Data Warehouse.</span></span>
* <span data-ttu-id="6fc86-122">**Másolás**: használata ADF toomove adatokat az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6fc86-122">**Copy**: Use ADF toomove data into SQL Data Warehouse.</span></span>  <span data-ttu-id="6fc86-123">Ez a művelet ADF szabványos adatok mechanizmusát vagy a PolyBase hello alatt hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="6fc86-123">This operation can use ADF's standard data movement mechanism or PolyBase under hello covers.</span></span> 

<span data-ttu-id="6fc86-124">Lásd: [integráció az Azure Data Factoryvel](sql-data-warehouse-integrate-azure-data-factory.md) vagy hello [Azure Data Factory dokumentáció](https://azure.microsoft.com/documentation/services/data-factory/) további információt.</span><span class="sxs-lookup"><span data-stu-id="6fc86-124">See [Integrate with Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) or hello [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/) for more information.</span></span>

## <a name="azure-machine-learning"></a><span data-ttu-id="6fc86-125">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6fc86-125">Azure Machine Learning</span></span>
<span data-ttu-id="6fc86-126">Az Azure Machine Learning egy teljes körűen felügyelt analytics szolgáltatás, amely lehetővé teszi, hogy a felhasználók toocreate bonyolult modellek prediktív eszközök számos kihasználva.</span><span class="sxs-lookup"><span data-stu-id="6fc86-126">Azure Machine Learning is a fully managed analytics service which allows users toocreate intricate models leveraging a large set of predictive tools.</span></span>  <span data-ttu-id="6fc86-127">Az SQL Data Warehouse a következő funkciókat hello támogatott, a forrás- és a célkiszolgálón, ezek a modellek:</span><span class="sxs-lookup"><span data-stu-id="6fc86-127">SQL Data Warehouse is supported as both a source and destination for these models with hello following functionality:</span></span>

* <span data-ttu-id="6fc86-128">**Adatok olvasása:** modellek meghajtó léptékű T-SQL használatával az SQL Data Warehouse ellen.</span><span class="sxs-lookup"><span data-stu-id="6fc86-128">**Read Data:** Drive models at scale using T-SQL against SQL Data Warehouse.</span></span>
* <span data-ttu-id="6fc86-129">**Adatok írása:** véglegesítési bármely modellből módosításait tooSQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="6fc86-129">**Write Data:** Commit changes from any model back tooSQL Data Warehouse.</span></span>

<span data-ttu-id="6fc86-130">Lásd: [integráció az Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) vagy hello [Azure Machine Learning dokumentációs](https://azure.microsoft.com/services/machine-learning/) további információt.</span><span class="sxs-lookup"><span data-stu-id="6fc86-130">See [Integrate with Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) or hello [Azure Machine Learning documentation](https://azure.microsoft.com/services/machine-learning/) for more information.</span></span>

## <a name="azure-stream-analytics"></a><span data-ttu-id="6fc86-131">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="6fc86-131">Azure Stream Analytics</span></span>
<span data-ttu-id="6fc86-132">Az Azure Stream Analytics egy összetett, teljes körűen felügyelt infrastruktúra dolgoz fel, és az Azure Event Hubs alapján generált események adatainak felhasználása.</span><span class="sxs-lookup"><span data-stu-id="6fc86-132">Azure Stream Analytics is a complex, fully managed infrastructure for processing and consuming event data generated from Azure Event Hub.</span></span>  <span data-ttu-id="6fc86-133">Az SQL Data Warehouse szolgáltatással integrációja lehetővé teszi az adatfolyamként történő, hatékony feldolgozott adatokat toobe és tárolt relációs adatok mélyebb engedélyezése mellett, további speciális elemzési.</span><span class="sxs-lookup"><span data-stu-id="6fc86-133">Integration with SQL Data Warehouse allows for streaming data toobe effectively processed and stored alongside relational data enabling deeper, more advanced analysis.</span></span>  

* <span data-ttu-id="6fc86-134">**Job Output:** küldési kimenetét a Stream Analytics feladatok közvetlenül tooSQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="6fc86-134">**Job Output:** Send output from Stream Analytics jobs directly tooSQL Data Warehouse.</span></span>

<span data-ttu-id="6fc86-135">Lásd: [integráció az Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) vagy hello [Azure Stream Analytics-dokumentáció](https://azure.microsoft.com/documentation/services/stream-analytics/) további információt.</span><span class="sxs-lookup"><span data-stu-id="6fc86-135">See [Integrate with Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) or hello [Azure Stream Analytics documentation](https://azure.microsoft.com/documentation/services/stream-analytics/) for more information.</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
