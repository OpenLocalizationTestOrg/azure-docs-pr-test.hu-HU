---
title: "aaaCLI példa-Azure SQL-adatbázis létrehozása |} Microsoft Docs"
description: "Az Azure CLI például parancsfájl-toocreate SQL-adatbázis"
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
ms.openlocfilehash: 0d54e284e19f16387813e24d7beb7ab048a39263
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toocreate-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>CLI toocreate egy Azure SQL-adatbázis használata és a tűzfalszabályok beállítása

Az Azure CLI mintaparancsfájl egy Azure SQL Database adatbázist hoz létre, és egy kiszolgálószintű tűzfalszabály konfigurálása. Hello parancsfájl sikeres futtatása után hello elérhető SQL-adatbázis az összes Azure-szolgáltatások és hello konfigurált IP-címét. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az sql-kiszolgáló létrehozása](/cli/azure/sql/server#create) | Hogy a gazdagépek hello SQL Database logikai kiszolgáló létrehozása. |
| [az sql server tűzfal létrehozása](/cli/azure/sql/server/firewall-rule#create) | Létrehoz egy tűzfal szabály tooallow hozzáférés tooall SQL-adatbázisok hello megadott IP-címtartomány a hello kiszolgálóján. |
| [az sql-adatbázis létrehozása](/cli/azure/sql/db#create) | Hello SQL Database logikai kiszolgáló hello hoz létre. |
| [az csoport törlése](/cli/azure/resource#delete) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További SQL-adatbázis CLI parancsfájl minták hello található [dokumentáció az Azure SQL Database](../sql-database-cli-samples.md).

