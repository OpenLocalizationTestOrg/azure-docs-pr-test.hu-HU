---
title: "Terheléselosztó Resource Manager támogatása aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 3c02d9382de00fefe6af8f4f8a2b7b62b344f285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a><span data-ttu-id="1cc10-104">Az Azure erőforrás-kezelő támogatási használata az Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="1cc10-104">Using Azure Resource Manager Support with Azure Load Balancer</span></span>

<span data-ttu-id="1cc10-105">Az Azure Resource Manager hello előnyben részesített felügyeleti keretrendszere, amely az Azure.</span><span class="sxs-lookup"><span data-stu-id="1cc10-105">Azure Resource Manager is hello preferred management framework for services in Azure.</span></span> <span data-ttu-id="1cc10-106">Az Azure Load Balancer Azure Resource Manager-alapú API-k és eszközök használatával kezelhetők.</span><span class="sxs-lookup"><span data-stu-id="1cc10-106">Azure Load Balancer can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="concepts"></a><span data-ttu-id="1cc10-107">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="1cc10-107">Concepts</span></span>

<span data-ttu-id="1cc10-108">A Resource Manager Azure Load Balancer tartalmazza a következő gyermekerőforrásait hello:</span><span class="sxs-lookup"><span data-stu-id="1cc10-108">With Resource Manager, Azure Load Balancer contains hello following child resources:</span></span>

* <span data-ttu-id="1cc10-109">Terheléselosztó előtér-IP-konfiguráció – tartalmazhat egy vagy több előtérbeli IP-címek, más néven a virtuális IP-címek (VIP).</span><span class="sxs-lookup"><span data-stu-id="1cc10-109">Front-end IP configuration – a Load balancer can include one or more front end IP addresses, otherwise known as a virtual IPs (VIPs).</span></span> <span data-ttu-id="1cc10-110">A következő IP-címek érkező forgalom hello szolgál.</span><span class="sxs-lookup"><span data-stu-id="1cc10-110">These IP addresses serve as ingress for hello traffic.</span></span>
* <span data-ttu-id="1cc10-111">Háttér-címkészlet – ezek a hello virtuális gép hálózati kártya (NIC) toowhich terhelés eloszlik a társított IP-címek.</span><span class="sxs-lookup"><span data-stu-id="1cc10-111">Back-end address pool – these are IP addresses associated with hello virtual machine Network Interface Card (NIC) toowhich load will be distributed.</span></span>
* <span data-ttu-id="1cc10-112">Terheléselosztási szabályok – a szabály tulajdonság képezi le egy adott előtérbeli IP-cím és port kombinációja tooa set háttérbeli IP-címek és port kombináció.</span><span class="sxs-lookup"><span data-stu-id="1cc10-112">Load balancing rules – a rule property maps a given front end IP and port combination tooa set of back end IP addresses and port combination.</span></span> <span data-ttu-id="1cc10-113">Egyetlen terheléselosztó több terheléselosztási szabállyal is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="1cc10-113">A single load balancer can have multiple load balancing rules.</span></span> <span data-ttu-id="1cc10-114">Minden szabály egy virtuális gépekhez társított előtérbeli IP és port, illetve háttérbeli IP és port kombinációja.</span><span class="sxs-lookup"><span data-stu-id="1cc10-114">Each rule is a combination of a front-end IP and port and back-end IP and port associated with VMs.</span></span>
* <span data-ttu-id="1cc10-115">Mintavételt – mintavételek menüpontban, tookeep nyomon Virtuálisgép-példányok állapotát hello engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1cc10-115">Probes – probes enable you tookeep track of hello health of VM instances.</span></span> <span data-ttu-id="1cc10-116">Ha nem sikerül egy állapotmintáihoz, hello Virtuálisgép-példány megnyílik kívül Elforgatás automatikusan.</span><span class="sxs-lookup"><span data-stu-id="1cc10-116">If a health probe fails, hello VM instance will be taken out of rotation automatically.</span></span>
* <span data-ttu-id="1cc10-117">Bejövő forgalmat kezelő NAT-szabályok – NAT-szabályok meghatározása hello hello front-end IP átfutó bejövő forgalmat, és vissza az elosztott toohello befejező IP.</span><span class="sxs-lookup"><span data-stu-id="1cc10-117">Inbound NAT rules – NAT rules defining hello inbound traffic flowing through hello front end IP and distributed toohello back end IP.</span></span>

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a><span data-ttu-id="1cc10-118">Gyorsindítási sablonok</span><span class="sxs-lookup"><span data-stu-id="1cc10-118">Quickstart templates</span></span>

<span data-ttu-id="1cc10-119">Az Azure Resource Manager lehetővé teszi tooprovision olyan deklaratív sablonok segítségével az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="1cc10-119">Azure Resource Manager allows you tooprovision your applications using a declarative template.</span></span> <span data-ttu-id="1cc10-120">Egyetlen sablonnal több szolgáltatást is üzembe helyezhet azok függőségeivel együtt.</span><span class="sxs-lookup"><span data-stu-id="1cc10-120">In a single template, you can deploy multiple services along with their dependencies.</span></span> <span data-ttu-id="1cc10-121">Hello használata azonos sablon toorepeatedly az alkalmazást, minden szakasza hello alkalmazás-életciklus helyezi üzembe.</span><span class="sxs-lookup"><span data-stu-id="1cc10-121">You use hello same template toorepeatedly deploy your application during every stage of hello application lifecycle.</span></span>

