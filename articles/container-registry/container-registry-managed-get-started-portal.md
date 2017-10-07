---
title: "aaaCreate titkos Docker beállításjegyzék - Azure-portál |} Microsoft Docs"
description: "Első lépések, létrehozását és kezelését a személyes Docker-tároló nyilvántartó, hello Azure-portálon"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: cf3ce0dcf3036d0e9cd1eaf01721deccb00248d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-portal"></a><span data-ttu-id="b152a-103">Hozzon létre egy felügyelt tároló beállításjegyzék hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="b152a-103">Create a managed container registry using hello Azure portal</span></span>

<span data-ttu-id="b152a-104">Az Azure Container Registry egy felügyelt Docker-tárolóregisztrációs adatbázis-szolgáltatás, amely a privát Docker-tárolók rendszerképeinek tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="b152a-104">Azure Container Registry is a managed Docker container registry service used for storing private Docker container images.</span></span> <span data-ttu-id="b152a-105">Ez az útmutató adatokat hello Azure-portál használatával felügyelt Azure-tároló beállításjegyzék példány létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b152a-105">This guide details creating a managed Azure Container Registry instance using hello Azure portal.</span></span>

<span data-ttu-id="b152a-106">A felügyelt Azure-tárolóregisztrációs adatbázisok előzetes verziós kiadásban, és nem az összes régióban érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b152a-106">Managed Azure container registries are in preview and not available in all regions.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="b152a-107">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="b152a-107">Log in tooAzure</span></span>

<span data-ttu-id="b152a-108">Jelentkezzen be toohello http://portal.azure.com: az Azure portál.</span><span class="sxs-lookup"><span data-stu-id="b152a-108">Log in toohello Azure portal at http://portal.azure.com.</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="b152a-109">Tároló-beállításjegyzék létrehozása</span><span class="sxs-lookup"><span data-stu-id="b152a-109">Create a container registry</span></span>

1. <span data-ttu-id="b152a-110">Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.</span><span class="sxs-lookup"><span data-stu-id="b152a-110">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="b152a-111">Hello a piactéren **Azure tároló beállításjegyzék** , és jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="b152a-111">Search hello marketplace for **Azure container registry** and select it.</span></span>

