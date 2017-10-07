---
title: "aaaWhat egy hello Azure SQL Database szolgáltatás? | Microsoft Docs"
description: "Egy bevezető tooSQL adatbázis beolvasása: technikai részletei és funkciói a Microsoft relációs adatbázis-kezelő rendszerének (RDBMS) hello felhőben."
keywords: "Bevezetés toosql, a bevezetés toosql, mi az az sql-adatbázis"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: cgronlun
ms.assetid: c561f600-a292-4e3b-b1d4-8ab89b81db48
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/30/2017
ms.author: carlrab
ms.openlocfilehash: 24ca383ad7e140133d19debc8899f172ee4aa424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-azure-sql-database-service"></a>Mi az hello Azure SQL Database szolgáltatás? 

Az SQL Database általános célú relációs adatbázis-szolgáltatás a Microsoft Azure-ban, amely egyebek mellett relációs, JSON-, térbeli és XML-struktúrákat is támogat. [Dinamikusan méretezhető teljesítményt](sql-database-service-tiers.md) nyújt és olyan lehetőségeket kínál, mint az [oszlopcentrikus indexelés](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) kivételes mélységű elemzéshez és jelentéskészítéshez, és a [memóriabeli OLTP](sql-database-in-memory.md) a kivételesen nagy teljesítményű tranzakció-feldolgozáshoz. A Microsoft kezeli az összes javítását és zökkenőmentesen hello SQL kódbázis frissítése, és kivonatolja számítógépnél az alapul szolgáló infrastruktúra hello az összes felügyeleti. 

SQL-adatbázis a kódbázis osztanak meg hello [Microsoft SQL Server adatbázismotor](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation). A Microsoft cloud-első stratégia hello elérhető legújabb képességeket az SQL Server áll kiadott első tooSQL adatbázis, és tooSQL kiszolgálójának nevével. Ez a megközelítés lehetővé teszi hello legújabb SQL Server funkciói a javítás céljából nincs terhelés, vagy frissíti - és az új funkciók vizsgálni több millió adatbázisok között. A bejelentett új funkciókról az alábbi helyeken kaphat tájékoztatást:

