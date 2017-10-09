---
title: "Application Insights Telemetria adatmodell - aaaAzure Telemetriai környezetben |} Microsoft Docs"
description: "Application Insights telemetria környezetben adatmodell"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/15/2017
ms.author: sergkanz
ms.openlocfilehash: 6cdd6240d1c448f883d104a871ee9d5f6b5af2ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="telemetry-context-application-insights-data-model"></a>Telemetria a környezetben: Application Insights adatmodell

Minden telemetriai elem lehet egy szigorú típusmegadású környezet mezőket. Minden mező lehetővé teszi, hogy egy adott figyelési forgatókönyvhöz. Hello egyéni tulajdonságok gyűjtemény toostore egyéni vagy az alkalmazás-specifikus környezetfüggő információkat használja.


##<a name="application-version"></a>Alkalmazás verziószáma

Információk hello alkalmazás környezet mezőiben mindig tárgya hello hello telemetriai adatokat küldő alkalmazás. Alkalmazás verziószáma használt tooanalyze trend változásait hello alkalmazás viselkedését, valamint a korrelációs toohello központi telepítései.

Maximális hossz: 1024


##<a name="client-ip-address"></a>Ügyfél IP-címe

hello hello ügyféleszköz IP-címe. IPv4 és IPv6 használata támogatott. Telemetriai adatokat küldött a szolgáltatásból, hello hely környezetben hello szolgáltatásban hello műveletet kezdeményezett hello felhasználóról van. Az Application Insights hello földrajzihely-információk kinyerése hello ügyfél IP-címről, és majd csonkolja azt. Ezért ügyfél IP-önmagában nem használható azonosításra alkalmas adatokat. 

Maximális hossz: 46


##<a name="device-type"></a>Eszköz típusa

Ez a mező eredetileg hello eszköz hello végfelhasználói hello alkalmazás tooindicate hello típusát. Ma hello eszköz típushoz "Browser" a kiszolgálóoldali telemetriai adatokból a hello eszköztípus "PC" használt elsősorban a toodistinguish JavaScript telemetriai adatokat.

Maximális hossz: 64


##<a name="operation-id"></a>A művelet azonosítója

Hello legfelső szintű tevékenység egyedi azonosítója. Ez az azonosító lehetővé teszi, hogy toogroup telemetriai több összetevői között. Lásd: [telemetriai korrelációs](application-insights-correlation.md) részleteiről. hello műveletazonosító kérelmet vagy a lap nézet hozta létre. Egyéb telemetriai beállítja a mező toohello tartozó kérelem vagy a lap nézetet tartalmazó hello. 

Maximális hossz: 128


##<a name="parent-operation-id"></a>Szülő művelet azonosítója

hello azonnali szülő hello telemetriai elem egyedi azonosítója. Lásd: [telemetriai korrelációs](application-insights-correlation.md) részleteiről.

Maximális hossz: 128


##<a name="operation-name"></a>A művelet neve

hello (csoport) hello művelet neve. hello művelet neve vagy a kérelmet, vagy a lap nézet hozta létre. Telemetriai minden más elemet a hello kérelemmel vagy a lap nézetet tartalmazó mező toohello értékének beállítása. Műveletek (például "GET otthoni/Index") a csoport összes hello telemetriai elemek kereséséhez használt művelet neve. A context tulajdonság használt tooanswer kérdéseket, például "Mik hello tipikus kivételek ezen az oldalon."

Maximális hossz: 1024


##<a name="synthetic-source-of-hello-operation"></a>Szintetikus forrás hello művelet

Szintetikus forrás nevére. Néhány telemetriai hello alkalmazásból szintetikus forgalom jelöl. Lehet, hogy webes webbejáró indexelési hello webhely, hely rendelkezésreállás figyelésére szolgáló tesztek, például maga Application Insights SDK diagnosztikai szalagtárak nyomkövetéseit.

Maximális hossz: 1024


##<a name="session-id"></a>munkamenet-azonosító

Munkamenet-azonosító - hello felhasználói beavatkozás hello alkalmazással hello példányát. Információk hello munkamenet környezetben mezőiben mindig tárgya hello végfelhasználói. Telemetriai adatokat küldött a szolgáltatás, a munkamenet-környezet hello hello szolgáltatásban hello műveletet kezdeményezett hello felhasználóról van.

Maximális hossz: 64


##<a name="anonymous-user-id"></a>A névtelen felhasználói azonosító

A névtelen felhasználói azonosító. Hello végfelhasználói hello alkalmazás jelöli. Telemetriai adatokat küldött a szolgáltatásból, hello felhasználói környezet hello szolgáltatásban hello műveletet kezdeményezett hello felhasználóról van.

[A mintavételi](app-insights-sampling.md) hello technikák toominimize hello mennyisége gyűjtött telemetriai egyike. A mintavételi kísérletek tooeither algoritmus minta bejövő vagy kimenő összes hello korrelált telemetriai adatokat. A névtelen felhasználói azonosítót használja a pontszám generációs mintavétel. Így a névtelen felhasználói azonosító elég véletlenszerű értéknek kell lennie. 

A névtelen felhasználói azonosító toostore felhasználónév használata a hello mező való visszaélés. Hitelesített felhasználói azonosítóját használnia.

Maximális hossz: 128


##<a name="authenticated-user-id"></a>Hitelesített felhasználó azonosítója

Hitelesített felhasználó azonosítója. a névtelen felhasználói azonosítóját, ez a mező ellentétes hello hello felhasználót egy rövid nevet jelöli. Óta a PII adatok akkor van alapértelmezés szerint nem gyűjt a legtöbb SDK-ban.

Maximális hossz: 1024


##<a name="account-id"></a>Futtatófiók-azonosítóvá

A több-bérlős alkalmazásokhoz Ez a hello futtatófiók-Azonosítóvá vagy nevét, mely hello felhasználó működhet együtt. Példák az Azure portál vagy blog neve bloggolás platform előfizetés-azonosító lehet.

Maximális hossz: 1024


##<a name="cloud-role"></a>Felhő szerepkör

Hello szerepkör hello alkalmazás nevének egy részét képezi. A Maps közvetlenül toohello szerepkör neve az azure-ban. Akkor is használt toodistinguish micro szolgáltatásokat, amelyek egyetlen alkalmazás részét képezik.

Maximális hossz: 256


##<a name="cloud-role-instance"></a>Felhő szerepkör példánya

Hello alkalmazást futtató hello példány nevét. Számítógép neve a helyszíni, az Azure-példány nevét.

Maximális hossz: 256


##<a name="internal-sdk-version"></a>Belső: SDK-verzió

SDK-verzió. Lásd: https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification információt.

Maximális hossz: 64


##<a name="internal-node-name"></a>Belső: A csomópont neve

Ez a mező képviseli hello csomópontnév számlázási célokat szolgál. Toooverride hello szabványos észlelési csomópontok használni.

Maximális hossz: 256


## <a name="next-steps"></a>Következő lépések

- Ismerje meg, hogyan túl[kiterjesztése és a telemetriai adatok szűrése](app-insights-api-filtering-sampling.md).
- Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.
- Tekintse meg a standard környezetben tulajdonsággyűjteményében [konfigurációs](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).
