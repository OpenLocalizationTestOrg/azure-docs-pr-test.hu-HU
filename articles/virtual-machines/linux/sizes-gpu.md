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
# <a name="gpu-linux-vm-sizes"></a>GPU Linux Virtuálisgép-méretek

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Illesztőprogram telepítése és ellenőrzési lépések, tekintse meg [N-series illesztőprogram telepítése Linux](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Nem ajánlott, hogy telepítése X kiszolgáló vagy a más rendszerekkel, amely az Ubuntu NC virtuális gépeken futó hello nouveau illesztőprogramot használja. NVIDIA GPU illesztőprogramok telepítése előtt toodisable hello nouveau illesztőprogram szükséges.  

## <a name="other-sizes"></a>Más méretek
- [Általános célú](sizes-general.md)
- [Számításra optimalizált](sizes-compute.md)
- [Memóriaoptimalizált](sizes-memory.md)
- [Tárolásra optimalizált](sizes-storage.md)
- [Nagy teljesítményű számítás](sizes-hpc.md)

## <a name="next-steps"></a>Következő lépések
További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.