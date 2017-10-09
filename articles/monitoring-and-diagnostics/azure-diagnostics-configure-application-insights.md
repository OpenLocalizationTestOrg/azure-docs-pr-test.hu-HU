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
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-tooapplication-insights"></a><span data-ttu-id="d9326-103">A felhőalapú szolgáltatás, a virtuális gép vagy a Service Fabric diagnosztikai adatok tooApplication Insights küldése</span><span class="sxs-lookup"><span data-stu-id="d9326-103">Send Cloud Service, Virtual Machine, or Service Fabric diagnostic data tooApplication Insights</span></span>
<span data-ttu-id="d9326-104">Felhőszolgáltatások, virtuális gép, virtuálisgép-méretezési csoportok és a Service Fabric összes hello Azure Diagnostics bővítmény toocollect adatait használja.</span><span class="sxs-lookup"><span data-stu-id="d9326-104">Cloud services, Virtual Machines, Virtual Machine Scale Sets and Service Fabric all use hello Azure Diagnostics extension toocollect data.</span></span>  <span data-ttu-id="d9326-105">Az Azure diagnosztikai adatokat tooAzure tárolási táblák küldi el.</span><span class="sxs-lookup"><span data-stu-id="d9326-105">Azure diagnostics sends data tooAzure Storage tables.</span></span>  <span data-ttu-id="d9326-106">Azonban is minden cső vagy az Azure Diagnostics kiterjesztéssel 1.5-ös vagy újabb hello tooother helyének részhalmaza.</span><span class="sxs-lookup"><span data-stu-id="d9326-106">However, you can also pipe all or a subset of hello data tooother locations using Azure Diagnostics extension 1.5 or later.</span></span>

<span data-ttu-id="d9326-107">Ez a cikk ismerteti, hogyan toosend adatait hello Azure Diagnostics bővítmény tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="d9326-107">This article describes how toosend data from hello Azure Diagnostics extension tooApplication Insights.</span></span>

## <a name="diagnostics-configuration-explained"></a><span data-ttu-id="d9326-108">Diagnosztikai konfiguráció ismertetése</span><span class="sxs-lookup"><span data-stu-id="d9326-108">Diagnostics configuration explained</span></span>
<span data-ttu-id="d9326-109">hello Azure diagnostics bővítmény 1.5-ös bevezetett felül, amelyek további helyre, ahol a diagnosztikai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="d9326-109">hello Azure diagnostics extension 1.5 introduced sinks, which are additional locations where you can send diagnostic data.</span></span>

<span data-ttu-id="d9326-110">Példa az konfigurálását, a fogadó Application Insights:</span><span class="sxs-lookup"><span data-stu-id="d9326-110">Example configuration of a sink for Application Insights:</span></span>

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
- <span data-ttu-id="d9326-111">Hello **fogadó** *neve* attribútum, amely egyedileg azonosítja a hello fogadó karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="d9326-111">hello **Sink** *name* attribute is a string value that uniquely identifies hello sink.</span></span>

