---
title: "aaaConnect és az Azure Service Fabric szolgáltatásokkal kommunikálni |} Microsoft Docs"
description: "Ismerje meg, hogyan tooresolve, csatlakozzon, és a Service Fabric szolgáltatásokkal kommunikálni."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: msfussell
ms.assetid: 7d1052ec-2c9f-443d-8b99-b75c97266e6c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/9/2017
ms.author: vturecek
ms.openlocfilehash: b8b374a71d4c5d21f48a560a3a8c81b357fe418d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Csatlakozás és a Service Fabric szolgáltatásokkal kommunikálni
A Service Fabric szolgáltatás fut. valahol a Service Fabric-fürt, általában pontjain több virtuális géphez Akkor helyezheti át egy helyen tooanother, a hello szolgáltatás tulajdonosa, vagy automatikusan a Service Fabric. Szolgáltatások nincsenek statikus módon kapcsolt tooa az adott számítógépet vagy a cím.

A Service Fabric-alkalmazás általában sok különböző szolgáltatások, ahol minden szolgáltatás hajt végre egy speciális feladat tevődik össze. Ezek a szolgáltatások kommunikálhatnak egymással tooform egy teljes funkciót, például egy webes alkalmazás megjelenítési különböző részeit. Alkalmazások, amelyek tooand szolgáltatásokkal kommunikálni ügyfél is van. Ez a dokumentum ismerteti hogyan tooset és a Service Fabric a szolgáltatások közötti kommunikáció.

A Microsoft Virtual Academy videó azt is ismerteti, amelyek a szolgáltatások közötti kommunikáció:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965">  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a>Kapcsolja a saját protokoll
A Service Fabric kezelhetőek hello életciklusát a szolgáltatások, de nem létrehozni, akkor a szolgáltatások mire döntéseket. Ez magában foglalja a kommunikációt. Ha a szolgáltatás már meg van nyitva a Service Fabric, a szolgáltatás lehetőség tooset fel a bejövő kéréseket a végpont által bármilyen protokollt vagy a kommunikációs verem használatával szeretne. A szolgáltatás figyelni fogja normál **IP:port** cím bármely címzési séma, például egy URI-t használ. Több egy szolgáltatáspéldány vagy replikák megoszthatja az gazdafolyamat, ebben az esetben azok fog kell toouse különböző porttal, vagy használjon egy port megosztása mechanizmus, például a Windows hello http.sys kernel-illesztőprogram. Mindkét esetben minden szolgáltatáspéldány vagy egy replika kell egyedi módon címezhetők.

![Szolgáltatás-végpontok][1]

## <a name="service-discovery-and-resolution"></a>A szolgáltatásészlelés és megoldás szerint
Elosztott rendszer, a szolgáltatások át lehet egy gép tooanother adott idő alatt. Ez akkor fordulhat elő, beleértve az erőforrások terhelésének, a frissítések, feladatátvétel vagy kibővített különböző okokból. Ez azt jelenti, szolgáltatás-végpont címére módosítható, mert a hello szolgáltatás áthelyezi toonodes különböző IP-címekkel rendelkező, és előfordulhat, hogy nyissa meg a különböző portokon Ha hello service dinamikus kiválasztott portot használ.

![Szolgáltatások][7]

A Service Fabric biztosít a felderítés és a névfeloldási szolgáltatás nevű hello szolgáltatás. hello szolgáltatás megőrzi a táblázat, amely leképezhető nevű szolgáltatáspéldány toohello végpont címek azok figyelni. A Service Fabric összes elnevezett szolgáltatáspéldány képviselt URI-k, mint például egyedi nevük legyen `"fabric:/MyApplication/MyService"`. hello szolgáltatás hello neve hello szolgáltatás hello élettartamuk során változatlan marad, csak hello végpont címre módosítható, ha a szolgáltatások. Ez a hasonló toowebsites, amelyek állandó URL-címek, de ha hello IP-címet módosíthatja. És hasonló tooDNS hello webhely, amely kiküszöböli a webhely URL-címek tooIP címét, a Service Fabric a regisztráló, amely leképezhető szolgáltatás nevek tootheir végpont címe.

![Szolgáltatás-végpontok][2]

