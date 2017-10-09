---
title: "a Linux virtuális Gépet az Azure CLI 1.0 hello másolatát aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy-egy példányát az Azure Linux virtuális gép hello hello Resource Manager üzembe helyezési modellel Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 997a2c8109e7083ececd76fd1013e9ed4d3e6afd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-hello-azure-cli-10"></a>Az Azure CLI 1.0 hello Azure-on futó Linux virtuális gépek másolatának létrehozása
Ez a cikk bemutatja, hogyan toocreate egy példányát az Azure virtuális gép (VM) futó Linux használt hello Resource Manager üzembe helyezési modellben. Először másolja át a hello operációs rendszer és az adatok lemezek tooa új tárolót, majd beállítása hello hálózati erőforrások és hello új virtuális gép létrehozása.

Emellett [töltse fel, és hozzon létre egy virtuális Gépet az egyéni lemezképet](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- Az Azure CLI 1.0 – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="before-you-begin"></a>Előkészületek
Győződjön meg arról, hogy teljesülnek-e hello hello lépések megkezdése előtt a következő előfeltételek:

* Hello rendelkezik [Azure CLI](../../cli-install-nodejs.md) letöltötte és telepítette a számítógépen. 
* A meglévő Azure Linux virtuális Gépre vonatkozó kell néhány információt is:

| Forrás Virtuálisgép-adatok | Ha tooget azt |
| --- | --- |
| a virtuális gép neve |`azure vm list` |
| Erőforráscsoport neve |`azure vm list` |
| Hely |`azure vm list` |
| A tárfiók neve |`azure storage account list -g <resourceGroup>` |
| Tárolónév |`azure storage container list -a <sourcestorageaccountname>` |
| Forrásfájl virtuális gép virtuális merevlemez neve |`azure storage blob list --container <containerName>` |

* Az új virtuális gép kapcsolatos toomake néhány lesz szüksége:   <br> -Tároló neve   <br> Virtuálisgép - nevet   <br> Virtuálisgép - méret   <br> -vNet neve   <br> -Alhálózat neve   <br> -IP név   <br> -Hálózati név

## <a name="login-and-set-your-subscription"></a>Bejelentkezés és az előfizetés beállítása
1. Bejelentkezési toohello CLI-t.

    ```azurecli
    azure login
    ```
2. Győződjön meg arról, hogy az erőforrás-kezelő módban van.

    ```azurecli
    azure config mode arm
    ```
3. Állítsa be a megfelelő előfizetés hello. Használhatja az "azure-fiók list" toosee összes előfizetését.

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-hello-vm"></a>Hello VM leállítása
Állítsa le és hello forrás virtuális gép felszabadítása. Az előfizetés és az erőforráscsoport-nevek a "list azure virtuális gép" tooget összes hello virtuális gépek listáját is használhatja.

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-hello-vhd"></a>Hello virtuális merevlemez másolása
Virtuális merevlemez hello átmásolhatja hello forrás toohello célhelyet hello használata `azure storage blob copy start`. Ebben a példában a toocopy hello VHD toohello fogjuk ugyanazt a tárfiókot, de egy másik tárolóban.

toocopy hello VHD tooanother tárolóhoz hello ugyanazt a tárfiókot, írja be:

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-hello-virtual-network-for-your-new-vm"></a>Az új virtuális gép virtuális hálózati hello beállítása
A virtuális hálózat és a hálózati adapter beállítása az új virtuális gép. 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-hello-new-vm"></a>Hozzon létre új virtuális gép hello
Mostantól létrehozhat egy virtuális Gépet a feltöltött virtuális lemez [resource manager-sablon használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) vagy keresztül hello hello URI tooyour megadásával CLI lemezre másolt beírásával:

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a>Következő lépések
toolearn hogyan toouse Azure CLI toomanage a új virtuális gépet, lásd: [hello Azure Resource Manager az Azure parancssori felület parancsait](../azure-cli-arm-commands.md).

