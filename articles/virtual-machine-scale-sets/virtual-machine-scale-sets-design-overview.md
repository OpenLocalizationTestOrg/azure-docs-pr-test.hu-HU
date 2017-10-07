---
title: "Azure virtuálisgép-méretezési csoportok szempontjai aaaDesign |} Microsoft Docs"
description: "További tudnivalók az Azure virtuálisgép-méretezési csoportok kialakítási szempontjai"
keywords: "Linux virtuális gép, virtuálisgép-méretezési beállítása"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: c27c6a59-a0ab-4117-a01b-42b049464ca1
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: f8644d36fe5903bd4b74df26dca5dc3211ee3516
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-considerations-for-scale-sets"></a>Méretezési csoportok kialakítási szempontjai
Ebben a témakörben ismertetett tervezési megfontolások a virtuálisgép-méretezési készlet. Mik azok a virtuálisgép-méretezési csoportok kapcsolatos információkért tekintse meg túl[virtuális gépek méretezési készletek áttekintése](virtual-machine-scale-sets-overview.md).

## <a name="when-toouse-scale-sets-instead-of-virtual-machines"></a>Ha toouse skálázási készletekben helyett virtuális gépek?
Általában méretezési csoportok hasznosak magas rendelkezésre állású infrastruktúra üzembe helyezéséhez ahol gépek halmaza hasonló konfigurációval rendelkezik. Azonban bizonyos funkciók találhatók csak méretezési csoportok míg egyéb szolgáltatásokat csak a virtuális gépek. A rendezés toomake tájékozott döntést arról, hogy mikor toouse egyes technológiák azt kell először tekintse meg néhány méretezési csoportok, de nem a virtuális gépek rendelkezésre álló általánosan használt hello szolgáltatást:

### <a name="scale-set-specific-features"></a>Méretezési készlet-specifikus szolgáltatásai

