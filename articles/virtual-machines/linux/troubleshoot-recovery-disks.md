---
title: "a Linux virtuális gép hibaelhárítás az Azure CLI 2.0 hello aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan Linux virtuális gép problémák használatával csatlakozó hello OS lemez tooa helyreállítási virtuális gép tootroubleshoot hello Azure CLI 2.0"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: iainfou
ms.openlocfilehash: 776d61b61280f46e3699157addcdb1e7dfb6818e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-with-hello-azure-cli-20"></a>Linux virtuális gép operációs rendszer hello lemez tooa helyreállítási virtuális Gépet hello Azure CLI 2.0 csatolásával hibaelhárítása
Ha a Linux virtuális gép (VM) rendszerindító vagy a lemez hibát tapasztal, szükség lehet a tooperform hibaelhárítási hello virtuális merevlemezen magát. Ilyenek például a bejegyzés érvénytelen lenne `/etc/fstab` , amely megakadályozza hello méretű képes tooboot sikeresen. Ez a cikk részletek hogyan toouse hello Azure CLI 2.0 tooconnect a virtuális merevlemez lemez tooanother Linux virtuális gép toofix ki a hibákat, majd hozza létre újból az eredeti virtuális gép. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="recovery-process-overview"></a>Helyreállítási folyamat áttekintése
hibaelhárítási folyamatának hello a következőképpen történik:

1. Törölje a hello VM észlelt problémák, hello virtuális merevlemezek tartása.
2. Csatolja, és a csatlakoztatási hello virtuális merevlemez tooanother Linux virtuális gép hibaelhárítási célból.
3. Csatlakoztassa a VM hibaelhárítási toohello. Szerkesztheti a fájlokat, vagy futtassa az olyan eszközöket toofix problémák hello eredeti virtuális merevlemez.
4. Válassza le a lemezképet, és válassza le hello virtuális merevlemezt a virtuális gép hibakeresési hello.
5. Hello eredeti virtuális merevlemez virtuális gép létrehozása.

tooperform ezek a hibaelhárítási lépéseket, hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Hello alábbi példák cserélje le a paraméter nevét a saját értékeit. Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.


## <a name="determine-boot-issues"></a>Határozza meg a rendszerindítási problémák
Vizsgálja meg a hello soros kimeneti toodetermine miért a virtuális gép nem tud tooboot megfelelően. Ilyenek például a bejegyzés érvénytelen `/etc/fstab`, vagy az alapul szolgáló virtuális merevlemez alatt töröltek vagy áthelyeztek hello.

