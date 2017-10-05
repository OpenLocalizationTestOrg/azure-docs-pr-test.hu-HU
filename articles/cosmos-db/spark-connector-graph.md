---
title: "Azure Cosmos DB: Graph elemzés végrehajtása a Spark és Apache TinkerPop Gremlin használatával |} Microsoft Docs"
description: "Ez a cikk bemutatja beállíthatja és futtathatja a graph elemzés és párhuzamos számítási az Azure Cosmos Adatbázisba Spark és TinkerPop SparkGraphComputer vonatkozó utasításokat."
services: cosmosdb
documentationcenter: 
author: khdang
manager: shireest
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: gremlin
ms.topic: article
ms.date: 06/05/2017
ms.author: khdang
ms.openlocfilehash: 27c4d945e418b130c68cfde845571eb93658101e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a><span data-ttu-id="fbfb0-103">Azure Cosmos DB: Graph elemzés végrehajtása a Spark és Apache TinkerPop Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="fbfb0-103">Azure Cosmos DB: Perform graph analytics by using Spark and Apache TinkerPop Gremlin</span></span>

<span data-ttu-id="fbfb0-104">[Az Azure Cosmos DB](introduction.md) Microsoft globálisan elosztott, több modellre adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-104">[Azure Cosmos DB](introduction.md) is the globally distributed, multi-model database service from Microsoft.</span></span> <span data-ttu-id="fbfb0-105">Hozzon létre, és a dokumentum, a kulcs/érték és a graph-adatbázisok mindegyike kihasználhassa a globális terjesztési és vízszintes méretű képességek Azure Cosmos DB középpontjában lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-105">You can create and query document, key/value, and graph databases, all of which benefit from the global-distribution and horizontal-scale capabilities at the core of Azure Cosmos DB.</span></span> <span data-ttu-id="fbfb0-106">Az Azure Cosmos DB támogatja az online tranzakció-feldolgozási (OLTP) graph használó munkaterhelések [Apache TinkerPop Gremlin](graph-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fbfb0-106">Azure Cosmos DB supports online transaction processing (OLTP) graph workloads that use [Apache TinkerPop Gremlin](graph-introduction.md).</span></span>

<span data-ttu-id="fbfb0-107">[Spark](http://spark.apache.org/) Apache szoftver Foundation projekt, amely az általános célú online analitikus feldolgozási (OLAP) adatok feldolgozási összpontosít.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-107">[Spark](http://spark.apache.org/) is an Apache Software Foundation project that's focused on general-purpose online analytical processing (OLAP) data processing.</span></span> <span data-ttu-id="fbfb0-108">Spark hibrid a memória vagy lemez-alapú elosztott számítási modellt biztosít a Hadoop-MapReduce-modell hasonló.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-108">Spark provides a hybrid in-memory/disk-based distributed computing model that is similar to the Hadoop MapReduce model.</span></span> <span data-ttu-id="fbfb0-109">A felhőbeli Apache Spark on segítségével telepíthet [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="fbfb0-109">You can deploy Apache Spark in the cloud by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="fbfb0-110">Azure Cosmos DB és Spark kombinálásával végezheti el az OLTP és OLAP munkaterhelések Gremlin használatakor.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-110">By combining Azure Cosmos DB and Spark, you can perform both OLTP and OLAP workloads when you use Gremlin.</span></span> <span data-ttu-id="fbfb0-111">A gyors üzembe helyezési cikk bemutatja, hogyan Azure Cosmos DB Gremlin lekérdezéseinek futtatásához egy Azure HDInsight Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-111">This quick-start article demonstrates how to run Gremlin queries against Azure Cosmos DB on an Azure HDInsight Spark cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbfb0-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fbfb0-112">Prerequisites</span></span>

<span data-ttu-id="fbfb0-113">Mielőtt futtathatná ezt a mintát, rendelkeznie kell a következő előfeltételekkel:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-113">Before you can run this sample, you must have the following prerequisites:</span></span>
* <span data-ttu-id="fbfb0-114">Az Azure HDInsight Spark-fürt 2.0</span><span class="sxs-lookup"><span data-stu-id="fbfb0-114">Azure HDInsight Spark cluster 2.0</span></span>
* <span data-ttu-id="fbfb0-115">JDK 1.8 + (Ha még nem rendelkezik JDK, futtassa `apt-get install default-jdk`.)</span><span class="sxs-lookup"><span data-stu-id="fbfb0-115">JDK 1.8+ (If you don't have JDK, run `apt-get install default-jdk`.)</span></span>
* <span data-ttu-id="fbfb0-116">Maven (Ha még nem rendelkezik Maven, `apt-get install maven`.)</span><span class="sxs-lookup"><span data-stu-id="fbfb0-116">Maven (If you don't have Maven, run `apt-get install maven`.)</span></span>
* <span data-ttu-id="fbfb0-117">Azure-előfizetés ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span><span class="sxs-lookup"><span data-stu-id="fbfb0-117">An Azure subscription ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span></span>

<span data-ttu-id="fbfb0-118">Egy Azure HDInsight Spark-fürt beállításával kapcsolatos információkért lásd: [kiépítés HDInsight-fürtök](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="fbfb0-118">For information about how to set up an Azure HDInsight Spark cluster, see [Provisioning HDInsight clusters](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="fbfb0-119">Egy Azure Cosmos DB adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbfb0-119">Create an Azure Cosmos DB database account</span></span>

<span data-ttu-id="fbfb0-120">Először hozzon létre egy adatbázis-fiók a Graph API-t a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-120">First, create a database account with the Graph API by doing the following:</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="fbfb0-121">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fbfb0-121">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a><span data-ttu-id="fbfb0-122">Apache TinkerPop beolvasása</span><span class="sxs-lookup"><span data-stu-id="fbfb0-122">Get Apache TinkerPop</span></span>

<span data-ttu-id="fbfb0-123">Apache TinkerPop érhető el a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-123">Get Apache TinkerPop by doing the following:</span></span>

1. <span data-ttu-id="fbfb0-124">A fő csomópontra a HDInsight-fürt távoli `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-124">Remote to the master node of the HDInsight cluster `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span></span>

2. <span data-ttu-id="fbfb0-125">Klónozza a TinkerPop3 forráskódot, összeállítani, helyileg, és telepítse Maven-gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-125">Clone the TinkerPop3 source code, build it locally, and install it to Maven cache.</span></span>

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. <span data-ttu-id="fbfb0-126">A Spark-Gremlin beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="fbfb0-126">Install the Spark-Gremlin plug-in</span></span> 

    <span data-ttu-id="fbfb0-127">a.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-127">a.</span></span> <span data-ttu-id="fbfb0-128">A telepítés, a beépülő modul szőlőmust kezeli.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-128">The installation of the plug-in is handled by Grape.</span></span> <span data-ttu-id="fbfb0-129">A tárolóhelyekkel kapcsolatos információi szőlőmust, hogy letölthesse a beépülő modult és annak függőségeit feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-129">Populate the repositories information for Grape so it can download the plug-in and its dependencies.</span></span> 

      <span data-ttu-id="fbfb0-130">A szőlőmust konfigurációs fájl létrehozása, ha nincs jelen az `~/.groovy/grapeConfig.xml`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-130">Create the grape configuration file if it's not present at `~/.groovy/grapeConfig.xml`.</span></span> <span data-ttu-id="fbfb0-131">A következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-131">Use the following settings:</span></span>

    ```xml
    <ivysettings>
    <settings defaultResolver="downloadGrapes"/>
    <resolvers>
        <chain name="downloadGrapes">
        <filesystem name="cachedGrapes">
            <ivy pattern="${user.home}/.groovy/grapes/[organisation]/[module]/ivy-[revision].xml"/>
            <artifact pattern="${user.home}/.groovy/grapes/[organisation]/[module]/[type]s/[artifact]-[revision].[ext]"/>
        </filesystem>
        <ibiblio name="codehaus" root="http://repository.codehaus.org/" m2compatible="true"/>
        <ibiblio name="central" root="http://central.maven.org/maven2/" m2compatible="true"/>
        <ibiblio name="jitpack" root="https://jitpack.io" m2compatible="true"/>
        <ibiblio name="java.net2" root="http://download.java.net/maven/2/" m2compatible="true"/>
        <ibiblio name="apache-snapshots" root="http://repository.apache.org/snapshots/" m2compatible="true"/>
        <ibiblio name="local" root="file:${user.home}/.m2/repository/" m2compatible="true"/>
        </chain>
    </resolvers>
    </ivysettings>
    ``` 

    <span data-ttu-id="fbfb0-132">b.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-132">b.</span></span> <span data-ttu-id="fbfb0-133">Indítsa el a Gremlin konzol `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-133">Start Gremlin console `bin/gremlin.sh`.</span></span>
        
    <span data-ttu-id="fbfb0-134">c.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-134">c.</span></span> <span data-ttu-id="fbfb0-135">A Spark-Gremlin beépülő modul telepítése verziójával 3.3.0-SNAPSHOT, amely az előző lépésben parancsfájlkezelő:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-135">Install the Spark-Gremlin plug-in with version 3.3.0-SNAPSHOT, which you built in the previous steps:</span></span>

    ```bash
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :install org.apache.tinkerpop spark-gremlin 3.3.0-SNAPSHOT
    ==>loaded: [org.apache.tinkerpop, spark-gremlin, 3.3.0-SNAPSHOT] - restart the console to use [tinkerpop.spark]
    gremlin> :q
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :plugin use tinkerpop.spark
    ==>tinkerpop.spark activated
    ```

4. <span data-ttu-id="fbfb0-136">Ellenőrizze, hogy `Hadoop-Gremlin` , aktivált `:plugin list`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-136">Check to see whether `Hadoop-Gremlin` is activated with `:plugin list`.</span></span> <span data-ttu-id="fbfb0-137">Ez a beépülő modul, mert azt sikerült zavarják a Spark-Gremlin beépülő modul letiltása `:plugin unuse tinkerpop.hadoop`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-137">Disable this plug-in, because it could interfere with the Spark-Gremlin plug-in `:plugin unuse tinkerpop.hadoop`.</span></span>

## <a name="prepare-tinkerpop3-dependencies"></a><span data-ttu-id="fbfb0-138">Készítse elő a TinkerPop3 függőségek</span><span class="sxs-lookup"><span data-stu-id="fbfb0-138">Prepare TinkerPop3 dependencies</span></span>

<span data-ttu-id="fbfb0-139">Ha az előző lépésben a TinkerPop3 beépített, a folyamat is lekért összes jar-függőség a célkönyvtár a Spark- és a Hadoop.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-139">When you built TinkerPop3 in the previous step, the process also pulled all jar dependencies for Spark and Hadoop in the target directory.</span></span> <span data-ttu-id="fbfb0-140">Használja a JAR-fájlok, amelyek előre HDI együtt vannak telepítve, és lekéréses a további függőségek csak szükség szerint kivételével.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-140">Use the jars that are pre-installed with HDI, and pull in additional dependencies only as necessary.</span></span>

1. <span data-ttu-id="fbfb0-141">Keresse fel a Gremlin konzol tároló könyvtár: `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-141">Go to the Gremlin Console target directory at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span></span> 

2. <span data-ttu-id="fbfb0-142">Helyezze át az összes JAR-fájlok kivételével a `ext/` való `lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-142">Move all jars under `ext/` to `lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span></span>

3. <span data-ttu-id="fbfb0-143">Távolítsa el a szalagtárak jar összes `lib/` , amelyek nem a következők:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-143">Remove all jar libraries under `lib/` that are not in the following list:</span></span>

    ```bash
    # TinkerPop3
    gremlin-console-3.3.0-SNAPSHOT.jar
    gremlin-core-3.3.0-SNAPSHOT.jar       
    gremlin-groovy-3.3.0-SNAPSHOT.jar     
    gremlin-shaded-3.3.0-SNAPSHOT.jar     
    hadoop-gremlin-3.3.0-SNAPSHOT.jar     
    spark-gremlin-3.3.0-SNAPSHOT.jar      
    tinkergraph-gremlin-3.3.0-SNAPSHOT.jar

    # Gremlin depedencies
    asm-3.2.jar                                
    avro-1.7.4.jar                             
    caffeine-2.3.1.jar                         
    cglib-2.2.1-v20090111.jar                  
    gbench-0.4.3-groovy-2.4.jar                
    gprof-0.3.1-groovy-2.4.jar                 
    groovy-2.4.9-indy.jar                      
    groovy-2.4.9.jar                           
    groovy-console-2.4.9.jar                   
    groovy-groovysh-2.4.9-indy.jar             
    groovy-json-2.4.9-indy.jar                 
    groovy-jsr223-2.4.9-indy.jar               
    groovy-sql-2.4.9-indy.jar                  
    groovy-swing-2.4.9.jar                     
    groovy-templates-2.4.9.jar                 
    groovy-xml-2.4.9.jar                       
    hadoop-yarn-server-nodemanager-2.7.2.jar   
    hppc-0.7.1.jar                             
    javatuples-1.2.jar                         
    jaxb-impl-2.2.3-1.jar                      
    jbcrypt-0.4.jar                            
    jcabi-log-0.14.jar                         
    jcabi-manifests-1.1.jar                    
    jersey-core-1.9.jar                        
    jersey-guice-1.9.jar                       
    jersey-json-1.9.jar                        
    jettison-1.1.jar                           
    scalatest_2.11-2.2.6.jar                   
    servlet-api-2.5.jar                        
    snakeyaml-1.15.jar                         
    unused-1.0.0.jar                           
    xml-apis-1.3.04.jar                        
    ```

## <a name="get-the-azure-cosmos-db-spark-connector"></a><span data-ttu-id="fbfb0-144">Az Azure Cosmos DB Spark-összekötő beolvasása</span><span class="sxs-lookup"><span data-stu-id="fbfb0-144">Get the Azure Cosmos DB Spark connector</span></span>

1. <span data-ttu-id="fbfb0-145">Az Azure Cosmos DB Spark-összekötő beolvasása `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` és Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` a [Azure Cosmos DB Spark-összekötő a Githubon](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span><span class="sxs-lookup"><span data-stu-id="fbfb0-145">Get the Azure Cosmos DB Spark connector `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` and Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` from [Azure Cosmos DB Spark Connector on GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span></span>

2. <span data-ttu-id="fbfb0-146">Másik lehetőségként hozhat létre, helyileg.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-146">Alternatively, you can build it locally.</span></span> <span data-ttu-id="fbfb0-147">A Spark-Gremlin legújabb verzióját a következővel történt 1.6.1-es Spark, és nem kompatibilis a 2.0.2, Spark az Azure Cosmos DB Spark-összekötő jelenleg használt, mert a legújabb TinkerPop3 kód lefordításához, és manuálisan telepíti a JAR-fájlok kivételével.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-147">Because the latest version of Spark-Gremlin was built with Spark 1.6.1 and is not compatible with Spark 2.0.2, which is currently used in the Azure Cosmos DB Spark connector, you can build the latest TinkerPop3 code and install the jars manually.</span></span> <span data-ttu-id="fbfb0-148">Tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-148">Do the following:</span></span>

    <span data-ttu-id="fbfb0-149">a.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-149">a.</span></span> <span data-ttu-id="fbfb0-150">Az Azure Cosmos DB Spark-összekötő klónozását.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-150">Clone the Azure Cosmos DB Spark connector.</span></span>

    <span data-ttu-id="fbfb0-151">b.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-151">b.</span></span> <span data-ttu-id="fbfb0-152">Build TinkerPop3 (a korábbi lépésekben tette meg).</span><span class="sxs-lookup"><span data-stu-id="fbfb0-152">Build TinkerPop3 (already done in previous steps).</span></span> <span data-ttu-id="fbfb0-153">Telepítse az összes TinkerPop 3.3.0-SNAPSHOT JAR-fájlok kivételével helyileg.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-153">Install all TinkerPop 3.3.0-SNAPSHOT jars locally.</span></span>

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    <span data-ttu-id="fbfb0-154">c.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-154">c.</span></span> <span data-ttu-id="fbfb0-155">Frissítés `tinkerpop.version` `azure-documentdb-spark/pom.xml` való `3.3.0-SNAPSHOT`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-155">Update `tinkerpop.version` `azure-documentdb-spark/pom.xml` to `3.3.0-SNAPSHOT`.</span></span>
    
    <span data-ttu-id="fbfb0-156">d.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-156">d.</span></span> <span data-ttu-id="fbfb0-157">A Maven build.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-157">Build with Maven.</span></span> <span data-ttu-id="fbfb0-158">A szükséges JAR-fájlok kivételével kerülnek `target` és `target/alternateLocation`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-158">The needed jars are placed in `target` and `target/alternateLocation`.</span></span>

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. <span data-ttu-id="fbfb0-159">Másolja a korábban említett JAR-fájlok kivételével egy helyi, ~ / azure-documentdb-spark:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-159">Copy the previously mentioned jars to a local directory at ~/azure-documentdb-spark:</span></span>

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-the-dependencies-to-the-spark-worker-nodes"></a><span data-ttu-id="fbfb0-160">A Spark munkavégző csomópontokhoz függőségek terjesztése</span><span class="sxs-lookup"><span data-stu-id="fbfb0-160">Distribute the dependencies to the Spark worker nodes</span></span> 

1. <span data-ttu-id="fbfb0-161">Mivel az graph adatok átalakítása TinkerPop3 függ, el kell juttatnia minden Spark munkavégző csomópontokhoz kapcsolódó függőségek.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-161">Because the transformation of graph data depends on TinkerPop3, you must distribute the related dependencies to all Spark worker nodes.</span></span>

2. <span data-ttu-id="fbfb0-162">Másolja a korábban említett Gremlin függőségek, a CosmosDB Spark összekötő jar és CosmosDB Java SDK a feldolgozó csomópontok a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-162">Copy the previously mentioned Gremlin dependencies, the CosmosDB Spark connector jar, and CosmosDB Java SDK to the worker nodes by doing the following:</span></span>

    <span data-ttu-id="fbfb0-163">a.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-163">a.</span></span> <span data-ttu-id="fbfb0-164">Másolja az összes a JAR-fájlok kivételével az `~/azure-documentdb-spark`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-164">Copy all the jars into `~/azure-documentdb-spark`.</span></span>

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    <span data-ttu-id="fbfb0-165">b.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-165">b.</span></span> <span data-ttu-id="fbfb0-166">A Spark feldolgozó csomópontjaihoz, amely az Ambari irányítópultján a listájának lekérdezése a `Spark2 Clients` listájában a `Spark2` szakasz.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-166">Get the list of all Spark worker nodes, which you can find on Ambari Dashboard, in the `Spark2 Clients` list in the `Spark2` section.</span></span>

    <span data-ttu-id="fbfb0-167">c.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-167">c.</span></span> <span data-ttu-id="fbfb0-168">A csomópontok másolja könyvtárhoz.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-168">Copy that directory to each of the nodes.</span></span>

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-the-environment-variables"></a><span data-ttu-id="fbfb0-169">A környezeti változók beállítása</span><span class="sxs-lookup"><span data-stu-id="fbfb0-169">Set up the environment variables</span></span>

1. <span data-ttu-id="fbfb0-170">A Spark-fürt HDP verziója található.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-170">Find the HDP version of the Spark cluster.</span></span> <span data-ttu-id="fbfb0-171">A címtár-név alapján `/usr/hdp/` (például 2.5.4.2-7).</span><span class="sxs-lookup"><span data-stu-id="fbfb0-171">It is the directory name under `/usr/hdp/` (for example, 2.5.4.2-7).</span></span>

2. <span data-ttu-id="fbfb0-172">Állítsa be az összes csomópont hdp.version.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-172">Set hdp.version for all nodes.</span></span> <span data-ttu-id="fbfb0-173">Ambari irányítópulton Ugrás **YARN szakasz** > **Configs** > **speciális**, majd tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-173">In Ambari Dashboard, go to **YARN section** > **Configs** > **Advanced**, and then do the following:</span></span> 
 
    <span data-ttu-id="fbfb0-174">a.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-174">a.</span></span> <span data-ttu-id="fbfb0-175">A `Custom yarn-site`, adja hozzá az új tulajdonság `hdp.version` a fő csomóponton levő HDP verzió értékét.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-175">In `Custom yarn-site`, add a new property `hdp.version` with the value of the HDP version on the master node.</span></span> 
     
    <span data-ttu-id="fbfb0-176">b.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-176">b.</span></span> <span data-ttu-id="fbfb0-177">A konfigurációk mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-177">Save the configurations.</span></span> <span data-ttu-id="fbfb0-178">Nincsenek figyelmeztetések, amely figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-178">There are warnings, which you can ignore.</span></span> 
     
    <span data-ttu-id="fbfb0-179">c.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-179">c.</span></span> <span data-ttu-id="fbfb0-180">Az értesítési ikonok jelzik, indítsa újra a YARN és az Oozie-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-180">Restart the YARN and Oozie services as the notification icons indicate.</span></span>

3. <span data-ttu-id="fbfb0-181">Állítsa be az alábbi környezeti változókat a fő csomóponton (cserélje le az értékeket szükség szerint):</span><span class="sxs-lookup"><span data-stu-id="fbfb0-181">Set the following environment variables on the master node (replace the values as appropriate):</span></span>

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-the-graph-configuration"></a><span data-ttu-id="fbfb0-182">Készítse elő a graph-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="fbfb0-182">Prepare the graph configuration</span></span>

1. <span data-ttu-id="fbfb0-183">Egy konfigurációs fájl létrehozása az Azure Cosmos DB kapcsolódási paraméterek és a Spark-beállításokkal, és hogy a `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-183">Create a configuration file with the Azure Cosmos DB connection parameters and Spark settings, and put it at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span></span>

    ```
    gremlin.graph=org.apache.tinkerpop.gremlin.hadoop.structure.HadoopGraph
    gremlin.hadoop.jarsInDistributedCache=true
    gremlin.hadoop.defaultGraphComputer=org.apache.tinkerpop.gremlin.spark.process.computer.SparkGraphComputer

    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    gremlin.hadoop.graphWriter=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBOutputRDD

    ####################################
    # SparkGraphComputer Configuration #
    ####################################
    spark.master=yarn
    spark.executor.memory=3g
    spark.executor.instances=6
    spark.serializer=org.apache.spark.serializer.KryoSerializer
    spark.kryo.registrator=org.apache.tinkerpop.gremlin.spark.structure.io.gryo.GryoRegistrator
    gremlin.spark.persistContext=true

    # Classpath for the driver and executors
    spark.driver.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    spark.executor.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    
    ######################################
    # DocumentDB Spark connector         #
    ######################################
    spark.documentdb.connectionMode=Gateway
    spark.documentdb.schema_samplingratio=1.0
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    spark.documentdb.preferredRegions=FILLIN
    ```

2. <span data-ttu-id="fbfb0-184">Frissítés a `spark.driver.extraClassPath` és `spark.executor.extraClassPath` tartalmazza a JAR-fájlok, amelyek ebben az esetben terjesztve az előző lépésben kivételével könyvtárának `/home/sshuser/azure-documentdb-spark/*`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-184">Update the `spark.driver.extraClassPath` and `spark.executor.extraClassPath` to include the directory of the jars that you distributed in the previous step, in this case `/home/sshuser/azure-documentdb-spark/*`.</span></span>

3. <span data-ttu-id="fbfb0-185">Azure Cosmos DB adja meg a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-185">Provide the following details for Azure Cosmos DB:</span></span>

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-the-tinkerpop-graph-and-save-it-to-azure-cosmos-db"></a><span data-ttu-id="fbfb0-186">A TinkerPop graph betölteni, és mentse azt az Azure Cosmos-Adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="fbfb0-186">Load the TinkerPop graph, and save it to Azure Cosmos DB</span></span>
<span data-ttu-id="fbfb0-187">Bemutatják, hogyan lehet egy grafikonon megőrizni az Azure Cosmos DB, ebben a példában a használja a TinkerPop TinkerPop modern graph előre definiált.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-187">To demonstrate how to persist a graph into Azure Cosmos DB, this example uses the TinkerPop predefined TinkerPop modern graph.</span></span> <span data-ttu-id="fbfb0-188">A grafikon Kryo formátumban tárolja, és a TinkerPop tárház találhatók.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-188">The graph is stored in Kryo format, and it's provided in the TinkerPop repository.</span></span>

1. <span data-ttu-id="fbfb0-189">Mert Gremlin YARN módban futnak, meg kell nyitnia a Diagramadatok a Hadoop-fájlrendszer érhető el.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-189">Because you are running Gremlin in YARN mode, you must make the graph data available in the Hadoop file system.</span></span> <span data-ttu-id="fbfb0-190">Az alábbi parancsokkal könyvtár és a helyi graph fájl másolása.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-190">Use the following commands to make a directory and copy the local graph file into it.</span></span> 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. <span data-ttu-id="fbfb0-191">Ideiglenesen frissítése a `gremlin-spark.properties` használandó `GryoInputFormat` olvasni a diagramon.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-191">Temporarily update the `gremlin-spark.properties` file to use `GryoInputFormat` to read the graph.</span></span> <span data-ttu-id="fbfb0-192">Is utalhat `inputLocation` a könyvtárként hoz létre, mint a következőket:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-192">Also indicate `inputLocation` as the directory you create, as in the following:</span></span>

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. <span data-ttu-id="fbfb0-193">Indítsa el az Gremlin konzolját, és hozza létre az megőrizni az adatokat a konfigurált Azure Cosmos DB gyűjteményhez a következő számítási lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-193">Start Gremlin Console, and then create the following computation steps to persist data to the configured Azure Cosmos DB collection:</span></span>  

    <span data-ttu-id="fbfb0-194">a.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-194">a.</span></span> <span data-ttu-id="fbfb0-195">A grafikon létrehozása `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-195">Create the graph `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span></span>

    <span data-ttu-id="fbfb0-196">b.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-196">b.</span></span> <span data-ttu-id="fbfb0-197">Használja a SparkGraphComputer írásra `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-197">Use SparkGraphComputer for writing `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span></span>

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[gryoinputformat->documentdboutputrdd]
    gremlin> hg = graph.
                compute(SparkGraphComputer.class).
                result(GraphComputer.ResultGraph.NEW).
                persist(GraphComputer.Persist.EDGES).
                program(TraversalVertexProgram.build().
                    traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)), "gremlin-groovy", "g.V()").
                    create(graph)).
                submit().
                get() 
    ==>result[hadoopgraph[documentdbinputrdd->documentdboutputrdd],memory[size:1]]
    ```

4. <span data-ttu-id="fbfb0-198">Az adatkezelő ellenőrizheti, hogy az adatokat az Azure Cosmos Adatbázishoz nincs megőrizve.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-198">From Data Explorer, you can verify that the data has been persisted to Azure Cosmos DB.</span></span>

## <a name="load-the-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a><span data-ttu-id="fbfb0-199">A grafikon betöltése az Azure Cosmos-Adatbázisból, és Gremlin lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="fbfb0-199">Load the graph from Azure Cosmos DB, and run Gremlin queries</span></span>

1. <span data-ttu-id="fbfb0-200">A grafikon betöltéséhez szerkesztése `gremlin-spark.properties` beállítása `graphReader` való `DocumentDBInputRDD`:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-200">To load the graph, edit `gremlin-spark.properties` to set `graphReader` to `DocumentDBInputRDD`:</span></span>

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. <span data-ttu-id="fbfb0-201">A grafikon betöltése, haladnak át az adatokat és Gremlin lekérdezéseket futtathat vele a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-201">Load the graph, traverse the data, and run Gremlin queries with it by doing the following:</span></span>

    <span data-ttu-id="fbfb0-202">a.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-202">a.</span></span> <span data-ttu-id="fbfb0-203">Indítsa el a Gremlin konzolt `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-203">Start the Gremlin Console `bin/gremlin.sh`.</span></span>

    <span data-ttu-id="fbfb0-204">b.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-204">b.</span></span> <span data-ttu-id="fbfb0-205">Hozzon létre a diagramon a konfigurációs `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-205">Create the graph with the configuration `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span></span>

    <span data-ttu-id="fbfb0-206">c.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-206">c.</span></span> <span data-ttu-id="fbfb0-207">Hozzon létre egy grafikonon átjárás SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-207">Create a graph traversal with SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span></span>

    <span data-ttu-id="fbfb0-208">d.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-208">d.</span></span> <span data-ttu-id="fbfb0-209">A következő Gremlin graph lekérdezések futtatása:</span><span class="sxs-lookup"><span data-stu-id="fbfb0-209">Run the following Gremlin graph queries:</span></span>

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[documentdbinputrdd->documentdboutputrdd]
    gremlin> g = graph.traversal().withComputer(SparkGraphComputer)
    ==>graphtraversalsource[hadoopgraph[documentdbinputrdd->documentdboutputrdd], sparkgraphcomputer]
    gremlin> g.V().count()
    ==>6
    gremlin> g.E().count()
    ==>6
    gremlin> g.V(1).out().values('name')
    ==>josh
    ==>vadas
    ==>lop
    gremlin> g.V().hasLabel('person').coalesce(values('nickname'), values('name'))
    ==>josh
    ==>peter
    ==>vadas
    ==>marko
    gremlin> g.V().hasLabel('person').
            choose(values('name')).
                option('marko', values('age')).
                option('josh', values('name')).
                option('vadas', valueMap()).
                option('peter', label())
    ==>josh
    ==>person
    ==>[name:[vadas],age:[27]]
    ==>29
    ```

> [!NOTE]
> <span data-ttu-id="fbfb0-210">Részletesebb naplózás, állítsa be a napló szintjén lévő `conf/log4j-console.properties` részletesebb szintjét.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-210">To see more detailed logging, set the log level in `conf/log4j-console.properties` to a more verbose level.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="fbfb0-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbfb0-211">Next steps</span></span>

<span data-ttu-id="fbfb0-212">A gyors üzembe helyezési cikkben Azure Cosmos DB és Spark kombinálásával diagramjait munkavégzés hogy megismerte.</span><span class="sxs-lookup"><span data-stu-id="fbfb0-212">In this quick-start article, you've learned how to work with graphs by combining Azure Cosmos DB and Spark.</span></span>

> [!div class="nextstepaction"]
