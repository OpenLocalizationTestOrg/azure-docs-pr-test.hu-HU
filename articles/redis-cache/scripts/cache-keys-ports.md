---
title: "parancssori felület parancsfájl minta - Get hello állomásnév, portokat és az Azure Redis Cache kulcsok aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - Get hello állomásnév, portokat és a kulcsok Azure Redis Cache példányt"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: e6e794558087d6568438c439e2bf99fc46eeb8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-hostname-ports-and-keys-for-azure-redis-cache"></a>Azure Redis Cache hello állomásnév, portokat és a kulcsok beszerzése

Ebben a forgatókönyvben a megismerheti, hogyan használja az tooretrieve hello állomásnév, portokat és a kulcsok tooconnect tooan Azure Redis Cache példány.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok tooretrieve hello állomásnév, a kulcsok és a portok az Azure Redis Cache példány hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az redis megjelenítése](https://docs.microsoft.com/cli/azure/redis#show) | Beolvasni az Azure Redis Cache példány részleteit. |
| [az-redis-listázása](https://docs.microsoft.com/cli/azure/redis#list-keys) | Azure Redis Cache példány elérési kulcsainak beolvasása. |


## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure Redis Cache CLI parancsfájl minták hello található [Azure Redis Cache dokumentáció](../cli-samples.md).
