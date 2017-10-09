---
title: az Azure web app forgalom az Azure Traffic Manager aaaControlling
description: "A cikk nyújt összefoglaló információkat az Azure Traffic Manager tooAzure web Apps alkalmazások felügyeletében."
services: app-service\web
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.openlocfilehash: a93d4c9370046d54e401e36e7b495af8b711a2aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Azure-webalkalmazás-forgalom vezérlése az Azure Traffic Managerrel
> [!NOTE]
> Ez a cikk nyújt összefoglaló információkat a Microsoft Azure Traffic Manager tooAzure App Service Web Apps vonatkozik. További információ a Azure Traffic Manager magát felkeresésével hello hivatkozások hello Ez a cikk végén található.
> 
> 

## <a name="introduction"></a>Bevezetés
Használhatja az Azure Traffic Manager toocontrol hogyan webes ügyfelek tooweb elosztott alkalmazások az Azure App Service-ben. Webes alkalmazás végpontok Azure Traffic Manager-profil tooa ad hozzá, amikor Azure Traffic Manager nyomon követi az hello állapota (fut, leállítva vagy törölt) webalkalmazások így eldöntheti, amelyek ezekre a végpontokra kell kapnia a forgalmat.

## <a name="load-balancing-methods"></a>Terheléselosztási módszer betöltése
Az Azure Traffic Manager három másik terheléselosztási módszert használja. Ezekről az alábbi lista követve tooAzure webalkalmazások eseménykódok hello.

* **Feladatátvételi**: Ha web app klónok különböző régiókban, is használja a metódus tooconfigure egy webes alkalmazás tooservice minden webes ügyfélforgalmat, és egy másik régióban tooservice, amely az első webalkalmazás eset hello forgalom a másik webalkalmazás konfigurálása nem érhető el.
* **Ciklikus multiplexelés**: Ha web app klónok különböző régiókban, használhatja a metódus toodistribute forgalom egyaránt különböző régiókban hello webes alkalmazások között.
* **Teljesítmény**: hello teljesítmény metódus osztja el a forgalmat a hello legrövidebb oda-vissza idő tooclients alapján. hello teljesítmény metódus nem használható webalkalmazások belül hello ugyanabban a régióban vagy különböző régiókban.

## <a name="web-apps-and-traffic-manager-profiles"></a>Webalkalmazások és a Traffic Manager-profilok
webes alkalmazás forgalom tooconfigure hello irányítását, akkor profil létrehozása az Azure Traffic Managerben, amely három hello terheléselosztási korábban ismertetett módszerek valamelyikét használja, és adja hozzá hello végpontok (ebben az esetben az webalkalmazások) legyen toocontrol forgalom toohello profil. A webes alkalmazás állapota (fut, leállítva vagy törölt) van rendszeresen közölt toohello profilját, hogy az Azure Traffic Manager forgalom ennek megfelelően irányíthatók.

Azure Traffic Manager használata az Azure-ral, tartsa szem előtt tartva hello pontok a következő:

* A webes alkalmazás csak telepítésekhez belül hello ugyanabban a régióban, a webalkalmazások már helyeire feladatátvételi és ciklikus multiplexelés legutóbb tooweb módja nélkül.
* A központi telepítések hello ugyanabban a régióban, amely egy másik Azure-felhőszolgáltatásban együtt használni a webalkalmazásokat, kombinálhatja a mindkét típusú végpontok tooenable hibrid környezetekben.
* Egy webes alkalmazás végpontjának régiónként csak egy profilt adhat meg. Ha bejelöli a webes alkalmazás egy régióhoz, az adott régióban, már nem érhető el, hogy a profil-kiválasztási webalkalmazások fennmaradó hello végpontjaként.
* hello web app végpontok Azure Traffic Manager-profil meghatározott jelenik meg a hello **tartománynevek** hello konfigurálása lapon hello webalkalmazás hello-profil szakaszában, de csak akkor konfigurálható van.
* Miután hozzáadta a webes alkalmazás tooa profil, hello **webhely URL-címe** hello hello Web irányítópult az alkalmazás portál oldal jelenik meg, hello egyéni tartomány webalkalmazás URL-címe hello Ha beállított egyet. Ellenkező esetben megjelenik a hello Traffic Manager-profil URL-CÍMÉT (például `contoso.trafficmgr.com`). Mindkét hello közvetlen tartománynév hello webalkalmazás és hello Traffic Manager URL-cím fog megjelenni a hello webes alkalmazás konfigurálása lapon hello alatt **tartománynevek** szakasz.
* Egyéni tartományneve működni fog az elvárásoknak megfelelően, de a hozzáadása tooadding őket tooyour web apps, úgy kell konfigurálnia a DNS térkép toopoint toohello Traffic Manager URL-CÍMÉT. Olvashat be egy Azure web app alkalmazásban egy egyéni tartományt tooset lásd [egy egyéni tartománynevet egy Azure-webhely konfigurálása](app-service-web-tutorial-custom-domain.md).
* Csak a standard vagy prémium mód tooa Azure Traffic Manager-profil webalkalmazások adhat hozzá.

## <a name="next-steps"></a>Következő lépések
A fogalmi és technikai áttekintés az Azure Traffic Manager: [Traffic Manager – áttekintés](../traffic-manager/traffic-manager-overview.md).

Traffic Manager a Web Apps használatával kapcsolatos további információkért lásd: hello blogbejegyzések [Azure Traffic Manager használata az Azure-webhelyek](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) és [Azure Traffic Manager most már integrálható az Azure-webhelyek](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

