---
title: "Magas rendelkezésre állás a Hadoop - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan HDInsight-fürtök javítása a megbízhatóság és rendelkezésre állás további központi csomópontra. Ismerje meg, milyen hatással van ez Hadoop szolgáltatások például az Ambari és a Hive, és csatlakozni külön-külön minden átjárócsomópont SSH használatával."
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
ms.openlocfilehash: e66ba67a36fc48d1762ba302d708e060489fdc71
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="d259a-105">A HDInsight-beli Hadoop-fürtök rendelkezésre állása és megbízhatósága</span><span class="sxs-lookup"><span data-stu-id="d259a-105">Availability and reliability of Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="d259a-106">A HDInsight-fürtök két központi csomópont a rendelkezésre állás és a Hadoop-szolgáltatás és a futó feladatok megbízhatóságának növelése érdekében adja meg.</span><span class="sxs-lookup"><span data-stu-id="d259a-106">HDInsight clusters provide two head nodes to increase the availability and reliability of Hadoop services and jobs running.</span></span>

<span data-ttu-id="d259a-107">Hadoop éri el a magas rendelkezésre állást és megbízhatóságot által egy fürt több csomópontja replikálása szolgáltatásaikat és adataikat.</span><span class="sxs-lookup"><span data-stu-id="d259a-107">Hadoop achieves high availability and reliability by replicating services and data across multiple nodes in a cluster.</span></span> <span data-ttu-id="d259a-108">Standard disztribúciók Hadoop azonban általában csak egy átjárócsomóponttal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="d259a-108">However standard distributions of Hadoop typically have only a single head node.</span></span> <span data-ttu-id="d259a-109">Bármely, a egy átjárócsomóponttal kimaradás a fürt működésének leállását okozhatja.</span><span class="sxs-lookup"><span data-stu-id="d259a-109">Any outage of the single head node can cause the cluster to stop working.</span></span> <span data-ttu-id="d259a-110">HDInsight Hadoop által rendelkezésre állásának és megbízhatóságának növelése érdekében két headnodes biztosít.</span><span class="sxs-lookup"><span data-stu-id="d259a-110">HDInsight provides two headnodes to improve Hadoop's availability and reliability.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d259a-111">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="d259a-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d259a-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d259a-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="availability-and-reliability-of-nodes"></a><span data-ttu-id="d259a-113">Rendelkezésre állásának és megbízhatóságának csomópontok</span><span class="sxs-lookup"><span data-stu-id="d259a-113">Availability and reliability of nodes</span></span>

<span data-ttu-id="d259a-114">HDInsight-fürtök csomópontjai Azure virtuális gépek segítségével lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d259a-114">Nodes in an HDInsight cluster are implemented using Azure Virtual Machines.</span></span> <span data-ttu-id="d259a-115">Az alábbi szakaszok ismertetik az egyes csomóponttípusok HDInsight használt.</span><span class="sxs-lookup"><span data-stu-id="d259a-115">The following sections discuss the individual node types used with HDInsight.</span></span> 

