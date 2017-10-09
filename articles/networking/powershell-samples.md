---
title: "PowerShell-példák aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-példák"
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/24/2017
ms.author: georgewallace
ms.openlocfilehash: 130a6e755691b46a9549cad5acaa5bde4fe38e95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples-for-networking"></a><span data-ttu-id="7dabe-103">Hálózat az Azure PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="7dabe-103">Azure PowerShell Samples for networking</span></span>

<span data-ttu-id="7dabe-104">hello következő táblázat tartalmazza az Azure PowerShell használatával készített hivatkozások tooscripts.</span><span class="sxs-lookup"><span data-stu-id="7dabe-104">hello following table includes links tooscripts built using Azure PowerShell.</span></span>

| | |
|-|-|
|<span data-ttu-id="7dabe-105">**Azure-erőforrások közötti kapcsolat**</span><span class="sxs-lookup"><span data-stu-id="7dabe-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="7dabe-106">A többrétegű alkalmazások virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="7dabe-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7dabe-107">Előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="7dabe-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="7dabe-108">Forgalmi toohello előtér-alhálózat korlátozott tooHTTP, a forgalom toohello közben háttér-alhálózat korlátozott tooSQL, 1433-as port.</span><span class="sxs-lookup"><span data-stu-id="7dabe-108">Traffic toohello front-end subnet is limited tooHTTP, while traffic toohello back-end subnet is limited tooSQL, port 1433.</span></span> |
| [<span data-ttu-id="7dabe-109">A két partner virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="7dabe-109">Peer two virtual networks</span></span>](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7dabe-110">Hoz létre, és csatlakozik a hello két virtuális hálózat ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="7dabe-110">Creates and connects two virtual networks in hello same region.</span></span> |
| [<span data-ttu-id="7dabe-111">A hálózati virtuális készülék-útvonal forgalmát</span><span class="sxs-lookup"><span data-stu-id="7dabe-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7dabe-112">Hoz létre egy virtuális hálózati előtér- és alhálózatok és virtuális gép, amely képes tooroute forgalom hello két alhálózat között.</span><span class="sxs-lookup"><span data-stu-id="7dabe-112">Creates a virtual network with front-end and back-end subnets and a VM that is able tooroute traffic between hello two subnets.</span></span> |
| [<span data-ttu-id="7dabe-113">Bejövő és kimenő virtuális gép hálózati forgalom szűrésére</span><span class="sxs-lookup"><span data-stu-id="7dabe-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7dabe-114">Előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="7dabe-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="7dabe-115">Bejövő hálózati forgalom toohello előtér-alhálózat, korlátozott tooHTTP és a HTTPS...</span><span class="sxs-lookup"><span data-stu-id="7dabe-115">Inbound network traffic toohello front-end subnet is limited tooHTTP and HTTPS..</span></span> <span data-ttu-id="7dabe-116">Kimenő forgalom toohello Internet hello háttér-alhálózatból nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="7dabe-116">Outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="7dabe-117">**Terheléselosztás és a forgalom iránya betöltése**</span><span class="sxs-lookup"><span data-stu-id="7dabe-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="7dabe-118">Egyenleg forgalom tooVMs a magas rendelkezésre állású betöltése</span><span class="sxs-lookup"><span data-stu-id="7dabe-118">Load balance traffic tooVMs for high availability</span></span>](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7dabe-119">Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7dabe-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="7dabe-120">Terhelésének elosztása több webhely virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="7dabe-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-windows-powershell-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="7dabe-121">Hoz létre két virtuális gépek több IP-konfigurációk, illesztett tooan Azure rendelkezésre állási csoport, az Azure Load Balancer keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="7dabe-121">Creates two VMs with multiple IP configurations, joined tooan Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="7dabe-122">A magas rendelkezésre állásának különféle régiókban közvetlen forgalom</span><span class="sxs-lookup"><span data-stu-id="7dabe-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="7dabe-123">Két app service-csomagokról, a két web apps, a traffic manager-profil és a két traffic manager-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="7dabe-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
