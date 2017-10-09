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
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure REST API forgatókönyv figyelése
Ez a cikk bemutatja, hogyan tooperform hitelesítés, a kód használhassa hello [Microsoft Azure figyelő REST API-referencia](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

hello Azure figyelő API segítségével lehetséges tooprogrammatically lekérése hello elérhető alapértelmezett metrikai meghatározásainak (hello típus mérőszám például CPU-idő, a kérelmeket, stb.), a részletesség és a metrika értékek. Miután beolvasni, hello adatokat egy külön adattár, például az Azure SQL Database, az Azure Cosmos DB vagy az Azure Data Lake is menthető. Innen további elemzés végrehajtható igény szerint.

Mellett különböző metrika adatpontok dolgozik, mivel ez a cikk bemutatja, hello figyelő API segítségével lehetséges toolist riasztási szabályok tevékenység naplók megtekintése és még sok más. Elérhető műveletek teljes listáját lásd: hello [Microsoft Azure figyelő REST API-referencia](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Az Azure figyelő kérések hitelesítése
első lépés hello tooauthenticate hello kérelem.

Üdvözlő feladataival célmodellre hello Azure figyelő API használata hello Azure Resource Manager hitelesítési. Ezért összes kérelmet hitelesíteni kell az Azure Active Directoryval (Azure AD). Több megközelítés tooauthenticate hello ügyfélalkalmazás toocreate egy Azure AD szolgáltatás egyszerű és hello hitelesítési (JWT)-jogkivonatot lekérdezni. hello alábbi mintaparancsfájl azt mutatja be, létrehozása az Azure AD szolgáltatás egyszerű PowerShell segítségével. A részletes útmutató, tekintse meg a toohello dokumentációját a [Azure PowerShell toocreate a szolgáltatás egyszerű tooaccess erőforrásokat használó](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password). Lehetőség arra is túl[hello Azure-portálon keresztül szolgáltatásnevet létrehozni](../azure-resource-manager/resource-group-create-service-principal-portal.md).

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

tooquery hello Azure figyelő API hello ügyfélalkalmazás korábban hozott létre a szolgáltatás egyszerű tooauthenticate hello kell használnia. a következő példa PowerShell parancsfájl hello jeleníti meg az egyik módszer használatával hello [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) toohelp hello JWT hitelesítési token beszerzése. hello JWT jogkivonat egy HTTP-engedélyezési paraméter, a kérelmek toohello Azure figyelő REST API részeként lett átadva.

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

Hello hitelesítés beállítása lépés végrehajtása után lekérdezések majd elleni hello Azure figyelő REST API hajtható végre. Számos hasznos két lekérdezést.

1. Lista hello metrikai meghatározásainak erőforrás
2. Hello metrika értékek beolvasása

## <a name="retrieve-metric-definitions"></a>Metrikai meghatározásainak beolvasása
> [!NOTE]
> tooretrieve metrikai meghatározásainak hello Azure figyelő REST API-t használó "2016-03-01" hello API-verziót használja.
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
Az Azure Logic Apps hello metrikai meghatározásainak jelent a következő képernyőkép hasonló toohello:

![ALT "Metrika definition válasz JSON nézet".](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

További információkért lásd: hello [hello metrikai meghatározásainak Azure figyelő REST API-t az erőforráshoz tartozó listában](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentációját.

## <a name="retrieve-metric-values"></a>Metrika értékek beolvasása
Hello elérhető metrikai meghatározásainak ismert, ha van, majd a lehetséges tooretrieve hello kapcsolódó metrika értékek. Hello metrika neve "érték" (nem a "localizedValue" hello) használata a szűrési kéréseit (például lekérése hello "CpuTime" és "Kérelmek" metrika adatpontok). Ha nincs szűrők megadását, hello alapértelmezett metrika ad vissza.

> [!NOTE]
> tooretrieve metrika értékek hello Azure figyelő REST API-t használó "2016-06-01" hello API-verziót használja.
>
>

**Módszer**: beolvasása

**A kérelmi URI**: https://management.azure.com/subscriptions/*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/*{erőforrás-szolgáltató-namespace}*/*{erőforrástípus-}*/*{erőforrásnév}*/providers/microsoft.insights/metrics?$filter=*{szűrő}*& api-version =*{apiVersion}*

Például tooretrieve hello RunsSucceeded metrika adatpontok megadott időtartomány hello és egy metrikaindítójának aggregációs időköze 1 óra, hello kérelem a következőképpen nézne ki:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

hello eredmény megjelenik a következő képernyőkép hasonló toohello példa:

![ALT "JSON-válasz megjelenítő átlagos válaszideje Átjárómetrika értékeként"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

tooretrieve több adat- vagy Összesítés mutat, adja hozzá, hello metrika definition nevek és összesítési típusok toohello szűrő hello a következő példában látható módon:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>ARMClient használata
Egy alternatív toousing PowerShell (ahogy fent látható), akkor toouse [ARMClient](https://github.com/projectkudu/ARMClient) a Windows-számítógépre. ARMClient automatikusan kezeli hello Azure AD hitelesítési (és eredményül kapott JWT jogkivonat). hello lépései metrika adatok beolvasása ARMClient használatát:

1. Telepítés [Chocolatey](https://chocolatey.org/) és [ARMClient](https://github.com/projectkudu/ARMClient).
2. Írja be egy terminálablakot, *armclient.exe bejelentkezési*. Ez kéri a tooAzure toolog.
3. Típus *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. Típus *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*

![ALT "Hello Azure figyelés REST API-t a Using ARMClient toowork"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-hello-resource-id"></a>Hello erőforrás-azonosító lekérése
Hello REST API használatával valóban segítségével toounderstand hello elérhető metrikai meghatározásainak, a részletesség és a kapcsolódó értékek. Ez az információ akkor hasznos, ha hello segítségével [Azure könyvtár](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Hello megelőző kódot erőforrás-azonosító toouse hello esetén hello teljes elérési útja toohello kívánt Azure-erőforrás. Például az Azure Web Apps elleni tooquery, hello erőforrás-azonosító a következő lesz:

*/Subscriptions/{Subscription-ID}/resourceGroups/{Resource-group-name}/Providers/Microsoft.Web/Sites/{Site-Name}/*

hello alábbi lista néhány példa a különböző Azure-erőforrások erőforrás azonosító formátuma.

* **Az IoT-központ** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Devices/IotHubs/*{iot-központ-neve}*
* **A rugalmas SQL-készlet** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Sql/servers/*{alkalmazáskészlet-db}*/elasticpools/*{sql-alkalmazáskészlet-neve}*
* **SQL-adatbázis (v12)** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Sql/servers/*{kiszolgálónév}*/databases/*{adatbázisnév}*
* **A Service Bus** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.ServiceBus/*{névtér}*/*{szolgáltatásbusz-neve}*
* **Virtuálisgép-méretezési készlet** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{virtuálisgép-név}*
* **Virtuális gépek** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.Compute/virtualMachines/*{virtuálisgép-név}*
* **Az Event Hubs** -következő*{előfizetés-azonosító}*/resourceGroups/*{csoport-erőforrásnév}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*

Nincsenek alternatív módszerek tooretrieving hello erőforrás-azonosító, beleértve az Azure erőforrás-kezelő, a hello Azure-portálon, és PowerShell vagy Azure CLI hello hello szükséges erőforrás megtekintése.

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
toofind hello erőforrás-azonosító a kívánt erőforrás egyik hasznos módszer toouse hello [Azure erőforrás-kezelő](https://resources.azure.com) eszköz. Keresse meg a szükséges toohello erőforrás, és keresse meg, is látható, ahogy a következő képernyőkép hello hello Azonosítóval:

!["Az Azure erőforrás-kezelő" ALT](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure Portal
hello erőforrás-azonosító is lehet lekérni hello Azure-portálon. toodo Igen, keresse meg a szükséges toohello erőforrás, és válassza a Tulajdonságok parancsot. hello erőforrás-azonosító hello tulajdonságok panelére léphet, jelenik meg a következő képernyőkép hello látható módon:

![ALT "erőforrás-azonosító hello Azure-portálon hello tulajdonságok paneljén megjelenő"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
hello erőforrás-azonosító, valamint az Azure PowerShell-parancsmagok használatával lehet beolvasni. Tooobtain hello erőforrás-azonosító az Azure Web Apps, például hello Get-AzureRmWebApp parancsmag, mint a következő képernyőkép hello hajtható végre:

![ALT "erőforrás-azonosító Powershellen keresztül kapott"](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
tooretrieve hello hello Azure CLI használata az erőforrás-azonosító, hello "azure webalkalmazás megjelenítése" parancs megadásával hello "--json" lehetőséget, ahogy az alábbi képernyőfelvétel a hello:

![ALT "erőforrás-azonosító Powershellen keresztül kapott"](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Tevékenység napló adatainak beolvasása
A metrikai meghatározásainak és a kapcsolódó értékekről tooworking hozzáadását esetében is lehetséges tooretrieve további érdekes insights kapcsolódó tooAzure erőforrásokat. Például lehetséges tooquery [tevékenységnapló](https://msdn.microsoft.com/library/azure/dn931934.aspx) adatokat. hello következő mutatja be az Azure-előfizetés hello Azure figyelő REST API tooquery tevékenység naplóadatokat egy adott dátumtartományban használatával:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Következő lépések
* Felülvizsgálati hello [Figyelés áttekintése](monitoring-overview.md).
* Nézet hello [támogatott Azure-figyelő metrikák](monitoring-supported-metrics.md).
* Felülvizsgálati hello [Microsoft Azure figyelő REST API-referencia](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Felülvizsgálati hello [Azure könyvtár](https://msdn.microsoft.com/library/azure/mt417623.aspx).
