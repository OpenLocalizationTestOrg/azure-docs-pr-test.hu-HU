---
title: "aaaUse Powershell tooset riasztások az Application Insights |} Microsoft Docs"
description: "Az Application Insights tooget e-mailekhez mérőszám változásaira vonatkozó automatizálásához."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: d68e5f9511bb4015f59175724bc1a4a04ecf43e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooset-alerts-in-application-insights"></a>Az Application Insightsban PowerShell tooset riasztások használata
Automatizálható a hello konfigurálása [riasztások](app-insights-alerts.md) a [Application Insights](app-insights-overview.md).

Emellett képes [webhookok tooautomate a válasz tooan figyelmeztetés beállítása](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

> [!NOTE]
> Ha azt szeretné, hogy toocreate erőforrások és a riasztások hello azonos időben, javasoljuk, hogy [Azure Resource Manager-sablonnal](app-insights-powershell.md).
>
>

## <a name="one-time-setup"></a>Egyszeri beállítása
Ha még nem használta PowerShell Azure-előfizetéséhez előtt:

Hello Azure Powershell modul telepítése toorun hello parancsfájlok, ahová hello gépen.

* Telepítés [Microsoft Webplatform-telepítővel (v5 vagy magasabb)](http://www.microsoft.com/web/downloads/platform.aspx).
* Használja a Microsoft Azure Powershell tooinstall

## <a name="connect-tooazure"></a>Csatlakozás tooAzure
Indítsa el az Azure PowerShell és [tooyour előfizetés csatlakozás](/powershell/azure/overview):

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a>Riasztásokat kaphat
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Riasztások hozzáadása
    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a>1. példa
E-mailt kérek hello kiszolgálói válasz tooHTTP kérelmek, átlagosan több mint 5 perc, ha alacsonyabb, mint 1 másodperc. Az Application Insights-erőforrás neve IceCreamWebApp, és a Fabrikam az erőforráscsoporthoz tartozik. Azure-előfizetés hello hello tulajdonosa vagyok.

hello GUID hello előfizetés-azonosító (nem hello instrumentation kulcs hello alkalmazás).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if hello server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>2. példa
Egy alkalmazás, amelyben használni kell [trackmetric() függvény](app-insights-api-custom-events-metrics.md#trackmetric) tooreport "salesPerHour." nevű metrika Küldjön egy e-mailt toomy, munkatársakat Ha "salesPerHour" 100, 24 órás átlagosan alá süllyed.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

ugyanaz a szabály használható hello metrika hello hello segítségével jelentett [mérési paraméter](app-insights-api-custom-events-metrics.md#properties) TrackEvent vagy trackPageView például egy másik nyomkövetési hívás.

## <a name="metric-names"></a>Metrika neve
| Metrika neve | Képernyő neve | Leírás |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Böngészőkivételek |Hello böngészőben fellépő nem kezelt kivételek száma. |
| `basicExceptionServer.count` |Kivételek |Hello alkalmazása által nem kezelt kivételek száma |
| `clientPerformance.clientProcess.value` |Ügyfél feldolgozási ideje |Egy dokumentum utolsó bájtját hello fogadását, amíg hello DOM betöltése között eltelt idő. Aszinkron kérelmek feldolgozása még folyamatban lehet is. |
| `clientPerformance.networkConnection.value` |Lapbetöltési hálózati csatlakozás ideje |Idő hello böngésző tooconnect toohello hálózati vesz igénybe. 0 akkor lehet, ha a gyorsítótárba. |
| `clientPerformance.receiveRequest.value` |A fogadó válaszidő |Kérelem toostarting tooreceive válasz küldését böngésző között eltelt idő. |
| `clientPerformance.sendRequest.value` |A küldési kérelem ideje |Böngésző toosend kérelem szükséges időt. |
| `clientPerformance.total.value` |Böngésző lapbetöltési ideje |Felhasználói kérelemtől a DOM, a stíluslapok, parancsfájlok és a képeket idő töltődnek be. |
| `performanceCounter.available_bytes.value` |Rendelkezésre álló memória |A folyamat vagy a rendszer általi használatra azonnal elérhető fizikai memória. |
| `performanceCounter.io_data_bytes_per_sec.value` |Feldolgozási IO sebessége |Összes bájt / második olvasási és írásbeli toofiles, hálózaton és eszközökön. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |Kivétel arány |Kivételek száma másodpercenként. |
| `performanceCounter.percentage_processor_time.value` |CPU folyamat |eltelt idő hello alkalmazások folyamat hello processzor tooexecution utasításokat által használt összes Folyamatszál hello hány százalékát. |
| `performanceCounter.percentage_processor_total.value` |Processzoridő |hello töltött idő százalékos aránya hello processzor nem üresjárati szálak futtatásával töltött. |
| `performanceCounter.process_private_bytes.value` |Folyamat saját bájtok |Folyamataihoz hozzárendelt memória toohello figyelt alkalmazás. |
| `performanceCounter.request_execution_time.value` |ASP.NET-kérelem végrehajtási ideje |Hello legutóbbi kérelem végrehajtási ideje. |
| `performanceCounter.requests_in_application_queue.value` |ASP.NET-kérelem végrehajtásának várólista |Hello alkalmazás kérelem-várólistájának hossza. |
| `performanceCounter.requests_per_sec.value` |ASP.NET-kérelmek gyakorisága |Az ASP.NET toohello alkalmazás másodpercenként kérelmek száma másodpercenként, az összes. |
| `remoteDependencyFailed.durationMetric.count` |Függőségi hiba |Hello server alkalmazás tooexternal erőforrások felé indított sikertelen hívások száma. |
| `request.duration` |Kiszolgáló válaszideje |Egy HTTP-kérelem fogadása és hello válasz küldésének befejezése között eltelt idő. |
| `request.rate` |Kérelmek gyakorisága |Az összes kérelmek / másodperc toohello alkalmazás. |
| `requestFailed.count` |Sikertelen kérelmek |HTTP-kérelmek száma válaszkódot eredményező > = 400 |
| `view.count` |Lapmegtekintések |A weblap ügyfélfelhasználói kéréseinek száma. Szintetikus forgalom ki van szűrve. |
| {az egyéni metrika neve} |{A metrika neve} |A metrika értékét által jelentett [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) vagy hello [mérések paraméter követési hívásának](app-insights-api-custom-events-metrics.md#properties). |

különféle telemetriai modulok által küldött hello metrikák:

| Metrika csoport | Adatgyűjtő modulja |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>megtekintés |[Böngésző JavaScript](app-insights-javascript.md) |
| performanceCounter |[Teljesítmény](app-insights-configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Függőség](app-insights-configuration-with-applicationinsights-config.md) |
| kérelem,<br/>requestFailed |[Kiszolgálói kérelem](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>webhook
Is [automatizálhatja a válasz tooan riasztás](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Riasztást hoz létre Azure egy webcímet az Ön által választott fog hívni.

## <a name="see-also"></a>Lásd még:
* [Az Application Insights parancsfájl tooconfigure](app-insights-powershell-script-create-resource.md)
* [Az Application Insights és webes teszt erőforrások létrehozása sablonból](app-insights-powershell.md)
* [A Microsoft Azure Diagnostics tooApplication Insights kapcsoló automatizálásához](app-insights-powershell-azure-diagnostics.md)
* [A válasz tooan riasztás automatizálásához](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
