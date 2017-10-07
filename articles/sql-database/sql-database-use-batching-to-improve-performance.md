---
title: "az Azure SQL Database alkalmazásteljesítmény tooimprove kötegelés aaaHow toouse"
description: "hello témakör igazolja, hogy kötegelés adatbázis-műveletek nagy mértékben imroves hello sebesség és a méretezhetőség, az Azure SQL adatbázis-alkalmazások. Bár ezek a technológiák kötegelési bármely SQL Server-adatbázis használatához hello hello a cikk célja az Azure-on."
services: sql-database
documentationcenter: na
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 563862ca-c65a-46f6-975d-10df7ff6aa9c
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 124b203ee69c595f0813852ff09ef9ec6841233a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-batching-tooimprove-sql-database-application-performance"></a>Hogyan tooimprove SQL-adatbázis teljesítményének kötegelés toouse
Műveletek tooAzure SQL-adatbázis kötegelés jelentősen javítja a hello teljesítményét és méretezhetőségét, az alkalmazások. Ez a cikk első része hello rendelés toounderstand hello előnyeit, néhány minta teszteredmények összehasonlító szekvenciális és kötegelt kérelmek tooa SQL-adatbázis foglalja magában. hello hello cikk hátralévő megjeleníti a hello technikák, forgatókönyvek és szempontok toohelp toouse, az Azure-alkalmazásokban sikeresen kötegelés.

## <a name="why-is-batching-important-for-sql-database"></a>Miért van kötegelés fontos az SQL Database?
Egy jól ismert stratégia növelését a teljesítmény és méretezhetőség hívások tooa távoli szolgáltatás kötegelés. Nincs fix költségek tooany interakció, a távoli szolgáltatás, például a szerializálás, a hálózati átvitel és a deszerializálás feldolgozása. Ezek a költségek azokat a kötegek sok külön tranzakciókat minimálisra csökkenti.

A dokumentum azt szeretnénk tooexamine különböző SQL-adatbázis kötegelési stratégiák és forgatókönyvek. Bár ezek stratégiák is fontos SQL Server használó helyszíni alkalmazások esetén, több oka konzolban SQL-adatbázis kötegelés hello használata:

* Nincs potenciálisan nagyobb hálózati késés SQL-adatbázis eléréséhez, különösen akkor, ha az SQL-adatbázis a külső hello érnek ugyanazt a Microsoft Azure-adatközpont.
* SQL-adatbázis azt jelenti, hogy hello adatok hatékonyságát hello több-bérlős jellemzői hello hozzáférési réteg korrelálja toohello hello adatbázis teljes méretezhetőségét. SQL-adatbázis az egyes bérlői felhasználók megakadályozása kell legaktívabbak adatbázis erőforrások toohello hátrányára a többi bérlő. Válasz toousage, amelyek átlépik ezt az előre definiált kvótákat, az SQL-adatbázis átviteli csökkentheti vagy szabályozási kivételeket válaszolni. Hatékonyság, például a kötegelés, engedélyezze azt toodo munka a SQL-adatbázis a működés felső korlátjának elérése előtt. 
* Kötegelés esetében is, amelyek több adatbázist (horizontális) architektúrára való hatékony. az adatbázis tárolóegységekhez való együttműködéshez hello hatékonyságát még mindig kulcsfontosságú tényező az általános méretezhetőséget. 

SQL-adatbázis használatának előnyei hello egyike, hogy nincs toomanage hello kiszolgálók állomás hello adatbázis. Azonban a felügyelt infrastruktúra is azt jelenti, hogy másképp kapcsolatos adatbázis optimalizálás toothink. Keresse meg a tooimprove hello adatbázis hardver- vagy hálózati infrastruktúra már nem. A Microsoft Azure határozza meg azokat a környezetben. hello fő területet, amely befolyásolhatja az SQL-adatbázis és az alkalmazás együttműködését. Kötegelés egyike ezek az optimalizálások. 

hello papír első része hello SQL-adatbázis használata a .NET-alkalmazások különböző kötegelési technikák megvizsgálja. hello utolsó két szakaszok fedik le a kötegelési irányelvek és forgatókönyvek.

## <a name="batching-strategies"></a>Kötegelési stratégiák
### <a name="note-about-timing-results-in-this-topic"></a>Megjegyzés: Ebben a témakörben időzítési eredmény
> [!NOTE]
> Eredmények nem referenciaalapokhoz képest, de célja tooshow **relatív teljesítménye**. Időzítés legalább 10 teszt futtatása átlagosan alapulnak. Műveletek esetében a beszúrások, a program üres táblát. Ezek a tesztek mért előtti-12-es verzióra, és ezek nem feltétlenül toothroughput 12-es verziójú adatbázis hello új segítségével esetleg előforduló [szolgáltatásszintek](sql-database-service-tiers.md). hello relatív előnye, hogy a technikával kötegelés hello hasonlónak kell lenniük.
> 
> 

