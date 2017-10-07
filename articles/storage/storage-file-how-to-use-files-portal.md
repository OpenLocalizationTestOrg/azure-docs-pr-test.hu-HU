---
title: "aaaHow toomanage hello Azure portál Azure File storage |} Microsoft Docs"
description: "Ismerje meg, hogy toouse hello Azure portál toomanage Azure File storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 385b99ac1c3d97ca79059ae8229afb53f1e825cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-file-storage-from-hello-azure-portal"></a><span data-ttu-id="9b5b3-103">Hogyan toouse Azure File storage hello Azure portál</span><span class="sxs-lookup"><span data-stu-id="9b5b3-103">How toouse Azure File storage from hello Azure Portal</span></span>
<span data-ttu-id="9b5b3-104">Hello [Azure-portálon](https://portal.azure.com) felhasználói felülete lehetőséget nyújt az Azure File storage kezelése.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-104">hello [Azure portal](https://portal.azure.com) provides a user interface for managing Azure File storage.</span></span> <span data-ttu-id="9b5b3-105">Hello követő műveletek webböngészőből végezheti el:</span><span class="sxs-lookup"><span data-stu-id="9b5b3-105">You can perform hello following actions from your web browser:</span></span>

* <span data-ttu-id="9b5b3-106">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b5b3-106">Create a File Share</span></span>
* <span data-ttu-id="9b5b3-107">Töltse fel, és a fájlok tooand letöltése a fájlmegosztásból.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-107">Upload and download files tooand from your file share.</span></span>
* <span data-ttu-id="9b5b3-108">Hello az egyes fájlmegosztások valós használatának figyelése.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-108">Monitor hello actual usage of each file share.</span></span>
* <span data-ttu-id="9b5b3-109">Hello fájl megosztás méretkvótájának módosítása</span><span class="sxs-lookup"><span data-stu-id="9b5b3-109">Adjust hello file share size quota.</span></span>
* <span data-ttu-id="9b5b3-110">Hello csatlakoztatási parancsok toouse toomount a fájlmegosztás másolása a Windows vagy Linux rendszerű ügyfél.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-110">Copy hello mount commands toouse toomount your file share from a Windows or Linux client.</span></span>

## <a name="create-file-share"></a><span data-ttu-id="9b5b3-111">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b5b3-111">Create file share</span></span>
1. <span data-ttu-id="9b5b3-112">Bejelentkezés toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-112">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="9b5b3-113">Hello navigációs menüjében kattintson **tárfiókok** vagy **tárfiókok (klasszikus)**.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-113">On hello navigation menu, click **Storage accounts** or **Storage accounts (classic)**.</span></span>
    
    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. <span data-ttu-id="9b5b3-115">Válassza ki a tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-115">Choose your storage account.</span></span>

    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. <span data-ttu-id="9b5b3-117">Válassza a „Fájlok” szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-117">Choose "Files" service.</span></span>

    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. <span data-ttu-id="9b5b3-119">Kattintson a "Fájlmegosztások", és kövesse az első fájlmegosztás hello hivatkozás toocreate.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-119">Click "File shares" and follow hello link toocreate your first file share.</span></span>

    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. <span data-ttu-id="9b5b3-121">Adja meg hello fájlmegosztás neve és hello fájl (felfelé too5120 GB) megosztás toocreate hello méretét az első fájlmegosztását.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-121">Fill in hello file share name and hello size of hello file share (up too5120 GB) toocreate your first file share.</span></span> <span data-ttu-id="9b5b3-122">Ha hello fájlmegosztás létrejött, bármely fájlrendszerből, amely támogatja az SMB 2.1 vagy SMB 3.0 csatlakoztathatja azt.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-122">Once hello file share has been created, you can mount it from any file system that supports SMB 2.1 or SMB 3.0.</span></span> <span data-ttu-id="9b5b3-123">Sikerült kattintva **kvóta** hello megosztás toochange hello fájlméret hello fájl mentése too5120 GB.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-123">You could click on **Quota** on hello file share toochange hello size of hello file up too5120 GB.</span></span> <span data-ttu-id="9b5b3-124">Tekintse meg a túl[Azure Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/) az Azure File storage tooestimate hello tárolási költsége.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-124">Please refer too[Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) tooestimate hello storage cost of using Azure File storage.</span></span>

    ![Hogyan toocreate fájlmegosztás hello portálon képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a><span data-ttu-id="9b5b3-126">Fájlok fel- és letöltése</span><span class="sxs-lookup"><span data-stu-id="9b5b3-126">Upload and download files</span></span>
1. <span data-ttu-id="9b5b3-127">Válasszon egy már létrehozott fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-127">Choose one file share your have already created.</span></span>

    ![Hogyan tooupload és a letöltési fájlok hello portal képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. <span data-ttu-id="9b5b3-129">Kattintson a **feltöltése** a fájlok feltöltésére hello felhasználói felületének megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-129">Click **Upload** to open hello user interface for files uploading.</span></span>

    ![Hogyan tooupload hello portal fájljainak képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-toofile-share"></a><span data-ttu-id="9b5b3-131">Csatlakozás toofile megosztás</span><span class="sxs-lookup"><span data-stu-id="9b5b3-131">Connect toofile share</span></span>
-  <span data-ttu-id="9b5b3-132">Kattintson a **Connect** csatlakoztatását a hello fájlmegosztás hello parancssori lekérése a Windows és Linux.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-132">Click **Connect** to get hello command line for mounting hello file share from Windows and Linux.</span></span> <span data-ttu-id="9b5b3-133">A Linux-felhasználók számára, olvassa el a is túl[hogyan toouse Linux Azure File storage](storage-how-to-use-files-linux.md) más Linux terjesztésekről további csatlakoztatását a útmutatót.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-133">For Linux users, you can also refer too[How toouse Azure File storage with Linux](storage-how-to-use-files-linux.md) for more mounting instructions for other Linux distributions.</span></span>

    ![Hogyan toomount hello fájlmegosztás képernyőkép](media/storage-file-how-to-use-files-portal/use-files-portal-connect.png)
-  <span data-ttu-id="9b5b3-135">Hello fájl elhelyezésére szolgáló parancsok Windows vagy Linux megosztáshoz, majd futtassa az Ön Azure virtuális gép vagy a helyi gép másolhatja.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-135">You can copy hello commands for mounting file share on Windows or Linux and run it from you Azure VM or on-premises machine.</span></span>

    ![Képernyőkép a hello csatlakoztatási parancsok Windows és Linux rendszerekhez](media/storage-file-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

<span data-ttu-id="9b5b3-137">**Tipp:**</span><span class="sxs-lookup"><span data-stu-id="9b5b3-137">**Tip:**</span></span>  
<span data-ttu-id="9b5b3-138">toofind hello tárfiók_elérési_kulcsa csatlakoztatáshoz, kattintson a **nézet elérési kulcsainak a tárfiókhoz** hello hello alján csatlakozzon a lap.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-138">toofind hello storage account access key for mounting, click on **View access keys for this storage account** at hello bottom of hello connect page.</span></span>

## <a name="see-also"></a><span data-ttu-id="9b5b3-139">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="9b5b3-139">See also</span></span>
<span data-ttu-id="9b5b3-140">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="9b5b3-140">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="9b5b3-141">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="9b5b3-141">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="9b5b3-142">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="9b5b3-142">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)