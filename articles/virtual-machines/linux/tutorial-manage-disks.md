---
title: az Azure CLI hello lemezek aaaManage Azure |} Microsoft Docs
description: "Útmutató – az Azure CLI hello Azure lemezek kezelése"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ad29cfbec5f9738f47705b19d0450c9a2f555492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-hello-azure-cli"></a>Az Azure CLI hello Azure lemezek kezelése

Az Azure virtuális gépek használata a lemezek toostore hello virtuális gépek operációs rendszer, alkalmazások és adatok. Virtuális gép létrehozása esetén fontos toochoose a lemez méretét és a konfigurációs megfelelő toohello várható terhelést. Ez az oktatóanyag ismerteti, központi telepítésére és felügyeletére a virtuális gépek lemezei. További tudnivalók:

> [!div class="checklist"]
> * Az operációs rendszer és a ideiglenes lemezek
> * Az adatlemezek
> * Standard és Premium lemezek
> * Lemez teljesítménye
> * Csatlakoztatása és adatlemezek előkészítése
> * Lemezek átméretezése
> * Lemez pillanatképek


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="default-azure-disks"></a>Azure-lemezeket alapértelmezett

Egy Azure virtuális gép létrehozásakor a két lemez automatikusan csatolt toohello virtuális gép. 

**Operációsrendszer-lemez** - operációsrendszer-lemezek is kell méretezni too1 terabájt fel, és a gazdagépek hello virtuális gépek operációs rendszer. az operációs rendszer hello lemez lett címkézve */dev/sda* alapértelmezés szerint. hello lemez gyorsítótárazás hello operációsrendszer-lemez konfigurációját úgy optimalizálták, hogy az operációs rendszer teljesítményét. Ez a konfiguráció miatt hello operációsrendszer-lemez **nem kell** alkalmazások vagy az adatok tárolására szolgál. Az alkalmazások és adatok használja az adatlemezek, amely részletes leírást a cikk későbbi részében. 

**Ideiglenes lemez** -ideiglenes lemezeken található SSD-meghajtó használatát hello azonos Azure-gazdagép, hello VM. Ideiglenes lemezek magas performant és műveletek, például a ideiglenes adatfeldolgozási használható. Azonban ha hello VM áthelyezett tooa új gazdagép, eltávolítják egy ideiglenes lemezen tárolt adatokat. Virtuálisgép-méretet hello hello hello ideiglenes lemez mérete határozza meg. Ideiglenes lemezek tartalma */dev/sdb* , és a csatlakoztatási pont a */mnt*.

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

Premium lemezek SSD-alapú nagy teljesítményű, alacsony késésű lemez üzemelnek. Tökéletes éles munkaterhelést futtató virtuális gépekhez. Prémium szintű Storage támogatja a DS-méretek, DSv2-méretek, GS sorozatnak és FS sorozatú virtuális gépeket. Prémium szintű lemezekhez három típusa (P10, P20, P30) helyen, és hello lemez mérete hello hello lemez típusa határozza meg. Ha választja, a lemez mérete hello értéke függvény toohello következő típusa. Például ha hello lemez mérete 128 GB-nál kevesebb, hello lemeztípus P10. Ha hello lemezméretet 129 GB és 512 GB közötti, hello mérete egy P20. Több mint 512 GB semmit hello mérete a P30.

### <a name="premium-disk-performance"></a>Prémium szintű lemez teljesítménye

|Prémium szintű storage lemeztípus | P10 | P20 | P30 |
| --- | --- | --- | --- |
| A lemezméret (kerek mentése) | 128 GB | 512 GB | 1024 GB (1 TB) |
| Lemezenkénti maximális IOPS-érték | 500 | 2,300 | 5,000 |
Adattovábbítás lemezenként | 100 MB/s | 150 MB/s | 200 MB/s |

Táblázat felett hello azonosítja a maximális iops-érték lemezenként, magasabb szintű teljesítmény legyen elérhető több adatlemezek szétosztott. Virtuális gép Standard_GS5 például 80000 IOPS maximális érhető el. A virtuális gép maximális iops-értéket részletes információkért lásd: [Linux Virtuálisgép-méretek](sizes.md).

