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
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a><span data-ttu-id="424fe-103">Az Azure HDInsight Spark Caffe elosztott mély tanulási használata</span><span class="sxs-lookup"><span data-stu-id="424fe-103">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>


## <a name="introduction"></a><span data-ttu-id="424fe-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="424fe-104">Introduction</span></span>

<span data-ttu-id="424fe-105">A részletes tanulási van érintő mindent az RTM szállítására egészségügy és még sok más.</span><span class="sxs-lookup"><span data-stu-id="424fe-105">Deep learning is impacting everything from healthcare to transportation to manufacturing, and more.</span></span> <span data-ttu-id="424fe-106">A vállalatok állítja a tanulás rögzített problémák, például megoldására mély [besorolás kép](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [beszédfelismerés](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), a felismerési objektum, és a gépi fordítás.</span><span class="sxs-lookup"><span data-stu-id="424fe-106">Companies are turning to deep learning to solve hard problems, like [image classification](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [speech recognition](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), object recognition, and machine translation.</span></span> 

<span data-ttu-id="424fe-107">Nincsenek [számos népszerű keretrendszerekre](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), többek között a következőket [Microsoft kognitív eszközkészlet](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, stb. Caffe egyik a leghíresebb nem szimbolikus (imperatív) Neurális hálózat keretrendszerek, és számos területen, például a számítógép stratégiai széles körben használt.</span><span class="sxs-lookup"><span data-stu-id="424fe-107">There are [many popular frameworks](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), including [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, etc. Caffe is one of the most famous non-symbolic (imperative) neural network frameworks, and widely used in many areas including computer vision.</span></span> <span data-ttu-id="424fe-108">Ezenkívül [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinálja az Apache Spark, ebben az esetben a mély tanulási Caffe könnyen használható egy létező Hadoop cluster együtt Spark ETL folyamatok, csökkenti a rendszer összetettségét és végpont tanulás késését.</span><span class="sxs-lookup"><span data-stu-id="424fe-108">Furthermore, [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) combines Caffe with Apache Spark, in which case deep learning can be easily used on an existing Hadoop cluster together with Spark ETL pipelines, reducing system complexity and latency for end-to-end learning.</span></span>

<span data-ttu-id="424fe-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) a felhő csak teljes körűen felügyelt Hadoop ajánlat, amely optimalizált nyílt forráskódú elemzési fürtök biztosít a Spark, Hive, MapReduce, HBase, Storm, Kafka és R Server alapját 99,9 %-os SLA-t.</span><span class="sxs-lookup"><span data-stu-id="424fe-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) is the only fully-managed cloud Hadoop offering that provides optimized open source analytic clusters for Spark, Hive, MapReduce, HBase, Storm, Kafka, and R Server backed by a 99.9% SLA.</span></span> <span data-ttu-id="424fe-110">Ezen big data technológiák és ISV-alkalmazások mindegyike könnyűszerrel helyezhető üzembe felügyelt fürtök formájában, nagyvállalati szintű biztonsággal és figyeléssel.</span><span class="sxs-lookup"><span data-stu-id="424fe-110">Each of these big data technologies and ISV applications are easily deployable as managed clusters with enterprise-level security and monitoring.</span></span>

<span data-ttu-id="424fe-111">Egyes felhasználók arra kérjük, a hdinsight platformon, amely a Microsoft PaaS Hadoop termék részletes learning használatáról.</span><span class="sxs-lookup"><span data-stu-id="424fe-111">Some users are asking us about how to use deep learning on HDInsight, which is Microsoft's PaaS Hadoop product.</span></span> <span data-ttu-id="424fe-112">Igazolnia kell több megosztást a jövőben, de azt szeretnénk, hogy összesítse a HDInsight Spark Caffe használatával műszaki blog ma.</span><span class="sxs-lookup"><span data-stu-id="424fe-112">We will have more to share in the future, but today we want to summarize a technical blog on how to use Caffe on HDInsight Spark.</span></span>

<span data-ttu-id="424fe-113">Ha Caffe előtt telepítette, megfigyelheti, hogy ezt a keretrendszert rendszer telepíteni egy kicsit kihívást.</span><span class="sxs-lookup"><span data-stu-id="424fe-113">If you have installed Caffe before, you will notice that installing this framework is a little bit challenging.</span></span> <span data-ttu-id="424fe-114">Az ebben a blogban azt először mutatják be telepítése [a Spark Caffe](https://github.com/yahoo/CaffeOnSpark) a HDInsight-fürtök, majd használja a beépített MNIST bemutató demostrate való használata a HDInsight Spark CPU-n elosztott mély Learning használatáról.</span><span class="sxs-lookup"><span data-stu-id="424fe-114">In this blog, we will first illustrate how to install [Caffe on Spark](https://github.com/yahoo/CaffeOnSpark) for an HDInsight cluster, then use the built-in MNIST demo to demostrate how to use Distributed Deep Learning using HDInsight Spark on CPUs.</span></span>

<span data-ttu-id="424fe-115">Nincsenek négy fő lépést kell legyen a HDInsight működik.</span><span class="sxs-lookup"><span data-stu-id="424fe-115">There are four major steps to get it work on HDInsight.</span></span>

1. <span data-ttu-id="424fe-116">Telepítse a szükséges függőségek a csomópontokon</span><span class="sxs-lookup"><span data-stu-id="424fe-116">Install the required dependencies on all the nodes</span></span>
2. <span data-ttu-id="424fe-117">Caffe létrehozása a Spark on hdinsight az átjárócsomópont-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="424fe-117">Build Caffe on Spark for HDInsight on the head node</span></span>
3. <span data-ttu-id="424fe-118">A szükséges kódtárak összes munkavégző csomópontokhoz terjesztése</span><span class="sxs-lookup"><span data-stu-id="424fe-118">Distribute the required libraries to all the worker nodes</span></span>
4. <span data-ttu-id="424fe-119">A Caffe modellek írása, és futtassa azt distributely</span><span class="sxs-lookup"><span data-stu-id="424fe-119">Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="424fe-120">Mivel a HDInsight a PaaS megoldás, azt funkciókat nyújtja a kiváló platform -, hogy egyes feladatok elvégzéséhez egyszerűen.</span><span class="sxs-lookup"><span data-stu-id="424fe-120">Since HDInsight is a PaaS solution, it offers great platform features - so it is quite easy to perform some tasks.</span></span> <span data-ttu-id="424fe-121">Egyik szolgáltatása, amely a következő blogbejegyzésben fokozottan használjuk nevezik [parancsfájlművelet](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), amellyel testreszabásához fürtcsomópontok (átjárócsomópont, munkavégző csomópont vagy élcsomópont) rendszerhéj parancsot végrehajthat.</span><span class="sxs-lookup"><span data-stu-id="424fe-121">One of the features that we heavily use in this blog post is called [Script Action](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), with which you can execute shell commands to customize cluster nodes (head node, worker node, or edge node).</span></span>

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a><span data-ttu-id="424fe-122">1. lépés: A szükséges függőségek telepítése minden csomópontján</span><span class="sxs-lookup"><span data-stu-id="424fe-122">Step 1:  Install the required dependencies on all the nodes</span></span>

<span data-ttu-id="424fe-123">Első lépésként, a szükséges függőségek telepítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="424fe-123">To get started, we need to install the dependencies we need.</span></span> <span data-ttu-id="424fe-124">A Caffe hely és [CaffeOnSpark hely](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) kínál néhány nagyon hasznos wiki való telepítéséhez a függőségeket a Spark YARN módban (amely a HDInsight Spark üzemmód), de ellenőriznünk kell hozzáadni a HDInsight platformon néhány további függőségek.</span><span class="sxs-lookup"><span data-stu-id="424fe-124">The Caffe site and [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) offers some very useful wiki for installing the dependencies for Spark on YARN mode (which is the mode for HDInsight Spark), but we need to add a few more dependencies for HDInsight platform.</span></span> <span data-ttu-id="424fe-125">Azt a parancsfájlművelet használják az alábbi, és futtassa az átjárócsomópontokkal és feldolgozó csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="424fe-125">We will use the script action as below and run it on all the head nodes and worker nodes.</span></span> <span data-ttu-id="424fe-126">A parancsfájlművelet lépnek körülbelül 20 percet, mivel azok a többi csomagot is függ.</span><span class="sxs-lookup"><span data-stu-id="424fe-126">This script action will take about 20 minutes, as those dependencies also depend on other packages.</span></span> <span data-ttu-id="424fe-127">Néhány, a HDInsight-fürtjéhez, például egy GitHub helyre vagy az alapértelmezett BLOB storage-fiók által elérhető helyen akkor kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="424fe-127">You should put it in some location that is accessible to your HDInsight cluster, such as a GitHub location or the default BLOB storage account.</span></span>

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


<span data-ttu-id="424fe-128">A fenti parancsfájlművelet két lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="424fe-128">There are two steps in the script action above.</span></span> <span data-ttu-id="424fe-129">Az első lépés, hogy a szükséges kódtárak telepítése.</span><span class="sxs-lookup"><span data-stu-id="424fe-129">The first step is to install all the required libraries.</span></span> <span data-ttu-id="424fe-130">A tárak tartalmazza a szükséges kódtárak (például gflags, glog) Caffe fordítása és Caffe fut (például numpy).</span><span class="sxs-lookup"><span data-stu-id="424fe-130">Those libraries include the necessary libraries for both compiling Caffe(such as gflags, glog) and running Caffe (such as numpy).</span></span> <span data-ttu-id="424fe-131">CPU-optimalizálásra libatlas használunk, de más optimalizálási könyvtárak, például MKL vagy CUDA (GPU) a telepítési mindig kövesse a CaffeOnSpark wiki.</span><span class="sxs-lookup"><span data-stu-id="424fe-131">We are using libatlas for CPU optimization, but you can always follow the CaffeOnSpark wiki on installing other optimization libraries, such as MKL or CUDA (for GPU).</span></span>

<span data-ttu-id="424fe-132">A második lépésben letöltéséhez fordítása, és futásidőben Caffe protobuf 2.5.0 telepítése.</span><span class="sxs-lookup"><span data-stu-id="424fe-132">The second step is to download, compile, and install protobuf 2.5.0 for Caffe during runtime.</span></span> <span data-ttu-id="424fe-133">Protobuf 2.5.0 [szükséges](https://github.com/yahoo/CaffeOnSpark/issues/87), azonban a jelen verziójában nem áll rendelkezésre Ubuntu 16, a csomag, ezért ellenőriznünk kell, hogy a forrás-kódjában.</span><span class="sxs-lookup"><span data-stu-id="424fe-133">Protobuf 2.5.0 [is required](https://github.com/yahoo/CaffeOnSpark/issues/87), however this version is not available as a package on Ubuntu 16, so we need to compile it from the source code.</span></span> <span data-ttu-id="424fe-134">Van még néhány erőforrások az interneten, hogy, például hogy miként [Ez](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span><span class="sxs-lookup"><span data-stu-id="424fe-134">There are also a few resources on the Internet on how to compile it, such as [this](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span></span>

<span data-ttu-id="424fe-135">Egyszerűen a kezdéshez csak a parancsfájlművelet alapján futtathatók a fürt összes munkavégző csomópontokhoz és átjárócsomópontokkal (a HDInsight 3.5).</span><span class="sxs-lookup"><span data-stu-id="424fe-135">To simply get started, you can just run this script action against your cluster to all the worker nodes and head nodes (for HDInsight 3.5).</span></span> <span data-ttu-id="424fe-136">A Parancsfájlműveletek futó fürt futtatásával, vagy a fürt létesítése idő alatt is futtathatja a parancsfájl-műveletek.</span><span class="sxs-lookup"><span data-stu-id="424fe-136">You can either run the script actions for a running cluster, or you can also run the script actions during the cluster provision time.</span></span> <span data-ttu-id="424fe-137">A Parancsfájlműveletek olvashat, a dokumentáció [Itt](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span><span class="sxs-lookup"><span data-stu-id="424fe-137">For more details on the script actions, please see the documentation [here](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span></span>

![A Parancsfájlműveletek függőségek telepítése](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-the-head-node"></a><span data-ttu-id="424fe-139">2. lépés: Létrehozása Caffe a hdinsight Spark-kiszolgálón az átjárócsomópont</span><span class="sxs-lookup"><span data-stu-id="424fe-139">Step 2: Build Caffe on Spark for HDInsight on the head node</span></span>

<span data-ttu-id="424fe-140">A második lépés, hogy a headnode Caffe létrehozása, és közzétennie a lefordított tárak összes munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="424fe-140">The second step is to build Caffe on the headnode, and then distribute the compiled libraries to all the worker nodes.</span></span> <span data-ttu-id="424fe-141">Ebben a lépésben szüksége lesz a [ssh azokat a headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), majd egyszerűen kövesse a [CaffeOnSpark build folyamat](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), és a parancsfájl segítségével CaffeOnSpark fejlesztheti néhány további lépést az alábbiakban található.</span><span class="sxs-lookup"><span data-stu-id="424fe-141">In this step, you will need to [ssh into your headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), then simply follow the [CaffeOnSpark build process](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), and below is the script you can use to build CaffeOnSpark with a few additional steps.</span></span> 

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

<span data-ttu-id="424fe-142">Szükség lehet több, mint a CaffeOnSpark a dokumentációban szerint hajtsa végre.</span><span class="sxs-lookup"><span data-stu-id="424fe-142">You may need to do more than what the documentation of CaffeOnSpark says.</span></span> <span data-ttu-id="424fe-143">A változások a következők:</span><span class="sxs-lookup"><span data-stu-id="424fe-143">The changes are:</span></span>
- <span data-ttu-id="424fe-144">CPU csak módosítsa, majd libatlas használni erre a célra.</span><span class="sxs-lookup"><span data-stu-id="424fe-144">Change to CPU only and use libatlas for this particular purpose.</span></span>
- <span data-ttu-id="424fe-145">Az adatkészletek helyezze el a BLOB Storage, amely egy megosztott hely, amely elérhető az összes munkavégző csomópontokhoz későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="424fe-145">Put the datasets to the BLOB storage, which is a shared location that is accessible to all worker nodes for later use.</span></span>
- <span data-ttu-id="424fe-146">A BLOB storage a lefordított Caffe tárak PUT, és később fogja másolni a tárak Parancsfájlműveletek használatával további fordítási idő elkerülése érdekében minden csomópontja számára.</span><span class="sxs-lookup"><span data-stu-id="424fe-146">Put the compiled Caffe libraries to BLOB storage, and later you will copy those libraries to all the nodes using script actions to avoid additional compilation time.</span></span>


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a><span data-ttu-id="424fe-147">Hibaelhárítás: Egy telepítsenek BuildException történt: exec adott vissza: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="424fe-147">Troubleshooting: An Ant BuildException has occured: exec returned: 2</span></span>

<span data-ttu-id="424fe-148">Amikor először próbálja CaffeOnSpark létrehozásához, néha azt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="424fe-148">When first trying to build CaffeOnSpark, sometimes it will say</span></span>

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

<span data-ttu-id="424fe-149">A kód tárház által egyszerűen tiszta "tiszta ellenőrizze", majd az futtatási "Ellenőrizze build" lesz a probléma megoldásához, mindaddig, amíg a helyes függőség van.</span><span class="sxs-lookup"><span data-stu-id="424fe-149">Simply clean the code repository by "make clean" and then run "make build" will solve this issue, as long as you have the correct dependencies.</span></span>

### <a name="troubleshooting-maven-repository-connection-time-out"></a><span data-ttu-id="424fe-150">Hibaelhárítás: Maven tárház kapcsolat időtúllépése</span><span class="sxs-lookup"><span data-stu-id="424fe-150">Troubleshooting: Maven repository connection time out</span></span>

<span data-ttu-id="424fe-151">Egyes esetekben maven ad a kapcsolat időtúllépési hiba, hasonló alatt:</span><span class="sxs-lookup"><span data-stu-id="424fe-151">Sometimes maven gives me the connection time out error, similar to below:</span></span>

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

<span data-ttu-id="424fe-152">Az OK lehet néhány perc várakozás után, és csak próbálja építse újra a kódot, így előfordulhat, hogy Maven valamilyen módon korlátozza a forgalmat egy adott IP-címről.</span><span class="sxs-lookup"><span data-stu-id="424fe-152">It will be OK after waiting for a few minutes and then just try to rebuild the code, so it might be Maven somehow limits the traffic from a given IP address.</span></span>


### <a name="troubleshooting-test-failure-for-caffe"></a><span data-ttu-id="424fe-153">Hibáinak elhárítása: Caffe hiba teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="424fe-153">Troubleshooting: Test failure for Caffe</span></span>

<span data-ttu-id="424fe-154">Valószínűleg látni fogja a teszt sikertelen CaffeOnSpark, hasonló az alábbiakban a végső ellenőrzése során.</span><span class="sxs-lookup"><span data-stu-id="424fe-154">You probably will see a test failure when doing the final check for CaffeOnSpark, similar with below.</span></span> <span data-ttu-id="424fe-155">Ez az UTF-8 kódolással kapcsolatos prabably, de kell nincs hatással az Caffe kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="424fe-155">This is prabably related with UTF-8 encoding, but should not impact the usage of Caffe</span></span>

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a><span data-ttu-id="424fe-156">3. lépés: A szükséges kódtárak összes munkavégző csomópontokhoz terjesztése</span><span class="sxs-lookup"><span data-stu-id="424fe-156">Step 3: Distribute the required libraries to all the worker nodes</span></span>

<span data-ttu-id="424fe-157">A következő lépés az, hogy a tárak elosztása (alapvetően a CaffeOnSpark/caffe-nyilvános/terjesztése/lib tárak/és CaffeOnSpark/caffe-distri/terjesztése/lib /) összes csomópontjának.</span><span class="sxs-lookup"><span data-stu-id="424fe-157">The next step is to distribute the libraries (basically the libraries in CaffeOnSpark/caffe-public/distribute/lib/ and CaffeOnSpark/caffe-distri/distribute/lib/) to all the nodes.</span></span> <span data-ttu-id="424fe-158">2. lépésben azt BLOB Storage tárolóban helyezhető el a tárak, és ebben a lépésben használjuk Parancsfájlműveletek másolja az átjárócsomópontokkal és feldolgozó csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="424fe-158">In Step 2, we put those libraries on BLOB storage, and in this step, we will use script actions to copy it to all the head nodes and worker nodes.</span></span>

<span data-ttu-id="424fe-159">Ehhez az szükséges, egyszerű futtató egy parancsfájlművelet az alábbi (kell a fürthöz megadott megfelelő helyére mutasson):</span><span class="sxs-lookup"><span data-stu-id="424fe-159">To do this, simple run a script action as below (you need to point to the right location specific to your cluster):</span></span>

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

<span data-ttu-id="424fe-160">A 2. lépésben, azt helyezése a BLOB-tároló elérhető összes csomópontjának, mert ebben a lépésben azt csak egyszerűen másolásához csomópontjaihoz.</span><span class="sxs-lookup"><span data-stu-id="424fe-160">Because in step 2, we put it on the BLOB storage which is accessible to all the nodes, in this step we just simply copy it to all the nodes.</span></span>

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a><span data-ttu-id="424fe-161">4. lépés: A Caffe modellek írása, és futtassa azt distributely</span><span class="sxs-lookup"><span data-stu-id="424fe-161">Step 4: Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="424fe-162">Miután a fenti lépéseket, Caffe telepítve a headnode képezve, és dolgozunk visszaigazolásához.</span><span class="sxs-lookup"><span data-stu-id="424fe-162">After running the above steps, Caffe is alreay installed on the headnode and we are good to go.</span></span> <span data-ttu-id="424fe-163">A következő lépésre egy Caffe modell írni.</span><span class="sxs-lookup"><span data-stu-id="424fe-163">The next step is to write a Caffe model.</span></span> 

<span data-ttu-id="424fe-164">Caffe "kifejező architektúra segítségével", ahol egy modell létrehozására, egyszerűen adja meg a konfigurációs fájlt, és minden (a legtöbb esetben) nélkül kódolása.</span><span class="sxs-lookup"><span data-stu-id="424fe-164">Caffe is using an "expressive architecture", where for composing a model, you just need to define a configuration file, and without coding at all (in most cases).</span></span> <span data-ttu-id="424fe-165">Ezért a következőkben van.</span><span class="sxs-lookup"><span data-stu-id="424fe-165">So let's take a look there.</span></span> 

<span data-ttu-id="424fe-166">A modell azt ma még betanítani egy olyan minta modell MNIST képzési.</span><span class="sxs-lookup"><span data-stu-id="424fe-166">The model we will train today is a sample model for MNIST training.</span></span> <span data-ttu-id="424fe-167">A MNIST adatbázis kézzel számjegyek 60 000 példák betanítási készlete, és 10 000 példák TesztKészlet rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="424fe-167">The MNIST database of handwritten digits has a training set of 60,000 examples, and a test set of 10,000 examples.</span></span> <span data-ttu-id="424fe-168">A korábbiakhoz NIST elérhető egy részét is.</span><span class="sxs-lookup"><span data-stu-id="424fe-168">It is a subset of a larger set available from NIST.</span></span> <span data-ttu-id="424fe-169">A számjegyek mérete normalizált és a rögzített méretű kép középre törölték.</span><span class="sxs-lookup"><span data-stu-id="424fe-169">The digits have been size-normalized and centered in a fixed-size image.</span></span> <span data-ttu-id="424fe-170">CaffeOnSpark rendelkezik néhány parancsprogramot, töltse le a DataSet adatkészlet és a megfelelő formátumba alakítsa át.</span><span class="sxs-lookup"><span data-stu-id="424fe-170">CaffeOnSpark has some scripts to download the dataset and convert it into the right format.</span></span>

<span data-ttu-id="424fe-171">CaffeOnSpark MNIST képzési biztosít a hálózati topológiák példákat.</span><span class="sxs-lookup"><span data-stu-id="424fe-171">CaffeOnSpark provides some network topologies example for MNIST training.</span></span> <span data-ttu-id="424fe-172">A hálózati architektúra (a hálózati topológia) és optimalizálási felosztása töltött kialakítást rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="424fe-172">It has a nice design of splitting the network architecture (the topology of the network) and optimization.</span></span> <span data-ttu-id="424fe-173">Ebben az esetben két fájl van szükség:</span><span class="sxs-lookup"><span data-stu-id="424fe-173">In this case, There are two files required:</span></span> 

<span data-ttu-id="424fe-174">a "Solver" fájl ({CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt$) áttekintése, valamint az optimalizálás, és létrehozzon paraméter frissítések szolgál.</span><span class="sxs-lookup"><span data-stu-id="424fe-174">the "Solver" file (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) is used for overseeing the optimization and generating parameter updates.</span></span> <span data-ttu-id="424fe-175">Például azt határozza meg hogy Processzor- vagy gpu-t fogja használni, mi az a mérlegserpenyőre, hogyan sok a közelítés lesz, stb. Is meghatározza, melyik idegsejt hálózati topológia használjon a program (amely a második fájl szükséges).</span><span class="sxs-lookup"><span data-stu-id="424fe-175">For example, it defines whether CPU or GPU will be used, what's the momentum, how many iterations will be, etc. It also defines which neuron network topology should the program use (which is the second file we need).</span></span> <span data-ttu-id="424fe-176">A Solver kapcsolatos további információkért tekintse meg [Caffe dokumentáció](http://caffe.berkeleyvision.org/tutorial/solver.html).</span><span class="sxs-lookup"><span data-stu-id="424fe-176">For more information about Solver, please refer to [Caffe documentation](http://caffe.berkeleyvision.org/tutorial/solver.html).</span></span>

<span data-ttu-id="424fe-177">Ehhez a példához mivel GPU, hanem CPU használjuk kell módosítjuk az utolsó sort:</span><span class="sxs-lookup"><span data-stu-id="424fe-177">For this example, since we are using CPU rather than GPU, we should change the last line to:</span></span>

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

<span data-ttu-id="424fe-179">Más sorok igény szerint módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="424fe-179">You can change other lines as needed.</span></span>

<span data-ttu-id="424fe-180">A második fájl ({CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt$) határozza meg, hogyan a idegsejt hálózati mint, és a megfelelő bemeneti és kimeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="424fe-180">The second file (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) defines how the neuron network looks like, and the relevant input and output file.</span></span> <span data-ttu-id="424fe-181">Is frissíteni kell a fájlt a betanítási adatok helyének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="424fe-181">We also need to update the file to reflect the training data location.</span></span> <span data-ttu-id="424fe-182">A következő részben (kell a fürthöz megadott megfelelő helyére mutasson) lenet_memory_train_test.prototxt módosítása:</span><span class="sxs-lookup"><span data-stu-id="424fe-182">Change the following part in lenet_memory_train_test.prototxt (you need to point to the right location specific to your cluster):</span></span>

- <span data-ttu-id="424fe-183">Módosítsa a "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" "wasb: / / / projektek/machine_learning/image_dataset/mnist_train_lmdb"</span><span class="sxs-lookup"><span data-stu-id="424fe-183">change the "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" to "wasb:///projects/machine_learning/image_dataset/mnist_train_lmdb"</span></span>
- <span data-ttu-id="424fe-184">"file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" módosítsa "wasb: / / / projektek/machine_learning/image_dataset/mnist_test_lmdb"</span><span class="sxs-lookup"><span data-stu-id="424fe-184">change "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" to "wasb:///projects/machine_learning/image_dataset/mnist_test_lmdb"</span></span>

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

<span data-ttu-id="424fe-186">További információ megadásával a hálózati kapcsolatban, tekintse meg a [MNIST dataset Caffe dokumentációja](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span><span class="sxs-lookup"><span data-stu-id="424fe-186">For more information on how to define the network, please check the [Caffe documentation on MNIST dataset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span></span>

<span data-ttu-id="424fe-187">Ebben a blogban céljából csak használjuk a egyszerű MNIST példa.</span><span class="sxs-lookup"><span data-stu-id="424fe-187">For the purpose of this blog, we just use this simple MNIST example.</span></span> <span data-ttu-id="424fe-188">Az átjárócsomópont kell futtatja a parancsot:</span><span class="sxs-lookup"><span data-stu-id="424fe-188">You should run the command below from the head node:</span></span>

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

<span data-ttu-id="424fe-189">Alapvetően az osztja el a szükséges fájlok (lenet_memory_solver.prototxt és lenet_memory_train_test.prototxt) minden YARN tárolóhoz, és állítsa is a megfelelő ELÉRÉSI útját minden egyes Spark illesztőprogram/végrehajtó LD_LIBRARY_PATH, amelyet az előző kódrészletet és a helyet, amely rendelkezik CaffeOnSpark szalagtárak mutat.</span><span class="sxs-lookup"><span data-stu-id="424fe-189">Basically it distributes the required files (lenet_memory_solver.prototxt and lenet_memory_train_test.prototxt) to each YARN container, and also set the relevant PATH of each Spark driver/executor to LD_LIBRARY_PATH, which is defined in the previous code snippet and points to the location that has CaffeOnSpark libraries.</span></span> 

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="424fe-190">Figyelés és hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="424fe-190">Monitoring and troubleshooting</span></span>

<span data-ttu-id="424fe-191">Mivel az YARN fürt mód használjuk, ebben az esetben a Spark illesztőprogram ütemezi egy tetszőleges tárolóhoz (és egy tetszőleges munkavégző csomópont) csak akkor jelenik meg a konzol kimeneti hasonlót:</span><span class="sxs-lookup"><span data-stu-id="424fe-191">Since we are using YARN cluster mode, in which case the Spark driver will be scheduled to an arbitrary container (and an arbitrary worker node) you should only see in the console outputting something like:</span></span>

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

<span data-ttu-id="424fe-192">Ha szeretné tudni, hogy mi történt, általában szeretné lekérni a Spark vezető naplót, amely tartalmaz további információt.</span><span class="sxs-lookup"><span data-stu-id="424fe-192">If you want to know what happened, you usually need to get the Spark driver's log, which has more information.</span></span> <span data-ttu-id="424fe-193">Ebben az esetben meg kell a YARN felhasználói felületen a megfelelő YARN naplóit kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="424fe-193">In this case, you need to go to the YARN UI to find the relevant YARN logs.</span></span> <span data-ttu-id="424fe-194">A YARN felhasználói felületen az URL-cím által szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="424fe-194">You can get the YARN UI by this URL:</span></span> 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN FELHASZNÁLÓI FELÜLETEN](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

<span data-ttu-id="424fe-196">Tekintse meg az adott alkalmazás hány erőforrásokat is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="424fe-196">You can take a look at how many resources are allocated for this particular application.</span></span> <span data-ttu-id="424fe-197">A "Feladatütemező" hivatkozásra kattinthat, és majd látni fogja, hogy ehhez az alkalmazáshoz nincsenek futó 9 tárolók.</span><span class="sxs-lookup"><span data-stu-id="424fe-197">You can click the "Scheduler" link, and then you will see that for this application, there are 9 containers running.</span></span> <span data-ttu-id="424fe-198">YARN 8 végrehajtója arra kérjük, és egy másik tárolóban illesztőprogram folyamat.</span><span class="sxs-lookup"><span data-stu-id="424fe-198">We ask YARN to provide 8 executors, and another container is for driver process.</span></span> 

![YARN Feladatütemező](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

<span data-ttu-id="424fe-200">Érdemes lehet ellenőrizze az illesztőprogram naplók és tároló naplókat, ha nincsenek hibák.</span><span class="sxs-lookup"><span data-stu-id="424fe-200">You may want to check the  driver logs or container logs if there are failures.</span></span> <span data-ttu-id="424fe-201">Az illesztőprogram-naplók az Alkalmazásazonosítót a YARN felhasználói felületen kattintson, majd kattintson a "Naplózza" gombra.</span><span class="sxs-lookup"><span data-stu-id="424fe-201">For driver logs, you can click the application ID in YARN UI, then click the "Logs" button.</span></span> <span data-ttu-id="424fe-202">Az stderr írja a az illesztőprogram-naplókat.</span><span class="sxs-lookup"><span data-stu-id="424fe-202">The driver logs are written into stderr.</span></span>

![YARN FELHASZNÁLÓI FELÜLETEN 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

<span data-ttu-id="424fe-204">Például előfordulhat, hogy megjelenik a hiba alább az illesztőprogram naplókból némelyike arról túl sok végrehajtója osszon ki.</span><span class="sxs-lookup"><span data-stu-id="424fe-204">For example, you might see some of the error below from the driver logs, indicating you allocate too many executors.</span></span>

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

<span data-ttu-id="424fe-205">Egyes esetekben a probléma akkor fordulhat elő, illesztőprogramok helyett végrehajtója.</span><span class="sxs-lookup"><span data-stu-id="424fe-205">Sometimes, the issue can happen in executors rather than drivers.</span></span> <span data-ttu-id="424fe-206">Ebben az esetben kell a tároló naplókban találhatók.</span><span class="sxs-lookup"><span data-stu-id="424fe-206">In this case, you need to check the container logs.</span></span> <span data-ttu-id="424fe-207">Mindig beolvasni a tároló naplókat, és majd kérje le a hibás tároló.</span><span class="sxs-lookup"><span data-stu-id="424fe-207">You can always get the container logs, and then get the failed container.</span></span> <span data-ttu-id="424fe-208">Például a hiba előfordulhat, hogy megfelel a Caffe futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="424fe-208">For example, you might meet this failure when running Caffe.</span></span>

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

<span data-ttu-id="424fe-209">Ebben az esetben kell beszereznie a sikertelen tárolóhely-azonosító (a fenti eset, hogy a rendszer container_1485916338528_0008_05_000005).</span><span class="sxs-lookup"><span data-stu-id="424fe-209">In this case, you need to get the failed container ID (in the above case, it is container_1485916338528_0008_05_000005).</span></span> <span data-ttu-id="424fe-210">Majd futtatásához szükséges</span><span class="sxs-lookup"><span data-stu-id="424fe-210">Then you need to run</span></span> 

    yarn logs -containerId container_1485916338528_0008_03_000005

<span data-ttu-id="424fe-211">az a headnode.</span><span class="sxs-lookup"><span data-stu-id="424fe-211">from the headnode.</span></span> <span data-ttu-id="424fe-212">Az ellenőrzés tároló hiba után oka GPU mód használatával (ahol érdemes inkább CPU mód) a lenet_memory_solver.prototxt.</span><span class="sxs-lookup"><span data-stu-id="424fe-212">After checking container failure, it is caused by using GPU mode (where you should use CPU mode instead) in lenet_memory_solver.prototxt.</span></span>

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a><span data-ttu-id="424fe-213">Eredmények beolvasása</span><span class="sxs-lookup"><span data-stu-id="424fe-213">Getting results</span></span>

<span data-ttu-id="424fe-214">Mivel azt 8 végrehajtója osztja fel, és a hálózati topológia egyszerű, csak elméletileg körülbelül 30 percet az eredmény futtatásához.</span><span class="sxs-lookup"><span data-stu-id="424fe-214">Since we are allocating 8 executors, and the network topology is simple, it should only take around 30 minutes to run the result.</span></span> <span data-ttu-id="424fe-215">A parancssorból, láthatja, hogy azt a modell használatba wasb:///mnist.model, és az eredmények nevű wasb: / / / mnist_features_result.</span><span class="sxs-lookup"><span data-stu-id="424fe-215">From the command line, you can see that we put the model to wasb:///mnist.model, and put the results to a folder named wasb:///mnist_features_result.</span></span>

<span data-ttu-id="424fe-216">Az eredményeket kaphat rendszerű</span><span class="sxs-lookup"><span data-stu-id="424fe-216">You can get the results by running</span></span>

    hadoop fs -cat hdfs:///mnist_features_result/*

<span data-ttu-id="424fe-217">és az eredmény a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="424fe-217">and the result looks like:</span></span>

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

<span data-ttu-id="424fe-218">A SampleID MNIST adatkészlet ID jelöli, és a címke a modell azonosító szám.</span><span class="sxs-lookup"><span data-stu-id="424fe-218">The SampleID represents the ID in the MNIST dataset, and the label is the number that the model identifies.</span></span>


## <a name="conclusion"></a><span data-ttu-id="424fe-219">Összegzés</span><span class="sxs-lookup"><span data-stu-id="424fe-219">Conclusion</span></span>

<span data-ttu-id="424fe-220">Ebben a dokumentációban próbált CaffeOnSpark telepítése a futó egy egyszerű példa.</span><span class="sxs-lookup"><span data-stu-id="424fe-220">In this documentation, you have tried to install CaffeOnSpark with running a simple example.</span></span> <span data-ttu-id="424fe-221">HDInsight egy teljes felügyelt felhőalapú elosztott számítási platformot, és a legjobb hely a nagy adatkészlet gépi tanulási és speciális elemzésekre munkaterhelések futtatása, és elosztott mély tanulási, használhatja Caffe a HDInsight Spark mély tanulási feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="424fe-221">HDInsight is a full managed cloud distributed compute platform, and is the best place for running machine learning and advanced analytics workloads on large data set, and for distributed deep learning, you can use Caffe on HDInsight Spark to perform deep learning tasks.</span></span>


## <span data-ttu-id="424fe-222"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="424fe-222"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="424fe-223">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="424fe-223">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="424fe-224">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="424fe-224">Scenarios</span></span>
* [<span data-ttu-id="424fe-225">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="424fe-225">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="424fe-226">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="424fe-226">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a><span data-ttu-id="424fe-227">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="424fe-227">Manage resources</span></span>
* [<span data-ttu-id="424fe-228">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="424fe-228">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

