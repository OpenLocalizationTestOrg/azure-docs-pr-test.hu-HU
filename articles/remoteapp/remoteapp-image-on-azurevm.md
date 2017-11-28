---
title: "Azure RemoteApp-lemezkép az Azure virtuális gép alapján aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate kezdve az Azure virtuális gépként az Azure RemoteApp lemezkép."
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
ms.openlocfilehash: 2d432bcb15be68a2946d91b5f36f41d980726338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a><span data-ttu-id="94f18-103">Egy Azure RemoteApp-rendszerképet az Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="94f18-103">Create a Azure RemoteApp image based on an Azure virtual machine</span></span>
> [!IMPORTANT]
> <span data-ttu-id="94f18-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="94f18-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="94f18-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="94f18-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="94f18-106">Azure RemoteApp-lemezképek (amely a gyűjteményben lévő megosztott hello alkalmazások tárolására) hozhat létre egy Azure virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="94f18-106">You can create Azure RemoteApp images (which hold hello apps you share in your collection) from an Azure virtual machine.</span></span> <span data-ttu-id="94f18-107">Akkor is kiválaszthatják toouse hozzáadott toohello Azure virtuális gép kép gyűjtemény összes hello Azure RemoteApp kép vonatkozó követelményeknek megfelelő virtuálisgép-lemezkép - használhatja, hogy a Virtuálisgép-lemezkép kiindulási pontként a saját virtuális gép, ha azt szeretné.</span><span class="sxs-lookup"><span data-stu-id="94f18-107">You could also choose toouse a virtual machine image we added toohello Azure VM image gallery that meets all hello Azure RemoteApp image requirements - you can use that VM image as a starting point for your own VM, if you want.</span></span> <span data-ttu-id="94f18-108">Hello "Windows Server távoli asztali munkamenetgazda" kép hello könyvtárban keresse.</span><span class="sxs-lookup"><span data-stu-id="94f18-108">Just look for hello "Windows Server Remote Desktop Session Host" image in hello library.</span></span>

<span data-ttu-id="94f18-109">Saját rendszerkép alapján egy Azure virtuális gép két lépéseket toocreate – hello lemezkép létrehozása és majd töltse fel a hello Azure virtuális könyvtár tooAzure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="94f18-109">There are two steps toocreate your own image based on an Azure VM - create hello image and then upload it from hello Azure VM library tooAzure RemoteApp.</span></span>

## <a name="create-a-custom-image-based-on-an-azure-vm"></a><span data-ttu-id="94f18-110">Egy Azure virtuális Gépen alapuló egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="94f18-110">Create a custom image based on an Azure VM</span></span>
<span data-ttu-id="94f18-111">Ezen lépések toocreate egy Azure virtuális Gépen alapuló rendszerképet használja.</span><span class="sxs-lookup"><span data-stu-id="94f18-111">Use these steps toocreate an image based on an Azure VM.</span></span>

