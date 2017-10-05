---
title: "Automatikus oszlopszélesség egy Windows Azure-ban a PowerShell használatával |} Microsoft Docs"
description: "A Resource Manager üzembe helyezési modellel, az Azure Powershell létrehozott Windows virtuális gépek méretét."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 742efd1496de9ce76b1e5636297ef30f546bd108
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-windows-vm"></a><span data-ttu-id="d07de-103">A Windows virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="d07de-103">Resize a Windows VM</span></span>
<span data-ttu-id="d07de-104">Ez a cikk bemutatja, hogyan méretezze át egy Windows virtuális Gépet, a Resource Manager üzembe helyezési modellel Azure Powershell használatával létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d07de-104">This article shows you how to resize a Windows VM, created in the Resource Manager deployment model using Azure Powershell.</span></span>

<span data-ttu-id="d07de-105">Miután létrehozta a virtuális gép (VM), méretezhető a virtuális gép felfelé vagy lefelé a Virtuálisgép-méretet módosításával.</span><span class="sxs-lookup"><span data-stu-id="d07de-105">After you create a virtual machine (VM), you can scale the VM up or down by changing the VM size.</span></span> <span data-ttu-id="d07de-106">Néhány esetben először a virtuális gép kell felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="d07de-106">In some cases, you must deallocate the VM first.</span></span> <span data-ttu-id="d07de-107">Ez akkor fordulhat elő, ha új mérete nem érhető el a virtuális gép jelenleg üzemeltető hardver fürt.</span><span class="sxs-lookup"><span data-stu-id="d07de-107">This can happen if the new size is not available on the hardware cluster that is currently hosting the VM.</span></span>

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a><span data-ttu-id="d07de-108">Egy Windows virtuális gép nem a rendelkezésre állási csoportok átméretezése</span><span class="sxs-lookup"><span data-stu-id="d07de-108">Resize a Windows VM not in an availability set</span></span>
1. <span data-ttu-id="d07de-109">A hardver fürt, ahol a virtuális gép tárolása elérhető Virtuálisgép-méretek listázása.</span><span class="sxs-lookup"><span data-stu-id="d07de-109">List the VM sizes that are available on the hardware cluster where the VM is hosted.</span></span> 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. <span data-ttu-id="d07de-110">Ha a kívánt méretet, a következő parancsokat a virtuális gép átméretezésével.</span><span class="sxs-lookup"><span data-stu-id="d07de-110">If the desired size is listed, run the following commands to resize the VM.</span></span> <span data-ttu-id="d07de-111">Ha nem jelenik meg a kívánt méretet, folytassa a 3. lépésre.</span><span class="sxs-lookup"><span data-stu-id="d07de-111">If the desired size is not listed, go on to step 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="d07de-112">Ha a kívánt méretet nem szerepel, a következő parancsokat a virtuális gép felszabadítása méretezze át, és indítsa újra a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="d07de-112">If the desired size is not listed, run the following commands to deallocate the VM, resize it, and restart the VM.</span></span>
   
    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [!WARNING]
> <span data-ttu-id="d07de-113">A virtuális Géphez rendelt dinamikus IP-címek a virtuális gép felszabadítása kiadását.</span><span class="sxs-lookup"><span data-stu-id="d07de-113">Deallocating the VM releases any dynamic IP addresses assigned to the VM.</span></span> <span data-ttu-id="d07de-114">Az operációsrendszer- és adatlemezek, nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="d07de-114">The OS and data disks are not affected.</span></span> 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a><span data-ttu-id="d07de-115">Egy Windows virtuális gép rendelkezésre állási csoportba átméretezése</span><span class="sxs-lookup"><span data-stu-id="d07de-115">Resize a Windows VM in an availability set</span></span>
<span data-ttu-id="d07de-116">Ha egy virtuális gép rendelkezésre állási csoportba új mérete nem érhető el a virtuális Gépet tartalmazó hardver fürtön, majd a rendelkezésre állási csoport virtuális gépeinek kell átméretezni a virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="d07de-116">If the new size for a VM in an availability set is not available on the hardware cluster currently hosting the VM, then all VMs in the availability set will need to be deallocated to resize the VM.</span></span> <span data-ttu-id="d07de-117">Is szükség lehet frissíteni a rendelkezésre állási csoportban, miután egy virtuális gép át lett méretezve más virtuális gépek méretét.</span><span class="sxs-lookup"><span data-stu-id="d07de-117">You also might need to update the size of other VMs in the availability set after one VM has been resized.</span></span> <span data-ttu-id="d07de-118">A virtuális gépek rendelkezésre állási csoportba átméretezéséhez hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d07de-118">To resize a VM in an availability set, perform the following steps.</span></span>

1. <span data-ttu-id="d07de-119">A hardver fürt, ahol a virtuális gép tárolása elérhető Virtuálisgép-méretek listázása.</span><span class="sxs-lookup"><span data-stu-id="d07de-119">List the VM sizes that are available on the hardware cluster where the VM is hosted.</span></span>
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. <span data-ttu-id="d07de-120">Ha a kívánt méretet, a következő parancsokat a virtuális gép átméretezésével.</span><span class="sxs-lookup"><span data-stu-id="d07de-120">If the desired size is listed, run the following commands to resize the VM.</span></span> <span data-ttu-id="d07de-121">Ha nem szerepel, folytassa a 3.</span><span class="sxs-lookup"><span data-stu-id="d07de-121">If it is not listed, go to step 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="d07de-122">Ha nem jelenik meg a kívánt méretet, folytassa a következő lépéseket a rendelkezésre állási csoport virtuális gépeinek felszabadítani, méretezze át a virtuális gépek és újra kell indítania őket.</span><span class="sxs-lookup"><span data-stu-id="d07de-122">If the desired size is not listed, continue with the following steps to deallocate all VMs in the availability set, resize VMs, and restart them.</span></span>
4. <span data-ttu-id="d07de-123">Állítsa le a rendelkezésre állási csoportot az összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d07de-123">Stop all VMs in the availability set.</span></span>
   
   ```powershell
   $rg = "<resourceGroupName>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
   } 
   ```
5. <span data-ttu-id="d07de-124">Automatikus oszlopszélesség, és indítsa újra a virtuális gépek a rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="d07de-124">Resize and restart the VMs in the availability set.</span></span>
   
   ```powershell
   $rg = "<resourceGroupName>"
   $newSize = "<newVmSize>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
     $vm.HardwareProfile.VmSize = $newSize
     Update-AzureRmVM -ResourceGroupName $rg -VM $vm
     Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
   }
   ```

## <a name="next-steps"></a><span data-ttu-id="d07de-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d07de-125">Next steps</span></span>
* <span data-ttu-id="d07de-126">További méretezhetőséget, a virtuális gép több példányának futtatása, és kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="d07de-126">For additional scalability, run multiple VM instances and scale out.</span></span> <span data-ttu-id="d07de-127">További információkért lásd: [automatikus méretezése a Windows-alapú gépek egy virtuálisgép-méretezési csoportban lévő](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="d07de-127">For more information, see [Automatically scale Windows machines in a Virtual Machine Scale Set](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span></span>

