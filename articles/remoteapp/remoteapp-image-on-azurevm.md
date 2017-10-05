---
title: "Hozzon létre egy Azure virtuális Gépen alapuló rendszerképet az Azure RemoteApp |} Microsoft Docs"
description: "Lemezkép létrehozása az Azure RemoteApp kezdve az Azure virtuális géphez tudnivalók."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d41583ef-6cd8-4115-8dcb-b2cd5b3d301a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ee64b86835af8e6237cddcd8acc779fc6dac8214
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a><span data-ttu-id="9d3a3-103">Egy Azure RemoteApp-rendszerképet az Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d3a3-103">Create a Azure RemoteApp image based on an Azure virtual machine</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9d3a3-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="9d3a3-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="9d3a3-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="9d3a3-106">Azure RemoteApp-lemezképek (amely a gyűjteményben lévő megosztott alkalmazások tárolására) hozhat létre egy Azure virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-106">You can create Azure RemoteApp images (which hold the apps you share in your collection) from an Azure virtual machine.</span></span> <span data-ttu-id="9d3a3-107">Egy virtuálisgép-lemezkép az Azure RemoteApp kép összes követelménynek megfelelő Azure virtuális gép lemezképének gyűjteményébe hozzáadott használatára is kiválaszthatják,-használhatja, hogy a Virtuálisgép-lemezkép kiindulási pontként a saját virtuális gép, ha azt szeretné.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-107">You could also choose to use a virtual machine image we added to the Azure VM image gallery that meets all the Azure RemoteApp image requirements - you can use that VM image as a starting point for your own VM, if you want.</span></span> <span data-ttu-id="9d3a3-108">Keresse a "Windows Server távoli asztali munkamenetgazda" kép a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-108">Just look for the "Windows Server Remote Desktop Session Host" image in the library.</span></span>

<span data-ttu-id="9d3a3-109">Hozzon létre egy saját egy Azure virtuális Gépen alapuló rendszerképet - lemezkép létrehozásához, és töltse fel azt az Azure virtuális gép könyvtárból Azure RemoteApp két lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-109">There are two steps to create your own image based on an Azure VM - create the image and then upload it from the Azure VM library to Azure RemoteApp.</span></span>

## <a name="create-a-custom-image-based-on-an-azure-vm"></a><span data-ttu-id="9d3a3-110">Egy Azure virtuális Gépen alapuló egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d3a3-110">Create a custom image based on an Azure VM</span></span>
<span data-ttu-id="9d3a3-111">Ezek a lépések segítségével hozzon létre egy Azure virtuális Gépen alapuló rendszerképet.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-111">Use these steps to create an image based on an Azure VM.</span></span>

