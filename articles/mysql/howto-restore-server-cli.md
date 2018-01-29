---
title: "Biztonsági mentése és visszaállítása egy kiszolgálóhoz az Azure-adatbázis MySQL |} Microsoft Docs"
description: "Útmutató: biztonsági mentése és visszaállítása egy kiszolgálóhoz az Azure-adatbázis a MySQL az Azure parancssori felület használatával."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/28/2017
ms.openlocfilehash: 44b3c68b8df4006d3fe087e5ad4118d7616d3d9a
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-by-using-the-azure-cli"></a>Biztonsági mentése és visszaállítása egy kiszolgálóhoz az Azure-adatbázis MySQL az Azure parancssori felület használatával

Egy kiszolgáló-adatbázis visszaállítása korábbi dátumra 35 nap 7 kiterjedő MySQL Azure adatbázis használata.

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató útmutató befejezéséhez lesz szüksége:
- Egy [a MySQL-kiszolgáló és az adatbázis Azure-adatbázis](quickstart-create-mysql-server-database-using-azure-portal.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Telepítésekor és az Azure parancssori felület helyileg használja, ez az útmutató útmutató használatához Azure CLI 2.0-s vagy újabb verziója. Erősítse meg a verzió, az Azure CLI parancssorba írja be a következőt `az --version`. Telepítéséhez vagy frissítéséhez, lásd: [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

## <a name="backup-happens-automatically"></a>Automatikusan megtörténik a biztonsági mentés
MySQL az Azure-adatbázis használata esetén az adatbázis-szolgáltatás automatikusan révén a szolgáltatás biztonsági másolatot, 5 percenként. 

Az alapszintű csomag a biztonsági mentések érhetők el 7 napban. Standard csomagra a biztonsági mentések érhetők el 35 napon. További információkért lásd: [tarifacsomagok MySQL az Azure-adatbázis](concepts-service-tiers.md).

Az automatikus biztonsági mentési szolgáltatással a kiszolgáló és az adatbázisok visszaállítása korábbi dátumra, vagy mikor.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Adatbázis visszaállítása egy korábbi időpontra időben az Azure parancssori felület használatával
A kiszolgáló helyreállítása egy korábbi időpontra időben a MySQL az Azure-adatbázis használatával. A visszaállított adatok másolását egy új kiszolgálóra, és a meglévő kiszolgáló marad, mert a. Például egy táblázat véletlenül megszakadása ma órakor, visszaállíthatja az idő előtt déltől. Ezt követően beolvasható a hiányzó táblázat és az adatokat a kiszolgáló visszaállított példányát. 

A kiszolgáló visszaállításához használja az Azure parancssori felület [az mysql-kiszolgálójának visszaállítását](/cli/azure/mysql/server#az_mysql_server_restore) parancsot.

### <a name="run-the-restore-command"></a>A restore parancs futtatása

Állítsa vissza a kiszolgáló, az Azure CLI parancssorba írja be a következő parancsot:

```azurecli-interactive
az mysql server restore --resource-group myResourceGroup --name myserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server myserver4demo
```

A `az mysql server restore` parancs paraméterei a következők:
| Beállítás | Ajánlott érték | Leírás  |
| --- | --- | --- |
| Erőforráscsoport | myResourceGroup |  Az erőforráscsoport, ahol a forráskiszolgáló található.  |
| név | myserver visszaállítása | A restore parancs által létrehozott új kiszolgáló neve. |
| visszaállítás--időpontban | 2017-04-13T13:59:00Z | Válasszon ki egy pontot időben történő visszaállításához. A dátum és idő a forráskiszolgáló biztonsági mentés megőrzési időn belül kell lennie. Dátum és idő ISO8601 formátumot használja. Például használhatja a saját helyi időzóna, például a `2017-04-13T05:59:00-08:00`. Használhatja a UTC Zulu formátumban, például `2017-04-13T13:59:00Z`. |
| forrás-kiszolgáló | myserver4demo | Név vagy azonosító a forráskiszolgáló visszaállítása. |

Időben egy korábbi állapotba szeretné visszaállítani egy kiszolgálót, létrejön egy új kiszolgálót. Az eredeti kiszolgáló és az idő megadott pontjáról adatbázisainak az új kiszolgálóra történő átmásolása.

A hely és a visszaállított kiszolgáló árképzési szint értékek továbbra is ugyanaz, mint az eredeti kiszolgálón. 

A `az mysql server restore` parancs szinkron. Miután visszaállította a kiszolgáló, segítségével azt újra meg kell ismételni a különböző ponton időben. 

A visszaállítási folyamat befejezése után keresse meg az új kiszolgálón, és győződjön meg arról, hogy az adatok visszaállítása, az elvártaknak megfelelően.

## <a name="next-steps"></a>Következő lépések
[Adatkapcsolattárak MySQL az Azure-adatbázis](concepts-connection-libraries.md)
