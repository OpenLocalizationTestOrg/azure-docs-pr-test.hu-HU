---
title: az Apache Kafka - Azure HDInsight aaaStart |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate egy Apache Kafka Azure HDInsight fürt. Megtudhatja, hogyan toocreate témakörök, előfizetőket és fogyasztók."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: b93299d88dc2cf9a9764662509308ff75fd74474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="start-with-apache-kafka-preview-on-hdinsight"></a><span data-ttu-id="6e606-104">Az Apache Kafka (előzetes verzió) használatának első lépései a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="6e606-104">Start with Apache Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="6e606-105">Megtudhatja, hogyan toocreate, és egy [Apache Kafka](https://kafka.apache.org) on Azure HDInsight fürt.</span><span class="sxs-lookup"><span data-stu-id="6e606-105">Learn how toocreate and use an [Apache Kafka](https://kafka.apache.org) cluster on Azure HDInsight.</span></span> <span data-ttu-id="6e606-106">A Kafka egy, a HDInsighthoz is elérhető, nyílt forráskódú elosztott adatstreamelési platform.</span><span class="sxs-lookup"><span data-stu-id="6e606-106">Kafka is an open-source, distributed streaming platform that is available with HDInsight.</span></span> <span data-ttu-id="6e606-107">Gyakran használják üzenet közvetítőként, mivel hasonló funkciókat biztosít tooa közzétételi-feliratkozási üzenet-várólista.</span><span class="sxs-lookup"><span data-stu-id="6e606-107">It is often used as a message broker, as it provides functionality similar tooa publish-subscribe message queue.</span></span>

> [!NOTE]
> <span data-ttu-id="6e606-108">Jelenleg a Kafka két verziója érhető el a HDInsighttal: a 0.9.0 (HDInsight 3.4) és a 0.10.0 (HDInsight 3.5 és 3.6).</span><span class="sxs-lookup"><span data-stu-id="6e606-108">There are currently two versions of Kafka available with HDInsight; 0.9.0 (HDInsight 3.4) and 0.10.0 (HDInsight 3.5 and 3.6).</span></span> <span data-ttu-id="6e606-109">Ez a dokumentum hello lépések azt feltételezik, hogy a HDInsight 3.6 Kafka használ.</span><span class="sxs-lookup"><span data-stu-id="6e606-109">hello steps in this document assume that you are using Kafka on HDInsight 3.6.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a><span data-ttu-id="6e606-110">Kafka-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e606-110">Create a Kafka cluster</span></span>

<span data-ttu-id="6e606-111">Következő lépések toocreate egy Kafka HDInsight-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="6e606-111">Use hello following steps toocreate a Kafka on HDInsight cluster:</span></span>

