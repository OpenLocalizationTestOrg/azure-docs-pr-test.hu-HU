---
title: "aaaHelp biztonságos kommunikációs szolgáltatások az Azure Service Fabric |} Microsoft Docs"
description: "Hogyan toohelp biztonságos kommunikáció megbízható számára, amely áttekintést az Azure Service Fabric-fürt futnak."
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: vturecek
ms.assetid: fc129c1a-fbe4-4339-83ae-0e69a41654e0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: 15201eb503322b17db329b319f1f42860b0f0c6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Azure Service Fabric-szolgáltatások biztonságos kommunikáció érdekében
> [!div class="op_single_selector"]
> * [C# Windowson](service-fabric-reliable-services-secure-communication.md)
> * [Java Linuxon](service-fabric-reliable-services-secure-communication-java.md)
>
>

A biztonság az egyik legfontosabb szempontja hello kommunikációra. hello Reliable Services alkalmazás-keretrendszer tartalmaz néhány előre elkészített kommunikációs verem és tooimprove biztonsági használt eszközök. Ez a cikk arról, hogyan tooimprove biztonsági használatakor távoli eljáráshívási szolgáltatás és a Windows Communication Foundation (WCF) kommunikációs verem hello-kiszolgálóhoz.

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Számítógépek biztonságossá tétele a szolgáltatás használatakor a távelérési szolgáltatás
Használunk, egy meglévő [példa](service-fabric-reliable-services-communication-remoting.md) , amely ismerteti, hogyan tooset megbízható szolgáltatások távoli eljáráshívást fel. toohelp biztonságos egy szolgáltatás, a távelérés szolgáltatás használatakor, kövesse az alábbi lépéseket:

1. Hozzon létre egy felület `IHelloWorldStateful`, meghatározó hello módszereket, amelyek számára a szolgáltatás a távoli eljáráshívás használható. A szolgáltatás által használt `FabricTransportServiceRemotingListener`, amellyel deklarálva van a hello `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` névtér. Ez egy `ICommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```
2. Adja hozzá a figyelő beállításai és a hitelesítő adatokat.

    Győződjön meg arról, hogy szeretné-e toouse toohelp biztonságos hello fürt összes csomópontjának hello telepítve van a szolgáltatások közötti kommunikáció hello tanúsítvány. Két módon is megadható figyelő beállításai és a hitelesítő adatokat:

   1. Adja meg azokat közvetlenül a hello szolgáltatás kódot:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           FabricTransportRemotingListenerSettings  listenerSettings = new FabricTransportRemotingListenerSettings
           {
               MaxMessageSize = 10000000,
               SecurityCredentials = GetSecurityCredentials()
           };
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
           };
       }

       private static SecurityCredentials GetSecurityCredentials()
       {
           // Provide certificate details.
           var x509Credentials = new X509Credentials
           {
               FindType = X509FindType.FindByThumbprint,
               FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
               StoreLocation = StoreLocation.LocalMachine,
               StoreName = "My",
               ProtectionLevel = ProtectionLevel.EncryptAndSign
           };
           x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
           x509Credentials.RemoteCertThumbprints.Add("9FEF3950642138446CC364A396E1E881DB76B483");
           return x509Credentials;
       }
       ```
   2. Adja meg azokat a egy [a konfigurációs csomag](service-fabric-application-model.md):

       Adja hozzá a `TransportSettings` szakasz hello settings.xml fájlban.

       ```xml
       <Section Name="HelloWorldStatefulTransportSettings">
           <Parameter Name="MaxMessageSize" Value="10000000" />
           <Parameter Name="SecurityCredentialsType" Value="X509" />
           <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
           <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
           <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
           <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
           <Parameter Name="CertificateStoreName" Value="My" />
           <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
           <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
       </Section>
       ```

       Ebben az esetben hello `CreateServiceReplicaListeners` módszert fog kinézni:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(
                       context,this,FabricTransportRemotingListenerSettings .LoadFrom("HelloWorldStatefulTransportSettings")))
           };
       }
       ```

        Ha ad hozzá egy `TransportSettings` hello settings.xml fájlban szakasz `FabricTransportRemotingListenerSettings ` fog betöltődni ebben a szakaszban ismertetett összes hello beállítás alapértelmezés szerint.

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Ebben az esetben hello `CreateServiceReplicaListeners` módszert fog kinézni:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                return new[]{
                        new ServiceReplicaListener(
                            (context) => new FabricTransportServiceRemotingListener(context,this))};
            };
        }
        ```
3. Hívható módszerek egy védett szolgáltatásra hello használata helyett hello távoli eljáráshívási verem, használatával `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` osztály toocreate egy szolgáltatási proxy használata `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Adjon át `FabricTransportRemotingSettings`, amely tartalmazza `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "9FEF3950642138446CC364A396E1E881DB76B483",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
    x509Credentials.RemoteCertThumbprints.Add("4FEF3950642138446CC364A396E1E881DB76B48C");

    FabricTransportRemotingSettings transportSettings = new FabricTransportRemotingSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Ha hello Ügyfélkód egy szolgáltatás részeként fut, betöltheti `FabricTransportRemotingSettings` hello settings.xml fájlból. Hozzon létre hasonló toohello szolgáltatás kód HelloWorldClientTransportSettings szakasz korábbi látható módon. Hajtsa végre a következő módosításokat toohello Ügyfélkód hello:

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Hello ügyfél nem fut a szolgáltatás részeként, ha egy client_name.settings.xml fájlt létrehozhatja a hello azonos helyet, ahol hello client_name.exe. Ezután hozzon létre egy TransportSettings szakaszt, az adott fájlban.

    Hasonló toohello szolgáltatást, ha ad hozzá egy `TransportSettings` ügyfél settings.xml/client_name.settings.xml szakasz `FabricTransportRemotingSettings` ebben a szakaszban ismertetett összes hello-beállítások alapértelmezés szerint tölti be.

    Ebben az esetben hello korábbi kód még tovább egyszerűsített:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Számítógépek biztonságossá tétele a szolgáltatás egy WCF-alapú kommunikációs verem használatakor
Egy meglévő használjuk [példa](service-fabric-reliable-services-communication-wcf.md) , amely azt ismerteti, hogyan tooset egy WCF-alapú kommunikáció verem megbízható szolgáltatásokhoz. toohelp biztonságos egy szolgáltatás, amikor egy WCF-alapú kommunikációs verem használja, kövesse az alábbi lépéseket:

1. Hello szolgáltatás toohelp biztonságos hello WCF kommunikációs figyelő szüksége (`WcfCommunicationListener`) az Ön által létrehozott. toodo, módosítsa a hello `CreateServiceReplicaListeners` metódust.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in hello ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```
2. Hello ügyfél hello `WcfCommunicationClient` osztály a létrehozott hello előző [példa](service-fabric-reliable-services-communication-wcf.md) változatlan marad. De csak akkor toooverride hello `CreateClientAsync` metódusában `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details toohello ChannelFactory credentials.
            // These credentials will be used by hello clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Használjon `SecureWcfCommunicationClientFactory` toocreate kommunikációs WCF-ügyfél (`WcfCommunicationClient`). Módszerekkel hello ügyfél tooinvoke szolgáltatás.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Következő lépések
* [Webes API-t a Reliable Services OWIN](service-fabric-reliable-services-communication-webapi.md)
