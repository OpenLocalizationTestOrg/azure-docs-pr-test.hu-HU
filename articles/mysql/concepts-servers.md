---
title: "MySQL az Azure-adatbázis aaaServer fogalmak |} Microsoft Docs"
description: "Ez a témakör szempontjait és irányelveket MySQL-kiszolgálók Azure-adatbázishoz való munkához."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 07/06/2017
ms.openlocfilehash: 4d994cbdbde93ac5af3f4a2a7375b5851bebb1cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>Kiszolgáló kapcsolatos fogalmak, MySQL az Azure-adatbázis
Ez a témakör szempontjait és irányelveket MySQL-kiszolgálók Azure-adatbázishoz való munkához.

## <a name="what-is-an-azure-database-for-mysql-server"></a>Mi az az Azure-adatbázis MySQL-kiszolgáló?

Egy Azure MySQL-kiszolgáló adatbázisa több adatbázis egy központi felügyeleti pontot. Azonos, előfordulhat, hogy ismernie kell a helyszíni world hello MySQL server szerkezet hello. Hello Azure adatbázis MySQL szolgáltatás kifejezetten, felügyelt, teljesítmény garanciákat nyújt, elérhetővé teszi a hozzáférést és a szolgáltatások kiszolgálói szinten.

Egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis:

- Azure-előfizetés jön létre.
- Az adatbázisok hello szülő erőforrás van.
- Az adatbázisok névteret biztosít.
- A tároló erős élettartama szemantikájú - kiszolgáló törlése, és törli a hello tartalmazott adatbázisok.
- Collocates erőforrások régióban.
- A csatlakozási végpont server és adatbázis-hozzáférést biztosít.
- Felügyeleti házirendek, amelyek érvényesek a tooits adatbázisok hello hatókör biztosít: bejelentkezés, tűzfal, felhasználók, szerepkörök, konfiguráció stb.
- Több verzióban érhető el. További információkért lásd: [támogatott Azure-adatbázis MySQL-adatbázis verziók](./concepts-supported-versions.md).

A MySQL-kiszolgálóhoz létrehozott Azure-adatbázisban egy vagy több adatbázist is létrehozhat. Részt vesz egy önálló adatbázis / server tooutilize toocreate hello erőforrások, vagy hozzon létre több adatbázisok tooshare hello erőforrásokat. hello árképzési strukturált kiszolgálónként, a tarifacsomag hello konfigurációtól függően számítási egység, tárhely (GB). További információkért lásd: [Tarifacsomagok](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-tooan-azure-database-for-mysql-server"></a>Hogyan csatlakozzon és tooan Azure adatbázis MySQL-kiszolgáló hitelesítéséhez?

hello következő elemeket segítenek biztosítani a biztonságos hozzáférés tooyour adatbázis.

|||
| :-- | :-- |
| **Hitelesítés és engedélyezés** | MySQL-kiszolgálóhoz tartozó Azure-adatbázis natív MySQL-hitelesítését támogatja. Csatlakozhat, és a hello kiszolgáló-rendszergazdai bejelentkezés tooserver hitelesítéséhez. |
| **Protocol (Protokoll)** | hello szolgáltatást a MySQL által használt üzenet-alapú protokoll használatát támogatja. |
| **TCP/IP** | TCP/IP felett, és a Unix-tartomány szoftvercsatornákon keresztül hello protokoll támogatott. |
| **Tűzfal** | toohelp az adatok védelméhez, egy tűzfalszabály megakadályozza, hogy az összes tooyour adatbáziskiszolgáló vagy tooits adatbázisok, amíg azok a számítógépek megadott engedélye. Lásd: [Azure-adatbázis a MySQL-kiszolgáló tűzfalszabályainak](./concepts-firewall-rules.md). |
| **SSL** | hello szolgáltatás végrehajtó, az alkalmazások és az adatbázis-kiszolgáló közötti SSL-kapcsolatokat támogatja.  Lásd: [konfigurálása az SSL-kapcsolat az alkalmazás toosecurely a MySQL adatbázis tooAzure kapcsolati](./howto-configure-ssl.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Hogyan kezelheti a kiszolgáló?
Azure-adatbázis a MySQL-kiszolgálók kezelése a hello Azure-portál használatával, vagy az Azure parancssori felület hello.

## <a name="next-steps"></a>Következő lépések
- Hello szolgáltatás áttekintését lásd: [Azure adatbázis MySQL – áttekintés](./overview.md)
- Adott erőforrásokra vonatkozó információkból tájékoódhat kvótái és korlátai alapján a **szolgáltatásréteg**, lásd: [Szolgáltatásszinteken](./concepts-service-tiers.md)
- Csatlakozás toohello szolgáltatással kapcsolatos további információkért lásd: [MySQL az Azure-adatbázis adatkapcsolattárak](./concepts-connection-libraries.md).
