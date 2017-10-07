---
title: "aaaAzure tároló példányok oktatóanyag – alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Azure tároló példányok útmutató - alkalmazás központi telepítése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b9fb098d9491e1073f0be4b14a0b9b1a18f16095
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-tooazure-container-instances"></a><span data-ttu-id="9d016-103">Egy tároló tooAzure tároló példányok telepítése</span><span class="sxs-lookup"><span data-stu-id="9d016-103">Deploy a container tooAzure Container Instances</span></span>

<span data-ttu-id="9d016-104">Ez a hello egy háromrészes oktatóanyag utolsó.</span><span class="sxs-lookup"><span data-stu-id="9d016-104">This is hello last of a three-part tutorial.</span></span> <span data-ttu-id="9d016-105">A korábbi szakaszokban [a tároló-lemezkép létrejött](container-instances-tutorial-prepare-app.md) és [tooan Azure tároló beállításjegyzék leküldött](container-instances-tutorial-prepare-acr.md).</span><span class="sxs-lookup"><span data-stu-id="9d016-105">In previous sections, [a container image was created](container-instances-tutorial-prepare-app.md) and [pushed tooan Azure Container Registry](container-instances-tutorial-prepare-acr.md).</span></span> <span data-ttu-id="9d016-106">Ez a szakasz hello az oktatóanyag befejezése hello tároló tooAzure tároló példányok üzembe helyezésével.</span><span class="sxs-lookup"><span data-stu-id="9d016-106">This section completes hello tutorial by deploying hello container tooAzure Container Instances.</span></span> <span data-ttu-id="9d016-107">Befejeződött a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="9d016-107">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9d016-108">Egy Azure Resource Manager-sablonnal a tárolócsoport meghatározása</span><span class="sxs-lookup"><span data-stu-id="9d016-108">Defining a container group using an Azure Resource Manager template</span></span>
> * <span data-ttu-id="9d016-109">A tárolócsoport hello hello Azure parancssori felület használatával telepítése</span><span class="sxs-lookup"><span data-stu-id="9d016-109">Deploying hello container group using hello Azure CLI</span></span>
> * <span data-ttu-id="9d016-110">Tároló naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="9d016-110">Viewing container logs</span></span>

## <a name="deploy-hello-container-using-hello-azure-cli"></a><span data-ttu-id="9d016-111">Hello Azure parancssori felület használatával hello tároló üzembe</span><span class="sxs-lookup"><span data-stu-id="9d016-111">Deploy hello container using hello Azure CLI</span></span>

<span data-ttu-id="9d016-112">hello Azure parancssori felület lehetővé teszi, hogy egy parancs a tároló tooAzure tároló példányok telepítését.</span><span class="sxs-lookup"><span data-stu-id="9d016-112">hello Azure CLI enables deployment of a container tooAzure Container Instances in a single command.</span></span> <span data-ttu-id="9d016-113">Mivel hello tároló lemezkép az Azure-tároló titkos beállításjegyzék hello, meg kell adnia a hitelesítő adatok szükséges tooaccess hello azt.</span><span class="sxs-lookup"><span data-stu-id="9d016-113">Since hello container image is hosted in hello private Azure Container Registry, you must include hello credentials required tooaccess it.</span></span> <span data-ttu-id="9d016-114">Ha szükséges, alább látható módon lekérdezheti azokat.</span><span class="sxs-lookup"><span data-stu-id="9d016-114">If necessary, you can query them as shown below.</span></span>

<span data-ttu-id="9d016-115">Tároló beállításjegyzék bejelentkezési kiszolgáló (a beállításjegyzék nevű frissítés):</span><span class="sxs-lookup"><span data-stu-id="9d016-115">Container registry login server (update with your registry name):</span></span>

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

<span data-ttu-id="9d016-116">Tároló beállításjegyzék jelszó:</span><span class="sxs-lookup"><span data-stu-id="9d016-116">Container registry password:</span></span>

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

<span data-ttu-id="9d016-117">toodeploy a tároló lemezkép erőforrással hello tároló beállításjegyzékből kérelem 1 Processzormagok és 1GB memória, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="9d016-117">toodeploy your container image from hello container registry with a resource request of 1 CPU core and 1GB of memory, run hello following command:</span></span>

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

<span data-ttu-id="9d016-118">Néhány másodpercen belül kap egy kezdeti választ az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9d016-118">Within a few seconds, you will receive an initial response from Azure Resource Manager.</span></span> <span data-ttu-id="9d016-119">hello központi telepítésének, használjon tooview hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="9d016-119">tooview hello state of hello deployment, use:</span></span>

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

<span data-ttu-id="9d016-120">Ez a parancs futtatását, amíg a hello állapota Folytatás *függőben lévő* túl*futtató*.</span><span class="sxs-lookup"><span data-stu-id="9d016-120">We can continue running this command until hello state changes from *pending* too*running*.</span></span> <span data-ttu-id="9d016-121">Azt is lépjen.</span><span class="sxs-lookup"><span data-stu-id="9d016-121">Then we can proceed.</span></span>

## <a name="view-hello-application-and-container-logs"></a><span data-ttu-id="9d016-122">Hello alkalmazás és a tároló naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="9d016-122">View hello application and container logs</span></span>

<span data-ttu-id="9d016-123">Miután a hello központi telepítés sikeres, nyissa meg a böngésző toohello IP-cím látható a következő parancs hello hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="9d016-123">Once hello deployment succeeds, open your browser toohello IP address shown in hello output of hello following command:</span></span>

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world app hello böngészőben][aci-app-browser]

<span data-ttu-id="9d016-125">Hello napló kimeneti hello tároló is megtekintheti:</span><span class="sxs-lookup"><span data-stu-id="9d016-125">You can also view hello log output of hello container:</span></span>

```azurecli-interactive
az container logs --name aci-tutorial-app -g myResourceGroup
```

<span data-ttu-id="9d016-126">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9d016-126">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a><span data-ttu-id="9d016-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d016-127">Next steps</span></span>

<span data-ttu-id="9d016-128">Ebben az oktatóanyagban a tárolók tooAzure tároló példányok telepítésének hello folyamat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="9d016-128">In this tutorial, you completed hello process of deploying your containers tooAzure Container Instances.</span></span> <span data-ttu-id="9d016-129">befejeződtek a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9d016-129">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9d016-130">Az Azure parancssori felület telepítése hello tárolót hello Azure tároló beállításjegyzék használatával hello</span><span class="sxs-lookup"><span data-stu-id="9d016-130">Deploying hello container from hello Azure Container Registry using hello Azure CLI</span></span>
> * <span data-ttu-id="9d016-131">Hello böngészőben hello alkalmazás megtekintése</span><span class="sxs-lookup"><span data-stu-id="9d016-131">Viewing hello application in hello browser</span></span>
> * <span data-ttu-id="9d016-132">Megtekintés hello tároló naplók</span><span class="sxs-lookup"><span data-stu-id="9d016-132">Viewing hello container logs</span></span>

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
