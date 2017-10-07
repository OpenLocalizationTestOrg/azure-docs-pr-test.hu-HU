---
title: "virtuális merevlemez aaaUpload fájl tooAzure DevTest Labs PowerShell használatával |} Microsoft Docs"
description: "Töltse fel a VHD fájlt toolab tárfiók PowerShell használatával"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 9c3ee96e212457b0ef8203714b419350cb97f895
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-powershell"></a><span data-ttu-id="25221-103">Töltse fel a VHD fájlt toolab tárfiók PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="25221-103">Upload VHD file toolab's storage account using PowerShell</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="25221-104">Az Azure DevTest Labs szolgáltatásban a VHD-fájlokat lehet használt toocreate egyéni képek, amelyek használt tooprovision virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="25221-104">In Azure DevTest Labs, VHD files can be used toocreate custom images, which are used tooprovision virtual machines.</span></span> <span data-ttu-id="25221-105">hello következő lépések végigvezetik PowerShell tooupload használatával egy virtuális merevlemez fájl tooa tesztkörnyezet tárfiókja.</span><span class="sxs-lookup"><span data-stu-id="25221-105">hello following steps walk you through using PowerShell tooupload a VHD file tooa lab's storage account.</span></span> <span data-ttu-id="25221-106">A VHD-fájl feltöltése után hello [további lépések szakaszt](#next-steps) felsorolja az egyes cikkeket, amelyek bemutatják, hogyan toocreate hello egy egyéni lemezkép feltöltése a VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="25221-106">Once you've uploaded your VHD file, hello [Next steps section](#next-steps) lists some articles that illustrate how toocreate a custom image from hello uploaded VHD file.</span></span> <span data-ttu-id="25221-107">További információ a lemezek és a VHD-ken az Azure-ban: [lemezek és a virtuális merevlemezek a virtuális gépek](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="25221-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="25221-108">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="25221-108">Step-by-step instructions</span></span>

<span data-ttu-id="25221-109">a következő lépéseket bejárása le a virtuális merevlemez feltöltése fájl tooAzure DevTest Labs PowerShell-lel hello.</span><span class="sxs-lookup"><span data-stu-id="25221-109">hello following steps walk you through uploading a VHD file tooAzure DevTest Labs using PowerShell.</span></span> 

1. <span data-ttu-id="25221-110">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="25221-110">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="25221-111">Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="25221-111">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="25221-112">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="25221-112">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="25221-113">Hello labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="25221-113">On hello lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="25221-114">A tesztkörnyezet hello **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="25221-114">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="25221-115">A hello **egyéni lemezképek** panelen válasszon ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="25221-115">On hello **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="25221-116">A hello **egyéni lemezkép** panelen válassza **VHD**.</span><span class="sxs-lookup"><span data-stu-id="25221-116">On hello **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="25221-117">A hello **VHD** panelen válassza **a PowerShell használatával virtuális merevlemez feltöltéséhez**.</span><span class="sxs-lookup"><span data-stu-id="25221-117">On hello **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Töltse fel a virtuális merevlemez a PowerShell használatával](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. <span data-ttu-id="25221-119">A hello **PowerShell-lel lemezkép feltöltése a** panelen, a Másolás generált hello PowerShell parancsfájl tooa szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="25221-119">On hello **Upload an image using PowerShell** blade, copy hello generated PowerShell script tooa text editor.</span></span>

1. <span data-ttu-id="25221-120">Módosítsa a hello **LocalFilePath** hello paramétere **Add-AzureVhd** parancsmag toopoint toohello hello tooupload kívánt VHD-fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="25221-120">Modify hello **LocalFilePath** parameter of hello **Add-AzureVhd** cmdlet toopoint toohello location of hello VHD file you want tooupload.</span></span>

1. <span data-ttu-id="25221-121">A PowerShell parancssorból futtassa a hello **Add-AzureVhd** parancsmag (hello módosított rendelkező **LocalFilePath** paraméter).</span><span class="sxs-lookup"><span data-stu-id="25221-121">At a PowerShell prompt, run hello **Add-AzureVhd** cmdlet (with hello modified **LocalFilePath** parameter).</span></span>

> [!WARNING] 
> 
> <span data-ttu-id="25221-122">a VHD-fájl feltöltése hello folyamat megnőhet hello hello VHD-fájl méretétől és a kapcsolat sebességétől függően.</span><span class="sxs-lookup"><span data-stu-id="25221-122">hello process of uploading a VHD file can be lengthy depending on hello size of hello VHD file and your connection speed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25221-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25221-123">Next steps</span></span>

- [<span data-ttu-id="25221-124">Hozzon létre egy egyéni lemezképet egy VHD-fájlt hello Azure-portálon az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="25221-124">Create a custom image in Azure DevTest Labs from a VHD file using hello Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="25221-125">Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="25221-125">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
