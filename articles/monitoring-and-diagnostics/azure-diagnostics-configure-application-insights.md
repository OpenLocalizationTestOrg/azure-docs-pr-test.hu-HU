---
title: aaaConfigure Azure Diagnostics toosend adatok tooApplication Insights |} Microsoft Docs
description: "Hello Azure Diagnostics nyilvános konfigurációs toosend adatok tooApplication Insights frissítése."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: f9e12c3e-c307-435e-a149-ef0fef20513a
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2016
ms.author: robb
ms.openlocfilehash: 7c36f29da8fdc12fa58c17458348a311b900b0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-tooapplication-insights"></a>A felhőalapú szolgáltatás, a virtuális gép vagy a Service Fabric diagnosztikai adatok tooApplication Insights küldése
Felhőszolgáltatások, virtuális gép, virtuálisgép-méretezési csoportok és a Service Fabric összes hello Azure Diagnostics bővítmény toocollect adatait használja.  Az Azure diagnosztikai adatokat tooAzure tárolási táblák küldi el.  Azonban is minden cső vagy az Azure Diagnostics kiterjesztéssel 1.5-ös vagy újabb hello tooother helyének részhalmaza.

Ez a cikk ismerteti, hogyan toosend adatait hello Azure Diagnostics bővítmény tooApplication Insights.

## <a name="diagnostics-configuration-explained"></a>Diagnosztikai konfiguráció ismertetése
hello Azure diagnostics bővítmény 1.5-ös bevezetett felül, amelyek további helyre, ahol a diagnosztikai adatokat küldhet.

Példa az konfigurálását, a fogadó Application Insights:

```XML
<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "ApplicationInsights",
            "ApplicationInsights": "{Insert InstrumentationKey}",
            "Channels": {
                "Channel": [
                    {
                        "logLevel": "Error",
                        "name": "MyTopDiagData"
                    },
                    {
                        "logLevel": "Error",
                        "name": "MyLogData"
                    }
                ]
            }
        }
    ]
}
```
- Hello **fogadó** *neve* attribútum, amely egyedileg azonosítja a hello fogadó karakterlánc-érték.

- Hello **ApplicationInsights** elem megadja az Application insights-erőforrás ahol hello Azure diagnosztikai adatok küldése hello instrumentation kulcsot.
    - Ha még nem rendelkezik egy meglévő Application Insights-erőforrást, lásd: [hozzon létre egy új Application Insights-erőforrást](../application-insights/app-insights-create-new-resource.md) hello instrumentation kulcs erőforrás létrehozása és a további tájékoztatást.
    - Ha egy felhőalapú szolgáltatást biztosít az Azure SDK 2.8 fejleszt, a instrumentation kulcs automatikusan feltöltődik értékkel. hello érték alapul hello **APPINSIGHTS_INSTRUMENTATIONKEY** szolgáltatás konfigurációs beállítás, amikor csomagolására hello Cloud Service-projektet. Lásd: [használja az Application Insights az Azure Diagnostics tootroubleshoot Felhőszolgáltatás problémák](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).

- Hello **csatornák** egy vagy több elemet tartalmaz **csatorna** elemek.
    - Hello *neve* attribútum egyedileg toothat csatorna hivatkozik.
    - Hello *loglevel* attribútum Megadja, hogy a csatorna hello hello naplózási szint lehetővé teszi lehetővé. a legtöbb tooleast információt sorrendje hello elérhető naplózási szintek a következők:
        - Részletes
        - Információ
        - Figyelmeztetés
        - Hiba
        - Kritikus

Egy csatorna úgy viselkedik, mint egy szűrőt, és lehetővé teszi tooselect adott naplózási szintek toosend toohello cél fogadó. Például akkor sikerült részletes gyűjtését és elküldhesse az összeset toostorage, de csak a hibák toohello gyűjtő küldés.

hello következő ábra ezt a kapcsolatot mutatja be.

