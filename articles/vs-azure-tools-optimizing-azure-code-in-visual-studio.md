---
title: az Azure a Visual Studio code aaaOptimizing |} Microsoft Docs
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
ms.openlocfilehash: 7df932def9dc16c93de29fc6a77c8fc121fda338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-your-azure-code"></a><span data-ttu-id="97eac-103">Az Azure kód optimalizálása</span><span class="sxs-lookup"><span data-stu-id="97eac-103">Optimizing Your Azure Code</span></span>
<span data-ttu-id="97eac-104">Ha a Microsoft Azure használó alkalmazások által programozási, van néhány toohelp ajánlott eljárásit app méretezhetőséget, viselkedését, egy felhőalapú környezetben a teljesítményt és a problémák elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="97eac-104">When you’re programming apps that use Microsoft Azure, there are some coding practices you should follow toohelp avoid problems with app scalability, behavior and performance in a cloud environment.</span></span> <span data-ttu-id="97eac-105">A Microsoft biztosít egy Azure kód elemző eszköz, amely ismeri fel és azonosítja a leggyakrabban észlelt problémák számos, és segítséget nyújt a megoldásukkal együtt.</span><span class="sxs-lookup"><span data-stu-id="97eac-105">Microsoft provides an Azure Code Analysis tool that recognizes and identifies several of these commonly-encountered issues and helps you resolve them.</span></span> <span data-ttu-id="97eac-106">A Visual Studio NuGet útján hello eszköz töltheti le.</span><span class="sxs-lookup"><span data-stu-id="97eac-106">You can download hello tool in Visual Studio via NuGet.</span></span>

## <a name="azure-code-analysis-rules"></a><span data-ttu-id="97eac-107">Az Azure Analysis kód szabályok</span><span class="sxs-lookup"><span data-stu-id="97eac-107">Azure Code Analysis rules</span></span>
<span data-ttu-id="97eac-108">hello Azure kód elemző eszköz tooautomatically jelzőt az Azure kódot, ha talál teljesítményt érintő ismert problémákat szabályainak hello használja.</span><span class="sxs-lookup"><span data-stu-id="97eac-108">hello Azure Code Analysis tool uses hello following rules tooautomatically flag your Azure code when it finds known performance-impacting issues.</span></span> <span data-ttu-id="97eac-109">Észlelt problémák jelenhetnek meg a figyelmeztetéseket vagy fordítási hibákat.</span><span class="sxs-lookup"><span data-stu-id="97eac-109">Detected issues appear as a warnings or compiler errors.</span></span> <span data-ttu-id="97eac-110">A villanykörte ikonnal gyakran kipróbálni a kód javítások és a javaslatok tooresolve hello figyelmeztetés vagy hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="97eac-110">Code fixes or suggestions tooresolve hello warning or error are often provided through a light bulb icon.</span></span>

## <a name="avoid-using-default-in-process-session-state-mode"></a><span data-ttu-id="97eac-111">Ne használja az alapértelmezett (a folyamat) munkamenet-állapot módját</span><span class="sxs-lookup"><span data-stu-id="97eac-111">Avoid using default (in-process) session state mode</span></span>
### <a name="id"></a><span data-ttu-id="97eac-112">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-112">ID</span></span>
<span data-ttu-id="97eac-113">AP0000</span><span class="sxs-lookup"><span data-stu-id="97eac-113">AP0000</span></span>

### <a name="description"></a><span data-ttu-id="97eac-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-114">Description</span></span>
<span data-ttu-id="97eac-115">Ha hello alapértelmezett (a folyamat) munkamenet-állapot módját használja a felhőalapú alkalmazásokhoz, a munkamenet-állapot elveszhet.</span><span class="sxs-lookup"><span data-stu-id="97eac-115">If you use hello default (in-process) session state mode for cloud applications, you can lose session state.</span></span>

<span data-ttu-id="97eac-116">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-116">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-117">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-117">Reason</span></span>
<span data-ttu-id="97eac-118">Alapértelmezés szerint hello web.config fájlban megadott hello munkamenet-állapot módját a folyamatban.</span><span class="sxs-lookup"><span data-stu-id="97eac-118">By default, hello session state mode specified in hello web.config file is in-process.</span></span> <span data-ttu-id="97eac-119">Is ha hello konfigurációs fájlban megadott bejegyzés, a munkamenet-állapot módját a hello tooin-folyamat alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="97eac-119">Also, if no entry specified in hello configuration file, hello Session State mode defaults tooin-process.</span></span> <span data-ttu-id="97eac-120">hello folyamaton belüli módot a munkamenet-állapot hello webkiszolgáló a memóriában tárolja.</span><span class="sxs-lookup"><span data-stu-id="97eac-120">hello in-process mode stores session state in memory on hello web server.</span></span> <span data-ttu-id="97eac-121">Amikor egy újraindítása, vagy egy új példányt a terheléselosztás és feladatátvétel támogatásáról szolgál, hello hello webkiszolgáló a memóriában tárolt munkamenet-állapot nem menti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="97eac-121">When an instance is restarted or a new instance is used for load balancing or failover support, hello session state stored in memory on hello web server isn’t saved.</span></span> <span data-ttu-id="97eac-122">Ez a helyzet meggátolja, hogy hello nem méretezhető hello felhő.</span><span class="sxs-lookup"><span data-stu-id="97eac-122">This situation prevents hello application from being scalable on hello cloud.</span></span>