### <a name="transactions"></a>Tranzakciók
Úgy tűnik, hogy a rendellenes toobegin megvitatása tranzakciók által kötegelés áttekintése. De a tranzakciók ügyféloldali hello használata finom kiszolgálóoldali kötegelési hatással van, amely javítja a teljesítményt. És a tranzakciók hozzáadása is lehetséges csak néhány sornyi kódot, így ezek biztosítanak egy gyorsan tooimprove teljesítmény egymást követő műveletek.

Vegye figyelembe a következő C#-kódban insert sorozatát tartalmazó hello és frissítési műveletek egyszerű táblán.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

az ADO.NET kód egymás után következő hello ezeket a műveleteket hajt végre.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

hello legjobb módja toooptimize ezt a kódot tooimplement van valamilyen ügyféloldali kötegelése hívásokat. Azonban ez a kód egy egyszerű módon tooincrease hello teljesítményének használatával egyszerűen hívások hello sorozatát szerepel egy tranzakcióban. Itt hello ugyanazt a kódot, amely egy tranzakció használja.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();

        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }

        transaction.Commit();
    }

Tranzakciók mindkét ezekben a példákban ténylegesen használatban van. Hello első példában minden egyes tekintendő, amely az implicit tranzakciókban. Hello második példában az explicit tranzakciók becsomagolja összes hello hívások. Hello hello dokumentációja / [írási előre tranzakciónapló](https://msdn.microsoft.com/library/ms186259.aspx), naplórekordokat esetén kiürített toohello lemez hello tranzakció véglegesítése. Így több hívást együtt egy tranzakcióban, hello írási toohello tranzakciónapló tudja elhalasztani, amíg hello tranzakció. Érvényben engedélyezi a kötegelés hello írások toohello server tranzakciós napló.

a következő táblázat hello néhány alkalmi vizsgálati eredményeket jeleníti meg. hello tesztek kerülnek végrehajtásra hello azonos szekvenciális beszúrása rendelkező és anélküli tranzakciók. További perspektívát, hello első teszteket hajtson futott távolról a Microsoft Azure-ban egy hordozható toohello adatbázisból. hello második együttesét a tesztek futtatása egy felhőalapú szolgáltatás, hogy mindkét tartózkodott hello belül azonos adatbázist a Microsoft Azure datacenter (USA nyugati régiója). hello következő táblázatban hello időtartam ezredmásodpercben a szekvenciális Beszúrások rendelkező és anélküli tranzakciók.

**A helyszíni tooAzure**:

| Műveletek | Nincs tranzakció (ms) | Tranzakció (ms) |
| --- | --- | --- |
| 1 |130 |402 |
| 10 |1208 |1226 |
| 100 |12662 |10395 |
| 1000 |128852 |102917 |

**Az Azure tooAzure (ugyanabban az adatközpontban)**:

| Műveletek | Nincs tranzakció (ms) | Tranzakció (ms) |
| --- | --- | --- |
| 1 |21 |26 |
| 10 |220 |56 |
| 100 |2145 |341 |
| 1000 |21479 |2756 |

> [!NOTE]
> A eredményei nem referenciaalapokhoz képest. Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).
> 
> 

Hello előző teszt eredményei alapján teljesítmény ténylegesen csökkenti alkalmazásburkoló egyetlen műveletben szerepel egy tranzakcióban. De növelésével műveletek egy tranzakción belül hello száma, hello teljesítményjavulást több lesz megjelölve. hello teljesítménybeli különbség az akkor is jobban észlelhető, ha minden műveletnél hello Microsoft Azure adatközponton belül történik. hello SQL-adatbázis használata a Microsoft Azure datacenter külső hello nagyobb késéseket overshadows hello jobb teljesítménye tranzakciók használatával.

Noha a tranzakciók hello használata növelheti a teljesítményt, továbbra is túl[tekintse át az ajánlott eljárások az tranzakciók és kapcsolatok](https://msdn.microsoft.com/library/ms187484.aspx). Tartsa hello tranzakció és a lehető legrövidebb lehetséges és Bezárás hello adatbázis-kapcsolat hello munkahelyi befejeződése után. hello utasítás használatával az előző példában hello biztosítja, hogy az hello kapcsolat le van zárva hello későbbi kódblokk befejezéséről.

hello előző példa bemutatja, hogy adhat hozzá egy helyi tranzakció tooany ADO.NET kódot két sort. Tranzakciók tooimprove hello teljesítményének kódot, amely lehetővé teszi a szekvenciális beszúrási, frissítési és törlési műveletek gyors lehetőséget kínál. Azonban a hello leggyorsabb teljesítmény érdekében érdemes megfontolni hello kód további tootake előnye, hogy az ügyféloldali kötegelés, például a táblázat értékű paramétereket.

Az ADO.NET tranzakciókkal kapcsolatos további információkért lásd: [ADO.NET helyi tranzakciók](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).

### <a name="table-valued-parameters"></a>tábla értékű paraméter
Tábla értékű paraméter paraméterekkel a Transact-SQL-utasítások, tárolt eljárások és függvények, felhasználó által definiált táblatípusokban támogatja. Ez az ügyféloldali kötegelési módszer lehetővé teszi, hogy toosend több sornyi adatot hello tábla értékű paraméter belül. toouse tábla értékű paraméter, először egy táblatípus határozza meg. a következő Transact-SQL-utasítás hello nevű tábla típus létrehozása **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


A kódban, hozzon létre egy **DataTable** hello a pontos azonos nevét és típusát hello táblatípus. Ez átadni **DataTable** egy paraméterben, szöveges lekérdezés vagy tárolt eljárás hívása. hello következő példa bemutatja, ezzel a módszerrel:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. hello following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }

        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);

        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });

        cmd.ExecuteNonQuery();
    }

