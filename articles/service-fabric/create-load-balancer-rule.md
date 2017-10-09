---
title: "egy Azure Load Balancer szabály fürt aaaCreate"
description: "Állítson be egy Azure Load Balancer tooopen portot az Azure Service Fabric-fürt."
services: service-fabric
documentationcenter: na
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/22/2017
ms.author: adegeo
ms.openlocfilehash: 4a40f62422bd895d782be8cbaace5f4e1af81db3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-for-a-service-fabric-cluster"></a><span data-ttu-id="e58b0-103">Nyissa meg a Service Fabric-fürt által használt portokat</span><span class="sxs-lookup"><span data-stu-id="e58b0-103">Open ports for a Service Fabric cluster</span></span>

<span data-ttu-id="e58b0-104">az Azure Service Fabric-fürt üzembe helyezéséhez hello terheléselosztó irányítja a forgalmat tooyour app egy csomóponton futó.</span><span class="sxs-lookup"><span data-stu-id="e58b0-104">hello load balancer deployed with your Azure Service Fabric cluster directs traffic tooyour app running on a node.</span></span> <span data-ttu-id="e58b0-105">Ha megváltoztatja az alkalmazás toouse egy másik portra, tesz közzé, hogy a port (vagy az útvonal egy másik portot) a hello Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="e58b0-105">If you change your app toouse a different port, you must expose that port (or route a different port) in hello Azure Load Balancer.</span></span>

