---
title: "aaaAzure Resource Health – gyakori kérdések |} Microsoft Docs"
description: "Azure-erőforrás állapotának áttekintése"
services: Resource health
documentationcenter: dev-center-name
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/05/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: fe29b2df1f9fee4b77d0100c7d872df837dc4b4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-faq"></a>Az Azure Resource Health – gyakori kérdések
Ismerje meg, hello toocommon szolgáltatással kapcsolatos kérdésekre Azure-erőforrás állapota.

## <a name="what-is-azure-resource-health"></a>Mi az Azure-erőforrás állapota?
A Resource Health segítséget nyújt a diagnosztizálásban és a támogatás igénylésében, ha egy Azure-ral kapcsolatos probléma hatással van az erőforrásaira. Figyelmeztet az erőforrások hello aktuális és korábbi állapotát, és segít a problémák elhárítása érdekében. A Resource Health műszaki támogatást nyújt, ha segítségre van szüksége az Azure szolgáltatásait érintő problémákkal kapcsolatban.  

## <a name="what-is-hello-resource-health-intended-for"></a>Mi az erőforrás állapota szánt hello?
Amennyiben az erőforrás problémát észlelt, erőforrás állapota alapján könnyebben felderíthetők hello alapvető ok. Súgó toomitigate hello probléma és biztosít a technikai támogatási szolgálathoz Ha Azure-szolgáltatásokkal kapcsolatos problémákról további segítségre van szüksége.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Ellenőrzi, milyen állapotfigyelő végzi az erőforrás állapota?
Erőforrás állapota alapján hello különböző ellenőrzést hajt végre [erőforrástípus](resource-health-checks-resource-types.md). Az ellenőrzések háromféle tervezett tooimplement problémák: 
- Nem tervezett események, például egy nem várt állomás újraindítás
- Tervezett események, például a gazda operációs rendszer számára ütemezett frissítések
- Akkor kiváltott események végrehajtott műveletek, például egy felhasználó egy virtuális gép újraindítása

## <a name="what-does-each-of-hello-health-status-mean"></a>Mit jelent a hello állapotadatainak minden egyes?
Három különböző egészségügyi állapot van:
- Érhető el: Nincs olyan ismert probléma az Azure platformon, amely ehhez az erőforráshoz érintő sikerült hello
- Nem érhető el: Erőforrás állapota észlelt problémákat, amelyek hatással vannak a hello erőforrás
- Ismeretlen: Erőforrás állapota nem sikerült megállapítani hello állapotát egy erőforrást, mert az információk fogadása leállt. 

## <a name="what-does-hello-unknown-status-mean-is-something-wrong-with-my-resource"></a>Mi nem hello állapota ismeretlen középérték? Valamilyen probléma van vele a erőforrás?
hello állapot toounknown van beállítva, amikor az erőforrás állapota nem fogadja az adott erőforrásra vonatkozó adatokat. Ez az állapot nem végleges feltüntetése hello állapotának hello erőforrás azokban az esetekben, ahol olyan problémákat tapasztal, azt jelentheti, egy Azure probléma lépett fel.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Hogyan tudjanak segítséget kérni, amely nem érhető el erőforrás?
Hello Resource Health paneljéről támogatási kérelmet elküldheti. Ha hello erőforrás nem érhető el, nem kell a Microsoft tooopen kérelmet támogatási megállapodása mert platform események.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Erőforrás állapota platform problémák és valami tettem cased elérhetetlensége megkülönböztetni?
Igen, ha egy erőforrás nem érhető el, erőforrás állapota azonosítja a hello alapvető ok belül e kategóriák közül: 
-   Felhasználó által kezdeményezett művelet
-   Tervezett esemény 
-   Nem tervezett esemény