![Diagnosztikai nyilvános konfigurációja](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

a következő ábra hello hello konfigurációs érték és leírás foglalja össze. Több mosdók hello konfigurációs hello hierarchia különböző szintjein is felvehet. hello felső szintjén hello fogadó úgy működik, mint a globális beállítás és hello egy meghatározott, egyéni hello elem úgy viselkedik, mint egy felülbírálás toothat globális beállítás.

![Diagnosztika fogadók esetében az Application Insights-konfiguráció](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a>Teljes fogadó konfigurációja – példa
Ez egy teljes példa hello nyilvános konfigurációs fájlba
1. elküldi az összes hiba tooApplication Insights (hello megadott **DiagnosticMonitorConfiguration** csomópont)
2. Részletes szintű alkalmazásnaplók hello naplókat is küld (hello megadott **naplók** csomópont).

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent toothis channel -->
    <DiagnosticInfrastructureLogs />
    <PerformanceCounters>
      <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
      <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
    </PerformanceCounters>
    <WindowsEventLog scheduledTransferPeriod="PT1M">
      <DataSource name="Application!*" />
    </WindowsEventLog>
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent toothis channel -->
  </DiagnosticMonitorConfiguration>

<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
  </SinksConfig>
</WadCfg>
```
```JSON
"WadCfg": {
    "DiagnosticMonitorConfiguration": {
        "overallQuotaInMB": 4096,
        "sinks": "ApplicationInsights.MyTopDiagData", "_comment": "All info below sent toothis channel",
        "DiagnosticInfrastructureLogs": {
        },
        "PerformanceCounters": {
            "PerformanceCounterConfiguration": [
                {
                    "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                    "sampleRate": "PT3M"
                },
                {
                    "counterSpecifier": "\\Memory\\Available MBytes",
                    "sampleRate": "PT3M"
                }
            ]
        },
        "WindowsEventLog": {
            "scheduledTransferPeriod": "PT1M",
            "DataSource": [
                {
                    "name": "Application!*"
                }
            ]
        },
        "Logs": {
            "scheduledTransferPeriod": "PT1M",
            "scheduledTransferLogLevelFilter": "Verbose",
            "sinks": "ApplicationInsights.MyLogData", "_comment": "This specific info sent toothis channel"
        }
    },
    "SinksConfig": {
        "Sink": [
            {
                "name": "ApplicationInsights",
                "ApplicationInsights": "{Insert InstrumentationKey}",
                "Channels": {
                    "Channel": [
                        {
                            "logLevel": "Error",
                            "name": "MyTopDiagData"
                        },
                        {
                            "logLevel": "Verbose",
                            "name": "MyLogData"
                        }
                    ]
                }
            }
        ]
    }
}
```
Hello előző konfigurációban az alábbi hello rendelkezik hello jelentése a következő:

### <a name="send-all-hello-data-that-is-being-collected-by-azure-diagnostics"></a>Hello Azure diagnostics által gyűjtött összes adat küldése

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-toohello-application-insights-sink"></a>Csak a hiba naplók toohello Application Insights gyűjtő küldés

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-tooapplication-insights"></a>Részletes alkalmazásnaplók tooApplication Insights küldése

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a>Korlátozások

- **Csatornák csak típusa és a teljesítmény nem számlálók naplózásához.** Ha egy csatornát a teljesítmény számláló elemet adja meg, figyelmen kívül.
- **egy csatorna hello naplózási szint nem lehet hosszabb hello naplózási szint esetében mi az Azure diagnostics gyűjtött.** Például nem hello naplók elem alkalmazásnaplóban hibáinak gyűjtése, és próbálja toosend részletes naplókat toohello Application Insights fogadó. Hello *scheduledTransferLogLevelFilter* attribútum mindig gyűjtése egyenlő vagy mint hello naplózza, akkor további naplók próbált toosend tooa fogadó.
- **Az Azure diagnostics bővítmény tooApplication Insights által gyűjtött blob adatokat nem lehet elküldeni.** Például semmit hello alatt megadott *könyvtárak* csomópont. Összeomlási memóriaképek hello tényleges összeomlási memóriakép tooblob tárolási zajlik, és csak összeomlási memóriakép hello értesítést jött létre tooApplication Insights zajlik.

## <a name="next-steps"></a>Következő lépések
* Megtudhatja, hogyan túl[tekintse meg az Azure diagnostics](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) az Application insights szolgáltatással.
* Használjon [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) tooenable hello Azure diagnostics bővítményt az alkalmazáshoz.
* Használjon [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) tooenable hello Azure diagnostics-bővítmény alkalmazás
