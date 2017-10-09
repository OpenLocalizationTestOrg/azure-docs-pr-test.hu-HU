---
title: "egy Azure belső aaaCreate terheléselosztó - klasszikus PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy belső terheléselosztó hello klasszikus üzembe helyezési modellben a PowerShell használatával"
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
ms.openlocfilehash: 382db80c42ffab09905513019b72e85a4f9dfeff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a><span data-ttu-id="e3003-103">Bevezetés a belső terheléselosztó (klasszikus) PowerShell használatával történő létrehozásába</span><span class="sxs-lookup"><span data-stu-id="e3003-103">Get started creating an internal load balancer (classic) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e3003-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e3003-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="e3003-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e3003-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="e3003-106">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="e3003-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="e3003-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e3003-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="e3003-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="e3003-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="e3003-109">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="e3003-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="e3003-110">Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e3003-110">Learn how too[perform these steps using hello Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="e3003-111">Belső terheléselosztó készlet létrehozása a virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="e3003-111">Create an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="e3003-112">belső terheléselosztót beállítani, és szeretne küldeni a forgalom tooit kiszolgálók hello toocreate, toodo hello következő rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e3003-112">toocreate an internal load balancer set and hello servers that will send their traffic tooit, you have toodo hello following:</span></span>

1. <span data-ttu-id="e3003-113">Belső terheléselosztás hello végpont egy elosztott terhelésű készlet hello kiszolgálóin bejövő forgalom toobe terhelés leendő példányt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e3003-113">Create an instance of Internal Load Balancing that will be hello endpoint of incoming traffic toobe load balanced across hello servers of a load-balanced set.</span></span>
2. <span data-ttu-id="e3003-114">Hello bejövő forgalmat fog kapni toohello virtuális gépek megfelelő végpont-hozzáadáshoz.</span><span class="sxs-lookup"><span data-stu-id="e3003-114">Add endpoints corresponding toohello virtual machines that will be receiving hello incoming traffic.</span></span>
3. <span data-ttu-id="e3003-115">Hello forgalom toobe az elosztott terhelésű toosend a forgalom toohello virtuális IP-cím (VIP) cím hello belső terheléselosztás példány küldi hello kiszolgálók konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e3003-115">Configure hello servers that will be sending hello traffic toobe load balanced toosend their traffic toohello virtual IP (VIP) address of hello Internal Load Balancing instance.</span></span>

### <a name="step-1-create-an-internal-load-balancing-instance"></a><span data-ttu-id="e3003-116">1. lépés: Belső terheléselosztási példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="e3003-116">Step 1: Create an Internal Load Balancing instance</span></span>

