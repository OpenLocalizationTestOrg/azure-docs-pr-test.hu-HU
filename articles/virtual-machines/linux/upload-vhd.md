---
title: "aaaUpload vagy Azure CLI 2.0 egyéni Linux virtuális gép másolása |} Microsoft Docs"
description: "Töltse fel vagy másolja a testreszabott virtuális gépek hello Resource Manager üzembe helyezési modellben és hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/06/2017
ms.author: cynthn
ms.openlocfilehash: 79af897120a6ba7f4a427ba6c7d0c31b7b870bd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a>Linux virtuális gép létrehozása az Azure CLI 2.0 hello egyéni lemezről

<!-- rename toocreate-vm-specialized -->

A cikkből megtudhatja, hogyan tooupload testreszabott virtuális merevlemez (VHD), vagy másolja a meglévő VHD az Azure-ban, és hozzon létre új Linux virtuális gépek (VM) hello egyéni lemezről. Telepítheti és konfigurálhatja a Linux distro tooyour követelményeknek és majd használni az adott VHD tooquickly hozzon létre egy új Azure virtuális gépet.

Ha azt szeretné toocreate több virtuális gép a testreszabott lemezről, a virtuális gép vagy a virtuális merevlemez egy lemezképet kell létrehoznia. További információkért lásd: [hozzon létre egy Azure virtuális gép hello parancssori felület használatával egyéni lemezkép](tutorial-custom-images.md).

