---
title: "Azure virtuális gépek tooManaged lemezek aaaMigrate |} Microsoft Docs"
description: "A storage-fiókok toouse felügyelt lemezek nem felügyelt lemezek használatával létrehozott Azure virtuális gépek áttelepítését."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 29420f13c4ffd5b25726e0ef1aafe89347286a89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-vms-toomanaged-disks-in-azure"></a>Telepítse át az Azure virtuális gépek tooManaged lemezt az Azure-ban

Azure-lemezeket felügyelt hello kell eltávolításával egyszerűbbé teszi a tárolókezelési tooseparately storage-fiókok kezelése.  A meglévő Azure virtuális gépek tooManaged lemezek toobenefit nagyobb megbízhatóságot, rendelkezésre állási csoport a virtuális gépek is telepíthet át. Ez biztosítja, hogy hello lemezek különböző virtuális gépek a rendelkezésre állási csoport elég elkülönül minden más tooavoid egyetlen ponton a hibákat. A rendelkezésre állási csoport a különböző tárolási méretezési egységeit (bélyegek) egy méretezési egység tárolóhibák miatt toohardware és szoftver hibák miatt hello hatását korlátozó különböző virtuális lemezek automatikusan helyezi.
A igények alapján választhat a két típusú tárolási lehetőségek közül választhat:

- [Prémium szintű felügyelt lemez](../../storage/common/storage-premium-storage.md) tartós állapot meghajtót (SSD)-alapú tárolási adathordozókon rendszerrel highperformance, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek támogatása. Hello sebesség előnyeit, és ezek a lemezek áttelepítése tooPremium kezelt lemezek teljesítményét is igénybe vehet.

- [Standard szintű felügyelt lemez](../../storage/common/storage-standard-storage.md) használják a merevlemez-meghajtó (HDD)-alapú tárolási adathordozókon, és a legalkalmasabb fejlesztési és tesztelési célú és egyéb gyakori hozzáférés munkaterhelések kevesebb bizalmas tooperformance variancia.

Áttelepítheti a következő esetekben tooManaged lemezek:

| Telepítse át...                                            | Dokumentáció-hivatkozás                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alakítsa át az önálló virtuális gépek és virtuális gépek egy rendelkezésre állási készlet toomanaged lemezeken   | [Alakítsa át a virtuális gépek felügyelt toouse lemezeket](convert-unmanaged-to-managed-disks.md) |
| Egy virtuális a klasszikus tooResource Manager kezelt lemezeken     | [Egyetlen virtuális gép áttelepítése](migrate-single-classic-to-resource-manager.md)  | 
| A klasszikus tooResource Manager kezelt lemezeken vNet összes hello virtuális     | [IaaS-erőforrásokra át klasszikus tooResource Manager](migration-classic-resource-manager-ps.md) , majd [egy virtuális gép átalakítása nem felügyelt lemezek toomanaged lemezekből](convert-unmanaged-to-managed-disks.md) | 






## <a name="plan-for-hello-conversion-toomanaged-disks"></a>Hello átalakítás megtervezése tooManaged lemezek

Ez a szakasz segítséget nyújt toomake hello legjobb döntést a virtuális gép és a lemez típusok.


## <a name="location"></a>Hely

Jelölje ki a helyet, ahol Azure felügyelt lemezek érhetőek el. Ha tooPremium kezelt lemezek, is győződjön meg arról, hogy prémium szintű storage elérhető hello régióban, ha azt tervezi, hogy toomove. Lásd: [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services) elérhető helyről naprakész tájékoztatást.

## <a name="vm-sizes"></a>A virtuális gépek mérete

TooPremium kezelt lemezek telepít át, ha vannak tooupdate hello mérete hello VM tooPremium képes tárméret elérhető hello régió, ahol a virtuális gép. Tekintse át, amelyek képesek a prémium szintű Storage hello Virtuálisgép-méretek. hello Azure virtuális gép mérete specifikációk szereplő [virtuális gépek méretei](sizes.md).
Tekintse át a virtuális gépek, amely együttműködik a prémium szintű Storage, és válassza ki a hello leginkább megfelelő virtuális gép méretét, amely a legjobban megfelel a számítási feladatok hello teljesítményétől. Győződjön meg arról, hogy nincs elegendő sávszélesség érhető el a virtuális gép toodrive hello lemez forgalom.

## <a name="disk-sizes"></a>Lemezméretek

**Prémium szintű felügyelt lemez**

Hét különböző típusú együtt a virtuális Gépet felügyelt prémium lemezeket, és mindegyik rendelkezik-e adott iops-érték és átviteli korlátok. Vegye figyelembe a működés felső korlátjának hello prémium szintű lemez típusát a virtuális gép alapján az alkalmazás kapacitása, teljesítmény, méretezhetőség hello igényeinek, és a maximális betölti kiválasztásakor.

| Prémium szintű lemezek típusa  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Lemezméret           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS-érték lemezenként       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Adattovábbítás lemezenként | 25 MB / s  | 50 MB / s  | 100 MB / s | 150 MB / s | 200 MB / s | 250 MB / s | 250 MB / s |

**Standard szintű felügyelt lemez**

Standard szintű felügyelt lemez együtt a virtuális gép hét típusa van. Azok a különböző kapacitással rendelkeznek, de azonos IOPS és átviteli sebességének korlátai. Válassza ki az alkalmazás hello kapacitásigények alapján standard szintű felügyelt lemez hello típusú.

| Standard lemez típusa  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Lemezméret           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOPS-érték lemezenként       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Adattovábbítás lemezenként | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s | 

## <a name="disk-caching-policy"></a>Lemez gyorsítótárazási házirend

**Prémium szintű felügyelt lemez**

Alapértelmezés szerint a gyorsítótárazási házirend lemez van *írásvédett* az összes hello prémium adatlemezek, és *írható-olvasható* hello prémium operációsrendszer-lemez csatolni a virtuális gép toohello. A konfigurációs beállítás ajánlott tooachieve hello optimális teljesítménye az alkalmazás IOs-hez. Írási műveleteket vagy a csak írható adatlemezek (köztük SQL Server) tiltsa le a lemezt gyorsítótárazás, hogy az alkalmazás jobb teljesítményt érhet el.

## <a name="pricing"></a>Díjszabás

Felülvizsgálati hello [kezelt lemezek árképzési](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Prémium szintű felügyelt lemez árképzési legyen, mint hello nem felügyelt Premium lemezek. Azonban a standard szintű felügyelt lemez árképzési más nem felügyelt Standard lemezeknél.



## <a name="next-steps"></a>Következő lépések

- További információ [kezelt lemezek](managed-disks-overview.md)
