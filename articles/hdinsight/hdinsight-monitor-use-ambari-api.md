---
title: "Az Ambari API-jával - Azure hdinsight Hadoop-fürtök figyelése |} Microsoft Docs"
description: "Az Apache Ambari API-k létrehozása, kezelése és figyelése a Hadoop-fürtök használja. Intuitív operátori eszközök és API-k elrejtése a Hadoop összetettségét."
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
ms.openlocfilehash: b6fc2098027690eb76b69b1427f0e9541b8a7a69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a><span data-ttu-id="d6aad-104">A HDInsight-beli Hadoop-fürtök figyelése az Ambari API használatával</span><span class="sxs-lookup"><span data-stu-id="d6aad-104">Monitor Hadoop clusters in HDInsight using the Ambari API</span></span>
<span data-ttu-id="d6aad-105">Ismerje meg, hogy a HDInsight-fürtök figyelése az Ambari API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="d6aad-105">Learn how to monitor HDInsight clusters by using Ambari APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="d6aad-106">A cikkben szereplő információkat elsősorban a Windows-alapú HDInsight-fürtök az Ambari REST API egy csak olvasható verziója van.</span><span class="sxs-lookup"><span data-stu-id="d6aad-106">The information in this article is primarily for Windows-based HDInsight clusters, which provide a read-only version of the Ambari REST API.</span></span> <span data-ttu-id="d6aad-107">Linux-alapú fürtök esetében lásd: [kezelése Hadoop fürtök az Ambari segítségével](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="d6aad-107">For Linux-based clusters, see [Manage Hadoop clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
> 
> 

## <a name="what-is-ambari"></a><span data-ttu-id="d6aad-108">Mi az az Ambari?</span><span class="sxs-lookup"><span data-stu-id="d6aad-108">What is Ambari?</span></span>
<span data-ttu-id="d6aad-109">[Apache Ambari] [ ambari-home] kiépítésére, kezelésére és Apache Hadoop-fürtök ellenőrzésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="d6aad-109">[Apache Ambari][ambari-home] is used for provisioning, managing, and monitoring Apache Hadoop clusters.</span></span> <span data-ttu-id="d6aad-110">Operátori eszközök intuitív gyűjteményét és egy robusztus API-készletet foglal magában, amelyek elfedik a Hadoop összetettségét, és leegyszerűsítik a fürtök működését.</span><span class="sxs-lookup"><span data-stu-id="d6aad-110">It includes an intuitive collection of operator tools and a robust set of APIs that hide the complexity of Hadoop, simplifying the operation of clusters.</span></span> <span data-ttu-id="d6aad-111">Az API-k kapcsolatos további információkért lásd: [Ambari API-referencia][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="d6aad-111">For more information about the APIs, see [Ambari API Reference][ambari-api-reference].</span></span> 

<span data-ttu-id="d6aad-112">A HDInsight jelenleg csak az Ambari figyelési funkció támogatja.</span><span class="sxs-lookup"><span data-stu-id="d6aad-112">HDInsight currently supports only the Ambari monitoring feature.</span></span> <span data-ttu-id="d6aad-113">Ambari API 1.0 HDInsight 3.0-s és a 2.1-es verziójú fürtöket támogatja.</span><span class="sxs-lookup"><span data-stu-id="d6aad-113">Ambari API 1.0 is supported by HDInsight version 3.0 and 2.1 clusters.</span></span> <span data-ttu-id="d6aad-114">Ez a cikk ismerteti a HDInsight-fürtökön 3.1 és 2.1 elérése során Ambari API-k.</span><span class="sxs-lookup"><span data-stu-id="d6aad-114">This article covers accessing Ambari APIs on HDInsight version 3.1 and 2.1 clusters.</span></span> <span data-ttu-id="d6aad-115">A legfontosabb különbség a kettő között, hogy egyes összetevői új képességeket nyújtanak (például előzmények feladatkiszolgáló) bevezetésével megváltoztak.</span><span class="sxs-lookup"><span data-stu-id="d6aad-115">The key difference between the two is that some of the components have changed with the introduction of new capabilities (such as the Job History Server).</span></span> 

<span data-ttu-id="d6aad-116">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="d6aad-116">**Prerequisites**</span></span>

<span data-ttu-id="d6aad-117">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="d6aad-117">Before you begin this tutorial, you must have the following items:</span></span>

* <span data-ttu-id="d6aad-118">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="d6aad-118">**A workstation with Azure PowerShell**.</span></span>
* <span data-ttu-id="d6aad-119">(Választható) [cURL][curl].</span><span class="sxs-lookup"><span data-stu-id="d6aad-119">(Optional) [cURL][curl].</span></span> <span data-ttu-id="d6aad-120">A telepítéshez lásd: [kiadások és letöltéseket tartalmazó oldalon cURL][curl-download].</span><span class="sxs-lookup"><span data-stu-id="d6aad-120">To install it, see [cURL Releases and Downloads][curl-download].</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="d6aad-121">Ha használja a cURL-parancsot a Windows rendszerben egyetlen idézőjelek beállítás értékek helyett használjon idézőjelek közé.</span><span class="sxs-lookup"><span data-stu-id="d6aad-121">When use the cURL command in Windows, use double-quotation marks instead of single-quotation marks for the option values.</span></span>
  > 
  > 
* <span data-ttu-id="d6aad-122">**Egy Azure HDInsight fürt**.</span><span class="sxs-lookup"><span data-stu-id="d6aad-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="d6aad-123">Fürtök kiépítése kapcsolatos útmutatásért lásd: [használatának megkezdésében a HDInsight] [ hdinsight-get-started] vagy [Provision HDInsight clusters][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="d6aad-123">For instructions about cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="d6aad-124">A következő adatokat az oktatóanyag teljesítéséhez szüksége:</span><span class="sxs-lookup"><span data-stu-id="d6aad-124">You need the following data to go through the tutorial:</span></span>
  
  | <span data-ttu-id="d6aad-125">Fürt tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d6aad-125">Cluster property</span></span> | <span data-ttu-id="d6aad-126">Az Azure PowerShell változó neve</span><span class="sxs-lookup"><span data-stu-id="d6aad-126">Azure PowerShell variable name</span></span> | <span data-ttu-id="d6aad-127">Érték</span><span class="sxs-lookup"><span data-stu-id="d6aad-127">Value</span></span> | <span data-ttu-id="d6aad-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="d6aad-128">Description</span></span> |
  | --- | --- | --- | --- |
  |   <span data-ttu-id="d6aad-129">A HDInsight-fürt neve</span><span class="sxs-lookup"><span data-stu-id="d6aad-129">HDInsight cluster name</span></span> |<span data-ttu-id="d6aad-130">$clusterName</span><span class="sxs-lookup"><span data-stu-id="d6aad-130">$clusterName</span></span> | |<span data-ttu-id="d6aad-131">A HDInsight-fürt neve.</span><span class="sxs-lookup"><span data-stu-id="d6aad-131">The name of your HDInsight cluster.</span></span> |
  |   <span data-ttu-id="d6aad-132">Fürt felhasználónév</span><span class="sxs-lookup"><span data-stu-id="d6aad-132">Cluster username</span></span> |<span data-ttu-id="d6aad-133">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="d6aad-133">$clusterUsername</span></span> | |<span data-ttu-id="d6aad-134">Fürt felhasználónevet megadva, a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="d6aad-134">Cluster user name specified when the cluster was created.</span></span> |
  |   <span data-ttu-id="d6aad-135">Fürt jelszó</span><span class="sxs-lookup"><span data-stu-id="d6aad-135">Cluster password</span></span> |<span data-ttu-id="d6aad-136">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="d6aad-136">$clusterPassword</span></span> | |<span data-ttu-id="d6aad-137">Fürt felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="d6aad-137">Cluster user password.</span></span> |

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


## <a name="jump-start"></a><span data-ttu-id="d6aad-138">Feladat gyors végrehajtásának megkönnyítésére</span><span class="sxs-lookup"><span data-stu-id="d6aad-138">Jump-start</span></span>
<span data-ttu-id="d6aad-139">Számos módon figyelheti a HDInsight-fürtök az Ambari használatával.</span><span class="sxs-lookup"><span data-stu-id="d6aad-139">There are several ways to use Ambari to monitor HDInsight clusters.</span></span>

<span data-ttu-id="d6aad-140">**Azure PowerShell használata**</span><span class="sxs-lookup"><span data-stu-id="d6aad-140">**Use Azure PowerShell**</span></span>

<span data-ttu-id="d6aad-141">A következő Azure PowerShell-parancsfájl lekérdezi a MapReduce feladatot követő *HDInsight 3.5 fürtben.*</span><span class="sxs-lookup"><span data-stu-id="d6aad-141">The following Azure PowerShell script gets the MapReduce job tracker information *in an HDInsight 3.5 cluster.*</span></span>  <span data-ttu-id="d6aad-142">A fő különbség, hogy azt le ezeket a részleteket a YARN szerviz (hanem MapReduce).</span><span class="sxs-lookup"><span data-stu-id="d6aad-142">The key difference is that we pull these details from the YARN service (rather than MapReduce).</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName/services/YARN/components/RESOURCEMANAGER"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

<span data-ttu-id="d6aad-143">A következő PowerShell-parancsfájl lekérdezi a MapReduce-feladatok követő adatait *HDInsight 2.1-fürtben lévő*:</span><span class="sxs-lookup"><span data-stu-id="d6aad-143">The following PowerShell script gets the MapReduce job tracker information *in an HDInsight 2.1 cluster*:</span></span>

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

<span data-ttu-id="d6aad-144">A kimenete:</span><span class="sxs-lookup"><span data-stu-id="d6aad-144">The output is:</span></span>

![Jobtracker kimeneti][img-jobtracker-output]

<span data-ttu-id="d6aad-146">**A cURL használata**</span><span class="sxs-lookup"><span data-stu-id="d6aad-146">**Use cURL**</span></span>

<span data-ttu-id="d6aad-147">Az alábbi példa fürtinformációkat lekérdezi a cURL használatával:</span><span class="sxs-lookup"><span data-stu-id="d6aad-147">The following example gets cluster information by using cURL:</span></span>

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

<span data-ttu-id="d6aad-148">A kimenete:</span><span class="sxs-lookup"><span data-stu-id="d6aad-148">The output is:</span></span>

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

<span data-ttu-id="d6aad-149">**A 10/8/2014 kiadásban**:</span><span class="sxs-lookup"><span data-stu-id="d6aad-149">**For the 10/8/2014 release**:</span></span>

<span data-ttu-id="d6aad-150">Az Ambari végpont használatakor "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", a *gazdaszámítógép_neve* mező a gazdagép neve helyett a csomópont teljesen minősített tartománynevét (FQDN) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d6aad-150">When using the Ambari endpoint, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", the *host_name* field returns the fully qualified domain name (FQDN) of the node instead of the host name.</span></span> <span data-ttu-id="d6aad-151">A 10/8/2014 kiadásban előtt ebben a példában adott vissza egyszerűen "**headnode0**".</span><span class="sxs-lookup"><span data-stu-id="d6aad-151">Before the 10/8/2014 release, this example returned simply "**headnode0**".</span></span> <span data-ttu-id="d6aad-152">10/8/2014 megjelenése után az FQDN első "**headnode0. { ClusterDNS} .azurehdinsight .net**", az előző példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="d6aad-152">After the 10/8/2014 release, you get the FQDN "**headnode0.{ClusterDNS}.azurehdinsight.net**", as shown in the previous example.</span></span> <span data-ttu-id="d6aad-153">Ez a módosítás lehetővé teszi a forgatókönyvekben, ahol több fürt típust (például a HBase és a Hadoop) is telepíthető egy virtuális hálózathoz (VNET) szükséges.</span><span class="sxs-lookup"><span data-stu-id="d6aad-153">This change was required to facilitate scenarios where multiple cluster types (such as HBase and Hadoop) can be deployed in one virtual network (VNET).</span></span> <span data-ttu-id="d6aad-154">Ez történik, például a HBase a háttér-platformként a Hadoop használatakor.</span><span class="sxs-lookup"><span data-stu-id="d6aad-154">This happens, for example, when using HBase as a back-end platform for Hadoop.</span></span>

## <a name="ambari-monitoring-apis"></a><span data-ttu-id="d6aad-155">Ambari API-k figyelése</span><span class="sxs-lookup"><span data-stu-id="d6aad-155">Ambari monitoring APIs</span></span>
<span data-ttu-id="d6aad-156">Az alábbi táblázatban a leggyakrabban használt Ambari API-hívások figyelési némelyike.</span><span class="sxs-lookup"><span data-stu-id="d6aad-156">The following table lists some of the most common Ambari monitoring API calls.</span></span> <span data-ttu-id="d6aad-157">A API-val kapcsolatos további információkért lásd: [Ambari API-referencia][ambari-api-reference].</span><span class="sxs-lookup"><span data-stu-id="d6aad-157">For more information about the API, see [Ambari API Reference][ambari-api-reference].</span></span>

| <span data-ttu-id="d6aad-158">A figyelő API-hívás</span><span class="sxs-lookup"><span data-stu-id="d6aad-158">Monitor API call</span></span> | <span data-ttu-id="d6aad-159">URI</span><span class="sxs-lookup"><span data-stu-id="d6aad-159">URI</span></span> | <span data-ttu-id="d6aad-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="d6aad-160">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d6aad-161">Fürtök beolvasása</span><span class="sxs-lookup"><span data-stu-id="d6aad-161">Get clusters</span></span> |`/api/v1/clusters` | |
| <span data-ttu-id="d6aad-162">Fürt-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d6aad-162">Get cluster info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net` |<span data-ttu-id="d6aad-163">fürtök, a szolgáltatások, a gazdagépek</span><span class="sxs-lookup"><span data-stu-id="d6aad-163">clusters, services, hosts</span></span> |
| <span data-ttu-id="d6aad-164">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d6aad-164">Get services</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services` |<span data-ttu-id="d6aad-165">Szolgáltatások közé tartoznak: hdfs, mapreduce</span><span class="sxs-lookup"><span data-stu-id="d6aad-165">Services include: hdfs, mapreduce</span></span> |
| <span data-ttu-id="d6aad-166">Szolgáltatások adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d6aad-166">Get services info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>` | |
| <span data-ttu-id="d6aad-167">Szolgáltatás-összetevők beszerzése</span><span class="sxs-lookup"><span data-stu-id="d6aad-167">Get service components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components` |<span data-ttu-id="d6aad-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span><span class="sxs-lookup"><span data-stu-id="d6aad-168">HDFS: namenode, datanodeMapReduce: jobtracker; tasktracker</span></span> |
| <span data-ttu-id="d6aad-169">Összetevő-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d6aad-169">Get component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>` |<span data-ttu-id="d6aad-170">ServiceComponentInfo, gazdagép-összetevők, metrikák</span><span class="sxs-lookup"><span data-stu-id="d6aad-170">ServiceComponentInfo, host-components, metrics</span></span> |
| <span data-ttu-id="d6aad-171">Gazdagépek beolvasása</span><span class="sxs-lookup"><span data-stu-id="d6aad-171">Get hosts</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts` |<span data-ttu-id="d6aad-172">headnode0, workernode0</span><span class="sxs-lookup"><span data-stu-id="d6aad-172">headnode0, workernode0</span></span> |
| <span data-ttu-id="d6aad-173">Gazdagép-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d6aad-173">Get host info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>` | |
| <span data-ttu-id="d6aad-174">Gazdagép-összetevők beszerzése</span><span class="sxs-lookup"><span data-stu-id="d6aad-174">Get host components</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components` |<span data-ttu-id="d6aad-175">namenode, erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="d6aad-175">namenode, resourcemanager</span></span> |
| <span data-ttu-id="d6aad-176">Összetevő-adatok beolvasása a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="d6aad-176">Get host component info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>` |<span data-ttu-id="d6aad-177">HostRoles, összetevőt, a gazdagép, a metrikák</span><span class="sxs-lookup"><span data-stu-id="d6aad-177">HostRoles, component, host, metrics</span></span> |
| <span data-ttu-id="d6aad-178">Konfigurációk beolvasása</span><span class="sxs-lookup"><span data-stu-id="d6aad-178">Get configurations</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations` |<span data-ttu-id="d6aad-179">Config típusok: core-webhely, hdfs-webhely, mapred-helyet, hive-hely</span><span class="sxs-lookup"><span data-stu-id="d6aad-179">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |
| <span data-ttu-id="d6aad-180">Konfigurációs adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d6aad-180">Get configuration info.</span></span> |`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>` |<span data-ttu-id="d6aad-181">Config típusok: core-webhely, hdfs-webhely, mapred-helyet, hive-hely</span><span class="sxs-lookup"><span data-stu-id="d6aad-181">Config types: core-site, hdfs-site, mapred-site, hive-site</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d6aad-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6aad-182">Next Steps</span></span>
<span data-ttu-id="d6aad-183">Most megtanulhatta, hogyan használható az Ambari API-hívások figyelése.</span><span class="sxs-lookup"><span data-stu-id="d6aad-183">Now you have learned how to use Ambari monitoring API calls.</span></span> <span data-ttu-id="d6aad-184">További tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="d6aad-184">To learn more, see:</span></span>

* <span data-ttu-id="d6aad-185">[Az Azure portál használata a HDInsight-fürtök kezelése][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="d6aad-185">[Manage HDInsight clusters using the Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="d6aad-186">[Az Azure PowerShell HDInsight-fürtök kezelése][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="d6aad-186">[Manage HDInsight clusters using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="d6aad-187">[Parancssori felület használata a HDInsight-fürtök kezelése][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="d6aad-187">[Manage HDInsight clusters using command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="d6aad-188">[HDInsight-dokumentáció][hdinsight-documentation]</span><span class="sxs-lookup"><span data-stu-id="d6aad-188">[HDInsight documentation][hdinsight-documentation]</span></span>
* <span data-ttu-id="d6aad-189">[Első lépései a hdinsight eszközzel][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="d6aad-189">[Get started with HDInsight][hdinsight-get-started]</span></span>

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
