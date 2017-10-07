---
title: "aaaCreate az első Service Fabric-alkalmazás a C# |} Microsoft Docs"
description: "Bevezetés toocreating egy Microsoft Azure Service Fabric-alkalmazás az állapotmentes és állapotalapú szolgáltatással."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d9b44d75-e905-468e-b867-2190ce97379a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: vturecek
ms.openlocfilehash: e95e67cc84be1b83c936b250cae9112ddc77b963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a>Ismerkedés a Reliable Services használatával
> [!div class="op_single_selector"]
> * [C# Windowson](service-fabric-reliable-services-quick-start.md)
> * [Java Linuxon](service-fabric-reliable-services-quick-start-java.md)
> 
> 

Az Azure Service Fabric-alkalmazás egy vagy több olyan szolgáltatás, amely az Ön kódjának futtatásához tartalmazza. Ez az útmutató bemutatja, hogyan toocreate állapotmentes és állapotalapú mind a Service Fabric-alkalmazások [Reliable Services](service-fabric-reliable-services-introduction.md).  A Microsoft Virtual Academy videó azt is bemutatja, hogyan toocreate állapotmentes működnie:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965">  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a>Alapfogalmak
csak akkor Reliable Services használatába tooget toounderstand néhány főbb fogalmait és kifejezéseit van szükség:

* **Szolgáltatás típusa**: Ez a szolgáltatás megvalósítása. Hello osztályt írt induló `StatelessService` és bármely más kód vagy függőségek használt, valamint nevét és verziószámát.
* **Szolgáltatáspéldány nevű**: toorun a szolgáltatás létrehozása a szolgáltatás típusának nevesített példányai sokkal például létrehozhat egy osztálytípus objektum példánya. A szolgáltatáspéldány hello képernyőn hello segítségével URI névvel rendelkezik "fabric: /" sémáját, például a "fabric: / MyApp/MyService".
* **Szolgáltatásgazda**: hello hoz létre egy gazdafolyamaton belül kell toorun szolgáltatáspéldány nevű. hello szolgáltatás állomás csak az egyik folyamat, ahol a szolgáltatás példányának futtatható.
* **Eszközregisztrációs szolgáltatás**: regisztráció egyesíti mindent. hello szolgáltatástípus regisztrálva kell lennie a Service Fabric hello futásidejű szolgáltatás a gazdagép tooallow Service Fabric toocreate példányai azt toorun.  

## <a name="create-a-stateless-service"></a>Állapot nélküli szolgáltatás létrehozása
Egy állapotmentes szolgáltatások olyan szolgáltatás, amely jelenleg hello alapértelmezetté a felhőalapú alkalmazásokhoz. Ez az állapot nélküli tekintendő, mert hello szolgáltatást tartalmaz, amelyet a toobe megbízhatóan tárolja, vagy a magas rendelkezésre állású adatokat. Ha az állapot nélküli szolgáltatás egy példánya leáll, az összes belső állapotában elvész. Az ilyen típusú szolgáltatás állapotban kell lennie megőrzött tooan külső áruházban, például Azure táblák vagy az SQL-adatbázisban, amíg toobe végzett magas rendelkezésre állású és megbízható.

Indítsa el a Visual Studio 2015-öt vagy a Visual Studio 2017 rendszergazdaként, és hozzon létre egy új Service Fabric-projektet nevű *HelloWorld*:

![Hello új projekt párbeszédpanel bezárásához toocreate egy új Service Fabric-alkalmazás használata](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

Majd létre szeretne hozni egy állapotmentes szolgáltatások nevű projekt *HelloWorldStateless*:

![A hello második párbeszédpanelen állapotmentes szolgáltatások-projekt létrehozása](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

A megoldás most két projektet tartalmaz:

* *HelloWorld*. Ez a hello *alkalmazás* projektet, amely tartalmazza a *szolgáltatások*. Is tartalmaz, amely leírja a az alkalmazás hello alkalmazás, valamint a PowerShell-parancsfájlok, amelyek segítenek toodeploy számos hello alkalmazásjegyzék.
* *HelloWorldStateless*. Ez a hello projektet. Hello állapotmentes szolgáltatások megvalósítási tartalmaz.

## <a name="implement-hello-service"></a>Hello szolgáltatás megvalósítása
Nyissa meg hello **HelloWorldStateless.cs** fájl hello szolgáltatás projektben. A Service Fabric szolgáltatás futtatható bármely üzleti logika. hello szolgáltatás API két belépési pontok biztosít a kódot:

* Egy nyílt belépési pont metódus hívása *RunAsync*, ahol el lehet kezdeni a munkaterheléseket, ideértve a hosszan futó számítási feladatok végrehajtása.

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* Ahol a kommunikációs verem szerkesztőprogramban, például az ASP.NET Core csatlakoztatható kommunikációs belépési pont. Ez azért, ahol elindíthatja kéréseket fogad a felhasználók és más szolgáltatások védelmét.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

Ebben az oktatóanyagban tárgyaljuk hello `RunAsync()` belépési pont metódusa. Ez azért, ahol azonnal elindíthatja a kódja.
hello projektsablon tartalmaz egy mintát végrehajtásának `RunAsync()` , amely növeli a működés közbeni száma.

> [!NOTE]
> Hogyan toowork a kommunikációs verem kapcsolatos részletekért lásd: [Service Fabric Web API szolgáltatások OWIN önálló üzemeltetéséhez.](service-fabric-reliable-services-communication-webapi.md)
> 
> 

### <a name="runasync"></a>RunAsync
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    long iterations = 0;

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        ServiceEventSource.Current.ServiceMessage(this, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

hello platform meghívja ezt a módszert, a szolgáltatás egy példánya esetén elhelyezett és készen állnak a tooexecute. Az állapotmentes szolgáltatások, amely egyszerűen jelenti, hogy hello service-példány már meg van nyitva. A megszakítási jogkivonat van megadva toocoordinate, amikor a szolgáltatáspéldány toobe zárva. A Service Fabric a megnyitása/bezárása ciklus egy szolgáltatáspéldány fordulhatnak elő sokszor élettartama hello hello szolgáltatás egészére. Ez akkor fordulhat elő, beleértve a különböző okokból:

* hello rendszer áthelyezi a szolgáltatáspéldány terheléselosztás erőforrás.
* Hibák merülnek fel, a kódban.
* hello alkalmazás- vagy frissítése.
* hello mögöttes hardver során kimaradás lép fel.

A vezénylési hello rendszer tookeep kezeli a szolgáltatás magas rendelkezésre állású és megfelelően kiegyensúlyozott.

`RunAsync()`meg nem blokkolják szinkron módon történik. RunAsync megvalósítását kell vissza egy feladatot, vagy a bármely hosszan futó vagy blokkoló műveletek tooallow hello futásidejű toocontinue await. Megjegyzés: a hello `while(true)` hello előző példában a feladatot visszaadó hurok `await Task.Delay()` szolgál. Ha a terhelést a szinkron módon kell blokkolja, olyan időszakra ütemezze egy új feladatot `Task.Run()` a a `RunAsync` végrehajtására.

A számítási feladatok megszakítását egy együttműködési elérhető cancellation jogkivonat megadott hello által összehangolva. hello rendszer megvárja, hogy a feladat tooend (által sikeres befejezése, megszakítása vagy hiba) előtt átvitel során. Fontos toohonor hello cancellation token, Befejezés bármely működik, és zárja be `RunAsync()` mihamarabbi Ha hello rendszer törlését kéri.

Az állapotmentes szolgáltatások példában hello száma lokális változó tárolja. De mivel ez egy állapotmentes szolgáltatások, tárolt hello érték csak az aktuális életciklusát hello a szolgáltatáspéldány létezik-e. Hello szolgáltatás helyezi át, vagy újraindul, hello érték elvész.

## <a name="create-a-stateful-service"></a>Az állapotalapú szolgáltatás létrehozása
A Service Fabric egy olyan szolgáltatás, amely állapotalapú új típusú vezet be. Az állapotalapú szolgáltatás megbízhatóan hello szolgáltatást, közös elhelyezésű által használt hello kóddal belül is állapotban. Állapot köszönhetően magas rendelkezésre állású Service Fabric hello kell toopersist tooan külső állapottárolóhoz nélkül.

egy számláló értéke a rendelkezésre álló és állandó állapotmentes toohighly tooconvert, még akkor is hello szolgáltatás áthelyezi vagy újraindul, úgy kell állapotalapú szolgáltatásból.

Az azonos hello *HelloWorld* alkalmazás, adhat hozzá egy új szolgáltatás kattintson a jobb gombbal a hello szolgáltatások hivatkozások hello projektet, majd válassza a **Hozzáadás -> új Service Fabric-szolgáltatás**.

![A szolgáltatás tooyour Service Fabric-alkalmazás hozzáadása](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

Válassza ki **állapotalapú alkalmazások és szolgáltatások szolgáltatás** és adjon neki nevet *HelloWorldStateful*. Kattintson az **OK** gombra.

![Hello új projekt párbeszédpanel bezárásához toocreate egy új állapotalapú Service Fabric-szolgáltatás használata](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

Az alkalmazás most már rendelkeznie kell a két szolgáltatás: hello állapotmentes szolgáltatások *HelloWorldStateless* és hello állapotalapú szolgáltatási *HelloWorldStateful*.

Állapotalapú service rendelkezik hello ugyanazon belépési pontok állapotmentes szolgáltatásként. hello fő különbség hello rendelkezésre állását a *állapotszolgáltató* , amely megbízhatóan tárolja állapotát. A Service Fabric tartalmaz egy állapot szolgáltató általi megvalósítása nevű [megbízható gyűjtemények](service-fabric-reliable-services-reliable-collections.md), amely lehetővé teszi a replikált adatok struktúrákat hello megbízható állapotkezelője létrehozása. Egy állapot-nyilvántartó megbízható szolgáltatás alapértelmezés szerint az állapotszolgáltató használja.

Nyissa meg **HelloWorldStateful.cs** a *HelloWorldStateful*, amely tartalmazza a következő RunAsync metódusában hello:

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");

            ServiceEventSource.Current.ServiceMessage(this, "Current Counter Value: {0}",
                result.HasValue ? result.Value.ToString() : "Value does not exist.");

            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, hello transaction aborts, all changes are
            // discarded, and nothing is saved toohello secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a>RunAsync
`RunAsync()`az állapot nélküli és állapotalapú alkalmazások és szolgáltatások hasonlóan működnek. Azonban az állapotalapú service-ben hello platform hajt végre további feladata az Ön nevében végrehajtása előtt `RunAsync()`. Ez a munkahelyi tartalmazhatnak, győződjön meg arról, hogy hello megbízható állapotkezelője és megbízható gyűjtemények készen toouse.

### <a name="reliable-collections-and-hello-reliable-state-manager"></a>Megbízható gyűjtemények és hello megbízható állapotkezelő
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) a szótár megvalósítása használható tooreliably állapot tárolása hello szolgáltatásban. A Service Fabric és megbízható gyűjtemények tárolható adatok közvetlenül a szolgáltatás egy külső állandó tároló hello szükségessége nélkül. Megbízható gyűjtemények hogy az adatok magas rendelkezésre állású. A Service Fabric ezt a feladatot el létrehozásával és kezelésével több *replikák* meg a szolgáltatás. Egy kötelező hello összetett szolgáltatásokkal, ezeket a replikák és a Állapotváltások kivonatolja API-t is tartalmazza.

Megbízható gyűjtemények .NET bármilyen, beleértve a felhasználói típusok, a figyelmeztetések néhány tudja tárolni:

* A Service Fabric lehetővé teszi a állapot magas rendelkezésre állású által *replikálása* csomópontokat, és megbízható gyűjtemények állapot tárolásának az adatlemezt toolocal minden replikán. Ez azt jelenti, hogy minden, a megbízható gyűjtemények tárolt kell *szerializálható*. Alapértelmezés szerint megbízható gyűjtemények használata [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) a szerializálási, így esetén fontos, hogy van-e a típusok toomake [hello adategyezmény-szerializáló által támogatott](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) hello alapértelmezett használatakor a szerializáló.
* Objektumok lesznek replikálva a magas rendelkezésre állású, ha megbízható gyűjteményre tranzakciók véglegesítése után. A megbízható gyűjtemények tárolt objektumok kell tartani a szolgáltatás a helyi memóriához. Ez azt jelenti, hogy rendelkezik-e a helyi hivatkozási toohello objektumot.
  
   Fontos, hogy Ön nem mutálódni azokat az objektumokat helyi példányát egy tranzakcióban hello megbízható gyűjtemény a frissítési művelet végrehajtása nélkül. Ennek az az oka objektumok toolocal példányait módosításokat a rendszer nem replikálja automatikusan. Kell újra beszúrása hello objektum vissza hello szótár vagy hello valamelyikével *frissítése* hello szótár metódusai.

hello megbízható állapotkezelője megbízható gyűjtemények az Ön kezeli. Egyszerűen megkérheti hello megbízható állapotkezelője megbízható gyűjtemény neve és bárhol, bármikor a szolgáltatásban. hello megbízható állapotkezelője biztosítja, hogy vissza a hivatkozás. Nem ajánlott menteni hivatkozások tooreliable gyűjtemény példányok osztály tagváltozók vagy tulajdonságok. Különleges figyelmet kell fordítani a beállított hello hivatkozás tooensure tooan példány mindig hello szolgáltatás életciklusának. hello megbízható állapotkezelője kezeli a megfelelőek Önnek, és ismétlési látogatások van optimalizálva.

### <a name="transactional-and-asynchronous-operations"></a>Tranzakciós és aszinkron műveletek
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

Megbízható a gyűjteményeknél hello számos ugyanazokat a műveleteket, amelyek a `System.Collections.Generic` és `System.Collections.Concurrent` megfelelők esetében, kivéve a LINQ. A megbízható gyűjtemények műveletek aszinkron jellegűek. Ennek az az oka az írási műveletek megbízható gyűjteményekkel i/o-műveletek tooreplicate végrehajtani, és adatok toodisk megmaradnak.

Megbízható gyűjtemény műveletek *tranzakciós*, így biztosítható állapot konzisztens több megbízható gyűjtemények és műveletek. Például akkor lehet, hogy egy megbízható várólista munkaelem created, végrehajtania egy műveletet hozzá és mentése hello eredmény megbízható szótárat, egy tranzakción belül. Ez a rendszer egy atomi művelet, és Ez garantálja, hogy sikeres lesz a teljes műveletet hello vagy hello teljes művelet állítja vissza. Ha a hiba akkor fordul elő, akkor created hello elem után, de hello eredmény mentése előtt, hello teljes tranzakció vissza lesz állítva, és hello marad, hello feldolgozás várakozási sorban.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
A rendszer most visszaadja a toohello *HelloWorld* alkalmazás. Most már létrehozhatja és a szolgáltatások telepítéséhez. Amikor lenyomja az **F5**, az alkalmazás a beépített és telepített tooyour helyi fürt lesz.

Hello szolgáltatási futtató start, követően létrehozott hello esemény Windows (nyomkövetés) események megtekintheti egy **a diagnosztikai** ablak. Ne feledje, hogy hello események megjelenítésének hello állapotmentes szolgáltatások és az állapotalapú szolgáltatás hello hello alkalmazásban. Akár szüneteltetheti is hello adatfolyam hello kattintva **szüneteltetése** gombra. Egy üzenet hello részleteinek üzenet kibontásával majd ellenőrizheti.

> [!NOTE]
> Hello alkalmazás futtatása előtt győződjön meg arról, hogy rendelkezik-e a helyi fejlesztési fürtök fut-e. Tekintse meg a hello [– első lépések útmutató](service-fabric-get-started.md) a helyi környezet beállításával kapcsolatos információk.
> 
> 

![A Visual Studio diagnosztikai eseményeinek megtekintése](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a>Következő lépések
[A Visual Studio a Service Fabric-alkalmazás hibakeresése](service-fabric-debugging-your-application.md)

[Első lépések: az OWIN futtató önálló Service Fabric Web API szolgáltatások](service-fabric-reliable-services-communication-webapi.md)

[További információ a megbízható gyűjtemények](service-fabric-reliable-services-reliable-collections.md)

[Alkalmazás üzembe helyezése](service-fabric-deploy-remove-applications.md)

[Az alkalmazásfrissítés](service-fabric-application-upgrade.md)

[Fejlesztői útmutató a Reliable Services](https://msdn.microsoft.com/library/azure/dn706529.aspx)

