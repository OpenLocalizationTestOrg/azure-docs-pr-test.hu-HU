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
ms.openlocfilehash: 456b226af4495c3b446cb79c99cf9494dde9fca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="9b877-103">Hogyan tootag egy Linux virtuális gép az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="9b877-103">How tootag a Linux virtual machine in Azure</span></span>
<span data-ttu-id="9b877-104">Ez a cikk ismerteti a különböző módokon tootag Linux virtuális gépek Azure-ban keresztül hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="9b877-104">This article describes different ways tootag a Linux virtual machine in Azure through hello Resource Manager deployment model.</span></span> <span data-ttu-id="9b877-105">Címke található a felhasználó által definiált kulcs/érték párok, amely lehet tenni közvetlenül egy erőforrás vagy egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9b877-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="9b877-106">Azure jelenleg legfeljebb too15 címkék erőforrás pedig erőforráscsoportban támogat.</span><span class="sxs-lookup"><span data-stu-id="9b877-106">Azure currently supports up too15 tags per resource and resource group.</span></span> <span data-ttu-id="9b877-107">Címke lehet, hogy helyezni egy erőforrás létrehozása a hello időpontjában, vagy hozzá tooan meglévő erőforrást.</span><span class="sxs-lookup"><span data-stu-id="9b877-107">Tags may be placed on a resource at hello time of creation or added tooan existing resource.</span></span> <span data-ttu-id="9b877-108">Ne feledje, az erőforrások hello Resource Manager üzembe helyezési modellben csak létrehozott címkék használhatók.</span><span class="sxs-lookup"><span data-stu-id="9b877-108">Please note, tags are supported for resources created via hello Resource Manager deployment model only.</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a><span data-ttu-id="9b877-109">Az Azure CLI-címkézés</span><span class="sxs-lookup"><span data-stu-id="9b877-109">Tagging with Azure CLI</span></span>
<span data-ttu-id="9b877-110">toobegin, kell hello legújabb [Azure CLI 2.0 (előzetes verzió)](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="9b877-110">toobegin, you need hello latest [Azure CLI 2.0 (Preview)](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="9b877-111">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](tag-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9b877-111">You can also perform these steps with hello [Azure CLI 1.0](tag-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="9b877-112">Az összes tulajdonság egy adott virtuális gép, beleértve a hello címkék, ezzel a paranccsal tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="9b877-112">You can view all properties for a given Virtual Machine, including hello tags, using this command:</span></span>

        az vm show --resource-group MyResourceGroup --name MyTestVM

<span data-ttu-id="9b877-113">tooadd hello Azure CLI segítségével új virtuális gép címke, hello használható `azure vm update` parancs együtt hello tag paraméter **--beállítása**:</span><span class="sxs-lookup"><span data-stu-id="9b877-113">tooadd a new VM tag through hello Azure CLI, you can use hello `azure vm update` command along with hello tag parameter **--set**:</span></span>

        az vm update --resource-group MyResourceGroup --name MyTestVM –-set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2

<span data-ttu-id="9b877-114">tooremove címkék hello használható **--eltávolítása** hello paraméterének `azure vm update` parancsot.</span><span class="sxs-lookup"><span data-stu-id="9b877-114">tooremove tags, you can use hello **--remove** parameter in hello `azure vm update` command.</span></span>

        az vm update –-resource-group MyResourceGroup –-name MyTestVM --remove tags.myNewTagName1


<span data-ttu-id="9b877-115">Most, hogy azt címkék tooour erőforrások az Azure parancssori felület alkalmazott, és a portál hello, most vessen egy pillantást hello használati részletek toosee hello címkék hello számlázási portálon.</span><span class="sxs-lookup"><span data-stu-id="9b877-115">Now that we have applied tags tooour resources Azure CLI and hello Portal, let’s take a look at hello usage details toosee hello tags in hello billing portal.</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="9b877-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b877-116">Next steps</span></span>
* <span data-ttu-id="9b877-117">toolearn az Azure-erőforrások, címkézéssel kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése] [ Azure Resource Manager Overview] és [tooorganize címkék használata az Azure-erőforrások] [ Using Tags tooorganize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="9b877-117">toolearn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags tooorganize your Azure Resources][Using Tags tooorganize your Azure Resources].</span></span>
* <span data-ttu-id="9b877-118">toosee hogyan címkék segítségével kezelése az Azure-erőforrások, lásd: [ismertetése a Azure számlázási] [ Understanding your Azure Bill] és [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás] [Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="9b877-118">toosee how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
