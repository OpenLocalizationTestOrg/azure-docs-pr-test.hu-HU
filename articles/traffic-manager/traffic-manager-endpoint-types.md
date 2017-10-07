---
title: "aaaTraffic Manager Végponttípusok |} Microsoft Docs"
description: "Ez a cikk ismerteti a különböző használható az Azure Traffic Manager-végpontok"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 4e506986-f78d-41d1-becf-56c8516e4d21
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: kumud
ms.openlocfilehash: 787412ac6207f76791bf3ff753d1df2767b1a964
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-endpoints"></a>Traffic Manager-végpontok
Microsoft Azure Traffic Manager lehetővé teszi hálózati forgalom Mitől toocontrol elosztott tooapplication központi telepítések futtató különböző adatközpontokban. Minden alkalmazás központi telepítésének egy végpontként a Traffic Manager konfigurálása. Traffic Manager DNS-kérelmet kap, ha azt választja ki egy elérhető végpontot tooreturn hello DNS-választ. A TRAFFIC manager hello választott hello aktuális végpont állapota és a forgalom-útválasztási módszer hello alapjait. További információkért lásd: [Traffic Manager működése](traffic-manager-how-traffic-manager-works.md).

Traffic Manager által támogatott végpont három típusa van:
* **Azure-végpontok** Azure-ban üzemeltetett szolgáltatásokhoz tartoznak.
* **Külső végpontok száma** a szolgáltatások üzemeltetett külső Azure, a helyi vagy egy másik szolgáltató használata.
* **A beágyazott végpontok** használt toocombine Traffic Manager-profilok toocreate rugalmasabb forgalom-útválasztási sémák toosupport hello igényeinek nagyobb, összetettebb központi telepítéseket is.

Hogyan van összevonva különböző típusú végpontok egy Traffic Manager-profil nincs korlátozva van. Az egyes profilok típusú végpontok esetében bármilyen kombinációját is tartalmazhat.

a következő szakaszok hello minden alaposabb típusú végpont ismertetik.

## <a name="azure-endpoints"></a>Azure-végpontok

Azure-végpontok Azure-alapú szolgáltatásokhoz a Traffic Manager tartoznak. a következő Azure erőforrástípusok hello támogatottak:

* A felhőalapú szolgáltatások "Klasszikus" infrastruktúra-szolgáltatási virtuális gépeknél és PaaS.
* Web Apps
* Nyilvános erőforrások (amely lehet csatlakoztatott tooVMs közvetlenül, vagy egy Azure Load Balancer). hello publicIpAddress rendelkeznie kell egy DNS-nevet a Traffic Manager-profil használt toobe rendelve.

Nyilvános erőforrásokhoz olyan erőforrások, Azure Resource Manager. Hello klasszikus üzembe helyezési modellel azok nem léteznek. Így azok csak a támogatott Traffic Manager Azure Resource Manager lép. hello más típusú végpontok esetében támogatottak Resource Manager és a hello klasszikus üzembe helyezési modellel keresztül.

