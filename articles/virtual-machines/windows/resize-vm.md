---
title: aaaUse PowerShell tooresize egy Windows Azure-ban |} Microsoft Docs
description: "A Windows hello Resource Manager üzembe helyezési modellel, az Azure Powershell használatával létrehozott virtuális gépek méretét."
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
ms.openlocfilehash: a4a80f3bc99911e4f1a095f0ce63aca00fa50694
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-windows-vm"></a><span data-ttu-id="52c9c-103">A Windows virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="52c9c-103">Resize a Windows VM</span></span>
<span data-ttu-id="52c9c-104">Ez a cikk bemutatja, hogyan tooresize egy Windows virtuális gép létrehozása hello Resource Manager üzembe helyezési modellel Azure Powershell használatával.</span><span class="sxs-lookup"><span data-stu-id="52c9c-104">This article shows you how tooresize a Windows VM, created in hello Resource Manager deployment model using Azure Powershell.</span></span>

<span data-ttu-id="52c9c-105">Miután létrehozta a virtuális gép (VM), méretezhető hello VM felfelé vagy lefelé hello Virtuálisgép-méretet módosításával.</span><span class="sxs-lookup"><span data-stu-id="52c9c-105">After you create a virtual machine (VM), you can scale hello VM up or down by changing hello VM size.</span></span> <span data-ttu-id="52c9c-106">Néhány esetben először hello VM kell felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="52c9c-106">In some cases, you must deallocate hello VM first.</span></span> <span data-ttu-id="52c9c-107">Ez akkor fordulhat elő, ha hello új mérete nem érhető el a virtuális gép hello jelenleg üzemeltető hello hardver fürt.</span><span class="sxs-lookup"><span data-stu-id="52c9c-107">This can happen if hello new size is not available on hello hardware cluster that is currently hosting hello VM.</span></span>

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a><span data-ttu-id="52c9c-108">Egy Windows virtuális gép nem a rendelkezésre állási csoportok átméretezése</span><span class="sxs-lookup"><span data-stu-id="52c9c-108">Resize a Windows VM not in an availability set</span></span>
1. <span data-ttu-id="52c9c-109">Ahol hello virtuális gép tárolása hello hardver fürtön elérhető hello Virtuálisgép-méretek listázása.</span><span class="sxs-lookup"><span data-stu-id="52c9c-109">List hello VM sizes that are available on hello hardware cluster where hello VM is hosted.</span></span> 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. <span data-ttu-id="52c9c-110">Ha hello szükséges méret szerepel, futtassa a következő parancsok tooresize hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="52c9c-110">If hello desired size is listed, run hello following commands tooresize hello VM.</span></span> <span data-ttu-id="52c9c-111">Ha hello szükséges méret nem szerepel a listában, nyissa meg toostep 3.</span><span class="sxs-lookup"><span data-stu-id="52c9c-111">If hello desired size is not listed, go on toostep 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="52c9c-112">Ha hello szükséges méret nem szerepel a listában, futtassa a következő parancsok toodeallocate hello VM, méretezze át, és indítsa újra a virtuális gép hello hello.</span><span class="sxs-lookup"><span data-stu-id="52c9c-112">If hello desired size is not listed, run hello following commands toodeallocate hello VM, resize it, and restart hello VM.</span></span>
   
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
> <span data-ttu-id="52c9c-113">Virtuális gép hello felszabadítása dinamikus IP-címek hozzárendelése a virtuális gép toohello kiadását.</span><span class="sxs-lookup"><span data-stu-id="52c9c-113">Deallocating hello VM releases any dynamic IP addresses assigned toohello VM.</span></span> <span data-ttu-id="52c9c-114">az operációs rendszer hello és adatlemezek nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="52c9c-114">hello OS and data disks are not affected.</span></span> 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a><span data-ttu-id="52c9c-115">Egy Windows virtuális gép rendelkezésre állási csoportba átméretezése</span><span class="sxs-lookup"><span data-stu-id="52c9c-115">Resize a Windows VM in an availability set</span></span>
<span data-ttu-id="52c9c-116">Ha hello új egy rendelkezésre állási csoportot a virtuális gép mérete nem érhető el a hello hardver fürt hello tartalmazó virtuális gép, majd hello rendelkezésre állási csoport virtuális gépeinek kell toobe tooresize hello virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="52c9c-116">If hello new size for a VM in an availability set is not available on hello hardware cluster currently hosting hello VM, then all VMs in hello availability set will need toobe deallocated tooresize hello VM.</span></span> <span data-ttu-id="52c9c-117">Előfordulhat, hogy szükség tooupdate hello méretétől, más virtuális gépek hello rendelkezésre állási csoportban, miután egy virtuális gép át lett méretezve.</span><span class="sxs-lookup"><span data-stu-id="52c9c-117">You also might need tooupdate hello size of other VMs in hello availability set after one VM has been resized.</span></span> <span data-ttu-id="52c9c-118">tooresize a virtuális gépek rendelkezésre állási csoportba, hajtsa végre a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="52c9c-118">tooresize a VM in an availability set, perform hello following steps.</span></span>

1. <span data-ttu-id="52c9c-119">Ahol hello virtuális gép tárolása hello hardver fürtön elérhető hello Virtuálisgép-méretek listázása.</span><span class="sxs-lookup"><span data-stu-id="52c9c-119">List hello VM sizes that are available on hello hardware cluster where hello VM is hosted.</span></span>
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. <span data-ttu-id="52c9c-120">Ha hello szükséges méret szerepel, futtassa a következő parancsok tooresize hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="52c9c-120">If hello desired size is listed, run hello following commands tooresize hello VM.</span></span> <span data-ttu-id="52c9c-121">Ha nem szerepel, folytassa a toostep 3.</span><span class="sxs-lookup"><span data-stu-id="52c9c-121">If it is not listed, go toostep 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="52c9c-122">Hello szükséges méret nem szerepel a listában, folytassa a következő lépéseket toodeallocate hello hello rendelkezésre állási csoport virtuális gépeinek, méretezze át a virtuális gépek és újra kell indítania őket.</span><span class="sxs-lookup"><span data-stu-id="52c9c-122">If hello desired size is not listed, continue with hello following steps toodeallocate all VMs in hello availability set, resize VMs, and restart them.</span></span>
4. <span data-ttu-id="52c9c-123">Állítsa le virtuális gépeinek hello rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="52c9c-123">Stop all VMs in hello availability set.</span></span>
   
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
5. <span data-ttu-id="52c9c-124">Átméretezése, és indítsa újra a hello virtuális gépek hello rendelkezésre állási készlet.</span><span class="sxs-lookup"><span data-stu-id="52c9c-124">Resize and restart hello VMs in hello availability set.</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="52c9c-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="52c9c-125">Next steps</span></span>
* <span data-ttu-id="52c9c-126">További méretezhetőséget, a virtuális gép több példányának futtatása, és kiterjesztése. További információkért lásd: [automatikus méretezése a Windows-alapú gépek egy virtuálisgép-méretezési csoportban lévő](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="52c9c-126">For additional scalability, run multiple VM instances and scale out. For more information, see [Automatically scale Windows machines in a Virtual Machine Scale Set](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span></span>

