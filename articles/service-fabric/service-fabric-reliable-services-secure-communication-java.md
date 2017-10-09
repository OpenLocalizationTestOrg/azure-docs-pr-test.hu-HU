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
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Azure Service Fabric-szolgáltatások biztonságos kommunikáció érdekében
> [!div class="op_single_selector"]
> * [C# Windowson](service-fabric-reliable-services-secure-communication.md)
> * [Java Linuxon](service-fabric-reliable-services-secure-communication-java.md)
>
>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Számítógépek biztonságossá tétele a szolgáltatás használatakor a távelérési szolgáltatás
Fogjuk használni egy meglévő [példa](service-fabric-reliable-services-communication-remoting-java.md) , amely ismerteti, hogyan tooset megbízható szolgáltatások távoli eljáráshívást fel. toohelp biztonságos egy szolgáltatás, a távelérés szolgáltatás használatakor, kövesse az alábbi lépéseket:

1. Hozzon létre egy felület `HelloWorldStateless`, meghatározó hello módszereket, amelyek számára a szolgáltatás a távoli eljáráshívás használható. A szolgáltatás által használt `FabricTransportServiceRemotingListener`, amellyel deklarálva van a hello `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` csomag. Ez egy `CommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít.

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
2. Adja hozzá a figyelő beállításai és a hitelesítő adatokat.

    Győződjön meg arról, hogy szeretné-e toouse toohelp biztonságos hello fürt összes csomópontjának hello telepítve van a szolgáltatások közötti kommunikáció hello tanúsítvány. Két módon is megadható figyelő beállításai és a hitelesítő adatokat:

   1. Adja meg azokat a egy [a konfigurációs csomag](service-fabric-application-model.md):

       Adja hozzá a `TransportSettings` szakasz hello settings.xml fájlban.

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

       Ebben az esetben hello `createServiceInstanceListeners` módszert fog kinézni:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        Ha ad hozzá egy `TransportSettings` hello settings.xml fájlban bármely előtag nélkül szakasz `FabricTransportListenerSettings` fog betöltődni ebben a szakaszban ismertetett összes hello beállítás alapértelmezés szerint.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Ebben az esetben hello `CreateServiceInstanceListeners` módszert fog kinézni:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Hívható módszerek egy védett szolgáltatásra hello használata helyett hello távoli eljáráshívási verem, használatával `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` osztály toocreate egy szolgáltatási proxy használata `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.

    Ha hello Ügyfélkód egy szolgáltatás részeként fut, betöltheti `FabricTransportSettings` hello settings.xml fájlból. Hozzon létre hasonló toohello szolgáltatás kód TransportSettings szakasz korábbi látható módon. Hajtsa végre a következő módosításokat toohello Ügyfélkód hello:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
