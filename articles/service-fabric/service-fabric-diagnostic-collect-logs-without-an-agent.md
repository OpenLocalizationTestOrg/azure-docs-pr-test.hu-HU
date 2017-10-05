---
title: "Gyűjteni a közvetlenül az Azure Service Fabric-szolgáltatás folyamatai |} A Microsoft Azure"
description: "A Service Fabric alkalmazások naplók küldésével közvetlenül egy központi helyen, például az Azure Application Insights vagy Elasticsearch, anélkül, hogy az Azure diagnosztikai ügynök ismerteti."
services: service-fabric
documentationcenter: .net
author: karolz-ms
manager: rwike77
editor: 
ms.assetid: ab92c99b-1edd-4677-8c28-4e591d909b47
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/18/2017
ms.author: karolz
redirect_url: /azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow
ms.openlocfilehash: b7d2541928f4248750417a77d99033c8b4354dcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a><span data-ttu-id="d7dba-103">Gyűjteni a közvetlenül az Azure Service Fabric-szolgáltatás folyamata</span><span class="sxs-lookup"><span data-stu-id="d7dba-103">Collect logs directly from an Azure Service Fabric service process</span></span>
## <a name="in-process-log-collection"></a><span data-ttu-id="d7dba-104">A folyamaton belüli naplógyűjtést</span><span class="sxs-lookup"><span data-stu-id="d7dba-104">In-process log collection</span></span>
<span data-ttu-id="d7dba-105">Naplófájljainak gyűjtése a alkalmazás használatával [Azure Diagnostics bővítmény](service-fabric-diagnostics-how-to-setup-wad.md) jó megoldás az **Azure Service Fabric** szolgáltatások kicsi, napló források és célok esetén nem változik gyakran, és nincs a forrás- és a célhelyek között egyértelmű leképezés.</span><span class="sxs-lookup"><span data-stu-id="d7dba-105">Collecting application logs using [Azure Diagnostics extension](service-fabric-diagnostics-how-to-setup-wad.md) is a good option for **Azure Service Fabric** services if the set of log sources and destinations is small, does not change often, and there is a straightforward mapping between the sources and their destinations.</span></span> <span data-ttu-id="d7dba-106">Ha nem, a szolgáltatásokat, a naplófájlok elküldése közvetlenül a központi hely helyett.</span><span class="sxs-lookup"><span data-stu-id="d7dba-106">If not, an alternative is to have services send their logs directly to a central location.</span></span> <span data-ttu-id="d7dba-107">Ezt a folyamatot nevezik **folyamat naplógyűjtést** és lehetséges több előnye is van:</span><span class="sxs-lookup"><span data-stu-id="d7dba-107">This process is known as **in-process log collection** and has several potential advantages:</span></span>

* <span data-ttu-id="d7dba-108">*Egyszerű konfiguráció és a központi telepítés*</span><span class="sxs-lookup"><span data-stu-id="d7dba-108">*Easy configuration and deployment*</span></span>

    * <span data-ttu-id="d7dba-109">Diagnosztikai adatok gyűjtésének konfigurálása most nem része a szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="d7dba-109">The configuration of diagnostic data collection is just part of the service configuration.</span></span> <span data-ttu-id="d7dba-110">Akkor is könnyen mindig biztosítható, hogy "szinkronizálva" a többi alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d7dba-110">It is easy to always keep it "in sync" with the rest of the application.</span></span>
    * <span data-ttu-id="d7dba-111">Alkalmazás vagy a szolgáltatás konfigurációja, könnyen elérhető.</span><span class="sxs-lookup"><span data-stu-id="d7dba-111">Per-application or per-service configuration is easily achievable.</span></span>
        * <span data-ttu-id="d7dba-112">Ügynök-alapú naplógyűjtést általában igényel egy külön a telepítését és konfigurálását a diagnosztikai ügynök, amely extra felügyeleti feladatot, és a hibák lehetséges forrását.</span><span class="sxs-lookup"><span data-stu-id="d7dba-112">Agent-based log collection usually requires a separate deployment and configuration of the diagnostic agent, which is an extra administrative task and a potential source of errors.</span></span> <span data-ttu-id="d7dba-113">Gyakran van az ügynök engedélyezett-e a virtuális gép (csomópont) csak egy példánya, és az ügynök konfigurációját minden futó alkalmazások és szolgáltatások ezen a csomóponton osztozik.</span><span class="sxs-lookup"><span data-stu-id="d7dba-113">Often there is only one instance of the agent allowed per virtual machine (node) and the agent configuration is shared among all applications and services running on that node.</span></span> 

