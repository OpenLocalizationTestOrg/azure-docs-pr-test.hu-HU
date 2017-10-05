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
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a>Gyűjteni a közvetlenül az Azure Service Fabric-szolgáltatás folyamata
## <a name="in-process-log-collection"></a>A folyamaton belüli naplógyűjtést
Naplófájljainak gyűjtése a alkalmazás használatával [Azure Diagnostics bővítmény](service-fabric-diagnostics-how-to-setup-wad.md) jó megoldás az **Azure Service Fabric** szolgáltatások kicsi, napló források és célok esetén nem változik gyakran, és nincs a forrás- és a célhelyek között egyértelmű leképezés. Ha nem, a szolgáltatásokat, a naplófájlok elküldése közvetlenül a központi hely helyett. Ezt a folyamatot nevezik **folyamat naplógyűjtést** és lehetséges több előnye is van:

* *Egyszerű konfiguráció és a központi telepítés*

    * Diagnosztikai adatok gyűjtésének konfigurálása most nem része a szolgáltatás konfigurációját. Akkor is könnyen mindig biztosítható, hogy "szinkronizálva" a többi alkalmazáshoz.
    * Alkalmazás vagy a szolgáltatás konfigurációja, könnyen elérhető.
        * Ügynök-alapú naplógyűjtést általában igényel egy külön a telepítését és konfigurálását a diagnosztikai ügynök, amely extra felügyeleti feladatot, és a hibák lehetséges forrását. Gyakran van az ügynök engedélyezett-e a virtuális gép (csomópont) csak egy példánya, és az ügynök konfigurációját minden futó alkalmazások és szolgáltatások ezen a csomóponton osztozik. 

* *Rugalmasság*
   
    * Az alkalmazás is küldheti az adatokat, ahol nyissa meg kell, mindaddig, amíg egy ügyfél könyvtár, amely támogatja a célként megadott tárolási rendszereket. Új célokat felveheti igény szerint.
    * Összetett rögzítési, szűrési és adatösszesítés szabályok kétféleképpen valósítható meg.
    * Ügynök-alapú naplógyűjtést gyakran korlátozott általi az adatokat, amely támogatja a az ügynök. Néhány ügynökök olyan bővíthető.

* *Belső alkalmazás adatokhoz és a környezetben*
   
    * A diagnosztikai alrendszer, az alkalmazás/kiszolgáló folyamat-keretrendszeren belül fut. könnyen is kiegészítheti a nyomkövetési adatokat környezetfüggő adatokkal.
    * Az ügynök-alapú naplógyűjtést az adatok az ügynök bizonyos folyamatok közti kommunikációs mechanizmus, például a Windows esemény-nyomkövetése keresztül kell küldeni. Ez az eljárás további sikerült korlátozható.

Kombinálhatja, és igénybe vehesse a gyűjtemény közül egyik módszer az lehetőség. Ezenkívül előfordulhat, hogy a legjobb megoldás az számos alkalmazás. Ügynök-alapú gyűjtemény egy olyan természetes megoldás az egész fürt és az egyedi fürtcsomópontokat kapcsolatos naplók gyűjtésére szolgáló. Sokkal több megbízható módot, a folyamat naplógyűjtést, mint szolgáltatás indítási problémák diagnosztizálásához és összeomlik. Is számos szolgáltatásokkal, a Service Fabric-fürt belül futó, minden egyes szolgáltatás ennek során a saját folyamaton belüli naplógyűjtést eredményezi számos kimenő kapcsolatok a fürtből. Kimenő kapcsolatok nagy számú adóztatásától van, a hálózati alrendszer, valamint az a naplócél. Például egy ügynök [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) adatokat gyűjtsön a több szolgáltatás és átviteli javítása néhány kapcsolatokon keresztül minden adat küldése. 

