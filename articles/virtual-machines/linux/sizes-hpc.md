---
title: "Linux virtuális gép aaaAzure méretének - HPC |} Microsoft Docs"
description: "A Linux nagy teljesítményű számítástechnikai virtuális gépek Azure-ban elérhető különböző méretű hello sorolja fel."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 0b02ee592902ff8097a71898faea1ac17a947d1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-compute-linux-vm-sizes"></a>Nagy teljesítményű számítási Linux Virtuálisgép-méretek

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>Az RDMA-kompatibilis példányok
Egy részhalmazát hello számítási igényű példányok (H16r, H16mr, A8 és A9) beállítást, a távoli közvetlen memória-hozzáférés (RDMA) hálózati kapcsolatot a hálózati adaptert. Ez az interfész továbbá toohello szabványos Azure hálózati illesztő elérhető tooother Virtuálisgép-méretek. 
  
Ez az interfész lehetővé teszi, hogy hello RDMA-kompatibilis példányok toocommunicate InfiniBand-hálózaton, működő FDR sebességét H16r és H16mr virtuális gépek és a gyorsító A8 és A9 virtuális gépek QDR sebességet. E RDMA-képességeinek képes javítani a Message Passing Interface (MPI) alkalmazások hello méretezhetőségére és teljesítményére.

Az alábbiakban követelmények az RDMA-kompatibilis Linux virtuális gépek tooaccess hello Azure RDMA hálózati:
 
* **Azokat a terjesztéseket** - telepítenie kell a virtuális gépek az RDMA-kompatibilis SUSE Linux Enterprise Server (SLES), vagy rosszindulatú Wave szoftver (korábbi nevén OpenLogic) – CentOS-alapú HPC képek hello Azure piactéren. hello következő piactéren elérhető rendszerkép támogatják az RDMA-kapcsolatot:
  
    * SLES 12 HPC, vagy a SLES 12 SP1 SP1 HPC (prémium)
    
    * 7.3 HPC centOS-alapú, 7.1-es HPC CentOS-alapú, 6,8 HPC CentOS-alapú vagy 6.5 HPC CentOS-alapú  
 
        > [!NOTE]
        > H sorozatú virtuális gépekhez javasoljuk a SLES 12 SP1 HPC lemezkép vagy a CentOS-alapú 7.1-es vagy a későbbi HPC-lemezképet.
        >
        > A hello CentOS alapú HPC-lemezképek, a kernel frissítések le vannak tiltva a hello **yum** konfigurációs fájlt. Ennek az az oka hello Linux RDMA illesztőprogramok képezi RPM-csomag, és előfordulhat, hogy nem működik a illesztőprogram-frissítést, ha hello kernel frissül.
        > 
        > 
* **MPI** -Intel MPI könyvtár 5.x
  
    Hello Piactéri lemezképhez választott, külön licencelési, a telepítés vagy Intel MPI konfigurációja lehet szükség, az alábbiak szerint: 
  
  * **SLES 12 SP1 HPC lemezkép** -Intel MPI-csomagok terjesztése a virtuális gép hello. Telepítse a hello a következő parancs futtatásával:

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * **HPC-lemezképek centOS alapú** -Intel MPI 5.1 már telepítve van.  
    
    További rendszerkonfiguráció szükséges toorun MPI-feladatok a fürtözött virtuális gépeken. Például egy fürtön található virtuális gépek kell tooestablish hello között megbízhatósági számítási csomópontjain. Tipikus beállítások, lásd: [egy Linux RDMA fürt toorun MPI alkalmazások beállítása](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="network-topology-considerations"></a>Hálózati topológia szempontjai
* Az RDMA-kompatibilis Linux virtuális gépeken futó Azure-ban Eth1 RDMA hálózati forgalom számára van fenntartva. Ne módosítsa a Eth1 beállításokat vagy semmilyen információt toothis hálózati utaló hello konfigurációs fájlban. Eth0 rendszeres Azure hálózati forgalom számára van fenntartva.

* Az Azure IP keresztül InfiniBand (IB) nem támogatott. Csak IB feletti RDMA esetén támogatott.

## <a name="using-hpc-pack"></a>HPC Pack használatával
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft szabad HPC fürt és a feladat felügyeleti megoldás, a beállítás egy a toouse hello számítási igényű példányok Linux meg. HPC Pack legújabb kiadásaiban hello toorun a számítási csomópontok az Azure virtuális gépeken, a Windows Server átjárócsomópont által kezelt telepített számos Linux terjesztésekről támogatja. RDMA-kompatibilis Linux számítási csomópontok Intel MPI fut, a HPC Pack ütemezése és Linux MPI alkalmazások futtatását, hogy hozzáférési hello RDMA hálózati. Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Más méretek
- [Általános célú](sizes-general.md)
- [Számításra optimalizált](sizes-compute.md)
- [Memóriaoptimalizált](sizes-memory.md)
- [Tárolásra optimalizált](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Következő lépések

- tooget lépések telepítésére és használatára számítási igényű méretek és rdma-t, Linux, lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.