Megoldása és a csatlakozás tooservices magában foglalja a következő ismétlődő lépéseket hello:

* **Hárítsa el**: Get hello végpontot, amely a szolgáltatás közzétette a hello szolgáltatás.
* **Csatlakozás**: Connect toohello szolgáltatás keresztül függetlenül, hogy a végpont által használt protokoll azt.
* **Próbálja meg újra**: számos előnnyel, például ha hello szolgáltatás át lett helyezve, mivel hello utolsó idő hello végpontcím lett feloldva a kapcsolódási kísérlet sikertelenek lehetnek. Ebben az esetben hello resolve megelőző, és csatlakozzon újra megpróbálja toobe. lépést kell, és ez a ciklus ismétlődik mindaddig, amíg hello csatlakozás sikeres.

## <a name="connecting-tooother-services"></a>Csatlakozás tooother szolgáltatások
Szolgáltatások tooeach csatlakozás a fürtben található egyéb általában közvetlenül hozzáférhet az egyéb szolgáltatások hello végpontok mert fürtben található csomópontok hello hello ugyanazon a helyi hálózaton. toomake könnyebb tooconnect szolgáltatások között, a Service Fabric nyújt kiegészítő szolgáltatásokat használó hello szolgáltatás. A DNS-szolgáltatás és a fordított proxy szolgáltatás.


### <a name="dns-service"></a>DNS-szolgáltatás
Számos szolgáltatás, főleg indexelése szolgáltatások lehet egy meglévő URL-címet, mert éppen képes tooresolve ezek helyett hello szabványos DNS protokoll (hello szolgáltatás protokoll) nagyon hasznos, különösen az alkalmazás "növekedési, az eltolás mértékét megadó" forgatókönyvek. Ennek az az pontosan milyen hello DNS szolgáltatásnak nincs. Lehetővé teszi, hogy Ön toomap DNS-nevek tooa szolgáltatás neve, és ezért a végpont IP-címeket feloldani. 

Látható hello, az alábbi ábrán, hello DNS-szolgáltatás fut a Service Fabric-fürt hello, DNS-nevek tooservice nevek amelyből vannak majd hello szolgáltatás tooreturn hello végpont címek tooconnect való képezi le. hello DNS-név hello szolgáltatás létrehozása a hello időpontjában valósul meg. 

![Szolgáltatás-végpontok][9]

Hogyan toouse hello DNS-szolgáltatás: a további részletekért [DNS-szolgáltatás az Azure Service Fabric](service-fabric-dnsservice.md) cikk.

### <a name="reverse-proxy-service"></a>Fordított proxy szolgáltatás
fordított proxy hello hello fürt, amely elérhetővé teszi a HTTP-végpontokról, beleértve a HTTPS szolgáltatások megoldást. hello fordított proxy jelentősen egyszerűbb hívása más szolgáltatások és azok módszerek azzal, hogy egy adott URI formátumú hello megoldásához leírók csatlakozzon, egy szolgáltatás toocommunicate egy másik használatával újra lépéseinek hello elnevezési állapotát. Ez azt jelenti elrejti az Ön Naming Service hello azáltal, hogy a lehető legegyszerűbb hívja az egy URL-címet egyéb szolgáltatások hívásakor.

![Szolgáltatás-végpontok][10]

További információ a hogyan toouse hello fordított proxy szolgáltatás: [fordított proxy az Azure Service Fabric](service-fabric-reverseproxy.md) cikk.

## <a name="connections-from-external-clients"></a>Külső ügyfelek kapcsolódását
Szolgáltatások tooeach csatlakozás a fürtben található egyéb általában közvetlenül hozzáférhet az egyéb szolgáltatások hello végpontok mert fürtben található csomópontok hello hello ugyanazon a helyi hálózaton. Bizonyos környezetekben előfordulhat azonban, fürt, amely a külső érkező forgalmat korlátozott számú portok keresztül továbbítja a terheléselosztó mögött. Ezekben az esetekben szolgáltatások továbbra is kommunikálhatnak egymással és hárítsa el a címek az hello szolgáltatás, de további lépéseket tett tooallow külső ügyfelek tooconnect tooservices kell lennie.

