---
title: "Virtuálisgép-ügynök – áttekintés aaaAzure |} Microsoft Docs"
description: "Az Azure virtuális gép ügynök – áttekintés"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: nepeters
ms.openlocfilehash: b03542b9a9c711000fab18ed82e9b17ee5510bbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-agent-overview"></a>Az Azure virtuális gép ügynökének áttekintése

hello Microsoft Azure virtuális gép ügynökének (de ügynök) egy biztonságos, egyszerű folyamat hello Azure Fabric Controller interakcióba VM kezeli. hello Virtuálisgép-ügynök engedélyezése és a végrehajtása az Azure virtuálisgép-bővítmények az elsődleges szerepkör tartozik. Virtuálisgép-bővítmények, amely lehetővé teszi a virtuális gépek, például a telepítése és beállítása a szoftver központi telepítési konfigurációs utáni. Virtuálisgép-bővítmények is engedélyezheti a helyreállítási szolgáltatás például a virtuális gépek hello a rendszergazdai jelszó alaphelyzetbe állításával. Nélkül hello Azure Virtuálisgép-ügynök a virtuálisgép-bővítmények nem lehet futtatni.

Ez a dokumentum a telepítés, észlelési és hello Azure virtuális gép ügynök eltávolításának részletes.

## <a name="install-hello-vm-agent"></a>Hello Virtuálisgép-ügynök telepítése

### <a name="azure-gallery-image"></a>Azure katalógusában kép

hello Azure Virtuálisgép-ügynök telepítve van a Windows virtuális gépek Azure-katalógus lemezképből telepített alapértelmezés szerint. Hello portál, a PowerShell, a parancssori felületen vagy a Azure Resource Manager-sablonok az Azure katalógusában lemezképének telepítésekor hello Azure Virtuálisgép-ügynök is van telepítve. 

### <a name="manual-installation"></a>Manuális telepítés

hello Windows Virtuálisgép-ügynök manuálisan telepíthető a Windows installer-csomag. Manuális telepítés akkor lehet szükség, ha egy egyéni virtuálisgép-lemezkép létrehozása, amely telepíti az Azure-ban. toomanually telepítés hello Virtuálisgép-ügynök a Windows hello Virtuálisgép-ügynök telepítő letölteni erről a helyről [Windows Azure VM ügynök letöltése](http://go.microsoft.com/fwlink/?LinkID=394789). 

Virtuálisgép-ügynök hello hello windows installer fájlra duplán kattintva is telepíthető. Az automatikus vagy felügyelet nélküli telepítése hello Virtuálisgép-ügynök futtassa le a következő parancs hello.

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-hello-vm-agent"></a>Virtuálisgép-ügynök hello észlelése

### <a name="powershell"></a>PowerShell

hello Azure Resource Manager PowerShell modul használt tooretrieve információk kapcsolatos Azure virtuális gépek is lehet. Futó `Get-AzureRmVM` meglehetősen bit, többek között az üzembe helyezési állapota hello Azure Virtuálisgép-ügynök a hello adja vissza.

```PowerShell
Get-AzureRmVM
```

hello az alábbiakban látható hello egy részhalmazát `Get-AzureRmVM` kimeneti. Értesítés hello `ProvisionVMAgent` tulajdonság ágyazott `OSProfile`, ez a tulajdonság lehet használt toodetermine, ha hello Virtuálisgép-ügynök üzembe helyezett toohello virtuális gép.

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

a következő parancsfájl hello lehet használt tooreturn virtuális gépek neveit és a Virtuálisgép-ügynök hello hello állapotának tömör listáját.

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Manuális észlelés

Amikor bejelentkezik a Windows Azure VM tooa, a Feladatkezelő futó folyamatok használt tooexamine is lehet. toocheck a hello Azure Virtuálisgép-ügynök, nyissa meg a Feladatkezelőt > hello részletek fülre, és keresse meg a folyamat nevét `WindowsAzureGuestAgent.exe`. hello folyamat jelzi, hogy hello Virtuálisgép-ügynök telepítve van.

## <a name="upgrade-hello-vm-agent"></a>Hello Virtuálisgép-ügynök frissítése

hello Azure virtuális gép Windows ügynök automatikusan frissítve. Új virtuális gépekre telepített tooAzure vannak, akkor kapnak hello legújabb Virtuálisgép-ügynök. Egyéni Virtuálisgép-rendszerképek kell manuálisan frissített tooinclude hello új Virtuálisgép-ügynök.
