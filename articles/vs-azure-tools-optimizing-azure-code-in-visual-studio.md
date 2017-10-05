---
title: "A Visual Studio Azure kód optimalizálása |} Microsoft Docs"
description: "Ismerje meg, az Azure kód optimalizálási eszközök Visual Studio érdekében a kód megbízhatóbb és jobban végrehajtása."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 8f145502a856798d6e69ac11f324c72fa23f938e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="optimizing-your-azure-code"></a><span data-ttu-id="bc303-103">Az Azure kód optimalizálása</span><span class="sxs-lookup"><span data-stu-id="bc303-103">Optimizing Your Azure Code</span></span>
<span data-ttu-id="bc303-104">Ha a Microsoft Azure használó alkalmazások által programozási, van néhány alkalmazás méretezhetőséget, viselkedését, egy felhőalapú környezetben a teljesítményt és a problémák elkerülése érdekében ajánlott eljárásit.</span><span class="sxs-lookup"><span data-stu-id="bc303-104">When you’re programming apps that use Microsoft Azure, there are some coding practices you should follow to help avoid problems with app scalability, behavior and performance in a cloud environment.</span></span> <span data-ttu-id="bc303-105">A Microsoft biztosít egy Azure kód elemző eszköz, amely ismeri fel és azonosítja a leggyakrabban észlelt problémák számos, és segítséget nyújt a megoldásukkal együtt.</span><span class="sxs-lookup"><span data-stu-id="bc303-105">Microsoft provides an Azure Code Analysis tool that recognizes and identifies several of these commonly-encountered issues and helps you resolve them.</span></span> <span data-ttu-id="bc303-106">Az eszköz a Visual Studio NuGet útján lehet letölteni.</span><span class="sxs-lookup"><span data-stu-id="bc303-106">You can download the tool in Visual Studio via NuGet.</span></span>

## <a name="azure-code-analysis-rules"></a><span data-ttu-id="bc303-107">Az Azure Analysis kód szabályok</span><span class="sxs-lookup"><span data-stu-id="bc303-107">Azure Code Analysis rules</span></span>
<span data-ttu-id="bc303-108">Az Azure kód elemző eszköz a következő szabályok segítségével automatikusan jelzőt az Azure kódot, ha talál teljesítményt érintő ismert problémákat.</span><span class="sxs-lookup"><span data-stu-id="bc303-108">The Azure Code Analysis tool uses the following rules to automatically flag your Azure code when it finds known performance-impacting issues.</span></span> <span data-ttu-id="bc303-109">Észlelt problémák jelenhetnek meg a figyelmeztetéseket vagy fordítási hibákat.</span><span class="sxs-lookup"><span data-stu-id="bc303-109">Detected issues appear as a warnings or compiler errors.</span></span> <span data-ttu-id="bc303-110">A villanykörte ikonnal gyakran kipróbálni a kód javítások és a javaslatok a figyelmeztetés vagy a hiba megoldásához.</span><span class="sxs-lookup"><span data-stu-id="bc303-110">Code fixes or suggestions to resolve the warning or error are often provided through a light bulb icon.</span></span>

## <a name="avoid-using-default-in-process-session-state-mode"></a><span data-ttu-id="bc303-111">Ne használja az alapértelmezett (a folyamat) munkamenet-állapot módját</span><span class="sxs-lookup"><span data-stu-id="bc303-111">Avoid using default (in-process) session state mode</span></span>
### <a name="id"></a><span data-ttu-id="bc303-112">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-112">ID</span></span>
<span data-ttu-id="bc303-113">AP0000</span><span class="sxs-lookup"><span data-stu-id="bc303-113">AP0000</span></span>

### <a name="description"></a><span data-ttu-id="bc303-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-114">Description</span></span>
<span data-ttu-id="bc303-115">Ha használja az alapértelmezett (a folyamat) munkamenet-állapot módját a felhőalapú alkalmazásokhoz, a munkamenet-állapot elveszhet.</span><span class="sxs-lookup"><span data-stu-id="bc303-115">If you use the default (in-process) session state mode for cloud applications, you can lose session state.</span></span>

<span data-ttu-id="bc303-116">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-116">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-117">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-117">Reason</span></span>
<span data-ttu-id="bc303-118">Alapértelmezés szerint a web.config fájlban megadott munkamenet-állapot módját a folyamatban.</span><span class="sxs-lookup"><span data-stu-id="bc303-118">By default, the session state mode specified in the web.config file is in-process.</span></span> <span data-ttu-id="bc303-119">Is ha a konfigurációs fájlban megadott bejegyzés, a munkamenet-állapot módját a alapértelmezés szerint az a folyamat.</span><span class="sxs-lookup"><span data-stu-id="bc303-119">Also, if no entry specified in the configuration file, the Session State mode defaults to in-process.</span></span> <span data-ttu-id="bc303-120">A folyamaton belüli módot a munkamenet-állapot az a memóriában tárolja a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="bc303-120">The in-process mode stores session state in memory on the web server.</span></span> <span data-ttu-id="bc303-121">Amikor egy újraindítása, vagy egy új példányt a terheléselosztás és feladatátvétel támogatásáról szolgál, a webkiszolgáló a memóriában tárolt munkamenet-állapot nem menti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="bc303-121">When an instance is restarted or a new instance is used for load balancing or failover support, the session state stored in memory on the web server isn’t saved.</span></span> <span data-ttu-id="bc303-122">Ez a helyzet megakadályozza, hogy az alkalmazás nem méretezhető a felhő.</span><span class="sxs-lookup"><span data-stu-id="bc303-122">This situation prevents the application from being scalable on the cloud.</span></span>

