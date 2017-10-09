---
title: "aaaQuickstart - Azure Docker Swarm-fürt Linux |} Microsoft Docs"
description: "Ismerje meg gyorsan, toocreate a Docker Swarm-fürt Linux tárolók az Azure Tárolószolgáltatásban hello Azure parancssori felület a."
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
ms.openlocfilehash: 3028d2d00585360ec163518bf98f69bb0dd44dec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-docker-swarm-cluster"></a>Docker Swarm-fürt üzembe helyezése

A gyors üzembe helyezési a Docker Swarm-fürt van telepítve hello Azure parancssori felület használatával. Egy előtér-webkiszolgáló és a Redis példánya több tároló alkalmazás majd üzemel, és futtatása hello fürtön. Ezt követően hello alkalmazás keresztül érhető el-e hello internet.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

A gyors üzembe helyezés megköveteli, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai csoport, amelyben az Azure-erőforrások üzembe helyezése és kezelése zajlik.

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westus* helyét.

```azurecli-interactive
az group create --name myResourceGroup --location westus
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

Hozzon létre egy Docker Swarm fürtöt az Azure Tárolószolgáltatásban hello [az acs létre](/cli/azure/acs#create) parancsot. 

hello alábbi példakód létrehozza a fürt nevű *mySwarmCluster* egy Linux fő csomópont- és Linux-ügynök három csomópontot.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type Swarm --resource-group myResourceGroup --generate-ssh-keys
```

Pár perc múlva hello parancs befejeződött, és hello fürt json formátumú információt ad vissza.

## <a name="connect-toohello-cluster"></a>Csatlakoztassa toohello fürtöt

A gyors üzembe helyezési teljes kell hello Docker Swarm fő és a Docker-ügynök készlet hello hello IP-címét. Futtassa a következő parancs tooreturn hello mindkét IP-címeket.


```bash
az network public-ip list --resource-group myResourceGroup --query '[*].{Name:name,IPAddress:ipAddress}' -o table
```

Kimenet:

```bash
Name                                                                 IPAddress
-------------------------------------------------------------------  -------------
swarmm-agent-ip-myswarmcluster-myresourcegroup-d5b9d4agent-66066781  52.179.23.131
swarmm-master-ip-myswarmcluster-myresourcegroup-d5b9d4mgmt-66066781  52.141.37.199
```

Hozzon létre egy SSH alagút toohello Swarm fő. Cserélje le `IPAddress` hello Swarm főkiszolgáló hello IP-címmel.

```bash
ssh -p 2200 -fNL 2375:localhost:2375 azureuser@IPAddress
```

Set hello `DOCKER_HOST` környezeti változó. Ez lehetővé teszi toorun docker parancsok elleni Docker Swarm hello hello gazdagép toospecify hello neve nélkül.

```bash
export DOCKER_HOST=:2375
```

Most már áll készen toorun Docker-szolgáltatásokat a Docker Swarm hello.


## <a name="run-hello-application"></a>Hello alkalmazás futtatása

Hozzon létre egy fájlt `docker-compose.yaml` és a következő tartalom másolása hello bele.

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

Futtassa a következő parancs toocreate hello Azure szavazattal szolgáltatás hello.

```bash
docker-compose up -d
```

Kimenet:

```bash
Creating network "user_default" with hello default driver
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

## <a name="test-hello-application"></a>Hello alkalmazás tesztelése

Keresse meg a hello Swarm ügynök készlet tootest hello Azure szavazattal alkalmazás kimenő toohello IP-címét.

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