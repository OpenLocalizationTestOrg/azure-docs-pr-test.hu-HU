---
title: "Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus Azure Resource Manager |} Microsoft Docs"
description: "Ez a cikk does egy műszaki részletes bemutatója a platform által támogatott áttelepítési erőforrások a klasszikus Azure Resource Managerbe"
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
ms.openlocfilehash: 4898998fe27c48bab4dee3dbaed5a174e23dc83a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="technical-deep-dive-on-platform-supported-migration-from-classic-to-azure-resource-manager"></a><span data-ttu-id="56b34-103">Részletes technikai útmutató a klasszikusból az Azure Resource Manager-alapú üzemi modellbe történő, platform által támogatott migrálásról</span><span class="sxs-lookup"><span data-stu-id="56b34-103">Technical deep dive on platform-supported migration from classic to Azure Resource Manager</span></span>
<span data-ttu-id="56b34-104">A következőkben a részletesen az Azure klasszikus telepítési modellből az Azure Resource Manager telepítési modell való áttelepítéssel.</span><span class="sxs-lookup"><span data-stu-id="56b34-104">Let's take a deep-dive on migrating from the Azure classic deployment model to the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="56b34-105">Úgy tekintünk, amelyekkel jobban megértheti, hogyan az Azure platformon áttelepíti a két üzembe helyezési modellek közötti erőforrások erőforrás és a szolgáltatás szintű erőforrások.</span><span class="sxs-lookup"><span data-stu-id="56b34-105">We look at resources at a resource and feature level to help you understand how the Azure platform migrates resources between the two deployment models.</span></span> <span data-ttu-id="56b34-106">További információkért olvassa el a szolgáltatás közlemény cikk: [Platform által támogatott áttelepítési IaaS erőforrások a klasszikus Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="56b34-106">For more information, please read the service announcement article: [Platform-supported migration of IaaS resources from classic to Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-migration-deep-dive](../../../includes/virtual-machines-common-classic-resource-manager-migration-deep-dive.md)]

## <a name="next-steps"></a><span data-ttu-id="56b34-107">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="56b34-107">Next steps</span></span>

* [<span data-ttu-id="56b34-108">IaaS-erőforrásokra a klasszikus Azure Resource Manager platform által támogatott áttelepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="56b34-108">Overview of platform-supported migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="56b34-109">Az IaaS-erőforrások klasszikusból Azure Resource Manager-alapú környezetbe való áttelepítésének megtervezése</span><span class="sxs-lookup"><span data-stu-id="56b34-109">Planning for migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="56b34-110">IaaS-erőforrások áttelepítése a klasszikus Azure Resource Manager PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="56b34-110">Use PowerShell to migrate IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="56b34-111">IaaS-erőforrások áttelepítése a klasszikus Azure Resource Manager parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="56b34-111">Use CLI to migrate IaaS resources from classic to Azure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="56b34-112">IaaS-erőforrásokra a klasszikus Azure Resource Manager áttelepítésének védelmével kapcsolatos közösségi eszközök</span><span class="sxs-lookup"><span data-stu-id="56b34-112">Community tools for assisting with migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="56b34-113">A leggyakoribb áttelepítési hibák áttekintése</span><span class="sxs-lookup"><span data-stu-id="56b34-113">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="56b34-114">A leggyakrabban feltett kérdésekre áttelepítése IaaS-erőforrásokra a klasszikus Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="56b34-114">Review the most frequently asked questions about migrating IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
