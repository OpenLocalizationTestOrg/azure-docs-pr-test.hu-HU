---
title: "aaaCreate és kezelheti az Azure-adatbázis MySQL tűzfalszabályok Azure parancssori felületével |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate és kezelheti az Azure-adatbázis használata az Azure CLI parancssori MySQL tűzfalszabályok."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: b2f0b4ccf34ce88e3a5e72a64d3f8c78a5bc2a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a>Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok Azure parancssori felület használatával
Kiszolgálószintű tűzfal-szabályokat engedélyezhetik a rendszergazdák toomanage hozzáférés tooan Azure adatbázis MySQL-kiszolgáló egy adott IP-cím vagy az IP-címek. Kényelmes Azure parancssori felület parancsait, segítségével létrehozása, frissítése, törlése, listában, és tűzfal-szabályok toomanage megjelenítése a kiszolgálón. Az áttekintést az Azure-adatbázis MySQL tűzfalak, lásd: [a MySQL-kiszolgáló tűzfalszabályainak Azure-adatbázis](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Előfeltételek
* [Az Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli)
* Telepítse az Azure Python SDK PostgreSQL és a MySQL-szolgáltatások
* Hello Azure CLI összetevő PostgreSQL és a MySQL-szolgáltatások telepítése
* Azure-adatbázis létrehozása MySQL-kiszolgálóhoz

## <a name="firewall-rule-commands"></a>A tűzfalszabály parancsok:
Hello **az mysql-tűzfalszabályt** a parancsnak az Azure CLI toocreate törlése, a listában, megjelenítése, és tűzfalszabályainak frissítése.

Parancsok:
- **Hozzon létre**: hozzon létre egy Azure-beli MySQL tűzfalszabály létrehozása.
- **Törlés**: az Azure-beli MySQL tűzfalszabály törlése.
- **lista** : hello Azure MySQL kiszolgáló tűzfalszabályainak listában.
- **megjelenítése** : hello részletek az Azure-beli MySQL-kiszolgálók tűzfalszabály megjelenítése.
- **frissítés**: az Azure-beli MySQL tűzfalszabály módosítása.

## <a name="login-tooazure-and-list-your-azure-database-for-mysql-servers"></a>Bejelentkezési tooAzure és a MySQL-kiszolgálók az Azure-adatbázis listája
Az Azure parancssori felület biztonságosan kapcsolódnak az Azure-fiókjával. Használjon hello **az bejelentkezési** toodo ez parancsot.

1. Futtassa a parancsot követő hello parancssorból hello.
```azurecli
az login
```
Ez a parancs egy kódot toouse hello következő lépésben lesz kimeneti.

2. Egy webes böngésző tooopen hello lapon [https://aka.ms/devicelogin](https://aka.ms/devicelogin) , és írja be a kódját hello.

3. Hello parancssorba jelentkezzen be Azure hitelesítő adatait.

4. A bejelentkezési azonosító jogosult, ha előfizetések listája kinyomtatja hello konzolon. Másolja a szükséges hello előfizetés tooset hello aktuális előfizetés toobe használt soron hello azonosítója.
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. Ha bizonytalan hello nevek hello Azure adatbázisok előfizetésbe és erőforráscsoportba csoport MySQL-kiszolgálók listája

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Vegye figyelembe a hello name attribútum hello listázása, ez az használt toospecify mely MySQL server toowork meg. Ha szükséges, az adott kiszolgáló toousing hello name attribútum tooconfirm nevének helyességéről, győződjön meg arról, hogy hello részletek:

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Tűzfalszabályok listája a MySQL-kiszolgálóhoz tartozó Azure-adatbázis 
Hello kiszolgálónevet és hello az erőforráscsoport neve, a lista hello meglévő kiszolgáló tűzfalszabályainak hello kiszolgálón használata. Figyelje meg, hogy hello server name attribútum van megadva a hello **--kiszolgáló** váltson, és nem hello **--name** váltani.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
hello kimeneti hello szabályok felsorolja, ha vannak ilyenek, alapértelmezés szerint a JSON formátumban. Hello kapcsoló segítségével **--eredménytábla** a tábla hello kimenetként olvashatóbb formátumban.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a>Tűzfalszabály létrehozásához Azure-adatbázis a MySQL-kiszolgáló
Hello Azure-beli MySQL kiszolgáló nevét és az erőforráscsoport neve hello segítségével hozzon létre egy új tűzfalszabály hello kiszolgálón. Nevezze el a hello szabály, a hello kezdő IP- és a záró IP hello szabály toocover tooallow hozzáférést egy adott IP-címek számára.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Egyes IP címek toobe hozzáférése engedélyezett, adja meg ugyanazt a címet hello hello kezdő IP- és a záró IP-, ebben a példában szemléltetett módon.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Sikeres, akkor hello parancs kimenetében megjelennek létrehozott, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit. Ha hiba történik, hello kimenet megjeleníti hibaüzenet-szöveg helyette.

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a>Az Azure Database-tűzfalszabály MySQL-kiszolgáló frissítése 
A hello Azure MySQL kiszolgálónevet és hello erőforráscsoport-név, frissítse a meglévő tűzfalszabály hello kiszolgálón. Adja meg a meglévő tűzfalszabály hello bemeneti és a hello kezdő IP-cím és a záró IP attribútumok tooupdate hello nevét.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Sikeres, akkor hello parancs kimenetében megjelennek frissítette, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit. Ha hiba történik, hello kimenet megjeleníti hibaüzenet-szöveg helyette.

> [!NOTE]
> Ha hello tűzfalszabály nem létezik, akkor hello frissítés parancs létrejön.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Azure-adatbázis a tűzfalszabály részletei MySQL kiszolgáló megjelenítése
A hello Azure MySQL kiszolgálónevet és hello erőforráscsoport-név, megjelenítése hello meglévő tűzfalszabály részletei hello kiszolgálóról. Meglévő tűzfalszabály hello hello neve meg bemeneti adatként.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Sikeres, akkor hello parancs kimenetében megjelennek megadott, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit. Ha hiba történik, hello kimenet megjeleníti hibaüzenet-szöveg helyette.

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a>Törli a tűzfalszabályt Azure-adatbázis a MySQL-kiszolgáló
A hello Azure MySQL kiszolgálónevet és hello erőforráscsoport-név, távolítsa el a meglévő tűzfalszabály hello kiszolgálóról. Adja meg a meglévő tűzfalszabály hello hello nevét.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Sikeres, akkor nincs nincs kimenete. Hiba esetén hello hibaüzenet-szöveg eredmény.

## <a name="next-steps"></a>Következő lépések
- Több megismerkedett [a MySQL-kiszolgáló tűzfalszabályainak Azure-adatbázis](./concepts-firewall-rules.md)
- [Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok hello Azure-portál használatával](./howto-manage-firewall-using-portal.md)
