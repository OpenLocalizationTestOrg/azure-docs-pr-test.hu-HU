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
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a>Azure Cosmos DB: Graph elemzés végrehajtása a Spark és Apache TinkerPop Gremlin használatával

[Az Azure Cosmos DB](introduction.md) van globálisan elosztott, több modellre adatbázis-szolgáltatás a Microsoft hello. Hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és vízszintes méretű képességekről az Azure-Cosmos adatbázis hello core lekérdezése. Az Azure Cosmos DB támogatja az online tranzakció-feldolgozási (OLTP) graph használó munkaterhelések [Apache TinkerPop Gremlin](graph-introduction.md).

[Spark](http://spark.apache.org/) Apache szoftver Foundation projekt, amely az általános célú online analitikus feldolgozási (OLAP) adatok feldolgozási összpontosít. Spark hibrid a memória vagy lemez-alapú elosztott számítási modellt biztosít, amely hasonló toohello Hadoop MapReduce modell. Az Apache Spark on hello felhő segítségével telepíthet [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).

Azure Cosmos DB és Spark kombinálásával végezheti el az OLTP és OLAP munkaterhelések Gremlin használatakor. Ez gyors üzembe helyezési a cikk bemutatja, hogyan toorun Gremlin lekérdezi szemben Azure Cosmos DB egy Azure HDInsight Spark-fürtön.

## <a name="prerequisites"></a>Előfeltételek

Ez a minta futtatásához, a következő előfeltételek hello kell rendelkeznie:
* Az Azure HDInsight Spark-fürt 2.0
* JDK 1.8 + (Ha még nem rendelkezik JDK, futtassa `apt-get install default-jdk`.)
* Maven (Ha még nem rendelkezik Maven, `apt-get install maven`.)
* Azure-előfizetés ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])

