---
title: "az Azure SQL Database útválasztási függő aaaData |} Microsoft Docs"
description: "Hogyan toouse hello ShardMapManager osztály a .NET-alkalmazások adatok függő útválasztási, szilánkos adatbázisokat az Azure SQL-adatbázis szolgáltatása"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: ddove
ms.openlocfilehash: 34014508ae01905686791fe096bb275cb84f53b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-dependent-routing"></a><span data-ttu-id="e3e96-103">Adatfüggő útválasztás</span><span class="sxs-lookup"><span data-stu-id="e3e96-103">Data dependent routing</span></span>
<span data-ttu-id="e3e96-104">**Adatok függő útválasztási** hello képességét toouse hello adat lekérdezés tooroute hello kérelem tooan megfelelő adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="e3e96-104">**Data dependent routing** is hello ability toouse hello data in a query tooroute hello request tooan appropriate database.</span></span> <span data-ttu-id="e3e96-105">Ez az alapvető mintát, az szilánkos adatbázisok használatakor.</span><span class="sxs-lookup"><span data-stu-id="e3e96-105">This is a fundamental pattern when working with sharded databases.</span></span> <span data-ttu-id="e3e96-106">hello kérelemkörnyezet is lehet használt tooroute hello kérelem, különösen akkor, ha hello horizontális kulcs része nem hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="e3e96-106">hello request context may also be used tooroute hello request, especially if hello sharding key is not part of hello query.</span></span> <span data-ttu-id="e3e96-107">Minden adott lekérdezés vagy a tranzakció az adatok függő útválasztási használó alkalmazások nem korlátozott tooaccessing kérelmenként egy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="e3e96-107">Each specific query or transaction in an application using data dependent routing is restricted tooaccessing a single database per request.</span></span> <span data-ttu-id="e3e96-108">Hello Azure SQL Database rugalmas eszközök, az Útválasztás valósítható meg a hello  **[ShardMapManager osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  ADO.NET-alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="e3e96-108">For hello Azure SQL Database Elastic tools, this routing is accomplished with hello **[ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)** in ADO.NET applications.</span></span>

<span data-ttu-id="e3e96-109">hello alkalmazást nem kell tootrack különböző kapcsolati karakterláncok vagy más szeletek hello szilánkos környezetben adatok társított DB helyek.</span><span class="sxs-lookup"><span data-stu-id="e3e96-109">hello application does not need tootrack various connection strings or DB locations associated with different slices of data in hello sharded environment.</span></span> <span data-ttu-id="e3e96-110">Ehelyett hello [Shard térkép Manager](sql-database-elastic-scale-shard-map-management.md) megnyílik kapcsolatok toohello megfelelő adatbázisok szükség esetén hello shard és hello horizontális kulcs, amely hello cél hello alkalmazás kérelem hello értékének hello adatai alapján.</span><span class="sxs-lookup"><span data-stu-id="e3e96-110">Instead, hello [Shard Map Manager](sql-database-elastic-scale-shard-map-management.md) opens connections toohello correct databases when needed, based on hello data in hello shard map and hello value of hello sharding key that is hello target of hello application’s request.</span></span> <span data-ttu-id="e3e96-111">hello kulcsa általában hello *customer_id*, *tenant_id*, *date_key*, és néhány egyéb egyedi azonosító, amelyet hello adatbázis kérelem alapvető paraméter).</span><span class="sxs-lookup"><span data-stu-id="e3e96-111">hello key is typically hello *customer_id*, *tenant_id*, *date_key*, or some other specific identifier that is a fundamental parameter of hello database request).</span></span> 

