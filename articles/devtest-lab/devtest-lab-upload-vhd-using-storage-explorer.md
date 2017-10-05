---
title: "VHD-fájl feltöltése az Azure DevTest Labs segítségével a Microsoft Azure Tártallózó |} Microsoft Docs"
description: "Használatával a Microsoft Azure Tártallózó tesztlabor a tárfiók VHD-fájl feltöltése"
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
ms.openlocfilehash: 502e2536fb0fd2e9dfc4c7b85a6fb4e18202f38f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upload-vhd-file-to-labs-storage-account-using-microsoft-azure-storage-explorer"></a><span data-ttu-id="13e38-103">Használatával a Microsoft Azure Tártallózó tesztlabor a tárfiók VHD-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="13e38-103">Upload VHD file to lab's storage account using Microsoft Azure Storage Explorer</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="13e38-104">Azure DevTest Labs szolgáltatásban, a VHD-fájlok segítségével hozzon létre egyéni képek, amelyek segítségével virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="13e38-104">In Azure DevTest Labs, VHD files can be used to create custom images, which are used to provision virtual machines.</span></span> <span data-ttu-id="13e38-105">Ez a cikk bemutatja, hogyan használandó [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy VHD-fájlt feltölteni a tesztkörnyezet tárfiókja.</span><span class="sxs-lookup"><span data-stu-id="13e38-105">This article illustrates how to use [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) to upload a VHD file to a lab's storage account.</span></span> <span data-ttu-id="13e38-106">Miután a VHD-fájl feltöltött a [további lépések szakaszt](#next-steps) felsorolja az egyes cikkeket, amelyek bemutatják, hogyan lehet a feltöltött VHD-fájl létrehozása egyéni lemezkép.</span><span class="sxs-lookup"><span data-stu-id="13e38-106">Once you've uploaded your VHD file, the [Next steps section](#next-steps) lists some articles that illustrate how to create a custom image from the uploaded VHD file.</span></span> <span data-ttu-id="13e38-107">További információ a lemezek és a VHD-ken az Azure-ban: [lemezek és a virtuális merevlemezek a virtuális gépek](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="13e38-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="13e38-108">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="13e38-108">Step-by-step instructions</span></span>

