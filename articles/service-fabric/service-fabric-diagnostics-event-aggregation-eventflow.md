---
title: "Az Azure Service Fabric esemény összesítési rendelkező EventFlow |} Microsoft Docs"
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
ms.openlocfilehash: 90d26a77b749e70de3a7d910f15820653e2ef39b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="event-aggregation-and-collection-using-eventflow"></a><span data-ttu-id="98900-103">Esemény összevonásának és a gyűjtemény EventFlow használatával</span><span class="sxs-lookup"><span data-stu-id="98900-103">Event aggregation and collection using EventFlow</span></span>

<span data-ttu-id="98900-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) irányíthatja a események csomópont egy vagy több figyelési célhelyre.</span><span class="sxs-lookup"><span data-stu-id="98900-104">[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) can route events from a node to one or more monitoring destinations.</span></span> <span data-ttu-id="98900-105">Mivel a NuGet-csomagot a projekt része, EventFlow kód és a konfigurációs változatlan marad a szolgáltatás, így kiküszöböli a fentebb említett Azure Diagnostics kapcsolatos csomópontonként konfigurációs probléma.</span><span class="sxs-lookup"><span data-stu-id="98900-105">Because it is included as a NuGet package in your service project, EventFlow code and configuration travel with the service, eliminating the per-node configuration issue mentioned earlier about Azure Diagnostics.</span></span> <span data-ttu-id="98900-106">EventFlow belül a szolgáltatási folyamat fut, és a beállított kimenetek közvetlenül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="98900-106">EventFlow runs within your service process, and connects directly to the configured outputs.</span></span> <span data-ttu-id="98900-107">A közvetlen kapcsolat miatt EventFlow használható Azure, a tároló és a helyszíni-szolgáltatási telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="98900-107">Because of the direct connection, EventFlow works for Azure, container, and on-premises service deployments.</span></span> <span data-ttu-id="98900-108">Ügyeljen arra, ha futtatja EventFlow nagy sűrűségű helyzetekben, például a tárolóban lévő minden EventFlow folyamat ugyanis egy külső kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="98900-108">Be careful if you run EventFlow in high-density scenarios, such as in a container, because each EventFlow pipeline makes an external connection.</span></span> <span data-ttu-id="98900-109">Így, ha több folyamatok működteti, több kimenő kapcsolatok!</span><span class="sxs-lookup"><span data-stu-id="98900-109">So, if you host several processes, you get several outbound connections!</span></span> <span data-ttu-id="98900-110">Ez nem mértékű problémát jelent a Service Fabric-alkalmazások esetében, mert az összes replika egy `ServiceType` futtatja ugyanabban a folyamatban, és a kimenő kapcsolatok számának korlátozása.</span><span class="sxs-lookup"><span data-stu-id="98900-110">This isn't as much a concern for Service Fabric applications, because all replicas of a `ServiceType` run in the same process, and this limits the number of outbound connections.</span></span> <span data-ttu-id="98900-111">EventFlow is kínál események szűrése, így csak a megadott szűrőnek megfelelő események kerülnek.</span><span class="sxs-lookup"><span data-stu-id="98900-111">EventFlow also offers event filtering, so that only the events that match the specified filter are sent.</span></span>

## <a name="setting-up-eventflow"></a><span data-ttu-id="98900-112">EventFlow beállítása</span><span class="sxs-lookup"><span data-stu-id="98900-112">Setting up EventFlow</span></span>

<span data-ttu-id="98900-113">EventFlow bináris NuGet-csomagok készletként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="98900-113">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="98900-114">Service Fabric-szolgáltatás projektbe EventFlow hozzáadásához kattintson a jobb gombbal a projektre a Megoldáskezelőben, és válassza a "Manage NuGet packages".</span><span class="sxs-lookup"><span data-stu-id="98900-114">To add EventFlow to a Service Fabric service project, right-click the project in the Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="98900-115">Váltson át a "Tallózás" fülre, és keresse meg "`Diagnostics.EventFlow`":</span><span class="sxs-lookup"><span data-stu-id="98900-115">Switch to the "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![A Visual Studio NuGet-Csomagkezelő felhasználói felület EventFlow NuGet-csomagok](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

