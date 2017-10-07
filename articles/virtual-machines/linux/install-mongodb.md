---
title: "a Linux virtuális gép hello Azure CLI és MongoDB aaaInstall |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális gép iusing hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 97a4d9913f0eeaf7b8bf15d7fc81befe538cdc8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm"></a>Hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális gép
[MongoDB](http://www.mongodb.org) egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis. Ez a cikk bemutatja, hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális gép hello Azure CLI 2.0 és. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](install-mongodb-nodejs.md). Példák, amelyek részletesen hogyan számára:

* [Kézi telepítését és konfigurálását egy alapszintű MongoDB-példány](#manually-install-and-configure-mongodb-on-a-vm)
* [Egy alapszintű MongoDB-példány használata a Resource Manager-sablon létrehozása](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Hozzon létre egy összetett MongoDB replikával szilánkos fürt beállítja a Resource Manager-sablon használatával](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Kézi telepítését és konfigurálását a MongoDB a virtuális gép
MongoDB [telepítési utasításokkal](https://docs.mongodb.com/manual/administration/install-on-linux/) a Linux disztribúciókkal, beleértve a Red Hat / CentOS, SUSE, Ubuntu és Debian. hello alábbi példa létrehoz egy *CentOS* virtuális gép. toocreate ebben a környezetben hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal. hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal. hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* nevű felhasználóval történő *azureuser* SSH nyilvános kulcsos hitelesítés használatával

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH toohello saját felhasználónévvel és hello VM `publicIpAddress` hello kimeneti hello előző lépésben szereplő:

```bash
ssh azureuser@<publicIpAddress>
```

tooadd hello telepítési források mongodb, hozzon létre egy **yum** tárház fájlt az alábbiak szerint:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

Nyissa meg a hello MongoDB tárház fájlt szerkesztésre. Adja hozzá az alábbi hello:

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

Telepítse a MongoDB használatával **yum** az alábbiak szerint:

```bash
sudo yum install -y mongodb-org
```

Alapértelmezés szerint SELinux megvalósul CentOS képek, amely megakadályozza a MongoDB-hozzáférése. Csoportházirend-kezelés eszközei a telepítésével és beállításával SELinux tooallow MongoDB toooperate az alapértelmezett TCP-port 27017 az alábbiak szerint:

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Indítsa el hello MongoDB szolgáltatást az alábbiak szerint:

```bash
sudo service mongod start
```

Ellenőrizze hello MongoDB telepítési helyi hello segítségével `mongo` ügyfél:

```bash
mongo
```

Bizonyos adatokat, és majd a Keresés most tesztelheti a hello MongoDB-példány:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Ha szükséges, MongoDB toostart automatikus konfigurálásához a rendszer újraindítása közben:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Alapszintű MongoDB-példány létrehozása sablon használatával CentOS
Egy Azure gyors üzembe helyezés sablont a Githubból a következő hello segítségével CentOS virtuális létrehozhat egy alapszintű MongoDB-példány. Ez a sablon Linux tooadd hello egyéni parancsprogramok futtatására szolgáló bővítmény használ egy **yum** tárház tooyour CentOS virtuális gép található, újonnan létrehozott, és telepítse a mongodb-Protokolltámogatással.

* [Alapszintű MongoDB-példány a CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

toocreate ebben a környezetben hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login). Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ezután telepítse a MongoDB sablon hello [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create). Adja meg a saját erőforrás nevét, és ahol szükséges, az ilyen méretű *newStorageAccountName*, *virtualNetworkName*, és *vmSize*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"},
    "virtualNetworkName": {"value": "myVnet"},
    "vmSize": {"value": "Standard_DS2_v2"},
    "vmName": {"value": "myVM"},
    "publicIPAddressName": {"value": "myPublicIP"},
    "nicName": {"value": "myNic"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

Jelentkezzen be a virtuális gép toohello hello nyilvános DNS-címét a virtuális gép használ. Megtekintheti a hello nyilvános DNS-cím [az vm megjelenítése](/cli/azure/vm#show):

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

SSH tooyour VM saját felhasználónévvel és a nyilvános DNS-címét:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Ellenőrizze hello MongoDB telepítési helyi hello segítségével `mongo` ügyfél az alábbiak szerint:

```bash
mongo
```

Most tesztfiókok hello hozzáadása bizonyos adatokat, majd keresse az alábbiak szerint:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>A sablon használatával CentOS összetett MongoDB szilánkos fürt létrehozása
Létrehozhat egy összetett MongoDB szilánkos fürtöt hello Azure gyors üzembe helyezés sablont a Githubból a következő. Ez a sablon a következő hello [MongoDB szilánkos fürt gyakorlati tanácsok](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundancia és a magas rendelkezésre állású. hello sablon két szilánkok, az egyes replika három csomópontot hoz létre. Egy konfigurációs kiszolgálót replikakészlethez három csomópontot is létrejön, és két **mongos** útválasztó kiszolgálók tooprovide konzisztencia tooapplications a hello szilánkok között.

* [MongoDB horizontális fürt a CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Ez összetett szilánkos MongoDB-fürt telepítése kell lennie, több mint 20 mag, amely általában hello alapértelmezett magok száma régiónként az előfizetéshez. Nyissa meg az Azure támogatási kérelem tooincrease a magok száma.

toocreate ebben a környezetben hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login). Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ezután telepítse a MongoDB sablon hello [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create). Adja meg a saját erőforrás nevét, és ahol szükséges, az ilyen méretű *mongoAdminUsername*, *sizeOfDataDiskInGB*, és *configNodeVmSize*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "mongoAdminUsername": {"value": "mongoadmin"},
    "mongoAdminPassword": {"value": "P@ssw0rd!"},
    "dnsNamePrefix": {"value": "mypublicdns"},
    "environment": {"value": "AzureCloud"},
    "numDataDisks": {"value": "4"},
    "sizeOfDataDiskInGB": {"value": 20},
    "centOsVersion": {"value": "7.0"},
    "routerNodeVmSize": {"value": "Standard_DS3_v2"},
    "configNodeVmSize": {"value": "Standard_DS3_v2"},
    "replicaNodeVmSize": {"value": "Standard_DS3_v2"},
    "zabbixServerIPAddress": {"value": "Null"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json \
  --name myMongoDBCluster \
  --no-wait
```

A központi telepítés akár egy órát toodeploy eltarthat, és minden hello Virtuálisgép-példány konfigurálása. Hello `--no-wait` jelző hello végén lévő megelőző tooreturn vezérlő toohello parancs parancssorba, miután hello sablon-üzembehelyezés elfogadta hello Azure platformon hello használata. Ezután megtekinthet hello telepítési állapotát a [az csoport telepítési megjelenítése](/cli/azure/group/deployment#show). hello alábbi példa nézetek hello hello állapota *myMongoDBCluster* hello telepítésének *myResourceGroup* erőforráscsoport:

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a>Következő lépések
Ezekben a példákban csatlakozás toohello MongoDB-példány helyileg a virtuális gép hello. Ha azt szeretné, hogy tooconnect toohello MongoDB-példány egy másik virtuális gép vagy a hálózatról, ellenőrizze a megfelelő hello [jönnek létre a hálózati biztonsági csoportszabályok](nsg-quickstart.md).

Ezek a példák telepítése hello core MongoDB környezet fejlesztési célokra szolgál. Alkalmazza a szükséges hello biztonsági konfigurációs beállításokat a környezetéhez. További információkért lásd: hello [MongoDB biztonsági docs](https://docs.mongodb.com/manual/security/).

Sablonok használata létrehozásával kapcsolatos további információkért lásd: hello [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).

hello Azure Resource Manager-sablonok hello egyéni parancsprogramok futtatására szolgáló bővítmény toodownload használja, és parancsfájlok végrehajtása a virtuális gépeken. További információkért lásd: [Using hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény Linux virtuális gépek](extensions-customscript.md).

