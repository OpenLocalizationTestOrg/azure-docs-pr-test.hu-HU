---
title: "aaaAzure Resource health – gyakori kérdések |} Microsoft Docs"
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
ms.openlocfilehash: 5c5cfa116340094ffb1d6d5b206a11d389a0305a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-faq"></a>Az Azure Resource health – gyakori kérdések
Ismerje meg, hello toocommon szolgáltatással kapcsolatos kérdésekre az Azure-erőforrás állapota.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
* [Mi az az Azure Resource Health?](#what-is-azure-resource-health)
* [Mi szól a hello erőforrás állapota?](#what-is-the-resource-health-intended-for)
* [Ellenőrzi, milyen állapotfigyelő végzi az erőforrás állapota?](#what-health-checks-are-performed-by-resource-health)
* [Mit jelent a hello állapotadatainak minden egyes?](#what-does-each-of-the-health-status-mean)
* [Mi nem hello állapota ismeretlen középérték? Valamilyen probléma van vele a erőforrás?](#what-does-the-unknown-status-mean-is-something-wrong-with-my-resource)
* [Hogyan tudjanak segítséget kérni, amely nem érhető el erőforrás?](#how-can-i-get-help-for-a-resource-that-is-unavailable)
* [Erőforrás állapota platform problémák és valami tettem cased elérhetetlensége megkülönböztetni?](#does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did)
* [I integrálható erőforrás állapota a Hálózatfigyelő eszközök?](#can-i-integrate-resource-health-with-my-monitoring-tools)
* [Hol található az erőforrás állapota?](#where-do-i-find-resource-health)
* [Az erőforrás állapota minden erőforrástípus esetén érhető el?](#is-resource-health-available-for-all-resource-types)
* [Mit tegyek, ha az erőforrás elérhető láthatók, de I úgy érzi, nem?](#what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not)
* [Minden Azure-régió érhető el az erőforrás állapota?](#is-resource-health-available-for-all-azure-regions)
* [Miben különbözik erőforrás állapota hello az állapotjelző irányítópulthoz vagy hello Azure portál szolgáltatás értesítést?](#how-is-resource-health-different-from-the-service-health-dashboard-or-the-azure-portal-service-notifications)
* [Szükséges tooactivate erőforrás állapota az egyes erőforrások?](#do-i-need-to-activate-resource-health-for-each-resource)
* [Szükség tooenable erőforrás állapota a szervezetem számára?](#do-we-need-to-enable-resource-health-for-my-organization)
* [Érhető el erőforrás állapota ingyenesen?](#is-resource-health-available-free-of-charge)
* [Mik azok a hello javaslatok erőforrás állapota biztosító?](#what-are-the-recommendations-that-resource-health-provides)


## <a name="what-is-azure-resource-health"></a>Mi az Azure-erőforrás állapota?
A Resource Health segítséget nyújt a diagnosztizálásban és a támogatás igénylésében, ha egy Azure-ral kapcsolatos probléma hatással van az erőforrásaira. Figyelmeztet az erőforrások hello aktuális és korábbi állapotát, és segít a problémák elhárítása érdekében. A Resource Health műszaki támogatást nyújt, ha segítségre van szüksége az Azure szolgáltatásait érintő problémákkal kapcsolatban.  

## <a name="what-is-hello-resource-health-intended-for"></a>Mi szól a hello erőforrás állapota?
Amennyiben az erőforrás problémát észlelt, erőforrás állapota alapján könnyebben felderíthetők hello alapvető ok. Súgó toomitigate hello probléma és biztosít a technikai támogatási szolgálathoz Ha Azure-szolgáltatásokkal kapcsolatos problémákról további segítségre van szüksége.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Ellenőrzi, milyen állapotfigyelő végzi az erőforrás állapota?
Erőforrás állapota alapján hello különböző ellenőrzést hajt végre [erőforrástípus](resource-health-checks-resource-types.md). Az ellenőrzések háromféle tervezett tooimplement problémák: 
1. Nem tervezett események, például egy nem várt állomás újraindítás
2. Tervezett események, például a gazda operációs rendszer számára ütemezett frissítések
3. Akkor kiváltott események végrehajtott műveletek, például egy felhasználó egy virtuális gép újraindítása

## <a name="what-does-each-of-hello-health-status-mean"></a>Mit jelent a hello állapotadatainak minden egyes?
Három különböző egészségügyi állapot van:
1. Érhető el: Nincs olyan ismert probléma az Azure platformon, amely ehhez az erőforráshoz érintő sikerült hello
2. Nem érhető el: Erőforrás állapota észlelt problémákat, amelyek hatással vannak a hello erőforrás
3. Ismeretlen: Erőforrás állapota nem sikerült megállapítani hello állapotát egy erőforrást, mert az információk fogadása leállt. 

## <a name="what-does-hello-unknown-status-mean-is-something-wrong-with-my-resource"></a>Mi nem hello állapota ismeretlen középérték? Valamilyen probléma van vele a erőforrás?
hello állapot toounknown van beállítva, amikor az erőforrás állapota nem fogadja az adott erőforrásra vonatkozó adatokat. Ez az állapot nem végleges feltüntetése hello állapotának hello erőforrás azokban az esetekben, ahol olyan problémákat tapasztal, azt jelentheti, egy Azure probléma lépett fel.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Hogyan tudjanak segítséget kérni, amely nem érhető el erőforrás?
Hello állapotfigyelő erőforráspanelen támogatási kérelmet elküldheti. Ha hello erőforrás nem érhető el, nem kell a Microsoft tooopen kérelmet támogatási megállapodása mert platform események.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Erőforrás állapota platform problémák és valami tettem cased elérhetetlensége megkülönböztetni?
Igen, ha egy erőforrás nem érhető el, erőforrás állapota azonosítja a hello alapvető ok belül e kategóriák közül: 
1.  Felhasználó által kezdeményezett művelet
2.  Tervezett esemény 
3.  Nem tervezett esemény

Hello portálon felhasználó által kezdeményezett műveletek jelennek meg a kék értesítési ikon használatával tervezett és nem tervezett eseményeket használ, piros figyelmeztető ikon látható. További részletek szerepelnek hello [erőforrás állapotának áttekintése](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>I integrálható erőforrás állapota a Hálózatfigyelő eszközök?
Erőforrás állapota egy szolgáltatás, amely toohelp meg diagnosztizálni és megoldani az Azure szolgáltatásokkal kapcsolatos problémákról, amely az erőforrások hatással. Használhatja hello resource health API tooprogrammatically beszerzése hello állapotát, javasoljuk, hogy az erőforrások metrikák toomonitor használhatja. Ha problémát észlel, az erőforrás állapota segít meghatározni, hello alapvető ok, és végigvezeti Önt a műveletek tooaddress őket. Látogasson el [Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) toolearn használatát metrikák toocheck az erőforrások többet.

## <a name="where-do-i-find-resource-health"></a>Hol található az erőforrás állapota?
Azure-portálon toohello jelentkezzen, többféleképpen is erőforrás állapota végezheti el:
1. Keresse meg a tooyour erőforrás. Kattintson a bal oldali navigációs hello, **erőforrás állapota**.
2. Nyissa meg toohello Azure figyelő panelen.  Kattintson a bal oldali navigációs hello, **erőforrás állapota**.
3. Nyissa meg hello **súgó + támogatás** panelre. Ehhez kattintson a hello kérdőjel hello jobb felső sarkában hello portálon, és jelölje be a **súgó + támogatás**. Amikor hello panel nyílik meg, kattintson a **erőforrás állapota**

Hello erőforrás állapota API tooobtain információ hello állapotát az erőforrások is használható.

## <a name="is-resource-health-available-for-all-resource-types"></a>Az erőforrás állapota minden erőforrástípus esetén érhető el?
hello állapot-ellenőrzési eredményeire és erőforrásállapot támogatott erőforrástípus található [Itt](resource-health-checks-resource-types.md)

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Mit tegyek, ha az erőforrás elérhető láthatók, de I úgy érzi, nem?"
Egy erőforrás hello állapotának ellenőrzésekor hello állapot szerinti kattinthat **helytelen állapot jelentést**. Mielőtt hello jelentés küldése, lehetősége van hello biztosító további részleteket a miért úgy véli hello aktuális állapot érvénytelen.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Minden Azure-régió érhető el az erőforrás állapota? 
A következő régiókban hello kivételével az összes Azure geos erőforrás állapota nem érhető el:
* USA-beli államigazgatás – Virginia
* USA-beli államigazgatás – Iowa
* US DoD – Kelet
* US DoD – Középső régió
* Közép-Németország
* Északkelet-Németország
* Kelet-Kína
* Észak-Kína

## <a name="how-is-resource-health-different-from-hello-service-health-dashboard-or-hello-azure-portal-service-notifications"></a>Miben különbözik erőforrás állapota hello az állapotjelző irányítópulthoz vagy hello Azure portál szolgáltatás értesítést?
erőforrás állapota hello információ pontosabb, mint mi szolgáltatja hello Azure az állapotjelző irányítópulthoz.

Mivel [Azure állapot](https://status.azure.com) és hello portál szolgáltatás értesítések tájékoztatnak, szolgáltatásokkal kapcsolatos problémákról, amelyek hatással vannak a felhasználók (például egy Azure-régió) széles körét, erőforrás állapota csak kapcsolódik részletesebb események közzététele. toohello adott erőforrás. Például ha egy gazdagép váratlanul újraindul, a resource health csak ezek az ügyfelek amelynek virtuális gépek az adott gazdagépen futó riasztást küld.

Fontos toonotice, hogy látható-e az erőforrások érintő események befejezése tooprovide, erőforrás állapota is felfed szolgáltatás értesítések és az állapotjelző irányítópulthoz hello közzé eseményeket.

## <a name="do-i-need-tooactivate-resource-health-for-each-resource"></a>Szükséges tooactivate erőforrás állapota az egyes erőforrások?
Nem, állapottal kapcsolatos adatok érhető el az összes erőforrástípus erőforrás állapota keresztül érhető el. 

## <a name="do-we-need-tooenable-resource-health-for-my-organization"></a>Szükség tooenable erőforrás állapota a szervezetem számára?
Nem.  Azure-erőforrás állapotának belül hello Azure-portálhoz a telepítési követelmények nélkül érhető el.

## <a name="is-resource-health-available-free-of-charge"></a>Érhető el erőforrás állapota ingyenesen?
Igen.  Az Azure-erőforrás állapota az ingyenesen elérhető.

## <a name="what-are-hello-recommendations-that-resource-health-provides"></a>Mik azok a hello javaslatok erőforrás állapota biztosító?
Hello állapot alapján, erőforrás állapota nyújt hibaelhárítási felhasznált hello célja hello idő csökkentése a javaslatokat. A rendelkezésre álló erőforrások hello javaslatok arra utalnak, hogy toosolve hello leggyakoribb problémák szembesülnek. Ha hello erőforrás nem érhető el miatt tooan Azure váratlan esemény, hello előtérbe kerül segít alatt és után hello helyreállítási folyamatot. 

## <a name="next-steps"></a>Következő lépések

Tekintse meg e erőforrások toolearn további információ az erőforrás állapota:
-  [Azure-erőforrás állapotának áttekintése](Resource-health-overview.md)
-  [Az Azure Resource Health segítségével elérhető erőforrástípusok és állapotellenőrzések](resource-health-checks-resource-types.md)
