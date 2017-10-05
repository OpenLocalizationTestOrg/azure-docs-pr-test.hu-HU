---
title: "A Windows-alapú virtuális gépek az Azure erőforráscsoportok |} Microsoft Docs"
description: "További tudnivalók a főbb tervezési és megvalósítási irányelvek erőforráscsoportok az Azure-infrastruktúra szolgáltatások telepítéséhez."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0fbcabcd-e96d-4d71-a526-431984887451
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0aca77af04f895d516f4943188d7890ae9586b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-group-guidelines-for-windows-vms"></a>Windows virtuális gépek Azure-erőforrás csoport irányelvek

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Ez a cikk logikailag el a környezet létrehozás és erőforráscsoportok összetevők csoport megismerésére fókuszál.

## <a name="implementation-guidelines-for-resource-groups"></a>Erőforráscsoportok implementációs segédlet
Döntéseket:

* Lesz a build erőforráscsoportok ki az alapvető infrastruktúra összetevői, vagy a teljes alkalmazás központi telepítése?
* Korlátozza a hozzáférést szerepköralapú hozzáférés-vezérlést használó erőforráscsoportok kell?

Feladatok:

* Adja meg mi alapvető infrastruktúra összetevők és dedikált erőforrás csoportokra lesz szükség.
* Tekintse át a Resource Manager-sablonok egységes, reprodukálható telepítések megvalósításához.
* Adja meg, milyen hozzáférési szerepkörökhöz való hozzáférés szabályozása van szüksége.
* Hozzon létre az erőforráscsoportok az elnevezési konvenciót használ. Használhatja az Azure PowerShell vagy a portálon.

## <a name="resource-groups"></a>Erőforráscsoportok
Az Azure akkor logikailag csoportosítják a kapcsolódó erőforrások, például a tárfiókokat, virtuális hálózatok és virtuális gépek (VM) telepítése, kezelése és karbantartása azokat egyetlen egységként. Ez a megközelítés egyszerűbbé teszi az alkalmazás központi telepítéséhez, miközben a kapcsolódó erőforrások együtt felügyeleti szempontból vagy hozzáférést mások ehhez a csoporthoz, az erőforrások. Az erőforráscsoportok nevei legfeljebb 90 karakter hosszúságú lehet. Az erőforráscsoportok átfogóbb ismertetése, olvassa el a [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).

Erőforráscsoportokkal alapfunkciója azt a képességet a sablonok használatával környezet felé. A sablon egy egyszerűen egy JSON fájl deklarálja a tárolási, hálózatkezelési és számítási erőforrásokat. Kapcsolódó egyéni parancsfájlok vagy alkalmazandó konfigurációk is definiálhat. Ezek a sablonok használatával egységes, reprodukálható központi telepítések az alkalmazások létrehozásához. Ez a megközelítés megkönnyíti, hogy ki a fejlesztési környezet létrehozása, majd, hogy ugyanazt a sablont éles központi telepítések, vagy fordítva. A sablonok segítségével jobban megértheti, olvassa el a [a sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md) , amely végigvezeti Önt minden lépés az épület ki egy sablont.

Két különböző megközelítés közül választhat a környezetében erőforráscsoportok tervezésekor van:

* Egyes alkalmazások telepítése a tárfiókokat, virtuális hálózatok és alhálózatok felderítéséhez, a virtuális gépek, kombináló erőforráscsoportok betöltése terheléselosztók stb.
* Központi erőforrás-csoportok a core virtuális hálózatok és alhálózatok vagy tárolási fiókokat tartalmazhat. Az alkalmazások majd szerepelnek, saját erőforrás csoportjai csak virtuális gépek, terheléselosztók, hálózati adapterek, stb.

Ön kibővítési, a virtuális hálózatok és alhálózatok központi erőforrás-csoportok létrehozása teszi megkönnyíti a létesítmények közötti hálózati kapcsolati lehetőségek hibrid kapcsolatok. Az alternatív megoldás, az egyes alkalmazásoknál a saját virtuális hálózatot, amelyhez a konfigurációs és karbantartási.  [Szerepköralapú hozzáférés-vezérlést](../../active-directory/role-based-access-control-what-is.md) biztosít a részletes módot erőforráscsoportok való hozzáférés szabályozása. Az éles környezetben szabályozhatja, hogy a felhasználók férhetnek hozzá azokhoz az erőforrásokhoz, vagy az alapvető infrastruktúra erőforrások korlátozhatja csak infrastruktúra szakemberei számára, így azokat. Az alkalmazástulajdonosok csak az alkalmazás-összetevők az erőforráscsoportot és nem az Azure-infrastruktúra a környezet core hozzáféréssel rendelkeznek. Tervezésének abban a környezetben, fontolja meg a felhasználókat, akik hozzá kell férniük az erőforrásokhoz, és ennek megfelelően kialakítása az erőforráscsoportokat. 

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

