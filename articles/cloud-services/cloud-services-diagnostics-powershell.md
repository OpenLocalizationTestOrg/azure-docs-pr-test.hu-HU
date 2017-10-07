---
title: "a PowerShell használata Azure Felhőszolgáltatások aaaEnable diagnosztika |} Microsoft Docs"
description: "Ismerje meg, hogy miként tooenable diagnosztika felhőalapú szolgáltatásokat, PowerShell használatával"
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
ms.openlocfilehash: 7c7444df13edc8d7f5663e20ec7558d36aac45d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="d61c6-103">A PowerShell használata Azure Cloud Services diagnosztika engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d61c6-103">Enable diagnostics in Azure Cloud Services using PowerShell</span></span>
<span data-ttu-id="d61c6-104">Diagnosztikai adatok, például alkalmazásnaplók gyűjtheti, stb. egy felhőalapú szolgáltatás a teljesítményszámlálók hello Azure Diagnostics bővítmény.</span><span class="sxs-lookup"><span data-stu-id="d61c6-104">You can collect diagnostic data like application logs, performance counters etc. from a Cloud Service using hello Azure Diagnostics extension.</span></span> <span data-ttu-id="d61c6-105">Ez a cikk ismerteti, hogyan tooenable hello Azure Diagnostics bővítmény egy felhőalapú szolgáltatás PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="d61c6-105">This article describes how tooenable hello Azure Diagnostics extension for a Cloud Service using PowerShell.</span></span>  <span data-ttu-id="d61c6-106">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) Ez a cikk szükséges hello előfeltételeinek.</span><span class="sxs-lookup"><span data-stu-id="d61c6-106">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello prerequisites needed for this article.</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="d61c6-107">A diagnosztikai bővítmény engedélyezése egy felhőszolgáltatás telepítésének részeként</span><span class="sxs-lookup"><span data-stu-id="d61c6-107">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="d61c6-108">Ez a megközelítés típus megfelelő toocontinuous integrációs forgatókönyvet, ahol a bővítmény telepítése részeként engedélyezhető hello diagnosztika hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d61c6-108">This approach is applicable toocontinuous integration type of scenarios, where hello diagnostics extension can be enabled as part of deploying hello cloud service.</span></span> <span data-ttu-id="d61c6-109">Új felhőalapú szolgáltatás központi telepítés létrehozásakor a hello sikeres hello diagnosztika bővítmény engedélyezéséhez *ExtensionConfiguration* paraméter toohello [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d61c6-109">When creating a new Cloud Service deployment you can enable hello diagnostics extension by passing in hello *ExtensionConfiguration* parameter toohello [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="d61c6-110">Hello *ExtensionConfiguration* paraméter a tömb diagnosztikai konfiguráció, amely hello segítségével hozhatók létre [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d61c6-110">hello *ExtensionConfiguration* parameter takes an array of diagnostics configurations that can be created using hello [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.</span></span>

<span data-ttu-id="d61c6-111">hello következő példa bemutatja, hogyan engedélyezheti a diagnosztika a webrole típusról és WorkerRole, egy másik diagnosztikai konfigurációja rendelkezik egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d61c6-111">hello following example shows how you can enable diagnostics for a cloud service with a WebRole and WorkerRole, each having a different diagnostics configuration.</span></span>

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

<span data-ttu-id="d61c6-112">Ha hello diagnosztika konfigurációs fájl határozza meg a `StorageAccount` elemet a tárfiók nevére, majd hello `New-AzureServiceDiagnosticsExtensionConfig` parancsmag automatikusan használja a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="d61c6-112">If hello diagnostics configuration file specifies a `StorageAccount` element with a storage account name, then hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet will automatically use that storage account.</span></span> <span data-ttu-id="d61c6-113">A toowork hello tárfiókot kell a toobe hello ugyanahhoz az előfizetéshez, hello telepített felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d61c6-113">For this toowork, hello storage account needs toobe in hello same subscription as hello Cloud Service being deployed.</span></span>

<span data-ttu-id="d61c6-114">Azure SDK 2.6 adattovábbítás harmadik félnek hello bővítmény konfigurációs fájlok hello MSBuild által generált közzététele tároló kimeneti hello tárfióknév hello diagnosztika konfigurációs mezőben megadott karakterlánc hello szolgáltatás konfigurációs fájlját (.cscfg) alapján tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d61c6-114">From Azure SDK 2.6 onward hello extension configuration files generated by hello MSBuild publish target output will include hello storage account name based on hello diagnostics configuration string specified in hello service configuration file (.cscfg).</span></span> <span data-ttu-id="d61c6-115">hello az alábbi parancsfájl bemutatja, hogyan tooparse hello bővítmény konfigurációs fájlok hello kimenet közzététele és diagnosztika bővítményének beállítását a szerepkörönként hello felhőalapú szolgáltatás telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="d61c6-115">hello script below shows you how tooparse hello Extension configuration files from hello publish target output and configure diagnostics extension for each role when deploying hello cloud service.</span></span>

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find hello Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find hello RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
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

<span data-ttu-id="d61c6-116">A Visual Studio Online használja a hasonló megközelítés hello diagnosztika kiterjesztésű a Felhőszolgáltatások automatikus központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d61c6-116">Visual Studio Online uses a similar approach for automated deployments of Cloud Services with hello diagnostics extension.</span></span> <span data-ttu-id="d61c6-117">Lásd: [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) teljes például.</span><span class="sxs-lookup"><span data-stu-id="d61c6-117">See [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) for a complete example.</span></span>

<span data-ttu-id="d61c6-118">Ha nincs `StorageAccount` akkor szükséges, hogy a hello toopass hello diagnosztika a konfigurációban megadott *StorageAccountName* paraméter toohello parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d61c6-118">If no `StorageAccount` was specified in hello diagnostics configuration, then you need toopass in hello *StorageAccountName* parameter toohello cmdlet.</span></span> <span data-ttu-id="d61c6-119">Ha hello *StorageAccountName* paraméter meg van adva, majd hello parancsmag mindig a hello paraméterrel megadott hello tárfiókot használja, és nem hello egy hello diagnosztika konfigurációs fájlban megadott.</span><span class="sxs-lookup"><span data-stu-id="d61c6-119">If hello *StorageAccountName* parameter is specified, then hello cmdlet will always use hello storage account that is specified in hello parameter and not hello one that is specified in hello diagnostics configuration file.</span></span>

<span data-ttu-id="d61c6-120">Ha hello diagnosztikai tárfiók van egy másik előfizetésben található hello felhőalapú szolgáltatás, a tooexplicitly akkor adjon át hello *StorageAccountName* és *StorageAccountKey* paraméterek toohello parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d61c6-120">If hello diagnostics storage account is in a different subscription from hello Cloud Service, then you need tooexplicitly pass in hello *StorageAccountName* and *StorageAccountKey* parameters toohello cmdlet.</span></span> <span data-ttu-id="d61c6-121">Hello *StorageAccountKey* paraméter nem van szükség, ha hello diagnosztikai tárfiók hello azonos előfizetésben, azonos módon hello parancsmag is automatikusan lekérdezéséhez és beállításához hello kulcs értékét, ha hello diagnosztika bővítmény engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d61c6-121">hello *StorageAccountKey* parameter is not needed when hello diagnostics storage account is in hello same subscription, as hello cmdlet can automatically query and set hello key value when enabling hello diagnostics extension.</span></span> <span data-ttu-id="d61c6-122">Azonban ha hello diagnosztikai tárfiók egy másik előfizetésben található, akkor hello parancsmag nem feltétlenül tudja tooget hello kulcs automatikusan és kell tooexplicitly keresztül hello hello kulcs megadásához *StorageAccountKey* a paraméter.</span><span class="sxs-lookup"><span data-stu-id="d61c6-122">However, if hello diagnostics storage account is in a different subscription, then hello cmdlet might not be able tooget hello key automatically and you need tooexplicitly specify hello key through hello *StorageAccountKey* parameter.</span></span>

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="d61c6-123">A diagnosztikai bővítmény engedélyezése egy meglévő felhőszolgáltatáson</span><span class="sxs-lookup"><span data-stu-id="d61c6-123">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="d61c6-124">Használhatja a hello [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) parancsmag tooenable vagy frissítés diagnosztikai konfigurációja egy Felhőszolgáltatás, amely már fut a.</span><span class="sxs-lookup"><span data-stu-id="d61c6-124">You can use hello [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooenable or update diagnostics configuration on a Cloud Service that is already running.</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="d61c6-125">Az aktuális diagnosztikai bővítmény konfigurációjának lekérése</span><span class="sxs-lookup"><span data-stu-id="d61c6-125">Get current diagnostics extension configuration</span></span>
<span data-ttu-id="d61c6-126">Használjon hello [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) parancsmag tooget hello diagnosztika vonatkozó aktuális konfigurációját egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d61c6-126">Use hello [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooget hello current diagnostics configuration for a cloud service.</span></span>

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a><span data-ttu-id="d61c6-127">A diagnosztikai bővítmény eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d61c6-127">Remove diagnostics extension</span></span>
<span data-ttu-id="d61c6-128">diagnosztika a felhőn ki tooturn szolgáltatás akkor használható hello [Remove-AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d61c6-128">tooturn off diagnostics on a cloud service you can use hello [Remove-AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="d61c6-129">Ha engedélyezte a hello diagnosztika bővítmény használatával *Set-AzureServiceDiagnosticsExtension* vagy hello *New-AzureServiceDiagnosticsExtensionConfig* nélkül hello *szerepkör* paraméter, akkor eltávolíthatja hello bővítmény használatával *Remove-AzureServiceDiagnosticsExtension* nélkül hello *szerepkör* paraméter.</span><span class="sxs-lookup"><span data-stu-id="d61c6-129">If you enabled hello diagnostics extension using either *Set-AzureServiceDiagnosticsExtension* or hello *New-AzureServiceDiagnosticsExtensionConfig* without hello *Role* parameter then you can remove hello extension using *Remove-AzureServiceDiagnosticsExtension* without hello *Role* parameter.</span></span> <span data-ttu-id="d61c6-130">Ha hello *szerepkör* paraméter lett megadva, amikor engedélyezése hello bővítményt, akkor azt is használni kell hello bővítmény eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="d61c6-130">If hello *Role* parameter was used when enabling hello extension then it must also be used when removing hello extension.</span></span>

<span data-ttu-id="d61c6-131">tooremove hello diagnosztika bővítmény minden egyes szerepkör alapján:</span><span class="sxs-lookup"><span data-stu-id="d61c6-131">tooremove hello diagnostics extension from each individual role:</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a><span data-ttu-id="d61c6-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d61c6-132">Next Steps</span></span>
* <span data-ttu-id="d61c6-133">Az Azure diagnostics és az egyéb technikák tootroubleshoot problémák használatával További útmutatóért lásd: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="d61c6-133">For additional guidance on using Azure diagnostics and other techniques tootroubleshoot problems, see [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](cloud-services-dotnet-diagnostics.md).</span></span>
* <span data-ttu-id="d61c6-134">Hello [diagnosztika konfigurációs séma](https://msdn.microsoft.com/library/azure/dn782207.aspx) különböző xml-konfiguráció beállítások hello diagnosztika bővítmény hello ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d61c6-134">hello [Diagnostics Configuration Schema](https://msdn.microsoft.com/library/azure/dn782207.aspx) explains hello various xml configurations options for hello diagnostics extension.</span></span>
* <span data-ttu-id="d61c6-135">Hogyan tooenable hello diagnosztika bővítményt a virtuális gépek: toolearn [Windows virtuális gép létrehozása a figyelési és -diagnosztika Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md)</span><span class="sxs-lookup"><span data-stu-id="d61c6-135">toolearn how tooenable hello diagnostics extension for Virtual Machines, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md)</span></span>
