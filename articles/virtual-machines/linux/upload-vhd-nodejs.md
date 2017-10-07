---
title: "az Azure CLI 1.0 egyéni Linux kép aaaUpload |} Microsoft Docs"
description: "Hozzon létre, és töltse fel a virtuális merevlemez (VHD) tooAzure hello Resource Manager üzembe helyezési modellben és hello Azure CLI 1.0 használatával egyéni Linux képének."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou
ms.openlocfilehash: 0bbd232debee1e632233d1c010e388dbc1548bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-hello-azure-cli-10"></a>Töltse fel, és a Linux virtuális gép létrehozása az egyéni lemezképet hello Azure CLI 1.0 használatával
Ez a cikk bemutatja, hogyan egy virtuális merevlemez (VHD) tooAzure használatával tooupload hello Resource Manager üzembe helyezési modellben és a Linux virtuális gépek létrehozása az egyéni lemezképet. Ez a funkció lehetővé teszi tooinstall, és egy Linux distro tooyour követelmények konfigurálása, és ezután használja az adott VHD tooquickly létrehozása az Azure virtuális gépek (VM).


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="quick-commands"></a>Gyors parancsok
Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello hello kiinduló parancsok tooupload egy virtuális gép tooAzure. Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#requirements).

Győződjön meg arról, hogy rendelkezik [hello Azure CLI 1.0](../../cli-install-nodejs.md) jelentkezett be, és a Resource Manager módot használ:

```azurecli
azure config mode arm
```

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `myimages`.

Először hozzon létre egy erőforráscsoportot. hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `WestUs` helye:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

A tárolási fiók toohold a virtuális lemezeket hozhat létre. hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

