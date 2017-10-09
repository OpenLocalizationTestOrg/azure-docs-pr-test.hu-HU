---
title: "aaaMonitor és kezelése az Ambari REST API - Azure HDInsight Hadoop |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Ambari toomonitor és az Azure HDInsight Hadoop-fürtök kezelése. Ebből a dokumentumból megtudhatja, hogyan toouse hello Ambari REST API-t tartalmazza a HDInsight-fürtök."
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
ms.openlocfilehash: 1866a77c8e402231bccbcfba7174253aca41339b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-rest-api"></a><span data-ttu-id="503e4-104">HDInsight-fürtök kezelése az Ambari REST API hello segítségével</span><span class="sxs-lookup"><span data-stu-id="503e4-104">Manage HDInsight clusters by using hello Ambari REST API</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="503e4-105">Ismerje meg, hogyan toouse Ambari REST API toomanage hello és az Azure HDInsight Hadoop-fürtök figyelése.</span><span class="sxs-lookup"><span data-stu-id="503e4-105">Learn how toouse hello Ambari REST API toomanage and monitor Hadoop clusters in Azure HDInsight.</span></span>

<span data-ttu-id="503e4-106">Apache Ambari egyszerűbbé teszi a hello kezelése és figyelése a Hadoop fürtök egy egyszerű toouse webes felhasználói felület és a REST API-k megadásával.</span><span class="sxs-lookup"><span data-stu-id="503e4-106">Apache Ambari simplifies hello management and monitoring of a Hadoop cluster by providing an easy toouse web UI and REST API.</span></span> <span data-ttu-id="503e4-107">A HDInsight-fürtök hello Linux operációs rendszert használó Ambari szerepel.</span><span class="sxs-lookup"><span data-stu-id="503e4-107">Ambari is included on HDInsight clusters that use hello Linux operating system.</span></span> <span data-ttu-id="503e4-108">Ambari toomonitor hello fürtöt használ, és a konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="503e4-108">You can use Ambari toomonitor hello cluster and make configuration changes.</span></span>

## <span data-ttu-id="503e4-109"><a id="whatis"></a>Mi az az Ambari</span><span class="sxs-lookup"><span data-stu-id="503e4-109"><a id="whatis"></a>What is Ambari</span></span>

