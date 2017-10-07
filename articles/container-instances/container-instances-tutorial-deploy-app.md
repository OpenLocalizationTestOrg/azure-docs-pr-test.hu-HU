---
title: "aaaAzure tároló példányok oktatóanyag – alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Azure tároló példányok útmutató - alkalmazás központi telepítése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b9fb098d9491e1073f0be4b14a0b9b1a18f16095
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-tooazure-container-instances"></a>Egy tároló tooAzure tároló példányok telepítése

Ez a hello egy háromrészes oktatóanyag utolsó. A korábbi szakaszokban [a tároló-lemezkép létrejött](container-instances-tutorial-prepare-app.md) és [tooan Azure tároló beállításjegyzék leküldött](container-instances-tutorial-prepare-acr.md). Ez a szakasz hello az oktatóanyag befejezése hello tároló tooAzure tároló példányok üzembe helyezésével. Befejeződött a lépések az alábbiak:

> [!div class="checklist"]
> * Egy Azure Resource Manager-sablonnal a tárolócsoport meghatározása
> * A tárolócsoport hello hello Azure parancssori felület használatával telepítése
> * Tároló naplók megtekintése

## <a name="deploy-hello-container-using-hello-azure-cli"></a>Hello Azure parancssori felület használatával hello tároló üzembe

hello Azure parancssori felület lehetővé teszi, hogy egy parancs a tároló tooAzure tároló példányok telepítését. Mivel hello tároló lemezkép az Azure-tároló titkos beállításjegyzék hello, meg kell adnia a hitelesítő adatok szükséges tooaccess hello azt. Ha szükséges, alább látható módon lekérdezheti azokat.

Tároló beállításjegyzék bejelentkezési kiszolgáló (a beállításjegyzék nevű frissítés):

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

Tároló beállításjegyzék jelszó:

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

toodeploy a tároló lemezkép erőforrással hello tároló beállításjegyzékből kérelem 1 Processzormagok és 1GB memória, futtassa a következő parancs hello:

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

Néhány másodpercen belül kap egy kezdeti választ az Azure Resource Manager. hello központi telepítésének, használjon tooview hello állapotát:

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

Ez a parancs futtatását, amíg a hello állapota Folytatás *függőben lévő* túl*futtató*. Azt is lépjen.

## <a name="view-hello-application-and-container-logs"></a>Hello alkalmazás és a tároló naplók megtekintése

Miután a hello központi telepítés sikeres, nyissa meg a böngésző toohello IP-cím látható a következő parancs hello hello kimenete:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world app hello böngészőben][aci-app-browser]

Hello napló kimeneti hello tároló is megtekintheti:

```azurecli-interactive
az container logs --name aci-tutorial-app -g myResourceGroup
```

Kimenet:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban a tárolók tooAzure tároló példányok telepítésének hello folyamat befejeződött. befejeződtek a hello a következő lépéseket:

> [!div class="checklist"]
> * Az Azure parancssori felület telepítése hello tárolót hello Azure tároló beállításjegyzék használatával hello
> * Hello böngészőben hello alkalmazás megtekintése
> * Megtekintés hello tároló naplók

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
