---
title: "Caffe használni az Azure HDInsight Spark elosztott mély tanulási |} Microsoft Docs"
description: "Az Azure HDInsight Spark Caffe elosztott mély tanulási használata"
services: hdinsight
documentationcenter: 
author: xiaoyongzhu
manager: asadk
editor: cgronlun
tags: azure-portal
ms.assetid: 71dcd1ad-4cad-47ad-8a9d-dcb7fa3c2ff9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: xiaoyzhu
ms.openlocfilehash: 14b7808c9534bce3049422d6bce1e8914b2c2fbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Az Azure HDInsight Spark Caffe elosztott mély tanulási használata


## <a name="introduction"></a>Bevezetés

A részletes tanulási van érintő mindent az RTM szállítására egészségügy és még sok más. A vállalatok állítja a tanulás rögzített problémák, például megoldására mély [besorolás kép](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [beszédfelismerés](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), a felismerési objektum, és a gépi fordítás. 

Nincsenek [számos népszerű keretrendszerekre](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), többek között a következőket [Microsoft kognitív eszközkészlet](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, stb. Caffe egyik a leghíresebb nem szimbolikus (imperatív) Neurális hálózat keretrendszerek, és számos területen, például a számítógép stratégiai széles körben használt. Ezenkívül [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinálja az Apache Spark, ebben az esetben a mély tanulási Caffe könnyen használható egy létező Hadoop cluster együtt Spark ETL folyamatok, csökkenti a rendszer összetettségét és végpont tanulás késését.

[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) a felhő csak teljes körűen felügyelt Hadoop ajánlat, amely optimalizált nyílt forráskódú elemzési fürtök biztosít a Spark, Hive, MapReduce, HBase, Storm, Kafka és R Server alapját 99,9 %-os SLA-t. Ezen big data technológiák és ISV-alkalmazások mindegyike könnyűszerrel helyezhető üzembe felügyelt fürtök formájában, nagyvállalati szintű biztonsággal és figyeléssel.

Egyes felhasználók arra kérjük, a hdinsight platformon, amely a Microsoft PaaS Hadoop termék részletes learning használatáról. Igazolnia kell több megosztást a jövőben, de azt szeretnénk, hogy összesítse a HDInsight Spark Caffe használatával műszaki blog ma.

Ha Caffe előtt telepítette, megfigyelheti, hogy ezt a keretrendszert rendszer telepíteni egy kicsit kihívást. Az ebben a blogban azt először mutatják be telepítése [a Spark Caffe](https://github.com/yahoo/CaffeOnSpark) a HDInsight-fürtök, majd használja a beépített MNIST bemutató demostrate való használata a HDInsight Spark CPU-n elosztott mély Learning használatáról.

Nincsenek négy fő lépést kell legyen a HDInsight működik.

1. Telepítse a szükséges függőségek a csomópontokon
2. Caffe létrehozása a Spark on hdinsight az átjárócsomópont-kiszolgálón
3. A szükséges kódtárak összes munkavégző csomópontokhoz terjesztése
4. A Caffe modellek írása, és futtassa azt distributely

Mivel a HDInsight a PaaS megoldás, azt funkciókat nyújtja a kiváló platform -, hogy egyes feladatok elvégzéséhez egyszerűen. Egyik szolgáltatása, amely a következő blogbejegyzésben fokozottan használjuk nevezik [parancsfájlművelet](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), amellyel testreszabásához fürtcsomópontok (átjárócsomópont, munkavégző csomópont vagy élcsomópont) rendszerhéj parancsot végrehajthat.

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a>1. lépés: A szükséges függőségek telepítése minden csomópontján

Első lépésként, a szükséges függőségek telepítése szükséges. A Caffe hely és [CaffeOnSpark hely](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) kínál néhány nagyon hasznos wiki való telepítéséhez a függőségeket a Spark YARN módban (amely a HDInsight Spark üzemmód), de ellenőriznünk kell hozzáadni a HDInsight platformon néhány további függőségek. Azt a parancsfájlművelet használják az alábbi, és futtassa az átjárócsomópontokkal és feldolgozó csomópontokat. A parancsfájlművelet lépnek körülbelül 20 percet, mivel azok a többi csomagot is függ. Néhány, a HDInsight-fürtjéhez, például egy GitHub helyre vagy az alapértelmezett BLOB storage-fiók által elérhető helyen akkor kell helyezni.

    #!/bin/bash
    #Please be aware that installing the below will add additional 20 mins to cluster creation because of the dependencies
    #installing all dependencies, including the ones mentioned in http://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose we install them on all the nodes

    sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler maven libatlas-base-dev libgflags-dev libgoogle-glog-dev liblmdb-dev build-essential  libboost-all-dev python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose

    #install protobuf
    wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
    sudo tar xzvf protobuf-2.5.0.tar.gz -C /tmp/
    cd /tmp/protobuf-2.5.0/
    sudo ./configure
    sudo make
    sudo make check
    sudo make install
    sudo ldconfig
    echo "protobuf installation done"


A fenti parancsfájlművelet két lépésből áll. Az első lépés, hogy a szükséges kódtárak telepítése. A tárak tartalmazza a szükséges kódtárak (például gflags, glog) Caffe fordítása és Caffe fut (például numpy). CPU-optimalizálásra libatlas használunk, de más optimalizálási könyvtárak, például MKL vagy CUDA (GPU) a telepítési mindig kövesse a CaffeOnSpark wiki.

A második lépésben letöltéséhez fordítása, és futásidőben Caffe protobuf 2.5.0 telepítése. Protobuf 2.5.0 [szükséges](https://github.com/yahoo/CaffeOnSpark/issues/87), azonban a jelen verziójában nem áll rendelkezésre Ubuntu 16, a csomag, ezért ellenőriznünk kell, hogy a forrás-kódjában. Van még néhány erőforrások az interneten, hogy, például hogy miként [Ez](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)

Egyszerűen a kezdéshez csak a parancsfájlművelet alapján futtathatók a fürt összes munkavégző csomópontokhoz és átjárócsomópontokkal (a HDInsight 3.5). A Parancsfájlműveletek futó fürt futtatásával, vagy a fürt létesítése idő alatt is futtathatja a parancsfájl-műveletek. A Parancsfájlműveletek olvashat, a dokumentáció [Itt](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)

![A Parancsfájlműveletek függőségek telepítése](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-the-head-node"></a>2. lépés: Létrehozása Caffe a hdinsight Spark-kiszolgálón az átjárócsomópont

A második lépés, hogy a headnode Caffe létrehozása, és közzétennie a lefordított tárak összes munkavégző csomópontokhoz. Ebben a lépésben szüksége lesz a [ssh azokat a headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), majd egyszerűen kövesse a [CaffeOnSpark build folyamat](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), és a parancsfájl segítségével CaffeOnSpark fejlesztheti néhány további lépést az alábbiakban található. 

    #!/bin/bash
    git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
    export CAFFE_ON_SPARK=$(pwd)/CaffeOnSpark

    pushd ${CAFFE_ON_SPARK}/caffe-public/
    cp Makefile.config.example Makefile.config
    echo "INCLUDE_DIRS += ${JAVA_HOME}/include" >> Makefile.config
    #Below configurations might need to be updated based on actual cases. For example, if you are using GPU, or using a different BLAS library, you may want to update those settings accordingly.
    echo "CPU_ONLY := 1" >> Makefile.config
    echo "BLAS := atlas" >> Makefile.config
    echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
    echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
    popd

    #compile CaffeOnSpark
    pushd ${CAFFE_ON_SPARK}
    #always clean up the environment before building (especially when rebuiding), or there will be errors such as "failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2"
    make clean 
    #the build step usually takes 20~30 mins, since it has a lot maven dependencies
    make build 
    popd
    export LD_LIBRARY_PATH=${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib

    hadoop fs -mkdir -p wasb:///projects/machine_learning/image_dataset

    ${CAFFE_ON_SPARK}/scripts/setup-mnist.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/mnist_*_lmdb wasb:///projects/machine_learning/image_dataset/

    ${CAFFE_ON_SPARK}/scripts/setup-cifar10.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/cifar10_*_lmdb wasb:///projects/machine_learning/image_dataset/

    #put the already compiled CaffeOnSpark libraries to wasb storage, then read back to each node using script actions. This is because CaffeOnSpark requires all the nodes have the libarries
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-public/distribute/lib/
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-distri/distribute/lib/* /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-public/distribute/lib/* /CaffeOnSpark/caffe-public/distribute/lib/

Szükség lehet több, mint a CaffeOnSpark a dokumentációban szerint hajtsa végre. A változások a következők:
- CPU csak módosítsa, majd libatlas használni erre a célra.
- Az adatkészletek helyezze el a BLOB Storage, amely egy megosztott hely, amely elérhető az összes munkavégző csomópontokhoz későbbi használatra.
- A BLOB storage a lefordított Caffe tárak PUT, és később fogja másolni a tárak Parancsfájlműveletek használatával további fordítási idő elkerülése érdekében minden csomópontja számára.


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a>Hibaelhárítás: Egy telepítsenek BuildException történt: exec adott vissza: 2. régiója

Amikor először próbálja CaffeOnSpark létrehozásához, néha azt jelenik meg:

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

A kód tárház által egyszerűen tiszta "tiszta ellenőrizze", majd az futtatási "Ellenőrizze build" lesz a probléma megoldásához, mindaddig, amíg a helyes függőség van.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Hibaelhárítás: Maven tárház kapcsolat időtúllépése

Egyes esetekben maven ad a kapcsolat időtúllépési hiba, hasonló alatt:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

Az OK lehet néhány perc várakozás után, és csak próbálja építse újra a kódot, így előfordulhat, hogy Maven valamilyen módon korlátozza a forgalmat egy adott IP-címről.


### <a name="troubleshooting-test-failure-for-caffe"></a>Hibáinak elhárítása: Caffe hiba teszteléséhez

Valószínűleg látni fogja a teszt sikertelen CaffeOnSpark, hasonló az alábbiakban a végső ellenőrzése során. Ez az UTF-8 kódolással kapcsolatos prabably, de kell nincs hatással az Caffe kihasználtsága

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a>3. lépés: A szükséges kódtárak összes munkavégző csomópontokhoz terjesztése

A következő lépés az, hogy a tárak elosztása (alapvetően a CaffeOnSpark/caffe-nyilvános/terjesztése/lib tárak/és CaffeOnSpark/caffe-distri/terjesztése/lib /) összes csomópontjának. 2. lépésben azt BLOB Storage tárolóban helyezhető el a tárak, és ebben a lépésben használjuk Parancsfájlműveletek másolja az átjárócsomópontokkal és feldolgozó csomópontokat.

Ehhez az szükséges, egyszerű futtató egy parancsfájlművelet az alábbi (kell a fürthöz megadott megfelelő helyére mutasson):

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

A 2. lépésben, azt helyezése a BLOB-tároló elérhető összes csomópontjának, mert ebben a lépésben azt csak egyszerűen másolásához csomópontjaihoz.

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a>4. lépés: A Caffe modellek írása, és futtassa azt distributely

Miután a fenti lépéseket, Caffe telepítve a headnode képezve, és dolgozunk visszaigazolásához. A következő lépésre egy Caffe modell írni. 

Caffe "kifejező architektúra segítségével", ahol egy modell létrehozására, egyszerűen adja meg a konfigurációs fájlt, és minden (a legtöbb esetben) nélkül kódolása. Ezért a következőkben van. 

A modell azt ma még betanítani egy olyan minta modell MNIST képzési. A MNIST adatbázis kézzel számjegyek 60 000 példák betanítási készlete, és 10 000 példák TesztKészlet rendelkezik. A korábbiakhoz NIST elérhető egy részét is. A számjegyek mérete normalizált és a rögzített méretű kép középre törölték. CaffeOnSpark rendelkezik néhány parancsprogramot, töltse le a DataSet adatkészlet és a megfelelő formátumba alakítsa át.

CaffeOnSpark MNIST képzési biztosít a hálózati topológiák példákat. A hálózati architektúra (a hálózati topológia) és optimalizálási felosztása töltött kialakítást rendelkezik. Ebben az esetben két fájl van szükség: 

a "Solver" fájl ({CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt$) áttekintése, valamint az optimalizálás, és létrehozzon paraméter frissítések szolgál. Például azt határozza meg hogy Processzor- vagy gpu-t fogja használni, mi az a mérlegserpenyőre, hogyan sok a közelítés lesz, stb. Is meghatározza, melyik idegsejt hálózati topológia használjon a program (amely a második fájl szükséges). A Solver kapcsolatos további információkért tekintse meg [Caffe dokumentáció](http://caffe.berkeleyvision.org/tutorial/solver.html).

Ehhez a példához mivel GPU, hanem CPU használjuk kell módosítjuk az utolsó sort:

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

Más sorok igény szerint módosíthatók.

A második fájl ({CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt$) határozza meg, hogyan a idegsejt hálózati mint, és a megfelelő bemeneti és kimeneti fájl. Is frissíteni kell a fájlt a betanítási adatok helyének megfelelően. A következő részben (kell a fürthöz megadott megfelelő helyére mutasson) lenet_memory_train_test.prototxt módosítása:

- Módosítsa a "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" "wasb: / / / projektek/machine_learning/image_dataset/mnist_train_lmdb"
- "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" módosítsa "wasb: / / / projektek/machine_learning/image_dataset/mnist_test_lmdb"

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

További információ megadásával a hálózati kapcsolatban, tekintse meg a [MNIST dataset Caffe dokumentációja](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)

Ebben a blogban céljából csak használjuk a egyszerű MNIST példa. Az átjárócsomópont kell futtatja a parancsot:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

Alapvetően az osztja el a szükséges fájlok (lenet_memory_solver.prototxt és lenet_memory_train_test.prototxt) minden YARN tárolóhoz, és állítsa is a megfelelő ELÉRÉSI útját minden egyes Spark illesztőprogram/végrehajtó LD_LIBRARY_PATH, amelyet az előző kódrészletet és a helyet, amely rendelkezik CaffeOnSpark szalagtárak mutat. 

## <a name="monitoring-and-troubleshooting"></a>Figyelés és hibaelhárítás

Mivel az YARN fürt mód használjuk, ebben az esetben a Spark illesztőprogram ütemezi egy tetszőleges tárolóhoz (és egy tetszőleges munkavégző csomópont) csak akkor jelenik meg a konzol kimeneti hasonlót:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Ha szeretné tudni, hogy mi történt, általában szeretné lekérni a Spark vezető naplót, amely tartalmaz további információt. Ebben az esetben meg kell a YARN felhasználói felületen a megfelelő YARN naplóit kereséséhez. A YARN felhasználói felületen az URL-cím által szerezheti be: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN FELHASZNÁLÓI FELÜLETEN](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

Tekintse meg az adott alkalmazás hány erőforrásokat is igénybe vehet. A "Feladatütemező" hivatkozásra kattinthat, és majd látni fogja, hogy ehhez az alkalmazáshoz nincsenek futó 9 tárolók. YARN 8 végrehajtója arra kérjük, és egy másik tárolóban illesztőprogram folyamat. 

![YARN Feladatütemező](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

Érdemes lehet ellenőrizze az illesztőprogram naplók és tároló naplókat, ha nincsenek hibák. Az illesztőprogram-naplók az Alkalmazásazonosítót a YARN felhasználói felületen kattintson, majd kattintson a "Naplózza" gombra. Az stderr írja a az illesztőprogram-naplókat.

![YARN FELHASZNÁLÓI FELÜLETEN 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

Például előfordulhat, hogy megjelenik a hiba alább az illesztőprogram naplókból némelyike arról túl sok végrehajtója osszon ki.

    17/02/01 07:26:06 ERROR ApplicationMaster: User class threw exception: java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
    java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
        at com.yahoo.ml.caffe.CaffeOnSpark.trainWithValidation(CaffeOnSpark.scala:261)
        at com.yahoo.ml.caffe.CaffeOnSpark$.main(CaffeOnSpark.scala:42)
        at com.yahoo.ml.caffe.CaffeOnSpark.main(CaffeOnSpark.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.spark.deploy.yarn.ApplicationMaster$$anon$2.run(ApplicationMaster.scala:627)

Egyes esetekben a probléma akkor fordulhat elő, illesztőprogramok helyett végrehajtója. Ebben az esetben kell a tároló naplókban találhatók. Mindig beolvasni a tároló naplókat, és majd kérje le a hibás tároló. Például a hiba előfordulhat, hogy megfelel a Caffe futtatásakor.

    17/02/01 07:12:05 WARN YarnAllocator: Container marked as failed: container_1485916338528_0008_05_000005 on host: 10.0.0.14. Exit status: 134. Diagnostics: Exception from container-launch.
    Container id: container_1485916338528_0008_05_000005
    Exit code: 134
    Exception message: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

    Stack trace: ExitCodeException exitCode=134: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

        at org.apache.hadoop.util.Shell.runCommand(Shell.java:933)
        at org.apache.hadoop.util.Shell.run(Shell.java:844)
        at org.apache.hadoop.util.Shell$ShellCommandExecutor.execute(Shell.java:1123)
        at org.apache.hadoop.yarn.server.nodemanager.DefaultContainerExecutor.launchContainer(DefaultContainerExecutor.java:225)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:317)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:83)
        at java.util.concurrent.FutureTask.run(FutureTask.java:266)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
        at java.lang.Thread.run(Thread.java:745)


    Container exited with a non-zero exit code 134

Ebben az esetben kell beszereznie a sikertelen tárolóhely-azonosító (a fenti eset, hogy a rendszer container_1485916338528_0008_05_000005). Majd futtatásához szükséges 

    yarn logs -containerId container_1485916338528_0008_03_000005

az a headnode. Az ellenőrzés tároló hiba után oka GPU mód használatával (ahol érdemes inkább CPU mód) a lenet_memory_solver.prototxt.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Eredmények beolvasása

Mivel azt 8 végrehajtója osztja fel, és a hálózati topológia egyszerű, csak elméletileg körülbelül 30 percet az eredmény futtatásához. A parancssorból, láthatja, hogy azt a modell használatba wasb:///mnist.model, és az eredmények nevű wasb: / / / mnist_features_result.

Az eredményeket kaphat rendszerű

    hadoop fs -cat hdfs:///mnist_features_result/*

és az eredmény a következőhöz hasonló:

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

A SampleID MNIST adatkészlet ID jelöli, és a címke a modell azonosító szám.


## <a name="conclusion"></a>Összegzés

Ebben a dokumentációban próbált CaffeOnSpark telepítése a futó egy egyszerű példa. HDInsight egy teljes felügyelt felhőalapú elosztott számítási platformot, és a legjobb hely a nagy adatkészlet gépi tanulási és speciális elemzésekre munkaterhelések futtatása, és elosztott mély tanulási, használhatja Caffe a HDInsight Spark mély tanulási feladatok elvégzéséhez.


## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban](hdinsight-apache-spark-resource-manager.md)

