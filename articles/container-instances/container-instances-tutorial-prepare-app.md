---
title: "aaaAzure tároló példányok útmutató – az alkalmazás előkészítése |} Az Azure Docs"
description: "Egy alkalmazás központi telepítési tooAzure tároló példányok előkészítése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 406ba796e5fefb1527f2e894cc3f7bbd8f7a5fd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-for-deployment-tooazure-container-instances"></a><span data-ttu-id="73d8d-103">A központi telepítés tooAzure tároló példányok tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="73d8d-103">Create container for deployment tooAzure Container Instances</span></span>

<span data-ttu-id="73d8d-104">Az Azure Container Instances lehetővé teszi Docker-tárolók üzembe helyezését az Azure-infrastruktúrán anélkül, hogy ehhez virtuális gépeket kellene kiépítenie vagy magasabb szintű szolgáltatást kellene alkalmaznia.</span><span class="sxs-lookup"><span data-stu-id="73d8d-104">Azure Container Instances enables deployment of Docker containers onto Azure infrastructure without provisioning any virtual machines or adopting any higher-level service.</span></span> <span data-ttu-id="73d8d-105">Jelen oktatóanyagban egy egyszerű webalkalmazást hozhat létre a Node.js szolgáltatásban, majd becsomagolhatja azt egy, az Azure Container Instances használatával futtatható tárolóba.</span><span class="sxs-lookup"><span data-stu-id="73d8d-105">In this tutorial, you will build a simple web application in Node.js and package it in a container that can be run using Azure Container Instances.</span></span> <span data-ttu-id="73d8d-106">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="73d8d-106">We will cover:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="73d8d-107">Alkalmazás forrásának klónozása a GitHubról</span><span class="sxs-lookup"><span data-stu-id="73d8d-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="73d8d-108">Tárolórendszerképek létrehozása az alkalmazás forrásából</span><span class="sxs-lookup"><span data-stu-id="73d8d-108">Creating container images from application source</span></span>
> * <span data-ttu-id="73d8d-109">Tesztelési hello lemezképet egy helyi Docker-környezetben</span><span class="sxs-lookup"><span data-stu-id="73d8d-109">Testing hello images in a local Docker environment</span></span>

<span data-ttu-id="73d8d-110">A következő útmutatókból meg fogja feltölteni a kép tooan Azure tároló beállításjegyzék, és telepíteni azt tooAzure tároló példányok.</span><span class="sxs-lookup"><span data-stu-id="73d8d-110">In subsequent tutorials, you will upload your image tooan Azure Container Registry, and then deploy them tooAzure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="73d8d-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="73d8d-111">Before you begin</span></span>

