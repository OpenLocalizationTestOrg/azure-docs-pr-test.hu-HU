---
title: "aaaAbout számítási igényű Linux virtuális gépekről |} Microsoft Docs"
description: "Háttér-információkat és a Linux virtuális gépek hello H-sorozat és A8, A9, A10, és A11 számítási igényű méret használatának szempontjai"
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
ms.openlocfilehash: 37636840a3f809ac19354a5a7993257216f675f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a>H-sorozat és számítási igényű A-sorozatú virtuális gépeken Linux kapcsolatos
Itt háttér-információkat, és használatával kapcsolatos megfontolandó újabb Azure H-sorozat hello korábbi A8, A9, A10 és A11 méretű, más néven hello *számítási igényű* példányok. Ez a cikk foglalkozik, ezek mérete használatáról Linux virtuális gépekhez. Használhatja őket [Windows virtuális gépek](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Alapvető specifikációk, tárolási kapacitás és a lemez adatai: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-toohello-rdma-network"></a>Hozzáférés toohello RDMA hálózati
Az RDMA-kompatibilis Linux virtuális gépek a következő támogatott Linux HPC disztribúcióiról, valamint a támogatott MPI megvalósítási tootake előnyeit hello Azure RDMA hálózati hello valamelyikét futtató tárolófürtöket hozhat létre. Lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) üzembe helyezési lehetőségei és minta konfigurációs lépéseket.

* **Azokat a terjesztéseket** - telepítenie kell a virtuális gépek az RDMA-kompatibilis SUSE Linux Enterprise Server (SLES), vagy rosszindulatú Wave szoftver (korábbi nevén OpenLogic) – CentOS-alapú HPC képek hello Azure piactéren. hello következő piactéren elérhető rendszerkép támogatják az RDMA-kapcsolatot:
  
    * SLES 12 HPC, SLES 12 SP1 SP1 HPC (prémium)
    
    * 7.1-es HPC centOS-alapú, 6.5 HPC CentOS-alapú  
 
        > [!NOTE]
        > H sorozatú virtuális gépekhez javasoljuk a SLES 12 SP1 HPC lemezképhez vagy 7.1-es HPC CentOS alapú lemezkép.
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
    
    További rendszerkonfiguráció szükséges toorun MPI-feladatok a fürtözött virtuális gépeken. Például egy fürtön található virtuális gépek kell tooestablish hello között megbízhatósági számítási csomópontjain. Tipikus beállítások, lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="considerations-for-hpc-pack-and-linux"></a>HPC Pack és a Linux kapcsolatos szempontok
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft szabad HPC fürt és a feladat felügyeleti megoldás, biztosít egy lehetőséget az Ön rendelkező Linux toouse hello számítási igényű példányai. HPC Pack legújabb kiadásaiban hello toorun a számítási csomópontok az Azure virtuális gépeken, a Windows Server átjárócsomópont által kezelt telepített számos Linux terjesztésekről támogatja. RDMA-kompatibilis Linux számítási csomópontok Intel MPI fut, a HPC Pack ütemezése és Linux MPI alkalmazások futtatását, hogy hozzáférési hello RDMA hálózati. tooget indult el, lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="network-topology-considerations"></a>Hálózati topológia szempontjai
* Az RDMA-kompatibilis Linux virtuális gépeken futó Azure-ban Eth1 RDMA hálózati forgalom számára van fenntartva. Ne módosítsa a Eth1 beállításokat vagy semmilyen információt toothis hálózati utaló hello konfigurációs fájlban. Eth0 rendszeres Azure hálózati forgalom számára van fenntartva.
* Az Azure IP keresztül InfiniBand (IB) nem támogatott. Csak IB feletti RDMA esetén támogatott.



## <a name="next-steps"></a>Következő lépések
* További információk a elérhetősége és ára hello számítási igényű méretű: [virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).
* Tárolási kapacitás és a lemez adatai: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* tooget lépések telepítésére és használatára számítási igényű méretek és rdma-t, Linux, lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

