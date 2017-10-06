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
# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF-alapú kommunikációs verem a Reliable Services
lehetővé teszi a keretrendszer hello Reliable Services szolgáltatás szerzők toochoose hello kommunikációs verem toouse szeretnék a szolgáltatás számára. Azok az általuk választott keresztül hello hello kommunikációs verem csatlakoztatható **ICommunicationListener** hello által visszaadott [CreateServiceReplicaListeners vagy CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) módszerek. hello keretrendszer által biztosított szolgáltatás szerzők számára, akik toouse WCF-alapú kommunikációt hello Windows Communication Foundation (WCF) alapuló hello kommunikációs verem megvalósítása.

## <a name="wcf-communication-listener"></a>WCF kommunikációs figyelő
WCF-specifikus hello végrehajtásának **ICommunicationListener** hello által biztosított **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** osztály.

Közpénzek ne mondja ki a szolgáltatási szerződés típusú tudunk.`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

A következő módon hello szolgáltatás hello azt hozhat létre egy WCF-kommunikáció figyelő.

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

## <a name="writing-clients-for-hello-wcf-communication-stack"></a>Ügyfelek hello WCF kommunikációs verem írása
WCF, hello keretrendszer használatával szolgáltatásokkal toocommunicate biztosít az ügyfelek írásra **WcfClientCommunicationFactory**, ez a WCF-specifikus hello végrehajtásának [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

hello WCF kommunikációs csatorna számára elérhető hello **WcfCommunicationClient** hello által létrehozott **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Ügyfélkód használható hello **WcfCommunicationClientFactory** együtt hello **WcfCommunicationClient** mely valósít meg **ServicePartitionClient** toodetermine Szolgáltatásvégpont hello és hello szolgáltatásra.

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
> hello alapértelmezett ServicePartitionResolver azt feltételezi, hogy hello ügyfél futtatja a fürtön, amelyen hello szolgáltatást. Ha, amely nem hello esetben ServicePartitionResolver objektum létrehozása, és hello fürt kapcsolati végpontok adjon át.
> 
> 

## <a name="next-steps"></a>Következő lépések
* [A távoli eljáráshívás a Reliable Services távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md)
* [Webes API-t a Reliable Services OWIN](service-fabric-reliable-services-communication-webapi.md)
* [A Reliable Services kommunikáció biztonságához](service-fabric-reliable-services-secure-communication.md)

