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
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="41d8a-103">Ismerkedés a Reliable Services használatával</span><span class="sxs-lookup"><span data-stu-id="41d8a-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="41d8a-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="41d8a-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="41d8a-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="41d8a-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
> 
> 

<span data-ttu-id="41d8a-106">Az Azure Service Fabric-alkalmazás egy vagy több olyan szolgáltatás, amely az Ön kódjának futtatásához tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41d8a-106">An Azure Service Fabric application contains one or more services that run your code.</span></span> <span data-ttu-id="41d8a-107">Ez az útmutató bemutatja, hogyan toocreate állapotmentes és állapotalapú mind a Service Fabric-alkalmazások [Reliable Services](service-fabric-reliable-services-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="41d8a-107">This guide shows you how toocreate both stateless and stateful Service Fabric applications with [Reliable Services](service-fabric-reliable-services-introduction.md).</span></span>  <span data-ttu-id="41d8a-108">A Microsoft Virtual Academy videó azt is bemutatja, hogyan toocreate állapotmentes működnie:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span><span class="sxs-lookup"><span data-stu-id="41d8a-108">This Microsoft Virtual Academy video also shows you how toocreate a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a><span data-ttu-id="41d8a-109">Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="41d8a-109">Basic concepts</span></span>
<span data-ttu-id="41d8a-110">csak akkor Reliable Services használatába tooget toounderstand néhány főbb fogalmait és kifejezéseit van szükség:</span><span class="sxs-lookup"><span data-stu-id="41d8a-110">tooget started with Reliable Services, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="41d8a-111">**Szolgáltatás típusa**: Ez a szolgáltatás megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="41d8a-111">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="41d8a-112">Hello osztályt írt induló `StatelessService` és bármely más kód vagy függőségek használt, valamint nevét és verziószámát.</span><span class="sxs-lookup"><span data-stu-id="41d8a-112">It is defined by hello class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="41d8a-113">**Szolgáltatáspéldány nevű**: toorun a szolgáltatás létrehozása a szolgáltatás típusának nevesített példányai sokkal például létrehozhat egy osztálytípus objektum példánya.</span><span class="sxs-lookup"><span data-stu-id="41d8a-113">**Named service instance**: toorun your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="41d8a-114">A szolgáltatáspéldány hello képernyőn hello segítségével URI névvel rendelkezik "fabric: /" sémáját, például a "fabric: / MyApp/MyService".</span><span class="sxs-lookup"><span data-stu-id="41d8a-114">A service instance has a name in hello form of a URI using hello "fabric:/" scheme, such as "fabric:/MyApp/MyService".</span></span>
* <span data-ttu-id="41d8a-115">**Szolgáltatásgazda**: hello hoz létre egy gazdafolyamaton belül kell toorun szolgáltatáspéldány nevű.</span><span class="sxs-lookup"><span data-stu-id="41d8a-115">**Service host**: hello named service instances you create need toorun inside a host process.</span></span> <span data-ttu-id="41d8a-116">hello szolgáltatás állomás csak az egyik folyamat, ahol a szolgáltatás példányának futtatható.</span><span class="sxs-lookup"><span data-stu-id="41d8a-116">hello service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="41d8a-117">**Eszközregisztrációs szolgáltatás**: regisztráció egyesíti mindent.</span><span class="sxs-lookup"><span data-stu-id="41d8a-117">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="41d8a-118">hello szolgáltatástípus regisztrálva kell lennie a Service Fabric hello futásidejű szolgáltatás a gazdagép tooallow Service Fabric toocreate példányai azt toorun.</span><span class="sxs-lookup"><span data-stu-id="41d8a-118">hello service type must be registered with hello Service Fabric runtime in a service host tooallow Service Fabric toocreate instances of it toorun.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="41d8a-119">Állapot nélküli szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="41d8a-119">Create a stateless service</span></span>
<span data-ttu-id="41d8a-120">Egy állapotmentes szolgáltatások olyan szolgáltatás, amely jelenleg hello alapértelmezetté a felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="41d8a-120">A stateless service is a type of service that is currently hello norm in cloud applications.</span></span> <span data-ttu-id="41d8a-121">Ez az állapot nélküli tekintendő, mert hello szolgáltatást tartalmaz, amelyet a toobe megbízhatóan tárolja, vagy a magas rendelkezésre állású adatokat.</span><span class="sxs-lookup"><span data-stu-id="41d8a-121">It is considered stateless because hello service itself does not contain data that needs toobe stored reliably or made highly available.</span></span> <span data-ttu-id="41d8a-122">Ha az állapot nélküli szolgáltatás egy példánya leáll, az összes belső állapotában elvész.</span><span class="sxs-lookup"><span data-stu-id="41d8a-122">If an instance of a stateless service shuts down, all of its internal state is lost.</span></span> <span data-ttu-id="41d8a-123">Az ilyen típusú szolgáltatás állapotban kell lennie megőrzött tooan külső áruházban, például Azure táblák vagy az SQL-adatbázisban, amíg toobe végzett magas rendelkezésre állású és megbízható.</span><span class="sxs-lookup"><span data-stu-id="41d8a-123">In this type of service, state must be persisted tooan external store, such as Azure Tables or a SQL database, for it toobe made highly available and reliable.</span></span>

