---
title: "Apache Kafka futó Storm - Azure aaaUse |} Microsoft Docs"
description: "Apache Kafka HDInsight alatt futó Apache Storm együtt települ. Ismerje meg, hogyan toowrite tooKafka, és azt, majd olvassa használatával hello Storm KafkaBolt és KafkaSpout összetevői. Megtudhatja, hogyan toouse hello fluxus keretrendszer toodefine, és küldje el a Storm-topológiák."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e4941329-1580-4cd8-b82e-a2258802c1a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: larryfr
ms.openlocfilehash: 95701f51dfdf6f1a859dcde96d7053df4f21701f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a><span data-ttu-id="aa77e-105">Apache Kafka (előzetes verzió) használata a HDInsight alatt futó Storm</span><span class="sxs-lookup"><span data-stu-id="aa77e-105">Use Apache Kafka (preview) with Storm on HDInsight</span></span>

<span data-ttu-id="aa77e-106">Megtudhatja, hogyan toouse alatt futó Apache Storm tooread az írási és olvasási tooApache Kafka.</span><span class="sxs-lookup"><span data-stu-id="aa77e-106">Learn how toouse Apache Storm tooread from and write tooApache Kafka.</span></span> <span data-ttu-id="aa77e-107">Ez a példa is mutatja be, hogyan toosave adatait egy Storm-topológia toohello HDFS-kompatibilis fájl használt HDInsight rendszert.</span><span class="sxs-lookup"><span data-stu-id="aa77e-107">This example also demonstrates how toosave data from a Storm topology toohello HDFS-compatible file system used by HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="aa77e-108">hello jelen dokumentumban leírt lépések, amely a HDInsight alatt futó Storm, mind a HDInsight-fürt Kafka tartalmazza az Azure erőforrás-csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aa77e-108">hello steps in this document create an Azure resource group that contains both a Storm on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="aa77e-109">Ezeken a fürtökön hello Kafka fürt kommunikálnak mindkét egy Azure virtuális hálózatot, amely lehetővé teszi a Storm-fürt toodirectly hello belül található.</span><span class="sxs-lookup"><span data-stu-id="aa77e-109">These clusters are both located within an Azure Virtual Network, which allows hello Storm cluster toodirectly communicate with hello Kafka cluster.</span></span>
> 
> <span data-ttu-id="aa77e-110">Amikor elkészült, a jelen dokumentumban leírt lépések hello, ne felejtse el toodelete hello fürtök tooavoid felesleges költségek.</span><span class="sxs-lookup"><span data-stu-id="aa77e-110">When you are done with hello steps in this document, remember toodelete hello clusters tooavoid excess charges.</span></span>

## <a name="get-hello-code"></a><span data-ttu-id="aa77e-111">Hello kód beolvasása</span><span class="sxs-lookup"><span data-stu-id="aa77e-111">Get hello code</span></span>

