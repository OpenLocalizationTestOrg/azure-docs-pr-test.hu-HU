---
title: "aaaAzure Tárolószolgáltatás oktatóanyag – Scale Application |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató – Scale Application"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, Micro-szolgáltatások, Kubernetes és az Azure-"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 29571eef0fd91bd6b40f00d8c018539f320179bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a><span data-ttu-id="b6f27-104">Skála Kubernetes három munkaállomás-csoporttal és Kubernetes infrastruktúra</span><span class="sxs-lookup"><span data-stu-id="b6f27-104">Scale Kubernetes pods and Kubernetes infrastructure</span></span>

<span data-ttu-id="b6f27-105">Ha korábban már következő hello oktatóprogramok, működő Kubernetes fürt rendelkezik az Azure Tárolószolgáltatásban, és hello Azure szavazás alkalmazást telepítette.</span><span class="sxs-lookup"><span data-stu-id="b6f27-105">If you've been following hello tutorials, you have a working Kubernetes cluster in Azure Container Service and you deployed hello Azure Voting app.</span></span> 

<span data-ttu-id="b6f27-106">Ebben az oktatóanyagban öt részét hét, horizontális felskálázás hello három munkaállomás-csoporttal hello alkalmazásban, majd próbálja pod automatikus skálázást.</span><span class="sxs-lookup"><span data-stu-id="b6f27-106">In this tutorial, part five of seven, you scale out hello pods in hello app and try pod autoscaling.</span></span> <span data-ttu-id="b6f27-107">Azt is megtudhatja, hogyan tooscale hello számú Azure virtuális gép ügynök csomópontok toochange hello fürt kapacitása munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="b6f27-107">You also learn how tooscale hello number of Azure VM agent nodes toochange hello cluster's capacity for hosting workloads.</span></span> <span data-ttu-id="b6f27-108">Befejeződött a következő feladatokból áll:</span><span class="sxs-lookup"><span data-stu-id="b6f27-108">Tasks completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b6f27-109">Manuális skálázás Kubernetes három munkaállomás-csoporttal</span><span class="sxs-lookup"><span data-stu-id="b6f27-109">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="b6f27-110">Automatikus skálázás három munkaállomás-csoporttal hello alkalmazás előtér futtató konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b6f27-110">Configuring Autoscale pods running hello app front end</span></span>
> * <span data-ttu-id="b6f27-111">Hello Kubernetes Azure ügynök csomópontok méretezése</span><span class="sxs-lookup"><span data-stu-id="b6f27-111">Scale hello Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="b6f27-112">A következő útmutatókból hello Azure szavazattal alkalmazás frissül, és az Operations Management Suite konfigurált toomonitor hello Kubernetes fürt.</span><span class="sxs-lookup"><span data-stu-id="b6f27-112">In subsequent tutorials, hello Azure Vote application is updated, and Operations Management Suite configured toomonitor hello Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b6f27-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b6f27-113">Before you begin</span></span>

