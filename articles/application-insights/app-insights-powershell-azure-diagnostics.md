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
# <a name="using-powershell-tooset-up-application-insights-for-an-azure-web-app"></a><span data-ttu-id="f5a6f-103">Használja fel az Application Insights PowerShell tooset Azure-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="f5a6f-103">Using PowerShell tooset up Application Insights for an Azure web app</span></span>
<span data-ttu-id="f5a6f-104">[A Microsoft Azure](https://azure.com) lehet [toosend Azure Diagnostics konfigurált](app-insights-azure-diagnostics.md) túl[Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f5a6f-104">[Microsoft Azure](https://azure.com) can be [configured toosend Azure Diagnostics](app-insights-azure-diagnostics.md) too[Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="f5a6f-105">hello diagnosztika tooAzure felhőalapú szolgáltatásokhoz és az Azure virtuális gépek vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="f5a6f-105">hello diagnostics relate tooAzure Cloud Services and Azure VMs.</span></span> <span data-ttu-id="f5a6f-106">Kiegészíti hello telemetriai adatot küld hello Application Insights SDK használatával hello alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="f5a6f-106">They complement hello telemetry that you send from within hello app using hello Application Insights SDK.</span></span> <span data-ttu-id="f5a6f-107">Részeként hello automatizálása új erőforrás létrehozása az Azure-ban, a PowerShell-diagnosztika konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="f5a6f-107">As part of automating hello process of creating new resources in Azure, you can configure diagnostics using PowerShell.</span></span>

## <a name="azure-template"></a><span data-ttu-id="f5a6f-108">Azure-sablon</span><span class="sxs-lookup"><span data-stu-id="f5a6f-108">Azure template</span></span>
<span data-ttu-id="f5a6f-109">Hello webalkalmazás az Azure-ban, és az Azure Resource Manager-sablonnal erőforrások létrehozása, ha az Application Insights konfigurálhatja a toohello erőforrások csomópont hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f5a6f-109">If hello web app is in Azure and you create your resources using an Azure Resource Manager template, you can configure Application Insights by adding this toohello resources node:</span></span>

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

* <span data-ttu-id="f5a6f-110">`nameOfAIAppResource`-hello Application Insights-erőforrás nevét</span><span class="sxs-lookup"><span data-stu-id="f5a6f-110">`nameOfAIAppResource` - a name for hello Application Insights resource</span></span>
* <span data-ttu-id="f5a6f-111">`myWebAppName`-hello webes alkalmazás hello azonosítója</span><span class="sxs-lookup"><span data-stu-id="f5a6f-111">`myWebAppName` - hello id of hello web app</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="f5a6f-112">A diagnosztikai bővítmény engedélyezése egy felhőszolgáltatás telepítésének részeként</span><span class="sxs-lookup"><span data-stu-id="f5a6f-112">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="f5a6f-113">Hello `New-AzureDeployment` parancsmag-paraméterrel rendelkezik `ExtensionConfiguration`, így tovább is diagnosztika konfigurációk tömbjét.</span><span class="sxs-lookup"><span data-stu-id="f5a6f-113">hello `New-AzureDeployment` cmdlet has a parameter `ExtensionConfiguration`, which takes an array of diagnostics configurations.</span></span> <span data-ttu-id="f5a6f-114">Ezek segítségével hozhatók létre hello `New-AzureServiceDiagnosticsExtensionConfig` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f5a6f-114">These can be created using hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet.</span></span> <span data-ttu-id="f5a6f-115">Példa:</span><span class="sxs-lookup"><span data-stu-id="f5a6f-115">For example:</span></span>

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

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="f5a6f-116">A diagnosztikai bővítmény engedélyezése egy meglévő felhőszolgáltatáson</span><span class="sxs-lookup"><span data-stu-id="f5a6f-116">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="f5a6f-117">Meglévő szolgáltatáson használja a következőt: `Set-AzureServiceDiagnosticsExtension`.</span><span class="sxs-lookup"><span data-stu-id="f5a6f-117">On an existing service, use `Set-AzureServiceDiagnosticsExtension`.</span></span>

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

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="f5a6f-118">Az aktuális diagnosztikai bővítmény konfigurációjának lekérése</span><span class="sxs-lookup"><span data-stu-id="f5a6f-118">Get current diagnostics extension configuration</span></span>
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a><span data-ttu-id="f5a6f-119">A diagnosztikai bővítmény eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f5a6f-119">Remove diagnostics extension</span></span>
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="f5a6f-120">Ha engedélyezte a hello diagnosztika bővítmény használatával `Set-AzureServiceDiagnosticsExtension` vagy `New-AzureServiceDiagnosticsExtensionConfig` hello szerepkör paramétert, majd eltávolíthatja hello bővítmény használatával `Remove-AzureServiceDiagnosticsExtension` hello szerepkör paraméter nélkül.</span><span class="sxs-lookup"><span data-stu-id="f5a6f-120">If you enabled hello diagnostics extension using either `Set-AzureServiceDiagnosticsExtension` or `New-AzureServiceDiagnosticsExtensionConfig` without hello Role parameter, then you can remove hello extension using `Remove-AzureServiceDiagnosticsExtension` without hello Role parameter.</span></span> <span data-ttu-id="f5a6f-121">Ha hello szerepkör paraméter lett megadva, amikor hello bővítmény engedélyezése majd azt kell is hello bővítmény eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="f5a6f-121">If hello Role parameter was used when enabling hello extension then it must also be used when removing hello extension.</span></span>

<span data-ttu-id="f5a6f-122">tooremove hello diagnosztika bővítmény minden egyes szerepkör alapján:</span><span class="sxs-lookup"><span data-stu-id="f5a6f-122">tooremove hello diagnostics extension from each individual role:</span></span>

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a><span data-ttu-id="f5a6f-123">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f5a6f-123">See also</span></span>
* [<span data-ttu-id="f5a6f-124">Azure Cloud Services alkalmazások figyelése az Application Insights segítségével</span><span class="sxs-lookup"><span data-stu-id="f5a6f-124">Monitor Azure Cloud Services apps with Application Insights</span></span>](app-insights-cloudservices.md)
* [<span data-ttu-id="f5a6f-125">Azure Diagnostics tooApplication Insights küldése</span><span class="sxs-lookup"><span data-stu-id="f5a6f-125">Send Azure Diagnostics tooApplication Insights</span></span>](app-insights-azure-diagnostics.md)
* [<span data-ttu-id="f5a6f-126">Riasztások konfigurálásának automatizálása</span><span class="sxs-lookup"><span data-stu-id="f5a6f-126">Automate configuring alerts</span></span>](app-insights-powershell-alerts.md)