<span data-ttu-id="e3003-117">Létező felhőalapú szolgáltatást, vagy egy felhőalapú szolgáltatás, a regionális virtuális hálózatot telepített a következő Windows PowerShell-parancsok hello belső terheléselosztás példány hozhatja létre:</span><span class="sxs-lookup"><span data-stu-id="e3003-117">For an existing cloud service or a cloud service deployed under a regional virtual network, you can create an Internal Load Balancing instance with hello following Windows PowerShell commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
$ilb="<Name of your ILB instance>"
$subnet="<Name of hello subnet within your virtual network>"
$IP="<hello IPv4 address toouse on hello subnet-optional>"

Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP
```

<span data-ttu-id="e3003-118">Vegye figyelembe, hogy a hello használata [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell-parancsmag hello DefaultProbe paraméterhalmaz használja.</span><span class="sxs-lookup"><span data-stu-id="e3003-118">Note that this use of hello [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell cmdlet uses hello DefaultProbe parameter set.</span></span> <span data-ttu-id="e3003-119">A további paraméterkészletekkel kapcsolatos további információkért lásd: [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span><span class="sxs-lookup"><span data-stu-id="e3003-119">For more information on additional parameter sets, see [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span></span>

### <a name="step-2-add-endpoints-toohello-internal-load-balancing-instance"></a><span data-ttu-id="e3003-120">2. lépés: Végpontok toohello belső terheléselosztás példány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e3003-120">Step 2: Add endpoints toohello Internal Load Balancing instance</span></span>

<span data-ttu-id="e3003-121">Például:</span><span class="sxs-lookup"><span data-stu-id="e3003-121">Here is an example:</span></span>

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

### <a name="step-3-configure-your-servers-toosend-their-traffic-toohello-new-internal-load-balancing-endpoint"></a><span data-ttu-id="e3003-122">3. lépés: Konfigurálja a kiszolgálók toosend a forgalom toohello új belső terheléselosztás végpontot</span><span class="sxs-lookup"><span data-stu-id="e3003-122">Step 3: Configure your servers toosend their traffic toohello new Internal Load Balancing endpoint</span></span>

<span data-ttu-id="e3003-123">Konfigurálnia kell túl hello kiszolgálók érkező forgalmat folyamatos toobe elosztott terhelésű toouse hello új IP-címét (hello VIP) hello belső terheléselosztás példány.</span><span class="sxs-lookup"><span data-stu-id="e3003-123">You have too configure hello servers whose traffic is going toobe load balanced toouse hello new IP address (hello VIP) of hello Internal Load Balancing instance.</span></span> <span data-ttu-id="e3003-124">Ez az hello cím melyik belső terheléselosztás hello példány figyel.</span><span class="sxs-lookup"><span data-stu-id="e3003-124">This is hello address on which hello Internal Load Balancing instance is listening.</span></span> <span data-ttu-id="e3003-125">A legtöbb esetben szüksége toojust hozzáadása vagy módosítása egy DNS-rekordot hello VIP hello belső terheléselosztás példány.</span><span class="sxs-lookup"><span data-stu-id="e3003-125">In most cases, you need toojust add or modify a DNS record for hello VIP of hello Internal Load Balancing instance.</span></span>

<span data-ttu-id="e3003-126">Hello belső terheléselosztás példány hello létrehozásakor megadott hello IP-címet, ha már rendelkezik hello VIP.</span><span class="sxs-lookup"><span data-stu-id="e3003-126">If you specified hello IP address during hello creation of hello Internal Load Balancing instance, you already have hello VIP.</span></span> <span data-ttu-id="e3003-127">Ellenkező esetben a következő parancsok hello hello VIP látható:</span><span class="sxs-lookup"><span data-stu-id="e3003-127">Otherwise, you can see hello VIP from hello following commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="e3003-128">toouse ezek a parancsok, töltse ki a hello értékek és eltávolítás hello < és >.</span><span class="sxs-lookup"><span data-stu-id="e3003-128">toouse these commands, fill in hello values and remove hello < and >.</span></span> <span data-ttu-id="e3003-129">Például:</span><span class="sxs-lookup"><span data-stu-id="e3003-129">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="e3003-130">A Get-AzureInternalLoadBalancer parancs hello hello megjelenítését jegyezze fel a hello IP-címet, és hello a szükséges módosításokat tooyour kiszolgálók vagy DNS-rekordok tooensure, amely a forgalom toohello VIP küldi el.</span><span class="sxs-lookup"><span data-stu-id="e3003-130">From hello display of hello Get-AzureInternalLoadBalancer command, note hello IP address and make hello necessary changes tooyour servers or DNS records tooensure that traffic gets sent toohello VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="e3003-131">hello Microsoft Azure platform különféle felügyeleti forgatókönyvekhez, amik egy nyilvánosan elérhető, statikus IPv4-címet használ.</span><span class="sxs-lookup"><span data-stu-id="e3003-131">hello Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="e3003-132">hello IP-cím 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="e3003-132">hello IP address is 168.63.129.16.</span></span> <span data-ttu-id="e3003-133">Ezt az IP-címet nem blokkolhatja egy tűzfal sem, mert ez nem várt viselkedést okozhat.</span><span class="sxs-lookup"><span data-stu-id="e3003-133">This IP address should not be blocked by any firewalls, because it can cause unexpected behavior.</span></span>
> <span data-ttu-id="e3003-134">A tekintetben tooAzure belső terheléselosztás az IP-címet használják mintavételt hello load balancer toodetermine hello állapotát a virtuális gépek elosztott terhelésű készlet figyelése.</span><span class="sxs-lookup"><span data-stu-id="e3003-134">With respect tooAzure Internal Load Balancing, this IP address is used by monitoring probes from hello load balancer toodetermine hello health state for virtual machines in a load balanced set.</span></span> <span data-ttu-id="e3003-135">Ha egy hálózati biztonsági csoportot használt toorestrict forgalom tooAzure virtuális gépek egy belső elosztott terhelésű készlet vagy alkalmazott tooa virtuális hálózati alhálózat, győződjön meg arról, hogy a hálózati biztonsági szabály legyen-e adva a 168.63.129.16 tooallow forgalom.</span><span class="sxs-lookup"><span data-stu-id="e3003-135">If a Network Security Group is used toorestrict traffic tooAzure virtual machines in an internally load-balanced set or is applied tooa Virtual Network Subnet, ensure that a Network Security Rule is added tooallow traffic from 168.63.129.16.</span></span>

## <a name="example-of-internal-load-balancing"></a><span data-ttu-id="e3003-136">Példa a belső terheléselosztásra</span><span class="sxs-lookup"><span data-stu-id="e3003-136">Example of internal load balancing</span></span>

<span data-ttu-id="e3003-137">toostep le a hello end-tooend létrehozásának folyamatán egy elosztott terhelésű készlet két példa konfigurációk esetén lásd a következő szakaszok hello.</span><span class="sxs-lookup"><span data-stu-id="e3003-137">toostep you through hello end-tooend process of creating a load-balanced set for two example configurations, see hello following sections.</span></span>

### <a name="an-internet-facing-multi-tier-application"></a><span data-ttu-id="e3003-138">Az internetről elérhető, több rétegből álló alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e3003-138">An Internet facing, multi-tier application</span></span>

<span data-ttu-id="e3003-139">Azt szeretné, hogy tooprovide Internet felé néző webes kiszolgálók egy csoportja egy elosztott terhelésű dokumentumadatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e3003-139">You want tooprovide a load balanced database service for  a set of Internet-facing web servers.</span></span> <span data-ttu-id="e3003-140">Mindkét kiszolgálókészletet ugyanaz az Azure felhőszolgáltatás üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="e3003-140">Both sets of servers are hosted in a single Azure cloud service.</span></span> <span data-ttu-id="e3003-141">Web server forgalom tooTCP 1433-as port hello adatbázis szinten lévő két virtuális gép között elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e3003-141">Web server traffic tooTCP port 1433 must be distributed among two virtual machines in hello database tier.</span></span> <span data-ttu-id="e3003-142">1. ábra hello konfiguráció látható.</span><span class="sxs-lookup"><span data-stu-id="e3003-142">Figure 1 shows hello configuration.</span></span>

![Adatbázis-rétegből hello belső elosztott terhelésű készlet](./media/load-balancer-internal-getstarted/IC736321.png)

<span data-ttu-id="e3003-144">hello konfigurációs hello következő tevődik össze:</span><span class="sxs-lookup"><span data-stu-id="e3003-144">hello configuration consists of hello following:</span></span>

* <span data-ttu-id="e3003-145">hello meglévő felhőszolgáltatás hello virtuális gépeket mytestcloud neve.</span><span class="sxs-lookup"><span data-stu-id="e3003-145">hello existing cloud service hosting hello virtual machines is named mytestcloud.</span></span>
* <span data-ttu-id="e3003-146">hello két meglévő adatbázis-kiszolgáló megnevezett D1, DB2.</span><span class="sxs-lookup"><span data-stu-id="e3003-146">hello two existing database servers are named DB1, DB2.</span></span>
* <span data-ttu-id="e3003-147">Hello webes réteg webkiszolgálók toohello Helyadatbázis-kiszolgálói adatbázis-rétegből hello hello magánhálózati IP-cím használatával kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="e3003-147">Web servers in hello web tier connect toohello database servers in hello database tier by using hello private IP address.</span></span> <span data-ttu-id="e3003-148">Egy másik lehetőség toouse a saját DNS hello virtuális hálózat, és manuálisan regisztrálni egy A rekordot hello belső terheléselosztó-készlet.</span><span class="sxs-lookup"><span data-stu-id="e3003-148">Another option is toouse your own DNS for hello virtual network and manually register an A record for hello internal load balancer set.</span></span>

<span data-ttu-id="e3003-149">hello következő parancsok konfigurálása nevű új belső terheléselosztás példány **ILBset** , és adja hozzá a végpontok toohello virtuális gépek megfelelő toohello a két adatbázis-kiszolgálóin:</span><span class="sxs-lookup"><span data-stu-id="e3003-149">hello following commands configure a new Internal Load Balancing instance named **ILBset** and add endpoints toohello virtual machines corresponding toohello two database servers:</span></span>

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

## <a name="remove-an-internal-load-balancing-configuration"></a><span data-ttu-id="e3003-150">Belső terheléselosztási konfiguráció eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e3003-150">Remove an Internal Load Balancing configuration</span></span>

<span data-ttu-id="e3003-151">a virtuális gép egy belső terheléselosztó példányból, a következő parancsok használata hello végpontjaként tooremove:</span><span class="sxs-lookup"><span data-stu-id="e3003-151">tooremove a virtual machine as an endpoint from an internal load balancer instance, use hello following commands:</span></span>

```powershell
$svc="<Cloud service name>"
$vmname="<Name of hello VM>"
$epname="<Name of hello endpoint>"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="e3003-152">toouse ezek a parancsok, töltse ki hello értékek eltávolításával hello < és >.</span><span class="sxs-lookup"><span data-stu-id="e3003-152">toouse these commands, fill in hello values, removing hello < and >.</span></span>

