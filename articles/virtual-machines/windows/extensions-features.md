---
title: "aaaVirtual gép bővítményeket és szolgáltatásokat a Windows Azure-ban |} Microsoft Docs"
description: "Ismerje meg, milyen bővítmények érhetők el, mi akkor adja meg, vagy javítania szerint csoportosítva, az Azure virtuális gépeken."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/06/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 61ccfd696b38e9be1026d836d5796c2346fd650f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Virtuálisgép-bővítmények és a Windows szolgáltatások

Az Azure virtuálisgép-bővítmények kis alkalmazásokat, amelyek telepítés utáni konfigurációs és automatizálási feladatok Azure virtuális gépeken. Például ha egy virtuális gépet a Szoftvertelepítés, a víruskereső védelmet, vagy a Docker konfigurációs igényel, a Virtuálisgép-bővítmény is használt toocomplete kell ezeket a feladatokat. Az Azure Virtuálisgép-bővítmények hello Azure CLI PowerShell, Azure Resource Manager sablonok segítségével futtathatja, és hello Azure-portálon. Bővítmények mellékelhető egy új virtuálisgép-telepítést, vagy bármely létező rendszert futtathat.

Ez a dokumentum áttekintést virtuálisgép-bővítmények, hogyan toodetect, kezeléséhez, és távolítsa el a virtuálisgép-bővítmények a virtuálisgép-bővítmények és útmutatás használatának előfeltételei. A dokumentum általános információkat tartalmaz a Virtuálisgép-bővítmények érhetők el, mert egyes potenciálisan egyedi konfigurációval. Minden egyes dokumentum adott toohello egyedi bővítmény részletei bővítmény-specifikus megtalálhatók.

## <a name="use-cases-and-samples"></a>Használati esetek és minták

Nincsenek elérhető számos különböző Azure Virtuálisgép-bővítmények, az adott használati eset. Néhány példa használati esetek a következők:

- Alkalmazza a PowerShell kívánt állapot konfigurációk tooa virtuális gép Windows hello DSC-bővítmény használatával. További információkért lásd: [Azure kívánt állapot konfigurációs kiterjesztése](extensions-dsc-overview.md).
- Konfigurálja a virtuális gép figyelése hello Microsoft Monitoring Agent Virtuálisgép-bővítmény használatával. További információkért lásd: [csatlakozás Azure virtuális gépek tooLog Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).
- Az Azure infrastruktúra hello Datadog bővítmény a figyelés konfigurálása. További információkért lásd: hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Konfiguráljon egy Azure virtuális gép Chef. További információkért lásd: [automatizálása Azure virtuális gépek telepítése a Chef](chef-automation.md).

Továbbá tooprocess-specifikus bővítmények, egy egyéni parancsprogramok futtatására szolgáló bővítmény érhető el a Windows és Linux virtuális gépek. Egyéni parancsprogramok futtatására szolgáló bővítmény Windows hello lehetővé teszi, hogy a PowerShell parancsfájl toobe futtasson egy virtuális gépen. Ez akkor hasznos, amikor az identitásfelügyelet Azure-környezetekhez, hogy milyen natív Azure tooling biztosíthat túl beállításokra van szükség. További információkért lásd: [Windows virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md).


## <a name="prerequisites"></a>Előfeltételek

Minden egyes virtuálisgép-bővítmény is rendelkeznek a saját előfeltételek. Például hello Docker Virtuálisgép-bővítmény van egy támogatott Linux-disztribúció előfeltétele. Egyes bővítmények követelményeinek részletes leírást talál hello bővítmény vonatkozó dokumentációt.

### <a name="azure-vm-agent"></a>Azure virtuálisgép-ügynök
hello Azure Virtuálisgép-ügynök kezelése az Azure virtuális gép és a hello Azure fabric controller közötti interakció. hello Virtuálisgép-ügynök telepítése és kezelése az Azure virtuális gépeken, beleértve a Virtuálisgép-bővítmények futtató sok működési jellemzői felelős. hello Azure Virtuálisgép-ügynök telepítve van az Azure piactéren elérhető rendszerkép és támogatott operációs rendszerekre telepíthető.

