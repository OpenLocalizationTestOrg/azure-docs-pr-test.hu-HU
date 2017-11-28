---
title: "aaaAzure Tárolószolgáltatás oktatóanyag – alkalmazása |} Microsoft Docs"
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
ms.openlocfilehash: c467498bab7952926a18e45ffbb21051a98739d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-an-application-in-kubernetes"></a><span data-ttu-id="26a21-104">Kubernetes tartozó alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="26a21-104">Update an application in Kubernetes</span></span>

<span data-ttu-id="26a21-105">Kubernetes az alkalmazás telepítése után azt egy új tároló kép vagy lemezkép verziója lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="26a21-105">After you deploy an application in Kubernetes, it can be updated by specifying a new container image or image version.</span></span> <span data-ttu-id="26a21-106">Ha egy alkalmazás módosítását, hello frissítés bevezetése elő van készítve, hogy csak egy részét hello telepítési egyidejűleg frissül.</span><span class="sxs-lookup"><span data-stu-id="26a21-106">When you update an application, hello update rollout is staged so that only a portion of hello deployment is concurrently updated.</span></span> <span data-ttu-id="26a21-107">A előkészített frissítés lehetővé teszi, hogy a hello alkalmazás tookeep hello frissítés alatt fut, és visszaállítási mechanizmust biztosít, ha a központi telepítési hiba lép fel.</span><span class="sxs-lookup"><span data-stu-id="26a21-107">This staged update enables hello application tookeep running during hello update, and provides a rollback mechanism if a deployment failure occurs.</span></span> 

<span data-ttu-id="26a21-108">Ebben az oktatóanyagban hét, hello Azure szavazattal mintaalkalmazás hat részét frissül.</span><span class="sxs-lookup"><span data-stu-id="26a21-108">In this tutorial, part six of seven, hello sample Azure Vote app is updated.</span></span> <span data-ttu-id="26a21-109">Feladatokat, a következők:</span><span class="sxs-lookup"><span data-stu-id="26a21-109">Tasks that you complete include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="26a21-110">Hello előtér-alkalmazás kódja frissítése</span><span class="sxs-lookup"><span data-stu-id="26a21-110">Updating hello front-end application code</span></span>
> * <span data-ttu-id="26a21-111">Frissített tároló lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="26a21-111">Creating an updated container image</span></span>
> * <span data-ttu-id="26a21-112">Hello tároló kép tooAzure tároló beállításjegyzék küldését</span><span class="sxs-lookup"><span data-stu-id="26a21-112">Pushing hello container image tooAzure Container Registry</span></span>
> * <span data-ttu-id="26a21-113">Hello frissített tároló rendszerkép központi telepítése</span><span class="sxs-lookup"><span data-stu-id="26a21-113">Deploying hello updated container image</span></span>

<span data-ttu-id="26a21-114">A következő útmutatókból Operations Management Suite konfigurált toomonitor hello Kubernetes fürtön.</span><span class="sxs-lookup"><span data-stu-id="26a21-114">In subsequent tutorials, Operations Management Suite is configured toomonitor hello Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="26a21-115">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="26a21-115">Before you begin</span></span>

<span data-ttu-id="26a21-116">Az előző oktatóanyagokat egy alkalmazás egy tároló lemezképpel csomagolása, hello lemezkép feltöltése tooAzure tároló beállításjegyzék és a létrehozott Kubernetes fürt.</span><span class="sxs-lookup"><span data-stu-id="26a21-116">In previous tutorials, an application was packaged into a container image, hello image uploaded tooAzure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="26a21-117">hello alkalmazás majd hello Kubernetes fürt volt futtatva.</span><span class="sxs-lookup"><span data-stu-id="26a21-117">hello application was then run on hello Kubernetes cluster.</span></span> 

<span data-ttu-id="26a21-118">Ha még nem fejeződött be az alábbi lépéseket, és szeretné, hogy mentén toofollow, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="26a21-118">If you haven't completed these steps, and want toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

## <a name="update-application"></a><span data-ttu-id="26a21-119">Alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="26a21-119">Update application</span></span>

