---
title: "aaaGet Azure továbbítási WCF-továbbítókat használhatnak a .NET használatába |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure továbbítási WCF továbbítja tooconnect két különböző helyen üzemeltetett alkalmazások."
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
ms.openlocfilehash: a652617fc2e9b7c8d62d39fa914f77df6e3a1771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-relay-wcf-relays-with-net"></a><span data-ttu-id="7e589-103">Hogyan toouse Azure továbbítási WCF továbbítja a .NET</span><span class="sxs-lookup"><span data-stu-id="7e589-103">How toouse Azure Relay WCF relays with .NET</span></span>
<span data-ttu-id="7e589-104">Ez a cikk ismerteti, hogyan toouse hello Azure továbbítási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7e589-104">This article describes how toouse hello Azure Relay service.</span></span> <span data-ttu-id="7e589-105">hello Kódminták C# nyelven íródtak, és hello Windows Communication Foundation (WCF) API használata hello Service Bus-összeállításban található bővítményekkel.</span><span class="sxs-lookup"><span data-stu-id="7e589-105">hello samples are written in C# and use hello Windows Communication Foundation (WCF) API with extensions contained in hello Service Bus assembly.</span></span> <span data-ttu-id="7e589-106">Az Azure relayjel kapcsolatos további információkért lásd: hello [Azure továbbítási áttekintése](relay-what-is-it.md).</span><span class="sxs-lookup"><span data-stu-id="7e589-106">For more information about Azure relay, see hello [Azure Relay overview](relay-what-is-it.md).</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a><span data-ttu-id="7e589-107">Mi az a WCF továbbító?</span><span class="sxs-lookup"><span data-stu-id="7e589-107">What is WCF Relay?</span></span>

<span data-ttu-id="7e589-108">hello Azure [ *WCF továbbító* ](relay-what-is-it.md) szolgáltatás lehetővé teszi, hogy toobuild hibrid alkalmazások, amelyek egy Azure-adatközpontban és a saját helyszíni vállalati környezetben is futnak.</span><span class="sxs-lookup"><span data-stu-id="7e589-108">hello Azure [*WCF Relay*](relay-what-is-it.md) service enables you toobuild hybrid applications that run in both an Azure datacenter and your own on-premises enterprise environment.</span></span> <span data-ttu-id="7e589-109">hello továbbítási szolgáltatás ezt úgy segíti elő, toosecurely teszi közzé a Windows Communication Foundation (WCF) szolgáltatásokat, amelyek a vállalati hálózati toohello nyilvános felhőben, anélkül, hogy tooopen egy tűzfalkapcsolatot, vagy igénylő zavaró módosításokat tooa vállalati hálózati infrastruktúrában.</span><span class="sxs-lookup"><span data-stu-id="7e589-109">hello relay service facilitates this by enabling you toosecurely expose Windows Communication Foundation (WCF) services that reside within a corporate enterprise network toohello public cloud, without having tooopen a firewall connection, or requiring intrusive changes tooa corporate network infrastructure.</span></span>

