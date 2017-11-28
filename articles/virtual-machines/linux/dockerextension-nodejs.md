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
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension-with-hello-azure-cli-10"></a><span data-ttu-id="3fcfb-103">Hozzon létre egy Docker-környezetet a Docker Virtuálisgép-bővítmény hello használata hello Azure CLI 1.0 Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3fcfb-103">Create a Docker environment in Azure using hello Docker VM extension with hello Azure CLI 1.0</span></span>
<span data-ttu-id="3fcfb-104">Docker egy népszerű tárolóinak kezelése és a lemezkép-készítési platform, amely lehetővé teszi tooquickly együttműködnek a tárolók a Linux (és a Windows is).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-104">Docker is a popular container management and imaging platform that allows you tooquickly work with containers on Linux (and Windows as well).</span></span> <span data-ttu-id="3fcfb-105">Az Azure különböző módja van Docker tooyour igényei szerint telepítheti.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-105">In Azure, there are various ways you can deploy Docker according tooyour needs.</span></span> <span data-ttu-id="3fcfb-106">Ez a cikk foglalkozik a hello Docker Virtuálisgép-bővítmény és az Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-106">This article focuses on using hello Docker VM extension and Azure Resource Manager templates.</span></span> 

<span data-ttu-id="3fcfb-107">Hello különböző központi telepítési módszer, beleértve a Docker gép és az Azure tárolószolgáltatások, további információ: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-107">For more information about hello different deployment methods, including using Docker Machine and Azure Container Services, see hello following articles:</span></span>

