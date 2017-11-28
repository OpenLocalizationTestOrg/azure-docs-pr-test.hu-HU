---
title: "Azure PowerShell tooenable diagnosztika a Windows virtuális gép aaaUse |} Microsoft Docs"
services: virtual-machines-windows
documentationcenter: 
description: "Megtudhatja, hogyan toouse PowerShell tooenable Azure Diagnostics Windows rendszerű virtuális gépen"
author: sbtron
manager: timlt
editor: 
ms.assetid: 2e6d88f2-1980-4a24-827e-a81616a0d247
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: saurabh
ms.openlocfilehash: e945f0de154b5ba600f845f0d577b48e2254573b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooenable-azure-diagnostics-in-a-virtual-machine-running-windows"></a><span data-ttu-id="84123-103">A Windows rendszerű virtuális gép PowerShell tooenable Azure Diagnostics használata</span><span class="sxs-lookup"><span data-stu-id="84123-103">Use PowerShell tooenable Azure Diagnostics in a virtual machine running Windows</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="84123-104">Az Azure Diagnostics hello funkció, amely lehetővé teszi a központilag telepített alkalmazás vonatkozó diagnosztikai adatok gyűjtésére hello Azure belül.</span><span class="sxs-lookup"><span data-stu-id="84123-104">Azure Diagnostics is hello capability within Azure that enables hello collection of diagnostic data on a deployed application.</span></span> <span data-ttu-id="84123-105">Hello diagnosztika bővítmény toocollect diagnosztikai adatokat használhatja például az alkalmazásnaplók vagy egy Azure virtuális gép (VM) Windows rendszerű teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="84123-105">You can use hello diagnostics extension toocollect diagnostic data like application logs or performance counters from an Azure virtual machine (VM) that is running Windows.</span></span> <span data-ttu-id="84123-106">Ez a cikk ismerteti, hogyan toouse Windows PowerShell tooenable hello diagnosztika bővítményt a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="84123-106">This article describes how toouse Windows PowerShell tooenable hello diagnostics extension for a VM.</span></span> <span data-ttu-id="84123-107">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) Ez a cikk szükséges hello előfeltételeinek.</span><span class="sxs-lookup"><span data-stu-id="84123-107">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello prerequisites needed for this article.</span></span>

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-resource-manager-deployment-model"></a><span data-ttu-id="84123-108">Hello diagnosztika bővítmény engedélyezése, ha hello Resource Manager üzembe helyezési modellben</span><span class="sxs-lookup"><span data-stu-id="84123-108">Enable hello diagnostics extension if you use hello Resource Manager deployment model</span></span>
<span data-ttu-id="84123-109">Hello diagnosztika bővítmény engedélyezéséhez hello Azure Resource Manager üzembe helyezési modellben keresztül a Windows virtuális gépek hozzáadásával hello bővítmény konfigurációs toohello Resource Manager-sablon létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="84123-109">You can enable hello diagnostics extension while you create a Windows VM through hello Azure Resource Manager deployment model by adding hello extension configuration toohello Resource Manager template.</span></span> <span data-ttu-id="84123-110">Lásd: [Windows virtuális gép létrehozása a figyelési és diagnosztika hello Azure Resource Manager-sablon használatával](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="84123-110">See [Create a Windows virtual machine with monitoring and diagnostics by using hello Azure Resource Manager template](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="84123-111">tooenable hello diagnosztika bővítmény egy meglévő virtuális gépen keresztül hello Resource Manager üzembe helyezési modellben létrehozott, hello használható [Set-AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) PowerShell-parancsmag alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="84123-111">tooenable hello diagnostics extension on an existing VM that was created through hello Resource Manager deployment model, you can use hello [Set-AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) PowerShell cmdlet as shown below.</span></span>

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


<span data-ttu-id="84123-112">*$diagnosticsconfig_path* hello elérési toohello tartalmazó fájl hello diagnosztika konfigurációs XML-kódban van, a hello [minta](#sample-diagnostics-configuration) alatt.</span><span class="sxs-lookup"><span data-stu-id="84123-112">*$diagnosticsconfig_path* is hello path toohello file that contains hello diagnostics configuration in XML, as described in hello [sample](#sample-diagnostics-configuration) below.</span></span>  

<span data-ttu-id="84123-113">Ha hello diagnosztika konfigurációs fájl határozza meg a **StorageAccount** elemet a tárfiók nevére, majd hello *Set-AzureRMVMDiagnosticsExtension* parancsfájl automatikusan beállítja az hello diagnosztikai bővítmény toosend diagnosztikai adatok toothat tárfiók.</span><span class="sxs-lookup"><span data-stu-id="84123-113">If hello diagnostics configuration file specifies a **StorageAccount** element with a storage account name, then hello *Set-AzureRMVMDiagnosticsExtension* script will automatically set hello diagnostics extension toosend diagnostic data toothat storage account.</span></span> <span data-ttu-id="84123-114">A toowork hello tárfiókot kell a toobe hello ugyanahhoz az előfizetéshez, mivel a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="84123-114">For this toowork, hello storage account needs toobe in hello same subscription as hello VM.</span></span>

<span data-ttu-id="84123-115">Ha nincs **StorageAccount** akkor szükséges, hogy a hello toopass hello diagnosztika a konfigurációban megadott *StorageAccountName* paraméter toohello parancsmag.</span><span class="sxs-lookup"><span data-stu-id="84123-115">If no **StorageAccount** was specified in hello diagnostics configuration, then you need toopass in hello *StorageAccountName* parameter toohello cmdlet.</span></span> <span data-ttu-id="84123-116">Ha hello *StorageAccountName* paraméter meg van adva, majd hello parancsmag mindig a hello paraméterrel megadott hello tárfiókot használja, és nem hello egy hello diagnosztika konfigurációs fájlban megadott.</span><span class="sxs-lookup"><span data-stu-id="84123-116">If hello *StorageAccountName* parameter is specified, then hello cmdlet will always use hello storage account that is specified in hello parameter and not hello one that is specified in hello diagnostics configuration file.</span></span>

<span data-ttu-id="84123-117">Ha hello diagnosztikai tárfiók van egy másik előfizetésben található a virtuális gép, hello tooexplicitly akkor adjon át hello *StorageAccountName* és *StorageAccountKey* toohello a parancsmag paramétereit.</span><span class="sxs-lookup"><span data-stu-id="84123-117">If hello diagnostics storage account is in a different subscription from hello VM, then you need tooexplicitly pass in hello *StorageAccountName* and *StorageAccountKey* parameters toohello cmdlet.</span></span> <span data-ttu-id="84123-118">Hello *StorageAccountKey* paraméter nem van szükség, ha hello diagnosztikai tárfiók hello azonos előfizetésben, azonos módon hello parancsmag is automatikusan lekérdezéséhez és beállításához hello kulcs értékét, ha hello diagnosztika bővítmény engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84123-118">hello *StorageAccountKey* parameter is not needed when hello diagnostics storage account is in hello same subscription, as hello cmdlet can automatically query and set hello key value when enabling hello diagnostics extension.</span></span> <span data-ttu-id="84123-119">Azonban ha hello diagnosztikai tárfiók egy másik előfizetésben található, akkor hello parancsmag nem feltétlenül tudja tooget hello kulcs automatikusan és kell tooexplicitly keresztül hello hello kulcs megadásához *StorageAccountKey* a paraméter.</span><span class="sxs-lookup"><span data-stu-id="84123-119">However, if hello diagnostics storage account is in a different subscription, then hello cmdlet might not be able tooget hello key automatically and you need tooexplicitly specify hello key through hello *StorageAccountKey* parameter.</span></span>  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

<span data-ttu-id="84123-120">A virtuális gép hello diagnosztika bővítmény engedélyezése után kaphat hello segítségével aktuális hello-beállítások [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="84123-120">Once hello diagnostics extension is enabled on a VM, you can get hello current settings by using hello [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) cmdlet.</span></span>

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

<span data-ttu-id="84123-121">hello parancsmag visszaadja *PublicSettings*, tartalmazó hello diagnosztikai konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="84123-121">hello cmdlet returns *PublicSettings*, which contains hello diagnostics configuration.</span></span> <span data-ttu-id="84123-122">Támogatott konfiguráció, WadCfg és xmlCfg két típusú léteznek.</span><span class="sxs-lookup"><span data-stu-id="84123-122">There are two kinds of configuration supported, WadCfg and xmlCfg.</span></span> <span data-ttu-id="84123-123">WadCfg JSON-konfigurációt, és xmlCfg Base64-kódolású formátumú XML-konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="84123-123">WadCfg is JSON configuration, and xmlCfg is XML configuration in a Base64-encoded format.</span></span> <span data-ttu-id="84123-124">tooread hello XML, toodecode kell azt.</span><span class="sxs-lookup"><span data-stu-id="84123-124">tooread hello XML, you need toodecode it.</span></span>

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

<span data-ttu-id="84123-125">Hello [Remove-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) parancsmag használt tooremove hello diagnosztika bővítményt a virtuális gép hello lehet.</span><span class="sxs-lookup"><span data-stu-id="84123-125">hello [Remove-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) cmdlet can be used tooremove hello diagnostics extension from hello VM.</span></span>  

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-classic-deployment-model"></a><span data-ttu-id="84123-126">Hello diagnosztika bővítmény engedélyezése, ha hello klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="84123-126">Enable hello diagnostics extension if you use hello classic deployment model</span></span>
<span data-ttu-id="84123-127">Használhatja a hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) parancsmag tooenable egy diagnosztikai bővítménnyel a hello klasszikus telepítési modell használatával létrehozott virtuális Gépre.</span><span class="sxs-lookup"><span data-stu-id="84123-127">You can use hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet tooenable a diagnostics extension on a VM that you create through hello classic deployment model.</span></span> <span data-ttu-id="84123-128">hello következő példa bemutatja, hogyan toocreate hello klasszikus telepítési modell hello diagnosztika kiterjesztésű használatával új virtuális gép engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="84123-128">hello following example shows how toocreate a new VM through hello classic deployment model with hello diagnostics extension enabled.</span></span>

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

<span data-ttu-id="84123-129">tooenable hello diagnosztika bővítmény egy meglévő virtuális gépen keresztül hello klasszikus üzembe helyezési modellel, első használata hello létrehozott [Get-AzureVM](/powershell/module/azure/get-azurevm) parancsmag tooget hello Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="84123-129">tooenable hello diagnostics extension on an existing VM that was created through hello classic deployment model, first use hello [Get-AzureVM](/powershell/module/azure/get-azurevm) cmdlet tooget hello VM configuration.</span></span> <span data-ttu-id="84123-130">Ezután frissítse a hello virtuális gép konfigurációs tooinclude hello diagnosztika bővítmény hello segítségével [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="84123-130">Then update hello VM configuration tooinclude hello diagnostics extension by using hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet.</span></span> <span data-ttu-id="84123-131">Végezetül alkalmazásához hello frissített konfigurációs toohello VM [frissítés-AzureVM](/powershell/module/azure/update-azurevm).</span><span class="sxs-lookup"><span data-stu-id="84123-131">Finally, apply hello updated configuration toohello VM by using [Update-AzureVM](/powershell/module/azure/update-azurevm).</span></span>

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a><span data-ttu-id="84123-132">Mintakonfiguráció diagnosztika</span><span class="sxs-lookup"><span data-stu-id="84123-132">Sample diagnostics configuration</span></span>
<span data-ttu-id="84123-133">hello a következő XML hello diagnosztikai nyilvános konfigurációja hello fent parancsfájlok nem használható.</span><span class="sxs-lookup"><span data-stu-id="84123-133">hello following XML can be used for hello diagnostics public configuration with hello above scripts.</span></span> <span data-ttu-id="84123-134">Ez a minta-konfiguráció lesz átvitele különböző teljesítmény számlálók toohello diagnosztikai tárfiók, és hello alkalmazás, a biztonság, és a rendszer csatornák hello Windows eseménynaplóiban keresse meg a hibákat, és a hibák hello diagnosztika infrastruktúra-naplókat.</span><span class="sxs-lookup"><span data-stu-id="84123-134">This sample configuration will transfer various performance counters toohello diagnostics storage account, along with errors from hello application, security, and system channels in hello Windows event logs and any errors from hello diagnostics infrastructure logs.</span></span>

<span data-ttu-id="84123-135">hello beállításokat kell toobe frissített tooinclude hello következő:</span><span class="sxs-lookup"><span data-stu-id="84123-135">hello configuration needs toobe updated tooinclude hello following:</span></span>

* <span data-ttu-id="84123-136">Hello *resourceID* hello attribútumának **metrikák** elem toobe hello erőforrás-azonosító hello VM frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="84123-136">hello *resourceID* attribute of hello **Metrics** element needs toobe updated with hello resource ID for hello VM.</span></span>
  
  * <span data-ttu-id="84123-137">hello erőforrás-azonosítója a következő mintát hello segítségével lehet létrehozni: "/Subscriptions/ {*hello előfizetés hello virtuális gép előfizetés-azonosító*} /resourceGroups/ {*hello erőforráscsoport-névre hello VM*} / providers/Microsoft.Compute/virtualMachines/ {*nevet a virtuális gép hello*} ".</span><span class="sxs-lookup"><span data-stu-id="84123-137">hello resource ID can be constructed by using hello following pattern: "/subscriptions/{*subscription ID for hello subscription with hello VM*}/resourceGroups/{*hello resourcegroup name for hello VM*}/providers/Microsoft.Compute/virtualMachines/{*hello VM Name*}".</span></span>
  * <span data-ttu-id="84123-138">Például ha hello hello VM futtató hello előfizetéshez tartozó előfizetés-azonosító van **11111111-1111-1111-1111-111111111111**, hello erőforráscsoport nevét hello erőforráscsoport **MyResourceGroup**, és hello nevet a virtuális gép **MyWindowsVM**, majd az értékét hello *resourceID* lenne:</span><span class="sxs-lookup"><span data-stu-id="84123-138">For example, if hello subscription ID for hello subscription where hello VM is running is **11111111-1111-1111-1111-111111111111**, hello resource group name for hello resource group is **MyResourceGroup**, and hello VM Name is **MyWindowsVM**, then hello value for *resourceID* would be:</span></span>
    
      ```
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * <span data-ttu-id="84123-139">Hogyan mérőszámok alapján generált hello teljesítményszámlálók és metrikákat konfigurációs van-e további információ: [Azure Diagnostics metrikák table Storage](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).</span><span class="sxs-lookup"><span data-stu-id="84123-139">For more information on how metrics are generated based on hello performance counters and metrics configuration, see [Azure Diagnostics metrics table in storage](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).</span></span>
* <span data-ttu-id="84123-140">Hello **StorageAccount** elem toobe hello diagnosztikai tárfiók hello nevű frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="84123-140">hello **StorageAccount** element needs toobe updated with hello name of hello diagnostics storage account.</span></span>
  
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for hello VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a><span data-ttu-id="84123-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84123-141">Next steps</span></span>
* <span data-ttu-id="84123-142">A hello Azure Diagnostics funkció és az egyéb technikák tootroubleshoot problémák További útmutatóért lásd: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](../../cloud-services/cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="84123-142">For additional guidance on using hello Azure Diagnostics capability and other techniques tootroubleshoot problems, see [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](../../cloud-services/cloud-services-dotnet-diagnostics.md).</span></span>
* <span data-ttu-id="84123-143">[Diagnosztikai konfiguráció séma](https://msdn.microsoft.com/library/azure/mt634524.aspx) különböző XML-konfiguráció beállítások hello diagnosztika bővítmény hello ismerteti.</span><span class="sxs-lookup"><span data-stu-id="84123-143">[Diagnostics configurations schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) explains hello various XML configurations options for hello diagnostics extension.</span></span>

