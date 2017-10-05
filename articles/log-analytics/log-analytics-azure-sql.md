---
title: "A Naplóelemzési Azure SQL elemzési megoldások |} Microsoft Docs"
description: "Az Azure SQL elemzési megoldások kezelheti az Azure SQL Database adatbázisok."
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
ms.openlocfilehash: cab45cc6dd621eb4a95ef5f1842ec38c25e980b6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a><span data-ttu-id="1702f-103">Azure SQL adatbázis Azure SQL elemzés (előzetes verzió) Naplóelemzési figyelése</span><span class="sxs-lookup"><span data-stu-id="1702f-103">Monitor Azure SQL Database using Azure SQL Analytics (Preview) in Log Analytics</span></span>

![Az Azure SQL elemzés szimbólum](./media/log-analytics-azure-sql/azure-sql-symbol.png)

<span data-ttu-id="1702f-105">Az Azure SQL elemzési megoldás az Azure Naplóelemzés gyűjt, és fontos SQL Azure teljesítménymutatók visualizes.</span><span class="sxs-lookup"><span data-stu-id="1702f-105">The Azure SQL Analytics solution in Azure Log Analytics collects and visualizes important SQL Azure performance metrics.</span></span> <span data-ttu-id="1702f-106">A metrikák gyűjtött megoldás segítségével létrehozhat egyéni ellenőrzési szabályok és értesítések.</span><span class="sxs-lookup"><span data-stu-id="1702f-106">By using the metrics that you collect with the solution, you can create custom monitoring rules and alerts.</span></span> <span data-ttu-id="1702f-107">Figyelheti, hogy az Azure SQL Database és a rugalmas készlet metrikák több Azure-előfizetések és rugalmas készletek, és jelenítheti meg őket.</span><span class="sxs-lookup"><span data-stu-id="1702f-107">And, you can monitor Azure SQL Database and elastic pool metrics across multiple Azure subscriptions and elastic pools and visualize them.</span></span> <span data-ttu-id="1702f-108">A megoldás is segítséget az alkalmazás-készlet minden egyes rétegben problémák azonosításához.</span><span class="sxs-lookup"><span data-stu-id="1702f-108">The solution also helps you to identify issues at each layer of your application stack.</span></span>  <span data-ttu-id="1702f-109">Használja [Azure diagnosztikai metrikák](log-analytics-azure-storage.md) Log Analytics-nézetet megjeleníteni az adatokat az Azure SQL-adatbázisok és rugalmas készletek egyetlen Naplóelemzési munkaterület együtt.</span><span class="sxs-lookup"><span data-stu-id="1702f-109">It uses [Azure Diagnostic metrics](log-analytics-azure-storage.md) together with Log Analytics views to present data about all your Azure SQL databases and elastic pools in a single Log Analytics workspace.</span></span>

<span data-ttu-id="1702f-110">Jelenleg ez preview megoldás legfeljebb 150 000 Azure SQL-adatbázisok és 5000 SQL rugalmas készletek / munkaterület támogatja.</span><span class="sxs-lookup"><span data-stu-id="1702f-110">Currently, this preview solution supports up to 150,000 Azure SQL Databases and 5,000 SQL Elastic Pools per workspace.</span></span>

<span data-ttu-id="1702f-111">Az Azure SQL elemzési megoldás, mások számára elérhető Naplóelemzési, például segítségével megfigyelheti és az Azure-erőforrások értesítéseket állapotával kapcsolatos – ebben az esetben az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1702f-111">The Azure SQL Analytics solution, like others available for Log Analytics, helps you monitor and receive notifications about the health of your Azure resources—in this case, Azure SQL Database.</span></span> <span data-ttu-id="1702f-112">A Microsoft Azure SQL Database egy méretezhető relációs adatbázis-szolgáltatás, amely az Azure felhőben futó alkalmazások ismerős SQL-kiszolgáló-szerű képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="1702f-112">Microsoft Azure SQL Database is a scalable relational database service that provides familiar SQL-Server-like capabilities to applications running in the Azure cloud.</span></span> <span data-ttu-id="1702f-113">A Naplóelemzési segítségével gyűjtése, összefüggéseket és strukturált és strukturálatlan adatok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="1702f-113">Log Analytics helps you to collect, correlate, and visualize structured and unstructured data.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="1702f-114">Összekapcsolt források</span><span class="sxs-lookup"><span data-stu-id="1702f-114">Connected sources</span></span>

