---
title: Azure SQL Data Warehouse aaaTroubleshooting |} Microsoft Docs
description: "Hibaelhárítás az Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/30/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 3c53a39f77057fe0bcc053a0b896555abf397515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse hibaelhárítása
Ez a témakör listák néhány hello ügyfeleink hozzáadunk gyakori hibaelhárítási kérdések.

## <a name="connecting"></a>Csatlakozás
| Probléma | Megoldás: |
|:--- |:--- |
| "NT AUTHORITY\NÉVTELEN bejelentkezés" felhasználó bejelentkezése sikertelen volt. (A Microsoft SQL Server, hiba: 18456) |Ez akkor fordul elő, amikor egy AAD-felhasználó megpróbál tooconnect toohello master adatbázis, de nem rendelkezik a felhasználó a főadatbázisban.  a probléma, vagy adja meg az SQL Data Warehouse kívánja tooconnect tooat kapcsolati idő vagy hello felhasználói toohello fő adatbázis hozzáadása hello toocorrect.  Lásd: [biztonsági áttekintése] [ Security overview] cikkben olvashat. |
| egyszerű "sajátfelhasználónév" nincs hello kiszolgáló képes tooaccess hello "fő" adatbázis hello a jelenlegi biztonsági környezetben. Nem lehet megnyitni a felhasználói alapértelmezett adatbázist. A bejelentkezés sikertelen volt. "Sajátfelhasználónév" felhasználó bejelentkezése sikertelen volt. (A Microsoft SQL Server, hiba: 916) |Ez akkor fordul elő, amikor egy AAD-felhasználó megpróbál tooconnect toohello master adatbázis, de nem rendelkezik a felhasználó a főadatbázisban.  a probléma, vagy adja meg az SQL Data Warehouse kívánja tooconnect tooat kapcsolati idő vagy hello felhasználói toohello fő adatbázis hozzáadása hello toocorrect.  Lásd: [biztonsági áttekintése] [ Security overview] cikkben olvashat. |
| CTAIP hiba |Ez a hiba akkor fordulhat elő, a bejelentkezési hello SQL server főadatbázis, de nem a hello SQL Data Warehouse-adatbázis létrehozása.  Ha ez a hiba, tekintse meg hello [biztonsági áttekintése] [ Security overview] cikk.  Ez a cikk azt ismerteti, hogyan toocreate hozzon létre egy felhasználónevet és egy felhasználó fő, majd hogyan toocreate a felhasználókat a hello SQL Data Warehouse-adatbázis. |
| Tiltsa le tűzfal |Az Azure SQL-adatbázisok csak ismert IP-címek hozzáférési tooa adatbázist használ, kiszolgáló és az adatbázis-szintű tűzfalak tooensure védi. hello tűzfalak biztonságosabbak alapértelmezett, ami azt jelenti, hogy explicit módon engedélyeznie kell, és az IP-cím vagy a címtartományt, mielőtt az csatlakozna.  tooconfigure a tűzfalon a hozzáférést, kövesse hello [kiszolgáló tűzfal elérésének konfigurálása az ügyfél IP-címhez] [ Configure server firewall access for your client IP] a hello [utasításokat kiépítés] [Provisioning instructions]. |
| Az eszköz vagy az illesztőprogram nem lehet kapcsolódni |Az SQL Data Warehouse használatát javasolja [SSMS][SSMS], [az SSDT a Visual Studio][SSDT for Visual Studio], vagy [sqlcmd] [ sqlcmd] tooquery adatait. Illesztőprogramok és a kapcsolódó tooSQL adatraktár további részletekért lásd: [az Azure SQL Data Warehouse illesztőprogramok] [ Drivers for Azure SQL Data Warehouse] és [csatlakozzon az SQL Data Warehouse tooAzure] [ Connect tooAzure SQL Data Warehouse] cikkeket. |