<span data-ttu-id="98900-117">Látni fogja a különböző csomagok listája jelenik meg, az "A bemeneti" és "Kimeneti" címkével ellátott.</span><span class="sxs-lookup"><span data-stu-id="98900-117">You will see a list of various packages show up, labeled with "Inputs" and "Outputs".</span></span> <span data-ttu-id="98900-118">EventFlow különböző különböző naplózási szolgáltatók és elemzőkkel támogatja.</span><span class="sxs-lookup"><span data-stu-id="98900-118">EventFlow supports various different logging providers and analyzers.</span></span> <span data-ttu-id="98900-119">A szolgáltatásüzemeltetési EventFlow attól függően, hogy a forrás- és az alkalmazásnaplókat a megfelelő csomagokat kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="98900-119">The service hosting EventFlow should include appropriate packages depending on the source and destination for the application logs.</span></span> <span data-ttu-id="98900-120">A core ServiceFabric csomag mellett is szükség van legalább egy bemeneti és kimeneti konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="98900-120">In addition to the core ServiceFabric package, you also need at least one Input and Output configured.</span></span> <span data-ttu-id="98900-121">A exmaple a következő csomagok adhat hozzá Application insights szolgáltatásnak elküldött EventSource események:</span><span class="sxs-lookup"><span data-stu-id="98900-121">For exmaple, you can add the following packages to sent EventSource events to Application Insights:</span></span>

* <span data-ttu-id="98900-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource`a szolgáltatás az EventSource osztályból származik, és a szabványos EventSources adatok rögzítéséhez *Microsoft-ServiceFabric-szolgáltatások* és *Microsoft-ServiceFabric-szereplője*)</span><span class="sxs-lookup"><span data-stu-id="98900-122">`Microsoft.Diagnostics.EventFlow.Input.EventSource` to capture data from the service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* <span data-ttu-id="98900-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`(fogjuk küldeni a naplókat az Azure Application Insights-erőforrás)</span><span class="sxs-lookup"><span data-stu-id="98900-123">`Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights` (we are going to send the logs to an Azure Application Insights resource)</span></span>
* <span data-ttu-id="98900-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(lehetővé teszi, hogy a Service Fabric szolgáltatáskonfiguráció EventFlow folyamat inicializálása és jelentéseket, a Service Fabric állapotjelentések diagnosztikai adatok küldésének problémákat)</span><span class="sxs-lookup"><span data-stu-id="98900-124">`Microsoft.Diagnostics.EventFlow.ServiceFabric`(enables initialization of the EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

>[!NOTE]
><span data-ttu-id="98900-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource`csomag igényli-e a projekt, amelyekre a .NET-keretrendszer 4.6-os vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="98900-125">`Microsoft.Diagnostics.EventFlow.Input.EventSource` package requires the service project to target .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="98900-126">Ellenőrizze, hogy beállította a megfelelő célkeretrendszer a projekt tulajdonságait a csomag telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="98900-126">Make sure you set the appropriate target framework in project properties before installing this package.</span></span>

<span data-ttu-id="98900-127">A csomagok telepítése után a következő lépés, hogy konfigurálja és EventFlow engedélyezése a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="98900-127">After all the packages are installed, the next step is to configure and enable EventFlow in the service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="98900-128">Napló gyűjtésének engedélyezése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="98900-128">Configuring and enabling log collection</span></span>
<span data-ttu-id="98900-129">A EventFlow-feldolgozási folyamat felelős a naplókat küld a konfigurációs fájlban tárolt specifikáció készült.</span><span class="sxs-lookup"><span data-stu-id="98900-129">The EventFlow pipeline responsible for sending the logs is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="98900-130">A `Microsoft.Diagnostics.EventFlow.ServiceFabric` a csomag telepíti a kiindulási EventFlow konfigurációs fájl `PackageRoot\Config` nevű megoldás mappa `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="98900-130">The `Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder, named `eventFlowConfig.json`.</span></span> <span data-ttu-id="98900-131">A konfigurációs fájlnak kell módosítani kell az alapértelmezett szolgáltatás adatait `EventSource` osztály, és bármely más bemeneti adatokat szeretné konfigurálni, és küldhetnek adatokat a megfelelő helyre.</span><span class="sxs-lookup"><span data-stu-id="98900-131">This configuration file needs to be modified to capture data from the default service `EventSource` class, and any other inputs you want to configure, and send data to the appropriate place.</span></span>

