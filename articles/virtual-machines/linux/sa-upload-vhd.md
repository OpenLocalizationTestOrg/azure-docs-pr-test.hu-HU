---
title: "Azure CLI 2.0 egyéni Linux lemezzel aaaUpload |} Microsoft Docs"
description: "Hozzon létre, és töltse fel a virtuális merevlemez (VHD) tooAzure hello Resource Manager üzembe helyezési modellben és hello Azure CLI 2.0 használatával"
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
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: cf8556ab4dfe6c4e5eff4e99fe1ddc44393f774c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a>Töltse fel, és a Linux virtuális gép létrehozása az Azure CLI 2.0 hello egyéni lemezről
Ez a cikk bemutatja, hogyan tooupload egy virtuális merevlemez (VHD) tooan az Azure storage rendelkező fiók hello Azure CLI 2.0, és a Linux virtuális gépek létrehozása a egyéni lemezt. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ez a funkció lehetővé teszi tooinstall, és egy Linux distro tooyour követelmények konfigurálása, és ezután használja az adott VHD tooquickly létrehozása az Azure virtuális gépek (VM).

Ez a témakör az hello végső virtuális merevlemezek, de is végrehajthatja a lépések használatával tárfiókok használ [által kezelt lemezeken](upload-vhd.md). 

## <a name="quick-commands"></a>Gyors parancsok
Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello hello kiinduló parancsok tooupload egy virtuális merevlemez tooAzure. Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#requirements).

Győződjön meg arról, hogy rendelkezik hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `mydisks`.

Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `WestUs` helye:

```azurecli
az group create --name myResourceGroup --location westus
```

