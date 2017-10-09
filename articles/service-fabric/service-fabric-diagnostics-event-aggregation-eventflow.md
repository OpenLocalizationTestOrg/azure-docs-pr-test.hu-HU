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
# <a name="event-aggregation-and-collection-using-eventflow"></a><span data-ttu-id="7301d-103">Esemény összevonásának és a gyűjtemény EventFlow használatával</span><span class="sxs-lookup"><span data-stu-id="7301d-103">Event aggregation and collection using EventFlow</span></span>

<span data-ttu-id="7301d-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) irányíthatja a csomópont tooone vagy további figyelési célok származó események.</span><span class="sxs-lookup"><span data-stu-id="7301d-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) can route events from a node tooone or more monitoring destinations.</span></span> <span data-ttu-id="7301d-105">Mivel a NuGet-csomagot a projekt része, EventFlow kód és a konfiguráció változatlan marad hello szolgáltatást, a korábban említett Azure Diagnostics kapcsolatos hello csomópontonként konfigurációs probléma kiküszöbölése.</span><span class="sxs-lookup"><span data-stu-id="7301d-105">Because it is included as a NuGet package in your service project, EventFlow code and configuration travel with hello service, eliminating hello per-node configuration issue mentioned earlier about Azure Diagnostics.</span></span> <span data-ttu-id="7301d-106">EventFlow belül a szolgáltatási folyamat fut, és a konfigurált toohello kimenetek közvetlenül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="7301d-106">EventFlow runs within your service process, and connects directly toohello configured outputs.</span></span> <span data-ttu-id="7301d-107">Hello közvetlen kapcsolat, mert EventFlow használható Azure, a tároló és a helyszíni-szolgáltatási telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="7301d-107">Because of hello direct connection, EventFlow works for Azure, container, and on-premises service deployments.</span></span> <span data-ttu-id="7301d-108">Ügyeljen arra, ha futtatja EventFlow nagy sűrűségű helyzetekben, például a tárolóban lévő minden EventFlow folyamat ugyanis egy külső kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="7301d-108">Be careful if you run EventFlow in high-density scenarios, such as in a container, because each EventFlow pipeline makes an external connection.</span></span> <span data-ttu-id="7301d-109">Így, ha több folyamatok működteti, több kimenő kapcsolatok!</span><span class="sxs-lookup"><span data-stu-id="7301d-109">So, if you host several processes, you get several outbound connections!</span></span> <span data-ttu-id="7301d-110">Ez nem mértékű problémát jelent a Service Fabric-alkalmazások esetében, mert az összes replika egy `ServiceType` futtassa a hello ugyanezt a folyamatot, és a kimenő kapcsolatok hello számának korlátozása.</span><span class="sxs-lookup"><span data-stu-id="7301d-110">This isn't as much a concern for Service Fabric applications, because all replicas of a `ServiceType` run in hello same process, and this limits hello number of outbound connections.</span></span> <span data-ttu-id="7301d-111">EventFlow is kínál események szűrése, így csak a hello megadott szűrőnek megfelelő hello események kerülnek.</span><span class="sxs-lookup"><span data-stu-id="7301d-111">EventFlow also offers event filtering, so that only hello events that match hello specified filter are sent.</span></span>

## <a name="setting-up-eventflow"></a><span data-ttu-id="7301d-112">EventFlow beállítása</span><span class="sxs-lookup"><span data-stu-id="7301d-112">Setting up EventFlow</span></span>

