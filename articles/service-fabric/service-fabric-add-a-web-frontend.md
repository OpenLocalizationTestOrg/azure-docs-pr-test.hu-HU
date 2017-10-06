---
title: "aaaCreate előtér az ASP.NET Core segítségével Azure Service Fabric-alkalmazás |} Microsoft Docs"
description: "A Service Fabric-alkalmazás toohello webes tenni egy ASP.NET Core projektet és a szolgáltatások közötti kommunikáció keresztül a távelérési szolgáltatás használatával."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: 0c4454d6cac4c2e343bd6e93e56d3d2f0ebfc4ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>A szolgáltatás előtér-webkiszolgáló az ASP.NET Core segítségével alkalmazás létrehozása
Alapértelmezés szerint az Azure Service Fabric szolgáltatások nem biztosítanak a nyilvános csatoló toohello webes. tooexpose az alkalmazás funkcióinak tooHTTP ügyfelek rendelkezik egy webes projekt tooact belépési pontként, és ott tooyour majd kommunikációt toocreate adott szolgáltatást.

Az oktatóanyag azt átvételéhez ahol azt korábban befejezte a hello [az első alkalmazás létrehozásának a Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) oktatóanyagot, és adja hozzá az ASP.NET Core webszolgáltatás elé hello állapot-nyilvántartó teljesítményszámláló szolgáltatást. Ha még nem tette meg, lépjen vissza, és, hogy az oktatóanyag első lépéseit.

## <a name="set-up-your-environment-for-aspnet-core"></a>Állítsa be a környezetet az ASP.NET Core

