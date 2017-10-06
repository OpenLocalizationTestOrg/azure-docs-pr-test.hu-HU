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
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a><span data-ttu-id="482fd-103">A szolgáltatás előtér-webkiszolgáló az ASP.NET Core segítségével alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="482fd-103">Build a web service front end for your application using ASP.NET Core</span></span>
<span data-ttu-id="482fd-104">Alapértelmezés szerint az Azure Service Fabric szolgáltatások nem biztosítanak a nyilvános csatoló toohello webes.</span><span class="sxs-lookup"><span data-stu-id="482fd-104">By default, Azure Service Fabric services do not provide a public interface toohello web.</span></span> <span data-ttu-id="482fd-105">tooexpose az alkalmazás funkcióinak tooHTTP ügyfelek rendelkezik egy webes projekt tooact belépési pontként, és ott tooyour majd kommunikációt toocreate adott szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="482fd-105">tooexpose your application's functionality tooHTTP clients, you have toocreate a web project tooact as an entry point and then communicate from there tooyour individual services.</span></span>

<span data-ttu-id="482fd-106">Az oktatóanyag azt átvételéhez ahol azt korábban befejezte a hello [az első alkalmazás létrehozásának a Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) oktatóanyagot, és adja hozzá az ASP.NET Core webszolgáltatás elé hello állapot-nyilvántartó teljesítményszámláló szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="482fd-106">In this tutorial, we pick up where we left off in hello [Creating your first application in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) tutorial and add an ASP.NET Core web service in front of hello stateful counter service.</span></span> <span data-ttu-id="482fd-107">Ha még nem tette meg, lépjen vissza, és, hogy az oktatóanyag első lépéseit.</span><span class="sxs-lookup"><span data-stu-id="482fd-107">If you have not already done so, you should go back and step through that tutorial first.</span></span>

## <a name="set-up-your-environment-for-aspnet-core"></a><span data-ttu-id="482fd-108">Állítsa be a környezetet az ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="482fd-108">Set up your environment for ASP.NET Core</span></span>

