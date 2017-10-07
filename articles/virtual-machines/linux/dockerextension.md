---
title: "aaaUse hello Azure Docker Virtuálisgép-bővítmény |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Docker Virtuálisgép-bővítmény tooquickly hello és biztonságosan egy Docker-környezet az Azure-ban a Resource Manager-sablonok és hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 936d67d7-6921-4275-bf11-1e0115e66b7f
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 8e43adc594192773466ccd2d3e455105f14c1a61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension"></a>Hozzon létre egy Docker-környezetet az Azure-ban hello Docker Virtuálisgép-bővítmény
Docker egy népszerű tárolóinak kezelése és a lemezkép-készítési platform, amely lehetővé teszi a Linux tooquickly együttműködnek a tárolók. Az Azure különböző módja van Docker tooyour igényei szerint telepítheti. Ez a cikk foglalkozik hello Docker Virtuálisgép-bővítmény és az Azure Resource Manager-sablonok használatáról az Azure CLI 2.0 hello. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](dockerextension-nodejs.md).

## <a name="azure-docker-vm-extension-overview"></a>Az Azure Docker Virtuálisgép-bővítmény áttekintése
hello Azure Docker Virtuálisgép-bővítmény telepíti, és konfigurálja a hello Docker démon, a Docker-ügyfél és a Docker Compose a Linux virtuális gép (VM). Hello Azure Docker Virtuálisgép-bővítmény használatával, hogy további vezérlő és szolgáltatásokkal, mint egyszerűen Docker számítógépet használja, vagy hozzon létre hello Docker állomás, saját magának. Ezek további szolgáltatásokat, például a [Docker Compose](https://docs.docker.com/compose/overview/), ellenőrizze a hello Azure Docker Virtuálisgép-bővítmény alkalmas robusztusabb fejlesztői vagy üzemi környezetben való.

Hello különböző központi telepítési módszer, beleértve a Docker gép és az Azure tárolószolgáltatások, további információ: a következő cikkek hello:

* tooquickly típusnak egy alkalmazást, hozhat létre egy Docker állomáshoz használatával [Docker gép](docker-machine.md).
* toobuild éles használatra kész, méretezhető környezetek, amelyek további ütemezés és a felügyeleti eszközök biztosítanak, telepíthet egy [Docker Swarm-fürt a Azure-tároló szolgáltatásokra](../../container-service/dcos-swarm/container-service-deployment.md).

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a>A sablont a hello Azure Docker Virtuálisgép-bővítmény telepítése
Most egy meglévő gyors üzembe helyezés sablon toocreate Ubuntu virtuális gép által használt hello Azure Docker Virtuálisgép-bővítmény tooinstall használja, és hello Docker állomás konfigurálása. Hello sablon Itt tekintheti meg: [egyszerű Ubuntu virtuális gép a Docker-telepítés](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westus* helye:

```azurecli
az group create --name myResourceGroup --location westus
```

Ezután telepítse a virtuális gép és [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) , amely tartalmazza az Azure Docker Virtuálisgép-bővítmény hello [a Githubon az Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Adja meg a saját értékeit *newStorageAccountName*, *adminUsername*, *adminPassword*, és *dnsNameForPublicIP* az alábbiak szerint:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Hello telepítési toofinish néhány percet vesz igénybe. Hello telepítés befejeződése után [áthelyezése toonext lépéssel](#deploy-your-first-nginx-container) tooSSH tooyour virtuális gép. 

Opcionálisan tooinstead visszatérési felügyeletének toohello kérése és a telepítés lehetővé hello továbbra hello háttérben, adja hozzá a hello `--no-wait` parancs megelőző toohello jelzőt. Ez a folyamat lehetővé teszi a tooperform más tevékenységet hello CLI közben hello telepítési továbbra is fennáll, néhány percig. 

Majd megtekintheti az hello Docker-gazdagép állapotával kapcsolatos adatokat [az vm megjelenítése](/cli/azure/vm#show). hello alábbi példa ellenőrzi hello nevű virtuális gép állapotát hello *myDockerVM* (alapértelmezett neve hello sablonból hello – ne módosítsa a nevet) nevű hello erőforráscsoportban *myResourceGroup*:

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

Ha ez a parancs visszaadja *sikeres*, hello telepítés véget ért, és a következő lépés hello a virtuális gép SSH toohello is.

## <a name="deploy-your-first-nginx-container"></a>Az első nginx tároló üzembe
tooview részleteit a virtuális Gépet, többek között a következőket hello DNS-nevét használja `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`. SSH tooyour új Docker üzemeltetni a helyi számítógépről az alábbiak szerint:

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

Miután bejelentkezett toohello Docker-állomás, most futtassa az egy nginx-tároló:

```bash
sudo docker run -d -p 80:80 nginx
```

hello kimenete a következő példa hello nginx kép letöltöttként hasonló toohello és egy tároló lépések:

```bash
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

A Docker gazdagépen futó az alábbiak szerint hello tárolók hello állapotának ellenőrzése:

```bash
sudo docker ps
```

hello kimeneti a következő példa hasonló toohello, hello nginx tároló megjelenítő fut, és a 80-as és 443-as TCP-portot és a továbbított:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

toosee működés közben, a tároló megnyitása legfeljebb egy webböngészőben, és írja be a Docker állomás hello DNS-neve:

![Futó ngnix tároló](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Az Azure Docker Virtuálisgép-bővítmény sablon hivatkozása
hello előző példa egy meglévő gyors üzembe helyezés sablont használja. Hello Azure Docker Virtuálisgép-bővítmény a saját Resource Manager-sablonok is telepíthet. toodo Igen, vegye fel a hello tooyour Resource Manager-sablonok a következő, hello meghatározása `vmName` a virtuális gép megfelelően:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.*",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Részletes útmutató Resource Manager-sablonok használatával olvasásával található [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Következő lépések
Kezdésként érdemes lehet túl[hello Docker démon TCP-port konfigurálása](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), megértéséhez [Docker biztonsági](https://docs.docker.com/engine/security/security/), vagy használatával tároló üzembe helyezése [Docker Compose](https://docs.docker.com/compose/overview/). Hello Azure Docker Virtuálisgép-bővítmény maga további információkért lásd: hello [GitHub-projekt](https://github.com/Azure/azure-docker-extension/).

Hello további Docker telepítési lehetőségek az Azure-ban kapcsolatos információkért olvassa el:

* [Docker gép használata a hello Azure illesztőprogram](docker-machine.md)  
* [Ismerkedés a Docker és toodefine összeállítása, és futtassa a több tároló alkalmazást egy Azure virtuális gépen a](docker-compose-quickstart.md).
* [Az Azure Container Service-fürt üzembe helyezése](../../container-service/dcos-swarm/container-service-deployment.md)

