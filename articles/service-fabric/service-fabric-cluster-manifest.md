---
title: "Az Azure Service Fabric önálló fürt konfigurálása |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az önálló vagy titkos Service Fabric-fürt."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dekapur
ms.openlocfilehash: 9885dce18dabac4a945dafd219e3ae190e34a83b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a><span data-ttu-id="063be-103">Önálló Windows-fürt konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="063be-103">Configuration settings for standalone Windows cluster</span></span>
<span data-ttu-id="063be-104">Ez a cikk ismerteti, hogyan konfigurálhatja egy különálló Service Fabric fürt használatával a ***művelet*** fájlt.</span><span class="sxs-lookup"><span data-stu-id="063be-104">This article describes how to configure a standalone Service Fabric cluster using the ***ClusterConfig.JSON*** file.</span></span> <span data-ttu-id="063be-105">Ez a fájl határozza meg a Service Fabric-csomópont és az IP-címek, a csomópontok különböző típusú vonatkozó információkat a fürt, a biztonsági beállításokkal, valamint a hálózati topológia hiba/frissítési tartományok tekintetében az önálló fürthöz használható.</span><span class="sxs-lookup"><span data-stu-id="063be-105">You can use this file to specify information such as the Service Fabric nodes and their IP addresses, different types of nodes on the cluster, the security configurations as well as the network topology in terms of fault/upgrade domains, for your standalone cluster.</span></span>

<span data-ttu-id="063be-106">Ha Ön [a különálló Service Fabric-csomag](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), a művelet fájl néhány minták töltődnek le a munkahelyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="063be-106">When you [download the standalone Service Fabric package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), a few samples of the ClusterConfig.JSON file are downloaded to your work machine.</span></span> <span data-ttu-id="063be-107">A minták *DevCluster* útra segít hozzon létre egy fürtöt ugyanazon a számítógépen, például a logikai csomópontok három csomópontjaihoz.</span><span class="sxs-lookup"><span data-stu-id="063be-107">The samples having *DevCluster* in their names will help you create a cluster with all three nodes on the same machine, like logical nodes.</span></span> <span data-ttu-id="063be-108">Ezeken kívül legalább egy csomópont jelölésűnek kell lennie egy elsődleges csomóponton.</span><span class="sxs-lookup"><span data-stu-id="063be-108">Out of these, at least one node must be marked as a primary node.</span></span> <span data-ttu-id="063be-109">A fürt akkor hasznos, ha egy fejlesztési vagy tesztelési környezetben, és egy éles fürt nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="063be-109">This cluster is useful for a development or test environment and is not supported as a production cluster.</span></span> <span data-ttu-id="063be-110">A minták *MultiMachine* a név segítségével hozhat létre egy éles minőségi fürt minden csomópont egy külön számítógépen.</span><span class="sxs-lookup"><span data-stu-id="063be-110">The samples having *MultiMachine* in their names, will help you create a production quality cluster, with each node on a separate machine.</span></span>