<span data-ttu-id="e3e96-112">További információkért lásd: [skálázás, SQL Server Data függő útválasztási](https://technet.microsoft.com/library/cc966448.aspx).</span><span class="sxs-lookup"><span data-stu-id="e3e96-112">For more information, see [Scaling Out SQL Server with Data Dependent Routing](https://technet.microsoft.com/library/cc966448.aspx).</span></span>

## <a name="download-hello-client-library"></a><span data-ttu-id="e3e96-113">Töltse le a hello ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="e3e96-113">Download hello client library</span></span>
<span data-ttu-id="e3e96-114">tooget hello osztály telepítés hello [Elastic Database ügyféloldali kódtár](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="e3e96-114">tooget hello class, install hello [Elastic Database Client Library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span> 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a><span data-ttu-id="e3e96-115">Egy ShardMapManager használata adatok függő útválasztási alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="e3e96-115">Using a ShardMapManager in a data dependent routing application</span></span>
<span data-ttu-id="e3e96-116">Alkalmazások kell példányosítani hello **ShardMapManager** hello gyári hívás használatával az inicializálás során  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="e3e96-116">Applications should instantiate hello **ShardMapManager** during initialization, using hello factory call **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span></span> <span data-ttu-id="e3e96-117">Ebben a példában mind a **ShardMapManager** és egy adott **ShardMap** benne foglalt inicializálása.</span><span class="sxs-lookup"><span data-stu-id="e3e96-117">In this example, both a **ShardMapManager** and a specific **ShardMap** that it contains are initialized.</span></span> <span data-ttu-id="e3e96-118">Ez a példa azt mutatja be hello GetSqlShardMapManager és [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) módszerek.</span><span class="sxs-lookup"><span data-stu-id="e3e96-118">This example shows hello GetSqlShardMapManager and [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) methods.</span></span>

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-hello-shard-map"></a><span data-ttu-id="e3e96-119">Hello shard leképezés első legkisebb lehetséges jogosultságokat biztosító hitelesítő adatok használata</span><span class="sxs-lookup"><span data-stu-id="e3e96-119">Use lowest privilege credentials possible for getting hello shard map</span></span>
<span data-ttu-id="e3e96-120">Ha egy alkalmazás nem van kezelésére hello shard térkép magát, hello gyári metódusban használt hello hitelesítő adatok csak olvasási engedélyekkel kell rendelkezniük a hello **globális Shard térkép** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e3e96-120">If an application is not manipulating hello shard map itself, hello credentials used in hello factory method should have just read-only permissions on hello **Global Shard Map** database.</span></span> <span data-ttu-id="e3e96-121">Ezek a hitelesítő adatok általában eltérnek a használt hitelesítő adatok tooopen kapcsolatok toohello shard térkép manager.</span><span class="sxs-lookup"><span data-stu-id="e3e96-121">These credentials are typically different from credentials used tooopen connections toohello shard map manager.</span></span> <span data-ttu-id="e3e96-122">Lásd még: [a hitelesítő adatokat használni tooaccess hello Elastic Database ügyféloldali kódtárának](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="e3e96-122">See also [Credentials used tooaccess hello Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span> 

## <a name="call-hello-openconnectionforkey-method"></a><span data-ttu-id="e3e96-123">Hello OpenConnectionForKey metódus hívása</span><span class="sxs-lookup"><span data-stu-id="e3e96-123">Call hello OpenConnectionForKey method</span></span>
<span data-ttu-id="e3e96-124">Hello  **[ShardMap.OpenConnectionForKey metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  adja vissza egy ADO.Net készen parancsok toohello megfelelő adatbázis hello hello értéke alapján kiállító **kulcs**paraméter.</span><span class="sxs-lookup"><span data-stu-id="e3e96-124">hello **[ShardMap.OpenConnectionForKey method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)** returns an ADO.Net connection ready for issuing commands toohello appropriate database based on hello value of hello **key** parameter.</span></span> <span data-ttu-id="e3e96-125">A shard adatbázisban tárolja a hello alkalmazás által hello **ShardMapManager**, így ezek a kérelmek jellemzően nem foglalják magukban egy adatbázis-lekérdezés alapján hello **globális Shard térkép** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e3e96-125">Shard information is cached in hello application by hello **ShardMapManager**, so these requests do not typically involve a database lookup against hello **Global Shard Map** database.</span></span> 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* <span data-ttu-id="e3e96-126">Hello **kulcs** paraméter keresési kulcsként adatbázisba hello shard térkép toodetermine hello megfelelő hello kéréshez használt.</span><span class="sxs-lookup"><span data-stu-id="e3e96-126">hello **key** parameter is used as a lookup key into hello shard map toodetermine hello appropriate database for hello request.</span></span> 
* <span data-ttu-id="e3e96-127">Hello **connectionString** használt toopass csak hello felhasználói hitelesítő adatok hello szükség van.</span><span class="sxs-lookup"><span data-stu-id="e3e96-127">hello **connectionString** is used toopass only hello user credentials for hello desired connection.</span></span> <span data-ttu-id="e3e96-128">Nincs adatbázis vagy a kiszolgáló neve szerepelnek a *connectionString* óta hello módszer határozza meg, hogy hello adatbázis és a kiszolgáló hello **ShardMap**.</span><span class="sxs-lookup"><span data-stu-id="e3e96-128">No database name or server name are included in this *connectionString* since hello method will determine hello database and server using hello **ShardMap**.</span></span> 
* <span data-ttu-id="e3e96-129">Hello  **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  túl meg kell**ConnectionOptions.Validate** Ha egy olyan környezetben, ahol a shard leképezi módosíthatja, és a sorok tooother adatbázisok, előfordulhat, hogy áthelyezése egy vegyes vagy az egyesítési művelet eredménye.</span><span class="sxs-lookup"><span data-stu-id="e3e96-129">hello **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)** should be set too**ConnectionOptions.Validate** if an environment where shard maps may change and rows may move tooother databases as a result of split or merge operations.</span></span> <span data-ttu-id="e3e96-130">Ebbe beletartozik egy rövid lekérdezés toohello helyi shard térkép hello céladatbázis (toohello globális shard társítani) hello kapcsolat előtt a rendszer toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e3e96-130">This involves a brief query toohello local shard map on hello target database (not toohello global shard map) before hello connection is delivered toohello application.</span></span> 

<span data-ttu-id="e3e96-131">Ha elleni hello helyi shard térkép hello érvényesítése sikertelen (Ez azt jelzi, hogy hello gyorsítótár nem megfelelő), hello Shard térkép Manager hello globális shard térkép tooobtain hello új helyes értéket fogja kérdezni hello kereséshez, frissítse hello gyorsítótárát, és szerezze be és hello vissza megfelelő adatbázis-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="e3e96-131">If hello validation against hello local shard map fails (indicating that hello cache is incorrect), hello Shard Map Manager will query hello global shard map tooobtain hello new correct value for hello lookup, update hello cache, and obtain and return hello appropriate database connection.</span></span> 

<span data-ttu-id="e3e96-132">Használjon **ConnectionOptions.None** csak amikor szilánkok leképezése módosítások esetén nem követelmény, míg az alkalmazás online.</span><span class="sxs-lookup"><span data-stu-id="e3e96-132">Use **ConnectionOptions.None** only when shard mapping changes are not expected while an application is online.</span></span> <span data-ttu-id="e3e96-133">Ebben az esetben feltételezhető, gyorsítótárazott hello értékek tooalways lehet helyes, és hello extra körbejárási érvényesítési hívás toohello céladatbázis biztonságosan kihagyja.</span><span class="sxs-lookup"><span data-stu-id="e3e96-133">In that case, hello cached values can be assumed tooalways be correct, and hello extra round-trip validation call toohello target database can be safely skipped.</span></span> <span data-ttu-id="e3e96-134">Amely csökkenti az adatbázis-forgalom.</span><span class="sxs-lookup"><span data-stu-id="e3e96-134">That reduces database traffic.</span></span> <span data-ttu-id="e3e96-135">Hello **connectionOptions** is lehet beállítani egy értéket a konfigurációs fájl tooindicate keresztül e horizontális módosítások várható vagy során egy adott időn belül nem.</span><span class="sxs-lookup"><span data-stu-id="e3e96-135">hello **connectionOptions** may also be set via a value in a configuration file tooindicate whether sharding changes are expected or not during a period of time.</span></span>  

<span data-ttu-id="e3e96-136">Ez a példa egy egész kulcs értékét hello **CustomerID**használatával egy **ShardMap** nevű objektum **customerShardMap**.</span><span class="sxs-lookup"><span data-stu-id="e3e96-136">This example uses hello value of an integer key **CustomerID**, using a **ShardMap** object named **customerShardMap**.</span></span>  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect toohello shard for that customer ID. No need toocall a SqlConnection 
    // constructor followed by hello Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

<span data-ttu-id="e3e96-137">Hello **OpenConnectionForKey** metódus visszaadja egy új toohello megfelelő adatbázis már nyitott kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="e3e96-137">hello **OpenConnectionForKey** method returns a new already-open connection toohello correct database.</span></span> <span data-ttu-id="e3e96-138">Ily módon be kapcsolatok továbbra is teljes körű kihasználása ADO.Net kapcsolatkészletezést.</span><span class="sxs-lookup"><span data-stu-id="e3e96-138">Connections utilized in this way still take full advantage of ADO.Net connection pooling.</span></span> <span data-ttu-id="e3e96-139">Mindaddig, amíg tranzakciók és a kérelmek is teljesíteni egy shard egyszerre, hello csak módosítása erre szükség van egy alkalmazás már használja az ADO.Net legyen.</span><span class="sxs-lookup"><span data-stu-id="e3e96-139">As long as transactions and requests can be satisfied by one shard at a time, this should be hello only modification necessary in an application already using ADO.Net.</span></span> 

<span data-ttu-id="e3e96-140">Hello  **[OpenConnectionForKeyAsync metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  érhetők el, ha az alkalmazás lehetővé teszi az aszinkron programozás használja az ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="e3e96-140">hello **[OpenConnectionForKeyAsync method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)** is also available if your application makes use asynchronous programming with ADO.Net.</span></span> <span data-ttu-id="e3e96-141">Az eszköz hello adatok függő útválasztási ADO megfelelő működése. NET tartozó  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metódust.</span><span class="sxs-lookup"><span data-stu-id="e3e96-141">Its behavior is hello data dependent routing equivalent of ADO.Net's **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)** method.</span></span>

## <a name="integrating-with-transient-fault-handling"></a><span data-ttu-id="e3e96-142">Átmeneti hiba kezelési integrálása</span><span class="sxs-lookup"><span data-stu-id="e3e96-142">Integrating with transient fault handling</span></span>
<span data-ttu-id="e3e96-143">Hello felhőben adatok alkalmazásokat fejleszt ajánlott tooensure átmeneti hello alkalmazás által észlelt, és hogy hello műveletek ismétlődnek több alkalommal hiba kiváltása előtt.</span><span class="sxs-lookup"><span data-stu-id="e3e96-143">A best practice in developing data access applications in hello cloud is tooensure that transient faults are caught by hello app, and that hello operations are retried several times before throwing an error.</span></span> <span data-ttu-id="e3e96-144">A tárgyalt kezelése a felhőalapú alkalmazások átmeneti hiba [átmeneti hiba kezelése](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span><span class="sxs-lookup"><span data-stu-id="e3e96-144">Transient fault handling for cloud applications is discussed at [Transient Fault Handling](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span></span> 

<span data-ttu-id="e3e96-145">Átmeneti hiba kezelési egyszerre is használható természetes hello adatok függő útválasztási mintával.</span><span class="sxs-lookup"><span data-stu-id="e3e96-145">Transient fault handling can coexist naturally with hello Data Dependent Routing pattern.</span></span> <span data-ttu-id="e3e96-146">hello kulcsfontosságú követelmény az tooretry hello teljes hozzáférési kérelem hello beleértve **használatával** lekért hello adatok függő útválasztási kapcsolat blokkolása.</span><span class="sxs-lookup"><span data-stu-id="e3e96-146">hello key requirement is tooretry hello entire data access request including hello **using** block that obtained hello data-dependent routing connection.</span></span> <span data-ttu-id="e3e96-147">a fenti hello példa sikerült kell írni, az alábbiak szerint (Megjegyzés: a kijelölt módosítása).</span><span class="sxs-lookup"><span data-stu-id="e3e96-147">hello example above could be rewritten as follows (note highlighted change).</span></span> 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a><span data-ttu-id="e3e96-148">Példa - adatok függő útválasztással átmeneti hiba kezelése</span><span class="sxs-lookup"><span data-stu-id="e3e96-148">Example - data dependent routing with transient fault handling</span></span>
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect toohello shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


<span data-ttu-id="e3e96-149">Szükséges tooimplement átmeneti hiba kezelési vannak csomagok automatikusan letöltött hello rugalmas adatbázis mintaalkalmazás összeállításakor.</span><span class="sxs-lookup"><span data-stu-id="e3e96-149">Packages necessary tooimplement transient fault handling are downloaded automatically when you build hello elastic database sample application.</span></span> <span data-ttu-id="e3e96-150">Csomagok is elérhetők külön, a [vállalati Library - átmeneti hiba kezelési alkalmazás blokk](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span><span class="sxs-lookup"><span data-stu-id="e3e96-150">Packages are also available separately at [Enterprise Library - Transient Fault Handling Application Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span></span> <span data-ttu-id="e3e96-151">6.0 vagy újabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="e3e96-151">Use version 6.0 or later.</span></span> 

## <a name="transactional-consistency"></a><span data-ttu-id="e3e96-152">Tranzakciós konzisztencia</span><span class="sxs-lookup"><span data-stu-id="e3e96-152">Transactional consistency</span></span>
<span data-ttu-id="e3e96-153">Az összes operations helyi tooa shard garantáltan tranzakciós tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="e3e96-153">Transactional properties are guaranteed for all operations local tooa shard.</span></span> <span data-ttu-id="e3e96-154">Például keresztül az adatok függő útválasztási tranzakciók hello kapcsolat hello hello cél shard hatókörén belül végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="e3e96-154">For example, transactions submitted through data-dependent routing execute within hello scope of hello target shard for hello connection.</span></span> <span data-ttu-id="e3e96-155">Jelenleg nincsenek történő besorolásakor több kapcsolatot egy tranzakcióban megadott képességek, és ezért nincsenek nem tranzakciós garanciák műveletekhez szilánkok keresztül zajlik.</span><span class="sxs-lookup"><span data-stu-id="e3e96-155">At this time, there are no capabilities provided for enlisting multiple connections into a transaction, and therefore there are no transactional guarantees for operations performed across shards.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3e96-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e3e96-156">Next steps</span></span>
<span data-ttu-id="e3e96-157">a shard toodetach vagy tooreattach a shard lásd [hello RecoveryManager osztály toofix shard térkép problémák használatával](sql-database-elastic-database-recovery-manager.md)</span><span class="sxs-lookup"><span data-stu-id="e3e96-157">toodetach a shard, or tooreattach a shard, see [Using hello RecoveryManager class toofix shard map problems](sql-database-elastic-database-recovery-manager.md)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

