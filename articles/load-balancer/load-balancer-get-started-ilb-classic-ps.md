---
title: "Azure-alapú belső terheléselosztó létrehozása – klasszikus PowerShell | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre belső terheléselosztó a PowerShell használatával a klasszikus üzembehelyezési modellben"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 3be93168-3787-45a5-a194-9124fe386493
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: f701fb3564c62cf8088cc4362a10c5e2c2301ae6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a><span data-ttu-id="b009f-103">Bevezetés a belső terheléselosztó (klasszikus) PowerShell használatával történő létrehozásába</span><span class="sxs-lookup"><span data-stu-id="b009f-103">Get started creating an internal load balancer (classic) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b009f-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b009f-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="b009f-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b009f-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="b009f-106">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b009f-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="b009f-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b009f-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="b009f-108">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b009f-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="b009f-109">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="b009f-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="b009f-110">Ismerje meg, [hogyan hajthatja végre ezeket a lépéseket a Resource Manager-modell használatával](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b009f-110">Learn how to [perform these steps using the Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="b009f-111">Belső terheléselosztó készlet létrehozása a virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="b009f-111">Create an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="b009f-112">Egy belső terheléselosztó készletnek, illetve azoknak a kiszolgálóknak a létrehozásához, amelyek a forgalmukat küldik rá, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="b009f-112">To create an internal load balancer set and the servers that will send their traffic to it, you have to do the following:</span></span>

1. <span data-ttu-id="b009f-113">Hozzon létre egy belső terheléselosztási példányt, amely annak a bejövő forgalomnak a végpontja lesz, amelynek a terhelését el kell osztani az elosztott terhelésű készlet kiszolgálóin.</span><span class="sxs-lookup"><span data-stu-id="b009f-113">Create an instance of Internal Load Balancing that will be the endpoint of incoming traffic to be load balanced across the servers of a load-balanced set.</span></span>
2. <span data-ttu-id="b009f-114">Adja hozzá a virtuális gépekhez rendelt végpontokat, amelyek a bejövő forgalmat fogják kapni.</span><span class="sxs-lookup"><span data-stu-id="b009f-114">Add endpoints corresponding to the virtual machines that will be receiving the incoming traffic.</span></span>
3. <span data-ttu-id="b009f-115">Konfigurálja úgy a kiszolgálókat, amelyek a terheléselosztóra fogják küldeni a forgalmat, hogy azt a belső terheléselosztási példány virtuális IP-címére (VIP) küldjék.</span><span class="sxs-lookup"><span data-stu-id="b009f-115">Configure the servers that will be sending the traffic to be load balanced to send their traffic to the virtual IP (VIP) address of the Internal Load Balancing instance.</span></span>

### <a name="step-1-create-an-internal-load-balancing-instance"></a><span data-ttu-id="b009f-116">1. lépés: Belső terheléselosztási példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="b009f-116">Step 1: Create an Internal Load Balancing instance</span></span>

<span data-ttu-id="b009f-117">Egy meglévő felhőalapú szolgáltatás vagy egy regionális virtuális hálózat alatt üzembe helyezett felhőalapú szolgáltatás esetében a következő Windows PowerShell-parancsokkal hozhat létre belső terheléselosztási példányt:</span><span class="sxs-lookup"><span data-stu-id="b009f-117">For an existing cloud service or a cloud service deployed under a regional virtual network, you can create an Internal Load Balancing instance with the following Windows PowerShell commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
$ilb="<Name of your ILB instance>"
$subnet="<Name of the subnet within your virtual network>"
$IP="<The IPv4 address to use on the subnet-optional>"

Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP
```

<span data-ttu-id="b009f-118">Vegye figyelembe, hogy az [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell-parancsmag ilyen használata a DefaultProbe paraméterkészletet használja.</span><span class="sxs-lookup"><span data-stu-id="b009f-118">Note that this use of the [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell cmdlet uses the DefaultProbe parameter set.</span></span> <span data-ttu-id="b009f-119">A további paraméterkészletekkel kapcsolatos további információkért lásd: [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span><span class="sxs-lookup"><span data-stu-id="b009f-119">For more information on additional parameter sets, see [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span></span>

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a><span data-ttu-id="b009f-120">2. lépés: Végpontok hozzáadása a belső terheléselosztási példányhoz</span><span class="sxs-lookup"><span data-stu-id="b009f-120">Step 2: Add endpoints to the Internal Load Balancing instance</span></span>

<span data-ttu-id="b009f-121">Például:</span><span class="sxs-lookup"><span data-stu-id="b009f-121">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
$lbsetname="lbset"
$prot="tcp"
$locport=1433
$pubport=1433
$ilb="ilbset"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a><span data-ttu-id="b009f-122">3. lépés: A kiszolgálók konfigurálása, hogy a forgalmat az új belső terheléselosztási végpontra küldjék</span><span class="sxs-lookup"><span data-stu-id="b009f-122">Step 3: Configure your servers to send their traffic to the new Internal Load Balancing endpoint</span></span>

<span data-ttu-id="b009f-123">Azokat a kiszolgálókat, amelyeknek a forgalmán terheléselosztás történik, úgy kell konfigurálni, hogy a belső terheléselosztási példány új IP-címét (a VIP-címet) használják.</span><span class="sxs-lookup"><span data-stu-id="b009f-123">You have to  configure the servers whose traffic is going to be load balanced to use the new IP address (the VIP) of the Internal Load Balancing instance.</span></span> <span data-ttu-id="b009f-124">Ez az a cím, amelyet a belső terheléselosztási példány figyel.</span><span class="sxs-lookup"><span data-stu-id="b009f-124">This is the address on which the Internal Load Balancing instance is listening.</span></span> <span data-ttu-id="b009f-125">A legtöbb esetben csak hozzá kell adnia egy DNS-rekordot a belső terheléselosztási példány VIP-címéhez, vagy módosítania kell a DNS-rekordot.</span><span class="sxs-lookup"><span data-stu-id="b009f-125">In most cases, you need to just add or modify a DNS record for the VIP of the Internal Load Balancing instance.</span></span>

<span data-ttu-id="b009f-126">Ha a belső terheléselosztási példány létrehozása során adta meg az IP-címet, már rendelkezik a VIP-címmel.</span><span class="sxs-lookup"><span data-stu-id="b009f-126">If you specified the IP address during the creation of the Internal Load Balancing instance, you already have the VIP.</span></span> <span data-ttu-id="b009f-127">Ellenkező esetben a VIP-cím a következő parancsokkal tekinthető meg:</span><span class="sxs-lookup"><span data-stu-id="b009f-127">Otherwise, you can see the VIP from the following commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="b009f-128">A parancsok használatához töltse ki az értékeket, és távolítsa el a < és a > karaktereket.</span><span class="sxs-lookup"><span data-stu-id="b009f-128">To use these commands, fill in the values and remove the < and >.</span></span> <span data-ttu-id="b009f-129">Például:</span><span class="sxs-lookup"><span data-stu-id="b009f-129">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="b009f-130">A Get-AzureInternalLoadBalancer parancs kimenetéből írja fel az IP-címet, és végezze el a megfelelő változtatásokat a kiszolgálókon vagy a DNS-rekordokon annak a biztosítására, hogy a rendszer a VIP-címre küldje a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="b009f-130">From the display of the Get-AzureInternalLoadBalancer command, note the IP address and make the necessary changes to your servers or DNS records to ensure that traffic gets sent to the VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="b009f-131">A Microsoft Azure platform egy statikus, nyilvánosan irányítható IPv4-címet használ számos különböző felügyeleti forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="b009f-131">The Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="b009f-132">Az IP-cím a 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="b009f-132">The IP address is 168.63.129.16.</span></span> <span data-ttu-id="b009f-133">Ezt az IP-címet nem blokkolhatja egy tűzfal sem, mert ez nem várt viselkedést okozhat.</span><span class="sxs-lookup"><span data-stu-id="b009f-133">This IP address should not be blocked by any firewalls, because it can cause unexpected behavior.</span></span>
> <span data-ttu-id="b009f-134">Az Azure belső terheléselosztás esetében ezt az IP-címet használják a terheléselosztó figyelési mintavételezői az elosztott terhelésű készlet virtuális gépeinek állapotmeghatározásához.</span><span class="sxs-lookup"><span data-stu-id="b009f-134">With respect to Azure Internal Load Balancing, this IP address is used by monitoring probes from the load balancer to determine the health state for virtual machines in a load balanced set.</span></span> <span data-ttu-id="b009f-135">Ha egy belső elosztott terhelésű készletben hálózati biztonsági csoportot használnak az Azure virtuális gépekre irányuló forgalom korlátozásához, vagy egy virtuális alhálózaton alkalmazzák, győződjön meg arról, hogy a 168.63.129.16 címről érkező adatforgalom engedélyezéséhez felvettek egy hálózati biztonsági szabályt.</span><span class="sxs-lookup"><span data-stu-id="b009f-135">If a Network Security Group is used to restrict traffic to Azure virtual machines in an internally load-balanced set or is applied to a Virtual Network Subnet, ensure that a Network Security Rule is added to allow traffic from 168.63.129.16.</span></span>

## <a name="example-of-internal-load-balancing"></a><span data-ttu-id="b009f-136">Példa a belső terheléselosztásra</span><span class="sxs-lookup"><span data-stu-id="b009f-136">Example of internal load balancing</span></span>

<span data-ttu-id="b009f-137">A következő szakaszok lépésenként mutatják be egy elosztott terhelésű készlet létrehozásának teljes folyamatát két példakonfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="b009f-137">To step you through the end-to end process of creating a load-balanced set for two example configurations, see the following sections.</span></span>

### <a name="an-internet-facing-multi-tier-application"></a><span data-ttu-id="b009f-138">Az internetről elérhető, több rétegből álló alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b009f-138">An Internet facing, multi-tier application</span></span>

<span data-ttu-id="b009f-139">Elosztott terhelésű adatbázis-szolgáltatást szeretne biztosítani internetről elérhető webkiszolgálók készlete számára.</span><span class="sxs-lookup"><span data-stu-id="b009f-139">You want to provide a load balanced database service for  a set of Internet-facing web servers.</span></span> <span data-ttu-id="b009f-140">Mindkét kiszolgálókészletet ugyanaz az Azure felhőszolgáltatás üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="b009f-140">Both sets of servers are hosted in a single Azure cloud service.</span></span> <span data-ttu-id="b009f-141">Az 1433-as TCP-portra irányuló webkiszolgálói forgalmat két virtuális gép között kell elosztani az adatbázisszinten.</span><span class="sxs-lookup"><span data-stu-id="b009f-141">Web server traffic to TCP port 1433 must be distributed among two virtual machines in the database tier.</span></span> <span data-ttu-id="b009f-142">Az 1. ábra a konfigurációt mutatja.</span><span class="sxs-lookup"><span data-stu-id="b009f-142">Figure 1 shows the configuration.</span></span>

![Belső elosztott terhelésű készlet az adatbázisszint számára](./media/load-balancer-internal-getstarted/IC736321.png)

<span data-ttu-id="b009f-144">A konfiguráció a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b009f-144">The configuration consists of the following:</span></span>

* <span data-ttu-id="b009f-145">A meglévő felhőszolgáltatás neve, amely a virtuális gépeket üzemelteti, a mytestcloud.</span><span class="sxs-lookup"><span data-stu-id="b009f-145">The existing cloud service hosting the virtual machines is named mytestcloud.</span></span>
* <span data-ttu-id="b009f-146">A két meglévő adatbázis-kiszolgáló neve DB1 és DB2.</span><span class="sxs-lookup"><span data-stu-id="b009f-146">The two existing database servers are named DB1, DB2.</span></span>
* <span data-ttu-id="b009f-147">A webszint webkiszolgálói az adatbázisszint adatbázis-kiszolgálóihoz magánhálózati IP-címek használatával csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="b009f-147">Web servers in the web tier connect to the database servers in the database tier by using the private IP address.</span></span> <span data-ttu-id="b009f-148">Egy másik megoldás saját DNS használata a virtuális hálózathoz, és A-rekord manuális regisztrálása a belső terheléselosztó készlethez.</span><span class="sxs-lookup"><span data-stu-id="b009f-148">Another option is to use your own DNS for the virtual network and manually register an A record for the internal load balancer set.</span></span>

<span data-ttu-id="b009f-149">A következő parancsok **ILBset** néven egy új belső terheléselosztási példányt konfigurálnak, és a két adatbázis-kiszolgálóra vonatkozóan végpontokat adnak hozzá a virtuális gépekhez:</span><span class="sxs-lookup"><span data-stu-id="b009f-149">The following commands configure a new Internal Load Balancing instance named **ILBset** and add endpoints to the virtual machines corresponding to the two database servers:</span></span>

```powershell
$svc="mytestcloud"
$ilb="ilbset"
Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
$prot="tcp"
$locport=1433
$pubport=1433
$epname="TCP-1433-1433"
$lbsetname="lbset"
$vmname="DB1"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

$epname="TCP-1433-1433-2"
$vmname="DB2"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

## <a name="remove-an-internal-load-balancing-configuration"></a><span data-ttu-id="b009f-150">Belső terheléselosztási konfiguráció eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b009f-150">Remove an Internal Load Balancing configuration</span></span>

<span data-ttu-id="b009f-151">Használja a következő parancsokat a virtuális gép végpontként történő eltávolítására a belső terheléselosztási példányról:</span><span class="sxs-lookup"><span data-stu-id="b009f-151">To remove a virtual machine as an endpoint from an internal load balancer instance, use the following commands:</span></span>

```powershell
$svc="<Cloud service name>"
$vmname="<Name of the VM>"
$epname="<Name of the endpoint>"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="b009f-152">A parancsok használatához töltse ki az értékeket, a < és a > karaktert eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="b009f-152">To use these commands, fill in the values, removing the < and >.</span></span>

<span data-ttu-id="b009f-153">Például:</span><span class="sxs-lookup"><span data-stu-id="b009f-153">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="b009f-154">Belső terheléselosztási példány eltávolításához egy felhőszolgáltatásból használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="b009f-154">To remove an internal load balancer instance from a cloud service, use the following commands:</span></span>

```powershell
$svc="<Cloud service name>"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

<span data-ttu-id="b009f-155">A parancsok használatához töltse ki az értékeket, és távolítsa el a < és a > karaktereket.</span><span class="sxs-lookup"><span data-stu-id="b009f-155">To use these commands, fill in the value and remove the < and >.</span></span>

<span data-ttu-id="b009f-156">Például:</span><span class="sxs-lookup"><span data-stu-id="b009f-156">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

## <a name="additional-information-about-internal-load-balancer-cmdlets"></a><span data-ttu-id="b009f-157">További információ a belső terheléselosztási parancsmagokról</span><span class="sxs-lookup"><span data-stu-id="b009f-157">Additional information about internal load balancer cmdlets</span></span>

<span data-ttu-id="b009f-158">A belső terheléselosztási parancsmagokkal kapcsolatos további információkért futtassa a Windows PowerShell parancssorában a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="b009f-158">To obtain additional information about Internal Load Balancing cmdlets, run the following commands at a Windows PowerShell prompt:</span></span>

```powershell
Get-Help New-AzureInternalLoadBalancerConfig -full
Get-Help Add-AzureInternalLoadBalancer -full
Get-Help Get-AzureInternalLoadbalancer -full
Get-Help Remove-AzureInternalLoadBalancer -full
```

## <a name="next-steps"></a><span data-ttu-id="b009f-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b009f-159">Next steps</span></span>

[<span data-ttu-id="b009f-160">A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával</span><span class="sxs-lookup"><span data-stu-id="b009f-160">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="b009f-161">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b009f-161">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

