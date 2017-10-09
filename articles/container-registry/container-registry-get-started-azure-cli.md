---
title: "aaaCreate az Docker-tároló beállításjegyzék titkos - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ismerkedés a létrehozása és kezelése a saját Docker-tároló nyilvántartó, hello Azure CLI 2.0"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0d876a70b71a5e1bd564fbc9198f693dfe8a347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-cli-20"></a>Hozza létre a titkos Docker tároló beállításkulcs hello Azure CLI 2.0 használatával
Hello parancsokat használhatja [Azure CLI 2.0](https://github.com/Azure/azure-cli) toocreate egy tároló beállításjegyzék és a beállítások kezelése a Linux, Mac vagy Windows számítógépről. Is hozhat létre, és hello segítségével kezeléséhez tároló [Azure-portálon](container-registry-get-started-portal.md) vagy programozott módon hello tároló beállításjegyzék [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).


* Háttér és fogalmak: [hello áttekintése](container-registry-intro.md)
* A Súgó a tároló beállításjegyzék parancssori felület parancsait (`az acr` parancsok), adja át a hello `-h` paraméter tooany parancsot.


## <a name="prerequisites"></a>Előfeltételek
* **Az Azure CLI 2.0**: tooinstall és hello CLI 2.0 első lépések című hello [telepítési utasításokat](/cli/azure/install-azure-cli). Jelentkezzen be Azure-előfizetés tooyour futtatásával `az login`. További információkért lásd: [Ismerkedés a hello CLI 2.0](/cli/azure/get-started-with-azure-cli).
* **Erőforráscsoport**: A tárolójegyzék létrehozása előtt hozzon létre egy [erőforráscsoportot](../azure-resource-manager/resource-group-overview.md#resource-groups), vagy használjon egy meglévő erőforráscsoportot. Ellenőrizze, hogy hello erőforráscsoport van egy helyet, ahol hello tároló beállításszolgáltatás [elérhető](https://azure.microsoft.com/regions/services/). egy erőforrás csoport használatával toocreate hello CLI 2.0, lásd: [hello CLI 2.0 hivatkozás](/cli/azure/group).
* **A tárfiók** (nem kötelező): hozzon létre egy szabványos Azure [tárfiók](../storage/common/storage-introduction.md) tooback hello tároló hello rendszerleíró adatbázis ugyanazon a helyen. Ha nem adja meg a storage-fiók létrehozásakor a beállításjegyzéket `az acr create`, hello parancs létrehoz egyet. a tárolási fiók használatával toocreate CLI 2.0 hello című [hello CLI 2.0 hivatkozás](/cli/azure/storage/account). A Premium Storage jelenleg nem támogatott.
* **Szolgáltatás egyszerű** (nem kötelező): Ha beállításjegyzékbeli hello CLI létrehozásához, alapértelmezés szerint nincs beállítva a hozzáférés. Igényeitől függően rendelje hozzá egy meglévő Azure Active Directory szolgáltatás egyszerű tooa beállításjegyzék (vagy hozzon létre, és rendelje hozzá egy új), vagy engedélyezze a hello beállításjegyzék rendszergazda felhasználói fiókot. A cikk későbbi részében hello szakaszban talál. Beállításjegyzék-hozzáféréssel kapcsolatos további információkért lásd: [hitelesítés hello tároló rendszerleíró](container-registry-authentication.md).

## <a name="create-a-container-registry"></a>Tároló-beállításjegyzék létrehozása
Futtassa a hello `az acr create` parancs toocreate tároló beállításjegyzékbeli.

> [!TIP]
> A beállításjegyzék létrehozásakor adjon meg egy globálisan egyedi legfelső szintű tartománynevet, amely csak betűket és számokat tartalmaz. hello beállításjegyzék neve hello példákban `myRegistry1`, de helyettesítse a saját, egyedi nevét.
>
>

a következő parancsot használja hello minimális paraméterek toocreate tároló beállításjegyzék hello `myRegistry1` hello erőforráscsoportban `myResourceGroup`, és hello segítségével *alapvető* termékváltozat:

```azurecli
az acr create --name myRegistry1 --resource-group myResourceGroup --sku Basic
```

* A(z) `--storage-account-name` nem kötelező. Ha nem megadott, a storage-fiók létrejön a neve, amely hello beállításjegyzék neve és hello időbélyegzőnek megadott erőforráscsoport.

Hello beállításjegyzék létrehozásakor hello kimenete hasonló toohello következő:

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T18:36:29.124842+00:00",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry
/registries/myRegistry1",
  "location": "southcentralus",
  "loginServer": "myregistry1.azurecr.io",
  "name": "myRegistry1",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myregistry123456789"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}

```


Különösen ügyeljen a következőre:

* `id`-Előfizetését, ha azt szeretné, hogy a szolgáltatás egyszerű tooassign igénylő hello beállításjegyzék azonosítója.
* `loginServer`-hello teljesen minősített név túl meg[toohello beállításjegyzék-e jelentkezni](container-registry-authentication.md). Ebben a példában hello neve: `myregistry1.exp.azurecr.io` (kisbetűket).

## <a name="assign-a-service-principal"></a>Egyszerű szolgáltatás hozzárendelése
Parancssori felület 2.0 parancsok tooassign egy Azure Active Directory szolgáltatás egyszerű tooa beállításjegyzék használja. hello szolgáltatás egyszerű ezekben a példákban szerepét hello tulajdonos, de rendelhet [más szerepkörök](../active-directory/role-based-access-control-configure.md) irányíthatók.

### <a name="create-a-service-principal-and-assign-access-toohello-registry"></a>Egy egyszerű szolgáltatás létrehozása és hozzárendelése hozzáférés toohello beállításjegyzék
A következő parancs hello, egy új szolgáltatás egyszerű tulajdonos szerepkör hozzáférés toohello beállításjegyzék-azonosító hello átadni kapja `--scopes` paraméter. Adjon meg egy erős jelszót a hello `--password` paraméter.

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a>Meglévő egyszerű szolgáltatás hozzárendelése
Ha már rendelkezik egy egyszerű szolgáltatást, és szeretné, hogy tooassign azt tulajdonos szerepkör hozzáférés toohello beállításjegyzék, futtassa a következő példa egy parancshoz hasonló toohello. Hello szolgáltatás egyszerű Alkalmazásazonosító hello segítségével át `--assignee` paraméter:

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a>Rendszergazdai hitelesítő adatok kezelése
Minden tároló-beállításjegyzékhez automatikusan létrejön egy rendszergazdai fiók, amely alapértelmezés szerint le van tiltva. a következő példák szemléltetik hello `az acr` parancssori felület parancsai toomanage hello rendszergazdai hitelesítő adatait a tároló beállításjegyzék.

### <a name="obtain-admin-user-credentials"></a>Rendszergazdai hitelesítő adatok beszerzése
```azurecli
az acr credential show -n myRegistry1
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Rendszergazdai felhasználó engedélyezése meglévő beállításjegyzékhez
```azurecli
az acr update -n myRegistry1 --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Rendszergazdai felhasználó letiltása meglévő beállításjegyzékhez
```azurecli
az acr update -n myRegistry1 --admin-enabled false
```

## <a name="list-images-and-tags"></a>Képek és címkék listázása
Használjon hello `az acr` parancssori felület parancsai tooquery hello képek és címkék tárházban.

> [!NOTE]
> Jelenleg, tároló-beállításjegyzék nem támogatja a hello `docker search` parancs tooquery képek és címkék.


### <a name="list-repositories"></a>Adattárak listázása
hello alábbi példa felsorolja a beállításjegyzék (JavaScript Object Notation) JSON formátumban hello adattárak:

```azurecli
az acr repository list -n myRegistry1 -o json
```

### <a name="list-tags"></a>Címkék listázása
hello alábbi példa felsorolja a hello hello címkék **minták/nginx** tárház JSON formátumban:

```azurecli
az acr repository show-tags -n myRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Következő lépések
* [Leküldéses az első kép hello Docker parancssori felület használatával](container-registry-get-started-docker-cli.md)
