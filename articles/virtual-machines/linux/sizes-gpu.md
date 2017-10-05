---
title: "Azure Linux virtuális gép méretének - GPU |} Microsoft Docs"
description: "A különböző GPU listák az Azure-ban a Linux virtuális gépekhez elérhető méretek optimalizált."
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
ms.openlocfilehash: 5c9bf89feba519147b07f2810fe4da882664e89e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="gpu-linux-vm-sizes"></a><span data-ttu-id="188b9-103">GPU Linux Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="188b9-103">GPU Linux VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

<span data-ttu-id="188b9-104">Illesztőprogram telepítése és ellenőrzési lépések, tekintse meg [N-series illesztőprogram telepítése Linux](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="188b9-104">For driver installation and verification steps, see [N-series driver setup for Linux](n-series-driver-setup.md).</span></span>

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* <span data-ttu-id="188b9-105">Nem ajánlott, hogy telepítése X kiszolgáló vagy a más rendszerekkel, az Ubuntu NC virtuális gépeken futó nouveau illesztőprogramot használja.</span><span class="sxs-lookup"><span data-stu-id="188b9-105">We don't recommend installing X server or other systems that use the nouveau driver on Ubuntu NC VMs.</span></span> <span data-ttu-id="188b9-106">NVIDIA GPU-illesztőprogramok a telepítés előtt tiltsa le a nouveau illesztőprogramot kell.</span><span class="sxs-lookup"><span data-stu-id="188b9-106">Before installing NVIDIA GPU drivers, you need to disable the nouveau driver.</span></span>  

## <a name="other-sizes"></a><span data-ttu-id="188b9-107">Más méretek</span><span class="sxs-lookup"><span data-stu-id="188b9-107">Other sizes</span></span>
- [<span data-ttu-id="188b9-108">Általános célú</span><span class="sxs-lookup"><span data-stu-id="188b9-108">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="188b9-109">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="188b9-109">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="188b9-110">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="188b9-110">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="188b9-111">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="188b9-111">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="188b9-112">Nagy teljesítményű számítás</span><span class="sxs-lookup"><span data-stu-id="188b9-112">High performance compute</span></span>](sizes-hpc.md)

## <a name="next-steps"></a><span data-ttu-id="188b9-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="188b9-113">Next steps</span></span>
<span data-ttu-id="188b9-114">További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="188b9-114">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>