> [!NOTE]
> <span data-ttu-id="d259a-116">A fürt típusa nem minden csomópont-típusok használhatók.</span><span class="sxs-lookup"><span data-stu-id="d259a-116">Not all node types are used for a cluster type.</span></span> <span data-ttu-id="d259a-117">Például egy Hadoop-fürt típusa nem rendelkezik egyetlen Nimbus csomópontot.</span><span class="sxs-lookup"><span data-stu-id="d259a-117">For example, a Hadoop cluster type does not have any Nimbus nodes.</span></span> <span data-ttu-id="d259a-118">A HDInsight-fürttípusok által használt csomópontok további információkért lásd: a fürt típusok szakasza a [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d259a-118">For more information on nodes used by HDInsight cluster types, see the Cluster types section of the [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) document.</span></span>

### <a name="head-nodes"></a><span data-ttu-id="d259a-119">HEAD csomópontok</span><span class="sxs-lookup"><span data-stu-id="d259a-119">Head nodes</span></span>

<span data-ttu-id="d259a-120">Ahhoz, hogy a magas rendelkezésre állású Hadoop szolgáltatásokat, a HDInsight két átjárócsomópontokkal biztosít.</span><span class="sxs-lookup"><span data-stu-id="d259a-120">To ensure high availability of Hadoop services, HDInsight provides two head nodes.</span></span> <span data-ttu-id="d259a-121">Mindkét átjárócsomópontokkal egyidejűleg aktív és a HDInsight-fürtön belül futnak.</span><span class="sxs-lookup"><span data-stu-id="d259a-121">Both head nodes are active and running within the HDInsight cluster simultaneously.</span></span> <span data-ttu-id="d259a-122">Egyes szolgáltatások, például a HDFS vagy YARN, egy adott időpontban csak "active", egy központi csomóponton.</span><span class="sxs-lookup"><span data-stu-id="d259a-122">Some services, such as HDFS or YARN, are only 'active' on one head node at any given time.</span></span> <span data-ttu-id="d259a-123">Más szolgáltatásokon, például a hiveserver2-n vagy a Hive Metaadattárhoz aktívak mindkét központi csomóponton egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="d259a-123">Other services such as HiveServer2 or Hive MetaStore are active on both head nodes at the same time.</span></span>

<span data-ttu-id="d259a-124">HEAD csomópontok (és más csomópontok a Hdinsightban) rendelkezik egy numerikus érték, a csomópont állomásnév részeként.</span><span class="sxs-lookup"><span data-stu-id="d259a-124">Head nodes (and other nodes in HDInsight) have a numeric value as part of the hostname of the node.</span></span> <span data-ttu-id="d259a-125">Például `hn0-CLUSTERNAME` vagy `hn4-CLUSTERNAME`.</span><span class="sxs-lookup"><span data-stu-id="d259a-125">For example, `hn0-CLUSTERNAME` or `hn4-CLUSTERNAME`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d259a-126">Ne társítson a numerikus értéket-e a csomópont a elsődleges vagy másodlagos.</span><span class="sxs-lookup"><span data-stu-id="d259a-126">Do not associate the numeric value with whether a node is primary or secondary.</span></span> <span data-ttu-id="d259a-127">A numerikus értéke csak elérhető adjon egyedi nevet az egyes csomópontok.</span><span class="sxs-lookup"><span data-stu-id="d259a-127">The numeric value is only present to provide a unique name for each node.</span></span>

### <a name="nimbus-nodes"></a><span data-ttu-id="d259a-128">Nimbus-csomópontok</span><span class="sxs-lookup"><span data-stu-id="d259a-128">Nimbus Nodes</span></span>

<span data-ttu-id="d259a-129">Nimbus csomópont alatt futó Storm-fürtökkel érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d259a-129">Nimbus nodes are available with Storm clusters.</span></span> <span data-ttu-id="d259a-130">A Nimbus csomópontot ellátni hasonló a Hadoop JobTracker terjesztése és feldolgozási munkavégző csomópontokhoz átívelő figyelésére is alkalmas.</span><span class="sxs-lookup"><span data-stu-id="d259a-130">The Nimbus nodes provide similar functionality to the Hadoop JobTracker by distributing and monitoring processing across worker nodes.</span></span> <span data-ttu-id="d259a-131">A HDInsight két Nimbus csomóponttal rendelkezik a Storm-fürtök</span><span class="sxs-lookup"><span data-stu-id="d259a-131">HDInsight provides two Nimbus nodes for Storm clusters</span></span>

### <a name="zookeeper-nodes"></a><span data-ttu-id="d259a-132">Zookeeper csomópontok</span><span class="sxs-lookup"><span data-stu-id="d259a-132">Zookeeper nodes</span></span>

<span data-ttu-id="d259a-133">[ZooKeeper](http://zookeeper.apache.org/) csomópontok használt fő szolgáltatást az átjárócsomópontokkal vezető választás.</span><span class="sxs-lookup"><span data-stu-id="d259a-133">[ZooKeeper](http://zookeeper.apache.org/) nodes are used for leader election of master services on head nodes.</span></span> <span data-ttu-id="d259a-134">Akkor is használhatók biztosítását, hogy szolgáltatások, a adatcsomópontokat (munkavégző) és az átjárók tudja, hogy mely átjárócsomópont szolgáltatás főkulcsának aktív.</span><span class="sxs-lookup"><span data-stu-id="d259a-134">They are also used to insure that services, data (worker) nodes, and gateways know which head node a master service is active on.</span></span> <span data-ttu-id="d259a-135">Alapértelmezés szerint a HDInsight nyújt három ZooKeeper csomópontok.</span><span class="sxs-lookup"><span data-stu-id="d259a-135">By default, HDInsight provides three ZooKeeper nodes.</span></span>

### <a name="worker-nodes"></a><span data-ttu-id="d259a-136">Munkavégző csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="d259a-136">Worker nodes</span></span>

<span data-ttu-id="d259a-137">Munkavégző csomópontokhoz hajtsa végre a tényleges adatok elemzéséhez, amikor egy feladatot a fürt.</span><span class="sxs-lookup"><span data-stu-id="d259a-137">Worker nodes perform the actual data analysis when a job is submitted to the cluster.</span></span> <span data-ttu-id="d259a-138">Ha egy feldolgozó csomópont leáll, a feladat azt végző elküldve a worker egy másik csomópontra.</span><span class="sxs-lookup"><span data-stu-id="d259a-138">If a worker node fails, the task that it was performing is submitted to another worker node.</span></span> <span data-ttu-id="d259a-139">Alapértelmezés szerint a HDInsight létrehoz négy munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="d259a-139">By default, HDInsight creates four worker nodes.</span></span> <span data-ttu-id="d259a-140">Ez a szám a igényeinek alatt és a fürt létrehozása után módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d259a-140">You can change this number to suit your needs both during and after cluster creation.</span></span>

### <a name="edge-node"></a><span data-ttu-id="d259a-141">Élcsomópont</span><span class="sxs-lookup"><span data-stu-id="d259a-141">Edge node</span></span>

<span data-ttu-id="d259a-142">Egy élcsomópontot aktívan nem vesz részt a fürtön belül adatelemzés.</span><span class="sxs-lookup"><span data-stu-id="d259a-142">An edge node does not actively participate in data analysis within the cluster.</span></span> <span data-ttu-id="d259a-143">Amikor olyan Hadoop fejlesztők vagy adatszakértőkön szolgál.</span><span class="sxs-lookup"><span data-stu-id="d259a-143">It is used by developers or data scientists when working with Hadoop.</span></span> <span data-ttu-id="d259a-144">Élcsomópont él, mint a fürt többi csomópontjának azonos Azure virtuális hálózatban, és közvetlenül hozzáférhet az összes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="d259a-144">The edge node lives in the same Azure Virtual Network as the other nodes in the cluster, and can directly access all other nodes.</span></span> <span data-ttu-id="d259a-145">Élcsomópont kritikus Hadoop-szolgáltatás vagy a feladatok távol erőforrások anélkül is használható.</span><span class="sxs-lookup"><span data-stu-id="d259a-145">The edge node can be used without taking resources away from critical Hadoop services or analysis jobs.</span></span>

<span data-ttu-id="d259a-146">Az R Server on HDInsight jelenleg az egyetlen fürt típus, amely alapértelmezés szerint egy élcsomópontot biztosít.</span><span class="sxs-lookup"><span data-stu-id="d259a-146">Currently, R Server on HDInsight is the only cluster type that provides an edge node by default.</span></span> <span data-ttu-id="d259a-147">Az R Server on HDInsight, az élcsomópont használatos teszt R-kód helyileg a csomóponton a fürt elosztott feldolgozásra való továbbítás előtt.</span><span class="sxs-lookup"><span data-stu-id="d259a-147">For R Server on HDInsight, the edge node is used test R code locally on the node before submitting it to the cluster for distributed processing.</span></span>

<span data-ttu-id="d259a-148">Egy élcsomópontot fürt típusa nem R Server használatáról információkért lásd: a [peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d259a-148">For information on using an edge node with cluster types other than R Server, see the [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) document.</span></span>

## <a name="accessing-the-nodes"></a><span data-ttu-id="d259a-149">A csomópontok elérése</span><span class="sxs-lookup"><span data-stu-id="d259a-149">Accessing the nodes</span></span>

<span data-ttu-id="d259a-150">A fürt az interneten keresztül való hozzáférés a nyilvános átjárón keresztül valósul meg.</span><span class="sxs-lookup"><span data-stu-id="d259a-150">Access to the cluster over the internet is provided through a public gateway.</span></span> <span data-ttu-id="d259a-151">Hozzáférés korlátozódik az átjárócsomópontokkal csatlakozik, és (ha van ilyen) a peremhálózati csomópont.</span><span class="sxs-lookup"><span data-stu-id="d259a-151">Access is limited to connecting to the head nodes and (if one exists) the edge node.</span></span> <span data-ttu-id="d259a-152">A head csomópontokon futó szolgáltatásokhoz való hozzáférés nem azzal, hogy több átjárócsomópontokkal történik.</span><span class="sxs-lookup"><span data-stu-id="d259a-152">Access to services running on the head nodes is not effected by having multiple head nodes.</span></span> <span data-ttu-id="d259a-153">A nyilvános átjáró irányítja a kérelmeket az átjárócsomóponthoz, amelyen a kért szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d259a-153">The public gateway routes requests to the head node that hosts the requested service.</span></span> <span data-ttu-id="d259a-154">Például Ambari kiszolgálóvá a másodlagos átjárócsomópont, ha az átjáró irányítja a bejövő kéréseket az Ambari ahhoz a csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="d259a-154">For example, if Ambari is currently hosted on the secondary head node, the gateway routes incoming requests for Ambari to that node.</span></span>

<span data-ttu-id="d259a-155">Hozzáférés a nyilvános átjárón keresztül port 443-as (HTTPS), a 22-es és 23 korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="d259a-155">Access over the public gateway is limited to port 443 (HTTPS), 22, and 23.</span></span>

* <span data-ttu-id="d259a-156">Port __443-as__ Ambari és egyéb webes felhasználói felületén vagy a head csomópontokon futó REST API-k elérésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="d259a-156">Port __443__ is used to access Ambari and other web UI or REST APIs hosted on the head nodes.</span></span>

* <span data-ttu-id="d259a-157">Port __22__ elérni az elsődleges átjárócsomópont vagy élcsomópont az SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="d259a-157">Port __22__ is used to access the primary head node or edge node with SSH.</span></span>

* <span data-ttu-id="d259a-158">Port __23__ az SSH a másodlagos átjárócsomópont eléréséhez használt.</span><span class="sxs-lookup"><span data-stu-id="d259a-158">Port __23__ is used to access the secondary head node with SSH.</span></span> <span data-ttu-id="d259a-159">Például `ssh username@mycluster-ssh.azurehdinsight.net` nevű fürt elsődleges átjárócsomópontjához csatlakozik **sajátfürt**.</span><span class="sxs-lookup"><span data-stu-id="d259a-159">For example, `ssh username@mycluster-ssh.azurehdinsight.net` connects to the primary head node of the cluster named **mycluster**.</span></span>

<span data-ttu-id="d259a-160">További információ az SSH használatával, tekintse meg a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d259a-160">For more information on using SSH, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

### <a name="internal-fully-qualified-domain-names-fqdn"></a><span data-ttu-id="d259a-161">Belső teljes tartománynevek (FQDN)</span><span class="sxs-lookup"><span data-stu-id="d259a-161">Internal fully qualified domain names (FQDN)</span></span>

<span data-ttu-id="d259a-162">HDInsight-fürtök csomópontjai rendelkeznek, egy belső IP-cím és a teljes tartománynév, csak elérhető a fürtből.</span><span class="sxs-lookup"><span data-stu-id="d259a-162">Nodes in an HDInsight cluster have an internal IP address and FQDN that can only be accessed from the cluster.</span></span> <span data-ttu-id="d259a-163">A belső FQDN vagy IP-címet használ a fürt szolgáltatásai elérésekor a Ambari ellenőrzése a a szolgáltatás eléréséhez használt IP vagy FQDN Formátumban kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d259a-163">When accessing services on the cluster using the internal FQDN or IP address, you should use Ambari to verify the IP or FQDN to use when accessing the service.</span></span>

<span data-ttu-id="d259a-164">Például az Oozie-szolgáltatás csak futtathatja egy átjárócsomópont, és használja a `oozie` SSH-munkamenetet a parancshoz szükséges, a szolgáltatás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="d259a-164">For example, the Oozie service can only run on one head node, and using the `oozie` command from an SSH session requires the URL to the service.</span></span> <span data-ttu-id="d259a-165">Az URL-cím a következő paranccsal olvasható be a Ambari:</span><span class="sxs-lookup"><span data-stu-id="d259a-165">This URL can be retrieved from Ambari by using the following command:</span></span>

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

<span data-ttu-id="d259a-166">Ez a parancs értéket ad vissza a következő parancsot, amely tartalmazza a belső URL-cím használata hasonlít a `oozie` parancs:</span><span class="sxs-lookup"><span data-stu-id="d259a-166">This command returns a value similar to the following command, which contains the internal URL to use with the `oozie` command:</span></span>

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

<span data-ttu-id="d259a-167">Az Ambari REST API használatával további információkért lásd: [figyelése és az Ambari REST API használatával kezelése HDInsight](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="d259a-167">For more information on working with the Ambari REST API, see [Monitor and Manage HDInsight using the Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

### <a name="accessing-other-node-types"></a><span data-ttu-id="d259a-168">Egyéb elérése</span><span class="sxs-lookup"><span data-stu-id="d259a-168">Accessing other node types</span></span>

<span data-ttu-id="d259a-169">Csatlakozhat a csomópontot, amely nem érhető el közvetlenül az interneten keresztül az alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="d259a-169">You can connect to nodes that are not directly accessible over the internet by using the following methods:</span></span>

* <span data-ttu-id="d259a-170">**SSH**: Miután csatlakozott egy átjárócsomóponttal SSH használatával, majd használhatja SSH az átjárócsomópont kapcsolódni a fürt többi tagján.</span><span class="sxs-lookup"><span data-stu-id="d259a-170">**SSH**: Once connected to a head node using SSH, you can then use SSH from the head node to connect to other nodes in the cluster.</span></span> <span data-ttu-id="d259a-171">További információ: [SSH használata a HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d259a-171">For more information, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

* <span data-ttu-id="d259a-172">**SSH-alagút**: egy webszolgáltatás-bővítmény egy olyan csomópontot, amely nem érhető el az internet eléréséhez szükséges, ha az SSH-alagút kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d259a-172">**SSH Tunnel**: If you need to access a web service hosted on one of the nodes that is not exposed to the internet, you must use an SSH tunnel.</span></span> <span data-ttu-id="d259a-173">További információkért lásd: a [használhat SSH-alagutat a hdinsight eszközzel](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d259a-173">For more information, see the [Use an SSH tunnel with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

* <span data-ttu-id="d259a-174">**Azure-beli virtuális hálózat**: a HDInsight-fürt része egy Azure virtuális hálózatra, ha minden erőforrást azonos virtuális hálózaton közvetlenül érhetik el a fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="d259a-174">**Azure Virtual Network**: If your HDInsight cluster is part of an Azure Virtual Network, any resource on the same Virtual Network can directly access all nodes in the cluster.</span></span> <span data-ttu-id="d259a-175">További információkért lásd: a [kiterjesztése HDInsight az Azure Virtual Network a](hdinsight-extend-hadoop-virtual-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d259a-175">For more information, see the [Extend HDInsight using Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

## <a name="how-to-check-on-a-service-status"></a><span data-ttu-id="d259a-176">Az a szolgáltatás állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d259a-176">How to check on a service status</span></span>

<span data-ttu-id="d259a-177">Az átjárócsomópontokkal futó szolgáltatások állapotának ellenőrzéséhez használja az Ambari webes felhasználói felületén vagy az Ambari REST API-t.</span><span class="sxs-lookup"><span data-stu-id="d259a-177">To check the status of services that run on the head nodes, use the Ambari Web UI or the Ambari REST API.</span></span>

### <a name="ambari-web-ui"></a><span data-ttu-id="d259a-178">Ambari webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="d259a-178">Ambari Web UI</span></span>

<span data-ttu-id="d259a-179">Az Ambari webes felhasználói felületén, https://CLUSTERNAME.azurehdinsight.net megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="d259a-179">The Ambari Web UI is viewable at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="d259a-180">Cserélje le a **CLUSTERNAME** elemet a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="d259a-180">Replace **CLUSTERNAME** with the name of your cluster.</span></span> <span data-ttu-id="d259a-181">Ha a rendszer kéri, adja meg a HTTP-felhasználó hitelesítő adatait a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="d259a-181">If prompted, enter the HTTP user credentials for your cluster.</span></span> <span data-ttu-id="d259a-182">Az alapértelmezett HTTP felhasználónév **admin** , a jelszó pedig a fürt létrehozásakor megadott jelszó.</span><span class="sxs-lookup"><span data-stu-id="d259a-182">The default HTTP user name is **admin** and the password is the password you entered when creating the cluster.</span></span>

<span data-ttu-id="d259a-183">Amikor megérkezik az Ambari oldalon, a telepített szolgáltatások szerepelnek a lap bal oldalon található.</span><span class="sxs-lookup"><span data-stu-id="d259a-183">When you arrive on the Ambari page, the installed services are listed on the left of the page.</span></span>

![Telepített szolgáltatások](./media/hdinsight-high-availability-linux/services.png)

<span data-ttu-id="d259a-185">A szolgáltatás állapotának mellett megjelenő ikonok több van.</span><span class="sxs-lookup"><span data-stu-id="d259a-185">There are a series of icons that may appear next to a service to indicate status.</span></span> <span data-ttu-id="d259a-186">Az a szolgáltatással kapcsolatos riasztások használatával tekinthetők a **riasztások** az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="d259a-186">Any alerts related to a service can be viewed using the **Alerts** link at the top of the page.</span></span> <span data-ttu-id="d259a-187">További információk megjelenítéséhez a minden egyes szolgáltatás választhatja ki.</span><span class="sxs-lookup"><span data-stu-id="d259a-187">You can select each service to view more information on it.</span></span>

<span data-ttu-id="d259a-188">A szolgáltatás lapján információt nyújt az egyes szolgáltatások konfigurációja és állapota, amíg azt nem szolgál információval melyik központi csomóponton a szolgáltatás fut a.</span><span class="sxs-lookup"><span data-stu-id="d259a-188">While the service page provides information on the status and configuration of each service, it does not provide information on which head node the service is running on.</span></span> <span data-ttu-id="d259a-189">Ez az információ megtekintéséhez használja a **állomások** az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="d259a-189">To view this information, use the **Hosts** link at the top of the page.</span></span> <span data-ttu-id="d259a-190">Ez a lap megjeleníti a fürtben, beleértve az átjárócsomópontokkal állomások.</span><span class="sxs-lookup"><span data-stu-id="d259a-190">This page displays hosts within the cluster, including the head nodes.</span></span>

![állomások listájához](./media/hdinsight-high-availability-linux/hosts.png)

<span data-ttu-id="d259a-192">A hivatkozás egyik az átjárócsomópontokkal látható értesítések valamelyikének kiválasztásakor a szolgáltatások és a leállt csomóponton összetevőket.</span><span class="sxs-lookup"><span data-stu-id="d259a-192">Selecting the link for one of the head nodes displays the services and components running on that node.</span></span>

![Összetevő-állapot](./media/hdinsight-high-availability-linux/nodeservices.png)

<span data-ttu-id="d259a-194">További információ az Ambari használatával, lásd: [figyelése és kezelése az Ambari webes felhasználói felület használata a HDInsight](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="d259a-194">For more information on using Ambari, see [Monitor and manage HDInsight using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

### <a name="ambari-rest-api"></a><span data-ttu-id="d259a-195">Ambari REST API-n</span><span class="sxs-lookup"><span data-stu-id="d259a-195">Ambari REST API</span></span>

<span data-ttu-id="d259a-196">Az Ambari REST API-t az interneten keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="d259a-196">The Ambari REST API is available over the internet.</span></span> <span data-ttu-id="d259a-197">A HDInsight nyilvános átjáró az átjárócsomóponthoz, amely a REST API-t tartalmazó útválasztási kérelmeket kezeli.</span><span class="sxs-lookup"><span data-stu-id="d259a-197">The HDInsight public gateway handles routing requests to the head node that is currently hosting the REST API.</span></span>

<span data-ttu-id="d259a-198">A következő paranccsal ellenőrizze az Ambari REST API-n keresztül egy szolgáltatás állapotát:</span><span class="sxs-lookup"><span data-stu-id="d259a-198">You can use the following command to check the state of a service through the Ambari REST API:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* <span data-ttu-id="d259a-199">Cserélje le **jelszó** az a HTTP (rendszergazda) felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="d259a-199">Replace **PASSWORD** with the HTTP user (admin) account password.</span></span>
* <span data-ttu-id="d259a-200">Cserélje le a **CLUSTERNAME** elemet a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="d259a-200">Replace **CLUSTERNAME** with the name of the cluster.</span></span>
* <span data-ttu-id="d259a-201">Cserélje le **szolgáltatásnév** tekintse meg a kívánt szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="d259a-201">Replace **SERVICENAME** with the name of the service you want to check the status of.</span></span>

<span data-ttu-id="d259a-202">Például tekintse meg a **HDFS** szolgáltatás nevű fürt **sajátfürt**, a jelszó **jelszó**, akkor használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d259a-202">For example, to check the status of the **HDFS** service on a cluster named **mycluster**, with a password of **password**, you would use the following command:</span></span>

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

<span data-ttu-id="d259a-203">A válasz a következő JSON hasonlít:</span><span class="sxs-lookup"><span data-stu-id="d259a-203">The response is similar to the following JSON:</span></span>

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

<span data-ttu-id="d259a-204">Az URL-cím közli velünk, hogy a szolgáltatás jelenleg fut egy átjárócsomópont nevű **hn0-CLUSTERNAME**.</span><span class="sxs-lookup"><span data-stu-id="d259a-204">The URL tells us that the service is currently running on a head node named **hn0-CLUSTERNAME**.</span></span>

<span data-ttu-id="d259a-205">Az állapot közli velünk, hogy a szolgáltatás jelenleg fut, vagy **elindítva**.</span><span class="sxs-lookup"><span data-stu-id="d259a-205">The state tells us that the service is currently running, or **STARTED**.</span></span>

<span data-ttu-id="d259a-206">Ha nem tudja, milyen szolgáltatások telepítve vannak a fürtön, a következő parancs használatával kérdezheti le:</span><span class="sxs-lookup"><span data-stu-id="d259a-206">If you do not know what services are installed on the cluster, you can use the following command to retrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

<span data-ttu-id="d259a-207">Az Ambari REST API használatával további információkért lásd: [figyelése és az Ambari REST API használatával kezelése HDInsight](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="d259a-207">For more information on working with the Ambari REST API, see [Monitor and Manage HDInsight using the Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

#### <a name="service-components"></a><span data-ttu-id="d259a-208">Szolgáltatás-összetevők</span><span class="sxs-lookup"><span data-stu-id="d259a-208">Service components</span></span>

<span data-ttu-id="d259a-209">Szolgáltatások tartalmazhatnak, tekintse meg a külön-külön kívánt összetevőket.</span><span class="sxs-lookup"><span data-stu-id="d259a-209">Services may contain components that you wish to check the status of individually.</span></span> <span data-ttu-id="d259a-210">Például a HDFS a NameNode összetevőt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d259a-210">For example, HDFS contains the NameNode component.</span></span> <span data-ttu-id="d259a-211">A parancs információk megtekintéséhez kattintson egy összetevő a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="d259a-211">To view information on a component, the command would be:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

<span data-ttu-id="d259a-212">Ha nem tudja, milyen összetevők szolgáltatás által biztosított, a következő parancs használatával kérdezheti le:</span><span class="sxs-lookup"><span data-stu-id="d259a-212">If you do not know what components are provided by a service, you can use the following command to retrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-to-access-log-files-on-the-head-nodes"></a><span data-ttu-id="d259a-213">Hogyan érhetők el az átjárócsomópontokkal naplófájlok</span><span class="sxs-lookup"><span data-stu-id="d259a-213">How to access log files on the head nodes</span></span>

### <a name="ssh"></a><span data-ttu-id="d259a-214">SSH</span><span class="sxs-lookup"><span data-stu-id="d259a-214">SSH</span></span>

<span data-ttu-id="d259a-215">Ha egy SSH-n keresztül. átjárócsomópontjához csatlakozik, naplófájlok alatt található **/var/log**.</span><span class="sxs-lookup"><span data-stu-id="d259a-215">While connected to a head node through SSH, log files can be found under **/var/log**.</span></span> <span data-ttu-id="d259a-216">Például **/var/log/hadoop-yarn/yarn** YARN naplók tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="d259a-216">For example, **/var/log/hadoop-yarn/yarn** contain logs for YARN.</span></span>

<span data-ttu-id="d259a-217">Minden átjárócsomópont egyedi naplóbejegyzések, van így ellenőrizni kell mind a naplókat.</span><span class="sxs-lookup"><span data-stu-id="d259a-217">Each head node can have unique log entries, so you should check the logs on both.</span></span>

### <a name="sftp"></a><span data-ttu-id="d259a-218">SFTP</span><span class="sxs-lookup"><span data-stu-id="d259a-218">SFTP</span></span>

<span data-ttu-id="d259a-219">Az SSH File Transfer Protocol vagy biztonságos fájl Transfer Protocol (SFTP) átjárócsomópontjához csatlakozik, és töltse le a naplófájlok közvetlenül is.</span><span class="sxs-lookup"><span data-stu-id="d259a-219">You can also connect to the head node using the SSH File Transfer Protocol or Secure File Transfer Protocol (SFTP), and download the log files directly.</span></span>

<span data-ttu-id="d259a-220">Hasonló egy SSH-ügyfélprogrammal, amikor a fürthöz csatlakozik biztosítania kell az SSH-felhasználói fiók nevét és az SSH-cím, a fürt.</span><span class="sxs-lookup"><span data-stu-id="d259a-220">Similar to using an SSH client, when connecting to the cluster you must provide the SSH user account name and the SSH address of the cluster.</span></span> <span data-ttu-id="d259a-221">Például: `sftp username@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="d259a-221">For example, `sftp username@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="d259a-222">Adja meg a fiók, amikor a rendszer kéri a jelszót, vagy adja meg a nyilvános kulcs használatával a `-i` paraméter.</span><span class="sxs-lookup"><span data-stu-id="d259a-222">Provide the password for the account when prompted, or provide a public key using the `-i` parameter.</span></span>

<span data-ttu-id="d259a-223">Miután csatlakozott, lehetősége lesz a `sftp>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="d259a-223">Once connected, you are presented with a `sftp>` prompt.</span></span> <span data-ttu-id="d259a-224">A parancssorból módosíthatja, könyvtárak, fájlok feltöltését és letöltését.</span><span class="sxs-lookup"><span data-stu-id="d259a-224">From this prompt, you can change directories, upload, and download files.</span></span> <span data-ttu-id="d259a-225">Például a következő parancsok lépjen a **/var/log/hadoop/hdfs** directory és az összes olyan fájl a könyvtárban, majd letöltése.</span><span class="sxs-lookup"><span data-stu-id="d259a-225">For example, the following commands change directories to the **/var/log/hadoop/hdfs** directory and then download all files in the directory.</span></span>

    cd /var/log/hadoop/hdfs
    get *

<span data-ttu-id="d259a-226">Elérhető parancsok listáját, írja be a `help` , a `sftp>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="d259a-226">For a list of available commands, enter `help` at the `sftp>` prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="d259a-227">Van grafikus felületek, amelyek lehetővé teszik a fájlrendszer esetén SFTP használatával jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="d259a-227">There are also graphical interfaces that allow you to visualize the file system when connected using SFTP.</span></span> <span data-ttu-id="d259a-228">Például [MobaXTerm](http://mobaxterm.mobatek.net/) lehetővé teszi, hogy keresse meg a fájlrendszer hasonló a Windows Explorer-felületet segítségével.</span><span class="sxs-lookup"><span data-stu-id="d259a-228">For example, [MobaXTerm](http://mobaxterm.mobatek.net/) allows you to browse the file system using an interface similar to Windows Explorer.</span></span>

### <a name="ambari"></a><span data-ttu-id="d259a-229">Ambari</span><span class="sxs-lookup"><span data-stu-id="d259a-229">Ambari</span></span>

> [!NOTE]
> <span data-ttu-id="d259a-230">Ambari naplófájlokat eléréséhez az SSH-alagút kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d259a-230">To access log files using Ambari, you must use an SSH tunnel.</span></span> <span data-ttu-id="d259a-231">A webes felületek, az egyes szolgáltatások nem érhetők el nyilvánosan az interneten.</span><span class="sxs-lookup"><span data-stu-id="d259a-231">The web interfaces for the individual services are not exposed publicly on the Internet.</span></span> <span data-ttu-id="d259a-232">Az SSH-alagúton keresztül információkért lásd: a [használata SSH Tunneling](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d259a-232">For information on using an SSH tunnel, see the [Use SSH Tunneling](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

<span data-ttu-id="d259a-233">Az Ambari webes felhasználói felületén jelölje ki a kívánt naplók megtekintése (például YARN) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d259a-233">From the Ambari Web UI, select the service you wish to view logs for (for example, YARN).</span></span> <span data-ttu-id="d259a-234">Ezután **Gyorshivatkozások** a naplók megtekintéséhez melyik központi csomópont kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="d259a-234">Then use **Quick Links** to select which head node to view the logs for.</span></span>

![Gyorshivatkozások használ a naplók megtekintéséhez](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-to-configure-the-node-size"></a><span data-ttu-id="d259a-236">A csomópont méretének beállítása</span><span class="sxs-lookup"><span data-stu-id="d259a-236">How to configure the node size</span></span>

<span data-ttu-id="d259a-237">A csomópont mérete csak akkor jelölhető ki, fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="d259a-237">The size of a node can only be selected during cluster creation.</span></span> <span data-ttu-id="d259a-238">A HDInsight a különböző elérhető Virtuálisgép-méretek listáját megtalálhatja a [árképzést ismertető oldalra HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="d259a-238">You can find a list of the different VM sizes available for HDInsight on the [HDInsight pricing page](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="d259a-239">A fürt létrehozásakor megadhatja a csomópontok méretét.</span><span class="sxs-lookup"><span data-stu-id="d259a-239">When creating a cluster, you can specify the size of the nodes.</span></span> <span data-ttu-id="d259a-240">Az alábbi információk megadása a méret használatával nyújt útmutatást a [Azure-portálon][preview-portal], [Azure PowerShell][azure-powershell], és a [Azure CLI][azure-cli]:</span><span class="sxs-lookup"><span data-stu-id="d259a-240">The following information provides guidance on how to specify the size using the [Azure portal][preview-portal], [Azure PowerShell][azure-powershell], and the [Azure CLI][azure-cli]:</span></span>

* <span data-ttu-id="d259a-241">**Azure-portálon**: hoz létre egy fürtöt, beállíthatja a csomópontok a fürt által használt mérete:</span><span class="sxs-lookup"><span data-stu-id="d259a-241">**Azure portal**: When creating a cluster, you can set the size of the nodes used by the cluster:</span></span>

    ![A méret kiválasztása a fürt létrehozása varázsló képe](./media/hdinsight-high-availability-linux/headnodesize.png)

* <span data-ttu-id="d259a-243">**Az Azure CLI**: használata esetén a `azure hdinsight cluster create` parancs használatával beállíthatja a head munkavégző és ZooKeeper csomópontok méretét a `--headNodeSize`, `--workerNodeSize`, és `--zookeeperNodeSize` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="d259a-243">**Azure CLI**: When using the `azure hdinsight cluster create` command, you can set the size of the head, worker, and ZooKeeper nodes by using the `--headNodeSize`, `--workerNodeSize`, and `--zookeeperNodeSize` parameters.</span></span>

* <span data-ttu-id="d259a-244">**Az Azure PowerShell**: használata esetén a `New-AzureRmHDInsightCluster` parancsmag, beállíthatja a head munkavégző és ZooKeeper csomópontok méretének használatával a `-HeadNodeVMSize`, `-WorkerNodeSize`, és `-ZookeeperNodeSize` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="d259a-244">**Azure PowerShell**: When using the `New-AzureRmHDInsightCluster` cmdlet, you can set the size of the head, worker, and ZooKeeper nodes by using the `-HeadNodeVMSize`, `-WorkerNodeSize`, and `-ZookeeperNodeSize` parameters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d259a-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d259a-245">Next steps</span></span>

<span data-ttu-id="d259a-246">Az alábbi hivatkozások segítségével további ebben a dokumentumban említett szempontot.</span><span class="sxs-lookup"><span data-stu-id="d259a-246">Use the following links to learn more about things mentioned in this document.</span></span>

* [<span data-ttu-id="d259a-247">Ambari REST-referencia</span><span class="sxs-lookup"><span data-stu-id="d259a-247">Ambari REST Reference</span></span>](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [<span data-ttu-id="d259a-248">Telepítse és konfigurálja az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="d259a-248">Install and configure the Azure CLI</span></span>](../cli-install-nodejs.md)
* [<span data-ttu-id="d259a-249">Telepítse és konfigurálja az Azure PowerShellt</span><span class="sxs-lookup"><span data-stu-id="d259a-249">Install and configure Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="d259a-250">HDInsight a Ambari kezelése</span><span class="sxs-lookup"><span data-stu-id="d259a-250">Manage HDInsight using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)
* [<span data-ttu-id="d259a-251">Linux-alapú HDInsight-fürtök kiépítése</span><span class="sxs-lookup"><span data-stu-id="d259a-251">Provision Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
