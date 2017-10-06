---
title: "aaaI Mobile Services használatához hogyan App Service segít?"
description: "Ismerje meg, milyen előnyökkel does az App Service szolgáltatásnak tooyour meglévő Mobile Services-projektek."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 26b68a11-8352-4f78-acd2-e4e0ec177781
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 315cc6eedcdca6c3f9f9bb9fd5ec7baf655b7e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started"></a>A Mobile Services-t használom, miben válhat előnyömre az App Service?
## <a name="overview"></a>Áttekintés
Az Ön által jelenleg használt Mobile Services mobilszolgáltatás biztonságban van, és a továbbiakban is támogatni fogjuk. Azonban vannak előnyei hello száma *Azure App Service* platformot biztosít a mobilalkalmazás által nem biztosított ma a Mobile Services:

* Egyszerűbb, átláthatóbb és költséghatékonyabb konstrukció alkalmazásokhoz; a választék webes és mobilos ügyfeleket egyaránt tartalmaz
* Új üzemeltetési funkciók, többek között a WebJobs használata, egyéni CNAME-ek és jobb ellenőrzés
* Kulcsrakész integráció a Traffic Managerrel
* Kapcsolat tooyour a helyszíni erőforrások és a VPN-kapcsolatok hozzáadása tooHybrid a VNet segítségével
* Az alkalmazáshoz kötődő figyelés, riasztás és hibaelhárítás a NewRelic vagy az AppInsights segítségével
* Szélesebb választék hello mögöttes számítási erőforrások és az árképzés terén
* Beépített automatikus skálázás, terheléselosztás és teljesítményfigyelés
* Beépített átmeneti üzembe helyezési, biztonsági mentési, visszaállítási és működés közbeni tesztelési funkciók

## <a name="new-hosting-features"></a>Új üzemeltetési funkciók
A *Azure App Service* hello *mobilalkalmazás* háttér kód futtatása a hello azonos tárolóban Web App és API-alkalmazásba. Így kihasználhatja a ebben a tárolóban, köztük olyanokat is, amelyek még nincsenek jelenleg a Mobile Services szolgáltatásait hello:

* Folyamatosan futó háttéralkalmazás-logika hozzáadása a WebJobson keresztül
* A háttéralkalmazás-kód állandó futásának biztosítása
* Egyéni CNAME tooprovide rövid használja, és állandó nevet tooyour mobil háttéralkalmazás végpontjainak
* Az alkalmazás földrajzi skálázása a Traffic Managerrel
* Bármely kívánt függvénytár vagy csomag bevonása
* (.NET esetében) az ASP.NET bármely funkciójának használata (például MVC)
* (A Node.js) Éljen az olyan hello csomópont ökoszisztéma, beleértve a közös MVC szalagtárak bármely tiszta JavaScript-függvénytárat.

## <a name="access-on-premises-data-using-vnet"></a>Helyszíni adatok elérése VNet segítségével
A Mobile Services ma már használhatja a hibrid kapcsolatok tooaccess a helyszíni erőforrások. Bizonyos esetekben azonban érdemesebb lehet VPN-megoldást használni. Az *Azure App Service* szolgáltatásban az Azure VNet is használható a Mobile Apps-háttéralkalmazás kódjához.

## <a name="use-your-favorite-backend-language"></a>A kívánt háttéralkalmazás-nyelv használata
*Az Azure App Service* ajánlatokat nevű szélesebb körű és az ASP.NET és a Node.js platformot, többek között gazdagabb támogatása hozzáférés toohello legújabb futtatókörnyezeteket.

## <a name="set-up-automatic-scale"></a>Automatikus skálázás beállítása
A Mobile Servicesben a háttéralkalmazás kódjának összes példánya kisebb méretű virtuális gépeken futott. *Az Azure App Service* lehetővé teszi a virtuális gépek, a beállítások jóval szélesebb körben tooselect hello méretét. Akkor is is gyors vertikális vagy horizontális toohandle bármilyen beérkező terhelés esetén különféle teljesítménymutatók alapján.

## <a name="be-in-hello-know"></a>Lehet, hogy a hello "tudja"
Tooissues reagálni a valós idejű figyelés és a riasztások tooautomatically értesíti, amelyeket a csapatával. Fejlett alkalmazáselemzési és -figyelési funkciókat a New Relic és az appinsights által biztosított tooget is betekintést a mobilalkalmazás teljesítményével hogyan integrálhatja. A *Azure App Service* most már beállíthatja programozottan és hello Azure portálon keresztül, számos különböző Teljesítményelemzési mutatón alapuló riasztást.

## <a name="keep-your-assets-safe"></a>Védelem objektumai számára
A szolgáltatás automatikus biztonsági mentést készít a háttéralkalmazásról és az adatbázisról. Kódja és adatai katasztrófa utáni biztonságos, könnyen visszaállított, hogy lehetővé teszi a toorun az üzleti az vetett bizalmat.

## <a name="ready-stage-go"></a>Vigyázz, kész, rajt!
Az *Azure App Service* segítségével több különböző privát tesztelési és átmeneti előkészítő környezet hozható létre a mobilalkalmazásokhoz. Használhatja őket a tooperform tesztelés központi telepítése előtt. Lapozófájl-kapacitás tooproduction állásidő nélkül. Webalkalmazások előre betölti, ami hozzájárul a lehető legjobb felhasználói élményhez hello.

Az *App Service* meglévő Mobile Service szolgáltatáshoz való használatát máris megkezdheti ezzel az [oktatóanyaggal](app-service-mobile-migrating-from-mobile-services.md).
