---
title: "Egy Windows Azure VM csatlakoztatási Azure file storage |} Microsoft Docs"
description: "Fájl tárolása a felhőben az Azure file storage szolgáltatással, és a felhőalapú fájlmegosztást csatlakoztathatja egy Azure virtuális gép (VM)."
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
ms.openlocfilehash: 6ffb2d2da1e2439df6f5da543411e3c2c68d3435
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a><span data-ttu-id="ecc31-103">Azure fájlmegosztásokat Windows virtuális gépek használata</span><span class="sxs-lookup"><span data-stu-id="ecc31-103">Use Azure file shares with Windows VMs</span></span> 

<span data-ttu-id="ecc31-104">Azure fájlmegosztásokat is tárolhatja és érheti fájlokat a virtuális Gépet is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ecc31-104">You can use Azure file shares as a way to store and access files from your VM.</span></span> <span data-ttu-id="ecc31-105">Például egy parancsfájl vagy tárolhatja a virtuális gépeinek megosztani kívánt alkalmazáskonfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="ecc31-105">For example, you can store a script or an application configuration file that you want all your VMs to share.</span></span> <span data-ttu-id="ecc31-106">Ebben a témakörben megmutatjuk, hogyan hozhat létre, és az Azure fájlmegosztások csatlakoztatása, és hogyan fájlok feltöltését és letöltését.</span><span class="sxs-lookup"><span data-stu-id="ecc31-106">In this topic, we show you how to create and mount an Azure file share, and how to upload and download files.</span></span>

## <a name="connect-to-a-file-share-from-a-vm"></a><span data-ttu-id="ecc31-107">Csatlakoztatja a fájlmegosztást a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="ecc31-107">Connect to a file share from a VM</span></span>

