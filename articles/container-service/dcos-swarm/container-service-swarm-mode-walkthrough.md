---
title: "Rövid útmutató – Azure Docker CE-fürt létrehozása Linux rendszeren | Microsoft Docs"
description: "Ebből a rövid útmutatóból megtudhatja, hogyan hozhat létre az Azure CLI segítségével Docker CE-fürtöt Linux-tárolókhoz az Azure Container Service-ben."
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
ms.date: 08/25/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 7b8336e3865e7032e3ee0d5e4ee712bcb95aa4b5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-docker-ce-cluster"></a><span data-ttu-id="92af8-103">Docker CE-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="92af8-103">Deploy Docker CE cluster</span></span>

<span data-ttu-id="92af8-104">Ebben a rövid útmutatóban egy Docker CE-fürtöt helyezünk üzembe az Azure CLI-vel.</span><span class="sxs-lookup"><span data-stu-id="92af8-104">In this quick start, a Docker CE cluster is deployed using the Azure CLI.</span></span> <span data-ttu-id="92af8-105">Ezután egy webes előtérrendszert és egy Redis-példányt magában foglaló többtárolós alkalmazást helyezünk üzembe és futtatunk a fürtön.</span><span class="sxs-lookup"><span data-stu-id="92af8-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on the cluster.</span></span> <span data-ttu-id="92af8-106">Miután végeztünk ezzel, az alkalmazás elérhető lesz az interneten.</span><span class="sxs-lookup"><span data-stu-id="92af8-106">Once completed, the application is accessible over the internet.</span></span>

<span data-ttu-id="92af8-107">A Docker CE az Azure Container Service-ben előzetes verzióban érhető el, és **éles számítási feladatokra nem használható**.</span><span class="sxs-lookup"><span data-stu-id="92af8-107">Docker CE on Azure Container Service is in preview and **should not be used for production workloads**.</span></span>

<span data-ttu-id="92af8-108">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="92af8-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="92af8-109">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a gyorsútmutatóhoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="92af8-109">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="92af8-110">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="92af8-110">Run `az --version` to find the version.</span></span> <span data-ttu-id="92af8-111">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="92af8-111">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="92af8-112">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="92af8-112">Create a resource group</span></span>

<span data-ttu-id="92af8-113">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="92af8-113">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="92af8-114">Az Azure-erőforráscsoport olyan logikai csoport, amelyben az Azure-erőforrások üzembe helyezése és kezelése zajlik.</span><span class="sxs-lookup"><span data-stu-id="92af8-114">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="92af8-115">A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot a *ukwest* helyen.</span><span class="sxs-lookup"><span data-stu-id="92af8-115">The following example creates a resource group named *myResourceGroup* in the *ukwest* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location ukwest
```

<span data-ttu-id="92af8-116">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="92af8-116">Output:</span></span>

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

## <a name="create-docker-swarm-cluster"></a><span data-ttu-id="92af8-117">Docker Swarm-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="92af8-117">Create Docker Swarm cluster</span></span>

<span data-ttu-id="92af8-118">Az Azure Container Service-ben az [az acs create](/cli/azure/acs#create) paranccsal hozhat létre Docker CE-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="92af8-118">Create a Docker CE cluster in Azure Container Service with the [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="92af8-119">A következő példa egy *mySwarmCluster* nevű fürtöt hoz létre egy Linux-főcsomóponttal és három Linux-ügyfélcsomóponttal.</span><span class="sxs-lookup"><span data-stu-id="92af8-119">The following example creates a cluster named *mySwarmCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type dockerce --resource-group myResourceGroup --generate-ssh-keys
```

<span data-ttu-id="92af8-120">Néhány perc múlva befejeződik a parancs végrehajtása, és visszaadja a fürttel kapcsolatos adatokat JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="92af8-120">After several minutes, the command completes and returns json formatted information about the cluster.</span></span>

