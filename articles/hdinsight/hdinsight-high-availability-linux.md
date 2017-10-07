---
title: "Hadoop - Azure HDInsight aaaHigh rendelkezésre állásának |} Microsoft Docs"
description: "Ismerje meg, hogyan HDInsight-fürtök javítása a megbízhatóság és rendelkezésre állás további központi csomópontra. Ismerje meg, milyen hatással van ez az Ambari és a Hive, például a Hadoop-szolgáltatás is, hogyan tooindividually csatlakozás tooeach átjárócsomópont SSH használatával."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
keywords: "hadoop magas rendelkezésre állás"
ms.assetid: 99c9f59c-cf6b-4529-99d1-bf060435e8d4
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 07/28/2017
ms.author: larryfr
ms.openlocfilehash: 9ff62afe6b63b241cb984225233157219f8d7411
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="21293-105">A HDInsight-beli Hadoop-fürtök rendelkezésre állása és megbízhatósága</span><span class="sxs-lookup"><span data-stu-id="21293-105">Availability and reliability of Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="21293-106">A HDInsight-fürtök adja meg a két átjárócsomópontokkal tooincrease hello rendelkezésre állásának és megbízhatóságának Hadoop-szolgáltatás és a futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="21293-106">HDInsight clusters provide two head nodes tooincrease hello availability and reliability of Hadoop services and jobs running.</span></span>

<span data-ttu-id="21293-107">Hadoop éri el a magas rendelkezésre állást és megbízhatóságot által egy fürt több csomópontja replikálása szolgáltatásaikat és adataikat.</span><span class="sxs-lookup"><span data-stu-id="21293-107">Hadoop achieves high availability and reliability by replicating services and data across multiple nodes in a cluster.</span></span> <span data-ttu-id="21293-108">Standard disztribúciók Hadoop azonban általában csak egy átjárócsomóponttal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="21293-108">However standard distributions of Hadoop typically have only a single head node.</span></span> <span data-ttu-id="21293-109">Bármely kimaradás hello egyetlen központi csomópont hello fürt toostop működő okozhat.</span><span class="sxs-lookup"><span data-stu-id="21293-109">Any outage of hello single head node can cause hello cluster toostop working.</span></span> <span data-ttu-id="21293-110">A HDInsight két headnodes tooimprove Hadoop rendelkezésre állást és megbízhatóságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="21293-110">HDInsight provides two headnodes tooimprove Hadoop's availability and reliability.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21293-111">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="21293-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="21293-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="21293-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="availability-and-reliability-of-nodes"></a><span data-ttu-id="21293-113">Rendelkezésre állásának és megbízhatóságának csomópontok</span><span class="sxs-lookup"><span data-stu-id="21293-113">Availability and reliability of nodes</span></span>

<span data-ttu-id="21293-114">HDInsight-fürtök csomópontjai Azure virtuális gépek segítségével lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="21293-114">Nodes in an HDInsight cluster are implemented using Azure Virtual Machines.</span></span> <span data-ttu-id="21293-115">hello következő szakaszok tárgyalják hello egyes csomóponttípusok HDInsight együtt.</span><span class="sxs-lookup"><span data-stu-id="21293-115">hello following sections discuss hello individual node types used with HDInsight.</span></span> 

