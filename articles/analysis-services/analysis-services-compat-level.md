---
title: "Adatok modell kompatibilitási szint az Azure Analysis Services |} Microsoft Docs"
description: "Táblázatos modell kompatibilitási szintnek ismertetése."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/16/2017
ms.author: owend
ms.openlocfilehash: b11ba54c2cdc2675ec535368e7076613a5290212
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a><span data-ttu-id="81633-103">Az Analysis Services rendszerbeli táblázatos modellek kompatibilitási szintjének</span><span class="sxs-lookup"><span data-stu-id="81633-103">Compatibility level for Analysis Services tabular models</span></span>

<span data-ttu-id="81633-104">*Kompatibilitási szint* az Analysis Services motor a kiadási-specifikus viselkedések hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="81633-104">*Compatibility level* refers to release-specific behaviors in the Analysis Services engine.</span></span> <span data-ttu-id="81633-105">A kompatibilitási szint módosítása általában egybe az SQL Server kiadások.</span><span class="sxs-lookup"><span data-stu-id="81633-105">Changes to the compatibility level typically coincide with major releases of SQL Server.</span></span> <span data-ttu-id="81633-106">Ezeket a módosításokat is valósíthatók meg az Azure Analysis Services mindkét platformok közötti paritás fenntartása érdekében.</span><span class="sxs-lookup"><span data-stu-id="81633-106">These changes are also implemented in Azure Analysis Services to maintain parity between both platforms.</span></span> <span data-ttu-id="81633-107">Kompatibilitási szint módosítások is érintik a táblázatos modellek szolgáltatásához.</span><span class="sxs-lookup"><span data-stu-id="81633-107">Compatibility level changes also affect features available in your tabular models.</span></span> <span data-ttu-id="81633-108">Például DirectQuery és az objektum táblázatos metaadatok rendelkezik különböző megvalósítások attól függően, hogy a kompatibilitási szintet.</span><span class="sxs-lookup"><span data-stu-id="81633-108">For example, DirectQuery and tabular object metadata have different implementations depending on the compatibility level.</span></span> 

<span data-ttu-id="81633-109">Az Azure Analysis Services támogatja a 1400 és 1200-as kompatibilitási szintű táblázatos modellek.</span><span class="sxs-lookup"><span data-stu-id="81633-109">Azure Analysis Services supports tabular models at the 1200 and 1400 compatibility levels.</span></span>

<span data-ttu-id="81633-110">A legfrissebb kompatibilitási szintet a 1 400.</span><span class="sxs-lookup"><span data-stu-id="81633-110">The latest compatibility level is 1400.</span></span> <span data-ttu-id="81633-111">Ez a szint az SQL Server 2017 Analysis Services megegyezik.</span><span class="sxs-lookup"><span data-stu-id="81633-111">This level coincides with SQL Server 2017 Analysis Services.</span></span> <span data-ttu-id="81633-112">Az 1 400-kompatibilitási szint a főbb funkciók a következők:</span><span class="sxs-lookup"><span data-stu-id="81633-112">Major features in the 1400 compatibility level include:</span></span>

*  <span data-ttu-id="81633-113">Új infrastruktúra adatkapcsolat és a táblázatos modellek történő TOM API-k és TMSL parancsfájlkezelés támogatása.</span><span class="sxs-lookup"><span data-stu-id="81633-113">New infrastructure for data connectivity and import into tabular models with support for TOM APIs and TMSL scripting.</span></span> <span data-ttu-id="81633-114">Ezen új szolgáltatás lehetővé teszi, hogy a további adatforrások, például az Azure Blob-tároló támogatása.</span><span class="sxs-lookup"><span data-stu-id="81633-114">This new feature enables support for additional data sources such as Azure Blob storage.</span></span>
*  <span data-ttu-id="81633-115">Adatok átalakítása és adatok adategyesítési képességeket adatok beolvasása és M kifejezések használatával.</span><span class="sxs-lookup"><span data-stu-id="81633-115">Data transformation and data mashup capabilities by using Get Data and M expressions.</span></span>
*  <span data-ttu-id="81633-116">Mértékek egy DAX-kifejezés az adott sorok tulajdonsághoz támogatja.</span><span class="sxs-lookup"><span data-stu-id="81633-116">Measures support a Detail Rows property with a DAX expression.</span></span> <span data-ttu-id="81633-117">Ez a tulajdonság lehetővé teszi, hogy az ügyfél eszközök, például a Microsoft Excel fúrjon le a program egy összesített jelentés részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="81633-117">This property enables client tools like Microsoft Excel to drill down to detailed data from an aggregated report.</span></span> <span data-ttu-id="81633-118">Például ha a felhasználó egy régiót, és a hónap összes értékesítés, akkor megtekintheti a társított rendelés részleteit.</span><span class="sxs-lookup"><span data-stu-id="81633-118">For example, when users view total sales for a region and month, they can view the associated order details.</span></span> 
*  <span data-ttu-id="81633-119">A tábla és oszlop neve mellett található adatok biztonsági objektumszintű.</span><span class="sxs-lookup"><span data-stu-id="81633-119">Object-level security for table and column names, in addition to the data within them.</span></span>
*  <span data-ttu-id="81633-120">Továbbfejlesztett támogatása az egyenetlen szélű hierarchiákat.</span><span class="sxs-lookup"><span data-stu-id="81633-120">Enhanced support for ragged hierarchies.</span></span>
*  <span data-ttu-id="81633-121">Teljesítmény- és továbbfejlesztések.</span><span class="sxs-lookup"><span data-stu-id="81633-121">Performance and monitoring improvements.</span></span>
  
