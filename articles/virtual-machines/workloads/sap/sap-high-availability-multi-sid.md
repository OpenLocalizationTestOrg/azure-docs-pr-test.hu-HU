---
title: "egy SAP multi-SID-konfiguráció Azure aaaCreate |} Microsoft Docs"
description: "Az útmutató toohigh rendelkezésre állási SAP NetWeaver multi-SID konfigurációban Windows virtuális gépeken"
services: virtual-machines-windows, virtual-network, storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 0b89b4f8-6d6c-45d7-8d20-fe93430217ca
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2a4b12928231743c59af86dab72595ad38d364b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a><span data-ttu-id="602d0-103">Egy SAP NetWeaver multi-SID-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="602d0-103">Create an SAP NetWeaver multi-SID configuration</span></span>

[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md
[sap-ha-guide]:sap-high-availability-guide.md 
[sap-ha-guide-figure-6001]:./media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:./media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:./media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:./media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:./media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png
[networking-limits-azure-resource-manager]:../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[sap-ha-guide-9.1.1]:sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 
[sap-ha-guide-8.8]:sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.12.3.3]:sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 
[sap-ha-guide-9]:sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 
[sap-ha-guide-9.1.2]:sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 
[sap-ha-guide-9.1.3]:sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 
[sap-ha-guide-9.1.4]:sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 
[sap-ha-guide-9.4]:sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a 
[sap-ha-guide-9.5]:sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 
[sap-ha-guide-9.6]:sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 
[sap-ha-guide-10]:sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9

<span data-ttu-id="602d0-104">2016 szeptemberétől a Microsoft, amely egy szolgáltatás, ahol több virtuális IP-cím kezelheti az Azure belső terheléselosztó használatával.</span><span class="sxs-lookup"><span data-stu-id="602d0-104">In September 2016, Microsoft released a feature where you can manage multiple virtual IP addresses by using an Azure internal load balancer.</span></span> <span data-ttu-id="602d0-105">Ez a funkció már létezik a hello Azure külső terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="602d0-105">This functionality already exists in hello Azure external load balancer.</span></span>

<span data-ttu-id="602d0-106">Ha egy SAP üzemelő példányt, használhatja egy belső terheléselosztó toocreate Windows fürtkonfiguráció az SAP ASC/SCS, dokumentált módon hello [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken] [ sap-ha-guide].</span><span class="sxs-lookup"><span data-stu-id="602d0-106">If you have an SAP deployment, you can use an internal load balancer toocreate a Windows cluster configuration for SAP ASCS/SCS, as documented in hello [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide].</span></span>

<span data-ttu-id="602d0-107">Hogyan toomove egyetlen ASC/SCS telepítési tooan SAP multi-SID-konfigurációról további SAP ASC/SCS telepítésével fürtözött be egy meglévő Windows Server feladatátvételi fürtszolgáltatási (WSFC) fürt példányok cikkben összpontosít.</span><span class="sxs-lookup"><span data-stu-id="602d0-107">This article focuses on how toomove from a single ASCS/SCS installation tooan SAP multi-SID configuration by installing additional SAP ASCS/SCS clustered instances into an existing Windows Server Failover Clustering (WSFC) cluster.</span></span> <span data-ttu-id="602d0-108">Ez a folyamat befejezése után fog konfigurálta az SAP multi-SID-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="602d0-108">When this process is completed, you will have configured an SAP multi-SID cluster.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="602d0-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="602d0-109">Prerequisites</span></span>
<span data-ttu-id="602d0-110">Már konfigurálta a WSFC-fürt, amely egy SAP ASC/SCS példány szolgál a hello leírtaknak megfelelően [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken] [ sap-ha-guide] és ezen a diagramon látható módon.</span><span class="sxs-lookup"><span data-stu-id="602d0-110">You have already configured a WSFC cluster that is used for one SAP ASCS/SCS instance, as discussed in hello [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide] and as shown in this diagram.</span></span>

