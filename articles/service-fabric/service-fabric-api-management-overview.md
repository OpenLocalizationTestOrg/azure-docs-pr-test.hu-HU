---
title: "a Service Fabric aaaAzure API Management áttekintése |} Microsoft Docs"
description: "Ez a cikk egy bevezető toousing Azure API Management, mint egy átjáró tooyour Service Fabric-alkalmazások."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: vturecek
ms.openlocfilehash: f01dc570a11e68cd4a2d878abbe6019e209e2f5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-with-azure-api-management-overview"></a>A Service Fabric Azure API Management áttekintése

A felhőalapú alkalmazásokhoz általában kell egy előtér-átjáró tooprovide egy olyan hibaérzékeny pontot érkező felhasználók, eszközök és más alkalmazások. A Service Fabric átjáró lehet bármely állapotmentes szolgáltatások, mint egy [ASP.NET Core alkalmazás](service-fabric-reliable-services-communication-aspnetcore.md), vagy egy másik szolgáltatás, amely a forgalom érkező, például a [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IoT Hub](https://docs.microsoft.com/azure/iot-hub/), vagy [az Azure API Management](https://docs.microsoft.com/azure/api-management/).

Ez a cikk egy bevezető toousing Azure API Management, mint egy átjáró tooyour Service Fabric-alkalmazások. Az API Management közvetlenül integrálható a Service Fabric, így toopublish API-k az útválasztási szabályok tooyour Service Fabric háttérszolgáltatások széles skáláját. 

## <a name="architecture"></a>Architektúra
Egy közös Service Fabric-architektúrát használ egy egyoldalas webalkalmazást, amely HTTP-hívást tooback-szolgáltatások HTTP API-k visszaállítását végzi. Hello [Service Fabric-bevezető mintaalkalmazás](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) ebbe az architektúrába példáját mutatja be.

Ebben a forgatókönyvben egy állapot nélküli webszolgáltatás átjáróként szolgál hello hello Service Fabric-alkalmazás azokat. Ez a módszer igényli egy webszolgáltatás, amelyet a proxy HTTP is toowrite kérelmek tooback-szolgáltatások, ahogy az ábra a következő hello:

![A Service Fabric Azure API Management topológia – áttekintés][sf-web-app-stateless-gateway]

Mivel alkalmazások összetettségét, úgy, hogy az API-k tárfiókokhoz háttérszolgáltatások elé kell átjárók hello nő. Az Azure API Management az tervezett toohandle összetett API-k útválasztási szabályait, hozzáférés-vezérlési, sebességkorlátozást, figyelés, eseménynaplózás, és választ meg a minimális munka gyorsítótárazást. Az Azure API Management Service Fabric szolgáltatás felderítése partíció megoldás támogatja, és a replika kijelölés toointelligently útvonal igényel közvetlenül tooback-szolgáltatások a Service Fabric így nem kell toowrite saját állapotmentes API-átjáró. 

Ebben a forgatókönyvben a webes felhasználói felület továbbra is kiszolgált keresztül egy webszolgáltatás-bővítmény, amíg HTTP API-hívások által kezelt és Azure API Management keresztül történik, ahogy az ábra a következő hello hello:

![A Service Fabric Azure API Management topológia – áttekintés][sf-apim-web-app]

## <a name="application-scenarios"></a>Alkalmazáshasználati helyzetek

Lehet, hogy szolgáltatásait a Service Fabric állapot nélküli és állapotalapú, és azok particionálható három rendszerek egyikének használatával: egypéldányos, int-64 tartományon, és megnevezett. Szolgáltatási végpont feloldási van szükség, egy adott szolgáltatáspéldány az adott partíciók azonosításához. A szolgáltatás a végpont feloldásakor mindkét hello szolgáltatás példány neve (például `fabric:/myapp/myservice`), valamint hello adott partíció hello szolgáltatást meg kell adni, kivéve, Egypéldányos partíció hello esetében.

Az Azure API Management állapotmentes szolgáltatások, az állapotalapú szolgáltatások és bármely particionálási sémát bármely kombinációja használható.

## <a name="send-traffic-tooa-stateless-service"></a>Küldési forgalom tooa állapotmentes szolgáltatások

Hello legegyszerűbb esetben a forgalmat továbbítja tooa állapotmentes szolgáltatások példány. tooachieve, ez az API Management műveletet tartalmazza a Service Fabric-háttér, amely leképezhető tooa adott állapotmentes szolgáltatások példánya a Service Fabric-háttér hello egy bejövő feldolgozási házirendje. Toothat szolgáltatás küldött kérések küldése tooa véletlenszerű hello állapotmentes szolgáltatások-példány másolatának.

#### <a name="example"></a>Példa
A forgatókönyv a következő hello, a Service Fabric-alkalmazás tartalmazza a állapotmentes szolgáltatások nevű `fabric:/app/fooservice`, amely közzétesz egy belső HTTP API-t. hello szolgáltatás példánynév ismert, és kódolva közvetlenül a hello API Management bejövő házirend. 

![A Service Fabric Azure API Management topológia – áttekintés][sf-apim-static-stateless]

## <a name="send-traffic-tooa-stateful-service"></a>Küldési forgalom tooa állapotalapú szolgáltatás

Hasonló toohello állapotmentes szolgáltatások forgatókönyvben forgalom továbbítható tooa állapotalapú service-példány. Ebben az esetben az API Management műveletet tartalmaz egy bejövő feldolgozási házirendje a Service Fabric-háttér, amely leképezhető egy kérelem tooa adott egy adott partícióra *állapotalapú alkalmazások és szolgáltatások* szolgáltatáspéldány. hello partíció toomap minden egyes kérelem toois számított egy lambda módszerrel néhány bemenetének hello bejövő HTTP-kérelem, például egy érték hello URL-cím használatával. hello házirend konfigurált toosend kérelmek toohello elsődleges replika csak vagy tooa véletlenszerű olvasási műveletek replikája lehet.

#### <a name="example"></a>Példa

A forgatókönyv a következő hello, a Service Fabric-alkalmazás tartalmazza egy particionált állapotalapú service nevű `fabric:/app/userservice` , amely közzétesz egy belső HTTP API-t. hello szolgáltatás példánynév ismert, és kódolva közvetlenül a hello API Management bejövő házirend.  

hello szolgáltatás particionálása hello Int64 partícióséma két partíciókkal rendelkező és a kulcs széles kiterjedő `Int64.MinValue` túl`Int64.MaxValue`. hello háttér-házirend kiszámítja a partíciós kulcs, azon belül hello átalakításával `id` hello URL-cím kérelem elérési útja tooa 64 bites egész szám, bár egy algoritmus használt ide toocompute hello partíciókulcs lehet megadott érték. 

![A Service Fabric Azure API Management topológia – áttekintés][sf-apim-static-stateful]

## <a name="send-traffic-toomultiple-stateless-services"></a>Forgalom küldése toomultiple állapotmentes szolgáltatások

A speciális esetben meghatározhatja, hogy a maps kérelmek mint egy szolgáltatáspéldány toomore API Management művelet. Ebben az esetben minden műveletet tartalmaz egy házirendet, amely a hello bejövő HTTP-kérelem, például az hello URL-cím elérési út vagy lekérdezési karakterláncot, és állapotalapú szolgáltatások hello esetben az értékek alapján tooa adott szolgáltatáspéldány egy partíción belül hello szolgáltatáspéldány kéréseket . 

tooachieve Ez az API Management műveletet tartalmaz egy bejövő feldolgozási házirendje a Service Fabric-háttér, amely leképezhető tooa állapotmentes szolgáltatások példány hello Service Fabric-háttér hello bejövő HTTP-kérelem lekért értékek alapján. Kérelmek tooa szolgáltatáspéldány tooa véletlenszerű hello service-példány másolatának küldése.

#### <a name="example"></a>Példa

Ebben a példában egy új állapotmentes szolgáltatáspéldány hello a következő képlet használatával dinamikusan generált nevű létrejön az alkalmazás minden felhasználó számára:
 
 - `fabric:/app/users/<username>`

 Minden szolgáltatás egyedi névvel rendelkezik, de hello nevek nem ismert kezdeti mert hello szolgáltatások válasz toouser jönnek létre, vagy a rendszergazda adja meg, és így nem lehet kódolt APIM házirendek vagy útválasztási szabályokat. Ehelyett hello szolgáltatás toowhich toosend kérelmet hello neve hello hello háttér-házirend-definíció létrehozása történik `name` hello URL-cím vonatkozó kérelem elérési útját a megadott érték. Példa:

  - A kérelem túl`/api/users/foo` irányított tooservice példánya`fabric:/app/users/foo`
  - A kérelem túl`/api/users/bar` irányított tooservice példánya`fabric:/app/users/bar`

![A Service Fabric Azure API Management topológia – áttekintés][sf-apim-dynamic-stateless]

## <a name="send-traffic-toomultiple-stateful-services"></a>Forgalom küldése toomultiple állapotalapú szolgáltatások

Hasonló toohello állapotmentes szolgáltatások példában egy művelet leképezheti API Management kérelmek toomore, mint egy **állapotalapú alkalmazások és szolgáltatások** példány szolgáltatás, ebben az esetben akkor is előfordulhat, hogy újra kell tooperform partíció megoldási minden egyes állapotalapú szolgáltatás a példány.

tooachieve Ez az API Management műveletet tartalmaz egy bejövő feldolgozási házirendje a Service Fabric-háttér, amely leképezhető tooa állapotalapú service-példány a hello Service Fabric-háttér hello bejövő HTTP-kérelem lekért értékek alapján. Egy kérelem toospecific szolgáltatáspéldány, hello kérelem toomapping továbbá csatlakoztatott tooa adott partíción belül hello szolgáltatáspéldány, és szükség esetén tooeither hello elsődleges másodpéldány, vagy hello partíción belül véletlenszerű másodlagos replika is lehet.

#### <a name="example"></a>Példa

Ebben a példában egy új állapot-nyilvántartó szolgáltatáspéldány hozzon létre hello alkalmazás felhasználóinak egy dinamikusan előállított neve a következő képlet hello használata:
 
 - `fabric:/app/users/<username>`

 Minden szolgáltatás egyedi névvel rendelkezik, de hello nevek nem ismert kezdeti mert hello szolgáltatások válasz toouser jönnek létre, vagy a rendszergazda adja meg, és így nem lehet kódolt APIM házirendek vagy útválasztási szabályokat. Ehelyett hello szolgáltatás toowhich toosend kérelmet hello neve hello hello háttér-házirend-definíció létrehozása történik `name` megadott érték hello URL-cím vonatkozó kérelem elérési útját. Példa:

  - A kérelem túl`/api/users/foo` irányított tooservice példánya`fabric:/app/users/foo`
  - A kérelem túl`/api/users/bar` irányított tooservice példánya`fabric:/app/users/bar`

Minden szolgáltatáspéldány is particionálása hello Int64 partícióséma két partíciókkal rendelkező és a kulcs széles kiterjedő `Int64.MinValue` túl`Int64.MaxValue`. hello háttér-házirend kiszámítja a partíciós kulcs, azon belül hello átalakításával `id` hello URL-cím kérelem elérési útja tooa 64 bites egész szám, bár egy algoritmus használt ide toocompute hello partíciókulcs lehet megadott érték. 

![A Service Fabric Azure API Management topológia – áttekintés][sf-apim-dynamic-stateful]

## <a name="next-steps"></a>Következő lépések

Hajtsa végre a hello [– első lépések útmutató](service-fabric-api-management-quick-start.md) tooset fel az első Service Fabric fürt az API Management és a folyamat által az API Management tooyour szolgáltatásokon keresztül.

<!-- links -->

<!-- pics -->
[sf-apim-web-app]: ./media/service-fabric-api-management-overview/sf-apim-web-app.png
[sf-web-app-stateless-gateway]: ./media/service-fabric-api-management-overview/sf-web-app-stateless-gateway.png
[sf-apim-static-stateless]: ./media/service-fabric-api-management-overview/sf-apim-static-stateless.png
[sf-apim-static-stateful]: ./media/service-fabric-api-management-overview/sf-apim-static-stateful.png
[sf-apim-dynamic-stateless]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateless.png
[sf-apim-dynamic-stateful]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateful.png