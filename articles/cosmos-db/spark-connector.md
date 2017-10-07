---
title: aaaConnecting Apache Spark tooAzure Cosmos DB |} Microsoft Docs
description: "Használja az oktatóanyag toolearn, amely lehetővé teszi tooconnect Apache Spark tooAzure Cosmos DB elosztott tooperform összesítéseinek és a Microsoft hello több-bérlős globálisan elosztott adatbázis rendszeren adatok sciences hello Azure Cosmos DB Spark connector névjegye amely a hello felhő szolgál."
keywords: az Apache Spark on
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: c4f46007-2606-4273-ab16-29d0e15c0736
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: denlee
ms.openlocfilehash: 70b496fc5ca8f65675f0224e749637f5d533c346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accelerate-real-time-big-data-analytics-with-hello-spark-tooazure-cosmos-db-connector"></a>Hello Spark tooAzure Cosmos DB Connector valós idejű big data elemzések érdekében

hello Spark tooAzure Cosmos DB összekötő lehetővé teszi, hogy a bemeneti forrás vagy kimeneti fogadóját feladatok az Apache Spark on Azure Cosmos DB tooact. Csatlakozás [Spark](http://spark.apache.org/) túl[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) felgyorsítják a képes toosolve gyorsan változó adattudomány használható Azure Cosmos DB tooquickly problémák továbbra is fennáll, és adatokat lekérdezni. hello Spark tooAzure Cosmos DB összekötő hatékonyan hello natív Azure Cosmos DB felügyelt indexek használja. hello indexek frissíthető oszlopok lehetővé teszik, amikor analytics elvégezhető, és leküldéses-le nyilas predikátum fast-módosítása globálisan szűrése adatait, amely az eszközök internetes hálózatát (IoT) toodata tudományos és elemzés forgatókönyvek között.

Spark GraphX és hello Gremlin graph API-kat az Azure Cosmos DB használata, lásd: [Spark és Apache TinkerPop Gremlin graph időben](spark-connector-graph.md).

## <a name="download"></a>Letöltés

tooget elindult, hello Spark tooAzure Cosmos DB connector (előzetes verzió) letöltése hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) GitHub tárházából.

## <a name="connector-components"></a>Összekötő-összetevők

hello összekötő hello a következő összetevőket használja:

* [Az Azure Cosmos DB](http://documentdb.com) lehetővé teszi, hogy az ügyfelek tooprovision és rugalmasan méretezhető átviteli sebesség és a tárolási tetszőleges számú földrajzi régiók között. hello szolgáltatást kínál:
   * Kapcsolja be a kulcs [globális terjesztési](distribute-data-globally.md) és horizontális skálázhatóságot
   * Garantált egyetlen számjegy késések hello 99th PERCENTILIS:
   * [Több jól meghatározott konzisztencia modellek](consistency-levels.md)
   * Magas rendelkezésre állású többhelyű képességekkel garantált
   * Minden szolgáltatás üzemelnek iparágvezető, átfogó [szolgáltatási szintek](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).

* [Az Apache Spark on](http://spark.apache.org/) egy hatékony nyílt forráskódú feldolgozási motor, amely a sebesség, valamint a kifinomult analytics könnyű épül.

* [Apache Spark on Azure Hdinsighttal](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) , hogy az Apache Spark on hello felhő kritikus telepítések segítségével telepíthet [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).

Hivatalos támogatott verziók:

| Összetevő | Verzió |
|---------|-------|
|Apache Spark|2.0+|
| Scala| 2.11|
| Az Azure DocumentDB Java SDK | 1.10.0 |

Ez a cikk segítséget nyújt a futtatásával néhány egyszerű minták Python (a pyDocumentDB) keresztül, és hello Scala felületek.

Két megközelítés tooconnect Apache Spark és Azure Cosmos DB vannak:
- Használja a pyDocumentDB keresztül hello [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).
- Hozzon létre egy Java-alapú Spark tooAzure Cosmos DB összekötőt hello felügyelniük [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).

## <a name="pydocumentdb-implementation"></a>pyDocumentDB végrehajtása
aktuális hello [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) lehetővé teszi a tooconnect Spark tooAzure Cosmos DB a hello a következő ábrán látható módon:

![Spark tooAzure Cosmos DB adatfolyam a pyDocumentDB DB keresztül](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-hello-pydocumentdb-implementation"></a>Hello pyDocumentDB megvalósítási adatfolyama

hello adatáramlás a következőképpen történik:

1. hello Spark főcsomópont toohello Azure Cosmos DB átjáró csomópont pyDocumentDB keresztül kapcsolódik. A felhasználó csak hello Spark és Azure Cosmos DB-kapcsolatok ad meg. Kapcsolatok toohello megfelelő master és átjáró-csomópontok átlátszó toohello felhasználókként szerepelnek.
2. hello átjárócsomópont teszi hello Azure Cosmos DB hello lekérdezés ezt követően futtató hello adatcsomópontokat hello gyűjtemény a partíciók ellen irányuló lekérdezés esetén. hello választ a lekérdezések akkor küldi vissza toohello átjárócsomópontnak, és adott eredménykészlet toohello Spark főcsomópont ad vissza.
3. További lekérdezéseket (például egy Spark DataFrame) toohello Spark munkavégző csomópontokhoz feldolgozásra kerülnek.

Spark és Azure Cosmos DB közötti kommunikáció korlátozott toohello Spark főcsomópont és Azure Cosmos DB átjárócsomópontok.  hello lekérdezések gyors hello transport layer a két csomópont között lehetővé teszi, hogy nyissa meg.

### <a name="install-pydocumentdb"></a>Telepítse a pyDocumentDB
Telepíthető pyDocumentDB az illesztőprogram-csomópont használatával **pip**, például:

```
pip install pyDocumentDB
```


### <a name="connect-spark-tooazure-cosmos-db-via-pydocumentdb"></a>Csatlakozás a Spark tooAzure Cosmos DB pyDocumentDB keresztül
hello kommunikációs átviteli hello jellemzője az egyszerűség viszonylag egyszerű pyDocumentDB használatával lehetővé teszi a Spark tooAzure Cosmos DB a lekérdezés végrehajtása.

a következő kódrészletben látható kód hogyan hello toouse pyDocumentDB Spark környezetben.

```
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring hello connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]


# Set keys tooconnect tooAzure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

Hello kódrészletet leírtaknak megfelelően:

* hello Azure Cosmos DB Python SDK-t (`pyDocumentDB`) hello tartalmazza az összes szükséges kapcsolódási paraméterek hello. Például hello elsődleges helyek paraméter úgy dönt, hogy hello olvassa el a replika és a prioritási sorrendben.
*  Hello szükséges kódtárak importálja és konfigurálja a **főkulcsos** és **állomás** toocreate hello Azure Cosmos DB *ügyfél* (**pydocumentdb.document_ ügyfél**).


### <a name="execute-spark-queries-via-pydocumentdb"></a>A Spark-lekérdezéseket hajt végre a pyDocumentDB keresztül
a következő példák használata hello Azure Cosmos DB példány hello előző részlet hello segítségével létrehozott hello megadott írásvédett kulcsok. hello következő kódrészletet csatlakozik toohello **airports.codes** gyűjtemény hello DoctorWho fiók, mint a korábban meghatározott, és futtat egy lekérdezés tooextract hello repülőtéren városokat Washington államban.

```
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations hello Azure Cosmos DB client will use tooconnect toohello database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Push into list `elements`
elements = list(query)
```

Hello lekérdezés keresztül végrehajtása után **lekérdezés**, hello eredménye egy **query_iterable. QueryIterable** , amely a konvertált tooa Python listája. A Python lista lehet könnyen konvertált tooa Spark DataFrame hello kód a következő használatával:

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-hello-pydocumentdb-tooconnect-spark-tooazure-cosmos-db"></a>Hello pyDocumentDB tooconnect Spark tooAzure Cosmos DB miért érdemes használni?
Csatlakozás Spark tooAzure Cosmos DB pyDocumentDB használatával általában forgatókönyvek esetén az adott:

* Azt szeretné, hogy a toouse Python.
* Az Azure Cosmos DB tooSpark viszonylag kis eredményhalmazt vannak ad vissza. Vegye figyelembe, hogy az Azure Cosmos Adatbázisba az alapul szolgáló dataset hello Igen tekintélyes lehet. Szűrők, ez azt jelenti, hogy az Azure Cosmos DB adatforrás predikátum szűrők futtatott alkalmazzák.  

## <a name="spark-tooazure-cosmos-db-connector"></a>Spark tooAzure Cosmos DB összekötő

hello Spark tooAzure Cosmos DB összekötőt használja hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) és mozgatja az adatokat hello Spark munkavégző csomópontokhoz és Azure Cosmos DB között, ahogy az ábra a következő hello:

![Hello Spark tooAzure Cosmos DB Connector adatfolyama](./media/spark-connector/spark-connector.png)

hello adatáramlás a következőképpen történik:

1. hello Spark főcsomópont toohello Azure Cosmos DB átjáró csomópont tooobtain hello partíciótérképen csatlakozik. A felhasználó csak hello Spark és Azure Cosmos DB-kapcsolatok ad meg. Kapcsolatok toohello megfelelő master és átjáró-csomópontok átlátszó toohello felhasználókként szerepelnek.
2. Ez az információ hátsó toohello Spark főcsomópont valósul meg.  Ezen a ponton képes tooparse hello lekérdezési toodetermine hello partíciók és az Azure Cosmos-Adatbázisba, hogy kell-e tooaccess helyükre kell lennie.
3. Ez az információ az átvitt toohello Spark munkavégző csomópontokhoz.
4. hello Spark munkavégző csomópontok toohello Azure Cosmos DB partíciók csatlakoznak, közvetlen tooextract hello adatokat, és vissza hello adatok toohello Spark partíciók hello Spark munkavégző csomópontokhoz.

Spark és Azure Cosmos DB közötti kommunikáció oka jelentősen gyorsabb adatátvitelt jelölik a hello hello Spark munkavégző csomópontokhoz és hello Azure Cosmos DB adatok csomópontok (partíciók) között.

### <a name="build-hello-spark-tooazure-cosmos-db-connector"></a>Hello Spark tooAzure Cosmos DB összekötő létrehozása
Jelenleg a hello összekötő projektet maven használja. toobuild hello összekötő függőségek nélkül is futtathatja:
```
mvn clean package
```
Hello hello JAR legújabb verzióját a hello is letölthető *kiadott* mappát.

### <a name="include-hello-azure-cosmos-db-spark-jar"></a>Hello Azure Cosmos DB külső JAR tartalmazza
Kód futtatása előtt kell tooinclude hello Azure Cosmos DB külső JAR.  Hello használata **spark-rendszerhéj**, majd hello segítségével megadhat hello JAR **--JAR-fájlok kivételével** lehetőséget.  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

Tooexecute hello JAR függőségek nélkül, használja a következő kód hello:

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

A notebook szolgáltatással, például az Azure HDInsight Jupyter notebook szolgáltatás használatakor hello használhatja **spark magic** parancsokat:

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

Hello **JAR-fájlok kivételével** parancs lehetővé teszi, hogy tooinclude hello két JARs, amelyek szükségesek a **azure-cosmosdb-spark** (saját magát és hello Azure DocumentDB Java SDK-t), és amelyeket **scala-megfelelően** , hogy ne zavarja Livy meghívja hello (Jupyter notebook > Livy > Spark).

### <a name="connect-spark-tooazure-cosmos-db-using-hello-connector"></a>Csatlakozás a Spark tooAzure Cosmos DB használatával hello összekötő
Bár hello kommunikációs átviteli egy kicsit bonyolultabb, a lekérdezés végrehajtása a Spark tooAzure Cosmos DB hello-összekötő használatával is jelentősen gyorsabb.

a következő kódrészletet hello bemutatja, hogyan toouse hello összekötő Spark környezetben.

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection tooyour collection
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Hello kódrészletet leírtaknak megfelelően:

- **Azure-cosmosdb-spark** hello tartalmazza az összes szükséges kapcsolódási paraméterek, többek között hello elsődleges helyek hello. Kiválaszthatja például hello olvassa el a replika és a prioritási sorrendben.
- Ebben az esetben hello szükséges könyvtárak importálása, és a főkulcsos és a gazdagép toocreate hello Azure Cosmos DB-ügyfél konfigurálása.

### <a name="execute-spark-queries-via-hello-connector"></a>Hajtsa végre a Spark lekérdezéseket hello összekötőn keresztül

a következő példa használ hello Azure Cosmos DB példány hello előző részlet hello segítségével létrehozott hello megadott írásvédett kulcsok. hello következő kódrészletet toohello DepartureDelays.flights_pcoll gyűjtemény (a korábban megadott DoctorWho fiók hello) csatlakozik, és futtat egy lekérdezés tooextract hello repülési késési információkat, amelyek a budapesti vannak induló légijáratok.

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-hello-spark-tooazure-cosmos-db-connector-implementation"></a>Hello Spark tooAzure Cosmos DB összekötő megvalósítási miért érdemes használni?

Csatlakozás Spark tooAzure Cosmos DB hello összekötővel általában forgatókönyvek van ahol:

* Szeretné, hogy toouse Scala és tooinclude egy Python-burkoló leírtaknak megfelelően a frissítést [probléma 3: hozzáadása Python burkoló és példák](https://github.com/Azure/azure-cosmosdb-spark/issues/3).
* Nagy mennyiségű adatok tootransfer között az Apache Spark on Azure Cosmos DB rendelkezik.

toogive egy meghatározni, hello lekérdezés teljesítménybeli különbség az, hogy látja hello [lekérdezés teszt futtatása wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="distributed-aggregation-example"></a>Elosztott összesítési – példa
Ez a témakör néhány példa arra, hogyan lehet úgy teheti meg, elosztott összesítések és az elemzések Apache Spark- és Azure Cosmos DB együtt. Azure Cosmos-adatbázis már támogatja az összesítéseket, amely hello ismertet [bolygónk méretezési összesíti az Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/). Ez hogyan-EK toohello mellett az Apache Spark szint.

Vegye figyelembe, hogy ezeket az összesítéseket a hivatkozás toohello [Spark tooAzure Cosmos DB összekötő notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).

### <a name="connect-tooflights-sample-data"></a>Csatlakozás tooflights mintaadatok
Összesítési példákban elérni bizonyos repülési teljesítményadatokat tárolt a **DoctorWho** Azure Cosmos DB adatbázisban. tooconnect tooit kell tooutilize hello a következő kódrészletet:

```
// Import Spark tooAzure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect tooAzure Cosmos DB Database
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US 2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Ezt a kódrészletet a dolgozunk is folyamatos toorun, hogy az átvitel hello Azure Cosmos DB tooSpark származó adatok szűrt set alap lekérdezés ahol ez utóbbi hello végezhet elosztott összesítések. Ebben az esetben arra kérjük, hogy a budapesti (SZE) egymástól légijáratoknál.

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

hello következő eredmények által előállított hello lekérdezések futtatása Jupyter notebook szolgáltatás hello.  Ne feledje, hogy minden hello kódtöredékek általános és nem adott tooany szolgáltatás.

### <a name="running-limit-and-count-queries"></a>Futó korlát és száma
Csak Ön például használt tooin SQL/Spark SQL, most kezdetben a **korlát** lekérdezést:

![Spark korlát lekérdezés](./media/spark-connector/spark-sql-query.png)

hello következő lekérdezésben van egy egyszerű és gyors **száma** lekérdezést:

![Spark COUNT lekérdezés](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a>A GROUP BY lekérdezés
A következő be van állítva, hogy könnyen futtatható az **GROUP BY** lekérdezések írásában, az Azure Cosmos DB adatbázishoz:

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY lekérdezés diagramhoz](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a>KÜLÖNÁLLÓ, ORDER BY lekérdezés
Itt egy **DISTINCT, ORDER BY** lekérdezést:

![Spark GROUP BY lekérdezés diagramhoz](./media/spark-connector/order-by-query.png)

### <a name="continue-hello-flight-data-analysis"></a>Továbbra is hello felé továbbított adatok elemzése
A következő példa lekérdezések toocontinue hello felé továbbított adatok elemzése hello használhatja:

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a>Leggyakoribb 5 késleltetett célok (városokat) onnan induló Budapest
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark felső késések diagramhoz](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a>Közepes késések által a budapesti induló cél városokat kiszámítása
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark közepes késések diagramhoz](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a>Következő lépések

Ha még nem tette meg, le hello Spark tooAzure Cosmos DB összekötő hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-tárházban és további források hello hello tárházban felfedezése:

* [Elosztott összesítések példák](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Mintaparancsfájlok és notebookok](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Érdemes lehet tooreview hello [Apache Spark SQL, DataFrames és adatkészletek útmutató](http://spark.apache.org/docs/latest/sql-programming-guide.html) és hello [Apache Spark on Azure Hdinsighttal](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) cikk.
