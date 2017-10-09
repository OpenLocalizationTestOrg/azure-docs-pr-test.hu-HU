---
title: "az Azure-ban Docker számítógép üzemelteti aaaCreate Docker |} Microsoft Docs"
description: "Docker gép toocreate docker-gazdagépekből az Azure használatát ismerteti."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: fbf67e8189bbf33f874c4a9b619a931f28ccee12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a><span data-ttu-id="9f47c-103">Docker gazdagépek létrehozása az Azure-ban a docker-machine segítségével</span><span class="sxs-lookup"><span data-stu-id="9f47c-103">Create Docker Hosts in Azure with Docker-Machine</span></span>
<span data-ttu-id="9f47c-104">Futó [Docker](https://www.docker.com/) tárolók igényel a gazdagép virtuális gép futó hello docker démon.</span><span class="sxs-lookup"><span data-stu-id="9f47c-104">Running [Docker](https://www.docker.com/) containers requires a host VM running hello docker daemon.</span></span>
<span data-ttu-id="9f47c-105">Ez a témakör ismerteti, hogyan toouse hello [docker-gép](https://docs.docker.com/machine/) toocreate új Linux virtuális gépek, konfigurálva hello Docker démon, Azure-ban futó parancsot.</span><span class="sxs-lookup"><span data-stu-id="9f47c-105">This topic describes how toouse hello [docker-machine](https://docs.docker.com/machine/) command toocreate new Linux VMs, configured with hello Docker daemon, running in Azure.</span></span> 

<span data-ttu-id="9f47c-106">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="9f47c-106">**Note:**</span></span> 

* <span data-ttu-id="9f47c-107">*Ez a cikk docker-gép verziójának 0.9.0-rc2, vagy nagyobb függ.*</span><span class="sxs-lookup"><span data-stu-id="9f47c-107">*This article depends on docker-machine version 0.9.0-rc2 or greater*</span></span>
* <span data-ttu-id="9f47c-108">*Windows-tárolók docker-gép közelében jövőbeli hello keresztül támogatják*</span><span class="sxs-lookup"><span data-stu-id="9f47c-108">*Windows Containers will be supported through docker-machine in hello near future*</span></span>

## <a name="create-vms-with-docker-machine"></a><span data-ttu-id="9f47c-109">Hozzon létre virtuális gépek Docker gép</span><span class="sxs-lookup"><span data-stu-id="9f47c-109">Create VMs with Docker Machine</span></span>
<span data-ttu-id="9f47c-110">Létrehozott egy docker állomás virtuális gépet az Azure-ban hello `docker-machine create` parancs használatával hello `azure` illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="9f47c-110">Create docker host VMs in Azure with hello `docker-machine create` command using hello `azure` driver.</span></span> 

<span data-ttu-id="9f47c-111">hello Azure illesztőprogram igényel az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="9f47c-111">hello Azure driver requires your subscription ID.</span></span> <span data-ttu-id="9f47c-112">Használhatja a hello [Azure CLI](cli-install-nodejs.md) vagy hello [Azure Portal](https://portal.azure.com) tooretrieve az Azure-előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="9f47c-112">You can use hello [Azure CLI](cli-install-nodejs.md) or hello [Azure Portal](https://portal.azure.com) tooretrieve your Azure Subscription.</span></span> 

<span data-ttu-id="9f47c-113">**Hello Azure portál használatával**</span><span class="sxs-lookup"><span data-stu-id="9f47c-113">**Using hello Azure Portal**</span></span>

* <span data-ttu-id="9f47c-114">Válassza ki **előfizetések** a hello bal oldali navigációs lapjáról, és másolja hello előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="9f47c-114">Select **Subscriptions** from hello left navigation page and copy hello subscription id.</span></span>

<span data-ttu-id="9f47c-115">**Hello Azure parancssori felület használatával**</span><span class="sxs-lookup"><span data-stu-id="9f47c-115">**Using hello Azure CLI**</span></span>

* <span data-ttu-id="9f47c-116">Típus ```azure account list``` és másolási hello előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="9f47c-116">Type ```azure account list``` and copy hello subscription id.</span></span>

<span data-ttu-id="9f47c-117">Típus `docker-machine create --driver azure` toosee hello beállítások és az alapértelmezett értékekre.</span><span class="sxs-lookup"><span data-stu-id="9f47c-117">Type `docker-machine create --driver azure` toosee hello options and their default values.</span></span>
<span data-ttu-id="9f47c-118">Azt is láthatja, hello [Docker Azure illesztőprogram dokumentáció](https://docs.docker.com/machine/drivers/azure/) vonatkozó információ.</span><span class="sxs-lookup"><span data-stu-id="9f47c-118">You can also see hello [Docker Azure Driver documentation](https://docs.docker.com/machine/drivers/azure/) for more info.</span></span> 

<span data-ttu-id="9f47c-119">hello alábbi példa alapul hello [alapértelmezett értékek](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), de igény szerint állítsa be ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="9f47c-119">hello following example relies upon hello [default values](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), but it does optionally set these values:</span></span> 

* <span data-ttu-id="9f47c-120">Azure-dns hello nyilvános IP-cím társított hello név és generált tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="9f47c-120">azure-dns for hello name associated with hello public IP and certificates generated.</span></span> <span data-ttu-id="9f47c-121">Ez az a virtuális gép hello DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="9f47c-121">This is hello DNS name of your virtual machine.</span></span> <span data-ttu-id="9f47c-122">hello virtuális gép ezután biztonságosan állítsa le, kiadási hello dinamikus IP, és adja meg a hello képességét tooreconnect hello vm újra egy új IP-elindulása után.</span><span class="sxs-lookup"><span data-stu-id="9f47c-122">hello VM can then safely stop, release hello dynamic IP, and provide hello ability tooreconnect after hello vm starts again with a new IP.</span></span> <span data-ttu-id="9f47c-123">hello előtagja UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com régióban egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9f47c-123">hello name prefix must be unique for that region  UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span></span>
* <span data-ttu-id="9f47c-124">Nyissa meg a 80-as portot a virtuális gép hello a kimenő internet-hozzáféréséhez</span><span class="sxs-lookup"><span data-stu-id="9f47c-124">open port 80 on hello VM for outbound internet access</span></span>
* <span data-ttu-id="9f47c-125">prémium szintű tároló gyorsabb VM tooutilize hello mérete</span><span class="sxs-lookup"><span data-stu-id="9f47c-125">size of hello VM tooutilize faster premium storage</span></span>
* <span data-ttu-id="9f47c-126">prémium szintű storage használt hello méretű lemez</span><span class="sxs-lookup"><span data-stu-id="9f47c-126">premium storage used for hello vm disk</span></span>

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a><span data-ttu-id="9f47c-127">A docker-gép docker állomás</span><span class="sxs-lookup"><span data-stu-id="9f47c-127">Choose a docker host with docker-machine</span></span>
<span data-ttu-id="9f47c-128">Miután egy bejegyzést a docker-gépen ahhoz, hogy a gazdagép, hello alapértelmezett gazdagép docker parancsok futtatásakor állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="9f47c-128">Once you have an entry in docker-machine for your host, you can set hello default host when running docker commands.</span></span>

## <a name="using-powershell"></a><span data-ttu-id="9f47c-129">A PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="9f47c-129">Using PowerShell</span></span>
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a><span data-ttu-id="9f47c-130">Bash használatával</span><span class="sxs-lookup"><span data-stu-id="9f47c-130">Using Bash</span></span>
```bash
eval $(docker-machine env MyDockerHost)
```

<span data-ttu-id="9f47c-131">Most futtathatja docker parancsok hello megadott állomás ellen</span><span class="sxs-lookup"><span data-stu-id="9f47c-131">You can now run docker commands against hello specified host</span></span>

```
docker ps
docker info
```

## <a name="run-a-container"></a><span data-ttu-id="9f47c-132">Egy tároló futtatása</span><span class="sxs-lookup"><span data-stu-id="9f47c-132">Run a container</span></span>
<span data-ttu-id="9f47c-133">Egy konfigurált gazdagéppel most futtathatja egy egyszerű web server tootest hogy helyesen lett-e konfigurálva a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="9f47c-133">With a host configured, you can now run a simple web server tootest whether your host was configured correctly.</span></span>
<span data-ttu-id="9f47c-134">Itt azt a szabványos nginx-lemezkép használatához adja meg, hogy kell-e figyelni 80-as porton, és, hogy ha hello állomás virtuális gép újraindul, hello tárolót fog újraindul, valamint (`--restart=always`).</span><span class="sxs-lookup"><span data-stu-id="9f47c-134">Here we use a standard nginx image, specify that it should listen on port 80, and that if hello host VM restarts, hello container will restart as well (`--restart=always`).</span></span> 

```bash
docker run -d -p 80:80 --restart=always nginx
```

<span data-ttu-id="9f47c-135">hello kimeneti hasonlóan kell kinéznie hello következő:</span><span class="sxs-lookup"><span data-stu-id="9f47c-135">hello output should look something like hello following:</span></span>

```
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-hello-container"></a><span data-ttu-id="9f47c-136">Hello tároló</span><span class="sxs-lookup"><span data-stu-id="9f47c-136">Test hello container</span></span>
<span data-ttu-id="9f47c-137">Vizsgálja meg a futó tárolók használatával `docker ps`:</span><span class="sxs-lookup"><span data-stu-id="9f47c-137">Examine running containers using `docker ps`:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

<span data-ttu-id="9f47c-138">És tárolót, típusú futtató toosee hello `docker-machine ip <VM name>` toofind hello IP cím tooenter hello böngészőben:</span><span class="sxs-lookup"><span data-stu-id="9f47c-138">And, toosee hello running container, type `docker-machine ip <VM name>` toofind hello IP address tooenter in hello browser:</span></span>

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Futó ngnix tároló](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a><span data-ttu-id="9f47c-140">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="9f47c-140">Summary</span></span>
<span data-ttu-id="9f47c-141">Docker-számítógéppel könnyen megadhat docker-gazdagépekből az Azure-ban a az egyes docker állomás érvényesítést.</span><span class="sxs-lookup"><span data-stu-id="9f47c-141">With docker-machine, you can easily provision docker hosts in Azure for your individual docker host validations.</span></span>
<span data-ttu-id="9f47c-142">Termelési környezetben futtató tároló, lásd: hello [Azure Tárolószolgáltatásban](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="9f47c-142">For production hosting of containers, see hello [Azure Container Service](http://aka.ms/AzureContainerService)</span></span>

<span data-ttu-id="9f47c-143">a Visual Studio .NET Core alkalmazások toodevelop lásd [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)</span><span class="sxs-lookup"><span data-stu-id="9f47c-143">toodevelop .NET Core Applications with Visual Studio, see [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)</span></span>