Hello előző példában hello **SqlCommand** objektum egy tábla értékű paraméter a sor beszúrása  **@TestTvp** . korábban létrehozott hello **DataTable** objektum hozzá van rendelve a hello toothis paraméterrel **SqlCommand.Parameters.Add** metódust. Kötegelési hello beszúrása egy hívás jelentősen növeli hello teljesítmény szekvenciális Beszúrások keresztül.

tooimprove hello előző példa folytatásaként, használja a tárolt eljárás egy szöveges parancs helyett. a következő Transact-SQL-parancs hello hoz létre, amely hello tárolt eljárás **SimpleTestTableType** tábla értékű paraméter.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Módosítsa a hello **SqlCommand** hello előző kód példa toohello következő deklaráció objektum.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

A legtöbb esetben a tábla értékű paraméter rendelkezik egyenértékű vagy jobb teljesítményt biztosít, mint más kötegelési módszerek. Tábla értékű paraméterek gyakran érdemes, mivel rugalmasabb, mint más beállítások. Egyéb módszerek, például SQL tömeges másolás, például csak új sorok beszúrását hello lehetővé teszik. De tábla értékű paraméter segítségével is logika hello tárolt eljárás toodetermine mely sorai frissítések érhetők el, és amelyek beszúrása. hello táblatípus is módosított toocontain egy "Művelet" oszlopot, amely azt jelzi, hogy hello megadott sort kell beszúrni, frissíteni, vagy törölve.

a következő táblázat hello alkalmi teszteredmények hello használható a tábla értékű paraméterek mutatja ezredmásodpercben.

| Műveletek | A helyszíni tooAzure (ms) | Az Azure ugyanabban az adatközpontban (ms) |
| --- | --- | --- |
| 1 |124 |32 |
| 10 |131 |25 |
| 100 |338 |51 |
| 1000 |2615 |382 |
| 10000 |23830 |3586 |

> [!NOTE]
> A eredményei nem referenciaalapokhoz képest. Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).
> 
> 

hello jobb teljesítménye a kötegelés azonnal kétségtelenül. Hello előző szekvenciális teszt 1000 műveletek másodpercet vett igénybe 129 külső hello datacenter és hello adatközponton belül a 21 másodperc. De a tábla értékű paraméter 1000-műveletek egy csak 2.6-os másodperc hello adatközponton kívülről és idõtartamtól hello adatközponton belül.

További információ a tábla értékű paraméter: [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>SQL tömeges másolási
SQL tömeges másolási egy másik módja tooinsert nagy mennyiségű adat egy cél adatbázisba. .NET-alkalmazásokban használható hello **SqlBulkCopy** osztály tooperform tömeges beszúrási műveletek. **SqlBulkCopy** hasonló függvény toohello parancssori eszköz, a **Bcp.exe**, vagy a Transact-SQL-utasítás hello **TÖMEGES Beszúrás**. hello következő kódrészlet példa bemutatja, hogyan toobulk másolási hello hello forrás sort **DataTable**, tábla, az SQL Server, MyTable toohello céltábla.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Néhány esetben, ha tömeges másolás előnyben részesített tábla értékű paraméter felett van. Tekintse meg a tábla értékű paramétert, és TÖMEGES beszúrási műveletek hello témakör hello összehasonlító táblázatában [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb510489.aspx).

hello következő alkalmi vizsgálati eredmények megjelenítése hello teljesítményét a kötegelés **SqlBulkCopy** ezredmásodpercben.

| Műveletek | A helyszíni tooAzure (ms) | Az Azure ugyanabban az adatközpontban (ms) |
| --- | --- | --- |
| 1 |433 |57 |
| 10 |441 |32 |
| 100 |636 |53 |
| 1000 |2535 |341 |
| 10000 |21605 |2737 |

> [!NOTE]
> A eredményei nem referenciaalapokhoz képest. Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).
> 
> 

