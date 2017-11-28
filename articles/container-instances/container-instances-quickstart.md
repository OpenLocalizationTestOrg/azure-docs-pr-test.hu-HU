---
title: "Az első Azure Container Instances-tároló létrehozása | Azure Docs"
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
ms.openlocfilehash: 905f69e5e1e237a31d9bb1e326969ec83292c244
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a><span data-ttu-id="f5fb8-103">Az első tároló létrehozása az Azure Container Instances szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="f5fb8-103">Create your first container in Azure Container Instances</span></span>

<span data-ttu-id="f5fb8-104">Az Azure Container Instances segítségével egyszerűen hozhat létre és felügyelhet tárolókat az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-104">Azure Container Instances makes it easy to create and manage containers in Azure.</span></span> <span data-ttu-id="f5fb8-105">Ebben a rövid útmutatóban létrehozhat egy tárolót az Azure-ban, és közzéteheti az interneten egy nyilvános IP-címen keresztül.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-105">In this quick start, you will create a container in Azure and expose it to the internet with a public IP address.</span></span> <span data-ttu-id="f5fb8-106">Ez a művelet egyetlen paranccsal hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-106">This operation is completed in a single command.</span></span> <span data-ttu-id="f5fb8-107">Néhány másodperc elteltével a következőt láthatja a böngészőben:</span><span class="sxs-lookup"><span data-stu-id="f5fb8-107">Within just a few seconds, you will see this in your browser:</span></span>

![Az Azure Container Instances használatával üzembe helyezett alkalmazás képe a böngészőben][aci-app-browser]

<span data-ttu-id="f5fb8-109">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f5fb8-110">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a gyorsútmutatóhoz az Azure CLI 2.0.12-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-110">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.12 or later.</span></span> <span data-ttu-id="f5fb8-111">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-111">Run `az --version` to find the version.</span></span> <span data-ttu-id="f5fb8-112">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f5fb8-112">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="f5fb8-113">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="f5fb8-113">Create a resource group</span></span>

<span data-ttu-id="f5fb8-114">Az Azure Container Instances Azure-erőforrások, amelyeket egy Azure-erőforráscsoportba kell helyezni. Ez egy olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-114">Azure Container Instances are Azure resources and must be placed in an Azure resource group, a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="f5fb8-115">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-115">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="f5fb8-116">A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot az *eastus* helyen.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-116">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a><span data-ttu-id="f5fb8-117">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5fb8-117">Create a container</span></span>

<span data-ttu-id="f5fb8-118">A tároló létrehozásához meg kell adnia egy nevet, egy Docker-rendszerképet és egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-118">You can create a container by providing a name, a Docker image, and an Azure resource group.</span></span> <span data-ttu-id="f5fb8-119">Ha szeretné, közzéteheti a tárolót az interneten egy nyilvános IP-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-119">You can optionally expose the container to the internet with a public IP address.</span></span> <span data-ttu-id="f5fb8-120">Ebben az esetben egy, a [Node.js](http://nodejs.org) használatával létrehozott, nagyon egyszerű webalkalmazást tartalmazó tárolót használunk.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-120">In this case, we'll use a container that hosts a very simple web app written in [Node.js](http://nodejs.org).</span></span>

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

<span data-ttu-id="f5fb8-121">Néhány másodperc elteltével válasz érkezik a kérésre.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-121">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="f5fb8-122">A tároló kezdetben **Létrehozás** állapotban lesz, de néhány másodpercen belül el kell indulnia.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-122">Initially, the container will be in a **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="f5fb8-123">Az állapotot a `show` paranccsal ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="f5fb8-123">You can check the status using the `show` command:</span></span>

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="f5fb8-124">A kimenet alján látható a tároló kiépítési állapota és IP-címe:</span><span class="sxs-lookup"><span data-stu-id="f5fb8-124">At the bottom of the output, you will see the container's provisioning state and its IP address:</span></span>

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

<span data-ttu-id="f5fb8-125">Miután a tároló átvált a **Sikeres** állapotba, elérhetővé válik a böngészőben a megadott IP-címen.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-125">Once the container moves to the **Succeeded** state, you can reach it in the browser using the IP address provided.</span></span> 

![Az Azure Container Instances használatával üzembe helyezett alkalmazás képe a böngészőben][aci-app-browser]

## <a name="pull-the-container-logs"></a><span data-ttu-id="f5fb8-127">A tároló naplóinak lekérése</span><span class="sxs-lookup"><span data-stu-id="f5fb8-127">Pull the container logs</span></span>

<span data-ttu-id="f5fb8-128">A létrehozott tároló naplóit a `logs` paranccsal kérheti le:</span><span class="sxs-lookup"><span data-stu-id="f5fb8-128">You can pull the logs for the container you created using the `logs` command:</span></span>

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="f5fb8-129">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="f5fb8-129">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-the-container"></a><span data-ttu-id="f5fb8-130">A tároló törlése</span><span class="sxs-lookup"><span data-stu-id="f5fb8-130">Delete the container</span></span>

<span data-ttu-id="f5fb8-131">Miután végzett a tárolóval, a `delete` parancs használatával távolíthatja el:</span><span class="sxs-lookup"><span data-stu-id="f5fb8-131">When you are done with the container, you can remove it using the `delete` command:</span></span>

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="f5fb8-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f5fb8-132">Next steps</span></span>

<span data-ttu-id="f5fb8-133">A jelen rövid útmutatóban alkalmazott tároló kódja a Docker-fájljával egyetemben elérhető [a GitHubon][app-github-repo].</span><span class="sxs-lookup"><span data-stu-id="f5fb8-133">All of the code for the container used in this quick start is available [on GitHub][app-github-repo], along with its Dockerfile.</span></span> <span data-ttu-id="f5fb8-134">Ha próbaképpen szeretné maga létrehozni és üzembe helyezni az Azure Container Instances szolgáltatásban az Azure Container Registry használatával, folytassa az Azure Container Instances oktatóanyagával.</span><span class="sxs-lookup"><span data-stu-id="f5fb8-134">If you'd like to try building it yourself and deploying it to Azure Container Instances using the Azure Container Registry, continue to the Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f5fb8-135">Az Azure Container Instances oktatóanyagai</span><span class="sxs-lookup"><span data-stu-id="f5fb8-135">Azure Container Instances tutorials</span></span>](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png