* <span data-ttu-id="3fcfb-108">tooquickly típusnak egy alkalmazást, hozhat létre egy Docker állomáshoz használatával [Docker gép](docker-machine.md).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-108">tooquickly prototype an app, you can create a single Docker host using [Docker Machine](docker-machine.md).</span></span>
* <span data-ttu-id="3fcfb-109">Nagyobb, stabilabb környezetben használható hello Azure Docker Virtuálisgép-bővítmény, amely is támogatja a [Docker Compose](https://docs.docker.com/compose/overview/) toogenerate konzisztens tároló központi telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-109">For larger, more stable environments, you can use hello Azure Docker VM extension, which also supports [Docker Compose](https://docs.docker.com/compose/overview/) toogenerate consistent container deployments.</span></span> <span data-ttu-id="3fcfb-110">Ez a cikk adatokat hello Azure Docker Virtuálisgép-bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-110">This article details using hello Azure Docker VM extension.</span></span>
* <span data-ttu-id="3fcfb-111">toobuild éles használatra kész, méretezhető környezetek, amelyek további ütemezés és a felügyeleti eszközök biztosítanak, telepíthet egy [Docker Swarm-fürt a Azure-tároló szolgáltatásokra](../../container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-111">toobuild production-ready, scalable environments that provide additional scheduling and management tools, you can deploy a [Docker Swarm cluster on Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="3fcfb-112">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="3fcfb-112">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="3fcfb-113">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-113">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="3fcfb-114">[Az Azure CLI 1.0](#azure-docker-vm-extension-overview) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="3fcfb-114">[Azure CLI 1.0](#azure-docker-vm-extension-overview) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="3fcfb-115">[Az Azure CLI 2.0](dockerextension.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="3fcfb-115">[Azure CLI 2.0](dockerextension.md) - our next generation CLI for hello resource management deployment model</span></span> 

## <a name="azure-docker-vm-extension-overview"></a><span data-ttu-id="3fcfb-116">Az Azure Docker Virtuálisgép-bővítmény áttekintése</span><span class="sxs-lookup"><span data-stu-id="3fcfb-116">Azure Docker VM extension overview</span></span>
<span data-ttu-id="3fcfb-117">hello Azure Docker Virtuálisgép-bővítmény telepíti, és konfigurálja a hello Docker démon, a Docker-ügyfél és a Docker Compose a Linux virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-117">hello Azure Docker VM extension installs and configures hello Docker daemon, Docker client, and Docker Compose in your Linux virtual machine (VM).</span></span> <span data-ttu-id="3fcfb-118">Hello Azure Docker Virtuálisgép-bővítmény használatával, hogy további vezérlő és szolgáltatásokkal, mint egyszerűen Docker számítógépet használja, vagy hozzon létre hello Docker állomás, saját magának.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-118">By using hello Azure Docker VM extension, you have more control and features than simply using Docker Machine or creating hello Docker host yourself.</span></span> <span data-ttu-id="3fcfb-119">Ezek további szolgáltatásokat, például a [Docker Compose](https://docs.docker.com/compose/overview/), ellenőrizze a hello Azure Docker Virtuálisgép-bővítmény alkalmas robusztusabb fejlesztői vagy üzemi környezetben való.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-119">These additional features, such as [Docker Compose](https://docs.docker.com/compose/overview/), make hello Azure Docker VM extension suited for more robust developer or production environments.</span></span>

<span data-ttu-id="3fcfb-120">Az Azure Resource Manager-sablonok a környezet hello struktúra határozza meg.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-120">Azure Resource Manager templates define hello entire structure of your environment.</span></span> <span data-ttu-id="3fcfb-121">Lehetővé teszi toocreate és erőforrások, például a hello Docker állomás virtuális gépek, a tárolás, a szerepköralapú hozzáférés-vezérlést (RBAC) és a diagnosztika konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-121">Templates allow you toocreate and configure resources such as hello Docker host VMs, storage, Role-Based Access Controls (RBAC), and diagnostics.</span></span> <span data-ttu-id="3fcfb-122">A sablonok toocreate további központi telepítéseket egységes módon is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-122">You can reuse these templates toocreate additional deployments in a consistent manner.</span></span> <span data-ttu-id="3fcfb-123">További információ az Azure Resource Manager és a sablonok: [Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-123">For more information about Azure Resource Manager and templates, see [Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span> 

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a><span data-ttu-id="3fcfb-124">A sablont a hello Azure Docker Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="3fcfb-124">Deploy a template with hello Azure Docker VM extension</span></span>
<span data-ttu-id="3fcfb-125">Most egy meglévő gyors üzembe helyezés sablon toocreate Ubuntu virtuális gép által használt hello Azure Docker Virtuálisgép-bővítmény tooinstall használja, és hello Docker állomás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-125">Let's use an existing quickstart template toocreate an Ubuntu VM that uses hello Azure Docker VM extension tooinstall and configure hello Docker host.</span></span> <span data-ttu-id="3fcfb-126">Hello sablon Itt tekintheti meg: [egyszerű Ubuntu virtuális gép a Docker-telepítés](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-126">You can view hello template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="3fcfb-127">Hello kell [Azure CLI legújabb](../../cli-install-nodejs.md) telepítve, és bejelentkezett használatával hello Resource Manager módra az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-127">You need hello [latest Azure CLI](../../cli-install-nodejs.md) installed and logged in using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="3fcfb-128">Azure CLI hello sablon URI megadása hello segítségével hello sablon üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-128">Deploy hello template using hello Azure CLI, specifying hello template URI.</span></span> <span data-ttu-id="3fcfb-129">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westus* helyét.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-129">hello following example creates a resource group named *myResourceGroup* in hello *westus* location.</span></span> <span data-ttu-id="3fcfb-130">A saját erőforráscsoport-név és hely a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-130">Use your own resource group name and location as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="3fcfb-131">Hello kér tooname válaszolja meg a tárfiók, és adja meg a felhasználónevet és jelszót adjon meg egy DNS-név.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-131">Answer hello prompts tooname your storage account, provide a username and password, and provide a DNS name.</span></span> <span data-ttu-id="3fcfb-132">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-132">hello output is similar toohello following example:</span></span>

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

<span data-ttu-id="3fcfb-133">hello Azure CLI-t adja vissza, toohello kérdés csak néhány másodperc múlva, de a Docker állomás létrehozása még folyamatban van, és a konfigurált hello Azure Docker Virtuálisgép-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-133">hello Azure CLI returns you toohello prompt after only a few seconds, but your Docker host is still being created and configured by hello Azure Docker VM extension.</span></span> <span data-ttu-id="3fcfb-134">Hello telepítési toofinish néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-134">It takes a few minutes for hello deployment toofinish.</span></span> <span data-ttu-id="3fcfb-135">Hello segítségével hello Docker gazdagép állapotával kapcsolatos részleteket `azure vm show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-135">You can view details about hello Docker host status using hello `azure vm show` command.</span></span>

<span data-ttu-id="3fcfb-136">hello alábbi példa ellenőrzi hello nevű virtuális gép állapotát hello *myDockerVM* (alapértelmezett neve hello sablonból hello – ne módosítsa a nevet) nevű hello erőforráscsoportban *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-136">hello following example checks hello status of hello VM named *myDockerVM* (hello default name from hello template - don't change this name) in hello resource group named *myResourceGroup*.</span></span> <span data-ttu-id="3fcfb-137">Adja meg a hello az előző lépésben létrehozott hello erőforráscsoport hello nevét:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-137">Enter hello name of hello resource group you created in hello preceding step:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

<span data-ttu-id="3fcfb-138">hello kimenete hello `azure vm show` parancs van a hasonló toohello következő példát:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-138">hello output of hello `azure vm show` command is similar toohello following example:</span></span>

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

<span data-ttu-id="3fcfb-139">Tetején hello hello kimeneti, lásd a hello **ProvisioningState** a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-139">Near hello top of hello output, you see hello **ProvisioningState** of hello VM.</span></span> <span data-ttu-id="3fcfb-140">Amikor megjelenik *sikeres*, hello telepítés véget ért, és a virtuális gép SSH toohello is.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-140">When this displays *Succeeded*, hello deployment has finished and you can SSH toohello VM.</span></span>

<span data-ttu-id="3fcfb-141">Hello kimeneti hello vége felé *FQDN* hello teljesen minősített tartományneve a Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-141">Towards hello end of hello output, *FQDN* displays hello fully qualified domain name of your Docker host.</span></span> <span data-ttu-id="3fcfb-142">Az FQDN-je valóban használt funkciókért tooSSH tooyour Docker állomás hello hátralévő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-142">This FQDN is what you use tooSSH tooyour Docker host in hello remaining steps.</span></span>

## <a name="deploy-your-first-nginx-container"></a><span data-ttu-id="3fcfb-143">Az első nginx tároló üzembe</span><span class="sxs-lookup"><span data-stu-id="3fcfb-143">Deploy your first nginx container</span></span>
<span data-ttu-id="3fcfb-144">Egyszer hello telepítés véget ért, SSH tooyour új Docker állomás a helyi számítógépről.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-144">Once hello deployment has finished, SSH tooyour new Docker host from your local computer.</span></span> <span data-ttu-id="3fcfb-145">Adja meg a saját felhasználónév és a teljes tartománynév</span><span class="sxs-lookup"><span data-stu-id="3fcfb-145">Enter your own username and FQDN as follows:</span></span>

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="3fcfb-146">Miután bejelentkezett toohello Docker-állomás, most futtassa az egy nginx-tároló:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-146">Once logged in toohello Docker host, let's run an nginx container:</span></span>

```bash
sudo docker run -d -p 80:80 nginx
```

<span data-ttu-id="3fcfb-147">hello kimenete a következő példa hello nginx kép letöltöttként hasonló toohello és egy tároló lépések:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-147">hello output is similar toohello following example as hello nginx image is downloaded and a container started:</span></span>

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

<span data-ttu-id="3fcfb-148">A Docker gazdagépen futó az alábbiak szerint hello tárolók hello állapotának ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-148">Check hello status of hello containers running on your Docker host as follows:</span></span>

```bash
sudo docker ps
```

<span data-ttu-id="3fcfb-149">hello kimeneti a következő példa hasonló toohello, hello nginx tároló megjelenítő fut, és a 80-as és 443-as TCP-portot és a továbbított:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-149">hello output is similar toohello following example, showing that hello nginx container is running and TCP ports 80 and 443 and being forwarded:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

<span data-ttu-id="3fcfb-150">toosee működés közben, a tároló nyisson meg egy webes böngésző össze, és írja be a Docker gazdagép hello FQDN-neve:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-150">toosee your container in action, open up a web browser and enter hello FQDN name of your Docker host:</span></span>

![Futó ngnix tároló](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a><span data-ttu-id="3fcfb-152">Az Azure Docker Virtuálisgép-bővítmény sablon hivatkozása</span><span class="sxs-lookup"><span data-stu-id="3fcfb-152">Azure Docker VM extension template reference</span></span>
<span data-ttu-id="3fcfb-153">hello előző példa egy meglévő gyors üzembe helyezés sablont használja.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-153">hello previous example uses an existing quickstart template.</span></span> <span data-ttu-id="3fcfb-154">Hello Azure Docker Virtuálisgép-bővítmény a saját Resource Manager-sablonok is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="3fcfb-154">You can also deploy hello Azure Docker VM extension with your own Resource Manager templates.</span></span> <span data-ttu-id="3fcfb-155">toodo Igen, vegye fel a hello tooyour Resource Manager-sablonok a következő, hello meghatározása *vmName* a virtuális gép megfelelően:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-155">toodo so, add hello following tooyour Resource Manager templates, defining hello *vmName* of your VM appropriately:</span></span>

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

<span data-ttu-id="3fcfb-156">Részletes útmutató Resource Manager-sablonok használatával olvasásával található [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-156">You can find more detailed walkthrough on using Resource Manager templates by reading [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3fcfb-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3fcfb-157">Next steps</span></span>
<span data-ttu-id="3fcfb-158">Kezdésként érdemes lehet túl[hello Docker démon TCP-port konfigurálása](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), megértéséhez [Docker biztonsági](https://docs.docker.com/engine/security/security/), vagy használatával tároló üzembe helyezése [Docker Compose](https://docs.docker.com/compose/overview/).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-158">You may wish too[configure hello Docker daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), understand [Docker security](https://docs.docker.com/engine/security/security/), or deploy containers using [Docker Compose](https://docs.docker.com/compose/overview/).</span></span> <span data-ttu-id="3fcfb-159">Hello Azure Docker Virtuálisgép-bővítmény maga további információkért lásd: hello [GitHub-projekt](https://github.com/Azure/azure-docker-extension/).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-159">For more information on hello Azure Docker VM Extension itself, see hello [GitHub project](https://github.com/Azure/azure-docker-extension/).</span></span>

<span data-ttu-id="3fcfb-160">Hello további Docker telepítési lehetőségek az Azure-ban kapcsolatos információkért olvassa el:</span><span class="sxs-lookup"><span data-stu-id="3fcfb-160">Read more information about hello additional Docker deployment options in Azure:</span></span>

* [<span data-ttu-id="3fcfb-161">Docker gép használata a hello Azure illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="3fcfb-161">Use Docker Machine with hello Azure driver</span></span>](docker-machine.md)  
* <span data-ttu-id="3fcfb-162">[Ismerkedés a Docker és toodefine összeállítása, és futtassa a több tároló alkalmazást egy Azure virtuális gépen a](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="3fcfb-162">[Get Started with Docker and Compose toodefine and run a multi-container application on an Azure virtual machine](docker-compose-quickstart.md).</span></span>
* [<span data-ttu-id="3fcfb-163">Az Azure Container Service-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="3fcfb-163">Deploy an Azure Container Service cluster</span></span>](../../container-service/dcos-swarm/container-service-deployment.md)

