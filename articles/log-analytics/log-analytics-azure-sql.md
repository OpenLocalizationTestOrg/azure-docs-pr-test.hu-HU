---
title: "a Naplóelemzési SQL elemzési megoldások aaaAzure |} Microsoft Docs"
description: "hello Azure SQL elemzési megoldások kezelheti az Azure SQL Database adatbázisok."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: fe228bb3cb3f9d578a84707c3917f02fbeb8a627
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a><span data-ttu-id="bde45-103">Azure SQL adatbázis Azure SQL elemzés (előzetes verzió) Naplóelemzési figyelése</span><span class="sxs-lookup"><span data-stu-id="bde45-103">Monitor Azure SQL Database using Azure SQL Analytics (Preview) in Log Analytics</span></span>

![Az Azure SQL elemzés szimbólum](./media/log-analytics-azure-sql/azure-sql-symbol.png)

<span data-ttu-id="bde45-105">hello Azure SQL elemzési megoldás az Azure Naplóelemzés gyűjt, és fontos SQL Azure teljesítménymutatók visualizes.</span><span class="sxs-lookup"><span data-stu-id="bde45-105">hello Azure SQL Analytics solution in Azure Log Analytics collects and visualizes important SQL Azure performance metrics.</span></span> <span data-ttu-id="bde45-106">Hello metrikák gyűjtött hello megoldás segítségével létrehozhat egyéni ellenőrzési szabályok és értesítések.</span><span class="sxs-lookup"><span data-stu-id="bde45-106">By using hello metrics that you collect with hello solution, you can create custom monitoring rules and alerts.</span></span> <span data-ttu-id="bde45-107">Figyelheti, hogy az Azure SQL Database és a rugalmas készlet metrikák több Azure-előfizetések és rugalmas készletek, és jelenítheti meg őket.</span><span class="sxs-lookup"><span data-stu-id="bde45-107">And, you can monitor Azure SQL Database and elastic pool metrics across multiple Azure subscriptions and elastic pools and visualize them.</span></span> <span data-ttu-id="bde45-108">hello megoldás is segítséget nyújt az alkalmazáscsoportokat az egyes rétegekben tooidentify problémákat.</span><span class="sxs-lookup"><span data-stu-id="bde45-108">hello solution also helps you tooidentify issues at each layer of your application stack.</span></span>  <span data-ttu-id="bde45-109">Használja [Azure diagnosztikai metrikák](log-analytics-azure-storage.md) együtt Naplóelemzési megtekinti egyetlen Naplóelemzési munkaterület az Azure SQL-adatbázisok és a rugalmas készletek toopresent adatait.</span><span class="sxs-lookup"><span data-stu-id="bde45-109">It uses [Azure Diagnostic metrics](log-analytics-azure-storage.md) together with Log Analytics views toopresent data about all your Azure SQL databases and elastic pools in a single Log Analytics workspace.</span></span>

<span data-ttu-id="bde45-110">Jelenleg ez a minta megoldás legfeljebb too150, 000 Azure SQL-adatbázisok és 5000 SQL rugalmas készletek / munkaterület támogat.</span><span class="sxs-lookup"><span data-stu-id="bde45-110">Currently, this preview solution supports up too150,000 Azure SQL Databases and 5,000 SQL Elastic Pools per workspace.</span></span>

<span data-ttu-id="bde45-111">Azure SQL elemzési megoldás, mások számára elérhető Naplóelemzési, például hello segítségével megfigyelheti és hello állapotát az Azure-erőforrások értesítések fogadása – ebben az esetben az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="bde45-111">hello Azure SQL Analytics solution, like others available for Log Analytics, helps you monitor and receive notifications about hello health of your Azure resources—in this case, Azure SQL Database.</span></span> <span data-ttu-id="bde45-112">A Microsoft Azure SQL Database az ismerős SQL-kiszolgáló-szerű képességeket tooapplications hello Azure felhőben futó biztosító méretezhető relációs adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bde45-112">Microsoft Azure SQL Database is a scalable relational database service that provides familiar SQL-Server-like capabilities tooapplications running in hello Azure cloud.</span></span> <span data-ttu-id="bde45-113">A Naplóelemzési toocollect nyújt segítséget, összefüggéseket és strukturált és strukturálatlan adatok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="bde45-113">Log Analytics helps you toocollect, correlate, and visualize structured and unstructured data.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="bde45-114">Összekapcsolt források</span><span class="sxs-lookup"><span data-stu-id="bde45-114">Connected sources</span></span>

