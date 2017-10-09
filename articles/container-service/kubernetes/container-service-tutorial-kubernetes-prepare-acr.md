---
title: "aaaAzure Tárolószolgáltatás oktatóanyag – ACR előkészítése |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató - ACR előkészítése"
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
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3980e5ce4eb9836f83c761a2f76c944bb3f13060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Üzembe helyezés és használat Azure tároló beállításjegyzék

Az Azure tároló beállításjegyzék (ACR) egy Azure-alapú, személyes beállításjegyzék Docker-tároló lemezképek. Ez az oktatóanyag, része két hét, végigvezeti Azure tároló beállításjegyzék-példány telepítését, valamint egy tároló kép tooit kérdez le. Befejeződött a lépések az alábbiak:

> [!div class="checklist"]
> * Egy Azure tároló beállításjegyzék (ACR) példány telepítése
> * A tároló lemezkép ACR a címkézés
> * Hello kép tooACR feltöltése

A következő útmutatókból adott ACR példány integrálva van egy Azure-tároló szolgáltatás Kubernetes fürthöz történő biztonságos futtatására, tároló-lemezképeket. 

## <a name="before-you-begin"></a>Előkészületek

A hello [az oktatóanyag előző](./container-service-tutorial-kubernetes-prepare-app.md), a tároló-lemezkép létrejött egy egyszerű Azure szavazás alkalmazáshoz. Ebben az oktatóanyagban a kép tooan Azure tároló beállításjegyzék fejlesztőre. Ha nem hozott létre hello Azure szavazás alkalmazás lemezképét, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md). Alternatív megoldásként hello lépések részletes itt együttműködnek a tároló képet.

Ez az oktatóanyag megköveteli, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="deploy-azure-container-registry"></a>Telepítse az Azure tároló beállításjegyzék

Egy Azure-tároló beállításjegyzék való telepítésekor, először egy erőforráscsoportot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Ebben a példában az erőforráscsoport neve *myResourceGroup* jön létre az hello *westeurope* régióban.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Hozzon létre egy Azure-tárolóba beállításjegyzék hello [az acr létrehozása](/cli/azure/acr#create) parancsot. egy tároló beállításjegyzék hello neve **egyedinek kell lennie**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

Ez az oktatóanyag hello részeinek, egész használjuk "acrname" helyőrzőként a hello beállításjegyzék Tárolónév választott.

## <a name="container-registry-login"></a>Tároló beállításjegyzék bejelentkezés

Be kell jelentkeznie tooyour ACR példány képek tooit előtt. Használjon hello [az acr bejelentkezési](https://docs.microsoft.com/en-us/cli/azure/acr#login) toocomplete hello művelet parancsot. Tooprovide hello egyedi adott név toohello tároló beállításjegyzék létrehozásakor van szüksége.

```azurecli
az acr login --name <acrName>
```

hello parancs visszaadja a "Sikeres bejelentkezés" üzenet, amint befejeződött.

## <a name="tag-container-images"></a>Címke tároló lemezképek

Minden egyes tároló kép hello beállításjegyzék hello loginServer nevű címkézett toobe kell. Ezt a címkét használható útválasztási Ha tároló képek tooan kép beállításjegyzék végez leküldést.

az aktuális képek, használjon hello listáját toosee [docker képek](https://docs.docker.com/engine/reference/commandline/images/) parancs.

```bash
docker images
```

Kimenet:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

tooget hello loginServer neve, futtassa a következő parancs hello.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Most, címke hello *azure-szavazat-front* hello loginServer hello tároló beállításjegyzék lemezkép. Továbbá adja hozzá `:redis-v1` hello lemezképnév toohello végét. Ezt a címkét hello lemezkép verziója jelzi.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

Miután címkézett, futtassa az [docker képek] (https://docs.docker.com/engine/reference/commandline/images/) tooverify hello műveletet.

```bash
docker images
```

Kimenet:

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-tooregistry"></a>Leküldéses képek tooregistry

Hello leküldéses *azure-szavazat-front* kép toohello beállításjegyzék. 

Hello ACR loginServer neve használja a következő példa hello, cserélje le a környezetből hello loginServer.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

Néhány perc toocomplete jut.

## <a name="list-images-in-registry"></a>A beállításjegyzékben lemezképek felsorolása

tooreturn tooyour Azure-tárolóba beállításjegyzék felhasználói hello értesítését lemezképeket listája [az acr tárház lista](/cli/azure/acr/repository#list) parancsot. Frissítse a hello parancs hello ACR neve.

```azurecli
az acr repository list --name <acrName> --output table
```

Kimenet:

```azurecli
Result
----------------
azure-vote-front
```

Majd egy adott lemezkép toosee hello címkék hello [az acr tárház megjelenítése-címkék](/cli/azure/acr/repository#show-tags) parancsot.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Kimenet:

```azurecli
Result
--------
redis-v1
```

Oktatóanyag befejezése után hello tároló kép rendelkezik lett tárolja, a saját Azure tároló beállításjegyzék-példányban. Ez a kép a következő útmutatókból ACR tooa Kubernetes fürtről van telepítve.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban az Azure-tároló beállításjegyzék ACS Kubernetes fürt használatra lett előkészítve. befejeződtek a hello a következő lépéseket:

> [!div class="checklist"]
> * Egy Azure-tároló beállításjegyzék-példány telepítése
> * A tároló-lemezkép az ACR címkézett
> * Feltöltött hello kép tooACR

Az Azure-ban Kubernetes fürt telepítésével kapcsolatos útmutató toolearn következő toohello előzetes.

> [!div class="nextstepaction"]
> [Kubernetes fürt központi telepítése](./container-service-tutorial-kubernetes-deploy-cluster.md)