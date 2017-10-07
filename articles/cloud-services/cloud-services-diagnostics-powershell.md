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
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>A PowerShell használata Azure Cloud Services diagnosztika engedélyezése
Diagnosztikai adatok, például alkalmazásnaplók gyűjtheti, stb. egy felhőalapú szolgáltatás a teljesítményszámlálók hello Azure Diagnostics bővítmény. Ez a cikk ismerteti, hogyan tooenable hello Azure Diagnostics bővítmény egy felhőalapú szolgáltatás PowerShell használatával.  Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) Ez a cikk szükséges hello előfeltételeinek.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>A diagnosztikai bővítmény engedélyezése egy felhőszolgáltatás telepítésének részeként
Ez a megközelítés típus megfelelő toocontinuous integrációs forgatókönyvet, ahol a bővítmény telepítése részeként engedélyezhető hello diagnosztika hello felhőalapú szolgáltatás. Új felhőalapú szolgáltatás központi telepítés létrehozásakor a hello sikeres hello diagnosztika bővítmény engedélyezéséhez *ExtensionConfiguration* paraméter toohello [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) parancsmag. Hello *ExtensionConfiguration* paraméter a tömb diagnosztikai konfiguráció, amely hello segítségével hozhatók létre [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) parancsmag.

hello következő példa bemutatja, hogyan engedélyezheti a diagnosztika a webrole típusról és WorkerRole, egy másik diagnosztikai konfigurációja rendelkezik egy felhőalapú szolgáltatás.

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

Ha hello diagnosztika konfigurációs fájl határozza meg a `StorageAccount` elemet a tárfiók nevére, majd hello `New-AzureServiceDiagnosticsExtensionConfig` parancsmag automatikusan használja a tárfiók. A toowork hello tárfiókot kell a toobe hello ugyanahhoz az előfizetéshez, hello telepített felhőalapú szolgáltatás.

Azure SDK 2.6 adattovábbítás harmadik félnek hello bővítmény konfigurációs fájlok hello MSBuild által generált közzététele tároló kimeneti hello tárfióknév hello diagnosztika konfigurációs mezőben megadott karakterlánc hello szolgáltatás konfigurációs fájlját (.cscfg) alapján tartalmazza. hello az alábbi parancsfájl bemutatja, hogyan tooparse hello bővítmény konfigurációs fájlok hello kimenet közzététele és diagnosztika bővítményének beállítását a szerepkörönként hello felhőalapú szolgáltatás telepítésekor.

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

A Visual Studio Online használja a hasonló megközelítés hello diagnosztika kiterjesztésű a Felhőszolgáltatások automatikus központi telepítéséhez. Lásd: [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) teljes például.

Ha nincs `StorageAccount` akkor szükséges, hogy a hello toopass hello diagnosztika a konfigurációban megadott *StorageAccountName* paraméter toohello parancsmag. Ha hello *StorageAccountName* paraméter meg van adva, majd hello parancsmag mindig a hello paraméterrel megadott hello tárfiókot használja, és nem hello egy hello diagnosztika konfigurációs fájlban megadott.

Ha hello diagnosztikai tárfiók van egy másik előfizetésben található hello felhőalapú szolgáltatás, a tooexplicitly akkor adjon át hello *StorageAccountName* és *StorageAccountKey* paraméterek toohello parancsmag. Hello *StorageAccountKey* paraméter nem van szükség, ha hello diagnosztikai tárfiók hello azonos előfizetésben, azonos módon hello parancsmag is automatikusan lekérdezéséhez és beállításához hello kulcs értékét, ha hello diagnosztika bővítmény engedélyezése. Azonban ha hello diagnosztikai tárfiók egy másik előfizetésben található, akkor hello parancsmag nem feltétlenül tudja tooget hello kulcs automatikusan és kell tooexplicitly keresztül hello hello kulcs megadásához *StorageAccountKey* a paraméter.

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>A diagnosztikai bővítmény engedélyezése egy meglévő felhőszolgáltatáson
Használhatja a hello [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) parancsmag tooenable vagy frissítés diagnosztikai konfigurációja egy Felhőszolgáltatás, amely már fut a.

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a>Az aktuális diagnosztikai bővítmény konfigurációjának lekérése
Használjon hello [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) parancsmag tooget hello diagnosztika vonatkozó aktuális konfigurációját egy felhőalapú szolgáltatás.

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a>A diagnosztikai bővítmény eltávolítása
diagnosztika a felhőn ki tooturn szolgáltatás akkor használható hello [Remove-AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) parancsmag.

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Ha engedélyezte a hello diagnosztika bővítmény használatával *Set-AzureServiceDiagnosticsExtension* vagy hello *New-AzureServiceDiagnosticsExtensionConfig* nélkül hello *szerepkör* paraméter, akkor eltávolíthatja hello bővítmény használatával *Remove-AzureServiceDiagnosticsExtension* nélkül hello *szerepkör* paraméter. Ha hello *szerepkör* paraméter lett megadva, amikor engedélyezése hello bővítményt, akkor azt is használni kell hello bővítmény eltávolításakor.

tooremove hello diagnosztika bővítmény minden egyes szerepkör alapján:

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a>Következő lépések
* Az Azure diagnostics és az egyéb technikák tootroubleshoot problémák használatával További útmutatóért lásd: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](cloud-services-dotnet-diagnostics.md).
* Hello [diagnosztika konfigurációs séma](https://msdn.microsoft.com/library/azure/dn782207.aspx) különböző xml-konfiguráció beállítások hello diagnosztika bővítmény hello ismerteti.
* Hogyan tooenable hello diagnosztika bővítményt a virtuális gépek: toolearn [Windows virtuális gép létrehozása a figyelési és -diagnosztika Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md)
