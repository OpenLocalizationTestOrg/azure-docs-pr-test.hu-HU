---
title: "aaaCreate az első Azure tároló példányok tároló |} Az Azure Docs"
description: "Az Azure Container Instances üzembe helyezése és az első lépések"
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
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b57c52714933bd3b28c44d33f9af7cd1f23fb951
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a><span data-ttu-id="a89b3-103">Az első tároló létrehozása az Azure Container Instances szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="a89b3-103">Create your first container in Azure Container Instances</span></span>

<span data-ttu-id="a89b3-104">Azure tároló példányok teszi, hogy könnyen toocreate, és az Azure-ban tárolók kezelése.</span><span class="sxs-lookup"><span data-stu-id="a89b3-104">Azure Container Instances makes it easy toocreate and manage containers in Azure.</span></span> <span data-ttu-id="a89b3-105">A gyors üzembe helyezési a létrehoz egy tárolót az Azure-ban, és közzétenni toohello internet nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="a89b3-105">In this quick start, you will create a container in Azure and expose it toohello internet with a public IP address.</span></span> <span data-ttu-id="a89b3-106">Ez a művelet egyetlen paranccsal hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="a89b3-106">This operation is completed in a single command.</span></span> <span data-ttu-id="a89b3-107">Néhány másodperc elteltével a következőt láthatja a böngészőben:</span><span class="sxs-lookup"><span data-stu-id="a89b3-107">Within just a few seconds, you will see this in your browser:</span></span>

![Az Azure Container Instances használatával üzembe helyezett alkalmazás képe a böngészőben][aci-app-browser]

<span data-ttu-id="a89b3-109">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="a89b3-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a89b3-110">Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.12 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a89b3-110">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.12 or later.</span></span> <span data-ttu-id="a89b3-111">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="a89b3-111">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a89b3-112">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a89b3-112">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="a89b3-113">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="a89b3-113">Create a resource group</span></span>

<span data-ttu-id="a89b3-114">Az Azure Container Instances Azure-erőforrások, amelyeket egy Azure-erőforráscsoportba kell helyezni. Ez egy olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a89b3-114">Azure Container Instances are Azure resources and must be placed in an Azure resource group, a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="a89b3-115">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="a89b3-115">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="a89b3-116">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="a89b3-116">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a><span data-ttu-id="a89b3-117">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="a89b3-117">Create a container</span></span>

<span data-ttu-id="a89b3-118">A tároló létrehozásához meg kell adnia egy nevet, egy Docker-rendszerképet és egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a89b3-118">You can create a container by providing a name, a Docker image, and an Azure resource group.</span></span> <span data-ttu-id="a89b3-119">Opcionálisan teszi közzé hello tároló toohello internet nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="a89b3-119">You can optionally expose hello container toohello internet with a public IP address.</span></span> <span data-ttu-id="a89b3-120">Ebben az esetben egy, a [Node.js](http://nodejs.org) használatával létrehozott, nagyon egyszerű webalkalmazást tartalmazó tárolót használunk.</span><span class="sxs-lookup"><span data-stu-id="a89b3-120">In this case, we'll use a container that hosts a very simple web app written in [Node.js](http://nodejs.org).</span></span>

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

<span data-ttu-id="a89b3-121">Néhány másodpercen belül szerezheti be a válasz tooyour kérelmet.</span><span class="sxs-lookup"><span data-stu-id="a89b3-121">Within a few seconds, you should get a response tooyour request.</span></span> <span data-ttu-id="a89b3-122">Kezdetben hello tároló akkor fog szerepelni a **létrehozása** állapotát, de néhány másodpercen belül kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="a89b3-122">Initially, hello container will be in a **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="a89b3-123">Ellenőrizheti a hello használatával hello állapotát `show` parancs:</span><span class="sxs-lookup"><span data-stu-id="a89b3-123">You can check hello status using hello `show` command:</span></span>

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="a89b3-124">Hello kimeneti hello alsó részén látni fogja a hello tároló üzembe helyezési állapota és az IP-címe:</span><span class="sxs-lookup"><span data-stu-id="a89b3-124">At hello bottom of hello output, you will see hello container's provisioning state and its IP address:</span></span>

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

<span data-ttu-id="a89b3-125">Miután hello tároló áthelyezi toohello **sikeres** állapotba kerül, hello böngészőben megadott hello IP-cím használatával csatlakozni tud hozzá.</span><span class="sxs-lookup"><span data-stu-id="a89b3-125">Once hello container moves toohello **Succeeded** state, you can reach it in hello browser using hello IP address provided.</span></span> 

![Az Azure Container Instances használatával üzembe helyezett alkalmazás képe a böngészőben][aci-app-browser]

## <a name="pull-hello-container-logs"></a><span data-ttu-id="a89b3-127">Hello tároló naplók lekéréses</span><span class="sxs-lookup"><span data-stu-id="a89b3-127">Pull hello container logs</span></span>

<span data-ttu-id="a89b3-128">Képes lekérni a létrehozott hello hello tároló hello naplók `logs` parancs:</span><span class="sxs-lookup"><span data-stu-id="a89b3-128">You can pull hello logs for hello container you created using hello `logs` command:</span></span>

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="a89b3-129">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="a89b3-129">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-hello-container"></a><span data-ttu-id="a89b3-130">Hello tároló törlése</span><span class="sxs-lookup"><span data-stu-id="a89b3-130">Delete hello container</span></span>

<span data-ttu-id="a89b3-131">Amikor elkészült, hello tárolóval, eltávolíthatja azt hello segítségével `delete` parancs:</span><span class="sxs-lookup"><span data-stu-id="a89b3-131">When you are done with hello container, you can remove it using hello `delete` command:</span></span>

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="a89b3-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a89b3-132">Next steps</span></span>

<span data-ttu-id="a89b3-133">Az összes hello kód számára érhető el a gyors üzembe helyezési hello tárolójában [a Githubon][app-github-repo], a Dockerfile együtt.</span><span class="sxs-lookup"><span data-stu-id="a89b3-133">All of hello code for hello container used in this quick start is available [on GitHub][app-github-repo], along with its Dockerfile.</span></span> <span data-ttu-id="a89b3-134">Ha szeretné tootry felépítése saját magának, és telepíti azokat tooAzure tároló példányok hello Azure tároló beállításjegyzék használata, továbbra is toohello Azure tároló példányok oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a89b3-134">If you'd like tootry building it yourself and deploying it tooAzure Container Instances using hello Azure Container Registry, continue toohello Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a89b3-135">Az Azure Container Instances oktatóanyagai</span><span class="sxs-lookup"><span data-stu-id="a89b3-135">Azure Container Instances tutorials</span></span>](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png