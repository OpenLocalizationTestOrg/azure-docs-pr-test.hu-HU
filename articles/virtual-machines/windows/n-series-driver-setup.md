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
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>Windows Server rendszert futtató N sorozatú virtuális gépek GPU illesztőprogramok beállítása
hello GPU képességek az Azure-N-adatsorozat tootake előnyeit a virtuális gépek Windows Server 2016 vagy Windows Server 2012 R2 fut, a telepítés támogatott NVIDIA video-illesztőprogramok. Ez a cikk lépéseit illesztőprogram beállítása az N-sorozatú virtuális gép telepítése után. Telepítési információk illesztőprogram érhető el is [Linux virtuális gépek](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Alapvető specifikációk, tárolási kapacitás és a lemez adatai: [GPU Windows Virtuálisgép-méretek](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a>Illesztőprogram telepítése

1. Csatlakozzon a távoli asztal tooeach N sorozatú virtuális gép.

2. Töltse le, bontsa ki és hello támogatott illesztőprogramját a Windows operációs rendszert.

Azure virtuális gépeken portok HV újraindításra szükség az illesztőprogram telepítése után. A hálózati vezérlő által virtuális gépeken újraindításra szükség.

## <a name="verify-driver-installation"></a>Illesztőprogram telepítésének ellenőrzése

Illesztőprogram-telepítőfájl az Eszközkezelőben ellenőrizheti. hello következő példa bemutatja hello Tesla K80 kártya sikeres konfigurációs egy Azure NC virtuális gépen.

![GPU illesztőprogram tulajdonságai](./media/n-series-driver-setup/GPU_driver_properties.png)

tooquery hello GPU az eszköz állapotát, futtassa a hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) parancssori segédprogram hello illesztőprogrammal telepítve.

1. Nyisson meg egy parancssort, és módosítsa a toohello **C:\Program Files\NVIDIA Corporation\NVSMI** könyvtár.

2. Futtatás **nvidia-smi**. Ha telepítve van a hello illesztőprogram kimeneti hasonló toobelow jelenik meg. Vegye figyelembe, hogy **GPU-Util** látható **0 %** kivéve, ha jelenleg futtatja a GPU munkaterhelés hello virtuális gép.

![NVIDIA eszköz állapota](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a>RDMA hálózati NC24r virtuális gépekhez

RDMA hálózati kapcsolat engedélyezhető a hello telepített NC24r virtuális gépek azonos rendelkezésre állási csoportot. hello HpcVmDrivers bővítmény tooinstall Windows hálózati eszköz-illesztőprogramok, amelyek lehetővé teszik az RDMA-kapcsolatot hozzá kell adni. tooadd hello VM bővítmény tooan NC24r VM használja [Azure PowerShell](/powershell/azure/overview) az Azure Resource Manager parancsmagok.

> [!NOTE]
> Jelenleg csak Windows Server 2012 R2 támogatja hello RDMA hálózati NC24r virtuális gépeken.
> 

tooinstall hello legújabb 1.1-es verzió HpcVMDrivers kiterjesztése a meglévő RDMA-kompatibilis virtuális hello USA nyugati régiója régióban myVM nevű virtuális:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  További információkért lásd: [virtuálisgép-bővítmények és a Windows szolgáltatások](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

hello RDMA hálózati Message Passing Interface (MPI) forgalmat támogatja a futó alkalmazások [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) vagy Intel MPI 5.x. 


## <a name="next-steps"></a>Következő lépések

* A hello N sorozatú virtuális gépek hello NVIDIA Feldolgozóegységekkel kapcsolatos további információkért lásd:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (az Azure NC virtuális gépek)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (az Azure Permanens virtuális gépek)

* Alkalmazások GPU-gyorsított hello NVIDIA Tesla Feldolgozóegységekkel a fejlesztők is letöltheti és telepítheti a CUDA eszközkészlet 8 hello [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) vagy [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe). További információkért lásd: hello [CUDA a telepítési útmutató](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


