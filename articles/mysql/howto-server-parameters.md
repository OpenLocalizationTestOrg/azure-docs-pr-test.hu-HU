---
title: "aaaHow tooConfigure Server paraméterek MySQL az Azure-adatbázis |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure elérhető kiszolgáló adatait az Azure Database MySQL használatára vonatkozó hello Azure-portálon."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/19/2017
ms.openlocfilehash: 8292c8eda605854a06b6a9c82185c857bd353cfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-server-parameters-in-azure-database-for-mysql-using-hello-azure-portal"></a>Hogyan MySQL az Azure-adatbázis tooconfigure kiszolgáló paramétereit hello Azure-portálon

Azure MySQL-adatbázis támogatja az egyes kiszolgáló-paraméterek konfigurációja. Ez a cikk ismerteti, hogyan tooconfigure ezek a paraméterek hello Azure-portálon, és listák hello támogatott paraméterek hello alapértékeket és hello érvényes értékek tartományán. Nem minden kiszolgáló paraméterek módosítható. Csak az itt felsorolt ők hello támogatott.

## <a name="navigate-tooserver-parameters-blade-on-azure-portal"></a>Keresse meg a tooServer (paraméterek) panelen az Azure-portálon

Jelentkezzen be Azure-portálon toohello, majd kattintson az Azure-adatbázis MySQL a kiszolgálónév. A hello **beállítások** területén kattintson **Server paraméterek** tooopen hello kiszolgáló (paraméterek) panelen a hello MySQL az Azure-adatbázis.

![Az Azure portál kiszolgálójának (paraméterek) panelen](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>A kiszolgáló konfigurálható paraméterek listája

a következő táblázat hello támogatott hello server paramétereket ismerteti. hello paraméterek tooyour alkalmazás követelmények szerint konfigurálható.

> [!div class="mx-tableFixed"]
|Paraméter neve|Alapértelmezett érték|tartomány|Leírás|
|---|---|---|---|
|binlog_group_commit_sync_delay|1000|0, 11-1000000|Vezérlők hány ezredmásodperc hello bináris napló véglegesítési vár hello bináris napló fájl toodisk szinkronizálása előtt.|
|binlog_group_commit_sync_no_delay_count|0|0-1000000|a tranzakciók toowait az aktuális késleltetési hello binlog-csoport-commit-sync-késleltetés által megadott megszakítása előtt hello maximális számát.|
|character_set_server|LATIN1|BIG5, UTF8MB4, stb.|Hello alapértelmezett server karakterkészlet charset_name használják.|
|div_precision_increment|4|0-30|Hello eredmény osztás műveletek, melyik tooincrease hello skála számjegyek száma.|
|group_concat_max_len|1024|4-16777216|Eredmények maximális megengedett hossz hello GROUP_CONCAT() bájtokban.|
|innodb_adaptive_hash_index|ON|KIKAPCSOLVA|E innodb adaptív kivonatindexek engedélyezésekor vagy letiltásakor.|
|innodb_lock_wait_timeout|50|1-3600|hello időtartamot másodpercben egy InnoDB tranzakció megvárja-e a egy sor zárolása előtt próbálkozik.|
|interactive_timeout|1800|10-1800|Hány másodperc hello server megvárja-e a tevékenység Bezárás előtt interaktív kapcsolaton.|
|log_bin_trust_function_creators|KIKAPCSOLVA|KIKAPCSOLVA|Ez a változó érvényes, ha a bináris naplózás engedélyezve van. Azt szabályozza, hogy creators lehet tárolt függvény nem toohello bináris napló írása nem biztonságos események toobe okozó tárolt toocreate funkciók megbízható-e.|
|log_queries_not_using_indexes|KIKAPCSOLVA|KIKAPCSOLVA|Naplók kérelmeket, amelyek várt tooretrieve összes sort tooslow lekérdezés naplót.|
|log_slow_admin_statements|KIKAPCSOLVA|KIKAPCSOLVA|Lassú rendszergazdai utasítások tartalmaznak toohello lassú lekérdezés napló írása hello utasításokban.|
|log_throttle_queries_not_using_indexes|0|0-4294967295|Korlátok hello írható toohello lassú lekérdezés napló percenként ilyen lekérdezések száma.|
|long_query_time|10|1-1E + 100|Ha a lekérdezés hosszabb időbe telik a hány másodpercig, hello server növeli hello Slow_queries állapot változó.|
|max_allowed_packet|536870912|1024-1073741824|hello maximális mérete egy csomag vagy bármely generált/közbenső karakterlánc vagy bármely hello mysql_stmt_send_long_data() C API-függvénye által küldött paramétert.|
|min_examined_row_limit|0|0-18446744073709551615|Naplózza a lekérdezések, amelyek nagyobb, mint a sorok hello lassú lekérdezési naplóba hello konfigurálva. |
|server_id|3293747068|1000-4294967295|hello Azonosítóját, a replikáció toogive használt egyes fő, és slave egy egyedi azonosító.|
|slave_net_timeout|60|30-3600|hello száma másodpercenként toowait hello főkiszolgálóról hello alárendelt tekinti hello a kapcsolat megszakadt, mielőtt további adatok olvasása hello megszakítja, és megpróbál tooreconnect.|
|slow_query_log|KIKAPCSOLVA|KIKAPCSOLVA|Engedélyezi vagy letiltja a hello lassú lekérdezés napló.|
|sql_mode|kijelölt 0|ALLOW_INVALID_DATES, IGNORE_SPACE, stb.|hello kiszolgáló SQL üzemmódot.|
|TIME_ZONE|RENDSZER|Példák:-8: 00, +05: 30|hello server időzóna.|
|wait_timeout|120|60-240|hello száma másodpercenként hello server megvárja-e a tevékenység a Bezárás előtt nem interaktív kapcsolat.|

## <a name="next-steps"></a>Következő lépések
- [Adatkapcsolattárak MySQL az Azure-adatbázis](concepts-connection-libraries.md)
