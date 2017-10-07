---
title: "aaaConvert egy Azure virtuális gép tooa méretezési |} Microsoft Docs"
description: "Létrehozhat és telepíthet a Linux Azure virtuálisgép-méretezési hello Azure CLI állítható be."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 04/05/2017
ms.author: adegeo
ms.openlocfilehash: e228282ac4855cef589b8500e74e9d461f9aed84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-an-existing-azure-virtual-machine-tooa-scale-set"></a>Alakítsa át egy meglévő Azure-beli virtuális gép tooa méretezési

Az oktatóanyag bemutatja, hogyan toouse Azure CLI 2.0 tooconvert egy virtuális gép tooa virtuálisgép-méretezési készlet. Azt is megtudhatja, hogyan tooautomate hello konfigurációs hello virtuális gépek méretezési hello beállítása. További információ a hogyan tooinstall 2.0, az Azure CLI: [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md). Méretezési csoportok kapcsolatos további információkért lásd: [virtuálisgép-skálázási készletekben](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

## <a name="step-1---deprovision-hello-vm"></a>1. lépés – hello VM kiosztásának megszüntetése

SSH tooconnect toohello virtuális gép használja.

Virtuális gép deprovision hello segítségével hello Azure virtuális gép ügynök toodelete fájlokat és adatokat. A megszüntetés részletes áttekintése, lásd: [Linux virtuális gép rögzítése](capture-image.md).

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-hello-vm"></a>2. lépés – hello virtuális gép képének rögzítése

Részletes megtudhatja, hogy a, [Linux virtuális gép rögzítése](capture-image.md).

Felszabadítani a virtuális gép és hello [az virtuális gép felszabadítása](/cli/azure/vm#deallocate):

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Generalize tulajdonsággal rendelkező virtuális gépet hello [az vm generalize](/cli/azure/vm#generalize):

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

Kép készítése hello VM erőforrás [az lemezkép létrehozása](/cli/azure/image#create):

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-hello-scale-set"></a>3. lépés – hello méretezési készlet létrehozása

Első hello **azonosító** hello kép.

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

A kép erőforrás a virtuális gép létrehozása [az vmss létrehozása](/cli/azure/vmss#create):

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

Ez a parancs is 10 GB-os adatlemezt csatolni. Ne feledje, hogy attól függően, hogy a virtuális gép hello kiválasztott méret (használtuk **Standard_DS1_v2**), az adatlemezek megengedett hello száma nem egyezik. További információkért tekintse át a hello [virtuálisgép-méretek](sizes.md).

Miután hello méretezési befejeződik, csatlakoztassa a tooit. IP-címek listájának lekérése az SSH hello példányok [az vmss lista--kapcsolat-példányadatait](/cli/azure/vmss#list-instance-connection-info):

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

Most toohello virtuális gép példány tooinitialize hello adatlemezt csatlakoztathat

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-hello-data-disk"></a>4. lépés: hello adatok lemez inicializálása

Csatlakoztatott toohello virtuális gépet, miközben hello lemez particionálása `fdisk`:

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

A fájl toohello rendszerpartíción hello írási `mkfs` parancs:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Csatlakoztassa a hello új lemezt, így hello operációs rendszerben érhető el:

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

hello lemez mostantól lehet ellenőrizni a hello datadrive csatlakozásipont keresztül fér hozzá `ls /datadrive/`.

Záró hello SSH-munkamenetet.


## <a name="step-5---configure-firewall"></a>5. lépés - a tűzfal konfigurálása

Lyukasztás lyuk keresztül hello tűzfal toohello webkiszolgálón üzemeltetett hello méretezési készlet. Hello méretezési csoportban hozták létre, egy terhelés-kiegyenlítő is hozták létre, és használta **SSH** toohello egyedi virtuális gépeket. kétféle információt, amely akkor kell tooopen egy portot, az Azure parancssori felület használatával.

* **Előtérbeli IP-címkészlet**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* **Háttérbeli IP-címkészlet**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

E két nevekkel, nyissa meg port **80**.

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a>6. lépés - automatizálásához

hello adatlemez toobe minden egyes virtuálisgép-példányt konfigurálni kell. A Microsoft automatizálhatja a virtuális gép hello hello hello konfigurálását **CustomScript** bővítmény.

Először hozzon létre egy *.sh* parancsfájlt, ami hello lemez formátum parancsokat tartalmazza.

```sh
#!/bin/bash

# Setup hello data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

Ezt követően töltse fel a parancsfájl fájl toowhere hello **CustomScript** bővítmény-e hozzáférési engedélye. Egy példány érhető [Itt](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).

Hozzon létre egy helyi fájlt **settings.json** és hello JSON blokk azt követően. Hello `flieUris` tulajdonságot kell beállítani, a parancsfájl feltöltöttnek toowhere.

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

A parancs tooyour skála hello beállított telepítése **CustomScript** bővítmény hello hivatkozó **settings.json** imént létrehozott fájl.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

Aktuális példányainak, valamint a később létrehozott skálázással példányok automatikusan futtatja ezt a bővítményt.

## <a name="step-7---configure-autoscale-rules"></a>7. lépés – az automatikus skálázási szabályok konfigurálása

Automatikus skálázási szabályok jelenleg az Azure parancssori felület nem állítható be. Használjon hello [Azure-portálon](https://portal.azure.com) tooconfigure automatikus skálázási.

## <a name="step-8---management-tasks"></a>8 - felügyeleti feladatokat. lépés

Hello méretezési hello életciklusa során szükség lehet a toorun egy vagy több felügyeleti feladatokat. Emellett érdemes lehet különböző életciklus-feladatokat automatizáló parancsfájlokat toocreate, és hello Azure parancssori Felületet biztosít egy gyorsan toodo ezeket a feladatokat. Az alábbiakban néhány gyakori feladatot.

### <a name="get-connection-info"></a>Kapcsolat-adatok beolvasása

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a>Állítsa be a példányszám (manuális méretezési)

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a>Erőforráscsoport törlése

Erőforráscsoport törlésekor a belül található összes erőforrást is törlődnek.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések
További információ az egyes virtuálisgép-méretezési hello beállítása ebben az oktatóanyagban bevezetett szolgáltatások toolearn hello a következő információkat lásd:

- [Az Azure virtuálisgép-méretezési csoportok áttekintése](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [Az Azure Load Balancer áttekintése](../../load-balancer/load-balancer-overview.md)
- [A hálózati biztonsági csoportokkal hálózati adatforgalom szabályozásához](../../virtual-network/virtual-networks-nsg.md)