---
title: "aaaUsing elastic database ügyféloldali kódtár az Entity Framework |} Microsoft Docs"
description: "Elastic Database ügyféloldali kódtár és Entity Framework használja az adatbázisok kódolása"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: b9c3065b-cb92-41be-aa7f-deba23e7e159
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: torsteng
ms.openlocfilehash: 917f6d28d9855c0b42afe2c008613a9bbb3ec6b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-client-library-with-entity-framework"></a>Az Entity Framework rugalmas adatbázis ügyféloldali kódtár
Ez a dokumentum hello módosítások mutatja az Entity Framework alkalmazásban, amelyek a hello szükséges toointegrate [skálázáshoz rugalmas adatbáziseszközöket](sql-database-elastic-scale-introduction.md). hello elsősorban összeállítása [shard térkép felügyeleti](sql-database-elastic-scale-shard-map-management.md) és [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md) az Entity Framework hello **Code First** megközelítést. Hello [Code először - új adatbázis](http://msdn.microsoft.com/data/jj193542.aspx) EF útmutató futó példánkban ez a dokumentum funkcionál. Ez a dokumentum kísérő hello mintakód minták a Visual Studio-Kódminták hello beállítása rugalmas adatbázis eszközök részét képezi.

## <a name="downloading-and-running-hello-sample-code"></a>Töltsön le és futtasson hello mintakód
Ez a cikk toodownload hello kódját:

* A Visual Studio 2012 vagy újabb rendszer szükséges. 
* Töltse le a hello [rugalmas adatbázis-eszközök az Azure SQL - entitás Keretrendszeri integrációját minta](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-bae904ba) msdn. Bontsa ki a hello minta tooa helye a.
* Indítsa el a Visual Studiót. 
* A Visual Studio alkalmazásban válassza ki a fájl -> Nyissa meg a projekt/megoldás. 
* A hello **projekt megnyitása** párbeszédpanelen keresse meg a letöltött toohello mintát, és válassza ki **EntityFrameworkCodeFirst.sln** tooopen hello minta. 

toorun hello minta toocreate három üres adatbázist, az Azure SQL Database kell:

* A shard térkép Manager-adatbázis
* A shard 1 adatbázis
* A shard 2 adatbázis

Miután létrehozta ezeket az adatbázisokat, töltse ki hello hely tulajdonosainak **Program.cs** az Azure SQL adatbázis-kiszolgáló neve, hello adatbázis nevét és a hitelesítő adatok tooconnect toohello adatbázisok. A Visual Studio hello megoldás felépítéséhez. A Visual Studio szükséges hello NuGet-csomagok hello elastic database ügyféloldali kódtárának, Entity Framework és átmeneti hiba kezelése hello felépítési folyamat részeként fogja letölteni. Győződjön meg arról, hogy a megoldás NuGet-csomagok visszaállítására engedélyezve. Kattintson a jobb gombbal a hello megoldásfájlt a Visual Studio Solution Explorer hello engedélyezheti ezt a beállítást. 

## <a name="entity-framework-workflows"></a>Entity Framework munkafolyamatai
Entitás-keretrendszer fejlesztői támaszkodjon a következő négy munkafolyamatok toobuild alkalmazások és alkalmazás objektumának tooensure megőrzését hello egyikét: 

* **Első (új adatbázis) code**: hello EF fejlesztői hello alkalmazáskód hello modell hoz létre, és ezután EF generál, hello adatbázis. 
* **Első (meglévő adatbázis) code**: hello fejlesztői lehetővé teszi, hogy az EF hello alkalmazáskód hello modell generálása egy meglévő adatbázist.
* **A modell első**: hello fejlesztői hello EF designer hello modell hoz létre, és ezután EF hoz létre hello adatbázis hello modellből.
* **Adatbázis-első**: hello fejlesztő EF tooling eszköz tooinfer hello modellt egy meglévő adatbázist. 

Ezek a módszerek hello DbContext osztályt tootransparently támaszkodnak az összes adatbázis-kapcsolatok és adatbázisséma egy alkalmazás kezelése. Ismertetik részletesebben hello dokumentum későbbi szakaszában hello DbContext alaposztályban különböző konstruktorok lehetővé teszik a különböző szintű kapcsolat létrehozásakor, ellenőrzésre adatbázis rendszerindítása és séma létrehozásához. Kihívást erednek elsősorban hello tényt, hogy hello adatbázis kapcsolat kezelése EF által biztosított metszi hello kapcsolat kezelésére alkalmas hello adatok függő útválasztási felületek megadott hello elastic database ügyféloldali kódtár által. 

## <a name="elastic-database-tools-assumptions"></a>A rugalmas adatbázis eszközök Előfeltételek
Kifejezés-definíciók, lásd: [rugalmas adatbázis eszközök szószedet](sql-database-elastic-scale-glossary.md).

A rugalmas adatbázis ügyféloldali kódtár az alkalmazásadatok shardlets nevű partíciója határozza meg. Shardlets egy horizontális skálázási kulcs azonosítják, és a csatlakoztatott toospecific adatbázis. Az alkalmazások előfordulhat, hogy annyi adatbázis igény szerint és terjesztése hello shardlets tooprovide elég kapacitás vagy a teljesítmény aktuális üzleti követelmények. horizontális kulcsértékei toohello adatbázisok hello leképezése egy hello rugalmas adatbázis-ügyfél API-k által megadott shard leképezés tárolja. Ez a funkció hívása **Shard térkép felügyeleti**, vagy röviden SMM. hello shard térkép hello broker az adatbázis-kapcsolatok a kéréseket, amelyben egy horizontális skálázási kulcsot is funkcionál. Toothis funkció szerint irányítjuk **adatok függő útválasztási**. 

hello shard térkép kezelő felhasználók védi a inkonzisztens nézetek, amely akkor fordulhat elő, amikor párhuzamos shardlet felügyeleti műveletek (például egy shard tooanother költöztetésére adatait) hogy shardlet adatokká. toodo tehát hello hello ügyfél könyvtár broker hello Helyadatbázis-kapcsolatot az alkalmazás által kezelt shard leképezések. Ez lehetővé teszi, hogy hello shard leképezés funkció tooautomatically kill adatbázis-kapcsolat amikor shard felügyeleti műveletek jelentős hatással lehet a hello shardlet hello kapcsolat számára lett létrehozva. Ez a megközelítés kell toointegrate egyes EF-EK funkciókat, például új kapcsolatok létrehozása egy létező egy toocheck az adatbázis létezését. Általában a megfigyelés le lett, hogy hello szabványos DbContext konstruktorok csak működik megbízhatóan biztonságosan klónozható lezárt adatbázis-kapcsolatok az EF munka. rugalmas kialakítás elve hello helyette tooonly megnyitott broker kapcsolatok. Egy hiszi, hogy egy előtt átadása akkor toohello EF DbContext hello ügyféloldali kódtár által közvetítőalapú kapcsolat bezárása előfordulhat, hogy a probléma megoldására. Azonban hello kapcsolat bezárása, és a függő EF toore-megnyitáskor, egy foregoes hello érvényesítési és konzisztencia-ellenőrzést hello szalagtár végzi. hello áttelepítések funkció EF, azonban használja ezeket az alapul szolgáló adatbázisséma oly módon, hogy transzparens toohello alkalmazás kapcsolatok toomanage hello. Ideális esetben azt kívánja tooretain, majd minden ezeket a képességeket hello elastic database ügyféloldali kódtár és EF a hello egyesítése ugyanahhoz az alkalmazáshoz. hello alábbi szakasz ismerteti ezek a tulajdonságok és részletes követelményeket. 

## <a name="requirements"></a>Követelmények
Hello elastic database ügyféloldali kódtárának és a Entity Framework API-k használata, ha azt szeretnénk, ha tooretain hello következő tulajdonságai: 

* **Kibővített**: hello adatrétegbeli alkalmazási hello szilánkos hello kapacitás iránti igények kielégítése érdekében a megfelelő hello alkalmazás tooadd vagy a remove-adatbázisok. Ez azt jelenti, hogy az ellenőrzése alatt tartja a adatbázisok és hello rugalmas adatbázis shard térkép manager API-k toomanage adatbázisok és shardlets leképezéseit segítségével hello hello létrehozását és törlését. 
* **Konzisztencia**: hello alkalmazás horizontális alkalmazza, és használja az adatok függő útválasztási képességek hello ügyféloldali kódtár hello. tooavoid sérülés vagy hibás lekérdezések eredményeit, kapcsolatok keresztül van-e közvetítőalapú hello shard térkép manager. Ez megőrzi a érvényesítése és a konzisztencia is.
* **Első Code**: az EF-EK kód első paradigma tooretain hello kényelmi célokat szolgál. A kód első osztályok hello alkalmazás átlátható módon vannak leképezve az alapul szolgáló adatbázis felépítését toohello. hello alkalmazáskód, amely a legtöbb területét az alapul szolgáló adatbázis-feldolgozási hello részt maszkolnia DbSets kommunikál.
* **Séma**: Entity Framework kezdeti adatbázis-séma létrehozása és későbbi séma alakulása áttelepítések keresztül kezeli. Megtartja ezeket a képességeket, az alkalmazás testreszabásához az adatok fejlődésének hello legkönnyebben. 

hello következő útmutatást arra utasítja a hogyan toosatisfy rugalmas adatbáziseszközöket Code First alkalmazásokhoz követelménynek. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Az adatok függő útválasztás EF DbContext használata
Adatbázis-kapcsolatok az Entity Framework általában kezelhetők alosztályokat **DbContext**. Hozzon létre a alosztályok származó **DbContext**. Itt adhatja meg azt a **DbSets** amelyek megvalósítják az hello adatbázisról biztonsági objektumok gyűjteményeit adják CLR az alkalmazáshoz. Az adatok függő útválasztási hello a környezetben azt nem feltétlenül vonatkozik az egyéb EF code első alkalmazás forgatókönyvek, amelyek számos hasznos tulajdonság azonosíthatja: 

* hello adatbázis már létezik, és hello rugalmas adatbázis shard leképezés regisztrálva van. 
* hello séma hello alkalmazás már üzembe helyezett toohello adatbázis (alábbi leírásban is). 
* Adatok függő útválasztási kapcsolatok toohello adatbázis által hello shard térkép vannak közvetítőalapú. 

toointegrate **DbContexts** kibővített az útválasztási adatok függő:

1. Hello shard térkép Manager hello rugalmas adatbázis ügyfél felületeken keresztül fizikai adatbázis-kapcsolatok létrehozása 
2. Hello hello kapcsolatot burkolása **DbContext** alosztálya lesz
3. Hello kapcsolat továbbítja hello **DbContext** osztályok tooensure összes hello feldolgozási hello EF oldalon történik is létrehozhatja. 

a következő példakód hello mutatja be ezt a módszert használja. (Ez a kód egyben a Visual Studio-projekt kísérő hello)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed toohello proper shard by hello shard map manager. 
        // Note that hello base class c'tor call will fail for an open connection
        // if migrations need toobe done and SQL credentials are used. This is hello reason for hello 
        // separation of c'tors into hello data-dependent routing case (this c'tor) and hello internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map toobroker a validated connection for hello given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Főbb pontjai
* Egy új konstruktor a felváltja a hello DbContext alosztály hello alapértelmezett konstruktor 
* hello új konstruktor argumentummal hello szükséges adatok függő útválasztási elastic database ügyféloldali kódtárának keresztül:
  
  * hello shard térkép tooaccess hello adatok függő útválasztási felületek,
  * hello horizontális kulcs tooidentify hello shardlet
  * a kapcsolati karakterlánc hello adatok függő útválasztási kapcsolat toohello shard hello hitelesítő adatait. 
* hello hívás toohello alaposztály konstruktor veszi egy detour statikus metódus, amely minden hello lépéseket végzi el az adatok függő útválasztási szükséges. 
  
  * Hello OpenConnectionForKey hívás hello rugalmas adatbázis ügyfél felületek használ a hello shard térkép tooestablish nyitott kapcsolat.
  * hello shard térkép hello nyitott kapcsolat toohello shard, amely tárolja a megadott horizontális skálázási kulcs hello hello shardlet hoz létre.
  * A megnyitott kapcsolat hátsó toohello alaposztály konstruktorának, hogy ez a kapcsolat nem ahelyett, hogy automatikusan hozzon létre egy új kapcsolatot EF EF által használt toobe DbContext tooindicate lett átadva. A módszer hello kapcsolat címkézett hello rugalmas adatbázis ügyfél API-t, hogy garantálják, hogy a shard térkép felügyeleti műveletek konzisztencia.

Új konstruktor hello használata helyett hello alapértelmezett konstruktor a kódban a DbContext alosztálya. Például: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

hello új konstruktor hello kapcsolat toohello shard hello értékének által azonosított hello shardlet hello adatokat tároló megnyitása **tenantid1**. hello kód hello **használatával** blokk marad változatlan tooaccess hello **DbSet** az EF szalaghasználata hello shard a blogok **tenantid1**. Ez módosítja szemantikáját blokkot használ, úgy, hogy az összes művelet most hello hello kód hatókörű toohello egy shard ahol **tenantid1** maradnak. Például a LINQ lekérdezés keresztül hello blogok **DbSet** volna csak térjen vissza a hello aktuális shard tárolt blogok, de nem hello azokat, egyéb szilánkok tárolja.  

#### <a name="transient-faults-handling"></a>Átmeneti hibák kezelése
hello Microsoft Patterns & eljárások csapat közzétett hello [hello átmeneti hiba kezelési alkalmazás blokk](https://msdn.microsoft.com/library/dn440719.aspx). hello könyvtár a rugalmas bővítést ügyféloldali kódtár EF együtt használatos. Azonban ügyeljen arra, hogy a átmeneti kivételen tooa hely, ahol azt is ellenőrizze, hogy hello új konstruktor használja egy átmeneti hiba után, hogy minden új kapcsolódási kísérlet történik, azt kell tweaked hello konstruktorok használatával adja vissza. Ellenkező esetben a kapcsolat helyes szilánkok nem garantált, és hello kapcsolat nélküli biztosítékok nincsenek toohello megmarad, módosítás toohello shard térkép fordulhat elő. 

hello következő mintakód bemutatja, hogyan SQL újrapróbálkozási házirendje használható körül hello új **DbContext** alosztály konstruktorok: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** hello a fenti kódot típusúként van definiálva egy **SqlDatabaseTransientErrorDetectionStrategy** újrapróbálkozás-szám 10-es és 5 másodperc várjon az újrapróbálkozások között eltelt idő. Ez a megközelítés hasonló toohello útmutató EF és a felhasználó által kezdeményezett tranzakció (lásd: [korlátozásai a végrehajtási stratégiák újrapróbálkozás (EF6 és újabb verziók esetében)](http://msdn.microsoft.com/data/dn307226). Mindkét esetben szükséges, hogy hello alkalmazás program vezérli hello hatókör toowhich hello átmeneti kivételt adja vissza: tooeither nyissa meg újra a tranzakciót hello, vagy (ahogy) hozza létre újból, hogy a használt rugalmas adatbázis hello hello megfelelő konstruktor a hello környezetben ügyféloldali kódtárára.

hello kell toocontrol, ahol átmeneti kivételek a hálózatról, biztonsági hatókör is kizárja hello használata hello beépített **SqlAzureExecutionStrategy** az EF elérhető lesz. **SqlAzureExecutionStrategy** volna nyissa meg újra a kapcsolatot, de nem használható **OpenConnectionForKey** , és ezért minden hello érvényesítési hello részeként végzett **OpenConnectionForKey** hívható meg. Ehelyett használja az hello kódminta a hello beépített **DefaultExecutionStrategy** az EF is elérhető lesz. Túl adatkötetekkel**SqlAzureExecutionStrategy**, megfelelően hello újrapróbálkozási házirendje kezeljen átmeneti hiba együtt működik. hello végrehajtási házirend beállítása a hello **ElasticScaleDbConfiguration** osztály. Vegye figyelembe, hogy nem toouse döntöttünk **DefaultSqlExecutionStrategy** óta azt javasolja, hogy toouse **SqlAzureExecutionStrategy** átmeneti kivételek esetén – amely vezetne toowrong viselkedés című szakaszban leírtaknak megfelelően. Hello különböző újrapróbálkozásos házirendek és EF további információkért lásd: [kapcsolati rugalmasságot az EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Konstruktor újraírások
hello fenti hitelesítésikód-példák bemutatják hello alapértelmezett konstruktor átírja rendelés toouse adatok függő entitás-keretrendszer hello útválasztással az alkalmazáshoz szükséges. a következő táblázat hello használatúvá Ez a megközelítés tooother konstruktor. 

| Aktuális konstruktor | Az adatok egy átírt konstruktor | Alap konstruktor | Megjegyzések |
| --- | --- | --- | --- |
| MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |hello kapcsolat toobe hello shard térkép és hello adatok függő útválasztási kulcs szükséges. Tooby-fázis automatikus kapcsolat létrehozásának EF kell, és inkább a hello shard térkép toobroker hello kapcsolat. |
| MyContext(string) |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |hello kapcsolat hello shard térkép és hello adatok függő útválasztási kulcs. A rögzített adatbázis neve vagy a kapcsolati karakterlánc nem fognak működni azok kihagyva érvényesítési hello shard térkép által. |
| MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, logikai) |hello kapcsolat hello shard térkép és horizontális kulcs megadott megadott hello modellt a rendszer létrehozza. hello lefordított modell toohello alap c'tor átadásra kerül. |
| MyContext (DbConnection, bool) |ElasticScaleContext (ShardMap, TKey, logikai) |DbContext (DbConnection, bool) |hello kapcsolat hello shard térkép és hello kulcs következtetni toobe igényel. Azt nem adható meg bemenetként (kivéve, ha a bemenetet hello shard térkép és hello kulcs már használja). logikai érték hello átadásra kerül. |
| MyContext (karakterlánc, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, logikai) |hello kapcsolat hello shard térkép és hello kulcs következtetni toobe igényel. Azt nem adható meg bemenetként (kivéve, ha a bemenetet használt hello shard térkép és hello kulcs). hello lefordított modell átadásra kerül. |
| MyContext (ObjectContext, bool) |ElasticScaleContext (ShardMap, TKey, ObjectContext, bool) |DbContext (ObjectContext, bool) |hello új konstruktort, amely bármilyen kapcsolatot hello ObjectContext átadott bemeneti adatokként a rugalmas bővítést által kezelt átirányítását tooa kapcsolat tooensure kell. Részletes leírását az ObjectContexts nem ez a dokumentum hello terjed. |
| MyContext (DbConnection, DbCompiledModel, logikai) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel, bool) |DbContext (DbConnection, DbCompiledModel, logikai); |hello kapcsolat hello shard térkép és hello kulcs következtetni toobe igényel. hello kapcsolat nem adható meg bemenetként (kivéve, ha a bemenetet hello shard térkép és hello kulcs már használja). Modell és logikai toohello alaposztály konstruktorának átadott. |

## <a name="shard-schema-deployment-through-ef-migrations"></a>A shard séma telepítési keresztül EF-áttelepítések
Automatikus séma-kezelés a hello entitás-keretrendszer által biztosított, a könnyebb elérhetőség érdekében. A rugalmas adatbázis-eszközt használó alkalmazások hello környezetében szeretnénk tooretain ezen képesség tooautomatically rendelkezés hello séma létrehozott toonewly szilánkok adatbázisok hozzáadásakor toohello szilánkos alkalmazás. hello elsődleges használati eset hello adatrétegbeli szilánkos alkalmazásokhoz EF-tooincrease kapacitás. Hello adatbázis felügyeleti elérhető EF-EK képességet biztosít a séma felügyeleti hagyatkoznia csökkenti EF épülő szilánkos alkalmazással. 

Séma telepítési EF áttelepítések keresztül működik a legjobban a **ilyen formában kapcsolatok**. Ez a ezzel szemben toohello forgatókönyvben az adatok függő továbbításához hello rugalmas adatbázis ügyfél API által megadott hello megnyitott kapcsolat támaszkodik. Egy másik különbség hello konzisztencia követelmény: az összes adat-függő útválasztási kapcsolatok tooprotect egyidejű shard térkép manipuláció elleni kívánatos tooensure konzisztenciát, miközben még nincs egy kezdeti séma telepítési tooa új adatbázis kapcsolatos probléma amely még nincs regisztrálva a hello shard térkép, és le lett foglalva a toohold shardlets még nem rendelkezik. Ezért azt is támaszkodnak rendszeres adatbázis-kapcsolatok a forgatókönyvek szerint megakadályozását toodata függő útválasztási.  

Ennek eredménye tooan megközelítés ahol séma telepítési keresztül EF áttelepítések szorosan együtt használja hello regisztrációs hello új adatbázis, a shard hello alkalmazás shard leképezés. Ez a következő előfeltételek hello támaszkodik: 

* hello adatbázis már létezik. 
* hello adatbázis - magánál tartja nincs felhasználói séma és a nem felhasználói adatokat.
* hello adatbázis nem még elérhetőek hello rugalmas adatbázis ügyfél API-k az adatok függő továbbításához. 

Az előfeltételek teljesülnek, a szokványos létrehozhatjuk megbízhatatlan megnyitott **SqlConnection** tookick ki EF áttelepítések séma központi telepítéshez. a következő példakód hello szemlélteti ezt a módszert használja. 

        // Enter a new shard - i.e. an empty database - toohello shard map, allocate a first tenant tooit  
        // and kick off EF intialization of hello database toodeploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext tootrigger migrations and schema deployment for hello new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query tooengage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register hello mapping of hello tenant toohello shard in hello shard map. 
            // After this step, data-dependent routing on hello shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 


Ez a példa bemutatja hello metódus **RegisterNewShard** , hogy a regisztereket hello shard leképezés, shard hello hello séma EF áttelepítések keresztül telepíti, és egy egy horizontális skálázási kulcs toohello szilánkok leképezése tárolja. Hello konstruktor támaszkodnak **DbContext** alosztály (**ElasticScaleContext** hello mintában) bemenetként SQL kapcsolati karakterlánc időt vesz igénybe. Ez a konstruktor hello kódját egyszerű feladat, mint a következő példa azt mutatja meg hello: 

        // C'tor toodeploy schema and migrations tooa new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that hello schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 

Előfordulhat, hogy egy használt öröklődés forrása: hello alaposztály hello konstruktor hello verzióját. De az EF alapértelmezett inicializáló hello hello kódot kell tooensure kapcsolódás esetén használatos. Ezért hello rövid detour hello statikus metódusnak hello alaposztály konstruktornak hello kapcsolati karakterlánccal hívása előtt. Vegye figyelembe, hogy a szilánkok hello regisztrációs az EF hello inicializáló beállítások nem ütköznek egy másik tartomány vagy a folyamat tooensure kell futtatni. 

## <a name="limitations"></a>Korlátozások
Ebben a dokumentumban leírt hello megközelítések néhány korlátozásokkal jár: 

* EF használó alkalmazások **LocalDb** először toomigrate tooa rendszeres SQL Server-adatbázis elastic database ügyféloldali kódtár használata előtt. A rugalmas bővítést ki egy alkalmazást a horizontális skálázás nincs lehetőség a **LocalDb**. Vegye figyelembe, hogy fejlesztési továbbra is használhatja **LocalDb**. 
* Bármely, amelyek azt sugallják adatbázis változás történt változások toohello alkalmazás keresztül az összes szilánkok EF áttelepítések toogo kell. hello mintakód a dokumentum nem bemutatják, hogyan toodo ez. Érdemes lehet az adatbázis-frissítés és a ConnectionString paraméter tooiterate keresztül minden szilánkok; vagy a kivonat hello T-SQL parancsfájl használata a frissítés-adatbázis migrálása függőben hello hello - parancsfájl a beállítást, és hello T-SQL parancsfájl tooyour szilánkok alkalmazni.  
* A kérelemben megadott, feltételezzük, hogy az adatbázis-feldolgozási mindegyikét tartalmazza egyetlen shard hello kérelem által biztosított hello horizontális kulcs által meghatározott. Azonban ez a feltételezés nem mindig tartsa igaz. Például, ha nincs lehetséges toomake egy horizontális skálázási kulcsát. tooaddress a, hello ügyféloldali kódtár biztosít hello **MultiShardQuery** osztály, amely a kapcsolat absztrakciós több szilánkok keresztül lekérdezése. Tanulási toouse hello **MultiShardQuery** EF együtt nem ez a dokumentum hello terjed

## <a name="conclusion"></a>Összegzés
Hello a jelen dokumentumban leírt lépéseket, keresztül EF alkalmazások hello elastic database ügyféloldali kódtár funkció segítségével az adatok függő hello konstruktora újrabontása útválasztást **DbContext** hello EF használt alosztályok az alkalmazás. Ezen korlátok hello módosítások szükséges toothose helyének **DbContext** osztályok már léteznek. Ezenkívül EF alkalmazások továbbra is az automatikus séma telepítésből toobenefit hello szükséges EF áttelepítések hello regisztrálása új szilánkok és hello shard leképezés tartalmazó meghívása hello lépéseket kombinálásával. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
