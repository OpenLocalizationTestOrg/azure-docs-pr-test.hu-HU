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
# <a name="data-dependent-routing"></a>Adatfüggő útválasztás
**Adatok függő útválasztási** hello képességét toouse hello adat lekérdezés tooroute hello kérelem tooan megfelelő adatbázisban. Ez az alapvető mintát, az szilánkos adatbázisok használatakor. hello kérelemkörnyezet is lehet használt tooroute hello kérelem, különösen akkor, ha hello horizontális kulcs része nem hello lekérdezés. Minden adott lekérdezés vagy a tranzakció az adatok függő útválasztási használó alkalmazások nem korlátozott tooaccessing kérelmenként egy adatbázist. Hello Azure SQL Database rugalmas eszközök, az Útválasztás valósítható meg a hello  **[ShardMapManager osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  ADO.NET-alkalmazásokban.

hello alkalmazást nem kell tootrack különböző kapcsolati karakterláncok vagy más szeletek hello szilánkos környezetben adatok társított DB helyek. Ehelyett hello [Shard térkép Manager](sql-database-elastic-scale-shard-map-management.md) megnyílik kapcsolatok toohello megfelelő adatbázisok szükség esetén hello shard és hello horizontális kulcs, amely hello cél hello alkalmazás kérelem hello értékének hello adatai alapján. hello kulcsa általában hello *customer_id*, *tenant_id*, *date_key*, és néhány egyéb egyedi azonosító, amelyet hello adatbázis kérelem alapvető paraméter). 