A Köteg mérete kisebb, hello használata tábla értékű paraméter outperformed hello **SqlBulkCopy** osztály. Azonban **SqlBulkCopy** az 1000 és 10 000 sorok hello tesztek során végrehajtott 12-31 % gyorsabb, mint a tábla értékű paraméter. Tábla értékű paraméterek, például **SqlBulkCopy** van a kötegelt Beszúrás jó választás, különösen akkor, ha képest műveletek nem kötegelni toohello teljesítményét.

A tömeges másolás az ADO.NET további információkért lásd: [az SQL Server tömeges másolási műveletek](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Több soron kívüli Beszúrás paraméteres utasításokat
Egy kis kötegek esetben egy nagy tooconstruct paraméteres INSERT utasításban, amely több sor beszúrása. hello a következő példakód azt mutatja be, ezzel a módszerrel.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";

        SqlCommand cmd = new SqlCommand(insertCommand, connection);

        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }

        cmd.ExecuteNonQuery();
    }


Ez a példa arra szolgál, tooshow hello alapvető fogalma. A modell forgatókönyv volna ismétlése szükséges hello entitások tooconstruct hello lekérdezési karakterlánc és hello parancs paraméterei egyidejűleg. Legfeljebb 2100 lekérdezési paraméterek, ez korlátozza hello sorok maximális számát, amelyek az ilyen módon dolgozhatók tooa összesen.

a következő alkalmi eredmények megjelenítése hello teljesítményének tesztelése az ilyen típusú insert utasítás ezredmásodpercben hello.

| Műveletek | Tábla értékű paraméter (ms) | Utasításból INSERT (ms) |
| --- | --- | --- |
| 1 |32 |20 |
| 10 |30 |25 |
| 100 |33 |51 |

> [!NOTE]
> A eredményei nem referenciaalapokhoz képest. Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).
> 
> 

Ezt a módszert használja, amelyek 100-nál kevesebb sort kötegek némileg gyorsabb lehet. Bár hello javítása kis, ez a módszer akkor lehet, hogy kiválóan működjenek az adott alkalmazás helyzetnek lehetősége.

