---
title: "Docker-gazdagépekből létrehozása az Azure-ban Docker gép |} Microsoft Docs"
description: "Docker gép docker-gazdagépekből létrehozása az Azure-ban való használatát ismerteti."
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
ms.openlocfilehash: 766d327a87ed13e04166d71c3d9ae0a1e7a66d19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a><span data-ttu-id="3a0f9-103">Docker gazdagépek létrehozása az Azure-ban a docker-machine segítségével</span><span class="sxs-lookup"><span data-stu-id="3a0f9-103">Create Docker Hosts in Azure with Docker-Machine</span></span>
<span data-ttu-id="3a0f9-104">Futó [Docker](https://www.docker.com/) tárolók egy gazdagépet a docker démon futtató virtuális igényel.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-104">Running [Docker](https://www.docker.com/) containers requires a host VM running the docker daemon.</span></span>
<span data-ttu-id="3a0f9-105">Ez a témakör ismerteti, hogyan használható a [docker-gép](https://docs.docker.com/machine/) parancs futtatásával hozzon létre új Linux virtuális gépek a Docker démon, Azure-beli konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-105">This topic describes how to use the [docker-machine](https://docs.docker.com/machine/) command to create new Linux VMs, configured with the Docker daemon, running in Azure.</span></span> 

<span data-ttu-id="3a0f9-106">**Megjegyzés:**</span><span class="sxs-lookup"><span data-stu-id="3a0f9-106">**Note:**</span></span> 

* <span data-ttu-id="3a0f9-107">*Ez a cikk docker-gép verziójának 0.9.0-rc2, vagy nagyobb függ.*</span><span class="sxs-lookup"><span data-stu-id="3a0f9-107">*This article depends on docker-machine version 0.9.0-rc2 or greater*</span></span>
* <span data-ttu-id="3a0f9-108">*Windows-tárolók keresztül támogatják docker-gép a közeljövőben*</span><span class="sxs-lookup"><span data-stu-id="3a0f9-108">*Windows Containers will be supported through docker-machine in the near future*</span></span>

## <a name="create-vms-with-docker-machine"></a><span data-ttu-id="3a0f9-109">Hozzon létre virtuális gépek Docker gép</span><span class="sxs-lookup"><span data-stu-id="3a0f9-109">Create VMs with Docker Machine</span></span>
<span data-ttu-id="3a0f9-110">Létrehozott egy docker állomás virtuális gépet az Azure-ban a `docker-machine create` parancs használatával a `azure` illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-110">Create docker host VMs in Azure with the `docker-machine create` command using the `azure` driver.</span></span> 

<span data-ttu-id="3a0f9-111">Az Azure meghajtó csak az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-111">The Azure driver requires your subscription ID.</span></span> <span data-ttu-id="3a0f9-112">Használhatja a [Azure CLI](cli-install-nodejs.md) vagy a [Azure Portal](https://portal.azure.com) beolvasása az Azure-előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-112">You can use the [Azure CLI](cli-install-nodejs.md) or the [Azure Portal](https://portal.azure.com) to retrieve your Azure Subscription.</span></span> 

<span data-ttu-id="3a0f9-113">**Az Azure portál használatával**</span><span class="sxs-lookup"><span data-stu-id="3a0f9-113">**Using the Azure Portal**</span></span>

* <span data-ttu-id="3a0f9-114">Válassza ki **előfizetések** a a bal oldali navigációs lapjáról, és másolja az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-114">Select **Subscriptions** from the left navigation page and copy the subscription id.</span></span>

<span data-ttu-id="3a0f9-115">**Az Azure CLI-vel**</span><span class="sxs-lookup"><span data-stu-id="3a0f9-115">**Using the Azure CLI**</span></span>

* <span data-ttu-id="3a0f9-116">Típus ```azure account list``` , és másolja az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-116">Type ```azure account list``` and copy the subscription id.</span></span>

<span data-ttu-id="3a0f9-117">Típus `docker-machine create --driver azure` , a beállításokat, és az alapértelmezett értékekre.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-117">Type `docker-machine create --driver azure` to see the options and their default values.</span></span>
<span data-ttu-id="3a0f9-118">Azt is láthatja a [Docker Azure illesztőprogram dokumentáció](https://docs.docker.com/machine/drivers/azure/) vonatkozó információ.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-118">You can also see the [Docker Azure Driver documentation](https://docs.docker.com/machine/drivers/azure/) for more info.</span></span> 

<span data-ttu-id="3a0f9-119">Az alábbi példa alapul a [alapértelmezett értékek](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), de igény szerint állítsa be ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="3a0f9-119">The following example relies upon the [default values](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), but it does optionally set these values:</span></span> 

* <span data-ttu-id="3a0f9-120">Azure-dns generált tanúsítványokat és a nyilvános IP-cím társított név.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-120">azure-dns for the name associated with the public IP and certificates generated.</span></span> <span data-ttu-id="3a0f9-121">Ez az a virtuális gép DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-121">This is the DNS name of your virtual machine.</span></span> <span data-ttu-id="3a0f9-122">A virtuális gép ezután biztonságosan állítsa le, az dinamikus IP-cím felszabadítása, és lehetővé teszik a virtuális gép újra kezdődik-e egy új IP-cím feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-122">The VM can then safely stop, release the dynamic IP, and provide the ability to reconnect after the vm starts again with a new IP.</span></span> <span data-ttu-id="3a0f9-123">A név előtagja egyedinek kell lennie az adott régióban UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-123">The name prefix must be unique for that region  UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span></span>
* <span data-ttu-id="3a0f9-124">Nyissa meg a virtuális Gépet, a kimenő internet-hozzáférés 80-as port</span><span class="sxs-lookup"><span data-stu-id="3a0f9-124">open port 80 on the VM for outbound internet access</span></span>
* <span data-ttu-id="3a0f9-125">gyorsabb prémium szintű storage magukat, hogy a virtuális gép mérete</span><span class="sxs-lookup"><span data-stu-id="3a0f9-125">size of the VM to utilize faster premium storage</span></span>
* <span data-ttu-id="3a0f9-126">prémium szintű storage, használja a virtuálisgép-lemez</span><span class="sxs-lookup"><span data-stu-id="3a0f9-126">premium storage used for the vm disk</span></span>

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a><span data-ttu-id="3a0f9-127">A docker-gép docker állomás</span><span class="sxs-lookup"><span data-stu-id="3a0f9-127">Choose a docker host with docker-machine</span></span>
<span data-ttu-id="3a0f9-128">Miután egy bejegyzést a docker-gépen ahhoz, hogy a gazdagép, az alapértelmezett állomás docker parancsok futtatásakor állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-128">Once you have an entry in docker-machine for your host, you can set the default host when running docker commands.</span></span>

## <a name="using-powershell"></a><span data-ttu-id="3a0f9-129">A PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="3a0f9-129">Using PowerShell</span></span>
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a><span data-ttu-id="3a0f9-130">Bash használatával</span><span class="sxs-lookup"><span data-stu-id="3a0f9-130">Using Bash</span></span>
```bash
eval $(docker-machine env MyDockerHost)
```

<span data-ttu-id="3a0f9-131">A megadott gazdagépcsoport alapján most futtathatók docker-parancsok</span><span class="sxs-lookup"><span data-stu-id="3a0f9-131">You can now run docker commands against the specified host</span></span>

```
docker ps
docker info
```

## <a name="run-a-container"></a><span data-ttu-id="3a0f9-132">Egy tároló futtatása</span><span class="sxs-lookup"><span data-stu-id="3a0f9-132">Run a container</span></span>
<span data-ttu-id="3a0f9-133">Egy konfigurált gazdagéppel most futtathatja egy egyszerű webkiszolgálót, és ellenőrizze, hogy helyesen lett-e konfigurálva a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-133">With a host configured, you can now run a simple web server to test whether your host was configured correctly.</span></span>
<span data-ttu-id="3a0f9-134">Itt azt a szabványos nginx-lemezkép használatához adja meg, hogy kell-e figyelni 80-as porton, és, hogy ha a gazdagép virtuális gép újraindul, a tároló fog újraindul, valamint (`--restart=always`).</span><span class="sxs-lookup"><span data-stu-id="3a0f9-134">Here we use a standard nginx image, specify that it should listen on port 80, and that if the host VM restarts, the container will restart as well (`--restart=always`).</span></span> 

```bash
docker run -d -p 80:80 --restart=always nginx
```

<span data-ttu-id="3a0f9-135">A kimeneti hasonlóan kell kinéznie a következő:</span><span class="sxs-lookup"><span data-stu-id="3a0f9-135">The output should look something like the following:</span></span>

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a><span data-ttu-id="3a0f9-136">A tároló tesztelése</span><span class="sxs-lookup"><span data-stu-id="3a0f9-136">Test the container</span></span>
<span data-ttu-id="3a0f9-137">Vizsgálja meg a futó tárolók használatával `docker ps`:</span><span class="sxs-lookup"><span data-stu-id="3a0f9-137">Examine running containers using `docker ps`:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

<span data-ttu-id="3a0f9-138">És a futó tároló parancsot kell beírnia `docker-machine ip <VM name>` az IP-címet adja meg a böngészőben kereséséhez:</span><span class="sxs-lookup"><span data-stu-id="3a0f9-138">And, to see the running container, type `docker-machine ip <VM name>` to find the IP address to enter in the browser:</span></span>

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Futó ngnix tároló](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a><span data-ttu-id="3a0f9-140">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="3a0f9-140">Summary</span></span>
<span data-ttu-id="3a0f9-141">Docker-számítógéppel könnyen megadhat docker-gazdagépekből az Azure-ban a az egyes docker állomás érvényesítést.</span><span class="sxs-lookup"><span data-stu-id="3a0f9-141">With docker-machine, you can easily provision docker hosts in Azure for your individual docker host validations.</span></span>
<span data-ttu-id="3a0f9-142">Éles üzemeltető tároló, tekintse meg a [Azure Tárolószolgáltatásban](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="3a0f9-142">For production hosting of containers, see the [Azure Container Service](http://aka.ms/AzureContainerService)</span></span>

<span data-ttu-id="3a0f9-143">A Visual Studio .NET Core alkalmazások fejlesztéséhez, lásd: [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)</span><span class="sxs-lookup"><span data-stu-id="3a0f9-143">To develop .NET Core Applications with Visual Studio, see [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)</span></span>

