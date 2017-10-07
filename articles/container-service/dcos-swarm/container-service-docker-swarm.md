---
title: "Azure Swarm aaaManage Docker API-fürtöt |} Microsoft Docs"
description: "Tárolók tooa Docker Swarm-fürt üzembe az Azure Tárolószolgáltatásban"
services: container-service
documentationcenter: 
author: rgardler
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: bb9b07c82a7b48caeb2e351455797cbf2a6e7480
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="container-management-with-docker-swarm"></a>Tárolókezelés a Docker Swarmmal
A Docker Swarm olyan környezetet biztosít, amelyben tárolóalapú számítási feladatokat helyezhet üzembe egy Docker-gazdagépekből álló készletben. A docker Swarm hello natív Docker API-t használja. hello munkafolyamattal a Docker Swarmról majdnem azonos toowhat lenne, az egyetlen tároló-gazdagépen. Ez a dokumentum egyszerű példák segítségével ismerteti, hogy miként helyezhetők üzembe a tárolóalapú munkafolyamatok a Docker Swarm Azure tárolószolgáltatás-példányaiban. További részletes dokumentációt a Docker Swarmról a [ Docker.com](https://docs.docker.com/swarm/) webhelyen talál.

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

A dokumentumban szereplő toohello gyakorlatok előfeltételei:

[Swarm-fürt létrehozása az Azure Container Service-ben](container-service-deployment.md)

[Csatlakozás az Azure Tárolószolgáltatásban hello Swarm-fürthöz](../container-service-connect.md)

## <a name="deploy-a-new-container"></a>Új tároló üzembe helyezése
egy új tároló Docker Swarm, hello az toocreate hello használata `docker run` (Győződjön meg arról, hogy egy SSH-alagút toohello hello fenti előfeltételeknek megfelelően főkiszolgálók megnyitott) parancsot. Ez a példa létrehoz egy tároló hello `yeasy/simple-web` lemezképet:

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Hello tároló létrehozása után a `docker ps` hello tároló tooreturn információt. Itt figyelje meg, hogy hello Swarm ügynök hello tárolót futtató szerepel:

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Hello Swarm ügynök terheléselosztójának hello nyilvános DNS-nevén keresztül ebben a tárolóban futó alkalmazást hello érhetők el. Ez az információ hello Azure-portálon találja meg:  

![Valós látogatási eredmények](./media/container-service-docker-swarm/real-visit.jpg)  

Alapértelmezés szerint hello terheléselosztó rendelkezik portok: 80-as, 8080-as és 443 megnyitása. Ha azt szeretné, hogy egy másik porton tooconnect szüksége lesz tooopen ezt a portot, az Azure Load Balancer hello hello ügynök készlet.

## <a name="deploy-multiple-containers"></a>Több tároló üzembe helyezése
Több tároló indulnak el, futtassa a "docker Futtatás" többször, mint hello használhatja `docker ps` parancs toosee hello tárolók futtató futnak a kiszolgálón. A hello az alábbi példában három tároló egyenlően van elosztva három Swarm ügynök hello között:  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Tároló üzembe helyezése a Docker Compose-zal
A több tároló Docker Compose tooautomate hello telepítését és konfigurálását is használhatja. toodo Igen, győződjön meg arról, hogy a Secure Shell (SSH) alagút létrejött adott hello DOCKER_HOST változó (lásd a fenti hello Előfeltételek) van beállítva.

Hozzon létre egy docker-compose.yml fájlt a helyi számítógépen. toodo, ezzel [minta](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Futtatás `docker-compose up -d` toostart hello üzemelő tárolópéldányokat:

```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Végezetül hello futó tárolók listája az eredmény. Ez a lista tükrözi hello tárolók üzembe helyezése a Docker Compose használatával:

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Természetesen használhatja `docker-compose ps` tooexamine csak hello definiált tárolók a `compose.yml` fájlt.

## <a name="next-steps"></a>Következő lépések
[További információ a Docker Swarmról](https://docs.docker.com/swarm/)