> [!NOTE]
> <span data-ttu-id="21293-116">A fürt típusa nem minden csomópont-típusok használhatók.</span><span class="sxs-lookup"><span data-stu-id="21293-116">Not all node types are used for a cluster type.</span></span> <span data-ttu-id="21293-117">Például egy Hadoop-fürt típusa nem rendelkezik egyetlen Nimbus csomópontot.</span><span class="sxs-lookup"><span data-stu-id="21293-117">For example, a Hadoop cluster type does not have any Nimbus nodes.</span></span> <span data-ttu-id="21293-118">További információ a HDInsight-fürttípusok által használt csomópontok című hello fürt típusok hello [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="21293-118">For more information on nodes used by HDInsight cluster types, see hello Cluster types section of hello [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) document.</span></span>

### <a name="head-nodes"></a><span data-ttu-id="21293-119">HEAD csomópontok</span><span class="sxs-lookup"><span data-stu-id="21293-119">Head nodes</span></span>

<span data-ttu-id="21293-120">tooensure magas rendelkezésre állású Hadoop szolgáltatásokat, a HDInsight két átjárócsomópontokkal biztosít.</span><span class="sxs-lookup"><span data-stu-id="21293-120">tooensure high availability of Hadoop services, HDInsight provides two head nodes.</span></span> <span data-ttu-id="21293-121">Mindkét átjárócsomópontokkal érvényesek hello HDInsight-fürtön belül futó egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="21293-121">Both head nodes are active and running within hello HDInsight cluster simultaneously.</span></span> <span data-ttu-id="21293-122">Egyes szolgáltatások, például a HDFS vagy YARN, egy adott időpontban csak "active", egy központi csomóponton.</span><span class="sxs-lookup"><span data-stu-id="21293-122">Some services, such as HDFS or YARN, are only 'active' on one head node at any given time.</span></span> <span data-ttu-id="21293-123">Más szolgáltatásokon, például a hiveserver2-n vagy a Hive Metaadattárhoz aktívak: hello mindkét központi csomópontján ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="21293-123">Other services such as HiveServer2 or Hive MetaStore are active on both head nodes at hello same time.</span></span>

<span data-ttu-id="21293-124">HEAD csomópontok (és más csomópontok a Hdinsightban) hello állomásnév hello csomópont részeként numerikus értéknek kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="21293-124">Head nodes (and other nodes in HDInsight) have a numeric value as part of hello hostname of hello node.</span></span> <span data-ttu-id="21293-125">Például `hn0-CLUSTERNAME` vagy `hn4-CLUSTERNAME`.</span><span class="sxs-lookup"><span data-stu-id="21293-125">For example, `hn0-CLUSTERNAME` or `hn4-CLUSTERNAME`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21293-126">Ne társítson hello numerikus érték-e a csomópont a elsődleges vagy másodlagos.</span><span class="sxs-lookup"><span data-stu-id="21293-126">Do not associate hello numeric value with whether a node is primary or secondary.</span></span> <span data-ttu-id="21293-127">hello numerikus érték csak a jelenlegi tooprovide egy egyedi nevet az egyes csomópontok.</span><span class="sxs-lookup"><span data-stu-id="21293-127">hello numeric value is only present tooprovide a unique name for each node.</span></span>

### <a name="nimbus-nodes"></a><span data-ttu-id="21293-128">Nimbus-csomópontok</span><span class="sxs-lookup"><span data-stu-id="21293-128">Nimbus Nodes</span></span>

<span data-ttu-id="21293-129">Nimbus csomópont alatt futó Storm-fürtökkel érhetők el.</span><span class="sxs-lookup"><span data-stu-id="21293-129">Nimbus nodes are available with Storm clusters.</span></span> <span data-ttu-id="21293-130">hello Nimbus csomópontot hasonló funkciókat toohello Hadoop JobTracker terjesztése és a feldolgozó csomópontok átívelő figyelésére is alkalmas feldolgozási adja meg.</span><span class="sxs-lookup"><span data-stu-id="21293-130">hello Nimbus nodes provide similar functionality toohello Hadoop JobTracker by distributing and monitoring processing across worker nodes.</span></span> <span data-ttu-id="21293-131">A HDInsight két Nimbus csomóponttal rendelkezik a Storm-fürtök</span><span class="sxs-lookup"><span data-stu-id="21293-131">HDInsight provides two Nimbus nodes for Storm clusters</span></span>

### <a name="zookeeper-nodes"></a><span data-ttu-id="21293-132">Zookeeper csomópontok</span><span class="sxs-lookup"><span data-stu-id="21293-132">Zookeeper nodes</span></span>

<span data-ttu-id="21293-133">[ZooKeeper](http://zookeeper.apache.org/) csomópontok használt fő szolgáltatást az átjárócsomópontokkal vezető választás.</span><span class="sxs-lookup"><span data-stu-id="21293-133">[ZooKeeper](http://zookeeper.apache.org/) nodes are used for leader election of master services on head nodes.</span></span> <span data-ttu-id="21293-134">Azok is, hogy szolgáltatások, a adatcsomópontokat (munkavégző) és az átjárók tudja, melyik szolgáltatás főkulcsának aktív központi csomópont használt tooinsure.</span><span class="sxs-lookup"><span data-stu-id="21293-134">They are also used tooinsure that services, data (worker) nodes, and gateways know which head node a master service is active on.</span></span> <span data-ttu-id="21293-135">Alapértelmezés szerint a HDInsight nyújt három ZooKeeper csomópontok.</span><span class="sxs-lookup"><span data-stu-id="21293-135">By default, HDInsight provides three ZooKeeper nodes.</span></span>

### <a name="worker-nodes"></a><span data-ttu-id="21293-136">Munkavégző csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="21293-136">Worker nodes</span></span>

<span data-ttu-id="21293-137">Munkavégző csomópontokhoz hello tényleges adatelemzés hajtható végre, ha egy feladat elküldött toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="21293-137">Worker nodes perform hello actual data analysis when a job is submitted toohello cluster.</span></span> <span data-ttu-id="21293-138">Ha egy feldolgozó csomópont leáll, hello azt végző feladata elküldött tooanother munkavégző csomópont.</span><span class="sxs-lookup"><span data-stu-id="21293-138">If a worker node fails, hello task that it was performing is submitted tooanother worker node.</span></span> <span data-ttu-id="21293-139">Alapértelmezés szerint a HDInsight létrehoz négy munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="21293-139">By default, HDInsight creates four worker nodes.</span></span> <span data-ttu-id="21293-140">Módosíthatja a szám toosuit igényeinek alatt és a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="21293-140">You can change this number toosuit your needs both during and after cluster creation.</span></span>

### <a name="edge-node"></a><span data-ttu-id="21293-141">Élcsomópont</span><span class="sxs-lookup"><span data-stu-id="21293-141">Edge node</span></span>

<span data-ttu-id="21293-142">Egy élcsomópontot nem aktívan részt adatelemzés hello fürtön belül.</span><span class="sxs-lookup"><span data-stu-id="21293-142">An edge node does not actively participate in data analysis within hello cluster.</span></span> <span data-ttu-id="21293-143">Amikor olyan Hadoop fejlesztők vagy adatszakértőkön szolgál.</span><span class="sxs-lookup"><span data-stu-id="21293-143">It is used by developers or data scientists when working with Hadoop.</span></span> <span data-ttu-id="21293-144">hello peremhálózati csomópont életét a hello azonos Azure virtuális hálózaton hello hello fürt többi csomópontjára, és közvetlenül hozzáférhet az összes többi csomópontok.</span><span class="sxs-lookup"><span data-stu-id="21293-144">hello edge node lives in hello same Azure Virtual Network as hello other nodes in hello cluster, and can directly access all other nodes.</span></span> <span data-ttu-id="21293-145">hello élcsomópont kritikus Hadoop-szolgáltatás vagy a feladatok távol erőforrások anélkül is használható.</span><span class="sxs-lookup"><span data-stu-id="21293-145">hello edge node can be used without taking resources away from critical Hadoop services or analysis jobs.</span></span>

<span data-ttu-id="21293-146">Az R Server on HDInsight jelenleg hello csak fürt típusa, amely alapértelmezés szerint egy élcsomópontot biztosít.</span><span class="sxs-lookup"><span data-stu-id="21293-146">Currently, R Server on HDInsight is hello only cluster type that provides an edge node by default.</span></span> <span data-ttu-id="21293-147">Az R Server on HDInsight, hello élcsomópont használjuk teszt R kód helyileg hello csomóponton való elküldése toohello fürt elosztott feldolgozás előtt.</span><span class="sxs-lookup"><span data-stu-id="21293-147">For R Server on HDInsight, hello edge node is used test R code locally on hello node before submitting it toohello cluster for distributed processing.</span></span>

<span data-ttu-id="21293-148">Egy élcsomópontot fürt típusa nem R Server használatáról információkért lásd: hello [peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="21293-148">For information on using an edge node with cluster types other than R Server, see hello [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) document.</span></span>

## <a name="accessing-hello-nodes"></a><span data-ttu-id="21293-149">Hello csomópontok elérése</span><span class="sxs-lookup"><span data-stu-id="21293-149">Accessing hello nodes</span></span>

<span data-ttu-id="21293-150">Hozzáférés toohello fürt keresztül hello internet biztosított nyilvános átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="21293-150">Access toohello cluster over hello internet is provided through a public gateway.</span></span> <span data-ttu-id="21293-151">Hozzáférése korlátozott tooconnecting toohello átjárócsomópontokkal és (ha van ilyen) hello élcsomópont.</span><span class="sxs-lookup"><span data-stu-id="21293-151">Access is limited tooconnecting toohello head nodes and (if one exists) hello edge node.</span></span> <span data-ttu-id="21293-152">Hozzáférés tooservices hello központi csomópontokon futó azzal, hogy több átjárócsomópontokkal nem történik.</span><span class="sxs-lookup"><span data-stu-id="21293-152">Access tooservices running on hello head nodes is not effected by having multiple head nodes.</span></span> <span data-ttu-id="21293-153">hello nyilvános átjáró útvonalak kérelmek toohello átjárócsomópont hello üzemeltető kért szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="21293-153">hello public gateway routes requests toohello head node that hosts hello requested service.</span></span> <span data-ttu-id="21293-154">Például ha Ambari kiszolgálóvá hello másodlagos átjárócsomópont, hello átjáró irányítja a bejövő kérelmek Ambari toothat csomópont.</span><span class="sxs-lookup"><span data-stu-id="21293-154">For example, if Ambari is currently hosted on hello secondary head node, hello gateway routes incoming requests for Ambari toothat node.</span></span>

<span data-ttu-id="21293-155">A rendszer korlátozott tooport 443-as (HTTPS), a 22-es és 23 elérést hello nyilvános átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="21293-155">Access over hello public gateway is limited tooport 443 (HTTPS), 22, and 23.</span></span>

* <span data-ttu-id="21293-156">Port __443-as__ használt tooaccess az Ambari és egyéb webes felhasználói felületén vagy a REST API-k hello átjárócsomópontokkal üzemeltet.</span><span class="sxs-lookup"><span data-stu-id="21293-156">Port __443__ is used tooaccess Ambari and other web UI or REST APIs hosted on hello head nodes.</span></span>

* <span data-ttu-id="21293-157">Port __22__ használt tooaccess hello elsődleges központi csomópont és él SSH-csomópont.</span><span class="sxs-lookup"><span data-stu-id="21293-157">Port __22__ is used tooaccess hello primary head node or edge node with SSH.</span></span>

* <span data-ttu-id="21293-158">Port __23__ használt tooaccess hello másodlagos átjárócsomópont az SSH van.</span><span class="sxs-lookup"><span data-stu-id="21293-158">Port __23__ is used tooaccess hello secondary head node with SSH.</span></span> <span data-ttu-id="21293-159">Például `ssh username@mycluster-ssh.azurehdinsight.net` toohello elsődleges nevű hello fürt átjárócsomópontjához csatlakozik **sajátfürt**.</span><span class="sxs-lookup"><span data-stu-id="21293-159">For example, `ssh username@mycluster-ssh.azurehdinsight.net` connects toohello primary head node of hello cluster named **mycluster**.</span></span>

<span data-ttu-id="21293-160">További információ az SSH használatával, lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="21293-160">For more information on using SSH, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

### <a name="internal-fully-qualified-domain-names-fqdn"></a><span data-ttu-id="21293-161">Belső teljes tartománynevek (FQDN)</span><span class="sxs-lookup"><span data-stu-id="21293-161">Internal fully qualified domain names (FQDN)</span></span>

<span data-ttu-id="21293-162">HDInsight-fürtök csomópontjai rendelkeznek, egy belső IP-cím és a teljes Tartománynevet, amellyel csak hello fürtből érhető el.</span><span class="sxs-lookup"><span data-stu-id="21293-162">Nodes in an HDInsight cluster have an internal IP address and FQDN that can only be accessed from hello cluster.</span></span> <span data-ttu-id="21293-163">Szolgáltatások hello belső FQDN vagy IP-cím használatával hello fürtön elérésekor használandó Ambari tooverify hello IP-cím vagy FQDN toouse hello szolgáltatás elérésével.</span><span class="sxs-lookup"><span data-stu-id="21293-163">When accessing services on hello cluster using hello internal FQDN or IP address, you should use Ambari tooverify hello IP or FQDN toouse when accessing hello service.</span></span>

<span data-ttu-id="21293-164">Például hello szolgáltatást csak futtathatja egy átjárócsomópont Oozie és hello használata `oozie` parancsot SSH-munkamenetet a hello URL-cím toohello szolgáltatás szükséges.</span><span class="sxs-lookup"><span data-stu-id="21293-164">For example, hello Oozie service can only run on one head node, and using hello `oozie` command from an SSH session requires hello URL toohello service.</span></span> <span data-ttu-id="21293-165">Az URL-cím lehet beolvasni az Ambari hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="21293-165">This URL can be retrieved from Ambari by using hello following command:</span></span>

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

<span data-ttu-id="21293-166">Ez a parancs visszaadja egy értéket a következő parancsot, amely tartalmaz hello belső URL-cím toouse hello a hasonló toohello `oozie` parancs:</span><span class="sxs-lookup"><span data-stu-id="21293-166">This command returns a value similar toohello following command, which contains hello internal URL toouse with hello `oozie` command:</span></span>

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

<span data-ttu-id="21293-167">Hello Ambari REST API használatával további információkért lásd: [figyelése és kezelése HDInsight hello Ambari REST API-t a](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="21293-167">For more information on working with hello Ambari REST API, see [Monitor and Manage HDInsight using hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

### <a name="accessing-other-node-types"></a><span data-ttu-id="21293-168">Egyéb elérése</span><span class="sxs-lookup"><span data-stu-id="21293-168">Accessing other node types</span></span>

<span data-ttu-id="21293-169">Toonodes, amelyek nem érhető el over internet hello hello a következő módszerek használatával közvetlenül kapcsolódhatnak:</span><span class="sxs-lookup"><span data-stu-id="21293-169">You can connect toonodes that are not directly accessible over hello internet by using hello following methods:</span></span>

* <span data-ttu-id="21293-170">**SSH**: Miután csatlakozott az SSH, használatával tooa átjárócsomópont ezután használhatja az SSH a hello átjárócsomópont tooconnect tooother hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="21293-170">**SSH**: Once connected tooa head node using SSH, you can then use SSH from hello head node tooconnect tooother nodes in hello cluster.</span></span> <span data-ttu-id="21293-171">További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="21293-171">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

* <span data-ttu-id="21293-172">**SSH-alagút**: Ha egy webszolgáltatás-bővítmény üzemeltetett tooaccess van szüksége, amely nem hello csomópontok egyikét kitett toohello internet, használnia kell az SSH-alagút.</span><span class="sxs-lookup"><span data-stu-id="21293-172">**SSH Tunnel**: If you need tooaccess a web service hosted on one of hello nodes that is not exposed toohello internet, you must use an SSH tunnel.</span></span> <span data-ttu-id="21293-173">További információkért lásd: hello [használhat SSH-alagutat a hdinsight eszközzel](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="21293-173">For more information, see hello [Use an SSH tunnel with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

* <span data-ttu-id="21293-174">**Azure-beli virtuális hálózat**: Ha a HDInsight fürt része egy Azure virtuális hálózatra, az erőforrásoknál hello ugyanaz a virtuális hálózati közvetlenül hozzáférhet hello fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="21293-174">**Azure Virtual Network**: If your HDInsight cluster is part of an Azure Virtual Network, any resource on hello same Virtual Network can directly access all nodes in hello cluster.</span></span> <span data-ttu-id="21293-175">További információkért lásd: hello [kiterjesztése HDInsight az Azure Virtual Network a](hdinsight-extend-hadoop-virtual-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="21293-175">For more information, see hello [Extend HDInsight using Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

## <a name="how-toocheck-on-a-service-status"></a><span data-ttu-id="21293-176">Hogyan toocheck a szolgáltatás állapota</span><span class="sxs-lookup"><span data-stu-id="21293-176">How toocheck on a service status</span></span>

<span data-ttu-id="21293-177">hello átjárócsomópontokról futó szolgáltatások állapotának toocheck hello hello Ambari webes felhasználói felület használata, vagy hello Ambari REST API-t.</span><span class="sxs-lookup"><span data-stu-id="21293-177">toocheck hello status of services that run on hello head nodes, use hello Ambari Web UI or hello Ambari REST API.</span></span>

### <a name="ambari-web-ui"></a><span data-ttu-id="21293-178">Ambari webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="21293-178">Ambari Web UI</span></span>

<span data-ttu-id="21293-179">Ambari webes felhasználói felületén hello: https://CLUSTERNAME.azurehdinsight.net megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="21293-179">hello Ambari Web UI is viewable at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="21293-180">Cserélje le **CLUSTERNAME** hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="21293-180">Replace **CLUSTERNAME** with hello name of your cluster.</span></span> <span data-ttu-id="21293-181">Ha a rendszer kéri, adja meg a hello HTTP felhasználói hitelesítő adatok a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="21293-181">If prompted, enter hello HTTP user credentials for your cluster.</span></span> <span data-ttu-id="21293-182">hello alapértelmezett HTTP felhasználónév **admin** hello jelszó pedig hello fürt létrehozásakor megadott hello jelszó.</span><span class="sxs-lookup"><span data-stu-id="21293-182">hello default HTTP user name is **admin** and hello password is hello password you entered when creating hello cluster.</span></span>

<span data-ttu-id="21293-183">Hello Ambari oldalon érkezésekor hello balra hello lap hello telepített szolgáltatások listája látható.</span><span class="sxs-lookup"><span data-stu-id="21293-183">When you arrive on hello Ambari page, hello installed services are listed on hello left of hello page.</span></span>

![Telepített szolgáltatások](./media/hdinsight-high-availability-linux/services.png)

<span data-ttu-id="21293-185">Nincsenek tovább tooa szolgáltatás tooindicate állapota látható ikonok sorozata.</span><span class="sxs-lookup"><span data-stu-id="21293-185">There are a series of icons that may appear next tooa service tooindicate status.</span></span> <span data-ttu-id="21293-186">E riasztások kapcsolódó tooa szolgáltatás megtekinthetők a hello **riasztások** hivatkozás hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="21293-186">Any alerts related tooa service can be viewed using hello **Alerts** link at hello top of hello page.</span></span> <span data-ttu-id="21293-187">Minden szolgáltatás tooview választhat további tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="21293-187">You can select each service tooview more information on it.</span></span>

<span data-ttu-id="21293-188">Hello szolgáltatás lapján információt nyújt hello állapotát, és minden egyes szolgáltatás konfigurációját, amíg azt nem szolgál információval, amelyen átjárócsomópont hello szolgáltatás fut a.</span><span class="sxs-lookup"><span data-stu-id="21293-188">While hello service page provides information on hello status and configuration of each service, it does not provide information on which head node hello service is running on.</span></span> <span data-ttu-id="21293-189">tooview ezen információk, használjon hello **állomások** hivatkozás hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="21293-189">tooview this information, use hello **Hosts** link at hello top of hello page.</span></span> <span data-ttu-id="21293-190">Ez a lap megjeleníti a hello fürtben, beleértve a hello átjárócsomópontokkal állomások.</span><span class="sxs-lookup"><span data-stu-id="21293-190">This page displays hosts within hello cluster, including hello head nodes.</span></span>

![állomások listájához](./media/hdinsight-high-availability-linux/hosts.png)

<span data-ttu-id="21293-192">Egy hello átjárócsomópontokkal kiválasztásával hello hivatkozás hello szolgáltatás és a leállt csomóponton összetevők megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="21293-192">Selecting hello link for one of hello head nodes displays hello services and components running on that node.</span></span>

![Összetevő-állapot](./media/hdinsight-high-availability-linux/nodeservices.png)

<span data-ttu-id="21293-194">Ambari használatával kapcsolatos további információkért lásd: [figyelése és kezelése a HDInsight hello Ambari webes felhasználói felületén a](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="21293-194">For more information on using Ambari, see [Monitor and manage HDInsight using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

### <a name="ambari-rest-api"></a><span data-ttu-id="21293-195">Ambari REST API-n</span><span class="sxs-lookup"><span data-stu-id="21293-195">Ambari REST API</span></span>

<span data-ttu-id="21293-196">hello Ambari REST API-n keresztül hello érhető internet.</span><span class="sxs-lookup"><span data-stu-id="21293-196">hello Ambari REST API is available over hello internet.</span></span> <span data-ttu-id="21293-197">hello HDInsight nyilvános átjáró útválasztási kérelmek toohello átjárócsomópont jelenleg hello REST API-t futtató kezeli.</span><span class="sxs-lookup"><span data-stu-id="21293-197">hello HDInsight public gateway handles routing requests toohello head node that is currently hosting hello REST API.</span></span>

<span data-ttu-id="21293-198">A következő parancs toocheck hello hello Ambari REST API-n keresztül szolgáltatás állapotának hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="21293-198">You can use hello following command toocheck hello state of a service through hello Ambari REST API:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* <span data-ttu-id="21293-199">Cserélje le **jelszó** a hello HTTP (rendszergazda) felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="21293-199">Replace **PASSWORD** with hello HTTP user (admin) account password.</span></span>
* <span data-ttu-id="21293-200">Cserélje le **CLUSTERNAME** hello nevű hello fürt.</span><span class="sxs-lookup"><span data-stu-id="21293-200">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>
* <span data-ttu-id="21293-201">Cserélje le **szolgáltatásnév** hello nevű hello szolgáltatást szeretné toocheck hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="21293-201">Replace **SERVICENAME** with hello name of hello service you want toocheck hello status of.</span></span>

<span data-ttu-id="21293-202">Például toocheck hello állapotának hello **HDFS** szolgáltatás nevű fürt **sajátfürt**, a jelszó **jelszó**, hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="21293-202">For example, toocheck hello status of hello **HDFS** service on a cluster named **mycluster**, with a password of **password**, you would use hello following command:</span></span>

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

<span data-ttu-id="21293-203">a rendszer a következő JSON hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="21293-203">hello response is similar toohello following JSON:</span></span>

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

<span data-ttu-id="21293-204">hello URL-cím közli velünk, hogy hello szolgáltatás jelenleg fut egy átjárócsomópont nevű **hn0-CLUSTERNAME**.</span><span class="sxs-lookup"><span data-stu-id="21293-204">hello URL tells us that hello service is currently running on a head node named **hn0-CLUSTERNAME**.</span></span>

<span data-ttu-id="21293-205">hello állapot jelzi, hogy hello szolgáltatás jelenleg fut, vagy **elindítva**.</span><span class="sxs-lookup"><span data-stu-id="21293-205">hello state tells us that hello service is currently running, or **STARTED**.</span></span>

<span data-ttu-id="21293-206">Ha nem tudja, milyen szolgáltatások telepítve vannak a hello fürtön, használhatja a következő parancs tooretrieve listáját hello:</span><span class="sxs-lookup"><span data-stu-id="21293-206">If you do not know what services are installed on hello cluster, you can use hello following command tooretrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

<span data-ttu-id="21293-207">Hello Ambari REST API használatával további információkért lásd: [figyelése és kezelése HDInsight hello Ambari REST API-t a](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="21293-207">For more information on working with hello Ambari REST API, see [Monitor and Manage HDInsight using hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

#### <a name="service-components"></a><span data-ttu-id="21293-208">Szolgáltatás-összetevők</span><span class="sxs-lookup"><span data-stu-id="21293-208">Service components</span></span>

<span data-ttu-id="21293-209">Szolgáltatások tartalmazhatnak toocheck hello állapotának külön-külön kívánja összetevőket.</span><span class="sxs-lookup"><span data-stu-id="21293-209">Services may contain components that you wish toocheck hello status of individually.</span></span> <span data-ttu-id="21293-210">Például a HDFS hello NameNode összetevőt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="21293-210">For example, HDFS contains hello NameNode component.</span></span> <span data-ttu-id="21293-211">egy összetevő tooview információkért hello parancs a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="21293-211">tooview information on a component, hello command would be:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

<span data-ttu-id="21293-212">Ha nem tudja, milyen összetevők szolgáltatás által biztosított, a következő parancs tooretrieve listáját hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="21293-212">If you do not know what components are provided by a service, you can use hello following command tooretrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-tooaccess-log-files-on-hello-head-nodes"></a><span data-ttu-id="21293-213">Hogyan tooaccess naplófájljai hello átjárócsomópontokkal</span><span class="sxs-lookup"><span data-stu-id="21293-213">How tooaccess log files on hello head nodes</span></span>

### <a name="ssh"></a><span data-ttu-id="21293-214">SSH</span><span class="sxs-lookup"><span data-stu-id="21293-214">SSH</span></span>

<span data-ttu-id="21293-215">Csatlakoztatott tooa átjárócsomópont SSH-n keresztül, miközben naplófájlok alatt található **/var/log**.</span><span class="sxs-lookup"><span data-stu-id="21293-215">While connected tooa head node through SSH, log files can be found under **/var/log**.</span></span> <span data-ttu-id="21293-216">Például **/var/log/hadoop-yarn/yarn** YARN naplók tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="21293-216">For example, **/var/log/hadoop-yarn/yarn** contain logs for YARN.</span></span>

<span data-ttu-id="21293-217">Minden egyes átjárócsomópont egyedi naplóbejegyzések, van így ellenőrizni kell a hello bejelentkezik is.</span><span class="sxs-lookup"><span data-stu-id="21293-217">Each head node can have unique log entries, so you should check hello logs on both.</span></span>

### <a name="sftp"></a><span data-ttu-id="21293-218">SFTP</span><span class="sxs-lookup"><span data-stu-id="21293-218">SFTP</span></span>

<span data-ttu-id="21293-219">Toohello átjárócsomópont hello SSH File Transfer Protocol vagy biztonságos fájl Transfer Protocol (SFTP) segítségével csatlakozzon, és közvetlenül letöltheti hello naplófájlokat is.</span><span class="sxs-lookup"><span data-stu-id="21293-219">You can also connect toohello head node using hello SSH File Transfer Protocol or Secure File Transfer Protocol (SFTP), and download hello log files directly.</span></span>

<span data-ttu-id="21293-220">Egy SSH-ügyfél, meg kell adnia toohello fürt kapcsolódáskor hasonló toousing hello SSH felhasználói fiók nevét és hello SSH-cím hello fürt.</span><span class="sxs-lookup"><span data-stu-id="21293-220">Similar toousing an SSH client, when connecting toohello cluster you must provide hello SSH user account name and hello SSH address of hello cluster.</span></span> <span data-ttu-id="21293-221">Például: `sftp username@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="21293-221">For example, `sftp username@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="21293-222">Hello jelszót adjon meg hello fiókot, amikor a rendszer kéri, vagy adjon meg egy nyilvános kulcsot használó hello `-i` paraméter.</span><span class="sxs-lookup"><span data-stu-id="21293-222">Provide hello password for hello account when prompted, or provide a public key using hello `-i` parameter.</span></span>

<span data-ttu-id="21293-223">Miután csatlakozott, lehetősége lesz a `sftp>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="21293-223">Once connected, you are presented with a `sftp>` prompt.</span></span> <span data-ttu-id="21293-224">A parancssorból módosíthatja, könyvtárak, fájlok feltöltését és letöltését.</span><span class="sxs-lookup"><span data-stu-id="21293-224">From this prompt, you can change directories, upload, and download files.</span></span> <span data-ttu-id="21293-225">Például a következő parancsok hello módosítása könyvtárak toohello **/var/log/hadoop/hdfs** directory és a majd letöltési hello directory összes fájlját.</span><span class="sxs-lookup"><span data-stu-id="21293-225">For example, hello following commands change directories toohello **/var/log/hadoop/hdfs** directory and then download all files in hello directory.</span></span>

    cd /var/log/hadoop/hdfs
    get *

<span data-ttu-id="21293-226">Elérhető parancsok listáját, írja be a `help` : hello `sftp>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="21293-226">For a list of available commands, enter `help` at hello `sftp>` prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="21293-227">Van grafikus felületek, amelyek lehetővé teszik toovisualize hello fájlrendszer SFTP használatával csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="21293-227">There are also graphical interfaces that allow you toovisualize hello file system when connected using SFTP.</span></span> <span data-ttu-id="21293-228">Például [MobaXTerm](http://mobaxterm.mobatek.net/) lehetővé teszi egy felület hasonló tooWindows Explorer használt toobrowse hello fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="21293-228">For example, [MobaXTerm](http://mobaxterm.mobatek.net/) allows you toobrowse hello file system using an interface similar tooWindows Explorer.</span></span>

### <a name="ambari"></a><span data-ttu-id="21293-229">Ambari</span><span class="sxs-lookup"><span data-stu-id="21293-229">Ambari</span></span>

> [!NOTE]
> <span data-ttu-id="21293-230">tooaccess naplófájlok Ambari használatával, az SSH-alagút kell használnia.</span><span class="sxs-lookup"><span data-stu-id="21293-230">tooaccess log files using Ambari, you must use an SSH tunnel.</span></span> <span data-ttu-id="21293-231">az egyes szolgáltatások hello hello webes felületek nem érhetők el nyilvánosan az interneten hello.</span><span class="sxs-lookup"><span data-stu-id="21293-231">hello web interfaces for hello individual services are not exposed publicly on hello Internet.</span></span> <span data-ttu-id="21293-232">Az SSH-alagúton keresztül információkért lásd: hello [használata SSH Tunneling](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="21293-232">For information on using an SSH tunnel, see hello [Use SSH Tunneling](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

<span data-ttu-id="21293-233">Hello Ambari webes felhasználói felületén jelölje ki kívánja tooview naplók (például YARN) a hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="21293-233">From hello Ambari Web UI, select hello service you wish tooview logs for (for example, YARN).</span></span> <span data-ttu-id="21293-234">Ezután **Gyorshivatkozások** tooselect mely átjárócsomópont tooview hello naplózza.</span><span class="sxs-lookup"><span data-stu-id="21293-234">Then use **Quick Links** tooselect which head node tooview hello logs for.</span></span>

![Tooview naplók segítségével gyors hivatkozásokat tartalmaz.](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-tooconfigure-hello-node-size"></a><span data-ttu-id="21293-236">Hogyan tooconfigure hello csomópont mérete</span><span class="sxs-lookup"><span data-stu-id="21293-236">How tooconfigure hello node size</span></span>

<span data-ttu-id="21293-237">egy csomópont hello mérete csak akkor jelölhető ki, fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="21293-237">hello size of a node can only be selected during cluster creation.</span></span> <span data-ttu-id="21293-238">Található hello listája elérhető különböző VM méretű hdinsight hello [árképzést ismertető oldalra HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="21293-238">You can find a list of hello different VM sizes available for HDInsight on hello [HDInsight pricing page](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="21293-239">A fürt létrehozásakor megadhatja hello csomópontok hello méretét.</span><span class="sxs-lookup"><span data-stu-id="21293-239">When creating a cluster, you can specify hello size of hello nodes.</span></span> <span data-ttu-id="21293-240">hello alábbi információkat nyújt útmutatást hogyan toospecify hello méret használatával hello [Azure-portálon][preview-portal], [Azure PowerShell][azure-powershell], és hello [Azure CLI][azure-cli]:</span><span class="sxs-lookup"><span data-stu-id="21293-240">hello following information provides guidance on how toospecify hello size using hello [Azure portal][preview-portal], [Azure PowerShell][azure-powershell], and hello [Azure CLI][azure-cli]:</span></span>

* <span data-ttu-id="21293-241">**Azure-portálon**: a fürt létrehozásakor beállíthatja hello fürt által használt hello csomópontok hello mérete:</span><span class="sxs-lookup"><span data-stu-id="21293-241">**Azure portal**: When creating a cluster, you can set hello size of hello nodes used by hello cluster:</span></span>

    ![A méret kiválasztása a fürt létrehozása varázsló képe](./media/hdinsight-high-availability-linux/headnodesize.png)

* <span data-ttu-id="21293-243">**Az Azure CLI**: hello használatakor `azure hdinsight cluster create` parancs hello segítségével beállíthatja a hello mérete hello head, munkavégző és ZooKeeper csomópontok `--headNodeSize`, `--workerNodeSize`, és `--zookeeperNodeSize` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="21293-243">**Azure CLI**: When using hello `azure hdinsight cluster create` command, you can set hello size of hello head, worker, and ZooKeeper nodes by using hello `--headNodeSize`, `--workerNodeSize`, and `--zookeeperNodeSize` parameters.</span></span>

* <span data-ttu-id="21293-244">**Az Azure PowerShell**: hello használatakor `New-AzureRmHDInsightCluster` parancsmaggal beállíthatja hello mérete hello head, munkavégző és ZooKeeper csomópontok hello segítségével `-HeadNodeVMSize`, `-WorkerNodeSize`, és `-ZookeeperNodeSize` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="21293-244">**Azure PowerShell**: When using hello `New-AzureRmHDInsightCluster` cmdlet, you can set hello size of hello head, worker, and ZooKeeper nodes by using hello `-HeadNodeVMSize`, `-WorkerNodeSize`, and `-ZookeeperNodeSize` parameters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21293-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21293-245">Next steps</span></span>

<span data-ttu-id="21293-246">A következő hivatkozások toolearn további ebben a dokumentumban említett szempontot hello használata.</span><span class="sxs-lookup"><span data-stu-id="21293-246">Use hello following links toolearn more about things mentioned in this document.</span></span>

* [<span data-ttu-id="21293-247">Ambari REST-referencia</span><span class="sxs-lookup"><span data-stu-id="21293-247">Ambari REST Reference</span></span>](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [<span data-ttu-id="21293-248">Telepítse és konfigurálja a hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="21293-248">Install and configure hello Azure CLI</span></span>](../cli-install-nodejs.md)
* [<span data-ttu-id="21293-249">Telepítse és konfigurálja az Azure PowerShellt</span><span class="sxs-lookup"><span data-stu-id="21293-249">Install and configure Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="21293-250">HDInsight a Ambari kezelése</span><span class="sxs-lookup"><span data-stu-id="21293-250">Manage HDInsight using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)
* [<span data-ttu-id="21293-251">Linux-alapú HDInsight-fürtök kiépítése</span><span class="sxs-lookup"><span data-stu-id="21293-251">Provision Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
