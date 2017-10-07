---
title: "adatraktár kapacitáskorlátait aaaSQL |} Microsoft Docs"
description: "A kapcsolatok, adatbázisok, táblák és az SQL Data Warehouse lekérdezések maximális értékeket."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: e1eac122-baee-4200-a2ed-f38bfa0f67ce
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 8619cb997f0955d649d447cb8ca15cd742cc70b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-capacity-limits"></a>Az SQL Data Warehouse kapacitáskorlátait
hello táblázatokban tartalmazhat hello Azure SQL Data Warehouse különböző összetevői engedélyezett maximális értékeket.

## <a name="workload-management"></a>Terheléskezelés
| Kategória | Leírás | Maximális |
|:--- |:--- |:--- |
| [Adattárházegységek (DWU)][Data Warehouse Units (DWU)] |Maximális DWU egyetlen SQL-adatraktár |6000 |
| [Adattárházegységek (DWU)][Data Warehouse Units (DWU)] |Maximális DWU egy önálló SQL-kiszolgálóhoz |Alapértelmezés szerint 6000<br/><br/> Alapértelmezés szerint minden SQL server (pl. myserver.database.windows.net) rendelkezik egy DTU-Kvótáról 45,000, amely lehetővé teszi a too6000 DWU. Ez a kvóta egyszerűen egy biztonsági korlát. A kvótája által [a támogatási jegy létrehozása] [ creating a support ticket] választja *kvóta* hello kérelem típusa.  a DTU, meg kell szorozni toocalculate 7.5 hello hello teljes DWU szükséges. Az aktuális DTU-használat hello SQL server panelen hello portálon tekintheti meg. Felfüggesztett, mind a nem felfüggesztett adatbázisok száma felé hello DTU-kvótát. |
| Adatbázis-kapcsolat |Egyidejű megnyitott munkamenetek |1024<br/><br/>Legfeljebb 1024 aktív kapcsolatot, amelyek mindegyike elküldheti a kérelmek tooa SQL Data Warehouse adatbázis hello támogatott ugyanannyi időt vesz igénybe. Vegye figyelembe, hogy vannak-e hello lekérdezések száma, amelyek ténylegesen végrehajtható egyidejűleg vonatkozó korlátozások. Hello feldolgozási korlát túllépésekor hello kéréseket, amelyekre egy belső várólistán ahol feldolgozott toobe arra vár. |
| Adatbázis-kapcsolat |Maximális memória előkészített utasítások |20 MB |
| [Munkaterhelés-kezelés][Workload management] |Maximális párhuzamos lekérdezések |32<br/><br/> Alapértelmezés szerint az SQL Data Warehouse legfeljebb 32 egyidejű lekérdezéseket és lekérdezések fennmaradó várólisták hajthat végre.<br/><br/>hello párhuzamossági szint csökkentheti a felhasználók hozzárendelése esetén tooa magasabb erőforrásosztály, vagy ha az SQL Data Warehouse alacsony DWU van konfigurálva. Néhány lekérdezést, például a DMV lekérdezések toorun mindig engedélyezett. |
| [A TempDB][Tempdb] |A Tempdb maximális mérete |399 GB-ot DW100. Ezért DWU1000 Tempdb van méretű too3.99 TB |

