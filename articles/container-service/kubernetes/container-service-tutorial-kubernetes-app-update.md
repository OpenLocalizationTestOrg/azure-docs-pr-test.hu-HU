---
title: "Azure Tárolószolgáltatás útmutató - alkalmazása |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató - alkalmazása"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: db580da3e2d70892bc37305394df5be609ebb8a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="update-an-application-in-kubernetes"></a><span data-ttu-id="1d7c8-104">Kubernetes tartozó alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="1d7c8-104">Update an application in Kubernetes</span></span>

<span data-ttu-id="1d7c8-105">Kubernetes az alkalmazás telepítése után azt egy új tároló kép vagy lemezkép verziója lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-105">After you deploy an application in Kubernetes, it can be updated by specifying a new container image or image version.</span></span> <span data-ttu-id="1d7c8-106">Ha egy alkalmazás módosítását, a frissítés bevezetése elő van készítve, hogy a központi telepítés egy részének egyidejűleg frissül.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-106">When you update an application, the update rollout is staged so that only a portion of the deployment is concurrently updated.</span></span> <span data-ttu-id="1d7c8-107">A két lépésben frissítés lehetővé teszi, hogy az alkalmazás tovább futnak a frissítés során, és visszaállítási mechanizmust biztosít, ha a központi telepítési hiba lép fel.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-107">This staged update enables the application to keep running during the update, and provides a rollback mechanism if a deployment failure occurs.</span></span> 

<span data-ttu-id="1d7c8-108">Ebben az oktatóanyagban hét, hat része a mintaalkalmazás Azure szavazattal frissül.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-108">In this tutorial, part six of seven, the sample Azure Vote app is updated.</span></span> <span data-ttu-id="1d7c8-109">Feladatokat, a következők:</span><span class="sxs-lookup"><span data-stu-id="1d7c8-109">Tasks that you complete include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1d7c8-110">Az előtér-alkalmazás kódja frissítése</span><span class="sxs-lookup"><span data-stu-id="1d7c8-110">Updating the front-end application code</span></span>
> * <span data-ttu-id="1d7c8-111">Frissített tároló lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d7c8-111">Creating an updated container image</span></span>
> * <span data-ttu-id="1d7c8-112">A tároló kép küldését Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="1d7c8-112">Pushing the container image to Azure Container Registry</span></span>
> * <span data-ttu-id="1d7c8-113">A frissített tároló rendszerkép központi telepítése</span><span class="sxs-lookup"><span data-stu-id="1d7c8-113">Deploying the updated container image</span></span>

<span data-ttu-id="1d7c8-114">A következő útmutatókból Operations Management Suite a Kubernetes fürt figyelésére van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-114">In subsequent tutorials, Operations Management Suite is configured to monitor the Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1d7c8-115">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="1d7c8-115">Before you begin</span></span>

<span data-ttu-id="1d7c8-116">Az előző oktatóanyagok egy tároló lemezképet, az Azure-tároló beállításjegyzék feltöltött lemezkép és a létrehozott Kubernetes fürt alkalmazás lett csomagolva.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-116">In previous tutorials, an application was packaged into a container image, the image uploaded to Azure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="1d7c8-117">Az alkalmazás ezután futtatták a Kubernetes fürtön.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-117">The application was then run on the Kubernetes cluster.</span></span> 

