---
title: "Az Azure parancssori felület 2.0 rendelkező virtuálisgép-méretezési csoportok kezelése |} Microsoft Docs"
description: "Közös Azure CLI 2.0 parancsok futtatásával kezelheti a virtuálisgép-méretezési csoportok, például indítása és leállítása egy példányát, vagy módosítsa a skála kapacitás beállítása."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 6ae05dc8faf950f584806d9b4a3e7e1466ded652
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/23/2017
---
# <a name="manage-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>A virtuálisgép-méretezési beállítása az Azure CLI 2.0 kezelése
A virtuálisgép-méretezési csoport életciklusa során szükség lehet egy vagy több felügyeleti feladatok futtatásához. Emellett érdemes lehet különböző életciklus-feladatokat automatizáló parancsfájlokat hozhatnak létre. Ez a cikk részletezi az egyes közös Azure CLI 2.0 parancsok, amelyek lehetővé teszik, hogy ezeket a műveleteket.

Felügyeleti feladatok elvégzéséhez szüksége van a legújabb Azure CLI 2.0 build. Hogyan kell telepíteni, és a legújabb verzióját használja a további információkért lásd: [az Azure CLI 2.0-s verzióját](/cli/azure/install-azure-cli). Ha egy virtuálisgép-méretezési csoport létrehozásához szükséges, akkor [hozzon létre egy méretezési beállítása az Azure portálon](virtual-machine-scale-sets-create-portal.md).