<span data-ttu-id="26a21-120">Ebben az oktatóanyagban toocomplete hello lépéseiért kell rendelkeznie klónozott hello Azure szavazattal alkalmazás másolatát.</span><span class="sxs-lookup"><span data-stu-id="26a21-120">toocomplete hello steps in this tutorial, you must have cloned a copy of hello Azure Vote application.</span></span> <span data-ttu-id="26a21-121">Szükség esetén hozzon létre a klónozott példány hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="26a21-121">If necessary, create this cloned copy with hello following command:</span></span>

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

<span data-ttu-id="26a21-122">Nyissa meg hello `config_file.cfg` bármely kód vagy szöveges szerkesztővel fájlt.</span><span class="sxs-lookup"><span data-stu-id="26a21-122">Open hello `config_file.cfg` file with any code or text editor.</span></span> <span data-ttu-id="26a21-123">Ez a fájl a következő hello klónozott tárház könyvtárában hello található.</span><span class="sxs-lookup"><span data-stu-id="26a21-123">You can find this file under hello following directory of hello cloned repo.</span></span>

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

<span data-ttu-id="26a21-124">Hello értékeinek módosítása `VOTE1VALUE` és `VOTE2VALUE`, majd mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="26a21-124">Change hello values for `VOTE1VALUE` and `VOTE2VALUE`, and then save hello file.</span></span>

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

