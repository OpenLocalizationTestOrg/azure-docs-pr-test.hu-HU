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
# <a name="collect-logs-directly-from-an-azure-service-fabric-service-process"></a>Gyűjteni a közvetlenül az Azure Service Fabric-szolgáltatás folyamata
## <a name="in-process-log-collection"></a>A folyamaton belüli naplógyűjtést
Naplófájljainak gyűjtése a alkalmazás használatával [Azure Diagnostics bővítmény](service-fabric-diagnostics-how-to-setup-wad.md) jó megoldás az **Azure Service Fabric** szolgáltatások kicsi, napló források és célok hello beállítása esetén nem változik gyakran, és nincs egy egyszerű leképezése hello források és a célhelyek között van. Ha nem, a másik lehetőség az toohave szolgáltatások naplófájljainak elküldése e-azok közvetlenül tooa központi helyen. Ezt a folyamatot nevezik **folyamat naplógyűjtést** és lehetséges több előnye is van:

* *Egyszerű konfiguráció és a központi telepítés*

    * diagnosztikai adatok gyűjtésének hello konfigurálása most nem részei hello szolgáltatás konfigurációját. Egyszerű tooalways megőrzése "szinkronizálva" hello, rest-hello alkalmazás is.
    * Alkalmazás vagy a szolgáltatás konfigurációja, könnyen elérhető.
        * Ügynök-alapú naplógyűjtést általában külön központi telepítési és konfigurációs hello diagnosztikai ügynök, amely extra felügyeleti feladatot, és lehetséges forrását hibák szükséges. Gyakran csak egy példánya engedélyezett-e a virtuális gép (csomópont) hello ügynök és a hello gazdagépügynök-konfigurálási osztozik az összes futó alkalmazások és szolgáltatások ezen a csomóponton. 

* *Rugalmasság*
   
    * hello alkalmazás adatküldés hello bárhol toogo, kell, amíg nincs egy ügyféloldali kódtár megcélzott hello adatok tárolórendszer támogató. Új célokat felveheti igény szerint.
    * Összetett rögzítési, szűrési és adatösszesítés szabályok kétféleképpen valósítható meg.
    * Ügynök-alapú naplógyűjtést gyakran korlátozott általi hello adatok, amelyek hello ügynök támogatja. Néhány ügynökök olyan bővíthető.

* *Hozzáférés toointernal alkalmazásadatok és a környezetben*
   
    * hello diagnosztikai alrendszer hello alkalmazás/kiszolgáló folyamat belül futó könnyen is kiegészítheti hello nyomkövetések környezetfüggő adatokkal.
    * Az ügynök-alapú naplógyűjtést hello adatokat kell elküldeni tooan ügynök keresztül bizonyos folyamatok közti kommunikációs mechanizmus, például a Windows esemény-nyomkövetése. Ez az eljárás további sikerült korlátozható.

Ez nem lehetséges toocombine és a gyűjtemény közül egyik módszer hasznos. Ezenkívül előfordulhat, hogy számos alkalmazás hello legjobb megoldást. Ügynök-alapú gyűjtemény egy olyan természetes megoldás naplók kapcsolódó toohello teljes fürtön és az egyedi fürtcsomópontokat gyűjtéséhez. Az módja a sokkal megbízhatóbb, mint a folyamaton belüli naplógyűjtést, toodiagnose szolgáltatás indításával kapcsolatos hibák és összeomlások. Is számos szolgáltatásokkal, a Service Fabric-fürt belül futó, ennek során a saját folyamaton belüli naplógyűjtést minden egyes szolgáltatás eredményezi számos kimenő kapcsolatok hello fürtből. Kimenő kapcsolatok nagy számú hello hálózati alrendszer, valamint az hello naplócél van adóztatásától. Például egy ügynök [ **Azure Diagnostics** ](../cloud-services/cloud-services-dotnet-diagnostics.md) adatokat gyűjtsön a több szolgáltatás és átviteli javítása néhány kapcsolatokon keresztül minden adat küldése. 

