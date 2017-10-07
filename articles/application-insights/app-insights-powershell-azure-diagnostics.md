---
title: aaaUsing PowerShell toosetup Application Insights egy Azure-ban |} Microsoft Docs
description: "Konfigurálása Azure Diagnostics toopipe tooApplication Insights automatizálásához."
services: application-insights
documentationcenter: .net
author: sbtron
manager: carmonm
ms.assetid: 4ac803a8-f424-4c0c-b18f-4b9c189a64a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/17/2015
ms.author: bwren
ms.openlocfilehash: c48a5d8eb23df162522860935af876063aaa6976
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-powershell-tooset-up-application-insights-for-an-azure-web-app"></a>Használja fel az Application Insights PowerShell tooset Azure-webalkalmazás
[A Microsoft Azure](https://azure.com) lehet [toosend Azure Diagnostics konfigurált](app-insights-azure-diagnostics.md) túl[Azure Application Insights](app-insights-overview.md). hello diagnosztika tooAzure felhőalapú szolgáltatásokhoz és az Azure virtuális gépek vonatkoznak. Kiegészíti hello telemetriai adatot küld hello Application Insights SDK használatával hello alkalmazáson belül. Részeként hello automatizálása új erőforrás létrehozása az Azure-ban, a PowerShell-diagnosztika konfigurálhatja.

## <a name="azure-template"></a>Azure-sablon
Hello webalkalmazás az Azure-ban, és az Azure Resource Manager-sablonnal erőforrások létrehozása, ha az Application Insights konfigurálhatja a toohello erőforrások csomópont hozzáadása:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-hello Application Insights-erőforrás nevét
* `myWebAppName`-hello webes alkalmazás hello azonosítója

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>A diagnosztikai bővítmény engedélyezése egy felhőszolgáltatás telepítésének részeként
Hello `New-AzureDeployment` parancsmag-paraméterrel rendelkezik `ExtensionConfiguration`, így tovább is diagnosztika konfigurációk tömbjét. Ezek segítségével hozhatók létre hello `New-AzureServiceDiagnosticsExtensionConfig` parancsmag. Példa:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>A diagnosztikai bővítmény engedélyezése egy meglévő felhőszolgáltatáson
Meglévő szolgáltatáson használja a következőt: `Set-AzureServiceDiagnosticsExtension`.

```ps

    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Az aktuális diagnosztikai bővítmény konfigurációjának lekérése
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>A diagnosztikai bővítmény eltávolítása
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Ha engedélyezte a hello diagnosztika bővítmény használatával `Set-AzureServiceDiagnosticsExtension` vagy `New-AzureServiceDiagnosticsExtensionConfig` hello szerepkör paramétert, majd eltávolíthatja hello bővítmény használatával `Remove-AzureServiceDiagnosticsExtension` hello szerepkör paraméter nélkül. Ha hello szerepkör paraméter lett megadva, amikor hello bővítmény engedélyezése majd azt kell is hello bővítmény eltávolításakor.

tooremove hello diagnosztika bővítmény minden egyes szerepkör alapján:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Lásd még:
* [Azure Cloud Services alkalmazások figyelése az Application Insights segítségével](app-insights-cloudservices.md)
* [Azure Diagnostics tooApplication Insights küldése](app-insights-azure-diagnostics.md)
* [Riasztások konfigurálásának automatizálása](app-insights-powershell-alerts.md)

