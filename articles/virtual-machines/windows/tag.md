---
title: "aaaHow tootag egy Windows virtuális gép erőforrásához az Azure-ban |} Microsoft Docs"
description: "További tudnivalók az Azure-ban hello Resource Manager üzembe helyezési modellben létrehozott Windows virtuális gépek címkézés"
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
ms.openlocfilehash: 160416ddc35998b3c98c6e579668a6a5eb6de6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="a0aff-103">Hogyan tootag Windows virtuális gépként az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="a0aff-103">How tootag a Windows virtual machine in Azure</span></span>
<span data-ttu-id="a0aff-104">Ez a cikk ismerteti a különböző módokon tootag Windows virtuális gépként az Azure-ban keresztül hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="a0aff-104">This article describes different ways tootag a Windows virtual machine in Azure through hello Resource Manager deployment model.</span></span> <span data-ttu-id="a0aff-105">Címke található a felhasználó által definiált kulcs/érték párok, amely lehet tenni közvetlenül egy erőforrás vagy egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a0aff-105">Tags are user-defined key/value pairs which can be placed directly on a resource or a resource group.</span></span> <span data-ttu-id="a0aff-106">Azure jelenleg legfeljebb too15 címkék erőforrás pedig erőforráscsoportban támogat.</span><span class="sxs-lookup"><span data-stu-id="a0aff-106">Azure currently supports up too15 tags per resource and resource group.</span></span> <span data-ttu-id="a0aff-107">Címke lehet, hogy helyezni egy erőforrás létrehozása a hello időpontjában, vagy hozzá tooan meglévő erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a0aff-107">Tags may be placed on a resource at hello time of creation or added tooan existing resource.</span></span> <span data-ttu-id="a0aff-108">Vegye figyelembe, hogy az erőforrások hello Resource Manager üzembe helyezési modellben csak létrehozott címkék használhatók.</span><span class="sxs-lookup"><span data-stu-id="a0aff-108">Please note that tags are supported for resources created via hello Resource Manager deployment model only.</span></span> <span data-ttu-id="a0aff-109">Ha azt szeretné, hogy a Linux virtuális gépek tootag, [hogyan tootag egy Linux virtuális gép az Azure-ban](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a0aff-109">If you want tootag a Linux virtual machine, see [How tootag a Linux virtual machine in Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a><span data-ttu-id="a0aff-110">A PowerShell-címkézés</span><span class="sxs-lookup"><span data-stu-id="a0aff-110">Tagging with PowerShell</span></span>
<span data-ttu-id="a0aff-111">toocreate, adja hozzá, és a PowerShell segítségével címkék törlése, tooset kell fel a [PowerShell-környezet az Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span><span class="sxs-lookup"><span data-stu-id="a0aff-111">toocreate, add, and delete tags through PowerShell, you first need tooset up your [PowerShell environment with Azure Resource Manager][PowerShell environment with Azure Resource Manager].</span></span> <span data-ttu-id="a0aff-112">Hello telepítő befejezése után a számítási, hálózati és tárolási erőforrásokat a létrehozásakor vagy PowerShell hello erőforrás létrehozása után elhelyezheti címkék.</span><span class="sxs-lookup"><span data-stu-id="a0aff-112">Once you have completed hello setup, you can place tags on Compute, Network, and Storage resources at creation or after hello resource is created via PowerShell.</span></span> <span data-ttu-id="a0aff-113">Ez a cikk megtekintéséhez vagy szerkesztéséhez címkéket a virtuális gépek összpontosít.</span><span class="sxs-lookup"><span data-stu-id="a0aff-113">This article will concentrate on viewing/editing tags placed on Virtual Machines.</span></span>

<span data-ttu-id="a0aff-114">Először keresse meg a virtuális gép tooa keresztül hello `Get-AzureRmVM` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="a0aff-114">First, navigate tooa Virtual Machine through hello `Get-AzureRmVM` cmdlet.</span></span>

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

<span data-ttu-id="a0aff-115">Ha a virtuális gép már tartalmazza a címkék, majd megjelenik minden hello címkék az erőforrás:</span><span class="sxs-lookup"><span data-stu-id="a0aff-115">If your Virtual Machine already contains tags, you will then see all hello tags on your resource:</span></span>

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

<span data-ttu-id="a0aff-116">Ha szeretné tooadd címkék a PowerShell segítségével, használhatja a hello `Set-AzureRmResource` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a0aff-116">If you would like tooadd tags through PowerShell, you can use hello `Set-AzureRmResource` command.</span></span> <span data-ttu-id="a0aff-117">Vegye figyelembe a PowerShell segítségével, címkék frissítésekor címkék egész frissülnek.</span><span class="sxs-lookup"><span data-stu-id="a0aff-117">Note when updating tags through PowerShell, tags are updated as a whole.</span></span> <span data-ttu-id="a0aff-118">Így címke tooa egy erőforrást, amely már a címkét ad hozzá, ha szüksége lesz tooinclude toobe hello erőforrás elhelyezni kívánt összes hello címkét.</span><span class="sxs-lookup"><span data-stu-id="a0aff-118">So if you are adding one tag tooa resource that already has tags, you will need tooinclude all hello tags that you want toobe placed on hello resource.</span></span> <span data-ttu-id="a0aff-119">Az alábbiakban példája hogyan tooadd további címkék tooa erőforrás PowerShell parancsmagokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="a0aff-119">Below is an example of how tooadd additional tags tooa resource through PowerShell Cmdlets.</span></span>

<span data-ttu-id="a0aff-120">Ez a parancsmag első beállítja hello címkék helyezve *MyTestVM* toohello *$tags* változó hello segítségével `Get-AzureRmResource` és `Tags` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="a0aff-120">This first cmdlet sets all of hello tags placed on *MyTestVM* toohello *$tags* variable, using hello `Get-AzureRmResource` and `Tags` property.</span></span>

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

<span data-ttu-id="a0aff-121">hello második parancs megjeleníti az adott változó hello hello címkék.</span><span class="sxs-lookup"><span data-stu-id="a0aff-121">hello second command displays hello tags for hello given variable.</span></span>

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

<span data-ttu-id="a0aff-122">hello harmadik parancs hozzáadja egy további címke toohello *$tags* változó.</span><span class="sxs-lookup"><span data-stu-id="a0aff-122">hello third command adds an additional tag toohello *$tags* variable.</span></span> <span data-ttu-id="a0aff-123">Vegye figyelembe a hello hello használata  **+=**  tooappend hello új kulcs/érték pár toohello *$tags* listája.</span><span class="sxs-lookup"><span data-stu-id="a0aff-123">Note hello use of hello **+=** tooappend hello new key/value pair toohello *$tags* list.</span></span>

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

<span data-ttu-id="a0aff-124">hello negyedik parancs beállítja a hello definiált hello címkék *$tags* változó toohello resource biztosítani.</span><span class="sxs-lookup"><span data-stu-id="a0aff-124">hello fourth command sets all of hello tags defined in hello *$tags* variable toohello given resource.</span></span> <span data-ttu-id="a0aff-125">Ebben az esetben is MyTestVM.</span><span class="sxs-lookup"><span data-stu-id="a0aff-125">In this case, it is MyTestVM.</span></span>

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

<span data-ttu-id="a0aff-126">hello ötödik parancs megjeleníti hello címkék hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="a0aff-126">hello fifth command displays all of hello tags on hello resource.</span></span> <span data-ttu-id="a0aff-127">Ahogy látja, *hely* most egy címkét a típusúként van definiálva *MyLocation* hello értékként.</span><span class="sxs-lookup"><span data-stu-id="a0aff-127">As you can see, *Location* is now defined as a tag with *MyLocation* as hello value.</span></span>

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

<span data-ttu-id="a0aff-128">További részletek toolearn címkézése a PowerShell segítségével, tekintse meg hello [Azure Resource parancsmagok][Azure Resource Cmdlets].</span><span class="sxs-lookup"><span data-stu-id="a0aff-128">toolearn more about tagging through PowerShell, check out hello [Azure Resource Cmdlets][Azure Resource Cmdlets].</span></span>

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a><span data-ttu-id="a0aff-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0aff-129">Next steps</span></span>
* <span data-ttu-id="a0aff-130">toolearn az Azure-erőforrások, címkézéssel kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése] [ Azure Resource Manager Overview] és [tooorganize címkék használata az Azure-erőforrások] [ Using Tags tooorganize your Azure Resources].</span><span class="sxs-lookup"><span data-stu-id="a0aff-130">toolearn more about tagging your Azure resources, see [Azure Resource Manager Overview][Azure Resource Manager Overview] and [Using Tags tooorganize your Azure Resources][Using Tags tooorganize your Azure Resources].</span></span>
* <span data-ttu-id="a0aff-131">toosee hogyan címkék segítségével kezelése az Azure-erőforrások, lásd: [ismertetése a Azure számlázási] [ Understanding your Azure Bill] és [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás] [Gain insights into your Microsoft Azure resource consumption].</span><span class="sxs-lookup"><span data-stu-id="a0aff-131">toosee how tags can help you manage your use of Azure resources, see [Understanding your Azure Bill][Understanding your Azure Bill] and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption].</span></span>

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
