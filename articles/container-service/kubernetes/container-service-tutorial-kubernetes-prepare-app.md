---
title: "aaaAzure Tárolószolgáltatás útmutató – az alkalmazás előkészítése |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató – az alkalmazás előkészítése"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b537ecc9ff50358fb65b128bfe6eb894dd088cc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-images-toobe-used-with-azure-container-service"></a><span data-ttu-id="dcf6b-104">Tároló képek toobe együtt az Azure Tárolószolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="dcf6b-104">Create container images toobe used with Azure Container Service</span></span>

<span data-ttu-id="dcf6b-105">Ebben az oktatóanyagban az első rész hét, a tárolót több alkalmazás Kubernetes használatra kész.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-105">In this tutorial, part one of seven, a multi-container application is prepared for use in Kubernetes.</span></span> <span data-ttu-id="dcf6b-106">Befejeződött a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="dcf6b-106">Steps completed include:</span></span>  

> [!div class="checklist"]
> * <span data-ttu-id="dcf6b-107">Alkalmazás forrásának klónozása a GitHubról</span><span class="sxs-lookup"><span data-stu-id="dcf6b-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="dcf6b-108">Hello alkalmazás forrásból tároló lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="dcf6b-108">Creating a container image from hello application source</span></span>
> * <span data-ttu-id="dcf6b-109">Helyi Docker környezetben hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="dcf6b-109">Testing hello application in a local Docker environment</span></span>

<span data-ttu-id="dcf6b-110">Ezt követően a helyi fejlesztési környezetben a következő alkalmazás hello érhető el.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-110">Once completed, hello following application is accessible in your local development environment.</span></span>

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/azure-vote.png)

<span data-ttu-id="dcf6b-112">A következő útmutatókból hello tároló kép feltöltött tooan Azure tároló beállításkulcs, és ezután futtassa az Azure-ban üzemeltetett Kubernetes fürt.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-112">In subsequent tutorials, hello container image is uploaded tooan Azure Container Registry, and then run in an Azure hosted Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="dcf6b-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="dcf6b-113">Before you begin</span></span>

<span data-ttu-id="dcf6b-114">Az oktatóanyag feltételezi, hogy rendelkezik a Docker fő fogalmaira, például a tárolókra, tárolórendszerképekre és az alapszintű Docker-parancsokra vonatkozó alapvető ismeretekkel.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-114">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="dcf6b-115">Amennyiben szükséges, tekintse meg a tárolók alapfogalmainak ismertetését a [Bevezetés a Docker használatába]( https://docs.docker.com/get-started/) című cikkben.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-115">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="dcf6b-116">toocomplete ebben az oktatóanyagban egy Docker környezetre van szükség.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-116">toocomplete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="dcf6b-117">A Docker csomagokat biztosít, amelyekkel a Docker egyszerűen konfigurálható bármely [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) vagy [Linux](https://docs.docker.com/engine/installation/#supported-platforms) rendszeren.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-117">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="dcf6b-118">Az alkalmazáskód letöltése</span><span class="sxs-lookup"><span data-stu-id="dcf6b-118">Get application code</span></span>

<span data-ttu-id="dcf6b-119">Ebben az oktatóanyagban használt hello mintaalkalmazás egy alapszintű szavazó alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-119">hello sample application used in this tutorial is a basic voting app.</span></span> <span data-ttu-id="dcf6b-120">hello alkalmazás áll egy előtér-webkiszolgáló és egy háttér-Redis-példányt.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-120">hello application consists of a front-end web component and a back-end Redis instance.</span></span> <span data-ttu-id="dcf6b-121">hello webszolgáltatás összetevője egy egyéni tároló lemezképpel lesz csomagolva.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-121">hello web component is packaged into a custom container image.</span></span> <span data-ttu-id="dcf6b-122">hello Redis példány Docker hubról változatlan kép használja.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-122">hello Redis instance uses an unmodified image from Docker Hub.</span></span>  

<span data-ttu-id="dcf6b-123">Git toodownload hello alkalmazás tooyour fejlesztési környezet egy példányát használja.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-123">Use git toodownload a copy of hello application tooyour development environment.</span></span>

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="dcf6b-124">Belső hello klónozott directory hello alkalmazás forráskódjához, egy előre létrehozott Docker compose fájlt, és Kubernetes jegyzékfájlt.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-124">Inside hello cloned directory is hello application source code, a pre-created Docker compose file, and a Kubernetes manifest file.</span></span> <span data-ttu-id="dcf6b-125">Ezeket a fájlokat olyan használt toocreate eszközök hello oktatóanyag beállítása során.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-125">These files are used toocreate assets throughout hello tutorial set.</span></span> 

## <a name="create-container-images"></a><span data-ttu-id="dcf6b-126">Tároló képek létrehozása</span><span class="sxs-lookup"><span data-stu-id="dcf6b-126">Create container images</span></span>

