---
title: "Felügyeletéhez és kezeléséhez az Ambari REST API - Azure HDInsight Hadoop |} Microsoft Docs"
description: "Megtudhatja, hogyan figyelheti és kezelheti az Azure HDInsight Hadoop-fürtök az Ambari használatával. Ebben a dokumentumban, megtudhatja, hogyan használható az Ambari REST API-t a HDInsight-fürtök részét képező."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 7960d83bce22d4f671d61e9aaf55561bc24308f8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a><span data-ttu-id="0cb73-104">A HDInsight-fürtök kezelése az Ambari REST API használatával</span><span class="sxs-lookup"><span data-stu-id="0cb73-104">Manage HDInsight clusters by using the Ambari REST API</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="0cb73-105">Ismerje meg, hogy kezelni és megfigyelni az Azure HDInsight Hadoop-fürtök az Ambari REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="0cb73-105">Learn how to use the Ambari REST API to manage and monitor Hadoop clusters in Azure HDInsight.</span></span>

<span data-ttu-id="0cb73-106">Apache Ambari egyszerűbbé teszi a felügyeleti és a Hadoop-fürthöz, adja meg a webes felhasználói felület és a REST API használatát egy könnyen ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="0cb73-106">Apache Ambari simplifies the management and monitoring of a Hadoop cluster by providing an easy to use web UI and REST API.</span></span> <span data-ttu-id="0cb73-107">Ambari szerepel-e a Linux operációs rendszer használata a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="0cb73-107">Ambari is included on HDInsight clusters that use the Linux operating system.</span></span> <span data-ttu-id="0cb73-108">Ambari használatával figyelheti a fürt és a konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0cb73-108">You can use Ambari to monitor the cluster and make configuration changes.</span></span>

## <span data-ttu-id="0cb73-109"><a id="whatis"></a>Mi az az Ambari</span><span class="sxs-lookup"><span data-stu-id="0cb73-109"><a id="whatis"></a>What is Ambari</span></span>

