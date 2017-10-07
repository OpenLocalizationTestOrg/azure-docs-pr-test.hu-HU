---
title: "AAA \"Azure CLI parancsfájl méretű Azure adatbázis a PostgreSQL |} Microsoft dokumentumok\""
description: "Az Azure CLI parancsfájl minta - skálázási Azure adatbázis PostgreSQL server tooa más-más teljesítménybeli szint hello metrikák lekérdezése után."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: 678b28941dbb4334cb374d4888991a00b44966b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Figyelheti és méretezhető egy PostgreSQL egykiszolgálós Azure parancssori felület használatával
Ez a parancsfájlpélda CLI PostgreSQL server tooa különböző teljesítményszintet egy Azure-adatbázis arányosan hello metrikák lekérdezése után. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl
Ez a parancsfájlpélda módosítsa a kijelölt hello sorok toocustomize hello rendszergazda felhasználónevét és jelszavát. Hello az figyelő-parancsokat a saját előfizetés-azonosító szerepel hello előfizetés-azonosító cseréje.[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása
Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a>Parancsfájl ismertetése
A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| **A parancs** | **Megjegyzések** |
|---|---|
| [az csoport létrehozása](/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az postgres kiszolgáló létrehozása](/cli/azure/postgres/server#create) | Létrehoz egy PostgreSQL hello adatbázisokat üzemeltető kiszolgálón. |
| [az a figyelő metrikák listája](/cli/azure/monitor/metrics#list) | Hello metrika listaértéket hello erőforrások. |
| [az csoport törlése](/cli/azure/group#delete) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések
- További információt az Azure CLI hello: [Azure CLI-dokumentáció](/cli/azure/overview)
- Próbálja meg a további parancsfájlok: [PostgreSQL Azure-adatbázis Azure CLI-minták](../sample-scripts-azure-cli.md)
- További információt a méretezés: [szolgáltatásszintek](../concepts-service-tiers.md) és [egységek számítási és tárolási egység](../concepts-compute-unit-and-storage.md)