### <a name="dataadapter"></a>DataAdapter
Hello **DataAdapter** osztály toomodify lehetővé teszi egy **DataSet** objektumot, és küldje el a INSERT, UPDATE és DELETE műveletek hello változik. Hello használata **DataAdapter** ezen a módon kikapcsolja, fontos, hívások elválasztó toonote legyenek készülve az egyes különálló műveletet. tooimprove teljesítmény érdekében használjon hello **UpdateBatchSize** egyszerre kell lehet kötegelni műveletek tulajdonság toohello száma. További információkért lásd: [végrehajtása kötegelt műveletek használatával DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Entitás-keretrendszer
Entitás-keretrendszer jelenleg nem támogatja kötegelés. Hello Közösségből származó különböző fejlesztők próbált toodemonstrate lehetséges megoldások, például a felülbírálás hello **a SaveChanges metódus** metódust. De hello megoldások általában összetett és testre szabott toohello alkalmazás és az adatokat az adatmodellbe. hello Entity Framework codeplex-projekt jelenleg is rendelkezik az ismertető a szolgáltatás kérésre. tooview ismertető, lásd: [tervezési értekezlet megjegyzések - 2012 augusztus 2](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
A teljesség kedvéért azt látja, hogy az XML kötegelési stratégiát, fontos tootalk. Az XML-kód hello használata azonban más módszerekkel nem előnyöket és számos hátránya rendelkezik. hello megközelítés hasonló tootable értékű paraméterek, de egy XML-fájl vagy karakterlánc átadása tooa tárolt eljárás nem felhasználói tábla. hello tárolt eljárás elemez hello parancsok hello tárolt eljárás.

Van több hátrányai toothis módszert:

* Az XML működő nehézkes lehet, és hibalehetőségeket rejt magában hiba.
* Elemzési hello XML hello adatbázison processzorigényes lehet.
* A legtöbb esetben ez a módszer lassabb, mint a tábla értékű paraméter.

Ezen okok miatt hello XML kötegelt lekérdezések használata nem ajánlott.

## <a name="batching-considerations"></a>Kötegelési kapcsolatos szempontok
a következő szakaszok hello további útmutatás nyújtása a hello használata az SQL-adatbázis alkalmazások kötegelés.

### <a name="tradeoffs"></a>Mellékhatásokkal
Attól függően, hogy az architektúrák kötegelés magába foglaló a teljesítményt és rugalmasságot közötti kompromisszumot. Vegye figyelembe például hello forgatókönyv, ahol a szerepkör váratlanul leáll. Ha elveszíti egy adatsornak, hello szempontjából kisebb, mint egy nagy sorköteg el nem küldött elvesztése hello hatását. Nagyobb veszélynek van, amikor sorok elegendő pufferrel, mielőtt elküldi őket a megadott időkeretnél toohello adatbázis.

Miatt ez kompromisszumot kiértékelheti, hogy Ön kötegelt hello típusú műveletek. A Batch-agresszívabb (nagyobb kötegek és hosszabb idő windows) kevésbé fontos adatokkal.

### <a name="batch-size"></a>Köteg mérete
A tesztelés során történt általában nem szolgál előnyökkel toobreaking nagy kötegek kisebb adattömbökbe. Gyakran ez felosztása, mint egy egyetlen nagy kötegelt lassabban eredményezett. Példaként vegyünk egy forgatókönyvet, ahol azt szeretné, hogy tooinsert 1000 sor. hello következő táblázatban mennyi idő alatt toouse tábla értékű paraméter tooinsert 1000 sorok, amikor kisebb kötegekben osztva.

| Köteg mérete | Az ismétlés | Tábla értékű paraméter (ms) |
| --- | --- | --- |
| 1000 |1 |347 |
| 500 |2 |355 |
| 100 |10 |465 |
| 50 |20 |630 |

> [!NOTE]
> A eredményei nem referenciaalapokhoz képest. Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).
> 
> 

Láthatja, hogy a legjobb teljesítményt hello 1000 sor toosubmit egyszerre őket. Más tesztekben (itt nem látható) történt egy kis teljesítmény nyereség toobreak be két kötegek 5000 10000 sor kötegben. Azonban ezekben a tesztekben hello táblaséma viszonylag egyszerű, így végre kell hajtania teszteli, az adatokat és a Köteg mérete tooverify ezen eredmények.

Egy másik tényező tooconsider, hogy hello teljes kötegelt túl nagyra nő, ha SQL-adatbázis előfordulhat, hogy sávszélesség-szabályozási és toocommit hello kötegelt elutasítja. Ha az épp ezért tökéletes választás a köteg méretének vizsgálat az adott forgatókönyv toodetermine hello legjobb eredmények elérése érdekében. Hello köteg mérete konfigurálható tegye a futásidejű tooenable gyors beállításai alapján a teljesítmény- vagy hibákat.

Végezetül egyenleg hello köteg mérete hello hello kockázatot jelentő társított kötegelés. Ha átmeneti hiba merül fel, vagy hello szerepkör meghiúsul, fontolja meg a hello következmények hello művelet vagy hello adatvesztés hello kötegben.

### <a name="parallel-processing"></a>Párhuzamos feldolgozás
Mi történik, ha tartott hello megközelítés hello a köteg méretének csökkentését, de több szál tooexecute hello munkahelyi használt? Ebben az esetben a tesztek bemutatta, hogy több kisebb többszálas kötegek általában végre a nagyobb kötegek rosszabb. hello következő teszt megpróbál egy vagy több párhuzamos kötegekben tooinsert 1000 sor. Ez a vizsgálat bemutatja, hogyan több egyidejű kötegek ténylegesen csökkent teljesítményt.

| A köteg méretének [ismétlési] | Két szállal (ms) | Négy szálak (ms) | Hat szálak (ms) |
| --- | --- | --- | --- |
| 1000 [1] |277 |315 |266 |
| 500 [2] |548 |278 |256 |
| 250 [4] |405 |329 |265 |
| 100 [10] |488 |439 |391 |

> [!NOTE]
> A eredményei nem referenciaalapokhoz képest. Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).
> 
> 

Több lehetséges oka lehet hello akár teljesítménycsökkenés esedékes tooparallelism:

* Nincsenek egy helyett több egyidejű hálózati hívást.
* Egyetlen tábla több műveleteket versengés és blokkolja a eredményezhet.
* Nincsenek társított terhek többszálas.
* több kapcsolat megnyitása hello költsége ez fontosabb, mint a hello előnye, hogy a párhuzamos feldolgozást.

Különböző táblákhoz vagy adatbázisok célozhat esetén lehetséges toosee néhány teljesítmény szerezhet az ezt a stratégiát. Adatbázis horizontális vagy összevonási lenne a forgatókönyv ezt a módszert használja. Horizontális több adatbázisok és útvonalak különböző az tooeach-adatbázist használja. Ha egy kis köteg tooa másik adatbázishoz, hatékonyabb lehet majd hello műveletet hajt végre párhuzamosan. Azonban hello jobb teljesítménye nincs elég jelentős toouse egy döntési toouse adatbázis horizontális a megoldásban a hello alapjaként.

Néhány terveibe kisebb kötegekben párhuzamos végrehajtása eredményezhet továbbfejlesztett kapacitásának terhelés alatt a rendszer. Ebben az esetben akkor is, ha a nagyobb kötegek gyorsabb tooprocess, párhuzamosan több kötegenként lehet hatékonyabb.

Ha párhuzamos végrehajtás használja, fontolja meg az ellenőrző hello szálak maximális száma worker. Kevesebb gyorsabb végrehajtási idő, és kevesebb a versengés eredményezheti. Figyelembe venni, a hello egyéb terheléseket, amelyeket ez hello céladatbázis kapcsolatok és a tranzakciók egyaránt helyezi.

