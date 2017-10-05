---
title: "Függő az Azure SQL Database útválasztási adatok |} Microsoft Docs"
description: "A ShardMapManager osztály használata a .NET-alkalmazásokban az adatok függő útválasztási, szilánkos adatbázisokat az Azure SQL-adatbázis szolgáltatása"
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
ms.openlocfilehash: 6b68bbb0133afd1493acdb58f79f3eeaf6a8d7cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="data-dependent-routing"></a><span data-ttu-id="b7aa5-103">Adatfüggő útválasztás</span><span class="sxs-lookup"><span data-stu-id="b7aa5-103">Data dependent routing</span></span>
<span data-ttu-id="b7aa5-104">**Adatok függő útválasztási** képessége, hogy a lekérdezés az adatok használatával továbbítja a kérelmet a megfelelő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-104">**Data dependent routing** is the ability to use the data in a query to route the request to an appropriate database.</span></span> <span data-ttu-id="b7aa5-105">Ez az alapvető mintát, az szilánkos adatbázisok használatakor.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-105">This is a fundamental pattern when working with sharded databases.</span></span> <span data-ttu-id="b7aa5-106">A kérés környezete is használható a kérelem továbbításához, különösen akkor, ha a horizontális kulcs része nem a lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-106">The request context may also be used to route the request, especially if the sharding key is not part of the query.</span></span> <span data-ttu-id="b7aa5-107">Minden egyedi lekérdezés vagy a tranzakció az adatok függő útválasztási használó alkalmazások elérése egy önálló adatbázis kérelmenként korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-107">Each specific query or transaction in an application using data dependent routing is restricted to accessing a single database per request.</span></span> <span data-ttu-id="b7aa5-108">Az Azure SQL Database rugalmas eszközök, az Útválasztás segítségével történik a  **[ShardMapManager osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  ADO.NET-alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-108">For the Azure SQL Database Elastic tools, this routing is accomplished with the **[ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)** in ADO.NET applications.</span></span>

<span data-ttu-id="b7aa5-109">Az alkalmazás nem kell különböző kapcsolati karakterláncok és az adatok a szilánkos környezetben más szeletek társított DB helyek követheti nyomon.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-109">The application does not need to track various connection strings or DB locations associated with different slices of data in the sharded environment.</span></span> <span data-ttu-id="b7aa5-110">Ehelyett a [Shard térkép Manager](sql-database-elastic-scale-shard-map-management.md) a helyes adatbázis szükség esetén megnyílik-kapcsolatok a shard és az alkalmazás kérelem céljaként a horizontális kulcsnak az értéke adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-110">Instead, the [Shard Map Manager](sql-database-elastic-scale-shard-map-management.md) opens connections to the correct databases when needed, based on the data in the shard map and the value of the sharding key that is the target of the application’s request.</span></span> <span data-ttu-id="b7aa5-111">A kulcs van általában a *customer_id*, *tenant_id*, *date_key*, vagy az adatbázis-kérelem alapvető paraméter néhány más egyedi azonosító).</span><span class="sxs-lookup"><span data-stu-id="b7aa5-111">The key is typically the *customer_id*, *tenant_id*, *date_key*, or some other specific identifier that is a fundamental parameter of the database request).</span></span> 