<span data-ttu-id="41d8a-124">Indítsa el a Visual Studio 2015-öt vagy a Visual Studio 2017 rendszergazdaként, és hozzon létre egy új Service Fabric-projektet nevű *HelloWorld*:</span><span class="sxs-lookup"><span data-stu-id="41d8a-124">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project named *HelloWorld*:</span></span>

![Hello új projekt párbeszédpanel bezárásához toocreate egy új Service Fabric-alkalmazás használata](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

<span data-ttu-id="41d8a-126">Majd létre szeretne hozni egy állapotmentes szolgáltatások nevű projekt *HelloWorldStateless*:</span><span class="sxs-lookup"><span data-stu-id="41d8a-126">Then create a stateless service project named *HelloWorldStateless*:</span></span>

![A hello második párbeszédpanelen állapotmentes szolgáltatások-projekt létrehozása](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

<span data-ttu-id="41d8a-128">A megoldás most két projektet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="41d8a-128">Your solution now contains two projects:</span></span>

* <span data-ttu-id="41d8a-129">*HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="41d8a-129">*HelloWorld*.</span></span> <span data-ttu-id="41d8a-130">Ez a hello *alkalmazás* projektet, amely tartalmazza a *szolgáltatások*.</span><span class="sxs-lookup"><span data-stu-id="41d8a-130">This is hello *application* project that contains your *services*.</span></span> <span data-ttu-id="41d8a-131">Is tartalmaz, amely leírja a az alkalmazás hello alkalmazás, valamint a PowerShell-parancsfájlok, amelyek segítenek toodeploy számos hello alkalmazásjegyzék.</span><span class="sxs-lookup"><span data-stu-id="41d8a-131">It also contains hello application manifest that describes hello application, as well as a number of PowerShell scripts that help you toodeploy your application.</span></span>
* <span data-ttu-id="41d8a-132">*HelloWorldStateless*.</span><span class="sxs-lookup"><span data-stu-id="41d8a-132">*HelloWorldStateless*.</span></span> <span data-ttu-id="41d8a-133">Ez a hello projektet.</span><span class="sxs-lookup"><span data-stu-id="41d8a-133">This is hello service project.</span></span> <span data-ttu-id="41d8a-134">Hello állapotmentes szolgáltatások megvalósítási tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="41d8a-134">It contains hello stateless service implementation.</span></span>

## <a name="implement-hello-service"></a><span data-ttu-id="41d8a-135">Hello szolgáltatás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="41d8a-135">Implement hello service</span></span>
<span data-ttu-id="41d8a-136">Nyissa meg hello **HelloWorldStateless.cs** fájl hello szolgáltatás projektben.</span><span class="sxs-lookup"><span data-stu-id="41d8a-136">Open hello **HelloWorldStateless.cs** file in hello service project.</span></span> <span data-ttu-id="41d8a-137">A Service Fabric szolgáltatás futtatható bármely üzleti logika.</span><span class="sxs-lookup"><span data-stu-id="41d8a-137">In Service Fabric, a service can run any business logic.</span></span> <span data-ttu-id="41d8a-138">hello szolgáltatás API két belépési pontok biztosít a kódot:</span><span class="sxs-lookup"><span data-stu-id="41d8a-138">hello service API provides two entry points for your code:</span></span>

* <span data-ttu-id="41d8a-139">Egy nyílt belépési pont metódus hívása *RunAsync*, ahol el lehet kezdeni a munkaterheléseket, ideértve a hosszan futó számítási feladatok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="41d8a-139">An open-ended entry point method, called *RunAsync*, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* <span data-ttu-id="41d8a-140">Ahol a kommunikációs verem szerkesztőprogramban, például az ASP.NET Core csatlakoztatható kommunikációs belépési pont.</span><span class="sxs-lookup"><span data-stu-id="41d8a-140">A communication entry point where you can plug in your communication stack of choice, such as ASP.NET Core.</span></span> <span data-ttu-id="41d8a-141">Ez azért, ahol elindíthatja kéréseket fogad a felhasználók és más szolgáltatások védelmét.</span><span class="sxs-lookup"><span data-stu-id="41d8a-141">This is where you can start receiving requests from users and other services.</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

<span data-ttu-id="41d8a-142">Ebben az oktatóanyagban tárgyaljuk hello `RunAsync()` belépési pont metódusa.</span><span class="sxs-lookup"><span data-stu-id="41d8a-142">In this tutorial, we will focus on hello `RunAsync()` entry point method.</span></span> <span data-ttu-id="41d8a-143">Ez azért, ahol azonnal elindíthatja a kódja.</span><span class="sxs-lookup"><span data-stu-id="41d8a-143">This is where you can immediately start running your code.</span></span>
<span data-ttu-id="41d8a-144">hello projektsablon tartalmaz egy mintát végrehajtásának `RunAsync()` , amely növeli a működés közbeni száma.</span><span class="sxs-lookup"><span data-stu-id="41d8a-144">hello project template includes a sample implementation of `RunAsync()` that increments a rolling count.</span></span>

> [!NOTE]
> <span data-ttu-id="41d8a-145">Hogyan toowork a kommunikációs verem kapcsolatos részletekért lásd: [Service Fabric Web API szolgáltatások OWIN önálló üzemeltetéséhez.](service-fabric-reliable-services-communication-webapi.md)</span><span class="sxs-lookup"><span data-stu-id="41d8a-145">For details about how toowork with a communication stack, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md)</span></span>
> 
> 

### <a name="runasync"></a><span data-ttu-id="41d8a-146">RunAsync</span><span class="sxs-lookup"><span data-stu-id="41d8a-146">RunAsync</span></span>
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

<span data-ttu-id="41d8a-147">hello platform meghívja ezt a módszert, a szolgáltatás egy példánya esetén elhelyezett és készen állnak a tooexecute.</span><span class="sxs-lookup"><span data-stu-id="41d8a-147">hello platform calls this method when an instance of a service is placed and ready tooexecute.</span></span> <span data-ttu-id="41d8a-148">Az állapotmentes szolgáltatások, amely egyszerűen jelenti, hogy hello service-példány már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="41d8a-148">For a stateless service, that simply means when hello service instance is opened.</span></span> <span data-ttu-id="41d8a-149">A megszakítási jogkivonat van megadva toocoordinate, amikor a szolgáltatáspéldány toobe zárva.</span><span class="sxs-lookup"><span data-stu-id="41d8a-149">A cancellation token is provided toocoordinate when your service instance needs toobe closed.</span></span> <span data-ttu-id="41d8a-150">A Service Fabric a megnyitása/bezárása ciklus egy szolgáltatáspéldány fordulhatnak elő sokszor élettartama hello hello szolgáltatás egészére.</span><span class="sxs-lookup"><span data-stu-id="41d8a-150">In Service Fabric, this open/close cycle of a service instance can occur many times over hello lifetime of hello service as a whole.</span></span> <span data-ttu-id="41d8a-151">Ez akkor fordulhat elő, beleértve a különböző okokból:</span><span class="sxs-lookup"><span data-stu-id="41d8a-151">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="41d8a-152">hello rendszer áthelyezi a szolgáltatáspéldány terheléselosztás erőforrás.</span><span class="sxs-lookup"><span data-stu-id="41d8a-152">hello system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="41d8a-153">Hibák merülnek fel, a kódban.</span><span class="sxs-lookup"><span data-stu-id="41d8a-153">Faults occur in your code.</span></span>
* <span data-ttu-id="41d8a-154">hello alkalmazás- vagy frissítése.</span><span class="sxs-lookup"><span data-stu-id="41d8a-154">hello application or system is upgraded.</span></span>
* <span data-ttu-id="41d8a-155">hello mögöttes hardver során kimaradás lép fel.</span><span class="sxs-lookup"><span data-stu-id="41d8a-155">hello underlying hardware experiences an outage.</span></span>

<span data-ttu-id="41d8a-156">A vezénylési hello rendszer tookeep kezeli a szolgáltatás magas rendelkezésre állású és megfelelően kiegyensúlyozott.</span><span class="sxs-lookup"><span data-stu-id="41d8a-156">This orchestration is managed by hello system tookeep your service highly available and properly balanced.</span></span>

<span data-ttu-id="41d8a-157">`RunAsync()`meg nem blokkolják szinkron módon történik.</span><span class="sxs-lookup"><span data-stu-id="41d8a-157">`RunAsync()` should not block synchronously.</span></span> <span data-ttu-id="41d8a-158">RunAsync megvalósítását kell vissza egy feladatot, vagy a bármely hosszan futó vagy blokkoló műveletek tooallow hello futásidejű toocontinue await.</span><span class="sxs-lookup"><span data-stu-id="41d8a-158">Your implementation of RunAsync should return a Task or await on any long-running or blocking operations tooallow hello runtime toocontinue.</span></span> <span data-ttu-id="41d8a-159">Megjegyzés: a hello `while(true)` hello előző példában a feladatot visszaadó hurok `await Task.Delay()` szolgál.</span><span class="sxs-lookup"><span data-stu-id="41d8a-159">Note in hello `while(true)` loop in hello previous example, a Task-returning `await Task.Delay()` is used.</span></span> <span data-ttu-id="41d8a-160">Ha a terhelést a szinkron módon kell blokkolja, olyan időszakra ütemezze egy új feladatot `Task.Run()` a a `RunAsync` végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="41d8a-160">If your workload must block synchronously, you should schedule a new Task with `Task.Run()` in your `RunAsync` implementation.</span></span>

<span data-ttu-id="41d8a-161">A számítási feladatok megszakítását egy együttműködési elérhető cancellation jogkivonat megadott hello által összehangolva.</span><span class="sxs-lookup"><span data-stu-id="41d8a-161">Cancellation of your workload is a cooperative effort orchestrated by hello provided cancellation token.</span></span> <span data-ttu-id="41d8a-162">hello rendszer megvárja, hogy a feladat tooend (által sikeres befejezése, megszakítása vagy hiba) előtt átvitel során.</span><span class="sxs-lookup"><span data-stu-id="41d8a-162">hello system will wait for your task tooend (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="41d8a-163">Fontos toohonor hello cancellation token, Befejezés bármely működik, és zárja be `RunAsync()` mihamarabbi Ha hello rendszer törlését kéri.</span><span class="sxs-lookup"><span data-stu-id="41d8a-163">It is important toohonor hello cancellation token, finish any work, and exit `RunAsync()` as quickly as possible when hello system requests cancellation.</span></span>

<span data-ttu-id="41d8a-164">Az állapotmentes szolgáltatások példában hello száma lokális változó tárolja.</span><span class="sxs-lookup"><span data-stu-id="41d8a-164">In this stateless service example, hello count is stored in a local variable.</span></span> <span data-ttu-id="41d8a-165">De mivel ez egy állapotmentes szolgáltatások, tárolt hello érték csak az aktuális életciklusát hello a szolgáltatáspéldány létezik-e.</span><span class="sxs-lookup"><span data-stu-id="41d8a-165">But because this is a stateless service, hello value that's stored exists only for hello current lifecycle of its service instance.</span></span> <span data-ttu-id="41d8a-166">Hello szolgáltatás helyezi át, vagy újraindul, hello érték elvész.</span><span class="sxs-lookup"><span data-stu-id="41d8a-166">When hello service moves or restarts, hello value is lost.</span></span>

## <a name="create-a-stateful-service"></a><span data-ttu-id="41d8a-167">Az állapotalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="41d8a-167">Create a stateful service</span></span>
<span data-ttu-id="41d8a-168">A Service Fabric egy olyan szolgáltatás, amely állapotalapú új típusú vezet be.</span><span class="sxs-lookup"><span data-stu-id="41d8a-168">Service Fabric introduces a new kind of service that is stateful.</span></span> <span data-ttu-id="41d8a-169">Az állapotalapú szolgáltatás megbízhatóan hello szolgáltatást, közös elhelyezésű által használt hello kóddal belül is állapotban.</span><span class="sxs-lookup"><span data-stu-id="41d8a-169">A stateful service can maintain state reliably within hello service itself, co-located with hello code that's using it.</span></span> <span data-ttu-id="41d8a-170">Állapot köszönhetően magas rendelkezésre állású Service Fabric hello kell toopersist tooan külső állapottárolóhoz nélkül.</span><span class="sxs-lookup"><span data-stu-id="41d8a-170">State is made highly available by Service Fabric without hello need toopersist state tooan external store.</span></span>

<span data-ttu-id="41d8a-171">egy számláló értéke a rendelkezésre álló és állandó állapotmentes toohighly tooconvert, még akkor is hello szolgáltatás áthelyezi vagy újraindul, úgy kell állapotalapú szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="41d8a-171">tooconvert a counter value from stateless toohighly available and persistent, even when hello service moves or restarts, you need a stateful service.</span></span>

<span data-ttu-id="41d8a-172">Az azonos hello *HelloWorld* alkalmazás, adhat hozzá egy új szolgáltatás kattintson a jobb gombbal a hello szolgáltatások hivatkozások hello projektet, majd válassza a **Hozzáadás -> új Service Fabric-szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="41d8a-172">In hello same *HelloWorld* application, you can add a new service by right-clicking on hello Services references in hello application project and selecting **Add ->  New Service Fabric Service**.</span></span>

![A szolgáltatás tooyour Service Fabric-alkalmazás hozzáadása](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

<span data-ttu-id="41d8a-174">Válassza ki **állapotalapú alkalmazások és szolgáltatások szolgáltatás** és adjon neki nevet *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="41d8a-174">Select **Stateful Service** and name it *HelloWorldStateful*.</span></span> <span data-ttu-id="41d8a-175">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="41d8a-175">Click **OK**.</span></span>

![Hello új projekt párbeszédpanel bezárásához toocreate egy új állapotalapú Service Fabric-szolgáltatás használata](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

<span data-ttu-id="41d8a-177">Az alkalmazás most már rendelkeznie kell a két szolgáltatás: hello állapotmentes szolgáltatások *HelloWorldStateless* és hello állapotalapú szolgáltatási *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="41d8a-177">Your application should now have two services: hello stateless service *HelloWorldStateless* and hello stateful service *HelloWorldStateful*.</span></span>

<span data-ttu-id="41d8a-178">Állapotalapú service rendelkezik hello ugyanazon belépési pontok állapotmentes szolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="41d8a-178">A stateful service has hello same entry points as a stateless service.</span></span> <span data-ttu-id="41d8a-179">hello fő különbség hello rendelkezésre állását a *állapotszolgáltató* , amely megbízhatóan tárolja állapotát.</span><span class="sxs-lookup"><span data-stu-id="41d8a-179">hello main difference is hello availability of a *state provider* that can store state reliably.</span></span> <span data-ttu-id="41d8a-180">A Service Fabric tartalmaz egy állapot szolgáltató általi megvalósítása nevű [megbízható gyűjtemények](service-fabric-reliable-services-reliable-collections.md), amely lehetővé teszi a replikált adatok struktúrákat hello megbízható állapotkezelője létrehozása.</span><span class="sxs-lookup"><span data-stu-id="41d8a-180">Service Fabric comes with a state provider implementation called [Reliable Collections](service-fabric-reliable-services-reliable-collections.md), which lets you create replicated data structures through hello Reliable State Manager.</span></span> <span data-ttu-id="41d8a-181">Egy állapot-nyilvántartó megbízható szolgáltatás alapértelmezés szerint az állapotszolgáltató használja.</span><span class="sxs-lookup"><span data-stu-id="41d8a-181">A stateful Reliable Service uses this state provider by default.</span></span>

<span data-ttu-id="41d8a-182">Nyissa meg **HelloWorldStateful.cs** a *HelloWorldStateful*, amely tartalmazza a következő RunAsync metódusában hello:</span><span class="sxs-lookup"><span data-stu-id="41d8a-182">Open **HelloWorldStateful.cs** in *HelloWorldStateful*, which contains hello following RunAsync method:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="41d8a-183">RunAsync</span><span class="sxs-lookup"><span data-stu-id="41d8a-183">RunAsync</span></span>
<span data-ttu-id="41d8a-184">`RunAsync()`az állapot nélküli és állapotalapú alkalmazások és szolgáltatások hasonlóan működnek.</span><span class="sxs-lookup"><span data-stu-id="41d8a-184">`RunAsync()` operates similarly in stateful and stateless services.</span></span> <span data-ttu-id="41d8a-185">Azonban az állapotalapú service-ben hello platform hajt végre további feladata az Ön nevében végrehajtása előtt `RunAsync()`.</span><span class="sxs-lookup"><span data-stu-id="41d8a-185">However, in a stateful service, hello platform performs additional work on your behalf before it executes `RunAsync()`.</span></span> <span data-ttu-id="41d8a-186">Ez a munkahelyi tartalmazhatnak, győződjön meg arról, hogy hello megbízható állapotkezelője és megbízható gyűjtemények készen toouse.</span><span class="sxs-lookup"><span data-stu-id="41d8a-186">This work can include ensuring that hello Reliable State Manager and Reliable Collections are ready toouse.</span></span>

### <a name="reliable-collections-and-hello-reliable-state-manager"></a><span data-ttu-id="41d8a-187">Megbízható gyűjtemények és hello megbízható állapotkezelő</span><span class="sxs-lookup"><span data-stu-id="41d8a-187">Reliable Collections and hello Reliable State Manager</span></span>
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

<span data-ttu-id="41d8a-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) a szótár megvalósítása használható tooreliably állapot tárolása hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="41d8a-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) is a dictionary implementation that you can use tooreliably store state in hello service.</span></span> <span data-ttu-id="41d8a-189">A Service Fabric és megbízható gyűjtemények tárolható adatok közvetlenül a szolgáltatás egy külső állandó tároló hello szükségessége nélkül.</span><span class="sxs-lookup"><span data-stu-id="41d8a-189">With Service Fabric and Reliable Collections, you can store data directly in your service without hello need for an external persistent store.</span></span> <span data-ttu-id="41d8a-190">Megbízható gyűjtemények hogy az adatok magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="41d8a-190">Reliable Collections make your data highly available.</span></span> <span data-ttu-id="41d8a-191">A Service Fabric ezt a feladatot el létrehozásával és kezelésével több *replikák* meg a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="41d8a-191">Service Fabric accomplishes this by creating and managing multiple *replicas* of your service for you.</span></span> <span data-ttu-id="41d8a-192">Egy kötelező hello összetett szolgáltatásokkal, ezeket a replikák és a Állapotváltások kivonatolja API-t is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41d8a-192">It also provides an API that abstracts away hello complexities of managing those replicas and their state transitions.</span></span>

<span data-ttu-id="41d8a-193">Megbízható gyűjtemények .NET bármilyen, beleértve a felhasználói típusok, a figyelmeztetések néhány tudja tárolni:</span><span class="sxs-lookup"><span data-stu-id="41d8a-193">Reliable Collections can store any .NET type, including your custom types, with a couple of caveats:</span></span>

* <span data-ttu-id="41d8a-194">A Service Fabric lehetővé teszi a állapot magas rendelkezésre állású által *replikálása* csomópontokat, és megbízható gyűjtemények állapot tárolásának az adatlemezt toolocal minden replikán.</span><span class="sxs-lookup"><span data-stu-id="41d8a-194">Service Fabric makes your state highly available by *replicating* state across nodes, and Reliable Collections store your data toolocal disk on each replica.</span></span> <span data-ttu-id="41d8a-195">Ez azt jelenti, hogy minden, a megbízható gyűjtemények tárolt kell *szerializálható*.</span><span class="sxs-lookup"><span data-stu-id="41d8a-195">This means that everything that is stored in Reliable Collections must be *serializable*.</span></span> <span data-ttu-id="41d8a-196">Alapértelmezés szerint megbízható gyűjtemények használata [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) a szerializálási, így esetén fontos, hogy van-e a típusok toomake [hello adategyezmény-szerializáló által támogatott](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) hello alapértelmezett használatakor a szerializáló.</span><span class="sxs-lookup"><span data-stu-id="41d8a-196">By default, Reliable Collections use [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) for serialization, so it's important toomake sure that your types are [supported by hello Data Contract Serializer](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) when you use hello default serializer.</span></span>
* <span data-ttu-id="41d8a-197">Objektumok lesznek replikálva a magas rendelkezésre állású, ha megbízható gyűjteményre tranzakciók véglegesítése után.</span><span class="sxs-lookup"><span data-stu-id="41d8a-197">Objects are replicated for high availability when you commit transactions on Reliable Collections.</span></span> <span data-ttu-id="41d8a-198">A megbízható gyűjtemények tárolt objektumok kell tartani a szolgáltatás a helyi memóriához.</span><span class="sxs-lookup"><span data-stu-id="41d8a-198">Objects stored in Reliable Collections are kept in local memory in your service.</span></span> <span data-ttu-id="41d8a-199">Ez azt jelenti, hogy rendelkezik-e a helyi hivatkozási toohello objektumot.</span><span class="sxs-lookup"><span data-stu-id="41d8a-199">This means that you have a local reference toohello object.</span></span>
  
   <span data-ttu-id="41d8a-200">Fontos, hogy Ön nem mutálódni azokat az objektumokat helyi példányát egy tranzakcióban hello megbízható gyűjtemény a frissítési művelet végrehajtása nélkül.</span><span class="sxs-lookup"><span data-stu-id="41d8a-200">It is important that you do not mutate local instances of those objects without performing an update operation on hello reliable collection in a transaction.</span></span> <span data-ttu-id="41d8a-201">Ennek az az oka objektumok toolocal példányait módosításokat a rendszer nem replikálja automatikusan.</span><span class="sxs-lookup"><span data-stu-id="41d8a-201">This is because changes toolocal instances of objects will not be replicated automatically.</span></span> <span data-ttu-id="41d8a-202">Kell újra beszúrása hello objektum vissza hello szótár vagy hello valamelyikével *frissítése* hello szótár metódusai.</span><span class="sxs-lookup"><span data-stu-id="41d8a-202">You must re-insert hello object back into hello dictionary or use one of hello *update* methods on hello dictionary.</span></span>

<span data-ttu-id="41d8a-203">hello megbízható állapotkezelője megbízható gyűjtemények az Ön kezeli.</span><span class="sxs-lookup"><span data-stu-id="41d8a-203">hello Reliable State Manager manages Reliable Collections for you.</span></span> <span data-ttu-id="41d8a-204">Egyszerűen megkérheti hello megbízható állapotkezelője megbízható gyűjtemény neve és bárhol, bármikor a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="41d8a-204">You can simply ask hello Reliable State Manager for a reliable collection by name at any time and at any place in your service.</span></span> <span data-ttu-id="41d8a-205">hello megbízható állapotkezelője biztosítja, hogy vissza a hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="41d8a-205">hello Reliable State Manager ensures that you get a reference back.</span></span> <span data-ttu-id="41d8a-206">Nem ajánlott menteni hivatkozások tooreliable gyűjtemény példányok osztály tagváltozók vagy tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="41d8a-206">We don't recommended that you save references tooreliable collection instances in class member variables or properties.</span></span> <span data-ttu-id="41d8a-207">Különleges figyelmet kell fordítani a beállított hello hivatkozás tooensure tooan példány mindig hello szolgáltatás életciklusának.</span><span class="sxs-lookup"><span data-stu-id="41d8a-207">Special care must be taken tooensure that hello reference is set tooan instance at all times in hello service lifecycle.</span></span> <span data-ttu-id="41d8a-208">hello megbízható állapotkezelője kezeli a megfelelőek Önnek, és ismétlési látogatások van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="41d8a-208">hello Reliable State Manager handles this work for you, and it's optimized for repeat visits.</span></span>

### <a name="transactional-and-asynchronous-operations"></a><span data-ttu-id="41d8a-209">Tranzakciós és aszinkron műveletek</span><span class="sxs-lookup"><span data-stu-id="41d8a-209">Transactional and asynchronous operations</span></span>
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

<span data-ttu-id="41d8a-210">Megbízható a gyűjteményeknél hello számos ugyanazokat a műveleteket, amelyek a `System.Collections.Generic` és `System.Collections.Concurrent` megfelelők esetében, kivéve a LINQ.</span><span class="sxs-lookup"><span data-stu-id="41d8a-210">Reliable Collections have many of hello same operations that their `System.Collections.Generic` and `System.Collections.Concurrent` counterparts do, except LINQ.</span></span> <span data-ttu-id="41d8a-211">A megbízható gyűjtemények műveletek aszinkron jellegűek.</span><span class="sxs-lookup"><span data-stu-id="41d8a-211">Operations on Reliable Collections are asynchronous.</span></span> <span data-ttu-id="41d8a-212">Ennek az az oka az írási műveletek megbízható gyűjteményekkel i/o-műveletek tooreplicate végrehajtani, és adatok toodisk megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="41d8a-212">This is because write operations with Reliable Collections perform I/O operations tooreplicate and persist data toodisk.</span></span>

<span data-ttu-id="41d8a-213">Megbízható gyűjtemény műveletek *tranzakciós*, így biztosítható állapot konzisztens több megbízható gyűjtemények és műveletek.</span><span class="sxs-lookup"><span data-stu-id="41d8a-213">Reliable Collection operations are *transactional*, so that you can keep state consistent across multiple Reliable Collections and operations.</span></span> <span data-ttu-id="41d8a-214">Például akkor lehet, hogy egy megbízható várólista munkaelem created, végrehajtania egy műveletet hozzá és mentése hello eredmény megbízható szótárat, egy tranzakción belül.</span><span class="sxs-lookup"><span data-stu-id="41d8a-214">For example, you may dequeue a work item from a Reliable Queue, perform an operation on it, and save hello result in a Reliable Dictionary, all within a single transaction.</span></span> <span data-ttu-id="41d8a-215">Ez a rendszer egy atomi művelet, és Ez garantálja, hogy sikeres lesz a teljes műveletet hello vagy hello teljes művelet állítja vissza.</span><span class="sxs-lookup"><span data-stu-id="41d8a-215">This is treated as an atomic operation, and it guarantees that either hello entire operation will succeed or hello entire operation will roll back.</span></span> <span data-ttu-id="41d8a-216">Ha a hiba akkor fordul elő, akkor created hello elem után, de hello eredmény mentése előtt, hello teljes tranzakció vissza lesz állítva, és hello marad, hello feldolgozás várakozási sorban.</span><span class="sxs-lookup"><span data-stu-id="41d8a-216">If an error occurs after you dequeue hello item but before you save hello result, hello entire transaction is rolled back and hello item remains in hello queue for processing.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="41d8a-217">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="41d8a-217">Run hello application</span></span>
<span data-ttu-id="41d8a-218">A rendszer most visszaadja a toohello *HelloWorld* alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="41d8a-218">We now return toohello *HelloWorld* application.</span></span> <span data-ttu-id="41d8a-219">Most már létrehozhatja és a szolgáltatások telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="41d8a-219">You can now build and deploy your services.</span></span> <span data-ttu-id="41d8a-220">Amikor lenyomja az **F5**, az alkalmazás a beépített és telepített tooyour helyi fürt lesz.</span><span class="sxs-lookup"><span data-stu-id="41d8a-220">When you press **F5**, your application will be built and deployed tooyour local cluster.</span></span>

<span data-ttu-id="41d8a-221">Hello szolgáltatási futtató start, követően létrehozott hello esemény Windows (nyomkövetés) események megtekintheti egy **a diagnosztikai** ablak.</span><span class="sxs-lookup"><span data-stu-id="41d8a-221">After hello services start running, you can view hello generated Event Tracing for Windows (ETW) events in a **Diagnostic Events** window.</span></span> <span data-ttu-id="41d8a-222">Ne feledje, hogy hello események megjelenítésének hello állapotmentes szolgáltatások és az állapotalapú szolgáltatás hello hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="41d8a-222">Note that hello events displayed are from both hello stateless service and hello stateful service in hello application.</span></span> <span data-ttu-id="41d8a-223">Akár szüneteltetheti is hello adatfolyam hello kattintva **szüneteltetése** gombra.</span><span class="sxs-lookup"><span data-stu-id="41d8a-223">You can pause hello stream by clicking hello **Pause** button.</span></span> <span data-ttu-id="41d8a-224">Egy üzenet hello részleteinek üzenet kibontásával majd ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="41d8a-224">You can then examine hello details of a message by expanding that message.</span></span>

> [!NOTE]
> <span data-ttu-id="41d8a-225">Hello alkalmazás futtatása előtt győződjön meg arról, hogy rendelkezik-e a helyi fejlesztési fürtök fut-e.</span><span class="sxs-lookup"><span data-stu-id="41d8a-225">Before you run hello application, make sure that you have a local development cluster running.</span></span> <span data-ttu-id="41d8a-226">Tekintse meg a hello [– első lépések útmutató](service-fabric-get-started.md) a helyi környezet beállításával kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="41d8a-226">Check out hello [getting started guide](service-fabric-get-started.md) for information on setting up your local environment.</span></span>
> 
> 

![A Visual Studio diagnosztikai eseményeinek megtekintése](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a><span data-ttu-id="41d8a-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41d8a-228">Next steps</span></span>
[<span data-ttu-id="41d8a-229">A Visual Studio a Service Fabric-alkalmazás hibakeresése</span><span class="sxs-lookup"><span data-stu-id="41d8a-229">Debug your Service Fabric application in Visual Studio</span></span>](service-fabric-debugging-your-application.md)

[<span data-ttu-id="41d8a-230">Első lépések: az OWIN futtató önálló Service Fabric Web API szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="41d8a-230">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>](service-fabric-reliable-services-communication-webapi.md)

[<span data-ttu-id="41d8a-231">További információ a megbízható gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="41d8a-231">Learn more about Reliable Collections</span></span>](service-fabric-reliable-services-reliable-collections.md)

[<span data-ttu-id="41d8a-232">Alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="41d8a-232">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[<span data-ttu-id="41d8a-233">Az alkalmazásfrissítés</span><span class="sxs-lookup"><span data-stu-id="41d8a-233">Application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="41d8a-234">Fejlesztői útmutató a Reliable Services</span><span class="sxs-lookup"><span data-stu-id="41d8a-234">Developer reference for Reliable Services</span></span>](https://msdn.microsoft.com/library/azure/dn706529.aspx)

