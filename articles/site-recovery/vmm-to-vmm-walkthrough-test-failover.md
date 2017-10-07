---
title: "a Hyper-V virtuális gép replikációs tooa másodlagos hely az Azure Site Recovery feladatátvételi teszt aaaRun |} Microsoft Docs"
description: "Ismerteti, hogyan toorun Hyper-V virtuális gép replikációs tooa feladatátvételi tesztet másodlagos System Center VMM helyet az Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a3fd11ce-65a1-4308-8435-45cf954353ef
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: a60411541c2db001c573735bcb0d651f6618f295
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-tooa-secondary-site"></a>10. lépés: Hyper-V replikáció tooa másodlagos hely feladatátvételi teszt futtatása


Miután engedélyezte a Hyper-V virtuális gépek (VM) replikációs rendelkező [Azure Site Recovery](site-recovery-overview.md), ez a cikk toorun feladatátvételi teszt használja. Feladatátvételi teszt ellenőrzi, hogy a replikáció működik, az éles környezetben befolyásolása nélkül. 


A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Előkészületek

- Amikor egy feladatátvételi teszt folyamatban váltanak, megadhatja a hello hálózati toowhich hello tesztreplikaként működő virtuális gépek csatlakozni fognak. [További](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) hello hálózati beállításokkal kapcsolatos.
- Azt javasoljuk, hogy egy hálózati hálózatra való leképezés során kiválasztott nem választja.
- Ebben a cikkben hello útmutatások mutatják be, hogyan toofail egy virtuális keresztül. További információ a [a helyreállítási terv létrehozása](site-recovery-create-recovery-plans.md) Ha azt szeretné, toofail át több virtuális gép együtt.
- További információ a [teszteket korlátozásai](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).
- hello tooa virtuális gép feladatátvételi tesztje során megadott IP-cím hello azonos IP-címet, amely a virtuális gép hello kapja a tervezett vagy nem tervezett feladatátvételre (feltéve, hogy hello IP-cím érhető el a feladatátvételi teszt hálózatában hello). Ha hello IP-cím nem érhető el a feladatátvételi teszt hálózatában hello, hello VM kap egy másik IP-címet, amely a feladatátvételi teszt hálózatában hello érhető el.
- Ha a végpontok közötti hálózati kapcsolat gép teljes validatation toodo feladatátvételi teszt szeretné az éles hálózattól, vegye figyelembe, hogy:
    - elsődleges virtuális gép le kell állítani akkor, ha a feladatátvételi teszt hello hello. Ellenkező esetben két virtuális gépek ugyanazzal az identitással fog futni hello hello azonos hálózati: hello ugyanannyi időt vesz igénybe. 
    - Ha módosítja tootest virtuális gépeket, ezeket a módosításokat is elvesznek, tesztelésekor karbantartása hello feladatátvételi. Ezek a változások nem replikált hátsó toohello elsődleges virtuális gép.
    - Tesztelési egy éles hálózati környezetben a termelési számítási feladatokhoz toodowntime vezet. Kérje meg a felhasználók nem toouse hello app Ha hello vész-helyreállítási részletezési van folyamatban.  


## <a name="run-a-test-failover-for-a-vm"></a>A virtuális gépek feladatátvételi teszt futtatása

1. egy virtuális, keresztül toofail a **replikált elemek**, kattintson a virtuális gép hello > **feladatátvételi teszt**.
2. A **feladatátvételi teszt**, adja meg, hogyan teszt virtuális gépek lesznek csatlakoztatott toonetworks hello a feladatátvételi teszt után. 
3. Kattintson a **OK** toobegin hello feladatátvételi. Nyomon követni a hello **feladatok** fülre.
5. Miután a feladatátvétel befejeződött, győződjön meg arról, hogy hello teszt virtuális gépek elindítása sikeresen.
6. Amikor elkészült, kattintson a **karbantartása a feladatátvételi teszt** a hello helyreállítási terv.
7. A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez. Ez a lépés törli az hello virtuális gépek és a hálózatokban, amelyek a feladatátvételi teszt során lettek létrehozva.


## <a name="next-steps"></a>Következő lépések

Hello telepítés tesztelését, követően további információ más típusú [feladatátvételi](site-recovery-failover.md).
