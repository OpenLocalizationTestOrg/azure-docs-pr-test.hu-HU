---
title: "Csatlakozás az Apache Spark on Azure Cosmos DB |} Microsoft Docs"
description: "Ez az oktatóanyag segítségével további információkhoz az Azure Cosmos DB Spark-összekötő, amely lehetővé teszi a kapcsolódást az Apache Spark on Azure Cosmos DB elosztott összesítésekkel és adatokkal sciences végre a több-bérlős globálisan elosztott készült, a felhő Microsoft adatbázisrendszer."
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
ms.openlocfilehash: 8ecbb478c81cde25bbd0d1c9ee07ae02b07f8cc7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="accelerate-real-time-big-data-analytics-with-the-spark-to-azure-cosmos-db-connector"></a><span data-ttu-id="2a5a4-104">A Spark a valós idejű big data elemzések érdekében, és Azure Cosmos DB-összekötő</span><span class="sxs-lookup"><span data-stu-id="2a5a4-104">Accelerate real-time big-data analytics with the Spark to Azure Cosmos DB connector</span></span>

<span data-ttu-id="2a5a4-105">A Spark on Azure Cosmos DB összekötőre lehetővé teszi, hogy az Azure Cosmos DB egy bemeneti forrás vagy a kimeneti fogadóját Apache Spark-feladatok nevében járhasson el.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-105">The Spark to Azure Cosmos DB connector enables Azure Cosmos DB to act as an input source or output sink for Apache Spark jobs.</span></span> <span data-ttu-id="2a5a4-106">Csatlakozás [Spark](http://spark.apache.org/) való [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) felgyorsítják arra, hogy a gyorsan változó adatok tudományos problémák megoldásához használható Azure Cosmos DB gyorsan és adatait.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-106">Connecting [Spark](http://spark.apache.org/) to [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) accelerates your ability to solve fast-moving data science problems where you can use Azure Cosmos DB to quickly persist and query data.</span></span> <span data-ttu-id="2a5a4-107">A Spark on Azure Cosmos DB összekötőre hatékonyan használja a natív Azure Cosmos DB felügyelt indexeket.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-107">The Spark to Azure Cosmos DB connector efficiently utilizes the native Azure Cosmos DB managed indexes.</span></span> <span data-ttu-id="2a5a4-108">Az indexek frissíthető oszlopok lehetővé teszik, amikor elemzés végrehajtása, és leküldéses-le nyilas predikátum szűrése fast-módosítása globális adatokat, amelyek az eszközök internetes hálózatát (IoT) között adatok tudományos és elemzés forgatókönyvekre.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-108">The indexes enable updateable columns when you perform analytics and push-down predicate filtering against fast-changing globally distributed data, which range from Internet of Things (IoT) to data science and analytics scenarios.</span></span>

<span data-ttu-id="2a5a4-109">Spark GraphX és a Gremlin graph API-kat az Azure Cosmos DB használata, lásd: [Spark és Apache TinkerPop Gremlin graph időben](spark-connector-graph.md).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-109">For working with Spark GraphX and the Gremlin graph APIs of Azure Cosmos DB, see [Perform graph analytics using Spark and Apache TinkerPop Gremlin](spark-connector-graph.md).</span></span>

## <a name="download"></a><span data-ttu-id="2a5a4-110">Letöltés</span><span class="sxs-lookup"><span data-stu-id="2a5a4-110">Download</span></span>

<span data-ttu-id="2a5a4-111">A kezdéshez töltse le a Spark on Azure Cosmos DB connector (előzetes verzió) az a [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-111">To get started, download the Spark to Azure Cosmos DB connector (preview) from the [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) repository on GitHub.</span></span>

## <a name="connector-components"></a><span data-ttu-id="2a5a4-112">Összekötő-összetevők</span><span class="sxs-lookup"><span data-stu-id="2a5a4-112">Connector components</span></span>

<span data-ttu-id="2a5a4-113">Az összekötő a következő összetevőket használja:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-113">The connector utilizes the following components:</span></span>

* <span data-ttu-id="2a5a4-114">[Az Azure Cosmos DB](http://documentdb.com) lehetővé teszi az ügyfelek kiépítéséhez és rugalmasan méretezhető átviteli sebesség és a tárolási tetszőleges számú földrajzi régiók között.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-114">[Azure Cosmos DB](http://documentdb.com) enables customers to provision and elastically scale both throughput and storage across any number of geographical regions.</span></span> <span data-ttu-id="2a5a4-115">A szolgáltatás nyújt:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-115">The service offers:</span></span>
   * <span data-ttu-id="2a5a4-116">Kapcsolja be a kulcs [globális terjesztési](distribute-data-globally.md) és horizontális skálázhatóságot</span><span class="sxs-lookup"><span data-stu-id="2a5a4-116">Turn key [global distribution](distribute-data-globally.md) and horizontal scale</span></span>
   * <span data-ttu-id="2a5a4-117">Egyetlen számjegy késések fordulnak elő az 99th aránya a garantált</span><span class="sxs-lookup"><span data-stu-id="2a5a4-117">Guaranteed single digit latencies at the 99th percentile</span></span>
   * [<span data-ttu-id="2a5a4-118">Több jól meghatározott konzisztencia modellek</span><span class="sxs-lookup"><span data-stu-id="2a5a4-118">Multiple well-defined consistency models</span></span>](consistency-levels.md)
   * <span data-ttu-id="2a5a4-119">Magas rendelkezésre állású többhelyű képességekkel garantált</span><span class="sxs-lookup"><span data-stu-id="2a5a4-119">Guaranteed high availability with multi-homing capabilities</span></span>
   * <span data-ttu-id="2a5a4-120">Minden szolgáltatás üzemelnek iparágvezető, átfogó [szolgáltatási szintek](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-120">All features are backed by industry-leading, comprehensive [service level agreements](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLAs).</span></span>

* <span data-ttu-id="2a5a4-121">[Az Apache Spark on](http://spark.apache.org/) egy hatékony nyílt forráskódú feldolgozási motor, amely a sebesség, valamint a kifinomult analytics könnyű épül.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-121">[Apache Spark](http://spark.apache.org/) is a powerful open source processing engine that's built around speed, ease of use, and sophisticated analytics.</span></span>

* <span data-ttu-id="2a5a4-122">[Apache Spark on Azure Hdinsighttal](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) , hogy a segítségével telepíthet a felhőbe a kritikus fontosságú központi telepítések elvégzéséhez az Apache Spark on [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-122">[Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) so that you can deploy Apache Spark in the cloud for mission-critical deployments by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="2a5a4-123">Hivatalos támogatott verziók:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-123">Officially supported versions:</span></span>

| <span data-ttu-id="2a5a4-124">Összetevő</span><span class="sxs-lookup"><span data-stu-id="2a5a4-124">Component</span></span> | <span data-ttu-id="2a5a4-125">Verzió</span><span class="sxs-lookup"><span data-stu-id="2a5a4-125">Version</span></span> |
|---------|-------|
|<span data-ttu-id="2a5a4-126">Apache Spark</span><span class="sxs-lookup"><span data-stu-id="2a5a4-126">Apache Spark</span></span>|<span data-ttu-id="2a5a4-127">2.0+</span><span class="sxs-lookup"><span data-stu-id="2a5a4-127">2.0+</span></span>|
| <span data-ttu-id="2a5a4-128">Scala</span><span class="sxs-lookup"><span data-stu-id="2a5a4-128">Scala</span></span>| <span data-ttu-id="2a5a4-129">2.11</span><span class="sxs-lookup"><span data-stu-id="2a5a4-129">2.11</span></span>|
| <span data-ttu-id="2a5a4-130">Az Azure DocumentDB Java SDK</span><span class="sxs-lookup"><span data-stu-id="2a5a4-130">Azure DocumentDB Java SDK</span></span> | <span data-ttu-id="2a5a4-131">1.10.0</span><span class="sxs-lookup"><span data-stu-id="2a5a4-131">1.10.0</span></span> |

<span data-ttu-id="2a5a4-132">Ez a cikk segít néhány egyszerű példák futtatása Python (a pyDocumentDB) keresztül és a Scala-felületek használatával.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-132">This article helps you run some simple samples by using Python (via pyDocumentDB) and the Scala interfaces.</span></span>

<span data-ttu-id="2a5a4-133">Apache Spark és Azure Cosmos DB kétféleképpen történhet:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-133">There are two approaches to connect Apache Spark and Azure Cosmos DB:</span></span>
- <span data-ttu-id="2a5a4-134">PyDocumentDB keresztül használja a [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-134">Use pyDocumentDB via the [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span></span>
- <span data-ttu-id="2a5a4-135">Hozzon létre egy Java-alapú Spark on Azure Cosmos DB-összekötőhöz való használatával a [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-135">Create a Java-based Spark to Azure Cosmos DB connector by utilizing the [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

## <a name="pydocumentdb-implementation"></a><span data-ttu-id="2a5a4-136">pyDocumentDB végrehajtása</span><span class="sxs-lookup"><span data-stu-id="2a5a4-136">pyDocumentDB implementation</span></span>
<span data-ttu-id="2a5a4-137">Az aktuális [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) lehetővé teszi a Spark on Azure Cosmos DB csatlakozni, az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-137">The current [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) enables you to connect Spark to Azure Cosmos DB as shown in the following diagram:</span></span>

![Spark Azure Cosmos DB adatfolyamra pyDocumentDB DB keresztül](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-the-pydocumentdb-implementation"></a><span data-ttu-id="2a5a4-139">A pyDocumentDB megvalósítási adatfolyama</span><span class="sxs-lookup"><span data-stu-id="2a5a4-139">Data flow of the pyDocumentDB implementation</span></span>

<span data-ttu-id="2a5a4-140">Az adatfolyam a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-140">The data flow is as follows:</span></span>

1. <span data-ttu-id="2a5a4-141">A Spark főcsomópont az Azure Cosmos DB átjárócsomópont pyDocumentDB keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-141">The Spark master node connects to the Azure Cosmos DB gateway node via pyDocumentDB.</span></span> <span data-ttu-id="2a5a4-142">A felhasználó csak a Spark- és Azure Cosmos DB-kapcsolatok ad meg.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-142">A user specifies only the Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="2a5a4-143">A megfelelő fő és átjáró csomópontok való csatlakozás történik a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-143">Connections to the respective master and gateway nodes are transparent to the user.</span></span>
2. <span data-ttu-id="2a5a4-144">Az átjárócsomópont teszi Azure Cosmos DB, ahol a lekérdezést később fut a gyűjtemény a partíciók az adatok csomópontok irányuló lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-144">The gateway node makes the query against Azure Cosmos DB where the query subsequently runs against the collection's partitions in the data nodes.</span></span> <span data-ttu-id="2a5a4-145">A lekérdezések választ küld vissza az átjárócsomópontnak, és adott eredményhalmaz a Spark főcsomópont küld vissza.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-145">The response for those queries is sent back to the gateway node, and that result set is returned to the Spark master node.</span></span>
3. <span data-ttu-id="2a5a4-146">A Spark munkavégző csomópontokhoz későbbi lekérdezéseket (például egy Spark DataFrame) kerülnek feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-146">Subsequent queries (for example, against a Spark DataFrame) are sent to the Spark worker nodes for processing.</span></span>

<span data-ttu-id="2a5a4-147">Spark és Azure Cosmos DB közötti kommunikáció a Spark főcsomópont és az Azure Cosmos DB átjárócsomópontok korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-147">Communication between Spark and Azure Cosmos DB is limited to the Spark master node and Azure Cosmos DB gateway nodes.</span></span>  <span data-ttu-id="2a5a4-148">A lekérdezések lépjen a gyors lehetővé teszi, hogy a szállítási réteg a két csomópont között.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-148">The queries go as fast as the transport layer between these two nodes allows.</span></span>

### <a name="install-pydocumentdb"></a><span data-ttu-id="2a5a4-149">Telepítse a pyDocumentDB</span><span class="sxs-lookup"><span data-stu-id="2a5a4-149">Install pyDocumentDB</span></span>
<span data-ttu-id="2a5a4-150">Telepíthető pyDocumentDB az illesztőprogram-csomópont használatával **pip**, például:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-150">You can install pyDocumentDB on your driver node by using **pip**, for example:</span></span>

```
pip install pyDocumentDB
```


### <a name="connect-spark-to-azure-cosmos-db-via-pydocumentdb"></a><span data-ttu-id="2a5a4-151">Csatlakozás az Azure Cosmos Adatbázishoz Spark pyDocumentDB</span><span class="sxs-lookup"><span data-stu-id="2a5a4-151">Connect Spark to Azure Cosmos DB via pyDocumentDB</span></span>
<span data-ttu-id="2a5a4-152">A kommunikáció átviteli egyszerűsége hajt végre a lekérdezés végrehajtása a Spark on Azure Cosmos DB a pyDocumentDB viszonylag egyszerű.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-152">The simplicity of the communication transport makes execution of a query from Spark to Azure Cosmos DB by using pyDocumentDB relatively simple.</span></span>

<span data-ttu-id="2a5a4-153">A következő kódrészletet a pyDocumentDB használata Spark környezetben jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-153">The following code snippet shows how to use pyDocumentDB in a Spark context.</span></span>

```
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]


# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

<span data-ttu-id="2a5a4-154">A kódrészletet leírtaknak megfelelően:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-154">As noted in the code snippet:</span></span>

* <span data-ttu-id="2a5a4-155">Az Azure Cosmos DB Python SDK (`pyDocumentDB`) tartalmazza az összes szükséges kapcsolati paraméter.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-155">The Azure Cosmos DB Python SDK (`pyDocumentDB`) contains the all the necessary connection parameters.</span></span> <span data-ttu-id="2a5a4-156">Például az elsődleges helyek paraméter úgy dönt, a olvasható replika és a prioritási sorrendben.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-156">For example, the preferred locations parameter chooses the read replica and priority order.</span></span>
*  <span data-ttu-id="2a5a4-157">A szükséges kódtárak importálja és konfigurálja a **főkulcsos** és **állomás** létrehozása az Azure Cosmos DB *ügyfél* (**pydocumentdb.document_client**).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-157">Import the necessary libraries and configure your **masterKey** and **host** to create the Azure Cosmos DB *client* (**pydocumentdb.document_client**).</span></span>


### <a name="execute-spark-queries-via-pydocumentdb"></a><span data-ttu-id="2a5a4-158">A Spark-lekérdezéseket hajt végre a pyDocumentDB keresztül</span><span class="sxs-lookup"><span data-stu-id="2a5a4-158">Execute Spark Queries via pyDocumentDB</span></span>
<span data-ttu-id="2a5a4-159">A következő példák az előző részlet megadott írásvédett kulcsok használatával létrehozott Azure Cosmos DB példányának használatára.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-159">The following examples use the Azure Cosmos DB instance that was created in the previous snippet by using the specified read-only keys.</span></span> <span data-ttu-id="2a5a4-160">A következő kódrészletet csatlakozik a **airports.codes** gyűjtemény a DoctorWho fiók, mint a korábban meghatározott, és a repülőtéri várost Washington államban kibontásához lekérdezés futtatása.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-160">The following code snippet connects to the **airports.codes** collection in the DoctorWho account as specified earlier and runs a query to extract the airport cities in Washington state.</span></span>

```
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
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

<span data-ttu-id="2a5a4-161">A lekérdezés végrehajtása keresztül után **lekérdezés**, az eredmény egy **query_iterable. QueryIterable** , Python listájára alakítja át.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-161">After the query has been executed via **query**, the result is a **query_iterable.QueryIterable** that is converted to a Python list.</span></span> <span data-ttu-id="2a5a4-162">Python listáját könnyen átalakítható a Spark DataFrame az alábbi kód használatával:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-162">A Python list can be easily converted to a Spark DataFrame by using the following code:</span></span>

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-the-pydocumentdb-to-connect-spark-to-azure-cosmos-db"></a><span data-ttu-id="2a5a4-163">Miért érdemes használni a pyDocumentDB Spark az Azure Cosmos Adatbázishoz csatlakozni?</span><span class="sxs-lookup"><span data-stu-id="2a5a4-163">Why use the pyDocumentDB to connect Spark to Azure Cosmos DB?</span></span>
<span data-ttu-id="2a5a4-164">Spark pyDocumentDB segítségével az Azure Cosmos Adatbázishoz kapcsolódó forgatókönyvek általában van ahol:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-164">Connecting Spark to Azure Cosmos DB by using pyDocumentDB is typically for scenarios where:</span></span>

* <span data-ttu-id="2a5a4-165">Python használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-165">You want to use Python.</span></span>
* <span data-ttu-id="2a5a4-166">A Spark az Azure Cosmos Adatbázisból viszonylag kis eredményhalmazt vannak ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-166">You are returning a relatively small result set from Azure Cosmos DB to Spark.</span></span> <span data-ttu-id="2a5a4-167">Vegye figyelembe, hogy az Azure Cosmos Adatbázisba az alapul szolgáló dataset Igen tekintélyes lehet.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-167">Note that the underlying dataset in Azure Cosmos DB can be quite large.</span></span> <span data-ttu-id="2a5a4-168">Szűrők, ez azt jelenti, hogy az Azure Cosmos DB adatforrás predikátum szűrők futtatott alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-168">You are applying filters, that is, running predicate filters, against your Azure Cosmos DB source.</span></span>  

## <a name="spark-to-azure-cosmos-db-connector"></a><span data-ttu-id="2a5a4-169">Spark és Azure Cosmos DB-összekötő</span><span class="sxs-lookup"><span data-stu-id="2a5a4-169">Spark to Azure Cosmos DB connector</span></span>

<span data-ttu-id="2a5a4-170">A Spark on Azure Cosmos DB összekötőre használja a [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) és mozgatja az adatokat a Spark munkavégző csomópontokhoz és Azure Cosmos DB között, az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-170">The Spark to Azure Cosmos DB connector utilizes the [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) and moves data between the Spark worker nodes and Azure Cosmos DB as shown in the following diagram:</span></span>

![A Spark az Azure Cosmos DB összekötő adatfolyama](./media/spark-connector/spark-connector.png)

<span data-ttu-id="2a5a4-172">Az adatfolyam a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-172">The data flow is as follows:</span></span>

1. <span data-ttu-id="2a5a4-173">A Spark főcsomópont az beszerzése a partíciótérképen Azure Cosmos DB átjárócsomópont csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-173">The Spark master node connects to the Azure Cosmos DB gateway node to obtain the partition map.</span></span> <span data-ttu-id="2a5a4-174">A felhasználó csak a Spark- és Azure Cosmos DB-kapcsolatok ad meg.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-174">A user specifies only the Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="2a5a4-175">A megfelelő fő és átjáró csomópontok való csatlakozás történik a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-175">Connections to the respective master and gateway nodes are transparent to the user.</span></span>
2. <span data-ttu-id="2a5a4-176">Ezt az információt a külső fő csomópont valósul meg.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-176">This information is provided back to the Spark master node.</span></span>  <span data-ttu-id="2a5a4-177">Ezen a ponton adható meg, hogy a partíció és az Azure Cosmos Adatbázisba való hozzáférést igénylő helyükre értelmezni tudja.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-177">At this point, you should be able to parse the query to determine the partitions and their locations in Azure Cosmos DB that you need to access.</span></span>
3. <span data-ttu-id="2a5a4-178">Ezek az információk átkerülnek a Spark munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-178">This information is transmitted to the Spark worker nodes.</span></span>
4. <span data-ttu-id="2a5a4-179">A Spark munkavégző csomópontokhoz csatlakozni az Azure Cosmos DB partíciókat közvetlenül kinyeri az adatokat, és térjen vissza az adatokat a Spark a partíciók a Spark munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-179">The Spark worker nodes connect to the Azure Cosmos DB partitions directly to extract the data and return the data to the Spark partitions in the Spark worker nodes.</span></span>

<span data-ttu-id="2a5a4-180">Spark és Azure Cosmos DB közötti kommunikáció oka jelentősen gyorsabb az adatátvitelt jelölik a Spark munkavégző csomópontokhoz és az Azure Cosmos DB adatok csomópontok (partíciók) között.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-180">Communication between Spark and Azure Cosmos DB is significantly faster because the data movement is between the Spark worker nodes and the Azure Cosmos DB data nodes (partitions).</span></span>

### <a name="build-the-spark-to-azure-cosmos-db-connector"></a><span data-ttu-id="2a5a4-181">A Spark és Azure Cosmos DB-összekötő létrehozása</span><span class="sxs-lookup"><span data-stu-id="2a5a4-181">Build the Spark to Azure Cosmos DB connector</span></span>
<span data-ttu-id="2a5a4-182">Az összekötő projekt jelenleg maven használja.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-182">Currently, the connector project uses maven.</span></span> <span data-ttu-id="2a5a4-183">Az összekötő függőségek nélkül létrehozásához futtathatja:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-183">To build the connector without dependencies, you can run:</span></span>
```
mvn clean package
```
<span data-ttu-id="2a5a4-184">Emellett letöltheti a legújabb verzióját a JAR a *kiadott* mappát.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-184">You can also download the latest versions of the JAR from the *releases* folder.</span></span>

### <a name="include-the-azure-cosmos-db-spark-jar"></a><span data-ttu-id="2a5a4-185">Az Azure Cosmos DB külső JAR tartalmazza</span><span class="sxs-lookup"><span data-stu-id="2a5a4-185">Include the Azure Cosmos DB Spark JAR</span></span>
<span data-ttu-id="2a5a4-186">A kód végrehajtása előtt meg kell adnia az Azure Cosmos DB külső JAR.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-186">Before you execute any code, you need to include the Azure Cosmos DB Spark JAR.</span></span>  <span data-ttu-id="2a5a4-187">Használatakor a **spark-rendszerhéj**, majd a használatával megadhatja a JAR a **--JAR-fájlok kivételével** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-187">If you are using the **spark-shell**, then you can include the JAR by using the **--jars** option.</span></span>  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

<span data-ttu-id="2a5a4-188">A JAR függőségek nélkül végrehajtani, használja a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-188">If you want to execute the JAR without dependencies, use the following code:</span></span>

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

<span data-ttu-id="2a5a4-189">Ha a notebook szolgáltatással, például az Azure HDInsight Jupyter notebook szolgáltatást használ, akkor használhatja a **spark magic** parancsokat:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-189">If you are using a notebook service such as Azure HDInsight Jupyter notebook service, you can use the **spark magic** commands:</span></span>

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

<span data-ttu-id="2a5a4-190">A **JAR-fájlok kivételével** parancs lehetővé teszi a két JAR-fájlok kivételével, a szükséges **azure-cosmosdb-spark** (önmagában, illetve az Azure DocumentDB Java SDK-t), és amelyeket **scala-tükrözze** , hogy ne zavarja a Livy hívások (Jupyter notebook > Livy > Spark).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-190">The **jars** command enables you to include the two JARs that are needed for **azure-cosmosdb-spark** (itself and the Azure DocumentDB Java SDK) and exclude **scala-reflect** so that it does not interfere with the Livy calls (Jupyter notebook > Livy > Spark).</span></span>

### <a name="connect-spark-to-azure-cosmos-db-using-the-connector"></a><span data-ttu-id="2a5a4-191">Spark csatlakozzon az összekötővel Azure Cosmos Adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-191">Connect Spark to Azure Cosmos DB using the connector</span></span>
<span data-ttu-id="2a5a4-192">Bár a kommunikációs átviteli egy kicsit bonyolultabb, a lekérdezés végrehajtása a Spark on Azure Cosmos DB-összekötő segítségével is jelentősen gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-192">Although the communication transport is a little more complicated, executing a query from Spark to Azure Cosmos DB by using the connector is significantly faster.</span></span>

<span data-ttu-id="2a5a4-193">A következő kódrészletet bemutatja, hogyan Spark környezetben az összekötő használatára.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-193">The following code snippet shows how to use the connector in a Spark context.</span></span>

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
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

<span data-ttu-id="2a5a4-194">A kódrészletet leírtaknak megfelelően:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-194">As noted in the code snippet:</span></span>

- <span data-ttu-id="2a5a4-195">**Azure-cosmosdb-spark** az összes szükséges kapcsolat paramétereit tartalmazza, többek között az elsődleges helyek.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-195">**azure-cosmosdb-spark** contains the all the necessary connection parameters, which include the preferred locations.</span></span> <span data-ttu-id="2a5a4-196">Például kiválaszthatja a olvasható replika és a prioritási sorrendben.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-196">For example, you can choose the read replica and priority order.</span></span>
- <span data-ttu-id="2a5a4-197">Ebben az esetben a szükséges könyvtárak importálása, és a főkulcsos és az állomás hozhat létre az Azure Cosmos DB ügyfél.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-197">Just import the necessary libraries and configure your masterKey and host to create the Azure Cosmos DB client.</span></span>

### <a name="execute-spark-queries-via-the-connector"></a><span data-ttu-id="2a5a4-198">Az összekötőn keresztül Spark-lekérdezéseket hajt végre</span><span class="sxs-lookup"><span data-stu-id="2a5a4-198">Execute Spark queries via the connector</span></span>

<span data-ttu-id="2a5a4-199">Az alábbi példában az Azure Cosmos DB-példány az előző részlet jött létre a megadott írásvédett kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-199">The following example uses the Azure Cosmos DB instance that was created in the previous snippet by using the specified read-only keys.</span></span> <span data-ttu-id="2a5a4-200">A következő kódrészletet a DepartureDelays.flights_pcoll gyűjtemény (a korábban megadott DoctorWho fiókban) csatlakozik, és a felhőszolgáltató közötti átviteléhez késleltetés információkat, amelyek a budapesti vannak induló légijáratok lekérdezés futtatása.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-200">The following code snippet connects to the DepartureDelays.flights_pcoll collection (in the DoctorWho account as specified earlier) and runs a query to extract the flight delay information of flights that are departing from Seattle.</span></span>

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-the-spark-to-azure-cosmos-db-connector-implementation"></a><span data-ttu-id="2a5a4-201">A Spark on Azure Cosmos DB összekötő megvalósításához miért érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="2a5a4-201">Why use the Spark to Azure Cosmos DB connector implementation?</span></span>

<span data-ttu-id="2a5a4-202">Forgatókönyvek általában Spark csatlakozik Azure Cosmos DB-összekötővel van ahol:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-202">Connecting Spark to Azure Cosmos DB by using the connector is typically for scenarios where:</span></span>

* <span data-ttu-id="2a5a4-203">Scala használja, és frissítse úgy, hogy tartalmazzák a Python-burkoló leírtaknak megfelelően [probléma 3: hozzáadása Python burkoló és példák](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-203">You want to use Scala and update it to include a Python wrapper as noted in [Issue 3: Add Python wrapper and examples](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span></span>
* <span data-ttu-id="2a5a4-204">Rendelkezik egy nagy mennyiségű adatot Apache Spark- és Azure Cosmos DB közötti átviteléhez.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-204">You have a large amount of data to transfer between Apache Spark and Azure Cosmos DB.</span></span>

<span data-ttu-id="2a5a4-205">Segítenek a lekérdezés teljesítménybeli különbség az, tekintse meg a [lekérdezés teszt futtatása wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-205">To give you an idea of the query performance difference, see the [Query Test Runs wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span></span>

## <a name="distributed-aggregation-example"></a><span data-ttu-id="2a5a4-206">Elosztott összesítési – példa</span><span class="sxs-lookup"><span data-stu-id="2a5a4-206">Distributed aggregation example</span></span>
<span data-ttu-id="2a5a4-207">Ez a témakör néhány példa arra, hogyan lehet úgy teheti meg, elosztott összesítések és az elemzések Apache Spark- és Azure Cosmos DB együtt.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-207">This section provides some examples of how you can do distributed aggregations and analytics by using Apache Spark and Azure Cosmos DB together.</span></span> <span data-ttu-id="2a5a4-208">Azure Cosmos-adatbázis már támogatja az összesítéseket, amely ismertet a [bolygónk méretezési összesíti az Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-208">Azure Cosmos DB already supports aggregations, which is discussed in the [Planet scale aggregates with Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span></span> <span data-ttu-id="2a5a4-209">Ez hogyan-EK az Apache Spark a következő szintre.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-209">Here is how you can take it to the next level with Apache Spark.</span></span>

<span data-ttu-id="2a5a4-210">Vegye figyelembe, hogy ezeket az összesítéseket reference, hogy a [Spark az Azure Cosmos DB összekötő notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span><span class="sxs-lookup"><span data-stu-id="2a5a4-210">Note that these aggregations are in reference to the [Spark to Azure Cosmos DB Connector notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span></span>

### <a name="connect-to-flights-sample-data"></a><span data-ttu-id="2a5a4-211">Járatok minta adatokhoz</span><span class="sxs-lookup"><span data-stu-id="2a5a4-211">Connect to flights sample data</span></span>
<span data-ttu-id="2a5a4-212">Összesítési példákban elérni bizonyos repülési teljesítményadatokat tárolt a **DoctorWho** Azure Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-212">These aggregation examples access some flight performance data that's stored in our **DoctorWho** Azure Cosmos DB database.</span></span> <span data-ttu-id="2a5a4-213">A csatlakozáshoz, szüksége használatára a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-213">To connect to it, you need to utilize the following code snippet:</span></span>

```
// Import Spark to Azure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect to Azure Cosmos DB Database
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

<span data-ttu-id="2a5a4-214">Ezt a kódrészletet a is fogjuk továbbítani a szűrt adatkészletet az Azure Cosmos Adatbázisból Spark ahol ez utóbbi végezhet elosztott összesítések alap-lekérdezés futtatható.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-214">With this snippet, we are also going to run a base query that transfers the filtered set of data from Azure Cosmos DB to Spark where the latter can perform distributed aggregates.</span></span> <span data-ttu-id="2a5a4-215">Ebben az esetben arra kérjük, hogy a budapesti (SZE) egymástól légijáratoknál.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-215">In this case, we are asking for flights that depart from Seattle (SEA).</span></span>

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

<span data-ttu-id="2a5a4-216">Az alábbi eredményeket a lekérdezések futtatása Jupyter notebook szolgáltatásból hoztak létre.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-216">The following results were generated by running the queries from the Jupyter notebook service.</span></span>  <span data-ttu-id="2a5a4-217">Ne feledje, hogy a kódrészleteket általános és nem meghatározott, bármely szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-217">Note that all the code snippets are generic and not specific to any service.</span></span>

### <a name="running-limit-and-count-queries"></a><span data-ttu-id="2a5a4-218">Futó korlát és száma</span><span class="sxs-lookup"><span data-stu-id="2a5a4-218">Running LIMIT and COUNT queries</span></span>
<span data-ttu-id="2a5a4-219">Fentiekhez hasonló által használt SQL/Spark SQL, kezdjük egy **korlát** lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-219">Just like you're used to in SQL/Spark SQL, let's start off with a **LIMIT** query:</span></span>

![Spark korlát lekérdezés](./media/spark-connector/spark-sql-query.png)

<span data-ttu-id="2a5a4-221">A következő lekérdezés az egyszerű és gyors **száma** lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-221">The next query is a simple and fast **COUNT** query:</span></span>

![Spark COUNT lekérdezés](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a><span data-ttu-id="2a5a4-223">A GROUP BY lekérdezés</span><span class="sxs-lookup"><span data-stu-id="2a5a4-223">GROUP BY query</span></span>
<span data-ttu-id="2a5a4-224">A következő be van állítva, hogy könnyen futtatható az **GROUP BY** lekérdezések írásában, az Azure Cosmos DB adatbázishoz:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-224">In this next set, we can easily run **GROUP BY** queries against our Azure Cosmos DB database:</span></span>

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY lekérdezés diagramhoz](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a><span data-ttu-id="2a5a4-226">KÜLÖNÁLLÓ, ORDER BY lekérdezés</span><span class="sxs-lookup"><span data-stu-id="2a5a4-226">DISTINCT, ORDER BY query</span></span>
<span data-ttu-id="2a5a4-227">Itt egy **DISTINCT, ORDER BY** lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-227">And here is a **DISTINCT, ORDER BY** query:</span></span>

![Spark GROUP BY lekérdezés diagramhoz](./media/spark-connector/order-by-query.png)

### <a name="continue-the-flight-data-analysis"></a><span data-ttu-id="2a5a4-229">Továbbra is az felé továbbított adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="2a5a4-229">Continue the flight data analysis</span></span>
<span data-ttu-id="2a5a4-230">A következő példa lekérdezések felé továbbított adatok elemzése a folytatáshoz használhatja:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-230">You can use the following example queries to continue analysis of the flight data:</span></span>

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a><span data-ttu-id="2a5a4-231">Leggyakoribb 5 késleltetett célok (városokat) onnan induló Budapest</span><span class="sxs-lookup"><span data-stu-id="2a5a4-231">Top 5 delayed destinations (cities) departing from Seattle</span></span>
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark felső késések diagramhoz](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a><span data-ttu-id="2a5a4-233">Közepes késések által a budapesti induló cél városokat kiszámítása</span><span class="sxs-lookup"><span data-stu-id="2a5a4-233">Calculate median delays by destination cities departing from Seattle</span></span>
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark közepes késések diagramhoz](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a><span data-ttu-id="2a5a4-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a5a4-235">Next steps</span></span>

<span data-ttu-id="2a5a4-236">Ha még nem tette meg, töltse le a Spark on Azure Cosmos DB-összekötőt a [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-tárházban és vizsgálja meg a tárházban további források:</span><span class="sxs-lookup"><span data-stu-id="2a5a4-236">If you haven't already, download the Spark to Azure Cosmos DB connector from the [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub repository and explore the additional resources in the repo:</span></span>

* [<span data-ttu-id="2a5a4-237">Elosztott összesítések példák</span><span class="sxs-lookup"><span data-stu-id="2a5a4-237">Distributed Aggregations Examples</span></span>](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [<span data-ttu-id="2a5a4-238">Mintaparancsfájlok és notebookok</span><span class="sxs-lookup"><span data-stu-id="2a5a4-238">Sample Scripts and Notebooks</span></span>](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

<span data-ttu-id="2a5a4-239">Is érdemes áttekinteni a [Apache Spark SQL, DataFrames és adatkészletek útmutató](http://spark.apache.org/docs/latest/sql-programming-guide.html) és a [Apache Spark on Azure Hdinsighttal](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2a5a4-239">You might also want to review the [Apache Spark SQL, DataFrames, and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html) and the [Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) article.</span></span>