![Magas rendelkezésre állású SAP ASC/SCS példány][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a><span data-ttu-id="602d0-112">Tároló-architektúra</span><span class="sxs-lookup"><span data-stu-id="602d0-112">Target architecture</span></span>

<span data-ttu-id="602d0-113">hello célja tooinstall több SAP ABAP ASC vagy SAP Java SCS fürtözött példánya hello azonos WSFC-fürtre, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="602d0-113">hello goal is tooinstall multiple SAP ABAP ASCS or SAP Java SCS clustered instances in hello same WSFC cluster, as illustrated here:</span></span>

![Több SAP ASC/SCS fürtözött példánya, az Azure-ban][sap-ha-guide-figure-6002]

> [!NOTE]
><span data-ttu-id="602d0-115">Nincs titkos előtér-IP-címek minden Azure belső terheléselosztóhoz számának korlátozása toohello.</span><span class="sxs-lookup"><span data-stu-id="602d0-115">There is a limit toohello number of private front-end IPs for each Azure internal load balancer.</span></span>
>
><span data-ttu-id="602d0-116">egy WSFC-fürtön SAP ASC/SCS példányok maximális száma hello titkos előtér-IP-címek minden Azure belső terheléselosztóhoz maximális száma egyenlő toohello.</span><span class="sxs-lookup"><span data-stu-id="602d0-116">hello maximum number of SAP ASCS/SCS instances in one WSFC cluster is equal toohello maximum number of private front-end IPs for each Azure internal load balancer.</span></span>
>

<span data-ttu-id="602d0-117">Terheléselosztó korlátok kapcsolatos további információkért lásd: "magánhálózati front-end IP-cím terheléselosztó" a [hálózatkezelés korlátok: Azure Resource Manager][networking-limits-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="602d0-117">For more information about load-balancer limits, see "Private front end IP per load balancer" in [Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager].</span></span>

<span data-ttu-id="602d0-118">hello teljes fekvő két magas rendelkezésre állású SAP rendszerrel néz ki:</span><span class="sxs-lookup"><span data-stu-id="602d0-118">hello complete landscape with two high-availability SAP systems would look like this:</span></span>

![SAP magas rendelkezésre állású multi-SID telepítése két SAP rendszer SID-k][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> <span data-ttu-id="602d0-120">hello beállítása hello a következő feltételeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="602d0-120">hello setup must meet hello following conditions:</span></span>
> - <span data-ttu-id="602d0-121">hello SAP ASC/SCS példányai közös hello azonos WSFC-fürtre.</span><span class="sxs-lookup"><span data-stu-id="602d0-121">hello SAP ASCS/SCS instances must share hello same WSFC cluster.</span></span>
> - <span data-ttu-id="602d0-122">Minden adatbázis-kezelő SID rendelkeznie kell a saját dedikált WSFC-fürtre.</span><span class="sxs-lookup"><span data-stu-id="602d0-122">Each DBMS SID must have its own dedicated WSFC cluster.</span></span>
> - <span data-ttu-id="602d0-123">SAP alkalmazáskiszolgálók tooone SAP rendszer SID tartozó rendelkeznie kell a saját dedikált virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="602d0-123">SAP application servers that belong tooone SAP system SID must have their own dedicated VMs.</span></span>


## <a name="prepare-hello-infrastructure"></a><span data-ttu-id="602d0-124">Hello infrastruktúra előkészítése</span><span class="sxs-lookup"><span data-stu-id="602d0-124">Prepare hello infrastructure</span></span>
<span data-ttu-id="602d0-125">tooprepare az infrastruktúra telepítése egy SAP ASC/SCS másodpéldányt a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="602d0-125">tooprepare your infrastructure, you can install an additional SAP ASCS/SCS instance with hello following parameters:</span></span>

| <span data-ttu-id="602d0-126">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="602d0-126">Parameter name</span></span> | <span data-ttu-id="602d0-127">Érték</span><span class="sxs-lookup"><span data-stu-id="602d0-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="602d0-128">SAP ASC/SCS SID</span><span class="sxs-lookup"><span data-stu-id="602d0-128">SAP ASCS/SCS SID</span></span> |<span data-ttu-id="602d0-129">PR1-lb-ASC</span><span class="sxs-lookup"><span data-stu-id="602d0-129">pr1-lb-ascs</span></span> |
| <span data-ttu-id="602d0-130">SAP DBMS belső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="602d0-130">SAP DBMS internal load balancer</span></span> | <span data-ttu-id="602d0-131">PR5</span><span class="sxs-lookup"><span data-stu-id="602d0-131">PR5</span></span> |
| <span data-ttu-id="602d0-132">SAP virtuális állomás neve</span><span class="sxs-lookup"><span data-stu-id="602d0-132">SAP virtual host name</span></span> | <span data-ttu-id="602d0-133">pr5-sap-cl</span><span class="sxs-lookup"><span data-stu-id="602d0-133">pr5-sap-cl</span></span> |
| <span data-ttu-id="602d0-134">SAP ASC/SCS virtuális gazdagép IP-címe (további Azure load balancer IP-címe)</span><span class="sxs-lookup"><span data-stu-id="602d0-134">SAP ASCS/SCS virtual host IP address (additional Azure load balancer IP address)</span></span> | <span data-ttu-id="602d0-135">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="602d0-135">10.0.0.50</span></span> |
| <span data-ttu-id="602d0-136">SAP ASC/SCS példányszámának</span><span class="sxs-lookup"><span data-stu-id="602d0-136">SAP ASCS/SCS instance number</span></span> | <span data-ttu-id="602d0-137">50</span><span class="sxs-lookup"><span data-stu-id="602d0-137">50</span></span> |
| <span data-ttu-id="602d0-138">ILB mintavételi portot további SAP ASC/SCS-példány</span><span class="sxs-lookup"><span data-stu-id="602d0-138">ILB probe port for additional SAP ASCS/SCS instance</span></span> | <span data-ttu-id="602d0-139">62350</span><span class="sxs-lookup"><span data-stu-id="602d0-139">62350</span></span> |

> [!NOTE]
> <span data-ttu-id="602d0-140">Az SAP ASC/SCS példányokat a minden IP-cím csak egy egyedi mintavételi portot.</span><span class="sxs-lookup"><span data-stu-id="602d0-140">For SAP ASCS/SCS cluster instances, each IP address requires a unique probe port.</span></span> <span data-ttu-id="602d0-141">Például egy IP-cím, egy Azure belső terheléselosztón 62300 mintavételi portot használ, ha nincs más IP-címet a terheléselosztóhoz mintavételi portot 62300 is használhat.</span><span class="sxs-lookup"><span data-stu-id="602d0-141">For example, if one IP address on an Azure internal load balancer uses probe port 62300, no other IP address on that load balancer can use probe port 62300.</span></span>
>
><span data-ttu-id="602d0-142">A célokra mintavételi portot 62300 már foglalt, mert használjuk 62350 mintavételi portot.</span><span class="sxs-lookup"><span data-stu-id="602d0-142">For our purposes, because probe port 62300 is already reserved, we are using probe port 62350.</span></span>

<span data-ttu-id="602d0-143">SAP ASC/SCS további példányokat is telepíthet hello létező WSFC-fürtön két csomóponttal rendelkező:</span><span class="sxs-lookup"><span data-stu-id="602d0-143">You can install additional SAP ASCS/SCS instances in hello existing WSFC cluster with two nodes:</span></span>

| <span data-ttu-id="602d0-144">Virtuálisgép-szerepkör</span><span class="sxs-lookup"><span data-stu-id="602d0-144">Virtual machine role</span></span> | <span data-ttu-id="602d0-145">Virtuális gép állomásneve</span><span class="sxs-lookup"><span data-stu-id="602d0-145">Virtual machine host name</span></span> | <span data-ttu-id="602d0-146">Statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="602d0-146">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="602d0-147">1. fürtcsomópont ASC/SCS-példány</span><span class="sxs-lookup"><span data-stu-id="602d0-147">1st cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="602d0-148">PR1-ASC-0</span><span class="sxs-lookup"><span data-stu-id="602d0-148">pr1-ascs-0</span></span> |<span data-ttu-id="602d0-149">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="602d0-149">10.0.0.10</span></span> |
| <span data-ttu-id="602d0-150">2. fürtcsomópont ASC/SCS-példány</span><span class="sxs-lookup"><span data-stu-id="602d0-150">2nd cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="602d0-151">PR1-ASC-1</span><span class="sxs-lookup"><span data-stu-id="602d0-151">pr1-ascs-1</span></span> |<span data-ttu-id="602d0-152">10.0.0.9</span><span class="sxs-lookup"><span data-stu-id="602d0-152">10.0.0.9</span></span> |

### <a name="create-a-virtual-host-name-for-hello-clustered-sap-ascsscs-instance-on-hello-dns-server"></a><span data-ttu-id="602d0-153">Hozzon létre egy virtuális nevet fürtözött hello SAP ASC/SCS példány hello DNS-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="602d0-153">Create a virtual host name for hello clustered SAP ASCS/SCS instance on hello DNS server</span></span>

<span data-ttu-id="602d0-154">Egy DNS-bejegyzést hello ASC/SCS példány hello virtuális állomás nevét a következő paraméterek hello segítségével hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="602d0-154">You can create a DNS entry for hello virtual host name of hello ASCS/SCS instance by using hello following parameters:</span></span>

| <span data-ttu-id="602d0-155">Új SAP ASC/SCS virtuális állomás neve</span><span class="sxs-lookup"><span data-stu-id="602d0-155">New SAP ASCS/SCS virtual host name</span></span> | <span data-ttu-id="602d0-156">Társított IP-cím</span><span class="sxs-lookup"><span data-stu-id="602d0-156">Associated IP address</span></span> |
| --- | --- | --- |
|<span data-ttu-id="602d0-157">pr5-sap-cl</span><span class="sxs-lookup"><span data-stu-id="602d0-157">pr5-sap-cl</span></span> |<span data-ttu-id="602d0-158">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="602d0-158">10.0.0.50</span></span> |

<span data-ttu-id="602d0-159">hello új állomás nevének és IP-cím hello DNS-kezelőben, ahogy az alábbi képernyőfelvétel a hello láthatók:</span><span class="sxs-lookup"><span data-stu-id="602d0-159">hello new host name and IP address are displayed in hello DNS Manager, as shown in hello following screenshot:</span></span>

![DNS-kezelő lista hello kiemelés definiált hello új SAP ASC/SCS fürt virtuális nevét és a TCP/IP-cím DNS-bejegyzés][sap-ha-guide-figure-6004]

<span data-ttu-id="602d0-161">hello való létrehozásakor a DNS-bejegyzés is eljárása fő hello részletes [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="602d0-161">hello procedure for creating a DNS entry is also described in detail in hello main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9.1.1].</span></span>

> [!NOTE]
> <span data-ttu-id="602d0-162">hello új IP-cím hozzárendelése hello további ASC/SCS példány toohello virtuális állomás nevét kell lennie hello ugyanaz, mint a hello toohello SAP Azure terheléselosztóhoz rendelt új IP-címet.</span><span class="sxs-lookup"><span data-stu-id="602d0-162">hello new IP address that you assign toohello virtual host name of hello additional ASCS/SCS instance must be hello same as hello new IP address that you assigned toohello SAP Azure load balancer.</span></span>
>
><span data-ttu-id="602d0-163">A mi esetünkben hello IP-cím 10.0.0.50.</span><span class="sxs-lookup"><span data-stu-id="602d0-163">In our scenario, hello IP address is 10.0.0.50.</span></span>

### <a name="add-an-ip-address-tooan-existing-azure-internal-load-balancer-by-using-powershell"></a><span data-ttu-id="602d0-164">IP cím tooan meglévő Azure belső terheléselosztó hozzáadása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="602d0-164">Add an IP address tooan existing Azure internal load balancer by using PowerShell</span></span>

<span data-ttu-id="602d0-165">egy SAP ASC/SCS példánya a nagyobb toocreate hello azonos WSFC fürt esetén használjon PowerShell tooadd egy meglévő Azure belső terheléselosztó IP-cím tooan.</span><span class="sxs-lookup"><span data-stu-id="602d0-165">toocreate more than one SAP ASCS/SCS instance in hello same WSFC cluster, use PowerShell tooadd an IP address tooan existing Azure internal load balancer.</span></span> <span data-ttu-id="602d0-166">Minden IP-címet igényel a saját terheléselosztási szabályok, a mintavételi portot, az előtér-IP-készlet és a háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="602d0-166">Each IP address requires its own load-balancing rules, probe port, front-end IP pool, and back-end pool.</span></span>

<span data-ttu-id="602d0-167">hello következő parancsfájl hozzáad egy új IP cím tooan meglévő terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="602d0-167">hello following script adds a new IP address tooan existing load balancer.</span></span> <span data-ttu-id="602d0-168">Hello PowerShell változók a környezet frissítése.</span><span class="sxs-lookup"><span data-stu-id="602d0-168">Update hello PowerShell variables for your environment.</span></span> <span data-ttu-id="602d0-169">hello parancsfájlt hoz létre minden SAP ASC/SCS porthoz valamennyi szükséges terheléselosztási szabályok.</span><span class="sxs-lookup"><span data-stu-id="602d0-169">hello script will create all needed load-balancing rules for all SAP ASCS/SCS ports.</span></span>

```powershell

# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get hello Azure VNet and subnet
$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzureRmLoadBalancer

# Get new updated configuration
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzureRmLoadBalancer

# Get new updated config
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs toobackend pool
$BEPool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of hello '$VMName' VM toohello backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        #start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for hello ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for hello port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzureRmLoadBalancer

Write-Host "Succesfully added new IP '$ILBIP' toohello internal load balancer '$ILBName'!" -ForegroundColor Green

```
<span data-ttu-id="602d0-170">Miután hello parancsfájl lefutott, jelennek meg hello hello Azure-portálon, ahogy az alábbi képernyőfelvétel a hello:</span><span class="sxs-lookup"><span data-stu-id="602d0-170">After hello script has run, hello results are displayed in hello Azure portal, as shown in hello following screenshot:</span></span>

![Hello Azure-portálon található új előtér-IP-készlet][sap-ha-guide-figure-6005]

### <a name="add-disks-toocluster-machines-and-configure-hello-sios-cluster-share-disk"></a><span data-ttu-id="602d0-172">Lemezek toocluster hozzáadni, és konfigurálja a hello SIOS fürtlemez megosztás</span><span class="sxs-lookup"><span data-stu-id="602d0-172">Add disks toocluster machines, and configure hello SIOS cluster share disk</span></span>

<span data-ttu-id="602d0-173">Hozzá kell adnia egy új megosztás fürtlemez minden további SAP ASC/SCS-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="602d0-173">You must add a new cluster share disk for each additional SAP ASCS/SCS instance.</span></span> <span data-ttu-id="602d0-174">A Windows Server 2012 R2 hello WSFC megosztás fürtlemez jelenleg használatban, hello SIOS DataKeeper szoftveres megoldás.</span><span class="sxs-lookup"><span data-stu-id="602d0-174">For Windows Server 2012 R2, hello WSFC cluster share disk currently in use is hello SIOS DataKeeper software solution.</span></span>

<span data-ttu-id="602d0-175">A következő hello:</span><span class="sxs-lookup"><span data-stu-id="602d0-175">Do hello following:</span></span>
1. <span data-ttu-id="602d0-176">Adja hozzá a további lemezként vagy lemezekként, hello azonos méretűnek (amely kell toostripe) tooeach hello a fürtcsomópontok és formázni őket.</span><span class="sxs-lookup"><span data-stu-id="602d0-176">Add an additional disk or disks of hello same size (which you need toostripe) tooeach of hello cluster nodes, and format them.</span></span>
2. <span data-ttu-id="602d0-177">Storage replikáció konfigurálását a SIOS DataKeeper.</span><span class="sxs-lookup"><span data-stu-id="602d0-177">Configure storage replication with SIOS DataKeeper.</span></span>

<span data-ttu-id="602d0-178">Az eljárás azt feltételezi, hogy még telepítette SIOS DataKeeper hello WSFC-fürt gépeken.</span><span class="sxs-lookup"><span data-stu-id="602d0-178">This procedure assumes that you have already installed SIOS DataKeeper on hello WSFC cluster machines.</span></span> <span data-ttu-id="602d0-179">Ha már telepítette azt, hello gépek közötti replikáció most konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="602d0-179">If you have installed it, you must now configure replication between hello machines.</span></span> <span data-ttu-id="602d0-180">a fő hello részletes leírása a hello folyamat [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide-8.12.3.3].</span><span class="sxs-lookup"><span data-stu-id="602d0-180">hello process is described in detail in hello main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.12.3.3].</span></span>  

![A szinkron tükrözés hello új SAP ASC/SCS megosztás lemez DataKeeper][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a><span data-ttu-id="602d0-182">Virtuális gépek SAP alkalmazáskiszolgálókra és az adatbázis-kezelő fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="602d0-182">Deploy VMs for SAP application servers and DBMS cluster</span></span>

<span data-ttu-id="602d0-183">toocomplete hello infrastruktúra előkészítése hello második SAP rendszer hello a következő:</span><span class="sxs-lookup"><span data-stu-id="602d0-183">toocomplete hello infrastructure preparation for hello second SAP system, do hello following:</span></span>

1. <span data-ttu-id="602d0-184">Az SAP alkalmazáskiszolgálók dedikált virtuális gépek telepítése, és azokat a saját dedikált rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="602d0-184">Deploy dedicated VMs for SAP application servers and put them in their own dedicated availability group.</span></span>
2. <span data-ttu-id="602d0-185">Adatbázis-kezelő fürt dedikált virtuális gépek telepítése, és azokat a saját dedikált rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="602d0-185">Deploy dedicated VMs for DBMS cluster and put them in their own dedicated availability group.</span></span>


## <a name="install-hello-second-sap-sid2-netweaver-system"></a><span data-ttu-id="602d0-186">Hello második SAP SID2 NetWeaver rendszer telepítése</span><span class="sxs-lookup"><span data-stu-id="602d0-186">Install hello second SAP SID2 NetWeaver system</span></span>

<span data-ttu-id="602d0-187">hello teljes egy második SAP SID2 rendszer telepítését eljárást a fő hello [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide-9].</span><span class="sxs-lookup"><span data-stu-id="602d0-187">hello complete process of installing a second SAP SID2 system is described in hello main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9].</span></span>

<span data-ttu-id="602d0-188">hello magas szintű eljárás a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="602d0-188">hello high-level procedure is as follows:</span></span>

1. <span data-ttu-id="602d0-189">[Hello SAP első fürtcsomópontra telepíteni][sap-ha-guide-9.1.2].</span><span class="sxs-lookup"><span data-stu-id="602d0-189">[Install hello SAP first cluster node][sap-ha-guide-9.1.2].</span></span>  
 <span data-ttu-id="602d0-190">Ebben a lépésben telepíti SAP, ha a magas rendelkezésre állású ASC/SCS példánya hello **létező WSFC-fürt csomópont 1**.</span><span class="sxs-lookup"><span data-stu-id="602d0-190">In this step, you are installing SAP with a high-availability ASCS/SCS instance on hello **EXISTING WSFC cluster node 1**.</span></span>

2. <span data-ttu-id="602d0-191">[Hello SAP profil hello ASC/SCS példány módosítása][sap-ha-guide-9.1.3].</span><span class="sxs-lookup"><span data-stu-id="602d0-191">[Modify hello SAP profile of hello ASCS/SCS instance][sap-ha-guide-9.1.3].</span></span>

3. <span data-ttu-id="602d0-192">[Konfigurálja a mintavételi portot][sap-ha-guide-9.1.4].</span><span class="sxs-lookup"><span data-stu-id="602d0-192">[Configure a probe port][sap-ha-guide-9.1.4].</span></span>  
 <span data-ttu-id="602d0-193">Ebben a lépésben a PowerShell használatával konfigurál egy SAP-erőforrás SAP-SID2-IP mintavételi portot.</span><span class="sxs-lookup"><span data-stu-id="602d0-193">In this step, you are configuring an SAP cluster resource SAP-SID2-IP probe port by using PowerShell.</span></span> <span data-ttu-id="602d0-194">Hajtsa végre ezt a konfigurációt hello SAP ASC/SCS fürtcsomópontok egyike.</span><span class="sxs-lookup"><span data-stu-id="602d0-194">Execute this configuration on one of hello SAP ASCS/SCS cluster nodes.</span></span>

4. <span data-ttu-id="602d0-195">[Hello adatbázispéldány telepítés] [sap-magas rendelkezésre állású-útmutató-9.2].</span><span class="sxs-lookup"><span data-stu-id="602d0-195">[Install hello database instance][sap-ha-guide-9.2].</span></span>  
 <span data-ttu-id="602d0-196">Ebben a lépésben kívánja telepíteni az adatbázis-kezelő egy dedikált WSFC-fürtre.</span><span class="sxs-lookup"><span data-stu-id="602d0-196">In this step, you are installing DBMS on a dedicated WSFC cluster.</span></span>

5. <span data-ttu-id="602d0-197">[Hello második fürtcsomópont telepítés] [sap-magas rendelkezésre állású-útmutató-9.3].</span><span class="sxs-lookup"><span data-stu-id="602d0-197">[Install hello second cluster node][sap-ha-guide-9.3].</span></span>  
 <span data-ttu-id="602d0-198">Ebben a lépésben telepíti, ha a magas rendelkezésre állású ASC/SCS példánya hello létező WSFC fürtcsomóponton 2 SAP.</span><span class="sxs-lookup"><span data-stu-id="602d0-198">In this step, you are installing SAP with a high-availability ASCS/SCS instance on hello existing WSFC cluster node 2.</span></span>

6. <span data-ttu-id="602d0-199">Nyissa meg a Windows tűzfal portjainak hello SAP ASC/SCS példányhoz, és ProbePort.</span><span class="sxs-lookup"><span data-stu-id="602d0-199">Open Windows Firewall ports for hello SAP ASCS/SCS instance and ProbePort.</span></span>  
 <span data-ttu-id="602d0-200">Mindkét fürtcsomóponton SAP ASC/SCS-példányok használt az összes Windows tűzfal portjainak SAP ASC/SCS használt nyitja meg.</span><span class="sxs-lookup"><span data-stu-id="602d0-200">On both cluster nodes that are used for SAP ASCS/SCS instances, you are opening all Windows Firewall ports that are used by SAP ASCS/SCS.</span></span> <span data-ttu-id="602d0-201">Ezeket a portokat hello szereplő [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide-8.8].</span><span class="sxs-lookup"><span data-stu-id="602d0-201">These ports are listed in hello [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.8].</span></span>  
 <span data-ttu-id="602d0-202">Hello Azure belső terheléselosztási terheléselosztó mintavételi portot, amely a mi esetünkben 62350 is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="602d0-202">Also open hello Azure internal load balancer probe port, which is 62350 in our scenario.</span></span>

7. <span data-ttu-id="602d0-203">[Hello indítási típust hello SAP SSZON Windows szolgáltatáspéldány módosítása][sap-ha-guide-9.4].</span><span class="sxs-lookup"><span data-stu-id="602d0-203">[Change hello start type of hello SAP ERS Windows service instance][sap-ha-guide-9.4].</span></span>

8. <span data-ttu-id="602d0-204">[Hello SAP elsődleges alkalmazáskiszolgáló telepítése] [ sap-ha-guide-9.5] új hello a dedikált virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="602d0-204">[Install hello SAP primary application server][sap-ha-guide-9.5] on hello new dedicated VM.</span></span>

9. <span data-ttu-id="602d0-205">[Hello SAP további alkalmazáskiszolgáló telepítése] [ sap-ha-guide-9.6] új hello a dedikált virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="602d0-205">[Install hello SAP additional application server][sap-ha-guide-9.6] on hello new dedicated VM.</span></span>

10. <span data-ttu-id="602d0-206">[Tesztelje a hello SAP ASC/SCS példány feladatátvétel és SIOS replikációs][sap-ha-guide-10].</span><span class="sxs-lookup"><span data-stu-id="602d0-206">[Test hello SAP ASCS/SCS instance failover and SIOS replication][sap-ha-guide-10].</span></span>

## <a name="next-steps"></a><span data-ttu-id="602d0-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="602d0-207">Next steps</span></span>

- <span data-ttu-id="602d0-208">[Hálózatkezelés korlátok: az Azure Resource Manager][networking-limits-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="602d0-208">[Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager]</span></span>
- <span data-ttu-id="602d0-209">[Több virtuális IP-címek az Azure terheléselosztó][load-balancer-multivip-overview]</span><span class="sxs-lookup"><span data-stu-id="602d0-209">[Multiple VIPs for Azure Load Balancer][load-balancer-multivip-overview]</span></span>
- <span data-ttu-id="602d0-210">[Útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="602d0-210">[Guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide]</span></span>