<span data-ttu-id="7301d-113">EventFlow bináris NuGet-csomagok készletként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7301d-113">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="7301d-114">tooadd EventFlow tooa Service Fabric-projekt hello projektre a Solution Explorer hello gombbal, és válassza a "Manage NuGet packages".</span><span class="sxs-lookup"><span data-stu-id="7301d-114">tooadd EventFlow tooa Service Fabric service project, right-click hello project in hello Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="7301d-115">Váltás toohello "Tallózás" fülre, és keressen a "`Diagnostics.EventFlow`":</span><span class="sxs-lookup"><span data-stu-id="7301d-115">Switch toohello "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![A Visual Studio NuGet-Csomagkezelő felhasználói felület EventFlow NuGet-csomagok](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

<span data-ttu-id="7301d-117">Látni fogja a különböző csomagok listája jelenik meg, az "A bemeneti" és "Kimeneti" címkével ellátott.</span><span class="sxs-lookup"><span data-stu-id="7301d-117">You will see a list of various packages show up, labeled with "Inputs" and "Outputs".</span></span> <span data-ttu-id="7301d-118">EventFlow különböző különböző naplózási szolgáltatók és elemzőkkel támogatja.</span><span class="sxs-lookup"><span data-stu-id="7301d-118">EventFlow supports various different logging providers and analyzers.</span></span> <span data-ttu-id="7301d-119">hello szolgáltatást üzemeltető EventFlow attól függően, hogy hello forrása és célja hello alkalmazásnaplók megfelelő csomagokat kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="7301d-119">hello service hosting EventFlow should include appropriate packages depending on hello source and destination for hello application logs.</span></span> <span data-ttu-id="7301d-120">Ezenkívül toohello core ServiceFabric csomag is meg kell legalább egy bemeneti és kimeneti konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7301d-120">In addition toohello core ServiceFabric package, you also need at least one Input and Output configured.</span></span> <span data-ttu-id="7301d-121">Exmaple adhat hozzá a következő csomagok toosent EventSource események tooApplication Insights hello:</span><span class="sxs-lookup"><span data-stu-id="7301d-121">For exmaple, you can add hello following packages toosent EventSource events tooApplication Insights:</span></span>

* <span data-ttu-id="7301d-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource`hello szolgáltatást az EventSource osztályból származik, illetve a szabványos EventSources például toocapture adatok *Microsoft-ServiceFabric-szolgáltatások* és *Microsoft-ServiceFabric-szereplője*)</span><span class="sxs-lookup"><span data-stu-id="7301d-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource` toocapture data from hello service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* <span data-ttu-id="7301d-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`(fogjuk toosend hello naplók tooan Azure Application Insights-erőforrás)</span><span class="sxs-lookup"><span data-stu-id="7301d-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights` (we are going toosend hello logs tooan Azure Application Insights resource)</span></span>
* <span data-ttu-id="7301d-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(lehetővé teszi, hogy a Service Fabric szolgáltatáskonfiguráció hello EventFlow folyamat inicializálása és jelentéseket, a Service Fabric állapotjelentések diagnosztikai adatok küldésének problémákat)</span><span class="sxs-lookup"><span data-stu-id="7301d-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(enables initialization of hello EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

>[!NOTE]
><span data-ttu-id="7301d-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource`csomag igényli-e hello szolgáltatás projekt tootarget .NET keretrendszer 4.6-os vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="7301d-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource` package requires hello service project tootarget .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="7301d-126">Ellenőrizze, hogy beállította hello megfelelő célkeretrendszer a projekt tulajdonságait a csomag telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="7301d-126">Make sure you set hello appropriate target framework in project properties before installing this package.</span></span>

<span data-ttu-id="7301d-127">Ha minden hello csomagok telepítve van, a hello a következő lépés tooconfigure és EventFlow engedélyezése hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7301d-127">After all hello packages are installed, hello next step is tooconfigure and enable EventFlow in hello service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="7301d-128">Napló gyűjtésének engedélyezése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7301d-128">Configuring and enabling log collection</span></span>
<span data-ttu-id="7301d-129">a konfigurációs fájlban tárolt specifikáció hello EventFlow feldolgozási folyamat felelős hello naplók küldése készült.</span><span class="sxs-lookup"><span data-stu-id="7301d-129">hello EventFlow pipeline responsible for sending hello logs is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="7301d-130">Hello `Microsoft.Diagnostics.EventFlow.ServiceFabric` a csomag telepíti a kiindulási EventFlow konfigurációs fájl `PackageRoot\Config` nevű megoldás mappa `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="7301d-130">hello `Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder, named `eventFlowConfig.json`.</span></span> <span data-ttu-id="7301d-131">A konfigurációs fájlnak kell hello alapértelmezett szolgáltatás adatait módosított toobe toocapture `EventSource` osztály, és bármely más bemeneti adatokat szeretné, hogy tooconfigure, és az adatok toohello megfelelő helyre küldeni.</span><span class="sxs-lookup"><span data-stu-id="7301d-131">This configuration file needs toobe modified toocapture data from hello default service `EventSource` class, and any other inputs you want tooconfigure, and send data toohello appropriate place.</span></span>

<span data-ttu-id="7301d-132">Íme egy minta *eventFlowConfig.json* a fent említett hello NuGet-csomagok alapján:</span><span class="sxs-lookup"><span data-stu-id="7301d-132">Here is a sample *eventFlowConfig.json* based on hello NuGet packages mentioned above:</span></span>
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

<span data-ttu-id="7301d-133">szolgáltatás ServiceEventSource hello neve nem hello hello név tulajdonságának értéke hello `EventSourceAttribute` toohello ServiceEventSource osztály alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="7301d-133">hello name of service's ServiceEventSource is hello value of hello Name property of hello `EventSourceAttribute` applied toohello ServiceEventSource class.</span></span> <span data-ttu-id="7301d-134">Az összes megadott hello `ServiceEventSource.cs` fájlt, amely hello szolgáltatást kód része.</span><span class="sxs-lookup"><span data-stu-id="7301d-134">It is all specified in hello `ServiceEventSource.cs` file, which is part of hello service code.</span></span> <span data-ttu-id="7301d-135">Például a hello hello ServiceEventSource következő kód részlet hello neve nem *értéket-Alkalmaz1-Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="7301d-135">For example, in hello following code snippet hello name of hello ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

<span data-ttu-id="7301d-136">Vegye figyelembe, hogy `eventFlowConfig.json` fájl szolgáltatás konfigurációs csomag része.</span><span class="sxs-lookup"><span data-stu-id="7301d-136">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="7301d-137">Módosítások toothis fájl tartalmazhat hello szolgáltatás teljes vagy konfiguráció-csak frissítések, tulajdonos tooService háló frissítési állapot-ellenőrzési eredményeire és automatikus visszaállítási sikertelen frissítése esetén.</span><span class="sxs-lookup"><span data-stu-id="7301d-137">Changes toothis file can be included in full- or configuration-only upgrades of hello service, subject tooService Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="7301d-138">További információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="7301d-138">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="7301d-139">Hello *szűrők* hello konfigurációs szakasza lehetővé teszi a toofurther hello információkat, amelyet toogo keresztül hello EventFlow csővezeték toohello kimenetek, hogy lehetővé teszi a toodrop testreszabása vagy bizonyos információkat tartalmaznak, illetve hello módosítása hello eseményadatok szerkezete.</span><span class="sxs-lookup"><span data-stu-id="7301d-139">hello *filters* section of hello config allows you toofurther customize hello information that is going toogo through hello EventFlow pipeline toohello outputs, allowing you toodrop or include certain information, or change hello structure of hello event data.</span></span> <span data-ttu-id="7301d-140">A szűrés további információkért lásd: [EventFlow szűrők](https://github.com/Azure/diagnostics-eventflow#filters).</span><span class="sxs-lookup"><span data-stu-id="7301d-140">For more information on filtering, see [EventFlow filters](https://github.com/Azure/diagnostics-eventflow#filters).</span></span>

<span data-ttu-id="7301d-141">hello utolsó lépés a szolgáltatás indítási kódban található tooinstantiate EventFlow csővezeték `Program.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="7301d-141">hello final step is tooinstantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file:</span></span>

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

<span data-ttu-id="7301d-142">a hello hello paraméterként átadott hello neve `CreatePipeline` hello metódusában `ServiceFabricDiagnosticsPipelineFactory` hello hello neve *állapotfigyelő entitás* képviselő hello EventFlow napló adatgyűjtési folyamat.</span><span class="sxs-lookup"><span data-stu-id="7301d-142">hello name passed as hello parameter of hello `CreatePipeline` method of hello `ServiceFabricDiagnosticsPipelineFactory` is hello name of hello *health entity* representing hello EventFlow log collection pipeline.</span></span> <span data-ttu-id="7301d-143">Ezt a nevet használja, ha a EventFlow észlel, és hiba és jelenti a Service Fabric állapotfigyelő alrendszer hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="7301d-143">This name is used if EventFlow encounters and error and reports it through hello Service Fabric health subsystem.</span></span>

### <a name="using-service-fabric-settings-and-application-parameters-tooin-eventflowconfig"></a><span data-ttu-id="7301d-144">A Service Fabric-beállítások és az alkalmazás paraméterek tooin eventFlowConfig használatával</span><span class="sxs-lookup"><span data-stu-id="7301d-144">Using Service Fabric settings and application parameters tooin eventFlowConfig</span></span>

<span data-ttu-id="7301d-145">EventFlow támogatja a Service Fabric-beállítások és alkalmazások paremeters tooconfigure EventFlow beállításainak használatával.</span><span class="sxs-lookup"><span data-stu-id="7301d-145">EventFlow supports using Service Fabric settings and application paremeters tooconfigure EventFlow settings.</span></span> <span data-ttu-id="7301d-146">Olvassa el a tooService háló beállítások paraméterek értékek ezen különleges szintaxis használatával:</span><span class="sxs-lookup"><span data-stu-id="7301d-146">You can refer tooService Fabric settings parameters using this special syntax for values:</span></span>

```json
servicefabric:/<section-name>/<setting-name>
``` 

<span data-ttu-id="7301d-147">`<section-name>`a Service Fabric konfigurációs szakaszban hello hello neve és `<setting-name>` hello konfigurációs beállítás hello érték, amely lesz használt tooconfigure egy EventFlow beállítást biztosít.</span><span class="sxs-lookup"><span data-stu-id="7301d-147">`<section-name>` is hello name of hello Service Fabric configuration section, and `<setting-name>` is hello configuration setting providing hello value that will be used tooconfigure an EventFlow setting.</span></span> <span data-ttu-id="7301d-148">További kapcsolatos tooread toodo, lépjen túl[Service Fabric-beállítás- és alkalmazás támogatása](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span><span class="sxs-lookup"><span data-stu-id="7301d-148">tooread more about how toodo this, go too[Support for Service Fabric settings and application parameters](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span></span>

## <a name="verification"></a><span data-ttu-id="7301d-149">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="7301d-149">Verification</span></span>

<span data-ttu-id="7301d-150">Indítsa el a szolgáltatást, és tekintse meg a hibakeresési hello Visual Studio kimeneti ablakában.</span><span class="sxs-lookup"><span data-stu-id="7301d-150">Start your service and observe hello Debug output window in Visual Studio.</span></span> <span data-ttu-id="7301d-151">Hello szolgáltatást az elindítása után jelenítse meg az megbizonyosodhat róla, hogy a szolgáltatás küld toohello kimeneti konfigurált rögzíti.</span><span class="sxs-lookup"><span data-stu-id="7301d-151">After hello service is started, you should start seeing evidence that your service is sending records toohello output that you have configured.</span></span> <span data-ttu-id="7301d-152">Nyissa meg a tooyour esemény elemzése és a képi megjelenítés platformot, és győződjön meg arról, hogy a naplók van elindítva tooshow fel (eltarthat néhány percig).</span><span class="sxs-lookup"><span data-stu-id="7301d-152">Navigate tooyour event analysis and visualization platform and confirm that logs have started tooshow up (could take a few minutes).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7301d-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7301d-153">Next steps</span></span>

* [<span data-ttu-id="7301d-154">Esemény elemzése és a képi megjelenítés az Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="7301d-154">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="7301d-155">Esemény elemzése és az OMS képi megjelenítés</span><span class="sxs-lookup"><span data-stu-id="7301d-155">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)
* [<span data-ttu-id="7301d-156">EventFlow dokumentáció</span><span class="sxs-lookup"><span data-stu-id="7301d-156">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)