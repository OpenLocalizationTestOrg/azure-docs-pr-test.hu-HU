---
title: "Azure tároló példányok útmutató – Azure tároló beállításjegyzék előkészítése |} Microsoft Docs"
description: "Azure tároló példányok útmutató – Azure tároló beállításjegyzék előkészítése"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: cc96ba9f5abd45a7503ba3327b30e1f809391384
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-use-azure-container-registry"></a><span data-ttu-id="b6681-104">Üzembe helyezés és használat Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="b6681-104">Deploy and use Azure Container Registry</span></span>

<span data-ttu-id="b6681-105">Ez az egy háromrészes oktatóanyag második rész.</span><span class="sxs-lookup"><span data-stu-id="b6681-105">This is part two of a three-part tutorial.</span></span> <span data-ttu-id="b6681-106">Az a [előző lépésben](./container-instances-tutorial-prepare-app.md), a tároló-lemezkép létrejött egy egyszerű webes alkalmazáshoz írt [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="b6681-106">In the [previous step](./container-instances-tutorial-prepare-app.md), a container image was created for a simple web application written in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="b6681-107">Ez a kép ebben az oktatóanyagban a rendszer előkészítésre továbbít egy Azure-tároló beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="b6681-107">In this tutorial, this image is pushed to an Azure Container Registry.</span></span> <span data-ttu-id="b6681-108">A tároló kép nem hozott létre, ha vissza [oktatóanyag 1 – tároló kép létrehozása](./container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="b6681-108">If you have not created the container image, return to [Tutorial 1 – Create container image](./container-instances-tutorial-prepare-app.md).</span></span> 

<span data-ttu-id="b6681-109">Az Azure-tároló beállításjegyzék egy Azure-alapú, személyes beállításjegyzék Docker-tároló lemezképek.</span><span class="sxs-lookup"><span data-stu-id="b6681-109">The Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="b6681-110">Ez az oktatóanyag végigvezeti Azure tároló beállításjegyzék-példány telepítését, valamint a végez leküldést a tároló-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="b6681-110">This tutorial walks through deploying an Azure Container Registry instance, and pushing a container image to it.</span></span> <span data-ttu-id="b6681-111">Befejeződött a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="b6681-111">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b6681-112">Egy Azure-tároló beállításjegyzék-példány telepítése</span><span class="sxs-lookup"><span data-stu-id="b6681-112">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="b6681-113">Azure-tároló beállításjegyzék címkézési tároló képe</span><span class="sxs-lookup"><span data-stu-id="b6681-113">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="b6681-114">Azure-tároló beállításjegyzék Rendszerkép feltöltése</span><span class="sxs-lookup"><span data-stu-id="b6681-114">Uploading image to Azure Container Registry</span></span>

<span data-ttu-id="b6681-115">A következő útmutatókból telepít a tároló a személyes beállításjegyzékből Azure tároló példányok.</span><span class="sxs-lookup"><span data-stu-id="b6681-115">In subsequent tutorials, you deploy the container from your private registry to Azure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b6681-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b6681-116">Before you begin</span></span>

<span data-ttu-id="b6681-117">Ez az oktatóanyag megköveteli, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="b6681-117">This tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b6681-118">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b6681-118">Run `az --version` to find the version.</span></span> <span data-ttu-id="b6681-119">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b6681-119">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="b6681-120">Telepítse az Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="b6681-120">Deploy Azure Container Registry</span></span>

<span data-ttu-id="b6681-121">Egy Azure-tároló beállításjegyzék való telepítésekor, először egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b6681-121">When deploying an Azure Container Registry, you first need a resource group.</span></span> <span data-ttu-id="b6681-122">Egy Azure-erőforráscsoportot gyűjteményei logikai mely Azure az erőforrások telepítése és kezelése.</span><span class="sxs-lookup"><span data-stu-id="b6681-122">An Azure resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="b6681-123">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="b6681-123">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="b6681-124">Ebben a példában az erőforráscsoport neve *myResourceGroup* jön létre a *eastus* régióban.</span><span class="sxs-lookup"><span data-stu-id="b6681-124">In this example, a resource group named *myResourceGroup* is created in the *eastus* region.</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="b6681-125">Hozzon létre egy Azure-tárolóba beállításjegyzéket a [az acr létrehozása](/cli/azure/acr#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6681-125">Create an Azure Container registry with the [az acr create](/cli/azure/acr#create) command.</span></span> <span data-ttu-id="b6681-126">Egy tároló beállításjegyzék neve **egyedinek kell lennie**.</span><span class="sxs-lookup"><span data-stu-id="b6681-126">The name of a Container Registry **must be unique**.</span></span> <span data-ttu-id="b6681-127">A következő példában használjuk a neve *mycontainerregistry082*.</span><span class="sxs-lookup"><span data-stu-id="b6681-127">In the following example, we use the name *mycontainerregistry082*.</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

<span data-ttu-id="b6681-128">Ez az oktatóanyag a többi, egész használjuk `<acrname>` a tároló neve választott számára.</span><span class="sxs-lookup"><span data-stu-id="b6681-128">Throughout the rest of this tutorial, we use `<acrname>` as a placeholder for the container registry name that you chose.</span></span>

## <a name="container-registry-login"></a><span data-ttu-id="b6681-129">Tároló beállításjegyzék bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b6681-129">Container registry login</span></span>

<span data-ttu-id="b6681-130">A ACR példányát előtt képek rá kell bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="b6681-130">You must log in to your ACR instance before pushing images to it.</span></span> <span data-ttu-id="b6681-131">Használja a [az acr bejelentkezési](https://docs.microsoft.com/en-us/cli/azure/acr#login) parancs használatával végrehajtani a műveletet.</span><span class="sxs-lookup"><span data-stu-id="b6681-131">Use the [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login) command to complete the operation.</span></span> <span data-ttu-id="b6681-132">Meg kell adnia az egyedi név, a tároló beállításjegyzék létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b6681-132">You need to provide the unique name given to the container registry when it was created.</span></span>

```azurecli
az acr login --name <acrName>
```

<span data-ttu-id="b6681-133">A parancs visszaadja a "Sikeres bejelentkezés" üzenet, amint befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b6681-133">The command returns a 'Login Succeeded’ message once completed.</span></span>

## <a name="tag-container-image"></a><span data-ttu-id="b6681-134">Címke tároló kép</span><span class="sxs-lookup"><span data-stu-id="b6681-134">Tag container image</span></span>

<span data-ttu-id="b6681-135">A titkos beállításjegyzékből tároló lemezkép telepítéséhez, a lemezkép kell címkézését a `loginServer` nevét a beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="b6681-135">To deploy a container image from a private registry, the image needs to be tagged with the `loginServer` name of the registry.</span></span>

<span data-ttu-id="b6681-136">Lemezképek aktuális listájának megtekintéséhez használja a `docker images` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6681-136">To see a list of current images, use the `docker images` command.</span></span>

```bash
docker images
```

<span data-ttu-id="b6681-137">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b6681-137">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

<span data-ttu-id="b6681-138">Ahhoz, hogy a loginServer nevét, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6681-138">To get the loginServer name, run the following command.</span></span>

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

<span data-ttu-id="b6681-139">Címke a *aci-oktatóanyag – alkalmazás* a tároló beállításjegyzék loginServer lemezkép.</span><span class="sxs-lookup"><span data-stu-id="b6681-139">Tag the *aci-tutorial-app* image with the loginServer of the container registry.</span></span> <span data-ttu-id="b6681-140">Továbbá adja hozzá `:v1` , a lemezkép neve végén.</span><span class="sxs-lookup"><span data-stu-id="b6681-140">Also, add `:v1` to the end of the image name.</span></span> <span data-ttu-id="b6681-141">Ezt a címkét azt jelzi, hogy a lemezkép verziószámát.</span><span class="sxs-lookup"><span data-stu-id="b6681-141">This tag indicates the image version number.</span></span>

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

<span data-ttu-id="b6681-142">Miután megjelölve, futtassa az `docker images` ellenőrzése a műveletet.</span><span class="sxs-lookup"><span data-stu-id="b6681-142">Once tagged, run `docker images` to verify the operation.</span></span>

```bash
docker images
```

<span data-ttu-id="b6681-143">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b6681-143">Output:</span></span>

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a><span data-ttu-id="b6681-144">Azure-tároló beállításjegyzék leküldéses kép</span><span class="sxs-lookup"><span data-stu-id="b6681-144">Push image to Azure Container Registry</span></span>

<span data-ttu-id="b6681-145">Leküldéses a *aci-oktatóanyag – alkalmazás* kép a beállításjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="b6681-145">Push the *aci-tutorial-app* image to the registry.</span></span>

<span data-ttu-id="b6681-146">Az alábbi példa használatával, a tároló loginServer neve cserélje le a környezetből loginServer.</span><span class="sxs-lookup"><span data-stu-id="b6681-146">Using the following example, replace the container registry loginServer name with the loginServer from your environment.</span></span>

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a><span data-ttu-id="b6681-147">Azure-tároló beállításjegyzék lemezképek felsorolása</span><span class="sxs-lookup"><span data-stu-id="b6681-147">List images in Azure Container Registry</span></span>

<span data-ttu-id="b6681-148">Olyan lemezképkészlet, amellyel az Azure-tárolóba beállításjegyzék értesítését listájához való visszatéréshez felhasználói a [az acr tárház lista](/cli/azure/acr/repository#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6681-148">To return a list of images that have been pushed to your Azure Container registry, user the [az acr repository list](/cli/azure/acr/repository#list) command.</span></span> <span data-ttu-id="b6681-149">A parancs frissíti a tároló neve.</span><span class="sxs-lookup"><span data-stu-id="b6681-149">Update the command with the container registry name.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="b6681-150">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b6681-150">Output:</span></span>

```azurecli
Result
----------------
aci-tutorial-app
```

<span data-ttu-id="b6681-151">A címkék azokba megtekintéséhez használja a [az acr tárház megjelenítése-címkék](/cli/azure/acr/repository#show-tags) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6681-151">And then to see the tags for a specific image, use the [az acr repository show-tags](/cli/azure/acr/repository#show-tags) command.</span></span>

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

<span data-ttu-id="b6681-152">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b6681-152">Output:</span></span>

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a><span data-ttu-id="b6681-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6681-153">Next steps</span></span>

<span data-ttu-id="b6681-154">Az oktatóanyag egy Azure-tároló beállításjegyzék Azure tároló példányok való használatra készült, és a tároló kép leküldött volt.</span><span class="sxs-lookup"><span data-stu-id="b6681-154">In this tutorial, an Azure Container Registry was prepared for use with Azure Container Instances, and the container image was pushed.</span></span> <span data-ttu-id="b6681-155">A következő lépéseket hajtotta végre:</span><span class="sxs-lookup"><span data-stu-id="b6681-155">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b6681-156">Egy Azure-tároló beállításjegyzék-példány telepítése</span><span class="sxs-lookup"><span data-stu-id="b6681-156">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="b6681-157">Azure-tároló beállításjegyzék címkézési tároló képe</span><span class="sxs-lookup"><span data-stu-id="b6681-157">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="b6681-158">Azure-tároló beállításjegyzék Rendszerkép feltöltése</span><span class="sxs-lookup"><span data-stu-id="b6681-158">Uploading image to Azure Container Registry</span></span>

<span data-ttu-id="b6681-159">A következő oktatóanyag az Azure-ban Azure tároló példányok a tároló telepítésével kapcsolatos további továbblépés.</span><span class="sxs-lookup"><span data-stu-id="b6681-159">Advance to the next tutorial to learn about deploying the container to Azure using Azure Container Instances.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b6681-160">Azure-tároló példányok – tárolók üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="b6681-160">Deploy containers to Azure Container Instances</span></span>](./container-instances-tutorial-deploy-app.md)
