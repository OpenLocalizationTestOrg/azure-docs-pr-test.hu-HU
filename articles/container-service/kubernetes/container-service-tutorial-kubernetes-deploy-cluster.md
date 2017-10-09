---
title: "aaaAzure Tárolószolgáltatás oktatóanyag - fürt központi telepítése |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató - fürt központi telepítése"
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
ms.openlocfilehash: c4c8cc95c88d9c2077d0322f57e5d3159e2dd0ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a><span data-ttu-id="c1294-104">Az Azure Tárolószolgáltatásban Kubernetes fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="c1294-104">Deploy a Kubernetes cluster in Azure Container Service</span></span>

<span data-ttu-id="c1294-105">Kubernetes elosztott platformot kínál a tárolóalapú alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c1294-105">Kubernetes provides a distributed platform for containerized applications.</span></span> <span data-ttu-id="c1294-106">Azure Tárolószolgáltatás készen áll a Kubernetes éles fürt üzembe esetén egyszerűen és gyorsan.</span><span class="sxs-lookup"><span data-stu-id="c1294-106">With Azure Container Service, provisioning of a production ready Kubernetes cluster is simple and quick.</span></span> <span data-ttu-id="c1294-107">Ez az oktatóanyag, 7, 3. rész egy Azure-tároló szolgáltatás Kubernetes fürtre van telepítve.</span><span class="sxs-lookup"><span data-stu-id="c1294-107">In this tutorial, part 3 of 7, an Azure Container Service Kubernetes cluster is deployed.</span></span> <span data-ttu-id="c1294-108">Befejeződött a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="c1294-108">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1294-109">Egy Kubernetes ACS-fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="c1294-109">Deploying a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="c1294-110">Hello Kubernetes CLI (kubectl) telepítése</span><span class="sxs-lookup"><span data-stu-id="c1294-110">Installation of hello Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="c1294-111">Kubectl konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1294-111">Configuration of kubectl</span></span>

<span data-ttu-id="c1294-112">A következő útmutatókból hello Azure szavazattal alkalmazás telepített toohello fürt, méretezhető, frissítése és az Operations Management Suite konfigurált toomonitor hello Kubernetes fürt.</span><span class="sxs-lookup"><span data-stu-id="c1294-112">In subsequent tutorials, hello Azure Vote application is deployed toohello cluster, scaled, updated, and Operations Management Suite is configured toomonitor hello Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c1294-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c1294-113">Before you begin</span></span>

<span data-ttu-id="c1294-114">Az előző oktatóanyagok a tároló-lemezkép létrehozásának és fel kell tölteni tooan Azure tároló beállításjegyzék példány.</span><span class="sxs-lookup"><span data-stu-id="c1294-114">In previous tutorials, a container image was created and uploaded tooan Azure Container Registry instance.</span></span> <span data-ttu-id="c1294-115">Ha nem volna ezeket a lépéseket, és szeretné mentén toofollow, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="c1294-115">If you have not done these steps, and would like toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span>

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="c1294-116">Kubernetes-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1294-116">Create Kubernetes cluster</span></span>

