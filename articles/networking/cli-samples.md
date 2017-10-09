---
title: "parancssori felület minták aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 8001b7e72480cfd0122325f7fb81c32aaad072d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-networking"></a><span data-ttu-id="d2719-103">Hálózati Azure CLI-minták</span><span class="sxs-lookup"><span data-stu-id="d2719-103">Azure CLI Samples for networking</span></span>

<span data-ttu-id="d2719-104">hello alábbi táblázat tartalmaz hivatkozásokat toobash parancsfájlokat hello Azure parancssori felület használatával készített.</span><span class="sxs-lookup"><span data-stu-id="d2719-104">hello following table includes links toobash scripts built using hello Azure CLI.</span></span>

| | |
|-|-|
|<span data-ttu-id="d2719-105">**Azure-erőforrások közötti kapcsolat**</span><span class="sxs-lookup"><span data-stu-id="d2719-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="d2719-106">A többrétegű alkalmazások virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2719-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="d2719-107">Előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="d2719-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="d2719-108">Forgalmi toohello előtér-alhálózat korlátozott tooHTTP és SSH, a forgalom toohello közben háttér-alhálózat korlátozott tooMySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="d2719-108">Traffic toohello front-end subnet is limited tooHTTP and SSH, while traffic toohello back-end subnet is limited tooMySQL, port 3306.</span></span> |
| [<span data-ttu-id="d2719-109">A két partner virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="d2719-109">Peer two virtual networks</span></span>](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="d2719-110">Hoz létre, és csatlakozik a hello két virtuális hálózat ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="d2719-110">Creates and connects two virtual networks in hello same region.</span></span> |
| [<span data-ttu-id="d2719-111">A hálózati virtuális készülék-útvonal forgalmát</span><span class="sxs-lookup"><span data-stu-id="d2719-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="d2719-112">Hoz létre egy virtuális hálózati előtér- és alhálózatok és virtuális gép, amely képes tooroute forgalom hello két alhálózat között.</span><span class="sxs-lookup"><span data-stu-id="d2719-112">Creates a virtual network with front-end and back-end subnets and a VM that is able tooroute traffic between hello two subnets.</span></span> |
| [<span data-ttu-id="d2719-113">Bejövő és kimenő virtuális gép hálózati forgalom szűrésére</span><span class="sxs-lookup"><span data-stu-id="d2719-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="d2719-114">Előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="d2719-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="d2719-115">Bejövő hálózati forgalom toohello előtér-alhálózat korlátozva tooHTTP, HTTPS és SSH.</span><span class="sxs-lookup"><span data-stu-id="d2719-115">Inbound network traffic toohello front-end subnet is limited tooHTTP, HTTPS and SSH.</span></span> <span data-ttu-id="d2719-116">Kimenő forgalom toohello Internet hello háttér-alhálózatból nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="d2719-116">Outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="d2719-117">**Terheléselosztás és a forgalom iránya betöltése**</span><span class="sxs-lookup"><span data-stu-id="d2719-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="d2719-118">Egyenleg forgalom tooVMs a magas rendelkezésre állású betöltése</span><span class="sxs-lookup"><span data-stu-id="d2719-118">Load balance traffic tooVMs for high availability</span></span>](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="d2719-119">Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d2719-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="d2719-120">Terhelésének elosztása több webhely virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="d2719-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="d2719-121">Hoz létre két virtuális gépek több IP-konfigurációk, illesztett tooan Azure rendelkezésre állási csoport, az Azure Load Balancer keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="d2719-121">Creates two VMs with multiple IP configurations, joined tooan Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="d2719-122">A magas rendelkezésre állásának különféle régiókban közvetlen forgalom</span><span class="sxs-lookup"><span data-stu-id="d2719-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="d2719-123">Két app service-csomagokról, a két web apps, a traffic manager-profil és a két traffic manager-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2719-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
