---
title: "illesztőprogram-telepítő aaaAzure N-sorozat a Windows |} Microsoft Docs"
description: "Hogyan tooset N sorozatú virtuális gépek Azure-beli Windows NVIDIA GPU-illesztőprogram"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/07/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2acce57d4f9eb1d02bf3bc0303bcb9690475698c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a><span data-ttu-id="80823-103">Windows Server rendszert futtató N sorozatú virtuális gépek GPU illesztőprogramok beállítása</span><span class="sxs-lookup"><span data-stu-id="80823-103">Set up GPU drivers for N-series VMs running Windows Server</span></span>
<span data-ttu-id="80823-104">hello GPU képességek az Azure-N-adatsorozat tootake előnyeit a virtuális gépek Windows Server 2016 vagy Windows Server 2012 R2 fut, a telepítés támogatott NVIDIA video-illesztőprogramok.</span><span class="sxs-lookup"><span data-stu-id="80823-104">tootake advantage of hello GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, install supported NVIDIA graphics drivers.</span></span> <span data-ttu-id="80823-105">Ez a cikk lépéseit illesztőprogram beállítása az N-sorozatú virtuális gép telepítése után.</span><span class="sxs-lookup"><span data-stu-id="80823-105">This article provides driver setup steps after you deploy an N-series VM.</span></span> <span data-ttu-id="80823-106">Telepítési információk illesztőprogram érhető el is [Linux virtuális gépek](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="80823-106">Driver setup information is also available for [Linux VMs](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="80823-107">Alapvető specifikációk, tárolási kapacitás és a lemez adatai: [GPU Windows Virtuálisgép-méretek](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="80823-107">For basic specs, storage capacities, and disk details, see [GPU Windows VM sizes](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a><span data-ttu-id="80823-108">Illesztőprogram telepítése</span><span class="sxs-lookup"><span data-stu-id="80823-108">Driver installation</span></span>

1. <span data-ttu-id="80823-109">Csatlakozzon a távoli asztal tooeach N sorozatú virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="80823-109">Connect by Remote Desktop tooeach N-series VM.</span></span>

2. <span data-ttu-id="80823-110">Töltse le, bontsa ki és hello támogatott illesztőprogramját a Windows operációs rendszert.</span><span class="sxs-lookup"><span data-stu-id="80823-110">Download, extract, and install hello supported driver for your Windows operating system.</span></span>

<span data-ttu-id="80823-111">Azure virtuális gépeken portok HV újraindításra szükség az illesztőprogram telepítése után.</span><span class="sxs-lookup"><span data-stu-id="80823-111">On Azure NV VMs, a restart is required after driver installation.</span></span> <span data-ttu-id="80823-112">A hálózati vezérlő által virtuális gépeken újraindításra szükség.</span><span class="sxs-lookup"><span data-stu-id="80823-112">On NC VMs, a restart is not required.</span></span>

## <a name="verify-driver-installation"></a><span data-ttu-id="80823-113">Illesztőprogram telepítésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="80823-113">Verify driver installation</span></span>

<span data-ttu-id="80823-114">Illesztőprogram-telepítőfájl az Eszközkezelőben ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="80823-114">You can verify driver installation in Device Manager.</span></span> <span data-ttu-id="80823-115">hello következő példa bemutatja hello Tesla K80 kártya sikeres konfigurációs egy Azure NC virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="80823-115">hello following example shows successful configuration of hello Tesla K80 card on an Azure NC VM.</span></span>

![GPU illesztőprogram tulajdonságai](./media/n-series-driver-setup/GPU_driver_properties.png)

<span data-ttu-id="80823-117">tooquery hello GPU az eszköz állapotát, futtassa a hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) parancssori segédprogram hello illesztőprogrammal telepítve.</span><span class="sxs-lookup"><span data-stu-id="80823-117">tooquery hello GPU device state, run hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with hello driver.</span></span>

1. <span data-ttu-id="80823-118">Nyisson meg egy parancssort, és módosítsa a toohello **C:\Program Files\NVIDIA Corporation\NVSMI** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="80823-118">Open a command prompt and change toohello **C:\Program Files\NVIDIA Corporation\NVSMI** directory.</span></span>

2. <span data-ttu-id="80823-119">Futtatás **nvidia-smi**.</span><span class="sxs-lookup"><span data-stu-id="80823-119">Run **nvidia-smi**.</span></span> <span data-ttu-id="80823-120">Ha telepítve van a hello illesztőprogram kimeneti hasonló toobelow jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="80823-120">If hello driver is installed you will see output similar toobelow.</span></span> <span data-ttu-id="80823-121">Vegye figyelembe, hogy **GPU-Util** látható **0 %** kivéve, ha jelenleg futtatja a GPU munkaterhelés hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="80823-121">Note that **GPU-Util** shows **0%** unless you are currently running a GPU workload on hello VM.</span></span>

![NVIDIA eszköz állapota](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a><span data-ttu-id="80823-123">RDMA hálózati NC24r virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="80823-123">RDMA network for NC24r VMs</span></span>

<span data-ttu-id="80823-124">RDMA hálózati kapcsolat engedélyezhető a hello telepített NC24r virtuális gépek azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="80823-124">RDMA network connectivity can be enabled on NC24r VMs deployed in hello same availability set.</span></span> <span data-ttu-id="80823-125">hello HpcVmDrivers bővítmény tooinstall Windows hálózati eszköz-illesztőprogramok, amelyek lehetővé teszik az RDMA-kapcsolatot hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="80823-125">hello HpcVmDrivers extension must be added tooinstall Windows network device drivers that enable RDMA connectivity.</span></span> <span data-ttu-id="80823-126">tooadd hello VM bővítmény tooan NC24r VM használja [Azure PowerShell](/powershell/azure/overview) az Azure Resource Manager parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="80823-126">tooadd hello VM extension tooan NC24r VM, use [Azure PowerShell](/powershell/azure/overview) cmdlets for Azure Resource Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="80823-127">Jelenleg csak Windows Server 2012 R2 támogatja hello RDMA hálózati NC24r virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="80823-127">Currently, only Windows Server 2012 R2 supports hello RDMA network on NC24r VMs.</span></span>
> 

<span data-ttu-id="80823-128">tooinstall hello legújabb 1.1-es verzió HpcVMDrivers kiterjesztése a meglévő RDMA-kompatibilis virtuális hello USA nyugati régiója régióban myVM nevű virtuális:</span><span class="sxs-lookup"><span data-stu-id="80823-128">tooinstall hello latest version 1.1 HpcVMDrivers extension on an existing RDMA-capable VM named myVM in hello West US region:</span></span>
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  <span data-ttu-id="80823-129">További információkért lásd: [virtuálisgép-bővítmények és a Windows szolgáltatások](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="80823-129">For more information, see [Virtual machine extensions and features for Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="80823-130">hello RDMA hálózati Message Passing Interface (MPI) forgalmat támogatja a futó alkalmazások [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) vagy Intel MPI 5.x.</span><span class="sxs-lookup"><span data-stu-id="80823-130">hello RDMA network supports Message Passing Interface (MPI) traffic for applications running with [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) or Intel MPI 5.x.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="80823-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80823-131">Next steps</span></span>

* <span data-ttu-id="80823-132">A hello N sorozatú virtuális gépek hello NVIDIA Feldolgozóegységekkel kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="80823-132">For more information about hello NVIDIA GPUs on hello N-series VMs, see:</span></span>
    * <span data-ttu-id="80823-133">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (az Azure NC virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="80823-133">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (for Azure NC VMs)</span></span>
    * <span data-ttu-id="80823-134">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (az Azure Permanens virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="80823-134">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (for Azure NV VMs)</span></span>

* <span data-ttu-id="80823-135">Alkalmazások GPU-gyorsított hello NVIDIA Tesla Feldolgozóegységekkel a fejlesztők is letöltheti és telepítheti a CUDA eszközkészlet 8 hello [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) vagy [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe).</span><span class="sxs-lookup"><span data-stu-id="80823-135">Developers building GPU-accelerated applications for hello NVIDIA Tesla GPUs can also download and install hello CUDA Toolkit 8 for [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) or [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe).</span></span> <span data-ttu-id="80823-136">További információkért lásd: hello [CUDA a telepítési útmutató](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).</span><span class="sxs-lookup"><span data-stu-id="80823-136">For more information, see hello [CUDA Installation Guide](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).</span></span>


