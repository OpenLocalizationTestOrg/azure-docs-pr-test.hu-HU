---
title: "Hozzon létre és OpenBSD VM-lemezkép feltöltése az Azure-bA |} Microsoft Docs"
description: "Ismerje meg, hogyan hozhat létre, és töltse fel a virtuális merevlemez (VHD), amely tartalmazza az operációs rendszer OpenBSD az Azure parancssori felületen keresztül Azure virtuális gép létrehozása"
services: virtual-machines-linux
documentationcenter: 
author: thomas1206
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: huishao
ms.openlocfilehash: 9b4163471f3dc8483993b9ac762694af4e926aa0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-upload-an-openbsd-disk-image-to-azure"></a>Hozzon létre és OpenBSD lemez lemezkép feltöltése az Azure-bA
Ez a cikk bemutatja, hogyan hozhat létre, és töltse fel a virtuális merevlemez (VHD), amely tartalmazza a OpenBSD operációs rendszer. Miután a feltöltés, segítségével azt saját képként (VM) virtuális gép létrehozása az Azure parancssori felületen keresztül Azure-ban.


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik-e a következő elemek:

* **Azure-előfizetés** – Ha nincs fiókja, létrehozhat egyet, néhány perc alatt. Ha MSDN-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Ellenkező esetben megtudhatja, hogyan [hozzon létre egy próbafiókot](https://azure.microsoft.com/pricing/free-trial/).  
* **Az Azure CLI 2.0** -győződjön meg arról, hogy a legújabb [Azure CLI 2.0](/cli/azure/install-azure-cli) telepítve, és az Azure-fiókja bejelentkezett [az bejelentkezési](/cli/azure/#login).
* **OpenBSD operációs rendszer van telepítve, a .vhd-fájllá** -OpenBSD támogatott operációs rendszer (6.1-es verzió) telepítenie kell egy virtuális merevlemezt. Több különféle eszköz található .vhd fájlok létrehozásához. Például a Hyper-V hálózatvirtualizálási megoldás segítségével például a .vhd fájlt, és az operációs rendszer telepítése. Leírja, hogyan kell telepíteni és használni a Hyper-V, lásd: [telepítése Hyper-V virtuális gép létrehozása és](http://technet.microsoft.com/library/hh846766.aspx).


## <a name="prepare-openbsd-image-for-azure"></a>Az Azure-OpenBSD lemezkép előkészítése
A virtuális gépen, amelyre telepítette a OpenBSD operációs rendszer 6.1, amely hozzá a Hyper-V támogatásához, hajtsa végre az alábbi eljárásokat:

1. Ha a DHCP telepítése során nem engedélyezett, engedélyezze a szolgáltatást az alábbiak szerint:

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. Az alábbiak szerint állíthatja be a soros konzol:

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. Konfigurálja a csomag telepítése az alábbiak szerint:

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. Alapértelmezés szerint a `root` felhasználó le van tiltva, az Azure virtuális gépeken. Felhasználók futtathatók parancsokat emelt szintű jogosultságokkal a `doas` OpenBSD VM parancsot. Doas alapértelmezés szerint engedélyezve van. További információkért lásd: [doas.conf](http://man.openbsd.org/doas.conf.5). 

5. Telepítse és konfigurálja az alábbiak szerint az Azure Agent ügynököt előfeltételei:

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. Az Azure-ügynök legújabb kiadásának mindig található [Github](https://github.com/Azure/WALinuxAgent/releases). Az ügynök telepítése az alábbiak szerint:

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > Azure-ügynök telepítése után célszerű ellenőrizni, hogy fut-e az alábbiak szerint:
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. A rendszer megtisztítsa tőle, és lehetővé teszi a megfelelő reprovisioning kiosztásának megszüntetése. A következő parancs is törli, az utolsó kiépített felhasználói fiókot és a kapcsolódó adatokat:

    ```sh
    waagent -deprovision+user -force
    ```

Most, állítsa le a virtuális Gépet.


## <a name="prepare-the-vhd"></a>A virtuális merevlemez előkészítése
A VHDX formátum nem támogatott az Azure csak **rögzített VHD**. A lemez átalakítása Hyper-V kezelőjével vagy a Powershell rögzített VHD formátumú [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) parancsmag. Példa van, mint következő.

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a>Tároló-erőforrások létrehozása és feltöltése
Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a a *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

A storage-fiók létrehozása a virtuális merevlemez feltöltéséhez, [az storage-fiók létrehozása](/cli/azure/storage/account#create). Tárfiókneveket kell egyedinek lennie, ezért adja meg saját nevét. Az alábbi példa létrehoz egy nevű tárfiók *mystorageaccount*:

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

A tárfiók való hozzáférést, szerezze be a kulcsot a [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list) az alábbiak szerint:

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

Logikailag külön a VHD-be feltölteni, hozzon létre egy tárfiók tárolójában [az tároló létrehozása](/cli/azure/storage/container#create):

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

Végül, töltse fel a virtuális merevlemez [az tárolási blob feltöltése](/cli/azure/storage/blob#upload) az alábbiak szerint:

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a>A virtuális Merevlemezt a virtuális gép létrehozása
Virtuális gép és hozhat létre egy [mintaparancsfájl](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) vagy közvetlenül az [az virtuális gép létrehozása](/cli/azure/vm#create). A feltöltött OpenBSD VHD megadásához használja a `--image` paraméter az alábbiak szerint:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Az IP-cím beszerzése a OpenBSD tulajdonsággal rendelkező virtuális gépet [az vm-ip-címeinek listáját](/cli/azure/vm#list-ip-addresses) az alábbiak szerint:

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

SSH most a szokásos módon OpenBSD VM is:
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a>Következő lépések
Ha azt szeretné, további információkat a Hyper-V-támogatást a OpenBSD6.1, olvassa el a [OpenBSD 6.1](https://www.openbsd.org/61.html) és [hyperv.4](http://man.openbsd.org/hyperv.4).

Ha azt szeretné, a felügyelt lemezes virtuális gép létrehozása, olvasása [az lemez](/cli/azure/disk). 