<span data-ttu-id="98900-132">Íme egy minta *eventFlowConfig.json* a fent említett NuGet-csomagok alapján:</span><span class="sxs-lookup"><span data-stu-id="98900-132">Here is a sample *eventFlowConfig.json* based on the NuGet packages mentioned above:</span></span>
```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace the following value with your service's ServiceEventSource name)
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
      // (replace the following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

<span data-ttu-id="98900-133">Szolgáltatás ServiceEventSource neve nem a név tulajdonságának értéke a `EventSourceAttribute` ServiceEventSource osztály.</span><span class="sxs-lookup"><span data-stu-id="98900-133">The name of service's ServiceEventSource is the value of the Name property of the `EventSourceAttribute` applied to the ServiceEventSource class.</span></span> <span data-ttu-id="98900-134">Az összes megadott a `ServiceEventSource.cs` fájl, amely a szolgáltatáskód hibáit része.</span><span class="sxs-lookup"><span data-stu-id="98900-134">It is all specified in the `ServiceEventSource.cs` file, which is part of the service code.</span></span> <span data-ttu-id="98900-135">Például a következő kódrészletet a neve, a ServiceEventSource: *értéket-Alkalmaz1-Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="98900-135">For example, in the following code snippet the name of the ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

<span data-ttu-id="98900-136">Vegye figyelembe, hogy `eventFlowConfig.json` fájl szolgáltatás konfigurációs csomag része.</span><span class="sxs-lookup"><span data-stu-id="98900-136">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="98900-137">A fájl módosításait a szolgáltatás a Service Fabric frissítési állapot-ellenőrzési eredményeire és automatikus visszaállítási, ha a frissítés nem sikerülne teljes vagy konfiguráció-csak frissítés tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="98900-137">Changes to this file can be included in full- or configuration-only upgrades of the service, subject to Service Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="98900-138">További információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="98900-138">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="98900-139">A *szűrők* konfigurációs szakasza lehetővé teszi további, amelyet szeretne lépjen a EventFlow-feldolgozási folyamaton keresztül a kimenetek, lehetővé téve dobja el, és bizonyos információval adatok személyre szabására, vagy módosítsa a szerkezete a eseményadatok.</span><span class="sxs-lookup"><span data-stu-id="98900-139">The *filters* section of the config allows you to further customize the information that is going to go through the EventFlow pipeline to the outputs, allowing you to drop or include certain information, or change the structure of the event data.</span></span> <span data-ttu-id="98900-140">A szűrés további információkért lásd: [EventFlow szűrők](https://github.com/Azure/diagnostics-eventflow#filters).</span><span class="sxs-lookup"><span data-stu-id="98900-140">For more information on filtering, see [EventFlow filters](https://github.com/Azure/diagnostics-eventflow#filters).</span></span>

<span data-ttu-id="98900-141">Az utolsó lépés, hogy a szolgáltatás indítási kódban található EventFlow csővezeték példányosítható `Program.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="98900-141">The final step is to instantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file:</span></span>

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
        /// This is the entry point of the service host process.
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

<span data-ttu-id="98900-142">Nevét paraméterként, a `CreatePipeline` metódusában a `ServiceFabricDiagnosticsPipelineFactory` neve a *állapotfigyelő entitás* képviselő a EventFlow napló adatgyűjtési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="98900-142">The name passed as the parameter of the `CreatePipeline` method of the `ServiceFabricDiagnosticsPipelineFactory` is the name of the *health entity* representing the EventFlow log collection pipeline.</span></span> <span data-ttu-id="98900-143">Ezt a nevet használja, ha a EventFlow észlel, és hiba és jelenti a a Service Fabric állapotfigyelő alrendszer keresztül.</span><span class="sxs-lookup"><span data-stu-id="98900-143">This name is used if EventFlow encounters and error and reports it through the Service Fabric health subsystem.</span></span>

