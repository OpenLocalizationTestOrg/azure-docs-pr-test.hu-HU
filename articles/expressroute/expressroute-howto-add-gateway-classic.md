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
ms.openlocfilehash: 6f37d4d9cba546b5416ab99040f5ef6dae273380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a><span data-ttu-id="80a9a-103">A virtuális hálózati átjáró konfigurálása az ExpressRoute (klasszikus) PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="80a9a-103">Configure a virtual network gateway for ExpressRoute using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="80a9a-104">Resource Manager – PowerShell</span><span class="sxs-lookup"><span data-stu-id="80a9a-104">Resource Manager - PowerShell</span></span>](expressroute-howto-add-gateway-resource-manager.md)
> * [<span data-ttu-id="80a9a-105">Klasszikus - PowerShell</span><span class="sxs-lookup"><span data-stu-id="80a9a-105">Classic - PowerShell</span></span>](expressroute-howto-add-gateway-classic.md)
> * [<span data-ttu-id="80a9a-106">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="80a9a-106">Video - Azure Portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

<span data-ttu-id="80a9a-107">Ez a cikk részletesen ismerteti keresztül hello lépéseket tooadd, méretezze át, és távolítsa el a virtuális hálózathoz (VNet) átjáró egy már meglévő vnet.</span><span class="sxs-lookup"><span data-stu-id="80a9a-107">This article will walk you through hello steps tooadd, resize, and remove a virtual network (VNet) gateway for a pre-existing VNet.</span></span> <span data-ttu-id="80a9a-108">hello ehhez a konfigurációhoz lépésekre kifejezetten a Vnetek hello használatával létrehozott **klasszikus üzembe helyezési modellel** és fogja tárolni egy ExpressRoute-konfigurációja használható.</span><span class="sxs-lookup"><span data-stu-id="80a9a-108">hello steps for this configuration are specifically for VNets that were created using hello **classic deployment model** and that will be be used in an ExpressRoute configuration.</span></span> 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="80a9a-109">**Tudnivalók az Azure üzembe helyezési modelljeiről**</span><span class="sxs-lookup"><span data-stu-id="80a9a-109">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a><span data-ttu-id="80a9a-110">Mielőtt hozzálát</span><span class="sxs-lookup"><span data-stu-id="80a9a-110">Before beginning</span></span>
<span data-ttu-id="80a9a-111">Győződjön meg arról, hogy telepítette-e az ehhez a konfigurációhoz szükséges hello Azure PowerShell-parancsmagok (1.0.2-es vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="80a9a-111">Verify that you have installed hello Azure PowerShell cmdlets needed for this configuration (1.0.2 or later).</span></span> <span data-ttu-id="80a9a-112">Ha még nem telepítette a hello parancsmagok, szüksége lesz a toodo Igen hello konfigurációs lépések megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="80a9a-112">If you haven't installed hello cmdlets, you'll need toodo so before beginning hello configuration steps.</span></span> <span data-ttu-id="80a9a-113">További információ az Azure PowerShell telepítése: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="80a9a-113">For more information about installing Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a><span data-ttu-id="80a9a-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80a9a-114">Next steps</span></span>
<span data-ttu-id="80a9a-115">Hello hálózatok átjáró létrehozása után a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot társíthatja.</span><span class="sxs-lookup"><span data-stu-id="80a9a-115">After you have created hello VNet gateway, you can link your VNet tooan ExpressRoute circuit.</span></span> <span data-ttu-id="80a9a-116">Lásd: [csatolni a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="80a9a-116">See [Link a Virtual Network tooan ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).</span></span>

