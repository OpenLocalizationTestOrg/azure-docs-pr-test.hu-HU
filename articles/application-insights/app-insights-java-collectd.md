---
title: "Linux - Azure Java web app teljesítményére aaaMonitor |} Microsoft Docs"
description: "Bővített alkalmazásteljesítmény-figyelés a Java-webhely hello CollectD beépülő modul az Application insights szolgáltatással."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: bwren
ms.openlocfilehash: f783e8607a83b2b43f67d3a2fc20f100aa2f75ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>collectd: Linux teljesítménymutatók az Application Insightsban


tooexplore Linux rendszer metrikák a [Application Insights](app-insights-overview.md), telepítse [collectd](http://collectd.org/), valamint azokat az Application Insights beépülő modult. A nyílt forráskódú megoldás különböző rendszer és a hálózati statisztikákat gyűjt.

Általában fogja használni collectd, ha már rendelkezik [a Java webes szolgáltatás az Application insights szolgáltatással tagolva][java]. Biztosít további adatok toohelp meg tooenhance az alkalmazás teljesítmény vagy a problémák diagnosztizálásához. 

![a minta diagramok](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a>A rendszerállapot-kulcs beszerzése
A hello [Microsoft Azure-portálon](https://portal.azure.com), nyissa meg hello [Application Insights](app-insights-overview.md) hello adatok tooappear kívánt erőforrás. (Vagy [hozzon létre egy új erőforrást](app-insights-create-new-resource.md).)

Tegye meg a hello erőforrás azonosítja hello instrumentation kulcs másolatával.

![Keresse meg az összes, nyissa meg az erőforrás és majd a hello Essentials legördülő listán, válassza ki, és másolja hello Instrumentation kulcs](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-hello-plug-in"></a>Collectd és hello beépülő modul telepítése
A Linux server gépeken:

1. Telepítés [collectd](http://collectd.org/) 5.4.0 verzió vagy újabb.
2. Töltse le a hello [Application Insights collectd író beépülő modul](https://aka.ms/aijavasdk). Vegye figyelembe a hello verziószáma.
3. Hello beépülő modul JAR másolja be `/usr/share/collectd/java`.
4. Szerkesztés `/etc/collectd/collectd.conf`:
   * Győződjön meg arról, hogy [Java beépülő modul hello](https://collectd.org/wiki/index.php/Plugin:Java) engedélyezve van.
   * Frissítés hello JVMArg hello java.class.path tooinclude hello JAR követően. Frissítés hello verzió száma toomatch hello egy letöltött:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Adja hozzá a kódrészletet, az erőforrás Instrumentation kulcs hello használata:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Íme egy példa konfigurációs fájl részeként:

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

Konfigurálja az egyéb [collectd beépülő modulok](https://collectd.org/wiki/index.php/Table_of_Plugins), amelyek különböző adatokat gyűjthet különböző forrásokból származó.

Indítsa újra a megfelelő tooits collectd [manuális](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-hello-data-in-application-insights"></a>Az Application Insightsban hello adatok megtekintése
Nyissa meg az Application Insights-erőforrás [Metrikaböngésző, és adja hozzá a diagramok][metrics], kívánt hello metrikák kiválasztásával hello egyéni kategóriából toosee.

![](./media/app-insights-java-collectd/result.png)

Alapértelmezés szerint hello metrikák összesítése, amelyből hello metrikák gyűjtött összes állomás számítógépével. tooview hello metrikák állomásonként hello diagram részletei panelen kapcsolja be a csoportosítás, és válassza a toogroup CollectD-állomás.

## <a name="tooexclude-upload-of-specific-statistics"></a>az adott statisztika tooexclude feltöltése a
Alapértelmezés szerint a hello Application Insights beépülő modult minden engedélyezett hello collectd által gyűjtött összes hello adatok "read" beépülő modulok küld. 

adott beépülő modulok, illetve az adatforrások tooexclude adatait:

* Szerkessze a hello konfigurációs fájlt. 
* A `<Plugin ApplicationInsightsWriter>`, adja hozzá a direktíva sorokat:

| Irányelv | Következmény |
| --- | --- |
| `Exclude disk` |Hello által gyűjtött összes adat kizárása `disk` beépülő modul |
| `Exclude disk:read,write` |Nevű kizárása hello `read` és `write` a hello `disk` beépülő modul. |

Külön irányelvek és egy új sor.

## <a name="problems"></a>Problémákat tapasztal?
*Nem szerepel az adatok hello portálon*

* Nyissa meg [keresési] [ diagnostic] toosee Ha hello nyers események érkezett. Hosszabb tooappear néha a metrikaböngészőben tartanak.
* Szükség lehet túl[tűzfalkivételeket a kimenő adatok beállítása](app-insights-ip-addresses.md)
* Nyomkövetés engedélyezése a hello Application Insights beépülő modult. Adja hozzá a sort belül `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Nyisson meg egy terminált, és collectd részletes üzemmódban toosee indítsa el a probléma merül fel jelentések:
  * `sudo collectd -f`

## <a name="known-issue"></a>Ismert hiba

hello Application Insights írási beépülő modul nem kompatibilis a bizonyos olvasási beépülő modulok. Néhány beépülő modulok néha küldése "NaN" ahol hello Application Insights beépülő modul egy lebegőpontos számot vár.

Jelenség: hello collectd napló mutatja, amely tartalmazza az "Eszközintelligencia:... SyntaxError: váratlan lexikális elem N ".

Megkerülő megoldás: Hello probléma írási beépülő modulok által gyűjtött adatok kihagyása. 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


