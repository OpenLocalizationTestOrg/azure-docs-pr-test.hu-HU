---
title: "aaaAzure tároló példányok oktatóanyag – Azure tároló beállításjegyzék előkészítése |} Microsoft Docs"
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
ms.openlocfilehash: 2525626125740c3c861fad36aad207d0b587ff54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a><span data-ttu-id="d9860-104">Üzembe helyezés és használat Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="d9860-104">Deploy and use Azure Container Registry</span></span>

<span data-ttu-id="d9860-105">Ez az egy háromrészes oktatóanyag második rész.</span><span class="sxs-lookup"><span data-stu-id="d9860-105">This is part two of a three-part tutorial.</span></span> <span data-ttu-id="d9860-106">A hello [előző lépésben](./container-instances-tutorial-prepare-app.md), a tároló-lemezkép létrejött egy egyszerű webes alkalmazáshoz írt [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="d9860-106">In hello [previous step](./container-instances-tutorial-prepare-app.md), a container image was created for a simple web application written in [Node.js](http://nodejs.org).</span></span> <span data-ttu-id="d9860-107">Ebben az oktatóanyagban a kép tooan Azure tároló beállításjegyzék fejlesztőre.</span><span class="sxs-lookup"><span data-stu-id="d9860-107">In this tutorial, this image is pushed tooan Azure Container Registry.</span></span> <span data-ttu-id="d9860-108">Hello tároló kép nem hozott létre, ha vissza túl[oktatóanyag 1 – tároló kép létrehozása](./container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="d9860-108">If you have not created hello container image, return too[Tutorial 1 – Create container image](./container-instances-tutorial-prepare-app.md).</span></span> 

<span data-ttu-id="d9860-109">hello Azure tároló beállításjegyzék egy Azure-alapú, személyes beállításjegyzék Docker-tároló lemezképek.</span><span class="sxs-lookup"><span data-stu-id="d9860-109">hello Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="d9860-110">Ez az oktatóanyag végigvezeti Azure tároló beállításjegyzék-példány telepítését, valamint egy tároló kép tooit kérdez le.</span><span class="sxs-lookup"><span data-stu-id="d9860-110">This tutorial walks through deploying an Azure Container Registry instance, and pushing a container image tooit.</span></span> <span data-ttu-id="d9860-111">Befejeződött a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="d9860-111">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d9860-112">Egy Azure-tároló beállításjegyzék-példány telepítése</span><span class="sxs-lookup"><span data-stu-id="d9860-112">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="d9860-113">Azure-tároló beállításjegyzék címkézési tároló képe</span><span class="sxs-lookup"><span data-stu-id="d9860-113">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="d9860-114">Kép tooAzure tároló beállításjegyzék feltöltése</span><span class="sxs-lookup"><span data-stu-id="d9860-114">Uploading image tooAzure Container Registry</span></span>

<span data-ttu-id="d9860-115">A következő útmutatókból hello tároló a személyes beállításjegyzék tooAzure tároló példányok a telepítése.</span><span class="sxs-lookup"><span data-stu-id="d9860-115">In subsequent tutorials, you deploy hello container from your private registry tooAzure Container Instances.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d9860-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d9860-116">Before you begin</span></span>

<span data-ttu-id="d9860-117">Ez az oktatóanyag megköveteli, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d9860-117">This tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="d9860-118">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="d9860-118">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="d9860-119">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d9860-119">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="d9860-120">Telepítse az Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="d9860-120">Deploy Azure Container Registry</span></span>

<span data-ttu-id="d9860-121">Egy Azure-tároló beállításjegyzék való telepítésekor, először egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d9860-121">When deploying an Azure Container Registry, you first need a resource group.</span></span> <span data-ttu-id="d9860-122">Egy Azure-erőforráscsoportot gyűjteményei logikai mely Azure az erőforrások telepítése és kezelése.</span><span class="sxs-lookup"><span data-stu-id="d9860-122">An Azure resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="d9860-123">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9860-123">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="d9860-124">Ebben a példában az erőforráscsoport neve *myResourceGroup* jön létre az hello *eastus* régióban.</span><span class="sxs-lookup"><span data-stu-id="d9860-124">In this example, a resource group named *myResourceGroup* is created in hello *eastus* region.</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="d9860-125">Hozzon létre egy Azure-tárolóba beállításjegyzék hello [az acr létrehozása](/cli/azure/acr#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9860-125">Create an Azure Container registry with hello [az acr create](/cli/azure/acr#create) command.</span></span> <span data-ttu-id="d9860-126">egy tároló beállításjegyzék hello neve **egyedinek kell lennie**.</span><span class="sxs-lookup"><span data-stu-id="d9860-126">hello name of a Container Registry **must be unique**.</span></span> <span data-ttu-id="d9860-127">A következő példa hello, hello nevét használjuk *mycontainerregistry082*.</span><span class="sxs-lookup"><span data-stu-id="d9860-127">In hello following example, we use hello name *mycontainerregistry082*.</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

<span data-ttu-id="d9860-128">Ebben az oktatóanyagban hello részeinek, egész használjuk `<acrname>` a hello beállításjegyzék Tárolónév választott helyőrzőként.</span><span class="sxs-lookup"><span data-stu-id="d9860-128">Throughout hello rest of this tutorial, we use `<acrname>` as a placeholder for hello container registry name that you chose.</span></span>

## <a name="container-registry-login"></a><span data-ttu-id="d9860-129">Tároló beállításjegyzék bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9860-129">Container registry login</span></span>

<span data-ttu-id="d9860-130">Be kell jelentkeznie tooyour ACR példány képek tooit előtt.</span><span class="sxs-lookup"><span data-stu-id="d9860-130">You must log in tooyour ACR instance before pushing images tooit.</span></span> <span data-ttu-id="d9860-131">Használjon hello [az acr bejelentkezési](https://docs.microsoft.com/en-us/cli/azure/acr#login) toocomplete hello művelet parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9860-131">Use hello [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login) command toocomplete hello operation.</span></span> <span data-ttu-id="d9860-132">Tooprovide hello egyedi adott név toohello tároló beállításjegyzék létrehozásakor van szüksége.</span><span class="sxs-lookup"><span data-stu-id="d9860-132">You need tooprovide hello unique name given toohello container registry when it was created.</span></span>

```azurecli
az acr login --name <acrName>
```

<span data-ttu-id="d9860-133">hello parancs visszaadja a "Sikeres bejelentkezés" üzenet, amint befejeződött.</span><span class="sxs-lookup"><span data-stu-id="d9860-133">hello command returns a 'Login Succeeded’ message once completed.</span></span>

## <a name="tag-container-image"></a><span data-ttu-id="d9860-134">Címke tároló kép</span><span class="sxs-lookup"><span data-stu-id="d9860-134">Tag container image</span></span>

<span data-ttu-id="d9860-135">a tároló lemezkép titkos beállításjegyzékből toodeploy, hello lemezképet kell hello címkéjű toobe `loginServer` hello beállításjegyzék neve.</span><span class="sxs-lookup"><span data-stu-id="d9860-135">toodeploy a container image from a private registry, hello image needs toobe tagged with hello `loginServer` name of hello registry.</span></span>

<span data-ttu-id="d9860-136">az aktuális képek, használjon hello listáját toosee `docker images` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9860-136">toosee a list of current images, use hello `docker images` command.</span></span>

```bash
docker images
```

<span data-ttu-id="d9860-137">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="d9860-137">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

<span data-ttu-id="d9860-138">tooget hello loginServer neve, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="d9860-138">tooget hello loginServer name, run hello following command.</span></span>

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

<span data-ttu-id="d9860-139">Címke hello *aci-oktatóanyag – alkalmazás* hello loginServer hello tároló beállításjegyzék lemezkép.</span><span class="sxs-lookup"><span data-stu-id="d9860-139">Tag hello *aci-tutorial-app* image with hello loginServer of hello container registry.</span></span> <span data-ttu-id="d9860-140">Továbbá adja hozzá `:v1` hello lemezképnév toohello végét.</span><span class="sxs-lookup"><span data-stu-id="d9860-140">Also, add `:v1` toohello end of hello image name.</span></span> <span data-ttu-id="d9860-141">Ezt a címkét azt jelzi, hogy hello lemezkép verziószámát.</span><span class="sxs-lookup"><span data-stu-id="d9860-141">This tag indicates hello image version number.</span></span>

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

<span data-ttu-id="d9860-142">Miután megjelölve, futtassa az `docker images` tooverify hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="d9860-142">Once tagged, run `docker images` tooverify hello operation.</span></span>

```bash
docker images
```

<span data-ttu-id="d9860-143">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="d9860-143">Output:</span></span>

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-tooazure-container-registry"></a><span data-ttu-id="d9860-144">Leküldéses kép tooAzure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="d9860-144">Push image tooAzure Container Registry</span></span>

<span data-ttu-id="d9860-145">Hello leküldéses *aci-oktatóanyag – alkalmazás* kép toohello beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="d9860-145">Push hello *aci-tutorial-app* image toohello registry.</span></span>

<span data-ttu-id="d9860-146">Hello Tárolónév beállításjegyzék loginServer használja a következő példa hello, cserélje le a környezetből hello loginServer.</span><span class="sxs-lookup"><span data-stu-id="d9860-146">Using hello following example, replace hello container registry loginServer name with hello loginServer from your environment.</span></span>

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a><span data-ttu-id="d9860-147">Azure-tároló beállításjegyzék lemezképek felsorolása</span><span class="sxs-lookup"><span data-stu-id="d9860-147">List images in Azure Container Registry</span></span>

<span data-ttu-id="d9860-148">tooreturn tooyour Azure-tárolóba beállításjegyzék felhasználói hello értesítését lemezképeket listája [az acr tárház lista](/cli/azure/acr/repository#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9860-148">tooreturn a list of images that have been pushed tooyour Azure Container registry, user hello [az acr repository list](/cli/azure/acr/repository#list) command.</span></span> <span data-ttu-id="d9860-149">Hello parancs hello Tárolónév beállításjegyzék frissítése.</span><span class="sxs-lookup"><span data-stu-id="d9860-149">Update hello command with hello container registry name.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="d9860-150">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="d9860-150">Output:</span></span>

```azurecli
Result
----------------
aci-tutorial-app
```

<span data-ttu-id="d9860-151">Majd egy adott lemezkép toosee hello címkék hello [az acr tárház megjelenítése-címkék](/cli/azure/acr/repository#show-tags) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9860-151">And then toosee hello tags for a specific image, use hello [az acr repository show-tags](/cli/azure/acr/repository#show-tags) command.</span></span>

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

<span data-ttu-id="d9860-152">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="d9860-152">Output:</span></span>

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a><span data-ttu-id="d9860-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9860-153">Next steps</span></span>

<span data-ttu-id="d9860-154">Az oktatóanyag egy Azure-tároló beállításjegyzék Azure tároló példányok való használatra készült, és hello tároló kép leküldött volt.</span><span class="sxs-lookup"><span data-stu-id="d9860-154">In this tutorial, an Azure Container Registry was prepared for use with Azure Container Instances, and hello container image was pushed.</span></span> <span data-ttu-id="d9860-155">befejeződtek a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d9860-155">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d9860-156">Egy Azure-tároló beállításjegyzék-példány telepítése</span><span class="sxs-lookup"><span data-stu-id="d9860-156">Deploying an Azure Container Registry instance</span></span>
> * <span data-ttu-id="d9860-157">Azure-tároló beállításjegyzék címkézési tároló képe</span><span class="sxs-lookup"><span data-stu-id="d9860-157">Tagging container image for Azure Container Registry</span></span>
> * <span data-ttu-id="d9860-158">Kép tooAzure tároló beállításjegyzék feltöltése</span><span class="sxs-lookup"><span data-stu-id="d9860-158">Uploading image tooAzure Container Registry</span></span>

<span data-ttu-id="d9860-159">Előzetes toohello oktatóanyag következő toolearn hello tároló tooAzure Azure tároló példányok használatával telepítéséről.</span><span class="sxs-lookup"><span data-stu-id="d9860-159">Advance toohello next tutorial toolearn about deploying hello container tooAzure using Azure Container Instances.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d9860-160">Tárolók tooAzure tároló példányok telepítése</span><span class="sxs-lookup"><span data-stu-id="d9860-160">Deploy containers tooAzure Container Instances</span></span>](./container-instances-tutorial-deploy-app.md)
