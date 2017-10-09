---
title: "aaaAzure tároló példányok útmutató – az alkalmazás előkészítése |} Az Azure Docs"
description: "Egy alkalmazás központi telepítési tooAzure tároló példányok előkészítése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 406ba796e5fefb1527f2e894cc3f7bbd8f7a5fd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-for-deployment-tooazure-container-instances"></a>A központi telepítés tooAzure tároló példányok tároló létrehozása

Az Azure Container Instances lehetővé teszi Docker-tárolók üzembe helyezését az Azure-infrastruktúrán anélkül, hogy ehhez virtuális gépeket kellene kiépítenie vagy magasabb szintű szolgáltatást kellene alkalmaznia. Jelen oktatóanyagban egy egyszerű webalkalmazást hozhat létre a Node.js szolgáltatásban, majd becsomagolhatja azt egy, az Azure Container Instances használatával futtatható tárolóba. Az oktatóanyag a következőket ismerteti:

> [!div class="checklist"]
> * Alkalmazás forrásának klónozása a GitHubról  
> * Tárolórendszerképek létrehozása az alkalmazás forrásából
> * Tesztelési hello lemezképet egy helyi Docker-környezetben

A következő útmutatókból meg fogja feltölteni a kép tooan Azure tároló beállításjegyzék, és telepíteni azt tooAzure tároló példányok.

## <a name="before-you-begin"></a>Előkészületek

Az oktatóanyag feltételezi, hogy rendelkezik a Docker fő fogalmaira, például a tárolókra, tárolórendszerképekre és az alapszintű Docker-parancsokra vonatkozó alapvető ismeretekkel. Amennyiben szükséges, tekintse meg a tárolók alapfogalmainak ismertetését a [Bevezetés a Docker használatába]( https://docs.docker.com/get-started/) című cikkben. 

toocomplete ebben az oktatóanyagban egy Docker környezetre van szükség. A Docker csomagokat biztosít, amelyekkel a Docker egyszerűen konfigurálható bármely [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) vagy [Linux](https://docs.docker.com/engine/installation/#supported-platforms) rendszeren.

## <a name="get-application-code"></a>Az alkalmazáskód letöltése

Ebben az oktatóanyagban hello minta tartalmaz egy egyszerű webalkalmazást a beépített [Node.js](http://nodejs.org). hello alkalmazás szolgálja ki a statikus HTML-lapot, és néz ki:

![Az oktatóanyag alkalmazása böngészőben megjelenítve][aci-tutorial-app]

Git toodownload hello mintát használja:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-hello-container-image"></a>Hello tároló lemezkép

hello hello minta tárházban megadott Dockerfile bemutatja, hogyan hello tárolóra épül. Elindítja a egy [hivatalos Node.js kép] [ dockerhub-nodeimage] alapján [Alpine Linux](https://alpinelinux.org/), egy kis terjesztési, amely kiválóan alkalmas toouse tárolókhoz. Majd hello alkalmazásfájlok hello tárolóba másolja, függőségek hello csomópont Package Manager használatával telepíti, és végül a hello alkalmazás elindul.

```
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Használjon hello `docker build` toocreate hello tároló képét, címkézés azt *aci-oktatóanyag – alkalmazás*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Használjon hello `docker images` toosee beépített hello lemezképet:

```bash
docker images
```

Kimenet:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-hello-container-locally"></a>Futtassa helyben a hello tároló

Mielőtt újból hello tároló tooAzure tároló példányok telepítését, futtassa helyileg tooconfirm a működését. Hello `-d` kapcsoló lehetővé teszi, hogy a hello tároló hello háttérben futnak, amíg a `-p` lehetővé teszi a toomap egy tetszőleges a számítási tooport hello tároló 80-as port.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

Nyissa meg hello böngésző toohttp://localhost:8080 tooconfirm tároló hello fut.

![Helyileg hello böngészőben futó hello alkalmazás][aci-tutorial-app-local]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban létrehozására egy tároló lemezképnek, amely lehet a telepített tooAzure tároló példányok. befejeződtek a hello a következő lépéseket:

> [!div class="checklist"]
> * A Klónozás hello alkalmazás adatforrás a Githubról  
> * Tárolórendszerképek létrehozása az alkalmazás forrásából
> * Tesztelés helyileg hello tároló

Előzetes toohello oktatóanyag következő toolearn tároló lemezképek tárolása egy Azure-tároló beállításjegyzékben.

> [!div class="nextstepaction"]
> [Leküldéses képek tooAzure tároló beállításjegyzék](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png