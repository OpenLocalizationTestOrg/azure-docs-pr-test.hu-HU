---
title: "Linux virtuális gépet a MongoDB aaaInstall hello Azure CLI 1.0 |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális gépek az Azure-ban hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 4ce21a2c63da7d00a4422e0a6766e2103e7f12d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm-using-hello-azure-cli-10"></a>Hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális gépet az Azure CLI 1.0 hello
[MongoDB](http://www.mongodb.org) egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis. Ez a cikk bemutatja, hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális Gépet az Azure-ban hello Resource Manager üzembe helyezési modellben. Példák, amelyek részletesen hogyan számára:

* [Kézi telepítését és konfigurálását egy alapszintű MongoDB-példány](#manually-install-and-configure-mongodb-on-a-vm)
* [Egy alapszintű MongoDB-példány használata a Resource Manager-sablon létrehozása](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Hozzon létre egy összetett MongoDB replikával szilánkos fürt beállítja a Resource Manager-sablon használatával](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- Az Azure CLI 1.0 – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](create-cli-complete-nodejs.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Kézi telepítését és konfigurálását a MongoDB a virtuális gép
MongoDB [telepítési utasításokkal](https://docs.mongodb.com/manual/administration/install-on-linux/) a Linux disztribúciókkal, beleértve a Red Hat / CentOS, SUSE, Ubuntu és Debian. hello alábbi példa létrehoz egy *CentOS* mentve SSH-kulcsot használó virtuális gépek *~/.ssh/id_rsa.pub*. Válasz hello bekéri a tárfiók neve, a DNS-neve vagy a rendszergazdai hitelesítő adatokat:

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

Jelentkezzen be a virtuális gép toohello hello jelenik meg a virtuális gép létrehozását lépést megelőző hello hello végén nyilvános IP-cím használata:

```bash
ssh azureuser@40.78.23.145
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

Alapértelmezés szerint SELinux megvalósul CentOS képek, amely megakadályozza a MongoDB-hozzáférése. Csoportházirend felügyeleti eszközök telepítése, és az alapértelmezett TCP-port 27017 SELinux tooallow MongoDB toooperate az alábbiak szerint konfigurálja. 

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
Egy Azure gyors üzembe helyezés sablont a Githubból a következő hello segítségével CentOS virtuális létrehozhat egy alapszintű MongoDB-példány. Ez a sablon Linux tooadd hello egyéni parancsprogramok futtatására szolgáló bővítmény használ egy `yum` tárház tooyour CentOS virtuális gép található, újonnan létrehozott, és telepítse a mongodb-Protokolltámogatással.

* [Alapszintű MongoDB-példány a CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

hello alábbi példa létrehoz egy erőforráscsoport hello nevű `myResourceGroup` a hello `eastus` régióban. Adja meg a saját értékek a következők szerint:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> hello Azure CLI-t adja vissza, tooa kérdés létrehozásának hello telepítési, de hello telepítési néhány másodpercen belül, és konfigurációs tart, néhány perc toocomplete. Hello hello központi telepítés állapotának ellenőrzéséhez `azure group deployment show myResourceGroup`, írja be az erőforráscsoport neve hello ennek megfelelően. Várjon, amíg hello **ProvisioningState** látható *sikeres* közben tooSSH toohello VM előtt.

Hello üzembe helyezés befejeződik, virtuális gép SSH toohello. A virtuális gép hello segítségével hello IP-cím lekérésére `azure vm show` hasonlóan hello például a következő parancsot:

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

Hello kimeneti hello végéhez hello nyilvános IP-cím akkor jelenik meg. SSH tooyour virtuális gép a virtuális gép hello IP-címmel:

```bash
ssh azureuser@138.91.149.74
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

hello alábbi példa létrehoz egy erőforráscsoport hello nevű *myResourceGroup* a hello *eastus* régióban. Adja meg a saját értékek a következők szerint:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> hello Azure CLI-t adja vissza, tooa kérdés hello központi telepítés létrehozása néhány másodpercen belül, de hello telepítés és konfigurálás is igénybe vehet egy óra toocomplete keresztül. Hello hello központi telepítés állapotának ellenőrzéséhez `azure group deployment show myResourceGroup`, ennek megfelelően hangolását hello az erőforráscsoport nevét. Várjon, amíg hello **ProvisioningState** látható *sikeres* toohello virtuális gépek csatlakoztatása előtt.


## <a name="next-steps"></a>Következő lépések
Ezekben a példákban csatlakozás toohello MongoDB-példány helyileg a virtuális gép hello. Ha azt szeretné, hogy tooconnect toohello MongoDB-példány egy másik virtuális gép vagy a hálózatról, ellenőrizze a megfelelő hello [jönnek létre a hálózati biztonsági csoportszabályok](nsg-quickstart.md).

Sablonok használata létrehozásával kapcsolatos további információkért lásd: hello [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).

hello Azure Resource Manager-sablonok hello egyéni parancsprogramok futtatására szolgáló bővítmény toodownload használja, és parancsfájlok végrehajtása a virtuális gépeken. További információkért lásd: [Using hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény Linux virtuális gépek](extensions-customscript.md).

