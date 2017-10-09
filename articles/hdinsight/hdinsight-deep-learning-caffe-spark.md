---
title: "az Azure HDInsight Spark Caffe aaaUse elosztott mély biztonságával |} Microsoft Docs"
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
ms.openlocfilehash: d6476a7ed3a0df38538e845d7d5404067b01113c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Az Azure HDInsight Spark Caffe elosztott mély tanulási használata


## <a name="introduction"></a>Bevezetés

Mély tanulási egészségügyi tootransportation toomanufacturing eltávolításában, és több van hatással. Vállalatok be toosolve rögzített problémák, például a tanulási toodeep [besorolás kép](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [beszédfelismerés](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), a felismerési objektum, és a gépi fordítás. 

Nincsenek [számos népszerű keretrendszerekre](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), többek között a következőket [Microsoft kognitív eszközkészlet](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, stb. Caffe egyik hello leghíresebb nem szimbolikus (imperatív) Neurális hálózat keretrendszerek, és számos területen, például a számítógép stratégiai széles körben használt. Ezenkívül [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinálja az Apache Spark, ebben az esetben a mély tanulási Caffe könnyen használható egy létező Hadoop cluster együtt Spark ETL folyamatok, csökkenti a rendszer összetettségét és végpont tanulás késését.

[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) hello csak teljes körűen felügyelt felhőalapú Hadoop ajánlat, amely optimalizálva nyílt forráskódú elemzési fürtök Spark, Hive, MapReduce, HBase, Storm, Kafka és R Server üzemelnek 99,9 %-os SLA-t. Ezen big data technológiák és ISV-alkalmazások mindegyike könnyűszerrel helyezhető üzembe felügyelt fürtök formájában, nagyvállalati szintű biztonsággal és figyeléssel.

Egyes felhasználók arra kérjük, kapcsolatos részletes a hdinsight platformon, amely a Microsoft PaaS Hadoop termék tanulási toouse. Azt el fogja tudni jövőbeli hello további tooshare, de még ma azt szeretnénk, hogy hogyan műszaki blog toosummarize toouse Caffe a HDInsight Spark.

Ha Caffe előtt telepítette, megfigyelheti, hogy ezt a keretrendszert rendszer telepíteni egy kicsit kihívást. Az ebben a blogban azt először mutatják be hogyan tooinstall [a Spark Caffe](https://github.com/yahoo/CaffeOnSpark) a HDInsight-fürtök, majd használja hello beépített MNIST bemutató toodemostrate hogyan toouse elosztott mély tanulási használata a HDInsight Spark CPU-n.

Nincsenek négy fő lépést tooget, akkor a HDInsight működni.

1. Minden hello csomópontján hello szükséges függőségek telepítése
2. A HDInsight Spark hello központi csomóponton Caffe épül
3. Szükséges hello szalagtárak tooall hello munkavégző csomópontokhoz terjesztése
4. A Caffe modellek írása, és futtassa azt distributely

Mivel a HDInsight a PaaS megoldás, azt funkciókat nyújtja a kiváló platform -, hogy könnyen tooperform néhány feladatot. Hello szolgáltatása, amely a következő blogbejegyzésben fokozottan használjuk nevezik [parancsfájlművelet](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), amely Ön is végrehajthatja a rendszerhéj parancsok toocustomize fürtcsomópontok (átjárócsomópont, munkavégző csomópont vagy élcsomópont).

## <a name="step-1--install-hello-required-dependencies-on-all-hello-nodes"></a>1. lépés: Hello szükséges függőségek telepítése hello minden csomópontján

tooget elindult, igazolnia kell a tooinstall hello függőségek szükséges. hello Caffe hely és [CaffeOnSpark hely](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) kínál néhány nagyon hasznos wiki hello függőségek telepítése a Spark a YARN módban (amely a HDInsight Spark hello módban), de ellenőriznünk kell tooadd néhány további függőségek HDInsight platformon. Rendszer hello parancsfájlművelet használja az alábbi, és futtassa az összes hello átjárócsomópontokkal és feldolgozó csomópontokat. A parancsfájlművelet lépnek körülbelül 20 percet, mivel azok a többi csomagot is függ. Néhány helyen elérhető tooyour HDInsight-fürtjéhez, például a GitHub-helyét vagy hello alapértelmezett BLOB storage-fiók akkor kell helyezni.

    #!/bin/bash
    #Please be aware that installing hello below will add additional 20 mins toocluster creation because of hello dependencies
    #installing all dependencies, including hello ones mentioned in http://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose we install them on all hello nodes

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


A fenti hello parancsfájlművelet két lépésből áll. első lépés hello tooinstall az összes szükséges kódtárak hello. A tárak hello szükséges kódtárak (például gflags, glog) Caffe fordítása és Caffe fut (például numpy) tartalmazza. CPU-optimalizálásra libatlas használunk, de más optimalizálási könyvtárak, például MKL vagy CUDA (GPU) a telepítési hello CaffeOnSpark wiki mindig kövesse.

hello második lépésben toodownload, fordítása, és telepítse protobuf 2.5.0 Caffe a futtatás során. Protobuf 2.5.0 [szükséges](https://github.com/yahoo/CaffeOnSpark/issues/87), azonban a jelen verziójában nem áll rendelkezésre Ubuntu 16, a csomag, ezért ellenőriznünk kell, hogy toocompile hello forráskód le. Van még néhány erőforrások a hello Internet hogyan toocompile, például a [Ez](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)

Ismerkedés a toosimply, csak a parancsfájlművelet alapján futtathatók a fürt tooall hello munkavégző csomópontok és a head csomópontok (a HDInsight 3.5). Hello Parancsfájlműveletek futó fürt futtatásával, vagy hello Parancsfájlműveletek hello fürt létesítése idő alatt is futtathatja. Hello Parancsfájlműveletek a további részletekért lásd: hello dokumentáció [Itt](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)

![Parancsfájl-műveletek tooInstall függőségek](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-hello-head-node"></a>2. lépés: Létrehozása Caffe a hdinsight Spark-kiszolgálón hello átjárócsomópont

hello második lépése a hello headnode Caffe toobuild, és közzétennie a lefordított hello szalagtárak tooall hello munkavégző csomópontokhoz. Ebben a lépésben szüksége lesz túl[ssh azokat a headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), kövesse egyszerűen hello [CaffeOnSpark build folyamat](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), és az alábbiakban található néhány további lépést toobuild CaffeOnSpark használható hello parancsfájl. 

    #!/bin/bash
    git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
    export CAFFE_ON_SPARK=$(pwd)/CaffeOnSpark

    pushd ${CAFFE_ON_SPARK}/caffe-public/
    cp Makefile.config.example Makefile.config
    echo "INCLUDE_DIRS += ${JAVA_HOME}/include" >> Makefile.config
    #Below configurations might need toobe updated based on actual cases. For example, if you are using GPU, or using a different BLAS library, you may want tooupdate those settings accordingly.
    echo "CPU_ONLY := 1" >> Makefile.config
    echo "BLAS := atlas" >> Makefile.config
    echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
    echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
    popd

    #compile CaffeOnSpark
    pushd ${CAFFE_ON_SPARK}
    #always clean up hello environment before building (especially when rebuiding), or there will be errors such as "failed tooexecute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2"
    make clean 
    #hello build step usually takes 20~30 mins, since it has a lot maven dependencies
    make build 
    popd
    export LD_LIBRARY_PATH=${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib

    hadoop fs -mkdir -p wasb:///projects/machine_learning/image_dataset

    ${CAFFE_ON_SPARK}/scripts/setup-mnist.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/mnist_*_lmdb wasb:///projects/machine_learning/image_dataset/

    ${CAFFE_ON_SPARK}/scripts/setup-cifar10.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/cifar10_*_lmdb wasb:///projects/machine_learning/image_dataset/

    #put hello already compiled CaffeOnSpark libraries toowasb storage, then read back tooeach node using script actions. This is because CaffeOnSpark requires all hello nodes have hello libarries
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-public/distribute/lib/
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-distri/distribute/lib/* /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-public/distribute/lib/* /CaffeOnSpark/caffe-public/distribute/lib/

Szükség lehet a toodo több mint milyen hello dokumentációját CaffeOnSpark szerint. hello változások a következők:
- Csak tooCPU módosítsa, majd libatlas használni erre a célra.
- PUT hello adatkészletek toohello BLOB-tároló, amely egy megosztott hely, amely elérhető tooall munkavégző csomópontokhoz későbbi használatra.
- PUT hello Caffe szalagtárak tooBLOB tárolási lefordítva, és később fogja másolni ezen parancsfájl műveletek tooavoid további a fordítás során használt szalagtárak tooall hello csomópontok.


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a>Hibaelhárítás: Egy telepítsenek BuildException történt: exec adott vissza: 2. régiója

Amikor először próbálja toobuild CaffeOnSpark, néha azt jelenik meg:

    failed tooexecute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

Egyszerűen tiszta hello kód tárház által "tiszta ellenőrizze", és ezután futtatási "később build" lesz a probléma megoldásához, mindaddig, amíg hello helyes függőség van.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Hibaelhárítás: Maven tárház kapcsolat időtúllépése

Egyes esetekben maven ad hello kapcsolat időtúllépési hiba, hasonló toobelow:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request too{s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

OK lehet néhány perc várakozás után, és próbálja csak toorebuild hello kódot, így előfordulhat, hogy Maven valamilyen módon korlátok hello egy adott IP-címről érkező forgalmat.


### <a name="troubleshooting-test-failure-for-caffe"></a>Hibáinak elhárítása: Caffe hiba teszteléséhez

Valószínűleg látni fogja a teszt sikertelen CaffeOnSpark, hasonló az alábbi hello végső ellenőrzése során. Ez az UTF-8 kódolással kapcsolatos prabably, de kell befolyásolja a Caffe hello használata

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-hello-required-libraries-tooall-hello-worker-nodes"></a>3. lépés: A szükséges hello szalagtárak tooall hello munkavégző csomópontokhoz terjesztése

hello következő lépésre toodistribute hello függvénytárak (alapvetően hello CaffeOnSpark/caffe-nyilvános/terjesztése/lib tárak/és CaffeOnSpark/caffe-distri/terjesztése/lib /) tooall hello csomópontok. 2. lépésben, azt a tárak elhelyezése BLOB-tároló, és ebben a lépésben a parancsfájl-műveletek toocopy használjuk, tooall hello átjárócsomópontokkal és feldolgozó csomópontokat.

toodo a, a parancsfájlművelet futtassa az alábbi egyszerű (kell toopoint toohello helyen adott tooyour fürt):

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

A 2. lépésben, azt helyezése hello BLOB-tároló, amely elérhető tooall hello csomópontok, mert ebben a lépésben azt csak egyszerűen másolhatja azt tooall hello csomópontok.

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a>4. lépés: A Caffe modellek írása, és futtassa azt distributely

Miután hello a fenti lépéseket, Caffe képezve hello headnode telepítve, és jó toogo vagy folyamatban. következő lépés hello toowrite Caffe modell. 

Caffe használ egy "kifejező architektúra", ha a modell csak akkor kell toodefine egy konfigurációs fájl létrehozása, és minden (a legtöbb esetben) kódolás nélkül. Ezért a következőkben van. 

azt ma még betanítani hello modell egy olyan minta modell MNIST képzési. hello MNIST adatbázis kézzel számjegyek 60 000 példák betanítási készlete, és 10 000 példák TesztKészlet rendelkezik. A korábbiakhoz NIST elérhető egy részét is. hello számjegyek mérete normalizált és a rögzített méretű kép középre törölték. CaffeOnSpark rendelkezik néhány parancsfájlok toodownload hello adatkészletet, majd átalakíthatja hello megfelelő formátumú.

CaffeOnSpark MNIST képzési biztosít a hálózati topológiák példákat. A megosztással hello hálózati architektúra (hello hálózat topológiája hello) és optimalizálási töltött kialakítást rendelkezik. Ebben az esetben két fájl van szükség: 

hello "Solver" ({CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt$) fájllal hello optimalizálási felügyeletéért és paraméter frissítések létrehozásakor. Például azt határozza meg hogy Processzor- vagy gpu-t fogja használni, mi az az hello mérlegserpenyőre, hogyan sok a közelítés lesz, stb. Is meghatározza, melyik idegsejt hálózati topológia kell hello program használata (amely hello második fájl szükséges). A Solver kapcsolatos további információkért tekintse meg az túl[Caffe dokumentáció](http://caffe.berkeleyvision.org/tutorial/solver.html).

Ehhez a példához mivel GPU, hanem CPU használjuk kell módosítjuk hello utolsó sort:

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

Más sorok igény szerint módosíthatók.

hello második fájl ({CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt$) határozza meg, hogyan hello idegsejt hálózati néz, és hello vonatkozó bemeneti és kimeneti fájl. Tooupdate hello fájl tooreflect hello betanítási adatok helye is szükséges. Változás hello lenet_memory_train_test.prototxt (kell toopoint toohello helyen adott tooyour fürt) részt a következő:

- túl módosítása hello "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" "wasb: / / / projektek/machine_learning/image_dataset/mnist_train_lmdb"
- Módosítsa a "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" túl "wasb: / / / projektek/machine_learning/image_dataset/mnist_test_lmdb"

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

Hogyan toodefine hello hálózati további információkért tekintse meg az hello [MNIST dataset Caffe dokumentációja](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)

Az ebben a blogban hello célra csak használjuk a egyszerű MNIST példa. Az átjárócsomópont hello futtatnia kell hello-parancsot:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

Alapvetően szükséges hello terjesztett fájlok (lenet_memory_solver.prototxt és lenet_memory_train_test.prototxt) tooeach fonal tárolót, és állítson be úgy a megfelelő ELÉRÉSI útját minden egyes Spark illesztőprogram/végrehajtó tooLD_LIBRARY_PATH, amelyet a hello hello Előző kód részlet és pontok toohello olyan helyet, amelynek CaffeOnSpark szalagtárak. 

## <a name="monitoring-and-troubleshooting"></a>Figyelés és hibaelhárítás

Azt a YARN fürt módot, mivel ebben az esetben hello Spark illesztőprogram lesz-e ütemezett tooan tetszőleges tárolóhoz (és egy tetszőleges munkavégző csomópont) csak akkor jelenik meg a hello konzol írása alábbihoz hasonló:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Ha szeretné tooknow, mi történt, általában szüksége tooget hello Spark illesztőprogram naplót, amely további részleteket tartalmaz. Ebben az esetben szüksége toogo toohello YARN felhasználói felületen toofind hello vonatkozó YARN naplóit. Kaphat a YARN felhasználói felületen az URL-cím által hello: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN FELHASZNÁLÓI FELÜLETEN](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

Tekintse meg az adott alkalmazás hány erőforrásokat is igénybe vehet. Hello "Feladatütemező" hivatkozásra kattinthat, és majd látni fogja, hogy ehhez az alkalmazáshoz nincsenek futó 9 tárolók. 8 végrehajtója YARN tooprovide megkérjük, és egy másik tárolóban illesztőprogram folyamat. 

![YARN Feladatütemező](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

Érdemes lehet toocheck hello illesztőprogram naplók és a tároló naplók hibák esetén. Az illesztőprogram-naplók hello Alkalmazásazonosítót a YARN felhasználói felületen kattintson, majd hello "Logs" gombra. az stderr írja a hello illesztőprogram naplókat.

![YARN FELHASZNÁLÓI FELÜLETEN 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

Például előfordulhat, hogy megjelenik néhány hello hiba alább hello illesztőprogram naplókból arról túl sok végrehajtója osszon ki.

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

Egyes esetekben hello a probléma akkor fordulhat elő, illesztőprogramok helyett végrehajtója. Ebben az esetben szüksége toocheck hello tároló naplókat. Akkor is mindig hello tároló naplókat, és majd hello sikertelen tároló. Például a hiba előfordulhat, hogy megfelel a Caffe futtatásakor.

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

Ebben az esetben szüksége tooget sikertelen hello tárolóhely-azonosító (hello fenti eset, hogy a rendszer container_1485916338528_0008_05_000005). Akkor szükséges, hogy toorun 

    yarn logs -containerId container_1485916338528_0008_03_000005

a hello headnode. Az ellenőrzés tároló hiba után oka GPU mód használatával (ahol érdemes inkább CPU mód) a lenet_memory_solver.prototxt.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written tooSTDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Eredmények beolvasása

Mivel jelenleg 8 végrehajtója osztja fel, és egyszerű hello hálózati topológia, csak gyorsabban körülbelül 30 percet toorun hello eredménye. Hello parancssorból láthatja, hogy a Microsoft hello modell toowasb:///mnist.model put, és hogy a hello eredmények tooa mappájában wasb: / / / mnist_features_result.

Futtassa a hello eredményeit szeretné beolvasni

    hadoop fs -cat hdfs:///mnist_features_result/*

és hello eredménye a következőhöz hasonló:

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

hello SampleID hello azonosító hello MNIST adatkészlet jelöli, és hello címke hello szám, amely a modell hello azonosítására szolgál.


## <a name="conclusion"></a>Összegzés

Ebben a dokumentációban próbált tooinstall CaffeOnSpark egy egyszerű példa futtatásával. HDInsight egy teljes felügyelt felhőalapú elosztott számítási platformot, és hello a legjobb hely a nagy adatkészlet gépi tanulási és speciális elemzésekre munkaterhelések futtatása, és az elosztott mély learning, HDInsight Spark tooperform mély tanulási használhatja a Caffe feladatok.


## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)

