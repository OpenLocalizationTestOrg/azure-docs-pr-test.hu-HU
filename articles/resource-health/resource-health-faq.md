---
title: "Az Azure Resource health – gyakori kérdések |} Microsoft Docs"
description: "Azure-erőforrás állapotának áttekintése"
services: Resource health
documentationcenter: dev-center-name
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
ms.openlocfilehash: 41522a85cac05304b3ae60c45b48920eefbe8f5c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-health-faq"></a>Az Azure Resource health – gyakori kérdések
További tudnivalók az Azure-erőforrás állapotának kapcsolatos gyakori kérdésekre adott válaszok.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
* [Mi az az Azure Resource Health?](#what-is-azure-resource-health)
* [Mi szól a az erőforrás állapota?](#what-is-the-resource-health-intended-for)
* [Ellenőrzi, milyen állapotfigyelő végzi az erőforrás állapota?](#what-health-checks-are-performed-by-resource-health)
* [Mit jelent egyes állapotát?](#what-does-each-of-the-health-status-mean)
* [Mit jelent az ismeretlen állapot? Valamilyen probléma van vele a erőforrás?](#what-does-the-unknown-status-mean-is-something-wrong-with-my-resource)
* [Hogyan tudjanak segítséget kérni, amely nem érhető el erőforrás?](#how-can-i-get-help-for-a-resource-that-is-unavailable)
* [Erőforrás állapota platform problémák és valami tettem cased elérhetetlensége megkülönböztetni?](#does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did)
* [I integrálható erőforrás állapota a Hálózatfigyelő eszközök?](#can-i-integrate-resource-health-with-my-monitoring-tools)
* [Hol található az erőforrás állapota?](#where-do-i-find-resource-health)
* [Az erőforrás állapota minden erőforrástípus esetén érhető el?](#is-resource-health-available-for-all-resource-types)
* [Mit tegyek, ha az erőforrás elérhető láthatók, de I úgy érzi, nem?](#what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not)
* [Minden Azure-régió érhető el az erőforrás állapota?](#is-resource-health-available-for-all-azure-regions)
* [Miben különbözik erőforrás állapota az állapotjelző irányítópulton vagy az Azure portál szolgáltatás értesítéseket?](#how-is-resource-health-different-from-the-service-health-dashboard-or-the-azure-portal-service-notifications)
* [Kell aktiválni az egyes erőforrások erőforrás állapota?](#do-i-need-to-activate-resource-health-for-each-resource)
* [Szeretnénk engedélyezése erőforrás állapota a szervezetem számára?](#do-we-need-to-enable-resource-health-for-my-organization)
* [Érhető el erőforrás állapota ingyenesen?](#is-resource-health-available-free-of-charge)
* [Mik a javaslatok, amely erőforrás állapota?](#what-are-the-recommendations-that-resource-health-provides)


## <a name="what-is-azure-resource-health"></a>Mi az Azure-erőforrás állapota?
A Resource Health segítséget nyújt a diagnosztizálásban és a támogatás igénylésében, ha egy Azure-ral kapcsolatos probléma hatással van az erőforrásaira. Tájékoztatja az erőforrásai aktuális és korábbi állapotáról, és segít a problémák kezelésében. A Resource Health műszaki támogatást nyújt, ha segítségre van szüksége az Azure szolgáltatásait érintő problémákkal kapcsolatban.  

## <a name="what-is-the-resource-health-intended-for"></a>Mi szól a az erőforrás állapota?
Amennyiben az erőforrás problémát észlelt, erőforrás állapota segítségével diagnosztizálhatja az alapvető okát. Biztosít segítenek mérsékelni a problémát, és a technikai támogatási szolgálathoz, ha az Azure-szolgáltatásokkal kapcsolatos problémákról további segítségre van szüksége.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Ellenőrzi, milyen állapotfigyelő végzi az erőforrás állapota?
Erőforrás állapota alapján különböző ellenőrzést hajt végre a [erőforrástípus](resource-health-checks-resource-types.md). Az ellenőrzések tervezett három típusú problémákat végrehajtásához: 
1. Nem tervezett események, például egy nem várt állomás újraindítás
2. Tervezett események, például a gazda operációs rendszer számára ütemezett frissítések
3. Akkor kiváltott események végrehajtott műveletek, például egy felhasználó egy virtuális gép újraindítása

## <a name="what-does-each-of-the-health-status-mean"></a>Mit jelent egyes állapotát?
Három különböző egészségügyi állapot van:
1. Rendelkezésre álló: Nincs olyan ismert probléma az Azure platform, amely tudta mely negatív hatással lehet az erőforrás
2. Nem érhető el: Erőforrás állapota azt észlelte, hogy az erőforrás van érintő problémák
3. Ismeretlen: Erőforrás állapota nem sikerült megállapítani az erőforrás állapotát, mert az információk fogadása leállt. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>Mit jelent az ismeretlen állapot? Valamilyen probléma van vele a erőforrás?
Ha az erőforrás állapota nem fogadja az adott erőforrásra vonatkozó adatokat állapota ismeretlen értéke. Ez az állapot nem végleges feltüntetése azokban az esetekben, ahol tapasztal, az erőforrás állapota azt jelentheti, egy Azure probléma lépett fel.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Hogyan tudjanak segítséget kérni, amely nem érhető el erőforrás?
A health erőforráspanelen támogatási kérelmet elküldheti. Nem kell kérést nyithat, ha az erőforrás nem érhető el a Microsoft támogatási megállapodás mert platform események.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Erőforrás állapota platform problémák és valami tettem cased elérhetetlensége megkülönböztetni?
Igen, ha egy erőforrás nem érhető el, a erőforrás állapota azonosítja az alapvető ok belül e kategóriák közül: 
1.  Felhasználó által kezdeményezett művelet
2.  Tervezett esemény 
3.  Nem tervezett esemény

A portálon felhasználó által kezdeményezett műveletek jelennek meg a kék értesítési ikon használatával tervezett és nem tervezett eseményeket használ, piros figyelmeztető ikon látható. További részletek szerepelnek a [erőforrás állapotának áttekintése](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>I integrálható erőforrás állapota a Hálózatfigyelő eszközök?
Erőforrás állapota egy olyan szolgáltatás, célja, hogy diagnosztizálni és megoldani az Azure szolgáltatásokkal kapcsolatos problémákról, amely hatással lehet az erőforrásokat. Az erőforrás állapota API segítségével programozott módon beszerzése az állapotát, de javasolt mérőszámok segítségével figyelheti az erőforrások. Ha problémát észlel, az erőforrás állapota segít meghatározni, az alapvető ok, és végigvezeti Önt a műveletek hozzájuk. Látogasson el [Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) tudhat meg többet használatát metrikák az erőforrások kereséséhez.

## <a name="where-do-i-find-resource-health"></a>Hol található az erőforrás állapota?
Az Azure-portálon való bejelentkezést követően többféleképpen is erőforrás állapota végezheti el:
1. Nyissa meg az erőforrás. Kattintson a bal oldali navigációs **erőforrás állapota**.
2. Nyissa meg az Azure-figyelő paneljét.  Kattintson a bal oldali navigációs **erőforrás állapota**.
3. Nyissa meg a **súgó + támogatás** panelen kattintson a portál jobb felső sarkában található, majd jelölje be **súgó + támogatás**. Amikor megnyílik a panel, kattintson a **erőforrás állapota**

Az erőforrás állapota API segítségével is beolvasni az erőforrások állapotával kapcsolatos információkat.

## <a name="is-resource-health-available-for-all-resource-types"></a>Az erőforrás állapota minden erőforrástípus esetén érhető el?
Állapot-ellenőrzési eredményeire és erőforrásállapot támogatott erőforrástípus található [Itt](resource-health-checks-resource-types.md)

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Mit tegyek, ha az erőforrás elérhető láthatók, de I úgy érzi, nem?"
Egy erőforrás állapotának ellenőrzésekor a állapot szerinti kattinthat **helytelen állapot jelentést**. Mielőtt elküldi a jelentést, lehetősége van a miért úgy gondolja, a jelenlegi állapot érvénytelen. további részleteket biztosít.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Minden Azure-régió érhető el az erőforrás állapota? 
Erőforrás állapota érhető el az összes Azure geos, kivéve az alábbi területek között:
* USA-beli államigazgatás – Virginia
* USA-beli államigazgatás – Iowa
* US DoD – Kelet
* US DoD – Középső régió
* Közép-Németország
* Északkelet-Németország
* Kelet-Kína
* Észak-Kína

## <a name="how-is-resource-health-different-from-the-service-health-dashboard-or-the-azure-portal-service-notifications"></a>Miben különbözik erőforrás állapota az állapotjelző irányítópulton vagy az Azure portál szolgáltatás értesítéseket?
Az erőforrás állapota által biztosított információk pontosabb, mint mi biztosítja az Azure az állapotjelző irányítópulthoz.

Mivel [Azure állapot](https://status.azure.com) és a portál szolgáltatás értesítések tájékoztatnak, szolgáltatásokkal kapcsolatos problémákról, amelyek hatással vannak a felhasználók (például egy Azure-régió) széles körét, erőforrás állapota csak az adott erőforrás szempontjából részletesebb események közzététele. Például ha egy gazdagép váratlanul újraindul, a resource health csak ezek az ügyfelek amelynek virtuális gépek az adott gazdagépen futó riasztást küld.

Fontos, hogy figyelje meg, hogy biztosítja, hogy az erőforrások érintő események teljes láthatóságát, erőforrás állapota is felfed szolgáltatás értesítések és az állapotjelző irányítópulton a közzétett események.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>Kell aktiválni az egyes erőforrások erőforrás állapota?
Nem, állapottal kapcsolatos adatok érhető el az összes erőforrástípus erőforrás állapota keresztül érhető el. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>Szeretnénk engedélyezése erőforrás állapota a szervezetem számára?
Nem.  Az Azure-erőforrás állapota az Azure portálon a telepítési követelmények nélkül érhető el.

## <a name="is-resource-health-available-free-of-charge"></a>Érhető el erőforrás állapota ingyenesen?
Igen.  Az Azure-erőforrás állapota az ingyenesen elérhető.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>Mik a javaslatok, amely erőforrás állapota?
Az állapot alapján, erőforrás állapota nyújt javaslatokat azzal a céllal, idejének csökkentése hibaelhárítási felhasznált. A rendelkezésre álló erőforrások a javaslatok fókusz a leggyakoribb problémák ügyfelek megoldására tapasztal. Ha az erőforrás nem érhető el az Azure nem tervezett esemény miatt, a fókusz segít alatt és a helyreállítási folyamat után lesz. 

## <a name="next-steps"></a>Következő lépések

Tekintse meg ezeket az erőforrásokat erőforrás állapota tájékozódhat:
-  [Azure-erőforrás állapotának áttekintése](Resource-health-overview.md)
-  [Az Azure Resource Health segítségével elérhető erőforrástípusok és állapotellenőrzések](resource-health-checks-resource-types.md)
