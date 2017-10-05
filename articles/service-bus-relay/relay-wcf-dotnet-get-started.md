---
title: "Ismerkedés az Azure-továbbítási WCF továbbítja a .NET |} Microsoft Docs"
description: "Megtudhatja, hogyan használhatja az Azure továbbítási WCF továbbítók két különböző helyen üzemeltetett alkalmazások kapcsolódáshoz."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5493281a-c2e5-49f2-87ee-9d3ffb782c75
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: 1af1ac78398d65e6a87f0d24d6198f3dfbc82ffd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-relay-wcf-relays-with-net"></a><span data-ttu-id="fbe28-103">A .NET szegélyhálózatába, az Azure-továbbítási WCF használata</span><span class="sxs-lookup"><span data-stu-id="fbe28-103">How to use Azure Relay WCF relays with .NET</span></span>
<span data-ttu-id="fbe28-104">Ez a cikk ismerteti, hogyan használhatja az Azure-továbbítási szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fbe28-104">This article describes how to use the Azure Relay service.</span></span> <span data-ttu-id="fbe28-105">A kódminták C# nyelven íródtak, és a Windows Communication Foundation (WCF) API-t használják a Service Bus-összeállításban található bővítményekkel.</span><span class="sxs-lookup"><span data-stu-id="fbe28-105">The samples are written in C# and use the Windows Communication Foundation (WCF) API with extensions contained in the Service Bus assembly.</span></span> <span data-ttu-id="fbe28-106">Az Azure relayjel kapcsolatos további információkért lásd: a [Azure továbbítási áttekintése](relay-what-is-it.md).</span><span class="sxs-lookup"><span data-stu-id="fbe28-106">For more information about Azure relay, see the [Azure Relay overview](relay-what-is-it.md).</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a><span data-ttu-id="fbe28-107">Mi az a WCF továbbító?</span><span class="sxs-lookup"><span data-stu-id="fbe28-107">What is WCF Relay?</span></span>

<span data-ttu-id="fbe28-108">Az Azure [ *WCF továbbító* ](relay-what-is-it.md) szolgáltatás lehetővé teszi hibrid alkalmazások összeállítását, amelyek egy Azure-adatközpontban és a saját helyszíni vállalati környezetben is futnak.</span><span class="sxs-lookup"><span data-stu-id="fbe28-108">The Azure [*WCF Relay*](relay-what-is-it.md) service enables you to build hybrid applications that run in both an Azure datacenter and your own on-premises enterprise environment.</span></span> <span data-ttu-id="fbe28-109">A továbbítási szolgáltatás ezt úgy segíti elő, hogy biztonságosan teszik elérhetővé a anélkül, hogy meg kellene nyitni egy tűzfalkapcsolatot, vagy zavaró módosításokat kellene a vállalati hálózati infrastruktúrában végrehajtani a nyilvános felhőbe, a vállalati hálózaton található Windows Communication Foundation (WCF) szolgáltatásait.</span><span class="sxs-lookup"><span data-stu-id="fbe28-109">The relay service facilitates this by enabling you to securely expose Windows Communication Foundation (WCF) services that reside within a corporate enterprise network to the public cloud, without having to open a firewall connection, or requiring intrusive changes to a corporate network infrastructure.</span></span>

