---
title: "Az SQL Data Warehouse szolgáltatással integrált megoldások |} Microsoft Docs"
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
ms.openlocfilehash: d407c29f99fd7537590ec787febd84a9e3f4f353
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a><span data-ttu-id="830de-103">Használja ki az egyéb szolgáltatásokat az SQL Data Warehouse szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="830de-103">Leverage other services with SQL Data Warehouse</span></span>
<span data-ttu-id="830de-104">Az alapvető funkciók mellett az SQL Data Warehouse lehetővé teszi a felhasználók számos egyéb szolgáltatásokat az Azure-ban mellett azt.</span><span class="sxs-lookup"><span data-stu-id="830de-104">In addition to its core functionality, SQL Data Warehouse enables users to leverage many of the other services in Azure alongside it.</span></span>  <span data-ttu-id="830de-105">Pontosabban jelenleg a rendszer átirányította mélyen integrálása a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="830de-105">Specifically, we have currently taken steps to deeply integrate with the following:</span></span>

* <span data-ttu-id="830de-106">Power BI</span><span class="sxs-lookup"><span data-stu-id="830de-106">Power BI</span></span>
* <span data-ttu-id="830de-107">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="830de-107">Azure Data Factory</span></span>
* <span data-ttu-id="830de-108">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="830de-108">Azure Machine Learning</span></span>
* <span data-ttu-id="830de-109">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="830de-109">Azure Stream Analytics</span></span>

<span data-ttu-id="830de-110">Jelenleg dolgozunk, az Azure-ökoszisztéma között további szolgáltatások csatlakozhatnak.</span><span class="sxs-lookup"><span data-stu-id="830de-110">We are working to connect with more services across the Azure ecosystem.</span></span>

## <a name="power-bi"></a><span data-ttu-id="830de-111">Power BI</span><span class="sxs-lookup"><span data-stu-id="830de-111">Power BI</span></span>
<span data-ttu-id="830de-112">A Power BI-integráció lehetővé teszi, hogy kihasználja az SQL Data Warehouse a dinamikus jelentéskészítéssel a számítási teljesítmény és a Power bi képi megjelenítések.</span><span class="sxs-lookup"><span data-stu-id="830de-112">Power BI integration allows you to leverage the compute power of SQL Data Warehouse with the dynamic reporting and visualization of Power BI.</span></span> <span data-ttu-id="830de-113">A Power BI integrációs jelenleg tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="830de-113">Power BI integration currently includes:</span></span>

* <span data-ttu-id="830de-114">**Közvetlen csatlakozás**: egy speciális logikai leküldéses közzététele elleni SQL Data Warehouse-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="830de-114">**Direct Connect**: A more advanced connection with logical pushdown against SQL Data Warehouse.</span></span>  <span data-ttu-id="830de-115">Magasabb szinten gyorsabb elemzése biztosít.</span><span class="sxs-lookup"><span data-stu-id="830de-115">This provides faster analysis on a larger scale.</span></span>
* <span data-ttu-id="830de-116">**Nyissa meg a Power BI**: A "Megnyitás Power BI-ban" gombra példány információt átadja a Power BI lehetővé teszi a zökkenőmentesebb kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="830de-116">**Open in Power BI**: The 'Open in Power BI' button passes instance information to Power BI, allowing for a more seamless connection.</span></span>

