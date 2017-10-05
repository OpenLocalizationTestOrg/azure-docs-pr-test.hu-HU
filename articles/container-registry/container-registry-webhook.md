---
title: "Azure-tárolót beállításjegyzék webhookok |} Microsoft Docs"
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
ms.openlocfilehash: d0190f5725671c320d92b897f0dcef7a526a86e3
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="using-azure-container-registry-webhooks---azure-portal"></a>Azure-tároló beállításjegyzék webhookok - Azure-portál használatával

Egy Azure-tárolót beállításjegyzék tárolja, és kezeli a saját Docker tároló képek, hasonló ahhoz, ahogy a Docker Hub nyilvános Docker-lemezképet tárolja. Bizonyos műveleteket kerül sor egy, a beállításjegyzék-adattárak eseményindító események webhookok használhatja. Webhook válaszolhassanak a beállításjegyzék szinten események, vagy azok le egy adott tárház címke hatóköre beállítható úgy. 

További háttér és fogalmak a [áttekintése](./container-registry-intro.md).

## <a name="prerequisites"></a>Előfeltételek 

- Azure-tárolót felügyelt beállításjegyzék - hozzon létre egy felügyelt tároló beállításjegyzék az Azure-előfizetéshez. Például használja az Azure-portálon vagy az Azure CLI 2.0. 
- A docker parancssori felület – Docker-gazdagépként a helyi számítógépet, és a Docker parancssori felület parancsait eléréséhez telepítse a Docker-motorhoz. 

## <a name="create-webhook-azure-portal"></a>Azure-portálon webhook létrehozása

1. Jelentkezzen be az Azure-portálon, és keresse meg a beállításjegyzéket, amelyen létrehozásához webhook. 

2. A tároló paneljén kattintson a "Webhookok" fülre. 

3. Válassza ki a "Hozzáadás" a webhook panel eszköztáron. 

4. Fejezze be a *Webhook létrehozása* képernyőn a következő információkat:

| Érték | Leírás |
|---|---|
| Név | Kíván beállítani a webhook nevét. Ez csak kisbetűket és számokat tartalmazhat, és 5-50 karakter közötti lehet. |
| Szolgáltatás URI-azonosítója | Az URI, ahol a webhook POST értesítést küldjön-e. |
| Egyéni fejlécek | A POST-kérelmet együtt átadni kívánt fejléceket. Össze kell a "kulcs: érték" formátumban. |
| Eseményindító műveletek | A webhook kiváltó műveletek. Jelenleg webhookok forrása a leküldéses és/vagy törlési műveletek a lemezképre. |
| status | A webhook létrehozása után állapota. Alapértelmezés szerint engedélyezve van. |
| Hatókör | A hatókör, ahol a webhook működik. Alapértelmezés szerint a hatóköre az összes esemény a beállításjegyzékben. Az adható meg a tárház vagy egy címkét a következő formátumban "tárház: címke". |

Példa webhook űrlap:

![VEZÉNYLŐTÍPUSÚ FELHASZNÁLÓI FELÜLET](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Az Azure parancssori felület webhook létrehozása

A webhook az Azure parancssori felület használatával hozhat létre a [az acr webhook létrehozása](/cli/azure/acr/webhook#create) parancsot.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Teszt webhook

### <a name="azure-portal"></a>Azure Portal

Való használata előtt a webhook tárolóra kép leküldéses és törlési műveletek, használatával vizsgálható a **Ping** gombra. Használatakor a Ping egy általános post kérést küld a megadott végpontot, és naplózza a választ. Ez akkor hasznos győződjön meg arról, hogy a webhook megfelelően van beállítva.

1. Válassza ki a vizsgálni kívánt webhook. 
2. A felső eszköztárban látható válassza ki a "Ping" műveletet. 
3. Ellenőrizze a kérelem és válasz.

### <a name="azure-cli"></a>Azure CLI

Az Azure parancssori felülettel ACR webhook teszteléséhez használja a [az acr webhook ping](/cli/azure/acr/webhook#ping) parancsot.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Az eredmények megtekintéséhez használja a [az acr webhook lista-események](/cli/azure/acr/webhook#list-events) parancsot. 

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Webhook törlése

### <a name="azure-portal"></a>Azure Portal

Minden egyes webhook törölheti a webhook és a Törlés gomb az Azure portálon kiválasztásával.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```