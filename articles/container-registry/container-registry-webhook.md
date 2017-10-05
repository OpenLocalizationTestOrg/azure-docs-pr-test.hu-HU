---
title: "Azure-tárolót beállításjegyzék webhookok |} Microsoft Docs"
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
ms.openlocfilehash: d0190f5725671c320d92b897f0dcef7a526a86e3
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="using-azure-container-registry-webhooks---azure-portal"></a><span data-ttu-id="b534e-104">Azure-tároló beállításjegyzék webhookok - Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="b534e-104">Using Azure Container Registry webhooks - Azure portal</span></span>

<span data-ttu-id="b534e-105">Egy Azure-tárolót beállításjegyzék tárolja, és kezeli a saját Docker tároló képek, hasonló ahhoz, ahogy a Docker Hub nyilvános Docker-lemezképet tárolja.</span><span class="sxs-lookup"><span data-stu-id="b534e-105">An Azure container registry stores and manages private Docker container images, similar to the way Docker Hub stores public Docker images.</span></span> <span data-ttu-id="b534e-106">Bizonyos műveleteket kerül sor egy, a beállításjegyzék-adattárak eseményindító események webhookok használhatja.</span><span class="sxs-lookup"><span data-stu-id="b534e-106">You use webhooks to trigger events when certain actions take place in one of your registry repositories.</span></span> <span data-ttu-id="b534e-107">Webhook válaszolhassanak a beállításjegyzék szinten események, vagy azok le egy adott tárház címke hatóköre beállítható úgy.</span><span class="sxs-lookup"><span data-stu-id="b534e-107">Webhooks can respond to events at the registry level or they can be scoped down to a specific repository tag.</span></span> 