### <a name="using-service-fabric-settings-and-application-parameters-to-in-eventflowconfig"></a><span data-ttu-id="98900-144">A Service Fabric-beállítások és az alkalmazás paramétereit eventFlowConfig része</span><span class="sxs-lookup"><span data-stu-id="98900-144">Using Service Fabric settings and application parameters to in eventFlowConfig</span></span>

<span data-ttu-id="98900-145">EventFlow támogatja a Service Fabric-beállítások és alkalmazások paremeters EventFlow beállítások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="98900-145">EventFlow supports using Service Fabric settings and application paremeters to configure EventFlow settings.</span></span> <span data-ttu-id="98900-146">Olvassa el a Service Fabric beállítások paraméterek értékek ezen különleges szintaxis használatával:</span><span class="sxs-lookup"><span data-stu-id="98900-146">You can refer to Service Fabric settings parameters using this special syntax for values:</span></span>

```json
servicefabric:/<section-name>/<setting-name>
``` 

<span data-ttu-id="98900-147">`<section-name>`a Service Fabric konfigurációs szakasz neve és `<setting-name>` adva egy EventFlow beállítás konfigurálásához használt konfigurációs beállítás.</span><span class="sxs-lookup"><span data-stu-id="98900-147">`<section-name>` is the name of the Service Fabric configuration section, and `<setting-name>` is the configuration setting providing the value that will be used to configure an EventFlow setting.</span></span> <span data-ttu-id="98900-148">Olvassa el a további információt ebben az esetben lépjen [Service Fabric-beállítás- és alkalmazás támogatása](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span><span class="sxs-lookup"><span data-stu-id="98900-148">To read more about how to do this, go to [Support for Service Fabric settings and application parameters](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).</span></span>

## <a name="verification"></a><span data-ttu-id="98900-149">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="98900-149">Verification</span></span>

<span data-ttu-id="98900-150">Indítsa el a szolgáltatást, és tekintse meg a hibakeresési Visual Studio kimeneti ablakában.</span><span class="sxs-lookup"><span data-stu-id="98900-150">Start your service and observe the Debug output window in Visual Studio.</span></span> <span data-ttu-id="98900-151">A szolgáltatás az elindítása után jelenítse meg az megbizonyosodhat róla, hogy a szolgáltatás rekordot küld a kimeneti konfigurált.</span><span class="sxs-lookup"><span data-stu-id="98900-151">After the service is started, you should start seeing evidence that your service is sending records to the output that you have configured.</span></span> <span data-ttu-id="98900-152">Nyissa meg az esemény elemzése és a képi megjelenítés platform, és győződjön meg arról, hogy a naplók megjelenítendő van elindítva (eltarthat néhány percig) fel.</span><span class="sxs-lookup"><span data-stu-id="98900-152">Navigate to your event analysis and visualization platform and confirm that logs have started to show up (could take a few minutes).</span></span>

## <a name="next-steps"></a><span data-ttu-id="98900-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98900-153">Next steps</span></span>

* [<span data-ttu-id="98900-154">Esemény elemzése és a képi megjelenítés az Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="98900-154">Event Analysis and Visualization with Application Insights</span></span>](service-fabric-diagnostics-event-analysis-appinsights.md)
* [<span data-ttu-id="98900-155">Esemény elemzése és az OMS képi megjelenítés</span><span class="sxs-lookup"><span data-stu-id="98900-155">Event Analysis and Visualization with OMS</span></span>](service-fabric-diagnostics-event-analysis-oms.md)
* [<span data-ttu-id="98900-156">EventFlow dokumentáció</span><span class="sxs-lookup"><span data-stu-id="98900-156">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)