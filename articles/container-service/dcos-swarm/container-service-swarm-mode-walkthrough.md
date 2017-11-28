---
title: "aaaQuickstart - Azure Docker CE fürt Linux |} Microsoft Docs"
description: "Ismerje meg gyorsan, toocreate Linux tárolók az Azure Tárolószolgáltatásban hello Azure parancssori felület a Docker CE fürtökben."
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
ms.openlocfilehash: 6c26c12ed085ec379c3486095a5fa51379afc5a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-docker-ce-cluster"></a><span data-ttu-id="ab646-103">Docker CE-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="ab646-103">Deploy Docker CE cluster</span></span>

<span data-ttu-id="ab646-104">A gyors üzembe helyezési Docker CE fürttagként van telepítve hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="ab646-104">In this quick start, a Docker CE cluster is deployed using hello Azure CLI.</span></span> <span data-ttu-id="ab646-105">Egy előtér-webkiszolgáló és a Redis példánya több tároló alkalmazás majd üzemel, és futtatása hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="ab646-105">A multi-container application consisting of web front end and a Redis instance is then deployed and run on hello cluster.</span></span> <span data-ttu-id="ab646-106">Ezt követően hello alkalmazás keresztül érhető el-e hello internet.</span><span class="sxs-lookup"><span data-stu-id="ab646-106">Once completed, hello application is accessible over hello internet.</span></span>

<span data-ttu-id="ab646-107">A Docker CE az Azure Container Service-ben előzetes verzióban érhető el, és **éles számítási feladatokra nem használható**.</span><span class="sxs-lookup"><span data-stu-id="ab646-107">Docker CE on Azure Container Service is in preview and **should not be used for production workloads**.</span></span>

<span data-ttu-id="ab646-108">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="ab646-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="ab646-109">Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="ab646-109">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="ab646-110">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="ab646-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ab646-111">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ab646-111">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="ab646-112">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="ab646-112">Create a resource group</span></span>

<span data-ttu-id="ab646-113">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="ab646-113">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="ab646-114">Az Azure-erőforráscsoport olyan logikai csoport, amelyben az Azure-erőforrások üzembe helyezése és kezelése zajlik.</span><span class="sxs-lookup"><span data-stu-id="ab646-114">An Azure resource group is a logical group in which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="ab646-115">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *ukwest* helyét.</span><span class="sxs-lookup"><span data-stu-id="ab646-115">hello following example creates a resource group named *myResourceGroup* in hello *ukwest* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location ukwest
```

<span data-ttu-id="ab646-116">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="ab646-116">Output:</span></span>

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

## <a name="create-docker-swarm-cluster"></a><span data-ttu-id="ab646-117">Docker Swarm-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab646-117">Create Docker Swarm cluster</span></span>

<span data-ttu-id="ab646-118">Hozzon létre egy Docker CE fürtöt az Azure Tárolószolgáltatásban hello [az acs létre](/cli/azure/acs#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="ab646-118">Create a Docker CE cluster in Azure Container Service with hello [az acs create](/cli/azure/acs#create) command.</span></span> 

<span data-ttu-id="ab646-119">hello alábbi példakód létrehozza a fürt nevű *mySwarmCluster* egy Linux fő csomópont- és Linux-ügynök három csomópontot.</span><span class="sxs-lookup"><span data-stu-id="ab646-119">hello following example creates a cluster named *mySwarmCluster* with one Linux master node and three Linux agent nodes.</span></span>

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type dockerce --resource-group myResourceGroup --generate-ssh-keys
```

<span data-ttu-id="ab646-120">Pár perc múlva hello parancs befejeződött, és hello fürt json formátumú információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="ab646-120">After several minutes, hello command completes and returns json formatted information about hello cluster.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="ab646-121">Csatlakoztassa toohello fürtöt</span><span class="sxs-lookup"><span data-stu-id="ab646-121">Connect toohello cluster</span></span>

