---
title: "aaaApache Spark Kafka - Azure HDInsight Stream továbbítása |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Spark Apache Spark toostream adatok virtuális gépbe vagy onnan Apache Kafka DStreams használatával. Ebben a példában a HDInsight Spark a Jupyter notebook használatával adatok folyamatos átviteléhez."
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
ms.openlocfilehash: f48b37aadafa4979cd27af68e8417db6acc8a0e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="6474c-105">Apache Spark streaming (DStream) például Kafka (előzetes verzió) a HDInsight a</span><span class="sxs-lookup"><span data-stu-id="6474c-105">Apache Spark streaming (DStream) example with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="6474c-106">Megtudhatja, hogyan toouse Spark Apache Spark toostream adatok vagy a HDInsight a DStreams Apache Kafka abból.</span><span class="sxs-lookup"><span data-stu-id="6474c-106">Learn how toouse Spark Apache Spark toostream data into or out of Apache Kafka on HDInsight using DStreams.</span></span> <span data-ttu-id="6474c-107">A példa hello Spark-fürtön futó Jupyter notebook.</span><span class="sxs-lookup"><span data-stu-id="6474c-107">This example uses a Jupyter notebook that runs on hello Spark cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="6474c-108">hello jelen dokumentumban leírt lépések, amely tartalmazza a Spark on HDInsight és egy HDInsight-fürt Kafka Azure erőforráscsoport-csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6474c-108">hello steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="6474c-109">Ezeken a fürtökön hello Kafka fürt kommunikálnak mindkét egy Azure virtuális hálózatot, amely lehetővé teszi a Spark-fürt toodirectly hello belül található.</span><span class="sxs-lookup"><span data-stu-id="6474c-109">These clusters are both located within an Azure Virtual Network, which allows hello Spark cluster toodirectly communicate with hello Kafka cluster.</span></span>
>
> <span data-ttu-id="6474c-110">Amikor elkészült, a jelen dokumentumban leírt lépések hello, ne felejtse el toodelete hello fürtök tooavoid felesleges költségek.</span><span class="sxs-lookup"><span data-stu-id="6474c-110">When you are done with hello steps in this document, remember toodelete hello clusters tooavoid excess charges.</span></span>

## <a name="create-hello-clusters"></a><span data-ttu-id="6474c-111">Hello fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="6474c-111">Create hello clusters</span></span>

<span data-ttu-id="6474c-112">A HDInsight Apache Kafka nem biztosít hozzáférést toohello Kafka brókerek képest hello a nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="6474c-112">Apache Kafka on HDInsight does not provide access toohello Kafka brokers over hello public internet.</span></span> <span data-ttu-id="6474c-113">Kafka fürt hello, amelyeket tooKafka kell lennie a megbeszélések hello hello csomópontként azonos Azure virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="6474c-113">Anything that talks tooKafka must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="6474c-114">Ehhez a példához hello Kafka, mind a Spark-fürtök találhatók egy Azure virtuális hálózatra.</span><span class="sxs-lookup"><span data-stu-id="6474c-114">For this example, both hello Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="6474c-115">a következő ábra azt mutatja be, hogyan kommunikációs hello fürtök között zajló kommunikációról hello:</span><span class="sxs-lookup"><span data-stu-id="6474c-115">hello following diagram shows how communication flows between hello clusters:</span></span>

