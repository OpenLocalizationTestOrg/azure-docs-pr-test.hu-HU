---
title: "a Docker tároló fürt – az Azure parancssori felület aaaDeploy |} Microsoft Docs"
description: "Kubernetes-, DC/OS- vagy Docker Swarm-megoldás üzembe helyezése az Azure Container Service-ben az Azure CLI 2.0 használatával"
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: saudas
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: cdfa4ce69de343dcc7070bc2c58b5132c4062084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-hello-azure-cli-20"></a><span data-ttu-id="d76e2-103">Egy Docker-tároló üzemeltetési hello Azure CLI 2.0 használatával megoldás telepítése</span><span class="sxs-lookup"><span data-stu-id="d76e2-103">Deploy a Docker container hosting solution using hello Azure CLI 2.0</span></span>

<span data-ttu-id="d76e2-104">Használjon hello `az acs` hello Azure CLI 2.0 toocreate parancsai és az Azure Tárolószolgáltatásban fürtök kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="d76e2-104">Use hello `az acs` commands in hello Azure CLI 2.0 toocreate and manage clusters in Azure Container Service.</span></span> <span data-ttu-id="d76e2-105">Az Azure Tárolószolgáltatás-fürt hello segítségével is telepíthet [Azure-portálon](container-service-deployment.md) vagy hello Azure tároló szolgáltatás API-k.</span><span class="sxs-lookup"><span data-stu-id="d76e2-105">You can also deploy an Azure Container Service cluster by using hello [Azure portal](container-service-deployment.md) or hello Azure Container Service APIs.</span></span>

