---
title: "Azure-adatbázis PostgreSQL-kiszolgáló tűzfalszabályainak |} Microsoft Docs"
description: "Tűzfalszabályok ismerteti az Azure-adatbázis PostgreSQL-kiszolgáló számára."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 1d46a4434c483c3612a9a7b4cdef718d6dc3e765
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-server-firewall-rules"></a>Azure-adatbázis PostgreSQL-kiszolgáló tűzfalszabályainak
Tűzfalak tooyour adatbázis-kiszolgáló minden hozzáférés tiltása, amíg megadhatja, hogy mely számítógépek rendelkeznek engedéllyel. hello tűzfal engedélyezi a hozzáférést toohello kiszolgáló IP-cím az egyes kérelmek származó hello alapján.
tooconfigure tűzfalszabályok elfogadható IP-címek tartományát adja meg a tűzfal létrehozása. Hello kiszolgálószintű tűzfal-szabályokat hozhat létre.

**Tűzfal-szabályok:** ezeket a szabályokat a teljes Azure-adatbázis engedélyezze ügyfelek tooaccess PostgreSQL-kiszolgálón, ez azt jelenti, hogy az összes hello adatbázis belül hello azonos logikai kiszolgáló. Kiszolgálószintű tűzfal-szabályokat hello Azure-portál használatával vagy az Azure parancssori felület parancsait konfigurálhatja. szabályok toocreate kiszolgálószintű tűzfal, hello előfizetés tulajdonosának vagy egy előfizetés közreműködőjének kell lennie.

## <a name="firewall-overview"></a>Tűzfal áttekintése
Az összes adatbázis hozzáférés tooyour Azure adatbázis PostgreSQL-kiszolgáló alapértelmezés szerint hello tűzfal blokkolja. toobegin a kiszolgálót egy másik számítógépre, a használatával kell toospecify egy vagy több kiszolgálószintű tűzfal szabályok tooenable tooyour kiszolgáló. Hello tűzfal szabályok toospecify mely IP-címtartományt a hello Internet tooallow használja. Hello tűzfalszabályok ne legyenek hatással Access toohello magát az Azure portál.
A kapcsolódási kísérletek hello Internet, és Azure először haladnak keresztül hello tűzfal ahhoz, azok képes elérni a PostgreSQL-adatbázisban, ahogy az ábra a következő hello:

![Példa áramló hello tűzfal működése](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-hello-internet"></a>A hello Internet csatlakoztatása
Kiszolgálószintű tűzfal-szabályokat alkalmazni tooall adatbázisok hello Azure adatbázis PostgreSQL-kiszolgáló. Ha hello IP-cím hello kérelem hello kiszolgálószintű tűzfal szabályokban megadott hello tartományok egyikébe esik, hello kapcsolat kapnak.
Ha hello IP-cím hello kérelem nincs belül hello tartományok hello adatbázis szintű valamelyikében vagy megadott kiszolgálószintű tűzfal-szabályok, hello kapcsolódási kérelem sikertelen lesz.
Például ha az alkalmazás a PostgreSQL JDBC-illesztőt csatlakozik, a hiba történt a tooconnect, amikor hello tűzfal blokkolja a hello kapcsolat találkozhat.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: végzetes: nincs pg\_hba.conf bejegyzés állomás "123.45.67.890", a felhasználó "adminuser", az adatbázis "postgresql", SSL

## <a name="programmatically-managing-firewall-rules"></a>Tűzfalszabályok szoftveres kezelése
Emellett Azure-portálon, a szabályok lehetnek tűzfal toohello felügyelt programozott módon az Azure parancssori felület használatával.
Lásd még: [hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok Azure parancssori felület használatával](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-hello-database-firewall"></a>Hello adatbázis tűzfal hibaelhárítása
Vegye figyelembe a következő pontokat, amikor hozzáférési toohello Microsoft Azure-adatbázis PostgreSQL-kiszolgáló szolgáltatás nem a várt módon működnek várt hello:

* **Módosítások toohello engedélyezési lista rendelkezik még nincsenek érvényben:** lehet, mint amennyit egy 5 perces késleltetést használ az Azure-adatbázis toohello PostgreSQL-kiszolgáló tűzfal konfigurációs tootake hatás változik.

* **hello bejelentkezési nincs engedélyezve, vagy helytelen jelszót használt:** egy bejelentkezési nincs engedélye az hello Azure Database-kiszolgáló vagy hello használt jelszó PostgreSQL helytelen az, ha hello Azure-adatbázis kapcsolati toohello PostgreSQL Server megtagadva. Csak egy tűzfal-beállítás létrehozásához biztosít az ügyfelek számára egy lehetőség tooattempt tooyour server; kapcsolódás minden ügyfél hello szükséges biztonsági adatokat kell megadni.

Például egy JDBC-ügyfélprogram segítségével hello a következő hiba jelenhet meg.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: végzetes: "felhasználónév" felhasználói jelszó hitelesítése sikertelen.

* **Dinamikus IP-cím:** Ha az internethez, a dinamikus IP-címzést és tapasztal hello tűzfalon keresztül történt, próbálkozzon a következő megoldások hello egyikét:

* Kérje meg az internetszolgáltató (ISP) hello IP-címtartomány hozzárendelt tooyour ügyfélszámítógépek számára, hogy hozzáférési hello Azure adatbázis PostgreSQL-kiszolgáló, és adja hozzá a hello IP-címtartomány tűzfal szabály.

* Első statikus IP-címzés helyette az ügyfélszámítógépek számára, és adja hozzá a hello IP-címek, tűzfal-szabályokat.

## <a name="next-steps"></a>Következő lépések
A kiszolgáló- és adatbázis tűzfal-szabályok létrehozása a cikkekben talál:
* [Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok hello Azure-portál használatával](howto-manage-firewall-using-portal.md)
* [Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok Azure parancssori felület használatával](howto-manage-firewall-using-cli.md)