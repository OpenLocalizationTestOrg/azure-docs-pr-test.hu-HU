---
title: "aaaHow tooload egyenleg Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure betölteni a terheléselosztó toocreate egy magas rendelkezésre állású és biztonságos alkalmazás három Linux virtuális gépek között"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: f01752c3caec3489ee13e63000775769f3236e11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-linux-virtual-machines-in-azure-toocreate-a-highly-available-application"></a>Hogyan tooload egyenleg Linux virtuális gépek Azure toocreate a magas rendelkezésre állású alkalmazások
Terheléselosztás biztosít a rendelkezésre állási magasabb szintű bejövő kérelmek elosztásával el több virtuális gépre. Ebben az oktatóanyagban elsajátíthatja hello különböző összetevőkről hello Azure terheléselosztó ossza el a forgalmat, és magas rendelkezésre állás biztosításához. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Hozzon létre egy Azure terheléselosztó
> * A health terheléselosztói mintavétel létrehozása
> * Terheléselosztó forgalomra vonatkozó szabályok létrehozása
> * Használja a felhő inicializálás toocreate egy alapszintű Node.js alkalmazást
> * Virtuális gépek létrehozása és csatolása tooa terheléselosztó
> * A művelet egy terhelés-kiegyenlítő megtekintése
> * Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="azure-load-balancer-overview"></a>Az Azure load balancer áttekintése
Egy Azure terheléselosztó a réteg-4 (TCP, UDP) terheléselosztóhoz, amely a magas rendelkezésre állást biztosít azáltal, hogy a bejövő forgalom kifogástalan állapotú virtuális gépek között. Terheléselosztói állapotfigyelő mintavétel az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat tooan működési virtuális gép.

Egy előtér-IP-konfigurációja, amely tartalmaz egy vagy több nyilvános IP-címeket adhat meg. Az előtér-IP-konfiguráció lehetővé teszi, hogy a load balancer és az alkalmazások toobe elérhető hello interneten keresztül. 

Virtuális gépek csatlakozni a virtuális hálózati kártya (NIC) használatával tooa terheléselosztóhoz. toodistribute forgalom toohello virtuális gépeket, egy háttér címkészletet hello IP-címek hello virtuális (NIC) csatlakoztatott toohello terheléselosztó tartalmazza.

forgalom toocontrol hello áramló, megadhatja a terheléselosztási szabály az adott portok és protokollok, amelyek kapcsolódnak tooyour virtuális gépek.

Ha túl követte az oktatóanyag előző hello[hozzon létre egy virtuálisgép-méretezési csoport](tutorial-create-vmss.md), a terheléselosztó lett létrehozva. Ezeket az összetevőket az Ön hello méretezési részeként lettek konfigurálva.


