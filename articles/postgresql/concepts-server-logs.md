---
title: "Azure-adatbázis bejelentkezésekor aaaServer PostgreSQL |} Microsoft Docs"
description: "PostgreSQL az Azure-adatbázis lekérdezés és a hiba naplókat hoz létre."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 22575f3882ce67fe640952f0a8dbfd8dcb4b16fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>-Kiszolgáló naplóit Azure-adatbázisban PostgreSQL 
Azure PostgreSQL-adatbázishoz lekérdezés és a hiba generál naplókat. Hozzáférés tootransaction naplókat azonban nem támogatott. Ezek a naplók használt tooidentify kell, hibaelhárításához és konfigurációs hibák és optimális teljesítményének javítása. További információkért lásd: [jelentéskészítési és naplózási hiba](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Hozzáférést kiszolgálónaplókban
Listáról, és töltse le az Azure PostgreSQL server hibanaplójában talál hello Azure portál használatával [Azure CLI](howto-configure-server-logs-using-cli.md) és Azure REST API-k.

## <a name="log-retention"></a>Napló megőrzési
Megőrzési idő hello segítségével a rendszer naplóit is beállíthatja a **napló\_megőrzési\_időszak** a kiszolgálóhoz társított paraméter. Ehhez a paraméterhez hello egység érték nap (tooconfirm kell). hello alapértelmezett érték három nap. hello maximális érték nem 7 nap. Vegye figyelembe, hogy a kiszolgálónak rendelkeznie kell elegendő lefoglalt tároló toocontain hello megőrzi a rendszernapló fájljaiban.
hello naplófájlok fog elforgatása minden 1 óra vagy 100 MB-os méretet, amelyik előbb következik be.

## <a name="configure-logging-for-azure-postgresql-server"></a>Azure PostgreSQL-kiszolgáló naplózásának konfigurálása
A kiszolgáló használatával engedélyezheti a lekérdezések naplózása és a hibanaplózás. A hibanaplókban automatikus vákuumszivattyú, kapcsolat és ellenőrzőpontokat információt tartalmazhat.

Lekérdezés naplózása engedélyezhető a PostgreSQL DB példány úgy, hogy két kiszolgáló paraméterek: napló\_utasítás és a naplófájlok\_min\_időtartam\_utasítás.

Hello **napló\_utasítás** paraméter határozza meg, melyik SQL-utasítások naplózza. Javasoljuk, hogy a paraméter túl***összes*** összes toolog utasítás, hello alapértelmezett értéke van none (kell tooconfirm).

Hello **napló\_min\_időtartam\_utasítás** paraméterkészletek hello korlátot, a naplózott utasítás toobe ezredmásodpercben. Hello paraméter beállítása már futó összes SQL-utasítások naplózza. Ez a paraméter (kell tooconfirm) alapértelmezés szerint ki letiltva és beállított toominus 1 (-1). Ez a paraméter engedélyezése az alkalmazások nem optimalizált lekérdezéseket nyomkövetési hasznos lehet.

Hello **napló\_min\_üzenetek** lehetővé teszi a toocontrol üzenet szintek írt toohello kiszolgáló naplóját. hello alapértelmezett figyelmet IGÉNYEL. (tooconfirm kell)

A beállításokkal további információkért lásd: [jelentéskészítési és naplózási hiba](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) dokumentációját. Azure-adatbázis különösen konfigurálása PostgreSQL-kiszolgáló adatait, tekintse meg [Server PostgreSQL az Azure Database-naplók](concepts-server-logs.md).

## <a name="next-steps"></a>Következő lépések
- Azure CLI parancssori felületen, használatával tooaccess a naplókat, lásd: [beállítása és hozzáférést kiszolgálónaplókban olvashatók Azure parancssori felület használatával](howto-configure-server-logs-using-cli.md)
- A kiszolgáló paraméterek további információkért lásd: [testre szabhatja a kiszolgáló konfigurációs paraméterek Azure parancssori felület használatával](howto-configure-server-parameters-using-cli.md).