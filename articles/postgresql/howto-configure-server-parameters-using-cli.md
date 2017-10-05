---
title: "Állítsa be a paramétereket az Azure-adatbázis PostgreSQL |} Microsoft Docs"
description: "Ez a cikk ismerteti a paramétereket az Azure-adatbázis konfigurálása az Azure CLI parancssorból PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: c8a3b5a0225c2cede180d8d57681f2e1a6c6cc3a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Testre szabhatja a kiszolgáló konfigurációs paraméterek Azure parancssori felület használatával
Listáról, megjelenítése, és a parancssori felület (Azure CLI) használatával Azure PostgreSQL-kiszolgáló konfigurációs paraméterek frissítését. Azonban csak egy részhalmazát motor konfigurációk kiszolgálói szinten ki vannak téve, és módosíthatja. 

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató Útmutató lépéseit, az alábbiak szükségesek:
- A kiszolgáló és az adatbázis [PostgreSQL az Azure-adatbázis létrehozása](quickstart-create-server-database-azure-cli.md)
- Telepítés [Azure CLI 2.0](/cli/azure/install-azure-cli) sor segédprogram parancsot, vagy használja az Azure-felhő rendszerhéj a böngészőben.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Lista kiszolgáló konfigurációs paraméterek az Azure Database PostgreSQL-kiszolgáló
Minden módosíthatóvá paraméter a kiszolgáló, az értékek listában, futtassa a [az postgres kiszolgálólista konfigurációs](/cli/azure/postgres/server/configuration#list) parancsot.

A kiszolgáló konfigurációs paraméterek, a kiszolgáló listázhatja **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a>Kiszolgálókonfiguráció paraméter részletek megjelenítése
Egy adott konfigurációs paraméter a kiszolgáló részletei láthatók, futtassa a [az postgres kiszolgáló konfigurációs megjelenítése](/cli/azure/postgres/server/configuration#show) parancsot.

Ez a példa bemutatja részleteit a **napló\_min\_üzenetek** kiszolgáló konfigurációs paraméter kiszolgáló **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **contoso.com.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a>Módosítsa a kiszolgáló konfigurációs paraméter értéke
Egy bizonyos kiszolgáló konfigurációs paraméter értéke is módosíthatja, és ezzel frissíti a PostgreSQL-kiszolgáló motor alapul szolgáló konfigurációs érték. Frissíteni a konfigurációt használja a [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) parancsot. 

Frissítése az **napló\_min\_üzenetek** kiszolgáló konfigurációs paraméter kiszolgáló **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **contoso.com.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
Ha szeretne egy konfigurációs paraméter értékének alaphelyzetbe állítása, egyszerűen választja hagyja ki az opcionális `--value` paraméter, és a szolgáltatás az alapértelmezett érték vonatkoznak. A fenti példában az alábbihoz hasonlóan fog kinézni:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
Ez a művelet visszaállítja a **napló\_min\_üzenetek** konfigurációját, és az alapértelmezett érték **figyelmeztetés**. További részletek a kiszolgálókonfiguráció és a megengedett értékek, dokumentációjában PostgreSQL a [kiszolgálókonfiguráció](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Következő lépések
- Konfigurálja, és hozzáférést kiszolgálónaplókban, [Server PostgreSQL az Azure Database-naplók](concepts-server-logs.md)
