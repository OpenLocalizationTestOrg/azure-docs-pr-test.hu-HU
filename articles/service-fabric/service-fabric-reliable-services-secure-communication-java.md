---
title: "Azure Service Fabric-szolgáltatások biztonságos kommunikáció érdekében |} Microsoft Docs"
description: "Azure Service Fabric-fürtben lévő futó megbízható szolgáltatások biztonságos kommunikáció érdekében áttekintése."
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: c4634e3d8efb1745fffcfe3e647e43d867038716
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a><span data-ttu-id="cd5c8-103">Azure Service Fabric-szolgáltatások biztonságos kommunikáció érdekében</span><span class="sxs-lookup"><span data-stu-id="cd5c8-103">Help secure communication for services in Azure Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cd5c8-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="cd5c8-104">C# on Windows</span></span>](service-fabric-reliable-services-secure-communication.md)
> * [<span data-ttu-id="cd5c8-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="cd5c8-105">Java on Linux</span></span>](service-fabric-reliable-services-secure-communication-java.md)
>
>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a><span data-ttu-id="cd5c8-106">Számítógépek biztonságossá tétele a szolgáltatás használatakor a távelérési szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cd5c8-106">Help secure a service when you're using service remoting</span></span>
<span data-ttu-id="cd5c8-107">Fogjuk használni egy meglévő [példa](service-fabric-reliable-services-communication-remoting-java.md) , amely ismerteti, hogyan megbízható szolgáltatások távoli eljáráshívást beállítani.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-107">We'll be using an existing [example](service-fabric-reliable-services-communication-remoting-java.md) that explains how to set up remoting for reliable services.</span></span> <span data-ttu-id="cd5c8-108">Számítógépek biztonságossá tétele a szolgáltatás használatakor a távelérési szolgáltatás, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cd5c8-108">To help secure a service when you're using service remoting, follow these steps:</span></span>

1. <span data-ttu-id="cd5c8-109">Illesztőfelület, hozzon létre `HelloWorldStateless`, amely meghatározza, hogy a módszereket, amelyek számára a szolgáltatás a távoli eljáráshívás használható.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-109">Create an interface, `HelloWorldStateless`, that defines the methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="cd5c8-110">A szolgáltatás által használt `FabricTransportServiceRemotingListener`, amely deklarálva van a `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` csomag.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-110">Your service will use `FabricTransportServiceRemotingListener`, which is declared in the `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` package.</span></span> <span data-ttu-id="cd5c8-111">Ez egy `CommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-111">This is an `CommunicationListener` implementation that provides remoting capabilities.</span></span>

    ```java
    public interface HelloWorldStateless extends Service {
        CompletableFuture<String> getHelloWorld();
    }

    class HelloWorldStatelessImpl extends StatelessService implements HelloWorldStateless {
        @Override
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
        return listeners;
        }

        public CompletableFuture<String> getHelloWorld() {
            return CompletableFuture.completedFuture("Hello World!");
        }
    }
    ```
2. <span data-ttu-id="cd5c8-112">Adja hozzá a figyelő beállításai és a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-112">Add listener settings and security credentials.</span></span>

    <span data-ttu-id="cd5c8-113">Győződjön meg arról, hogy a szolgáltatások közötti kommunikáció biztonságossá tételéhez használni kívánt tanúsítvány telepítve van-e a fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-113">Make sure that the certificate that you want to use to help secure your service communication is installed on all the nodes in the cluster.</span></span> <span data-ttu-id="cd5c8-114">Két módon is megadható figyelő beállításai és a hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="cd5c8-114">There are two ways that you can provide listener settings and security credentials:</span></span>

   1. <span data-ttu-id="cd5c8-115">Adja meg azokat a egy [a konfigurációs csomag](service-fabric-application-model.md):</span><span class="sxs-lookup"><span data-stu-id="cd5c8-115">Provide them by using a [config package](service-fabric-application-model.md):</span></span>

       <span data-ttu-id="cd5c8-116">Adja hozzá a `TransportSettings` szakasz a settings.xml fájlban.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-116">Add a `TransportSettings` section in the settings.xml file.</span></span>

       ```xml
       <!--Section name should always end with "TransportSettings".-->
       <!--Here we are using a prefix "HelloWorldStateless".-->
        <Section Name="HelloWorldStatelessTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509_2" />
            <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
            <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
        </Section>

       ```

       <span data-ttu-id="cd5c8-117">Ebben az esetben a `createServiceInstanceListeners` módszert fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="cd5c8-117">In this case, the `createServiceInstanceListeners` method will look like this:</span></span>

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        <span data-ttu-id="cd5c8-118">Ha ad hozzá egy `TransportSettings` bármely előtag nélkül a settings.xml fájlban szakasz `FabricTransportListenerSettings` betölti alapértelmezés szerint ez a szakasz az összes beállítás.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-118">If you add a `TransportSettings` section in the settings.xml file without any prefix, `FabricTransportListenerSettings` will load all the settings from this section by default.</span></span>

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        <span data-ttu-id="cd5c8-119">Ebben az esetben a `CreateServiceInstanceListeners` módszert fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="cd5c8-119">In this case, the `CreateServiceInstanceListeners` method will look like this:</span></span>

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. <span data-ttu-id="cd5c8-120">Hívható módszerek biztonságos szolgáltatás használata helyett a távoli eljáráshívás verem használatával a `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` a szolgáltatásproxy létrehozására, használja az osztály `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-120">When you call methods on a secured service by using the remoting stack, instead of using the `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` class to create a service proxy, use `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.</span></span>

    <span data-ttu-id="cd5c8-121">Ha az Ügyfélkód egy szolgáltatás részeként fut, betöltheti `FabricTransportSettings` a settings.xml fájlból.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-121">If the client code is running as part of a service, you can load `FabricTransportSettings` from the settings.xml file.</span></span> <span data-ttu-id="cd5c8-122">Hozzon létre egy TransportSettings szakaszt, amelyek hasonlóak a szolgáltatáskód hibáit, amint azt korábban.</span><span class="sxs-lookup"><span data-stu-id="cd5c8-122">Create a TransportSettings section that is similar to the service code, as shown earlier.</span></span> <span data-ttu-id="cd5c8-123">A következő módosításokat az ügyfél kód:</span><span class="sxs-lookup"><span data-stu-id="cd5c8-123">Make the following changes to the client code:</span></span>

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
