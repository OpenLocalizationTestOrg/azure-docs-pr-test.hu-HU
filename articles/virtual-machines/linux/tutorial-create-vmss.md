---
title: "egy Azure Linux virtuálisgép-méretezési csoportok aaaCreate |} Microsoft Docs"
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
ms.openlocfilehash: 00dd81043f9be46ef2dc6dfe97eefdb20944ee13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a>Hozzon létre egy virtuálisgép-méretezési és magas rendelkezésre állású Linux alkalmazás telepítése
A virtuálisgép-méretezési csoport toodeploy lehetővé teszi, és az azonos, az automatikus skálázást virtuális gépek kezelésére. Hello hello méretezési csoportban lévő virtuális gépek száma manuálisan méretezhető, vagy a szabályok tooautoscale CPU kihasználtsága, a memória igény szerint vagy a hálózati forgalom alapján határozza meg. Ebben az oktatóanyagban telepít egy virtuálisgép-méretezési beállítása az Azure-ban. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Egy alkalmazás tooscale felhő inicializálás toocreate használata
> * Hozzon létre egy virtuálisgép-méretezési csoport
> * Növeli vagy csökkenti a méretezési csoportban lévő példányok hello száma
> * Kapcsolatinformáció méretezési készlet példányok megtekintése
> * Méretezési csoportban lévő adatok lemez használata


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="scale-set-overview"></a>Méretezési készlet – áttekintés
A virtuálisgép-méretezési csoport toodeploy lehetővé teszi, és az azonos, az automatikus skálázást virtuális gépek kezelésére. Skálázási készletekben használata hello azonos összetevők, megismerte hello előző oktatóprogram túl[hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md). Állítsa be, és logikai hiba és a frissítési tartományok között elosztott rendelkezésre állási méretezési csoportban lévő virtuális gépek jönnek létre.

Virtuális gépek méretezési csoportban lévő igény szerint jönnek létre. Megadhatja az automatikus skálázási szabályok toocontrol hogyan és mikor virtuális gépek hozzáadásakor vagy eltávolításakor hello méretezési készlet. Ezek a szabályok alapján metrikák például CPU-terhelést, a memória használata vagy a hálózati forgalmat is elindíthatja.

Skálázási készletekben támogatási too1, 000 virtuális gépeken, ha az Azure platformon lemezképet használ fel. A termelési számítási feladatokhoz, Kezdésként túl[hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md). Too100 virtuális gépeinek egy méretezési állítható be, ha egyéni lemezkép használatával hozhat létre.


## <a name="create-an-app-tooscale"></a>Hozzon létre egy alkalmazást tooscale
Az éles környezetben való használathoz, Kezdésként túl[hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md) , amely tartalmazza az alkalmazás telepítését és konfigurálását. A jelen oktatóanyag esetében lehetővé teszi, hogy első rendszerindítási tooquickly futó virtuális gépek, tekintse meg a méretezési művelet készletben hello testreszabása.

Az előző oktatóanyagban megtanulta [hogyan toocustomize egy Linux virtuális gép első indításakor](tutorial-automate-vm-deployment.md) a felhő inicializálás. Használhatja ugyanazon felhő inicializálás konfigurációs fájl tooinstall NGINX hello és egy egyszerű "Hello World" Node.js-alkalmazás futtatása. 

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


## <a name="create-a-scale-set"></a>Méretezési készlet létrehozása
A méretezési csoport létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupScaleSet* a hello *eastus* helye:

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

