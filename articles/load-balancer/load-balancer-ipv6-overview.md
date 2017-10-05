---
title: "IPv6-alapú, az Azure Load Balancer áttekintése |} Microsoft Docs"
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
ms.openlocfilehash: 8cca857314ecf37ef51700fd25aef228515ecd0a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a><span data-ttu-id="05b68-104">IPv6-alapú, az Azure Load Balancer áttekintése</span><span class="sxs-lookup"><span data-stu-id="05b68-104">Overview of IPv6 for Azure Load Balancer</span></span>

<span data-ttu-id="05b68-105">Az Internet felé néző terheléselosztók üzembe helyezhetők IPv6-címet.</span><span class="sxs-lookup"><span data-stu-id="05b68-105">Internet-facing load balancers can be deployed with an IPv6 address.</span></span> <span data-ttu-id="05b68-106">IPv4-kapcsolatot, valamint lehetővé teszi a következő lehetőségeket:</span><span class="sxs-lookup"><span data-stu-id="05b68-106">In addition to IPv4 connectivity, this enables the following capabilities:</span></span>

* <span data-ttu-id="05b68-107">Natív végpont IPv6-kapcsolatot nyilvános internetes ügyfelek és az Azure virtuális gépek (VM) között a terheléselosztó keresztül.</span><span class="sxs-lookup"><span data-stu-id="05b68-107">Native end-to-end IPv6 connectivity between public Internet clients and Azure Virtual Machines (VMs) through the load balancer.</span></span>
* <span data-ttu-id="05b68-108">Natív végpont IPv6-alapú kimenő kapcsolatok virtuális gépek és a nyilvános Internet IPv6-kompatibilis ügyfelek között.</span><span class="sxs-lookup"><span data-stu-id="05b68-108">Native end-to-end IPv6 outbound connectivity between VMs and public Internet IPv6-enabled clients.</span></span>

<span data-ttu-id="05b68-109">A következő kép bemutatja, az IPv6 működése az Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="05b68-109">The following picture illustrates the IPv6 functionality for Azure Load Balancer.</span></span>