<span data-ttu-id="ab646-122">A gyors üzembe helyezési teljes kell hello hello Docker Swarm fő és a Docker-ügynök készlet hello teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="ab646-122">Throughout this quick start, you need hello FQDN of both hello Docker Swarm master and hello Docker agent pool.</span></span> <span data-ttu-id="ab646-123">Futtassa a következő parancs tooreturn hello mindkét hello főkiszolgáló-ügynök teljes tartományneveit.</span><span class="sxs-lookup"><span data-stu-id="ab646-123">Run hello following command tooreturn both hello master and agent FQDNs.</span></span>


```bash
az acs list --resource-group myResourceGroup --query '[*].{Master:masterProfile.fqdn,Agent:agentPoolProfiles[0].fqdn}' -o table
```

<span data-ttu-id="ab646-124">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="ab646-124">Output:</span></span>

```bash
Master                                                               Agent
-------------------------------------------------------------------  --------------------------------------------------------------------
myswarmcluster-myresourcegroup-d5b9d4mgmt.ukwest.cloudapp.azure.com  myswarmcluster-myresourcegroup-d5b9d4agent.ukwest.cloudapp.azure.com
```

<span data-ttu-id="ab646-125">Hozzon létre egy SSH alagút toohello Swarm fő.</span><span class="sxs-lookup"><span data-stu-id="ab646-125">Create an SSH tunnel toohello Swarm master.</span></span> <span data-ttu-id="ab646-126">Cserélje le `MasterFQDN` hello Swarm főkiszolgáló hello FQDN címmel.</span><span class="sxs-lookup"><span data-stu-id="ab646-126">Replace `MasterFQDN` with hello FQDN address of hello Swarm master.</span></span>

```bash
ssh -p 2200 -fNL localhost:2374:/var/run/docker.sock azureuser@MasterFQDN
```

<span data-ttu-id="ab646-127">Set hello `DOCKER_HOST` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="ab646-127">Set hello `DOCKER_HOST` environment variable.</span></span> <span data-ttu-id="ab646-128">Ez lehetővé teszi toorun docker parancsok elleni Docker Swarm hello hello gazdagép toospecify hello neve nélkül.</span><span class="sxs-lookup"><span data-stu-id="ab646-128">This allows you toorun docker commands against hello Docker Swarm without having toospecify hello name of hello host.</span></span>

```bash
export DOCKER_HOST=localhost:2374
```

<span data-ttu-id="ab646-129">Most már áll készen toorun Docker-szolgáltatásokat a Docker Swarm hello.</span><span class="sxs-lookup"><span data-stu-id="ab646-129">You are now ready toorun Docker services on hello Docker Swarm.</span></span>


## <a name="run-hello-application"></a><span data-ttu-id="ab646-130">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="ab646-130">Run hello application</span></span>

<span data-ttu-id="ab646-131">Hozzon létre egy fájlt `azure-vote.yaml` és a következő tartalom másolása hello bele.</span><span class="sxs-lookup"><span data-stu-id="ab646-131">Create a file named `azure-vote.yaml` and copy hello following content into it.</span></span>


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