<span data-ttu-id="97eac-123">Az ASP.NET munkamenet-állapot munkamenet-állapot adatainak támogatja a több, eltérő tárolási lehetőség: InProc, StateServer, SQL Server, egyéni, és ki.</span><span class="sxs-lookup"><span data-stu-id="97eac-123">ASP.NET session state supports several different storage options for session state data: InProc, StateServer, SQLServer, Custom, and Off.</span></span> <span data-ttu-id="97eac-124">Ajánlott a használata egyéni mód toohost adatait a munkamenet-állapot külső áruházban, például a [Azure munkamenetállapot-szolgáltató az Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span><span class="sxs-lookup"><span data-stu-id="97eac-124">It’s recommended that you use Custom mode toohost data on an external Session State store, such as [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521).</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-125">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-125">Solution</span></span>
<span data-ttu-id="97eac-126">Egy ajánlott megoldás, egy felügyelt gyorsítótár szolgáltatásra toostore munkamenet-állapot.</span><span class="sxs-lookup"><span data-stu-id="97eac-126">One recommended solution is toostore session state on a managed cache service.</span></span> <span data-ttu-id="97eac-127">Megtudhatja, hogyan toouse [Azure munkamenetállapot-szolgáltató az Redis](http://go.microsoft.com/fwlink/?LinkId=401521) toostore a munkamenet-állapot.</span><span class="sxs-lookup"><span data-stu-id="97eac-127">Learn how toouse [Azure Session State provider for Redis](http://go.microsoft.com/fwlink/?LinkId=401521) toostore your session state.</span></span> <span data-ttu-id="97eac-128">Akkor is is tároló munkamenet állapot, a más helyek tooensure az alkalmazás méretezhető hello felhő.</span><span class="sxs-lookup"><span data-stu-id="97eac-128">You can also store session state in other places tooensure your application is scalable on hello cloud.</span></span> <span data-ttu-id="97eac-129">olvassa el az alternatív megoldások további toolearn [munkamenet-állapot módok](https://msdn.microsoft.com/library/ms178586).</span><span class="sxs-lookup"><span data-stu-id="97eac-129">toolearn more about alternative solutions please read [Session State Modes](https://msdn.microsoft.com/library/ms178586).</span></span>

## <a name="run-method-should-not-be-async"></a><span data-ttu-id="97eac-130">Futtatási mód nem lehet aszinkron</span><span class="sxs-lookup"><span data-stu-id="97eac-130">Run method should not be async</span></span>
### <a name="id"></a><span data-ttu-id="97eac-131">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-131">ID</span></span>
<span data-ttu-id="97eac-132">AP1000</span><span class="sxs-lookup"><span data-stu-id="97eac-132">AP1000</span></span>

### <a name="description"></a><span data-ttu-id="97eac-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-133">Description</span></span>
<span data-ttu-id="97eac-134">Aszinkron metódusok létrehozása (például [await](https://msdn.microsoft.com/library/hh156528.aspx)) kívül hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer, majd a hívás hello aszinkron metódusok a [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-134">Create asynchronous methods (such as [await](https://msdn.microsoft.com/library/hh156528.aspx)) outside of hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and then call hello async methods from [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx).</span></span> <span data-ttu-id="97eac-135">Hello deklaráló [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus aszinkron módon hatására hello feldolgozói szerepkör tooenter újraindítás hurkot.</span><span class="sxs-lookup"><span data-stu-id="97eac-135">Declaring hello [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method as async causes hello worker role tooenter a restart loop.</span></span>

<span data-ttu-id="97eac-136">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-136">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-137">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-137">Reason</span></span>
<span data-ttu-id="97eac-138">Aszinkron metódusok hello belüli meghívása [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus okoz hello felhő futásidejű toorecycle hello feldolgozói szerepkör-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="97eac-138">Calling async methods inside hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes hello cloud service runtime toorecycle hello worker role.</span></span> <span data-ttu-id="97eac-139">A feldolgozói szerepkör indulásakor az összes program végrehajtását belül kerül sor hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="97eac-139">When a worker role starts, all program execution takes place inside hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="97eac-140">A meglévő hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus okoz hello feldolgozói szerepkör toorestart.</span><span class="sxs-lookup"><span data-stu-id="97eac-140">Exiting hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method causes hello worker role toorestart.</span></span> <span data-ttu-id="97eac-141">Hello feldolgozói szerepkör futásidejű találatok hello aszinkron metódussal, ha minden műveletnél kiszállítja hello aszinkron metódus után, és adja vissza.</span><span class="sxs-lookup"><span data-stu-id="97eac-141">When hello worker role runtime hits hello async method, it dispatches all operations after hello async method and then returns.</span></span> <span data-ttu-id="97eac-142">Ennek hatására hello feldolgozói szerepkör tooexit a hello [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust, és indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="97eac-142">This causes hello worker role tooexit from hello [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method and restart.</span></span> <span data-ttu-id="97eac-143">Hello következő munkamenetben végrehajtás a hello feldolgozói szerepkör találatok hello aszinkron metódus újra és újraindul, okozó hello feldolgozói szerepkör toorecycle újra is.</span><span class="sxs-lookup"><span data-stu-id="97eac-143">In hello next iteration of execution, hello worker role hits hello async method again and restarts, causing hello worker role toorecycle again as well.</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-144">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-144">Solution</span></span>
<span data-ttu-id="97eac-145">Helyezze el az összes aszinkron művelet kívül hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="97eac-145">Place all async operations outside of hello [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method.</span></span> <span data-ttu-id="97eac-146">Majd, meghívják a átkerült hello aszinkron metódusnak belül hello [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) módszer, például RunAsync () .wait.</span><span class="sxs-lookup"><span data-stu-id="97eac-146">Then, call hello refactored async method from inside hello [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method, such as RunAsync().wait.</span></span> <span data-ttu-id="97eac-147">hello Azure kód elemzőeszköz segíthet a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="97eac-147">hello Azure Code Analysis tool can help you fix this issue.</span></span>

<span data-ttu-id="97eac-148">a következő kódrészletet hello hello kód a hiba javítása mutatja be:</span><span class="sxs-lookup"><span data-stu-id="97eac-148">hello following code snippet demonstrates hello code fix for this issue:</span></span>

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

## <a name="use-service-bus-shared-access-signature-authentication"></a><span data-ttu-id="97eac-149">Service Bus közös hozzáférésű Jogosultságkód-hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="97eac-149">Use Service Bus Shared Access Signature authentication</span></span>
### <a name="id"></a><span data-ttu-id="97eac-150">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-150">ID</span></span>
<span data-ttu-id="97eac-151">AP2000</span><span class="sxs-lookup"><span data-stu-id="97eac-151">AP2000</span></span>

### <a name="description"></a><span data-ttu-id="97eac-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-152">Description</span></span>
<span data-ttu-id="97eac-153">Hitelesítéshez használandó közös hozzáférésű Jogosultságkód (SAS).</span><span class="sxs-lookup"><span data-stu-id="97eac-153">Use Shared Access Signature (SAS) for authentication.</span></span> <span data-ttu-id="97eac-154">A service bus hitelesítéshez Access Control Service (ACS) is elavult.</span><span class="sxs-lookup"><span data-stu-id="97eac-154">Access Control Service (ACS) is being deprecated for service bus authentication.</span></span>

<span data-ttu-id="97eac-155">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-155">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-156">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-156">Reason</span></span>
<span data-ttu-id="97eac-157">A fokozott biztonság érdekében Azure Active Directory SAS hitelesítési ACS hitelesítési lecseréli.</span><span class="sxs-lookup"><span data-stu-id="97eac-157">For enhanced security, Azure Active Directory is replacing ACS authentication with SAS authentication.</span></span> <span data-ttu-id="97eac-158">Lásd: [Azure Active Directory rendszer hello jövőbeli az ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) hello áttűnés terv olvashat.</span><span class="sxs-lookup"><span data-stu-id="97eac-158">See [Azure Active Directory is hello future of ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) for information on hello transition plan.</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-159">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-159">Solution</span></span>
<span data-ttu-id="97eac-160">SAS-hitelesítés használata az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="97eac-160">Use SAS authentication in your apps.</span></span> <span data-ttu-id="97eac-161">hello a következő példa bemutatja, hogyan toouse egy meglévő SAS-token tooaccess egy service bus névtér vagy entitás.</span><span class="sxs-lookup"><span data-stu-id="97eac-161">hello following example shows how toouse an existing SAS token tooaccess a service bus namespace or entity.</span></span>

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

<span data-ttu-id="97eac-162">Tekintse meg a következő témaköröket további tudnivalókért hello.</span><span class="sxs-lookup"><span data-stu-id="97eac-162">See hello following topics for more information.</span></span>

* <span data-ttu-id="97eac-163">Megtudhatja, [megosztott hozzáférési aláírást hitelesítést a Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span><span class="sxs-lookup"><span data-stu-id="97eac-163">For an overview, see [Shared Access Signature Authentication with Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)</span></span>
* [<span data-ttu-id="97eac-164">Hogyan toouse megosztott hozzáférési aláírása hitelesítés a Service busszal</span><span class="sxs-lookup"><span data-stu-id="97eac-164">How toouse Shared Access Signature Authentication with Service Bus</span></span>](https://msdn.microsoft.com/library/dn205161.aspx)
* <span data-ttu-id="97eac-165">Egy minta-projekt lásd [használatával közös hozzáférésű Jogosultságkód (SAS) hitelesítés a Service Bus-előfizetések](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span><span class="sxs-lookup"><span data-stu-id="97eac-165">For a sample project, see [Using Shared Access Signature (SAS) authentication with Service Bus Subscriptions](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)</span></span>

## <a name="consider-using-onmessage-method-tooavoid-receive-loop"></a><span data-ttu-id="97eac-166">Érdemes lehet OnMessage metódus tooavoid "az üzenetfogadási hurok"</span><span class="sxs-lookup"><span data-stu-id="97eac-166">Consider using OnMessage method tooavoid "receive loop"</span></span>
### <a name="id"></a><span data-ttu-id="97eac-167">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-167">ID</span></span>
<span data-ttu-id="97eac-168">AP2002</span><span class="sxs-lookup"><span data-stu-id="97eac-168">AP2002</span></span>

### <a name="description"></a><span data-ttu-id="97eac-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-169">Description</span></span>
<span data-ttu-id="97eac-170">tooavoid, amelyek egy "az üzenetfogadási hurok," hívó hello **OnMessage** metódus egy jobb megoldás, mint a hívó hello üzenetek fogadására **Receive** metódust.</span><span class="sxs-lookup"><span data-stu-id="97eac-170">tooavoid going into a "receive loop," calling hello **OnMessage** method is a better solution for receiving messages than calling hello **Receive** method.</span></span> <span data-ttu-id="97eac-171">Azonban hello használata **Receive** metódust, és egy nem alapértelmezett server várakozási idő megadni, akkor győződjön meg arról, hogy hello server várakozási idő több mint egy percig.</span><span class="sxs-lookup"><span data-stu-id="97eac-171">However, if you must use hello **Receive** method, and you specify a non-default server wait time, make sure hello server wait time is more than one minute.</span></span>

<span data-ttu-id="97eac-172">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-172">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-173">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-173">Reason</span></span>
<span data-ttu-id="97eac-174">Meghívásakor **OnMessage**, hello ügyfél elindul egy belső üzenet szivattyú, amely folyamatosan kérdezze le az hello várólista vagy az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="97eac-174">When calling **OnMessage**, hello client starts an internal message pump that constantly polls hello queue or subscription.</span></span> <span data-ttu-id="97eac-175">Az üzenet szivattyú által kiállított hívás tooreceive üzenetek végtelen hurkot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="97eac-175">This message pump contains an infinite loop that issues a call tooreceive messages.</span></span> <span data-ttu-id="97eac-176">Hello hívás túllépi az időkorlátot, ha egy új hívás ad ki.</span><span class="sxs-lookup"><span data-stu-id="97eac-176">If hello call times out, it issues a new call.</span></span> <span data-ttu-id="97eac-177">hello időkorlátja hello hello értéke határozza meg [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) hello tulajdonságának [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)használt.</span><span class="sxs-lookup"><span data-stu-id="97eac-177">hello timeout interval is determined by hello value of hello [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) property of hello [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)that’s being used.</span></span>

<span data-ttu-id="97eac-178">hello használatának előnye **OnMessage** képest túl**Receive** , hogy a felhasználóknak nem kell toomanually kérdezze le az üzenetek, kivételek kezelése, több üzenetet párhuzamosan feldolgozásához, és végezze el a hello üzenetek.</span><span class="sxs-lookup"><span data-stu-id="97eac-178">hello advantage of using **OnMessage** compared too**Receive** is that users don’t have toomanually poll for messages, handle exceptions, process multiple messages in parallel, and complete hello messages.</span></span>

<span data-ttu-id="97eac-179">Ha meghívja a **Receive** nélkül használja az alapértelmezett értékét, lehet, hogy hello *ServerWaitTime* értéke nagyobb, mint egy perc.</span><span class="sxs-lookup"><span data-stu-id="97eac-179">If you call **Receive** without using its default value, be sure hello *ServerWaitTime* value is more than one minute.</span></span> <span data-ttu-id="97eac-180">Beállítás *ServerWaitTime* toomore egy percnél megakadályozza, hogy a hello kiszolgáló köszönőüzenetei teljesen megérkezése előtt időtúllépés miatt.</span><span class="sxs-lookup"><span data-stu-id="97eac-180">Setting *ServerWaitTime* toomore than one minute prevents hello server from timing out before hello message is fully received.</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-181">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-181">Solution</span></span>
<span data-ttu-id="97eac-182">Tekintse meg a következő ajánlott módjait hitelesítésikód-példák hello.</span><span class="sxs-lookup"><span data-stu-id="97eac-182">Please see hello following code examples for recommended usages.</span></span> <span data-ttu-id="97eac-183">További részletekért lásd: [QueueClient.OnMessage metódus (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)és [QueueClient.Receive metódus (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-183">For more details, see [QueueClient.OnMessage Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)and [QueueClient.Receive Method (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).</span></span>

<span data-ttu-id="97eac-184">tooimprove hello teljesítményének hello Azure üzenetcsere infrastruktúra esetén, lásd: hello kialakításban [aszinkron üzenetkezelési ismertetése](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-184">tooimprove hello performance of hello Azure messaging infrastructure, see hello design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

<span data-ttu-id="97eac-185">hello az alábbiakban látható egy példa segítségével **OnMessage** tooreceive üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="97eac-185">hello following is an example of using **OnMessage** tooreceive messages.</span></span>

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if hello message-pump should call complete on messages after hello callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates hello maximum number of concurrent calls toohello callback hello pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you tooget notified of any errors encountered by hello message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates hello message pump and callback is invoked for each message that is recieved, calling close on hello client will stop hello pump.
    {
        // Process hello message.
    }, options);
    Console.WriteLine("Press any key tooexit.");
    Console.ReadKey();
```

<span data-ttu-id="97eac-186">hello az alábbiakban látható egy példa segítségével **Receive** hello alapértelmezett kiszolgálóval várakozási idő.</span><span class="sxs-lookup"><span data-stu-id="97eac-186">hello following is an example of using **Receive** with hello default server wait time.</span></span>

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

<span data-ttu-id="97eac-187">hello az alábbiakban látható egy példa segítségével **Receive** egy nem alapértelmezett kiszolgálóval várakozási idő.</span><span class="sxs-lookup"><span data-stu-id="97eac-187">hello following is an example of using **Receive** with a non-default server wait time.</span></span>

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
## <a name="consider-using-asynchronous-service-bus-methods"></a><span data-ttu-id="97eac-188">Érdemes lehet aszinkron Service Bus-metódusok</span><span class="sxs-lookup"><span data-stu-id="97eac-188">Consider using asynchronous Service Bus methods</span></span>
### <a name="id"></a><span data-ttu-id="97eac-189">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-189">ID</span></span>
<span data-ttu-id="97eac-190">AP2003</span><span class="sxs-lookup"><span data-stu-id="97eac-190">AP2003</span></span>

### <a name="description"></a><span data-ttu-id="97eac-191">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-191">Description</span></span>
<span data-ttu-id="97eac-192">Aszinkron Service Bus módszerek tooimprove teljesítmény használata közvetítőalapú üzenettovábbítás.</span><span class="sxs-lookup"><span data-stu-id="97eac-192">Use asynchronous Service Bus methods tooimprove performance with brokered messaging.</span></span>

<span data-ttu-id="97eac-193">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-193">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-194">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-194">Reason</span></span>
<span data-ttu-id="97eac-195">Alkalmazás program egyidejű aszinkron metódusok használatával lehetővé teszi, mert minden hívás végrehajtása nem tiltja a hello fő szálnak.</span><span class="sxs-lookup"><span data-stu-id="97eac-195">Using asynchronous methods enables application program concurrency because executing each call doesn’t block hello main thread.</span></span> <span data-ttu-id="97eac-196">A Service Bus üzenetkezelés módszerek, egy olyan műveletet hajt használatakor (Küldés, kapni, törlés, stb.) időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="97eac-196">When using Service Bus messaging methods, performing an operation (send, receive, delete, etc.) takes time.</span></span> <span data-ttu-id="97eac-197">Most hello Service Bus szolgáltatás által hello feldolgozási hello művelet szerepel továbbá toohello késését hello helykérelemmel és válasszal hello.</span><span class="sxs-lookup"><span data-stu-id="97eac-197">This time includes hello processing of hello operation by hello Service Bus service in addition toohello latency of hello request and hello reply.</span></span> <span data-ttu-id="97eac-198">műveletek másodpercenkénti idő tooincrease hello száma műveletek végre kell hajtani egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="97eac-198">tooincrease hello number of operations per time, operations must execute concurrently.</span></span> <span data-ttu-id="97eac-199">További információt lásd túl[gyakorlati tanácsok a teljesítmény javítását használatával Service Bus Közvetítőalapú üzenetkezelés](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-199">For more information please refer too[Best Practices for Performance Improvements Using Service Bus Brokered Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-200">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-200">Solution</span></span>
<span data-ttu-id="97eac-201">Lásd: [QueueClient osztály (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) hogyan toouse hello ajánlott aszinkron metódus kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="97eac-201">See [QueueClient Class (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) for information about how toouse hello recommended asynchronous method.</span></span>

<span data-ttu-id="97eac-202">tooimprove hello teljesítményének hello Azure üzenetcsere infrastruktúra esetén, lásd: hello kialakításban [aszinkron üzenetkezelési ismertetése](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-202">tooimprove hello performance of hello Azure messaging infrastructure, see hello design pattern [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

## <a name="consider-partitioning-service-bus-queues-and-topics"></a><span data-ttu-id="97eac-203">Vegye figyelembe a particionálási Service Bus-üzenetsorok és témakörök</span><span class="sxs-lookup"><span data-stu-id="97eac-203">Consider partitioning Service Bus queues and topics</span></span>
### <a name="id"></a><span data-ttu-id="97eac-204">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-204">ID</span></span>
<span data-ttu-id="97eac-205">AP2004</span><span class="sxs-lookup"><span data-stu-id="97eac-205">AP2004</span></span>

### <a name="description"></a><span data-ttu-id="97eac-206">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-206">Description</span></span>
<span data-ttu-id="97eac-207">Partíció Service Bus-üzenetsorok és témakörök a jobb teljesítmény érdekében a Service Bus üzenetkezelés.</span><span class="sxs-lookup"><span data-stu-id="97eac-207">Partition Service Bus queues and topics for better performance with Service Bus messaging.</span></span>

<span data-ttu-id="97eac-208">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-208">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-209">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-209">Reason</span></span>
<span data-ttu-id="97eac-210">Particionálás a Service Bus-üzenetsorok és témakörök növeli a teljesítményt átviteli sebesség és a szolgáltatás rendelkezésre állási, mivel a hello teljes átviteli sebességgel particionált üzenetsor vagy témakör már nem korlátozzák egyetlen üzenet broker vagy üzenetküldési tárolóban hello teljesítmény szerint.</span><span class="sxs-lookup"><span data-stu-id="97eac-210">Partitioning Service Bus queues and topics increases performance throughput and service availability because hello overall throughput of a partitioned queue or topic is no longer limited by hello performance of a single message broker or messaging store.</span></span> <span data-ttu-id="97eac-211">Ezenkívül átmenetileg nem működik az üzenetküldési tárolóban nem elérhetetlenné particionált üzenetsor vagy témakör.</span><span class="sxs-lookup"><span data-stu-id="97eac-211">In addition, a temporary outage of a messaging store doesn’t make a partitioned queue or topic unavailable.</span></span> <span data-ttu-id="97eac-212">További információkért lásd: [üzenetküldési entitások particionálás](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-212">For more information, see [Partitioning Messaging Entities](https://msdn.microsoft.com/library/azure/dn520246.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-213">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-213">Solution</span></span>
<span data-ttu-id="97eac-214">a következő kódrészletben látható kód hogyan hello üzenetküldési entitások toopartition.</span><span class="sxs-lookup"><span data-stu-id="97eac-214">hello following code snippet shows how toopartition messaging entities.</span></span>

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

<span data-ttu-id="97eac-215">További információkért lásd: [particionálva Service Bus-üzenetsorok és témakörök |} A Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) hello, valamint [Microsoft Azure Service Bus particionálva várólista](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) minta.</span><span class="sxs-lookup"><span data-stu-id="97eac-215">For more information, see [Partitioned Service Bus Queues and Topics | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) and check out hello [Microsoft Azure Service Bus Partitioned Queue](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) sample.</span></span>

## <a name="do-not-set-sharedaccessstarttime"></a><span data-ttu-id="97eac-216">Ne adja meg az SharedAccessStartTime</span><span class="sxs-lookup"><span data-stu-id="97eac-216">Do not set SharedAccessStartTime</span></span>
### <a name="id"></a><span data-ttu-id="97eac-217">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-217">ID</span></span>
<span data-ttu-id="97eac-218">AP3001</span><span class="sxs-lookup"><span data-stu-id="97eac-218">AP3001</span></span>

### <a name="description"></a><span data-ttu-id="97eac-219">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-219">Description</span></span>
<span data-ttu-id="97eac-220">Ne SharedAccessStartTimeset toohello aktuális indításakor tooimmediately hello megosztott hozzáférési házirend használatával.</span><span class="sxs-lookup"><span data-stu-id="97eac-220">You should avoid using SharedAccessStartTimeset toohello current time tooimmediately start hello Shared Access policy.</span></span> <span data-ttu-id="97eac-221">Csak akkor kell tooset ezt a tulajdonságot, ha egy későbbi időpontban szeretné toostart hello megosztott hozzáférési házirend.</span><span class="sxs-lookup"><span data-stu-id="97eac-221">You only need tooset this property if you want toostart hello Shared Access policy at a later time.</span></span>

<span data-ttu-id="97eac-222">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-222">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-223">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-223">Reason</span></span>
<span data-ttu-id="97eac-224">Számítógépóra-szinkronizálás adatközpontok között enyhe időeltérése okoz.</span><span class="sxs-lookup"><span data-stu-id="97eac-224">Clock synchronization causes a slight time difference among datacenters.</span></span> <span data-ttu-id="97eac-225">Például úgy szeretné logikailag gondolja, hogy beállítás hello kezdési idejét a tároló SAS-házirend szerint hello aktuális idő DateTime.Now vagy egy hasonló módon, akkor hello SAS házirend tootake hatás azonnal.</span><span class="sxs-lookup"><span data-stu-id="97eac-225">For example, you would logically think setting hello start time of a storage SAS policy as hello current time by using DateTime.Now or a similar method will cause hello SAS policy tootake effect immediately.</span></span> <span data-ttu-id="97eac-226">Azonban hello enyhe időeltérést adatközpont között problémákat okozhat a mivel lehet, hogy néhány datacenter eset némileg későbbi, mint hello kezdési ideje, míg mások azt előre.</span><span class="sxs-lookup"><span data-stu-id="97eac-226">However, hello slight time differences between datacenters can cause problems with this since some datacenter times might be slightly later than hello start time, while others ahead of it.</span></span> <span data-ttu-id="97eac-227">Ennek eredményeképpen hello SAS házirend lejárhat gyorsan (vagy akár azonnal) Ha hello házirend élettartam értéke túl rövid.</span><span class="sxs-lookup"><span data-stu-id="97eac-227">As a result, hello SAS policy can expire quickly (or even immediately) if hello policy lifetime is set too short.</span></span>

<span data-ttu-id="97eac-228">Az Azure storage közös hozzáférésű Jogosultságkód használatával további útmutatást lásd: [bevezetéséről tábla SAS (közös hozzáférésű Jogosultságkód), az üzenetsor SAS és a frissítés tooBlob SAS - Microsoft Azure tárolás fejlesztői Blog - hely kezdőlap - MSDN-blogok](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-228">For more guidance on using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update tooBlob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-229">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-229">Solution</span></span>
<span data-ttu-id="97eac-230">Távolítsa el a hello utasításon belül, amely beállítja a hello megosztott hozzáférési házirend hello kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="97eac-230">Remove hello statement that sets hello start time of hello shared access policy.</span></span> <span data-ttu-id="97eac-231">hello Azure kód elemző eszközt biztosít a probléma egy javítást.</span><span class="sxs-lookup"><span data-stu-id="97eac-231">hello Azure Code Analysis tool provides a fix for this issue.</span></span> <span data-ttu-id="97eac-232">Biztonságkezelés a további információkért lásd: hello kialakításban [Valet kulcs mintát](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-232">For more information on security management, please see hello design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="97eac-233">a következő kódrészletet hello hello kód a hiba javítása mutatja be.</span><span class="sxs-lookup"><span data-stu-id="97eac-233">hello following code snippet demonstrates hello code fix for this issue.</span></span>

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a><span data-ttu-id="97eac-234">Megosztott hozzáférési házirend lejárati idő több mint öt perc kell lennie.</span><span class="sxs-lookup"><span data-stu-id="97eac-234">Shared Access Policy expiry time must be more than five minutes</span></span>
### <a name="id"></a><span data-ttu-id="97eac-235">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-235">ID</span></span>
<span data-ttu-id="97eac-236">AP3002</span><span class="sxs-lookup"><span data-stu-id="97eac-236">AP3002</span></span>

### <a name="description"></a><span data-ttu-id="97eac-237">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-237">Description</span></span>
<span data-ttu-id="97eac-238">Lehet, mint amennyit az adatközpontok különböző helyeken "óra eltérésére." néven ismert tooa feltétel miatt között órák eltérés öt perc</span><span class="sxs-lookup"><span data-stu-id="97eac-238">There can be as much as a five minute difference in clocks among datacenters at different locations due tooa condition known as "clock skew."</span></span> <span data-ttu-id="97eac-239">tooprevent hello SAS házirend jogkivonat lejárjanak korábbi, mint a tervezett, állítsa be hello lejárati idő toobe több mint öt perc.</span><span class="sxs-lookup"><span data-stu-id="97eac-239">tooprevent hello SAS policy token from expiring earlier than planned, set hello expiry time toobe more than five minutes.</span></span>

<span data-ttu-id="97eac-240">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-240">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-241">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-241">Reason</span></span>
<span data-ttu-id="97eac-242">Hello világ különböző helyeken adatközpontok egy órajel szinkronizálni.</span><span class="sxs-lookup"><span data-stu-id="97eac-242">Datacenters at different locations around hello world synchronize by a clock signal.</span></span> <span data-ttu-id="97eac-243">Óra jel tootravel toodifferent helyeket időt vesz igénybe, mert lehet a különböző földrajzi helyen adatközpontokat közötti idő eltérés Bár minden támaszkodnak szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="97eac-243">Because it takes time for clock signal tootravel toodifferent locations, there can be a time variance between datacenters at different geographical locations although everything is supposedly synchronized.</span></span> <span data-ttu-id="97eac-244">Ez eltérést hatással lehet a hello megosztott hozzáférési házirend kezdési időt és a lejárati időközt.</span><span class="sxs-lookup"><span data-stu-id="97eac-244">This time difference can affect hello Shared Access policy start time and expiration interval.</span></span> <span data-ttu-id="97eac-245">Ezért tooensure megosztott hozzáférési házirend azonnal érvénybe lép, hello kezdete nem adja meg.</span><span class="sxs-lookup"><span data-stu-id="97eac-245">Therefore, tooensure Shared Access policy takes effect immediately, don’t specify hello start time.</span></span> <span data-ttu-id="97eac-246">Ezenkívül ellenőrizze, hogy hello lejárati idő több mint 5 perc tooprevent korai időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="97eac-246">In addition, make sure hello expiration time is more than 5 minutes tooprevent early timeout.</span></span>

<span data-ttu-id="97eac-247">Az Azure storage közös hozzáférésű Jogosultságkód használatával kapcsolatos további információkért lásd: [bevezetéséről tábla SAS (közös hozzáférésű Jogosultságkód), az üzenetsor SAS és a frissítés tooBlob SAS - Microsoft Azure tárolás fejlesztői Blog - hely kezdőlap - MSDN-blogok](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-247">For more information about using Shared Access Signature on Azure storage, see [Introducing Table SAS (Shared Access Signature), Queue SAS and update tooBlob SAS - Microsoft Azure Storage Team Blog - Site Home - MSDN Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-248">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-248">Solution</span></span>
<span data-ttu-id="97eac-249">Biztonságkezelés további információkért lásd: hello kialakításban [Valet kulcs mintát](https://msdn.microsoft.com/library/dn568102.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-249">For more information on security management, see hello design pattern [Valet Key Pattern](https://msdn.microsoft.com/library/dn568102.aspx).</span></span>

<span data-ttu-id="97eac-250">hello az alábbiakban látható egy példa nem adja meg a megosztott hozzáférési házirend kezdő időpont.</span><span class="sxs-lookup"><span data-stu-id="97eac-250">hello following is an example of not specifying a Shared Access policy start time.</span></span>

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="97eac-251">hello például a megosztott hozzáférési házirend kezdési idő megadása a öt percnél nagyobb házirend lejárati időtartam látható.</span><span class="sxs-lookup"><span data-stu-id="97eac-251">hello following is an example of specifying a Shared Access policy start time with a policy expiration duration greater than five minutes.</span></span>

```
// hello shared access policy provides  
// read/write access toohello container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // tooensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

<span data-ttu-id="97eac-252">További információkért lásd: [létrehozhat és használhat egy közös hozzáférésű Jogosultságkód](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-252">For more information, see [Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="use-cloudconfigurationmanager"></a><span data-ttu-id="97eac-253">CloudConfigurationManager használata</span><span class="sxs-lookup"><span data-stu-id="97eac-253">Use CloudConfigurationManager</span></span>
### <a name="id"></a><span data-ttu-id="97eac-254">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-254">ID</span></span>
<span data-ttu-id="97eac-255">AP4000</span><span class="sxs-lookup"><span data-stu-id="97eac-255">AP4000</span></span>

### <a name="description"></a><span data-ttu-id="97eac-256">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-256">Description</span></span>
<span data-ttu-id="97eac-257">Hello segítségével [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) projektek osztály, például az Azure Websites és az Azure mobile services nem vezetnek be futásidejű problémákat.</span><span class="sxs-lookup"><span data-stu-id="97eac-257">Using hello [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) class for projects such as Azure Website and Azure mobile services won't introduce runtime issues.</span></span> <span data-ttu-id="97eac-258">Ajánlott eljárásként, célszerű egy jó ötlet toouse felhő[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) minden Azure felhőalapú alkalmazásokhoz konfigurációk kezelése egyesített módja.</span><span class="sxs-lookup"><span data-stu-id="97eac-258">As a best practice, however, it's a good idea toouse Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) as a unified way of managing configurations for all Azure Cloud applications.</span></span>

<span data-ttu-id="97eac-259">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-259">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-260">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-260">Reason</span></span>
<span data-ttu-id="97eac-261">CloudConfigurationManager hello konfigurációs fájl megfelelő toohello alkalmazás környezetet olvassa be.</span><span class="sxs-lookup"><span data-stu-id="97eac-261">CloudConfigurationManager reads hello configuration file appropriate toohello application environment.</span></span>

[<span data-ttu-id="97eac-262">CloudConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="97eac-262">CloudConfigurationManager</span></span>](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a><span data-ttu-id="97eac-263">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-263">Solution</span></span>
<span data-ttu-id="97eac-264">A kód toouse hello refactor [CloudConfigurationManager osztály](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-264">Refactor your code toouse hello [CloudConfigurationManager Class](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx).</span></span> <span data-ttu-id="97eac-265">Hiba a kód javítása hello Azure kód elemző eszköz által biztosított.</span><span class="sxs-lookup"><span data-stu-id="97eac-265">A code fix for this issue is provided by hello Azure Code Analysis tool.</span></span>

<span data-ttu-id="97eac-266">a következő kódrészletet hello hello kód a hiba javítása mutatja be.</span><span class="sxs-lookup"><span data-stu-id="97eac-266">hello following code snippet demonstrates hello code fix for this issue.</span></span> <span data-ttu-id="97eac-267">Csere</span><span class="sxs-lookup"><span data-stu-id="97eac-267">Replace</span></span>

`var settings = ConfigurationManager.AppSettings["mySettings"];`

<span data-ttu-id="97eac-268">a</span><span class="sxs-lookup"><span data-stu-id="97eac-268">with</span></span>

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

<span data-ttu-id="97eac-269">Íme egy példa hogyan toostore hello konfigurációs beállítás egy App.config vagy a Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="97eac-269">Here's an example of how toostore hello configuration setting in a App.config or Web.config file.</span></span> <span data-ttu-id="97eac-270">Adja hozzá a hello beállítások toohello appSettings szakaszt hello konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="97eac-270">Add hello settings toohello appSettings section of hello configuration file.</span></span> <span data-ttu-id="97eac-271">hello az alábbiakban az előző példakódban hello hello Web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="97eac-271">hello following is hello Web.config file for hello previous code example.</span></span>

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a><span data-ttu-id="97eac-272">Kerülje a kódolt kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="97eac-272">Avoid using hard-coded connection strings</span></span>
### <a name="id"></a><span data-ttu-id="97eac-273">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-273">ID</span></span>
<span data-ttu-id="97eac-274">AP4001</span><span class="sxs-lookup"><span data-stu-id="97eac-274">AP4001</span></span>

### <a name="description"></a><span data-ttu-id="97eac-275">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-275">Description</span></span>
<span data-ttu-id="97eac-276">Ha a kódolt kapcsolati karakterláncokat használ, és tooupdate kell őket, később lesz toomake módosítások tooyour forráskód rendelkezik, és fordítsa újra hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="97eac-276">If you use hard-coded connection strings and you need tooupdate them later, you’ll have toomake changes tooyour source code and recompile hello application.</span></span> <span data-ttu-id="97eac-277">Azonban ha egy konfigurációs fájlban tárolja a kapcsolati karakterláncokat, később bármikor módosíthatja azokat hello konfigurációs fájl egyszerűen frissítésével.</span><span class="sxs-lookup"><span data-stu-id="97eac-277">However, if you store your connection strings in a configuration file, you can change them later by simply updating hello configuration file.</span></span>

<span data-ttu-id="97eac-278">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-278">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-279">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-279">Reason</span></span>
<span data-ttu-id="97eac-280">Rögzített megadás kapcsolati karakterláncok a hibás célszerű, mert problémák amikor kapcsolati karakterláncok toobe gyorsan módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="97eac-280">Hard-coding connection strings is a bad practice because it introduces problems when connection strings need toobe changed quickly.</span></span> <span data-ttu-id="97eac-281">Ezenkívül ha hello projekt toobe toosource vezérlőben be van jelölve, kódolt kapcsolati karakterláncok vezethet biztonsági rések óta hello karakterláncok hello forráskód lehet megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="97eac-281">In addition, if hello project needs toobe checked in toosource control, hard-coded connection strings introduce security vulnerabilities since hello strings can be viewed in hello source code.</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-282">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-282">Solution</span></span>
<span data-ttu-id="97eac-283">Kapcsolati karakterláncok tárolni hello konfigurációs fájlok vagy az Azure környezetben.</span><span class="sxs-lookup"><span data-stu-id="97eac-283">Store connection strings in hello configuration files or Azure environments.</span></span>

* <span data-ttu-id="97eac-284">Önálló alkalmazások esetén használja az app.config toostore kapcsolatikarakterlánc-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="97eac-284">For standalone applications, use app.config toostore connection string settings.</span></span>
* <span data-ttu-id="97eac-285">IIS kiszolgálón futó webes alkalmazásokhoz használja a web.config toostore kapcsolati karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="97eac-285">For IIS-hosted web applications, use web.config toostore connection strings.</span></span>
* <span data-ttu-id="97eac-286">Az ASP.NET-alkalmazások vNext használja a configuration.json toostore kapcsolati karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="97eac-286">For ASP.NET vNext applications, use configuration.json toostore connection strings.</span></span>

<span data-ttu-id="97eac-287">Konfigurációk fájlok – például a web.config vagy az App.config fájlt használatáról információkért lásd: [ASP.NET webes beállítási útmutatója](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span><span class="sxs-lookup"><span data-stu-id="97eac-287">For information on using configurations files such as web.config or app.config, see [ASP.NET Web Configuration Guidelines](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx).</span></span> <span data-ttu-id="97eac-288">Az Azure környezeti változók munkahelyi információkért lásd: [webhelyek Azure: hogyan alkalmazás karakterláncok és a kapcsolati karakterláncok működik](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="97eac-288">For information on how Azure environment variables work, see [Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span> <span data-ttu-id="97eac-289">A kapcsolati karakterlánc tárolása verziókezelő információkért lásd: [ne tegye a bizalmas adatokat például kapcsolati karakterláncok forráskódraktárban tárolt fájlok](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span><span class="sxs-lookup"><span data-stu-id="97eac-289">For information on storing connection string in source control, see [avoid putting sensitive information such as connection strings in files that are stored in source code repository](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).</span></span>

## <a name="use-diagnostics-configuration-file"></a><span data-ttu-id="97eac-290">Diagnosztika konfigurációs fájl használata</span><span class="sxs-lookup"><span data-stu-id="97eac-290">Use diagnostics configuration file</span></span>
### <a name="id"></a><span data-ttu-id="97eac-291">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-291">ID</span></span>
<span data-ttu-id="97eac-292">AP5000</span><span class="sxs-lookup"><span data-stu-id="97eac-292">AP5000</span></span>

### <a name="description"></a><span data-ttu-id="97eac-293">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-293">Description</span></span>
<span data-ttu-id="97eac-294">Diagnosztikai beállítások konfigurálására a kódban, többek között a hello API programozási Microsoft.WindowsAzure.Diagnostics, hello diagnostics.wadcfg fájlban konfigurálni kell diagnosztika beállításait.</span><span class="sxs-lookup"><span data-stu-id="97eac-294">Instead of configuring diagnostics settings in your code such as by using hello Microsoft.WindowsAzure.Diagnostics programming API, you should configure diagnostics settings in hello diagnostics.wadcfg file.</span></span> <span data-ttu-id="97eac-295">(Vagy, ha Azure SDK 2.5 diagnostics.wadcfgx).</span><span class="sxs-lookup"><span data-stu-id="97eac-295">(Or, diagnostics.wadcfgx if you use Azure SDK 2.5).</span></span> <span data-ttu-id="97eac-296">Ezzel az eljárással diagnosztikai beállítások módosításával anélkül, hogy toorecompile a kódot.</span><span class="sxs-lookup"><span data-stu-id="97eac-296">By doing this, you can change diagnostics settings without having toorecompile your code.</span></span>

<span data-ttu-id="97eac-297">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-297">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-298">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-298">Reason</span></span>
<span data-ttu-id="97eac-299">Mielőtt az Azure SDK 2.5-ös (amely az Azure diagnostics 1.3), Azure Diagnostics (ÜVEGVATTA) sikerült konfigurálni a számos különböző módszer használatával: hozzáadná toohello konfigurációs blob Storage, feltétlenül szükséges kódot, deklaratív konfigurációs vagy hello alapértelmezett használatával konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="97eac-299">Before Azure SDK 2.5 (which uses Azure diagnostics 1.3), Azure Diagnostics (WAD) could be configured by using several different methods: adding it toohello configuration blob in storage, by using imperative code, declarative configuration, or hello default configuration.</span></span> <span data-ttu-id="97eac-300">Azonban hello elsődleges módon tooconfigure diagnosztika toouse egy XML-konfigurációs fájl (diagnostics.wadcfg vagy diagnositcs.wadcfgx SDK 2.5 és újabb) hello alkalmazás projektben.</span><span class="sxs-lookup"><span data-stu-id="97eac-300">However, hello preferred way tooconfigure diagnostics is toouse an XML configuration file (diagnostics.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later) in hello application project.</span></span> <span data-ttu-id="97eac-301">Ez a megközelítés a hello diagnostics.wadcfg fájl teljesen definiálja a konfigurációt hello és frissíthető és újra telepíteni fogja a.</span><span class="sxs-lookup"><span data-stu-id="97eac-301">In this approach, hello diagnostics.wadcfg file completely defines hello configuration and can be updated and redeployed at will.</span></span> <span data-ttu-id="97eac-302">Hello diagnostics.wadcfg konfigurációs fájljának hello használatát keverése hello programozási módszerek konfigurációk hello segítségével állítjuk [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)vagy [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx) osztályok tooconfusion vezethet.</span><span class="sxs-lookup"><span data-stu-id="97eac-302">Mixing hello use of hello diagnostics.wadcfg configuration file with hello programmatic methods of setting configurations by using hello [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)or [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)classes can lead tooconfusion.</span></span> <span data-ttu-id="97eac-303">Lásd: [inicializálása vagy Azure Diagnostics konfigurációjának módosítása](https://msdn.microsoft.com/library/azure/hh411537.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="97eac-303">See [Initialize or Change Azure Diagnostics Configuration](https://msdn.microsoft.com/library/azure/hh411537.aspx) for more information.</span></span>

<span data-ttu-id="97eac-304">(Az Azure SDK 2.5 része) ÜVEGVATTA 1.3 kezdve már nem lehetséges toouse kód tooconfigure diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="97eac-304">Beginning with WAD 1.3 (included with Azure SDK 2.5), it’s no longer possible toouse code tooconfigure diagnostics.</span></span> <span data-ttu-id="97eac-305">Ennek eredményeképpen csak biztosíthat hello konfiguráció alkalmazása vagy hello diagnosztika bővítmény frissítése.</span><span class="sxs-lookup"><span data-stu-id="97eac-305">As a result, you can only provide hello configuration when applying or updating hello diagnostics extension.</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-306">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-306">Solution</span></span>
<span data-ttu-id="97eac-307">Hello diagnosztika configuration designer toomove diagnosztikai beállítások toohello diagnosztika konfigurációs fájlt használja (diagnositcs.wadcfg vagy diagnositcs.wadcfgx SDK 2.5 és újabb).</span><span class="sxs-lookup"><span data-stu-id="97eac-307">Use hello diagnostics configuration designer toomove diagnostic settings toohello diagnostics configuration file (diagnositcs.wadcfg or diagnositcs.wadcfgx for SDK 2.5 and later).</span></span> <span data-ttu-id="97eac-308">Emellett ajánlott telepíteni [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) hello legújabb diagnosztikai funkciót használja.</span><span class="sxs-lookup"><span data-stu-id="97eac-308">It’s also recommended that you install [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) and use hello latest diagnostics feature.</span></span>

1. <span data-ttu-id="97eac-309">A megjeleníteni kívánt tooconfigure hello szerepkör hello helyi menüben kattintson a Tulajdonságok parancsra, és válassza a hello konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="97eac-309">On hello shortcut menu for hello role that you want tooconfigure, choose Properties, and then choose hello Configuration tab.</span></span>
2. <span data-ttu-id="97eac-310">A hello **diagnosztika** területen győződjön meg arról, hogy hello **engedélyezése diagnosztikai** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="97eac-310">In hello **Diagnostics** section, make sure that hello **Enable Diagnostics** check box is selected.</span></span>
3. <span data-ttu-id="97eac-311">Válassza ki a hello **konfigurálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="97eac-311">Choose hello **Configure** button.</span></span>

   ![Hello diagnosztika engedélyezése beállítás elérése](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   <span data-ttu-id="97eac-313">Lásd: [diagnosztika konfigurálása az Azure Cloud Services és a virtuális gépek](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="97eac-313">See [Configuring Diagnostics for Azure Cloud Services and Virtual Machines](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) for more information.</span></span>

## <a name="avoid-declaring-dbcontext-objects-as-static"></a><span data-ttu-id="97eac-314">Kerülje a DbContext objektumokat statikus deklaráló</span><span class="sxs-lookup"><span data-stu-id="97eac-314">Avoid declaring DbContext objects as static</span></span>
### <a name="id"></a><span data-ttu-id="97eac-315">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="97eac-315">ID</span></span>
<span data-ttu-id="97eac-316">AP6000</span><span class="sxs-lookup"><span data-stu-id="97eac-316">AP6000</span></span>

### <a name="description"></a><span data-ttu-id="97eac-317">Leírás</span><span class="sxs-lookup"><span data-stu-id="97eac-317">Description</span></span>
<span data-ttu-id="97eac-318">toosave memória elkerülése deklaráló statikus DBContext objektumokat.</span><span class="sxs-lookup"><span data-stu-id="97eac-318">toosave memory, avoid declaring DBContext objects as static.</span></span>

<span data-ttu-id="97eac-319">Ossza meg az ötletek és visszajelzés: [Azure kód elemzés visszajelzés](http://go.microsoft.com/fwlink/?LinkId=403771).</span><span class="sxs-lookup"><span data-stu-id="97eac-319">Please share your ideas and feedback at [Azure Code Analysis feedback](http://go.microsoft.com/fwlink/?LinkId=403771).</span></span>

### <a name="reason"></a><span data-ttu-id="97eac-320">Ok</span><span class="sxs-lookup"><span data-stu-id="97eac-320">Reason</span></span>
<span data-ttu-id="97eac-321">DBContext objektumok tárolására hello minden hívás lekérdezés eredményei.</span><span class="sxs-lookup"><span data-stu-id="97eac-321">DBContext objects hold hello query results from each call.</span></span> <span data-ttu-id="97eac-322">Statikus DBContext objektumok nem értékesítik, amíg a hello alkalmazástartomány eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="97eac-322">Static DBContext objects are not disposed until hello application domain is unloaded.</span></span> <span data-ttu-id="97eac-323">Ezért a statikus DBContext objektum nagy mennyiségű memóriát is felhasználhatnak.</span><span class="sxs-lookup"><span data-stu-id="97eac-323">Therefore, a static DBContext object can consume large amounts of memory.</span></span>

### <a name="solution"></a><span data-ttu-id="97eac-324">Megoldás</span><span class="sxs-lookup"><span data-stu-id="97eac-324">Solution</span></span>
<span data-ttu-id="97eac-325">DBContext deklarálható lokális változó vagy nem statikus mezőn, a feladat használni, és engedélyezze azt követően ártalmatlanítani.</span><span class="sxs-lookup"><span data-stu-id="97eac-325">Declare DBContext as a local variable or non-static instance field, use it for a task, and then let it be disposed of after use.</span></span>

<span data-ttu-id="97eac-326">a következő példa MVC-vezérlő osztályhoz hello bemutatja, hogyan toouse hello DBContext objektum.</span><span class="sxs-lookup"><span data-stu-id="97eac-326">hello following example MVC controller class shows how toouse hello DBContext object.</span></span>

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass tooDbContext        
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

## <a name="next-steps"></a><span data-ttu-id="97eac-327">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97eac-327">Next steps</span></span>
<span data-ttu-id="97eac-328">További információ az optimalizálás, és az Azure-alkalmazások esetén hibaelhárítási toolearn lásd [hibaelhárítása a webes alkalmazás az Azure App Service szolgáltatásban a Visual Studio használatával](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="97eac-328">toolearn more about optimizing and troubleshooting Azure apps, see [Troubleshoot a web app in Azure App Service using Visual Studio](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>
