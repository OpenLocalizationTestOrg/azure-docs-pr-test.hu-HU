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
# <a name="high-performance-compute-vm-sizes"></a>Nagy teljesítményű számítási Virtuálisgép-méretek

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>Az RDMA-kompatibilis példányok
Egy részhalmazát hello számítási igényű példányok (H16r, H16mr, A8 és A9) beállítást, a távoli közvetlen memória-hozzáférés (RDMA) hálózati kapcsolatot a hálózati adaptert. Ez az interfész továbbá toohello szabványos Azure hálózati illesztő elérhető tooother Virtuálisgép-méretek. 
  
Ez az interfész lehetővé teszi, hogy hello RDMA-kompatibilis példányok toocommunicate InfiniBand-hálózaton, működő FDR sebességét H16r és H16mr virtuális gépek és a gyorsító A8 és A9 virtuális gépek QDR sebességet. E RDMA-képességeinek képes javítani a Message Passing Interface (MPI) alkalmazások hello méretezhetőségére és teljesítményére.

Az alábbiakban követelmények az RDMA-kompatibilis Windows-alapú virtuális gépek tooaccess hello Azure RDMA hálózati: 

* **Operációs rendszer**
  
  Windows Server 2012 R2, Windows Server 2012-ben
  
  > [!NOTE]
  > Jelenleg a Windows Server 2016 nem támogatja az Azure-ban RDMA-kapcsolatot.
  >

* **Rendelkezésre állási csoportot, vagy a felhőalapú szolgáltatás** – telepítés hello RDMA-kompatibilisek-e virtuális gépek egyazon rendelkezésre állási csoportban (használatakor hello Azure Resource Manager üzembe helyezési modellben) hello vagy ugyanazon a felhőalapú szolgáltatás hello (használatakor hello klasszikus üzembe helyezési modellel). Azure Batch használatakor hello RDMA-kompatibilisek-e virtuális gépek hello kell ugyanabban a készletben.

* **MPI** -Microsoft MPI (MS-MPI) 2012 R2 vagy újabb, illetve Intel MPI könyvtár 5.x

  Támogatott MPI-megvalósítások használja a Microsoft hálózati közvetlen kapcsolat toocommunicate hello példányai között. 

* **RDMA hálózati címterüket** -hello RDMA hálózati az Azure-ban hello cím terület 172.16.0.0/16 foglalja le. toorun MPI-alkalmazást a példányt az Azure virtuális hálózat, győződjön meg arról, hogy a virtuális hálózat címtere hello nem fedi át a hello RDMA hálózati.

* **HpcVmDrivers Virtuálisgép-bővítmény** -RDMA-kompatibilis virtuális gépeken, hozzá kell adnia a hello HpcVmDrivers bővítmény tooinstall Windows hálózati eszköz-illesztőprogramok a RDMA hálózati kapcsolatot. (Egyes központi telepítések A8 és A9 példányok, hello HpcVmDrivers bővítmény egészül ki automatikusan.) tooadd hello VM bővítmény tooa VM, használhat [Azure PowerShell](/powershell/azure/overview) parancsmagok. 

  
  a következő parancs telepíti egy meglévő RDMA-kompatibilis nevű virtuális gép legutóbbi 1.1-es verzió HpcVMDrivers kiterjesztés hello hello *myVM* nevű hello erőforráscsoportban telepített *myResourceGroup* a hello  *USA nyugati régiója* régió:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  További információkért lásd: [virtuálisgép-bővítmények és a szolgáltatások](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Hello a telepített virtuális gépek is dolgozhat bővítmények [klasszikus üzembe helyezési modellel](classic/manage-extensions.md).


## <a name="using-hpc-pack"></a>HPC Pack használatával

[A Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft szabad HPC fürt és a feladat felügyeleti megoldás, a beállítás egy a toocreate a számítási fürt Azure toorun MPI Windows-alapú alkalmazások és más HPC-munkaterhelések. HPC Pack 2012 R2 és újabb verziók például futtatókörnyezetének az MS-MPI, amely RDMA-kompatibilisek-e virtuális gépek hello Azure RDMA hálózati központi telepítésekor használ.




## <a name="other-sizes"></a>Más méretek
- [Általános célú](sizes-general.md)
- [Számításra optimalizált](sizes-compute.md)
- [Memóriaoptimalizált](../virtual-machines-windows-sizes-memory.md)
- [Tárolásra optimalizált](../virtual-machines-windows-sizes-storage.md)
- [GPU-optimalizált](sizes-gpu.md)

## <a name="next-steps"></a>Következő lépések

- A feladatlisták toouse hello számítási igényű példányokhoz HPC Pack a Windows Server, lásd: [HPC Pack toorun MPI alkalmazásokkal Windows RDMA fürt beállítása](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- toouse számítási igényű példányok MPI alkalmazások az Azure Batch futtatásakor lásd [használata többpéldányos feladatok toorun Message Passing Interface (MPI) alkalmazások az Azure Batch](../../batch/batch-mpi.md).

- További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.