<span data-ttu-id="1702f-115">Az Azure SQL elemzési megoldások ügynökök nem használ, a Naplóelemzés szolgáltatáshoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="1702f-115">The Azure SQL Analytics solution doesn't use agents to connect to the Log Analytics service.</span></span>

<span data-ttu-id="1702f-116">Az alábbi táblázat áttekintést nyújt az ebben a megoldásban támogatott összekapcsolt forrásokról.</span><span class="sxs-lookup"><span data-stu-id="1702f-116">The following table describes the connected sources that are supported by this solution.</span></span>

| <span data-ttu-id="1702f-117">Összekapcsolt forrás</span><span class="sxs-lookup"><span data-stu-id="1702f-117">Connected Source</span></span> | <span data-ttu-id="1702f-118">Támogatás</span><span class="sxs-lookup"><span data-stu-id="1702f-118">Support</span></span> | <span data-ttu-id="1702f-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="1702f-119">Description</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="1702f-120">Windows-ügynökök</span><span class="sxs-lookup"><span data-stu-id="1702f-120">Windows agents</span></span>](log-analytics-windows-agents.md) | <span data-ttu-id="1702f-121">Nem</span><span class="sxs-lookup"><span data-stu-id="1702f-121">No</span></span> | <span data-ttu-id="1702f-122">A közvetlen Windows-ügynökök nem használják a megoldás.</span><span class="sxs-lookup"><span data-stu-id="1702f-122">Direct Windows agents are not used by the solution.</span></span> |
| [<span data-ttu-id="1702f-123">Linux-ügynökök</span><span class="sxs-lookup"><span data-stu-id="1702f-123">Linux agents</span></span>](log-analytics-linux-agents.md) | <span data-ttu-id="1702f-124">Nem</span><span class="sxs-lookup"><span data-stu-id="1702f-124">No</span></span> | <span data-ttu-id="1702f-125">Közvetlen Linux-ügynökök nem használják a megoldás.</span><span class="sxs-lookup"><span data-stu-id="1702f-125">Direct Linux agents are not used by the solution.</span></span> |
| [<span data-ttu-id="1702f-126">SCOM felügyeleti csoport</span><span class="sxs-lookup"><span data-stu-id="1702f-126">SCOM management group</span></span>](log-analytics-om-agents.md) | <span data-ttu-id="1702f-127">Nem</span><span class="sxs-lookup"><span data-stu-id="1702f-127">No</span></span> | <span data-ttu-id="1702f-128">Közvetlen kapcsolat az SCOM-ügynököt a szolgáltatáshoz a megoldás nem használja.</span><span class="sxs-lookup"><span data-stu-id="1702f-128">A direct connection from the SCOM agent to Log Analytics is not used by the solution.</span></span> |
| [<span data-ttu-id="1702f-129">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="1702f-129">Azure storage account</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="1702f-130">Nem</span><span class="sxs-lookup"><span data-stu-id="1702f-130">No</span></span> | <span data-ttu-id="1702f-131">A Naplóelemzési nem beolvasni az adatokat egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="1702f-131">Log Analytics does not read the data from a storage account.</span></span> |
| [<span data-ttu-id="1702f-132">Az Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="1702f-132">Azure diagnostics</span></span>](log-analytics-azure-storage.md) | <span data-ttu-id="1702f-133">Igen</span><span class="sxs-lookup"><span data-stu-id="1702f-133">Yes</span></span> | <span data-ttu-id="1702f-134">Azure metrikaadatokat közvetlenül az Azure Log Analytics érkezik.</span><span class="sxs-lookup"><span data-stu-id="1702f-134">Azure metric data is sent to Log Analytics directly by Azure.</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="1702f-135">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1702f-135">Prerequisites</span></span>

- <span data-ttu-id="1702f-136">Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="1702f-136">An Azure Subscription.</span></span> <span data-ttu-id="1702f-137">Ha még nincs fiókja, létrehozhat egy a [szabad](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1702f-137">If you don't have one, you can create one for [free](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="1702f-138">A Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="1702f-138">A Log Analytics workspace.</span></span> <span data-ttu-id="1702f-139">Egy meglévő is használhatja, vagy beállíthatja a [hozzon létre egy újat](log-analytics-get-started.md) Ez a megoldás használata előtt.</span><span class="sxs-lookup"><span data-stu-id="1702f-139">You can use an existing one, or you can [create a new one](log-analytics-get-started.md) before you start using this solution.</span></span>
- <span data-ttu-id="1702f-140">Azure Diagnostics engedélyezése az Azure SQL-adatbázisok és rugalmas készletek és [úgy konfigurálja azokat az adatokat küldeni a Naplóelemzési](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="1702f-140">Enable Azure Diagnostics for your Azure SQL databases and elastic pools and [configure them to send their data to Log Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span>

## <a name="configuration"></a><span data-ttu-id="1702f-141">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="1702f-141">Configuration</span></span>

<span data-ttu-id="1702f-142">A következő lépésekkel adja hozzá az Azure SQL elemzési megoldások a munkaterületre.</span><span class="sxs-lookup"><span data-stu-id="1702f-142">Perform the following steps to add the Azure SQL Analytics solution to your workspace.</span></span>

1. <span data-ttu-id="1702f-143">Az Azure SQL elemzési megoldások hozzáadása a munkaterület [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot követve [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="1702f-143">Add the Azure SQL Analytics solution to your workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="1702f-144">Az Azure portálon kattintson **új** (a + szimbólumra), majd válassza az erőforrások listájához, **figyelés + felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="1702f-144">In the Azure portal, click **New** (the + symbol), then in the list of resources, select **Monitoring + Management**.</span></span>  
    <span data-ttu-id="1702f-145">![Felügyelet és kezelés](./media/log-analytics-azure-sql/monitoring-management.png)</span><span class="sxs-lookup"><span data-stu-id="1702f-145">![Monitoring + Management](./media/log-analytics-azure-sql/monitoring-management.png)</span></span>
3. <span data-ttu-id="1702f-146">Az a **figyelés + felügyeleti** listában kattintson **láthatja az összes**.</span><span class="sxs-lookup"><span data-stu-id="1702f-146">In the **Monitoring + Management** list click **See all**.</span></span>
4. <span data-ttu-id="1702f-147">Az a **ajánlott** listában, kattintson **további** , majd új listájában keresse meg **Azure SQL elemzés (előzetes verzió)** meg és jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="1702f-147">In the **Recommended** list, click **More** , and then in the new list, find **Azure SQL Analytics (Preview)** and then select it.</span></span>  
    <span data-ttu-id="1702f-148">![Az Azure SQL elemzési megoldások](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span><span class="sxs-lookup"><span data-stu-id="1702f-148">![Azure SQL Analytics solution](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)</span></span>
5. <span data-ttu-id="1702f-149">Az a **Azure SQL elemzés (előzetes verzió)** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1702f-149">In the **Azure SQL Analytics (Preview)** blade, click **Create**.</span></span>  
    <span data-ttu-id="1702f-150">![Létrehozás](./media/log-analytics-azure-sql/portal-create.png)</span><span class="sxs-lookup"><span data-stu-id="1702f-150">![Create](./media/log-analytics-azure-sql/portal-create.png)</span></span>
6. <span data-ttu-id="1702f-151">Az a **hozzon létre új megoldás** panelen, válassza ki, amely hozzá szeretné adni, hogy a munkaterület, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1702f-151">In the **Create new solution** blade, select the workspace that you want to add the solution to and then click **Create**.</span></span>  
    <span data-ttu-id="1702f-152">![munkaterület felvétele](./media/log-analytics-azure-sql/add-to-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="1702f-152">![add to workspace](./media/log-analytics-azure-sql/add-to-workspace.png)</span></span>


### <a name="to-configure-multiple-azure-subscriptions"></a><span data-ttu-id="1702f-153">Több Azure-előfizetések beállítása</span><span class="sxs-lookup"><span data-stu-id="1702f-153">To configure multiple Azure subscriptions</span></span>

<span data-ttu-id="1702f-154">Több előfizetések támogatásához a PowerShell parancsfájl használatát [engedélyezése Azure erőforrás metrikáit naplózás PowerShell-lel](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span><span class="sxs-lookup"><span data-stu-id="1702f-154">To support multiple subscriptions, use the PowerShell script from [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).</span></span> <span data-ttu-id="1702f-155">Adja meg a munkaterület-azonosító paramétert, a diagnosztikai adatokat küldeni a források egy Azure-előfizetésben más Azure-előfizetések munkaterületeinek a parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="1702f-155">Provide the workspace resource ID as a parameter when executing the script to send diagnostic data from resources in one Azure subscription to a workspace in another Azure subscription.</span></span>

<span data-ttu-id="1702f-156">**Példa**</span><span class="sxs-lookup"><span data-stu-id="1702f-156">**Example**</span></span>

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a><span data-ttu-id="1702f-157">A megoldás használata</span><span class="sxs-lookup"><span data-stu-id="1702f-157">Using the solution</span></span>

<span data-ttu-id="1702f-158">A megoldás a munkaterülethez való hozzáadásakor az Azure SQL elemzés csempe hozzáadódik a munkaterület, és megjeleníti a áttekintése.</span><span class="sxs-lookup"><span data-stu-id="1702f-158">When you add the solution to your workspace, the Azure SQL Analytics tile is added to your workspace, and it appears in Overview.</span></span> <span data-ttu-id="1702f-159">A csempe az Azure SQL-adatbázisok és az Azure SQL rugalmas készletek a megoldás csatlakozik a számát mutatja.</span><span class="sxs-lookup"><span data-stu-id="1702f-159">The tile shows the number of Azure SQL databases and Azure SQL elastic pools that the solution is connected to.</span></span>

![Az Azure SQL elemzés csempe](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a><span data-ttu-id="1702f-161">Azure SQL analitikai adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="1702f-161">Viewing Azure SQL Analytics data</span></span>

<span data-ttu-id="1702f-162">Kattintson a **Azure SQL elemzés** csempére kattintva nyissa meg az Azure SQL-elemzések irányítópultján.</span><span class="sxs-lookup"><span data-stu-id="1702f-162">Click on the **Azure SQL Analytics** tile to open the Azure SQL Analytics dashboard.</span></span> <span data-ttu-id="1702f-163">Az irányítópult az alább megadott paneleken tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1702f-163">The dashboard includes the blades defined below.</span></span> <span data-ttu-id="1702f-164">Minden egyes panel legfeljebb 15 források (előfizetés, kiszolgáló, a rugalmas készlet és adatbázis) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1702f-164">Each blade lists up to 15 resources (subscription, server, elastic pool, and database).</span></span> <span data-ttu-id="1702f-165">Kattintson az erőforrásokat, hogy az adott erőforráshoz az irányítópult megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="1702f-165">Click any of the resources to open the dashboard for that specific resource.</span></span> <span data-ttu-id="1702f-166">A rugalmas készlet vagy adatbázis tartalmazza a kiválasztott erőforrás metrikáit rendelkező diagramok.</span><span class="sxs-lookup"><span data-stu-id="1702f-166">Elastic Pool or Database contains the charts with metrics for a selected resource.</span></span> <span data-ttu-id="1702f-167">Kattintson a diagram a naplófájl-keresési párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="1702f-167">Click a chart to open the Log Search dialog.</span></span>

| <span data-ttu-id="1702f-168">Panel</span><span class="sxs-lookup"><span data-stu-id="1702f-168">Blade</span></span> | <span data-ttu-id="1702f-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="1702f-169">Description</span></span> |
|---|---|
| <span data-ttu-id="1702f-170">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="1702f-170">Subscriptions</span></span> | <span data-ttu-id="1702f-171">Előfizetések számának csatlakoztatott kiszolgálók, a készletek és az adatbázisok listája.</span><span class="sxs-lookup"><span data-stu-id="1702f-171">List of subscriptions with number of connected servers, pools, and databases.</span></span> |
| <span data-ttu-id="1702f-172">Kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="1702f-172">Servers</span></span> | <span data-ttu-id="1702f-173">Kiszolgálók számának csatlakoztatott készletek és az adatbázisok listája.</span><span class="sxs-lookup"><span data-stu-id="1702f-173">List of servers with number of connected pools and databases.</span></span> |
| <span data-ttu-id="1702f-174">Rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="1702f-174">Elastic Pools</span></span> | <span data-ttu-id="1702f-175">A maximális GB és edtu-ra a vizsgált időszakban csatlakoztatott rugalmas készletek listája.</span><span class="sxs-lookup"><span data-stu-id="1702f-175">List of connected elastic pools with maximum GB and eDTU in the observed period.</span></span> |
|<span data-ttu-id="1702f-176">Adatbázisok</span><span class="sxs-lookup"><span data-stu-id="1702f-176">Databases</span></span> | <span data-ttu-id="1702f-177">A vizsgált időszakban maximális GB és DTU csatlakoztatott adatbázisok listája.</span><span class="sxs-lookup"><span data-stu-id="1702f-177">List of connected databases with maximum GB and DTU in the observed period.</span></span>|


### <a name="analyze-data-and-create-alerts"></a><span data-ttu-id="1702f-178">Adatok elemzése és értesítések</span><span class="sxs-lookup"><span data-stu-id="1702f-178">Analyze data and create alerts</span></span>

<span data-ttu-id="1702f-179">Az Azure SQL Database-forrásokból származó adatokat tartalmazó könnyen riasztásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="1702f-179">You can easily create alerts with the data coming from Azure SQL Database resources.</span></span> <span data-ttu-id="1702f-180">Íme néhány hasznos [naplófájl-keresési](log-analytics-log-searches.md) riasztások használható lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="1702f-180">Here are a couple of useful [log search](log-analytics-log-searches.md) queries that you can use for alerting:</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


<span data-ttu-id="1702f-181">*Az Azure SQL Database magas DTU*</span><span class="sxs-lookup"><span data-stu-id="1702f-181">*High DTU on Azure SQL Database*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="1702f-182">*Magas, az Azure SQL Database rugalmas készlet DTU*</span><span class="sxs-lookup"><span data-stu-id="1702f-182">*High DTU on Azure SQL Database Elastic Pool*</span></span>

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

<span data-ttu-id="1702f-183">A riasztás-alapú lekérdezések segítségével az Azure SQL Database és a rugalmas készletek megadott küszöbértékek riasztás.</span><span class="sxs-lookup"><span data-stu-id="1702f-183">You can use these alert-based queries to alert on specific thresholds for both Azure SQL Database and elastic pools.</span></span> <span data-ttu-id="1702f-184">Az OMS-munkaterület a riasztás konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="1702f-184">To configure an alert for your OMS workspace:</span></span>

#### <a name="to-configure-an-alert-for-your-workspace"></a><span data-ttu-id="1702f-185">A munkaterület riasztás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1702f-185">To configure an alert for your workspace</span></span>

1. <span data-ttu-id="1702f-186">Lépjen a [OMS-portálon](http://mms.microsoft.com/) , jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="1702f-186">Go to the [OMS portal](http://mms.microsoft.com/) and sign in.</span></span>
2. <span data-ttu-id="1702f-187">Nyissa meg a munkaterületet, a megoldás konfigurált.</span><span class="sxs-lookup"><span data-stu-id="1702f-187">Open the workspace that you have configured for the solution.</span></span>
3. <span data-ttu-id="1702f-188">A áttekintése lapon kattintson a **Azure SQL elemzés (előzetes verzió)** csempére.</span><span class="sxs-lookup"><span data-stu-id="1702f-188">On the Overview page, click the **Azure SQL Analytics (Preview)** tile.</span></span>
4. <span data-ttu-id="1702f-189">A példa lekérdezések egyikét futtatja.</span><span class="sxs-lookup"><span data-stu-id="1702f-189">Run one of the example queries.</span></span>
5. <span data-ttu-id="1702f-190">Kattintson a napló keresési **riasztási**.</span><span class="sxs-lookup"><span data-stu-id="1702f-190">In Log Search, click **Alert**.</span></span>  
<span data-ttu-id="1702f-191">![riasztás létrehozása, a keresés](./media/log-analytics-azure-sql/create-alert01.png)</span><span class="sxs-lookup"><span data-stu-id="1702f-191">![create alert in search](./media/log-analytics-azure-sql/create-alert01.png)</span></span>
6. <span data-ttu-id="1702f-192">Az a **riasztási szabály hozzáadása** lapján konfigurálja a megfelelő tulajdonságokat és a meghatározott küszöbértékeket, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1702f-192">On the **Add Alert Rule** page, configure the appropriate properties and the specific thresholds that you want and then click **Save**.</span></span>  
<span data-ttu-id="1702f-193">![riasztási szabály hozzáadása](./media/log-analytics-azure-sql/create-alert02.png)</span><span class="sxs-lookup"><span data-stu-id="1702f-193">![add alert rule](./media/log-analytics-azure-sql/create-alert02.png)</span></span>

### <a name="act-on-azure-sql-analytics-data"></a><span data-ttu-id="1702f-194">Azure SQL analitikai adatok intézkedjen</span><span class="sxs-lookup"><span data-stu-id="1702f-194">Act on Azure SQL Analytics data</span></span>

<span data-ttu-id="1702f-195">Például végezheti el a leghasznosabb lekérdezések egyik hasonlítsa össze az összes Azure SQL rugalmas készlet DTU-felhasználásban minden Azure-előfizetés között.</span><span class="sxs-lookup"><span data-stu-id="1702f-195">As an example, one of the most useful queries that you can perform is to compare the DTU utilization for all Azure SQL Elastic Pools across all your Azure subscriptions.</span></span> <span data-ttu-id="1702f-196">Adatbázis átviteli egységek (DTU) a Basic, Standard és Premium adatbázisok és a készletek teljesítményszintjének relatív kapacitását lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="1702f-196">Database Throughput Unit (DTU) provides a way to describe the relative capacity of a performance level of Basic, Standard, and Premium databases and pools.</span></span> <span data-ttu-id="1702f-197">Dtu-inak száma CPU egy kevert mérésén alapulnak, memória, olvas és ír.</span><span class="sxs-lookup"><span data-stu-id="1702f-197">DTUs are based on a blended measure of CPU, memory, reads, and writes.</span></span> <span data-ttu-id="1702f-198">Dtu növelje, a teljesítmény a teljesítményszintet által kínált növekszik.</span><span class="sxs-lookup"><span data-stu-id="1702f-198">As DTUs increase, the power offered by the performance level increases.</span></span> <span data-ttu-id="1702f-199">Például egy 5 dtu-val rendelkező teljesítményszintet áramellátása ötször több mint egy teljesítményszintet az 1 DTU.</span><span class="sxs-lookup"><span data-stu-id="1702f-199">For example, a performance level with 5 DTUs has five times more power than a performance level with 1 DTU.</span></span> <span data-ttu-id="1702f-200">A maximális DTU-kvótát minden egyes kiszolgáló és a rugalmas készlet vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="1702f-200">A maximum DTU quota applies to each server and elastic pool.</span></span>

<span data-ttu-id="1702f-201">A következő naplófájl-keresési lekérdezés futtatásával könnyen megállapítható, ha a kihasználatlanul vannak hagy, vagy az SQL Azure rugalmas készletek okhoz keresztül.</span><span class="sxs-lookup"><span data-stu-id="1702f-201">By running the following Log Search query, you can easily tell if you are underutilizing or over utilizing your SQL Azure elastic pools.</span></span>

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> <span data-ttu-id="1702f-202">Ha a munkaterületet lett frissítve a [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezés megváltozna a következők.</span><span class="sxs-lookup"><span data-stu-id="1702f-202">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above query would change to the following.</span></span>
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

<span data-ttu-id="1702f-203">A következő példában láthatja, hogy rendelkezik-e egy magas kihasználtsága 100 %-os közelében egy rugalmas készlet DTU, míg mások csak nagyon kis használati rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="1702f-203">In the following example, you can see that one elastic pool has a high usage near 100% DTU while others have very little usage.</span></span> <span data-ttu-id="1702f-204">Használatával megvizsgálhatja a további lehetséges legutóbbi módosítások Azure tevékenység-naplók segítségével környezetében elhárítása.</span><span class="sxs-lookup"><span data-stu-id="1702f-204">You can investigate further to troubleshoot potential recent changes in your environment using Azure Activity logs.</span></span>

![Naplófájl-keresési eredmények – magas kihasználtsága](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a><span data-ttu-id="1702f-206">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1702f-206">See also</span></span>

- <span data-ttu-id="1702f-207">Használjon [napló keresések](log-analytics-log-searches.md) a Log Analyticshez Azure SQL részletes adatainak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="1702f-207">Use [Log Searches](log-analytics-log-searches.md) in Log Analytics to view detailed Azure SQL data.</span></span>
- <span data-ttu-id="1702f-208">[Hozzon létre egy saját irányítópultok](log-analytics-dashboards.md) Azure SQL-adatok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="1702f-208">[Create your own dashboards](log-analytics-dashboards.md) showing Azure SQL data.</span></span>
- <span data-ttu-id="1702f-209">[Hozzon létre a riasztások](log-analytics-alerts.md) Ha meghatározott Azure SQL-esemény történik.</span><span class="sxs-lookup"><span data-stu-id="1702f-209">[Create alerts](log-analytics-alerts.md) when specific Azure SQL events occur.</span></span>