- **[SQL adatbázis Azure menetrend](https://azure.microsoft.com/roadmap/?category=databases)**: egy sor toofind újdonságok, és mi várható tovább. 
- **[Azure SQL Database blog](https://azure.microsoft.com/blog/topics/database)**: Itt az SQL Server csapatának tagjai írnak az SQL Database újdonságairól és funkcióiról. 

A több szolgáltatási szinten is kiszámítható teljesítményt nyújtó SQL Database dinamikus méretezhetőséget kínál állásidő nélkül, beépített intelligens optimalizálással, globális méretezhetőséggel és rendelkezésre állással és fejlett biztonsági beállításokkal – mindezt szinte adminisztráció nélkül. Ezek a képességek lehetővé teszik az alkalmazások gyors fejlesztésére és az idő toomarket felgyorsítása, hanem értékes időt és erőforrásokat toomanaging virtuális gépek és infrastruktúra toofocus. hello szolgáltatás jelenleg 38 adatokat az SQL-adatbázis erőforrásokból hello világ további adatközpontok online állapotba kerüljön, amely lehetővé teszi toorun az adatbázis egy adott adatközpont környéken.

> [!NOTE]
> Az Azure platformbiztonságáról az [Azure biztonsági és adatkezelési központ](https://azure.microsoft.com/support/trust-center/security/) részben talál információkat.
>

## <a name="scalable-performance-and-pools"></a>Méretezhető teljesítmény és készletek

Az SQL Database-zel minden adatbázis önálló, hordozható, és saját garantált teljesítményt nyújtó [szolgáltatásszinttel](sql-database-service-tiers.md) rendelkezik. SQL-adatbázis különböző teljesítményszintet biztosít a különböző igényeinek, és lehetővé teszi, hogy a adatbázisok toobe készletezett toomaximize hello erőforrások használatát és költségtakarékosabb munkavégzésben.

### <a name="adjust-performance-and-scale-without-downtime"></a>Teljesítmény módosítása és skálázása leállási idő nélkül

SQL-adatbázis kínál négy szolgáltatásszintek toosupport egyszerűsített tooheavyweight adatbázis számítási feladatait: Basic, Standard, Premium és prémium RS. Kisméretű, egyetlen adatbázison alacsony költséggel havonta az első alkalmazás elkészítésére, és módosítsa a szolgáltatási rétegben manuálisan vagy programon keresztül, a megoldás bármely idő toomeet hello igényeinek. Módosíthatja a teljesítmény tooyour alkalmazás vagy tooyour ügyfelek állásidő nélkül. Dinamikus méretezhetőség lehetővé teszi, hogy az adatbázis tootransparently válaszolni toorapidly erőforrás-követelmények és lehetővé teszi, hogy Ön tooonly díj ellenében hello erőforrásokat kell, amikor szüksége van rájuk.

   ![méretezés](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="elastic-pools-toomaximize-resource-utilization"></a>Rugalmas készletek toomaximize erőforrás-használat

Számos vállalkozás és alkalmazás képes toocreate önálló adatbázisok alatt, és igény szerinti teljesítmény felfelé vagy lefelé van elegendő, különösen ha használati minták viszonylag jól jelezhetők előre. De ha előre nem látható használati minták, azt teheti, hogy rögzített toomanage költségek és az üzleti modell. [Rugalmas készletek](sql-database-elastic-pool.md) tervezett toosolve van probléma. hello fogalma egyszerű. Egyedi adatbázis helyett a teljesítmény erőforrások tooa készletet foglal le, és fizessen hello teljesítményéért erőforrások hello készlet, nem pedig az önálló adatbázisok teljesítményét. 

   ![rugalmas készletek](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

A rugalmas készletek toofocus a tárcsázás adatbázis teljesítményének növekszik és csökken, igény szerinti erőforrások ingadozik, nincs szükség. hello készletezett adatbázisok erőforrást hello teljesítmény hello rugalmas készlet igény szerint. Készletezett adatbázisok felhasználását, de nem lépik túl hello készlet hello korlátai által megszabott, a költség marad előre jelezhető, még akkor is, ha egyes adatbázis-használat nem. Mi az további, akkor [hozzá és távolíthat el az adatbázisok toohello készlet](sql-database-elastic-pool-manage-portal.md), skálázás adatbázisok toothousands költségvetést, amelyek belül az összes adatbázisra skálázhatja az alkalmazást. Is vezérlő hello minimális és maximális erőforrások elérhető toodatabases a hello készlet tooensure hello készletben adatbázis a teljes alkalmazáskészlet-erőforrásokat, és hogy minden készletezett adatbázis garantált minimális mennyiségű erőforrást hello. További információ a rugalmas készleteket használó SaaS-alkalmazások szerkezeti kialakításainak toolearn lásd [Tervminták több-bérlős SaaS-alkalmazásokhoz az SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="blend-single-databases-with-pooled-databases"></a>Önálló adatbázisok beolvasztása a készletezett adatbázisokba

Akár önálló adatbázisokat, akár rugalmas készleteket választ, a későbbiekben is sok mindent megváltoztathat. Önálló adatbázisokat rugalmas készletek, és módosítsa a hello szolgáltatási szinteket az önálló adatbázisok és rugalmas készletek gyors és egyszerű tooadapt tooyour helyzet. Hello teljesítménye és elérése révén az Azure, az lehetővé teszi a vegyes és-egyezést más Azure SQL Database toomeet az egyedi alkalmazás tervezési igények, a meghajtó költségeket, és a erőforrás hatékonyság szolgáltatásokhoz, és új üzleti lehetőségek kiaknázása.

### <a name="extensive-monitoring-and-alerting-capabilities"></a>Széles körű figyelési és riasztási funkciók

De hogyan is összehasonlítja hello önálló adatbázisok és rugalmas készletek relatív teljesítménye? Honnan tudhatja hello kattintson a jobb gombbal-stop felfelé és lefelé tárcsázás? Hello használata [beépített teljesítményfigyelési](sql-database-performance.md) és [riasztási](sql-database-insights-alerts-portal.md) eszközök hello teljesítmény értékelések alapján együtt [az önálló adatbázisok adatbázis-tranzakciós egységek (dtu-k) és rugalmas dtu-i (edtu-k) rugalmas készletek](sql-database-what-is-a-dtu.md). Ezen eszközök segítségével gyorsan felmérheti hello hatása skálázás felfelé vagy lefelé alapján a jelenlegi vagy a teljesítményigény projektre. A részletekért olvassa el [Az SQL Database beállításai és teljesítménye: mi érhető el az egyes szolgáltatásszinteken](sql-database-service-tiers.md) című részt.

Az SQL Database emellett [metrikák és diagnosztikai naplók kibocsátásával](sql-database-metrics-diag-logging.md) is képes megkönnyíteni a felügyeletet. SQL-adatbázis toostore erőforrás-használat, a munkavállalók és a munkamenetek és a kapcsolat egy Azure erőforrásainak konfigurálhatja:

- **Azure Storage**: Nagy tömegű telemetriai adat alacsony költségű archiválására
- **Azure Event Hub**: Az SQL Database telemetriai adatainak integrálásra saját egyedi figyelési megoldásokkal vagy élő adatfolyamatokkal
- **Azure Log Analytics**: Beépített figyelési megoldás jelentéskészítő, riasztó és a hibák következményeit enyhítő funkciókkal

    ![architektúra](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Rendelkezésre állás

Az Azure szolgáltatói szerződésében [(SLA)](http://azure.microsoft.com/support/legal/sla/) az ágazatban élenjáró módon 99,99 százalékos elérhetőséget biztosítunk – a Microsoft által kezelt adatbázisok globális hálózata teszi lehetővé, hogy alkalmazása a hét mind a 7 napján, napi 24 órában fusson. Az SQL Database ezen felül olyan beépített funkciókkal szolgálja [az üzletmenet folytonosságát és a globális méretezhetőséget](sql-database-business-continuity.md), mint például a következők:

- **[Automatikus biztonsági mentések](sql-database-automated-backups.md)**: Az SQL Database automatikusan készít teljes, differenciális és tranzakciónapló-alapú biztonsági mentéseket.
- **[Időpontban visszaállítások](sql-database-recovery-using-backups.md)**: SQL-adatbázis helyreállítási tooany pont támogatja hello automatikus biztonsági mentés megőrzési időn belül.
- **[Aktív georeplikáció](sql-database-geo-replication-overview.md)**: SQL-adatbázis lehetővé teszi, hogy tooconfigure toofour olvasható másodlagos mentése vagy hello adatbázisok azonos vagy globálisan elosztott az Azure-adatközpont.  Például ha nagyszámú egyidejű olvasási tranzakciókat tartalmazó adatbázissal katalógus egy SaaS-alkalmazáshoz, használjon aktív georeplikáció tooenable globális olvasható méretezési és szűk keresztmetszeteinek megszüntetéséhez hello elsődleges, amelyek miatt tooread munkaterhelések. 
- **[Feladatátvételi csoportok](sql-database-geo-replication-overview.md)**: SQL-adatbázis lehetővé teszi a tooenable magas rendelkezésre állás és a terheléselosztási globális léptékű, többek között a transzparens georeplikáció és a nagy adatbázisok és rugalmas készletek feladatátvételi. Feladatátvételi csoportok és aktív georeplikáció lehetővé teszi, hogy létrehozása adminisztrációs terhet hagyja az összes hello összetett figyeléséhez, Útválasztás és feladatátvételi vezénylési tooSQL adatbázis globálisan elosztott SaaS-alkalmazásokhoz.

## <a name="built-in-intelligence"></a>Beépített intelligencia

Az SQL Database beépített funkciói, amely segít jelentősen csökkenti futtatásáért és felügyeletéért felelős adatbázisok hello költségeit, és a lehető legnagyobbra növeli a teljesítményt és a az alkalmazás biztonsági beolvasása. Több millió felhasználói 24 órás munkaterheket futtatnak, SQL-adatbázis gyűjt, és teljes mértékben betartása mellett az ügyfelek adatvédelmének hello háttérben dolgozza fel a telemetriai adatokat, a nagy mennyiségű. Különböző algoritmusokat folyamatosan értékelése hello telemetriai adatokat, hogy hello szolgáltatás ismerje meg, és alkalmazkodnak az alkalmazással. Az elemzés alapján, hello szolgáltatás felmerül és a teljesítmény javítása a következőkhöz ideális tooyour adott munkaterhelés javaslatok. 

### <a name="automatic-performance-tuning"></a>A teljesítmény automatikus finomhangolása

SQL Database hello részletes betekintést lekérdezi, hogy kell-e toomonitor is tartalmaz. SQL-adatbázis az adatbázis-minták értesül, és lehetővé teszi, hogy a hogy tooadapt az adatbázis-séma tooyour munkaterhelés. Az SQL Database az [SQL Database Advisor](sql-database-advisor.md) segítségével javaslatokat tesz a teljesítmény finomhangolására, amelyeket Ön felülvizsgálhat és alkalmazhat. Az adatbázisok folyamatos figyelése azonban nehéz, fárasztó feladat, különösen akkor, ha sok adatbázisról van szó. Adatbázisok hatalmas számú kezelése lehet lehetetlen toodo hatékonyan még az összes elérhető eszközöket és jelentéseket SQL Database és az Azure-portálon. Helyett figyelése, és az adatbázis manuálisan beállítása, érdemes lehet néhány hello figyelése és műveletek tooSQL beállítása delegálása adatbázis automatikus hangolási funkció használata. SQL-adatbázis automatikusan alkalmazza a javaslatok, teszteket, és ellenőrzi az egyes műveletek tooensure hangolási hello teljesítményének javítása tartja. Ezzel a módszerrel SQL-adatbázis automatikusan alkalmazkodik tooyour munkaterhelés ellenőrzött és biztonságos módon. Automatikus hangolása azt jelenti, hogy az adatbázis teljesítményét hello gondosan ellenőrzött és előtt és után minden hangolási művelet képest, hello teljesítmény nem javul, ha hello hangolása művelet van-e vissza.

A partnerek futtató ma, több [több-bérlős Szolgáltatottszoftver-alkalmazásoknál](sql-database-design-patterns-multi-tenancy-saas-applications.md) automatikus teljesítményhangolás, az alkalmazások mindig rendelkezik-e stabil és kiszámítható teljesítmény toomake vannak függő SQL-adatbázis felett. A számukra Ez a szolgáltatás rendkívüli mértékben csökkenti a hello kockázatot a teljesítmény, amelyhez hello éjszakai hello közepén. Ezenkívül, mivel a vásárlói bázisunk részét is használja az SQL Server, használják hello SQL-adatbázis toohelp által biztosított azonos indexelési javaslatok az SQL Server-ügyfeleknek.

Az SQL Database két szempont alapján képes automatikus finomhangolást végezni:

- **[Automatikus indexkezelés](sql-database-automatic-tuning.md#automatic-index-management)**: Azonosítja az adatbázishoz hozzáadandó és az abból eltávolítandó indexeket.
- **[Automatikus terv-korrekció](sql-database-automatic-tuning.md#automatic-plan-choice-correction)**: Azonosítja a hibás terveket és javítja az SQL tervek teljesítmény-problémáit (hamarosan megjelenik, az SQL Server 2017-ben már elérhető).

### <a name="adaptive-query-processing"></a>Adaptív lekérdezés-feldolgozás

Azt is való felvételekor hello [adaptív lekérdezés feldolgozása](/sql/relational-databases/performance/adaptive-query-processing) operációsrendszer-funkciókat tooSQL adatbázis, beleértve több utasításból álló táblázat értékű függvények kihagyásos végrehajtási kötegelt módban memória adjon visszajelzést, és kötegelt módban adaptív illesztések . További útmutatás nyújtása a cím problémák kapcsolódó toohistorically intractable lekérdezés optimalizálási teljesítményproblémákat hasonló "ismerje meg, és alkalmazkodáshoz" technikák egyes adaptív lekérdezés feldolgozása szolgáltatásokról vonatkozik.

### <a name="intelligent-threat-detection"></a>Intelligens fenyegetésészlelés

 [SQL Fenyegetésészlelés](sql-database-threat-detection.md) kihasználja [SQL Database auditing](sql-database-auditing.md) toocontinuously figyelése Azure SQL-adatbázisok a potenciálisan káros kísérletek tooaccess bizalmas adatokat. A fenyegetésészlelés SQL biztonsági, amely lehetővé teszi, hogy az ügyfelek toodetect és toopotential fenyegetések válaszol, a rendellenes tevékenységek adja meg a biztonsági riasztások előforduló egy új réteget biztosít. A felhasználók riasztást kapnak a gyanús adatbázis-tevékenységekről, a lehetséges biztonsági résekről, az SQL-injektálásos támadásokról, valamint a rendellenes adatbázis-hozzáférési mintákról. SQL fenyegetés észlelési riasztásokat gyanús tevékenység részleteinek megadása, és hogyan művelet javasolja tooinvestigate és hello fenyegetést. Felhasználók hello gyanús események toodetermine felfedezheti, ha hello esemény eredményei egy kísérlet tooaccess megsértik, vagy kihasznál hello adatbázis adatait. A fenyegetésészlelés teszi egyszerű tooaddress potenciális fenyegetések toohello adatbázis hello kell toobe egy biztonsági szakértő nélkül, vagy speciális biztonsági rendszerek figyelése kezelése.

## <a name="advanced-security-and-compliance"></a>Magas szintű biztonság és megfelelőség

SQL Database is tartalmaz számos [beépített biztonsági és megfelelőségi szolgáltatásokat](sql-database-security-overview.md) toohelp az alkalmazás megfelelő különféle biztonsági és megfelelőségi követelményeknek. 

### <a name="auditing-for-compliance-and-security"></a>Naplózás a megfelelőség és biztonság szolgálatában

[SQL Database Auditing](sql-database-auditing.md) követi az adatbázisban, és beírja őket az Azure-tárfiókot tooan naplót. A naplózás segíthet a jogszabályi megfelelőség fenntartásában és az adatbázison végzett tevékenység megértésében, valamint az esetleg üzleti veszélyeket vagy biztonsági problémákat jelző rendellenességek feltárásában.

### <a name="data-encryption-at-rest"></a>Adat-titkosítás inaktív állapotban

SQL-adatbázis [átlátható adattitkosítás](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database) tranzakció naplófájlok nyugalmi és valós idejű titkosítási és visszafejtési hello adatbázis, a társított biztonsági másolatok, kártevő szándékú tevékenységek hello fenyegetés elleni segítségével anélkül, hogy a módosítások toohello alkalmazás. 2017 májusától minden újonnan létrehozott Azure SQL adatbázis automatikusan transzparens adattitkosítás (TDE) védelmet kap. TDE SQL bizonyítása nyugalmi titkosítási technológia, amely sok megfelelőségi szabványok tooprotect adathordozók lopás elleni szükséges. Az ügyfelek hello TDE titkosítási kulcsokat és más titkos adatokat is kezelheti az Azure Key Vault segítségével biztonságos és megfelelő módon.

### <a name="data-encryption-in-motion"></a>Adattitkosítás menet közben

SQL-adatbázis hello csak adatbázis toooffer Rendszervédelem útban, inaktív és feldolgozására lekérdezés során a bizalmas adatok [mindig titkosítja](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine). Mindig titkosított egy iparági-első unparalleled adatok kínál hello lopás a kulcsfontosságú adatokat érintő problémák elleni biztonsági. Például mindig titkosítja, az ügyfelek hitelkártyaszámok tárolja titkosított formában hello adatbázis mindig, még akkor is, a lekérdezés feldolgozása közben lehetővé visszafejtési használati hello ponton engedélyezett személyzeti vagy tooprocess igénylő alkalmazások adatokat.

### <a name="dynamic-data-masking"></a>Dinamikus adatmaszkolás

[SQL-adatbázis dinamikus adatmaszkolási](sql-database-dynamic-data-masking-get-started.md) bizalmas adatok veszélyeztetettségének korlátozása maszkolás azt toonon jogosultságú felhasználók által. Dinamikus adatmaszkolási megakadályozza, hogy jogosulatlan hozzáférés toosensitive adatokat azáltal, hogy az ügyfelek toodesignate mennyi hello bizalmas adatok tooreveal hello alkalmazás réteg gyakorolt minimális hatás mellett. Egy csoportházirend-alapú biztonsági funkció, hello bizalmas adatok hello eredménykészletben lekérdezés elrejti a kijelölt adatbázis mezők keresztül közben hello adatbázis hello adatai nem változik.

### <a name="row-level-security"></a>Sorszintű biztonság

[A sorszintű biztonsági](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) lehetővé teszi, hogy az ügyfelek toocontrol hozzáférés toorows egy adatbázis tábláinak hello jellemzők hello felhasználó lekérdezése alapján (például a csoport tagságát vagy végrehajtási környezet által). A sorszintű biztonságot (RLS) egyszerűbbé teszi a hello tervezési és az alkalmazás biztonsági kódolása. RLS lehetővé teszi sor adatelérési tooimplement korlátozásait. Például annak érdekében, hogy munkavállalók férhessenek hozzá csak ezen adatok sorokat, amelyek a megfelelő tootheir részleg, vagy a felhasználói adatok hozzáférés tooonly hello adatok vonatkozó tootheir vállalati korlátozása.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-integráció és többtényezős hitelesítés

SQL-adatbázis lehetővé teszi, hogy akkor toocentrally adatbázis-felhasználó és más Microsoft-szolgáltatásokban az identitások kezeléséhez [Azure Active Directory-integráció](sql-database-aad-authentication.md). Ez a funkció egyszerűsíti az engedélyek kezelését és fokozza a biztonságot. Az Azure Active Directory támogatja [a multi-factor authentication](sql-database-ssms-mfa-authentication.md) (MFA) tooincrease adat és alkalmazás támogatása egyetlen rang a folyamat közben biztonsági.

### <a name="compliance-certification"></a>Megfelelőségi tanúsítvány

Az SQL Database rendszeres ellenőrzéseken vesz részt és számos megfelelőségi szabványnak tesz eleget tanúsított módon. További információkért lásd: hello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), ahol hello legfrissebb listáját megtalálja [SQL-adatbázis megfelelőségi minősítései közül](https://azure.microsoft.com/support/trust-center/services/).

## <a name="easy-to-use-tools"></a>Egyszerűen használható eszközök

Az SQL Database egyszerűbbé és hatékonyabbá teszi az alkalmazások létrehozását és karbantartását. SQL-adatbázis lehetővé teszi a legjobb toofocus: kiváló alkalmazások készítését. Az SQL Database-ben a már meglévő eszközeivel és szakértelmével dolgozhat és fejleszthet.

- **[Azure-portálon hello](https://portal.azure.com/)**: egy webalapú alkalmazást az összes Azure-szolgáltatások kezelése 
- **[SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)**: egy ingyenes, letölthető ügyfél-alkalmazást az SQL Server tooSQL adatbázis bármely SQL infrastruktúra kezelése
- **[SQL Server Data Tools a Visual Studióban](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)**: Ingyenes, letölthető ügyféloldali alkalmazás SQL Server relációs adatbázisok, Azure SQL adatbázisok, Integration Services csomagok, Analysis Services adatmodellek, és Reporting Services-jelentések fejlesztéséhez.
- **[A Visual Studio Code](https://code.visualstudio.com/docs)**: egy ingyenes, letölthető, nyissa meg a forrás, a kód szerkesztése a Windows, a macOS és a Linux, amely támogatja a bővítményeket, ezek közé tartoznak a hello [mssql bővítmény](https://aka.ms/mssql-marketplace) Microsoft SQL Server lekérdezése Az Azure SQL Database, és az SQL Data Warehouse.

SQL-adatbázis támogatja a Python, Java, Node.js, PHP, Ruby, és a hello MacOS .NET, Linux és Windows épület-alkalmazásokat. SQL-adatbázis által támogatott hello azonos [adatkapcsolattárak](sql-database-libraries.md) és SQL Server.

## <a name="engage-with-hello-sql-server-engineering-team"></a>Az SQL Server mérnöki csapathoz hello megszólítása

- [DBA-veremcsere](https://dba.stackexchange.com/questions/tagged/sql-server): Kérdések az adatbázis rendszergazdájának
- [Veremtúlcsordulás](http://stackoverflow.com/questions/tagged/sql-server): Kérdések a fejlesztőknek
- [MSDN fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?category=sqlserver): Műszaki kérdések
- [Microsoft Connect](https://connect.microsoft.com/SQLServer/Feedback): Hibák jelentése és funkciók kérése
- [Reddit](https://www.reddit.com/r/SQLServer/): Az SQL Server megvitatása

## <a name="next-steps"></a>Következő lépések

- Lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/sql-database/) önálló adatbázisok és rugalmas készletek költségeinek összehasonlításáért és számító eszközét.

- Tekintse meg a quick elindul használatba tooget:

  - [SQL-adatbázis létrehozása hello Azure-portálon](sql-database-get-started-portal.md)  
  - [Hozzon létre egy SQL-adatbázis hello Azure parancssori felület](sql-database-get-started-cli.md)
  - [SQL Database létrehozása PowerShell használatával](sql-database-get-started-powershell.md)

- Több Azure CLI és PowerShell-mintát talál itt:
  - [Azure CLI-minták az SQL Database-hez](sql-database-cli-samples.md)
  - [Azure PowerShell-minták az SQL Database-hez](sql-database-powershell-samples.md)
