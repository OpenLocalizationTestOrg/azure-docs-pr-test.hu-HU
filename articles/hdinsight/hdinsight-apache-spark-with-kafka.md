---
title: Adatfolyam-Kafka - Azure HDInsight az Apache Spark on |} Microsoft Docs
description: "Megtudhatja, hogyan használja Spark az Apache Spark on az adatfolyam adatok virtuális gépbe vagy onnan Apache Kafka DStreams használatával. Ebben a példában a HDInsight Spark a Jupyter notebook használatával adatok folyamatos átviteléhez."
keywords: "kafka például kafka zookeeper, spark streamelési kafka, spark streamelési kafka – példa"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd8f53c1-bdee-4921-b683-3be4c46c2039
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.openlocfilehash: 81fa319f6fb94bdabacd8f68d14b9a1063a9749a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="74b86-105">Apache Spark streaming (DStream) például Kafka (előzetes verzió) a HDInsight a</span><span class="sxs-lookup"><span data-stu-id="74b86-105">Apache Spark streaming (DStream) example with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="74b86-106">Útmutató Spark Apache Spark adatfolyam adatok vagy a HDInsight a DStreams Apache Kafka abból.</span><span class="sxs-lookup"><span data-stu-id="74b86-106">Learn how to use Spark Apache Spark to stream data into or out of Apache Kafka on HDInsight using DStreams.</span></span> <span data-ttu-id="74b86-107">A példa a Spark-fürtön futó Jupyter notebook.</span><span class="sxs-lookup"><span data-stu-id="74b86-107">This example uses a Jupyter notebook that runs on the Spark cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="74b86-108">A jelen dokumentumban leírt lépések, amely tartalmazza a Spark on HDInsight és egy HDInsight-fürt Kafka Azure erőforráscsoport-csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="74b86-108">The steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="74b86-109">Ezeken a fürtökön is egy Azure virtuális hálózatot, amely lehetővé teszi a Kafka közvetlenül kommunikálni a Spark-fürtön belül található a fürtön.</span><span class="sxs-lookup"><span data-stu-id="74b86-109">These clusters are both located within an Azure Virtual Network, which allows the Spark cluster to directly communicate with the Kafka cluster.</span></span>
>
> <span data-ttu-id="74b86-110">Amikor elkészült, a jelen dokumentumban leírt lépések, ne felejtse el a felesleges költségek elkerülése érdekében a fürtök törlése.</span><span class="sxs-lookup"><span data-stu-id="74b86-110">When you are done with the steps in this document, remember to delete the clusters to avoid excess charges.</span></span>

## <a name="create-the-clusters"></a><span data-ttu-id="74b86-111">A fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="74b86-111">Create the clusters</span></span>

<span data-ttu-id="74b86-112">A HDInsight Apache Kafka nem férhet hozzá a Kafka brókerek a nyilvános interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="74b86-112">Apache Kafka on HDInsight does not provide access to the Kafka brokers over the public internet.</span></span> <span data-ttu-id="74b86-113">A Kafka fürt csomópontja azonos Azure virtuális hálózaton, amelyeket a kiszolgálóhoz csatlakozik Kafka kell lennie.</span><span class="sxs-lookup"><span data-stu-id="74b86-113">Anything that talks to Kafka must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="74b86-114">Ehhez a példához a Kafka és a Spark-fürtök egy Azure virtuális hálózat található.</span><span class="sxs-lookup"><span data-stu-id="74b86-114">For this example, both the Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="74b86-115">Az alábbi ábra bemutatja, hogyan kommunikációs a fürtök között zajló kommunikációról:</span><span class="sxs-lookup"><span data-stu-id="74b86-115">The following diagram shows how communication flows between the clusters:</span></span>

