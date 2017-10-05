---
title: "A PowerShell használata Azure Cloud Services diagnosztika engedélyezése |} Microsoft Docs"
description: "Útmutató: a PowerShell használatával felhőszolgáltatások diagnosztika engedélyezése"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 66e08754-8639-4022-ae18-4237749ba17d
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/06/2016
ms.author: adegeo
ms.openlocfilehash: 8dd9724981860c9cd4ccc443cc2bfdc465811e7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="cfad3-103">A PowerShell használata Azure Cloud Services diagnosztika engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cfad3-103">Enable diagnostics in Azure Cloud Services using PowerShell</span></span>
<span data-ttu-id="cfad3-104">Diagnosztikai adatok alkalmazásnaplók, például begyűjtheti a teljesítményszámlálók stb az egy Felhőszolgáltatás, az Azure Diagnostics kiterjesztés használatával.</span><span class="sxs-lookup"><span data-stu-id="cfad3-104">You can collect diagnostic data like application logs, performance counters etc. from a Cloud Service using the Azure Diagnostics extension.</span></span> <span data-ttu-id="cfad3-105">Ez a cikk ismerteti az Azure Diagnostics-bővítmény engedélyezése egy felhőalapú szolgáltatás PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="cfad3-105">This article describes how to enable the Azure Diagnostics extension for a Cloud Service using PowerShell.</span></span>  <span data-ttu-id="cfad3-106">Lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) esetében ez a cikk szükséges előfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="cfad3-106">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for the prerequisites needed for this article.</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="cfad3-107">A diagnosztikai bővítmény engedélyezése egy felhőszolgáltatás telepítésének részeként</span><span class="sxs-lookup"><span data-stu-id="cfad3-107">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="cfad3-108">Ezt a módszert olyan forgatókönyvekben, ahol a diagnosztika bővítmény telepítése a felhőalapú szolgáltatás részeként engedélyezhető folyamatos integrációt típusú alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="cfad3-108">This approach is applicable to continuous integration type of scenarios, where the diagnostics extension can be enabled as part of deploying the cloud service.</span></span> <span data-ttu-id="cfad3-109">Új felhőalapú szolgáltatás központi telepítés létrehozásakor a sikeres a diagnosztika bővítmény engedélyezéséhez a *ExtensionConfiguration* paramétert a [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="cfad3-109">When creating a new Cloud Service deployment you can enable the diagnostics extension by passing in the *ExtensionConfiguration* parameter to the [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="cfad3-110">A *ExtensionConfiguration* paraméter a tömb diagnosztika konfigurációk használatával hozható létre a [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="cfad3-110">The *ExtensionConfiguration* parameter takes an array of diagnostics configurations that can be created using the [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.</span></span>

<span data-ttu-id="cfad3-111">A következő példa bemutatja, hogyan engedélyezheti a diagnosztika a webrole típusról és WorkerRole, egy másik diagnosztikai konfigurációja rendelkezik egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cfad3-111">The following example shows how you can enable diagnostics for a cloud service with a WebRole and WorkerRole, each having a different diagnostics configuration.</span></span>

```powershell
$service_name = "MyService"
$service_package = "CloudService.cspkg"
$service_config = "ServiceConfiguration.Cloud.cscfg"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)
```

<span data-ttu-id="cfad3-112">Ha a diagnosztika konfigurációs fájl határozza meg a `StorageAccount` elemet a tárfiók neve, majd a `New-AzureServiceDiagnosticsExtensionConfig` parancsmag automatikusan használja a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="cfad3-112">If the diagnostics configuration file specifies a `StorageAccount` element with a storage account name, then the `New-AzureServiceDiagnosticsExtensionConfig` cmdlet will automatically use that storage account.</span></span> <span data-ttu-id="cfad3-113">Ennek működéséhez a tárfiók eltérőnek kell lennie a felhőalapú szolgáltatásként telepített ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="cfad3-113">For this to work, the storage account needs to be in the same subscription as the Cloud Service being deployed.</span></span>

<span data-ttu-id="cfad3-114">Azure SDK 2.6 meghajtóbetűjeltől közzé az MSBuild generált bővítmény konfigurációs fájlokban kimenet tartalmazza a tárfiók nevét a szolgáltatás konfigurációs (.cscfg) fájljában megadott diagnosztika konfigurációs karakterlánc alapján.</span><span class="sxs-lookup"><span data-stu-id="cfad3-114">From Azure SDK 2.6 onward the extension configuration files generated by the MSBuild publish target output will include the storage account name based on the diagnostics configuration string specified in the service configuration file (.cscfg).</span></span> <span data-ttu-id="cfad3-115">Az alábbi parancsfájl bemutatja, hogyan a közzététel kimenet bővítmény konfigurációs fájl elemzése és diagnosztika bővítményének beállítását a minden egyes szerepkör a felhőalapú szolgáltatás telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="cfad3-115">The script below shows you how to parse the Extension configuration files from the publish target output and configure diagnostics extension for each role when deploying the cloud service.</span></span>

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find the Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
    {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
    }
}
New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations
```

<span data-ttu-id="cfad3-116">A Visual Studio Online használja a hasonló megközelítés diagnosztika kiterjesztésű a Felhőszolgáltatások automatikus központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cfad3-116">Visual Studio Online uses a similar approach for automated deployments of Cloud Services with the diagnostics extension.</span></span> <span data-ttu-id="cfad3-117">Lásd: [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) teljes például.</span><span class="sxs-lookup"><span data-stu-id="cfad3-117">See [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) for a complete example.</span></span>

<span data-ttu-id="cfad3-118">Ha nincs `StorageAccount` adott a diagnosztika konfigurációban, akkor kell átadni a *StorageAccountName* paramétert a parancsmaghoz.</span><span class="sxs-lookup"><span data-stu-id="cfad3-118">If no `StorageAccount` was specified in the diagnostics configuration, then you need to pass in the *StorageAccountName* parameter to the cmdlet.</span></span> <span data-ttu-id="cfad3-119">Ha a *StorageAccountName* paraméter meg van adva, akkor a parancsmag mindig használja a tárfiók a paraméter, a nem a másik a diagnosztika konfigurációs fájlban megadott.</span><span class="sxs-lookup"><span data-stu-id="cfad3-119">If the *StorageAccountName* parameter is specified, then the cmdlet will always use the storage account that is specified in the parameter and not the one that is specified in the diagnostics configuration file.</span></span>

<span data-ttu-id="cfad3-120">Ha a diagnosztikai tárfiók egy másik előfizetésben található felhőalapú szolgáltatásából, akkor meg kell átadni explicit módon a *StorageAccountName* és *StorageAccountKey* a parancsmag paramétereit.</span><span class="sxs-lookup"><span data-stu-id="cfad3-120">If the diagnostics storage account is in a different subscription from the Cloud Service, then you need to explicitly pass in the *StorageAccountName* and *StorageAccountKey* parameters to the cmdlet.</span></span> <span data-ttu-id="cfad3-121">A *StorageAccountKey* paraméter nincs szükség esetén a diagnosztikai tárfiók ugyanahhoz az előfizetéshez, a parancsmag automatikusan lekérdezési és állítható be a kulcs értékét, ha a diagnosztika bővítmény engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cfad3-121">The *StorageAccountKey* parameter is not needed when the diagnostics storage account is in the same subscription, as the cmdlet can automatically query and set the key value when enabling the diagnostics extension.</span></span> <span data-ttu-id="cfad3-122">Azonban ha a diagnosztikai tárfiók egy másik előfizetésben található, akkor a parancsmag nem feltétlenül tudja automatikusan a kulcs eléréséhez, és kell explicit módon adja meg a kulcsot keresztül a *StorageAccountKey* paraméter.</span><span class="sxs-lookup"><span data-stu-id="cfad3-122">However, if the diagnostics storage account is in a different subscription, then the cmdlet might not be able to get the key automatically and you need to explicitly specify the key through the *StorageAccountKey* parameter.</span></span>

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="cfad3-123">A diagnosztikai bővítmény engedélyezése egy meglévő felhőszolgáltatáson</span><span class="sxs-lookup"><span data-stu-id="cfad3-123">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="cfad3-124">Használhatja a [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) parancsmag engedélyezéséhez, vagy egy Felhőszolgáltatás, amely már fut a diagnosztikai konfiguráció frissítése.</span><span class="sxs-lookup"><span data-stu-id="cfad3-124">You can use the [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet to enable or update diagnostics configuration on a Cloud Service that is already running.</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="cfad3-125">Az aktuális diagnosztikai bővítmény konfigurációjának lekérése</span><span class="sxs-lookup"><span data-stu-id="cfad3-125">Get current diagnostics extension configuration</span></span>
<span data-ttu-id="cfad3-126">Használja a [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) parancsmagot, hogy megkapja az aktuális diagnosztikai konfiguráció egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cfad3-126">Use the [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet to get the current diagnostics configuration for a cloud service.</span></span>

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a><span data-ttu-id="cfad3-127">A diagnosztikai bővítmény eltávolítása</span><span class="sxs-lookup"><span data-stu-id="cfad3-127">Remove diagnostics extension</span></span>
<span data-ttu-id="cfad3-128">Egy felhőszolgáltatás, használhatja a diagnosztika kikapcsolása a [Remove-AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="cfad3-128">To turn off diagnostics on a cloud service you can use the [Remove-AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="cfad3-129">Ha engedélyezte a diagnosztikai bővítmény használatával *Set-AzureServiceDiagnosticsExtension* vagy a *New-AzureServiceDiagnosticsExtensionConfig* nélkül a *szerepkör* paraméter, akkor eltávolíthatja a bővítmény használatával *Remove-AzureServiceDiagnosticsExtension* nélkül a *szerepkör* paraméter.</span><span class="sxs-lookup"><span data-stu-id="cfad3-129">If you enabled the diagnostics extension using either *Set-AzureServiceDiagnosticsExtension* or the *New-AzureServiceDiagnosticsExtensionConfig* without the *Role* parameter then you can remove the extension using *Remove-AzureServiceDiagnosticsExtension* without the *Role* parameter.</span></span> <span data-ttu-id="cfad3-130">Ha a *szerepkör* paraméter lett megadva, ha engedélyezi a bővítményt, akkor azt is használható a bővítmény eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="cfad3-130">If the *Role* parameter was used when enabling the extension then it must also be used when removing the extension.</span></span>

<span data-ttu-id="cfad3-131">A diagnosztika bővítmény egyes szerepkörökből való eltávolítása:</span><span class="sxs-lookup"><span data-stu-id="cfad3-131">To remove the diagnostics extension from each individual role:</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a><span data-ttu-id="cfad3-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cfad3-132">Next Steps</span></span>
* <span data-ttu-id="cfad3-133">Hibaelhárítás az Azure diagnostics és az egyéb technikák segítségével további útmutatást lásd: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="cfad3-133">For additional guidance on using Azure diagnostics and other techniques to troubleshoot problems, see [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](cloud-services-dotnet-diagnostics.md).</span></span>
* <span data-ttu-id="cfad3-134">A [diagnosztika konfigurációs séma](https://msdn.microsoft.com/library/azure/dn782207.aspx) a diagnosztika bővítmény különböző xml konfigurációk lehetőségeit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="cfad3-134">The [Diagnostics Configuration Schema](https://msdn.microsoft.com/library/azure/dn782207.aspx) explains the various xml configurations options for the diagnostics extension.</span></span>
* <span data-ttu-id="cfad3-135">Engedélyezi a diagnosztika bővítményt a virtuális gépek, lásd: [Windows virtuális gép létrehozása a figyelési és -diagnosztika Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md)</span><span class="sxs-lookup"><span data-stu-id="cfad3-135">To learn how to enable the diagnostics extension for Virtual Machines, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md)</span></span>