Ebben a cikkben megmutatjuk, hogyan állíthat be egy folyamaton belüli napló gyűjtemény használja [ **EventFlow nyílt forráskódú könyvtár**](https://github.com/Azure/diagnostics-eventflow). Más könyvtárak használhatók például az ugyanerre a célra, de EventFlow rendelkezik az előnye, hogy tervezték, kifejezetten a folyamaton belüli naplógyűjtést és a Service Fabric-szolgáltatásokat támogatja. Használjuk [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) napló céljaként. Más helyekre, például [ **Event Hubs** ](https://azure.microsoft.com/services/event-hubs/) vagy [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) is támogatottak. Csak a megfelelő NuGet-csomag telepítését és konfigurálását a cél a EventFlow konfigurációs fájl kérdése. Az Application Insights eltérő napló célok további információkért lásd: [EventFlow dokumentáció](https://github.com/Azure/diagnostics-eventflow).

## <a name="adding-eventflow-library-to-a-service-fabric-service-project"></a>A Service Fabric-szolgáltatás projektbe EventFlow könyvtár hozzáadása
EventFlow bináris NuGet-csomagok készletként érhetők el. Service Fabric-szolgáltatás projektbe EventFlow hozzáadásához kattintson a jobb gombbal a projektre a Megoldáskezelőben, és válassza a "Manage NuGet packages". Váltson át a "Tallózás" fülre, és keresse meg "`Diagnostics.EventFlow`":

![A Visual Studio NuGet-Csomagkezelő felhasználói felület EventFlow NuGet-csomagok][1]

A szolgáltatásüzemeltetési EventFlow attól függően, hogy a forrás- és az alkalmazásnaplókat a megfelelő csomagokat kell tartalmaznia. Adja hozzá a következő csomagokhoz: 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * (a szolgáltatás az EventSource osztályból származik, és a szabványos EventSources adatok rögzítéséhez *Microsoft-ServiceFabric-szolgáltatások* és *Microsoft-ServiceFabric-szereplője*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * (fogjuk küldeni a naplókat az Azure Application Insights-erőforrás)  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * (lehetővé teszi, hogy a Service Fabric szolgáltatáskonfiguráció EventFlow folyamat inicializálása és jelentéseket, a Service Fabric állapotjelentések diagnosztikai adatok küldésének problémákat)

> [!NOTE]
> `Microsoft.Diagnostics.EventFlow.Inputs.EventSource`csomag igényli-e a projekt, amelyekre a .NET-keretrendszer 4.6-os vagy újabb. Ellenőrizze, hogy beállította a megfelelő célkeretrendszer a projekt tulajdonságait a csomag telepítése előtt. 

A csomagok telepítése után a következő lépés, hogy konfigurálja és EventFlow engedélyezése a szolgáltatásban.

## <a name="configuring-and-enabling-log-collection"></a>Napló gyűjtésének engedélyezése és konfigurálása
A konfigurációs fájlban tárolt specifikáció EventFlow sorban, a naplók elküldéséért készült. `Microsoft.Diagnostics.EventFlow.ServiceFabric`a csomag telepíti a kiindulási EventFlow konfigurációs fájl `PackageRoot\Config` mappát. A Fájlnév `eventFlowConfig.json`. A konfigurációs fájlnak kell módosítani kell az alapértelmezett szolgáltatás adatait `EventSource` osztályhoz, és adatokat küldeni a Application Insights szolgáltatással.

> [!NOTE]
> Azt feltételezik, hogy ismeri a **Azure Application Insights** szolgáltatás, és hogy van-e az Application Insights-erőforrást, amely a Service Fabric-szolgáltatás figyelése használatát tervezi. Ha további tájékoztatásra van szüksége, tekintse át [Application Insights-erőforrás létrehozása](../application-insights/app-insights-create-new-resource.md).

Nyissa meg a `eventFlowConfig.json` a szerkesztőben fájlt, és annak tartalma módosítható, mert a lent látható módon. Feltétlenül cserélje le a ServiceEventSource nevét és az Application Insights instrumentation kulcs megjegyzések megfelelően. 

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
> Szolgáltatás ServiceEventSource neve nem a név tulajdonságának értéke a `EventSourceAttribute` ServiceEventSource osztály. Az összes megadott a `ServiceEventSource.cs` fájl, amely a szolgáltatáskód hibáit része. Például a következő kódrészletet a neve, a ServiceEventSource: *értéket-Alkalmaz1-Stateless1*:
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

Vegye figyelembe, hogy `eventFlowConfig.json` fájl szolgáltatás konfigurációs csomag része. A fájl módosításait a szolgáltatás a Service Fabric frissítési állapot-ellenőrzési eredményeire és automatikus visszaállítási, ha a frissítés nem sikerülne teljes vagy konfiguráció-csak frissítés tartalmazhat. További információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).

Az utolsó lépés, hogy a szolgáltatás indítási kódban található EventFlow csővezeték példányosítható `Program.cs` fájlt. A következő példa kiegészítéseket EventFlow kapcsolatos megjegyzések kezdődően lesznek megjelölve `****`:

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

Nevét paraméterként, a `CreatePipeline` metódusában a `ServiceFabricDiagnosticsPipelineFactory` neve a *állapotfigyelő entitás* képviselő a EventFlow napló adatgyűjtési folyamatot. Ezt a nevet használja, ha a EventFlow észlel, és hiba és jelenti a a Service Fabric állapotfigyelő alrendszer keresztül.

## <a name="verification"></a>Ellenőrzés
Indítsa el a szolgáltatást, és tekintse meg a hibakeresési Visual Studio kimeneti ablakában. A szolgáltatás az elindítása után jelenítse meg, hogy a szolgáltatás "Application Insights Telemetria" rekordot küld bizonyító adatok. Nyisson meg egy webböngészőt, és keresse meg a nyissa meg az Application Insights-erőforrást. Nyissa meg a "Search" lapon (az alapértelmezett "Overview" panelen elején). Rövid késleltetés után jelenítse meg a nyomkövetések az Application Insights-portálon:

![Application Insights portálon találja meg a Service Fabric-alkalmazás naplók megjelenítése][2]

## <a name="next-steps"></a>Következő lépések
* [További információk felderítésére, és a Service Fabric-szolgáltatás figyelése](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [EventFlow dokumentáció](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
