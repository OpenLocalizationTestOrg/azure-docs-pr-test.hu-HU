---
title: "Service Fabric esemény összesítési rendelkező EventFlow aaaAzure |} Microsoft Docs"
description: "További tudnivalók összesítésére és események gyűjtése EventFlow figyelési és diagnosztika Azure Service Fabric-fürt segítségével."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: c0141d3ed72d835139250af3589e298fd22d8f89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-eventflow"></a>Esemény összevonásának és a gyűjtemény EventFlow használatával

[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) irányíthatja a csomópont tooone vagy további figyelési célok származó események. Mivel a NuGet-csomagot a projekt része, EventFlow kód és a konfiguráció változatlan marad hello szolgáltatást, a korábban említett Azure Diagnostics kapcsolatos hello csomópontonként konfigurációs probléma kiküszöbölése. EventFlow belül a szolgáltatási folyamat fut, és a konfigurált toohello kimenetek közvetlenül csatlakozik. Hello közvetlen kapcsolat, mert EventFlow használható Azure, a tároló és a helyszíni-szolgáltatási telepítéseket. Ügyeljen arra, ha futtatja EventFlow nagy sűrűségű helyzetekben, például a tárolóban lévő minden EventFlow folyamat ugyanis egy külső kapcsolatot. Így, ha több folyamatok működteti, több kimenő kapcsolatok! Ez nem mértékű problémát jelent a Service Fabric-alkalmazások esetében, mert az összes replika egy `ServiceType` futtassa a hello ugyanezt a folyamatot, és a kimenő kapcsolatok hello számának korlátozása. EventFlow is kínál események szűrése, így csak a hello megadott szűrőnek megfelelő hello események kerülnek.

## <a name="setting-up-eventflow"></a>EventFlow beállítása

EventFlow bináris NuGet-csomagok készletként érhetők el. tooadd EventFlow tooa Service Fabric-projekt hello projektre a Solution Explorer hello gombbal, és válassza a "Manage NuGet packages". Váltás toohello "Tallózás" fülre, és keressen a "`Diagnostics.EventFlow`":

![A Visual Studio NuGet-Csomagkezelő felhasználói felület EventFlow NuGet-csomagok](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

Látni fogja a különböző csomagok listája jelenik meg, az "A bemeneti" és "Kimeneti" címkével ellátott. EventFlow különböző különböző naplózási szolgáltatók és elemzőkkel támogatja. hello szolgáltatást üzemeltető EventFlow attól függően, hogy hello forrása és célja hello alkalmazásnaplók megfelelő csomagokat kell tartalmaznia. Ezenkívül toohello core ServiceFabric csomag is meg kell legalább egy bemeneti és kimeneti konfigurálva. Exmaple adhat hozzá a következő csomagok toosent EventSource események tooApplication Insights hello:

* `Microsoft.Diagnostics.EventFlow.Input.EventSource`hello szolgáltatást az EventSource osztályból származik, illetve a szabványos EventSources például toocapture adatok *Microsoft-ServiceFabric-szolgáltatások* és *Microsoft-ServiceFabric-szereplője*)
* `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`(fogjuk toosend hello naplók tooan Azure Application Insights-erőforrás)
* `Microsoft.Diagnostics.EventFlow.ServiceFabric`(lehetővé teszi, hogy a Service Fabric szolgáltatáskonfiguráció hello EventFlow folyamat inicializálása és jelentéseket, a Service Fabric állapotjelentések diagnosztikai adatok küldésének problémákat)

>[!NOTE]
>`Microsoft.Diagnostics.EventFlow.Input.EventSource`csomag igényli-e hello szolgáltatás projekt tootarget .NET keretrendszer 4.6-os vagy újabb. Ellenőrizze, hogy beállította hello megfelelő célkeretrendszer a projekt tulajdonságait a csomag telepítése előtt.

Ha minden hello csomagok telepítve van, a hello a következő lépés tooconfigure és EventFlow engedélyezése hello szolgáltatásban.

## <a name="configuring-and-enabling-log-collection"></a>Napló gyűjtésének engedélyezése és konfigurálása
a konfigurációs fájlban tárolt specifikáció hello EventFlow feldolgozási folyamat felelős hello naplók küldése készült. Hello `Microsoft.Diagnostics.EventFlow.ServiceFabric` a csomag telepíti a kiindulási EventFlow konfigurációs fájl `PackageRoot\Config` nevű megoldás mappa `eventFlowConfig.json`. A konfigurációs fájlnak kell hello alapértelmezett szolgáltatás adatait módosított toobe toocapture `EventSource` osztály, és bármely más bemeneti adatokat szeretné, hogy tooconfigure, és az adatok toohello megfelelő helyre küldeni.

