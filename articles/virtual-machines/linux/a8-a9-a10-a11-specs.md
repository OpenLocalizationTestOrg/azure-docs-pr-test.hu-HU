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
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a><span data-ttu-id="a1072-103">H-sorozat és számítási igényű A-sorozatú virtuális gépeken Linux kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="a1072-103">About H-series and compute-intensive A-series VMs for Linux</span></span>
<span data-ttu-id="a1072-104">Itt háttér-információkat, és használatával kapcsolatos megfontolandó újabb Azure H-sorozat hello korábbi A8, A9, A10 és A11 méretű, más néven hello *számítási igényű* példányok.</span><span class="sxs-lookup"><span data-stu-id="a1072-104">Here is background information and some considerations for using hello newer Azure H-series and hello earlier A8, A9, A10, and A11 sizes, also known as *compute-intensive* instances.</span></span> <span data-ttu-id="a1072-105">Ez a cikk foglalkozik, ezek mérete használatáról Linux virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="a1072-105">This article focuses on using these sizes for Linux VMs.</span></span> <span data-ttu-id="a1072-106">Használhatja őket [Windows virtuális gépek](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a1072-106">You can also use them for [Windows VMs](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="a1072-107">Alapvető specifikációk, tárolási kapacitás és a lemez adatai: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a1072-107">For basic specs, storage capacities, and disk details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-toohello-rdma-network"></a><span data-ttu-id="a1072-108">Hozzáférés toohello RDMA hálózati</span><span class="sxs-lookup"><span data-stu-id="a1072-108">Access toohello RDMA network</span></span>
<span data-ttu-id="a1072-109">Az RDMA-kompatibilis Linux virtuális gépek a következő támogatott Linux HPC disztribúcióiról, valamint a támogatott MPI megvalósítási tootake előnyeit hello Azure RDMA hálózati hello valamelyikét futtató tárolófürtöket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="a1072-109">You can create clusters of RDMA-capable Linux VMs that run one of hello following supported Linux HPC distributions and a supported MPI implementation tootake advantage of hello Azure RDMA network.</span></span> <span data-ttu-id="a1072-110">Lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) üzembe helyezési lehetőségei és minta konfigurációs lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a1072-110">See [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) for deployment options and sample configuration steps.</span></span>

* <span data-ttu-id="a1072-111">**Azokat a terjesztéseket** - telepítenie kell a virtuális gépek az RDMA-kompatibilis SUSE Linux Enterprise Server (SLES), vagy rosszindulatú Wave szoftver (korábbi nevén OpenLogic) – CentOS-alapú HPC képek hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="a1072-111">**Distributions** - You must deploy VMs from RDMA-capable SUSE Linux Enterprise Server (SLES) or Rogue Wave Software (formerly OpenLogic) CentOS-based HPC images in hello Azure Marketplace.</span></span> <span data-ttu-id="a1072-112">hello következő piactéren elérhető rendszerkép támogatják az RDMA-kapcsolatot:</span><span class="sxs-lookup"><span data-stu-id="a1072-112">hello following Marketplace images support RDMA connectivity:</span></span>
  
    * <span data-ttu-id="a1072-113">SLES 12 HPC, SLES 12 SP1 SP1 HPC (prémium)</span><span class="sxs-lookup"><span data-stu-id="a1072-113">SLES 12 SP1 for HPC, SLES 12 SP1 for HPC (Premium)</span></span>
    
    * <span data-ttu-id="a1072-114">7.1-es HPC centOS-alapú, 6.5 HPC CentOS-alapú</span><span class="sxs-lookup"><span data-stu-id="a1072-114">CentOS-based 7.1 HPC, CentOS-based 6.5 HPC</span></span>  
 
        > [!NOTE]
        > <span data-ttu-id="a1072-115">H sorozatú virtuális gépekhez javasoljuk a SLES 12 SP1 HPC lemezképhez vagy 7.1-es HPC CentOS alapú lemezkép.</span><span class="sxs-lookup"><span data-stu-id="a1072-115">For H-series VMs, we recommend either a SLES 12 SP1 for HPC image or CentOS-based 7.1 HPC image.</span></span>
        >
        > <span data-ttu-id="a1072-116">A hello CentOS alapú HPC-lemezképek, a kernel frissítések le vannak tiltva a hello **yum** konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="a1072-116">On hello CentOS-based HPC images, kernel updates are disabled in hello **yum** configuration file.</span></span> <span data-ttu-id="a1072-117">Ennek az az oka hello Linux RDMA illesztőprogramok képezi RPM-csomag, és előfordulhat, hogy nem működik a illesztőprogram-frissítést, ha hello kernel frissül.</span><span class="sxs-lookup"><span data-stu-id="a1072-117">This is because hello Linux RDMA drivers are distributed as an RPM package, and driver updates might not work if hello kernel is updated.</span></span>
        > 
        > 
