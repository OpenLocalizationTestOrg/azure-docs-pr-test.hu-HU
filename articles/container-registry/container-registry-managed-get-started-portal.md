---
title: "aaaCreate titkos Docker beállításjegyzék - Azure-portál |} Microsoft Docs"
description: "Első lépések, létrehozását és kezelését a személyes Docker-tároló nyilvántartó, hello Azure-portálon"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: cf3ce0dcf3036d0e9cd1eaf01721deccb00248d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-portal"></a>Hozzon létre egy felügyelt tároló beállításjegyzék hello Azure-portál használatával

Az Azure Container Registry egy felügyelt Docker-tárolóregisztrációs adatbázis-szolgáltatás, amely a privát Docker-tárolók rendszerképeinek tárolására szolgál. Ez az útmutató adatokat hello Azure-portál használatával felügyelt Azure-tároló beállításjegyzék példány létrehozása.

A felügyelt Azure-tárolóregisztrációs adatbázisok előzetes verziós kiadásban, és nem az összes régióban érhetők el.

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Jelentkezzen be toohello http://portal.azure.com: az Azure portál.

## <a name="create-a-container-registry"></a>Tároló-beállításjegyzék létrehozása

1. Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.

2. Hello a piactéren **Azure tároló beállításjegyzék** , és jelölje ki.

3. Kattintson a **létrehozása** hello ACR létrehozás panelen megnyitott lesz.

    ![A tároló-beállításjegyzékek beállításai](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. A hello **Azure tároló beállításjegyzék** panelen adja meg a következő információ hello. Amikor elkészült, kattintson a **Létrehozás** gombra.

    a. **Beállításjegyzék neve**: egy globálisan egyedi legfelső szintű tartománynév az adott beállításjegyzékhez. Ebben a példában hello beállításkulcs értéke *myAzureContainerRegistry1*, de helyettesítse a saját, egyedi nevét. hello név csak betűket és számokat tartalmazhat.

    b. **Erőforráscsoport**: Válasszon ki egy létező [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups) vagy egy új hello típusnév.

    c. **Hely**: válassza ki az Azure-adatközpontban helyet, ahol hello szolgáltatás [elérhető](https://azure.microsoft.com/regions/services/), például a **déli középső Régiójában**.

    d. **A rendszergazdai jogú felhasználó**: Ha azt szeretné, egy rendszergazda felhasználó tooaccess hello beállításjegyzék engedélyezése. Ez a beállítás hello beállításkulcs létrehozása után módosíthatja.

    e. **Használja a felügyelt beállításjegyzék**: kattintson az Igen toohave ACR automatikusan hello beállításjegyzék tárterület kezelése, webhookok használja, és az AAD-hitelesítés használatára.

    f. **Tarifacsomag**: Válasszon egy tarifacsomagot. További információért tekintse meg itt az ACR-díjszabást.

## <a name="log-in-tooacr-instance"></a>Jelentkezzen be tooACR példány

Kérdez le, és húzza a tároló képek, előtt be kell jelentkeznie toohello ACR példány. 

toodo tehát hello Azure CLI 2.0 használja. Első lépésként szükség esetén jelentkezzen be Azure-ban hello [az bejelentkezési](/cli/azure/#login) parancsot. 

```azurecli
az login
```

Következő lépésként az hello [az acr bejelentkezési](/cli/azure/acr#login) parancs toolog az Azure-tároló beállításjegyzék toohello.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

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

A gyors üzembe helyezési a létrehozott egy felügyelt Azure-tároló beállításjegyzék-példányhoz hello Azure-portál használatával.

> [!div class="nextstepaction"]
> [Leküldéses az első kép hello Docker parancssori felület használatával](container-registry-get-started-docker-cli.md)
