---
title: "aaaCreate az Docker-tároló beállításjegyzék titkos - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ismerkedés a létrehozása és kezelése a saját Docker-tároló nyilvántartó, hello Azure CLI 2.0"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: 2cadf42db0681a09c95486510f1e65c6f87c5280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-cli"></a><span data-ttu-id="3cbce-103">Hozzon létre egy felügyelt tároló beállításjegyzék hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="3cbce-103">Create a managed container registry using hello Azure CLI</span></span>

<span data-ttu-id="3cbce-104">Az Azure Container Registry egy felügyelt Docker-tárolóregisztrációs adatbázis-szolgáltatás, amely a privát Docker-tárolók rendszerképeinek tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="3cbce-104">Azure Container Registry is a managed Docker container registry service used for storing private Docker container images.</span></span> <span data-ttu-id="3cbce-105">Ez az útmutató adatokat hello Azure parancssori felület használatával felügyelt Azure-tároló beállításjegyzék példány létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3cbce-105">This guide details creating a managed Azure Container Registry instance using hello Azure CLI.</span></span>

<span data-ttu-id="3cbce-106">A felügyelt Azure-tárolóregisztrációs adatbázisok előzetes verziós kiadásban, és nem az összes régióban érhetők el.</span><span class="sxs-lookup"><span data-stu-id="3cbce-106">Managed Azure container registries are in preview and not available in all regions.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3cbce-107">Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="3cbce-107">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="3cbce-108">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="3cbce-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="3cbce-109">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3cbce-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="3cbce-110">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="3cbce-110">Create a resource group</span></span>

<span data-ttu-id="3cbce-111">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="3cbce-111">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="3cbce-112">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3cbce-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="3cbce-113">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westcentralus* helyét.</span><span class="sxs-lookup"><span data-stu-id="3cbce-113">hello following example creates a resource group named *myResourceGroup* in hello *westcentralus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-container-registry"></a><span data-ttu-id="3cbce-114">Tároló-beállításjegyzék létrehozása</span><span class="sxs-lookup"><span data-stu-id="3cbce-114">Create a container registry</span></span>

<span data-ttu-id="3cbce-115">Hozzon létre egy ACR példányt hello segítségével [az acr létrehozása](/cli/azure/acr#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="3cbce-115">Create an ACR instance using hello [az acr create](/cli/azure/acr#create) command.</span></span>

> [!NOTE]
> <span data-ttu-id="3cbce-116">A beállításjegyzék létrehozásakor adjon meg egy globálisan egyedi legfelső szintű tartománynevet, amely csak betűket és számokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3cbce-116">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span>

 <span data-ttu-id="3cbce-117">hello beállításjegyzék neve hello példában *myContainerRegistry1*, helyettesítse a saját, egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="3cbce-117">hello registry name in hello example is *myContainerRegistry1*, substitute a unique name of your own.</span></span>

```azurecli
az acr create --name myContainerRegistry1 --resource-group myResourceGroup --sku Managed_Standard
```

<span data-ttu-id="3cbce-118">Hello beállításjegyzék létrehozásakor hello kimenete hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="3cbce-118">When hello registry is created, hello output is similar toohello following:</span></span>

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-29T04:50:28.607134+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry1",
  "location": "westcentralus",
  "loginServer": "mycontainerregistry1.azurecr.io",
  "name": "myContainerRegistry1",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Managed_Standard",
    "tier": "Managed"
  },
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="log-in-tooacr-instance"></a><span data-ttu-id="3cbce-119">Jelentkezzen be tooACR példány</span><span class="sxs-lookup"><span data-stu-id="3cbce-119">Log in tooACR instance</span></span>

<span data-ttu-id="3cbce-120">Kérdez le, és húzza a tároló képek, előtt be kell jelentkeznie toohello ACR példány.</span><span class="sxs-lookup"><span data-stu-id="3cbce-120">Before pushing and pulling container images, you must log in toohello ACR instance.</span></span> <span data-ttu-id="3cbce-121">toodo Igen, használja a hello [az acr bejelentkezési](/cli/azure/acr#login) parancsot.</span><span class="sxs-lookup"><span data-stu-id="3cbce-121">toodo so, use hello [az acr login](/cli/azure/acr#login) command.</span></span>

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

<span data-ttu-id="3cbce-122">hello parancs visszaadja a "Sikeres bejelentkezés" üzenet, amint befejeződött.</span><span class="sxs-lookup"><span data-stu-id="3cbce-122">hello command returns a 'Login Succeeded' message once completed.</span></span>

## <a name="use-azure-container-registry"></a><span data-ttu-id="3cbce-123">Az Azure Container Registry használata</span><span class="sxs-lookup"><span data-stu-id="3cbce-123">Use Azure Container Registry</span></span>

### <a name="list-container-images"></a><span data-ttu-id="3cbce-124">Tárolórendszerképek listázása</span><span class="sxs-lookup"><span data-stu-id="3cbce-124">List container images</span></span>

<span data-ttu-id="3cbce-125">Használjon hello `az acr` parancssori felület parancsai tooquery hello képek és címkék tárházban.</span><span class="sxs-lookup"><span data-stu-id="3cbce-125">Use hello `az acr` CLI commands tooquery hello images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="3cbce-126">Jelenleg, tároló-beállításjegyzék nem támogatja a hello `docker search` parancs tooquery képek és címkék.</span><span class="sxs-lookup"><span data-stu-id="3cbce-126">Currently, Container Registry does not support hello `docker search` command tooquery for images and tags.</span></span>

### <a name="list-repositories"></a><span data-ttu-id="3cbce-127">Adattárak listázása</span><span class="sxs-lookup"><span data-stu-id="3cbce-127">List repositories</span></span>

<span data-ttu-id="3cbce-128">hello alábbi példa felsorolja a beállításjegyzék (JavaScript Object Notation) JSON formátumban hello adattárak:</span><span class="sxs-lookup"><span data-stu-id="3cbce-128">hello following example lists hello repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="3cbce-129">Címkék listázása</span><span class="sxs-lookup"><span data-stu-id="3cbce-129">List tags</span></span>

<span data-ttu-id="3cbce-130">hello alábbi példa felsorolja a hello hello címkék **minták/nginx** tárház JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="3cbce-130">hello following example lists hello tags on hello **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="3cbce-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3cbce-131">Next steps</span></span>

<span data-ttu-id="3cbce-132">A gyors üzembe helyezési a létrehozott egy felügyelt Azure tároló beállításjegyzék-példányhoz hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="3cbce-132">In this quick start, you've created a managed Azure Container Registry instance using hello Azure CLI.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3cbce-133">Leküldéses az első kép hello Docker parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="3cbce-133">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
