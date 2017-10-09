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
# <a name="high-performance-compute-linux-vm-sizes"></a><span data-ttu-id="72211-103">Nagy teljesítményű számítási Linux Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="72211-103">High performance compute Linux VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a><span data-ttu-id="72211-104">Az RDMA-kompatibilis példányok</span><span class="sxs-lookup"><span data-stu-id="72211-104">RDMA-capable instances</span></span>
<span data-ttu-id="72211-105">Egy részhalmazát hello számítási igényű példányok (H16r, H16mr, A8 és A9) beállítást, a távoli közvetlen memória-hozzáférés (RDMA) hálózati kapcsolatot a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="72211-105">A subset of hello compute-intensive instances (H16r, H16mr, A8, and A9) feature a network interface for remote direct memory access (RDMA) connectivity.</span></span> <span data-ttu-id="72211-106">Ez az interfész továbbá toohello szabványos Azure hálózati illesztő elérhető tooother Virtuálisgép-méretek.</span><span class="sxs-lookup"><span data-stu-id="72211-106">This interface is in addition toohello standard Azure network interface available tooother VM sizes.</span></span> 
  
<span data-ttu-id="72211-107">Ez az interfész lehetővé teszi, hogy hello RDMA-kompatibilis példányok toocommunicate InfiniBand-hálózaton, működő FDR sebességét H16r és H16mr virtuális gépek és a gyorsító A8 és A9 virtuális gépek QDR sebességet.</span><span class="sxs-lookup"><span data-stu-id="72211-107">This interface allows hello RDMA-capable instances toocommunicate over an InfiniBand network, operating at FDR rates for H16r and H16mr virtual machines, and QDR rates for A8 and A9 virtual machines.</span></span> <span data-ttu-id="72211-108">E RDMA-képességeinek képes javítani a Message Passing Interface (MPI) alkalmazások hello méretezhetőségére és teljesítményére.</span><span class="sxs-lookup"><span data-stu-id="72211-108">These RDMA capabilities can boost hello scalability and performance of Message Passing Interface (MPI) applications.</span></span>

<span data-ttu-id="72211-109">Az alábbiakban követelmények az RDMA-kompatibilis Linux virtuális gépek tooaccess hello Azure RDMA hálózati:</span><span class="sxs-lookup"><span data-stu-id="72211-109">Following are requirements for RDMA-capable Linux VMs tooaccess hello Azure RDMA network:</span></span>
 
* <span data-ttu-id="72211-110">**Azokat a terjesztéseket** - telepítenie kell a virtuális gépek az RDMA-kompatibilis SUSE Linux Enterprise Server (SLES), vagy rosszindulatú Wave szoftver (korábbi nevén OpenLogic) – CentOS-alapú HPC képek hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="72211-110">**Distributions** - You must deploy VMs from RDMA-capable SUSE Linux Enterprise Server (SLES) or Rogue Wave Software (formerly OpenLogic) CentOS-based HPC images in hello Azure Marketplace.</span></span> <span data-ttu-id="72211-111">hello következő piactéren elérhető rendszerkép támogatják az RDMA-kapcsolatot:</span><span class="sxs-lookup"><span data-stu-id="72211-111">hello following Marketplace images support RDMA connectivity:</span></span>
  
    * <span data-ttu-id="72211-112">SLES 12 HPC, vagy a SLES 12 SP1 SP1 HPC (prémium)</span><span class="sxs-lookup"><span data-stu-id="72211-112">SLES 12 SP1 for HPC, or SLES 12 SP1 for HPC (Premium)</span></span>
    
    * <span data-ttu-id="72211-113">7.3 HPC centOS-alapú, 7.1-es HPC CentOS-alapú, 6,8 HPC CentOS-alapú vagy 6.5 HPC CentOS-alapú</span><span class="sxs-lookup"><span data-stu-id="72211-113">CentOS-based 7.3 HPC, CentOS-based 7.1 HPC, CentOS-based 6.8 HPC, or CentOS-based 6.5 HPC</span></span>  
 
        > [!NOTE]
        > <span data-ttu-id="72211-114">H sorozatú virtuális gépekhez javasoljuk a SLES 12 SP1 HPC lemezkép vagy a CentOS-alapú 7.1-es vagy a későbbi HPC-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="72211-114">For H-series VMs, we recommend either a SLES 12 SP1 for HPC image or CentOS-based 7.1 or later HPC image.</span></span>
        >
        > <span data-ttu-id="72211-115">A hello CentOS alapú HPC-lemezképek, a kernel frissítések le vannak tiltva a hello **yum** konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="72211-115">On hello CentOS-based HPC images, kernel updates are disabled in hello **yum** configuration file.</span></span> <span data-ttu-id="72211-116">Ennek az az oka hello Linux RDMA illesztőprogramok képezi RPM-csomag, és előfordulhat, hogy nem működik a illesztőprogram-frissítést, ha hello kernel frissül.</span><span class="sxs-lookup"><span data-stu-id="72211-116">This is because hello Linux RDMA drivers are distributed as an RPM package, and driver updates might not work if hello kernel is updated.</span></span>
        > 
        > 
