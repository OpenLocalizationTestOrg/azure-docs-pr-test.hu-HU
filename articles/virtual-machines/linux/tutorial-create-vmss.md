---
title: "Hozzon létre egy virtuálisgép-méretezési csoportok Linux az Azure-ban |} Microsoft Docs"
description: "A Linux virtuális gépet egy virtuálisgép-méretezési csoport segítségével egy magas rendelkezésre állású alkalmazás létrehozását és telepítését"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.openlocfilehash: 2b8d519e11f70eda164bd8f6e131a3989f242ab0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a>Hozzon létre egy virtuálisgép-méretezési és magas rendelkezésre állású Linux alkalmazás telepítése
A virtuálisgép-méretezési csoport lehetővé teszi, telepítéséhez és kezeléséhez azonos, az automatikus skálázást virtuális gépek halmazát jelenti. A méretezési csoportban lévő virtuális gépek száma manuálisan méretezheti, vagy az automatikus skálázás CPU kihasználtsága, a memória igény szerint vagy a hálózati forgalom alapján szabályok megadása. Ebben az oktatóanyagban telepít egy virtuálisgép-méretezési beállítása az Azure-ban. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Felhő inicializálás segítségével méretezési-alkalmazás létrehozása
> * Hozzon létre egy virtuálisgép-méretezési csoport
> * Növeli vagy csökkenti a méretezési csoportban lévő példányok száma
> * Kapcsolatinformáció méretezési készlet példányok megtekintése
> * Méretezési csoportban lévő adatok lemez használata


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Telepítése és a parancssori felület helyileg használata mellett dönt, ha ez az oktatóanyag van szükség, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="scale-set-overview"></a>Méretezési készlet – áttekintés
A virtuálisgép-méretezési csoport lehetővé teszi, telepítéséhez és kezeléséhez azonos, az automatikus skálázást virtuális gépek halmazát jelenti. Méretezési csoportok az azonos összetevőket használnak, mint az előző oktatóanyag megismerte [hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md). Állítsa be, és logikai hiba és a frissítési tartományok között elosztott rendelkezésre állási méretezési csoportban lévő virtuális gépek jönnek létre.

Virtuális gépek méretezési csoportban lévő igény szerint jönnek létre. Megadhatja az automatikus skálázási szabályok, hogy hogyan és mikor virtuális gépek hozzáadásakor vagy eltávolításakor a méretezési készlet. Ezek a szabályok alapján metrikák például CPU-terhelést, a memória használata vagy a hálózati forgalmat is elindíthatja.

Skálázási készletekben legfeljebb 1000 virtuális gépek támogatása, ha az Azure platformon lemezképet használ. A termelési számítási feladatokhoz, érdemes lehet [hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md). Legfeljebb 100 virtuális gépek egy méretezési állítható be, ha egyéni lemezkép használatával hozhat létre.


## <a name="create-an-app-to-scale"></a>Méretezési-alkalmazás létrehozása
Az éles környezetben való használathoz, érdemes lehet [hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md) , amely tartalmazza az alkalmazás telepítését és konfigurálását. A jelen oktatóanyag esetében lehetővé teszi, hogy gyorsan megtekintheti a méretezési művelet a készletben lévő első rendszerindító virtuális gépek testreszabása.

Az előző oktatóanyagban megtanulta [első indításakor Linux virtuális gépek testreszabása](tutorial-automate-vm-deployment.md) a felhő inicializálás. Az azonos felhő inicializálás konfigurációs fájl segítségével NGINX telepítheti és futtathatja egy egyszerű "Hello, World" Node.js alkalmazást. 

Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* , majd illessze be a következő konfigurációt. A felhő rendszerhéj nem a helyi számítógépen hozzon létre például a fájlt. Adja meg `sensible-editor cloud-init.txt` hozza létre a fájlt, és elérhető szerkesztők listájának megtekintéséhez. Győződjön meg arról, hogy az egész felhő inicializálás fájl megfelelően lett lemásolva különösen az első sor:

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


## <a name="create-a-scale-set"></a>Méretezési készlet létrehozása
A méretezési csoport létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create). Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupScaleSet* a a *eastus* helye:

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

