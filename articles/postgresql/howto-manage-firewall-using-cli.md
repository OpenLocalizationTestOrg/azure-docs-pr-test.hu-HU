---
title: "aaaCreate és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok Azure parancssori felületével |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate és kezelheti az Azure-adatbázis használata az Azure CLI parancssori PostgreSQL tűzfalszabályok."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 06e34c9e3996c2ec3df63191d794bc34d0ca0cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok Azure parancssori felület használatával
Kiszolgálószintű tűzfal-szabályokat engedélyezhetik a rendszergazdák toomanage hozzáférés tooan Azure adatbázis PostgreSQL-kiszolgáló egy adott IP-cím vagy az IP-címek. Kényelmes Azure parancssori felület parancsait, segítségével létrehozása, frissítése, törlése, listában, és tűzfal-szabályok toomanage megjelenítése a kiszolgálón. Az áttekintést az Azure-adatbázis PostgreSQL tűzfalak, lásd: [PostgreSQL-kiszolgáló tűzfalszabályainak az Azure-adatbázis](concepts-firewall-rules.md)

## <a name="prerequisites"></a>Előfeltételek
Ez hogyan tooguide keresztül toostep, lesz szüksége:
- Egy [PostgreSQL-kiszolgáló és adatbázis Azure-adatbázis](quickstart-create-server-database-azure-cli.md)
- Telepítés [Azure CLI 2.0](/cli/azure/install-azure-cli) parancssori segédprogram, vagy használjon hello Azure Cloud rendszerhéj hello böngészőben.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Tűzfalszabályok konfigurálása az Azure Database PostgreSQL
Hello [az postgres-tűzfalszabályt](/cli/azure/postgres/server/firewall-rule) parancsok is használt tooconfigure tűzfalszabályokat.

## <a name="list-firewall-rules"></a>Tűzfalszabályok listája 
hello toolist hello meglévő kiszolgáló-tűzfalszabályok hello kiszolgálón futtassa [az postgres tűzfalszabály kiszolgálólista](/cli/azure/postgres/server/firewall-rule#list) parancsot.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
hello kimeneti hello szabályokat sorolja fel, ha vannak ilyenek, alapértelmezés szerint a JSON formátumban. Hello kapcsoló segítségével `--output table` a tábla hello kimenetként olvashatóbb formátumban.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a>Tűzfalszabály létrehozása
egy új tűzfalszabály hello kiszolgálón, futtassa a hello toocreate [az postgres-tűzfalszabály létrehozása](/cli/azure/postgres/server/firewall-rule#create) parancsot. 

Ez a példa lehetővé teszi, hogy minden IP címek tooaccess hello kiszolgálók számos **mypgserver-20170401.postgres.database.azure.com**
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
tooallow egy egyes IP-cím tooaccess, adja meg ugyanazt a címet hello hello kezdő IP- és a záró IP-, ebben a példában szemléltetett módon.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "AllowSingleIpAddress" --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Sikeres, akkor a hello parancs kimenetében létrehozott, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit sorolja fel. Ha hiba történik, a hello helyette kimeneti showserror üzenet szövegét.

## <a name="update-firewall-rule"></a>Tűzfalszabály módosítása 
Meglévő tűzfalszabály hello server használatával frissítse [az postgres server tűzfalszabály frissítés](/cli/azure/postgres/server/firewall-rule#update) parancsot. Adja meg a meglévő tűzfalszabály hello bemeneti és a hello kezdő IP-cím és a záró IP attribútumok tooupdate hello nevét.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
Sikeres, akkor a hello parancs kimenetében frissítette, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit sorolja fel. Ha hiba történik, a hello helyette kimeneti showserror üzenet szövegét.
> [!NOTE]
> Ha hello tűzfalszabály nem létezik, az hello frissítés parancs végrehajtásakor létrejön.

## <a name="show-firewall-rule-details"></a>Tűzfal szabály részleteinek megjelenítése
Is megjelenítheti hello meglévő tűzfal kiszolgáló részletei futtatásával [az postgres server tűzfalszabály megjelenítése](/cli/azure/postgres/server/firewall-rule#show) parancsot.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Sikeres, akkor a hello parancs kimenetében megadott, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit sorolja fel. Ha hiba történik, a hello helyette kimeneti showserror üzenet szövegét.

## <a name="delete-firewall-rule"></a>Tűzfalszabály törlése
toorevoke hozzáférést egy IP-címtartomány hello kiszolgálóról meglévő tűzfalszabály törlése a következő futtatásával hello [az postgres-tűzfalszabály törlése](/cli/azure/postgres/server/firewall-rule#delete) parancsot. Adja meg a meglévő tűzfalszabály hello hello nevét.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Sikeres, akkor nincs nincs kimenete. Hiba esetén hello hibaüzenet-szöveg adja vissza.

## <a name="next-steps"></a>Következő lépések
- Hasonlóképpen, használhatja a webböngésző túl[hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok hello Azure-portál használatával](howto-manage-firewall-using-portal.md)
- Több megismerkedett [PostgreSQL-kiszolgáló tűzfalszabályainak az Azure-adatbázis](concepts-firewall-rules.md)
- Az Azure-adatbázis tooan PostgreSQL-kiszolgálóhoz csatlakozó útmutatásért lásd: [adatkapcsolattárak PostgreSQL Azure-adatbázis](concepts-connection-libraries.md)
