---
title: "Előfizetés és a Linux virtuális gépek az Azure-fiók |} Microsoft Docs"
description: "További tudnivalók a főbb tervezési és megvalósítási irányelvek előfizetések és az Azure-fiókok."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19343826-7eef-42a1-98be-4ec65b0f377a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 19695a9960d8e8f0dfca4bf0ca10761fe6ae7ff0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-linux-vms"></a>Linux virtuális gépek Azure előfizetés és a fiókok irányelvek

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Ez a cikk foglalkozik az előfizetés és a fiókok kezelése, a környezet megközelítési alapos ismerete, és a felhasználói bázis növekszik.

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Implementációs segédlet előfizetések és-fiókok
Döntéseket:

* Milyen előfizetések és fiókokat kell az informatikai munkaterhelés vagy az infrastruktúra tárolására beállított?
* Hogyan szeretné bontani a hierarchiában a szervezet megfelelően?

Feladatok:

* Adja meg a logikai szervezeti hierarchia, szerint szeretné egy előfizetési szintről a kezeléshez.
* Ezt a logikai hierarchiát megfeleltetéséhez szükséges fiókok és minden egyes fiókkal előfizetések megadása.
* Hozzon létre az előfizetések és az elnevezési fiókokat.

## <a name="subscriptions-and-accounts"></a>Előfizetések és fiókok
Azure használatához szükség van egy vagy több Azure-előfizetések. Erőforrások – például virtuális gépek (VM), illetve ezek előfizetések szerepel virtuális hálózatok.

* A vállalati ügyfelek általában rendelkeznek a vállalati beléptetési, amely a childwindow-erőforrás a hierarchiában, és egy vagy több fiókot társítva.
* A felhasználók és az ügyfelek a vállalati beléptetési nélkül a childwindow-erőforrást az a fiók.
* Előfizetések tartoznak fiókok, és egy vagy több előfizetés fiókonként is lehet. Az Azure rekordok számlázási adatok az előfizetés szintjén.

A fiók vagy előfizetés kapcsolat két hierarchiaszintekkel legfeljebb miatt fontos az elnevezési fiókok és-előfizetések számlázási igényeihez igazítani. Például ha egy globális vállalat Azure használ, akkor előfordulhat, hogy úgy a egy fiók régiónként, és előfizetések felügyelt rendelkezik a terület szint:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Például használhatja az alábbi szerkezettel:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Egy régiót úgy dönt, hogy egy adott csoporthoz tartozó egynél több előfizetéssel rendelkezik, ha az elnevezési oly módon, a felesleges adatokat a fiók vagy előfizetés nevét a kódolni kell tartalmaznia. Ez a szervezet lehetővé teszi, hogy massaging elszámolási adatok a hierarchia új szintek során számlázási jelentések létrehozásához:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Az alábbi példában a szervezet volt látható:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

A nagyvállalati szerződéssel nyújtunk részletes számlázási egy letölthető fájlt egy olyan fiók, vagy az összes fiók használatával.

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

