---
title: "aaaAzure Windows Virtuálisgép-méretek – HPC |} Microsoft Docs"
description: "Elérhető a Windows Azure virtuális gépek számítási nagy teljesítményű hello különböző méretű sorolja fel."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 092bc32cfe048f439ad833911bef4ed17eda7977
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-compute-vm-sizes"></a><span data-ttu-id="b9080-103">Nagy teljesítményű számítási Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="b9080-103">High performance compute VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a><span data-ttu-id="b9080-104">Az RDMA-kompatibilis példányok</span><span class="sxs-lookup"><span data-stu-id="b9080-104">RDMA-capable instances</span></span>
<span data-ttu-id="b9080-105">Egy részhalmazát hello számítási igényű példányok (H16r, H16mr, A8 és A9) beállítást, a távoli közvetlen memória-hozzáférés (RDMA) hálózati kapcsolatot a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="b9080-105">A subset of hello compute-intensive instances (H16r, H16mr, A8, and A9) feature a network interface for remote direct memory access (RDMA) connectivity.</span></span> <span data-ttu-id="b9080-106">Ez az interfész továbbá toohello szabványos Azure hálózati illesztő elérhető tooother Virtuálisgép-méretek.</span><span class="sxs-lookup"><span data-stu-id="b9080-106">This interface is in addition toohello standard Azure network interface available tooother VM sizes.</span></span> 
  
<span data-ttu-id="b9080-107">Ez az interfész lehetővé teszi, hogy hello RDMA-kompatibilis példányok toocommunicate InfiniBand-hálózaton, működő FDR sebességét H16r és H16mr virtuális gépek és a gyorsító A8 és A9 virtuális gépek QDR sebességet.</span><span class="sxs-lookup"><span data-stu-id="b9080-107">This interface allows hello RDMA-capable instances toocommunicate over an InfiniBand network, operating at FDR rates for H16r and H16mr virtual machines, and QDR rates for A8 and A9 virtual machines.</span></span> <span data-ttu-id="b9080-108">E RDMA-képességeinek képes javítani a Message Passing Interface (MPI) alkalmazások hello méretezhetőségére és teljesítményére.</span><span class="sxs-lookup"><span data-stu-id="b9080-108">These RDMA capabilities can boost hello scalability and performance of Message Passing Interface (MPI) applications.</span></span>

<span data-ttu-id="b9080-109">Az alábbiakban követelmények az RDMA-kompatibilis Windows-alapú virtuális gépek tooaccess hello Azure RDMA hálózati:</span><span class="sxs-lookup"><span data-stu-id="b9080-109">Following are requirements for RDMA-capable Windows VMs tooaccess hello Azure RDMA network:</span></span> 

* <span data-ttu-id="b9080-110">**Operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="b9080-110">**Operating system**</span></span>
  
  <span data-ttu-id="b9080-111">Windows Server 2012 R2, Windows Server 2012-ben</span><span class="sxs-lookup"><span data-stu-id="b9080-111">Windows Server 2012 R2, Windows Server 2012</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="b9080-112">Jelenleg a Windows Server 2016 nem támogatja az Azure-ban RDMA-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="b9080-112">Currently, Windows Server 2016 does not support RDMA connectivity in Azure.</span></span>
  >

* <span data-ttu-id="b9080-113">**Rendelkezésre állási csoportot, vagy a felhőalapú szolgáltatás** – telepítés hello RDMA-kompatibilisek-e virtuális gépek egyazon rendelkezésre állási csoportban (használatakor hello Azure Resource Manager üzembe helyezési modellben) hello vagy ugyanazon a felhőalapú szolgáltatás hello (használatakor hello klasszikus üzembe helyezési modellel).</span><span class="sxs-lookup"><span data-stu-id="b9080-113">**Availability set or cloud service** – Deploy hello RDMA-capable VMs in hello same availability set (when you use hello Azure Resource Manager deployment model) or hello same cloud service (when you use hello classic deployment model).</span></span> <span data-ttu-id="b9080-114">Azure Batch használatakor hello RDMA-kompatibilisek-e virtuális gépek hello kell ugyanabban a készletben.</span><span class="sxs-lookup"><span data-stu-id="b9080-114">If you use Azure Batch, hello RDMA-capable VMs must be in hello same pool.</span></span>

* <span data-ttu-id="b9080-115">**MPI** -Microsoft MPI (MS-MPI) 2012 R2 vagy újabb, illetve Intel MPI könyvtár 5.x</span><span class="sxs-lookup"><span data-stu-id="b9080-115">**MPI** - Microsoft MPI (MS-MPI) 2012 R2 or later, Intel MPI Library 5.x</span></span>

  <span data-ttu-id="b9080-116">Támogatott MPI-megvalósítások használja a Microsoft hálózati közvetlen kapcsolat toocommunicate hello példányai között.</span><span class="sxs-lookup"><span data-stu-id="b9080-116">Supported MPI implementations use hello Microsoft Network Direct interface toocommunicate between instances.</span></span> 

