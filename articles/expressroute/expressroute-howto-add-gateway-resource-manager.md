---
title: "Vegyen fel egy virtuális hálózat virtuális hálózati átjáró ExpressRoute: PowerShell: Azure |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan ExpressRoute egy már létrehozott erőforrás-kezelő virtuális hálózatot a virtuális hálózat átjárót ad hozzá."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: charwen
ms.openlocfilehash: 3aeddd03e0be548933775164ae790ba208fc13ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a><span data-ttu-id="9b7dc-103">Az ExpressRoute virtuális hálózati átjáróinak konfigurálása a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="9b7dc-103">Configure a virtual network gateway for ExpressRoute using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9b7dc-104">Resource Manager – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9b7dc-104">Resource Manager - Azure portal</span></span>](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [<span data-ttu-id="9b7dc-105">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b7dc-105">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="9b7dc-106">Klasszikus - PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b7dc-106">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="9b7dc-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9b7dc-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="9b7dc-108">Ez a cikk végigvezeti a lépéseken hozzáadásához átméretezése, és távolítsa el a virtuális hálózathoz (VNet) átjáró egy már meglévő vnet.</span><span class="sxs-lookup"><span data-stu-id="9b7dc-108">This article walks you through the steps to add, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="9b7dc-109">Ehhez a konfigurációhoz lépésekre, kifejezetten a Vnetek létrehozott erőforrás-kezelő telepítési modellel, amely egy ExpressRoute-konfigurációt fogja használni.</span><span class="sxs-lookup"><span data-stu-id="9b7dc-109">The steps for this configuration are specifically for VNets that were created using the Resource Manager deployment model that will be used in an ExpressRoute configuration.</span></span> <span data-ttu-id="9b7dc-110">További információ a virtuális hálózati átjárók és az átjáró konfigurációs beállításainak ExpressRoute: [kapcsolatos az ExpressRoute virtuális hálózati átjárók](expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="9b7dc-110">For more information about virtual network gateways and gateway configuration settings for ExpressRoute, see [About virtual network gateways for ExpressRoute](expressroute-about-virtual-network-gateways.md).</span></span> 


## <a name="before-beginning"></a><span data-ttu-id="9b7dc-111">Mielőtt hozzálát</span><span class="sxs-lookup"><span data-stu-id="9b7dc-111">Before beginning</span></span>
<span data-ttu-id="9b7dc-112">Győződjön meg arról, hogy telepítette-e a legújabb Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="9b7dc-112">Verify that you have installed the latest Azure PowerShell cmdlets.</span></span> <span data-ttu-id="9b7dc-113">Ha még nem telepítette a legújabb parancsmagok, ehhez a konfigurációs lépések megkezdése előtt szüksége.</span><span class="sxs-lookup"><span data-stu-id="9b7dc-113">If you haven't installed the latest cmdlets, you need to do so before beginning the configuration steps.</span></span> <span data-ttu-id="9b7dc-114">További információk: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9b7dc-114">For more information, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="9b7dc-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b7dc-115">Next steps</span></span>
<span data-ttu-id="9b7dc-116">Miután létrehozta a virtuális hálózat átjáró, a virtuális hálózat hozzákapcsolhatja egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="9b7dc-116">After you have created the VNet gateway, you can link your VNet to an ExpressRoute circuit.</span></span> <span data-ttu-id="9b7dc-117">Lásd: [virtuális hálózat csatolása ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="9b7dc-117">See [Link a Virtual Network to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

