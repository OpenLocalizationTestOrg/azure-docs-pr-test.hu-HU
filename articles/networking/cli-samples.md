---
title: "Az Azure CLI minták |} Microsoft Docs"
description: "Az Azure CLI-minták"
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: kumud
ms.openlocfilehash: 7977460f61bfdabd399e45e86d9bbf2e5083992b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-samples-for-networking"></a><span data-ttu-id="c22cc-103">Hálózati Azure CLI-minták</span><span class="sxs-lookup"><span data-stu-id="c22cc-103">Azure CLI Samples for networking</span></span>

<span data-ttu-id="c22cc-104">A következő táblázat a bash parancsfájlok az Azure parancssori felület használatával készített mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c22cc-104">The following table includes links to bash scripts built using the Azure CLI.</span></span>

| | |
|-|-|
|<span data-ttu-id="c22cc-105">**Azure-erőforrások közötti kapcsolat**</span><span class="sxs-lookup"><span data-stu-id="c22cc-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="c22cc-106">A többrétegű alkalmazások virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c22cc-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c22cc-107">Előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="c22cc-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="c22cc-108">Az előtér-alhálózat felé irányuló forgalom korlátozódik HTTP és az SSH-közben az adatforgalom a háttér-alhálózathoz MySQL, port 3306 korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="c22cc-108">Traffic to the front-end subnet is limited to HTTP and SSH, while traffic to the back-end subnet is limited to MySQL, port 3306.</span></span> |
| [<span data-ttu-id="c22cc-109">A két partner virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="c22cc-109">Peer two virtual networks</span></span>](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c22cc-110">Hoz létre, és két virtuális hálózat ugyanabban a régióban kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="c22cc-110">Creates and connects two virtual networks in the same region.</span></span> |
| [<span data-ttu-id="c22cc-111">A hálózati virtuális készülék-útvonal forgalmát</span><span class="sxs-lookup"><span data-stu-id="c22cc-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c22cc-112">Hoz létre egy virtuális hálózati előtér- és alhálózatok és virtuális gép, amely képes irányíthatja a forgalmat a két alhálózat között.</span><span class="sxs-lookup"><span data-stu-id="c22cc-112">Creates a virtual network with front-end and back-end subnets and a VM that is able to route traffic between the two subnets.</span></span> |
| [<span data-ttu-id="c22cc-113">Bejövő és kimenő virtuális gép hálózati forgalom szűrésére</span><span class="sxs-lookup"><span data-stu-id="c22cc-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c22cc-114">Előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="c22cc-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="c22cc-115">Az előtér-alhálózat bejövő hálózati forgalmának HTTP, HTTPS és SSH korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="c22cc-115">Inbound network traffic to the front-end subnet is limited to HTTP, HTTPS and SSH.</span></span> <span data-ttu-id="c22cc-116">Kimenő forgalom az internethez, a háttér-alhálózatból nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="c22cc-116">Outbound traffic to the Internet from the back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="c22cc-117">**Terheléselosztás és a forgalom iránya betöltése**</span><span class="sxs-lookup"><span data-stu-id="c22cc-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="c22cc-118">Betöltési oszthatja el a forgalmat a virtuális gépek magas rendelkezésre álláshoz</span><span class="sxs-lookup"><span data-stu-id="c22cc-118">Load balance traffic to VMs for high availability</span></span>](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c22cc-119">Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c22cc-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="c22cc-120">Terhelésének elosztása több webhely virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="c22cc-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c22cc-121">Hoz létre két virtuális gépek több IP-konfigurációk, csatlakozik Azure rendelkezésre állási csoport, az Azure Load Balancer keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="c22cc-121">Creates two VMs with multiple IP configurations, joined to an Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="c22cc-122">A magas rendelkezésre állásának különféle régiókban közvetlen forgalom</span><span class="sxs-lookup"><span data-stu-id="c22cc-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="c22cc-123">Két app service-csomagokról, a két web apps, a traffic manager-profil és a két traffic manager-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="c22cc-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
