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
ms.openlocfilehash: 0be5c9b12cdba4a428c809d00e1e68785a9ec1ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a><span data-ttu-id="0c226-103">Azure Cosmos DB: Graph elemzés végrehajtása a Spark és Apache TinkerPop Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="0c226-103">Azure Cosmos DB: Perform graph analytics by using Spark and Apache TinkerPop Gremlin</span></span>

<span data-ttu-id="0c226-104">[Az Azure Cosmos DB](introduction.md) van globálisan elosztott, több modellre adatbázis-szolgáltatás a Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="0c226-104">[Azure Cosmos DB](introduction.md) is hello globally distributed, multi-model database service from Microsoft.</span></span> <span data-ttu-id="0c226-105">Hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és vízszintes méretű képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="0c226-105">You can create and query document, key/value, and graph databases, all of which benefit from hello global-distribution and horizontal-scale capabilities at hello core of Azure Cosmos DB.</span></span> <span data-ttu-id="0c226-106">Az Azure Cosmos DB támogatja az online tranzakció-feldolgozási (OLTP) graph használó munkaterhelések [Apache TinkerPop Gremlin](graph-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0c226-106">Azure Cosmos DB supports online transaction processing (OLTP) graph workloads that use [Apache TinkerPop Gremlin](graph-introduction.md).</span></span>

<span data-ttu-id="0c226-107">[Spark](http://spark.apache.org/) Apache szoftver Foundation projekt, amely az általános célú online analitikus feldolgozási (OLAP) adatok feldolgozási összpontosít.</span><span class="sxs-lookup"><span data-stu-id="0c226-107">[Spark](http://spark.apache.org/) is an Apache Software Foundation project that's focused on general-purpose online analytical processing (OLAP) data processing.</span></span> <span data-ttu-id="0c226-108">Spark hibrid a memória vagy lemez-alapú elosztott számítási modellt biztosít, amely hasonló toohello Hadoop MapReduce modell.</span><span class="sxs-lookup"><span data-stu-id="0c226-108">Spark provides a hybrid in-memory/disk-based distributed computing model that is similar toohello Hadoop MapReduce model.</span></span> <span data-ttu-id="0c226-109">Az Apache Spark on hello felhő segítségével telepíthet [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span><span class="sxs-lookup"><span data-stu-id="0c226-109">You can deploy Apache Spark in hello cloud by using [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).</span></span>

<span data-ttu-id="0c226-110">Azure Cosmos DB és Spark kombinálásával végezheti el az OLTP és OLAP munkaterhelések Gremlin használatakor.</span><span class="sxs-lookup"><span data-stu-id="0c226-110">By combining Azure Cosmos DB and Spark, you can perform both OLTP and OLAP workloads when you use Gremlin.</span></span> <span data-ttu-id="0c226-111">Ez gyors üzembe helyezési a cikk bemutatja, hogyan toorun Gremlin lekérdezi szemben Azure Cosmos DB egy Azure HDInsight Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="0c226-111">This quick-start article demonstrates how toorun Gremlin queries against Azure Cosmos DB on an Azure HDInsight Spark cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c226-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0c226-112">Prerequisites</span></span>

<span data-ttu-id="0c226-113">Ez a minta futtatásához, a következő előfeltételek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="0c226-113">Before you can run this sample, you must have hello following prerequisites:</span></span>
* <span data-ttu-id="0c226-114">Az Azure HDInsight Spark-fürt 2.0</span><span class="sxs-lookup"><span data-stu-id="0c226-114">Azure HDInsight Spark cluster 2.0</span></span>
* <span data-ttu-id="0c226-115">JDK 1.8 + (Ha még nem rendelkezik JDK, futtassa `apt-get install default-jdk`.)</span><span class="sxs-lookup"><span data-stu-id="0c226-115">JDK 1.8+ (If you don't have JDK, run `apt-get install default-jdk`.)</span></span>
* <span data-ttu-id="0c226-116">Maven (Ha még nem rendelkezik Maven, `apt-get install maven`.)</span><span class="sxs-lookup"><span data-stu-id="0c226-116">Maven (If you don't have Maven, run `apt-get install maven`.)</span></span>
* <span data-ttu-id="0c226-117">Azure-előfizetés ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span><span class="sxs-lookup"><span data-stu-id="0c226-117">An Azure subscription ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])</span></span>

<span data-ttu-id="0c226-118">További információ mentése az Azure HDInsight Spark-fürt tooset lásd [kiépítés HDInsight-fürtök](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="0c226-118">For information about how tooset up an Azure HDInsight Spark cluster, see [Provisioning HDInsight clusters](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="0c226-119">Egy Azure Cosmos DB adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="0c226-119">Create an Azure Cosmos DB database account</span></span>

<span data-ttu-id="0c226-120">Először hozzon létre egy adatbázis-fiók hello Graph API hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="0c226-120">First, create a database account with hello Graph API by doing hello following:</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="0c226-121">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0c226-121">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a><span data-ttu-id="0c226-122">Apache TinkerPop beolvasása</span><span class="sxs-lookup"><span data-stu-id="0c226-122">Get Apache TinkerPop</span></span>

<span data-ttu-id="0c226-123">Töltse le az Apache TinkerPop hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="0c226-123">Get Apache TinkerPop by doing hello following:</span></span>

1. <span data-ttu-id="0c226-124">Távoli toohello főcsomópont hello HDInsight-fürt `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="0c226-124">Remote toohello master node of hello HDInsight cluster `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.</span></span>

2. <span data-ttu-id="0c226-125">Klónozza hello TinkerPop3 forráskódját, összeállítani, helyileg, és telepítse tooMaven gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="0c226-125">Clone hello TinkerPop3 source code, build it locally, and install it tooMaven cache.</span></span>

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. <span data-ttu-id="0c226-126">Hello Spark-Gremlin beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="0c226-126">Install hello Spark-Gremlin plug-in</span></span> 

    <span data-ttu-id="0c226-127">a.</span><span class="sxs-lookup"><span data-stu-id="0c226-127">a.</span></span> <span data-ttu-id="0c226-128">hello beépülő modul telepítésének hello szőlőmust kezeli.</span><span class="sxs-lookup"><span data-stu-id="0c226-128">hello installation of hello plug-in is handled by Grape.</span></span> <span data-ttu-id="0c226-129">Feltöltése szőlőmust hello adattárak adatait, hogy letölthesse hello beépülő modult és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="0c226-129">Populate hello repositories information for Grape so it can download hello plug-in and its dependencies.</span></span> 

      <span data-ttu-id="0c226-130">Hello szőlőmust konfigurációs fájl létrehozása, ha nincs jelen az `~/.groovy/grapeConfig.xml`.</span><span class="sxs-lookup"><span data-stu-id="0c226-130">Create hello grape configuration file if it's not present at `~/.groovy/grapeConfig.xml`.</span></span> <span data-ttu-id="0c226-131">A következő beállítások hello használata:</span><span class="sxs-lookup"><span data-stu-id="0c226-131">Use hello following settings:</span></span>

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

    <span data-ttu-id="0c226-132">b.</span><span class="sxs-lookup"><span data-stu-id="0c226-132">b.</span></span> <span data-ttu-id="0c226-133">Indítsa el a Gremlin konzol `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="0c226-133">Start Gremlin console `bin/gremlin.sh`.</span></span>
        
    <span data-ttu-id="0c226-134">c.</span><span class="sxs-lookup"><span data-stu-id="0c226-134">c.</span></span> <span data-ttu-id="0c226-135">Hello Spark-Gremlin beépülő modul telepítése verziójával 3.3.0-SNAPSHOT, amely a korábbi lépésekben hello parancsfájlkezelő:</span><span class="sxs-lookup"><span data-stu-id="0c226-135">Install hello Spark-Gremlin plug-in with version 3.3.0-SNAPSHOT, which you built in hello previous steps:</span></span>

    ```bash
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :install org.apache.tinkerpop spark-gremlin 3.3.0-SNAPSHOT
    ==>loaded: [org.apache.tinkerpop, spark-gremlin, 3.3.0-SNAPSHOT] - restart hello console toouse [tinkerpop.spark]
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

4. <span data-ttu-id="0c226-136">Ellenőrizze hogy toosee `Hadoop-Gremlin` , aktivált `:plugin list`.</span><span class="sxs-lookup"><span data-stu-id="0c226-136">Check toosee whether `Hadoop-Gremlin` is activated with `:plugin list`.</span></span> <span data-ttu-id="0c226-137">Ez a beépülő modul, mert azt sikerült zavarják a Spark-Gremlin hello beépülő modul letiltása `:plugin unuse tinkerpop.hadoop`.</span><span class="sxs-lookup"><span data-stu-id="0c226-137">Disable this plug-in, because it could interfere with hello Spark-Gremlin plug-in `:plugin unuse tinkerpop.hadoop`.</span></span>

## <a name="prepare-tinkerpop3-dependencies"></a><span data-ttu-id="0c226-138">Készítse elő a TinkerPop3 függőségek</span><span class="sxs-lookup"><span data-stu-id="0c226-138">Prepare TinkerPop3 dependencies</span></span>

<span data-ttu-id="0c226-139">Amikor hello előző lépésben a TinkerPop3 beépített, hello folyamat is lekért összes jar-függőség hello célkönyvtár a forráskönyvtárban a Spark- és a Hadoop.</span><span class="sxs-lookup"><span data-stu-id="0c226-139">When you built TinkerPop3 in hello previous step, hello process also pulled all jar dependencies for Spark and Hadoop in hello target directory.</span></span> <span data-ttu-id="0c226-140">Hello JAR-fájlok kivételével, amely előre HDI együtt vannak telepítve, és lekéréses a további függőségek csak szükség szerint használja.</span><span class="sxs-lookup"><span data-stu-id="0c226-140">Use hello jars that are pre-installed with HDI, and pull in additional dependencies only as necessary.</span></span>

1. <span data-ttu-id="0c226-141">Nyissa meg toohello Gremlin konzol tároló könyvtár: `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span><span class="sxs-lookup"><span data-stu-id="0c226-141">Go toohello Gremlin Console target directory at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`.</span></span> 

2. <span data-ttu-id="0c226-142">Helyezze át az összes JAR-fájlok kivételével a `ext/` túl`lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span><span class="sxs-lookup"><span data-stu-id="0c226-142">Move all jars under `ext/` too`lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.</span></span>

3. <span data-ttu-id="0c226-143">Távolítsa el a szalagtárak jar összes `lib/` , hogy nem a hello következő listán:</span><span class="sxs-lookup"><span data-stu-id="0c226-143">Remove all jar libraries under `lib/` that are not in hello following list:</span></span>

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

## <a name="get-hello-azure-cosmos-db-spark-connector"></a><span data-ttu-id="0c226-144">Hello Azure Cosmos DB Spark összekötő beolvasása</span><span class="sxs-lookup"><span data-stu-id="0c226-144">Get hello Azure Cosmos DB Spark connector</span></span>

1. <span data-ttu-id="0c226-145">Első hello Azure Cosmos DB Spark összekötő `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` és Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` a [Azure Cosmos DB Spark-összekötő a Githubon](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span><span class="sxs-lookup"><span data-stu-id="0c226-145">Get hello Azure Cosmos DB Spark connector `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` and Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` from [Azure Cosmos DB Spark Connector on GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).</span></span>

2. <span data-ttu-id="0c226-146">Másik lehetőségként hozhat létre, helyileg.</span><span class="sxs-lookup"><span data-stu-id="0c226-146">Alternatively, you can build it locally.</span></span> <span data-ttu-id="0c226-147">Mivel hello legújabb verzióját a Spark-Gremlin 1.6.1-es Spark a következővel történt, és nem kompatibilis a hello Azure Cosmos DB Spark Connector jelenleg használt 2.0.2, Spark hello legújabb TinkerPop3 kód létrehozhatja és manuális telepítése hello JAR-fájlok kivételével.</span><span class="sxs-lookup"><span data-stu-id="0c226-147">Because hello latest version of Spark-Gremlin was built with Spark 1.6.1 and is not compatible with Spark 2.0.2, which is currently used in hello Azure Cosmos DB Spark connector, you can build hello latest TinkerPop3 code and install hello jars manually.</span></span> <span data-ttu-id="0c226-148">A következő hello:</span><span class="sxs-lookup"><span data-stu-id="0c226-148">Do hello following:</span></span>

    <span data-ttu-id="0c226-149">a.</span><span class="sxs-lookup"><span data-stu-id="0c226-149">a.</span></span> <span data-ttu-id="0c226-150">Hello Azure Cosmos DB Spark összekötő klónozását.</span><span class="sxs-lookup"><span data-stu-id="0c226-150">Clone hello Azure Cosmos DB Spark connector.</span></span>

    <span data-ttu-id="0c226-151">b.</span><span class="sxs-lookup"><span data-stu-id="0c226-151">b.</span></span> <span data-ttu-id="0c226-152">Build TinkerPop3 (a korábbi lépésekben tette meg).</span><span class="sxs-lookup"><span data-stu-id="0c226-152">Build TinkerPop3 (already done in previous steps).</span></span> <span data-ttu-id="0c226-153">Telepítse az összes TinkerPop 3.3.0-SNAPSHOT JAR-fájlok kivételével helyileg.</span><span class="sxs-lookup"><span data-stu-id="0c226-153">Install all TinkerPop 3.3.0-SNAPSHOT jars locally.</span></span>

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    <span data-ttu-id="0c226-154">c.</span><span class="sxs-lookup"><span data-stu-id="0c226-154">c.</span></span> <span data-ttu-id="0c226-155">Frissítés `tinkerpop.version` `azure-documentdb-spark/pom.xml` túl`3.3.0-SNAPSHOT`.</span><span class="sxs-lookup"><span data-stu-id="0c226-155">Update `tinkerpop.version` `azure-documentdb-spark/pom.xml` too`3.3.0-SNAPSHOT`.</span></span>
    
    <span data-ttu-id="0c226-156">d.</span><span class="sxs-lookup"><span data-stu-id="0c226-156">d.</span></span> <span data-ttu-id="0c226-157">A Maven build.</span><span class="sxs-lookup"><span data-stu-id="0c226-157">Build with Maven.</span></span> <span data-ttu-id="0c226-158">hello szükséges JAR-fájlok kivételével kerülnek `target` és `target/alternateLocation`.</span><span class="sxs-lookup"><span data-stu-id="0c226-158">hello needed jars are placed in `target` and `target/alternateLocation`.</span></span>

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. <span data-ttu-id="0c226-159">Másolás hello azt már korábban említettük JAR-fájlok kivételével tooa helyi könyvtárat ~ / azure-documentdb-spark:</span><span class="sxs-lookup"><span data-stu-id="0c226-159">Copy hello previously mentioned jars tooa local directory at ~/azure-documentdb-spark:</span></span>

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-hello-dependencies-toohello-spark-worker-nodes"></a><span data-ttu-id="0c226-160">Hello függőségek toohello Spark munkavégző csomópontokhoz terjesztése</span><span class="sxs-lookup"><span data-stu-id="0c226-160">Distribute hello dependencies toohello Spark worker nodes</span></span> 

1. <span data-ttu-id="0c226-161">Mivel Diagramadatok hello átalakítása TinkerPop3 függ, el kell juttatnia hello kapcsolódó függőségek tooall Spark munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="0c226-161">Because hello transformation of graph data depends on TinkerPop3, you must distribute hello related dependencies tooall Spark worker nodes.</span></span>

2. <span data-ttu-id="0c226-162">Másolás hello azt már korábban említettük Gremlin függőségek, hello CosmosDB Spark összekötő jar és CosmosDB Java SDK toohello munkavégző csomópontokhoz hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="0c226-162">Copy hello previously mentioned Gremlin dependencies, hello CosmosDB Spark connector jar, and CosmosDB Java SDK toohello worker nodes by doing hello following:</span></span>

    <span data-ttu-id="0c226-163">a.</span><span class="sxs-lookup"><span data-stu-id="0c226-163">a.</span></span> <span data-ttu-id="0c226-164">Másolja az összes hello üveg `~/azure-documentdb-spark`.</span><span class="sxs-lookup"><span data-stu-id="0c226-164">Copy all hello jars into `~/azure-documentdb-spark`.</span></span>

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    <span data-ttu-id="0c226-165">b.</span><span class="sxs-lookup"><span data-stu-id="0c226-165">b.</span></span> <span data-ttu-id="0c226-166">Spark feldolgozó csomópontjaihoz, amely az hello az Ambari irányítópultján található hello listáját `Spark2 Clients` hello listájában `Spark2` szakasz.</span><span class="sxs-lookup"><span data-stu-id="0c226-166">Get hello list of all Spark worker nodes, which you can find on Ambari Dashboard, in hello `Spark2 Clients` list in hello `Spark2` section.</span></span>

    <span data-ttu-id="0c226-167">c.</span><span class="sxs-lookup"><span data-stu-id="0c226-167">c.</span></span> <span data-ttu-id="0c226-168">Másolja az adott könyvtár tooeach hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="0c226-168">Copy that directory tooeach of hello nodes.</span></span>

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-hello-environment-variables"></a><span data-ttu-id="0c226-169">Hello környezeti változók beállítása</span><span class="sxs-lookup"><span data-stu-id="0c226-169">Set up hello environment variables</span></span>

1. <span data-ttu-id="0c226-170">Hello Spark-fürt hello HDP verziója található.</span><span class="sxs-lookup"><span data-stu-id="0c226-170">Find hello HDP version of hello Spark cluster.</span></span> <span data-ttu-id="0c226-171">Hello directory név alapján `/usr/hdp/` (például 2.5.4.2-7).</span><span class="sxs-lookup"><span data-stu-id="0c226-171">It is hello directory name under `/usr/hdp/` (for example, 2.5.4.2-7).</span></span>

2. <span data-ttu-id="0c226-172">Állítsa be az összes csomópont hdp.version.</span><span class="sxs-lookup"><span data-stu-id="0c226-172">Set hdp.version for all nodes.</span></span> <span data-ttu-id="0c226-173">Ambari irányítópult megtekintéséhez lépjen túl**YARN szakasz** > **Configs** > **speciális**, és ezután hello a következő:</span><span class="sxs-lookup"><span data-stu-id="0c226-173">In Ambari Dashboard, go too**YARN section** > **Configs** > **Advanced**, and then do hello following:</span></span> 
 
    <span data-ttu-id="0c226-174">a.</span><span class="sxs-lookup"><span data-stu-id="0c226-174">a.</span></span> <span data-ttu-id="0c226-175">A `Custom yarn-site`, adja hozzá az új tulajdonság `hdp.version` hello értékű hello HDP verzió hello fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="0c226-175">In `Custom yarn-site`, add a new property `hdp.version` with hello value of hello HDP version on hello master node.</span></span> 
     
    <span data-ttu-id="0c226-176">b.</span><span class="sxs-lookup"><span data-stu-id="0c226-176">b.</span></span> <span data-ttu-id="0c226-177">Hello konfigurációk mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="0c226-177">Save hello configurations.</span></span> <span data-ttu-id="0c226-178">Nincsenek figyelmeztetések, amely figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="0c226-178">There are warnings, which you can ignore.</span></span> 
     
    <span data-ttu-id="0c226-179">c.</span><span class="sxs-lookup"><span data-stu-id="0c226-179">c.</span></span> <span data-ttu-id="0c226-180">Hello YARN és az Oozie szolgáltatások újraindításának hello értesítési ikonok jelzik.</span><span class="sxs-lookup"><span data-stu-id="0c226-180">Restart hello YARN and Oozie services as hello notification icons indicate.</span></span>

3. <span data-ttu-id="0c226-181">Környezeti változók hello fő csomóponton (a név felülírandó hello értékeket szükség szerint) a következő set hello:</span><span class="sxs-lookup"><span data-stu-id="0c226-181">Set hello following environment variables on hello master node (replace hello values as appropriate):</span></span>

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-hello-graph-configuration"></a><span data-ttu-id="0c226-182">Hello graph konfigurációs előkészítése</span><span class="sxs-lookup"><span data-stu-id="0c226-182">Prepare hello graph configuration</span></span>

1. <span data-ttu-id="0c226-183">Egy konfigurációs fájl létrehozása hello Azure Cosmos DB kapcsolódási paraméterek és beállítások Spark, és helyezze a `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span><span class="sxs-lookup"><span data-stu-id="0c226-183">Create a configuration file with hello Azure Cosmos DB connection parameters and Spark settings, and put it at `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.</span></span>

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

    # Classpath for hello driver and executors
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

2. <span data-ttu-id="0c226-184">Frissítés hello `spark.driver.extraClassPath` és `spark.executor.extraClassPath` tooinclude hello könyvtárában hello JAR-fájlok kivételével, amelyek ebben az esetben elosztott hello a korábbi lépésben `/home/sshuser/azure-documentdb-spark/*`.</span><span class="sxs-lookup"><span data-stu-id="0c226-184">Update hello `spark.driver.extraClassPath` and `spark.executor.extraClassPath` tooinclude hello directory of hello jars that you distributed in hello previous step, in this case `/home/sshuser/azure-documentdb-spark/*`.</span></span>

3. <span data-ttu-id="0c226-185">Adja meg a következő adatok az Azure Cosmos DB hello:</span><span class="sxs-lookup"><span data-stu-id="0c226-185">Provide hello following details for Azure Cosmos DB:</span></span>

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-hello-tinkerpop-graph-and-save-it-tooazure-cosmos-db"></a><span data-ttu-id="0c226-186">Hello TinkerPop graph betölteni, és mentse azt tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0c226-186">Load hello TinkerPop graph, and save it tooAzure Cosmos DB</span></span>
<span data-ttu-id="0c226-187">toodemonstrate hogyan toopersist be Azure Cosmos DB, ebben a példában használt hello TinkerPop grafikon előre definiált TinkerPop modern grafikon.</span><span class="sxs-lookup"><span data-stu-id="0c226-187">toodemonstrate how toopersist a graph into Azure Cosmos DB, this example uses hello TinkerPop predefined TinkerPop modern graph.</span></span> <span data-ttu-id="0c226-188">hello graph Kryo formátumban tárolja, és hello TinkerPop tárház találhatók.</span><span class="sxs-lookup"><span data-stu-id="0c226-188">hello graph is stored in Kryo format, and it's provided in hello TinkerPop repository.</span></span>

1. <span data-ttu-id="0c226-189">Mivel Gremlin YARN módban futtatja, meg kell nyitnia hello Diagramadatok hello Hadoop-fájlrendszer érhető el.</span><span class="sxs-lookup"><span data-stu-id="0c226-189">Because you are running Gremlin in YARN mode, you must make hello graph data available in hello Hadoop file system.</span></span> <span data-ttu-id="0c226-190">Használjon hello következő parancsokat toomake könyvtár és hello helyi graph fájl másolása bele.</span><span class="sxs-lookup"><span data-stu-id="0c226-190">Use hello following commands toomake a directory and copy hello local graph file into it.</span></span> 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. <span data-ttu-id="0c226-191">Ideiglenesen frissítése hello `gremlin-spark.properties` toouse fájl `GryoInputFormat` tooread hello grafikon.</span><span class="sxs-lookup"><span data-stu-id="0c226-191">Temporarily update hello `gremlin-spark.properties` file toouse `GryoInputFormat` tooread hello graph.</span></span> <span data-ttu-id="0c226-192">Is utalhat `inputLocation` hello könyvtár hoz létre, mint hello következő:</span><span class="sxs-lookup"><span data-stu-id="0c226-192">Also indicate `inputLocation` as hello directory you create, as in hello following:</span></span>

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. <span data-ttu-id="0c226-193">Indítsa el Gremlin konzolt, és hozzon létre a következő számítási lépéseket toopersist toohello konfigurált Azure Cosmos DB adatgyűjtés hello:</span><span class="sxs-lookup"><span data-stu-id="0c226-193">Start Gremlin Console, and then create hello following computation steps toopersist data toohello configured Azure Cosmos DB collection:</span></span>  

    <span data-ttu-id="0c226-194">a.</span><span class="sxs-lookup"><span data-stu-id="0c226-194">a.</span></span> <span data-ttu-id="0c226-195">Hozzon létre hello graph `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span><span class="sxs-lookup"><span data-stu-id="0c226-195">Create hello graph `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.</span></span>

    <span data-ttu-id="0c226-196">b.</span><span class="sxs-lookup"><span data-stu-id="0c226-196">b.</span></span> <span data-ttu-id="0c226-197">Használja a SparkGraphComputer írásra `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span><span class="sxs-lookup"><span data-stu-id="0c226-197">Use SparkGraphComputer for writing `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.</span></span>

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

4. <span data-ttu-id="0c226-198">Data Explorer ellenőrizheti, hogy korábban már hello megőrzött tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0c226-198">From Data Explorer, you can verify that hello data has been persisted tooAzure Cosmos DB.</span></span>

## <a name="load-hello-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a><span data-ttu-id="0c226-199">Hello graph betöltése az Azure Cosmos-Adatbázisból, és Gremlin lekérdezések futtatása</span><span class="sxs-lookup"><span data-stu-id="0c226-199">Load hello graph from Azure Cosmos DB, and run Gremlin queries</span></span>

1. <span data-ttu-id="0c226-200">tooload hello graph-szerkesztése `gremlin-spark.properties` tooset `graphReader` túl`DocumentDBInputRDD`:</span><span class="sxs-lookup"><span data-stu-id="0c226-200">tooload hello graph, edit `gremlin-spark.properties` tooset `graphReader` too`DocumentDBInputRDD`:</span></span>

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. <span data-ttu-id="0c226-201">Betöltési hello diagramot, haladnak át hello adatok, lekérdezések és futtatása Gremlin vele hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="0c226-201">Load hello graph, traverse hello data, and run Gremlin queries with it by doing hello following:</span></span>

    <span data-ttu-id="0c226-202">a.</span><span class="sxs-lookup"><span data-stu-id="0c226-202">a.</span></span> <span data-ttu-id="0c226-203">Indítsa el a hello Gremlin konzol `bin/gremlin.sh`.</span><span class="sxs-lookup"><span data-stu-id="0c226-203">Start hello Gremlin Console `bin/gremlin.sh`.</span></span>

    <span data-ttu-id="0c226-204">b.</span><span class="sxs-lookup"><span data-stu-id="0c226-204">b.</span></span> <span data-ttu-id="0c226-205">Hozzon létre hello graph hello konfigurációs `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span><span class="sxs-lookup"><span data-stu-id="0c226-205">Create hello graph with hello configuration `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.</span></span>

    <span data-ttu-id="0c226-206">c.</span><span class="sxs-lookup"><span data-stu-id="0c226-206">c.</span></span> <span data-ttu-id="0c226-207">Hozzon létre egy grafikonon átjárás SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span><span class="sxs-lookup"><span data-stu-id="0c226-207">Create a graph traversal with SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.</span></span>

    <span data-ttu-id="0c226-208">d.</span><span class="sxs-lookup"><span data-stu-id="0c226-208">d.</span></span> <span data-ttu-id="0c226-209">Futtassa a következő Gremlin graph lekérdezések hello:</span><span class="sxs-lookup"><span data-stu-id="0c226-209">Run hello following Gremlin graph queries:</span></span>

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
> <span data-ttu-id="0c226-210">toosee részletesebb naplózás, hello naplózási szintjének beállítása az `conf/log4j-console.properties` tooa részletesebb szintjét.</span><span class="sxs-lookup"><span data-stu-id="0c226-210">toosee more detailed logging, set hello log level in `conf/log4j-console.properties` tooa more verbose level.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="0c226-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0c226-211">Next steps</span></span>

<span data-ttu-id="0c226-212">A gyors üzembe helyezési cikkben hogy megismerte hogyan toowork rendelkező Azure Cosmos DB és Spark kombinálásával grafikonokon.</span><span class="sxs-lookup"><span data-stu-id="0c226-212">In this quick-start article, you've learned how toowork with graphs by combining Azure Cosmos DB and Spark.</span></span>

> [!div class="nextstepaction"]