<span data-ttu-id="26a21-125">Használjon [docker compose](https://docs.docker.com/compose/) toore-hello előtér-kép és a frissített futtatási hello alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="26a21-125">Use [docker-compose](https://docs.docker.com/compose/) toore-create hello front-end image and run hello updated application.</span></span>

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a><span data-ttu-id="26a21-126">Alkalmazás helyi teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="26a21-126">Test application locally</span></span>

<span data-ttu-id="26a21-127">Keresse meg a túl`http://localhost:8080` toosee hello frissíteni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="26a21-127">Browse too`http://localhost:8080` toosee hello updated application.</span></span>

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a><span data-ttu-id="26a21-129">Címke és leküldéses lemezképek</span><span class="sxs-lookup"><span data-stu-id="26a21-129">Tag and push images</span></span>

<span data-ttu-id="26a21-130">Címke hello *azure-szavazat-front* hello loginServer hello tároló beállításjegyzék lemezkép.</span><span class="sxs-lookup"><span data-stu-id="26a21-130">Tag hello *azure-vote-front* image with hello loginServer of hello container registry.</span></span>

<span data-ttu-id="26a21-131">Azure-tároló beállításjegyzék használata, get hello bejelentkezési kiszolgálónevet hello [az acr lista](/cli/azure/acr#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="26a21-131">If you're using Azure Container Registry, get hello login server name with hello [az acr list](/cli/azure/acr#list) command.</span></span>

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

<span data-ttu-id="26a21-132">Használjon [docker címke](https://docs.docker.com/engine/reference/commandline/tag/) tootag hello kép.</span><span class="sxs-lookup"><span data-stu-id="26a21-132">Use [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) tootag hello image.</span></span> <span data-ttu-id="26a21-133">Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló neve vagy a nyilvános beállításjegyzék állomásnév.</span><span class="sxs-lookup"><span data-stu-id="26a21-133">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="26a21-134">Használjon [docker leküldéses](https://docs.docker.com/engine/reference/commandline/push/) tooupload hello kép tooyour beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="26a21-134">Use [docker push](https://docs.docker.com/engine/reference/commandline/push/) tooupload hello image tooyour registry.</span></span> <span data-ttu-id="26a21-135">Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló neve vagy a nyilvános beállításjegyzék állomásnév.</span><span class="sxs-lookup"><span data-stu-id="26a21-135">Replace `<acrLoginServer>` with your Azure Container Registry login server name or public registry hostname.</span></span>

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a><span data-ttu-id="26a21-136">Frissítés alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="26a21-136">Deploy update application</span></span>

<span data-ttu-id="26a21-137">tooensure maximális hasznos üzemidő hello alkalmazás pod több példánya fut.</span><span class="sxs-lookup"><span data-stu-id="26a21-137">tooensure maximum uptime, multiple instances of hello application pod must be running.</span></span> <span data-ttu-id="26a21-138">Ellenőrizze a konfiguráció hello [kubectl beolvasása pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancs.</span><span class="sxs-lookup"><span data-stu-id="26a21-138">Verify this configuration with hello [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```bash
kubectl get pod
```

<span data-ttu-id="26a21-139">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="26a21-139">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

<span data-ttu-id="26a21-140">Ha nem rendelkezik több három munkaállomás-csoporttal hello azure-szavazat-első kép fut, méretezhető hello *azure-szavazat-front* központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="26a21-140">If you don't have multiple pods running hello azure-vote-front image, scale hello *azure-vote-front* deployment.</span></span>


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

<span data-ttu-id="26a21-141">tooupdate hello alkalmazás használatát hello [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="26a21-141">tooupdate hello application, use hello [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) command.</span></span> <span data-ttu-id="26a21-142">Frissítés `<acrLoginServer>` hello bejelentkezési névvel kiszolgáló vagy a gazdagép a tároló beállításjegyzékről.</span><span class="sxs-lookup"><span data-stu-id="26a21-142">Update `<acrLoginServer>` with hello login server or host name of your container registry.</span></span>

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

<span data-ttu-id="26a21-143">toomonitor hello központi telepítését, használatát hello [kubectl beolvasása pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.</span><span class="sxs-lookup"><span data-stu-id="26a21-143">toomonitor hello deployment, use hello [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span> <span data-ttu-id="26a21-144">Frissített hello alkalmazás lett telepítve, mint a három munkaállomás-csoporttal lezárva, és újból létrehozza hello új tároló lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="26a21-144">As hello updated application is deployed, your pods are terminated and re-created with hello new container image.</span></span>

```azurecli-interactive
kubectl get pod
```

<span data-ttu-id="26a21-145">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="26a21-145">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a><span data-ttu-id="26a21-146">Frissített alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="26a21-146">Test updated application</span></span>

<span data-ttu-id="26a21-147">Hello külső IP-cím hello az beszerzése *azure-szavazat-front* szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="26a21-147">Get hello external IP address of hello *azure-vote-front* service.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front
```

<span data-ttu-id="26a21-148">Tallózás toohello IP cím toosee hello alkalmazás frissítése.</span><span class="sxs-lookup"><span data-stu-id="26a21-148">Browse toohello IP address toosee hello updated application.</span></span>

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a><span data-ttu-id="26a21-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26a21-150">Next steps</span></span>

<span data-ttu-id="26a21-151">Ebben az oktatóanyagban egy alkalmazás frissítése, és ez tooa Kubernetes fürt frissítése megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="26a21-151">In this tutorial, you updated an application and rolled out this update tooa Kubernetes cluster.</span></span> <span data-ttu-id="26a21-152">befejeződtek a hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="26a21-152">hello following tasks were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="26a21-153">Frissített hello előtér-alkalmazás kódja</span><span class="sxs-lookup"><span data-stu-id="26a21-153">Updated hello front-end application code</span></span>
> * <span data-ttu-id="26a21-154">Frissített tároló lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="26a21-154">Created an updated container image</span></span>
> * <span data-ttu-id="26a21-155">Hello tároló kép tooAzure tároló beállításjegyzék leküldött</span><span class="sxs-lookup"><span data-stu-id="26a21-155">Pushed hello container image tooAzure Container Registry</span></span>
> * <span data-ttu-id="26a21-156">Frissített telepített hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="26a21-156">Deployed hello updated application</span></span>

<span data-ttu-id="26a21-157">Előzetes toohello oktatóanyag következő toolearn kapcsolatos toomonitor az Operations Management Suite Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="26a21-157">Advance toohello next tutorial toolearn about how toomonitor Kubernetes with Operations Management Suite.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="26a21-158">A Kubernetes monitorozása az OMS használatával</span><span class="sxs-lookup"><span data-stu-id="26a21-158">Monitor Kubernetes with OMS</span></span>](./container-service-tutorial-kubernetes-monitor.md)