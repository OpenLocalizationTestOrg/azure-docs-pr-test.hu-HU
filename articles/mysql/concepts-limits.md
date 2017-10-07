---
title: "a MySQL az Azure-adatbázis aaaLimitations |} Microsoft Docs"
description: "A MySQL az Azure-adatbázis előzetes verzió korlátozásai ismerteti."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 9c877c592bf640f62182d8761c9c51363882d706
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a>Az Azure-adatbázis korlátozásai MySQL (előzetes verzió)
hello Azure adatbázis MySQL szolgáltatás nyilvános előzetes verziójában van. hello következő részek a kapacitás és működési korlátai hello adatbázis szolgáltatásban.

## <a name="service-tier-maximums"></a>Szolgáltatási szint méretkorlát
Azure MySQL-adatbázis a kiszolgáló létrehozása választhat több szolgáltatásszinttel rendelkezik. További információkért lásd: [az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md).  

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
> 1040 (08004). hiba: Túl sok a kapcsolat

## <a name="preview-functional-limitations"></a>Előzetes verzió működési korlátozásai:
### <a name="scale-operations"></a>A skálázási műveletek:
1.  Dinamikus méretezés kiszolgálók között szolgáltatásszintek jelenleg nem támogatott. Ez azt jelenti, hogy Basic és Standard szolgáltatásszintek közötti váltás.
2.  Dinamikus igény szerinti növelését tárolási előre létrehozott kiszolgálón jelenleg nem támogatott.
3.  Kiszolgáló tároló méretének csökkentése nem támogatott.

### <a name="server-version-upgrades"></a>Kiszolgáló verziófrissítések:
- Fő adatbázis motor verziók közötti automatikus áttelepítési jelenleg nem támogatott.

### <a name="subscription-management"></a>Előfizetés-kezelés:
- Dinamikusan áthelyezése előfizetés és az erőforráscsoport előre létrehozott kiszolgálók jelenleg nem támogatott.

### <a name="point-in-time-restore"></a>Pont-a--visszaállítás egy korábbi időpontra:
1.  Toodifferent szolgáltatásréteg és/vagy számítási egység és a tárhely mérete visszaállítása nem engedélyezett.
2.  Az eldobott kiszolgáló visszaállítása nem támogatott.

## <a name="next-steps"></a>További lépések:
[Az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md)
[MySQL-adatbázis verziója támogatott](concepts-supported-versions.md)
