---
title: "Hadoop-fürtök aaaMonitor a Hdinsightban az Ambari API - hello Azure |} Microsoft Docs"
description: "Használjon hello Apache Ambari API-k létrehozása, kezelése és figyelése a Hadoop-fürtök. Intuitív operátori eszközök és API-k elrejtése hello Hadoop összetettségét."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
editor: cgronlun
manager: jhubbard
ms.assetid: 052135b3-d497-4acc-92ff-71cee49356ff
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: d61a8aae5ddfcd7d44f2e4cc899e0a4da5e5fdcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-hadoop-clusters-in-hdinsight-using-hello-ambari-api"></a><span data-ttu-id="b03cb-104">Hello Ambari API segítségével hdinsight Hadoop-fürtök figyelése</span><span class="sxs-lookup"><span data-stu-id="b03cb-104">Monitor Hadoop clusters in HDInsight using hello Ambari API</span></span>
<span data-ttu-id="b03cb-105">Ismerje meg, hogyan toomonitor HDInsight clusters Ambari API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="b03cb-105">Learn how toomonitor HDInsight clusters by using Ambari APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="b03cb-106">a cikkben szereplő információkat hello elsősorban a Windows-alapú HDInsight-fürtök, amely hello Ambari REST API-t egy csak olvasható verziója van.</span><span class="sxs-lookup"><span data-stu-id="b03cb-106">hello information in this article is primarily for Windows-based HDInsight clusters, which provide a read-only version of hello Ambari REST API.</span></span> <span data-ttu-id="b03cb-107">Linux-alapú fürtök esetében lásd: [kezelése Hadoop fürtök az Ambari segítségével](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="b03cb-107">For Linux-based clusters, see [Manage Hadoop clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
> 
> 

## <a name="what-is-ambari"></a><span data-ttu-id="b03cb-108">Mi az az Ambari?</span><span class="sxs-lookup"><span data-stu-id="b03cb-108">What is Ambari?</span></span>
<span data-ttu-id="b03cb-109">[Apache Ambari] [ ambari-home] kiépítésére, kezelésére és Apache Hadoop-fürtök ellenőrzésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="b03cb-109">[Apache Ambari][ambari-home] is used for provisioning, managing, and monitoring Apache Hadoop clusters.</span></span> <span data-ttu-id="b03cb-110">Operátori eszközök intuitív gyűjteményét és egy robusztus API-k, amelyek elfedik hello Hadoop összetettségét, és a fürtök működését hello egyszerűsítése tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b03cb-110">It includes an intuitive collection of operator tools and a robust set of APIs that hide hello complexity of Hadoop, simplifying hello operation of clusters.</span></span> <span data-ttu-id="b03cb-111">API-k hello kapcsolatos további információkért lásd: [Ambari API-referencia][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="b03cb-111">For more information about hello APIs, see [Ambari API Reference][ambari-api-reference].</span></span> 

<span data-ttu-id="b03cb-112">A HDInsight jelenleg csak hello Ambari a figyelési funkció támogatja.</span><span class="sxs-lookup"><span data-stu-id="b03cb-112">HDInsight currently supports only hello Ambari monitoring feature.</span></span> <span data-ttu-id="b03cb-113">Ambari API 1.0 HDInsight 3.0-s és a 2.1-es verziójú fürtöket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b03cb-113">Ambari API 1.0 is supported by HDInsight version 3.0 and 2.1 clusters.</span></span> <span data-ttu-id="b03cb-114">Ez a cikk ismerteti a HDInsight-fürtökön 3.1 és 2.1 elérése során Ambari API-k.</span><span class="sxs-lookup"><span data-stu-id="b03cb-114">This article covers accessing Ambari APIs on HDInsight version 3.1 and 2.1 clusters.</span></span> <span data-ttu-id="b03cb-115">hello hello két fő különbség az, hogy néhány hello összetevőjét megváltoztak hello bevezetése új képességeket nyújtanak (például előzmények feladatkiszolgáló hello).</span><span class="sxs-lookup"><span data-stu-id="b03cb-115">hello key difference between hello two is that some of hello components have changed with hello introduction of new capabilities (such as hello Job History Server).</span></span> 

<span data-ttu-id="b03cb-116">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="b03cb-116">**Prerequisites**</span></span>

<span data-ttu-id="b03cb-117">Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="b03cb-117">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="b03cb-118">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="b03cb-118">**A workstation with Azure PowerShell**.</span></span>
* <span data-ttu-id="b03cb-119">(Választható) [cURL][curl].</span><span class="sxs-lookup"><span data-stu-id="b03cb-119">(Optional) [cURL][curl].</span></span> <span data-ttu-id="b03cb-120">tooinstall, lásd: [kiadások és letöltéseket tartalmazó oldalon cURL][curl-download].</span><span class="sxs-lookup"><span data-stu-id="b03cb-120">tooinstall it, see [cURL Releases and Downloads][curl-download].</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="b03cb-121">Ha használja a hello cURL-parancsot a Windows rendszerben egyetlen idézőjelek hello beállítás értékek helyett használjon idézőjelek közé.</span><span class="sxs-lookup"><span data-stu-id="b03cb-121">When use hello cURL command in Windows, use double-quotation marks instead of single-quotation marks for hello option values.</span></span>
  > 
  > 
* <span data-ttu-id="b03cb-122">**Egy Azure HDInsight fürt**.</span><span class="sxs-lookup"><span data-stu-id="b03cb-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="b03cb-123">Fürtök kiépítése kapcsolatos útmutatásért lásd: [használatának megkezdésében a HDInsight] [ hdinsight-get-started] vagy [Provision HDInsight clusters][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="b03cb-123">For instructions about cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="b03cb-124">A következő adatok toogo hello oktatóanyag hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="b03cb-124">You need hello following data toogo through hello tutorial:</span></span>
  
  | <span data-ttu-id="b03cb-125">Fürt tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b03cb-125">Cluster property</span></span> | <span data-ttu-id="b03cb-126">Az Azure PowerShell változó neve</span><span class="sxs-lookup"><span data-stu-id="b03cb-126">Azure PowerShell variable name</span></span> | <span data-ttu-id="b03cb-127">Érték</span><span class="sxs-lookup"><span data-stu-id="b03cb-127">Value</span></span> | <span data-ttu-id="b03cb-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="b03cb-128">Description</span></span> |
  | --- | --- | --- | --- |
  |   <span data-ttu-id="b03cb-129">A HDInsight-fürt neve</span><span class="sxs-lookup"><span data-stu-id="b03cb-129">HDInsight cluster name</span></span> |<span data-ttu-id="b03cb-130">$clusterName</span><span class="sxs-lookup"><span data-stu-id="b03cb-130">$clusterName</span></span> | |<span data-ttu-id="b03cb-131">a HDInsight-fürt hello neve.</span><span class="sxs-lookup"><span data-stu-id="b03cb-131">hello name of your HDInsight cluster.</span></span> |
  |   <span data-ttu-id="b03cb-132">Fürt felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b03cb-132">Cluster username</span></span> |<span data-ttu-id="b03cb-133">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="b03cb-133">$clusterUsername</span></span> | |<span data-ttu-id="b03cb-134">A megadott fürtnév felhasználói hello fürt létrehozásának.</span><span class="sxs-lookup"><span data-stu-id="b03cb-134">Cluster user name specified when hello cluster was created.</span></span> |
  |   <span data-ttu-id="b03cb-135">Fürt jelszó</span><span class="sxs-lookup"><span data-stu-id="b03cb-135">Cluster password</span></span> |<span data-ttu-id="b03cb-136">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="b03cb-136">$clusterPassword</span></span> | |<span data-ttu-id="b03cb-137">Fürt felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b03cb-137">Cluster user password.</span></span> |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a><span data-ttu-id="b03cb-138">Feladat gyors végrehajtásának megkönnyítésére</span><span class="sxs-lookup"><span data-stu-id="b03cb-138">Jump-start</span></span>
<span data-ttu-id="b03cb-139">Számos módon toouse Ambari toomonitor a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="b03cb-139">There are several ways toouse Ambari toomonitor HDInsight clusters.</span></span>

<span data-ttu-id="b03cb-140">**Azure PowerShell használata**</span><span class="sxs-lookup"><span data-stu-id="b03cb-140">**Use Azure PowerShell**</span></span>

<span data-ttu-id="b03cb-141">Azure PowerShell-parancsfájl a következő hello lekérdezi, hello MapReduce feladatot követő *HDInsight 3.5 fürtben.*</span><span class="sxs-lookup"><span data-stu-id="b03cb-141">hello following Azure PowerShell script gets hello MapReduce job tracker information *in an HDInsight 3.5 cluster.*</span></span>  <span data-ttu-id="b03cb-142">hello fő különbség, hogy azt lekéréses ezen adatok hello YARN szerviz (hanem MapReduce).</span><span class="sxs-lookup"><span data-stu-id="b03cb-142">hello key difference is that we pull these details from hello YARN service (rather than MapReduce).</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

<span data-ttu-id="b03cb-143">a következő PowerShell-parancsfájl hello lekérdezi, hello MapReduce feladatot követő *HDInsight 2.1-fürtben lévő*:</span><span class="sxs-lookup"><span data-stu-id="b03cb-143">hello following PowerShell script gets hello MapReduce job tracker information *in an HDInsight 2.1 cluster*:</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

<span data-ttu-id="b03cb-144">hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="b03cb-144">hello output is:</span></span>

![Jobtracker kimeneti][img-jobtracker-output]

<span data-ttu-id="b03cb-146">**A cURL használata**</span><span class="sxs-lookup"><span data-stu-id="b03cb-146">**Use cURL**</span></span>

<span data-ttu-id="b03cb-147">hello alábbi példa lekérdezi fürtinformációkat cURL használatával:</span><span class="sxs-lookup"><span data-stu-id="b03cb-147">hello following example gets cluster information by using cURL:</span></span>

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

<span data-ttu-id="b03cb-148">hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="b03cb-148">hello output is:</span></span>

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

<span data-ttu-id="b03cb-149">**Hello 10/8/2014 kiadásban**:</span><span class="sxs-lookup"><span data-stu-id="b03cb-149">**For hello 10/8/2014 release**:</span></span>

<span data-ttu-id="b03cb-150">Ha hello Ambari használatával végpont, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" hello *gazdaszámítógép_neve* mező hello teljesen minősített tartománynevét (FQDN) hello csomópont hello állomás neve helyett adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b03cb-150">When using hello Ambari endpoint, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", hello *host_name* field returns hello fully qualified domain name (FQDN) of hello node instead of hello host name.</span></span> <span data-ttu-id="b03cb-151">Hello 10/8/2014 kiadásban, mielőtt ez a példa adott vissza egyszerűen "**headnode0**".</span><span class="sxs-lookup"><span data-stu-id="b03cb-151">Before hello 10/8/2014 release, this example returned simply "**headnode0**".</span></span> <span data-ttu-id="b03cb-152">Hello 10/8/2014 kiadásban után kapott hello FQDN "**headnode0. { ClusterDNS} .azurehdinsight .net**", hello előző példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="b03cb-152">After hello 10/8/2014 release, you get hello FQDN "**headnode0.{ClusterDNS}.azurehdinsight.net**", as shown in hello previous example.</span></span> <span data-ttu-id="b03cb-153">Ez a változás történt szükséges toofacilitate forgatókönyvek, ahol több fürt típust (például a HBase és a Hadoop) is telepíthető egy virtuális hálózathoz (VNET).</span><span class="sxs-lookup"><span data-stu-id="b03cb-153">This change was required toofacilitate scenarios where multiple cluster types (such as HBase and Hadoop) can be deployed in one virtual network (VNET).</span></span> <span data-ttu-id="b03cb-154">Ez történik, például a HBase a háttér-platformként a Hadoop használatakor.</span><span class="sxs-lookup"><span data-stu-id="b03cb-154">This happens, for example, when using HBase as a back-end platform for Hadoop.</span></span>

## <a name="ambari-monitoring-apis"></a><span data-ttu-id="b03cb-155">Ambari API-k figyelése</span><span class="sxs-lookup"><span data-stu-id="b03cb-155">Ambari monitoring APIs</span></span>
<span data-ttu-id="b03cb-156">hello alábbi táblázat néhány hello leggyakoribb Ambari figyelési API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="b03cb-156">hello following table lists some of hello most common Ambari monitoring API calls.</span></span> <span data-ttu-id="b03cb-157">Hello API kapcsolatos további információkért lásd: [Ambari API-referencia][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="b03cb-157">For more information about hello API, see [Ambari API Reference][ambari-api-reference].</span></span>

| <span data-ttu-id="b03cb-158">A figyelő API-hívás</span><span class="sxs-lookup"><span data-stu-id="b03cb-158">Monitor API call</span></span> | <span data-ttu-id="b03cb-159">URI</span><span class="sxs-lookup"><span data-stu-id="b03cb-159">URI</span></span> | <span data-ttu-id="b03cb-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="b03cb-160">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b03cb-161">Fürtök beolvasása</span><span class="sxs-lookup"><span data-stu-id="b03cb-161">Get clusters</span></span> |`/api/v1/clusters` | |
| <span data-ttu-id="b03cb-162">Fürt-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b03cb-162">Get cluster info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |<span data-ttu-id="b03cb-163">fürtök, a szolgáltatások, a gazdagépek</span><span class="sxs-lookup"><span data-stu-id="b03cb-163">clusters, services, hosts</span></span> |
| <span data-ttu-id="b03cb-164">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b03cb-164">Get services</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |<span data-ttu-id="b03cb-165">Szolgáltatások közé tartoznak: hdfs, mapreduce</span><span class="sxs-lookup"><span data-stu-id="b03cb-165">Services include: hdfs, mapreduce</span></span> |
| <span data-ttu-id="b03cb-166">Szolgáltatások adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b03cb-166">Get services info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| <span data-ttu-id="b03cb-167">Szolgáltatás-összetevők beszerzése</span><span class="sxs-lookup"><span data-stu-id="b03cb-167">Get service components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |<span data-ttu-id="b03cb-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span><span class="sxs-lookup"><span data-stu-id="b03cb-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span></span> |
| <span data-ttu-id="b03cb-169">Összetevő-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b03cb-169">Get component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |<span data-ttu-id="b03cb-170">ServiceComponentInfo, gazdagép-összetevők, metrikák</span><span class="sxs-lookup"><span data-stu-id="b03cb-170">ServiceComponentInfo, host-components, metrics</span></span> |
| <span data-ttu-id="b03cb-171">Gazdagépek beolvasása</span><span class="sxs-lookup"><span data-stu-id="b03cb-171">Get hosts</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |<span data-ttu-id="b03cb-172">headnode0, workernode0</span><span class="sxs-lookup"><span data-stu-id="b03cb-172">headnode0, workernode0</span></span> |
| <span data-ttu-id="b03cb-173">Gazdagép-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b03cb-173">Get host info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| <span data-ttu-id="b03cb-174">Gazdagép-összetevők beszerzése</span><span class="sxs-lookup"><span data-stu-id="b03cb-174">Get host components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |<span data-ttu-id="b03cb-175">namenode, erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="b03cb-175">namenode, resourcemanager</span></span> |
| <span data-ttu-id="b03cb-176">Összetevő-adatok beolvasása a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="b03cb-176">Get host component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |<span data-ttu-id="b03cb-177">HostRoles, összetevőt, a gazdagép, a metrikák</span><span class="sxs-lookup"><span data-stu-id="b03cb-177">HostRoles, component, host, metrics</span></span> |
| <span data-ttu-id="b03cb-178">Konfigurációk beolvasása</span><span class="sxs-lookup"><span data-stu-id="b03cb-178">Get configurations</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |<span data-ttu-id="b03cb-179">Config típusok: core-webhely, hdfs-webhely, mapred-helyet, hive-hely</span><span class="sxs-lookup"><span data-stu-id="b03cb-179">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |
| <span data-ttu-id="b03cb-180">Konfigurációs adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b03cb-180">Get configuration info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |<span data-ttu-id="b03cb-181">Config típusok: core-webhely, hdfs-webhely, mapred-helyet, hive-hely</span><span class="sxs-lookup"><span data-stu-id="b03cb-181">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b03cb-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b03cb-182">Next Steps</span></span>
<span data-ttu-id="b03cb-183">Most megtanulta, hogyan toouse Ambari API figyelési hívja.</span><span class="sxs-lookup"><span data-stu-id="b03cb-183">Now you have learned how toouse Ambari monitoring API calls.</span></span> <span data-ttu-id="b03cb-184">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="b03cb-184">toolearn more, see:</span></span>

* <span data-ttu-id="b03cb-185">[Azure-portálon hello HDInsight-fürtök kezelése][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="b03cb-185">[Manage HDInsight clusters using hello Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="b03cb-186">[Az Azure PowerShell HDInsight-fürtök kezelése][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="b03cb-186">[Manage HDInsight clusters using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="b03cb-187">[Parancssori felület használata a HDInsight-fürtök kezelése][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="b03cb-187">[Manage HDInsight clusters using command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="b03cb-188">[HDInsight-dokumentáció][hdinsight-documentation]</span><span class="sxs-lookup"><span data-stu-id="b03cb-188">[HDInsight documentation][hdinsight-documentation]</span></span>
* <span data-ttu-id="b03cb-189">[Első lépései a hdinsight eszközzel][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="b03cb-189">[Get started with HDInsight][hdinsight-get-started]</span></span>

[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: https://docs.microsoft.com/azure/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