<span data-ttu-id="d76e2-106">A Súgó a `az acs` parancsokat, és adja át hello `-h` paraméter tooany parancsot.</span><span class="sxs-lookup"><span data-stu-id="d76e2-106">For help on `az acs` commands, pass hello `-h` parameter tooany command.</span></span> <span data-ttu-id="d76e2-107">Például: `az acs create -h`.</span><span class="sxs-lookup"><span data-stu-id="d76e2-107">For example: `az acs create -h`.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="d76e2-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d76e2-108">Prerequisites</span></span>
<span data-ttu-id="d76e2-109">egy Azure Tárolószolgáltatási fürt használt toocreate hello Azure CLI 2.0, meg kell:</span><span class="sxs-lookup"><span data-stu-id="d76e2-109">toocreate an Azure Container Service cluster using hello Azure CLI 2.0, you must:</span></span>
* <span data-ttu-id="d76e2-110">szükség van egy Azure-fiókra ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/));</span><span class="sxs-lookup"><span data-stu-id="d76e2-110">have an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/))</span></span>
* <span data-ttu-id="d76e2-111">telepítette és hello beállítása [Azure CLI 2.0](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="d76e2-111">have installed and set up hello [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

## <a name="get-started"></a><span data-ttu-id="d76e2-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="d76e2-112">Get started</span></span> 
### <a name="log-in-tooyour-account"></a><span data-ttu-id="d76e2-113">Jelentkezzen be tooyour fiók</span><span class="sxs-lookup"><span data-stu-id="d76e2-113">Log in tooyour account</span></span>
```azurecli
az login 
```

<span data-ttu-id="d76e2-114">Kövesse a hello kér toolog az interaktív módon.</span><span class="sxs-lookup"><span data-stu-id="d76e2-114">Follow hello prompts toolog in interactively.</span></span> <span data-ttu-id="d76e2-115">Az egyéb módszerek toolog, lásd: [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="d76e2-115">For other methods toolog in, see [Get started with Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).</span></span>

### <a name="set-your-azure-subscription"></a><span data-ttu-id="d76e2-116">Azure-előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="d76e2-116">Set your Azure subscription</span></span>

<span data-ttu-id="d76e2-117">Ha több Azure-előfizetéssel rendelkezik, állítsa be a hello alapértelmezett előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d76e2-117">If you have more than one Azure subscription, set hello default subscription.</span></span> <span data-ttu-id="d76e2-118">Példa:</span><span class="sxs-lookup"><span data-stu-id="d76e2-118">For example:</span></span>

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a><span data-ttu-id="d76e2-119">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="d76e2-119">Create a resource group</span></span>
<span data-ttu-id="d76e2-120">Javasoljuk, hogy mindegyik fürthöz hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d76e2-120">We recommend that you create a resource group for every cluster.</span></span> <span data-ttu-id="d76e2-121">Válasszon ki egy Azure-régiót, amelyben az Azure Container Service [elérhető](https://azure.microsoft.com/en-us/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="d76e2-121">Specify an Azure region in which Azure Container Service is [available](https://azure.microsoft.com/en-us/regions/services/).</span></span> <span data-ttu-id="d76e2-122">Példa:</span><span class="sxs-lookup"><span data-stu-id="d76e2-122">For example:</span></span>

```azurecli
az group create -n acsrg1 -l "westus"
```
<span data-ttu-id="d76e2-123">Hasonló toohello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="d76e2-123">Output is similar toohello following:</span></span>

![Hozzon létre egy erőforráscsoportot](./media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a><span data-ttu-id="d76e2-125">Azure Container Service-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d76e2-125">Create an Azure Container Service cluster</span></span>

<span data-ttu-id="d76e2-126">toocreate egy fürt használja `az acs create`.</span><span class="sxs-lookup"><span data-stu-id="d76e2-126">toocreate a cluster, use `az acs create`.</span></span>
<span data-ttu-id="d76e2-127">Hello fürt nevét és hello előző lépésben létrehozott hello erőforráscsoport hello nevét kötelező paraméterek tartoznak.</span><span class="sxs-lookup"><span data-stu-id="d76e2-127">A name for hello cluster and hello name of hello resource group created in hello previous step are mandatory parameters.</span></span> 

<span data-ttu-id="d76e2-128">Más bemeneti értékei set toodefault (lásd a következő képernyő hello) kivéve, ha a megfelelő kapcsolók használata felül.</span><span class="sxs-lookup"><span data-stu-id="d76e2-128">Other inputs are set toodefault values (see hello following screen) unless overwritten using their respective switches.</span></span> <span data-ttu-id="d76e2-129">Például a hello orchestrator tooDC/operációs rendszer alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="d76e2-129">For example, hello orchestrator is set by default tooDC/OS.</span></span> <span data-ttu-id="d76e2-130">És ha nem adja meg egy, a DNS-előtagja hello fürt neve alapján jön létre.</span><span class="sxs-lookup"><span data-stu-id="d76e2-130">And if you don't specify one, a DNS name prefix is created based on hello cluster name.</span></span>

![az acs create használata](./media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a><span data-ttu-id="d76e2-132">Gyors `acs create` alapértelmezett beállítások használatával</span><span class="sxs-lookup"><span data-stu-id="d76e2-132">Quick `acs create` using defaults</span></span>
<span data-ttu-id="d76e2-133">Ha egy SSH-RSA nyilvános kulcsfájl `id_rsa.pub` hello alapértelmezett helyen (vagy létrehozni egy a [OS X- és Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) vagy [Windows](../../virtual-machines/linux/ssh-from-windows.md)), használjon hello hasonló parancsot:</span><span class="sxs-lookup"><span data-stu-id="d76e2-133">If you have an SSH RSA public key file `id_rsa.pub` in hello default location (or created one for [OS X and Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../../virtual-machines/linux/ssh-from-windows.md)), use a command like hello following:</span></span>

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
<span data-ttu-id="d76e2-134">Ha nincs nyilvános SSH-kulcsa, használja ezt a második parancsot.</span><span class="sxs-lookup"><span data-stu-id="d76e2-134">If you don't have an SSH public key, use this second command.</span></span> <span data-ttu-id="d76e2-135">Ez a parancs a hello `--generate-ssh-keys` kapcsoló létrehoz egyet.</span><span class="sxs-lookup"><span data-stu-id="d76e2-135">This command with hello `--generate-ssh-keys` switch creates one for you.</span></span>

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

<span data-ttu-id="d76e2-136">Hello parancs megadása után várja meg a létrehozott hello fürt toobe körülbelül 10 perc.</span><span class="sxs-lookup"><span data-stu-id="d76e2-136">After you enter hello command, wait for about 10 minutes for hello cluster toobe created.</span></span> <span data-ttu-id="d76e2-137">hello parancs kimenete hello master és ügynök csomópontok és egy SSH parancs tooconnect toohello első főkiszolgálójának teljesen minősített tartománynevek (FQDN) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d76e2-137">hello command output includes fully qualified domain names (FQDNs) of hello master and agent nodes and an SSH command tooconnect toohello first master.</span></span> <span data-ttu-id="d76e2-138">Íme egy rövidített kimenet:</span><span class="sxs-lookup"><span data-stu-id="d76e2-138">Here is abbreviated output:</span></span>

![Kép az ACS create parancsról](./media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> <span data-ttu-id="d76e2-140">Hello [Kubernetes forgatókönyv](../kubernetes/container-service-kubernetes-walkthrough.md) bemutatja, hogyan toouse `az acs create` az alapértelmezett értékek toocreate egy Kubernetes fürt.</span><span class="sxs-lookup"><span data-stu-id="d76e2-140">hello [Kubernetes walkthrough](../kubernetes/container-service-kubernetes-walkthrough.md) shows how toouse `az acs create` with default values toocreate a Kubernetes cluster.</span></span>
>

## <a name="manage-acs-clusters"></a><span data-ttu-id="d76e2-141">ACS-fürtök kezelése</span><span class="sxs-lookup"><span data-stu-id="d76e2-141">Manage ACS clusters</span></span>

<span data-ttu-id="d76e2-142">További használata `az acs` parancsok toomanage a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d76e2-142">Use additional `az acs` commands toomanage your cluster.</span></span> <span data-ttu-id="d76e2-143">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="d76e2-143">Here are some examples.</span></span>

### <a name="list-clusters-under-a-subscription"></a><span data-ttu-id="d76e2-144">Előfizetés alá tartozó fürtök listázása</span><span class="sxs-lookup"><span data-stu-id="d76e2-144">List clusters under a subscription</span></span>

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a><span data-ttu-id="d76e2-145">Erőforráscsoportba tartozó fürtök listázása</span><span class="sxs-lookup"><span data-stu-id="d76e2-145">List clusters in a resource group</span></span>

```azurecli
az acs list -g acsrg1 --output table
```

![acs list](./media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a><span data-ttu-id="d76e2-147">Container Service-fürt részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="d76e2-147">Display details of a container service cluster</span></span>

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![acs show](./media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-hello-cluster"></a><span data-ttu-id="d76e2-149">Skála hello fürt</span><span class="sxs-lookup"><span data-stu-id="d76e2-149">Scale hello cluster</span></span>
<span data-ttu-id="d76e2-150">Az ügyfélcsomópontok horizontális le- és felskálázása is engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="d76e2-150">Both scaling in and scaling out of agent nodes are allowed.</span></span> <span data-ttu-id="d76e2-151">hello paraméter `new-agent-count` hello új hello ACS fürt ügynökök száma.</span><span class="sxs-lookup"><span data-stu-id="d76e2-151">hello parameter `new-agent-count` is hello new number of agents in hello ACS cluster.</span></span>

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![acs scale](./media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a><span data-ttu-id="d76e2-153">Container Service-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="d76e2-153">Delete a container service cluster</span></span>
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
<span data-ttu-id="d76e2-154">Ez a parancs nem törli a hello tárolószolgáltatás létrehozása során létrehozott összes erőforrás (hálózati és tárolási).</span><span class="sxs-lookup"><span data-stu-id="d76e2-154">This command does not delete all resources (network and storage) created while creating hello container service.</span></span> <span data-ttu-id="d76e2-155">toodelete összes erőforrás könnyen, ajánlott minden fürt eltérő erőforráscsoportban telepít.</span><span class="sxs-lookup"><span data-stu-id="d76e2-155">toodelete all resources easily, it is recommended you deploy each cluster in a distinct resource group.</span></span> <span data-ttu-id="d76e2-156">Ezután törölni hello erőforráscsoport hello fürt már nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="d76e2-156">Then, delete hello resource group when hello cluster is no longer required.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d76e2-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d76e2-157">Next steps</span></span>
<span data-ttu-id="d76e2-158">Most, hogy működő fürtje van, tekintse meg ezeket a dokumentumokat a kapcsolatra és a felügyeletre vonatkozó részletekért:</span><span class="sxs-lookup"><span data-stu-id="d76e2-158">Now that you have a functioning cluster, see these documents for connection and management details:</span></span>

* [<span data-ttu-id="d76e2-159">Csatlakozás Azure Tárolószolgáltatás-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="d76e2-159">Connect tooan Azure Container Service cluster</span></span>](../container-service-connect.md)
* [<span data-ttu-id="d76e2-160">Az Azure Container Service és a DC/OS használata</span><span class="sxs-lookup"><span data-stu-id="d76e2-160">Work with Azure Container Service and DC/OS</span></span>](container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="d76e2-161">Az Azure Container Service és a Docker Swarm használata</span><span class="sxs-lookup"><span data-stu-id="d76e2-161">Work with Azure Container Service and Docker Swarm</span></span>](container-service-docker-swarm.md)
* [<span data-ttu-id="d76e2-162">Az Azure Container Service és a Kubernetes használata</span><span class="sxs-lookup"><span data-stu-id="d76e2-162">Work with Azure Container Service and Kubernetes</span></span>](../kubernetes/container-service-kubernetes-walkthrough.md)