---
title: "-Kiszolgáló naplóit Azure-adatbázisban PostgreSQL |} Microsoft Docs"
description: "PostgreSQL az Azure-adatbázis lekérdezés és a hiba naplókat hoz létre."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 10f6c490a4fdb4c70cb80b035eaebb9d2ad2e699
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>-Kiszolgáló naplóit Azure-adatbázisban PostgreSQL 
Azure PostgreSQL-adatbázishoz lekérdezés és a hiba generál naplókat. Tranzakciós naplók elérése azonban nem támogatott. Ezek a naplók segítségével azonosíthatja, hibaelhárításához és konfigurációs hibák és optimális teljesítményének javítása. További információkért lásd: [jelentéskészítési és naplózási hiba](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Hozzáférést kiszolgálónaplókban
Listáról, és töltse le az Azure PostgreSQL server hibanaplójában talál az Azure portál használatával [Azure CLI](howto-configure-server-logs-using-cli.md) és Azure REST API-k.

## <a name="log-retention"></a>Napló megőrzési
A megőrzési időtartam állíthatja be, a rendszer naplóz, használja a **napló\_megőrzési\_időszak** a kiszolgálóhoz társított paraméter. Ez a paraméter a mértékegysége nap (megerősítéséhez szükséges). Az alapértelmezett érték három nap. A maximális értéke a 7 nap. Vegye figyelembe, hogy a kiszolgáló kell van-e elegendő lefoglalt megtartott naplófájlokat tartalmazó tároló.
A naplófájlok fog elforgatása minden 1 óra vagy 100 MB-os méretet, amelyik előbb következik be.

## <a name="configure-logging-for-azure-postgresql-server"></a>Azure PostgreSQL-kiszolgáló naplózásának konfigurálása
A kiszolgáló használatával engedélyezheti a lekérdezések naplózása és a hibanaplózás. A hibanaplókban automatikus vákuumszivattyú, kapcsolat és ellenőrzőpontokat információt tartalmazhat.

Lekérdezés naplózása engedélyezhető a PostgreSQL DB példány úgy, hogy két kiszolgáló paraméterek: napló\_utasítás és a naplófájlok\_min\_időtartam\_utasítás.

A **napló\_utasítás** paraméter határozza meg, melyik SQL-utasítások naplózza. Javasoljuk, hogy ha a paramétert a ***összes*** összes utasítást; bejelentkezni az alapértelmezett érték: nincs (megerősítéséhez szükséges).

A **napló\_min\_időtartam\_utasítás** paraméter értéke ezredmásodpercet fejez be kell jelentkezniük utasítás állítja be a korlátot. Összes SQL-utasítások futtatása hosszabb, mint a paraméter értéke a rendszer naplózza. Ez a paraméter le van tiltva, és (szükség megerősítése) alapértelmezés szerint mínuszjel (-1) 1 értékre. Ez a paraméter engedélyezése az alkalmazások nem optimalizált lekérdezéseket nyomkövetési hasznos lehet.

A **napló\_min\_üzenetek** lehetővé teszi, hogy a kiszolgáló naplóját írt szabályozhatja, hogy mely szintek jelenik meg. Az alapértelmezett figyelmet IGÉNYEL. (megerősítéséhez szükséges)

A beállításokkal további információkért lásd: [jelentéskészítési és naplózási hiba](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) dokumentációját. Azure-adatbázis különösen konfigurálása PostgreSQL-kiszolgáló adatait, tekintse meg [Server PostgreSQL az Azure Database-naplók](concepts-server-logs.md).

## <a name="next-steps"></a>Következő lépések
- Hozzáférés az Azure CLI parancssori felület használatával a naplókat, lásd: [beállítása és hozzáférést kiszolgálónaplókban olvashatók Azure parancssori felület használatával](howto-configure-server-logs-using-cli.md)
- A kiszolgáló paraméterek további információkért lásd: [testre szabhatja a kiszolgáló konfigurációs paraméterek Azure parancssori felület használatával](howto-configure-server-parameters-using-cli.md).