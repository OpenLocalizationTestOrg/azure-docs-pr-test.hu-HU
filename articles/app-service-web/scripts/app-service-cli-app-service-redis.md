---
title: "Az Azure CLI-parancsfájlt minta - webalkalmazás csatlakozni a redis gyorsítótár |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - webalkalmazás csatlakozni a redis gyorsítótár"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b697c8508a6c3422b6b0d0ca36843a9c884b505f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-a-redis-cache"></a>Egy webes alkalmazás csatlakoztatása a redis gyorsítótár

Ebben a forgatókönyvben, megtudhatja, hogyan egy Azure redis cache és az Azure-webalkalmazás létrehozása. Ezután a redis gyorsítótárt összekapcsolja a web app alkalmazást beállítások használatával.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

Ezt a parancsfájlt a következő parancsok használatával hozzon létre egy erőforráscsoportot, webalkalmazás, redis gyorsítótárat, és minden kapcsolódó erőforrás. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az App Service-csomag létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | App Service-csomag létrehozása. Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára. |
| [az alkalmazás-kulcs létrehozása](https://docs.microsoft.com/cli/azure/webapp#create) | Létrehoz egy Azure-webalkalmazásban. |
| [az redis létrehozása](https://docs.microsoft.com/en-us/cli/azure/redis#create) | Hozzon létre új Redis Cache példányt. Ez az adatok tárolásához. |
| [az-redis-listázása](https://docs.microsoft.com/en-us/cli/azure/redis#list-keys) | A redis cache példány az elérési kulcsainak listázása. |
| [az alkalmazás kulcs appsettings konfiguráció](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás. Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).