<span data-ttu-id="ecc31-108">Ez a szakasz azt feltételezi, hogy már rendelkezik egy fájlmegosztást, amelyhez csatlakozni kíván.</span><span class="sxs-lookup"><span data-stu-id="ecc31-108">This section assumes you already have a file share that you want to connect to.</span></span> <span data-ttu-id="ecc31-109">Ha szeretne létrehozni egyet szüksége, tekintse meg [fájlmegosztás létrehozása](#create-a-file-share) a témakör későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="ecc31-109">If you need to create one, see [Create a file share](#create-a-file-share) later in this topic.</span></span>

1. <span data-ttu-id="ecc31-110">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ecc31-110">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ecc31-111">Kattintson a bal oldali menü **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="ecc31-111">On the left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="ecc31-112">Válassza ki a tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="ecc31-112">Choose your storage account.</span></span>
4. <span data-ttu-id="ecc31-113">Az a **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="ecc31-113">In the **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="ecc31-114">Válasszon egy fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="ecc31-114">Select a file share.</span></span>
6. <span data-ttu-id="ecc31-115">Kattintson a **Connect** megnyit egy olyan lapot, hogy a fájlmegosztás Windows vagy Linux csatlakoztatására parancssori szintaxisát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ecc31-115">Click **Connect** to open a page that shows the command-line syntax for mounting the file share from Windows or Linux.</span></span>
7. <span data-ttu-id="ecc31-116">Jelölje ki a parancs szintaxisát, és illessze be a Jegyzettömb vagy valahol máshol ahol könnyedén elérheti azt.</span><span class="sxs-lookup"><span data-stu-id="ecc31-116">Highlight the syntax of the command and paste it into Notepad or someplace else where you can easily access it.</span></span> 
8. <span data-ttu-id="ecc31-117">Szerkessze a szintaxisát, és távolítsa el a bevezető ** > **, és cserélje le *[meghajtó_betűjele]* a meghajtó betűjelével (például **y**) helyének a fájlmegosztás csatlakoztatásához.</span><span class="sxs-lookup"><span data-stu-id="ecc31-117">Edit the syntax to remove the leading **> ** and replace *[drive letter]* with the drive letter (for example, **Y:**) where you would like to mount the file share.</span></span>
8. <span data-ttu-id="ecc31-118">Csatlakoztassa a virtuális Gépet, és nyisson meg egy parancssort.</span><span class="sxs-lookup"><span data-stu-id="ecc31-118">Connect to your VM and open a command prompt.</span></span>
9. <span data-ttu-id="ecc31-119">Illessze be a szerkesztett kapcsolat szintaxist, majd nyomja le **Enter**.</span><span class="sxs-lookup"><span data-stu-id="ecc31-119">Paste in the edited connection syntax and hit **Enter**.</span></span>
10. <span data-ttu-id="ecc31-120">Ha a kapcsolat létrejött, a hibaüzenet a **a parancs sikeresen befejeződött.**</span><span class="sxs-lookup"><span data-stu-id="ecc31-120">When the connection has been created, you get the message **The command completed successfully.**</span></span>
11. <span data-ttu-id="ecc31-121">Ellenőrizze a kapcsolatot, írja be a meghajtóbetűjelet, amelynek váltson, és írja be **dir** a fájlmegosztás tartalmának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="ecc31-121">Check the connection by typing in the drive letter to switch to that drive and then type **dir** to see the contents of the file share.</span></span>



## <a name="create-a-file-share"></a><span data-ttu-id="ecc31-122">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ecc31-122">Create a file share</span></span> 
1. <span data-ttu-id="ecc31-123">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ecc31-123">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ecc31-124">Kattintson a bal oldali menü **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="ecc31-124">On the left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="ecc31-125">Válassza ki a tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="ecc31-125">Choose your storage account.</span></span>
4. <span data-ttu-id="ecc31-126">Az a **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="ecc31-126">In the **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="ecc31-127">A fájl szolgáltatás lapján, kattintson a **+ fájlmegosztás** létrehozása az első fájlmegosztását. \\</span><span class="sxs-lookup"><span data-stu-id="ecc31-127">In the File Service page, click **+ File share** to create your first file share.\\</span></span>
6. <span data-ttu-id="ecc31-128">Adja meg a fájlmegosztás neve.</span><span class="sxs-lookup"><span data-stu-id="ecc31-128">Fill in the file share name.</span></span> <span data-ttu-id="ecc31-129">A fájlmegosztás neve kisbetűket, számokat és kötőjeleket egyetlen használhatja.</span><span class="sxs-lookup"><span data-stu-id="ecc31-129">File share names can use lowercase letters, numbers and single hyphens.</span></span> <span data-ttu-id="ecc31-130">A név nem kezdődhet kötőjellel, és nem használható több egymást követő kötőjelet.</span><span class="sxs-lookup"><span data-stu-id="ecc31-130">The name cannot start with a hyphen and you can't use multiple, consecutive hyphens.</span></span> 
7. <span data-ttu-id="ecc31-131">Töltse ki hogyan nagy a fájl lehet, legfeljebb 5120 GB korlátozást.</span><span class="sxs-lookup"><span data-stu-id="ecc31-131">Fill in a limit on how large the file can be, up to 5120 GB.</span></span>
8. <span data-ttu-id="ecc31-132">Kattintson a **OK** központi telepítése a fájlmegosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="ecc31-132">Click **OK** to deploy the file share.</span></span>
   
## <a name="upload-files"></a><span data-ttu-id="ecc31-133">Fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="ecc31-133">Upload files</span></span>
1. <span data-ttu-id="ecc31-134">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ecc31-134">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ecc31-135">Kattintson a bal oldali menü **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="ecc31-135">On the left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="ecc31-136">Válassza ki a tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="ecc31-136">Choose your storage account.</span></span>
4. <span data-ttu-id="ecc31-137">Az a **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="ecc31-137">In the **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="ecc31-138">Válasszon egy fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="ecc31-138">Select a file share.</span></span>
6. <span data-ttu-id="ecc31-139">Kattintson a **feltöltése** megnyitásához a **fájlok feltöltése** lap.</span><span class="sxs-lookup"><span data-stu-id="ecc31-139">Click **Upload** to open the **Upload files** page.</span></span>
7. <span data-ttu-id="ecc31-140">Kattintson a feltöltendő fájl a helyi fájlrendszerben tallózással mappa ikonra.</span><span class="sxs-lookup"><span data-stu-id="ecc31-140">Click on the folder icon to browse your local file system for a file to upload.</span></span>   
8. <span data-ttu-id="ecc31-141">Kattintson a **feltöltése** feltölteni a fájlt a fájlmegosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="ecc31-141">Click **Upload** to upload the file to the file share.</span></span>

## <a name="download-files"></a><span data-ttu-id="ecc31-142">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="ecc31-142">Download files</span></span>
1. <span data-ttu-id="ecc31-143">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ecc31-143">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ecc31-144">Kattintson a bal oldali menü **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="ecc31-144">On the left menu, click **Storage accounts**.</span></span>
3. <span data-ttu-id="ecc31-145">Válassza ki a tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="ecc31-145">Choose your storage account.</span></span>
4. <span data-ttu-id="ecc31-146">Az a **áttekintése** lap **szolgáltatások**, jelölje be **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="ecc31-146">In the **Overview** page, under **Services**, select **Files**.</span></span>
5. <span data-ttu-id="ecc31-147">Válasszon egy fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="ecc31-147">Select a file share.</span></span>
6. <span data-ttu-id="ecc31-148">Kattintson a jobb gombbal a fájlra, és válassza a **letöltése** letöltheti a helyi számítógépre.</span><span class="sxs-lookup"><span data-stu-id="ecc31-148">Right-click on the file and choose **Download** to download it to your local machine.</span></span>
   

## <a name="next-steps"></a><span data-ttu-id="ecc31-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecc31-149">Next steps</span></span>

<span data-ttu-id="ecc31-150">Is létrehozása és kezelése a PowerShell használatával fájlmegosztások.</span><span class="sxs-lookup"><span data-stu-id="ecc31-150">You can also create and manage file shares using PowerShell.</span></span> <span data-ttu-id="ecc31-151">További információkért lásd: [Ismerkedés az Azure File storage on Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="ecc31-151">For more information, see [Get started with Azure File storage on Windows](../../storage/files/storage-dotnet-how-to-use-files.md).</span></span>
