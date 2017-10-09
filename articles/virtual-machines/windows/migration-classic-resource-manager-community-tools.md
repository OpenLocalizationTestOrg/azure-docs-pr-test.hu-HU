---
title: "aaaCommunity eszközök – helyezze át a hagyományos erőforrások tooAzure erőforrás-kezelő |} Microsoft Docs"
description: "Ez a cikk katalógusok hello eszközök hello közösségi toohelp által kiadott át IaaS-erőforrásokra klasszikus toohello Azure Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 67d5624a14d12a2d9eb46eb12aef461b706d4589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="community-tools-toomigrate-iaas-resources-from-classic-tooazure-resource-manager"></a><span data-ttu-id="f046c-103">Közösségi toomigrate IaaS-erőforrásokra a klasszikus tooAzure erőforrás-kezelő eszközei</span><span class="sxs-lookup"><span data-stu-id="f046c-103">Community tools toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>
<span data-ttu-id="f046c-104">Ez a cikk katalógusok hello eszközök a klasszikus toohello Azure Resource Manager üzembe helyezési modellben az IaaS-erőforrások áttelepítése a hello közösségi tooassist által biztosított.</span><span class="sxs-lookup"><span data-stu-id="f046c-104">This article catalogs hello tools that have been provided by hello community tooassist with migration of IaaS resources from classic toohello Azure Resource Manager deployment model.</span></span>

> [!NOTE]
> <span data-ttu-id="f046c-105">Ezek az eszközök hivatalosan nem támogatottak az Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="f046c-105">These tools are not officially supported by Microsoft Support.</span></span> <span data-ttu-id="f046c-106">Ezért nyissa meg a Githubon forrása, és elégedett tooaccept PRs javítások vagy további helyzeteket is el.</span><span class="sxs-lookup"><span data-stu-id="f046c-106">Therefore they are open sourced on GitHub and we're happy tooaccept PRs for fixes or additional scenarios.</span></span> <span data-ttu-id="f046c-107">tooreport problémát, hello GitHub problémák funkció használata.</span><span class="sxs-lookup"><span data-stu-id="f046c-107">tooreport an issue, use hello GitHub issues feature.</span></span>
> 
> <span data-ttu-id="f046c-108">Ezekkel az eszközökkel áttelepítése, akkor a klasszikus virtuális gép állásidő.</span><span class="sxs-lookup"><span data-stu-id="f046c-108">Migrating with these tools will cause downtime for your classic Virtual Machine.</span></span> <span data-ttu-id="f046c-109">Ha az áttelepítéshez támogatott platform van szüksége, látogasson el</span><span class="sxs-lookup"><span data-stu-id="f046c-109">If you're looking for platform supported migration, visit</span></span> 
> 
>   * [<span data-ttu-id="f046c-110">Támogatott platformon IaaS-erőforrásokra klasszikus tooAzure erőforrás-kezelő veremből áttelepítése</span><span class="sxs-lookup"><span data-stu-id="f046c-110">Platform supported migration of IaaS resources from Classic tooAzure Resource Manager stack</span></span>](migration-classic-resource-manager-overview.md)
>   * [<span data-ttu-id="f046c-111">Műszaki részletes bemutatója a Platform támogatja az áttelepítést a klasszikus tooAzure erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="f046c-111">Technical Deep Dive on Platform supported migration from Classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md)
>   * [<span data-ttu-id="f046c-112">IaaS-erőforrásokra át klasszikus tooAzure erőforrás-kezelő Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f046c-112">Migrate IaaS resources from Classic tooAzure Resource Manager using Azure PowerShell</span></span>](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a><span data-ttu-id="f046c-113">AsmMetadataParser</span><span class="sxs-lookup"><span data-stu-id="f046c-113">AsmMetadataParser</span></span>
<span data-ttu-id="f046c-114">Ez az erőforrás-kezelő Azure szolgáltatásfelügyelet tooAzure vállalati áttelepítés részeként létrehozott segítő eszközök gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="f046c-114">This is a collection of helper tools created as part of enterprise migrations from Azure Service Management tooAzure Resource Manager.</span></span> <span data-ttu-id="f046c-115">Ez az eszköz lehetővé teszi tooreplicate be egy másik előfizetést, amely vizsgálatához használható, és a termelési előfizetésben hello áttelepítés futtatása előtt problémák megoldása során vas infrastruktúráját.</span><span class="sxs-lookup"><span data-stu-id="f046c-115">This tool allows you tooreplicate your infrastructure into another subscription which can be used for testing migration and iron out any issues before running hello migration on your Production subscription.</span></span>

[<span data-ttu-id="f046c-116">Hivatkozás toohello eszköz dokumentációja</span><span class="sxs-lookup"><span data-stu-id="f046c-116">Link toohello tool documentation</span></span>](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a><span data-ttu-id="f046c-117">migAz</span><span class="sxs-lookup"><span data-stu-id="f046c-117">migAz</span></span>
<span data-ttu-id="f046c-118">migAz egy további lehetőség toomigrate klasszikus IaaS erőforrások tooAzure Resource Manager IaaS-erőforrásokra teljes készletét.</span><span class="sxs-lookup"><span data-stu-id="f046c-118">migAz is an additional option toomigrate a complete set of classic IaaS resources tooAzure Resource Manager IaaS resources.</span></span> <span data-ttu-id="f046c-119">hello áttelepítési következhet be hello azonos előfizetés vagy különböző előfizetések és az előfizetés típusú (pl.: CSP előfizetések).</span><span class="sxs-lookup"><span data-stu-id="f046c-119">hello migration can occur within hello same subscription or between different subscriptions and subscription types (ex: CSP subscriptions).</span></span>

[<span data-ttu-id="f046c-120">Hivatkozás toohello eszköz dokumentációja</span><span class="sxs-lookup"><span data-stu-id="f046c-120">Link toohello tool documentation</span></span>](https://github.com/Azure/migAz)

## <a name="next-steps"></a><span data-ttu-id="f046c-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f046c-121">Next Steps</span></span>

* [<span data-ttu-id="f046c-122">IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="f046c-122">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="f046c-123">Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="f046c-123">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="f046c-124">IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése</span><span class="sxs-lookup"><span data-stu-id="f046c-124">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="f046c-125">PowerShell toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata</span><span class="sxs-lookup"><span data-stu-id="f046c-125">Use PowerShell toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="f046c-126">Parancssori felület toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata</span><span class="sxs-lookup"><span data-stu-id="f046c-126">Use CLI toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="f046c-127">A leggyakoribb áttelepítési hibák áttekintése</span><span class="sxs-lookup"><span data-stu-id="f046c-127">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="f046c-128">Felülvizsgálati hello legtöbb kapcsolatos gyakori kérdések a klasszikus tooAzure erőforrás-kezelő áttelepítése IaaS-erőforrásokra</span><span class="sxs-lookup"><span data-stu-id="f046c-128">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

