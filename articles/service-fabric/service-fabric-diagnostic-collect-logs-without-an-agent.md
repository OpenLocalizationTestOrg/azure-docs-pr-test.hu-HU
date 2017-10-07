---
title: "aaaCollect naplók közvetlenül az Azure Service Fabric a szolgáltatás folyamatának |} A Microsoft Azure"
description: "A Service Fabric alkalmazások elküldheti a naplófájlokat, közvetlen tooa központi helyen, például Azure Application Insights vagy Elasticsearch, anélkül, hogy az Azure diagnosztikai ügynök ismerteti."
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
ms.openlocfilehash: d0681a2a6aaa76028d7cb469c31c006f24bbe954
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a><span data-ttu-id="dd9a2-103">Gyűjteni a közvetlenül az Azure Service Fabric-szolgáltatás folyamata</span><span class="sxs-lookup"><span data-stu-id="dd9a2-103">Collect logs directly from an Azure Service Fabric service process</span></span>
## <a name="in-process-log-collection"></a><span data-ttu-id="dd9a2-104">A folyamaton belüli naplógyűjtést</span><span class="sxs-lookup"><span data-stu-id="dd9a2-104">In-process log collection</span></span>
<span data-ttu-id="dd9a2-105">Naplófájljainak gyűjtése a alkalmazás használatával [Azure Diagnostics bővítmény](service-fabric-diagnostics-how-to-setup-wad.md) jó megoldás az **Azure Service Fabric** szolgáltatások kicsi, napló források és célok hello beállítása esetén nem változik gyakran, és nincs egy egyszerű leképezése hello források és a célhelyek között van.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-105">Collecting application logs using [Azure Diagnostics extension](service-fabric-diagnostics-how-to-setup-wad.md) is a good option for **Azure Service Fabric** services if hello set of log sources and destinations is small, does not change often, and there is a straightforward mapping between hello sources and their destinations.</span></span> <span data-ttu-id="dd9a2-106">Ha nem, a másik lehetőség az toohave szolgáltatások naplófájljainak elküldése e-azok közvetlenül tooa központi helyen.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-106">If not, an alternative is toohave services send their logs directly tooa central location.</span></span> <span data-ttu-id="dd9a2-107">Ezt a folyamatot nevezik **folyamat naplógyűjtést** és lehetséges több előnye is van:</span><span class="sxs-lookup"><span data-stu-id="dd9a2-107">This process is known as **in-process log collection** and has several potential advantages:</span></span>

* <span data-ttu-id="dd9a2-108">*Egyszerű konfiguráció és a központi telepítés*</span><span class="sxs-lookup"><span data-stu-id="dd9a2-108">*Easy configuration and deployment*</span></span>

    * <span data-ttu-id="dd9a2-109">diagnosztikai adatok gyűjtésének hello konfigurálása most nem részei hello szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-109">hello configuration of diagnostic data collection is just part of hello service configuration.</span></span> <span data-ttu-id="dd9a2-110">Egyszerű tooalways megőrzése "szinkronizálva" hello, rest-hello alkalmazás is.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-110">It is easy tooalways keep it "in sync" with hello rest of hello application.</span></span>
    * <span data-ttu-id="dd9a2-111">Alkalmazás vagy a szolgáltatás konfigurációja, könnyen elérhető.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-111">Per-application or per-service configuration is easily achievable.</span></span>
        * <span data-ttu-id="dd9a2-112">Ügynök-alapú naplógyűjtést általában külön központi telepítési és konfigurációs hello diagnosztikai ügynök, amely extra felügyeleti feladatot, és lehetséges forrását hibák szükséges.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-112">Agent-based log collection usually requires a separate deployment and configuration of hello diagnostic agent, which is an extra administrative task and a potential source of errors.</span></span> <span data-ttu-id="dd9a2-113">Gyakran csak egy példánya engedélyezett-e a virtuális gép (csomópont) hello ügynök és a hello gazdagépügynök-konfigurálási osztozik az összes futó alkalmazások és szolgáltatások ezen a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-113">Often there is only one instance of hello agent allowed per virtual machine (node) and hello agent configuration is shared among all applications and services running on that node.</span></span> 

