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
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a><span data-ttu-id="49b0d-103">Több-bérlős alkalmazásokhoz a rugalmas adatbáziseszközöket és sorszintű biztonsággal</span><span class="sxs-lookup"><span data-stu-id="49b0d-103">Multi-tenant applications with elastic database tools and row-level security</span></span>
<span data-ttu-id="49b0d-104">[Rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md) és [sorszintű biztonságot (RLS)](https://msdn.microsoft.com/library/dn765131) egy hatékony méretezéshez rugalmasan és hatékonyan hello adatrétegbeli egy több-bérlős alkalmazás az Azure SQL Database képességeket kínál.</span><span class="sxs-lookup"><span data-stu-id="49b0d-104">[Elastic database tools](sql-database-elastic-scale-get-started.md) and [row-level security (RLS)](https://msdn.microsoft.com/library/dn765131) offer a powerful set of capabilities for flexibly and efficiently scaling hello data tier of a multi-tenant application with Azure SQL Database.</span></span> <span data-ttu-id="49b0d-105">Lásd: [Tervminták több-bérlős SaaS-alkalmazásokhoz az Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="49b0d-105">See [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) for more information.</span></span> 

<span data-ttu-id="49b0d-106">Ez a cikk bemutatja, hogyan toouse e technológiák együttesen toobuild egy alkalmazás egy kiválóan méretezhető réteghez, amely támogatja a több-bérlős szilánkok használatával **ADO.NET SqlClient** és/vagy **Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="49b0d-106">This article illustrates how toouse these technologies together toobuild an application with a highly scalable data tier that supports multi-tenant shards, using **ADO.NET SqlClient** and/or **Entity Framework**.</span></span>  

* <span data-ttu-id="49b0d-107">**Rugalmas adatbáziseszközöket** lehetővé teszi a fejlesztők tooscale hello adatok réteg az alkalmazások szabványos horizontális eljárások használatával a .NET-kódtárakra és az Azure-szolgáltatások sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="49b0d-107">**Elastic database tools** enables developers tooscale out hello data tier of an application via industry-standard sharding practices using a set of .NET libraries and Azure service templates.</span></span> <span data-ttu-id="49b0d-108">Hello Elastic Database ügyféloldali kódtár használata szilánkok kezelése segítségével automatizálhatja és egyszerűsítésére hello infrastrukturális feladatok általában társított horizontális.</span><span class="sxs-lookup"><span data-stu-id="49b0d-108">Managing shards with using hello Elastic Database Client Library helps automate and streamline many of hello infrastructural tasks typically associated with sharding.</span></span> 
* <span data-ttu-id="49b0d-109">**A sorszintű biztonsági** lehetővé teszi, hogy a fejlesztők toostore több bérlők hello azonos adatbázis-biztonsági házirendek toofilter ki azon sorait, amelyek nem tartoznak a lekérdezés végrehajtása toohello bérlő használata esetén.</span><span class="sxs-lookup"><span data-stu-id="49b0d-109">**Row-level security** enables developers toostore data for multiple tenants in hello same database using security policies toofilter out rows that do not belong toohello tenant executing a query.</span></span> <span data-ttu-id="49b0d-110">Központi hozzáférési logika az RLS belül hello adatbázis, ahelyett, hogy mint hello alkalmazásban egyszerűbbé teszi a karbantartás, és csökkenti a hiba hello kockázatát, mint egy alkalmazás kódbázis nő.</span><span class="sxs-lookup"><span data-stu-id="49b0d-110">Centralizing access logic with RLS inside hello database, rather than in hello application, simplifies maintenance and reduces hello risk of error as an application’s codebase grows.</span></span> 

<span data-ttu-id="49b0d-111">Együtt ezeket funkciókat, egy alkalmazás is kihasználhatja költség megtakarítások és a hatékonyság növekedését le adatokat tárol, a több bérlők hello ugyanazt a shard adatbázis.</span><span class="sxs-lookup"><span data-stu-id="49b0d-111">Using these features together, an application can benefit from cost savings and efficiency gains by storing data for multiple tenants in hello same shard database.</span></span> <span data-ttu-id="49b0d-112">Hello: egy időben, az alkalmazás továbbra is egyetlen-bérlő szilánkok "prémium" bérlők számára, akik szigorúbb teljesítmény garanciák igényel, mivel a több-bérlős szilánkok nem garantálják egyenlő erőforrás terjesztési bérlők között van hello rugalmasságot toooffer elkülönített.</span><span class="sxs-lookup"><span data-stu-id="49b0d-112">At hello same time, an application still has hello flexibility toooffer isolated, single-tenant shards for “premium” tenants who require stricter performance guarantees since multi-tenant shards do not guarantee equal resource distribution among tenants.</span></span>  

<span data-ttu-id="49b0d-113">Összefoglalva, a rugalmas adatbázis ügyféloldali kódtár hello [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md) API-k automatikusan csatlakozzon a horizontális kulcsával (általában a "TenantId") tartalmazó bérlők toohello megfelelő shard adatbázis.</span><span class="sxs-lookup"><span data-stu-id="49b0d-113">In short, hello elastic database client library’s [data dependent routing](sql-database-elastic-scale-data-dependent-routing.md) APIs automatically connect tenants toohello correct shard database containing their sharding key (generally a “TenantId”).</span></span> <span data-ttu-id="49b0d-114">A csatlakozás után az RLS biztonsági házirend hello adatbázison belül biztosítja bérlők is csak való hozzáférést a sorokat a TenantId.</span><span class="sxs-lookup"><span data-stu-id="49b0d-114">Once connected, an RLS security policy within hello database ensures that tenants can only access rows that contain their TenantId.</span></span> <span data-ttu-id="49b0d-115">Feltételezzük, hogy minden olyan táblát tartalmaz-e a TenantId oszlop tooindicate mely sorai tooeach bérlőhöz tartozik.</span><span class="sxs-lookup"><span data-stu-id="49b0d-115">It is assumed that all tables contain a TenantId column tooindicate which rows belong tooeach tenant.</span></span> 

![Bloggolás app architektúrája][1]

## <a name="download-hello-sample-project"></a><span data-ttu-id="49b0d-117">Hello mintaprojektet letöltése</span><span class="sxs-lookup"><span data-stu-id="49b0d-117">Download hello sample project</span></span>
### <a name="prerequisites"></a><span data-ttu-id="49b0d-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="49b0d-118">Prerequisites</span></span>
* <span data-ttu-id="49b0d-119">Használja a Visual Studio (2012 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="49b0d-119">Use Visual Studio (2012 or higher)</span></span> 
* <span data-ttu-id="49b0d-120">Hozzon létre három Azure SQL-adatbázisok</span><span class="sxs-lookup"><span data-stu-id="49b0d-120">Create three Azure SQL Databases</span></span> 
* <span data-ttu-id="49b0d-121">A minta-projekt letöltése: [az Azure SQL - több-Bérlős szilánkok rugalmas adatbázis-eszközök](http://go.microsoft.com/?linkid=9888163)</span><span class="sxs-lookup"><span data-stu-id="49b0d-121">Download sample project: [Elastic DB Tools for Azure SQL - Multi-Tenant Shards](http://go.microsoft.com/?linkid=9888163)</span></span>
  * <span data-ttu-id="49b0d-122">Töltse ki az adatbázisokat a hello elején hello információi **Program.cs**</span><span class="sxs-lookup"><span data-stu-id="49b0d-122">Fill in hello information for your databases at hello beginning of **Program.cs**</span></span> 

<span data-ttu-id="49b0d-123">Ebben a projektben egy ismertetett hello bővíti [rugalmas adatbázis-eszközök az Azure SQL - entitás Keretrendszeri integrációját](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) a több-bérlős shard adatbázisok támogatásának hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="49b0d-123">This project extends hello one described in [Elastic DB Tools for Azure SQL - Entity Framework Integration](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) by adding support for multi-tenant shard databases.</span></span> <span data-ttu-id="49b0d-124">Blogok és hozzászólásokat, négy bérlők és két több-bérlős shard adatbázis hello diagram fent ismertetett módon való létrehozásának egyszerű konzolalkalmazásként-buildekről nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="49b0d-124">It builds a simple console application for creating blogs and posts, with four tenants and two multi-tenant shard databases as illustrated in hello above diagram.</span></span> 

<span data-ttu-id="49b0d-125">Hozza létre és hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="49b0d-125">Build and run hello application.</span></span> <span data-ttu-id="49b0d-126">Ez fogja bootstrap hello rugalmas adatbázis eszközök shard térkép manager és a következő tesztek futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="49b0d-126">This will bootstrap hello elastic database tools’ shard map manager and run hello following tests:</span></span> 

1. <span data-ttu-id="49b0d-127">Az Entity Framework és a LINQ, hozzon létre egy új blog, és akkor jelenít meg az egyes bérlők számára a blogok</span><span class="sxs-lookup"><span data-stu-id="49b0d-127">Using Entity Framework and LINQ, create a new blog and then display all blogs for each tenant</span></span>
2. <span data-ttu-id="49b0d-128">A bérlő a blogok ADO.NET SqlClient használ, megjelenítése</span><span class="sxs-lookup"><span data-stu-id="49b0d-128">Using ADO.NET SqlClient, display all blogs for a tenant</span></span>
3. <span data-ttu-id="49b0d-129">Próbálja meg tooinsert hello megfelelő bérlői tooverify, hogy hiba történt a blog</span><span class="sxs-lookup"><span data-stu-id="49b0d-129">Try tooinsert a blog for hello wrong tenant tooverify that an error is thrown</span></span>  

<span data-ttu-id="49b0d-130">Figyelje meg, hogy RLS még nincs engedélyezve a hello shard adatbázisok, mert e vizsgálatok során probléma: bérlők képes toosee blogok, amelyek nem tartoznak toothem, és nem korlátozott hello alkalmazás hello megfelelő bérlő blog beszúrni.</span><span class="sxs-lookup"><span data-stu-id="49b0d-130">Notice that because RLS has not yet been enabled in hello shard databases, each of these tests reveals a problem: tenants are able toosee blogs that do not belong toothem, and hello application is not prevented from inserting a blog for hello wrong tenant.</span></span> <span data-ttu-id="49b0d-131">hello a cikk ismerteti, hogyan tooresolve kényszerítésével problémák bérlői elkülönítési az RLS.</span><span class="sxs-lookup"><span data-stu-id="49b0d-131">hello remainder of this article describes how tooresolve these problems by enforcing tenant isolation with RLS.</span></span> <span data-ttu-id="49b0d-132">Két lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="49b0d-132">There are two steps:</span></span> 

1. <span data-ttu-id="49b0d-133">**Alkalmazás réteg**: hello alkalmazás kódjának módosítása tooalways set hello aktuális TenantId hello SESSION_CONTEXT a kapcsolat megnyitása után.</span><span class="sxs-lookup"><span data-stu-id="49b0d-133">**Application tier**: Modify hello application code tooalways set hello current TenantId in hello SESSION_CONTEXT after opening a connection.</span></span> <span data-ttu-id="49b0d-134">hello mintaprojektet már megtörtént a.</span><span class="sxs-lookup"><span data-stu-id="49b0d-134">hello sample project has already done this.</span></span> 
2. <span data-ttu-id="49b0d-135">**Adatszint**: RLS biztonsági szabályzat létrehozása az egyes shard adatbázis toofilter hello TenantId alapján sorokat SESSION_CONTEXT tárolja.</span><span class="sxs-lookup"><span data-stu-id="49b0d-135">**Data tier**: Create an RLS security policy in each shard database toofilter rows based on hello TenantId stored in SESSION_CONTEXT.</span></span> <span data-ttu-id="49b0d-136">Szüksége lesz toodo a shard adatbázisok mindegyike esetében, ellenkező esetben a több-bérlős szilánkok sorok nem lesznek szűrve.</span><span class="sxs-lookup"><span data-stu-id="49b0d-136">You will need toodo this for each of your shard databases, otherwise rows in multi-tenant shards will not be filtered.</span></span> 

## <a name="step-1-application-tier-set-tenantid-in-hello-sessioncontext"></a><span data-ttu-id="49b0d-137">1. lépés) alkalmazás réteg: állítsa be a TenantId a hello SESSION_CONTEXT</span><span class="sxs-lookup"><span data-stu-id="49b0d-137">Step 1) Application tier: Set TenantId in hello SESSION_CONTEXT</span></span>
<span data-ttu-id="49b0d-138">Után hello elastic database ügyféloldali kódtár adatok függő útválasztási API-k, hello alkalmazás még csatlakozó tooa shard adatbázist vissza kell tootell hello adatbázis melyik TenantId használja ezt a kapcsolatot, hogy az RLS-biztonsági házirend kiszűrheti sorok tartozó tooother bérlők.</span><span class="sxs-lookup"><span data-stu-id="49b0d-138">After connecting tooa shard database using hello elastic database client library’s data dependent routing APIs, hello application still needs tootell hello database which TenantId is using that connection so that an RLS security policy can filter out rows belonging tooother tenants.</span></span> <span data-ttu-id="49b0d-139">Ez az információ ajánlott toopass hello toostore hello az adott kapcsolathoz hello az aktuális TenantId [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx).</span><span class="sxs-lookup"><span data-stu-id="49b0d-139">hello recommended way toopass this information is toostore hello current TenantId for that connection in hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx).</span></span> <span data-ttu-id="49b0d-140">(Megjegyzés: Másik megoldásként használhat [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), de SESSION_CONTEXT jobb megoldás, mert az egyszerűbb toouse, alapértelmezés szerint NULL értéket ad vissza, és támogatja a kulcs-érték párok.)</span><span class="sxs-lookup"><span data-stu-id="49b0d-140">(Note: You could alternatively use [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), but SESSION_CONTEXT is a better option because it is easier toouse, returns NULL by default, and supports key-value pairs.)</span></span>

### <a name="entity-framework"></a><span data-ttu-id="49b0d-141">Entity Framework</span><span class="sxs-lookup"><span data-stu-id="49b0d-141">Entity Framework</span></span>
<span data-ttu-id="49b0d-142">Az Entity Framework használó alkalmazások, hello legegyszerűbb megoldás, belül ElasticScaleContext felülbírálása hello SESSION_CONTEXT ismertetett tooset hello [adatok függő útválasztás használata EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="49b0d-142">For applications using Entity Framework, hello easiest approach is tooset hello SESSION_CONTEXT within hello ElasticScaleContext override described in [Data Dependent Routing using EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext).</span></span> <span data-ttu-id="49b0d-143">Kell a visszatérésre hello kapcsolaton keresztül az adatok függő útválasztási közvetítőalapú, egyszerűen hozzon létre, és hajtsa végre a "TenantId" megadó SqlCommand hello SESSION_CONTEXT toohello shardingKey az adott kapcsolathoz megadott.</span><span class="sxs-lookup"><span data-stu-id="49b0d-143">Before returning hello connection brokered through data dependent routing, simply create and execute a SqlCommand that sets 'TenantId' in hello SESSION_CONTEXT toohello shardingKey specified for that connection.</span></span> <span data-ttu-id="49b0d-144">Ezzel a módszerrel csak akkor kell toowrite kód után tooset hello SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="49b0d-144">This way, you only need toowrite code once tooset hello SESSION_CONTEXT.</span></span> 

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

<span data-ttu-id="49b0d-145">Most hello SESSION_CONTEXT automatikusan beállított hello TenantId megadott, amikor ElasticScaleContext meghívták:</span><span class="sxs-lookup"><span data-stu-id="49b0d-145">Now hello SESSION_CONTEXT is automatically set with hello specified TenantId whenever ElasticScaleContext is invoked:</span></span> 

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

### <a name="adonet-sqlclient"></a><span data-ttu-id="49b0d-146">Az ADO.NET SqlClient</span><span class="sxs-lookup"><span data-stu-id="49b0d-146">ADO.NET SqlClient</span></span>
<span data-ttu-id="49b0d-147">Az alkalmazások ADO.NET SqlClient hello ajánlott megközelítés toocreate körül, amely automatikusan beállítja a TenantId"hello SESSION_CONTEXT toohello ShardMap.OpenConnectionForKey() burkoló függvény előtt javítsa ki a TenantId ad vissza egy a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="49b0d-147">For applications using ADO.NET SqlClient, hello recommended approach is toocreate a wrapper function around ShardMap.OpenConnectionForKey() that automatically sets 'TenantId' in hello SESSION_CONTEXT toohello correct TenantId before returning a connection.</span></span> <span data-ttu-id="49b0d-148">SESSION_CONTEXT mindig a beállított tooensure, csak nyisson meg kapcsolatokat a burkoló funkció segítségével.</span><span class="sxs-lookup"><span data-stu-id="49b0d-148">tooensure that SESSION_CONTEXT is always set, you should only open connections using this wrapper function.</span></span>

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

## <a name="step-2-data-tier-create-row-level-security-policy"></a><span data-ttu-id="49b0d-149">2. lépés) az adatok réteg: sorszintű biztonsági szabályzat létrehozása</span><span class="sxs-lookup"><span data-stu-id="49b0d-149">Step 2) Data tier: Create row-level security policy</span></span>
### <a name="create-a-security-policy-toofilter-hello-rows-each-tenant-can-access"></a><span data-ttu-id="49b0d-150">Hozzon létre egy biztonsági házirend toofilter hello férhetnek hozzá mindegyik bérlő sorok</span><span class="sxs-lookup"><span data-stu-id="49b0d-150">Create a security policy toofilter hello rows each tenant can access</span></span>
<span data-ttu-id="49b0d-151">Most, hogy hello alkalmazás állít a SESSION_CONTEXT hello aktuális TenantId kérdez le, mielőtt az RLS-biztonsági házirend lekérdezések szűrheti és egy másik TenantId tartalmazó sorok kihagyása.</span><span class="sxs-lookup"><span data-stu-id="49b0d-151">Now that hello application is setting SESSION_CONTEXT with hello current TenantId before querying, an RLS security policy can filter queries and exclude rows that have a different TenantId.</span></span>  

<span data-ttu-id="49b0d-152">RLS megvalósítása a T-SQL: egy felhasználó által definiált függvény hello access programot határozza meg, és egy biztonsági házirend köti a táblák függvény tooany száma.</span><span class="sxs-lookup"><span data-stu-id="49b0d-152">RLS is implemented in T-SQL: a user-defined function defines hello access logic, and a security policy binds this function tooany number of tables.</span></span> <span data-ttu-id="49b0d-153">Ebben a projektben hello függvény fog egyszerűen győződjön meg arról, hogy hello alkalmazás (nem pedig egy másik SQL-felhasználó) csatlakoztatott toohello adatbázis, és adott hello SESSION_CONTEXT tárolt "TenantId" hello megegyezik-e a TenantId adott sor hello.</span><span class="sxs-lookup"><span data-stu-id="49b0d-153">For this project, hello function will simply verify that hello application (rather than some other SQL user) is connected toohello database, and that hello 'TenantId' stored in hello SESSION_CONTEXT matches hello TenantId of a given row.</span></span> <span data-ttu-id="49b0d-154">A szűrőpredikátum lehetővé teszi a megfelelő ezen feltételek toopass keresztül hello szűrő SELECT, UPDATE és DELETE lekérdezések; sorok és egy blokkpredikátumok megakadályozza, hogy ezek a feltételek nem beszúrt vagy frissített megsértő sorok.</span><span class="sxs-lookup"><span data-stu-id="49b0d-154">A filter predicate will allow rows that meet these conditions toopass through hello filter for SELECT, UPDATE, and DELETE queries; and a block predicate will prevent rows that violate these conditions from being INSERTed or UPDATEd.</span></span> <span data-ttu-id="49b0d-155">Ha nincs megadva SESSION_CONTEXT, ad vissza NULL és a sor nem lesz látható, vagy képes toobe beszúrni.</span><span class="sxs-lookup"><span data-stu-id="49b0d-155">If SESSION_CONTEXT has not been set, it will return NULL and no rows will be visible or able toobe inserted.</span></span> 

<span data-ttu-id="49b0d-156">RLS, tooenable hajtható végre a következő T-SQL használatával, vagy a Visual Studio (SSDT), SSMS, minden szegmensben osztják meg hello vagy hello projektben tartalmazott PowerShell-parancsfájl hello (vagy használatakor [rugalmas adatbázis-feladatok](sql-database-elastic-jobs-overview.md), használhatja azt tooautomate végrehajtása a T-SQL minden szegmensben osztják meg):</span><span class="sxs-lookup"><span data-stu-id="49b0d-156">tooenable RLS, execute hello following T-SQL on all shards using either Visual Studio (SSDT), SSMS, or hello PowerShell script included in hello project (or if you are using [Elastic Database Jobs](sql-database-elastic-jobs-overview.md), you can use it tooautomate execution of this T-SQL on all shards):</span></span> 

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
> <span data-ttu-id="49b0d-157">Összetettebb tooadd hello predikátum a táblák több száz igénylő projektek használhatja a segítő tárolt eljárás, amely automatikusan hoz létre egy biztonsági házirend predikátum ad hozzá a séma összes tábla.</span><span class="sxs-lookup"><span data-stu-id="49b0d-157">For more complex projects that need tooadd hello predicate on hundreds of tables, you can use a helper stored procedure that automatically generates a security policy adding a predicate on all tables in a schema.</span></span> <span data-ttu-id="49b0d-158">Lásd: [sorszintű biztonság alkalmazása tooall táblák - segítő parancsfájl (blogbejegyzés)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).</span><span class="sxs-lookup"><span data-stu-id="49b0d-158">See [Apply Row-Level Security tooall tables - helper script (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).</span></span>  
> 
> 

<span data-ttu-id="49b0d-159">Most futtassa újra a hello mintaalkalmazás, ha bérlők lesz képes toosee csak azon sorait, amelyek toothem tartozik.</span><span class="sxs-lookup"><span data-stu-id="49b0d-159">Now if you run hello sample application again, tenants will able toosee only rows that belong toothem.</span></span> <span data-ttu-id="49b0d-160">Ezenkívül hello alkalmazás nem szúrható be azon sorait, amelyek tootenants hello egy jelenleg csatlakoztatott toohello shard adatbázis nem tartozik, és látható sorok toohave egy másik TenantId nem tudja frissíteni.</span><span class="sxs-lookup"><span data-stu-id="49b0d-160">In addition, hello application cannot insert rows that belong tootenants other than hello one currently connected toohello shard database, and it cannot update visible rows toohave a different TenantId.</span></span> <span data-ttu-id="49b0d-161">Hello kísérletet tesz toodo vagy, ha egy DbUpdateException generál.</span><span class="sxs-lookup"><span data-stu-id="49b0d-161">If hello application attempts toodo either, a DbUpdateException will be raised.</span></span>

<span data-ttu-id="49b0d-162">Ha később ad hozzá egy új tábla, egyszerűen ALTER hello biztonsági házirend, és és a block predikátumok hello új táblázat hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="49b0d-162">If you add a new table later on, simply ALTER hello security policy and add filter and block predicates on hello new table:</span></span> 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-tooautomatically-populate-tenantid-for-inserts"></a><span data-ttu-id="49b0d-163">Adja hozzá az alapértelmezett megkötések tooautomatically beszúrása a TenantId feltöltése</span><span class="sxs-lookup"><span data-stu-id="49b0d-163">Add default constraints tooautomatically populate TenantId for INSERTs</span></span>
<span data-ttu-id="49b0d-164">Adhat meg egy alapértelmezett korlátozás minden tábla tooautomatically feltöltése hello TenantId a hello jelenleg tárolt SESSION_CONTEXT sorok beszúráskor érték.</span><span class="sxs-lookup"><span data-stu-id="49b0d-164">You can put a default constraint on each table tooautomatically populate hello TenantId with hello value currently stored in SESSION_CONTEXT when inserting rows.</span></span> <span data-ttu-id="49b0d-165">Példa:</span><span class="sxs-lookup"><span data-stu-id="49b0d-165">For example:</span></span> 

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

<span data-ttu-id="49b0d-166">Hello alkalmazás most már nem kell a TenantId toospecify sorok beszúráskor:</span><span class="sxs-lookup"><span data-stu-id="49b0d-166">Now hello application does not need toospecify a TenantId when inserting rows:</span></span> 

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
> <span data-ttu-id="49b0d-167">Ha az Entity Framework projektek használja alapértelmezett korlátozásokban, javasolt, hogy nem tartalmaznak-e be a hello TenantId oszlop az EF adatmodell.</span><span class="sxs-lookup"><span data-stu-id="49b0d-167">If you use default constraints for an Entity Framework project, it is recommended that you do NOT include hello TenantId column in your EF data model.</span></span> <span data-ttu-id="49b0d-168">Ennek oka az Entity Framework lekérdezések automatikusan megadhatja az olyan alapértelmezett értékeket, amelyek felülírják hello alapértelmezett korlátozásokban T-SQL-ben létrehozott SESSION_CONTEXT használó.</span><span class="sxs-lookup"><span data-stu-id="49b0d-168">This is because Entity Framework queries automatically supply default values which will override hello default constraints created in T-SQL that use SESSION_CONTEXT.</span></span> <span data-ttu-id="49b0d-169">toouse alapértelmezett korlátozásokban szereplő hello minta projekt, például el kell távolítani a TenantId DataClasses.cs (és futtatási Add-áttelepítés hello Csomagkezelő konzol), és, hogy a mező hello használata T-SQL tooensure csak hello adatbázistáblák szerepel.</span><span class="sxs-lookup"><span data-stu-id="49b0d-169">toouse default constraints in hello sample project, for instance, you should remove TenantId from DataClasses.cs (and run Add-Migration in hello Package Manager Console) and use T-SQL tooensure that hello field only exists in hello database tables.</span></span> <span data-ttu-id="49b0d-170">Ezzel a módszerrel EF nem automatikusan megadja helytelen alapértelmezett értékek adatok beszúráskor.</span><span class="sxs-lookup"><span data-stu-id="49b0d-170">This way, EF will not automatically supply incorrect default values when inserting data.</span></span> 
> 
> 

### <a name="optional-enable-a-superuser-tooaccess-all-rows"></a><span data-ttu-id="49b0d-171">(Választható) Egy "felügyelő" tooaccess minden sor engedélyezése</span><span class="sxs-lookup"><span data-stu-id="49b0d-171">(Optional) Enable a "superuser" tooaccess all rows</span></span>
<span data-ttu-id="49b0d-172">Egyes alkalmazások toocreate "felügyelőt" elérő összes sorát, például a rendelés tooenable egyetlen bérlő számára az összes szilánkok vagy tooperform vegyes/egyesítési műveletek, például az adatbázisok közötti áthelyezése bérlői sorok szilánkok a reporting válhat.</span><span class="sxs-lookup"><span data-stu-id="49b0d-172">Some applications may want toocreate a "superuser" who can access all rows, for instance, in order tooenable reporting across all tenants on all shards, or tooperform Split/Merge operations on shards that involve moving tenant rows between databases.</span></span> <span data-ttu-id="49b0d-173">tooenable, minden egyes shard adatbázisban kell hozzon létre egy új SQL-felhasználó (ebben a példában "felügyelő" jelöli).</span><span class="sxs-lookup"><span data-stu-id="49b0d-173">tooenable this, you should create a new SQL user ("superuser" in this example) in each shard database.</span></span> <span data-ttu-id="49b0d-174">Majd alter hello biztonsági házirendet, és egy új predikátum függvény, amely lehetővé teszi a felhasználó tooaccess összes sort:</span><span class="sxs-lookup"><span data-stu-id="49b0d-174">Then alter hello security policy with a new predicate function that allows this user tooaccess all rows:</span></span>

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


### <a name="maintenance"></a><span data-ttu-id="49b0d-175">Karbantartás</span><span class="sxs-lookup"><span data-stu-id="49b0d-175">Maintenance</span></span>
* <span data-ttu-id="49b0d-176">**Új szilánkok hozzáadása**: végre kell hajtani a bármely új szilánkok hello T-SQL parancsfájl tooenable RLS, ellenkező esetben ezek a szilánkok lekérdezései nem lesznek szűrve.</span><span class="sxs-lookup"><span data-stu-id="49b0d-176">**Adding new shards**: You must execute hello T-SQL script tooenable RLS on any new shards, otherwise queries on these shards will not be filtered.</span></span>
* <span data-ttu-id="49b0d-177">**Új táblák hozzáadásáról**: hozzá kell adnia egy szűrő és predikátum toohello biztonsági házirendet a összes szilánkok letiltása, amikor egy új tábla létrehozása, ellenkező esetben hello új tábla lekérdezései nem lesznek szűrve.</span><span class="sxs-lookup"><span data-stu-id="49b0d-177">**Adding new tables**: You must add a filter and block predicate toohello security policy on all shards whenever a new table is created, otherwise queries on hello new table will not be filtered.</span></span> <span data-ttu-id="49b0d-178">Ez automatizálható a DDL-eseményindítót használ a [sorszintű biztonság alkalmazása automatikusan toonewly létre táblák (blogbejegyzés)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).</span><span class="sxs-lookup"><span data-stu-id="49b0d-178">This can be automated using a DDL trigger, as described in [Apply Row-Level Security automatically toonewly created tables (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).</span></span>

## <a name="summary"></a><span data-ttu-id="49b0d-179">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="49b0d-179">Summary</span></span>
<span data-ttu-id="49b0d-180">Az alkalmazás adatokat réteg támogatja a több-bérlős és a bérlői egyetlen szilánkok használt együtt tooscale lehetnek rugalmas adatbáziseszközöket és sorszintű biztonság.</span><span class="sxs-lookup"><span data-stu-id="49b0d-180">Elastic database tools and row-level security can be used together tooscale out an application’s data tier with support for both multi-tenant and single-tenant shards.</span></span> <span data-ttu-id="49b0d-181">Több-bérlős szilánkok lehet használt toostore adatok hatékonyabban (különösen olyan esetekben, ahol nagyszámú bérlők csak néhány sornyi adatot), egyetlen-bérlő közben szilánkok lehet használt toosupport prémium bérlők szigorúbb teljesítmény és elkülönítés követelmények.</span><span class="sxs-lookup"><span data-stu-id="49b0d-181">Multi-tenant shards can be used toostore data more efficiently (particularly in cases where a large number of tenants have only a few rows of data), while single-tenant shards can be used toosupport premium tenants with stricter performance and isolation requirements.</span></span>  <span data-ttu-id="49b0d-182">További információkért lásd: [sorszintű biztonság hivatkozás](https://msdn.microsoft.com/library/dn765131).</span><span class="sxs-lookup"><span data-stu-id="49b0d-182">For more information, see [Row-Level Security reference](https://msdn.microsoft.com/library/dn765131).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="49b0d-183">További források</span><span class="sxs-lookup"><span data-stu-id="49b0d-183">Additional resources</span></span>
* [<span data-ttu-id="49b0d-184">Mi az Azure rugalmas készletek?</span><span class="sxs-lookup"><span data-stu-id="49b0d-184">What is an Azure elastic pool?</span></span>](sql-database-elastic-pool.md)
* [<span data-ttu-id="49b0d-185">Horizontális felskálázás az Azure SQL Database segítségével</span><span class="sxs-lookup"><span data-stu-id="49b0d-185">Scaling out with Azure SQL Database</span></span>](sql-database-elastic-scale-introduction.md)
* [<span data-ttu-id="49b0d-186">Tervezési minták az Azure SQL Database-t használó több-bérlős SaaS-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="49b0d-186">Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database</span></span>](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [<span data-ttu-id="49b0d-187">Hitelesítés a több-bérlős alkalmazásokban az Azure AD és az OpenID Connect segítségével</span><span class="sxs-lookup"><span data-stu-id="49b0d-187">Authentication in multitenant apps, using Azure AD and OpenID Connect</span></span>](../guidance/guidance-multitenant-identity-authenticate.md)
* [<span data-ttu-id="49b0d-188">A Tailspin Surveys-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="49b0d-188">Tailspin Surveys application</span></span>](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a><span data-ttu-id="49b0d-189">Kérdések és funkciókra vonatkozó kérések</span><span class="sxs-lookup"><span data-stu-id="49b0d-189">Questions and Feature Requests</span></span>
<span data-ttu-id="49b0d-190">A kérdésekhez, lépjen kapcsolatba a hello toous [SQL-adatbázis fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a funkciókra vonatkozó kérések, adjon hozzá toohello [SQL adatbázis-visszajelzési fórumon](https://feedback.azure.com/forums/217321-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="49b0d-190">For questions, please reach out toous on hello [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them toohello [SQL Database feedback forum](https://feedback.azure.com/forums/217321-sql-database/).</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


