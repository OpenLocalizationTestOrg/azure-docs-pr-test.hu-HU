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
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a><span data-ttu-id="4f11e-103">Azure Service Fabric-szolgáltatások biztonságos kommunikáció érdekében</span><span class="sxs-lookup"><span data-stu-id="4f11e-103">Help secure communication for services in Azure Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4f11e-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="4f11e-104">C# on Windows</span></span>](service-fabric-reliable-services-secure-communication.md)
> * [<span data-ttu-id="4f11e-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="4f11e-105">Java on Linux</span></span>](service-fabric-reliable-services-secure-communication-java.md)
>
>

<span data-ttu-id="4f11e-106">A biztonság az egyik legfontosabb szempontja hello kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="4f11e-106">Security is one of hello most important aspects of communication.</span></span> <span data-ttu-id="4f11e-107">hello Reliable Services alkalmazás-keretrendszer tartalmaz néhány előre elkészített kommunikációs verem és tooimprove biztonsági használt eszközök.</span><span class="sxs-lookup"><span data-stu-id="4f11e-107">hello Reliable Services application framework provides a few prebuilt communication stacks and tools that you can use tooimprove security.</span></span> <span data-ttu-id="4f11e-108">Ez a cikk arról, hogyan tooimprove biztonsági használatakor távoli eljáráshívási szolgáltatás és a Windows Communication Foundation (WCF) kommunikációs verem hello-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="4f11e-108">This article talks about how tooimprove security when you're using service remoting and hello Windows Communication Foundation (WCF) communication stack.</span></span>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a><span data-ttu-id="4f11e-109">Számítógépek biztonságossá tétele a szolgáltatás használatakor a távelérési szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4f11e-109">Help secure a service when you're using service remoting</span></span>
<span data-ttu-id="4f11e-110">Használunk, egy meglévő [példa](service-fabric-reliable-services-communication-remoting.md) , amely ismerteti, hogyan tooset megbízható szolgáltatások távoli eljáráshívást fel.</span><span class="sxs-lookup"><span data-stu-id="4f11e-110">We are using an existing [example](service-fabric-reliable-services-communication-remoting.md) that explains how tooset up remoting for reliable services.</span></span> <span data-ttu-id="4f11e-111">toohelp biztonságos egy szolgáltatás, a távelérés szolgáltatás használatakor, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4f11e-111">toohelp secure a service when you're using service remoting, follow these steps:</span></span>

1. <span data-ttu-id="4f11e-112">Hozzon létre egy felület `IHelloWorldStateful`, meghatározó hello módszereket, amelyek számára a szolgáltatás a távoli eljáráshívás használható.</span><span class="sxs-lookup"><span data-stu-id="4f11e-112">Create an interface, `IHelloWorldStateful`, that defines hello methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="4f11e-113">A szolgáltatás által használt `FabricTransportServiceRemotingListener`, amellyel deklarálva van a hello `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` névtér.</span><span class="sxs-lookup"><span data-stu-id="4f11e-113">Your service will use `FabricTransportServiceRemotingListener`, which is declared in hello `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` namespace.</span></span> <span data-ttu-id="4f11e-114">Ez egy `ICommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="4f11e-114">This is an `ICommunicationListener` implementation that provides remoting capabilities.</span></span>

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
2. <span data-ttu-id="4f11e-115">Adja hozzá a figyelő beállításai és a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f11e-115">Add listener settings and security credentials.</span></span>

    <span data-ttu-id="4f11e-116">Győződjön meg arról, hogy szeretné-e toouse toohelp biztonságos hello fürt összes csomópontjának hello telepítve van a szolgáltatások közötti kommunikáció hello tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="4f11e-116">Make sure that hello certificate that you want toouse toohelp secure your service communication is installed on all hello nodes in hello cluster.</span></span> <span data-ttu-id="4f11e-117">Két módon is megadható figyelő beállításai és a hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="4f11e-117">There are two ways that you can provide listener settings and security credentials:</span></span>

   1. <span data-ttu-id="4f11e-118">Adja meg azokat közvetlenül a hello szolgáltatás kódot:</span><span class="sxs-lookup"><span data-stu-id="4f11e-118">Provide them directly in hello service code:</span></span>

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
   2. <span data-ttu-id="4f11e-119">Adja meg azokat a egy [a konfigurációs csomag](service-fabric-application-model.md):</span><span class="sxs-lookup"><span data-stu-id="4f11e-119">Provide them by using a [config package](service-fabric-application-model.md):</span></span>

       <span data-ttu-id="4f11e-120">Adja hozzá a `TransportSettings` szakasz hello settings.xml fájlban.</span><span class="sxs-lookup"><span data-stu-id="4f11e-120">Add a `TransportSettings` section in hello settings.xml file.</span></span>

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

       <span data-ttu-id="4f11e-121">Ebben az esetben hello `CreateServiceReplicaListeners` módszert fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="4f11e-121">In this case, hello `CreateServiceReplicaListeners` method will look like this:</span></span>

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

        <span data-ttu-id="4f11e-122">Ha ad hozzá egy `TransportSettings` hello settings.xml fájlban szakasz `FabricTransportRemotingListenerSettings ` fog betöltődni ebben a szakaszban ismertetett összes hello beállítás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="4f11e-122">If you add a `TransportSettings` section in hello settings.xml file , `FabricTransportRemotingListenerSettings ` will load all hello settings from this section by default.</span></span>

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        <span data-ttu-id="4f11e-123">Ebben az esetben hello `CreateServiceReplicaListeners` módszert fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="4f11e-123">In this case, hello `CreateServiceReplicaListeners` method will look like this:</span></span>

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
3. <span data-ttu-id="4f11e-124">Hívható módszerek egy védett szolgáltatásra hello használata helyett hello távoli eljáráshívási verem, használatával `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` osztály toocreate egy szolgáltatási proxy használata `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="4f11e-124">When you call methods on a secured service by using hello remoting stack, instead of using hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` class toocreate a service proxy, use `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`.</span></span> <span data-ttu-id="4f11e-125">Adjon át `FabricTransportRemotingSettings`, amely tartalmazza `SecurityCredentials`.</span><span class="sxs-lookup"><span data-stu-id="4f11e-125">Pass in `FabricTransportRemotingSettings`, which contains `SecurityCredentials`.</span></span>

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

    <span data-ttu-id="4f11e-126">Ha hello Ügyfélkód egy szolgáltatás részeként fut, betöltheti `FabricTransportRemotingSettings` hello settings.xml fájlból.</span><span class="sxs-lookup"><span data-stu-id="4f11e-126">If hello client code is running as part of a service, you can load `FabricTransportRemotingSettings` from hello settings.xml file.</span></span> <span data-ttu-id="4f11e-127">Hozzon létre hasonló toohello szolgáltatás kód HelloWorldClientTransportSettings szakasz korábbi látható módon.</span><span class="sxs-lookup"><span data-stu-id="4f11e-127">Create a HelloWorldClientTransportSettings section that is similar toohello service code, as shown earlier.</span></span> <span data-ttu-id="4f11e-128">Hajtsa végre a következő módosításokat toohello Ügyfélkód hello:</span><span class="sxs-lookup"><span data-stu-id="4f11e-128">Make hello following changes toohello client code:</span></span>

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    <span data-ttu-id="4f11e-129">Hello ügyfél nem fut a szolgáltatás részeként, ha egy client_name.settings.xml fájlt létrehozhatja a hello azonos helyet, ahol hello client_name.exe.</span><span class="sxs-lookup"><span data-stu-id="4f11e-129">If hello client is not running as part of a service, you can create a client_name.settings.xml file in hello same location where hello client_name.exe is.</span></span> <span data-ttu-id="4f11e-130">Ezután hozzon létre egy TransportSettings szakaszt, az adott fájlban.</span><span class="sxs-lookup"><span data-stu-id="4f11e-130">Then create a TransportSettings section in that file.</span></span>

    <span data-ttu-id="4f11e-131">Hasonló toohello szolgáltatást, ha ad hozzá egy `TransportSettings` ügyfél settings.xml/client_name.settings.xml szakasz `FabricTransportRemotingSettings` ebben a szakaszban ismertetett összes hello-beállítások alapértelmezés szerint tölti be.</span><span class="sxs-lookup"><span data-stu-id="4f11e-131">Similar toohello service, if you add a `TransportSettings` section in client settings.xml/client_name.settings.xml, `FabricTransportRemotingSettings` loads all hello settings from this section by default.</span></span>

    <span data-ttu-id="4f11e-132">Ebben az esetben hello korábbi kód még tovább egyszerűsített:</span><span class="sxs-lookup"><span data-stu-id="4f11e-132">In that case, hello earlier code is even further simplified:</span></span>  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a><span data-ttu-id="4f11e-133">Számítógépek biztonságossá tétele a szolgáltatás egy WCF-alapú kommunikációs verem használatakor</span><span class="sxs-lookup"><span data-stu-id="4f11e-133">Help secure a service when you're using a WCF-based communication stack</span></span>
<span data-ttu-id="4f11e-134">Egy meglévő használjuk [példa](service-fabric-reliable-services-communication-wcf.md) , amely azt ismerteti, hogyan tooset egy WCF-alapú kommunikáció verem megbízható szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="4f11e-134">We are using an existing [example](service-fabric-reliable-services-communication-wcf.md) that explains how tooset up a WCF-based communication stack for reliable services.</span></span> <span data-ttu-id="4f11e-135">toohelp biztonságos egy szolgáltatás, amikor egy WCF-alapú kommunikációs verem használja, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4f11e-135">toohelp secure a service when you're using a WCF-based communication stack, follow these steps:</span></span>

1. <span data-ttu-id="4f11e-136">Hello szolgáltatás toohelp biztonságos hello WCF kommunikációs figyelő szüksége (`WcfCommunicationListener`) az Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4f11e-136">For hello service, you need toohelp secure hello WCF communication listener (`WcfCommunicationListener`) that you create.</span></span> <span data-ttu-id="4f11e-137">toodo, módosítsa a hello `CreateServiceReplicaListeners` metódust.</span><span class="sxs-lookup"><span data-stu-id="4f11e-137">toodo this, modify hello `CreateServiceReplicaListeners` method.</span></span>

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
2. <span data-ttu-id="4f11e-138">Hello ügyfél hello `WcfCommunicationClient` osztály a létrehozott hello előző [példa](service-fabric-reliable-services-communication-wcf.md) változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="4f11e-138">In hello client, hello `WcfCommunicationClient` class that was created in hello previous [example](service-fabric-reliable-services-communication-wcf.md) remains unchanged.</span></span> <span data-ttu-id="4f11e-139">De csak akkor toooverride hello `CreateClientAsync` metódusában `WcfCommunicationClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="4f11e-139">But you need toooverride hello `CreateClientAsync` method of `WcfCommunicationClientFactory`:</span></span>

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

    <span data-ttu-id="4f11e-140">Használjon `SecureWcfCommunicationClientFactory` toocreate kommunikációs WCF-ügyfél (`WcfCommunicationClient`).</span><span class="sxs-lookup"><span data-stu-id="4f11e-140">Use `SecureWcfCommunicationClientFactory` toocreate a WCF communication client (`WcfCommunicationClient`).</span></span> <span data-ttu-id="4f11e-141">Módszerekkel hello ügyfél tooinvoke szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4f11e-141">Use hello client tooinvoke service methods.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="4f11e-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f11e-142">Next steps</span></span>
* [<span data-ttu-id="4f11e-143">Webes API-t a Reliable Services OWIN</span><span class="sxs-lookup"><span data-stu-id="4f11e-143">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
