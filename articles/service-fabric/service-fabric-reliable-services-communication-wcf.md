---
title: "Megbízható szolgáltatások WCF kommunikációs verem |} Microsoft Docs"
description: "A beépített WCF kommunikációs verem a Service Fabric-ügyfélszolgáltatás WCF kommunikációt a Reliable Services biztosít."
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
ms.openlocfilehash: 7037620ebdc26a9f18531064bf45d058f5060e39
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a><span data-ttu-id="aa190-103">WCF-alapú kommunikációs verem a Reliable Services</span><span class="sxs-lookup"><span data-stu-id="aa190-103">WCF-based communication stack for Reliable Services</span></span>
<span data-ttu-id="aa190-104">A Reliable Services keretrendszer lehetővé teszi, hogy a szolgáltatás szerzők kiválasztása a kommunikációs verem, amelyeket be szeretne használni a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="aa190-104">The Reliable Services framework allows service authors to choose the communication stack that they want to use for their service.</span></span> <span data-ttu-id="aa190-105">Azok a kommunikációs verem az általuk választott keresztül is csatlakoztathatja a **ICommunicationListener** által visszaadott a [CreateServiceReplicaListeners vagy CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) módszerek.</span><span class="sxs-lookup"><span data-stu-id="aa190-105">They can plug in the communication stack of their choice via the **ICommunicationListener** returned from the [CreateServiceReplicaListeners or CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) methods.</span></span> <span data-ttu-id="aa190-106">A keretrendszer a kommunikációs verem, a Windows Communication Foundation (WCF) szolgáltatást szeretné használni a WCF-alapú kommunikációt szerzőknek alapú megvalósítását.</span><span class="sxs-lookup"><span data-stu-id="aa190-106">The framework provides an implementation of the communication stack based on the Windows Communication Foundation (WCF) for service authors who want to use WCF-based communication.</span></span>

## <a name="wcf-communication-listener"></a><span data-ttu-id="aa190-107">WCF kommunikációs figyelő</span><span class="sxs-lookup"><span data-stu-id="aa190-107">WCF Communication Listener</span></span>
<span data-ttu-id="aa190-108">A WCF-specifikus végrehajtásának **ICommunicationListener** biztosítja a **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** osztály.</span><span class="sxs-lookup"><span data-stu-id="aa190-108">The WCF-specific implementation of **ICommunicationListener** is provided by the **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** class.</span></span>

<span data-ttu-id="aa190-109">Közpénzek ne mondja ki a szolgáltatási szerződés típusú tudunk.`ICalculator`</span><span class="sxs-lookup"><span data-stu-id="aa190-109">Lest say we have a service contract of type `ICalculator`</span></span>

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

<span data-ttu-id="aa190-110">A szolgáltatás a következő módon létrehozhatunk olyan WCF kommunikációs figyelő.</span><span class="sxs-lookup"><span data-stu-id="aa190-110">We can create a WCF communication listener in the service the following manner.</span></span>

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a><span data-ttu-id="aa190-111">A WCF kommunikációs verem ügyfelek írása</span><span class="sxs-lookup"><span data-stu-id="aa190-111">Writing clients for the WCF communication stack</span></span>
<span data-ttu-id="aa190-112">Ügyfelek szolgáltatásokkal kommunikálni a WCF írásra, a keretrendszer által biztosított **WcfClientCommunicationFactory**, vagyis a WCF-specifikus végrehajtásának [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="aa190-112">For writing clients to communicate with services by using WCF, the framework provides **WcfClientCommunicationFactory**, which is the WCF-specific implementation of [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).</span></span>

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

<span data-ttu-id="aa190-113">A WCF kommunikációs csatornát érhetők el a **WcfCommunicationClient** hozta létre a **WcfCommunicationClientFactory**.</span><span class="sxs-lookup"><span data-stu-id="aa190-113">The WCF communication channel can be accessed from the **WcfCommunicationClient** created by the **WcfCommunicationClientFactory**.</span></span>

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

<span data-ttu-id="aa190-114">Ügyfélkód használhatja a **WcfCommunicationClientFactory** együtt a **WcfCommunicationClient** mely valósít meg **ServicePartitionClient** meghatározásához a végpont szolgáltatást, és a szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="aa190-114">Client code can use the **WcfCommunicationClientFactory** along with the **WcfCommunicationClient** which implements **ServicePartitionClient** to determine the service endpoint and communicate with the service.</span></span>

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
> [!NOTE]
> <span data-ttu-id="aa190-115">Az alapértelmezett ServicePartitionResolver feltételezi, hogy az ügyfél fut a fürtön, amelyen a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="aa190-115">The default ServicePartitionResolver assumes that the client is running in same cluster as the service.</span></span> <span data-ttu-id="aa190-116">Ha ez nem ez a helyzet, ServicePartitionResolver objektum létrehozása, és a fürt kapcsolati végpontok adjon át.</span><span class="sxs-lookup"><span data-stu-id="aa190-116">If that is not the case, create a ServicePartitionResolver object and pass in the cluster connection endpoints.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="aa190-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa190-117">Next steps</span></span>
* [<span data-ttu-id="aa190-118">A távoli eljáráshívás a Reliable Services távoli eljáráshívás</span><span class="sxs-lookup"><span data-stu-id="aa190-118">Remote procedure call with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="aa190-119">Webes API-t a Reliable Services OWIN</span><span class="sxs-lookup"><span data-stu-id="aa190-119">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="aa190-120">A Reliable Services kommunikáció biztonságához</span><span class="sxs-lookup"><span data-stu-id="aa190-120">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)

