---
title: "Apache Spark strukturált Streaming Kafka – az Azure HDInsight |} Microsoft Docs"
description: "Útmutató Apache Spark streaming (DStream) adatok eléréséhez, vagy abból Apache Kafka. Ebben a példában a HDInsight Spark a Jupyter notebook használatával adatok folyamatos átviteléhez."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/09/2017
ms.author: larryfr
ms.openlocfilehash: 02b49e13e8f54c3d55310f4d2b21c7e09c91fe81
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="768c5-104">Használja a hdinsight Spark strukturált Stream továbbítása Kafka (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="768c5-104">Use Spark Structured Streaming with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="768c5-105">Ismerje meg, hogyan használható a adatokat olvasni az Apache Kafka on Azure HDInsight Spark strukturált Streaming.</span><span class="sxs-lookup"><span data-stu-id="768c5-105">Learn how to use Spark Structured Streaming to read data from Apache Kafka on Azure HDInsight.</span></span>

<span data-ttu-id="768c5-106">Strukturált Spark streaming olyan adatfolyam feldolgozása a Spark SQL épül.</span><span class="sxs-lookup"><span data-stu-id="768c5-106">Spark structured streaming is a stream processing engine built on Spark SQL.</span></span> <span data-ttu-id="768c5-107">Lehetővé teszi adatfolyam számítások express ugyanaz, mint a batch számítási statikus adatok.</span><span class="sxs-lookup"><span data-stu-id="768c5-107">It allows you to express streaming computations the same as batch computation on static data.</span></span> <span data-ttu-id="768c5-108">A strukturált Streaming további információkért lásd: a [strukturált Streaming programozási útmutató [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) az Apache.org webhelyen.</span><span class="sxs-lookup"><span data-stu-id="768c5-108">For more information on Structured Streaming, see the [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) at Apache.org.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="768c5-109">Ebben a példában HDInsight 3.6 Spark 2.1 használni.</span><span class="sxs-lookup"><span data-stu-id="768c5-109">This example used Spark 2.1 on HDInsight 3.6.</span></span> <span data-ttu-id="768c5-110">Strukturált Streaming tekinthető __alpha__ Spark 2.1-es verziójának.</span><span class="sxs-lookup"><span data-stu-id="768c5-110">Structured Streaming is considered __alpha__ on Spark 2.1.</span></span>
>
> <span data-ttu-id="768c5-111">A jelen dokumentumban leírt lépések, amely tartalmazza a Spark on HDInsight és egy HDInsight-fürt Kafka Azure erőforráscsoport-csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="768c5-111">The steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="768c5-112">Ezeken a fürtökön is egy Azure virtuális hálózatot, amely lehetővé teszi a Kafka közvetlenül kommunikálni a Spark-fürtön belül található a fürtön.</span><span class="sxs-lookup"><span data-stu-id="768c5-112">These clusters are both located within an Azure Virtual Network, which allows the Spark cluster to directly communicate with the Kafka cluster.</span></span>
>
> <span data-ttu-id="768c5-113">Amikor elkészült, a jelen dokumentumban leírt lépések, ne felejtse el a felesleges költségek elkerülése érdekében a fürtök törlése.</span><span class="sxs-lookup"><span data-stu-id="768c5-113">When you are done with the steps in this document, remember to delete the clusters to avoid excess charges.</span></span>

## <a name="create-the-clusters"></a><span data-ttu-id="768c5-114">A fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="768c5-114">Create the clusters</span></span>

<span data-ttu-id="768c5-115">A HDInsight Apache Kafka nem férhet hozzá a Kafka brókerek a nyilvános interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="768c5-115">Apache Kafka on HDInsight does not provide access to the Kafka brokers over the public internet.</span></span> <span data-ttu-id="768c5-116">A Kafka fürt csomópontja azonos Azure virtuális hálózaton, amelyeket a kiszolgálóhoz csatlakozik Kafka kell lennie.</span><span class="sxs-lookup"><span data-stu-id="768c5-116">Anything that talks to Kafka must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="768c5-117">Ehhez a példához a Kafka és a Spark-fürtök egy Azure virtuális hálózat található.</span><span class="sxs-lookup"><span data-stu-id="768c5-117">For this example, both the Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="768c5-118">Az alábbi ábra bemutatja, hogyan kommunikációs a fürtök között zajló kommunikációról:</span><span class="sxs-lookup"><span data-stu-id="768c5-118">The following diagram shows how communication flows between the clusters:</span></span>

![Spark és Kafka fürtök egy Azure virtuális hálózatban ábrája](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="768c5-120">A virtuális hálózaton belüli kommunikáció a Kafka szolgáltatás korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="768c5-120">The Kafka service is limited to communication within the virtual network.</span></span> <span data-ttu-id="768c5-121">A fürtben, mint az SSH és az Ambari, más szolgáltatások az interneten keresztül is elérhető.</span><span class="sxs-lookup"><span data-stu-id="768c5-121">Other services on the cluster, such as SSH and Ambari, can be accessed over the internet.</span></span> <span data-ttu-id="768c5-122">A hdinsight eszközzel elérhető nyilvános portokon további információkért lásd: [portok és a HDInsight által használt URI-azonosítók](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="768c5-122">For more information on the public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="768c5-123">Létrehozhat egy Azure virtuális hálózatra, Kafka, és a Spark-fürtök manuális, célszerűbb Azure Resource Manager sablonnal.</span><span class="sxs-lookup"><span data-stu-id="768c5-123">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="768c5-124">Az alábbi lépések segítségével telepíthet egy Azure virtuális hálózatot, Kafka, és a Spark-fürtök az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="768c5-124">Use the following steps to deploy an Azure virtual network, Kafka, and Spark clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="768c5-125">A következő gomb segítségével jelentkezzen be az Azure-ba, és nyissa meg a sablon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="768c5-125">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    <span data-ttu-id="768c5-126">Az Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span><span class="sxs-lookup"><span data-stu-id="768c5-126">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span></span>

    <span data-ttu-id="768c5-127">Ez a sablon hoz létre a következő erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="768c5-127">This template creates the following resources:</span></span>

    * <span data-ttu-id="768c5-128">Egy Kafka HDInsight 3.5-fürtön.</span><span class="sxs-lookup"><span data-stu-id="768c5-128">A Kafka on HDInsight 3.5 cluster.</span></span>
    * <span data-ttu-id="768c5-129">A Spark on HDInsight 3.6 fürtön.</span><span class="sxs-lookup"><span data-stu-id="768c5-129">A Spark on HDInsight 3.6 cluster.</span></span>
    * <span data-ttu-id="768c5-130">Egy Azure virtuális hálózatot, amely a HDInsight-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="768c5-130">An Azure Virtual Network, which contains the HDInsight clusters.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="768c5-131">A strukturált, ebben a példában használt adatfolyam-továbbítási notebook a Spark on HDInsight 3.6 igényel.</span><span class="sxs-lookup"><span data-stu-id="768c5-131">The structured streaming notebook used in this example requires Spark on HDInsight 3.6.</span></span> <span data-ttu-id="768c5-132">Ha a HDInsight Spark egy korábbi verzióját használja, hibák merülnek fel a notebook használatakor.</span><span class="sxs-lookup"><span data-stu-id="768c5-132">If you use an earlier version of Spark on HDInsight, you receive errors when using the notebook.</span></span>

2. <span data-ttu-id="768c5-133">A következő információk segítségével a feltöltik a **egyéni telepítési** panel:</span><span class="sxs-lookup"><span data-stu-id="768c5-133">Use the following information to populate the entries on the **Custom deployment** blade:</span></span>
   
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="768c5-135">**Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="768c5-135">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="768c5-136">Ez a csoport tartalmazza a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="768c5-136">This group contains the HDInsight cluster.</span></span>

    * <span data-ttu-id="768c5-137">**Hely**: Adjon meg egy földrajzilag Önhöz legközelebb eső helyet.</span><span class="sxs-lookup"><span data-stu-id="768c5-137">**Location**: Select a location geographically close to you.</span></span>

    * <span data-ttu-id="768c5-138">**Fürt neve kiinduló**: Ez az érték használható a Spark alap néven és Kafka fürtök.</span><span class="sxs-lookup"><span data-stu-id="768c5-138">**Base Cluster Name**: This value is used as the base name for the Spark and Kafka clusters.</span></span> <span data-ttu-id="768c5-139">Ha például **hdi** hoz létre a Spark, spark-hdi__ nevű és egy Kafka fürtön nevű **kafka-hdi**.</span><span class="sxs-lookup"><span data-stu-id="768c5-139">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="768c5-140">**A fürt bejelentkezési felhasználónevét**: A rendszergazda felhasználóneve a Spark és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="768c5-140">**Cluster Login User Name**: The admin user name for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="768c5-141">**A fürt bejelentkezési jelszó**: a Spark és Kafka fürt rendszergazdai jelszóval.</span><span class="sxs-lookup"><span data-stu-id="768c5-141">**Cluster Login Password**: The admin user password for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="768c5-142">**SSH-felhasználónév**: az SSH-felhasználó a Spark- és Kafka fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="768c5-142">**SSH User Name**: The SSH user to create for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="768c5-143">**SSH-jelszónak**: a Spark és Kafka fürt SSH-felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="768c5-143">**SSH Password**: The password for the SSH user for the Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="768c5-144">Olvassa el a **feltételek és kikötések**, majd válassza ki **elfogadom a feltételeket és a fenti feltételek**.</span><span class="sxs-lookup"><span data-stu-id="768c5-144">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="768c5-145">Végül ellenőrizze **rögzítés az irányítópulton** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="768c5-145">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="768c5-146">A fürt létrehozása nagyjából 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="768c5-146">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="768c5-147">Az erőforrások létrehozása után, ekkor megnyílik az erőforráscsoport panel.</span><span class="sxs-lookup"><span data-stu-id="768c5-147">Once the resources have been created, you are redirected to the resource group blade.</span></span>

![A virtuális hálózat és a fürtök erőforráscsoport panel](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="768c5-149">Figyelje meg, hogy a HDInsight-fürtök neve **spark-BASENAME** és **kafka-BASENAME**, ahol BASENAME a sablonhoz megadott név.</span><span class="sxs-lookup"><span data-stu-id="768c5-149">Notice that the names of the HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="768c5-150">Ezeket a neveket a későbbi lépésekben használja, a fürtök történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="768c5-150">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="get-the-kafka-brokers"></a><span data-ttu-id="768c5-151">A Kafka brókerek beolvasása</span><span class="sxs-lookup"><span data-stu-id="768c5-151">Get the Kafka brokers</span></span>

<span data-ttu-id="768c5-152">Ebben a példában a kódot a Kafka broker a fürt állomásai között Kafka csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="768c5-152">The code in this example connects to the Kafka broker hosts in the Kafka cluster.</span></span> <span data-ttu-id="768c5-153">A Kafka broker állomások megkereséséhez használja a következő PowerShell- vagy Bash példa:</span><span class="sxs-lookup"><span data-stu-id="768c5-153">To find the Kafka broker hosts, use the following PowerShell or Bash example:</span></span>

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name
($brokerHosts -join ":9092,") + ":9092"
```

```bash
curl -u admin:$PASSWORD -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")'
```

> [!NOTE]
> <span data-ttu-id="768c5-154">Ez a példa vár `$PASSWORD` magában foglalja a fürt bejelentkezési azonosító jelszavának és `$CLUSTERNAME` magában foglalja a Kafka fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="768c5-154">This example expects `$PASSWORD` to contain the password for the cluster login, and `$CLUSTERNAME` to contain the name of the Kafka cluster.</span></span>
>
> <span data-ttu-id="768c5-155">Ez a példa a [jq](https://stedolan.github.io/jq/) segédprogram a JSON-dokumentum kívül az adatok.</span><span class="sxs-lookup"><span data-stu-id="768c5-155">This example uses the [jq](https://stedolan.github.io/jq/) utility to parse data out of the JSON document.</span></span>

<span data-ttu-id="768c5-156">A kimenet az alábbi szöveghez hasonló:</span><span class="sxs-lookup"><span data-stu-id="768c5-156">The output is similar to the following text:</span></span>

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

<span data-ttu-id="768c5-157">Ezt az információt akkor menteni, mert használatban van ebben a dokumentumban a következő szakaszok.</span><span class="sxs-lookup"><span data-stu-id="768c5-157">Save this information, as it is used in the following sections of this document.</span></span>

## <a name="get-the-notebooks"></a><span data-ttu-id="768c5-158">A notebookok beolvasása</span><span class="sxs-lookup"><span data-stu-id="768c5-158">Get the notebooks</span></span>

<span data-ttu-id="768c5-159">A jelen dokumentumban ismertetett példa kódja megtalálható [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span><span class="sxs-lookup"><span data-stu-id="768c5-159">The code for the example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span></span>

## <a name="upload-the-notebooks"></a><span data-ttu-id="768c5-160">Töltse fel a notebookok</span><span class="sxs-lookup"><span data-stu-id="768c5-160">Upload the notebooks</span></span>

<span data-ttu-id="768c5-161">Az alábbi lépések segítségével töltse fel a notebookok a projektből a Spark on HDInsight-fürt számára:</span><span class="sxs-lookup"><span data-stu-id="768c5-161">Use the following steps to upload the notebooks from the project to your Spark on HDInsight cluster:</span></span>

1. <span data-ttu-id="768c5-162">A böngészőben csatlakoztassa a Jupyter notebook a Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="768c5-162">In your web browser, connect to the Jupyter notebook on your Spark cluster.</span></span> <span data-ttu-id="768c5-163">Cserélje le a következő URL-címet, `CLUSTERNAME` a Kafka fürt nevű:</span><span class="sxs-lookup"><span data-stu-id="768c5-163">In the following URL, replace `CLUSTERNAME` with the name of your Kafka cluster:</span></span>

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    <span data-ttu-id="768c5-164">Amikor a rendszer kéri, adja meg a fürt bejelentkezési (rendszergazda) és a fürt létrehozásakor használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="768c5-164">When prompted, enter the cluster login (admin) and password used when you created the cluster.</span></span>

2. <span data-ttu-id="768c5-165">A lap felső jobb oldalán, használja a __feltöltése__ gombra kattintva töltse fel a __adatfolyam-Twitter-üzeneteket-To_Kafka.ipynb__ fájl a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="768c5-165">From the upper right side of the page, use the __Upload__ button to upload the __Stream-Tweets-To_Kafka.ipynb__ file to the cluster.</span></span> <span data-ttu-id="768c5-166">Válassza ki __nyitott__ való feltöltés indításához.</span><span class="sxs-lookup"><span data-stu-id="768c5-166">Select __Open__ to start the upload.</span></span>

    ![A feltöltési gomb használatával válassza ki, majd töltse fel a notebook](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Válassza ki a KafkaStreaming.ipynb fájlt](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. <span data-ttu-id="768c5-169">Keresse a __adatfolyam-Twitter-üzeneteket-To_Kafka.ipynb__ bejegyzés notebookok, és válassza ki a listában __feltöltése__ mellette gombra.</span><span class="sxs-lookup"><span data-stu-id="768c5-169">Find the __Stream-Tweets-To_Kafka.ipynb__ entry in the list of notebooks, and select __Upload__ button beside it.</span></span>

    ![A feltöltési gomb melletti a KafkaStreaming.ipynb bejegyzés segítségével töltse fel a notebook kiszolgáló](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. <span data-ttu-id="768c5-171">Ismételje meg a 1-3 betöltése a __Spark-strukturált-adatfolyam-a-Kafka.ipynb__ notebookot.</span><span class="sxs-lookup"><span data-stu-id="768c5-171">Repeat steps 1-3 to load the __Spark-Structured-Streaming-From-Kafka.ipynb__ notebook.</span></span>

## <a name="load-tweets-into-kafka"></a><span data-ttu-id="768c5-172">Twitter-üzeneteket betölthető Kafka</span><span class="sxs-lookup"><span data-stu-id="768c5-172">Load tweets into Kafka</span></span>

<span data-ttu-id="768c5-173">A fájlok feltöltése után válassza a __adatfolyam-Twitter-üzeneteket-To_Kafka.ipynb__ nyissa meg a notebook bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="768c5-173">Once the files have been uploaded, select the __Stream-Tweets-To_Kafka.ipynb__ entry to open the notebook.</span></span> <span data-ttu-id="768c5-174">Kövesse a notebook Twitter-üzeneteket betölthető Kafka.</span><span class="sxs-lookup"><span data-stu-id="768c5-174">Follow the steps in the notebook to load tweets into Kafka.</span></span>

## <a name="process-tweets-using-spark-structured-streaming"></a><span data-ttu-id="768c5-175">Folyamat Twitter-üzeneteket Spark strukturált adatfolyam használata</span><span class="sxs-lookup"><span data-stu-id="768c5-175">Process tweets using Spark Structured Streaming</span></span>

<span data-ttu-id="768c5-176">Jupyter Notebook kezdőlapján válassza ki a __Spark-strukturált-adatfolyam-a-Kafka.ipynb__ bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="768c5-176">From the Jupyter Notebook home page, select the __Spark-Structured-Streaming-From-Kafka.ipynb__ entry.</span></span> <span data-ttu-id="768c5-177">Kövesse a notebook Twitter-üzeneteket betölteni a Kafka Spark strukturált Streaming használatával.</span><span class="sxs-lookup"><span data-stu-id="768c5-177">Follow the steps in the notebook to load tweets from Kafka using Spark Structured Streaming.</span></span>

## <a name="next-steps"></a><span data-ttu-id="768c5-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="768c5-178">Next steps</span></span>

<span data-ttu-id="768c5-179">Most, hogy megismerte a használata Spark strukturált Streaming rendelkezik, tekintse meg a következő dokumentumok tudhat meg többet a Spark és Kafka használata:</span><span class="sxs-lookup"><span data-stu-id="768c5-179">Now that you have learned how to use Spark Structured Streaming, see the following documents to learn more about working with Spark and Kafka:</span></span>

* <span data-ttu-id="768c5-180">[Spark streaming (DStream) rendelkező Kafka használata](hdinsight-apache-spark-with-kafka.md).</span><span class="sxs-lookup"><span data-stu-id="768c5-180">[How to use Spark streaming (DStream) with Kafka](hdinsight-apache-spark-with-kafka.md).</span></span>
* [<span data-ttu-id="768c5-181">Indítsa el a Jupyter Notebook és a Spark on HDInsight</span><span class="sxs-lookup"><span data-stu-id="768c5-181">Start with Jupyter Notebook and Spark on HDInsight</span></span>](hdinsight-apache-spark-jupyter-spark-sql.md)