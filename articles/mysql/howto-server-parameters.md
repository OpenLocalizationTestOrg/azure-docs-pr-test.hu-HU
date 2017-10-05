---
title: "Kiszolgáló adatait az Azure-adatbázis konfigurálása MySQL |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure-adatbázisban elérhető kiszolgáló paraméterek konfigurálása MySQL az Azure portál használatával."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/19/2017
ms.openlocfilehash: 718bbf359253849fbc989c563ffd6d1099f92348
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-using-the-azure-portal"></a>Kiszolgáló adatait az Azure-adatbázis konfigurálása MySQL az Azure portál használatával

Azure MySQL-adatbázis támogatja az egyes kiszolgáló-paraméterek konfigurációja. Ez a cikk azt ismerteti, hogyan konfigurálhatja ezeket a paramétereket az Azure portál használatával, és felsorolja a használható paraméterek, az alapértelmezett értékeket, valamint az érvényes értékek tartományán. Nem minden kiszolgáló paraméterek módosítható. Az itt felsorolt megfelelően támogatottak.

## <a name="navigate-to-server-parameters-blade-on-azure-portal"></a>Navigáljon az Azure-portál (Server paraméterek) panelen

Jelentkezzen be az Azure-portálon, majd kattintson az Azure-adatbázis MySQL a kiszolgálónév. Az a **beállítások** területén kattintson **Server paraméterek** Server paraméterek panel megnyitásához az Azure-adatbázis a MySQL.

![Az Azure portál kiszolgálójának (paraméterek) panelen](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>A kiszolgáló konfigurálható paraméterek listája

A következő táblázat felsorolja a jelenleg támogatott kiszolgálói paramétereket. A paraméterek konfigurálhatja az alkalmazás követelményeinek megfelelően.

> [!div class="mx-tableFixed"]
|Paraméter neve|Alapértelmezett érték|tartomány|Leírás|
|---|---|---|---|
|binlog_group_commit_sync_delay|1000|0, 11-1000000|Vezérlők hány ezredmásodperc bináris napló véglegesítése vár lemez bináris fájl szinkronizálása előtt.|
|binlog_group_commit_sync_no_delay_count|0|0-1000000|Az aktuális késleltetési binlog-csoport-commit-sync-késleltetés által megadott megszakítása előtt várja meg a tranzakciók maximális száma.|
|character_set_server|LATIN1|BIG5, UTF8MB4, stb.|Az alapértelmezett kiszolgáló karakterkészlet charset_name használják.|
|div_precision_increment|4|0-30|A skála osztás műveletek a eredményének növelésének számjegyek száma.|
|group_concat_max_len|1024|4-16777216|Esetén a GROUP_CONCAT() eredmények maximális engedélyezett hosszát.|
|innodb_adaptive_hash_index|ON|KIKAPCSOLVA|E innodb adaptív kivonatindexek engedélyezésekor vagy letiltásakor.|
|innodb_lock_wait_timeout|50|1-3600|Az időtartamot másodpercben egy InnoDB tranzakció megvárja-e a egy sor zárolása előtt próbálkozik.|
|interactive_timeout|1800|10-1800|A kiszolgáló megvárja-e a Bezárás előtt interaktív kapcsolaton tevékenység másodpercek számát.|
|log_bin_trust_function_creators|KIKAPCSOLVA|KIKAPCSOLVA|Ez a változó érvényes, ha a bináris naplózás engedélyezve van. Azt szabályozza, hogy tárolt függvény creators tárolt függvények, amelyek a bináris naplóban írandó nem biztonságos események létrehozásához nem lesz megbízható.|
|log_queries_not_using_indexes|KIKAPCSOLVA|KIKAPCSOLVA|Naplózza a lekérdezések, amelyek várhatóan lassú lekérdezés napló minden sorok beolvasása.|
|log_slow_admin_statements|KIKAPCSOLVA|KIKAPCSOLVA|Lassú felügyeleti utasításokat tartalmaz az utasításokban, a lassú lekérdezés naplójába.|
|log_throttle_queries_not_using_indexes|0|0-4294967295|Korlátozza az ilyen lekérdezések / perc, a lassú lekérdezési naplóba írt.|
|long_query_time|10|1-1E + 100|Ha a lekérdezés hosszabb időbe telik a hány másodpercig, a kiszolgáló növeli a Slow_queries állapot változó.|
|max_allowed_packet|536870912|1024-1073741824|A mysql_stmt_send_long_data() C API-függvénye küldött egy csomag vagy a bármely generált/közbenső karakterlánc, illetve bármely paraméter maximális méretét.|
|min_examined_row_limit|0|0-18446744073709551615|Naplózza a lekérdezések, amelyek nagyobb, mint a konfigurált számú sorok lassú lekérdezési naplóba. |
|server_id|3293747068|1000-4294967295|A kiszolgáló azonosítója, egyes fő és egy egyedi azonosító slave replikációs szerepel.|
|slave_net_timeout|60|30-3600|Várjon további adatait a master az alárendelt úgy ítéli meg, a kapcsolat megszakadt, mielőtt megszakítja az Olvasás, és száma kapcsolatlétesítési.|
|slow_query_log|KIKAPCSOLVA|KIKAPCSOLVA|Engedélyezi vagy letiltja a lassú lekérdezés naplót.|
|sql_mode|kijelölt 0|ALLOW_INVALID_DATES, IGNORE_SPACE, stb.|Az aktuális kiszolgáló SQL mód.|
|TIME_ZONE|RENDSZER|Példák:-8: 00, +05: 30|A kiszolgáló időzóna.|
|wait_timeout|120|60-240|A kiszolgáló pedig a Bezárás előtt nem interaktív kapcsolat tevékenység másodpercek számát.|

## <a name="next-steps"></a>Következő lépések
- [Adatkapcsolattárak MySQL az Azure-adatbázis](concepts-connection-libraries.md)
