---
title: "Virtuális gép méretezési készletek csatolt adatlemezek aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse csatolt adatok lemez is van a virtuálisgép-méretezési csoportok"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: guybo
ms.openlocfilehash: 77b66f80934c0aaf7bb1ad0de00a738052a878ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a>Azure-beli virtuálisgép-méretezési csoportok és csatlakoztatott adatlemezek
Az Azure-beli [virtuálisgép-méretezési csoportok](/azure/virtual-machine-scale-sets/) mostantól támogatják a csatlakoztatott adatlemezekkel rendelkező virtuális gépeket. Az adatlemezek hello tárolási profilban Azure felügyelt lemezekkel létrehozott méretezési csoportok definiálhatók. Korábban hello érhető el virtuális gépek méretezési csoportok csak közvetlenül csatlakoztatott tárolók beállítások lettek hello operációsrendszer-lemezek vagy ideiglenes meghajtók.

> [!NOTE]
>  Amikor létrehoz egy méretezési definiált csatolt adatlemezekkel rendelkező készletben, toomount továbbra is szükséges és formátum hello lemezek a belül egy virtuális gép toouse őket (az önálló Azure virtuális gépek csak hasonló). Egy kényelmes módszert arra toodo toouse egy egyéni parancsfájl-kiterjesztésen, amely meghívja a szabványos parancsfájlként toopartition és formázza az összes hello adatok lemezt a virtuális gép.

