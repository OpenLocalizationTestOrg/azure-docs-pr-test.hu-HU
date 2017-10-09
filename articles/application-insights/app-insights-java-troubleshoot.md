---
title: az Application Insights aaaTroubleshoot Java webes projekt
description: "Hibaelhárítási útmutató - figyelési élő Java-alkalmazásokhoz az Application insights szolgáltatással."
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: c274c01b1992971fae194c3e510512ca06ab76b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Hibaelhárítás, kérdések és válaszok: Application Insights Java-hoz
Kérdések és problémák [Azure Application Insights Java nyelven][java]? Az alábbiakban néhány tipp.

## <a name="build-errors"></a>Build hibák
**Az eclipse-ben Maven vagy a gradle-lel, Application Insights SDK hozzáadása hello kattintáskor build vagy ellenőrzőösszeg érvényesítési hiba.**

* Ha függőségi hello <version> elem minta használatával helyettesítő karaktereket is tartalmazó (pl. (Maven) `<version>[1.0,)</version>` vagy (Gradle) `version:'1.0.+'`), próbáljon meg egy adott verziójához helyette például `1.0.2`. Lásd: hello [kibocsátási megjegyzéseket](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) hello legújabb verziójához.

## <a name="no-data"></a>Nincs adat
**I Application Insights hozzáadása sikeresen befejeződött, és saját alkalmazás lefutott, de soha nem láthatta, szeretnék adatokat hello portálon.**

* Várjon egy percet, és kattintson a frissítés parancsra. hello diagramok rendszeresen frissítse magát, de manuálisan is frissítheti. hello frissítési időköze hello időtartománya hello diagram függ.
* Ellenőrizze, hogy van-e egy rendszerállapot-kulcsot a hello ApplicationInsights.xml fájlban (a hello erőforrások mappában a projekt)
* Győződjön meg arról, hogy nincs `<DisableTelemetry>true</DisableTelemetry>` csomópont hello XML-fájlban.
* A tűzfal lehetséges, hogy 80-as és 443-as kimenő forgalom toodc.services.visualstudio.com tooopen TCP-portok. Lásd: hello [tűzfalkivételeket teljes listája](app-insights-ip-addresses.md)
* A Microsoft Azure hello board indításához hello állapot szolgáltatástérkép tekintse meg. Ha néhány riasztási jelzések, várjon, amíg azok vissza kellett volna tooOK majd zárja be és nyissa meg újra az Application Insights-alkalmazás paneljének.
* Kapcsolja be a naplózás toohello IDE konzolablakban hozzáadásával egy `<SDKLogger />` elem hello ApplicationInsights.xml fájlban (a hello erőforrások mappában a projekt), és ellenőrzés végrehajtásával kerüli meg a [hiba] bejegyzések hello legfelső szintű csomópontja alatt.
* Győződjön meg arról, hogy helyes-e ApplicationInsights.xml fájl sikeresen betöltötte hello Java SDK-t, ha megnézi a hello a konzol kimeneti üzenetek "konfigurációs fájl sikeresen talált" utasítás hello.
* Ha hello konfigurációs fájl nem található, ellenőrizze a hello kimeneti üzenetek toosee ahol keresett hello konfigurációs fájlban, és győződjön meg arról, hogy azokat a keresési helyek egyikén található ApplicationInsights.xml hello. A szokásos megoldás, mint közelében hello Application Insights SDK JARs elhelyezhet hello konfigurációs fájlban. Például: a Tomcat, ez azt jelentené hello WEB-INF/lib mappát.

