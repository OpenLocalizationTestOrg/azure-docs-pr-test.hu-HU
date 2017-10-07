---
title: "Linux virtuális gépek Azure-ban aaaResource csoportok |} Microsoft Docs"
description: "Tudnivalók: hello legfontosabb tervezési és megvalósítási erőforráscsoportok üzembe helyezés az Azure infrastruktúra-szolgáltatásokat."
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
ms.openlocfilehash: 8809cb5eeb9a166d2bcf1946cd26b0ee748f8cd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-group-guidelines-for-linux-vms"></a>Linux virtuális gépek Azure-erőforrás csoport irányelvek 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Ez a cikk a hogyan toologically ki a környezet létrehozása, és az erőforráscsoportok hello összetevők csoport megismerésére fókuszál.

## <a name="implementation-guidelines-for-resource-groups"></a>Erőforráscsoportok implementációs segédlet
Döntéseket:

* Lesz a kimenő erőforráscsoportok toobuild hello alapvető infrastruktúra összetevői, vagy a teljes alkalmazás központi telepítése?
* Szüksége van toorestrict hozzáférés tooResource csoportok használatával a szerepköralapú hozzáférés-vezérlést?

Feladatok:

* Adja meg mi alapvető infrastruktúra összetevők és dedikált erőforrás csoportokra lesz szükség.
* Felülvizsgálati hogyan tooimplement Resource Manager-sablonok egységes, reprodukálható telepítésekhez.
* Határozza meg, milyen hozzáférési szerepkörökhöz szabályozásának kell elérni tooResource csoportok.
* Az erőforráscsoportok használata az elnevezési hello készlet létrehozása. Hello Azure CLI vagy a portál is használhatja.

## <a name="resource-groups"></a>Erőforráscsoportok
Az Azure akkor logikailag kapcsolódó erőforrások, például a tárfiókokat, virtuális hálózatok és virtuális gépek (VM) toodeploy csoport, kezelése és karbantartásához őket egyetlen egységként. Ez a megközelítés lehetővé teszi könnyebb toodeploy alkalmazások tartani minden hello kapcsolatos közben erőforrások együtt felügyeleti szempontból vagy toogrant mások hozzáférés toothat rendelkező erőforrások csoportjának. Az erőforráscsoportok nevei legfeljebb 90 karakter hosszúságú lehet. Az erőforráscsoportok átfogóbb megértheti, olvassa el hello [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).

TooResource csoportok alapfunkciója hello képességét toobuild a környezetben a JSON-fájl, amely deklarál hello tárolási, hálózatkezelési és számítási erőforrásokat. Kapcsolódó egyéni parancsfájlok vagy konfigurációk tooapply is definiálhat. Ezek a JSON-sablonok segítségével az alkalmazások hoz létre egységes, reprodukálható központi telepítéseket. Ez a megközelítés lehetővé teszi a fejlesztési környezet létrehozása, majd, hogy ugyanazon sablon toocreate éles környezet, vagy fordítva. Kra-sablonokkal kapcsolatban, olvassa el a [sablonokhoz hello](../../azure-resource-manager/resource-manager-template-walkthrough.md) , amely végigvezeti Önt egy JSON-sablon létrehozására lépéseit.

Két különböző megközelítés közül választhat a környezetében erőforráscsoportok tervezésekor van:

* Egyes alkalmazások telepítése, amely hello tárfiókokat, virtuális hálózatok és alhálózatok felderítéséhez, a virtuális gépek, erőforráscsoportok betöltése terheléselosztók stb.
* Központi erőforrás-csoportok a core virtuális hálózatok és alhálózatok vagy tárolási fiókokat tartalmazhat. Az alkalmazások majd szerepelnek, saját erőforrás csoportjai csak virtuális gépek, terheléselosztók, hálózati adapterek, stb.

A horizontális, a virtuális hálózatok központi erőforrás-csoportok létrehozása és alhálózatok toobuild egyszerűbbé teszi a létesítmények közötti hálózati kapcsolat a hibrid kapcsolat beállítások. minden alkalmazás toohave a saját virtuális hálózati konfigurációs és karbantartási igénylő várja hello alternatív módszert. [Szerepköralapú hozzáférés-vezérlést](../../active-directory/role-based-access-control-what-is.md) hozzáférést egy részletes módon toocontrol tooResource csoportok. Az éles környezetben hello felhasználói előfordulhat, hogy ezeket az erőforrásokat, illetve szabályozható hello alapvető infrastruktúra erőforrások csak infrastruktúra mérnökök toowork velük korlátozhatja. Az alkalmazástulajdonosok csak access toohello alkalmazás-összetevők az erőforráscsoportot és nem hello core a környezet az Azure infrastruktúra belül van. A környezet tervezésének, fontolja meg toohello erőforrások eléréséhez szükséges, és ennek megfelelően. az erőforráscsoportok terv hello felhasználók. 

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

