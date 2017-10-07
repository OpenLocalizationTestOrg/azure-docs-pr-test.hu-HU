---
title: "virtuális merevlemez aaaUpload fájl tooAzure DevTest Labs segítségével a Microsoft Azure Tártallózó |} Microsoft Docs"
description: "Töltse fel a Microsoft Azure Tártallózó használó virtuális merevlemez fájl toolab tárfiókot"
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
ms.openlocfilehash: 686691e3676cea4b2d7cd8bf045bc43a792c667e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-microsoft-azure-storage-explorer"></a><span data-ttu-id="43ed6-103">Töltse fel a Microsoft Azure Tártallózó használó virtuális merevlemez fájl toolab tárfiókot</span><span class="sxs-lookup"><span data-stu-id="43ed6-103">Upload VHD file toolab's storage account using Microsoft Azure Storage Explorer</span></span>

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

<span data-ttu-id="43ed6-104">Az Azure DevTest Labs szolgáltatásban a VHD-fájlokat lehet használt toocreate egyéni képek, amelyek használt tooprovision virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="43ed6-104">In Azure DevTest Labs, VHD files can be used toocreate custom images, which are used tooprovision virtual machines.</span></span> <span data-ttu-id="43ed6-105">Ez a cikk bemutatja, hogyan toouse [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooupload egy VHD-fájl tooa tesztkörnyezet tárfiókja.</span><span class="sxs-lookup"><span data-stu-id="43ed6-105">This article illustrates how toouse [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooupload a VHD file tooa lab's storage account.</span></span> <span data-ttu-id="43ed6-106">A VHD-fájl feltöltése után hello [további lépések szakaszt](#next-steps) felsorolja az egyes cikkeket, amelyek bemutatják, hogyan toocreate hello egy egyéni lemezkép feltöltése a VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="43ed6-106">Once you've uploaded your VHD file, hello [Next steps section](#next-steps) lists some articles that illustrate how toocreate a custom image from hello uploaded VHD file.</span></span> <span data-ttu-id="43ed6-107">További információ a lemezek és a VHD-ken az Azure-ban: [lemezek és a virtuális merevlemezek a virtuális gépek](../virtual-machines/linux/about-disks-and-vhds.md)</span><span class="sxs-lookup"><span data-stu-id="43ed6-107">For more information about disks and VHDs in Azure, see [About disks and VHDs for virtual machines](../virtual-machines/linux/about-disks-and-vhds.md)</span></span>

## <a name="step-by-step-instructions"></a><span data-ttu-id="43ed6-108">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="43ed6-108">Step-by-step instructions</span></span>

<span data-ttu-id="43ed6-109">következő lépések bejárása le a virtuális merevlemez feltöltése fájl használatával tooDevTest Labs hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="43ed6-109">hello following steps walk you through uploading a VHD file tooDevTest Labs using [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>

1. <span data-ttu-id="43ed6-110">[Töltse le és telepítse a Microsoft Azure Tártallózó hello legújabb verziójának hello](http://www.storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="43ed6-110">[Download and install hello latest version of hello Microsoft Azure Storage Explorer](http://www.storageexplorer.com).</span></span>

1. <span data-ttu-id="43ed6-111">Hello Azure-portál használatával hello tesztkörnyezet tárfiókja nevére hello beolvasása:</span><span class="sxs-lookup"><span data-stu-id="43ed6-111">Get hello name of hello lab's storage account using hello Azure portal:</span></span>

    1. <span data-ttu-id="43ed6-112">Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="43ed6-112">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
    
    1. <span data-ttu-id="43ed6-113">Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.</span><span class="sxs-lookup"><span data-stu-id="43ed6-113">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>
    
    1. <span data-ttu-id="43ed6-114">Labs hello listában jelölje ki hello kívánt labor.</span><span class="sxs-lookup"><span data-stu-id="43ed6-114">From hello list of labs, select hello desired lab.</span></span>  
    
    1. <span data-ttu-id="43ed6-115">Hello labor paneljén válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-115">On hello lab's blade, select **Configuration**.</span></span> 
    
    1. <span data-ttu-id="43ed6-116">A tesztkörnyezet hello **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-116">On hello lab **Configuration** blade, select **Custom images (VHDs)**.</span></span>
    
    1. <span data-ttu-id="43ed6-117">A hello **egyéni lemezképek** panelen válasszon ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-117">On hello **Custom images** blade, Select **+Add**.</span></span> 
    
    1. <span data-ttu-id="43ed6-118">A hello **egyéni lemezkép** panelen válassza **VHD**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-118">On hello **Custom image** blade, select **VHD**.</span></span>
    
    1. <span data-ttu-id="43ed6-119">A hello **VHD** panelen válassza **a PowerShell használatával virtuális merevlemez feltöltéséhez**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-119">On hello **VHD** blade, select **Upload a VHD using PowerShell**.</span></span>
    
        ![Töltse fel a virtuális merevlemez a PowerShell használatával][0]
    
    1. <span data-ttu-id="43ed6-121">Hello **PowerShell-lel lemezkép feltöltése a** panelt jeleníti meg a hívás toohello **Add-AzureVhd** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="43ed6-121">hello **Upload an image using PowerShell** blade displays a call toohello **Add-AzureVhd** cmdlet.</span></span> <span data-ttu-id="43ed6-122">az első paraméter hello (*cél*) hello a tárfióknév hello labor hello a következő formátumban tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="43ed6-122">hello first parameter (*Destination*) contains hello storage account name for hello lab in hello following format:</span></span>
    
        <span data-ttu-id="43ed6-123">https://<STORAGE-ACCOUNT-Name>.BLOB.Core.Windows.NET/uploads/...</span><span class="sxs-lookup"><span data-stu-id="43ed6-123">https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...</span></span> 

    1. <span data-ttu-id="43ed6-124">Jegyezze fel a hello tárfiók neve, a későbbi lépésekben használatban van.</span><span class="sxs-lookup"><span data-stu-id="43ed6-124">Make note of hello storage account name as it is used in later steps.</span></span>
    
1. <span data-ttu-id="43ed6-125">Csatlakozás tooan Tártallózó Azure-előfizetés fiókot.</span><span class="sxs-lookup"><span data-stu-id="43ed6-125">Connect tooan Azure subscription account using Storage Explorer.</span></span>

    > [!TIP] 
    > 
    > <span data-ttu-id="43ed6-126">A Tártallózó kapcsolati beállításokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="43ed6-126">Storage Explorer supports several connection options.</span></span> <span data-ttu-id="43ed6-127">Ez a rész az Azure-előfizetéshez társított csatlakozó tooa tárfiók mutat.</span><span class="sxs-lookup"><span data-stu-id="43ed6-127">This section illustrates connecting tooa storage account associated with your Azure subscription.</span></span> <span data-ttu-id="43ed6-128">toosee hello egyéb Tártallózó által támogatott kapcsolódási beállítások, tekintse meg a toohello cikk [Ismerkedés a Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="43ed6-128">toosee hello other connection options supported by Storage Explorer, refer toohello article, [Getting started with Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>
 
    1. <span data-ttu-id="43ed6-129">Nyissa meg a Tártallózót.</span><span class="sxs-lookup"><span data-stu-id="43ed6-129">Open Storage Explorer.</span></span>
    
    1. <span data-ttu-id="43ed6-130">A Tártallózó, válassza ki a **Azure Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-130">In Storage Explorer, select **Azure Account settings**.</span></span> 
    
        ![Azure-fiók beállításai][1]
    
    1. <span data-ttu-id="43ed6-132">hello bal oldali ablaktáblán hello Microsoft-fiókkal bejelentkezett azt jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="43ed6-132">hello left pane displays hello Microsoft accounts you've logged in to.</span></span> <span data-ttu-id="43ed6-133">tooconnect tooanother fiókját, válassza **vegyen fel egy fiókot**, és hajtsa végre a hello párbeszédpanelek toosign be legalább egy aktív Azure-előfizetéssel társított Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="43ed6-133">tooconnect tooanother account, select **Add an account**, and follow hello dialogs toosign in with a Microsoft account that is associated with at least one active Azure subscription.</span></span>
    
        ![Fiók hozzáadása][2]
    
    1. <span data-ttu-id="43ed6-135">Miután sikeresen bejelentkezett egy Microsoft-fiókkal, hello bal oldali ablaktáblán tölti fel hello fiókhoz társított Azure-előfizetések.</span><span class="sxs-lookup"><span data-stu-id="43ed6-135">Once you successfully sign in with a Microsoft account, hello left pane populates with hello Azure subscriptions associated with that account.</span></span> <span data-ttu-id="43ed6-136">Válassza ki az Azure-előfizetést, amelyhez szeretné, hogy toowork, és válassza hello **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-136">Select hello Azure subscriptions with which you want toowork, and then select **Apply**.</span></span> <span data-ttu-id="43ed6-137">(Kiválasztásával **előfizetéseket** megjelenítése és elrejtése az összes kijelölt hello, vagy nincs hello felsorolt Azure-előfizetések.)</span><span class="sxs-lookup"><span data-stu-id="43ed6-137">(Selecting **All subscriptions** toggles hello selection of all or none of hello listed Azure subscriptions.)</span></span>
    
        ![Azure-előfizetések kiválasztása][3]
    
    1. <span data-ttu-id="43ed6-139">hello bal oldali panelen kiválasztott hello Azure-előfizetéssel társított tárfiókokat hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="43ed6-139">hello left pane displays hello storage accounts associated with hello selected Azure subscriptions.</span></span>
    
        ![Kiválasztott Azure-előfizetések][4]

1. <span data-ttu-id="43ed6-141">Keresse meg a hello tesztkörnyezet tárfiókja:</span><span class="sxs-lookup"><span data-stu-id="43ed6-141">Locate hello lab's storage account:</span></span>

    1. <span data-ttu-id="43ed6-142">Hello Tártallózó bal oldali ablaktáblán keresse meg, és bontsa ki az Azure-előfizetés hello labor birtokló hello hello csomópontot.</span><span class="sxs-lookup"><span data-stu-id="43ed6-142">In hello Storage Explorer left pane, locate, and expand hello node for hello Azure subscription that owns hello lab.</span></span>
    
    1. <span data-ttu-id="43ed6-143">Hello előfizetési csomópontot, bontsa ki a **Tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-143">Under hello subscription's node, expand **Storage Accounts**.</span></span>

    1. <span data-ttu-id="43ed6-144">Bontsa ki a hello labor tárolási fiók csomópont tooreveal csomópontok a **Blobtárolók**, **fájlmegosztások**, **várólisták**, és **táblák**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-144">Expand hello lab's storage account node tooreveal nodes for **Blob Containers**, **File Shares**, **Queues**, and **Tables**.</span></span>
    
    1. <span data-ttu-id="43ed6-145">Bontsa ki a hello **Blobtárolók** csomópont.</span><span class="sxs-lookup"><span data-stu-id="43ed6-145">Expand hello **Blob Containers** node.</span></span>
    
    1. <span data-ttu-id="43ed6-146">Válasszon hello feltöltések blob tároló toodisplay tartalmának hello jobb oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="43ed6-146">Select hello uploads blob container toodisplay its contents in hello right pane.</span></span>
        
        ![Directory feltöltése][5]

1. <span data-ttu-id="43ed6-148">A Tártallózóval hello VHD-fájl feltöltése:</span><span class="sxs-lookup"><span data-stu-id="43ed6-148">Upload hello VHD file using Storage Explorer:</span></span>

    1. <span data-ttu-id="43ed6-149">Hello Tártallózó jobb oldali ablaktáblában kell megjelennie a hello hello BLOB listáját **feltölt** hello tesztkörnyezet tárfiókja blob tároló.</span><span class="sxs-lookup"><span data-stu-id="43ed6-149">In hello Storage Explorer right pane, you should see a listing of hello blobs in hello **uploads** blob container of hello lab's storage account.</span></span> <span data-ttu-id="43ed6-150">Válassza hello blob szerkesztő eszköztár **feltöltése**</span><span class="sxs-lookup"><span data-stu-id="43ed6-150">On hello blob editor toolbar, select **Upload**</span></span> 
        
        ![Töltse fel gomb][6]
    
    1. <span data-ttu-id="43ed6-152">A hello **feltöltése** legördülő menüben válassza **fájlok feltöltése...** .</span><span class="sxs-lookup"><span data-stu-id="43ed6-152">From hello **Upload** drop-down menu, select **Upload files...**.</span></span>
    
    1. <span data-ttu-id="43ed6-153">A hello **fájlok feltöltése** párbeszédpanelen jelölje be hello három ponttal.</span><span class="sxs-lookup"><span data-stu-id="43ed6-153">On hello **Upload files** dialog, select hello ellipsis.</span></span>
        
        ![Fájl kiválasztása][8]  

    1. <span data-ttu-id="43ed6-155">A hello **válassza fájlok tooupload** párbeszédpanelen Tallózás toohello VHD-fájl szükséges, válassza ki azt, és válassza **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-155">On hello **Select files tooupload** dialog, browse toohello desired VHD file, select it, and then select **Open**.</span></span>
    
    1. <span data-ttu-id="43ed6-156">Ha a visszaadott toohello **fájlok feltöltése** párbeszédpanelen módosítsa **Blob-típusú** túl**oldalakra vonatkozó Blob**.</span><span class="sxs-lookup"><span data-stu-id="43ed6-156">When returned toohello **Upload files** dialog, change **Blob type** too**Page Blob**.</span></span>
    
    1. <span data-ttu-id="43ed6-157">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="43ed6-157">Select **Upload**.</span></span>

        ![Fájl kiválasztása][9]  
    
    1. <span data-ttu-id="43ed6-159">a Tártallózó hello **tevékenységnapló** ablaktábla megjeleníti azokat a hello letöltés állapota (valamint hivatkozások toocancel hello feltöltés).</span><span class="sxs-lookup"><span data-stu-id="43ed6-159">hello Storage Explorer **Activity Log** pane shows hello download status (along with links toocancel hello upload).</span></span> <span data-ttu-id="43ed6-160">a VHD-fájl feltöltése hello folyamat megnőhet hello hello VHD-fájl méretétől és a kapcsolat sebességétől függően.</span><span class="sxs-lookup"><span data-stu-id="43ed6-160">hello process of uploading a VHD file can be lengthy depending on hello size of hello VHD file and your connection speed.</span></span> 

        ![Fájlfeltöltés állapot][10]  

## <a name="next-steps"></a><span data-ttu-id="43ed6-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43ed6-162">Next steps</span></span>

- [<span data-ttu-id="43ed6-163">Hozzon létre egy egyéni lemezképet egy VHD-fájlt hello Azure-portálon az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="43ed6-163">Create a custom image in Azure DevTest Labs from a VHD file using hello Azure portal</span></span>](devtest-lab-create-template.md)
- [<span data-ttu-id="43ed6-164">Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="43ed6-164">Create a custom image in Azure DevTest Labs from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

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
