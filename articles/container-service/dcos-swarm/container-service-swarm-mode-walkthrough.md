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
# <a name="deploy-docker-ce-cluster"></a>Docker CE-fürt üzembe helyezése

A gyors üzembe helyezési Docker CE fürttagként van telepítve hello Azure parancssori felület használatával. Egy előtér-webkiszolgáló és a Redis példánya több tároló alkalmazás majd üzemel, és futtatása hello fürtön. Ezt követően hello alkalmazás keresztül érhető el-e hello internet.

A Docker CE az Azure Container Service-ben előzetes verzióban érhető el, és **éles számítási feladatokra nem használható**.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai csoport, amelyben az Azure-erőforrások üzembe helyezése és kezelése zajlik.

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *ukwest* helyét.

```azurecli-interactive
az group create --name myResourceGroup --location ukwest
```

Kimenet:

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

## <a name="create-docker-swarm-cluster"></a>Docker Swarm-fürt létrehozása

Hozzon létre egy Docker CE fürtöt az Azure Tárolószolgáltatásban hello [az acs létre](/cli/azure/acs#create) parancsot. 

hello alábbi példakód létrehozza a fürt nevű *mySwarmCluster* egy Linux fő csomópont- és Linux-ügynök három csomópontot.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type dockerce --resource-group myResourceGroup --generate-ssh-keys
```

Pár perc múlva hello parancs befejeződött, és hello fürt json formátumú információt ad vissza.

## <a name="connect-toohello-cluster"></a>Csatlakoztassa toohello fürtöt

A gyors üzembe helyezési teljes kell hello hello Docker Swarm fő és a Docker-ügynök készlet hello teljes Tartományneve. Futtassa a következő parancs tooreturn hello mindkét hello főkiszolgáló-ügynök teljes tartományneveit.


```bash
az acs list --resource-group myResourceGroup --query '[*].{Master:masterProfile.fqdn,Agent:agentPoolProfiles[0].fqdn}' -o table
```

Kimenet:

```bash
Master                                                               Agent
-------------------------------------------------------------------  --------------------------------------------------------------------
myswarmcluster-myresourcegroup-d5b9d4mgmt.ukwest.cloudapp.azure.com  myswarmcluster-myresourcegroup-d5b9d4agent.ukwest.cloudapp.azure.com
```

Hozzon létre egy SSH alagút toohello Swarm fő. Cserélje le `MasterFQDN` hello Swarm főkiszolgáló hello FQDN címmel.

```bash
ssh -p 2200 -fNL localhost:2374:/var/run/docker.sock azureuser@MasterFQDN
```

Set hello `DOCKER_HOST` környezeti változó. Ez lehetővé teszi toorun docker parancsok elleni Docker Swarm hello hello gazdagép toospecify hello neve nélkül.

```bash
export DOCKER_HOST=localhost:2374
```

Most már áll készen toorun Docker-szolgáltatásokat a Docker Swarm hello.


## <a name="run-hello-application"></a>Hello alkalmazás futtatása

Hozzon létre egy fájlt `azure-vote.yaml` és a következő tartalom másolása hello bele.


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

Futtassa a hello [docker verem telepítése](https://docs.docker.com/engine/reference/commandline/stack_deploy/) toocreate hello Azure szavazattal szolgáltatás parancsot.

```bash
docker stack deploy azure-vote --compose-file azure-vote.yaml
```

Kimenet:

```bash
Creating network azure-vote_default
Creating service azure-vote_azure-vote-back
Creating service azure-vote_azure-vote-front
```

Használjon hello [docker verem ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) tooreturn hello központi telepítési állapotának hello alkalmazás parancsot.

```bash
docker stack ps azure-vote
```

Egyszer hello `CURRENT STATE` minden szolgáltatás van `Running`, hello alkalmazás készen áll.

```bash
ID                  NAME                            IMAGE                                 NODE                               DESIRED STATE       CURRENT STATE                ERROR               PORTS
tnklkv3ogu3i        azure-vote_azure-vote-front.1   microsoft/azure-vote-front:redis-v1   swarmm-agentpool0-66066781000004   Running             Running 5 seconds ago                            
lg99i4hy68r9        azure-vote_azure-vote-back.1    redis:latest                          swarmm-agentpool0-66066781000002   Running             Running about a minute ago
```

## <a name="test-hello-application"></a>Hello alkalmazás tesztelése

Keresse meg a toohello hello Swarm ügynök készlet tootest kimenő hello Azure szavazattal alkalmazás teljes Tartományneve.

![Böngészés tooAzure szavazattal képe](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Fürt törlése
Amikor hello fürt már nem szükséges, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, a tárolószolgáltatás és a minden kapcsolódó erőforrások parancsot.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a>Hello kód beolvasása

A gyors üzembe helyezési az előre létrehozott tároló képek használt toocreate egy Docker-szolgáltatás volt. hello alkalmazáskód, Dockerfile, és a kapcsolódó új fájlt a Githubon érhetők el.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezési a Docker Swarm-fürt telepítése, és a tároló több alkalmazás tooit telepítve.

meleg Docker integrálása a Visual Studio Team Services, kapcsolatos toolearn toohello CI/CD Docker Swarm és VSTS továbbra is.

> [!div class="nextstepaction"]
> [CI/CD – Docker Swarm és VSTS](./container-service-docker-swarm-setup-ci-cd.md)