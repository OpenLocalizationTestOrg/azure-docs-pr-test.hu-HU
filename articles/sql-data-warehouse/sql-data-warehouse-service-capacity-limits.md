---
title: "Az SQL Data Warehouse kapacitáskorlátait |} Microsoft Docs"
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
ms.date: 12/14/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 3a8edb3806f981ebb6f8c1ca6c994ae198df2ec2
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/16/2017
---
# <a name="sql-data-warehouse-capacity-limits"></a>Az SQL Data Warehouse kapacitáskorlátait
Az alábbi táblázatban a különböző összetevőket az Azure SQL Data Warehouse engedélyezett maximális értékeket tartalmazza.

## <a name="workload-management"></a>Terheléskezelés
| Kategória | Leírás | Maximum |
|:--- |:--- |:--- |
| [Adattárházegységek (DWU)][Data Warehouse Units (DWU)] |Maximális DWU egyetlen SQL-adatraktár | A rugalmasság optimalizálva [teljesítményszinttel](performance-tiers.md): DW6000<br></br>A számítási optimalizált [teljesítményszinttel](performance-tiers.md): DW30000c |
| [Adattárházegységek (DWU)][Data Warehouse Units (DWU)] |Alapértelmezett DTU kiszolgálónként |54,000<br></br>Alapértelmezés szerint minden SQL server (például myserver.database.windows.net) rendelkezik egy DTU-Kvótáról 54 000, amely lehetővé teszi, hogy legfeljebb DW6000c. Ez a kvóta egyszerűen egy biztonsági korlát. A kvótája által [a támogatási jegy létrehozása] [ creating a support ticket] választja *kvóta* kérelem típusként.  A DTU kiszámításához van, a 7.5 szorozza meg a teljes DWU szükséges, vagy a 9.0 szorozza meg a teljes cDWU szükséges. Példa:<br></br>7.5 = 45,000 x DW6000 dtu-k<br></br>X 9.0 = 54 000 DW600c dtu-inak száma.<br></br>Az SQL server beállítás az aktuális DTU-használat a portálon tekintheti meg. Felfüggesztett, mind a megkezdődhet adatbázisok száma felé a DTU-kvótát. |
| Adatbázis-kapcsolat |Egyidejű megnyitott munkamenetek |1024<br/><br/>A 1024 aktív munkamenetek mindegyikének kérelmezheti SQL Data Warehouse-adatbázishoz egy időben. Vegye figyelembe, hogy egyidejűleg futtatható szolgáltatásobjektumok lekérdezések száma korlátozott. A feldolgozási korlát túllépésekor a kéréseket, amelyekre egy belső várólistán ahol vár feldolgozásra. |
| Adatbázis-kapcsolat |Maximális memória előkészített utasítások |20 MB |
| [Munkaterhelés-kezelés][Workload management] |Maximális párhuzamos lekérdezések |32<br/><br/> Alapértelmezés szerint az SQL Data Warehouse legfeljebb 32 egyidejű lekérdezéseket és lekérdezések fennmaradó várólisták hajthat végre.<br/><br/>Az egyidejű lekérdezések descrease is, amikor felhasználók vannak hozzárendelve a magasabb erőforrás osztályok, vagy ha az SQL Data warehouse-bA rendelkezik alsó [szolgáltatási szint](performance-tiers.md#service-levels). Néhány lekérdezést, például a DMV lekérdezések futtatása mindig engedélyezett. |
| [a TempDB][Tempdb] |Maximális GB |399 GB-ot DW100. Ezért a DWU1000, a tempdb mérete és 3.99 TB |

## <a name="database-objects"></a>Adatbázis-objektumok
| Kategória | Leírás | Maximum |
|:--- |:--- |:--- |
| Adatbázis |Maximális méret |A lemezen tömörített 240 TB<br/><br/>Ez a terület független a tempdb vagy a napló, és ezért van kijelölve az ezen a helyen állandó táblákat.  Fürtözött oszlopcentrikus tömörítési becsült 5 X.  Ez a fajta tömörítés lehetővé teszi, hogy az adatbázis méretét a körülbelül 1 PB, ha minden tábla fürtözött oszlopcentrikus (az alapértelmezett táblatípus). |
| Tábla |Maximális méret |A lemezen tömörített 60 TB |
| Tábla |Adatbázisonként táblák |2 milliárd |
| Tábla |Táblánként oszlopok |1024 oszlopot |
| Tábla |Bájt / oszlop |Oszlop függ [adattípus][data type].  A határ 8000 char adattípus, nvarchar a 4000 vagy MAX adattípusok 2 GB. |
| Tábla |Soronként meghatározott bájt |8060 bájt<br/><br/>A soronkénti bájtok száma kiszámítása ugyanolyan módon, mert az SQL Server lap tömörítést. Például az SQL Server, az SQL Data Warehouse támogatja a sor-túlcsordulás tárolóról, ami lehetővé teszi, hogy **változó hosszúságú oszloppal** a soron kívüli leküldött. Változó hosszúságú sorok soron kívüli elküldte azokat, csak 24 bájtos gyökér tárolja a fő rekordban. További információkért lásd: a [sor-túlcsordulás adatok meghaladja a 8 KB-os][Row-Overflow Data Exceeding 8 KB]. |
| Tábla |Táblánként partíciók |15,000<br/><br/>A nagy teljesítményű, azt javasoljuk, számának minimalizálása a partíciók száma meg kell során továbbra is támogató üzleti igényeinek. A partíciók számának növekedésével a terhelés adatok Definition nyelvi (DDL) és az adatok adatkezelési nyelvi (DML) műveletekhez növekedésének és az alacsonyabb teljesítményt. |
| Tábla |Az karakter / partíció határérték. |4000 |
| Index |A nem fürtözött indexek táblánként. |999<br/><br/>Csak a sortárindex táblák vonatkozik. |
| Index |Fürtözött indexek táblánként. |1<br><br/>Sortárindex és a oszlopcentrikus vonatkozik. |
| Index |Indexkulcs mérete. |900 bájtot.<br/><br/>Csak a sortárindex indexek vonatkozik.<br/><br/>Több mint 900 bájtos maximális mérettel varchar típusú oszlopokon indexek is létrehozható, ha a meglévő adatok az oszlopok nem haladja meg a 900 bájtot az index létrehozásakor. Azonban később INSERT vagy az oszlopokat, amelyek a teljes mérete meghaladja a 900 bájtot frissítés művelet sikertelen lesz. |
| Index |Kulcs oszlopot. |16<br/><br/>Csak a sortárindex indexek vonatkozik. Fürtözött oszlopcentrikus indexek tartalmazza az összes oszlopot. |
| Statisztika |A kombinált oszlopok értékeinek mérete. |900 bájtot. |
| Statisztika |Statisztika objektumonként oszlopok. |32 |
| Statisztika |A statisztika táblánként oszlopokon létrehozni. |30,000 |
| Tárolt eljárások |Maximális beágyazási szintet. |8 |
| Nézet |Soronként megtekintése |1,024 |

## <a name="loads"></a>Terhelések
| Kategória | Leírás | Maximum |
|:--- |:--- |:--- |
| A Polybase terhelések |Soronként MB |1<br/><br/>A Polybase csak a sorok, amelyekre 1 MB-nál kisebb méretű, és nem tölthető be, VARCHAR(MAX), NVARCHAR(MAX) vagy VARBINARY(MAX) tölti be.<br/><br/> |

## <a name="queries"></a>Lekérdezések
| Kategória | Leírás | Maximum |
|:--- |:--- |:--- |
| Lekérdezés |A felhasználói táblák aszinkron lekérdezésekben. |1000 |
| Lekérdezés |Egyidejű lekérdezéseket a rendszer nézetek. |100 |
| Lekérdezés |A rendszer nézetek aszinkron lekérdezésekben |1000 |
| Lekérdezés |Maximális paraméterek |2098 |
| Batch |Maximális méret |65,536*4096 |
| Jelölje be eredmények |Oszlopok soronkénti |4096<br/><br/>Soronként legfeljebb 4096 oszlopok a SELECT eredmény soha nem rendelkezhet. Nem garantálja, hogy mindig legyen a 4096 van. Ha a lekérdezésterv egy ideiglenes táblát igényel, az 1024-oszlopok maximális táblánként is vonatkozhatnak. |
| VÁLASSZA KI |Beágyazott segédlekérdezések |32<br/><br/>Soha nem lehet több mint 32 beágyazott segédlekérdezések SELECT utasítással. Nincs nem garantálja, hogy mindig legyen a 32. Például a CSATLAKOZZON egy segédlekérdezésbe is bevezetéséhez a lekérdezéstervben. A segédlekérdezések számát is is korlátozza a memória. |
| VÁLASSZA KI |CSATLAKOZTATÁS / oszlopok |1024 oszlopot<br/><br/>Az ILLESZTÉS soha nem lehet több mint 1024 oszlopot. Nincs nem garantálja, hogy mindig legyen a 1024. Ha az ILLESZTÉS terv több oszlop, mint az ILLESZTÉS eredményeit tartalmazó ideiglenes táblát igényel, az 1024 korlát vonatkozik, az ideiglenes táblába. |
| VÁLASSZA KI |A GROUP BY oszlopok bájtokat. |8060<br/><br/>A GROUP BY záradékban oszlopok 8060 bájtok maximális rendelkezhet. |
| VÁLASSZA KI |Bájtok ORDER BY oszlopok száma |8060 bájt.<br/><br/>Az oszlopok az ORDER BY záradékban legfeljebb 8060 bájt lehet. |
| Azonosítók és állandók utasítás száma |Hivatkozott azonosítók és állandók száma. |65,535<br/><br/>Az SQL Data Warehouse azonosítók és a lekérdezés egyetlen kifejezés tartalmazó állandók számának korlátozása. Ezt a határt 65 535. Haladja meg az SQL Server-hiba 8632 szám eredményez. További információkért lásd: [belső hiba: elérte az egyik korlátozását][Internal error: An expression services limit has been reached]. |

## <a name="metadata"></a>Metaadatok
| Rendszernézet | Sorok maximális száma |
|:--- |:--- |
| sys.dm_pdw_component_health_alerts |10,000 |
| sys.dm_pdw_dms_cores |100 |
| sys.dm_pdw_dms_workers |Teljes száma a legutóbbi 1000 DMS munkavállalók SQL-lekérdezések. |
| sys.dm_pdw_errors |10,000 |
| sys.dm_pdw_exec_requests |10,000 |
| sys.dm_pdw_exec_sessions |10,000 |
| sys.dm_pdw_request_steps |A legutóbbi 1000 SQL kérelmek sys.dm_pdw_exec_requests tárolt lépései teljes száma. |
| sys.dm_pdw_os_event_logs |10,000 |
| sys.dm_pdw_sql_requests |A legutóbbi 1000 SQL kérelmek sys.dm_pdw_exec_requests vannak tárolva. |

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