<span data-ttu-id="b7aa5-112">További információkért lásd: [skálázás, SQL Server Data függő útválasztási](https://technet.microsoft.com/library/cc966448.aspx).</span><span class="sxs-lookup"><span data-stu-id="b7aa5-112">For more information, see [Scaling Out SQL Server with Data Dependent Routing](https://technet.microsoft.com/library/cc966448.aspx).</span></span>

## <a name="download-the-client-library"></a><span data-ttu-id="b7aa5-113">Töltse le az ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="b7aa5-113">Download the client library</span></span>
<span data-ttu-id="b7aa5-114">Ahhoz, hogy az osztály, telepítse a [Elastic Database ügyféloldali kódtár](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="b7aa5-114">To get the class, install the [Elastic Database Client Library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span> 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a><span data-ttu-id="b7aa5-115">Egy ShardMapManager használata adatok függő útválasztási alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="b7aa5-115">Using a ShardMapManager in a data dependent routing application</span></span>
<span data-ttu-id="b7aa5-116">Alkalmazások példányt kell létrehozni a **ShardMapManager** az inicializálás során használja a gyári hívás  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-116">Applications should instantiate the **ShardMapManager** during initialization, using the factory call **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span></span> <span data-ttu-id="b7aa5-117">Ebben a példában mind a **ShardMapManager** és egy adott **ShardMap** benne foglalt inicializálása.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-117">In this example, both a **ShardMapManager** and a specific **ShardMap** that it contains are initialized.</span></span> <span data-ttu-id="b7aa5-118">Ez a példa bemutatja a GetSqlShardMapManager és [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) módszerek.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-118">This example shows the GetSqlShardMapManager and [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) methods.</span></span>

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a><span data-ttu-id="b7aa5-119">A szilánkok leképezés első legkisebb lehetséges jogosultságokat biztosító hitelesítő adatok használata</span><span class="sxs-lookup"><span data-stu-id="b7aa5-119">Use lowest privilege credentials possible for getting the shard map</span></span>
<span data-ttu-id="b7aa5-120">Ha egy alkalmazás nem van kezelésére a shard térkép magát, a gyári metódusban használt hitelesítő adatok csak olvasási engedélyekkel kell rendelkezniük a **globális Shard térkép** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-120">If an application is not manipulating the shard map itself, the credentials used in the factory method should have just read-only permissions on the **Global Shard Map** database.</span></span> <span data-ttu-id="b7aa5-121">Ezek a hitelesítő adatok általában eltérnek a shard térkép Manager kapcsolatok megnyitásához használt hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-121">These credentials are typically different from credentials used to open connections to the shard map manager.</span></span> <span data-ttu-id="b7aa5-122">Lásd még: [az Elastic Database ügyféloldali kódtár eléréséhez használt hitelesítő adatok](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="b7aa5-122">See also [Credentials used to access the Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span> 

## <a name="call-the-openconnectionforkey-method"></a><span data-ttu-id="b7aa5-123">A OpenConnectionForKey metódus hívása</span><span class="sxs-lookup"><span data-stu-id="b7aa5-123">Call the OpenConnectionForKey method</span></span>
<span data-ttu-id="b7aa5-124">A  **[ShardMap.OpenConnectionForKey metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  adja vissza egy ADO.Net készen áll a parancsok kiadása a megfelelő adatbázisba értéke alapján a **kulcs** a paraméter.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-124">The **[ShardMap.OpenConnectionForKey method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)** returns an ADO.Net connection ready for issuing commands to the appropriate database based on the value of the **key** parameter.</span></span> <span data-ttu-id="b7aa5-125">A shard információkat tárolja a rendszer az alkalmazás által a **ShardMapManager**, így ezek a kérelmek általában nem tartalmaznak egy adatbázis-lekérdezés alapján a **globális Shard térkép** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-125">Shard information is cached in the application by the **ShardMapManager**, so these requests do not typically involve a database lookup against the **Global Shard Map** database.</span></span> 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* <span data-ttu-id="b7aa5-126">A **kulcs** paraméterrel keresési kulcsként azokat a shard térkép határozza meg a megfelelő adatbázis a kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-126">The **key** parameter is used as a lookup key into the shard map to determine the appropriate database for the request.</span></span> 
* <span data-ttu-id="b7aa5-127">A **connectionString** csak a felhasználói hitelesítő adatok továbbítása a kívánt kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-127">The **connectionString** is used to pass only the user credentials for the desired connection.</span></span> <span data-ttu-id="b7aa5-128">Nincs adatbázis vagy a kiszolgáló neve szerepelnek a *connectionString* óta metódus határozza meg, hogy az adatbázis és a kiszolgáló használja a **ShardMap**.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-128">No database name or server name are included in this *connectionString* since the method will determine the database and server using the **ShardMap**.</span></span> 
* <span data-ttu-id="b7aa5-129">A  **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  meg **ConnectionOptions.Validate** egy olyan környezetben, ahol a shard leképezi módosíthatja, ha a sorok más adatbázisok következtében előfordulhat, hogy áthelyezése megosztott vagy egyesítési műveletek.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-129">The **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)** should be set to **ConnectionOptions.Validate** if an environment where shard maps may change and rows may move to other databases as a result of split or merge operations.</span></span> <span data-ttu-id="b7aa5-130">Ebbe beletartozik a helyi shard leképezés a cél rövid lekérdezés adatbázist (nem a globális shard térkép) előtt a kapcsolatot a rendszer az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-130">This involves a brief query to the local shard map on the target database (not to the global shard map) before the connection is delivered to the application.</span></span> 

<span data-ttu-id="b7aa5-131">Ha a helyi shard térkép elvégzett sémaellenőrzésen sikertelen (Ez azt jelzi, hogy a gyorsítótár nem megfelelő), a Shard térkép Manager le fogja kérdezni a globális shard térkép szerezze be az új megfelelő értéket a keresési frissíteni a gyorsítótárat, és szerezze be és térjen vissza a megfelelő adatbázis a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-131">If the validation against the local shard map fails (indicating that the cache is incorrect), the Shard Map Manager will query the global shard map to obtain the new correct value for the lookup, update the cache, and obtain and return the appropriate database connection.</span></span> 

<span data-ttu-id="b7aa5-132">Használjon **ConnectionOptions.None** csak amikor szilánkok leképezése módosítások esetén nem követelmény, míg az alkalmazás online.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-132">Use **ConnectionOptions.None** only when shard mapping changes are not expected while an application is online.</span></span> <span data-ttu-id="b7aa5-133">Ebben az esetben a gyorsítótárazott értékeket is feltételezhető, hogy mindig a megfelelő lehet, és a céladatbázis extra körbejárási érvényesítési hívása biztonságosan kihagyja.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-133">In that case, the cached values can be assumed to always be correct, and the extra round-trip validation call to the target database can be safely skipped.</span></span> <span data-ttu-id="b7aa5-134">Amely csökkenti az adatbázis-forgalom.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-134">That reduces database traffic.</span></span> <span data-ttu-id="b7aa5-135">A **connectionOptions** is beállítható, keresztül értéket annak jelzésére, hogy horizontális módosítások várható a konfigurációs fájlban vagy egy adott időn belül során nem.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-135">The **connectionOptions** may also be set via a value in a configuration file to indicate whether sharding changes are expected or not during a period of time.</span></span>  

<span data-ttu-id="b7aa5-136">Ez a példa egy egész kulcsnak az értéke **CustomerID**használatával egy **ShardMap** nevű objektum **customerShardMap**.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-136">This example uses the value of an integer key **CustomerID**, using a **ShardMap** object named **customerShardMap**.</span></span>  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect to the shard for that customer ID. No need to call a SqlConnection 
    // constructor followed by the Open method.
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

<span data-ttu-id="b7aa5-137">A **OpenConnectionForKey** metódus visszaadja az új kapcsolat már nyitva a megfelelő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-137">The **OpenConnectionForKey** method returns a new already-open connection to the correct database.</span></span> <span data-ttu-id="b7aa5-138">Ily módon be kapcsolatok továbbra is teljes körű kihasználása ADO.Net kapcsolatkészletezést.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-138">Connections utilized in this way still take full advantage of ADO.Net connection pooling.</span></span> <span data-ttu-id="b7aa5-139">Mindaddig, amíg tranzakciók és a kérelmek lehet teljesíteni egy shard által egy időben, a szükséges egy alkalmazás már használja az ADO.Net csak módosítás legyen.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-139">As long as transactions and requests can be satisfied by one shard at a time, this should be the only modification necessary in an application already using ADO.Net.</span></span> 

<span data-ttu-id="b7aa5-140">A  **[OpenConnectionForKeyAsync metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  érhetők el, ha az alkalmazás lehetővé teszi az aszinkron programozás használja az ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-140">The **[OpenConnectionForKeyAsync method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)** is also available if your application makes use asynchronous programming with ADO.Net.</span></span> <span data-ttu-id="b7aa5-141">A viselkedését az függő útválasztási ADO egyenrangúként adatai. NET tartozó  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metódust.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-141">Its behavior is the data dependent routing equivalent of ADO.Net's **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)** method.</span></span>

## <a name="integrating-with-transient-fault-handling"></a><span data-ttu-id="b7aa5-142">Átmeneti hiba kezelési integrálása</span><span class="sxs-lookup"><span data-stu-id="b7aa5-142">Integrating with transient fault handling</span></span>
<span data-ttu-id="b7aa5-143">Adatok felhőalapú alkalmazásokat fejleszt ajánlott annak biztosítására, hogy átmeneti hibák az alkalmazás által észlelt, és, hogy a műveletek előtt hibát jelez a többször van újra.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-143">A best practice in developing data access applications in the cloud is to ensure that transient faults are caught by the app, and that the operations are retried several times before throwing an error.</span></span> <span data-ttu-id="b7aa5-144">A tárgyalt kezelése a felhőalapú alkalmazások átmeneti hiba [átmeneti hiba kezelése](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span><span class="sxs-lookup"><span data-stu-id="b7aa5-144">Transient fault handling for cloud applications is discussed at [Transient Fault Handling](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span></span> 

<span data-ttu-id="b7aa5-145">Átmeneti hiba kezelési egyszerre is használható természetesen az adatok függő útválasztási mintával.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-145">Transient fault handling can coexist naturally with the Data Dependent Routing pattern.</span></span> <span data-ttu-id="b7aa5-146">A kulcs mérete, majd ismételje meg a teljes hozzáférés kérelem, például a **használatával** lekért adatok függő útválasztási kapcsolat blokkolása.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-146">The key requirement is to retry the entire data access request including the **using** block that obtained the data-dependent routing connection.</span></span> <span data-ttu-id="b7aa5-147">(Megjegyzés: a kijelölt módosítása) az alábbi módon sikerült kell írni a fenti példa.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-147">The example above could be rewritten as follows (note highlighted change).</span></span> 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a><span data-ttu-id="b7aa5-148">Példa - adatok függő útválasztással átmeneti hiba kezelése</span><span class="sxs-lookup"><span data-stu-id="b7aa5-148">Example - data dependent routing with transient fault handling</span></span>
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect to the shard for a customer ID. 
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


<span data-ttu-id="b7aa5-149">A rugalmas adatbázis-minta alkalmazás építésekor átmeneti hiba kezelési végrehajtásához szükséges csomagok automatikusan letöltődnek.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-149">Packages necessary to implement transient fault handling are downloaded automatically when you build the elastic database sample application.</span></span> <span data-ttu-id="b7aa5-150">Csomagok is elérhetők külön, a [vállalati Library - átmeneti hiba kezelési alkalmazás blokk](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span><span class="sxs-lookup"><span data-stu-id="b7aa5-150">Packages are also available separately at [Enterprise Library - Transient Fault Handling Application Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span></span> <span data-ttu-id="b7aa5-151">6.0 vagy újabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-151">Use version 6.0 or later.</span></span> 

## <a name="transactional-consistency"></a><span data-ttu-id="b7aa5-152">Tranzakciós konzisztencia</span><span class="sxs-lookup"><span data-stu-id="b7aa5-152">Transactional consistency</span></span>
<span data-ttu-id="b7aa5-153">Tranzakciós tulajdonságai garantáltan minden műveletnél helyi, a szilánkcímtárban.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-153">Transactional properties are guaranteed for all operations local to a shard.</span></span> <span data-ttu-id="b7aa5-154">Például tranzakciók adatok függő útválasztási keresztül hajtható végre, a kapcsolat a cél shard hatókörén belül.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-154">For example, transactions submitted through data-dependent routing execute within the scope of the target shard for the connection.</span></span> <span data-ttu-id="b7aa5-155">Jelenleg nincsenek történő besorolásakor több kapcsolatot egy tranzakcióban megadott képességek, és ezért nincsenek nem tranzakciós garanciák műveletekhez szilánkok keresztül zajlik.</span><span class="sxs-lookup"><span data-stu-id="b7aa5-155">At this time, there are no capabilities provided for enlisting multiple connections into a transaction, and therefore there are no transactional guarantees for operations performed across shards.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7aa5-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7aa5-156">Next steps</span></span>
<span data-ttu-id="b7aa5-157">Válassza le a shard, vagy egy shard újracsatolni [shard térkép problémák elhárításának a RecoveryManager osztály használatával](sql-database-elastic-database-recovery-manager.md)</span><span class="sxs-lookup"><span data-stu-id="b7aa5-157">To detach a shard, or to reattach a shard, see [Using the RecoveryManager class to fix shard map problems](sql-database-elastic-database-recovery-manager.md)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

