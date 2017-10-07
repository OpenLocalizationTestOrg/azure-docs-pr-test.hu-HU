---
title: "aaaUsing elastic database ügyféloldali kódtára a Dapper |} Microsoft Docs"
description: "A rugalmas adatbázis ügyféloldali kódtár Dapper használja."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 463d2676-3b19-47c2-83df-f8c50492c9d2
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: c22ece2a977265e93850f0ad3f3ca48f0a8733ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-elastic-database-client-library-with-dapper"></a>A rugalmas adatbázis ügyféloldali kódtár használata Dapper
Ez a dokumentum a fejlesztők számára, amely Dapper toobuild alkalmazások alapulnak, de azt is szeretnék tooembrace van [elastic database tooling](sql-database-elastic-scale-introduction.md) toocreate alkalmazások, amelyek megvalósítják horizontális tooscale kibővített az adatréteg.  Ez a dokumentum Dapper-alapú alkalmazások, amelyek a rugalmas adatbáziseszközöket szükséges toointegrate hello változásait mutatja be. A fókusz van a hello rugalmas adatbázis shard felügyelete és az adatok függő összeállítása útválasztással Dapper. 

**Példakód**: [rugalmas adatbáziseszközöket az Azure SQL Database - Dapper integrációs](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).

Integrálása **Dapper** és **DapperExtensions** hello az Azure SQL Database elastic database ügyféloldali kódtár nem könnyű. Az alkalmazások adatokat függő hello létrehozása módosítása, illetve új megnyitását útválasztást használhatja [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektumok toouse hello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) hello hívja [ügyfél szalagtár](http://msdn.microsoft.com/library/azure/dn765902.aspx). Ez korlátozza az alkalmazás tooonly, ahol új kapcsolatok létrehozása és megnyitott változásairól. 

## <a name="dapper-overview"></a>Dapper áttekintése
**Dapper** egy objektum relációs leképező van. Leképezi a .NET-objektumokat az alkalmazás tooa relációs adatbázisból (és fordítva). hello első része hello mintakód bemutatja, hogyan integrálható hello elastic database ügyféloldali kódtárának Dapper-alapú alkalmazások. második része hello hello mintakód bemutatja, hogyan toointegrate Dapper és a DapperExtensions használatakor.  

hello leképező funkcióit Dapper kiterjesztésmetódusok biztosít, amelyek megkönnyítik a küldő T-SQL-utasítások végrehajtása vagy a lekérdező hello adatbázis adatbázis-kapcsolatok. Például Dapper teszi, hogy a .NET-objektumokat és az SQL-utasításainak hello paraméterei között könnyen toomap **Execute** hívások vagy a .NET objektumokba SQL-lekérdezések eredményeinek tooconsume hello **lekérdezés**Dapper hívásait. 

DapperExtensions használatakor nem kell többé tooprovide hello SQL-utasításokat. Bővítmények módszerek **GetList** vagy **beszúrása** hello adatbázis-kapcsolaton keresztül hello SQL-utasítások létrehozásához hello színfalak mögött.

Egy másik Dapper, és DapperExtensions előnye, hogy hello alkalmazás vezérlők hello hello adatbázis-kapcsolat létrehozását. Ezzel a megoldással hello elastic database ügyféloldali kódtár brókerek adatbázis-kapcsolatok shardlets toodatabases hello leképezése alapján, amely interakciót.

tooget hello Dapper szerelvényeket, lásd: [Dapper pont nettó](http://www.nuget.org/packages/Dapper/). Hello Dapper bővítmények, lásd: [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-hello-elastic-database-client-library"></a>Hello elastic database ügyféloldali kódtárának gyors áttekintése
Hello elastic database ügyféloldali kódtár, a szolgáltatás megadhatja az alkalmazásadatok nevű partíciója *shardlets* oldalváltozókhoz toodatabases és azonosításához által *horizontális kulcsok*. Tetszőleges számú adatbázist kell, és a shardlets szét ezeket az adatbázisokat is. horizontális kulcsértékei toohello adatbázisok hello leképezése hello könyvtár API-k által biztosított shard térkép tárolja. Ez a funkció neve **shard térkép felügyeleti**. hello shard térkép hello broker az adatbázis-kapcsolatok a kéréseket, amelyben egy horizontális skálázási kulcsot is funkcionál. Ez a lehetőség a hivatkozott tooas **adatok függő útválasztási**.

![A shard leképezések és az adatok függő útválasztási][1]

hello shard térkép kezelő felhasználók védi a inkonzisztens nézetek hello adatbázisok hogy egyidejű shardlet felügyeleti műveletek során esetlegesen előforduló shardlet adatokká. toodo tehát hello shard maps broker hello adatbázis-kapcsolatok hello könyvtár-val készült alkalmazás. A shard felügyeleti műveletek jelentős hatással lehet a hello shardlet, lehetővé teszi hello shard leképezés funkció tooautomatically kill egy adatbázis-kapcsolatot. 

Hello hagyományos módon toocreate kapcsolatok helyett a Dapper, igazolnia kell toouse hello [OpenConnectionForKey metódus](http://msdn.microsoft.com/library/azure/dn824099.aspx). Ez biztosítja, hogy minden hello érvényesítési történik, és kapcsolatok felügyelt megfelelően Ha adatokat szilánkok között helyezi.

### <a name="requirements-for-dapper-integration"></a>Dapper integrációs követelményei
Hello elastic database ügyféloldali kódtárának és a hello Dapper API-k használata, ha azt szeretnénk, ha tooretain hello következő tulajdonságai:

* **Scaleout**: azt szeretné, hogy tooadd, vagy távolítsa el adatbázisok hello adatrétegbeli alkalmazási hello szilánkos hello kapacitás iránti igények kielégítése érdekében a megfelelő hello alkalmazás. 
* **Konzisztencia**: mivel az alkalmazás horizontálisan horizontális használatával, igazolnia kell tooperform adatok függő útválasztást. Ezért szeretnénk toouse hello adatok függő útválasztási képességeit hello könyvtár toodo. Különösen azt szeretnénk, ha tooretain hello érvényesítés, és konzisztencia biztosítja, hogy a hello shard térkép manager rendelés tooavoid sérülés vagy hibás lekérdezések eredményeit a rendszer közvetítőalapú kapcsolatok számát biztosítja. Ez biztosítja, hogy kapcsolatok tooa shardlet megadott elutasították vagy leállt, ha (például) hello shardlet jelenleg áthelyezett tooa különböző shard vegyes/egyesítés API-k használatával.
* **Objektum leképezésének**: szeretnénk hello alkalmazásban osztályok és az alapul szolgáló adatbázis felépítését hello közötti Dapper tootranslate által biztosított hello megfeleltetéseket tooretain hello kényelmi célokat szolgál. 

hello következő szakasz útmutatást nyújt a alapuló alkalmazások követelménynek **Dapper** és **DapperExtensions**.

## <a name="technical-guidance"></a>Műszaki útmutató
### <a name="data-dependent-routing-with-dapper"></a>Függő Dapper az útválasztási adatok
Dapper hello alkalmazás általában létrehozásáért és hello kapcsolatok toohello az alapul szolgáló adatbázis megnyitása. T típus megadott hello alkalmazás, Dapper eredményeket ad vissza lekérdezés, T. Dapper típusú .NET gyűjteményekre hello leképezés a hello T-SQL sorok toohello eredményobjektumok T. típusú hajt végre. Dapper hasonlóan SQL értékek vagy adatok adatkezelési nyelv utasítások paramétereinek leképezi a .NET-objektumokat. Dapper nyújt ez a funkció a rendszeres hello kiterjesztésmetódusok keresztül [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) hello ADO .NET SQL klienskódtárak objektumot. hello DDR hello rugalmas, méretezhető API-k által visszaküldött SQL kapcsolódási megtalálhatók rendszeres [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektumok. Ez lehetővé teszi toodirectly használata Dapper bővítmények hello ügyféloldali kódtár DDR API-t által visszaadott hello típus keresztül, mert is egyszerű SQL ügyfél kapcsolat.

E megfigyelések hello elastic database ügyféloldali kódtár által Dapper a(z) közvetítőalapú egyszerű toouse kapcsolatok révén.

Ez a Kódpélda (a minta kísérő hello) mutatja be, ahol hello horizontális kulcs hello alkalmazás toohello könyvtár toobroker hello kapcsolat toohello jobb shard által biztosított hello megközelítés.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

hello hívás toohello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API a felváltja hello alapértelmezett létrehozása és egy ügyfél SQL-kapcsolat megnyitása. Hello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) hívás argumentummal hello szükséges adatok függő útválasztás: 

* hello shard térkép tooaccess hello adatok függő útválasztási felületek
* hello horizontális kulcs tooidentify hello shardlet
* hello hitelesítő adatokat (felhasználónév és jelszó) tooconnect toohello shard

hello shard térkép objektum hoz létre a kapcsolat toohello shard, amely tárolja a megadott horizontális skálázási kulcs hello hello shardlet. hello rugalmas adatbázis ügyfél API-kat is hello kapcsolat tooimplement a konzisztencia biztosítja, hogy címkét. Hello óta hívása túl[OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) adja vissza a normál SQL ügyfélkapcsolati objektumot, hello későbbi hívás toohello **Execute** kiterjesztésmetódus Dapper követi a szabványos Dapper hello ajánlott.

Lekérdezések munkahelyi túlságosan azok hello azonos módon - először megnyitja hello kapcsolat használatával [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) hello ügyfél API-t. .NET-objektumokba használhatják a hello rendszeres Dapper bővítmény módszerek toomap hello eredményeit az SQL-lekérdezés:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Vegye figyelembe, hogy hello **használatával** hello DDR kapcsolat hatókörökhöz blokkolása hello blokk toohello egy shard ahol tenantId1 tartják belül az összes művelet. hello lekérdezés csak tárolt hello aktuális shard, de nem megfelelően tárolt bármely más szilánkok hello blogok adja vissza. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Függő Dapper és DapperExtensions útválasztási adatok
További kiterjesztések biztosíthat további kényelmi és absztrakciós hello adatbázisból, adatbázis-alkalmazások fejlesztése során az ökoszisztéma Dapper tartalmaz. DapperExtensions csak példaként szolgál. 

Az alkalmazásban használt DapperExtensions nem változik, hogyan adatbázis-kapcsolatok létrehozása és kezelése. Még mindig hello alkalmazás felelősségi tooopen kapcsolatokat, és rendszeres SQL ügyfél kapcsolatobjektumok hello kiterjesztésmetódusok által várható. A Microsoft hello támaszkodhat [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) fent leírt módon. Mint hello következő mintakódok jelzik, hello mindössze annyi a változás, azt már nem rendelkezik toowrite hello T-SQL utasítások:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

És itt hello kódminta hello lekérdezés: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Átmeneti kezelése
hello Microsoft Patterns & eljárások csapat közzétett hello [átmeneti hiba kezelési alkalmazás blokk](http://msdn.microsoft.com/library/hh680934.aspx) toohelp alkalmazásfejlesztők mérséklése észlelt, amikor hello felhőben futó közös átmeneti hiba feltételeket. További információkért lásd: [Perseverance, minden Triumphs titkos: átmeneti hiba kezelési alkalmazás blokk hello segítségével](http://msdn.microsoft.com/library/dn440719.aspx).

hello kódminta támaszkodik hello átmeneti hiba könyvtár tooprotect átmeneti hibák ellen. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** hello a fenti kódot típusúként van definiálva egy **SqlDatabaseTransientErrorDetectionStrategy** újrapróbálkozás-szám 10-es és 5 másodperc várjon az újrapróbálkozások között eltelt idő. Ha tranzakciók használ, győződjön meg arról, hogy az újra gombra hatókör Visszalépés toohello hello tranzakció hello esetben egy átmeneti hiba verziótól.

## <a name="limitations"></a>Korlátozások
Ebben a dokumentumban leírt hello megközelítések néhány korlátozásokkal jár:

* hello mintakód a dokumentum nem bemutatják, hogyan toomanage séma szilánkok között.
* A kérelemben megadott, feltételezzük, hogy az adatbázis-feldolgozási tartalmazza egyetlen shard hello kérelem által biztosított hello horizontális kulcs által meghatározott. Azonban ez azt feltételezi mindig tartalmaz, például ha az nem lehetséges toomake egy horizontális skálázási kulcsát. tooaddress a, hello elastic database ügyféloldali kódtára tartalmaz hello [MultiShardQuery osztály](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). hello osztály megvalósít egy kapcsolat absztrakciós több szilánkok keresztül lekérdezése. MultiShardQuery Dapper együttes alkalmazásával nem ez a dokumentum hello terjed.

## <a name="conclusion"></a>Összegzés
Alkalmazás Dapper és DapperExtensions könnyen is kihasználhatja a rugalmas adatbázis-eszközt az Azure SQL Database. Ebben a dokumentumban leírt hello lépéseit, ezeket az alkalmazásokat is használhat hello eszköz funkció függő útválasztási módosítása hello létrehozása és megnyitásakor az új adatok [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektumok toouse hello [ OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) hello elastic database ügyféloldali kódtár hívás. Ez korlátozza az új kapcsolatok létrehozása és megnyitott hello alkalmazás módosítások szükséges toothose helyeken. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