Hello portálon felhasználó által kezdeményezett műveletek jelennek meg a kék értesítési ikon használatával tervezett és nem tervezett eseményeket használ, piros figyelmeztető ikon látható. További részletek szerepelnek hello [erőforrás állapota – áttekintés](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>I integrálható erőforrás állapota a Hálózatfigyelő eszközök?
Erőforrás állapota egy szolgáltatás, amely toohelp meg diagnosztizálni és megoldani az Azure szolgáltatásokkal kapcsolatos problémákról, amely az erőforrások hatással. Használhatja hello Resource Health API tooprogrammatically beszerzése hello állapotát, javasoljuk, hogy az erőforrások metrikák toomonitor használhatja. Ha problémát észlel, Resource Health segít meghatározni, hello okozza, és végigvezeti a műveletek tooaddress őket. Látogasson el [Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) toolearn használatát metrikák toocheck az erőforrások többet.

## <a name="where-do-i-find-resource-health"></a>Hol található az erőforrás állapota?
Azure-portálon toohello jelentkezzen, többféleképpen is Resource Health végezheti el:
- Keresse meg a tooyour erőforrás. A bal oldali navigációs hello, válassza ki **erőforrás állapota**
- Nyissa meg toohello Azure figyelő panelen.  A bal oldali navigációs hello, válassza ki **erőforrás állapota**.
- Nyissa meg hello **súgó + támogatás** hello kérdőjel kiválasztásával hello jobb felső sarkában hello portálon, és jelölje be a panelt **súgó + támogatás**. Miután hello panel nyílik meg, válassza ki a **erőforrás állapota**

Hello Resource Health API tooobtain információ hello állapotát az erőforrások is használhatja.

## <a name="is-resource-health-available-for-all-resource-types"></a>Az erőforrás állapota minden erőforrástípus esetén érhető el?
hello állapot-ellenőrzési eredményeire és a Resource Health támogatott típusú erőforrások listája található [Itt](resource-health-checks-resource-types.md).

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Mit tegyek, ha az erőforrás elérhető láthatók, de I úgy érzi, nem?"
Egy erőforrás hello állapotának ellenőrzésekor hello állapot szerinti kattinthat **helytelen állapot jelentést**. Mielőtt hello jelentés küldése, lehetősége van hello biztosító további részleteket a miért úgy véli hello aktuális állapot érvénytelen.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Érhető el erőforrás állapota minden Azure-régió? 
A következő régiókban hello kivételével az összes Azure geos erőforrás állapota nem érhető el:
- USA-beli államigazgatás – Virginia
- USA-beli államigazgatás – Iowa
- US DoD – Kelet
- US DoD – Középső régió
- Közép-Németország
- Északkelet-Németország
- Kelet-Kína
- Észak-Kína

## <a name="how-is-resource-health-different-from-hello-service-health-dashboard-or-hello-azure-portal-service-notifications"></a>Miben különbözik Resource Health hello az állapotjelző irányítópulthoz vagy hello Azure portál szolgáltatás értesítést?
Erőforrás állapota hello információ pontosabb, mint mi szolgáltatja hello Azure az állapotjelző irányítópulthoz.

Mivel [Azure állapot](https://status.azure.com) és hello portál szolgáltatás értesítések tájékoztatnak, szolgáltatásokkal kapcsolatos problémákról, amelyek hatással vannak a felhasználók (például egy Azure-régió) széles körét, erőforrás állapota csak kapcsolódik részletesebb események közzététele. toohello adott erőforrás. Például ha egy gazdagép váratlanul újraindul, a Resource Health csak ezek az ügyfelek amelynek virtuális gépek az adott gazdagépen futó riasztást küld.

Ez fontos toonotice, hogy látható-e az erőforrások, felületek események közzétételre szolgáltatáshoz értesítést az erőforrás állapota érintő események befejezése tooprovide és hello az állapotjelző irányítópulthoz.

## <a name="do-i-need-tooactivate-resource-health-for-each-resource"></a>Szükséges tooactivate erőforrás állapota az egyes erőforrások?
Nem, állapottal kapcsolatos adatok érhető el az összes erőforrástípus Resource Health keresztül érhető el. 

## <a name="do-we-need-tooenable-resource-health-for-my-organization"></a>Szükség tooenable erőforrás állapota a szervezetem számára?
Nem.  Az Azure Resource Health belül hello Azure-portálhoz a telepítési követelmények nélkül érhető el.

## <a name="is-resource-health-available-free-of-charge"></a>Érhető el erőforrás állapota ingyenesen?
Igen.  Az Azure Resource Health ingyenesen elérhető.

## <a name="what-are-hello-recommendations-that-resource-health-provides"></a>Mik azok a Resource Health biztosító hello javaslatok?
Hello állapot alapján, erőforrás állapota nyújt hibaelhárítási felhasznált hello célja hello idő csökkentése a javaslatokat. A rendelkezésre álló erőforrások hello javaslatok arra utalnak, hogy toosolve hello leggyakoribb problémák szembesülnek. Ha hello erőforrás nem érhető el miatt tooan Azure váratlan esemény, hello előtérbe kerül segít alatt és után hello helyreállítási folyamatot. 

## <a name="next-steps"></a>Következő lépések

További tudnivalók az erőforrás állapota:
-  [Az Azure Resource Health áttekintése](Resource-health-overview.md)
-  [Az Azure Resource Health segítségével elérhető erőforrástípusok és állapot-ellenőrzések](resource-health-checks-resource-types.md)