Információ a támogatott operációs rendszerek és a telepítési utasításokat: [Azure virtuális gépi ügynöke](agent-user-guide.md).

## <a name="discover-vm-extensions"></a>Virtuálisgép-bővítmények felderítése
Számos különböző Virtuálisgép-bővítmények állnak rendelkezésre az Azure virtuális gépekkel. toosee teljes listáját, futtassa a következő parancs hello Azure Resource Manager PowerShell modullal hello. Győződjön meg arról, hogy toospecify hello szükséges hely jelenik meg a parancsot.

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>Futtassa a Virtuálisgép-bővítmények

Azure virtuálisgép-bővítmények futtathatja a meglévő virtuális gépeken, amelyek akkor hasznos, ha a szükséges konfigurációs módosítások toomake, vagy állítsa helyre a kapcsolatot egy már telepített virtuális gépen. Virtuálisgép-bővítmények is Azure Resource Manager sablon központi telepítéseket is telepíthet. Bővítmények a Resource Manager-sablonok segítségével engedélyezheti az Azure virtuális gépek toobe üzembe helyezését és konfigurálását hello telepítés utáni beavatkozás nélkül.

a következő módszerek hello használt toorun egy bővítmény egy meglévő virtuális gépek ellen lehet.

### <a name="powershell"></a>PowerShell

Több PowerShell-parancsok futtatása az egyes bővítmények léteznek. toosee a listáját, és futtassa a következő PowerShell-parancsok hello.

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

Ez a kimeneti hasonló toohello következőket biztosítja:

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    2.2.0      AzureRM.Compute
```

hello alábbi példa hello Custom Script bővítmény toodownload parancsfájlt használ a egy GitHub-tárházban települ hello cél virtuális gépre, majd futtassa hello parancsfájl. Az egyéni parancsprogramok futtatására szolgáló bővítmény hello további információkért lásd: [egyéni parancsfájl-bővítmény áttekintése](extensions-customscript.md).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

Ebben a példában a hello VM hozzáférési bővítmény használt tooreset hello rendszergazdai jelszót egy Windows virtuális gép. A virtuális gép hozzáférési bővítmény hello további információkért lásd: [alaphelyzetbe állítja a távoli asztal szolgáltatás egy Windows virtuális gépre](reset-rdp.md).

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

Hello `Set-AzureRmVMExtension` parancs lehet használt toostart Virtuálisgép-bővítmény. További információkért lásd: hello [Set-AzureRmVMExtension hivatkozás](https://msdn.microsoft.com/en-us/library/mt603745.aspx).


### <a name="azure-portal"></a>Azure Portal

A Virtuálisgép-bővítmény alkalmazott tooan meglévő virtuális gép hello Azure-portálon keresztül lehet. toodo úgy, válassza ki a virtuális gép hello toouse szeretne, válassza a **bővítmények**, és kattintson a **Hozzáadás**. Ez a rendelkezésre álló bővítmények listáját tartalmazza. Válassza ki a hello egy szeretné, hajtsa végre a hello hello varázsló lépéseit.

hello következő kép bemutatja hello hello hello Azure-portál a Microsoft Antimalware-bővítmény telepítése.

![Kártevőirtó-kiterjesztés telepítése](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sablonok

Virtuálisgép-bővítmények hozzáadott tooan Azure Resource Manager-sablon és hello sablon hello telepítés végrehajtása. Központi telepítése és egy sablon létrehozása esetén hasznos teljesen konfigurált Azure-környezetekhez. Például hello következő JSON származik a Resource Manager-sablon, amely telepíti az elosztott terhelésű virtuális gépek és az Azure SQL-adatbázis, és ezután telepíti a .NET Core alkalmazások minden egyes virtuális gépen. Virtuálisgép-bővítmény hello hello Szoftvertelepítés gondoskodik.

További információkért lásd: hello [teljes Resource Manager-sablon](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

További információkért lásd: [szerzői Azure Resource Manager-sablon is van Windows Virtuálisgép-bővítmények](template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>Virtuálisgép-bővítmény adatvédelem

Ha egy Virtuálisgép-bővítmény, szükséges tooinclude bizalmas információk, például a hitelesítő adatokat, a tárfiókok neve és a fiók tárelérési kulcsok lehet. Több Virtuálisgép-bővítmények közé tartozik a védett konfigurációkat, amelyek titkosítja az adatokat, és csak visszafejti hello cél virtuális gépen belül. Minden bővítmény rendelkezik egy adott védett konfigurációs sémában, amely a bővítmény-specifikus dokumentációjának nyilatkozatát.

a következő példa hello Windows hello egyéni parancsprogramok futtatására szolgáló bővítmény példányának jelennek meg. Figyelje meg, hogy hello parancs tooexecute hitelesítő adatokat tartalmazza. Ebben a példában hello parancs tooexecute nem lesznek titkosítva.


```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Biztonságos hello végrehajtási karakterlánc hello mozgatásával **parancs tooexecute** tulajdonság toohello **védett** konfigurációs.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

