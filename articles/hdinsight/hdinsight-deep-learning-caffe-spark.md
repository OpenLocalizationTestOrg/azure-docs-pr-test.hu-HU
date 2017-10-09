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
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a><span data-ttu-id="a1491-103">Az Azure HDInsight Spark Caffe elosztott mély tanulási használata</span><span class="sxs-lookup"><span data-stu-id="a1491-103">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>


## <a name="introduction"></a><span data-ttu-id="a1491-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="a1491-104">Introduction</span></span>

<span data-ttu-id="a1491-105">Mély tanulási egészségügyi tootransportation toomanufacturing eltávolításában, és több van hatással.</span><span class="sxs-lookup"><span data-stu-id="a1491-105">Deep learning is impacting everything from healthcare tootransportation toomanufacturing, and more.</span></span> <span data-ttu-id="a1491-106">Vállalatok be toosolve rögzített problémák, például a tanulási toodeep [besorolás kép](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [beszédfelismerés](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), a felismerési objektum, és a gépi fordítás.</span><span class="sxs-lookup"><span data-stu-id="a1491-106">Companies are turning toodeep learning toosolve hard problems, like [image classification](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [speech recognition](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), object recognition, and machine translation.</span></span> 

<span data-ttu-id="a1491-107">Nincsenek [számos népszerű keretrendszerekre](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), többek között a következőket [Microsoft kognitív eszközkészlet](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, stb. Caffe egyik hello leghíresebb nem szimbolikus (imperatív) Neurális hálózat keretrendszerek, és számos területen, például a számítógép stratégiai széles körben használt.</span><span class="sxs-lookup"><span data-stu-id="a1491-107">There are [many popular frameworks](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), including [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, etc. Caffe is one of hello most famous non-symbolic (imperative) neural network frameworks, and widely used in many areas including computer vision.</span></span> <span data-ttu-id="a1491-108">Ezenkívül [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinálja az Apache Spark, ebben az esetben a mély tanulási Caffe könnyen használható egy létező Hadoop cluster együtt Spark ETL folyamatok, csökkenti a rendszer összetettségét és végpont tanulás késését.</span><span class="sxs-lookup"><span data-stu-id="a1491-108">Furthermore, [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) combines Caffe with Apache Spark, in which case deep learning can be easily used on an existing Hadoop cluster together with Spark ETL pipelines, reducing system complexity and latency for end-to-end learning.</span></span>

<span data-ttu-id="a1491-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) hello csak teljes körűen felügyelt felhőalapú Hadoop ajánlat, amely optimalizálva nyílt forráskódú elemzési fürtök Spark, Hive, MapReduce, HBase, Storm, Kafka és R Server üzemelnek 99,9 %-os SLA-t.</span><span class="sxs-lookup"><span data-stu-id="a1491-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) is hello only fully-managed cloud Hadoop offering that provides optimized open source analytic clusters for Spark, Hive, MapReduce, HBase, Storm, Kafka, and R Server backed by a 99.9% SLA.</span></span> <span data-ttu-id="a1491-110">Ezen big data technológiák és ISV-alkalmazások mindegyike könnyűszerrel helyezhető üzembe felügyelt fürtök formájában, nagyvállalati szintű biztonsággal és figyeléssel.</span><span class="sxs-lookup"><span data-stu-id="a1491-110">Each of these big data technologies and ISV applications are easily deployable as managed clusters with enterprise-level security and monitoring.</span></span>

<span data-ttu-id="a1491-111">Egyes felhasználók arra kérjük, kapcsolatos részletes a hdinsight platformon, amely a Microsoft PaaS Hadoop termék tanulási toouse.</span><span class="sxs-lookup"><span data-stu-id="a1491-111">Some users are asking us about how toouse deep learning on HDInsight, which is Microsoft's PaaS Hadoop product.</span></span> <span data-ttu-id="a1491-112">Azt el fogja tudni jövőbeli hello további tooshare, de még ma azt szeretnénk, hogy hogyan műszaki blog toosummarize toouse Caffe a HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="a1491-112">We will have more tooshare in hello future, but today we want toosummarize a technical blog on how toouse Caffe on HDInsight Spark.</span></span>

<span data-ttu-id="a1491-113">Ha Caffe előtt telepítette, megfigyelheti, hogy ezt a keretrendszert rendszer telepíteni egy kicsit kihívást.</span><span class="sxs-lookup"><span data-stu-id="a1491-113">If you have installed Caffe before, you will notice that installing this framework is a little bit challenging.</span></span> <span data-ttu-id="a1491-114">Az ebben a blogban azt először mutatják be hogyan tooinstall [a Spark Caffe](https://github.com/yahoo/CaffeOnSpark) a HDInsight-fürtök, majd használja hello beépített MNIST bemutató toodemostrate hogyan toouse elosztott mély tanulási használata a HDInsight Spark CPU-n.</span><span class="sxs-lookup"><span data-stu-id="a1491-114">In this blog, we will first illustrate how tooinstall [Caffe on Spark](https://github.com/yahoo/CaffeOnSpark) for an HDInsight cluster, then use hello built-in MNIST demo toodemostrate how toouse Distributed Deep Learning using HDInsight Spark on CPUs.</span></span>

<span data-ttu-id="a1491-115">Nincsenek négy fő lépést tooget, akkor a HDInsight működni.</span><span class="sxs-lookup"><span data-stu-id="a1491-115">There are four major steps tooget it work on HDInsight.</span></span>

1. <span data-ttu-id="a1491-116">Minden hello csomópontján hello szükséges függőségek telepítése</span><span class="sxs-lookup"><span data-stu-id="a1491-116">Install hello required dependencies on all hello nodes</span></span>
2. <span data-ttu-id="a1491-117">A HDInsight Spark hello központi csomóponton Caffe épül</span><span class="sxs-lookup"><span data-stu-id="a1491-117">Build Caffe on Spark for HDInsight on hello head node</span></span>
3. <span data-ttu-id="a1491-118">Szükséges hello szalagtárak tooall hello munkavégző csomópontokhoz terjesztése</span><span class="sxs-lookup"><span data-stu-id="a1491-118">Distribute hello required libraries tooall hello worker nodes</span></span>
4. <span data-ttu-id="a1491-119">A Caffe modellek írása, és futtassa azt distributely</span><span class="sxs-lookup"><span data-stu-id="a1491-119">Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="a1491-120">Mivel a HDInsight a PaaS megoldás, azt funkciókat nyújtja a kiváló platform -, hogy könnyen tooperform néhány feladatot.</span><span class="sxs-lookup"><span data-stu-id="a1491-120">Since HDInsight is a PaaS solution, it offers great platform features - so it is quite easy tooperform some tasks.</span></span> <span data-ttu-id="a1491-121">Hello szolgáltatása, amely a következő blogbejegyzésben fokozottan használjuk nevezik [parancsfájlművelet](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), amely Ön is végrehajthatja a rendszerhéj parancsok toocustomize fürtcsomópontok (átjárócsomópont, munkavégző csomópont vagy élcsomópont).</span><span class="sxs-lookup"><span data-stu-id="a1491-121">One of hello features that we heavily use in this blog post is called [Script Action](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), with which you can execute shell commands toocustomize cluster nodes (head node, worker node, or edge node).</span></span>

## <a name="step-1--install-hello-required-dependencies-on-all-hello-nodes"></a><span data-ttu-id="a1491-122">1. lépés: Hello szükséges függőségek telepítése hello minden csomópontján</span><span class="sxs-lookup"><span data-stu-id="a1491-122">Step 1:  Install hello required dependencies on all hello nodes</span></span>

<span data-ttu-id="a1491-123">tooget elindult, igazolnia kell a tooinstall hello függőségek szükséges.</span><span class="sxs-lookup"><span data-stu-id="a1491-123">tooget started, we need tooinstall hello dependencies we need.</span></span> <span data-ttu-id="a1491-124">hello Caffe hely és [CaffeOnSpark hely](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) kínál néhány nagyon hasznos wiki hello függőségek telepítése a Spark a YARN módban (amely a HDInsight Spark hello módban), de ellenőriznünk kell tooadd néhány további függőségek HDInsight platformon.</span><span class="sxs-lookup"><span data-stu-id="a1491-124">hello Caffe site and [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) offers some very useful wiki for installing hello dependencies for Spark on YARN mode (which is hello mode for HDInsight Spark), but we need tooadd a few more dependencies for HDInsight platform.</span></span> <span data-ttu-id="a1491-125">Rendszer hello parancsfájlművelet használja az alábbi, és futtassa az összes hello átjárócsomópontokkal és feldolgozó csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="a1491-125">We will use hello script action as below and run it on all hello head nodes and worker nodes.</span></span> <span data-ttu-id="a1491-126">A parancsfájlművelet lépnek körülbelül 20 percet, mivel azok a többi csomagot is függ.</span><span class="sxs-lookup"><span data-stu-id="a1491-126">This script action will take about 20 minutes, as those dependencies also depend on other packages.</span></span> <span data-ttu-id="a1491-127">Néhány helyen elérhető tooyour HDInsight-fürtjéhez, például a GitHub-helyét vagy hello alapértelmezett BLOB storage-fiók akkor kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="a1491-127">You should put it in some location that is accessible tooyour HDInsight cluster, such as a GitHub location or hello default BLOB storage account.</span></span>

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


<span data-ttu-id="a1491-128">A fenti hello parancsfájlművelet két lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="a1491-128">There are two steps in hello script action above.</span></span> <span data-ttu-id="a1491-129">első lépés hello tooinstall az összes szükséges kódtárak hello.</span><span class="sxs-lookup"><span data-stu-id="a1491-129">hello first step is tooinstall all hello required libraries.</span></span> <span data-ttu-id="a1491-130">A tárak hello szükséges kódtárak (például gflags, glog) Caffe fordítása és Caffe fut (például numpy) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a1491-130">Those libraries include hello necessary libraries for both compiling Caffe(such as gflags, glog) and running Caffe (such as numpy).</span></span> <span data-ttu-id="a1491-131">CPU-optimalizálásra libatlas használunk, de más optimalizálási könyvtárak, például MKL vagy CUDA (GPU) a telepítési hello CaffeOnSpark wiki mindig kövesse.</span><span class="sxs-lookup"><span data-stu-id="a1491-131">We are using libatlas for CPU optimization, but you can always follow hello CaffeOnSpark wiki on installing other optimization libraries, such as MKL or CUDA (for GPU).</span></span>

<span data-ttu-id="a1491-132">hello második lépésben toodownload, fordítása, és telepítse protobuf 2.5.0 Caffe a futtatás során.</span><span class="sxs-lookup"><span data-stu-id="a1491-132">hello second step is toodownload, compile, and install protobuf 2.5.0 for Caffe during runtime.</span></span> <span data-ttu-id="a1491-133">Protobuf 2.5.0 [szükséges](https://github.com/yahoo/CaffeOnSpark/issues/87), azonban a jelen verziójában nem áll rendelkezésre Ubuntu 16, a csomag, ezért ellenőriznünk kell, hogy toocompile hello forráskód le.</span><span class="sxs-lookup"><span data-stu-id="a1491-133">Protobuf 2.5.0 [is required](https://github.com/yahoo/CaffeOnSpark/issues/87), however this version is not available as a package on Ubuntu 16, so we need toocompile it from hello source code.</span></span> <span data-ttu-id="a1491-134">Van még néhány erőforrások a hello Internet hogyan toocompile, például a [Ez](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span><span class="sxs-lookup"><span data-stu-id="a1491-134">There are also a few resources on hello Internet on how toocompile it, such as [this](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span></span>

<span data-ttu-id="a1491-135">Ismerkedés a toosimply, csak a parancsfájlművelet alapján futtathatók a fürt tooall hello munkavégző csomópontok és a head csomópontok (a HDInsight 3.5).</span><span class="sxs-lookup"><span data-stu-id="a1491-135">toosimply get started, you can just run this script action against your cluster tooall hello worker nodes and head nodes (for HDInsight 3.5).</span></span> <span data-ttu-id="a1491-136">Hello Parancsfájlműveletek futó fürt futtatásával, vagy hello Parancsfájlműveletek hello fürt létesítése idő alatt is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="a1491-136">You can either run hello script actions for a running cluster, or you can also run hello script actions during hello cluster provision time.</span></span> <span data-ttu-id="a1491-137">Hello Parancsfájlműveletek a további részletekért lásd: hello dokumentáció [Itt](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span><span class="sxs-lookup"><span data-stu-id="a1491-137">For more details on hello script actions, please see hello documentation [here](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span></span>

![Parancsfájl-műveletek tooInstall függőségek](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-hello-head-node"></a><span data-ttu-id="a1491-139">2. lépés: Létrehozása Caffe a hdinsight Spark-kiszolgálón hello átjárócsomópont</span><span class="sxs-lookup"><span data-stu-id="a1491-139">Step 2: Build Caffe on Spark for HDInsight on hello head node</span></span>

<span data-ttu-id="a1491-140">hello második lépése a hello headnode Caffe toobuild, és közzétennie a lefordított hello szalagtárak tooall hello munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="a1491-140">hello second step is toobuild Caffe on hello headnode, and then distribute hello compiled libraries tooall hello worker nodes.</span></span> <span data-ttu-id="a1491-141">Ebben a lépésben szüksége lesz túl[ssh azokat a headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), kövesse egyszerűen hello [CaffeOnSpark build folyamat](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), és az alábbiakban található néhány további lépést toobuild CaffeOnSpark használható hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="a1491-141">In this step, you will need too[ssh into your headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), then simply follow hello [CaffeOnSpark build process](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), and below is hello script you can use toobuild CaffeOnSpark with a few additional steps.</span></span> 

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

<span data-ttu-id="a1491-142">Szükség lehet a toodo több mint milyen hello dokumentációját CaffeOnSpark szerint.</span><span class="sxs-lookup"><span data-stu-id="a1491-142">You may need toodo more than what hello documentation of CaffeOnSpark says.</span></span> <span data-ttu-id="a1491-143">hello változások a következők:</span><span class="sxs-lookup"><span data-stu-id="a1491-143">hello changes are:</span></span>
- <span data-ttu-id="a1491-144">Csak tooCPU módosítsa, majd libatlas használni erre a célra.</span><span class="sxs-lookup"><span data-stu-id="a1491-144">Change tooCPU only and use libatlas for this particular purpose.</span></span>
- <span data-ttu-id="a1491-145">PUT hello adatkészletek toohello BLOB-tároló, amely egy megosztott hely, amely elérhető tooall munkavégző csomópontokhoz későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="a1491-145">Put hello datasets toohello BLOB storage, which is a shared location that is accessible tooall worker nodes for later use.</span></span>
- <span data-ttu-id="a1491-146">PUT hello Caffe szalagtárak tooBLOB tárolási lefordítva, és később fogja másolni ezen parancsfájl műveletek tooavoid további a fordítás során használt szalagtárak tooall hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="a1491-146">Put hello compiled Caffe libraries tooBLOB storage, and later you will copy those libraries tooall hello nodes using script actions tooavoid additional compilation time.</span></span>


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a><span data-ttu-id="a1491-147">Hibaelhárítás: Egy telepítsenek BuildException történt: exec adott vissza: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="a1491-147">Troubleshooting: An Ant BuildException has occured: exec returned: 2</span></span>

<span data-ttu-id="a1491-148">Amikor először próbálja toobuild CaffeOnSpark, néha azt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a1491-148">When first trying toobuild CaffeOnSpark, sometimes it will say</span></span>

    failed tooexecute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

<span data-ttu-id="a1491-149">Egyszerűen tiszta hello kód tárház által "tiszta ellenőrizze", és ezután futtatási "később build" lesz a probléma megoldásához, mindaddig, amíg hello helyes függőség van.</span><span class="sxs-lookup"><span data-stu-id="a1491-149">Simply clean hello code repository by "make clean" and then run "make build" will solve this issue, as long as you have hello correct dependencies.</span></span>

### <a name="troubleshooting-maven-repository-connection-time-out"></a><span data-ttu-id="a1491-150">Hibaelhárítás: Maven tárház kapcsolat időtúllépése</span><span class="sxs-lookup"><span data-stu-id="a1491-150">Troubleshooting: Maven repository connection time out</span></span>

<span data-ttu-id="a1491-151">Egyes esetekben maven ad hello kapcsolat időtúllépési hiba, hasonló toobelow:</span><span class="sxs-lookup"><span data-stu-id="a1491-151">Sometimes maven gives me hello connection time out error, similar toobelow:</span></span>

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request too{s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

<span data-ttu-id="a1491-152">OK lehet néhány perc várakozás után, és próbálja csak toorebuild hello kódot, így előfordulhat, hogy Maven valamilyen módon korlátok hello egy adott IP-címről érkező forgalmat.</span><span class="sxs-lookup"><span data-stu-id="a1491-152">It will be OK after waiting for a few minutes and then just try toorebuild hello code, so it might be Maven somehow limits hello traffic from a given IP address.</span></span>


### <a name="troubleshooting-test-failure-for-caffe"></a><span data-ttu-id="a1491-153">Hibáinak elhárítása: Caffe hiba teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="a1491-153">Troubleshooting: Test failure for Caffe</span></span>

<span data-ttu-id="a1491-154">Valószínűleg látni fogja a teszt sikertelen CaffeOnSpark, hasonló az alábbi hello végső ellenőrzése során.</span><span class="sxs-lookup"><span data-stu-id="a1491-154">You probably will see a test failure when doing hello final check for CaffeOnSpark, similar with below.</span></span> <span data-ttu-id="a1491-155">Ez az UTF-8 kódolással kapcsolatos prabably, de kell befolyásolja a Caffe hello használata</span><span class="sxs-lookup"><span data-stu-id="a1491-155">This is prabably related with UTF-8 encoding, but should not impact hello usage of Caffe</span></span>

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-hello-required-libraries-tooall-hello-worker-nodes"></a><span data-ttu-id="a1491-156">3. lépés: A szükséges hello szalagtárak tooall hello munkavégző csomópontokhoz terjesztése</span><span class="sxs-lookup"><span data-stu-id="a1491-156">Step 3: Distribute hello required libraries tooall hello worker nodes</span></span>

<span data-ttu-id="a1491-157">hello következő lépésre toodistribute hello függvénytárak (alapvetően hello CaffeOnSpark/caffe-nyilvános/terjesztése/lib tárak/és CaffeOnSpark/caffe-distri/terjesztése/lib /) tooall hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="a1491-157">hello next step is toodistribute hello libraries (basically hello libraries in CaffeOnSpark/caffe-public/distribute/lib/ and CaffeOnSpark/caffe-distri/distribute/lib/) tooall hello nodes.</span></span> <span data-ttu-id="a1491-158">2. lépésben, azt a tárak elhelyezése BLOB-tároló, és ebben a lépésben a parancsfájl-műveletek toocopy használjuk, tooall hello átjárócsomópontokkal és feldolgozó csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="a1491-158">In Step 2, we put those libraries on BLOB storage, and in this step, we will use script actions toocopy it tooall hello head nodes and worker nodes.</span></span>

<span data-ttu-id="a1491-159">toodo a, a parancsfájlművelet futtassa az alábbi egyszerű (kell toopoint toohello helyen adott tooyour fürt):</span><span class="sxs-lookup"><span data-stu-id="a1491-159">toodo this, simple run a script action as below (you need toopoint toohello right location specific tooyour cluster):</span></span>

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

<span data-ttu-id="a1491-160">A 2. lépésben, azt helyezése hello BLOB-tároló, amely elérhető tooall hello csomópontok, mert ebben a lépésben azt csak egyszerűen másolhatja azt tooall hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="a1491-160">Because in step 2, we put it on hello BLOB storage which is accessible tooall hello nodes, in this step we just simply copy it tooall hello nodes.</span></span>

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a><span data-ttu-id="a1491-161">4. lépés: A Caffe modellek írása, és futtassa azt distributely</span><span class="sxs-lookup"><span data-stu-id="a1491-161">Step 4: Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="a1491-162">Miután hello a fenti lépéseket, Caffe képezve hello headnode telepítve, és jó toogo vagy folyamatban.</span><span class="sxs-lookup"><span data-stu-id="a1491-162">After running hello above steps, Caffe is alreay installed on hello headnode and we are good toogo.</span></span> <span data-ttu-id="a1491-163">következő lépés hello toowrite Caffe modell.</span><span class="sxs-lookup"><span data-stu-id="a1491-163">hello next step is toowrite a Caffe model.</span></span> 

<span data-ttu-id="a1491-164">Caffe használ egy "kifejező architektúra", ha a modell csak akkor kell toodefine egy konfigurációs fájl létrehozása, és minden (a legtöbb esetben) kódolás nélkül.</span><span class="sxs-lookup"><span data-stu-id="a1491-164">Caffe is using an "expressive architecture", where for composing a model, you just need toodefine a configuration file, and without coding at all (in most cases).</span></span> <span data-ttu-id="a1491-165">Ezért a következőkben van.</span><span class="sxs-lookup"><span data-stu-id="a1491-165">So let's take a look there.</span></span> 

<span data-ttu-id="a1491-166">azt ma még betanítani hello modell egy olyan minta modell MNIST képzési.</span><span class="sxs-lookup"><span data-stu-id="a1491-166">hello model we will train today is a sample model for MNIST training.</span></span> <span data-ttu-id="a1491-167">hello MNIST adatbázis kézzel számjegyek 60 000 példák betanítási készlete, és 10 000 példák TesztKészlet rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a1491-167">hello MNIST database of handwritten digits has a training set of 60,000 examples, and a test set of 10,000 examples.</span></span> <span data-ttu-id="a1491-168">A korábbiakhoz NIST elérhető egy részét is.</span><span class="sxs-lookup"><span data-stu-id="a1491-168">It is a subset of a larger set available from NIST.</span></span> <span data-ttu-id="a1491-169">hello számjegyek mérete normalizált és a rögzített méretű kép középre törölték.</span><span class="sxs-lookup"><span data-stu-id="a1491-169">hello digits have been size-normalized and centered in a fixed-size image.</span></span> <span data-ttu-id="a1491-170">CaffeOnSpark rendelkezik néhány parancsfájlok toodownload hello adatkészletet, majd átalakíthatja hello megfelelő formátumú.</span><span class="sxs-lookup"><span data-stu-id="a1491-170">CaffeOnSpark has some scripts toodownload hello dataset and convert it into hello right format.</span></span>

<span data-ttu-id="a1491-171">CaffeOnSpark MNIST képzési biztosít a hálózati topológiák példákat.</span><span class="sxs-lookup"><span data-stu-id="a1491-171">CaffeOnSpark provides some network topologies example for MNIST training.</span></span> <span data-ttu-id="a1491-172">A megosztással hello hálózati architektúra (hello hálózat topológiája hello) és optimalizálási töltött kialakítást rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a1491-172">It has a nice design of splitting hello network architecture (hello topology of hello network) and optimization.</span></span> <span data-ttu-id="a1491-173">Ebben az esetben két fájl van szükség:</span><span class="sxs-lookup"><span data-stu-id="a1491-173">In this case, There are two files required:</span></span> 

<span data-ttu-id="a1491-174">hello "Solver" ({CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt$) fájllal hello optimalizálási felügyeletéért és paraméter frissítések létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a1491-174">hello "Solver" file (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) is used for overseeing hello optimization and generating parameter updates.</span></span> <span data-ttu-id="a1491-175">Például azt határozza meg hogy Processzor- vagy gpu-t fogja használni, mi az az hello mérlegserpenyőre, hogyan sok a közelítés lesz, stb. Is meghatározza, melyik idegsejt hálózati topológia kell hello program használata (amely hello második fájl szükséges).</span><span class="sxs-lookup"><span data-stu-id="a1491-175">For example, it defines whether CPU or GPU will be used, what's hello momentum, how many iterations will be, etc. It also defines which neuron network topology should hello program use (which is hello second file we need).</span></span> <span data-ttu-id="a1491-176">A Solver kapcsolatos további információkért tekintse meg az túl[Caffe dokumentáció](http://caffe.berkeleyvision.org/tutorial/solver.html).</span><span class="sxs-lookup"><span data-stu-id="a1491-176">For more information about Solver, please refer too[Caffe documentation](http://caffe.berkeleyvision.org/tutorial/solver.html).</span></span>

<span data-ttu-id="a1491-177">Ehhez a példához mivel GPU, hanem CPU használjuk kell módosítjuk hello utolsó sort:</span><span class="sxs-lookup"><span data-stu-id="a1491-177">For this example, since we are using CPU rather than GPU, we should change hello last line to:</span></span>

    # solver mode: CPU or GPU
    solver_mode: CPU

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

<span data-ttu-id="a1491-179">Más sorok igény szerint módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="a1491-179">You can change other lines as needed.</span></span>

<span data-ttu-id="a1491-180">hello második fájl ({CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt$) határozza meg, hogyan hello idegsejt hálózati néz, és hello vonatkozó bemeneti és kimeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="a1491-180">hello second file (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) defines how hello neuron network looks like, and hello relevant input and output file.</span></span> <span data-ttu-id="a1491-181">Tooupdate hello fájl tooreflect hello betanítási adatok helye is szükséges.</span><span class="sxs-lookup"><span data-stu-id="a1491-181">We also need tooupdate hello file tooreflect hello training data location.</span></span> <span data-ttu-id="a1491-182">Változás hello lenet_memory_train_test.prototxt (kell toopoint toohello helyen adott tooyour fürt) részt a következő:</span><span class="sxs-lookup"><span data-stu-id="a1491-182">Change hello following part in lenet_memory_train_test.prototxt (you need toopoint toohello right location specific tooyour cluster):</span></span>

- <span data-ttu-id="a1491-183">túl módosítása hello "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" "wasb: / / / projektek/machine_learning/image_dataset/mnist_train_lmdb"</span><span class="sxs-lookup"><span data-stu-id="a1491-183">change hello "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" too"wasb:///projects/machine_learning/image_dataset/mnist_train_lmdb"</span></span>
- <span data-ttu-id="a1491-184">Módosítsa a "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" túl "wasb: / / / projektek/machine_learning/image_dataset/mnist_test_lmdb"</span><span class="sxs-lookup"><span data-stu-id="a1491-184">change "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" too"wasb:///projects/machine_learning/image_dataset/mnist_test_lmdb"</span></span>

![Caffe Config](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

<span data-ttu-id="a1491-186">Hogyan toodefine hello hálózati további információkért tekintse meg az hello [MNIST dataset Caffe dokumentációja](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span><span class="sxs-lookup"><span data-stu-id="a1491-186">For more information on how toodefine hello network, please check hello [Caffe documentation on MNIST dataset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span></span>

<span data-ttu-id="a1491-187">Az ebben a blogban hello célra csak használjuk a egyszerű MNIST példa.</span><span class="sxs-lookup"><span data-stu-id="a1491-187">For hello purpose of this blog, we just use this simple MNIST example.</span></span> <span data-ttu-id="a1491-188">Az átjárócsomópont hello futtatnia kell hello-parancsot:</span><span class="sxs-lookup"><span data-stu-id="a1491-188">You should run hello command below from hello head node:</span></span>

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

<span data-ttu-id="a1491-189">Alapvetően szükséges hello terjesztett fájlok (lenet_memory_solver.prototxt és lenet_memory_train_test.prototxt) tooeach fonal tárolót, és állítson be úgy a megfelelő ELÉRÉSI útját minden egyes Spark illesztőprogram/végrehajtó tooLD_LIBRARY_PATH, amelyet a hello hello Előző kód részlet és pontok toohello olyan helyet, amelynek CaffeOnSpark szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="a1491-189">Basically it distributes hello required files (lenet_memory_solver.prototxt and lenet_memory_train_test.prototxt) tooeach YARN container, and also set hello relevant PATH of each Spark driver/executor tooLD_LIBRARY_PATH, which is defined in hello previous code snippet and points toohello location that has CaffeOnSpark libraries.</span></span> 

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="a1491-190">Figyelés és hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="a1491-190">Monitoring and troubleshooting</span></span>

<span data-ttu-id="a1491-191">Azt a YARN fürt módot, mivel ebben az esetben hello Spark illesztőprogram lesz-e ütemezett tooan tetszőleges tárolóhoz (és egy tetszőleges munkavégző csomópont) csak akkor jelenik meg a hello konzol írása alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="a1491-191">Since we are using YARN cluster mode, in which case hello Spark driver will be scheduled tooan arbitrary container (and an arbitrary worker node) you should only see in hello console outputting something like:</span></span>

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

<span data-ttu-id="a1491-192">Ha szeretné tooknow, mi történt, általában szüksége tooget hello Spark illesztőprogram naplót, amely további részleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a1491-192">If you want tooknow what happened, you usually need tooget hello Spark driver's log, which has more information.</span></span> <span data-ttu-id="a1491-193">Ebben az esetben szüksége toogo toohello YARN felhasználói felületen toofind hello vonatkozó YARN naplóit.</span><span class="sxs-lookup"><span data-stu-id="a1491-193">In this case, you need toogo toohello YARN UI toofind hello relevant YARN logs.</span></span> <span data-ttu-id="a1491-194">Kaphat a YARN felhasználói felületen az URL-cím által hello:</span><span class="sxs-lookup"><span data-stu-id="a1491-194">You can get hello YARN UI by this URL:</span></span> 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN FELHASZNÁLÓI FELÜLETEN](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

<span data-ttu-id="a1491-196">Tekintse meg az adott alkalmazás hány erőforrásokat is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="a1491-196">You can take a look at how many resources are allocated for this particular application.</span></span> <span data-ttu-id="a1491-197">Hello "Feladatütemező" hivatkozásra kattinthat, és majd látni fogja, hogy ehhez az alkalmazáshoz nincsenek futó 9 tárolók.</span><span class="sxs-lookup"><span data-stu-id="a1491-197">You can click hello "Scheduler" link, and then you will see that for this application, there are 9 containers running.</span></span> <span data-ttu-id="a1491-198">8 végrehajtója YARN tooprovide megkérjük, és egy másik tárolóban illesztőprogram folyamat.</span><span class="sxs-lookup"><span data-stu-id="a1491-198">We ask YARN tooprovide 8 executors, and another container is for driver process.</span></span> 

![YARN Feladatütemező](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

<span data-ttu-id="a1491-200">Érdemes lehet toocheck hello illesztőprogram naplók és a tároló naplók hibák esetén.</span><span class="sxs-lookup"><span data-stu-id="a1491-200">You may want toocheck hello  driver logs or container logs if there are failures.</span></span> <span data-ttu-id="a1491-201">Az illesztőprogram-naplók hello Alkalmazásazonosítót a YARN felhasználói felületen kattintson, majd hello "Logs" gombra.</span><span class="sxs-lookup"><span data-stu-id="a1491-201">For driver logs, you can click hello application ID in YARN UI, then click hello "Logs" button.</span></span> <span data-ttu-id="a1491-202">az stderr írja a hello illesztőprogram naplókat.</span><span class="sxs-lookup"><span data-stu-id="a1491-202">hello driver logs are written into stderr.</span></span>

![YARN FELHASZNÁLÓI FELÜLETEN 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

<span data-ttu-id="a1491-204">Például előfordulhat, hogy megjelenik néhány hello hiba alább hello illesztőprogram naplókból arról túl sok végrehajtója osszon ki.</span><span class="sxs-lookup"><span data-stu-id="a1491-204">For example, you might see some of hello error below from hello driver logs, indicating you allocate too many executors.</span></span>

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

<span data-ttu-id="a1491-205">Egyes esetekben hello a probléma akkor fordulhat elő, illesztőprogramok helyett végrehajtója.</span><span class="sxs-lookup"><span data-stu-id="a1491-205">Sometimes, hello issue can happen in executors rather than drivers.</span></span> <span data-ttu-id="a1491-206">Ebben az esetben szüksége toocheck hello tároló naplókat.</span><span class="sxs-lookup"><span data-stu-id="a1491-206">In this case, you need toocheck hello container logs.</span></span> <span data-ttu-id="a1491-207">Akkor is mindig hello tároló naplókat, és majd hello sikertelen tároló.</span><span class="sxs-lookup"><span data-stu-id="a1491-207">You can always get hello container logs, and then get hello failed container.</span></span> <span data-ttu-id="a1491-208">Például a hiba előfordulhat, hogy megfelel a Caffe futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="a1491-208">For example, you might meet this failure when running Caffe.</span></span>

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

<span data-ttu-id="a1491-209">Ebben az esetben szüksége tooget sikertelen hello tárolóhely-azonosító (hello fenti eset, hogy a rendszer container_1485916338528_0008_05_000005).</span><span class="sxs-lookup"><span data-stu-id="a1491-209">In this case, you need tooget hello failed container ID (in hello above case, it is container_1485916338528_0008_05_000005).</span></span> <span data-ttu-id="a1491-210">Akkor szükséges, hogy toorun</span><span class="sxs-lookup"><span data-stu-id="a1491-210">Then you need toorun</span></span> 

    yarn logs -containerId container_1485916338528_0008_03_000005

<span data-ttu-id="a1491-211">a hello headnode.</span><span class="sxs-lookup"><span data-stu-id="a1491-211">from hello headnode.</span></span> <span data-ttu-id="a1491-212">Az ellenőrzés tároló hiba után oka GPU mód használatával (ahol érdemes inkább CPU mód) a lenet_memory_solver.prototxt.</span><span class="sxs-lookup"><span data-stu-id="a1491-212">After checking container failure, it is caused by using GPU mode (where you should use CPU mode instead) in lenet_memory_solver.prototxt.</span></span>

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written tooSTDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a><span data-ttu-id="a1491-213">Eredmények beolvasása</span><span class="sxs-lookup"><span data-stu-id="a1491-213">Getting results</span></span>

<span data-ttu-id="a1491-214">Mivel jelenleg 8 végrehajtója osztja fel, és egyszerű hello hálózati topológia, csak gyorsabban körülbelül 30 percet toorun hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="a1491-214">Since we are allocating 8 executors, and hello network topology is simple, it should only take around 30 minutes toorun hello result.</span></span> <span data-ttu-id="a1491-215">Hello parancssorból láthatja, hogy a Microsoft hello modell toowasb:///mnist.model put, és hogy a hello eredmények tooa mappájában wasb: / / / mnist_features_result.</span><span class="sxs-lookup"><span data-stu-id="a1491-215">From hello command line, you can see that we put hello model toowasb:///mnist.model, and put hello results tooa folder named wasb:///mnist_features_result.</span></span>

<span data-ttu-id="a1491-216">Futtassa a hello eredményeit szeretné beolvasni</span><span class="sxs-lookup"><span data-stu-id="a1491-216">You can get hello results by running</span></span>

    hadoop fs -cat hdfs:///mnist_features_result/*

<span data-ttu-id="a1491-217">és hello eredménye a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="a1491-217">and hello result looks like:</span></span>

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

<span data-ttu-id="a1491-218">hello SampleID hello azonosító hello MNIST adatkészlet jelöli, és hello címke hello szám, amely a modell hello azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="a1491-218">hello SampleID represents hello ID in hello MNIST dataset, and hello label is hello number that hello model identifies.</span></span>


## <a name="conclusion"></a><span data-ttu-id="a1491-219">Összegzés</span><span class="sxs-lookup"><span data-stu-id="a1491-219">Conclusion</span></span>

<span data-ttu-id="a1491-220">Ebben a dokumentációban próbált tooinstall CaffeOnSpark egy egyszerű példa futtatásával.</span><span class="sxs-lookup"><span data-stu-id="a1491-220">In this documentation, you have tried tooinstall CaffeOnSpark with running a simple example.</span></span> <span data-ttu-id="a1491-221">HDInsight egy teljes felügyelt felhőalapú elosztott számítási platformot, és hello a legjobb hely a nagy adatkészlet gépi tanulási és speciális elemzésekre munkaterhelések futtatása, és az elosztott mély learning, HDInsight Spark tooperform mély tanulási használhatja a Caffe feladatok.</span><span class="sxs-lookup"><span data-stu-id="a1491-221">HDInsight is a full managed cloud distributed compute platform, and is hello best place for running machine learning and advanced analytics workloads on large data set, and for distributed deep learning, you can use Caffe on HDInsight Spark tooperform deep learning tasks.</span></span>


## <span data-ttu-id="a1491-222"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a1491-222"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="a1491-223">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="a1491-223">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="a1491-224">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="a1491-224">Scenarios</span></span>
* [<span data-ttu-id="a1491-225">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="a1491-225">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="a1491-226">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="a1491-226">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a><span data-ttu-id="a1491-227">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="a1491-227">Manage resources</span></span>
* [<span data-ttu-id="a1491-228">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="a1491-228">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