## <a name="set-compatibility-level"></a><span data-ttu-id="81633-122">Kompatibilitási szint beállítása</span><span class="sxs-lookup"><span data-stu-id="81633-122">Set compatibility level</span></span> 
 <span data-ttu-id="81633-123">Amikor létrehoz egy új táblázatos modell projektet SSDT, megadhatja a kompatibilitási szintje a **táblázatos modellek tervezőjében** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81633-123">When creating a new tabular model project in SSDT, you can specify the compatibility level on the **Tabular model designer** dialog.</span></span> 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 <span data-ttu-id="81633-125">Ha bejelöli a **ne jelenjen meg többé ez az üzenet** lehetőség, minden ezt követő projektek használata a megadott alapértelmezett kompatibilitási szintet.</span><span class="sxs-lookup"><span data-stu-id="81633-125">If you select the **Do not show this message again** option, all subsequent projects use the compatibility level you specified as the default.</span></span> <span data-ttu-id="81633-126">Az SSDT-ben az alapértelmezett kompatibilitási szint **eszközök** > **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="81633-126">You can change the default compatibility level in SSDT in **Tools** > **Options**.</span></span>  
  
 <span data-ttu-id="81633-127">Frissíti a meglévő táblázatos modell projekt SSDT-ben, adja meg a **kompatibilitási szint** tulajdonság a modell **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="81633-127">To upgrade an existing tabular model project in SSDT, set  the **Compatibility Level** property in the model **Properties** window.</span></span> <span data-ttu-id="81633-128">A szem előtt tartani, a kompatibilitási szint frissítése nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="81633-128">Keep in-mind, upgrading the compatibility level is irreversible.</span></span>
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a><span data-ttu-id="81633-129">Az SQL Server Management Studio táblázatos modell adatbázis kompatibilitási szintjének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="81633-129">Check compatibility level for a tabular model database in SQL Server Management Studio</span></span> 
 <span data-ttu-id="81633-130">Az SSMS, kattintson a jobb gombbal az adatbázis neve > **tulajdonságok** > **kompatibilitási szint**.</span><span class="sxs-lookup"><span data-stu-id="81633-130">In SSMS, right-click the database name > **Properties** > **Compatibility Level**.</span></span>  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a><span data-ttu-id="81633-131">Támogatott kompatibilitási szint szolgáltatáshoz az ssms kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="81633-131">Check supported compatibility level for a server in SSMS</span></span>  
 <span data-ttu-id="81633-132">Az SSMS, kattintson a jobb gombbal a kiszolgáló neve > **tulajdonságok** > **támogatott kompatibilitási szint**.</span><span class="sxs-lookup"><span data-stu-id="81633-132">In SSMS, right-click the server name>  **Properties** > **Supported Compatibility Level**.</span></span>  
  
 <span data-ttu-id="81633-133">Ez a tulajdonság meghatározza a legmagasabb szintű kompatibilitási egy adatbázist, amely a kiszolgálón (kivéve az előzetes verzió) fog futni.</span><span class="sxs-lookup"><span data-stu-id="81633-133">This property specifies the highest compatibility level of a database that will run on the server (excluding preview).</span></span> <span data-ttu-id="81633-134">A támogatott kompatibilitási szintje nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="81633-134">The supported compatibility level cannot be changed.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="81633-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81633-135">Next steps</span></span>
  <span data-ttu-id="81633-136">[A modell létrehozása Azure-portálon](analysis-services-create-model-portal.md) </span><span class="sxs-lookup"><span data-stu-id="81633-136">[Create a model in Azure portal](analysis-services-create-model-portal.md) </span></span>  
  [<span data-ttu-id="81633-137">Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="81633-137">Manage Analysis Services</span></span>](analysis-services-manage.md)  
