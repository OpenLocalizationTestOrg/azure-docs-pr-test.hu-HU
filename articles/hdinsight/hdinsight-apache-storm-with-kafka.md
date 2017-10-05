---
title: "Apache Kafka használata a HDInsight - Azure alatt futó Storm |} Microsoft Docs"
description: "Apache Kafka HDInsight alatt futó Apache Storm együtt települ. Megtudhatja, hogyan Kafka írni, és majd olvasni, a Storm megadott KafkaBolt és KafkaSpout összetevővel. Is megtudhatja, hogyan határozza meg, és küldje el a Storm-topológiák fluxus keretében segítségével."
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
ms.openlocfilehash: e8895ef3c11aea48513e4060a20f5f49b11fc961
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a><span data-ttu-id="125f8-105">Apache Kafka (előzetes verzió) használata a HDInsight alatt futó Storm</span><span class="sxs-lookup"><span data-stu-id="125f8-105">Use Apache Kafka (preview) with Storm on HDInsight</span></span>

<span data-ttu-id="125f8-106">Útmutató: az olvasási és írási Apache Kafka alatt futó Apache Storm használatával.</span><span class="sxs-lookup"><span data-stu-id="125f8-106">Learn how to use Apache Storm to read from and write to Apache Kafka.</span></span> <span data-ttu-id="125f8-107">Ez a példa is mutatja be a HDFS-kompatibilis fájlrendszer használják a HDInsight a Storm-topológia adatainak mentése.</span><span class="sxs-lookup"><span data-stu-id="125f8-107">This example also demonstrates how to save data from a Storm topology to the HDFS-compatible file system used by HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="125f8-108">A jelen dokumentumban leírt lépések, amely a HDInsight alatt futó Storm, mind a HDInsight-fürt Kafka tartalmazza az Azure erőforrás-csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="125f8-108">The steps in this document create an Azure resource group that contains both a Storm on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="125f8-109">Ezeken a fürtökön is egy Azure virtuális hálózatot, amely lehetővé teszi a Storm-fürt közvetlenül kommunikálni a Kafka belül található a fürtön.</span><span class="sxs-lookup"><span data-stu-id="125f8-109">These clusters are both located within an Azure Virtual Network, which allows the Storm cluster to directly communicate with the Kafka cluster.</span></span>
> 
> <span data-ttu-id="125f8-110">Amikor elkészült, a jelen dokumentumban leírt lépések, ne felejtse el a felesleges költségek elkerülése érdekében a fürtök törlése.</span><span class="sxs-lookup"><span data-stu-id="125f8-110">When you are done with the steps in this document, remember to delete the clusters to avoid excess charges.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="125f8-111">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="125f8-111">Get the code</span></span>

