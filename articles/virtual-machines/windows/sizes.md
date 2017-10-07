---
title: "az Azure-ban méretének aaaWindows VM |} Microsoft Docs"
description: "Windows virtuális gépek Azure-ban elérhető különböző méretű hello sorolja fel."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 80bd8241b134031c41b56224e841c7557c4845cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Az Azure-ban a Windows virtuális gépek méretei

Ez a cikk ismerteti a hello elérhető méretek, és beállítások hello toorun használhatja a Windows-alkalmazások és számítási feladatok Azure virtuális gépeken. Amikor arra készül, toouse ezeket az erőforrásokat is biztosít telepítési szempontok toobe tudomást.  Ez a cikk érhető el is [Linux virtuális gépek](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


| Típus                     | Méretek           |    Leírás       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Általános célú](sizes-general.md)          | Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7 | Kiegyensúlyozott processzor-memória arány. Tesztelés és fejlesztési kis toomedium adatbázisok és alacsony toomedium forgalom webkiszolgálók ideális. |
| [Számításra optimalizált](sizes-compute.md)        | FS, F             | Magas processzor-memória arány. Alkalmas közepes adatforgalmú webkiszolgálók, hálózati berendezések, kötegfolyamatok és alkalmazáskiszolgálók számára.        |
| [Memóriaoptimalizált](../virtual-machines-windows-sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Magas memória-CPU aránya. A relációs adatbázis-kiszolgálók, közepes méretű toolarge gyorsítótárak és memórián belüli analytics nagy.                 |
| [Tárolásra optimalizált](../virtual-machines-windows-sizes-storage.md)        | Ls                | Magas lemez-adatátviteli és I/O-műveleti jellemzők. Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.                                                         |
| [GPU](sizes-gpu.md)            | PORTOK HV, NC            | Speciális virtuális gépekre nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt. Egy vagy több Feldolgozóegységekkel érhető el.       |
| [Nagy teljesítményű számítás](sizes-hpc.md) | H, A8-11          | A leggyorsabb és leghatékonyabb processzorral rendelkező virtuális gépeink, választható nagy átviteli sebességű (távoli közvetlen memória-hozzáférést lehetővé tevő) hálózati adapterrel. 

<br> 

- Különböző méretű hello az árazással kapcsolatos információkért lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). 
- általános korlátozások toosee Azure virtuális gépeken, lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).
- Tárolási költségek hello tárfiókban használt lapok számítása külön alapul. További információkért [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).
- További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.



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
- [Memóriaoptimalizált](../virtual-machines-windows-sizes-memory.md)
- [Tárolásra optimalizált](../virtual-machines-windows-sizes-storage.md)
- [GPU-optimalizált](sizes-gpu.md)
- [Nagy teljesítményű számítás](sizes-hpc.md)



