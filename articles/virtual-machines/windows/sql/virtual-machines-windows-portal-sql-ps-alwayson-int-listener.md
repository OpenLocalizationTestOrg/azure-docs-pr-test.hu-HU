---
title: "aaaConfigure mindig a rendelkezésre állási csoport figyelője – a Microsoft Azure |} Microsoft Docs"
description: "Konfigurálja a rendelkezésre állási csoport figyelője a hello Azure Resource Manager modellt, a belső terheléselosztók használatával egy vagy több IP-címmel."
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/22/2017
ms.author: mikeray
ms.openlocfilehash: 81edfe2c2ea536d8dcec466f36fccf8bc0e02c2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a><span data-ttu-id="b9ad8-103">Egy vagy több Always On rendelkezésre állási csoport figyelői - erőforrás-kezelő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9ad8-103">Configure one or more Always On availability group listeners - Resource Manager</span></span>
<span data-ttu-id="b9ad8-104">Ez a témakör bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="b9ad8-104">This topic shows how to:</span></span>

* <span data-ttu-id="b9ad8-105">Hozzon létre egy belső elosztott terhelésű az SQL Server rendelkezésre állási csoportok PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-105">Create an internal load balancer for SQL Server availability groups using PowerShell cmdlets.</span></span>
* <span data-ttu-id="b9ad8-106">Adjon hozzá további IP címek tooa terheléselosztó egynél több rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-106">Add additional IP addresses tooa load balancer for more than one availability group.</span></span> 

<span data-ttu-id="b9ad8-107">Egy rendelkezésre állási csoport figyelőjének egy virtuális hálózat neve, hogy az ügyfelek kapcsolódnak toofor adatbázis eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-107">An availability group listener is a virtual network name that clients connect toofor database access.</span></span> <span data-ttu-id="b9ad8-108">Az Azure virtuális gépeken a terheléselosztó hello figyelő hello IP-címet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-108">On Azure virtual machines, a load balancer holds hello IP address for hello listener.</span></span> <span data-ttu-id="b9ad8-109">hello terhelés terheléselosztó útvonalak forgalom toohello az SQL Server példányát, amely hello mintavételi portot figyel.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-109">hello load balancer routes traffic toohello instance of SQL Server that is listening on hello probe port.</span></span> <span data-ttu-id="b9ad8-110">Általában a rendelkezésre állási csoport belső terheléselosztót használja.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-110">Usually, an availability group uses an internal load balancer.</span></span> <span data-ttu-id="b9ad8-111">Az Azure belső terheléselosztót rendelkezhet egy vagy több IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-111">An Azure internal load balancer can host one or many IP addresses.</span></span> <span data-ttu-id="b9ad8-112">Minden IP-cím egy adott mintavételi portot használja.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-112">Each IP address uses a specific probe port.</span></span> <span data-ttu-id="b9ad8-113">Ez a dokumentum bemutatja, hogyan toouse PowerShell toocreate olyan terheléselosztóhoz, vagy adja hozzá az IP-címek tooan meglévő terheléselosztó SQL Server rendelkezésre állási csoportok.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-113">This document shows how toouse PowerShell toocreate a load balancer, or add IP addresses tooan existing load balancer for SQL Server availability groups.</span></span> 

