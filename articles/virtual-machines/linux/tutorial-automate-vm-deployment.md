---
title: "Linux virtuális gép első indításakor az Azure-ban aaaCustomize |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse felhő inicializálás és a Key Vault toocustomze Linux virtuális gépek hello először indulnak az Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 280189723ac0205226f9c0068bd605da13249ace
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-a-linux-virtual-machine-on-first-boot"></a>Hogyan toocustomize egy Linux virtuális gép első rendszerindítás
Egy korábbi oktatóanyagban, megtudta, hogyan tooSSH tooa virtuális gép (VM) és NGINX manuálisan telepítenie. virtuális gépek toocreate gyorsan és egységes módon, valamilyen automatizált művelettel általában van szükség. Egy általános módszer toocustomize egy virtuális gép első indításakor toouse [felhő inicializálás](https://cloudinit.readthedocs.io). Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Felhő inicializálás konfigurációs fájl létrehozása
> * A felhő inicializálás fájlt használó virtuális gép létrehozása
> * Hello virtuális gép létrehozása után egy futó Node.js-alkalmazás megtekintése
> * Key Vault toosecurely tároló-tanúsítványokat használ
> * Automatizálhatja a biztonságos telepítéseket NGINX felhő inicializálás


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).  



## <a name="cloud-init-overview"></a>Felhő inicializálás áttekintése
[Felhő inicializálás](https://cloudinit.readthedocs.io) megegyezik egy széles körben használt megközelítés toocustomize Linux virtuális gép elinduló hello az első alkalommal. Felhő inicializálás tooinstall csomagot, és fájlokat, vagy tooconfigure felhasználók és biztonsági írása. Felhő inicializálás hello rendszerindítási folyamat során fut, mert nincsenek további lépések nem és az ügynökök tooapply a konfigurációt.

Felhő inicializálás terjesztéseket is használható. Például, hogy ne használjon **apt-get-telepítés** vagy **yum telepítése** tooinstall csomagot. Helyette megadhatja csomagok tooinstall listáját. Felhő inicializálás hello natív csomag felügyeleti eszköz automatikusan választ hello distro használ.

Folyamatban, a partnerek tooget felhő inicializálás része azokkal, valamint, hogy ezek biztosítanak tooAzure hello képek működik. a következő táblázat hello hello aktuális felhő inicializálás elérhetőségét a Azure platformon képek ismerteti:

| Alias | Közzétevő | Ajánlat | SKU | Verzió |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04-ES LTS VERZIÓ |legújabb |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |legújabb |
| CoreOS |CoreOS |CoreOS |Stable |legújabb |


## <a name="create-cloud-init-config-file"></a>Felhő inicializálás konfigurációs fájl létrehozása
toosee felhő inicializálás a művelet hozzon létre egy virtuális Gépet, amely NGINX telepíti, és egy egyszerű "Hello, World" Node.js-alkalmazást futtat. hello inicializálás felhőalapú konfiguráció a következő szükséges hello csomagok telepíti, létrehoz egy Node.js-alkalmazást, majd inicializálása és hello alkalmazás elindítja.

Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* és a Beillesztés hello a következő konfigurációs. A felhő rendszerhéj hello nem a helyi számítógépen hozzon létre például hello fájlt. A szerkesztő kívánja használata. Adja meg `sensible-editor cloud-init.txt` toocreate hello fájlt, és elérhető szerkesztők listájának megtekintéséhez. Győződjön meg arról, hogy hello egész felhő inicializálás fájl megfelelően lett lemásolva, különösen az első sor hello:

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

Felhő inicializálás konfigurációs beállításokkal kapcsolatos további információkért lásd: [felhő inicializálás config példák](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása
A virtuális gépek létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAutomate* a hello *eastus* helye:

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

Most létrehozza a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create). Használjon hello `--custom-data` paraméter toopass a felhő inicializálás konfigurációs fájlban. Adja meg a teljes elérési útja toohello hello *felhő-init.txt* config, ha a jelenlegi munkakönyvtár kívül hello fájlt mentette. hello alábbi példakód létrehozza a virtuális gépek nevű *myAutomatedVM*:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Hello VM toobe hozott létre, néhány percet vesz igénybe hello csomagok tooinstall és hello app toostart. Nincsenek háttérfeladatok, hogy toorun után hello Azure CLI-t adja vissza, akkor toohello kérdés. Elképzelhető, hogy egy másik néhány perc alatt hello alkalmazás eléréséhez. Hello virtuális gép létrehozása, jegyezze fel a hello `publicIpAddress` hello Azure CLI által megjelenített. Ez a cím használt tooaccess hello Node.js-alkalmazás webböngésző segítségével.

webes forgalom tooreach a virtuális Gépet, nyissa meg 80-as porton az hello Internet rendelkező tooallow [az vm-port megnyitása](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a>Vizsgálati a webes alkalmazás
Most nyisson meg egy webböngészőt, és írja be *http://<publicIpAddress>*  hello címsorába. Adja meg a saját nyilvános IP-címet a virtuális gép hello folyamatot létrehozni. A Node.js-alkalmazás a következő példa hello hasonlóan jelenik meg:

![Futó NGINX-hely megtekintése](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a>A tanúsítványokat a Key Vault beszúrása
A választható szakasz bemutatja, hogyan lehet biztonságosan tanúsítványok tárolása az Azure Key Vault és szúrjon azokat a Virtuálisgép-telepítéshez hello során. Ahelyett, hogy az egyéni lemezkép hello tanúsítványokat tartalmazó bővíthetőség-a, ez a folyamat biztosítja, hogy hello legfrissebb tanúsítványok vannak olyanok tooa virtuális gép első indításakor. Hello során hello tanúsítvány soha nem hagyja hello Azure platformon, vagy fel van fedve egy parancsfájl, a parancssori előzmények vagy a sablonban.

Az Azure Key Vault megvédi a titkosítási kulcsok és titkos kulcsokat, például a tanúsítványok vagy jelszavakat. Key Vault segítségével teheti gördülékennyé hello kulcskezelési folyamatot, és lehetővé teszi a hozzáférést, és az adatok titkosításához használt kulcsok toomaintain irányítását. Ebben a forgatókönyvben vezet be az egyes Key Vault fogalmak toocreate és a tanúsítvány használata, azonban a nem teljes körű áttekintést hogyan toouse kulcstároló.

hello lépések bemutatják, hogyan zajlik:

- Egy Azure-tároló létrehozása
- Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése
- A virtuális gép tooa hello tanúsítvány tooinject a titkos kulcs létrehozása
- Hozzon létre egy virtuális Gépet, és hello tanúsítvány beszúrása

### <a name="create-an-azure-key-vault"></a>Egy Azure-tároló létrehozása
Először hozzon létre egy rendelkező Key Vaultból [az keyvault létrehozása](/cli/azure/keyvault#create) és engedélyezi azt használja a virtuális gépek telepítésekor. Minden Key Vault szükséges egy egyedi nevet, és minden kisbetű kell lennie. Cserélje le *mykeyvault* a következő példa a saját egyedi névvel Key Vault hello:

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a>Tanúsítvány jön létre, és tárolja a Key Vault
Az éles környezetben való használathoz, importálnia kell a egy érvényes tanúsítvány megbízható-szolgáltató által aláírt [az keyvault tanúsítvány importálása](/cli/azure/keyvault/certificate#import). Ebben az oktatóanyagban hello következő példa bemutatja, hogyan hozhat létre egy önaláírt tanúsítványt [az keyvault tanúsítvány létrehozása](/cli/azure/keyvault/certificate#create) használó hello alapértelmezett tanúsítási szabályzat:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a>Készítse elő a tanúsítvány használható a virtuális Géphez
toouse hello tanúsítvány során hello VM folyamatot létrehozni, szerezze be a tanúsítvány hello azonosítója [az keyvault lista-verzióját](/cli/azure/keyvault/secret#list-versions). virtuális gép hello hello tanúsítványt kell az egy bizonyos formátum tooinject azt a rendszerindító, így átalakítása hello tanúsítvány [az vm formátum-titkos](/cli/azure/vm#format-secret). a következő példa hello rendeli hozzá ezen parancsok toovariables könnyű használatra a hello hello kimenete lépéseket:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-toosecure-nginx"></a>Felhő inicializálás config toosecure NGINX létrehozása
Ha hoz létre egy virtuális Gépet, a tanúsítványok és kulcsok tárolódnak védett hello */var/lib/waagent/* könyvtár. tooautomate hozzáadását hello tanúsítvány toohello VM és NGINX konfigurálása egy frissített felhő inicializálás config hello előző példa is használhatja.

Hozzon létre egy fájlt *felhő-init-secured.txt* és a Beillesztés hello a következő konfigurációs. Újra Ha felhő rendszerhéj hello használatához hozzon létre hello felhő inicializálás konfigurációs fájl vannak, és nem a helyi számítógépen. Használjon `sensible-editor cloud-init-secured.txt` toocreate hello fájlt, és elérhető szerkesztők listájának megtekintéséhez. Győződjön meg arról, hogy hello egész felhő inicializálás fájl megfelelően lett lemásolva, különösen az első sor hello:

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
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
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
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-secure-vm"></a>Biztonságos virtuális gép létrehozása
Most létrehozza a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create). hello tanúsítványának adatait a Key Vault beszúrta a hello `--secrets` paraméter. Hello előző példához hasonlóan is át hello felhő inicializálás config a hello `--custom-data` paraméter:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-secured.txt \
    --secrets "$vm_secret"
```

Hello VM toobe hozott létre, néhány percet vesz igénybe hello csomagok tooinstall és hello app toostart. Nincsenek háttérfeladatok, hogy toorun után hello Azure CLI-t adja vissza, akkor toohello kérdés. Elképzelhető, hogy egy másik néhány perc alatt hello alkalmazás eléréséhez. Hello virtuális gép létrehozása, jegyezze fel a hello `publicIpAddress` hello Azure CLI által megjelenített. Ez a cím használt tooaccess hello Node.js-alkalmazás webböngésző segítségével.

biztonságos webes forgalom tooreach a virtuális Gépet, nyissa meg 443-as portot a hello Internet rendelkező tooallow [az vm-port megnyitása](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a>Biztonságos webes alkalmazás tesztelése
Most nyisson meg egy webböngészőt, és írja be *https://<publicIpAddress>*  hello címsorába. Adja meg a saját nyilvános IP-címet a virtuális gép hello folyamatot létrehozni. Fogadja el a hello biztonsági figyelmeztetést, ha egy önaláírt tanúsítványt használt:

![Fogadja el a webes böngésző biztonsági figyelmeztetés](./media/tutorial-automate-vm-deployment/browser-warning.png)

A biztonságos NGINX hely és a Node.js alkalmazás ezután megjelenik, mint például a következő hello:

![Futó biztonságos NGINX webhely megtekintése](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban a virtuális gépek a felhő inicializálás első rendszerindítás konfigurálta. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Felhő inicializálás konfigurációs fájl létrehozása
> * A felhő inicializálás fájlt használó virtuális gép létrehozása
> * Hello virtuális gép létrehozása után egy futó Node.js-alkalmazás megtekintése
> * Key Vault toosecurely tároló-tanúsítványokat használ
> * Automatizálhatja a biztonságos telepítéseket NGINX felhő inicializálás

Hogyan előzetes toohello következő útmutató toolearn toocreate egyéni Virtuálisgép-lemezképek.

> [!div class="nextstepaction"]
> [Egyéni virtuálisgép-rendszerképek létrehozása](./tutorial-custom-images.md)