<span data-ttu-id="bc303-123">Az ASP.NET munkamenet-állapot munkamenet-állapot adatainak támogatja a több, eltérő tárolási lehetőség: InProc, StateServer, SQL Server, egyéni, és ki.</span><span class="sxs-lookup"><span data-stu-id="bc303-123">ASP.NET session state supports several different storage options for session state data: InProc, StateServer, SQLServer, Custom, and Off.</span></span> <span data-ttu-id="bc303-124">Javasoljuk, hogy egyéni módot használja adatok tárolására a munkamenet-állapot külső áruházban, például a [Azure munkamenetállapot-szolgáltató az Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span><span class="sxs-lookup"><span data-stu-id="bc303-124">It’s recommended that you use Custom mode to host data on an external Session State store, such as [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-125">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-125">Solution</span></span>
<span data-ttu-id="bc303-126">Egyetlen ajánlott megoldás, hogy munkamenet-állapot tárolása egy felügyelt gyorsítótár szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="bc303-126">One recommended solution is to store session state on a managed cache service.</span></span> <span data-ttu-id="bc303-127">Ismerje meg, hogyan használható [Azure munkamenetállapot-szolgáltató az Redis](http://go.microsoft.com/fwlink/?LinkId=401521) a munkamenet-állapot tárolásához.</span><span class="sxs-lookup"><span data-stu-id="bc303-127">Learn how to use [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521) to store your session state.</span></span> <span data-ttu-id="bc303-128">A munkamenet-állapot, így az alkalmazás méretezhető a felhő más helyen is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="bc303-128">You can also store session state in other places to ensure your application is scalable on the cloud.</span></span> <span data-ttu-id="bc303-129">További információt olvassa el az alternatív megoldások [munkamenet-állapot módok](https://msdn.microsoft.com/library/ms178586).</span><span class="sxs-lookup"><span data-stu-id="bc303-129">To learn more about alternative solutions please read [Session State Modes](https://msdn.microsoft.com/library/ms178586).</span></span>

## <a name="run-method-should-not-be-async"></a><span data-ttu-id="bc303-130">Futtatási mód nem lehet aszinkron</span><span class="sxs-lookup"><span data-stu-id="bc303-130">Run method should not be async</span></span>
### <a name="id"></a><span data-ttu-id="bc303-131">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-131">ID</span></span>
<span data-ttu-id="bc303-132">AP1000</span><span class="sxs-lookup"><span data-stu-id="bc303-132">AP1000</span></span>

### <a name="description"></a><span data-ttu-id="bc303-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-133">Description</span></span>
<span data-ttu-id="bc303-134">Aszinkron metódusok létrehozása (például [await](https://msdn.microsoft.com/library/hh156528.aspx)) kívül a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer, majd hívja a az aszinkron metódusoknak [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-134">Create asynchronous methods (such as [await](https://msdn.microsoft.com/library/hh156528.aspx)) outside of the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and then call the async methods from [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span></span> <span data-ttu-id="bc303-135">Deklaráló a [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus aszinkron módon okoz a feldolgozói szerepkör újraindítás hurok megadását.</span><span class="sxs-lookup"><span data-stu-id="bc303-135">Declaring the [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method as async causes the worker role to enter a restart loop.</span></span>

<span data-ttu-id="bc303-136">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-136">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-137">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-137">Reason</span></span>
<span data-ttu-id="bc303-138">Aszinkron metódusok belüli meghívása a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus okoz a felhőalapú szolgáltatás futásideje újrahasznosítása a feldolgozói szerepkör.</span><span class="sxs-lookup"><span data-stu-id="bc303-138">Calling async methods inside the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes the cloud service runtime to recycle the worker role.</span></span> <span data-ttu-id="bc303-139">A feldolgozói szerepkör indulásakor az összes program végrehajtását belül kerül sor a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="bc303-139">When a worker role starts, all program execution takes place inside the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="bc303-140">Kilépés a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus okoz a feldolgozói szerepkör újraindítására.</span><span class="sxs-lookup"><span data-stu-id="bc303-140">Exiting the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes the worker role to restart.</span></span> <span data-ttu-id="bc303-141">A feldolgozói szerepkör futásidejű találatok az aszinkron metódussal, ha minden műveletnél kiszállítja az aszinkron metódus után, és adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bc303-141">When the worker role runtime hits the async method, it dispatches all operations after the async method and then returns.</span></span> <span data-ttu-id="bc303-142">Ennek hatására a Kilépés a feldolgozói szerepkör a [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust, és indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="bc303-142">This causes the worker role to exit from the [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and restart.</span></span> <span data-ttu-id="bc303-143">A következő munkamenetben végrehajtás a feldolgozói szerepkör az aszinkron metódusban találatok újra, és újraindul, a feldolgozói szerepkör újrahasznosítása újra is, amely.</span><span class="sxs-lookup"><span data-stu-id="bc303-143">In the next iteration of execution, the worker role hits the async method again and restarts, causing the worker role to recycle again as well.</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-144">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-144">Solution</span></span>
<span data-ttu-id="bc303-145">Helyezze el az összes aszinkron művelet kívül a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="bc303-145">Place all async operations outside of the [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="bc303-146">Majd, meghívják a a átkerült aszinkron metódus belül a [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer, például RunAsync () .wait.</span><span class="sxs-lookup"><span data-stu-id="bc303-146">Then, call the refactored async method from inside the [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method, such as RunAsync().wait.</span></span> <span data-ttu-id="bc303-147">A Azure kód elemző eszköz segít a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="bc303-147">The Azure Code Analysis tool can help you fix this issue.</span></span>

<span data-ttu-id="bc303-148">A következő kódrészletet mutatja be a kódját a hiba javítása:</span><span class="sxs-lookup"><span data-stu-id="bc303-148">The following code snippet demonstrates the code fix for this issue:</span></span>

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a><span data-ttu-id="bc303-149">Service Bus közös hozzáférésű Jogosultságkód-hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="bc303-149">Use Service Bus Shared Access Signature authentication</span></span>
### <a name="id"></a><span data-ttu-id="bc303-150">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-150">ID</span></span>
<span data-ttu-id="bc303-151">AP2000</span><span class="sxs-lookup"><span data-stu-id="bc303-151">AP2000</span></span>

### <a name="description"></a><span data-ttu-id="bc303-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-152">Description</span></span>
<span data-ttu-id="bc303-153">Hitelesítéshez használandó közös hozzáférésű Jogosultságkód (SAS).</span><span class="sxs-lookup"><span data-stu-id="bc303-153">Use Shared Access Signature (SAS) for authentication.</span></span> <span data-ttu-id="bc303-154">A service bus hitelesítéshez Access Control Service (ACS) is elavult.</span><span class="sxs-lookup"><span data-stu-id="bc303-154">Access Control Service (ACS) is being deprecated for service bus authentication.</span></span>

<span data-ttu-id="bc303-155">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-155">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-156">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-156">Reason</span></span>
<span data-ttu-id="bc303-157">A fokozott biztonság érdekében Azure Active Directory SAS hitelesítési ACS hitelesítési lecseréli.</span><span class="sxs-lookup"><span data-stu-id="bc303-157">For enhanced security, Azure Active Directory is replacing ACS authentication with SAS authentication.</span></span> <span data-ttu-id="bc303-158">Lásd: [Azure Active Directory az ACS a jövőben](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) az átmenet terv olvashat.</span><span class="sxs-lookup"><span data-stu-id="bc303-158">See [Azure Active Directory is the future of ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) for information on the transition plan.</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-159">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-159">Solution</span></span>
<span data-ttu-id="bc303-160">SAS-hitelesítés használata az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="bc303-160">Use SAS authentication in your apps.</span></span> <span data-ttu-id="bc303-161">A következő példa bemutatja, hogyan egy meglévő SAS-jogkivonat egy service bus-névtér vagy entitás elérésére használhat.</span><span class="sxs-lookup"><span data-stu-id="bc303-161">The following example shows how to use an existing SAS token to access a service bus namespace or entity.</span></span>

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

<span data-ttu-id="bc303-162">A következő témakörökben további információt.</span><span class="sxs-lookup"><span data-stu-id="bc303-162">See the following topics for more information.</span></span>

* <span data-ttu-id="bc303-163">Megtudhatja, [megosztott hozzáférési aláírást hitelesítést a Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span><span class="sxs-lookup"><span data-stu-id="bc303-163">For an overview, see [Shared Access Signature Authentication with Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span></span>
* [<span data-ttu-id="bc303-164">A Service Bus megosztott hozzáférési aláírást hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="bc303-164">How to use Shared Access Signature Authentication with Service Bus</span></span>](https://msdn.microsoft.com/library/dn205161.aspx)
* <span data-ttu-id="bc303-165">Egy minta-projekt lásd [használatával közös hozzáférésű Jogosultságkód (SAS) hitelesítés a Service Bus-előfizetések](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span><span class="sxs-lookup"><span data-stu-id="bc303-165">For a sample project, see [Using Shared Access Signature (SAS) authentication with Service Bus Subscriptions](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span></span>

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a><span data-ttu-id="bc303-166">Érdemes lehet OnMessage metódus "az üzenetfogadási hurok" elkerülése érdekében</span><span class="sxs-lookup"><span data-stu-id="bc303-166">Consider using OnMessage method to avoid "receive loop"</span></span>
### <a name="id"></a><span data-ttu-id="bc303-167">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-167">ID</span></span>
<span data-ttu-id="bc303-168">AP2002</span><span class="sxs-lookup"><span data-stu-id="bc303-168">AP2002</span></span>

### <a name="description"></a><span data-ttu-id="bc303-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-169">Description</span></span>
<span data-ttu-id="bc303-170">Egy "az üzenetfogadási hurok," üzembe elkerülése érdekében hívja a **OnMessage** metódus hívása mint üzenetek fogadása egy jobb megoldás a **Receive** metódust.</span><span class="sxs-lookup"><span data-stu-id="bc303-170">To avoid going into a "receive loop," calling the **OnMessage** method is a better solution for receiving messages than calling the **Receive** method.</span></span> <span data-ttu-id="bc303-171">Azonban ha kell használnia a **Receive** metódust, és adjon meg egy nem alapértelmezett server várakozási idő, győződjön meg arról, hogy a kiszolgáló várakozási idő több mint egy percig.</span><span class="sxs-lookup"><span data-stu-id="bc303-171">However, if you must use the **Receive** method, and you specify a non-default server wait time, make sure the server wait time is more than one minute.</span></span>

<span data-ttu-id="bc303-172">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-172">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-173">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-173">Reason</span></span>
<span data-ttu-id="bc303-174">Meghívásakor **OnMessage**, az ügyfél elindul egy belső üzenet szivattyú, amely folyamatosan kérdezze le a várólista vagy az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="bc303-174">When calling **OnMessage**, the client starts an internal message pump that constantly polls the queue or subscription.</span></span> <span data-ttu-id="bc303-175">Az üzenet szivattyú által kiállított üzeneteket fogadni hívás végtelen hurkot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bc303-175">This message pump contains an infinite loop that issues a call to receive messages.</span></span> <span data-ttu-id="bc303-176">A hívás túllépi az időkorlátot, ha egy új hívás ad ki.</span><span class="sxs-lookup"><span data-stu-id="bc303-176">If the call times out, it issues a new call.</span></span> <span data-ttu-id="bc303-177">Az időkorlát értéke határozza meg a [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) tulajdonsága a [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)használt.</span><span class="sxs-lookup"><span data-stu-id="bc303-177">The timeout interval is determined by the value of the [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) property of the [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)that’s being used.</span></span>

<span data-ttu-id="bc303-178">Használatának előnye **OnMessage** képest **Receive** , hogy a felhasználóknak nem kell manuálisan kérdezze le az üzenetek, kivételek kezelése, párhuzamosan több üzenetek feldolgozásához, és fejezze be az üzenetek.</span><span class="sxs-lookup"><span data-stu-id="bc303-178">The advantage of using **OnMessage** compared to **Receive** is that users don’t have to manually poll for messages, handle exceptions, process multiple messages in parallel, and complete the messages.</span></span>

<span data-ttu-id="bc303-179">Ha meghívja a **Receive** anélkül, hogy az alapértelmezett értéket használja, ügyeljen a *ServerWaitTime* értéke nagyobb, mint egy perc.</span><span class="sxs-lookup"><span data-stu-id="bc303-179">If you call **Receive** without using its default value, be sure the *ServerWaitTime* value is more than one minute.</span></span> <span data-ttu-id="bc303-180">Beállítás *ServerWaitTime* több mint egy perc alatt megakadályozza, hogy a kiszolgáló időtúllépés miatt az üzenet teljesen megérkezése előtt.</span><span class="sxs-lookup"><span data-stu-id="bc303-180">Setting *ServerWaitTime* to more than one minute prevents the server from timing out before the message is fully received.</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-181">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-181">Solution</span></span>
<span data-ttu-id="bc303-182">Ellenőrizze a következő példák az ajánlott módjait.</span><span class="sxs-lookup"><span data-stu-id="bc303-182">Please see the following code examples for recommended usages.</span></span> <span data-ttu-id="bc303-183">További részletekért lásd: [QueueClient.OnMessage metódus (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)és [QueueClient.Receive metódus (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-183">For more details, see [QueueClient.OnMessage Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)and [QueueClient.Receive Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span></span>

<span data-ttu-id="bc303-184">Az Azure üzenetküldési infrastruktúra a teljesítmény javítása érdekében tekintse meg a kialakítási mintában [aszinkron üzenetkezelési ismertetése](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-184">To improve the performance of the Azure messaging infrastructure, see the design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

<span data-ttu-id="bc303-185">Az alábbiakban egy példa annak **OnMessage** üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="bc303-185">The following is an example of using **OnMessage** to receive messages.</span></span>

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

<span data-ttu-id="bc303-186">Az alábbiakban egy példa annak **Receive** az alapértelmezett kiszolgáló a várakozási idő.</span><span class="sxs-lookup"><span data-stu-id="bc303-186">The following is an example of using **Receive** with the default server wait time.</span></span>

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

<span data-ttu-id="bc303-187">Az alábbiakban egy példa annak **Receive** egy nem alapértelmezett kiszolgálóval várakozási idő.</span><span class="sxs-lookup"><span data-stu-id="bc303-187">The following is an example of using **Receive** with a non-default server wait time.</span></span>

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a><span data-ttu-id="bc303-188">Érdemes lehet aszinkron Service Bus-metódusok</span><span class="sxs-lookup"><span data-stu-id="bc303-188">Consider using asynchronous Service Bus methods</span></span>
### <a name="id"></a><span data-ttu-id="bc303-189">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-189">ID</span></span>
<span data-ttu-id="bc303-190">AP2003</span><span class="sxs-lookup"><span data-stu-id="bc303-190">AP2003</span></span>

### <a name="description"></a><span data-ttu-id="bc303-191">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-191">Description</span></span>
<span data-ttu-id="bc303-192">Módszerekkel aszinkron Service Bus közvetítőalapú üzenettovábbítás a teljesítmény javításához.</span><span class="sxs-lookup"><span data-stu-id="bc303-192">Use asynchronous Service Bus methods to improve performance with brokered messaging.</span></span>

<span data-ttu-id="bc303-193">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-193">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-194">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-194">Reason</span></span>
<span data-ttu-id="bc303-195">Aszinkron metódusok használatával lehetővé teszi a program egyidejű application, mert minden hívás végrehajtása ne blokkolja a fő szálnak.</span><span class="sxs-lookup"><span data-stu-id="bc303-195">Using asynchronous methods enables application program concurrency because executing each call doesn’t block the main thread.</span></span> <span data-ttu-id="bc303-196">A Service Bus üzenetkezelés módszerek, egy olyan műveletet hajt használatakor (Küldés, kapni, törlés, stb.) időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="bc303-196">When using Service Bus messaging methods, performing an operation (send, receive, delete, etc.) takes time.</span></span> <span data-ttu-id="bc303-197">Most a Service Bus szolgáltatás mellett a kérelem és a válasz késleltetése a művelet feldolgozása tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bc303-197">This time includes the processing of the operation by the Service Bus service in addition to the latency of the request and the reply.</span></span> <span data-ttu-id="bc303-198">Az idő alatt műveletek számának növeléséhez, műveletek végre kell hajtani egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="bc303-198">To increase the number of operations per time, operations must execute concurrently.</span></span> <span data-ttu-id="bc303-199">További információ a tekintse meg [gyakorlati tanácsok a teljesítmény javítását használatával Service Bus Közvetítőalapú üzenetkezelés](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-199">For more information please refer to [Best Practices for Performance Improvements Using Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-200">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-200">Solution</span></span>
<span data-ttu-id="bc303-201">Lásd: [QueueClient osztály (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) a javasolt aszinkron metódus használatával kapcsolatos információt.</span><span class="sxs-lookup"><span data-stu-id="bc303-201">See [QueueClient Class (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) for information about how to use the recommended asynchronous method.</span></span>

<span data-ttu-id="bc303-202">Az Azure üzenetküldési infrastruktúra a teljesítmény javítása érdekében tekintse meg a kialakítási mintában [aszinkron üzenetkezelési ismertetése](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-202">To improve the performance of the Azure messaging infrastructure, see the design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

## <a name="consider-partitioning-service-bus-queues-and-topics"></a><span data-ttu-id="bc303-203">Vegye figyelembe a particionálási Service Bus-üzenetsorok és témakörök</span><span class="sxs-lookup"><span data-stu-id="bc303-203">Consider partitioning Service Bus queues and topics</span></span>
### <a name="id"></a><span data-ttu-id="bc303-204">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-204">ID</span></span>
<span data-ttu-id="bc303-205">AP2004</span><span class="sxs-lookup"><span data-stu-id="bc303-205">AP2004</span></span>

### <a name="description"></a><span data-ttu-id="bc303-206">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-206">Description</span></span>
<span data-ttu-id="bc303-207">Partíció Service Bus-üzenetsorok és témakörök a jobb teljesítmény érdekében a Service Bus üzenetkezelés.</span><span class="sxs-lookup"><span data-stu-id="bc303-207">Partition Service Bus queues and topics for better performance with Service Bus messaging.</span></span>

<span data-ttu-id="bc303-208">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-208">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-209">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-209">Reason</span></span>
<span data-ttu-id="bc303-210">Service Bus-üzenetsorok és témakörök particionálás növeli a teljesítményt átviteli sebesség és a szolgáltatás rendelkezésre állási, mivel a már nem korlátozzák a teljes átviteli képessége – a particionált üzenetsor vagy témakör egyetlen üzenet broker vagy üzenetküldési tárolóban teljesítmény szerint.</span><span class="sxs-lookup"><span data-stu-id="bc303-210">Partitioning Service Bus queues and topics increases performance throughput and service availability because the overall throughput of a partitioned queue or topic is no longer limited by the performance of a single message broker or messaging store.</span></span> <span data-ttu-id="bc303-211">Ezenkívül átmenetileg nem működik az üzenetküldési tárolóban nem elérhetetlenné particionált üzenetsor vagy témakör.</span><span class="sxs-lookup"><span data-stu-id="bc303-211">In addition, a temporary outage of a messaging store doesn’t make a partitioned queue or topic unavailable.</span></span> <span data-ttu-id="bc303-212">További információkért lásd: [üzenetküldési entitások particionálás](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-212">For more information, see [Partitioning Messaging Entities](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-213">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-213">Solution</span></span>
<span data-ttu-id="bc303-214">A következő kódrészletet üzenetküldési entitások particionálásáról jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bc303-214">The following code snippet shows how to partition messaging entities.</span></span>

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

<span data-ttu-id="bc303-215">További információkért lásd: [particionálva Service Bus-üzenetsorok és témakörök |} A Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) és tekintse meg a [Microsoft Azure Service Bus particionálva várólista](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) minta.</span><span class="sxs-lookup"><span data-stu-id="bc303-215">For more information, see [Partitioned Service Bus Queues and Topics | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) and check out the [Microsoft Azure Service Bus Partitioned Queue](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) sample.</span></span>

## <a name="do-not-set-sharedaccessstarttime"></a><span data-ttu-id="bc303-216">Ne adja meg az SharedAccessStartTime</span><span class="sxs-lookup"><span data-stu-id="bc303-216">Do not set SharedAccessStartTime</span></span>
### <a name="id"></a><span data-ttu-id="bc303-217">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-217">ID</span></span>
<span data-ttu-id="bc303-218">AP3001</span><span class="sxs-lookup"><span data-stu-id="bc303-218">AP3001</span></span>

### <a name="description"></a><span data-ttu-id="bc303-219">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-219">Description</span></span>
<span data-ttu-id="bc303-220">Az aktuális idő azonnal el tudja indítani a megosztott hozzáférési házirend SharedAccessStartTimeset segítségével kerülendő.</span><span class="sxs-lookup"><span data-stu-id="bc303-220">You should avoid using SharedAccessStartTimeset to the current time to immediately start the Shared Access policy.</span></span> <span data-ttu-id="bc303-221">Csak szeretné állítani ezt a tulajdonságot, ha el szeretné indítani a megosztott hozzáférési házirend egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="bc303-221">You only need to set this property if you want to start the Shared Access policy at a later time.</span></span>

<span data-ttu-id="bc303-222">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-222">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-223">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-223">Reason</span></span>
<span data-ttu-id="bc303-224">Számítógépóra-szinkronizálás adatközpontok között enyhe időeltérése okoz.</span><span class="sxs-lookup"><span data-stu-id="bc303-224">Clock synchronization causes a slight time difference among datacenters.</span></span> <span data-ttu-id="bc303-225">Például akkor logikailag gondol a kezdési időt a tároló SAS-házirend beállítását az aktuális idő DateTime.Now használatával, vagy hasonló módon, akkor azonnal érvénybe lépnek a SAS-házirendet.</span><span class="sxs-lookup"><span data-stu-id="bc303-225">For example, you would logically think setting the start time of a storage SAS policy as the current time by using DateTime.Now or a similar method will cause the SAS policy to take effect immediately.</span></span> <span data-ttu-id="bc303-226">Azonban az Adatközpont között enyhe időeltérést problémákat okozhat a mivel lehet, hogy néhány datacenter eset némileg későbbi a kezdési időpontnál, míg mások azt előre.</span><span class="sxs-lookup"><span data-stu-id="bc303-226">However, the slight time differences between datacenters can cause problems with this since some datacenter times might be slightly later than the start time, while others ahead of it.</span></span> <span data-ttu-id="bc303-227">Ennek eredményeképpen a SAS-házirend lejárhat gyorsan (vagy akár azonnal) Ha a házirend-élettartam értéke túl rövid.</span><span class="sxs-lookup"><span data-stu-id="bc303-227">As a result, the SAS policy can expire quickly (or even immediately) if the policy lifetime is set too short.</span></span>

<span data-ttu-id="bc303-228">Az Azure storage közös hozzáférésű Jogosultságkód használatával további útmutatást lásd: [bevezetéséről tábla SAS (közös hozzáférésű Jogosultságkód), üzenetsor SAS és a frissítést a Blob SAS - Microsoft Azure tárolás fejlesztői Blog - hely kezdőlap - MSDN-blogok](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-228">For more guidance on using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update to Blob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-229">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-229">Solution</span></span>
<span data-ttu-id="bc303-230">Távolítsa el az utasítást, amely beállítja a megosztott elérési házirend kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="bc303-230">Remove the statement that sets the start time of the shared access policy.</span></span> <span data-ttu-id="bc303-231">Az Azure kód elemző eszköz biztosít egy javítást a probléma.</span><span class="sxs-lookup"><span data-stu-id="bc303-231">The Azure Code Analysis tool provides a fix for this issue.</span></span> <span data-ttu-id="bc303-232">A biztonsági felügyelet további információkért lásd: a kialakítási mintában [Valet kulcs mintát](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-232">For more information on security management, please see the design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="bc303-233">A következő kódrészletet mutatja be a kódját a hiba javítása.</span><span class="sxs-lookup"><span data-stu-id="bc303-233">The following code snippet demonstrates the code fix for this issue.</span></span>

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a><span data-ttu-id="bc303-234">Megosztott hozzáférési házirend lejárati idő több mint öt perc kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bc303-234">Shared Access Policy expiry time must be more than five minutes</span></span>
### <a name="id"></a><span data-ttu-id="bc303-235">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-235">ID</span></span>
<span data-ttu-id="bc303-236">AP3002</span><span class="sxs-lookup"><span data-stu-id="bc303-236">AP3002</span></span>

### <a name="description"></a><span data-ttu-id="bc303-237">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-237">Description</span></span>
<span data-ttu-id="bc303-238">Lehet, mint amennyit az órák miatt egy "óra eltérésére." néven ismert különböző helyeken adatközpontok között eltérés öt perc</span><span class="sxs-lookup"><span data-stu-id="bc303-238">There can be as much as a five minute difference in clocks among datacenters at different locations due to a condition known as "clock skew."</span></span> <span data-ttu-id="bc303-239">A biztonsági Társítások megelőzése érdekében házirend jogkivonat lejárjanak korábbi, mint a tervezett és a lejárati idő több mint öt perc állítsa be.</span><span class="sxs-lookup"><span data-stu-id="bc303-239">To prevent the SAS policy token from expiring earlier than planned, set the expiry time to be more than five minutes.</span></span>

<span data-ttu-id="bc303-240">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-240">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-241">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-241">Reason</span></span>
<span data-ttu-id="bc303-242">A világ különböző helyeken adatközpontok egy órajel szinkronizálni.</span><span class="sxs-lookup"><span data-stu-id="bc303-242">Datacenters at different locations around the world synchronize by a clock signal.</span></span> <span data-ttu-id="bc303-243">A különböző helyekre továbbítani órajel időt vesz igénybe, mert lehet a különböző földrajzi helyen adatközpontokat közötti idő eltérés Bár minden támaszkodnak szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="bc303-243">Because it takes time for clock signal to travel to different locations, there can be a time variance between datacenters at different geographical locations although everything is supposedly synchronized.</span></span> <span data-ttu-id="bc303-244">Ez eltérést hatással lehet a megosztott hozzáférési házirend kezdési időt és a lejárati időközt.</span><span class="sxs-lookup"><span data-stu-id="bc303-244">This time difference can affect the Shared Access policy start time and expiration interval.</span></span> <span data-ttu-id="bc303-245">Ezért győződjön meg arról, megosztott hozzáférési házirend azonnal érvénybe lép, nem adja meg a kezdési időpontot.</span><span class="sxs-lookup"><span data-stu-id="bc303-245">Therefore, to ensure Shared Access policy takes effect immediately, don’t specify the start time.</span></span> <span data-ttu-id="bc303-246">Emellett ellenőrizze, hogy a lejárati idő több mint 5 perc korai időtúllépés megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="bc303-246">In addition, make sure the expiration time is more than 5 minutes to prevent early timeout.</span></span>

<span data-ttu-id="bc303-247">Az Azure storage közös hozzáférésű Jogosultságkód használatával kapcsolatos további információkért lásd: [bevezetéséről tábla SAS (közös hozzáférésű Jogosultságkód), üzenetsor SAS és a frissítést a Blob SAS - Microsoft Azure tárolás fejlesztői Blog - hely kezdőlap - MSDN-blogok](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-247">For more information about using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update to Blob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-248">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-248">Solution</span></span>
<span data-ttu-id="bc303-249">Biztonságkezelés további információkért tekintse meg a kialakítási mintában [Valet kulcs mintát](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-249">For more information on security management, see the design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="bc303-250">Nem adja meg a megosztott hozzáférési házirend kezdő időpont példát a következő:</span><span class="sxs-lookup"><span data-stu-id="bc303-250">The following is an example of not specifying a Shared Access policy start time.</span></span>

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="bc303-251">A megosztott hozzáférési házirend kezdési idő megadása öt percnél nagyobb házirend lejárati időtartammal rendelkező példát a következő:</span><span class="sxs-lookup"><span data-stu-id="bc303-251">The following is an example of specifying a Shared Access policy start time with a policy expiration duration greater than five minutes.</span></span>

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="bc303-252">További információkért lásd: [létrehozhat és használhat egy közös hozzáférésű Jogosultságkód](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-252">For more information, see [Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="use-cloudconfigurationmanager"></a><span data-ttu-id="bc303-253">CloudConfigurationManager használata</span><span class="sxs-lookup"><span data-stu-id="bc303-253">Use CloudConfigurationManager</span></span>
### <a name="id"></a><span data-ttu-id="bc303-254">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-254">ID</span></span>
<span data-ttu-id="bc303-255">AP4000</span><span class="sxs-lookup"><span data-stu-id="bc303-255">AP4000</span></span>

### <a name="description"></a><span data-ttu-id="bc303-256">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-256">Description</span></span>
<span data-ttu-id="bc303-257">Használja a [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) projektek osztály, például az Azure Websites és az Azure mobile services nem vezetnek be futásidejű problémákat.</span><span class="sxs-lookup"><span data-stu-id="bc303-257">Using the [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) class for projects such as Azure Website and Azure mobile services won't introduce runtime issues.</span></span> <span data-ttu-id="bc303-258">Ajánlott eljárásként, azonban célszerű a felhő használandó[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) minden Azure felhőalapú alkalmazásokhoz konfigurációk kezelése egyesített módja.</span><span class="sxs-lookup"><span data-stu-id="bc303-258">As a best practice, however, it's a good idea to use Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) as a unified way of managing configurations for all Azure Cloud applications.</span></span>

<span data-ttu-id="bc303-259">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-259">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-260">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-260">Reason</span></span>
<span data-ttu-id="bc303-261">CloudConfigurationManager olvassa be a konfigurációs fájl megfelelő az alkalmazás-környezetbe.</span><span class="sxs-lookup"><span data-stu-id="bc303-261">CloudConfigurationManager reads the configuration file appropriate to the application environment.</span></span>

[<span data-ttu-id="bc303-262">CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="bc303-262">CloudConfigurationManager</span></span>](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a><span data-ttu-id="bc303-263">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-263">Solution</span></span>
<span data-ttu-id="bc303-264">A kód használatához refactor a [CloudConfigurationManager osztály](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-264">Refactor your code to use the [CloudConfigurationManager Class](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span></span> <span data-ttu-id="bc303-265">Az Azure kód elemző eszköz a kód a hiba javítása biztosítja.</span><span class="sxs-lookup"><span data-stu-id="bc303-265">A code fix for this issue is provided by the Azure Code Analysis tool.</span></span>

<span data-ttu-id="bc303-266">A következő kódrészletet mutatja be a kódját a hiba javítása.</span><span class="sxs-lookup"><span data-stu-id="bc303-266">The following code snippet demonstrates the code fix for this issue.</span></span> <span data-ttu-id="bc303-267">Csere</span><span class="sxs-lookup"><span data-stu-id="bc303-267">Replace</span></span>

`var settings = ConfigurationManager.AppSettings["mySettings"];`

<span data-ttu-id="bc303-268">a</span><span class="sxs-lookup"><span data-stu-id="bc303-268">with</span></span>

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

<span data-ttu-id="bc303-269">Íme egy példa egy App.config vagy a Web.config fájlban tárolhatja a konfigurációs beállítás.</span><span class="sxs-lookup"><span data-stu-id="bc303-269">Here's an example of how to store the configuration setting in a App.config or Web.config file.</span></span> <span data-ttu-id="bc303-270">A beállítások hozzáadása a konfigurációs fájl appSettings szakaszában.</span><span class="sxs-lookup"><span data-stu-id="bc303-270">Add the settings to the appSettings section of the configuration file.</span></span> <span data-ttu-id="bc303-271">Az előző példakódban a Web.config fájlban a következő:</span><span class="sxs-lookup"><span data-stu-id="bc303-271">The following is the Web.config file for the previous code example.</span></span>

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a><span data-ttu-id="bc303-272">Kerülje a kódolt kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="bc303-272">Avoid using hard-coded connection strings</span></span>
### <a name="id"></a><span data-ttu-id="bc303-273">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-273">ID</span></span>
<span data-ttu-id="bc303-274">AP4001</span><span class="sxs-lookup"><span data-stu-id="bc303-274">AP4001</span></span>

### <a name="description"></a><span data-ttu-id="bc303-275">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-275">Description</span></span>
<span data-ttu-id="bc303-276">Ha a kódolt kapcsolati karakterláncokat használ, és azokat később frissíteni kell, összekapcsolta módosítja a forráskódot, és fordítsa újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bc303-276">If you use hard-coded connection strings and you need to update them later, you’ll have to make changes to your source code and recompile the application.</span></span> <span data-ttu-id="bc303-277">Azonban ha egy konfigurációs fájlban tárolja a kapcsolati karakterláncokat, később bármikor módosíthatja azokat a konfigurációs fájl egyszerűen frissítésével.</span><span class="sxs-lookup"><span data-stu-id="bc303-277">However, if you store your connection strings in a configuration file, you can change them later by simply updating the configuration file.</span></span>

<span data-ttu-id="bc303-278">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-278">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-279">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-279">Reason</span></span>
<span data-ttu-id="bc303-280">Rögzített megadás kapcsolati karakterláncok a hibás célszerű, mert problémák amikor kapcsolati karakterláncok gyorsan módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="bc303-280">Hard-coding connection strings is a bad practice because it introduces problems when connection strings need to be changed quickly.</span></span> <span data-ttu-id="bc303-281">Emellett a projekt van szüksége verziókövetési rendszerrel ellenőrizni kell, ha kódolt kapcsolati karakterláncok vezethet biztonsági rések óta a karakterláncok a forráskód lehet megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="bc303-281">In addition, if the project needs to be checked in to source control, hard-coded connection strings introduce security vulnerabilities since the strings can be viewed in the source code.</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-282">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-282">Solution</span></span>
<span data-ttu-id="bc303-283">Kapcsolati karakterláncok tárolni a konfigurációs fájlok vagy az Azure környezetben.</span><span class="sxs-lookup"><span data-stu-id="bc303-283">Store connection strings in the configuration files or Azure environments.</span></span>

* <span data-ttu-id="bc303-284">Az önálló alkalmazások app.config segítségével tárolja a kapcsolódási karakterlánc beállításainak.</span><span class="sxs-lookup"><span data-stu-id="bc303-284">For standalone applications, use app.config to store connection string settings.</span></span>
* <span data-ttu-id="bc303-285">IIS kiszolgálón futó webes alkalmazásokhoz a web.config használatával kapcsolati karakterláncok tárolni.</span><span class="sxs-lookup"><span data-stu-id="bc303-285">For IIS-hosted web applications, use web.config to store connection strings.</span></span>
* <span data-ttu-id="bc303-286">Az ASP.NET-alkalmazások vNext configuration.json segítségével kapcsolati karakterláncok tárolni.</span><span class="sxs-lookup"><span data-stu-id="bc303-286">For ASP.NET vNext applications, use configuration.json to store connection strings.</span></span>

<span data-ttu-id="bc303-287">Konfigurációk fájlok – például a web.config vagy az App.config fájlt használatáról információkért lásd: [ASP.NET webes beállítási útmutatója](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span><span class="sxs-lookup"><span data-stu-id="bc303-287">For information on using configurations files such as web.config or app.config, see [ASP.NET Web Configuration Guidelines](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span></span> <span data-ttu-id="bc303-288">Az Azure környezeti változók munkahelyi információkért lásd: [webhelyek Azure: hogyan alkalmazás karakterláncok és a kapcsolati karakterláncok működik](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="bc303-288">For information on how Azure environment variables work, see [Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span> <span data-ttu-id="bc303-289">A kapcsolati karakterlánc tárolása verziókezelő információkért lásd: [ne tegye a bizalmas adatokat például kapcsolati karakterláncok forráskódraktárban tárolt fájlok](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span><span class="sxs-lookup"><span data-stu-id="bc303-289">For information on storing connection string in source control, see [avoid putting sensitive information such as connection strings in files that are stored in source code repository](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span></span>

## <a name="use-diagnostics-configuration-file"></a><span data-ttu-id="bc303-290">Diagnosztika konfigurációs fájl használata</span><span class="sxs-lookup"><span data-stu-id="bc303-290">Use diagnostics configuration file</span></span>
### <a name="id"></a><span data-ttu-id="bc303-291">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-291">ID</span></span>
<span data-ttu-id="bc303-292">AP5000</span><span class="sxs-lookup"><span data-stu-id="bc303-292">AP5000</span></span>

### <a name="description"></a><span data-ttu-id="bc303-293">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-293">Description</span></span>
<span data-ttu-id="bc303-294">Diagnosztikai beállítások konfigurálására a kódban, többek között a Microsoft.WindowsAzure.Diagnostics programozási API használatával, konfigurálnia kell az diagnosztikai beállítások a diagnostics.wadcfg fájlban.</span><span class="sxs-lookup"><span data-stu-id="bc303-294">Instead of configuring diagnostics settings in your code such as by using the Microsoft.WindowsAzure.Diagnostics programming API, you should configure diagnostics settings in the diagnostics.wadcfg file.</span></span> <span data-ttu-id="bc303-295">(Vagy, ha Azure SDK 2.5 diagnostics.wadcfgx).</span><span class="sxs-lookup"><span data-stu-id="bc303-295">(Or, diagnostics.wadcfgx if you use Azure SDK 2.5).</span></span> <span data-ttu-id="bc303-296">Ezzel az eljárással módosíthatja diagnosztikai beállítások nem kell fordítani a kódot.</span><span class="sxs-lookup"><span data-stu-id="bc303-296">By doing this, you can change diagnostics settings without having to recompile your code.</span></span>

<span data-ttu-id="bc303-297">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-297">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-298">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-298">Reason</span></span>
<span data-ttu-id="bc303-299">Mielőtt az Azure SDK 2.5-ös (amely az Azure diagnostics 1.3), Azure Diagnostics (ÜVEGVATTA) sikerült konfigurálni a számos különböző módszer használatával: hozzáadná a konfiguráció blob Storage, feltétlenül szükséges kódot, deklaratív konfigurációs vagy az alapértelmezett konfiguráció használatával.</span><span class="sxs-lookup"><span data-stu-id="bc303-299">Before Azure SDK 2.5 (which uses Azure diagnostics 1.3), Azure Diagnostics (WAD) could be configured by using several different methods: adding it to the configuration blob in storage, by using imperative code, declarative configuration, or the default configuration.</span></span> <span data-ttu-id="bc303-300">Azonban az előnyben részesített diagnosztika konfigurálása módja az XML konfigurációs fájl (diagnostics.wadcfg vagy diagnositcs.wadcfgx SDK 2.5 és újabb) az alkalmazás projektben.</span><span class="sxs-lookup"><span data-stu-id="bc303-300">However, the preferred way to configure diagnostics is to use an XML configuration file (diagnostics.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later) in the application project.</span></span> <span data-ttu-id="bc303-301">Ezt a módszert használja, a diagnostics.wadcfg fájl teljesen konfigurációját és frissíthető és újra telepíteni fogja a.</span><span class="sxs-lookup"><span data-stu-id="bc303-301">In this approach, the diagnostics.wadcfg file completely defines the configuration and can be updated and redeployed at will.</span></span> <span data-ttu-id="bc303-302">A diagnostics.wadcfg konfigurációs fájl használatának keverése kell beállítania a konfigurációk használatával programozott módon a [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)vagy [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)osztályok zavart okozhat.</span><span class="sxs-lookup"><span data-stu-id="bc303-302">Mixing the use of the diagnostics.wadcfg configuration file with the programmatic methods of setting configurations by using the [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)or [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)classes can lead to confusion.</span></span> <span data-ttu-id="bc303-303">Lásd: [inicializálása vagy Azure Diagnostics konfigurációjának módosítása](https://msdn.microsoft.com/library/azure/hh411537.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="bc303-303">See [Initialize or Change Azure Diagnostics Configuration](https://msdn.microsoft.com/library/azure/hh411537.aspx) for more information.</span></span>

<span data-ttu-id="bc303-304">(Az Azure SDK 2.5 része) ÜVEGVATTA 1.3 kezdve már nem használható kód diagnosztika konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="bc303-304">Beginning with WAD 1.3 (included with Azure SDK 2.5), it’s no longer possible to use code to configure diagnostics.</span></span> <span data-ttu-id="bc303-305">Ennek eredményeképpen csak biztosíthat a konfigurációban, ha az alkalmazása vagy a diagnosztika-bővítmény frissítése.</span><span class="sxs-lookup"><span data-stu-id="bc303-305">As a result, you can only provide the configuration when applying or updating the diagnostics extension.</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-306">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-306">Solution</span></span>
<span data-ttu-id="bc303-307">A diagnosztika configuration designer segítségével diagnosztikai beállítások áthelyezése a diagnosztika konfigurációs fájl (diagnositcs.wadcfg vagy diagnositcs.wadcfgx SDK 2.5 és újabb).</span><span class="sxs-lookup"><span data-stu-id="bc303-307">Use the diagnostics configuration designer to move diagnostic settings to the diagnostics configuration file (diagnositcs.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later).</span></span> <span data-ttu-id="bc303-308">Emellett ajánlott telepíteni [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) és diagnosztika szolgáltatás legújabb verziója.</span><span class="sxs-lookup"><span data-stu-id="bc303-308">It’s also recommended that you install [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) and use the latest diagnostics feature.</span></span>

1. <span data-ttu-id="bc303-309">A helyi menüben a konfigurálni kívánt szerepkör esetében válassza a tulajdonságok, és kattintson a konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="bc303-309">On the shortcut menu for the role that you want to configure, choose Properties, and then choose the Configuration tab.</span></span>
2. <span data-ttu-id="bc303-310">Az a **diagnosztika** területen győződjön meg arról, hogy a **engedélyezése diagnosztikai** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="bc303-310">In the **Diagnostics** section, make sure that the **Enable Diagnostics** check box is selected.</span></span>
3. <span data-ttu-id="bc303-311">Válassza ki a **konfigurálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc303-311">Choose the **Configure** button.</span></span>

   ![A diagnosztika engedélyezése beállítás elérése](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   <span data-ttu-id="bc303-313">Lásd: [diagnosztika konfigurálása az Azure Cloud Services és a virtuális gépek](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="bc303-313">See [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) for more information.</span></span>

## <a name="avoid-declaring-dbcontext-objects-as-static"></a><span data-ttu-id="bc303-314">Kerülje a DbContext objektumokat statikus deklaráló</span><span class="sxs-lookup"><span data-stu-id="bc303-314">Avoid declaring DbContext objects as static</span></span>
### <a name="id"></a><span data-ttu-id="bc303-315">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="bc303-315">ID</span></span>
<span data-ttu-id="bc303-316">AP6000</span><span class="sxs-lookup"><span data-stu-id="bc303-316">AP6000</span></span>

### <a name="description"></a><span data-ttu-id="bc303-317">Leírás</span><span class="sxs-lookup"><span data-stu-id="bc303-317">Description</span></span>
<span data-ttu-id="bc303-318">A memóriahasználat, ne a deklaráló statikus DBContext objektumokat.</span><span class="sxs-lookup"><span data-stu-id="bc303-318">To save memory, avoid declaring DBContext objects as static.</span></span>

<span data-ttu-id="bc303-319">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="bc303-319">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="bc303-320">Ok</span><span class="sxs-lookup"><span data-stu-id="bc303-320">Reason</span></span>
<span data-ttu-id="bc303-321">DBContext objektumok minden hívás a lekérdezés eredményeinek tárolásához.</span><span class="sxs-lookup"><span data-stu-id="bc303-321">DBContext objects hold the query results from each call.</span></span> <span data-ttu-id="bc303-322">Statikus DBContext objektumok nem értékesítik, amíg az alkalmazástartomány eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="bc303-322">Static DBContext objects are not disposed until the application domain is unloaded.</span></span> <span data-ttu-id="bc303-323">Ezért a statikus DBContext objektum nagy mennyiségű memóriát is felhasználhatnak.</span><span class="sxs-lookup"><span data-stu-id="bc303-323">Therefore, a static DBContext object can consume large amounts of memory.</span></span>

### <a name="solution"></a><span data-ttu-id="bc303-324">Megoldás</span><span class="sxs-lookup"><span data-stu-id="bc303-324">Solution</span></span>
<span data-ttu-id="bc303-325">DBContext deklarálható lokális változó vagy nem statikus mezőn, a feladat használni, és engedélyezze azt követően ártalmatlanítani.</span><span class="sxs-lookup"><span data-stu-id="bc303-325">Declare DBContext as a local variable or non-static instance field, use it for a task, and then let it be disposed of after use.</span></span>

<span data-ttu-id="bc303-326">Az alábbi példa MVC-vezérlő osztályhoz bemutatja, hogyan használja a DBContext objektumot.</span><span class="sxs-lookup"><span data-stu-id="bc303-326">The following example MVC controller class shows how to use the DBContext object.</span></span>

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a><span data-ttu-id="bc303-327">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc303-327">Next steps</span></span>
<span data-ttu-id="bc303-328">Optimalizálás, és tájékozódhat hibaelhárítása az Azure-alkalmazások súgójának [hibaelhárítása a webes alkalmazás az Azure App Service szolgáltatásban a Visual Studio használatával](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="bc303-328">To learn more about optimizing and troubleshooting Azure apps, see [Troubleshoot a web app in Azure App Service using Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>