## <a name="tools"></a>Eszközök
| Probléma | Megoldás: |
|:--- |:--- |
| A Visual Studio object Explorerben hiányzik az AAD-felhasználókat |Ez az egy ismert probléma.  A probléma megoldásához hello felhasználók megtekintése [sys.database_principals][sys.database_principals].  Lásd: [hitelesítési tooAzure SQL Data Warehouse] [ Authentication tooAzure SQL Data Warehouse] toolearn további Azure Active Directory használatáról az SQL Data Warehouse szolgáltatással. |
|Manuális parancsfájlok, hello scripting varázsló segítségével, vagy a SSMS kapcsolatra lassú, lefagyott vagy előállító hibák| Győződjön meg arról, hogy létrejöttek-e a felhasználók hello master adatbázisban. A parancsprogram-beállítások ellenőrizze azt is, hogy a "Microsoft Azure SQL Data Warehouse Edition" be van állítva az hello motor edition és a motor típus: "Microsoft Azure SQL adatbázis".|

## <a name="performance"></a>Teljesítmény
| Probléma | Megoldás: |
|:--- |:--- |
| Lekérdezés teljesítmény hibaelhárítása |Ha egy adott lekérdezés tootroubleshoot próbálja, kezdje [tanulási hogyan toomonitor a lekérdezések][Learning how toomonitor your queries]. |
| Gyenge a lekérdezések teljesítményét és tervek gyakran hiányzó statisztika eredménye |hello leggyakoribb gyenge teljesítményt oka a statisztikákat a táblák hiánya.  Lásd: [fenntartása a tábla statisztikai adatainak] [ Statistics] kapcsolatos részletes tudnivalókért toocreate statisztika, és ezért azok kritikus tooyour teljesítményét. |
| Alacsony feldolgozási / lekérdezések várólistára |Understanding [munkaterhelés felügyeleti] [ Workload management] fontos a sorrend toounderstand hogyan toobalance memóriafoglalás együtt. |
| Hogyan tooimplement gyakorlati tanácsok |hello legjobb hely toostart toolearn módon tooimprove lekérdezési teljesítmény van [gyakorlati tanácsok az SQL Data Warehouse] [ SQL Data Warehouse best practices] cikk. |
| Hogyan tooimprove teljesítmény skálázás |Néha hello megoldás tooimproving teljesítménye toosimply adja hozzá a további számítási power tooyour lekérdezések [az SQL Data Warehouse skálázás][Scaling your SQL Data Warehouse]. |
| Gyenge lekérdezési teljesítményt, gyenge index minőségi miatt |Néhány eset lekérdezéseket is lassulást miatt [gyenge oszlopcentrikus index minőségi][Poor columnstore index quality].  Ebben a cikkben találhat további információt, és hogyan túl[Rebuild indexeli tooimprove szegmens minőségi][Rebuild indexes tooimprove segment quality]. |

## <a name="system-management"></a>Rendszer-felügyeleti
| Probléma | Megoldás: |
|:--- |:--- |
| 40847. üzenet: Nem tudta végrehajtani a hello műveletet, mert a kiszolgáló túllépné az engedélyezett adatbázis tranzakciós egység beállított kvótát 45000 hello. |Vagy csökkentse a hello [DWU] [ DWU] hello adatbázis toocreate próbált vagy [a kvóta növelését][request a quota increase]. |
| Lemezterület-kihasználás kivizsgálása |Lásd: [méretek tábla] [ Table sizes] toounderstand hello lemezterület-kihasználás a rendszer. |
| Súgó a táblák kezelése |Lásd: hello [tábla áttekintése] [ Overview] cikk való kezeléséhez a táblák segítségét.  Ez a cikk hivatkozásokat is tartalmaz részletes témakörökre például [adattípusok tábla][Data types], [terjesztése egy tábla][Distribute], [tábla indexelő][Index], [tábla particionáló][Partition], [fenntartása a tábla statisztikai adatainak] [ Statistics] és [az ideiglenes táblák][Temporary]. |
|Az Azure portál hello nem frissíti a átlátható titkosítási (TDE) folyamatjelző|Megtekintheti a TDE keresztül hello állapotának [powershell](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption).|

