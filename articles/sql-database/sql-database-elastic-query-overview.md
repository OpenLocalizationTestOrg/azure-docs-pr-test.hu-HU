---
title: "aaaAzure SQL Database rugalmas lekérdezési áttekintése |} Microsoft Docs"
description: "Hello rugalmas lekérdezési szolgáltatás áttekintése"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: a8bf0e2c-bc74-44d0-9b1e-bcc9a6aa2e33
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: mlandzic
ms.openlocfilehash: db17f551882cfcae0da67fdda12708baeb6db81c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-elastic-query-overview-preview"></a>Az Azure SQL Database rugalmas lekérdezési áttekintése (előzetes verzió)
hello rugalmas lekérdezés (az előzetes verzió) funkciójával toorun kiterjedő több adatbázis az Azure SQL Database Transact-SQL lekérdezés. Lehetővé teszi tooperform közötti adatbázis-lekérdezések tooaccess távoli táblákat és tooconnect Microsoft és harmadik féltől származó eszközök (az Excel, Power BI, Tableau, stb.) tooquery több adatbázisból adatok rétegek között. Ezzel a szolgáltatással horizontális felskálázás lekérdezések toolarge adat szintet az SQL-adatbázis és az üzleti intelligenciával jelentések hello eredményeinek képi megjelenítése.


## <a name="why-use-elastic-queries"></a>Rugalmas lekérdezések miért érdemes használni?

**Azure SQL Database**

A lekérdezés teljesen a T-SQL Azure SQL-adatbázisok között. Ez lehetővé teszi a távoli adatbázis lekérdezése csak olvasható. Ez lehetővé teszi a beállítás a meglévő helyszíni SQL Server ügyfelek toomigrate három és négy part nevek vagy csatolt kiszolgáló tooSQL DB használatával.

**Standard csomagban érhető el**

Rugalmas lekérdezési hello szabványos teljesítményszinttel hozzáadása toohello prémium szintű teljesítmény réteg támogatott. Című rész hello az előzetes verzió korlátozásai alatt a teljesítmény alacsonyabb rétegek teljesítményének korlátozásai.

**Leküldéses tooremote adatbázisok**

Rugalmas lekérdezések most tolható paraméterek toohello távoli adatbázisai végrehajtásra.

**A tárolt eljárás végrehajtása**

Távoli tárolt eljárás hívások vagy a távoli függvények használatával hajtható végre [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714).

**Rugalmasság**

Rugalmas lekérdezéssel külső táblák most jelentheti tooremote táblák különböző séma vagy tábla neve.

## <a name="elastic-query-scenarios"></a>Rugalmas lekérdezési forgatókönyvek

hello célja toofacilitate forgatókönyvek, ahol több adatbázis hozzájárul az egyetlen teljes eredmény sorok lekérdezése. hello lekérdezés vagy összeállítható hello felhasználó vagy alkalmazás közvetlenül vagy közvetve eszközökkel, amelyek csatlakoztatott toohello adatbázis. Ez különösen fontos jelentések, kereskedelmi BI vagy adatok-integrációs eszközök használatával létrehozásakor – vagy bármilyen alkalmazás, és nem módosítható. Rugalmas lekérdezéshez lekérheti az eszközök, például az Excel, a Power BI, a Tableau vagy a Cognos hello ismerős SQL Server csatlakozási élmény több adatbázis között.
Egy rugalmas lekérdezés lehetővé teszi, hogy mennyire egyszerű a hozzáférés tooan teljes gyűjteményt az adatbázisok SQL Server Management Studio vagy Visual Studio által kibocsátott lekérdezések keresztül, és megkönnyíti a kereszt-adatbázis lekérdezése az Entity Framework vagy más ORM környezetekben. 1. ábra mutatja egy olyan forgatókönyvet, ahol egy meglévő felhőbeli alkalmazás (hello használó [elastic database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md)) réteg a kiterjesztett adatok épít, és egy rugalmas lekérdezési adatbázisok közötti jelentéskészítéshez használt.

**1. ábra** kiterjesztett adatszinten használt rugalmas lekérdezés

![A kiterjesztett adatszinten rugalmas lekérdezés][1]

Rugalmas lekérdezés forgatókönyvet a következő topológiákat hello jellemző:

