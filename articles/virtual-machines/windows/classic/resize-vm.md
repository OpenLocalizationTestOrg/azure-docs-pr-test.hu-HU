---
title: "Egy Windows virtuális Gépet a klasszikus üzembe helyezési modellel - Azure átméretezése |} Microsoft Docs"
description: "A klasszikus üzembe helyezési modellel, az Azure Powershell létrehozott Windows virtuális gépek méretét."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e3038215-001c-406e-904d-e0f4e326a4c7
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 4277bc8394c7ba140291e9dc776162e87deab96b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a><span data-ttu-id="098af-103">A Windows virtuális gép létrehozása a klasszikus üzembe helyezési modellel átméretezése</span><span class="sxs-lookup"><span data-stu-id="098af-103">Resize a Windows VM created in the classic deployment model</span></span>
<span data-ttu-id="098af-104">Ez a cikk bemutatja, hogyan méretezze át egy Windows virtuális Gépet, a klasszikus üzembe helyezési modellel, az Azure Powershell használatával létrehozni.</span><span class="sxs-lookup"><span data-stu-id="098af-104">This article shows you how to resize a Windows VM, created in the classic deployment model using Azure Powershell.</span></span>

<span data-ttu-id="098af-105">Annak eldöntéséhez, hogy átméretezni a virtuális gép lehetőséget, nincsenek két fogalom, amely a tartomány a méretezze át a virtuális gép számára elérhető méretek szabályozza.</span><span class="sxs-lookup"><span data-stu-id="098af-105">When considering the ability to resize a VM, there are two concepts which control the range of sizes available to resize the virtual machine.</span></span> <span data-ttu-id="098af-106">Az első fogalma a régió, ahol a virtuális Gépre van telepítve.</span><span class="sxs-lookup"><span data-stu-id="098af-106">The first concept is the region in which your VM is deployed.</span></span> <span data-ttu-id="098af-107">A Virtuálisgép-méretek elérhető régióban listáját az Azure-régiókat weblap szolgáltatások lapon alatt áll.</span><span class="sxs-lookup"><span data-stu-id="098af-107">The list of VM sizes available in region is under the Services tab of the Azure Regions web page.</span></span> <span data-ttu-id="098af-108">A második fogalma a fizikai hardver a virtuális Gépet tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="098af-108">The second concept is the physical hardware currently hosting your VM.</span></span> <span data-ttu-id="098af-109">A fizikai kiszolgálók, virtuális gépeket üzemeltet a fürtök közös fizikai hardver együtt vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="098af-109">The physical servers hosting VMs are grouped together in clusters of common physical hardware.</span></span> <span data-ttu-id="098af-110">A módszer egy virtuális gép méretének módosítása eltér attól függően, hogy a kívánt új Virtuálisgép-méretet támogatja a hardveres fürt a virtuális Gépet tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="098af-110">The method of changing a VM size differs depending on if the desired new VM size is supported by the hardware cluster currently hosting the VM.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="098af-111">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="098af-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="098af-112">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="098af-112">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="098af-113">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="098af-113">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="098af-114">Emellett [méretezze át a virtuális gép létrehozása a Resource Manager üzembe helyezési modellel](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="098af-114">You can also [resize a VM created in the Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="add-your-account"></a><span data-ttu-id="098af-115">A fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="098af-115">Add your account</span></span>
<span data-ttu-id="098af-116">Konfigurálnia kell az Azure PowerShell, a klasszikus Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="098af-116">You must configure Azure PowerShell to work with classic Azure resources.</span></span> <span data-ttu-id="098af-117">Konfigurálhatja az Azure PowerShell hagyományos erőforrások kezeléséhez az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="098af-117">Follow the steps below to configure Azure PowerShell to manage classic resources.</span></span>

1. <span data-ttu-id="098af-118">A PowerShell-parancssorba írja be a `Add-AzureAccount` kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="098af-118">At the PowerShell prompt, type `Add-AzureAccount` and click **Enter**.</span></span> 
2. <span data-ttu-id="098af-119">Írja be az Azure-előfizetéshez társított e-mail címét, és kattintson a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="098af-119">Type in the email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="098af-120">Adja meg a fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="098af-120">Type in the password for your account.</span></span> 
4. <span data-ttu-id="098af-121">Kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="098af-121">Click **Sign in**.</span></span> 

## <a name="resize-in-the-same-hardware-cluster"></a><span data-ttu-id="098af-122">Hardver ugyanazon fürt átméretezése</span><span class="sxs-lookup"><span data-stu-id="098af-122">Resize in the same hardware cluster</span></span>
<span data-ttu-id="098af-123">A virtuális gép elérhető, a hardver-fürt a virtuális Gépet üzemeltető méretre átméretezéséhez hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="098af-123">To resize a VM to a size available in the hardware cluster hosting the VM, perform the following steps.</span></span>

