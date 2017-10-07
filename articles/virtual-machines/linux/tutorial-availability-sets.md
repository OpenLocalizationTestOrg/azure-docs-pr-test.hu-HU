---
title: "aaaAvailability oktatóanyag beállítja a Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "További információk a rendelkezésre állási készletek hello Linux virtuális gépek Azure-ban."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2a91e4a6057180035ec51410d9fffccaca343758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a>Hogyan toouse rendelkezésre állási beállítása


Ebből az oktatóanyagból megtudhatja, hogyan tooincrease hello rendelkezésre állása és megbízhatósága szempontjából a virtuális gép megoldások Azure-ban egy képesség nevű rendelkezésre állási készletek. Rendelkezésre állási készletek győződjön meg arról, hogy több elkülönített hardver fürt központi telepítése az Azure virtuális gépek elosztott hello. Ez biztosítja, hogy ha az Azure hardveres vagy szoftveres hiba akkor fordul elő, csak a virtuális gépek alárendelt meghatározott csökkenhet, és, amely a teljes megoldás elérhető és az azt használó ügyfelek hello szempontjából működési marad.

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Rendelkezésre állási csoport létrehozása
> * Hozzon létre egy virtuális gép rendelkezésre állási csoportba
> * Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="availability-set-overview"></a>Rendelkezésre állási csoport – áttekintés

A logikai csoportosításhoz képessége, amelyik használható rendelkezésre állási csoport megtalálható, hogy hello Virtuálisgép-erőforrások helyezi-e benne el különítve egymástól belül egy Azure-adatközpontban telepítésekor Azure tooensure. Azure biztosítja, hogy hello helyezi-e belül egy rendelkezésre állási készlet több fizikai kiszolgálókon futó virtuális gépek, a számítási, például rackszekrények, a tárolási egység és a hálózati kapcsolók. Ez biztosítja, hogy hello eseményben hardver vagy Azure szoftverhiba lép fel, csökkenhet a virtuális gépek csak egy részét, és az alkalmazás általános mentése marad, és továbbra is toobe elérhető tooyour ügyfelek. Az alapvető funkció tooleverage rendelkezésre állási csoportokkal akkor, ha azt szeretné, hogy megbízható toobuild megoldások.

Mérlegeljük, ahol például 4 előtér-webkiszolgáló, és 2 háttér-virtuális gép egy adatbázist az tipikus Virtuálisgép-alapú megoldás. Az Azure-érdemes toodefine két rendelkezésre állási csoportok csak a virtuális gépekre telepítheti: egy rendelkezésre állási készlet hello "web" réteg és a rendelkezésre állási csoporthoz hello "adatbázis" szinthez. Egy új virtuális Gépet, majd megadhatja a hello a rendelkezésre állási csoportban paraméter toohello az virtuális gép létrehozása parancs, és az Azure automatikusan biztosítja, hogy hello belül elérhető hello hoz létre virtuális gépek létrehozásakor a készlet több fizikai hardver-erőforrások között munkakönyvtárral. Ez azt jelenti, hogy hello fizikai hardver, az egyik webkiszolgálón vagy adatbázis-kiszolgáló virtuális gépeken futó hibásan működik, ha tudja, hogy hello a webkiszolgáló és az adatbázis virtuális gépek más példányai marad futó részletes mert másik hardveren.

Rendelkezésre állási készletek mindig használjon, ha azt szeretné, hogy toodeploy megbízható Virtuálisgép-alapú megoldások Azure-ban.


## <a name="create-an-availability-set"></a>Rendelkezésre állási csoport létrehozása

