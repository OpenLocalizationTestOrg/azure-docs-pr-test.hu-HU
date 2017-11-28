---
title: "Terheléselosztó támogatása az Azure Resource Manager |} Microsoft Docs"
description: "Az Azure Resource Manager terheléselosztóhoz powershell használatával. Terheléselosztó sablonok használata"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: d0394f11-ee5a-4407-9d86-79c936297265
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: d06c924f384a2684b5a91c202039c581796c1091
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a><span data-ttu-id="9334b-104">Az Azure erőforrás-kezelő támogatási használata az Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="9334b-104">Using Azure Resource Manager Support with Azure Load Balancer</span></span>

<span data-ttu-id="9334b-105">Az Azure Resource Manager az előnyben részesített felügyeleti keretet biztosít az Azure-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="9334b-105">Azure Resource Manager is the preferred management framework for services in Azure.</span></span> <span data-ttu-id="9334b-106">Az Azure Load Balancer Azure Resource Manager-alapú API-k és eszközök használatával kezelhetők.</span><span class="sxs-lookup"><span data-stu-id="9334b-106">Azure Load Balancer can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="concepts"></a><span data-ttu-id="9334b-107">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="9334b-107">Concepts</span></span>

<span data-ttu-id="9334b-108">A Resource Manager Azure terheléselosztó a következő gyermekerőforrásait tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="9334b-108">With Resource Manager, Azure Load Balancer contains the following child resources:</span></span>

* <span data-ttu-id="9334b-109">Terheléselosztó előtér-IP-konfiguráció – tartalmazhat egy vagy több előtérbeli IP-címek, más néven a virtuális IP-címek (VIP).</span><span class="sxs-lookup"><span data-stu-id="9334b-109">Front-end IP configuration – a Load balancer can include one or more front end IP addresses, otherwise known as a virtual IPs (VIPs).</span></span> <span data-ttu-id="9334b-110">Ezek az IP-címek szolgálnak a forgalom bemeneti pontjaként.</span><span class="sxs-lookup"><span data-stu-id="9334b-110">These IP addresses serve as ingress for the traffic.</span></span>
* <span data-ttu-id="9334b-111">Háttér-címkészlet – ezek azok az a virtuális gép hálózati kártya (NIC), amelyhez terhelés eloszlik a társított IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="9334b-111">Back-end address pool – these are IP addresses associated with the virtual machine Network Interface Card (NIC) to which load will be distributed.</span></span>
* <span data-ttu-id="9334b-112">Terheléselosztási szabályok – egy szabály tulajdonság leképezi a megadott előtér-IP és port kombinációján a háttérbeli IP-címek egy készletének – port kombináció.</span><span class="sxs-lookup"><span data-stu-id="9334b-112">Load balancing rules – a rule property maps a given front end IP and port combination to a set of back end IP addresses and port combination.</span></span> <span data-ttu-id="9334b-113">Egyetlen terheléselosztó több terheléselosztási szabállyal is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="9334b-113">A single load balancer can have multiple load balancing rules.</span></span> <span data-ttu-id="9334b-114">Minden szabály egy virtuális gépekhez társított előtérbeli IP és port, illetve háttérbeli IP és port kombinációja.</span><span class="sxs-lookup"><span data-stu-id="9334b-114">Each rule is a combination of a front-end IP and port and back-end IP and port associated with VMs.</span></span>
* <span data-ttu-id="9334b-115">Mintavételt – a mintavételt lehetővé teszik a Virtuálisgép-példányok állapotának nyomon követéséhez.</span><span class="sxs-lookup"><span data-stu-id="9334b-115">Probes – probes enable you to keep track of the health of VM instances.</span></span> <span data-ttu-id="9334b-116">Ha egy állapotmintáihoz nem sikerül, a Virtuálisgép-példány lesz Elforgatás kívül automatikusan.</span><span class="sxs-lookup"><span data-stu-id="9334b-116">If a health probe fails, the VM instance will be taken out of rotation automatically.</span></span>
* <span data-ttu-id="9334b-117">Bejövő NAT-szabályok – NAT-szabályok meghatározása a bejövő forgalom haladnak keresztül az első IP befejezését, majd az háttérbeli IP-cím kiosztva.</span><span class="sxs-lookup"><span data-stu-id="9334b-117">Inbound NAT rules – NAT rules defining the inbound traffic flowing through the front end IP and distributed to the back end IP.</span></span>

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a><span data-ttu-id="9334b-118">Gyorsindítási sablonok</span><span class="sxs-lookup"><span data-stu-id="9334b-118">Quickstart templates</span></span>

<span data-ttu-id="9334b-119">Az Azure Resource Manager lehetővé teszi, hogy alkalmazásait egy deklaratív sablon használatával helyezze üzembe.</span><span class="sxs-lookup"><span data-stu-id="9334b-119">Azure Resource Manager allows you to provision your applications using a declarative template.</span></span> <span data-ttu-id="9334b-120">Egyetlen sablonnal több szolgáltatást is üzembe helyezhet azok függőségeivel együtt.</span><span class="sxs-lookup"><span data-stu-id="9334b-120">In a single template, you can deploy multiple services along with their dependencies.</span></span> <span data-ttu-id="9334b-121">Ugyanazt a sablont újra és újra, az alkalmazás életciklusának minden fázisában felhasználhatja az alkalmazás üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="9334b-121">You use the same template to repeatedly deploy your application during every stage of the application lifecycle.</span></span>