Erre két lehetősége van:
* [VHD feltöltése](#option-1-upload-a-specialized-vhd)
* [Egy meglévő Azure virtuális gép másolása](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a>Gyors parancsok

Segítségével új virtuális gép létrehozásakor [az virtuális gép létrehozása](/cli/azure/vm#create) testreszabott vagy speciális lemezről, **csatolása** hello lemez (--csatolása operációsrendszer-lemez) egy egyéni vagy Piactéri lemezkép megadása helyett (--kép). hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* hello felügyelt nevű lemez *myManagedDisk* a testreszabott virtuális merevlemezből létrehozni:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Követelmények
toocomplete hello lépéseket követve van szüksége:

* Linux virtuális gépek, amelyek az Azure-ban történő használatra készült. Hello [Prepare hello VM](#prepare-the-vm) című szakaszát ismerteti, hogyan toofind distro adott telepítéséről információt hello Azure Linux ügynök (waagent) hello VM toowork megfelelően az Azure-ban és az Ön toobe képes tooconnect szükséges tooit SSH használatával.
* a meglévő VHD-fájl hello [Azure által támogatott Linux-disztribúció](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuális lemez hello VHD-formátumban. Több különféle eszköz toocreate érhetők el a virtuális gép és a virtuális merevlemez:
  * Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve toouse a lemezkép formátumú virtuális Merevlemezt. Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával **qemu-img átalakítása**.
  * Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello újabb VHDX formátum nem támogatott az Azure-ban. Amikor létrehoz egy virtuális Gépet, adja meg a virtuális merevlemez hello formátumban. Ha szükséges, VHDX-lemezek tooVHD használatával konvertálhatja [qemu-img átalakítása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy hello [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot. További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért meg kell tooconvert ilyen lemezek toostatic VHD-k feltöltés előtt. Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert a dinamikus lemezek a hello folyamat tooAzure feltöltése során.
> 
> 


* Győződjön meg arról, hogy rendelkezik hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-hello-vm"></a>Hello virtuális gép előkészítése

Azure támogatja a különböző Linux terjesztésekről (lásd: [támogatott Disztribúciókkal](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). a következő cikkek hello végigvezetik hogyan tooprepare hello Azure által támogatott különböző Linux-disztribúció:

* [CentOS-alapú Disztribúciók](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Egyéb - nem támogatott Disztribúciókkal](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Lásd még: hello [Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további általános tippek Linux lemezképek előkészítése az Azure.

> [!NOTE]
> Hello [Azure platformon SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) Linux operációs rendszert futtató, csak ha egyik hello által támogatott disztribúciók használt hello konfigurációs adatait a támogatott verziók tooVMs érvényes [az Azure által támogatott Linux Azokat a terjesztéseket](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="option-1-upload-a-vhd"></a>1. lehetőség: A virtuális merevlemez feltöltéséhez.

Testre szabott virtuális Merevlemezt, amely a helyi számítógépen futó vagy egy másik felhőből exportált feltölthet. toouse hello VHD toocreate egy új Azure virtuális Gépet, kell tooupload hello VHD tooa tárolási fiókját, és hozhat létre egy felügyelt lemezt hello VHD-t. 

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Feltöltése az egyéni lemez és virtuális gépek létrehozását, mielőtt először toocreate nevű erőforráscsoport [az csoport létrehozása](/cli/azure/group#create).

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye: [Azure felügyelt lemezekhez – áttekintés](../windows/managed-disks-overview.md)
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>Create a storage account

Hozzon létre egy tárfiókot, az egyéni lemez és virtuális gépek [az storage-fiók létrehozása](/cli/azure/storage/account#create). 

hello alábbi példa létrehoz egy nevű tárfiók *mystorageaccount* hello erőforráscsoportban korábban hozott létre:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Tárfiókkulcsok listázása
Az Azure létrehoz két 512 bites kulcsot minden tárfiók. A hozzáférési kulcsot használnak az toohello tárfiókot, például az írási műveletek elvégzése hitelesítésekor. Tudjon meg többet az [kezelése hozzáférési toostorage Itt](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Megtekintheti a hello hívóbetűk [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).

Hello elérési kulcsainak megtekintése létrehozott tárfiók hello:

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

hello kimeneti hasonlít:

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
Jegyezze fel a **key1** még szüksége lesz rájuk toointeract a storage-fiók hello következő lépésben leírtak szerint.

### <a name="create-a-storage-container"></a>A tároló létrehozása
A hello azonos módon, hogy hozzon létre különböző könyvtárak toologically rendezheti a helyi fájlrendszer, a lemezek létrehozni a tárolási fiók tooorganize tárolókra. A storage-fiók korlátlan számú tárolót tárolhat tartalmazhat. Hozzon létre egy tárolót a [az tároló létrehozása](/cli/azure/storage/container#create).

hello alábbi példa létrehoz egy nevű tárolót *mydisks*:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-hello-vhd"></a>Hello VHD feltöltése
Töltsön fel az egyéni lemezzel [az tárolási blob feltöltése](/cli/azure/storage/blob#upload). Töltse fel, és tárolja az egyéni lemezt oldalblobként.

Adja meg a hozzáférési kulcsot a hello tároló hello előző lépésben létrehozott, és ezután hello az elérési út toohello egyéni lemezt a helyi számítógépen:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
VHD feltöltése hello eltarthat egy ideig.

### <a name="create-a-managed-disk"></a>Kezelt lemez létrehozása


Hozzon létre egy felügyelt lemezes hello virtuális merevlemez használatával [az lemez létrehozása](/cli/azure/disk#create). hello alábbi példa létrehoz egy nevű felügyelt lemezes *myManagedDisk* hello VHD-t a tárfiók és tároló nevű tooyour feltöltött:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>2. lehetőség: Egy meglévő virtuális gép másolása

Is létrehozhat a Azure, majd az operációs rendszer hello lemezmásolás testreszabott VM hello és tooa új virtuális gép toocreate csatlakoztatása egy másik példányt. Ez ideális tesztelési, de ha egy meglévő Azure virtuális gép toouse hello modell több új virtuális gépek, valóban készítsen egy **kép** helyette. További információ a lemezkép létrehozása egy meglévő Azure virtuális gépről: [létrehozhat egyéni rendszerképeket egy Azure virtuális gép hello parancssori felület használatával](tutorial-custom-images.md)

### <a name="create-a-snapshot"></a>Pillanatkép létrehozása

Ez a példa pillanatképet hoz létre a virtuális gépek nevű *myVM* erőforráscsoportban *myResourceGroup* nevű pillanatképet készít, és *osDiskSnapshot*.

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-hello-managed-disk"></a>Hello kezelt lemez létrehozása

Hozzon létre egy új felügyelt lemezes hello pillanatkép.

Hello pillanatkép hello azonosító beszerzése. Ebben a példában hello pillanatkép nevű *osDiskSnapshot* és hello *myResourceGroup* erőforráscsoportot.

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

Hello kezelt lemez létrehozása. Ebben a példában létrehozunk egy felügyelt lemezes nevű *myManagedDisk* a pillanatképből, amely 128 GB-nál standard szintű tárolót.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-hello-vm"></a>Hello virtuális gép létrehozása

Hozza létre a virtuális Gépet a [az virtuális gép létrehozása](/cli/azure/vm#create) , és csatlakoztassa (--csatolása operációsrendszer-lemez) hello felügyelt hello az operációs rendszer lemezeként lemez. hello alábbi példakód létrehozza a virtuális gépek nevű *myNewVM* hello felügyelt a feltöltött virtuális merevlemez alapján létrehozott lemez:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

A virtuális gép hello képes tooSSH kell hello forrás virtuális gép hello hitelesítő adataival. 

## <a name="next-steps"></a>Következő lépések
Miután előkészített és feltöltése az egyéni virtuális lemez, akkor további információ [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md). Akkor is érdemes lehet túl[hozzá adatlemezt](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour új virtuális gépeket. Ha a virtuális gépeken futó, hogy kell-e tooaccess alkalmazásai, ne feledje túl[nyisson meg portokat és a végpontok](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