Rendelkezésre állási készlet használatával hozhat létre [az virtuális gép rendelkezésre állási-csoport létrehozása](/cli/azure/vm/availability-set#create). Ebben a példában a frissítés és a tartalék tartományok mindkét hello száma hivatott *2* a hello rendelkezésre állási csoport elnevezett *myAvailabilitySet* a hello *myResourceGroupAvailability*erőforráscsoportot.

Hozzon létre egy erőforráscsoportot.

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Rendelkezésre állási készletek lehetővé teszik a tooisolate erőforrások "tartalék tartományok" és "tartományok frissítése". A **tartalék tartomány** kiszolgáló + hálózati, tárolási elkülönített gyűjteményét képviseli erőforrásokat. A fenti példa hello azt jelzi, hogy azt szeretné, hogy a rendelkezésre állási csoport toobe elosztva legalább két tartalék tartományok között, ha a virtuális gépek vannak telepítve. Azt is jelzi, hogy azt szeretné, hogy a rendelkezésre állási készlet elosztott két **tartományok frissítése**.  Két frissítési tartományok gondoskodjon arról, hogy amikor a szoftverfrissítések az Azure-ban a Virtuálisgép-erőforrások elkülönített, akadályozza meg, hogy minden hello szoftver frissítését, hello alatt a virtuális gép fut egyszerre.

## <a name="configure-virtual-network"></a>Virtuális hálózat konfigurálása
Mielőtt központilag az egyes virtuális gépek, és tesztelheti a terheléselosztó, hozzon létre virtuális hálózati erőforrások támogató hello. Virtuális hálózatok kapcsolatos további információkért lásd: hello [Azure virtuális hálózatok kezelése](tutorial-virtual-network.md) oktatóanyag.

### <a name="create-network-resources"></a>Hálózati erőforrások létrehozása
A virtuális hálózat létrehozása [az hálózati vnet létrehozása](/cli/azure/network/vnet#create). hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* nevű alhálózattal *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupAvailability \
    --name myVnet \
    --subnet-name mySubnet
```
Virtuális hálózati adapter jönnek létre [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create). hello alábbi példakód létrehozza három virtuális hálózati adapter. (Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre a az alkalmazást a következő hello lépések). További virtuális hálózati adapterek és virtuális gépek létrehozása tetszőleges időpontban, és azok hozzáadása toohello terheléselosztó:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupAvailability \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-vms-inside-an-availability-set"></a>Hozzon létre virtuális gépek rendelkezésre állási csoportok belül

Virtuális gépek hello rendelkezésre állási készlet meg arról, hogy helyesen hello hardver vannak elosztva toomake belül kell létrehoznia. Meglévő virtuális gép tooan rendelkezésre állási készlet létrehozása után nem vehető fel. 

Amikor hoz létre egy virtuális gép használatával [az virtuális gép létrehozása](/cli/azure/vm#create) hello a rendelkezésre állási csoportban hello segítségével megadhat `--availability-set` paraméter toospecify hello hello rendelkezésre állási készlet nevét.

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --nics myNic$i \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

Most már tudunk két virtuális gép belül az újonnan létrehozott rendelkezésre állási csoportot. Mivel azokra hello azonos rendelkezésre állási csoport Azure biztosítja az adott hello virtuális gépek és az összes az erőforrások (beleértve a adatlemezek) különítve fizikai hardver különböző pontjain. Ehhez a terjesztéshez biztosíthatja, hogy mekkora a teljes méretű megoldás magas rendelkezésre állás érdekében.

Virtuális gépek hozzáadásakor merülhetnek egyetlen művelet, győződjön meg arról, hogy egy adott virtuális gép méretét már nem elérhető toouse a rendelkezésre állási csoport belül. A probléma akkor fordulhat elő, ha már nincs elég kapacitás tooadd el hello elkülönítési szabályok hello rendelkezésre állási csoport adatainak megőrzése mellett. Ellenőrizheti toosee milyen Virtuálisgép-méretek meglévő rendelkezésre állási készlet használatával hello belül elérhető toouse `--availability-set list-sizes` paraméter.

## <a name="check-for-available-vm-sizes"></a>Ellenőrizze az elérhető Virtuálisgép-méretek 

További virtuális gépek toohello rendelkezésre állási csoportban később adhat hozzá, de kell tooknow milyen Virtuálisgép-méretek hello hardveren érhetők el. Használjon [az virtuális gép rendelkezésre állási-készlet lista-méretek](/cli/azure/availability-set#list-sizes) toolist hello rendelkezésre állási csoport fürt összes hello elérhető méretek hello hardveren.

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Rendelkezésre állási csoport létrehozása
> * Hozzon létre egy virtuális gép rendelkezésre állási csoportba
> * Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek

Előzetes toohello oktatóanyag következő toolearn kapcsolatos virtuálisgép-méretezési készlet.

> [!div class="nextstepaction"]
> [Virtuálisgép-méretezési csoport létrehozása](tutorial-create-vmss.md)

