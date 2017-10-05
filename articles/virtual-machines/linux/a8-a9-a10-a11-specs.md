---
title: "Linux virtuális gépek számítási igényű kapcsolatos |} Microsoft Docs"
description: "Háttér-információkat és a Linux virtuális gépek a H-sorozat és A8, A9, A10, és A11 számítási igényű méret használatának szempontjai"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 16cf6e2d-f401-4b22-af8c-9a337179f5f6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a2307f7055966ec7146b5da0b4daf1ad469abe2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a>H-sorozat és számítási igényű A-sorozatú virtuális gépeken Linux kapcsolatos
Íme a háttér-információkat és az újabb Azure H-adatsorozat és a korábbi A8, A9, A10 és A11 méretű, más néven használatával kapcsolatos megfontolandó *számítási igényű* példányok. Ez a cikk foglalkozik, ezek mérete használatáról Linux virtuális gépekhez. Használhatja őket [Windows virtuális gépek](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Alapvető specifikációk, tárolási kapacitás és a lemez adatai: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Az RDMA hálózati hozzáférés
Az RDMA-kompatibilis Linux virtuális gépek, hogy a következő futtatási egyik támogatott Linux HPC disztribúcióiról, valamint a támogatott MPI megvalósítását, hogy kihasználja az Azure RDMA hálózati tárolófürtöket hozhat létre. Lásd: [MPI alkalmazások futtatásához hozzon létre egy Linux RDMA fürt](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) üzembe helyezési lehetőségei és minta konfigurációs lépéseket.

* **Azokat a terjesztéseket** -virtuális gépek telepítenie kell az RDMA-kompatibilis SUSE Linux Enterprise Server (SLES) vagy rosszindulatú Wave szoftver (korábbi nevén OpenLogic) – CentOS-alapú HPC lemezképet az Azure piactéren. A következő piactéren elérhető rendszerkép támogatják az RDMA-kapcsolatot:
  
    * SLES 12 HPC, SLES 12 SP1 SP1 HPC (prémium)
    
    * 7.1-es HPC centOS-alapú, 6.5 HPC CentOS-alapú  
 
        > [!NOTE]
        > H sorozatú virtuális gépekhez javasoljuk a SLES 12 SP1 HPC lemezképhez vagy 7.1-es HPC CentOS alapú lemezkép.
        >
        > A CentOS-alapú HPC-lemezképek, a kernel frissítések le vannak tiltva a **yum** konfigurációs fájlt. Ennek az az oka egy RPM-csomag terjesztése a Linux RDMA-illesztőprogramok és illesztőprogram-frissítést nem lehet, hogy működik, ha a kernel frissül.
        > 
        > 
* **MPI** -Intel MPI könyvtár 5.x
  
    A Piactéri lemezképhez választott, külön licencelési, a telepítés vagy Intel MPI konfigurációja lehet szükség, az alábbiak szerint: 
  
  * **SLES 12 SP1 HPC lemezkép** -Intel MPI-csomagok terjesztése a virtuális Gépen. Telepítse a következő parancs futtatásával:

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * **HPC-lemezképek centOS alapú** -Intel MPI 5.1 már telepítve van.  
    
    További rendszerkonfiguráció MPI-feladatok futtatása a fürtözött virtuális gépeken van szükség. Például egy fürtön található virtuális gépek szeretné a számítási csomópontok közötti megbízhatósági kapcsolat létrehozása. Tipikus beállítások, lásd: [MPI alkalmazások futtatásához hozzon létre egy Linux RDMA fürt](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="considerations-for-hpc-pack-and-linux"></a>HPC Pack és a Linux kapcsolatos szempontok
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft szabad HPC fürt és a feladat felügyeleti megoldás, a számítási igényű példányok használata a Linux és egy lehetőséget biztosít. HPC Pack támogatási legújabb kiadásaiban futtathatnak több Linux terjesztésekről számítási csomópontok telepítve az Azure virtuális gépeken, a Windows Server átjárócsomópont kezeli. Az RDMA-kompatibilis Linux számítási csomópontok Intel MPI fut, a HPC Pack is ütemezése és futtatása Linux MPI az RDMA hálózati elérhető alkalmazásokat. Első lépésként tekintse meg a [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="network-topology-considerations"></a>Hálózati topológia szempontjai
* Az RDMA-kompatibilis Linux virtuális gépeken futó Azure-ban Eth1 RDMA hálózati forgalom számára van fenntartva. Ne módosítsa a Eth1 beállításokat vagy a konfigurációs fájl hivatkozó ezen a hálózaton található bármely információ. Eth0 rendszeres Azure hálózati forgalom számára van fenntartva.
* Az Azure IP keresztül InfiniBand (IB) nem támogatott. Csak IB feletti RDMA esetén támogatott.



## <a name="next-steps"></a>Következő lépések
* További információk a elérhetősége és ára a számítási igényű méretű: [virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).
* Tárolási kapacitás és a lemez adatai: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Első lépésként telepítésére és használatára számítási igényű méretek és rdma-t, Linux, lásd: [MPI alkalmazások futtatásához hozzon létre egy Linux RDMA fürt](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

