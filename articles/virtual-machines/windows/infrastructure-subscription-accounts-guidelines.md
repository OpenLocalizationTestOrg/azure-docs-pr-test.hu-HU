---
title: "aaaSubscription és a fiókot a következő Windows-alapú virtuális gépek Azure-ban |} Microsoft Docs"
description: "Tudnivalók: hello legfontosabb tervezési és megvalósítási előfizetések és a fiókok az Azure-on."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 761fa847-78b0-4078-a33a-d95d198d1029
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f9dc712af559b04490be1dc721a9b9f7fe9ed88f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-windows-vms"></a>Windows virtuális gépek Azure előfizetés és a fiókok irányelvek

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Ez a cikk foglalkozik a megértése, hogyan tooapproach előfizetés és a fiókok kezelése, a környezet és a felhasználói bázis növekszik.

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Implementációs segédlet előfizetések és-fiókok
Döntéseket:

* Milyen set előfizetések és a fiókok van szüksége toohost az informatikai munkaterhelés vagy az infrastruktúra?
* Hogyan toobreak hello hierarchia toofit le a szervezet?

Feladatok:

* Adja meg a logikai szervezeti hierarchia szerint szeretné toomanage azt egy előfizetés szintjéről.
* toomatch ezt a logikai hierarchiát, adja meg a szükséges hello fiókjaihoz és előfizetéseihez minden egyes fiókkal.
* Hozzon létre hello előfizetések és az elnevezési fiókokat.

## <a name="subscriptions-and-accounts"></a>Előfizetések és fiókok
az Azure-ral toowork, szüksége van egy vagy több Azure-előfizetések. Erőforrások – például virtuális gépek (VM), illetve ezek előfizetések szerepel virtuális hálózatok.

* A vállalati ügyfelek a vállalati beléptetési, amely hello childwindow-erőforrás hello hierarchiában, és társított tooone vagy további fiókok általában rendelkeznek.
* A felhasználók és az ügyfelek a vállalati beléptetési nélkül a hello childwindow-erőforrás az hello fiók.
* Előfizetések társított tooaccounts, és egy vagy több előfizetés fiókonként is lehet. Az Azure rekordok számlázási adatokat hello előfizetés szintjén.

Lejáró hello/előfizetés kapcsolat két hierarchiaszintekkel toohello korlátot fontos tooalign hello elnevezési konvenciót kell számlázási fiókjaihoz és előfizetéseihez toohello a. Például ha egy globális vállalat Azure használ, előfordulhat, hogy választ toohave egy fiók régiónként, és előfizetések felügyelt rendelkezik régió szint hello:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Használhatja például a következő struktúra hello:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Ha egy régió egyetlen előfizetéssel társított tooa adott csoport nagyobb toohave úgy dönt, hello elnevezési konvenciót kell tartalmaznia egy módon tooencode hello hello fiókkal vagy hello előfizetés nevét a további adatokat. A szervezet engedélyezi a számlázási toogenerate hello új szintek hierarchia massaging számlázási jelentések során:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

hello szervezet a következő példa hello nézhet:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

A nagyvállalati szerződéssel nyújtunk részletes számlázási egy letölthető fájlt egy olyan fiók, vagy az összes fiók használatával.

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

