---
title: "Egy Azure virtuális gép átalakítása egy méretezési |} Microsoft Docs"
description: "Létrehozhat és telepíthet a Linux Azure virtuálisgép-méretezési beállítása az Azure parancssori felület segítségével."
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
ms.openlocfilehash: 8d3376d2791b1349298db618d475ce5573083702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="convert-an-existing-azure-virtual-machine-to-a-scale-set"></a>Meglévő Azure virtuális gép átalakítása egy méretezési csoport

Az oktatóanyag bemutatja, hogyan használható az Azure CLI 2.0 egy virtuálisgép-méretezési csoport egy virtuális gép átalakítása. Is megismerheti, hogyan automatizálható a méretezési csoportban lévő virtuális gépek konfigurációját. Azure CLI 2.0 telepítéséről további információkért lásd: [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md). Méretezési csoportok kapcsolatos további információkért lásd: [virtuálisgép-skálázási készletekben](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

## <a name="step-1---deprovision-the-vm"></a>1. lépés – a virtuális gép kiosztásának megszüntetése

Az SSH használata a virtuális Géphez való kapcsolódáshoz.

Kiosztásának megszüntetése a virtuális Gépet az Azure Virtuálisgép-ügynök használatával törli a fájlokat és adatokat. A megszüntetés részletes áttekintése, lásd: [Linux virtuális gép rögzítése](capture-image.md).

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-the-vm"></a>2. lépés – a virtuális gép lemezképének rögzítése

Részletes megtudhatja, hogy a, [Linux virtuális gép rögzítése](capture-image.md).

A virtuális Géphez a felszabadítani [az virtuális gép felszabadítása](/cli/azure/vm#deallocate):

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

A virtuális Géphez a generalize [az vm generalize](/cli/azure/vm#generalize):

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

Kép készítése a VM erőforrás [az lemezkép létrehozása](/cli/azure/image#create):

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-the-scale-set"></a>3. lépés - a méretezési készlet létrehozása

Beolvasása a **azonosító** kép.

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

Ez a parancs is 10 GB-os adatlemezt csatolni. Ne feledje, hogy attól függően, hogy a virtuális Géphez kiválasztott méret (használtuk **Standard_DS1_v2**), az adatlemezek száma az engedélyezett különböző. További információkért tekintse át a [virtuálisgép-méretek](sizes.md).

Ha a skála befejeződik, kapcsolódni hozzá. A példányok IP-címek listájának lekérése az SSH [az vmss lista--kapcsolat-példányadatait](/cli/azure/vmss#list-instance-connection-info):

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

Most csatlakozhat a virtuálisgép-példányt a adatlemez inicializálása

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-the-data-disk"></a>4. lépés - az adatok lemez inicializálása

Ha a virtuális gép csatlakozik, a lemez partícióazonosító `fdisk`:

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

A fájlrendszer írni a partíció a `mkfs` parancs:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Csatlakoztassa az új lemezt, úgy, hogy az operációs rendszerben érhető el:

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

A lemez is lehet a datadrive csatlakozási pont, amely ellenőrizhető keresztül fér hozzá `ls /datadrive/`.

Az SSH-munkamenet befejezéséhez.


## <a name="step-5---configure-firewall"></a>5. lépés - a tűzfal konfigurálása

Lyukasztás lyuk a webkiszolgálónak, a méretezési készlet által üzemeltetett a tűzfalon keresztül. A méretezési csoportban hozták létre, egy terhelés-kiegyenlítő is hozták létre, és használta **SSH** az egyes virtuális gépekhez. Nyisson meg egy portot, kétféle információt, amely akkor kell Azure parancssori felület használatával.

* **Előtérbeli IP-címkészlet**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* **Háttérbeli IP-címkészlet**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

E két nevekkel, nyissa meg port **80**.

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a>6. lépés - automatizálásához

Az adatok lemezre kell megadni minden egyes virtuálisgép-példányon. Azt automatizálhatja a virtuális gép konfigurációját a **CustomScript** bővítmény.

Először hozzon létre egy *.sh* parancsfájlt, ami a lemez formátum parancsokat tartalmazza.

```sh
#!/bin/bash

# Setup the data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

Ezt követően töltse fel, hogy a parancsfájl arra, ahol a **CustomScript** bővítmény-e hozzáférési engedélye. Egy példány érhető [Itt](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).

Hozzon létre egy helyi fájlt **settings.json** és a következő JSON-blokk be. A `flieUris` tulajdonságot kell beállítani, ahol a parancsfájl feltöltöttnek való.

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

Telepítse ezt a parancsot a skála állítható be a **CustomScript** bővítmény hivatkozik a **settings.json** imént létrehozott fájlt.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

Aktuális példányainak, valamint a később létrehozott skálázással példányok automatikusan futtatja ezt a bővítményt.

## <a name="step-7---configure-autoscale-rules"></a>7. lépés – az automatikus skálázási szabályok konfigurálása

Automatikus skálázási szabályok jelenleg az Azure parancssori felület nem állítható be. Használja a [Azure-portálon](https://portal.azure.com) automatikus skálázás konfigurálása.

## <a name="step-8---management-tasks"></a>8 - felügyeleti feladatokat. lépés

A méretezési életciklusa során szükség lehet egy vagy több felügyeleti feladatok futtatásához. Emellett érdemes lehet különböző életciklus-feladatokat automatizáló parancsfájlokat hozhatnak létre, és az Azure parancssori felület e feladatok elvégzéséhez gyors lehetőséget kínál. Az alábbiakban néhány gyakori feladatot.

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
Ebben az oktatóanyagban bevezetett virtuálisgép-méretezési készlet szolgáltatások némelyike kapcsolatos további tudnivalókért tekintse meg a következő információkat:

- [Az Azure virtuálisgép-méretezési csoportok áttekintése](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [Az Azure Load Balancer áttekintése](../../load-balancer/load-balancer-overview.md)
- [A hálózati biztonsági csoportokkal hálózati adatforgalom szabályozásához](../../virtual-network/virtual-networks-nsg.md)