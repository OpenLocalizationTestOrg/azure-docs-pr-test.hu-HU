---
title: "a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő férhet hozzá a részletes aaaTechnical |} Microsoft Docs"
description: "Ez a cikk egy műszaki részletes bemutatója a platform által támogatott áttelepítési erőforrások does a klasszikus tooAzure erőforrás-kezelő"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 1ee40d32-a5e8-42a2-97d0-3232fd3cbb98
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: c7196f9bef6e06c72b5cf85041b674bfb25b7c05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="technical-deep-dive-on-platform-supported-migration-from-classic-tooazure-resource-manager"></a><span data-ttu-id="26eeb-103">Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="26eeb-103">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>
<span data-ttu-id="26eeb-104">Vegyünk egy részletesen a hello Azure klasszikus telepítési modell toohello Azure Resource Manager telepítési modell áttelepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="26eeb-104">Let's take a deep-dive on migrating from hello Azure classic deployment model toohello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="26eeb-105">Azt erőforrásokat érhet el egy erőforrás tekintse meg, és hogyan hello Azure platformon áttelepíti erőforrások közötti hello két üzembe helyezési modellel tisztában szintű toohelp funkció.</span><span class="sxs-lookup"><span data-stu-id="26eeb-105">We look at resources at a resource and feature level toohelp you understand how hello Azure platform migrates resources between hello two deployment models.</span></span> <span data-ttu-id="26eeb-106">További információkért olvassa el az hello szolgáltatás közlemény cikk: [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének Platform által támogatott](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="26eeb-106">For more information, please read hello service announcement article: [Platform-supported migration of IaaS resources from classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-migration-deep-dive](../../../includes/virtual-machines-common-classic-resource-manager-migration-deep-dive.md)]

## <a name="next-steps"></a><span data-ttu-id="26eeb-107">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26eeb-107">Next steps</span></span>

* [<span data-ttu-id="26eeb-108">IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="26eeb-108">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="26eeb-109">IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése</span><span class="sxs-lookup"><span data-stu-id="26eeb-109">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="26eeb-110">PowerShell toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata</span><span class="sxs-lookup"><span data-stu-id="26eeb-110">Use PowerShell toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="26eeb-111">Parancssori felület toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata</span><span class="sxs-lookup"><span data-stu-id="26eeb-111">Use CLI toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="26eeb-112">IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének védelmével kapcsolatos közösségi eszközök</span><span class="sxs-lookup"><span data-stu-id="26eeb-112">Community tools for assisting with migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="26eeb-113">A leggyakoribb áttelepítési hibák áttekintése</span><span class="sxs-lookup"><span data-stu-id="26eeb-113">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="26eeb-114">Felülvizsgálati hello legtöbb kapcsolatos gyakori kérdések a klasszikus tooAzure erőforrás-kezelő áttelepítése IaaS-erőforrásokra</span><span class="sxs-lookup"><span data-stu-id="26eeb-114">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