<span data-ttu-id="1cc10-122">Sablonok tartalmazhatnak a virtuális gépek, virtuális hálózatok, rendelkezésre állási csoportok, hálózati adapterek (NIC), Storage-fiókok, Terheléselosztók, hálózati biztonsági csoportok és nyilvános IP-címek definícióit.</span><span class="sxs-lookup"><span data-stu-id="1cc10-122">Templates can include definitions for Virtual Machines, Virtual Networks, Availability Sets, Network Interfaces (NICs), Storage Accounts, Load Balancers, Network Security Groups, and Public IPs.</span></span> <span data-ttu-id="1cc10-123">A sablonokkal hozhat létre mindent megtalál az összetett alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="1cc10-123">With templates you can create everything you need for a complex application.</span></span> <span data-ttu-id="1cc10-124">hello sablonfájl ellenőrizhetők a verziókövetés és együttműködés tartalomkezelési rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="1cc10-124">hello template file can be checked into content management system for version control and collaboration.</span></span>

[<span data-ttu-id="1cc10-125">További információ a sablonok</span><span class="sxs-lookup"><span data-stu-id="1cc10-125">Learn more about templates</span></span>](../azure-resource-manager/resource-manager-template-walkthrough.md)

[<span data-ttu-id="1cc10-126">További információ a hálózati erőforrások</span><span class="sxs-lookup"><span data-stu-id="1cc10-126">Learn more about Network Resources</span></span>](../virtual-network/resource-groups-networking.md)

<span data-ttu-id="1cc10-127">Gyors üzembe helyezés sablonok használata az Azure Load Balancer itt található: egy [GitHub-tárházban](https://github.com/Azure/azure-quickstart-templates) üzemeltető közösségi létrehozott sablonok készlete.</span><span class="sxs-lookup"><span data-stu-id="1cc10-127">Quickstart templates using Azure Load Balancer can be found in a [GitHub repository](https://github.com/Azure/azure-quickstart-templates) hosting a set of community generated templates.</span></span>

<span data-ttu-id="1cc10-128">Sablonok példái:</span><span class="sxs-lookup"><span data-stu-id="1cc10-128">Examples of templates:</span></span>

* [<span data-ttu-id="1cc10-129">2 virtuális gép egy terheléselosztó és terheléselosztási szabályok</span><span class="sxs-lookup"><span data-stu-id="1cc10-129">2 VMs in a Load Balancer and load balancing rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544799)
* [<span data-ttu-id="1cc10-130">2 virtuális gép egy belső terheléselosztó és a terheléselosztó szabályait a VNETEN belül</span><span class="sxs-lookup"><span data-stu-id="1cc10-130">2 VMs in a VNET with an Internal Load Balancer and Load Balancer rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544800)
* [<span data-ttu-id="1cc10-131">2 virtuális gép egy terheléselosztó és hello Terheléselosztó NAT-szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1cc10-131">2 VMs in a Load Balancer and configure NAT rules on hello LB</span></span>](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a><span data-ttu-id="1cc10-132">A PowerShell vagy a CLI Azure Load Balancer beállítása</span><span class="sxs-lookup"><span data-stu-id="1cc10-132">Setting up Azure Load Balancer with a PowerShell or CLI</span></span>

<span data-ttu-id="1cc10-133">Ismerkedés az Azure Resource Manager parancsmagok, a parancssori eszköz és a REST API-k</span><span class="sxs-lookup"><span data-stu-id="1cc10-133">Get started with Azure Resource Manager cmdlets, command line tools, and REST APIs</span></span>

* <span data-ttu-id="1cc10-134">[Azure-hálózat parancsmagokat](https://msdn.microsoft.com/library/azure/mt163510.aspx) lehet használt toocreate terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="1cc10-134">[Azure Networking Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) can be used toocreate a Load Balancer.</span></span>
* [<span data-ttu-id="1cc10-135">Hogyan toocreate egy terhelés-kiegyenlítő Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="1cc10-135">How toocreate a load balancer using Azure Resource Manager</span></span>](load-balancer-get-started-ilb-arm-ps.md)
* [<span data-ttu-id="1cc10-136">Hello Azure CLI használata az Azure erőforrás-kezelés</span><span class="sxs-lookup"><span data-stu-id="1cc10-136">Using hello Azure CLI with Azure Resource Management</span></span>](../xplat-cli-azure-resource-manager.md)
* [<span data-ttu-id="1cc10-137">Terheléselosztó REST API-k</span><span class="sxs-lookup"><span data-stu-id="1cc10-137">Load Balancer REST APIs</span></span>](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a><span data-ttu-id="1cc10-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1cc10-138">Next steps</span></span>

<span data-ttu-id="1cc10-139">Is [Internet felé néző terheléselosztó létrehozásához](load-balancer-get-started-internet-arm-ps.md) és konfigurálása, hogy milyen típusú [mód](load-balancer-distribution-mode.md) egy meghatározott típusú terheléselosztó hálózati forgalom viselkedését.</span><span class="sxs-lookup"><span data-stu-id="1cc10-139">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for a specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="1cc10-140">Megtudhatja, hogyan toomanage [üresjárati TCP időtúllépési beállítások terheléselosztó](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="1cc10-140">Learn how toomanage [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="1cc10-141">Ez akkor fontos, ha az alkalmazás igényeinek tookeep kapcsolatok életben kiszolgálók egy terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="1cc10-141">This is important when your application needs tookeep connections alive for servers behind a load balancer.</span></span>