### <a name="related-performance-factors"></a>A teljesítmény kapcsolódó tényezők
Adatbázis teljesítménye jellemző útmutatást is kötegelés hatással van. Például be nagy elsődleges kulcs, vagy sok fürtözetlen indexeire rendelkező táblák csökken a teljesítmény.

Ha a tábla értékű paraméter tárolt eljárás használatához paranccsal hello **SET NOCOUNT ON** hello eljárás hello elején. A jelen nyilatkozat mellőzi hello visszatérési hello eljárás hello érintett sorok hello száma. Azonban a tesztelés során használatát hello **SET NOCOUNT ON** kellett nincs hatása, vagy teljesítménye csökkent. hello vizsgálati tárolt eljárás egyszerű volt egyetlen **BESZÚRÁSA** hello tábla értékű paraméter parancsot. Akkor lehet, hogy összetettebb tárolt eljárások a jelen nyilatkozat előnyös. Nem érdemes feltételezni, hogy hozzáadása, de **SET NOCOUNT ON** tooyour tárolt eljárás automatikusan javítja a teljesítményt. toounderstand hello hatása, tesztelje a tárolt eljárás használatával és anélkül hello **SET NOCOUNT ON** utasítást.

## <a name="batching-scenarios"></a>Kötegelése forgatókönyvek
hello alábbi szakaszok azt ismertetik, hogyan toouse táblázat értékű paramétereket a három alkalmazás-forgatókönyveket. hello első forgatókönyv bemutatja, hogyan pufferelés és kötegelés hogyan tudnak együttműködni. második forgatókönyv hello javítja a teljesítményt, egyetlen tárolt eljárás hívása a fő-részletek műveletet hajt végre. végső forgatókönyvet mutat be hello hogyan toouse tábla értékű paraméter "UPSERT" műveletben.

### <a name="buffering"></a>Pufferelés
Habár van néhány olyan forgatókönyvet, nyilvánvaló jelölt kötegelés, ott sikerült előnyeit által késleltetett feldolgozási kötegelés több forgatókönyv áll. Azonban késleltetett feldolgozása is hordoz magában, ha nagyobb kockázata, hogy egy nem várt hiba hello esemény hello adatok elvesznek. Fontos toounderstand ennek a veszélyét, és vegye figyelembe a hello következményeiről.

Tegyük fel, amely nyomon követi a minden felhasználó hello előzménylistáján webalkalmazás. Minden lap kérésre hello alkalmazás sikerült egy adatbázis hívás toorecord hello felhasználói lap nézetben. De magasabb teljesítmény és méretezhetőség pufferelés hello felhasználók navigációs tevékenységeket, és elküldi az adatokat toohello adatbázis kötegekben úgy lehet elérni. Hello adatbázis módosítása során eltelt idő és/vagy puffer mérete által aktiválhatók. Egy szabály például megadhatja, hogy hello kötegelt fel kell dolgozni, 20 másodperc, vagy amikor hello puffer eléri-e 1000 elem után.

hello alábbi példakód [reaktív bővítmények – a Rx](https://msdn.microsoft.com/data/gg577609) tooprocess pufferelt figyelési osztály által kiváltott esemény. Amikor a puffer kitöltés hello vagy időtúllépés, a felhasználói adatok hello kötegelt küldött toohello adatbázis egy tábla értékű paraméter.

hello következő NavHistoryData osztály modellek hello felhasználói navigációs adatok. Például a felhasználói azonosító hello alapszintű információkat tartalmaz, hello URL-cím elérhető, hello hozzáférés időpontja.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

hello NavHistoryDataMonitor osztály hello felhasználói navigációs adatok toohello adatbázis pufferelés felelős. Tartalmaz egy metódust, RecordUserNavigationEntry, amely válaszol-e megjelenítve jelzi egy **OnAdded** esemény. hello következő kód bemutatja hello konstruktor logika, amely használja a Rx toocreate hello események alapján egy megfigyelhető gyűjteményt. Majd feliratkozva toothis megfigyelhető gyűjtemény hello puffer metódussal. hello túlterhelési határozza meg, hogy hello puffer küldjön 20 másodpercenként vagy 1000 bejegyzéseket.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

hello kezelő pufferelt hello elemek mindegyikét alakítja át a tábla értékű típus, és ezután továbbítja a típus tooa tárolt eljárás, hogy folyamatok hello kötegelt. hello következő kód bemutatja hello hello NavHistoryDataEventArgs és a hello NavHistoryDataMonitor osztályok teljes definíciója.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }

    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;

        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }

        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }

        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }

            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();

                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;

                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });

                cmd.ExecuteNonQuery();
            }
        }
    }

