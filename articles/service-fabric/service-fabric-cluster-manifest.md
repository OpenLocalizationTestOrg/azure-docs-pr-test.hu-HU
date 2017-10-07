---
title: "aaaConfigure az Azure Service Fabric önálló fürthöz |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure az önálló vagy titkos Service Fabric-fürt."
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
ms.openlocfilehash: ce2ad387162a05668bbd3a271c754776fe471850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a><span data-ttu-id="59d4e-103">Önálló Windows-fürt konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="59d4e-103">Configuration settings for standalone Windows cluster</span></span>
<span data-ttu-id="59d4e-104">Ez a cikk ismerteti, hogyan egy különálló Service Fabric fürt használt tooconfigure hello ***művelet*** fájlt.</span><span class="sxs-lookup"><span data-stu-id="59d4e-104">This article describes how tooconfigure a standalone Service Fabric cluster using hello ***ClusterConfig.JSON*** file.</span></span> <span data-ttu-id="59d4e-105">A fájl toospecify információt használhatja például hello Service Fabric-csomópont és az IP-címek, a csomópontok különböző típusú hello fürt, hello biztonsági beállításokkal, valamint hello hálózati topológia hiba/frissítési tartományok tekintetében a különálló fürt.</span><span class="sxs-lookup"><span data-stu-id="59d4e-105">You can use this file toospecify information such as hello Service Fabric nodes and their IP addresses, different types of nodes on hello cluster, hello security configurations as well as hello network topology in terms of fault/upgrade domains, for your standalone cluster.</span></span>

<span data-ttu-id="59d4e-106">Ha Ön [hello különálló Service Fabric-csomag](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), néhány hello művelet fájl mintákat letöltött tooyour munkahelyi gépet.</span><span class="sxs-lookup"><span data-stu-id="59d4e-106">When you [download hello standalone Service Fabric package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), a few samples of hello ClusterConfig.JSON file are downloaded tooyour work machine.</span></span> <span data-ttu-id="59d4e-107">hello minták *DevCluster* útra segít hozzon létre egy fürtöt azonos számítógépre, például a logikai csomópontok hello három csomópontjaihoz.</span><span class="sxs-lookup"><span data-stu-id="59d4e-107">hello samples having *DevCluster* in their names will help you create a cluster with all three nodes on hello same machine, like logical nodes.</span></span> <span data-ttu-id="59d4e-108">Ezeken kívül legalább egy csomópont jelölésűnek kell lennie egy elsődleges csomóponton.</span><span class="sxs-lookup"><span data-stu-id="59d4e-108">Out of these, at least one node must be marked as a primary node.</span></span> <span data-ttu-id="59d4e-109">A fürt akkor hasznos, ha egy fejlesztési vagy tesztelési környezetben, és egy éles fürt nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="59d4e-109">This cluster is useful for a development or test environment and is not supported as a production cluster.</span></span> <span data-ttu-id="59d4e-110">hello minták *MultiMachine* a név segítségével hozhat létre egy éles minőségi fürt minden csomópont egy külön számítógépen.</span><span class="sxs-lookup"><span data-stu-id="59d4e-110">hello samples having *MultiMachine* in their names, will help you create a production quality cluster, with each node on a separate machine.</span></span>