1. <span data-ttu-id="6e606-112">A hello [Azure-portálon](https://portal.azure.com), jelölje be **+ új**, **Eszközintelligencia + analitika**, majd válassza ki **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="6e606-112">From hello [Azure portal](https://portal.azure.com), select **+ NEW**, **Intelligence + Analytics**, and then select **HDInsight**.</span></span>
   
    ![HDInsight-fürt létrehozása](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. <span data-ttu-id="6e606-114">A **alapjai**, adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="6e606-114">From **Basics**, enter hello following information:</span></span>

    * <span data-ttu-id="6e606-115">**A fürt neve**: hello hello HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="6e606-115">**Cluster Name**: hello name of hello HDInsight cluster.</span></span>
    * <span data-ttu-id="6e606-116">**Előfizetés**: hello előfizetés toouse kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="6e606-116">**Subscription**: Select hello subscription toouse.</span></span>
    * <span data-ttu-id="6e606-117">**A fürt bejelentkezési felhasználónevének** és **fürt bejelentkezési jelszó**: hello bejelentkezési hello fürt eléréséhez a HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="6e606-117">**Cluster login username** and **Cluster login password**: hello login when accessing hello cluster over HTTPS.</span></span> <span data-ttu-id="6e606-118">A hitelesítő adatok tooaccess szolgáltatások például hello Ambari webes felhasználói felületén vagy a REST API-t használja.</span><span class="sxs-lookup"><span data-stu-id="6e606-118">You use these credentials tooaccess services such as hello Ambari Web UI or REST API.</span></span>
    * <span data-ttu-id="6e606-119">**Secure Shell (SSH) felhasználónév**: hello bejelentkezési SSH-n keresztül hello fürt eléréséhez használt.</span><span class="sxs-lookup"><span data-stu-id="6e606-119">**Secure Shell (SSH) username**: hello login used when accessing hello cluster over SSH.</span></span> <span data-ttu-id="6e606-120">Alapértelmezés szerint hello jelszó hello ugyanaz, mint a hello fürt bejelentkezési jelszót.</span><span class="sxs-lookup"><span data-stu-id="6e606-120">By default hello password is hello same as hello cluster login password.</span></span>
    * <span data-ttu-id="6e606-121">**Erőforráscsoport**: hello erőforrás csoport toocreate hello fürtöt.</span><span class="sxs-lookup"><span data-stu-id="6e606-121">**Resource Group**: hello resource group toocreate hello cluster in.</span></span>
    * <span data-ttu-id="6e606-122">**Hely**: hello Azure-régiót toocreate hello fürtöt.</span><span class="sxs-lookup"><span data-stu-id="6e606-122">**Location**: hello Azure region toocreate hello cluster in.</span></span>
   
 ![Előfizetés kiválasztása](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. <span data-ttu-id="6e606-124">Válassza ki **típusú fürt**, majd a készlet hello következő értékeit és **fürtkonfiguráció**:</span><span class="sxs-lookup"><span data-stu-id="6e606-124">Select **Cluster type**, and then set hello following values from **Cluster configuration**:</span></span>
   
    * <span data-ttu-id="6e606-125">**Fürt típusa**: Kafka</span><span class="sxs-lookup"><span data-stu-id="6e606-125">**Cluster Type**: Kafka</span></span>

    * <span data-ttu-id="6e606-126">**Verzió**: Kafka 0.10.0 (HDI 3.6)</span><span class="sxs-lookup"><span data-stu-id="6e606-126">**Version**: Kafka 0.10.0 (HDI 3.6)</span></span>

    * <span data-ttu-id="6e606-127">**Fürt szintje**: Standard</span><span class="sxs-lookup"><span data-stu-id="6e606-127">**Cluster Tier**: Standard</span></span>
     
 <span data-ttu-id="6e606-128">Végül, használja a hello **válasszon** toosave beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="6e606-128">Finally, use hello **Select** button toosave settings.</span></span>
     
 ![Fürttípus kiválasztása](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. <span data-ttu-id="6e606-130">Hello fürt típusának kiválasztása után használja a hello __válasszon__ gomb tooset hello fürt típusa.</span><span class="sxs-lookup"><span data-stu-id="6e606-130">After selecting hello cluster type, use hello __Select__ button tooset hello cluster type.</span></span> <span data-ttu-id="6e606-131">Következő lépésként az hello __következő__ gomb toofinish alapszintű konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6e606-131">Next, use hello __Next__ button toofinish basic configuration.</span></span>

5. <span data-ttu-id="6e606-132">A **Tárolás** panelen válasszon ki vagy hozzon létre egy Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="6e606-132">From **Storage**, select or create a Storage account.</span></span> <span data-ttu-id="6e606-133">A jelen dokumentumban leírt lépések hello hagyja hello más mezőlistából hello alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="6e606-133">For hello steps in this document, leave hello other fields at hello default values.</span></span> <span data-ttu-id="6e606-134">Használjon hello __következő__ gomb toosave tárolási konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="6e606-134">Use hello __Next__ button toosave storage configuration.</span></span>

    ![Állítsa be a HDInsight-hello tárolási fiók beállításai](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. <span data-ttu-id="6e606-136">A __(nem kötelező) alkalmazások__, jelölje be __következő__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="6e606-136">From __Applications (optional)__, select __Next__ toocontinue.</span></span> <span data-ttu-id="6e606-137">Az alábbi példához nem szükséges alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6e606-137">No applications are required for this example.</span></span>

7. <span data-ttu-id="6e606-138">A __a fürt méretét__, jelölje be __következő__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="6e606-138">From __Cluster size__, select __Next__ toocontinue.</span></span>

    > [!WARNING]
    > <span data-ttu-id="6e606-139">a HDInsight Kafka tooguarantee rendelkezésre állását, a fürt tartalmaznia kell legalább három munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="6e606-139">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span>

    ![Set hello Kafka foglalásiegység-méret](./media/hdinsight-apache-kafka-get-started/kafka-cluster-size.png)

    > [!NOTE]
    > <span data-ttu-id="6e606-141">Hello **egyes feldolgozó csomópontok lemezek** bejegyzés vezérlők hello méretezhetőséget biztosít a hdinsight Kafka.</span><span class="sxs-lookup"><span data-stu-id="6e606-141">hello **disks per worker node** entry controls hello scalability of Kafka on HDInsight.</span></span> <span data-ttu-id="6e606-142">További információ: [Configure storage and scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md) (A HDInsightban futó Kafka tárolójának és méretezhetőségének konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="6e606-142">For more information, see [Configure storage and scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md).</span></span>

8. <span data-ttu-id="6e606-143">A __speciális beállítások__, jelölje be __következő__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="6e606-143">From __Advanced settings__, select __Next__ toocontinue.</span></span>

9. <span data-ttu-id="6e606-144">A hello **összegzés**, tekintse át a hello fürt hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="6e606-144">From hello **Summary**, review hello configuration for hello cluster.</span></span> <span data-ttu-id="6e606-145">Használjon hello __szerkesztése__ hivatkozásait toochange életbe léptetett beállítások helytelenek.</span><span class="sxs-lookup"><span data-stu-id="6e606-145">Use hello __Edit__ links toochange any settings that are incorrect.</span></span> <span data-ttu-id="6e606-146">Végezetül the__Create__ gomb toocreate hello-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="6e606-146">Finally, use the__Create__ button toocreate hello cluster.</span></span>
   
    ![A fürtkonfiguráció összegzése](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > <span data-ttu-id="6e606-148">Too20 perc toocreate hello fürt is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="6e606-148">It can take up too20 minutes toocreate hello cluster.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="6e606-149">Csatlakoztassa toohello fürtöt</span><span class="sxs-lookup"><span data-stu-id="6e606-149">Connect toohello cluster</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e606-150">Hello lépések végrehajtásához, egy SSH-ügyfelet kell használnia.</span><span class="sxs-lookup"><span data-stu-id="6e606-150">When performing hello following steps, you must use an SSH client.</span></span> <span data-ttu-id="6e606-151">További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6e606-151">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

<span data-ttu-id="6e606-152">Az ügyfél SSH tooconnect toohello-fürt használatára:</span><span class="sxs-lookup"><span data-stu-id="6e606-152">From your client, use SSH tooconnect toohello cluster:</span></span>

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

<span data-ttu-id="6e606-153">Cserélje le **SSHUSER** a fürt létrehozása során megadott hello SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="6e606-153">Replace **SSHUSER** with hello SSH username you provided during cluster creation.</span></span> <span data-ttu-id="6e606-154">Cserélje le **CLUSTERNAME** hello nevű hello fürt.</span><span class="sxs-lookup"><span data-stu-id="6e606-154">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>

<span data-ttu-id="6e606-155">Amikor a rendszer kéri, adja meg az SSH-fiókjának hello használt hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="6e606-155">When prompted, enter hello password you used for hello SSH account.</span></span>

<span data-ttu-id="6e606-156">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="6e606-156">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="6e606-157"><a id="getkafkainfo"></a>Hello Zookeeper és Broker állomásadatai beolvasása</span><span class="sxs-lookup"><span data-stu-id="6e606-157"><a id="getkafkainfo"></a>Get hello Zookeeper and Broker host information</span></span>

<span data-ttu-id="6e606-158">Amikor olyan Kafka, ismernie kell a két állomás érték; Hello *Zookeeper* gazdagépek és hello *Broker* gazdagépek.</span><span class="sxs-lookup"><span data-stu-id="6e606-158">When working with Kafka, you must know two host values; hello *Zookeeper* hosts and hello *Broker* hosts.</span></span> <span data-ttu-id="6e606-159">Ezek a gazdagépek hello Kafka API, és küldje el hello segédeszközök számos Kafka használhatók.</span><span class="sxs-lookup"><span data-stu-id="6e606-159">These hosts are used with hello Kafka API and many of hello utilities that ship with Kafka.</span></span>

<span data-ttu-id="6e606-160">A következő lépéseket toocreate környezeti változók hello állomás adatokat tartalmazó hello használata.</span><span class="sxs-lookup"><span data-stu-id="6e606-160">Use hello following steps toocreate environment variables that contain hello host information.</span></span> <span data-ttu-id="6e606-161">Ezek a környezeti változók a jelen dokumentumban leírt lépések hello használják.</span><span class="sxs-lookup"><span data-stu-id="6e606-161">These environment variables are used in hello steps in this document.</span></span>

1. <span data-ttu-id="6e606-162">SSH kapcsolat toohello fürtök, használjon hello következő parancsot a tooinstall hello `jq` segédprogram.</span><span class="sxs-lookup"><span data-stu-id="6e606-162">From an SSH connection toohello cluster, use hello following command tooinstall hello `jq` utility.</span></span> <span data-ttu-id="6e606-163">Ez a segédprogram használt tooparse JSON-dokumentumokat, és hasznos hello broker állomás adatok beolvasása:</span><span class="sxs-lookup"><span data-stu-id="6e606-163">This utility is used tooparse JSON documents, and is useful in retrieving hello broker host information:</span></span>
   
    ```bash
    sudo apt -y install jq
    ```

2. <span data-ttu-id="6e606-164">tooset hello környezeti változók információkkal lekért Ambari, a következő parancsok használata hello:</span><span class="sxs-lookup"><span data-stu-id="6e606-164">tooset hello environment variables with information retrieved from Ambari, use hello following commands:</span></span>

    ```bash
    CLUSTERNAME='your cluster name'
    PASSWORD='your cluster password'
    export KAFKAZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="6e606-165">Állítsa be `CLUSTERNAME=` hello Kafka fürt toohello nevét.</span><span class="sxs-lookup"><span data-stu-id="6e606-165">Set `CLUSTERNAME=` toohello name of hello Kafka cluster.</span></span> <span data-ttu-id="6e606-166">Állítsa be `PASSWORD=` hello fürt létrehozásakor használt toohello (rendszergazda) bejelentkezési jelszót.</span><span class="sxs-lookup"><span data-stu-id="6e606-166">Set `PASSWORD=` toohello login (admin) password you used when creating hello cluster.</span></span>

    <span data-ttu-id="6e606-167">hello következő szöveg látható egy példa hello tartalmát `$KAFKAZKHOSTS`:</span><span class="sxs-lookup"><span data-stu-id="6e606-167">hello following text is an example of hello contents of `$KAFKAZKHOSTS`:</span></span>
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    <span data-ttu-id="6e606-168">hello következő szöveg látható egy példa hello tartalmát `$KAFKABROKERS`:</span><span class="sxs-lookup"><span data-stu-id="6e606-168">hello following text is an example of hello contents of `$KAFKABROKERS`:</span></span>
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > <span data-ttu-id="6e606-169">Hello `cut` parancs a gazdagépek tootwo állomás bejegyzés használt tootrim hello listája.</span><span class="sxs-lookup"><span data-stu-id="6e606-169">hello `cut` command is used tootrim hello list of hosts tootwo host entries.</span></span> <span data-ttu-id="6e606-170">Egy Kafka fogyasztó vagy készítő létrehozásakor nem kell tooprovide hello teljes állomások listája.</span><span class="sxs-lookup"><span data-stu-id="6e606-170">You do not need tooprovide hello full list of hosts when creating a Kafka consumer or producer.</span></span>
   
    > [!WARNING]
    > <span data-ttu-id="6e606-171">Ne használja a hello információkat a munkamenet által visszaadott tooalways pontos lehet.</span><span class="sxs-lookup"><span data-stu-id="6e606-171">Do not rely on hello information returned from this session tooalways be accurate.</span></span> <span data-ttu-id="6e606-172">Ha hello fürt méretezéséhez új brókerek hozzáadásakor vagy eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="6e606-172">If you scale hello cluster, new brokers are added or removed.</span></span> <span data-ttu-id="6e606-173">Ha hiba lép fel, és egy csomópont váltja fel, hello host name hello csomópont módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="6e606-173">If a failure occurs and a node is replaced, hello host name for hello node may change.</span></span>
    >
    > <span data-ttu-id="6e606-174">Hello Zookeeper és broker állomások adatokat kell beolvasni, hamarosan használat előtt mindig érvényes információk tooensure.</span><span class="sxs-lookup"><span data-stu-id="6e606-174">You should retrieve hello Zookeeper and broker hosts information shortly before you use it tooensure you have valid information.</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="6e606-175">Üzenettémakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e606-175">Create a topic</span></span>

<span data-ttu-id="6e606-176">A Kafka *témaköröknek* nevezett kategóriákban tárolja az adatstreameket.</span><span class="sxs-lookup"><span data-stu-id="6e606-176">Kafka stores streams of data in categories called *topics*.</span></span> <span data-ttu-id="6e606-177">Az SSH kapcsolat tooa fürt headnode, a megadott Kafka toocreate témakör parancsfájl használata:</span><span class="sxs-lookup"><span data-stu-id="6e606-177">From An SSH connection tooa cluster headnode, use a script provided with Kafka toocreate a topic:</span></span>

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

<span data-ttu-id="6e606-178">Ezzel a paranccsal összekapcsolja tooZookeeper tárolt hello állomás információk használata `$KAFKAZKHOSTS`, majd hozza létre a nevű Kafka témakör **tesztelése**.</span><span class="sxs-lookup"><span data-stu-id="6e606-178">This command connects tooZookeeper using hello host information stored in `$KAFKAZKHOSTS`, and then create Kafka topic named **test**.</span></span> <span data-ttu-id="6e606-179">Hello témakör hozott létre a következő parancsfájl toolist témakörök hello ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="6e606-179">You can verify that hello topic was created by using hello following script toolist topics:</span></span>

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

<span data-ttu-id="6e606-180">hello a parancs felsorolja Kafka témakörök hello tartalmazó **tesztelése** témakör.</span><span class="sxs-lookup"><span data-stu-id="6e606-180">hello output of this command lists Kafka topics, which contains hello **test** topic.</span></span>

## <a name="produce-and-consume-records"></a><span data-ttu-id="6e606-181">Rekordok létrehozása és felhasználása</span><span class="sxs-lookup"><span data-stu-id="6e606-181">Produce and consume records</span></span>

<span data-ttu-id="6e606-182">A Kafka témakörökben tárolja a *rekordokat*.</span><span class="sxs-lookup"><span data-stu-id="6e606-182">Kafka stores *records* in topics.</span></span> <span data-ttu-id="6e606-183">A rekordokat *előállítók* hozzák létre, és *fogyasztók* használják fel.</span><span class="sxs-lookup"><span data-stu-id="6e606-183">Records are produced by *producers*, and consumed by *consumers*.</span></span> <span data-ttu-id="6e606-184">Az előállítók *Kafka-közvetítőktől* kérik le a rekordokat.</span><span class="sxs-lookup"><span data-stu-id="6e606-184">Producers retrieve records from Kafka *brokers*.</span></span> <span data-ttu-id="6e606-185">A HDInsight-fürt mindegyik feldolgozó csomópontja egy Kafka-közvetítő.</span><span class="sxs-lookup"><span data-stu-id="6e606-185">Each worker node in your HDInsight cluster is a Kafka broker.</span></span>

<span data-ttu-id="6e606-186">Írja be a következő lépéseket toostore rekordok hello teszt témakörbe korábban létrehozott, és olvassa el őket az ügyféllel hello használata:</span><span class="sxs-lookup"><span data-stu-id="6e606-186">Use hello following steps toostore records into hello test topic you created earlier, and then read them using a consumer:</span></span>

1. <span data-ttu-id="6e606-187">Hello SSH-munkamenetet, a megadott Kafka toowrite rekordok toohello témakörrel parancsfájl használata:</span><span class="sxs-lookup"><span data-stu-id="6e606-187">From hello SSH session, use a script provided with Kafka toowrite records toohello topic:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    <span data-ttu-id="6e606-188">Nem ismét toohello Rákérdezés a parancs után.</span><span class="sxs-lookup"><span data-stu-id="6e606-188">You are not returned toohello prompt after this command.</span></span> <span data-ttu-id="6e606-189">Ehelyett írja be a néhány szöveges üzenet, majd **Ctrl + C** toostop toohello témakör küldésekor.</span><span class="sxs-lookup"><span data-stu-id="6e606-189">Instead, type a few text messages and then use **Ctrl + C** toostop sending toohello topic.</span></span> <span data-ttu-id="6e606-190">A rendszer minden sort külön rekordként küld el.</span><span class="sxs-lookup"><span data-stu-id="6e606-190">Each line is sent as a separate record.</span></span>

2. <span data-ttu-id="6e606-191">A megadott Kafka tooread bejegyzéseit hello témakör parancsprogram használata:</span><span class="sxs-lookup"><span data-stu-id="6e606-191">Use a script provided with Kafka tooread records from hello topic:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    <span data-ttu-id="6e606-192">Ez a parancs hello rekordok kikeresi hello témakör, és megjeleníti őket.</span><span class="sxs-lookup"><span data-stu-id="6e606-192">This command retrieves hello records from hello topic and displays them.</span></span> <span data-ttu-id="6e606-193">Használatával `--from-beginning` hello fogyasztói toostart közli a hello elejétől hello adatfolyam, így minden rekordot a rendszer beolvassa.</span><span class="sxs-lookup"><span data-stu-id="6e606-193">Using `--from-beginning` tells hello consumer toostart from hello beginning of hello stream, so all records are retrieved.</span></span>

3. <span data-ttu-id="6e606-194">Használjon __Ctrl + C__ toostop hello fogyasztói.</span><span class="sxs-lookup"><span data-stu-id="6e606-194">Use __Ctrl + C__ toostop hello consumer.</span></span>

## <a name="producer-and-consumer-api"></a><span data-ttu-id="6e606-195">Előállítói és fogyasztói API</span><span class="sxs-lookup"><span data-stu-id="6e606-195">Producer and consumer API</span></span>

<span data-ttu-id="6e606-196">Akkor is szoftveresen is előállításához és fogyasztásához hello rekordok [Kafka API-k](http://kafka.apache.org/documentation#api).</span><span class="sxs-lookup"><span data-stu-id="6e606-196">You can also programmatically produce and consume records using hello [Kafka APIs](http://kafka.apache.org/documentation#api).</span></span> <span data-ttu-id="6e606-197">toobuild egy Java-készítő és a fogyasztói, használja a fejlesztési környezetet az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="6e606-197">toobuild a Java producer and consumer, use hello following steps from your development environment.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e606-198">A fejlesztési környezetben telepített összetevők a következő hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="6e606-198">You must have hello following components installed in your development environment:</span></span>
>
> * <span data-ttu-id="6e606-199">[Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) vagy azzal egyenértékű, például az OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="6e606-199">[Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or an equivalent, such as OpenJDK.</span></span>
>
> * [<span data-ttu-id="6e606-200">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="6e606-200">Apache Maven</span></span>](http://maven.apache.org/)
>
> * <span data-ttu-id="6e606-201">Egy SSH-ügyfél és a hello `scp` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6e606-201">An SSH client and hello `scp` command.</span></span> <span data-ttu-id="6e606-202">További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6e606-202">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

1. <span data-ttu-id="6e606-203">Töltse le a hello példák [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).</span><span class="sxs-lookup"><span data-stu-id="6e606-203">Download hello examples from [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).</span></span> <span data-ttu-id="6e606-204">Például hello készítő/fogyasztói használni hello projekt hello `Producer-Consumer` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6e606-204">For hello producer/consumer example, use hello project in hello `Producer-Consumer` directory.</span></span> <span data-ttu-id="6e606-205">Ebben a példában a következő osztályok hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6e606-205">This example contains hello following classes:</span></span>
   
    * <span data-ttu-id="6e606-206">**Futtatás** -hello fogyasztó vagy a gyártó kezdődik.</span><span class="sxs-lookup"><span data-stu-id="6e606-206">**Run** - starts either hello consumer or producer.</span></span>

    * <span data-ttu-id="6e606-207">**Gyártó** -tárolók 1 000 000 rekordot toohello témakör.</span><span class="sxs-lookup"><span data-stu-id="6e606-207">**Producer** - stores 1,000,000 records toohello topic.</span></span>

    * <span data-ttu-id="6e606-208">**Fogyasztó** -rekordot olvas hello témakör.</span><span class="sxs-lookup"><span data-stu-id="6e606-208">**Consumer** - reads records from hello topic.</span></span>

2. <span data-ttu-id="6e606-209">egy jar csomag toocreate módosítani könyvtárak toohello hello `Producer-Consumer` könyvtárra, és használja hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6e606-209">toocreate a jar package, change directories toohello location of hello `Producer-Consumer` directory and use hello following command:</span></span>

    ```
    mvn clean package
    ```

    <span data-ttu-id="6e606-210">A parancs létrehozza a `target` nevű könyvtárat, amely a `kafka-producer-consumer-1.0-SNAPSHOT.jar` nevű fájlt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6e606-210">This command creates a directory named `target`, that contains a file named `kafka-producer-consumer-1.0-SNAPSHOT.jar`.</span></span>

3. <span data-ttu-id="6e606-211">Használjon hello következő parancsok toocopy hello `kafka-producer-consumer-1.0-SNAPSHOT.jar` tooyour HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="6e606-211">Use hello following commands toocopy hello `kafka-producer-consumer-1.0-SNAPSHOT.jar` file tooyour HDInsight cluster:</span></span>
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    <span data-ttu-id="6e606-212">Cserélje le **SSHUSER** hello SSH-felhasználó a fürt, és cserélje le a **CLUSTERNAME** hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="6e606-212">Replace **SSHUSER** with hello SSH user for your cluster, and replace **CLUSTERNAME** with hello name of your cluster.</span></span> <span data-ttu-id="6e606-213">Amikor a rendszer kéri hello jelszó megadása hello SSH-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6e606-213">When prompted enter hello password for hello SSH user.</span></span>

4. <span data-ttu-id="6e606-214">Egyszer hello `scp` parancs végrehajtása hello fájl másolása, csatlakoztassa toohello fürtöt SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="6e606-214">Once hello `scp` command finishes copying hello file, connect toohello cluster using SSH.</span></span> <span data-ttu-id="6e606-215">A következő parancs toowrite rekordok toohello Teszt témakör hello használata:</span><span class="sxs-lookup"><span data-stu-id="6e606-215">Use hello following command toowrite records toohello test topic:</span></span>

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

5. <span data-ttu-id="6e606-216">Amikor hello véget ért, használja a hello parancs tooread hello a témakör a következő:</span><span class="sxs-lookup"><span data-stu-id="6e606-216">Once hello process has finished, use hello following command tooread from hello topic:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    <span data-ttu-id="6e606-217">hello rekordok Olvasás, bejegyzések, száma mellett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6e606-217">hello records read, along with a count of records, is displayed.</span></span> <span data-ttu-id="6e606-218">Láthatja, hogy néhány több mint 1 000 000 több rekordok toohello témakör parancsfájl használata a korábbi lépésben küldött hibaként naplózza.</span><span class="sxs-lookup"><span data-stu-id="6e606-218">You may see a few more than 1,000,000 logged as you sent several records toohello topic using a script in an earlier step.</span></span>

6. <span data-ttu-id="6e606-219">Használjon __Ctrl + C__ tooexit hello fogyasztói.</span><span class="sxs-lookup"><span data-stu-id="6e606-219">Use __Ctrl + C__ tooexit hello consumer.</span></span>

### <a name="multiple-consumers"></a><span data-ttu-id="6e606-220">Több fogyasztó</span><span class="sxs-lookup"><span data-stu-id="6e606-220">Multiple consumers</span></span>

<span data-ttu-id="6e606-221">A Kafka-fogyasztók egy fogyasztói csoportot használnak a rekordok olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="6e606-221">Kafka consumers use a consumer group when reading records.</span></span> <span data-ttu-id="6e606-222">Azonos csoport hello használata több felhasználóból terhelést eredmények eloszlik a témakör az olvasási műveletek.</span><span class="sxs-lookup"><span data-stu-id="6e606-222">Using hello same group with multiple consumers results in load balanced reads from a topic.</span></span> <span data-ttu-id="6e606-223">Hello csoport minden fogyasztó hello rekordok része.</span><span class="sxs-lookup"><span data-stu-id="6e606-223">Each consumer in hello group receives a portion of hello records.</span></span> <span data-ttu-id="6e606-224">Ez a folyamat, a művelet a következő használatát hello lépések toosee:</span><span class="sxs-lookup"><span data-stu-id="6e606-224">toosee this process in action, use hello following steps:</span></span>

1. <span data-ttu-id="6e606-225">Új SSH munkamenet toohello fürt, nyissa meg a, hogy két őket.</span><span class="sxs-lookup"><span data-stu-id="6e606-225">Open a new SSH session toohello cluster, so that you have two of them.</span></span> <span data-ttu-id="6e606-226">Minden munkamenet használja a hello toostart követően az ügyféllel azonos fogyasztói Csoportazonosító hello:</span><span class="sxs-lookup"><span data-stu-id="6e606-226">In each session, use hello following toostart a consumer with hello same consumer group ID:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    <span data-ttu-id="6e606-227">A paranccsal elindítja a fogyasztó hello csoport azonosítójával `mygroup`.</span><span class="sxs-lookup"><span data-stu-id="6e606-227">This command starts a consumer using hello group ID `mygroup`.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e606-228">Hello parancsokat használhatja hello [információért hello Zookeeper és Broker állomás](#getkafkainfo) szakasz tooset `$KAFKABROKERS` a SSH-munkamenethez.</span><span class="sxs-lookup"><span data-stu-id="6e606-228">Use hello commands in hello [Get hello Zookeeper and Broker host information](#getkafkainfo) section tooset `$KAFKABROKERS` for this SSH session.</span></span>

2. <span data-ttu-id="6e606-229">Nézze meg bejegyzésként minden munkamenet számok hello hello témakör megkapja.</span><span class="sxs-lookup"><span data-stu-id="6e606-229">Watch as each session counts hello records it receives from hello topic.</span></span> <span data-ttu-id="6e606-230">mindkét munkamenetek hello összesen kell kell hello azonos módon korábban kapott egy fogyasztói.</span><span class="sxs-lookup"><span data-stu-id="6e606-230">hello total of both sessions should be hello same as you received previously from one consumer.</span></span>

<span data-ttu-id="6e606-231">Az ügyfelek belül azonos csoport hello partíciókat a hello témakör segítségével kezel hello használatát.</span><span class="sxs-lookup"><span data-stu-id="6e606-231">Consumption by clients within hello same group is handled through hello partitions for hello topic.</span></span> <span data-ttu-id="6e606-232">A hello `test` korábban létrehozott témakör nyolc partíciókkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6e606-232">For hello `test` topic created earlier, it has eight partitions.</span></span> <span data-ttu-id="6e606-233">Nyissa meg a nyolc SSH-munkamenetet, és indítsa el a fogyasztó minden munkamenetben, ha mindegyik felhasználó rekordok hello témakör egyetlen partícióra olvassa be.</span><span class="sxs-lookup"><span data-stu-id="6e606-233">If you open eight SSH sessions and launch a consumer in all sessions, each consumer reads records from a single partition for hello topic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e606-234">A fogyasztói csoportban található fogyasztói példányok száma nem haladhatja meg a partíciók számát.</span><span class="sxs-lookup"><span data-stu-id="6e606-234">There cannot be more consumer instances in a consumer group than partitions.</span></span> <span data-ttu-id="6e606-235">Ebben a példában egy fogyasztói csoporton tartalmazhat tooeight fogyasztók mentése mivel az hello hello témakörben található partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="6e606-235">In this example, one consumer group can contain up tooeight consumers since that is hello number of partitions in hello topic.</span></span> <span data-ttu-id="6e606-236">Emellett lehet több, legfeljebb nyolc fogyasztóval rendelkező fogyasztói csoportja is.</span><span class="sxs-lookup"><span data-stu-id="6e606-236">Or you can have multiple consumer groups, each with no more than eight consumers.</span></span>

<span data-ttu-id="6e606-237">Kafka tárolt bejegyzések hello rendelés kaptak a partíción belül vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="6e606-237">Records stored in Kafka are stored in hello order they are received within a partition.</span></span> <span data-ttu-id="6e606-238">tooachieve-sorrendjük kézbesítési rekordok *a partíción belül*, hozzon létre egy felhasználói csoportot, ahol felhasználói példányok száma hello partíciók hello számának egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="6e606-238">tooachieve in-ordered delivery for records *within a partition*, create a consumer group where hello number of consumer instances matches hello number of partitions.</span></span> <span data-ttu-id="6e606-239">tooachieve-sorrendjük kézbesítési rekordok *hello témakör*, hozzon létre egy felhasználói csoport csak egy felhasználói példányt.</span><span class="sxs-lookup"><span data-stu-id="6e606-239">tooachieve in-ordered delivery for records *within hello topic*, create a consumer group with only one consumer instance.</span></span>

## <a name="streaming-api"></a><span data-ttu-id="6e606-240">Streamelési API</span><span class="sxs-lookup"><span data-stu-id="6e606-240">Streaming API</span></span>

<span data-ttu-id="6e606-241">adatfolyam-API hello hozzá lett adva tooKafka verziójában 0.10.0-s; a korábbi támaszkodnak Apache Spark vagy Storm adatfolyam feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="6e606-241">hello streaming API was added tooKafka in version 0.10.0; earlier versions rely on Apache Spark or Storm for stream processing.</span></span>

1. <span data-ttu-id="6e606-242">Ha még nem tette meg, töltse le a hello példák [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) tooyour fejlesztési környezet.</span><span class="sxs-lookup"><span data-stu-id="6e606-242">If you haven't already done so, download hello examples from [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) tooyour development environment.</span></span> <span data-ttu-id="6e606-243">A példa streaming hello, használjon hello projekt hello `streaming` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6e606-243">For hello streaming example, use hello project in hello `streaming` directory.</span></span>
   
    <span data-ttu-id="6e606-244">A projekt tartalmaz csak egy osztály `Stream`, amely olvassa be az rekordok hello `test` korábban létrehozott témakör.</span><span class="sxs-lookup"><span data-stu-id="6e606-244">This project contains only one class, `Stream`, which reads records from hello `test` topic created previously.</span></span> <span data-ttu-id="6e606-245">Megszámolja hello szavakat, olvassa el, és bocsát ki minden egyes nevű word és count tooa témakör `wordcounts`.</span><span class="sxs-lookup"><span data-stu-id="6e606-245">It counts hello words read, and emits each word and count tooa topic named `wordcounts`.</span></span> <span data-ttu-id="6e606-246">Hello `wordcounts` témakör ebben a szakaszban egy későbbi lépésben jön létre.</span><span class="sxs-lookup"><span data-stu-id="6e606-246">hello `wordcounts` topic is created in a later step in this section.</span></span>

2. <span data-ttu-id="6e606-247">Hello parancssorban a fejlesztési környezetben módosítani könyvtárak toohello hello `Streaming` könyvtárra, és a következő parancs toocreate jar-csomag használata hello:</span><span class="sxs-lookup"><span data-stu-id="6e606-247">From hello command line in your development environment, change directories toohello location of hello `Streaming` directory, and then use hello following command toocreate a jar package:</span></span>

    ```bash
    mvn clean package
    ```

    <span data-ttu-id="6e606-248">A parancs létrehozza a `target` nevű könyvtárat, amely a `kafka-streaming-1.0-SNAPSHOT.jar` nevű fájlt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6e606-248">This command creates a directory named `target`, which contains a file named `kafka-streaming-1.0-SNAPSHOT.jar`.</span></span>

3. <span data-ttu-id="6e606-249">Használjon hello következő parancsok toocopy hello `kafka-streaming-1.0-SNAPSHOT.jar` tooyour HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="6e606-249">Use hello following commands toocopy hello `kafka-streaming-1.0-SNAPSHOT.jar` file tooyour HDInsight cluster:</span></span>
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    <span data-ttu-id="6e606-250">Cserélje le **SSHUSER** hello SSH-felhasználó a fürt, és cserélje le a **CLUSTERNAME** hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="6e606-250">Replace **SSHUSER** with hello SSH user for your cluster, and replace **CLUSTERNAME** with hello name of your cluster.</span></span> <span data-ttu-id="6e606-251">Amikor a rendszer kéri hello jelszó megadása hello SSH-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6e606-251">When prompted enter hello password for hello SSH user.</span></span>

4. <span data-ttu-id="6e606-252">Egyszer hello `scp` parancs végrehajtása hello fájl másolása, csatlakozzon az SSH használatával toohello fürt, és használja a következő parancs toocreate hello hello `wordcounts` témakör:</span><span class="sxs-lookup"><span data-stu-id="6e606-252">Once hello `scp` command finishes copying hello file, connect toohello cluster using SSH, and then use hello following command toocreate hello `wordcounts` topic:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. <span data-ttu-id="6e606-253">Ezután indítsa el az adatfolyam-folyamat a következő parancs hello segítségével hello:</span><span class="sxs-lookup"><span data-stu-id="6e606-253">Next, start hello streaming process by using hello following command:</span></span>
   
    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    <span data-ttu-id="6e606-254">Adatfolyam-folyamat hello háttérben hello indítja el.</span><span class="sxs-lookup"><span data-stu-id="6e606-254">This command starts hello streaming process in hello background.</span></span>

6. <span data-ttu-id="6e606-255">Használjon hello következő parancsot a toosend üzenetek toohello `test` témakör.</span><span class="sxs-lookup"><span data-stu-id="6e606-255">Use hello following command toosend messages toohello `test` topic.</span></span> <span data-ttu-id="6e606-256">Ezek az üzenetek dolgozza fel hello adatfolyam-példa:</span><span class="sxs-lookup"><span data-stu-id="6e606-256">These messages are processed by hello streaming example:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. <span data-ttu-id="6e606-257">A következő parancs tooview hello kimeneti toohello írt használata hello `wordcounts` témakör hello adatfolyam-folyamat által:</span><span class="sxs-lookup"><span data-stu-id="6e606-257">Use hello following command tooview hello output that is written toohello `wordcounts` topic by hello streaming process:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > <span data-ttu-id="6e606-258">tooview hello adatokat, akkor kell mondja el hello tooprint hello kulcsa és hello deszerializáló toouse a hello kulcs-érték.</span><span class="sxs-lookup"><span data-stu-id="6e606-258">tooview hello data, you must tell hello consumer tooprint hello key and hello deserializer toouse for hello key and value.</span></span> <span data-ttu-id="6e606-259">hello kulcsnév hello szó, és hello kulcsérték hello számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6e606-259">hello key name is hello word, and hello key value contains hello count.</span></span>
   
    <span data-ttu-id="6e606-260">a kimeneti hello hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="6e606-260">hello output is similar toohello following text:</span></span>
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
        a       13805
        snow    13637
   
    > [!NOTE]
    > <span data-ttu-id="6e606-261">minden alkalommal, amikor a rendszer észlelt egy word-os hello száma.</span><span class="sxs-lookup"><span data-stu-id="6e606-261">hello count increments each time a word is encountered.</span></span>

7. <span data-ttu-id="6e606-262">Hello használata __Ctrl + C__ tooexit hello fogyasztói, akkor használja a hello `fg` toobring hello adatfolyam háttér feladat hátsó toohello előtér parancsot.</span><span class="sxs-lookup"><span data-stu-id="6e606-262">Use hello __Ctrl + C__ tooexit hello consumer, then use hello `fg` command toobring hello streaming background task back toohello foreground.</span></span> <span data-ttu-id="6e606-263">Használjon __Ctrl + C__ tooexit azt is.</span><span class="sxs-lookup"><span data-stu-id="6e606-263">Use __Ctrl + C__ tooexit it also.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="6e606-264">Hello fürt törlése</span><span class="sxs-lookup"><span data-stu-id="6e606-264">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="6e606-265">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="6e606-265">Troubleshoot</span></span>

<span data-ttu-id="6e606-266">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="6e606-266">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e606-267">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e606-267">Next steps</span></span>

<span data-ttu-id="6e606-268">Ebből a dokumentumból megismerte rendelkezik használata a HDInsight Apache Kafka hello alapjait.</span><span class="sxs-lookup"><span data-stu-id="6e606-268">In this document, you have learned hello basics of working with Apache Kafka on HDInsight.</span></span> <span data-ttu-id="6e606-269">Hello toolearn több Kafka használatával kapcsolatban a következő használja:</span><span class="sxs-lookup"><span data-stu-id="6e606-269">Use hello following toolearn more about working with Kafka:</span></span>

* [<span data-ttu-id="6e606-270">Magas rendelkezésre állású adatok biztosítása a HDInsightban futó Kafka platformmal</span><span class="sxs-lookup"><span data-stu-id="6e606-270">Ensure high availability of your data with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-high-availability.md)
* [<span data-ttu-id="6e606-271">A méretezhetőség növelése felügyelt lemezek HDInsightban futó Kafkával történő konfigurálásával</span><span class="sxs-lookup"><span data-stu-id="6e606-271">Increase scalability by configuring managed disks with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-scalability.md)
* <span data-ttu-id="6e606-272">Az [Apache Kafka dokumentációja](http://kafka.apache.org/documentation.html) a kafka.apache.org webhelyen.</span><span class="sxs-lookup"><span data-stu-id="6e606-272">[Apache Kafka documentation](http://kafka.apache.org/documentation.html) at kafka.apache.org.</span></span>
* [<span data-ttu-id="6e606-273">A HDInsight MirrorMaker toocreate Kafka másolatának használata</span><span class="sxs-lookup"><span data-stu-id="6e606-273">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="6e606-274">Az Apache Storm használata a HDInsighton futó Kafkával</span><span class="sxs-lookup"><span data-stu-id="6e606-274">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="6e606-275">Az Apache Spark használata a Kafkával a HDInsighton</span><span class="sxs-lookup"><span data-stu-id="6e606-275">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="6e606-276">Csatlakozás tooKafka egy Azure virtuális hálózaton keresztül</span><span class="sxs-lookup"><span data-stu-id="6e606-276">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
