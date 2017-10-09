---
title: a Site Recovery aaaMigrate tooAzure |} Microsoft Docs
description: "Ez a cikk áttekintést áttelepítése virtuális gépek és fizikai kiszolgálók tooAzure az Azure Site Recovery szolgáltatással"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/05/2017
ms.author: raynew
ms.openlocfilehash: 6e65deee337c5371d441812ddb820dc8bc233684
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-tooazure-with-site-recovery"></a>A Site Recovery tooAzure áttelepítése

A cikkből megtudhatja a virtuális gépek és fizikai kiszolgálók áttelepítéséhez hello Azure Site Recovery szolgáltatással.

Webhely-helyreállítás egy Azure szolgáltatása tooyour BCDR stratégia megvalósításában replikáció a helyszíni fizikai kiszolgálóknak és virtuális gépek toohello Azure-felhőbe vagy tooa másodlagos adatközpontba. Az elsődleges helyen valamilyen okból kimaradás lép fel, ha feladatátvételt toohello másodlagos helyre tookeep alkalmazások és számítási feladatok nem állnak. Ön nem hátsó tooyour elsődleges hely toonormal műveleteket a rendszer visszaadja. További információ: [Mi a Site Recovery?](site-recovery-overview.md) A Site Recovery toomigrate a meglévő helyszíni munkaterhelések tooAzure tooexpedite a felhő utazás és az elérhető funkciókat, amelyek az Azure kínál tömbje hello is használható.

A gyors áttekintést a tooperform áttelepítési, tekintse meg a toothis videó.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]