További információkért lásd: [skálázás, SQL Server Data függő útválasztási](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-hello-client-library"></a>Töltse le a hello ügyféloldali kódtár
tooget hello osztály telepítés hello [Elastic Database ügyféloldali kódtár](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Egy ShardMapManager használata adatok függő útválasztási alkalmazásokban
Alkalmazások kell példányosítani hello **ShardMapManager** hello gyári hívás használatával az inicializálás során  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**. Ebben a példában mind a **ShardMapManager** és egy adott **ShardMap** benne foglalt inicializálása. Ez a példa azt mutatja be hello GetSqlShardMapManager és [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) módszerek.

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-hello-shard-map"></a>Hello shard leképezés első legkisebb lehetséges jogosultságokat biztosító hitelesítő adatok használata
Ha egy alkalmazás nem van kezelésére hello shard térkép magát, hello gyári metódusban használt hello hitelesítő adatok csak olvasási engedélyekkel kell rendelkezniük a hello **globális Shard térkép** adatbázis. Ezek a hitelesítő adatok általában eltérnek a használt hitelesítő adatok tooopen kapcsolatok toohello shard térkép manager. Lásd még: [a hitelesítő adatokat használni tooaccess hello Elastic Database ügyféloldali kódtárának](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-hello-openconnectionforkey-method"></a>Hello OpenConnectionForKey metódus hívása
Hello  **[ShardMap.OpenConnectionForKey metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  adja vissza egy ADO.Net készen parancsok toohello megfelelő adatbázis hello hello értéke alapján kiállító **kulcs**paraméter. A shard adatbázisban tárolja a hello alkalmazás által hello **ShardMapManager**, így ezek a kérelmek jellemzően nem foglalják magukban egy adatbázis-lekérdezés alapján hello **globális Shard térkép** adatbázis. 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* Hello **kulcs** paraméter keresési kulcsként adatbázisba hello shard térkép toodetermine hello megfelelő hello kéréshez használt. 
* Hello **connectionString** használt toopass csak hello felhasználói hitelesítő adatok hello szükség van. Nincs adatbázis vagy a kiszolgáló neve szerepelnek a *connectionString* óta hello módszer határozza meg, hogy hello adatbázis és a kiszolgáló hello **ShardMap**. 
* Hello  **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  túl meg kell**ConnectionOptions.Validate** Ha egy olyan környezetben, ahol a shard leképezi módosíthatja, és a sorok tooother adatbázisok, előfordulhat, hogy áthelyezése egy vegyes vagy az egyesítési művelet eredménye. Ebbe beletartozik egy rövid lekérdezés toohello helyi shard térkép hello céladatbázis (toohello globális shard társítani) hello kapcsolat előtt a rendszer toohello alkalmazás. 

Ha elleni hello helyi shard térkép hello érvényesítése sikertelen (Ez azt jelzi, hogy hello gyorsítótár nem megfelelő), hello Shard térkép Manager hello globális shard térkép tooobtain hello új helyes értéket fogja kérdezni hello kereséshez, frissítse hello gyorsítótárát, és szerezze be és hello vissza megfelelő adatbázis-kapcsolatot. 

Használjon **ConnectionOptions.None** csak amikor szilánkok leképezése módosítások esetén nem követelmény, míg az alkalmazás online. Ebben az esetben feltételezhető, gyorsítótárazott hello értékek tooalways lehet helyes, és hello extra körbejárási érvényesítési hívás toohello céladatbázis biztonságosan kihagyja. Amely csökkenti az adatbázis-forgalom. Hello **connectionOptions** is lehet beállítani egy értéket a konfigurációs fájl tooindicate keresztül e horizontális módosítások várható vagy során egy adott időn belül nem.  

Ez a példa egy egész kulcs értékét hello **CustomerID**használatával egy **ShardMap** nevű objektum **customerShardMap**.  

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

Hello **OpenConnectionForKey** metódus visszaadja egy új toohello megfelelő adatbázis már nyitott kapcsolat. Ily módon be kapcsolatok továbbra is teljes körű kihasználása ADO.Net kapcsolatkészletezést. Mindaddig, amíg tranzakciók és a kérelmek is teljesíteni egy shard egyszerre, hello csak módosítása erre szükség van egy alkalmazás már használja az ADO.Net legyen. 

Hello  **[OpenConnectionForKeyAsync metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  érhetők el, ha az alkalmazás lehetővé teszi az aszinkron programozás használja az ADO.Net. Az eszköz hello adatok függő útválasztási ADO megfelelő működése. NET tartozó  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metódust.

## <a name="integrating-with-transient-fault-handling"></a>Átmeneti hiba kezelési integrálása
Hello felhőben adatok alkalmazásokat fejleszt ajánlott tooensure átmeneti hello alkalmazás által észlelt, és hogy hello műveletek ismétlődnek több alkalommal hiba kiváltása előtt. A tárgyalt kezelése a felhőalapú alkalmazások átmeneti hiba [átmeneti hiba kezelése](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx). 

Átmeneti hiba kezelési egyszerre is használható természetes hello adatok függő útválasztási mintával. hello kulcsfontosságú követelmény az tooretry hello teljes hozzáférési kérelem hello beleértve **használatával** lekért hello adatok függő útválasztási kapcsolat blokkolása. a fenti hello példa sikerült kell írni, az alábbiak szerint (Megjegyzés: a kijelölt módosítása). 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Példa - adatok függő útválasztással átmeneti hiba kezelése
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


Szükséges tooimplement átmeneti hiba kezelési vannak csomagok automatikusan letöltött hello rugalmas adatbázis mintaalkalmazás összeállításakor. Csomagok is elérhetők külön, a [vállalati Library - átmeneti hiba kezelési alkalmazás blokk](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/). 6.0 vagy újabb verzióját használja. 

## <a name="transactional-consistency"></a>Tranzakciós konzisztencia
Az összes operations helyi tooa shard garantáltan tranzakciós tulajdonságai. Például keresztül az adatok függő útválasztási tranzakciók hello kapcsolat hello hello cél shard hatókörén belül végrehajtani. Jelenleg nincsenek történő besorolásakor több kapcsolatot egy tranzakcióban megadott képességek, és ezért nincsenek nem tranzakciós garanciák műveletekhez szilánkok keresztül zajlik.

## <a name="next-steps"></a>Következő lépések
a shard toodetach vagy tooreattach a shard lásd [hello RecoveryManager osztály toofix shard térkép problémák használatával](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