## <a name="service-fabric-in-azure"></a>A Service Fabric az Azure-ban
Az Azure Service Fabric-fürtök az Azure terheléselosztó mögött helyezkedik el. Minden külső forgalom toohello fürt kell haladnia hello terheléselosztóhoz. hello terheléselosztó lesz automatikusan továbbítsa a forgalmat egy adott port tooa véletlenszerű levő *csomópont* , amely rendelkezik hello nyissa meg a ugyanazt a portot. hello Azure Load Balancer csak ismer portok nyitva a hello *csomópontok*, portok nyitva személy nem ismer *szolgáltatások*.

![Az Azure Load Balancer és a Service Fabric topológia][3]

Például a rendelés tooaccept külső porton forgalom **80-as**, a következő dolgot hello úgy kell konfigurálni:

1. Az írási olyan szolgáltatás, amely a 80-as porton figyel. Hello szolgáltatás ServiceManifest.xml 80-as portot konfigurálja, és nyissa meg a figyelő hello szolgáltatásban, például egy önálló üzemeltetett webkiszolgálón.

    ```xml
    <Resources>
        <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>
    ```
    ```csharp
        class HttpCommunicationListener : ICommunicationListener
        {
            ...

            public Task<string> OpenAsync(CancellationToken cancellationToken)
            {
                EndpointResourceDescription endpoint =
                    serviceContext.CodePackageActivationContext.GetEndpoint("WebEndpoint");

                string uriPrefix = $"{endpoint.Protocol}://+:{endpoint.Port}/myapp/";

                this.httpListener = new HttpListener();
                this.httpListener.Prefixes.Add(uriPrefix);
                this.httpListener.Start();

                string publishUri = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
                return Task.FromResult(publishUri);
            }

            ...
        }

        class WebService : StatelessService
        {
            ...

            protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
            {
                return new[] { new ServiceInstanceListener(context => new HttpCommunicationListener(context))};
            }

            ...
        }
    ```
    ```java
        class HttpCommunicationlistener implements CommunicationListener {
            ...

            @Override
            public CompletableFuture<String> openAsync(CancellationToken arg0) {
                EndpointResourceDescription endpoint =
                    this.serviceContext.getCodePackageActivationContext().getEndpoint("WebEndpoint");
                try {
                    HttpServer server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress(endpoint.getPort()), 0);
                    server.start();

                    String publishUri = String.format("http://%s:%d/",
                        this.serviceContext.getNodeContext().getIpAddressOrFQDN(), endpoint.getPort());
                    return CompletableFuture.completedFuture(publishUri);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }

            ...
        }

        class WebService extends StatelessService {
            ...

            @Override
            protected List<ServiceInstanceListener> createServiceInstanceListeners() {
                <ServiceInstanceListener> listeners = new ArrayList<ServiceInstanceListener>();
                listeners.add(new ServiceInstanceListener((context) -> new HttpCommunicationlistener(context)));
                return listeners;       
            }

            ...
        }
    ```
2. A Service Fabric-fürt létrehozása az Azure-ban, és adja meg a port **80** hello csomóponttípus hello szolgáltatást üzemeltető egyéni végpont-portjaként működik. Ha egynél több csomópont típusa van, beállíthat egy *elhelyezési korlátozás* hello szolgáltatás tooensure csak az hello egyéni végponti port nyitva van hello csomóponttípus fut a.

    ![A csomóponttípus port megnyitása][4]
3. Hello fürt létrehozása után adja meg Azure Load Balancer hello hello fürt erőforráscsoport tooforward forgalom 80-as porton. Hello Azure-portálon keresztül fürt létrehozása, ha ez be van állítva automatikusan minden konfigurált egyéni végpont-porthoz.

    ![Továbbítsa a forgalmat a hello Azure Load Balancer][5]
4. hello Azure Load Balancer használ egy mintavételi toodetermine e, vagy nem toosend forgalom tooa csomópontjában. hello mintavételi rendszeresen ellenőrzi a minden egyes csomópont toodetermine hello csomópont válaszol-e a végpont. Hello mintavételi tooreceive választ után a konfigurált számú alkalommal nem sikerül, ha hello terheléselosztó leállítja a forgalom toothat csomópont küldésekor. Hello Azure-portálon keresztül a fürt létrehozásakor egy mintavételt automatikusan be van állítva minden konfigurált egyéni végpont-porthoz.

    ![Továbbítsa a forgalmat a hello Azure Load Balancer][8]

