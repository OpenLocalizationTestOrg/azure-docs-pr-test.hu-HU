---
title: "Az Azure SQL adatbázis metrikák & diagnosztikai naplózás |} Microsoft Docs"
description: "Ismerje meg az erőforrás-használat, a kapcsolat és a lekérdezés végrehajtási statisztika tárolására az Azure SQL Database-erőforrás konfigurálása."
services: sql-database
documentationcenter: 
author: vvasic
manager: jhubbard
editor: 
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: vvasic
ms.openlocfilehash: bf41aa530c68ea0e94a09d1dab63237c6f42bce7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a><span data-ttu-id="b11eb-103">Az Azure SQL Database metrikák és diagnosztikai naplózás</span><span class="sxs-lookup"><span data-stu-id="b11eb-103">Azure SQL Database metrics and diagnostics logging</span></span> 
<span data-ttu-id="b11eb-104">Az Azure SQL-adatbázis el tudná küldeni, metrikákat és egyszerűbb figyelési diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="b11eb-104">Azure SQL Database can emit metrics and diagnostic logs for easier monitoring.</span></span> <span data-ttu-id="b11eb-105">Konfigurálhatja az Azure SQL-adatbázis tárolásához, erőforrás-használat, a dolgozók és a munkamenetek és a kapcsolat valamelyikébe ezek Azure-erőforrások:</span><span class="sxs-lookup"><span data-stu-id="b11eb-105">You can configure Azure SQL Database to store resource usage, workers and sessions, and connectivity into one of these Azure resources:</span></span>
- <span data-ttu-id="b11eb-106">**Azure Storage**: Nagy tömegű telemetriai adat alacsony költségű archiválására</span><span class="sxs-lookup"><span data-stu-id="b11eb-106">**Azure Storage**: For archiving vast amounts of telemetry for a small price</span></span>
- <span data-ttu-id="b11eb-107">**Az Azure Event Hubs**: az Azure SQL Database telemetriai integrálása a figyelési megoldást igényelnek egyéni vagy a működés közbeni folyamatok</span><span class="sxs-lookup"><span data-stu-id="b11eb-107">**Azure Event Hub**: For integrating Azure SQL Database telemetry with your custom monitoring solution or hot pipelines</span></span>
- <span data-ttu-id="b11eb-108">**Az Azure Naplóelemzés**: az a felügyeleti megoldás reporting, riasztás és képességek kiküszöböléséhez kezdő verzióról</span><span class="sxs-lookup"><span data-stu-id="b11eb-108">**Azure Log Analytics**: For out of the box monitoring solution with reporting, alerting, and mitigating capabilities</span></span> 

    ![architektúra](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a><span data-ttu-id="b11eb-110">Naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b11eb-110">Enable logging</span></span>

<span data-ttu-id="b11eb-111">Metrikák és diagnosztikai naplózás alapértelmezés szerint nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="b11eb-111">Metrics and diagnostics logging is not enabled by default.</span></span> <span data-ttu-id="b11eb-112">Engedélyezi, és kezelheti a metrikák és a naplózás a következő módszerek egyikét használva diagnosztika:</span><span class="sxs-lookup"><span data-stu-id="b11eb-112">You can enable and manage metrics and diagnostics logging using one of the following methods:</span></span>
- <span data-ttu-id="b11eb-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b11eb-113">Azure portal</span></span>
- <span data-ttu-id="b11eb-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b11eb-114">PowerShell</span></span>
- <span data-ttu-id="b11eb-115">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b11eb-115">Azure CLI</span></span>
- <span data-ttu-id="b11eb-116">REST API</span><span class="sxs-lookup"><span data-stu-id="b11eb-116">REST API</span></span> 
- <span data-ttu-id="b11eb-117">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="b11eb-117">Resource Manager template</span></span>

<span data-ttu-id="b11eb-118">Metrikák és diagnosztikai naplózás engedélyezése esetén meg kell adnia a Azure erőforráscsoport, ahol a kijelölt adatok gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="b11eb-118">When you enable metrics and diagnostics logging, you need to specify the Azure resource where selected data is collected.</span></span> <span data-ttu-id="b11eb-119">Elérhető lehetőségek:</span><span class="sxs-lookup"><span data-stu-id="b11eb-119">Options available:</span></span>
- <span data-ttu-id="b11eb-120">Log Analytics</span><span class="sxs-lookup"><span data-stu-id="b11eb-120">Log analytics</span></span>
- <span data-ttu-id="b11eb-121">Eseményközpont</span><span class="sxs-lookup"><span data-stu-id="b11eb-121">Event Hub</span></span>
- <span data-ttu-id="b11eb-122">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b11eb-122">Azure Storage</span></span> 

<span data-ttu-id="b11eb-123">Új Azure-erőforrás kiépítése, vagy jelöljön ki egy meglévő erőforrást.</span><span class="sxs-lookup"><span data-stu-id="b11eb-123">You can provision a new Azure resource or select an existing resource.</span></span> <span data-ttu-id="b11eb-124">Miután kiválasztotta a tárolási erőforrások, meg kell adnia az összegyűjtendő adatok.</span><span class="sxs-lookup"><span data-stu-id="b11eb-124">After selecting the storage resource, you need to specify which data to collect.</span></span> <span data-ttu-id="b11eb-125">Elérhető lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="b11eb-125">Options available include:</span></span>

- <span data-ttu-id="b11eb-126">**[1 perces metrikák](sql-database-metrics-diag-logging.md#1-minute-metrics)**  -DTU százaléka, a DTU határt, a Processzor százalékban tartalmaz napló írása fizikai adatot olvasott a következő százalékos aránya, százalékos, sikeres vagy sikertelen/letiltott tűzfalkapcsolatok, munkamenetek százalékos, munkavállalók százalékos, tárolási, tárolási százalékos, XTP tárolási százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="b11eb-126">**[1-minute metrics](sql-database-metrics-diag-logging.md#1-minute-metrics)** - contains DTU percentage, DTU limit, CPU percentage, Physical data read percentage, Log write percentage, Successful/Failed/Blocked by firewall connections, sessions percentage, workers percentage, storage, storage percentage, XTP storage percentage</span></span>

<span data-ttu-id="b11eb-127">Eseményközpont vagy egy AzureStorage fiókot ad meg, ha megadhat egy megőrzési házirend régebbi, mint a kijelölt időszakot törlődik adatok megadásához.</span><span class="sxs-lookup"><span data-stu-id="b11eb-127">If you specify Event Hub or an AzureStorage account, you can specify a retention policy to specify that data that is older than a selected time period is deleted.</span></span> <span data-ttu-id="b11eb-128">A Naplóelemzési ad meg, ha az adatmegőrzési függ a kijelölt tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="b11eb-128">If you specify Log Analytics, the retention policy depends on the selected pricing tier.</span></span> <span data-ttu-id="b11eb-129">Tudjon meg többet az [Naplóelemzési árképzési](https://azure.microsoft.com/pricing/details/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="b11eb-129">Read more about [Log Analytics pricing](https://azure.microsoft.com/pricing/details/log-analytics/).</span></span> 

<span data-ttu-id="b11eb-130">Azt javasoljuk, hogy olvassa el is a [áttekintése a Microsoft Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md) és [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) felmérheti, nem csak hogyan engedélyezze a naplózást, de a metrikák és a napló kategóriák különböző Azure-szolgáltatás által támogatott cikkeket.</span><span class="sxs-lookup"><span data-stu-id="b11eb-130">We recommend that you read both the [Overview of metrics in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) and [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) articles to gain an understanding of not only how to enable logging, but the metrics and log categories supported by the various Azure services.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="b11eb-131">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b11eb-131">Azure portal</span></span>

<span data-ttu-id="b11eb-132">Metrikák és az Azure portálon diagnosztikai naplók gyűjtemény engedélyezéséhez navigáljon az Azure SQL adatbázis vagy a rugalmas készlet lap, és kattintson **diagnosztikai beállítások**.</span><span class="sxs-lookup"><span data-stu-id="b11eb-132">To enable metrics and diagnostic logs collection in the Azure portal, navigate to your Azure SQL database or elastic pool page, and then click **Diagnostic settings**.</span></span>

   ![az Azure portálon engedélyezése](./media/sql-database-metrics-diag-logging/enable-portal.png)

### <a name="powershell"></a><span data-ttu-id="b11eb-134">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b11eb-134">PowerShell</span></span>

<span data-ttu-id="b11eb-135">Metrikák és a PowerShell használatával diagnosztikai naplózás engedélyezéséhez használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="b11eb-135">To enable metrics and diagnostics logging using PowerShell, use the following commands:</span></span>

- <span data-ttu-id="b11eb-136">Ahhoz, hogy a Storage-fiókok a diagnosztikai naplók tárolására, az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="b11eb-136">To enable storage of Diagnostic Logs in a Storage Account, use this command:</span></span>

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   <span data-ttu-id="b11eb-137">A Tárfiók azonosítója az erőforrás-azonosítója, amelyhez hozzá szeretné küldeni a naplókat a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b11eb-137">The Storage Account ID is the resource id for the storage account to which you want to send the logs.</span></span>

- <span data-ttu-id="b11eb-138">Adatfolyamként való küldése a diagnosztikai naplók Eseményközpontokba való engedélyezéséhez az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="b11eb-138">To enable streaming of Diagnostic Logs to an Event Hub, use this command:</span></span>

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   <span data-ttu-id="b11eb-139">A Service Bus Szabályazonosító egy ilyen formátumú karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="b11eb-139">The Service Bus Rule ID is a string with this format:</span></span>

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- <span data-ttu-id="b11eb-140">A Naplóelemzési munkaterület elküldését a diagnosztikai naplók engedélyezéséhez az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="b11eb-140">To enable sending of Diagnostic Logs to a Log Analytics workspace, use this command:</span></span>

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- <span data-ttu-id="b11eb-141">Az erőforrás-azonosítója a Naplóelemzési munkaterület a következő paranccsal szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="b11eb-141">You can obtain the resource id of your Log Analytics workspace using the following command:</span></span>

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

<span data-ttu-id="b11eb-142">Ezek a paraméterek ahhoz, hogy több kimenet beállításai kombinálhatja.</span><span class="sxs-lookup"><span data-stu-id="b11eb-142">You can combine these parameters to enable multiple output options.</span></span>

### <a name="cli"></a><span data-ttu-id="b11eb-143">parancssori felület</span><span class="sxs-lookup"><span data-stu-id="b11eb-143">CLI</span></span>

<span data-ttu-id="b11eb-144">Metrikák és az Azure parancssori felület használatával diagnosztikai naplózás engedélyezéséhez használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="b11eb-144">To enable metrics and diagnostics logging using the Azure CLI, use the following commands:</span></span>

- <span data-ttu-id="b11eb-145">Ahhoz, hogy a Storage-fiókok a diagnosztikai naplók tárolására, az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="b11eb-145">To enable storage of Diagnostic Logs in a Storage Account, use this command:</span></span>

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   <span data-ttu-id="b11eb-146">A Tárfiók azonosítója az erőforrás-azonosítója, amelyhez hozzá szeretné küldeni a naplókat a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b11eb-146">The Storage Account ID is the resource id for the storage account to which you want to send the logs.</span></span>

- <span data-ttu-id="b11eb-147">Adatfolyamként való küldése a diagnosztikai naplók Eseményközpontokba való engedélyezéséhez az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="b11eb-147">To enable streaming of Diagnostic Logs to an Event Hub, use this command:</span></span>

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   <span data-ttu-id="b11eb-148">A Service Bus Szabályazonosító egy ilyen formátumú karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="b11eb-148">The Service Bus Rule ID is a string with this format:</span></span>

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- <span data-ttu-id="b11eb-149">A Naplóelemzési munkaterület elküldését a diagnosztikai naplók engedélyezéséhez az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="b11eb-149">To enable sending of Diagnostic Logs to a Log Analytics workspace, use this command:</span></span>

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

<span data-ttu-id="b11eb-150">Ezek a paraméterek ahhoz, hogy több kimenet beállításai kombinálhatja.</span><span class="sxs-lookup"><span data-stu-id="b11eb-150">You can combine these parameters to enable multiple output options.</span></span>

### <a name="rest-api"></a><span data-ttu-id="b11eb-151">REST API</span><span class="sxs-lookup"><span data-stu-id="b11eb-151">REST API</span></span>

<span data-ttu-id="b11eb-152">Olvassa el, hogyan [a Azure REST API használatával diagnosztikai beállítások módosításához](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="b11eb-152">Read about how to [change Diagnostic settings using the Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span> 

### <a name="resource-manager-template"></a><span data-ttu-id="b11eb-153">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="b11eb-153">Resource Manager template</span></span>

<span data-ttu-id="b11eb-154">Olvassa el, hogyan [engedélyezze a diagnosztikát a Resource Manager sablonnal erőforrás létrehozásakor](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md).</span><span class="sxs-lookup"><span data-stu-id="b11eb-154">Read about how to [enable Diagnostic settings at resource creation using Resource Manager template](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md).</span></span> 

## <a name="stream-into-log-analytics"></a><span data-ttu-id="b11eb-155">A Naplóelemzési adatfolyam</span><span class="sxs-lookup"><span data-stu-id="b11eb-155">Stream into Log Analytics</span></span> 
<span data-ttu-id="b11eb-156">Az Azure SQL Database metrikák és diagnosztikai naplók továbbítható a beépített "Küldése a Log Analyticshez" beállítás használatával, a portálon, vagy az Azure PowerShell parancsmagok, az Azure parancssori felület vagy Azure figyelő REST API-n keresztül diagnosztikai beállítás Naplóelemzési engedélyezésével Naplóelemzési be.</span><span class="sxs-lookup"><span data-stu-id="b11eb-156">Azure SQL Database metrics and diagnostic logs can be streamed into Log Analytics using the built-in “Send to Log Analytics” option in the portal, or by enabling Log Analytics in a diagnostic setting via Azure PowerShell cmdlets, Azure CLI, or Azure Monitor REST API.</span></span>

### <a name="installation-overview"></a><span data-ttu-id="b11eb-157">Telepítés – áttekintés</span><span class="sxs-lookup"><span data-stu-id="b11eb-157">Installation overview</span></span>

<span data-ttu-id="b11eb-158">Figyelése Azure SQL Database járműflotta a Naplóelemzési egyszerű.</span><span class="sxs-lookup"><span data-stu-id="b11eb-158">Monitoring Azure SQL Database fleet is simple with Log Analytics.</span></span> <span data-ttu-id="b11eb-159">Három lépésre szükség:</span><span class="sxs-lookup"><span data-stu-id="b11eb-159">Three steps are required:</span></span>

1.  <span data-ttu-id="b11eb-160">Log Analytics-erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b11eb-160">Create Log Analytics resource</span></span>
2.  <span data-ttu-id="b11eb-161">A metrikák és diagnosztikai naplók rekordot a létrehozott Log Analytics-adatbázisok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b11eb-161">Configure databases to record metrics and diagnostic logs into the created Log Analytics</span></span>
3.  <span data-ttu-id="b11eb-162">Telepítés **Azure SQL elemzés** Log Analytics-galériából megoldás</span><span class="sxs-lookup"><span data-stu-id="b11eb-162">Install **Azure SQL Analytics** solution from gallery in Log Analytics</span></span>

### <a name="create-log-analytics-resource"></a><span data-ttu-id="b11eb-163">Log Analytics-erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b11eb-163">Create Log Analytics resource</span></span>

1. <span data-ttu-id="b11eb-164">Kattintson a **új** a bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="b11eb-164">Click **New** in the left-hand menu.</span></span>
2. <span data-ttu-id="b11eb-165">Kattintson a **figyelési + kezelése**</span><span class="sxs-lookup"><span data-stu-id="b11eb-165">Click **Monitoring + Management**</span></span>
3. <span data-ttu-id="b11eb-166">Kattintson a **Analytics naplózása**</span><span class="sxs-lookup"><span data-stu-id="b11eb-166">Click **Log Analytics**</span></span>
4. <span data-ttu-id="b11eb-167">Töltse ki a Naplóelemzési űrlapot szükséges további információ: munkaterület nevét, előfizetés, erőforráscsoportot, helyét és IP-címek.</span><span class="sxs-lookup"><span data-stu-id="b11eb-167">Fill in the Log Analytics form with the additional information required: workspace name, subscription, resource group, location, and pricing tier.</span></span>

   ![a naplóelemzési](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-to-record-metrics-and-diagnostic-logs"></a><span data-ttu-id="b11eb-169">Rekord metrikák és diagnosztikai naplók-adatbázisok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b11eb-169">Configure databases to record metrics and diagnostic logs</span></span>

<span data-ttu-id="b11eb-170">A legegyszerűbben úgy konfigurálja, ahol adatbázisok jegyezze fel a metrikák van az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="b11eb-170">The easiest way to configure where databases record their metrics is through the Azure portal.</span></span> <span data-ttu-id="b11eb-171">Az Azure-portálon az Azure SQL Database erőforrás keresse meg és kattintson a **diagnosztikai beállítások**.</span><span class="sxs-lookup"><span data-stu-id="b11eb-171">In the Azure portal, navigate to your Azure SQL Database resource and click **Diagnostics settings**.</span></span> 

### <a name="install-the-azure-sql-analytics-solution-from-gallery"></a><span data-ttu-id="b11eb-172">Telepítse az Azure SQL elemzési megoldások gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b11eb-172">Install the Azure SQL Analytics solution from gallery</span></span>  

1. <span data-ttu-id="b11eb-173">Miután a Naplóelemzési erőforrás jön létre, és az adatok áramlik bele, telepítse az Azure SQL elemzési megoldások.</span><span class="sxs-lookup"><span data-stu-id="b11eb-173">Once the Log Analytics resource is created and your data is flowing into it, install Azure SQL Analytics solution.</span></span> <span data-ttu-id="b11eb-174">Ez végezhető el a **megoldások gyűjtemény** , amely a OMS kezdőlapon és az ügyféloldali menüjében található.</span><span class="sxs-lookup"><span data-stu-id="b11eb-174">This can be done through the **Solutions Gallery** that you can find on the OMS homepage and in the side menu.</span></span> <span data-ttu-id="b11eb-175">Az oldalon található, és kattintson a **Azure SQL elemzés** megoldás, és kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b11eb-175">In the gallery, find and click **Azure SQL Analytics** solution and click **Add**.</span></span>

   ![felügyeleti megoldás](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. <span data-ttu-id="b11eb-177">A kezdőlapon OMS, egy új csempe nevű **Azure SQL elemzés** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b11eb-177">On your OMS homepage, a new tile called **Azure SQL Analytics** appears.</span></span> <span data-ttu-id="b11eb-178">Ez a csempe kiválasztásával megnyílik az Azure SQL-elemzések irányítópultján.</span><span class="sxs-lookup"><span data-stu-id="b11eb-178">Selecting this tile opens the Azure SQL Analytics dashboard.</span></span>

### <a name="using-azure-sql-analytics-solution"></a><span data-ttu-id="b11eb-179">Az Azure SQL elemzés megoldással</span><span class="sxs-lookup"><span data-stu-id="b11eb-179">Using Azure SQL Analytics Solution</span></span>

<span data-ttu-id="b11eb-180">Az Azure SQL elemzés, amely lehetővé teszi a hierarchiában, az Azure SQL Database erőforrások közötti navigáláshoz hierarchikus irányítópult.</span><span class="sxs-lookup"><span data-stu-id="b11eb-180">Azure SQL Analytics is a hierarchical dashboard that allows you to navigate through the hierarchy of Azure SQL Database resources.</span></span> <span data-ttu-id="b11eb-181">Ez a funkció lehetővé teszi magas szintű figyelését, de emellett lehetővé teszi a hatókör csak a megfelelő készletéhez erőforrások a figyelést.</span><span class="sxs-lookup"><span data-stu-id="b11eb-181">This capability enables you to do high-level monitoring but it also enables you to scope your monitoring to just the right set of resources.</span></span>
<span data-ttu-id="b11eb-182">Az irányítópulton a kiválasztott erőforrás a különböző erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="b11eb-182">Dashboard contains the lists of different resources under the selected resource.</span></span> <span data-ttu-id="b11eb-183">Például a kiválasztott előfizetéshez megtekintheti az összes kiszolgálók, rugalmas készletek és adatbázisok, amelyek a kijelölt előfizetéshez tartozik.</span><span class="sxs-lookup"><span data-stu-id="b11eb-183">For example, for a selected subscription you can see the all servers, elastic pools and databases that belong to the selected subscription.</span></span> <span data-ttu-id="b11eb-184">Emellett a rugalmas készletek és adatbázisokat, megtekintheti az erőforrás-használati metrikák erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b11eb-184">Additionally, for Elastic Pools and databases, you can see the resource usage metrics of that resource.</span></span> <span data-ttu-id="b11eb-185">Ez magában foglalja diagramok DTU, Processzor, IO, napló, munkamenetek, dolgozók, kapcsolatok, valamint tárolási GB-ban.</span><span class="sxs-lookup"><span data-stu-id="b11eb-185">This includes charts for DTU, CPU, IO, LOG, sessions, workers, connections, and storage in GB.</span></span>

## <a name="stream-into-azure-event-hub"></a><span data-ttu-id="b11eb-186">Az Azure Event Hubs adatfolyam</span><span class="sxs-lookup"><span data-stu-id="b11eb-186">Stream into Azure Event Hub</span></span>

<span data-ttu-id="b11eb-187">Az Azure SQL Database metrikák és diagnosztikai naplók továbbítható a beépített "Stream az eseményközpontba" beállítás használatával, a portálon, vagy a Service Bus szabály azonosítója az Azure PowerShell-parancsmagok, az Azure parancssori felület vagy a Azure figyelő REST API-t egy diagnosztikai beállítás engedélyezésével az Event Hubs be.</span><span class="sxs-lookup"><span data-stu-id="b11eb-187">Azure SQL Database metrics and diagnostic logs can be streamed into Event Hub using the built-in “Stream to an event hub” option in the portal, or by enabling Service Bus Rule Id in a diagnostic setting via Azure PowerShell Cmdlets, Azure CLI, or Azure Monitor REST API.</span></span> 

### <a name="what-to-do-with-metrics-and-diagnostic-logs-in-event-hub"></a><span data-ttu-id="b11eb-188">Mit kell tenni a metrikák és diagnosztikai naplófájlok az Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="b11eb-188">What to do with metrics and diagnostic logs in Event Hub?</span></span>
<span data-ttu-id="b11eb-189">Ha a kijelölt adatokat az Event Hubs továbbítja adatfolyamként, áll egy lépéssel közelebb kerülnek engedélyezése speciális figyelési forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="b11eb-189">Once the selected data is streamed into Event Hub, you are one step closer to enabling advanced monitoring scenarios.</span></span> <span data-ttu-id="b11eb-190">Az Event Hubs a „bejárati ajtó” funkcióját látja el egy eseményfolyamat számára, az adatoknak egy eseményközpontba való összegyűjtését követően az adatok bármilyen valós idejű elemzési szolgáltató vagy kötegelési/tárolóadapter segítségével átalakíthatók és tárolhatók.</span><span class="sxs-lookup"><span data-stu-id="b11eb-190">Event Hubs acts as the "front door" for an event pipeline, and once data is collected into an Event Hub, it can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="b11eb-191">Az Event Hubs elválasztja az eseménystreamek létrehozását azok felhasználásától, így az események felhasználói a saját ütemezésüknek megfelelően férhetnek hozzá az eseményekhez.</span><span class="sxs-lookup"><span data-stu-id="b11eb-191">Event Hubs decouples the production of a stream of events from the consumption of those events, so that event consumers can access the events on their own schedule.</span></span> <span data-ttu-id="b11eb-192">Az Event Hubs további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="b11eb-192">For more information on Event Hub, see:</span></span>

- <span data-ttu-id="b11eb-193">[Mik az Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?</span><span class="sxs-lookup"><span data-stu-id="b11eb-193">[What are Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?</span></span>
- [<span data-ttu-id="b11eb-194">Bevezetés az Event Hubs használatába</span><span class="sxs-lookup"><span data-stu-id="b11eb-194">Get started with Event Hubs</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


<span data-ttu-id="b11eb-195">Az alábbiakban néhány módokon használhatja a adatfolyam-továbbítási funkció:</span><span class="sxs-lookup"><span data-stu-id="b11eb-195">Here are just a few ways you might use the streaming capability:</span></span>

-   <span data-ttu-id="b11eb-196">Szolgáltatás állapotának megtekintéséhez folyamatos "forró path" adatokat a PowerBI - Event Hubs használatával, a Stream Analytics és a Power BI, könnyen alakíthatja a metrikák és diagnosztikai adatokat közel valós idejű elemzése az Azure-szolgáltatásoknak.</span><span class="sxs-lookup"><span data-stu-id="b11eb-196">View service health by streaming “hot path” data to PowerBI - Using Event Hubs, Stream Analytics, and PowerBI, you can easily transform your metrics and diagnostics data into near real-time insights on your Azure services.</span></span> <span data-ttu-id="b11eb-197">Megtudhatja, hogyan állíthat be az Event Hubs adatfeldolgozásra Streamelemzéssel és kimenetként Power bi használatát [Stream Analytics és a Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="b11eb-197">For an overview of how to set up an Event Hubs, process data with Stream Analytics, and use PowerBI as an output, see [Stream Analytics and Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span></span>
-   <span data-ttu-id="b11eb-198">Stream naplók a külső naplózása és telemetriai adatfolyamok – használja az Event Hubs streaming meg kérheti le a metrikákat, és diagnosztikai jelentkezik be a különböző külső figyelése és a naplófájlok elemzési megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="b11eb-198">Stream logs to third-party logging and telemetry streams – Using Event Hubs streaming you can get your metrics and diagnostic logs in to different third-party monitoring and log analytics solutions.</span></span> 
-   <span data-ttu-id="b11eb-199">Egy egyéni telemetriai adatok és a naplózás platform összeállítása – Ha már rendelkezik egy egyedi telemetriai platform vagy vannak csak gondolat egy kiválóan méretezhető épület jellegétől függően az Event Hubs közzétételi-feliratkozási lehetővé teszi rugalmasan betöltési a diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="b11eb-199">Build a custom telemetry and logging platform – If you already have a custom-built telemetry platform or are just thinking about building one, the highly scalable publish-subscribe nature of Event Hubs allows you to flexibly ingest diagnostic logs.</span></span> <span data-ttu-id="b11eb-200">Lásd: [Dan Rosanova útmutató az Event Hubs egy globális méretű telemetriai platform használatával](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span><span class="sxs-lookup"><span data-stu-id="b11eb-200">See [Dan Rosanova’s guide to using Event Hubs in a global scale telemetry platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span></span>

## <a name="stream-into-azure-storage"></a><span data-ttu-id="b11eb-201">Az Azure Storage adatfolyam</span><span class="sxs-lookup"><span data-stu-id="b11eb-201">Stream into Azure Storage</span></span>

<span data-ttu-id="b11eb-202">Az Azure SQL Database metrikák és diagnosztikai naplók az Azure Storage a beépített "Archív tárfiókba" beállítás használatával, az Azure portálon, vagy Azure Storage Azure PowerShell-parancsmagok, az Azure parancssori felület vagy a Azure figyelő REST API-t egy diagnosztikai beállítás engedélyezésével is tárolhatók.</span><span class="sxs-lookup"><span data-stu-id="b11eb-202">Azure SQL Database metrics and diagnostic logs can be stored into Azure Storage using the built-in "Archive to a storage account” option in the Azure portal, or by enabling Azure Storage in a diagnostic setting via Azure PowerShell Cmdlets, Azure CLI, or Azure Monitor REST API.</span></span>

### <a name="schema-of-metrics-and-diagnostic-logs-in-the-storage-account"></a><span data-ttu-id="b11eb-203">A metrikák és diagnosztikai naplókat a tárfiókban lévő séma</span><span class="sxs-lookup"><span data-stu-id="b11eb-203">Schema of metrics and diagnostic logs in the storage account</span></span>

<span data-ttu-id="b11eb-204">Ha metrikák és diagnosztikai naplók gyűjtemény áll, tárolót választotta, amikor az első adatsor sem érhető el a storage-fiók jön létre.</span><span class="sxs-lookup"><span data-stu-id="b11eb-204">Once you have set up metrics and diagnostic logs collection, a storage container is created in the storage account you selected when the first rows of data are available.</span></span> <span data-ttu-id="b11eb-205">A blobok szerkezete van:</span><span class="sxs-lookup"><span data-stu-id="b11eb-205">The structure of these blobs is:</span></span>

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
<span data-ttu-id="b11eb-206">Vagy egyszerűen több:</span><span class="sxs-lookup"><span data-stu-id="b11eb-206">Or, more simply:</span></span>

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

<span data-ttu-id="b11eb-207">A blob nevének 1 perces metrikáihoz lehet, például:</span><span class="sxs-lookup"><span data-stu-id="b11eb-207">For example, a blob name for 1-minute metrics might be:</span></span>

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

<span data-ttu-id="b11eb-208">Abban az esetben, ha szeretné rögzíteni az adatokat a rugalmas készletből, blob neve a következő kissé eltérő:</span><span class="sxs-lookup"><span data-stu-id="b11eb-208">In case you want to record the data from the Elastic Pool, blob name is a bit different:</span></span>

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-azure-storage"></a><span data-ttu-id="b11eb-209">Az Azure storage metrikák és a naplók letöltése</span><span class="sxs-lookup"><span data-stu-id="b11eb-209">Download metrics and logs from Azure storage</span></span>

<span data-ttu-id="b11eb-210">Lásd: [metrikák és diagnosztikai naplók letöltése Azure Storage-ból](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)</span><span class="sxs-lookup"><span data-stu-id="b11eb-210">See [Download metrics and diagnostic logs from Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)</span></span>

## <a name="1-minute-metrics"></a><span data-ttu-id="b11eb-211">1 perces metrikák</span><span class="sxs-lookup"><span data-stu-id="b11eb-211">1-minute metrics</span></span>

| |  |
|---|---|
|<span data-ttu-id="b11eb-212">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="b11eb-212">**Resource**</span></span>|<span data-ttu-id="b11eb-213">**Metrikák**</span><span class="sxs-lookup"><span data-stu-id="b11eb-213">**Metrics**</span></span>|
|<span data-ttu-id="b11eb-214">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="b11eb-214">Database</span></span>|<span data-ttu-id="b11eb-215">DTU százalékos DTU használt, DTU határt, a processzor, a fizikai adatáttelepítések olvasási százalékos, napló írása százalékos, sikeres vagy sikertelen/letiltott tűzfalkapcsolatok, munkamenetek százalékos, munkavállalók százalékos, tárolási, tárolási százalékos, XTP tárolási százalékos, holtpont</span><span class="sxs-lookup"><span data-stu-id="b11eb-215">DTU percentage, DTU used, DTU limit, CPU percentage, Physical data read percentage, Log write percentage, Successful/Failed/Blocked by firewall connections, sessions percentage, workers percentage, storage, storage percentage, XTP storage percentage, deadlocks</span></span> |
|<span data-ttu-id="b11eb-216">A rugalmas készlet</span><span class="sxs-lookup"><span data-stu-id="b11eb-216">Elastic pool</span></span>|<span data-ttu-id="b11eb-217">eDTU százalékos eDTU használt, eDTU határt, a processzor, a fizikai adatáttelepítések olvasási százalékos, napló írása százalékos, munkamenetek százalékos, munkavállalók százalékos, tárolási, tárolási százalékos, tárolási kapacitás, XTP tárolási százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="b11eb-217">eDTU percentage, eDTU used, eDTU limit, CPU percentage, Physical data read percentage, Log write percentage, sessions percentage, workers percentage, storage, storage percentage, storage limit, XTP storage percentage</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="b11eb-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b11eb-218">Next steps</span></span>

- <span data-ttu-id="b11eb-219">Mindkét olvassa el a [áttekintése a Microsoft Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md) és [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) cikkek felmérheti, nem csak hogyan engedélyezze a naplózást, de a metrikák és a napló kategóriák különböző Azure-szolgáltatás által támogatott.</span><span class="sxs-lookup"><span data-stu-id="b11eb-219">Read both the [Overview of metrics in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) and [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) articles to gain an understanding of not only how to enable logging, but the metrics and log categories supported by the various Azure services.</span></span>
- <span data-ttu-id="b11eb-220">Ezek a cikkek az event hubs megismeréséhez olvassa el:</span><span class="sxs-lookup"><span data-stu-id="b11eb-220">Read these articles to learn about event hubs:</span></span>
   - <span data-ttu-id="b11eb-221">[Mik az Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?</span><span class="sxs-lookup"><span data-stu-id="b11eb-221">[What are Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?</span></span>
   - [<span data-ttu-id="b11eb-222">Bevezetés az Event Hubs használatába</span><span class="sxs-lookup"><span data-stu-id="b11eb-222">Get started with Event Hubs</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- <span data-ttu-id="b11eb-223">Lásd: [metrikák és diagnosztikai naplók letöltése Azure Storage-ból](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)</span><span class="sxs-lookup"><span data-stu-id="b11eb-223">See [Download metrics and diagnostic logs from Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)</span></span>
