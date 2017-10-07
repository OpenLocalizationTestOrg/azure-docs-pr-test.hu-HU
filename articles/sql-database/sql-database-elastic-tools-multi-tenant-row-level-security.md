---
title: "aaaMulti bérlői alkalmazások rugalmas adatbáziseszközöket és sorszintű biztonsággal"
description: "Ismerje meg, hogyan toouse rugalmas adatbáziseszközöket és a sorszintű biztonsági toobuild egy alkalmazás és az Azure SQL Database, amely támogatja a több-bérlős szilánkok egy kiválóan méretezhető réteg."
metakeywords: azure sql database elastic tools multi tenant row level security rls
services: sql-database
documentationcenter: 
manager: jhubbard
author: tmullaney
ms.assetid: e72d3cfe-e9be-4326-b776-9c6d96c0a18e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: thmullan;torsteng
ms.openlocfilehash: e00076a8db4a295374993aedd49f2318bd4d701d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Több-bérlős alkalmazásokhoz a rugalmas adatbáziseszközöket és sorszintű biztonsággal
[Rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md) és [sorszintű biztonságot (RLS)](https://msdn.microsoft.com/library/dn765131) egy hatékony méretezéshez rugalmasan és hatékonyan hello adatrétegbeli egy több-bérlős alkalmazás az Azure SQL Database képességeket kínál. Lásd: [Tervminták több-bérlős SaaS-alkalmazásokhoz az Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) további információt. 

Ez a cikk bemutatja, hogyan toouse e technológiák együttesen toobuild egy alkalmazás egy kiválóan méretezhető réteghez, amely támogatja a több-bérlős szilánkok használatával **ADO.NET SqlClient** és/vagy **Entity Framework**.  

* **Rugalmas adatbáziseszközöket** lehetővé teszi a fejlesztők tooscale hello adatok réteg az alkalmazások szabványos horizontális eljárások használatával a .NET-kódtárakra és az Azure-szolgáltatások sablon használatával. Hello Elastic Database ügyféloldali kódtár használata szilánkok kezelése segítségével automatizálhatja és egyszerűsítésére hello infrastrukturális feladatok általában társított horizontális. 
* **A sorszintű biztonsági** lehetővé teszi, hogy a fejlesztők toostore több bérlők hello azonos adatbázis-biztonsági házirendek toofilter ki azon sorait, amelyek nem tartoznak a lekérdezés végrehajtása toohello bérlő használata esetén. Központi hozzáférési logika az RLS belül hello adatbázis, ahelyett, hogy mint hello alkalmazásban egyszerűbbé teszi a karbantartás, és csökkenti a hiba hello kockázatát, mint egy alkalmazás kódbázis nő. 

Együtt ezeket funkciókat, egy alkalmazás is kihasználhatja költség megtakarítások és a hatékonyság növekedését le adatokat tárol, a több bérlők hello ugyanazt a shard adatbázis. Hello: egy időben, az alkalmazás továbbra is egyetlen-bérlő szilánkok "prémium" bérlők számára, akik szigorúbb teljesítmény garanciák igényel, mivel a több-bérlős szilánkok nem garantálják egyenlő erőforrás terjesztési bérlők között van hello rugalmasságot toooffer elkülönített.  

Összefoglalva, a rugalmas adatbázis ügyféloldali kódtár hello [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md) API-k automatikusan csatlakozzon a horizontális kulcsával (általában a "TenantId") tartalmazó bérlők toohello megfelelő shard adatbázis. A csatlakozás után az RLS biztonsági házirend hello adatbázison belül biztosítja bérlők is csak való hozzáférést a sorokat a TenantId. Feltételezzük, hogy minden olyan táblát tartalmaz-e a TenantId oszlop tooindicate mely sorai tooeach bérlőhöz tartozik. 

![Bloggolás app architektúrája][1]

## <a name="download-hello-sample-project"></a>Hello mintaprojektet letöltése
### <a name="prerequisites"></a>Előfeltételek
* Használja a Visual Studio (2012 vagy újabb) 
* Hozzon létre három Azure SQL-adatbázisok 
* A minta-projekt letöltése: [az Azure SQL - több-Bérlős szilánkok rugalmas adatbázis-eszközök](http://go.microsoft.com/?linkid=9888163)
  * Töltse ki az adatbázisokat a hello elején hello információi **Program.cs** 

Ebben a projektben egy ismertetett hello bővíti [rugalmas adatbázis-eszközök az Azure SQL - entitás Keretrendszeri integrációját](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) a több-bérlős shard adatbázisok támogatásának hozzáadásával. Blogok és hozzászólásokat, négy bérlők és két több-bérlős shard adatbázis hello diagram fent ismertetett módon való létrehozásának egyszerű konzolalkalmazásként-buildekről nyújtanak. 

Hozza létre és hello alkalmazás futtatásához. Ez fogja bootstrap hello rugalmas adatbázis eszközök shard térkép manager és a következő tesztek futtatása hello: 

1. Az Entity Framework és a LINQ, hozzon létre egy új blog, és akkor jelenít meg az egyes bérlők számára a blogok
2. A bérlő a blogok ADO.NET SqlClient használ, megjelenítése
3. Próbálja meg tooinsert hello megfelelő bérlői tooverify, hogy hiba történt a blog  

Figyelje meg, hogy RLS még nincs engedélyezve a hello shard adatbázisok, mert e vizsgálatok során probléma: bérlők képes toosee blogok, amelyek nem tartoznak toothem, és nem korlátozott hello alkalmazás hello megfelelő bérlő blog beszúrni. hello a cikk ismerteti, hogyan tooresolve kényszerítésével problémák bérlői elkülönítési az RLS. Két lépésben történik: 

1. **Alkalmazás réteg**: hello alkalmazás kódjának módosítása tooalways set hello aktuális TenantId hello SESSION_CONTEXT a kapcsolat megnyitása után. hello mintaprojektet már megtörtént a. 
2. **Adatszint**: RLS biztonsági szabályzat létrehozása az egyes shard adatbázis toofilter hello TenantId alapján sorokat SESSION_CONTEXT tárolja. Szüksége lesz toodo a shard adatbázisok mindegyike esetében, ellenkező esetben a több-bérlős szilánkok sorok nem lesznek szűrve. 

## <a name="step-1-application-tier-set-tenantid-in-hello-sessioncontext"></a>1. lépés) alkalmazás réteg: állítsa be a TenantId a hello SESSION_CONTEXT
Után hello elastic database ügyféloldali kódtár adatok függő útválasztási API-k, hello alkalmazás még csatlakozó tooa shard adatbázist vissza kell tootell hello adatbázis melyik TenantId használja ezt a kapcsolatot, hogy az RLS-biztonsági házirend kiszűrheti sorok tartozó tooother bérlők. Ez az információ ajánlott toopass hello toostore hello az adott kapcsolathoz hello az aktuális TenantId [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx). (Megjegyzés: Másik megoldásként használhat [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), de SESSION_CONTEXT jobb megoldás, mert az egyszerűbb toouse, alapértelmezés szerint NULL értéket ad vissza, és támogatja a kulcs-érték párok.)

### <a name="entity-framework"></a>Entity Framework
Az Entity Framework használó alkalmazások, hello legegyszerűbb megoldás, belül ElasticScaleContext felülbírálása hello SESSION_CONTEXT ismertetett tooset hello [adatok függő útválasztás használata EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Kell a visszatérésre hello kapcsolaton keresztül az adatok függő útválasztási közvetítőalapú, egyszerűen hozzon létre, és hajtsa végre a "TenantId" megadó SqlCommand hello SESSION_CONTEXT toohello shardingKey az adott kapcsolathoz megadott. Ezzel a módszerrel csak akkor kell toowrite kód után tooset hello SESSION_CONTEXT. 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data dependent routing. This call will open a validated connection routed toohello proper 
// shard by hello shard map manager. Note that hello base class c'tor call will fail for an open connection 
// if migrations need toobe done and SQL credentials are used. This is hello reason for hello  
// separation of c'tors into hello DDR case (this c'tor) and hello internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map toobroker a validated connection for hello given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT tooshardingKey tooenable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

Most hello SESSION_CONTEXT automatikusan beállított hello TenantId megadott, amikor ElasticScaleContext meghívták: 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;

        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### <a name="adonet-sqlclient"></a>Az ADO.NET SqlClient
Az alkalmazások ADO.NET SqlClient hello ajánlott megközelítés toocreate körül, amely automatikusan beállítja a TenantId"hello SESSION_CONTEXT toohello ShardMap.OpenConnectionForKey() burkoló függvény előtt javítsa ki a TenantId ad vissza egy a kapcsolat. SESSION_CONTEXT mindig a beállított tooensure, csak nyisson meg kapcsolatokat a burkoló funkció segítségével.

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with hello correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method tooensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map toobroker a validated connection for hello given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT tooshardingKey tooenable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs will be listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="step-2-data-tier-create-row-level-security-policy"></a>2. lépés) az adatok réteg: sorszintű biztonsági szabályzat létrehozása
### <a name="create-a-security-policy-toofilter-hello-rows-each-tenant-can-access"></a>Hozzon létre egy biztonsági házirend toofilter hello férhetnek hozzá mindegyik bérlő sorok
Most, hogy hello alkalmazás állít a SESSION_CONTEXT hello aktuális TenantId kérdez le, mielőtt az RLS-biztonsági házirend lekérdezések szűrheti és egy másik TenantId tartalmazó sorok kihagyása.  

RLS megvalósítása a T-SQL: egy felhasználó által definiált függvény hello access programot határozza meg, és egy biztonsági házirend köti a táblák függvény tooany száma. Ebben a projektben hello függvény fog egyszerűen győződjön meg arról, hogy hello alkalmazás (nem pedig egy másik SQL-felhasználó) csatlakoztatott toohello adatbázis, és adott hello SESSION_CONTEXT tárolt "TenantId" hello megegyezik-e a TenantId adott sor hello. A szűrőpredikátum lehetővé teszi a megfelelő ezen feltételek toopass keresztül hello szűrő SELECT, UPDATE és DELETE lekérdezések; sorok és egy blokkpredikátumok megakadályozza, hogy ezek a feltételek nem beszúrt vagy frissített megsértő sorok. Ha nincs megadva SESSION_CONTEXT, ad vissza NULL és a sor nem lesz látható, vagy képes toobe beszúrni. 

RLS, tooenable hajtható végre a következő T-SQL használatával, vagy a Visual Studio (SSDT), SSMS, minden szegmensben osztják meg hello vagy hello projektben tartalmazott PowerShell-parancsfájl hello (vagy használatakor [rugalmas adatbázis-feladatok](sql-database-elastic-jobs-overview.md), használhatja azt tooautomate végrehajtása a T-SQL minden szegmensben osztják meg): 

```
CREATE SCHEMA rls -- separate schema tooorganize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- hello user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [!TIP]
> Összetettebb tooadd hello predikátum a táblák több száz igénylő projektek használhatja a segítő tárolt eljárás, amely automatikusan hoz létre egy biztonsági házirend predikátum ad hozzá a séma összes tábla. Lásd: [sorszintű biztonság alkalmazása tooall táblák - segítő parancsfájl (blogbejegyzés)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).  
> 
> 

Most futtassa újra a hello mintaalkalmazás, ha bérlők lesz képes toosee csak azon sorait, amelyek toothem tartozik. Ezenkívül hello alkalmazás nem szúrható be azon sorait, amelyek tootenants hello egy jelenleg csatlakoztatott toohello shard adatbázis nem tartozik, és látható sorok toohave egy másik TenantId nem tudja frissíteni. Hello kísérletet tesz toodo vagy, ha egy DbUpdateException generál.

Ha később ad hozzá egy új tábla, egyszerűen ALTER hello biztonsági házirend, és és a block predikátumok hello új táblázat hozzáadása: 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-tooautomatically-populate-tenantid-for-inserts"></a>Adja hozzá az alapértelmezett megkötések tooautomatically beszúrása a TenantId feltöltése
Adhat meg egy alapértelmezett korlátozás minden tábla tooautomatically feltöltése hello TenantId a hello jelenleg tárolt SESSION_CONTEXT sorok beszúráskor érték. Példa: 

```
-- Create default constraints tooauto-populate TenantId with hello value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

Hello alkalmazás most már nem kell a TenantId toospecify sorok beszúráskor: 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [!NOTE]
> Ha az Entity Framework projektek használja alapértelmezett korlátozásokban, javasolt, hogy nem tartalmaznak-e be a hello TenantId oszlop az EF adatmodell. Ennek oka az Entity Framework lekérdezések automatikusan megadhatja az olyan alapértelmezett értékeket, amelyek felülírják hello alapértelmezett korlátozásokban T-SQL-ben létrehozott SESSION_CONTEXT használó. toouse alapértelmezett korlátozásokban szereplő hello minta projekt, például el kell távolítani a TenantId DataClasses.cs (és futtatási Add-áttelepítés hello Csomagkezelő konzol), és, hogy a mező hello használata T-SQL tooensure csak hello adatbázistáblák szerepel. Ezzel a módszerrel EF nem automatikusan megadja helytelen alapértelmezett értékek adatok beszúráskor. 
> 
> 

### <a name="optional-enable-a-superuser-tooaccess-all-rows"></a>(Választható) Egy "felügyelő" tooaccess minden sor engedélyezése
Egyes alkalmazások toocreate "felügyelőt" elérő összes sorát, például a rendelés tooenable egyetlen bérlő számára az összes szilánkok vagy tooperform vegyes/egyesítési műveletek, például az adatbázisok közötti áthelyezése bérlői sorok szilánkok a reporting válhat. tooenable, minden egyes shard adatbázisban kell hozzon létre egy új SQL-felhasználó (ebben a példában "felügyelő" jelöli). Majd alter hello biztonsági házirendet, és egy új predikátum függvény, amely lehetővé teszi a felhasználó tooaccess összes sort:

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in hello new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a>Karbantartás
* **Új szilánkok hozzáadása**: végre kell hajtani a bármely új szilánkok hello T-SQL parancsfájl tooenable RLS, ellenkező esetben ezek a szilánkok lekérdezései nem lesznek szűrve.
* **Új táblák hozzáadásáról**: hozzá kell adnia egy szűrő és predikátum toohello biztonsági házirendet a összes szilánkok letiltása, amikor egy új tábla létrehozása, ellenkező esetben hello új tábla lekérdezései nem lesznek szűrve. Ez automatizálható a DDL-eseményindítót használ a [sorszintű biztonság alkalmazása automatikusan toonewly létre táblák (blogbejegyzés)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Összefoglalás
Az alkalmazás adatokat réteg támogatja a több-bérlős és a bérlői egyetlen szilánkok használt együtt tooscale lehetnek rugalmas adatbáziseszközöket és sorszintű biztonság. Több-bérlős szilánkok lehet használt toostore adatok hatékonyabban (különösen olyan esetekben, ahol nagyszámú bérlők csak néhány sornyi adatot), egyetlen-bérlő közben szilánkok lehet használt toosupport prémium bérlők szigorúbb teljesítmény és elkülönítés követelmények.  További információkért lásd: [sorszintű biztonság hivatkozás](https://msdn.microsoft.com/library/dn765131). 

## <a name="additional-resources"></a>További források
* [Mi az Azure rugalmas készletek?](sql-database-elastic-pool.md)
* [Horizontális felskálázás az Azure SQL Database segítségével](sql-database-elastic-scale-introduction.md)
* [Tervezési minták az Azure SQL Database-t használó több-bérlős SaaS-alkalmazásokhoz](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Hitelesítés a több-bérlős alkalmazásokban az Azure AD és az OpenID Connect segítségével](../guidance/guidance-multitenant-identity-authenticate.md)
* [A Tailspin Surveys-alkalmazás](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Kérdések és funkciókra vonatkozó kérések
A kérdésekhez, lépjen kapcsolatba a hello toous [SQL-adatbázis fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a funkciókra vonatkozó kérések, adjon hozzá toohello [SQL adatbázis-visszajelzési fórumon](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