Azure-végpontok használata esetén a Traffic Manager észleli, ha a "Klasszikus" IaaS virtuális gépek, a felhőalapú szolgáltatás vagy a webes alkalmazás leáll, majd elindítani. Ez az állapot hello végpont állapota is megjelenik. Lásd: [Traffic Manager-végpont figyelési](traffic-manager-monitoring.md#endpoint-and-profile-status) részleteiről. Hello az alapul szolgáló szolgáltatás leállítását követően a Traffic Manager nem végez végpont állapotellenőrzést vagy közvetlen forgalom toohello végpontot. Nincs a Traffic Manager számlázási események fordulnak elő a hello leállította a példányt. Amikor hello szolgáltatás újraindítása, folytatása számlázási és hello végpont jogosult tooreceive forgalmat. Az észlelés nem érvényes tooPublicIpAddress végpontok.

## <a name="external-endpoints"></a>Külső végpontok száma

Külső végpontok száma a Azure-on kívüli szolgáltatáshoz használt. Például egy szolgáltatást a helyben tárolt vagy másik szolgáltatóhoz. Külső végpontok száma használt külön-külön vagy együtt az Azure-végpontok a hello ugyanazt a Traffic Manager-profilt. Külső végpontok száma az Azure-végpontok kombinálásával lehetővé teszi, hogy a különböző forgatókönyvekben:

* Vagy az aktív-aktív vagy aktív-passzív feladatátvevő modell használjon nagyobb Azure tooprovide redundancia egy meglévő helyszíni alkalmazást.
* tooreduce alkalmazás várakozási ideje a felhasználók hello világ, egy meglévő helyszíni alkalmazás tooadditional földrajzi helyeken az Azure-ban kiterjesztése. További információkért lásd: ["Teljesítmény" Traffic Manager forgalom-útválasztás](traffic-manager-routing-methods.md#performance).
* Folyamatosan vagy egy "kapacitásnövelés felhő" megoldás toomeet egy csúcs az igények, használja a Azure tooprovide további kapacitást meglévő helyszíni alkalmazás esetén.

Bizonyos esetekben hasznos toouse külső végpontok száma tooreference Azure szolgáltatások (tekintse meg a hello [gyakran ismételt kérdések](traffic-manager-faqs.md#traffic-manager-endpoints)). Ebben az esetben állapotellenőrzést amelyekért hello Azure-végpontok arány nem hello külső végpontok száma sebessége. Azure-végpontok, eltérően leállítása vagy törlése az alapul szolgáló szolgáltatás, hello állapotfigyelő vizsgálja, számlázási továbbra is fennáll, addig, amíg letiltása, vagy törölni a Traffic Manager-hello végpontot.

## <a name="nested-endpoints"></a>Beágyazott végpontok

Beágyazott végpontok több Traffic Manager profilok toocreate rugalmas forgalom-útválasztási sémák egyesítése, és támogatja a nagyobb, összetett központi telepítések hello igényeinek. Beágyazott végpontokon "child" profil meg van adva egy végpont tooa "parent" profil. Mindkét hello gyermek és szülő profil tartalmazhat bármilyen típusú, beleértve a más beágyazott profilok végpontja. További információkért lásd: [Traffic Manager-profilok beágyazott](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps végpontként

További vegye figyelembe a következőket: a Traffic Manager végpontként webalkalmazások konfigurálásakor:

1. Csak Web Apps hello "Standard" SKU vagy annál újabb jogosultak a Traffic Managerrel történő használathoz. Próbálja meg tooadd egy webalkalmazást az alsó SKU-sikertelen. Alacsonyabb verziójúra változtatása hello egy már meglévő webalkalmazás Termékváltozata eredmények a Traffic Manager forgalom toothat webalkalmazás már nem küld.
2. Amikor a végpont egy HTTP-kérelem érkezik, az hello "fogadó" fejléc a következő hello kérelem toodetermine mely Web App service hello igényeljen. hello állomásfejléc hello DNS használt név tooinitiate hello kérelmet, például "contosoapp.azurewebsites.net" tartalmaz. egy másik DNS-nevet a webalkalmazással, hello DNS-név toouse hello alkalmazást az egyéni tartomány nevét regisztrálva kell lennie. A webes alkalmazás végpont Azure végpontjaként felvételekor hello Traffic Manager-profil DNS-név a rendszer automatikusan regisztrálja az hello App. Ez a regisztráció automatikusan törlődik, ha hello végpontja törölve.
3. Minden egyes Traffic Manager-profil legfeljebb egy webalkalmazás-végpontot rendelkezhet minden Azure-régióban. az ennél a határértéknél körül toowork, beállíthat egy webalkalmazás külső végpont. További információkért lásd: hello [gyakran ismételt kérdések](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Engedélyezése és letiltása a végpontok száma

A Traffic Manager végpont letiltása lehet, hogy a karbantartási módban, vagy újratelepítés alatt álló végpontok hasznos tootemporarily remove-forgalmat. Ha hello végpont újra fut, újra engedélyezni lehet.

Végpontok engedélyezve van, és tiltja, hogy hello Traffic Manager portal, PowerShell, a parancssori felületen vagy a REST API-t olyan erőforrás-kezelő és a hello klasszikus üzembe helyezési modellel is támogatottak.

> [!NOTE]
> Nem rendelkezik Azure végpont letiltása toodo rendszerbeli üzembe helyezési állapotát az Azure-ban. Az Azure-szolgáltatás (például egy virtuális gép vagy a webes alkalmazás továbbra is fut, és képes tooreceive forgalom akkor is, ha le van tiltva a Traffic Manager. Forgalom közvetlen toohello szolgáltatás példány helyett keresztül hello Traffic Manager DNS-név profile lehet megoldani. További információkért lásd: [Traffic Manager működése](traffic-manager-how-traffic-manager-works.md).

hello aktuális való jogosultságát, mindegyik végpont tooreceive forgalom hello a következő tényezőktől függ:

* hello-profil állapota (engedélyezett vagy letiltott)
* hello végpont állapota (engedélyezett vagy letiltott)
* hello állapotellenőrzést használó hello eredményei

További információkért lásd: [Traffic Manager-végpont figyelési](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> A Traffic Manager DNS szint hello működik, mert már nem tooinfluence meglévő kapcsolatok tooany végpont. A végpont nem érhető el, ha a Traffic Manager irányítja új kapcsolatok tooanother elérhető végpontot. Azonban hello állomás hello le van tiltva vagy nem megfelelő végpont mögött továbbra is meglévő kapcsolatokon keresztül tooreceive forgalom mindaddig, amíg a munkamenetek megszűnik. Alkalmazások hello munkamenet időtartama tooallow forgalom toodrain a meglévő kapcsolatok kell korlátoznia.

Ha egy profil végpontjai le vannak tiltva, vagy maga hello-profil le van tiltva, a Traffic Manager egy "NXDOMAIN" válasz tooa új DNS-lekérdezést küld.


## <a name="next-steps"></a>Következő lépések

* Ismerje meg, [Traffic Manager működése](traffic-manager-how-traffic-manager-works.md).
* Ismerje meg a Traffic Manager [végpont figyelési és automatikus feladatátvételt](traffic-manager-monitoring.md).
* Ismerje meg a Traffic Manager [forgalom-útválasztási módszerei](traffic-manager-routing-methods.md).
