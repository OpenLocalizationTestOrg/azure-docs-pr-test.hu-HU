---
title: "Spark strukturált Stream továbbítása Kafka - Azure HDInsight aaaApache |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Apache Spark (DStream) tooget streamadatok virtuális gépbe vagy onnan Apache Kafka. Ebben a példában a HDInsight Spark a Jupyter notebook használatával adatok folyamatos átviteléhez."
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
ms.openlocfilehash: 0837e8fc5ea314e644daed029d596feeb2b02c68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="e9764-104">Használja a hdinsight Spark strukturált Stream továbbítása Kafka (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="e9764-104">Use Spark Structured Streaming with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="e9764-105">Megtudhatja, hogyan toouse Apache Kafka on Azure HDInsight Spark strukturált Streaming tooread adatokat.</span><span class="sxs-lookup"><span data-stu-id="e9764-105">Learn how toouse Spark Structured Streaming tooread data from Apache Kafka on Azure HDInsight.</span></span>

<span data-ttu-id="e9764-106">Strukturált Spark streaming olyan adatfolyam feldolgozása a Spark SQL épül.</span><span class="sxs-lookup"><span data-stu-id="e9764-106">Spark structured streaming is a stream processing engine built on Spark SQL.</span></span> <span data-ttu-id="e9764-107">Lehetővé teszi, tooexpress adatfolyam számítások hello ugyanaz, mint a batch számítási statikus adatok.</span><span class="sxs-lookup"><span data-stu-id="e9764-107">It allows you tooexpress streaming computations hello same as batch computation on static data.</span></span> <span data-ttu-id="e9764-108">A strukturált Streaming további információkért lásd: hello [strukturált Streaming programozási útmutató [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) az Apache.org webhelyen.</span><span class="sxs-lookup"><span data-stu-id="e9764-108">For more information on Structured Streaming, see hello [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) at Apache.org.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e9764-109">Ebben a példában HDInsight 3.6 Spark 2.1 használni.</span><span class="sxs-lookup"><span data-stu-id="e9764-109">This example used Spark 2.1 on HDInsight 3.6.</span></span> <span data-ttu-id="e9764-110">Strukturált Streaming tekinthető __alpha__ Spark 2.1-es verziójának.</span><span class="sxs-lookup"><span data-stu-id="e9764-110">Structured Streaming is considered __alpha__ on Spark 2.1.</span></span>
>
> <span data-ttu-id="e9764-111">hello jelen dokumentumban leírt lépések, amely tartalmazza a Spark on HDInsight és egy HDInsight-fürt Kafka Azure erőforráscsoport-csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e9764-111">hello steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="e9764-112">Ezeken a fürtökön hello Kafka fürt kommunikálnak mindkét egy Azure virtuális hálózatot, amely lehetővé teszi a Spark-fürt toodirectly hello belül található.</span><span class="sxs-lookup"><span data-stu-id="e9764-112">These clusters are both located within an Azure Virtual Network, which allows hello Spark cluster toodirectly communicate with hello Kafka cluster.</span></span>
>
> <span data-ttu-id="e9764-113">Amikor elkészült, a jelen dokumentumban leírt lépések hello, ne felejtse el toodelete hello fürtök tooavoid felesleges költségek.</span><span class="sxs-lookup"><span data-stu-id="e9764-113">When you are done with hello steps in this document, remember toodelete hello clusters tooavoid excess charges.</span></span>

## <a name="create-hello-clusters"></a><span data-ttu-id="e9764-114">Hello fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9764-114">Create hello clusters</span></span>

<span data-ttu-id="e9764-115">A HDInsight Apache Kafka nem biztosít hozzáférést toohello Kafka brókerek képest hello a nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="e9764-115">Apache Kafka on HDInsight does not provide access toohello Kafka brokers over hello public internet.</span></span> <span data-ttu-id="e9764-116">Kafka fürt hello, amelyeket tooKafka kell lennie a megbeszélések hello hello csomópontként azonos Azure virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="e9764-116">Anything that talks tooKafka must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="e9764-117">Ehhez a példához hello Kafka, mind a Spark-fürtök találhatók egy Azure virtuális hálózatra.</span><span class="sxs-lookup"><span data-stu-id="e9764-117">For this example, both hello Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="e9764-118">a következő ábra azt mutatja be, hogyan kommunikációs hello fürtök között zajló kommunikációról hello:</span><span class="sxs-lookup"><span data-stu-id="e9764-118">hello following diagram shows how communication flows between hello clusters:</span></span>

![Spark és Kafka fürtök egy Azure virtuális hálózatban ábrája](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="e9764-120">hello Kafka szolgáltatás korlátozott toocommunication hello virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="e9764-120">hello Kafka service is limited toocommunication within hello virtual network.</span></span> <span data-ttu-id="e9764-121">Egyéb szolgáltatások hello fürtön, például az SSH és az Ambari, keresztül is elérhető hello internet.</span><span class="sxs-lookup"><span data-stu-id="e9764-121">Other services on hello cluster, such as SSH and Ambari, can be accessed over hello internet.</span></span> <span data-ttu-id="e9764-122">Nyilvános és a HDInsight együttes rendelkezésre álló hello-portok további információkért lásd: [portok és a HDInsight által használt URI-azonosítók](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="e9764-122">For more information on hello public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="e9764-123">Bár manuálisan is létrehozhat egy Azure virtuális hálózatra, Kafka és Spark fürtök, ez a beállítás könnyebb toouse Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="e9764-123">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="e9764-124">Használjon hello alábbi lépéseit toodeploy egy Azure virtuális hálózatra, Kafka, és a Spark-fürtök tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e9764-124">Use hello following steps toodeploy an Azure virtual network, Kafka, and Spark clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="e9764-125">A tooAzure gomb toosign és hello Azure-portál megnyitása hello sablon hello használata.</span><span class="sxs-lookup"><span data-stu-id="e9764-125">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    <span data-ttu-id="e9764-126">hello Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span><span class="sxs-lookup"><span data-stu-id="e9764-126">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span></span>

    <span data-ttu-id="e9764-127">Ez a sablon a következő erőforrások hello hoz létre:</span><span class="sxs-lookup"><span data-stu-id="e9764-127">This template creates hello following resources:</span></span>

    * <span data-ttu-id="e9764-128">Egy Kafka HDInsight 3.5-fürtön.</span><span class="sxs-lookup"><span data-stu-id="e9764-128">A Kafka on HDInsight 3.5 cluster.</span></span>
    * <span data-ttu-id="e9764-129">A Spark on HDInsight 3.6 fürtön.</span><span class="sxs-lookup"><span data-stu-id="e9764-129">A Spark on HDInsight 3.6 cluster.</span></span>
    * <span data-ttu-id="e9764-130">Egy Azure virtuális hálózatot, amely tartalmazza a HDInsight-fürtök hello.</span><span class="sxs-lookup"><span data-stu-id="e9764-130">An Azure Virtual Network, which contains hello HDInsight clusters.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e9764-131">hello strukturált adatfolyam notebook használt ebben a példában a Spark on HDInsight 3.6 igényel.</span><span class="sxs-lookup"><span data-stu-id="e9764-131">hello structured streaming notebook used in this example requires Spark on HDInsight 3.6.</span></span> <span data-ttu-id="e9764-132">Ha a HDInsight Spark egy korábbi verzióját használja, hibák merülnek fel hello notebook használatakor.</span><span class="sxs-lookup"><span data-stu-id="e9764-132">If you use an earlier version of Spark on HDInsight, you receive errors when using hello notebook.</span></span>

2. <span data-ttu-id="e9764-133">Információk toopopulate hello tételek követően hello használata hello **egyéni telepítési** panel:</span><span class="sxs-lookup"><span data-stu-id="e9764-133">Use hello following information toopopulate hello entries on hello **Custom deployment** blade:</span></span>
   
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="e9764-135">**Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="e9764-135">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="e9764-136">Ez a csoport hello HDInsight-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e9764-136">This group contains hello HDInsight cluster.</span></span>

    * <span data-ttu-id="e9764-137">**Hely**: Válasszon egy helyet a földrajzi elhelyezkedés alapján Bezárás tooyou.</span><span class="sxs-lookup"><span data-stu-id="e9764-137">**Location**: Select a location geographically close tooyou.</span></span>

    * <span data-ttu-id="e9764-138">**Fürt neve kiinduló**: Ez az érték neveként hello alapszintű hello Spark és Kafka fürtök használható.</span><span class="sxs-lookup"><span data-stu-id="e9764-138">**Base Cluster Name**: This value is used as hello base name for hello Spark and Kafka clusters.</span></span> <span data-ttu-id="e9764-139">Ha például **hdi** hoz létre a Spark, spark-hdi__ nevű és egy Kafka fürtön nevű **kafka-hdi**.</span><span class="sxs-lookup"><span data-stu-id="e9764-139">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="e9764-140">**A fürt bejelentkezési felhasználónevét**: hello rendszergazda felhasználóneve hello Spark és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="e9764-140">**Cluster Login User Name**: hello admin user name for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="e9764-141">**A fürt bejelentkezési jelszó**: hello rendszergazdai jelszóval hello Spark és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="e9764-141">**Cluster Login Password**: hello admin user password for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="e9764-142">**SSH-felhasználónév**: hello SSH felhasználói toocreate hello Spark és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="e9764-142">**SSH User Name**: hello SSH user toocreate for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="e9764-143">**SSH-jelszónak**: hello SSH felhasználó hello Spark és Kafka fürt hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e9764-143">**SSH Password**: hello password for hello SSH user for hello Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="e9764-144">Olvasási hello **feltételek és kikötések**, majd válassza ki **toohello feltételek és kikötések fenti elfogadom**.</span><span class="sxs-lookup"><span data-stu-id="e9764-144">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="e9764-145">Végül ellenőrizze **PIN-kód toodashboard** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="e9764-145">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="e9764-146">Körülbelül 20 percet toocreate hello fürtök szükséges.</span><span class="sxs-lookup"><span data-stu-id="e9764-146">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="e9764-147">Létrehozása után a hello erőforrásokat, átirányított toohello erőforráscsoport panel áll.</span><span class="sxs-lookup"><span data-stu-id="e9764-147">Once hello resources have been created, you are redirected toohello resource group blade.</span></span>

![Erőforráscsoport panel hello vnet és fürtök](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="e9764-149">Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **spark-BASENAME** és **kafka-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét.</span><span class="sxs-lookup"><span data-stu-id="e9764-149">Notice that hello names of hello HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="e9764-150">Ezeket a neveket használja a későbbi lépésekben toohello fürtök kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="e9764-150">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="get-hello-kafka-brokers"></a><span data-ttu-id="e9764-151">Hello Kafka brókerek beolvasása</span><span class="sxs-lookup"><span data-stu-id="e9764-151">Get hello Kafka brokers</span></span>

<span data-ttu-id="e9764-152">hello ebben a példában kód csatlakozik toohello Kafka replikaszervező hello Kafka fürt gazdagépei.</span><span class="sxs-lookup"><span data-stu-id="e9764-152">hello code in this example connects toohello Kafka broker hosts in hello Kafka cluster.</span></span> <span data-ttu-id="e9764-153">toofind hello Kafka broker gazdagépek, a következő PowerShell- vagy Bash példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="e9764-153">toofind hello Kafka broker hosts, use hello following PowerShell or Bash example:</span></span>

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
$clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
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
> <span data-ttu-id="e9764-154">Ez a példa vár `$PASSWORD` toocontain hello jelszó hello fürt bejelentkezési azonosítóhoz, és `$CLUSTERNAME` toocontain hello hello Kafka fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="e9764-154">This example expects `$PASSWORD` toocontain hello password for hello cluster login, and `$CLUSTERNAME` toocontain hello name of hello Kafka cluster.</span></span>
>
> <span data-ttu-id="e9764-155">Ez a példa hello [jq](https://stedolan.github.io/jq/) segédprogram tooparse adatokat hello JSON-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="e9764-155">This example uses hello [jq](https://stedolan.github.io/jq/) utility tooparse data out of hello JSON document.</span></span>

<span data-ttu-id="e9764-156">a kimeneti hello hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="e9764-156">hello output is similar toohello following text:</span></span>

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

<span data-ttu-id="e9764-157">Ezt az információt akkor menteni, mert a következő szakaszok a jelen dokumentum hello használatban van.</span><span class="sxs-lookup"><span data-stu-id="e9764-157">Save this information, as it is used in hello following sections of this document.</span></span>

## <a name="get-hello-notebooks"></a><span data-ttu-id="e9764-158">Hello notebookok beolvasása</span><span class="sxs-lookup"><span data-stu-id="e9764-158">Get hello notebooks</span></span>

<span data-ttu-id="e9764-159">a jelen dokumentumban ismertetett hello például hello kód megtalálható [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span><span class="sxs-lookup"><span data-stu-id="e9764-159">hello code for hello example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span></span>

## <a name="upload-hello-notebooks"></a><span data-ttu-id="e9764-160">Hello notebookok feltöltése</span><span class="sxs-lookup"><span data-stu-id="e9764-160">Upload hello notebooks</span></span>

<span data-ttu-id="e9764-161">Lépéseket tooupload hello notebookok követően – hello projekt tooyour Spark on HDInsight-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="e9764-161">Use hello following steps tooupload hello notebooks from hello project tooyour Spark on HDInsight cluster:</span></span>

1. <span data-ttu-id="e9764-162">A böngészőben csatlakozzon a toohello Jupyter notebook a Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="e9764-162">In your web browser, connect toohello Jupyter notebook on your Spark cluster.</span></span> <span data-ttu-id="e9764-163">Hello a következő URL-címe, cserélje le `CLUSTERNAME` hello nevű a Kafka fürt:</span><span class="sxs-lookup"><span data-stu-id="e9764-163">In hello following URL, replace `CLUSTERNAME` with hello name of your Kafka cluster:</span></span>

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    <span data-ttu-id="e9764-164">Amikor a rendszer kéri, adja meg a hello fürt felhasználónevet (rendszergazda) és hello fürt létrehozásakor használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="e9764-164">When prompted, enter hello cluster login (admin) and password used when you created hello cluster.</span></span>

2. <span data-ttu-id="e9764-165">Hello felső jobb oldalán álló hello oldal, használja a hello __feltöltése__ gomb tooupload hello __adatfolyam-Twitter-üzeneteket-To_Kafka.ipynb__ toohello fájlfürt.</span><span class="sxs-lookup"><span data-stu-id="e9764-165">From hello upper right side of hello page, use hello __Upload__ button tooupload hello __Stream-Tweets-To_Kafka.ipynb__ file toohello cluster.</span></span> <span data-ttu-id="e9764-166">Válassza ki __nyitott__ toostart hello feltöltése.</span><span class="sxs-lookup"><span data-stu-id="e9764-166">Select __Open__ toostart hello upload.</span></span>

    ![Hello feltöltési gomb tooselect használja, és töltse fel a notebook](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Hello KafkaStreaming.ipynb fájl kiválasztása](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. <span data-ttu-id="e9764-169">Hello található __adatfolyam-Twitter-üzeneteket-To_Kafka.ipynb__ hello lista notebookok, és válassza ki a bejegyzést __feltöltése__ mellette gombra.</span><span class="sxs-lookup"><span data-stu-id="e9764-169">Find hello __Stream-Tweets-To_Kafka.ipynb__ entry in hello list of notebooks, and select __Upload__ button beside it.</span></span>

    ![Használjon hello Feltöltés gombra hello KafkaStreaming.ipynb bejegyzés tooupload mellett azt toohello notebook kiszolgáló](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. <span data-ttu-id="e9764-171">Ismételje meg a 1-3 tooload hello __Spark-strukturált-adatfolyam-a-Kafka.ipynb__ notebookot.</span><span class="sxs-lookup"><span data-stu-id="e9764-171">Repeat steps 1-3 tooload hello __Spark-Structured-Streaming-From-Kafka.ipynb__ notebook.</span></span>

## <a name="load-tweets-into-kafka"></a><span data-ttu-id="e9764-172">Twitter-üzeneteket betölthető Kafka</span><span class="sxs-lookup"><span data-stu-id="e9764-172">Load tweets into Kafka</span></span>

<span data-ttu-id="e9764-173">Hello fájlok feltöltése után válassza a hello __adatfolyam-Twitter-üzeneteket-To_Kafka.ipynb__ bejegyzés tooopen hello notebookot.</span><span class="sxs-lookup"><span data-stu-id="e9764-173">Once hello files have been uploaded, select hello __Stream-Tweets-To_Kafka.ipynb__ entry tooopen hello notebook.</span></span> <span data-ttu-id="e9764-174">Kövesse hello hello notebook tooload Twitter-üzeneteket Kafka be.</span><span class="sxs-lookup"><span data-stu-id="e9764-174">Follow hello steps in hello notebook tooload tweets into Kafka.</span></span>

## <a name="process-tweets-using-spark-structured-streaming"></a><span data-ttu-id="e9764-175">Folyamat Twitter-üzeneteket Spark strukturált adatfolyam használata</span><span class="sxs-lookup"><span data-stu-id="e9764-175">Process tweets using Spark Structured Streaming</span></span>

<span data-ttu-id="e9764-176">Hello Jupyter Notebook kezdőlapját, válassza ki hello __Spark-strukturált-adatfolyam-a-Kafka.ipynb__ bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="e9764-176">From hello Jupyter Notebook home page, select hello __Spark-Structured-Streaming-From-Kafka.ipynb__ entry.</span></span> <span data-ttu-id="e9764-177">Kövesse hello hello notebook tooload Twitter-üzeneteket a Spark strukturált Streaming használatával Kafka.</span><span class="sxs-lookup"><span data-stu-id="e9764-177">Follow hello steps in hello notebook tooload tweets from Kafka using Spark Structured Streaming.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9764-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e9764-178">Next steps</span></span>

<span data-ttu-id="e9764-179">Most, hogy megtanulta, hogyan toouse Spark strukturált Streaming, tekintse meg a következő dokumentumok toolearn további információk a Spark és Kafka hello:</span><span class="sxs-lookup"><span data-stu-id="e9764-179">Now that you have learned how toouse Spark Structured Streaming, see hello following documents toolearn more about working with Spark and Kafka:</span></span>

* <span data-ttu-id="e9764-180">[Hogyan toouse Spark streamelési (DStream) rendelkező Kafka](hdinsight-apache-spark-with-kafka.md).</span><span class="sxs-lookup"><span data-stu-id="e9764-180">[How toouse Spark streaming (DStream) with Kafka](hdinsight-apache-spark-with-kafka.md).</span></span>
* [<span data-ttu-id="e9764-181">Indítsa el a Jupyter Notebook és a Spark on HDInsight</span><span class="sxs-lookup"><span data-stu-id="e9764-181">Start with Jupyter Notebook and Spark on HDInsight</span></span>](hdinsight-apache-spark-jupyter-spark-sql.md)