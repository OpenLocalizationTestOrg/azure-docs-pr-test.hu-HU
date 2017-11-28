---
title: "Azure Tárolószolgáltatás útmutató - fürt központi telepítése |} Microsoft Docs"
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
ms.openlocfilehash: 472697c1f0c18859087d7b448e1786d85c27aca0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a><span data-ttu-id="12d73-104">Az Azure Tárolószolgáltatásban Kubernetes fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="12d73-104">Deploy a Kubernetes cluster in Azure Container Service</span></span>

<span data-ttu-id="12d73-105">Kubernetes elosztott platformot kínál a tárolóalapú alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="12d73-105">Kubernetes provides a distributed platform for containerized applications.</span></span> <span data-ttu-id="12d73-106">Azure Tárolószolgáltatás készen áll a Kubernetes éles fürt üzembe esetén egyszerűen és gyorsan.</span><span class="sxs-lookup"><span data-stu-id="12d73-106">With Azure Container Service, provisioning of a production ready Kubernetes cluster is simple and quick.</span></span> <span data-ttu-id="12d73-107">Ez az oktatóanyag, 7, 3. rész egy Azure-tároló szolgáltatás Kubernetes fürtre van telepítve.</span><span class="sxs-lookup"><span data-stu-id="12d73-107">In this tutorial, part 3 of 7, an Azure Container Service Kubernetes cluster is deployed.</span></span> <span data-ttu-id="12d73-108">Befejeződött a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="12d73-108">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12d73-109">Egy Kubernetes ACS-fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="12d73-109">Deploying a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="12d73-110">A Kubernetes CLI (kubectl) telepítése</span><span class="sxs-lookup"><span data-stu-id="12d73-110">Installation of the Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="12d73-111">Kubectl konfigurálása</span><span class="sxs-lookup"><span data-stu-id="12d73-111">Configuration of kubectl</span></span>

<span data-ttu-id="12d73-112">A következő útmutatókból az Azure szavazattal alkalmazás központi telepítése a fürt, méretezhető, frissítése, és az Operations Management Suite a Kubernetes fürt figyelésére van beállítva.</span><span class="sxs-lookup"><span data-stu-id="12d73-112">In subsequent tutorials, the Azure Vote application is deployed to the cluster, scaled, updated, and Operations Management Suite is configured to monitor the Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="12d73-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="12d73-113">Before you begin</span></span>

