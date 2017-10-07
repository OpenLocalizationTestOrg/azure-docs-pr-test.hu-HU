---
title: "Azure adatbázisban a következő PostgreSQL aaaServer fogalmak |} Microsoft Docs"
description: "Ez a témakör szempontjait és útmutatást biztosít PostgreSQL-kiszolgálók Azure-adatbázis használata."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/06/2017
ms.openlocfilehash: 9cc7816992f2ddedd76fdf906075a723b97720a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-servers"></a>Azure-adatbázis PostgreSQL-kiszolgálók
Ez a témakör szempontjait és útmutatást biztosít PostgreSQL-kiszolgálók Azure-adatbázis használata.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Mi az az Azure-adatbázis PostgreSQL-kiszolgáló?
Egy Azure PostgreSQL-kiszolgáló adatbázisa több adatbázis egy központi felügyeleti pontot. Azonos, előfordulhat, hogy ismernie kell a helyszíni world hello PostgreSQL server szerkezet hello. Hello PostgreSQL szolgáltatás kifejezetten, felügyelt, teljesítmény garanciákat nyújt, elérhetővé teszi a hozzáférést és a szolgáltatások kiszolgálói szinten.

Egy PostgreSQL-kiszolgálóhoz tartozó Azure-adatbázis:

- Azure-előfizetés jön létre.
- Az adatbázisok hello szülő erőforrás van.
- Az adatbázisok névteret biztosít.
- A tároló erős élettartama szemantikájú - kiszolgáló törlése, és törli a hello tartalmazott adatbázisok.
- Collocates erőforrások régióban.
- A csatlakozási végpont biztosít a kiszolgáló és az adatbázis-hozzáférési (. postgresql.database.azure.com).
- Felügyeleti házirendek, amelyek érvényesek a tooits adatbázisok hello hatókör biztosít: bejelentkezés, tűzfal, felhasználók, szerepkörök, konfiguráció stb.
- Több verzióban érhető el. További információkért lásd: [PostgreSQL támogatott adatbázis-verziók](concepts-supported-versions.md).
- Az extensible felhasználók. További információkért lásd: [PostgreSQL-bővítmények](concepts-extensions.md).

Azure adatbázisban PostgreSQL-kiszolgáló egy vagy több adatbázist is létrehozhat. Részt vesz egy önálló adatbázis / server tooutilize toocreate hello erőforrások, vagy hozzon létre több adatbázisok tooshare hello erőforrásokat. hello árképzési strukturált kiszolgálónként, a tarifacsomag hello konfigurációtól függően számítási egység, tárhely (GB). További részletekért lásd: [Tarifacsomagok](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-tooan-azure-database-for-postgresql-server"></a>Hogyan csatlakozzon és tooan Azure adatbázis PostgreSQL-kiszolgáló hitelesítéséhez?
hello következő elemeket segítenek biztosítani a biztonságos hozzáférés tooyour adatbázis.

|||
| :-- | :-- |
| **Hitelesítés és engedélyezés** | Azure-adatbázis PostgreSQL-kiszolgáló natív PostgreSQL-hitelesítését támogatja. Csatlakozhat, és a hello kiszolgáló-rendszergazdai bejelentkezés tooserver hitelesítéséhez. |
| **Protocol (Protokoll)** | hello szolgáltatást egy PostgreSQL által használt üzenet-alapú protokoll használatát támogatja. |
| **TCP/IP** | TCP/IP felett, és a Unix-tartomány szoftvercsatornákon keresztül hello protokoll támogatott. |
| **Tűzfal** | toohelp az adatok védelméhez, egy tűzfalszabály megakadályozza, hogy az összes tooyour adatbáziskiszolgáló vagy tooits adatbázisok, amíg azok a számítógépek megadott engedélye. Lásd: [PostgreSQL-kiszolgáló tűzfalszabályainak az Azure-adatbázis](concepts-firewall-rules.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Hogyan kezelheti a kiszolgáló?
Azure-adatbázis kezelheti a PostgreSQL-kiszolgálók használatával hello Azure-portálon vagy hello [Azure CLI](/cli/azure/postgres).

## <a name="next-steps"></a>Következő lépések
- Hello szolgáltatás áttekintését lásd: [Azure adatbázis PostgreSQL – áttekintés](overview.md)
- Adott erőforrásokra vonatkozó információkból tájékoódhat kvótái és korlátai alapján a **szolgáltatásréteg**, lásd: [Szolgáltatásszinteken](concepts-service-tiers.md)
- Csatlakozás toohello szolgáltatás információkért lásd: [PostgreSQL az Azure-adatbázis adatkapcsolattárak](concepts-connection-libraries.md).
