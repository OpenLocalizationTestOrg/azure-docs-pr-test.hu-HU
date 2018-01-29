---
title: "Parancssori felület például egy SQL rugalmas készlet-Azure SQL Database méretezi |} Microsoft Docs"
description: "Az Azure parancssori felület a példaként megadott parancsfájlt a rugalmas SQL-készletet, az Azure SQL-adatbázis méretezése"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 1bd1bb6005f463a8a317ec959661ac6c75d600d1
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/07/2017
---
# <a name="use-cli-to-scale-a-sql-elastic-pool-in-azure-sql-database"></a>A rugalmas SQL-készletet, az Azure SQL-adatbázis méretezése parancssori felület használatával

Az Azure CLI mintaparancsfájl hoz létre SQL rugalmas készletek, készletezett adatbázisok áthelyezése, és a rugalmas készlet teljesítményszintet vált. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat egy erőforráscsoportot, a logikai kiszolgáló, az SQL-adatbázis és a tűzfalszabályok létrehozásához. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#az_group_create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az sql-kiszolgáló létrehozása](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | Az SQL-adatbázist futtató logikai kiszolgáló létrehozása. |
| [az sql rugalmas-címkészletek létrehozása](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create) | A logikai kiszolgálón belül a rugalmas adatbáziskészlet létrehozása. |
| [az sql-adatbázis létrehozása](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_create) | Az SQL-adatbázis létrehozása a logikai kiszolgálón. |
| [az sql rugalmas-gyűjtők frissítése](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) | A rugalmas adatbáziskészlet frissíti, ebben a példában a hozzárendelt edtu-ra módosítja. |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További SQL-adatbázis CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure SQL Database](../sql-database-cli-samples.md).