![A WCF Relay szolgáltatással kapcsolatos fogalmak](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

<span data-ttu-id="7e589-111">Az Azure Relay lehetővé teszi a meglévő vállalati környezetben toohost WCF-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="7e589-111">Azure Relay enables you toohost WCF services within your existing enterprise environment.</span></span> <span data-ttu-id="7e589-112">Figyeli a bejövő munkamenetek és kérések toothese WCF szolgáltatások toohello továbbítási szolgáltatás Azure-ban futó majd delegálhatja.</span><span class="sxs-lookup"><span data-stu-id="7e589-112">You can then delegate listening for incoming sessions and requests toothese WCF services toohello relay service running within Azure.</span></span> <span data-ttu-id="7e589-113">Ez lehetővé teszi, hogy Ön tooexpose ezek Azure-ban vagy toomobile munkavállalók vagy partner extranetes környezetben futó szolgáltatások tooapplication kód.</span><span class="sxs-lookup"><span data-stu-id="7e589-113">This enables you tooexpose these services tooapplication code running in Azure, or toomobile workers or extranet partner environments.</span></span> <span data-ttu-id="7e589-114">Relay lehetővé teszi toosecurely vezérlő, ki férhet hozzá ezeket a szolgáltatásokat a minden részletre kiterjedő szinten.</span><span class="sxs-lookup"><span data-stu-id="7e589-114">Relay enables you toosecurely control who can access these services at a fine-grained level.</span></span> <span data-ttu-id="7e589-115">A hatékony és biztonságos módot tooexpose az alkalmazás funkciói és az adatokat a meglévő vállalati megoldásaiból és előnyeit, hello felhőből biztosít.</span><span class="sxs-lookup"><span data-stu-id="7e589-115">It provides a powerful and secure way tooexpose application functionality and data from your existing enterprise solutions and take advantage of it from hello cloud.</span></span>

<span data-ttu-id="7e589-116">A cikk ismerteti, hogyan toouse Azure továbbítási toocreate egy WCF-webszolgáltatás kitett TCP-csatornakötéssel tehet, két fél között biztonságos párbeszédet megvalósító használatával.</span><span class="sxs-lookup"><span data-stu-id="7e589-116">This article discusses how toouse Azure Relay toocreate a WCF web service, exposed using a TCP channel binding, that implements a secure conversation between two parties.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-hello-service-bus-nuget-package"></a><span data-ttu-id="7e589-117">Hello Service Bus NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="7e589-117">Get hello Service Bus NuGet package</span></span>
<span data-ttu-id="7e589-118">Hello [Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus) hello legegyszerűbb módja tooget hello Service Bus API és tooconfigure az alkalmazás az összes Service Bus-függőséggel hello van.</span><span class="sxs-lookup"><span data-stu-id="7e589-118">hello [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is hello easiest way tooget hello Service Bus API and tooconfigure your application with all of hello Service Bus dependencies.</span></span> <span data-ttu-id="7e589-119">tooinstall hello NuGet-csomagot a projekt hello a következő:</span><span class="sxs-lookup"><span data-stu-id="7e589-119">tooinstall hello NuGet package in your project, do hello following:</span></span>

1. <span data-ttu-id="7e589-120">A Megoldáskezelőben kattintson a jobb gombbal a **Hivatkozások** elemre, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="7e589-120">In Solution Explorer, right-click **References**, then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="7e589-121">Keressen a "Service Bus" és a select hello **Microsoft Azure Service Bus** elemet.</span><span class="sxs-lookup"><span data-stu-id="7e589-121">Search for "Service Bus" and select hello **Microsoft Azure Service Bus** item.</span></span> <span data-ttu-id="7e589-122">Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a következő párbeszédpanel hello:</span><span class="sxs-lookup"><span data-stu-id="7e589-122">Click **Install** toocomplete hello installation, then close hello following dialog box:</span></span>
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a><span data-ttu-id="7e589-123">Közzétételére és felhasználására a SOAP-webszolgáltatás TCP-vel</span><span class="sxs-lookup"><span data-stu-id="7e589-123">Expose and consume a SOAP web service with TCP</span></span>
<span data-ttu-id="7e589-124">egy meglévő WCF SOAP-webszolgáltatás külső felhasználásra tooexpose, gondoskodnia kell módosítások toohello szolgáltatás kötéseit és címeit.</span><span class="sxs-lookup"><span data-stu-id="7e589-124">tooexpose an existing WCF SOAP web service for external consumption, you must make changes toohello service bindings and addresses.</span></span> <span data-ttu-id="7e589-125">Elképzelhető, hogy szükség módosítások tooyour konfigurációs fájl vagy a kód módosítására, attól függően, hogy hogyan hogy állítson be és konfigurálta a WCF-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="7e589-125">This may require changes tooyour configuration file or it could require code changes, depending on how you have set up and configured your WCF services.</span></span> <span data-ttu-id="7e589-126">Vegye figyelembe, hogy a WCF lehetővé teszi toohave több hálózati végpont keresztül hello ugyanazt a szolgáltatást, tehát megtarthatja a meglévő hello külső továbbítási végpontok hozzáadása közben belső végpont hozzáférni: hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="7e589-126">Note that WCF allows you toohave multiple network endpoints over hello same service, so you can retain hello existing internal endpoints while adding relay endpoints for external access at hello same time.</span></span>

<span data-ttu-id="7e589-127">Ebben a feladatban egy egyszerű WCF-szolgáltatás hozza létre, és adja hozzá a továbbítási figyelő tooit.</span><span class="sxs-lookup"><span data-stu-id="7e589-127">In this task, you build a simple WCF service and add a relay listener tooit.</span></span> <span data-ttu-id="7e589-128">Ebben a gyakorlatban azt feltételezi, hogy a Visual Studio bizonyos fokú ismeretét, és ezért nem végezze el a projekt létrehozása minden hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="7e589-128">This exercise assumes some familiarity with Visual Studio, and therefore does not walk through all hello details of creating a project.</span></span> <span data-ttu-id="7e589-129">Ehelyett hello kód összpontosít.</span><span class="sxs-lookup"><span data-stu-id="7e589-129">Instead, it focuses on hello code.</span></span>

<span data-ttu-id="7e589-130">A lépések megkezdése előtt hajtsa végre a következő eljárás tooset be a környezetet hello:</span><span class="sxs-lookup"><span data-stu-id="7e589-130">Before starting these steps, complete hello following procedure tooset up your environment:</span></span>

1. <span data-ttu-id="7e589-131">Visual Studio hozzon létre egy konzolalkalmazást, amely két projektet tartalmaz, "Ügyfél" és "Szolgáltatás" hello megoldás belül.</span><span class="sxs-lookup"><span data-stu-id="7e589-131">Within Visual Studio, create a console application that contains two projects, "Client" and "Service", within hello solution.</span></span>
2. <span data-ttu-id="7e589-132">Hello Service Bus NuGet csomag tooboth projektek hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="7e589-132">Add hello Service Bus NuGet package tooboth projects.</span></span> <span data-ttu-id="7e589-133">Ez a csomag összes hello szükséges szerelvény hivatkozások tooyour projektek hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="7e589-133">This package adds all hello necessary assembly references tooyour projects.</span></span>

### <a name="how-toocreate-hello-service"></a><span data-ttu-id="7e589-134">Hogyan toocreate hello szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7e589-134">How toocreate hello service</span></span>
<span data-ttu-id="7e589-135">Először hozzon létre hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7e589-135">First, create hello service itself.</span></span> <span data-ttu-id="7e589-136">Minden WCF-szolgáltatás legalább három különálló részből áll:</span><span class="sxs-lookup"><span data-stu-id="7e589-136">Any WCF service consists of at least three distinct parts:</span></span>

* <span data-ttu-id="7e589-137">Amely leírja, milyen üzenetek lesznek cserélve és milyen műveleteket toobe meghívni egy szerződés meghatározásából.</span><span class="sxs-lookup"><span data-stu-id="7e589-137">Definition of a contract that describes what messages are exchanged and what operations are toobe invoked.</span></span>
* <span data-ttu-id="7e589-138">A szerződés megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="7e589-138">Implementation of that contract.</span></span>
* <span data-ttu-id="7e589-139">Az gazdagépet, amely hello WCF-szolgáltatást futtatja, és elérhetővé teszi a több végpontok.</span><span class="sxs-lookup"><span data-stu-id="7e589-139">Host that hosts hello WCF service and exposes several endpoints.</span></span>

<span data-ttu-id="7e589-140">Ebben a szakaszban hello kódpéldák hárítsa el ezeket az összetevőket.</span><span class="sxs-lookup"><span data-stu-id="7e589-140">hello code examples in this section address each of these components.</span></span>

<span data-ttu-id="7e589-141">hello definiál egy művelettel, `AddNumbers`, amely két számot ad, és hello eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7e589-141">hello contract defines a single operation, `AddNumbers`, that adds two numbers and returns hello result.</span></span> <span data-ttu-id="7e589-142">Hello `IProblemSolverChannel` felület lehetővé teszi, hogy hello ügyfél toomore, könnyedén kezelhető hello proxy élettartamát.</span><span class="sxs-lookup"><span data-stu-id="7e589-142">hello `IProblemSolverChannel` interface enables hello client toomore easily manage hello proxy lifetime.</span></span> <span data-ttu-id="7e589-143">Az ilyen interfész létrehozása ajánlott eljárás.</span><span class="sxs-lookup"><span data-stu-id="7e589-143">Creating such an interface is considered a best practice.</span></span> <span data-ttu-id="7e589-144">Egy jó ötlet tooput a szerződést egy különálló fájlban definition, hogy a "Ügyfél" és a "Szolgáltatás" projektek is hivatkozni lehessen a fájlt, de hello kód be mindkét projektbe is másolhatja.</span><span class="sxs-lookup"><span data-stu-id="7e589-144">It's a good idea tooput this contract definition into a separate file so that you can reference that file from both your "Client" and "Service" projects, but you can also copy hello code into both projects.</span></span>

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

<span data-ttu-id="7e589-145">Hello szerződés elkészült, a hello megvalósítása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="7e589-145">With hello contract in place, hello implementation is as follows:</span></span>

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a><span data-ttu-id="7e589-146">Szolgáltatásgazda konfigurálása szoftveresen</span><span class="sxs-lookup"><span data-stu-id="7e589-146">Configure a service host programmatically</span></span>
<span data-ttu-id="7e589-147">Hello szerződés és a megvalósítás elkészült üzemeltethető hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7e589-147">With hello contract and implementation in place, you can now host hello service.</span></span> <span data-ttu-id="7e589-148">Üzemeltetési belül történik egy [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) objektum, amely gondoskodik felügyelő példányairól hello szolgáltatást, és a gazdagépek hello végpontok az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="7e589-148">Hosting occurs inside a [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) object, which takes care of managing instances of hello service and hosts hello endpoints that listen for messages.</span></span> <span data-ttu-id="7e589-149">hello alábbira konfigurálja hello szolgáltatást egy normál helyi végponttal és egy továbbító végpont tooillustrate hello megjelenését, egymás mellett, a belső és külső végpontok is.</span><span class="sxs-lookup"><span data-stu-id="7e589-149">hello following code configures hello service with both a regular local endpoint and a relay endpoint tooillustrate hello appearance, side by side, of internal and external endpoints.</span></span> <span data-ttu-id="7e589-150">Cserélje le a hello karakterlánc *névtér* a névtér nevével és *yourKey* hello előző telepítő lépésben beszerzett hello SAS-kulccsal.</span><span class="sxs-lookup"><span data-stu-id="7e589-150">Replace hello string *namespace* with your namespace name and *yourKey* with hello SAS key that you obtained in hello previous setup step.</span></span>

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

Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();

sh.Close();
```

<span data-ttu-id="7e589-151">Hello példában két végpontot, amely a hello létrehozása azonos szerződés megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="7e589-151">In hello example, you create two endpoints that are on hello same contract implementation.</span></span> <span data-ttu-id="7e589-152">Az egyik helyi, egy Azure-továbbítási keresztül van kivetítve.</span><span class="sxs-lookup"><span data-stu-id="7e589-152">One is local and one is projected through Azure Relay.</span></span> <span data-ttu-id="7e589-153">hello fő különbségek hello kötések; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) a helyi hello és [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) hello továbbítási végpont és hello címek.</span><span class="sxs-lookup"><span data-stu-id="7e589-153">hello key differences between them are hello bindings; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) for hello local one and [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) for hello relay endpoint and hello addresses.</span></span> <span data-ttu-id="7e589-154">hello helyi végpont egy különálló porttal rendelkező helyi hálózati címmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7e589-154">hello local endpoint has a local network address with a distinct port.</span></span> <span data-ttu-id="7e589-155">hello továbbítási végpont rendelkezik végpontcímmel hello karakterlánc `sb`, a névtér nevét, és a hello elérési út "solver."</span><span class="sxs-lookup"><span data-stu-id="7e589-155">hello relay endpoint has an endpoint address composed of hello string `sb`, your namespace name, and hello path "solver."</span></span> <span data-ttu-id="7e589-156">Az eredmény hello URI `sb://[serviceNamespace].servicebus.windows.net/solver`, hello szolgáltatás végpontjának azonosítása egy Service Bus (továbbítóként) TCP-végpontként külső teljesen minősített DNS-név.</span><span class="sxs-lookup"><span data-stu-id="7e589-156">This results in hello URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifying hello service endpoint as a Service Bus (relay) TCP endpoint with a fully qualified external DNS name.</span></span> <span data-ttu-id="7e589-157">Helyezi-e hello kód cseréje hello helyőrzőket a hello `Main` hello funkcióját **szolgáltatás** alkalmazás, hogy egy működő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7e589-157">If you place hello code replacing hello placeholders into hello `Main` function of hello **Service** application, you will have a functional service.</span></span> <span data-ttu-id="7e589-158">Ha azt szeretné, hogy a szolgáltatás toolisten kizárólag a hello továbbítási, távolítsa el a hello helyi végpont deklarációját.</span><span class="sxs-lookup"><span data-stu-id="7e589-158">If you want your service toolisten exclusively on hello relay, remove hello local endpoint declaration.</span></span>

### <a name="configure-a-service-host-in-hello-appconfig-file"></a><span data-ttu-id="7e589-159">Szolgáltatásgazda konfigurálása hello App.config fájlban</span><span class="sxs-lookup"><span data-stu-id="7e589-159">Configure a service host in hello App.config file</span></span>
<span data-ttu-id="7e589-160">Hello állomás hello App.config fájl segítségével is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="7e589-160">You can also configure hello host using hello App.config file.</span></span> <span data-ttu-id="7e589-161">hello szolgáltatásüzemeltetési kód ebben az esetben a következő példában hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7e589-161">hello service hosting code in this case appears in hello next example.</span></span>

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();
sh.Close();
```

<span data-ttu-id="7e589-162">hello App.config fájlba hello végpontdefiníciók.</span><span class="sxs-lookup"><span data-stu-id="7e589-162">hello endpoint definitions move into hello App.config file.</span></span> <span data-ttu-id="7e589-163">hello NuGet-csomag már hozzáadta definíciók toohello App.config fájlt, számos, amelyeket hello szükséges konfigurációs bővítmények továbbítási Azure.</span><span class="sxs-lookup"><span data-stu-id="7e589-163">hello NuGet package has already added a range of definitions toohello App.config file, which are hello required configuration extensions for Azure Relay.</span></span> <span data-ttu-id="7e589-164">a következő példának, amely pontos hello hello egyenértékű hello előző kóddal, közvetlenül a hello alatt kell megjelennie **system.serviceModel** elemet.</span><span class="sxs-lookup"><span data-stu-id="7e589-164">hello following example, which is hello exact equivalent of hello previous code, should appear directly beneath hello **system.serviceModel** element.</span></span> <span data-ttu-id="7e589-165">A kódpélda feltételezi, hogy a projekt C# névterének neve **Service**.</span><span class="sxs-lookup"><span data-stu-id="7e589-165">This code example assumes that your project C# namespace is named **Service**.</span></span>
<span data-ttu-id="7e589-166">Hello helyőrzőket cserélje le a továbbítási névtér nevét és a SAS-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7e589-166">Replace hello placeholders with your relay namespace name and SAS key.</span></span>

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

<span data-ttu-id="7e589-167">Miután elvégezte ezeket a módosításokat, hello szolgáltatás elindul-e, mint korábban, de most két élő végponttal: egy helyivel, és egy figyelő hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="7e589-167">After you make these changes, hello service starts as it did before, but with two live endpoints: one local and one listening in hello cloud.</span></span>

### <a name="create-hello-client"></a><span data-ttu-id="7e589-168">Hello ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e589-168">Create hello client</span></span>
#### <a name="configure-a-client-programmatically"></a><span data-ttu-id="7e589-169">Ügyfél konfigurálása szoftveresen</span><span class="sxs-lookup"><span data-stu-id="7e589-169">Configure a client programmatically</span></span>
<span data-ttu-id="7e589-170">tooconsume hello szolgáltatást, a WCF-ügyfél segítségével összeállíthat egy [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objektum.</span><span class="sxs-lookup"><span data-stu-id="7e589-170">tooconsume hello service, you can construct a WCF client using a [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) object.</span></span> <span data-ttu-id="7e589-171">A Service Bus egy jogkivonat-alapú biztonsági modellt használ, amely a SAS segítségével van megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="7e589-171">Service Bus uses a token-based security model implemented using SAS.</span></span> <span data-ttu-id="7e589-172">Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) osztály biztonsági jogkivonat-szolgáltató jelöli, amelyek néhány jól ismert jogkivonat-szolgáltatót adnak vissza beépített gyári metódusokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="7e589-172">hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) class represents a security token provider with built-in factory methods that return some well-known token providers.</span></span> <span data-ttu-id="7e589-173">hello alábbi példában hello [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metódus toohandle hello hello megfelelő SAS-jogkivonat megszerzése.</span><span class="sxs-lookup"><span data-stu-id="7e589-173">hello following example uses hello [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) method toohandle hello acquisition of hello appropriate SAS token.</span></span> <span data-ttu-id="7e589-174">hello név és kulcs azok hello előző szakaszban leírtak szerint hello portálról beszerzett.</span><span class="sxs-lookup"><span data-stu-id="7e589-174">hello name and key are those obtained from hello portal as described in hello previous section.</span></span>

<span data-ttu-id="7e589-175">Első, referencia vagy másolási hello `IProblemSolver` szerződéskódra hello szolgáltatásból az ügyfélprojektbe.</span><span class="sxs-lookup"><span data-stu-id="7e589-175">First, reference or copy hello `IProblemSolver` contract code from hello service into your client project.</span></span>

<span data-ttu-id="7e589-176">Ezután cserélje le a hello hello kód `Main` metódus hello ügyfél újra hello helyőrző szöveg cseréje a továbbítási névterére és SAS-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7e589-176">Then, replace hello code in hello `Main` method of hello client, again replacing hello placeholder text with your relay namespace and SAS key.</span></span>

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

<span data-ttu-id="7e589-177">Most már lefordíthatja hello ügyfél és a hello szolgáltatást, futtathatja őket (hello szolgáltatás futtatásához először), és hello ügyfél meghívja a hello szolgáltatást, és kiírja **9**.</span><span class="sxs-lookup"><span data-stu-id="7e589-177">You can now build hello client and hello service, run them (run hello service first), and hello client calls hello service and prints **9**.</span></span> <span data-ttu-id="7e589-178">Futtathatja hello ügyfél- és a különböző gépeken, akár hálózatokon keresztül, és hello kommunikáció továbbra is működni fognak.</span><span class="sxs-lookup"><span data-stu-id="7e589-178">You can run hello client and server on different machines, even across networks, and hello communication will still work.</span></span> <span data-ttu-id="7e589-179">hello Ügyfélkód hello felhőben vagy helyben is futtatható.</span><span class="sxs-lookup"><span data-stu-id="7e589-179">hello client code can also run in hello cloud or locally.</span></span>

#### <a name="configure-a-client-in-hello-appconfig-file"></a><span data-ttu-id="7e589-180">Ügyfél konfigurálása hello App.config fájlban</span><span class="sxs-lookup"><span data-stu-id="7e589-180">Configure a client in hello App.config file</span></span>
<span data-ttu-id="7e589-181">hello a következő kód bemutatja, hogyan tooconfigure hello ügyfelet hello App.config fájl.</span><span class="sxs-lookup"><span data-stu-id="7e589-181">hello following code shows how tooconfigure hello client using hello App.config file.</span></span>

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

<span data-ttu-id="7e589-182">hello App.config fájlba hello végpontdefiníciók.</span><span class="sxs-lookup"><span data-stu-id="7e589-182">hello endpoint definitions move into hello App.config file.</span></span> <span data-ttu-id="7e589-183">hello következő példának, amely van hello ugyanaz, mint a korábban felsorolt hello kódot, alatt kell megjelennie közvetlenül hello `<system.serviceModel>` elemet.</span><span class="sxs-lookup"><span data-stu-id="7e589-183">hello following example, which is hello same as hello code listed previously, should appear directly beneath hello `<system.serviceModel>` element.</span></span> <span data-ttu-id="7e589-184">Itt mint korábban, le kell cserélnie hello helyőrzőket a továbbítási névtér és SAS-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7e589-184">Here, as before, you must replace hello placeholders with your relay namespace and SAS key.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7e589-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e589-185">Next steps</span></span>
<span data-ttu-id="7e589-186">Most, hogy megismerte az Azure-továbbítási hello alapjait, kövesse az alábbi hivatkozások további toolearn.</span><span class="sxs-lookup"><span data-stu-id="7e589-186">Now that you've learned hello basics of Azure Relay, follow these links toolearn more.</span></span>

* [<span data-ttu-id="7e589-187">Mi az az Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="7e589-187">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="7e589-188">Az Azure Service Bus-architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="7e589-188">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* <span data-ttu-id="7e589-189">Töltse le a Service Bus-minták [Azure-minták] [ Azure samples] , vagy keresse meg a hello [Service Bus-minták áttekintése][overview of Service Bus samples].</span><span class="sxs-lookup"><span data-stu-id="7e589-189">Download Service Bus samples from [Azure samples][Azure samples] or see hello [overview of Service Bus samples][overview of Service Bus samples].</span></span>

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