Hozzon létre egy tárolási fiók toohold a virtuális lemezek, amelyek [az storage-fiók létrehozása](/cli/azure/storage/account#create). hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

A tárfiók hello elérési kulcsainak listázása [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list). Jegyezze fel a `key1`:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

Hozzon létre egy tárolót a tárfiókon belül hello biztonságitár-kulcs használatával kapott [az tároló létrehozása](/cli/azure/storage/container#create). hello alábbi példa létrehoz egy nevű tárolót `mydisks` hello tárolási értékének használatával `key1`:

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Végül, töltse fel a létrehozott VHD-toohello tároló [az tárolási blob feltöltése](/cli/azure/storage/blob#upload). Adja meg a hello helyi elérési út tooyour virtuális merevlemez alapján `/path/to/disk/mydisk.vhd`:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

Adja meg a hello URI tooyour lemez (`--image`) rendelkező [az virtuális gép létrehozása](/cli/azure/vm#create). hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` hello korábban feltöltött virtuális lemez segítségével:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

hello céltárfiókot hello ugyanaz, mint ahol a virtuális lemez feltöltött toobe rendelkezik. Továbbá toospecify kell, vagy a vonatkozó választ, minden hello hello szükséges további paraméterek **az virtuális gép létrehozása** parancs például a virtuális hálózat, a nyilvános IP-cím, a felhasználónevet és az SSH-kulcsok. További tudnivalók hello [érhető el erőforrás-kezelő parancssori paraméterek](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Követelmények
toocomplete hello lépéseket követve van szüksége:

* **Linux operációs rendszer van telepítve, a .vhd-fájllá** -telepíteni egy [Azure által támogatott Linux-disztribúció](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa hello VHD lévő virtuális lemezek formátumban. Több különféle eszköz toocreate érhetők el a virtuális gép és a virtuális merevlemez:
  * Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve toouse a lemezkép formátumú virtuális Merevlemezt. Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával `qemu-img convert`.
  * Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello újabb VHDX formátum nem támogatott az Azure-ban. Amikor létrehoz egy virtuális Gépet, adja meg a virtuális merevlemez hello formátumban. Ha szükséges, VHDX-lemezek tooVHD használatával konvertálhatja [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot. További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért meg kell tooconvert ilyen lemezek toostatic VHD-k feltöltés előtt. Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert a dinamikus lemezek a hello folyamat tooAzure feltöltése során.
> 
> 

* Az egyéni lemez alapján létrehozott virtuális gépek kell lennie, hello azonos maga hello lemezként storage-fiók
  * Hozzon létre egy tárolási fiók és a tároló toohold, az egyéni lemez és a létrehozott virtuális gépek
  * Miután létrehozta a virtuális gépek, nyugodtan törölheti a lemezen

Győződjön meg arról, hogy rendelkezik hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `mydisks`.

<a id="prepimage"> </a>

## <a name="prepare-hello-disk-toobe-uploaded"></a>Készítse elő a hello lemez toobe feltöltve.
Azure támogatja a különböző Linux terjesztésekről (lásd: [támogatott Disztribúciókkal](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). a következő cikkek hello végigvezetik hogyan tooprepare hello Azure által támogatott különböző Linux-disztribúció:

* **[CentOS-alapú Disztribúciók](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Egyéb - nem támogatott Disztribúciókkal](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Lásd még: hello  **[Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes)**  kapcsolatos további általános tippek Linux lemezképek előkészítése az Azure.

> [!NOTE]
> Hello [Azure platformon SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) Linux operációs rendszert futtató, csak ha egyik hello által támogatott disztribúciók használt hello konfigurációs adatait a támogatott verziók tooVMs érvényes [az Azure által támogatott Linux Azokat a terjesztéseket](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
Erőforráscsoportok logikailag összefogni összes hello Azure-erőforrások toosupport a virtuális gépek, például a hello virtuális hálózati és tárolási. További információ erőforráscsoportok, lásd: [erőforráscsoportokat áttekintése](../../azure-resource-manager/resource-group-overview.md). Feltöltése az egyéni lemez és virtuális gépek létrehozását, mielőtt először toocreate nevű erőforráscsoport [az csoport létrehozása](/cli/azure/group#create).

hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westus` helye:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>Create a storage account

Hozzon létre egy tárfiókot, az egyéni lemez és virtuális gépek [az storage-fiók létrehozása](/cli/azure/storage/account#create). A virtuális gépeket hoz létre az egyéni lemez kell toobe a nem felügyelt lemezzel hello ugyanazt a tárfiókot, a lemeznek. 

hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount` hello erőforráscsoportban korábban hozott létre:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Tárfiókkulcsok listázása
Az Azure létrehoz két 512 bites kulcsot minden tárfiók. A hozzáférési kulcsot használnak az toohello tárfiókot, például az írási műveletek toocarry hitelesítésekor. Tudjon meg többet az [kezelése hozzáférési toostorage Itt](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Megtekintheti a hello hívóbetűk [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).

Hello elérési kulcsainak megtekintése létrehozott tárfiók hello:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
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
Jegyezze fel a `key1` még szüksége lesz rájuk toointeract a storage-fiók hello következő lépésben leírtak szerint.

## <a name="create-a-storage-container"></a>A tároló létrehozása
A hello azonos módon, hogy hozzon létre különböző könyvtárak toologically rendezheti a helyi fájlrendszer, a lemezek létrehozni a tárolási fiók tooorganize tárolókra. A storage-fiók korlátlan számú tárolót tárolhat tartalmazhat. Hozzon létre egy tárolót a [az tároló létrehozása](/cli/azure/storage/container#create).

hello alábbi példa létrehoz egy nevű tárolót `mydisks`:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>Virtuális merevlemez feltöltéséhez
Töltsön fel az egyéni lemezzel [az tárolási blob feltöltése](/cli/azure/storage/blob#upload). Töltse fel, és tárolja az egyéni lemezt oldalblobként.

Adja meg a hozzáférési kulcsot a hello tároló hello előző lépésben létrehozott, és ezután hello az elérési út toohello egyéni lemezt a helyi számítógépen:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-hello-vm"></a>Hello virtuális gép létrehozása
nem felügyelt lemezzel rendelkező virtuális gép toocreate meg hello URI tooyour lemezt (`--image`) rendelkező [az virtuális gép létrehozása](/cli/azure/vm#create). hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` hello korábban feltöltött virtuális lemez segítségével:

Megadja a hello `--image` paraméterrel [az virtuális gép létrehozása](/cli/azure/vm#create) toopoint tooyour egyéni lemez. Győződjön meg arról, hogy `--storage-account` egyező hello tárfiók a egyéni lemezen tárolja. Nincs toouse hello ugyanabban a tárolóban, egyéni lemez toostore hello a virtuális gépek. Győződjön meg arról, hogy toocreate további tárolókkal hello azonos módon hello korábbi lépéseket az egyéni lemez feltöltés előtt.

hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` a egyéni lemezről:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

Ön továbbra is szükséges toospecify, vagy a választ kér, összes hello hello szükséges további paraméterek **az virtuális gép létrehozása** parancsot például felhasználónév és SSH-kulcsok.


## <a name="resource-manager-template"></a>Resource Manager-sablon
Az Azure Resource Manager-sablonok a JavaScript Object Notation (JSON) fájlok hello környezet toobuild kívánja. hello sablonok az erőforrás-szolgáltatók toodifferent például számítási és hálózati részletezik. Meglévő sablonok használhatja, vagy a saját írni. Tudjon meg többet az [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).

Hello belül `Microsoft.Compute/virtualMachines` a sablon-szolgáltatót, hogy egy `storageProfile` hello konfigurációs adatait a virtuális gép tartalmazó csomópontot. hello két fő paraméterek tooedit hello `image` és `vhd` URI-azonosítók, amelyek tooyour egyéni és az új virtuális gép virtuális lemezzel. hello következő hello JSON látható az egyéni lemezt használ:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Használhat [a meglévő sablon toocreate egy virtuális Gépet egy egyéni lemezképből](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) vagy foglalkozó témakört [a saját Azure Resource Manager-sablonok készítése](../../azure-resource-manager/resource-group-authoring-templates.md). 

Ha egy sablon konfigurálva van, a [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) toocreate a virtuális gépek. Adja meg a JSON-sablon URI hello hello `--template-uri` paraméter:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Ha egy számítógépen helyben tárolt JSON-fájl, használhatja a hello `--template-file` paraméter helyette:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Következő lépések
Miután előkészített és feltöltése az egyéni virtuális lemez, akkor további információ [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md). Akkor is érdemes lehet túl[hozzá adatlemezt](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour új virtuális gépeket. Ha a virtuális gépeken futó, hogy kell-e tooaccess alkalmazásai, ne feledje túl[nyisson meg portokat és a végpontok](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

