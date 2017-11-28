---
title: "Rövid útmutató – Azure Docker Swarm-fürt létrehozása Linux rendszeren | Microsoft Docs"
description: "Ebből a rövid útmutatóból megtudhatja, hogyan hozhat létre az Azure CLI segítségével Docker Swarm-fürtöt Linux-tárolókhoz az Azure Container Service-ben."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, Docker, Swarm
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/14/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 1d10c347795227ed056a95d1bcd4aff82af7b876
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-docker-swarm-cluster"></a><span data-ttu-id="b9bc3-103">Docker Swarm-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="b9bc3-103">Deploy Docker Swarm cluster</span></span>

<span data-ttu-id="b9bc3-104">Ebben a rövid útmutatóban egy Docker Swarm-fürtöt helyezünk üzembe az Azure CLI-vel.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-104">In this quick start, a Docker Swarm cluster is deployed using the Azure CLI.</span></span> <span data-ttu-id="b9bc3-105">Ezután egy webes előtérrendszert és egy Redis-példányt magában foglaló többtárolós alkalmazást helyezünk üzembe és futtatunk a fürtön.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on the cluster.</span></span> <span data-ttu-id="b9bc3-106">Miután végeztünk ezzel, az alkalmazás elérhető lesz az interneten.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-106">Once completed, the application is accessible over the internet.</span></span>

<span data-ttu-id="b9bc3-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="b9bc3-108">A rövid útmutatóhoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-108">This quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b9bc3-109">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="b9bc3-110">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b9bc3-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="b9bc3-111">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b9bc3-111">Create a resource group</span></span>

<span data-ttu-id="b9bc3-112">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-112">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="b9bc3-113">Az Azure-erőforráscsoport olyan logikai csoport, amelyben az Azure-erőforrások üzembe helyezése és kezelése zajlik.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-113">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="b9bc3-114">A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot a *westus* helyen.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-114">The following example creates a resource group named *myResourceGroup* in the *westus* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="b9bc3-115">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b9bc3-115">Output:</span></span>

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westcentralus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-docker-swarm-cluster"></a><span data-ttu-id="b9bc3-116">Docker Swarm-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9bc3-116">Create Docker Swarm cluster</span></span>