<span data-ttu-id="e58b0-106">A service fabric-fürt tooAzure üzembe helyezésekor, a terheléselosztó automatikusan létrejött meg.</span><span class="sxs-lookup"><span data-stu-id="e58b0-106">When you deployed your service fabric cluster tooAzure, a load balancer was automatically created for you.</span></span> <span data-ttu-id="e58b0-107">Ha nem rendelkezik olyan terheléselosztóhoz, lásd: [egy internetre irányuló terheléskiegyenlítő](..\load-balancer\load-balancer-get-started-internet-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e58b0-107">If you do not have a load balancer, see [Configure an Internet-facing load balancer](..\load-balancer\load-balancer-get-started-internet-portal.md).</span></span>

## <a name="configure-service-fabric"></a><span data-ttu-id="e58b0-108">A service fabric konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e58b0-108">Configure service fabric</span></span>

<span data-ttu-id="e58b0-109">A Service Fabric-alkalmazás **ServiceManifest.xml** konfigurációs fájl határozza meg az alkalmazás vár toouse hello végpontok.</span><span class="sxs-lookup"><span data-stu-id="e58b0-109">Your Service Fabric application **ServiceManifest.xml** config file defines hello endpoints your application expects toouse.</span></span> <span data-ttu-id="e58b0-110">Hello konfigurációs fájl frissített toodefine végpont után hello terheléselosztó kell lennie, hogy (vagy egy másik) frissített tooexpose port.</span><span class="sxs-lookup"><span data-stu-id="e58b0-110">After hello config file has been updated toodefine an endpoint, hello load balancer must be updated tooexpose that (or a different) port.</span></span> <span data-ttu-id="e58b0-111">Hogyan toocreate hello szolgáltatás hálóvégpont további információkért lásd: [egy végpont beállítása](service-fabric-service-manifest-resources.md).</span><span class="sxs-lookup"><span data-stu-id="e58b0-111">For more information on how toocreate hello service fabric endpoint, see [Setup an Endpoint](service-fabric-service-manifest-resources.md).</span></span>

## <a name="create-a-load-balancer-rule"></a><span data-ttu-id="e58b0-112">Hozzon létre olyan terheléselosztó szabályhoz</span><span class="sxs-lookup"><span data-stu-id="e58b0-112">Create a load balancer rule</span></span>

<span data-ttu-id="e58b0-113">Olyan terheléselosztó szabályhoz egy internetre irányuló portot nyit, és az alkalmazás által használt forgalom toohello belső csomópont port továbbítja.</span><span class="sxs-lookup"><span data-stu-id="e58b0-113">A load balancer rule opens up an internet-facing port and forwards traffic toohello internal node's port used by your application.</span></span> <span data-ttu-id="e58b0-114">Ha nem rendelkezik olyan terheléselosztóhoz, lásd: [egy internetre irányuló terheléskiegyenlítő](..\load-balancer\load-balancer-get-started-internet-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e58b0-114">If you do not have a load balancer, see [Configure an Internet-facing load balancer](..\load-balancer\load-balancer-get-started-internet-portal.md).</span></span>

<span data-ttu-id="e58b0-115">terheléselosztó szabály toocollect hello a következő információkat kell toocreate:</span><span class="sxs-lookup"><span data-stu-id="e58b0-115">toocreate a load balancer rule, you need toocollect hello following information:</span></span>

- <span data-ttu-id="e58b0-116">Terheléselosztó neve.</span><span class="sxs-lookup"><span data-stu-id="e58b0-116">Load balancer name.</span></span>
- <span data-ttu-id="e58b0-117">Erőforráscsoport hello a terheléselosztó és a service fabric-fürt betöltése.</span><span class="sxs-lookup"><span data-stu-id="e58b0-117">Resource group of hello load balancer and service fabric cluster.</span></span>
- <span data-ttu-id="e58b0-118">Külső porton.</span><span class="sxs-lookup"><span data-stu-id="e58b0-118">External port.</span></span>
- <span data-ttu-id="e58b0-119">Belső port.</span><span class="sxs-lookup"><span data-stu-id="e58b0-119">Internal port.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="e58b0-120">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e58b0-120">Azure CLI</span></span>
>[!NOTE]
><span data-ttu-id="e58b0-121">Hello terheléselosztó toodetermine hello neve van szüksége, használja a parancs tooquickly get terheléselosztókról, illetve a kapcsolódó hello erőforráscsoportok listáját.</span><span class="sxs-lookup"><span data-stu-id="e58b0-121">If you need toodetermine hello name of hello load balancer, use this command tooquickly get a list of all load balancers and hello associated resource groups.</span></span>
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>

<span data-ttu-id="e58b0-122">Ami mindössze egy egyetlen parancs toocreate olyan terheléselosztó szabályhoz a hello **Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="e58b0-122">It only takes a single command toocreate a load balancer rule with hello **Azure CLI**.</span></span> <span data-ttu-id="e58b0-123">Egyszerűen tooknow mindkét hello hello terhelés terheléselosztó és erőforrás csoport toocreate új szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="e58b0-123">You just need tooknow both hello name of hello load balancer and resource group toocreate a new rule.</span></span>

```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

<span data-ttu-id="e58b0-124">hello Azure CLI parancs néhány hello a következő táblázatban ismertetett paraméterekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e58b0-124">hello Azure CLI command has a few parameters that are described in hello following table:</span></span>

| <span data-ttu-id="e58b0-125">Paraméter</span><span class="sxs-lookup"><span data-stu-id="e58b0-125">Parameter</span></span> | <span data-ttu-id="e58b0-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="e58b0-126">Description</span></span> |
| --------- | ----------- |
| `--backend-port`  | <span data-ttu-id="e58b0-127">hello port hello service fabric-alkalmazás figyel.</span><span class="sxs-lookup"><span data-stu-id="e58b0-127">hello port hello service fabric application is listening to.</span></span> |
| `--frontend-port` | <span data-ttu-id="e58b0-128">hello port hello terheléselosztó tesz elérhetővé a külső kapcsolatok betölteni.</span><span class="sxs-lookup"><span data-stu-id="e58b0-128">hello port hello load balancer exposes for external connections.</span></span> |
| `-lb-name` | <span data-ttu-id="e58b0-129">hello hello neve terheléselosztó toochange betölteni.</span><span class="sxs-lookup"><span data-stu-id="e58b0-129">hello name of hello load balancer toochange.</span></span> |
| `-g`       | <span data-ttu-id="e58b0-130">hello erőforráscsoport, amelyen hello terheléselosztó és a service fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="e58b0-130">hello resource group that has both hello load balancer and service fabric cluster.</span></span> |
| `-n`       | <span data-ttu-id="e58b0-131">hello kiválasztott hello szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="e58b0-131">hello chosen name of hello rule.</span></span> |


>[!NOTE]
><span data-ttu-id="e58b0-132">További információ a hogyan toocreate hello Azure parancssori felület, a terheléselosztó: [hozzon létre egy terhelés-kiegyenlítő hello Azure CLI](..\load-balancer\load-balancer-get-started-internet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e58b0-132">For more information on how toocreate a load balancer with hello Azure CLI, see [Create a load balancer with hello Azure CLI](..\load-balancer\load-balancer-get-started-internet-arm-cli.md).</span></span>

## <a name="powershell"></a><span data-ttu-id="e58b0-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e58b0-133">PowerShell</span></span>

>[!NOTE]
><span data-ttu-id="e58b0-134">Hello terheléselosztó toodetermine hello neve van szüksége, használja a parancs tooquickly get terheléselosztókról, illetve a kapcsolódó erőforráscsoportok listáját.</span><span class="sxs-lookup"><span data-stu-id="e58b0-134">If you need toodetermine hello name of hello load balancer, use this command tooquickly get a list of all load balancers and associated resource groups.</span></span>
>
>`Get-AzureRmLoadBalancer | Select Name, ResourceGroupName`

<span data-ttu-id="e58b0-135">PowerShell egy kicsit bonyolultabb, mint az Azure parancssori felület hello.</span><span class="sxs-lookup"><span data-stu-id="e58b0-135">PowerShell is a little more complicated than hello Azure CLI.</span></span> <span data-ttu-id="e58b0-136">Fogalmilag akkor hajtsa végre a következő lépéseket toocreate hello szabály.</span><span class="sxs-lookup"><span data-stu-id="e58b0-136">Conceptually, do hello following steps toocreate a rule.</span></span>

1. <span data-ttu-id="e58b0-137">Hello terheléselosztó beolvasása az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="e58b0-137">Get hello load balancer from Azure.</span></span>
2. <span data-ttu-id="e58b0-138">Hozzon létre egy szabályt.</span><span class="sxs-lookup"><span data-stu-id="e58b0-138">Create a rule.</span></span>
3. <span data-ttu-id="e58b0-139">Hello szabály toohello terheléselosztó felvétele.</span><span class="sxs-lookup"><span data-stu-id="e58b0-139">Add hello rule toohello load balancer.</span></span>
4. <span data-ttu-id="e58b0-140">Hello terheléselosztó frissítése.</span><span class="sxs-lookup"><span data-stu-id="e58b0-140">Update hello load balancer.</span></span>

```powershell
# Get hello load balancer
$lb = Get-AzureRmLoadBalancer -Name LB-svcfab3 -ResourceGroupName svcfab_cli

# Create hello rule based on information from hello load balancer.
$lbrule = New-AzureRmLoadBalancerRuleConfig -Name my-app-rule7 -Protocol Tcp -FrontendPort 39990 -BackendPort 40009 `
                                            -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
                                            -BackendAddressPool  $lb.BackendAddressPools[0] `
                                            -Probe $lb.Probes[0]

# Add hello rule toohello load balancer
$lb.LoadBalancingRules.Add($lbrule)

# Update hello load balancer on Azure
$lb | Set-AzureRmLoadBalancer
```

<span data-ttu-id="e58b0-141">Hello vonatkozó `New-AzureRmLoadBalancerRuleConfig` parancs hello `-FrontendPort` jelöli hello port hello terheléselosztó elérhetővé teszi a külső kapcsolatot, és hello `-BackendPort` jelöli hello port hello service fabric-alkalmazást figyel.</span><span class="sxs-lookup"><span data-stu-id="e58b0-141">Regarding hello `New-AzureRmLoadBalancerRuleConfig` command, hello `-FrontendPort` represents hello port hello load balancer exposes for external connections, and hello `-BackendPort` represents hello port hello service fabric app is listening to.</span></span>

>[!NOTE]
><span data-ttu-id="e58b0-142">További információ a hogyan toocreate PowerShell, a terheléselosztó: [terheléselosztó létrehozása a PowerShell használatával](..\load-balancer\load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e58b0-142">For more information on how toocreate a load balancer with PowerShell, see [Create a load balancer with PowerShell](..\load-balancer\load-balancer-get-started-internet-arm-ps.md).</span></span>