- <span data-ttu-id="d9326-112">Hello **ApplicationInsights** elem megadja az Application insights-erőforrás ahol hello Azure diagnosztikai adatok küldése hello instrumentation kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d9326-112">hello **ApplicationInsights** element specifies instrumentation key of hello Application insights resource where hello Azure diagnostics data is sent.</span></span>
    - <span data-ttu-id="d9326-113">Ha még nem rendelkezik egy meglévő Application Insights-erőforrást, lásd: [hozzon létre egy új Application Insights-erőforrást](../application-insights/app-insights-create-new-resource.md) hello instrumentation kulcs erőforrás létrehozása és a további tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="d9326-113">If you don't have an existing Application Insights resource, see [Create a new Application Insights resource](../application-insights/app-insights-create-new-resource.md) for more information on creating a resource and getting hello instrumentation key.</span></span>
    - <span data-ttu-id="d9326-114">Ha egy felhőalapú szolgáltatást biztosít az Azure SDK 2.8 fejleszt, a instrumentation kulcs automatikusan feltöltődik értékkel.</span><span class="sxs-lookup"><span data-stu-id="d9326-114">If you are developing a Cloud Service with Azure SDK 2.8 and later, this instrumentation key is automatically populated.</span></span> <span data-ttu-id="d9326-115">hello érték alapul hello **APPINSIGHTS_INSTRUMENTATIONKEY** szolgáltatás konfigurációs beállítás, amikor csomagolására hello Cloud Service-projektet.</span><span class="sxs-lookup"><span data-stu-id="d9326-115">hello value is based on hello **APPINSIGHTS_INSTRUMENTATIONKEY** service configuration setting when packaging hello Cloud Service project.</span></span> <span data-ttu-id="d9326-116">Lásd: [használja az Application Insights az Azure Diagnostics tootroubleshoot Felhőszolgáltatás problémák](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span><span class="sxs-lookup"><span data-stu-id="d9326-116">See [Use Application Insights with Azure Diagnostics tootroubleshoot Cloud Service issues](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span></span>

- <span data-ttu-id="d9326-117">Hello **csatornák** egy vagy több elemet tartalmaz **csatorna** elemek.</span><span class="sxs-lookup"><span data-stu-id="d9326-117">hello **Channels** element contains one or more **Channel** elements.</span></span>
    - <span data-ttu-id="d9326-118">Hello *neve* attribútum egyedileg toothat csatorna hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="d9326-118">hello *name* attribute uniquely refers toothat channel.</span></span>
    - <span data-ttu-id="d9326-119">Hello *loglevel* attribútum Megadja, hogy a csatorna hello hello naplózási szint lehetővé teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="d9326-119">hello *loglevel* attribute lets you specify hello log level that hello channel allows.</span></span> <span data-ttu-id="d9326-120">a legtöbb tooleast információt sorrendje hello elérhető naplózási szintek a következők:</span><span class="sxs-lookup"><span data-stu-id="d9326-120">hello available log levels in order of most tooleast information are:</span></span>
        - <span data-ttu-id="d9326-121">Részletes</span><span class="sxs-lookup"><span data-stu-id="d9326-121">Verbose</span></span>
        - <span data-ttu-id="d9326-122">Információ</span><span class="sxs-lookup"><span data-stu-id="d9326-122">Information</span></span>
        - <span data-ttu-id="d9326-123">Figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="d9326-123">Warning</span></span>
        - <span data-ttu-id="d9326-124">Hiba</span><span class="sxs-lookup"><span data-stu-id="d9326-124">Error</span></span>
        - <span data-ttu-id="d9326-125">Kritikus</span><span class="sxs-lookup"><span data-stu-id="d9326-125">Critical</span></span>

<span data-ttu-id="d9326-126">Egy csatorna úgy viselkedik, mint egy szűrőt, és lehetővé teszi tooselect adott naplózási szintek toosend toohello cél fogadó.</span><span class="sxs-lookup"><span data-stu-id="d9326-126">A channel acts like a filter and allows you tooselect specific log levels toosend toohello target sink.</span></span> <span data-ttu-id="d9326-127">Például akkor sikerült részletes gyűjtését és elküldhesse az összeset toostorage, de csak a hibák toohello gyűjtő küldés.</span><span class="sxs-lookup"><span data-stu-id="d9326-127">For example, you could collect verbose logs and send them toostorage, but send only Errors toohello sink.</span></span>

<span data-ttu-id="d9326-128">hello következő ábra ezt a kapcsolatot mutatja be.</span><span class="sxs-lookup"><span data-stu-id="d9326-128">hello following graphic shows this relationship.</span></span>

![Diagnosztikai nyilvános konfigurációja](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

<span data-ttu-id="d9326-130">a következő ábra hello hello konfigurációs érték és leírás foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="d9326-130">hello following graphic summarizes hello configuration values and how they work.</span></span> <span data-ttu-id="d9326-131">Több mosdók hello konfigurációs hello hierarchia különböző szintjein is felvehet.</span><span class="sxs-lookup"><span data-stu-id="d9326-131">You can include multiple sinks in hello configuration at different levels in hello hierarchy.</span></span> <span data-ttu-id="d9326-132">hello felső szintjén hello fogadó úgy működik, mint a globális beállítás és hello egy meghatározott, egyéni hello elem úgy viselkedik, mint egy felülbírálás toothat globális beállítás.</span><span class="sxs-lookup"><span data-stu-id="d9326-132">hello sink at hello top level acts as a global setting and hello one specified at hello individual element acts like an override toothat global setting.</span></span>

![Diagnosztika fogadók esetében az Application Insights-konfiguráció](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a><span data-ttu-id="d9326-134">Teljes fogadó konfigurációja – példa</span><span class="sxs-lookup"><span data-stu-id="d9326-134">Complete sink configuration example</span></span>
<span data-ttu-id="d9326-135">Ez egy teljes példa hello nyilvános konfigurációs fájlba</span><span class="sxs-lookup"><span data-stu-id="d9326-135">Here is a complete example of hello public configuration file that</span></span>
1. <span data-ttu-id="d9326-136">elküldi az összes hiba tooApplication Insights (hello megadott **DiagnosticMonitorConfiguration** csomópont)</span><span class="sxs-lookup"><span data-stu-id="d9326-136">sends all errors tooApplication Insights (specified at hello **DiagnosticMonitorConfiguration** node)</span></span>
2. <span data-ttu-id="d9326-137">Részletes szintű alkalmazásnaplók hello naplókat is küld (hello megadott **naplók** csomópont).</span><span class="sxs-lookup"><span data-stu-id="d9326-137">also sends Verbose level logs for hello Application Logs (specified at hello **Logs** node).</span></span>

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
<span data-ttu-id="d9326-138">Hello előző konfigurációban az alábbi hello rendelkezik hello jelentése a következő:</span><span class="sxs-lookup"><span data-stu-id="d9326-138">In hello previous configuration, hello following lines have hello following meanings:</span></span>

### <a name="send-all-hello-data-that-is-being-collected-by-azure-diagnostics"></a><span data-ttu-id="d9326-139">Hello Azure diagnostics által gyűjtött összes adat küldése</span><span class="sxs-lookup"><span data-stu-id="d9326-139">Send all hello data that is being collected by Azure diagnostics</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-toohello-application-insights-sink"></a><span data-ttu-id="d9326-140">Csak a hiba naplók toohello Application Insights gyűjtő küldés</span><span class="sxs-lookup"><span data-stu-id="d9326-140">Send only error logs toohello Application Insights sink</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-tooapplication-insights"></a><span data-ttu-id="d9326-141">Részletes alkalmazásnaplók tooApplication Insights küldése</span><span class="sxs-lookup"><span data-stu-id="d9326-141">Send Verbose application logs tooApplication Insights</span></span>

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a><span data-ttu-id="d9326-142">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="d9326-142">Limitations</span></span>

- <span data-ttu-id="d9326-143">**Csatornák csak típusa és a teljesítmény nem számlálók naplózásához.**</span><span class="sxs-lookup"><span data-stu-id="d9326-143">**Channels only log type and not performance counters.**</span></span> <span data-ttu-id="d9326-144">Ha egy csatornát a teljesítmény számláló elemet adja meg, figyelmen kívül.</span><span class="sxs-lookup"><span data-stu-id="d9326-144">If you specify a channel with a performance counter element, it is ignored.</span></span>
- <span data-ttu-id="d9326-145">**egy csatorna hello naplózási szint nem lehet hosszabb hello naplózási szint esetében mi az Azure diagnostics gyűjtött.**</span><span class="sxs-lookup"><span data-stu-id="d9326-145">**hello log level for a channel cannot exceed hello log level for what is being collected by Azure diagnostics.**</span></span> <span data-ttu-id="d9326-146">Például nem hello naplók elem alkalmazásnaplóban hibáinak gyűjtése, és próbálja toosend részletes naplókat toohello Application Insights fogadó.</span><span class="sxs-lookup"><span data-stu-id="d9326-146">For example, you cannot collect Application Log errors in hello Logs element and try toosend Verbose logs toohello Application Insight sink.</span></span> <span data-ttu-id="d9326-147">Hello *scheduledTransferLogLevelFilter* attribútum mindig gyűjtése egyenlő vagy mint hello naplózza, akkor további naplók próbált toosend tooa fogadó.</span><span class="sxs-lookup"><span data-stu-id="d9326-147">hello *scheduledTransferLogLevelFilter* attribute must always collect equal or more logs than hello logs you are trying toosend tooa sink.</span></span>
- <span data-ttu-id="d9326-148">**Az Azure diagnostics bővítmény tooApplication Insights által gyűjtött blob adatokat nem lehet elküldeni.**</span><span class="sxs-lookup"><span data-stu-id="d9326-148">**You cannot send blob data collected by Azure diagnostics extension tooApplication Insights.**</span></span> <span data-ttu-id="d9326-149">Például semmit hello alatt megadott *könyvtárak* csomópont.</span><span class="sxs-lookup"><span data-stu-id="d9326-149">For example, anything specified under hello *Directories* node.</span></span> <span data-ttu-id="d9326-150">Összeomlási memóriaképek hello tényleges összeomlási memóriakép tooblob tárolási zajlik, és csak összeomlási memóriakép hello értesítést jött létre tooApplication Insights zajlik.</span><span class="sxs-lookup"><span data-stu-id="d9326-150">For Crash Dumps hello actual crash dump is sent tooblob storage and only a notification that hello crash dump was generated is sent tooApplication Insights.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9326-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9326-151">Next Steps</span></span>
* <span data-ttu-id="d9326-152">Megtudhatja, hogyan túl[tekintse meg az Azure diagnostics](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) az Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="d9326-152">Learn how too[view your Azure diagnostics information](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) in Application Insights.</span></span>
* <span data-ttu-id="d9326-153">Használjon [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) tooenable hello Azure diagnostics bővítményt az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d9326-153">Use [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) tooenable hello Azure diagnostics extension for your application.</span></span>
* <span data-ttu-id="d9326-154">Használjon [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) tooenable hello Azure diagnostics-bővítmény alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d9326-154">Use [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) tooenable hello Azure diagnostics extension for your application</span></span>
