---
title: "aaaCreate vagy a PowerShell használatával automatikusan Azure Resource Manager-sablonok labs módosítása |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure Resource Manager-sablon is van a PowerShell toocreate vagy automatikusan labor DevTest labs módosítása"
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
ms.openlocfilehash: 29c8bc67caaec17b1f8926dde4e5d9d314b06600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a><span data-ttu-id="97c17-103">Létrehozhat vagy módosíthat labs automatikusan az Azure Resource Manager-sablonok és a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="97c17-103">Create or modify labs automatically using Azure Resource Manager templates and PowerShell</span></span>

<span data-ttu-id="97c17-104">DevTest Labs biztosít sok Azure Resource Manager-sablonok és a PowerShell-parancsfájlok, amely gyorsan és automatikusan hozzon létre új labs vagy módosítsa a meglévő labs és majd telepítenie kell ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="97c17-104">DevTest Labs provides many Azure Resource Manager templates and PowerShell scripts that can help you quickly and automatically create new labs or modify existing labs and then deploy these resources.</span></span>

<span data-ttu-id="97c17-105">Ez a cikk segít hello folyamatot, amely használatával a sablonok és a parancsfájlok tooautomate hello létrehozását, módosítását és a labs központi telepítését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="97c17-105">This article helps guide you through hello process of using these templates and scripts tooautomate hello creation, modification, and deployment of your labs.</span></span> <span data-ttu-id="97c17-106">Ez a cikk azt is bemutatja, hogyan toouse PowerShell tooperform néhány gyakori feladatok a DevTest Labs szolgáltatásban kapcsolatos további információk megkeresésének.</span><span class="sxs-lookup"><span data-stu-id="97c17-106">This article also shows you where you can find more information about how toouse PowerShell tooperform some common tasks in DevTest Labs.</span></span>

## <a name="step-1-gather-your-templates-and-scripts"></a><span data-ttu-id="97c17-107">1. lépés: A sablonok és a parancsfájlok összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="97c17-107">Step 1: Gather your templates and scripts</span></span>
<span data-ttu-id="97c17-108">Előre végrehajtott található [Azure Resource Manager-sablonok](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) és [PowerShell-parancsfájlok](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) , a nyilvános [Github-tárházban](https://github.com/Azure/azure-devtestlab).</span><span class="sxs-lookup"><span data-stu-id="97c17-108">You can find pre-made [Azure Resource Manager templates](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) and [PowerShell scripts](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) at our public [Github repository](https://github.com/Azure/azure-devtestlab).</span></span> <span data-ttu-id="97c17-109">Használja őket-van, vagy testre is szabhatja őket az igényeinek, és tárolja őket a saját [privát Git-tárház](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="97c17-109">Use them as-is, or customize them for your needs and store them in your own [private Git repo](devtest-lab-add-artifact-repo.md).</span></span> 

## <a name="step-2-modify-your-azure-resource-manager-template"></a><span data-ttu-id="97c17-110">2. lépés: Az Azure Resource Manager-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="97c17-110">Step 2: Modify your Azure Resource Manager template</span></span>
<span data-ttu-id="97c17-111">Hajtsa végre az hello készítésével [az első Azure Resource Manager-sablon létrehozása](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) Ha soha nem hozott létre egy sablont előtt.</span><span class="sxs-lookup"><span data-stu-id="97c17-111">You can follow hello steps at [Create your first Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) if you have never created a template before.</span></span>

<span data-ttu-id="97c17-112">Emellett [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) sok útmutatást és javaslatokat toohelp kínál, amelyek megbízható és könnyű toouse Azure Resource Manager-sablonokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="97c17-112">In addition, [Best practices for creating Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) offers many guidelines and suggestions toohelp you create Azure Resource Manager templates that are reliable and easy toouse.</span></span> <span data-ttu-id="97c17-113">Általában egy változata, hello módszerek egyikét használhatja vagy példák megadott, és módosítsa a sablon az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="97c17-113">Typically, you will use a variation of one of hello approaches or examples provided and modify your template for your needs.</span></span>

## <a name="step-3-deploy-resources-with-powershell"></a><span data-ttu-id="97c17-114">3. lépés: Erőforrások az PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="97c17-114">Step 3: Deploy resources with PowerShell</span></span>
<span data-ttu-id="97c17-115">Miután a sablonok és a parancsfájlok testreszabott, lépésekkel hello szükséges túl[erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="97c17-115">After you have customized your templates and scripts, follow hello steps necessary too[deploy resources with Resource Manager templates and Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span> <span data-ttu-id="97c17-116">hello a cikk az Azure PowerShell használata az Azure Resource Manager sablonok toodeploy általános információkat az erőforrások tooAzure nyújt.</span><span class="sxs-lookup"><span data-stu-id="97c17-116">hello article provides general information about using Azure PowerShell with Azure Resource Manager templates toodeploy your resources tooAzure.</span></span>


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a><span data-ttu-id="97c17-117">Gyakori feladatok elvégzése a DevTest Labs szolgáltatásban a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="97c17-117">Common tasks you can perform in DevTest labs using PowerShell</span></span>
<span data-ttu-id="97c17-118">Nincsenek sok kapcsolatos további általános feladatok PowerShell segítségével automatizálható.</span><span class="sxs-lookup"><span data-stu-id="97c17-118">There are many other common tasks that you can automate by using PowerShell.</span></span> <span data-ttu-id="97c17-119">hello hello dokumentáció a következő szakaszok felsorolják hello lépéseket szükséges tooperform ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="97c17-119">hello following sections of hello documentation outline hello steps required tooperform these tasks.</span></span>

* [<span data-ttu-id="97c17-120">Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl</span><span class="sxs-lookup"><span data-stu-id="97c17-120">Create a custom image from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [<span data-ttu-id="97c17-121">Töltse fel a VHD fájlt toolab tárfiók PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="97c17-121">Upload VHD file toolab's storage account using PowerShell</span></span>](devtest-lab-upload-vhd-using-powershell.md)
* [<span data-ttu-id="97c17-122">Adja hozzá egy külső felhasználó tooa labor PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="97c17-122">Add an external user tooa lab using PowerShell</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [<span data-ttu-id="97c17-123">PowerShell-lel labor egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="97c17-123">Create a lab custom role using PowerShell</span></span>](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a><span data-ttu-id="97c17-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97c17-124">Next steps</span></span>
* <span data-ttu-id="97c17-125">Megtudhatja, hogyan toocreate egy [privát Git-tárház](devtest-lab-add-artifact-repo.md) tárolására az egyéni sablonok vagy parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="97c17-125">Learn how toocreate a [private Git repository](devtest-lab-add-artifact-repo.md) where you will store your customized templates or scripts.</span></span>
* <span data-ttu-id="97c17-126">Fedezze fel hello [Azure Resource Manager sablonok Azure gyors üzembe helyezési sablon gyűjteményből](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="97c17-126">Explore hello [Azure Resource Manager templates from Azure Quickstart template gallery](https://github.com/Azure/azure-quickstart-templates).</span></span>