## <a name="create-a-scale-set-with-attached-data-disks"></a>Csatlakoztatott adatlemezekkel rendelkező méretezési csoport létrehozása
Egy egyszerű módon toocreate egy méretezési csatlakoztatott lemezzel toouse hello [Azure CLI](https://github.com/Azure/azure-cli) _vmss létrehozása_ parancsot. a következő példa hello hoz létre egy Azure-erőforráscsoportot, és a Virtuálisgép-méretezési készlet 10 Ubuntu virtuális gépek, külön 2 csatolt adatlemezek, 50 GB-os és 100 GB-osztályban.
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
Vegye figyelembe, hogy hello _vmss létrehozása_ parancs bizonyos konfigurációs értékeket alapértelmezés szerint, ha nem adja meg azokat. toosee hello elérhető lehetőségek, hogy próbálja meg lehet felülbírálni:
```bash
az vmss create --help
```
Állítsa be a mellékelt adatok lemezek terjedő skálán toodefine egy méretezési Azure Resource Manager-sablon, egy másik módja toocreate közé tartozik egy _dataDisks_ hello szakasz _storageProfile_, és hello telepítése sablon. hello 50 GB-os és 100 GB szabad fenti példa hello sablonban ehhez hasonló lenne meghatározott:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```
Egy csatlakoztatott lemezzel, az itt megadott egy teljes, készen áll a toodeploy például egy méretezési készlet sablon láthatja: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).

## <a name="adding-a-data-disk-tooan-existing-scale-set"></a>Adatok lemez tooan meglévő méretezési hozzáadása beállítása
> [!NOTE]
>  Adatok lemezek tooa méretezési jött létre, amely csak csatolhat [Azure felügyelt lemezek](./virtual-machine-scale-sets-managed-disks.md).

Hozzáadhat egy adatok lemez tooa Virtuálisgép-méretezési beállítása az Azure parancssori felület használatával _az vmss lemez csatolása_ parancsot. Győződjön meg arról, hogy olyan logikaiegység-számot ad meg, amely még nincs használatban. a következő példa CLI hello ad hozzá egy 50 GB-os meghajtó toolun 3:
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

a következő PowerShell-példa hello ad hozzá egy 50 GB-os meghajtó toolun 3:
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> Másik Virtuálisgép-méretek hello számok csatlakoztatott meghajtók támogatják a különböző korlátokkal rendelkeznek. Ellenőrizze a hello [virtuális gép mérete alapján](../virtual-machines/windows/sizes.md) új lemez hozzáadása előtt.

Egy lemezt hozzáad egy új belépési toohello hozzáadásával _dataDisks_ hello tulajdonság _storageProfile_ a skála állítsa be a definíció- és hello módosítás alkalmazása. tootest, a keresés hello egy meglévő méretezési készlet definíciója [Azure erőforrás-kezelő](https://resources.azure.com/). Válassza ki _szerkesztése_ , és adja hozzá egy új lemezt toohello listájának beolvasása. Például a fenti hello-példáját:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

Válassza ki _PUT_ tooapply hello tooyour méretezési változik. Ez a példa akkor működik, ha a használt virtuális gép mérete kettőnél több csatlakoztatott adatlemezt támogat.

> [!NOTE]
> Amikor a módosítás tooa méretezési definition például hozzáadásával vagy eltávolításával adatlemezt, tooall az újonnan létrehozott virtuális gépek vonatkozik, de tooexisting virtuális gépek csak akkor, ha hello _upgradePolicy_ tulajdonsága túl "automatikus". Ha túl "Manual" értéke, meg kell-e toomanually hello új modell tooexisting virtuális gépek alkalmazni. Ehhez a hello portál hello segítségével _frissítés-AzureRmVmssInstance_ PowerShell parancsot, vagy a hello segítségével _az vmss frissítés-példányokat_ CLI parancsot.

## <a name="adding-pre-populated-data-disks-tooan-existent-scale-set"></a>Előre megadott adatok lemezek tooan létező méretezési hozzáadása beállítása 
> Lemezek tooan létező hozzáadásakor méretezési modell úgy lett kialakítva, hello lemez mindig készül üres. Ebben a forgatókönyvben is hello méretezési készlet által létrehozott új példányok. Ez a viselkedés nem, mert hello scaleset van egy üres adatlemez. Rendelés toocreate előre megadott adatmeghajtókon egy létező méretezési készlet modell, a következő két lehetőség közül választhat:

* Adatok másolása az hello példányt 0 VM toohello adatok lemez(ek) hello a többi virtuális gép egy egyéni parancsfájl futtatásával.
* Hozzon létre egy felügyelt képre hello operációsrendszer-lemez és adatlemez (hello szükséges adatokkal), és hozzon létre egy új scaleset hello kép. Így minden létrehozott új virtuális Gépet fog rendelkezni az adatok lemezre hello scaleset hello definíciójában megadott. Mivel ez a definíció hivatkozhatnak, amely rendelkezik a testreszabott adatlemezt tooan rendszerképének hello scaleset minden virtuális gép lesz automatikusan kapja meg ezeket a módosításokat.

> hello módon toocreate egyéni lemezkép itt található: [egy felügyelt képre egy általánosított virtuális gép létrehozása az Azure-ban](/azure/virtual-machines/windows/capture-image-resource/) 

> hello felhasználó számára szükséges toocapture hello 0 virtuális gép, amely rendelkezik a szükséges adatok hello példányt, és ezután használja az adott vhd hello kép definíciója.

## <a name="removing-a-data-disk-from-a-scale-set"></a>Adatlemez eltávolítása méretezési csoportból
Adatlemez az Azure parancssori felület _az vmss disk detach_ parancsával távolítható el a virtuálisgép-méretezési csoportokból. Például hello parancsban hello lemez eltávolítása a lun 2 van megadva:
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
Hasonló módon is eltávolíthat egy lemezt egy bejegyzés eltávolítása hello által beállított méretezési _dataDisks_ hello tulajdonság _storageProfile_ és hello módosítás alkalmazása. 

## <a name="additional-notes"></a>További megjegyzések
Az Azure által kezelt lemezek és a skála állítsa a mellékelt adatok lemezek is elérhetők az API verzió [ _2016-04-30-előzetes_ ](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) vagy újabb hello Microsoft.Compute API.

Hello kezdeti végrehajtásának méretezési csoportok csatlakoztatott lemezre támogatása nem csatolása vagy leválasztása adatlemezek méretezési csoportban lévő egyes virtuális gépek és a.

Az Azure Portal kezdetben csak korlátozott támogatást biztosít a méretezési csoportok csatlakoztatott adatlemezei számára. Azure-sablonok is használhatja a követelményeitől függően a parancssori felület, a PowerShell, az SDK-k és a REST API toomanage csatlakoztatott lemezekkel.