<span data-ttu-id="ab646-132">Futtassa a hello [docker verem telepítése](https://docs.docker.com/engine/reference/commandline/stack_deploy/) toocreate hello Azure szavazattal szolgáltatás parancsot.</span><span class="sxs-lookup"><span data-stu-id="ab646-132">Run hello [docker stack deploy](https://docs.docker.com/engine/reference/commandline/stack_deploy/) command toocreate hello Azure Vote service.</span></span>

```bash
docker stack deploy azure-vote --compose-file azure-vote.yaml
```

<span data-ttu-id="ab646-133">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="ab646-133">Output:</span></span>

```bash
Creating network azure-vote_default
Creating service azure-vote_azure-vote-back
Creating service azure-vote_azure-vote-front
```

<span data-ttu-id="ab646-134">Használjon hello [docker verem ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) tooreturn hello központi telepítési állapotának hello alkalmazás parancsot.</span><span class="sxs-lookup"><span data-stu-id="ab646-134">Use hello [docker stack ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) command tooreturn hello deployment status of hello application.</span></span>

```bash
docker stack ps azure-vote
```

<span data-ttu-id="ab646-135">Egyszer hello `CURRENT STATE` minden szolgáltatás van `Running`, hello alkalmazás készen áll.</span><span class="sxs-lookup"><span data-stu-id="ab646-135">Once hello `CURRENT STATE` of each service is `Running`, hello application is ready.</span></span>

```bash
ID                  NAME                            IMAGE                                 NODE                               DESIRED STATE       CURRENT STATE                ERROR               PORTS
tnklkv3ogu3i        azure-vote_azure-vote-front.1   microsoft/azure-vote-front:redis-v1   swarmm-agentpool0-66066781000004   Running             Running 5 seconds ago                            
lg99i4hy68r9        azure-vote_azure-vote-back.1    redis:latest                          swarmm-agentpool0-66066781000002   Running             Running about a minute ago
```

## <a name="test-hello-application"></a><span data-ttu-id="ab646-136">Hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="ab646-136">Test hello application</span></span>

<span data-ttu-id="ab646-137">Keresse meg a toohello hello Swarm ügynök készlet tootest kimenő hello Azure szavazattal alkalmazás teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="ab646-137">Browse toohello FQDN of hello Swarm agent pool tootest out hello Azure Vote application.</span></span>

![Böngészés tooAzure szavazattal képe](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a><span data-ttu-id="ab646-139">Fürt törlése</span><span class="sxs-lookup"><span data-stu-id="ab646-139">Delete cluster</span></span>
<span data-ttu-id="ab646-140">Amikor hello fürt már nem szükséges, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, a tárolószolgáltatás és a minden kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="ab646-140">When hello cluster is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, container service, and all related resources.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a><span data-ttu-id="ab646-141">Hello kód beolvasása</span><span class="sxs-lookup"><span data-stu-id="ab646-141">Get hello code</span></span>

<span data-ttu-id="ab646-142">A gyors üzembe helyezési az előre létrehozott tároló képek használt toocreate egy Docker-szolgáltatás volt.</span><span class="sxs-lookup"><span data-stu-id="ab646-142">In this quick start, pre-created container images have been used toocreate a Docker service.</span></span> <span data-ttu-id="ab646-143">hello alkalmazáskód, Dockerfile, és a kapcsolódó új fájlt a Githubon érhetők el.</span><span class="sxs-lookup"><span data-stu-id="ab646-143">hello related application code, Dockerfile, and Compose file are available on GitHub.</span></span>

[<span data-ttu-id="ab646-144">https://github.com/Azure-Samples/azure-voting-app-redis</span><span class="sxs-lookup"><span data-stu-id="ab646-144">https://github.com/Azure-Samples/azure-voting-app-redis</span></span>](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a><span data-ttu-id="ab646-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab646-145">Next steps</span></span>

<span data-ttu-id="ab646-146">A gyors üzembe helyezési a Docker Swarm-fürt telepítése, és a tároló több alkalmazás tooit telepítve.</span><span class="sxs-lookup"><span data-stu-id="ab646-146">In this quick start, you deployed a Docker Swarm cluster and deployed a multi-container application tooit.</span></span>

<span data-ttu-id="ab646-147">meleg Docker integrálása a Visual Studio Team Services, kapcsolatos toolearn toohello CI/CD Docker Swarm és VSTS továbbra is.</span><span class="sxs-lookup"><span data-stu-id="ab646-147">toolearn about integrating Docker warm with Visual Studio Team Services, continue toohello CI/CD with Docker Swarm and VSTS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ab646-148">CI/CD – Docker Swarm és VSTS</span><span class="sxs-lookup"><span data-stu-id="ab646-148">CI/CD with Docker Swarm and VSTS</span></span>](./container-service-docker-swarm-setup-ci-cd.md)