---
title: "aaaAzure erőforrás állapotának áttekintése |} Microsoft Docs"
description: "Azure-erőforrás állapotának áttekintése"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: resource-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 06/01/2016
ms.author: BernardoAMunoz
ms.openlocfilehash: b920241b2f64a0695ba2c32e9ccb0c2c3739986d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-overview"></a>Azure-erőforrás állapotának áttekintése
 
A Resource Health segítséget nyújt a diagnosztizálásban és a támogatás igénylésében, ha egy Azure-ral kapcsolatos probléma hatással van az erőforrásaira. Figyelmeztet az erőforrások hello aktuális és korábbi állapotát, és segít a problémák elhárítása érdekében. A Resource Health műszaki támogatást nyújt, ha segítségre van szüksége az Azure szolgáltatásait érintő problémákkal kapcsolatban.

Mivel [Azure állapot](https://status.azure.com) , amelyek hatással vannak az Azure-ügyfél széleskörű szolgáltatásokkal kapcsolatos problémákról nyújt tájékoztatást, erőforrás állapota tesz lehetővé az erőforrások hello rendszerállapot személyre szabott irányítópultot. Erőforrás állapota elsajátíthatja, hogy az erőforrások nem érhetők el a hello volt, mindig hello fizetési határideje tooAzure szolgáltatásokkal kapcsolatos problémákról. Így az Ön toounderstand egyszerű Ha szolgáltatásiszint-szerződésben garantált megsértettek. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Egy erőforrás szempontjából, és hogyan működik az erőforrás állapota úgy dönt, hogy erőforrás kifogástalan-e vagy sem?
Egy erőforrást egy erőforrástípusra, például az Azure-szolgáltatások Azure Resource Manageren keresztül, által kínált példánya: egy virtuális gépet, a webes alkalmazás vagy egy SQL-adatbázis.

Erőforrás állapota hello különböző Azure-szolgáltatások tooassess által kibocsátott, ha egy erőforrás nem működik megfelelően, vagy nincs jelek támaszkodik. Erőforrás állapota nem kifogástalan, ha az erőforrás állapota elemzi további információt toodetermine hello hello probléma forrása. Meghatározza azt is, műveletek Microsoft tart toofix hello probléma vagy mi tooaddress hello elvégezhető műveletek hello problémát okozhat. 

Felülvizsgálati hello teljes listáját erőforrástípusok és állapotát ellenőrzi [Azure-erőforrás állapotának](resource-health-checks-resource-types.md) további részleteket a health értékelési módját.

## <a name="health-status-provided-by-resource-health"></a>Erőforrás állapota által megadott állapotadatai
egy erőforrás hello állapotát a következő állapotok hello egyike:

### <a name="available"></a>Elérhető
hello szolgáltatás nem talált hello erőforrás állapotának hello érintő eseményeket. Azokban az esetekben, ahol hello erőforrás helyreállt nem tervezett leállás során hello utolsó 24 órában látni fogja hello **mostanában helyre** értesítést.

![Erőforrás állapotának rendelkezésre állású virtuális gép](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Nem érhető el
hello szolgáltatás azt észlelte, egy platform folyamatban lévő vagy a nem platform eseményt érintő hello erőforrás hello állapotát.

#### <a name="platform-events"></a>Platform-események
Ezeket az eseményeket váltja ki több hello Azure-infrastruktúra összetevői, és mindkét ütemezett műveletek, például a tervezett karbantartások és nem várt események például egy nem tervezett állomás újraindítás.

Erőforrás állapota hello esemény, hello helyreállítási folyamat további részleteit és lehetővé teszi toocontact támogatási még akkor is, ha nincs egy aktív Microsoft támogatja a szerződést.

![Erőforrás állapota nem érhető el virtuális gép tooplatform esemény miatt](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Nem-Platform események
Ezeket az eseményeket váltja ki a felhasználók, például egy virtuális gép leállítása vagy hello maximális száma kapcsolatok tooa Redis Cache elérése által végrehajtott műveleteket.

![Erőforrás állapota nem érhető el virtuális gép toonon platformesemény miatt](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Ismeretlen
Állapotfigyelő azt jelzi, hogy erőforrás állapota nem kapta meg az ehhez az erőforráshoz információ a több mint 10 percig. Bár ez az állapot nem végleges feltüntetése hello erőforrás hello állapotát, egy fontos hibaelhárítási folyamatának hello adatpont:
* Ha hello erőforrás hello erőforrás állapotának várt hello futtató tooAvailable frissíti néhány perc múlva.
* Ha hello erőforrás problémákat tapasztal, hello ismeretlen állapot javasolhat hello erőforrás kihatással van a hello platform esemény történt.

![Erőforrás állapota ismeretlen virtuális gép](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a>A nem megfelelő állapotú jelentés
Ha bármikor úgy véli hello aktuális állapot nem megfelelő, akkor is ossza meg velünk kattintva **helytelen állapot jelentést**. Azokban az esetekben, ahol van egy Azure probléma által érintett javasoljuk, toocontact támogatási hello erőforráspanelen állapotát. 

![Erőforrás állapota nem megfelelő állapot jelentése](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Előzményadatok
Van-e hozzáférési too14 nappal korábbi egészségügyi adatok mentése kattintva **előzményeinek megtekintése** hello Resource health panelen. 

![A jelentés előzményei erőforrás állapota](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Bevezetés
tooopen egy erőforrás erőforrás állapota
1.  Jelentkezzen be a hello Azure-portálon.
2.  Keresse meg a tooyour erőforrás.
3.  A hello erőforrás hello bal oldalon található kattintson **erőforrás állapota**.

![Nyissa meg az erőforrás állapota erőforráspanelen](./media/resource-health-overview/from-resource-blade.png)

Erőforrás állapota kattintva is elérheti **további szolgáltatások**, és írja be **erőforrás állapota** a szűrő szöveg mezőben tooopen hello **súgó + támogatás** panelen. Végül [ **erőforrás állapota**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Nyissa meg a további szolgáltatás erőforrás állapota](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Következő lépések

Tekintse meg e erőforrások toolearn további információ az erőforrás állapota:
-  [Erőforrástípusok és állapotát ellenőrzi az Azure-erőforrás állapota](resource-health-checks-resource-types.md)
-  [Azure-erőforrás állapotának kapcsolatos gyakori kérdések](resource-health-faq.md)




