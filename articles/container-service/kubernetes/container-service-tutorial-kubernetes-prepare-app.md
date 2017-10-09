---
title: "aaaAzure Tárolószolgáltatás útmutató – az alkalmazás előkészítése |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató – az alkalmazás előkészítése"
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
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b537ecc9ff50358fb65b128bfe6eb894dd088cc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-images-toobe-used-with-azure-container-service"></a>Tároló képek toobe együtt az Azure Tárolószolgáltatás létrehozása

Ebben az oktatóanyagban az első rész hét, a tárolót több alkalmazás Kubernetes használatra kész. Befejeződött a lépések az alábbiak:  

> [!div class="checklist"]
> * Alkalmazás forrásának klónozása a GitHubról  
> * Hello alkalmazás forrásból tároló lemezkép létrehozása
> * Helyi Docker környezetben hello alkalmazás tesztelése

Ezt követően a helyi fejlesztési környezetben a következő alkalmazás hello érhető el.

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/azure-vote.png)

A következő útmutatókból hello tároló kép feltöltött tooan Azure tároló beállításkulcs, és ezután futtassa az Azure-ban üzemeltetett Kubernetes fürt.

## <a name="before-you-begin"></a>Előkészületek

Az oktatóanyag feltételezi, hogy rendelkezik a Docker fő fogalmaira, például a tárolókra, tárolórendszerképekre és az alapszintű Docker-parancsokra vonatkozó alapvető ismeretekkel. Amennyiben szükséges, tekintse meg a tárolók alapfogalmainak ismertetését a [Bevezetés a Docker használatába]( https://docs.docker.com/get-started/) című cikkben. 

toocomplete ebben az oktatóanyagban egy Docker környezetre van szükség. A Docker csomagokat biztosít, amelyekkel a Docker egyszerűen konfigurálható bármely [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) vagy [Linux](https://docs.docker.com/engine/installation/#supported-platforms) rendszeren.

## <a name="get-application-code"></a>Az alkalmazáskód letöltése

Ebben az oktatóanyagban használt hello mintaalkalmazás egy alapszintű szavazó alkalmazást. hello alkalmazás áll egy előtér-webkiszolgáló és egy háttér-Redis-példányt. hello webszolgáltatás összetevője egy egyéni tároló lemezképpel lesz csomagolva. hello Redis példány Docker hubról változatlan kép használja.  

Git toodownload hello alkalmazás tooyour fejlesztési környezet egy példányát használja.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Belső hello klónozott directory hello alkalmazás forráskódjához, egy előre létrehozott Docker compose fájlt, és Kubernetes jegyzékfájlt. Ezeket a fájlokat olyan használt toocreate eszközök hello oktatóanyag beállítása során. 

## <a name="create-container-images"></a>Tároló képek létrehozása

[Docker Compose](https://docs.docker.com/compose/) lehet tooautomate hello build tároló képek és hello alkalmazások központi telepítését figyelemmel több tárolót használja.

Futtassa hello docker-compose.yml fájlt toocreate hello tároló kép, letöltési hello Redis lemezképet, és hello alkalmazás indításához.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up -d
```

Befejezése után használja hello [docker képek](https://docs.docker.com/engine/reference/commandline/images/) toosee hello létrehozott lemezképek parancsot.

```bash
docker images
```

Figyelje meg, hogy három képek letöltése vagy létrehozni. Hello *azure-szavazat-front* kép hello alkalmazást tartalmaz. Származik hello *nginx-flask* kép. a Docker Hub letöltött hello Redis kép.

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Futtassa a hello [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) parancs toosee hello futó tárolók.

```bash
docker ps
```

Kimenet:

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Alkalmazás helyi teszteléséhez

Keresse meg a toohttp://localhost:8080 toosee hello alkalmazást futtat.

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Most, hogy az alkalmazás működésének ellenőrzése megtörtént, futó tárolók hello leállt, és eltávolítja. Ne törölje az hello tároló képek. Hello *azure-szavazat-front* kép hello következő oktatóanyagában feltöltött tooan Azure tároló beállításjegyzék példány.

Futtassa a következő futó tárolók toostop hello hello.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml stop
```

A következő parancs hello leállt hello tárolók törölnie.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml rm
```

A művelet befejezését, a tároló lemezkép hello Azure szavazattal alkalmazást tartalmazó rendelkezik.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy alkalmazás teszteltük és hello alkalmazáshoz létrehozott tároló képek. befejeződtek a hello a következő lépéseket:

> [!div class="checklist"]
> * A Klónozás hello alkalmazás adatforrás a Githubról  
> * A tároló lemezkép létrehozott alkalmazás adatforrás
> * Helyi Docker környezetben tesztelt hello alkalmazás

Előzetes toohello oktatóanyag következő toolearn tároló lemezképek tárolása egy Azure-tároló beállításjegyzékben.

> [!div class="nextstepaction"]
> [Leküldéses képek tooAzure tároló beállításjegyzék](./container-service-tutorial-kubernetes-prepare-acr.md)