* **A vertikális particionálás - adatbázisok közötti lekérdezések** (1. topológia): hello adatok particionálása függőleges egy adatrétegbeli adatbázisok számú között. A táblák más-más részhalmazához általában a különböző adatbázisokhoz elhelyezve. Ez azt jelenti, hogy adott hello sémája nem egyezik meg a különböző adatbázisokhoz. Például a készlet összes tábla esetén egy adatbázis míg nyilvántartási kapcsolatos összes táblázatot egy második adatbázison. A topológia gyakori alkalmazási esetei szükséges egy tooquery között vagy toocompile jelentések adatbázisok táblái között.
* **Vízszintes particionálására - horizontális** (topológia 2): adatok particionálása vízszintesen réteg toodistribute sorok méretezett kimenő adatok között. Ezzel a megközelítéssel hello séma megegyezik a programban részt vevő összes adatbázis. Ez a megközelítés "horizontális" néven is ismert. Horizontális hajtható végre, és (1) hello rugalmas adatbázis eszközök tárak vagy (2) önkiszolgáló-horizontális használatával kezelhetők. Egy rugalmas lekérdezési használt tooquery vagy fordítási jelentések számos szilánkok között.

> [!NOTE]
> Rugalmas lekérdezési működik a legjobban az alkalmi jelentéskészítési forgatókönyvekhez, ahol hello adatrétegbeli hello feldolgozási többsége elvégezhető. Jelentéskészítési munkaterhelést vagy adatraktározási forgatókönyvekben összetettebb lekérdezések esetében is érdemes lehet [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
>  

## <a name="vertical-partitioning---cross-database-queries"></a>A vertikális particionálás - közötti adatbázis-lekérdezések

toobegin kódolási, lásd: [első lépések (a vertikális particionálás) közötti adatbázis-lekérdezés](sql-database-elastic-query-getting-started-vertical.md).

Egy rugalmas lekérdezés lehet használt toomake adatokat egy SQL-adatbázis elérhető tooother SQL-adatbázisban található. Ez lehetővé teszi egy adatbázis toorefer tootables lekérdezéseket bármely más távoli SQL-adatbázisban. hello első lépése az toodefine egy külső adatforrásból a minden egyes távoli adatbázishoz. hello külső adatforrás van definiálva, amelyből el kívánja toogain hozzáférés tootables hello távoli adatbázis található hello helyi adatbázisban. Nem módosult a távoli adatbázis hello szükségesek. A tipikus függőleges particionálási forgatókönyveket, amelyekben a különböző adatbázisokhoz rendelkeznek különböző sémákat rugalmas lekérdezések lehetnek használt tooimplement gyakori alkalmazási esetekben, például a hozzáférés tooreference és adatbázisok közötti lekérdezése.

> [!IMPORTANT]
> Az ALTER ANY külső ADATFORRÁS engedéllyel kell rendelkeznie. Ez az engedély megtalálható hello ALTER DATABASE engedéllyel. Az ALTER ANY külső ADATFORRÁS-engedélyek az alapul szolgáló adatforrás szükséges toorefer toohello is.
>

**Referenciaadatok**: hello topológia hivatkozás adatok felügyeletére használható. Hello az alábbi ábra a referenciaadatok két tábla (T1 és T2) továbbra is egy dedikált adatbázison. Rugalmas lekérdezéssel, érhetők el a T1 és T2 táblák távolról más adatbázisból származó hello ábrán látható módon. Ha referencia táblákat kis vagy távoli referencia táblába lekérdezések 1 topológiájának használata szelektív predikátumok rendelkezik.

**2. ábra** vertikális particionálás - rugalmas lekérdezési tooquery referenciaadatok használatával

![Vertikális particionálás - rugalmas lekérdezési tooquery referenciaadatok használatával][3]

**Adatbázisok közötti lekérdezése**: rugalmas lekérdezések lehetővé teszik a használati esetek igénylő számos SQL-adatbázisok közötti lekérdezése. 3. ábrán látható négy különböző adatbázist: CRM-hez, a szoftverleltár, a HR és a termékek. Hello adatbázisok közül az egyik végzett lekérdezések is kell elérhető tooone, vagy minden más adatbázisok hello. Rugalmas lekérdezéssel, beállíthatja az adatbázis ebben az esetben egyes hello négy adatbázisok néhány egyszerű DDL-utasításokban futtatásával. Ez egyszeri konfigurálás után hozzáférést tooa távoli tábla egyszerűen tooa helyi táblára hivatkozik, a T-SQL-lekérdezések, illetve a BI-eszközökkel. Ezt a módszert akkor ajánlott, ha a távoli lekérdezéseknél hello nem eredményeket nagy.

**3. ábra** vertikális particionálás - különböző adatbázisok közötti rugalmas lekérdezési tooquery használatával

![Vertikális particionálás - különböző adatbázisok közötti rugalmas lekérdezési tooquery használatával][4]

hello lépések konfigurálása a hozzáférés tooa táblában található hello a távoli SQL-adatbázisok azonos igénylő függőleges particionálási forgatókönyvek rugalmas adatbázis-lekérdezéseinek séma:

* [Hozzon létre FŐKULCS](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* [KÜLSŐ ADATFORRÁS létrehozása/DROP](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource típusú **RDBMS**
* [A külső tábla létrehozása/DROP](https://msdn.microsoft.com/library/dn935021.aspx) táblanév

Miután hello DDL-utasításokban, hello távoli tábla "mytable", mintha egy helyi táblára érheti el. Az Azure SQL Database automatikusan kapcsolat toohello távoli adatbázis megnyitása, feldolgozza a kérést a távoli adatbázis hello és hello eredményeket ad vissza.

## <a name="horizontal-partitioning---sharding"></a>Vízszintes particionálás - horizontális
A jelentéskészítési feladatok keresztül a szilánkos, azaz, vízszintesen particionálva, rugalmas lekérdezési tooperform adatszinten szükség van egy [rugalmas adatbázis shard térkép](sql-database-elastic-scale-shard-map-management.md) toorepresent hello adatbázisok hello adatok réteg. Általában csak egyetlen shard térképre használatban van ebben a forgatókönyvben, és rugalmas lekérdezési lehetőségeket (átjárócsomópont) dedikált adatbázis lekérdezések jelentéskészítéshez hello belépési pontként szolgál. Csak a dedikált adatbázis hozzáférés toohello shard térkép kell. 4. ábra szemlélteti, ez a topológia és hello rugalmas lekérdezési adatbázis és a shard hozzárendelése és konfigurációja. hello adatrétegbeli hello adatbázisok lehet bármely Azure SQL Database verzióját vagy kiadását. Hello elastic database ügyféloldali kódtár és shard maps létrehozásával kapcsolatos további információkért lásd: [Shard térkép felügyeleti](sql-database-elastic-scale-shard-map-management.md).

**4. ábra** vízszintes particionálás - rugalmas lekérdezéssel felett horizontálisan skálázott adatok rétegek jelentéskészítéshez

![Vízszintes particionálás - rugalmas lekérdezéssel felett horizontálisan skálázott adatok rétegek jelentéskészítéshez][5]

> [!NOTE]
> A rugalmas adatbázis lekérdezése (átjárócsomópont) lehet külön adatbázis, vagy hello lehet ugyanaz a gazdagépek hello shard térkép adatbázis. Bármilyen konfiguráció választja, győződjön meg arról, hogy az adatbázis szolgáltatás- és rétegű ez elég magas toohandle hello várható időtartam bejelentkezés/lekérdezésekre vonatkozó kérelmek.

hello lépések konfigurálja a vízszintes particionálási igénylő forgatókönyvek hozzáférés tooa set tábla található rugalmas adatbázis-lekérdezéseinek (általában) több távoli SQL-adatbázisok:

* [Hozzon létre FŐKULCS](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* Hozzon létre egy [shard térkép](sql-database-elastic-scale-shard-map-management.md) képviselő az adatréteg hello elastic database ügyféloldali kódtár segítségével.   
* [KÜLSŐ ADATFORRÁS létrehozása/DROP](https://msdn.microsoft.com/library/dn935022.aspx) típusú mydatasource **SHARD_MAP_MANAGER**
* [A külső tábla létrehozása/DROP](https://msdn.microsoft.com/library/dn935021.aspx) táblanév

Miután elvégezte ezeket a lépéseket, érheti el hello vízszintesen particionált tábla "mytable", mintha egy helyi táblára. Az Azure SQL Database automatikusan megnyílik több párhuzamos kapcsolatok toohello távoli adatbázis hello táblák fizikailag tároló, a távoli adatbázis hello hello kérelmeket dolgozza fel, és hello eredményeket ad vissza.
További információ a hello vízszintes particionálási forgatókönyv találhatók a szükséges hello lépéseket [vízszintes particionálására rugalmas lekérdezési](sql-database-elastic-query-horizontal-partitioning.md).

toobegin kódolási, lásd: [Ismerkedés a vízszintes particionálására (horizontális) rugalmas lekérdezési](sql-database-elastic-query-getting-started.md).

## <a name="t-sql-querying"></a>T-SQL-lekérdezés
Miután meghatározta a külső adatforrások és a külső táblák, használhatja rendszeres SQL Server kapcsolati karakterláncok tooconnect toohello adatbázisok ahol definiálva a a külső táblák. Ezt követően futtathatja T-SQL-utasítások a külső táblákon végrehajtott a kapcsolat az alábbiakban leírt hello korlátozásokkal. További információt és példákat a T-SQL-lekérdezések találhatók hello dokumentáció témakörök a [vízszintes particionálás](sql-database-elastic-query-horizontal-partitioning.md) és [vertikális particionálás](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Eszközök kapcsolattal
Rendszeres SQL Server kapcsolati karakterláncok tooconnect is használhatja, az alkalmazások és BI vagy adatok integrációs eszközök toodatabases, amelyek külső táblákon. Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként. Miután csatlakozott, tekintse meg a toohello rugalmas lekérdezési adatbázis és a hello külső tábla az adatbázis, ahogy kellene tennie bármely egyéb SQL Server adatbázis csatlakozni, toowith az eszköz.

> [!IMPORTANT]
> Hitelesítés az Azure Active Directoryval rugalmas lekérdezési jelenleg nem támogatott.
> 
> 

## <a name="cost"></a>Költségek
Rugalmas lekérdezési szerepel, az Azure SQL Database adatbázisok hello költségét. Vegye figyelembe, hogy a távoli adatbázisokhoz belül hol áll egy másik adatközpont, mint a Rugalmas lekérdezési végpont hello topológiák támogatottak, de a távoli adatbázisokhoz adatforgalommal regular van szó, [Azure díjszabás](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Előzetes verzió korlátozásai
* Az első rugalmas lekérdezés futtatása is eltarthat, tooa hello szabványos teljesítmény-rétegen néhány percig. Most szükség tooload hello rugalmas lekérdezési funkcionalitás; teljesítmény betöltése növeli a magasabb teljesítmény rétegekkel.
* A külső adatforrások vagy a külső táblákhoz az SSMS vagy az SSDT Scripting még nem támogatott.
* Importálási/exportálási az SQL DB egyelőre nem támogatják a külső adatforrások és a külső táblákra. Ha toouse Import/Export van szüksége, dobható el, ezek az objektumok exportálása előtt, és ezután hozza létre őket importálása után.
* Rugalmas lekérdezés csak jelenleg csak olvasási hozzáféréssel tooexternal táblákat. Azonban használhatja teljes T-SQL funkciókat hello adatbázison ahol hello külső tábla meg van adva. Ez akkor lehet hasznos, például az ideiglenes eredmények megőrzése használ, például VÁLASSZA < column_list > < local_table > azokat, vagy toodefine tárolt eljárások hello rugalmas adatbázis lekérdezése tooexternal táblák vonatkoznak.
* Típus: nvarchar(max), kivéve a LOB-típusok nem támogatottak a külső tábla definíciókat. Megoldás hozhat létre hello távoli adatbázis hello LOB típusú adat típus: nvarchar(max) kerül egy nézet, a külső tábla megadása helyett hello alaptábla hello nézet keresztül és majd típussá hello eredeti LOB típusú újra üzembe a lekérdezésekben.
* A külső táblákon végrehajtott oszlop statisztikai adatainak jelenleg nem támogatottak. Táblák statisztika támogatottak, de manuálisan létrehozott toobe kell.

## <a name="feedback"></a>Visszajelzés
Ossza meg a felhasználói élmény kapcsolatos visszajelzéseket rugalmas lekérdezési betartásának vizsgálatára disqus-beszélgetésben teheti az alábbi, hello MSDN fórumain, vagy a Stackoverflow. Azt is hello szolgáltatást (hibák, nyers szélén, a szolgáltatás hézagok) visszajelzést különböző.

## <a name="next-steps"></a>Következő lépések

* Függőleges particionálási oktatóanyagért lásd a [első lépések (a vertikális particionálás) közötti adatbázis-lekérdezés](sql-database-elastic-query-getting-started-vertical.md).
* A szintaxis és a minta lekérdezések függőleges particionált adatok, lásd: [adatok lekérdezése függőleges particionálva)](sql-database-elastic-query-vertical-partitioning.md)
* Vízszintes particionálás (horizontális) oktatóanyagért lásd a [Ismerkedés a vízszintes particionálására (horizontális) rugalmas lekérdezési](sql-database-elastic-query-getting-started.md).
* A szintaxis és a minta lekérdezések vízszintesen particionált adatok, lásd: [adatok vízszintesen lekérdezése particionálva)](sql-database-elastic-query-horizontal-partitioning.md)
* Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
