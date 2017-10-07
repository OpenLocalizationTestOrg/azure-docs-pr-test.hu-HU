---
title: "a webkiszolgáló SSL-tanúsítványok az Azure-ban aaaSecure |} Microsoft Docs"
description: "Ismerje meg, hogyan toosecure hello NGINX webkiszolgáló SSL-tanúsítványok a Linux virtuális gép az Azure-ban"
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
ms.date: 07/17/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d3a62d77ac05c9aa2a44356b7c8e44cb485b81aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a>Biztonságos Linux virtuális gépre az Azure-ban SSL-tanúsítványokkal egy webkiszolgáló
toosecure webkiszolgálók, a Secure Sockets később (SSL) tanúsítvány lehet tooencrypt webes forgalom használt. Ezek SSL-tanúsítványokat tárolhatja az Azure Key Vault, és a tanúsítványok tooLinux virtuális gépek (VM) biztonságos telepítéshez engedélyezése az Azure-ban. Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Egy Azure-tároló létrehozása
> * Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése
> * Hozzon létre egy virtuális Gépet, és hello NGINX-webkiszolgáló telepítése
> * Hello tanúsítvány behelyezése hello VM és SSL-kötést NGINX konfigurálása

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).  


## <a name="overview"></a>Áttekintés
Az Azure Key Vault megvédi a titkosítási kulcsok és titkos kulcsok, ilyen tanúsítványokat vagy jelszavakat. Key Vault segítségével teheti gördülékennyé hello tanúsítvány felügyeleti folyamatot, és lehetővé teszi a kulcsok ezeknek a tanúsítványoknak elérő toomaintain irányítását. Hozzon létre egy önaláírt tanúsítványt Key Vault belül, vagy töltsön fel egy meglévő, a megbízható tanúsítványt, amely már Ön a tulajdonosa.

Ahelyett, hogy az egyéni Virtuálisgép-lemezkép tanúsítványokat tartalmaz bővíthetőség a tanúsítványok behelyezése egy futó virtuális Gépet. Ez a folyamat biztosítja, hogy naprakész tanúsítványok hello telepítve vannak a webkiszolgáló üzembe helyezése során. Újítsa meg, vagy cserélje le a tanúsítványt, ha még nincs toocreate egy új egyéni Virtuálisgép-lemezkép. hello legújabb tanúsítványok automatikusan vannak olyanok, további virtuális gépek létrehozásakor. Hello teljes folyamat során hello tanúsítványok soha nem hagyja hello Azure platformon, vagy egy parancsfájl, a parancssori előzmények vagy a sablon ki vannak téve.


## <a name="create-an-azure-key-vault"></a>Egy Azure-tároló létrehozása
Tanúsítványok és a kulcstároló létrehozásához, hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupSecureWeb* a hello *eastus* helye:

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

Ezután hozzon létre egy rendelkező Key Vaultból [az keyvault létrehozása](/cli/azure/keyvault#create) és engedélyezi azt használja a virtuális gépek telepítésekor. Minden Key Vault szükséges egy egyedi nevet, és minden kisbetű kell lennie. Cserélje le  *<mykeyvault>*  a következő példa a saját egyedi névvel Key Vault hello:

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Egy tanúsítvány jön létre, és tárolja a Key Vault
Az éles környezetben való használathoz, importálnia kell a egy érvényes tanúsítvány megbízható-szolgáltató által aláírt [az keyvault tanúsítvány importálása](/cli/azure/certificate#import). Ebben az oktatóanyagban hello következő példa bemutatja, hogyan hozhat létre egy önaláírt tanúsítványt [az keyvault tanúsítvány létrehozása](/cli/azure/certificate#create) használó hello alapértelmezett tanúsítási szabályzat:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a>Készítse elő a tanúsítványt egy virtuális Géphez való használatra
toouse hello tanúsítvány során hello VM folyamatot létrehozni, szerezze be a tanúsítvány hello azonosítója [az keyvault lista-verzióját](/cli/azure/keyvault/secret#list-versions). Átalakítás hello tanúsítvány [az vm formátum-secret](/cli/azure/vm#format-secret). a következő példa hello rendeli hozzá ezen parancsok toovariables könnyű használatra a hello hello kimenete lépéseket:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-toosecure-nginx"></a>Hozzon létre egy felhő-init config toosecure NGINX
[Felhő inicializálás](https://cloudinit.readthedocs.io) megegyezik egy széles körben használt megközelítés toocustomize Linux virtuális gép elinduló hello az első alkalommal. Felhő inicializálás tooinstall csomagot, és fájlokat, vagy tooconfigure felhasználók és biztonsági írása. Felhő inicializálás hello rendszerindítási folyamat során fut, mert nincsenek további lépések nem és az ügynökök tooapply a konfigurációt.

Ha hoz létre egy virtuális Gépet, a tanúsítványok és kulcsok tárolódnak védett hello */var/lib/waagent/* könyvtár. tooautomate hello tanúsítvány hozzáadása toohello virtuális gép és konfigurálás hello webkiszolgáló, használja a felhő inicializálás. Ebben a példában azt telepítse és konfigurálja hello NGINX-webkiszolgálón. Használhatja a hello azonos tooinstall feldolgozni, és konfigurálja az Apache. 

Hozzon létre egy fájlt *felhő-init-webalkalmazás-server.txt* és a következő konfigurációs Beillesztés hello:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a>Biztonságos virtuális gép létrehozása
Most létrehozza a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create). hello tanúsítványának adatait a Key Vault beszúrta a hello `--secrets` paraméter. Adja meg a hello felhő inicializálás konfigurációban a hello `--custom-data` paraméter:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

Hello VM toobe hozott létre, néhány percet vesz igénybe hello csomagok tooinstall és hello app toostart. Hello virtuális gép létrehozása, jegyezze fel a hello `publicIpAddress` hello Azure CLI által megjelenített. Ez a cím használt tooaccess van a webhely egy webböngészőben.

biztonságos webes forgalom tooreach a virtuális Gépet, nyissa meg 443-as portot a hello Internet rendelkező tooallow [az vm-port megnyitása](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-hello-secure-web-app"></a>Teszt hello biztonságos webes alkalmazás
Most nyisson meg egy webböngészőt, és írja be *https://<publicIpAddress>*  hello címsorába. Adja meg a saját nyilvános IP-címet a virtuális gép hello folyamatot létrehozni. Fogadja el a hello biztonsági figyelmeztetést, ha egy önaláírt tanúsítványt használt:

![Fogadja el a webes böngésző biztonsági figyelmeztetés](./media/tutorial-secure-web-server/browser-warning.png)

A biztonságos NGINX webhely ezután megjelenik, mint például a következő hello:

![Futó biztonságos NGINX webhely megtekintése](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban az NGINX webkiszolgáló az Azure Key Vault tárolt SSL-tanúsítvánnyal védett. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Egy Azure-tároló létrehozása
> * Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése
> * Hozzon létre egy virtuális Gépet, és hello NGINX-webkiszolgáló telepítése
> * Hello tanúsítvány behelyezése hello VM és SSL-kötést NGINX konfigurálása

Kövesse a hivatkozást toosee előzetesen elkészített virtuálisgép-parancsprogram minták.

> [!div class="nextstepaction"]
> [Windows virtuális gép parancsfájl minták](./cli-samples.md)