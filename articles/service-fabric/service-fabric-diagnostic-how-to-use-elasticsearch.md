---
title: "a Service Fabric-alkalmazás nyomkövetési tároló Elasticsearch aaaUsing |} Microsoft Docs"
description: "Ismerteti a Service Fabric-alkalmazások használatát Elasticsearch és Kibana toostore, Tárgymutató és kereső alkalmazás nyomkövetési adatokat (naplókat) keresztül"
services: service-fabric
documentationcenter: .net
author: karolz-ms
manager: adegeo
editor: 
ms.assetid: e59b0c39-e468-4d9e-b453-d5f2a8ad20d8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: karolz@microsoft.com
redirect_url: /azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow
ms.openlocfilehash: b5977c54e69319e3caa376e44a02f971b66a3254
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Használjon Elasticsearch, mint a Service Fabric-alkalmazás nyomkövetési tárolásához
## <a name="introduction"></a>Bevezetés
Ez a cikk ismerteti, hogyan [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric/) alkalmazások **Elasticsearch** és **Kibana** nyomkövetési alkalmazástárolót, indexelő és keresési. [Elasticsearch](https://www.elastic.co/guide/index.html) egy nyílt forráskódú, elosztott és méretezhető valós idejű keresési és az elemzések motor, amely jól használható a feladathoz. Microsoft Azure-ban futó Windows és Linux rendszerű virtuális gépekre telepíthető. Elasticsearch hatékonyan tud feldolgozni *strukturált* technológiák használatával például előállított nyomkövetési **esemény Windows (nyomkövetés)**.

A Service Fabric futásidejű toosource diagnosztikai adatok (nyomkövetés készíthet) ETW használja. Hello ajánlott módszer a Service Fabric-alkalmazások toosource a diagnosztikai adatokat túl. Ugyanazt a mechanizmust lehetővé teszi, hogy a futtatókörnyezet által biztosított és az alkalmazás által átadott nyomkövetéseket és hibaelhárítást teszi közötti összefüggést hello segítségével. Service Fabric a Visual Studio projektsablonjai közé tartozik a naplózás alkalmazásprogramozási Felületet (hello .NET alapján **EventSource** osztály), amely ETW-nyomkövetési bocsát ki alapértelmezés szerint. Service Fabric-alkalmazás általános áttekintést nyomkövetés használatával ETW, lásd: [figyelése és diagnosztizálása szolgáltatásai a helyi számítógép fejlesztési telepítő](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Hello nyomkövetések tooshow fel a Elasticsearch, a szükséges toobe rögzített hello Service Fabric fürt csomópontjai valós időben (közben hello alkalmazás fut), és elküldi a tooan Elasticsearch végpont. Nyomkövetés rögzítésének két fő lehetőség áll rendelkezésre:

* **A folyamaton belüli nyomkövetés rögzítése**  
  hello alkalmazás vagy szolgáltatás folyamattal, pontosabban, kimenő hello diagnosztikai toohello nyomkövetési adattár (Elasticsearch) küldéséért felelős.
* **Folyamaton-nyomkövetés rögzítése**  
  Egy különálló ügynök hello szolgáltatás folyamat vagy folyamatok nyomok rögzítését, és elküldi őket toohello nyomkövetési tároló.

Az alábbiakban, azt hogyan tooset be a Azure-ban Elasticsearch tárgyalják hello informatikai szakemberek és leírása mindkét hátrányait beállítások rögzítése, és azt ismertetik, hogyan tooconfigure a Service Fabric szolgáltatás toosend adatok tooElasticsearch

## <a name="set-up-elasticsearch-on-azure"></a>Az Azure-on Elasticsearch beállítása
hello legegyszerűbb módja tooset hello Elasticsearch szolgáltatás az Azure-on keresztül történik [ **Azure Resource Manager-sablonok**](../azure-resource-manager/resource-group-overview.md). Egy átfogó [Elasticsearch gyors üzembe helyezés Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) érhető el az Azure gyors üzembe helyezési sablonok tárházba. Ez a sablon külön tárfiókok használja a méretezési egységeket (csomópontok csoportokat). Is kiépíthetők külön ügyfél és kiszolgáló-csomópontokat a különböző konfigurációt és különböző számú adatlemezt csatolni.

Itt egy másik sablont, úgynevezett használjuk **ES-MultiNode** a hello [Azure diagnosztikai eszközök tárház](https://github.com/Azure/azure-diagnostics-tools). Ez a sablon könnyebb toouse, és az alapvető HTTP-hitelesítés által védett Elasticsearch fürtöt hoz létre. Mielőtt továbblépne, töltik le a hello tárház GitHub tooyour gépről (hello tárház klónozása, vagy egy zip-fájl letöltése). ES-MultiNode hello sablon hello hello mappában található ugyanazzal a névvel.

### <a name="prepare-a-machine-toorun-elasticsearch-installation-scripts"></a>Készítse elő a gép toorun Elasticsearch telepítési parancsfájlokat
hello legegyszerűbb módja toouse hello ES-MultiNode sablon van keresztül nevű megadott Azure PowerShell parancsfájl `CreateElasticSearchCluster`. toouse ezt a parancsfájlt, és van szükség tooinstall PowerShell-modulok nevű eszköz **openssl**. Ez utóbbi hello fürtöt hoz létre, amely használt tooadminister SSH-kulcs a Elasticsearch távolról van szükség.

`CreateElasticSearchCluster`Könnyű használat hello ES-MultiNode sablon a Windows-gépről készült parancsfájl. Lehetséges toouse hello sablon egy-Windows számítógépen, de a forgatókönyv nem ez a cikk hello terjed.

1. Ha még nem telepítette őket már, telepítse [ **Azure PowerShell-modulok**](http://aka.ms/webpi-azps). Amikor a rendszer kéri, kattintson a **futtatása**, majd **telepítése**. Az Azure PowerShell 1.3-as vagy újabb rendszer szükséges.
2. Hello **openssl** eszköz megtalálható a hello terjesztési [ **Git for Windows**](http://www.git-scm.com/downloads). Ha még nem tette meg, telepítse [Git for Windows](http://www.git-scm.com/downloads) most. (hello alapértelmezett telepítési beállítások: OK gombra.)
3. Feltételezve, hogy a Git telepítve, de a hello elérési útja nem szerepel, nyissa meg a Microsoft Azure PowerShell ablakot, és futtassa a következő parancsok hello:
   
    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```
   
    Cserélje le a hello `<Git installation folder>` hello Git a számítógépen; helyen lévő hello alapértelmezett érték a **"C:\Program Files\Git"**. Vegye figyelembe a hello pontosvesszővel hello első elérési hello elején.
4. Győződjön meg arról, hogy van-e bejelentkezve tooAzure (keresztül [ `Add-AzureRmAccount` ](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag) és, hogy megadta-e, amelyeket hello előfizetés használt toocreate a rugalmas keresési fürt. Ellenőrizheti, hogy megfelelő előfizetés van kiválasztva, használatával `Get-AzureRmContext` és `Get-AzureRmSubscription` parancsmagok.
5. Ha még nem tette meg, módosíthatja a hello aktuális directory toohello ES-MultiNode mappát.

### <a name="run-hello-createelasticsearchcluster-script"></a>Hello CreateElasticSearchCluster parancsfájl futtatása
Hello parancsfájl futtatása előtt nyissa meg a hello `azuredeploy-parameters.json` fájlt, és győződjön meg arról, vagy hello parancsfájl paraméterek értékének megadására. a következő paraméterek hello itt találhatók:

| Paraméter neve | Leírás |
| --- | --- |
| dnsNameForLoadBalancerIP |hello nevet használt toocreate hello nyilvánosan láthatóvá DNS-nevét hello rugalmas keresési fürt (hello Azure-régiót toohello megadott tartománynév hozzáfűzésével). Például ha a paraméter értéke "myBigCluster", és a kiválasztott hello Azure-régió az USA nyugati régiója, hello eredményül kapott DNS-neve hello fürt myBigCluster.westus.cloudapp.azure.com:. <br /><br />Ez a név is szolgál az hello rugalmas keresési fürtben, mint az adatok csomópontnevek társított sok összetevők legfelső szintű nevét. |
| adminUsername |hello rugalmas keresési fürt kezeléséhez hello hello rendszergazdai fiók nevét (a megfelelő SSH-kulcsok automatikusan jönnek létre). |
| dataNodeCount |hello hello rugalmas keresési fürtben található csomópontok számát. hello hello parancsfájl jelenlegi verziója nem különbözteti meg adatokat és a lekérdezés csomópontok; minden csomópont mindkét szerepet. Alapértelmezett too3 csomópontok. |
| dataDiskSize |hello mérete (GB) adatlemezek az egyes adatcsomóponton lefoglalt. Minden csomópont kap 4 adatlemezek, kizárólag dedikált tooElastic keresési szolgáltatást. |
| Régió |Azure-régió, ahol kell lennie a hello rugalmas keresési fürt hello neve. |
| esUserName |hello felhasználónév, amely hello felhasználó konfigurálva toohave hozzáférés tooES fürt (tulajdonos tooHTTP egyszerű hitelesítés). hello jelszó paraméterfájl nem része, és meg kell adni, ha `CreateElasticSearchCluster` parancsfájl hívják. |
| vmSizeDataNodes |hello rugalmas keresési fürtcsomópontok esetén, az Azure virtuális gép méretét. Alapértelmezett tooStandard_D2. |

Most már készen áll a toorun hello parancsfájl áll. A probléma hello a következő parancsot:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

Ha 

| Parancsfájl-paraméter neve | Leírás |
| --- | --- |
| `<es-group-name>` |hello összes rugalmas keresési fürterőforrás tartalmazó hello Azure erőforráscsoport nevét. |
| `<azure-region>` |hello neve hello Azure-régió, ahol hello rugalmas keresési fürt kell létrehozni. |
| `<es-password>` |hello hello rugalmas keresési felhasználó jelszavát. |

> [!NOTE]
> Ha egy NullReferenceException hello teszt-használják parancsmag, a tooAzure toolog elfelejtette (`Add-AzureRmAccount`).
> 
> 

Ha hibaüzenetet kap hello parancsfájl futtatását, és annak meghatározását, hogy hello hiba okozta-e a megfelelő sablon paraméter értékét, hello paraméterfájl javítsa ki, és futtassa újra hello parancsfájl másik erőforráscsoport-nevet. Is felhasználhatja hello ugyanazt az erőforráscsoport neve és karbantartása hello régi hello parancsfájl rendelkezik hello hozzáadásával `-RemoveExistingResourceGroup` paraméter toohello parancsfájl hívása.

### <a name="result-of-running-hello-createelasticsearchcluster-script"></a>Hello CreateElasticSearchCluster parancsprogram futtatása eredménye
Miután lefuttatta a hello `CreateElasticSearchCluster` parancsfájl, a következő fő összetevők hello jön létre. Az ebben a példában feltételezzük, hogy használja a "myBigCluster" hello hello értékeként `dnsNameForLoadBalancerIP` paraméter adott hello régióban, amelyben létrehozta hello fürt pedig USA nyugati régiója.

| Összetevő | Nevét, helyét és megjegyzések |
| --- | --- |
| SSH-kulcs Távfelügyelet |myBigCluster.key fájl (hello CreateElasticSearchCluster lett futtassa mely hello). <br /><br />A kulcsfájl lehet használt tooconnect toohello admin csomópont és (keresztül hello admin csomópont) toodata hello fürt csomópontja. |
| Felügyelet munkaterület |myBigCluster-admin.westus.cloudapp.azure.com <br /><br />A dedikált virtuális gépek Elasticsearch fürt Távfelügyelet – hello csak egyet, amely lehetővé teszi, hogy a külső SSH-kapcsolatokhoz. Az összes hello Elasticsearch fürtcsomópontok, de az azonos virtuális hálózaton does hello Elasticsearch szolgáltatások futtassa fut. |
| Adatok csomópontok |myBigCluster1... myBigCluster*N* <br /><br />Adatok csomópontok Elasticsearch és Kibana szolgáltatások futnak. SSH tooeach csomópont keresztül, de csak hello admin csomópont keresztül is elérheti. |
| Elasticsearch fürt |http://myBigCluster.westus.cloudapp.Azure.com/es/ <br /><br />hello elsődleges végpont hello Elasticsearch fürthöz (Megjegyzés: hello /es utótag). Alapszintű HTTP-hitelesítés védi (volt hello megadott esUserName/esPassword paraméterek hello ES-MultiNode sablon hello hitelesítő adatok). hello fürt rendelkezik is hello head beépülő modul telepítve (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) alapvető Fürtfelügyelet. |
| Kibana szolgáltatás |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />hello Kibana szolgáltatás tooshow adatok beállítása a hello Elasticsearch fürtöt létrehozni. Hello védi, hello ugyanazon hitelesítő fürt magát. |

## <a name="in-process-versus-out-of-process-trace-capturing"></a>A folyamat és a folyamaton a nyomkövetés rögzítése
Hello bevezetésben azt említett két alapvető módokat diagnosztikai adatok gyűjtése: a folyamat és a folyamaton. Mindegyik rendelkezik-e előnyeiről és hátrányairól.

Hello előnyei **folyamaton belüli nyomkövetés rögzítése** tartalmazza:

1. *Egyszerű konfiguráció és a központi telepítés*
   
   * diagnosztikai adatok gyűjtésének hello konfigurálása most nem része az hello alkalmazás konfigurációja. Egyszerű tooalways megőrzése "szinkronizálva" hello, rest-hello alkalmazás is.
   * Alkalmazás vagy a szolgáltatás konfigurációja, könnyen elérhető.
   * Folyamaton-nyomkövetés rögzítése általában külön telepítési és a szükséges konfigurációs hello diagnosztikai ügynök, amely extra felügyeleti feladatot, és a hibák lehetséges forrását. hello adott ügynök technológia gyakran lehetővé teszi, hogy a virtuális gépenként (csomópont) hello ügynök csak egy példánya. Ez azt jelenti, hogy a konfigurálás hello diagnosztikai konfiguráció hello gyűjtemény összes alkalmazás által közösen, és ezen a csomóponton futó szolgáltatásokat.
2. *Rugalmasság*
   
   * hello alkalmazás adatküldés hello bárhol toogo, kell, amíg nincs egy ügyféloldali kódtár megcélzott hello adatok tárolórendszer támogató. Új mosdók felveheti igény szerint.
   * Összetett rögzítési, szűrési és adatösszesítés szabályok kétféleképpen valósítható meg.
   * Egy folyamaton-nyomkövetés rögzítése gyakran korlátozott általi hello adatok, amelyek hello ügynök támogatja. Néhány ügynökök olyan bővíthető.
3. *Hozzáférés toointernal alkalmazásadatok és a környezetben*
   
   * hello diagnosztikai alrendszer hello alkalmazás/kiszolgáló folyamat belül futó könnyen is kiegészítheti hello nyomkövetések környezetfüggő adatokkal.
   * Hello-folyamaton kívüli módszert használja, a hello adatokat kell elküldeni tooan ügynök keresztül bizonyos folyamatok közti kommunikációs mechanizmus, például a Windows esemény-nyomkövetése. Ez az eljárás további sikerült korlátozható.

Hello előnyei **folyamaton-nyomkövetés rögzítése** tartalmazza:

1. *képes toomonitor hello alkalmazás hello és összeomlási memóriaképek összegyűjtése*
   
   * A folyamaton belüli nyomkövetés rögzítése sikertelen lehet, ha hello alkalmazás toostart meghibásodik, vagy összeomlik. Független ügynök sokkal nagyobb lehetséges, hogy a kritikus fontosságú hibaelhárítási információkat tartalmaz.<br /><br />
2. *Lejárat robusztusság és bevált teljesítmény*
   
   * Az ügynök (például egy Microsoft Azure diagnosztikai ügynök) platform szállító által fejlesztett tulajdonos toorigorous tesztelése és túlélésért folyó-korlátozási lett.
   * A folyamaton belüli nyomkövetési rögzítését, a gondot kell fordítani a tooensure, hogy elküldi a diagnosztikai adatokat az alkalmazás folyamatának hello tevékenység nem hello alkalmazás fő feladatok megzavarhatja vagy időzítés vagy a teljesítmény problémák bevezetni. Egymástól függetlenül futó ügynök kevesebb a hibalehetőség toothese problémák és tervezett toolimit kifejezetten a hello rendszer kifejtett hatását.

Ez nem lehetséges toocombine és mindkét megközelítés hasznos. Ezenkívül előfordulhat, hogy számos alkalmazás hello legjobb megoldást.

Itt használjuk hello **Microsoft.Diagnostic.Listeners könyvtár** és hello folyamaton belüli követéséhez fürtről a Service Fabric application tooan Elasticsearch toosend adatok rögzítéséhez.

## <a name="use-hello-listeners-library-toosend-diagnostic-data-tooelasticsearch"></a>Hello figyelői könyvtár toosend diagnosztikai adatok tooElasticsearch használata
hello Microsoft.Diagnostic.Listeners könyvtár PartyCluster minta Service Fabric-alkalmazás részét képezi. toouse azt:

1. Töltse le [hello PartyCluster minta](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) a Githubról.
2. Hello Microsoft.Diagnostics.Listeners és Microsoft.Diagnostics.Listeners.Fabric projektek (teljes mappák) másolása hello PartyCluster minta toohello megoldás könyvtármappát toosend hello adatok tooElasticsearch kellene hello-alkalmazás .
3. Nyissa meg hello cél megoldást, kattintson a jobb gombbal a Megoldáskezelőben hello hello megoldás csomópontja, és válassza **létező projekt hozzáadása**. Adja hozzá a hello Microsoft.Diagnostics.Listeners projekt toohello megoldás. Az ismételt hello azonos hello Microsoft.Diagnostics.Listeners.Fabric projekt.
4. Vegye fel a projektbe egy hivatkozást a szolgáltatás projekt(ek) toohello két hozzáadott projektek. (Minden egyes szolgáltatás toosend adatok tooElasticsearch kellene hivatkoznia kell Microsoft.Diagnostics.EventListeners és Microsoft.Diagnostics.EventListeners.Fabric).
   
    ![Projekt hivatkozik tooMicrosoft.Diagnostics.EventListeners és Microsoft.Diagnostics.EventListeners.Fabric könyvtárak][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Service Fabric általános rendelkezésre állási kiadás és Microsoft.Diagnostics.Tracing Nuget-csomag
Service Fabric általános rendelkezésre állási kiadásban (2.0.135, 2016. március 31-én kiadott) készült alkalmazások cél **.NET-keretrendszer 4.5.2-es**. Ezen verziója hello legmagasabb hello hello GA kiadás hello időben Azure által támogatott .NET-keretrendszer verzióját. Sajnos hello keretrendszer ezen verziója nem rendelkezik bizonyos eseményfigyelő Visszahívásán API-k, hogy hello Microsoft.Diagnostics.Listeners könyvtárban van szüksége. (A háló alkalmazások API-k naplózás hello alapját képező hello összetevő) EventSource és eseményfigyelő Visszahívásán szorosan összekapcsolt, mert minden olyan projekthez, hello Microsoft.Diagnostics.Listeners függvénytár kell használnia egy alternatív implementáció a EventSource. Ez a megvalósítás hello által biztosított **Microsoft.Diagnostics.Tracing Nuget-csomag**, Microsoft által fejlesztett. hello csomag teljesen visszamenőlegesen kompatibilis EventSource szerepel hello framework, ezért nem kódmódosításokat szükséges hivatkozott névtér végrehajtott kell lennie.

toostart hello Microsoft.Diagnostics.Tracing megvalósításával hello EventSource osztályból, kövesse az alábbi lépéseket kell toosend adatok tooElasticsearch minden szolgáltatás projekt esetében:

1. Kattintson jobb gombbal a hello projektet, és válassza a **Nuget-csomagok kezelése**.
2. Váltás toohello nuget.org csomag forrása (Ha még nincs kiválasztva), és keressen a "**Microsoft.Diagnostics.Tracing**".
3. Telepítse a hello `Microsoft.Diagnostics.Tracing.EventSource` csomagot (és függőségeinek).
4. Nyissa meg hello **ServiceEventSource.cs** vagy **ActorEventSource.cs** szolgáltatásprojektre fájlt, és cserélje le a hello `using System.Diagnostics.Tracing` irányelv fölött hello hello fájl `using Microsoft.Diagnostics.Tracing` direktíva.

Ezeket a lépéseket csak akkor szükséges egyszer hello **.NET-keretrendszer 4.6** Microsoft Azure támogatja.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch figyelő által okozott és konfigurálása
hello tooElasticsearch diagnosztikai adatok küldése az utolsó lépés példánya toocreate: `ElasticSearchListener` konfigurálása Elasticsearch kapcsolati adatait. hello figyelő automatikusan a hello szolgáltatás projektben EventSource osztályba keresztül kiváltott összes eseményeket rögzíti. Jogcímadatokat toobe hello szolgáltatás hello élettartama során, ajánlott hello helyezze toocreate hello szolgáltatás inicializálása kódban van. Ez az állapotmentes szolgáltatások hello inicializálási kód sikerült megjelenését hello szükséges módosítása után (kiegészítéseket jelölt kezdve megjegyzések `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add hello following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

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
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider, new FabricHealthReporter("ElasticSearchEventListener"));
                }

                // hello ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name tooa .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of hello class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that hello ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Elasticsearch kapcsolati adatait kell rendezni egy külön szakaszban hello szolgáltatás konfigurációs fájljában (**PackageRoot\Config\Settings.xml**). hello szakasz hello nevét meg kell felelnie a toohello átadott toohello érték `FabricConfigurationProvider` konstruktor, például:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
az értékek hello `serviceUri`, `userName` és `password` paraméterek megfelelnek toohello Elasticsearch fürt végpontcím Elasticsearch felhasználónevet és jelszót, illetve. `indexNamePrefix`hello előtag Elasticsearch indexek; hello Microsoft.Diagnostics.Listeners library naponta a saját adataikhoz új index létrehozása.

### <a name="verification"></a>Ellenőrzés
Ennyi az egész! Most hello szolgáltatás fut, amikor elindítja nyomkövetések toohello Elasticsearch szolgáltatás hello konfigurációjában megadott küldésekor. Ellenőrizheti a nyitó hello hello cél Elasticsearch példányához tartozó Kibana felhasználói felület. A jelen példában hello lap címe http://myBigCluster.westus.cloudapp.azure.com/. Ellenőrizze, hogy az hello választott hello neve előtaggal indexeli `ElasticSearchListener` példány valóban létrehozott és adatokkal feltöltve.

![Kibana ábrázoló PartyCluster alkalmazásesemények][2]

## <a name="next-steps"></a>Következő lépések
* [További információk felderítésére, és a Service Fabric-szolgáltatás figyelése](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