Ez a cikk ismerteti a hello telepítési [Azure-portálon](https://portal.azure.com). Hello [a klasszikus Azure portálon](https://manage.windowsazure.com/) is használt toomaintain létező Site Recovery-tárolóhoz, de nem hozható létre új tárolók.

Ez a cikk hello alsó megjegyzések utáni. Technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="what-do-we-mean-by-migration"></a>Mit értünk áttelepítés alatt?

A Site Recovery replikációja, a helyszíni virtuális gépek és fizikai kiszolgálók, tooAzure vagy tooa másodlagos hely telepítése. Gépek, replikálása feladatátvételt őket hello elsődleges helyről kimaradások esetén fordulhat elő, és azokat hátsó toohello elsődleges hely sikertelen azt állítja helyre. Továbbá toothis, a használhatja a Site Recovery toomigrate virtuális gépek és fizikai kiszolgálók tooAzure, hogy a felhasználók érhetik el azokat, Azure virtuális gépeken. Áttelepítési replikációs, és a feladatátvétel az elsődleges hely tooAzure hello és az áttelepítés végrehajtásához kézmozdulatok terjed ki.

## <a name="what-can-site-recovery-migrate"></a>Melyen típusú áttelepítéseket lehet elvégezni a Site Recoveryvel?

A következőket teheti:

- Telepítse át a helyszíni Hyper-V virtuális gépek, VMware virtuális gépek és fizikai kiszolgálók toorun Azure virtuális gépeken futó számítási feladatok. Ebben a forgatókönyvben teljes replikációt és feladat-visszavételt is végrehajthat.
- Áttelepíthet [Azure IaaS virtuális gépeket](site-recovery-migrate-azure-to-azure.md) Azure-régiók között. Ebben az esetben jelenleg csak az áttelepítés támogatott, a feladat-visszavétel nem.
- Telepítse át [AWS Windows-példányok](site-recovery-migrate-aws-to-azure.md) tooAzure IaaS virtuális gépeket. Ebben az esetben jelenleg csak az áttelepítés támogatott, a feladat-visszavétel nem.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Helyszíni virtuális gépek és fizikai kiszolgálók áttelepítése

toomigrate helyszíni Hyper-V virtuális gépek, a VMware virtuális gépek és fizikai kiszolgálók, szinte ugyanaz, mint a szokásos replikáció lépések hello követi.

1. Helyreállítási tár beállítása
2. Konfigurálja a szükséges hello felügyeleti kiszolgálókat (VMware, VMM, Hyper-V - attól függően, hogy mit toomigrate), toohello tároló adja hozzá, és adja meg a replikációs beállításokat.
3. Hello gépeket replikációs engedélyezni szeretné, hogy toomigrate
4. Hello kezdeti az áttelepítés után futtassa a Gyorsellenőrzés feladatátvételi tooensure, hogy legyen, vagy ha minden megfelelően működik.
5. A replikációs környezet működésének ellenőrzése után, attól függően, hogy a forgatókönyv [mit támogat](site-recovery-failover.md), indítson el egy tervezett vagy nem tervezett feladatátvételt. Azt javasoljuk, használjon egy tervezett feladatátvételt, ha lehetséges.
6. Az áttelepítéshez akkor nem kell toocommit feladatátvétel, vagy törölje azt. Ehelyett válassza ki az hello **az áttelepítés végrehajtásához** beállítást minden gép toomigrate keresi.
     - A **replikált elemek**, kattintson a jobb gombbal a virtuális gép hello, és kattintson a **az áttelepítés végrehajtásához**. Kattintson a **OK** toocomplete. Előrehaladásának hello virtuális gép tulajdonságai, a teljes áttelepítési feladat hello figyelésével **Site Recovery-feladatok**.
     - Hello **az áttelepítés végrehajtásához** művelet hello áttelepítési folyamat befejezését, eltávolítja hello gép replikálását, és leállítja a Site Recovery számlázási hello gép.

![teljesáttelepítés](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

## <a name="migrate-between-azure-regions"></a>Áttelepítés Azure-régiók között

A Site Recoveryvel Azure-régiók között is áttelepíthet Azure virtuális gépeket. Ebben a forgatókönyvben jelenleg csak az áttelepítés támogatott. Más szóval hello Azure virtuális gépek replikálása, és azokat átadja tooanother régió, de nem utasíthat el vissza. Ebben a forgatókönyvben beállította a Recovery Services-tároló telepítheti egy helyszíni konfigurációs kiszolgáló toomanage replikálásra, toohello tároló adja hozzá és adja meg a replikációs beállításokat. A feladatátvételi teszt hello gépek toomigrate szeretne, és futtatni egy gyors engedélyezheti a replikálást. Ezután hello lefuttatta a nem tervezett feladatátvétel **az áttelepítés végrehajtásához** lehetőséget.

## <a name="migrate-aws-tooazure"></a>Telepítse át az AWS tooAzure

AWS példányok tooAzure virtuális gépeket telepíthet át. Ebben a forgatókönyvben jelenleg csak az áttelepítés támogatott. Más szóval hello AWS példányok replikálja, és azokat átadja tooAzure, de nem utasíthat el vissza. AWS példányok kezeli, hello ugyanaz, mint a fizikai kiszolgálók az áttelepítéshez. Recovery Services-tároló beállításában, telepítheti egy helyszíni konfigurációs kiszolgáló toomanage replikálásra, toohello tároló adja hozzá, és adja meg a replikációs beállításokat. A feladatátvételi teszt hello gépek toomigrate szeretne, és futtatni egy gyors engedélyezheti a replikálást. Ezután hello lefuttatta a nem tervezett feladatátvétel **az áttelepítés végrehajtásához** lehetőséget.




## <a name="next-steps"></a>Következő lépések

- [VMware virtuális gépek tooAzure áttelepítése](site-recovery-vmware-to-azure.md)
- [A VMM-felhők tooAzure Hyper-V virtuális gépek áttelepítése](site-recovery-vmm-to-azure.md)
- [Telepítse át a Hyper-V virtuális gépek VMM tooAzure nélkül](site-recovery-hyper-v-site-to-azure.md)
- [Azure virtuális gépek áttelepítése Azure-régiók között](site-recovery-migrate-azure-to-azure.md)
- [Telepítse át az AWS példányok tooAzure](site-recovery-migrate-aws-to-azure.md)
- [Készítse elő az áttelepített gépek tooenable replikációs](site-recovery-azure-to-azure-after-migration.md) tooanother régió vész-helyreállítási igényekre.
- Gondoskodjon számítási feladatai védelméről az [Azure virtuális gépek replikálásával](site-recovery-azure-to-azure.md).
