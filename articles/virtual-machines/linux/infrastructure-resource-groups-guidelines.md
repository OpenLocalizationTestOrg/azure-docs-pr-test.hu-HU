---
title: "Linux virtuális gépek az Azure erőforráscsoportok |} Microsoft Docs"
description: "További tudnivalók a főbb tervezési és megvalósítási irányelvek erőforráscsoportok az Azure-infrastruktúra szolgáltatások telepítéséhez."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 93ab9d93-965a-46b9-9c87-a10d652a6422
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 452acde571164a3ab4ce2dcccf99d2aed90361fe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-resource-group-guidelines-for-linux-vms"></a>Linux virtuális gépek Azure-erőforrás csoport irányelvek 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Ez a cikk logikailag el a környezet létrehozás és erőforráscsoportok összetevők csoport megismerésére fókuszál.

## <a name="implementation-guidelines-for-resource-groups"></a>Erőforráscsoportok implementációs segédlet
Döntéseket:

* Lesz a build erőforráscsoportok ki az alapvető infrastruktúra összetevői, vagy a teljes alkalmazás központi telepítése?
* Korlátozza a hozzáférést szerepköralapú hozzáférés-vezérlést használó erőforráscsoportok kell?

Feladatok:

* Adja meg mi alapvető infrastruktúra összetevők és dedikált erőforrás csoportokra lesz szükség.
* Tekintse át a Resource Manager-sablonok egységes, reprodukálható telepítések megvalósításához.
* Adja meg, milyen hozzáférési szerepkörökhöz való hozzáférés szabályozása van szüksége.
* Hozzon létre az erőforráscsoportok az elnevezési konvenciót használ. Használhatja az Azure parancssori felület vagy a portálon.

## <a name="resource-groups"></a>Erőforráscsoportok
Az Azure akkor logikailag csoportosítják a kapcsolódó erőforrások, például a tárfiókokat, virtuális hálózatok és virtuális gépek (VM) telepítése, kezelése és karbantartása azokat egyetlen egységként. Ez a megközelítés egyszerűbbé teszi az alkalmazás központi telepítéséhez, miközben a kapcsolódó erőforrások együtt felügyeleti szempontból vagy hozzáférést mások ehhez a csoporthoz, az erőforrások. Az erőforráscsoportok nevei legfeljebb 90 karakter hosszúságú lehet. Egy átfogóbb megértheti az erőforráscsoportok, áttekintheti a [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).

Erőforráscsoportokkal alapfunkciója azt a képességet a környezetben, egy JSON-fájl, amely a tárolási, hálózatkezelési deklarál segítségével készítse el és számítási erőforrásokat. Kapcsolódó egyéni parancsfájlok vagy alkalmazandó konfigurációk is definiálhat. Ezek a JSON-sablonok segítségével az alkalmazások hoz létre egységes, reprodukálható központi telepítéseket. Ez a megközelítés lehetővé teszi a fejlesztési környezet létrehozása, majd, hogy ugyanazt a sablont éles központi telepítések, vagy fordítva. Kra-sablonokkal kapcsolatban, olvassa el a [a sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md) , amely végigvezeti Önt egy JSON-sablon létrehozására lépéseit.

Két különböző megközelítés közül választhat a környezetében erőforráscsoportok tervezésekor van:

* Egyes alkalmazások telepítése a tárfiókokat, virtuális hálózatok és alhálózatok felderítéséhez, a virtuális gépek, kombináló erőforráscsoportok betöltése terheléselosztók stb.
* Központi erőforrás-csoportok a core virtuális hálózatok és alhálózatok vagy tárolási fiókokat tartalmazhat. Az alkalmazások majd szerepelnek, saját erőforrás csoportjai csak virtuális gépek, terheléselosztók, hálózati adapterek, stb.

Ön kibővítési, a virtuális hálózatok és alhálózatok központi erőforrás-csoportok létrehozása teszi megkönnyíti a létesítmények közötti hálózati kapcsolati lehetőségek hibrid kapcsolatok. Az alternatív megoldás, az egyes alkalmazásoknál a saját virtuális hálózatot, amelyhez a konfigurációs és karbantartási. [Szerepköralapú hozzáférés-vezérlést](../../active-directory/role-based-access-control-what-is.md) biztosít a részletes módot erőforráscsoportok való hozzáférés szabályozása. Az éles környezetben szabályozhatja, hogy a felhasználók férhetnek hozzá azokhoz az erőforrásokhoz, vagy az alapvető infrastruktúra erőforrások korlátozhatja csak infrastruktúra szakemberei számára, így azokat. Az alkalmazástulajdonosok csak az alkalmazás-összetevők az erőforráscsoportot és nem az Azure-infrastruktúra a környezet core hozzáféréssel rendelkeznek. Tervezésének abban a környezetben, fontolja meg a felhasználókat, akik hozzá kell férniük az erőforrásokhoz, és ennek megfelelően kialakítása az erőforráscsoportokat. 

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