## <a name="connect-to-the-cluster"></a><span data-ttu-id="92af8-121">Csatlakozás a fürthöz</span><span class="sxs-lookup"><span data-stu-id="92af8-121">Connect to the cluster</span></span>

<span data-ttu-id="92af8-122">A rövid útmutató során szükség lesz a Docker Swarm-főkiszolgáló és a Docker-ügynökkészlet teljes tartománynevére.</span><span class="sxs-lookup"><span data-stu-id="92af8-122">Throughout this quick start, you need the FQDN of both the Docker Swarm master and the Docker agent pool.</span></span> <span data-ttu-id="92af8-123">Futtassa az alábbi parancsot a fő és az ügynök FQDN-ek lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="92af8-123">Run the following command to return both the master and agent FQDNs.</span></span>


```bash
az acs list --resource-group myResourceGroup --query '[*].{Master:masterProfile.fqdn,Agent:agentPoolProfiles[0].fqdn}' -o table
```

<span data-ttu-id="92af8-124">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="92af8-124">Output:</span></span>

```bash
Master                                                               Agent
-------------------------------------------------------------------  --------------------------------------------------------------------
myswarmcluster-myresourcegroup-d5b9d4mgmt.ukwest.cloudapp.azure.com  myswarmcluster-myresourcegroup-d5b9d4agent.ukwest.cloudapp.azure.com
```

<span data-ttu-id="92af8-125">Hozzon létre egy SSH-alagutat a Swarm-főkiszolgáló felé.</span><span class="sxs-lookup"><span data-stu-id="92af8-125">Create an SSH tunnel to the Swarm master.</span></span> <span data-ttu-id="92af8-126">Az `MasterFQDN` elemet cserélje le a Swarm-főkiszolgáló FQDN címére.</span><span class="sxs-lookup"><span data-stu-id="92af8-126">Replace `MasterFQDN` with the FQDN address of the Swarm master.</span></span>

```bash
ssh -p 2200 -fNL localhost:2374:/var/run/docker.sock azureuser@MasterFQDN
```

<span data-ttu-id="92af8-127">Adja meg a `DOCKER_HOST` környezeti változót.</span><span class="sxs-lookup"><span data-stu-id="92af8-127">Set the `DOCKER_HOST` environment variable.</span></span> <span data-ttu-id="92af8-128">Ez lehetővé teszi a Docker Swarmra irányuló Docker-parancsok futtatását anélkül, hogy meg kellene adni a gazdagép nevét.</span><span class="sxs-lookup"><span data-stu-id="92af8-128">This allows you to run docker commands against the Docker Swarm without having to specify the name of the host.</span></span>

```bash
export DOCKER_HOST=localhost:2374
```

<span data-ttu-id="92af8-129">Most már készen áll a Docker-szolgáltatások futtatására a Docker Swarmon.</span><span class="sxs-lookup"><span data-stu-id="92af8-129">You are now ready to run Docker services on the Docker Swarm.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="92af8-130">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="92af8-130">Run the application</span></span>

<span data-ttu-id="92af8-131">Hozzon létre egy `azure-vote.yaml` nevű fájlt, és másolja az alábbi tartalmat a fájlba.</span><span class="sxs-lookup"><span data-stu-id="92af8-131">Create a file named `azure-vote.yaml` and copy the following content into it.</span></span>