* <span data-ttu-id="a1072-118">**MPI** -Intel MPI könyvtár 5.x</span><span class="sxs-lookup"><span data-stu-id="a1072-118">**MPI** - Intel MPI Library 5.x</span></span>
  
    <span data-ttu-id="a1072-119">Hello Piactéri lemezképhez választott, külön licencelési, a telepítés vagy Intel MPI konfigurációja lehet szükség, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a1072-119">Depending on hello Marketplace image you choose, separate licensing, installation, or configuration of Intel MPI may be needed, as follows:</span></span> 
  
  * <span data-ttu-id="a1072-120">**SLES 12 SP1 HPC lemezkép** -Intel MPI-csomagok terjesztése a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="a1072-120">**SLES 12 SP1 for HPC image** - Intel MPI packages are distributed on hello VM.</span></span> <span data-ttu-id="a1072-121">Telepítse a hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a1072-121">Install by running hello following command:</span></span>

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * <span data-ttu-id="a1072-122">**HPC-lemezképek centOS alapú** -Intel MPI 5.1 már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="a1072-122">**CentOS-based HPC images**  - Intel MPI 5.1 is already installed.</span></span>  
    
    <span data-ttu-id="a1072-123">További rendszerkonfiguráció szükséges toorun MPI-feladatok a fürtözött virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="a1072-123">Additional system configuration is needed toorun MPI jobs on clustered VMs.</span></span> <span data-ttu-id="a1072-124">Például egy fürtön található virtuális gépek kell tooestablish hello között megbízhatósági számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="a1072-124">For example, on a cluster of VMs, you need tooestablish trust among hello compute nodes.</span></span> <span data-ttu-id="a1072-125">Tipikus beállítások, lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a1072-125">For typical settings, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="considerations-for-hpc-pack-and-linux"></a><span data-ttu-id="a1072-126">HPC Pack és a Linux kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="a1072-126">Considerations for HPC Pack and Linux</span></span>
<span data-ttu-id="a1072-127">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft szabad HPC fürt és a feladat felügyeleti megoldás, biztosít egy lehetőséget az Ön rendelkező Linux toouse hello számítási igényű példányai.</span><span class="sxs-lookup"><span data-stu-id="a1072-127">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, provides one option for you toouse hello compute-intensive instances with Linux.</span></span> <span data-ttu-id="a1072-128">HPC Pack legújabb kiadásaiban hello toorun a számítási csomópontok az Azure virtuális gépeken, a Windows Server átjárócsomópont által kezelt telepített számos Linux terjesztésekről támogatja.</span><span class="sxs-lookup"><span data-stu-id="a1072-128">hello latest releases of HPC Pack support several Linux distributions toorun on compute nodes deployed in Azure VMs, managed by a Windows Server head node.</span></span> <span data-ttu-id="a1072-129">RDMA-kompatibilis Linux számítási csomópontok Intel MPI fut, a HPC Pack ütemezése és Linux MPI alkalmazások futtatását, hogy hozzáférési hello RDMA hálózati.</span><span class="sxs-lookup"><span data-stu-id="a1072-129">With RDMA-capable Linux compute nodes running Intel MPI, HPC Pack can schedule and run Linux MPI applications that access hello RDMA network.</span></span> <span data-ttu-id="a1072-130">tooget indult el, lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a1072-130">tooget started, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="network-topology-considerations"></a><span data-ttu-id="a1072-131">Hálózati topológia szempontjai</span><span class="sxs-lookup"><span data-stu-id="a1072-131">Network topology considerations</span></span>
* <span data-ttu-id="a1072-132">Az RDMA-kompatibilis Linux virtuális gépeken futó Azure-ban Eth1 RDMA hálózati forgalom számára van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="a1072-132">On RDMA-enabled Linux VMs in Azure, Eth1 is reserved for RDMA network traffic.</span></span> <span data-ttu-id="a1072-133">Ne módosítsa a Eth1 beállításokat vagy semmilyen információt toothis hálózati utaló hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="a1072-133">Do not change any Eth1 settings or any information in hello configuration file referring toothis network.</span></span> <span data-ttu-id="a1072-134">Eth0 rendszeres Azure hálózati forgalom számára van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="a1072-134">Eth0 is reserved for regular Azure network traffic.</span></span>
* <span data-ttu-id="a1072-135">Az Azure IP keresztül InfiniBand (IB) nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="a1072-135">In Azure, IP over InfiniBand (IB) is not supported.</span></span> <span data-ttu-id="a1072-136">Csak IB feletti RDMA esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="a1072-136">Only RDMA over IB is supported.</span></span>



## <a name="next-steps"></a><span data-ttu-id="a1072-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1072-137">Next steps</span></span>
* <span data-ttu-id="a1072-138">További információk a elérhetősége és ára hello számítási igényű méretű: [virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span><span class="sxs-lookup"><span data-stu-id="a1072-138">For details about availability and pricing of hello compute-intensive sizes, see [Virtual Machines pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span></span>
* <span data-ttu-id="a1072-139">Tárolási kapacitás és a lemez adatai: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a1072-139">For storage capacities and disk details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="a1072-140">tooget lépések telepítésére és használatára számítási igényű méretek és rdma-t, Linux, lásd: [állítson be egy Linux RDMA fürt toorun MPI alkalmazások](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a1072-140">tooget started deploying and using compute-intensive sizes with RDMA on Linux, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