<span data-ttu-id="b9ad8-114">hello képességét tooassign több IP címek tooan belső elosztott terhelésű új tooAzure, és csak Resource Manager modellt érhető el.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-114">hello ability tooassign multiple IP addresses tooan internal load balancer is new tooAzure and is only available in Resource Manager model.</span></span> <span data-ttu-id="b9ad8-115">toocomplete ebben a feladatban van szüksége a Resource Manager modellt az Azure virtuális gépeken telepített egy SQL Server rendelkezésre állási csoport toohave.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-115">toocomplete this task, you need toohave a SQL Server availability group deployed on Azure virtual machines in Resource Manager model.</span></span> <span data-ttu-id="b9ad8-116">Mindkét SQL Server virtuális gépek kell tartozniuk toohello azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-116">Both SQL Server virtual machines must belong toohello same availability set.</span></span> <span data-ttu-id="b9ad8-117">Használhatja a hello [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically hello rendelkezésre állási csoport létrehozása az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-117">You can use hello [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically create hello availability group in Azure Resource Manager.</span></span> <span data-ttu-id="b9ad8-118">Ez a sablon hello rendelkezésre állási csoportból, beleértve a belső terheléselosztó hello automatikusan létrehozza.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-118">This template automatically creates hello availability group, including hello internal load balancer for you.</span></span> <span data-ttu-id="b9ad8-119">Ha kívánja, akkor [kézzel konfigurálásához az Always On rendelkezésre állási csoport](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="b9ad8-119">If you prefer, you can [manually configure an Always On availability group](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

<span data-ttu-id="b9ad8-120">Ez a témakör megköveteli, hogy a rendelkezésre állási csoportok már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-120">This topic requires that your availability groups are already configured.</span></span>  

<span data-ttu-id="b9ad8-121">Kapcsolódó témakörök az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="b9ad8-121">Related topics include:</span></span>

* [<span data-ttu-id="b9ad8-122">AlwaysOn rendelkezésre állási csoportok konfigurálása Azure virtuális gépen (GUI)</span><span class="sxs-lookup"><span data-stu-id="b9ad8-122">Configure AlwaysOn Availability Groups in Azure VM (GUI)</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [<span data-ttu-id="b9ad8-123">Egy VNet – VNet-kapcsolat beállítása az Azure Resource Manager és a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b9ad8-123">Configure a VNet-to-VNet connection by using Azure Resource Manager and PowerShell</span></span>](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-hello-windows-firewall"></a><span data-ttu-id="b9ad8-124">A Windows tűzfal hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9ad8-124">Configure hello Windows Firewall</span></span>
<span data-ttu-id="b9ad8-125">Hello Windows tűzfal tooallow SQL Server hozzáférés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-125">Configure hello Windows Firewall tooallow SQL Server access.</span></span> <span data-ttu-id="b9ad8-126">hello tűzfalszabályok engedélyezése hello figyelő mintavétel, valamint hello SQL Server-példány TCP-kapcsolatok toohello portok használatát.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-126">hello firewall rules allow TCP connections toohello ports use by hello SQL Server instance, and hello listener probe.</span></span> <span data-ttu-id="b9ad8-127">Részletes útmutatásért lásd: [beállítani a Windows tűzfalat a hozzáféréshez](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="b9ad8-127">For detailed instructions, see [Configure a Windows Firewall for Database Engine Access](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span></span> <span data-ttu-id="b9ad8-128">SQL Server port hello és hello mintavételi portot a bejövő szabályt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-128">Create an inbound rule for hello SQL Server port and for hello probe port.</span></span>

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a><span data-ttu-id="b9ad8-129">Mintaparancsfájl: A belső terheléselosztók létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b9ad8-129">Example Script: Create an internal load balancer with PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="b9ad8-130">Ha a rendelkezésre állási csoport hello létre [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), hello belső terheléselosztóhoz már létre lett hozva.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-130">If you created your availability group with hello [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), hello internal load balancer was already created.</span></span> 
> 
> 

<span data-ttu-id="b9ad8-131">hello következő PowerShell-parancsfájlt hoz létre a belső terheléselosztók hello terheléselosztási szabályok konfigurálása és IP-cím beállítása hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-131">hello following PowerShell script creates an internal load balancer, configures hello load balancing rules, and sets an IP address for hello load balancer.</span></span> <span data-ttu-id="b9ad8-132">toorun hello parancsfájl, nyissa meg a Windows PowerShell ISE, és illessze be a hello parancsfájlt hello parancsfájl ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-132">toorun hello script, open Windows PowerShell ISE, and paste hello script in hello Script pane.</span></span> <span data-ttu-id="b9ad8-133">Használjon `Login-AzureRMAccount` a tooPowerShell toolog.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-133">Use `Login-AzureRMAccount` toolog in tooPowerShell.</span></span> <span data-ttu-id="b9ad8-134">Ha több Azure-előfizetéssel rendelkezik, `Select-AzureRmSubscription ` tooset hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-134">If you have multiple Azure subscriptions, use `Select-AzureRmSubscription ` tooset hello subscription.</span></span> 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # hello Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # hello Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for hello front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for hello back-end configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <span data-ttu-id="b9ad8-135"><a name="Add-IP"></a>Mintaparancsfájl: az IP cím tooan meglévő terheléselosztó felvétele a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b9ad8-135"><a name="Add-IP"></a> Example script: Add an IP address tooan existing load balancer with PowerShell</span></span>
<span data-ttu-id="b9ad8-136">toouse több mint egy rendelkezésre állási csoportból, vegyen fel egy további IP cím toohello terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-136">toouse more than one availability group, add an additional IP address toohello load balancer.</span></span> <span data-ttu-id="b9ad8-137">Minden IP-cím a saját terheléselosztási szabály, a mintavételi portot és az első port szükséges.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-137">Each IP address requires its own load balancing rule, probe port, and front port.</span></span>

<span data-ttu-id="b9ad8-138">hello előtér-port hello port, hogy alkalmazások tooconnect toohello SQL Server-példányt használ.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-138">hello front-end port is hello port that applications use tooconnect toohello SQL Server instance.</span></span> <span data-ttu-id="b9ad8-139">Másik rendelkezésre állási csoportok használható IP-címek hello előtér-portra.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-139">IP addresses for different availability groups can use hello same front-end port.</span></span>

> [!NOTE]
> <span data-ttu-id="b9ad8-140">SQL Server rendelkezésre állási csoportok minden IP-cím szükséges egy adott mintavételi portot.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-140">For SQL Server availability groups, each IP address requires a specific probe port.</span></span> <span data-ttu-id="b9ad8-141">Például ha egy IP-címet a terheléselosztóhoz 59999 mintavételi portot használ, nincs más IP-címeit, hogy a terheléselosztó mintavételi portot 59999 is használhat.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-141">For example, if one IP address on a load balancer uses probe port 59999, no other IP addresses on that load balancer can use probe port 59999.</span></span>

* <span data-ttu-id="b9ad8-142">A load balancer korlátok kapcsolatos információkért lásd: **terheléselosztó magánhálózati front-end IP-cím** alatt [hálózatkezelés korlátok - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span><span class="sxs-lookup"><span data-stu-id="b9ad8-142">For information about load balancer limits, see **Private front end IP per load balancer** under [Networking Limits - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span></span>
* <span data-ttu-id="b9ad8-143">További információ a rendelkezésre állási csoport korlátok: [korlátozások (rendelkezésre állási csoportok)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span><span class="sxs-lookup"><span data-stu-id="b9ad8-143">For information about availability group limits, see [Restrictions (Availability Groups)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span></span>

<span data-ttu-id="b9ad8-144">hello következő parancsfájl hozzáad egy új IP cím tooan meglévő terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-144">hello following script adds a new IP address tooan existing load balancer.</span></span> <span data-ttu-id="b9ad8-145">hello ILB hello figyelő portot hello terheléselosztási előtér-portot használja.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-145">hello ILB uses hello listener port for hello load balancing front-end port.</span></span> <span data-ttu-id="b9ad8-146">Ezt a portot, hogy az SQL Server nem hello port lehet.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-146">This port can be hello port that SQL Server is listening on.</span></span> <span data-ttu-id="b9ad8-147">Az SQL Server alapértelmezett példánya hello port az 1433-as.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-147">For default instances of SQL Server, hello port is 1433.</span></span> <span data-ttu-id="b9ad8-148">hello terheléselosztási szabály a rendelkezésre állási csoportban van szükség, egy fix IP-cím (közvetlen kiszolgálói válasz), hello háttér-portot kell hello azonos hello előtér-portjaként működik.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-148">hello load balancing rule for an availability group requires a floating IP (direct server return) so hello back-end port is hello same as hello front-end port.</span></span> <span data-ttu-id="b9ad8-149">A környezet hello változók frissítése.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-149">Update hello variables for your environment.</span></span> 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```

## <a name="configure-hello-listener"></a><span data-ttu-id="b9ad8-150">Hello figyelő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9ad8-150">Configure hello listener</span></span>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-hello-listener-port-in-sql-server-management-studio"></a><span data-ttu-id="b9ad8-151">Az SQL Server Management Studio hello figyelő port beállítása</span><span class="sxs-lookup"><span data-stu-id="b9ad8-151">Set hello listener port in SQL Server Management Studio</span></span>

1. <span data-ttu-id="b9ad8-152">Indítsa el az SQL Server Management Studio eszközt, és csatlakozzon a toohello elsődleges másodpéldány.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-152">Launch SQL Server Management Studio and connect toohello primary replica.</span></span>

1. <span data-ttu-id="b9ad8-153">Keresse meg a túl**AlwaysOn magas rendelkezésre állású** | **rendelkezésre állási csoportok** | **rendelkezésre állási csoport figyelői**.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-153">Navigate too**AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**.</span></span> 

1. <span data-ttu-id="b9ad8-154">Most látnia kell a Feladatátvevőfürt-kezelő létrehozta hello figyelő nevét.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-154">You should now see hello listener name that you created in Failover Cluster Manager.</span></span> <span data-ttu-id="b9ad8-155">Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-155">Right-click hello listener name and click **Properties**.</span></span>

1. <span data-ttu-id="b9ad8-156">A hello **Port** hello rendelkezésre állási csoport figyelőjének hello portszáma hello segítségével adja meg a korábban használt $EndpointPort (1433 volt hello alapértelmezett), majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-156">In hello **Port** box, specify hello port number for hello availability group listener by using hello $EndpointPort you used earlier (1433 was hello default), then click **OK**.</span></span>

## <a name="test-hello-connection-toohello-listener"></a><span data-ttu-id="b9ad8-157">Teszt hello kapcsolat toohello figyelő</span><span class="sxs-lookup"><span data-stu-id="b9ad8-157">Test hello connection toohello listener</span></span>

<span data-ttu-id="b9ad8-158">tootest hello kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="b9ad8-158">tootest hello connection:</span></span>

1. <span data-ttu-id="b9ad8-159">RDP tooa hello az SQL Server azonos virtuális hálózat, de nem saját hello replika.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-159">RDP tooa SQL Server that is in hello same virtual network, but does not own hello replica.</span></span> <span data-ttu-id="b9ad8-160">Is ez hello hello fürt más SQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-160">This can be hello other SQL Server in hello cluster.</span></span>

1. <span data-ttu-id="b9ad8-161">Használjon **sqlcmd** segédprogram tootest hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-161">Use **sqlcmd** utility tootest hello connection.</span></span> <span data-ttu-id="b9ad8-162">Például a következő parancsfájl hello hoz létre egy **sqlcmd** kapcsolat toohello elsődleges replika hello figyelő Windows-hitelesítés használatával:</span><span class="sxs-lookup"><span data-stu-id="b9ad8-162">For example, hello following script establishes a **sqlcmd** connection toohello primary replica through hello listener with Windows authentication:</span></span>
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    <span data-ttu-id="b9ad8-163">Ha hello figyelő eltérő portot használ hello alapértelmezett portot (1433), adja meg a hello portot hello kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-163">If hello listener is using a port other than hello default port (1433), specify hello port in hello connection string.</span></span> <span data-ttu-id="b9ad8-164">Például hello következő sqlcmd paranccsal összekapcsolja az tooa figyelési port 1435:</span><span class="sxs-lookup"><span data-stu-id="b9ad8-164">For example, hello following sqlcmd command connects tooa listener at port 1435:</span></span> 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

<span data-ttu-id="b9ad8-165">hello SQLCMD kapcsolat automatikusan csatlakozik a toowhichever példány SQL Server-gazdagépek hello elsődleges replika.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-165">hello SQLCMD connection automatically connects toowhichever instance of SQL Server hosts hello primary replica.</span></span> 

> [!NOTE]
> <span data-ttu-id="b9ad8-166">Győződjön meg arról, hogy a megadott hello port a hello tűzfalat mindkét SQL Server nyitva-e.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-166">Make sure that hello port you specify is open on hello firewall of both SQL Servers.</span></span> <span data-ttu-id="b9ad8-167">Mindkét kiszolgáló egy bejövő forgalomra vonatkozó szabály hello használt TCP-port szükséges.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-167">Both servers require an inbound rule for hello TCP port that you use.</span></span> <span data-ttu-id="b9ad8-168">Lásd: [hozzáadása vagy szerkesztése tűzfalszabály](http://technet.microsoft.com/library/cc753558.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-168">See [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx) for more information.</span></span> 
> 
> 

## <a name="guidelines-and-limitations"></a><span data-ttu-id="b9ad8-169">Irányelvek és korlátozások</span><span class="sxs-lookup"><span data-stu-id="b9ad8-169">Guidelines and limitations</span></span>
<span data-ttu-id="b9ad8-170">Vegye figyelembe a következő irányelveket a rendelkezésre állási csoport figyelőjének az Azure-ban a belső terheléselosztó hello:</span><span class="sxs-lookup"><span data-stu-id="b9ad8-170">Note hello following guidelines on availability group listener in Azure using internal load balancer:</span></span>

* <span data-ttu-id="b9ad8-171">A belső terheléselosztót, csak lépjen hello figyelő a hello belül azonos virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-171">With an internal load balancer, you only access hello listener from within hello same virtual network.</span></span>


## <a name="for-more-information"></a><span data-ttu-id="b9ad8-172">További információ</span><span class="sxs-lookup"><span data-stu-id="b9ad8-172">For more information</span></span>
<span data-ttu-id="b9ad8-173">További információkért lásd: [beállítása Always On rendelkezésre állási csoport az Azure virtuális gép manuálisan](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="b9ad8-173">For more information, see [Configure Always On availability group in Azure VM manually](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

## <a name="powershell-cmdlets"></a><span data-ttu-id="b9ad8-174">PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="b9ad8-174">PowerShell cmdlets</span></span>
<span data-ttu-id="b9ad8-175">A következő PowerShell-parancsmagok toocreate az Azure virtuális gépek belső terheléselosztót hello használata.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-175">Use hello following PowerShell cmdlets toocreate an internal load balancer for Azure virtual machines.</span></span>

* <span data-ttu-id="b9ad8-176">[Új AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) új terheléselosztó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-176">[New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) creates a load balancer.</span></span> 
* <span data-ttu-id="b9ad8-177">[Új AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) hoz létre egy terheléselosztó előtér-IP-konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-177">[New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) creates a front-end IP configuration for a load balancer.</span></span> 
* <span data-ttu-id="b9ad8-178">[Új AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) hoz létre egy terheléselosztó szabály konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-178">[New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) creates a rule configuration for a load balancer.</span></span> 
* <span data-ttu-id="b9ad8-179">[Új AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) hoz létre egy háttér címkészlet beállítása a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-179">[New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) creates a backend address pool configuration for a load balancer.</span></span> 
* <span data-ttu-id="b9ad8-180">[Új AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) hoz létre egy terhelés-kiegyenlítő mintavételi konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-180">[New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) creates a probe configuration for a load balancer.</span></span>
* <span data-ttu-id="b9ad8-181">[Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) terheléselosztó eltávolítása egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b9ad8-181">[Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) removes a load balancer from an Azure resource group.</span></span>
