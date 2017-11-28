---
title: "Azure Service Fabric-szolgáltatások biztonságos kommunikáció érdekében |} Microsoft Docs"
description: "Azure Service Fabric-fürtben lévő futó megbízható szolgáltatások biztonságos kommunikáció érdekében áttekintése."
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
ms.openlocfilehash: 53119244f8f09c0c6c43f43761af1cc074f8d0af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a><span data-ttu-id="f6e95-103">Azure Service Fabric-szolgáltatások biztonságos kommunikáció érdekében</span><span class="sxs-lookup"><span data-stu-id="f6e95-103">Help secure communication for services in Azure Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f6e95-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="f6e95-104">C# on Windows</span></span>](service-fabric-reliable-services-secure-communication.md)
> * [<span data-ttu-id="f6e95-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="f6e95-105">Java on Linux</span></span>](service-fabric-reliable-services-secure-communication-java.md)
>
>

<span data-ttu-id="f6e95-106">A biztonság az egyik legfontosabb szempontja a kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="f6e95-106">Security is one of the most important aspects of communication.</span></span> <span data-ttu-id="f6e95-107">A Reliable Services alkalmazás-keretrendszer tartalmaz néhány előre elkészített kommunikációs verem és eszközöket, amelyek a biztonság növelése érdekében használhatja.</span><span class="sxs-lookup"><span data-stu-id="f6e95-107">The Reliable Services application framework provides a few prebuilt communication stacks and tools that you can use to improve security.</span></span> <span data-ttu-id="f6e95-108">Ez a cikk beszél biztonsági javítására, ha a szolgáltatás távoli eljáráshívás és a Windows Communication Foundation (WCF) kommunikációs verem használata.</span><span class="sxs-lookup"><span data-stu-id="f6e95-108">This article talks about how to improve security when you're using service remoting and the Windows Communication Foundation (WCF) communication stack.</span></span>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a><span data-ttu-id="f6e95-109">Számítógépek biztonságossá tétele a szolgáltatás használatakor a távelérési szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f6e95-109">Help secure a service when you're using service remoting</span></span>
<span data-ttu-id="f6e95-110">Egy meglévő használjuk [példa](service-fabric-reliable-services-communication-remoting.md) , amely ismerteti, hogyan megbízható szolgáltatások távoli eljáráshívást beállítani.</span><span class="sxs-lookup"><span data-stu-id="f6e95-110">We are using an existing [example](service-fabric-reliable-services-communication-remoting.md) that explains how to set up remoting for reliable services.</span></span> <span data-ttu-id="f6e95-111">Számítógépek biztonságossá tétele a szolgáltatás használatakor a távelérési szolgáltatás, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f6e95-111">To help secure a service when you're using service remoting, follow these steps:</span></span>

1. <span data-ttu-id="f6e95-112">Illesztőfelület, hozzon létre `IHelloWorldStateful`, amely meghatározza, hogy a módszereket, amelyek számára a szolgáltatás a távoli eljáráshívás használható.</span><span class="sxs-lookup"><span data-stu-id="f6e95-112">Create an interface, `IHelloWorldStateful`, that defines the methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="f6e95-113">A szolgáltatás által használt `FabricTransportServiceRemotingListener`, amely deklarálva van a `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` névtér.</span><span class="sxs-lookup"><span data-stu-id="f6e95-113">Your service will use `FabricTransportServiceRemotingListener`, which is declared in the `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` namespace.</span></span> <span data-ttu-id="f6e95-114">Ez egy `ICommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="f6e95-114">This is an `ICommunicationListener` implementation that provides remoting capabilities.</span></span>

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
2. <span data-ttu-id="f6e95-115">Adja hozzá a figyelő beállításai és a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="f6e95-115">Add listener settings and security credentials.</span></span>

    <span data-ttu-id="f6e95-116">Győződjön meg arról, hogy a szolgáltatások közötti kommunikáció biztonságossá tételéhez használni kívánt tanúsítvány telepítve van-e a fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="f6e95-116">Make sure that the certificate that you want to use to help secure your service communication is installed on all the nodes in the cluster.</span></span> <span data-ttu-id="f6e95-117">Két módon is megadható figyelő beállításai és a hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="f6e95-117">There are two ways that you can provide listener settings and security credentials:</span></span>

   1. <span data-ttu-id="f6e95-118">Adja meg azokat közvetlenül a szolgáltatás-kódban:</span><span class="sxs-lookup"><span data-stu-id="f6e95-118">Provide them directly in the service code:</span></span>

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
   2. <span data-ttu-id="f6e95-119">Adja meg azokat a egy [a konfigurációs csomag](service-fabric-application-model.md):</span><span class="sxs-lookup"><span data-stu-id="f6e95-119">Provide them by using a [config package](service-fabric-application-model.md):</span></span>

       <span data-ttu-id="f6e95-120">Adja hozzá a `TransportSettings` szakasz a settings.xml fájlban.</span><span class="sxs-lookup"><span data-stu-id="f6e95-120">Add a `TransportSettings` section in the settings.xml file.</span></span>

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

       <span data-ttu-id="f6e95-121">Ebben az esetben a `CreateServiceReplicaListeners` módszert fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="f6e95-121">In this case, the `CreateServiceReplicaListeners` method will look like this:</span></span>

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

        <span data-ttu-id="f6e95-122">Ha ad hozzá egy `TransportSettings` szakasz a settings.xml fájlban `FabricTransportRemotingListenerSettings ` betölti alapértelmezés szerint ez a szakasz az összes beállítás.</span><span class="sxs-lookup"><span data-stu-id="f6e95-122">If you add a `TransportSettings` section in the settings.xml file , `FabricTransportRemotingListenerSettings ` will load all the settings from this section by default.</span></span>

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        <span data-ttu-id="f6e95-123">Ebben az esetben a `CreateServiceReplicaListeners` módszert fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="f6e95-123">In this case, the `CreateServiceReplicaListeners` method will look like this:</span></span>

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
3. <span data-ttu-id="f6e95-124">Hívható módszerek biztonságos szolgáltatás használata helyett a távoli eljáráshívás verem használatával a `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` a szolgáltatásproxy létrehozására, használja az osztály `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="f6e95-124">When you call methods on a secured service by using the remoting stack, instead of using the `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` class to create a service proxy, use `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`.</span></span> <span data-ttu-id="f6e95-125">Adjon át `FabricTransportRemotingSettings`, amely tartalmazza `SecurityCredentials`.</span><span class="sxs-lookup"><span data-stu-id="f6e95-125">Pass in `FabricTransportRemotingSettings`, which contains `SecurityCredentials`.</span></span>

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

    <span data-ttu-id="f6e95-126">Ha az Ügyfélkód egy szolgáltatás részeként fut, betöltheti `FabricTransportRemotingSettings` a settings.xml fájlból.</span><span class="sxs-lookup"><span data-stu-id="f6e95-126">If the client code is running as part of a service, you can load `FabricTransportRemotingSettings` from the settings.xml file.</span></span> <span data-ttu-id="f6e95-127">Hozzon létre egy HelloWorldClientTransportSettings szakaszt, amelyek hasonlóak a szolgáltatáskód hibáit, amint azt korábban.</span><span class="sxs-lookup"><span data-stu-id="f6e95-127">Create a HelloWorldClientTransportSettings section that is similar to the service code, as shown earlier.</span></span> <span data-ttu-id="f6e95-128">A következő módosításokat az ügyfél kód:</span><span class="sxs-lookup"><span data-stu-id="f6e95-128">Make the following changes to the client code:</span></span>

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    <span data-ttu-id="f6e95-129">Ha az ügyfél nem fut a szolgáltatás részeként, ugyanazon a helyen, ahol a client_name.exe az client_name.settings.xml fájlt is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="f6e95-129">If the client is not running as part of a service, you can create a client_name.settings.xml file in the same location where the client_name.exe is.</span></span> <span data-ttu-id="f6e95-130">Ezután hozzon létre egy TransportSettings szakaszt, az adott fájlban.</span><span class="sxs-lookup"><span data-stu-id="f6e95-130">Then create a TransportSettings section in that file.</span></span>

    <span data-ttu-id="f6e95-131">Hasonló a szolgáltatáshoz való hozzáadásakor a `TransportSettings` ügyfél settings.xml/client_name.settings.xml szakasz `FabricTransportRemotingSettings` tölt be az összes beállítás alapértelmezés szerint ebben a szakaszban ismertetett.</span><span class="sxs-lookup"><span data-stu-id="f6e95-131">Similar to the service, if you add a `TransportSettings` section in client settings.xml/client_name.settings.xml, `FabricTransportRemotingSettings` loads all the settings from this section by default.</span></span>

    <span data-ttu-id="f6e95-132">Ebben az esetben a korábbi kód még tovább egyszerűsített:</span><span class="sxs-lookup"><span data-stu-id="f6e95-132">In that case, the earlier code is even further simplified:</span></span>  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a><span data-ttu-id="f6e95-133">Számítógépek biztonságossá tétele a szolgáltatás egy WCF-alapú kommunikációs verem használatakor</span><span class="sxs-lookup"><span data-stu-id="f6e95-133">Help secure a service when you're using a WCF-based communication stack</span></span>
<span data-ttu-id="f6e95-134">Egy meglévő használjuk [példa](service-fabric-reliable-services-communication-wcf.md) , amely elmagyarázza, hogyan állíthat be egy WCF-alapú kommunikációs verem megbízható szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f6e95-134">We are using an existing [example](service-fabric-reliable-services-communication-wcf.md) that explains how to set up a WCF-based communication stack for reliable services.</span></span> <span data-ttu-id="f6e95-135">Számítógépek biztonságossá tétele a szolgáltatás egy WCF-alapú kommunikációs verem használatakor, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f6e95-135">To help secure a service when you're using a WCF-based communication stack, follow these steps:</span></span>

1. <span data-ttu-id="f6e95-136">A szolgáltatás kell biztonságossá tétele a WCF-kommunikáció figyelő (`WcfCommunicationListener`) az Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f6e95-136">For the service, you need to help secure the WCF communication listener (`WcfCommunicationListener`) that you create.</span></span> <span data-ttu-id="f6e95-137">Ehhez az szükséges, módosítsa a `CreateServiceReplicaListeners` metódust.</span><span class="sxs-lookup"><span data-stu-id="f6e95-137">To do this, modify the `CreateServiceReplicaListeners` method.</span></span>

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

        // Add certificate details in the ServiceHost credentials.
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
2. <span data-ttu-id="f6e95-138">Az ügyfél a `WcfCommunicationClient` osztály, amely jött létre az előző [példa](service-fabric-reliable-services-communication-wcf.md) változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="f6e95-138">In the client, the `WcfCommunicationClient` class that was created in the previous [example](service-fabric-reliable-services-communication-wcf.md) remains unchanged.</span></span> <span data-ttu-id="f6e95-139">Felül kell bírálni, de a `CreateClientAsync` metódusában `WcfCommunicationClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="f6e95-139">But you need to override the `CreateClientAsync` method of `WcfCommunicationClientFactory`:</span></span>

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
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
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

    <span data-ttu-id="f6e95-140">Használjon `SecureWcfCommunicationClientFactory` WCF kommunikációs ügyfelet létrehozni a (`WcfCommunicationClient`).</span><span class="sxs-lookup"><span data-stu-id="f6e95-140">Use `SecureWcfCommunicationClientFactory` to create a WCF communication client (`WcfCommunicationClient`).</span></span> <span data-ttu-id="f6e95-141">Az ügyfél segítségével szolgáltatás metódusok.</span><span class="sxs-lookup"><span data-stu-id="f6e95-141">Use the client to invoke service methods.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="f6e95-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6e95-142">Next steps</span></span>
* [<span data-ttu-id="f6e95-143">Webes API-t a Reliable Services OWIN</span><span class="sxs-lookup"><span data-stu-id="f6e95-143">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