<span data-ttu-id="b9bc3-117">Docker Swarm-fürtöt az [az acs create](/cli/azure/acs#create) paranccsal hozhat létre az Azure Container Service-ben.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-117">Create a Docker Swarm cluster in Azure Container Service with the [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="b9bc3-118">A következő példa egy *mySwarmCluster* nevű fürtöt hoz létre egy Linux-főcsomóponttal és három Linux-ügyfélcsomóponttal.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-118">The following example creates a cluster named *mySwarmCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type Swarm --resource-group myResourceGroup --generate-ssh-keys
```

<span data-ttu-id="b9bc3-119">Néhány perc múlva befejeződik a parancs végrehajtása, és visszaadja a fürttel kapcsolatos adatokat JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-119">After several minutes, the command completes and returns json formatted information about the cluster.</span></span>

## <a name="connect-to-the-cluster"></a><span data-ttu-id="b9bc3-120">Csatlakozás a fürthöz</span><span class="sxs-lookup"><span data-stu-id="b9bc3-120">Connect to the cluster</span></span>

<span data-ttu-id="b9bc3-121">A rövid útmutató során szükség lesz a Docker Swarm-főkiszolgáló és a Docker-ügynökkészlet IP-címére.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-121">Throughout this quick start, you need the IP address of both the Docker Swarm master and the Docker agent pool.</span></span> <span data-ttu-id="b9bc3-122">Futtassa az alábbi parancsot a két IP-cím lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-122">Run the following command to return both IP addresses.</span></span>


```bash
az network public-ip list --resource-group myResourceGroup --query '[*].{Name:name,IPAddress:ipAddress}' -o table
```

<span data-ttu-id="b9bc3-123">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b9bc3-123">Output:</span></span>

```bash
Name                                                                 IPAddress
-------------------------------------------------------------------  -------------
swarmm-agent-ip-myswarmcluster-myresourcegroup-d5b9d4agent-66066781  52.179.23.131
swarmm-master-ip-myswarmcluster-myresourcegroup-d5b9d4mgmt-66066781  52.141.37.199
```

<span data-ttu-id="b9bc3-124">Hozzon létre egy SSH-alagutat a Swarm-főkiszolgáló felé.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-124">Create an SSH tunnel to the Swarm master.</span></span> <span data-ttu-id="b9bc3-125">Az `IPAddress` elemet cserélje le a Swarm-főkiszolgáló IP-címére.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-125">Replace `IPAddress` with the IP address of the Swarm master.</span></span>

```bash
ssh -p 2200 -fNL 2375:localhost:2375 azureuser@IPAddress
```

<span data-ttu-id="b9bc3-126">Adja meg a `DOCKER_HOST` környezeti változót.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-126">Set the `DOCKER_HOST` environment variable.</span></span> <span data-ttu-id="b9bc3-127">Ez lehetővé teszi a Docker Swarmra irányuló Docker-parancsok futtatását anélkül, hogy meg kellene adni a gazdagép nevét.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-127">This allows you to run docker commands against the Docker Swarm without having to specify the name of the host.</span></span>

```bash
export DOCKER_HOST=:2375
```

<span data-ttu-id="b9bc3-128">Most már készen áll a Docker-szolgáltatások futtatására a Docker Swarmon.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-128">You are now ready to run Docker services on the Docker Swarm.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="b9bc3-129">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="b9bc3-129">Run the application</span></span>

<span data-ttu-id="b9bc3-130">Hozzon létre egy `docker-compose.yaml` nevű fájlt, és másolja az alábbi tartalmat a fájlba.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-130">Create a file named `docker-compose.yaml` and copy the following content into it.</span></span>

```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    container_name: azure-vote-back
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    container_name: azure-vote-front
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

<span data-ttu-id="b9bc3-131">Futtassa az alábbi parancsot az Azure Vote-szolgáltatás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-131">Run the following command to create the Azure Vote service.</span></span>

```bash
docker-compose up -d
```

<span data-ttu-id="b9bc3-132">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b9bc3-132">Output:</span></span>

```bash
Creating network "user_default" with the default driver
Pulling azure-vote-front (microsoft/azure-vote-front:redis-v1)...
swarm-agent-EE873B23000005: Pulling microsoft/azure-vote-front:redis-v1...
swarm-agent-EE873B23000004: Pulling microsoft/azure-vote-front:redis-v1... : downloaded
Pulling azure-vote-back (redis:latest)...
swarm-agent-EE873B23000004: Pulling redis:latest... : downloaded
Creating azure-vote-front ... 
Creating azure-vote-back ... 
Creating azure-vote-front
Creating azure-vote-back ...
```

## <a name="test-the-application"></a><span data-ttu-id="b9bc3-133">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="b9bc3-133">Test the application</span></span>

<span data-ttu-id="b9bc3-134">Lépjen a Swarm-ügynökkészlet IP-címére az Azure Vote-alkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-134">Browse to the IP address of the Swarm agent pool to test out the Azure Vote application.</span></span>

![Az Azure Vote keresését ábrázoló kép](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a><span data-ttu-id="b9bc3-136">Fürt törlése</span><span class="sxs-lookup"><span data-stu-id="b9bc3-136">Delete cluster</span></span>
<span data-ttu-id="b9bc3-137">Ha a fürtre már nincs szükség, az [az group delete](/cli/azure/group#delete) paranccsal törölheti az erőforráscsoportot, a tárolószolgáltatást és az összes kapcsolódó erőforrást.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-137">When the cluster is no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, container service, and all related resources.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a><span data-ttu-id="b9bc3-138">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="b9bc3-138">Get the code</span></span>

<span data-ttu-id="b9bc3-139">Ebben a rövid útmutatóban előre létrehozott tárolórendszerképekkel hoztunk létre egy Docker-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-139">In this quick start, pre-created container images have been used to create a Docker service.</span></span> <span data-ttu-id="b9bc3-140">A kapcsolódó alkalmazáskód, Docker-fájl és Compose-fájl a GitHubon érhető el.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-140">The related application code, Dockerfile, and Compose file are available on GitHub.</span></span>

[<span data-ttu-id="b9bc3-141">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="b9bc3-141">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="b9bc3-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9bc3-142">Next steps</span></span>

<span data-ttu-id="b9bc3-143">Ebben a rövid útmutatóban egy Docker Swarm-fürtöt és azon egy többtárolós alkalmazást helyezett üzembe.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-143">In this quick start, you deployed a Docker Swarm cluster and deployed a multi-container application to it.</span></span>

<span data-ttu-id="b9bc3-144">A Docker Swarm és a Visual Studio Team Services integrálásával kapcsolatos információk megtekintéséhez olvassa el a CI/CD, a Docker Swarm és a VSTS használatával foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="b9bc3-144">To learn about integrating Docker warm with Visual Studio Team Services, continue to the CI/CD with Docker Swarm and VSTS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b9bc3-145">CI/CD – Docker Swarm és VSTS</span><span class="sxs-lookup"><span data-stu-id="b9bc3-145">CI/CD with Docker Swarm and VSTS</span></span>](./container-service-docker-swarm-setup-ci-cd.md)