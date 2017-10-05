---
title: "Azure Container Instances oktatóanyag – Az alkalmazás előkészítése | Azure Docs"
description: "Alkalmazás előkészítése az Azure Container Instances szolgáltatásban való üzembe helyezéshez"
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
ms.openlocfilehash: 167297e10eed11833623ff797e676ad43c65f9ad
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a><span data-ttu-id="7f92d-103">Tároló létrehozása az Azure Container Instances szolgáltatásban való üzembe helyezéshez</span><span class="sxs-lookup"><span data-stu-id="7f92d-103">Create container for deployment to Azure Container Instances</span></span>

<span data-ttu-id="7f92d-104">Az Azure Container Instances lehetővé teszi Docker-tárolók üzembe helyezését az Azure-infrastruktúrán anélkül, hogy ehhez virtuális gépeket kellene kiépítenie vagy magasabb szintű szolgáltatást kellene alkalmaznia.</span><span class="sxs-lookup"><span data-stu-id="7f92d-104">Azure Container Instances enables deployment of Docker containers onto Azure infrastructure without provisioning any virtual machines or adopting any higher-level service.</span></span> <span data-ttu-id="7f92d-105">Jelen oktatóanyagban egy egyszerű webalkalmazást hozhat létre a Node.js szolgáltatásban, majd becsomagolhatja azt egy, az Azure Container Instances használatával futtatható tárolóba.</span><span class="sxs-lookup"><span data-stu-id="7f92d-105">In this tutorial, you will build a simple web application in Node.js and package it in a container that can be run using Azure Container Instances.</span></span> <span data-ttu-id="7f92d-106">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="7f92d-106">We will cover:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7f92d-107">Alkalmazás forrásának klónozása a GitHubról</span><span class="sxs-lookup"><span data-stu-id="7f92d-107">Cloning application source from GitHub</span></span>  
> * <span data-ttu-id="7f92d-108">Tárolórendszerképek létrehozása az alkalmazás forrásából</span><span class="sxs-lookup"><span data-stu-id="7f92d-108">Creating container images from application source</span></span>
> * <span data-ttu-id="7f92d-109">A rendszerképek tesztelése helyi Docker-környezetben</span><span class="sxs-lookup"><span data-stu-id="7f92d-109">Testing the images in a local Docker environment</span></span>

<span data-ttu-id="7f92d-110">A következő oktatóanyagokban feltöltheti majd a rendszerképet egy Azure Container Registry jegyzékbe, majd üzembe helyezheti az Azure Container Instances szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7f92d-110">In subsequent tutorials, you will upload your image to an Azure Container Registry, and then deploy them to Azure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7f92d-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="7f92d-111">Before you begin</span></span>