<span data-ttu-id="73d8d-112">Az oktatóanyag feltételezi, hogy rendelkezik a Docker fő fogalmaira, például a tárolókra, tárolórendszerképekre és az alapszintű Docker-parancsokra vonatkozó alapvető ismeretekkel.</span><span class="sxs-lookup"><span data-stu-id="73d8d-112">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="73d8d-113">Amennyiben szükséges, tekintse meg a tárolók alapfogalmainak ismertetését a [Bevezetés a Docker használatába]( https://docs.docker.com/get-started/) című cikkben.</span><span class="sxs-lookup"><span data-stu-id="73d8d-113">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="73d8d-114">toocomplete ebben az oktatóanyagban egy Docker környezetre van szükség.</span><span class="sxs-lookup"><span data-stu-id="73d8d-114">toocomplete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="73d8d-115">A Docker csomagokat biztosít, amelyekkel a Docker egyszerűen konfigurálható bármely [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) vagy [Linux](https://docs.docker.com/engine/installation/#supported-platforms) rendszeren.</span><span class="sxs-lookup"><span data-stu-id="73d8d-115">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="73d8d-116">Az alkalmazáskód letöltése</span><span class="sxs-lookup"><span data-stu-id="73d8d-116">Get application code</span></span>

<span data-ttu-id="73d8d-117">Ebben az oktatóanyagban hello minta tartalmaz egy egyszerű webalkalmazást a beépített [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="73d8d-117">hello sample in this tutorial includes a simple web application built in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="73d8d-118">hello alkalmazás szolgálja ki a statikus HTML-lapot, és néz ki:</span><span class="sxs-lookup"><span data-stu-id="73d8d-118">hello app serves a static HTML page and looks like this:</span></span>

![Az oktatóanyag alkalmazása böngészőben megjelenítve][aci-tutorial-app]

<span data-ttu-id="73d8d-120">Git toodownload hello mintát használja:</span><span class="sxs-lookup"><span data-stu-id="73d8d-120">Use git toodownload hello sample:</span></span>

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-hello-container-image"></a><span data-ttu-id="73d8d-121">Hello tároló lemezkép</span><span class="sxs-lookup"><span data-stu-id="73d8d-121">Build hello container image</span></span>

<span data-ttu-id="73d8d-122">hello hello minta tárházban megadott Dockerfile bemutatja, hogyan hello tárolóra épül.</span><span class="sxs-lookup"><span data-stu-id="73d8d-122">hello Dockerfile provided in hello sample repo shows how hello container is built.</span></span> <span data-ttu-id="73d8d-123">Elindítja a egy [hivatalos Node.js kép] [ dockerhub-nodeimage] alapján [Alpine Linux](https://alpinelinux.org/), egy kis terjesztési, amely kiválóan alkalmas toouse tárolókhoz.</span><span class="sxs-lookup"><span data-stu-id="73d8d-123">It starts from an [official Node.js image][dockerhub-nodeimage] based on [Alpine Linux](https://alpinelinux.org/), a small distribution that is well suited toouse with containers.</span></span> <span data-ttu-id="73d8d-124">Majd hello alkalmazásfájlok hello tárolóba másolja, függőségek hello csomópont Package Manager használatával telepíti, és végül a hello alkalmazás elindul.</span><span class="sxs-lookup"><span data-stu-id="73d8d-124">It then copies hello application files into hello container, installs dependencies using hello Node Package Manager, and finally starts hello application.</span></span>

```
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

<span data-ttu-id="73d8d-125">Használjon hello `docker build` toocreate hello tároló képét, címkézés azt *aci-oktatóanyag – alkalmazás*:</span><span class="sxs-lookup"><span data-stu-id="73d8d-125">Use hello `docker build` command toocreate hello container image, tagging it as *aci-tutorial-app*:</span></span>

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

<span data-ttu-id="73d8d-126">Használjon hello `docker images` toosee beépített hello lemezképet:</span><span class="sxs-lookup"><span data-stu-id="73d8d-126">Use hello `docker images` toosee hello built image:</span></span>

```bash
docker images
```

<span data-ttu-id="73d8d-127">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="73d8d-127">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-hello-container-locally"></a><span data-ttu-id="73d8d-128">Futtassa helyben a hello tároló</span><span class="sxs-lookup"><span data-stu-id="73d8d-128">Run hello container locally</span></span>

<span data-ttu-id="73d8d-129">Mielőtt újból hello tároló tooAzure tároló példányok telepítését, futtassa helyileg tooconfirm a működését.</span><span class="sxs-lookup"><span data-stu-id="73d8d-129">Before you try deploying hello container tooAzure Container Instances, run it locally tooconfirm that it works.</span></span> <span data-ttu-id="73d8d-130">Hello `-d` kapcsoló lehetővé teszi, hogy a hello tároló hello háttérben futnak, amíg a `-p` lehetővé teszi a toomap egy tetszőleges a számítási tooport hello tároló 80-as port.</span><span class="sxs-lookup"><span data-stu-id="73d8d-130">hello `-d` switch lets hello container run in hello background, while `-p` allows you toomap an arbitrary port on your compute tooport 80 in hello container.</span></span>

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

<span data-ttu-id="73d8d-131">Nyissa meg hello böngésző toohttp://localhost:8080 tooconfirm tároló hello fut.</span><span class="sxs-lookup"><span data-stu-id="73d8d-131">Open hello browser toohttp://localhost:8080 tooconfirm that hello container is running.</span></span>

![Helyileg hello böngészőben futó hello alkalmazás][aci-tutorial-app-local]

## <a name="next-steps"></a><span data-ttu-id="73d8d-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73d8d-133">Next steps</span></span>

<span data-ttu-id="73d8d-134">Ebben az oktatóanyagban létrehozására egy tároló lemezképnek, amely lehet a telepített tooAzure tároló példányok.</span><span class="sxs-lookup"><span data-stu-id="73d8d-134">In this tutorial, you created a container image that can be deployed tooAzure Container Instances.</span></span> <span data-ttu-id="73d8d-135">befejeződtek a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="73d8d-135">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="73d8d-136">A Klónozás hello alkalmazás adatforrás a Githubról</span><span class="sxs-lookup"><span data-stu-id="73d8d-136">Cloning hello application source from GitHub</span></span>  
> * <span data-ttu-id="73d8d-137">Tárolórendszerképek létrehozása az alkalmazás forrásából</span><span class="sxs-lookup"><span data-stu-id="73d8d-137">Creating container images from application source</span></span>
> * <span data-ttu-id="73d8d-138">Tesztelés helyileg hello tároló</span><span class="sxs-lookup"><span data-stu-id="73d8d-138">Testing hello container locally</span></span>

<span data-ttu-id="73d8d-139">Előzetes toohello oktatóanyag következő toolearn tároló lemezképek tárolása egy Azure-tároló beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="73d8d-139">Advance toohello next tutorial toolearn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="73d8d-140">Leküldéses képek tooAzure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="73d8d-140">Push images tooAzure Container Registry</span></span>](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png