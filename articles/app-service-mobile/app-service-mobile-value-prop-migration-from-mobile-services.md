---
title: "A Mobile Services szolgáltatást használom, miben válhat előnyömre az App Service?"
description: "Ismerje meg, milyen előnyökkel jár az App Service szolgáltatásnak a meglévő Mobile Services projektjeibe való felvétele."
services: app-service\mobile
documentationcenter: ios
author: conceptdev
manager: crdun
editor: 
ms.assetid: 26b68a11-8352-4f78-acd2-e4e0ec177781
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: bdf49265b5ef88d11f4ed669aa05036839c574eb
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/04/2018
---
# <a name="getting-started"></a>A Mobile Services-t használom, miben válhat előnyömre az App Service?
## <a name="overview"></a>Áttekintés
Az Ön által jelenleg használt Mobile Services mobilszolgáltatás biztonságban van, és a továbbiakban is támogatni fogjuk. Az *Azure App Service* platform azonban számos olyan előnyt kínál mobilalkalmazása számára, amelyek a Mobile Services szolgáltatásban nem érhetők el:

* Egyszerűbb, átláthatóbb és költséghatékonyabb konstrukció alkalmazásokhoz; a választék webes és mobilos ügyfeleket egyaránt tartalmaz
* Új üzemeltetési funkciók, többek között a WebJobs használata, egyéni CNAME-ek és jobb ellenőrzés
* Kulcsrakész integráció a Traffic Managerrel
* Kapcsolat létesítése a helyszíni erőforrásokkal és VPN-ekkel hibrid kapcsolatok és virtuális hálózat használatával
* Az alkalmazáshoz kötődő figyelés, riasztás és hibaelhárítás a NewRelic vagy az AppInsights segítségével
* Szélesebb választék a mögöttes számítási erőforrások és az árképzés terén
* Beépített automatikus skálázás, terheléselosztás és teljesítményfigyelés
* Beépített átmeneti üzembe helyezési, biztonsági mentési, visszaállítási és működés közbeni tesztelési funkciók

## <a name="new-hosting-features"></a>Új üzemeltetési funkciók
Az *Azure App Service* szolgáltatásban a *Mobile Apps*-háttéralkalmazás ugyanabban a tárolóban fut, mint a webalkalmazás és az API-alkalmazás. Így igénybe veheti az ezen tároló által biztosított funkciókat, köztük olyanokat is, amelyek jelenleg nem találhatók meg a Mobile Services szolgáltatásban:

* Folyamatosan futó háttéralkalmazás-logika hozzáadása a WebJobson keresztül
* A háttéralkalmazás-kód állandó futásának biztosítása
* Egyéni CName nevek használata, amelyek barátságos és állandó nevet biztosítanak a mobil háttéralkalmazás végpontjainak
* Az alkalmazás földrajzi skálázása a Traffic Managerrel
* Bármely kívánt függvénytár vagy csomag bevonása
* (.NET esetében) az ASP.NET bármely funkciójának használata (például MVC)
* (Node.js esetében) A Node ökoszisztémához tartozó bármely tiszta JavaScript-függvénytár használata (például közös MVC-függvénytárak)

## <a name="access-on-premises-data-using-vnet"></a>Helyszíni adatok elérése VNet segítségével
A Mobile Services segítségével ma már hibrid kapcsolatok használatával is hozzáférhet helyszíni erőforrásaihoz. Bizonyos esetekben azonban érdemesebb lehet VPN-megoldást használni. Az *Azure App Service* szolgáltatásban az Azure VNet is használható a Mobile Apps-háttéralkalmazás kódjához.

## <a name="use-your-favorite-backend-language"></a>A kívánt háttéralkalmazás-nyelv használata
Az *Azure App Service* szélesebb körben és több funkcióval támogatja az ASP.NET és a Node.js platformot, többek között a legújabb futtatókörnyezeteket is elérhetővé teszi.

## <a name="set-up-automatic-scale"></a>Automatikus skálázás beállítása
A Mobile Servicesben a háttéralkalmazás kódjának összes példánya kisebb méretű virtuális gépeken futott. Az *Azure App Service* szolgáltatásban jóval szélesebb körben választhatja meg a virtuális gépek méretét. Ezenfelül az ügyfelektől beérkező terhelés esetén gyors vertikális vagy horizontális skálázást végezhet a korábbi teljesítménymutatók alapján.

## <a name="be-in-the-know"></a>Részletes információk a rendszerről
A felmerülő problémákról Ön és csapata is valós időben értesülhet az automatikus figyelési funkciók és riasztások segítségével. Ha szeretne részleteiben is tisztában lenni a mobilalkalmazás teljesítményével, akár a New Relic és az AppInsights által biztosított fejlett alkalmazáselemzési és -figyelési funkciókat is integrálhatja. Az *Azure App Service* segítségével mostantól számos különböző teljesítményelemzési mutatón alapuló riasztást állíthat be, akár a programon, akár az Azure Portalon keresztül.

## <a name="keep-your-assets-safe"></a>Védelem objektumai számára
A szolgáltatás automatikus biztonsági mentést készít a háttéralkalmazásról és az adatbázisról. Kódja és adatai még katasztrófa esetén sem vesznek el, és könnyen helyreállíthatók, így magabiztosan vezetheti üzletét.

## <a name="ready-stage-go"></a>Vigyázz, kész, rajt!
Az *Azure App Service* segítségével több különböző privát tesztelési és átmeneti előkészítő környezet hozható létre a mobilalkalmazásokhoz. Ezek segítségével még a végleges telepítés előtt alaposan tesztelheti az alkalmazásokat. Majd egyetlen perc leállási idő nélkül éles üzemre válthat. A webalkalmazásokat a rendszer előre betölti, ami hozzájárul a lehető legjobb felhasználói élményhez.

Az *App Service* meglévő Mobile Service szolgáltatáshoz való használatát máris megkezdheti ezzel az [oktatóanyaggal](app-service-mobile-migrating-from-mobile-services.md).
