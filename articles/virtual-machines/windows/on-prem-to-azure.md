---
title: "az AWS és egyéb platformok tooManaged aaaMigrate lemezterület az Azure-ban |} Microsoft Docs"
description: "Virtuális gépek létrehozása az Azure-ban a többi felhőből, például az AWS vagy más virtualizációs platformokról feltöltött virtuális merevlemezek és Azure felügyelt lemezek előnyeit."
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
ms.date: 02/07/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 66c3912397ab905aafb3910e13ac711befb8f502
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-amazon-web-services-aws-and-other-platforms-toomanaged-disks-in-azure"></a>Telepítse át Amazon Web Services (AWS) és más platformokra tooManaged lemezt az Azure-ban

Feltöltheti a VHD-fájlok AWS vagy a helyszíni virtualizációs megoldások tooAzure toocreate virtuális gépek felügyelt lemezek előnyeit. Azure-lemezeket felügyelt eltávolítja az Azure IaaS virtuális gépeket a hello toomanaging tárfiókok szükségességét. Ön hello típusát (prémium és Standard) és a szükséges lemez mérete tooonly rendelkezik, és az Azure létrehoz és kezelheti a hello lemez. 

A virtuális általánosított, és speciális feltöltheti. 
- **Virtuális merevlemez általánosítva** -volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el. 
- **Virtuális merevlemez kifejezetten** -hello felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép kezeli. 