<span data-ttu-id="830de-117">Lásd: [integráció a Power BI](sql-data-warehouse-integrate-power-bi.md) vagy a [Power BI-dokumentáció](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="830de-117">See [Integrate with Power BI](sql-data-warehouse-integrate-power-bi.md) or the [Power BI documentation](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) for more information.</span></span>

## <a name="azure-data-factory"></a><span data-ttu-id="830de-118">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="830de-118">Azure Data Factory</span></span>
<span data-ttu-id="830de-119">Az Azure Data Factory lehetőséget ad a felhasználók egy felügyelt platformon összetett kivonat-betöltési folyamatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="830de-119">Azure Data Factory gives users a managed platform to create complex Extract-Load pipelines.</span></span>  <span data-ttu-id="830de-120">Az SQL Data Warehouse-integráció az Azure Data Factoryvel az alábbiakat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="830de-120">SQL Data Warehouse's integration with Azure Data Factory includes the following:</span></span>

* <span data-ttu-id="830de-121">**Tárolt eljárások**: Levezényelni a SQL Data Warehouse tárolt eljárások végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="830de-121">**Stored Procedures**: Orchestrate the execution of stored procedures on SQL Data Warehouse.</span></span>
* <span data-ttu-id="830de-122">**Másolás**: használata ADF áthelyezni az adatokat az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="830de-122">**Copy**: Use ADF to move data into SQL Data Warehouse.</span></span>  <span data-ttu-id="830de-123">Ez a művelet a ADF szabványos adatok mechanizmusát vagy a PolyBase a színfalak.</span><span class="sxs-lookup"><span data-stu-id="830de-123">This operation can use ADF's standard data movement mechanism or PolyBase under the covers.</span></span> 

<span data-ttu-id="830de-124">Lásd: [integráció az Azure Data Factoryvel](sql-data-warehouse-integrate-azure-data-factory.md) vagy a [Azure Data Factory dokumentáció](https://azure.microsoft.com/documentation/services/data-factory/) további információt.</span><span class="sxs-lookup"><span data-stu-id="830de-124">See [Integrate with Azure Data Factory](sql-data-warehouse-integrate-azure-data-factory.md) or the [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/) for more information.</span></span>

## <a name="azure-machine-learning"></a><span data-ttu-id="830de-125">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="830de-125">Azure Machine Learning</span></span>
<span data-ttu-id="830de-126">Az Azure Machine Learning egy teljes körűen felügyelt analytics szolgáltatás, amely lehetővé teszi a felhasználók számára, hogy bonyolult modellek prediktív eszközök számos kihasználva.</span><span class="sxs-lookup"><span data-stu-id="830de-126">Azure Machine Learning is a fully managed analytics service which allows users to create intricate models leveraging a large set of predictive tools.</span></span>  <span data-ttu-id="830de-127">Az SQL Data Warehouse az alábbi funkciókat, a forrás- és a célkiszolgálón, ezek a modellek használata támogatott:</span><span class="sxs-lookup"><span data-stu-id="830de-127">SQL Data Warehouse is supported as both a source and destination for these models with the following functionality:</span></span>

* <span data-ttu-id="830de-128">**Adatok olvasása:** modellek meghajtó léptékű T-SQL használatával az SQL Data Warehouse ellen.</span><span class="sxs-lookup"><span data-stu-id="830de-128">**Read Data:** Drive models at scale using T-SQL against SQL Data Warehouse.</span></span>
* <span data-ttu-id="830de-129">**Adatok írása:** véglegesítési visszavált a modellből az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="830de-129">**Write Data:** Commit changes from any model back to SQL Data Warehouse.</span></span>

<span data-ttu-id="830de-130">Lásd: [integráció az Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) vagy a [Azure Machine Learning dokumentációs](https://azure.microsoft.com/services/machine-learning/) további információt.</span><span class="sxs-lookup"><span data-stu-id="830de-130">See [Integrate with Azure Machine Learning](sql-data-warehouse-integrate-azure-machine-learning.md) or the [Azure Machine Learning documentation](https://azure.microsoft.com/services/machine-learning/) for more information.</span></span>

## <a name="azure-stream-analytics"></a><span data-ttu-id="830de-131">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="830de-131">Azure Stream Analytics</span></span>
<span data-ttu-id="830de-132">Az Azure Stream Analytics egy összetett, teljes körűen felügyelt infrastruktúra dolgoz fel, és az Azure Event Hubs alapján generált események adatainak felhasználása.</span><span class="sxs-lookup"><span data-stu-id="830de-132">Azure Stream Analytics is a complex, fully managed infrastructure for processing and consuming event data generated from Azure Event Hub.</span></span>  <span data-ttu-id="830de-133">Az SQL Data Warehouse integrációja lehetővé teszi, hogy az adatfolyamként történő hatékony feldolgozása és relációs adatok mélyebb, speciális elemzési engedélyezése mellett tárolt adatok.</span><span class="sxs-lookup"><span data-stu-id="830de-133">Integration with SQL Data Warehouse allows for streaming data to be effectively processed and stored alongside relational data enabling deeper, more advanced analysis.</span></span>  

* <span data-ttu-id="830de-134">**Job Output:** kimenetként a Stream Analytics-feladatok közvetlenül az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="830de-134">**Job Output:** Send output from Stream Analytics jobs directly to SQL Data Warehouse.</span></span>

<span data-ttu-id="830de-135">Lásd: [integráció az Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) vagy a [Azure Stream Analytics-dokumentáció](https://azure.microsoft.com/documentation/services/stream-analytics/) további információt.</span><span class="sxs-lookup"><span data-stu-id="830de-135">See [Integrate with Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md) or the [Azure Stream Analytics documentation](https://azure.microsoft.com/documentation/services/stream-analytics/) for more information.</span></span>

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
