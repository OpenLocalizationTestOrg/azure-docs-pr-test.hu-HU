---
title: "aaaCreate és feltöltése az OpenBSD virtuális gép kép tooAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate és feltöltése egy virtuális merevlemez (VHD) tartalmazó hello OpenBSD operációs rendszer toocreate egy Azure virtuális gépet az Azure parancssori felület használatával"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
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
ms.author: kyliel
ms.openlocfilehash: 0524f45ea1ecec37e55cbe9d54438a571a831de7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-an-openbsd-disk-image-tooazure"></a>Hozzon létre, és töltse fel az OpenBSD lemez kép tooAzure
Ez a cikk bemutatja, hogyan toocreate és feltöltése egy virtuális merevlemez (VHD) tartalmazó hello OpenBSD operációs rendszer. Után a feltöltés, mint a saját kép toocreate egy virtuális gép (VM) az Azure parancssori felületen keresztül Azure-ban használhatja.


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik-e a következő elemek hello:

* **Azure-előfizetés** – Ha nincs fiókja, létrehozhat egyet, néhány perc alatt. Ha MSDN-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Ellenkező esetben megtudhatja, hogyan túl[hozzon létre egy próbafiókot](https://azure.microsoft.com/pricing/free-trial/).  
* **Az Azure CLI 2.0** -legyen hello legújabb [Azure CLI 2.0](/cli/azure/install-azure-cli) telepítve, és az Azure-fiók tooyour bejelentkezve [az bejelentkezési](/cli/azure/#login).
* **OpenBSD operációs rendszer van telepítve, a .vhd-fájllá** -OpenBSD támogatott operációs rendszer (6.1-es verzió) telepített tooa virtuális merevlemezt kell lennie. Több különféle eszköz toocreate .vhd fájlok léteznek. Például egy virtualizálási megoldást használja például a Hyper-V toocreate hello .vhd fájl, és hello operációs rendszer telepítéséhez. Leírja, hogyan tooinstall és használja a Hyper-V, tekintse meg a [telepítése Hyper-V virtuális gép létrehozása és](http://technet.microsoft.com/library/hh846766.aspx).


## <a name="prepare-openbsd-image-for-azure"></a>Az Azure-OpenBSD lemezkép előkészítése
Hello VM hello OpenBSD operációs rendszer 6.1, amely a Hyper-V támogatása, amelyen a hajtsa végre a következő eljárások hello:

1. Ha a DHCP telepítése során nem engedélyezett, engedélyezze a hello szolgáltatást az alábbiak szerint:

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
   
4. Alapértelmezés szerint hello `root` felhasználó le van tiltva, az Azure virtuális gépeken. Felhasználók futtathatják a parancsokat emelt szintű jogosultságokkal hello segítségével `doas` OpenBSD VM parancsot. Doas alapértelmezés szerint engedélyezve van. További információkért lásd: [doas.conf](http://man.openbsd.org/doas.conf.5). 

5. Telepítse és konfigurálja az alábbiak szerint hello Azure ügynök előfeltételei:

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. hello Azure ügynök legújabb kiadása hello mindig található [Github](https://github.com/Azure/WALinuxAgent/releases). Hello ügynök telepítése az alábbiak szerint:

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > Azure-ügynök telepítése után egy jó ötlet tooverify, hogy fut-e az alábbiak szerint:
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. Deprovision hello rendszer tooclean, és ellenőrizze, megfelelő-e reprovisioning. hello következő parancs is törli hello utolsó kiosztott felhasználói fiókkal és a kapcsolódó hello adatokat:

    ```sh
    waagent -deprovision+user -force
    ```

Most, állítsa le a virtuális Gépet.


## <a name="prepare-hello-vhd"></a>Hello virtuális merevlemez előkészítése
hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**. Átalakítás hello toofixed VHD formátummal Hyper-V kezelőjével vagy a hello Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) parancsmag. Példa van, mint következő.

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a>Tároló-erőforrások létrehozása és feltöltése
Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

tooupload a VHD-t, hozzon létre egy tárfiókot a [az storage-fiók létrehozása](/cli/azure/storage/account#create). Tárfiókneveket kell egyedinek lennie, ezért adja meg saját nevét. hello alábbi példa létrehoz egy nevű tárfiók *mystorageaccount*:

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

toocontrol toohello tárfiók eléréséhez, hello biztonságitár-kulcs az beszerzése [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list) az alábbiak szerint:

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

toologically külön hello tölt fel, virtuális merevlemezek létrehozni egy tárolót a hello tárfiókon belül [az tároló létrehozása](/cli/azure/storage/container#create):

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
Virtuális gép és hozhat létre egy [mintaparancsfájl](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) vagy közvetlenül az [az virtuális gép létrehozása](/cli/azure/vm#create). toospecify hello OpenBSD VHD feltöltött, használjon hello `--image` paraméter az alábbiak szerint:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Hello IP-cím beszerzése a OpenBSD tulajdonsággal rendelkező virtuális gépet [az vm-ip-címeinek listáját](/cli/azure/vm#list-ip-addresses) az alábbiak szerint:

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

Most SSH tooyour OpenBSD VM szokásos módon teheti:
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a>Következő lépések
Ha azt szeretné, hogy további információk a Hyper-V támogatja a OpenBSD6.1 tooknow, olvassa el [OpenBSD 6.1](https://www.openbsd.org/61.html) és [hyperv.4](http://man.openbsd.org/hyperv.4).

Ha azt szeretné, hogy a felügyelt lemezes virtuális gép toocreate, olvassa el [az lemez](/cli/azure/disk). 
