---
title: "egy Azure-szolgáltatások ideiglenesek, amely hatással van az Azure Felhőszolgáltatások hello eseményben aaaWhat toodo |} Microsoft Docs"
description: "Ismerje meg, milyen toodo egy Azure-szolgáltatások ideiglenesek, amely hatással van az Azure Felhőszolgáltatások hello eseményben."
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: e52634ab-003d-4f1e-85fa-794f6cd12ce4
ms.service: cloud-services
ms.workload: cloud-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: mmccrory
ms.openlocfilehash: bb1f1835fc91a23772f81801b67d5786c190ba19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-toodo-in-hello-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Milyen toodo hello eseményben egy Azure-szolgáltatások ideiglenesek, amely hatással van az Azure Cloud Services csomag
A Microsoft dolgozunk rögzített toomake meg arról, hogy a szolgáltatások mindig elérhető tooyou újból. Kényszeríti a befolyásolható néha hatással velünk, hogy a nem tervezett szolgáltatás megszakadása miatt.

A Microsoft egy szolgáltatási szint szerződés (SLA) biztosít hasznos üzemidő és csatlakozási kötelezettségvállalást a hozzá tartozó szolgáltatások. az egyes Azure-szolgáltatások SLA hello található [Azure szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).

Azure már számos beépített platform szolgáltatást, amely támogatja a magas rendelkezésre állású alkalmazások. További információk a szolgáltatásokról, olvassa el a [vész-helyreállítási és magas rendelkezésre állás a Azure-alkalmazások](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Ez a cikk ismerteti a valódi katasztrófa utáni helyreállítás, amikor egy teljes régió megfelelő toomajor természeti katasztrófa vagy szélesebb körben szolgáltatáskiesést kimaradás. Ezek ritkán fordul elő eseményeket, de előbb elő kell készítenie az, hogy van-e egy teljes terület kimaradás hello lehetőségét. Ha egy teljes terület a szolgáltatás szüneteltetése, hello helyileg redundáns másolatok adatait szeretné ideiglenesen nem érhető el. Ha engedélyezett a georeplikáció, az Azure Storage blobs és táblák három további másolatot tárol egy másik régióban. Azure hello esemény teljes regionális kimaradás vagy egy olyan vészhelyzet esetén milyen hello elsődleges régió nincs helyreállítható, amelyek az összes hello DNS bejegyzés toohello georeplikált régióban.

> [!NOTE]
> Vegye figyelembe, hogy nem tudja befolyásolni bármely ezt a folyamatot, és csak megtörténik az Adatközpont kiterjedő szolgáltatás üzemzavarokhoz vezethet. Ebből kifolyólag kell is használ, más alkalmazás-specifikus biztonsági mentési stratégia tooachieve hello legmagasabb rendelkezésre állási szintet. További információkért lásd: [vész-helyreállítási és magas rendelkezésre állás a Microsoft Azure épülő alkalmazások](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md). Ha szeretné tudni tooaffect toobe saját feladatátvételt, érdemes lehet tooconsider hello használata [írásvédett georedundáns tárolás (RA-GRS)](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage), amely az adatok írásvédett másolatot készít egy másik régióban.
>
>


## <a name="option-1-use-a-backup-deployment-through-azure-traffic-manager"></a>1. lehetőség: A biztonsági mentési központi telepítés keresztül Azure Traffic Manager használata
hello legtöbb hatékony vész-helyreállítási megoldást magában foglalja a karbantartása több különböző régiókban az alkalmazás központi telepítését, majd használatával [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) toodirect forgalom közöttük. Az Azure Traffic Manager biztosít több [útválasztási metódusait](../traffic-manager/traffic-manager-routing-methods.md), így dönthet úgy, hogy toomanage a üzembe helyezése egy elsődleges, illetve biztonsági mentési modell vagy toosplit forgalom közötti őket.

![Azure Cloud Services az Azure Traffic Manager régiók közötti terheléselosztás](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

Hello leggyorsabb válasz toohello megszűnését egy régiót, fontos, hogy konfigurálja a Traffic Manager [végpontmonitoring kijelző](../traffic-manager/traffic-manager-monitoring.md).

## <a name="option-2-deploy-your-application-tooa-new-region"></a>2. lehetőség: Az alkalmazás tooa új régióban üzembe helyezése
Több aktív központi telepítés fenntartása a hello előző beállítás azt eredményezi, a folyamatban lévő többletköltséggel azok háromszorosa. Ha a helyreállítási idő célkitűzése (RTO) elég rugalmas és hello eredeti kód vagy a lefordított Cloud Services csomag van, az az alkalmazás egy új példányának létrehozása egy másik régióban, és frissítse a DNS-rekordok toopoint toohello új telepítés.

Részletesebben arról, hogyan toocreate és a cloud service-alkalmazás központi telepítése, lásd: [hogyan toocreate és központi telepítése egy felhőalapú szolgáltatás](cloud-services-how-to-create-deploy-portal.md).

Attól függően, hogy az alkalmazás-adatforrások szükség lehet az toocheck hello helyreállítási eljárásokat az alkalmazási adatforrás esetében.

* Az Azure Storage adatforrásokat, lásd: [Azure Storage replikációs](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage) toocheck hello rendelkezésre álló lehetőségek hello alapján a választott replikációs modellt az alkalmazáshoz.
* SQL-adatbázis adatforrások, olvassa el a [áttekintése: üzleti folytonossági és az adatbázis katasztrófa utáni helyreállítás az SQL Database Felhőbeli](../sql-database/sql-database-business-continuity.md) toocheck hello lehetőségek állnak rendelkezésre a választott hello replikációs modellt az alkalmazás alapján.


## <a name="option-3-wait-for-recovery"></a>3. lehetőség: Várjon, amíg a helyreállítás
Ebben az esetben nincs teendő beavatkozást, de a szolgáltatás nem lesz elérhető hello régió visszaállításáig. Hello tekintheti meg a szolgáltatás jelenlegi állapota hello [Azure az állapotjelző irányítópulthoz](https://azure.microsoft.com/status/).

## <a name="next-steps"></a>Következő lépések
További információk hogyan tooimplement vész-helyreállítási és magas rendelkezésre állási stratégiájának: toolearn [vész-helyreállítási és magas rendelkezésre állás a Azure-alkalmazások](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

a felhő platform képességei, részletes műszaki megértése toodevelop lásd [Azure rugalmassági műszaki útmutatót](../resiliency/resiliency-technical-guidance.md).
