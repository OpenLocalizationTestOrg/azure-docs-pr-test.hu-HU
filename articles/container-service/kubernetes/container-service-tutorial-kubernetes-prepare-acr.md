---
title: "aaaAzure Tárolószolgáltatás oktatóanyag – ACR előkészítése |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató - ACR előkészítése"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3980e5ce4eb9836f83c761a2f76c944bb3f13060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a><span data-ttu-id="244f1-104">Üzembe helyezés és használat Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="244f1-104">Deploy and use Azure Container Registry</span></span>

<span data-ttu-id="244f1-105">Az Azure tároló beállításjegyzék (ACR) egy Azure-alapú, személyes beállításjegyzék Docker-tároló lemezképek.</span><span class="sxs-lookup"><span data-stu-id="244f1-105">Azure Container Registry (ACR) is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="244f1-106">Ez az oktatóanyag, része két hét, végigvezeti Azure tároló beállításjegyzék-példány telepítését, valamint egy tároló kép tooit kérdez le.</span><span class="sxs-lookup"><span data-stu-id="244f1-106">This tutorial, part two of seven, walks through deploying an Azure Container Registry instance, and pushing a container image tooit.</span></span> <span data-ttu-id="244f1-107">Befejeződött a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="244f1-107">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="244f1-108">Egy Azure tároló beállításjegyzék (ACR) példány telepítése</span><span class="sxs-lookup"><span data-stu-id="244f1-108">Deploying an Azure Container Registry (ACR) instance</span></span>
> * <span data-ttu-id="244f1-109">A tároló lemezkép ACR a címkézés</span><span class="sxs-lookup"><span data-stu-id="244f1-109">Tagging a container image for ACR</span></span>
> * <span data-ttu-id="244f1-110">Hello kép tooACR feltöltése</span><span class="sxs-lookup"><span data-stu-id="244f1-110">Uploading hello image tooACR</span></span>

<span data-ttu-id="244f1-111">A következő útmutatókból adott ACR példány integrálva van egy Azure-tároló szolgáltatás Kubernetes fürthöz történő biztonságos futtatására, tároló-lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="244f1-111">In subsequent tutorials, this ACR instance is integrated with an Azure Container Service Kubernetes cluster, for securely running container images.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="244f1-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="244f1-112">Before you begin</span></span>

<span data-ttu-id="244f1-113">A hello [az oktatóanyag előző](./container-service-tutorial-kubernetes-prepare-app.md), a tároló-lemezkép létrejött egy egyszerű Azure szavazás alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="244f1-113">In hello [previous tutorial](./container-service-tutorial-kubernetes-prepare-app.md), a container image was created for a simple Azure Voting application.</span></span> <span data-ttu-id="244f1-114">Ebben az oktatóanyagban a kép tooan Azure tároló beállításjegyzék fejlesztőre.</span><span class="sxs-lookup"><span data-stu-id="244f1-114">In this tutorial, this image is pushed tooan Azure Container Registry.</span></span> <span data-ttu-id="244f1-115">Ha nem hozott létre hello Azure szavazás alkalmazás lemezképét, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="244f1-115">If you have not created hello Azure Voting app image, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> <span data-ttu-id="244f1-116">Alternatív megoldásként hello lépések részletes itt együttműködnek a tároló képet.</span><span class="sxs-lookup"><span data-stu-id="244f1-116">Alternatively, hello steps detailed here work with any container image.</span></span>

<span data-ttu-id="244f1-117">Ez az oktatóanyag megköveteli, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="244f1-117">This tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="244f1-118">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="244f1-118">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="244f1-119">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="244f1-119">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="244f1-120">Telepítse az Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="244f1-120">Deploy Azure Container Registry</span></span>

<span data-ttu-id="244f1-121">Egy Azure-tároló beállításjegyzék való telepítésekor, először egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="244f1-121">When deploying an Azure Container Registry, you first need a resource group.</span></span> <span data-ttu-id="244f1-122">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="244f1-122">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="244f1-123">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="244f1-123">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="244f1-124">Ebben a példában az erőforráscsoport neve *myResourceGroup* jön létre az hello *westeurope* régióban.</span><span class="sxs-lookup"><span data-stu-id="244f1-124">In this example, a resource group named *myResourceGroup* is created in hello *westeurope* region.</span></span>