## <a name="polybase"></a>PolyBase
| Probléma | Megoldás: |
|:--- |:--- |
| Betöltési nagy sorok miatt meghiúsul |Nagy sor támogatása jelenleg nem érhető el a Polybase.  Ez azt jelenti, hogy ha a tábla tartalmaz, VARCHAR(MAX), NVARCHAR(MAX) vagy VARBINARY(MAX), külső táblákon nem lehet használt tooload adatait.  Nagy sorok terhelések jelenleg csak Azure Data Factory (a BCP-vel), az Azure Stream Analytics, a SSIS, a BCP vagy a .NET SQLBulkCopy osztály hello támogatott. Nagy sorok PolyBase támogatása egy későbbi kiadásban lesz hozzáadva. |
| MAXIMÁLIS adattípus-táblázat BCP betöltése sikertelen |Nincs olyan ismert probléma, amely megköveteli, hogy az VARCHAR(MAX), NVARCHAR(MAX) vagy VARBINARY(MAX) kerüljenek-e bizonyos esetekben hello tábla hello végén.  Helyezze a hello tábla maximális oszlopok toohello végét. |

## <a name="differences-from-sql-database"></a>Eltérések a SQL-adatbázis
| Probléma | Megoldás: |
|:--- |:--- |
| Nem támogatott SQL-adatbázis szolgáltatások |Lásd: [táblában funkciók nem támogatott][Unsupported table features]. |
| Nem támogatott SQL-adatbázis adattípusok |Lásd: [nem támogatott típusú adatokat][Unsupported data types]. |
| Törlés és frissítési korlátozások |Lásd: [frissítési megkerülő megoldások][UPDATE workarounds], [törlése lehetséges megoldások] [ DELETE workarounds] és [használatával CTAS toowork körül nem támogatott frissítési és TÖRLÉS szintaxis][Using CTAS toowork around unsupported UPDATE and DELETE syntax]. |
| MERGE utasítás nem támogatott. |Lásd: [egyesítési lehetséges megoldások][MERGE workarounds]. |
| Tárolt eljárás korlátozásai |Lásd: [tárolt eljárás korlátozások] [ Stored procedure limitations] toounderstand tárolt eljárások hello korlátozások némelyike. |
| Felhasználó által megadott függvények nem támogatják a SELECT utasítás |Ez az a felhasználó által megadott függvények aktuális korlátozása.  Lásd: [CREATE FUNCTION] [ CREATE FUNCTION] hello a szintaxis támogatott. |

## <a name="next-steps"></a>Következő lépések
Ha Ön volt nem toofind egy megoldás tooyour probléma újabb, az alábbiakban néhány más erőforrások, próbálja meg.

* [Blogok]
* [Funkciókérések]
* [Videók]
* [CAT csapatblogok]
* [Támogatási jegy létrehozása]
* [MSDN-fórum]
* [Stack Overflow-fórum]
* [Twitter]

<!--Image references-->

<!--Article references-->
[Security overview]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT for Visual Studio]: ./sql-data-warehouse-install-visual-studio.md
[Drivers for Azure SQL Data Warehouse]: ./sql-data-warehouse-connection-strings.md
[Connect tooAzure SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Támogatási jegy létrehozása]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Scaling your SQL Data Warehouse]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md
[request a quota increase]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Learning how toomonitor your queries]: ./sql-data-warehouse-manage-monitor.md
[Provisioning instructions]: ./sql-data-warehouse-get-started-provision.md
[Configure server firewall access for your client IP]: ./sql-data-warehouse-get-started-provision.md#create-a-server-level-firewall-rule-in-the-azure-portal
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[Table sizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes tooimprove segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Using CTAS toowork around unsupported UPDATE and DELETE syntax]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[UPDATE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[DELETE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE workarounds]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Stored procedure limitations]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentication tooAzure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md
[Working around hello PolyBase UTF-8 requirement]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[CREATE FUNCTION]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogok]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT csapatblogok]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funkciókérések]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-fórum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stack Overflow-fórum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videók]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
