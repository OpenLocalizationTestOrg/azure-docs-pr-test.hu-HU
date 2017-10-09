---
title: "aaaQuickstart - Azure Kubernetes fürt Linux |} Microsoft Docs"
description: "Gyorsan további toocreate a Linux tárolók az Azure Tárolószolgáltatásban hello Azure CLI Kubernetes fürtökben."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 8b0d7a803148c1cbf329f4b76f2e99b4b7e14983
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-linux-containers"></a><span data-ttu-id="72f5b-103">Kubernetes-fürt üzembe helyezése Linux-tárolók esetén</span><span class="sxs-lookup"><span data-stu-id="72f5b-103">Deploy Kubernetes cluster for Linux containers</span></span>

<span data-ttu-id="72f5b-104">A gyors üzembe helyezési Kubernetes fürttagként van telepítve hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="72f5b-104">In this quick start, a Kubernetes cluster is deployed using hello Azure CLI.</span></span> <span data-ttu-id="72f5b-105">Egy előtér-webkiszolgáló és a Redis példánya több tároló alkalmazás majd üzemel, és futtatása hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="72f5b-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on hello cluster.</span></span> <span data-ttu-id="72f5b-106">Ezt követően hello alkalmazás keresztül érhető el-e hello internet.</span><span class="sxs-lookup"><span data-stu-id="72f5b-106">Once completed, hello application is accessible over hello internet.</span></span> 

