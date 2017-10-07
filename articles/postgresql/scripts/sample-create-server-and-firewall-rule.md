---
title: "AAA \"Azure CLI Script – hozzon létre egy Azure-adatbázis PostgreSQL |} Microsoft dokumentumok\""
description: "Az Azure CLI parancsfájl minta - adatbázist hoz létre Azure PostgreSQL-kiszolgáló, és beállítja egy kiszolgálószintű tűzfalszabályt."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: bbe31f77283aa4a3bb36d1fd9171c280594a8267
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-hello-azure-cli"></a>Egy PostgreSQL-kiszolgálóhoz tartozó Azure-adatbázis létrehozása és konfigurálása a tűzfalszabályok hello Azure parancssori felület használatával
Ez a parancsfájlpélda CLI adatbázist hoz létre Azure PostgreSQL-kiszolgáló, és konfigurálja egy kiszolgálószintű tűzfalszabályt. Hello parancsfájl sikeres futtatása után hello PostgreSQL-kiszolgáló elérhető az összes Azure-szolgáltatásokat, és hello konfigurált IP-címet.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl
Ez a parancsfájlpélda szerkeszteni hello kiemelt sorok toocustomize hello rendszergazda felhasználónevét és jelszavát.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása
Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a>Parancsfájl ismertetése
A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| **A parancs** | **Megjegyzések** |
|---|---|
| [az csoport létrehozása](/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az postgres kiszolgáló létrehozása](/cli/azure/postgres/server#create) | Létrehoz egy PostgreSQL hello adatbázisokat üzemeltető kiszolgálón. |
| [az postgres kiszolgáló tűzfal létrehozása](/cli/azure/postgres/server/firewall-rule#create) | Hoz létre egy tűzfal szabály tooallow toohello kiszolgálót, és tartozó adatbázisok hello megadott IP-címtartományt. |
| [az csoport törlése](/cli/azure/group#delete) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések
- További információt az Azure CLI hello: [Azure CLI-dokumentáció](/cli/azure/overview)
- Próbálja meg a további parancsfájlok: [PostgreSQL Azure-adatbázis Azure CLI-minták](../sample-scripts-azure-cli.md)