<span data-ttu-id="482fd-109">A Visual Studio 2017 toofollow ebben az oktatóanyagban együtt van szüksége.</span><span class="sxs-lookup"><span data-stu-id="482fd-109">You need Visual Studio 2017 toofollow along with this tutorial.</span></span> <span data-ttu-id="482fd-110">Bármely edition fog tenni.</span><span class="sxs-lookup"><span data-stu-id="482fd-110">Any edition will do.</span></span> 

 - [<span data-ttu-id="482fd-111">Telepítse a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="482fd-111">Install Visual Studio 2017</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="482fd-112">az ASP.NET Core Service Fabric-alkalmazások toodevelop, rendelkeznie kell a következő telepített feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="482fd-112">toodevelop ASP.NET Core Service Fabric applications, you should have hello following workloads installed:</span></span>
 - <span data-ttu-id="482fd-113">**Azure fejlesztési** (alatt *webes & felhő*)</span><span class="sxs-lookup"><span data-stu-id="482fd-113">**Azure development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="482fd-114">**Az ASP.NET és a webes fejlesztési** (alatt *webes & felhő*)</span><span class="sxs-lookup"><span data-stu-id="482fd-114">**ASP.NET and web development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="482fd-115">**A .NET core platformfüggetlen fejlesztésekhez** (alatt *egyéb Toolsets*)</span><span class="sxs-lookup"><span data-stu-id="482fd-115">**.NET Core cross-platform development** (under *Other Toolsets*)</span></span>

>[!Note] 
><span data-ttu-id="482fd-116">hello .NET Core tools for Visual Studio 2015 már nem frissítés alatt álló, de a Visual Studio 2015 még használja, ha szüksége van-e toohave [.NET Core VS 2015 tooling eszköz Preview 2](https://www.microsoft.com/net/download/core) telepítve.</span><span class="sxs-lookup"><span data-stu-id="482fd-116">hello .NET Core tools for Visual Studio 2015 are no longer being updated, however if you are still using Visual Studio 2015, you need toohave [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) installed.</span></span>

## <a name="add-an-aspnet-core-service-tooyour-application"></a><span data-ttu-id="482fd-117">Az ASP.NET Core szolgáltatás tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="482fd-117">Add an ASP.NET Core service tooyour application</span></span>
<span data-ttu-id="482fd-118">Az ASP.NET Core egy egyszerűsített, platformfüggetlen webes fejlesztési keretrendszer, hogy toocreate modern webes felhasználói Felületét használja, és webes API-khoz.</span><span class="sxs-lookup"><span data-stu-id="482fd-118">ASP.NET Core is a lightweight, cross-platform web development framework that you can use toocreate modern web UI and web APIs.</span></span> 

<span data-ttu-id="482fd-119">ASP.NET webes API projektet tooour meglévő alkalmazás adjuk hozzá.</span><span class="sxs-lookup"><span data-stu-id="482fd-119">Let's add an ASP.NET Web API project tooour existing application.</span></span>

1. <span data-ttu-id="482fd-120">A Megoldáskezelőben kattintson a jobb gombbal **szolgáltatások** belül hello projektet, és válassza a **Hozzáadás > új Service Fabric-szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="482fd-120">In Solution Explorer, right-click **Services** within hello application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Új szolgáltatás tooan meglévő alkalmazás hozzáadása][vs-add-new-service]
2. <span data-ttu-id="482fd-122">A hello **szolgáltatás létrehozása** lapon, válassza ki **ASP.NET Core** és adjon neki egy nevet.</span><span class="sxs-lookup"><span data-stu-id="482fd-122">On hello **Create a Service** page, choose **ASP.NET Core** and give it a name.</span></span>
   
    ![ASP.NET webszolgáltatás kiválasztásával hello új szolgáltatás párbeszédpanelje][vs-new-service-dialog]

3. <span data-ttu-id="482fd-124">hello következő oldal biztosít az ASP.NET Core projektsablonjai.</span><span class="sxs-lookup"><span data-stu-id="482fd-124">hello next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="482fd-125">Vegye figyelembe, hogy ezek az vannak hello azonos lehetősége, hogy mutatunk be Ha kis mennyiségű kód tooregister hozott létre az ASP.NET Core projektek kívül a Service Fabric-alkalmazás hello szolgáltatást a Service Fabric-futtatókörnyezet hello.</span><span class="sxs-lookup"><span data-stu-id="482fd-125">Note that these are hello same choices that you would see if you created an ASP.NET Core project outside of a Service Fabric application, with a small amount of additional code tooregister hello service with hello Service Fabric runtime.</span></span> <span data-ttu-id="482fd-126">A jelen oktatóanyag esetében válassza ki a **Web API**.</span><span class="sxs-lookup"><span data-stu-id="482fd-126">For this tutorial, choose **Web API**.</span></span> <span data-ttu-id="482fd-127">Azonban alkalmazhat hello azonos fogalmak toobuilding teljes webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="482fd-127">However, you can apply hello same concepts toobuilding a full web application.</span></span>
   
    ![ASP.NET-projekt kiválasztása][vs-new-aspnet-project-dialog]
   
    <span data-ttu-id="482fd-129">A Web API-projekt létrehozása után az alkalmazás rendelkeznie kell két szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="482fd-129">Once your Web API project is created, you should have two services in your application.</span></span> <span data-ttu-id="482fd-130">Az alkalmazás továbbra toobuild, adhat hozzá további szolgáltatások hello pontosan azonos módon.</span><span class="sxs-lookup"><span data-stu-id="482fd-130">As you continue toobuild your application, you can add more services in exactly hello same way.</span></span> <span data-ttu-id="482fd-131">Minden egyes lehet függetlenül rendszerverzióval ellátott és frissített.</span><span class="sxs-lookup"><span data-stu-id="482fd-131">Each can be independently versioned and upgraded.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="482fd-132">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="482fd-132">Run hello application</span></span>
<span data-ttu-id="482fd-133">Mi azt régebben már kötöttek, most egyfajta tooget hello új alkalmazás és egy pillantást hello alapértelmezett viselkedését, hogy az ASP.NET Core Web API sablon hello biztosít hajtsa végre a megfelelő telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="482fd-133">tooget a sense of what we've done, let's deploy hello new application and take a look at hello default behavior that hello ASP.NET Core Web API template provides.</span></span>

1. <span data-ttu-id="482fd-134">Nyomja le az F5 billentyűt a Visual Studio toodebug hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="482fd-134">Press F5 in Visual Studio toodebug hello app.</span></span>
2. <span data-ttu-id="482fd-135">Központi telepítés befejeződése után a Visual Studio elindítja egy böngésző toohello gyökérmappájában hello ASP.NET Web API-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="482fd-135">When deployment is complete, Visual Studio launches a browser toohello root of hello ASP.NET Web API service.</span></span> <span data-ttu-id="482fd-136">hello ASP.NET Core webes API-sablon nem biztosítanak alapértelmezett viselkedése hello gyökér, így hello böngészőben 404 hibaüzenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="482fd-136">hello ASP.NET Core Web API template doesn't provide default behavior for hello root, so you should see a 404 error in hello browser.</span></span>
3. <span data-ttu-id="482fd-137">Adja hozzá `/api/values` hello böngészőben toohello helyét.</span><span class="sxs-lookup"><span data-stu-id="482fd-137">Add `/api/values` toohello location in hello browser.</span></span> <span data-ttu-id="482fd-138">Ez elindítja a hello `Get` hello ValuesController hello Web API sablonban metódust.</span><span class="sxs-lookup"><span data-stu-id="482fd-138">This invokes hello `Get` method on hello ValuesController in hello Web API template.</span></span> <span data-ttu-id="482fd-139">Hello alapértelmezett válasz hello sablon – két karakterláncokat tartalmazó JSON-tömb által biztosított adja vissza:</span><span class="sxs-lookup"><span data-stu-id="482fd-139">It returns hello default response that is provided by hello template--a JSON array that contains two strings:</span></span>
   
    ![Az ASP.NET Core Web API sablonból visszaadott alapértelmezett értékek][browser-aspnet-template-values]
   
    <span data-ttu-id="482fd-141">Hello oktatóanyag végére hello, ezen a lapon fog hello hello helyett az állapotalapú szolgáltatásból legutóbbi számlálóérték alapértelmezett karakterláncok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="482fd-141">By hello end of hello tutorial, this page will show hello most recent counter value from our stateful service instead of hello default strings.</span></span>

## <a name="connect-hello-services"></a><span data-ttu-id="482fd-142">Connect hello szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="482fd-142">Connect hello services</span></span>
<span data-ttu-id="482fd-143">A Service Fabric hogyan kommunikáljanak megbízható szolgáltatások teljes rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="482fd-143">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="482fd-144">Egyetlen alkalmazásban lehetséges, hogy szolgáltatásokat, amelyek a TCP, más szolgáltatásokkal, hogy elérhető-e egy HTTP REST API-n keresztül, és továbbra is más szolgáltatások, amelyek elérhetők a webes szoftvercsatornák keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="482fd-144">Within a single application, you might have services that are accessible via TCP, other services that are accessible via an HTTP REST API, and still other services that are accessible via web sockets.</span></span> <span data-ttu-id="482fd-145">Háttér hello lehetőségeit és hello mellékhatásokkal jár, lásd: [szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="482fd-145">For background on hello options available and hello tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span> <span data-ttu-id="482fd-146">Ebben az oktatóanyagban használjuk [Service Fabric szolgáltatás távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md), az hello SDK megadott.</span><span class="sxs-lookup"><span data-stu-id="482fd-146">In this tutorial, we use [Service Fabric Service Remoting](service-fabric-reliable-services-communication-remoting.md), provided in hello SDK.</span></span>

<span data-ttu-id="482fd-147">Hello szolgáltatás távoli eljáráshívási megközelítés (tapasztalatait távoli eljáráshívások vagy RPC-k) akkor határozza meg egy felület tooact hello nyilvános szerződés hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="482fd-147">In hello Service Remoting approach (modeled on remote procedure calls or RPCs), you define an interface tooact as hello public contract for hello service.</span></span> <span data-ttu-id="482fd-148">Ezt követően használja a adott felület toogenerate proxyosztály hello szolgáltatás való kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="482fd-148">Then, you use that interface toogenerate a proxy class for interacting with hello service.</span></span>

### <a name="create-hello-remoting-interface"></a><span data-ttu-id="482fd-149">Hello távelérési kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="482fd-149">Create hello remoting interface</span></span>
<span data-ttu-id="482fd-150">Először hozzon létre hello felület tooact hello szerződés hello állapotalapú szolgáltatási és egyéb szolgáltatások, a nagybetűk hello ASP.NET Core webes projekt között.</span><span class="sxs-lookup"><span data-stu-id="482fd-150">Let's start by creating hello interface tooact as hello contract between hello stateful service and other services, in this case hello ASP.NET Core web project.</span></span> <span data-ttu-id="482fd-151">Ez az interfész által az összes azt használó toomake RPC-hívások, így a saját Class Library projektben létrehozunk szolgáltatások kell osztani.</span><span class="sxs-lookup"><span data-stu-id="482fd-151">This interface must be shared by all services that use it toomake RPC calls, so we'll create it in its own Class Library project.</span></span>

1. <span data-ttu-id="482fd-152">A Megoldáskezelőben kattintson a jobb gombbal a megoldás, és válassza a **Hozzáadás** > **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="482fd-152">In Solution Explorer, right-click your solution and choose **Add** > **New Project**.</span></span>

2. <span data-ttu-id="482fd-153">Válassza ki a hello **Visual C#** bal oldali navigációs panelen, és jelölje ki hello hello bejegyzést **Class Library** sablont.</span><span class="sxs-lookup"><span data-stu-id="482fd-153">Choose hello **Visual C#** entry in hello left navigation pane and then select hello **Class Library** template.</span></span> <span data-ttu-id="482fd-154">Győződjön meg arról, hogy hello .NET-keretrendszer verziója túl van-e állítva**4.5.2-es**.</span><span class="sxs-lookup"><span data-stu-id="482fd-154">Ensure that hello .NET Framework version is set too**4.5.2**.</span></span>
   
    ![Az állapotalapú szolgáltatás felület projekt létrehozása][vs-add-class-library-project]

3. <span data-ttu-id="482fd-156">Telepítse a hello **Microsoft.ServiceFabric.Services.Remoting** NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="482fd-156">Install hello **Microsoft.ServiceFabric.Services.Remoting** NuGet package.</span></span> <span data-ttu-id="482fd-157">Keresse meg **Microsoft.ServiceFabric.Services.Remoting** a hello NuGet csomagot kezelő majd telepítse hello megoldás az összes olyan projekt, amelyek a távelérés szolgáltatás, beleértve:</span><span class="sxs-lookup"><span data-stu-id="482fd-157">Search for  **Microsoft.ServiceFabric.Services.Remoting** in hello NuGet package manager and install it for all projects in hello solution that use Service Remoting, including:</span></span>
   - <span data-ttu-id="482fd-158">hello szolgáltatásfelület tartalmazó hello Class Library projekt</span><span class="sxs-lookup"><span data-stu-id="482fd-158">hello Class Library project that contains hello service interface</span></span>
   - <span data-ttu-id="482fd-159">hello állapotalapú Service-projekt</span><span class="sxs-lookup"><span data-stu-id="482fd-159">hello Stateful Service project</span></span>
   - <span data-ttu-id="482fd-160">az ASP.NET Core webes projekt hello</span><span class="sxs-lookup"><span data-stu-id="482fd-160">hello ASP.NET Core web service project</span></span>
   
    ![Hello szolgáltatások a NuGet csomag hozzáadása][vs-services-nuget-package]

4. <span data-ttu-id="482fd-162">Hello osztály könyvtárban hozzon létre egy felület egy módszert, `GetCountAsync`, és hello felületének a kiterjesztése `Microsoft.ServiceFabric.Services.Remoting.IService`.</span><span class="sxs-lookup"><span data-stu-id="482fd-162">In hello class library, create an interface with a single method, `GetCountAsync`, and extend hello interface from `Microsoft.ServiceFabric.Services.Remoting.IService`.</span></span> <span data-ttu-id="482fd-163">hello távelérési kapcsolat kell származnia: az illesztőfelület tooindicate, hogy a rendszer a szolgáltatás távoli eljáráshívási felület.</span><span class="sxs-lookup"><span data-stu-id="482fd-163">hello remoting interface must derive from this interface tooindicate that it is a Service Remoting interface.</span></span>
   
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

### <a name="implement-hello-interface-in-your-stateful-service"></a><span data-ttu-id="482fd-164">Az állapot-nyilvántartó szolgáltatásban hello illesztőfelület megvalósítása</span><span class="sxs-lookup"><span data-stu-id="482fd-164">Implement hello interface in your stateful service</span></span>
<span data-ttu-id="482fd-165">Most, hogy definiáltuk hello felületet, tooimplement kell azt a hello állapotalapú szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="482fd-165">Now that we have defined hello interface, we need tooimplement it in hello stateful service.</span></span>

1. <span data-ttu-id="482fd-166">Az állapotalapú szolgáltatás hozzá egy hivatkozást toohello hordozhatóosztálytár-projektjének hello felület tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="482fd-166">In your stateful service, add a reference toohello class library project that contains hello interface.</span></span>
   
    ![Egy hivatkozási toohello hordozhatóosztálytár-projektjének hello állapotalapú szolgáltatás hozzáadása][vs-add-class-library-reference]
2. <span data-ttu-id="482fd-168">Keresse meg a hello osztályból származó `StatefulService`, például a `MyStatefulService`, és kiterjesztése tooimplement hello `ICounter` felületet.</span><span class="sxs-lookup"><span data-stu-id="482fd-168">Locate hello class that inherits from `StatefulService`, such as `MyStatefulService`, and extend it tooimplement hello `ICounter` interface.</span></span>
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. <span data-ttu-id="482fd-169">Most valósítja meg hello egyetlen hello definiált `ICounter` felületet, `GetCountAsync`.</span><span class="sxs-lookup"><span data-stu-id="482fd-169">Now implement hello single method that is defined in hello `ICounter` interface, `GetCountAsync`.</span></span>
   
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

### <a name="expose-hello-stateful-service-using-a-service-remoting-listener"></a><span data-ttu-id="482fd-170">Hello állapotalapú szolgáltatás használata a távoli eljáráshívási szolgáltatás figyelő indításához</span><span class="sxs-lookup"><span data-stu-id="482fd-170">Expose hello stateful service using a service remoting listener</span></span>
<span data-ttu-id="482fd-171">A hello `ICounter` megvalósított hello utolsó lépésként objektumfelület tooopen hello szolgáltatás távelérési kommunikációs csatorna.</span><span class="sxs-lookup"><span data-stu-id="482fd-171">With hello `ICounter` interface implemented, hello final step is tooopen hello Service Remoting communication channel.</span></span> <span data-ttu-id="482fd-172">Állapotalapú szolgáltatások esetén a Service Fabric nevű felülbírálható módszert kínál `CreateServiceReplicaListeners`.</span><span class="sxs-lookup"><span data-stu-id="482fd-172">For stateful services, Service Fabric provides an overridable method called `CreateServiceReplicaListeners`.</span></span> <span data-ttu-id="482fd-173">Ezzel a módszerrel megadhatja egy vagy több kommunikációs figyelőket, megjeleníteni kívánt tooenable a szolgáltatás kommunikációs hello típusa alapján.</span><span class="sxs-lookup"><span data-stu-id="482fd-173">With this method, you can specify one or more communication listeners, based on hello type of communication that you want tooenable for your service.</span></span>

> [!NOTE]
> <span data-ttu-id="482fd-174">hello egyenértékű módszer nyithatók meg a kommunikációs csatorna toostateless szolgáltatások neve `CreateServiceInstanceListeners`.</span><span class="sxs-lookup"><span data-stu-id="482fd-174">hello equivalent method for opening a communication channel toostateless services is called `CreateServiceInstanceListeners`.</span></span>

<span data-ttu-id="482fd-175">Ebben az esetben azt cseréli a meglévő hello `CreateServiceReplicaListeners` metódust, és adjon meg egy példánya `ServiceRemotingListener`, ami, amely létrehoz egy RPC-végpont metódus akkor hívható meg az ügyfelek a `ServiceProxy`.</span><span class="sxs-lookup"><span data-stu-id="482fd-175">In this case, we replace hello existing `CreateServiceReplicaListeners` method and provide an instance of `ServiceRemotingListener`, which creates an RPC endpoint that is callable from clients through `ServiceProxy`.</span></span>  

<span data-ttu-id="482fd-176">Hello `CreateServiceRemotingListener` hello bővítmény metódusa `IService` felület lehetővé teszi a tooeasily hozzon létre egy `ServiceRemotingListener` alapértelmezett beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="482fd-176">hello `CreateServiceRemotingListener` extension method on hello `IService` interface allows you tooeasily create a `ServiceRemotingListener` with all default settings.</span></span> <span data-ttu-id="482fd-177">toouse a kiterjesztésmetódus, ellenőrizze, hogy hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` importált névtér.</span><span class="sxs-lookup"><span data-stu-id="482fd-177">toouse this extension method, ensure you have hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` namespace imported.</span></span> 

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


### <a name="use-hello-serviceproxy-class-toointeract-with-hello-service"></a><span data-ttu-id="482fd-178">Hello ServiceProxy osztály toointeract hello szolgáltatás használatához</span><span class="sxs-lookup"><span data-stu-id="482fd-178">Use hello ServiceProxy class toointeract with hello service</span></span>
<span data-ttu-id="482fd-179">Az állapotalapú szolgáltatás már készen áll a tooreceive forgalom más szolgáltatások RPC-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="482fd-179">Our stateful service is now ready tooreceive traffic from other services over RPC.</span></span> <span data-ttu-id="482fd-180">Így továbbra is hozzáadásával hello kód toocommunicate vele az ASP.NET-webszolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="482fd-180">So all that remains is adding hello code toocommunicate with it from hello ASP.NET web service.</span></span>

1. <span data-ttu-id="482fd-181">Az ASP.NET projektben adja hozzá egy hivatkozást toohello osztálytár hello tartalmazó `ICounter` felületet.</span><span class="sxs-lookup"><span data-stu-id="482fd-181">In your ASP.NET project, add a reference toohello class library that contains hello `ICounter` interface.</span></span>

2. <span data-ttu-id="482fd-182">Korábban hozzáadott hello **Microsoft.ServiceFabric.Services.Remoting** NuGet csomag toohello ASP.NET-projekt.</span><span class="sxs-lookup"><span data-stu-id="482fd-182">Earlier you added hello **Microsoft.ServiceFabric.Services.Remoting** NuGet package toohello ASP.NET project.</span></span> <span data-ttu-id="482fd-183">Ez a csomag biztosít hello `ServiceProxy` osztály, amely toomake RPC meghívja a toohello állapot-nyilvántartó szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="482fd-183">This package provides hello `ServiceProxy` class which you use toomake RPC calls toohello stateful service.</span></span> <span data-ttu-id="482fd-184">Győződjön meg arról, a NuGet-csomagja telepítve van az ASP.NET Core webes projekt hello.</span><span class="sxs-lookup"><span data-stu-id="482fd-184">Ensure this NuGet package is installed in hello ASP.NET Core web service project.</span></span>

4. <span data-ttu-id="482fd-185">A hello **tartományvezérlők** mappa, nyissa meg hello `ValuesController` osztály.</span><span class="sxs-lookup"><span data-stu-id="482fd-185">In hello **Controllers** folder, open hello `ValuesController` class.</span></span> <span data-ttu-id="482fd-186">Vegye figyelembe, hogy hello `Get` metódus jelenleg csak egy "érték1" és "érték2" – amely megfelel a mi hello böngészőben korábban látott kódolt karakterlánc-tömbben ad vissza.</span><span class="sxs-lookup"><span data-stu-id="482fd-186">Note that hello `Get` method currently just returns a hard-coded string array of "value1" and "value2"--which matches what we saw earlier in hello browser.</span></span> <span data-ttu-id="482fd-187">Ez a megvalósítás cserélje le a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="482fd-187">Replace this implementation with hello following code:</span></span>
   
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
   
    <span data-ttu-id="482fd-188">hello első kódsort hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="482fd-188">hello first line of code is hello key one.</span></span> <span data-ttu-id="482fd-189">toocreate hello ICounter toohello állapot-nyilvántartó proxyszolgáltatás, kell tooprovide kétféle információt: hello szolgáltatás egy partíció azonosítója és hello nevét.</span><span class="sxs-lookup"><span data-stu-id="482fd-189">toocreate hello ICounter proxy toohello stateful service, you need tooprovide two pieces of information: a partition ID and hello name of hello service.</span></span>
   
    <span data-ttu-id="482fd-190">Particionálási tooscale állapotalapú szolgáltatások használatához összeállításának be különböző gyűjtők állapotukra egy definiálhat, például a felhasználói azonosító vagy irányítószámot kulcs alapján.</span><span class="sxs-lookup"><span data-stu-id="482fd-190">You can use partitioning tooscale stateful services by breaking up their state into different buckets, based on a key that you define, such as a customer ID or postal code.</span></span> <span data-ttu-id="482fd-191">Trivial alkalmazás, a hello állapotalapú szolgáltatásból csak rendelkezik egy partíciót, így hello kulcs nem számít.</span><span class="sxs-lookup"><span data-stu-id="482fd-191">In our trivial application, hello stateful service only has one partition, so hello key doesn't matter.</span></span> <span data-ttu-id="482fd-192">Bármely Ön által biztosított kulcs irányítja toohello egyazon partícióra kerüljenek.</span><span class="sxs-lookup"><span data-stu-id="482fd-192">Any key that you provide will lead toohello same partition.</span></span> <span data-ttu-id="482fd-193">toolearn szolgáltatások, particionálás bővebben lásd: [hogyan toopartition Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="482fd-193">toolearn more about partitioning services, see [How toopartition Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).</span></span>
   
    <span data-ttu-id="482fd-194">hello szolgáltatás neve megkülönbözteti a hello űrlap háló URI: /&lt;$application_name&gt;/&lt;service_name&gt;.</span><span class="sxs-lookup"><span data-stu-id="482fd-194">hello service name is a URI of hello form fabric:/&lt;application_name&gt;/&lt;service_name&gt;.</span></span>
   
    <span data-ttu-id="482fd-195">Két adatot, a Service Fabric egyedileg azonosíthatja a hello gép kérelmek kell küldeni.</span><span class="sxs-lookup"><span data-stu-id="482fd-195">With these two pieces of information, Service Fabric can uniquely identify hello machine that requests should be sent to.</span></span> <span data-ttu-id="482fd-196">Hello `ServiceProxy` osztály is gördülékenyen kezeli hello esetben hello állapotalapú szolgáltatási partíció üzemeltető hello gép sikertelen lesz, és egy másik gépen kell lennie az előléptetett tootake helyére.</span><span class="sxs-lookup"><span data-stu-id="482fd-196">hello `ServiceProxy` class also seamlessly handles hello case where hello machine that hosts hello stateful service partition fails and another machine must be promoted tootake its place.</span></span> <span data-ttu-id="482fd-197">Ez az absztrakció révén írás hello ügyfél kód toodeal jelentősen egyszerűbb más szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="482fd-197">This abstraction makes writing hello client code toodeal with other services significantly simpler.</span></span>
   
    <span data-ttu-id="482fd-198">Miután tudunk hello proxy, azt egyszerűen meghívása hello `GetCountAsync` metódus és az eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="482fd-198">Once we have hello proxy, we simply invoke hello `GetCountAsync` method and return its result.</span></span>

5. <span data-ttu-id="482fd-199">Nyomja le az F5 újra toorun hello módosított alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="482fd-199">Press F5 again toorun hello modified application.</span></span> <span data-ttu-id="482fd-200">Mint előtt, a Visual Studio automatikusan elindítja hello böngésző toohello hello webes projekt gyökerében.</span><span class="sxs-lookup"><span data-stu-id="482fd-200">As before, Visual Studio automatically launches hello browser toohello root of hello web project.</span></span> <span data-ttu-id="482fd-201">Hello "API-t és-értékek" elérési út, és megtekintheti az hello aktuális számlálóérték adott vissza.</span><span class="sxs-lookup"><span data-stu-id="482fd-201">Add hello "api/values" path, and you should see hello current counter value returned.</span></span>
   
    ![hello állapot-nyilvántartó számlálóérték hello böngészőben jelenik meg][browser-aspnet-counter-value]
   
    <span data-ttu-id="482fd-203">Frissítse a böngészőt hello rendszeres időközönként a toosee hello számláló értéke frissítés.</span><span class="sxs-lookup"><span data-stu-id="482fd-203">Refresh hello browser periodically toosee hello counter value update.</span></span>

## <a name="kestrel-and-weblistener"></a><span data-ttu-id="482fd-204">Vércse és WebListener</span><span class="sxs-lookup"><span data-stu-id="482fd-204">Kestrel and WebListener</span></span>

<span data-ttu-id="482fd-205">hello alapértelmezett ASP.NET Core webkiszolgáló, vércse, úgynevezett van [jelenleg nem támogatott a közvetlen internet-forgalom kezelésére](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="482fd-205">hello default ASP.NET Core web server, known as Kestrel, is [not currently supported for handling direct internet traffic](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span></span> <span data-ttu-id="482fd-206">Ennek eredményeképpen hello ASP.NET Core állapotmentes szolgáltatások a sablon a Service Fabric által [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="482fd-206">As a result, hello ASP.NET Core stateless service template for Service Fabric uses [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) by default.</span></span> 

<span data-ttu-id="482fd-207">a Service Fabric szolgáltatások bővebben vércse és WebListener toolearn tekintse meg a túl[ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="482fd-207">toolearn more about Kestrel and WebListener in Service Fabric services, please refer too[ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

## <a name="connecting-tooa-reliable-actor-service"></a><span data-ttu-id="482fd-208">Csatlakozás tooa megbízható szereplő szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="482fd-208">Connecting tooa Reliable Actor service</span></span>
<span data-ttu-id="482fd-209">Ez az oktatóanyag összpontosított hozzáadása egy előtér-webkiszolgáló, amely egy állapotalapú szolgáltatással továbbítani.</span><span class="sxs-lookup"><span data-stu-id="482fd-209">This tutorial focused on adding a web front end that communicated with a stateful service.</span></span> <span data-ttu-id="482fd-210">Egy nagyon hasonló modell tootalk tooactors is végrehajthatja.</span><span class="sxs-lookup"><span data-stu-id="482fd-210">However, you can follow a very similar model tootalk tooactors.</span></span> <span data-ttu-id="482fd-211">Visual Studio megbízható szereplő projekt létrehozása, automatikusan létrehoz egy illesztőfelület-projekt meg.</span><span class="sxs-lookup"><span data-stu-id="482fd-211">When you create a Reliable Actor project, Visual Studio automatically generates an interface project for you.</span></span> <span data-ttu-id="482fd-212">Használhatja a felület toogenerate az aktor proxy hello webes projekt toocommunicate hello résztvevővel.</span><span class="sxs-lookup"><span data-stu-id="482fd-212">You can use that interface toogenerate an actor proxy in hello web project toocommunicate with hello actor.</span></span> <span data-ttu-id="482fd-213">hello kommunikációs csatorna számára automatikusan biztosított.</span><span class="sxs-lookup"><span data-stu-id="482fd-213">hello communication channel is provided automatically.</span></span> <span data-ttu-id="482fd-214">Így toodo nem kell semmi, hogy-e megfelelő tooestablishing egy `ServiceRemotingListener` például ebben az oktatóanyagban hello állapotalapú szolgáltatás esetében tette.</span><span class="sxs-lookup"><span data-stu-id="482fd-214">So you do not need toodo anything that is equivalent tooestablishing a `ServiceRemotingListener` like you did for hello stateful service in this tutorial.</span></span>

## <a name="how-web-services-work-on-your-local-cluster"></a><span data-ttu-id="482fd-215">A helyi fürtön lévő webes szolgáltatások működése</span><span class="sxs-lookup"><span data-stu-id="482fd-215">How web services work on your local cluster</span></span>
<span data-ttu-id="482fd-216">Általánosságban elmondható pontosan hello azonos a Service Fabric application tooa több gép fürtön, amikor központilag telepített a helyi fürthöz, és lehet magas benne, hogy működik a várt módon telepítheti.</span><span class="sxs-lookup"><span data-stu-id="482fd-216">In general, you can deploy exactly hello same Service Fabric application tooa multi-machine cluster that you deployed on your local cluster and be highly confident that it works as you expect.</span></span> <span data-ttu-id="482fd-217">Ez azért, mert a helyi fürt egyszerűen egy öt-csomópont-konfiguráció, amely összecsukott tooa egyetlen számítógépen.</span><span class="sxs-lookup"><span data-stu-id="482fd-217">This is because your local cluster is simply a five-node configuration that is collapsed tooa single machine.</span></span>

<span data-ttu-id="482fd-218">Tooweb szolgáltatások ismét, azonban nincs egy kulcs nuance.</span><span class="sxs-lookup"><span data-stu-id="482fd-218">When it comes tooweb services, however, there is one key nuance.</span></span> <span data-ttu-id="482fd-219">Ha a fürt egy terheléselosztó mögött helyezkedik el, mint az Azure-ban, győződjön meg arról, hogy a webes szolgáltatások telepítve vannak minden egyes számítógépen óta hello terheléselosztó egyszerűen ciklikus multiplexelések hello gépek közötti forgalmat.</span><span class="sxs-lookup"><span data-stu-id="482fd-219">When your cluster sits behind a load balancer, as it does in Azure, you must ensure that your web services are deployed on every machine since hello load balancer simply round-robins traffic across hello machines.</span></span> <span data-ttu-id="482fd-220">Ehhez által hello beállítása `InstanceCount` hello szolgáltatás toohello különleges érték-1".</span><span class="sxs-lookup"><span data-stu-id="482fd-220">You can do this by setting hello `InstanceCount` for hello service toohello special value of "-1".</span></span>

<span data-ttu-id="482fd-221">Ezzel szemben, amikor helyileg futtat egy webszolgáltatás-bővítmény, kell tooensure hello szolgáltatást, hogy csak egy példányban fut.</span><span class="sxs-lookup"><span data-stu-id="482fd-221">By contrast, when you run a web service locally, you need tooensure that only one instance of hello service is running.</span></span> <span data-ttu-id="482fd-222">Ellenkező esetben futtatnia az ütközések több folyamat által figyelt hello ugyanazt az elérési út és a port.</span><span class="sxs-lookup"><span data-stu-id="482fd-222">Otherwise, you run into conflicts from multiple processes that are listening on hello same path and port.</span></span> <span data-ttu-id="482fd-223">Ennek eredményeképpen hello web service példányszám beállításaként túl "1" helyi telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="482fd-223">As a result, hello web service instance count should be set too"1" for local deployments.</span></span>

<span data-ttu-id="482fd-224">Hogyan tooconfigure eltérő értékek tartoznak különböző környezetben: toolearn [alkalmazás paramétereinek több környezet kezelése](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="482fd-224">toolearn how tooconfigure different values for different environment, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="482fd-225">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="482fd-225">Next steps</span></span>
<span data-ttu-id="482fd-226">Most, hogy a webalkalmazás első set végül az ASP.NET Core az alkalmazáshoz, további információ [ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) az ASP.NET Core működése a Service Fabric alapos ismeretét.</span><span class="sxs-lookup"><span data-stu-id="482fd-226">Now that you have a web front end set up for your application with ASP.NET Core, learn more about [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) for an in-depth understanding of how ASP.NET Core works with Service Fabric.</span></span>

<span data-ttu-id="482fd-227">Ezt követően [tudhat meg többet a szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md) általában a teljes tooget kép, hogyan szolgáltatás a kommunikációt a Service Fabric működik.</span><span class="sxs-lookup"><span data-stu-id="482fd-227">Next, [learn more about communicating with services](service-fabric-connect-and-communicate-with-services.md) in general tooget a complete picture of how service communication works in Service Fabric.</span></span>

<span data-ttu-id="482fd-228">Ha elvégezte a szolgáltatások közötti kommunikáció működésével, alaposan megismernie [fürt létrehozása az Azure-ban, és telepítheti az alkalmazás toohello felhő](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="482fd-228">Once you have a good understanding of how service communication works, [create a cluster in Azure and deploy your application toohello cloud](service-fabric-cluster-creation-via-portal.md).</span></span>

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
