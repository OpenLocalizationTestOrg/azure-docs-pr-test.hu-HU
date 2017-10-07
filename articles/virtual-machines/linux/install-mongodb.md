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
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm"></a><span data-ttu-id="4a9f6-103">Hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="4a9f6-103">How tooinstall and configure MongoDB on a Linux VM</span></span>
<span data-ttu-id="4a9f6-104">[MongoDB](http://www.mongodb.org) egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="4a9f6-105">Ez a cikk bemutatja, hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális gép hello Azure CLI 2.0 és.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-105">This article shows you how tooinstall and configure MongoDB on a Linux VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="4a9f6-106">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](install-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-106">You can also perform these steps with hello [Azure CLI 1.0](install-mongodb-nodejs.md).</span></span> <span data-ttu-id="4a9f6-107">Példák, amelyek részletesen hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-107">Examples are shown that detail how to:</span></span>

* [<span data-ttu-id="4a9f6-108">Kézi telepítését és konfigurálását egy alapszintű MongoDB-példány</span><span class="sxs-lookup"><span data-stu-id="4a9f6-108">Manually install and configure a basic MongoDB instance</span></span>](#manually-install-and-configure-mongodb-on-a-vm)
* [<span data-ttu-id="4a9f6-109">Egy alapszintű MongoDB-példány használata a Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a9f6-109">Create a basic MongoDB instance using a Resource Manager template</span></span>](#create-basic-mongodb-instance-on-centos-using-a-template)
* [<span data-ttu-id="4a9f6-110">Hozzon létre egy összetett MongoDB replikával szilánkos fürt beállítja a Resource Manager-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="4a9f6-110">Create a complex MongoDB sharded cluster with replica sets using a Resource Manager template</span></span>](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a><span data-ttu-id="4a9f6-111">Kézi telepítését és konfigurálását a MongoDB a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="4a9f6-111">Manually install and configure MongoDB on a VM</span></span>
<span data-ttu-id="4a9f6-112">MongoDB [telepítési utasításokkal](https://docs.mongodb.com/manual/administration/install-on-linux/) a Linux disztribúciókkal, beleértve a Red Hat / CentOS, SUSE, Ubuntu és Debian.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-112">MongoDB [provide installation instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) for Linux distros including Red Hat / CentOS, SUSE, Ubuntu, and Debian.</span></span> <span data-ttu-id="4a9f6-113">hello alábbi példa létrehoz egy *CentOS* virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-113">hello following example creates a *CentOS* VM.</span></span> <span data-ttu-id="4a9f6-114">toocreate ebben a környezetben hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-114">toocreate this environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="4a9f6-115">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-115">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="4a9f6-116">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-116">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="4a9f6-117">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-117">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="4a9f6-118">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* nevű felhasználóval történő *azureuser* SSH nyilvános kulcsos hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="4a9f6-118">hello following example creates a VM named *myVM* with a user named *azureuser* using SSH public key authentication</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="4a9f6-119">SSH toohello saját felhasználónévvel és hello VM `publicIpAddress` hello kimeneti hello előző lépésben szereplő:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-119">SSH toohello VM using your own username and hello `publicIpAddress` listed in hello output from hello previous step:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="4a9f6-120">tooadd hello telepítési források mongodb, hozzon létre egy **yum** tárház fájlt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-120">tooadd hello installation sources for MongoDB, create a **yum** repository file as follows:</span></span>

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

<span data-ttu-id="4a9f6-121">Nyissa meg a hello MongoDB tárház fájlt szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-121">Open hello MongoDB repo file for editing.</span></span> <span data-ttu-id="4a9f6-122">Adja hozzá az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-122">Add hello following lines:</span></span>

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

<span data-ttu-id="4a9f6-123">Telepítse a MongoDB használatával **yum** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-123">Install MongoDB using **yum** as follows:</span></span>

```bash
sudo yum install -y mongodb-org
```

<span data-ttu-id="4a9f6-124">Alapértelmezés szerint SELinux megvalósul CentOS képek, amely megakadályozza a MongoDB-hozzáférése.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-124">By default, SELinux is enforced on CentOS images that prevents you from accessing MongoDB.</span></span> <span data-ttu-id="4a9f6-125">Csoportházirend-kezelés eszközei a telepítésével és beállításával SELinux tooallow MongoDB toooperate az alapértelmezett TCP-port 27017 az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-125">Install policy management tools and configure SELinux tooallow MongoDB toooperate on its default TCP port 27017 as follows:</span></span>

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

<span data-ttu-id="4a9f6-126">Indítsa el hello MongoDB szolgáltatást az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-126">Start hello MongoDB service as follows:</span></span>

```bash
sudo service mongod start
```

<span data-ttu-id="4a9f6-127">Ellenőrizze hello MongoDB telepítési helyi hello segítségével `mongo` ügyfél:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-127">Verify hello MongoDB installation by connecting using hello local `mongo` client:</span></span>

```bash
mongo
```

<span data-ttu-id="4a9f6-128">Bizonyos adatokat, és majd a Keresés most tesztelheti a hello MongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-128">Now test hello MongoDB instance by adding some data and then searching:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

<span data-ttu-id="4a9f6-129">Ha szükséges, MongoDB toostart automatikus konfigurálásához a rendszer újraindítása közben:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-129">If desired, configure MongoDB toostart automatically during a system reboot:</span></span>

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a><span data-ttu-id="4a9f6-130">Alapszintű MongoDB-példány létrehozása sablon használatával CentOS</span><span class="sxs-lookup"><span data-stu-id="4a9f6-130">Create basic MongoDB instance on CentOS using a template</span></span>
<span data-ttu-id="4a9f6-131">Egy Azure gyors üzembe helyezés sablont a Githubból a következő hello segítségével CentOS virtuális létrehozhat egy alapszintű MongoDB-példány.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-131">You can create a basic MongoDB instance on a single CentOS VM using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="4a9f6-132">Ez a sablon Linux tooadd hello egyéni parancsprogramok futtatására szolgáló bővítmény használ egy **yum** tárház tooyour CentOS virtuális gép található, újonnan létrehozott, és telepítse a mongodb-Protokolltámogatással.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-132">This template uses hello Custom Script extension for Linux tooadd a **yum** repository tooyour newly created CentOS VM and then install MongoDB.</span></span>

* <span data-ttu-id="4a9f6-133">[Alapszintű MongoDB-példány a CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="4a9f6-133">[Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span></span>

<span data-ttu-id="4a9f6-134">toocreate ebben a környezetben hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-134">toocreate this environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="4a9f6-135">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-135">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="4a9f6-136">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-136">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="4a9f6-137">Ezután telepítse a MongoDB sablon hello [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-137">Next, deploy hello MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="4a9f6-138">Adja meg a saját erőforrás nevét, és ahol szükséges, az ilyen méretű *newStorageAccountName*, *virtualNetworkName*, és *vmSize*:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-138">Define your own resource names and sizes where needed such as for *newStorageAccountName*, *virtualNetworkName*, and *vmSize*:</span></span>

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

<span data-ttu-id="4a9f6-139">Jelentkezzen be a virtuális gép toohello hello nyilvános DNS-címét a virtuális gép használ.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-139">Log on toohello VM using hello public DNS address of your VM.</span></span> <span data-ttu-id="4a9f6-140">Megtekintheti a hello nyilvános DNS-cím [az vm megjelenítése](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="4a9f6-140">You can view hello public DNS address with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

<span data-ttu-id="4a9f6-141">SSH tooyour VM saját felhasználónévvel és a nyilvános DNS-címét:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-141">SSH tooyour VM using your own username and public DNS address:</span></span>

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

<span data-ttu-id="4a9f6-142">Ellenőrizze hello MongoDB telepítési helyi hello segítségével `mongo` ügyfél az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-142">Verify hello MongoDB installation by connecting using hello local `mongo` client as follows:</span></span>

```bash
mongo
```

<span data-ttu-id="4a9f6-143">Most tesztfiókok hello hozzáadása bizonyos adatokat, majd keresse az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-143">Now test hello instance by adding some data and searching as follows:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a><span data-ttu-id="4a9f6-144">A sablon használatával CentOS összetett MongoDB szilánkos fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a9f6-144">Create a complex MongoDB Sharded Cluster on CentOS using a template</span></span>
<span data-ttu-id="4a9f6-145">Létrehozhat egy összetett MongoDB szilánkos fürtöt hello Azure gyors üzembe helyezés sablont a Githubból a következő.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-145">You can create a complex MongoDB sharded cluster using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="4a9f6-146">Ez a sablon a következő hello [MongoDB szilánkos fürt gyakorlati tanácsok](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundancia és a magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-146">This template follows hello [MongoDB sharded cluster best practices](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundancy and high availability.</span></span> <span data-ttu-id="4a9f6-147">hello sablon két szilánkok, az egyes replika három csomópontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-147">hello template creates two shards, with three nodes in each replica set.</span></span> <span data-ttu-id="4a9f6-148">Egy konfigurációs kiszolgálót replikakészlethez három csomópontot is létrejön, és két **mongos** útválasztó kiszolgálók tooprovide konzisztencia tooapplications a hello szilánkok között.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-148">One config server replica set with three nodes is also created, plus two **mongos** router servers tooprovide consistency tooapplications from across hello shards.</span></span>

* <span data-ttu-id="4a9f6-149">[MongoDB horizontális fürt a CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="4a9f6-149">[MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span></span>

> [!WARNING]
> <span data-ttu-id="4a9f6-150">Ez összetett szilánkos MongoDB-fürt telepítése kell lennie, több mint 20 mag, amely általában hello alapértelmezett magok száma régiónként az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-150">Deploying this complex MongoDB sharded cluster requires more than 20 cores, which is typically hello default core count per region for a subscription.</span></span> <span data-ttu-id="4a9f6-151">Nyissa meg az Azure támogatási kérelem tooincrease a magok száma.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-151">Open an Azure support request tooincrease your core count.</span></span>

<span data-ttu-id="4a9f6-152">toocreate ebben a környezetben hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-152">toocreate this environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="4a9f6-153">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-153">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="4a9f6-154">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-154">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="4a9f6-155">Ezután telepítse a MongoDB sablon hello [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-155">Next, deploy hello MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="4a9f6-156">Adja meg a saját erőforrás nevét, és ahol szükséges, az ilyen méretű *mongoAdminUsername*, *sizeOfDataDiskInGB*, és *configNodeVmSize*:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-156">Define your own resource names and sizes where needed such as for *mongoAdminUsername*, *sizeOfDataDiskInGB*, and *configNodeVmSize*:</span></span>

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

<span data-ttu-id="4a9f6-157">A központi telepítés akár egy órát toodeploy eltarthat, és minden hello Virtuálisgép-példány konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-157">This deployment can take over an hour toodeploy and configure all hello VM instances.</span></span> <span data-ttu-id="4a9f6-158">Hello `--no-wait` jelző hello végén lévő megelőző tooreturn vezérlő toohello parancs parancssorba, miután hello sablon-üzembehelyezés elfogadta hello Azure platformon hello használata.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-158">hello `--no-wait` flag is used at hello end of hello preceding command tooreturn control toohello command prompt once hello template deployment has been accepted by hello Azure platform.</span></span> <span data-ttu-id="4a9f6-159">Ezután megtekinthet hello telepítési állapotát a [az csoport telepítési megjelenítése](/cli/azure/group/deployment#show).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-159">You can then view hello deployment status with [az group deployment show](/cli/azure/group/deployment#show).</span></span> <span data-ttu-id="4a9f6-160">hello alábbi példa nézetek hello hello állapota *myMongoDBCluster* hello telepítésének *myResourceGroup* erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="4a9f6-160">hello following example views hello status for hello *myMongoDBCluster* deployment in hello *myResourceGroup* resource group:</span></span>

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a><span data-ttu-id="4a9f6-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4a9f6-161">Next steps</span></span>
<span data-ttu-id="4a9f6-162">Ezekben a példákban csatlakozás toohello MongoDB-példány helyileg a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-162">In these examples, you connect toohello MongoDB instance locally from hello VM.</span></span> <span data-ttu-id="4a9f6-163">Ha azt szeretné, hogy tooconnect toohello MongoDB-példány egy másik virtuális gép vagy a hálózatról, ellenőrizze a megfelelő hello [jönnek létre a hálózati biztonsági csoportszabályok](nsg-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-163">If you want tooconnect toohello MongoDB instance from another VM or network, ensure hello appropriate [Network Security Group rules are created](nsg-quickstart.md).</span></span>

<span data-ttu-id="4a9f6-164">Ezek a példák telepítése hello core MongoDB környezet fejlesztési célokra szolgál.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-164">These examples deploy hello core MongoDB environment for development purposes.</span></span> <span data-ttu-id="4a9f6-165">Alkalmazza a szükséges hello biztonsági konfigurációs beállításokat a környezetéhez.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-165">Apply hello required security configuration options for your environment.</span></span> <span data-ttu-id="4a9f6-166">További információkért lásd: hello [MongoDB biztonsági docs](https://docs.mongodb.com/manual/security/).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-166">For more information, see hello [MongoDB security docs](https://docs.mongodb.com/manual/security/).</span></span>

<span data-ttu-id="4a9f6-167">Sablonok használata létrehozásával kapcsolatos további információkért lásd: hello [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-167">For more information about creating using templates, see hello [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="4a9f6-168">hello Azure Resource Manager-sablonok hello egyéni parancsprogramok futtatására szolgáló bővítmény toodownload használja, és parancsfájlok végrehajtása a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="4a9f6-168">hello Azure Resource Manager templates use hello Custom Script Extension toodownload and execute scripts on your VMs.</span></span> <span data-ttu-id="4a9f6-169">További információkért lásd: [Using hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény Linux virtuális gépek](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="4a9f6-169">For more information, see [Using hello Azure Custom Script Extension with Linux Virtual Machines](extensions-customscript.md).</span></span>

