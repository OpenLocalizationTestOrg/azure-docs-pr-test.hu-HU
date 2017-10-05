---
title: "Töltse fel a VHD-fájlt a Azure DevTest Labs AzCopy használatával |} Microsoft Docs"
description: "AzCopy segítségével tesztlabor a tárfiók VHD-fájl feltöltése"
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
ms.openlocfilehash: a4f43354740d9f17570932b0b9c753f46d67dc33
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a><span data-ttu-id="e2008-103">AzCopy segítségével tesztlabor a tárfiók VHD-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="e2008-103">Upload VHD file to lab's storage account using AzCopy</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="e2008-104">Azure DevTest Labs szolgáltatásban, a VHD-fájlok segítségével hozzon létre egyéni képek, amelyek segítségével virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="e2008-104">In Azure DevTest Labs, VHD files can be used to create custom images, which are used to provision virtual machines.</span></span> <span data-ttu-id="e2008-105">A következő lépések végigvezetik a VHD-fájl feltöltése a tesztkörnyezet tárfiókja az AzCopy parancssori segédprogram segítségével.</span><span class="sxs-lookup"><span data-stu-id="e2008-105">The following steps walk you through using the AzCopy command-line utility to upload a VHD file to a lab's storage account.</span></span> <span data-ttu-id="e2008-106">Miután a VHD-fájl feltöltött a [további lépések szakaszt](#next-steps) felsorolja az egyes cikkeket, amelyek bemutatják, hogyan lehet a feltöltött VHD-fájl létrehozása egyéni lemezkép.</span><span class="sxs-lookup"><span data-stu-id="e2008-106">Once you've uploaded your VHD file, the [Next steps section](#next-steps) lists some articles that illustrate how to create a custom image from the uploaded VHD file.</span></span> <span data-ttu-id="e2008-107">További információ a lemezek és a VHD-ken az Azure-ban: [lemezek és a virtuális merevlemezek a virtuális gépek](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="e2008-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

> [!NOTE] 
>  
> <span data-ttu-id="e2008-108">AzCopy csak Windows parancssori segédprogram.</span><span class="sxs-lookup"><span data-stu-id="e2008-108">AzCopy is a Windows-only command-line utility.</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="e2008-109">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="e2008-109">Step-by-step instructions</span></span>

<span data-ttu-id="e2008-110">A következő lépések végigvezetik a VHD-fájl feltöltése a Azure DevTest Labs segítségével [AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="e2008-110">The following steps walk you through uploading a VHD file to Azure DevTest Labs using [AzCopy](http://aka.ms/downloadazcopy).</span></span> 

1. <span data-ttu-id="e2008-111">Töltse le az Azure portál használatával a tesztkörnyezet tárfiókja nevét:</span><span class="sxs-lookup"><span data-stu-id="e2008-111">Get the name of the lab's storage account using the Azure portal:</span></span>

1. <span data-ttu-id="e2008-112">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="e2008-112">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="e2008-113">Válassza a **További szolgáltatások**, majd a **DevTest Labs** elemet a listából.</span><span class="sxs-lookup"><span data-stu-id="e2008-113">Select **More services**, and then select **DevTest Labs** from the list.</span></span>

1. <span data-ttu-id="e2008-114">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e2008-114">From the list of labs, select the desired lab.</span></span>  

1. <span data-ttu-id="e2008-115">A labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="e2008-115">On the lab's blade, select **Configuration**.</span></span> 

1. <span data-ttu-id="e2008-116">A tesztlabor a **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="e2008-116">On the lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>

1. <span data-ttu-id="e2008-117">Az a **egyéni lemezképek** panelen válasszon ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e2008-117">On the **Custom images** blade, Select **+Add**.</span></span> 

1. <span data-ttu-id="e2008-118">Az a **egyéni lemezkép** panelen válassza **VHD**.</span><span class="sxs-lookup"><span data-stu-id="e2008-118">On the **Custom image** blade, select **VHD**.</span></span>

1. <span data-ttu-id="e2008-119">Az a **VHD** panelen válassza **a PowerShell használatával virtuális merevlemez feltöltéséhez**.</span><span class="sxs-lookup"><span data-stu-id="e2008-119">On the **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>

    ![Töltse fel a virtuális merevlemez a PowerShell használatával](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. <span data-ttu-id="e2008-121">A **PowerShell-lel lemezkép feltöltése a** csempe megjeleníti hívása a **Add-AzureVhd** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e2008-121">The **Upload an image using PowerShell** blade displays a call to the **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="e2008-122">Az első paraméter (*cél*) tartalmaz a blob-tároló URI-JÁNAK (*feltölt*) a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="e2008-122">The first parameter (*Destination*) contains the URI for a blob container (*uploads*) in the following format:</span></span>

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. <span data-ttu-id="e2008-123">Jegyezze meg a teljes URI-címe, a későbbi lépésekben használatban van.</span><span class="sxs-lookup"><span data-stu-id="e2008-123">Make note of the full URI as it is used in later steps.</span></span>

1. <span data-ttu-id="e2008-124">Töltse fel az AzCopy VHD-fájlt:</span><span class="sxs-lookup"><span data-stu-id="e2008-124">Upload the VHD file using AzCopy:</span></span>
 
1. <span data-ttu-id="e2008-125">[Töltse le és telepítse a legújabb verziót az AzCopy](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="e2008-125">[Download and install the latest version of AzCopy](http://aka.ms/downloadazcopy).</span></span>

1. <span data-ttu-id="e2008-126">Nyisson meg egy parancsablakot, és keresse meg az AzCopy telepítési könyvtárára.</span><span class="sxs-lookup"><span data-stu-id="e2008-126">Open a command window and navigate to the AzCopy installation directory.</span></span> <span data-ttu-id="e2008-127">Másik lehetőségként az AzCopy telepítési helyet adhat hozzá a fájlrendszerbeli elérési.</span><span class="sxs-lookup"><span data-stu-id="e2008-127">Optionally, you can add the AzCopy installation location to your system path.</span></span> <span data-ttu-id="e2008-128">Alapértelmezés szerint a AzCopy telepítve van a következő könyvtárra:</span><span class="sxs-lookup"><span data-stu-id="e2008-128">By default, AzCopy is installed to the following directory:</span></span>

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. <span data-ttu-id="e2008-129">A fiók kulcs és a blob tároló URI használatával, a következő parancsot a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="e2008-129">Using the storage account key and blob container URI, run the following command at the command prompt.</span></span> <span data-ttu-id="e2008-130">A *vhdFileName* érték nem lehet idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="e2008-130">The *vhdFileName* value needs to be in quotes.</span></span> <span data-ttu-id="e2008-131">A folyamat egy VHD-fájl feltöltése megnőhet méretét a VHD-fájlt és a kapcsolat sebességétől függően.</span><span class="sxs-lookup"><span data-stu-id="e2008-131">The process of uploading a VHD file can be lengthy depending on the size of the VHD file and your connection speed.</span></span>   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a><span data-ttu-id="e2008-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2008-132">Next steps</span></span>

- [<span data-ttu-id="e2008-133">Létrehozhat egyéni rendszerképeket a VHD-fájl az Azure portál használata az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="e2008-133">Create a custom image in Azure DevTest Labs from a VHD file using the Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="e2008-134">Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="e2008-134">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)