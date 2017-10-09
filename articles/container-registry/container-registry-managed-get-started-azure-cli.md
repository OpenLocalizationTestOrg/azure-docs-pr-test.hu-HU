---
title: "aaaCreate az Docker-tároló beállításjegyzék titkos - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ismerkedés a létrehozása és kezelése a saját Docker-tároló nyilvántartó, hello Azure CLI 2.0"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: 2cadf42db0681a09c95486510f1e65c6f87c5280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-cli"></a>Hozzon létre egy felügyelt tároló beállításjegyzék hello Azure parancssori felület használatával

Az Azure Container Registry egy felügyelt Docker-tárolóregisztrációs adatbázis-szolgáltatás, amely a privát Docker-tárolók rendszerképeinek tárolására szolgál. Ez az útmutató adatokat hello Azure parancssori felület használatával felügyelt Azure-tároló beállításjegyzék példány létrehozása.

A felügyelt Azure-tárolóregisztrációs adatbázisok előzetes verziós kiadásban, és nem az összes régióban érhetők el.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *westcentralus* helyét.

```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-container-registry"></a>Tároló-beállításjegyzék létrehozása

Hozzon létre egy ACR példányt hello segítségével [az acr létrehozása](/cli/azure/acr#create) parancsot.

> [!NOTE]
> A beállításjegyzék létrehozásakor adjon meg egy globálisan egyedi legfelső szintű tartománynevet, amely csak betűket és számokat tartalmaz.

 hello beállításjegyzék neve hello példában *myContainerRegistry1*, helyettesítse a saját, egyedi nevét.

```azurecli
az acr create --name myContainerRegistry1 --resource-group myResourceGroup --sku Managed_Standard
```

Hello beállításjegyzék létrehozásakor hello kimenete hasonló toohello következő:

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-29T04:50:28.607134+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry1",
  "location": "westcentralus",
  "loginServer": "mycontainerregistry1.azurecr.io",
  "name": "myContainerRegistry1",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Managed_Standard",
    "tier": "Managed"
  },
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="log-in-tooacr-instance"></a>Jelentkezzen be tooACR példány

Kérdez le, és húzza a tároló képek, előtt be kell jelentkeznie toohello ACR példány. toodo Igen, használja a hello [az acr bejelentkezési](/cli/azure/acr#login) parancsot.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

hello parancs visszaadja a "Sikeres bejelentkezés" üzenet, amint befejeződött.

## <a name="use-azure-container-registry"></a>Az Azure Container Registry használata

### <a name="list-container-images"></a>Tárolórendszerképek listázása

Használjon hello `az acr` parancssori felület parancsai tooquery hello képek és címkék tárházban.

> [!NOTE]
> Jelenleg, tároló-beállításjegyzék nem támogatja a hello `docker search` parancs tooquery képek és címkék.

### <a name="list-repositories"></a>Adattárak listázása

hello alábbi példa felsorolja a beállításjegyzék (JavaScript Object Notation) JSON formátumban hello adattárak:

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Címkék listázása

hello alábbi példa felsorolja a hello hello címkék **minták/nginx** tárház JSON formátumban:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezési a létrehozott egy felügyelt Azure tároló beállításjegyzék-példányhoz hello Azure parancssori felület használatával.

> [!div class="nextstepaction"]
> [Leküldéses az első kép hello Docker parancssori felület használatával](container-registry-get-started-docker-cli.md)