<span data-ttu-id="bde45-115">hello Azure SQL elemzési megoldások ügynökök tooconnect toohello Naplóelemzés szolgáltatás nem használja.</span><span class="sxs-lookup"><span data-stu-id="bde45-115">hello Azure SQL Analytics solution doesn't use agents tooconnect toohello Log Analytics service.</span></span>

<span data-ttu-id="bde45-116">a következő táblázat hello hello csatlakoztatott adatforrások, ez a megoldás által támogatott ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bde45-116">hello following table describes hello connected sources that are supported by this solution.</span></span>

| <span data-ttu-id="bde45-117">Összekapcsolt forrás</span><span class="sxs-lookup"><span data-stu-id="bde45-117">Connected Source</span></span> | <span data-ttu-id="bde45-118">Támogatás</span><span class="sxs-lookup"><span data-stu-id="bde45-118">Support</span></span> | <span data-ttu-id="bde45-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="bde45-119">Description</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="bde45-120">Windows-ügynökök</span><span class="sxs-lookup"><span data-stu-id="bde45-120">Windows agents</span></span>](log-analytics-windows-agents.md) | <span data-ttu-id="bde45-121">Nem</span><span class="sxs-lookup"><span data-stu-id="bde45-121">No</span></span> | <span data-ttu-id="bde45-122">A közvetlen Windows-ügynökök nem hello megoldás által használt.</span><span class="sxs-lookup"><span data-stu-id="bde45-122">Direct Windows agents are not used by hello solution.</span></span> |
| [<span data-ttu-id="bde45-123">Linux-ügynökök</span><span class="sxs-lookup"><span data-stu-id="bde45-123">Linux agents</span></span>](log-analytics-linux-agents.md) | <span data-ttu-id="bde45-124">Nem</span><span class="sxs-lookup"><span data-stu-id="bde45-124">No</span></span> | <span data-ttu-id="bde45-125">Közvetlen Linux-ügynökök nem hello megoldás által használt.</span><span class="sxs-lookup"><span data-stu-id="bde45-125">Direct Linux agents are not used by hello solution.</span></span> |
| [<span data-ttu-id="bde45-126">SCOM felügyeleti csoport</span><span class="sxs-lookup"><span data-stu-id="bde45-126">SCOM management group</span></span>](log-analytics-om-agents.md) | <span data-ttu-id="bde45-127">Nem</span><span class="sxs-lookup"><span data-stu-id="bde45-127">No</span></span> | <span data-ttu-id="bde45-128">Hello SCOM ügynök tooLog Analytics a közvetlen kapcsolat hello megoldás nem használja.</span><span class="sxs-lookup"><span data-stu-id="bde45-128">A direct connection from hello SCOM agent tooLog Analytics is not used by hello solution.</span></span> |
| [<span data-ttu-id="bde45-129">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="bde45-129">Azure storage account</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="bde45-130">Nem</span><span class="sxs-lookup"><span data-stu-id="bde45-130">No</span></span> | <span data-ttu-id="bde45-131">A Naplóelemzési nem hello adatokat olvasni egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="bde45-131">Log Analytics does not read hello data from a storage account.</span></span> |
| [<span data-ttu-id="bde45-132">Az Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="bde45-132">Azure diagnostics</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="bde45-133">Igen</span><span class="sxs-lookup"><span data-stu-id="bde45-133">Yes</span></span> | <span data-ttu-id="bde45-134">Az Azure metrika adatküldést tooLog Analytics közvetlenül az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="bde45-134">Azure metric data is sent tooLog Analytics directly by Azure.</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="bde45-135">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bde45-135">Prerequisites</span></span>

- <span data-ttu-id="bde45-136">Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="bde45-136">An Azure Subscription.</span></span> <span data-ttu-id="bde45-137">Ha még nincs fiókja, létrehozhat egy a [szabad](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="bde45-137">If you don't have one, you can create one for [free](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="bde45-138">A Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="bde45-138">A Log Analytics workspace.</span></span> <span data-ttu-id="bde45-139">Egy meglévő is használhatja, vagy beállíthatja a [hozzon létre egy újat](log-analytics-get-started.md) Ez a megoldás használata előtt.</span><span class="sxs-lookup"><span data-stu-id="bde45-139">You can use an existing one, or you can [create a new one](log-analytics-get-started.md) before you start using this solution.</span></span>
- <span data-ttu-id="bde45-140">Azure Diagnostics engedélyezése az Azure SQL-adatbázisok és rugalmas készletek és [toosend konfigurálja őket az adatok tooLog Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="bde45-140">Enable Azure Diagnostics for your Azure SQL databases and elastic pools and [configure them toosend their data tooLog Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span>

## <a name="configuration"></a><span data-ttu-id="bde45-141">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="bde45-141">Configuration</span></span>

<span data-ttu-id="bde45-142">Hajtsa végre a következő lépéseket tooadd hello Azure SQL elemzés megoldás tooyour munkaterület hello.</span><span class="sxs-lookup"><span data-stu-id="bde45-142">Perform hello following steps tooadd hello Azure SQL Analytics solution tooyour workspace.</span></span>

1. <span data-ttu-id="bde45-143">Hello Azure SQL elemzés megoldás tooyour munkaterület hozzáadása [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="bde45-143">Add hello Azure SQL Analytics solution tooyour workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="bde45-144">Hello Azure-portálon, kattintson **új** (hello + szimbólumra), majd hello az erőforrások listájához, jelölje ki **figyelés + felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="bde45-144">In hello Azure portal, click **New** (hello + symbol), then in hello list of resources, select **Monitoring + Management**.</span></span>  
    <span data-ttu-id="bde45-145">![Felügyelet és kezelés](./media/log-analytics-azure-sql/monitoring-management.png)</span><span class="sxs-lookup"><span data-stu-id="bde45-145">![Monitoring + Management](./media/log-analytics-azure-sql/monitoring-management.png)</span></span>
3. <span data-ttu-id="bde45-146">A hello **figyelés + felügyeleti** listában kattintson **láthatja az összes**.</span><span class="sxs-lookup"><span data-stu-id="bde45-146">In hello **Monitoring + Management** list click **See all**.</span></span>
4. <span data-ttu-id="bde45-147">A hello **ajánlott** listában, kattintson **további** , majd a hello új listában keresse meg **Azure SQL elemzés (előzetes verzió)** meg és jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="bde45-147">In hello **Recommended** list, click **More** , and then in hello new list, find **Azure SQL Analytics (Preview)** and then select it.</span></span>  
    <span data-ttu-id="bde45-148">![Az Azure SQL elemzési megoldások](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span><span class="sxs-lookup"><span data-stu-id="bde45-148">![Azure SQL Analytics solution](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span></span>
5. <span data-ttu-id="bde45-149">A hello **Azure SQL elemzés (előzetes verzió)** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bde45-149">In hello **Azure SQL Analytics (Preview)** blade, click **Create**.</span></span>  
    <span data-ttu-id="bde45-150">![Létrehozás](./media/log-analytics-azure-sql/portal-create.png)</span><span class="sxs-lookup"><span data-stu-id="bde45-150">![Create](./media/log-analytics-azure-sql/portal-create.png)</span></span>
6. <span data-ttu-id="bde45-151">A hello **hozzon létre új megoldás** panelen, jelölje be hello munkaterület tooadd hello megoldás tooand szeretné, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bde45-151">In hello **Create new solution** blade, select hello workspace that you want tooadd hello solution tooand then click **Create**.</span></span>  
    <span data-ttu-id="bde45-152">![tooworkspace hozzáadása](./media/log-analytics-azure-sql/add-to-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="bde45-152">![add tooworkspace](./media/log-analytics-azure-sql/add-to-workspace.png)</span></span>


### <a name="tooconfigure-multiple-azure-subscriptions"></a><span data-ttu-id="bde45-153">tooconfigure több Azure-előfizetéssel</span><span class="sxs-lookup"><span data-stu-id="bde45-153">tooconfigure multiple Azure subscriptions</span></span>

<span data-ttu-id="bde45-154">toosupport több előfizetéssel hello PowerShell parancsfájl használatát [engedélyezése Azure erőforrás metrikáit naplózás PowerShell-lel](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="bde45-154">toosupport multiple subscriptions, use hello PowerShell script from [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span> <span data-ttu-id="bde45-155">Adja meg a hello munkaterület erőforrás-azonosító paramétert hello parancsfájl toosend diagnosztikai adatokat egy Azure-előfizetéssel tooa munkaterületén más Azure-előfizetések erőforrásokból végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="bde45-155">Provide hello workspace resource ID as a parameter when executing hello script toosend diagnostic data from resources in one Azure subscription tooa workspace in another Azure subscription.</span></span>

<span data-ttu-id="bde45-156">**Példa**</span><span class="sxs-lookup"><span data-stu-id="bde45-156">**Example**</span></span>

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-hello-solution"></a><span data-ttu-id="bde45-157">Hello megoldással</span><span class="sxs-lookup"><span data-stu-id="bde45-157">Using hello solution</span></span>

<span data-ttu-id="bde45-158">Hello megoldás tooyour munkaterület hozzáadásakor hello Azure SQL elemzés csempe kerüljön tooyour munkaterület, és úgy tűnik, az Áttekintés.</span><span class="sxs-lookup"><span data-stu-id="bde45-158">When you add hello solution tooyour workspace, hello Azure SQL Analytics tile is added tooyour workspace, and it appears in Overview.</span></span> <span data-ttu-id="bde45-159">hello csempe az Azure SQL-adatbázisok és az Azure SQL rugalmas készletek hello megoldás kapcsolódó hello számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bde45-159">hello tile shows hello number of Azure SQL databases and Azure SQL elastic pools that hello solution is connected to.</span></span>

![Az Azure SQL elemzés csempe](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a><span data-ttu-id="bde45-161">Azure SQL analitikai adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="bde45-161">Viewing Azure SQL Analytics data</span></span>

<span data-ttu-id="bde45-162">Kattintson a hello **Azure SQL elemzés** csempe tooopen hello Azure SQL-elemzések irányítópultján.</span><span class="sxs-lookup"><span data-stu-id="bde45-162">Click on hello **Azure SQL Analytics** tile tooopen hello Azure SQL Analytics dashboard.</span></span> <span data-ttu-id="bde45-163">hello irányítópult tartalmazza az alábbiakban meghatározott hello paneleken.</span><span class="sxs-lookup"><span data-stu-id="bde45-163">hello dashboard includes hello blades defined below.</span></span> <span data-ttu-id="bde45-164">Minden egyes panel too15 erőforrások (előfizetés, kiszolgáló, a rugalmas készlet és adatbázis) sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="bde45-164">Each blade lists up too15 resources (subscription, server, elastic pool, and database).</span></span> <span data-ttu-id="bde45-165">Kattintson bármelyik hello erőforrások tooopen hello irányítópult, hogy az adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="bde45-165">Click any of hello resources tooopen hello dashboard for that specific resource.</span></span> <span data-ttu-id="bde45-166">A rugalmas készlet vagy adatbázis tartalmazza a hello diagramok a kiválasztott erőforrás metrikáit.</span><span class="sxs-lookup"><span data-stu-id="bde45-166">Elastic Pool or Database contains hello charts with metrics for a selected resource.</span></span> <span data-ttu-id="bde45-167">Kattintson a diagram tooopen hello napló keresése párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bde45-167">Click a chart tooopen hello Log Search dialog.</span></span>

| <span data-ttu-id="bde45-168">Panel</span><span class="sxs-lookup"><span data-stu-id="bde45-168">Blade</span></span> | <span data-ttu-id="bde45-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="bde45-169">Description</span></span> |
|---|---|
| <span data-ttu-id="bde45-170">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="bde45-170">Subscriptions</span></span> | <span data-ttu-id="bde45-171">Előfizetések számának csatlakoztatott kiszolgálók, a készletek és az adatbázisok listája.</span><span class="sxs-lookup"><span data-stu-id="bde45-171">List of subscriptions with number of connected servers, pools, and databases.</span></span> |
| <span data-ttu-id="bde45-172">Kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="bde45-172">Servers</span></span> | <span data-ttu-id="bde45-173">Kiszolgálók számának csatlakoztatott készletek és az adatbázisok listája.</span><span class="sxs-lookup"><span data-stu-id="bde45-173">List of servers with number of connected pools and databases.</span></span> |
| <span data-ttu-id="bde45-174">Rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="bde45-174">Elastic Pools</span></span> | <span data-ttu-id="bde45-175">A maximális GB és a megfigyelt hello eDTU csatlakoztatott rugalmas készletek listája időszak.</span><span class="sxs-lookup"><span data-stu-id="bde45-175">List of connected elastic pools with maximum GB and eDTU in hello observed period.</span></span> |
|<span data-ttu-id="bde45-176">Adatbázisok</span><span class="sxs-lookup"><span data-stu-id="bde45-176">Databases</span></span> | <span data-ttu-id="bde45-177">A megfigyelt hello maximális GB és DTU csatlakoztatott adatbázisok listája időszak.</span><span class="sxs-lookup"><span data-stu-id="bde45-177">List of connected databases with maximum GB and DTU in hello observed period.</span></span>|


### <a name="analyze-data-and-create-alerts"></a><span data-ttu-id="bde45-178">Adatok elemzése és értesítések</span><span class="sxs-lookup"><span data-stu-id="bde45-178">Analyze data and create alerts</span></span>

<span data-ttu-id="bde45-179">Az Azure SQL adatbázis-erőforrások érkező hello adatokkal könnyen riasztásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="bde45-179">You can easily create alerts with hello data coming from Azure SQL Database resources.</span></span> <span data-ttu-id="bde45-180">Íme néhány hasznos [naplófájl-keresési](log-analytics-log-searches.md) riasztások használható lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="bde45-180">Here are a couple of useful [log search](log-analytics-log-searches.md) queries that you can use for alerting:</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


<span data-ttu-id="bde45-181">*Az Azure SQL Database magas DTU*</span><span class="sxs-lookup"><span data-stu-id="bde45-181">*High DTU on Azure SQL Database*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="bde45-182">*Magas, az Azure SQL Database rugalmas készlet DTU*</span><span class="sxs-lookup"><span data-stu-id="bde45-182">*High DTU on Azure SQL Database Elastic Pool*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="bde45-183">A megadott küszöbértékek a riasztás-alapú lekérdezések tooalert használhatja az Azure SQL Database és a rugalmas készletek.</span><span class="sxs-lookup"><span data-stu-id="bde45-183">You can use these alert-based queries tooalert on specific thresholds for both Azure SQL Database and elastic pools.</span></span> <span data-ttu-id="bde45-184">tooconfigure egy riasztást az OMS-munkaterület:</span><span class="sxs-lookup"><span data-stu-id="bde45-184">tooconfigure an alert for your OMS workspace:</span></span>

#### <a name="tooconfigure-an-alert-for-your-workspace"></a><span data-ttu-id="bde45-185">a munkaterület riasztást tooconfigure</span><span class="sxs-lookup"><span data-stu-id="bde45-185">tooconfigure an alert for your workspace</span></span>

1. <span data-ttu-id="bde45-186">Nyissa meg toohello [OMS-portálon](http://mms.microsoft.com/) , jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="bde45-186">Go toohello [OMS portal](http://mms.microsoft.com/) and sign in.</span></span>
2. <span data-ttu-id="bde45-187">Nyissa meg a hello munkaterület hello megoldás konfigurált.</span><span class="sxs-lookup"><span data-stu-id="bde45-187">Open hello workspace that you have configured for hello solution.</span></span>
3. <span data-ttu-id="bde45-188">Hello áttekintése lapon kattintson a hello **Azure SQL elemzés (előzetes verzió)** csempére.</span><span class="sxs-lookup"><span data-stu-id="bde45-188">On hello Overview page, click hello **Azure SQL Analytics (Preview)** tile.</span></span>
4. <span data-ttu-id="bde45-189">Hello mintalekérdezéseket egyikét futtatja.</span><span class="sxs-lookup"><span data-stu-id="bde45-189">Run one of hello example queries.</span></span>
5. <span data-ttu-id="bde45-190">Kattintson a napló keresési **riasztási**.</span><span class="sxs-lookup"><span data-stu-id="bde45-190">In Log Search, click **Alert**.</span></span>  
<span data-ttu-id="bde45-191">![riasztás létrehozása, a keresés](./media/log-analytics-azure-sql/create-alert01.png)</span><span class="sxs-lookup"><span data-stu-id="bde45-191">![create alert in search](./media/log-analytics-azure-sql/create-alert01.png)</span></span>
6. <span data-ttu-id="bde45-192">A hello **riasztási szabály hozzáadása** lapon adja meg a megfelelő tulajdonságok hello és hello megadott küszöbértékek, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="bde45-192">On hello **Add Alert Rule** page, configure hello appropriate properties and hello specific thresholds that you want and then click **Save**.</span></span>  
<span data-ttu-id="bde45-193">![riasztási szabály hozzáadása](./media/log-analytics-azure-sql/create-alert02.png)</span><span class="sxs-lookup"><span data-stu-id="bde45-193">![add alert rule](./media/log-analytics-azure-sql/create-alert02.png)</span></span>

### <a name="act-on-azure-sql-analytics-data"></a><span data-ttu-id="bde45-194">Azure SQL analitikai adatok intézkedjen</span><span class="sxs-lookup"><span data-stu-id="bde45-194">Act on Azure SQL Analytics data</span></span>

<span data-ttu-id="bde45-195">Tegyük fel hello leghasznosabb lekérdezések hajthat végre egyik toocompare hello DTU-felhasználásban összes Azure SQL rugalmas készlet az összes Azure-előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="bde45-195">As an example, one of hello most useful queries that you can perform is toocompare hello DTU utilization for all Azure SQL Elastic Pools across all your Azure subscriptions.</span></span> <span data-ttu-id="bde45-196">Adatbázis átviteli egységek (DTU) tartalmaz egy módja toodescribe hello a Basic, Standard és Premium adatbázisok és a készletek teljesítményszintjének relatív kapacitását.</span><span class="sxs-lookup"><span data-stu-id="bde45-196">Database Throughput Unit (DTU) provides a way toodescribe hello relative capacity of a performance level of Basic, Standard, and Premium databases and pools.</span></span> <span data-ttu-id="bde45-197">Dtu-inak száma CPU egy kevert mérésén alapulnak, memória, olvas és ír.</span><span class="sxs-lookup"><span data-stu-id="bde45-197">DTUs are based on a blended measure of CPU, memory, reads, and writes.</span></span> <span data-ttu-id="bde45-198">Mivel Dtu növelje, hello szintű teljesítménynövekedést által kínált hello power.</span><span class="sxs-lookup"><span data-stu-id="bde45-198">As DTUs increase, hello power offered by hello performance level increases.</span></span> <span data-ttu-id="bde45-199">Például egy 5 dtu-val rendelkező teljesítményszintet áramellátása ötször több mint egy teljesítményszintet az 1 DTU.</span><span class="sxs-lookup"><span data-stu-id="bde45-199">For example, a performance level with 5 DTUs has five times more power than a performance level with 1 DTU.</span></span> <span data-ttu-id="bde45-200">A maximális DTU-kvótáról tooeach kiszolgáló és a rugalmas készlet vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="bde45-200">A maximum DTU quota applies tooeach server and elastic pool.</span></span>

<span data-ttu-id="bde45-201">A következő naplófájl-keresési lekérdezés hello futtatásával könnyen megállapítható, ha vannak kihasználatlanul hagy vagy az SQL Azure rugalmas készletek több mint használatával.</span><span class="sxs-lookup"><span data-stu-id="bde45-201">By running hello following Log Search query, you can easily tell if you are underutilizing or over utilizing your SQL Azure elastic pools.</span></span>

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> <span data-ttu-id="bde45-202">Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezés hello megváltozna toohello következő.</span><span class="sxs-lookup"><span data-stu-id="bde45-202">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then hello above query would change toohello following.</span></span>
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

<span data-ttu-id="bde45-203">A következő példa hello, láthatja, hogy rendelkezik-e egy magas kihasználtsága 100 %-os közelében egy rugalmas készlet DTU, míg mások csak nagyon kis használati rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="bde45-203">In hello following example, you can see that one elastic pool has a high usage near 100% DTU while others have very little usage.</span></span> <span data-ttu-id="bde45-204">További tootroubleshoot lehetséges legutóbbi módosításainak használatával megvizsgálhatja a Azure tevékenységi naplóit a környezetben.</span><span class="sxs-lookup"><span data-stu-id="bde45-204">You can investigate further tootroubleshoot potential recent changes in your environment using Azure Activity logs.</span></span>

![Naplófájl-keresési eredmények – magas kihasználtsága](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a><span data-ttu-id="bde45-206">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="bde45-206">See also</span></span>

- <span data-ttu-id="bde45-207">Használjon [napló keresések](log-analytics-log-searches.md) Naplóelemzési tooview részletes Azure SQL data.</span><span class="sxs-lookup"><span data-stu-id="bde45-207">Use [Log Searches](log-analytics-log-searches.md) in Log Analytics tooview detailed Azure SQL data.</span></span>
- <span data-ttu-id="bde45-208">[Hozzon létre egy saját irányítópultok](log-analytics-dashboards.md) Azure SQL-adatok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="bde45-208">[Create your own dashboards](log-analytics-dashboards.md) showing Azure SQL data.</span></span>
- <span data-ttu-id="bde45-209">[Hozzon létre a riasztások](log-analytics-alerts.md) Ha meghatározott Azure SQL-esemény történik.</span><span class="sxs-lookup"><span data-stu-id="bde45-209">[Create alerts](log-analytics-alerts.md) when specific Azure SQL events occur.</span></span>