![Spark és Kafka fürtök egy Azure virtuális hálózatban ábrája](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="6474c-117">Bár az maga Kafka korlátozott toocommunication hello virtuális hálózaton belül, más szolgáltatásokkal hello fürtön például az SSH és az Ambari keresztül is elérhető hello internet.</span><span class="sxs-lookup"><span data-stu-id="6474c-117">Though Kafka itself is limited toocommunication within hello virtual network, other services on hello cluster such as SSH and Ambari can be accessed over hello internet.</span></span> <span data-ttu-id="6474c-118">Nyilvános és a HDInsight együttes rendelkezésre álló hello-portok további információkért lásd: [portok és a HDInsight által használt URI-azonosítók](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="6474c-118">For more information on hello public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="6474c-119">Bár manuálisan is létrehozhat egy Azure virtuális hálózatra, Kafka és Spark fürtök, ez a beállítás könnyebb toouse Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="6474c-119">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="6474c-120">Használjon hello alábbi lépéseit toodeploy egy Azure virtuális hálózatra, Kafka, és a Spark-fürtök tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="6474c-120">Use hello following steps toodeploy an Azure virtual network, Kafka, and Spark clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="6474c-121">A tooAzure gomb toosign és hello Azure-portál megnyitása hello sablon hello használata.</span><span class="sxs-lookup"><span data-stu-id="6474c-121">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    <span data-ttu-id="6474c-122">hello Azure Resource Manager sablon itt található: **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="6474c-122">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="6474c-123">a HDInsight Kafka tooguarantee rendelkezésre állását, a fürt tartalmaznia kell legalább három munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="6474c-123">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="6474c-124">Ez a sablon a három munkavégző csomópontokhoz tartalmaz Kafka fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6474c-124">This template creates a Kafka cluster that contains three worker nodes.</span></span>

    <span data-ttu-id="6474c-125">Ez a sablon egy HDInsight 3.6 fürtöt Kafka és Spark hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6474c-125">This template creates an HDInsight 3.6 cluster for both Kafka and Spark.</span></span>

2. <span data-ttu-id="6474c-126">Információk toopopulate hello tételek követően hello használata hello **egyéni telepítési** panel:</span><span class="sxs-lookup"><span data-stu-id="6474c-126">Use hello following information toopopulate hello entries on hello **Custom deployment** blade:</span></span>
   
    ![HDInsight egyéni központi telepítés](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="6474c-128">**Erőforráscsoport**: hozzon létre egy csoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="6474c-128">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="6474c-129">Ez a csoport hello HDInsight-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6474c-129">This group contains hello HDInsight cluster.</span></span>

    * <span data-ttu-id="6474c-130">**Hely**: Válasszon egy helyet a földrajzi elhelyezkedés alapján Bezárás tooyou.</span><span class="sxs-lookup"><span data-stu-id="6474c-130">**Location**: Select a location geographically close tooyou.</span></span>

    * <span data-ttu-id="6474c-131">**Fürt neve kiinduló**: Ez az érték neveként hello alapszintű hello Spark és Kafka fürtök használható.</span><span class="sxs-lookup"><span data-stu-id="6474c-131">**Base Cluster Name**: This value is used as hello base name for hello Spark and Kafka clusters.</span></span> <span data-ttu-id="6474c-132">Ha például **hdi** hoz létre a Spark, spark-hdi__ nevű és egy Kafka fürtön nevű **kafka-hdi**.</span><span class="sxs-lookup"><span data-stu-id="6474c-132">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="6474c-133">**A fürt bejelentkezési felhasználónevét**: hello rendszergazda felhasználóneve hello Spark és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="6474c-133">**Cluster Login User Name**: hello admin user name for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="6474c-134">**A fürt bejelentkezési jelszó**: hello rendszergazdai jelszóval hello Spark és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="6474c-134">**Cluster Login Password**: hello admin user password for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="6474c-135">**SSH-felhasználónév**: hello SSH felhasználói toocreate hello Spark és Kafka fürt.</span><span class="sxs-lookup"><span data-stu-id="6474c-135">**SSH User Name**: hello SSH user toocreate for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="6474c-136">**SSH-jelszónak**: hello SSH felhasználó hello Spark és Kafka fürt hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6474c-136">**SSH Password**: hello password for hello SSH user for hello Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="6474c-137">Olvasási hello **feltételek és kikötések**, majd válassza ki **toohello feltételek és kikötések fenti elfogadom**.</span><span class="sxs-lookup"><span data-stu-id="6474c-137">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="6474c-138">Végül ellenőrizze **PIN-kód toodashboard** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="6474c-138">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="6474c-139">Körülbelül 20 percet toocreate hello fürtök szükséges.</span><span class="sxs-lookup"><span data-stu-id="6474c-139">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="6474c-140">Létrehozása után a hello erőforrásokat, átirányított tooa panel hello fürtök és a webes irányítópult tartalmazó erőforráscsoport hello áll.</span><span class="sxs-lookup"><span data-stu-id="6474c-140">Once hello resources have been created, you are redirected tooa blade for hello resource group that contains hello clusters and web dashboard.</span></span>

![Erőforráscsoport panel hello vnet és fürtök](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="6474c-142">Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **spark-BASENAME** és **kafka-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6474c-142">Notice that hello names of hello HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="6474c-143">Ezeket a neveket használja a későbbi lépésekben toohello fürtök kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="6474c-143">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="use-hello-notebooks"></a><span data-ttu-id="6474c-144">Hello notebookok használata</span><span class="sxs-lookup"><span data-stu-id="6474c-144">Use hello notebooks</span></span>

<span data-ttu-id="6474c-145">a jelen dokumentumban ismertetett hello például hello kód megtalálható [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span><span class="sxs-lookup"><span data-stu-id="6474c-145">hello code for hello example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span></span>

<span data-ttu-id="6474c-146">Hello kövesse hello `README.md` toocomplete fájl ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="6474c-146">Follow hello steps in hello `README.md` file toocomplete this example.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="6474c-147">Hello fürt törlése</span><span class="sxs-lookup"><span data-stu-id="6474c-147">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="6474c-148">Mivel a jelen dokumentumban leírt lépések hello létrehozása a fürtök hello azonos Azure-erőforráscsoportot, törölheti a hello erőforráscsoportja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6474c-148">Since hello steps in this document create both clusters in hello same Azure resource group, you can delete hello resource group in hello Azure portal.</span></span> <span data-ttu-id="6474c-149">Hello-csoport törlése eltávolítja a hozta létre a következő a dokumentum, hello Azure virtuális hálózat és tárfiók hello fürtök által használt összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="6474c-149">Deleting hello group removes all resources created by following this document, hello Azure Virtual Network, and storage account used by hello clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6474c-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6474c-150">Next steps</span></span>

<span data-ttu-id="6474c-151">Ebben a példában megtanulta, hogyan toouse Spark tooread és tooKafka írni.</span><span class="sxs-lookup"><span data-stu-id="6474c-151">In this example, you learned how toouse Spark tooread and write tooKafka.</span></span> <span data-ttu-id="6474c-152">Használja a következő hivatkozások toodiscover hello más módokon toowork Kafka:</span><span class="sxs-lookup"><span data-stu-id="6474c-152">Use hello following links toodiscover other ways toowork with Kafka:</span></span>

* [<span data-ttu-id="6474c-153">Ismerkedés az Apache Kafka a HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="6474c-153">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="6474c-154">A HDInsight MirrorMaker toocreate Kafka másolatának használata</span><span class="sxs-lookup"><span data-stu-id="6474c-154">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="6474c-155">Az Apache Storm használata a HDInsighton futó Kafkával</span><span class="sxs-lookup"><span data-stu-id="6474c-155">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