* <span data-ttu-id="d7dba-114">*Rugalmasság*</span><span class="sxs-lookup"><span data-stu-id="d7dba-114">*Flexibility*</span></span>
   
    * <span data-ttu-id="d7dba-115">Az alkalmazás is küldheti az adatokat, ahol nyissa meg kell, mindaddig, amíg egy ügyfél könyvtár, amely támogatja a célként megadott tárolási rendszereket.</span><span class="sxs-lookup"><span data-stu-id="d7dba-115">The application can send the data wherever it needs to go, as long as there is a client library that supports the targeted data storage system.</span></span> <span data-ttu-id="d7dba-116">Új célokat felveheti igény szerint.</span><span class="sxs-lookup"><span data-stu-id="d7dba-116">New destinations can be added as desired.</span></span>
    * <span data-ttu-id="d7dba-117">Összetett rögzítési, szűrési és adatösszesítés szabályok kétféleképpen valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="d7dba-117">Complex capture, filtering, and data-aggregation rules can be implemented.</span></span>
    * <span data-ttu-id="d7dba-118">Ügynök-alapú naplógyűjtést gyakran korlátozott általi az adatokat, amely támogatja a az ügynök.</span><span class="sxs-lookup"><span data-stu-id="d7dba-118">Agent-based log collection is often limited by the data sinks that the agent supports.</span></span> <span data-ttu-id="d7dba-119">Néhány ügynökök olyan bővíthető.</span><span class="sxs-lookup"><span data-stu-id="d7dba-119">Some agents are extensible.</span></span>

* <span data-ttu-id="d7dba-120">*Belső alkalmazás adatokhoz és a környezetben*</span><span class="sxs-lookup"><span data-stu-id="d7dba-120">*Access to internal application data and context*</span></span>
   
    * <span data-ttu-id="d7dba-121">A diagnosztikai alrendszer, az alkalmazás/kiszolgáló folyamat-keretrendszeren belül fut. könnyen is kiegészítheti a nyomkövetési adatokat környezetfüggő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="d7dba-121">The diagnostic subsystem running inside the application/service process can easily augment the traces with contextual information.</span></span>
    * <span data-ttu-id="d7dba-122">Az ügynök-alapú naplógyűjtést az adatok az ügynök bizonyos folyamatok közti kommunikációs mechanizmus, például a Windows esemény-nyomkövetése keresztül kell küldeni.</span><span class="sxs-lookup"><span data-stu-id="d7dba-122">With agent-based log collection, the data must be sent to an agent via some inter-process communication mechanism, such as Event Tracing for Windows.</span></span> <span data-ttu-id="d7dba-123">Ez az eljárás további sikerült korlátozható.</span><span class="sxs-lookup"><span data-stu-id="d7dba-123">This mechanism could impose additional limitations.</span></span>

<span data-ttu-id="d7dba-124">Kombinálhatja, és igénybe vehesse a gyűjtemény közül egyik módszer az lehetőség.</span><span class="sxs-lookup"><span data-stu-id="d7dba-124">It is possible to combine and benefit from both collection methods.</span></span> <span data-ttu-id="d7dba-125">Ezenkívül előfordulhat, hogy a legjobb megoldás az számos alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d7dba-125">Indeed, it might be the best solution for many applications.</span></span> <span data-ttu-id="d7dba-126">Ügynök-alapú gyűjtemény egy olyan természetes megoldás az egész fürt és az egyedi fürtcsomópontokat kapcsolatos naplók gyűjtésére szolgáló.</span><span class="sxs-lookup"><span data-stu-id="d7dba-126">Agent-based collection is a natural solution for collecting logs related to the whole cluster and individual cluster nodes.</span></span> <span data-ttu-id="d7dba-127">Sokkal több megbízható módot, a folyamat naplógyűjtést, mint szolgáltatás indítási problémák diagnosztizálásához és összeomlik.</span><span class="sxs-lookup"><span data-stu-id="d7dba-127">It is much more reliable way, than in-process log collection, to diagnose service startup problems and crashes.</span></span> <span data-ttu-id="d7dba-128">Is számos szolgáltatásokkal, a Service Fabric-fürt belül futó, minden egyes szolgáltatás ennek során a saját folyamaton belüli naplógyűjtést eredményezi számos kimenő kapcsolatok a fürtből.</span><span class="sxs-lookup"><span data-stu-id="d7dba-128">Also, with many services running inside a Service Fabric cluster, each service doing its own in-process log collection results in numerous outgoing connections from the cluster.</span></span> <span data-ttu-id="d7dba-129">Kimenő kapcsolatok nagy számú adóztatásától van, a hálózati alrendszer, valamint az a naplócél.</span><span class="sxs-lookup"><span data-stu-id="d7dba-129">Large number of outgoing connections is taxing both for the network subsystem and for the log destination.</span></span> <span data-ttu-id="d7dba-130">Például egy ügynök [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) adatokat gyűjtsön a több szolgáltatás és átviteli javítása néhány kapcsolatokon keresztül minden adat küldése.</span><span class="sxs-lookup"><span data-stu-id="d7dba-130">An agent such as [**Azure Diagnostics**](../cloud-services/cloud-services-dotnet-diagnostics.md) can gather data from multiple services and send all data through a few connections, improving throughput.</span></span> 

