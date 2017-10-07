---
title: "aaaData modell kompatibilitási szint az Azure Analysis Services |} Microsoft Docs"
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
ms.openlocfilehash: bfaf0c60666729d1e6e0baf082c046ea9faa4e86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a><span data-ttu-id="82fb8-103">Az Analysis Services rendszerbeli táblázatos modellek kompatibilitási szintjének</span><span class="sxs-lookup"><span data-stu-id="82fb8-103">Compatibility level for Analysis Services tabular models</span></span>

<span data-ttu-id="82fb8-104">*Kompatibilitási szint* toorelease-specifikus viselkedések az Analysis Services-motor hello hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="82fb8-104">*Compatibility level* refers toorelease-specific behaviors in hello Analysis Services engine.</span></span> <span data-ttu-id="82fb8-105">Módosítások toohello kompatibilitási szint általában egybe az SQL Server kiadások.</span><span class="sxs-lookup"><span data-stu-id="82fb8-105">Changes toohello compatibility level typically coincide with major releases of SQL Server.</span></span> <span data-ttu-id="82fb8-106">Ezeket a módosításokat az Azure Analysis Services toomaintain paritása mindkét platform is valósíthatók meg.</span><span class="sxs-lookup"><span data-stu-id="82fb8-106">These changes are also implemented in Azure Analysis Services toomaintain parity between both platforms.</span></span> <span data-ttu-id="82fb8-107">Kompatibilitási szint módosítások is érintik a táblázatos modellek szolgáltatásához.</span><span class="sxs-lookup"><span data-stu-id="82fb8-107">Compatibility level changes also affect features available in your tabular models.</span></span> <span data-ttu-id="82fb8-108">Például DirectQuery és az objektum táblázatos metaadatok rendelkezik különböző megvalósítások attól függően, hogy hello kompatibilitási szintet.</span><span class="sxs-lookup"><span data-stu-id="82fb8-108">For example, DirectQuery and tabular object metadata have different implementations depending on hello compatibility level.</span></span> 

<span data-ttu-id="82fb8-109">Az Azure Analysis Services hello 1200-as és 1 400 kompatibilitási szintű táblázatos modellek támogatja.</span><span class="sxs-lookup"><span data-stu-id="82fb8-109">Azure Analysis Services supports tabular models at hello 1200 and 1400 compatibility levels.</span></span>

<span data-ttu-id="82fb8-110">hello legfrissebb kompatibilitási szintet a 1 400.</span><span class="sxs-lookup"><span data-stu-id="82fb8-110">hello latest compatibility level is 1400.</span></span> <span data-ttu-id="82fb8-111">Ez a szint az SQL Server 2017 Analysis Services megegyezik.</span><span class="sxs-lookup"><span data-stu-id="82fb8-111">This level coincides with SQL Server 2017 Analysis Services.</span></span> <span data-ttu-id="82fb8-112">Fő szolgáltatásainak hello 1 400 kompatibilitási szintje a következők:</span><span class="sxs-lookup"><span data-stu-id="82fb8-112">Major features in hello 1400 compatibility level include:</span></span>

*  <span data-ttu-id="82fb8-113">Új infrastruktúra adatkapcsolat és a táblázatos modellek történő TOM API-k és TMSL parancsfájlkezelés támogatása.</span><span class="sxs-lookup"><span data-stu-id="82fb8-113">New infrastructure for data connectivity and import into tabular models with support for TOM APIs and TMSL scripting.</span></span> <span data-ttu-id="82fb8-114">Ezen új szolgáltatás lehetővé teszi, hogy a további adatforrások, például az Azure Blob-tároló támogatása.</span><span class="sxs-lookup"><span data-stu-id="82fb8-114">This new feature enables support for additional data sources such as Azure Blob storage.</span></span>
*  <span data-ttu-id="82fb8-115">Adatok átalakítása és adatok adategyesítési képességeket adatok beolvasása és M kifejezések használatával.</span><span class="sxs-lookup"><span data-stu-id="82fb8-115">Data transformation and data mashup capabilities by using Get Data and M expressions.</span></span>
*  <span data-ttu-id="82fb8-116">Mértékek egy DAX-kifejezés az adott sorok tulajdonsághoz támogatja.</span><span class="sxs-lookup"><span data-stu-id="82fb8-116">Measures support a Detail Rows property with a DAX expression.</span></span> <span data-ttu-id="82fb8-117">Ez a tulajdonság lehetővé teszi, hogy az ügyfél eszközök, például a Microsoft Excel toodrill egy összesített jelentés toodetailed-adatok.</span><span class="sxs-lookup"><span data-stu-id="82fb8-117">This property enables client tools like Microsoft Excel toodrill down toodetailed data from an aggregated report.</span></span> <span data-ttu-id="82fb8-118">Például ha a felhasználó egy régiót, és a hónap összes értékesítés, akkor megtekintheti a kapcsolódó hello rendelés részleteit.</span><span class="sxs-lookup"><span data-stu-id="82fb8-118">For example, when users view total sales for a region and month, they can view hello associated order details.</span></span> 
*  <span data-ttu-id="82fb8-119">A tábla és oszlop objektumszintű biztonsági nevet, továbbá toohello adatok rajtuk.</span><span class="sxs-lookup"><span data-stu-id="82fb8-119">Object-level security for table and column names, in addition toohello data within them.</span></span>
*  <span data-ttu-id="82fb8-120">Továbbfejlesztett támogatása az egyenetlen szélű hierarchiákat.</span><span class="sxs-lookup"><span data-stu-id="82fb8-120">Enhanced support for ragged hierarchies.</span></span>
*  <span data-ttu-id="82fb8-121">Teljesítmény- és továbbfejlesztések.</span><span class="sxs-lookup"><span data-stu-id="82fb8-121">Performance and monitoring improvements.</span></span>
  
