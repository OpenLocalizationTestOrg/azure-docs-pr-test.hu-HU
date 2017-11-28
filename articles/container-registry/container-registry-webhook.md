---
title: "aaaAzure tároló beállításjegyzék webhookok |} Microsoft Docs"
description: "Azure-tárolót beállításjegyzék webhookok"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: "Docker, tárolók, ACR"
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2017
ms.author: nepeters
ms.openlocfilehash: adc2afec486007e2d54cd689e6f7ef8b1098db06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-container-registry-webhooks---azure-portal"></a><span data-ttu-id="62ae2-104">Azure-tároló beállításjegyzék webhookok - Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="62ae2-104">Using Azure Container Registry webhooks - Azure portal</span></span>

<span data-ttu-id="62ae2-105">Egy Azure-tárolót beállításjegyzék tárolja, és kezeli a saját Docker tároló képek, hasonló toohello módon Docker Hub nyilvános Docker-lemezképet tárolja.</span><span class="sxs-lookup"><span data-stu-id="62ae2-105">An Azure container registry stores and manages private Docker container images, similar toohello way Docker Hub stores public Docker images.</span></span> <span data-ttu-id="62ae2-106">Egyes műveletekre kerül sor egy, a beállításjegyzék-adattárak webhookok tootrigger események használhatja.</span><span class="sxs-lookup"><span data-stu-id="62ae2-106">You use webhooks tootrigger events when certain actions take place in one of your registry repositories.</span></span> <span data-ttu-id="62ae2-107">Webhook válaszolhassanak tooevents hello beállításjegyzék szinten, vagy azok le tooa adott tárház címke hatóköre beállítható úgy.</span><span class="sxs-lookup"><span data-stu-id="62ae2-107">Webhooks can respond tooevents at hello registry level or they can be scoped down tooa specific repository tag.</span></span> 

