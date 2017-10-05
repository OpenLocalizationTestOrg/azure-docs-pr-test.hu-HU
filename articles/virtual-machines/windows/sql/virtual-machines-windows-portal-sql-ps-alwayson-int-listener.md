---
title: "Konfigurálja, hogy az mindig a rendelkezésre állási csoport figyelője – a Microsoft Azure |} Microsoft Docs"
description: "Rendelkezésre állási csoport figyelői konfigurálása az Azure Resource Manager modellt, a belső terheléselosztók használatával egy vagy több IP-címekkel rendelkező."
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
ms.openlocfilehash: 74fa1e4c9cfa608a9a385f3dd82a0599fbcc421c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a><span data-ttu-id="26d91-103">Egy vagy több Always On rendelkezésre állási csoport figyelői - erőforrás-kezelő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26d91-103">Configure one or more Always On availability group listeners - Resource Manager</span></span>
<span data-ttu-id="26d91-104">Ez a témakör bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="26d91-104">This topic shows how to:</span></span>

* <span data-ttu-id="26d91-105">Hozzon létre egy belső elosztott terhelésű az SQL Server rendelkezésre állási csoportok PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="26d91-105">Create an internal load balancer for SQL Server availability groups using PowerShell cmdlets.</span></span>
* <span data-ttu-id="26d91-106">További IP-címek hozzáadása a terheléselosztó több rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="26d91-106">Add additional IP addresses to a load balancer for more than one availability group.</span></span> 

<span data-ttu-id="26d91-107">Egy rendelkezésre állási csoport figyelőjének egy virtuális hálózat neve, amely az ügyfelek kapcsolódnak az adatbázis eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="26d91-107">An availability group listener is a virtual network name that clients connect to for database access.</span></span> <span data-ttu-id="26d91-108">Az Azure virtuális gépeken a terheléselosztó a figyelő az IP-címet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="26d91-108">On Azure virtual machines, a load balancer holds the IP address for the listener.</span></span> <span data-ttu-id="26d91-109">A load balancer irányítja a forgalmat, hogy a mintavételi portot nem SQL Server példányához.</span><span class="sxs-lookup"><span data-stu-id="26d91-109">The load balancer routes traffic to the instance of SQL Server that is listening on the probe port.</span></span> <span data-ttu-id="26d91-110">Általában a rendelkezésre állási csoport belső terheléselosztót használja.</span><span class="sxs-lookup"><span data-stu-id="26d91-110">Usually, an availability group uses an internal load balancer.</span></span> <span data-ttu-id="26d91-111">Az Azure belső terheléselosztót rendelkezhet egy vagy több IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="26d91-111">An Azure internal load balancer can host one or many IP addresses.</span></span> <span data-ttu-id="26d91-112">Minden IP-cím egy adott mintavételi portot használja.</span><span class="sxs-lookup"><span data-stu-id="26d91-112">Each IP address uses a specific probe port.</span></span> <span data-ttu-id="26d91-113">Ez a dokumentum bemutatja, hogyan lehet PowerShell használatával egy terheléselosztó létrehozása vagy IP-címek hozzáadása a meglévő terheléselosztó az SQL Server rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="26d91-113">This document shows how to use PowerShell to create a load balancer, or add IP addresses to an existing load balancer for SQL Server availability groups.</span></span> 

