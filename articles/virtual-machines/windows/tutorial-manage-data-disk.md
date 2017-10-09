---
title: az Azure PowerShell hello lemezek aaaManage Azure |} Microsoft Docs
description: "Útmutató – az Azure PowerShell hello Azure lemezek kezelése"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2f61ad18bc94bab527d7ae593da603c6073adc89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-powershell"></a>Azure-lemezeket a PowerShell-lel kezelése

Az Azure virtuális gépek használata a lemezek toostore hello virtuális gépek operációs rendszer, alkalmazások és adatok. Virtuális gép létrehozása esetén fontos toochoose a lemez méretét és a konfigurációs megfelelő toohello várható terhelést. Ez az oktatóanyag ismerteti, központi telepítésére és felügyeletére a virtuális gépek lemezei. További tudnivalók:

> [!div class="checklist"]
> * Az operációs rendszer és a ideiglenes lemezek
> * Az adatlemezek
> * Standard és Premium lemezek
> * Lemez teljesítménye
> * Csatlakoztatása és adatlemezek előkészítése

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

## <a name="default-azure-disks"></a>Azure-lemezeket alapértelmezett

Egy Azure virtuális gép létrehozásakor a két lemez automatikusan csatolt toohello virtuális gép. 

**Operációsrendszer-lemez** - operációsrendszer-lemezek is kell méretezni too1 terabájt fel, és a gazdagépek hello virtuális gépek operációs rendszer.  hello operációsrendszer-lemez van rendelve egy meghajtóbetűjelét *c:* alapértelmezés szerint. hello lemez gyorsítótárazás hello operációsrendszer-lemez konfigurációját úgy optimalizálták, hogy az operációs rendszer teljesítményét. az operációs rendszer hello lemez **nem kell** alkalmazások vagy az adatok tárolására szolgál. Az alkalmazások és adatok használja az adatlemezt, amely a cikk későbbi részében részleteit.

**Ideiglenes lemez** -ideiglenes lemezeken található SSD-meghajtó használatát hello azonos Azure-gazdagép, hello VM. Ideiglenes lemezek magas performant és műveletek, például a ideiglenes adatfeldolgozási használható. Azonban ha hello VM áthelyezett tooa új gazdagép, eltávolítják egy ideiglenes lemezen tárolt adatokat. Virtuálisgép-méretet hello hello hello ideiglenes lemez mérete határozza meg. Ideiglenes lemezek vannak hozzárendelve a meghajtóbetűjel *d:* alapértelmezés szerint.

### <a name="temporary-disk-sizes"></a>Ideiglenes mérete

| Típus | Virtuálisgép-mérettel | Maximális ideiglenes lemez (GB) |
|----|----|----|
| [Általános célú](sizes-general.md) | A-D sorozat része | 800 |
| [Számításra optimalizált](sizes-compute.md) | F sorozat | 800 |
| [Memóriaoptimalizált](../virtual-machines-windows-sizes-memory.md) | D és G sorozat | 6144 |
| [Tárolásra optimalizált](../virtual-machines-windows-sizes-storage.md) | L sorozat | 5630 |
| [GPU](sizes-gpu.md) | N sorozat | 1440 |
| [Magas teljesítmény](sizes-hpc.md) | A és H sorozat | 2000 |

## <a name="azure-data-disks"></a>Az Azure data lemezek

További adatlemezt felvehető alkalmazások telepítése és adatok tárolására. Az adatlemezek minden helyzetben, ahol van szükség az adatok tartósságát és rugalmas tárolási kell használni. Minden adat lemez 1 terabájtnál maximális kapacitása nem. hello virtuális gép hello mérete határozza meg, hány adatlemezek csatolt tooa virtuális gép is lehet. A minden egyes virtuális core két adatlemezek kapcsolható ki. 

### <a name="max-data-disks-per-vm"></a>Maximális adatlemezek virtuális gépenként

| Típus | Virtuálisgép-mérettel | Maximális adatlemezek virtuális gépenként |
|----|----|----|
| [Általános célú](sizes-general.md) | A-D sorozat része | 32 |
| [Számításra optimalizált](sizes-compute.md) | F sorozat | 32 |
| [Memóriaoptimalizált](../virtual-machines-windows-sizes-memory.md) | D és G sorozat | 64 |
| [Tárolásra optimalizált](../virtual-machines-windows-sizes-storage.md) | L sorozat | 64 |
| [GPU](sizes-gpu.md) | N sorozat | 48 |
| [Magas teljesítmény](sizes-hpc.md) | A és H sorozat | 32 |

