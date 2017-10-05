---
title: "Létrehozhat vagy módosíthat labs Azure Resource Manager-sablonok használatával automatikusan a PowerShell használatával |} Microsoft Docs"
description: "Útmutató: Azure Resource Manager-sablonok létrehozására vagy módosítására automatikusan labor DevTest labs PowerShell segítségével"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: tarcher
ms.openlocfilehash: cea4531175df2cc39790497dc049d27e23ffa0c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a><span data-ttu-id="7ef76-103">Létrehozhat vagy módosíthat labs automatikusan az Azure Resource Manager-sablonok és a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7ef76-103">Create or modify labs automatically using Azure Resource Manager templates and PowerShell</span></span>

<span data-ttu-id="7ef76-104">DevTest Labs biztosít sok Azure Resource Manager-sablonok és a PowerShell-parancsfájlok, amely gyorsan és automatikusan hozzon létre új labs vagy módosítsa a meglévő labs és majd telepítenie kell ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="7ef76-104">DevTest Labs provides many Azure Resource Manager templates and PowerShell scripts that can help you quickly and automatically create new labs or modify existing labs and then deploy these resources.</span></span>

<span data-ttu-id="7ef76-105">Ez a cikk segít ismerteti a folyamatot, ezeket a sablonokat és parancsfájlok használatával automatizálhatja a létrehozását, módosítását és a tesztkörnyezetek központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="7ef76-105">This article helps guide you through the process of using these templates and scripts to automate the creation, modification, and deployment of your labs.</span></span> <span data-ttu-id="7ef76-106">Ez a cikk azt is bemutatja, hol találhatók a DevTest Labs szolgáltatásban néhány gyakori feladatok elvégzése PowerShell használatával kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="7ef76-106">This article also shows you where you can find more information about how to use PowerShell to perform some common tasks in DevTest Labs.</span></span>

## <a name="step-1-gather-your-templates-and-scripts"></a><span data-ttu-id="7ef76-107">1. lépés: A sablonok és a parancsfájlok összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="7ef76-107">Step 1: Gather your templates and scripts</span></span>
<span data-ttu-id="7ef76-108">Előre végrehajtott található [Azure Resource Manager-sablonok](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) és [PowerShell-parancsfájlok](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) , a nyilvános [Github-tárházban](https://github.com/Azure/azure-devtestlab).</span><span class="sxs-lookup"><span data-stu-id="7ef76-108">You can find pre-made [Azure Resource Manager templates](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) and [PowerShell scripts](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) at our public [Github repository](https://github.com/Azure/azure-devtestlab).</span></span> <span data-ttu-id="7ef76-109">Használja őket-van, vagy testre is szabhatja őket az igényeinek, és tárolja őket a saját [privát Git-tárház](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="7ef76-109">Use them as-is, or customize them for your needs and store them in your own [private Git repo](devtest-lab-add-artifact-repo.md).</span></span> 

## <a name="step-2-modify-your-azure-resource-manager-template"></a><span data-ttu-id="7ef76-110">2. lépés: Az Azure Resource Manager-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="7ef76-110">Step 2: Modify your Azure Resource Manager template</span></span>
<span data-ttu-id="7ef76-111">A következő lépések követésével [az első Azure Resource Manager-sablon létrehozása](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) Ha soha nem hozott létre egy sablont előtt.</span><span class="sxs-lookup"><span data-stu-id="7ef76-111">You can follow the steps at [Create your first Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) if you have never created a template before.</span></span>

<span data-ttu-id="7ef76-112">Emellett [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) kínál számos irányelvek és hozhat létre Azure Resource Manager-sablonok, amely javaslatokat is megbízható és könnyen használható.</span><span class="sxs-lookup"><span data-stu-id="7ef76-112">In addition, [Best practices for creating Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) offers many guidelines and suggestions to help you create Azure Resource Manager templates that are reliable and easy to use.</span></span> <span data-ttu-id="7ef76-113">Általában fogja használni a megközelítésekre, vagy a megadott példákat egyik változata, a sablon módosításához az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="7ef76-113">Typically, you will use a variation of one of the approaches or examples provided and modify your template for your needs.</span></span>

## <a name="step-3-deploy-resources-with-powershell"></a><span data-ttu-id="7ef76-114">3. lépés: Erőforrások az PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="7ef76-114">Step 3: Deploy resources with PowerShell</span></span>
<span data-ttu-id="7ef76-115">Miután a sablonok és a parancsfájlok testreszabott, szükséges lépéseit követve [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="7ef76-115">After you have customized your templates and scripts, follow the steps necessary to [deploy resources with Resource Manager templates and Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span> <span data-ttu-id="7ef76-116">A cikk az Azure PowerShell használata Azure Resource Manager-sablonok az erőforrások telepítése az Azure-bA általános információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="7ef76-116">The article provides general information about using Azure PowerShell with Azure Resource Manager templates to deploy your resources to Azure.</span></span>


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a><span data-ttu-id="7ef76-117">Gyakori feladatok elvégzése a DevTest Labs szolgáltatásban a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7ef76-117">Common tasks you can perform in DevTest labs using PowerShell</span></span>
<span data-ttu-id="7ef76-118">Nincsenek sok kapcsolatos további általános feladatok PowerShell segítségével automatizálható.</span><span class="sxs-lookup"><span data-stu-id="7ef76-118">There are many other common tasks that you can automate by using PowerShell.</span></span> <span data-ttu-id="7ef76-119">A dokumentáció az alábbi szakaszok felsorolják a feladatok végrehajtásához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7ef76-119">The following sections of the documentation outline the steps required to perform these tasks.</span></span>

* [<span data-ttu-id="7ef76-120">Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl</span><span class="sxs-lookup"><span data-stu-id="7ef76-120">Create a custom image from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [<span data-ttu-id="7ef76-121">Töltse fel a VHD-fájlt a tesztlabor a tárfiók PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7ef76-121">Upload VHD file to lab's storage account using PowerShell</span></span>](devtest-lab-upload-vhd-using-powershell.md)
* [<span data-ttu-id="7ef76-122">Külső felhasználó hozzáadása egy laborhoz PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7ef76-122">Add an external user to a lab using PowerShell</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [<span data-ttu-id="7ef76-123">PowerShell-lel labor egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ef76-123">Create a lab custom role using PowerShell</span></span>](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a><span data-ttu-id="7ef76-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ef76-124">Next steps</span></span>
* <span data-ttu-id="7ef76-125">Megtudhatja, hogyan hozzon létre egy [privát Git-tárház](devtest-lab-add-artifact-repo.md) tárolására az egyéni sablonok vagy parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="7ef76-125">Learn how to create a [private Git repository](devtest-lab-add-artifact-repo.md) where you will store your customized templates or scripts.</span></span>
* <span data-ttu-id="7ef76-126">Megismerkedhet a [Azure Resource Manager sablonok Azure gyors üzembe helyezési sablon gyűjteményből](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="7ef76-126">Explore the [Azure Resource Manager templates from Azure Quickstart template gallery](https://github.com/Azure/azure-quickstart-templates).</span></span>
