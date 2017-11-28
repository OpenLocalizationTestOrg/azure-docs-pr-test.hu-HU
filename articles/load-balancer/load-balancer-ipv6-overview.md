---
title: az Azure Load Balancer IPv6 aaaOverview |} Microsoft Docs
description: "IPv6-támogatás az Azure terheléselosztó és a virtuális gépek elosztott terhelésű ismertetése."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: "IPv6-alapú, azure load balancer, kettős verem, nyilvános IP-cím, natív ipv6, mobil, iot"
ms.assetid: 6a1d583f-a305-40fd-a94b-fa42e1943bbb
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
ms.openlocfilehash: 5b203f77d86cc1ad455f4ebb297097aef46b658d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a><span data-ttu-id="2687e-104">IPv6-alapú, az Azure Load Balancer áttekintése</span><span class="sxs-lookup"><span data-stu-id="2687e-104">Overview of IPv6 for Azure Load Balancer</span></span>

<span data-ttu-id="2687e-105">Az Internet felé néző terheléselosztók üzembe helyezhetők IPv6-címet.</span><span class="sxs-lookup"><span data-stu-id="2687e-105">Internet-facing load balancers can be deployed with an IPv6 address.</span></span> <span data-ttu-id="2687e-106">Ezenkívül tooIPv4 való kapcsolat esetén, ez lehetővé teszi a következő képességeket hello:</span><span class="sxs-lookup"><span data-stu-id="2687e-106">In addition tooIPv4 connectivity, this enables hello following capabilities:</span></span>

* <span data-ttu-id="2687e-107">Natív végpont IPv6-kapcsolatot nyilvános internetes ügyfelek és az Azure virtuális gépek (VM) közötti hello terheléselosztó keresztül.</span><span class="sxs-lookup"><span data-stu-id="2687e-107">Native end-to-end IPv6 connectivity between public Internet clients and Azure Virtual Machines (VMs) through hello load balancer.</span></span>
* <span data-ttu-id="2687e-108">Natív végpont IPv6-alapú kimenő kapcsolatok virtuális gépek és a nyilvános Internet IPv6-kompatibilis ügyfelek között.</span><span class="sxs-lookup"><span data-stu-id="2687e-108">Native end-to-end IPv6 outbound connectivity between VMs and public Internet IPv6-enabled clients.</span></span>

<span data-ttu-id="2687e-109">a következő kép hello hello IPv6 funkció az Azure Load Balancer mutatja be.</span><span class="sxs-lookup"><span data-stu-id="2687e-109">hello following picture illustrates hello IPv6 functionality for Azure Load Balancer.</span></span>

