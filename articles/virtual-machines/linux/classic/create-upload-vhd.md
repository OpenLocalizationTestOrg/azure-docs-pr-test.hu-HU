---
title: "aaaCreate, és töltse fel a Linux virtuális merevlemez tooAzure |} Microsoft Docs"
description: "Létrehozása és feltöltése az Azure virtuális merevlemez (VHD), amely tartalmazza a hello Linux operációs rendszert hello klasszikus telepítési modell segítségével"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8058ff98-db03-4309-9bf4-69842bd64dd4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: iainfou
ms.openlocfilehash: 77b01316386c4a6eb68c129fa68d42f0a8996edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-hello-linux-operating-system"></a>Létrehozásával, majd ismét feltölteni a virtuális merevlemez a Contains hello Linux operációs rendszer
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Emellett [feltöltése az Azure Resource Manager használatával egyéni lemezképet](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ez a cikk bemutatja, hogyan toocreate és a virtuális merevlemez (VHD), mint a saját használhassa feltöltése kép toocreate virtuális gépek Azure-ban. Ismerje meg, hogyan tooprepare hello operációs rendszer, hogy használhassa az toocreate több virtuális gép alapján, hogy a lemezképet. 


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik-e a következő elemek hello:

* **Linux operációs rendszer van telepítve, a .vhd-fájllá** -telepítette egy [Azure által támogatott Linux-disztribúció](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuális lemez hello VHD-formátumban. Több különféle eszköz toocreate érhetők el a virtuális gép és a virtuális merevlemez:
  * Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve toouse a lemezkép formátumú virtuális Merevlemezt. Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával `qemu-img convert`.
  * Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello újabb VHDX formátum nem támogatott az Azure-ban. Amikor létrehoz egy virtuális Gépet, adja meg a virtuális merevlemez hello formátumban. Ha szükséges, VHDX-lemezek tooVHD használatával konvertálhatja [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot. További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért meg kell tooconvert ilyen lemezek toostatic VHD-k feltöltés előtt. Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert a dinamikus lemezek a hello folyamat tooAzure feltöltése során.

* **Azure parancssori felület** -telepítés hello legújabb [Azure parancssori felület](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooupload hello VHD-t.

<a id="prepimage"> </a>

## <a name="step-1-prepare-hello-image-toobe-uploaded"></a>1. lépés: Felkészülés hello kép toobe feltöltve.
Azure támogatja a különböző Linux terjesztésekről (lásd: [támogatott Disztribúciókkal](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). a következő cikkek hello végigvezetik hogyan tooprepare hello Azure által támogatott különböző Linux-disztribúció. Miután elvégezte a következő útmutatók hello hello lépéseit, lépjen vissza ide, ha már van egy VHD-fájlt, amely készen áll a tooupload tooAzure:

* **[CentOS-alapú Disztribúciók](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Egyéb - nem támogatott Disztribúciókkal](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

> [!NOTE]
> hello Azure platformon SLA vonatkozik a Linux operációs rendszert futtató csak akkor, ha egy hello által támogatott disztribúciók használt hello konfigurációs adatait a támogatott verziók hello működő toovirtual gépek [Azure-Endorsed Terjesztéseket Linux ](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Az összes Linux terjesztésekről hello kép: Azure-katalógus a szükséges konfigurációval hello hitelesített terjesztéseket.
> 
> 

Lásd még: hello  **[Linux telepítési jegyzetek](../create-upload-generic.md#general-linux-installation-notes)**  kapcsolatos további általános tippek Linux lemezképek előkészítése az Azure.

<a id="connect"> </a>

## <a name="step-2-prepare-hello-connection-tooazure"></a>2. lépés: Hello kapcsolat tooAzure előkészítése
Ellenőrizze, hogy hello Azure CLI hello klasszikus üzembe helyezési modellel használ (`azure config mode asm`), majd jelentkezzen be tooyour fiók:

```azurecli
azure login
```


<a id="upload"> </a>

## <a name="step-3-upload-hello-image-tooazure"></a>3. lépés: Hello kép tooAzure feltöltése
A tárolási fiók tooupload kell a virtuális merevlemezt. Vagy válassza ki meglévő tárfiók vagy [hozzon létre egy újat](../../../storage/common/storage-create-storage-account.md).

Hello Azure CLI tooupload hello kép használata hello a következő parancs használatával:

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Az előző példában hello:

* **BlobStorageURL** toouse tervezi hello tárfiók hello URL-címe
* **YourImagesFolder** a blob storage tárolóban hello ahol van toostore a lemezképbe
* **VHDName** hello címke sem, hogy megjelenik-portál tooidentify hello virtuális merevlemezt.
* **PathToVHDFile** hello teljes elérési útja és neve hello .vhd fájlt a számítógépre.

a következő parancs hello egy teljes példát mutat be:

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-hello-image"></a>4. lépés: Virtuális gép létrehozása hello lemezképből
A virtuális gép használata `azure vm create` a hello ugyanúgy, ahogy a normál virtuális gépek. Adja meg a lemezkép hello előző lépésben megadott hello névnek. A következő példa hello, használjuk hello **myImage** hello előző lépésben megadott lemezkép neve:

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

toocreate a saját virtuális gépeket, adja meg a saját felhasználónév + a jelszó, a hely, a DNS-név és a kép neve.

## <a name="next-steps"></a>Következő lépések
További információkért lásd: [hello Azure klasszikus telepítési modell Azure parancssori felület referenciája](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

[Step 1: Prepare hello image toobe uploaded]:#prepimage
[Step 2: Prepare hello connection tooAzure]:#connect
[Step 3: Upload hello image tooAzure]:#upload