<span data-ttu-id="e3003-153">Például:</span><span class="sxs-lookup"><span data-stu-id="e3003-153">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="e3003-154">egy belső terheléselosztó példány egy felhőalapú szolgáltatás, a következő parancsok használata hello tooremove:</span><span class="sxs-lookup"><span data-stu-id="e3003-154">tooremove an internal load balancer instance from a cloud service, use hello following commands:</span></span>

```powershell
$svc="<Cloud service name>"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

<span data-ttu-id="e3003-155">Ezek a parancsok toouse hello érték töltse ki, és távolítsa el a hello < és >.</span><span class="sxs-lookup"><span data-stu-id="e3003-155">toouse these commands, fill in hello value and remove hello < and >.</span></span>

<span data-ttu-id="e3003-156">Például:</span><span class="sxs-lookup"><span data-stu-id="e3003-156">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

## <a name="additional-information-about-internal-load-balancer-cmdlets"></a><span data-ttu-id="e3003-157">További információ a belső terheléselosztási parancsmagokról</span><span class="sxs-lookup"><span data-stu-id="e3003-157">Additional information about internal load balancer cmdlets</span></span>

<span data-ttu-id="e3003-158">Belső terheléselosztás parancsmagokról, futtassa a következő parancsokat egy Windows PowerShell-parancssorba hello tooobtain további információ:</span><span class="sxs-lookup"><span data-stu-id="e3003-158">tooobtain additional information about Internal Load Balancing cmdlets, run hello following commands at a Windows PowerShell prompt:</span></span>

```powershell
Get-Help New-AzureInternalLoadBalancerConfig -full
Get-Help Add-AzureInternalLoadBalancer -full
Get-Help Get-AzureInternalLoadbalancer -full
Get-Help Remove-AzureInternalLoadBalancer -full
```

## <a name="next-steps"></a><span data-ttu-id="e3003-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e3003-159">Next steps</span></span>

[<span data-ttu-id="e3003-160">A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával</span><span class="sxs-lookup"><span data-stu-id="e3003-160">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="e3003-161">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e3003-161">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

