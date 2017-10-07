---
title: "az Azure file storage egy Windows Azure virtuális gépről aaaMount |} Microsoft Docs"
description: "Az Azure file storage szolgáltatással hello felhőben tárolja a fájlt, és a felhőalapú fájlmegosztást csatlakoztathatja egy Azure virtuális gép (VM)."
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 965f1c1b3f0d07fec6d86f9312a05e02e8ce7fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a><span data-ttu-id="dd5e5-103">Azure fájlmegosztásokat Windows virtuális gépek használata</span><span class="sxs-lookup"><span data-stu-id="dd5e5-103">Use Azure file shares with Windows VMs</span></span> 

<span data-ttu-id="dd5e5-104">Is használhatja az Azure fájlmegosztások egy módon toostore és hozzáférni a fájlokhoz a virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-104">You can use Azure file shares as a way toostore and access files from your VM.</span></span> <span data-ttu-id="dd5e5-105">Tárolhatja például a parancsfájl vagy az, hogy szeretné-e a virtuális gépek tooshare konfigurációs fájlját.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-105">For example, you can store a script or an application configuration file that you want all your VMs tooshare.</span></span> <span data-ttu-id="dd5e5-106">Ebben a témakörben megmutatjuk, hogyan toocreate és a csatlakoztatási egy Azure-fájlmegosztás, és hogyan tooupload és a letöltési fájlokat.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-106">In this topic, we show you how toocreate and mount an Azure file share, and how tooupload and download files.</span></span>

## <a name="connect-tooa-file-share-from-a-vm"></a><span data-ttu-id="dd5e5-107">Tooa fájlmegosztás csatlakoztatja a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="dd5e5-107">Connect tooa file share from a VM</span></span>