1. <span data-ttu-id="94f18-112">Hozzon létre egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="94f18-112">Create an Azure virtual machine.</span></span> <span data-ttu-id="94f18-113">Használhat hello "Windows Server távoli asztali munkamenetgazda" vagy "Windows Server távoli asztali munkamenet állomást a Microsoft Office 365 ProPlus" kép hello hello Azure virtuális gép lemezképének gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="94f18-113">You can use hello "Windows Server Remote Desktop Session Host" or hello "Windows Server Remote Desktop Session Host with Microsoft Office 365 ProPlus" image from hello Azure virtual machine image gallery.</span></span> <span data-ttu-id="94f18-114">Ez a rendszerkép összes hello Azure RemoteApp sablon kép követelménynek megfelelő.</span><span class="sxs-lookup"><span data-stu-id="94f18-114">This image meets all hello Azure RemoteApp template image requirements.</span></span>
   
    <span data-ttu-id="94f18-115">További információkért lásd: [Windows rendszerű virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="94f18-115">For details, see [Create a VM running Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="94f18-116">Csatlakozás a virtuális gép toohello és telepíteni, amelyet a Remoteappen keresztül tooshare hello alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="94f18-116">Connect toohello VM and install and configure hello apps that you want tooshare through RemoteApp.</span></span> <span data-ttu-id="94f18-117">Győződjön meg arról, hogy tooperform minden további, az alkalmazások által igényelt Windows konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="94f18-117">Make sure tooperform any additional Windows configurations required by your apps.</span></span>
   
    <span data-ttu-id="94f18-118">További információkért lásd: [hogyan tooLog a virtuális gép futó Windows Server tooa](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="94f18-118">For details, see [How tooLog on tooa Virtual Machine Running Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
3. <span data-ttu-id="94f18-119">Hello Windows Server távoli asztali munkamenetgazda lemezképeit egyikét használja, ha egy befoglalt érvényesítési parancsfájlt, amely biztosítja a virtuális gép megfelel-e hello RemoteApp előtti-reqs. van</span><span class="sxs-lookup"><span data-stu-id="94f18-119">If you are using one of hello Windows Server Remote Desktop Session Host images, there is an included validation script that will ensure your VM meets hello RemoteApp pre-reqs.</span></span> <span data-ttu-id="94f18-120">toorun parancsfájl, kattintson duplán a **ValidateRemoteAppImage** hello asztalon.</span><span class="sxs-lookup"><span data-stu-id="94f18-120">toorun script, double-click **ValidateRemoteAppImage** on hello desktop.</span></span> <span data-ttu-id="94f18-121">Győződjön meg arról, hogy a Folytatás toohello következő lépés előtt rögzítettek hello szkript által jelzett hibákat.</span><span class="sxs-lookup"><span data-stu-id="94f18-121">Ensure that all errors reported by hello script are fixed before proceeding toohello next step.</span></span>
4. <span data-ttu-id="94f18-122">A SYSPREP generalize és hello lemezképének.</span><span class="sxs-lookup"><span data-stu-id="94f18-122">SYSPREP generalize and capture hello image.</span></span> <span data-ttu-id="94f18-123">Lásd: [hogyan tooCapture egy Windows rendszerű virtuális gép tooUse sablonként](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="94f18-123">See [How tooCapture a Windows Virtual Machine tooUse as a Template](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) for instructions.</span></span>

## <a name="import-hello-image-into-hello-azure-remoteapp-image-library"></a><span data-ttu-id="94f18-124">Hello Azure RemoteApp Képtár hello kép importálása</span><span class="sxs-lookup"><span data-stu-id="94f18-124">Import hello image into hello Azure RemoteApp image library</span></span>
<span data-ttu-id="94f18-125">Ezen lépések tooimport hello új lemezképet használja az Azure Remoteappba:</span><span class="sxs-lookup"><span data-stu-id="94f18-125">Use these steps tooimport hello new image into Azure RemoteApp:</span></span>

1. <span data-ttu-id="94f18-126">A hello **Sablonrendszerképek** lapon:</span><span class="sxs-lookup"><span data-stu-id="94f18-126">In hello **Template Images** tab:</span></span>
   
   * <span data-ttu-id="94f18-127">Ha nincs meglévő lemezképet, kattintson a **feltölteni, vagy importáljon egy sablon rendszerképet**.</span><span class="sxs-lookup"><span data-stu-id="94f18-127">If you have no existing images, click **Upload or Import a Template Image**.</span></span>
   * <span data-ttu-id="94f18-128">Ha már legalább egy kép, kattintson a  **+**  tooadd új lemezképet.</span><span class="sxs-lookup"><span data-stu-id="94f18-128">If you have at least one image already, click **+** tooadd a new image.</span></span>
2. <span data-ttu-id="94f18-129">Válassza ki **kép importálása a virtuális gépek** könyvtárban, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="94f18-129">Select **Import an image from your Virtual Machines** library, and then click **Next**.</span></span>
3. <span data-ttu-id="94f18-130">Hello következő lapon válassza ki az egyéni lemezképet hello listából, és győződjön meg arról, hogy követték-e a lemezkép létrehozásakor hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="94f18-130">On hello next page, select your custom image from hello list and confirm that you followed hello steps listed when you created your image.</span></span> <span data-ttu-id="94f18-131">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="94f18-131">Click **Next**.</span></span>
4. <span data-ttu-id="94f18-132">Adjon meg egy nevet hello új RemoteApp lemezképet, majd mentse hello helyre hello pipa toostart hello az importálási folyamat kattintson.</span><span class="sxs-lookup"><span data-stu-id="94f18-132">Enter a name for hello new RemoteApp image and pick hello location, then click hello checkmark toostart hello import process.</span></span>

> [!NOTE]
> <span data-ttu-id="94f18-133">Azure virtuális gépek tooany Azure RemoteApp által támogatott Azure-beli hely által támogatott Azure bárhonnan importálhatja a lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="94f18-133">You can import images from any Azure location supported by Azure Virtual Machines tooany Azure location supported by Azure RemoteApp.</span></span> <span data-ttu-id="94f18-134">Attól függően, hogy hello helyek hello importálási too25 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="94f18-134">Depending on hello locations hello import can take up too25 minutes.</span></span>
> 
> 

<span data-ttu-id="94f18-135">Most már áll készen áll toocreate az új gyűjtemény vagy egy [felhő](remoteapp-create-cloud-deployment.md) gyűjtemény vagy [hibrid](remoteapp-create-hybrid-deployment.md), attól függően, az igényeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="94f18-135">Now you are ready toocreate your new collection, either a [cloud](remoteapp-create-cloud-deployment.md) collection or [hybrid](remoteapp-create-hybrid-deployment.md), depending on your needs.</span></span>

