---
title: "aaaAzure tároló példányok oktatóanyag – Azure tároló beállításjegyzék előkészítése |} Microsoft Docs"
description: "Azure tároló példányok útmutató – Azure tároló beállításjegyzék előkészítése"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2525626125740c3c861fad36aad207d0b587ff54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Üzembe helyezés és használat Azure tároló beállításjegyzék

Ez az egy háromrészes oktatóanyag második rész. A hello [előző lépésben](./container-instances-tutorial-prepare-app.md), a tároló-lemezkép létrejött egy egyszerű webes alkalmazáshoz írt [Node.js](http://nodejs.org). Ebben az oktatóanyagban a kép tooan Azure tároló beállításjegyzék fejlesztőre. Hello tároló kép nem hozott létre, ha vissza túl[oktatóanyag 1 – tároló kép létrehozása](./container-instances-tutorial-prepare-app.md). 

hello Azure tároló beállításjegyzék egy Azure-alapú, személyes beállításjegyzék Docker-tároló lemezképek. Ez az oktatóanyag végigvezeti Azure tároló beállításjegyzék-példány telepítését, valamint egy tároló kép tooit kérdez le. Befejeződött a lépések az alábbiak:

> [!div class="checklist"]
> * Egy Azure-tároló beállításjegyzék-példány telepítése
> * Azure-tároló beállításjegyzék címkézési tároló képe
> * Kép tooAzure tároló beállításjegyzék feltöltése

A következő útmutatókból hello tároló a személyes beállításjegyzék tooAzure tároló példányok a telepítése.

## <a name="before-you-begin"></a>Előkészületek

Ez az oktatóanyag megköveteli, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

## <a name="deploy-azure-container-registry"></a>Telepítse az Azure tároló beállításjegyzék

Egy Azure-tároló beállításjegyzék való telepítésekor, először egy erőforráscsoportot. Egy Azure-erőforráscsoportot gyűjteményei logikai mely Azure az erőforrások telepítése és kezelése.

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Ebben a példában az erőforráscsoport neve *myResourceGroup* jön létre az hello *eastus* régióban.

```azurecli
az group create --name myResourceGroup --location eastus
```

Hozzon létre egy Azure-tárolóba beállításjegyzék hello [az acr létrehozása](/cli/azure/acr#create) parancsot. egy tároló beállításjegyzék hello neve **egyedinek kell lennie**. A következő példa hello, hello nevét használjuk *mycontainerregistry082*.

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Ebben az oktatóanyagban hello részeinek, egész használjuk `<acrname>` a hello beállításjegyzék Tárolónév választott helyőrzőként.

## <a name="container-registry-login"></a>Tároló beállításjegyzék bejelentkezés

Be kell jelentkeznie tooyour ACR példány képek tooit előtt. Használjon hello [az acr bejelentkezési](https://docs.microsoft.com/en-us/cli/azure/acr#login) toocomplete hello művelet parancsot. Tooprovide hello egyedi adott név toohello tároló beállításjegyzék létrehozásakor van szüksége.

```azurecli
az acr login --name <acrName>
```

hello parancs visszaadja a "Sikeres bejelentkezés" üzenet, amint befejeződött.

## <a name="tag-container-image"></a>Címke tároló kép

a tároló lemezkép titkos beállításjegyzékből toodeploy, hello lemezképet kell hello címkéjű toobe `loginServer` hello beállításjegyzék neve.

az aktuális képek, használjon hello listáját toosee `docker images` parancsot.

```bash
docker images
```

Kimenet:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

tooget hello loginServer neve, futtassa a következő parancs hello.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Címke hello *aci-oktatóanyag – alkalmazás* hello loginServer hello tároló beállításjegyzék lemezkép. Továbbá adja hozzá `:v1` hello lemezképnév toohello végét. Ezt a címkét azt jelzi, hogy hello lemezkép verziószámát.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Miután megjelölve, futtassa az `docker images` tooverify hello műveletet.

```bash
docker images
```

Kimenet:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-tooazure-container-registry"></a>Leküldéses kép tooAzure tároló beállításjegyzék

Hello leküldéses *aci-oktatóanyag – alkalmazás* kép toohello beállításjegyzék.

Hello Tárolónév beállításjegyzék loginServer használja a következő példa hello, cserélje le a környezetből hello loginServer.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a>Azure-tároló beállításjegyzék lemezképek felsorolása

tooreturn tooyour Azure-tárolóba beállításjegyzék felhasználói hello értesítését lemezképeket listája [az acr tárház lista](/cli/azure/acr/repository#list) parancsot. Hello parancs hello Tárolónév beállításjegyzék frissítése.

```azurecli
az acr repository list --name <acrName> --output table
```

Kimenet:

```azurecli
Result
----------------
aci-tutorial-app
```

Majd egy adott lemezkép toosee hello címkék hello [az acr tárház megjelenítése-címkék](/cli/azure/acr/repository#show-tags) parancsot.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Kimenet:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Következő lépések

Az oktatóanyag egy Azure-tároló beállításjegyzék Azure tároló példányok való használatra készült, és hello tároló kép leküldött volt. befejeződtek a hello a következő lépéseket:

> [!div class="checklist"]
> * Egy Azure-tároló beállításjegyzék-példány telepítése
> * Azure-tároló beállításjegyzék címkézési tároló képe
> * Kép tooAzure tároló beállításjegyzék feltöltése

Előzetes toohello oktatóanyag következő toolearn hello tároló tooAzure Azure tároló példányok használatával telepítéséről.

> [!div class="nextstepaction"]
> [Tárolók tooAzure tároló példányok telepítése](./container-instances-tutorial-deploy-app.md)
