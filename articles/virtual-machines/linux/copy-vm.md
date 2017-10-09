---
title: "a Linux virtuális gépek Azure CLI 2.0 használatával aaaCopy |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy példányát az Azure Linux virtuális gép Azure CLI 2.0 és a felügyelt lemezek használatával."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/10/2017
ms.author: cynthn
ms.openlocfilehash: ee34a4259dd0c1e7bf49312fe3fe3ba809bf69e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a>Hozzon létre egy Linux virtuális gép egy másolatát Azure CLI 2.0 és felügyelt lemezek


Ez a cikk bemutatja, hogyan toocreate egy példányát az Azure virtuális gép (VM) Linux használó futtatása hello Azure CLI 2.0 és hello Azure Resource Manager üzembe helyezési modellben. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Emellett [töltse fel, és hozzon létre egy virtuális gép olyan virtuális merevlemezről](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Előfeltételek


-   Telepítés [Azure CLI 2.0](/cli/azure/install-az-cli2)

-   Az Azure-fiók tooan bejelentkezés [az bejelentkezési](/cli/azure/#login).

-   Rendelkezik egy Azure virtuális gép toouse hello a másolási forrásaként.

## <a name="step-1-stop-hello-source-vm"></a>1. lépés: Hello forrás virtuális gép leállítása


Hello forrás virtuális gép felszabadítása használatával [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).
hello alábbi példa felszabadítja a hello nevű virtuális gép **myVM** hello erőforráscsoportban **myResourceGroup**:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-hello-source-vm"></a>2. lépés: Hello forrás virtuális gép másolása


toocopy egy virtuális Gépet, akkor hello alapul szolgáló virtuális merevlemez másolatának létrehozása. A folyamat hello tartalmazó felügyelt lemezként speciális virtuális Merevlemezt hoz létre a forrás virtuális gép azonos konfigurációs és hello beállításokat.

További információ az Azure Managed Disksről: [Azure Managed Disks – áttekintés](../windows/managed-disks-overview.md). 

1.  Minden virtuális gép és hello nevét az operációsrendszer-lemez, a listában [az vm lista](/cli/azure/vm#list). hello alábbi példa felsorolja az erőforráscsoport nevű virtuális gépeinek **myResourceGroup**:
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    hello hasonló toohello a következő példa a kimenetre:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  Hozzon létre egy új hello lemezmásolás lemez használatával kezelhetők [az lemez létrehozása](/cli/azure/disk#create). hello alábbi példa létrehoz egy nevű lemez **myCopiedDisk** hello a felügyelt nevű lemez **myDisk**:

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  Felügyelt hello lemezek most az erőforráscsoportban ellenőrizheti [az Lemezlista](/cli/azure/disk#list). hello alábbi példa felsorolja felügyelt hello lemezek nevű hello erőforráscsoportban **myResourceGroup**:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  Hagyja ki túl["3. lépés: virtuális hálózat beállítása"](#step-3-set-up-a-virtual-network).


## <a name="step-3-set-up-a-virtual-network"></a>3. lépés: Virtuális hálózat beállítása


hello következő lépések hozzon létre egy új virtuális hálózat, az alhálózat, a nyilvános IP-cím és a virtuális hálózati kártya (NIC).

A virtuális gép másolása hibaelhárítási célokra, vagy további központi telepítéseket, nem biztos toouse egy virtuális gép egy meglévő virtuális hálózatban.

Ha a másolt virtuális gépek toocreate egy virtuális hálózati infrastruktúra, kövesse tovább hello néhány lépést. Ha nem szeretné, toocreate egy virtuális hálózathoz, hagyja ki túl[4. lépés: virtuális gép létrehozása](#step-4-create-a-vm).

1.  Hello virtuális hálózat létrehozása a [az hálózati vnet létrehozása](/cli/azure/network/vnet#create). hello alábbi példa létrehoz egy virtuális hálózatot nevű **myVnet** és nevű alhálózat **mySubnet**:

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  Hozzon létre egy nyilvános IP-cím használatával [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create). hello alábbi példa létrehoz egy nyilvános IP-cím nevű **myPublicIP** hello DNS-névvel, **mypublicdns**. (hello DNS-név kell egyedinek lennie, így adjon meg egy egyedi nevet.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  Hozzon létre hello hálózati adapter által használt [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).
    hello alábbi példa létrehoz egy hálózati adapter nevű **myNic** meg csatolt toothe **mySubnet** alhálózati:

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a>4. lépés: Virtuális gép létrehozása

Mostantól létrehozhat egy virtuális gép használatával [az virtuális gép létrehozása](/cli/azure/vm#create).

Adja meg a hello másolt felügyelt lemezes toouse hello az operációs rendszer lemezeként (--csatolása operációsrendszer-lemez), az alábbiak szerint:

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Következő lépések

toolearn hogyan toouse Azure CLI toomanage az új virtuális gép, lásd: [hello Azure Resource Manager az Azure parancssori felület parancsait](../azure-cli-arm-commands.md).
