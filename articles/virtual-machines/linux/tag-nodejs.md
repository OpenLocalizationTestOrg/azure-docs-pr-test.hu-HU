---
title: "egy Azure Linux virtuális gép aaaHow tootag |} Microsoft Docs"
description: "További információk a címkézés egy Azure Linux virtuális gép létrehozása az Azure-ban hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-linux
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: 0e99ea66a87b7e00eb21a2f72dd2bce8673778dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="e72b5-103">Hogyan tootag egy Linux virtuális gép az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e72b5-103">How tootag a Linux virtual machine in Azure</span></span>
<span data-ttu-id="e72b5-104">Ez a cikk ismerteti a különböző módokon tootag Linux virtuális gépek Azure-ban keresztül hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="e72b5-104">This article describes different ways tootag a Linux virtual machine in Azure through hello Resource Manager deployment model.</span></span> <span data-ttu-id="e72b5-105">Címke található a felhasználó által definiált kulcs/érték párok, amely lehet tenni közvetlenül egy erőforrás vagy egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="e72b5-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="e72b5-106">Azure jelenleg legfeljebb too15 címkék erőforrás pedig erőforráscsoportban támogat.</span><span class="sxs-lookup"><span data-stu-id="e72b5-106">Azure currently supports up too15 tags per resource and resource group.</span></span> <span data-ttu-id="e72b5-107">Címke lehet, hogy helyezni egy erőforrás létrehozása a hello időpontjában, vagy hozzá tooan meglévő erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e72b5-107">Tags may be placed on a resource at hello time of creation or added tooan existing resource.</span></span> <span data-ttu-id="e72b5-108">Ne feledje, az erőforrások hello Resource Manager üzembe helyezési modellben csak létrehozott címkék használhatók.</span><span class="sxs-lookup"><span data-stu-id="e72b5-108">Please note, tags are supported for resources created via hello Resource Manager deployment model only.</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a><span data-ttu-id="e72b5-109">Az Azure CLI-címkézés</span><span class="sxs-lookup"><span data-stu-id="e72b5-109">Tagging with Azure CLI</span></span>
<span data-ttu-id="e72b5-110">toobegin, [telepítése és konfigurálása az Azure parancssori felület hello](../../xplat-cli-azure-resource-manager.md) , és győződjön meg arról, hogy az erőforrás-kezelő módban (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="e72b5-110">toobegin, [install and configure hello Azure CLI](../../xplat-cli-azure-resource-manager.md) and make sure you are in Resource Manager mode (`azure config mode arm`).</span></span>

<span data-ttu-id="e72b5-111">Az összes tulajdonság egy adott virtuális gép, beleértve a hello címkék, ezzel a paranccsal tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="e72b5-111">You can view all properties for a given Virtual Machine, including hello tags, using this command:</span></span>

        azure vm show -g MyResourceGroup -n MyTestVM

<span data-ttu-id="e72b5-112">tooadd hello Azure CLI segítségével új virtuális gép címke, hello használható `azure vm set` parancs együtt hello tag paraméter **-t**:</span><span class="sxs-lookup"><span data-stu-id="e72b5-112">tooadd a new VM tag through hello Azure CLI, you can use hello `azure vm set` command along with hello tag parameter **-t**:</span></span>

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

<span data-ttu-id="e72b5-113">minden tooremove címkéket, használhatja a hello **-T** hello paraméterének `azure vm set` parancs.</span><span class="sxs-lookup"><span data-stu-id="e72b5-113">tooremove all tags, you can use hello **–T** parameter in hello `azure vm set` command.</span></span>

        azure vm set – g MyResourceGroup –n MyTestVM -T


<span data-ttu-id="e72b5-114">Most, hogy azt címkék tooour erőforrások az Azure parancssori felület alkalmazott, és a portál hello, most vessen egy pillantást hello használati részletek toosee hello címkék hello számlázási portálon.</span><span class="sxs-lookup"><span data-stu-id="e72b5-114">Now that we have applied tags tooour resources Azure CLI and hello Portal, let’s take a look at hello usage details toosee hello tags in hello billing portal.</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="e72b5-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e72b5-115">Next steps</span></span>
* <span data-ttu-id="e72b5-116">toolearn az Azure-erőforrások, címkézéssel kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése] [ Azure Resource Manager Overview] és [tooorganize címkék használata az Azure-erőforrások] [ Using Tags tooorganize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="e72b5-116">toolearn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags tooorganize your Azure Resources][Using Tags tooorganize your Azure Resources].</span></span>
* <span data-ttu-id="e72b5-117">toosee hogyan címkék segítségével kezelése az Azure-erőforrások, lásd: [ismertetése a Azure számlázási] [ Understanding your Azure Bill] és [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás] [Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="e72b5-117">toosee how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
