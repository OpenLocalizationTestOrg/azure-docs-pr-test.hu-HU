---
title: "virtuális merevlemez aaaUpload fájl tooAzure DevTest Labs AzCopy használatával |} Microsoft Docs"
description: "Töltse fel a VHD fájlt toolab tárfiók AzCopy használatával"
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
ms.openlocfilehash: 14f9e933b0bd27451f6bcb94841ecc381213e578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-azcopy"></a><span data-ttu-id="3c89e-103">Töltse fel a VHD fájlt toolab tárfiók AzCopy használatával</span><span class="sxs-lookup"><span data-stu-id="3c89e-103">Upload VHD file toolab's storage account using AzCopy</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="3c89e-104">Az Azure DevTest Labs szolgáltatásban a VHD-fájlokat lehet használt toocreate egyéni képek, amelyek használt tooprovision virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="3c89e-104">In Azure DevTest Labs, VHD files can be used toocreate custom images, which are used tooprovision virtual machines.</span></span> <span data-ttu-id="3c89e-105">hello lépések végigvezetik Önt hello AzCopy parancssori segédprogram tooupload használatával egy virtuális merevlemez fájl tooa tesztkörnyezet tárfiókja.</span><span class="sxs-lookup"><span data-stu-id="3c89e-105">hello following steps walk you through using hello AzCopy command-line utility tooupload a VHD file tooa lab's storage account.</span></span> <span data-ttu-id="3c89e-106">A VHD-fájl feltöltése után hello [további lépések szakaszt](#next-steps) felsorolja az egyes cikkeket, amelyek bemutatják, hogyan toocreate hello egy egyéni lemezkép feltöltése a VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="3c89e-106">Once you've uploaded your VHD file, hello [Next steps section](#next-steps) lists some articles that illustrate how toocreate a custom image from hello uploaded VHD file.</span></span> <span data-ttu-id="3c89e-107">További információ a lemezek és a VHD-ken az Azure-ban: [lemezek és a virtuális merevlemezek a virtuális gépek](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="3c89e-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

> [!NOTE] 
>  
> <span data-ttu-id="3c89e-108">AzCopy csak Windows parancssori segédprogram.</span><span class="sxs-lookup"><span data-stu-id="3c89e-108">AzCopy is a Windows-only command-line utility.</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="3c89e-109">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="3c89e-109">Step-by-step instructions</span></span>

<span data-ttu-id="3c89e-110">következő lépések bejárása le a virtuális merevlemez feltöltése fájl tooAzure DevTest Labs segítségével hello [AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="3c89e-110">hello following steps walk you through uploading a VHD file tooAzure DevTest Labs using [AzCopy](http://aka.ms/downloadazcopy).</span></span> 

1. <span data-ttu-id="3c89e-111">Hello Azure-portál használatával hello tesztkörnyezet tárfiókja nevére hello beolvasása:</span><span class="sxs-lookup"><span data-stu-id="3c89e-111">Get hello name of hello lab's storage account using hello Azure portal:</span></span>

1. <span data-ttu-id="3c89e-112">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="3c89e-112">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="3c89e-113">Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="3c89e-113">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="3c89e-114">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="3c89e-114">From hello list of labs, select hello desired lab.</span></span>  

1. <span data-ttu-id="3c89e-115">Hello labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="3c89e-115">On hello lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="3c89e-116">A tesztkörnyezet hello **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="3c89e-116">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="3c89e-117">A hello **egyéni lemezképek** panelen válasszon ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3c89e-117">On hello **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="3c89e-118">A hello **egyéni lemezkép** panelen válassza **VHD**.</span><span class="sxs-lookup"><span data-stu-id="3c89e-118">On hello **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="3c89e-119">A hello **VHD** panelen válassza **a PowerShell használatával virtuális merevlemez feltöltéséhez**.</span><span class="sxs-lookup"><span data-stu-id="3c89e-119">On hello **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Töltse fel a virtuális merevlemez a PowerShell használatával](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. <span data-ttu-id="3c89e-121">Hello **PowerShell-lel lemezkép feltöltése a** panelt jeleníti meg a hívás toohello **Add-AzureVhd** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="3c89e-121">hello **Upload an image using PowerShell** blade displays a call toohello **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="3c89e-122">az első paraméter hello (*cél*) hello URI blob-tároló tartalmazza (*feltölt*) a következő formátum hello:</span><span class="sxs-lookup"><span data-stu-id="3c89e-122">hello first parameter (*Destination*) contains hello URI for a blob container (*uploads*) in hello following format:</span></span>

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. <span data-ttu-id="3c89e-123">Jegyezze fel a hello teljes URI, a későbbi lépésekben használatban van.</span><span class="sxs-lookup"><span data-stu-id="3c89e-123">Make note of hello full URI as it is used in later steps.</span></span>

1. <span data-ttu-id="3c89e-124">Töltse fel az AzCopy segítségével hello VHD-fájlt:</span><span class="sxs-lookup"><span data-stu-id="3c89e-124">Upload hello VHD file using AzCopy:</span></span>
 
1. <span data-ttu-id="3c89e-125">[Töltse le és telepítse az AzCopy legújabb verzióját hello](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="3c89e-125">[Download and install hello latest version of AzCopy](http://aka.ms/downloadazcopy).</span></span>

1. <span data-ttu-id="3c89e-126">Nyisson meg egy parancsablakot, és keresse meg a toohello AzCopy telepítési könyvtárára.</span><span class="sxs-lookup"><span data-stu-id="3c89e-126">Open a command window and navigate toohello AzCopy installation directory.</span></span> <span data-ttu-id="3c89e-127">Másik lehetőségként hello AzCopy telepítési tooyour rendszer elérési útjának is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="3c89e-127">Optionally, you can add hello AzCopy installation location tooyour system path.</span></span> <span data-ttu-id="3c89e-128">Alapértelmezés szerint az AzCopy telepített toohello a következő könyvtár:</span><span class="sxs-lookup"><span data-stu-id="3c89e-128">By default, AzCopy is installed toohello following directory:</span></span>

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. <span data-ttu-id="3c89e-129">Hello fiók kulcs és a blob tároló URI, futtassa a következő parancs hello parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="3c89e-129">Using hello storage account key and blob container URI, run hello following command at hello command prompt.</span></span> <span data-ttu-id="3c89e-130">Hello *vhdFileName* értéket kell toobe idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="3c89e-130">hello *vhdFileName* value needs toobe in quotes.</span></span> <span data-ttu-id="3c89e-131">a VHD-fájl feltöltése hello folyamat megnőhet hello hello VHD-fájl méretétől és a kapcsolat sebességétől függően.</span><span class="sxs-lookup"><span data-stu-id="3c89e-131">hello process of uploading a VHD file can be lengthy depending on hello size of hello VHD file and your connection speed.</span></span>   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a><span data-ttu-id="3c89e-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c89e-132">Next steps</span></span>

- [<span data-ttu-id="3c89e-133">Hozzon létre egy egyéni lemezképet egy VHD-fájlt hello Azure-portálon az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="3c89e-133">Create a custom image in Azure DevTest Labs from a VHD file using hello Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="3c89e-134">Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="3c89e-134">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)