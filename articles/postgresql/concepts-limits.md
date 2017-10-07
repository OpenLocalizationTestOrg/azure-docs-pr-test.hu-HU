---
title: "Azure adatbázisban a következő PostgreSQL aaaLimitations |} Microsoft Docs"
description: "Azure-adatbázis korlátozásai PostgreSQL ismerteti."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 06/01/2017
ms.openlocfilehash: f53dd240e55e0633bc1dfb8ad25e1818fa8ae18c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Az Azure-adatbázis PostgreSQL korlátozásai
hello Azure adatbázis PostgreSQL szolgáltatás nyilvános előzetes verziójában van. hello következő részek a kapacitás és működési korlátai hello adatbázis szolgáltatásban.

## <a name="service-tier-maximums"></a>Szolgáltatási szint méretkorlát
Azure PostgreSQL-adatbázishoz a kiszolgáló létrehozása választhat több szolgáltatásszinttel rendelkezik. További információkért lásd: [az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md).  

Hiba egy maximális száma érték a kapcsolatok, a számítási egység és a tárolás, az egyes szolgáltatásszinteken hello szolgáltatás előzetes, az alábbiak szerint: 

|                            |                   |
| :------------------------- | :---------------- |
| **Kapcsolatok maximális száma**        |                   |
| Alapszintű 50 számítási egység     | 50 kapcsolatok    |
| Alapszintű 100 számítási egység    | 100 kapcsolatok száma   |
| Standard 100 számítási egység | 200 kapcsolatok   |
| Standard 200 számítási egység | 300 kapcsolatok   |
| Standard 400 számítási egység | 400 kapcsolatok   |
| Standard 800 számítási egység | 500 kapcsolatok száma   |
| **Maximális számítási egység**      |                   |
| Alapszintű szolgáltatásszint         | 100 számítási egység |
| Standard szolgáltatásszint      | 800 számítási egység |
| **Maximális tárolási**            |                   |
| Alapszintű szolgáltatásszint         | 1 TB              |
| Standard szolgáltatásszint      | 1 TB              |

Túl sok a kapcsolat elérésekor hello a következő hiba jelenhet meg:
> Súlyos hiba: sajnos már túl sok ügyfél

## <a name="preview-functional-limitations"></a>Előzetes verzió működési korlátozásai
### <a name="scale-operations"></a>A skálázási műveletek
1.  Dinamikus méretezés kiszolgálók között szolgáltatásszintek jelenleg nem támogatott. Ez azt jelenti, hogy Basic és Standard szolgáltatásszintek közötti váltás.
2.  Dinamikus igény szerinti növelését tárolási előre létrehozott kiszolgálón jelenleg nem támogatott.
3.  Kiszolgáló tároló méretének csökkentése nem támogatott.

### <a name="server-version-upgrades"></a>Kiszolgáló verziófrissítések
- Fő adatbázis motor verziók közötti automatikus áttelepítési jelenleg nem támogatott.

### <a name="subscription-management"></a>Előfizetés-kezelés
- Dinamikusan áthelyezése előfizetés és az erőforráscsoport előre létrehozott kiszolgálók jelenleg nem támogatott.

### <a name="point-in-time-restore"></a>Időponthoz kötött visszaállítás
1.  Toodifferent szolgáltatásréteg és/vagy számítási egység és a tárhely mérete visszaállítása nem engedélyezett.
2.  Az eldobott kiszolgáló visszaállítása nem támogatott.

## <a name="next-steps"></a>Következő lépések
- Megértéséhez [egyes tarifacsomagok elérhető](concepts-service-tiers.md)
- Megértéséhez [támogatott PostgreSQL-adatbázis verziója](concepts-supported-versions.md)
- Felülvizsgálati [hogyan tooBack mentése és visszaállítása egy Azure-adatbázis kiszolgáló PostgreSQL használatára vonatkozó hello Azure-portálon](howto-restore-server-portal.md)
