---
title: "Azure SQL-adatbázisban található Lekérdezéstár aaaOperating"
description: "Ismerje meg, hogyan toooperate hello Azure SQL-adatbázisban található Lekérdezéstár"
keywords: 
services: sql-database
documentationcenter: 
author: bonova
manager: jhubbard
editor: 
ms.assetid: 0cccf6bd-1327-44f7-a6f9-8eff0c210463
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: sqldb-performance
ms.workload: data-management
ms.date: 11/08/2016
ms.author: bonova
ms.openlocfilehash: ab741e49685bf6df3bceab219aaf57ea51cdb25d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="operating-hello-query-store-in-azure-sql-database"></a>Működési hello Azure SQL-adatbázisban található Lekérdezéstár
A Lekérdezéstár az Azure-ban érhető el egy teljes körűen felügyelt adatbázis-szolgáltatás, amely folyamatosan gyűjti, és minden lekérdezést előzménymodell részletes adatait jeleníti meg. Információ a Lekérdezéstárról, hasonló tooan repülőgép felé továbbított adatok író, amely jelentősen leegyszerűsíti a lekérdezési teljesítmény hibaelhárítási mind a felhőalapú és helyszíni felhasználók gondolja. Ez a cikk ismerteti a Lekérdezéstár az Azure-ban működő adott aspektusait. Ez előtti gyűjtött adatait használja, gyorsan diagnosztizálhatja és teljesítménybeli problémák megoldásához és így az üzleti összpontosító több időt tölt. 

A Lekérdezéstár le lett [világszerte elérhető](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) az Azure SQL Database, 2015. November óta. A Lekérdezéstár szolgáltatás hello alapja Teljesítményelemzés és a szolgáltatások, például a hangolása [SQL Database Advisor és teljesítmény irányítópult](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Ez a cikk közzétételi hello pillanatban Lekérdezéstár fut a több mint 200 000 felhasználói adatbázisokat az Azure, több hónapig megszakítás nélkül lekérdezéssel kapcsolatos adatok gyűjtése.

> [!IMPORTANT]
> A Microsoft aktiválási Lekérdezéstár az összes Azure SQL-adatbázisok (meglévő és új) hello folyamatban van. 
> 
> 

## <a name="optimal-query-store-configuration"></a>A Lekérdezéstár optimális konfiguráció
Ez a szakasz ismerteti, amelyek a tervezett tooensure megbízható művelet hello Lekérdezéstár és a függő szolgáltatások, például a optimális konfigurációs alapértelmezéseinek [SQL Database Advisor és teljesítmény irányítópult](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Alapértelmezett konfiguráció folyamatos az adatgyűjtés, a minimális OFF/READ_ONLY állapotban töltött ideje van optimalizálva.

| Konfiguráció | Leírás | Alapértelmezett | Megjegyzés |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Megadja a Lekérdezéstár is igénybe vehet, z ügyféladatbázis belül hello adatterülethez hello korlátja |100 |Az új adatbázisokat kényszerítése |
| AZ INTERVAL_LENGTH_MINUTES |Határozza meg az időtartományt, amely során összegyűjtött futásidejű statisztikája lekérdezésterveket összesített értéket és megőrzött méretét. Minden aktív lekérdezésterv legfeljebb egy sor rendelkezik ezzel a konfigurációval meghatározott ideig |60 |Az új adatbázisokat kényszerítése |
| AZ STALE_QUERY_THRESHOLD_DAYS |Időalapú törlési házirend, amely a megőrzött futásidejű statisztikák és inaktív lekérdezések hello megőrzési időtartam |30 |Az új adatbázisokat és adatbázisokat az előző alapértelmezetten (367) érvényes |
| A SIZE_BASED_CLEANUP_MODE BEÁLLÍTÁST |Megadja, hogy automatikus adatok karbantartása kerül sor a Lekérdezéstár adatok mérete megközelíti hello korlátot |AUTOMATIKUS |Az összes adatbázisra érvényes |
| A QUERY_CAPTURE_MODE BEÁLLÍTÁST |Megadja, hogy lekérdezések csak egy részhalmazát vagy az összes lekérdezés követi |AUTOMATIKUS |Az összes adatbázisra érvényes |
| FLUSH_INTERVAL_SECONDS |Meghatározza a maximális időtartamot, ameddig rögzített futásidejű statisztikája tárolják a memóriában, toodisk kiürítése előtt |900 |Az új adatbázisokat kényszerítése |
|  | | | |

> [!IMPORTANT]
> Minden Azure SQL-adatbázisban (lásd az előző fontos megjegyzés) Lekérdezéstár aktiválási utolsó szakasza hello automatikusan alkalmazza ezeket az alapértelmezett értékeket. A ritka be, miután az Azure SQL Database nem módosítják, az ügyfelek által beállított konfigurációs értékeket kivéve, ha negatív hatással elsődleges munkaterhelésére vagy hello Lekérdezéstár megbízható működésére.
> 
> 

Az egyéni beállításokkal toostay programmal, [ALTER DATABASE a Lekérdezéstár beállításokkal](https://msdn.microsoft.com/library/bb522682.aspx) toorevert konfigurációs toohello korábbi állapotába. Tekintse meg [gyakorlati tanácsok a Lekérdezéstár hello](https://msdn.microsoft.com/library/mt604821.aspx) ahhoz, hogyan felső toolearn döntött, hogy optimális konfigurációs paramétereket.

## <a name="next-steps"></a>Következő lépések
[SQL adatbázis elemzéséhez](sql-database-performance.md)

## <a name="additional-resources"></a>További források
A további információkat kivételét hello a következő cikkeket:

* [A felhőszolgáltató közötti átviteléhez rögzítő az adatbázis számára](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 
* [Hello Lekérdezéstár használatával teljesítmény figyelése](https://msdn.microsoft.com/library/dn817826.aspx)
* [A Lekérdezéstár használati forgatókönyvek](https://msdn.microsoft.com/library/mt614796.aspx)
* [Hello Lekérdezéstár használatával teljesítmény figyelése](https://msdn.microsoft.com/library/dn817826.aspx) 

