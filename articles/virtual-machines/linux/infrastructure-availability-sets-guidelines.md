---
title: "aaaAvailability beállítja a Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Tudnivalók: hello legfontosabb tervezési és megvalósítási rendelkezésre állási készletek telepítése az Azure infrastruktúra-szolgáltatásokat."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 24f1d91c-8cc0-4251-bb67-ac4c4c37e8cd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 78e4da5c8fd9eb186cafacb46454444b73a9439c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-availability-sets-guidelines-for-linux-vms"></a>Az Azure rendelkezésre állási készletek irányelvek Linux virtuális gépekhez

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Ez a cikk foglalkozik a ismertetése hello szükséges tervezési lépéseket, a rendelkezésre állási készletek tooensure az alkalmazások tervezett vagy nem tervezett események közben elérhető marad.

## <a name="implementation-guidelines-for-availability-sets"></a>A rendelkezésre állási csoportok implementációs segédlet
Döntéseket:

* Hány rendelkezésre állási csoportokra kell alkalmazni a hello különböző szerepkörök és rétegek a alkalmazás infrastruktúrában?

Feladatok:

* Adja meg a virtuális gépek hello számát minden alkalmazás réteg van szüksége.
* Meghatározza, hogy szükséges-e az alkalmazás használt hiba vagy a frissítési tartományok toobe tooadjust hello száma.
* Adja meg a szükséges hello rendelkezésre állási készletek használata az elnevezési konvenciót, és milyen virtuális gépek találhatók őket. A virtuális gép csak egy rendelkezésre állási csoport is található. 

## <a name="availability-sets"></a>Rendelkezésre állási csoportok
Az Azure virtuális gépek (VM) helyezhető tooa nevű rendelkezésre állási csoport logikai csoportosítása. Virtuális gépeken belül egy rendelkezésre állási csoport létrehozásakor hello Azure platformon virtuális gépek elhelyezésének hello elosztása az alapul szolgáló infrastruktúra hello. Kell egy tervezett karbantartási esemény toohello Azure platformon, vagy az alapul szolgáló hardver / infrastruktúra hibája, rendelkezésre állási készletek hello használata biztosítja, hogy legalább egy virtuális gép továbbra is futni fog.

Ajánlott eljárásként alkalmazások kell található egy virtuális. Egy rendelkezésre állási csoportot, amely tartalmaz egy virtuális nem tervezett vagy nem tervezett események hello Azure platformon belül bármely védelmet kapnak. Hello [Azure garantált szolgáltatási szintje](https://azure.microsoft.com/support/legal/sla/virtual-machines) szükséges egy rendelkezésre állási készlet tooallow hello elosztása a virtuális gépek az alapul szolgáló infrastruktúra hello belül két vagy több virtuális gépet. Ha használ [prémium szintű Azure Storage](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello Azure SLA vonatkozik tooa egyetlen virtuális gép.

hello alapul szolgáló infrastruktúra az Azure-ban toomultiple hardver fürtök oszlik meg. Hardver fürtökön egy tartományt a Virtuálisgép-méretek képes támogatni. Rendelkezésre állási csoportok időben csak egyetlen hardver fürt bármikor lehet üzemeltetni. Ezért hello a virtuális gép méretét, amely egy egyetlen rendelkezésre állási készlet létezhet, korlátozott toohello tartomány a Virtuálisgép-méretek hello hardver fürt által támogatott. hello hardver fürt hello rendelkezésre állási csoport van kiválasztva, ha hello hello a rendelkezésre állási csoporthoz az első virtuális gép van telepítve, vagy ha egy rendelkezésre állási csoportot, amelyben minden virtuális gép jelenleg hello felszabadítása leállt állapotban az első virtuális gép elindítása hello. a következő parancsot a CLI hello használt toodetermine hello a virtuális gép rendelkezésre állási csoport rendelkezésre méretű lehet: "az vm lista-méretek – hely \<karakterlánc\>"

Hardver fürtökön toomultiple frissítési tartományok és a tartalék tartományok oszlik meg. Ezekből a tartományokból egy közös frissítési ciklusa, vagy a megosztás hasonló fizikai infrastruktúra, például az energia és a hálózati fájlmegosztás mely állomások vannak definiálva. Azure automatikusan osztja el a virtuális gépek rendelkezésre állási készlet tartományok toomaintain rendelkezésre állás és a hibatűrés belül. Attól függően, hogy az alkalmazás- és hello belül egy rendelkezésre állási csoportot a virtuális gépek számát hello méretét, hello szám módosíthatja a tartományok toouse kívánja. További tudnivalók [kezelése a rendelkezésre állás és a frissítés és a tartalék tartományok használatát](manage-availability.md).

Az alkalmazás-infrastruktúra megtervezéséhez tervezze meg az hello alkalmazás rétegek toouse. Hello átadott csoport virtuális gépek azonos cél tooavailability készletben, például a rendelkezésre állási készlet a nginx vagy Apache futó előtér virtuális gépeket. Hozzon létre egy külön rendelkezésre állási csoportban, a MongoDB vagy MySQL rendszerű háttér-virtuális géphez. hello célja tooensure, hogy az alkalmazás minden összetevő rendelkezésre állási csoport által védett, és legalább egyszer példány mindig továbbra is futni fog.

Terheléselosztók állítható be a rendelkezésre állási csoportok mellett minden egyes alkalmazás réteg toowork elé, és győződjön meg arról, forgalom mindig-példányt futtató irányított tooa lehet. A terheléselosztó nélkül a virtuális gépek továbbra is fut a tervezett és nem tervezett karbantartási események teljes, de a végfelhasználó nem tudja tooresolve őket, ha hello elsődleges virtuális gép nem érhető el.

Tervezze meg az alkalmazás a magas rendelkezésre állású tárolási rétegben. hello ajánlott túl[kezelt lemezek használata virtuális gépek rendelkezésre állási csoport](manage-availability.md#use-managed-disks-for-vms-in-an-availability-set). Ha a nem felügyelt lemezek jelenleg használ, erősen ajánlott, túl[alakítsa át a virtuális gépek rendelkezésre állási csoport toouse kezelt lemezeken](convert-unmanaged-to-managed-disks.md#convert-vms-in-an-availability-set).

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