<span data-ttu-id="12d73-114">Az előző oktatóanyagokat a tároló-lemezkép létrejött, de feltöltött egy Azure-tároló beállításjegyzék-példányon.</span><span class="sxs-lookup"><span data-stu-id="12d73-114">In previous tutorials, a container image was created and uploaded to an Azure Container Registry instance.</span></span> <span data-ttu-id="12d73-115">Ha nem volna ezeket a lépéseket, és szeretné követéséhez, vissza [oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="12d73-115">If you have not done these steps, and would like to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span>

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="12d73-116">Kubernetes-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="12d73-116">Create Kubernetes cluster</span></span>

<span data-ttu-id="12d73-117">Az a [az oktatóanyag előző](./container-service-tutorial-kubernetes-prepare-acr.md), nevű erőforráscsoport *myResourceGroup* lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="12d73-117">In the [previous tutorial](./container-service-tutorial-kubernetes-prepare-acr.md), a resource group named *myResourceGroup* was created.</span></span> <span data-ttu-id="12d73-118">Ha nem tette, ez az erőforráscsoport most hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="12d73-118">If you have not done so, create this resource group now.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="12d73-119">Hozzon létre egy Kubernetes-fürtöt az Azure Container Service-ben az [az acs create](/cli/azure/acs#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="12d73-119">Create a Kubernetes cluster in Azure Container Service with the [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="12d73-120">A következő példa egy *myK8sCluster* nevű fürtöt hoz létre egy Linux-főcsomóponttal és három Linux-ügyfélcsomóponttal.</span><span class="sxs-lookup"><span data-stu-id="12d73-120">The following example creates a cluster named *myK8sCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

<span data-ttu-id="12d73-121">Pár perc múlva a parancs végrehajtását, és értéket ad vissza json formátumú az ACS telepítési információkat.</span><span class="sxs-lookup"><span data-stu-id="12d73-121">After several minutes, the command completes, and returns json formatted information about the ACS deployment.</span></span>

## <a name="install-the-kubectl-cli"></a><span data-ttu-id="12d73-122">A kubectl parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="12d73-122">Install the kubectl CLI</span></span>

<span data-ttu-id="12d73-123">Az Kubernetes fürthöz csatlakoztatja az ügyfélszámítógépről, használja a [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), a Kubernetes parancssori ügyfél.</span><span class="sxs-lookup"><span data-stu-id="12d73-123">To connect to the Kubernetes cluster from your client computer, use [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), the Kubernetes command-line client.</span></span> 

<span data-ttu-id="12d73-124">Az Azure CloudShell használata esetén a `kubectl` már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="12d73-124">If you're using Azure CloudShell, `kubectl` is already installed.</span></span> <span data-ttu-id="12d73-125">Ha helyileg telepíteni szeretne, használja a [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) parancsot.</span><span class="sxs-lookup"><span data-stu-id="12d73-125">If you want to install it locally, use the [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="12d73-126">Ha a Linux vagy macOS fut, szükség lehet a sudo futtatásához.</span><span class="sxs-lookup"><span data-stu-id="12d73-126">If running in Linux or macOS, you may need to run with sudo.</span></span> <span data-ttu-id="12d73-127">A Windows győződjön meg arról, a rendszerhéj futtatása rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="12d73-127">On Windows, ensure your shell has been run as administrator.</span></span>

```azurecli-interactive 
az acs kubernetes install-cli 
```

<span data-ttu-id="12d73-128">A Windows, az alapértelmezett telepítési van *c:\program files (x86)\kubectl.exe*.</span><span class="sxs-lookup"><span data-stu-id="12d73-128">On Windows, the default installation is *c:\program files (x86)\kubectl.exe*.</span></span> <span data-ttu-id="12d73-129">Szükség lehet a fájl hozzáadása a Windows útvonalhoz.</span><span class="sxs-lookup"><span data-stu-id="12d73-129">You may need to add this file to the Windows path.</span></span> 

## <a name="connect-with-kubectl"></a><span data-ttu-id="12d73-130">Kapcsolódás a kubectl parancssori ügyfélhez</span><span class="sxs-lookup"><span data-stu-id="12d73-130">Connect with kubectl</span></span>

<span data-ttu-id="12d73-131">Az [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) parancs futtatásával konfigurálja a `kubectl` ügyfelet úgy, hogy a saját Kubernetes-fürthöz kapcsolódjon.</span><span class="sxs-lookup"><span data-stu-id="12d73-131">To configure `kubectl` to connect to your Kubernetes cluster, run the [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

<span data-ttu-id="12d73-132">Ellenőrizze a kapcsolatot a fürthöz, futtassa a [kubectl beolvasása csomópontok](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.</span><span class="sxs-lookup"><span data-stu-id="12d73-132">To verify the connection to your cluster, run the [kubectl get nodes](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="12d73-133">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="12d73-133">Output:</span></span>

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

<span data-ttu-id="12d73-134">Oktatóanyag befejezése után következő, hogy az ACS Kubernetes fürt készen áll a munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="12d73-134">At tutorial completion, you have an ACS Kubernetes cluster ready for workloads.</span></span> <span data-ttu-id="12d73-135">A következő útmutatókból egy több tároló alkalmazás üzemel a fürthöz, horizontálisan, frissítése, és figyeli.</span><span class="sxs-lookup"><span data-stu-id="12d73-135">In subsequent tutorials, a multi-container application is deployed to this cluster, scaled out, updated, and monitored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12d73-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12d73-136">Next steps</span></span>

<span data-ttu-id="12d73-137">Ebben az oktatóanyagban az Azure-tároló szolgáltatás Kubernetes fürt tették elérhetővé telepítésre.</span><span class="sxs-lookup"><span data-stu-id="12d73-137">In this tutorial, an Azure Container Service Kubernetes cluster was deployed.</span></span> <span data-ttu-id="12d73-138">A következő lépéseket hajtotta végre:</span><span class="sxs-lookup"><span data-stu-id="12d73-138">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12d73-139">Egy Kubernetes ACS-fürt telepítése</span><span class="sxs-lookup"><span data-stu-id="12d73-139">Deployed a Kubernetes ACS cluster</span></span>
> * <span data-ttu-id="12d73-140">A Kubernetes CLI (kubectl) telepítése</span><span class="sxs-lookup"><span data-stu-id="12d73-140">Installed the Kubernetes CLI (kubectl)</span></span>
> * <span data-ttu-id="12d73-141">Konfigurált kubectl</span><span class="sxs-lookup"><span data-stu-id="12d73-141">Configured kubectl</span></span>

<span data-ttu-id="12d73-142">Továbblépés a következő oktatóanyag fürtben futó alkalmazás megismeréséhez.</span><span class="sxs-lookup"><span data-stu-id="12d73-142">Advance to the next tutorial to learn about running application on the cluster.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="12d73-143">Kubernetes az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="12d73-143">Deploy application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-deploy-application.md)
