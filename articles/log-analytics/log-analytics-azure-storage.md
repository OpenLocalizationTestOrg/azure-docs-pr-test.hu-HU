---
title: "aaaCollect Azure szolgáltatás naplók és a metrikák Naplóelemzési |} Microsoft Docs"
description: "Diagnosztika konfigurálása az Azure-erőforrások toowrite naplók és a metrikák tooLog elemzés."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1cede9a94ec83c4e3a95853dc2ec355d8df06d6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-azure-service-logs-and-metrics-for-use-in-log-analytics"></a>Azure szolgáltatás és a metrikák Naplóelemzési használható gyűjtése

Folyamatosan naplókat és az Azure-szolgáltatások metrikáját négy különböző módja van:

1. Az Azure diagnostics közvetlen tooLog Analytics (*diagnosztika* a következő táblázat hello)
2. Az Azure diagnostics tooAzure tárolási tooLog Analytics (*tárolási* a következő táblázat hello)
3. Az Azure szolgáltatások összekötők (*összekötők* a következő táblázat hello)
4. A Naplóelemzési (üres a következő táblázat hello és szolgáltatásokról, amelyek nem szerepelnek a listán) a toocollect, majd a post-adatokat parancsfájlok


| Szolgáltatás                 | Erőforrás típusa                           | Logs        | Mérőszámok     | Megoldás |
| --- | --- | --- | --- | --- |
| Alkalmazásátjárók    | Microsoft.Network/applicationGateways   | Diagnosztika | Diagnosztika | [Az Azure alkalmazás átjáró elemzés](log-analytics-azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-log-analytics) |
| Az Application insights    |                                         | összekötő   | összekötő   | [Application Insights-összekötő](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (előzetes verzió) |
| Automation-fiókok     | Microsoft.Automation/AutomationAccounts | Diagnosztika |             | [További információ](../automation/automation-manage-send-joblogs-log-analytics.md)|
| Batch-fiókok          | Microsoft.Batch/batchAccounts           | Diagnosztika | Diagnosztika | |
| Klasszikus cloud services csomag  |                                         | Storage     |             | [További információ](log-analytics-azure-storage-iis-table.md) |
| Cognitive Services      | Microsoft.CognitiveServices/accounts    |             | Diagnosztika | |
| A Data Lake analytics     | Microsoft.DataLakeAnalytics/accounts    | Diagnosztika |             | |
| A Data Lake store         | Microsoft.DataLakeStore/accounts        | Diagnosztika |             | |
| Event Hub névtér     | Microsoft.EventHub/namespaces           | Diagnosztika | Diagnosztika | |
| IoT-központok                | Microsoft.Devices/IotHubs               |             | Diagnosztika | |
| Key Vault               | Microsoft.KeyVault/vaults               | Diagnosztika |             | [KeyVault elemzés](log-analytics-azure-key-vault.md) |
| Terheléselosztók          | Microsoft.Network/loadBalancers         | Diagnosztika |             |  |
| Logic Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Diagnosztika | Diagnosztika | |
| Network Security Groups (Hálózati biztonsági csoportok) | Microsoft.Network/networksecuritygroups | Diagnosztika |             | [Azure hálózati biztonsági csoport elemzés](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) |
| Helyreállítási tárolók         | Microsoft.RecoveryServices/vaults       |             |             | [Az Azure Recovery Services Analytics (előzetes verzió)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
| Szolgáltatások keresése         | Microsoft.Search/searchServices         | Diagnosztika | Diagnosztika | |
| Service Bus-névtér   | Microsoft.ServiceBus/namespaces         | Diagnosztika | Diagnosztika | [Service Bus Analytics (előzetes verzió)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
| Service Fabric          |                                         | Storage     |             | [Service Fabric Analytics (előzetes verzió)](log-analytics-service-fabric.md) |
| SQL (12-es verzió)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Diagnosztika | [Az Azure SQL elemzés (előzetes verzió)](log-analytics-azure-sql.md) |
| Storage                 |                                         |             | Szkript      | [Az Azure Storage Analytics (előzetes verzió)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution) |
| Virtuális gépek        | Microsoft.Compute/virtualMachines       | Mellék   | Mellék <br> Diagnosztika  | |
| Virtuális gépek méretezési csoportok | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Diagnosztika | |
| Kiszolgáló-webfarmok        | Microsoft.Web/serverfarms               |             | Diagnosztika | |
| Webhelyek               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Diagnosztika | [Az Azure Web Apps Analytics (előzetes verzió)](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) |


> [!NOTE]
> A figyeléshez az Azure virtuális gépek (Linux és Windows rendszerekhez), azt javasoljuk, hello telepítése [napló Analytics Virtuálisgép-bővítmény](log-analytics-azure-vm-extension.md). hello ügynök biztosít a virtuális gépekről gyűjtött insights. Használhatja a hello bővítményt a virtuálisgép-méretezési készlet.
>
>

## <a name="azure-diagnostics-direct-toolog-analytics"></a>Az Azure diagnostics közvetlen tooLog elemzés
Sok Azure-erőforrások képes toowrite diagnosztikai naplók és a metrikák közvetlen tooLog elemzés, és ez egy elsődleges hello mód a hello adatgyűjtés elemzés céljából. Az Azure diagnostics használatakor adatot ír azonnal tooLog elemzés, és nincs szükség toofirst írási hello adatok toostorage van.

Azure-erőforrások, amely támogatja a [Azure figyelő](../monitoring-and-diagnostics/monitoring-overview.md) metrikák, valamint a naplókat elküldheti közvetlenül tooLog elemzés.

* Az elérhető mérőszámok hello hello részletekért lásd a túl[támogatott Azure-figyelő metrikák](../monitoring-and-diagnostics/monitoring-supported-metrics.md).
* Hello elérhető naplók hello részletekért lásd a túl[szolgáltatások és a séma támogatja a diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

### <a name="enable-diagnostics-with-powershell"></a>Diagnosztika engedélyezése a PowerShell-lel
Hello November 2016 (v2.3.0) vagy későbbi kiadásában a szükséges [Azure PowerShell](/powershell/azure/overview).

a következő PowerShell-példa azt mutatja meg hogyan hello toouse [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting) tooenable diagnosztika a hálózati biztonsági csoport. hello ugyanezt a megközelítést akkor működik, ha minden támogatott erőforrás - beállítása `$resourceId` tooenable diagnosztika a kívánt hello erőforrás toohello erőforrás-azonosító.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzureRmDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>A Resource Manager-sablonok diagnosztika engedélyezése

Ha létrejön, és hello diagnosztika elküldött tooyour Naplóelemzési munkaterület alatt is használhatja a sablont hasonló toohello egy erőforrás diagnosztikai tooenable. Ebben a példában az Automation-fiók, de minden támogatott erőforrástípusai működik szolgál.

```json
        {
            "type": "Microsoft.Automation/automationAccounts/providers/diagnosticSettings",
            "name": "[concat(parameters('omsAutomationAccountName'), '/', 'Microsoft.Insights/service')]",
            "apiVersion": "2015-07-01",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('omsAutomationAccountName'))]",
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspaceName'))]"
            ],
            "properties": {
                "workspaceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('omsWorkspaceName'))]",
                "logs": [
                    {
                        "category": "JobLogs",
                        "enabled": true
                    },
                    {
                        "category": "JobStreams",
                        "enabled": true
                    }
                ]
            }
        }
```

[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="azure-diagnostics-toostorage-then-toolog-analytics"></a>Az Azure diagnostics toostorage majd tooLog elemzés

Az egyes erőforrások belül a naplók gyűjtésére, lehetséges toosend hello naplók tooAzure tárolási és Naplóelemzési tooread hello naplók tárolásból konfigurálja.

A Naplóelemzési használhatja ezt a módszert toocollect diagnosztika az Azure storage hello erőforrások és a naplók a következő:

| Erőforrás | Logs |
| --- | --- |
| Service Fabric |ETWEvent <br> Működési események <br> Megbízható szereplő esemény <br> Megbízható szolgáltatás esemény |
| Virtuális gépek |Linux rendszernaplójába bejegyzett <br> Windows-esemény <br> IIS-napló <br> Windows ETWEvent |
| Webes szerepkörök <br> Feldolgozói szerepkörök |Linux rendszernaplójába bejegyzett <br> Windows-esemény <br> IIS-napló <br> Windows ETWEvent |

> [!NOTE]
> Van szó, tárolás és diagnosztika tooa tárfiók küldésekor tranzakciók és mikor Naplóelemzési hello adatokat olvas a tárfiók a Azure megszokott adatforgalmi díjakat.
>
>

Lásd: [blob-tároló használja az IIS és a tábla tárolási események](log-analytics-azure-storage-iis-table.md) toolearn további információk a Naplóelemzési hogyan be ezeket a naplókat.

## <a name="connectors-for-azure-services"></a>Az Azure szolgáltatások összekötők

Nincs összekötőként szolgál az Application Insights részére, amely lehetővé teszi, hogy az Application Insights toobe tooLog Analytics küldött által összegyűjtött adatokat.

További tudnivalók hello [Application Insights-összekötő](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-toocollect-and-post-data-toolog-analytics"></a>Parancsfájlok toocollect és a post data tooLog elemzés

Az Azure-szolgáltatásokat, amelyek nem rendelkeznek közvetlen módon toosend naplók és a metrikák tooLog Analytics egy Azure Automation-parancsfájl toocollect is használhatja a napló és a metrikák hello. hello parancsfájlt is majd küldési hello tooLog Analytics használatával végzett hello [adatgyűjtő API](log-analytics-data-collector-api.md)

hello Azure sablon gyűjteménye van [példák Azure Automation](https://azure.microsoft.com/en-us/resources/templates/?term=OMS) szolgáltatások, és küldje el azt tooLog Analytics toocollect adatait.

## <a name="next-steps"></a>Következő lépések

* [A blob storage használata az IIS és a tábla tárolási események](log-analytics-azure-storage-iis-table.md) tooread hello naplók az Azure-szolgáltatásokat, hogy az írási tootable diagnosztika tároló- és IIS írásbeli tooblob tárolási naplózza.
* [Megoldások](log-analytics-add-solutions.md) hello adatok tooprovide betekintést.
* [Használja a keresési lekérdezések](log-analytics-log-searches.md) tooanalyze hello adatokat.
