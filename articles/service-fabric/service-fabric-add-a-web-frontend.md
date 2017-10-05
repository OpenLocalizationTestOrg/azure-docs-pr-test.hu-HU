---
title: "Egy előtér-webkiszolgáló az ASP.NET Core segítségével Azure Service Fabric-alkalmazás létrehozása |} Microsoft Docs"
description: "Teszi közzé a weben a Service Fabric-alkalmazás használatával, az ASP.NET Core projektet és a szolgáltatások közötti kommunikációs szolgáltatás távoli eljáráshívás keresztül."
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
ms.openlocfilehash: b19aaa652f2c15573ded632ca1348e1a6752f080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a><span data-ttu-id="568fa-103">A szolgáltatás előtér-webkiszolgáló az ASP.NET Core segítségével alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="568fa-103">Build a web service front end for your application using ASP.NET Core</span></span>
<span data-ttu-id="568fa-104">Alapértelmezés szerint Azure Service Fabric-szolgáltatások nem biztosítanak egy nyilvános webes felületet.</span><span class="sxs-lookup"><span data-stu-id="568fa-104">By default, Azure Service Fabric services do not provide a public interface to the web.</span></span> <span data-ttu-id="568fa-105">Teszi közzé a HTTP-ügyfelek számára az alkalmazás működését, akkor belépési pontként személyekről, és onnan az egyes szolgáltatások majd kommunikálni webes projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="568fa-105">To expose your application's functionality to HTTP clients, you have to create a web project to act as an entry point and then communicate from there to your individual services.</span></span>

<span data-ttu-id="568fa-106">Az oktatóanyag azt átvételéhez ahol azt korábban befejezte a a [az első alkalmazás létrehozásának a Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) oktatóanyagot, és adja hozzá az ASP.NET Core webszolgáltatás előtt az állapot-nyilvántartó teljesítményszámláló szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="568fa-106">In this tutorial, we pick up where we left off in the [Creating your first application in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) tutorial and add an ASP.NET Core web service in front of the stateful counter service.</span></span> <span data-ttu-id="568fa-107">Ha még nem tette meg, lépjen vissza, és, hogy az oktatóanyag első lépéseit.</span><span class="sxs-lookup"><span data-stu-id="568fa-107">If you have not already done so, you should go back and step through that tutorial first.</span></span>

## <a name="set-up-your-environment-for-aspnet-core"></a><span data-ttu-id="568fa-108">Állítsa be a környezetet az ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="568fa-108">Set up your environment for ASP.NET Core</span></span>