## <a name="set-compatibility-level"></a><span data-ttu-id="82fb8-122">Kompatibilitási szint beállítása</span><span class="sxs-lookup"><span data-stu-id="82fb8-122">Set compatibility level</span></span> 
 <span data-ttu-id="82fb8-123">Amikor létrehoz egy új táblázatos modell projektet SSDT, megadhatja hello kompatibilitási szint hello **táblázatos modellek tervezőjében** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="82fb8-123">When creating a new tabular model project in SSDT, you can specify hello compatibility level on hello **Tabular model designer** dialog.</span></span> 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 <span data-ttu-id="82fb8-125">Ha hello **ne jelenjen meg többé ez az üzenet** lehetőség, minden ezt követő projektek használata hello alapértelmezettként megadott hello kompatibilitási szintet.</span><span class="sxs-lookup"><span data-stu-id="82fb8-125">If you select hello **Do not show this message again** option, all subsequent projects use hello compatibility level you specified as hello default.</span></span> <span data-ttu-id="82fb8-126">Hello alapértelmezett kompatibilitási szint az SSDT-ben **eszközök** > **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="82fb8-126">You can change hello default compatibility level in SSDT in **Tools** > **Options**.</span></span>  
  
 <span data-ttu-id="82fb8-127">az SSDT, set hello meglévő táblázatos modell projekt tooupgrade **kompatibilitási szint** hello modellben tulajdonság **tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="82fb8-127">tooupgrade an existing tabular model project in SSDT, set  hello **Compatibility Level** property in hello model **Properties** window.</span></span> <span data-ttu-id="82fb8-128">A szem előtt tartani, hello kompatibilitási szint frissítése nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="82fb8-128">Keep in-mind, upgrading hello compatibility level is irreversible.</span></span>
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a><span data-ttu-id="82fb8-129">Az SQL Server Management Studio táblázatos modell adatbázis kompatibilitási szintjének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="82fb8-129">Check compatibility level for a tabular model database in SQL Server Management Studio</span></span> 
 <span data-ttu-id="82fb8-130">Az SSMS, kattintson a jobb gombbal hello adatbázis neve > **tulajdonságok** > **kompatibilitási szint**.</span><span class="sxs-lookup"><span data-stu-id="82fb8-130">In SSMS, right-click hello database name > **Properties** > **Compatibility Level**.</span></span>  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a><span data-ttu-id="82fb8-131">Támogatott kompatibilitási szint szolgáltatáshoz az ssms kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="82fb8-131">Check supported compatibility level for a server in SSMS</span></span>  
 <span data-ttu-id="82fb8-132">Az SSMS, kattintson a jobb gombbal hello kiszolgáló neve > **tulajdonságok** > **támogatott kompatibilitási szint**.</span><span class="sxs-lookup"><span data-stu-id="82fb8-132">In SSMS, right-click hello server name>  **Properties** > **Supported Compatibility Level**.</span></span>  
  
 <span data-ttu-id="82fb8-133">Ez a tulajdonság meghatározza a legmagasabb kompatibilitási szintje hello egy adatbázist, amely (kivéve az előzetes verzió) hello kiszolgálón fog futni.</span><span class="sxs-lookup"><span data-stu-id="82fb8-133">This property specifies hello highest compatibility level of a database that will run on hello server (excluding preview).</span></span> <span data-ttu-id="82fb8-134">hello támogatott kompatibilitási szintje nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="82fb8-134">hello supported compatibility level cannot be changed.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="82fb8-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82fb8-135">Next steps</span></span>
  <span data-ttu-id="82fb8-136">[A modell létrehozása Azure-portálon](analysis-services-create-model-portal.md) </span><span class="sxs-lookup"><span data-stu-id="82fb8-136">[Create a model in Azure portal](analysis-services-create-model-portal.md) </span></span>  
  [<span data-ttu-id="82fb8-137">Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="82fb8-137">Manage Analysis Services</span></span>](analysis-services-manage.md)  