1. <span data-ttu-id="59d4e-111">*ClusterConfig.Unsecure.DevCluster.JSON* és *ClusterConfig.Unsecure.MultiMachine.JSON* hogyan toocreate egy nem biztonságos teszt- vagy éles fürt rendre megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="59d4e-111">*ClusterConfig.Unsecure.DevCluster.JSON* and *ClusterConfig.Unsecure.MultiMachine.JSON* show how toocreate an unsecured test or production cluster respectively.</span></span> 
2. <span data-ttu-id="59d4e-112">*ClusterConfig.Windows.DevCluster.JSON* és *ClusterConfig.Windows.MultiMachine.JSON* hogyan toocreate teszt- vagy éles fürt használatával biztonságossá téve megjelenítése [Windows biztonsági](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="59d4e-112">*ClusterConfig.Windows.DevCluster.JSON* and  *ClusterConfig.Windows.MultiMachine.JSON* show how toocreate test or production cluster, secured using [Windows security](service-fabric-windows-cluster-windows-security.md).</span></span>
3. <span data-ttu-id="59d4e-113">*ClusterConfig.X509.DevCluster.JSON* és *ClusterConfig.X509.MultiMachine.JSON* hogyan toocreate teszt- vagy éles fürt használatával biztonságossá téve megjelenítése [X509-alapú biztonsági](service-fabric-windows-cluster-x509-security.md).</span><span class="sxs-lookup"><span data-stu-id="59d4e-113">*ClusterConfig.X509.DevCluster.JSON* and *ClusterConfig.X509.MultiMachine.JSON* show how toocreate test or production cluster, secured using [X509 certificate-based security](service-fabric-windows-cluster-x509-security.md).</span></span> 

<span data-ttu-id="59d4e-114">Most azt megvizsgálja hello különböző részeit egy ***művelet*** alábbi fájlt.</span><span class="sxs-lookup"><span data-stu-id="59d4e-114">Now we will examine hello various sections of a ***ClusterConfig.JSON*** file as below.</span></span>

## <a name="general-cluster-configurations"></a><span data-ttu-id="59d4e-115">Általános fürtkonfigurációk</span><span class="sxs-lookup"><span data-stu-id="59d4e-115">General cluster configurations</span></span>
<span data-ttu-id="59d4e-116">Ez magában foglalja a hello széleskörű fürt specifikus konfigurációk, az alábbi hello JSON kódrészletben látható.</span><span class="sxs-lookup"><span data-stu-id="59d4e-116">This covers hello broad cluster specific configurations, as shown in hello JSON snippet below.</span></span>

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

<span data-ttu-id="59d4e-117">Rövid név tooyour Service Fabric-fürt toohello hozzárendelésével biztosíthat **neve** változó.</span><span class="sxs-lookup"><span data-stu-id="59d4e-117">You can give any friendly name tooyour Service Fabric cluster by assigning it toohello **name** variable.</span></span> <span data-ttu-id="59d4e-118">Hello **clusterConfigurationVersion** a fürt; hello verziószám növelése kell azt minden alkalommal, amikor a Service Fabric-fürt frissítése.</span><span class="sxs-lookup"><span data-stu-id="59d4e-118">hello **clusterConfigurationVersion** is hello version number of your cluster; you should increase it every time you upgrade your Service Fabric cluster.</span></span> <span data-ttu-id="59d4e-119">Azonban hagyja hello **apiVersion** toohello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="59d4e-119">You should however leave hello **apiVersion** toohello default value.</span></span>

<a id="clusternodes"></a>

## <a name="nodes-on-hello-cluster"></a><span data-ttu-id="59d4e-120">Hello fürtön található csomópontok</span><span class="sxs-lookup"><span data-stu-id="59d4e-120">Nodes on hello cluster</span></span>
<span data-ttu-id="59d4e-121">Konfigurálható hello csomópont a Service Fabric-fürt hello segítségével **csomópontok** szakaszban, mint a következő kódrészletet mutat be hello.</span><span class="sxs-lookup"><span data-stu-id="59d4e-121">You can configure hello nodes on your Service Fabric cluster by using hello **nodes** section, as hello following snippet shows.</span></span>

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

<span data-ttu-id="59d4e-122">A Service Fabric-fürt tartalmaznia kell legalább 3 csomópontok.</span><span class="sxs-lookup"><span data-stu-id="59d4e-122">A Service Fabric cluster must contain at least 3 nodes.</span></span> <span data-ttu-id="59d4e-123">A beállítás szerint további csomópontokat toothis szakasz is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="59d4e-123">You can add more nodes toothis section as per your setup.</span></span> <span data-ttu-id="59d4e-124">a következő táblázat hello hello konfigurációs beállítások az egyes csomópontok ismerteti.</span><span class="sxs-lookup"><span data-stu-id="59d4e-124">hello following table explains hello configuration settings for each node.</span></span>

| <span data-ttu-id="59d4e-125">**A csomópont-konfiguráció**</span><span class="sxs-lookup"><span data-stu-id="59d4e-125">**Node configuration**</span></span> | <span data-ttu-id="59d4e-126">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="59d4e-126">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="59d4e-127">Csomópontnév</span><span class="sxs-lookup"><span data-stu-id="59d4e-127">nodeName</span></span> |<span data-ttu-id="59d4e-128">Egy rövid nevet toohello csomópont biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="59d4e-128">You can give any friendly name toohello node.</span></span> |
| <span data-ttu-id="59d4e-129">IP-cím</span><span class="sxs-lookup"><span data-stu-id="59d4e-129">iPAddress</span></span> |<span data-ttu-id="59d4e-130">Nyisson meg egy parancsablakot, és írja be a csomópont hello IP-címének megállapítása `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="59d4e-130">Find out hello IP address of your node by opening a command window and typing `ipconfig`.</span></span> <span data-ttu-id="59d4e-131">Vegye figyelembe a hello IPV4-címet, és rendelje hozzá toohello **IP-cím** változó.</span><span class="sxs-lookup"><span data-stu-id="59d4e-131">Note hello IPV4 address and assign it toohello **iPAddress** variable.</span></span> |
| <span data-ttu-id="59d4e-132">nodeTypeRef</span><span class="sxs-lookup"><span data-stu-id="59d4e-132">nodeTypeRef</span></span> |<span data-ttu-id="59d4e-133">Minden csomópont rendelhetők hozzá másik csomóponttípus.</span><span class="sxs-lookup"><span data-stu-id="59d4e-133">Each node can be assigned a different node type.</span></span> <span data-ttu-id="59d4e-134">Hello [csomóponttípusok](#nodetypes) hello szakaszában az alábbi.</span><span class="sxs-lookup"><span data-stu-id="59d4e-134">hello [node types](#nodetypes) are defined in hello section below.</span></span> |
| <span data-ttu-id="59d4e-135">faultDomain</span><span class="sxs-lookup"><span data-stu-id="59d4e-135">faultDomain</span></span> |<span data-ttu-id="59d4e-136">Tartalék tartományok engedélyezése rendszergazdák toodefine hello fizikai fürtcsomóponton, amely előfordulhat, hogy sikertelen volt hello azonos időben tooshared fizikai függőségek miatt.</span><span class="sxs-lookup"><span data-stu-id="59d4e-136">Fault domains enable cluster administrators toodefine hello physical nodes that might fail at hello same time due tooshared physical dependencies.</span></span> |
| <span data-ttu-id="59d4e-137">upgradeDomain</span><span class="sxs-lookup"><span data-stu-id="59d4e-137">upgradeDomain</span></span> |<span data-ttu-id="59d4e-138">Frissítési tartományok jellemezhető a csomópontokra, amelyeket állítsa le a Service Fabric frissíti a vonatkozó hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="59d4e-138">Upgrade domains describe sets of nodes that are shut down for Service Fabric upgrades at about hello same time.</span></span> <span data-ttu-id="59d4e-139">Dönthet úgy, mely csomópontok tooassign toowhich frissítési tartományok, mint bármely fizikai követelmények nem korlátozza.</span><span class="sxs-lookup"><span data-stu-id="59d4e-139">You can choose which nodes tooassign toowhich Upgrade domains, as they are not limited by any physical requirements.</span></span> |

## <a name="cluster-properties"></a><span data-ttu-id="59d4e-140">Fürt tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="59d4e-140">Cluster properties</span></span>
<span data-ttu-id="59d4e-141">Hello **tulajdonságok** hello művelet szakaszában a következőképpen történik használt tooconfigure hello fürt.</span><span class="sxs-lookup"><span data-stu-id="59d4e-141">hello **properties** section in hello ClusterConfig.JSON is used tooconfigure hello cluster as follows.</span></span>

<a id="reliability"></a>

### <a name="reliability"></a><span data-ttu-id="59d4e-142">Megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="59d4e-142">Reliability</span></span>
<span data-ttu-id="59d4e-143">hello fogalma **reliabilityLevel** replikák száma hello vagy szolgáltatáspéldányok hello Service Fabric rendszer hello hello fürt elsődleges csomópontjának futtatható határoz meg.</span><span class="sxs-lookup"><span data-stu-id="59d4e-143">hello concept of **reliabilityLevel** defines hello number of replicas or instances of hello Service Fabric system services that can run on hello primary nodes of hello cluster.</span></span> <span data-ttu-id="59d4e-144">Meghatározza, hogy ezek a szolgáltatások hello megbízhatóságát, és ezért a fürt hello.</span><span class="sxs-lookup"><span data-stu-id="59d4e-144">It determines hello reliability of these services and hence hello cluster.</span></span> <span data-ttu-id="59d4e-145">hello értéke hello rendszer által számított fürt létrehozása és frissítése során.</span><span class="sxs-lookup"><span data-stu-id="59d4e-145">hello value is calcuated by hello system at cluster creation and upgrade time.</span></span>

### <a name="diagnostics"></a><span data-ttu-id="59d4e-146">Diagnosztika</span><span class="sxs-lookup"><span data-stu-id="59d4e-146">Diagnostics</span></span>
<span data-ttu-id="59d4e-147">Hello **diagnosticsStore** szakasz lehetővé teszi tooconfigure paraméterek tooenable diagnosztika és a hibaelhárítási csomópont vagy a fürt hibák, ahogy az alábbi részlet hello.</span><span class="sxs-lookup"><span data-stu-id="59d4e-147">hello **diagnosticsStore** section allows you tooconfigure parameters tooenable diagnostics and troubleshooting node or cluster failures, as shown in hello following snippet.</span></span> 

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

<span data-ttu-id="59d4e-148">Hello **metaadatok** a fürt diagnosztika leírása és a telepítő szerint állítható be.</span><span class="sxs-lookup"><span data-stu-id="59d4e-148">hello **metadata** is a description of your cluster diagnostics and can be set as per your setup.</span></span> <span data-ttu-id="59d4e-149">Ezek a változók megkönnyíti a ETW nyomkövetési napló- és összeomlási memóriaképeket, valamint teljesítményszámlálók gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="59d4e-149">These variables help in collecting ETW trace logs, crash dumps as well as performance counters.</span></span> <span data-ttu-id="59d4e-150">Olvasási [a követési naplóban](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) és [ETW-nyomkövetés](https://msdn.microsoft.com/library/ms751538.aspx) ETW-vel további információ a nyomkövetési naplóit.</span><span class="sxs-lookup"><span data-stu-id="59d4e-150">Read [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) and [ETW Tracing](https://msdn.microsoft.com/library/ms751538.aspx) for more information on ETW trace logs.</span></span> <span data-ttu-id="59d4e-151">Beleértve az összes napló [összeomlási memóriaképek](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) és [teljesítményszámlálók](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) lehet irányított toohello **connectionString** mappát a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="59d4e-151">All logs including [Crash dumps](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) and [performance counters](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) can be directed toohello **connectionString** folder on your machine.</span></span> <span data-ttu-id="59d4e-152">Is *AzureStorage* diagnosztika tárolásához.</span><span class="sxs-lookup"><span data-stu-id="59d4e-152">You can also use *AzureStorage* for storing diagnostics.</span></span> <span data-ttu-id="59d4e-153">Lentebb egy minta részlet.</span><span class="sxs-lookup"><span data-stu-id="59d4e-153">See below for a sample snippet.</span></span>

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a><span data-ttu-id="59d4e-154">Biztonság</span><span class="sxs-lookup"><span data-stu-id="59d4e-154">Security</span></span>
<span data-ttu-id="59d4e-155">Hello **biztonsági** szakasz esetén szükség a biztonságos különálló Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="59d4e-155">hello **security** section is necessary for a secure standalone Service Fabric cluster.</span></span> <span data-ttu-id="59d4e-156">a következő kódrészletet hello ebben a szakaszban egy részét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="59d4e-156">hello following snippet shows a part of this section.</span></span>

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

<span data-ttu-id="59d4e-157">Hello **metaadatok** a biztonságos fürt leírása és a telepítő szerint állítható be.</span><span class="sxs-lookup"><span data-stu-id="59d4e-157">hello **metadata** is a description of your secure cluster and can be set as per your setup.</span></span> <span data-ttu-id="59d4e-158">Hello **ClusterCredentialType** és **ServerCredentialType** határozzák meg, amely hello fürtből, valamint hello csomópontot megvalósítandó biztonsági hello típusú.</span><span class="sxs-lookup"><span data-stu-id="59d4e-158">hello **ClusterCredentialType** and **ServerCredentialType** determine hello type of security that hello cluster and hello nodes will implement.</span></span> <span data-ttu-id="59d4e-159">A megadható tooeither *X509* egy tanúsítványalapú biztonsági vagy *Windows* egy Azure Active Directory-alapú biztonság.</span><span class="sxs-lookup"><span data-stu-id="59d4e-159">They can be set tooeither *X509* for a certificate-based security, or *Windows* for an Azure Active Directory-based security.</span></span> <span data-ttu-id="59d4e-160">hello részeinek hello **biztonsági** szakasz hello biztonsági hello típusú alapul.</span><span class="sxs-lookup"><span data-stu-id="59d4e-160">hello rest of hello **security** section will be based on hello type of hello security.</span></span> <span data-ttu-id="59d4e-161">Olvasási [tanúsítványok-alapú biztonsági önálló fürtben](service-fabric-windows-cluster-x509-security.md) vagy [Windows biztonsági önálló fürtben](service-fabric-windows-cluster-windows-security.md) olvashat, hogyan kimenő hello toofill rest-e a hello **biztonsági**szakasz.</span><span class="sxs-lookup"><span data-stu-id="59d4e-161">Read [Certificates-based security in a standalone cluster](service-fabric-windows-cluster-x509-security.md) or [Windows security in a standalone cluster](service-fabric-windows-cluster-windows-security.md) for information on how toofill out hello rest of hello **security** section.</span></span>

<a id="nodetypes"></a>

### <a name="node-types"></a><span data-ttu-id="59d4e-162">Csomóponttípusok</span><span class="sxs-lookup"><span data-stu-id="59d4e-162">Node Types</span></span>
<span data-ttu-id="59d4e-163">Hello **NodeType tulajdonságok értéke** szakasz ismerteti, amely a fürt rendelkezik hello csomópontok hello típusú.</span><span class="sxs-lookup"><span data-stu-id="59d4e-163">hello **nodeTypes** section describes hello type of hello nodes that your cluster has.</span></span> <span data-ttu-id="59d4e-164">Fürt esetén kell adni legalább egy csomópont típusát, ahogy az alábbi hello részlet.</span><span class="sxs-lookup"><span data-stu-id="59d4e-164">At least one node type must be specified for a cluster, as shown in hello snippet below.</span></span> 

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

<span data-ttu-id="59d4e-165">Hello **neve** hello felhasználóbarát név az adott csomóponttípus.</span><span class="sxs-lookup"><span data-stu-id="59d4e-165">hello **name** is hello friendly name for this particular node type.</span></span> <span data-ttu-id="59d4e-166">a csomópont típusú csomópont toocreate rendelje hozzá a rövid név toohello **nodeTypeRef** változó az adott csomópont, ahogy azt korábban említettük [fent](#clusternodes).</span><span class="sxs-lookup"><span data-stu-id="59d4e-166">toocreate a node of this node type, assign its friendly name toohello **nodeTypeRef** variable for that node, as mentioned [above](#clusternodes).</span></span> <span data-ttu-id="59d4e-167">Az egyes csomópont adja meg a használandó hello kapcsolati végpontok.</span><span class="sxs-lookup"><span data-stu-id="59d4e-167">For each node type, define hello connection endpoints that will be used.</span></span> <span data-ttu-id="59d4e-168">Minden kapcsolat végpontokkal portszáma dönthet úgy, mindaddig, amíg azok nem ütköznek-e a fürt bármely más végpontja.</span><span class="sxs-lookup"><span data-stu-id="59d4e-168">You can choose any port number for these connection endpoints, as long as they do not conflict with any other endpoints in this cluster.</span></span> <span data-ttu-id="59d4e-169">Több csomópontos fürtben, lesz egy vagy több elsődleges csomóponton (pl. **isPrimary** túl beállítása*igaz*) hello attól függően, hogy [ **reliabilityLevel** ](#reliability).</span><span class="sxs-lookup"><span data-stu-id="59d4e-169">In a multi-node cluster, there will be one or more primary nodes (i.e. **isPrimary** set too*true*), depending on hello [**reliabilityLevel**](#reliability).</span></span> <span data-ttu-id="59d4e-170">Olvasási [Service Fabric fürt kapacitástervezésének szempontjai](service-fabric-cluster-capacity.md) információk **NodeType tulajdonságok értéke** és **reliabilityLevel**, és mi elsődleges és hello tooknow nem elsődleges csomóponttípusok.</span><span class="sxs-lookup"><span data-stu-id="59d4e-170">Read [Service Fabric cluster capacity planning considerations](service-fabric-cluster-capacity.md) for information on **nodeTypes** and **reliabilityLevel**, and tooknow what are primary and hello non-primary node types.</span></span> 

#### <a name="endpoints-used-tooconfigure-hello-node-types"></a><span data-ttu-id="59d4e-171">Végpontok használt tooconfigure hello csomóponttípusok</span><span class="sxs-lookup"><span data-stu-id="59d4e-171">Endpoints used tooconfigure hello node types</span></span>
* <span data-ttu-id="59d4e-172">*clientConnectionEndpointPort* hello port által használt hello ügyfél tooconnect toohello hello ügyfél API-k használata esetén.</span><span class="sxs-lookup"><span data-stu-id="59d4e-172">*clientConnectionEndpointPort* is hello port used by hello client tooconnect toohello cluster, when using hello client APIs.</span></span> 
* <span data-ttu-id="59d4e-173">*clusterConnectionEndpointPort* hello port, amelyen hello csomópontok kommunikálnak egymással.</span><span class="sxs-lookup"><span data-stu-id="59d4e-173">*clusterConnectionEndpointPort* is hello port at which hello nodes communicate with each other.</span></span>
* <span data-ttu-id="59d4e-174">*leaseDriverEndpointPort* hello port hello fürt bérleti illesztőprogram toofind kimenő használják, ha hello csomópontok még mindig aktív.</span><span class="sxs-lookup"><span data-stu-id="59d4e-174">*leaseDriverEndpointPort* is hello port used by hello cluster lease driver toofind out if hello nodes are still active.</span></span> 
* <span data-ttu-id="59d4e-175">*serviceConnectionEndpointPort* hello alkalmazások által használt hello port és a csomóponton, hogy adott csomóponton hello Service Fabric ügyféllel toocommunicate telepített szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="59d4e-175">*serviceConnectionEndpointPort* is hello port used by hello applications and services deployed on a node, toocommunicate with hello Service Fabric client on that particular node.</span></span>
* <span data-ttu-id="59d4e-176">*httpGatewayEndpointPort* hello Service Fabric Explorer tooconnect toohello fürt által használt hello port.</span><span class="sxs-lookup"><span data-stu-id="59d4e-176">*httpGatewayEndpointPort* is hello port used by hello Service Fabric Explorer tooconnect toohello cluster.</span></span>
* <span data-ttu-id="59d4e-177">*ephemeralPorts* hello felülbírálása [hello az operációs rendszer által használt dinamikus portok](https://support.microsoft.com/kb/929851).</span><span class="sxs-lookup"><span data-stu-id="59d4e-177">*ephemeralPorts* override hello [dynamic ports used by hello OS](https://support.microsoft.com/kb/929851).</span></span> <span data-ttu-id="59d4e-178">Service Fabric fogja használni a ezek alkalmazás portok része, és hello fennmaradó lesz elérhető a hello az operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="59d4e-178">Service Fabric will use a part of these as application ports and hello remaining will be available for hello OS.</span></span> <span data-ttu-id="59d4e-179">Azt fogja is tartományt képeznek le a tartomány toohello meglévő szerepel hello az operációs rendszer, így minden célra használhatja hello tartományok hello minta JSON-fájlokat a megadott.</span><span class="sxs-lookup"><span data-stu-id="59d4e-179">It will also map this range toohello existing range present in hello OS, so for all purposes, you can use hello ranges given in hello sample JSON files.</span></span> <span data-ttu-id="59d4e-180">Szüksége van arra, hogy a hello kezdő és záró portok hello hello különbségének legalább 255 toomake.</span><span class="sxs-lookup"><span data-stu-id="59d4e-180">You need toomake sure that hello difference between hello start and hello end ports is at least 255.</span></span> <span data-ttu-id="59d4e-181">Ha ezt a különbséget túl alacsony, mivel ez a tartomány megosztott hello operációs rendszerrel való ütközések futtathatnak.</span><span class="sxs-lookup"><span data-stu-id="59d4e-181">You may run into conflicts if this difference is too low, since this range is shared with hello operating system.</span></span> <span data-ttu-id="59d4e-182">Tekintse meg a konfigurált hello dinamikus porttartományt futtatásával `netsh int ipv4 show dynamicport tcp`.</span><span class="sxs-lookup"><span data-stu-id="59d4e-182">See hello configured dynamic port range by running `netsh int ipv4 show dynamicport tcp`.</span></span>
* <span data-ttu-id="59d4e-183">*applicationPorts* hello Service Fabric-alkalmazások által használt portok hello.</span><span class="sxs-lookup"><span data-stu-id="59d4e-183">*applicationPorts* are hello ports that will be used by hello Service Fabric applications.</span></span> <span data-ttu-id="59d4e-184">hello alkalmazás porttartományát legyen elég nagy toocover hello végpont követelmény az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="59d4e-184">hello application port range should be large enough toocover hello endpoint requirement of your applications.</span></span> <span data-ttu-id="59d4e-185">Ebben a tartományban kell lennie a hello dinamikus porttartományt hello gépen, azaz hello kizárólagos *ephemeralPorts* hello konfigurációjában beállított tartományon.</span><span class="sxs-lookup"><span data-stu-id="59d4e-185">This range should be exclusive from hello dynamic port range on hello machine, i.e. hello *ephemeralPorts* range as set in hello configuration.</span></span>  <span data-ttu-id="59d4e-186">A Service Fabric fog használni, ezek új portok szükségesek, valamint gondoskodunk hello tűzfal ezen portok megnyitása.</span><span class="sxs-lookup"><span data-stu-id="59d4e-186">Service Fabric will use these whenever new ports are required, as well as take care of opening hello firewall for these ports.</span></span> 
* <span data-ttu-id="59d4e-187">*reverseProxyEndpointPort* a választható fordított proxy végpontja.</span><span class="sxs-lookup"><span data-stu-id="59d4e-187">*reverseProxyEndpointPort* is an optional reverse proxy endpoint.</span></span> <span data-ttu-id="59d4e-188">Lásd: [Service Fabric fordított Proxy](service-fabric-reverseproxy.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="59d4e-188">See [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) for more details.</span></span> 

### <a name="log-settings"></a><span data-ttu-id="59d4e-189">Naplózási beállítások</span><span class="sxs-lookup"><span data-stu-id="59d4e-189">Log Settings</span></span>
<span data-ttu-id="59d4e-190">Hello **fabricSettings** szakasz lehetővé teszi, hogy tooset hello gyökérkönyvtárak hello Service Fabric adatokat és a naplókat.</span><span class="sxs-lookup"><span data-stu-id="59d4e-190">hello **fabricSettings** section allows you tooset hello root directories for hello Service Fabric data and logs.</span></span> <span data-ttu-id="59d4e-191">Testre szabhatja ezek csak hello kezdeti fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="59d4e-191">You can customize these only during hello initial cluster creation.</span></span> <span data-ttu-id="59d4e-192">Ez a szakasz egy minta szövegrészletet lentebb.</span><span class="sxs-lookup"><span data-stu-id="59d4e-192">See below for a sample snippet of this section.</span></span>

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

<span data-ttu-id="59d4e-193">Azt javasoljuk, hogy használatával egy-az operációs rendszer meghajtóján hello FabricDataRoot, és a fabriclogroot mappában, szemben az operációs rendszer összeomlások további megbízhatóságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="59d4e-193">We recommended using a non-OS drive as hello FabricDataRoot and FabricLogRoot as it provides more reliability against OS crashes.</span></span> <span data-ttu-id="59d4e-194">Vegye figyelembe, hogy csak a hello adatgyökerében testre, majd hello napló legfelső szintű kerülnek hello adatgyökerében alatt egy szinttel.</span><span class="sxs-lookup"><span data-stu-id="59d4e-194">Note that if you customize only hello data root, then hello log root will be placed one level below hello data root.</span></span>

### <a name="stateful-reliable-service-settings"></a><span data-ttu-id="59d4e-195">Állapot-nyilvántartó megbízható szolgáltatás beállításai</span><span class="sxs-lookup"><span data-stu-id="59d4e-195">Stateful Reliable Service Settings</span></span>
<span data-ttu-id="59d4e-196">Hello **KtlLogger** szakasz lehetővé teszi a Reliable Services tooset hello globális konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="59d4e-196">hello **KtlLogger** section allows you tooset hello global configuration settings for Reliable Services.</span></span> <span data-ttu-id="59d4e-197">Ezek a beállítások a további részletekért olvassa el a [állapot-nyilvántartó megbízható szolgáltatások konfigurálása](service-fabric-reliable-services-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="59d4e-197">For more details on these settings read [Configure stateful reliable services](service-fabric-reliable-services-configuration.md).</span></span>
<span data-ttu-id="59d4e-198">hello az alábbi példa bemutatja, hogyan toochange hello hello megosztott tranzakciós napló, amely lekérdezi hozza létre a tooback a megbízható gyűjtemények állapotalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="59d4e-198">hello example below shows how toochange hello hello shared transaction log that gets created tooback any reliable collections for stateful services.</span></span>

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a><span data-ttu-id="59d4e-199">Bővítmény szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="59d4e-199">Add-on features</span></span>
<span data-ttu-id="59d4e-200">tooconfigure bővítményeire, hello apiVersion legyen konfigurált mint "04-2017' vagy újabb, és addonFeatures toobe konfigurálni kell:</span><span class="sxs-lookup"><span data-stu-id="59d4e-200">tooconfigure add-on features, hello apiVersion should be configured as '04-2017' or higher, and addonFeatures needs toobe configured:</span></span>

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a><span data-ttu-id="59d4e-201">Tároló-támogatás</span><span class="sxs-lookup"><span data-stu-id="59d4e-201">Container support</span></span>
<span data-ttu-id="59d4e-202">tooenable tároló támogatása a windows server tároló, mind önálló fürtök a hyper-v tárolója, hello "DnsService" bővítmény szolgáltatás toobe engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="59d4e-202">tooenable container support for both windows server container and hyper-v container for standalone clusters, hello 'DnsService' add-on feature needs toobe enabled.</span></span>


## <a name="next-steps"></a><span data-ttu-id="59d4e-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59d4e-203">Next steps</span></span>
<span data-ttu-id="59d4e-204">Miután a különálló fürt beállítása szerint konfigurált teljes művelet fájlt, a cikk a következő hello fürtök telepítése [hozzon létre egy különálló Service Fabric-fürt](service-fabric-cluster-creation-for-windows-server.md) majd folytassa a túl[a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="59d4e-204">Once you have a complete ClusterConfig.JSON file configured as per your standalone cluster setup, you can deploy your cluster by following hello article [Create a standalone Service Fabric cluster](service-fabric-cluster-creation-for-windows-server.md) and then proceed too[visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>

