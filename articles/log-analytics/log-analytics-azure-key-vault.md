---
title: "Key Vault megoldás Naplóelemzési aaaAzure |} Microsoft Docs"
description: "Az Azure Key Vault-naplók Naplóelemzési tooreview hello Azure Key Vault megoldás is használhatja."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 1c6eae26ded7ad55b0159a3be09cdc9901596298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-analytics-solution-in-log-analytics"></a><span data-ttu-id="f7932-103">Log Analytics az Azure Key Vault Analytics megoldás</span><span class="sxs-lookup"><span data-stu-id="f7932-103">Azure Key Vault Analytics solution in Log Analytics</span></span>

![Key Vault szimbólum](./media/log-analytics-azure-keyvault/key-vault-analytics-symbol.png)

<span data-ttu-id="f7932-105">A Naplóelemzési tooreview naplózza az Azure Key Vault AuditEvent hello Azure Key Vault megoldás is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f7932-105">You can use hello Azure Key Vault solution in Log Analytics tooreview Azure Key Vault AuditEvent logs.</span></span>

<span data-ttu-id="f7932-106">toouse hello megoldás, az Azure Key Vault diagnosztika és a közvetlen hello diagnosztika tooa Naplóelemzési munkaterület tooenable naplózása szüksége.</span><span class="sxs-lookup"><span data-stu-id="f7932-106">toouse hello solution, you need tooenable logging of Azure Key Vault diagnostics and direct hello diagnostics tooa Log Analytics workspace.</span></span> <span data-ttu-id="f7932-107">Már nem szükséges toowrite hello naplók tooAzure Blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="f7932-107">It is not necessary toowrite hello logs tooAzure Blob storage.</span></span>