## <a name="create-and-attach-disks"></a>Hozzon létre, és csatlakoztassa a lemezeket

Az adatlemezek hozható létre és csatolni a virtuális gép létrehozásakor vagy meglévő virtuális gép tooan.

### <a name="attach-disk-at-vm-creation"></a>A virtuális gép létrehozásakor lemez csatolása

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot. 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

Hello virtuális gép létrehozása [az virtuális gép létrehozása]( /cli/azure/vm#create) parancsot. Hello `--datadisk-sizes-gb` argumentum használt toospecify, hogy egy további szabad létre kell hoznia és toohello virtuális géphez csatolni. toocreate és egynél több lemez csatolása, a lemez mérete értékének szóközökkel elválasztott kötetnevek listáját használja. A következő példa hello a virtuális gépek két adatlemezek, mindkét 128 GB hozza létre. Mivel hello mérete 128 GB-os, ezek a lemezek egyaránt be van állítva, P10s, amely legfeljebb 500 iops-érték lemezenként.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-tooexisting-vm"></a>Csatlakoztassa a lemezt tooexisting VM

toocreate és csatlakoztassa az új lemez tooan meglévő virtuális gépet, hello használata [az méretű lemez csatolása](/cli/azure/vm/disk#attach) parancsot. hello alábbi példa létrehoz egy prémium szintű lemez 128 GB-nál, és csatolja azt toohello hello utolsó lépésben létrehozott virtuális gép.

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Az adatlemezek előkészítése

A lemez virtuális géphez csatolt toohello követően hello operációs rendszer kell konfigurált toobe toouse hello lemez. hello a következő példa bemutatja, hogyan konfigurálhat egy toomanually egy lemezt. Ez a folyamat is automatizálható a felhő-init használatával egy [az oktatóanyag későbbi](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Kézi konfigurálás

Az SSH-kapcsolat létrehozása hello virtuális géppel. Cserélje le hello példa IP-cím hello nyilvános IP-cím hello virtuális gép.

```azurecli-interactive 
ssh 52.174.34.95
```

Partíció hello lemezzel `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Egy fájl rendszerpartíciót toohello írási hello segítségével `mkfs` parancsot.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Csatlakoztassa a hello új lemezt, így hello operációs rendszerben érhető el.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

hello lemez most már a következőkkel érhetők el hello *datadrive* hello futtatásával ellenőrizheti csatlakozási pont `df -h` parancsot. 

```bash
df -h
```

hello az alábbiakat mutatja be hello új meghajtót csatlakoztatott */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

amely a meghajtó hello tooensure van csatlakoztatni, a rendszer újraindítása után, akkor hozzá kell adni toohello */etc/fstab* fájlt. toodo tehát beolvasása hello UUID hello lemez a hello `blkid` segédprogram.

```bash
sudo -i blkid
```

hello kimenete felsorolja hello hello meghajtó UUID `/dev/sdc1` ebben az esetben.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Adja hozzá a következő toohello sor hasonló toohello */etc/fstab* fájlt. Is vegye figyelembe, hogy az írási korlátok használatával tiltható *korlát = 0*, ez a konfiguráció lemez a jobb teljesítmény érdekében. 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

Most, hogy hello lemez van konfigurálva, zárja be a hello SSH-munkamenetet.

```bash
exit
```

## <a name="resize-vm-disk"></a>Virtuálisgép-lemez átméretezése

Ha egy virtuális Gépre van telepítve, a hello operációsrendszer-lemez vagy a mellékelt adatok lemezzel mérete növelhető. A lemez mérete hello növelése akkor előnyös, ha több tárhelyet igényel vagy magasabb szintű teljesítmény (P10, P20, P30). Vegye figyelembe, hogy lemezek csökkenteni mérete nem lehet.

Lemez méretének növelése, előtt azonosító hello, vagy hello lemez neve van szükség. Használjon hello [az Lemezlista](/cli/azure/vm/disk#list) parancs tooreturn összes erőforráscsoport lemezeit. Jegyezze fel a hello lemez neve, hogy szeretné-e tooresize.

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

virtuális gép hello is felszabadított kell lennie. Használjon hello [az virtuális gép felszabadítása]( /cli/azure/vm#deallocate) toostop parancsot, és hello VM felszabadítani.

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Használjon hello [az lemez frissítés](/cli/azure/vm/disk#update) parancs tooresize hello lemez. Ez a példa átméretezi nevű lemez *myDataDisk* too1 terabájt.

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Hello átméretezés befejezése után indítsa el a virtuális gép hello.

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

Ha már átméretezték, ezért hello operációs rendszer tárolására, a rendszer automatikusan kiegészíti a hello partíció. Ha adatlemezt átméretezte, aktuális partíciókat kell toobe hello virtuális gépek operációs rendszer bővíthető.

## <a name="snapshot-azure-disks"></a>Pillanatkép készítése az Azure-lemezeket

Lemez pillanatkép létrehozása van folyamatban hello lemez olvasási csak, pont időponthoz kötött másolatot készít. Az Azure virtuális gép pillanatképei hasznos, ha gyorsan állapotmentést hello a virtuális gépek konfigurációs módosítások végrehajtása előtt. Hello esemény hello konfigurációs módosításokat a rendszer nem kívánt toobe, VM állapotának hello pillanatkép állíthatók vissza. Ha egy virtuális gép több lemez, a rendszer pillanatképet készít minden lemez függetlenül hello másoknak. Alkalmazás konzisztens biztonsági másolatok fogadására, fontolja meg a lemez pillanatképek készítése előtt hello VM leállítása. Másik megoldásként használhatja a hello [Azure Backup szolgáltatás](/azure/backup/), mely lehetővé teszi, hogy Ön tooperform automatikus biztonsági mentés hello virtuális gép futása közben.

### <a name="create-snapshot"></a>Pillanatkép létrehozása

Mielőtt létrehozna egy virtuálisgép-lemez pillanatképét, hello hello lemez nevű vagy azonosítójú van szükség. Használjon hello [az vm megjelenítése](https://docs.microsoft.com/en-us/cli/azure/vm#show) parancs tooreturn hello lemezazonosítót. Ebben a példában hello lemezazonosító van egy változó tárolja, hogy egy későbbi lépésben használható.

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Most, hogy hello virtuálisgép-lemez hello azonosítója, hello következő parancs létrehoz egy pillanatképet hello lemez.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Pillanatképből lemez létrehozása

A pillanatkép majd létrehozható egy lemezt, amely használt toorecreate hello virtuális géphez.

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Virtuális gép pillanatkép visszaállítása

toodemonstrate virtuális gép helyreállítása, törlési hello meglévő virtuális gépet. 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Hozzon létre egy új virtuális gép hello pillanatkép lemezről.

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Csatlakoztassa újra az adatok lemezre

Minden adatlemezek kell objektumkörnyezetben toobe toohello virtuális gépet.

Először található hello adatok Lemeznév hello segítségével [az Lemezlista](https://docs.microsoft.com/cli/azure/disk#list) parancsot. Ez a Példa helyek hello nevű változóban hello lemez neve *datadisk*, amellyel hello következő lépésben.

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Használjon hello [az méretű lemez csatolása](https://docs.microsoft.com/cli/azure/vm/disk#attach) parancs tooattach hello lemez.

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban megismerte méretű lemezek témakörök, mint:

> [!div class="checklist"]
> * Az operációs rendszer és a ideiglenes lemezek
> * Az adatlemezek
> * Standard és Premium lemezek
> * Lemez teljesítménye
> * Csatlakoztatása és adatlemezek előkészítése
> * Lemezek átméretezése
> * Lemez pillanatképek

Előzetes toohello oktatóanyag következő toolearn kapcsolatos automatizálása a Virtuálisgép-konfigurációhoz.

> [!div class="nextstepaction"]
> [Virtuálisgép-konfiguráció létrehozása](./tutorial-automate-vm-deployment.md)