* <span data-ttu-id="dd9a2-114">*Rugalmasság*</span><span class="sxs-lookup"><span data-stu-id="dd9a2-114">*Flexibility*</span></span>
   
    * <span data-ttu-id="dd9a2-115">hello alkalmazás adatküldés hello bárhol toogo, kell, amíg nincs egy ügyféloldali kódtár megcélzott hello adatok tárolórendszer támogató.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-115">hello application can send hello data wherever it needs toogo, as long as there is a client library that supports hello targeted data storage system.</span></span> <span data-ttu-id="dd9a2-116">Új célokat felveheti igény szerint.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-116">New destinations can be added as desired.</span></span>
    * <span data-ttu-id="dd9a2-117">Összetett rögzítési, szűrési és adatösszesítés szabályok kétféleképpen valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-117">Complex capture, filtering, and data-aggregation rules can be implemented.</span></span>
    * <span data-ttu-id="dd9a2-118">Ügynök-alapú naplógyűjtést gyakran korlátozott általi hello adatok, amelyek hello ügynök támogatja.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-118">Agent-based log collection is often limited by hello data sinks that hello agent supports.</span></span> <span data-ttu-id="dd9a2-119">Néhány ügynökök olyan bővíthető.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-119">Some agents are extensible.</span></span>

* <span data-ttu-id="dd9a2-120">*Hozzáférés toointernal alkalmazásadatok és a környezetben*</span><span class="sxs-lookup"><span data-stu-id="dd9a2-120">*Access toointernal application data and context*</span></span>
   
    * <span data-ttu-id="dd9a2-121">hello diagnosztikai alrendszer hello alkalmazás/kiszolgáló folyamat belül futó könnyen is kiegészítheti hello nyomkövetések környezetfüggő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-121">hello diagnostic subsystem running inside hello application/service process can easily augment hello traces with contextual information.</span></span>
    * <span data-ttu-id="dd9a2-122">Az ügynök-alapú naplógyűjtést hello adatokat kell elküldeni tooan ügynök keresztül bizonyos folyamatok közti kommunikációs mechanizmus, például a Windows esemény-nyomkövetése.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-122">With agent-based log collection, hello data must be sent tooan agent via some inter-process communication mechanism, such as Event Tracing for Windows.</span></span> <span data-ttu-id="dd9a2-123">Ez az eljárás további sikerült korlátozható.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-123">This mechanism could impose additional limitations.</span></span>

<span data-ttu-id="dd9a2-124">Ez nem lehetséges toocombine és a gyűjtemény közül egyik módszer hasznos.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-124">It is possible toocombine and benefit from both collection methods.</span></span> <span data-ttu-id="dd9a2-125">Ezenkívül előfordulhat, hogy számos alkalmazás hello legjobb megoldást.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-125">Indeed, it might be hello best solution for many applications.</span></span> <span data-ttu-id="dd9a2-126">Ügynök-alapú gyűjtemény egy olyan természetes megoldás naplók kapcsolódó toohello teljes fürtön és az egyedi fürtcsomópontokat gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-126">Agent-based collection is a natural solution for collecting logs related toohello whole cluster and individual cluster nodes.</span></span> <span data-ttu-id="dd9a2-127">Az módja a sokkal megbízhatóbb, mint a folyamaton belüli naplógyűjtést, toodiagnose szolgáltatás indításával kapcsolatos hibák és összeomlások.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-127">It is much more reliable way, than in-process log collection, toodiagnose service startup problems and crashes.</span></span> <span data-ttu-id="dd9a2-128">Is számos szolgáltatásokkal, a Service Fabric-fürt belül futó, ennek során a saját folyamaton belüli naplógyűjtést minden egyes szolgáltatás eredményezi számos kimenő kapcsolatok hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-128">Also, with many services running inside a Service Fabric cluster, each service doing its own in-process log collection results in numerous outgoing connections from hello cluster.</span></span> <span data-ttu-id="dd9a2-129">Kimenő kapcsolatok nagy számú hello hálózati alrendszer, valamint az hello naplócél van adóztatásától.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-129">Large number of outgoing connections is taxing both for hello network subsystem and for hello log destination.</span></span> <span data-ttu-id="dd9a2-130">Például egy ügynök [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) adatokat gyűjtsön a több szolgáltatás és átviteli javítása néhány kapcsolatokon keresztül minden adat küldése.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-130">An agent such as [**Azure Diagnostics**](../cloud-services/cloud-services-dotnet-diagnostics.md) can gather data from multiple services and send all data through a few connections, improving throughput.</span></span> 