<span data-ttu-id="72f5b-107">a dokumentumban használt hello mintaalkalmazás Python nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="72f5b-107">hello example application used in this document is written in Python.</span></span> <span data-ttu-id="72f5b-108">bármely tároló kép Kubernetes fürtbe használt toodeploy lehetnek hello fogalmakat és itt részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="72f5b-108">hello concepts and steps detailed here can be used toodeploy any container image into a Kubernetes cluster.</span></span> <span data-ttu-id="72f5b-109">hello kódot, Dockerfile, és előre létrehozott Kubernetes jegyzékfájlt kapcsolódó toothis projekt legyenek elérhetők a [GitHub](https://github.com/Azure-Samples/azure-voting-app-redis.git).</span><span class="sxs-lookup"><span data-stu-id="72f5b-109">hello code, Dockerfile, and pre-created Kubernetes manifest files related toothis project are available on [GitHub](https://github.com/Azure-Samples/azure-voting-app-redis.git).</span></span>

![Böngészés tooAzure szavazattal képe](media/container-service-kubernetes-walkthrough/azure-vote.png)

<span data-ttu-id="72f5b-111">A gyors üzembe helyezési azt feltételezi, hogy alapszinten megértse, Kubernetes fogalmakat, Kubernetes részletes információkat lásd: hello [Kubernetes dokumentáció]( https://kubernetes.io/docs/home/).</span><span class="sxs-lookup"><span data-stu-id="72f5b-111">This quick start assumes a basic understanding of Kubernetes concepts, for detailed information on Kubernetes see hello [Kubernetes documentation]( https://kubernetes.io/docs/home/).</span></span>

<span data-ttu-id="72f5b-112">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="72f5b-112">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="72f5b-113">Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="72f5b-113">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="72f5b-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="72f5b-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="72f5b-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="72f5b-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="72f5b-116">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="72f5b-116">Create a resource group</span></span>

<span data-ttu-id="72f5b-117">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="72f5b-117">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="72f5b-118">Az Azure-erőforráscsoport olyan logikai csoport, amelyben az Azure-erőforrások üzembe helyezése és kezelése zajlik.</span><span class="sxs-lookup"><span data-stu-id="72f5b-118">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="72f5b-119">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westeurope* helyét.</span><span class="sxs-lookup"><span data-stu-id="72f5b-119">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="72f5b-120">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="72f5b-120">Output:</span></span>

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westeurope",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="72f5b-121">Kubernetes-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="72f5b-121">Create Kubernetes cluster</span></span>

<span data-ttu-id="72f5b-122">Hozzon létre egy Kubernetes fürtöt az Azure Tárolószolgáltatásban hello [az acs létre](/cli/azure/acs#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="72f5b-122">Create a Kubernetes cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> <span data-ttu-id="72f5b-123">hello alábbi példakód létrehozza a fürt nevű *myK8sCluster* egy Linux fő csomópont- és Linux-ügynök három csomópontot.</span><span class="sxs-lookup"><span data-stu-id="72f5b-123">hello following example creates a cluster named *myK8sCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8sCluster --generate-ssh-keys 
```

<span data-ttu-id="72f5b-124">Pár perc múlva hello parancs befejeződött, és hello fürt json formátumú információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="72f5b-124">After several minutes, hello command completes and returns json formatted information about hello cluster.</span></span> 

## <a name="connect-toohello-cluster"></a><span data-ttu-id="72f5b-125">Csatlakoztassa toohello fürtöt</span><span class="sxs-lookup"><span data-stu-id="72f5b-125">Connect toohello cluster</span></span>

<span data-ttu-id="72f5b-126">toomanage Kubernetes fürt esetén használjon [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes parancssori ügyfél.</span><span class="sxs-lookup"><span data-stu-id="72f5b-126">toomanage a Kubernetes cluster, use [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes command-line client.</span></span> 

<span data-ttu-id="72f5b-127">Ha az Azure CloudShellt használja, a kubectl már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="72f5b-127">If you're using Azure CloudShell, kubectl is already installed.</span></span> <span data-ttu-id="72f5b-128">Ha azt szeretné, hogy tooinstall helyben, hogy használni tudja hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) parancsot.</span><span class="sxs-lookup"><span data-stu-id="72f5b-128">If you want tooinstall it locally, you can use hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="72f5b-129">tooconfigure kubectl tooconnect tooyour Kubernetes fürthöz, futtassa a hello [az acs kubernetes get-hitelesítő adatok](/cli/azure/acs/kubernetes#get-credentials) parancsot.</span><span class="sxs-lookup"><span data-stu-id="72f5b-129">tooconfigure kubectl tooconnect tooyour Kubernetes cluster, run hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="72f5b-130">Ez a lépés tölti le a hitelesítő adatokat, és konfigurálja a hello Kubernetes CLI toouse őket.</span><span class="sxs-lookup"><span data-stu-id="72f5b-130">This step downloads credentials and configures hello Kubernetes CLI toouse them.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

<span data-ttu-id="72f5b-131">tooverify hello kapcsolat tooyour fürt használata hello [kubectl beolvasása](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancs tooreturn hello fürtcsomópontok listáját.</span><span class="sxs-lookup"><span data-stu-id="72f5b-131">tooverify hello connection tooyour cluster, use hello [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command tooreturn a list of hello cluster nodes.</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="72f5b-132">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="72f5b-132">Output:</span></span>

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-14ad53a1-0    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-1    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-2    Ready                      10m       v1.6.6
k8s-master-14ad53a1-0   Ready,SchedulingDisabled   10m       v1.6.6
```

## <a name="run-hello-application"></a><span data-ttu-id="72f5b-133">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="72f5b-133">Run hello application</span></span>

<span data-ttu-id="72f5b-134">Kubernetes jegyzékfájlt határozza meg a kívánt állapot hello fürt, beleértve a mi tároló képek rendszerűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="72f5b-134">A Kubernetes manifest file defines a desired state for hello cluster, including what container images should be running.</span></span> <span data-ttu-id="72f5b-135">Ebben a példában a jegyzék esetén használt toocreate szükséges toorun hello Azure szavazattal alkalmazás objektumot.</span><span class="sxs-lookup"><span data-stu-id="72f5b-135">For this example, a manifest is used toocreate all objects needed toorun hello Azure Vote application.</span></span> 

<span data-ttu-id="72f5b-136">Hozzon létre egy fájlt `azure-vote.yml` és másolja át azt a következő YAM hello.</span><span class="sxs-lookup"><span data-stu-id="72f5b-136">Create a file named `azure-vote.yml` and copy into it hello following YAML.</span></span> <span data-ttu-id="72f5b-137">Ha az Azure Cloud Shellben dolgozik, ez a fájl a vi vagy a Nano segítségével hozható létre, ugyanúgy, mint egy virtuális vagy fizikai rendszeren.</span><span class="sxs-lookup"><span data-stu-id="72f5b-137">If you are working in Azure Cloud Shell, this file can be created using vi or Nano as if working on a virtual or physical system.</span></span>

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:redis-v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

<span data-ttu-id="72f5b-138">Használjon hello [kubectl létrehozása](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) toorun hello alkalmazás parancsot.</span><span class="sxs-lookup"><span data-stu-id="72f5b-138">Use hello [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) command toorun hello application.</span></span>

```azurecli-interactive
kubectl create -f azure-vote.yml
```

<span data-ttu-id="72f5b-139">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="72f5b-139">Output:</span></span>

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-hello-application"></a><span data-ttu-id="72f5b-140">Hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="72f5b-140">Test hello application</span></span>

<span data-ttu-id="72f5b-141">Hello alkalmazás fut, mint egy [Kubernetes szolgáltatás](https://kubernetes.io/docs/concepts/services-networking/service/) jön létre, hogy tesz elérhetővé hello alkalmazás előtér toohello internet.</span><span class="sxs-lookup"><span data-stu-id="72f5b-141">As hello application is run, a [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) is created that exposes hello application front end toohello internet.</span></span> <span data-ttu-id="72f5b-142">A folyamat eltarthat néhány percig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="72f5b-142">This process can take a few minutes toocomplete.</span></span> 

<span data-ttu-id="72f5b-143">toomonitor folyamatban, használjon hello [kubectl beolvasása szolgáltatás](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) hello parancsot `--watch` argumentum.</span><span class="sxs-lookup"><span data-stu-id="72f5b-143">toomonitor progress, use hello [kubectl get service](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command with hello `--watch` argument.</span></span>

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

<span data-ttu-id="72f5b-144">Kezdetben hello **külső IP-** a hello *azure-szavazat-front* szolgáltatás jelenik meg *függőben lévő*.</span><span class="sxs-lookup"><span data-stu-id="72f5b-144">Initially hello **EXTERNAL-IP** for hello *azure-vote-front* service appears as *pending*.</span></span> <span data-ttu-id="72f5b-145">Miután hello külső IP-cím megváltozott *függőben lévő* tooan *IP-cím*, használjon `CTRL-C` toostop hello kubectl figyelési folyamat.</span><span class="sxs-lookup"><span data-stu-id="72f5b-145">Once hello EXTERNAL-IP address has changed from *pending* tooan *IP address*, use `CTRL-C` toostop hello kubectl watch process.</span></span> 
  
```bash
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

<span data-ttu-id="72f5b-146">Tallózhatnak toohello külső IP cím toosee hello Azure szavazattal alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="72f5b-146">You can now browse toohello external IP address toosee hello Azure Vote App.</span></span>

![Böngészés tooAzure szavazattal képe](media/container-service-kubernetes-walkthrough/azure-vote.png)  

## <a name="delete-cluster"></a><span data-ttu-id="72f5b-148">Fürt törlése</span><span class="sxs-lookup"><span data-stu-id="72f5b-148">Delete cluster</span></span>
<span data-ttu-id="72f5b-149">Amikor hello fürt már nem szükséges, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, a tárolószolgáltatás és a minden kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="72f5b-149">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a><span data-ttu-id="72f5b-150">Hello kód beolvasása</span><span class="sxs-lookup"><span data-stu-id="72f5b-150">Get hello code</span></span>

<span data-ttu-id="72f5b-151">A gyors üzembe helyezési az előre létrehozott tároló képek használt toocreate Kubernetes központi telepítés volt.</span><span class="sxs-lookup"><span data-stu-id="72f5b-151">In this quick start, pre-created container images have been used toocreate a Kubernetes deployment.</span></span> <span data-ttu-id="72f5b-152">hello alkalmazáskód, Dockerfile, és a kapcsolódó Kubernetes jegyzékfájl a Githubon érhetők el.</span><span class="sxs-lookup"><span data-stu-id="72f5b-152">hello related application code, Dockerfile, and Kubernetes manifest file are available on GitHub.</span></span>

[<span data-ttu-id="72f5b-153">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="72f5b-153">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="72f5b-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72f5b-154">Next steps</span></span>

<span data-ttu-id="72f5b-155">A gyors üzembe helyezési Kubernetes fürt telepített, és a tároló több alkalmazás tooit telepítve.</span><span class="sxs-lookup"><span data-stu-id="72f5b-155">In this quick start, you deployed a Kubernetes cluster and deployed a multi-container application tooit.</span></span> 

<span data-ttu-id="72f5b-156">További információ az Azure Tárolószolgáltatás és a lépésein végighaladva teljes kód toodeployment példában toolearn toohello Kubernetes fürt oktatóanyag továbbra is.</span><span class="sxs-lookup"><span data-stu-id="72f5b-156">toolearn more about Azure Container Service, and walk through a complete code toodeployment example, continue toohello Kubernetes cluster tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="72f5b-157">ACS Kubernetes-fürtök kezelése</span><span class="sxs-lookup"><span data-stu-id="72f5b-157">Manage an ACS Kubernetes cluster</span></span>](./container-service-tutorial-kubernetes-prepare-app.md)