## <a name="troubleshoot-vm-extensions"></a>Virtuálisgép-bővítmények hibaelhárítása

Előfordulhat, hogy az egyes Virtuálisgép-bővítmény adott hibaelhárítási lépéseket. Például ha egyéni parancsprogramok futtatására szolgáló bővítmény hello használata esetén parancsfájl végrehajtásának részletes adatai található helyileg hello bővítmény futtatása hello virtuális gépen. Bővítmény-specifikus hibaelhárítási lépéseket részletes leírást talál bővítmény vonatkozó dokumentációt.

a következő hibaelhárítási lépéseket hello tooall virtuálisgép-bővítmények alkalmazni.

### <a name="view-extension-status"></a>Bővítmény állapotának megtekintése

Egy virtuálisgép-bővítmény futtatása a virtuális gépek ellen, után használja a PowerShell parancs tooreturn bővítmény állapota a következő hello. Példa paraméterneveknek cserélje le a saját értékeit. Hello `Name` paramétert fogad hello neve toohello bővítmény a végrehajtás során.

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

hello kimeneti következőhöz hasonló hello:

```json
ResourceGroupName       : myResourceGroup
VMName                  : myVM
Name                    : myExtensionName
Location                : westus
Etag                    : null
Publisher               : Microsoft.Azure.Extensions
ExtensionType           : DockerExtension
TypeHandlerVersion      : 1.0
Id                      : /subscriptions/mySubscriptionIS/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/myExtensionName
PublicSettings          :
ProtectedSettings       :
ProvisioningState       : Succeeded
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : False
ForceUpdateTag          :
```

Bővítmény végrehajtási állapotát hello Azure-portálon is megtalálhatók. egy bővítmény, jelölje be hello virtuális gép, tooview hello állapotának kiválasztása **bővítmények**, és válassza ki a kívánt bővítmény hello.

### <a name="rerun-vm-extensions"></a>Futtassa újra a Virtuálisgép-bővítmények

Előfordulhat, hogy esetekben, ahol a virtuálisgép-bővítmény kell toobe futtassa újra. Ehhez hello bővítmény eltávolítása, és ezután futtassa újból a hello bővítmény egy végrehajtási módszer az Ön által választott. egy bővítmény tooremove futtassa a következő parancs hello Azure PowerShell modul hello. Példa paraméterneveknek cserélje le a saját értékeit.

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Egy bővítmény hello Azure-portál használatával is eltávolítható. toodo így:

1. Válasszon ki egy virtuális gépet.
2. Válassza ki **bővítmények**.
3. Válassza ki a kívánt hello bővítmény.
4. Válassza ki **eltávolítása**.

## <a name="common-vm-extensions-reference"></a>Közös Virtuálisgép-bővítmények hivatkozása
| Bővítmény neve | Leírás | További információ |
| --- | --- | --- |
| A Windows egyéni parancsprogramok futtatására szolgáló bővítmény |Parancsfájlok futtatásához egy Azure virtuális gépen |[A Windows egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| A Windows DSC-bővítményt |PowerShell DSC (célállapot-konfiguráció) bővítmény |[A Windows DSC-bővítményt](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure Diagnostics bővítmény |Az Azure Diagnostics kezelése |[Azure Diagnostics bővítmény](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Azure virtuális gép hozzáférési bővítmény |Felhasználók és a hitelesítő adatok kezelése |[Linux virtuális gép hozzáférési bővítmény](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