<span data-ttu-id="aa77e-112">Ez a dokumentum hello példában hello kódját érhető el: [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span><span class="sxs-lookup"><span data-stu-id="aa77e-112">hello code for hello example used in this document is available at [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span></span>

<span data-ttu-id="aa77e-113">toocompile ebben a projektben van szüksége a fejlesztési környezet konfigurációját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-113">toocompile this project, you need hello following configuration for your development environment:</span></span>

* <span data-ttu-id="aa77e-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="aa77e-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="aa77e-115">HDInsight 3.5-ös vagy újabb rendszer szükséges Java 8.</span><span class="sxs-lookup"><span data-stu-id="aa77e-115">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="aa77e-116">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="aa77e-116">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

* <span data-ttu-id="aa77e-117">Egy SSH-ügyfél (hello kell `ssh` és `scp` parancsok) - információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="aa77e-117">An SSH client (you need hello `ssh` and `scp` commands) - For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="aa77e-118">Egy szövegszerkesztőben, vagy IDE.</span><span class="sxs-lookup"><span data-stu-id="aa77e-118">A text editor or IDE.</span></span>

<span data-ttu-id="aa77e-119">hello következő környezeti változó lehet beállítani a fejlesztő munkaállomás Java és hello JDK telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="aa77e-119">hello following environment variables may be set when you install Java and hello JDK on your development workstation.</span></span> <span data-ttu-id="aa77e-120">Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="aa77e-120">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="aa77e-121">`JAVA_HOME`-toohello rendszert tartalmazó könyvtár hello JDK kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="aa77e-121">`JAVA_HOME` - should point toohello directory where hello JDK is installed.</span></span>
* <span data-ttu-id="aa77e-122">`PATH`-a következő elérési utak hello tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="aa77e-122">`PATH` - should contain hello following paths:</span></span>
  
    * <span data-ttu-id="aa77e-123">`JAVA_HOME`(vagy ezzel egyenértékű elérési hello).</span><span class="sxs-lookup"><span data-stu-id="aa77e-123">`JAVA_HOME` (or hello equivalent path).</span></span>
    * <span data-ttu-id="aa77e-124">`JAVA_HOME\bin`(vagy ezzel egyenértékű elérési hello).</span><span class="sxs-lookup"><span data-stu-id="aa77e-124">`JAVA_HOME\bin` (or hello equivalent path).</span></span>
    * <span data-ttu-id="aa77e-125">hello mappát, ahová a Maven telepítve van.</span><span class="sxs-lookup"><span data-stu-id="aa77e-125">hello directory where Maven is installed.</span></span>

## <a name="create-hello-clusters"></a><span data-ttu-id="aa77e-126">Hello fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="aa77e-126">Create hello clusters</span></span>

<span data-ttu-id="aa77e-127">A HDInsight Apache Kafka nem biztosít hozzáférést toohello Kafka brókerek képest hello a nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="aa77e-127">Apache Kafka on HDInsight does not provide access toohello Kafka brokers over hello public internet.</span></span> <span data-ttu-id="aa77e-128">Kafka fürt hello, amelyeket tooKafka kell lennie a megbeszélések hello hello csomópontként azonos Azure virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="aa77e-128">Anything that talks tooKafka must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="aa77e-129">Ehhez a példához hello Kafka és a Storm-fürtök találhatók egy Azure virtuális hálózatra.</span><span class="sxs-lookup"><span data-stu-id="aa77e-129">For this example, both hello Kafka and Storm clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="aa77e-130">a következő ábra azt mutatja be, hogyan kommunikációs hello fürtök között zajló kommunikációról hello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-130">hello following diagram shows how communication flows between hello clusters:</span></span>

![A Storm és Kafka fürtök egy Azure virtuális hálózati diagramja](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="aa77e-132">Egyéb szolgáltatások hello fürtön például az SSH és az Ambari keresztül is elérhető hello internet.</span><span class="sxs-lookup"><span data-stu-id="aa77e-132">Other services on hello cluster such as SSH and Ambari can be accessed over hello internet.</span></span> <span data-ttu-id="aa77e-133">Nyilvános és a HDInsight együttes rendelkezésre álló hello-portok további információkért lásd: [portok és a HDInsight által használt URI-azonosítók](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="aa77e-133">For more information on hello public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="aa77e-134">Létrehozhat egy Azure virtuális hálózatra, Kafka, és a Storm-fürtök manuális, akkor könnyebb toouse Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="aa77e-134">While you can create an Azure virtual network, Kafka, and Storm clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="aa77e-135">Használjon hello alábbi lépéseit toodeploy egy Azure virtuális hálózatra, Kafka, és Storm-fürtök tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="aa77e-135">Use hello following steps toodeploy an Azure virtual network, Kafka, and Storm clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="aa77e-136">A tooAzure gomb toosign és hello Azure-portál megnyitása hello sablon hello használata.</span><span class="sxs-lookup"><span data-stu-id="aa77e-136">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    <span data-ttu-id="aa77e-137">hello Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span><span class="sxs-lookup"><span data-stu-id="aa77e-137">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span></span> <span data-ttu-id="aa77e-138">Létrehozza a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-138">It creates hello following resources:</span></span>
    
    * <span data-ttu-id="aa77e-139">Azure-erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="aa77e-139">Azure resource group</span></span>
    * <span data-ttu-id="aa77e-140">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="aa77e-140">Azure Virtual Network</span></span>
    * <span data-ttu-id="aa77e-141">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="aa77e-141">Azure Storage account</span></span>
    * <span data-ttu-id="aa77e-142">A HDInsight 3.6 (három munkavégző csomópontokhoz) verzió Kafka</span><span class="sxs-lookup"><span data-stu-id="aa77e-142">Kafka on HDInsight version 3.6 (three worker nodes)</span></span>
    * <span data-ttu-id="aa77e-143">Verzió (három munkavégző csomópontokhoz) 3.6 HDInsight alatt futó Storm</span><span class="sxs-lookup"><span data-stu-id="aa77e-143">Storm on HDInsight version 3.6 (three worker nodes)</span></span>

  > [!WARNING]
  > <span data-ttu-id="aa77e-144">a HDInsight Kafka tooguarantee rendelkezésre állását, a fürt tartalmaznia kell legalább három munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="aa77e-144">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="aa77e-145">Ez a sablon a három munkavégző csomópontokhoz tartalmaz Kafka fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="aa77e-145">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="aa77e-146">Útmutatás toopopulate hello bejegyzések követően hello használata hello **egyéni telepítési** panel:</span><span class="sxs-lookup"><span data-stu-id="aa77e-146">Use hello following guidance toopopulate hello entries on hello **Custom deployment** blade:</span></span>
   
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * <span data-ttu-id="aa77e-148">**Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-148">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="aa77e-149">Ez a csoport hello HDInsight-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="aa77e-149">This group contains hello HDInsight cluster.</span></span>
   
    * <span data-ttu-id="aa77e-150">**Hely**: Válasszon egy helyet a földrajzi elhelyezkedés alapján Bezárás tooyou.</span><span class="sxs-lookup"><span data-stu-id="aa77e-150">**Location**: Select a location geographically close tooyou.</span></span>

    * <span data-ttu-id="aa77e-151">**Fürt neve kiinduló**: Ez az érték neveként hello alapszintű hello Storm és Kafka fürtök használható.</span><span class="sxs-lookup"><span data-stu-id="aa77e-151">**Base Cluster Name**: This value is used as hello base name for hello Storm and Kafka clusters.</span></span> <span data-ttu-id="aa77e-152">Például írja be **hdi** hoz létre egy Storm-fürt nevű **storm-hdi** és nevű Kafka fürt **kafka-hdi**.</span><span class="sxs-lookup"><span data-stu-id="aa77e-152">For example, entering **hdi** creates a Storm cluster named **storm-hdi** and a Kafka cluster named **kafka-hdi**.</span></span>
   
    * <span data-ttu-id="aa77e-153">**A fürt bejelentkezési felhasználónevét**: hello rendszergazda felhasználóneve hello Storm és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-153">**Cluster Login User Name**: hello admin user name for hello Storm and Kafka clusters.</span></span>
   
    * <span data-ttu-id="aa77e-154">**A fürt bejelentkezési jelszó**: hello rendszergazdai jelszóval hello Storm és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-154">**Cluster Login Password**: hello admin user password for hello Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="aa77e-155">**SSH-felhasználónév**: hello SSH felhasználói toocreate hello Storm és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-155">**SSH User Name**: hello SSH user toocreate for hello Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="aa77e-156">**SSH-jelszónak**: hello SSH felhasználó hello Storm és Kafka fürt hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="aa77e-156">**SSH Password**: hello password for hello SSH user for hello Storm and Kafka clusters.</span></span>

3. <span data-ttu-id="aa77e-157">Olvasási hello **feltételek és kikötések**, majd válassza ki **toohello feltételek és kikötések fenti elfogadom**.</span><span class="sxs-lookup"><span data-stu-id="aa77e-157">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="aa77e-158">Végül ellenőrizze **PIN-kód toodashboard** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="aa77e-158">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="aa77e-159">Körülbelül 20 percet toocreate hello fürtök szükséges.</span><span class="sxs-lookup"><span data-stu-id="aa77e-159">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="aa77e-160">Létrehozása után a hello erőforrásokat, hello erőforráscsoport hello panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="aa77e-160">Once hello resources have been created, hello blade for hello resource group is displayed.</span></span>

![Erőforráscsoport panel hello vnet és fürtök](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="aa77e-162">Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **storm-BASENAME** és **kafka-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét.</span><span class="sxs-lookup"><span data-stu-id="aa77e-162">Notice that hello names of hello HDInsight clusters are **storm-BASENAME** and **kafka-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="aa77e-163">Ezeket a neveket használja a későbbi lépésekben toohello fürtök kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="aa77e-163">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="understanding-hello-code"></a><span data-ttu-id="aa77e-164">Hello kód ismertetése</span><span class="sxs-lookup"><span data-stu-id="aa77e-164">Understanding hello code</span></span>

<span data-ttu-id="aa77e-165">A projekt két topológiát tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="aa77e-165">This project contains two topologies:</span></span>

* <span data-ttu-id="aa77e-166">**KafkaWriter**: hello által meghatározott **writer.yaml** fájl, ez a topológia ír véletlenszerű mondat tooKafka hello alatt futó Apache Storm megadott KafkaBolt használatával.</span><span class="sxs-lookup"><span data-stu-id="aa77e-166">**KafkaWriter**: Defined by hello **writer.yaml** file, this topology writes random sentences tooKafka using hello KafkaBolt provided with Apache Storm.</span></span>

    <span data-ttu-id="aa77e-167">Ez a topológia használ egy egyéni **SentenceSpout** összetevő toogenerate véletlenszerű mondatokat.</span><span class="sxs-lookup"><span data-stu-id="aa77e-167">This topology uses a custom **SentenceSpout** component toogenerate random sentences.</span></span>

* <span data-ttu-id="aa77e-168">**KafkaReader**: hello által meghatározott **reader.yaml** fájl, ez a topológia olvassa be az adatokat Kafka hello alatt futó Apache Storm megadott KafkaSpout használatával, majd a naplók hello adatok toostdout.</span><span class="sxs-lookup"><span data-stu-id="aa77e-168">**KafkaReader**: Defined by hello **reader.yaml** file, this topology reads data from Kafka using hello KafkaSpout provided with Apache Storm, then logs hello data toostdout.</span></span>

    <span data-ttu-id="aa77e-169">Ez a topológia hello Storm-fürt hello Storm HdfsBolt toowrite adatok toodefault tárolást használ.</span><span class="sxs-lookup"><span data-stu-id="aa77e-169">This topology uses hello Storm HdfsBolt toowrite data toodefault storage for hello Storm cluster.</span></span>
### <a name="flux"></a><span data-ttu-id="aa77e-170">Fluxus</span><span class="sxs-lookup"><span data-stu-id="aa77e-170">Flux</span></span>

<span data-ttu-id="aa77e-171">hello topológia meghatározása [fluxus](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="aa77e-171">hello topologies are defined using [Flux](https://storm.apache.org/releases/1.1.0/flux.html).</span></span> <span data-ttu-id="aa77e-172">Fluxus bemutatott Storm 0.10.x, és lehetővé teszi a tooseparate hello topológiakonfiguráció hello kódból.</span><span class="sxs-lookup"><span data-stu-id="aa77e-172">Flux was introduced in Storm 0.10.x and allows you tooseparate hello topology configuration from hello code.</span></span> <span data-ttu-id="aa77e-173">Hello fluxus keretrendszert használó topológia esetén a hello topológia YAM fájlban van definiálva.</span><span class="sxs-lookup"><span data-stu-id="aa77e-173">For Topologies that use hello Flux framework, hello topology is defined in a YAML file.</span></span> <span data-ttu-id="aa77e-174">hello YAM fájl hello topológia része lehet.</span><span class="sxs-lookup"><span data-stu-id="aa77e-174">hello YAML file can be included as part of hello topology.</span></span> <span data-ttu-id="aa77e-175">Amikor hello topológia használt önálló fájl is lehet.</span><span class="sxs-lookup"><span data-stu-id="aa77e-175">It can also be a standalone file used when you submit hello topology.</span></span> <span data-ttu-id="aa77e-176">Fluxus futásidőben, ebben a példában használt változók behelyettesítését is támogatja.</span><span class="sxs-lookup"><span data-stu-id="aa77e-176">Flux also supports variable substitution at run-time, which is used in this example.</span></span>

<span data-ttu-id="aa77e-177">hello következő paraméterei vannak beállítva az alábbi topológiák futás közben:</span><span class="sxs-lookup"><span data-stu-id="aa77e-177">hello following parameters are set at run time for these topologies:</span></span>

* <span data-ttu-id="aa77e-178">`${kafka.topic}`: hello Kafka témakör, amely hello topológiák olvasására vagy írására hello nevét.</span><span class="sxs-lookup"><span data-stu-id="aa77e-178">`${kafka.topic}`: hello name of hello Kafka topic that hello topologies read/write to.</span></span>

* <span data-ttu-id="aa77e-179">`${kafka.broker.hosts}`: hello üzemelteti, hogy hello Kafka brókerek futtatnak.</span><span class="sxs-lookup"><span data-stu-id="aa77e-179">`${kafka.broker.hosts}`: hello hosts that hello Kafka brokers run on.</span></span> <span data-ttu-id="aa77e-180">hello broker információknak az alkalmazása által hello KafkaBolt tooKafka írása közben.</span><span class="sxs-lookup"><span data-stu-id="aa77e-180">hello broker information is used by hello KafkaBolt when writing tooKafka.</span></span>

* <span data-ttu-id="aa77e-181">`${kafka.zookeeper.hosts}`: hello gazdagépeken futó Zookeeper a hello Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-181">`${kafka.zookeeper.hosts}`: hello hosts that Zookeeper runs on in hello Kafka cluster.</span></span>

<span data-ttu-id="aa77e-182">Fluxus topológiák további információkért lásd: [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="aa77e-182">For more information on Flux topologies, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="download-and-compile-hello-project"></a><span data-ttu-id="aa77e-183">Töltse le és hello projekt lefordítása</span><span class="sxs-lookup"><span data-stu-id="aa77e-183">Download and compile hello project</span></span>

1. <span data-ttu-id="aa77e-184">A fejlesztési környezet le hello projektet a [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), nyisson meg egy parancssori és hello projekt letöltött könyvtárak toohello helyének módosításához.</span><span class="sxs-lookup"><span data-stu-id="aa77e-184">On your development environment, download hello project from [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), open a command-line, and change directories toohello location that you downloaded hello project.</span></span>

2. <span data-ttu-id="aa77e-185">A hello **hdinsight-storm-java-kafka** könyvtárába, használjon hello következő toocompile hello projekt parancsot, és központi telepítési csomagot hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="aa77e-185">From hello **hdinsight-storm-java-kafka** directory, use hello following command toocompile hello project and create a package for deployment:</span></span>

  ```bash
  mvn clean package
  ```

    <span data-ttu-id="aa77e-186">hello csomag folyamat létrehoz egy fájlt nevű `KafkaTopology-1.0-SNAPSHOT.jar` a hello `target` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="aa77e-186">hello package process creates a file named `KafkaTopology-1.0-SNAPSHOT.jar` in hello `target` directory.</span></span>

3. <span data-ttu-id="aa77e-187">Használja a következő parancsok toocopy hello csomag tooyour Storm on HDInsight-fürt hello.</span><span class="sxs-lookup"><span data-stu-id="aa77e-187">Use hello following commands toocopy hello package tooyour Storm on HDInsight cluster.</span></span> <span data-ttu-id="aa77e-188">Cserélje le **felhasználónév** hello SSH felhasználónévvel hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="aa77e-188">Replace **USERNAME** with hello SSH user name for hello cluster.</span></span> <span data-ttu-id="aa77e-189">Cserélje le **BASENAME** hello Alap nevű hello fürt létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-189">Replace **BASENAME** with hello base name you used when creating hello cluster.</span></span>

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    <span data-ttu-id="aa77e-190">Amikor a rendszer kéri, adja meg a hello fürtök létrehozásakor használt hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="aa77e-190">When prompted, enter hello password you used when creating hello clusters.</span></span>

## <a name="configure-hello-topology"></a><span data-ttu-id="aa77e-191">Hello topológia konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aa77e-191">Configure hello topology</span></span>

1. <span data-ttu-id="aa77e-192">A következő módszerek toodiscover hello valamelyikével Kafka broker állomások hello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-192">Use one of hello following methods toodiscover hello Kafka broker hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="aa77e-193">hello Bash példa feltételezi, hogy `$CLUSTERNAME` hello HDInsight-fürt hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="aa77e-193">hello Bash example assumes that `$CLUSTERNAME` contains hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="aa77e-194">Azt is feltételezi, hogy [jq](https://stedolan.github.io/jq/) telepítve van.</span><span class="sxs-lookup"><span data-stu-id="aa77e-194">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="aa77e-195">Amikor a rendszer kéri, adja meg hello jelszó hello fürt bejelentkezési fiók.</span><span class="sxs-lookup"><span data-stu-id="aa77e-195">When prompted, enter hello password for hello cluster login account.</span></span>

    <span data-ttu-id="aa77e-196">hello visszaadott érték a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-196">hello value returned is similar toohello following text:</span></span>

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > <span data-ttu-id="aa77e-197">Előfordulhat, hogy a fürt legalább két broker gazdagépek, amíg nem kell tooprovide összes állomások tooclients teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="aa77e-197">While there may be more than two broker hosts for your cluster, you do not need tooprovide a full list of all hosts tooclients.</span></span> <span data-ttu-id="aa77e-198">Egy vagy két is elegendő.</span><span class="sxs-lookup"><span data-stu-id="aa77e-198">One or two is enough.</span></span>

2. <span data-ttu-id="aa77e-199">A következő módszerek toodiscover hello Kafka Zookeeper állomások hello egyikét használja:</span><span class="sxs-lookup"><span data-stu-id="aa77e-199">Use one of hello following methods toodiscover hello Kafka Zookeeper hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="aa77e-200">hello Bash példa feltételezi, hogy `$CLUSTERNAME` hello HDInsight-fürt hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="aa77e-200">hello Bash example assumes that `$CLUSTERNAME` contains hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="aa77e-201">Azt is feltételezi, hogy [jq](https://stedolan.github.io/jq/) telepítve van.</span><span class="sxs-lookup"><span data-stu-id="aa77e-201">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="aa77e-202">Amikor a rendszer kéri, adja meg hello jelszó hello fürt bejelentkezési fiók.</span><span class="sxs-lookup"><span data-stu-id="aa77e-202">When prompted, enter hello password for hello cluster login account.</span></span>

    <span data-ttu-id="aa77e-203">hello visszaadott érték a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-203">hello value returned is similar toohello following text:</span></span>

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > <span data-ttu-id="aa77e-204">Miközben kettőnél több Zookeeper csomópontok, nem kell tooprovide összes állomások tooclients teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="aa77e-204">While there are more than two Zookeeper nodes, you do not need tooprovide a full list of all hosts tooclients.</span></span> <span data-ttu-id="aa77e-205">Egy vagy két is elegendő.</span><span class="sxs-lookup"><span data-stu-id="aa77e-205">One or two is enough.</span></span>

    <span data-ttu-id="aa77e-206">Mentse ezt az értéket, a rendszer később.</span><span class="sxs-lookup"><span data-stu-id="aa77e-206">Save this value, as it is used later.</span></span>

3. <span data-ttu-id="aa77e-207">Hello szerkesztése `dev.properties` hello hello projekt gyökerében található fájl.</span><span class="sxs-lookup"><span data-stu-id="aa77e-207">Edit hello `dev.properties` file in hello root of hello project.</span></span> <span data-ttu-id="aa77e-208">Ebben a fájlban adja hozzá a hello Broker és Zookeeper állomások információk toohello egyező sor.</span><span class="sxs-lookup"><span data-stu-id="aa77e-208">Add hello Broker and Zookeeper hosts information toohello matching lines in this file.</span></span> <span data-ttu-id="aa77e-209">hello alábbi példa segítségével konfigurálható: hello mintaértékek hello előző lépéseiből:</span><span class="sxs-lookup"><span data-stu-id="aa77e-209">hello following example is configured using hello sample values from hello previous steps:</span></span>

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. <span data-ttu-id="aa77e-210">Mentse a hello `dev.properties` fájlt, majd használja hello parancs tooupload következő azt toohello Storm-fürt:</span><span class="sxs-lookup"><span data-stu-id="aa77e-210">Save hello `dev.properties` file and then use hello following command tooupload it toohello Storm cluster:</span></span>

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="aa77e-211">Cserélje le **felhasználónév** hello SSH felhasználónévvel hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="aa77e-211">Replace **USERNAME** with hello SSH user name for hello cluster.</span></span> <span data-ttu-id="aa77e-212">Cserélje le **BASENAME** hello Alap nevű hello fürt létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-212">Replace **BASENAME** with hello base name you used when creating hello cluster.</span></span>

## <a name="start-hello-writer"></a><span data-ttu-id="aa77e-213">Indítsa el a hello írója</span><span class="sxs-lookup"><span data-stu-id="aa77e-213">Start hello writer</span></span>

1. <span data-ttu-id="aa77e-214">A következő tooconnect toohello Storm-fürt SSH használatával hello használata.</span><span class="sxs-lookup"><span data-stu-id="aa77e-214">Use hello following tooconnect toohello Storm cluster using SSH.</span></span> <span data-ttu-id="aa77e-215">Cserélje le **felhasználónév** a hello hello fürt létrehozásakor használt SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="aa77e-215">Replace **USERNAME** with hello SSH user name used when creating hello cluster.</span></span> <span data-ttu-id="aa77e-216">Cserélje le **BASENAME** hello fürt létrehozásakor használt hello Alap nevű.</span><span class="sxs-lookup"><span data-stu-id="aa77e-216">Replace **BASENAME** with hello base name used when creating hello cluster.</span></span>

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    <span data-ttu-id="aa77e-217">Amikor a rendszer kéri, adja meg a hello fürtök létrehozásakor használt hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="aa77e-217">When prompted, enter hello password you used when creating hello clusters.</span></span>
   
    <span data-ttu-id="aa77e-218">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="aa77e-218">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="aa77e-219">Hello SSH-kapcsolat alkalmazás a következő parancs toocreate hello hello topológia által használt Kafka témakör hello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-219">From hello SSH connection, use hello following command toocreate hello Kafka topic used by hello topology:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    <span data-ttu-id="aa77e-220">Cserélje le `$KAFKAZKHOSTS` hello Zookeeper a gazdagép hello előző szakaszban lekért információt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-220">Replace `$KAFKAZKHOSTS` with hello Zookeeper host information you retrieved in hello previous section.</span></span>

2. <span data-ttu-id="aa77e-221">Hello SSH kapcsolat toohello Storm-fürt használja a következő parancs toostart hello író topológia hello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-221">From hello SSH connection toohello Storm cluster, use hello following command toostart hello writer topology:</span></span>

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    <span data-ttu-id="aa77e-222">a paranccsal hello paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="aa77e-222">hello parameters used with this command are:</span></span>

    * <span data-ttu-id="aa77e-223">`org.apache.storm.flux.Flux`: Fluxus tooconfigure használja, és futtassa ezt a topológiát.</span><span class="sxs-lookup"><span data-stu-id="aa77e-223">`org.apache.storm.flux.Flux`: Use Flux tooconfigure and run this topology.</span></span>

    * <span data-ttu-id="aa77e-224">`--remote`: Hello topológia tooNimbus nyújt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-224">`--remote`: Submit hello topology tooNimbus.</span></span> <span data-ttu-id="aa77e-225">hello topológia hello munkavégző csomópontokhoz hello fürt között van elosztva.</span><span class="sxs-lookup"><span data-stu-id="aa77e-225">hello topology is distributed across hello worker nodes in hello cluster.</span></span>

    * <span data-ttu-id="aa77e-226">`-R /writer.yaml`: Használat hello `writer.yaml` tooconfigure hello topológia fájlt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-226">`-R /writer.yaml`: Use hello `writer.yaml` file tooconfigure hello topology.</span></span> <span data-ttu-id="aa77e-227">`-R`azt jelzi, hogy ehhez az erőforráshoz hello jar-fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="aa77e-227">`-R` indicates that this resource is included in hello jar file.</span></span> <span data-ttu-id="aa77e-228">Hello jar hello gyökerébe ezért van `/writer.yaml` hello elérési tooit van.</span><span class="sxs-lookup"><span data-stu-id="aa77e-228">It's in hello root of hello jar, so `/writer.yaml` is hello path tooit.</span></span>

    * <span data-ttu-id="aa77e-229">`--filter`: Hello bejegyzések feltöltése `writer.yaml` topológiáját értékek hello `dev.properties` fájlt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-229">`--filter`: Populate entries in hello `writer.yaml` topology using values in hello `dev.properties` file.</span></span> <span data-ttu-id="aa77e-230">Például a hello értékének hello `kafka.topic` hello fájlban bejegyzés használt tooreplace hello `${kafka.topic}` hello topológia meghatározása bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="aa77e-230">For example, hello value of hello `kafka.topic` entry in hello file is used tooreplace hello `${kafka.topic}` entry in hello topology definition.</span></span>

5. <span data-ttu-id="aa77e-231">Amikor hello topológia elindult, használja a hello parancs tooverify, hogy az adatok toohello Kafka témakör ír a következő:</span><span class="sxs-lookup"><span data-stu-id="aa77e-231">Once hello topology has started, use hello following command tooverify that it is writing data toohello Kafka topic:</span></span>

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    <span data-ttu-id="aa77e-232">Cserélje le `$KAFKAZKHOSTS` hello Zookeeper a gazdagép hello előző szakaszban lekért információt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-232">Replace `$KAFKAZKHOSTS` with hello Zookeeper host information you retrieved in hello previous section.</span></span>

    <span data-ttu-id="aa77e-233">Ez a parancs Kafka toomonitor hello témakör rendszerrel szállított parancsfájlt használ.</span><span class="sxs-lookup"><span data-stu-id="aa77e-233">This command uses a script shipped with Kafka toomonitor hello topic.</span></span> <span data-ttu-id="aa77e-234">Néhány percet, miután kell kezdenie véletlen toohello témakör írt mondatokat vissza.</span><span class="sxs-lookup"><span data-stu-id="aa77e-234">After a moment, it should start returning random sentences that have been written toohello topic.</span></span> <span data-ttu-id="aa77e-235">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="aa77e-235">hello output is similar toohello following example:</span></span>

        i am at two with nature             
        an apple a day keeps hello doctor away
        snow white and hello seven dwarfs     
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        four score and seven years ago      
        snow white and hello seven dwarfs     
        snow white and hello seven dwarfs     
        i am at two with nature             
        an apple a day keeps hello doctor away

    <span data-ttu-id="aa77e-236">Használja a Ctrl + c toostop hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="aa77e-236">Use Ctrl+c toostop hello script.</span></span>

## <a name="start-hello-reader"></a><span data-ttu-id="aa77e-237">Indítsa el a hello olvasó</span><span class="sxs-lookup"><span data-stu-id="aa77e-237">Start hello reader</span></span>

1. <span data-ttu-id="aa77e-238">Hello SSH munkamenet toohello Storm-fürt használja a következő parancs toostart hello olvasó topológia hello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-238">From hello SSH session toohello Storm cluster, use hello following command toostart hello reader topology:</span></span>

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. <span data-ttu-id="aa77e-239">Miután hello topológia elindítja, nyissa meg a hello Storm felhasználói felülete.</span><span class="sxs-lookup"><span data-stu-id="aa77e-239">Once hello topology starts, open hello Storm UI.</span></span> <span data-ttu-id="aa77e-240">A webes felhasználói felület https://storm-BASENAME.azurehdinsight.net/stormui helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="aa77e-240">This web UI is located at https://storm-BASENAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="aa77e-241">Cserélje le __BASENAME__ hello fürt létrehozásakor használt hello Alap nevű.</span><span class="sxs-lookup"><span data-stu-id="aa77e-241">Replace __BASENAME__ with hello base name used when hello cluster was created.</span></span> 

    <span data-ttu-id="aa77e-242">Amikor a rendszer kéri, hello rendszergazda bejelentkezési név használata (alapértelmezés szerint `admin`) és hello fürt létrehozásakor használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="aa77e-242">When prompted, use hello admin login name (default, `admin`) and password used when hello cluster was created.</span></span> <span data-ttu-id="aa77e-243">Megjelenik egy weblap hasonló toohello kép a következő:</span><span class="sxs-lookup"><span data-stu-id="aa77e-243">You see a web page similar toohello following image:</span></span>

    ![A Storm felhasználói felülete](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. <span data-ttu-id="aa77e-245">Hello Storm felhasználói felülete, válassza ki hello __kafka-olvasó__ hello hivatkozásra __topológia összegzése__ hello toodisplay információ szakasz __kafka-olvasó__ topológia.</span><span class="sxs-lookup"><span data-stu-id="aa77e-245">From hello Storm UI, select hello __kafka-reader__ link in hello __Topology Summary__ section toodisplay information about hello __kafka-reader__ topology.</span></span>

    ![Hello Storm webes felhasználói felület összefoglaló szakasza topológia](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. <span data-ttu-id="aa77e-247">hello naplózó-bolt összetevő, jelölje be hello hello példányai toodisplay információ __naplózó-bolt__ hello hivatkozásra __Boltokhoz (mindig)__ szakasz.</span><span class="sxs-lookup"><span data-stu-id="aa77e-247">toodisplay information about hello instances of hello logger-bolt component, select hello __logger-bolt__ link in hello __Bolts (All time)__ section.</span></span>

    ![Hello boltokhoz szakaszban naplózó-bolt-hivatkozás](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. <span data-ttu-id="aa77e-249">A hello __végrehajtója__ területen válassza ki a hivatkozást a hello __Port__ a hello-összetevő példányának oszlop toodisplay naplózási információt.</span><span class="sxs-lookup"><span data-stu-id="aa77e-249">In hello __Executors__ section, select a link in hello __Port__ column toodisplay logging information about this instance of hello component.</span></span>

    ![Végrehajtója hivatkozás](./media/hdinsight-apache-storm-with-kafka/executors.png)

    <span data-ttu-id="aa77e-251">hello napló hello Kafka témakörben olvasásakor hello adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="aa77e-251">hello log contains a log of hello data read from hello Kafka topic.</span></span> <span data-ttu-id="aa77e-252">hello információ hello található a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="aa77e-252">hello information in hello log is similar toohello following text:</span></span>

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] hello cow jumped over hello moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: hello cow jumped over hello moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps hello doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps hello doctor away

## <a name="stop-hello-topologies"></a><span data-ttu-id="aa77e-253">Állítsa le a hello topológiák</span><span class="sxs-lookup"><span data-stu-id="aa77e-253">Stop hello topologies</span></span>

<span data-ttu-id="aa77e-254">SSH munkamenet toohello alatt futó Storm fürtök a következő parancsok toostop hello Storm-topológiák hello használata:</span><span class="sxs-lookup"><span data-stu-id="aa77e-254">From an SSH session toohello Storm cluster, use hello following commands toostop hello Storm topologies:</span></span>

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-hello-cluster"></a><span data-ttu-id="aa77e-255">Hello fürt törlése</span><span class="sxs-lookup"><span data-stu-id="aa77e-255">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="aa77e-256">Mivel a jelen dokumentumban leírt lépések hello létrehozása a fürtök hello azonos Azure-erőforráscsoportot, törölheti a hello erőforráscsoportja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="aa77e-256">Since hello steps in this document create both clusters in hello same Azure resource group, you can delete hello resource group in hello Azure portal.</span></span> <span data-ttu-id="aa77e-257">Hello erőforráscsoport törlése eltávolítja a dokumentum a következő által létrehozott összes erőforrás.</span><span class="sxs-lookup"><span data-stu-id="aa77e-257">Deleting hello resource group removes all resources created by following this document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa77e-258">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa77e-258">Next steps</span></span>

<span data-ttu-id="aa77e-259">Tekintse meg a HDInsight alatt futó Storm használható további példa topológiák [példa Storm-topológiák és összetevők](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="aa77e-259">For more example topologies that can be used with Storm on HDInsight, see [Example Storm topologies and components](hdinsight-storm-example-topology.md).</span></span>

<span data-ttu-id="aa77e-260">A telepítését és megfigyelését a HDInsight Linux-alapú topológiák további információkért lásd: [központi telepítése és kezelése a Linux-alapú HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="aa77e-260">For information on deploying and monitoring topologies on Linux-based HDInsight, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>