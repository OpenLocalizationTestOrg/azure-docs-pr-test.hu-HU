---
title: "aaaConfigure hello szolgáltatás paraméterek Azure adatbázisban a következő PostgreSQL |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure hello szolgáltatás paraméterek az Azure Database PostgreSQL használatára vonatkozó hello Azure CLI parancssori."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 84a11de24ba87fc0eb6744aaa4b53f65a183903d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Testre szabhatja a kiszolgáló konfigurációs paraméterek Azure parancssori felület használatával
Listáról, megjelenítése és a konfigurációs paraméterek hello parancssori felület (Azure CLI) használatával Azure PostgreSQL-kiszolgáló frissítése. Azonban csak egy részhalmazát motor konfigurációk kiszolgálói szinten ki vannak téve, és módosíthatja. 

## <a name="prerequisites"></a>Előfeltételek
Ez hogyan tooguide keresztül toostep, lesz szüksége:
- A kiszolgáló és az adatbázis [PostgreSQL az Azure-adatbázis létrehozása](quickstart-create-server-database-azure-cli.md)
- Telepítés [Azure CLI 2.0](/cli/azure/install-azure-cli) parancssori segédprogram, vagy használjon hello Azure Cloud rendszerhéj hello böngészőben.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Lista kiszolgáló konfigurációs paraméterek az Azure Database PostgreSQL-kiszolgáló
toolist összes módosíthatóvá paraméter a kiszolgáló, az értékek, futtassa hello [az postgres kiszolgálólista konfigurációs](/cli/azure/postgres/server/configuration#list) parancsot.

Hello kiszolgáló konfigurációs paraméterek hello kiszolgáló listázhatja **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a>Kiszolgálókonfiguráció paraméter részletek megjelenítése
egy adott konfigurációs paramétert egy kiszolgálón, futtassa a hello tooshow adatait [az postgres kiszolgáló konfigurációs megjelenítése](/cli/azure/postgres/server/configuration#show) parancsot.

Ez a példa bemutatja hello részleteit **napló\_min\_üzenetek** kiszolgáló konfigurációs paraméter kiszolgáló **mypgserver-20170401.postgres.database.azure.com** alatt Erőforráscsoport **contoso.com.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a>Módosítsa a kiszolgáló konfigurációs paraméter értéke
Egy bizonyos kiszolgáló konfigurációs paraméter értékének hello is módosíthatja, és ekkor frissül az alapul szolgáló konfigurációs érték hello hello PostgreSQL server motor. tooupdate hello konfigurációs használata hello [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) parancsot. 

tooupdate hello **napló\_min\_üzenetek** kiszolgáló konfigurációs paraméter kiszolgáló **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportbatartozó**myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
Ha azt szeretné, hogy egy konfigurációs paraméter értékének tooreset hello, egyszerűen választja ki választható hello tooleave `--value` paraméter, és hello szolgáltatás érvényes hello alapértelmezett értéket. A fenti példában az alábbihoz hasonlóan fog kinézni:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
Ez a művelet visszaállítja hello **napló\_min\_üzenetek** konfigurációs toohello alapértelmezett értéke **figyelmeztetés**. További részletek a kiszolgálókonfiguráció és a megengedett értékek, dokumentációjában PostgreSQL a [kiszolgálókonfiguráció](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Következő lépések
- tooconfigure és hozzáférést kiszolgálónaplókban, lásd: [Server PostgreSQL az Azure Database-naplók](concepts-server-logs.md)