> [!NOTE]
> <span data-ttu-id="f7932-108">2017. január hello támogatott módszer a naplók küldésével Key Vault tooLog Analytics megváltozott.</span><span class="sxs-lookup"><span data-stu-id="f7932-108">In January 2017, hello supported way of sending logs from Key Vault tooLog Analytics changed.</span></span> <span data-ttu-id="f7932-109">Ha hello Key Vault megoldás használatával jeleníti meg a *(elavult)* hello cím alatt túl[hello régi Key Vault megoldás áttelepítés](#migrating-from-the-old-key-vault-solution) lépéseket toofollow kell.</span><span class="sxs-lookup"><span data-stu-id="f7932-109">If hello Key Vault solution you are using shows *(deprecated)* in hello title, refer too[migrating from hello old Key Vault solution](#migrating-from-the-old-key-vault-solution) for steps you need toofollow.</span></span>
>
>

## <a name="install-and-configure-hello-solution"></a><span data-ttu-id="f7932-110">Telepítse és konfigurálja a hello megoldás</span><span class="sxs-lookup"><span data-stu-id="f7932-110">Install and configure hello solution</span></span>
<span data-ttu-id="f7932-111">A következő utasításokat tooinstall hello használja, és hello Azure Key Vault megoldás konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="f7932-111">Use hello following instructions tooinstall and configure hello Azure Key Vault solution:</span></span>

1. <span data-ttu-id="f7932-112">Hello Azure Key Vault megoldást engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="f7932-112">Enable hello Azure Key Vault solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="f7932-113">Diagnosztikai naplózás a Key Vault-erőforrások toomonitor hello, vagy hello segítségével engedélyezése [portal](#enable-key-vault-diagnostics-in-the-portal) vagy [PowerShell](#enable-key-vault-diagnostics-using-powershell)</span><span class="sxs-lookup"><span data-stu-id="f7932-113">Enable diagnostics logging for hello Key Vault resources toomonitor, using either hello [portal](#enable-key-vault-diagnostics-in-the-portal) or [PowerShell](#enable-key-vault-diagnostics-using-powershell)</span></span>

### <a name="enable-key-vault-diagnostics-in-hello-portal"></a><span data-ttu-id="f7932-114">Key Vault diagnosztika hello portálon engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f7932-114">Enable Key Vault diagnostics in hello portal</span></span>

1. <span data-ttu-id="f7932-115">Hello Azure-portálon lépjen a toohello Key Vault erőforrás toomonitor</span><span class="sxs-lookup"><span data-stu-id="f7932-115">In hello Azure portal, navigate toohello Key Vault resource toomonitor</span></span>
2. <span data-ttu-id="f7932-116">Válassza ki *diagnosztikai naplók* tooopen hello a következő lap</span><span class="sxs-lookup"><span data-stu-id="f7932-116">Select *Diagnostics logs* tooopen hello following page</span></span>

   ![az Azure Key Vault csempe képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics01.png)
3. <span data-ttu-id="f7932-118">Kattintson a *a diagnosztika bekapcsolásához* tooopen hello a következő lap</span><span class="sxs-lookup"><span data-stu-id="f7932-118">Click *Turn on diagnostics* tooopen hello following page</span></span>

   ![az Azure Key Vault csempe képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics02.png)
4. <span data-ttu-id="f7932-120">a diagnosztikát, tooturn kattintson *a* alatt *állapota*</span><span class="sxs-lookup"><span data-stu-id="f7932-120">tooturn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="f7932-121">Kattintson a hello jelölőnégyzetét *tooLog Analytics küldése*</span><span class="sxs-lookup"><span data-stu-id="f7932-121">Click hello checkbox for *Send tooLog Analytics*</span></span>
6. <span data-ttu-id="f7932-122">Válasszon ki egy meglévő Naplóelemzési munkaterület, vagy egy munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="f7932-122">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="f7932-123">tooenable *AuditEvent* naplókat, kattintson a hello jelölőnégyzetet a napló</span><span class="sxs-lookup"><span data-stu-id="f7932-123">tooenable *AuditEvent* logs, click hello checkbox under Log</span></span>
8. <span data-ttu-id="f7932-124">Kattintson a *mentése* diagnosztika tooLog Analytics tooenable hello naplózása</span><span class="sxs-lookup"><span data-stu-id="f7932-124">Click *Save* tooenable hello logging of diagnostics tooLog Analytics</span></span>

### <a name="enable-key-vault-diagnostics-using-powershell"></a><span data-ttu-id="f7932-125">Engedélyezze a Key Vault diagnosztika PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f7932-125">Enable Key Vault diagnostics using PowerShell</span></span>
<span data-ttu-id="f7932-126">a következő PowerShell-parancsfájl hello azt szemlélteti, hogyan toouse `Set-AzureRmDiagnosticSetting` tooenable diagnosztikai Key Vault naplózása:</span><span class="sxs-lookup"><span data-stu-id="f7932-126">hello following PowerShell script provides an example of how toouse `Set-AzureRmDiagnosticSetting` tooenable diagnostic logging for Key Vault:</span></span>
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a><span data-ttu-id="f7932-127">Tekintse át az Azure Key Vault az gyűjtemény adatait</span><span class="sxs-lookup"><span data-stu-id="f7932-127">Review Azure Key Vault data collection details</span></span>
<span data-ttu-id="f7932-128">Az Azure Key Vault megoldás diagnosztikai naplók közvetlenül a Key Vault hello gyűjti.</span><span class="sxs-lookup"><span data-stu-id="f7932-128">Azure Key Vault solution collects diagnostics logs directly from hello Key Vault.</span></span>
<span data-ttu-id="f7932-129">Már nem szükséges toowrite hello naplók tooAzure Blob-tároló, és nincs ügynök szükséges adatok gyűjtését.</span><span class="sxs-lookup"><span data-stu-id="f7932-129">It is not necessary toowrite hello logs tooAzure Blob storage and no agent is required for data collection.</span></span>

<span data-ttu-id="f7932-130">hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan Azure Key Vault részleteit.</span><span class="sxs-lookup"><span data-stu-id="f7932-130">hello following table shows data collection methods and other details about how data is collected for Azure Key Vault.</span></span>

| <span data-ttu-id="f7932-131">Platform</span><span class="sxs-lookup"><span data-stu-id="f7932-131">Platform</span></span> | <span data-ttu-id="f7932-132">Közvetlen ügynök</span><span class="sxs-lookup"><span data-stu-id="f7932-132">Direct agent</span></span> | <span data-ttu-id="f7932-133">Systems Center Operations Manager-ügynök</span><span class="sxs-lookup"><span data-stu-id="f7932-133">Systems Center Operations Manager agent</span></span> | <span data-ttu-id="f7932-134">Azure</span><span class="sxs-lookup"><span data-stu-id="f7932-134">Azure</span></span> | <span data-ttu-id="f7932-135">Az Operations Manager szükséges?</span><span class="sxs-lookup"><span data-stu-id="f7932-135">Operations Manager required?</span></span> | <span data-ttu-id="f7932-136">Az Operations Manager ügynök adatait a felügyeleti csoport keresztül küldött</span><span class="sxs-lookup"><span data-stu-id="f7932-136">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="f7932-137">A gyűjtés gyakorisága</span><span class="sxs-lookup"><span data-stu-id="f7932-137">Collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="f7932-138">Azure</span><span class="sxs-lookup"><span data-stu-id="f7932-138">Azure</span></span> |  |  |<span data-ttu-id="f7932-139">&#8226;</span><span class="sxs-lookup"><span data-stu-id="f7932-139">&#8226;</span></span> |  |  | <span data-ttu-id="f7932-140">érkezésükkor</span><span class="sxs-lookup"><span data-stu-id="f7932-140">on arrival</span></span> |

## <a name="use-azure-key-vault"></a><span data-ttu-id="f7932-141">Az Azure Key Vault használata</span><span class="sxs-lookup"><span data-stu-id="f7932-141">Use Azure Key Vault</span></span>
<span data-ttu-id="f7932-142">Miután [hello megoldás telepítése](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), hello kulcstároló adatainak megtekintéséhez kattintson a hello **Azure Key Vault** hello a csempe **áttekintése** Naplóelemzési oldalán.</span><span class="sxs-lookup"><span data-stu-id="f7932-142">After you [install hello solution](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), view hello Key Vault data by clicking hello **Azure Key Vault** tile from hello **Overview** page of Log Analytics.</span></span>

![az Azure Key Vault csempe képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

<span data-ttu-id="f7932-144">Miután rákattintott hello **áttekintése** csempe, megtekintheti a naplókat, valamint majd részletezési összegzéseinek toodetails a következő kategóriák hello:</span><span class="sxs-lookup"><span data-stu-id="f7932-144">After you click hello **Overview** tile, you can view summaries of your logs and then drill in toodetails for hello following categories:</span></span>

* <span data-ttu-id="f7932-145">Az összes kulcstároló művelet időbeli kötet</span><span class="sxs-lookup"><span data-stu-id="f7932-145">Volume of all key vault operations over time</span></span>
* <span data-ttu-id="f7932-146">Nem sikerült a művelet kötetek adott idő alatt</span><span class="sxs-lookup"><span data-stu-id="f7932-146">Failed operation volumes over time</span></span>
* <span data-ttu-id="f7932-147">Átlagos műveleti várakozási művelet</span><span class="sxs-lookup"><span data-stu-id="f7932-147">Average operational latency by operation</span></span>
* <span data-ttu-id="f7932-148">Szolgáltatásminőség műveletekhez hello száma, amelyek több mint 1000 ms műveletek és a műveleteket, amelyek több mint 1000 ms listája</span><span class="sxs-lookup"><span data-stu-id="f7932-148">Quality of service for operations with hello number of operations that take more than 1000 ms and a list of operations that take more than 1000 ms</span></span>

![az Azure Key Vault irányítópult képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![az Azure Key Vault irányítópult képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="tooview-details-for-any-operation"></a><span data-ttu-id="f7932-151">bármely művelet tooview részletei</span><span class="sxs-lookup"><span data-stu-id="f7932-151">tooview details for any operation</span></span>
1. <span data-ttu-id="f7932-152">A hello **áttekintése** hello kattintson **Azure Key Vault** csempére.</span><span class="sxs-lookup"><span data-stu-id="f7932-152">On hello **Overview** page, click hello **Azure Key Vault** tile.</span></span>
2. <span data-ttu-id="f7932-153">A hello **Azure Key Vault** az irányítópult hello összefoglaló információkat valamelyik hello paneleken ellenőrizni, és kattintson egy tooview részletes információkat, akkor a hello napló keresése oldal.</span><span class="sxs-lookup"><span data-stu-id="f7932-153">On hello **Azure Key Vault** dashboard, review hello summary information in one of hello blades, and then click one tooview detailed information about it in hello log search page.</span></span>

    <span data-ttu-id="f7932-154">Bármely hello napló keresése lapok eredmények megtekintése idő, illetve részletes leírást és a keresési korábbi naplók.</span><span class="sxs-lookup"><span data-stu-id="f7932-154">On any of hello log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="f7932-155">Értékkorlátozás toonarrow hello eredmények is végezhet.</span><span class="sxs-lookup"><span data-stu-id="f7932-155">You can also filter by facets toonarrow hello results.</span></span>

## <a name="log-analytics-records"></a><span data-ttu-id="f7932-156">Log Analytics-rekordok</span><span class="sxs-lookup"><span data-stu-id="f7932-156">Log Analytics records</span></span>
<span data-ttu-id="f7932-157">az Azure Key Vault megoldás hello elemzi azt jelzi, hogy rendelkezik olyan típusú **KeyVaults** , hogy a rendszer begyűjti az [AuditEvent naplók](../key-vault/key-vault-logging.md) az Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="f7932-157">hello Azure Key Vault solution analyzes records that have a type of **KeyVaults** that are collected from [AuditEvent logs](../key-vault/key-vault-logging.md) in Azure Diagnostics.</span></span>  <span data-ttu-id="f7932-158">Ezeket a rekordokat tulajdonságainak vannak a következő táblázat hello:</span><span class="sxs-lookup"><span data-stu-id="f7932-158">Properties for these records are in hello following table:</span></span>  

| <span data-ttu-id="f7932-159">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f7932-159">Property</span></span> | <span data-ttu-id="f7932-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="f7932-160">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="f7932-161">Típus</span><span class="sxs-lookup"><span data-stu-id="f7932-161">Type</span></span> |<span data-ttu-id="f7932-162">*AzureDiagnostics*</span><span class="sxs-lookup"><span data-stu-id="f7932-162">*AzureDiagnostics*</span></span> |
| <span data-ttu-id="f7932-163">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="f7932-163">SourceSystem</span></span> |<span data-ttu-id="f7932-164">*Azure*</span><span class="sxs-lookup"><span data-stu-id="f7932-164">*Azure*</span></span> |
| <span data-ttu-id="f7932-165">CallerIpAddress</span><span class="sxs-lookup"><span data-stu-id="f7932-165">CallerIpAddress</span></span> |<span data-ttu-id="f7932-166">Hello kérelmet leadó hello ügyfél IP-címe</span><span class="sxs-lookup"><span data-stu-id="f7932-166">IP address of hello client who made hello request</span></span> |
| <span data-ttu-id="f7932-167">Kategória</span><span class="sxs-lookup"><span data-stu-id="f7932-167">Category</span></span> | <span data-ttu-id="f7932-168">*AuditEvent*</span><span class="sxs-lookup"><span data-stu-id="f7932-168">*AuditEvent*</span></span> |
| <span data-ttu-id="f7932-169">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="f7932-169">CorrelationId</span></span> |<span data-ttu-id="f7932-170">Egy nem kötelező GUID, amely az ügyfél hello teljen toocorrelate ügyféloldali naplók és a Szolgáltatásoldali (Key Vault) naplók.</span><span class="sxs-lookup"><span data-stu-id="f7932-170">An optional GUID that hello client can pass toocorrelate client-side logs with service-side (Key Vault) logs.</span></span> |
| <span data-ttu-id="f7932-171">DurationMs</span><span class="sxs-lookup"><span data-stu-id="f7932-171">DurationMs</span></span> |<span data-ttu-id="f7932-172">Idő, ezredmásodpercben megadva tooservice hello REST API-kérelem.</span><span class="sxs-lookup"><span data-stu-id="f7932-172">Time it took tooservice hello REST API request, in milliseconds.</span></span> <span data-ttu-id="f7932-173">Most nem tartalmazza a hálózati késés, így hello időt, amely mérni a hello ügyféloldalon nem egyeznek meg a megadott idő.</span><span class="sxs-lookup"><span data-stu-id="f7932-173">This time does not include network latency, so hello time that you measure on hello client side might not match this time.</span></span> |
| <span data-ttu-id="f7932-174">httpStatusCode_d</span><span class="sxs-lookup"><span data-stu-id="f7932-174">httpStatusCode_d</span></span> |<span data-ttu-id="f7932-175">Hello kérelem által visszaadott HTTP-állapotkódot (például *200*)</span><span class="sxs-lookup"><span data-stu-id="f7932-175">HTTP status code returned by hello request (for example, *200*)</span></span> |
| <span data-ttu-id="f7932-176">id_s</span><span class="sxs-lookup"><span data-stu-id="f7932-176">id_s</span></span> |<span data-ttu-id="f7932-177">Hello kérelem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="f7932-177">Unique ID of hello request</span></span> |
| <span data-ttu-id="f7932-178">identity_claim_appid_g</span><span class="sxs-lookup"><span data-stu-id="f7932-178">identity_claim_appid_g</span></span> | <span data-ttu-id="f7932-179">Hello alkalmazásazonosító GUID azonosítója</span><span class="sxs-lookup"><span data-stu-id="f7932-179">GUID for hello application id</span></span> |
| <span data-ttu-id="f7932-180">OperationName</span><span class="sxs-lookup"><span data-stu-id="f7932-180">OperationName</span></span> |<span data-ttu-id="f7932-181">Hello művelet, ahogy neve [Azure Key Vault naplózása](../key-vault/key-vault-logging.md)</span><span class="sxs-lookup"><span data-stu-id="f7932-181">Name of hello operation, as documented in [Azure Key Vault Logging](../key-vault/key-vault-logging.md)</span></span> |
| <span data-ttu-id="f7932-182">OperationVersion</span><span class="sxs-lookup"><span data-stu-id="f7932-182">OperationVersion</span></span> |<span data-ttu-id="f7932-183">Hello ügyfél által kért REST API-verzió (például *2015-06-01*)</span><span class="sxs-lookup"><span data-stu-id="f7932-183">REST API version requested by hello client (for example *2015-06-01*)</span></span> |
| <span data-ttu-id="f7932-184">requestUri_s</span><span class="sxs-lookup"><span data-stu-id="f7932-184">requestUri_s</span></span> |<span data-ttu-id="f7932-185">Hello kérelem URI-val</span><span class="sxs-lookup"><span data-stu-id="f7932-185">Uri of hello request</span></span> |
| <span data-ttu-id="f7932-186">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="f7932-186">Resource</span></span> |<span data-ttu-id="f7932-187">Hello kulcstároló neve</span><span class="sxs-lookup"><span data-stu-id="f7932-187">Name of hello key vault</span></span> |
| <span data-ttu-id="f7932-188">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f7932-188">ResourceGroup</span></span> |<span data-ttu-id="f7932-189">Hello kulcstároló erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="f7932-189">Resource group of hello key vault</span></span> |
| <span data-ttu-id="f7932-190">ResourceId</span><span class="sxs-lookup"><span data-stu-id="f7932-190">ResourceId</span></span> |<span data-ttu-id="f7932-191">Az Azure Resource Manager szerinti erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="f7932-191">Azure Resource Manager Resource ID.</span></span> <span data-ttu-id="f7932-192">Key Vault naplóinak ez pedig hello Key Vault erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="f7932-192">For Key Vault logs, this is hello Key Vault resource ID.</span></span> |
| <span data-ttu-id="f7932-193">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="f7932-193">ResourceProvider</span></span> |<span data-ttu-id="f7932-194">*MICROSOFT. KEYVAULT*</span><span class="sxs-lookup"><span data-stu-id="f7932-194">*MICROSOFT.KEYVAULT*</span></span> |
| <span data-ttu-id="f7932-195">ResourceType</span><span class="sxs-lookup"><span data-stu-id="f7932-195">ResourceType</span></span> | <span data-ttu-id="f7932-196">*TÁROLÓK*</span><span class="sxs-lookup"><span data-stu-id="f7932-196">*VAULTS*</span></span> |
| <span data-ttu-id="f7932-197">ResultSignature</span><span class="sxs-lookup"><span data-stu-id="f7932-197">ResultSignature</span></span> |<span data-ttu-id="f7932-198">HTTP-állapot (például *OK*)</span><span class="sxs-lookup"><span data-stu-id="f7932-198">HTTP status (for example, *OK*)</span></span> |
| <span data-ttu-id="f7932-199">ResultType</span><span class="sxs-lookup"><span data-stu-id="f7932-199">ResultType</span></span> |<span data-ttu-id="f7932-200">REST API-kérelem eredménye (például *sikeres*)</span><span class="sxs-lookup"><span data-stu-id="f7932-200">Result of REST API request (for example, *Success*)</span></span> |
| <span data-ttu-id="f7932-201">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="f7932-201">SubscriptionId</span></span> |<span data-ttu-id="f7932-202">Azure-előfizetése Azonosítóját tartalmazó hello Key Vault hello előfizetés</span><span class="sxs-lookup"><span data-stu-id="f7932-202">Azure subscription ID of hello subscription containing hello Key Vault</span></span> |

## <a name="migrating-from-hello-old-key-vault-solution"></a><span data-ttu-id="f7932-203">Hello régi Key Vault megoldás áttelepítése</span><span class="sxs-lookup"><span data-stu-id="f7932-203">Migrating from hello old Key Vault solution</span></span>
<span data-ttu-id="f7932-204">2017. január hello támogatott módszer a naplók küldésével Key Vault tooLog Analytics megváltozott.</span><span class="sxs-lookup"><span data-stu-id="f7932-204">In January 2017, hello supported way of sending logs from Key Vault tooLog Analytics changed.</span></span> <span data-ttu-id="f7932-205">Ezek a változások hello a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f7932-205">These changes provide hello following advantages:</span></span>
+ <span data-ttu-id="f7932-206">Közvetlen tooLog Analytics hello nélkül kell toouse tárfiók írja a naplókat</span><span class="sxs-lookup"><span data-stu-id="f7932-206">Logs are written directly tooLog Analytics without hello need toouse a storage account</span></span>
+ <span data-ttu-id="f7932-207">Kisebb késést hello időpontot, amikor naplók a generált legyenek elérhetők a Naplóelemzési toothem</span><span class="sxs-lookup"><span data-stu-id="f7932-207">Less latency from hello time when logs are generated toothem being available in Log Analytics</span></span>
+ <span data-ttu-id="f7932-208">Kevesebb konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="f7932-208">Fewer configuration steps</span></span>
+ <span data-ttu-id="f7932-209">Az összes Azure diagnostics közös formátum</span><span class="sxs-lookup"><span data-stu-id="f7932-209">A common format for all types of Azure diagnostics</span></span>

<span data-ttu-id="f7932-210">toouse hello megoldás frissítése:</span><span class="sxs-lookup"><span data-stu-id="f7932-210">toouse hello updated solution:</span></span>

1. [<span data-ttu-id="f7932-211">Diagnosztika toobe küldött Key Vault közvetlenül tooLog Analytics konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f7932-211">Configure diagnostics toobe sent directly tooLog Analytics from Key Vault</span></span>](#enable-key-vault-diagnostics-in-the-portal)  
2. <span data-ttu-id="f7932-212">Engedélyezze a hello Azure Key Vault megoldás hello ismertetett folyamatot [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjteménye](log-analytics-add-solutions.md)</span><span class="sxs-lookup"><span data-stu-id="f7932-212">Enable hello Azure Key Vault solution by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md)</span></span>
3. <span data-ttu-id="f7932-213">A lekérdezések, irányítópultok vagy riasztások toouse hello új adattípusra frissítése</span><span class="sxs-lookup"><span data-stu-id="f7932-213">Update any saved queries, dashboards, or alerts toouse hello new data type</span></span>
  + <span data-ttu-id="f7932-214">Típus: módosítást: KeyVaults tooAzureDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="f7932-214">Type is change from: KeyVaults tooAzureDiagnostics.</span></span> <span data-ttu-id="f7932-215">Hello ResourceType toofilter tooKey Vault naplóinak is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f7932-215">You can use hello ResourceType toofilter tooKey Vault Logs.</span></span>
  - <span data-ttu-id="f7932-216">Ahelyett, hogy: `Type=KeyVaults`, használata`Type=AzureDiagnostics ResourceType=VAULTS`</span><span class="sxs-lookup"><span data-stu-id="f7932-216">Instead of: `Type=KeyVaults`, use `Type=AzureDiagnostics ResourceType=VAULTS`</span></span>
  + <span data-ttu-id="f7932-217">Mezők: (mezőnevek számítanak a kis-és nagybetűket)</span><span class="sxs-lookup"><span data-stu-id="f7932-217">Fields: (Field names are case-sensitive)</span></span>
  - <span data-ttu-id="f7932-218">Bármely mezőhöz, amely rendelkezik egy utótagja \_s, \_d, vagy \_g a módosítás hello első karakter toolower esetben hello neve</span><span class="sxs-lookup"><span data-stu-id="f7932-218">For any field that has a suffix of \_s, \_d, or \_g in hello name, change hello first character toolower case</span></span>
  - <span data-ttu-id="f7932-219">Bármely mezőhöz, amely rendelkezik egy utótagja \_a név hello adatok o oszlik, egyedi mezők beágyazott hello mezőnevek alapján.</span><span class="sxs-lookup"><span data-stu-id="f7932-219">For any field that has a suffix of \_o in name, hello data is split into individual fields based on hello nested field names.</span></span> <span data-ttu-id="f7932-220">Például egy mező hello hello hívó UPN tárolja`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`</span><span class="sxs-lookup"><span data-stu-id="f7932-220">For example, hello UPN of hello caller is stored in a field `identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`</span></span>
   - <span data-ttu-id="f7932-221">Módosított mező CallerIpAddress tooCallerIPAddress</span><span class="sxs-lookup"><span data-stu-id="f7932-221">Field CallerIpAddress changed tooCallerIPAddress</span></span>
   - <span data-ttu-id="f7932-222">A mező RemoteIPCountry már nincs jelen</span><span class="sxs-lookup"><span data-stu-id="f7932-222">Field RemoteIPCountry is no longer present</span></span>
4. <span data-ttu-id="f7932-223">Távolítsa el a hello *Key Vault Analytics (elavult)* megoldás.</span><span class="sxs-lookup"><span data-stu-id="f7932-223">Remove hello *Key Vault Analytics (Deprecated)* solution.</span></span> <span data-ttu-id="f7932-224">Ha a PowerShell használata esetén`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`</span><span class="sxs-lookup"><span data-stu-id="f7932-224">If you are using PowerShell, use `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`</span></span>

<span data-ttu-id="f7932-225">Adatgyűjtés előtt hello módosítása nem látható a hello új megoldás.</span><span class="sxs-lookup"><span data-stu-id="f7932-225">Data collected before hello change is not visible in hello new solution.</span></span> <span data-ttu-id="f7932-226">A régi típusú és mezőnevek ezen adatok segítségével hello tooquery folytathatja.</span><span class="sxs-lookup"><span data-stu-id="f7932-226">You can continue tooquery for this data using hello old Type and field names.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f7932-227">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="f7932-227">Troubleshooting</span></span>
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a><span data-ttu-id="f7932-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f7932-228">Next steps</span></span>
* <span data-ttu-id="f7932-229">Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletes adatok az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f7932-229">Use [Log searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Azure Key Vault data.</span></span>
