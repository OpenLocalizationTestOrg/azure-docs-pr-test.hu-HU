---
title: "Linux virtuális gép aaaAzure méretének - GPU |} Microsoft Docs"
description: "Hello optimalizált GPU mérete eltér az Azure-ban a Linux virtuális gépekhez elérhető sorolja fel."
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
ms.openlocfilehash: e98f720499be37df4048aeb513aa4f6b187b7335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="gpu-linux-vm-sizes"></a><span data-ttu-id="607c5-103">GPU Linux Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="607c5-103">GPU Linux VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

<span data-ttu-id="607c5-104">Illesztőprogram telepítése és ellenőrzési lépések, tekintse meg [N-series illesztőprogram telepítése Linux](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="607c5-104">For driver installation and verification steps, see [N-series driver setup for Linux](n-series-driver-setup.md).</span></span>

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* <span data-ttu-id="607c5-105">Nem ajánlott, hogy telepítése X kiszolgáló vagy a más rendszerekkel, amely az Ubuntu NC virtuális gépeken futó hello nouveau illesztőprogramot használja.</span><span class="sxs-lookup"><span data-stu-id="607c5-105">We don't recommend installing X server or other systems that use hello nouveau driver on Ubuntu NC VMs.</span></span> <span data-ttu-id="607c5-106">NVIDIA GPU illesztőprogramok telepítése előtt toodisable hello nouveau illesztőprogram szükséges.</span><span class="sxs-lookup"><span data-stu-id="607c5-106">Before installing NVIDIA GPU drivers, you need toodisable hello nouveau driver.</span></span>  

## <a name="other-sizes"></a><span data-ttu-id="607c5-107">Más méretek</span><span class="sxs-lookup"><span data-stu-id="607c5-107">Other sizes</span></span>
- [<span data-ttu-id="607c5-108">Általános célú</span><span class="sxs-lookup"><span data-stu-id="607c5-108">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="607c5-109">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="607c5-109">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="607c5-110">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="607c5-110">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="607c5-111">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="607c5-111">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="607c5-112">Nagy teljesítményű számítás</span><span class="sxs-lookup"><span data-stu-id="607c5-112">High performance compute</span></span>](sizes-hpc.md)

## <a name="next-steps"></a><span data-ttu-id="607c5-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="607c5-113">Next steps</span></span>
<span data-ttu-id="607c5-114">További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="607c5-114">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>