![Azure Load Balancer IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

<span data-ttu-id="2687e-111">Amennyiben a telepített, egy IPv4- vagy IPv6-kompatibilis internetes ügyfél hello nyilvános IPv4- vagy IPv6-címek (vagy állomásnevek) hello Azure internetre irányuló terheléselosztót a kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="2687e-111">Once deployed, an IPv4 or IPv6-enabled Internet client can communicate with hello public IPv4 or IPv6 addresses (or hostnames) of hello Azure Internet-facing Load Balancer.</span></span> <span data-ttu-id="2687e-112">hello load balancer útvonalak hello IPv6 csomagok toohello saját IPv6-címek hello virtuális gépek hálózati címfordítás (NAT) használ.</span><span class="sxs-lookup"><span data-stu-id="2687e-112">hello load balancer routes hello IPv6 packets toohello private IPv6 addresses of hello VMs using network address translation (NAT).</span></span> <span data-ttu-id="2687e-113">hello IPv6-alapú internetes ügyfél nem tud közvetlenül kommunikálni hello hello virtuális gépek IPv6-cím.</span><span class="sxs-lookup"><span data-stu-id="2687e-113">hello IPv6 Internet client cannot communicate directly with hello IPv6 address of hello VMs.</span></span>

## <a name="features"></a><span data-ttu-id="2687e-114">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2687e-114">Features</span></span>

<span data-ttu-id="2687e-115">Azure Resource Manager használatával telepített virtuális gépek natív IPv6-támogatást biztosít:</span><span class="sxs-lookup"><span data-stu-id="2687e-115">Native IPv6 support for VMs deployed via Azure Resource Manager provides:</span></span>

1. <span data-ttu-id="2687e-116">IPv6-alapú szolgáltatások terhelésű hello Internet IPv6-alapú ügyfelek</span><span class="sxs-lookup"><span data-stu-id="2687e-116">Load-balanced IPv6 services for IPv6 clients on hello Internet</span></span>
2. <span data-ttu-id="2687e-117">Natív IPv6 és IPv4-alapú végpontok ("kettős halmozott") virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="2687e-117">Native IPv6 and IPv4 endpoints on VMs ("dual stacked")</span></span>
3. <span data-ttu-id="2687e-118">Bejövő és kimenő által kezdeményezett natív IPv6-kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="2687e-118">Inbound and outbound-initiated native IPv6 connections</span></span>
4. <span data-ttu-id="2687e-119">Támogatott protokollok-pl.: TCP, UDP és HTTP (S) engedélyezése számos különféle szolgáltatás architektúrák</span><span class="sxs-lookup"><span data-stu-id="2687e-119">Supported protocols such as TCP, UDP, and HTTP(S) enable a full range of service architectures</span></span>

## <a name="benefits"></a><span data-ttu-id="2687e-120">Előnyök</span><span class="sxs-lookup"><span data-stu-id="2687e-120">Benefits</span></span>

<span data-ttu-id="2687e-121">Ez a funkció lehetővé teszi, hogy a fő előnyei a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2687e-121">This functionality enables hello following key benefits:</span></span>

* <span data-ttu-id="2687e-122">Felel meg, hogy új alkalmazásokat kell-e elérhető csak tooIPv6 ügyfelek igénylő kormányzati szabályzat</span><span class="sxs-lookup"><span data-stu-id="2687e-122">Meet government regulations requiring that new applications be accessible tooIPv6-only clients</span></span>
* <span data-ttu-id="2687e-123">Enable mobil és az eszközök internetes hálózata (IOT) fejlesztők toouse kettős halmozott (IPv4 + IPv6) Azure virtuális gépek tooaddress hello növekvő mobile & IOT piacok</span><span class="sxs-lookup"><span data-stu-id="2687e-123">Enable mobile and Internet of things (IOT) developers toouse dual-stacked (IPv4+IPv6) Azure Virtual Machines tooaddress hello growing mobile & IOT markets</span></span>

## <a name="details-and-limitations"></a><span data-ttu-id="2687e-124">Részletek és korlátozások</span><span class="sxs-lookup"><span data-stu-id="2687e-124">Details and limitations</span></span>

<span data-ttu-id="2687e-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="2687e-125">Details</span></span>

* <span data-ttu-id="2687e-126">hello Azure DNS-szolgáltatás mind az IPv4 és IPv6 AAAA neve rekordok tartalmazza, és mindkét rekordok hello terheléselosztóhoz válaszol.</span><span class="sxs-lookup"><span data-stu-id="2687e-126">hello Azure DNS service contains both IPv4 A and IPv6 AAAA name records and responds with both records for hello load balancer.</span></span> <span data-ttu-id="2687e-127">hello ügyfél úgy dönt, hogy mely cím (IPv4 vagy IPv6) toocommunicate rendelkező.</span><span class="sxs-lookup"><span data-stu-id="2687e-127">hello client chooses which address (IPv4 or IPv6) toocommunicate with.</span></span>
* <span data-ttu-id="2687e-128">Amikor egy virtuális Gépet egy kapcsolat tooa nyilvános Internet IPv6-csatlakozóval csatlakoztatott eszközök kezdeményez, hello VM IPv6-cím forrása a lefordított (NAT) toohello nyilvános IPv6-cím hello terheléselosztó hálózati cím.</span><span class="sxs-lookup"><span data-stu-id="2687e-128">When a VM initiates a connection tooa public Internet IPv6-connected device, hello VM's source IPv6 address is network address translated (NAT) toohello public IPv6 address of hello load balancer.</span></span>
* <span data-ttu-id="2687e-129">Hello Linux operációs rendszert futtató virtuális gépek konfigurált tooreceive DHCP IPv6-alapú IP-címnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2687e-129">VMs running hello Linux operating system must be configured tooreceive an IPv6 IP address via DHCP.</span></span> <span data-ttu-id="2687e-130">Már van Azure-katalógus hello hello Linux lemezképeket számos konfigurált toosupport IPv6 módosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="2687e-130">Many of hello Linux images in hello Azure Gallery are already configured toosupport IPv6 without modification.</span></span> <span data-ttu-id="2687e-131">További információkért lásd: [DHCPv6 konfigurálása Linux virtuális gépekhez](load-balancer-ipv6-for-linux.md)</span><span class="sxs-lookup"><span data-stu-id="2687e-131">For more information, see [Configuring DHCPv6 for Linux VMs](load-balancer-ipv6-for-linux.md)</span></span>
* <span data-ttu-id="2687e-132">Ha úgy dönt, a terheléselosztóval mintavételi állapotfigyelő toouse egy IPv4-alapú mintavétel létrehozása és hello IPv4 és IPv6-végpontot használni.</span><span class="sxs-lookup"><span data-stu-id="2687e-132">If you choose toouse a health probe with your load balancer, create an IPv4 probe and use it with both hello IPv4 and IPv6 endpoints.</span></span> <span data-ttu-id="2687e-133">Ha hello szolgáltatást a virtuális gép leáll, a hello IPv4, mind az IPv6-végpontot kiveszik elforgatás.</span><span class="sxs-lookup"><span data-stu-id="2687e-133">If hello service on your VM goes down, both hello IPv4 and IPv6 endpoints are taken out of rotation.</span></span>

<span data-ttu-id="2687e-134">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="2687e-134">Limitations</span></span>

* <span data-ttu-id="2687e-135">IPv6 terheléselosztási szabályok hello Azure-portál nem adható hozzá.</span><span class="sxs-lookup"><span data-stu-id="2687e-135">You cannot add IPv6 load balancing rules in hello Azure portal.</span></span> <span data-ttu-id="2687e-136">hello szabályok csak hello sablon, CLI-t, a PowerShell segítségével hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="2687e-136">hello rules can only be created through hello template, CLI, PowerShell.</span></span>
* <span data-ttu-id="2687e-137">Előfordulhat, hogy nem frissít meglévő virtuális gépek toouse IPv6-címeket.</span><span class="sxs-lookup"><span data-stu-id="2687e-137">You may not upgrade existing VMs toouse IPv6 addresses.</span></span> <span data-ttu-id="2687e-138">Új virtuális gépeket kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="2687e-138">You must deploy new VMs.</span></span>
* <span data-ttu-id="2687e-139">Egy IPv6-cím rendelhetők hozzá az egyes virtuális gépek tooa egyetlen hálózati adapterrel.</span><span class="sxs-lookup"><span data-stu-id="2687e-139">A single IPv6 address can be assigned tooa single network interface in each VM.</span></span>
* <span data-ttu-id="2687e-140">hello nyilvános IPv6-címek tooa virtuális gép nem lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="2687e-140">hello public IPv6 addresses cannot be assigned tooa VM.</span></span> <span data-ttu-id="2687e-141">Csak hozzárendelhető tooa terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="2687e-141">They can only be assigned tooa load balancer.</span></span>
* <span data-ttu-id="2687e-142">A nyilvános IPv6-címek hello névkeresési DNS nem tudja konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="2687e-142">You cannot configure hello reverse DNS lookup for your public IPv6 addresses.</span></span>
* <span data-ttu-id="2687e-143">virtuális gépek hello hello IPv6-címekkel nem lehetnek tagjai az Azure-Felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="2687e-143">hello VMs with hello IPv6 addresses cannot be members of an Azure Cloud Service.</span></span> <span data-ttu-id="2687e-144">Azok csatlakoztatott tooan Azure Virtual Network (VNet) és az IPv4-címét keresztül kommunikálnak egymással.</span><span class="sxs-lookup"><span data-stu-id="2687e-144">They can be connected tooan Azure Virtual Network (VNet) and communicate with each other over their IPv4 addresses.</span></span>
* <span data-ttu-id="2687e-145">Saját IPv6-címek egy erőforráscsoportot az egyes virtuális gépek telepíthetők, de nem telepíthető az egy erőforráscsoport keresztül méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="2687e-145">Private IPv6 addresses can be deployed on individual VMs in a resource group but cannot be deployed into a resource group via Scale Sets.</span></span>
* <span data-ttu-id="2687e-146">Azure virtuális gépek IPv6 tooother virtuális gépeket, más Azure-szolgáltatások vagy a helyszíni eszközök keresztül nem tud csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="2687e-146">Azure VMs cannot connect over IPv6 tooother VMs, other Azure services, or on-premises devices.</span></span> <span data-ttu-id="2687e-147">Csak kommunikálhatnak hello Azure terheléselosztó IPv6-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="2687e-147">They can only communicate with hello Azure load balancer over IPv6.</span></span> <span data-ttu-id="2687e-148">Azonban amelyekkel kommunikálhatnak IPv4 használatával ezek más erőforrások.</span><span class="sxs-lookup"><span data-stu-id="2687e-148">However, they can communicate with these other resources using IPv4.</span></span>
* <span data-ttu-id="2687e-149">Hálózati biztonsági csoport (NSG) a védelem IPv4 protokoll kettős verem (IPv4 + IPv6) telepítések esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="2687e-149">Network Security Group (NSG) protection for IPv4 is supported in dual-stack (IPv4+IPv6) deployments.</span></span> <span data-ttu-id="2687e-150">Az NSG-k toohello IPv6 végpont nem vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="2687e-150">NSGs do not apply toohello IPv6 endpoints.</span></span>
* <span data-ttu-id="2687e-151">a virtuális gép nincs hello hello IPv6 végpont kitett közvetlenül toohello internet.</span><span class="sxs-lookup"><span data-stu-id="2687e-151">hello IPv6 endpoint on hello VM is not exposed directly toohello internet.</span></span> <span data-ttu-id="2687e-152">Egy terheléselosztó mögött van.</span><span class="sxs-lookup"><span data-stu-id="2687e-152">It is behind a load balancer.</span></span> <span data-ttu-id="2687e-153">Csak a megadott hello load balancer szabályok hello portok IPv6-kapcsolaton keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="2687e-153">Only hello ports specified in hello load balancer rules are accessible over IPv6.</span></span>
* <span data-ttu-id="2687e-154">Módosítása hello IdleTimeout paraméter, az IPv6 **jelenleg nem támogatott**.</span><span class="sxs-lookup"><span data-stu-id="2687e-154">Changing hello IdleTimeout parameter for IPv6 is **currently not supported**.</span></span> <span data-ttu-id="2687e-155">hello alapértelmezett érték négy perc.</span><span class="sxs-lookup"><span data-stu-id="2687e-155">hello default is four minutes.</span></span>
* <span data-ttu-id="2687e-156">Módosítása hello loadDistributionMethod paraméter, az IPv6 **jelenleg nem támogatott**.</span><span class="sxs-lookup"><span data-stu-id="2687e-156">Changing hello loadDistributionMethod parameter for IPv6 is **currently not supported**.</span></span>
* <span data-ttu-id="2687e-157">IPv6-alapú IP-címek fenntartott (ahol IP = static) vannak **jelenleg nem támogatott**.</span><span class="sxs-lookup"><span data-stu-id="2687e-157">Reserved IPv6 IPs (where IPAllocationMethod = static) are **currently not supported**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2687e-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2687e-158">Next steps</span></span>

<span data-ttu-id="2687e-159">Megtudhatja, hogyan toodeploy IPv6 terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="2687e-159">Learn how toodeploy a load balancer with IPv6.</span></span>

* [<span data-ttu-id="2687e-160">IPv6 régiónként rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="2687e-160">Availability of IPv6 by region</span></span>](https://go.microsoft.com/fwlink/?linkid=828357)
* [<span data-ttu-id="2687e-161">Központi telepítése egy terheléselosztó IPv6-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="2687e-161">Deploy a load balancer with IPv6 using a template</span></span>](load-balancer-ipv6-internet-template.md)
* [<span data-ttu-id="2687e-162">A terheléselosztó és az IPv6 az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="2687e-162">Deploy a load balancer with IPv6 using Azure PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
* [<span data-ttu-id="2687e-163">Egy Azure CLI-vel rendelkező IPv6-alapú terheléselosztó telepítése</span><span class="sxs-lookup"><span data-stu-id="2687e-163">Deploy a load balancer with IPv6 using Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