![A WCF Relay szolgáltatással kapcsolatos fogalmak](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

<span data-ttu-id="fbe28-111">Az Azure Relay lehetővé teszi WCF-szolgáltatások üzemeltetését a meglévő vállalati környezetben.</span><span class="sxs-lookup"><span data-stu-id="fbe28-111">Azure Relay enables you to host WCF services within your existing enterprise environment.</span></span> <span data-ttu-id="fbe28-112">Majd delegálhatja figyeli a bejövő munkamenetek és kérések a WCF-szolgáltatások a továbbítási szolgáltatás Azure-ban futó nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="fbe28-112">You can then delegate listening for incoming sessions and requests to these WCF services to the relay service running within Azure.</span></span> <span data-ttu-id="fbe28-113">Ez lehetővé teszi a szolgáltatások közzétételét az Azure-ban futó alkalmazáskódok, illetve mobil dolgozók vagy külső hálózaton lévő partnerkörnyezetek számára.</span><span class="sxs-lookup"><span data-stu-id="fbe28-113">This enables you to expose these services to application code running in Azure, or to mobile workers or extranet partner environments.</span></span> <span data-ttu-id="fbe28-114">Továbbító segítségével szabályozhatja, hogy biztonságosan ki férhet hozzá ezeket a szolgáltatásokat a minden részletre kiterjedő szinten.</span><span class="sxs-lookup"><span data-stu-id="fbe28-114">Relay enables you to securely control who can access these services at a fine-grained level.</span></span> <span data-ttu-id="fbe28-115">Hatékony és biztonságos módot biztosít a meglévő vállalati megoldásaiból származó alkalmazások és adatok közzétételére, valamint kihasználja a felhő előnyeit.</span><span class="sxs-lookup"><span data-stu-id="fbe28-115">It provides a powerful and secure way to expose application functionality and data from your existing enterprise solutions and take advantage of it from the cloud.</span></span>

<span data-ttu-id="fbe28-116">A cikkből megtudhatja, hogyan Azure továbbítási segítségével hozzon létre egy WCF-webszolgáltatás a TCP-csatornán kötelező, segítségével két fél között biztonságos párbeszédet megvalósító.</span><span class="sxs-lookup"><span data-stu-id="fbe28-116">This article discusses how to use Azure Relay to create a WCF web service, exposed using a TCP channel binding, that implements a secure conversation between two parties.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a><span data-ttu-id="fbe28-117">A Service Bus NuGet-csomag beszerzése</span><span class="sxs-lookup"><span data-stu-id="fbe28-117">Get the Service Bus NuGet package</span></span>
<span data-ttu-id="fbe28-118">A Service Bus API beszerzésének, valamint az alkalmazások az összes Service Bus-függőséggel való konfigurálásának legegyszerűbb módja a [Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus) telepítése.</span><span class="sxs-lookup"><span data-stu-id="fbe28-118">The [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is the easiest way to get the Service Bus API and to configure your application with all of the Service Bus dependencies.</span></span> <span data-ttu-id="fbe28-119">A NuGet-csomagnak a projektben való telepítéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fbe28-119">To install the NuGet package in your project, do the following:</span></span>

1. <span data-ttu-id="fbe28-120">A Megoldáskezelőben kattintson a jobb gombbal a **Hivatkozások** elemre, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="fbe28-120">In Solution Explorer, right-click **References**, then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="fbe28-121">Keressen a „Service Bus” kifejezésre, és válassza ki az **Microsoft Azure Service Bus** elemet.</span><span class="sxs-lookup"><span data-stu-id="fbe28-121">Search for "Service Bus" and select the **Microsoft Azure Service Bus** item.</span></span> <span data-ttu-id="fbe28-122">Kattintson az **Install** (Telepítés) gombra a telepítés befejezéséhez, majd zárja be a következő párbeszédpanelt:</span><span class="sxs-lookup"><span data-stu-id="fbe28-122">Click **Install** to complete the installation, then close the following dialog box:</span></span>
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a><span data-ttu-id="fbe28-123">Közzétételére és felhasználására a SOAP-webszolgáltatás TCP-vel</span><span class="sxs-lookup"><span data-stu-id="fbe28-123">Expose and consume a SOAP web service with TCP</span></span>
<span data-ttu-id="fbe28-124">Egy meglévő SOAP WCF-webszolgáltatás külső felhasználásra történő közzétételéhez módosítania kell a szolgáltatás kötéseit és címeit.</span><span class="sxs-lookup"><span data-stu-id="fbe28-124">To expose an existing WCF SOAP web service for external consumption, you must make changes to the service bindings and addresses.</span></span> <span data-ttu-id="fbe28-125">Ehhez a konfigurációs fájl módosítására vagy a kód módosítására lehet szükség, attól függően, hogy hogyan állította be és konfigurálta a WCF-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="fbe28-125">This may require changes to your configuration file or it could require code changes, depending on how you have set up and configured your WCF services.</span></span> <span data-ttu-id="fbe28-126">Vegye figyelembe, hogy a WCF lehetővé teszi, hogy több hálózati végpont használatát ugyanabban a szolgáltatásban, tehát megtarthatja a meglévő belső végpontokat egyszerre külső hozzáférés céljából továbbítási végpontok hozzáadása közben.</span><span class="sxs-lookup"><span data-stu-id="fbe28-126">Note that WCF allows you to have multiple network endpoints over the same service, so you can retain the existing internal endpoints while adding relay endpoints for external access at the same time.</span></span>

<span data-ttu-id="fbe28-127">Ebben a feladatban egy egyszerű WCF-szolgáltatás hozza létre, és adja hozzá a továbbítási figyelő.</span><span class="sxs-lookup"><span data-stu-id="fbe28-127">In this task, you build a simple WCF service and add a relay listener to it.</span></span> <span data-ttu-id="fbe28-128">A gyakorlat feltételezi a Visual Studio bizonyos fokú ismeretét, ezért nem ismerteti a projekt létrehozásának minden részletét.</span><span class="sxs-lookup"><span data-stu-id="fbe28-128">This exercise assumes some familiarity with Visual Studio, and therefore does not walk through all the details of creating a project.</span></span> <span data-ttu-id="fbe28-129">Ehelyett magára a kódra összpontosít.</span><span class="sxs-lookup"><span data-stu-id="fbe28-129">Instead, it focuses on the code.</span></span>

<span data-ttu-id="fbe28-130">Az alábbi lépések megkezdése előtt végezze el a következő eljárást a környezet beállításához:</span><span class="sxs-lookup"><span data-stu-id="fbe28-130">Before starting these steps, complete the following procedure to set up your environment:</span></span>

1. <span data-ttu-id="fbe28-131">A Visual Studióban a megoldáson belül hozzon létre egy konzolalkalmazást, amely két projektet, az „Ügyfél” és a „Szolgáltatás” projektet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fbe28-131">Within Visual Studio, create a console application that contains two projects, "Client" and "Service", within the solution.</span></span>
2. <span data-ttu-id="fbe28-132">Mindkét projekthez adja hozzá a Service Bus NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="fbe28-132">Add the Service Bus NuGet package to both projects.</span></span> <span data-ttu-id="fbe28-133">Ez a csomag hozzáadja a projektekhez az összes szükséges összeállítási referenciát.</span><span class="sxs-lookup"><span data-stu-id="fbe28-133">This package adds all the necessary assembly references to your projects.</span></span>

### <a name="how-to-create-the-service"></a><span data-ttu-id="fbe28-134">A szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbe28-134">How to create the service</span></span>
<span data-ttu-id="fbe28-135">Először hozza létre a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fbe28-135">First, create the service itself.</span></span> <span data-ttu-id="fbe28-136">Minden WCF-szolgáltatás legalább három különálló részből áll:</span><span class="sxs-lookup"><span data-stu-id="fbe28-136">Any WCF service consists of at least three distinct parts:</span></span>

* <span data-ttu-id="fbe28-137">Egy szerződés meghatározásából, amely leírja, hogy milyen üzenetek lesznek cserélve és milyen műveletek lesznek meghívva.</span><span class="sxs-lookup"><span data-stu-id="fbe28-137">Definition of a contract that describes what messages are exchanged and what operations are to be invoked.</span></span>
* <span data-ttu-id="fbe28-138">A szerződés megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="fbe28-138">Implementation of that contract.</span></span>
* <span data-ttu-id="fbe28-139">Egy állomásból, amely üzemelteti a WCF-szolgáltatást, és amely több végpontot is elérhetővé tesz.</span><span class="sxs-lookup"><span data-stu-id="fbe28-139">Host that hosts the WCF service and exposes several endpoints.</span></span>

<span data-ttu-id="fbe28-140">A jelen szakaszban lévő kódpéldák mindhárom összetevőt tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="fbe28-140">The code examples in this section address each of these components.</span></span>

<span data-ttu-id="fbe28-141">A szerződés egyetlen műveletet, a(z) `AddNumbers` műveletet definiálja, amely két számot ad össze, és visszaadja az eredményt.</span><span class="sxs-lookup"><span data-stu-id="fbe28-141">The contract defines a single operation, `AddNumbers`, that adds two numbers and returns the result.</span></span> <span data-ttu-id="fbe28-142">A(z) `IProblemSolverChannel` interfész lehetővé teszi az ügyfél számára, hogy egyszerűbben kezelje a proxy élettartamát.</span><span class="sxs-lookup"><span data-stu-id="fbe28-142">The `IProblemSolverChannel` interface enables the client to more easily manage the proxy lifetime.</span></span> <span data-ttu-id="fbe28-143">Az ilyen interfész létrehozása ajánlott eljárás.</span><span class="sxs-lookup"><span data-stu-id="fbe28-143">Creating such an interface is considered a best practice.</span></span> <span data-ttu-id="fbe28-144">Érdemes ezt a szerződést egy különálló fájlban elhelyezni, hogy az „Ügyfél” és a „Szolgáltatás” projektből is hivatkozni lehessen a fájlra, de a kódot bemásolhatja mindkét projektbe is.</span><span class="sxs-lookup"><span data-stu-id="fbe28-144">It's a good idea to put this contract definition into a separate file so that you can reference that file from both your "Client" and "Service" projects, but you can also copy the code into both projects.</span></span>

```csharp
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

<span data-ttu-id="fbe28-145">A szerződés elkészült, a megvalósítása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="fbe28-145">With the contract in place, the implementation is as follows:</span></span>

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a><span data-ttu-id="fbe28-146">Szolgáltatásgazda konfigurálása szoftveresen</span><span class="sxs-lookup"><span data-stu-id="fbe28-146">Configure a service host programmatically</span></span>
<span data-ttu-id="fbe28-147">Ha a szerződés és a megvalósítás elkészült, a szolgáltatás üzemeltethető.</span><span class="sxs-lookup"><span data-stu-id="fbe28-147">With the contract and implementation in place, you can now host the service.</span></span> <span data-ttu-id="fbe28-148">Az üzemeltetés egy [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) objektumon belül történik, amely gondoskodik a szolgáltatás felügyelő példányairól, és üzemelteti az üzeneteket figyelő végpontokat.</span><span class="sxs-lookup"><span data-stu-id="fbe28-148">Hosting occurs inside a [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) object, which takes care of managing instances of the service and hosts the endpoints that listen for messages.</span></span> <span data-ttu-id="fbe28-149">A következő kód egy normál helyi végponttal és egy továbbító végpont mutatja be a megjelenését, egymás mellett, a belső és külső végpontok úgy konfigurálja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fbe28-149">The following code configures the service with both a regular local endpoint and a relay endpoint to illustrate the appearance, side by side, of internal and external endpoints.</span></span> <span data-ttu-id="fbe28-150">A *namespace* karakterláncot helyettesítse a névtér nevével, a *yourKey* karakterláncot pedig az előző lépésben beszerzett SAS-kulccsal.</span><span class="sxs-lookup"><span data-stu-id="fbe28-150">Replace the string *namespace* with your namespace name and *yourKey* with the SAS key that you obtained in the previous setup step.</span></span>

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

<span data-ttu-id="fbe28-151">A példában két végpontot fog létrehozni, amelyek ugyanazon a szerződésmegvalósításhoz tartoznak.</span><span class="sxs-lookup"><span data-stu-id="fbe28-151">In the example, you create two endpoints that are on the same contract implementation.</span></span> <span data-ttu-id="fbe28-152">Az egyik helyi, egy Azure-továbbítási keresztül van kivetítve.</span><span class="sxs-lookup"><span data-stu-id="fbe28-152">One is local and one is projected through Azure Relay.</span></span> <span data-ttu-id="fbe28-153">A kettő közötti alapvető a kötéseik jelentik. [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) a helyi egy és [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) a továbbítási végpont és a címek.</span><span class="sxs-lookup"><span data-stu-id="fbe28-153">The key differences between them are the bindings; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) for the local one and [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) for the relay endpoint and the addresses.</span></span> <span data-ttu-id="fbe28-154">A helyi végpont egy helyi hálózati címmel rendelkezik egy különálló porttal.</span><span class="sxs-lookup"><span data-stu-id="fbe28-154">The local endpoint has a local network address with a distinct port.</span></span> <span data-ttu-id="fbe28-155">A továbbító végpont rendelkezik végpontcímmel karakterlánc `sb`, a névtér nevét, és az elérési út "solver."</span><span class="sxs-lookup"><span data-stu-id="fbe28-155">The relay endpoint has an endpoint address composed of the string `sb`, your namespace name, and the path "solver."</span></span> <span data-ttu-id="fbe28-156">Ennek eredményeként az URI `sb://[serviceNamespace].servicebus.windows.net/solver`, a szolgáltatás végpontjának azonosítása egy Service Bus (továbbítóként) TCP-végpontként külső teljesen minősített DNS-név.</span><span class="sxs-lookup"><span data-stu-id="fbe28-156">This results in the URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifying the service endpoint as a Service Bus (relay) TCP endpoint with a fully qualified external DNS name.</span></span> <span data-ttu-id="fbe28-157">Ha a kódot a helyőrzők behelyettesítésével elhelyezi a **Szolgáltatás** alkalmazás `Main` függvényében, egy működő szolgáltatást kap.</span><span class="sxs-lookup"><span data-stu-id="fbe28-157">If you place the code replacing the placeholders into the `Main` function of the **Service** application, you will have a functional service.</span></span> <span data-ttu-id="fbe28-158">Ha azt szeretné, hogy a szolgáltatás kizárólag a továbbítási figyelni, távolítsa el a helyi végpont deklarációját.</span><span class="sxs-lookup"><span data-stu-id="fbe28-158">If you want your service to listen exclusively on the relay, remove the local endpoint declaration.</span></span>

### <a name="configure-a-service-host-in-the-appconfig-file"></a><span data-ttu-id="fbe28-159">Szolgáltatásgazda konfigurálása az App.config fájlban</span><span class="sxs-lookup"><span data-stu-id="fbe28-159">Configure a service host in the App.config file</span></span>
<span data-ttu-id="fbe28-160">Az állomást az App.config fájl segítségével is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="fbe28-160">You can also configure the host using the App.config file.</span></span> <span data-ttu-id="fbe28-161">A szolgáltatásüzemeltetési kód ez esetben a következő példában látható.</span><span class="sxs-lookup"><span data-stu-id="fbe28-161">The service hosting code in this case appears in the next example.</span></span>

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

<span data-ttu-id="fbe28-162">A végpontdefiníciók ekkor az App.config fájlba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="fbe28-162">The endpoint definitions move into the App.config file.</span></span> <span data-ttu-id="fbe28-163">A NuGet-csomag már hozzá van adva a definíciók egy tartományát az App.config fájlhoz, amelyek az Azure-továbbítási szükséges konfigurációs bővítmények.</span><span class="sxs-lookup"><span data-stu-id="fbe28-163">The NuGet package has already added a range of definitions to the App.config file, which are the required configuration extensions for Azure Relay.</span></span> <span data-ttu-id="fbe28-164">A következő példának, amely pontosan megegyezik az előző kóddal, közvetlenül a **system.serviceModel** elem alatt kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="fbe28-164">The following example, which is the exact equivalent of the previous code, should appear directly beneath the **system.serviceModel** element.</span></span> <span data-ttu-id="fbe28-165">A kódpélda feltételezi, hogy a projekt C# névterének neve **Service**.</span><span class="sxs-lookup"><span data-stu-id="fbe28-165">This code example assumes that your project C# namespace is named **Service**.</span></span>
<span data-ttu-id="fbe28-166">A helyőrzőket cserélje le a továbbítási névtér nevét és a SAS-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="fbe28-166">Replace the placeholders with your relay namespace name and SAS key.</span></span>

```xml
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://<namespaceName>.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

<span data-ttu-id="fbe28-167">A módosítások végrehajtása után a szolgáltatás ugyanúgy elindul, mint korábban, de most két élő végponttal: egy helyivel, és eggyel, amely a felhőben figyel.</span><span class="sxs-lookup"><span data-stu-id="fbe28-167">After you make these changes, the service starts as it did before, but with two live endpoints: one local and one listening in the cloud.</span></span>

### <a name="create-the-client"></a><span data-ttu-id="fbe28-168">Az ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbe28-168">Create the client</span></span>
#### <a name="configure-a-client-programmatically"></a><span data-ttu-id="fbe28-169">Ügyfél konfigurálása szoftveresen</span><span class="sxs-lookup"><span data-stu-id="fbe28-169">Configure a client programmatically</span></span>
<span data-ttu-id="fbe28-170">A szolgáltatás felhasználásához összeállíthat egy WCF-ügyfelet egy [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objektum használatával.</span><span class="sxs-lookup"><span data-stu-id="fbe28-170">To consume the service, you can construct a WCF client using a [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) object.</span></span> <span data-ttu-id="fbe28-171">A Service Bus egy jogkivonat-alapú biztonsági modellt használ, amely a SAS segítségével van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="fbe28-171">Service Bus uses a token-based security model implemented using SAS.</span></span> <span data-ttu-id="fbe28-172">A [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) osztály egy beépített gyári metódusokat tartalmazó biztonságijogkivonat-szolgáltatót képvisel, amelyek néhány jól ismert jogkivonat-szolgáltatót adnak vissza.</span><span class="sxs-lookup"><span data-stu-id="fbe28-172">The [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) class represents a security token provider with built-in factory methods that return some well-known token providers.</span></span> <span data-ttu-id="fbe28-173">A következő példa a [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metódust használja a megfelelő SAS-jogkivonat beszerzésének kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="fbe28-173">The following example uses the [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) method to handle the acquisition of the appropriate SAS token.</span></span> <span data-ttu-id="fbe28-174">A név és a kulcs az előző szakaszban leírtak szerint a portálról lekért értékek.</span><span class="sxs-lookup"><span data-stu-id="fbe28-174">The name and key are those obtained from the portal as described in the previous section.</span></span>

<span data-ttu-id="fbe28-175">Először hivatkozzon a(z) `IProblemSolver` szerződéskódra, vagy másolja a szolgáltatásból az ügyfélprojektbe.</span><span class="sxs-lookup"><span data-stu-id="fbe28-175">First, reference or copy the `IProblemSolver` contract code from the service into your client project.</span></span>

<span data-ttu-id="fbe28-176">Ezután cserélje le a kód a `Main` metódus az ügyfél ismét cserélje le a helyőrzőket a továbbítási névtér és SAS-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="fbe28-176">Then, replace the code in the `Main` method of the client, again replacing the placeholder text with your relay namespace and SAS key.</span></span>

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "<namespaceName>", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

<span data-ttu-id="fbe28-177">Most már lefordíthatja az ügyfelet és a szolgáltatást, futtathatja őket (először a szolgáltatást), így az ügyfél meghívja a szolgáltatást, és a **9** értéket nyomtatja ki.</span><span class="sxs-lookup"><span data-stu-id="fbe28-177">You can now build the client and the service, run them (run the service first), and the client calls the service and prints **9**.</span></span> <span data-ttu-id="fbe28-178">Az ügyfelet és a szolgáltatást futtathatja eltérő gépeken, akár hálózatokon keresztül is, a kommunikáció továbbra is működni fog.</span><span class="sxs-lookup"><span data-stu-id="fbe28-178">You can run the client and server on different machines, even across networks, and the communication will still work.</span></span> <span data-ttu-id="fbe28-179">Az ügyfélkód a felhőben vagy helyben is futhat.</span><span class="sxs-lookup"><span data-stu-id="fbe28-179">The client code can also run in the cloud or locally.</span></span>

#### <a name="configure-a-client-in-the-appconfig-file"></a><span data-ttu-id="fbe28-180">Ügyfél konfigurálása az App.config fájlban</span><span class="sxs-lookup"><span data-stu-id="fbe28-180">Configure a client in the App.config file</span></span>
<span data-ttu-id="fbe28-181">A következő kód bemutatja, hogyan konfigurálhatja az ügyfelet az App.config fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="fbe28-181">The following code shows how to configure the client using the App.config file.</span></span>

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

<span data-ttu-id="fbe28-182">A végpontdefiníciók ekkor az App.config fájlba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="fbe28-182">The endpoint definitions move into the App.config file.</span></span> <span data-ttu-id="fbe28-183">A következő példának, amely megegyezik az előzőekben látható kóddal, közvetlenül alatt kell megjelennie a `<system.serviceModel>` elemet.</span><span class="sxs-lookup"><span data-stu-id="fbe28-183">The following example, which is the same as the code listed previously, should appear directly beneath the `<system.serviceModel>` element.</span></span> <span data-ttu-id="fbe28-184">Itt mint korábban, meg kell cserélje le a helyőrzőket a továbbítási névterére és SAS-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="fbe28-184">Here, as before, you must replace the placeholders with your relay namespace and SAS key.</span></span>

```xml
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://<namespaceName>.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a><span data-ttu-id="fbe28-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbe28-185">Next steps</span></span>
<span data-ttu-id="fbe28-186">Most, hogy megismerte az Azure-továbbító alapok, ezek hivatkozásokat követve tudhat meg többet.</span><span class="sxs-lookup"><span data-stu-id="fbe28-186">Now that you've learned the basics of Azure Relay, follow these links to learn more.</span></span>

* [<span data-ttu-id="fbe28-187">Mi az az Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="fbe28-187">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="fbe28-188">Az Azure Service Bus-architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="fbe28-188">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* <span data-ttu-id="fbe28-189">Töltse le a Service Bus-minták [Azure-minták] [ Azure samples] , vagy keresse meg a [Service Bus-minták áttekintése][overview of Service Bus samples].</span><span class="sxs-lookup"><span data-stu-id="fbe28-189">Download Service Bus samples from [Azure samples][Azure samples] or see the [overview of Service Bus samples][overview of Service Bus samples].</span></span>

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