<span data-ttu-id="7f92d-112">Az oktatóanyag feltételezi, hogy rendelkezik a Docker fő fogalmaira, például a tárolókra, tárolórendszerképekre és az alapszintű Docker-parancsokra vonatkozó alapvető ismeretekkel.</span><span class="sxs-lookup"><span data-stu-id="7f92d-112">This tutorial assumes a basic understanding of core Docker concepts such as containers, container images, and basic docker commands.</span></span> <span data-ttu-id="7f92d-113">Amennyiben szükséges, tekintse meg a tárolók alapfogalmainak ismertetését a [Bevezetés a Docker használatába]( https://docs.docker.com/get-started/) című cikkben.</span><span class="sxs-lookup"><span data-stu-id="7f92d-113">If needed, see [Get started with Docker]( https://docs.docker.com/get-started/) for a primer on container basics.</span></span> 

<span data-ttu-id="7f92d-114">Az oktatóanyag elvégzéséhez szüksége lesz egy Docker-fejlesztési környezetre.</span><span class="sxs-lookup"><span data-stu-id="7f92d-114">To complete this tutorial, you need a Docker development environment.</span></span> <span data-ttu-id="7f92d-115">A Docker csomagokat biztosít, amelyekkel a Docker egyszerűen konfigurálható bármely [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) vagy [Linux](https://docs.docker.com/engine/installation/#supported-platforms) rendszeren.</span><span class="sxs-lookup"><span data-stu-id="7f92d-115">Docker provides packages that easily configure Docker on any [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), or [Linux](https://docs.docker.com/engine/installation/#supported-platforms) system.</span></span>

## <a name="get-application-code"></a><span data-ttu-id="7f92d-116">Az alkalmazáskód letöltése</span><span class="sxs-lookup"><span data-stu-id="7f92d-116">Get application code</span></span>

<span data-ttu-id="7f92d-117">Az oktatóanyagban foglalt példa egy, a [Node.js](http://nodejs.org) használatával létrehozott egyszerű webalkalmazást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7f92d-117">The sample in this tutorial includes a simple web application built in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="7f92d-118">Az alkalmazás egy statikus HTML-oldalt szolgál ki, és így néz ki:</span><span class="sxs-lookup"><span data-stu-id="7f92d-118">The app serves a static HTML page and looks like this:</span></span>

![Az oktatóanyag alkalmazása böngészőben megjelenítve][aci-tutorial-app]

<span data-ttu-id="7f92d-120">Töltse le a mintát a Git használatával:</span><span class="sxs-lookup"><span data-stu-id="7f92d-120">Use git to download the sample:</span></span>

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a><span data-ttu-id="7f92d-121">Építse fel a tárolórendszerképet</span><span class="sxs-lookup"><span data-stu-id="7f92d-121">Build the container image</span></span>

<span data-ttu-id="7f92d-122">A mintatárházban elérhető Docker-fájl bemutatja a tároló felépítésének menetét.</span><span class="sxs-lookup"><span data-stu-id="7f92d-122">The Dockerfile provided in the sample repo shows how the container is built.</span></span> <span data-ttu-id="7f92d-123">Egy [hivatalos Node.js rendszerképpel][dockerhub-nodeimage] indul, amely az [Alpine Linux](https://alpinelinux.org/) rendszeren alapul – ez egy kisebb kiadás, amely jól használható a tárolókkal.</span><span class="sxs-lookup"><span data-stu-id="7f92d-123">It starts from an [official Node.js image][dockerhub-nodeimage] based on [Alpine Linux](https://alpinelinux.org/), a small distribution that is well suited to use with containers.</span></span> <span data-ttu-id="7f92d-124">Ezután bemásolja az alkalmazásfájlokat a tárolóba, telepíti a függőségeket a Node Package Manager használatával, és végül elindítja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7f92d-124">It then copies the application files into the container, installs dependencies using the Node Package Manager, and finally starts the application.</span></span>

```
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

<span data-ttu-id="7f92d-125">A `docker build` parancs használatával hozza létre a tárolórendszerképet, és lássa el az *aci-tutorial-app* címkével:</span><span class="sxs-lookup"><span data-stu-id="7f92d-125">Use the `docker build` command to create the container image, tagging it as *aci-tutorial-app*:</span></span>

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

<span data-ttu-id="7f92d-126">A `docker images` használatával tekintse meg a felépített rendszerképet:</span><span class="sxs-lookup"><span data-stu-id="7f92d-126">Use the `docker images` to see the built image:</span></span>

```bash
docker images
```

<span data-ttu-id="7f92d-127">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="7f92d-127">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-the-container-locally"></a><span data-ttu-id="7f92d-128">Futtassa helyileg a tárolót</span><span class="sxs-lookup"><span data-stu-id="7f92d-128">Run the container locally</span></span>

<span data-ttu-id="7f92d-129">Mielőtt megpróbálná üzembe helyezni a tárolót az Azure Container Instances szolgáltatásban, futtassa helyileg, hogy ellenőrizze a működését.</span><span class="sxs-lookup"><span data-stu-id="7f92d-129">Before you try deploying the container to Azure Container Instances, run it locally to confirm that it works.</span></span> <span data-ttu-id="7f92d-130">A `-d` kapcsolóval a tároló a háttérben működtethető, míg a `-p` kapcsolóval leképezheti a számítási erőforrás egy tetszőleges portját a tároló 80-as portjára.</span><span class="sxs-lookup"><span data-stu-id="7f92d-130">The `-d` switch lets the container run in the background, while `-p` allows you to map an arbitrary port on your compute to port 80 in the container.</span></span>

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

<span data-ttu-id="7f92d-131">Nyissa meg a böngészőben a http://localhost:8080 helyet, és ellenőrizze, hogy a tároló fut-e.</span><span class="sxs-lookup"><span data-stu-id="7f92d-131">Open the browser to http://localhost:8080 to confirm that the container is running.</span></span>

![Az alkalmazás helyileg történő futtatása a böngészőben][aci-tutorial-app-local]

## <a name="next-steps"></a><span data-ttu-id="7f92d-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f92d-133">Next steps</span></span>

<span data-ttu-id="7f92d-134">Az oktatóanyagban egy, az Azure Container Instances szolgáltatásban üzembe helyezhető tárolórendszerképet hozott létre.</span><span class="sxs-lookup"><span data-stu-id="7f92d-134">In this tutorial, you created a container image that can be deployed to Azure Container Instances.</span></span> <span data-ttu-id="7f92d-135">A következő lépéseket hajtotta végre:</span><span class="sxs-lookup"><span data-stu-id="7f92d-135">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7f92d-136">Az alkalmazás forrásának klónozása a GitHubról</span><span class="sxs-lookup"><span data-stu-id="7f92d-136">Cloning the application source from GitHub</span></span>  
> * <span data-ttu-id="7f92d-137">Tárolórendszerképek létrehozása az alkalmazás forrásából</span><span class="sxs-lookup"><span data-stu-id="7f92d-137">Creating container images from application source</span></span>
> * <span data-ttu-id="7f92d-138">A tároló helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="7f92d-138">Testing the container locally</span></span>

<span data-ttu-id="7f92d-139">Folytassa a következő oktatóanyaggal, amelyben a tárolórendszerképek az Azure Container Registry-ben való tárolásának módját ismerheti meg.</span><span class="sxs-lookup"><span data-stu-id="7f92d-139">Advance to the next tutorial to learn about storing container images in an Azure Container Registry.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7f92d-140">Rendszerképek leküldése az Azure Container Registry-be</span><span class="sxs-lookup"><span data-stu-id="7f92d-140">Push images to Azure Container Registry</span></span>](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png