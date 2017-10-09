---
title: "Figyelés REST API-forgatókönyv aaaAzure |} Microsoft Docs"
description: "Hogyan tooauthenticate kérelmek tooand használata hello Azure figyelési REST API-t."
author: mcollier
manager: 
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 565e6a88-3131-4a48-8b82-3effc9a3d5c6
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: mcollier
ms.openlocfilehash: b8ae3a03fd21af872f1dc5fed40a101a24ca1652
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitoring-rest-api-walkthrough"></a><span data-ttu-id="9ef10-103">Azure REST API forgatókönyv figyelése</span><span class="sxs-lookup"><span data-stu-id="9ef10-103">Azure Monitoring REST API Walkthrough</span></span>
<span data-ttu-id="9ef10-104">Ez a cikk bemutatja, hogyan tooperform hitelesítés, a kód használhassa hello [Microsoft Azure figyelő REST API-referencia](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ef10-104">This article shows you how tooperform authentication so your code can use hello [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>         

<span data-ttu-id="9ef10-105">hello Azure figyelő API segítségével lehetséges tooprogrammatically lekérése hello elérhető alapértelmezett metrikai meghatározásainak (hello típus mérőszám például CPU-idő, a kérelmeket, stb.), a részletesség és a metrika értékek.</span><span class="sxs-lookup"><span data-stu-id="9ef10-105">hello Azure Monitor API makes it possible tooprogrammatically retrieve hello available default metric definitions (hello type of metric such as CPU Time, Requests, etc.), granularity, and metric values.</span></span> <span data-ttu-id="9ef10-106">Miután beolvasni, hello adatokat egy külön adattár, például az Azure SQL Database, az Azure Cosmos DB vagy az Azure Data Lake is menthető.</span><span class="sxs-lookup"><span data-stu-id="9ef10-106">Once retrieved, hello data can be saved in a separate data store such as Azure SQL Database, Azure Cosmos DB, or Azure Data Lake.</span></span> <span data-ttu-id="9ef10-107">Innen további elemzés végrehajtható igény szerint.</span><span class="sxs-lookup"><span data-stu-id="9ef10-107">From there additional analysis can be performed as needed.</span></span>

<span data-ttu-id="9ef10-108">Mellett különböző metrika adatpontok dolgozik, mivel ez a cikk bemutatja, hello figyelő API segítségével lehetséges toolist riasztási szabályok tevékenység naplók megtekintése és még sok más.</span><span class="sxs-lookup"><span data-stu-id="9ef10-108">Besides working with various metric data points, as this article demonstrates, hello Monitor API makes it possible toolist alert rules, view activity logs, and much more.</span></span> <span data-ttu-id="9ef10-109">Elérhető műveletek teljes listáját lásd: hello [Microsoft Azure figyelő REST API-referencia](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ef10-109">For a full list of available operations, see hello [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>

## <a name="authenticating-azure-monitor-requests"></a><span data-ttu-id="9ef10-110">Az Azure figyelő kérések hitelesítése</span><span class="sxs-lookup"><span data-stu-id="9ef10-110">Authenticating Azure Monitor Requests</span></span>
<span data-ttu-id="9ef10-111">első lépés hello tooauthenticate hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="9ef10-111">hello first step is tooauthenticate hello request.</span></span>

<span data-ttu-id="9ef10-112">Üdvözlő feladataival célmodellre hello Azure figyelő API használata hello Azure Resource Manager hitelesítési.</span><span class="sxs-lookup"><span data-stu-id="9ef10-112">All hello tasks executed against hello Azure Monitor API use hello Azure Resource Manager authentication model.</span></span> <span data-ttu-id="9ef10-113">Ezért összes kérelmet hitelesíteni kell az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9ef10-113">Therefore, all requests must be authenticated with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="9ef10-114">Több megközelítés tooauthenticate hello ügyfélalkalmazás toocreate egy Azure AD szolgáltatás egyszerű és hello hitelesítési (JWT)-jogkivonatot lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="9ef10-114">One approach tooauthenticate hello client application is toocreate an Azure AD service principal and retrieve hello authentication (JWT) token.</span></span> <span data-ttu-id="9ef10-115">hello alábbi mintaparancsfájl azt mutatja be, létrehozása az Azure AD szolgáltatás egyszerű PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="9ef10-115">hello following sample script demonstrates creating an Azure AD service principal via PowerShell.</span></span> <span data-ttu-id="9ef10-116">A részletes útmutató, tekintse meg a toohello dokumentációját a [Azure PowerShell toocreate a szolgáltatás egyszerű tooaccess erőforrásokat használó](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span><span class="sxs-lookup"><span data-stu-id="9ef10-116">For a more detailed walk-through, refer toohello documentation on [using Azure PowerShell toocreate a service principal tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span></span> <span data-ttu-id="9ef10-117">Lehetőség arra is túl[hello Azure-portálon keresztül szolgáltatásnevet létrehozni](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9ef10-117">It is also possible too[create a service principal via hello Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate tooa specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for hello service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with hello designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role toohello newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

<span data-ttu-id="9ef10-118">tooquery hello Azure figyelő API hello ügyfélalkalmazás korábban hozott létre a szolgáltatás egyszerű tooauthenticate hello kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9ef10-118">tooquery hello Azure Monitor API, hello client application should use hello previously created service principal tooauthenticate.</span></span> <span data-ttu-id="9ef10-119">a következő példa PowerShell parancsfájl hello jeleníti meg az egyik módszer használatával hello [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) toohelp hello JWT hitelesítési token beszerzése.</span><span class="sxs-lookup"><span data-stu-id="9ef10-119">hello following example PowerShell script shows one approach, using hello [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) toohelp get hello JWT authentication token.</span></span> <span data-ttu-id="9ef10-120">hello JWT jogkivonat egy HTTP-engedélyezési paraméter, a kérelmek toohello Azure figyelő REST API részeként lett átadva.</span><span class="sxs-lookup"><span data-stu-id="9ef10-120">hello JWT token is passed as part of an HTTP Authorization parameter in requests toohello Azure Monitor REST API.</span></span>

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

<span data-ttu-id="9ef10-121">Hello hitelesítés beállítása lépés végrehajtása után lekérdezések majd elleni hello Azure figyelő REST API hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="9ef10-121">Once hello authentication setup step is complete, queries can then be executed against hello Azure Monitor REST API.</span></span> <span data-ttu-id="9ef10-122">Számos hasznos két lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="9ef10-122">There are two helpful queries:</span></span>

1. <span data-ttu-id="9ef10-123">Lista hello metrikai meghatározásainak erőforrás</span><span class="sxs-lookup"><span data-stu-id="9ef10-123">List hello metric definitions for a resource</span></span>
2. <span data-ttu-id="9ef10-124">Hello metrika értékek beolvasása</span><span class="sxs-lookup"><span data-stu-id="9ef10-124">Retrieve hello metric values</span></span>

## <a name="retrieve-metric-definitions"></a><span data-ttu-id="9ef10-125">Metrikai meghatározásainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="9ef10-125">Retrieve Metric Definitions</span></span>
> [!NOTE]
> <span data-ttu-id="9ef10-126">tooretrieve metrikai meghatározásainak hello Azure figyelő REST API-t használó "2016-03-01" hello API-verziót használja.</span><span class="sxs-lookup"><span data-stu-id="9ef10-126">tooretrieve metric definitions using hello Azure Monitor REST API, use "2016-03-01" as hello API version.</span></span>
>
>

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
<span data-ttu-id="9ef10-127">Az Azure Logic Apps hello metrikai meghatározásainak jelent a következő képernyőkép hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="9ef10-127">For an Azure Logic App, hello metric definitions would appear similar toohello following screenshot:</span></span>

![ALT "Metrika definition válasz JSON nézet".](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

<span data-ttu-id="9ef10-129">További információkért lásd: hello [hello metrikai meghatározásainak Azure figyelő REST API-t az erőforráshoz tartozó listában](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="9ef10-129">For more information, see hello [List hello metric definitions for a resource in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) documentation.</span></span>

## <a name="retrieve-metric-values"></a><span data-ttu-id="9ef10-130">Metrika értékek beolvasása</span><span class="sxs-lookup"><span data-stu-id="9ef10-130">Retrieve Metric Values</span></span>
<span data-ttu-id="9ef10-131">Hello elérhető metrikai meghatározásainak ismert, ha van, majd a lehetséges tooretrieve hello kapcsolódó metrika értékek.</span><span class="sxs-lookup"><span data-stu-id="9ef10-131">Once hello available metric definitions are known, it is then possible tooretrieve hello related metric values.</span></span> <span data-ttu-id="9ef10-132">Hello metrika neve "érték" (nem a "localizedValue" hello) használata a szűrési kéréseit (például lekérése hello "CpuTime" és "Kérelmek" metrika adatpontok).</span><span class="sxs-lookup"><span data-stu-id="9ef10-132">Use hello metric’s name ‘value’ (not hello ‘localizedValue’) for any filtering requests (for example, retrieve hello ‘CpuTime’ and ‘Requests’ metric data points).</span></span> <span data-ttu-id="9ef10-133">Ha nincs szűrők megadását, hello alapértelmezett metrika ad vissza.</span><span class="sxs-lookup"><span data-stu-id="9ef10-133">If no filters are specified, hello default metric is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="9ef10-134">tooretrieve metrika értékek hello Azure figyelő REST API-t használó "2016-06-01" hello API-verziót használja.</span><span class="sxs-lookup"><span data-stu-id="9ef10-134">tooretrieve metric values using hello Azure Monitor REST API, use "2016-06-01" as hello API version.</span></span>
>
>

<span data-ttu-id="9ef10-135">**Módszer**: beolvasása</span><span class="sxs-lookup"><span data-stu-id="9ef10-135">**Method**: GET</span></span>

<span data-ttu-id="9ef10-136">**A kérelmi URI**: https://management.azure.com/subscriptions/*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/*{erőforrás-szolgáltató-namespace}*/*{erőforrástípus-}*/*{erőforrásnév}*/providers/microsoft.insights/metrics?$filter=*{szűrő}*& api-version =*{apiVersion}*</span><span class="sxs-lookup"><span data-stu-id="9ef10-136">**Request URI**: https://management.azure.com/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?$filter=*{filter}*&api-version=*{apiVersion}*</span></span>

<span data-ttu-id="9ef10-137">Például tooretrieve hello RunsSucceeded metrika adatpontok megadott időtartomány hello és egy metrikaindítójának aggregációs időköze 1 óra, hello kérelem a következőképpen nézne ki:</span><span class="sxs-lookup"><span data-stu-id="9ef10-137">For example, tooretrieve hello RunsSucceeded metric data points for hello given time range and for a time grain of 1 hour, hello request would be as follows:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

<span data-ttu-id="9ef10-138">hello eredmény megjelenik a következő képernyőkép hasonló toohello példa:</span><span class="sxs-lookup"><span data-stu-id="9ef10-138">hello result would appear similar toohello example following screenshot:</span></span>

![ALT "JSON-válasz megjelenítő átlagos válaszideje Átjárómetrika értékeként"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

<span data-ttu-id="9ef10-140">tooretrieve több adat- vagy Összesítés mutat, adja hozzá, hello metrika definition nevek és összesítési típusok toohello szűrő hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="9ef10-140">tooretrieve multiple data or aggregation points, add hello metric definition names and aggregation types toohello filter, as seen in hello following example:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a><span data-ttu-id="9ef10-141">ARMClient használata</span><span class="sxs-lookup"><span data-stu-id="9ef10-141">Use ARMClient</span></span>
<span data-ttu-id="9ef10-142">Egy alternatív toousing PowerShell (ahogy fent látható), akkor toouse [ARMClient](https://github.com/projectkudu/ARMClient) a Windows-számítógépre.</span><span class="sxs-lookup"><span data-stu-id="9ef10-142">An alternative toousing PowerShell (as shown above), is toouse [ARMClient](https://github.com/projectkudu/ARMClient) on your Windows machine.</span></span> <span data-ttu-id="9ef10-143">ARMClient automatikusan kezeli hello Azure AD hitelesítési (és eredményül kapott JWT jogkivonat).</span><span class="sxs-lookup"><span data-stu-id="9ef10-143">ARMClient handles hello Azure AD authentication (and resulting JWT token) automatically.</span></span> <span data-ttu-id="9ef10-144">hello lépései metrika adatok beolvasása ARMClient használatát:</span><span class="sxs-lookup"><span data-stu-id="9ef10-144">hello following steps outline use of ARMClient for retrieving metric data:</span></span>

1. <span data-ttu-id="9ef10-145">Telepítés [Chocolatey](https://chocolatey.org/) és [ARMClient](https://github.com/projectkudu/ARMClient).</span><span class="sxs-lookup"><span data-stu-id="9ef10-145">Install [Chocolatey](https://chocolatey.org/) and [ARMClient](https://github.com/projectkudu/ARMClient).</span></span>
2. <span data-ttu-id="9ef10-146">Írja be egy terminálablakot, *armclient.exe bejelentkezési*.</span><span class="sxs-lookup"><span data-stu-id="9ef10-146">In a terminal window, type *armclient.exe login*.</span></span> <span data-ttu-id="9ef10-147">Ez kéri a tooAzure toolog.</span><span class="sxs-lookup"><span data-stu-id="9ef10-147">This prompts you toolog in tooAzure.</span></span>
3. <span data-ttu-id="9ef10-148">Típus *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span><span class="sxs-lookup"><span data-stu-id="9ef10-148">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span></span>
4. <span data-ttu-id="9ef10-149">Típus *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span><span class="sxs-lookup"><span data-stu-id="9ef10-149">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span></span>

![ALT "Hello Azure figyelés REST API-t a Using ARMClient toowork"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-hello-resource-id"></a><span data-ttu-id="9ef10-151">Hello erőforrás-azonosító lekérése</span><span class="sxs-lookup"><span data-stu-id="9ef10-151">Retrieve hello Resource ID</span></span>
<span data-ttu-id="9ef10-152">Hello REST API használatával valóban segítségével toounderstand hello elérhető metrikai meghatározásainak, a részletesség és a kapcsolódó értékek.</span><span class="sxs-lookup"><span data-stu-id="9ef10-152">Using hello REST API can really help toounderstand hello available metric definitions, granularity, and related values.</span></span> <span data-ttu-id="9ef10-153">Ez az információ akkor hasznos, ha hello segítségével [Azure könyvtár](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ef10-153">That information is helpful when using hello [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>

<span data-ttu-id="9ef10-154">Hello megelőző kódot erőforrás-azonosító toouse hello esetén hello teljes elérési útja toohello kívánt Azure-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="9ef10-154">For hello preceding code, hello resource ID toouse is hello full path toohello desired Azure resource.</span></span> <span data-ttu-id="9ef10-155">Például az Azure Web Apps elleni tooquery, hello erőforrás-azonosító a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="9ef10-155">For example, tooquery against an Azure Web App, hello resource ID would be:</span></span>

<span data-ttu-id="9ef10-156">*/Subscriptions/{Subscription-ID}/resourceGroups/{Resource-group-name}/Providers/Microsoft.Web/Sites/{Site-Name}/*</span><span class="sxs-lookup"><span data-stu-id="9ef10-156">*/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{site-name}/*</span></span>

<span data-ttu-id="9ef10-157">hello alábbi lista néhány példa a különböző Azure-erőforrások erőforrás azonosító formátuma.</span><span class="sxs-lookup"><span data-stu-id="9ef10-157">hello following list contains a few examples of resource ID formats for various Azure resources:</span></span>

* <span data-ttu-id="9ef10-158">**Az IoT-központ** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Devices/IotHubs/*{iot-központ-neve}*</span><span class="sxs-lookup"><span data-stu-id="9ef10-158">**IoT Hub** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*</span></span>
* <span data-ttu-id="9ef10-159">**A rugalmas SQL-készlet** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Sql/servers/*{alkalmazáskészlet-db}*/elasticpools/*{sql-alkalmazáskészlet-neve}*</span><span class="sxs-lookup"><span data-stu-id="9ef10-159">**Elastic SQL Pool** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*</span></span>
* <span data-ttu-id="9ef10-160">**SQL-adatbázis (v12)** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Sql/servers/*{kiszolgálónév}*/databases/*{adatbázisnév}*</span><span class="sxs-lookup"><span data-stu-id="9ef10-160">**SQL Database (v12)** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{server-name}*/databases/*{database-name}*</span></span>
* <span data-ttu-id="9ef10-161">**A Service Bus** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.ServiceBus/*{névtér}*/*{szolgáltatásbusz-neve}*</span><span class="sxs-lookup"><span data-stu-id="9ef10-161">**Service Bus** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.ServiceBus/*{namespace}*/*{servicebus-name}*</span></span>
* <span data-ttu-id="9ef10-162">**Virtuálisgép-méretezési készlet** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{virtuálisgép-név}*</span><span class="sxs-lookup"><span data-stu-id="9ef10-162">**VM Scale Sets** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{vm-name}*</span></span>
* <span data-ttu-id="9ef10-163">**Virtuális gépek** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Compute/virtualMachines/*{virtuálisgép-név}*</span><span class="sxs-lookup"><span data-stu-id="9ef10-163">**VMs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*</span></span>
* <span data-ttu-id="9ef10-164">**Az Event Hubs** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*</span><span class="sxs-lookup"><span data-stu-id="9ef10-164">**Event Hubs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*</span></span>

<span data-ttu-id="9ef10-165">Nincsenek alternatív módszerek tooretrieving hello erőforrás-azonosító, beleértve az Azure erőforrás-kezelő, a hello Azure-portálon, és PowerShell vagy Azure CLI hello hello szükséges erőforrás megtekintése.</span><span class="sxs-lookup"><span data-stu-id="9ef10-165">There are alternative approaches tooretrieving hello resource ID, including using Azure Resource Explorer, viewing hello desired resource in hello Azure portal, and via PowerShell or hello Azure CLI.</span></span>

### <a name="azure-resource-explorer"></a><span data-ttu-id="9ef10-166">Azure Resource Explorer</span><span class="sxs-lookup"><span data-stu-id="9ef10-166">Azure Resource Explorer</span></span>
<span data-ttu-id="9ef10-167">toofind hello erőforrás-azonosító a kívánt erőforrás egyik hasznos módszer toouse hello [Azure erőforrás-kezelő](https://resources.azure.com) eszköz.</span><span class="sxs-lookup"><span data-stu-id="9ef10-167">toofind hello resource ID for a desired resource, one helpful approach is toouse hello [Azure Resource Explorer](https://resources.azure.com) tool.</span></span> <span data-ttu-id="9ef10-168">Keresse meg a szükséges toohello erőforrás, és keresse meg, is látható, ahogy a következő képernyőkép hello hello Azonosítóval:</span><span class="sxs-lookup"><span data-stu-id="9ef10-168">Navigate toohello desired resource and then look at hello ID shown, as in hello following screenshot:</span></span>

!["Az Azure erőforrás-kezelő" ALT](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a><span data-ttu-id="9ef10-170">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9ef10-170">Azure portal</span></span>
<span data-ttu-id="9ef10-171">hello erőforrás-azonosító is lehet lekérni hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9ef10-171">hello resource ID can also be obtained from hello Azure portal.</span></span> <span data-ttu-id="9ef10-172">toodo Igen, keresse meg a szükséges toohello erőforrás, és válassza a Tulajdonságok parancsot.</span><span class="sxs-lookup"><span data-stu-id="9ef10-172">toodo so, navigate toohello desired resource and then select Properties.</span></span> <span data-ttu-id="9ef10-173">hello erőforrás-azonosító hello tulajdonságok panelére léphet, jelenik meg a következő képernyőkép hello látható módon:</span><span class="sxs-lookup"><span data-stu-id="9ef10-173">hello Resource ID is displayed in hello Properties blade, as seen in hello following screenshot:</span></span>

![ALT "erőforrás-azonosító hello Azure-portálon hello tulajdonságok paneljén megjelenő"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a><span data-ttu-id="9ef10-175">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ef10-175">Azure PowerShell</span></span>
<span data-ttu-id="9ef10-176">hello erőforrás-azonosító, valamint az Azure PowerShell-parancsmagok használatával lehet beolvasni.</span><span class="sxs-lookup"><span data-stu-id="9ef10-176">hello resource ID can be retrieved using Azure PowerShell cmdlets as well.</span></span> <span data-ttu-id="9ef10-177">Tooobtain hello erőforrás-azonosító az Azure Web Apps, például hello Get-AzureRmWebApp parancsmag, mint a következő képernyőkép hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="9ef10-177">For example, tooobtain hello resource ID for an Azure Web App, execute hello Get-AzureRmWebApp cmdlet, as in hello following screenshot:</span></span>

![ALT "erőforrás-azonosító Powershellen keresztül kapott"](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a><span data-ttu-id="9ef10-179">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9ef10-179">Azure CLI</span></span>
<span data-ttu-id="9ef10-180">tooretrieve hello hello Azure CLI használata az erőforrás-azonosító, hello "azure webalkalmazás megjelenítése" parancs megadásával hello "--json" lehetőséget, ahogy az alábbi képernyőfelvétel a hello:</span><span class="sxs-lookup"><span data-stu-id="9ef10-180">tooretrieve hello resource ID using hello Azure CLI, execute hello 'azure webapp show' command, specifying hello '--json' option, as shown in hello following screenshot:</span></span>

![ALT "erőforrás-azonosító Powershellen keresztül kapott"](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a><span data-ttu-id="9ef10-182">Tevékenység napló adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="9ef10-182">Retrieve Activity Log Data</span></span>
<span data-ttu-id="9ef10-183">A metrikai meghatározásainak és a kapcsolódó értékekről tooworking hozzáadását esetében is lehetséges tooretrieve további érdekes insights kapcsolódó tooAzure erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="9ef10-183">In addition tooworking with metric definitions and related values, it is also possible tooretrieve additional interesting insights related tooAzure resources.</span></span> <span data-ttu-id="9ef10-184">Például lehetséges tooquery [tevékenységnapló](https://msdn.microsoft.com/library/azure/dn931934.aspx) adatokat.</span><span class="sxs-lookup"><span data-stu-id="9ef10-184">As an example, it is possible tooquery [activity log](https://msdn.microsoft.com/library/azure/dn931934.aspx) data.</span></span> <span data-ttu-id="9ef10-185">hello következő mutatja be az Azure-előfizetés hello Azure figyelő REST API tooquery tevékenység naplóadatokat egy adott dátumtartományban használatával:</span><span class="sxs-lookup"><span data-stu-id="9ef10-185">hello following sample demonstrates using hello Azure Monitor REST API tooquery activity log data within a specific date range for an Azure subscription:</span></span>

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a><span data-ttu-id="9ef10-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ef10-186">Next steps</span></span>
* <span data-ttu-id="9ef10-187">Felülvizsgálati hello [Figyelés áttekintése](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9ef10-187">Review hello [Overview of Monitoring](monitoring-overview.md).</span></span>
* <span data-ttu-id="9ef10-188">Nézet hello [támogatott Azure-figyelő metrikák](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="9ef10-188">View hello [Supported metrics with Azure Monitor](monitoring-supported-metrics.md).</span></span>
* <span data-ttu-id="9ef10-189">Felülvizsgálati hello [Microsoft Azure figyelő REST API-referencia](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ef10-189">Review hello [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>
* <span data-ttu-id="9ef10-190">Felülvizsgálati hello [Azure könyvtár](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ef10-190">Review hello [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>
