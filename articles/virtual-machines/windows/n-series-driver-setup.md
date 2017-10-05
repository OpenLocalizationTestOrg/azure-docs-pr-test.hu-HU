---
title: "Windows Azure N-sorozat illesztőprogram beállítása |} Microsoft Docs"
description: "Windows Azure-ban futó N sorozatú virtuális gépek NVIDIA GPU illesztőprogramok beállítása"
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
ms.openlocfilehash: b480d10df777a2757c073ff77e1845d33d63163a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a><span data-ttu-id="19be6-103">Windows Server rendszert futtató N sorozatú virtuális gépek GPU illesztőprogramok beállítása</span><span class="sxs-lookup"><span data-stu-id="19be6-103">Set up GPU drivers for N-series VMs running Windows Server</span></span>
<span data-ttu-id="19be6-104">A Windows Server 2016 vagy a Windows Server 2012 R2 rendszert futtató Azure N sorozatú virtuális gépek GPU lehetőségeinek kihasználásához, telepítse a támogatott NVIDIA video-illesztőprogramok.</span><span class="sxs-lookup"><span data-stu-id="19be6-104">To take advantage of the GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, install supported NVIDIA graphics drivers.</span></span> <span data-ttu-id="19be6-105">Ez a cikk lépéseit illesztőprogram beállítása az N-sorozatú virtuális gép telepítése után.</span><span class="sxs-lookup"><span data-stu-id="19be6-105">This article provides driver setup steps after you deploy an N-series VM.</span></span> <span data-ttu-id="19be6-106">Telepítési információk illesztőprogram érhető el is [Linux virtuális gépek](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="19be6-106">Driver setup information is also available for [Linux VMs](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="19be6-107">Alapvető specifikációk, tárolási kapacitás és a lemez adatai: [GPU Windows Virtuálisgép-méretek](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="19be6-107">For basic specs, storage capacities, and disk details, see [GPU Windows VM sizes](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a><span data-ttu-id="19be6-108">Illesztőprogram telepítése</span><span class="sxs-lookup"><span data-stu-id="19be6-108">Driver installation</span></span>

1. <span data-ttu-id="19be6-109">Kapcsolódás az egyes N sorozatú virtuális gépek távoli asztalt.</span><span class="sxs-lookup"><span data-stu-id="19be6-109">Connect by Remote Desktop to each N-series VM.</span></span>

2. <span data-ttu-id="19be6-110">Töltse le, bontsa ki és telepítse a Windows operációs rendszer támogatott illesztőprogramját.</span><span class="sxs-lookup"><span data-stu-id="19be6-110">Download, extract, and install the supported driver for your Windows operating system.</span></span>

<span data-ttu-id="19be6-111">Azure virtuális gépeken portok HV újraindításra szükség az illesztőprogram telepítése után.</span><span class="sxs-lookup"><span data-stu-id="19be6-111">On Azure NV VMs, a restart is required after driver installation.</span></span> <span data-ttu-id="19be6-112">A hálózati vezérlő által virtuális gépeken újraindításra szükség.</span><span class="sxs-lookup"><span data-stu-id="19be6-112">On NC VMs, a restart is not required.</span></span>

## <a name="verify-driver-installation"></a><span data-ttu-id="19be6-113">Illesztőprogram telepítésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="19be6-113">Verify driver installation</span></span>

<span data-ttu-id="19be6-114">Illesztőprogram-telepítőfájl az Eszközkezelőben ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="19be6-114">You can verify driver installation in Device Manager.</span></span> <span data-ttu-id="19be6-115">A következő példa bemutatja a Tesla K80 kártya sikeres konfigurációs egy Azure NC virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="19be6-115">The following example shows successful configuration of the Tesla K80 card on an Azure NC VM.</span></span>

![GPU illesztőprogram tulajdonságai](./media/n-series-driver-setup/GPU_driver_properties.png)

<span data-ttu-id="19be6-117">A GPU eszköz állapotát lekérdező, futtassa a [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) parancssori segédprogram illesztőprogrammal telepítve.</span><span class="sxs-lookup"><span data-stu-id="19be6-117">To query the GPU device state, run the [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with the driver.</span></span>

1. <span data-ttu-id="19be6-118">Nyisson meg egy parancssort, és módosítsa a **C:\Program Files\NVIDIA Corporation\NVSMI** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="19be6-118">Open a command prompt and change to the **C:\Program Files\NVIDIA Corporation\NVSMI** directory.</span></span>

2. <span data-ttu-id="19be6-119">Futtatás **nvidia-smi**.</span><span class="sxs-lookup"><span data-stu-id="19be6-119">Run **nvidia-smi**.</span></span> <span data-ttu-id="19be6-120">Ha az adatforrásnak alatt hasonló kimenetet fog látni.</span><span class="sxs-lookup"><span data-stu-id="19be6-120">If the driver is installed you will see output similar to below.</span></span> <span data-ttu-id="19be6-121">Vegye figyelembe, hogy **GPU-Util** látható **0 %** kivéve, ha meg van nyitva egy GPU munkaterhelés a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="19be6-121">Note that **GPU-Util** shows **0%** unless you are currently running a GPU workload on the VM.</span></span>

![NVIDIA eszköz állapota](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a><span data-ttu-id="19be6-123">RDMA hálózati NC24r virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="19be6-123">RDMA network for NC24r VMs</span></span>

<span data-ttu-id="19be6-124">Az azonos rendelkezésre állási készlet telepített NC24r virtuális gépek RDMA hálózati kapcsolat engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="19be6-124">RDMA network connectivity can be enabled on NC24r VMs deployed in the same availability set.</span></span> <span data-ttu-id="19be6-125">Telepítse a Windows hálózati illesztőprogramokat, amelyek lehetővé teszik az RDMA-kapcsolatot a HpcVmDrivers bővítmény szerepelnie kell.</span><span class="sxs-lookup"><span data-stu-id="19be6-125">The HpcVmDrivers extension must be added to install Windows network device drivers that enable RDMA connectivity.</span></span> <span data-ttu-id="19be6-126">A Virtuálisgép-bővítmény adhat hozzá egy NC24r VM [Azure PowerShell](/powershell/azure/overview) az Azure Resource Manager parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="19be6-126">To add the VM extension to an NC24r VM, use [Azure PowerShell](/powershell/azure/overview) cmdlets for Azure Resource Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="19be6-127">Jelenleg csak Windows Server 2012 R2 támogatja az RDMA hálózati NC24r virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="19be6-127">Currently, only Windows Server 2012 R2 supports the RDMA network on NC24r VMs.</span></span>
> 

<span data-ttu-id="19be6-128">A legújabb 1.1-es verziójának telepítése HpcVMDrivers bővítmény egy meglévő RDMA-kompatibilis virtuális gépen az USA nyugati régiója régióban myVM nevű virtuális:</span><span class="sxs-lookup"><span data-stu-id="19be6-128">To install the latest version 1.1 HpcVMDrivers extension on an existing RDMA-capable VM named myVM in the West US region:</span></span>
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  <span data-ttu-id="19be6-129">További információkért lásd: [virtuálisgép-bővítmények és a Windows szolgáltatások](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="19be6-129">For more information, see [Virtual machine extensions and features for Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="19be6-130">Az RDMA hálózati Message Passing Interface (MPI) forgalmat támogatja a futó alkalmazások [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) vagy Intel MPI 5.x.</span><span class="sxs-lookup"><span data-stu-id="19be6-130">The RDMA network supports Message Passing Interface (MPI) traffic for applications running with [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) or Intel MPI 5.x.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="19be6-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19be6-131">Next steps</span></span>

* <span data-ttu-id="19be6-132">Az N-sorozatú virtuális gépeken a NVIDIA Feldolgozóegységekkel kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="19be6-132">For more information about the NVIDIA GPUs on the N-series VMs, see:</span></span>
    * <span data-ttu-id="19be6-133">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (az Azure NC virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="19be6-133">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (for Azure NC VMs)</span></span>
    * <span data-ttu-id="19be6-134">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (az Azure Permanens virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="19be6-134">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (for Azure NV VMs)</span></span>

* <span data-ttu-id="19be6-135">Alkalmazások GPU-az elérését gyorsítja fel a NVIDIA Tesla Feldolgozóegységekkel a fejlesztők is letöltheti és telepítheti a CUDA eszközkészlet 8 [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) vagy [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe).</span><span class="sxs-lookup"><span data-stu-id="19be6-135">Developers building GPU-accelerated applications for the NVIDIA Tesla GPUs can also download and install the CUDA Toolkit 8 for [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) or [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe).</span></span> <span data-ttu-id="19be6-136">További információkért lásd: a [CUDA a telepítési útmutató](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).</span><span class="sxs-lookup"><span data-stu-id="19be6-136">For more information, see the [CUDA Installation Guide](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).</span></span>