![Azure Load Balancer IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

<span data-ttu-id="05b68-111">Amennyiben telepített, egy IPv4- vagy IPv6-kompatibilis internetes ügyfél tud kommunikálni a nyilvános IPv4- vagy IPv6-címek (vagy állomásnevek) Azure Internet felé néző terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="05b68-111">Once deployed, an IPv4 or IPv6-enabled Internet client can communicate with the public IPv4 or IPv6 addresses (or hostnames) of the Azure Internet-facing Load Balancer.</span></span> <span data-ttu-id="05b68-112">A terheléselosztó az IPv6-csomagokat a saját IPv6-címek a virtuális gépek használata a hálózati címfordítás (NAT) továbbítja.</span><span class="sxs-lookup"><span data-stu-id="05b68-112">The load balancer routes the IPv6 packets to the private IPv6 addresses of the VMs using network address translation (NAT).</span></span> <span data-ttu-id="05b68-113">Az IPv6-alapú internetes ügyfél nem tud közvetlenül kommunikálni az IPv6-címet a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="05b68-113">The IPv6 Internet client cannot communicate directly with the IPv6 address of the VMs.</span></span>

## <a name="features"></a><span data-ttu-id="05b68-114">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="05b68-114">Features</span></span>

<span data-ttu-id="05b68-115">Azure Resource Manager használatával telepített virtuális gépek natív IPv6-támogatást biztosít:</span><span class="sxs-lookup"><span data-stu-id="05b68-115">Native IPv6 support for VMs deployed via Azure Resource Manager provides:</span></span>

1. <span data-ttu-id="05b68-116">IPv6-alapú szolgáltatások terhelésű az IPv6-ügyfelek az interneten</span><span class="sxs-lookup"><span data-stu-id="05b68-116">Load-balanced IPv6 services for IPv6 clients on the Internet</span></span>
2. <span data-ttu-id="05b68-117">Natív IPv6 és IPv4-alapú végpontok ("kettős halmozott") virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="05b68-117">Native IPv6 and IPv4 endpoints on VMs ("dual stacked")</span></span>
3. <span data-ttu-id="05b68-118">Bejövő és kimenő által kezdeményezett natív IPv6-kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="05b68-118">Inbound and outbound-initiated native IPv6 connections</span></span>
4. <span data-ttu-id="05b68-119">Támogatott protokollok-pl.: TCP, UDP és HTTP (S) engedélyezése számos különféle szolgáltatás architektúrák</span><span class="sxs-lookup"><span data-stu-id="05b68-119">Supported protocols such as TCP, UDP, and HTTP(S) enable a full range of service architectures</span></span>

## <a name="benefits"></a><span data-ttu-id="05b68-120">Előnyök</span><span class="sxs-lookup"><span data-stu-id="05b68-120">Benefits</span></span>

<span data-ttu-id="05b68-121">Ez a funkció lehetővé teszi, hogy a fő előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="05b68-121">This functionality enables the following key benefits:</span></span>

* <span data-ttu-id="05b68-122">Felel meg, hogy új alkalmazások számára elérhető, hogy csak IPv6-alapú ügyfelek igénylő kormányzati szabályzat</span><span class="sxs-lookup"><span data-stu-id="05b68-122">Meet government regulations requiring that new applications be accessible to IPv6-only clients</span></span>
* <span data-ttu-id="05b68-123">Enable mobil és az eszközök internetes hálózata (IOT) fejlesztők kettős halmozott (IPv4 + IPv6) Azure virtuális gépek használata a növekvő mobile & IOT piacok megoldására</span><span class="sxs-lookup"><span data-stu-id="05b68-123">Enable mobile and Internet of things (IOT) developers to use dual-stacked (IPv4+IPv6) Azure Virtual Machines to address the growing mobile & IOT markets</span></span>

## <a name="details-and-limitations"></a><span data-ttu-id="05b68-124">Részletek és korlátozások</span><span class="sxs-lookup"><span data-stu-id="05b68-124">Details and limitations</span></span>

<span data-ttu-id="05b68-125">Részletek</span><span class="sxs-lookup"><span data-stu-id="05b68-125">Details</span></span>

* <span data-ttu-id="05b68-126">Az Azure DNS szolgáltatást tartalmaz, mind az IPv4 és IPv6 AAAA neve rekordok, és válaszol, a terheléselosztóhoz tartozó bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="05b68-126">The Azure DNS service contains both IPv4 A and IPv6 AAAA name records and responds with both records for the load balancer.</span></span> <span data-ttu-id="05b68-127">Az ügyfél úgy dönt, hogy mely címet (IPv4 vagy IPv6) való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="05b68-127">The client chooses which address (IPv4 or IPv6) to communicate with.</span></span>
* <span data-ttu-id="05b68-128">Ha egy virtuális Gépet egy nyilvános Internet IPv6-csatlakozóval csatlakoztatott eszközök kapcsolatot kezdeményez, a virtuális gép forrás IPv6-cím, hálózati cím lefordított (NAT) a terheléselosztó a nyilvános IPv6-címére.</span><span class="sxs-lookup"><span data-stu-id="05b68-128">When a VM initiates a connection to a public Internet IPv6-connected device, the VM's source IPv6 address is network address translated (NAT) to the public IPv6 address of the load balancer.</span></span>
* <span data-ttu-id="05b68-129">A Linux operációs rendszert futtató virtuális gépek kell állítani a DHCP IPv6-alapú IP-címét.</span><span class="sxs-lookup"><span data-stu-id="05b68-129">VMs running the Linux operating system must be configured to receive an IPv6 IP address via DHCP.</span></span> <span data-ttu-id="05b68-130">A Linux-lemezképek, az Azure katalógusában számos már konfigurált IPv6-alapú módosítás nélkül támogatja.</span><span class="sxs-lookup"><span data-stu-id="05b68-130">Many of the Linux images in the Azure Gallery are already configured to support IPv6 without modification.</span></span> <span data-ttu-id="05b68-131">További információkért lásd: [DHCPv6 konfigurálása Linux virtuális gépekhez](load-balancer-ipv6-for-linux.md)</span><span class="sxs-lookup"><span data-stu-id="05b68-131">For more information, see [Configuring DHCPv6 for Linux VMs](load-balancer-ipv6-for-linux.md)</span></span>
* <span data-ttu-id="05b68-132">Ha egy állapotmintáihoz használni a terheléselosztóhoz, egy IPv4-alapú mintavétel létrehozása, és az IPv4 és IPv6-végpontok együtt használja azt.</span><span class="sxs-lookup"><span data-stu-id="05b68-132">If you choose to use a health probe with your load balancer, create an IPv4 probe and use it with both the IPv4 and IPv6 endpoints.</span></span> <span data-ttu-id="05b68-133">Ha a virtuális gépre a szolgáltatás leáll, az IPv4 és IPv6-végpontok kiveszik elforgatás.</span><span class="sxs-lookup"><span data-stu-id="05b68-133">If the service on your VM goes down, both the IPv4 and IPv6 endpoints are taken out of rotation.</span></span>

<span data-ttu-id="05b68-134">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="05b68-134">Limitations</span></span>

* <span data-ttu-id="05b68-135">Nem adható hozzá az IPv6 terheléselosztási szabályok az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="05b68-135">You cannot add IPv6 load balancing rules in the Azure portal.</span></span> <span data-ttu-id="05b68-136">A szabályok csak a sablon, CLI-t, a PowerShell segítségével hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="05b68-136">The rules can only be created through the template, CLI, PowerShell.</span></span>
* <span data-ttu-id="05b68-137">Előfordulhat, hogy nem frissít meglévő virtuális gépek IPv6-címek használatát.</span><span class="sxs-lookup"><span data-stu-id="05b68-137">You may not upgrade existing VMs to use IPv6 addresses.</span></span> <span data-ttu-id="05b68-138">Új virtuális gépeket kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="05b68-138">You must deploy new VMs.</span></span>
* <span data-ttu-id="05b68-139">Minden virtuális gép egyetlen hálózati adapter egy IPv6-cím is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="05b68-139">A single IPv6 address can be assigned to a single network interface in each VM.</span></span>
* <span data-ttu-id="05b68-140">A nyilvános IPv6-címek egy virtuális Gépet nem lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="05b68-140">The public IPv6 addresses cannot be assigned to a VM.</span></span> <span data-ttu-id="05b68-141">Is csak rendelni egy terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="05b68-141">They can only be assigned to a load balancer.</span></span>
* <span data-ttu-id="05b68-142">A DNS-címkeresés a nyilvános IPv6-címek nem konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="05b68-142">You cannot configure the reverse DNS lookup for your public IPv6 addresses.</span></span>
* <span data-ttu-id="05b68-143">Az IPv6-címeket a virtuális gépek nem lehetnek tagjai az Azure-Felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="05b68-143">The VMs with the IPv6 addresses cannot be members of an Azure Cloud Service.</span></span> <span data-ttu-id="05b68-144">Akkor lehet csatlakoztatni az Azure Virtual Network (VNet), és az IPv4-címét keresztül kommunikálnak egymással.</span><span class="sxs-lookup"><span data-stu-id="05b68-144">They can be connected to an Azure Virtual Network (VNet) and communicate with each other over their IPv4 addresses.</span></span>
* <span data-ttu-id="05b68-145">Saját IPv6-címek egy erőforráscsoportot az egyes virtuális gépek telepíthetők, de nem telepíthető az egy erőforráscsoport keresztül méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="05b68-145">Private IPv6 addresses can be deployed on individual VMs in a resource group but cannot be deployed into a resource group via Scale Sets.</span></span>
* <span data-ttu-id="05b68-146">Azure virtuális gépek más virtuális gépek, más Azure-szolgáltatások vagy a helyszíni eszközök IPv6-kapcsolaton keresztül nem tud csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="05b68-146">Azure VMs cannot connect over IPv6 to other VMs, other Azure services, or on-premises devices.</span></span> <span data-ttu-id="05b68-147">Csak kommunikálhatnak az Azure load balancer IPv6-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="05b68-147">They can only communicate with the Azure load balancer over IPv6.</span></span> <span data-ttu-id="05b68-148">Azonban amelyekkel kommunikálhatnak IPv4 használatával ezek más erőforrások.</span><span class="sxs-lookup"><span data-stu-id="05b68-148">However, they can communicate with these other resources using IPv4.</span></span>
* <span data-ttu-id="05b68-149">Hálózati biztonsági csoport (NSG) a védelem IPv4 protokoll kettős verem (IPv4 + IPv6) telepítések esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="05b68-149">Network Security Group (NSG) protection for IPv4 is supported in dual-stack (IPv4+IPv6) deployments.</span></span> <span data-ttu-id="05b68-150">Az NSG-k nem vonatkoznak az IPv6-végpontot.</span><span class="sxs-lookup"><span data-stu-id="05b68-150">NSGs do not apply to the IPv6 endpoints.</span></span>
* <span data-ttu-id="05b68-151">Az IPv6-végpont a virtuális gép nincs kitéve közvetlenül az interneten.</span><span class="sxs-lookup"><span data-stu-id="05b68-151">The IPv6 endpoint on the VM is not exposed directly to the internet.</span></span> <span data-ttu-id="05b68-152">Egy terheléselosztó mögött van.</span><span class="sxs-lookup"><span data-stu-id="05b68-152">It is behind a load balancer.</span></span> <span data-ttu-id="05b68-153">Csak azokat a portokat a terheléselosztási szabály megadott IPv6-kapcsolaton keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="05b68-153">Only the ports specified in the load balancer rules are accessible over IPv6.</span></span>
* <span data-ttu-id="05b68-154">Az IPv6 az megváltoztatása a IdleTimeout paraméter **jelenleg nem támogatott**.</span><span class="sxs-lookup"><span data-stu-id="05b68-154">Changing the IdleTimeout parameter for IPv6 is **currently not supported**.</span></span> <span data-ttu-id="05b68-155">Az alapértelmezett érték négy perc.</span><span class="sxs-lookup"><span data-stu-id="05b68-155">The default is four minutes.</span></span>
* <span data-ttu-id="05b68-156">Az IPv6 az megváltoztatása a loadDistributionMethod paraméter **jelenleg nem támogatott**.</span><span class="sxs-lookup"><span data-stu-id="05b68-156">Changing the loadDistributionMethod parameter for IPv6 is **currently not supported**.</span></span>
* <span data-ttu-id="05b68-157">IPv6-alapú IP-címek fenntartott (ahol IP = static) vannak **jelenleg nem támogatott**.</span><span class="sxs-lookup"><span data-stu-id="05b68-157">Reserved IPv6 IPs (where IPAllocationMethod = static) are **currently not supported**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05b68-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05b68-158">Next steps</span></span>

<span data-ttu-id="05b68-159">A terheléselosztó IPv6 telepítésének megismerése.</span><span class="sxs-lookup"><span data-stu-id="05b68-159">Learn how to deploy a load balancer with IPv6.</span></span>

* [<span data-ttu-id="05b68-160">IPv6 régiónként rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="05b68-160">Availability of IPv6 by region</span></span>](https://go.microsoft.com/fwlink/?linkid=828357)
* [<span data-ttu-id="05b68-161">Központi telepítése egy terheléselosztó IPv6-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="05b68-161">Deploy a load balancer with IPv6 using a template</span></span>](load-balancer-ipv6-internet-template.md)
* [<span data-ttu-id="05b68-162">A terheléselosztó és az IPv6 az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="05b68-162">Deploy a load balancer with IPv6 using Azure PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
* [<span data-ttu-id="05b68-163">Egy Azure CLI-vel rendelkező IPv6-alapú terheléselosztó telepítése</span><span class="sxs-lookup"><span data-stu-id="05b68-163">Deploy a load balancer with IPv6 using Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