1. <span data-ttu-id="9d3a3-112">Hozzon létre egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-112">Create an Azure virtual machine.</span></span> <span data-ttu-id="9d3a3-113">Használhatja a "Windows Server távoli asztali munkamenetgazda" vagy a "Windows Server távoli asztali munkamenet állomást a Microsoft Office 365 ProPlus" lemezkép az Azure virtuális gép lemezképének gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-113">You can use the "Windows Server Remote Desktop Session Host" or the "Windows Server Remote Desktop Session Host with Microsoft Office 365 ProPlus" image from the Azure virtual machine image gallery.</span></span> <span data-ttu-id="9d3a3-114">Ez a rendszerkép megfelel az összes Azure RemoteApp sablon kép.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-114">This image meets all the Azure RemoteApp template image requirements.</span></span>
   
    <span data-ttu-id="9d3a3-115">További információkért lásd: [Windows rendszerű virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9d3a3-115">For details, see [Create a VM running Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="9d3a3-116">Csatlakoztassa a virtuális Gépet, és telepítse, és konfigurálja a Remoteappen keresztül megosztani kívánt alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-116">Connect to the VM and install and configure the apps that you want to share through RemoteApp.</span></span> <span data-ttu-id="9d3a3-117">Ellenőrizze, hogy minden további Windows-konfigurációt, az alkalmazások által megkövetelt végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-117">Make sure to perform any additional Windows configurations required by your apps.</span></span>
   
    <span data-ttu-id="9d3a3-118">További információkért lásd: [jelentkezzen be a virtuális gép futó Windows Server hogyan](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9d3a3-118">For details, see [How to Log on to a Virtual Machine Running Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
3. <span data-ttu-id="9d3a3-119">A Windows Server távoli asztali munkamenetgazda lemezképeit egyik használ, nincs-e egy befoglalt érvényesítési parancsfájlt, amely biztosítja a virtuális gép megfelel-e a RemoteApp előtti reqs.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-119">If you are using one of the Windows Server Remote Desktop Session Host images, there is an included validation script that will ensure your VM meets the RemoteApp pre-reqs.</span></span> <span data-ttu-id="9d3a3-120">Parancsfájl futtatásához kattintson duplán a **ValidateRemoteAppImage** az asztalon.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-120">To run script, double-click **ValidateRemoteAppImage** on the desktop.</span></span> <span data-ttu-id="9d3a3-121">Győződjön meg arról, hogy a szkript által jelzett összes hibát a következő lépés a folytatás előtt rögzítettek.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-121">Ensure that all errors reported by the script are fixed before proceeding to the next step.</span></span>
4. <span data-ttu-id="9d3a3-122">A SYSPREP generalize, és a lemezképének rögzítése.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-122">SYSPREP generalize and capture the image.</span></span> <span data-ttu-id="9d3a3-123">Lásd: [sablon Windows virtuális gép rögzítése](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-123">See [How to Capture a Windows Virtual Machine to Use as a Template](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) for instructions.</span></span>

## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a><span data-ttu-id="9d3a3-124">A kép importálása az Azure RemoteApp kép könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-124">Import the image into the Azure RemoteApp image library</span></span>
<span data-ttu-id="9d3a3-125">Az új lemezkép az Azure Remoteappba importálásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9d3a3-125">Use these steps to import the new image into Azure RemoteApp:</span></span>

1. <span data-ttu-id="9d3a3-126">Az a **Sablonrendszerképek** lapon:</span><span class="sxs-lookup"><span data-stu-id="9d3a3-126">In the **Template Images** tab:</span></span>
   
   * <span data-ttu-id="9d3a3-127">Ha nincs meglévő lemezképet, kattintson a **feltölteni, vagy importáljon egy sablon rendszerképet**.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-127">If you have no existing images, click **Upload or Import a Template Image**.</span></span>
   * <span data-ttu-id="9d3a3-128">Ha már legalább egy kép, kattintson a  **+**  hozzáadása egy új lemezképet.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-128">If you have at least one image already, click **+** to add a new image.</span></span>
2. <span data-ttu-id="9d3a3-129">Válassza ki **kép importálása a virtuális gépek** könyvtárban, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-129">Select **Import an image from your Virtual Machines** library, and then click **Next**.</span></span>
3. <span data-ttu-id="9d3a3-130">A következő oldalon válassza ki az egyéni lemezképet a listából, és győződjön meg arról, hogy követték-e a lemezkép létrehozásakor ismertetett lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-130">On the next page, select your custom image from the list and confirm that you followed the steps listed when you created your image.</span></span> <span data-ttu-id="9d3a3-131">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-131">Click **Next**.</span></span>
4. <span data-ttu-id="9d3a3-132">Adjon meg egy nevet az új RemoteApp-lemezkép, és mentse a helyre, majd kattintson a pipa jelre az importálási folyamat elindításához.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-132">Enter a name for the new RemoteApp image and pick the location, then click the checkmark to start the import process.</span></span>

> [!NOTE]
> <span data-ttu-id="9d3a3-133">Lemezképek Azure bárhonnan, Azure virtuális gépek által támogatott bármely Azure RemoteApp által támogatott Azure helyre importálhatja.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-133">You can import images from any Azure location supported by Azure Virtual Machines to any Azure location supported by Azure RemoteApp.</span></span> <span data-ttu-id="9d3a3-134">Attól függően, hogy a helyek az importálás legfeljebb 25 percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-134">Depending on the locations the import can take up to 25 minutes.</span></span>
> 
> 

<span data-ttu-id="9d3a3-135">Most már készen áll az új gyűjtemény, vagy hozzon létre egy [felhő](remoteapp-create-cloud-deployment.md) gyűjtemény vagy [hibrid](remoteapp-create-hybrid-deployment.md), attól függően, az igényeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="9d3a3-135">Now you are ready to create your new collection, either a [cloud](remoteapp-create-cloud-deployment.md) collection or [hybrid](remoteapp-create-hybrid-deployment.md), depending on your needs.</span></span>

