---
title: "aaaQuickstart - a Windows Azure Kubernetes fürt |} Microsoft Docs"
description: "Ismerje meg gyorsan, toocreate a Kubernetes fürtökben az Azure Tárolószolgáltatásban tárolók Windows hello Azure parancssori felület."
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 85fe65a46ae8c78797e8a8a097c2a37f06329335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-windows-containers"></a><span data-ttu-id="3f735-103">Kubernetes-fürt üzembe helyezése Windows-tárolókhoz</span><span class="sxs-lookup"><span data-stu-id="3f735-103">Deploy Kubernetes cluster for Windows containers</span></span>

<span data-ttu-id="3f735-104">hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="3f735-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="3f735-105">Ez az útmutató részletek hello Azure CLI toodeploy használatával egy [Kubernetes](https://kubernetes.io/docs/home/) fürthöz [Azure Tárolószolgáltatás](../container-service-intro.md).</span><span class="sxs-lookup"><span data-stu-id="3f735-105">This guide details using hello Azure CLI toodeploy a [Kubernetes](https://kubernetes.io/docs/home/) cluster in [Azure Container Service](../container-service-intro.md).</span></span> <span data-ttu-id="3f735-106">Miután hello fürt központi telepítése, hello Kubernetes tooit csatlakozott `kubectl` parancssori eszköz, és az első Windows tároló üzembe.</span><span class="sxs-lookup"><span data-stu-id="3f735-106">Once hello cluster is deployed, you connect tooit with hello Kubernetes `kubectl` command-line tool, and you deploy your first Windows container.</span></span>

<span data-ttu-id="3f735-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="3f735-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3f735-108">Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="3f735-108">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="3f735-109">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="3f735-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="3f735-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3f735-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

> [!NOTE]
> <span data-ttu-id="3f735-111">Az Azure Container Service szolgáltatásban a Kubernetesen működő Windows-tárolók támogatása előzetes verzióban érhető el.</span><span class="sxs-lookup"><span data-stu-id="3f735-111">Support for Windows containers on Kubernetes in Azure Container Service is in preview.</span></span> 
>

## <a name="create-a-resource-group"></a><span data-ttu-id="3f735-112">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="3f735-112">Create a resource group</span></span>