* <span data-ttu-id="72211-117">**MPI** -Intel MPI könyvtár 5.x</span><span class="sxs-lookup"><span data-stu-id="72211-117">**MPI** - Intel MPI Library 5.x</span></span>
  
    <span data-ttu-id="72211-118">Hello Piactéri lemezképhez választott, külön licencelési, a telepítés vagy Intel MPI konfigurációja lehet szükség, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="72211-118">Depending on hello Marketplace image you choose, separate licensing, installation, or configuration of Intel MPI may be needed, as follows:</span></span> 
  
  * <span data-ttu-id="72211-119">**SLES 12 SP1 HPC lemezkép** -Intel MPI-csomagok terjesztése a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="72211-119">**SLES 12 SP1 for HPC image** - Intel MPI packages are distributed on hello VM.</span></span> <span data-ttu-id="72211-120">Telepítse a hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="72211-120">Install by running hello following command:</span></span>

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * <span data-ttu-id="72211-121">**HPC-lemezképek centOS alapú** -Intel MPI 5.1 már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="72211-121">**CentOS-based HPC images**  - Intel MPI 5.1 is already installed.</span></span>  
    
    <span data-ttu-id="72211-122">További rendszerkonfiguráció szükséges toorun MPI-feladatok a fürtözött virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="72211-122">Additional system configuration is needed toorun MPI jobs on clustered VMs.</span></span> <span data-ttu-id="72211-123">Például egy fürtön található virtuális gépek kell tooestablish hello között megbízhatósági számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="72211-123">For example, on a cluster of VMs, you need tooestablish trust among hello compute nodes.</span></span> <span data-ttu-id="72211-124">Tipikus beállítások, lásd: [egy Linux RDMA fürt toorun MPI alkalmazások beállítása](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="72211-124">For typical settings, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

### <a name="network-topology-considerations"></a><span data-ttu-id="72211-125">Hálózati topológia szempontjai</span><span class="sxs-lookup"><span data-stu-id="72211-125">Network topology considerations</span></span>
* <span data-ttu-id="72211-126">Az RDMA-kompatibilis Linux virtuális gépeken futó Azure-ban Eth1 RDMA hálózati forgalom számára van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="72211-126">On RDMA-enabled Linux VMs in Azure, Eth1 is reserved for RDMA network traffic.</span></span> <span data-ttu-id="72211-127">Ne módosítsa a Eth1 beállításokat vagy semmilyen információt toothis hálózati utaló hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="72211-127">Do not change any Eth1 settings or any information in hello configuration file referring toothis network.</span></span> <span data-ttu-id="72211-128">Eth0 rendszeres Azure hálózati forgalom számára van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="72211-128">Eth0 is reserved for regular Azure network traffic.</span></span>

* <span data-ttu-id="72211-129">Az Azure IP keresztül InfiniBand (IB) nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="72211-129">In Azure, IP over InfiniBand (IB) is not supported.</span></span> <span data-ttu-id="72211-130">Csak IB feletti RDMA esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="72211-130">Only RDMA over IB is supported.</span></span>

## <a name="using-hpc-pack"></a><span data-ttu-id="72211-131">HPC Pack használatával</span><span class="sxs-lookup"><span data-stu-id="72211-131">Using HPC Pack</span></span>
<span data-ttu-id="72211-132">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft szabad HPC fürt és a feladat felügyeleti megoldás, a beállítás egy a toouse hello számítási igényű példányok Linux meg.</span><span class="sxs-lookup"><span data-stu-id="72211-132">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, is one option for you toouse hello compute-intensive instances with Linux.</span></span> <span data-ttu-id="72211-133">HPC Pack legújabb kiadásaiban hello toorun a számítási csomópontok az Azure virtuális gépeken, a Windows Server átjárócsomópont által kezelt telepített számos Linux terjesztésekről támogatja.</span><span class="sxs-lookup"><span data-stu-id="72211-133">hello latest releases of HPC Pack support several Linux distributions toorun on compute nodes deployed in Azure VMs, managed by a Windows Server head node.</span></span> <span data-ttu-id="72211-134">RDMA-kompatibilis Linux számítási csomópontok Intel MPI fut, a HPC Pack ütemezése és Linux MPI alkalmazások futtatását, hogy hozzáférési hello RDMA hálózati.</span><span class="sxs-lookup"><span data-stu-id="72211-134">With RDMA-capable Linux compute nodes running Intel MPI, HPC Pack can schedule and run Linux MPI applications that access hello RDMA network.</span></span> <span data-ttu-id="72211-135">Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="72211-135">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="other-sizes"></a><span data-ttu-id="72211-136">Más méretek</span><span class="sxs-lookup"><span data-stu-id="72211-136">Other sizes</span></span>
- [<span data-ttu-id="72211-137">Általános célú</span><span class="sxs-lookup"><span data-stu-id="72211-137">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="72211-138">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="72211-138">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="72211-139">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="72211-139">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="72211-140">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="72211-140">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="72211-141">GPU</span><span class="sxs-lookup"><span data-stu-id="72211-141">GPU</span></span>](../windows/sizes-gpu.md)


## <a name="next-steps"></a><span data-ttu-id="72211-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72211-142">Next steps</span></span>

- <span data-ttu-id="72211-143">tooget lépések telepítésére és használatára számítási igényű méretek és rdma-t, Linux, lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="72211-143">tooget started deploying and using compute-intensive sizes with RDMA on Linux, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

- <span data-ttu-id="72211-144">További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="72211-144">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>




