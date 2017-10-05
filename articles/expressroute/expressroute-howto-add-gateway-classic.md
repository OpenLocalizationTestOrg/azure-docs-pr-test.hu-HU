---
title: "A virtuális hálózat átjárót konfigurálhatja a PowerShell használatával ExpressRoute: klasszikus: Azure |} Microsoft Docs"
description: "Klasszikus üzembe helyezés hálózatok átjáró konfigurálásához modellhez tartozó virtuális hálózaton, ExpressRoute-konfigurációhoz a PowerShell használatával."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 85ee0bc1-55be-4760-bfb4-34d9f2c96f30
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: 195a38fa45f1c514a93980e777fb0d8238aa3f3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a><span data-ttu-id="dfcc4-103">A virtuális hálózati átjáró konfigurálása az ExpressRoute (klasszikus) PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="dfcc4-103">Configure a virtual network gateway for ExpressRoute using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dfcc4-104">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="dfcc4-104">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="dfcc4-105">Klasszikus - PowerShell</span><span class="sxs-lookup"><span data-stu-id="dfcc4-105">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="dfcc4-106">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="dfcc4-106">Video - Azure Portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="dfcc4-107">Ez a cikk végigvezeti a lépések hozzáadásához átméretezése, és távolítsa el a virtuális hálózathoz (VNet) átjáró egy már meglévő vnet.</span><span class="sxs-lookup"><span data-stu-id="dfcc4-107">This article will walk you through the steps to add, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="dfcc4-108">Ehhez a konfigurációhoz lépésekre, kifejezetten a Vnetek használatával létrehozott a **klasszikus üzembe helyezési modellel** és fogja tárolni egy ExpressRoute-konfigurációja használható.</span><span class="sxs-lookup"><span data-stu-id="dfcc4-108">The steps for this configuration are specifically for VNets that were created using the **classic deployment model** and that will be be used in an ExpressRoute configuration.</span></span> 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="dfcc4-109">**Tudnivalók az Azure üzembehelyezési modellekről**</span><span class="sxs-lookup"><span data-stu-id="dfcc4-109">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a><span data-ttu-id="dfcc4-110">Mielőtt hozzálát</span><span class="sxs-lookup"><span data-stu-id="dfcc4-110">Before beginning</span></span>
<span data-ttu-id="dfcc4-111">Győződjön meg arról, hogy telepítette-e az ehhez a konfigurációhoz szükség Azure PowerShell-parancsmagok (1.0.2-es vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="dfcc4-111">Verify that you have installed the Azure PowerShell cmdlets needed for this configuration (1.0.2 or later).</span></span> <span data-ttu-id="dfcc4-112">Ha még nem telepítette a parancsmagok, szüksége lesz erre a konfigurációs lépések megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="dfcc4-112">If you haven't installed the cmdlets, you'll need to do so before beginning the configuration steps.</span></span> <span data-ttu-id="dfcc4-113">További információ az Azure PowerShell telepítése: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dfcc4-113">For more information about installing Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="dfcc4-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dfcc4-114">Next steps</span></span>
<span data-ttu-id="dfcc4-115">Miután létrehozta a virtuális hálózat átjáró, a virtuális hálózat hozzákapcsolhatja egy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="dfcc4-115">After you have created the VNet gateway, you can link your VNet to an ExpressRoute circuit.</span></span> <span data-ttu-id="dfcc4-116">Lásd: [virtuális hálózat csatolása ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="dfcc4-116">See [Link a Virtual Network to an ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).</span></span>