Most hozzon létre egy virtuálisgép-méretezési állítható be [az vmss létrehozása](/cli/azure/vmss#create). hello alábbi példa létrehoz egy méretezési készletben elnevezett *myScaleSet*, hello felhő inicializálás fájl toocustomize hello virtuális gép használja, és SSH-kulcsokat generál, ha azok nem léteznek:

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

Néhány perc toocreate vesz igénybe, és hello méretezési készlet erőforrások és a virtuális gépek konfigurálása. Nincsenek háttérfeladatok, hogy toorun után hello Azure CLI-t adja vissza, akkor toohello kérdés. Elképzelhető, hogy egy másik néhány perc alatt hello alkalmazás eléréséhez.


## <a name="allow-web-traffic"></a>Webalkalmazás-kezelési forgalom engedélyezése
A terheléselosztó hello virtuálisgép-méretezési csoport részeként automatikusan lett létrehozva. hello terheléselosztó elosztása forgalom meghatározott virtuális gépeket használ a load balancer szabályok készlete. Load balancer fogalmakat és hello következő oktatóanyagban konfigurálásával kapcsolatos részletesebb [hogyan tooload elosztása a virtuális gépek Azure-ban](tutorial-load-balancer.md).

tooallow forgalom tooreach hello webes alkalmazás, hozzon létre egy szabályt a [az hálózati terheléselosztó szabály létrehozása](/cli/azure/network/lb/rule#create). hello alábbi példa létrehoz egy nevű szabályt *myLoadBalancerRuleWeb*:

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
toosee az beszerzése hello nyilvános IP-címét a terheléselosztót, a Node.js-alkalmazás hello weben [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show). hello alábbi példa beszerzi hello IP-címet *myScaleSetLBPublicIP* hello méretezési részeként létrehozott:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Adja meg a nyilvános IP-cím hello tooa webböngészőben. hello alkalmazásokról, beleértve az adott hello VM betöltése elosztott terheléselosztó felé irányuló forgalom hello hello állomásnevét:

![Futó Node.js-alkalmazás](./media/tutorial-create-vmss/running-nodejs-app.png)

toosee hello méretezési készletben működés közben, akkor is kényszerített frissítési a webes böngésző toosee hello terhelését terheléselosztó forgalom szét az alkalmazást futtató összes hello virtuális gépet.


## <a name="management-tasks"></a>Felügyeleti feladatok
Hello méretezési hello életciklusa során szükség lehet a toorun egy vagy több felügyeleti feladatokat. Emellett érdemes lehet toocreate olyan parancsfájlok, amelyek különböző életciklus-feladatok automatizálásához. hello Azure CLI 2.0 biztosít egy gyorsan toodo ezeket a feladatokat. Az alábbiakban néhány gyakori feladatot.

### <a name="view-vms-in-a-scale-set"></a>Nézet virtuális gépek méretezési csoportban lévő
tooview a skála futó virtuális gépek listájának megadásához használja [az vmss-példányokat](/cli/azure/vmss#list-instances) az alábbiak szerint:

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

hello hasonló toohello a következő példa a kimenetre:

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a>Növeli vagy csökkenti a Virtuálisgép-példányok
példányok száma toosee hello jelenleg egy méretezési állította, használjon [az vmss megjelenítése](/cli/azure/vmss#show) és a lekérdezés *sku.capacity*:

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Majd manuálisan növelhető és csökkenthető hello méretezési a készletben lévő virtuális gépek száma hello [az vmss méretezési](/cli/azure/vmss#scale). hello alábbi mintakód hello virtuális gépek száma a méretezési készletben túl a*5*:

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

Automatikus skálázási szabályok segítségével határozza meg, hogyan tooscale felfelé vagy lefelé hello virtuális gépek számát a skála állítsa be a válasz toodemand például a hálózati forgalom vagy a CPU-használat. Ezek a szabályok jelenleg az Azure CLI 2.0 nem állítható be. Használjon hello [Azure-portálon](https://portal.azure.com) tooconfigure automatikus skálázási.

### <a name="get-connection-info"></a>Kapcsolat-adatok beolvasása
tooobtain kapcsolatadatok készül hello virtuális gépek a méretezési készletben, használja [az vmss lista--kapcsolat-példányadatait](/cli/azure/vmss#list-instance-connection-info). Ez a parancs kimenetében hello nyilvános IP-cím és port az egyes virtuális gépekhez, amely lehetővé teszi az SSH tooconnect:

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a>Az adatlemezek használata méretezési csoportok
Hozzon létre, és adatlemezek használata méretezési készlet. Egy korábbi oktatóanyagban megtanulta, hogyan túl[kezelése Azure lemezek](tutorial-manage-disks.md) , hogy a körvonal hello ajánlott eljárásokról és a teljesítménnyel kapcsolatos fejlesztések az alkalmazások támaszkodva hello operációsrendszer-lemez helyett adatlemezek.

### <a name="create-scale-set-with-data-disks"></a>Adatlemezekkel rendelkező méretezési készlet létrehozása
toocreate terjedő skálán állítsa be, és csatlakoztassa az adatlemezek hozzáadása hello `--data-disk-sizes-gb` paraméter toohello [az vmss létrehozása](/cli/azure/vmss#create) parancsot. hello alábbi példa létrehoz egy méretezési a készletben *50*Gb adatlemezt csatolni tooeach példány:

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
tooadd egy adatok lemez tooinstances a méretezési megadásához használja [az vmss lemez csatolása](/cli/azure/vmss/disk#attach). hello következő példakóddal felveheti egy *50*Gb szabad tooeach példány:

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a>Adatlemez leválasztása
tooremove egy adatok lemez tooinstances a méretezési megadásához használja [az vmss lemez leválasztása](/cli/azure/vmss/disk#detach). hello következő példában eltávolítjuk hello adatlemez LUN azonosítójú *2* az egyes példányok:

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban létre egy virtuálisgép-méretezési készlet. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Egy alkalmazás tooscale felhő inicializálás toocreate használata
> * Hozzon létre egy virtuálisgép-méretezési csoport
> * Növeli vagy csökkenti a méretezési csoportban lévő példányok hello száma
> * Kapcsolatinformáció méretezési készlet példányok megtekintése
> * Méretezési csoportban lévő adatok lemez használata

Előzetes toohello következő útmutató toolearn bővebben a hálózati terheléselosztást a virtuális gépek fogalmakat.

> [!div class="nextstepaction"]
> [Virtuális gépek terhelést elosztani](tutorial-load-balancer.md)