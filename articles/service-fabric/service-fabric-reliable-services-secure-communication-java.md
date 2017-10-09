---
title: "aaaHelp biztonságos kommunikációs szolgáltatások az Azure Service Fabric |} Microsoft Docs"
description: "Hogyan toohelp biztonságos kommunikáció megbízható számára, amely áttekintést az Azure Service Fabric-fürt futnak."
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
ms.openlocfilehash: 14db54d50c35478c1f2c156de0dba36f1427c8cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a><span data-ttu-id="dd34c-103">Azure Service Fabric-szolgáltatások biztonságos kommunikáció érdekében</span><span class="sxs-lookup"><span data-stu-id="dd34c-103">Help secure communication for services in Azure Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dd34c-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="dd34c-104">C# on Windows</span></span>](service-fabric-reliable-services-secure-communication.md)
> * [<span data-ttu-id="dd34c-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="dd34c-105">Java on Linux</span></span>](service-fabric-reliable-services-secure-communication-java.md)
>
>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a><span data-ttu-id="dd34c-106">Számítógépek biztonságossá tétele a szolgáltatás használatakor a távelérési szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="dd34c-106">Help secure a service when you're using service remoting</span></span>
<span data-ttu-id="dd34c-107">Fogjuk használni egy meglévő [példa](service-fabric-reliable-services-communication-remoting-java.md) , amely ismerteti, hogyan tooset megbízható szolgáltatások távoli eljáráshívást fel.</span><span class="sxs-lookup"><span data-stu-id="dd34c-107">We'll be using an existing [example](service-fabric-reliable-services-communication-remoting-java.md) that explains how tooset up remoting for reliable services.</span></span> <span data-ttu-id="dd34c-108">toohelp biztonságos egy szolgáltatás, a távelérés szolgáltatás használatakor, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dd34c-108">toohelp secure a service when you're using service remoting, follow these steps:</span></span>

1. <span data-ttu-id="dd34c-109">Hozzon létre egy felület `HelloWorldStateless`, meghatározó hello módszereket, amelyek számára a szolgáltatás a távoli eljáráshívás használható.</span><span class="sxs-lookup"><span data-stu-id="dd34c-109">Create an interface, `HelloWorldStateless`, that defines hello methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="dd34c-110">A szolgáltatás által használt `FabricTransportServiceRemotingListener`, amellyel deklarálva van a hello `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` csomag.</span><span class="sxs-lookup"><span data-stu-id="dd34c-110">Your service will use `FabricTransportServiceRemotingListener`, which is declared in hello `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` package.</span></span> <span data-ttu-id="dd34c-111">Ez egy `CommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="dd34c-111">This is an `CommunicationListener` implementation that provides remoting capabilities.</span></span>

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
2. <span data-ttu-id="dd34c-112">Adja hozzá a figyelő beállításai és a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="dd34c-112">Add listener settings and security credentials.</span></span>

    <span data-ttu-id="dd34c-113">Győződjön meg arról, hogy szeretné-e toouse toohelp biztonságos hello fürt összes csomópontjának hello telepítve van a szolgáltatások közötti kommunikáció hello tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="dd34c-113">Make sure that hello certificate that you want toouse toohelp secure your service communication is installed on all hello nodes in hello cluster.</span></span> <span data-ttu-id="dd34c-114">Két módon is megadható figyelő beállításai és a hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="dd34c-114">There are two ways that you can provide listener settings and security credentials:</span></span>

   1. <span data-ttu-id="dd34c-115">Adja meg azokat a egy [a konfigurációs csomag](service-fabric-application-model.md):</span><span class="sxs-lookup"><span data-stu-id="dd34c-115">Provide them by using a [config package](service-fabric-application-model.md):</span></span>

       <span data-ttu-id="dd34c-116">Adja hozzá a `TransportSettings` szakasz hello settings.xml fájlban.</span><span class="sxs-lookup"><span data-stu-id="dd34c-116">Add a `TransportSettings` section in hello settings.xml file.</span></span>

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

       <span data-ttu-id="dd34c-117">Ebben az esetben hello `createServiceInstanceListeners` módszert fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="dd34c-117">In this case, hello `createServiceInstanceListeners` method will look like this:</span></span>

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        <span data-ttu-id="dd34c-118">Ha ad hozzá egy `TransportSettings` hello settings.xml fájlban bármely előtag nélkül szakasz `FabricTransportListenerSettings` fog betöltődni ebben a szakaszban ismertetett összes hello beállítás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="dd34c-118">If you add a `TransportSettings` section in hello settings.xml file without any prefix, `FabricTransportListenerSettings` will load all hello settings from this section by default.</span></span>

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        <span data-ttu-id="dd34c-119">Ebben az esetben hello `CreateServiceInstanceListeners` módszert fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="dd34c-119">In this case, hello `CreateServiceInstanceListeners` method will look like this:</span></span>

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. <span data-ttu-id="dd34c-120">Hívható módszerek egy védett szolgáltatásra hello használata helyett hello távoli eljáráshívási verem, használatával `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` osztály toocreate egy szolgáltatási proxy használata `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="dd34c-120">When you call methods on a secured service by using hello remoting stack, instead of using hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` class toocreate a service proxy, use `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.</span></span>

    <span data-ttu-id="dd34c-121">Ha hello Ügyfélkód egy szolgáltatás részeként fut, betöltheti `FabricTransportSettings` hello settings.xml fájlból.</span><span class="sxs-lookup"><span data-stu-id="dd34c-121">If hello client code is running as part of a service, you can load `FabricTransportSettings` from hello settings.xml file.</span></span> <span data-ttu-id="dd34c-122">Hozzon létre hasonló toohello szolgáltatás kód TransportSettings szakasz korábbi látható módon.</span><span class="sxs-lookup"><span data-stu-id="dd34c-122">Create a TransportSettings section that is similar toohello service code, as shown earlier.</span></span> <span data-ttu-id="dd34c-123">Hajtsa végre a következő módosításokat toohello Ügyfélkód hello:</span><span class="sxs-lookup"><span data-stu-id="dd34c-123">Make hello following changes toohello client code:</span></span>

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
