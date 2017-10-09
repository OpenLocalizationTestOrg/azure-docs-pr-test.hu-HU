---
title: "Azure virtuális gépek aaaDisaster helyreállítási forgatókönyvek |} Microsoft Docs"
description: "Ismerje meg, milyen toodo, hogy az Azure szolgáltatás megszűnésének hatással van az Azure virtuális gépek hello eseményben."
services: virtual-machines
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: kmouss;aglick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 839c6f9c51a7f35b48ef3636bc346eef332bb06d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-toodo-in-hello-event-that-an-azure-service-disruption-impacts-azure-vms"></a>Milyen toodo hello esemény, hogy az Azure szolgáltatás megszűnésének befolyásolja-e az Azure virtuális gépeken
A Microsoft dolgozunk rögzített toomake meg arról, hogy a szolgáltatások mindig elérhető tooyou újból. Kényszeríti a befolyásolható néha hatással velünk, hogy a nem tervezett szolgáltatás megszakadása miatt.

A Microsoft egy szolgáltatási szint szerződés (SLA) biztosít hasznos üzemidő és csatlakozási kötelezettségvállalást a hozzá tartozó szolgáltatások. az egyes Azure-szolgáltatások SLA hello található [Azure szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).

Azure már számos beépített platform szolgáltatást, amely támogatja a magas rendelkezésre állású alkalmazások. További információk a szolgáltatásokról, olvassa el a [vész-helyreállítási és magas rendelkezésre állás a Azure-alkalmazások](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Ez a cikk ismerteti a valódi katasztrófa utáni helyreállítás, amikor egy teljes régió megfelelő toomajor természeti katasztrófa vagy szélesebb körben szolgáltatáskiesést kimaradás. Ezek ritkán fordul elő eseményeket, de előbb elő kell készítenie az, hogy van-e egy teljes terület kimaradás hello lehetőségét. Ha egy teljes terület a szolgáltatás szüneteltetése, hello helyileg redundáns másolatok adatait szeretné ideiglenesen nem érhető el. Ha engedélyezett a georeplikáció, az Azure Storage blobs és táblák három további másolatot tárol egy másik régióban. Azure hello esemény teljes regionális kimaradás vagy egy olyan vészhelyzet esetén milyen hello elsődleges régió nincs helyreállítható, amelyek az összes hello DNS bejegyzés toohello georeplikált régióban.

kezeli az ilyen ritka események toohelp, nyújtunk útmutató az Azure virtuális gépek hello esetében a szolgáltatás szüneteltetése hello teljes régió, ahol a virtuális gép az Azure-alkalmazás központi telepítése a következő hello.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>1. lehetőség: A feladatátvétel kezdeményezése Azure Site Recovery segítségével
Azure Site Recovery konfigurálhatja a virtuális gépek, így helyreállíthatja az alkalmazás, függetlenül attól, hogy az egyetlen kattintással perc alatt. Az Ön által választott tooAzure régió, és nem korlátozott toopaired régiók replikálhatja. Elkezdheti által [a virtuális gépek replikálásához](https://aka.ms/a2a-getting-started). Is [helyreállítási terv létrehozása](../site-recovery/site-recovery-create-recovery-plans.md) , hogy az alkalmazás hello teljes feladatátvételi folyamat automatizálható. Is [a feladatátvételi tesztek](../site-recovery/site-recovery-test-failover-to-azure.md) előre éles alkalmazás vagy hello folyamatban lévő replikáció befolyásolása nélkül. A elsődleges régió szüneteltetése hello esetben most [kezdeményezzen feladatátvételt](../site-recovery/site-recovery-failover.md) , és az alkalmazás cél régióban.


## <a name="option-2-wait-for-recovery"></a>2. lehetőség: Várjon, amíg a helyreállítás
Ebben az esetben a letöltés intézkedés nem szükséges. Ismerje meg, hogy jelenleg dolgozunk gondossággal toorestore szolgáltatás rendelkezésre állása. A szolgáltatás jelenlegi állapota hello tekintheti meg a [Azure az állapotjelző irányítópulthoz](https://azure.microsoft.com/status/).

Ez az hello legjobb lehetőség, ha nem állított be Azure Site Recovery, írásvédett georedundáns tárolás vagy georedundáns tárolás előzetes toohello megszakítása. Állított be georedundáns tárolás vagy írásvédett georedundáns tárolás hello tárfiók a virtuális gép virtuális merevlemezek (VHD) tároló, ha toorecover hello alapjául szolgáló lemezképhez VHD keresse meg, és próbálja tooprovision belőle egy új virtuális Gépet. Ez a beállítás nem előnyben részesített nem garantálja az adatok szinkronizálását, mert. Ezért ezt a beállítást nem garantált toowork.


> [!NOTE]
> Vegye figyelembe, hogy nem tudja befolyásolni bármely ezt a folyamatot, és akkor történik a régió kiterjedő szolgáltatás üzemzavarokhoz vezethet. Ebből kifolyólag kell is használ, más alkalmazás-specifikus biztonsági mentési stratégia tooachieve hello legmagasabb rendelkezésre állási szintet. További információkért lásd: hello szakasz a [vész-helyreállítási adatok stratégiák](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications#data-strategies-for-disaster-recovery).
>
>

## <a name="next-steps"></a>Következő lépések

- Start [védelme az Azure virtuális gépeken futó alkalmazások](https://aka.ms/a2a-getting-started) Azure Site Recovery segítségével

- További információk hogyan tooimplement vész-helyreállítási és magas rendelkezésre állási stratégiájának: toolearn [vész-helyreállítási és magas rendelkezésre állás a Azure-alkalmazások](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

- a felhő platform képességei, részletes műszaki megértése toodevelop lásd [Azure rugalmassági műszaki útmutatót](../resiliency/resiliency-technical-guidance.md).


- Ha hello utasítások nem egyértelmű, vagy ha azt szeretné, hogy a Microsoft toodo hello műveletek az Ön nevében, forduljon a [ügyfél-támogatási](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
