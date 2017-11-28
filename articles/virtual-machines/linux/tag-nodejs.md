---
title: "Hogyan címkét egy Azure Linux virtuális gép |} Microsoft Docs"
description: "További információk a címkézés egy Azure Linux virtuális gép létrehozása az Azure-ban a Resource Manager üzembe helyezési modellben."
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
ms.openlocfilehash: f643001c85e127ae39e9869ffdc689bcac232ccb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="713ff-103">Hogyan Linux virtuális gépek címke az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="713ff-103">How to tag a Linux virtual machine in Azure</span></span>
<span data-ttu-id="713ff-104">Ez a cikk ismerteti a különböző módjai a Linux virtuális gépek címke az Azure-ban a Resource Manager üzembe helyezési modellel keresztül.</span><span class="sxs-lookup"><span data-stu-id="713ff-104">This article describes different ways to tag a Linux virtual machine in Azure through the Resource Manager deployment model.</span></span> <span data-ttu-id="713ff-105">Címke található a felhasználó által definiált kulcs/érték párok, amely lehet tenni közvetlenül egy erőforrás vagy egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="713ff-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="713ff-106">Azure jelenleg legfeljebb 15 címkék erőforrás pedig erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="713ff-106">Azure currently supports up to 15 tags per resource and resource group.</span></span> <span data-ttu-id="713ff-107">Címkék erőforrás létrehozása idején helyezni vagy hozzáadni egy meglévő erőforrást.</span><span class="sxs-lookup"><span data-stu-id="713ff-107">Tags may be placed on a resource at the time of creation or added to an existing resource.</span></span> <span data-ttu-id="713ff-108">Vegye figyelembe, csak a Resource Manager üzembe helyezési modellel létrehozott erőforrások címkék használhatók.</span><span class="sxs-lookup"><span data-stu-id="713ff-108">Please note, tags are supported for resources created via the Resource Manager deployment model only.</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a><span data-ttu-id="713ff-109">Az Azure CLI-címkézés</span><span class="sxs-lookup"><span data-stu-id="713ff-109">Tagging with Azure CLI</span></span>
<span data-ttu-id="713ff-110">Első lépésként [telepítése és konfigurálása az Azure parancssori felület](../../xplat-cli-azure-resource-manager.md) , és győződjön meg arról, hogy az erőforrás-kezelő módban (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="713ff-110">To begin, [install and configure the Azure CLI](../../xplat-cli-azure-resource-manager.md) and make sure you are in Resource Manager mode (`azure config mode arm`).</span></span>

<span data-ttu-id="713ff-111">Az összes tulajdonság egy adott virtuális gép, többek között a címkéket, ezzel a paranccsal tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="713ff-111">You can view all properties for a given Virtual Machine, including the tags, using this command:</span></span>

        azure vm show -g MyResourceGroup -n MyTestVM

<span data-ttu-id="713ff-112">Egy új virtuális gép címke az Azure parancssori felületen keresztül hozzáadásához használja a `azure vm set` parancsot, és a tag paraméter **-t**:</span><span class="sxs-lookup"><span data-stu-id="713ff-112">To add a new VM tag through the Azure CLI, you can use the `azure vm set` command along with the tag parameter **-t**:</span></span>

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

<span data-ttu-id="713ff-113">Minden címkék eltávolításához használja a **– T** paramétere a `azure vm set` parancsot.</span><span class="sxs-lookup"><span data-stu-id="713ff-113">To remove all tags, you can use the **–T** parameter in the `azure vm set` command.</span></span>

        azure vm set – g MyResourceGroup –n MyTestVM -T


<span data-ttu-id="713ff-114">Most, hogy az erőforrások az Azure CLI és a portál jelenleg alkalmazott címkék, vessen egy pillantást a használat részleteiről a címkék a számlázási portál megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="713ff-114">Now that we have applied tags to our resources Azure CLI and the Portal, let’s take a look at the usage details to see the tags in the billing portal.</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="713ff-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="713ff-115">Next steps</span></span>
* <span data-ttu-id="713ff-116">Az Azure-erőforrások címkézés kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése] [ Azure Resource Manager Overview] és [címkék használata az Azure-erőforrások rendszerezéséhez][Using Tags to organize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="713ff-116">To learn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources].</span></span>
* <span data-ttu-id="713ff-117">Hogyan címkék segíthetnek az Azure-erőforrások kezeléséhez, olvassa el [ismertetése a Azure számlázási] [ Understanding your Azure Bill] és [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás][Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="713ff-117">To see how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