További információ mentése az Azure HDInsight Spark-fürt tooset lásd [kiépítés HDInsight-fürtök](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-an-azure-cosmos-db-database-account"></a>Egy Azure Cosmos DB adatbázisfiók létrehozása

Először hozzon létre egy adatbázis-fiók hello Graph API hello következő tevékenységek végrehajtásával:

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Gyűjtemény hozzáadása

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="get-apache-tinkerpop"></a>Apache TinkerPop beolvasása

Töltse le az Apache TinkerPop hello következő tevékenységek végrehajtásával:

1. Távoli toohello főcsomópont hello HDInsight-fürt `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.

2. Klónozza hello TinkerPop3 forráskódját, összeállítani, helyileg, és telepítse tooMaven gyorsítótár.

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. Hello Spark-Gremlin beépülő modul telepítése 

    a. hello beépülő modul telepítésének hello szőlőmust kezeli. Feltöltése szőlőmust hello adattárak adatait, hogy letölthesse hello beépülő modult és annak függőségeit. 

      Hello szőlőmust konfigurációs fájl létrehozása, ha nincs jelen az `~/.groovy/grapeConfig.xml`. A következő beállítások hello használata:

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

    b. Indítsa el a Gremlin konzol `bin/gremlin.sh`.
        
    c. Hello Spark-Gremlin beépülő modul telepítése verziójával 3.3.0-SNAPSHOT, amely a korábbi lépésekben hello parancsfájlkezelő:

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

4. Ellenőrizze hogy toosee `Hadoop-Gremlin` , aktivált `:plugin list`. Ez a beépülő modul, mert azt sikerült zavarják a Spark-Gremlin hello beépülő modul letiltása `:plugin unuse tinkerpop.hadoop`.

## <a name="prepare-tinkerpop3-dependencies"></a>Készítse elő a TinkerPop3 függőségek

Amikor hello előző lépésben a TinkerPop3 beépített, hello folyamat is lekért összes jar-függőség hello célkönyvtár a forráskönyvtárban a Spark- és a Hadoop. Hello JAR-fájlok kivételével, amely előre HDI együtt vannak telepítve, és lekéréses a további függőségek csak szükség szerint használja.

1. Nyissa meg toohello Gremlin konzol tároló könyvtár: `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`. 

2. Helyezze át az összes JAR-fájlok kivételével a `ext/` túl`lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.

3. Távolítsa el a szalagtárak jar összes `lib/` , hogy nem a hello következő listán:

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

## <a name="get-hello-azure-cosmos-db-spark-connector"></a>Hello Azure Cosmos DB Spark összekötő beolvasása

1. Első hello Azure Cosmos DB Spark összekötő `azure-documentdb-spark-0.0.3-SNAPSHOT.jar` és Cosmos DB Java SDK `azure-documentdb-1.10.0.jar` a [Azure Cosmos DB Spark-összekötő a Githubon](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).

2. Másik lehetőségként hozhat létre, helyileg. Mivel hello legújabb verzióját a Spark-Gremlin 1.6.1-es Spark a következővel történt, és nem kompatibilis a hello Azure Cosmos DB Spark Connector jelenleg használt 2.0.2, Spark hello legújabb TinkerPop3 kód létrehozhatja és manuális telepítése hello JAR-fájlok kivételével. A következő hello:

    a. Hello Azure Cosmos DB Spark összekötő klónozását.

    b. Build TinkerPop3 (a korábbi lépésekben tette meg). Telepítse az összes TinkerPop 3.3.0-SNAPSHOT JAR-fájlok kivételével helyileg.

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    c. Frissítés `tinkerpop.version` `azure-documentdb-spark/pom.xml` túl`3.3.0-SNAPSHOT`.
    
    d. A Maven build. hello szükséges JAR-fájlok kivételével kerülnek `target` és `target/alternateLocation`.

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. Másolás hello azt már korábban említettük JAR-fájlok kivételével tooa helyi könyvtárat ~ / azure-documentdb-spark:

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-hello-dependencies-toohello-spark-worker-nodes"></a>Hello függőségek toohello Spark munkavégző csomópontokhoz terjesztése 

1. Mivel Diagramadatok hello átalakítása TinkerPop3 függ, el kell juttatnia hello kapcsolódó függőségek tooall Spark munkavégző csomópontokhoz.

2. Másolás hello azt már korábban említettük Gremlin függőségek, hello CosmosDB Spark összekötő jar és CosmosDB Java SDK toohello munkavégző csomópontokhoz hello következő tevékenységek végrehajtásával:

    a. Másolja az összes hello üveg `~/azure-documentdb-spark`.

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    b. Spark feldolgozó csomópontjaihoz, amely az hello az Ambari irányítópultján található hello listáját `Spark2 Clients` hello listájában `Spark2` szakasz.

    c. Másolja az adott könyvtár tooeach hello csomópontok.

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-hello-environment-variables"></a>Hello környezeti változók beállítása

1. Hello Spark-fürt hello HDP verziója található. Hello directory név alapján `/usr/hdp/` (például 2.5.4.2-7).

2. Állítsa be az összes csomópont hdp.version. Ambari irányítópult megtekintéséhez lépjen túl**YARN szakasz** > **Configs** > **speciális**, és ezután hello a következő: 
 
    a. A `Custom yarn-site`, adja hozzá az új tulajdonság `hdp.version` hello értékű hello HDP verzió hello fő csomóponton. 
     
    b. Hello konfigurációk mentéséhez. Nincsenek figyelmeztetések, amely figyelmen kívül hagyhatja. 
     
    c. Hello YARN és az Oozie szolgáltatások újraindításának hello értesítési ikonok jelzik.

3. Környezeti változók hello fő csomóponton (a név felülírandó hello értékeket szükség szerint) a következő set hello:

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-hello-graph-configuration"></a>Hello graph konfigurációs előkészítése

1. Egy konfigurációs fájl létrehozása hello Azure Cosmos DB kapcsolódási paraméterek és beállítások Spark, és helyezze a `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.

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

2. Frissítés hello `spark.driver.extraClassPath` és `spark.executor.extraClassPath` tooinclude hello könyvtárában hello JAR-fájlok kivételével, amelyek ebben az esetben elosztott hello a korábbi lépésben `/home/sshuser/azure-documentdb-spark/*`.

3. Adja meg a következő adatok az Azure Cosmos DB hello:

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-hello-tinkerpop-graph-and-save-it-tooazure-cosmos-db"></a>Hello TinkerPop graph betölteni, és mentse azt tooAzure Cosmos DB
toodemonstrate hogyan toopersist be Azure Cosmos DB, ebben a példában használt hello TinkerPop grafikon előre definiált TinkerPop modern grafikon. hello graph Kryo formátumban tárolja, és hello TinkerPop tárház találhatók.

1. Mivel Gremlin YARN módban futtatja, meg kell nyitnia hello Diagramadatok hello Hadoop-fájlrendszer érhető el. Használjon hello következő parancsokat toomake könyvtár és hello helyi graph fájl másolása bele. 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. Ideiglenesen frissítése hello `gremlin-spark.properties` toouse fájl `GryoInputFormat` tooread hello grafikon. Is utalhat `inputLocation` hello könyvtár hoz létre, mint hello következő:

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. Indítsa el Gremlin konzolt, és hozzon létre a következő számítási lépéseket toopersist toohello konfigurált Azure Cosmos DB adatgyűjtés hello:  

    a. Hozzon létre hello graph `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.

    b. Használja a SparkGraphComputer írásra `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.

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

4. Data Explorer ellenőrizheti, hogy korábban már hello megőrzött tooAzure Cosmos DB.

## <a name="load-hello-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a>Hello graph betöltése az Azure Cosmos-Adatbázisból, és Gremlin lekérdezések futtatása

1. tooload hello graph-szerkesztése `gremlin-spark.properties` tooset `graphReader` túl`DocumentDBInputRDD`:

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. Betöltési hello diagramot, haladnak át hello adatok, lekérdezések és futtatása Gremlin vele hello következő tevékenységek végrehajtásával:

    a. Indítsa el a hello Gremlin konzol `bin/gremlin.sh`.

    b. Hozzon létre hello graph hello konfigurációs `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.

    c. Hozzon létre egy grafikonon átjárás SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.

    d. Futtassa a következő Gremlin graph lekérdezések hello:

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
> toosee részletesebb naplózás, hello naplózási szintjének beállítása az `conf/log4j-console.properties` tooa részletesebb szintjét.
>

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezési cikkben hogy megismerte hogyan toowork rendelkező Azure Cosmos DB és Spark kombinálásával grafikonokon.

> [!div class="nextstepaction"]