<span data-ttu-id="1d7c8-118">Ha még nem fejeződött be az alábbi lépéseket, és követéséhez, térjen vissza [oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="1d7c8-118">If you haven't completed these steps, and want to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

## <a name="update-application"></a><span data-ttu-id="1d7c8-119">Alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="1d7c8-119">Update application</span></span>

<span data-ttu-id="1d7c8-120">Az oktatóanyag lépéseinek végrehajtásához, egy példányát az Azure szavazattal alkalmazás rendelkezik klónozása.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-120">To complete the steps in this tutorial, you must have cloned a copy of the Azure Vote application.</span></span> <span data-ttu-id="1d7c8-121">Szükség esetén hozzon létre a klónozott másolása a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="1d7c8-121">If necessary, create this cloned copy with the following command:</span></span>

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="1d7c8-122">Nyissa meg a `config_file.cfg` bármely kód vagy szöveges szerkesztővel fájlt.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-122">Open the `config_file.cfg` file with any code or text editor.</span></span> <span data-ttu-id="1d7c8-123">Ez a fájl, a klónozott tárház a következő könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-123">You can find this file under the following directory of the cloned repo.</span></span>

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

<span data-ttu-id="1d7c8-124">Megváltoztatni az `VOTE1VALUE` és `VOTE2VALUE`, majd mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-124">Change the values for `VOTE1VALUE` and `VOTE2VALUE`, and then save the file.</span></span>

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

<span data-ttu-id="1d7c8-125">Használjon [docker compose](https://docs.docker.com/compose/) hozza létre az előtér-kép és a frissített alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-125">Use [docker-compose](https://docs.docker.com/compose/) to re-create the front-end image and run the updated application.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a><span data-ttu-id="1d7c8-126">Alkalmazás helyi teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="1d7c8-126">Test application locally</span></span>

<span data-ttu-id="1d7c8-127">Keresse meg a `http://localhost:8080` az frissített alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-127">Browse to `http://localhost:8080` to see the updated application.</span></span>

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a><span data-ttu-id="1d7c8-129">Címke és leküldéses lemezképek</span><span class="sxs-lookup"><span data-stu-id="1d7c8-129">Tag and push images</span></span>

<span data-ttu-id="1d7c8-130">Címke a *azure-szavazat-front* a tároló beállításjegyzék loginServer lemezkép.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-130">Tag the *azure-vote-front* image with the loginServer of the container registry.</span></span>

<span data-ttu-id="1d7c8-131">Azure-tároló beállításjegyzék használata, a bejelentkezési kiszolgálónevet beolvasása a [az acr lista](/cli/azure/acr#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-131">If you're using Azure Container Registry, get the login server name with the [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="1d7c8-132">Használjon [docker címke](https://docs.docker.com/engine/reference/commandline/tag/) használatával címkézhesse a lemezképet.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-132">Use [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) to tag the image.</span></span> <span data-ttu-id="1d7c8-133">Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló neve vagy a nyilvános beállításjegyzék állomásnév.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-133">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="1d7c8-134">Használjon [docker leküldéses](https://docs.docker.com/engine/reference/commandline/push/) való feltölti a lemezképet a beállításjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-134">Use [docker push](https://docs.docker.com/engine/reference/commandline/push/) to upload the image to your registry.</span></span> <span data-ttu-id="1d7c8-135">Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló neve vagy a nyilvános beállításjegyzék állomásnév.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-135">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a><span data-ttu-id="1d7c8-136">Frissítés alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="1d7c8-136">Deploy update application</span></span>

<span data-ttu-id="1d7c8-137">Maximális hasznos üzemidő biztosítása érdekében az alkalmazás fogyasztanak több példánya fut.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-137">To ensure maximum uptime, multiple instances of the application pod must be running.</span></span> <span data-ttu-id="1d7c8-138">Ez a konfiguráció ellenőrzése a [kubectl beolvasása pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-138">Verify this configuration with the [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```bash
kubectl get pod
```

<span data-ttu-id="1d7c8-139">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="1d7c8-139">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

<span data-ttu-id="1d7c8-140">Ha nem rendelkezik több három munkaállomás-csoporttal fut az azure-szavazat-első kép, méretezhető a *azure-szavazat-front* központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-140">If you don't have multiple pods running the azure-vote-front image, scale the *azure-vote-front* deployment.</span></span>


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

<span data-ttu-id="1d7c8-141">Az alkalmazás frissítésére, használja a [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-141">To update the application, use the [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) command.</span></span> <span data-ttu-id="1d7c8-142">Frissítés `<acrLoginServer>` a tároló beállításjegyzék bejelentkezési kiszolgáló vagy a gazdagép nevével.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-142">Update `<acrLoginServer>` with the login server or host name of your container registry.</span></span>

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="1d7c8-143">Az üzemelő példány figyelése a [kubectl beolvasása pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-143">To monitor the deployment, use the [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span> <span data-ttu-id="1d7c8-144">A frissített alkalmazás lett telepítve, mint a három munkaállomás-csoporttal lezárva, és újból létrehozza az új tároló lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-144">As the updated application is deployed, your pods are terminated and re-created with the new container image.</span></span>

```azurecli-interactive
kubectl get pod
```

<span data-ttu-id="1d7c8-145">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="1d7c8-145">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a><span data-ttu-id="1d7c8-146">Frissített alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="1d7c8-146">Test updated application</span></span>

<span data-ttu-id="1d7c8-147">A külső IP-címet az beszerzése a *azure-szavazat-front* szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-147">Get the external IP address of the *azure-vote-front* service.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front
```

<span data-ttu-id="1d7c8-148">Tallózással keresse meg az IP-címet a frissített alkalmazás megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-148">Browse to the IP address to see the updated application.</span></span>

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a><span data-ttu-id="1d7c8-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d7c8-150">Next steps</span></span>

<span data-ttu-id="1d7c8-151">Ebben az oktatóanyagban egy alkalmazás frissítése, és a frissítés Kubernetes fürtre megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-151">In this tutorial, you updated an application and rolled out this update to a Kubernetes cluster.</span></span> <span data-ttu-id="1d7c8-152">Befejeződtek a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="1d7c8-152">The following tasks were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1d7c8-153">Frissítve az előtér-alkalmazás kódja</span><span class="sxs-lookup"><span data-stu-id="1d7c8-153">Updated the front-end application code</span></span>
> * <span data-ttu-id="1d7c8-154">Frissített tároló lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d7c8-154">Created an updated container image</span></span>
> * <span data-ttu-id="1d7c8-155">A tároló kép leküldött Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="1d7c8-155">Pushed the container image to Azure Container Registry</span></span>
> * <span data-ttu-id="1d7c8-156">A frissített alkalmazás telepítve</span><span class="sxs-lookup"><span data-stu-id="1d7c8-156">Deployed the updated application</span></span>

<span data-ttu-id="1d7c8-157">Továbblépés figyelése az Operations Management Suite Kubernetes olvashat a következő oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="1d7c8-157">Advance to the next tutorial to learn about how to monitor Kubernetes with Operations Management Suite.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1d7c8-158">A Kubernetes monitorozása az OMS használatával</span><span class="sxs-lookup"><span data-stu-id="1d7c8-158">Monitor Kubernetes with OMS</span></span>](./container-service-tutorial-kubernetes-monitor.md)