Íme egy minta *eventFlowConfig.json* a fent említett hello NuGet-csomagok alapján:
```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace hello following value with your service's ServiceEventSource name)
        { "providerName": "your-service-EventSource-name" }
      ]
    }
  ],
  "filters": [
    {
      "type": "drop",
      "include": "Level == Verbose"
    }
  ],
  "outputs": [
    {
      "type": "ApplicationInsights",
      // (replace hello following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

szolgáltatás ServiceEventSource hello neve nem hello hello név tulajdonságának értéke hello `EventSourceAttribute` toohello ServiceEventSource osztály alkalmazza. Az összes megadott hello `ServiceEventSource.cs` fájlt, amely hello szolgáltatást kód része. Például a hello hello ServiceEventSource következő kód részlet hello neve nem *értéket-Alkalmaz1-Stateless1*:

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

Vegye figyelembe, hogy `eventFlowConfig.json` fájl szolgáltatás konfigurációs csomag része. Módosítások toothis fájl tartalmazhat hello szolgáltatás teljes vagy konfiguráció-csak frissítések, tulajdonos tooService háló frissítési állapot-ellenőrzési eredményeire és automatikus visszaállítási sikertelen frissítése esetén. További információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).

Hello *szűrők* hello konfigurációs szakasza lehetővé teszi a toofurther hello információkat, amelyet toogo keresztül hello EventFlow csővezeték toohello kimenetek, hogy lehetővé teszi a toodrop testreszabása vagy bizonyos információkat tartalmaznak, illetve hello módosítása hello eseményadatok szerkezete. A szűrés további információkért lásd: [EventFlow szűrők](https://github.com/Azure/diagnostics-eventflow#filters).

hello utolsó lépés a szolgáltatás indítási kódban található tooinstantiate EventFlow csővezeték `Program.cs` fájlt:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric;
using Microsoft.ServiceFabric.Services.Runtime;

// **** EventFlow namespace
using Microsoft.Diagnostics.EventFlow.ServiceFabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is hello entry point of hello service host process.
        /// </summary>
        private static void Main()
        {
            try
            {
                // **** Instantiate log collection via EventFlow
                using (var diagnosticsPipeline = ServiceFabricDiagnosticPipelineFactory.CreatePipeline("MyApplication-MyService-DiagnosticsPipeline"))
                {

                    ServiceRuntime.RegisterServiceAsync("Stateless1Type",
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                    Thread.Sleep(Timeout.Infinite);
                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
                throw;
            }
        }
    }
}
```

a hello hello paraméterként átadott hello neve `CreatePipeline` hello metódusában `ServiceFabricDiagnosticsPipelineFactory` hello hello neve *állapotfigyelő entitás* képviselő hello EventFlow napló adatgyűjtési folyamat. Ezt a nevet használja, ha a EventFlow észlel, és hiba és jelenti a Service Fabric állapotfigyelő alrendszer hello keresztül.

### <a name="using-service-fabric-settings-and-application-parameters-tooin-eventflowconfig"></a>A Service Fabric-beállítások és az alkalmazás paraméterek tooin eventFlowConfig használatával

EventFlow támogatja a Service Fabric-beállítások és alkalmazások paremeters tooconfigure EventFlow beállításainak használatával. Olvassa el a tooService háló beállítások paraméterek értékek ezen különleges szintaxis használatával:

```json
servicefabric:/<section-name>/<setting-name>
``` 

`<section-name>`a Service Fabric konfigurációs szakaszban hello hello neve és `<setting-name>` hello konfigurációs beállítás hello érték, amely lesz használt tooconfigure egy EventFlow beállítást biztosít. További kapcsolatos tooread toodo, lépjen túl[Service Fabric-beállítás- és alkalmazás támogatása](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).

## <a name="verification"></a>Ellenőrzés

Indítsa el a szolgáltatást, és tekintse meg a hibakeresési hello Visual Studio kimeneti ablakában. Hello szolgáltatást az elindítása után jelenítse meg az megbizonyosodhat róla, hogy a szolgáltatás küld toohello kimeneti konfigurált rögzíti. Nyissa meg a tooyour esemény elemzése és a képi megjelenítés platformot, és győződjön meg arról, hogy a naplók van elindítva tooshow fel (eltarthat néhány percig).

## <a name="next-steps"></a>Következő lépések

* [Esemény elemzése és a képi megjelenítés az Application insights szolgáltatással](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Esemény elemzése és az OMS képi megjelenítés](service-fabric-diagnostics-event-analysis-oms.md)
* [EventFlow dokumentáció](https://github.com/Azure/diagnostics-eventflow)