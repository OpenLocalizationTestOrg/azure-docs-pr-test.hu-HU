---
title: "egy virtuális Gépet az Azure-portálon hello aaaCreate |} Microsoft Docs"
description: "Windows virtuális gép létrehozása az Azure-portálon hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 3848a3d06a7ce377633db78ea385826ed40f94fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-running-windows-in-hello-azure-portal"></a><span data-ttu-id="f826b-103">A Windows hello Azure-portálon futó virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f826b-103">Create a virtual machine running Windows in hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f826b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f826b-104">Azure portal</span></span>](tutorial.md)
> * [<span data-ttu-id="f826b-105">PowerShell: Klasszikus telepítési</span><span class="sxs-lookup"><span data-stu-id="f826b-105">PowerShell: Classic deployment</span></span>](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="f826b-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f826b-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f826b-107">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="f826b-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="f826b-108">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="f826b-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="f826b-109">Ismerje meg, hogyan túl[hello Resource Manager telepítési modell használatával a következő lépésekkel](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello segítségével **Azure-portálon**.</span><span class="sxs-lookup"><span data-stu-id="f826b-109">Learn how too[perform these steps using hello Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) using hello **Azure portal**.</span></span>

<span data-ttu-id="f826b-110">Az oktatóanyag bemutatja, hogyan toocreate egy Azure virtuális gép (VM) fut a Windows hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f826b-110">This tutorial shows you how toocreate an Azure virtual machine (VM) running Windows in hello Azure portal.</span></span> <span data-ttu-id="f826b-111">A Windows Server lemezkép használjuk példaként, de ez csak egy hello a számos lemezképek Azure ajánlatokat.</span><span class="sxs-lookup"><span data-stu-id="f826b-111">We'll use a Windows Server image as an example, but that's just one of hello many images Azure offers.</span></span> <span data-ttu-id="f826b-112">Vegye figyelembe, hogy a rendszerképek függ-e az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f826b-112">Note that your image choices depend on your subscription.</span></span> <span data-ttu-id="f826b-113">Windows asztali rendszerképek például elérhető tooMSDN előfizetők lehet.</span><span class="sxs-lookup"><span data-stu-id="f826b-113">For example, Windows desktop images may be available tooMSDN subscribers.</span></span>

<span data-ttu-id="f826b-114">Ez a szakasz bemutatja, hogyan toouse hello **irányítópult** az Azure portál tooselect hello és majd a hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f826b-114">This section shows you how toouse hello **Dashboard** in hello Azure portal tooselect and then create hello virtual machine.</span></span>

<span data-ttu-id="f826b-115">Virtuális gépek használatával is létrehozhat [a saját lemezképek](createupload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="f826b-115">You can also create VMs using [your own images](createupload-vhd.md).</span></span> <span data-ttu-id="f826b-116">Ezzel és más módszerekkel kapcsolatos toolearn lásd [különböző módokon toocreate egy Windows rendszerű virtuális gép](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f826b-116">toolearn about this and other methods, see [Different ways toocreate a Windows virtual machine](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<!-- 02/27/2017 Video removed as it was based on hello classic portal. -->

## <span data-ttu-id="f826b-117"><a id="createvirtualmachine"></a>Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f826b-117"><a id="createvirtualmachine"> </a>Create hello virtual machine</span></span>
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a><span data-ttu-id="f826b-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f826b-118">Next steps</span></span>
* <span data-ttu-id="f826b-119">Ismerje meg, hogyan túl[hello Resource Manager üzembe helyezési modellben virtuális gép létrehozása](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f826b-119">Learn how too[create a VM using hello Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in hello Azure portal.</span></span>
* <span data-ttu-id="f826b-120">Jelentkezzen be a virtuális gép toohello.</span><span class="sxs-lookup"><span data-stu-id="f826b-120">Log on toohello virtual machine.</span></span> <span data-ttu-id="f826b-121">Útmutatásért lásd: [jelentkezzen be Windows Server rendszerű tooa virtuális gép](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="f826b-121">For instructions, see [Log on tooa virtual machine running Windows Server](connect-logon.md).</span></span>
* <span data-ttu-id="f826b-122">Csatlakoztassa a lemezadatokat toostore.</span><span class="sxs-lookup"><span data-stu-id="f826b-122">Attach a disk toostore data.</span></span> <span data-ttu-id="f826b-123">Csatolhat is üres és a lemezek, amelyek adatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="f826b-123">You can attach both empty disks and disks that contain data.</span></span> <span data-ttu-id="f826b-124">Útmutatásért lásd: hello [adatok lemez tooa Windows létrehozott virtuális gépek hello klasszikus üzembe helyezési modellel csatolása](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="f826b-124">For instructions, see hello [Attach a data disk tooa Windows virtual machine created with hello classic deployment model](attach-disk.md).</span></span>