A tárfiók hello elérési kulcsainak listázása. Jegyezze fel a `key1`:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Hozzon létre egy tárolót a beszerzett hello biztonságitár-kulcs használatával tárfiókon belül. hello alábbi példa létrehoz egy nevű tárolót `myimages` hello tárolási értékének használatával `key1`:

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Végül töltse fel a létrehozott VHD-toohello tároló. Adja meg a hello helyi elérési út tooyour virtuális merevlemez alapján `/path/to/disk/mydisk.vhd`:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Mostantól létrehozhat egy virtuális Gépet a feltöltött virtuális lemez [Resource Manager-sablon használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). Használhatja a parancssori felület hello hello URI tooyour lemez megadásával (`--image-urn`). hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` hello korábban feltöltött virtuális lemez segítségével:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

hello céltárfiókot hello ugyanaz, mint ahol a virtuális lemez feltöltött toobe rendelkezik. Továbbá toospecify kell, vagy a vonatkozó választ, minden hello hello szükséges további paraméterek `azure vm create` parancs például a virtuális hálózat, a nyilvános IP-cím, a felhasználónevet és az SSH-kulcsok. További tudnivalók hello [érhető el erőforrás-kezelő parancssori paraméterek](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Követelmények
toocomplete hello lépéseket követve van szüksége:

* **Linux operációs rendszer van telepítve, a .vhd-fájllá** -telepíteni egy [Azure által támogatott Linux-disztribúció](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa hello VHD lévő virtuális lemezek formátumban. Több különféle eszköz toocreate érhetők el a virtuális gép és a virtuális merevlemez:
  * Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve toouse a lemezkép formátumú virtuális Merevlemezt. Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával `qemu-img convert`.
  * Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello újabb VHDX formátum nem támogatott az Azure-ban. Amikor létrehoz egy virtuális Gépet, adja meg a virtuális merevlemez hello formátumban. Ha szükséges, VHDX-lemezek tooVHD használatával konvertálhatja [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot. További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért meg kell tooconvert ilyen lemezek toostatic VHD-k feltöltés előtt. Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert a dinamikus lemezek a hello folyamat tooAzure feltöltése során.
> 
> 

* Az egyéni lemezkép alapján létrehozott virtuális gépek kell lennie, hello azonos tárfiók maga hello képként
  * Hozzon létre egy tárolási fiók és a tároló toohold, az egyéni lemezképet és a létrehozott virtuális gépek
  * Miután létrehozta a virtuális gépek, nyugodtan törölheti a kép

Győződjön meg arról, hogy rendelkezik [hello Azure CLI 1.0](../../cli-install-nodejs.md) jelentkezett be, és a Resource Manager módot használ:

```azurecli
azure config mode arm
```

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `myimages`.

<a id="prepimage"> </a>

## <a name="prepare-hello-image-toobe-uploaded"></a>Készítse elő a hello kép toobe feltöltve.
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


## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
Erőforráscsoportok logikailag összefogni összes hello Azure-erőforrások toosupport a virtuális gépek, például a hello virtuális hálózati és tárolási. Tudjon meg többet az [Azure erőforráscsoportokat Itt](../../azure-resource-manager/resource-group-overview.md). Feltöltése az egyéni lemezképet, és a virtuális gépek létrehozását, mielőtt először toocreate egy erőforráscsoportot. 

hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `WestUS` helye:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Create a storage account
Virtuális gépek lapblobokat egy tárfiókon belül tárolódnak. Tudjon meg többet az [itt az Azure blob storage](../../storage/common/storage-introduction.md#blob-storage). Az egyéni lemezképet, és a virtuális gépek storage-fiók létrehozása. A virtuális gépeket hoz létre az egyéni lemez lemezképet kell toobe a hello ugyanazt a tárfiókot, mint a lemezkép.

hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount` hello erőforráscsoportban korábban hozott létre:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Tárfiókkulcsok listázása
Az Azure létrehoz két 512 bites kulcsot minden tárfiók. A hozzáférési kulcsot használnak az toohello tárfiókot, például az írási műveletek toocarry hitelesítésekor. Tudjon meg többet az [kezelése hozzáférési toostorage Itt](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Hello a hívóbetűk megtekintéséhez `azure storage account keys list` parancsot.

Hello elérési kulcsainak megtekintése létrehozott tárfiók hello:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
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
A hello azonos módon, hogy hozzon létre különböző könyvtárak toologically rendezheti a helyi fájlrendszer, a virtuális lemezek és a képek hozzon létre egy tárolási fiók tooorganize tárolókra. A storage-fiók korlátlan számú tárolót tárolhat tartalmazhat. 

hello alábbi példa létrehoz egy nevű tárolót `myimages`, hello előző lépésben kapott hello hozzáférési kulcs megadása (`key1`):

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Virtuális merevlemez feltöltéséhez
Most ténylegesen feltöltheti az egyéni lemezképet. A virtuális gép által használt összes virtuális lemezek, töltse fel, és tárolja az egyéni lemezképet oldalblobként.

Adja meg a hozzáférési kulcsot a hello tároló hello előző lépésben létrehozott, és ezután hello az elérési út toohello egyéni lemezképet a helyi számítógépen:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Egyéni lemezképet a virtuális gép létrehozása
Amikor virtuális gépeket hoz létre az egyéni lemezképet, adjon meg hello URI toohello lemezképet. Gondoskodjon arról, hogy hello rendeltetési tárolási fiók megfelel az egyéni lemezképet tároló. A virtuális gép hello Azure CLI vagy erőforrás-kezelő JSON-sablon használatával hozhat létre.

### <a name="create-a-vm-using-hello-azure-cli"></a>Hozzon létre egy virtuális Gépet hello Azure parancssori felület használatával
Megadja a hello `--image-urn` hello paraméterrel `azure vm create` parancs toopoint tooyour egyéni lemezképet. Győződjön meg arról, hogy `--storage-account-name` egyezések hello tárfiók az egyéni lemezképet tárolja. Nincs toouse hello ugyanabban a tárolóban, egyéni lemez kép toostore hello a virtuális gépek. Győződjön meg arról, hogy toocreate további tárolókkal hello azonos módon hello korábbi lépéseket az egyéni lemezképek feltöltés előtt.

hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` származó az egyéni lemezképet:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Ön továbbra is szükséges toospecify, vagy a választ kér, összes hello hello szükséges további paraméterek `azure vm create` parancs például a virtuális hálózat, a nyilvános IP-cím, a felhasználónevet és az SSH-kulcsok. További információk a hello [érhető el erőforrás-kezelő parancssori paraméterek](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>A virtuális gép egy JSON-sablon létrehozása
Az Azure Resource Manager-sablonok a JavaScript Object Notation (JSON) fájlok hello környezet toobuild kívánja. hello sablonok az erőforrás-szolgáltatók toodifferent például számítási és hálózati részletezik. Meglévő sablonok használhatja, vagy a saját írni. Tudjon meg többet az [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).

Hello belül `Microsoft.Compute/virtualMachines` a sablon-szolgáltatót, hogy egy `storageProfile` hello konfigurációs adatait a virtuális gép tartalmazó csomópontot. hello két fő paraméterek tooedit hello `image` és `vhd` URI-azonosítók, amelyek az új virtuális gép virtuális lemez és tooyour egyéni lemezképet. hello következő használatához az egyéni lemezképet hello JSON példáját mutatja be:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Használhat [a meglévő sablon toocreate egy virtuális Gépet egy egyéni lemezképből](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) vagy foglalkozó témakört [a saját Azure Resource Manager-sablonok készítése](../../azure-resource-manager/resource-group-authoring-templates.md). 

Ha már konfigurált egy sablon, hoz létre a virtuális gépek hello `azure group deployment create` parancsot. Adja meg a JSON-sablon URI hello hello `--template-uri` paraméter:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Ha egy számítógépen helyben tárolt JSON-fájl, használhatja a hello `--template-file` paraméter helyette:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Következő lépések
Miután előkészített és feltöltése az egyéni virtuális lemez, akkor további információ [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md). Akkor is érdemes lehet túl[hozzá adatlemezt](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour új virtuális gépeket. Ha a virtuális gépeken futó, hogy kell-e tooaccess alkalmazásai, ne feledje túl[nyisson meg portokat és a végpontok](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