## <a name="vm-disk-types"></a>Virtuális gép lemeztípusok

Azure két lemez biztosít.

### <a name="standard-disk"></a>Standard méretű lemez

A merevlemez-meghajtókra épülő Standard Storage költséghatékony tárolási megoldás, amely emellett jó teljesítményt nyújt. Standard lemezek ideális, ha a költséghatékony fejlesztői és munkaterhelés teszteléséhez.

### <a name="premium-disk"></a>Prémium szintű lemez

Premium lemezek SSD-alapú nagy teljesítményű, alacsony késésű lemez üzemelnek. Tökéletes éles munkaterhelést futtató virtuális gépekhez. Prémium szintű Storage támogatja a DS-méretek, DSv2-méretek, GS sorozatnak és FS sorozatú virtuális gépeket. Prémium szintű lemezekhez három típusa (P10, P20, P30) helyen, és hello lemez mérete hello hello lemez típusa határozza meg. Ha választja, a lemez mérete hello értéke függvény toohello következő típusa. Például ha hello mérete nem éri el 128 GB-os hello lemeztípus lesz P10, 129 és 512 P20 és több mint 512 P30 között. 

### <a name="premium-disk-performance"></a>Prémium szintű lemez teljesítménye

|Prémium szintű storage lemeztípus | P10 | P20 | P30 |
| --- | --- | --- | --- |
| A lemezméret (kerek mentése) | 128 GB | 512 GB | 1024 GB (1 TB) |
| IOPS-érték lemezenként | 500 | 2,300 | 5,000 |
Adattovábbítás lemezenként | 100 MB/s | 150 MB/s | 200 MB/s |

Táblázat felett hello azonosítja a maximális iops-érték lemezenként, magasabb szintű teljesítmény legyen elérhető több adatlemezek szétosztott. Például lehet lemezeket 64 adatok csatolt tooStandard_GS5 virtuális gép. Ha ezeknek a lemezeknek mindegyikének, egy P30 mérete, 80000 IOPS maximális érhető el. A virtuális gép maximális iops-értéket részletes információkért lásd: [Linux Virtuálisgép-méretek](./sizes.md).

## <a name="create-and-attach-disks"></a>Hozzon létre, és csatlakoztassa a lemezeket

Ebben az oktatóanyagban toocomplete hello példában rendelkeznie kell egy meglévő virtuális gépet. Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) hozhat létre egyet. Ha feldolgozása révén hello oktatóanyag, a csere hello erőforráscsoport és a virtuális gép nevét, ha szükséges.

Hozzon létre a kezdeti konfiguráció hello [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig). a következő példa hello konfigurálja 128 GB-nál a lemezt.

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

Hozzon létre hello adatlemez hello [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) parancsot.

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

Get hello virtuális gép, amelyet az tooadd hello adatok lemez toowith hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) parancsot.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Hozzáadás hello adatok lemez toohello virtuálisgép-konfigurációnak hello [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) parancsot.

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

Frissítse hello virtuális gépet hello [frissítés-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) parancsot.

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a>Az adatlemezek előkészítése

A lemez virtuális géphez csatolt toohello követően hello operációs rendszer kell konfigurált toobe toouse hello lemez. hello következő példa bemutatja, hogyan konfigurálhat egy hello első lemezt hozzá az toomanually toohello virtuális gép. Ez a folyamat is automatizálható hello segítségével [egyéni parancsprogramok futtatására szolgáló bővítmény](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Kézi konfigurálás

Hello virtuális gép RDP-kapcsolat létrehozásához. Nyissa meg a PowerShell és a parancsfájl futtatásával.

```powershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban megismerte méretű lemezek témakörök, mint:

> [!div class="checklist"]
> * Az operációs rendszer és a ideiglenes lemezek
> * Az adatlemezek
> * Standard és Premium lemezek
> * Lemez teljesítménye
> * Csatlakoztatása és adatlemezek előkészítése

Előzetes toohello oktatóanyag következő toolearn kapcsolatos automatizálása a Virtuálisgép-konfigurációhoz.

> [!div class="nextstepaction"]
> [Virtuálisgép-konfiguráció létrehozása](./tutorial-automate-vm-deployment.md)