Fontos tooremember hello mintavétel, valamint hello Azure Load Balancer csak tudnia: hello *csomópontok*, nem hello *szolgáltatások* hello csomópontokon futó. hello Azure Load Balancer mindig elküldi a forgalom toonodes toohello mintavételi válaszolnak, így gondot kell fordítani tooensure szolgáltatások érhetők el, amelyek képesek toorespond toohello mintavételi hello csomópontján.

## <a name="reliable-services-built-in-communication-api-options"></a>Megbízható szolgáltatások: Beépített kommunikációs API-beállítások
hello megbízható szolgáltatások keretében számos előre elkészített kommunikációs lehetőségekről részét képező. hello döntés arról, hogy mely egyik működhet a legjobban az Ön programozási modell, hello kommunikációs keretrendszer és programozási nyelv, a szolgáltatások írt hello hello hello választott függ.

* **Nem adott protokoll:** Ha egy adott kiválasztott kommunikációs keretrendszer nincs, de szeretné tooget valami naprakész, és fut. gyorsan, akkor hello ideális megoldás [szolgáltatás távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md), amely lehetővé teszi, hogy erős típusmegadású távoli eljáráshívásnak Reliable Services és Reliable Actors hívásokat. Ez a legegyszerűbb hello és leggyorsabb módon tooget használatába a szolgáltatások közötti kommunikáció. Szolgáltatás távoli eljáráshívási szolgáltatás címek, a kapcsolat, az újra gombra és a hibakezelés feloldását végzi. Ez az a C# és a Java-alkalmazások számára érhető el.
* **HTTP**: nyelvtől független kommunikációhoz HTTP biztosít egy szabványos választott nyelven is elérhető számos különböző, a Service Fabric által támogatott HTTP-kiszolgálók és eszközök. Szolgáltatások bármely érhető el, beleértve a HTTP-verem használható [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) C#-alkalmazások. C# nyelven írt ügyfelek kihasználhatják a hello `ICommunicationClient` és `ServicePartitionClient` osztályokat, mivel a Java, használja a hello `CommunicationClient` és `FabricServicePartitionClient` osztályok, [névfeloldási szolgáltatás, a HTTP-kapcsolatoknál és az újrapróbálkozási ciklusok](service-fabric-reliable-services-communication.md).
* **WCF**: Ha van, a kommunikáció keretrendszer WCF használó meglévő kódja, akkor használhatja a hello `WcfCommunicationListener` hello kiszolgálóoldali és `WcfCommunicationClient` és `ServicePartitionClient` osztályok hello ügyfél számára. Ez azonban lehetőség csak a C#-alkalmazást a Windows-alapú fürtök. További részletekért lásd: Ez a cikk [WCF-alapú megvalósítás hello kommunikációs verem](service-fabric-reliable-services-communication-wcf.md).

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Egyéni protokollok és egyéb kommunikációs keretrendszerek használatával
Szolgáltatások használhatnak minden protokoll vagy keretrendszer kommunikációhoz, hogy egy egyéni bináris protokoll TCP-szoftvercsatornák vagy az esemény streamelését keresztül-e [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) vagy [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/). A Service Fabric biztosít a kommunikációs API-t csatlakoztathatja a kommunikációs verem be, amíg minden hello toodiscover működik, és csatlakozzon az Ön kiveszik. Ebben a cikkben találhat kapcsolatos hello [megbízható kommunikáció modell](service-fabric-reliable-services-communication.md) további részleteket.

## <a name="next-steps"></a>Következő lépések
További információ a hello fogalmakat és API-k hello elérhető további [Reliable Services kommunikációs modellt](service-fabric-reliable-services-communication.md), majd használatának gyors megkezdése [szolgáltatás távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md) vagy mélyreható toolearn hogyan toowrite egy kommunikációs figyelővel [Web API-t önálló gazdagép OWIN](service-fabric-reliable-services-communication-webapi.md).

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