3. <span data-ttu-id="b152a-112">Kattintson a **létrehozása** hello ACR létrehozás panelen megnyitott lesz.</span><span class="sxs-lookup"><span data-stu-id="b152a-112">Click **Create** which will open hello ACR creation blade.</span></span>

    ![A tároló-beállításjegyzékek beállításai](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. <span data-ttu-id="b152a-114">A hello **Azure tároló beállításjegyzék** panelen adja meg a következő információ hello.</span><span class="sxs-lookup"><span data-stu-id="b152a-114">In hello **Azure Container Registry** blade, enter hello following information.</span></span> <span data-ttu-id="b152a-115">Amikor elkészült, kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b152a-115">Click **Create** when you are done.</span></span>

    <span data-ttu-id="b152a-116">a.</span><span class="sxs-lookup"><span data-stu-id="b152a-116">a.</span></span> <span data-ttu-id="b152a-117">**Beállításjegyzék neve**: egy globálisan egyedi legfelső szintű tartománynév az adott beállításjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="b152a-117">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="b152a-118">Ebben a példában hello beállításkulcs értéke *myAzureContainerRegistry1*, de helyettesítse a saját, egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="b152a-118">In this example, hello registry name is *myAzureContainerRegistry1*, but substitute a unique name of your own.</span></span> <span data-ttu-id="b152a-119">hello név csak betűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b152a-119">hello name can contain only letters and numbers.</span></span>

    <span data-ttu-id="b152a-120">b.</span><span class="sxs-lookup"><span data-stu-id="b152a-120">b.</span></span> <span data-ttu-id="b152a-121">**Erőforráscsoport**: Válasszon ki egy létező [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups) vagy egy új hello típusnév.</span><span class="sxs-lookup"><span data-stu-id="b152a-121">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span>

    <span data-ttu-id="b152a-122">c.</span><span class="sxs-lookup"><span data-stu-id="b152a-122">c.</span></span> <span data-ttu-id="b152a-123">**Hely**: válassza ki az Azure-adatközpontban helyet, ahol hello szolgáltatás [elérhető](https://azure.microsoft.com/regions/services/), például a **déli középső Régiójában**.</span><span class="sxs-lookup"><span data-stu-id="b152a-123">**Location**: Select an Azure datacenter location where hello service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="b152a-124">d.</span><span class="sxs-lookup"><span data-stu-id="b152a-124">d.</span></span> <span data-ttu-id="b152a-125">**A rendszergazdai jogú felhasználó**: Ha azt szeretné, egy rendszergazda felhasználó tooaccess hello beállításjegyzék engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b152a-125">**Admin user**: If you want, enable an admin user tooaccess hello registry.</span></span> <span data-ttu-id="b152a-126">Ez a beállítás hello beállításkulcs létrehozása után módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="b152a-126">You can change this setting after creating hello registry.</span></span>

    <span data-ttu-id="b152a-127">e.</span><span class="sxs-lookup"><span data-stu-id="b152a-127">e.</span></span> <span data-ttu-id="b152a-128">**Használja a felügyelt beállításjegyzék**: kattintson az Igen toohave ACR automatikusan hello beállításjegyzék tárterület kezelése, webhookok használja, és az AAD-hitelesítés használatára.</span><span class="sxs-lookup"><span data-stu-id="b152a-128">**Use managed registry**: Select yes toohave ACR automatically manage hello registry storage, use webhooks, and use AAD authentication.</span></span>

    <span data-ttu-id="b152a-129">f.</span><span class="sxs-lookup"><span data-stu-id="b152a-129">f.</span></span> <span data-ttu-id="b152a-130">**Tarifacsomag**: Válasszon egy tarifacsomagot. További információért tekintse meg itt az ACR-díjszabást.</span><span class="sxs-lookup"><span data-stu-id="b152a-130">**Pricing Tier**: Select a pricing tier, see here ACR pricing for more information.</span></span>

## <a name="log-in-tooacr-instance"></a><span data-ttu-id="b152a-131">Jelentkezzen be tooACR példány</span><span class="sxs-lookup"><span data-stu-id="b152a-131">Log in tooACR instance</span></span>

<span data-ttu-id="b152a-132">Kérdez le, és húzza a tároló képek, előtt be kell jelentkeznie toohello ACR példány.</span><span class="sxs-lookup"><span data-stu-id="b152a-132">Before pushing and pulling container images, you must log in toohello ACR instance.</span></span> 

<span data-ttu-id="b152a-133">toodo tehát hello Azure CLI 2.0 használja.</span><span class="sxs-lookup"><span data-stu-id="b152a-133">toodo so, use hello Azure CLI 2.0.</span></span> <span data-ttu-id="b152a-134">Első lépésként szükség esetén jelentkezzen be Azure-ban hello [az bejelentkezési](/cli/azure/#login) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b152a-134">First, if needed, log into Azure using hello [az login](/cli/azure/#login) command.</span></span> 

```azurecli
az login
```

<span data-ttu-id="b152a-135">Következő lépésként az hello [az acr bejelentkezési](/cli/azure/acr#login) parancs toolog az Azure-tároló beállításjegyzék toohello.</span><span class="sxs-lookup"><span data-stu-id="b152a-135">Next, use hello [az acr login](/cli/azure/acr#login) command toolog in toohello Azure Container Registry.</span></span>

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a><span data-ttu-id="b152a-136">Az Azure Container Registry használata</span><span class="sxs-lookup"><span data-stu-id="b152a-136">Use Azure Container Registry</span></span>

### <a name="list-container-images"></a><span data-ttu-id="b152a-137">Tárolórendszerképek listázása</span><span class="sxs-lookup"><span data-stu-id="b152a-137">List container images</span></span>

<span data-ttu-id="b152a-138">Használjon hello `az acr` parancssori felület parancsai tooquery hello képek és címkék tárházban.</span><span class="sxs-lookup"><span data-stu-id="b152a-138">Use hello `az acr` CLI commands tooquery hello images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="b152a-139">Jelenleg, tároló-beállításjegyzék nem támogatja a hello `docker search` parancs tooquery képek és címkék.</span><span class="sxs-lookup"><span data-stu-id="b152a-139">Currently, Container Registry does not support hello `docker search` command tooquery for images and tags.</span></span>

### <a name="list-repositories"></a><span data-ttu-id="b152a-140">Adattárak listázása</span><span class="sxs-lookup"><span data-stu-id="b152a-140">List repositories</span></span>

<span data-ttu-id="b152a-141">hello alábbi példa felsorolja a beállításjegyzék (JavaScript Object Notation) JSON formátumban hello adattárak:</span><span class="sxs-lookup"><span data-stu-id="b152a-141">hello following example lists hello repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="b152a-142">Címkék listázása</span><span class="sxs-lookup"><span data-stu-id="b152a-142">List tags</span></span>

<span data-ttu-id="b152a-143">hello alábbi példa felsorolja a hello hello címkék **minták/nginx** tárház JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="b152a-143">hello following example lists hello tags on hello **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="b152a-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b152a-144">Next steps</span></span>

<span data-ttu-id="b152a-145">A gyors üzembe helyezési a létrehozott egy felügyelt Azure-tároló beállításjegyzék-példányhoz hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="b152a-145">In this quick start, you've created a managed Azure Container Registry instance using hello Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b152a-146">Leküldéses az első kép hello Docker parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="b152a-146">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