```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

<span data-ttu-id="92af8-132">Futtassa a [docker stack deploy](https://docs.docker.com/engine/reference/commandline/stack_deploy/) parancsot az Azure Vote-szolgáltatás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="92af8-132">Run the [docker stack deploy](https://docs.docker.com/engine/reference/commandline/stack_deploy/) command to create the Azure Vote service.</span></span>

```bash
docker stack deploy azure-vote --compose-file azure-vote.yaml
```

<span data-ttu-id="92af8-133">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="92af8-133">Output:</span></span>

```bash
Creating network azure-vote_default
Creating service azure-vote_azure-vote-back
Creating service azure-vote_azure-vote-front
```

<span data-ttu-id="92af8-134">Használja a [docker stack ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) parancsot, hogy visszatérjen az alkalmazás üzembe helyezési állapotához.</span><span class="sxs-lookup"><span data-stu-id="92af8-134">Use the [docker stack ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) command to return the deployment status of the application.</span></span>

```bash
docker stack ps azure-vote
```

<span data-ttu-id="92af8-135">Az alkalmazás akkor áll készen, ha az egyes szolgáltatások `CURRENT STATE` állapota `Running`.</span><span class="sxs-lookup"><span data-stu-id="92af8-135">Once the `CURRENT STATE` of each service is `Running`, the application is ready.</span></span>

```bash
ID                  NAME                            IMAGE                                 NODE                               DESIRED STATE       CURRENT STATE                ERROR               PORTS
tnklkv3ogu3i        azure-vote_azure-vote-front.1   microsoft/azure-vote-front:redis-v1   swarmm-agentpool0-66066781000004   Running             Running 5 seconds ago                            
lg99i4hy68r9        azure-vote_azure-vote-back.1    redis:latest                          swarmm-agentpool0-66066781000002   Running             Running about a minute ago
```

## <a name="test-the-application"></a><span data-ttu-id="92af8-136">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="92af8-136">Test the application</span></span>

<span data-ttu-id="92af8-137">Lépjen a Swarm-ügynökkészlet teljes tartománynevére az Azure Vote-alkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="92af8-137">Browse to the FQDN of the Swarm agent pool to test out the Azure Vote application.</span></span>

![Az Azure Vote keresését ábrázoló kép](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a><span data-ttu-id="92af8-139">Fürt törlése</span><span class="sxs-lookup"><span data-stu-id="92af8-139">Delete cluster</span></span>
<span data-ttu-id="92af8-140">Ha a fürtre már nincs szükség, az [az group delete](/cli/azure/group#delete) paranccsal törölheti az erőforráscsoportot, a tárolószolgáltatást és az összes kapcsolódó erőforrást.</span><span class="sxs-lookup"><span data-stu-id="92af8-140">When the cluster is no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, container service, and all related resources.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a><span data-ttu-id="92af8-141">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="92af8-141">Get the code</span></span>

<span data-ttu-id="92af8-142">Ebben a rövid útmutatóban előre létrehozott tárolórendszerképekkel hoztunk létre egy Docker-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="92af8-142">In this quick start, pre-created container images have been used to create a Docker service.</span></span> <span data-ttu-id="92af8-143">A kapcsolódó alkalmazáskód, Docker-fájl és Compose-fájl a GitHubon érhető el.</span><span class="sxs-lookup"><span data-stu-id="92af8-143">The related application code, Dockerfile, and Compose file are available on GitHub.</span></span>

[<span data-ttu-id="92af8-144">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="92af8-144">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="92af8-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="92af8-145">Next steps</span></span>

<span data-ttu-id="92af8-146">Ebben a rövid útmutatóban egy Docker Swarm-fürtöt és azon egy többtárolós alkalmazást helyezett üzembe.</span><span class="sxs-lookup"><span data-stu-id="92af8-146">In this quick start, you deployed a Docker Swarm cluster and deployed a multi-container application to it.</span></span>

<span data-ttu-id="92af8-147">A Docker Swarm és a Visual Studio Team Services integrálásával kapcsolatos információk megtekintéséhez olvassa el a CI/CD, a Docker Swarm és a VSTS használatával foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="92af8-147">To learn about integrating Docker warm with Visual Studio Team Services, continue to the CI/CD with Docker Swarm and VSTS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="92af8-148">CI/CD – Docker Swarm és VSTS</span><span class="sxs-lookup"><span data-stu-id="92af8-148">CI/CD with Docker Swarm and VSTS</span></span>](./container-service-docker-swarm-setup-ci-cd.md)