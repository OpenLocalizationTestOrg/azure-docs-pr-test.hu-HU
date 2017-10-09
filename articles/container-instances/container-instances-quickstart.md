---
title: "aaaCreate az első Azure tároló példányok tároló |} Az Azure Docs"
description: "Az Azure Container Instances üzembe helyezése és az első lépések"
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
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b57c52714933bd3b28c44d33f9af7cd1f23fb951
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Az első tároló létrehozása az Azure Container Instances szolgáltatásban

Azure tároló példányok teszi, hogy könnyen toocreate, és az Azure-ban tárolók kezelése. A gyors üzembe helyezési a létrehoz egy tárolót az Azure-ban, és közzétenni toohello internet nyilvános IP-címmel. Ez a művelet egyetlen paranccsal hajtható végre. Néhány másodperc elteltével a következőt láthatja a böngészőben:

![Az Azure Container Instances használatával üzembe helyezett alkalmazás képe a böngészőben][aci-app-browser]

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.12 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Az Azure Container Instances Azure-erőforrások, amelyeket egy Azure-erőforráscsoportba kell helyezni. Ez egy olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Tároló létrehozása

A tároló létrehozásához meg kell adnia egy nevet, egy Docker-rendszerképet és egy Azure-erőforráscsoportot. Opcionálisan teszi közzé hello tároló toohello internet nyilvános IP-címmel. Ebben az esetben egy, a [Node.js](http://nodejs.org) használatával létrehozott, nagyon egyszerű webalkalmazást tartalmazó tárolót használunk.

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

Néhány másodpercen belül szerezheti be a válasz tooyour kérelmet. Kezdetben hello tároló akkor fog szerepelni a **létrehozása** állapotát, de néhány másodpercen belül kell kezdődnie. Ellenőrizheti a hello használatával hello állapotát `show` parancs:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

Hello kimeneti hello alsó részén látni fogja a hello tároló üzembe helyezési állapota és az IP-címe:

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Miután hello tároló áthelyezi toohello **sikeres** állapotba kerül, hello böngészőben megadott hello IP-cím használatával csatlakozni tud hozzá. 

![Az Azure Container Instances használatával üzembe helyezett alkalmazás képe a böngészőben][aci-app-browser]

## <a name="pull-hello-container-logs"></a>Hello tároló naplók lekéréses

Képes lekérni a létrehozott hello hello tároló hello naplók `logs` parancs:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Kimenet:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-hello-container"></a>Hello tároló törlése

Amikor elkészült, hello tárolóval, eltávolíthatja azt hello segítségével `delete` parancs:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Az összes hello kód számára érhető el a gyors üzembe helyezési hello tárolójában [a Githubon][app-github-repo], a Dockerfile együtt. Ha szeretné tootry felépítése saját magának, és telepíti azokat tooAzure tároló példányok hello Azure tároló beállításjegyzék használata, továbbra is toohello Azure tároló példányok oktatóanyag.

> [!div class="nextstepaction"]
> [Az Azure Container Instances oktatóanyagai](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png