<span data-ttu-id="c1294-117">A hello [az oktatóanyag előző](./container-service-tutorial-kubernetes-prepare-acr.md), nevű erőforráscsoport *myResourceGroup* lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="c1294-117">In hello [previous tutorial](./container-service-tutorial-kubernetes-prepare-acr.md), a resource group named *myResourceGroup* was created.</span></span> <span data-ttu-id="c1294-118">Ha nem tette, ez az erőforráscsoport most hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="c1294-118">If you have not done so, create this resource group now.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="c1294-119">Hozzon létre egy Kubernetes fürtöt az Azure Tárolószolgáltatásban hello [az acs létre](/cli/azure/acs#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1294-119">Create a Kubernetes cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="c1294-120">hello alábbi példakód létrehozza a fürt nevű *myK8sCluster* egy Linux fő csomópont- és Linux-ügynök három csomópontot.</span><span class="sxs-lookup"><span data-stu-id="c1294-120">hello following example creates a cluster named *myK8sCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

<span data-ttu-id="c1294-121">Pár perc múlva hello parancs végrehajtását, és értéket ad vissza json formátumú hello ACS fejlesztésével kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="c1294-121">After several minutes, hello command completes, and returns json formatted information about hello ACS deployment.</span></span>

## <a name="install-hello-kubectl-cli"></a><span data-ttu-id="c1294-122">Hello kubectl parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="c1294-122">Install hello kubectl CLI</span></span>

<span data-ttu-id="c1294-123">az ügyfélszámítógépen, használja a fürt Kubernetes tooconnect toohello [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes parancssori ügyfél.</span><span class="sxs-lookup"><span data-stu-id="c1294-123">tooconnect toohello Kubernetes cluster from your client computer, use [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes command-line client.</span></span> 

<span data-ttu-id="c1294-124">Az Azure CloudShell használata esetén a `kubectl` már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="c1294-124">If you're using Azure CloudShell, `kubectl` is already installed.</span></span> <span data-ttu-id="c1294-125">Ha azt szeretné, hogy tooinstall helyileg, használja a hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1294-125">If you want tooinstall it locally, use hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="c1294-126">Ha a Linux vagy macOS fut, szükség lehet a sudo toorun.</span><span class="sxs-lookup"><span data-stu-id="c1294-126">If running in Linux or macOS, you may need toorun with sudo.</span></span> <span data-ttu-id="c1294-127">A Windows győződjön meg arról, a rendszerhéj futtatása rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c1294-127">On Windows, ensure your shell has been run as administrator.</span></span>

```azurecli-interactive 
az acs kubernetes install-cli 
```

<span data-ttu-id="c1294-128">A Windows hello alapértelmezett telepítése van *c:\program files (x86)\kubectl.exe*.</span><span class="sxs-lookup"><span data-stu-id="c1294-128">On Windows, hello default installation is *c:\program files (x86)\kubectl.exe*.</span></span> <span data-ttu-id="c1294-129">Szükség lehet tooadd a toohello Windows elérési útja.</span><span class="sxs-lookup"><span data-stu-id="c1294-129">You may need tooadd this file toohello Windows path.</span></span> 

## <a name="connect-with-kubectl"></a><span data-ttu-id="c1294-130">Kapcsolódás a kubectl parancssori ügyfélhez</span><span class="sxs-lookup"><span data-stu-id="c1294-130">Connect with kubectl</span></span>

<span data-ttu-id="c1294-131">tooconfigure `kubectl` tooconnect tooyour Kubernetes fürthöz, futtassa a hello [az acs kubernetes get-hitelesítő adatok](/cli/azure/acs/kubernetes#get-credentials) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1294-131">tooconfigure `kubectl` tooconnect tooyour Kubernetes cluster, run hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

<span data-ttu-id="c1294-132">tooverify hello kapcsolat tooyour fürthöz, futtassa a hello [kubectl beolvasása csomópontok](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1294-132">tooverify hello connection tooyour cluster, run hello [kubectl get nodes](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="c1294-133">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="c1294-133">Output:</span></span>

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

<span data-ttu-id="c1294-134">Oktatóanyag befejezése után következő, hogy az ACS Kubernetes fürt készen áll a munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="c1294-134">At tutorial completion, you have an ACS Kubernetes cluster ready for workloads.</span></span> <span data-ttu-id="c1294-135">A következő útmutatókból a tárolót több alkalmazás telepített toothis fürt, horizontálisan, frissítése és figyelemmel kísérni.</span><span class="sxs-lookup"><span data-stu-id="c1294-135">In subsequent tutorials, a multi-container application is deployed toothis cluster, scaled out, updated, and monitored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1294-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1294-136">Next steps</span></span>

<span data-ttu-id="c1294-137">Ebben az oktatóanyagban az Azure-tároló szolgáltatás Kubernetes fürt tették elérhetővé telepítésre.</span><span class="sxs-lookup"><span data-stu-id="c1294-137">In this tutorial, an Azure Container Service Kubernetes cluster was deployed.</span></span> <span data-ttu-id="c1294-138">befejeződtek a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c1294-138">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1294-139">Egy Kubernetes ACS-fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="c1294-139">Deployed a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="c1294-140">Telepített hello Kubernetes CLI (kubectl)</span><span class="sxs-lookup"><span data-stu-id="c1294-140">Installed hello Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="c1294-141">Konfigurált kubectl</span><span class="sxs-lookup"><span data-stu-id="c1294-141">Configured kubectl</span></span>

<span data-ttu-id="c1294-142">Előzetes toohello oktatóanyag következő toolearn kapcsolatos hello fürt alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="c1294-142">Advance toohello next tutorial toolearn about running application on hello cluster.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c1294-143">Kubernetes az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="c1294-143">Deploy application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-deploy-application.md)
