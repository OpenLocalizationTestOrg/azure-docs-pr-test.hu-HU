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
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension"></a><span data-ttu-id="f2e76-103">Hozzon létre egy Docker-környezetet az Azure-ban hello Docker Virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="f2e76-103">Create a Docker environment in Azure using hello Docker VM extension</span></span>
<span data-ttu-id="f2e76-104">Docker egy népszerű tárolóinak kezelése és a lemezkép-készítési platform, amely lehetővé teszi a Linux tooquickly együttműködnek a tárolók.</span><span class="sxs-lookup"><span data-stu-id="f2e76-104">Docker is a popular container management and imaging platform that allows you tooquickly work with containers on Linux.</span></span> <span data-ttu-id="f2e76-105">Az Azure különböző módja van Docker tooyour igényei szerint telepítheti.</span><span class="sxs-lookup"><span data-stu-id="f2e76-105">In Azure, there are various ways you can deploy Docker according tooyour needs.</span></span> <span data-ttu-id="f2e76-106">Ez a cikk foglalkozik hello Docker Virtuálisgép-bővítmény és az Azure Resource Manager-sablonok használatáról az Azure CLI 2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="f2e76-106">This article focuses on using hello Docker VM extension and Azure Resource Manager templates with hello Azure CLI 2.0.</span></span> <span data-ttu-id="f2e76-107">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](dockerextension-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="f2e76-107">You can also perform these steps with hello [Azure CLI 1.0](dockerextension-nodejs.md).</span></span>

## <a name="azure-docker-vm-extension-overview"></a><span data-ttu-id="f2e76-108">Az Azure Docker Virtuálisgép-bővítmény áttekintése</span><span class="sxs-lookup"><span data-stu-id="f2e76-108">Azure Docker VM extension overview</span></span>
<span data-ttu-id="f2e76-109">hello Azure Docker Virtuálisgép-bővítmény telepíti, és konfigurálja a hello Docker démon, a Docker-ügyfél és a Docker Compose a Linux virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="f2e76-109">hello Azure Docker VM extension installs and configures hello Docker daemon, Docker client, and Docker Compose in your Linux virtual machine (VM).</span></span> <span data-ttu-id="f2e76-110">Hello Azure Docker Virtuálisgép-bővítmény használatával, hogy további vezérlő és szolgáltatásokkal, mint egyszerűen Docker számítógépet használja, vagy hozzon létre hello Docker állomás, saját magának.</span><span class="sxs-lookup"><span data-stu-id="f2e76-110">By using hello Azure Docker VM extension, you have more control and features than simply using Docker Machine or creating hello Docker host yourself.</span></span> <span data-ttu-id="f2e76-111">Ezek további szolgáltatásokat, például a [Docker Compose](https://docs.docker.com/compose/overview/), ellenőrizze a hello Azure Docker Virtuálisgép-bővítmény alkalmas robusztusabb fejlesztői vagy üzemi környezetben való.</span><span class="sxs-lookup"><span data-stu-id="f2e76-111">These additional features, such as [Docker Compose](https://docs.docker.com/compose/overview/), make hello Azure Docker VM extension suited for more robust developer or production environments.</span></span>

<span data-ttu-id="f2e76-112">Hello különböző központi telepítési módszer, beleértve a Docker gép és az Azure tárolószolgáltatások, további információ: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="f2e76-112">For more information about hello different deployment methods, including using Docker Machine and Azure Container Services, see hello following articles:</span></span>

* <span data-ttu-id="f2e76-113">tooquickly típusnak egy alkalmazást, hozhat létre egy Docker állomáshoz használatával [Docker gép](docker-machine.md).</span><span class="sxs-lookup"><span data-stu-id="f2e76-113">tooquickly prototype an app, you can create a single Docker host using [Docker Machine](docker-machine.md).</span></span>
* <span data-ttu-id="f2e76-114">toobuild éles használatra kész, méretezhető környezetek, amelyek további ütemezés és a felügyeleti eszközök biztosítanak, telepíthet egy [Docker Swarm-fürt a Azure-tároló szolgáltatásokra](../../container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="f2e76-114">toobuild production-ready, scalable environments that provide additional scheduling and management tools, you can deploy a [Docker Swarm cluster on Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).</span></span>

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a><span data-ttu-id="f2e76-115">A sablont a hello Azure Docker Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="f2e76-115">Deploy a template with hello Azure Docker VM extension</span></span>
<span data-ttu-id="f2e76-116">Most egy meglévő gyors üzembe helyezés sablon toocreate Ubuntu virtuális gép által használt hello Azure Docker Virtuálisgép-bővítmény tooinstall használja, és hello Docker állomás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f2e76-116">Let's use an existing quickstart template toocreate an Ubuntu VM that uses hello Azure Docker VM extension tooinstall and configure hello Docker host.</span></span> <span data-ttu-id="f2e76-117">Hello sablon Itt tekintheti meg: [egyszerű Ubuntu virtuális gép a Docker-telepítés](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="f2e76-117">You can view hello template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="f2e76-118">Hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="f2e76-118">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="f2e76-119">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="f2e76-119">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="f2e76-120">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westus* helye:</span><span class="sxs-lookup"><span data-stu-id="f2e76-120">hello following example creates a resource group named *myResourceGroup* in hello *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="f2e76-121">Ezután telepítse a virtuális gép és [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) , amely tartalmazza az Azure Docker Virtuálisgép-bővítmény hello [a Githubon az Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="f2e76-121">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes hello Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="f2e76-122">Adja meg a saját értékeit *newStorageAccountName*, *adminUsername*, *adminPassword*, és *dnsNameForPublicIP* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f2e76-122">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP* as follows:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="f2e76-123">Hello telepítési toofinish néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f2e76-123">It takes a few minutes for hello deployment toofinish.</span></span> <span data-ttu-id="f2e76-124">Hello telepítés befejeződése után [áthelyezése toonext lépéssel](#deploy-your-first-nginx-container) tooSSH tooyour virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="f2e76-124">Once hello deployment is finished, [move toonext step](#deploy-your-first-nginx-container) tooSSH tooyour VM.</span></span> 

<span data-ttu-id="f2e76-125">Opcionálisan tooinstead visszatérési felügyeletének toohello kérése és a telepítés lehetővé hello továbbra hello háttérben, adja hozzá a hello `--no-wait` parancs megelőző toohello jelzőt.</span><span class="sxs-lookup"><span data-stu-id="f2e76-125">Optionally, tooinstead return control toohello prompt and let hello deployment continue in hello background, add hello `--no-wait` flag toohello preceding command.</span></span> <span data-ttu-id="f2e76-126">Ez a folyamat lehetővé teszi a tooperform más tevékenységet hello CLI közben hello telepítési továbbra is fennáll, néhány percig.</span><span class="sxs-lookup"><span data-stu-id="f2e76-126">This process allows you tooperform other work in hello CLI while hello deployment continues for a few minutes.</span></span> 

<span data-ttu-id="f2e76-127">Majd megtekintheti az hello Docker-gazdagép állapotával kapcsolatos adatokat [az vm megjelenítése](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="f2e76-127">You can then view details about hello Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="f2e76-128">hello alábbi példa ellenőrzi hello nevű virtuális gép állapotát hello *myDockerVM* (alapértelmezett neve hello sablonból hello – ne módosítsa a nevet) nevű hello erőforráscsoportban *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="f2e76-128">hello following example checks hello status of hello VM named *myDockerVM* (hello default name from hello template - don't change this name) in hello resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="f2e76-129">Ha ez a parancs visszaadja *sikeres*, hello telepítés véget ért, és a következő lépés hello a virtuális gép SSH toohello is.</span><span class="sxs-lookup"><span data-stu-id="f2e76-129">When this command returns *Succeeded*, hello deployment has finished and you can SSH toohello VM in hello following step.</span></span>

## <a name="deploy-your-first-nginx-container"></a><span data-ttu-id="f2e76-130">Az első nginx tároló üzembe</span><span class="sxs-lookup"><span data-stu-id="f2e76-130">Deploy your first nginx container</span></span>
<span data-ttu-id="f2e76-131">tooview részleteit a virtuális Gépet, többek között a következőket hello DNS-nevét használja `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`.</span><span class="sxs-lookup"><span data-stu-id="f2e76-131">tooview details of your VM, including hello DNS name, use `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`.</span></span> <span data-ttu-id="f2e76-132">SSH tooyour új Docker üzemeltetni a helyi számítógépről az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f2e76-132">SSH tooyour new Docker host from your local computer as follows:</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="f2e76-133">Miután bejelentkezett toohello Docker-állomás, most futtassa az egy nginx-tároló:</span><span class="sxs-lookup"><span data-stu-id="f2e76-133">Once logged in toohello Docker host, let's run an nginx container:</span></span>

```bash
sudo docker run -d -p 80:80 nginx
```

<span data-ttu-id="f2e76-134">hello kimenete a következő példa hello nginx kép letöltöttként hasonló toohello és egy tároló lépések:</span><span class="sxs-lookup"><span data-stu-id="f2e76-134">hello output is similar toohello following example as hello nginx image is downloaded and a container started:</span></span>

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

<span data-ttu-id="f2e76-135">A Docker gazdagépen futó az alábbiak szerint hello tárolók hello állapotának ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="f2e76-135">Check hello status of hello containers running on your Docker host as follows:</span></span>

```bash
sudo docker ps
```

<span data-ttu-id="f2e76-136">hello kimeneti a következő példa hasonló toohello, hello nginx tároló megjelenítő fut, és a 80-as és 443-as TCP-portot és a továbbított:</span><span class="sxs-lookup"><span data-stu-id="f2e76-136">hello output is similar toohello following example, showing that hello nginx container is running and TCP ports 80 and 443 and being forwarded:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

<span data-ttu-id="f2e76-137">toosee működés közben, a tároló megnyitása legfeljebb egy webböngészőben, és írja be a Docker állomás hello DNS-neve:</span><span class="sxs-lookup"><span data-stu-id="f2e76-137">toosee your container in action, open up a web browser and enter hello DNS name of your Docker host:</span></span>

![Futó ngnix tároló](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a><span data-ttu-id="f2e76-139">Az Azure Docker Virtuálisgép-bővítmény sablon hivatkozása</span><span class="sxs-lookup"><span data-stu-id="f2e76-139">Azure Docker VM extension template reference</span></span>
<span data-ttu-id="f2e76-140">hello előző példa egy meglévő gyors üzembe helyezés sablont használja.</span><span class="sxs-lookup"><span data-stu-id="f2e76-140">hello previous example uses an existing quickstart template.</span></span> <span data-ttu-id="f2e76-141">Hello Azure Docker Virtuálisgép-bővítmény a saját Resource Manager-sablonok is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="f2e76-141">You can also deploy hello Azure Docker VM extension with your own Resource Manager templates.</span></span> <span data-ttu-id="f2e76-142">toodo Igen, vegye fel a hello tooyour Resource Manager-sablonok a következő, hello meghatározása `vmName` a virtuális gép megfelelően:</span><span class="sxs-lookup"><span data-stu-id="f2e76-142">toodo so, add hello following tooyour Resource Manager templates, defining hello `vmName` of your VM appropriately:</span></span>

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

<span data-ttu-id="f2e76-143">Részletes útmutató Resource Manager-sablonok használatával olvasásával található [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f2e76-143">You can find more detailed walkthrough on using Resource Manager templates by reading [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2e76-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2e76-144">Next steps</span></span>
<span data-ttu-id="f2e76-145">Kezdésként érdemes lehet túl[hello Docker démon TCP-port konfigurálása](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), megértéséhez [Docker biztonsági](https://docs.docker.com/engine/security/security/), vagy használatával tároló üzembe helyezése [Docker Compose](https://docs.docker.com/compose/overview/).</span><span class="sxs-lookup"><span data-stu-id="f2e76-145">You may wish too[configure hello Docker daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), understand [Docker security](https://docs.docker.com/engine/security/security/), or deploy containers using [Docker Compose](https://docs.docker.com/compose/overview/).</span></span> <span data-ttu-id="f2e76-146">Hello Azure Docker Virtuálisgép-bővítmény maga további információkért lásd: hello [GitHub-projekt](https://github.com/Azure/azure-docker-extension/).</span><span class="sxs-lookup"><span data-stu-id="f2e76-146">For more information on hello Azure Docker VM Extension itself, see hello [GitHub project](https://github.com/Azure/azure-docker-extension/).</span></span>

<span data-ttu-id="f2e76-147">Hello további Docker telepítési lehetőségek az Azure-ban kapcsolatos információkért olvassa el:</span><span class="sxs-lookup"><span data-stu-id="f2e76-147">Read more information about hello additional Docker deployment options in Azure:</span></span>

* [<span data-ttu-id="f2e76-148">Docker gép használata a hello Azure illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="f2e76-148">Use Docker Machine with hello Azure driver</span></span>](docker-machine.md)  
* <span data-ttu-id="f2e76-149">[Ismerkedés a Docker és toodefine összeállítása, és futtassa a több tároló alkalmazást egy Azure virtuális gépen a](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="f2e76-149">[Get Started with Docker and Compose toodefine and run a multi-container application on an Azure virtual machine](docker-compose-quickstart.md).</span></span>
* [<span data-ttu-id="f2e76-150">Az Azure Container Service-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="f2e76-150">Deploy an Azure Container Service cluster</span></span>](../../container-service/dcos-swarm/container-service-deployment.md)