<span data-ttu-id="568fa-109">A Visual Studio 2017 együtt az oktatóanyag követéséhez van szüksége.</span><span class="sxs-lookup"><span data-stu-id="568fa-109">You need Visual Studio 2017 to follow along with this tutorial.</span></span> <span data-ttu-id="568fa-110">Bármely edition fog tenni.</span><span class="sxs-lookup"><span data-stu-id="568fa-110">Any edition will do.</span></span> 

 - [<span data-ttu-id="568fa-111">Telepítse a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="568fa-111">Install Visual Studio 2017</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="568fa-112">Az ASP.NET Core Service Fabric-alkalmazások fejlesztésével, rendelkeznie kell az alábbi munkaterhelések telepítve:</span><span class="sxs-lookup"><span data-stu-id="568fa-112">To develop ASP.NET Core Service Fabric applications, you should have the following workloads installed:</span></span>
 - <span data-ttu-id="568fa-113">**Azure fejlesztési** (alatt *webes & felhő*)</span><span class="sxs-lookup"><span data-stu-id="568fa-113">**Azure development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="568fa-114">**Az ASP.NET és a webes fejlesztési** (alatt *webes & felhő*)</span><span class="sxs-lookup"><span data-stu-id="568fa-114">**ASP.NET and web development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="568fa-115">**A .NET core platformfüggetlen fejlesztésekhez** (alatt *egyéb Toolsets*)</span><span class="sxs-lookup"><span data-stu-id="568fa-115">**.NET Core cross-platform development** (under *Other Toolsets*)</span></span>

>[!Note] 
><span data-ttu-id="568fa-116">A .NET Core eszközöket a Visual Studio 2015 már nem frissítés alatt álló, ha a Visual Studio 2015 még használja, meg kell azonban [.NET Core VS 2015 tooling eszköz Preview 2](https://www.microsoft.com/net/download/core) telepítve.</span><span class="sxs-lookup"><span data-stu-id="568fa-116">The .NET Core tools for Visual Studio 2015 are no longer being updated, however if you are still using Visual Studio 2015, you need to have [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) installed.</span></span>

## <a name="add-an-aspnet-core-service-to-your-application"></a><span data-ttu-id="568fa-117">Az ASP.NET Core szolgáltatás hozzá az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="568fa-117">Add an ASP.NET Core service to your application</span></span>
<span data-ttu-id="568fa-118">Az ASP.NET Core egy egyszerűsített, platformfüggetlen webes fejlesztési keretrendszer, amely a webes API-k és modern webes felhasználói felület létrehozása segítségével.</span><span class="sxs-lookup"><span data-stu-id="568fa-118">ASP.NET Core is a lightweight, cross-platform web development framework that you can use to create modern web UI and web APIs.</span></span> 

<span data-ttu-id="568fa-119">Adjuk hozzá az ASP.NET Web API-projektet a meglévő alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="568fa-119">Let's add an ASP.NET Web API project to our existing application.</span></span>

1. <span data-ttu-id="568fa-120">A Megoldáskezelőben kattintson a jobb gombbal **szolgáltatások** az alkalmazásban le, és válassza a **Hozzáadás > új Service Fabric-szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="568fa-120">In Solution Explorer, right-click **Services** within the application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Egy új szolgáltatás hozzáadása meglévő alkalmazáshoz][vs-add-new-service]
2. <span data-ttu-id="568fa-122">Az a **szolgáltatás létrehozása** lapon, válassza ki **ASP.NET Core** és adjon neki egy nevet.</span><span class="sxs-lookup"><span data-stu-id="568fa-122">On the **Create a Service** page, choose **ASP.NET Core** and give it a name.</span></span>
   
    ![ASP.NET webszolgáltatás kiválasztása az új service párbeszédpanelen][vs-new-service-dialog]

3. <span data-ttu-id="568fa-124">A következő oldalon biztosít az ASP.NET Core projektsablonjai.</span><span class="sxs-lookup"><span data-stu-id="568fa-124">The next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="568fa-125">Ne feledje, hogy ezek a azonos lehetőségek, hogy mutatunk be Ha az ASP.NET Core projektek kívül a Service Fabric-alkalmazás, a szolgáltatás regisztrálására a Service Fabric-futtatókörnyezet kód kis mennyiségű hozott létre.</span><span class="sxs-lookup"><span data-stu-id="568fa-125">Note that these are the same choices that you would see if you created an ASP.NET Core project outside of a Service Fabric application, with a small amount of additional code to register the service with the Service Fabric runtime.</span></span> <span data-ttu-id="568fa-126">A jelen oktatóanyag esetében válassza ki a **Web API**.</span><span class="sxs-lookup"><span data-stu-id="568fa-126">For this tutorial, choose **Web API**.</span></span> <span data-ttu-id="568fa-127">Azonban koncepciók is alkalmazhat a teljes webalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="568fa-127">However, you can apply the same concepts to building a full web application.</span></span>
   
    ![ASP.NET-projekt kiválasztása][vs-new-aspnet-project-dialog]
   
    <span data-ttu-id="568fa-129">A Web API-projekt létrehozása után az alkalmazás rendelkeznie kell két szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="568fa-129">Once your Web API project is created, you should have two services in your application.</span></span> <span data-ttu-id="568fa-130">Továbbra is építenie az alkalmazást, mert a további szolgáltatások pontosan azonos módon is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="568fa-130">As you continue to build your application, you can add more services in exactly the same way.</span></span> <span data-ttu-id="568fa-131">Minden egyes lehet függetlenül rendszerverzióval ellátott és frissített.</span><span class="sxs-lookup"><span data-stu-id="568fa-131">Each can be independently versioned and upgraded.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="568fa-132">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="568fa-132">Run the application</span></span>
<span data-ttu-id="568fa-133">Ahhoz, hogy mi azt régebben már kötöttek egyfajta, most az új alkalmazás központi telepítése, és tekintse meg az alapértelmezett viselkedést, amely az ASP.NET Core webes API-sablon.</span><span class="sxs-lookup"><span data-stu-id="568fa-133">To get a sense of what we've done, let's deploy the new application and take a look at the default behavior that the ASP.NET Core Web API template provides.</span></span>

1. <span data-ttu-id="568fa-134">Nyomja le az F5 billentyűt az alkalmazás hibakeresése a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="568fa-134">Press F5 in Visual Studio to debug the app.</span></span>
2. <span data-ttu-id="568fa-135">Központi telepítés befejeződése után a Visual Studio elindítja egy böngészőt, és az ASP.NET Web API-szolgáltatás a legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="568fa-135">When deployment is complete, Visual Studio launches a browser to the root of the ASP.NET Web API service.</span></span> <span data-ttu-id="568fa-136">Az ASP.NET Core Web API nem biztosít alapértelmezett viselkedése a gyökér, 404 hibaüzenetet a böngészőben megjelenik.</span><span class="sxs-lookup"><span data-stu-id="568fa-136">The ASP.NET Core Web API template doesn't provide default behavior for the root, so you should see a 404 error in the browser.</span></span>
3. <span data-ttu-id="568fa-137">Adja hozzá `/api/values` a böngészőben a helyre.</span><span class="sxs-lookup"><span data-stu-id="568fa-137">Add `/api/values` to the location in the browser.</span></span> <span data-ttu-id="568fa-138">Ez elindítja a `Get` a webes API-sablonban ValuesController metódust.</span><span class="sxs-lookup"><span data-stu-id="568fa-138">This invokes the `Get` method on the ValuesController in the Web API template.</span></span> <span data-ttu-id="568fa-139">A sablon – két karakterláncokat tartalmazó JSON-tömb által biztosított alapértelmezett válasz adja vissza:</span><span class="sxs-lookup"><span data-stu-id="568fa-139">It returns the default response that is provided by the template--a JSON array that contains two strings:</span></span>
   
    ![Az ASP.NET Core Web API sablonból visszaadott alapértelmezett értékek][browser-aspnet-template-values]
   
    <span data-ttu-id="568fa-141">Az oktatóanyag végén ezen az oldalon jelennek meg a legutóbbi számláló értéke az alapértelmezett karakterlánc helyett az állapotalapú szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="568fa-141">By the end of the tutorial, this page will show the most recent counter value from our stateful service instead of the default strings.</span></span>

## <a name="connect-the-services"></a><span data-ttu-id="568fa-142">Csatlakozás a szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="568fa-142">Connect the services</span></span>
<span data-ttu-id="568fa-143">A Service Fabric hogyan kommunikáljanak megbízható szolgáltatások teljes rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="568fa-143">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="568fa-144">Egyetlen alkalmazásban lehetséges, hogy szolgáltatásokat, amelyek a TCP, más szolgáltatásokkal, hogy elérhető-e egy HTTP REST API-n keresztül, és továbbra is más szolgáltatások, amelyek elérhetők a webes szoftvercsatornák keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="568fa-144">Within a single application, you might have services that are accessible via TCP, other services that are accessible via an HTTP REST API, and still other services that are accessible via web sockets.</span></span> <span data-ttu-id="568fa-145">A rendelkezésre álló lehetőségeket, és a mellékhatásokkal jár a háttérben, lásd: [szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="568fa-145">For background on the options available and the tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span> <span data-ttu-id="568fa-146">Ebben az oktatóanyagban használjuk [Service Fabric szolgáltatás távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md), az SDK-ban megadott.</span><span class="sxs-lookup"><span data-stu-id="568fa-146">In this tutorial, we use [Service Fabric Service Remoting](service-fabric-reliable-services-communication-remoting.md), provided in the SDK.</span></span>

<span data-ttu-id="568fa-147">A szolgáltatás távoli eljáráshívási megközelítéssel (tapasztalatait távoli eljáráshívások vagy RPC-k) adja meg a szolgáltatás a nyilvános szerződés nevében járhasson el illesztőfelület.</span><span class="sxs-lookup"><span data-stu-id="568fa-147">In the Service Remoting approach (modeled on remote procedure calls or RPCs), you define an interface to act as the public contract for the service.</span></span> <span data-ttu-id="568fa-148">Ezt követően használja a kapcsolat a proxy osztály a szolgáltatással az létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="568fa-148">Then, you use that interface to generate a proxy class for interacting with the service.</span></span>

### <a name="create-the-remoting-interface"></a><span data-ttu-id="568fa-149">A távelérési kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="568fa-149">Create the remoting interface</span></span>
<span data-ttu-id="568fa-150">Először hozzon létre a kapcsolat működjön, és a szerződés az állapotalapú szolgáltatásból és egyéb szolgáltatások között, ebben az esetben az ASP.NET Core webes projekt.</span><span class="sxs-lookup"><span data-stu-id="568fa-150">Let's start by creating the interface to act as the contract between the stateful service and other services, in this case the ASP.NET Core web project.</span></span> <span data-ttu-id="568fa-151">Ez az interfész által hívásokat RPC, így a saját Class Library projektben létrehozunk használó összes szolgáltatások kell osztani.</span><span class="sxs-lookup"><span data-stu-id="568fa-151">This interface must be shared by all services that use it to make RPC calls, so we'll create it in its own Class Library project.</span></span>

1. <span data-ttu-id="568fa-152">A Megoldáskezelőben kattintson a jobb gombbal a megoldás, és válassza a **Hozzáadás** > **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="568fa-152">In Solution Explorer, right-click your solution and choose **Add** > **New Project**.</span></span>

2. <span data-ttu-id="568fa-153">Válassza ki a **Visual C#** bejegyzést a bal oldali navigációs panelen, és válassza ki a **Class Library** sablont.</span><span class="sxs-lookup"><span data-stu-id="568fa-153">Choose the **Visual C#** entry in the left navigation pane and then select the **Class Library** template.</span></span> <span data-ttu-id="568fa-154">Győződjön meg arról, hogy a .NET-keretrendszer verziója van-e állítva **4.5.2-es**.</span><span class="sxs-lookup"><span data-stu-id="568fa-154">Ensure that the .NET Framework version is set to **4.5.2**.</span></span>
   
    ![Az állapotalapú szolgáltatás felület projekt létrehozása][vs-add-class-library-project]

3. <span data-ttu-id="568fa-156">Telepítse a **Microsoft.ServiceFabric.Services.Remoting** NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="568fa-156">Install the **Microsoft.ServiceFabric.Services.Remoting** NuGet package.</span></span> <span data-ttu-id="568fa-157">Keresse meg **Microsoft.ServiceFabric.Services.Remoting** a nuget csomag manager, és telepítse az összes projektet, melyekben a megoldásban, amelyek a távelérés szolgáltatás többek között:</span><span class="sxs-lookup"><span data-stu-id="568fa-157">Search for  **Microsoft.ServiceFabric.Services.Remoting** in the NuGet package manager and install it for all projects in the solution that use Service Remoting, including:</span></span>
   - <span data-ttu-id="568fa-158">A Class Library projekt, amely tartalmazza a szolgáltatási felület</span><span class="sxs-lookup"><span data-stu-id="568fa-158">The Class Library project that contains the service interface</span></span>
   - <span data-ttu-id="568fa-159">Az állapotalapú szolgáltatási projektet</span><span class="sxs-lookup"><span data-stu-id="568fa-159">The Stateful Service project</span></span>
   - <span data-ttu-id="568fa-160">Az ASP.NET Core webes projekt</span><span class="sxs-lookup"><span data-stu-id="568fa-160">The ASP.NET Core web service project</span></span>
   
    ![A szolgáltatások a NuGet-csomag hozzáadása][vs-services-nuget-package]

4. <span data-ttu-id="568fa-162">A osztály könyvtárban hozzon létre egy felület egy módszert, `GetCountAsync`, és megnöveli a felületének a `Microsoft.ServiceFabric.Services.Remoting.IService`.</span><span class="sxs-lookup"><span data-stu-id="568fa-162">In the class library, create an interface with a single method, `GetCountAsync`, and extend the interface from `Microsoft.ServiceFabric.Services.Remoting.IService`.</span></span> <span data-ttu-id="568fa-163">A távoli eljáráshívás felületen Ez az interfész azt jelzi, hogy ez a szolgáltatás távoli eljáráshívási felületet kell származnia.</span><span class="sxs-lookup"><span data-stu-id="568fa-163">The remoting interface must derive from this interface to indicate that it is a Service Remoting interface.</span></span>
   
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

### <a name="implement-the-interface-in-your-stateful-service"></a><span data-ttu-id="568fa-164">A felület megvalósítása az állapot-nyilvántartó szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="568fa-164">Implement the interface in your stateful service</span></span>
<span data-ttu-id="568fa-165">Most, hogy definiáltuk a felületet, igazolnia kell megvalósítani, az állapotalapú szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="568fa-165">Now that we have defined the interface, we need to implement it in the stateful service.</span></span>

1. <span data-ttu-id="568fa-166">Az állapot-nyilvántartó szolgáltatásban a, amely tartalmazza a felület hordozhatóosztálytár-projektjének mutató hivatkozás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="568fa-166">In your stateful service, add a reference to the class library project that contains the interface.</span></span>
   
    ![Az állapot-nyilvántartó szolgáltatásban a hordozhatóosztálytár-projektjének mutató hivatkozás hozzáadása][vs-add-class-library-reference]
2. <span data-ttu-id="568fa-168">Keresse meg a osztályból származó `StatefulService`, például a `MyStatefulService`, és végrehajtásához kiterjesztése a `ICounter` felületet.</span><span class="sxs-lookup"><span data-stu-id="568fa-168">Locate the class that inherits from `StatefulService`, such as `MyStatefulService`, and extend it to implement the `ICounter` interface.</span></span>
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. <span data-ttu-id="568fa-169">Most valósítja meg az egyetlen definiált a `ICounter` felületet, `GetCountAsync`.</span><span class="sxs-lookup"><span data-stu-id="568fa-169">Now implement the single method that is defined in the `ICounter` interface, `GetCountAsync`.</span></span>
   
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

### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a><span data-ttu-id="568fa-170">Az állapotalapú szolgáltatás használata a távoli eljáráshívási szolgáltatás figyelő indításához</span><span class="sxs-lookup"><span data-stu-id="568fa-170">Expose the stateful service using a service remoting listener</span></span>
<span data-ttu-id="568fa-171">Az a `ICounter` megvalósítva, a végső lépés objektumfelület. a szolgáltatás távoli eljáráshívási kommunikációs csatorna megnyitását.</span><span class="sxs-lookup"><span data-stu-id="568fa-171">With the `ICounter` interface implemented, the final step is to open the Service Remoting communication channel.</span></span> <span data-ttu-id="568fa-172">Állapotalapú szolgáltatások esetén a Service Fabric nevű felülbírálható módszert kínál `CreateServiceReplicaListeners`.</span><span class="sxs-lookup"><span data-stu-id="568fa-172">For stateful services, Service Fabric provides an overridable method called `CreateServiceReplicaListeners`.</span></span> <span data-ttu-id="568fa-173">Ezzel a módszerrel megadhatja egy vagy több kommunikációs figyelőket, a kommunikációt, amelyre később engedélyezni kívánja a szolgáltatás típusa alapján.</span><span class="sxs-lookup"><span data-stu-id="568fa-173">With this method, you can specify one or more communication listeners, based on the type of communication that you want to enable for your service.</span></span>

> [!NOTE]
> <span data-ttu-id="568fa-174">Az állapotmentes szolgáltatások kommunikációs csatorna megnyitásakor egyenértékű módszer neve `CreateServiceInstanceListeners`.</span><span class="sxs-lookup"><span data-stu-id="568fa-174">The equivalent method for opening a communication channel to stateless services is called `CreateServiceInstanceListeners`.</span></span>

<span data-ttu-id="568fa-175">Ebben az esetben azt lecserélheti a meglévő `CreateServiceReplicaListeners` metódust, és adjon meg egy példánya `ServiceRemotingListener`, ami, amely létrehoz egy RPC-végpont metódus akkor hívható meg az ügyfelek a `ServiceProxy`.</span><span class="sxs-lookup"><span data-stu-id="568fa-175">In this case, we replace the existing `CreateServiceReplicaListeners` method and provide an instance of `ServiceRemotingListener`, which creates an RPC endpoint that is callable from clients through `ServiceProxy`.</span></span>  

<span data-ttu-id="568fa-176">A `CreateServiceRemotingListener` bővítmény metódust a `IService` felület lehetővé teszi, hogy könnyen létrehozhat egy `ServiceRemotingListener` alapértelmezett beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="568fa-176">The `CreateServiceRemotingListener` extension method on the `IService` interface allows you to easily create a `ServiceRemotingListener` with all default settings.</span></span> <span data-ttu-id="568fa-177">A bővítmény módszer használatához, ellenőrizze, hogy a `Microsoft.ServiceFabric.Services.Remoting.Runtime` importált névtér.</span><span class="sxs-lookup"><span data-stu-id="568fa-177">To use this extension method, ensure you have the `Microsoft.ServiceFabric.Services.Remoting.Runtime` namespace imported.</span></span> 

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


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a><span data-ttu-id="568fa-178">A szolgáltatás együttműködhet az ServiceProxy osztály használatával</span><span class="sxs-lookup"><span data-stu-id="568fa-178">Use the ServiceProxy class to interact with the service</span></span>
<span data-ttu-id="568fa-179">Az állapotalapú szolgáltatás más szolgáltatások forgalom fogadására RPC-n keresztül készen áll.</span><span class="sxs-lookup"><span data-stu-id="568fa-179">Our stateful service is now ready to receive traffic from other services over RPC.</span></span> <span data-ttu-id="568fa-180">Így továbbra is kommunikálni az ASP.NET-webszolgáltatás a kód hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="568fa-180">So all that remains is adding the code to communicate with it from the ASP.NET web service.</span></span>

1. <span data-ttu-id="568fa-181">Az ASP.NET projektben adja hozzá egy hivatkozást a osztály, amely tartalmazza a `ICounter` felületet.</span><span class="sxs-lookup"><span data-stu-id="568fa-181">In your ASP.NET project, add a reference to the class library that contains the `ICounter` interface.</span></span>

2. <span data-ttu-id="568fa-182">Korábban hozzáadott a **Microsoft.ServiceFabric.Services.Remoting** NuGet-csomagot kell az ASP.NET-projekt.</span><span class="sxs-lookup"><span data-stu-id="568fa-182">Earlier you added the **Microsoft.ServiceFabric.Services.Remoting** NuGet package to the ASP.NET project.</span></span> <span data-ttu-id="568fa-183">Ez a csomag biztosítja a `ServiceProxy` osztályt, amely RPC végrehajtásához használhatja az állapotalapú szolgáltatásból hívások.</span><span class="sxs-lookup"><span data-stu-id="568fa-183">This package provides the `ServiceProxy` class which you use to make RPC calls to the stateful service.</span></span> <span data-ttu-id="568fa-184">Győződjön meg arról, a NuGet csomag telepítve van az ASP.NET Core web service projektben.</span><span class="sxs-lookup"><span data-stu-id="568fa-184">Ensure this NuGet package is installed in the ASP.NET Core web service project.</span></span>

4. <span data-ttu-id="568fa-185">Az a **tartományvezérlők** mappa, nyissa meg a `ValuesController` osztály.</span><span class="sxs-lookup"><span data-stu-id="568fa-185">In the **Controllers** folder, open the `ValuesController` class.</span></span> <span data-ttu-id="568fa-186">Vegye figyelembe, hogy a `Get` metódus jelenleg csak a "érték1" és "érték2" – amely megfelel a Mi a böngészőben korábban látott kódolt karakterlánc tömbje ad vissza.</span><span class="sxs-lookup"><span data-stu-id="568fa-186">Note that the `Get` method currently just returns a hard-coded string array of "value1" and "value2"--which matches what we saw earlier in the browser.</span></span> <span data-ttu-id="568fa-187">Ez a megvalósítás cserélje le a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="568fa-187">Replace this implementation with the following code:</span></span>
   
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
   
    <span data-ttu-id="568fa-188">Az első kódsort, a kulcs egy.</span><span class="sxs-lookup"><span data-stu-id="568fa-188">The first line of code is the key one.</span></span> <span data-ttu-id="568fa-189">Az állapotalapú szolgáltatásból a ICounter proxy létrehozásához meg kell adnia kétféle információt: a Partícióazonosító és a szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="568fa-189">To create the ICounter proxy to the stateful service, you need to provide two pieces of information: a partition ID and the name of the service.</span></span>
   
    <span data-ttu-id="568fa-190">Használhatja a particionálás méretezési állapotalapú szolgáltatások által a különböző gyűjtők állapotukra összeállításának, egy definiálhat, például a felhasználói azonosító vagy irányítószámot kulcs alapján.</span><span class="sxs-lookup"><span data-stu-id="568fa-190">You can use partitioning to scale stateful services by breaking up their state into different buckets, based on a key that you define, such as a customer ID or postal code.</span></span> <span data-ttu-id="568fa-191">Trivial alkalmazásban. az állapotalapú szolgáltatásból csak rendelkezik egy partíciót, így a kulcs nem számít.</span><span class="sxs-lookup"><span data-stu-id="568fa-191">In our trivial application, the stateful service only has one partition, so the key doesn't matter.</span></span> <span data-ttu-id="568fa-192">Bármely Ön által biztosított kulcs partícióra irányítja.</span><span class="sxs-lookup"><span data-stu-id="568fa-192">Any key that you provide will lead to the same partition.</span></span> <span data-ttu-id="568fa-193">Szolgáltatások partícionálásra vonatkozó további tudnivalókért lásd: [particionálása Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="568fa-193">To learn more about partitioning services, see [How to partition Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).</span></span>
   
    <span data-ttu-id="568fa-194">A szolgáltatásnév megadása egy URI-címe, az űrlap fabric: /&lt;$application_name&gt;/&lt;service_name&gt;.</span><span class="sxs-lookup"><span data-stu-id="568fa-194">The service name is a URI of the form fabric:/&lt;application_name&gt;/&lt;service_name&gt;.</span></span>
   
    <span data-ttu-id="568fa-195">Két adatot, a Service Fabric egyedileg azonosíthatja a gépet, a kérelmek kell küldeni.</span><span class="sxs-lookup"><span data-stu-id="568fa-195">With these two pieces of information, Service Fabric can uniquely identify the machine that requests should be sent to.</span></span> <span data-ttu-id="568fa-196">A `ServiceProxy` osztály zökkenőmentesen is kezeli az esetben a számítógép, amelyen az állapotalapú szolgáltatási partíció nem sikerül, és egy másik gépen kell előléptetni a sor.</span><span class="sxs-lookup"><span data-stu-id="568fa-196">The `ServiceProxy` class also seamlessly handles the case where the machine that hosts the stateful service partition fails and another machine must be promoted to take its place.</span></span> <span data-ttu-id="568fa-197">Ezt az absztrakciót, lehetővé teszi az ügyfél programozás jelentősen egyszerűbb más szolgáltatásokkal foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="568fa-197">This abstraction makes writing the client code to deal with other services significantly simpler.</span></span>
   
    <span data-ttu-id="568fa-198">A proxy van, ha azt egyszerűen meghívása a `GetCountAsync` metódus és az eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="568fa-198">Once we have the proxy, we simply invoke the `GetCountAsync` method and return its result.</span></span>

5. <span data-ttu-id="568fa-199">Nyomja le az F5 újra a módosított alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="568fa-199">Press F5 again to run the modified application.</span></span> <span data-ttu-id="568fa-200">Mivel előtt, a Visual Studio automatikusan elindítja a böngésző számára a webes projekt gyökerében.</span><span class="sxs-lookup"><span data-stu-id="568fa-200">As before, Visual Studio automatically launches the browser to the root of the web project.</span></span> <span data-ttu-id="568fa-201">A "API-t és-értékek" elérési út, és az aktuális számlálóérték vissza kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="568fa-201">Add the "api/values" path, and you should see the current counter value returned.</span></span>
   
    ![Az állapot-nyilvántartó számlálóérték jelenik meg a böngészőben][browser-aspnet-counter-value]
   
    <span data-ttu-id="568fa-203">Frissítse a böngészőablakot, hogy rendszeresen tekintse meg a számláló értéke frissítése.</span><span class="sxs-lookup"><span data-stu-id="568fa-203">Refresh the browser periodically to see the counter value update.</span></span>

## <a name="kestrel-and-weblistener"></a><span data-ttu-id="568fa-204">Vércse és WebListener</span><span class="sxs-lookup"><span data-stu-id="568fa-204">Kestrel and WebListener</span></span>

<span data-ttu-id="568fa-205">Az alapértelmezett ASP.NET Core webkiszolgáló, vércse, úgynevezett [jelenleg nem támogatott a közvetlen internet-forgalom kezelésére](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="568fa-205">The default ASP.NET Core web server, known as Kestrel, is [not currently supported for handling direct internet traffic](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span></span> <span data-ttu-id="568fa-206">Ennek eredményeképpen a Service Fabric ASP.NET Core állapotmentes szolgáltatások sablonját használja [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="568fa-206">As a result, the ASP.NET Core stateless service template for Service Fabric uses [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) by default.</span></span> 

<span data-ttu-id="568fa-207">További információt a vércse és WebListener a Service Fabric-szolgáltatások, tekintse meg [ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="568fa-207">To learn more about Kestrel and WebListener in Service Fabric services, please refer to [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

## <a name="connecting-to-a-reliable-actor-service"></a><span data-ttu-id="568fa-208">Egy megbízható szereplő csatlakozik</span><span class="sxs-lookup"><span data-stu-id="568fa-208">Connecting to a Reliable Actor service</span></span>
<span data-ttu-id="568fa-209">Ez az oktatóanyag összpontosított hozzáadása egy előtér-webkiszolgáló, amely egy állapotalapú szolgáltatással továbbítani.</span><span class="sxs-lookup"><span data-stu-id="568fa-209">This tutorial focused on adding a web front end that communicated with a stateful service.</span></span> <span data-ttu-id="568fa-210">Egy nagyon hasonló modell felvegye a gyakrabban is végrehajthatja.</span><span class="sxs-lookup"><span data-stu-id="568fa-210">However, you can follow a very similar model to talk to actors.</span></span> <span data-ttu-id="568fa-211">Visual Studio megbízható szereplő projekt létrehozása, automatikusan létrehoz egy illesztőfelület-projekt meg.</span><span class="sxs-lookup"><span data-stu-id="568fa-211">When you create a Reliable Actor project, Visual Studio automatically generates an interface project for you.</span></span> <span data-ttu-id="568fa-212">Az aktor proxy létrehozni a szereplő folytatott kommunikációhoz a webes projekt használhatja, hogy a illesztő.</span><span class="sxs-lookup"><span data-stu-id="568fa-212">You can use that interface to generate an actor proxy in the web project to communicate with the actor.</span></span> <span data-ttu-id="568fa-213">A kommunikációs csatornát automatikusan valósul meg.</span><span class="sxs-lookup"><span data-stu-id="568fa-213">The communication channel is provided automatically.</span></span> <span data-ttu-id="568fa-214">Így nem kell tennie semmit, egyenértékű létrehozó egy `ServiceRemotingListener` például ebben az oktatóanyagban az állapotalapú szolgáltatásból esetében tette.</span><span class="sxs-lookup"><span data-stu-id="568fa-214">So you do not need to do anything that is equivalent to establishing a `ServiceRemotingListener` like you did for the stateful service in this tutorial.</span></span>

## <a name="how-web-services-work-on-your-local-cluster"></a><span data-ttu-id="568fa-215">A helyi fürtön lévő webes szolgáltatások működése</span><span class="sxs-lookup"><span data-stu-id="568fa-215">How web services work on your local cluster</span></span>
<span data-ttu-id="568fa-216">Általában pontosan az azonos a Service Fabric alkalmazást telepíti egy többgépes fürtben, a helyi fürtön lévő telepített, és lehet magas benne, hogy az elvárt módon működik.</span><span class="sxs-lookup"><span data-stu-id="568fa-216">In general, you can deploy exactly the same Service Fabric application to a multi-machine cluster that you deployed on your local cluster and be highly confident that it works as you expect.</span></span> <span data-ttu-id="568fa-217">Ez azért, mert a helyi fürt egyszerűen olyan öt csomópontból konfigurációt, amely egyetlen számítógépen van-e csukva.</span><span class="sxs-lookup"><span data-stu-id="568fa-217">This is because your local cluster is simply a five-node configuration that is collapsed to a single machine.</span></span>

<span data-ttu-id="568fa-218">A webszolgáltatások ismét, azonban nincs egy kulcs nuance.</span><span class="sxs-lookup"><span data-stu-id="568fa-218">When it comes to web services, however, there is one key nuance.</span></span> <span data-ttu-id="568fa-219">Ha a fürt egy terheléselosztó mögött helyezkedik el, mint az Azure-ban, győződjön meg arról, hogy a webes szolgáltatások telepítve vannak minden egyes számítógépen a terheléselosztó óta egyszerűen ciklikus multiplexelések a gépek közötti forgalmat.</span><span class="sxs-lookup"><span data-stu-id="568fa-219">When your cluster sits behind a load balancer, as it does in Azure, you must ensure that your web services are deployed on every machine since the load balancer simply round-robins traffic across the machines.</span></span> <span data-ttu-id="568fa-220">Ehhez úgy, hogy a `InstanceCount` a szolgáltatás számára a speciális érték-1".</span><span class="sxs-lookup"><span data-stu-id="568fa-220">You can do this by setting the `InstanceCount` for the service to the special value of "-1".</span></span>

<span data-ttu-id="568fa-221">Ezzel szemben ha Ön egy webszolgáltatás futtatását helyileg, gondoskodnia kell arról, hogy csak egy példánya fut. a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="568fa-221">By contrast, when you run a web service locally, you need to ensure that only one instance of the service is running.</span></span> <span data-ttu-id="568fa-222">Ellenkező esetben tapasztal ütközések a több elérési út és a port által figyelt folyamat.</span><span class="sxs-lookup"><span data-stu-id="568fa-222">Otherwise, you run into conflicts from multiple processes that are listening on the same path and port.</span></span> <span data-ttu-id="568fa-223">Ennek eredményeképpen a webes szolgáltatás példányok száma kell megadni a helyi központi telepítések elvégzéséhez "1".</span><span class="sxs-lookup"><span data-stu-id="568fa-223">As a result, the web service instance count should be set to "1" for local deployments.</span></span>

<span data-ttu-id="568fa-224">Különböző környezet különböző értékeket állíthat be, lásd: [alkalmazás paramétereinek több környezet kezelése](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="568fa-224">To learn how to configure different values for different environment, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="568fa-225">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="568fa-225">Next steps</span></span>
<span data-ttu-id="568fa-226">Most, hogy a webalkalmazás első set végül az ASP.NET Core az alkalmazáshoz, további információ [ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) az ASP.NET Core működése a Service Fabric alapos ismeretét.</span><span class="sxs-lookup"><span data-stu-id="568fa-226">Now that you have a web front end set up for your application with ASP.NET Core, learn more about [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) for an in-depth understanding of how ASP.NET Core works with Service Fabric.</span></span>

<span data-ttu-id="568fa-227">Ezt követően [tudhat meg többet a szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md) általában lekérni a teljes kép hogyan szolgáltatás a kommunikációt a Service Fabric működik.</span><span class="sxs-lookup"><span data-stu-id="568fa-227">Next, [learn more about communicating with services](service-fabric-connect-and-communicate-with-services.md) in general to get a complete picture of how service communication works in Service Fabric.</span></span>

<span data-ttu-id="568fa-228">Ha elvégezte a szolgáltatások közötti kommunikáció működésével, alaposan megismernie [fürt létrehozása az Azure-ban, és a felhőben az alkalmazás telepítése](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="568fa-228">Once you have a good understanding of how service communication works, [create a cluster in Azure and deploy your application to the cloud](service-fabric-cluster-creation-via-portal.md).</span></span>

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