## <a name="create-azure-load-balancer"></a>Az Azure terheléselosztó létrehozása
Ez a szakasz részletesen, hogyan hozhat létre, és minden egyes összetevő hello terheléselosztó konfigurálása. A terheléselosztó létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupLoadBalancer* a hello *eastus* helye:

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a>Hozzon létre egy nyilvános IP-címet
tooaccess rendszeren hello Internet, egy nyilvános IP-címet kell hello terheléselosztóhoz. Hozzon létre egy nyilvános IP-cím [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create). hello alábbi példa létrehoz egy nyilvános IP-cím nevű *myPublicIP* a hello *myResourceGroupLoadBalancer* erőforráscsoport:

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Terheléselosztó létrehozása
Hozzon létre egy terheléselosztót, [az hálózati terheléselosztó létrehozása](/cli/azure/network/lb#create). hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű *myLoadBalancer* és rendel hello *myPublicIP* cím toohello előtér-IP-konfiguráció:

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Hozzon létre egy állapotmintáihoz
tooallow hello terheléselosztó toomonitor hello állapotához az alkalmazás, használhat egy állapotmintáihoz. hello állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek hello terhelés terheléselosztó Elforgatás a válasz toohealth ellenőrzések alapján. Alapértelmezés szerint a virtuális gépek terheléselosztó terheléselosztási hello 15 másodperces időközönként két egymást követő hibák után törlődik. Létrehozhat egy állapotmintáihoz protokoll vagy egy meghatározott állapottal ellenőrzése lapon, az alkalmazás alapján. 

a következő példa hello egy TCP-Hálózatfigyelővel hoz létre. Egyéni HTTP mintavételt további részletes állapotának ellenőrzésére is létrehozhat. Ha egy egyéni HTTP-vizsgálatot, létre kell hoznia hello állapotának ellenőrzése lapon, például a *healthcheck.js*. hello mintavételi kell visszaadnia egy **HTTP 200 OK** hello terhelés terheléselosztó tookeep hello állomás Elforgatás válasz.

a TCP állapotmintáihoz toocreate, használhat [az hálózati lb mintavétel létrehozása](/cli/azure/network/lb/probe#create). hello alábbi példa létrehoz egy nevű állapotmintáihoz *myHealthProbe*:

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Hozzon létre olyan terheléselosztó szabályhoz
Olyan terheléselosztó szabályhoz használt toodefine forgalom Mitől elosztott toohello virtuális gépeket. Hello előtér-IP-konfiguráció hello bejövő forgalom és hello háttér-IP-készlet tooreceive hello forgalom, valamint hello szükséges forrás- és a célport adhat meg. a toomake meg arról, hogy csak a megfelelő virtuális gépek forgalom fogadására, is definiálhat hello állapotfigyelő mintavételi toouse.

Hozzon létre olyan terheléselosztó szabályhoz rendelkező [az hálózati terheléselosztó szabály létrehozása](/cli/azure/network/lb/rule#create). hello alábbi példa létrehoz egy nevű szabályt *myLoadBalancerRule*, használ hello *myHealthProbe* állapotmintáihoz és porton kiegyensúlyozza forgalom *80*:

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a>Virtuális hálózat konfigurálása
Mielőtt központilag az egyes virtuális gépek, és tesztelheti a terheléselosztó, hozzon létre virtuális hálózati erőforrások támogató hello. Virtuális hálózatok kapcsolatos további információkért lásd: hello [Azure virtuális hálózatok kezelése](tutorial-virtual-network.md) oktatóanyag.

### <a name="create-network-resources"></a>Hálózati erőforrások létrehozása
A virtuális hálózat létrehozása [az hálózati vnet létrehozása](/cli/azure/network/vnet#create). hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* nevű alhálózattal *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

használja a hálózati biztonsági csoport tooadd, [az hálózati nsg létrehozása](/cli/azure/network/nsg#create). hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

A hálózati biztonsági csoport szabály létrehozása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create). hello alábbi példa létrehoz egy hálózati biztonsági szabály nevű *myNetworkSecurityGroupRule*:

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

Virtuális hálózati adapter jönnek létre [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create). hello alábbi példakód létrehozza három virtuális hálózati adapter. (Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre a az alkalmazást a következő hello lépések). További virtuális hálózati adapterek és virtuális gépek létrehozása tetszőleges időpontban, és azok hozzáadása toohello terheléselosztó:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-virtual-machines"></a>Virtuális gépek létrehozása

### <a name="create-cloud-init-config"></a>Felhő inicializálás konfiguráció létrehozása
Az oktatóanyag előző [hogyan toocustomize egy Linux virtuális gép első indításakor](tutorial-automate-vm-deployment.md), akkor megtanulta, hogyan tooautomate virtuális gép testreszabása a felhő inicializálás. Használhatja ugyanazon felhő inicializálás konfigurációs fájl tooinstall NGINX hello és egy egyszerű "Hello World" Node.js-alkalmazás futtatása.

Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* és a Beillesztés hello a következő konfigurációs. A felhő rendszerhéj hello nem a helyi számítógépen hozzon létre például hello fájlt. Adja meg `sensible-editor cloud-init.txt` toocreate hello fájlt, és elérhető szerkesztők listájának megtekintéséhez. Győződjön meg arról, hogy hello egész felhő inicializálás fájl megfelelően lett lemásolva, különösen az első sor hello:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-virtual-machines"></a>Virtuális gépek létrehozása
tooimprove hello magas rendelkezésre állás az alkalmazás a virtuális gépeket helyez egy rendelkezésre állási csoportot. Rendelkezésre állási készletek kapcsolatos további információkért lásd az előző hello [hogyan toocreate magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md) oktatóanyag.

Állítsa be a rendelkezésre állási létrehozása [az virtuális gép rendelkezésre állási-csoport létrehozása](/cli/azure/vm/availability-set#create). hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett *myAvailabilitySet*:

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

Most a virtuális gépek hello hozhat létre [az virtuális gép létrehozása](/cli/azure/vm#create). hello alábbi példa létrehoz három virtuális gépek és SSH-kulcsokat generál, ha még nem léteznek:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

Nincsenek háttérfeladatok, hogy toorun után hello Azure CLI-t adja vissza, akkor toohello kérdés. Hello `--no-wait` paraméter nem várja meg az összes hello feladatok toocomplete. Elképzelhető, hogy egy másik néhány perc alatt hello alkalmazás eléréséhez. hello terheléselosztó automatikusan észleli, ha egyes virtuális gépek hello az alkalmazás fut.. Miután hello alkalmazás fut, a hello terheléselosztási szabály elindít toodistribute forgalmat.


## <a name="test-load-balancer"></a>Teszt terheléselosztó
Hello nyilvános IP-cím a terheléselosztó az beszerzése [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show). hello alábbi példa beszerzi hello IP-címet *myPublicIP* korábban létrehozott:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Majd tooa webböngészőben hello nyilvános IP-címet adhat meg. Ne feledje, mert néhány perc hello hello virtuális gépek toobe készen áll a szükséges hello terheléselosztó toodistribute forgalom toothem megkezdése előtt. hello alkalmazásokról, beleértve a virtuális gép hello hello állomásnevét adott hello terheléselosztó forgalom tooas hello a következő példa az elosztott:

![Futó Node.js-alkalmazás](./media/tutorial-load-balancer/running-nodejs-app.png)

toosee hello terheléselosztó forgalom szét az alkalmazást futtató összes három virtuális gépet, a kényszerített-frissítési a webböngésző is.


## <a name="add-and-remove-vms"></a>Hozzá és távolíthat el a virtuális gépek
Szükség lehet az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket hello tooperform karbantartása. toodeal megnövekedett forgalom tooyour alkalmazással, szükség lehet tooadd további virtuális gépeket. Ez a szakasz bemutatja, hogyan tooremove, vagy adja hozzá a virtuális gépek hello terheléselosztóról.

### <a name="remove-a-vm-from-hello-load-balancer"></a>Távolítsa el a virtuális gépek hello terheléselosztó
A virtuális gép eltávolítása hello háttér-címkészlet, amely [az hálózati hálózati adapter ip-config-címkészlet eltávolítása](/cli/azure/network/nic/ip-config/address-pool#remove). a következő példa eltávolítja hello hello virtuális hálózati Adapternek **myVM2** a *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

toosee hello terheléselosztó forgalom szét a másik két hello az alkalmazást futtató virtuális gépek akkor is kényszerített frissítési a webböngésző. Karbantartási is képes lemezvizsgálatok elvégzésére, hogy a virtuális gép, például az operációs rendszer frissítéseinek telepítése vagy a virtuális gép újraindítása végrehajtása hello.

### <a name="add-a-vm-toohello-load-balancer"></a>Virtuális gép toohello terheléselosztó felvétele
Virtuális gép karbantartásának végrehajtása után, vagy ha tooexpand kapacitás, hozzáadhat egy virtuális gép toohello háttér-címkészlet, amely [az hálózati hálózati adapter ip-config-címkészlet hozzáadása](/cli/azure/network/nic/ip-config/address-pool#add). hello következő példakóddal felveheti a hello virtuális hálózati Adapternek **myVM2** túl*myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy terhelés-kiegyenlítő létrehozott, és csatolt virtuális gépek tooit. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Hozzon létre egy Azure terheléselosztó
> * A health terheléselosztói mintavétel létrehozása
> * Terheléselosztó forgalomra vonatkozó szabályok létrehozása
> * Használja a felhő inicializálás toocreate egy alapszintű Node.js alkalmazást
> * Virtuális gépek létrehozása és csatolása tooa terheléselosztó
> * A művelet egy terhelés-kiegyenlítő megtekintése
> * Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó

Előzetes toohello következő útmutató toolearn további információk az Azure-beli virtuális hálózat összetevői.

> [!div class="nextstepaction"]
> [Virtuális gépek és virtuális hálózatok kezelése](tutorial-virtual-network.md)
