---
title: "Azure tároló példányok útmutató – Azure tároló beállításjegyzék előkészítése |} Microsoft Docs"
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
ms.openlocfilehash: cc96ba9f5abd45a7503ba3327b30e1f809391384
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Üzembe helyezés és használat Azure tároló beállításjegyzék

Ez az egy háromrészes oktatóanyag második rész. Az a [előző lépésben](./container-instances-tutorial-prepare-app.md), a tároló-lemezkép létrejött egy egyszerű webes alkalmazáshoz írt [Node.js](http://nodejs.org). Ez a kép ebben az oktatóanyagban a rendszer előkészítésre továbbít egy Azure-tároló beállításjegyzék. A tároló kép nem hozott létre, ha vissza [oktatóanyag 1 – tároló kép létrehozása](./container-instances-tutorial-prepare-app.md). 

Az Azure-tároló beállításjegyzék egy Azure-alapú, személyes beállításjegyzék Docker-tároló lemezképek. Ez az oktatóanyag végigvezeti Azure tároló beállításjegyzék-példány telepítését, valamint a végez leküldést a tároló-lemezkép. Befejeződött a lépések az alábbiak:

> [!div class="checklist"]
> * Egy Azure-tároló beállításjegyzék-példány telepítése
> * Azure-tároló beállításjegyzék címkézési tároló képe
> * Azure-tároló beállításjegyzék Rendszerkép feltöltése

A következő útmutatókból telepít a tároló a személyes beállításjegyzékből Azure tároló példányok.

## <a name="before-you-begin"></a>Előkészületek

Ez az oktatóanyag megköveteli, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

## <a name="deploy-azure-container-registry"></a>Telepítse az Azure tároló beállításjegyzék

Egy Azure-tároló beállításjegyzék való telepítésekor, először egy erőforráscsoportot. Egy Azure-erőforráscsoportot gyűjteményei logikai mely Azure az erőforrások telepítése és kezelése.

Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal. Ebben a példában az erőforráscsoport neve *myResourceGroup* jön létre a *eastus* régióban.

```azurecli
az group create --name myResourceGroup --location eastus
```

Hozzon létre egy Azure-tárolóba beállításjegyzéket a [az acr létrehozása](/cli/azure/acr#create) parancsot. Egy tároló beállításjegyzék neve **egyedinek kell lennie**. A következő példában használjuk a neve *mycontainerregistry082*.

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Ez az oktatóanyag a többi, egész használjuk `<acrname>` a tároló neve választott számára.

## <a name="container-registry-login"></a>Tároló beállításjegyzék bejelentkezés

A ACR példányát előtt képek rá kell bejelentkezni. Használja a [az acr bejelentkezési](https://docs.microsoft.com/en-us/cli/azure/acr#login) parancs használatával végrehajtani a műveletet. Meg kell adnia az egyedi név, a tároló beállításjegyzék létrehozásakor.

```azurecli
az acr login --name <acrName>
```

A parancs visszaadja a "Sikeres bejelentkezés" üzenet, amint befejeződött.

## <a name="tag-container-image"></a>Címke tároló kép

A titkos beállításjegyzékből tároló lemezkép telepítéséhez, a lemezkép kell címkézését a `loginServer` nevét a beállításjegyzékben.

Lemezképek aktuális listájának megtekintéséhez használja a `docker images` parancsot.

```bash
docker images
```

Kimenet:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Ahhoz, hogy a loginServer nevét, a következő parancsot.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Címke a *aci-oktatóanyag – alkalmazás* a tároló beállításjegyzék loginServer lemezkép. Továbbá adja hozzá `:v1` , a lemezkép neve végén. Ezt a címkét azt jelzi, hogy a lemezkép verziószámát.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Miután megjelölve, futtassa az `docker images` ellenőrzése a műveletet.

```bash
docker images
```

Kimenet:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Azure-tároló beállításjegyzék leküldéses kép

Leküldéses a *aci-oktatóanyag – alkalmazás* kép a beállításjegyzékhez.

Az alábbi példa használatával, a tároló loginServer neve cserélje le a környezetből loginServer.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a>Azure-tároló beállításjegyzék lemezképek felsorolása

Olyan lemezképkészlet, amellyel az Azure-tárolóba beállításjegyzék értesítését listájához való visszatéréshez felhasználói a [az acr tárház lista](/cli/azure/acr/repository#list) parancsot. A parancs frissíti a tároló neve.

```azurecli
az acr repository list --name <acrName> --output table
```

Kimenet:

```azurecli
Result
----------------
aci-tutorial-app
```

A címkék azokba megtekintéséhez használja a [az acr tárház megjelenítése-címkék](/cli/azure/acr/repository#show-tags) parancsot.

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

Az oktatóanyag egy Azure-tároló beállításjegyzék Azure tároló példányok való használatra készült, és a tároló kép leküldött volt. A következő lépéseket hajtotta végre:

> [!div class="checklist"]
> * Egy Azure-tároló beállításjegyzék-példány telepítése
> * Azure-tároló beállításjegyzék címkézési tároló képe
> * Azure-tároló beállításjegyzék Rendszerkép feltöltése

A következő oktatóanyag az Azure-ban Azure tároló példányok a tároló telepítésével kapcsolatos további továbblépés.

> [!div class="nextstepaction"]
> [Azure-tároló példányok – tárolók üzembe helyezése](./container-instances-tutorial-deploy-app.md)