Ebben a cikkben megmutatjuk, hogyan jelentkezzen be egy folyamaton belüli tooset gyűjtemény használja [ **EventFlow nyílt forráskódú könyvtár**](https://github.com/Azure/diagnostics-eventflow). Más könyvtárak használhatók hello azonos céllal, de EventFlow rendelkezik hello előnye, hogy tervezték, kifejezetten a folyamaton belüli napló gyűjtemény és toosupport Service Fabric-szolgáltatás. Használjuk [ **Azure Application Insights** ](https://azure.microsoft.com/services/application-insights/) hello napló célként. Más helyekre, például [ **Event Hubs** ](https://azure.microsoft.com/services/event-hubs/) vagy [ **Elasticsearch** ](https://www.elastic.co/products/elasticsearch) is támogatottak. Csak a megfelelő NuGet-csomag telepítése és konfigurálása a hello cél hello EventFlow konfigurációs fájlban kérdése. Az Application Insights eltérő napló célok további információkért lásd: [EventFlow dokumentáció](https://github.com/Azure/diagnostics-eventflow).

## <a name="adding-eventflow-library-tooa-service-fabric-service-project"></a>EventFlow könyvtár tooa Service Fabric projekt hozzáadása
EventFlow bináris NuGet-csomagok készletként érhetők el. tooadd EventFlow tooa Service Fabric-projekt hello projektre a Solution Explorer hello gombbal, és válassza a "Manage NuGet packages". Váltás toohello "Tallózás" fülre, és keressen a "`Diagnostics.EventFlow`":

![A Visual Studio NuGet-Csomagkezelő felhasználói felület EventFlow NuGet-csomagok][1]

hello szolgáltatást üzemeltető EventFlow attól függően, hogy hello forrása és célja hello alkalmazásnaplók megfelelő csomagokat kell tartalmaznia. Adja hozzá a következő csomagok hello: 

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` 
    * (toocapture adatok hello szolgáltatást az EventSource osztályból származik, illetve a szabványos EventSources például *Microsoft-ServiceFabric-szolgáltatások* és *Microsoft-ServiceFabric-szereplője*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` 
    * (fogjuk toosend hello naplók tooan Azure Application Insights-erőforrás)  
* `Microsoft.Diagnostics.EventFlow.ServiceFabric` 
    * (lehetővé teszi, hogy a Service Fabric szolgáltatáskonfiguráció hello EventFlow folyamat inicializálása és jelentéseket, a Service Fabric állapotjelentések diagnosztikai adatok küldésének problémákat)

> [!NOTE]
> `Microsoft.Diagnostics.EventFlow.Inputs.EventSource`csomag igényli-e hello szolgáltatás projekt tootarget .NET keretrendszer 4.6-os vagy újabb. Ellenőrizze, hogy beállította hello megfelelő célkeretrendszer a projekt tulajdonságait a csomag telepítése előtt. 

Ha minden hello csomagok telepítve van, a hello a következő lépés tooconfigure és EventFlow engedélyezése hello szolgáltatásban.

## <a name="configuring-and-enabling-log-collection"></a>Napló gyűjtésének engedélyezése és konfigurálása
A konfigurációs fájlban tárolt specifikáció EventFlow sorban, hello naplók elküldéséért készült. `Microsoft.Diagnostics.EventFlow.ServiceFabric`a csomag telepíti a kiindulási EventFlow konfigurációs fájl `PackageRoot\Config` mappát. hello Fájlnév `eventFlowConfig.json`. A konfigurációs fájlnak kell hello alapértelmezett szolgáltatás adatait módosított toobe toocapture `EventSource` osztályhoz, és küldjön adatokat tooApplication Insights szolgáltatás.

> [!NOTE]
> Azt feltételezik, hogy ismeri a **Azure Application Insights** szolgáltatás, és hogy van-e az Application Insights-erőforrás megtervezni toouse toomonitor a Service Fabric-szolgáltatás. Ha további tájékoztatásra van szüksége, tekintse át [Application Insights-erőforrás létrehozása](../application-insights/app-insights-create-new-resource.md).

Nyissa meg hello `eventFlowConfig.json` hello szerkesztő fájlt, és annak tartalma módosítható, mert a lent látható módon. Győződjön meg arról, hogy tooreplace hello ServiceEventSource nevét és az Application Insights instrumentation kulcs toocomments alapján történik. 

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
> szolgáltatás ServiceEventSource hello neve nem hello hello név tulajdonságának értéke hello `EventSourceAttribute` toohello ServiceEventSource osztály alkalmazza. Az összes megadott hello `ServiceEventSource.cs` fájlt, amely hello szolgáltatást kód része. Például a hello hello ServiceEventSource következő kód részlet hello neve nem *értéket-Alkalmaz1-Stateless1*:
> ```csharp
> [EventSource(Name = "MyCompany-Application1-Stateless1")]
> internal sealed class ServiceEventSource : EventSource
> {
>    // (rest of ServiceEventSource implementation)
>} 
> ```

Vegye figyelembe, hogy `eventFlowConfig.json` fájl szolgáltatás konfigurációs csomag része. Módosítások toothis fájl tartalmazhat hello szolgáltatás teljes vagy konfiguráció-csak frissítések, tulajdonos tooService háló frissítési állapot-ellenőrzési eredményeire és automatikus visszaállítási sikertelen frissítése esetén. További információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).

hello utolsó lépés a szolgáltatás indítási kódban található tooinstantiate EventFlow csővezeték `Program.cs` fájlt. Hello az alábbi példa EventFlow kapcsolatos kiegészítésekkel megjegyzések kezdődően lesznek megjelölve `****`:

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

## <a name="verification"></a>Ellenőrzés
Indítsa el a szolgáltatást, és tekintse meg a hibakeresési hello Visual Studio kimeneti ablakában. Hello szolgáltatást az elindítása után jelenítse meg, hogy a szolgáltatás "Application Insights Telemetria" rekordot küld bizonyító adatok. Nyisson meg egy webböngészőt, és keresse meg a lépjen tooyour Application Insights-erőforrást. Nyissa meg a "Search" lapját (tetején hello hello alapértelmezett "Overview" panelen). Rövid késleltetés után jelenítse meg a clusterconfig.JSON fájlban hello Application Insights portál:

![Application Insights portálon találja meg a Service Fabric-alkalmazás naplók megjelenítése][2]

## <a name="next-steps"></a>Következő lépések
* [További információk felderítésére, és a Service Fabric-szolgáltatás figyelése](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
* [EventFlow dokumentáció](https://github.com/Azure/diagnostics-eventflow)


<!--Image references-->
[1]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/eventflow-nugets.png
[2]: ./media/service-fabric-diagnostics-collect-logs-without-an-agent/ai-traces.png
