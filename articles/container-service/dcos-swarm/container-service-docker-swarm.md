---
title: "Docker API-t Azure Swarm-fürt kezeléséhez |} Microsoft Docs"
description: "A Docker Swarm-fürt az Azure Tárolószolgáltatás – tárolók üzembe helyezése"
services: container-service
documentationcenter: 
author: rgardler
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 6ca2d2e49c4b7f5eb0580e7091b09209f8b73a7c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="container-management-with-docker-swarm"></a><span data-ttu-id="bfb2f-104">Tárolókezelés a Docker Swarmmal</span><span class="sxs-lookup"><span data-stu-id="bfb2f-104">Container management with Docker Swarm</span></span>
<span data-ttu-id="bfb2f-105">A Docker Swarm olyan környezetet biztosít, amelyben tárolóalapú számítási feladatokat helyezhet üzembe egy Docker-gazdagépekből álló készletben.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-105">Docker Swarm provides an environment for deploying containerized workloads across a pooled set of Docker hosts.</span></span> <span data-ttu-id="bfb2f-106">A Docker Swarm a natív Docker API-t használja.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-106">Docker Swarm uses the native Docker API.</span></span> <span data-ttu-id="bfb2f-107">A Docker Swarm tárolókezelésének munkafolyamata majdnem azonos az egyetlen tároló-gazdagépen elvégzendő munkafolyamattal.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-107">The workflow for managing containers on a Docker Swarm is almost identical to what it would be on a single container host.</span></span> <span data-ttu-id="bfb2f-108">Ez a dokumentum egyszerű példák segítségével ismerteti, hogy miként helyezhetők üzembe a tárolóalapú munkafolyamatok a Docker Swarm Azure tárolószolgáltatás-példányaiban.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-108">This document provides simple examples of deploying containerized workloads in an Azure Container Service instance of Docker Swarm.</span></span> <span data-ttu-id="bfb2f-109">További részletes dokumentációt a Docker Swarmról a [ Docker.com](https://docs.docker.com/swarm/) webhelyen talál.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-109">For more in-depth documentation on Docker Swarm, see [Docker Swarm on Docker.com](https://docs.docker.com/swarm/).</span></span>

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="bfb2f-110">A dokumentumban szereplő gyakorlatok előfeltételei:</span><span class="sxs-lookup"><span data-stu-id="bfb2f-110">Prerequisites to the exercises in this document:</span></span>

[<span data-ttu-id="bfb2f-111">Swarm-fürt létrehozása az Azure Container Service-ben</span><span class="sxs-lookup"><span data-stu-id="bfb2f-111">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)

[<span data-ttu-id="bfb2f-112">Csatlakozás a Swarm-fürthöz az Azure Container Service-ben</span><span class="sxs-lookup"><span data-stu-id="bfb2f-112">Connect with the Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)

## <a name="deploy-a-new-container"></a><span data-ttu-id="bfb2f-113">Új tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="bfb2f-113">Deploy a new container</span></span>
<span data-ttu-id="bfb2f-114">Ha új tárolót szeretne létrehozni a Docker Swarmban, használja a `docker run` parancsot (ügyeljen arra, hogy a fenti előfeltételeknek megfelelően nyisson meg egy SSH-alagutat a főkiszolgálók felé).</span><span class="sxs-lookup"><span data-stu-id="bfb2f-114">To create a new container in the Docker Swarm, use the `docker run` command (ensuring that you have opened an SSH tunnel to the masters as per the prerequisites above).</span></span> <span data-ttu-id="bfb2f-115">Az alábbi példa létrehoz egy tárolót a `yeasy/simple-web` lemezképből:</span><span class="sxs-lookup"><span data-stu-id="bfb2f-115">This example creates a container from the `yeasy/simple-web` image:</span></span>

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

<span data-ttu-id="bfb2f-116">A tároló létrehozása után használja a `docker ps` parancsot a tárolóinformációk megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-116">After the container has been created, use `docker ps` to return information about the container.</span></span> <span data-ttu-id="bfb2f-117">Az információkban a tárolót tartalmazó Swarm ügynök is szerepel:</span><span class="sxs-lookup"><span data-stu-id="bfb2f-117">Notice here that the Swarm agent that is hosting the container is listed:</span></span>

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

<span data-ttu-id="bfb2f-118">A tárolóban futó alkalmazást a Swarm ügynök terheléselosztójának nyilvános DNS-nevén keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-118">You can now access the application that is running in this container through the public DNS name of the Swarm agent load balancer.</span></span> <span data-ttu-id="bfb2f-119">Ezeket az információkat megtalálja az Azure Portalon:</span><span class="sxs-lookup"><span data-stu-id="bfb2f-119">You can find this information in the Azure portal:</span></span>  

![Valós látogatási eredmények](./media/container-service-docker-swarm/real-visit.jpg)  

<span data-ttu-id="bfb2f-121">Alapértelmezés szerint a Load Balancer a 80-as, 8080-as és 443-as portot nyitja meg.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-121">By default the Load Balancer has ports 80, 8080 and 443 open.</span></span> <span data-ttu-id="bfb2f-122">Ha másik porton keresztül szeretne csatlakozni, akkor az Azure Load Balancerben meg kell nyitnia a portot az ügynökkészlet számára.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-122">If you want to connect on another port you will need to open that port on the Azure Load Balancer for the Agent Pool.</span></span>

## <a name="deploy-multiple-containers"></a><span data-ttu-id="bfb2f-123">Több tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="bfb2f-123">Deploy multiple containers</span></span>
<span data-ttu-id="bfb2f-124">Amikor több tároló is elindul a „docker run” többszöri végrehajtása után, a `docker ps` paranccsal megtekintheti, mely állomásokon futnak a tárolók.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-124">As multiple containers are started, by executing 'docker run' multiple times, you can use the `docker ps` command to see which hosts the containers are running on.</span></span> <span data-ttu-id="bfb2f-125">Az alábbi példában három tároló oszlik el egyenlően a három Swarm-ügynökön:</span><span class="sxs-lookup"><span data-stu-id="bfb2f-125">In the example below, three containers are spread evenly across the three Swarm agents:</span></span>  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a><span data-ttu-id="bfb2f-126">Tároló üzembe helyezése a Docker Compose-zal</span><span class="sxs-lookup"><span data-stu-id="bfb2f-126">Deploy containers by using Docker Compose</span></span>
<span data-ttu-id="bfb2f-127">A Docker Compose-zal automatizálhatja a több tároló telepítését és konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-127">You can use Docker Compose to automate the deployment and configuration of multiple containers.</span></span> <span data-ttu-id="bfb2f-128">Ehhez hozzon létre egy Secure Shell- (SSH-) alagutat, és állítsa be a DOCKER_HOST változót (lásd a feni előfeltételeket).</span><span class="sxs-lookup"><span data-stu-id="bfb2f-128">To do so, ensure that a Secure Shell (SSH) tunnel has been created and that the DOCKER_HOST variable has been set (see the pre-requisites above).</span></span>

<span data-ttu-id="bfb2f-129">Hozzon létre egy docker-compose.yml fájlt a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-129">Create a docker-compose.yml file on your local system.</span></span> <span data-ttu-id="bfb2f-130">Ehhez használja ezt a [mintát](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span><span class="sxs-lookup"><span data-stu-id="bfb2f-130">To do this, use this [sample](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span></span>

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

<span data-ttu-id="bfb2f-131">A tárolók üzembe helyezéséhez futtassa a `docker-compose up -d` parancsot:</span><span class="sxs-lookup"><span data-stu-id="bfb2f-131">Run `docker-compose up -d` to start the container deployments:</span></span>

```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

<span data-ttu-id="bfb2f-132">Végül megjelenik a futó tárolók listája.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-132">Finally, the list of running containers will be returned.</span></span> <span data-ttu-id="bfb2f-133">Ebben a listában a Docker Compose-zal üzembe helyezett tárolók szerepelnek:</span><span class="sxs-lookup"><span data-stu-id="bfb2f-133">This list reflects the containers that were deployed by using Docker Compose:</span></span>

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

<span data-ttu-id="bfb2f-134">A `docker-compose ps` használatával természetesen megvizsgálhatja a csak a `compose.yml` fájlban megadott tárolókat.</span><span class="sxs-lookup"><span data-stu-id="bfb2f-134">Naturally, you can use `docker-compose ps` to examine only the containers defined in your `compose.yml` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bfb2f-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bfb2f-135">Next steps</span></span>
[<span data-ttu-id="bfb2f-136">További információ a Docker Swarmról</span><span class="sxs-lookup"><span data-stu-id="bfb2f-136">Learn more about Docker Swarm</span></span>](https://docs.docker.com/swarm/)