## <a name="view-information-about-a-scale-set"></a>A méretezési adatainak megtekintése
A méretezési általános információ megtekintéséhez használja [az vmss megjelenítése](/cli/azure/vmss#show). A következő példa a méretezési készletben elnevezett információ lekérése *myScaleSet* a a *myResourceGroup* erőforráscsoportot. Adja meg a saját nevek a következők szerint:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
```


## <a name="view-vms-in-a-scale-set"></a>Nézet virtuális gépek méretezési csoportban lévő
Méretezési csoportban lévő Virtuálisgép-példány listájának megtekintéséhez használja [az vmss-példányokat](/cli/azure/vmss#list-instances). Az alábbi példa listában az összes Virtuálisgép-példány a méretezési készletben elnevezett *myScaleSet* a a *myResourceGroup* erőforráscsoportot. Adja meg ezeket a neveket a saját értékeit:

```azurecli
az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```

A megadott Virtuálisgép-példány további információkat tekinthet meg, vegye fel a `--instance-id` paramétert [get-példány-nézet az vmss](/cli/azure/vmss#get-instance-view) és adjon meg egy példány a megtekintéséhez. Az alábbi példa megtekinti az információ a Virtuálisgép-példány *0* a méretezési készletben elnevezett a *myScaleSet* és a *myResourceGroup* erőforráscsoportot. Adja meg a saját nevek a következők szerint:

```azurecli
az vmss get-instance-view \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --instance-id 0
```


## <a name="list-connection-information-for-vms"></a>Virtuális gépek listája kapcsolódási információt
Csatlakozni a virtuális gépek méretezési csoportban, akkor SSH vagy egy hozzárendelt nyilvános IP-cím és port számát a RDP lévő. Alapértelmezés szerint a hálózati címfordítás (NAT) szabályok hozzáadása történik, amely a távoli kapcsolat forgalmat továbbítja az egyes virtuális Azure terheléselosztóhoz. Kilistázhatja a cím és a portok méretezési csoportban lévő Virtuálisgép-példányokhoz való csatlakozáshoz, [az vmss lista--kapcsolat-példányadatait](/cli/azure/vmss#list-instance-connection-info). A méretezési készletben megnevezett példánya a következő példa lista kapcsolati információkat a virtuális gép *myScaleSet* és a *myResourceGroup* erőforráscsoportot. Adja meg ezeket a neveket a saját értékeit:

```azurecli
az vmss list-instance-connection-info \
  --resource-group myResourceGroup \
  --name myScaleSet
```


## <a name="change-the-capacity-of-a-scale-set"></a>A kapacitás, a méretezési módosítása
A fenti parancsok bemutatta, a méretezési és a Virtuálisgép-példányok kapcsolatos információkat. Növeléséhez vagy csökkentéséhez tegye a következőket a méretezési csoportban lévő példányok, módosíthatja a kapacitást. A méretezési hoz létre vagy eltávolítja a virtuális gépek szükséges számát, majd konfigurálja a virtuális gépek alkalmazás forgalom fogadására.

Már van egy méretezési csoportban lévő példányok száma, használja a [az vmss megjelenítése](/cli/azure/vmss#show) és a lekérdezés *sku.capacity*:

```azurecli
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Ezután manuálisan növeléséhez vagy csökkentéséhez tegye a következőket a méretezési készletben rendelkező virtuális gépek [az vmss méretezési](/cli/azure/vmss#scale). Az alábbi példában a virtuális gépek számát beállítja a méretezés beállítása *5*:

```azurecli
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 5
```

Ha a kapacitás és a skála frissítése néhány percet vesz be. Ha csökkenti a kapacitást és a skála beállítása, a virtuális gépek azonosítók eltávolítása először a legmagasabb példánnyal.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Állítsa le és indítsa el a virtuális gépek méretezési csoportban lévő
Méretezési csoportban lévő egy vagy több virtuális gép leállításához használja [az vmss stop](/cli/azure/vmss/stop). A `--instance-ids` paraméter lehetővé teszi egy vagy több virtuális gépek leállítása. Ha nem adja meg a Példányazonosítót, a méretezési csoportban lévő összes virtuális gép le van állítva. Több virtuális gép leállításához külön szóközzel minden példány azonosítója.

A következő példakód leállítja a példányt *0* a méretezési készletben elnevezett a *myScaleSet* és a *myResourceGroup* erőforráscsoportot. Adja meg a következőképpen:

```azurecli
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```

Leállított virtuális gépek maradnak, és továbbra is számítási költségek. Ha ehelyett kívánja a virtuális gép felszabadítása, és csak a tárolási költségek fel Önnek, akkor [az vmss felszabadítani](/cli/azure/vmss#deallocate). Felszabadítani a több virtuális géphez, külön szóközzel minden példány azonosítója. Az alábbi példa leáll, és felszabadítja a példány *0* a méretezési készletben elnevezett a *myScaleSet* és a *myResourceGroup* erőforráscsoportot. Adja meg a következőképpen:

```azurecli
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


### <a name="start-vms-in-a-scale-set"></a>Méretezési csoportban lévő virtuális gépek elindítása
Méretezési csoportban lévő egy vagy több virtuális gép indításához használja [az vmss start](/cli/azure/vmss#start). A `--instance-ids` paraméter lehetővé teszi egy vagy több virtuális gép elindításához. Ha nem adja meg a Példányazonosítót, a méretezési csoportban lévő összes virtuális gép indulnak el. Több virtuális gép elindításához külön szóközzel minden példány azonosítója.

A következő példában elindul a példány *0* a méretezési készletben elnevezett a *myScaleSet* és a *myResourceGroup* erőforráscsoportot. Adja meg a következőképpen:

```azurecli
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="restart-vms-in-a-scale-set"></a>Indítsa újra a virtuális gépek méretezési csoportban lévő
Méretezési csoportban lévő egy vagy több virtuális gép újraindításához használja [az vmss újraindítása](/cli/azure/vmss#restart). A `--instance-ids` paraméter lehetővé teszi egy vagy több virtuális gépek újraindítására. Ha nem adja meg a Példányazonosítót, a méretezési csoportban lévő összes virtuális gép újraindul. Több virtuális gép újraindításához külön szóközzel minden példány azonosítója.

Az alábbi példa újraindul példány *0* a méretezési készletben elnevezett a *myScaleSet* és a *myResourceGroup* erőforráscsoportot. Adja meg a következőképpen:

```azurecli
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="remove-vms-from-a-scale-set"></a>Távolítsa el a virtuális gépek egy méretezési készlet
Méretezési csoportban lévő egy vagy több virtuális gép eltávolításához használja [az vmss delete-példányok](/cli/azure/vmss#delete-instances). A `--instance-ids` paraméter lehetővé teszi egy vagy több virtuális gépek eltávolítása. Ha megad * azonosító, a méretezési csoportban lévő összes virtuális gép eltávolítása a példány. Több virtuális gép eltávolításához külön szóközzel minden példány azonosítója.

A következő példában eltávolítjuk példány *0* a méretezési készletben elnevezett a *myScaleSet* és a *myResourceGroup* erőforráscsoportot. Adja meg a következőképpen:

```azurecli
az vmss delete-instances --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="next-steps"></a>További lépések
A méretezési készlet kapcsolatos további általános feladatok közé tartozik hogyan [alkalmazás üzembe helyezése](virtual-machine-scale-sets-deploy-app.md), és [Virtuálisgép-példányok frissítéséhez](virtual-machine-scale-sets-upgrade-scale-set.md). Használhatja az Azure CLI [automatikus méretezése szabályok konfigurálása](virtual-machine-scale-sets-autoscale-overview.md).
