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
# <a name="accelerate-real-time-big-data-analytics-with-hello-spark-tooazure-cosmos-db-connector"></a><span data-ttu-id="ba413-104">Hello Spark tooAzure Cosmos DB Connector valós idejű big data elemzések érdekében</span><span class="sxs-lookup"><span data-stu-id="ba413-104">Accelerate real-time big-data analytics with hello Spark tooAzure Cosmos DB connector</span></span>

<span data-ttu-id="ba413-105">hello Spark tooAzure Cosmos DB összekötő lehetővé teszi, hogy a bemeneti forrás vagy kimeneti fogadóját feladatok az Apache Spark on Azure Cosmos DB tooact.</span><span class="sxs-lookup"><span data-stu-id="ba413-105">hello Spark tooAzure Cosmos DB connector enables Azure Cosmos DB tooact as an input source or output sink for Apache Spark jobs.</span></span> <span data-ttu-id="ba413-106">Csatlakozás [Spark](http://spark.apache.org/) túl[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) felgyorsítják a képes toosolve gyorsan változó adattudomány használható Azure Cosmos DB tooquickly problémák továbbra is fennáll, és adatokat lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="ba413-106">Connecting [Spark](http://spark.apache.org/) too[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) accelerates your ability toosolve fast-moving data science problems where you can use Azure Cosmos DB tooquickly persist and query data.</span></span> <span data-ttu-id="ba413-107">hello Spark tooAzure Cosmos DB összekötő hatékonyan hello natív Azure Cosmos DB felügyelt indexek használja.</span><span class="sxs-lookup"><span data-stu-id="ba413-107">hello Spark tooAzure Cosmos DB connector efficiently utilizes hello native Azure Cosmos DB managed indexes.</span></span> <span data-ttu-id="ba413-108">hello indexek frissíthető oszlopok lehetővé teszik, amikor analytics elvégezhető, és leküldéses-le nyilas predikátum fast-módosítása globálisan szűrése adatait, amely az eszközök internetes hálózatát (IoT) toodata tudományos és elemzés forgatókönyvek között.</span><span class="sxs-lookup"><span data-stu-id="ba413-108">hello indexes enable updateable columns when you perform analytics and push-down predicate filtering against fast-changing globally distributed data, which range from Internet of Things (IoT) toodata science and analytics scenarios.</span></span>

<span data-ttu-id="ba413-109">Spark GraphX és hello Gremlin graph API-kat az Azure Cosmos DB használata, lásd: [Spark és Apache TinkerPop Gremlin graph időben](spark-connector-graph.md).</span><span class="sxs-lookup"><span data-stu-id="ba413-109">For working with Spark GraphX and hello Gremlin graph APIs of Azure Cosmos DB, see [Perform graph analytics using Spark and Apache TinkerPop Gremlin](spark-connector-graph.md).</span></span>

## <a name="download"></a><span data-ttu-id="ba413-110">Letöltés</span><span class="sxs-lookup"><span data-stu-id="ba413-110">Download</span></span>

<span data-ttu-id="ba413-111">tooget elindult, hello Spark tooAzure Cosmos DB connector (előzetes verzió) letöltése hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="ba413-111">tooget started, download hello Spark tooAzure Cosmos DB connector (preview) from hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) repository on GitHub.</span></span>

## <a name="connector-components"></a><span data-ttu-id="ba413-112">Összekötő-összetevők</span><span class="sxs-lookup"><span data-stu-id="ba413-112">Connector components</span></span>

<span data-ttu-id="ba413-113">hello összekötő hello a következő összetevőket használja:</span><span class="sxs-lookup"><span data-stu-id="ba413-113">hello connector utilizes hello following components:</span></span>

