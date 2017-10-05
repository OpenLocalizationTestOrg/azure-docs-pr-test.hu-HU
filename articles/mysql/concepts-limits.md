---
title: "MySQL az Azure-adatbázis korlátozásai |} Microsoft Docs"
description: "A MySQL az Azure-adatbázis előzetes verzió korlátozásai ismerteti."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c61d70897b66c2ffee819ac98c38ab75000db907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a>Az Azure-adatbázis korlátozásai MySQL (előzetes verzió)
Az Azure-adatbázishoz a MySQL-szolgáltatás nyilvános előzetes verziójában van. A következő szakaszok ismertetik a kapacitás és az adatbázis szolgáltatásban működik korlátok.

## <a name="service-tier-maximums"></a>Szolgáltatási szint méretkorlát
Azure MySQL-adatbázis a kiszolgáló létrehozása választhat több szolgáltatásszinttel rendelkezik. További információkért lásd: [az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md).  

Nincs kapcsolatok, a számítási egység és a tárolás, az egyes szolgáltatásszintek tartalmának maximális száma a szolgáltatás előzetes az alábbiak szerint: 

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

Túl sok a kapcsolat elérésekor a következő hibaüzenet jelenhet meg:
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
1.  Különböző szolgáltatási rétegben és/vagy számítási egység és a tárhely mérete visszaállítása nem engedélyezett.
2.  Az eldobott kiszolgáló visszaállítása nem támogatott.

## <a name="next-steps"></a>További lépések:
[Az egyes szolgáltatásszinteken elérhető](concepts-service-tiers.md)
[MySQL-adatbázis verziója támogatott](concepts-supported-versions.md)