Most hozzon létre egy virtuálisgép-méretezési állítható be [az vmss létrehozása](/cli/azure/vmss#create). Az alábbi példakód létrehozza a méretezési készletben elnevezett *myScaleSet*, a felhő inicializálás fájlt használja a virtuális gép testreszabása és SSH-kulcsokat generál, ha azok nem léteznek:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

Hozza létre és konfigurálja a méretezési készlet erőforrások és a virtuális gépek néhány percet vesz igénybe. Nincsenek háttérfeladatok, hogy végezze el az Azure parancssori felület visszatér a parancssorba. Elképzelhető, hogy egy másik néhány percet, mielőtt hozzáférhetne az alkalmazáshoz.


## <a name="allow-web-traffic"></a>Webalkalmazás-kezelési forgalom engedélyezése
A terheléselosztó a virtuálisgép-méretezési csoport részeként automatikusan lett létrehozva. A load balancer egy meghatározott virtuális gépeket használ a load balancer szabályok készletét forgalom elosztása. Load balancer fogalmak és a következő oktatóanyag konfigurálásával kapcsolatos részletesebb [hogyan virtuális gépek terhelést elosztani az Azure-ban](tutorial-load-balancer.md).

A webes alkalmazás eléréséhez forgalom engedélyezéséhez hozzon létre egy szabály [az hálózati terheléselosztó szabály létrehozása](/cli/azure/network/lb/rule#create). Az alábbi példa létrehoz egy nevű szabályt *myLoadBalancerRuleWeb*:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a>Az alkalmazás tesztelése
Az Node.js alkalmazás megtekintése a weben, szerezze be a terheléselosztó a nyilvános IP-címe [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show). Az alábbi példa beolvassa az IP-címek *myScaleSetLBPublicIP* hozza létre a méretezési részeként:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Adja meg a nyilvános IP-címet egy webböngészőben. Az alkalmazás megjelenik, beleértve az állomásnevet, a virtuális gép, amelyek a terheléselosztó felé irányuló forgalom:

![Futó Node.js-alkalmazás](./media/tutorial-create-vmss/running-nodejs-app.png)

Tekintse meg a méretezési készletben működés közben, akkor is kényszerített frissítési a webböngészőt a terheléselosztó forgalom szét az alkalmazást futtató összes virtuális gép.


## <a name="management-tasks"></a>Felügyeleti feladatok
A méretezési életciklusa során szükség lehet egy vagy több felügyeleti feladatok futtatásához. Emellett érdemes lehet különböző életciklus-feladatokat automatizáló parancsfájlokat hozhatnak létre. Az Azure CLI 2.0 e feladatok elvégzéséhez gyors lehetőséget kínál. Az alábbiakban néhány gyakori feladatot.

### <a name="view-vms-in-a-scale-set"></a>Nézet virtuális gépek méretezési csoportban lévő
A méretezési csoportban lévő rendszert futtató virtuális gépek listájának megtekintéséhez használja [az vmss-példányokat](/cli/azure/vmss#list-instances) az alábbiak szerint:

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

A kimenet a következő példához hasonló:

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a>Növeli vagy csökkenti a Virtuálisgép-példányok
Már van egy méretezési csoportban lévő példányok száma, használja a [az vmss megjelenítése](/cli/azure/vmss#show) és a lekérdezés *sku.capacity*:

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Ezután manuálisan növeléséhez vagy csökkentéséhez tegye a következőket a méretezési készletben rendelkező virtuális gépek [az vmss méretezési](/cli/azure/vmss#scale). Az alábbi példában a virtuális gépek számát beállítja a méretezés beállítása *5*:

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

Automatikus skálázási szabályok segítségével meghatározhatja, hogyan kell felfelé vagy lefelé a virtuális gépek számát a méretezési iránti igény miatt például a hálózati forgalom vagy a CPU-használat készletben méretezése. Ezek a szabályok jelenleg az Azure CLI 2.0 nem állítható be. Használja a [Azure-portálon](https://portal.azure.com) automatikus skálázás konfigurálása.

### <a name="get-connection-info"></a>Kapcsolat-adatok beolvasása
A virtuális gépek a méretezési csoportok kapcsolati információ beszerzéséhez használja [az vmss lista--kapcsolat-példányadatait](/cli/azure/vmss#list-instance-connection-info). Ez a parancs kimenetében a nyilvános IP-cím és port az egyes virtuális gépekhez, amely lehetővé teszi, hogy csatlakozzon SSH:

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a>Az adatlemezek használata méretezési csoportok
Hozzon létre, és adatlemezek használata méretezési készlet. Egy korábbi oktatóanyagban megtanulta, hogyan [kezelése Azure lemezek](tutorial-manage-disks.md) , amely ismerteti az ajánlott eljárásokról és a teljesítménnyel kapcsolatos fejlesztések az operációsrendszer-lemezképet, hanem az adatlemezek alkalmazások készítéséhez.

### <a name="create-scale-set-with-data-disks"></a>Adatlemezekkel rendelkező méretezési készlet létrehozása
Hozzon létre egy méretezési és adatlemezt csatolni, adja hozzá a `--data-disk-sizes-gb` paramétert a [az vmss létrehozása](/cli/azure/vmss#create) parancsot. Az alábbi példakód létrehozza a skála állítható be *50*Gb adatlemezek csatolva minden példány:

```azurecli-interactive 
az vmss create \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetDisks \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --custom-data cloud-init.txt \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50
```

Ha példányok egy méretezési eltávolították, a mellékelt adatok lemezek is törlődnek.

### <a name="add-data-disks"></a>Adatlemez hozzáadása
A méretezési csoportban lévő példányok adhat hozzá adatlemezt, [az vmss lemez csatolása](/cli/azure/vmss/disk#attach). A következő példa egy *50*-példányokhoz Gb lemezterület:

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a>Adatlemez leválasztása
A méretezési csoportban lévő példányokhoz adatlemezt eltávolításához használja [az vmss lemez leválasztása](/cli/azure/vmss/disk#detach). A következő példában eltávolítjuk a LUN azonosítójú adatlemeze *2* az egyes példányok:

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban létre egy virtuálisgép-méretezési készlet. Megtudta, hogyan, hogy:

> [!div class="checklist"]
> * Felhő inicializálás segítségével méretezési-alkalmazás létrehozása
> * Hozzon létre egy virtuálisgép-méretezési csoport
> * Növeli vagy csökkenti a méretezési csoportban lévő példányok száma
> * Kapcsolatinformáció méretezési készlet példányok megtekintése
> * Méretezési csoportban lévő adatok lemez használata

A következő oktatóanyag további információt a hálózati terheléselosztást a virtuális gépek fogalmak továbblépés.

> [!div class="nextstepaction"]
> [Virtuális gépek terhelést elosztani](tutorial-load-balancer.md)