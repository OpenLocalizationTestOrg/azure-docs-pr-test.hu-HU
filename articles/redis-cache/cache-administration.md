---
title: Azure Redis Cache aaaHow tooadminister |} Microsoft Docs
description: "Ismerje meg, hogyan tooperform felügyeleti feladatokat, mint újraindul, és az Azure Redis Cache frissítések ütemezése"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 8c915ae6-5322-4046-9938-8f7832403000
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 07/05/2017
ms.author: sdanie
ms.openlocfilehash: eb24668a3f6264444e7d4daf1ac43b41b12dfe66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadminister-azure-redis-cache"></a>Hogyan tooadminister Azure Redis Cache-gyorsítótár
Ez a témakör ismerteti, hogyan tooperform felügyeleti feladatokat, mint [újraindítás](#reboot) és [frissítések ütemezése](#schedule-updates) az Azure Redis Cache-példányok.

## <a name="reboot"></a>Újraindítás
Hello **újraindítás** panel tooreboot lehetővé teszi a gyorsítótár egy vagy több csomópontján. Az újraindítás funkció lehetővé teszi, hogy Ön tootest a rugalmasságot az alkalmazás egy gyorsítótár-csomópont hibája esetén.

![Újraindítás](./media/cache-administration/redis-cache-administration-reboot.png)

Válassza ki a hello csomópontok tooreboot, és kattintson a **újraindítás**.

![Újraindítás](./media/cache-administration/redis-cache-reboot.png)

Ha a prémium szintű gyorsítótár fürtözési engedélyezve van, mely szilánkok hello gyorsítótár tooreboot a választhatja meg.

![Újraindítás](./media/cache-administration/redis-cache-reboot-cluster.png)

tooreboot a gyorsítótár egy vagy több csomópontján válasszon szükséges hello csomópontot, majd kattintson a **újraindítás**. Ha a prémium szintű gyorsítótár fürtözési engedélyezve van, jelölje ki a kívánt hello szilánkok tooreboot, és kattintson **újraindítás**. Néhány perc elteltével hello a kiválasztott csomópontok újraindítás, és néhány percen belül újra online állapotba kerülnek.

ügyfélalkalmazások hello gyakorolt változó attól függően, hogy mely csomópontok, hogy indítsa újra.

* **Fő** – Ha hello főcsomópont újraindított, az Azure Redis Cache sikertelen toohello replika csomópont felett áll, és elősegíthető a azt toomaster. A feladatátvétel során lehet egy rövid időszak, amelyben a kapcsolatok toohello gyorsítótár meghiúsulhat.
* **Az alárendelt** – Ha hello az alárendelt csomópont újraindítása után, általában nincs nem gyakorolt hatás toocache ügyfelek.
* **Fő és a kisegítő** – Ha mindkét gyorsítótár-csomópont újraindítása van, minden adatot elvesznek hello gyorsítótár és a kapcsolatok toohello gyorsítótár sikertelen lesz mindaddig, amíg az elsődleges csomópont hello ismét online elérhető lesz. Ha már konfigurált [adatmegőrzés](cache-how-to-premium-persistence.md), hello legfrissebb biztonsági másolat visszaállítása hello gyorsítótár ismét online elérhető, de bármilyen gyorsítótár írás hello legutóbbi biztonsági mentés óta történt elvesznek.
* **A prémium csomópontja gyorsítótár fürtözési engedélyezve** – Ha indítsa újra egy vagy több csomópontot az fürtözési prémium gyorsítótár engedélyezve van, hello kijelölt hello viselkedése csomópontok van hello azonos, ha újraindítás hello megfelelő csomópont vagy egy nem fürtözött csomópontja gyorsítótár.

> [!IMPORTANT]
> Minden tarifacsomagok Újraindítás most érhető el.
> 
> 

## <a name="reboot-faq"></a>Indítsa újra a gyakori kérdések
* [Melyik csomópontján kell újraindítása tootest az alkalmazásban?](#which-node-should-i-reboot-to-test-my-application)
* [Újraindítás hello gyorsítótár tooclear ügyfélkapcsolatokat is?](#can-i-reboot-the-cache-to-clear-client-connections)
* [I adat elvész a gyorsítótárból, ha a számítógép újraindítása a teendő?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [Újraindíthatja a gyorsítótár, a PowerShell, a parancssori felületen vagy a más felügyeleti eszközökhöz?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
* [Milyen árképzési tiers hello újraindítás funkciót használhatja a?](#what-pricing-tiers-can-use-the-reboot-functionality)

### <a name="which-node-should-i-reboot-tootest-my-application"></a>Melyik csomópontján kell újraindítása tootest az alkalmazásban?
az alkalmazás leállása ellen hello elsődleges csomópont a gyorsítótár, újraindítás hello tootest hello rugalmassági **fő** csomópont. az alkalmazás leállása ellen hello másodlagos csomópont, újraindítás hello tootest hello rugalmassági **alárendelt** csomópont. tootest hello rugalmasságot az alkalmazás teljes leállása ellen hello gyorsítótár, indítsa újra a **mindkét** csomópontok.

### <a name="can-i-reboot-hello-cache-tooclear-client-connections"></a>Újraindítás hello gyorsítótár tooclear ügyfélkapcsolatokat is?
Igen, ha újraindítja hello gyorsítótár törlődik minden ügyfél-kommunikációhoz. Rendszer újraindítása lehet hasznos hello esetében, ahol minden ügyfél-kommunikációhoz használt tooa logikai hiba vagy hello ügyfélalkalmazás hibája miatt. Minden tarifacsomag különböző rendelkezik [ügyfél kapcsolati határt](cache-configure.md#default-redis-server-configuration) a hello különböző méretű, és ezek a korlátozások vannak elérése után már semmilyen további ügyfélkapcsolatok elfogadottak. Egy módon tooclear hello gyorsítótár újraindítás biztosít minden ügyfél-kommunikációhoz.

> [!IMPORTANT]
> Ha újraindítja a gyorsítótár tooclear ügyfélkapcsolatokat, StackExchange.Redis automatikusan újracsatlakozik után hello Redis csomópont újra online állapotba kerül. Hello mögöttes probléma nem szűnik meg, ha hello ügyfélkapcsolatok használta toobe továbbra is.
> 
> 

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>I adat elvész a gyorsítótárból, ha a számítógép újraindítása a teendő?
Ha mindkét hello újraindítja **fő** és **alárendelt** csomópontok, a gyorsítótár hello (vagy adott shard használata egy prémium szintű gyorsítótár fürtözési engedélyezve) minden adat elveszik. Ha már konfigurált [adatmegőrzés](cache-how-to-premium-persistence.md), hello legfrissebb biztonsági másolat visszaállnak hello gyorsítótár ismét online elérhető, de bármilyen gyorsítótár írás után hello biztonsági másolat készült előfordult elvesznek.

Ha újraindítja hello csomópontok csak egyike, legyen általában adatvesztés, de továbbra is lehet. Ha például a hello fő csomópont újraindítása után, és a gyorsítótár-írási van folyamatban, hello gyorsítótár-írási hello adatait érték elveszett. Egy másik forgatókönyvet az adatvesztés lenne, ha egy csomópont újraindítása, és egyéb hello csomópont történik toogo le miatt tooa hiba történne hello ugyanannyi időt vesz igénybe. További információ a adatvesztés lehetséges okai: [a Redis toomy történt adatok?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md)

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Újraindíthatja a gyorsítótár, a PowerShell, a parancssori felületen vagy a más felügyeleti eszközökhöz?
Igen, a PowerShell útmutatásért lásd: [tooreboot a Redis gyorsítótár](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-hello-reboot-functionality"></a>Milyen árképzési tiers hello újraindítás funkciót használhatja a?
Újraindítás összes tarifacsomagok érhető el.

## <a name="schedule-updates"></a>Frissítések ütemezése
Hello **frissítések ütemezése** panel lehetővé teszi a Premium szint gyorsítótár karbantartási időszak toodesignate. Hello karbantartási időszak megadása esetén a Redis-kiszolgáló változásainak végrehajtott ezen időszak alatt történjen. 

> [!NOTE] 
> hello karbantartási időszak vonatkozzon, csak tooRedis kiszolgáló, és nem tooany Azure frissíti, vagy toohello hello hello gyorsítótár üzemeltető virtuális gépek operációs rendszerének frissítése.
> 
> 

![Frissítések ütemezése](./media/cache-administration/redis-schedule-updates.png)

toospecify karbantartási időszak, ellenőrizze a szükségeskonfiguráció-hello nap és hello karbantartási időszak kezdő időpontja minden nap adja meg, majd kattintson **OK**. Vegye figyelembe, hogy hello karbantartási ablak időpontja UTC szerint. 

> [!NOTE]
> a frissítések hello alapértelmezett karbantartási idejében öt órát. Ez az érték nincs hello Azure-portálon konfigurálható, de a segítségével konfigurálhatja a PowerShell használatával hello `MaintenanceWindow` hello paramétere [New-AzureRmRedisCacheScheduleEntry](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry) parancsmag. További információkért lásd: [kezelhetők a PowerShell, a parancssori felületen vagy a más felügyeleti eszközökhöz ütemezett frissítés?](#can-i-manage-scheduled-updates-using-powershell-cli-or-other-management-tools)
> 
> 

## <a name="schedule-updates-faq"></a>A frissítések ütemezése – Gyakori kérdések
* [Ha tegye frissítések fordulhat elő, ha hello ütemezés frissítések szolgáltatás nem használom?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Milyen típusú frissítések során hello elvégzett ütemezett karbantartási időszakot?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [A PowerShell, a parancssori felületen vagy a más felügyeleti eszközökhöz ütemezett frissítések lehet kezelni?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
* [Milyen árképzési tiers hello ütemezés frissítések funkciót használhatja a?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-hello-schedule-updates-feature"></a>Ha tegye frissítések fordulhat elő, ha hello ütemezés frissítések szolgáltatás nem használom?
Ha nem adja meg a karbantartási időszak, bármikor frissítéseket is végezhető.

### <a name="what-type-of-updates-are-made-during-hello-scheduled-maintenance-window"></a>Milyen típusú frissítések során hello elvégzett ütemezett karbantartási időszakot?
Csak Redis hello ütemezett karbantartási időszak alatt végrehajtott frissítések kiszolgáló. hello karbantartási időszak nem érvényes tooAzure frissítéseket, vagy toohello virtuális gép operációs rendszer frissítése.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>A PowerShell, a parancssori felületen vagy a más felügyeleti eszközökhöz felügyelt ütemezett frissítések lehetőségeket?
Igen, az ütemezett frissítések hello a következő PowerShell-parancsmagok használatával kezelheti:

* [Get-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/get-azurermrediscachepatchschedule)
* [Új AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/new-azurermrediscachepatchschedule)
* [Új AzureRmRedisCacheScheduleEntry](/powershell/module/azurerm.rediscache/new-azurermrediscachescheduleentry)
* [Remove-AzureRmRedisCachePatchSchedule](/powershell/module/azurerm.rediscache/remove-azurermrediscachepatchschedule)

### <a name="what-pricing-tiers-can-use-hello-schedule-updates-functionality"></a>Milyen árképzési tiers hello ütemezés frissítések funkciót használhatja a?
Hello **frissítések ütemezése** szolgáltatás csak a áll rendelkezésre hello prémium tarifacsomagra.

## <a name="next-steps"></a>Következő lépések
* Fedezze fel több [Azure Redis Cache prémium csomagban](cache-premium-tier-intro.md) szolgáltatásokat.

