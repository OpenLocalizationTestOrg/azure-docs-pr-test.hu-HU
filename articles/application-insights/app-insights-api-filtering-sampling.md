---
title: "aaaFiltering és a előfeldolgozása hello Azure Application Insights SDK |} Microsoft Docs"
description: "Telemetriai processzor- és Telemetria inicializálók írása hello SDK toofilter vagy tulajdonságok toohello adatok hozzáadása a hello telemetriai toohello Application Insights portál elküldése előtt."
services: application-insights
documentationcenter: 
author: beckylino
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 51b9db69b2375b8799718f1b0e1af77620dc2692
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="filtering-and-preprocessing-telemetry-in-hello-application-insights-sdk"></a>Szűrés és az Application Insights SDK hello telemetriai előfeldolgozása


Írása, és beépülő modulokat hello Application Insights SDK toocustomize hogyan telemetriai adatokat rögzíti, majd az Application Insights szolgáltatás toohello elküldés előtt feldolgozott konfigurálása.

* [A mintavételi](app-insights-sampling.md) anélkül, hogy befolyásolná a statisztika telemetriai hello mennyiségét csökkenti. Azonos tartja kapcsolatos adatok mutat, így közöttük, ha a probléma diagnosztizálása navigálhat. Hello portálon hello teljes számok pedig hello mintavételi rekordokkal toocompensate.
* Szűrés Telemetriai processzorokat [ASP.NET](#filtering) vagy [Java](app-insights-java-filter-telemetry.md) lehetővé teszi, hogy válasszon, vagy módosítsa az hello SDK telemetriai toohello server elküldés előtt. Telemetriai adatok mennyisége hello csökkentheti például kérelmek kizárja a robotokat. De szűrés mintavételi-nál több alapvető megközelítés tooreducing forgalmat. Mi kerül továbbításra teljesebb körű vezérlése lehetővé teszi, de vegye figyelembe, hogy az hatással van a statisztika - például toobe rendelkezik ki az összes sikeres kérelmek szűrése.
* [Telemetria inicializálók tulajdonságok hozzáadása](#add-properties) tooany telemetriai adatok küldése az alkalmazásból, beleértve a szabványos modulokban hello telemetriai. Például hozzáadhatja számított értékeket; vagy verziószámok hello portálon toofilter hello adatok alapján.
* [hello SDK API](app-insights-api-custom-events-metrics.md) használt toosend egyéni események és metrikák.

Előkészületek:

* Telepítse az Application Insights hello [SDK for ASP.NET](app-insights-asp-net.md) vagy [SDK Java](app-insights-java-get-started.md) az alkalmazásban.

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a>Szűrés: ITelemetryProcessor
Ez a módszer lehetővé teszi több közvetlen ellenőrzése alatt tartja a mi van, illetve tiltani szeretné hello telemetriai adatfolyamból. A mintavételi, párhuzamosan használható vagy külön-külön.

toofilter telemetriai adatokat, akkor írási telemetriai processzort, és regisztrálhatja azt az hello SDK. A processzor végighalad az összes telemetriai adat, és választhat toodrop hello le adatfolyam, vagy vegye fel a tulajdonságai. Ez magában foglalja a telemetriai hello szabványos modulokban például hello HTTP-kérelem adatgyűjtő és hello függőségi adatgyűjtő, valamint saját kezűleg írt telemetriai adatokat. Például szűrhetők telemetriai adatainak robots vagy sikeres függőségi hívások esetében érkező kérelmeket.

> [!WARNING]
> Szűrés hello SDK által küldött hello telemetriai processzorokat használó is eltolódhat hello statisztika kapcsolatban a hello portálon, és könnyebben nehéz toofollow kapcsolódó elemeket.
>
> Ehelyett érdemes [mintavételi](app-insights-sampling.md).
>
>

### <a name="create-a-telemetry-processor-c"></a>Hozzon létre egy telemetriai processzor (C#)
1. Győződjön meg arról, hogy hello Application Insights SDK célverzióját 2.0.0 verzió vagy újabb. Kattintson a jobb gombbal a projektre a Visual Studio Solution Explorerben, és válassza ki a NuGet-csomagok kezelése. A NuGet-Csomagkezelő ellenőrizze a Microsoft.ApplicationInsights.Web.
2. a szűrő toocreate ITelemetryProcessor valósítja meg. Ez egy másik bővítési pontot például telemetriai modul, telemetriai inicializáló és telemetriai csatorna.

    Figyelje meg, hogy a Telemetriai processzor feldolgozási láncolata összeállításához. Telemetria processzort hozható létre, ha egy hivatkozás toohello következő processzor át hello lánc. Ha a telemetriai adatok adatpont toohello folyamat metódus, a tevékenységeket végez el, és majd hívások hello hello láncban következő Telemetriai processzor.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors tooeach other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // toofilter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify hello item if required
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }

    ```
1. Helyezze be a ApplicationInsights.config:

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Ez az hello ahol inicializálni a mintavételi szűrő szakaszában azonos.)

Az osztály nyilvános elnevezett tulajdonságok megadásával átadhatók karakterlánc-értékek hello .config fájlból.

> [!WARNING]
> Mi gondoskodunk toomatch hello nevét és minden tulajdonságnevek hello .config fájl toohello osztály és a tulajdonságnevek hello kódban. Ha hello .config fájl egy nem létező típus vagy a tulajdonságra hivatkozik, hello SDK csendes sikertelenek lehetnek toosend bármely telemetriai adatokat.
>
>

**Másik lehetőségként** tudja inicializálni a kódban hello szűrő. A megfelelő inicializálási osztályban – például a Global.asax.cs AppStart - hello lánc a processzor beszúrása:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

Ezt a pontot fogja használni a processzor után létrehozott TelemetryClients.

### <a name="example-filters"></a>Példa szűrők
#### <a name="synthetic-requests"></a>Szintetikus kérelmek
Botok és a webalkalmazás-tesztek szűrik. Bár a Metrikaböngésző ad meg hello kimenő szintetikus források beállítás toofilter, ezt a beállítást, hello SDK szűréssel csökkenti a forgalmat.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Sikertelen hitelesítési
Szűrheti kérések "401-es" választ.

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // toofilter out an item, just terminate hello chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Kiszűrhetők a gyorsan távoli függőségi hívások esetében
Ha azt szeretné, amelyek lassú hívások toodiagnose csak, kiszűrhetők a hello gyors néhányat a meglévők közül.

> [!NOTE]
> Ez fogja döntés hello statisztika hello portálon megjelenik. hello függőségi diagram fog kinézni, mintha hello függőségi hívások esetében is az összes sikertelen.
>
>

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Függőségi problémák diagnosztikája
[Ebben a blogban](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) rendszeres pingelésre toodependencies automatikusan küldésével egy projekt toodiagnose függőségi problémákat ismerteti.


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a>Adja hozzá a tulajdonságok: ITelemetryInitializer
Használja a telemetriai adatok inicializálók toodefine globális tulajdonságait küldött az összes telemetriai adat; és toooverride kijelölt hello szabványos telemetriai modulok viselkedését.

Hello Application Insights webes csomag például HTTP-kérelmekre vonatkozó telemetriai adatokat gyűjt. Alapértelmezés szerint azt észleli, ha bármely kérelem válaszkód sikertelenként > = 400. Azonban ha azt szeretné, hogy egy sikeres 400 tootreat, megadhatja a telemetriai adatok inicializáló, amely beállítja hello sikeres tulajdonságot.

Ha megad egy telemetriai inicializáló, ha bármelyik hello Track*() nevezik módszert nevezik. Ez magában foglalja a hello szabványos telemetriai modulok által meghívott módszerek. Konvenció ezek a modulok egyik tulajdonságnak sem, amely már be van állítva egy inicializáló által nem állít be.

**Adja meg az inicializáló**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides hello default SDK
       * behavior of treating response codes >= 400 as failed requests
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set hello Success property, hello SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us toofilter these requests in hello portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave hello SDK tooset hello Success property      
        }
      }
    }
```

**Az inicializáló betöltése**

Az ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Másik lehetőségként* hello inicializáló a kód, például a Global.aspx.cs osztályból példányosítható:

```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Ez a minta több látszik.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a>JavaScript telemetriai inicializálók
*JavaScript*

Szúrja be a telemetriai adatok inicializáló közvetlenül a portáltól kapott hello portal hello inicializálási kód után:

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // toocheck hello telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // tooset custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // tooset custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Hello nem egyéni tulajdonságok hello telemetryItem elérhető összefoglalását lásd: [Application Insights exportálása adatmodell](app-insights-export-data-model.md).

Tetszőleges számú inicializálók szerint adhat hozzá.

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor és ITelemetryInitializer
Mi az a telemetriai adatok processzorral és telemetriai inicializálók hello különbségének?

* Van néhány átfedéseket, mi mindent velük a: használt tooadd tulajdonságok tootelemetry is lehet.
* TelemetryInitializers TelemetryProcessors előtt mindig fusson.
* TelemetryProcessors toocompletely csere engedélyezése, vagy vesse el a telemetriai adatok elemet.
* TelemetryProcessors teljesítmény számláló telemetriai nem feldolgozni.


## <a name="reference-docs"></a>Segédanyagok
* [API – Áttekintés](app-insights-api-custom-events-metrics.md)
* [ASP.NET-hivatkozás](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>SDK-kód
* [Az ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next"></a>Következő lépések
* [Keresési események és a naplókat](app-insights-diagnostic-search.md)
* [Mintavételezés](app-insights-sampling.md)
* [hibaelhárítással](app-insights-troubleshoot-faq.md)
