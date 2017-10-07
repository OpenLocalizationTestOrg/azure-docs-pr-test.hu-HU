---
title: "aaaAzure tároló beállításjegyzék webhookok |} Microsoft Docs"
description: "Azure-tárolót beállításjegyzék webhookok"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: "Docker, tárolók, ACR"
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2017
ms.author: nepeters
ms.openlocfilehash: adc2afec486007e2d54cd689e6f7ef8b1098db06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-container-registry-webhooks---azure-portal"></a>Azure-tároló beállításjegyzék webhookok - Azure-portál használatával

Egy Azure-tárolót beállításjegyzék tárolja, és kezeli a saját Docker tároló képek, hasonló toohello módon Docker Hub nyilvános Docker-lemezképet tárolja. Egyes műveletekre kerül sor egy, a beállításjegyzék-adattárak webhookok tootrigger események használhatja. Webhook válaszolhassanak tooevents hello beállításjegyzék szinten, vagy azok le tooa adott tárház címke hatóköre beállítható úgy. 

További háttér és fogalmak: hello [áttekintése](./container-registry-intro.md).

## <a name="prerequisites"></a>Előfeltételek 

- Azure-tárolót felügyelt beállításjegyzék - hozzon létre egy felügyelt tároló beállításjegyzék az Azure-előfizetéshez. Például hello Azure-portált használja, vagy Azure CLI 2.0 hello. 
- A docker parancssori felület - tooset egy Docker gazdagép és a hozzáférés hello Docker parancssori felület parancsait, mint a helyi számítógép telepítéséhez Docker-motorhoz. 

## <a name="create-webhook-azure-portal"></a>Azure-portálon webhook létrehozása

1. Jelentkezzen be Azure-portálon toohello, és keresse meg a kívánt toocreate webhookok toohello beállításjegyzék. 

2. Hello tároló paneljén válassza a hello "Webhookok" lapot. 

3. Válassza ki a "Hozzáadás" hello webhook panel eszköztáron. 

4. Teljes hello *Webhook létrehozása* űrlap, a következő információ hello:

| Érték | Leírás |
|---|---|
| Név | azt szeretné, hogy toogive toohello webhook hello neve. Ez csak kisbetűket és számokat tartalmazhat, és 5-50 karakter közötti lehet. |
| Szolgáltatás URI-azonosítója | hello URI, ahol hello webhook POST értesítést küldjön-e. |
| Egyéni fejlécek | A fejlécek kívánt toopass hello POST kéréssel együtt. Össze kell a "kulcs: érték" formátumban. |
| Eseményindító műveletek | Hello webhook kiváltó műveletek. Jelenleg webhookok forrása a leküldéses és/vagy a műveletek tooan lemezkép törlése. |
| status | hello állapota hello webhook létrehozása után. Alapértelmezés szerint engedélyezve van. |
| Hatókör | mely hello webhook works hello hatókörén. Alapértelmezés szerint hello hatóköre hello beállításjegyzékben található összes esemény. Azt adható meg a tárház vagy egy címkét a hello formátumban "tárház: címke". |

Példa webhook űrlap:

![VEZÉNYLŐTÍPUSÚ FELHASZNÁLÓI FELÜLET](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Az Azure parancssori felület webhook létrehozása

a webhook használatával toocreate hello Azure CLI használata hello [az acr webhook létrehozása](/cli/azure/acr/webhook#create) parancsot.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Teszt webhook

### <a name="azure-portal"></a>Azure Portal

Előzetes toousing hello webhook tárolóra leküldéses lemezképet, és törlési műveletek, hello segítségével vizsgálható **Ping** gombra. Használatakor a hello Ping egy általános post kérelem toohello megadott végpont és a naplók hello választ küldi. Ez akkor hasznos, amelyek hello webhook tooverify megfelelően be van állítva.

1. Válassza ki a kívánt tootest hello webhook. 
2. Hello felső eszköztárán válassza hello "Pingelje" műveletet. 
3. Ellenőrizze a hello kérelem és válasz.

### <a name="azure-cli"></a>Azure CLI

tootest egy ACR webhook hello Azure CLI használata hello [az acr webhook ping](/cli/azure/acr/webhook#ping) parancsot.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

toosee hello eredmény elérése érdekében használjon hello [az acr webhook lista-események](/cli/azure/acr/webhook#list-events) parancsot. 

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Webhook törlése

### <a name="azure-portal"></a>Azure Portal

Minden egyes webhook hello webhook és hello Törlés gomb az Azure-portálon hello kiválasztásával törölheti.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```