---
title: "az Azure-ban méretének aaaLinux VM |} Microsoft Docs"
description: "Linux virtuális gépek Azure-ban elérhető különböző méretű hello sorolja fel."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 56cbe0a0d7d34def07636edba74c4c699e336012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Az Azure Linux virtuális gépek méretei
Ez a cikk ismerteti hello elérhető méretek, és a Linux-alkalmazások és munkafolyamatok toorun használhatja az Azure virtuális gépek hello beállítások. Amikor arra készül, toouse ezeket az erőforrásokat is biztosít telepítési szempontok toobe tudomást. Ez a cikk érhető el is [Windows virtuális gépek](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


| Típus                     | Méretek           |    Leírás       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Általános célú](sizes-general.md)          | Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7,  | Kiegyensúlyozott processzor-memória arány. Tesztelés és fejlesztési kis toomedium adatbázisok és alacsony toomedium forgalom webkiszolgálók ideális. |
| [Számításra optimalizált](sizes-compute.md)        | FS, F             | Magas processzor-memória arány. Közepes forgalom webkiszolgálók, hálózati berendezésekbe, fürdőbe folyamatok és alkalmazáskiszolgálók jó.        |
| [Memóriaoptimalizált](sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Magas memória-CPU aránya. A relációs adatbázis-kiszolgálók, közepes méretű toolarge gyorsítótárak és memórián belüli analytics nagy.                 |
| [Tárolásra optimalizált](sizes-storage.md)        | Ls                | Magas lemez-adatátviteli és I/O-műveleti jellemzők. Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.                                                         |
| [GPU](sizes-gpu.md)            | PORTOK HV, NC            | Speciális virtuális gépekre nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt. Egy vagy több Feldolgozóegységekkel érhető el.       |
| [Nagy teljesítményű számítás](sizes-hpc.md) | H, A8-11          | A leggyorsabb és leghatékonyabb processzorral rendelkező virtuális gépeink, választható nagy átviteli sebességű (távoli közvetlen memória-hozzáférést lehetővé tevő) hálózati adapterrel. 

<br>

- Különböző méretű hello az árazással kapcsolatos információkért lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Az Azure-régiók Virtuálisgép-méretek rendelkezésre állását, lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/).
- általános korlátozások toosee Azure virtuális gépeken, lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).
- További tudnivalók [Azure számítási egység (ACU)](../windows/acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.


## <a name="rest-api"></a>REST API-n

A Virtuálisgép-méretek hello REST API tooquery használatáról információkért lásd: hello következő:

- [Átméretezéséhez elérhető virtuálisgép-méretek listázása](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [Az előfizetéshez elérhető virtuálisgép-méretek listázása](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [A rendelkezésre állási csoportok elérhető virtuálisgép-méretek listázása](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a>ACU

További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.

## <a name="next-steps"></a>Következő lépések

További tudnivalók hello másik Virtuálisgép-méretek elérhető:
- [Általános célú](sizes-general.md)
- [Számításra optimalizált](sizes-compute.md)
- [Memóriaoptimalizált](sizes-memory.md)
- [Tárolásra optimalizált](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Nagy teljesítményű számítás](sizes-hpc.md)