<span data-ttu-id="b534e-108">További háttér és fogalmak a [áttekintése](./container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="b534e-108">For more background and concepts, see the [overview](./container-registry-intro.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b534e-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b534e-109">Prerequisites</span></span> 

- <span data-ttu-id="b534e-110">Azure-tárolót felügyelt beállításjegyzék - hozzon létre egy felügyelt tároló beállításjegyzék az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b534e-110">Azure container managed registry - Create a managed container registry in your Azure subscription.</span></span> <span data-ttu-id="b534e-111">Például használja az Azure-portálon vagy az Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="b534e-111">For example, use the Azure portal or the Azure CLI 2.0.</span></span> 
- <span data-ttu-id="b534e-112">A docker parancssori felület – Docker-gazdagépként a helyi számítógépet, és a Docker parancssori felület parancsait eléréséhez telepítse a Docker-motorhoz.</span><span class="sxs-lookup"><span data-stu-id="b534e-112">Docker CLI - To set up your local computer as a Docker host and access the Docker CLI commands, install Docker Engine.</span></span> 

## <a name="create-webhook-azure-portal"></a><span data-ttu-id="b534e-113">Azure-portálon webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="b534e-113">Create webhook Azure portal</span></span>

1. <span data-ttu-id="b534e-114">Jelentkezzen be az Azure-portálon, és keresse meg a beállításjegyzéket, amelyen létrehozásához webhook.</span><span class="sxs-lookup"><span data-stu-id="b534e-114">Log in to the Azure portal and navigate to the registry in which you want to create webhooks.</span></span> 

2. <span data-ttu-id="b534e-115">A tároló paneljén kattintson a "Webhookok" fülre.</span><span class="sxs-lookup"><span data-stu-id="b534e-115">In the container blade, select the "Webhooks" tab.</span></span> 

3. <span data-ttu-id="b534e-116">Válassza ki a "Hozzáadás" a webhook panel eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="b534e-116">Select "Add" from the webhook blade toolbar.</span></span> 

4. <span data-ttu-id="b534e-117">Fejezze be a *Webhook létrehozása* képernyőn a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="b534e-117">Complete the *Create Webhook* form with the following information:</span></span>

| <span data-ttu-id="b534e-118">Érték</span><span class="sxs-lookup"><span data-stu-id="b534e-118">Value</span></span> | <span data-ttu-id="b534e-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="b534e-119">Description</span></span> |
|---|---|
| <span data-ttu-id="b534e-120">Név</span><span class="sxs-lookup"><span data-stu-id="b534e-120">Name</span></span> | <span data-ttu-id="b534e-121">Kíván beállítani a webhook nevét.</span><span class="sxs-lookup"><span data-stu-id="b534e-121">The name you want to give to the webhook.</span></span> <span data-ttu-id="b534e-122">Ez csak kisbetűket és számokat tartalmazhat, és 5-50 karakter közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="b534e-122">It can only contain lowercase letters and numbers and between 5-50 characters.</span></span> |
| <span data-ttu-id="b534e-123">Szolgáltatás URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="b534e-123">Service URI</span></span> | <span data-ttu-id="b534e-124">Az URI, ahol a webhook POST értesítést küldjön-e.</span><span class="sxs-lookup"><span data-stu-id="b534e-124">The URI where the webhook should send POST notifications.</span></span> |
| <span data-ttu-id="b534e-125">Egyéni fejlécek</span><span class="sxs-lookup"><span data-stu-id="b534e-125">Custom headers</span></span> | <span data-ttu-id="b534e-126">A POST-kérelmet együtt átadni kívánt fejléceket.</span><span class="sxs-lookup"><span data-stu-id="b534e-126">Headers you want to pass along with the POST request.</span></span> <span data-ttu-id="b534e-127">Össze kell a "kulcs: érték" formátumban.</span><span class="sxs-lookup"><span data-stu-id="b534e-127">They should be in "key: value" format.</span></span> |
| <span data-ttu-id="b534e-128">Eseményindító műveletek</span><span class="sxs-lookup"><span data-stu-id="b534e-128">Trigger actions</span></span> | <span data-ttu-id="b534e-129">A webhook kiváltó műveletek.</span><span class="sxs-lookup"><span data-stu-id="b534e-129">Actions that trigger the webhook.</span></span> <span data-ttu-id="b534e-130">Jelenleg webhookok forrása a leküldéses és/vagy törlési műveletek a lemezképre.</span><span class="sxs-lookup"><span data-stu-id="b534e-130">Currently webhooks can be triggered by push and/or delete actions to an image.</span></span> |
| <span data-ttu-id="b534e-131">status</span><span class="sxs-lookup"><span data-stu-id="b534e-131">Status</span></span> | <span data-ttu-id="b534e-132">A webhook létrehozása után állapota.</span><span class="sxs-lookup"><span data-stu-id="b534e-132">The status for the webhook after it's created.</span></span> <span data-ttu-id="b534e-133">Alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b534e-133">It's enabled by default.</span></span> |
| <span data-ttu-id="b534e-134">Hatókör</span><span class="sxs-lookup"><span data-stu-id="b534e-134">Scope</span></span> | <span data-ttu-id="b534e-135">A hatókör, ahol a webhook működik.</span><span class="sxs-lookup"><span data-stu-id="b534e-135">The scope at which the webhook works.</span></span> <span data-ttu-id="b534e-136">Alapértelmezés szerint a hatóköre az összes esemény a beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="b534e-136">By default the scope is for all events in the registry.</span></span> <span data-ttu-id="b534e-137">Az adható meg a tárház vagy egy címkét a következő formátumban "tárház: címke".</span><span class="sxs-lookup"><span data-stu-id="b534e-137">It can be specified for a repository or a tag by using the format "repository: tag".</span></span> |

<span data-ttu-id="b534e-138">Példa webhook űrlap:</span><span class="sxs-lookup"><span data-stu-id="b534e-138">Example webhook form:</span></span>

![VEZÉNYLŐTÍPUSÚ FELHASZNÁLÓI FELÜLET](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a><span data-ttu-id="b534e-140">Az Azure parancssori felület webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="b534e-140">Create webhook Azure CLI</span></span>

<span data-ttu-id="b534e-141">A webhook az Azure parancssori felület használatával hozhat létre a [az acr webhook létrehozása](/cli/azure/acr/webhook#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b534e-141">To create a webhook using the Azure CLI, use the [az acr webhook create](/cli/azure/acr/webhook#create) command.</span></span>

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a><span data-ttu-id="b534e-142">Teszt webhook</span><span class="sxs-lookup"><span data-stu-id="b534e-142">Test webhook</span></span>

### <a name="azure-portal"></a><span data-ttu-id="b534e-143">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b534e-143">Azure portal</span></span>

<span data-ttu-id="b534e-144">Való használata előtt a webhook tárolóra kép leküldéses és törlési műveletek, használatával vizsgálható a **Ping** gombra.</span><span class="sxs-lookup"><span data-stu-id="b534e-144">Prior to using the webhook on container image push and delete actions, it can be tested using the **Ping** button.</span></span> <span data-ttu-id="b534e-145">Használatakor a Ping egy általános post kérést küld a megadott végpontot, és naplózza a választ.</span><span class="sxs-lookup"><span data-stu-id="b534e-145">When used, the Ping sends a generic post request to the specified endpoint and logs the response.</span></span> <span data-ttu-id="b534e-146">Ez akkor hasznos győződjön meg arról, hogy a webhook megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="b534e-146">This is helpful to verify that the webhook has been set up correctly.</span></span>

1. <span data-ttu-id="b534e-147">Válassza ki a vizsgálni kívánt webhook.</span><span class="sxs-lookup"><span data-stu-id="b534e-147">Select the webhook you want to test.</span></span> 
2. <span data-ttu-id="b534e-148">A felső eszköztárban látható válassza ki a "Ping" műveletet.</span><span class="sxs-lookup"><span data-stu-id="b534e-148">In the top toolbar, select the "Ping" action.</span></span> 
3. <span data-ttu-id="b534e-149">Ellenőrizze a kérelem és válasz.</span><span class="sxs-lookup"><span data-stu-id="b534e-149">Check the request and response.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="b534e-150">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b534e-150">Azure CLI</span></span>

<span data-ttu-id="b534e-151">Az Azure parancssori felülettel ACR webhook teszteléséhez használja a [az acr webhook ping](/cli/azure/acr/webhook#ping) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b534e-151">To test an ACR webhook with the Azure CLI, use the [az acr webhook ping](/cli/azure/acr/webhook#ping) command.</span></span>

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

<span data-ttu-id="b534e-152">Az eredmények megtekintéséhez használja a [az acr webhook lista-események](/cli/azure/acr/webhook#list-events) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b534e-152">To see the results, use the [az acr webhook list-events](/cli/azure/acr/webhook#list-events) command.</span></span> 

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a><span data-ttu-id="b534e-153">Webhook törlése</span><span class="sxs-lookup"><span data-stu-id="b534e-153">Delete webhook</span></span>

### <a name="azure-portal"></a><span data-ttu-id="b534e-154">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b534e-154">Azure portal</span></span>

<span data-ttu-id="b534e-155">Minden egyes webhook törölheti a webhook és a Törlés gomb az Azure portálon kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="b534e-155">Each webhook can be deleted by selecting the webhook and then the delete button on the Azure portal.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="b534e-156">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b534e-156">Azure CLI</span></span>

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```