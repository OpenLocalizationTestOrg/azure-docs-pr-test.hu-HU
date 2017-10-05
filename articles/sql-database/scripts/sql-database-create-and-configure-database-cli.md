---
title: "Parancssori felület példa-egy Azure SQL-adatbázis létrehozása |} Microsoft Docs"
description: "Az Azure parancssori felület a példaként megadott parancsfájlt az SQL-adatbázis létrehozása"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 908898ca691d2b53b9f54afa60c41e091163bd50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>Parancssori felület használatával hozzon létre egy Azure SQL-adatbázis és a tűzfalszabályok konfigurálása

Az Azure CLI mintaparancsfájl egy Azure SQL Database adatbázist hoz létre, és egy kiszolgálószintű tűzfalszabály konfigurálása. Miután a parancsfájl sikeresen lefutott, az SQL-adatbázis elérhető az összes Azure-szolgáltatások és a konfigurált IP-címet. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[fő](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "SQL-adatbázis létrehozása")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az sql-kiszolgáló létrehozása](/cli/azure/sql/server#create) | Az SQL-adatbázist futtató logikai kiszolgáló létrehozása. |
| [az sql server tűzfal létrehozása](/cli/azure/sql/server/firewall-rule#create) | Engedélyezi a kiszolgálón lévő összes SQL-adatbázis elérését a megadott IP-címtartomány egy tűzfalszabályt hoz létre. |
| [az sql-adatbázis létrehozása](/cli/azure/sql/db#create) | Az SQL-adatbázis létrehozása a logikai kiszolgálón. |
| [az csoport törlése](/cli/azure/resource#delete) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További SQL-adatbázis CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure SQL Database](../sql-database-cli-samples.md).