<span data-ttu-id="503e4-110">[Apache Ambari](http://ambari.apache.org) webes felhasználói felület használt tooprovision, kezeléséhez, és figyelheti a Hadoop-fürtök biztosít.</span><span class="sxs-lookup"><span data-stu-id="503e4-110">[Apache Ambari](http://ambari.apache.org) provides web UI that can be used tooprovision, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="503e4-111">A fejlesztők integrálható a ezeket a képességeket a alkalmazások hello segítségével [Ambari REST API-k](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="503e4-111">Developers can integrate these capabilities into their applications by using hello [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="503e4-112">Alapértelmezés szerint a Linux-alapú HDInsight-fürtök Ambari valósul meg.</span><span class="sxs-lookup"><span data-stu-id="503e4-112">Ambari is provided by default with Linux-based HDInsight clusters.</span></span>

## <a name="how-toouse-hello-ambari-rest-api"></a><span data-ttu-id="503e4-113">Hogyan toouse hello Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="503e4-113">How toouse hello Ambari REST API</span></span>

> [!IMPORTANT]
> <span data-ttu-id="503e4-114">hello útmutatást és példákat a jelen dokumentum igényelnek a Linux operációs rendszert használó HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="503e4-114">hello information and examples in this document require an HDInsight cluster that uses Linux operating system.</span></span> <span data-ttu-id="503e4-115">További információkért lásd: [első lépései a hdinsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="503e4-115">For more information, see [Get started with HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

<span data-ttu-id="503e4-116">Ebben a dokumentumban hello példák hello Bourne rendszerhéj (bash) és a PowerShell-okat.</span><span class="sxs-lookup"><span data-stu-id="503e4-116">hello examples in this document are provided for both hello Bourne shell (bash) and PowerShell.</span></span> <span data-ttu-id="503e4-117">példák alapján történő teszteléskor az GNU hello bash bash 4.3.11, de más Unix ismertetése együtt kell működnie.</span><span class="sxs-lookup"><span data-stu-id="503e4-117">hello bash examples were tested with GNU bash 4.3.11, but should work with other Unix shells.</span></span> <span data-ttu-id="503e4-118">hello PowerShell-példák PowerShell 5.0 teszteltük, de a PowerShell 3.0-s vagy újabb verzióját kell működnie.</span><span class="sxs-lookup"><span data-stu-id="503e4-118">hello PowerShell examples were tested with PowerShell 5.0, but should work with PowerShell 3.0 or higher.</span></span>

<span data-ttu-id="503e4-119">Ha használja a hello __Bourne rendszerhéj__ (Bash), rendelkeznie kell hello következőkkel:</span><span class="sxs-lookup"><span data-stu-id="503e4-119">If using hello __Bourne shell__ (Bash), you must have hello following installed:</span></span>

* <span data-ttu-id="503e4-120">[cURL](http://curl.haxx.se/): cURL egy segédprogram, amely a REST API-khoz használt toowork hello parancssorból lehet.</span><span class="sxs-lookup"><span data-stu-id="503e4-120">[cURL](http://curl.haxx.se/): cURL is a utility that can be used toowork with REST APIs from hello command line.</span></span> <span data-ttu-id="503e4-121">Az ebben a dokumentumban az Ambari REST API hello használt toocommunicate is.</span><span class="sxs-lookup"><span data-stu-id="503e4-121">In this document, it is used toocommunicate with hello Ambari REST API.</span></span>

<span data-ttu-id="503e4-122">E Bash vagy a PowerShell használatával, rendelkeznie kell [jq](https://stedolan.github.io/jq/) telepítve.</span><span class="sxs-lookup"><span data-stu-id="503e4-122">Whether using Bash or PowerShell, you must also have [jq](https://stedolan.github.io/jq/) installed.</span></span> <span data-ttu-id="503e4-123">Jq olyan eszköz, amellyel a JSON-dokumentumok használata.</span><span class="sxs-lookup"><span data-stu-id="503e4-123">Jq is a utility for working with JSON documents.</span></span> <span data-ttu-id="503e4-124">Szerepel, **összes** hello Bash példák és **egy** hello PowerShell-példák.</span><span class="sxs-lookup"><span data-stu-id="503e4-124">It is used in **all** hello Bash examples, and **one** of hello PowerShell examples.</span></span>

### <a name="base-uri-for-ambari-rest-api"></a><span data-ttu-id="503e4-125">Alap URI-JÁNAK Ambari Rest API</span><span class="sxs-lookup"><span data-stu-id="503e4-125">Base URI for Ambari Rest API</span></span>

<span data-ttu-id="503e4-126">hello hello Ambari REST API-t a HDInsight az alap URI-azonosító https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, ahol **CLUSTERNAME** hello a fürt neve van.</span><span class="sxs-lookup"><span data-stu-id="503e4-126">hello base URI for hello Ambari REST API on HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, where **CLUSTERNAME** is hello name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="503e4-127">Amíg a fürtnév hello hello a teljesen minősített tartománynév (FQDN) része hello URI (CLUSTERNAME.azurehdinsight.net) azonban nem, más előfordulás hello URI-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="503e4-127">While hello cluster name in hello fully qualified domain name (FQDN) part of hello URI (CLUSTERNAME.azurehdinsight.net) is case-insensitive, other occurrences in hello URI are case-sensitive.</span></span> <span data-ttu-id="503e4-128">Például, ha a fürt neve `MyCluster`, az alábbiakban hello érvényes URI-azonosítók:</span><span class="sxs-lookup"><span data-stu-id="503e4-128">For example, if your cluster is named `MyCluster`, hello following are valid URIs:</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> <span data-ttu-id="503e4-129">hello következő URI-azonosítók hibát vissza, mert hello hello neve második előfordulása nem hello. Javítsa ki a eset.</span><span class="sxs-lookup"><span data-stu-id="503e4-129">hello following URIs return an error because hello second occurrence of hello name is not hello correct case.</span></span>
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a><span data-ttu-id="503e4-130">Authentication</span><span class="sxs-lookup"><span data-stu-id="503e4-130">Authentication</span></span>

<span data-ttu-id="503e4-131">A HDInsight tooAmbari csatlakozás igényel a HTTPS PROTOKOLLT.</span><span class="sxs-lookup"><span data-stu-id="503e4-131">Connecting tooAmbari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="503e4-132">Használjon hello rendszergazdai fiók neve (hello alapértelmezett érték **admin**) és a fürt létrehozása során megadott jelszót.</span><span class="sxs-lookup"><span data-stu-id="503e4-132">Use hello admin account name (hello default is **admin**) and password you provided during cluster creation.</span></span>

## <a name="examples-authentication-and-parsing-json"></a><span data-ttu-id="503e4-133">Példák: Hitelesítés és elemzése JSON</span><span class="sxs-lookup"><span data-stu-id="503e4-133">Examples: Authentication and parsing JSON</span></span>

<span data-ttu-id="503e4-134">hello a következő példák azt mutatják be, hogyan toomake hello egy GET kérelmet kiinduló Ambari REST API:</span><span class="sxs-lookup"><span data-stu-id="503e4-134">hello following examples demonstrate how toomake a GET request against hello base Ambari REST API:</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> <span data-ttu-id="503e4-135">hello Bash példák ebben a dokumentumban hajtsa végre a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="503e4-135">hello Bash examples in this document make hello following assumptions:</span></span>
>
> * <span data-ttu-id="503e4-136">hello bejelentkezési név hello fürt az alapértelmezett érték hello `admin`.</span><span class="sxs-lookup"><span data-stu-id="503e4-136">hello login name for hello cluster is hello default value of `admin`.</span></span>
> * <span data-ttu-id="503e4-137">`$PASSWORD`a HDInsight bejelentkezési parancs hello hello jelszót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="503e4-137">`$PASSWORD` contains hello password for hello HDInsight login command.</span></span> <span data-ttu-id="503e4-138">Használja ezt az értéket is megadhat `PASSWORD='mypassword'`.</span><span class="sxs-lookup"><span data-stu-id="503e4-138">You can set this value by using `PASSWORD='mypassword'`.</span></span>
> * <span data-ttu-id="503e4-139">`$CLUSTERNAME`hello fürt hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="503e4-139">`$CLUSTERNAME` contains hello name of hello cluster.</span></span> <span data-ttu-id="503e4-140">Használja ezt az értéket is megadhat`set CLUSTERNAME='clustername'`</span><span class="sxs-lookup"><span data-stu-id="503e4-140">You can set this value by using `set CLUSTERNAME='clustername'`</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> <span data-ttu-id="503e4-141">hello PowerShell-példák ebben a dokumentumban hajtsa végre a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="503e4-141">hello PowerShell examples in this document make hello following assumptions:</span></span>
>
> * <span data-ttu-id="503e4-142">`$creds`a hitelesítő objektum, amely tartalmazza a hello rendszergazda felhasználónevet és jelszót hello fürthöz van.</span><span class="sxs-lookup"><span data-stu-id="503e4-142">`$creds` is a credential object that contains hello admin login and password for hello cluster.</span></span> <span data-ttu-id="503e4-143">Használja ezt az értéket is megadhat `$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"` és hello hitelesítő adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="503e4-143">You can set this value by using `$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"` and providing hello credentials when prompted.</span></span>
> * <span data-ttu-id="503e4-144">`$clusterName`egy olyan karakterlánc, amely hello fürt hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="503e4-144">`$clusterName` is a string that contains hello name of hello cluster.</span></span> <span data-ttu-id="503e4-145">Használja ezt az értéket is megadhat `$clusterName="clustername"`.</span><span class="sxs-lookup"><span data-stu-id="503e4-145">You can set this value by using `$clusterName="clustername"`.</span></span>

<span data-ttu-id="503e4-146">Mindkét példák adja vissza egy JSON-dokumentum információkat a következő példa hasonló toohello karakterlánccal kezdődik:</span><span class="sxs-lookup"><span data-stu-id="503e4-146">Both examples return a JSON document that begins with information similar toohello following example:</span></span>

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

### <a name="parsing-json-data"></a><span data-ttu-id="503e4-147">JSON-adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="503e4-147">Parsing JSON data</span></span>

<span data-ttu-id="503e4-148">hello alábbi példában `jq` tooparse hello válasz JSON-dokumentum, és csak hello megjelenítése `health_report` hello eredmények adatait.</span><span class="sxs-lookup"><span data-stu-id="503e4-148">hello following example uses `jq` tooparse hello JSON response document and display only hello `health_report` information from hello results.</span></span>

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

<span data-ttu-id="503e4-149">A PowerShell 3.0-s és újabb rendszer biztosít hello `ConvertFrom-Json` parancsmag, amely olyan objektumot, amely a powershellből könnyebb toowork hello JSON-dokumentum alakítja.</span><span class="sxs-lookup"><span data-stu-id="503e4-149">PowerShell 3.0 and higher provides hello `ConvertFrom-Json` cmdlet, which converts hello JSON document into an object that is easier toowork with from PowerShell.</span></span> <span data-ttu-id="503e4-150">hello alábbi példában `ConvertFrom-Json` toodisplay csak hello `health_report` hello eredmények adatait.</span><span class="sxs-lookup"><span data-stu-id="503e4-150">hello following example uses `ConvertFrom-Json` toodisplay only hello `health_report` information from hello results.</span></span>

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> <span data-ttu-id="503e4-151">Ez a dokumentum használatát a legtöbb példa során `ConvertFrom-Json` toodisplay elemek hello-dokumentumból, hello [frissítés Ambari konfigurációs](#example-update-ambari-configuration) példa jq használ.</span><span class="sxs-lookup"><span data-stu-id="503e4-151">While most examples in this document use `ConvertFrom-Json` toodisplay elements from hello response document, hello [Update Ambari configuration](#example-update-ambari-configuration) example uses jq.</span></span> <span data-ttu-id="503e4-152">Jq használatban van ebben a példában tooconstruct hello JSON-válasz dokumentum egy új sablont.</span><span class="sxs-lookup"><span data-stu-id="503e4-152">Jq is used in this example tooconstruct a new template from hello JSON response document.</span></span>

<span data-ttu-id="503e4-153">Hello REST API-t a teljes referenciáért lásd: [Ambari API-referencia V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="503e4-153">For a complete reference of hello REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

## <a name="example-get-hello-fqdn-of-cluster-nodes"></a><span data-ttu-id="503e4-154">Példa: Hello fürtcsomópont FQDN beolvasása</span><span class="sxs-lookup"><span data-stu-id="503e4-154">Example: Get hello FQDN of cluster nodes</span></span>

<span data-ttu-id="503e4-155">A HDInsight használata, ha esetleg tooknow hello teljesen minősített tartományneve (FQDN) egy fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="503e4-155">When working with HDInsight, you may need tooknow hello fully qualified domain name (FQDN) of a cluster node.</span></span> <span data-ttu-id="503e4-156">Hello FQDN a hello segítségével a következő példák hello hello fürt különböző csomópontja egyszerűen beolvashatók:</span><span class="sxs-lookup"><span data-stu-id="503e4-156">You can easily retrieve hello FQDN for hello various nodes in hello cluster using hello following examples:</span></span>

* <span data-ttu-id="503e4-157">**Minden csomópont**</span><span class="sxs-lookup"><span data-stu-id="503e4-157">**All nodes**</span></span>

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

* <span data-ttu-id="503e4-158">**HEAD csomópontok**</span><span class="sxs-lookup"><span data-stu-id="503e4-158">**Head nodes**</span></span>

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

* <span data-ttu-id="503e4-159">**Munkavégző csomópontokhoz**</span><span class="sxs-lookup"><span data-stu-id="503e4-159">**Worker nodes**</span></span>

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

* <span data-ttu-id="503e4-160">**Zookeeper csomópontok**</span><span class="sxs-lookup"><span data-stu-id="503e4-160">**Zookeeper nodes**</span></span>

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

## <a name="example-get-hello-internal-ip-address-of-cluster-nodes"></a><span data-ttu-id="503e4-161">Példa: Hello belső IP-cím fürtcsomópontok beolvasása</span><span class="sxs-lookup"><span data-stu-id="503e4-161">Example: Get hello internal IP address of cluster nodes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="503e4-162">a szakaszban szereplő példák hello által visszaadott hello IP-címek vannak, nem közvetlenül elérhető over internet hello.</span><span class="sxs-lookup"><span data-stu-id="503e4-162">hello IP addresses returned by hello examples in this section are not directly accessible over hello internet.</span></span> <span data-ttu-id="503e4-163">Csak azok hello Azure virtuális hálózat, amely tartalmazza a HDInsight-fürt hello belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="503e4-163">They are only accessible within hello Azure Virtual Network that contains hello HDInsight cluster.</span></span>
>
> <span data-ttu-id="503e4-164">További információk a HDInsight és virtuális hálózatok: [kiterjesztése HDInsight képességek egyéni Azure virtuális hálózat használatával](hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="503e4-164">For more information on working with HDInsight and virtual networks, see [Extend HDInsight capabilities by using a custom Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).</span></span>

<span data-ttu-id="503e4-165">toofind hello IP-cím, ismernie kell a hello belső teljesen minősített tartományneve (FQDN) hello fürtcsomópontok.</span><span class="sxs-lookup"><span data-stu-id="503e4-165">toofind hello IP address, you must know hello internal fully qualified domain name (FQDN) of hello cluster nodes.</span></span> <span data-ttu-id="503e4-166">Miután hello teljes Tartománynevét, majd kaphat a hello állomás hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="503e4-166">Once you have hello FQDN, you can then get hello IP address of hello host.</span></span> <span data-ttu-id="503e4-167">hello alábbi példák először Ambari lekérdezése a hello hello minden gazdagép-csomópontok teljes Tartománynevét, majd hello IP-cím az egyes állomások Ambari lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="503e4-167">hello following examples first query Ambari for hello FQDN of all hello host nodes, then query Ambari for hello IP address of each host.</span></span>

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

## <a name="example-get-hello-default-storage"></a><span data-ttu-id="503e4-168">Példa: Hello alapértelmezett tárolási beolvasása</span><span class="sxs-lookup"><span data-stu-id="503e4-168">Example: Get hello default storage</span></span>

<span data-ttu-id="503e4-169">HDInsight-fürtök létrehozásakor kell használnia az Azure Storage-fiók vagy a Data Lake Store hello alapértelmezett tárolóként hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="503e4-169">When you create an HDInsight cluster, you must use an Azure Storage Account or Data Lake Store as hello default storage for hello cluster.</span></span> <span data-ttu-id="503e4-170">Használhatja Ambari tooretrieve ezt az információt hello fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="503e4-170">You can use Ambari tooretrieve this information after hello cluster has been created.</span></span> <span data-ttu-id="503e4-171">Ha például azt szeretné, hogy toohello adattároló tooread/írás HDInsight kívül.</span><span class="sxs-lookup"><span data-stu-id="503e4-171">For example, if you want tooread/write data toohello container outside HDInsight.</span></span>

<span data-ttu-id="503e4-172">hello alábbi példák lekérése hello alapértelmezett tárolási konfigurációt hello fürt:</span><span class="sxs-lookup"><span data-stu-id="503e4-172">hello following examples retrieve hello default storage configuration from hello cluster:</span></span>

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
> <span data-ttu-id="503e4-173">Ezek a példák vissza hello első alkalmazott konfiguráció toohello kiszolgáló (`service_config_version=1`) amely tartalmazza ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="503e4-173">These examples return hello first configuration applied toohello server (`service_config_version=1`) which contains this information.</span></span> <span data-ttu-id="503e4-174">Egy érték, amely a fürt létrehozása után módosítva lett letöltésekor, toolist hello konfigurációs verziók kell, és hello legújabb beolvasása.</span><span class="sxs-lookup"><span data-stu-id="503e4-174">If you retrieve a value that has been modified after cluster creation, you may need toolist hello configuration versions and retrieve hello latest one.</span></span>

<span data-ttu-id="503e4-175">hello visszatérési érték a következő példák hello hasonló tooone:</span><span class="sxs-lookup"><span data-stu-id="503e4-175">hello return value is similar tooone of hello following examples:</span></span>

* <span data-ttu-id="503e4-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`-Ez azt jelzi, hogy hello fürt által használt Azure Storage-fiók alapértelmezett tárolási.</span><span class="sxs-lookup"><span data-stu-id="503e4-176">`wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` - This value indicates that hello cluster is using an Azure Storage account for default storage.</span></span> <span data-ttu-id="503e4-177">Hello `ACCOUNTNAME` értéke hello tárfiók hello neve.</span><span class="sxs-lookup"><span data-stu-id="503e4-177">hello `ACCOUNTNAME` value is hello name of hello storage account.</span></span> <span data-ttu-id="503e4-178">Hello `CONTAINER` részét hello blob tároló hello tárfiókban hello neve.</span><span class="sxs-lookup"><span data-stu-id="503e4-178">hello `CONTAINER` portion is hello name of hello blob container in hello storage account.</span></span> <span data-ttu-id="503e4-179">hello tároló hello HDFS-kompatibilis tároló hello fürt hello gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="503e4-179">hello container is hello root of hello HDFS compatible storage for hello cluster.</span></span>

* <span data-ttu-id="503e4-180">`adl://home`-Ez azt jelzi, hogy hello fürt által használt egy Azure Data Lake Store alapértelmezett tárolási.</span><span class="sxs-lookup"><span data-stu-id="503e4-180">`adl://home` - This value indicates that hello cluster is using an Azure Data Lake Store for default storage.</span></span>

    <span data-ttu-id="503e4-181">toofind hello Data Lake Store-fiók neve, a következő példák hello használata:</span><span class="sxs-lookup"><span data-stu-id="503e4-181">toofind hello Data Lake Store account name, use hello following examples:</span></span>

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

    <span data-ttu-id="503e4-182">hello visszatérési értéke hasonló túl`ACCOUNTNAME.azuredatalakestore.net`, ahol `ACCOUNTNAME` hello hello Data Lake Store-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="503e4-182">hello return value is similar too`ACCOUNTNAME.azuredatalakestore.net`, where `ACCOUNTNAME` is hello name of hello Data Lake Store account.</span></span>

    <span data-ttu-id="503e4-183">Data Lake Store hello tárolási hello fürthöz, a következő példák használata hello tartalmazó belüli toofind hello könyvtár:</span><span class="sxs-lookup"><span data-stu-id="503e4-183">toofind hello directory within Data Lake Store that contains hello storage for hello cluster, use hello following examples:</span></span>

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

    <span data-ttu-id="503e4-184">hello visszatérési értéke hasonló túl`/clusters/CLUSTERNAME/`.</span><span class="sxs-lookup"><span data-stu-id="503e4-184">hello return value is similar too`/clusters/CLUSTERNAME/`.</span></span> <span data-ttu-id="503e4-185">Ez az érték út belül hello Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="503e4-185">This value is a path within hello Data Lake Store account.</span></span> <span data-ttu-id="503e4-186">Ez az elérési út hello HDFS kompatibilis fájlrendszer hello fürt hello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="503e4-186">This path is hello root of hello HDFS compatible file system for hello cluster.</span></span> 

> [!NOTE]
> <span data-ttu-id="503e4-187">Hello `Get-AzureRmHDInsightCluster` parancsmag által biztosított [Azure PowerShell](/powershell/azure/overview) is vissza hello tárolással kapcsolatos hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="503e4-187">hello `Get-AzureRmHDInsightCluster` cmdlet provided by [Azure PowerShell](/powershell/azure/overview) also returns hello storage information for hello cluster.</span></span>


## <a name="example-get-configuration"></a><span data-ttu-id="503e4-188">Példa: Get-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="503e4-188">Example: Get configuration</span></span>

1. <span data-ttu-id="503e4-189">A fürt számára rendelkezésre álló hello konfigurációk beolvasása.</span><span class="sxs-lookup"><span data-stu-id="503e4-189">Get hello configurations that are available for your cluster.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    <span data-ttu-id="503e4-190">Ebben a példában egy JSON-dokumentum hello aktuális konfigurációs tartalmazó adja vissza (hello által azonosított *címke* érték) hello összetevők hello fürtön telepítve.</span><span class="sxs-lookup"><span data-stu-id="503e4-190">This example returns a JSON document containing hello current configuration (identified by hello *tag* value) for hello components installed on hello cluster.</span></span> <span data-ttu-id="503e4-191">hello következő példa egy fájlból egy Spark-fürt típusa által visszaadott hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="503e4-191">hello following example is an excerpt from hello data returned from a Spark cluster type.</span></span>
   
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

2. <span data-ttu-id="503e4-192">Hello összetevő, amely érdekli hello konfigurációjának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="503e4-192">Get hello configuration for hello component that you are interested in.</span></span> <span data-ttu-id="503e4-193">Hello a következő példában cserélje le `INITIAL` hello előző kérelem által visszaadott hello címke értékkel.</span><span class="sxs-lookup"><span data-stu-id="503e4-193">In hello following example, replace `INITIAL` with hello tag value returned from hello previous request.</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    <span data-ttu-id="503e4-194">Ez a példa adja vissza egy JSON-dokumentum hello hello vonatkozó aktuális konfigurációját tartalmazó `core-site` összetevő.</span><span class="sxs-lookup"><span data-stu-id="503e4-194">This example returns a JSON document containing hello current configuration for hello `core-site` component.</span></span>

## <a name="example-update-configuration"></a><span data-ttu-id="503e4-195">Példa: Konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="503e4-195">Example: Update configuration</span></span>

1. <span data-ttu-id="503e4-196">Töltse le a hello jelenlegi konfigurációt, amely Ambari hello "kívánt konfiguráció" tárolja:</span><span class="sxs-lookup"><span data-stu-id="503e4-196">Get hello current configuration, which Ambari stores as hello "desired configuration":</span></span>

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    <span data-ttu-id="503e4-197">Ebben a példában egy JSON-dokumentum hello aktuális konfigurációs tartalmazó adja vissza (hello által azonosított *címke* érték) hello összetevők hello fürtön telepítve.</span><span class="sxs-lookup"><span data-stu-id="503e4-197">This example returns a JSON document containing hello current configuration (identified by hello *tag* value) for hello components installed on hello cluster.</span></span> <span data-ttu-id="503e4-198">hello következő példa egy fájlból egy Spark-fürt típusa által visszaadott hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="503e4-198">hello following example is an excerpt from hello data returned from a Spark cluster type.</span></span>
   
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
   
    <span data-ttu-id="503e4-199">Ebből a listából kell hello összetevő toocopy hello neve (például **spark\_thrift\_sparkconf** és hello **címke** érték.</span><span class="sxs-lookup"><span data-stu-id="503e4-199">From this list, you need toocopy hello name of hello component (for example, **spark\_thrift\_sparkconf** and hello **tag** value.</span></span>

2. <span data-ttu-id="503e4-200">Lekéri a hello összetevő és a címke hello konfigurálása hello a következő parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="503e4-200">Retrieve hello configuration for hello component and tag by using hello following commands:</span></span>
   
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
    > <span data-ttu-id="503e4-201">Cserélje le **spark-thrift-sparkconf** és **kezdeti** hello összetevő és tooretrieve hello konfigurációja kívánt címkét.</span><span class="sxs-lookup"><span data-stu-id="503e4-201">Replace **spark-thrift-sparkconf** and **INITIAL** with hello component and tag that you want tooretrieve hello configuration for.</span></span>
   
    <span data-ttu-id="503e4-202">Jq használt tooturn hello származó adatok HDInsight új konfigurációs sablonba.</span><span class="sxs-lookup"><span data-stu-id="503e4-202">Jq is used tooturn hello data retrieved from HDInsight into a new configuration template.</span></span> <span data-ttu-id="503e4-203">Pontosabban ezek a példák hajtsa végre a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="503e4-203">Specifically, these examples perform hello following actions:</span></span>
   
    * <span data-ttu-id="503e4-204">Létrehoz egy egyedi értéket tartalmazó hello karakterlánc "version" és a hello dátum, ami a `newtag`.</span><span class="sxs-lookup"><span data-stu-id="503e4-204">Creates a unique value containing hello string "version" and hello date, which is stored in `newtag`.</span></span>

    * <span data-ttu-id="503e4-205">Az új szükségeskonfiguráció hello legfelső szintű dokumentum létrehozása.</span><span class="sxs-lookup"><span data-stu-id="503e4-205">Creates a root document for hello new desired configuration.</span></span>

    * <span data-ttu-id="503e4-206">Lekérdezi hello hello tartalmát `.items[]` tömb, és hozzáadja azt a hello **desired_config** elemet.</span><span class="sxs-lookup"><span data-stu-id="503e4-206">Gets hello contents of hello `.items[]` array and adds it under hello **desired_config** element.</span></span>

    * <span data-ttu-id="503e4-207">Törlések hello `href`, `version`, és `Config` elemek, ezeket az elemeket, nem szükséges toosubmit egy új konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="503e4-207">Deletes hello `href`, `version`, and `Config` elements, as these elements aren't needed toosubmit a new configuration.</span></span>

    * <span data-ttu-id="503e4-208">Hozzáad egy `tag` elem értéke az `version#################`.</span><span class="sxs-lookup"><span data-stu-id="503e4-208">Adds a `tag` element with a value of `version#################`.</span></span> <span data-ttu-id="503e4-209">hello numerikus részét alapul hello aktuális dátumot.</span><span class="sxs-lookup"><span data-stu-id="503e4-209">hello numeric portion is based on hello current date.</span></span> <span data-ttu-id="503e4-210">Minden egyes-konfiguráció egyedi kódot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="503e4-210">Each configuration must have a unique tag.</span></span>
     
    <span data-ttu-id="503e4-211">Végezetül hello mentett adatok toohello `newconfig.json` dokumentum.</span><span class="sxs-lookup"><span data-stu-id="503e4-211">Finally, hello data is saved toohello `newconfig.json` document.</span></span> <span data-ttu-id="503e4-212">hello dokumentumstruktúrával megjelenjen-e a következő példa hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="503e4-212">hello document structure should appear similar toohello following example:</span></span>
     
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

3. <span data-ttu-id="503e4-213">Nyissa meg hello `newconfig.json` hello dokumentum és a módosítása vagy hozzáadása értékek `properties` objektum.</span><span class="sxs-lookup"><span data-stu-id="503e4-213">Open hello `newconfig.json` document and modify/add values in hello `properties` object.</span></span> <span data-ttu-id="503e4-214">hello változásai példa hello értékének `"spark.yarn.am.memory"` a `"1g"` túl`"3g"`.</span><span class="sxs-lookup"><span data-stu-id="503e4-214">hello following example changes hello value of `"spark.yarn.am.memory"` from `"1g"` too`"3g"`.</span></span> <span data-ttu-id="503e4-215">Bővíti ezenkívül `"spark.kryoserializer.buffer.max"` értékkel rendelkező `"256m"`.</span><span class="sxs-lookup"><span data-stu-id="503e4-215">It also adds `"spark.kryoserializer.buffer.max"` with a value of `"256m"`.</span></span>
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    <span data-ttu-id="503e4-216">Ha elkészült, hogy a módosításokat, mentse a hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="503e4-216">Save hello file once you are done making modifications.</span></span>

4. <span data-ttu-id="503e4-217">A következő parancsok toosubmit hello frissített konfigurációs tooAmbari hello használata.</span><span class="sxs-lookup"><span data-stu-id="503e4-217">Use hello following commands toosubmit hello updated configuration tooAmbari.</span></span>
   
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
   
    <span data-ttu-id="503e4-218">Ezek a parancsok nyújt hello hello tartalmát **newconfig.json** toohello fürt fájlt, hello új szükségeskonfiguráció-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="503e4-218">These commands submit hello contents of hello **newconfig.json** file toohello cluster as hello new desired configuration.</span></span> <span data-ttu-id="503e4-219">hello kérelem JSON-dokumentum adja vissza.</span><span class="sxs-lookup"><span data-stu-id="503e4-219">hello request returns a JSON document.</span></span> <span data-ttu-id="503e4-220">Hello **versionTag** elem ebben a dokumentumban meg kell felelnie a hello verzió elküldése megtörtént, és hello **configs** objektum tartalmazza a kért hello konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="503e4-220">hello **versionTag** element in this document should match hello version you submitted, and hello **configs** object contains hello configuration changes you requested.</span></span>

### <a name="example-restart-a-service-component"></a><span data-ttu-id="503e4-221">Példa: Indítsa újra a szolgáltatás valamelyik összetevője</span><span class="sxs-lookup"><span data-stu-id="503e4-221">Example: Restart a service component</span></span>

<span data-ttu-id="503e4-222">Ezen a ponton hello Ambari webes felhasználói felület tekinti meg, ha hello Spark szolgáltatást azt jelzi, hogy kell-e toobe hello új konfiguráció életbe léptetéséhez újra.</span><span class="sxs-lookup"><span data-stu-id="503e4-222">At this point, if you look at hello Ambari web UI, hello Spark service indicates that it needs toobe restarted before hello new configuration can take effect.</span></span> <span data-ttu-id="503e4-223">A következő lépéseket toorestart hello szolgáltatás hello használata.</span><span class="sxs-lookup"><span data-stu-id="503e4-223">Use hello following steps toorestart hello service.</span></span>

1. <span data-ttu-id="503e4-224">A következő tooenable karbantartási mód hello Spark szolgáltatást hello használata:</span><span class="sxs-lookup"><span data-stu-id="503e4-224">Use hello following tooenable maintenance mode for hello Spark service:</span></span>

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
   
    <span data-ttu-id="503e4-225">Ezek a parancsok küldése egy JSON dokumentum toohello kiszolgáló, amely bekapcsolja a karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="503e4-225">These commands send a JSON document toohello server that turns on maintenance mode.</span></span> <span data-ttu-id="503e4-226">Ellenőrizheti, hogy hello szolgáltatás most már karbantartási módban van hello kérelem a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="503e4-226">You can verify that hello service is now in maintenance mode using hello following request:</span></span>
   
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
   
    <span data-ttu-id="503e4-227">hello visszatérési értéke `ON`.</span><span class="sxs-lookup"><span data-stu-id="503e4-227">hello return value is `ON`.</span></span>

2. <span data-ttu-id="503e4-228">Következő lépésként az hello tooturn ki hello szolgáltatást a következő:</span><span class="sxs-lookup"><span data-stu-id="503e4-228">Next, use hello following tooturn off hello service:</span></span>

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
    
    <span data-ttu-id="503e4-229">a rendszer a következő példa hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="503e4-229">hello response is similar toohello following example:</span></span>
   
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
    > <span data-ttu-id="503e4-230">Hello `href` ezt az URI által visszaadott érték hello fürtcsomópont hello belső IP-címét használja.</span><span class="sxs-lookup"><span data-stu-id="503e4-230">hello `href` value returned by this URI is using hello internal IP address of hello cluster node.</span></span> <span data-ttu-id="503e4-231">toouse hello hello-fürt külső hello fürtből, akkor hello "10.0.0.18:8080" részét cserélje le.</span><span class="sxs-lookup"><span data-stu-id="503e4-231">toouse it from outside hello cluster, replace hello \`10.0.0.18:8080' portion with hello FQDN of hello cluster.</span></span> 
    
    <span data-ttu-id="503e4-232">a következő parancsok hello hello kérelem hello állapotának beolvasása:</span><span class="sxs-lookup"><span data-stu-id="503e4-232">hello following commands retrieve hello status of hello request:</span></span>

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

    <span data-ttu-id="503e4-233">A válasz `COMPLETED` azt jelzi, hogy adott hello kérelem befejeződött.</span><span class="sxs-lookup"><span data-stu-id="503e4-233">A response of `COMPLETED` indicates that hello request has finished.</span></span>

3. <span data-ttu-id="503e4-234">Hello előző kérelem után használja a következő toostart hello szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="503e4-234">Once hello previous request completes, use hello following toostart hello service.</span></span>
   
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
    <span data-ttu-id="503e4-235">hello szolgáltatás hello új konfigurációt használ.</span><span class="sxs-lookup"><span data-stu-id="503e4-235">hello service is now using hello new configuration.</span></span>

4. <span data-ttu-id="503e4-236">Végezetül használja a következő tooturn ki a karbantartási módot hello.</span><span class="sxs-lookup"><span data-stu-id="503e4-236">Finally, use hello following tooturn off maintenance mode.</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="503e4-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="503e4-237">Next steps</span></span>

<span data-ttu-id="503e4-238">Hello REST API-t a teljes referenciáért lásd: [Ambari API-referencia V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="503e4-238">For a complete reference of hello REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