1. <span data-ttu-id="063be-111">*ClusterConfig.Unsecure.DevCluster.JSON* és *ClusterConfig.Unsecure.MultiMachine.JSON* egy nem biztonságos teszt- vagy éles fürt rendre létrehozását mutatják be.</span><span class="sxs-lookup"><span data-stu-id="063be-111">*ClusterConfig.Unsecure.DevCluster.JSON* and *ClusterConfig.Unsecure.MultiMachine.JSON* show how to create an unsecured test or production cluster respectively.</span></span> 
2. <span data-ttu-id="063be-112">*ClusterConfig.Windows.DevCluster.JSON* és *ClusterConfig.Windows.MultiMachine.JSON* használatával biztonságossá teszt- vagy éles fürt létrehozását mutatják be [Windows biztonsági](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="063be-112">*ClusterConfig.Windows.DevCluster.JSON* and  *ClusterConfig.Windows.MultiMachine.JSON* show how to create test or production cluster, secured using [Windows security](service-fabric-windows-cluster-windows-security.md).</span></span>
3. <span data-ttu-id="063be-113">*ClusterConfig.X509.DevCluster.JSON* és *ClusterConfig.X509.MultiMachine.JSON* használatával biztonságossá teszt- vagy éles fürt létrehozását mutatják be [X509-alapú biztonsági](service-fabric-windows-cluster-x509-security.md).</span><span class="sxs-lookup"><span data-stu-id="063be-113">*ClusterConfig.X509.DevCluster.JSON* and *ClusterConfig.X509.MultiMachine.JSON* show how to create test or production cluster, secured using [X509 certificate-based security](service-fabric-windows-cluster-x509-security.md).</span></span> 

<span data-ttu-id="063be-114">Most azt megvizsgálja, hogy a különböző részeit egy ***művelet*** alábbi fájlt.</span><span class="sxs-lookup"><span data-stu-id="063be-114">Now we will examine the various sections of a ***ClusterConfig.JSON*** file as below.</span></span>

## <a name="general-cluster-configurations"></a><span data-ttu-id="063be-115">Általános fürtkonfigurációk</span><span class="sxs-lookup"><span data-stu-id="063be-115">General cluster configurations</span></span>
<span data-ttu-id="063be-116">Ez hozzá van rendelve a széles körű adott fürtkonfigurációk, ahogy az az alábbi JSON-részlet.</span><span class="sxs-lookup"><span data-stu-id="063be-116">This covers the broad cluster specific configurations, as shown in the JSON snippet below.</span></span>

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

<span data-ttu-id="063be-117">A Service Fabric-fürt bármely rövid nevet a való hozzárendelésével biztosíthat a **neve** változó.</span><span class="sxs-lookup"><span data-stu-id="063be-117">You can give any friendly name to your Service Fabric cluster by assigning it to the **name** variable.</span></span> <span data-ttu-id="063be-118">A **clusterConfigurationVersion** a fürt; a verziószám növelése kell azt minden alkalommal, amikor a Service Fabric-fürt frissítése.</span><span class="sxs-lookup"><span data-stu-id="063be-118">The **clusterConfigurationVersion** is the version number of your cluster; you should increase it every time you upgrade your Service Fabric cluster.</span></span> <span data-ttu-id="063be-119">Azonban hagyja meg az **apiVersion** az alapértelmezett értékre.</span><span class="sxs-lookup"><span data-stu-id="063be-119">You should however leave the **apiVersion** to the default value.</span></span>

<a id="clusternodes"></a>

## <a name="nodes-on-the-cluster"></a><span data-ttu-id="063be-120">A fürt csomópontjai</span><span class="sxs-lookup"><span data-stu-id="063be-120">Nodes on the cluster</span></span>
<span data-ttu-id="063be-121">Konfigurálhatja a csomópontok a Service Fabric-fürt használatával a **csomópontok** területen az alábbi kódrészletben látható módon.</span><span class="sxs-lookup"><span data-stu-id="063be-121">You can configure the nodes on your Service Fabric cluster by using the **nodes** section, as the following snippet shows.</span></span>

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

<span data-ttu-id="063be-122">A Service Fabric-fürt tartalmaznia kell legalább 3 csomópontok.</span><span class="sxs-lookup"><span data-stu-id="063be-122">A Service Fabric cluster must contain at least 3 nodes.</span></span> <span data-ttu-id="063be-123">A telepítő szerint több csomópont is hozzáadható ehhez a szakaszhoz.</span><span class="sxs-lookup"><span data-stu-id="063be-123">You can add more nodes to this section as per your setup.</span></span> <span data-ttu-id="063be-124">Az alábbi táblázat ismerteti az egyes csomópontok konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="063be-124">The following table explains the configuration settings for each node.</span></span>

| <span data-ttu-id="063be-125">**A csomópont-konfiguráció**</span><span class="sxs-lookup"><span data-stu-id="063be-125">**Node configuration**</span></span> | <span data-ttu-id="063be-126">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="063be-126">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="063be-127">Csomópontnév</span><span class="sxs-lookup"><span data-stu-id="063be-127">nodeName</span></span> |<span data-ttu-id="063be-128">Bármilyen rövid nevet adhat a csomópontra.</span><span class="sxs-lookup"><span data-stu-id="063be-128">You can give any friendly name to the node.</span></span> |
| <span data-ttu-id="063be-129">IP-cím</span><span class="sxs-lookup"><span data-stu-id="063be-129">iPAddress</span></span> |<span data-ttu-id="063be-130">Nyisson meg egy parancsablakot, és írja be az IP-cím, a csomópont található `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="063be-130">Find out the IP address of your node by opening a command window and typing `ipconfig`.</span></span> <span data-ttu-id="063be-131">Vegye figyelembe az IPV4-címet, majd rendelje hozzá a **IP-cím** változó.</span><span class="sxs-lookup"><span data-stu-id="063be-131">Note the IPV4 address and assign it to the **iPAddress** variable.</span></span> |
| <span data-ttu-id="063be-132">nodeTypeRef</span><span class="sxs-lookup"><span data-stu-id="063be-132">nodeTypeRef</span></span> |<span data-ttu-id="063be-133">Minden csomópont rendelhetők hozzá másik csomóponttípus.</span><span class="sxs-lookup"><span data-stu-id="063be-133">Each node can be assigned a different node type.</span></span> <span data-ttu-id="063be-134">A [csomóponttípusok](#nodetypes) határozzák meg a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="063be-134">The [node types](#nodetypes) are defined in the section below.</span></span> |
| <span data-ttu-id="063be-135">faultDomain</span><span class="sxs-lookup"><span data-stu-id="063be-135">faultDomain</span></span> |<span data-ttu-id="063be-136">Tartalék tartományok lehetővé teszik a rendszergazdák fürtön határozza meg a fizikai csomópontok, amelyek egyszerre megosztott fizikai függőségek miatt meghiúsulhat.</span><span class="sxs-lookup"><span data-stu-id="063be-136">Fault domains enable cluster administrators to define the physical nodes that might fail at the same time due to shared physical dependencies.</span></span> |
| <span data-ttu-id="063be-137">upgradeDomain</span><span class="sxs-lookup"><span data-stu-id="063be-137">upgradeDomain</span></span> |<span data-ttu-id="063be-138">Frissítési tartományok jellemezhető a csomópontokra, amelyeket nagyjából egy időben, a Service Fabric-frissítések állnak le.</span><span class="sxs-lookup"><span data-stu-id="063be-138">Upgrade domains describe sets of nodes that are shut down for Service Fabric upgrades at about the same time.</span></span> <span data-ttu-id="063be-139">Melyik frissítési tartományok hozzárendelése csomópontok dönthet úgy, mint bármely fizikai követelmények nem korlátozza.</span><span class="sxs-lookup"><span data-stu-id="063be-139">You can choose which nodes to assign to which Upgrade domains, as they are not limited by any physical requirements.</span></span> |

## <a name="cluster-properties"></a><span data-ttu-id="063be-140">Fürt tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="063be-140">Cluster properties</span></span>
<span data-ttu-id="063be-141">A **tulajdonságok** a művelet a szakasz a fürt az alábbiak szerint konfigurálására szolgál.</span><span class="sxs-lookup"><span data-stu-id="063be-141">The **properties** section in the ClusterConfig.JSON is used to configure the cluster as follows.</span></span>

<a id="reliability"></a>

### <a name="reliability"></a><span data-ttu-id="063be-142">Megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="063be-142">Reliability</span></span>
<span data-ttu-id="063be-143">A fogalom **reliabilityLevel** replikák száma vagy szolgáltatáspéldánynak a Service Fabric rendszer futtatható a fürt elsődleges csomópontjait határoz meg.</span><span class="sxs-lookup"><span data-stu-id="063be-143">The concept of **reliabilityLevel** defines the number of replicas or instances of the Service Fabric system services that can run on the primary nodes of the cluster.</span></span> <span data-ttu-id="063be-144">Meghatározza, hogy ezek a szolgáltatások megbízhatóságát, így a fürt.</span><span class="sxs-lookup"><span data-stu-id="063be-144">It determines the reliability of these services and hence the cluster.</span></span> <span data-ttu-id="063be-145">A rendszer által számított értéke fürt létrehozása és frissítése során.</span><span class="sxs-lookup"><span data-stu-id="063be-145">The value is calcuated by the system at cluster creation and upgrade time.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="063be-146">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="063be-146">Diagnostics</span></span>
<span data-ttu-id="063be-147">A **diagnosticsStore** szakasz lehetővé teszi a diagnosztika és a hibaelhárítási csomópont vagy a fürt hibák engedélyezése paramétereinek a konfigurálása, ahogy az az alábbi kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="063be-147">The **diagnosticsStore** section allows you to configure parameters to enable diagnostics and troubleshooting node or cluster failures, as shown in the following snippet.</span></span> 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

<span data-ttu-id="063be-148">A **metaadatok** a fürt diagnosztika leírása és a telepítő szerint állítható be.</span><span class="sxs-lookup"><span data-stu-id="063be-148">The **metadata** is a description of your cluster diagnostics and can be set as per your setup.</span></span> <span data-ttu-id="063be-149">Ezek a változók megkönnyíti a ETW nyomkövetési napló- és összeomlási memóriaképeket, valamint teljesítményszámlálók gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="063be-149">These variables help in collecting ETW trace logs, crash dumps as well as performance counters.</span></span> <span data-ttu-id="063be-150">Olvasási [a követési naplóban](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) és [ETW-nyomkövetés](https://msdn.microsoft.com/library/ms751538.aspx) ETW-vel további információ a nyomkövetési naplóit.</span><span class="sxs-lookup"><span data-stu-id="063be-150">Read [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) and [ETW Tracing](https://msdn.microsoft.com/library/ms751538.aspx) for more information on ETW trace logs.</span></span> <span data-ttu-id="063be-151">Beleértve az összes napló [összeomlási memóriaképek](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) és [teljesítményszámlálók](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) irányítható a **connectionString** mappát a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="063be-151">All logs including [Crash dumps](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) and [performance counters](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) can be directed to the **connectionString** folder on your machine.</span></span> <span data-ttu-id="063be-152">Is *AzureStorage* diagnosztika tárolásához.</span><span class="sxs-lookup"><span data-stu-id="063be-152">You can also use *AzureStorage* for storing diagnostics.</span></span> <span data-ttu-id="063be-153">Lentebb egy minta részlet.</span><span class="sxs-lookup"><span data-stu-id="063be-153">See below for a sample snippet.</span></span>

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a><span data-ttu-id="063be-154">Biztonság</span><span class="sxs-lookup"><span data-stu-id="063be-154">Security</span></span>
<span data-ttu-id="063be-155">A **biztonsági** szakasz esetén szükség a biztonságos különálló Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="063be-155">The **security** section is necessary for a secure standalone Service Fabric cluster.</span></span> <span data-ttu-id="063be-156">Az alábbi kódrészletben láthatja az ebben a szakaszban egy részét.</span><span class="sxs-lookup"><span data-stu-id="063be-156">The following snippet shows a part of this section.</span></span>

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

<span data-ttu-id="063be-157">A **metaadatok** a biztonságos fürt leírása és a telepítő szerint állítható be.</span><span class="sxs-lookup"><span data-stu-id="063be-157">The **metadata** is a description of your secure cluster and can be set as per your setup.</span></span> <span data-ttu-id="063be-158">A **ClusterCredentialType** és **ServerCredentialType** határozzák meg, hogy a fürt és a csomópontok megvalósítandó biztonsági típusú.</span><span class="sxs-lookup"><span data-stu-id="063be-158">The **ClusterCredentialType** and **ServerCredentialType** determine the type of security that the cluster and the nodes will implement.</span></span> <span data-ttu-id="063be-159">Akkor lehet megadni *X509* egy tanúsítványalapú biztonsági vagy *Windows* egy Azure Active Directory-alapú biztonság.</span><span class="sxs-lookup"><span data-stu-id="063be-159">They can be set to either *X509* for a certificate-based security, or *Windows* for an Azure Active Directory-based security.</span></span> <span data-ttu-id="063be-160">A többi a **biztonsági** szakasz a biztonsági típusú alapul.</span><span class="sxs-lookup"><span data-stu-id="063be-160">The rest of the **security** section will be based on the type of the security.</span></span> <span data-ttu-id="063be-161">Olvasási [tanúsítványok-alapú biztonsági önálló fürtben](service-fabric-windows-cluster-x509-security.md) vagy [Windows biztonsági önálló fürtben](service-fabric-windows-cluster-windows-security.md) adja meg a többi olvashat a **biztonsági** a szakasz.</span><span class="sxs-lookup"><span data-stu-id="063be-161">Read [Certificates-based security in a standalone cluster](service-fabric-windows-cluster-x509-security.md) or [Windows security in a standalone cluster](service-fabric-windows-cluster-windows-security.md) for information on how to fill out the rest of the **security** section.</span></span>

<a id="nodetypes"></a>

### <a name="node-types"></a><span data-ttu-id="063be-162">Csomóponttípusok</span><span class="sxs-lookup"><span data-stu-id="063be-162">Node Types</span></span>
<span data-ttu-id="063be-163">A **NodeType tulajdonságok értéke** szakasz ismerteti, amely rendelkezik a fürt a csomópontok típusú.</span><span class="sxs-lookup"><span data-stu-id="063be-163">The **nodeTypes** section describes the type of the nodes that your cluster has.</span></span> <span data-ttu-id="063be-164">Fürt esetén kell adni legalább egy csomópont típusát, ahogy az alábbi részlet.</span><span class="sxs-lookup"><span data-stu-id="063be-164">At least one node type must be specified for a cluster, as shown in the snippet below.</span></span> 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

<span data-ttu-id="063be-165">A **neve** az adott csomópont ilyen rövid neve.</span><span class="sxs-lookup"><span data-stu-id="063be-165">The **name** is the friendly name for this particular node type.</span></span> <span data-ttu-id="063be-166">A csomópont típusú csomópont létrehozása, hozzárendelése rövid nevét, hogy a **nodeTypeRef** változó az adott csomópont, ahogy azt korábban említettük [fent](#clusternodes).</span><span class="sxs-lookup"><span data-stu-id="063be-166">To create a node of this node type, assign its friendly name to the **nodeTypeRef** variable for that node, as mentioned [above](#clusternodes).</span></span> <span data-ttu-id="063be-167">Az egyes csomópont adja meg a kapcsolati végpontok használható.</span><span class="sxs-lookup"><span data-stu-id="063be-167">For each node type, define the connection endpoints that will be used.</span></span> <span data-ttu-id="063be-168">Minden kapcsolat végpontokkal portszáma dönthet úgy, mindaddig, amíg azok nem ütköznek-e a fürt bármely más végpontja.</span><span class="sxs-lookup"><span data-stu-id="063be-168">You can choose any port number for these connection endpoints, as long as they do not conflict with any other endpoints in this cluster.</span></span> <span data-ttu-id="063be-169">Több csomópontos fürtben, lesz egy vagy több elsődleges csomóponton (pl. **isPrimary** beállítása *igaz*) attól függően, hogy a [ **reliabilityLevel** ](#reliability).</span><span class="sxs-lookup"><span data-stu-id="063be-169">In a multi-node cluster, there will be one or more primary nodes (i.e. **isPrimary** set to *true*), depending on the [**reliabilityLevel**](#reliability).</span></span> <span data-ttu-id="063be-170">Olvasási [Service Fabric fürt kapacitástervezésének szempontjai](service-fabric-cluster-capacity.md) információk **NodeType tulajdonságok értéke** és **reliabilityLevel**, és hogy tudják, mit elsődleges és a nem elsődleges csomóponttípusok.</span><span class="sxs-lookup"><span data-stu-id="063be-170">Read [Service Fabric cluster capacity planning considerations](service-fabric-cluster-capacity.md) for information on **nodeTypes** and **reliabilityLevel**, and to know what are primary and the non-primary node types.</span></span> 

#### <a name="endpoints-used-to-configure-the-node-types"></a><span data-ttu-id="063be-171">A csomóponttípusok konfigurálásához használt végpontok</span><span class="sxs-lookup"><span data-stu-id="063be-171">Endpoints used to configure the node types</span></span>
* <span data-ttu-id="063be-172">*clientConnectionEndpointPort* az kapcsolódik a fürthöz, az ügyfél API-k használata esetén az ügyfél által használt port.</span><span class="sxs-lookup"><span data-stu-id="063be-172">*clientConnectionEndpointPort* is the port used by the client to connect to the cluster, when using the client APIs.</span></span> 
* <span data-ttu-id="063be-173">*clusterConnectionEndpointPort* a portot, amelyen a csomópontok kommunikálnak egymással.</span><span class="sxs-lookup"><span data-stu-id="063be-173">*clusterConnectionEndpointPort* is the port at which the nodes communicate with each other.</span></span>
* <span data-ttu-id="063be-174">*leaseDriverEndpointPort* a portot, ha szeretné tudni, ha a csomópontok még mindig aktív a fürt bérleti illesztőprogram használják.</span><span class="sxs-lookup"><span data-stu-id="063be-174">*leaseDriverEndpointPort* is the port used by the cluster lease driver to find out if the nodes are still active.</span></span> 
* <span data-ttu-id="063be-175">*serviceConnectionEndpointPort* a csomóponton való kommunikációhoz, hogy adott csomóponton a Service Fabric-ügyféllel telepített szolgáltatások és az alkalmazások által használt port.</span><span class="sxs-lookup"><span data-stu-id="063be-175">*serviceConnectionEndpointPort* is the port used by the applications and services deployed on a node, to communicate with the Service Fabric client on that particular node.</span></span>
* <span data-ttu-id="063be-176">*httpGatewayEndpointPort* a portot használják a Service Fabric Explorerben csatlakozzon a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="063be-176">*httpGatewayEndpointPort* is the port used by the Service Fabric Explorer to connect to the cluster.</span></span>
* <span data-ttu-id="063be-177">*ephemeralPorts* bírálja felül a [az operációs rendszer által használt dinamikus portok](https://support.microsoft.com/kb/929851).</span><span class="sxs-lookup"><span data-stu-id="063be-177">*ephemeralPorts* override the [dynamic ports used by the OS](https://support.microsoft.com/kb/929851).</span></span> <span data-ttu-id="063be-178">A Service Fabric ezek alkalmazás portok részét fogja használni, és a fennmaradó lesznek elérhetők az operációs rendszer számára.</span><span class="sxs-lookup"><span data-stu-id="063be-178">Service Fabric will use a part of these as application ports and the remaining will be available for the OS.</span></span> <span data-ttu-id="063be-179">Azt is felelteti meg ezt a tartományt a meglévő tartomány szerepel az operációs rendszer, így minden célra használhatja a minta JSON-fájlokat a megadott tartományokon.</span><span class="sxs-lookup"><span data-stu-id="063be-179">It will also map this range to the existing range present in the OS, so for all purposes, you can use the ranges given in the sample JSON files.</span></span> <span data-ttu-id="063be-180">Győződjön meg arról, hogy az a kezdő és záró portokat közötti különbség legalább 255 kell.</span><span class="sxs-lookup"><span data-stu-id="063be-180">You need to make sure that the difference between the start and the end ports is at least 255.</span></span> <span data-ttu-id="063be-181">Ha ezt a különbséget túl alacsony, mivel ez a tartomány meg van osztva az operációs rendszer futtathatja az ütközéseket.</span><span class="sxs-lookup"><span data-stu-id="063be-181">You may run into conflicts if this difference is too low, since this range is shared with the operating system.</span></span> <span data-ttu-id="063be-182">Tekintse meg a beállított dinamikus porttartományt futtatásával `netsh int ipv4 show dynamicport tcp`.</span><span class="sxs-lookup"><span data-stu-id="063be-182">See the configured dynamic port range by running `netsh int ipv4 show dynamicport tcp`.</span></span>
* <span data-ttu-id="063be-183">*applicationPorts* legyenek a, amely a Service Fabric-alkalmazások által használt portok.</span><span class="sxs-lookup"><span data-stu-id="063be-183">*applicationPorts* are the ports that will be used by the Service Fabric applications.</span></span> <span data-ttu-id="063be-184">Az alkalmazás porttartományát elég nagynak kell lennie, amelyek a végpont követelmény az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="063be-184">The application port range should be large enough to cover the endpoint requirement of your applications.</span></span> <span data-ttu-id="063be-185">Ebben a tartományban kell lennie azon a számítógépen, a dinamikus port tartományból kizárólagos azaz a *ephemeralPorts* tartomány, ahogyan az a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="063be-185">This range should be exclusive from the dynamic port range on the machine, i.e. the *ephemeralPorts* range as set in the configuration.</span></span>  <span data-ttu-id="063be-186">A Service Fabric fog használni, ezek új portok szükségesek, valamint gondoskodunk megnyitni ezeket a portokat a tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="063be-186">Service Fabric will use these whenever new ports are required, as well as take care of opening the firewall for these ports.</span></span> 
* <span data-ttu-id="063be-187">*reverseProxyEndpointPort* a választható fordított proxy végpontja.</span><span class="sxs-lookup"><span data-stu-id="063be-187">*reverseProxyEndpointPort* is an optional reverse proxy endpoint.</span></span> <span data-ttu-id="063be-188">Lásd: [Service Fabric fordított Proxy](service-fabric-reverseproxy.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="063be-188">See [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) for more details.</span></span> 

### <a name="log-settings"></a><span data-ttu-id="063be-189">Naplózási beállítások</span><span class="sxs-lookup"><span data-stu-id="063be-189">Log Settings</span></span>
<span data-ttu-id="063be-190">A **fabricSettings** szakasz lehetővé teszi, hogy meg kell adnia a gyökérkönyvtárak a Service Fabric-adatokat és a naplókat.</span><span class="sxs-lookup"><span data-stu-id="063be-190">The **fabricSettings** section allows you to set the root directories for the Service Fabric data and logs.</span></span> <span data-ttu-id="063be-191">Testre szabhatja ezek csak a kezdeti fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="063be-191">You can customize these only during the initial cluster creation.</span></span> <span data-ttu-id="063be-192">Ez a szakasz egy minta szövegrészletet lentebb.</span><span class="sxs-lookup"><span data-stu-id="063be-192">See below for a sample snippet of this section.</span></span>

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

<span data-ttu-id="063be-193">Azt javasoljuk, hogy a FabricDataRoot és a fabriclogroot mappában az operációs rendszer nem meghajtót használ, szemben az operációs rendszer további megbízhatóság összeomlik biztosít.</span><span class="sxs-lookup"><span data-stu-id="063be-193">We recommended using a non-OS drive as the FabricDataRoot and FabricLogRoot as it provides more reliability against OS crashes.</span></span> <span data-ttu-id="063be-194">Vegye figyelembe, hogy csak a adatgyökerében testre, majd a napló legfelső szintű kerülnek adatok gyökere alatt egy szinttel.</span><span class="sxs-lookup"><span data-stu-id="063be-194">Note that if you customize only the data root, then the log root will be placed one level below the data root.</span></span>

### <a name="stateful-reliable-service-settings"></a><span data-ttu-id="063be-195">Állapot-nyilvántartó megbízható szolgáltatás beállításai</span><span class="sxs-lookup"><span data-stu-id="063be-195">Stateful Reliable Service Settings</span></span>
<span data-ttu-id="063be-196">A **KtlLogger** szakasz lehetővé teszi, hogy meg kell adnia a Reliable Services globális konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="063be-196">The **KtlLogger** section allows you to set the global configuration settings for Reliable Services.</span></span> <span data-ttu-id="063be-197">Ezek a beállítások a további részletekért olvassa el a [állapot-nyilvántartó megbízható szolgáltatások konfigurálása](service-fabric-reliable-services-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="063be-197">For more details on these settings read [Configure stateful reliable services](service-fabric-reliable-services-configuration.md).</span></span>
<span data-ttu-id="063be-198">Az alábbi példa bemutatja, hogyan módosíthatja a biztonsági másolatot a megbízható gyűjtemények állapotalapú szolgáltatások jön létre a megosztott tranzakciónapló.</span><span class="sxs-lookup"><span data-stu-id="063be-198">The example below shows how to change the the shared transaction log that gets created to back any reliable collections for stateful services.</span></span>

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a><span data-ttu-id="063be-199">Bővítmény szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="063be-199">Add-on features</span></span>
<span data-ttu-id="063be-200">Bővítmény funkciók konfigurálására, a apiVersion konfigurált mint "04-2017' vagy újabb legyen, és addonFeatures kell megadni:</span><span class="sxs-lookup"><span data-stu-id="063be-200">To configure add-on features, the apiVersion should be configured as '04-2017' or higher, and addonFeatures needs to be configured:</span></span>

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a><span data-ttu-id="063be-201">Tároló-támogatás</span><span class="sxs-lookup"><span data-stu-id="063be-201">Container support</span></span>
<span data-ttu-id="063be-202">Ahhoz, hogy a windows server tároló és a hyper-v tároló önálló fürtök tároló támogatása, a "DnsService" bővítmény szolgáltatás engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="063be-202">To enable container support for both windows server container and hyper-v container for standalone clusters, the 'DnsService' add-on feature needs to be enabled.</span></span>


## <a name="next-steps"></a><span data-ttu-id="063be-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="063be-203">Next steps</span></span>
<span data-ttu-id="063be-204">Miután a különálló fürt beállítása szerint konfigurált teljes művelet fájlt, a cikk követve a fürtök telepítése [hozzon létre egy különálló Service Fabric-fürt](service-fabric-cluster-creation-for-windows-server.md) majd folytassa a [ a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="063be-204">Once you have a complete ClusterConfig.JSON file configured as per your standalone cluster setup, you can deploy your cluster by following the article [Create a standalone Service Fabric cluster](service-fabric-cluster-creation-for-windows-server.md) and then proceed to [visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