## <a name="database-objects"></a>Adatbázis-objektumok
| Kategória | Leírás | Maximális |
|:--- |:--- |:--- |
| Adatbázis |Maximális méret |A lemezen tömörített 240 TB<br/><br/>Ez a terület független a tempdb vagy a napló, és ezért-e a hely dedikált toopermanent táblák.  Fürtözött oszlopcentrikus tömörítési becsült 5 X.  Ez a fajta tömörítés hello adatbázis toogrow tooapproximately 1 PB lehetővé teszi, ha minden tábla fürtözött oszlopcentrikus (hello alapértelmezett táblatípus). |
| Tábla |Maximális méret |A lemezen tömörített 60 TB |
| Tábla |Adatbázisonként táblák |2 milliárd |
| Tábla |Táblánként oszlopok |1024 oszlopot |
| Tábla |Bájt / oszlop |Oszlop függ [adattípus][data type].  A határ 8000 char adattípus, nvarchar a 4000 vagy MAX adattípusok 2 GB. |
| Tábla |Soronként meghatározott bájt |8060 bájt<br/><br/>hello bájtban soronkénti kiszámítása a hello ugyanúgy, ahogy az az SQL Server lap tömörítést. Például az SQL Server, az SQL Data Warehouse támogatja a sor-túlcsordulás tárolóról, ami lehetővé teszi, hogy **változó hosszúságú oszloppal** toobe leküldött soron kívüli. Változó hosszúságú sorok soron kívüli elküldte azokat, csak 24 bájtos gyökér tárolja hello fő rekordban. További információkért lásd: hello [sor-túlcsordulás adatok meghaladja a 8 KB-os] [ Row-Overflow Data Exceeding 8 KB] MSDN-cikk tárgyalja. |
| Tábla |Táblánként partíciók |15,000<br/><br/>A nagy teljesítményű, azt javasoljuk, hello számának minimalizálása a partíciók száma meg kell során továbbra is támogató üzleti igényeinek. Hello, partíciók száma növekszik hello terhelés adatok Definition nyelvi (DDL) és az adatok adatkezelési nyelvi (DML) operations növekedésének és az alacsonyabb teljesítményt. |
| Tábla |Az karakter / partíció határérték. |4000 |
| Index |A nem fürtözött indexek táblánként. |999<br/><br/>Csak a táblázatok toorowstore vonatkozik. |
| Index |Fürtözött indexek táblánként. |1<br><br/>Tooboth sortárindex és oszlopcentrikus táblák vonatkozik. |
| Index |Indexkulcs mérete. |900 bájtot.<br/><br/>Toorowstore indexek csak vonatkozik.<br/><br/>Több mint 900 bájtos maximális mérettel varchar típusú oszlopokon indexek is létrehozható, ha hello hello oszlopokban található meglévő adatokat nem haladja meg a 900 bájtot hello index létrehozásakor. Azonban később BESZÚRÁSA vagy frissítési műveletek hello teljes mérete tooexceed 900 bájtot okozó hello oszlopokon sikertelen lesz. |
| Index |Kulcs oszlopot. |16<br/><br/>Toorowstore indexek csak vonatkozik. Fürtözött oszlopcentrikus indexek tartalmazza az összes oszlopot. |
| Statisztika |Hello mérete oszlopok értékeinek együtt. |900 bájtot. |
| Statisztika |Statisztika objektumonként oszlopok. |32 |
| Statisztika |A statisztika táblánként oszlopokon létrehozni. |30,000 |
| Tárolt eljárások |Maximális beágyazási szintet. |8 |
| Nézet |Soronként megtekintése |1,024 |

## <a name="loads"></a>Terhelések
| Kategória | Leírás | Maximális |
|:--- |:--- |:--- |
| A Polybase terhelések |Soronként MB |1<br/><br/>Polybase terhelések mindkét korlátozott tooloading sorok 1MB-nál kisebb, és nem tölthető be a tooVARCHR(MAX), NVARCHAR(MAX) vagy VARBINARY(MAX).<br/><br/> |