<span data-ttu-id="d7dba-131">Ebben a cikkben megmutatjuk, hogyan állíthat be egy folyamaton belüli napló gyűjtemény használja [ **EventFlow nyílt forráskódú könyvtár**](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="d7dba-131">In this article, we show how to set up an in-process log collection using [**EventFlow open-source library**](https://github.com/Azure/diagnostics-eventflow).</span></span> <span data-ttu-id="d7dba-132">Más könyvtárak használhatók például az ugyanerre a célra, de EventFlow rendelkezik az előnye, hogy tervezték, kifejezetten a folyamaton belüli naplógyűjtést és a Service Fabric-szolgáltatásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="d7dba-132">Other libraries might be used for the same purpose, but EventFlow has the benefit of having been designed specifically for in-process log collection and to support Service Fabric services.</span></span> <span data-ttu-id="d7dba-133">Használjuk [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) napló céljaként.</span><span class="sxs-lookup"><span data-stu-id="d7dba-133">We use [**Azure Application Insights**](https://azure.microsoft.com/services/application-insights/) as the log destination.</span></span> <span data-ttu-id="d7dba-134">Más helyekre, például [ **Event Hubs** ](https://azure.microsoft.com/services/event-hubs/) vagy [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) is támogatottak.</span><span class="sxs-lookup"><span data-stu-id="d7dba-134">Other destinations such as [**Event Hubs**](https://azure.microsoft.com/services/event-hubs/) or [**Elasticsearch**](https://www.elastic.co/products/elasticsearch) are also supported.</span></span> <span data-ttu-id="d7dba-135">Csak a megfelelő NuGet-csomag telepítését és konfigurálását a cél a EventFlow konfigurációs fájl kérdése.</span><span class="sxs-lookup"><span data-stu-id="d7dba-135">It is just a question of installing appropriate NuGet package and configuring the destination in the EventFlow configuration file.</span></span> <span data-ttu-id="d7dba-136">Az Application Insights eltérő napló célok további információkért lásd: [EventFlow dokumentáció](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="d7dba-136">For more information on log destinations other than Application Insights, see [EventFlow documentation](https://github.com/Azure/diagnostics-eventflow).</span></span>

## <a name="adding-eventflow-library-to-a-service-fabric-service-project"></a><span data-ttu-id="d7dba-137">A Service Fabric-szolgáltatás projektbe EventFlow könyvtár hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d7dba-137">Adding EventFlow library to a Service Fabric service project</span></span>
<span data-ttu-id="d7dba-138">EventFlow bináris NuGet-csomagok készletként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d7dba-138">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="d7dba-139">Service Fabric-szolgáltatás projektbe EventFlow hozzáadásához kattintson a jobb gombbal a projektre a Megoldáskezelőben, és válassza a "Manage NuGet packages".</span><span class="sxs-lookup"><span data-stu-id="d7dba-139">To add EventFlow to a Service Fabric service project, right-click the project in the Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="d7dba-140">Váltson át a "Tallózás" fülre, és keresse meg "`Diagnostics.EventFlow`":</span><span class="sxs-lookup"><span data-stu-id="d7dba-140">Switch to the "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![A Visual Studio NuGet-Csomagkezelő felhasználói felület EventFlow NuGet-csomagok][1]

<span data-ttu-id="d7dba-142">A szolgáltatásüzemeltetési EventFlow attól függően, hogy a forrás- és az alkalmazásnaplókat a megfelelő csomagokat kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="d7dba-142">The service hosting EventFlow should include appropriate packages depending on the source and destination for the application logs.</span></span> <span data-ttu-id="d7dba-143">Adja hozzá a következő csomagokhoz:</span><span class="sxs-lookup"><span data-stu-id="d7dba-143">Add the following packages:</span></span> 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * <span data-ttu-id="d7dba-144">(a szolgáltatás az EventSource osztályból származik, és a szabványos EventSources adatok rögzítéséhez *Microsoft-ServiceFabric-szolgáltatások* és *Microsoft-ServiceFabric-szereplője*)</span><span class="sxs-lookup"><span data-stu-id="d7dba-144">(to capture data from the service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * <span data-ttu-id="d7dba-145">(fogjuk küldeni a naplókat az Azure Application Insights-erőforrás)</span><span class="sxs-lookup"><span data-stu-id="d7dba-145">(we are going to send the logs to an Azure Application Insights resource)</span></span>  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * <span data-ttu-id="d7dba-146">(lehetővé teszi, hogy a Service Fabric szolgáltatáskonfiguráció EventFlow folyamat inicializálása és jelentéseket, a Service Fabric állapotjelentések diagnosztikai adatok küldésének problémákat)</span><span class="sxs-lookup"><span data-stu-id="d7dba-146">(enables initialization of the EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

> [!NOTE]
> <span data-ttu-id="d7dba-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource`csomag igényli-e a projekt, amelyekre a .NET-keretrendszer 4.6-os vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d7dba-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource` package requires the service project to target .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="d7dba-148">Ellenőrizze, hogy beállította a megfelelő célkeretrendszer a projekt tulajdonságait a csomag telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="d7dba-148">Make sure you set the appropriate target framework in project properties before installing this package.</span></span> 

<span data-ttu-id="d7dba-149">A csomagok telepítése után a következő lépés, hogy konfigurálja és EventFlow engedélyezése a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="d7dba-149">After all the packages are installed, the next step is to configure and enable EventFlow in the service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="d7dba-150">Napló gyűjtésének engedélyezése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d7dba-150">Configuring and enabling log collection</span></span>
<span data-ttu-id="d7dba-151">A konfigurációs fájlban tárolt specifikáció EventFlow sorban, a naplók elküldéséért készült.</span><span class="sxs-lookup"><span data-stu-id="d7dba-151">EventFlow pipeline, responsible for sending the logs, is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="d7dba-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric`a csomag telepíti a kiindulási EventFlow konfigurációs fájl `PackageRoot\Config` mappát.</span><span class="sxs-lookup"><span data-stu-id="d7dba-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder.</span></span> <span data-ttu-id="d7dba-153">A Fájlnév `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="d7dba-153">The file name is `eventFlowConfig.json`.</span></span> <span data-ttu-id="d7dba-154">A konfigurációs fájlnak kell módosítani kell az alapértelmezett szolgáltatás adatait `EventSource` osztályhoz, és adatokat küldeni a Application Insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="d7dba-154">This configuration file needs to be modified to capture data from the default service `EventSource` class and send data to Application Insights service.</span></span>

> [!NOTE]
> <span data-ttu-id="d7dba-155">Azt feltételezik, hogy ismeri a **Azure Application Insights** szolgáltatás, és hogy van-e az Application Insights-erőforrást, amely a Service Fabric-szolgáltatás figyelése használatát tervezi.</span><span class="sxs-lookup"><span data-stu-id="d7dba-155">We assume that you are familiar with **Azure Application Insights** service and that you have an Application Insights resource that you plan to use to monitor your Service Fabric service.</span></span> <span data-ttu-id="d7dba-156">Ha további tájékoztatásra van szüksége, tekintse át [Application Insights-erőforrás létrehozása](../application-insights/app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="d7dba-156">If you need more information, please see [Create an Application Insights resource](../application-insights/app-insights-create-new-resource.md).</span></span>

<span data-ttu-id="d7dba-157">Nyissa meg a `eventFlowConfig.json` a szerkesztőben fájlt, és annak tartalma módosítható, mert a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="d7dba-157">Open the `eventFlowConfig.json` file in the editor and change its content as shown below.</span></span> <span data-ttu-id="d7dba-158">Feltétlenül cserélje le a ServiceEventSource nevét és az Application Insights instrumentation kulcs megjegyzések megfelelően.</span><span class="sxs-lookup"><span data-stu-id="d7dba-158">Make sure to replace the ServiceEventSource name and Application Insights instrumentation key according to comments.</span></span> 

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

> [!NOTE]
> <span data-ttu-id="d7dba-159">Szolgáltatás ServiceEventSource neve nem a név tulajdonságának értéke a `EventSourceAttribute` ServiceEventSource osztály.</span><span class="sxs-lookup"><span data-stu-id="d7dba-159">The name of service's ServiceEventSource is the value of the Name property of the `EventSourceAttribute` applied to the ServiceEventSource class.</span></span> <span data-ttu-id="d7dba-160">Az összes megadott a `ServiceEventSource.cs` fájl, amely a szolgáltatáskód hibáit része.</span><span class="sxs-lookup"><span data-stu-id="d7dba-160">It is all specified in the `ServiceEventSource.cs` file, which is part of the service code.</span></span> <span data-ttu-id="d7dba-161">Például a következő kódrészletet a neve, a ServiceEventSource: *értéket-Alkalmaz1-Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="d7dba-161">For example, in the following code snippet the name of the ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

<span data-ttu-id="d7dba-162">Vegye figyelembe, hogy `eventFlowConfig.json` fájl szolgáltatás konfigurációs csomag része.</span><span class="sxs-lookup"><span data-stu-id="d7dba-162">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="d7dba-163">A fájl módosításait a szolgáltatás a Service Fabric frissítési állapot-ellenőrzési eredményeire és automatikus visszaállítási, ha a frissítés nem sikerülne teljes vagy konfiguráció-csak frissítés tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="d7dba-163">Changes to this file can be included in full- or configuration-only upgrades of the service, subject to Service Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="d7dba-164">További információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="d7dba-164">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="d7dba-165">Az utolsó lépés, hogy a szolgáltatás indítási kódban található EventFlow csővezeték példányosítható `Program.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d7dba-165">The final step is to instantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file.</span></span> <span data-ttu-id="d7dba-166">A következő példa kiegészítéseket EventFlow kapcsolatos megjegyzések kezdődően lesznek megjelölve `****`:</span><span class="sxs-lookup"><span data-stu-id="d7dba-166">In the following example  EventFlow-related additions are marked with comments starting with `****`:</span></span>

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

<span data-ttu-id="d7dba-167">Nevét paraméterként, a `CreatePipeline` metódusában a `ServiceFabricDiagnosticsPipelineFactory` neve a *állapotfigyelő entitás* képviselő a EventFlow napló adatgyűjtési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="d7dba-167">The name passed as the parameter of the `CreatePipeline` method of the `ServiceFabricDiagnosticsPipelineFactory` is the name of the *health entity* representing the EventFlow log collection pipeline.</span></span> <span data-ttu-id="d7dba-168">Ezt a nevet használja, ha a EventFlow észlel, és hiba és jelenti a a Service Fabric állapotfigyelő alrendszer keresztül.</span><span class="sxs-lookup"><span data-stu-id="d7dba-168">This name is used if EventFlow encounters and error and reports it through the Service Fabric health subsystem.</span></span>

## <a name="verification"></a><span data-ttu-id="d7dba-169">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="d7dba-169">Verification</span></span>
<span data-ttu-id="d7dba-170">Indítsa el a szolgáltatást, és tekintse meg a hibakeresési Visual Studio kimeneti ablakában.</span><span class="sxs-lookup"><span data-stu-id="d7dba-170">Start your service and observe the Debug output window in Visual Studio.</span></span> <span data-ttu-id="d7dba-171">A szolgáltatás az elindítása után jelenítse meg, hogy a szolgáltatás "Application Insights Telemetria" rekordot küld bizonyító adatok.</span><span class="sxs-lookup"><span data-stu-id="d7dba-171">After the service is started, you should start seeing evidence that your service is sending "Application Insights Telemetry" records.</span></span> <span data-ttu-id="d7dba-172">Nyisson meg egy webböngészőt, és keresse meg a nyissa meg az Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d7dba-172">Open a web browser and navigate go to your Application Insights resource.</span></span> <span data-ttu-id="d7dba-173">Nyissa meg a "Search" lapon (az alapértelmezett "Overview" panelen elején).</span><span class="sxs-lookup"><span data-stu-id="d7dba-173">Open "Search" tab (at the top of the default "Overview" blade).</span></span> <span data-ttu-id="d7dba-174">Rövid késleltetés után jelenítse meg a nyomkövetések az Application Insights-portálon:</span><span class="sxs-lookup"><span data-stu-id="d7dba-174">After a short delay you should start seeing your traces in the Application Insights portal:</span></span>

![Application Insights portálon találja meg a Service Fabric-alkalmazás naplók megjelenítése][2]

## <a name="next-steps"></a><span data-ttu-id="d7dba-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7dba-176">Next steps</span></span>
* [<span data-ttu-id="d7dba-177">További információk felderítésére, és a Service Fabric-szolgáltatás figyelése</span><span class="sxs-lookup"><span data-stu-id="d7dba-177">Learn more about diagnosing and monitoring a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [<span data-ttu-id="d7dba-178">EventFlow dokumentáció</span><span class="sxs-lookup"><span data-stu-id="d7dba-178">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