toouse az pufferelési osztály hello alkalmazás egy statikus NavHistoryDataMonitor objektumot hoz létre. Minden alkalommal, amikor egy felhasználó hozzáfér egy lap hello alkalmazás meghívja hello NavHistoryDataMonitor.RecordUserNavigationEntry metódust. pufferelés logika hello kiszolgálásához küld e bejegyzések toohello adatbázis kötegekben tootake folytatódik.

### <a name="master-detail"></a>Fő részletei
Tábla értékű paraméter egyszerű INSERT forgatókönyvek hasznosak. Lehet azonban nehezebb toobatch Beszúrások, például több mint egy olyan táblát. hello "kapcsolatú" például az is jó példa. hello fő tábla elsődleges entitás hello azonosítja. Egy vagy több részletek tábla kapcsolatos hello entitás több adatot tároljon. Ebben a forgatókönyvben a külső kulcsok kapcsolatai kényszerítése hello kapcsolat részletek tooa egyedi fő entitás. Vegye figyelembe a PurchaseOrder és a kapcsolódó OrderDetail tábla egyszerűsített verziója. a következő Transact-SQL hello hello PurchaseOrder tábla négy oszlopot hoz létre: OrderID, orderdate oszlopra, CustomerID és állapotát.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Minden egyes rendelés egy vagy több termék vásárlás tartalmazza. Ezek az információk rögzítése hello PurchaseOrderDetail táblában. a következő Transact-SQL hello hello PurchaseOrderDetail táblázatot hoz öt oszlopokkal: OrderID, OrderDetailID, ProductID, Egységár és OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

hello OrderID oszlop hello PurchaseOrderDetail tábla sorrendben kell hivatkoznia, hello PurchaseOrder táblából. a következő idegen kulcs definícióját hello kikényszeríti ennél a határértéknél.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Rendelés toouse tábla értékű paraméterek rendelkeznie kell egy felhasználó által definiált táblatípus minden céloldali táblához.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO

    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Majd adja meg, amely támogatja a következő típusú táblák tárolt eljárást. Ez az eljárás lehetővé teszi, hogy egy alkalmazás toolocally kötegelt rendeléseket és a rendelés részleteit egyetlen hívásban. hello következő Transact-SQL biztosít hello teljes tárolt eljárás deklaráció a következő példában szereplő beszerzési rendelés.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects hello order identifiers in hello @orders
    -- table with hello actual order identifiers in hello PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders toohello PurchaseOrder table, storing hello actual
    -- order identifiers in hello @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match hello passed-in order identifiers with hello actual identifiers
    -- and complete hello @IdentityLink table for use with inserting hello details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert hello order details into hello PurchaseOrderDetail table, 
          -- using hello actual order identifiers of hello master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

Ebben a példában a helyileg definiált hello @IdentityLink a táblázat sorainak újonnan beszúrt hello hello tényleges OrderID értéke. Ilyen rendelés azonosítókat hello hello ideiglenes OrderID értékek eltérnek @orders és @details tábla értékű paraméter. Emiatt hello @IdentityLink tábla majd összekapcsolja hello OrderID értékek hello @orders toohello valós OrderID paraméterértékek hello PurchaseOrder tábla hello új soraihoz. Ez a lépés után hello @IdentityLink tábla megkönnyítheti beszúrni hello rendelés részleteit a hello tényleges OrderID, amely eleget tesz a hello idegenkulcs-megkötés.

Ez a tárolt eljárás használható kód vagy más Transact-SQL-hívások. A dokumentum a kód például a hello tábla értékű paraméter című szakaszában talál. a következő Transact-SQL hello bemutatja, hogyan toocall hello sp_InsertOrdersBatch.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType

    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')

    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)

    exec sp_InsertOrdersBatch @orders, @details

Ez a megoldás lehetővé teszi, hogy minden kötegelt toouse OrderID értékek: 1 kezdődő. Ideiglenes OrderID értékeiről leírják hello kapcsolatok hello kötegben, de hello tényleges OrderID értékek határozzák meg a hello beszúrási művelet hello időpontjában. A befejezésre hello azonos utasítások hello előző példában szereplő, és egyedi rendeléseket hello adatbázis lehet létrehozni. Emiatt érdemes további kóddal vagy az adatbázis logika, amely megakadályozza a duplikált rendelések használatakor ezzel a technikával kötegelés.

Ez a példa mutatja be, hogy még inkább összetett adatbázis-műveletek, például a részletezés master operations, is lehet kötegelni tábla értékű paraméterek használatával.

### <a name="upsert"></a>UPSERT
Egy másik kötegelési forgatókönyv magában foglalja a egyidejűleg frissíteni a létező sorok és új sort beszúrni. Ez a művelet néha hivatkozott tooas "UPSERT" (frissítés + insert) művelet esetében. Ahelyett, hogy külön hívások tooINSERT és a frissítés elvégzése, hello MERGE utasítás a legjobb olyan környezethez a legalkalmasabb toothis feladat. hello MERGE utasítás végrehajtása mindkét insert és frissítési művelet egyetlen hívással.

