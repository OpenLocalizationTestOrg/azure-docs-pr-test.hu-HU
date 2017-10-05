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
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a><span data-ttu-id="b7817-103">H-sorozat és számítási igényű A-sorozatú virtuális gépeken Linux kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="b7817-103">About H-series and compute-intensive A-series VMs for Linux</span></span>
<span data-ttu-id="b7817-104">Íme a háttér-információkat és az újabb Azure H-adatsorozat és a korábbi A8, A9, A10 és A11 méretű, más néven használatával kapcsolatos megfontolandó *számítási igényű* példányok.</span><span class="sxs-lookup"><span data-stu-id="b7817-104">Here is background information and some considerations for using the newer Azure H-series and the earlier A8, A9, A10, and A11 sizes, also known as *compute-intensive* instances.</span></span> <span data-ttu-id="b7817-105">Ez a cikk foglalkozik, ezek mérete használatáról Linux virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="b7817-105">This article focuses on using these sizes for Linux VMs.</span></span> <span data-ttu-id="b7817-106">Használhatja őket [Windows virtuális gépek](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7817-106">You can also use them for [Windows VMs](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="b7817-107">Alapvető specifikációk, tárolási kapacitás és a lemez adatai: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7817-107">For basic specs, storage capacities, and disk details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a><span data-ttu-id="b7817-108">Az RDMA hálózati hozzáférés</span><span class="sxs-lookup"><span data-stu-id="b7817-108">Access to the RDMA network</span></span>
<span data-ttu-id="b7817-109">Az RDMA-kompatibilis Linux virtuális gépek, hogy a következő futtatási egyik támogatott Linux HPC disztribúcióiról, valamint a támogatott MPI megvalósítását, hogy kihasználja az Azure RDMA hálózati tárolófürtöket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b7817-109">You can create clusters of RDMA-capable Linux VMs that run one of the following supported Linux HPC distributions and a supported MPI implementation to take advantage of the Azure RDMA network.</span></span> <span data-ttu-id="b7817-110">Lásd: [MPI alkalmazások futtatásához hozzon létre egy Linux RDMA fürt](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) üzembe helyezési lehetőségei és minta konfigurációs lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b7817-110">See [Set up a Linux RDMA cluster to run MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) for deployment options and sample configuration steps.</span></span>

* <span data-ttu-id="b7817-111">**Azokat a terjesztéseket** -virtuális gépek telepítenie kell az RDMA-kompatibilis SUSE Linux Enterprise Server (SLES) vagy rosszindulatú Wave szoftver (korábbi nevén OpenLogic) – CentOS-alapú HPC lemezképet az Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="b7817-111">**Distributions** - You must deploy VMs from RDMA-capable SUSE Linux Enterprise Server (SLES) or Rogue Wave Software (formerly OpenLogic) CentOS-based HPC images in the Azure Marketplace.</span></span> <span data-ttu-id="b7817-112">A következő piactéren elérhető rendszerkép támogatják az RDMA-kapcsolatot:</span><span class="sxs-lookup"><span data-stu-id="b7817-112">The following Marketplace images support RDMA connectivity:</span></span>
  
    * <span data-ttu-id="b7817-113">SLES 12 HPC, SLES 12 SP1 SP1 HPC (prémium)</span><span class="sxs-lookup"><span data-stu-id="b7817-113">SLES 12 SP1 for HPC, SLES 12 SP1 for HPC (Premium)</span></span>
    
    * <span data-ttu-id="b7817-114">7.1-es HPC centOS-alapú, 6.5 HPC CentOS-alapú</span><span class="sxs-lookup"><span data-stu-id="b7817-114">CentOS-based 7.1 HPC, CentOS-based 6.5 HPC</span></span>  
 
        > [!NOTE]
        > <span data-ttu-id="b7817-115">H sorozatú virtuális gépekhez javasoljuk a SLES 12 SP1 HPC lemezképhez vagy 7.1-es HPC CentOS alapú lemezkép.</span><span class="sxs-lookup"><span data-stu-id="b7817-115">For H-series VMs, we recommend either a SLES 12 SP1 for HPC image or CentOS-based 7.1 HPC image.</span></span>
        >
        > <span data-ttu-id="b7817-116">A CentOS-alapú HPC-lemezképek, a kernel frissítések le vannak tiltva a **yum** konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="b7817-116">On the CentOS-based HPC images, kernel updates are disabled in the **yum** configuration file.</span></span> <span data-ttu-id="b7817-117">Ennek az az oka egy RPM-csomag terjesztése a Linux RDMA-illesztőprogramok és illesztőprogram-frissítést nem lehet, hogy működik, ha a kernel frissül.</span><span class="sxs-lookup"><span data-stu-id="b7817-117">This is because the Linux RDMA drivers are distributed as an RPM package, and driver updates might not work if the kernel is updated.</span></span>
        > 
        > 
* <span data-ttu-id="b7817-118">**MPI** -Intel MPI könyvtár 5.x</span><span class="sxs-lookup"><span data-stu-id="b7817-118">**MPI** - Intel MPI Library 5.x</span></span>
  
    <span data-ttu-id="b7817-119">A Piactéri lemezképhez választott, külön licencelési, a telepítés vagy Intel MPI konfigurációja lehet szükség, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b7817-119">Depending on the Marketplace image you choose, separate licensing, installation, or configuration of Intel MPI may be needed, as follows:</span></span> 
  
  * <span data-ttu-id="b7817-120">**SLES 12 SP1 HPC lemezkép** -Intel MPI-csomagok terjesztése a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="b7817-120">**SLES 12 SP1 for HPC image** - Intel MPI packages are distributed on the VM.</span></span> <span data-ttu-id="b7817-121">Telepítse a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b7817-121">Install by running the following command:</span></span>

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * <span data-ttu-id="b7817-122">**HPC-lemezképek centOS alapú** -Intel MPI 5.1 már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b7817-122">**CentOS-based HPC images**  - Intel MPI 5.1 is already installed.</span></span>  
    
    <span data-ttu-id="b7817-123">További rendszerkonfiguráció MPI-feladatok futtatása a fürtözött virtuális gépeken van szükség.</span><span class="sxs-lookup"><span data-stu-id="b7817-123">Additional system configuration is needed to run MPI jobs on clustered VMs.</span></span> <span data-ttu-id="b7817-124">Például egy fürtön található virtuális gépek szeretné a számítási csomópontok közötti megbízhatósági kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b7817-124">For example, on a cluster of VMs, you need to establish trust among the compute nodes.</span></span> <span data-ttu-id="b7817-125">Tipikus beállítások, lásd: [MPI alkalmazások futtatásához hozzon létre egy Linux RDMA fürt](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7817-125">For typical settings, see [Set up a Linux RDMA cluster to run MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="considerations-for-hpc-pack-and-linux"></a><span data-ttu-id="b7817-126">HPC Pack és a Linux kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="b7817-126">Considerations for HPC Pack and Linux</span></span>
<span data-ttu-id="b7817-127">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft szabad HPC fürt és a feladat felügyeleti megoldás, a számítási igényű példányok használata a Linux és egy lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="b7817-127">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, provides one option for you to use the compute-intensive instances with Linux.</span></span> <span data-ttu-id="b7817-128">HPC Pack támogatási legújabb kiadásaiban futtathatnak több Linux terjesztésekről számítási csomópontok telepítve az Azure virtuális gépeken, a Windows Server átjárócsomópont kezeli.</span><span class="sxs-lookup"><span data-stu-id="b7817-128">The latest releases of HPC Pack support several Linux distributions to run on compute nodes deployed in Azure VMs, managed by a Windows Server head node.</span></span> <span data-ttu-id="b7817-129">Az RDMA-kompatibilis Linux számítási csomópontok Intel MPI fut, a HPC Pack is ütemezése és futtatása Linux MPI az RDMA hálózati elérhető alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="b7817-129">With RDMA-capable Linux compute nodes running Intel MPI, HPC Pack can schedule and run Linux MPI applications that access the RDMA network.</span></span> <span data-ttu-id="b7817-130">Első lépésként tekintse meg a [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7817-130">To get started, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="network-topology-considerations"></a><span data-ttu-id="b7817-131">Hálózati topológia szempontjai</span><span class="sxs-lookup"><span data-stu-id="b7817-131">Network topology considerations</span></span>
* <span data-ttu-id="b7817-132">Az RDMA-kompatibilis Linux virtuális gépeken futó Azure-ban Eth1 RDMA hálózati forgalom számára van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="b7817-132">On RDMA-enabled Linux VMs in Azure, Eth1 is reserved for RDMA network traffic.</span></span> <span data-ttu-id="b7817-133">Ne módosítsa a Eth1 beállításokat vagy a konfigurációs fájl hivatkozó ezen a hálózaton található bármely információ.</span><span class="sxs-lookup"><span data-stu-id="b7817-133">Do not change any Eth1 settings or any information in the configuration file referring to this network.</span></span> <span data-ttu-id="b7817-134">Eth0 rendszeres Azure hálózati forgalom számára van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="b7817-134">Eth0 is reserved for regular Azure network traffic.</span></span>
* <span data-ttu-id="b7817-135">Az Azure IP keresztül InfiniBand (IB) nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b7817-135">In Azure, IP over InfiniBand (IB) is not supported.</span></span> <span data-ttu-id="b7817-136">Csak IB feletti RDMA esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="b7817-136">Only RDMA over IB is supported.</span></span>



## <a name="next-steps"></a><span data-ttu-id="b7817-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7817-137">Next steps</span></span>
* <span data-ttu-id="b7817-138">További információk a elérhetősége és ára a számítási igényű méretű: [virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span><span class="sxs-lookup"><span data-stu-id="b7817-138">For details about availability and pricing of the compute-intensive sizes, see [Virtual Machines pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span></span>
* <span data-ttu-id="b7817-139">Tárolási kapacitás és a lemez adatai: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7817-139">For storage capacities and disk details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="b7817-140">Első lépésként telepítésére és használatára számítási igényű méretek és rdma-t, Linux, lásd: [MPI alkalmazások futtatásához hozzon létre egy Linux RDMA fürt](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7817-140">To get started deploying and using compute-intensive sizes with RDMA on Linux, see [Set up a Linux RDMA cluster to run MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