<span data-ttu-id="9334b-122">Sablonok tartalmazhatnak a virtuális gépek, virtuális hálózatok, rendelkezésre állási csoportok, hálózati adapterek (NIC), Storage-fiókok, Terheléselosztók, hálózati biztonsági csoportok és nyilvános IP-címek definícióit.</span><span class="sxs-lookup"><span data-stu-id="9334b-122">Templates can include definitions for Virtual Machines, Virtual Networks, Availability Sets, Network Interfaces (NICs), Storage Accounts, Load Balancers, Network Security Groups, and Public IPs.</span></span> <span data-ttu-id="9334b-123">A sablonokkal hozhat létre mindent megtalál az összetett alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="9334b-123">With templates you can create everything you need for a complex application.</span></span> <span data-ttu-id="9334b-124">A sablonfájl ellenőrizhetők a verziókövetés és együttműködés tartalomkezelési rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="9334b-124">The template file can be checked into content management system for version control and collaboration.</span></span>

[<span data-ttu-id="9334b-125">További információ a sablonok</span><span class="sxs-lookup"><span data-stu-id="9334b-125">Learn more about templates</span></span>](../azure-resource-manager/resource-manager-template-walkthrough.md)

[<span data-ttu-id="9334b-126">További információ a hálózati erőforrások</span><span class="sxs-lookup"><span data-stu-id="9334b-126">Learn more about Network Resources</span></span>](../virtual-network/resource-groups-networking.md)

<span data-ttu-id="9334b-127">Gyors üzembe helyezés sablonok használata az Azure Load Balancer itt található: egy [GitHub-tárházban](https://github.com/Azure/azure-quickstart-templates) üzemeltető közösségi létrehozott sablonok készlete.</span><span class="sxs-lookup"><span data-stu-id="9334b-127">Quickstart templates using Azure Load Balancer can be found in a [GitHub repository](https://github.com/Azure/azure-quickstart-templates) hosting a set of community generated templates.</span></span>

<span data-ttu-id="9334b-128">Sablonok példái:</span><span class="sxs-lookup"><span data-stu-id="9334b-128">Examples of templates:</span></span>

* [<span data-ttu-id="9334b-129">2 virtuális gép egy terheléselosztó és terheléselosztási szabályok</span><span class="sxs-lookup"><span data-stu-id="9334b-129">2 VMs in a Load Balancer and load balancing rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544799)
* [<span data-ttu-id="9334b-130">2 virtuális gép egy belső terheléselosztó és a terheléselosztó szabályait a VNETEN belül</span><span class="sxs-lookup"><span data-stu-id="9334b-130">2 VMs in a VNET with an Internal Load Balancer and Load Balancer rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544800)
* [<span data-ttu-id="9334b-131">2 virtuális gép egy terheléselosztó és a Terheléselosztó NAT-szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9334b-131">2 VMs in a Load Balancer and configure NAT rules on the LB</span></span>](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a><span data-ttu-id="9334b-132">A PowerShell vagy a CLI Azure Load Balancer beállítása</span><span class="sxs-lookup"><span data-stu-id="9334b-132">Setting up Azure Load Balancer with a PowerShell or CLI</span></span>

<span data-ttu-id="9334b-133">Ismerkedés az Azure Resource Manager parancsmagok, a parancssori eszköz és a REST API-k</span><span class="sxs-lookup"><span data-stu-id="9334b-133">Get started with Azure Resource Manager cmdlets, command line tools, and REST APIs</span></span>

* <span data-ttu-id="9334b-134">[Azure-hálózat parancsmagokat](https://msdn.microsoft.com/library/azure/mt163510.aspx) hozzon létre egy terheléselosztó használható.</span><span class="sxs-lookup"><span data-stu-id="9334b-134">[Azure Networking Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) can be used to create a Load Balancer.</span></span>
* [<span data-ttu-id="9334b-135">Azure Resource Manager használatával terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9334b-135">How to create a load balancer using Azure Resource Manager</span></span>](load-balancer-get-started-ilb-arm-ps.md)
* [<span data-ttu-id="9334b-136">Az Azure erőforrás-kezelés az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="9334b-136">Using the Azure CLI with Azure Resource Management</span></span>](../xplat-cli-azure-resource-manager.md)
* [<span data-ttu-id="9334b-137">Terheléselosztó REST API-k</span><span class="sxs-lookup"><span data-stu-id="9334b-137">Load Balancer REST APIs</span></span>](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a><span data-ttu-id="9334b-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9334b-138">Next steps</span></span>

<span data-ttu-id="9334b-139">Is [Internet felé néző terheléselosztó létrehozásához](load-balancer-get-started-internet-arm-ps.md) és konfigurálása, hogy milyen típusú [mód](load-balancer-distribution-mode.md) egy meghatározott típusú terheléselosztó hálózati forgalom viselkedését.</span><span class="sxs-lookup"><span data-stu-id="9334b-139">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for a specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="9334b-140">Megtudhatja, hogyan kezelheti [üresjárati TCP időtúllépési beállítások terheléselosztó](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="9334b-140">Learn how to manage [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="9334b-141">Ez azért fontos, ha az alkalmazás kell életben tartása kapcsolatok kiszolgálók egy terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="9334b-141">This is important when your application needs to keep connections alive for servers behind a load balancer.</span></span>