<span data-ttu-id="b6f27-114">Az előző oktatóanyagokat egy alkalmazás egy tároló lemezképpel csomagolása, ez a Rendszerkép feltöltése tooAzure tároló beállításjegyzék és a létrehozott Kubernetes fürt.</span><span class="sxs-lookup"><span data-stu-id="b6f27-114">In previous tutorials, an application was packaged into a container image, this image uploaded tooAzure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="b6f27-115">hello alkalmazás majd hello Kubernetes fürt volt futtatva.</span><span class="sxs-lookup"><span data-stu-id="b6f27-115">hello application was then run on hello Kubernetes cluster.</span></span> <span data-ttu-id="b6f27-116">Ha nem volna ezeket a lépéseket, és szeretné mentén toofollow, térjen vissza a toohello [oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="b6f27-116">If you have not done these steps, and would like toofollow along, return toohello [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="b6f27-117">Legalább az oktatóanyag a Kubernetes fürtben futó alkalmazás van szükség.</span><span class="sxs-lookup"><span data-stu-id="b6f27-117">At minimum, this tutorial requires a Kubernetes cluster with a running application.</span></span>

## <a name="manually-scale-pods"></a><span data-ttu-id="b6f27-118">Manuálisan méretezhető, három munkaállomás-csoporttal</span><span class="sxs-lookup"><span data-stu-id="b6f27-118">Manually scale pods</span></span>

<span data-ttu-id="b6f27-119">Így sokkal hello Azure szavazattal előtér- és Redis példánya van telepítve, az egy replikához.</span><span class="sxs-lookup"><span data-stu-id="b6f27-119">Thus far, hello Azure Vote front-end and Redis instance have been deployed, each with a single replica.</span></span> <span data-ttu-id="b6f27-120">tooverify, futtassa a hello [kubectl beolvasása](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6f27-120">tooverify, run hello [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="b6f27-121">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b6f27-121">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

<span data-ttu-id="b6f27-122">Manuálisan módosítja a hello három munkaállomás-csoporttal hello száma `azure-vote-front` állításra hello [kubectl méretezési](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6f27-122">Manually change hello number of pods in hello `azure-vote-front` deployment using hello [kubectl scale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) command.</span></span> <span data-ttu-id="b6f27-123">Ez a példa hello számú too5 növeli.</span><span class="sxs-lookup"><span data-stu-id="b6f27-123">This example increases hello number too5.</span></span>

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

<span data-ttu-id="b6f27-124">Futtatás [kubectl első három munkaállomás-csoporttal](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) tooverify, hogy Kubernetes hoz létre a hello három munkaállomás-csoporttal.</span><span class="sxs-lookup"><span data-stu-id="b6f27-124">Run [kubectl get pods](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) tooverify that Kubernetes is creating hello pods.</span></span> <span data-ttu-id="b6f27-125">Egy perc vagy tette, után hello további három munkaállomás-csoporttal használja:</span><span class="sxs-lookup"><span data-stu-id="b6f27-125">After a minute or so, hello additional pods are running:</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="b6f27-126">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b6f27-126">Output:</span></span>

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a><span data-ttu-id="b6f27-127">Automatikus skálázás három munkaállomás-csoporttal</span><span class="sxs-lookup"><span data-stu-id="b6f27-127">Autoscale pods</span></span>

<span data-ttu-id="b6f27-128">Támogatja a Kubernetes [vízszintes pod automatikus skálázás](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) tooadjust hello száma attól függően, hogy a CPU-felhasználás, vagy más központi telepítés három munkaállomás-csoporttal kiválaszthatja a metrikákat.</span><span class="sxs-lookup"><span data-stu-id="b6f27-128">Kubernetes supports [horizontal pod autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) tooadjust hello number of pods in a deployment depending on CPU utilization or other select metrics.</span></span> 

<span data-ttu-id="b6f27-129">toouse hello autoscaler, a három munkaállomás-csoporttal kell rendelkeznie, CPU-kérelmek és meghatározott korlátozásokat.</span><span class="sxs-lookup"><span data-stu-id="b6f27-129">toouse hello autoscaler, your pods must have CPU requests and limits defined.</span></span> <span data-ttu-id="b6f27-130">A hello `azure-vote-front` központi telepítését, előtér-tároló kérelmek 0,25 Processzor 0,5 legfeljebb hello Processzor.</span><span class="sxs-lookup"><span data-stu-id="b6f27-130">In hello `azure-vote-front` deployment, hello front-end container requests 0.25 CPU, with a limit of 0.5 CPU.</span></span> <span data-ttu-id="b6f27-131">hello beállításokat a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="b6f27-131">hello settings look like:</span></span>

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

<span data-ttu-id="b6f27-132">hello alábbi példában hello [kubectl automatikus skálázás](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) tooautoscale hello száma a hello három munkaállomás-csoporttal parancs `azure-vote-front` központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="b6f27-132">hello following example uses hello [kubectl autoscale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) command tooautoscale hello number of pods in hello `azure-vote-front` deployment.</span></span> <span data-ttu-id="b6f27-133">Itt Ha a CPU-felhasználás meghaladja az 50 %-os, hello autoscaler hello három munkaállomás-csoporttal tooa legfeljebb 10 növeli.</span><span class="sxs-lookup"><span data-stu-id="b6f27-133">Here, if CPU utilization exceeds 50%, hello autoscaler increases hello pods tooa maximum of 10.</span></span>


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

<span data-ttu-id="b6f27-134">hello autoscaler, futtassa a következő parancs hello toosee hello állapota:</span><span class="sxs-lookup"><span data-stu-id="b6f27-134">toosee hello status of hello autoscaler, run hello following command:</span></span>

```azurecli-interactive
kubectl get hpa
```

<span data-ttu-id="b6f27-135">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b6f27-135">Output:</span></span>

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

<span data-ttu-id="b6f27-136">Néhány perc elteltével hello Azure szavazattal alkalmazást, és minimális terhelése hello pod replikák száma csökken automatikusan too3.</span><span class="sxs-lookup"><span data-stu-id="b6f27-136">After a few minutes, with minimal load on hello Azure Vote app, hello number of pod replicas decreases automatically too3.</span></span>

## <a name="scale-hello-agents"></a><span data-ttu-id="b6f27-137">Skála hello ügynökök</span><span class="sxs-lookup"><span data-stu-id="b6f27-137">Scale hello agents</span></span>

<span data-ttu-id="b6f27-138">Ha az alapértelmezett parancsokkal hello előző oktatóprogram Kubernetes fürt hozott létre, három ügynök csomópontot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b6f27-138">If you created your Kubernetes cluster using default commands in hello previous tutorial, it has three agent nodes.</span></span> <span data-ttu-id="b6f27-139">Ha a fürt több vagy kevesebb tároló munkaterhelések, manuálisan módosíthatja hello ügynökök száma.</span><span class="sxs-lookup"><span data-stu-id="b6f27-139">You can adjust hello number of agents manually if you plan more or fewer container workloads on your cluster.</span></span> <span data-ttu-id="b6f27-140">Használjon hello [az acs méretezése](/cli/azure/acs#scale) parancsot, és adja meg a hello ügynökök száma az hello `--new-agent-count` paraméter.</span><span class="sxs-lookup"><span data-stu-id="b6f27-140">Use hello [az acs scale](/cli/azure/acs#scale) command, and specify hello number of agents with hello `--new-agent-count` parameter.</span></span>

<span data-ttu-id="b6f27-141">hello alábbi példa növeli az ügynök csomópontok too4 hello Kubernetes fürt nevű hello száma *myK8sCluster*.</span><span class="sxs-lookup"><span data-stu-id="b6f27-141">hello following example increases hello number of agent nodes too4 in hello Kubernetes cluster named *myK8sCluster*.</span></span> <span data-ttu-id="b6f27-142">hello parancs néhány perc toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b6f27-142">hello command takes a couple of minutes toocomplete.</span></span>

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

<span data-ttu-id="b6f27-143">hello parancs kimenetét mutatja hello számát ügynök csomópontok hello értékének `agentPoolProfiles:count`:</span><span class="sxs-lookup"><span data-stu-id="b6f27-143">hello command output shows hello number of agent nodes in hello value of `agentPoolProfiles:count`:</span></span>

```azurecli
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a><span data-ttu-id="b6f27-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6f27-144">Next steps</span></span>

<span data-ttu-id="b6f27-145">Ebben az oktatóanyagban használt különböző méretezési szolgáltatásait a Kubernetes fürt.</span><span class="sxs-lookup"><span data-stu-id="b6f27-145">In this tutorial, you used different scaling features in your Kubernetes cluster.</span></span> <span data-ttu-id="b6f27-146">Feladatok kezelt tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b6f27-146">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b6f27-147">Manuális skálázás Kubernetes három munkaállomás-csoporttal</span><span class="sxs-lookup"><span data-stu-id="b6f27-147">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="b6f27-148">Automatikus skálázás három munkaállomás-csoporttal hello alkalmazás előtér futtató konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b6f27-148">Configuring Autoscale pods running hello app front end</span></span>
> * <span data-ttu-id="b6f27-149">Hello Kubernetes Azure ügynök csomópontok méretezése</span><span class="sxs-lookup"><span data-stu-id="b6f27-149">Scale hello Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="b6f27-150">Előzetes toohello oktatóanyag következő toolearn Kubernetes alkalmazás frissítésével kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b6f27-150">Advance toohello next tutorial toolearn about updating application in Kubernetes.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b6f27-151">Kubernetes tartozó alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="b6f27-151">Update an application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-app-update.md)