* <span data-ttu-id="b9080-117">**RDMA hálózati címterüket** -hello RDMA hálózati az Azure-ban hello cím terület 172.16.0.0/16 foglalja le.</span><span class="sxs-lookup"><span data-stu-id="b9080-117">**RDMA network address space** - hello RDMA network in Azure reserves hello address space 172.16.0.0/16.</span></span> <span data-ttu-id="b9080-118">toorun MPI-alkalmazást a példányt az Azure virtuális hálózat, győződjön meg arról, hogy a virtuális hálózat címtere hello nem fedi át a hello RDMA hálózati.</span><span class="sxs-lookup"><span data-stu-id="b9080-118">toorun MPI applications on instances deployed in an Azure virtual network, make sure that hello virtual network address space does not overlap hello RDMA network.</span></span>

* <span data-ttu-id="b9080-119">**HpcVmDrivers Virtuálisgép-bővítmény** -RDMA-kompatibilis virtuális gépeken, hozzá kell adnia a hello HpcVmDrivers bővítmény tooinstall Windows hálózati eszköz-illesztőprogramok a RDMA hálózati kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="b9080-119">**HpcVmDrivers VM extension** - On RDMA-capable VMs, you must add hello HpcVmDrivers extension tooinstall Windows network device drivers for RDMA connectivity.</span></span> <span data-ttu-id="b9080-120">(Egyes központi telepítések A8 és A9 példányok, hello HpcVmDrivers bővítmény egészül ki automatikusan.) tooadd hello VM bővítmény tooa VM, használhat [Azure PowerShell](/powershell/azure/overview) parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="b9080-120">(In certain deployments of A8 and A9 instances, hello HpcVmDrivers extension is added automatically.) tooadd hello VM extension tooa VM, you can use [Azure PowerShell](/powershell/azure/overview) cmdlets.</span></span> 

  
  <span data-ttu-id="b9080-121">a következő parancs telepíti egy meglévő RDMA-kompatibilis nevű virtuális gép legutóbbi 1.1-es verzió HpcVMDrivers kiterjesztés hello hello *myVM* nevű hello erőforráscsoportban telepített *myResourceGroup* a hello  *USA nyugati régiója* régió:</span><span class="sxs-lookup"><span data-stu-id="b9080-121">hello following command installs hello latest version 1.1 HpcVMDrivers extension on an existing RDMA-capable VM named *myVM* deployed in hello resource group named *myResourceGroup* in hello *West US* region:</span></span>

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  <span data-ttu-id="b9080-122">További információkért lásd: [virtuálisgép-bővítmények és a szolgáltatások](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b9080-122">For more information, see [Virtual machine extensions and features](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="b9080-123">Hello a telepített virtuális gépek is dolgozhat bővítmények [klasszikus üzembe helyezési modellel](classic/manage-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="b9080-123">You can also work with extensions for VMs deployed in hello [classic deployment model](classic/manage-extensions.md).</span></span>


## <a name="using-hpc-pack"></a><span data-ttu-id="b9080-124">HPC Pack használatával</span><span class="sxs-lookup"><span data-stu-id="b9080-124">Using HPC Pack</span></span>

<span data-ttu-id="b9080-125">[A Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft szabad HPC fürt és a feladat felügyeleti megoldás, a beállítás egy a toocreate a számítási fürt Azure toorun MPI Windows-alapú alkalmazások és más HPC-munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="b9080-125">[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, is one option for you toocreate a compute cluster in Azure toorun Windows-based MPI applications and other HPC workloads.</span></span> <span data-ttu-id="b9080-126">HPC Pack 2012 R2 és újabb verziók például futtatókörnyezetének az MS-MPI, amely RDMA-kompatibilisek-e virtuális gépek hello Azure RDMA hálózati központi telepítésekor használ.</span><span class="sxs-lookup"><span data-stu-id="b9080-126">HPC Pack 2012 R2 and later versions include a runtime environment for MS-MPI that uses hello Azure RDMA network when deployed on RDMA-capable VMs.</span></span>




## <a name="other-sizes"></a><span data-ttu-id="b9080-127">Más méretek</span><span class="sxs-lookup"><span data-stu-id="b9080-127">Other sizes</span></span>
- [<span data-ttu-id="b9080-128">Általános célú</span><span class="sxs-lookup"><span data-stu-id="b9080-128">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="b9080-129">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="b9080-129">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="b9080-130">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="b9080-130">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)
- [<span data-ttu-id="b9080-131">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="b9080-131">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)
- [<span data-ttu-id="b9080-132">GPU-optimalizált</span><span class="sxs-lookup"><span data-stu-id="b9080-132">GPU optimized</span></span>](sizes-gpu.md)

## <a name="next-steps"></a><span data-ttu-id="b9080-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9080-133">Next steps</span></span>

- <span data-ttu-id="b9080-134">A feladatlisták toouse hello számítási igényű példányokhoz HPC Pack a Windows Server, lásd: [HPC Pack toorun MPI alkalmazásokkal Windows RDMA fürt beállítása](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b9080-134">For checklists toouse hello compute-intensive instances with HPC Pack on Windows Server, see [Set up a Windows RDMA cluster with HPC Pack toorun MPI applications](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

- <span data-ttu-id="b9080-135">toouse számítási igényű példányok MPI alkalmazások az Azure Batch futtatásakor lásd [használata többpéldányos feladatok toorun Message Passing Interface (MPI) alkalmazások az Azure Batch](../../batch/batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="b9080-135">toouse compute-intensive instances when running MPI applications with Azure Batch, see [Use multi-instance tasks toorun Message Passing Interface (MPI) applications in Azure Batch](../../batch/batch-mpi.md).</span></span>

- <span data-ttu-id="b9080-136">További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="b9080-136">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>




