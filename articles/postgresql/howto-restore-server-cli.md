---
title: "Hogyan tooback össze, és visszaállíthatja egy kiszolgálóhoz az Azure-adatbázis PostgreSQL |} Microsoft Docs"
description: "Ismerje meg, hogyan tooback mentése és visszaállítása egy kiszolgálóhoz az Azure-adatbázis a PostgreSQL használatával hello Azure parancssori felület."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 0b9ed25e3e3a88dd5c7ffe2ae7c27f8eef9be710
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooback-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-hello-azure-cli"></a>Hogyan tooback mentése és visszaállítása egy kiszolgálóhoz az Azure-adatbázis a PostgreSQL használatával hello Azure parancssori felület

Azure-adatbázist használ a kiszolgáló adatbázis tooan PostgreSQL toorestore too35 7 napról kiterjedő korábbi dátumot.

## <a name="prerequisites"></a>Előfeltételek
toocomplete e-tooguide, hogyan lehet szüksége:
- Egy [PostgreSQL-kiszolgáló és adatbázis Azure-adatbázis](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Telepítésekor, és hello Azure CLI helyileg használja, ez hogyan tooguide használatához Azure CLI 2.0-s vagy újabb verziója. Adja meg a tooconfirm hello verziója, a parancssorba hello Azure CLI `az --version`. tooinstall vagy frissítés, lásd: [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

## <a name="back-up-happens-automatically"></a>Készítsen biztonsági másolatot automatikusan történik,
Amikor PostgreSQL Azure-adatbázist használ, hello adatbázis-szolgáltatás automatikusan teszi hello szolgáltatás biztonsági másolatot, 5 percenként. 

Az alapszintű csomag hello biztonsági mentések érhetők el a 7 napja. Standard szint hello biztonsági mentések érhetők el 35 napon. További információkért lásd: [Azure-adatbázis a tarifacsomagok PostgreSQL](concepts-service-tiers.md).

Az automatikus biztonsági mentési szolgáltatással hello kiszolgáló és az adatbázisok tooan visszaállítása korábbi dátumot, vagy mikor.

## <a name="restore-a-database-tooa-previous-point-in-time-by-using-hello-azure-cli"></a>Állítsa vissza egy adatbázis tooa korábbi időpontra időben hello Azure parancssori felület használatával
Az Azure Database PostgreSQL toorestore hello server tooa előző pont időben. hello visszaállított adatok másolt tooa új kiszolgálóra, és hello meglévő kiszolgáló marad, mert a. Például egy táblázat véletlenül megszakadása ma órakor, toohello idő előtt déltől helyreállíthatja. Ezután le hello hiányzik a táblázat, és vissza hello példányát hello kiszolgáló adatait. 

toorestore hello kiszolgáló, az Azure parancssori felület használata hello [az postgres kiszolgálójának visszaállítását](/cli/azure/postgres/server#restore) parancsot.

### <a name="run-hello-restore-command"></a>Hello visszaállítási parancs futtatása

toorestore hello server parancssorba hello Azure CLI adja meg a következő parancs hello:

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

Hello `az postgres server restore` parancshoz szükséges a következő paraméterek hello:
| Beállítás | Ajánlott érték | Leírás  |
| --- | --- | --- |
| Erőforráscsoport |  myResourceGroup |  Az erőforráscsoport, ahol a forráskiszolgáló hello van.  |
| név | mypgserver visszaállítása | új kiszolgáló hello hello visszaállítási parancs által létrehozott hello neve. |
| visszaállítás--időpontban | 2017-04-13T13:59:00Z | Jelölje ki a pont idő toorestore számára. A dátum és idő hello forráskiszolgáló készítsen biztonsági másolatot a megőrzési időn belül kell lennie. Használjon hello ISO8601 dátum és idő formátumban. Például használhatja a saját helyi időzóna, például a `2017-04-13T05:59:00-08:00`. Is használhatja a UTC Zulu formátumban, például hello `2017-04-13T13:59:00Z`. |
| forrás-kiszolgáló | mypgserver-20170401 | hello nevét vagy hello forrás server toorestore az azonosítója. |

Amikor egy kiszolgáló tooan helyreállítja idő korábbi pont, egy új kiszolgáló akkor jön létre. hello eredeti kiszolgáló és az adatbázisok hello van megadva időpont: másolt toohello új kiszolgálóra.

hello helyét és árképzési szint értékek a visszaállított hello-kiszolgálók hello azonos hello eredeti kiszolgálóként. 

Hello `az postgres server restore` parancs szinkron. Miután visszaállította hello kiszolgáló, azt újra használhatná toorepeat hello folyamat egy másik pont időben. 

Hello után állítsa vissza a folyamat befejeződik, keresse meg az új kiszolgáló hello, és győződjön meg arról, hogy hello adatok visszaállítása, az elvártaknak megfelelően.

## <a name="next-steps"></a>Következő lépések
[Adatkapcsolattárak PostgreSQL Azure-adatbázis](concepts-connection-libraries.md)