<span data-ttu-id="62ae2-108">További háttér és fogalmak: hello [áttekintése](./container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="62ae2-108">For more background and concepts, see hello [overview](./container-registry-intro.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62ae2-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="62ae2-109">Prerequisites</span></span> 

- <span data-ttu-id="62ae2-110">Azure-tárolót felügyelt beállításjegyzék - hozzon létre egy felügyelt tároló beállításjegyzék az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="62ae2-110">Azure container managed registry - Create a managed container registry in your Azure subscription.</span></span> <span data-ttu-id="62ae2-111">Például hello Azure-portált használja, vagy Azure CLI 2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="62ae2-111">For example, use hello Azure portal or hello Azure CLI 2.0.</span></span> 
- <span data-ttu-id="62ae2-112">A docker parancssori felület - tooset egy Docker gazdagép és a hozzáférés hello Docker parancssori felület parancsait, mint a helyi számítógép telepítéséhez Docker-motorhoz.</span><span class="sxs-lookup"><span data-stu-id="62ae2-112">Docker CLI - tooset up your local computer as a Docker host and access hello Docker CLI commands, install Docker Engine.</span></span> 

## <a name="create-webhook-azure-portal"></a><span data-ttu-id="62ae2-113">Azure-portálon webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="62ae2-113">Create webhook Azure portal</span></span>

1. <span data-ttu-id="62ae2-114">Jelentkezzen be Azure-portálon toohello, és keresse meg a kívánt toocreate webhookok toohello beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="62ae2-114">Log in toohello Azure portal and navigate toohello registry in which you want toocreate webhooks.</span></span> 

2. <span data-ttu-id="62ae2-115">Hello tároló paneljén válassza a hello "Webhookok" lapot.</span><span class="sxs-lookup"><span data-stu-id="62ae2-115">In hello container blade, select hello "Webhooks" tab.</span></span> 

3. <span data-ttu-id="62ae2-116">Válassza ki a "Hozzáadás" hello webhook panel eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="62ae2-116">Select "Add" from hello webhook blade toolbar.</span></span> 

4. <span data-ttu-id="62ae2-117">Teljes hello *Webhook létrehozása* űrlap, a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="62ae2-117">Complete hello *Create Webhook* form with hello following information:</span></span>

| <span data-ttu-id="62ae2-118">Érték</span><span class="sxs-lookup"><span data-stu-id="62ae2-118">Value</span></span> | <span data-ttu-id="62ae2-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="62ae2-119">Description</span></span> |
|---|---|
| <span data-ttu-id="62ae2-120">Név</span><span class="sxs-lookup"><span data-stu-id="62ae2-120">Name</span></span> | <span data-ttu-id="62ae2-121">azt szeretné, hogy toogive toohello webhook hello neve.</span><span class="sxs-lookup"><span data-stu-id="62ae2-121">hello name you want toogive toohello webhook.</span></span> <span data-ttu-id="62ae2-122">Ez csak kisbetűket és számokat tartalmazhat, és 5-50 karakter közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="62ae2-122">It can only contain lowercase letters and numbers and between 5-50 characters.</span></span> |
| <span data-ttu-id="62ae2-123">Szolgáltatás URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="62ae2-123">Service URI</span></span> | <span data-ttu-id="62ae2-124">hello URI, ahol hello webhook POST értesítést küldjön-e.</span><span class="sxs-lookup"><span data-stu-id="62ae2-124">hello URI where hello webhook should send POST notifications.</span></span> |
| <span data-ttu-id="62ae2-125">Egyéni fejlécek</span><span class="sxs-lookup"><span data-stu-id="62ae2-125">Custom headers</span></span> | <span data-ttu-id="62ae2-126">A fejlécek kívánt toopass hello POST kéréssel együtt.</span><span class="sxs-lookup"><span data-stu-id="62ae2-126">Headers you want toopass along with hello POST request.</span></span> <span data-ttu-id="62ae2-127">Össze kell a "kulcs: érték" formátumban.</span><span class="sxs-lookup"><span data-stu-id="62ae2-127">They should be in "key: value" format.</span></span> |
| <span data-ttu-id="62ae2-128">Eseményindító műveletek</span><span class="sxs-lookup"><span data-stu-id="62ae2-128">Trigger actions</span></span> | <span data-ttu-id="62ae2-129">Hello webhook kiváltó műveletek.</span><span class="sxs-lookup"><span data-stu-id="62ae2-129">Actions that trigger hello webhook.</span></span> <span data-ttu-id="62ae2-130">Jelenleg webhookok forrása a leküldéses és/vagy a műveletek tooan lemezkép törlése.</span><span class="sxs-lookup"><span data-stu-id="62ae2-130">Currently webhooks can be triggered by push and/or delete actions tooan image.</span></span> |
| <span data-ttu-id="62ae2-131">status</span><span class="sxs-lookup"><span data-stu-id="62ae2-131">Status</span></span> | <span data-ttu-id="62ae2-132">hello állapota hello webhook létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="62ae2-132">hello status for hello webhook after it's created.</span></span> <span data-ttu-id="62ae2-133">Alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="62ae2-133">It's enabled by default.</span></span> |
| <span data-ttu-id="62ae2-134">Hatókör</span><span class="sxs-lookup"><span data-stu-id="62ae2-134">Scope</span></span> | <span data-ttu-id="62ae2-135">mely hello webhook works hello hatókörén.</span><span class="sxs-lookup"><span data-stu-id="62ae2-135">hello scope at which hello webhook works.</span></span> <span data-ttu-id="62ae2-136">Alapértelmezés szerint hello hatóköre hello beállításjegyzékben található összes esemény.</span><span class="sxs-lookup"><span data-stu-id="62ae2-136">By default hello scope is for all events in hello registry.</span></span> <span data-ttu-id="62ae2-137">Azt adható meg a tárház vagy egy címkét a hello formátumban "tárház: címke".</span><span class="sxs-lookup"><span data-stu-id="62ae2-137">It can be specified for a repository or a tag by using hello format "repository: tag".</span></span> |

<span data-ttu-id="62ae2-138">Példa webhook űrlap:</span><span class="sxs-lookup"><span data-stu-id="62ae2-138">Example webhook form:</span></span>

![VEZÉNYLŐTÍPUSÚ FELHASZNÁLÓI FELÜLET](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a><span data-ttu-id="62ae2-140">Az Azure parancssori felület webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="62ae2-140">Create webhook Azure CLI</span></span>

<span data-ttu-id="62ae2-141">a webhook használatával toocreate hello Azure CLI használata hello [az acr webhook létrehozása](/cli/azure/acr/webhook#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="62ae2-141">toocreate a webhook using hello Azure CLI, use hello [az acr webhook create](/cli/azure/acr/webhook#create) command.</span></span>

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a><span data-ttu-id="62ae2-142">Teszt webhook</span><span class="sxs-lookup"><span data-stu-id="62ae2-142">Test webhook</span></span>

### <a name="azure-portal"></a><span data-ttu-id="62ae2-143">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="62ae2-143">Azure portal</span></span>

<span data-ttu-id="62ae2-144">Előzetes toousing hello webhook tárolóra leküldéses lemezképet, és törlési műveletek, hello segítségével vizsgálható **Ping** gombra.</span><span class="sxs-lookup"><span data-stu-id="62ae2-144">Prior toousing hello webhook on container image push and delete actions, it can be tested using hello **Ping** button.</span></span> <span data-ttu-id="62ae2-145">Használatakor a hello Ping egy általános post kérelem toohello megadott végpont és a naplók hello választ küldi.</span><span class="sxs-lookup"><span data-stu-id="62ae2-145">When used, hello Ping sends a generic post request toohello specified endpoint and logs hello response.</span></span> <span data-ttu-id="62ae2-146">Ez akkor hasznos, amelyek hello webhook tooverify megfelelően be van állítva.</span><span class="sxs-lookup"><span data-stu-id="62ae2-146">This is helpful tooverify that hello webhook has been set up correctly.</span></span>

1. <span data-ttu-id="62ae2-147">Válassza ki a kívánt tootest hello webhook.</span><span class="sxs-lookup"><span data-stu-id="62ae2-147">Select hello webhook you want tootest.</span></span> 
2. <span data-ttu-id="62ae2-148">Hello felső eszköztárán válassza hello "Pingelje" műveletet.</span><span class="sxs-lookup"><span data-stu-id="62ae2-148">In hello top toolbar, select hello "Ping" action.</span></span> 
3. <span data-ttu-id="62ae2-149">Ellenőrizze a hello kérelem és válasz.</span><span class="sxs-lookup"><span data-stu-id="62ae2-149">Check hello request and response.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="62ae2-150">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="62ae2-150">Azure CLI</span></span>

<span data-ttu-id="62ae2-151">tootest egy ACR webhook hello Azure CLI használata hello [az acr webhook ping](/cli/azure/acr/webhook#ping) parancsot.</span><span class="sxs-lookup"><span data-stu-id="62ae2-151">tootest an ACR webhook with hello Azure CLI, use hello [az acr webhook ping](/cli/azure/acr/webhook#ping) command.</span></span>

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

<span data-ttu-id="62ae2-152">toosee hello eredmény elérése érdekében használjon hello [az acr webhook lista-események](/cli/azure/acr/webhook#list-events) parancsot.</span><span class="sxs-lookup"><span data-stu-id="62ae2-152">toosee hello results, use hello [az acr webhook list-events](/cli/azure/acr/webhook#list-events) command.</span></span> 

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a><span data-ttu-id="62ae2-153">Webhook törlése</span><span class="sxs-lookup"><span data-stu-id="62ae2-153">Delete webhook</span></span>

### <a name="azure-portal"></a><span data-ttu-id="62ae2-154">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="62ae2-154">Azure portal</span></span>

<span data-ttu-id="62ae2-155">Minden egyes webhook hello webhook és hello Törlés gomb az Azure-portálon hello kiválasztásával törölheti.</span><span class="sxs-lookup"><span data-stu-id="62ae2-155">Each webhook can be deleted by selecting hello webhook and then hello delete button on hello Azure portal.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="62ae2-156">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="62ae2-156">Azure CLI</span></span>

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```