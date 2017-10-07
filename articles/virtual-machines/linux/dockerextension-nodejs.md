---
title: "aaaUse hello Azure Docker Virtuálisgép-bővítmény a hello Azure CLI 1.0 |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Docker Virtuálisgép-bővítmény tooquickly hello és biztonságosan központi telepítése egy Docker-környezethez az Azure Resource Manager-sablonok használatával."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 2133cdb1af741fe30093910fae5c3b2c91e8d5fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension-with-hello-azure-cli-10"></a>Hozzon létre egy Docker-környezetet a Docker Virtuálisgép-bővítmény hello használata hello Azure CLI 1.0 Azure-ban
Docker egy népszerű tárolóinak kezelése és a lemezkép-készítési platform, amely lehetővé teszi tooquickly együttműködnek a tárolók a Linux (és a Windows is). Az Azure különböző módja van Docker tooyour igényei szerint telepítheti. Ez a cikk foglalkozik a hello Docker Virtuálisgép-bővítmény és az Azure Resource Manager-sablonok. 

Hello különböző központi telepítési módszer, beleértve a Docker gép és az Azure tárolószolgáltatások, további információ: a következő cikkek hello:

* tooquickly típusnak egy alkalmazást, hozhat létre egy Docker állomáshoz használatával [Docker gép](docker-machine.md).
* Nagyobb, stabilabb környezetben használható hello Azure Docker Virtuálisgép-bővítmény, amely is támogatja a [Docker Compose](https://docs.docker.com/compose/overview/) toogenerate konzisztens tároló központi telepítéseket. Ez a cikk adatokat hello Azure Docker Virtuálisgép-bővítmény használatával.
* toobuild éles használatra kész, méretezhető környezetek, amelyek további ütemezés és a felügyeleti eszközök biztosítanak, telepíthet egy [Docker Swarm-fürt a Azure-tároló szolgáltatásokra](../../container-service/dcos-swarm/container-service-deployment.md).

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#azure-docker-vm-extension-overview) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](dockerextension.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell 

## <a name="azure-docker-vm-extension-overview"></a>Az Azure Docker Virtuálisgép-bővítmény áttekintése
hello Azure Docker Virtuálisgép-bővítmény telepíti, és konfigurálja a hello Docker démon, a Docker-ügyfél és a Docker Compose a Linux virtuális gép (VM). Hello Azure Docker Virtuálisgép-bővítmény használatával, hogy további vezérlő és szolgáltatásokkal, mint egyszerűen Docker számítógépet használja, vagy hozzon létre hello Docker állomás, saját magának. Ezek további szolgáltatásokat, például a [Docker Compose](https://docs.docker.com/compose/overview/), ellenőrizze a hello Azure Docker Virtuálisgép-bővítmény alkalmas robusztusabb fejlesztői vagy üzemi környezetben való.

Az Azure Resource Manager-sablonok a környezet hello struktúra határozza meg. Lehetővé teszi toocreate és erőforrások, például a hello Docker állomás virtuális gépek, a tárolás, a szerepköralapú hozzáférés-vezérlést (RBAC) és a diagnosztika konfigurálása. A sablonok toocreate további központi telepítéseket egységes módon is felhasználhatja. További információ az Azure Resource Manager és a sablonok: [Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md). 

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a>A sablont a hello Azure Docker Virtuálisgép-bővítmény telepítése
Most egy meglévő gyors üzembe helyezés sablon toocreate Ubuntu virtuális gép által használt hello Azure Docker Virtuálisgép-bővítmény tooinstall használja, és hello Docker állomás konfigurálása. Hello sablon Itt tekintheti meg: [egyszerű Ubuntu virtuális gép a Docker-telepítés](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Hello kell [Azure CLI legújabb](../../cli-install-nodejs.md) telepítve, és bejelentkezett használatával hello Resource Manager módra az alábbiak szerint:

```azurecli
azure config mode arm
```

Azure CLI hello sablon URI megadása hello segítségével hello sablon üzembe helyezése. hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westus* helyét. A saját erőforráscsoport-név és hely a következőképpen használhatja:

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Hello kér tooname válaszolja meg a tárfiók, és adja meg a felhasználónevet és jelszót adjon meg egy DNS-név. hello hasonló toohello a következő példa a kimenetre:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for hello following parameters
newStorageAccountName: mystorageaccount
adminUsername: azureuser
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicidns
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

hello Azure CLI-t adja vissza, toohello kérdés csak néhány másodperc múlva, de a Docker állomás létrehozása még folyamatban van, és a konfigurált hello Azure Docker Virtuálisgép-bővítmény. Hello telepítési toofinish néhány percet vesz igénybe. Hello segítségével hello Docker gazdagép állapotával kapcsolatos részleteket `azure vm show` parancsot.

hello alábbi példa ellenőrzi hello nevű virtuális gép állapotát hello *myDockerVM* (alapértelmezett neve hello sablonból hello – ne módosítsa a nevet) nevű hello erőforráscsoportban *myResourceGroup*. Adja meg a hello az előző lépésben létrehozott hello erőforráscsoport hello nevét:

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

hello kimenete hello `azure vm show` parancs van a hasonló toohello következő példát:

```azurecli
info:    Executing command vm show
+ Looking up hello VM "myDockerVM"
+ Looking up hello NIC "myVMNicD"
+ Looking up hello public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicdns.westus.cloudapp.azure.com
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Tetején hello hello kimeneti, lásd a hello **ProvisioningState** a virtuális gép hello. Amikor megjelenik *sikeres*, hello telepítés véget ért, és a virtuális gép SSH toohello is.

Hello kimeneti hello vége felé *FQDN* hello teljesen minősített tartományneve a Docker-állomás. Az FQDN-je valóban használt funkciókért tooSSH tooyour Docker állomás hello hátralévő lépéseket.

## <a name="deploy-your-first-nginx-container"></a>Az első nginx tároló üzembe
Egyszer hello telepítés véget ért, SSH tooyour új Docker állomás a helyi számítógépről. Adja meg a saját felhasználónév és a teljes tartománynév

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
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

toosee működés közben, a tároló nyisson meg egy webes böngésző össze, és írja be a Docker gazdagép hello FQDN-neve:

![Futó ngnix tároló](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Az Azure Docker Virtuálisgép-bővítmény sablon hivatkozása
hello előző példa egy meglévő gyors üzembe helyezés sablont használja. Hello Azure Docker Virtuálisgép-bővítmény a saját Resource Manager-sablonok is telepíthet. toodo Igen, vegye fel a hello tooyour Resource Manager-sablonok a következő, hello meghatározása *vmName* a virtuális gép megfelelően:

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
    "typeHandlerVersion": "1.1",
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

