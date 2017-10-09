---
title: "Vegyen fel egy virtuális hálózati átjáró tooa VNet ExpressRoute: PowerShell: Azure |} Microsoft Docs"
description: "Ez a cikk végigvezeti egy már létrehozott ExpressRoute az erőforrás-kezelő VNet hálózatok átjáró tooan hozzáadása."
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
ms.openlocfilehash: 8983430b426ad7c4af766294fa16427c5e9df5c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a><span data-ttu-id="2ed31-103">Az ExpressRoute virtuális hálózati átjáróinak konfigurálása a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="2ed31-103">Configure a virtual network gateway for ExpressRoute using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2ed31-104">Resource Manager – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2ed31-104">Resource Manager - Azure portal</span></span>](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [<span data-ttu-id="2ed31-105">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ed31-105">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="2ed31-106">Klasszikus - PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ed31-106">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="2ed31-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2ed31-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="2ed31-108">Ez a cikk bemutatja, hogyan hello lépéseket tooadd, méretezze át, és távolítsa el a virtuális hálózathoz (VNet) átjáró egy már meglévő vnet.</span><span class="sxs-lookup"><span data-stu-id="2ed31-108">This article walks you through hello steps tooadd, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="2ed31-109">hello ehhez a konfigurációhoz lépésekre kifejezetten a Vnetek hello Resource Manager telepítési modell ExpressRoute konfigurációban használt használatával létrehozott.</span><span class="sxs-lookup"><span data-stu-id="2ed31-109">hello steps for this configuration are specifically for VNets that were created using hello Resource Manager deployment model that will be used in an ExpressRoute configuration.</span></span> <span data-ttu-id="2ed31-110">További információ a virtuális hálózati átjárók és az átjáró konfigurációs beállításainak ExpressRoute: [kapcsolatos az ExpressRoute virtuális hálózati átjárók](expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="2ed31-110">For more information about virtual network gateways and gateway configuration settings for ExpressRoute, see [About virtual network gateways for ExpressRoute](expressroute-about-virtual-network-gateways.md).</span></span> 


## <a name="before-beginning"></a><span data-ttu-id="2ed31-111">Mielőtt hozzálát</span><span class="sxs-lookup"><span data-stu-id="2ed31-111">Before beginning</span></span>
<span data-ttu-id="2ed31-112">Győződjön meg arról, hogy telepítette-e az hello legújabb Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="2ed31-112">Verify that you have installed hello latest Azure PowerShell cmdlets.</span></span> <span data-ttu-id="2ed31-113">Ha még nem telepítette a hello legújabb parancsmagok, szükség van-e toodo Igen hello konfigurációs lépések megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="2ed31-113">If you haven't installed hello latest cmdlets, you need toodo so before beginning hello configuration steps.</span></span> <span data-ttu-id="2ed31-114">További információk: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2ed31-114">For more information, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="2ed31-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2ed31-115">Next steps</span></span>
<span data-ttu-id="2ed31-116">Hello hálózatok átjáró létrehozása után a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot társíthatja.</span><span class="sxs-lookup"><span data-stu-id="2ed31-116">After you have created hello VNet gateway, you can link your VNet tooan ExpressRoute circuit.</span></span> <span data-ttu-id="2ed31-117">Lásd: [csatolni a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="2ed31-117">See [Link a Virtual Network tooan ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

