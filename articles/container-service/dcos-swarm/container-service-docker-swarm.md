---
title: "Azure Swarm aaaManage Docker API-fürtöt |} Microsoft Docs"
description: "Tárolók tooa Docker Swarm-fürt üzembe az Azure Tárolószolgáltatásban"
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
ms.openlocfilehash: bb9b07c82a7b48caeb2e351455797cbf2a6e7480
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="container-management-with-docker-swarm"></a><span data-ttu-id="71f65-104">Tárolókezelés a Docker Swarmmal</span><span class="sxs-lookup"><span data-stu-id="71f65-104">Container management with Docker Swarm</span></span>
<span data-ttu-id="71f65-105">A Docker Swarm olyan környezetet biztosít, amelyben tárolóalapú számítási feladatokat helyezhet üzembe egy Docker-gazdagépekből álló készletben.</span><span class="sxs-lookup"><span data-stu-id="71f65-105">Docker Swarm provides an environment for deploying containerized workloads across a pooled set of Docker hosts.</span></span> <span data-ttu-id="71f65-106">A docker Swarm hello natív Docker API-t használja.</span><span class="sxs-lookup"><span data-stu-id="71f65-106">Docker Swarm uses hello native Docker API.</span></span> <span data-ttu-id="71f65-107">hello munkafolyamattal a Docker Swarmról majdnem azonos toowhat lenne, az egyetlen tároló-gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="71f65-107">hello workflow for managing containers on a Docker Swarm is almost identical toowhat it would be on a single container host.</span></span> <span data-ttu-id="71f65-108">Ez a dokumentum egyszerű példák segítségével ismerteti, hogy miként helyezhetők üzembe a tárolóalapú munkafolyamatok a Docker Swarm Azure tárolószolgáltatás-példányaiban.</span><span class="sxs-lookup"><span data-stu-id="71f65-108">This document provides simple examples of deploying containerized workloads in an Azure Container Service instance of Docker Swarm.</span></span> <span data-ttu-id="71f65-109">További részletes dokumentációt a Docker Swarmról a [ Docker.com](https://docs.docker.com/swarm/) webhelyen talál.</span><span class="sxs-lookup"><span data-stu-id="71f65-109">For more in-depth documentation on Docker Swarm, see [Docker Swarm on Docker.com](https://docs.docker.com/swarm/).</span></span>

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="71f65-110">A dokumentumban szereplő toohello gyakorlatok előfeltételei:</span><span class="sxs-lookup"><span data-stu-id="71f65-110">Prerequisites toohello exercises in this document:</span></span>

[<span data-ttu-id="71f65-111">Swarm-fürt létrehozása az Azure Container Service-ben</span><span class="sxs-lookup"><span data-stu-id="71f65-111">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)

[<span data-ttu-id="71f65-112">Csatlakozás az Azure Tárolószolgáltatásban hello Swarm-fürthöz</span><span class="sxs-lookup"><span data-stu-id="71f65-112">Connect with hello Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)

## <a name="deploy-a-new-container"></a><span data-ttu-id="71f65-113">Új tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="71f65-113">Deploy a new container</span></span>
<span data-ttu-id="71f65-114">egy új tároló Docker Swarm, hello az toocreate hello használata `docker run` (Győződjön meg arról, hogy egy SSH-alagút toohello hello fenti előfeltételeknek megfelelően főkiszolgálók megnyitott) parancsot.</span><span class="sxs-lookup"><span data-stu-id="71f65-114">toocreate a new container in hello Docker Swarm, use hello `docker run` command (ensuring that you have opened an SSH tunnel toohello masters as per hello prerequisites above).</span></span> <span data-ttu-id="71f65-115">Ez a példa létrehoz egy tároló hello `yeasy/simple-web` lemezképet:</span><span class="sxs-lookup"><span data-stu-id="71f65-115">This example creates a container from hello `yeasy/simple-web` image:</span></span>

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

<span data-ttu-id="71f65-116">Hello tároló létrehozása után a `docker ps` hello tároló tooreturn információt.</span><span class="sxs-lookup"><span data-stu-id="71f65-116">After hello container has been created, use `docker ps` tooreturn information about hello container.</span></span> <span data-ttu-id="71f65-117">Itt figyelje meg, hogy hello Swarm ügynök hello tárolót futtató szerepel:</span><span class="sxs-lookup"><span data-stu-id="71f65-117">Notice here that hello Swarm agent that is hosting hello container is listed:</span></span>

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

<span data-ttu-id="71f65-118">Hello Swarm ügynök terheléselosztójának hello nyilvános DNS-nevén keresztül ebben a tárolóban futó alkalmazást hello érhetők el.</span><span class="sxs-lookup"><span data-stu-id="71f65-118">You can now access hello application that is running in this container through hello public DNS name of hello Swarm agent load balancer.</span></span> <span data-ttu-id="71f65-119">Ez az információ hello Azure-portálon találja meg:</span><span class="sxs-lookup"><span data-stu-id="71f65-119">You can find this information in hello Azure portal:</span></span>  

![Valós látogatási eredmények](./media/container-service-docker-swarm/real-visit.jpg)  

<span data-ttu-id="71f65-121">Alapértelmezés szerint hello terheléselosztó rendelkezik portok: 80-as, 8080-as és 443 megnyitása.</span><span class="sxs-lookup"><span data-stu-id="71f65-121">By default hello Load Balancer has ports 80, 8080 and 443 open.</span></span> <span data-ttu-id="71f65-122">Ha azt szeretné, hogy egy másik porton tooconnect szüksége lesz tooopen ezt a portot, az Azure Load Balancer hello hello ügynök készlet.</span><span class="sxs-lookup"><span data-stu-id="71f65-122">If you want tooconnect on another port you will need tooopen that port on hello Azure Load Balancer for hello Agent Pool.</span></span>

## <a name="deploy-multiple-containers"></a><span data-ttu-id="71f65-123">Több tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="71f65-123">Deploy multiple containers</span></span>
<span data-ttu-id="71f65-124">Több tároló indulnak el, futtassa a "docker Futtatás" többször, mint hello használhatja `docker ps` parancs toosee hello tárolók futtató futnak a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="71f65-124">As multiple containers are started, by executing 'docker run' multiple times, you can use hello `docker ps` command toosee which hosts hello containers are running on.</span></span> <span data-ttu-id="71f65-125">A hello az alábbi példában három tároló egyenlően van elosztva három Swarm ügynök hello között:</span><span class="sxs-lookup"><span data-stu-id="71f65-125">In hello example below, three containers are spread evenly across hello three Swarm agents:</span></span>  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a><span data-ttu-id="71f65-126">Tároló üzembe helyezése a Docker Compose-zal</span><span class="sxs-lookup"><span data-stu-id="71f65-126">Deploy containers by using Docker Compose</span></span>
<span data-ttu-id="71f65-127">A több tároló Docker Compose tooautomate hello telepítését és konfigurálását is használhatja.</span><span class="sxs-lookup"><span data-stu-id="71f65-127">You can use Docker Compose tooautomate hello deployment and configuration of multiple containers.</span></span> <span data-ttu-id="71f65-128">toodo Igen, győződjön meg arról, hogy a Secure Shell (SSH) alagút létrejött adott hello DOCKER_HOST változó (lásd a fenti hello Előfeltételek) van beállítva.</span><span class="sxs-lookup"><span data-stu-id="71f65-128">toodo so, ensure that a Secure Shell (SSH) tunnel has been created and that hello DOCKER_HOST variable has been set (see hello pre-requisites above).</span></span>

<span data-ttu-id="71f65-129">Hozzon létre egy docker-compose.yml fájlt a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="71f65-129">Create a docker-compose.yml file on your local system.</span></span> <span data-ttu-id="71f65-130">toodo, ezzel [minta](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span><span class="sxs-lookup"><span data-stu-id="71f65-130">toodo this, use this [sample](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span></span>

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

<span data-ttu-id="71f65-131">Futtatás `docker-compose up -d` toostart hello üzemelő tárolópéldányokat:</span><span class="sxs-lookup"><span data-stu-id="71f65-131">Run `docker-compose up -d` toostart hello container deployments:</span></span>

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

<span data-ttu-id="71f65-132">Végezetül hello futó tárolók listája az eredmény.</span><span class="sxs-lookup"><span data-stu-id="71f65-132">Finally, hello list of running containers will be returned.</span></span> <span data-ttu-id="71f65-133">Ez a lista tükrözi hello tárolók üzembe helyezése a Docker Compose használatával:</span><span class="sxs-lookup"><span data-stu-id="71f65-133">This list reflects hello containers that were deployed by using Docker Compose:</span></span>

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

<span data-ttu-id="71f65-134">Természetesen használhatja `docker-compose ps` tooexamine csak hello definiált tárolók a `compose.yml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="71f65-134">Naturally, you can use `docker-compose ps` tooexamine only hello containers defined in your `compose.yml` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71f65-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="71f65-135">Next steps</span></span>
[<span data-ttu-id="71f65-136">További információ a Docker Swarmról</span><span class="sxs-lookup"><span data-stu-id="71f65-136">Learn more about Docker Swarm</span></span>](https://docs.docker.com/swarm/)

