---
title: "aaaReliable szolgáltatások WCF kommunikációs verem |} Microsoft Docs"
description: "hello beépített WCF kommunikációs verem a Service Fabric-ügyfélszolgáltatás WCF kommunikáció Reliable Services biztosítja."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 75516e1e-ee57-4bc7-95fe-71ec42d452b2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/07/2017
ms.author: bharatn
ms.openlocfilehash: 7feebef4d46a6ae66d05129f47f9b5911e82aec9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a><span data-ttu-id="46bcf-103">WCF-alapú kommunikációs verem a Reliable Services</span><span class="sxs-lookup"><span data-stu-id="46bcf-103">WCF-based communication stack for Reliable Services</span></span>
<span data-ttu-id="46bcf-104">lehetővé teszi a keretrendszer hello Reliable Services szolgáltatás szerzők toochoose hello kommunikációs verem toouse szeretnék a szolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="46bcf-104">hello Reliable Services framework allows service authors toochoose hello communication stack that they want toouse for their service.</span></span> <span data-ttu-id="46bcf-105">Azok az általuk választott keresztül hello hello kommunikációs verem csatlakoztatható **ICommunicationListener** hello által visszaadott [CreateServiceReplicaListeners vagy CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) módszerek.</span><span class="sxs-lookup"><span data-stu-id="46bcf-105">They can plug in hello communication stack of their choice via hello **ICommunicationListener** returned from hello [CreateServiceReplicaListeners or CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) methods.</span></span> <span data-ttu-id="46bcf-106">hello keretrendszer által biztosított szolgáltatás szerzők számára, akik toouse WCF-alapú kommunikációt hello Windows Communication Foundation (WCF) alapuló hello kommunikációs verem megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="46bcf-106">hello framework provides an implementation of hello communication stack based on hello Windows Communication Foundation (WCF) for service authors who want toouse WCF-based communication.</span></span>

## <a name="wcf-communication-listener"></a><span data-ttu-id="46bcf-107">WCF kommunikációs figyelő</span><span class="sxs-lookup"><span data-stu-id="46bcf-107">WCF Communication Listener</span></span>
<span data-ttu-id="46bcf-108">WCF-specifikus hello végrehajtásának **ICommunicationListener** hello által biztosított **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** osztály.</span><span class="sxs-lookup"><span data-stu-id="46bcf-108">hello WCF-specific implementation of **ICommunicationListener** is provided by hello **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** class.</span></span>

<span data-ttu-id="46bcf-109">Közpénzek ne mondja ki a szolgáltatási szerződés típusú tudunk.`ICalculator`</span><span class="sxs-lookup"><span data-stu-id="46bcf-109">Lest say we have a service contract of type `ICalculator`</span></span>

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

<span data-ttu-id="46bcf-110">A következő módon hello szolgáltatás hello azt hozhat létre egy WCF-kommunikáció figyelő.</span><span class="sxs-lookup"><span data-stu-id="46bcf-110">We can create a WCF communication listener in hello service hello following manner.</span></span>

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // hello name of hello endpoint configured in hello ServiceManifest under hello Endpoints section
            // that identifies hello endpoint that hello WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate hello binding information that you want hello service toouse.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-hello-wcf-communication-stack"></a><span data-ttu-id="46bcf-111">Ügyfelek hello WCF kommunikációs verem írása</span><span class="sxs-lookup"><span data-stu-id="46bcf-111">Writing clients for hello WCF communication stack</span></span>
<span data-ttu-id="46bcf-112">WCF, hello keretrendszer használatával szolgáltatásokkal toocommunicate biztosít az ügyfelek írásra **WcfClientCommunicationFactory**, ez a WCF-specifikus hello végrehajtásának [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="46bcf-112">For writing clients toocommunicate with services by using WCF, hello framework provides **WcfClientCommunicationFactory**, which is hello WCF-specific implementation of [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).</span></span>

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

<span data-ttu-id="46bcf-113">hello WCF kommunikációs csatorna számára elérhető hello **WcfCommunicationClient** hello által létrehozott **WcfCommunicationClientFactory**.</span><span class="sxs-lookup"><span data-stu-id="46bcf-113">hello WCF communication channel can be accessed from hello **WcfCommunicationClient** created by hello **WcfCommunicationClientFactory**.</span></span>

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

<span data-ttu-id="46bcf-114">Ügyfélkód használható hello **WcfCommunicationClientFactory** együtt hello **WcfCommunicationClient** mely valósít meg **ServicePartitionClient** toodetermine Szolgáltatásvégpont hello és hello szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="46bcf-114">Client code can use hello **WcfCommunicationClientFactory** along with hello **WcfCommunicationClient** which implements **ServicePartitionClient** toodetermine hello service endpoint and communicate with hello service.</span></span>

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with hello ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call hello service tooperform hello operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
> [!NOTE]
> <span data-ttu-id="46bcf-115">hello alapértelmezett ServicePartitionResolver azt feltételezi, hogy hello ügyfél futtatja a fürtön, amelyen hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="46bcf-115">hello default ServicePartitionResolver assumes that hello client is running in same cluster as hello service.</span></span> <span data-ttu-id="46bcf-116">Ha, amely nem hello esetben ServicePartitionResolver objektum létrehozása, és hello fürt kapcsolati végpontok adjon át.</span><span class="sxs-lookup"><span data-stu-id="46bcf-116">If that is not hello case, create a ServicePartitionResolver object and pass in hello cluster connection endpoints.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="46bcf-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46bcf-117">Next steps</span></span>
* [<span data-ttu-id="46bcf-118">A távoli eljáráshívás a Reliable Services távoli eljáráshívás</span><span class="sxs-lookup"><span data-stu-id="46bcf-118">Remote procedure call with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="46bcf-119">Webes API-t a Reliable Services OWIN</span><span class="sxs-lookup"><span data-stu-id="46bcf-119">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="46bcf-120">A Reliable Services kommunikáció biztonságához</span><span class="sxs-lookup"><span data-stu-id="46bcf-120">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)