1. <span data-ttu-id="098af-124">Futtassa a következő PowerShell-parancsot a hardver-fürt a felhőalapú szolgáltatás, amely tartalmazza a virtuális Gépet üzemeltető érhető el a Virtuálisgép-méretek listázása.</span><span class="sxs-lookup"><span data-stu-id="098af-124">Run the following PowerShell command to list the VM sizes available in the hardware cluster hosting the cloud service which contains the VM.</span></span>
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. <span data-ttu-id="098af-125">A következő parancsokat a virtuális gép átméretezésével.</span><span class="sxs-lookup"><span data-stu-id="098af-125">Run the following commands to resize the VM.</span></span>
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a><span data-ttu-id="098af-126">Új hardver fürt átméretezése</span><span class="sxs-lookup"><span data-stu-id="098af-126">Resize on a new hardware cluster</span></span>
<span data-ttu-id="098af-127">A virtuális gép nem érhető el, a hardver-fürt a virtuális Gépet, a felhőalapú üzemeltetési méretre átméretezése szolgáltatás és a felhőalapú szolgáltatás virtuális gépeinek kell újra létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="098af-127">To resize a VM to a size not available in the hardware cluster hosting the VM, the cloud service and all VMs in the cloud service must be recreated.</span></span> <span data-ttu-id="098af-128">Ezért a felhőalapú szolgáltatás virtuális gépeinek meg kell adni a hardver-fürtökön támogatott méretének minden felhőalapú szolgáltatás egyetlen hardver fürtön helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="098af-128">Each cloud service is hosted on a single hardware cluster so all VMs in the cloud service must be a size that is supported on a hardware cluster.</span></span> <span data-ttu-id="098af-129">A következő lépések azt ismerteti, hogyan méretezze át a virtuális gépek törlésével és majd újbóli létrehozása a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="098af-129">The following steps will describe how to resize a VM by deleting and then recreating the cloud service.</span></span>

1. <span data-ttu-id="098af-130">Futtassa a következő PowerShell-parancsot a Virtuálisgép-méretek elérhető régióban listázásához.</span><span class="sxs-lookup"><span data-stu-id="098af-130">Run the following PowerShell command to list the VM sizes available in the region.</span></span> 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. <span data-ttu-id="098af-131">Jegyezze fel az összes konfigurációs beállítások az egyes virtuális gépek a a felhőalapú szolgáltatás, amely tartalmazza a virtuális gépek a méretezhető.</span><span class="sxs-lookup"><span data-stu-id="098af-131">Make note of all configuration settings for each VM in the cloud service which contains the VM to be resized.</span></span> 
3. <span data-ttu-id="098af-132">Törölje a felhőalapú szolgáltatás, a lemezek megőrizni az egyes virtuális gépek beállítás választása esetén minden virtuális gépe.</span><span class="sxs-lookup"><span data-stu-id="098af-132">Delete all VMs in the cloud service selecting the option to retain the disks for each VM.</span></span>
4. <span data-ttu-id="098af-133">Hozza létre újra a virtuális Gépet a kívánt Virtuálisgép-méretet használó méretezhető át.</span><span class="sxs-lookup"><span data-stu-id="098af-133">Recreate the VM to be resized using the desired VM size.</span></span>
5. <span data-ttu-id="098af-134">Hozza létre újra a többi virtuális gépen, amely a felhőalapú szolgáltatás, a hardver fürt ilyenkor a felhőalapú szolgáltatáshoz elérhető Virtuálisgép-méretet használó voltak.</span><span class="sxs-lookup"><span data-stu-id="098af-134">Recreate all other VMs which were in the cloud service using a VM size available in the hardware cluster now hosting the cloud service.</span></span>

<span data-ttu-id="098af-135">Egy minta parancsfájlt törlése és újbóli létrehozása egy felhőszolgáltatás, új Virtuálisgép-méretet használó található [Itt](https://github.com/Azure/azure-vm-scripts).</span><span class="sxs-lookup"><span data-stu-id="098af-135">A sample script for deleting and recreating a cloud service using a new VM size can be found [here](https://github.com/Azure/azure-vm-scripts).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="098af-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="098af-136">Next steps</span></span>
* <span data-ttu-id="098af-137">[A virtuális gép létrehozása a Resource Manager üzembe helyezési modellel átméretezése](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="098af-137">[Resize a VM created in the Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

