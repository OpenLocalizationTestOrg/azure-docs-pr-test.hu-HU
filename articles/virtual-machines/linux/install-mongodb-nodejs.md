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
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm-using-hello-azure-cli-10"></a><span data-ttu-id="f2c0d-103">Hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális gépet az Azure CLI 1.0 hello</span><span class="sxs-lookup"><span data-stu-id="f2c0d-103">How tooinstall and configure MongoDB on a Linux VM using hello Azure CLI 1.0</span></span>
<span data-ttu-id="f2c0d-104">[MongoDB](http://www.mongodb.org) egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="f2c0d-105">Ez a cikk bemutatja, hogyan tooinstall és a MongoDB konfigurálása a Linux virtuális Gépet az Azure-ban hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-105">This article shows you how tooinstall and configure MongoDB on a Linux VM in Azure using hello Resource Manager deployment model.</span></span> <span data-ttu-id="f2c0d-106">Példák, amelyek részletesen hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-106">Examples are shown that detail how to:</span></span>

* [<span data-ttu-id="f2c0d-107">Kézi telepítését és konfigurálását egy alapszintű MongoDB-példány</span><span class="sxs-lookup"><span data-stu-id="f2c0d-107">Manually install and configure a basic MongoDB instance</span></span>](#manually-install-and-configure-mongodb-on-a-vm)
* [<span data-ttu-id="f2c0d-108">Egy alapszintű MongoDB-példány használata a Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2c0d-108">Create a basic MongoDB instance using a Resource Manager template</span></span>](#create-basic-mongodb-instance-on-centos-using-a-template)
* [<span data-ttu-id="f2c0d-109">Hozzon létre egy összetett MongoDB replikával szilánkos fürt beállítja a Resource Manager-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="f2c0d-109">Create a complex MongoDB sharded cluster with replica sets using a Resource Manager template</span></span>](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="f2c0d-110">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="f2c0d-110">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="f2c0d-111">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-111">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="f2c0d-112">Az Azure CLI 1.0 – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="f2c0d-112">Azure CLI 1.0 – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="f2c0d-113">[Az Azure CLI 2.0](create-cli-complete-nodejs.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="f2c0d-113">[Azure CLI 2.0](create-cli-complete-nodejs.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a><span data-ttu-id="f2c0d-114">Kézi telepítését és konfigurálását a MongoDB a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="f2c0d-114">Manually install and configure MongoDB on a VM</span></span>
<span data-ttu-id="f2c0d-115">MongoDB [telepítési utasításokkal](https://docs.mongodb.com/manual/administration/install-on-linux/) a Linux disztribúciókkal, beleértve a Red Hat / CentOS, SUSE, Ubuntu és Debian.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-115">MongoDB [provide installation instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) for Linux distros including Red Hat / CentOS, SUSE, Ubuntu, and Debian.</span></span> <span data-ttu-id="f2c0d-116">hello alábbi példa létrehoz egy *CentOS* mentve SSH-kulcsot használó virtuális gépek *~/.ssh/id_rsa.pub*.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-116">hello following example creates a *CentOS* VM using an SSH key stored at *~/.ssh/id_rsa.pub*.</span></span> <span data-ttu-id="f2c0d-117">Válasz hello bekéri a tárfiók neve, a DNS-neve vagy a rendszergazdai hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-117">Answer hello prompts for storage account name, DNS name, and admin credentials:</span></span>

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

<span data-ttu-id="f2c0d-118">Jelentkezzen be a virtuális gép toohello hello jelenik meg a virtuális gép létrehozását lépést megelőző hello hello végén nyilvános IP-cím használata:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-118">Log on toohello VM using hello public IP address displayed at hello end of hello preceding VM creation step:</span></span>

```bash
ssh azureuser@40.78.23.145
```

<span data-ttu-id="f2c0d-119">tooadd hello telepítési források mongodb, hozzon létre egy **yum** tárház fájlt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-119">tooadd hello installation sources for MongoDB, create a **yum** repository file as follows:</span></span>

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

<span data-ttu-id="f2c0d-120">Nyissa meg a hello MongoDB tárház fájlt szerkesztésre.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-120">Open hello MongoDB repo file for editing.</span></span> <span data-ttu-id="f2c0d-121">Adja hozzá az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-121">Add hello following lines:</span></span>

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

<span data-ttu-id="f2c0d-122">Telepítse a MongoDB használatával **yum** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-122">Install MongoDB using **yum** as follows:</span></span>

```bash
sudo yum install -y mongodb-org
```

<span data-ttu-id="f2c0d-123">Alapértelmezés szerint SELinux megvalósul CentOS képek, amely megakadályozza a MongoDB-hozzáférése.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-123">By default, SELinux is enforced on CentOS images that prevents you from accessing MongoDB.</span></span> <span data-ttu-id="f2c0d-124">Csoportházirend felügyeleti eszközök telepítése, és az alapértelmezett TCP-port 27017 SELinux tooallow MongoDB toooperate az alábbiak szerint konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-124">Install policy management tools and configure SELinux tooallow MongoDB toooperate on its default TCP port 27017 as follows.</span></span> 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

<span data-ttu-id="f2c0d-125">Indítsa el hello MongoDB szolgáltatást az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-125">Start hello MongoDB service as follows:</span></span>

```bash
sudo service mongod start
```

<span data-ttu-id="f2c0d-126">Ellenőrizze hello MongoDB telepítési helyi hello segítségével `mongo` ügyfél:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-126">Verify hello MongoDB installation by connecting using hello local `mongo` client:</span></span>

```bash
mongo
```

<span data-ttu-id="f2c0d-127">Bizonyos adatokat, és majd a Keresés most tesztelheti a hello MongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-127">Now test hello MongoDB instance by adding some data and then searching:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

<span data-ttu-id="f2c0d-128">Ha szükséges, MongoDB toostart automatikus konfigurálásához a rendszer újraindítása közben:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-128">If desired, configure MongoDB toostart automatically during a system reboot:</span></span>

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a><span data-ttu-id="f2c0d-129">Alapszintű MongoDB-példány létrehozása sablon használatával CentOS</span><span class="sxs-lookup"><span data-stu-id="f2c0d-129">Create basic MongoDB instance on CentOS using a template</span></span>
<span data-ttu-id="f2c0d-130">Egy Azure gyors üzembe helyezés sablont a Githubból a következő hello segítségével CentOS virtuális létrehozhat egy alapszintű MongoDB-példány.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-130">You can create a basic MongoDB instance on a single CentOS VM using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="f2c0d-131">Ez a sablon Linux tooadd hello egyéni parancsprogramok futtatására szolgáló bővítmény használ egy `yum` tárház tooyour CentOS virtuális gép található, újonnan létrehozott, és telepítse a mongodb-Protokolltámogatással.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-131">This template uses hello Custom Script extension for Linux tooadd a `yum` repository tooyour newly created CentOS VM and then install MongoDB.</span></span>

* <span data-ttu-id="f2c0d-132">[Alapszintű MongoDB-példány a CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="f2c0d-132">[Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span></span>

<span data-ttu-id="f2c0d-133">hello alábbi példa létrehoz egy erőforráscsoport hello nevű `myResourceGroup` a hello `eastus` régióban.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-133">hello following example creates a resource group with hello name `myResourceGroup` in hello `eastus` region.</span></span> <span data-ttu-id="f2c0d-134">Adja meg a saját értékek a következők szerint:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-134">Enter your own values as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> <span data-ttu-id="f2c0d-135">hello Azure CLI-t adja vissza, tooa kérdés létrehozásának hello telepítési, de hello telepítési néhány másodpercen belül, és konfigurációs tart, néhány perc toocomplete.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-135">hello Azure CLI returns you tooa prompt within a few seconds of creating hello deployment, but hello installation and configuration takes a few minutes toocomplete.</span></span> <span data-ttu-id="f2c0d-136">Hello hello központi telepítés állapotának ellenőrzéséhez `azure group deployment show myResourceGroup`, írja be az erőforráscsoport neve hello ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-136">Check hello status of hello deployment with `azure group deployment show myResourceGroup`, entering hello name of your resource group accordingly.</span></span> <span data-ttu-id="f2c0d-137">Várjon, amíg hello **ProvisioningState** látható *sikeres* közben tooSSH toohello VM előtt.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-137">Wait until hello **ProvisioningState** shows *Succeeded* before trying tooSSH toohello VM.</span></span>

<span data-ttu-id="f2c0d-138">Hello üzembe helyezés befejeződik, virtuális gép SSH toohello.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-138">Once hello deployment is complete, SSH toohello VM.</span></span> <span data-ttu-id="f2c0d-139">A virtuális gép hello segítségével hello IP-cím lekérésére `azure vm show` hasonlóan hello például a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-139">Obtain hello IP address of your VM using hello `azure vm show` command as in hello following example:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

<span data-ttu-id="f2c0d-140">Hello kimeneti hello végéhez hello nyilvános IP-cím akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-140">Near hello end of hello output, hello public IP address is displayed.</span></span> <span data-ttu-id="f2c0d-141">SSH tooyour virtuális gép a virtuális gép hello IP-címmel:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-141">SSH tooyour VM with hello IP address of your VM:</span></span>

```bash
ssh azureuser@138.91.149.74
```

<span data-ttu-id="f2c0d-142">Ellenőrizze hello MongoDB telepítési helyi hello segítségével `mongo` ügyfél az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-142">Verify hello MongoDB installation by connecting using hello local `mongo` client as follows:</span></span>

```bash
mongo
```

<span data-ttu-id="f2c0d-143">Most tesztfiókok hello hozzáadása bizonyos adatokat, majd keresse az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-143">Now test hello instance by adding some data and searching as follows:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a><span data-ttu-id="f2c0d-144">A sablon használatával CentOS összetett MongoDB szilánkos fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2c0d-144">Create a complex MongoDB Sharded Cluster on CentOS using a template</span></span>
<span data-ttu-id="f2c0d-145">Létrehozhat egy összetett MongoDB szilánkos fürtöt hello Azure gyors üzembe helyezés sablont a Githubból a következő.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-145">You can create a complex MongoDB sharded cluster using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="f2c0d-146">Ez a sablon a következő hello [MongoDB szilánkos fürt gyakorlati tanácsok](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundancia és a magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-146">This template follows hello [MongoDB sharded cluster best practices](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundancy and high availability.</span></span> <span data-ttu-id="f2c0d-147">hello sablon két szilánkok, az egyes replika három csomópontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-147">hello template creates two shards, with three nodes in each replica set.</span></span> <span data-ttu-id="f2c0d-148">Egy konfigurációs kiszolgálót replikakészlethez három csomópontot is létrejön, és két **mongos** útválasztó kiszolgálók tooprovide konzisztencia tooapplications a hello szilánkok között.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-148">One config server replica set with three nodes is also created, plus two **mongos** router servers tooprovide consistency tooapplications from across hello shards.</span></span>

* <span data-ttu-id="f2c0d-149">[MongoDB horizontális fürt a CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="f2c0d-149">[MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span></span>

> [!WARNING]
> <span data-ttu-id="f2c0d-150">Ez összetett szilánkos MongoDB-fürt telepítése kell lennie, több mint 20 mag, amely általában hello alapértelmezett magok száma régiónként az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-150">Deploying this complex MongoDB sharded cluster requires more than 20 cores, which is typically hello default core count per region for a subscription.</span></span> <span data-ttu-id="f2c0d-151">Nyissa meg az Azure támogatási kérelem tooincrease a magok száma.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-151">Open an Azure support request tooincrease your core count.</span></span>

<span data-ttu-id="f2c0d-152">hello alábbi példa létrehoz egy erőforráscsoport hello nevű *myResourceGroup* a hello *eastus* régióban.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-152">hello following example creates a resource group with hello name *myResourceGroup* in hello *eastus* region.</span></span> <span data-ttu-id="f2c0d-153">Adja meg a saját értékek a következők szerint:</span><span class="sxs-lookup"><span data-stu-id="f2c0d-153">Enter your own values as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> <span data-ttu-id="f2c0d-154">hello Azure CLI-t adja vissza, tooa kérdés hello központi telepítés létrehozása néhány másodpercen belül, de hello telepítés és konfigurálás is igénybe vehet egy óra toocomplete keresztül.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-154">hello Azure CLI returns you tooa prompt within a few seconds of creating hello deployment, but hello installation and configuration can take over an hour toocomplete.</span></span> <span data-ttu-id="f2c0d-155">Hello hello központi telepítés állapotának ellenőrzéséhez `azure group deployment show myResourceGroup`, ennek megfelelően hangolását hello az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-155">Check hello status of hello deployment with `azure group deployment show myResourceGroup`, adjusting hello name of your resource group accordingly.</span></span> <span data-ttu-id="f2c0d-156">Várjon, amíg hello **ProvisioningState** látható *sikeres* toohello virtuális gépek csatlakoztatása előtt.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-156">Wait until hello **ProvisioningState** shows *Succeeded* before connecting toohello VMs.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f2c0d-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2c0d-157">Next steps</span></span>
<span data-ttu-id="f2c0d-158">Ezekben a példákban csatlakozás toohello MongoDB-példány helyileg a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-158">In these examples, you connect toohello MongoDB instance locally from hello VM.</span></span> <span data-ttu-id="f2c0d-159">Ha azt szeretné, hogy tooconnect toohello MongoDB-példány egy másik virtuális gép vagy a hálózatról, ellenőrizze a megfelelő hello [jönnek létre a hálózati biztonsági csoportszabályok](nsg-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="f2c0d-159">If you want tooconnect toohello MongoDB instance from another VM or network, ensure hello appropriate [Network Security Group rules are created](nsg-quickstart.md).</span></span>

<span data-ttu-id="f2c0d-160">Sablonok használata létrehozásával kapcsolatos további információkért lásd: hello [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f2c0d-160">For more information about creating using templates, see hello [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="f2c0d-161">hello Azure Resource Manager-sablonok hello egyéni parancsprogramok futtatására szolgáló bővítmény toodownload használja, és parancsfájlok végrehajtása a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f2c0d-161">hello Azure Resource Manager templates use hello Custom Script Extension toodownload and execute scripts on your VMs.</span></span> <span data-ttu-id="f2c0d-162">További információkért lásd: [Using hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény Linux virtuális gépek](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="f2c0d-162">For more information, see [Using hello Azure Custom Script Extension with Linux Virtual Machines](extensions-customscript.md).</span></span>