Tábla értékű paraméterek hello MERGE utasítás tooperform frissítéseket és a Beszúrás használhatók. Vegyük példaként a következő oszlopok hello tartalmazó egyszerűsített alkalmazott táblázat: EmployeeID, az Utónév, a Vezetéknév, a SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

Ebben a példában a hello arra, hogy hello SocialSecurityNumber egyedi tooperform több alkalmazott EGYESÍTÉSÉVEL is használhatja. Először hozzon létre hello felhasználó által definiált táblatípus:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

A következő tárolt eljárás létrehozása, vagy írhat kódot, hogy használ hello MERGE utasítás tooperform hello frissítést, majd szúrja be. hello alábbi példában hello MERGE utasítás egy tábla értékű paraméter @employees, EmployeeTableType típusú. hello tartalmát hello @employees tábla itt nem látható.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

További információkért lásd: hello dokumentáció és példák hello MERGE utasításban. Ugyanazzal a munkahelyi elvégezhető egy több lépésben hello tárolt eljárás hívása külön INSERT vagy UPDATE műveletekkel, hello MERGE utasítás használata sokkal hatékonyabb. Adatbázis kód hogyan hozhat létre közvetlenül nélkül két adatbázis hívások az INSERT vagy UPDATE hello MERGE utasítás használó Transact-SQL-hívások is.

## <a name="recommendation-summary"></a>A javaslat összefoglaló
hello alábbi lista tartalmaz összefoglaló információkat a jelen témakörben bemutatott javaslatok kötegelés hello:

* Pufferelés és kötegelés tooincrease hello teljesítményét és méretezhetőségét SQL adatbázis-alkalmazások használata.
* Ismerje meg, hello mellékhatásokkal kötegelés/pufferelés és a rugalmasság között. Adott szerepkör meghibásodás során egy feldolgozatlan kötegelt az üzleti szempontból kritikus fontosságú adatok elvesztését kockáztatja hello hello teljesítmény előnye, hogy a kötegelés előfordulhat, hogy járó.
* Kísérlet történt tookeep összes hívások toohello adatbázis egyetlen datacenter tooreduce késleltetése belül.
* Ha úgy dönt, hogy egyetlen kötegelési technika, tábla értékű paraméter kínálnak hello legjobb teljesítményt és rugalmasságot biztosít.
* Hello leggyorsabb beszúrása teljesítmény, kövesse az alábbi általános irányelveket, de a forgatókönyv teszteléséhez:
  * < 100 sorai egy paraméteres INSERT parancs használata.
  * A < 1000 sor használja a táblázat értékű paramétereket.
  * A > = 1000 sorok, SqlBulkCopy használja.
* A és törlési műveletek frissítéséhez használja tábla értékű paraméter tárolt eljárás logikával határozza meg, hogy a helyes műveletet hello minden egyes sorára hello tábla paraméter.
* Köteg mérete irányelveket:
  * Hello legnagyobb kötegelt méretek számára az alkalmazás- és üzleti követelmények célszerű használni.
  * Nagy kötegek egyenleg hello teljesítmény hello kockázatot jelentő ideiglenes vagy végzetes hibák ettől kezdve. Mi az az újrapróbálkozások hello következménye és hello hello kötegben adatvesztés? 
  * Hello legnagyobb köteg mérete tooverify, hogy SQL-adatbázis nem elutasítania tesztelése.
  * A vezérlő kötegelés, például hello kötegméret vagy hello pufferelési időkerete konfigurációs beállítások létrehozása. Ezek a beállítások rugalmasságot biztosít. Hello viselkedésnek a termelési kötegelés hello felhőalapú szolgáltatás ismételt üzembe helyezésével módosíthatja.
* Kerülje a párhuzamos végrehajtás kötegek, amely több adatbázis egyetlen táblájára működik. Ha toodivide a kötegek között több munkavégző szál, futtassa a tesztek toodetermine hello ideális szálak száma. Után egy nem meghatározott küszöbértéket több szál fog miatta a teljesítmény, nem pedig növeli azt.
* Vegye figyelembe a pufferelés méret és így további forgatókönyvek kötegelés végrehajtási idő.

## <a name="next-steps"></a>Következő lépések
Ez a cikk arra irányul, hogy hogyan adatbázis tervezési és kódolási technikák kapcsolódó toobatching javíthatja az alkalmazás teljesítményét és méretezhetőségét. Ez azonban csak egy tényező az általános stratégiában. További módszereket tooimprove teljesítményét és méretezhetőségét, lásd: [Azure SQL Database teljesítményét útmutatást az önálló adatbázisok](sql-database-performance-guidance.md) és [rugalmas készletek ára és teljesítménye szempontok](sql-database-elastic-pool-guidance.md).