<span data-ttu-id="dcf6b-127">[Docker Compose](https://docs.docker.com/compose/) lehet tooautomate hello build tároló képek és hello alkalmazások központi telepítését figyelemmel több tárolót használja.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-127">[Docker Compose](https://docs.docker.com/compose/) can be used tooautomate hello build out of container images and hello deployment of multi-container applications.</span></span>

<span data-ttu-id="dcf6b-128">Futtassa hello docker-compose.yml fájlt toocreate hello tároló kép, letöltési hello Redis lemezképet, és hello alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-128">Run hello docker-compose.yml file toocreate hello container image, download hello Redis image, and start hello application.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up -d
```

<span data-ttu-id="dcf6b-129">Befejezése után használja hello [docker képek](https://docs.docker.com/engine/reference/commandline/images/) toosee hello létrehozott lemezképek parancsot.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-129">When completed, use hello [docker images](https://docs.docker.com/engine/reference/commandline/images/) command toosee hello created images.</span></span>

```bash
docker images
```

<span data-ttu-id="dcf6b-130">Figyelje meg, hogy három képek letöltése vagy létrehozni.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-130">Notice that three images have been downloaded or created.</span></span> <span data-ttu-id="dcf6b-131">Hello *azure-szavazat-front* kép hello alkalmazást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-131">hello *azure-vote-front* image contains hello application.</span></span> <span data-ttu-id="dcf6b-132">Származik hello *nginx-flask* kép.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-132">It was derived from hello *nginx-flask* image.</span></span> <span data-ttu-id="dcf6b-133">a Docker Hub letöltött hello Redis kép.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-133">hello Redis image was downloaded from Docker Hub.</span></span>

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

<span data-ttu-id="dcf6b-134">Futtassa a hello [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) parancs toosee hello futó tárolók.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-134">Run hello [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) command toosee hello running containers.</span></span>

```bash
docker ps
```

<span data-ttu-id="dcf6b-135">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="dcf6b-135">Output:</span></span>

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a><span data-ttu-id="dcf6b-136">Alkalmazás helyi teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="dcf6b-136">Test application locally</span></span>

<span data-ttu-id="dcf6b-137">Keresse meg a toohttp://localhost:8080 toosee hello alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-137">Browse toohttp://localhost:8080 toosee hello running application.</span></span>

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a><span data-ttu-id="dcf6b-139">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="dcf6b-139">Clean up resources</span></span>

<span data-ttu-id="dcf6b-140">Most, hogy az alkalmazás működésének ellenőrzése megtörtént, futó tárolók hello leállt, és eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-140">Now that application functionality has been validated, hello running containers can be stopped and removed.</span></span> <span data-ttu-id="dcf6b-141">Ne törölje az hello tároló képek.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-141">Do not delete hello container images.</span></span> <span data-ttu-id="dcf6b-142">Hello *azure-szavazat-front* kép hello következő oktatóanyagában feltöltött tooan Azure tároló beállításjegyzék példány.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-142">hello *azure-vote-front* image is uploaded tooan Azure Container Registry instance in hello next tutorial.</span></span>

<span data-ttu-id="dcf6b-143">Futtassa a következő futó tárolók toostop hello hello.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-143">Run hello following toostop hello running containers.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml stop
```

<span data-ttu-id="dcf6b-144">A következő parancs hello leállt hello tárolók törölnie.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-144">Delete hello stopped containers with hello following command.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml rm
```

<span data-ttu-id="dcf6b-145">A művelet befejezését, a tároló lemezkép hello Azure szavazattal alkalmazást tartalmazó rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-145">At completion, you have a container image that contains hello Azure Vote application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dcf6b-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dcf6b-146">Next steps</span></span>

<span data-ttu-id="dcf6b-147">Ebben az oktatóanyagban egy alkalmazás teszteltük és hello alkalmazáshoz létrehozott tároló képek.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-147">In this tutorial, an application was tested and container images created for hello application.</span></span> <span data-ttu-id="dcf6b-148">befejeződtek a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dcf6b-148">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dcf6b-149">A Klónozás hello alkalmazás adatforrás a Githubról</span><span class="sxs-lookup"><span data-stu-id="dcf6b-149">Cloning hello application source from GitHub</span></span>  
> * <span data-ttu-id="dcf6b-150">A tároló lemezkép létrehozott alkalmazás adatforrás</span><span class="sxs-lookup"><span data-stu-id="dcf6b-150">Created a container image from application source</span></span>
> * <span data-ttu-id="dcf6b-151">Helyi Docker környezetben tesztelt hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="dcf6b-151">Tested hello application in a local Docker environment</span></span>

<span data-ttu-id="dcf6b-152">Előzetes toohello oktatóanyag következő toolearn tároló lemezképek tárolása egy Azure-tároló beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="dcf6b-152">Advance toohello next tutorial toolearn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dcf6b-153">Leküldéses képek tooAzure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="dcf6b-153">Push images tooAzure Container Registry</span></span>](./container-service-tutorial-kubernetes-prepare-acr.md)