> [!IMPORTANT]
> Minden virtuális merevlemez tooAzure feltöltés, előtt kövesse [Windows VHD- vagy VHDX tooupload tooAzure előkészítése](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
>
>


| Forgatókönyv                                                                                                                         | Dokumentáció                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Rendelkezik egy meglévő AWS EC2, hogy sikerült tetszés toomigrate tooAzure felügyelt lemezek                                     | [A virtuális gépek áthelyezése az Amazon Web Services (AWS) tooAzure](aws-to-azure.md)                           |
| A virtuális gép és a más virtualizációs platform, hogy szeretné-e, egy kép toocreate toouse toouse több Azure virtuális gépeken. | [Egy általánosított virtuális merevlemez feltöltéséhez, és ezzel toocreate egy új virtuális gépek Azure-ban](upload-generalized-managed.md) |
| Egy egyedi módon testreszabott virtuális Gépet, hogy szeretné-e az Azure-ban toorecreate rendelkezik.                                                      | [Töltse fel a speciális VHD tooAzure és új virtuális gép létrehozása](create-vm-specialized.md)         |


## <a name="overview-of-managed-disks"></a>Felügyelt lemez – áttekintés

Azure-lemezeket felügyelt egyszerűbb virtuális gép kezelése hello kell toomanage tárfiókok eltávolításával. Által kezelt lemezeken is előnye a rendelkezésre állási csoport a virtuális gépek nagyobb megbízhatóságot. Ez biztosítja, hogy hello lemezek különböző virtuális gépek a rendelkezésre állási csoport elég elkülönül minden más tooavoid egyetlen ponton a hibákat. A rendelkezésre állási csoport a különböző tárolási méretezési egységeit (bélyegek) egy méretezési egység tárolóhibák miatt toohardware és szoftver hibák miatt hello hatását korlátozó különböző virtuális lemezek automatikusan helyezi. A igények alapján választhat a két típusú tárolási lehetőségek közül választhat: 
 
- [Prémium szintű felügyelt lemez](../../storage/common/storage-premium-storage.md) tartós állapot meghajtót (SSD) alapú tárolás tárolási adathordozókon rendszerrel highperformance, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek támogatása. Hello sebesség előnyeit, és ezek a lemezek áttelepítése tooPremium kezelt lemezek teljesítményét is igénybe vehet.  

- [Standard szintű felügyelt lemez](../../storage/common/storage-standard-storage.md) használják a merevlemez-meghajtó (HDD)-alapú tárolási adathordozókon, és a legalkalmasabb fejlesztési és tesztelési célú és egyéb gyakori hozzáférés munkaterhelések kevesebb bizalmas tooperformance variancia.  

## <a name="plan-for-hello-migration-toomanaged-disks"></a>Hello áttelepítés tervezése tooManaged lemezek

Ez a szakasz segítséget nyújt toomake hello legjobb döntést a virtuális gép és a lemez típusok.


### <a name="location"></a>Hely

Jelölje ki a helyet, ahol Azure felügyelt lemezek érhetőek el. TooPremium kezelt lemezek telepít át, ha is győződjön meg arról, hogy prémium szintű storage elérhető hello régióban, ha azt tervezi, hogy toomigrate. Lásd: [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services) elérhető helyről naprakész tájékoztatást.

### <a name="vm-sizes"></a>A virtuális gépek mérete

TooPremium kezelt lemezek telepít át, ha vannak tooupdate hello mérete hello VM tooPremium képes tárméret elérhető hello régió, ahol a virtuális gép. Tekintse át, amelyek képesek a prémium szintű Storage hello Virtuálisgép-méretek. hello Azure virtuális gép mérete specifikációk szereplő [virtuális gépek méretei](sizes.md).
Tekintse át a virtuális gépek, amely együttműködik a prémium szintű Storage, és válassza ki a hello leginkább megfelelő virtuális gép méretét, amely a legjobban megfelel a számítási feladatok hello teljesítményétől. Győződjön meg arról, hogy nincs elegendő sávszélesség érhető el a virtuális gép toodrive hello lemez forgalom.

### <a name="disk-sizes"></a>Lemezméretek

**Prémium szintű felügyelt lemez**

Háromféle együtt a virtuális gép prémium szintű felügyelt lemez van, és mindegyik rendelkezik-e adott iops-érték és átviteli korlátok. Vegye figyelembe a működés felső korlátjának hello prémium szintű lemez típusát a virtuális gép alapján az alkalmazás kapacitása, teljesítmény, méretezhetőség hello igényeinek, és a maximális betölti kiválasztása.

| Prémium szintű lemezek típusa  | P10               | P20               | P30               |
|---------------------|-------------------|-------------------|-------------------|
| Lemezméret           | 128 GB            | 512 GB            | 1024 GB (1 TB)    |
| IOPS-érték lemezenként       | 500               | 2300              | 5000              |
| Adattovábbítás lemezenként | 100 MB / s | 150 MB / s | 200 MB / s |

**Standard szintű felügyelt lemez**

Standard szintű felügyelt lemez, amely a virtuális gép használható öt típusa van. Azok a különböző kapacitással rendelkeznek, de azonos IOPS és átviteli sebességének korlátai. Válassza ki az alkalmazás hello kapacitásigények alapján standard szintű felügyelt lemez hello típusú.

| Standard lemez típusa  | S4               | S6               | S10              | S20              | S30              |
|---------------------|------------------|------------------|------------------|------------------|------------------|
| Lemezméret           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   |
| IOPS-érték lemezenként       | 500              | 500              | 500              | 500              | 500              |
| Adattovábbítás lemezenként | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s |

### <a name="disk-caching-policy"></a>Lemez gyorsítótárazási házirend 

**Prémium szintű felügyelt lemez**

Alapértelmezés szerint a gyorsítótárazási házirend lemez van *írásvédett* az összes hello prémium adatlemezek, és *írható-olvasható* hello prémium operációsrendszer-lemez csatolni a virtuális gép toohello. A konfigurációs beállítás ajánlott tooachieve hello optimális teljesítménye az alkalmazás IOs-hez. Írási műveleteket vagy a csak írható adatlemezek (köztük SQL Server) tiltsa le a lemezt gyorsítótárazás, hogy az alkalmazás jobb teljesítményt érhet el.

### <a name="pricing"></a>Díjszabás

Felülvizsgálati hello [kezelt lemezek árképzési](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Prémium szintű felügyelt lemez árképzési legyen, mint hello nem felügyelt Premium lemezek. Azonban a standard szintű felügyelt lemez árképzési más nem felügyelt Standard lemezeknél.


## <a name="next-steps"></a>Következő lépések

- Minden virtuális merevlemez tooAzure feltöltés, előtt kövesse [Windows VHD- vagy VHDX tooupload tooAzure előkészítése](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