<span data-ttu-id="3f735-113">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="3f735-113">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="3f735-114">Az Azure-erőforráscsoport olyan logikai csoport, amelyben az Azure-erőforrások üzembe helyezése és kezelése zajlik.</span><span class="sxs-lookup"><span data-stu-id="3f735-114">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="3f735-115">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="3f735-115">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a><span data-ttu-id="3f735-116">Kubernetes-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f735-116">Create Kubernetes cluster</span></span>
<span data-ttu-id="3f735-117">Hozzon létre egy Kubernetes fürtöt az Azure Tárolószolgáltatásban hello [az acs létre](/cli/azure/acs#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="3f735-117">Create a Kubernetes cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="3f735-118">hello alábbi példakód létrehozza a fürt nevű *myK8sCluster* egy Linux fő csomópont és a Windows-ügynök két csomópont.</span><span class="sxs-lookup"><span data-stu-id="3f735-118">hello following example creates a cluster named *myK8sCluster* with one Linux master node and two Windows agent nodes.</span></span> <span data-ttu-id="3f735-119">Ez a példa SSH kulcsok szükséges tooconnect toohello Linuxos fő hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3f735-119">This example creates SSH keys needed tooconnect toohello Linux master.</span></span> <span data-ttu-id="3f735-120">Ez a példa *azureuser* egy rendszergazda felhasználó neve és *myPassword12* hello jelszóként hello Windows csomópontján.</span><span class="sxs-lookup"><span data-stu-id="3f735-120">This example uses *azureuser* for an administrative user name and *myPassword12* as hello password on hello Windows nodes.</span></span> <span data-ttu-id="3f735-121">Ezen értékek toosomething megfelelő tooyour környezet frissítése.</span><span class="sxs-lookup"><span data-stu-id="3f735-121">Update these values toosomething appropriate tooyour environment.</span></span> 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

<span data-ttu-id="3f735-122">Pár perc múlva hello parancs befejeződik, és a telepítéssel kapcsolatos információk láthatók.</span><span class="sxs-lookup"><span data-stu-id="3f735-122">After several minutes, hello command completes, and shows you information about your deployment.</span></span>

## <a name="install-kubectl"></a><span data-ttu-id="3f735-123">A kubectl telepítése</span><span class="sxs-lookup"><span data-stu-id="3f735-123">Install kubectl</span></span>

<span data-ttu-id="3f735-124">az ügyfélszámítógépen, használja a fürt Kubernetes tooconnect toohello [ `kubectl` ](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes parancssori ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3f735-124">tooconnect toohello Kubernetes cluster from your client computer, use [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes command-line client.</span></span> 

<span data-ttu-id="3f735-125">Az Azure CloudShell használata esetén a `kubectl` már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="3f735-125">If you're using Azure CloudShell, `kubectl` is already installed.</span></span> <span data-ttu-id="3f735-126">Ha azt szeretné, hogy tooinstall helyben, hogy használni tudja hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) parancsot.</span><span class="sxs-lookup"><span data-stu-id="3f735-126">If you want tooinstall it locally, you can use hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) command.</span></span>

<span data-ttu-id="3f735-127">a következő példa telepíti az Azure parancssori felület hello `kubectl` tooyour rendszer.</span><span class="sxs-lookup"><span data-stu-id="3f735-127">hello following Azure CLI example installs `kubectl` tooyour system.</span></span> <span data-ttu-id="3f735-128">Windows rendszerben rendszergazdaként futtassa ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="3f735-128">On Windows, run this command as an administrator.</span></span>

```azurecli-interactive 
az acs kubernetes install-cli
```


## <a name="connect-with-kubectl"></a><span data-ttu-id="3f735-129">Kapcsolódás a kubectl parancssori ügyfélhez</span><span class="sxs-lookup"><span data-stu-id="3f735-129">Connect with kubectl</span></span>

<span data-ttu-id="3f735-130">tooconfigure `kubectl` tooconnect tooyour Kubernetes fürthöz, futtassa a hello [az acs kubernetes get-hitelesítő adatok](/cli/azure/acs/kubernetes#get-credentials) parancsot.</span><span class="sxs-lookup"><span data-stu-id="3f735-130">tooconfigure `kubectl` tooconnect tooyour Kubernetes cluster, run hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="3f735-131">hello alábbi példa letölti a Kubernetes fürt hello fürtkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3f735-131">hello following example downloads hello cluster configuration for your Kubernetes cluster.</span></span>

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

<span data-ttu-id="3f735-132">tooverify hello kapcsolat tooyour fürt a számítógépről, futtassa:</span><span class="sxs-lookup"><span data-stu-id="3f735-132">tooverify hello connection tooyour cluster from your machine, try running:</span></span>

```azurecli-interactive
kubectl get nodes
```

<span data-ttu-id="3f735-133">`kubectl`hello fő és ügynök csomópontok listája.</span><span class="sxs-lookup"><span data-stu-id="3f735-133">`kubectl` lists hello master and agent nodes.</span></span>

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```

## <a name="deploy-a-windows-iis-container"></a><span data-ttu-id="3f735-134">Windows IIS-tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="3f735-134">Deploy a Windows IIS container</span></span>

<span data-ttu-id="3f735-135">Docker-tárolót futtathat egy olyan Kubernetes-*podon* belül, amely egy vagy több tárolót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3f735-135">You can run a Docker container inside a Kubernetes *pod*, which contains one or more containers.</span></span> 

<span data-ttu-id="3f735-136">Alapvető példában egy JSON-fájl toospecify egy Microsoft Internet Information Server (IIS) tárolót használ, és létrehozza a hello pod hello használata `kubctl apply` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3f735-136">This basic example uses a JSON file toospecify a Microsoft Internet Information Server (IIS) container, and then creates hello pod using hello `kubctl apply` command.</span></span> 

<span data-ttu-id="3f735-137">Hozzon létre egy helyi fájlt `iis.json` és a következő szöveg másolása hello.</span><span class="sxs-lookup"><span data-stu-id="3f735-137">Create a local file named `iis.json` and copy hello following text.</span></span> <span data-ttu-id="3f735-138">Ez a fájl közli Kubernetes toorun IIS Windows Server 2016 Nano Server, a nyilvános tárolókban lemezképpel [Docker Hub](https://hub.docker.com/r/nanoserver/iis/).</span><span class="sxs-lookup"><span data-stu-id="3f735-138">This file tells Kubernetes toorun IIS on Windows Server 2016 Nano Server, using a public container image from [Docker Hub](https://hub.docker.com/r/nanoserver/iis/).</span></span> <span data-ttu-id="3f735-139">hello tároló 80-as portot, de kezdetben csak hello fürt hálózaton belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="3f735-139">hello container uses port 80, but initially is only accessible within hello cluster network.</span></span>

 ```JSON
 {
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "iis",
    "labels": {
      "name": "iis"
    }
  },
  "spec": {
    "containers": [
      {
        "name": "iis",
        "image": "nanoserver/iis",
        "ports": [
          {
          "containerPort": 80
          }
        ]
      }
    ],
    "nodeSelector": {
     "beta.kubernetes.io/os": "windows"
     }
   }
 }
 ```

<span data-ttu-id="3f735-140">toostart hello pod, típus:</span><span class="sxs-lookup"><span data-stu-id="3f735-140">toostart hello pod, type:</span></span>
  
```azurecli-interactive
kubectl apply -f iis.json
```  

<span data-ttu-id="3f735-141">tootrack hello telepítés, típus:</span><span class="sxs-lookup"><span data-stu-id="3f735-141">tootrack hello deployment, type:</span></span>
  
```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="3f735-142">Hello pod telepít, amíg hello állapota `ContainerCreating`.</span><span class="sxs-lookup"><span data-stu-id="3f735-142">While hello pod is deploying, hello status is `ContainerCreating`.</span></span> <span data-ttu-id="3f735-143">Hello tároló tooenter hello néhány percet is igénybe vehet `Running` állapotát.</span><span class="sxs-lookup"><span data-stu-id="3f735-143">It can take a few minutes for hello container tooenter hello `Running` state.</span></span>

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="3f735-144">Nézet hello IIS-kezdőlap</span><span class="sxs-lookup"><span data-stu-id="3f735-144">View hello IIS welcome page</span></span>

<span data-ttu-id="3f735-145">tooexpose hello pod toohello globális nyilvános IP-címmel, a következő parancs típusa hello:</span><span class="sxs-lookup"><span data-stu-id="3f735-145">tooexpose hello pod toohello world with a public IP address, type hello following command:</span></span>

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

<span data-ttu-id="3f735-146">Ezzel a paranccsal Kubernetes létrehoz egy szolgáltatást, és egy [Azure terheléselosztó szabályhoz](container-service-kubernetes-load-balancing.md) hello szolgáltatás nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="3f735-146">With this command, Kubernetes creates a service and an [Azure load balancer rule](container-service-kubernetes-load-balancing.md) with a public IP address for hello service.</span></span> 

<span data-ttu-id="3f735-147">Futtassa a következő parancs toosee hello hello szolgáltatás állapotának hello.</span><span class="sxs-lookup"><span data-stu-id="3f735-147">Run hello following command toosee hello status of hello service.</span></span>

```azurecli-interactive
kubectl get svc
```

<span data-ttu-id="3f735-148">Kezdetben hello IP-cím jelenik meg `pending`.</span><span class="sxs-lookup"><span data-stu-id="3f735-148">Initially hello IP address appears as `pending`.</span></span> <span data-ttu-id="3f735-149">Néhány perc elteltével hello hello külső IP-címe `iis` pod van beállítva:</span><span class="sxs-lookup"><span data-stu-id="3f735-149">After a few minutes, hello external IP address of hello `iis` pod is set:</span></span>
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

<span data-ttu-id="3f735-150">A choice toosee hello alapértelmezett IIS üdvözlőlap webböngészővel hello külső IP-címen használhatja:</span><span class="sxs-lookup"><span data-stu-id="3f735-150">You can use a web browser of your choice toosee hello default IIS welcome page at hello external IP address:</span></span>

![Böngészés tooIIS képe](./media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


## <a name="delete-cluster"></a><span data-ttu-id="3f735-152">Fürt törlése</span><span class="sxs-lookup"><span data-stu-id="3f735-152">Delete cluster</span></span>
<span data-ttu-id="3f735-153">Amikor hello fürt már nem szükséges, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, a tárolószolgáltatás és a minden kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="3f735-153">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a><span data-ttu-id="3f735-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f735-154">Next steps</span></span>

<span data-ttu-id="3f735-155">A rövid útmutató segítségével üzembe helyezett egy Kubernetes-fürtöt, kapcsolatot hozott létre a `kubectl` parancssori ügyféllel, és üzembe helyezett egy IIS-tárolóval rendelkező podot.</span><span class="sxs-lookup"><span data-stu-id="3f735-155">In this quick start, you deployed a Kubernetes cluster, connected with `kubectl`, and deployed a pod with an IIS container.</span></span> <span data-ttu-id="3f735-156">További információ az Azure Tárolószolgáltatás toolearn toohello Kubernetes oktatóanyag továbbra is.</span><span class="sxs-lookup"><span data-stu-id="3f735-156">toolearn more about Azure Container Service, continue toohello Kubernetes tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3f735-157">ACS Kubernetes-fürtök kezelése</span><span class="sxs-lookup"><span data-stu-id="3f735-157">Manage an ACS Kubernetes cluster</span></span>](container-service-tutorial-kubernetes-prepare-app.md)
