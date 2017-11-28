---
title: "Útmutató a Windows virtuális gép erőforrásához címke az Azure-ban |} Microsoft Docs"
description: "További tudnivalók az Azure-ban a Resource Manager üzembe helyezési modellel létrehozott Windows virtuális gépek címkézés"
services: virtual-machines-windows
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 56d17f45-e4a7-4d84-8022-b40334ae49d2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 5f00c4265cea3db02dbb09a7f81be636a3fdd3d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="d7d71-103">Útmutató a Windows rendszerű virtuális gép címke az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="d7d71-103">How to tag a Windows virtual machine in Azure</span></span>
<span data-ttu-id="d7d71-104">Ez a cikk ismerteti a Windows rendszerű virtuális gép címke az Azure-ban a Resource Manager üzembe helyezési modellel keresztül különböző módjai.</span><span class="sxs-lookup"><span data-stu-id="d7d71-104">This article describes different ways to tag a Windows virtual machine in Azure through the Resource Manager deployment model.</span></span> <span data-ttu-id="d7d71-105">Címke található a felhasználó által definiált kulcs/érték párok, amely lehet tenni közvetlenül egy erőforrás vagy egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d7d71-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="d7d71-106">Azure jelenleg legfeljebb 15 címkék erőforrás pedig erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="d7d71-106">Azure currently supports up to 15 tags per resource and resource group.</span></span> <span data-ttu-id="d7d71-107">Címkék erőforrás létrehozása idején helyezni vagy hozzáadni egy meglévő erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d7d71-107">Tags may be placed on a resource at the time of creation or added to an existing resource.</span></span> <span data-ttu-id="d7d71-108">Vegye figyelembe, hogy csak a Resource Manager üzembe helyezési modellel létrehozott erőforrások címkék használhatók.</span><span class="sxs-lookup"><span data-stu-id="d7d71-108">Please note that tags are supported for resources created via the Resource Manager deployment model only.</span></span> <span data-ttu-id="d7d71-109">Ha szeretne egy Linux virtuális gép címkét, lásd: [hogyan Linux virtuális gépek címke az Azure-ban](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7d71-109">If you want to tag a Linux virtual machine, see [How to tag a Linux virtual machine in Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a><span data-ttu-id="d7d71-110">A PowerShell-címkézés</span><span class="sxs-lookup"><span data-stu-id="d7d71-110">Tagging with PowerShell</span></span>
<span data-ttu-id="d7d71-111">Szeretne létrehozni, adja hozzá, és a PowerShell segítségével, először meg kell beállítása címkék törlése a [PowerShell-környezet az Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span><span class="sxs-lookup"><span data-stu-id="d7d71-111">To create, add, and delete tags through PowerShell, you first need to set up your [PowerShell environment with Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span></span> <span data-ttu-id="d7d71-112">A telepítés befejezése után a számítási, hálózati és tárolási erőforrásokat a létrehozásakor vagy az erőforrás PowerShell létrehozása után elhelyezheti címkék.</span><span class="sxs-lookup"><span data-stu-id="d7d71-112">Once you have completed the setup, you can place tags on Compute, Network, and Storage resources at creation or after the resource is created via PowerShell.</span></span> <span data-ttu-id="d7d71-113">Ez a cikk megtekintéséhez vagy szerkesztéséhez címkéket a virtuális gépek összpontosít.</span><span class="sxs-lookup"><span data-stu-id="d7d71-113">This article will concentrate on viewing/editing tags placed on Virtual Machines.</span></span>

<span data-ttu-id="d7d71-114">Először keresse meg a virtuális gép keresztül a `Get-AzureRmVM` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d7d71-114">First, navigate to a Virtual Machine through the `Get-AzureRmVM` cmdlet.</span></span>

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

<span data-ttu-id="d7d71-115">Ha a virtuális gép már tartalmazza a címkéket, az erőforrás majd jelenik meg a címkéket:</span><span class="sxs-lookup"><span data-stu-id="d7d71-115">If your Virtual Machine already contains tags, you will then see all the tags on your resource:</span></span>

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

<span data-ttu-id="d7d71-116">Ha szeretné a címkéket a PowerShell segítségével, használhatja a `Set-AzureRmResource` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d7d71-116">If you would like to add tags through PowerShell, you can use the `Set-AzureRmResource` command.</span></span> <span data-ttu-id="d7d71-117">Vegye figyelembe a PowerShell segítségével, címkék frissítésekor címkék egész frissülnek.</span><span class="sxs-lookup"><span data-stu-id="d7d71-117">Note when updating tags through PowerShell, tags are updated as a whole.</span></span> <span data-ttu-id="d7d71-118">Ezért egy címke, amely már a címkék erőforrás kíván hozzáadni, ha szüksége lesz az erőforrás elhelyezni kívánt címkéket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d7d71-118">So if you are adding one tag to a resource that already has tags, you will need to include all the tags that you want to be placed on the resource.</span></span> <span data-ttu-id="d7d71-119">Alább van további címkék hozzáadására egy PowerShell-parancsmagokkal erőforrás példát.</span><span class="sxs-lookup"><span data-stu-id="d7d71-119">Below is an example of how to add additional tags to a resource through PowerShell Cmdlets.</span></span>

<span data-ttu-id="d7d71-120">Az első parancsmag beállítja a címkék helyezve *MyTestVM* számára a *$tags* változó, használja a `Get-AzureRmResource` és `Tags` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="d7d71-120">This first cmdlet sets all of the tags placed on *MyTestVM* to the *$tags* variable, using the `Get-AzureRmResource` and `Tags` property.</span></span>

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

<span data-ttu-id="d7d71-121">A második parancs megjeleníti az adott változó címkék.</span><span class="sxs-lookup"><span data-stu-id="d7d71-121">The second command displays the tags for the given variable.</span></span>

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment

<span data-ttu-id="d7d71-122">A harmadik parancs hozzáadja egy további címkét a *$tags* változó.</span><span class="sxs-lookup"><span data-stu-id="d7d71-122">The third command adds an additional tag to the *$tags* variable.</span></span> <span data-ttu-id="d7d71-123">Vegye figyelembe a használatát a  **+=**  fűzhető hozzá az új kulcs/érték pár, hogy a *$tags* listája.</span><span class="sxs-lookup"><span data-stu-id="d7d71-123">Note the use of the **+=** to append the new key/value pair to the *$tags* list.</span></span>

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

<span data-ttu-id="d7d71-124">A negyedik parancs beállítja a meghatározott címkék a *$tags* változó az adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="d7d71-124">The fourth command sets all of the tags defined in the *$tags* variable to the given resource.</span></span> <span data-ttu-id="d7d71-125">Ebben az esetben is MyTestVM.</span><span class="sxs-lookup"><span data-stu-id="d7d71-125">In this case, it is MyTestVM.</span></span>

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

<span data-ttu-id="d7d71-126">Az ötödik parancs megjeleníti a címkék az erőforráson.</span><span class="sxs-lookup"><span data-stu-id="d7d71-126">The fifth command displays all of the tags on the resource.</span></span> <span data-ttu-id="d7d71-127">Ahogy látja, *hely* most egy címkét a típusúként van definiálva *MyLocation* értékeként.</span><span class="sxs-lookup"><span data-stu-id="d7d71-127">As you can see, *Location* is now defined as a tag with *MyLocation* as the value.</span></span>

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment
        Value        MyLocation
        Name        Location

<span data-ttu-id="d7d71-128">A PowerShell segítségével címkézés kapcsolatos további tudnivalókért tekintse meg a [Azure Resource parancsmagok][Azure Resource Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="d7d71-128">To learn more about tagging through PowerShell, check out the [Azure Resource Cmdlets][Azure Resource Cmdlets].</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="d7d71-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7d71-129">Next steps</span></span>
* <span data-ttu-id="d7d71-130">Az Azure-erőforrások címkézés kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése] [ Azure Resource Manager Overview] és [címkék használata az Azure-erőforrások rendszerezéséhez][Using Tags to organize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="d7d71-130">To learn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources].</span></span>
* <span data-ttu-id="d7d71-131">Hogyan címkék segíthetnek az Azure-erőforrások kezeléséhez, olvassa el [ismertetése a Azure számlázási] [ Understanding your Azure Bill] és [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás][Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="d7d71-131">To see how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