<span data-ttu-id="13e38-109">A következő lépések végigvezetik a VHD-fájl feltöltése a DevTest Labs segítségével [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="13e38-109">The following steps walk you through uploading a VHD file to DevTest Labs using [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>

1. <span data-ttu-id="13e38-110">[Töltse le és telepítse a legújabb verzióját a Microsoft Azure Tártallózó](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="13e38-110">[Download and install the latest version of the Microsoft Azure Storage Explorer](http://www.storageexplorer.com).</span></span>

1. <span data-ttu-id="13e38-111">Töltse le az Azure portál használatával a tesztkörnyezet tárfiókja nevét:</span><span class="sxs-lookup"><span data-stu-id="13e38-111">Get the name of the lab's storage account using the Azure portal:</span></span>

    1. <span data-ttu-id="13e38-112">Jelentkezzen be az [Azure Portalra](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="13e38-112">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
    
    1. <span data-ttu-id="13e38-113">Válassza a **További szolgáltatások**, majd a **DevTest Labs** elemet a listából.</span><span class="sxs-lookup"><span data-stu-id="13e38-113">Select **More services**, and then select **DevTest Labs** from the list.</span></span>
    
    1. <span data-ttu-id="13e38-114">Válassza ki a kívánt labor labs listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="13e38-114">From the list of labs, select the desired lab.</span></span>  
    
    1. <span data-ttu-id="13e38-115">A labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="13e38-115">On the lab's blade, select **Configuration**.</span></span> 
    
    1. <span data-ttu-id="13e38-116">A tesztlabor a **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="13e38-116">On the lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>
    
    1. <span data-ttu-id="13e38-117">Az a **egyéni lemezképek** panelen válasszon ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="13e38-117">On the **Custom images** blade, Select **+Add**.</span></span> 
    
    1. <span data-ttu-id="13e38-118">Az a **egyéni lemezkép** panelen válassza **VHD**.</span><span class="sxs-lookup"><span data-stu-id="13e38-118">On the **Custom image** blade, select **VHD**.</span></span>
    
    1. <span data-ttu-id="13e38-119">Az a **VHD** panelen válassza **a PowerShell használatával virtuális merevlemez feltöltéséhez**.</span><span class="sxs-lookup"><span data-stu-id="13e38-119">On the **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>
    
        ![Töltse fel a virtuális merevlemez a PowerShell használatával][0]
    
    1. <span data-ttu-id="13e38-121">A **PowerShell-lel lemezkép feltöltése a** csempe megjeleníti hívása a **Add-AzureVhd** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="13e38-121">The **Upload an image using PowerShell** blade displays a call to the **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="13e38-122">Az első paraméter (*cél*) tartalmazza a tárfiók nevét a labor a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="13e38-122">The first parameter (*Destination*) contains the storage account name for the lab in the following format:</span></span>
    
        <span data-ttu-id="13e38-123">https://<STORAGE-ACCOUNT-Name>.BLOB.Core.Windows.NET/uploads/...</span><span class="sxs-lookup"><span data-stu-id="13e38-123">https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...</span></span> 

    1. <span data-ttu-id="13e38-124">Jegyezze meg a tárfiók nevét, a későbbi lépésekben használatban van.</span><span class="sxs-lookup"><span data-stu-id="13e38-124">Make note of the storage account name as it is used in later steps.</span></span>
    
1. <span data-ttu-id="13e38-125">Csatlakozás Azure-előfizetés fiókhoz Tártallózó használatával.</span><span class="sxs-lookup"><span data-stu-id="13e38-125">Connect to an Azure subscription account using Storage Explorer.</span></span>

    > [!TIP] 
    > 
    > <span data-ttu-id="13e38-126">A Tártallózó kapcsolati beállításokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="13e38-126">Storage Explorer supports several connection options.</span></span> <span data-ttu-id="13e38-127">Ez a szakasz mutatja be, az Azure-előfizetéshez társított storage-fiókhoz való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="13e38-127">This section illustrates connecting to a storage account associated with your Azure subscription.</span></span> <span data-ttu-id="13e38-128">A Tártallózó által támogatott más kapcsolati beállítások megtekintéséhez tekintse meg a cikk [Ismerkedés a Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="13e38-128">To see the other connection options supported by Storage Explorer, refer to the article, [Getting started with Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>
 
    1. <span data-ttu-id="13e38-129">Nyissa meg a Tártallózót.</span><span class="sxs-lookup"><span data-stu-id="13e38-129">Open Storage Explorer.</span></span>
    
    1. <span data-ttu-id="13e38-130">A Tártallózó, válassza ki a **Azure Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="13e38-130">In Storage Explorer, select **Azure Account settings**.</span></span> 
    
        ![Azure-fiók beállításai][1]
    
    1. <span data-ttu-id="13e38-132">A bal oldali ablaktáblán, a Microsoft-fiókkal bejelentkezett meg.</span><span class="sxs-lookup"><span data-stu-id="13e38-132">The left pane displays the Microsoft accounts you've logged in to.</span></span> <span data-ttu-id="13e38-133">Egy másik fiókhoz csatlakozáshoz válassza a **Fiók hozzáadása** lehetőséget, és a párbeszédpanelek útmutatásait követve jelentkezzen be egy olyan Microsoft-fiókkal, amely legalább egy aktív Azure-előfizetéssel társítva van.</span><span class="sxs-lookup"><span data-stu-id="13e38-133">To connect to another account, select **Add an account**, and follow the dialogs to sign in with a Microsoft account that is associated with at least one active Azure subscription.</span></span>
    
        ![Fiók hozzáadása][2]
    
    1. <span data-ttu-id="13e38-135">Amint sikeresen bejelentkezett egy Microsoft-fiókkal, a bal oldali ablaktáblán megjelenik a fiókhoz társított összes Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="13e38-135">Once you successfully sign in with a Microsoft account, the left pane populates with the Azure subscriptions associated with that account.</span></span> <span data-ttu-id="13e38-136">Válassza ki azt az Azure-előfizetést amellyel dolgozni szeretne, majd válassza az **Alkalmaz** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="13e38-136">Select the Azure subscriptions with which you want to work, and then select **Apply**.</span></span> <span data-ttu-id="13e38-137">(Kiválasztásával **előfizetéseket** ki-és a kiválasztott összes vagy egyik sem a felsorolt Azure-előfizetést.)</span><span class="sxs-lookup"><span data-stu-id="13e38-137">(Selecting **All subscriptions** toggles the selection of all or none of the listed Azure subscriptions.)</span></span>
    
        ![Azure-előfizetések kiválasztása][3]
    
    1. <span data-ttu-id="13e38-139">A bal oldali ablaktábla megjeleníti a kiválasztott Azure-előfizetésekhez társított összes tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="13e38-139">The left pane displays the storage accounts associated with the selected Azure subscriptions.</span></span>
    
        ![Kiválasztott Azure-előfizetések][4]

1. <span data-ttu-id="13e38-141">Keresse meg a tesztkörnyezet tárfiókja:</span><span class="sxs-lookup"><span data-stu-id="13e38-141">Locate the lab's storage account:</span></span>

    1. <span data-ttu-id="13e38-142">A Tártallózó bal oldali ablaktáblán keresse meg, és bontsa ki a csomópontot az Azure-előfizetés, amely a tesztkörnyezet tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="13e38-142">In the Storage Explorer left pane, locate, and expand the node for the Azure subscription that owns the lab.</span></span>
    
    1. <span data-ttu-id="13e38-143">Az előfizetési csomópontot, bontsa ki a **Tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="13e38-143">Under the subscription's node, expand **Storage Accounts**.</span></span>

    1. <span data-ttu-id="13e38-144">Bontsa ki a labor tárolási fiók csomópontot, hogy láthatóvá váljon a csomópontok **Blobtárolók**, **fájlmegosztások**, **várólisták**, és **táblák**.</span><span class="sxs-lookup"><span data-stu-id="13e38-144">Expand the lab's storage account node to reveal nodes for **Blob Containers**, **File Shares**, **Queues**, and **Tables**.</span></span>
    
    1. <span data-ttu-id="13e38-145">Bontsa ki a **Blobtárolók** csomópont.</span><span class="sxs-lookup"><span data-stu-id="13e38-145">Expand the **Blob Containers** node.</span></span>
    
    1. <span data-ttu-id="13e38-146">A tartalom megjelenítéséhez a jobb oldali ablaktáblában a feltöltések blob-tároló választása.</span><span class="sxs-lookup"><span data-stu-id="13e38-146">Select the uploads blob container to display its contents in the right pane.</span></span>
        
        ![Directory feltöltése][5]

1. <span data-ttu-id="13e38-148">Töltse fel a VHD-fájlt a Tártallózó:</span><span class="sxs-lookup"><span data-stu-id="13e38-148">Upload the VHD file using Storage Explorer:</span></span>

    1. <span data-ttu-id="13e38-149">A Tártallózó jobb oldali ablaktáblában kell megjelennie a blobok a lista tartalmazza a **feltölt** blob tároló a tesztkörnyezet tárfiókja.</span><span class="sxs-lookup"><span data-stu-id="13e38-149">In the Storage Explorer right pane, you should see a listing of the blobs in the **uploads** blob container of the lab's storage account.</span></span> <span data-ttu-id="13e38-150">Válassza a blob-szerkesztő eszköztár **feltöltése**</span><span class="sxs-lookup"><span data-stu-id="13e38-150">On the blob editor toolbar, select **Upload**</span></span> 
        
        ![Töltse fel gomb][6]
    
    1. <span data-ttu-id="13e38-152">Az a **feltöltése** legördülő menüben válassza **fájlok feltöltése...** .</span><span class="sxs-lookup"><span data-stu-id="13e38-152">From the **Upload** drop-down menu, select **Upload files...**.</span></span>
    
    1. <span data-ttu-id="13e38-153">Az a **fájlok feltöltése** párbeszédpanelen válassza ki a három pont.</span><span class="sxs-lookup"><span data-stu-id="13e38-153">On the **Upload files** dialog, select the ellipsis.</span></span>
        
        ![Fájl kiválasztása][8]  

    1. <span data-ttu-id="13e38-155">A a **válassza ki a feltöltendő fájlt** párbeszédpanelen keresse meg a kívánt VHD-fájlt, jelölje ki, majd válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="13e38-155">On the **Select files to upload** dialog, browse to the desired VHD file, select it, and then select **Open**.</span></span>
    
    1. <span data-ttu-id="13e38-156">Amikor visszatér a **fájlok feltöltése** párbeszédpanelen módosítsa **Blob-típusú** való **oldalakra vonatkozó Blob**.</span><span class="sxs-lookup"><span data-stu-id="13e38-156">When returned to the **Upload files** dialog, change **Blob type** to **Page Blob**.</span></span>
    
    1. <span data-ttu-id="13e38-157">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="13e38-157">Select **Upload**.</span></span>

        ![Fájl kiválasztása][9]  
    
    1. <span data-ttu-id="13e38-159">A Tártallózó **tevékenységnapló** ablaktábla megjeleníti azokat a letöltés állapota (együtt megszakítani a feltöltést mutató).</span><span class="sxs-lookup"><span data-stu-id="13e38-159">The Storage Explorer **Activity Log** pane shows the download status (along with links to cancel the upload).</span></span> <span data-ttu-id="13e38-160">A folyamat egy VHD-fájl feltöltése megnőhet méretét a VHD-fájlt és a kapcsolat sebességétől függően.</span><span class="sxs-lookup"><span data-stu-id="13e38-160">The process of uploading a VHD file can be lengthy depending on the size of the VHD file and your connection speed.</span></span> 

        ![Fájlfeltöltés állapot][10]  

## <a name="next-steps"></a><span data-ttu-id="13e38-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13e38-162">Next steps</span></span>

- [<span data-ttu-id="13e38-163">Létrehozhat egyéni rendszerképeket a VHD-fájl az Azure portál használata az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="13e38-163">Create a custom image in Azure DevTest Labs from a VHD file using the Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="13e38-164">Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="13e38-164">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

[0]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-image-using-psh.png
[1]: ./media/devtest-lab-upload-vhd-using-storage-explorer/settings-icon.png
[2]: ./media/devtest-lab-upload-vhd-using-storage-explorer/add-account-link.png
[3]: ./media/devtest-lab-upload-vhd-using-storage-explorer/subscriptions-list.png
[4]: ./media/devtest-lab-upload-vhd-using-storage-explorer/storage-accounts-list.png
[5]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-dir.png
[6]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-button.png
[7]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-files.png
[8]: ./media/devtest-lab-upload-vhd-using-storage-explorer/select-file.png
[9]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-file.png
[10]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-status.png