<span data-ttu-id="dd9a2-131">Ebben a cikkben megmutatjuk, hogyan jelentkezzen be egy folyamaton belüli tooset gyűjtemény használja [ **EventFlow nyílt forráskódú könyvtár**](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="dd9a2-131">In this article, we show how tooset up an in-process log collection using [**EventFlow open-source library**](https://github.com/Azure/diagnostics-eventflow).</span></span> <span data-ttu-id="dd9a2-132">Más könyvtárak használhatók hello azonos céllal, de EventFlow rendelkezik hello előnye, hogy tervezték, kifejezetten a folyamaton belüli napló gyűjtemény és toosupport Service Fabric-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-132">Other libraries might be used for hello same purpose, but EventFlow has hello benefit of having been designed specifically for in-process log collection and toosupport Service Fabric services.</span></span> <span data-ttu-id="dd9a2-133">Használjuk [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) hello napló célként.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-133">We use [**Azure Application Insights**](https://azure.microsoft.com/services/application-insights/) as hello log destination.</span></span> <span data-ttu-id="dd9a2-134">Más helyekre, például [ **Event Hubs** ](https://azure.microsoft.com/services/event-hubs/) vagy [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) is támogatottak.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-134">Other destinations such as [**Event Hubs**](https://azure.microsoft.com/services/event-hubs/) or [**Elasticsearch**](https://www.elastic.co/products/elasticsearch) are also supported.</span></span> <span data-ttu-id="dd9a2-135">Csak a megfelelő NuGet-csomag telepítése és konfigurálása a hello cél hello EventFlow konfigurációs fájlban kérdése.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-135">It is just a question of installing appropriate NuGet package and configuring hello destination in hello EventFlow configuration file.</span></span> <span data-ttu-id="dd9a2-136">Az Application Insights eltérő napló célok további információkért lásd: [EventFlow dokumentáció](https://github.com/Azure/diagnostics-eventflow).</span><span class="sxs-lookup"><span data-stu-id="dd9a2-136">For more information on log destinations other than Application Insights, see [EventFlow documentation](https://github.com/Azure/diagnostics-eventflow).</span></span>

## <a name="adding-eventflow-library-tooa-service-fabric-service-project"></a><span data-ttu-id="dd9a2-137">EventFlow könyvtár tooa Service Fabric projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dd9a2-137">Adding EventFlow library tooa Service Fabric service project</span></span>
<span data-ttu-id="dd9a2-138">EventFlow bináris NuGet-csomagok készletként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-138">EventFlow binaries are available as a set of NuGet packages.</span></span> <span data-ttu-id="dd9a2-139">tooadd EventFlow tooa Service Fabric-projekt hello projektre a Solution Explorer hello gombbal, és válassza a "Manage NuGet packages".</span><span class="sxs-lookup"><span data-stu-id="dd9a2-139">tooadd EventFlow tooa Service Fabric service project, right-click hello project in hello Solution Explorer and choose "Manage NuGet packages."</span></span> <span data-ttu-id="dd9a2-140">Váltás toohello "Tallózás" fülre, és keressen a "`Diagnostics.EventFlow`":</span><span class="sxs-lookup"><span data-stu-id="dd9a2-140">Switch toohello "Browse" tab and search for "`Diagnostics.EventFlow`":</span></span>

![A Visual Studio NuGet-Csomagkezelő felhasználói felület EventFlow NuGet-csomagok][1]

<span data-ttu-id="dd9a2-142">hello szolgáltatást üzemeltető EventFlow attól függően, hogy hello forrása és célja hello alkalmazásnaplók megfelelő csomagokat kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-142">hello service hosting EventFlow should include appropriate packages depending on hello source and destination for hello application logs.</span></span> <span data-ttu-id="dd9a2-143">Adja hozzá a következő csomagok hello:</span><span class="sxs-lookup"><span data-stu-id="dd9a2-143">Add hello following packages:</span></span> 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * <span data-ttu-id="dd9a2-144">(toocapture adatok hello szolgáltatást az EventSource osztályból származik, illetve a szabványos EventSources például *Microsoft-ServiceFabric-szolgáltatások* és *Microsoft-ServiceFabric-szereplője*)</span><span class="sxs-lookup"><span data-stu-id="dd9a2-144">(toocapture data from hello service's EventSource class, and from standard EventSources such as *Microsoft-ServiceFabric-Services* and *Microsoft-ServiceFabric-Actors*)</span></span>
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * <span data-ttu-id="dd9a2-145">(fogjuk toosend hello naplók tooan Azure Application Insights-erőforrás)</span><span class="sxs-lookup"><span data-stu-id="dd9a2-145">(we are going toosend hello logs tooan Azure Application Insights resource)</span></span>  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * <span data-ttu-id="dd9a2-146">(lehetővé teszi, hogy a Service Fabric szolgáltatáskonfiguráció hello EventFlow folyamat inicializálása és jelentéseket, a Service Fabric állapotjelentések diagnosztikai adatok küldésének problémákat)</span><span class="sxs-lookup"><span data-stu-id="dd9a2-146">(enables initialization of hello EventFlow pipeline from Service Fabric service configuration and reports any problems with sending diagnostic data as Service Fabric health reports)</span></span>

> [!NOTE]
> <span data-ttu-id="dd9a2-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource`csomag igényli-e hello szolgáltatás projekt tootarget .NET keretrendszer 4.6-os vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-147">`Microsoft.Diagnostics.EventFlow.Inputs.EventSource` package requires hello service project tootarget .NET Framework 4.6 or newer.</span></span> <span data-ttu-id="dd9a2-148">Ellenőrizze, hogy beállította hello megfelelő célkeretrendszer a projekt tulajdonságait a csomag telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-148">Make sure you set hello appropriate target framework in project properties before installing this package.</span></span> 

<span data-ttu-id="dd9a2-149">Ha minden hello csomagok telepítve van, a hello a következő lépés tooconfigure és EventFlow engedélyezése hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-149">After all hello packages are installed, hello next step is tooconfigure and enable EventFlow in hello service.</span></span>

## <a name="configuring-and-enabling-log-collection"></a><span data-ttu-id="dd9a2-150">Napló gyűjtésének engedélyezése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dd9a2-150">Configuring and enabling log collection</span></span>
<span data-ttu-id="dd9a2-151">A konfigurációs fájlban tárolt specifikáció EventFlow sorban, hello naplók elküldéséért készült.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-151">EventFlow pipeline, responsible for sending hello logs, is created from a specification stored in a configuration file.</span></span> <span data-ttu-id="dd9a2-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric`a csomag telepíti a kiindulási EventFlow konfigurációs fájl `PackageRoot\Config` mappát.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-152">`Microsoft.Diagnostics.EventFlow.ServiceFabric` package installs a starting EventFlow configuration file under `PackageRoot\Config` solution folder.</span></span> <span data-ttu-id="dd9a2-153">hello Fájlnév `eventFlowConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-153">hello file name is `eventFlowConfig.json`.</span></span> <span data-ttu-id="dd9a2-154">A konfigurációs fájlnak kell hello alapértelmezett szolgáltatás adatait módosított toobe toocapture `EventSource` osztályhoz, és küldjön adatokat tooApplication Insights szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-154">This configuration file needs toobe modified toocapture data from hello default service `EventSource` class and send data tooApplication Insights service.</span></span>

> [!NOTE]
> <span data-ttu-id="dd9a2-155">Azt feltételezik, hogy ismeri a **Azure Application Insights** szolgáltatás, és hogy van-e az Application Insights-erőforrás megtervezni toouse toomonitor a Service Fabric-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-155">We assume that you are familiar with **Azure Application Insights** service and that you have an Application Insights resource that you plan toouse toomonitor your Service Fabric service.</span></span> <span data-ttu-id="dd9a2-156">Ha további tájékoztatásra van szüksége, tekintse át [Application Insights-erőforrás létrehozása](../application-insights/app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="dd9a2-156">If you need more information, please see [Create an Application Insights resource](../application-insights/app-insights-create-new-resource.md).</span></span>

<span data-ttu-id="dd9a2-157">Nyissa meg hello `eventFlowConfig.json` hello szerkesztő fájlt, és annak tartalma módosítható, mert a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-157">Open hello `eventFlowConfig.json` file in hello editor and change its content as shown below.</span></span> <span data-ttu-id="dd9a2-158">Győződjön meg arról, hogy tooreplace hello ServiceEventSource nevét és az Application Insights instrumentation kulcs toocomments alapján történik.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-158">Make sure tooreplace hello ServiceEventSource name and Application Insights instrumentation key according toocomments.</span></span> 

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

> [!NOTE]
> <span data-ttu-id="dd9a2-159">szolgáltatás ServiceEventSource hello neve nem hello hello név tulajdonságának értéke hello `EventSourceAttribute` toohello ServiceEventSource osztály alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-159">hello name of service's ServiceEventSource is hello value of hello Name property of hello `EventSourceAttribute` applied toohello ServiceEventSource class.</span></span> <span data-ttu-id="dd9a2-160">Az összes megadott hello `ServiceEventSource.cs` fájlt, amely hello szolgáltatást kód része.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-160">It is all specified in hello `ServiceEventSource.cs` file, which is part of hello service code.</span></span> <span data-ttu-id="dd9a2-161">Például a hello hello ServiceEventSource következő kód részlet hello neve nem *értéket-Alkalmaz1-Stateless1*:</span><span class="sxs-lookup"><span data-stu-id="dd9a2-161">For example, in hello following code snippet hello name of hello ServiceEventSource is *MyCompany-Application1-Stateless1*:</span></span>
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

<span data-ttu-id="dd9a2-162">Vegye figyelembe, hogy `eventFlowConfig.json` fájl szolgáltatás konfigurációs csomag része.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-162">Note that `eventFlowConfig.json` file is part of service configuration package.</span></span> <span data-ttu-id="dd9a2-163">Módosítások toothis fájl tartalmazhat hello szolgáltatás teljes vagy konfiguráció-csak frissítések, tulajdonos tooService háló frissítési állapot-ellenőrzési eredményeire és automatikus visszaállítási sikertelen frissítése esetén.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-163">Changes toothis file can be included in full- or configuration-only upgrades of hello service, subject tooService Fabric upgrade health checks and automatic rollback if there is upgrade failure.</span></span> <span data-ttu-id="dd9a2-164">További információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="dd9a2-164">For more information, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="dd9a2-165">hello utolsó lépés a szolgáltatás indítási kódban található tooinstantiate EventFlow csővezeték `Program.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-165">hello final step is tooinstantiate EventFlow pipeline in your service's startup code, located in `Program.cs` file.</span></span> <span data-ttu-id="dd9a2-166">Hello az alábbi példa EventFlow kapcsolatos kiegészítésekkel megjegyzések kezdődően lesznek megjelölve `****`:</span><span class="sxs-lookup"><span data-stu-id="dd9a2-166">In hello following example  EventFlow-related additions are marked with comments starting with `****`:</span></span>

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

<span data-ttu-id="dd9a2-167">a hello hello paraméterként átadott hello neve `CreatePipeline` hello metódusában `ServiceFabricDiagnosticsPipelineFactory` hello hello neve *állapotfigyelő entitás* képviselő hello EventFlow napló adatgyűjtési folyamat.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-167">hello name passed as hello parameter of hello `CreatePipeline` method of hello `ServiceFabricDiagnosticsPipelineFactory` is hello name of hello *health entity* representing hello EventFlow log collection pipeline.</span></span> <span data-ttu-id="dd9a2-168">Ezt a nevet használja, ha a EventFlow észlel, és hiba és jelenti a Service Fabric állapotfigyelő alrendszer hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-168">This name is used if EventFlow encounters and error and reports it through hello Service Fabric health subsystem.</span></span>

## <a name="verification"></a><span data-ttu-id="dd9a2-169">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="dd9a2-169">Verification</span></span>
<span data-ttu-id="dd9a2-170">Indítsa el a szolgáltatást, és tekintse meg a hibakeresési hello Visual Studio kimeneti ablakában.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-170">Start your service and observe hello Debug output window in Visual Studio.</span></span> <span data-ttu-id="dd9a2-171">Hello szolgáltatást az elindítása után jelenítse meg, hogy a szolgáltatás "Application Insights Telemetria" rekordot küld bizonyító adatok.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-171">After hello service is started, you should start seeing evidence that your service is sending "Application Insights Telemetry" records.</span></span> <span data-ttu-id="dd9a2-172">Nyisson meg egy webböngészőt, és keresse meg a lépjen tooyour Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="dd9a2-172">Open a web browser and navigate go tooyour Application Insights resource.</span></span> <span data-ttu-id="dd9a2-173">Nyissa meg a "Search" lapját (tetején hello hello alapértelmezett "Overview" panelen).</span><span class="sxs-lookup"><span data-stu-id="dd9a2-173">Open "Search" tab (at hello top of hello default "Overview" blade).</span></span> <span data-ttu-id="dd9a2-174">Rövid késleltetés után jelenítse meg a clusterconfig.JSON fájlban hello Application Insights portál:</span><span class="sxs-lookup"><span data-stu-id="dd9a2-174">After a short delay you should start seeing your traces in hello Application Insights portal:</span></span>

![Application Insights portálon találja meg a Service Fabric-alkalmazás naplók megjelenítése][2]

## <a name="next-steps"></a><span data-ttu-id="dd9a2-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dd9a2-176">Next steps</span></span>
* [<span data-ttu-id="dd9a2-177">További információk felderítésére, és a Service Fabric-szolgáltatás figyelése</span><span class="sxs-lookup"><span data-stu-id="dd9a2-177">Learn more about diagnosing and monitoring a Service Fabric service</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [<span data-ttu-id="dd9a2-178">EventFlow dokumentáció</span><span class="sxs-lookup"><span data-stu-id="dd9a2-178">EventFlow documentation</span></span>](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