<span data-ttu-id="125f8-112">Ebben a dokumentumban bemutatott példában kódja megtalálható [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span><span class="sxs-lookup"><span data-stu-id="125f8-112">The code for the example used in this document is available at [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span></span>

<span data-ttu-id="125f8-113">Ez a projekt fordítása, a következő konfigurációs a fejlesztési környezet szüksége:</span><span class="sxs-lookup"><span data-stu-id="125f8-113">To compile this project, you need the following configuration for your development environment:</span></span>

* <span data-ttu-id="125f8-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="125f8-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="125f8-115">HDInsight 3.5-ös vagy újabb rendszer szükséges Java 8.</span><span class="sxs-lookup"><span data-stu-id="125f8-115">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="125f8-116">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="125f8-116">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

* <span data-ttu-id="125f8-117">Egy SSH-ügyfél (van szüksége a `ssh` és `scp` parancsok) - információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="125f8-117">An SSH client (you need the `ssh` and `scp` commands) - For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="125f8-118">Egy szövegszerkesztőben, vagy IDE.</span><span class="sxs-lookup"><span data-stu-id="125f8-118">A text editor or IDE.</span></span>

<span data-ttu-id="125f8-119">Az alábbi környezeti változókat a fejlesztő munkaállomás Java és a JDK telepítésekor lehet beállítani.</span><span class="sxs-lookup"><span data-stu-id="125f8-119">The following environment variables may be set when you install Java and the JDK on your development workstation.</span></span> <span data-ttu-id="125f8-120">Azonban ellenőrizni kell, hogy léteznek, illetve a rendszer a megfelelő értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="125f8-120">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="125f8-121">`JAVA_HOME`-a a JDK mappáját kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="125f8-121">`JAVA_HOME` - should point to the directory where the JDK is installed.</span></span>
* <span data-ttu-id="125f8-122">`PATH`-a következő elérési utakat kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="125f8-122">`PATH` - should contain the following paths:</span></span>
  
    * <span data-ttu-id="125f8-123">`JAVA_HOME`(vagy ezzel egyenértékű elérési).</span><span class="sxs-lookup"><span data-stu-id="125f8-123">`JAVA_HOME` (or the equivalent path).</span></span>
    * <span data-ttu-id="125f8-124">`JAVA_HOME\bin`(vagy ezzel egyenértékű elérési).</span><span class="sxs-lookup"><span data-stu-id="125f8-124">`JAVA_HOME\bin` (or the equivalent path).</span></span>
    * <span data-ttu-id="125f8-125">A mappát, ahová a Maven telepítve van.</span><span class="sxs-lookup"><span data-stu-id="125f8-125">The directory where Maven is installed.</span></span>

## <a name="create-the-clusters"></a><span data-ttu-id="125f8-126">A fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="125f8-126">Create the clusters</span></span>

<span data-ttu-id="125f8-127">A HDInsight Apache Kafka nem férhet hozzá a Kafka brókerek a nyilvános interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="125f8-127">Apache Kafka on HDInsight does not provide access to the Kafka brokers over the public internet.</span></span> <span data-ttu-id="125f8-128">A Kafka fürt csomópontja azonos Azure virtuális hálózaton, amelyeket a kiszolgálóhoz csatlakozik Kafka kell lennie.</span><span class="sxs-lookup"><span data-stu-id="125f8-128">Anything that talks to Kafka must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="125f8-129">Ehhez a példához a Kafka és a Storm-fürtök egy Azure virtuális hálózat található.</span><span class="sxs-lookup"><span data-stu-id="125f8-129">For this example, both the Kafka and Storm clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="125f8-130">Az alábbi ábra bemutatja, hogyan kommunikációs a fürtök között zajló kommunikációról:</span><span class="sxs-lookup"><span data-stu-id="125f8-130">The following diagram shows how communication flows between the clusters:</span></span>

![A Storm és Kafka fürtök egy Azure virtuális hálózati diagramja](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="125f8-132">Az interneten keresztül elérhető egyéb szolgáltatások, például az SSH és az Ambari a fürtön.</span><span class="sxs-lookup"><span data-stu-id="125f8-132">Other services on the cluster such as SSH and Ambari can be accessed over the internet.</span></span> <span data-ttu-id="125f8-133">A hdinsight eszközzel elérhető nyilvános portokon további információkért lásd: [portok és a HDInsight által használt URI-azonosítók](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="125f8-133">For more information on the public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="125f8-134">Létrehozhat egy Azure virtuális hálózatra, Kafka, és a Storm-fürtök manuális, célszerűbb Azure Resource Manager sablonnal.</span><span class="sxs-lookup"><span data-stu-id="125f8-134">While you can create an Azure virtual network, Kafka, and Storm clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="125f8-135">Az alábbi lépések segítségével telepíthet egy Azure virtuális hálózatot, Kafka, és a Storm-fürtök az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="125f8-135">Use the following steps to deploy an Azure virtual network, Kafka, and Storm clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="125f8-136">A következő gomb segítségével jelentkezzen be az Azure-ba, és nyissa meg a sablon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="125f8-136">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    <span data-ttu-id="125f8-137">Az Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span><span class="sxs-lookup"><span data-stu-id="125f8-137">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span></span> <span data-ttu-id="125f8-138">Létrehozza a következőket:</span><span class="sxs-lookup"><span data-stu-id="125f8-138">It creates the following resources:</span></span>
    
    * <span data-ttu-id="125f8-139">Azure-erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="125f8-139">Azure resource group</span></span>
    * <span data-ttu-id="125f8-140">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="125f8-140">Azure Virtual Network</span></span>
    * <span data-ttu-id="125f8-141">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="125f8-141">Azure Storage account</span></span>
    * <span data-ttu-id="125f8-142">A HDInsight 3.6 (három munkavégző csomópontokhoz) verzió Kafka</span><span class="sxs-lookup"><span data-stu-id="125f8-142">Kafka on HDInsight version 3.6 (three worker nodes)</span></span>
    * <span data-ttu-id="125f8-143">Verzió (három munkavégző csomópontokhoz) 3.6 HDInsight alatt futó Storm</span><span class="sxs-lookup"><span data-stu-id="125f8-143">Storm on HDInsight version 3.6 (three worker nodes)</span></span>

  > [!WARNING]
  > <span data-ttu-id="125f8-144">A HDInsightban futó Kafka platform rendelkezésre állásának biztosításához fürtjének legalább három feldolgozó csomópontot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="125f8-144">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="125f8-145">Ez a sablon a három munkavégző csomópontokhoz tartalmaz Kafka fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="125f8-145">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="125f8-146">Az alábbi útmutató segítségével a feltöltik a **egyéni telepítési** panel:</span><span class="sxs-lookup"><span data-stu-id="125f8-146">Use the following guidance to populate the entries on the **Custom deployment** blade:</span></span>
   
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * <span data-ttu-id="125f8-148">**Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="125f8-148">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="125f8-149">Ez a csoport tartalmazza a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="125f8-149">This group contains the HDInsight cluster.</span></span>
   
    * <span data-ttu-id="125f8-150">**Hely**: Adjon meg egy földrajzilag Önhöz legközelebb eső helyet.</span><span class="sxs-lookup"><span data-stu-id="125f8-150">**Location**: Select a location geographically close to you.</span></span>

    * <span data-ttu-id="125f8-151">**Fürt neve kiinduló**: ezt az értéket használja, a Storm alapnevét és Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="125f8-151">**Base Cluster Name**: This value is used as the base name for the Storm and Kafka clusters.</span></span> <span data-ttu-id="125f8-152">Például írja be **hdi** hoz létre egy Storm-fürt nevű **storm-hdi** és nevű Kafka fürt **kafka-hdi**.</span><span class="sxs-lookup"><span data-stu-id="125f8-152">For example, entering **hdi** creates a Storm cluster named **storm-hdi** and a Kafka cluster named **kafka-hdi**.</span></span>
   
    * <span data-ttu-id="125f8-153">**A fürt bejelentkezési felhasználónevét**: A rendszergazda felhasználóneve a Storm és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="125f8-153">**Cluster Login User Name**: The admin user name for the Storm and Kafka clusters.</span></span>
   
    * <span data-ttu-id="125f8-154">**A fürt bejelentkezési jelszó**: a Storm és Kafka fürt rendszergazdai jelszóval.</span><span class="sxs-lookup"><span data-stu-id="125f8-154">**Cluster Login Password**: The admin user password for the Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="125f8-155">**SSH-felhasználónév**: az SSH-felhasználó a Storm és Kafka fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="125f8-155">**SSH User Name**: The SSH user to create for the Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="125f8-156">**SSH-jelszónak**: a Storm és Kafka fürt SSH-felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="125f8-156">**SSH Password**: The password for the SSH user for the Storm and Kafka clusters.</span></span>

3. <span data-ttu-id="125f8-157">Olvassa el a **feltételek és kikötések**, majd válassza ki **elfogadom a feltételeket és a fenti feltételek**.</span><span class="sxs-lookup"><span data-stu-id="125f8-157">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="125f8-158">Végül ellenőrizze **rögzítés az irányítópulton** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="125f8-158">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="125f8-159">A fürt létrehozása nagyjából 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="125f8-159">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="125f8-160">Az erőforrások létrehozása után, az erőforráscsoport panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="125f8-160">Once the resources have been created, the blade for the resource group is displayed.</span></span>

![A virtuális hálózat és a fürtök erőforráscsoport panel](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="125f8-162">Figyelje meg, hogy a HDInsight-fürtök neve **storm-BASENAME** és **kafka-BASENAME**, ahol BASENAME a sablonhoz megadott név.</span><span class="sxs-lookup"><span data-stu-id="125f8-162">Notice that the names of the HDInsight clusters are **storm-BASENAME** and **kafka-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="125f8-163">Ezeket a neveket a későbbi lépésekben használja, a fürtök történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="125f8-163">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="understanding-the-code"></a><span data-ttu-id="125f8-164">A kód ismertetése</span><span class="sxs-lookup"><span data-stu-id="125f8-164">Understanding the code</span></span>

<span data-ttu-id="125f8-165">A projekt két topológiát tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="125f8-165">This project contains two topologies:</span></span>

* <span data-ttu-id="125f8-166">**KafkaWriter**: által meghatározott a **writer.yaml** fájl, ez a topológia ír véletlenszerű mondat Kafka az Apache Storm megadott KafkaBolt használatával.</span><span class="sxs-lookup"><span data-stu-id="125f8-166">**KafkaWriter**: Defined by the **writer.yaml** file, this topology writes random sentences to Kafka using the KafkaBolt provided with Apache Storm.</span></span>

    <span data-ttu-id="125f8-167">Ez a topológia használ egy egyéni **SentenceSpout** összetevő véletlenszerű mondat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="125f8-167">This topology uses a custom **SentenceSpout** component to generate random sentences.</span></span>

* <span data-ttu-id="125f8-168">**KafkaReader**: által meghatározott a **reader.yaml** fájl, ez a topológia Kafka az Apache Storm megadott KafkaSpout segítségével olvassa be az adatokat, majd az adatokat naplózza az stdout.</span><span class="sxs-lookup"><span data-stu-id="125f8-168">**KafkaReader**: Defined by the **reader.yaml** file, this topology reads data from Kafka using the KafkaSpout provided with Apache Storm, then logs the data to stdout.</span></span>

    <span data-ttu-id="125f8-169">Ez a topológia a Storm HdfsBolt adatokat írni az alapértelmezett tároló a Storm-fürt használja.</span><span class="sxs-lookup"><span data-stu-id="125f8-169">This topology uses the Storm HdfsBolt to write data to default storage for the Storm cluster.</span></span>
### <a name="flux"></a><span data-ttu-id="125f8-170">Fluxus</span><span class="sxs-lookup"><span data-stu-id="125f8-170">Flux</span></span>

<span data-ttu-id="125f8-171">A topológia meghatározása [fluxus](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="125f8-171">The topologies are defined using [Flux](https://storm.apache.org/releases/1.1.0/flux.html).</span></span> <span data-ttu-id="125f8-172">Fluxus bemutatott Storm 0.10.x, és lehetővé teszi a topológia konfigurációjával kód külön.</span><span class="sxs-lookup"><span data-stu-id="125f8-172">Flux was introduced in Storm 0.10.x and allows you to separate the topology configuration from the code.</span></span> <span data-ttu-id="125f8-173">A fluxus keretrendszert használó topológia esetén a topológia egy YAM fájlban definiálva van.</span><span class="sxs-lookup"><span data-stu-id="125f8-173">For Topologies that use the Flux framework, the topology is defined in a YAML file.</span></span> <span data-ttu-id="125f8-174">A YAM fájl a topológia része lehet.</span><span class="sxs-lookup"><span data-stu-id="125f8-174">The YAML file can be included as part of the topology.</span></span> <span data-ttu-id="125f8-175">A topológia elküldésekor használt önálló fájl is lehet.</span><span class="sxs-lookup"><span data-stu-id="125f8-175">It can also be a standalone file used when you submit the topology.</span></span> <span data-ttu-id="125f8-176">Fluxus futásidőben, ebben a példában használt változók behelyettesítését is támogatja.</span><span class="sxs-lookup"><span data-stu-id="125f8-176">Flux also supports variable substitution at run-time, which is used in this example.</span></span>

<span data-ttu-id="125f8-177">Az alábbi topológiák futási időben vannak megadva a következő paraméterek:</span><span class="sxs-lookup"><span data-stu-id="125f8-177">The following parameters are set at run time for these topologies:</span></span>

* <span data-ttu-id="125f8-178">`${kafka.topic}`: A Kafka témakör, amely a topológiák olvasására vagy írására a neve.</span><span class="sxs-lookup"><span data-stu-id="125f8-178">`${kafka.topic}`: The name of the Kafka topic that the topologies read/write to.</span></span>

* <span data-ttu-id="125f8-179">`${kafka.broker.hosts}`: A gazdagépek a Kafka brókerek futtatható.</span><span class="sxs-lookup"><span data-stu-id="125f8-179">`${kafka.broker.hosts}`: The hosts that the Kafka brokers run on.</span></span> <span data-ttu-id="125f8-180">A broker információknak az alkalmazása által a KafkaBolt Kafka írásakor.</span><span class="sxs-lookup"><span data-stu-id="125f8-180">The broker information is used by the KafkaBolt when writing to Kafka.</span></span>

* <span data-ttu-id="125f8-181">`${kafka.zookeeper.hosts}`: A gazdagépeken futó Zookeeper a Kafka fürtben.</span><span class="sxs-lookup"><span data-stu-id="125f8-181">`${kafka.zookeeper.hosts}`: The hosts that Zookeeper runs on in the Kafka cluster.</span></span>

<span data-ttu-id="125f8-182">Fluxus topológiák további információkért lásd: [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="125f8-182">For more information on Flux topologies, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="download-and-compile-the-project"></a><span data-ttu-id="125f8-183">Töltse le és a projekt lefordítása</span><span class="sxs-lookup"><span data-stu-id="125f8-183">Download and compile the project</span></span>

1. <span data-ttu-id="125f8-184">A fejlesztési környezetben, töltse le a projektet a [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), nyisson meg egy parancssori és abba a mappába, hogy a projekt letöltött.</span><span class="sxs-lookup"><span data-stu-id="125f8-184">On your development environment, download the project from [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), open a command-line, and change directories to the location that you downloaded the project.</span></span>

2. <span data-ttu-id="125f8-185">Az a **hdinsight-storm-java-kafka** directory, a projekt lefordítása és központi telepítési csomagot hozhat létre a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="125f8-185">From the **hdinsight-storm-java-kafka** directory, use the following command to compile the project and create a package for deployment:</span></span>

  ```bash
  mvn clean package
  ```

    <span data-ttu-id="125f8-186">A csomag folyamat létrehoz egy fájlt nevű `KafkaTopology-1.0-SNAPSHOT.jar` a a `target` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="125f8-186">The package process creates a file named `KafkaTopology-1.0-SNAPSHOT.jar` in the `target` directory.</span></span>

3. <span data-ttu-id="125f8-187">A következő parancsok segítségével másolja a csomagot a Storm on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="125f8-187">Use the following commands to copy the package to your Storm on HDInsight cluster.</span></span> <span data-ttu-id="125f8-188">Cserélje le **felhasználónév** rendelkező a fürthöz az SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="125f8-188">Replace **USERNAME** with the SSH user name for the cluster.</span></span> <span data-ttu-id="125f8-189">Cserélje le **BASENAME** a fürt létrehozásakor használt alap névvel.</span><span class="sxs-lookup"><span data-stu-id="125f8-189">Replace **BASENAME** with the base name you used when creating the cluster.</span></span>

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    <span data-ttu-id="125f8-190">Amikor a rendszer kéri, írja be a jelszót, a fürt létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="125f8-190">When prompted, enter the password you used when creating the clusters.</span></span>

## <a name="configure-the-topology"></a><span data-ttu-id="125f8-191">A topológia konfigurálása</span><span class="sxs-lookup"><span data-stu-id="125f8-191">Configure the topology</span></span>

1. <span data-ttu-id="125f8-192">Az alábbi módszerek valamelyikével felderítése a Kafka broker gazdagépek:</span><span class="sxs-lookup"><span data-stu-id="125f8-192">Use one of the following methods to discover the Kafka broker hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
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
    > <span data-ttu-id="125f8-193">A Bash példa feltételezi, hogy `$CLUSTERNAME` a HDInsight-fürt nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="125f8-193">The Bash example assumes that `$CLUSTERNAME` contains the name of the HDInsight cluster.</span></span> <span data-ttu-id="125f8-194">Azt is feltételezi, hogy [jq](https://stedolan.github.io/jq/) telepítve van.</span><span class="sxs-lookup"><span data-stu-id="125f8-194">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="125f8-195">Amikor a rendszer kéri, adja meg a fürt bejelentkezési fiókjának jelszavát.</span><span class="sxs-lookup"><span data-stu-id="125f8-195">When prompted, enter the password for the cluster login account.</span></span>

    <span data-ttu-id="125f8-196">A visszaadott érték az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="125f8-196">The value returned is similar to the following text:</span></span>

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > <span data-ttu-id="125f8-197">Előfordulhat, hogy a fürt legalább két broker gazdagépek, amíg nem kell minden gazdagép ügyfelek teljes listáját tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="125f8-197">While there may be more than two broker hosts for your cluster, you do not need to provide a full list of all hosts to clients.</span></span> <span data-ttu-id="125f8-198">Egy vagy két is elegendő.</span><span class="sxs-lookup"><span data-stu-id="125f8-198">One or two is enough.</span></span>

2. <span data-ttu-id="125f8-199">Az alábbi módszerek valamelyikével felderítése a Kafka Zookeeper gazdagépek:</span><span class="sxs-lookup"><span data-stu-id="125f8-199">Use one of the following methods to discover the Kafka Zookeeper hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
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
    > <span data-ttu-id="125f8-200">A Bash példa feltételezi, hogy `$CLUSTERNAME` a HDInsight-fürt nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="125f8-200">The Bash example assumes that `$CLUSTERNAME` contains the name of the HDInsight cluster.</span></span> <span data-ttu-id="125f8-201">Azt is feltételezi, hogy [jq](https://stedolan.github.io/jq/) telepítve van.</span><span class="sxs-lookup"><span data-stu-id="125f8-201">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="125f8-202">Amikor a rendszer kéri, adja meg a fürt bejelentkezési fiókjának jelszavát.</span><span class="sxs-lookup"><span data-stu-id="125f8-202">When prompted, enter the password for the cluster login account.</span></span>

    <span data-ttu-id="125f8-203">A visszaadott érték az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="125f8-203">The value returned is similar to the following text:</span></span>

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > <span data-ttu-id="125f8-204">Miközben kettőnél több Zookeeper csomópontok, nem kell minden gazdagép ügyfelek teljes listáját tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="125f8-204">While there are more than two Zookeeper nodes, you do not need to provide a full list of all hosts to clients.</span></span> <span data-ttu-id="125f8-205">Egy vagy két is elegendő.</span><span class="sxs-lookup"><span data-stu-id="125f8-205">One or two is enough.</span></span>

    <span data-ttu-id="125f8-206">Mentse ezt az értéket, a rendszer később.</span><span class="sxs-lookup"><span data-stu-id="125f8-206">Save this value, as it is used later.</span></span>

3. <span data-ttu-id="125f8-207">Szerkessze a `dev.properties` fájlt a projekt gyökérkönyvtárában található.</span><span class="sxs-lookup"><span data-stu-id="125f8-207">Edit the `dev.properties` file in the root of the project.</span></span> <span data-ttu-id="125f8-208">A Zookeeper és a gazdagép-adatokat hozzáadni a fájlban található egyező sor.</span><span class="sxs-lookup"><span data-stu-id="125f8-208">Add the Broker and Zookeeper hosts information to the matching lines in this file.</span></span> <span data-ttu-id="125f8-209">A következő példa a minta értékeit az előző lépések segítségével konfigurálható:</span><span class="sxs-lookup"><span data-stu-id="125f8-209">The following example is configured using the sample values from the previous steps:</span></span>

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. <span data-ttu-id="125f8-210">Mentse a `dev.properties` fájlt, és a következő parancs használatával töltse fel azt a Storm-fürt:</span><span class="sxs-lookup"><span data-stu-id="125f8-210">Save the `dev.properties` file and then use the following command to upload it to the Storm cluster:</span></span>

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="125f8-211">Cserélje le **felhasználónév** rendelkező a fürthöz az SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="125f8-211">Replace **USERNAME** with the SSH user name for the cluster.</span></span> <span data-ttu-id="125f8-212">Cserélje le **BASENAME** a fürt létrehozásakor használt alap névvel.</span><span class="sxs-lookup"><span data-stu-id="125f8-212">Replace **BASENAME** with the base name you used when creating the cluster.</span></span>

## <a name="start-the-writer"></a><span data-ttu-id="125f8-213">Az író indítása</span><span class="sxs-lookup"><span data-stu-id="125f8-213">Start the writer</span></span>

1. <span data-ttu-id="125f8-214">A következő segítségével csatlakozzon a Storm fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="125f8-214">Use the following to connect to the Storm cluster using SSH.</span></span> <span data-ttu-id="125f8-215">Cserélje le **felhasználónév** az a fürt létrehozásakor használt SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="125f8-215">Replace **USERNAME** with the SSH user name used when creating the cluster.</span></span> <span data-ttu-id="125f8-216">Cserélje le **BASENAME** a fürt létrehozásakor használt alap névvel.</span><span class="sxs-lookup"><span data-stu-id="125f8-216">Replace **BASENAME** with the base name used when creating the cluster.</span></span>

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    <span data-ttu-id="125f8-217">Amikor a rendszer kéri, írja be a jelszót, a fürt létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="125f8-217">When prompted, enter the password you used when creating the clusters.</span></span>
   
    <span data-ttu-id="125f8-218">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="125f8-218">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="125f8-219">Az SSH-kapcsolat létrehozásához használja a topológia Kafka témakör alkalmazás a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="125f8-219">From the SSH connection, use the following command to create the Kafka topic used by the topology:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    <span data-ttu-id="125f8-220">Cserélje le `$KAFKAZKHOSTS` a Zookeeper a gazdagép az előző szakaszban lekért információt.</span><span class="sxs-lookup"><span data-stu-id="125f8-220">Replace `$KAFKAZKHOSTS` with the Zookeeper host information you retrieved in the previous section.</span></span>

2. <span data-ttu-id="125f8-221">Az a Storm fürthöz SSH-kapcsolat alkalmazás az író topológia indítsa el a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="125f8-221">From the SSH connection to the Storm cluster, use the following command to start the writer topology:</span></span>

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    <span data-ttu-id="125f8-222">Ezzel a paranccsal használt paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="125f8-222">The parameters used with this command are:</span></span>

    * <span data-ttu-id="125f8-223">`org.apache.storm.flux.Flux`: A fluxus használja a konfigurálására és futtatására ebben a topológiában.</span><span class="sxs-lookup"><span data-stu-id="125f8-223">`org.apache.storm.flux.Flux`: Use Flux to configure and run this topology.</span></span>

    * <span data-ttu-id="125f8-224">`--remote`: Küldje el a Nimbus topológiát.</span><span class="sxs-lookup"><span data-stu-id="125f8-224">`--remote`: Submit the topology to Nimbus.</span></span> <span data-ttu-id="125f8-225">A topológia van elosztva a fürt munkavégző csomópontjaihoz.</span><span class="sxs-lookup"><span data-stu-id="125f8-225">The topology is distributed across the worker nodes in the cluster.</span></span>

    * <span data-ttu-id="125f8-226">`-R /writer.yaml`: Használja a `writer.yaml` fájl a topológiát.</span><span class="sxs-lookup"><span data-stu-id="125f8-226">`-R /writer.yaml`: Use the `writer.yaml` file to configure the topology.</span></span> <span data-ttu-id="125f8-227">`-R`azt jelzi, hogy az erőforrás szerepel-e a jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="125f8-227">`-R` indicates that this resource is included in the jar file.</span></span> <span data-ttu-id="125f8-228">A jar gyökerébe ezért van `/writer.yaml` elérési útja azt.</span><span class="sxs-lookup"><span data-stu-id="125f8-228">It's in the root of the jar, so `/writer.yaml` is the path to it.</span></span>

    * <span data-ttu-id="125f8-229">`--filter`: Bejegyzések feltöltése a `writer.yaml` topológiáját szereplő értékek a `dev.properties` fájlt.</span><span class="sxs-lookup"><span data-stu-id="125f8-229">`--filter`: Populate entries in the `writer.yaml` topology using values in the `dev.properties` file.</span></span> <span data-ttu-id="125f8-230">Például értékének a `kafka.topic` bejegyzés a fájlban cserélje le azt a `${kafka.topic}` topológia meghatározása bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="125f8-230">For example, the value of the `kafka.topic` entry in the file is used to replace the `${kafka.topic}` entry in the topology definition.</span></span>

5. <span data-ttu-id="125f8-231">Ha a topológia elindult, a következő paranccsal ellenőrizheti, hogy azt az írás a Kafka témakör:</span><span class="sxs-lookup"><span data-stu-id="125f8-231">Once the topology has started, use the following command to verify that it is writing data to the Kafka topic:</span></span>

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    <span data-ttu-id="125f8-232">Cserélje le `$KAFKAZKHOSTS` a Zookeeper a gazdagép az előző szakaszban lekért információt.</span><span class="sxs-lookup"><span data-stu-id="125f8-232">Replace `$KAFKAZKHOSTS` with the Zookeeper host information you retrieved in the previous section.</span></span>

    <span data-ttu-id="125f8-233">Ez a parancs Kafka rendszerrel szállított parancsfájlt használ a figyelheti a témakör.</span><span class="sxs-lookup"><span data-stu-id="125f8-233">This command uses a script shipped with Kafka to monitor the topic.</span></span> <span data-ttu-id="125f8-234">Néhány percet, miután kell kezdenie véletlen a témakörbe írt mondatokat visszaadó.</span><span class="sxs-lookup"><span data-stu-id="125f8-234">After a moment, it should start returning random sentences that have been written to the topic.</span></span> <span data-ttu-id="125f8-235">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="125f8-235">The output is similar to the following example:</span></span>

        i am at two with nature             
        an apple a day keeps the doctor away
        snow white and the seven dwarfs     
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        four score and seven years ago      
        snow white and the seven dwarfs     
        snow white and the seven dwarfs     
        i am at two with nature             
        an apple a day keeps the doctor away

    <span data-ttu-id="125f8-236">Használja a Ctrl + c leállítja a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="125f8-236">Use Ctrl+c to stop the script.</span></span>

## <a name="start-the-reader"></a><span data-ttu-id="125f8-237">Indítsa el az olvasó</span><span class="sxs-lookup"><span data-stu-id="125f8-237">Start the reader</span></span>

1. <span data-ttu-id="125f8-238">Az SSH-munkamenetet a Storm fürthöz alkalmazás az olvasó topológia indítsa el a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="125f8-238">From the SSH session to the Storm cluster, use the following command to start the reader topology:</span></span>

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. <span data-ttu-id="125f8-239">Miután elindul a topológia, nyissa meg a Storm felhasználói felülete.</span><span class="sxs-lookup"><span data-stu-id="125f8-239">Once the topology starts, open the Storm UI.</span></span> <span data-ttu-id="125f8-240">A webes felhasználói felület https://storm-BASENAME.azurehdinsight.net/stormui helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="125f8-240">This web UI is located at https://storm-BASENAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="125f8-241">Cserélje le __BASENAME__ a fürt létrehozásakor használt alap névvel.</span><span class="sxs-lookup"><span data-stu-id="125f8-241">Replace __BASENAME__ with the base name used when the cluster was created.</span></span> 

    <span data-ttu-id="125f8-242">Amikor a rendszer kéri, a rendszergazdai bejelentkezési név használata (alapértelmezés szerint `admin`) és a fürt létrehozásakor használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="125f8-242">When prompted, use the admin login name (default, `admin`) and password used when the cluster was created.</span></span> <span data-ttu-id="125f8-243">Az alábbi képen hasonló webes lap jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="125f8-243">You see a web page similar to the following image:</span></span>

    ![A Storm felhasználói felülete](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. <span data-ttu-id="125f8-245">A Storm felhasználói felülete, válassza ki a __kafka-olvasó__ hivatkozásra a __topológia összegzése__ kapcsolatos információk megjelenítéséhez a szakasz a __kafka-olvasó__ topológia.</span><span class="sxs-lookup"><span data-stu-id="125f8-245">From the Storm UI, select the __kafka-reader__ link in the __Topology Summary__ section to display information about the __kafka-reader__ topology.</span></span>

    ![Topológia a Storm webes felhasználói felület összefoglaló szakasza](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. <span data-ttu-id="125f8-247">Információt szeretne megjeleníteni a naplózó-bolt összetevő példánya, válassza ki a __naplózó-bolt__ hivatkozásra a __Boltokhoz (mindig)__ szakasz.</span><span class="sxs-lookup"><span data-stu-id="125f8-247">To display information about the instances of the logger-bolt component, select the __logger-bolt__ link in the __Bolts (All time)__ section.</span></span>

    ![A boltokhoz szakaszban naplózó-bolt-hivatkozás](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. <span data-ttu-id="125f8-249">Az a __végrehajtója__ területen válassza ki a tartalmaz egy hivatkozást a __Port__ oszlop naplózási kapcsolatos információk megjelenítéséhez ezt a példányt.</span><span class="sxs-lookup"><span data-stu-id="125f8-249">In the __Executors__ section, select a link in the __Port__ column to display logging information about this instance of the component.</span></span>

    ![Végrehajtója hivatkozás](./media/hdinsight-apache-storm-with-kafka/executors.png)

    <span data-ttu-id="125f8-251">A napló tartalmazza a Kafka témakör adatsorból beolvasott adatok naplózása.</span><span class="sxs-lookup"><span data-stu-id="125f8-251">The log contains a log of the data read from the Kafka topic.</span></span> <span data-ttu-id="125f8-252">A naplóban szereplő információk hasonlít a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="125f8-252">The information in the log is similar to the following text:</span></span>

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] the cow jumped over the moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: the cow jumped over the moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps the doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps the doctor away

## <a name="stop-the-topologies"></a><span data-ttu-id="125f8-253">A topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="125f8-253">Stop the topologies</span></span>

<span data-ttu-id="125f8-254">A Storm fürthöz SSH-munkamenetet a Storm-topológiák leállításához alkalmazás a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="125f8-254">From an SSH session to the Storm cluster, use the following commands to stop the Storm topologies:</span></span>

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-the-cluster"></a><span data-ttu-id="125f8-255">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="125f8-255">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="125f8-256">A jelen dokumentumban leírt lépések az azonos Azure erőforráscsoport mindkét fürtöket létrehozni, mert az erőforráscsoportot az Azure portálon törölheti.</span><span class="sxs-lookup"><span data-stu-id="125f8-256">Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal.</span></span> <span data-ttu-id="125f8-257">Az erőforráscsoport törlése eltávolítja a jelen dokumentum a következő által létrehozott összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="125f8-257">Deleting the resource group removes all resources created by following this document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="125f8-258">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="125f8-258">Next steps</span></span>

<span data-ttu-id="125f8-259">Tekintse meg a HDInsight alatt futó Storm használható további példa topológiák [példa Storm-topológiák és összetevők](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="125f8-259">For more example topologies that can be used with Storm on HDInsight, see [Example Storm topologies and components](hdinsight-storm-example-topology.md).</span></span>

<span data-ttu-id="125f8-260">A telepítését és megfigyelését a HDInsight Linux-alapú topológiák további információkért lásd: [központi telepítése és kezelése a Linux-alapú HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="125f8-260">For information on deploying and monitoring topologies on Linux-based HDInsight, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>