<span data-ttu-id="dd5e5-108">Ez a szakasz azt feltételezi, hogy már rendelkezik, amelyet a tooconnect fájlmegosztással.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-108">This section assumes you already have a file share that you want tooconnect to.</span></span> <span data-ttu-id="dd5e5-109">Ha egy toocreate van szüksége, tekintse meg [fájlmegosztás létrehozása](#create-a-file-share) a témakör későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-109">If you need toocreate one, see [Create a file share](#create-a-file-share) later in this topic.</span></span>

1. <span data-ttu-id="dd5e5-110">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dd5e5-110">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="dd5e5-111">A hello bal oldali menüben kattintson **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-111">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="dd5e5-112">Válassza ki a tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-112">Choose your storage account.</span></span>
4. <span data-ttu-id="dd5e5-113">A hello **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-113">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="dd5e5-114">Válasszon egy fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-114">Select a file share.</span></span>
6. <span data-ttu-id="dd5e5-115">Kattintson a **Connect** tooopen egy oldal, amely hello parancssori szintaxist hello fájlmegosztás csatlakoztatása a Windows vagy Linux jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-115">Click **Connect** tooopen a page that shows hello command-line syntax for mounting hello file share from Windows or Linux.</span></span>
7. <span data-ttu-id="dd5e5-116">Jelöljön ki hello hello parancs szintaxisát, és illessze be a Jegyzettömb vagy valahol máshol ahol könnyedén elérheti azt.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-116">Highlight hello syntax of hello command and paste it into Notepad or someplace else where you can easily access it.</span></span> 
8. <span data-ttu-id="dd5e5-117">Hello szintaxis tooremove hello bevezető szerkesztése ** > **, és cserélje le *[meghajtó_betűjele]* hello meghajtóbetűjellel (például **y**) toomount hello fájlmegosztás helyének.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-117">Edit hello syntax tooremove hello leading **> ** and replace *[drive letter]* with hello drive letter (for example, **Y:**) where you would like toomount hello file share.</span></span>
8. <span data-ttu-id="dd5e5-118">Csatlakozás a virtuális gép tooyour, és nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-118">Connect tooyour VM and open a command prompt.</span></span>
9. <span data-ttu-id="dd5e5-119">Illessze be hello kapcsolat szintaxis szerkeszteni, majd nyomja le **Enter**.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-119">Paste in hello edited connection syntax and hit **Enter**.</span></span>
10. <span data-ttu-id="dd5e5-120">Ha hello kapcsolat létrejött, a hibaüzenet hello **hello parancs sikeresen befejeződött.**</span><span class="sxs-lookup"><span data-stu-id="dd5e5-120">When hello connection has been created, you get hello message **hello command completed successfully.**</span></span>
11. <span data-ttu-id="dd5e5-121">Ellenőrizze a hello kapcsolatot írja be a hello meghajtó betűjele tooswitch toothat meghajtó, és írja be **dir** hello fájlmegosztás toosee hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-121">Check hello connection by typing in hello drive letter tooswitch toothat drive and then type **dir** toosee hello contents of hello file share.</span></span>



## <a name="create-a-file-share"></a><span data-ttu-id="dd5e5-122">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="dd5e5-122">Create a file share</span></span> 
1. <span data-ttu-id="dd5e5-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dd5e5-123">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="dd5e5-124">A hello bal oldali menüben kattintson **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-124">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="dd5e5-125">Válassza ki a tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-125">Choose your storage account.</span></span>
4. <span data-ttu-id="dd5e5-126">A hello **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-126">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="dd5e5-127">Hello Fájlszolgáltatás oldalon kattintson **+ fájlmegosztás** az első fájlmegosztás toocreate. \\</span><span class="sxs-lookup"><span data-stu-id="dd5e5-127">In hello File Service page, click **+ File share** toocreate your first file share.\\</span></span>
6. <span data-ttu-id="dd5e5-128">Töltse ki hello fájlmegosztás neve.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-128">Fill in hello file share name.</span></span> <span data-ttu-id="dd5e5-129">A fájlmegosztás neve kisbetűket, számokat és kötőjeleket egyetlen használhatja.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-129">File share names can use lowercase letters, numbers and single hyphens.</span></span> <span data-ttu-id="dd5e5-130">hello neve nem kezdődhet kötőjellel, és nem használható több egymást követő kötőjelet.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-130">hello name cannot start with a hyphen and you can't use multiple, consecutive hyphens.</span></span> 
7. <span data-ttu-id="dd5e5-131">Adja meg a maximális méretét hello fájl mentése too5120 GB lehet.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-131">Fill in a limit on how large hello file can be, up too5120 GB.</span></span>
8. <span data-ttu-id="dd5e5-132">Kattintson a **OK** toodeploy hello fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-132">Click **OK** toodeploy hello file share.</span></span>
   
## <a name="upload-files"></a><span data-ttu-id="dd5e5-133">Fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="dd5e5-133">Upload files</span></span>
1. <span data-ttu-id="dd5e5-134">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dd5e5-134">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="dd5e5-135">A hello bal oldali menüben kattintson **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-135">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="dd5e5-136">Válassza ki a tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-136">Choose your storage account.</span></span>
4. <span data-ttu-id="dd5e5-137">A hello **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-137">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="dd5e5-138">Válasszon egy fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-138">Select a file share.</span></span>
6. <span data-ttu-id="dd5e5-139">Kattintson a **feltöltése** tooopen hello **fájlok feltöltése** lap.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-139">Click **Upload** tooopen hello **Upload files** page.</span></span>
7. <span data-ttu-id="dd5e5-140">Kattintson a hello mappa ikon toobrowse egy fájl tooupload a helyi fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-140">Click on hello folder icon toobrowse your local file system for a file tooupload.</span></span>   
8. <span data-ttu-id="dd5e5-141">Kattintson a **feltöltése** tooupload hello fájl toohello fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-141">Click **Upload** tooupload hello file toohello file share.</span></span>

## <a name="download-files"></a><span data-ttu-id="dd5e5-142">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="dd5e5-142">Download files</span></span>
1. <span data-ttu-id="dd5e5-143">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dd5e5-143">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="dd5e5-144">A hello bal oldali menüben kattintson **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-144">On hello left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="dd5e5-145">Válassza ki a tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-145">Choose your storage account.</span></span>
4. <span data-ttu-id="dd5e5-146">A hello **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-146">In hello **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="dd5e5-147">Válasszon egy fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-147">Select a file share.</span></span>
6. <span data-ttu-id="dd5e5-148">Kattintson a jobb gombbal a hello fájlt, és válassza a **letöltése** toodownload azt tooyour helyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-148">Right-click on hello file and choose **Download** toodownload it tooyour local machine.</span></span>
   

## <a name="next-steps"></a><span data-ttu-id="dd5e5-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dd5e5-149">Next steps</span></span>

<span data-ttu-id="dd5e5-150">Is létrehozása és kezelése a PowerShell használatával fájlmegosztások.</span><span class="sxs-lookup"><span data-stu-id="dd5e5-150">You can also create and manage file shares using PowerShell.</span></span> <span data-ttu-id="dd5e5-151">További információkért lásd: [Ismerkedés az Azure File storage on Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="dd5e5-151">For more information, see [Get started with Azure File storage on Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span></span>