<span data-ttu-id="0cb73-110">[Apache Ambari](http://ambari.apache.org) biztosít a webes felhasználói felület kiépítéséhez, kezelése és figyelése a Hadoop-fürtök használható.</span><span class="sxs-lookup"><span data-stu-id="0cb73-110">[Apache Ambari](http://ambari.apache.org) provides web UI that can be used to provision, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="0cb73-111">A fejlesztők integrálható a ezeket a képességeket a alkalmazások használatával a [Ambari REST API-k](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="0cb73-111">Developers can integrate these capabilities into their applications by using the [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="0cb73-112">Alapértelmezés szerint a Linux-alapú HDInsight-fürtök Ambari valósul meg.</span><span class="sxs-lookup"><span data-stu-id="0cb73-112">Ambari is provided by default with Linux-based HDInsight clusters.</span></span>

## <a name="how-to-use-the-ambari-rest-api"></a><span data-ttu-id="0cb73-113">Az Ambari REST API használatával</span><span class="sxs-lookup"><span data-stu-id="0cb73-113">How to use the Ambari REST API</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0cb73-114">Információkat és példákat a jelen dokumentum igényelnek a Linux operációs rendszert használó HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="0cb73-114">The information and examples in this document require an HDInsight cluster that uses Linux operating system.</span></span> <span data-ttu-id="0cb73-115">További információkért lásd: [első lépései a hdinsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0cb73-115">For more information, see [Get started with HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

<span data-ttu-id="0cb73-116">Ebben a dokumentumban szereplő példák a Bourne rendszerhéj (bash) és a PowerShell-okat.</span><span class="sxs-lookup"><span data-stu-id="0cb73-116">The examples in this document are provided for both the Bourne shell (bash) and PowerShell.</span></span> <span data-ttu-id="0cb73-117">A bash eszközt, példákat alapján történő teszteléskor az GNU bash 4.3.11, de más Unix ismertetése együtt kell működnie.</span><span class="sxs-lookup"><span data-stu-id="0cb73-117">The bash examples were tested with GNU bash 4.3.11, but should work with other Unix shells.</span></span> <span data-ttu-id="0cb73-118">A PowerShell-példák PowerShell 5.0 teszteltük, de a PowerShell 3.0-s vagy újabb verzióját kell működnie.</span><span class="sxs-lookup"><span data-stu-id="0cb73-118">The PowerShell examples were tested with PowerShell 5.0, but should work with PowerShell 3.0 or higher.</span></span>

<span data-ttu-id="0cb73-119">Ha használja a __Bourne rendszerhéj__ (Bash), rendelkeznie kell a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="0cb73-119">If using the __Bourne shell__ (Bash), you must have the following installed:</span></span>

* <span data-ttu-id="0cb73-120">[cURL](http://curl.haxx.se/): cURL egy segédprogram, amely a REST API-k használata a parancssorból is használható.</span><span class="sxs-lookup"><span data-stu-id="0cb73-120">[cURL](http://curl.haxx.se/): cURL is a utility that can be used to work with REST APIs from the command line.</span></span> <span data-ttu-id="0cb73-121">Ebben a dokumentumban szolgál az Ambari REST API folytatott kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="0cb73-121">In this document, it is used to communicate with the Ambari REST API.</span></span>

<span data-ttu-id="0cb73-122">E Bash vagy a PowerShell használatával, rendelkeznie kell [jq](https://stedolan.github.io/jq/) telepítve.</span><span class="sxs-lookup"><span data-stu-id="0cb73-122">Whether using Bash or PowerShell, you must also have [jq](https://stedolan.github.io/jq/) installed.</span></span> <span data-ttu-id="0cb73-123">Jq olyan eszköz, amellyel a JSON-dokumentumok használata.</span><span class="sxs-lookup"><span data-stu-id="0cb73-123">Jq is a utility for working with JSON documents.</span></span> <span data-ttu-id="0cb73-124">Szerepel, **összes** Bash példák és **egy** a PowerShell-példák.</span><span class="sxs-lookup"><span data-stu-id="0cb73-124">It is used in **all** the Bash examples, and **one** of the PowerShell examples.</span></span>

### <a name="base-uri-for-ambari-rest-api"></a><span data-ttu-id="0cb73-125">Alap URI-JÁNAK Ambari Rest API</span><span class="sxs-lookup"><span data-stu-id="0cb73-125">Base URI for Ambari Rest API</span></span>

<span data-ttu-id="0cb73-126">Az Ambari REST API-t a HDInsight alap URI-azonosító https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, ahol **CLUSTERNAME** a fürt neve.</span><span class="sxs-lookup"><span data-stu-id="0cb73-126">The base URI for the Ambari REST API on HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, where **CLUSTERNAME** is the name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0cb73-127">Míg a fürt neve a teljesen minősített tartománynév (FQDN) része az URI (CLUSTERNAME.azurehdinsight.net) a nagybetűk között, más előfordulások URI azonosítójában nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="0cb73-127">While the cluster name in the fully qualified domain name (FQDN) part of the URI (CLUSTERNAME.azurehdinsight.net) is case-insensitive, other occurrences in the URI are case-sensitive.</span></span> <span data-ttu-id="0cb73-128">Például, ha a fürt neve `MyCluster`, érvényes URI-azonosítók a következők:</span><span class="sxs-lookup"><span data-stu-id="0cb73-128">For example, if your cluster is named `MyCluster`, the following are valid URIs:</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> <span data-ttu-id="0cb73-129">A következő URI-k egy hibaüzenetet adja vissza, mert a név a második előfordulása nincs kis-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="0cb73-129">The following URIs return an error because the second occurrence of the name is not the correct case.</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a><span data-ttu-id="0cb73-130">Authentication</span><span class="sxs-lookup"><span data-stu-id="0cb73-130">Authentication</span></span>

<span data-ttu-id="0cb73-131">A HDInsight Ambari való csatlakozás igényel a HTTPS PROTOKOLLT.</span><span class="sxs-lookup"><span data-stu-id="0cb73-131">Connecting to Ambari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="0cb73-132">A rendszergazda fiók nevét használja (az alapértelmezett érték **admin**) és a fürt létrehozása során megadott jelszót.</span><span class="sxs-lookup"><span data-stu-id="0cb73-132">Use the admin account name (the default is **admin**) and password you provided during cluster creation.</span></span>

## <a name="examples-authentication-and-parsing-json"></a><span data-ttu-id="0cb73-133">Példák: Hitelesítés és elemzése JSON</span><span class="sxs-lookup"><span data-stu-id="0cb73-133">Examples: Authentication and parsing JSON</span></span>

<span data-ttu-id="0cb73-134">Az alábbi példák bemutatják, hogyan végezheti el az alap Ambari REST API egy GET kérelmet:</span><span class="sxs-lookup"><span data-stu-id="0cb73-134">The following examples demonstrate how to make a GET request against the base Ambari REST API:</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> <span data-ttu-id="0cb73-135">Ebben a dokumentumban a Bash példák hajtsa végre a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="0cb73-135">The Bash examples in this document make the following assumptions:</span></span>
>
> * <span data-ttu-id="0cb73-136">A bejelentkezési név a fürt az alapértelmezett értékének `admin`.</span><span class="sxs-lookup"><span data-stu-id="0cb73-136">The login name for the cluster is the default value of `admin`.</span></span>
> * <span data-ttu-id="0cb73-137">`$PASSWORD`a jelszót ahhoz, hogy a HDInsight bejelentkezési parancs tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0cb73-137">`$PASSWORD` contains the password for the HDInsight login command.</span></span> <span data-ttu-id="0cb73-138">Használja ezt az értéket is megadhat `PASSWORD='mypassword'`.</span><span class="sxs-lookup"><span data-stu-id="0cb73-138">You can set this value by using `PASSWORD='mypassword'`.</span></span>
> * <span data-ttu-id="0cb73-139">`$CLUSTERNAME`a fürt nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0cb73-139">`$CLUSTERNAME` contains the name of the cluster.</span></span> <span data-ttu-id="0cb73-140">Használja ezt az értéket is megadhat`set CLUSTERNAME='clustername'`</span><span class="sxs-lookup"><span data-stu-id="0cb73-140">You can set this value by using `set CLUSTERNAME='clustername'`</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> <span data-ttu-id="0cb73-141">Ebben a dokumentumban a PowerShell-példák hajtsa végre a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="0cb73-141">The PowerShell examples in this document make the following assumptions:</span></span>
>
> * <span data-ttu-id="0cb73-142">`$creds`a hitelesítő objektum, amely tartalmazza a rendszergazdai bejelentkezés és a jelszót a fürthöz van.</span><span class="sxs-lookup"><span data-stu-id="0cb73-142">`$creds` is a credential object that contains the admin login and password for the cluster.</span></span> <span data-ttu-id="0cb73-143">Használja ezt az értéket is megadhat `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` , és adja meg a hitelesítő adatokat, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="0cb73-143">You can set this value by using `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` and providing the credentials when prompted.</span></span>
> * <span data-ttu-id="0cb73-144">`$clusterName`egy olyan karakterlánc, amely a fürt nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0cb73-144">`$clusterName` is a string that contains the name of the cluster.</span></span> <span data-ttu-id="0cb73-145">Használja ezt az értéket is megadhat `$clusterName="clustername"`.</span><span class="sxs-lookup"><span data-stu-id="0cb73-145">You can set this value by using `$clusterName="clustername"`.</span></span>

<span data-ttu-id="0cb73-146">Mindkét példák adja vissza egy JSON-dokumentum információkat az alábbi példához hasonló karakterlánccal kezdődik:</span><span class="sxs-lookup"><span data-stu-id="0cb73-146">Both examples return a JSON document that begins with information similar to the following example:</span></span>

```json
{
"href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
"Clusters" : {
    "cluster_id" : 2,
    "cluster_name" : "CLUSTERNAME",
    "health_report" : {
    "Host/stale_config" : 0,
    "Host/maintenance_state" : 0,
    "Host/host_state/HEALTHY" : 7,
    "Host/host_state/UNHEALTHY" : 0,
    "Host/host_state/HEARTBEAT_LOST" : 0,
    "Host/host_state/INIT" : 0,
    "Host/host_status/HEALTHY" : 7,
    "Host/host_status/UNHEALTHY" : 0,
    "Host/host_status/UNKNOWN" : 0,
    "Host/host_status/ALERT" : 0
    ...
```

### <a name="parsing-json-data"></a><span data-ttu-id="0cb73-147">JSON-adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="0cb73-147">Parsing JSON data</span></span>

<span data-ttu-id="0cb73-148">Az alábbi példában `jq` elemezni a JSON-válasz dokumentum ki és jelenítheti meg a csak a `health_report` az eredmények adatait.</span><span class="sxs-lookup"><span data-stu-id="0cb73-148">The following example uses `jq` to parse the JSON response document and display only the `health_report` information from the results.</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

<span data-ttu-id="0cb73-149">A PowerShell 3.0-s és újabb rendszer biztosítja a `ConvertFrom-Json` parancsmagot, amely a JSON-dokumentum alakít át egy objektumot, amely egyszerűbbé teszik a Powershellből.</span><span class="sxs-lookup"><span data-stu-id="0cb73-149">PowerShell 3.0 and higher provides the `ConvertFrom-Json` cmdlet, which converts the JSON document into an object that is easier to work with from PowerShell.</span></span> <span data-ttu-id="0cb73-150">Az alábbi példában `ConvertFrom-Json` megjelenítése csak a `health_report` az eredmények adatait.</span><span class="sxs-lookup"><span data-stu-id="0cb73-150">The following example uses `ConvertFrom-Json` to display only the `health_report` information from the results.</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> <span data-ttu-id="0cb73-151">Ez a dokumentum használatát a legtöbb példa során `ConvertFrom-Json` a válasz dokumentumból elemek megjelenítéséhez a [frissítés Ambari konfigurációs](#example-update-ambari-configuration) példában jq.</span><span class="sxs-lookup"><span data-stu-id="0cb73-151">While most examples in this document use `ConvertFrom-Json` to display elements from the response document, the [Update Ambari configuration](#example-update-ambari-configuration) example uses jq.</span></span> <span data-ttu-id="0cb73-152">Jq szerepel ebben a példában a JSON-válasz dokumentum az új sablon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0cb73-152">Jq is used in this example to construct a new template from the JSON response document.</span></span>

<span data-ttu-id="0cb73-153">A REST API-t a teljes referenciáért lásd: [Ambari API-referencia V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="0cb73-153">For a complete reference of the REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

## <a name="example-get-the-fqdn-of-cluster-nodes"></a><span data-ttu-id="0cb73-154">Példa: Első fürtcsomópont teljes Tartományneve</span><span class="sxs-lookup"><span data-stu-id="0cb73-154">Example: Get the FQDN of cluster nodes</span></span>

<span data-ttu-id="0cb73-155">A HDInsight használata, amikor szükség lehet tudni, hogy a teljesen minősített tartományneve (FQDN) egy fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="0cb73-155">When working with HDInsight, you may need to know the fully qualified domain name (FQDN) of a cluster node.</span></span> <span data-ttu-id="0cb73-156">A különböző csomópontok használatával az alábbi példák a fürt teljes Tartománynevének könnyen le:</span><span class="sxs-lookup"><span data-stu-id="0cb73-156">You can easily retrieve the FQDN for the various nodes in the cluster using the following examples:</span></span>

* <span data-ttu-id="0cb73-157">**Minden csomópont**</span><span class="sxs-lookup"><span data-stu-id="0cb73-157">**All nodes**</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* <span data-ttu-id="0cb73-158">**HEAD csomópontok**</span><span class="sxs-lookup"><span data-stu-id="0cb73-158">**Head nodes**</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* <span data-ttu-id="0cb73-159">**Munkavégző csomópontokhoz**</span><span class="sxs-lookup"><span data-stu-id="0cb73-159">**Worker nodes**</span></span>

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* <span data-ttu-id="0cb73-160">**Zookeeper csomópontok**</span><span class="sxs-lookup"><span data-stu-id="0cb73-160">**Zookeeper nodes**</span></span>

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a><span data-ttu-id="0cb73-161">Példa: A belső IP-cím fürtcsomópontok beolvasása</span><span class="sxs-lookup"><span data-stu-id="0cb73-161">Example: Get the internal IP address of cluster nodes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0cb73-162">Az ebben a szakaszban szereplő példák által visszaadott IP-címek nem érhetők el közvetlenül az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="0cb73-162">The IP addresses returned by the examples in this section are not directly accessible over the internet.</span></span> <span data-ttu-id="0cb73-163">Csak azok a HDInsight-fürt tartalmazó Azure virtuális hálózaton belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="0cb73-163">They are only accessible within the Azure Virtual Network that contains the HDInsight cluster.</span></span>
>
> <span data-ttu-id="0cb73-164">További információk a HDInsight és virtuális hálózatok: [kiterjesztése HDInsight képességek egyéni Azure virtuális hálózat használatával](hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="0cb73-164">For more information on working with HDInsight and virtual networks, see [Extend HDInsight capabilities by using a custom Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span></span>

<span data-ttu-id="0cb73-165">Az IP-címének ismernie kell a fürt csomópontjai belső teljesen minősített tartománynevét (FQDN).</span><span class="sxs-lookup"><span data-stu-id="0cb73-165">To find the IP address, you must know the internal fully qualified domain name (FQDN) of the cluster nodes.</span></span> <span data-ttu-id="0cb73-166">Miután a teljes Tartománynevét, majd érheti el a gazdagép IP-címét.</span><span class="sxs-lookup"><span data-stu-id="0cb73-166">Once you have the FQDN, you can then get the IP address of the host.</span></span> <span data-ttu-id="0cb73-167">Az alábbi példák először Ambari kereshet a gazdagép-csomópontok teljes Tartománynevét, majd Ambari lekérdezni az egyes állomások IP-címét.</span><span class="sxs-lookup"><span data-stu-id="0cb73-167">The following examples first query Ambari for the FQDN of all the host nodes, then query Ambari for the IP address of each host.</span></span>

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts"
$resp = Invoke-WebRequest -Uri $uri -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

## <a name="example-get-the-default-storage"></a><span data-ttu-id="0cb73-168">Példa: Az alapértelmezett tároló beolvasása</span><span class="sxs-lookup"><span data-stu-id="0cb73-168">Example: Get the default storage</span></span>

<span data-ttu-id="0cb73-169">HDInsight-fürtök létrehozásakor kell használnia az Azure Storage-fiók vagy a Data Lake Store az alapértelmezett tárolóként a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="0cb73-169">When you create an HDInsight cluster, you must use an Azure Storage Account or Data Lake Store as the default storage for the cluster.</span></span> <span data-ttu-id="0cb73-170">Ambari segítségével ezt az információt lekérni a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="0cb73-170">You can use Ambari to retrieve this information after the cluster has been created.</span></span> <span data-ttu-id="0cb73-171">Ha például szeretné az adatokat a tárolót a HDInsight kívül olvasására vagy írására.</span><span class="sxs-lookup"><span data-stu-id="0cb73-171">For example, if you want to read/write data to the container outside HDInsight.</span></span>

<span data-ttu-id="0cb73-172">Az alábbi példák a alapértelmezett tárolási konfiguráció lekérése a fürt:</span><span class="sxs-lookup"><span data-stu-id="0cb73-172">The following examples retrieve the default storage configuration from the cluster:</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]
> <span data-ttu-id="0cb73-173">Ezek a példák, térjen vissza az első konfiguráció, a kiszolgálón alkalmazott (`service_config_version=1`) amely tartalmazza ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="0cb73-173">These examples return the first configuration applied to the server (`service_config_version=1`) which contains this information.</span></span> <span data-ttu-id="0cb73-174">Ha beolvasni egy érték, amely a fürt létrehozása után módosítva lett, szükség lehet a konfiguráció listában, és töltik le a legújabb.</span><span class="sxs-lookup"><span data-stu-id="0cb73-174">If you retrieve a value that has been modified after cluster creation, you may need to list the configuration versions and retrieve the latest one.</span></span>

<span data-ttu-id="0cb73-175">A visszatérési érték a következő példák hasonló:</span><span class="sxs-lookup"><span data-stu-id="0cb73-175">The return value is similar to one of the following examples:</span></span>

* <span data-ttu-id="0cb73-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`-Ez az érték azt jelzi, hogy a fürt alapértelmezett tárolására használt Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="0cb73-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` - This value indicates that the cluster is using an Azure Storage account for default storage.</span></span> <span data-ttu-id="0cb73-177">A `ACCOUNTNAME` értéke a tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="0cb73-177">The `ACCOUNTNAME` value is the name of the storage account.</span></span> <span data-ttu-id="0cb73-178">A `CONTAINER` részét pedig a tárfiók a blob-tároló neve.</span><span class="sxs-lookup"><span data-stu-id="0cb73-178">The `CONTAINER` portion is the name of the blob container in the storage account.</span></span> <span data-ttu-id="0cb73-179">A tároló a fürt HDFS-kompatibilis tárolási gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="0cb73-179">The container is the root of the HDFS compatible storage for the cluster.</span></span>

* <span data-ttu-id="0cb73-180">`adl://home`-Ez az érték azt jelzi, hogy a fürt egy Azure Data Lake Store használt alapértelmezett tárolási.</span><span class="sxs-lookup"><span data-stu-id="0cb73-180">`adl://home` - This value indicates that the cluster is using an Azure Data Lake Store for default storage.</span></span>

    <span data-ttu-id="0cb73-181">A Data Lake Store-fiók neve megkereséséhez használja az alábbi példákat:</span><span class="sxs-lookup"><span data-stu-id="0cb73-181">To find the Data Lake Store account name, use the following examples:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    <span data-ttu-id="0cb73-182">Az eredményül kapott értéket hasonlít `ACCOUNTNAME.azuredatalakestore.net`, ahol `ACCOUNTNAME` a Data Lake Store-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="0cb73-182">The return value is similar to `ACCOUNTNAME.azuredatalakestore.net`, where `ACCOUNTNAME` is the name of the Data Lake Store account.</span></span>

    <span data-ttu-id="0cb73-183">A könyvtár belül, amely tartalmazza a fürt tárolóhelyét Data Lake Store megkereséséhez használja az alábbi példákat:</span><span class="sxs-lookup"><span data-stu-id="0cb73-183">To find the directory within Data Lake Store that contains the storage for the cluster, use the following examples:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    <span data-ttu-id="0cb73-184">Az eredményül kapott értéket hasonlít `/clusters/CLUSTERNAME/`.</span><span class="sxs-lookup"><span data-stu-id="0cb73-184">The return value is similar to `/clusters/CLUSTERNAME/`.</span></span> <span data-ttu-id="0cb73-185">Ez az érték a Data Lake Store-fiókban út.</span><span class="sxs-lookup"><span data-stu-id="0cb73-185">This value is a path within the Data Lake Store account.</span></span> <span data-ttu-id="0cb73-186">Ez az elérési út a HDFS-kompatibilis fájlrendszer a fürt gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="0cb73-186">This path is the root of the HDFS compatible file system for the cluster.</span></span> 

> [!NOTE]
> <span data-ttu-id="0cb73-187">A `Get-AzureRmHDInsightCluster` parancsmag által biztosított [Azure PowerShell](/powershell/azure/overview) is a a fürt tárolási információkat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="0cb73-187">The `Get-AzureRmHDInsightCluster` cmdlet provided by [Azure PowerShell](/powershell/azure/overview) also returns the storage information for the cluster.</span></span>


## <a name="example-get-configuration"></a><span data-ttu-id="0cb73-188">Példa: Get-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="0cb73-188">Example: Get configuration</span></span>

1. <span data-ttu-id="0cb73-189">A konfigurációk a fürt számára rendelkezésre álló beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0cb73-189">Get the configurations that are available for your cluster.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    <span data-ttu-id="0cb73-190">Ebben a példában a jelenlegi konfiguráció tartalmazó JSON-dokumentum adja vissza (azonosíthatók a *címke* érték) a fürtben telepített összetevőket.</span><span class="sxs-lookup"><span data-stu-id="0cb73-190">This example returns a JSON document containing the current configuration (identified by the *tag* value) for the components installed on the cluster.</span></span> <span data-ttu-id="0cb73-191">A következő példa egy fájlból egy Spark-fürt típusa által visszaadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="0cb73-191">The following example is an excerpt from the data returned from a Spark cluster type.</span></span>
   
   ```json
   "spark-metrics-properties" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-fairscheduler" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-sparkconf" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   }
   ```

2. <span data-ttu-id="0cb73-192">A konfiguráció megváltozása összetevő beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0cb73-192">Get the configuration for the component that you are interested in.</span></span> <span data-ttu-id="0cb73-193">Az alábbi példában cserélje le `INITIAL` az előző kérelem által visszaadott a címke értékű.</span><span class="sxs-lookup"><span data-stu-id="0cb73-193">In the following example, replace `INITIAL` with the tag value returned from the previous request.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    <span data-ttu-id="0cb73-194">Ebben a példában a vonatkozó aktuális konfigurációját tartalmazó JSON-dokumentum adja vissza a `core-site` összetevő.</span><span class="sxs-lookup"><span data-stu-id="0cb73-194">This example returns a JSON document containing the current configuration for the `core-site` component.</span></span>

## <a name="example-update-configuration"></a><span data-ttu-id="0cb73-195">Példa: Konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="0cb73-195">Example: Update configuration</span></span>

1. <span data-ttu-id="0cb73-196">Töltse le a jelenlegi konfigurációt, és a "szükségeskonfiguráció" Ambari tárolja:</span><span class="sxs-lookup"><span data-stu-id="0cb73-196">Get the current configuration, which Ambari stores as the "desired configuration":</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    <span data-ttu-id="0cb73-197">Ebben a példában a jelenlegi konfiguráció tartalmazó JSON-dokumentum adja vissza (azonosíthatók a *címke* érték) a fürtben telepített összetevőket.</span><span class="sxs-lookup"><span data-stu-id="0cb73-197">This example returns a JSON document containing the current configuration (identified by the *tag* value) for the components installed on the cluster.</span></span> <span data-ttu-id="0cb73-198">A következő példa egy fájlból egy Spark-fürt típusa által visszaadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="0cb73-198">The following example is an excerpt from the data returned from a Spark cluster type.</span></span>
   
    ```json
    "spark-metrics-properties" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-fairscheduler" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-sparkconf" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    }
    ```
   
    <span data-ttu-id="0cb73-199">Ebből a listából kell másolnia az összetevő neve (például **spark\_thrift\_sparkconf** és a **címke** érték.</span><span class="sxs-lookup"><span data-stu-id="0cb73-199">From this list, you need to copy the name of the component (for example, **spark\_thrift\_sparkconf** and the **tag** value.</span></span>

2. <span data-ttu-id="0cb73-200">Az összetevő és a címke konfigurációjának beolvasása a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="0cb73-200">Retrieve the configuration for the component and tag by using the following commands:</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
    | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```powershell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=spark-thrift-sparkconf&tag=INITIAL" `
        -Credential $creds
    $resp.Content | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    > [!NOTE]
    > <span data-ttu-id="0cb73-201">Cserélje le **spark-thrift-sparkconf** és **kezdeti** az összetevő és a tag, szeretné beolvasni a konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="0cb73-201">Replace **spark-thrift-sparkconf** and **INITIAL** with the component and tag that you want to retrieve the configuration for.</span></span>
   
    <span data-ttu-id="0cb73-202">Kapcsolja be az új konfigurációs sablonba a HDInsight-ból beolvasott adat Jq szolgál.</span><span class="sxs-lookup"><span data-stu-id="0cb73-202">Jq is used to turn the data retrieved from HDInsight into a new configuration template.</span></span> <span data-ttu-id="0cb73-203">Pontosabban ezek a példák a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="0cb73-203">Specifically, these examples perform the following actions:</span></span>
   
    * <span data-ttu-id="0cb73-204">Létrehoz egy egyedi értéket a "version" karakterláncot és a dátum, ami a tartalmazó `newtag`.</span><span class="sxs-lookup"><span data-stu-id="0cb73-204">Creates a unique value containing the string "version" and the date, which is stored in `newtag`.</span></span>

    * <span data-ttu-id="0cb73-205">Az új szükségeskonfiguráció a legfelső szintű dokumentum létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0cb73-205">Creates a root document for the new desired configuration.</span></span>

    * <span data-ttu-id="0cb73-206">Lekérdezi a tartalmát a `.items[]` tömb, és hozzáadja azt a a **desired_config** elemet.</span><span class="sxs-lookup"><span data-stu-id="0cb73-206">Gets the contents of the `.items[]` array and adds it under the **desired_config** element.</span></span>

    * <span data-ttu-id="0cb73-207">Törli a `href`, `version`, és `Config` elemek, ezeket az elemeket, nem szükségesek a elküldeni a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0cb73-207">Deletes the `href`, `version`, and `Config` elements, as these elements aren't needed to submit a new configuration.</span></span>

    * <span data-ttu-id="0cb73-208">Hozzáad egy `tag` elem értéke az `version#################`.</span><span class="sxs-lookup"><span data-stu-id="0cb73-208">Adds a `tag` element with a value of `version#################`.</span></span> <span data-ttu-id="0cb73-209">A numerikus része az aktuális dátumot alapul.</span><span class="sxs-lookup"><span data-stu-id="0cb73-209">The numeric portion is based on the current date.</span></span> <span data-ttu-id="0cb73-210">Minden egyes-konfiguráció egyedi kódot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="0cb73-210">Each configuration must have a unique tag.</span></span>
     
    <span data-ttu-id="0cb73-211">Végül, az adatok mentése a `newconfig.json` dokumentum.</span><span class="sxs-lookup"><span data-stu-id="0cb73-211">Finally, the data is saved to the `newconfig.json` document.</span></span> <span data-ttu-id="0cb73-212">A dokumentum struktúra az alábbi példához hasonlóan kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="0cb73-212">The document structure should appear similar to the following example:</span></span>
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. <span data-ttu-id="0cb73-213">Nyissa meg a `newconfig.json` dokumentum és a módosítása vagy hozzáadása az `properties` objektum.</span><span class="sxs-lookup"><span data-stu-id="0cb73-213">Open the `newconfig.json` document and modify/add values in the `properties` object.</span></span> <span data-ttu-id="0cb73-214">Az alábbi példa értékének megváltoztatása `"spark.yarn.am.memory"` a `"1g"` való `"3g"`.</span><span class="sxs-lookup"><span data-stu-id="0cb73-214">The following example changes the value of `"spark.yarn.am.memory"` from `"1g"` to `"3g"`.</span></span> <span data-ttu-id="0cb73-215">Bővíti ezenkívül `"spark.kryoserializer.buffer.max"` értékkel rendelkező `"256m"`.</span><span class="sxs-lookup"><span data-stu-id="0cb73-215">It also adds `"spark.kryoserializer.buffer.max"` with a value of `"256m"`.</span></span>
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    <span data-ttu-id="0cb73-216">Ha elkészült, hogy a módosításokat, mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="0cb73-216">Save the file once you are done making modifications.</span></span>

4. <span data-ttu-id="0cb73-217">A frissített konfiguráció Ambari küldhetnek a következő parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="0cb73-217">Use the following commands to submit the updated configuration to Ambari.</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```
   
    <span data-ttu-id="0cb73-218">Ezek a parancsok, küldje el a tartalmát a **newconfig.json** a fürtön, amelyen az új szükségeskonfiguráció-fájlt.</span><span class="sxs-lookup"><span data-stu-id="0cb73-218">These commands submit the contents of the **newconfig.json** file to the cluster as the new desired configuration.</span></span> <span data-ttu-id="0cb73-219">A kérelem egy JSON-dokumentum adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0cb73-219">The request returns a JSON document.</span></span> <span data-ttu-id="0cb73-220">A **versionTag** elem ebben a dokumentumban meg kell felelnie a verzió benyújtott, és a **configs** objektum tartalmazza a kért konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0cb73-220">The **versionTag** element in this document should match the version you submitted, and the **configs** object contains the configuration changes you requested.</span></span>

### <a name="example-restart-a-service-component"></a><span data-ttu-id="0cb73-221">Példa: Indítsa újra a szolgáltatás valamelyik összetevője</span><span class="sxs-lookup"><span data-stu-id="0cb73-221">Example: Restart a service component</span></span>

<span data-ttu-id="0cb73-222">Ezen a ponton az Ambari webes felhasználói felület tekinti meg, ha a Spark szolgáltatást azt jelzi, hogy azt az új konfiguráció életbe léptetéséhez újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="0cb73-222">At this point, if you look at the Ambari web UI, the Spark service indicates that it needs to be restarted before the new configuration can take effect.</span></span> <span data-ttu-id="0cb73-223">Az alábbi lépések segítségével indítsa újra a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0cb73-223">Use the following steps to restart the service.</span></span>

1. <span data-ttu-id="0cb73-224">A Spark szolgáltatás karbantartási mód engedélyezéséhez használja a következőket:</span><span class="sxs-lookup"><span data-stu-id="0cb73-224">Use the following to enable maintenance mode for the Spark service:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```
   
    <span data-ttu-id="0cb73-225">Ezek a parancsok egy JSON-dokumentum küldeni a kiszolgáló, amely bekapcsolja a karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="0cb73-225">These commands send a JSON document to the server that turns on maintenance mode.</span></span> <span data-ttu-id="0cb73-226">Ellenőrizheti, hogy a szolgáltatás jelenleg karbantartási módba a következő kérelmet:</span><span class="sxs-lookup"><span data-stu-id="0cb73-226">You can verify that the service is now in maintenance mode using the following request:</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    <span data-ttu-id="0cb73-227">A visszatérési érték `ON`.</span><span class="sxs-lookup"><span data-stu-id="0cb73-227">The return value is `ON`.</span></span>

2. <span data-ttu-id="0cb73-228">Kapcsolja ki a szolgáltatás ezt követően használja a következő:</span><span class="sxs-lookup"><span data-stu-id="0cb73-228">Next, use the following to turn off the service:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```
    
    <span data-ttu-id="0cb73-229">A rendszer a választ az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="0cb73-229">The response is similar to the following example:</span></span>
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    > [!IMPORTANT]
    > <span data-ttu-id="0cb73-230">A `href` ezt az URI által visszaadott értéket használja a fürtcsomópont belső IP-címét.</span><span class="sxs-lookup"><span data-stu-id="0cb73-230">The `href` value returned by this URI is using the internal IP address of the cluster node.</span></span> <span data-ttu-id="0cb73-231">A használatához a fürtön kívül cserélje le a "10.0.0.18:8080" rész a fürt teljes Tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="0cb73-231">To use it from outside the cluster, replace the \`10.0.0.18:8080' portion with the FQDN of the cluster.</span></span> 
    
    <span data-ttu-id="0cb73-232">Az alábbi parancsokat a kérelem állapotának beolvasása:</span><span class="sxs-lookup"><span data-stu-id="0cb73-232">The following commands retrieve the status of the request:</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    <span data-ttu-id="0cb73-233">A válasz `COMPLETED` azt jelzi, hogy a kérelem befejeződött.</span><span class="sxs-lookup"><span data-stu-id="0cb73-233">A response of `COMPLETED` indicates that the request has finished.</span></span>

3. <span data-ttu-id="0cb73-234">Miután befejeződött az előző kérést, a szolgáltatás elindításához használja a következő.</span><span class="sxs-lookup"><span data-stu-id="0cb73-234">Once the previous request completes, use the following to start the service.</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```
   
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```
    <span data-ttu-id="0cb73-235">A szolgáltatás az új konfigurációt használ.</span><span class="sxs-lookup"><span data-stu-id="0cb73-235">The service is now using the new configuration.</span></span>

4. <span data-ttu-id="0cb73-236">Végezetül a következő használatával kapcsolja ki a karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="0cb73-236">Finally, use the following to turn off maintenance mode.</span></span>
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a><span data-ttu-id="0cb73-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0cb73-237">Next steps</span></span>

<span data-ttu-id="0cb73-238">A REST API-t a teljes referenciáért lásd: [Ambari API-referencia V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="0cb73-238">For a complete reference of the REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

