---
title: "Virtuális gép létrehozása az Azure portálon |} Microsoft Docs"
description: "Windows virtuális gép létrehozása az Azure portálon."
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
ms.openlocfilehash: 0981872ff819fdf49a9cc97afce3c212013ce76b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a><span data-ttu-id="e1826-103">Az Azure portálon Windows rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1826-103">Create a virtual machine running Windows in the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e1826-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e1826-104">Azure portal</span></span>](tutorial.md)
> * [<span data-ttu-id="e1826-105">PowerShell: Klasszikus telepítési</span><span class="sxs-lookup"><span data-stu-id="e1826-105">PowerShell: Classic deployment</span></span>](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="e1826-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e1826-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="e1826-107">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="e1826-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="e1826-108">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="e1826-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="e1826-109">Megtudhatja, hogyan [a következő lépések segítségével a Resource Manager üzembe helyezési modellel](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) használatával a **Azure-portálon**.</span><span class="sxs-lookup"><span data-stu-id="e1826-109">Learn how to [perform these steps using the Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) using the **Azure portal**.</span></span>

<span data-ttu-id="e1826-110">Az oktatóanyag bemutatja, hogyan hozzon létre egy Azure virtuális gép (VM) fut a Windows Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e1826-110">This tutorial shows you how to create an Azure virtual machine (VM) running Windows in the Azure portal.</span></span> <span data-ttu-id="e1826-111">A Windows Server lemezkép használjuk példaként, de ez csak egyike a számos rendszerképet az Azure kínál.</span><span class="sxs-lookup"><span data-stu-id="e1826-111">We'll use a Windows Server image as an example, but that's just one of the many images Azure offers.</span></span> <span data-ttu-id="e1826-112">Vegye figyelembe, hogy a rendszerképek függ-e az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e1826-112">Note that your image choices depend on your subscription.</span></span> <span data-ttu-id="e1826-113">Windows asztali rendszerképek például MSDN-előfizetők számára elérhető lehet.</span><span class="sxs-lookup"><span data-stu-id="e1826-113">For example, Windows desktop images may be available to MSDN subscribers.</span></span>

<span data-ttu-id="e1826-114">Ez a szakasz bemutatja, hogyan használható a **irányítópult** válassza ki, majd létre az a virtuális gépet az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e1826-114">This section shows you how to use the **Dashboard** in the Azure portal to select and then create the virtual machine.</span></span>

<span data-ttu-id="e1826-115">Virtuális gépek használatával is létrehozhat [a saját lemezképek](createupload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="e1826-115">You can also create VMs using [your own images](createupload-vhd.md).</span></span> <span data-ttu-id="e1826-116">Ezzel és más módszerekkel kapcsolatos további tudnivalókért lásd: [Windows virtuális gépek létrehozásának különböző módszerei](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e1826-116">To learn about this and other methods, see [Different ways to create a Windows virtual machine](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<!-- 02/27/2017 Video removed as it was based on the classic portal. -->

## <span data-ttu-id="e1826-117"><a id="createvirtualmachine"></a>a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1826-117"><a id="createvirtualmachine"> </a>Create the virtual machine</span></span>
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a><span data-ttu-id="e1826-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1826-118">Next steps</span></span>
* <span data-ttu-id="e1826-119">Megtudhatja, hogyan [a Resource Manager üzembe helyezési modellel virtuális gép létrehozása](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e1826-119">Learn how to [create a VM using the Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in the Azure portal.</span></span>
* <span data-ttu-id="e1826-120">Jelentkezzen be a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="e1826-120">Log on to the virtual machine.</span></span> <span data-ttu-id="e1826-121">Útmutatásért lásd: [jelentkezzen be a Windows Server rendszerű virtuális gép](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="e1826-121">For instructions, see [Log on to a virtual machine running Windows Server](connect-logon.md).</span></span>
* <span data-ttu-id="e1826-122">Adatok tárolására lemezt csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="e1826-122">Attach a disk to store data.</span></span> <span data-ttu-id="e1826-123">Csatolhat is üres és a lemezek, amelyek adatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="e1826-123">You can attach both empty disks and disks that contain data.</span></span> <span data-ttu-id="e1826-124">Útmutatásért lásd: a [adatlemezt csatolni a klasszikus üzembe helyezési modellel létrehozott Windows virtuális gépek](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="e1826-124">For instructions, see the [Attach a data disk to a Windows virtual machine created with the classic deployment model](attach-disk.md).</span></span>