- Hello méretezési készlet konfigurációja adja meg, ha egyszerűen frissíthető hello "kapacitása" tulajdonság toodeploy további virtuális gépek párhuzamosan. Ez egy sokkal egyszerűbb, mint a írása egy parancsfájl tooorchestrate párhuzamosan sok egyes virtuális gépek telepítése.
- Is [használata Azure automatikus skálázás tooautomatically méretezése a méretezési](./virtual-machine-scale-sets-autoscale-overview.md) , de nem az egyes virtuális gépek.
- Is [lemezkép-visszaállítási méretezési virtuális gépek](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-a-vm) , de [nem az egyes virtuális gépek](https://docs.microsoft.com/rest/api/compute/virtualmachines).
- Is [szükségesnél több erőforrás](./virtual-machine-scale-sets-design-overview.md) méretezési virtuális gépek nagyobb megbízhatóságot és gyorsabb telepítési időpontokat. Nem ehhez az egyes virtuális gépek csak írt egyéni kód toodo ez.
- Megadhat egy [házirend frissítése](./virtual-machine-scale-sets-upgrade-scale-set.md) toomake azt a méretezési csoportban lévő virtuális gépek között frissítésének könnyen tooroll. Az egyes virtuális gépeken meg kell levezényelni a frissítéseket magát.

### <a name="vm-specific-features"></a>VM-specifikus szolgáltatásai

A hello ugyanakkor, néhány funkció csak a virtuális gépek érhető el (legalább a hello jelenleg):

- Adatok lemezek toospecific csatolhat méretezési csoportban lévő összes virtuális gép egyes virtuális gépeken, de a mellékelt adatok lemezek vannak beállítva.
- Csatolhat a szolgáltatáskéréshez nem üres adatok lemezek tooindividual virtuális gépeket, de nem méretezési csoportban lévő virtuális gépek.
- Egy adott virtuális Gépre, de nem egy Virtuálisgép-méretezési csoportban lévő lehet pillanatkép.
- Az egy adott virtuális Gépre, de nem egy Virtuálisgép-méretezési csoportban lévő lemezkép rögzítheti.
- Natív lemezek toomanaged lemezek telepíthet át egy adott virtuális Gépre, de nem ehhez a virtuális gépek méretezési csoportban lévő.
- Rendelhet hozzá az IPv6 nyilvános IP-címek tooindividual virtuális gép hálózati adapterek, de nem ehhez a virtuális gépek méretezési csoportban lévő. Vegye figyelembe, hogy rendelhet IPv6 nyilvános IP-címek tooload terheléselosztók elé vagy az egyes virtuális gépek vagy virtuális gépek méretezési csoportjának virtuális gépeket.

## <a name="storage"></a>Storage

### <a name="scale-sets-with-azure-managed-disks"></a>Az Azure Managed lemezek méretezési csoportok
Méretezési csoportok hozhatók létre [Azure felügyelt lemezek](../virtual-machines/windows/managed-disks-overview.md) hagyományos Azure storage-fiókok helyett. Felügyelt lemezek hello a következő előnyöket biztosítják:
- Nem rendelkezik toopre-hello méretezési virtuális gépeket az Azure storage-fiókokat létrehozni.
- Megadhat [adatlemezt csatolni](virtual-machine-scale-sets-attached-disks.md) hello virtuális gépek a méretezési beállítása.
- Méretezési készlet túl konfigurálható[támogatja a too1, egy 000 virtuális gépek](virtual-machine-scale-sets-placement-groups.md). 

Ha egy meglévő sablont, is [hello sablon toouse felügyelt lemezek frissítése](virtual-machine-scale-sets-convert-template-to-md.md).

### <a name="user-managed-storage"></a>Felhasználó által felügyelt tárolási
Egy Azure felügyelt lemezzel nem definiált méretezési felhasználó által létrehozott fiókok toostore hello OS tárolólemezek hello virtuális gépek hello készlet támaszkodik. Storage-fiókot, vagy kevesebb mint 20 virtuális gép arányú tooachieve maximális IO, és használja ki az ajánlott _elhelyezésétől_ (lásd alább). Ajánlott továbbá hello tárfiókok neve elején karakterét hello elosztva hello ábécé. Ennek segítségével terhelés elosztva különböző belső rendszerek során. 


## <a name="overprovisioning"></a>Elhelyezésétől
Skálázási készletekben jelenleg alapértelmezett túl "elhelyezésétől" virtuális gépeket. Elhelyezésétől-e kapcsolva, hello méretezési ténylegesen fordulat további virtuális gépeinek mint kéri be, majd törli az extra virtuális Gépekért, miután hello irányuló kérés. a virtuális gépek számát sikeresen telepített hello. Elhelyezésétől növeli a kiépítési sikerességéről, és csökkenti a központi telepítés alkalmával. A hello extra virtuális Gépekért, és ezek nem számítanak a kvótakorlát felé nem kell fizetni.

Amíg elhelyezésétől javításához létesítési sikerességéről okozhat zavaró viselkedést az alkalmazás, amely nem tervezett toohandle extra virtuális Gépekért jelenik meg, és ezután eltűnőként van. tooturn elhelyezésétől ki, ellenőrizze, hogy a sablonban lévő karakterlánc a következő hello: `"overprovision": "false"`. További részletek találhatók a hello [méretezési beállítása REST API-dokumentáció](/rest/api/virtualmachinescalesets/create-or-update-a-set).

Ha a méretezési felhasználó által felügyelt tárolót használ, és elhelyezésétől kikapcsolja, rendelkezhet több mint 20 gépek, de nem ajánlott toogo fent 40 IO teljesítményének javítására szolgál. 

## <a name="limits"></a>Korlátok
A méretezési a Piactéri rendszerkép (más néven platformlemezkép) alapul, és Azure felügyelt lemezek támogatja too1, 000 virtuális gép mentése kapacitású toouse konfigurálva. A méretezési készlet toosupport 100-nál több virtuális gép konfigurálása, ha nem minden forgatókönyvek munkahelyi hello azonos (például terheléselosztás). További információkért lásd: [nagy virtuálisgép-méretezési csoportok használata](virtual-machine-scale-sets-placement-groups.md). 

Egy méretezési készletben beállított felhasználó által felügyelt tárfiókokkal jelenleg korlátozott too100 virtuális gépek (és a skála ajánlott 5 storage-fiókok).

Egy egyéni lemezkép (egy Ön által létrehozott) épülő méretezési too100 virtuális gépeinek, amikor Azure felügyelt lemezzel konfigurált kapacitású lehet. Hello méretezési felhasználó által felügyelt tárfiókok van beállítva, ha az összes operációs rendszer lemez VHD-k egy tárfiókon belül kell létrehoznia. Ennek eredményeképpen hello maximális ajánlott egyéni lemezkép épülő méretezési csoportban lévő virtuális gépek száma és felhasználó által felügyelt tárolási 20. Ha kikapcsolja elhelyezésétől, lépjen be too40.

További virtuális gépek esetén, mint a működés felső korlátjának engedélyezéséhez szüksége több skálázási készletekben, ahogy az toodeploy [sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).