A Visual Studio 2017 toofollow ebben az oktatóanyagban együtt van szüksége. Bármely edition fog tenni. 

 - [Telepítse a Visual Studio 2017](https://www.visualstudio.com/)

az ASP.NET Core Service Fabric-alkalmazások toodevelop, rendelkeznie kell a következő telepített feladatok hello:
 - **Azure fejlesztési** (alatt *webes & felhő*)
 - **Az ASP.NET és a webes fejlesztési** (alatt *webes & felhő*)
 - **A .NET core platformfüggetlen fejlesztésekhez** (alatt *egyéb Toolsets*)

>[!Note] 
>hello .NET Core tools for Visual Studio 2015 már nem frissítés alatt álló, de a Visual Studio 2015 még használja, ha szüksége van-e toohave [.NET Core VS 2015 tooling eszköz Preview 2](https://www.microsoft.com/net/download/core) telepítve.

## <a name="add-an-aspnet-core-service-tooyour-application"></a>Az ASP.NET Core szolgáltatás tooyour alkalmazás hozzáadása
Az ASP.NET Core egy egyszerűsített, platformfüggetlen webes fejlesztési keretrendszer, hogy toocreate modern webes felhasználói Felületét használja, és webes API-khoz. 

ASP.NET webes API projektet tooour meglévő alkalmazás adjuk hozzá.

1. A Megoldáskezelőben kattintson a jobb gombbal **szolgáltatások** belül hello projektet, és válassza a **Hozzáadás > új Service Fabric-szolgáltatás**.
   
    ![Új szolgáltatás tooan meglévő alkalmazás hozzáadása][vs-add-new-service]
2. A hello **szolgáltatás létrehozása** lapon, válassza ki **ASP.NET Core** és adjon neki egy nevet.
   
    ![ASP.NET webszolgáltatás kiválasztásával hello új szolgáltatás párbeszédpanelje][vs-new-service-dialog]

3. hello következő oldal biztosít az ASP.NET Core projektsablonjai. Vegye figyelembe, hogy ezek az vannak hello azonos lehetősége, hogy mutatunk be Ha kis mennyiségű kód tooregister hozott létre az ASP.NET Core projektek kívül a Service Fabric-alkalmazás hello szolgáltatást a Service Fabric-futtatókörnyezet hello. A jelen oktatóanyag esetében válassza ki a **Web API**. Azonban alkalmazhat hello azonos fogalmak toobuilding teljes webalkalmazás.
   
    ![ASP.NET-projekt kiválasztása][vs-new-aspnet-project-dialog]
   
    A Web API-projekt létrehozása után az alkalmazás rendelkeznie kell két szolgáltatást. Az alkalmazás továbbra toobuild, adhat hozzá további szolgáltatások hello pontosan azonos módon. Minden egyes lehet függetlenül rendszerverzióval ellátott és frissített.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
Mi azt régebben már kötöttek, most egyfajta tooget hello új alkalmazás és egy pillantást hello alapértelmezett viselkedését, hogy az ASP.NET Core Web API sablon hello biztosít hajtsa végre a megfelelő telepítéséhez.

1. Nyomja le az F5 billentyűt a Visual Studio toodebug hello alkalmazás.
2. Központi telepítés befejeződése után a Visual Studio elindítja egy böngésző toohello gyökérmappájában hello ASP.NET Web API-szolgáltatás. hello ASP.NET Core webes API-sablon nem biztosítanak alapértelmezett viselkedése hello gyökér, így hello böngészőben 404 hibaüzenetet kell megjelennie.
3. Adja hozzá `/api/values` hello böngészőben toohello helyét. Ez elindítja a hello `Get` hello ValuesController hello Web API sablonban metódust. Hello alapértelmezett válasz hello sablon – két karakterláncokat tartalmazó JSON-tömb által biztosított adja vissza:
   
    ![Az ASP.NET Core Web API sablonból visszaadott alapértelmezett értékek][browser-aspnet-template-values]
   
    Hello oktatóanyag végére hello, ezen a lapon fog hello hello helyett az állapotalapú szolgáltatásból legutóbbi számlálóérték alapértelmezett karakterláncok megjelenítése.

## <a name="connect-hello-services"></a>Connect hello szolgáltatások
A Service Fabric hogyan kommunikáljanak megbízható szolgáltatások teljes rugalmasságot biztosít. Egyetlen alkalmazásban lehetséges, hogy szolgáltatásokat, amelyek a TCP, más szolgáltatásokkal, hogy elérhető-e egy HTTP REST API-n keresztül, és továbbra is más szolgáltatások, amelyek elérhetők a webes szoftvercsatornák keresztül érhető el. Háttér hello lehetőségeit és hello mellékhatásokkal jár, lásd: [szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md). Ebben az oktatóanyagban használjuk [Service Fabric szolgáltatás távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md), az hello SDK megadott.

Hello szolgáltatás távoli eljáráshívási megközelítés (tapasztalatait távoli eljáráshívások vagy RPC-k) akkor határozza meg egy felület tooact hello nyilvános szerződés hello szolgáltatás. Ezt követően használja a adott felület toogenerate proxyosztály hello szolgáltatás való kommunikáció.

### <a name="create-hello-remoting-interface"></a>Hello távelérési kapcsolat létrehozása
Először hozzon létre hello felület tooact hello szerződés hello állapotalapú szolgáltatási és egyéb szolgáltatások, a nagybetűk hello ASP.NET Core webes projekt között. Ez az interfész által az összes azt használó toomake RPC-hívások, így a saját Class Library projektben létrehozunk szolgáltatások kell osztani.

1. A Megoldáskezelőben kattintson a jobb gombbal a megoldás, és válassza a **Hozzáadás** > **új projekt**.

2. Válassza ki a hello **Visual C#** bal oldali navigációs panelen, és jelölje ki hello hello bejegyzést **Class Library** sablont. Győződjön meg arról, hogy hello .NET-keretrendszer verziója túl van-e állítva**4.5.2-es**.
   
    ![Az állapotalapú szolgáltatás felület projekt létrehozása][vs-add-class-library-project]

3. Telepítse a hello **Microsoft.ServiceFabric.Services.Remoting** NuGet-csomagot. Keresse meg **Microsoft.ServiceFabric.Services.Remoting** a hello NuGet csomagot kezelő majd telepítse hello megoldás az összes olyan projekt, amelyek a távelérés szolgáltatás, beleértve:
   - hello szolgáltatásfelület tartalmazó hello Class Library projekt
   - hello állapotalapú Service-projekt
   - az ASP.NET Core webes projekt hello
   
    ![Hello szolgáltatások a NuGet csomag hozzáadása][vs-services-nuget-package]

4. Hello osztály könyvtárban hozzon létre egy felület egy módszert, `GetCountAsync`, és hello felületének a kiterjesztése `Microsoft.ServiceFabric.Services.Remoting.IService`. hello távelérési kapcsolat kell származnia: az illesztőfelület tooindicate, hogy a rendszer a szolgáltatás távoli eljáráshívási felület.
   
    ```c#
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-hello-interface-in-your-stateful-service"></a>Az állapot-nyilvántartó szolgáltatásban hello illesztőfelület megvalósítása
Most, hogy definiáltuk hello felületet, tooimplement kell azt a hello állapotalapú szolgáltatásból.

1. Az állapotalapú szolgáltatás hozzá egy hivatkozást toohello hordozhatóosztálytár-projektjének hello felület tartalmazó.
   
    ![Egy hivatkozási toohello hordozhatóosztálytár-projektjének hello állapotalapú szolgáltatás hozzáadása][vs-add-class-library-reference]
2. Keresse meg a hello osztályból származó `StatefulService`, például a `MyStatefulService`, és kiterjesztése tooimplement hello `ICounter` felületet.
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. Most valósítja meg hello egyetlen hello definiált `ICounter` felületet, `GetCountAsync`.
   
    ```c#
    public async Task<long> GetCountAsync()
    {
        var myDictionary = 
            await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### <a name="expose-hello-stateful-service-using-a-service-remoting-listener"></a>Hello állapotalapú szolgáltatás használata a távoli eljáráshívási szolgáltatás figyelő indításához
A hello `ICounter` megvalósított hello utolsó lépésként objektumfelület tooopen hello szolgáltatás távelérési kommunikációs csatorna. Állapotalapú szolgáltatások esetén a Service Fabric nevű felülbírálható módszert kínál `CreateServiceReplicaListeners`. Ezzel a módszerrel megadhatja egy vagy több kommunikációs figyelőket, megjeleníteni kívánt tooenable a szolgáltatás kommunikációs hello típusa alapján.

> [!NOTE]
> hello egyenértékű módszer nyithatók meg a kommunikációs csatorna toostateless szolgáltatások neve `CreateServiceInstanceListeners`.

Ebben az esetben azt cseréli a meglévő hello `CreateServiceReplicaListeners` metódust, és adjon meg egy példánya `ServiceRemotingListener`, ami, amely létrehoz egy RPC-végpont metódus akkor hívható meg az ügyfelek a `ServiceProxy`.  

Hello `CreateServiceRemotingListener` hello bővítmény metódusa `IService` felület lehetővé teszi a tooeasily hozzon létre egy `ServiceRemotingListener` alapértelmezett beállításokkal. toouse a kiterjesztésmetódus, ellenőrizze, hogy hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` importált névtér. 

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-hello-serviceproxy-class-toointeract-with-hello-service"></a>Hello ServiceProxy osztály toointeract hello szolgáltatás használatához
Az állapotalapú szolgáltatás már készen áll a tooreceive forgalom más szolgáltatások RPC-n keresztül. Így továbbra is hozzáadásával hello kód toocommunicate vele az ASP.NET-webszolgáltatás hello.

1. Az ASP.NET projektben adja hozzá egy hivatkozást toohello osztálytár hello tartalmazó `ICounter` felületet.

2. Korábban hozzáadott hello **Microsoft.ServiceFabric.Services.Remoting** NuGet csomag toohello ASP.NET-projekt. Ez a csomag biztosít hello `ServiceProxy` osztály, amely toomake RPC meghívja a toohello állapot-nyilvántartó szolgáltatást. Győződjön meg arról, a NuGet-csomagja telepítve van az ASP.NET Core webes projekt hello.

4. A hello **tartományvezérlők** mappa, nyissa meg hello `ValuesController` osztály. Vegye figyelembe, hogy hello `Get` metódus jelenleg csak egy "érték1" és "érték2" – amely megfelel a mi hello böngészőben korábban látott kódolt karakterlánc-tömbben ad vissza. Ez a megvalósítás cserélje le a következő kód hello:
   
    ```c#
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    hello első kódsort hello kulcs. toocreate hello ICounter toohello állapot-nyilvántartó proxyszolgáltatás, kell tooprovide kétféle információt: hello szolgáltatás egy partíció azonosítója és hello nevét.
   
    Particionálási tooscale állapotalapú szolgáltatások használatához összeállításának be különböző gyűjtők állapotukra egy definiálhat, például a felhasználói azonosító vagy irányítószámot kulcs alapján. Trivial alkalmazás, a hello állapotalapú szolgáltatásból csak rendelkezik egy partíciót, így hello kulcs nem számít. Bármely Ön által biztosított kulcs irányítja toohello egyazon partícióra kerüljenek. toolearn szolgáltatások, particionálás bővebben lásd: [hogyan toopartition Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).
   
    hello szolgáltatás neve megkülönbözteti a hello űrlap háló URI: /&lt;$application_name&gt;/&lt;service_name&gt;.
   
    Két adatot, a Service Fabric egyedileg azonosíthatja a hello gép kérelmek kell küldeni. Hello `ServiceProxy` osztály is gördülékenyen kezeli hello esetben hello állapotalapú szolgáltatási partíció üzemeltető hello gép sikertelen lesz, és egy másik gépen kell lennie az előléptetett tootake helyére. Ez az absztrakció révén írás hello ügyfél kód toodeal jelentősen egyszerűbb más szolgáltatásokkal.
   
    Miután tudunk hello proxy, azt egyszerűen meghívása hello `GetCountAsync` metódus és az eredményt adja vissza.

5. Nyomja le az F5 újra toorun hello módosított alkalmazás. Mint előtt, a Visual Studio automatikusan elindítja hello böngésző toohello hello webes projekt gyökerében. Hello "API-t és-értékek" elérési út, és megtekintheti az hello aktuális számlálóérték adott vissza.
   
    ![hello állapot-nyilvántartó számlálóérték hello böngészőben jelenik meg][browser-aspnet-counter-value]
   
    Frissítse a böngészőt hello rendszeres időközönként a toosee hello számláló értéke frissítés.

## <a name="kestrel-and-weblistener"></a>Vércse és WebListener

hello alapértelmezett ASP.NET Core webkiszolgáló, vércse, úgynevezett van [jelenleg nem támogatott a közvetlen internet-forgalom kezelésére](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel). Ennek eredményeképpen hello ASP.NET Core állapotmentes szolgáltatások a sablon a Service Fabric által [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) alapértelmezés szerint. 

a Service Fabric szolgáltatások bővebben vércse és WebListener toolearn tekintse meg a túl[ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="connecting-tooa-reliable-actor-service"></a>Csatlakozás tooa megbízható szereplő szolgáltatás
Ez az oktatóanyag összpontosított hozzáadása egy előtér-webkiszolgáló, amely egy állapotalapú szolgáltatással továbbítani. Egy nagyon hasonló modell tootalk tooactors is végrehajthatja. Visual Studio megbízható szereplő projekt létrehozása, automatikusan létrehoz egy illesztőfelület-projekt meg. Használhatja a felület toogenerate az aktor proxy hello webes projekt toocommunicate hello résztvevővel. hello kommunikációs csatorna számára automatikusan biztosított. Így toodo nem kell semmi, hogy-e megfelelő tooestablishing egy `ServiceRemotingListener` például ebben az oktatóanyagban hello állapotalapú szolgáltatás esetében tette.

## <a name="how-web-services-work-on-your-local-cluster"></a>A helyi fürtön lévő webes szolgáltatások működése
Általánosságban elmondható pontosan hello azonos a Service Fabric application tooa több gép fürtön, amikor központilag telepített a helyi fürthöz, és lehet magas benne, hogy működik a várt módon telepítheti. Ez azért, mert a helyi fürt egyszerűen egy öt-csomópont-konfiguráció, amely összecsukott tooa egyetlen számítógépen.

Tooweb szolgáltatások ismét, azonban nincs egy kulcs nuance. Ha a fürt egy terheléselosztó mögött helyezkedik el, mint az Azure-ban, győződjön meg arról, hogy a webes szolgáltatások telepítve vannak minden egyes számítógépen óta hello terheléselosztó egyszerűen ciklikus multiplexelések hello gépek közötti forgalmat. Ehhez által hello beállítása `InstanceCount` hello szolgáltatás toohello különleges érték-1".

Ezzel szemben, amikor helyileg futtat egy webszolgáltatás-bővítmény, kell tooensure hello szolgáltatást, hogy csak egy példányban fut. Ellenkező esetben futtatnia az ütközések több folyamat által figyelt hello ugyanazt az elérési út és a port. Ennek eredményeképpen hello web service példányszám beállításaként túl "1" helyi telepítés esetén.

Hogyan tooconfigure eltérő értékek tartoznak különböző környezetben: toolearn [alkalmazás paramétereinek több környezet kezelése](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Következő lépések
Most, hogy a webalkalmazás első set végül az ASP.NET Core az alkalmazáshoz, további információ [ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) az ASP.NET Core működése a Service Fabric alapos ismeretét.

Ezt követően [tudhat meg többet a szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md) általában a teljes tooget kép, hogyan szolgáltatás a kommunikációt a Service Fabric működik.

Ha elvégezte a szolgáltatások közötti kommunikáció működésével, alaposan megismernie [fürt létrehozása az Azure-ban, és telepítheti az alkalmazás toohello felhő](service-fabric-cluster-creation-via-portal.md).

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