#### <a name="i-used-toosee-data-but-it-has-stopped"></a>Használt toosee adatokat, de leállt
* Ellenőrizze a hello [állapot blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Elérte a havi kvótát, az adatokat? Nyissa meg a beállítások/kvóta és az árazás toofind ki. Ha igen, frissítse a csomagot, vagy további kapacitást kell fizetnie. Lásd: hello [séma árképzési](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-hello-data-im-expecting"></a>Nem szerepel az összes hello adatok I vagyok vár
* Nyissa meg a hello kvóták és a panel megnyitásához, és ellenőrizze e árképzési [mintavételi](app-insights-sampling.md) működik. (100 %-os átviteli azt jelenti, hogy a mintavételi műveletben nem.) az Application Insights szolgáltatás hello lehet set tooaccept csak töredéke alatt hello telemetriai adatot az alkalmazás nem érkezik. Ez segítséget nyújt tartásához telemetriai adatot a havi kvótán belül. 

## <a name="no-usage-data"></a>Nincs használati adatok
**Kérelmek és válaszidejét, de nincs lapmegtekintés, böngésző vagy felhasználói adatok láthatók.**

Sikeresen beállította az alkalmazás toosend telemetriai hello kiszolgálóról. Mostantól a következő lépés túl[állítsa be a weblapok toosend telemetriai hello webböngészőből][usage].

Azt is megteheti Ha az ügyfél az alkalmazásban egy [telefon vagy más eszköz][platforms], ott telemetriai adatokat küldhet. 

Használja az azonos hello instrumentation kulcs tooset fel az ügyfél- és telemetriai adatokat. hello adatok megjelennek, hello azonos Application Insights-erőforrást, és képes toocorrelate események az ügyfél és kiszolgáló lesz.


## <a name="disabling-telemetry"></a>Telemetria letiltása
**Hogyan letilthatja a telemetriai adatok gyűjtemény?**

A kódban:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Vagy** 

Frissítés ApplicationInsights.xml (mappában hello erőforrások a projekt). Adja hozzá a hello következő hello legfelső szintű csomópontja alatt:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

Hello XML metódussal rendelkezik toorestart hello alkalmazás hello érték módosításakor.

## <a name="changing-hello-target"></a>Hello cél módosítása
**Hogyan lehet módosítani a projekt adatokat küld az Azure erőforrás?**

* [Hello instrumentation kulcs hello új erőforrás lekérése.][java]
* Ha hozzáadta az Application Insights tooyour projekt hello Azure eszközkészlet használata az eclipse-ben, kattintson a jobb gombbal a webes projekthez, jelölje be **Azure**, **konfigurálja az Application Insights**, és módosítsa a hello kulcs.
* Ellenkező esetben frissíteni a hello erőforrások mappában a projekt ApplicationInsights.xml hello kulcsot.

## <a name="debug-data-from-hello-sdk"></a>Debug hello SDK adatait

**Hogyan tudhatom meg, milyen hello SDK végez műveletet?**

tooget mi történik a hello API-val kapcsolatos további információk hozzáadása `<SDKLogger/>` hello gyökércsomópont hello ApplicationInsights.xml konfigurációs fájl alapján.

Azt is beállíthatja, hogy hello naplózó toooutput tooa fájlt:

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

hello fájlok alatt található `%temp%\javasdklogs` vagy `java.io.tmpdir` esetén Tomcat kiszolgálót.


## <a name="hello-azure-start-screen"></a>hello Azure kezdőképernyőn
**Vagyok megnézi [hello Azure-portálon](https://portal.azure.com). Nem hello térkép arról, hogy valami kapcsolatos alkalmazásom?**

Azure-kiszolgálók állapotának hello nem, hello világ mutatja.

*Hello Azure start tábláról (kezdőképernyő) hogyan található adatok kapcsolatos alkalmazásom?*

Feltéve, hogy [az alkalmazás beállítása az Application Insights][java], kattintson a Tallózás gombra, válassza ki az Application Insights és válassza ki az alkalmazáshoz létrehozott hello alkalmazás-erőforrást. tooget nincs gyorsabb a későbbiekben rögzíthető az alkalmazás toohello start tábla.

## <a name="intranet-servers"></a>Intranetes kiszolgálók
**Képes a kiszolgáló is figyelése intranetről?**

Igen, a kiszolgáló elküldheti telemetriai toohello Application Insights portálon keresztül megadott hello a nyilvános internethez. 

A tűzfal lehetséges, hogy 80-as és 443-as kimenő forgalom toodc.services.visualstudio.com és f5.services.visualstudio.com tooopen TCP-portok.

## <a name="data-retention"></a>Adatmegőrzés
**Mennyi ideig adatok őrződnek meg hello portálon? Az biztonságos?**

Lásd: [az adatmegőrzés és az adatvédelmi][data].

## <a name="next-steps"></a>Következő lépések
**Az Application Insights saját Java server alkalmazás beállítása I. Mit tehetek?**

* [A weblapok rendelkezésre állásának figyelése][availability]
* [Weblap megfigyeléséhez][usage]
* [Használat követése és az eszköz alkalmazások problémák diagnosztizálása][platforms]
* [Az alkalmazás tootrack kódhasználat írása][track]
* [Diagnosztikai naplók rögzítése][javalogs]

## <a name="get-help"></a>Segítségkérés
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