## <a name="queries"></a>Lekérdezések
| Kategória | Leírás | Maximális |
|:--- |:--- |:--- |
| Lekérdezés |A felhasználói táblák aszinkron lekérdezésekben. |1000 |
| Lekérdezés |Egyidejű lekérdezéseket a rendszer nézetek. |100 |
| Lekérdezés |A rendszer nézetek aszinkron lekérdezésekben |1000 |
| Lekérdezés |Maximális paraméterek |2098 |
| Batch |Maximális méret |65,536*4096 |
| Jelölje be eredmények |Oszlopok soronkénti |4096<br/><br/>Válassza ki az eredmény a hello soronkénti legfeljebb 4096 oszlopok soha nem rendelkezhet. Nem garantálja, hogy mindig legyen a 4096 van. Ha hello lekérdezésterv egy ideiglenes táblát igényel, hello 1024 oszlopok maximális táblánként is vonatkozhatnak. |
| VÁLASSZA KI |Beágyazott segédlekérdezések |32<br/><br/>Soha nem lehet több mint 32 beágyazott segédlekérdezések SELECT utasítással. Nincs nem garantálja, hogy mindig legyen a 32. Például a CSATLAKOZZON egy segédlekérdezésbe is bevezetéséhez hello lekérdezéstervben. segédlekérdezések hello száma is is korlátozza a memória. |
| VÁLASSZA KI |CSATLAKOZTATÁS / oszlopok |1024 oszlopot<br/><br/>Hello ILLESZTÉSI soha nem lehet több mint 1024 oszlopot. Nincs nem garantálja, hogy mindig legyen a 1024. Ha hello ILLESZTÉSI terv hello ILLESZTÉS eredményeit-nál több oszlopot tartalmazó ideiglenes táblát igényel, hello 1024 korlát vonatkozik toohello ideiglenes tábla. |
| VÁLASSZA KI |A GROUP BY oszlopok bájtokat. |8060<br/><br/>hello oszlopok hello GROUP BY záradékban szerepelhetnek 8060 bájtok maximális. |
| VÁLASSZA KI |Bájtok ORDER BY oszlopok száma |8060 bájt.<br/><br/>hello oszlopok az ORDER BY záradékban hello legfeljebb 8060 bájt lehet. |
| Azonosítók és állandók utasítás száma |Hivatkozott azonosítók és állandók száma. |65,535<br/><br/>Az SQL Data Warehouse azonosítók és a lekérdezés egyetlen kifejezés tartalmazó állandók hello számának korlátozása. Ezt a határt 65 535. Haladja meg az SQL Server-hiba 8632 szám eredményez. További információkért lásd: [belső hiba: elérte az egyik korlátozását][Internal error: An expression services limit has been reached]. |

## <a name="metadata"></a>Metaadatok
| Rendszernézet | Sorok maximális száma |
|:--- |:--- |
| sys.dm_pdw_component_health_alerts |10,000 |
| sys.dm_pdw_dms_cores |100 |
| sys.dm_pdw_dms_workers |Teljes száma a legutóbbi 1000 hello DMS munkavállalók SQL-lekérdezések. |
| sys.dm_pdw_errors |10,000 |
| sys.dm_pdw_exec_requests |10,000 |
| sys.dm_pdw_exec_sessions |10,000 |
| sys.dm_pdw_request_steps |Lépéseket sys.dm_pdw_exec_requests tárolt legutóbbi 1000 hello SQL kérelmek teljes száma. |
| sys.dm_pdw_os_event_logs |10,000 |
| sys.dm_pdw_sql_requests |hello legutóbbi 1000 SQL kérelmek sys.dm_pdw_exec_requests vannak tárolva. |

## <a name="next-steps"></a>Következő lépések
Hivatkozás kapcsolatos további információkért lásd: [SQL Data Warehouse hivatkozás áttekintés][SQL Data Warehouse reference overview].

<!--Image references-->

<!--Article references-->
[Data Warehouse Units (DWU)]: ./sql-data-warehouse-overview-what-is.md
[SQL Data Warehouse reference overview]: ./sql-data-warehouse-overview-reference.md
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Tempdb]: ./sql-data-warehouse-tables-temporary.md
[data type]: ./sql-data-warehouse-tables-data-types.md
[creating a support ticket]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Row-Overflow Data Exceeding 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Internal error: An expression services limit has been reached]: https://support.microsoft.com/kb/913050
