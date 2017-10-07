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
# <a name="use-powershell-tooenable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>A Windows rendszerű virtuális gép PowerShell tooenable Azure Diagnostics használata
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Az Azure Diagnostics hello funkció, amely lehetővé teszi a központilag telepített alkalmazás vonatkozó diagnosztikai adatok gyűjtésére hello Azure belül. Hello diagnosztika bővítmény toocollect diagnosztikai adatokat használhatja például az alkalmazásnaplók vagy egy Azure virtuális gép (VM) Windows rendszerű teljesítményszámlálók. Ez a cikk ismerteti, hogyan toouse Windows PowerShell tooenable hello diagnosztika bővítményt a virtuális gépek. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) Ez a cikk szükséges hello előfeltételeinek.

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-resource-manager-deployment-model"></a>Hello diagnosztika bővítmény engedélyezése, ha hello Resource Manager üzembe helyezési modellben
Hello diagnosztika bővítmény engedélyezéséhez hello Azure Resource Manager üzembe helyezési modellben keresztül a Windows virtuális gépek hozzáadásával hello bővítmény konfigurációs toohello Resource Manager-sablon létrehozása során. Lásd: [Windows virtuális gép létrehozása a figyelési és diagnosztika hello Azure Resource Manager-sablon használatával](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

tooenable hello diagnosztika bővítmény egy meglévő virtuális gépen keresztül hello Resource Manager üzembe helyezési modellben létrehozott, hello használható [Set-AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) PowerShell-parancsmag alább látható módon.

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* hello elérési toohello tartalmazó fájl hello diagnosztika konfigurációs XML-kódban van, a hello [minta](#sample-diagnostics-configuration) alatt.  

Ha hello diagnosztika konfigurációs fájl határozza meg a **StorageAccount** elemet a tárfiók nevére, majd hello *Set-AzureRMVMDiagnosticsExtension* parancsfájl automatikusan beállítja az hello diagnosztikai bővítmény toosend diagnosztikai adatok toothat tárfiók. A toowork hello tárfiókot kell a toobe hello ugyanahhoz az előfizetéshez, mivel a virtuális gép hello.

Ha nincs **StorageAccount** akkor szükséges, hogy a hello toopass hello diagnosztika a konfigurációban megadott *StorageAccountName* paraméter toohello parancsmag. Ha hello *StorageAccountName* paraméter meg van adva, majd hello parancsmag mindig a hello paraméterrel megadott hello tárfiókot használja, és nem hello egy hello diagnosztika konfigurációs fájlban megadott.

Ha hello diagnosztikai tárfiók van egy másik előfizetésben található a virtuális gép, hello tooexplicitly akkor adjon át hello *StorageAccountName* és *StorageAccountKey* toohello a parancsmag paramétereit. Hello *StorageAccountKey* paraméter nem van szükség, ha hello diagnosztikai tárfiók hello azonos előfizetésben, azonos módon hello parancsmag is automatikusan lekérdezéséhez és beállításához hello kulcs értékét, ha hello diagnosztika bővítmény engedélyezése. Azonban ha hello diagnosztikai tárfiók egy másik előfizetésben található, akkor hello parancsmag nem feltétlenül tudja tooget hello kulcs automatikusan és kell tooexplicitly keresztül hello hello kulcs megadásához *StorageAccountKey* a paraméter.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

A virtuális gép hello diagnosztika bővítmény engedélyezése után kaphat hello segítségével aktuális hello-beállítások [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) parancsmag.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

hello parancsmag visszaadja *PublicSettings*, tartalmazó hello diagnosztikai konfigurációja. Támogatott konfiguráció, WadCfg és xmlCfg két típusú léteznek. WadCfg JSON-konfigurációt, és xmlCfg Base64-kódolású formátumú XML-konfigurációját. tooread hello XML, toodecode kell azt.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

Hello [Remove-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) parancsmag használt tooremove hello diagnosztika bővítményt a virtuális gép hello lehet.  

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-classic-deployment-model"></a>Hello diagnosztika bővítmény engedélyezése, ha hello klasszikus telepítési modell
Használhatja a hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) parancsmag tooenable egy diagnosztikai bővítménnyel a hello klasszikus telepítési modell használatával létrehozott virtuális Gépre. hello következő példa bemutatja, hogyan toocreate hello klasszikus telepítési modell hello diagnosztika kiterjesztésű használatával új virtuális gép engedélyezve van.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

tooenable hello diagnosztika bővítmény egy meglévő virtuális gépen keresztül hello klasszikus üzembe helyezési modellel, első használata hello létrehozott [Get-AzureVM](/powershell/module/azure/get-azurevm) parancsmag tooget hello Virtuálisgép-konfiguráció. Ezután frissítse a hello virtuális gép konfigurációs tooinclude hello diagnosztika bővítmény hello segítségével [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) parancsmag. Végezetül alkalmazásához hello frissített konfigurációs toohello VM [frissítés-AzureVM](/powershell/module/azure/update-azurevm).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Mintakonfiguráció diagnosztika
hello a következő XML hello diagnosztikai nyilvános konfigurációja hello fent parancsfájlok nem használható. Ez a minta-konfiguráció lesz átvitele különböző teljesítmény számlálók toohello diagnosztikai tárfiók, és hello alkalmazás, a biztonság, és a rendszer csatornák hello Windows eseménynaplóiban keresse meg a hibákat, és a hibák hello diagnosztika infrastruktúra-naplókat.

hello beállításokat kell toobe frissített tooinclude hello következő:

* Hello *resourceID* hello attribútumának **metrikák** elem toobe hello erőforrás-azonosító hello VM frissíteni kell.
  
  * hello erőforrás-azonosítója a következő mintát hello segítségével lehet létrehozni: "/Subscriptions/ {*hello előfizetés hello virtuális gép előfizetés-azonosító*} /resourceGroups/ {*hello erőforráscsoport-névre hello VM*} / providers/Microsoft.Compute/virtualMachines/ {*nevet a virtuális gép hello*} ".
  * Például ha hello hello VM futtató hello előfizetéshez tartozó előfizetés-azonosító van **11111111-1111-1111-1111-111111111111**, hello erőforráscsoport nevét hello erőforráscsoport **MyResourceGroup**, és hello nevet a virtuális gép **MyWindowsVM**, majd az értékét hello *resourceID* lenne:
    
      ```
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * Hogyan mérőszámok alapján generált hello teljesítményszámlálók és metrikákat konfigurációs van-e további információ: [Azure Diagnostics metrikák table Storage](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).
* Hello **StorageAccount** elem toobe hello diagnosztikai tárfiók hello nevű frissíteni kell.
  
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

## <a name="next-steps"></a>Következő lépések
* A hello Azure Diagnostics funkció és az egyéb technikák tootroubleshoot problémák További útmutatóért lásd: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](../../cloud-services/cloud-services-dotnet-diagnostics.md).
* [Diagnosztikai konfiguráció séma](https://msdn.microsoft.com/library/azure/mt634524.aspx) különböző XML-konfiguráció beállítások hello diagnosztika bővítmény hello ismerteti.