```azurecli
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="244f1-125">Hozzon létre egy Azure-tárolóba beállításjegyzék hello [az acr létrehozása](/cli/azure/acr#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="244f1-125">Create an Azure Container registry with hello [az acr create](/cli/azure/acr#create) command.</span></span> <span data-ttu-id="244f1-126">egy tároló beállításjegyzék hello neve **egyedinek kell lennie**.</span><span class="sxs-lookup"><span data-stu-id="244f1-126">hello name of a Container Registry **must be unique**.</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

<span data-ttu-id="244f1-127">Ez az oktatóanyag hello részeinek, egész használjuk "acrname" helyőrzőként a hello beállításjegyzék Tárolónév választott.</span><span class="sxs-lookup"><span data-stu-id="244f1-127">Throughout hello rest of this tutorial, we use "acrname" as a placeholder for hello container registry name that you chose.</span></span>

## <a name="container-registry-login"></a><span data-ttu-id="244f1-128">Tároló beállításjegyzék bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="244f1-128">Container registry login</span></span>

<span data-ttu-id="244f1-129">Be kell jelentkeznie tooyour ACR példány képek tooit előtt.</span><span class="sxs-lookup"><span data-stu-id="244f1-129">You must log in tooyour ACR instance before pushing images tooit.</span></span> <span data-ttu-id="244f1-130">Használjon hello [az acr bejelentkezési](https://docs.microsoft.com/en-us/cli/azure/acr#login) toocomplete hello művelet parancsot.</span><span class="sxs-lookup"><span data-stu-id="244f1-130">Use hello [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login) command toocomplete hello operation.</span></span> <span data-ttu-id="244f1-131">Tooprovide hello egyedi adott név toohello tároló beállításjegyzék létrehozásakor van szüksége.</span><span class="sxs-lookup"><span data-stu-id="244f1-131">You need tooprovide hello unique name given toohello container registry when it was created.</span></span>

```azurecli
az acr login --name <acrName>
```

<span data-ttu-id="244f1-132">hello parancs visszaadja a "Sikeres bejelentkezés" üzenet, amint befejeződött.</span><span class="sxs-lookup"><span data-stu-id="244f1-132">hello command returns a 'Login Succeeded’ message once completed.</span></span>

## <a name="tag-container-images"></a><span data-ttu-id="244f1-133">Címke tároló lemezképek</span><span class="sxs-lookup"><span data-stu-id="244f1-133">Tag container images</span></span>

<span data-ttu-id="244f1-134">Minden egyes tároló kép hello beállításjegyzék hello loginServer nevű címkézett toobe kell.</span><span class="sxs-lookup"><span data-stu-id="244f1-134">Each container image needs toobe tagged with hello loginServer name of hello registry.</span></span> <span data-ttu-id="244f1-135">Ezt a címkét használható útválasztási Ha tároló képek tooan kép beállításjegyzék végez leküldést.</span><span class="sxs-lookup"><span data-stu-id="244f1-135">This tag is used for routing when pushing container images tooan image registry.</span></span>

<span data-ttu-id="244f1-136">az aktuális képek, használjon hello listáját toosee [docker képek](https://docs.docker.com/engine/reference/commandline/images/) parancs.</span><span class="sxs-lookup"><span data-stu-id="244f1-136">toosee a list of current images, use hello [docker images](https://docs.docker.com/engine/reference/commandline/images/) command.</span></span>

```bash
docker images
```

<span data-ttu-id="244f1-137">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="244f1-137">Output:</span></span>

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

<span data-ttu-id="244f1-138">tooget hello loginServer neve, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="244f1-138">tooget hello loginServer name, run hello following command.</span></span>

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

<span data-ttu-id="244f1-139">Most, címke hello *azure-szavazat-front* hello loginServer hello tároló beállításjegyzék lemezkép.</span><span class="sxs-lookup"><span data-stu-id="244f1-139">Now, tag hello *azure-vote-front* image with hello loginServer of hello container registry.</span></span> <span data-ttu-id="244f1-140">Továbbá adja hozzá `:redis-v1` hello lemezképnév toohello végét.</span><span class="sxs-lookup"><span data-stu-id="244f1-140">Also, add `:redis-v1` toohello end of hello image name.</span></span> <span data-ttu-id="244f1-141">Ezt a címkét hello lemezkép verziója jelzi.</span><span class="sxs-lookup"><span data-stu-id="244f1-141">This tag indicates hello image version.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

<span data-ttu-id="244f1-142">Miután címkézett, futtassa az [docker képek] (https://docs.docker.com/engine/reference/commandline/images/) tooverify hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="244f1-142">Once tagged, run [docker images] (https://docs.docker.com/engine/reference/commandline/images/) tooverify hello operation.</span></span>

```bash
docker images
```

<span data-ttu-id="244f1-143">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="244f1-143">Output:</span></span>

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-tooregistry"></a><span data-ttu-id="244f1-144">Leküldéses képek tooregistry</span><span class="sxs-lookup"><span data-stu-id="244f1-144">Push images tooregistry</span></span>

<span data-ttu-id="244f1-145">Hello leküldéses *azure-szavazat-front* kép toohello beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="244f1-145">Push hello *azure-vote-front* image toohello registry.</span></span> 

<span data-ttu-id="244f1-146">Hello ACR loginServer neve használja a következő példa hello, cserélje le a környezetből hello loginServer.</span><span class="sxs-lookup"><span data-stu-id="244f1-146">Using hello following example, replace hello ACR loginServer name with hello loginServer from your environment.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

<span data-ttu-id="244f1-147">Néhány perc toocomplete jut.</span><span class="sxs-lookup"><span data-stu-id="244f1-147">This takes a couple of minutes toocomplete.</span></span>

## <a name="list-images-in-registry"></a><span data-ttu-id="244f1-148">A beállításjegyzékben lemezképek felsorolása</span><span class="sxs-lookup"><span data-stu-id="244f1-148">List images in registry</span></span>

<span data-ttu-id="244f1-149">tooreturn tooyour Azure-tárolóba beállításjegyzék felhasználói hello értesítését lemezképeket listája [az acr tárház lista](/cli/azure/acr/repository#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="244f1-149">tooreturn a list of images that have been pushed tooyour Azure Container registry, user hello [az acr repository list](/cli/azure/acr/repository#list) command.</span></span> <span data-ttu-id="244f1-150">Frissítse a hello parancs hello ACR neve.</span><span class="sxs-lookup"><span data-stu-id="244f1-150">Update hello command with hello ACR instance name.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="244f1-151">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="244f1-151">Output:</span></span>

```azurecli
Result
----------------
azure-vote-front
```

<span data-ttu-id="244f1-152">Majd egy adott lemezkép toosee hello címkék hello [az acr tárház megjelenítése-címkék](/cli/azure/acr/repository#show-tags) parancsot.</span><span class="sxs-lookup"><span data-stu-id="244f1-152">And then toosee hello tags for a specific image, use hello [az acr repository show-tags](/cli/azure/acr/repository#show-tags) command.</span></span>

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

<span data-ttu-id="244f1-153">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="244f1-153">Output:</span></span>

```azurecli
Result
--------
redis-v1
```

<span data-ttu-id="244f1-154">Oktatóanyag befejezése után hello tároló kép rendelkezik lett tárolja, a saját Azure tároló beállításjegyzék-példányban.</span><span class="sxs-lookup"><span data-stu-id="244f1-154">At tutorial completion, hello container image has been stored in a private Azure Container Registry instance.</span></span> <span data-ttu-id="244f1-155">Ez a kép a következő útmutatókból ACR tooa Kubernetes fürtről van telepítve.</span><span class="sxs-lookup"><span data-stu-id="244f1-155">This image is deployed from ACR tooa Kubernetes cluster in subsequent tutorials.</span></span>

## <a name="next-steps"></a><span data-ttu-id="244f1-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="244f1-156">Next steps</span></span>

<span data-ttu-id="244f1-157">Ebben az oktatóanyagban az Azure-tároló beállításjegyzék ACS Kubernetes fürt használatra lett előkészítve.</span><span class="sxs-lookup"><span data-stu-id="244f1-157">In this tutorial, an Azure Container Registry was prepared for use in an ACS Kubernetes cluster.</span></span> <span data-ttu-id="244f1-158">befejeződtek a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="244f1-158">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="244f1-159">Egy Azure-tároló beállításjegyzék-példány telepítése</span><span class="sxs-lookup"><span data-stu-id="244f1-159">Deployed an Azure Container Registry instance</span></span>
> * <span data-ttu-id="244f1-160">A tároló-lemezkép az ACR címkézett</span><span class="sxs-lookup"><span data-stu-id="244f1-160">Tagged a container image for ACR</span></span>
> * <span data-ttu-id="244f1-161">Feltöltött hello kép tooACR</span><span class="sxs-lookup"><span data-stu-id="244f1-161">Uploaded hello image tooACR</span></span>

<span data-ttu-id="244f1-162">Az Azure-ban Kubernetes fürt telepítésével kapcsolatos útmutató toolearn következő toohello előzetes.</span><span class="sxs-lookup"><span data-stu-id="244f1-162">Advance toohello next tutorial toolearn about deploying a Kubernetes cluster in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="244f1-163">Kubernetes fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="244f1-163">Deploy Kubernetes cluster</span></span>](./container-service-tutorial-kubernetes-deploy-cluster.md)