<span data-ttu-id="26d91-114">Több IP-címek kiosztása a belső terheléselosztók lehetőséget egy új Azure-ba, és csak érhető el a Resource Manager modellt.</span><span class="sxs-lookup"><span data-stu-id="26d91-114">The ability to assign multiple IP addresses to an internal load balancer is new to Azure and is only available in Resource Manager model.</span></span> <span data-ttu-id="26d91-115">Ez a feladat befejezéséhez szükség lehet a Resource Manager modellt az Azure virtuális gépeken telepített SQL Server rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="26d91-115">To complete this task, you need to have a SQL Server availability group deployed on Azure virtual machines in Resource Manager model.</span></span> <span data-ttu-id="26d91-116">Mindkét SQL Server virtuális gépek ugyanabban a rendelkezésre állási csoportba kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="26d91-116">Both SQL Server virtual machines must belong to the same availability set.</span></span> <span data-ttu-id="26d91-117">Használhatja a [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) automatikusan a rendelkezésre állási csoport létrehozásához az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="26d91-117">You can use the [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) to automatically create the availability group in Azure Resource Manager.</span></span> <span data-ttu-id="26d91-118">Ez a sablon automatikusan létrehozza a rendelkezésre állási csoport, beleértve a belső terheléselosztó meg.</span><span class="sxs-lookup"><span data-stu-id="26d91-118">This template automatically creates the availability group, including the internal load balancer for you.</span></span> <span data-ttu-id="26d91-119">Ha kívánja, akkor [kézzel konfigurálásához az Always On rendelkezésre állási csoport](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="26d91-119">If you prefer, you can [manually configure an Always On availability group](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

<span data-ttu-id="26d91-120">Ez a témakör megköveteli, hogy a rendelkezésre állási csoportok már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="26d91-120">This topic requires that your availability groups are already configured.</span></span>  

<span data-ttu-id="26d91-121">Kapcsolódó témakörök az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="26d91-121">Related topics include:</span></span>

* [<span data-ttu-id="26d91-122">AlwaysOn rendelkezésre állási csoportok konfigurálása Azure virtuális gépen (GUI)</span><span class="sxs-lookup"><span data-stu-id="26d91-122">Configure AlwaysOn Availability Groups in Azure VM (GUI)</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [<span data-ttu-id="26d91-123">Egy VNet – VNet-kapcsolat beállítása az Azure Resource Manager és a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="26d91-123">Configure a VNet-to-VNet connection by using Azure Resource Manager and PowerShell</span></span>](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a><span data-ttu-id="26d91-124">A Windows tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26d91-124">Configure the Windows Firewall</span></span>
<span data-ttu-id="26d91-125">SQL Server-hozzáférés engedélyezése a Windows tűzfal konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="26d91-125">Configure the Windows Firewall to allow SQL Server access.</span></span> <span data-ttu-id="26d91-126">A tűzfalszabályok engedélyezése a portokat használja a figyelő mintavétel, valamint az SQL Server-példány TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="26d91-126">The firewall rules allow TCP connections to the ports use by the SQL Server instance, and the listener probe.</span></span> <span data-ttu-id="26d91-127">Részletes útmutatásért lásd: [beállítani a Windows tűzfalat a hozzáféréshez](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="26d91-127">For detailed instructions, see [Configure a Windows Firewall for Database Engine Access](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span></span> <span data-ttu-id="26d91-128">Az SQL Server portja és a mintavételi portot a bejövő szabályt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="26d91-128">Create an inbound rule for the SQL Server port and for the probe port.</span></span>

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a><span data-ttu-id="26d91-129">Mintaparancsfájl: A belső terheléselosztók létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="26d91-129">Example Script: Create an internal load balancer with PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="26d91-130">Ha létrehozta a rendelkezésre állási csoportban található, a [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), a belső terheléselosztó már létre lett hozva.</span><span class="sxs-lookup"><span data-stu-id="26d91-130">If you created your availability group with the [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), the internal load balancer was already created.</span></span> 
> 
> 

<span data-ttu-id="26d91-131">A következő PowerShell-parancsfájlt hoz létre a belső terheléselosztók, konfigurálja a terheléselosztási szabályok, és beállítja az IP-címet a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="26d91-131">The following PowerShell script creates an internal load balancer, configures the load balancing rules, and sets an IP address for the load balancer.</span></span> <span data-ttu-id="26d91-132">A parancsfájl futtatásához nyissa meg a Windows PowerShell ISE, és illessze be a parancsfájlt a parancssori panelbe.</span><span class="sxs-lookup"><span data-stu-id="26d91-132">To run the script, open Windows PowerShell ISE, and paste the script in the Script pane.</span></span> <span data-ttu-id="26d91-133">Használjon `Login-AzureRMAccount` próbál bejelentkezni a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="26d91-133">Use `Login-AzureRMAccount` to log in to PowerShell.</span></span> <span data-ttu-id="26d91-134">Ha több Azure-előfizetéssel rendelkezik, `Select-AzureRmSubscription ` az előfizetés beállításához.</span><span class="sxs-lookup"><span data-stu-id="26d91-134">If you have multiple Azure subscriptions, use `Select-AzureRmSubscription ` to set the subscription.</span></span> 

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

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the back-end configuration

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

## <span data-ttu-id="26d91-135"><a name="Add-IP"></a>Mintaparancsfájl: IP-cím hozzáadása a meglévő terheléselosztó a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="26d91-135"><a name="Add-IP"></a> Example script: Add an IP address to an existing load balancer with PowerShell</span></span>
<span data-ttu-id="26d91-136">Egynél több rendelkezésre állási csoport, vegye fel egy további IP-címet a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="26d91-136">To use more than one availability group, add an additional IP address to the load balancer.</span></span> <span data-ttu-id="26d91-137">Minden IP-cím a saját terheléselosztási szabály, a mintavételi portot és az első port szükséges.</span><span class="sxs-lookup"><span data-stu-id="26d91-137">Each IP address requires its own load balancing rule, probe port, and front port.</span></span>

<span data-ttu-id="26d91-138">Az előtér-portot használja a portot, amelyet az SQL Server-példány való csatlakozáskor használandó alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="26d91-138">The front-end port is the port that applications use to connect to the SQL Server instance.</span></span> <span data-ttu-id="26d91-139">Másik rendelkezésre állási csoportok IP-címet a azonos előtér-portot is használhat.</span><span class="sxs-lookup"><span data-stu-id="26d91-139">IP addresses for different availability groups can use the same front-end port.</span></span>

> [!NOTE]
> <span data-ttu-id="26d91-140">SQL Server rendelkezésre állási csoportok minden IP-cím szükséges egy adott mintavételi portot.</span><span class="sxs-lookup"><span data-stu-id="26d91-140">For SQL Server availability groups, each IP address requires a specific probe port.</span></span> <span data-ttu-id="26d91-141">Például ha egy IP-címet a terheléselosztóhoz 59999 mintavételi portot használ, nincs más IP-címeit, hogy a terheléselosztó mintavételi portot 59999 is használhat.</span><span class="sxs-lookup"><span data-stu-id="26d91-141">For example, if one IP address on a load balancer uses probe port 59999, no other IP addresses on that load balancer can use probe port 59999.</span></span>

* <span data-ttu-id="26d91-142">A load balancer korlátok kapcsolatos információkért lásd: **terheléselosztó magánhálózati front-end IP-cím** alatt [hálózatkezelés korlátok - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span><span class="sxs-lookup"><span data-stu-id="26d91-142">For information about load balancer limits, see **Private front end IP per load balancer** under [Networking Limits - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span></span>
* <span data-ttu-id="26d91-143">További információ a rendelkezésre állási csoport korlátok: [korlátozások (rendelkezésre állási csoportok)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span><span class="sxs-lookup"><span data-stu-id="26d91-143">For information about availability group limits, see [Restrictions (Availability Groups)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span></span>

<span data-ttu-id="26d91-144">A következő parancsfájl egy új IP-cím hozzáadása a meglévő terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="26d91-144">The following script adds a new IP address to an existing load balancer.</span></span> <span data-ttu-id="26d91-145">A Példánynak a terheléselosztási előtér-portot figyelő-portot használja.</span><span class="sxs-lookup"><span data-stu-id="26d91-145">The ILB uses the listener port for the load balancing front-end port.</span></span> <span data-ttu-id="26d91-146">Ezt a portot a portot, amelyet az SQL-kiszolgáló figyel a következőn lehet.</span><span class="sxs-lookup"><span data-stu-id="26d91-146">This port can be the port that SQL Server is listening on.</span></span> <span data-ttu-id="26d91-147">Az alapértelmezett példány az SQL Server a port az 1433-as.</span><span class="sxs-lookup"><span data-stu-id="26d91-147">For default instances of SQL Server, the port is 1433.</span></span> <span data-ttu-id="26d91-148">A terheléselosztási szabály a rendelkezésre állási csoport hozni egy fix IP-cím (közvetlen kiszolgálói válasz), így a háttér-port nem azonos az előtér-port.</span><span class="sxs-lookup"><span data-stu-id="26d91-148">The load balancing rule for an availability group requires a floating IP (direct server return) so the back-end port is the same as the front-end port.</span></span> <span data-ttu-id="26d91-149">A változók a környezet frissítése.</span><span class="sxs-lookup"><span data-stu-id="26d91-149">Update the variables for your environment.</span></span> 

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

## <a name="configure-the-listener"></a><span data-ttu-id="26d91-150">A figyelő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26d91-150">Configure the listener</span></span>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-the-listener-port-in-sql-server-management-studio"></a><span data-ttu-id="26d91-151">A figyelő port beállítása az SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="26d91-151">Set the listener port in SQL Server Management Studio</span></span>

1. <span data-ttu-id="26d91-152">Indítsa el az SQL Server Management Studio eszközt, és kapcsolódjon az elsődleges másodpéldány.</span><span class="sxs-lookup"><span data-stu-id="26d91-152">Launch SQL Server Management Studio and connect to the primary replica.</span></span>

1. <span data-ttu-id="26d91-153">Navigáljon a **AlwaysOn magas rendelkezésre állás** | **rendelkezésre állási csoportok** | **rendelkezésre állási csoport figyelői**.</span><span class="sxs-lookup"><span data-stu-id="26d91-153">Navigate to **AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**.</span></span> 

1. <span data-ttu-id="26d91-154">Meg kell jelennie a figyelő nevét, amelyet a Feladatátvevőfürt-kezelőt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="26d91-154">You should now see the listener name that you created in Failover Cluster Manager.</span></span> <span data-ttu-id="26d91-155">Kattintson a jobb gombbal a figyelő nevét, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="26d91-155">Right-click the listener name and click **Properties**.</span></span>

1. <span data-ttu-id="26d91-156">Az a **Port** mezőben adja meg a portszámot az elérhetőségi csoport figyelője az a korábban használt $EndpointPort használatával (az alapértelmezett 1433-as volt az), majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="26d91-156">In the **Port** box, specify the port number for the availability group listener by using the $EndpointPort you used earlier (1433 was the default), then click **OK**.</span></span>

## <a name="test-the-connection-to-the-listener"></a><span data-ttu-id="26d91-157">Tesztelje a kapcsolatot a figyelő</span><span class="sxs-lookup"><span data-stu-id="26d91-157">Test the connection to the listener</span></span>

<span data-ttu-id="26d91-158">A kapcsolat ellenőrzéséhez:</span><span class="sxs-lookup"><span data-stu-id="26d91-158">To test the connection:</span></span>

1. <span data-ttu-id="26d91-159">RDP egy SQL Server ugyanazon virtuális hálózatban, de nem tulajdonosa a replikát.</span><span class="sxs-lookup"><span data-stu-id="26d91-159">RDP to a SQL Server that is in the same virtual network, but does not own the replica.</span></span> <span data-ttu-id="26d91-160">Ez lehet a más SQL Server a fürtben.</span><span class="sxs-lookup"><span data-stu-id="26d91-160">This can be the other SQL Server in the cluster.</span></span>

1. <span data-ttu-id="26d91-161">Használjon **sqlcmd** segédprogram létrehozott kapcsolat ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="26d91-161">Use **sqlcmd** utility to test the connection.</span></span> <span data-ttu-id="26d91-162">Például az alábbi parancsfájlt hoz létre egy **sqlcmd** kapcsolatot az elsődleges másodpéldány, a figyelő a Windows-hitelesítés használatával:</span><span class="sxs-lookup"><span data-stu-id="26d91-162">For example, the following script establishes a **sqlcmd** connection to the primary replica through the listener with Windows authentication:</span></span>
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    <span data-ttu-id="26d91-163">Ha a figyelő az alapértelmezettől eltérő portot használ (1433) portot, adja meg a portot a kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="26d91-163">If the listener is using a port other than the default port (1433), specify the port in the connection string.</span></span> <span data-ttu-id="26d91-164">A következő sqlcmd paranccsal például egy figyelő 1435 porton csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="26d91-164">For example, the following sqlcmd command connects to a listener at port 1435:</span></span> 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

<span data-ttu-id="26d91-165">Az SQLCMD kapcsolat automatikusan csatlakozik bármely SQL Server-példányt az elsődleges replikát.</span><span class="sxs-lookup"><span data-stu-id="26d91-165">The SQLCMD connection automatically connects to whichever instance of SQL Server hosts the primary replica.</span></span> 

> [!NOTE]
> <span data-ttu-id="26d91-166">Győződjön meg arról, hogy a megadott port meg nyitva a tűzfalon az mindkét SQL-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="26d91-166">Make sure that the port you specify is open on the firewall of both SQL Servers.</span></span> <span data-ttu-id="26d91-167">A TCP-portot, amelyekkel egy bejövő forgalomra vonatkozó szabály mindkét kiszolgáló szükséges.</span><span class="sxs-lookup"><span data-stu-id="26d91-167">Both servers require an inbound rule for the TCP port that you use.</span></span> <span data-ttu-id="26d91-168">Lásd: [hozzáadása vagy szerkesztése tűzfalszabály](http://technet.microsoft.com/library/cc753558.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="26d91-168">See [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx) for more information.</span></span> 
> 
> 

## <a name="guidelines-and-limitations"></a><span data-ttu-id="26d91-169">Irányelvek és korlátozások</span><span class="sxs-lookup"><span data-stu-id="26d91-169">Guidelines and limitations</span></span>
<span data-ttu-id="26d91-170">Vegye figyelembe a következő irányelveket a rendelkezésre állási csoport figyelőjének az Azure-ban a belső terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="26d91-170">Note the following guidelines on availability group listener in Azure using internal load balancer:</span></span>

* <span data-ttu-id="26d91-171">A belső terheléselosztót csak érhető el a figyelő a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="26d91-171">With an internal load balancer, you only access the listener from within the same virtual network.</span></span>


## <a name="for-more-information"></a><span data-ttu-id="26d91-172">További információ</span><span class="sxs-lookup"><span data-stu-id="26d91-172">For more information</span></span>
<span data-ttu-id="26d91-173">További információkért lásd: [beállítása Always On rendelkezésre állási csoport az Azure virtuális gép manuálisan](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="26d91-173">For more information, see [Configure Always On availability group in Azure VM manually](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

## <a name="powershell-cmdlets"></a><span data-ttu-id="26d91-174">PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="26d91-174">PowerShell cmdlets</span></span>
<span data-ttu-id="26d91-175">A következő PowerShell-parancsmagok használatával hozzon létre az Azure virtuális gépek belső terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="26d91-175">Use the following PowerShell cmdlets to create an internal load balancer for Azure virtual machines.</span></span>

* <span data-ttu-id="26d91-176">[Új AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) új terheléselosztó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="26d91-176">[New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) creates a load balancer.</span></span> 
* <span data-ttu-id="26d91-177">[Új AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) hoz létre egy terheléselosztó előtér-IP-konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="26d91-177">[New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) creates a front-end IP configuration for a load balancer.</span></span> 
* <span data-ttu-id="26d91-178">[Új AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) hoz létre egy terheléselosztó szabály konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="26d91-178">[New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) creates a rule configuration for a load balancer.</span></span> 
* <span data-ttu-id="26d91-179">[Új AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) hoz létre egy háttér címkészlet beállítása a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="26d91-179">[New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) creates a backend address pool configuration for a load balancer.</span></span> 
* <span data-ttu-id="26d91-180">[Új AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) hoz létre egy terhelés-kiegyenlítő mintavételi konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="26d91-180">[New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) creates a probe configuration for a load balancer.</span></span>
* <span data-ttu-id="26d91-181">[Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) terheléselosztó eltávolítása egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="26d91-181">[Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) removes a load balancer from an Azure resource group.</span></span>