Hello rendszerindító naplófájlok rendelkező [az virtuális gép rendszerindítási-diagnosztika get-rendszerindítási-naplófájl](/cli/azure/vm/boot-diagnostics#get-boot-log). hello alábbi példa lekérése hello soros kimeneti hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

Tekintse át a hello soros kimeneti toodetermine miért hello virtuális gép nem tud tooboot. Ha soros kimeneti hello nem ad meg semmilyen arra utal, hogy, szükség lehet a naplófájlokat tooreview `/var/log` után hello virtuális géphez csatlakoztatott merevlemez VM hibaelhárítási tooa.


## <a name="view-existing-virtual-hard-disk-details"></a>Meglévő virtuális merevlemez részleteinek megtekintése
A virtuális merevlemez (VHD) tooanother VM csatolhat, meg kell tooidentify hello hello operációsrendszer-lemez URI Azonosítóját. 

Tekintse meg a virtuális Gépet a [az vm megjelenítése](/cli/azure/vm#show). Használjon hello `--query` jelző tooextract hello URI toohello OS lemezre. hello alábbi példa lekérdezi hello nevű virtuális gép lemezeinek adatai `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

hello URI túl hasonló**https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.

## <a name="delete-existing-vm"></a>Meglévő virtuális gép törlése
A virtuális merevlemezek és a virtuális gépek az Azure-erőforrások két különböző típusa. A virtuális merevlemez hello operációs rendszert illeti, alkalmazások és konfigurációk tárolására. hello virtuális gépért csak metaadatokat, amelyek hello vagy a hely határozza meg, és hivatkozik arra az erőforrások, például egy virtuális merevlemezt vagy virtuális hálózati kártya (NIC). Minden virtuális merevlemez rendelkezik-e létrehozásakor kell csatolni a címbérlet tooa virtuális gép. Bár adatlemezt csatolni, és leválasztott hello virtuális gép futása közben is, hello operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép erőforrásához hello törlődik. hello bérleti tooassociate hello OS lemezt a virtuális gépek továbbra is, akkor is, ha ezt a virtuális Gépet felszabadított és leállított állapotban van.

első lépés toorecover hello a virtuális gép toodelete hello VM erőforrás magát. Virtuális gép törlése hello hello virtuális merevlemezek elhagyja a tárfiókban lévő. Hello a virtuális gép törlődik, miután hello virtuális merevlemez tooanother VM tootroubleshoot csatolja, és hárítsa el a hello hibákat.

Törölje a virtuális gép hello [az virtuális gép törlése](/cli/azure/vm#delete). a következő példa törlések hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportból `myResourceGroup`:

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

Várjon, amíg hello virtuális gép törlése előtt hello virtuális merevlemez tooanother virtuális gép csatlakoztatása befejeződött. hello virtuális merevlemezt, amely társítja azt hello VM hello bérlete hello virtuális merevlemez tooanother VM előtt kiadott toobe kell.


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a>Meglévő virtuális merevlemez tooanother VM csatolása
A hello ezután néhány lépést, akkor egy másik virtuális gép hibaelhárítási célból. Hello VM toobrowse hibaelhárítási meglévő virtuális merevlemez toothis csatolja, és hello lemez tartalmának szerkesztéséhez. Ez a folyamat toocorrect lehetővé teszi bármely konfigurációs hibák vagy a felülvizsgálati további alkalmazások vagy a rendszer naplófájljait, például. Válasszon, vagy hozzon létre egy másik virtuális gép toouse hibaelhárítási célból.

Hello meglévő virtuális merevlemez csatlakoztatása [nem felügyelt az virtuálisgép-lemez csatolása](/cli/azure/vm/unmanaged-disk#attach). Hello meglévő virtuálismerevlemez-fájl csatolása, adja meg a hello URI toohello lemez előző hello beolvasott `az vm show` parancsot. hello alábbi példa csatolja egy meglévő virtuális merevlemez toohello nevű virtuális gép hibakeresési `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`:

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-hello-attached-data-disk"></a>Hello csatolt adatlemez csatlakoztatása

> [!NOTE]
> hello következő példák részletesen hello lépéseit egy Ubuntu virtuális gép. Red Hat Enterprise Linux és SUSE, például a különböző Linux distro használata hello naplófájl helye és `mount` parancsok kissé eltérő lehet. A parancsok hello megfelelő változásokat az adott distro a tekintse meg a toohello dokumentációját.

1. SSH tooyour hibaelhárítási hello megfelelő hitelesítő adatokat használó virtuális gépek. Ha a lemez hello első adatok csatlakoztatott lemez tooyour VM hibaelhárítási, hello lemez valószínűleg csatlakozik-e meg a túl`/dev/sdc`. Használjon `dmseg` tooview csatlakoztatott lemezekkel:

    ```bash
    dmesg | grep SCSI
    ```

    hello hasonló toohello a következő példa a kimenetre:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    A fenti példa hello, hello operációsrendszer-lemez jelenleg `/dev/sda` hello ideiglenes lemez megadott összes virtuális Géphez és `/dev/sdb`. Ha több adatlemezek, hogy legyen `/dev/sdd`, `/dev/sde`, és így tovább.

2. Hozzon létre egy könyvtárat toomount a meglévő virtuális merevlemez. hello alábbi példa létrehoz egy könyvtárat nevű `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Ha több partíciót a meglévő virtuális merevlemezen, csatlakoztassa a szükséges hello partíció. hello alábbi példa csatlakoztatja, elsődleges partíció első hello `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Ajánlott toomount adatlemezek virtuális gépeken Azure használatával hello univerzálisan egyedi azonosító (UUID) hello virtuális merevlemez. A jelen rövid hibaelhárítási esetben csatlakoztatását a hello virtuális merevlemez segítségével hello UUID nincs szükség. Azonban a normál használja, a Szerkesztés `/etc/fstab` toomount virtuális merevlemezek helyett eszköznév UUID okozhat hello VM toofail tooboot.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Az eredeti virtuális merevlemez kapcsolatos problémák megoldása
Hello meglévő virtuális merevlemezzel csatlakoztatva is képes lemezvizsgálatok elvégzésére bármely karbantartási és hibaelhárítási lépéseket, igény szerint. Egyszer foglalkoztak hello problémák, folytassa a következő lépéseket hello.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Válassza le a lemezképet, és válassza le az eredeti virtuális merevlemez
Ha a hibák fakadó problémák megoldásával válassza le, és hello meglévő virtuális merevlemez válassza le a hibaelhárítási virtuális Gépet. Nem használhat a virtuális merevlemez más virtuális gép hello virtuális merevlemez toohello hibaelhárítás a virtuális gép csatlakoztatása hello bérleti kiadásáig.

1. A hello a virtuális gép hibakeresési SSH munkamenet tooyour leválasztása hello létező virtuális merevlemezt. Először kívül hello szülőkönyvtárában a csatlakoztatási pont módosítása:

    ```bash
    cd /
    ```

    Most leválasztása hello létező virtuális merevlemezt. hello alábbi példa leválasztja hello eszközéből `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Most válassza le a hello hello virtuális gép a virtuális merevlemezt. Lépjen ki a virtuális gép hibakeresési hello SSH-munkamenet tooyour. Lista hello csatolt virtuális gép és hibaelhárítási adatokat lemezek tooyour [az virtuálisgép-lemez nem felügyelt lista](/cli/azure/vm/unmanaged-disk#list). hello alábbi példa listák hello adatlemezt csatolni toohello nevű virtuális gép `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`:

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    Vegye figyelembe a meglévő virtuális merevlemez hello nevét. Például egy lemez neve hello hello URI-jának **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** van **myVHD**. 

    Válassza le a virtuális gép hello adatlemezét [nem felügyelt az virtuálisgép-lemez leválasztása](/cli/azure/vm/unmanaged-disk#detach). hello alábbi példa leválasztja nevű hello lemez `myVHD` hello nevű virtuális gép a `myVMRecovery` a hello `myResourceGroup` erőforráscsoport:

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a>Virtuális gép eredeti merevlemez létrehozása
toocreate egy virtuális Gépet az eredeti virtuális merevlemez használata [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd). hello tényleges JSON-sablon jelenleg a következő hivatkozás hello:

- https://RAW.githubusercontent.com/Azure/Azure-quickstart-Templates/Master/201-VM-specialized-VHD/azuredeploy.JSON

hello sablon telepíti a virtuális gépek virtuális merevlemez URI hello hello a korábbi parancsot. A hello sablon üzembe helyezése [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create). Adja meg a hello URI tooyour eredeti VHD és adja meg hello az operációs rendszer típusát, a Virtuálisgép-méretet és a virtuális gép nevét az alábbiak szerint:

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a>Engedélyezze újra a rendszerindítási diagnosztika
Hello létező virtuális merevlemezt a virtuális Gépet hoz létre, ha rendszerindítási diagnosztika automatikusan nem lehet engedélyezni. A rendszerindítási diagnosztika engedélyezése [az virtuális gép rendszerindítási-diagnosztika engedélyezése](/cli/azure/vm/boot-diagnostics#enable). hello következő példa engedélyezi hello hello nevű virtuális gép diagnosztikai kiterjesztés `myDeployedVM` nevű hello erőforráscsoportban `myResourceGroup`:

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a>Következő lépések
Ha a Kapcsolódás a virtuális gép tooyour problémát tapasztal, tekintse meg [hibaelhárítása SSH kapcsolatok tooan Azure virtuális gép](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Linux virtuális gép](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