* <span data-ttu-id="ba413-114">[Az Azure Cosmos DB](http://documentdb.com) lehetővé teszi, hogy az ügyfelek tooprovision és rugalmasan méretezhető átviteli sebesség és a tárolási tetszőleges számú földrajzi régiók között.</span><span class="sxs-lookup"><span data-stu-id="ba413-114">[Azure Cosmos DB](http://documentdb.com) enables customers tooprovision and elastically scale both throughput and storage across any number of geographical regions.</span></span> <span data-ttu-id="ba413-115">hello szolgáltatást kínál:</span><span class="sxs-lookup"><span data-stu-id="ba413-115">hello service offers:</span></span>
   * <span data-ttu-id="ba413-116">Kapcsolja be a kulcs [globális terjesztési](distribute-data-globally.md) és horizontális skálázhatóságot</span><span class="sxs-lookup"><span data-stu-id="ba413-116">Turn key [global distribution](distribute-data-globally.md) and horizontal scale</span></span>
   * <span data-ttu-id="ba413-117">Garantált egyetlen számjegy késések hello 99th PERCENTILIS:</span><span class="sxs-lookup"><span data-stu-id="ba413-117">Guaranteed single digit latencies at hello 99th percentile</span></span>
   * [<span data-ttu-id="ba413-118">Több jól meghatározott konzisztencia modellek</span><span class="sxs-lookup"><span data-stu-id="ba413-118">Multiple well-defined consistency models</span></span>](consistency-levels.md)
   * <span data-ttu-id="ba413-119">Magas rendelkezésre állású többhelyű képességekkel garantált</span><span class="sxs-lookup"><span data-stu-id="ba413-119">Guaranteed high availability with multi-homing capabilities</span></span>
   * <span data-ttu-id="ba413-120">Minden szolgáltatás üzemelnek iparágvezető, átfogó [szolgáltatási szintek](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).</span><span class="sxs-lookup"><span data-stu-id="ba413-120">All features are backed by industry-leading, comprehensive [service level agreements](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLAs).</span></span>

* <span data-ttu-id="ba413-121">[Az Apache Spark on](http://spark.apache.org/) egy hatékony nyílt forráskódú feldolgozási motor, amely a sebesség, valamint a kifinomult analytics könnyű épül.</span><span class="sxs-lookup"><span data-stu-id="ba413-121">[Apache Spark](http://spark.apache.org/) is a powerful open source processing engine that's built around speed, ease of use, and sophisticated analytics.</span></span>

* <span data-ttu-id="ba413-122">[Apache Spark on Azure Hdinsighttal](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) , hogy az Apache Spark on hello felhő kritikus telepítések segítségével telepíthet [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="ba413-122">[Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) so that you can deploy Apache Spark in hello cloud for mission-critical deployments by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="ba413-123">Hivatalos támogatott verziók:</span><span class="sxs-lookup"><span data-stu-id="ba413-123">Officially supported versions:</span></span>

| <span data-ttu-id="ba413-124">Összetevő</span><span class="sxs-lookup"><span data-stu-id="ba413-124">Component</span></span> | <span data-ttu-id="ba413-125">Verzió</span><span class="sxs-lookup"><span data-stu-id="ba413-125">Version</span></span> |
|---------|-------|
|<span data-ttu-id="ba413-126">Apache Spark</span><span class="sxs-lookup"><span data-stu-id="ba413-126">Apache Spark</span></span>|<span data-ttu-id="ba413-127">2.0+</span><span class="sxs-lookup"><span data-stu-id="ba413-127">2.0+</span></span>|
| <span data-ttu-id="ba413-128">Scala</span><span class="sxs-lookup"><span data-stu-id="ba413-128">Scala</span></span>| <span data-ttu-id="ba413-129">2.11</span><span class="sxs-lookup"><span data-stu-id="ba413-129">2.11</span></span>|
| <span data-ttu-id="ba413-130">Az Azure DocumentDB Java SDK</span><span class="sxs-lookup"><span data-stu-id="ba413-130">Azure DocumentDB Java SDK</span></span> | <span data-ttu-id="ba413-131">1.10.0</span><span class="sxs-lookup"><span data-stu-id="ba413-131">1.10.0</span></span> |

<span data-ttu-id="ba413-132">Ez a cikk segítséget nyújt a futtatásával néhány egyszerű minták Python (a pyDocumentDB) keresztül, és hello Scala felületek.</span><span class="sxs-lookup"><span data-stu-id="ba413-132">This article helps you run some simple samples by using Python (via pyDocumentDB) and hello Scala interfaces.</span></span>

<span data-ttu-id="ba413-133">Két megközelítés tooconnect Apache Spark és Azure Cosmos DB vannak:</span><span class="sxs-lookup"><span data-stu-id="ba413-133">There are two approaches tooconnect Apache Spark and Azure Cosmos DB:</span></span>
- <span data-ttu-id="ba413-134">Használja a pyDocumentDB keresztül hello [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span><span class="sxs-lookup"><span data-stu-id="ba413-134">Use pyDocumentDB via hello [Azure DocumentDB Python SDK](https://github.com/Azure/azure-documentdb-python).</span></span>
- <span data-ttu-id="ba413-135">Hozzon létre egy Java-alapú Spark tooAzure Cosmos DB összekötőt hello felügyelniük [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span><span class="sxs-lookup"><span data-stu-id="ba413-135">Create a Java-based Spark tooAzure Cosmos DB connector by utilizing hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java).</span></span>

## <a name="pydocumentdb-implementation"></a><span data-ttu-id="ba413-136">pyDocumentDB végrehajtása</span><span class="sxs-lookup"><span data-stu-id="ba413-136">pyDocumentDB implementation</span></span>
<span data-ttu-id="ba413-137">aktuális hello [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) lehetővé teszi a tooconnect Spark tooAzure Cosmos DB a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="ba413-137">hello current [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) enables you tooconnect Spark tooAzure Cosmos DB as shown in hello following diagram:</span></span>

![Spark tooAzure Cosmos DB adatfolyam a pyDocumentDB DB keresztül](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-hello-pydocumentdb-implementation"></a><span data-ttu-id="ba413-139">Hello pyDocumentDB megvalósítási adatfolyama</span><span class="sxs-lookup"><span data-stu-id="ba413-139">Data flow of hello pyDocumentDB implementation</span></span>

<span data-ttu-id="ba413-140">hello adatáramlás a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="ba413-140">hello data flow is as follows:</span></span>

1. <span data-ttu-id="ba413-141">hello Spark főcsomópont toohello Azure Cosmos DB átjáró csomópont pyDocumentDB keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="ba413-141">hello Spark master node connects toohello Azure Cosmos DB gateway node via pyDocumentDB.</span></span> <span data-ttu-id="ba413-142">A felhasználó csak hello Spark és Azure Cosmos DB-kapcsolatok ad meg.</span><span class="sxs-lookup"><span data-stu-id="ba413-142">A user specifies only hello Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="ba413-143">Kapcsolatok toohello megfelelő master és átjáró-csomópontok átlátszó toohello felhasználókként szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="ba413-143">Connections toohello respective master and gateway nodes are transparent toohello user.</span></span>
2. <span data-ttu-id="ba413-144">hello átjárócsomópont teszi hello Azure Cosmos DB hello lekérdezés ezt követően futtató hello adatcsomópontokat hello gyűjtemény a partíciók ellen irányuló lekérdezés esetén.</span><span class="sxs-lookup"><span data-stu-id="ba413-144">hello gateway node makes hello query against Azure Cosmos DB where hello query subsequently runs against hello collection's partitions in hello data nodes.</span></span> <span data-ttu-id="ba413-145">hello választ a lekérdezések akkor küldi vissza toohello átjárócsomópontnak, és adott eredménykészlet toohello Spark főcsomópont ad vissza.</span><span class="sxs-lookup"><span data-stu-id="ba413-145">hello response for those queries is sent back toohello gateway node, and that result set is returned toohello Spark master node.</span></span>
3. <span data-ttu-id="ba413-146">További lekérdezéseket (például egy Spark DataFrame) toohello Spark munkavégző csomópontokhoz feldolgozásra kerülnek.</span><span class="sxs-lookup"><span data-stu-id="ba413-146">Subsequent queries (for example, against a Spark DataFrame) are sent toohello Spark worker nodes for processing.</span></span>

<span data-ttu-id="ba413-147">Spark és Azure Cosmos DB közötti kommunikáció korlátozott toohello Spark főcsomópont és Azure Cosmos DB átjárócsomópontok.</span><span class="sxs-lookup"><span data-stu-id="ba413-147">Communication between Spark and Azure Cosmos DB is limited toohello Spark master node and Azure Cosmos DB gateway nodes.</span></span>  <span data-ttu-id="ba413-148">hello lekérdezések gyors hello transport layer a két csomópont között lehetővé teszi, hogy nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="ba413-148">hello queries go as fast as hello transport layer between these two nodes allows.</span></span>

### <a name="install-pydocumentdb"></a><span data-ttu-id="ba413-149">Telepítse a pyDocumentDB</span><span class="sxs-lookup"><span data-stu-id="ba413-149">Install pyDocumentDB</span></span>
<span data-ttu-id="ba413-150">Telepíthető pyDocumentDB az illesztőprogram-csomópont használatával **pip**, például:</span><span class="sxs-lookup"><span data-stu-id="ba413-150">You can install pyDocumentDB on your driver node by using **pip**, for example:</span></span>

```
pip install pyDocumentDB
```


### <a name="connect-spark-tooazure-cosmos-db-via-pydocumentdb"></a><span data-ttu-id="ba413-151">Csatlakozás a Spark tooAzure Cosmos DB pyDocumentDB keresztül</span><span class="sxs-lookup"><span data-stu-id="ba413-151">Connect Spark tooAzure Cosmos DB via pyDocumentDB</span></span>
<span data-ttu-id="ba413-152">hello kommunikációs átviteli hello jellemzője az egyszerűség viszonylag egyszerű pyDocumentDB használatával lehetővé teszi a Spark tooAzure Cosmos DB a lekérdezés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="ba413-152">hello simplicity of hello communication transport makes execution of a query from Spark tooAzure Cosmos DB by using pyDocumentDB relatively simple.</span></span>

<span data-ttu-id="ba413-153">a következő kódrészletben látható kód hogyan hello toouse pyDocumentDB Spark környezetben.</span><span class="sxs-lookup"><span data-stu-id="ba413-153">hello following code snippet shows how toouse pyDocumentDB in a Spark context.</span></span>

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

<span data-ttu-id="ba413-154">Hello kódrészletet leírtaknak megfelelően:</span><span class="sxs-lookup"><span data-stu-id="ba413-154">As noted in hello code snippet:</span></span>

* <span data-ttu-id="ba413-155">hello Azure Cosmos DB Python SDK-t (`pyDocumentDB`) hello tartalmazza az összes szükséges kapcsolódási paraméterek hello.</span><span class="sxs-lookup"><span data-stu-id="ba413-155">hello Azure Cosmos DB Python SDK (`pyDocumentDB`) contains hello all hello necessary connection parameters.</span></span> <span data-ttu-id="ba413-156">Például hello elsődleges helyek paraméter úgy dönt, hogy hello olvassa el a replika és a prioritási sorrendben.</span><span class="sxs-lookup"><span data-stu-id="ba413-156">For example, hello preferred locations parameter chooses hello read replica and priority order.</span></span>
*  <span data-ttu-id="ba413-157">Hello szükséges kódtárak importálja és konfigurálja a **főkulcsos** és **állomás** toocreate hello Azure Cosmos DB *ügyfél* (**pydocumentdb.document_ ügyfél**).</span><span class="sxs-lookup"><span data-stu-id="ba413-157">Import hello necessary libraries and configure your **masterKey** and **host** toocreate hello Azure Cosmos DB *client* (**pydocumentdb.document_client**).</span></span>


### <a name="execute-spark-queries-via-pydocumentdb"></a><span data-ttu-id="ba413-158">A Spark-lekérdezéseket hajt végre a pyDocumentDB keresztül</span><span class="sxs-lookup"><span data-stu-id="ba413-158">Execute Spark Queries via pyDocumentDB</span></span>
<span data-ttu-id="ba413-159">a következő példák használata hello Azure Cosmos DB példány hello előző részlet hello segítségével létrehozott hello megadott írásvédett kulcsok.</span><span class="sxs-lookup"><span data-stu-id="ba413-159">hello following examples use hello Azure Cosmos DB instance that was created in hello previous snippet by using hello specified read-only keys.</span></span> <span data-ttu-id="ba413-160">hello következő kódrészletet csatlakozik toohello **airports.codes** gyűjtemény hello DoctorWho fiók, mint a korábban meghatározott, és futtat egy lekérdezés tooextract hello repülőtéren városokat Washington államban.</span><span class="sxs-lookup"><span data-stu-id="ba413-160">hello following code snippet connects toohello **airports.codes** collection in hello DoctorWho account as specified earlier and runs a query tooextract hello airport cities in Washington state.</span></span>

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

<span data-ttu-id="ba413-161">Hello lekérdezés keresztül végrehajtása után **lekérdezés**, hello eredménye egy **query_iterable. QueryIterable** , amely a konvertált tooa Python listája.</span><span class="sxs-lookup"><span data-stu-id="ba413-161">After hello query has been executed via **query**, hello result is a **query_iterable.QueryIterable** that is converted tooa Python list.</span></span> <span data-ttu-id="ba413-162">A Python lista lehet könnyen konvertált tooa Spark DataFrame hello kód a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="ba413-162">A Python list can be easily converted tooa Spark DataFrame by using hello following code:</span></span>

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-hello-pydocumentdb-tooconnect-spark-tooazure-cosmos-db"></a><span data-ttu-id="ba413-163">Hello pyDocumentDB tooconnect Spark tooAzure Cosmos DB miért érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="ba413-163">Why use hello pyDocumentDB tooconnect Spark tooAzure Cosmos DB?</span></span>
<span data-ttu-id="ba413-164">Csatlakozás Spark tooAzure Cosmos DB pyDocumentDB használatával általában forgatókönyvek esetén az adott:</span><span class="sxs-lookup"><span data-stu-id="ba413-164">Connecting Spark tooAzure Cosmos DB by using pyDocumentDB is typically for scenarios where:</span></span>

* <span data-ttu-id="ba413-165">Azt szeretné, hogy a toouse Python.</span><span class="sxs-lookup"><span data-stu-id="ba413-165">You want toouse Python.</span></span>
* <span data-ttu-id="ba413-166">Az Azure Cosmos DB tooSpark viszonylag kis eredményhalmazt vannak ad vissza.</span><span class="sxs-lookup"><span data-stu-id="ba413-166">You are returning a relatively small result set from Azure Cosmos DB tooSpark.</span></span> <span data-ttu-id="ba413-167">Vegye figyelembe, hogy az Azure Cosmos Adatbázisba az alapul szolgáló dataset hello Igen tekintélyes lehet.</span><span class="sxs-lookup"><span data-stu-id="ba413-167">Note that hello underlying dataset in Azure Cosmos DB can be quite large.</span></span> <span data-ttu-id="ba413-168">Szűrők, ez azt jelenti, hogy az Azure Cosmos DB adatforrás predikátum szűrők futtatott alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="ba413-168">You are applying filters, that is, running predicate filters, against your Azure Cosmos DB source.</span></span>  

## <a name="spark-tooazure-cosmos-db-connector"></a><span data-ttu-id="ba413-169">Spark tooAzure Cosmos DB összekötő</span><span class="sxs-lookup"><span data-stu-id="ba413-169">Spark tooAzure Cosmos DB connector</span></span>

<span data-ttu-id="ba413-170">hello Spark tooAzure Cosmos DB összekötőt használja hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) és mozgatja az adatokat hello Spark munkavégző csomópontokhoz és Azure Cosmos DB között, ahogy az ábra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ba413-170">hello Spark tooAzure Cosmos DB connector utilizes hello [Azure DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java) and moves data between hello Spark worker nodes and Azure Cosmos DB as shown in hello following diagram:</span></span>

![Hello Spark tooAzure Cosmos DB Connector adatfolyama](./media/spark-connector/spark-connector.png)

<span data-ttu-id="ba413-172">hello adatáramlás a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="ba413-172">hello data flow is as follows:</span></span>

1. <span data-ttu-id="ba413-173">hello Spark főcsomópont toohello Azure Cosmos DB átjáró csomópont tooobtain hello partíciótérképen csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="ba413-173">hello Spark master node connects toohello Azure Cosmos DB gateway node tooobtain hello partition map.</span></span> <span data-ttu-id="ba413-174">A felhasználó csak hello Spark és Azure Cosmos DB-kapcsolatok ad meg.</span><span class="sxs-lookup"><span data-stu-id="ba413-174">A user specifies only hello Spark and Azure Cosmos DB connections.</span></span> <span data-ttu-id="ba413-175">Kapcsolatok toohello megfelelő master és átjáró-csomópontok átlátszó toohello felhasználókként szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="ba413-175">Connections toohello respective master and gateway nodes are transparent toohello user.</span></span>
2. <span data-ttu-id="ba413-176">Ez az információ hátsó toohello Spark főcsomópont valósul meg.</span><span class="sxs-lookup"><span data-stu-id="ba413-176">This information is provided back toohello Spark master node.</span></span>  <span data-ttu-id="ba413-177">Ezen a ponton képes tooparse hello lekérdezési toodetermine hello partíciók és az Azure Cosmos-Adatbázisba, hogy kell-e tooaccess helyükre kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ba413-177">At this point, you should be able tooparse hello query toodetermine hello partitions and their locations in Azure Cosmos DB that you need tooaccess.</span></span>
3. <span data-ttu-id="ba413-178">Ez az információ az átvitt toohello Spark munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="ba413-178">This information is transmitted toohello Spark worker nodes.</span></span>
4. <span data-ttu-id="ba413-179">hello Spark munkavégző csomópontok toohello Azure Cosmos DB partíciók csatlakoznak, közvetlen tooextract hello adatokat, és vissza hello adatok toohello Spark partíciók hello Spark munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="ba413-179">hello Spark worker nodes connect toohello Azure Cosmos DB partitions directly tooextract hello data and return hello data toohello Spark partitions in hello Spark worker nodes.</span></span>

<span data-ttu-id="ba413-180">Spark és Azure Cosmos DB közötti kommunikáció oka jelentősen gyorsabb adatátvitelt jelölik a hello hello Spark munkavégző csomópontokhoz és hello Azure Cosmos DB adatok csomópontok (partíciók) között.</span><span class="sxs-lookup"><span data-stu-id="ba413-180">Communication between Spark and Azure Cosmos DB is significantly faster because hello data movement is between hello Spark worker nodes and hello Azure Cosmos DB data nodes (partitions).</span></span>

### <a name="build-hello-spark-tooazure-cosmos-db-connector"></a><span data-ttu-id="ba413-181">Hello Spark tooAzure Cosmos DB összekötő létrehozása</span><span class="sxs-lookup"><span data-stu-id="ba413-181">Build hello Spark tooAzure Cosmos DB connector</span></span>
<span data-ttu-id="ba413-182">Jelenleg a hello összekötő projektet maven használja.</span><span class="sxs-lookup"><span data-stu-id="ba413-182">Currently, hello connector project uses maven.</span></span> <span data-ttu-id="ba413-183">toobuild hello összekötő függőségek nélkül is futtathatja:</span><span class="sxs-lookup"><span data-stu-id="ba413-183">toobuild hello connector without dependencies, you can run:</span></span>
```
mvn clean package
```
<span data-ttu-id="ba413-184">Hello hello JAR legújabb verzióját a hello is letölthető *kiadott* mappát.</span><span class="sxs-lookup"><span data-stu-id="ba413-184">You can also download hello latest versions of hello JAR from hello *releases* folder.</span></span>

### <a name="include-hello-azure-cosmos-db-spark-jar"></a><span data-ttu-id="ba413-185">Hello Azure Cosmos DB külső JAR tartalmazza</span><span class="sxs-lookup"><span data-stu-id="ba413-185">Include hello Azure Cosmos DB Spark JAR</span></span>
<span data-ttu-id="ba413-186">Kód futtatása előtt kell tooinclude hello Azure Cosmos DB külső JAR.</span><span class="sxs-lookup"><span data-stu-id="ba413-186">Before you execute any code, you need tooinclude hello Azure Cosmos DB Spark JAR.</span></span>  <span data-ttu-id="ba413-187">Hello használata **spark-rendszerhéj**, majd hello segítségével megadhat hello JAR **--JAR-fájlok kivételével** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ba413-187">If you are using hello **spark-shell**, then you can include hello JAR by using hello **--jars** option.</span></span>  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

<span data-ttu-id="ba413-188">Tooexecute hello JAR függőségek nélkül, használja a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="ba413-188">If you want tooexecute hello JAR without dependencies, use hello following code:</span></span>

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

<span data-ttu-id="ba413-189">A notebook szolgáltatással, például az Azure HDInsight Jupyter notebook szolgáltatás használatakor hello használhatja **spark magic** parancsokat:</span><span class="sxs-lookup"><span data-stu-id="ba413-189">If you are using a notebook service such as Azure HDInsight Jupyter notebook service, you can use hello **spark magic** commands:</span></span>

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

<span data-ttu-id="ba413-190">Hello **JAR-fájlok kivételével** parancs lehetővé teszi, hogy tooinclude hello két JARs, amelyek szükségesek a **azure-cosmosdb-spark** (saját magát és hello Azure DocumentDB Java SDK-t), és amelyeket **scala-megfelelően** , hogy ne zavarja Livy meghívja hello (Jupyter notebook > Livy > Spark).</span><span class="sxs-lookup"><span data-stu-id="ba413-190">hello **jars** command enables you tooinclude hello two JARs that are needed for **azure-cosmosdb-spark** (itself and hello Azure DocumentDB Java SDK) and exclude **scala-reflect** so that it does not interfere with hello Livy calls (Jupyter notebook > Livy > Spark).</span></span>

### <a name="connect-spark-tooazure-cosmos-db-using-hello-connector"></a><span data-ttu-id="ba413-191">Csatlakozás a Spark tooAzure Cosmos DB használatával hello összekötő</span><span class="sxs-lookup"><span data-stu-id="ba413-191">Connect Spark tooAzure Cosmos DB using hello connector</span></span>
<span data-ttu-id="ba413-192">Bár hello kommunikációs átviteli egy kicsit bonyolultabb, a lekérdezés végrehajtása a Spark tooAzure Cosmos DB hello-összekötő használatával is jelentősen gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="ba413-192">Although hello communication transport is a little more complicated, executing a query from Spark tooAzure Cosmos DB by using hello connector is significantly faster.</span></span>

<span data-ttu-id="ba413-193">a következő kódrészletet hello bemutatja, hogyan toouse hello összekötő Spark környezetben.</span><span class="sxs-lookup"><span data-stu-id="ba413-193">hello following code snippet shows how toouse hello connector in a Spark context.</span></span>

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

<span data-ttu-id="ba413-194">Hello kódrészletet leírtaknak megfelelően:</span><span class="sxs-lookup"><span data-stu-id="ba413-194">As noted in hello code snippet:</span></span>

- <span data-ttu-id="ba413-195">**Azure-cosmosdb-spark** hello tartalmazza az összes szükséges kapcsolódási paraméterek, többek között hello elsődleges helyek hello.</span><span class="sxs-lookup"><span data-stu-id="ba413-195">**azure-cosmosdb-spark** contains hello all hello necessary connection parameters, which include hello preferred locations.</span></span> <span data-ttu-id="ba413-196">Kiválaszthatja például hello olvassa el a replika és a prioritási sorrendben.</span><span class="sxs-lookup"><span data-stu-id="ba413-196">For example, you can choose hello read replica and priority order.</span></span>
- <span data-ttu-id="ba413-197">Ebben az esetben hello szükséges könyvtárak importálása, és a főkulcsos és a gazdagép toocreate hello Azure Cosmos DB-ügyfél konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ba413-197">Just import hello necessary libraries and configure your masterKey and host toocreate hello Azure Cosmos DB client.</span></span>

### <a name="execute-spark-queries-via-hello-connector"></a><span data-ttu-id="ba413-198">Hajtsa végre a Spark lekérdezéseket hello összekötőn keresztül</span><span class="sxs-lookup"><span data-stu-id="ba413-198">Execute Spark queries via hello connector</span></span>

<span data-ttu-id="ba413-199">a következő példa használ hello Azure Cosmos DB példány hello előző részlet hello segítségével létrehozott hello megadott írásvédett kulcsok.</span><span class="sxs-lookup"><span data-stu-id="ba413-199">hello following example uses hello Azure Cosmos DB instance that was created in hello previous snippet by using hello specified read-only keys.</span></span> <span data-ttu-id="ba413-200">hello következő kódrészletet toohello DepartureDelays.flights_pcoll gyűjtemény (a korábban megadott DoctorWho fiók hello) csatlakozik, és futtat egy lekérdezés tooextract hello repülési késési információkat, amelyek a budapesti vannak induló légijáratok.</span><span class="sxs-lookup"><span data-stu-id="ba413-200">hello following code snippet connects toohello DepartureDelays.flights_pcoll collection (in hello DoctorWho account as specified earlier) and runs a query tooextract hello flight delay information of flights that are departing from Seattle.</span></span>

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-hello-spark-tooazure-cosmos-db-connector-implementation"></a><span data-ttu-id="ba413-201">Hello Spark tooAzure Cosmos DB összekötő megvalósítási miért érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="ba413-201">Why use hello Spark tooAzure Cosmos DB connector implementation?</span></span>

<span data-ttu-id="ba413-202">Csatlakozás Spark tooAzure Cosmos DB hello összekötővel általában forgatókönyvek van ahol:</span><span class="sxs-lookup"><span data-stu-id="ba413-202">Connecting Spark tooAzure Cosmos DB by using hello connector is typically for scenarios where:</span></span>

* <span data-ttu-id="ba413-203">Szeretné, hogy toouse Scala és tooinclude egy Python-burkoló leírtaknak megfelelően a frissítést [probléma 3: hozzáadása Python burkoló és példák](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span><span class="sxs-lookup"><span data-stu-id="ba413-203">You want toouse Scala and update it tooinclude a Python wrapper as noted in [Issue 3: Add Python wrapper and examples](https://github.com/Azure/azure-cosmosdb-spark/issues/3).</span></span>
* <span data-ttu-id="ba413-204">Nagy mennyiségű adatok tootransfer között az Apache Spark on Azure Cosmos DB rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ba413-204">You have a large amount of data tootransfer between Apache Spark and Azure Cosmos DB.</span></span>

<span data-ttu-id="ba413-205">toogive egy meghatározni, hello lekérdezés teljesítménybeli különbség az, hogy látja hello [lekérdezés teszt futtatása wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span><span class="sxs-lookup"><span data-stu-id="ba413-205">toogive you an idea of hello query performance difference, see hello [Query Test Runs wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).</span></span>

## <a name="distributed-aggregation-example"></a><span data-ttu-id="ba413-206">Elosztott összesítési – példa</span><span class="sxs-lookup"><span data-stu-id="ba413-206">Distributed aggregation example</span></span>
<span data-ttu-id="ba413-207">Ez a témakör néhány példa arra, hogyan lehet úgy teheti meg, elosztott összesítések és az elemzések Apache Spark- és Azure Cosmos DB együtt.</span><span class="sxs-lookup"><span data-stu-id="ba413-207">This section provides some examples of how you can do distributed aggregations and analytics by using Apache Spark and Azure Cosmos DB together.</span></span> <span data-ttu-id="ba413-208">Azure Cosmos-adatbázis már támogatja az összesítéseket, amely hello ismertet [bolygónk méretezési összesíti az Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span><span class="sxs-lookup"><span data-stu-id="ba413-208">Azure Cosmos DB already supports aggregations, which is discussed in hello [Planet scale aggregates with Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/).</span></span> <span data-ttu-id="ba413-209">Ez hogyan-EK toohello mellett az Apache Spark szint.</span><span class="sxs-lookup"><span data-stu-id="ba413-209">Here is how you can take it toohello next level with Apache Spark.</span></span>

<span data-ttu-id="ba413-210">Vegye figyelembe, hogy ezeket az összesítéseket a hivatkozás toohello [Spark tooAzure Cosmos DB összekötő notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span><span class="sxs-lookup"><span data-stu-id="ba413-210">Note that these aggregations are in reference toohello [Spark tooAzure Cosmos DB Connector notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).</span></span>

### <a name="connect-tooflights-sample-data"></a><span data-ttu-id="ba413-211">Csatlakozás tooflights mintaadatok</span><span class="sxs-lookup"><span data-stu-id="ba413-211">Connect tooflights sample data</span></span>
<span data-ttu-id="ba413-212">Összesítési példákban elérni bizonyos repülési teljesítményadatokat tárolt a **DoctorWho** Azure Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="ba413-212">These aggregation examples access some flight performance data that's stored in our **DoctorWho** Azure Cosmos DB database.</span></span> <span data-ttu-id="ba413-213">tooconnect tooit kell tooutilize hello a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="ba413-213">tooconnect tooit, you need tooutilize hello following code snippet:</span></span>

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

<span data-ttu-id="ba413-214">Ezt a kódrészletet a dolgozunk is folyamatos toorun, hogy az átvitel hello Azure Cosmos DB tooSpark származó adatok szűrt set alap lekérdezés ahol ez utóbbi hello végezhet elosztott összesítések.</span><span class="sxs-lookup"><span data-stu-id="ba413-214">With this snippet, we are also going toorun a base query that transfers hello filtered set of data from Azure Cosmos DB tooSpark where hello latter can perform distributed aggregates.</span></span> <span data-ttu-id="ba413-215">Ebben az esetben arra kérjük, hogy a budapesti (SZE) egymástól légijáratoknál.</span><span class="sxs-lookup"><span data-stu-id="ba413-215">In this case, we are asking for flights that depart from Seattle (SEA).</span></span>

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

<span data-ttu-id="ba413-216">hello következő eredmények által előállított hello lekérdezések futtatása Jupyter notebook szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="ba413-216">hello following results were generated by running hello queries from hello Jupyter notebook service.</span></span>  <span data-ttu-id="ba413-217">Ne feledje, hogy minden hello kódtöredékek általános és nem adott tooany szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ba413-217">Note that all hello code snippets are generic and not specific tooany service.</span></span>

### <a name="running-limit-and-count-queries"></a><span data-ttu-id="ba413-218">Futó korlát és száma</span><span class="sxs-lookup"><span data-stu-id="ba413-218">Running LIMIT and COUNT queries</span></span>
<span data-ttu-id="ba413-219">Csak Ön például használt tooin SQL/Spark SQL, most kezdetben a **korlát** lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="ba413-219">Just like you're used tooin SQL/Spark SQL, let's start off with a **LIMIT** query:</span></span>

![Spark korlát lekérdezés](./media/spark-connector/spark-sql-query.png)

<span data-ttu-id="ba413-221">hello következő lekérdezésben van egy egyszerű és gyors **száma** lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="ba413-221">hello next query is a simple and fast **COUNT** query:</span></span>

![Spark COUNT lekérdezés](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a><span data-ttu-id="ba413-223">A GROUP BY lekérdezés</span><span class="sxs-lookup"><span data-stu-id="ba413-223">GROUP BY query</span></span>
<span data-ttu-id="ba413-224">A következő be van állítva, hogy könnyen futtatható az **GROUP BY** lekérdezések írásában, az Azure Cosmos DB adatbázishoz:</span><span class="sxs-lookup"><span data-stu-id="ba413-224">In this next set, we can easily run **GROUP BY** queries against our Azure Cosmos DB database:</span></span>

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY lekérdezés diagramhoz](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a><span data-ttu-id="ba413-226">KÜLÖNÁLLÓ, ORDER BY lekérdezés</span><span class="sxs-lookup"><span data-stu-id="ba413-226">DISTINCT, ORDER BY query</span></span>
<span data-ttu-id="ba413-227">Itt egy **DISTINCT, ORDER BY** lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="ba413-227">And here is a **DISTINCT, ORDER BY** query:</span></span>

![Spark GROUP BY lekérdezés diagramhoz](./media/spark-connector/order-by-query.png)

### <a name="continue-hello-flight-data-analysis"></a><span data-ttu-id="ba413-229">Továbbra is hello felé továbbított adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="ba413-229">Continue hello flight data analysis</span></span>
<span data-ttu-id="ba413-230">A következő példa lekérdezések toocontinue hello felé továbbított adatok elemzése hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="ba413-230">You can use hello following example queries toocontinue analysis of hello flight data:</span></span>

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a><span data-ttu-id="ba413-231">Leggyakoribb 5 késleltetett célok (városokat) onnan induló Budapest</span><span class="sxs-lookup"><span data-stu-id="ba413-231">Top 5 delayed destinations (cities) departing from Seattle</span></span>
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark felső késések diagramhoz](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a><span data-ttu-id="ba413-233">Közepes késések által a budapesti induló cél városokat kiszámítása</span><span class="sxs-lookup"><span data-stu-id="ba413-233">Calculate median delays by destination cities departing from Seattle</span></span>
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark közepes késések diagramhoz](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a><span data-ttu-id="ba413-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba413-235">Next steps</span></span>

<span data-ttu-id="ba413-236">Ha még nem tette meg, le hello Spark tooAzure Cosmos DB összekötő hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-tárházban és további források hello hello tárházban felfedezése:</span><span class="sxs-lookup"><span data-stu-id="ba413-236">If you haven't already, download hello Spark tooAzure Cosmos DB connector from hello [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub repository and explore hello additional resources in hello repo:</span></span>

* [<span data-ttu-id="ba413-237">Elosztott összesítések példák</span><span class="sxs-lookup"><span data-stu-id="ba413-237">Distributed Aggregations Examples</span></span>](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [<span data-ttu-id="ba413-238">Mintaparancsfájlok és notebookok</span><span class="sxs-lookup"><span data-stu-id="ba413-238">Sample Scripts and Notebooks</span></span>](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

<span data-ttu-id="ba413-239">Érdemes lehet tooreview hello [Apache Spark SQL, DataFrames és adatkészletek útmutató](http://spark.apache.org/docs/latest/sql-programming-guide.html) és hello [Apache Spark on Azure Hdinsighttal](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ba413-239">You might also want tooreview hello [Apache Spark SQL, DataFrames, and Datasets Guide](http://spark.apache.org/docs/latest/sql-programming-guide.html) and hello [Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) article.</span></span>