![Spark és Kafka fürtök egy Azure virtuális hálózatban ábrája](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="74b86-117">Bár Kafka, maga a virtuális hálózaton belüli kommunikáció korlátozódik, például az SSH és az Ambari a fürt más szolgáltatások az interneten keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="74b86-117">Though Kafka itself is limited to communication within the virtual network, other services on the cluster such as SSH and Ambari can be accessed over the internet.</span></span> <span data-ttu-id="74b86-118">A hdinsight eszközzel elérhető nyilvános portokon további információkért lásd: [portok és a HDInsight által használt URI-azonosítók](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="74b86-118">For more information on the public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="74b86-119">Létrehozhat egy Azure virtuális hálózatra, Kafka, és a Spark-fürtök manuális, célszerűbb Azure Resource Manager sablonnal.</span><span class="sxs-lookup"><span data-stu-id="74b86-119">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="74b86-120">Az alábbi lépések segítségével telepíthet egy Azure virtuális hálózatot, Kafka, és a Spark-fürtök az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="74b86-120">Use the following steps to deploy an Azure virtual network, Kafka, and Spark clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="74b86-121">A következő gomb segítségével jelentkezzen be az Azure-ba, és nyissa meg a sablon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="74b86-121">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    <span data-ttu-id="74b86-122">Az Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="74b86-122">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="74b86-123">A HDInsightban futó Kafka platform rendelkezésre állásának biztosításához fürtjének legalább három feldolgozó csomópontot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="74b86-123">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="74b86-124">Ez a sablon a három munkavégző csomópontokhoz tartalmaz Kafka fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="74b86-124">This template creates a Kafka cluster that contains three worker nodes.</span></span>

    <span data-ttu-id="74b86-125">Ez a sablon egy HDInsight 3.6 fürtöt Kafka és Spark hoz létre.</span><span class="sxs-lookup"><span data-stu-id="74b86-125">This template creates an HDInsight 3.6 cluster for both Kafka and Spark.</span></span>

2. <span data-ttu-id="74b86-126">A következő információk segítségével a feltöltik a **egyéni telepítési** panel:</span><span class="sxs-lookup"><span data-stu-id="74b86-126">Use the following information to populate the entries on the **Custom deployment** blade:</span></span>
   
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="74b86-128">**Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="74b86-128">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="74b86-129">Ez a csoport tartalmazza a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="74b86-129">This group contains the HDInsight cluster.</span></span>

    * <span data-ttu-id="74b86-130">**Hely**: Adjon meg egy földrajzilag Önhöz legközelebb eső helyet.</span><span class="sxs-lookup"><span data-stu-id="74b86-130">**Location**: Select a location geographically close to you.</span></span>

    * <span data-ttu-id="74b86-131">**Fürt neve kiinduló**: Ez az érték használható a Spark alap néven és Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="74b86-131">**Base Cluster Name**: This value is used as the base name for the Spark and Kafka clusters.</span></span> <span data-ttu-id="74b86-132">Ha például **hdi** hoz létre a Spark, spark-hdi__ nevű és egy Kafka fürtön nevű **kafka-hdi**.</span><span class="sxs-lookup"><span data-stu-id="74b86-132">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="74b86-133">**A fürt bejelentkezési felhasználónevét**: A rendszergazda felhasználóneve a Spark és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="74b86-133">**Cluster Login User Name**: The admin user name for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="74b86-134">**A fürt bejelentkezési jelszó**: a Spark és Kafka fürt rendszergazdai jelszóval.</span><span class="sxs-lookup"><span data-stu-id="74b86-134">**Cluster Login Password**: The admin user password for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="74b86-135">**SSH-felhasználónév**: az SSH-felhasználó a Spark- és Kafka fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="74b86-135">**SSH User Name**: The SSH user to create for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="74b86-136">**SSH-jelszónak**: a Spark és Kafka fürt SSH-felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="74b86-136">**SSH Password**: The password for the SSH user for the Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="74b86-137">Olvassa el a **feltételek és kikötések**, majd válassza ki **elfogadom a feltételeket és a fenti feltételek**.</span><span class="sxs-lookup"><span data-stu-id="74b86-137">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="74b86-138">Végül ellenőrizze **rögzítés az irányítópulton** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="74b86-138">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="74b86-139">A fürt létrehozása nagyjából 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="74b86-139">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="74b86-140">Az erőforrások létrehozása után, ekkor megnyílik egy panel, ahhoz az erőforráscsoporthoz, amely tartalmazza a fürtök és a webes irányítópult.</span><span class="sxs-lookup"><span data-stu-id="74b86-140">Once the resources have been created, you are redirected to a blade for the resource group that contains the clusters and web dashboard.</span></span>

![A virtuális hálózat és a fürtök erőforráscsoport panel](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="74b86-142">Figyelje meg, hogy a HDInsight-fürtök neve **spark-BASENAME** és **kafka-BASENAME**, ahol BASENAME a sablonhoz megadott név.</span><span class="sxs-lookup"><span data-stu-id="74b86-142">Notice that the names of the HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="74b86-143">Ezeket a neveket a későbbi lépésekben használja, a fürtök történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="74b86-143">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="use-the-notebooks"></a><span data-ttu-id="74b86-144">A notebookok használata</span><span class="sxs-lookup"><span data-stu-id="74b86-144">Use the notebooks</span></span>

<span data-ttu-id="74b86-145">A jelen dokumentumban ismertetett példa kódja megtalálható [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span><span class="sxs-lookup"><span data-stu-id="74b86-145">The code for the example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span></span>

<span data-ttu-id="74b86-146">Kövesse a `README.md` fájl ebben a példában befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="74b86-146">Follow the steps in the `README.md` file to complete this example.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="74b86-147">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="74b86-147">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="74b86-148">A jelen dokumentumban leírt lépések az azonos Azure erőforráscsoport mindkét fürtöket létrehozni, mert az erőforráscsoportot az Azure portálon törölheti.</span><span class="sxs-lookup"><span data-stu-id="74b86-148">Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal.</span></span> <span data-ttu-id="74b86-149">A csoport törlése eltávolítja a következő Ez a dokumentum, az Azure-beli virtuális hálózatra és a fürt által használt tárfiók által létrehozott összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="74b86-149">Deleting the group removes all resources created by following this document, the Azure Virtual Network, and storage account used by the clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74b86-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="74b86-150">Next steps</span></span>

<span data-ttu-id="74b86-151">Ebben a példában megtanulta, hogyan használható a Spark olvasási és írási Kafka.</span><span class="sxs-lookup"><span data-stu-id="74b86-151">In this example, you learned how to use Spark to read and write to Kafka.</span></span> <span data-ttu-id="74b86-152">Az alábbi hivatkozások segítségével felderíteni a más módon történő együttműködésre Kafka:</span><span class="sxs-lookup"><span data-stu-id="74b86-152">Use the following links to discover other ways to work with Kafka:</span></span>

* [<span data-ttu-id="74b86-153">Ismerkedés az Apache Kafka a HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="74b86-153">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="74b86-154">A MirrorMaker használata a Kafka replikájának HDInsighton való létrehozásához</span><span class="sxs-lookup"><span data-stu-id="74b86-154">Use MirrorMaker to create a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="74b86-155">Az Apache Storm használata a HDInsighton futó Kafkával</span><span class="sxs-lookup"><span data-stu-id="74b86-155">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

