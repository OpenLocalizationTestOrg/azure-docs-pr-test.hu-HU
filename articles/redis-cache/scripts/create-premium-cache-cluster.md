---
title: "aaaAzure CLI parancsfájl minta - fürtözési hozzon létre egy prémium szintű Azure Redis Cache |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - fürtözési Azure Redis Cache prémium réteg létrehozása"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: ca34d40059b282cb2abc7e3e2b8771226029744c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-premium-azure-redis-cache-with-clustering"></a>Hozzon létre egy prémium szintű Azure Redis Cache fürtözési

Ebben az esetben megismerheti, hogyan toocreate egy 6 GB prémium csomagban Azure Redis Cache fürtözési engedélyezve van, és két szilánkok.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate hello az erőforráscsoportot és a Premium szint redis gyorsítótár fürtözési engedélyezése. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az redis létrehozása](https://docs.microsoft.com/cli/azure/redis#create) | Hozzon létre a Redis Cache példányt. |


## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További Azure Redis Cache CLI parancsfájl minták hello található [Azure Redis Cache dokumentáció](../cli-samples.md).
