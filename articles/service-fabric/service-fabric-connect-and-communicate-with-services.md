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
# <a name="connect-and-communicate-with-services-in-service-fabric"></a><span data-ttu-id="b4c42-103">Csatlakozás és a Service Fabric szolgáltatásokkal kommunikálni</span><span class="sxs-lookup"><span data-stu-id="b4c42-103">Connect and communicate with services in Service Fabric</span></span>
<span data-ttu-id="b4c42-104">A Service Fabric szolgáltatás fut. valahol a Service Fabric-fürt, általában pontjain több virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="b4c42-104">In Service Fabric, a service runs somewhere in a Service Fabric cluster, typically distributed across multiple VMs.</span></span> <span data-ttu-id="b4c42-105">Akkor helyezheti át egy helyen tooanother, a hello szolgáltatás tulajdonosa, vagy automatikusan a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b4c42-105">It can be moved from one place tooanother, either by hello service owner, or automatically by Service Fabric.</span></span> <span data-ttu-id="b4c42-106">Szolgáltatások nincsenek statikus módon kapcsolt tooa az adott számítógépet vagy a cím.</span><span class="sxs-lookup"><span data-stu-id="b4c42-106">Services are not statically tied tooa particular machine or address.</span></span>

<span data-ttu-id="b4c42-107">A Service Fabric-alkalmazás általában sok különböző szolgáltatások, ahol minden szolgáltatás hajt végre egy speciális feladat tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="b4c42-107">A Service Fabric application is generally composed of many different services, where each service performs a specialized task.</span></span> <span data-ttu-id="b4c42-108">Ezek a szolgáltatások kommunikálhatnak egymással tooform egy teljes funkciót, például egy webes alkalmazás megjelenítési különböző részeit.</span><span class="sxs-lookup"><span data-stu-id="b4c42-108">These services may communicate with each other tooform a complete function, such as rendering different parts of a web application.</span></span> <span data-ttu-id="b4c42-109">Alkalmazások, amelyek tooand szolgáltatásokkal kommunikálni ügyfél is van.</span><span class="sxs-lookup"><span data-stu-id="b4c42-109">There are also client applications that connect tooand communicate with services.</span></span> <span data-ttu-id="b4c42-110">Ez a dokumentum ismerteti hogyan tooset és a Service Fabric a szolgáltatások közötti kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="b4c42-110">This document discusses how tooset up communication with and between your services in Service Fabric.</span></span>

<span data-ttu-id="b4c42-111">A Microsoft Virtual Academy videó azt is ismerteti, amelyek a szolgáltatások közötti kommunikáció:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span><span class="sxs-lookup"><span data-stu-id="b4c42-111">This Microsoft Virtual Academy video also discusses service communication: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965"></span></span>  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a><span data-ttu-id="b4c42-112">Kapcsolja a saját protokoll</span><span class="sxs-lookup"><span data-stu-id="b4c42-112">Bring your own protocol</span></span>
<span data-ttu-id="b4c42-113">A Service Fabric kezelhetőek hello életciklusát a szolgáltatások, de nem létrehozni, akkor a szolgáltatások mire döntéseket.</span><span class="sxs-lookup"><span data-stu-id="b4c42-113">Service Fabric helps manage hello lifecycle of your services but it does not make decisions about what your services do.</span></span> <span data-ttu-id="b4c42-114">Ez magában foglalja a kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="b4c42-114">This includes communication.</span></span> <span data-ttu-id="b4c42-115">Ha a szolgáltatás már meg van nyitva a Service Fabric, a szolgáltatás lehetőség tooset fel a bejövő kéréseket a végpont által bármilyen protokollt vagy a kommunikációs verem használatával szeretne.</span><span class="sxs-lookup"><span data-stu-id="b4c42-115">When your service is opened by Service Fabric, that's your service's opportunity tooset up an endpoint for incoming requests, using whatever protocol or communication stack you want.</span></span> <span data-ttu-id="b4c42-116">A szolgáltatás figyelni fogja normál **IP:port** cím bármely címzési séma, például egy URI-t használ.</span><span class="sxs-lookup"><span data-stu-id="b4c42-116">Your service will listen on a normal **IP:port** address using any addressing scheme, such as a URI.</span></span> <span data-ttu-id="b4c42-117">Több egy szolgáltatáspéldány vagy replikák megoszthatja az gazdafolyamat, ebben az esetben azok fog kell toouse különböző porttal, vagy használjon egy port megosztása mechanizmus, például a Windows hello http.sys kernel-illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="b4c42-117">Multiple service instances or replicas may share a host process, in which case they will either need toouse different ports or use a port-sharing mechanism, such as hello http.sys kernel driver in Windows.</span></span> <span data-ttu-id="b4c42-118">Mindkét esetben minden szolgáltatáspéldány vagy egy replika kell egyedi módon címezhetők.</span><span class="sxs-lookup"><span data-stu-id="b4c42-118">In either case, each service instance or replica in a host process must be uniquely addressable.</span></span>

![Szolgáltatás-végpontok][1]

## <a name="service-discovery-and-resolution"></a><span data-ttu-id="b4c42-120">A szolgáltatásészlelés és megoldás szerint</span><span class="sxs-lookup"><span data-stu-id="b4c42-120">Service discovery and resolution</span></span>
<span data-ttu-id="b4c42-121">Elosztott rendszer, a szolgáltatások át lehet egy gép tooanother adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="b4c42-121">In a distributed system, services may move from one machine tooanother over time.</span></span> <span data-ttu-id="b4c42-122">Ez akkor fordulhat elő, beleértve az erőforrások terhelésének, a frissítések, feladatátvétel vagy kibővített különböző okokból. Ez azt jelenti, szolgáltatás-végpont címére módosítható, mert a hello szolgáltatás áthelyezi toonodes különböző IP-címekkel rendelkező, és előfordulhat, hogy nyissa meg a különböző portokon Ha hello service dinamikus kiválasztott portot használ.</span><span class="sxs-lookup"><span data-stu-id="b4c42-122">This can happen for various reasons, including resource balancing, upgrades, failovers, or scale-out. This means service endpoint addresses change as hello service moves toonodes with different IP addresses, and may open on different ports if hello service uses a dynamically selected port.</span></span>

![Szolgáltatások][7]

<span data-ttu-id="b4c42-124">A Service Fabric biztosít a felderítés és a névfeloldási szolgáltatás nevű hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4c42-124">Service Fabric provides a discovery and resolution service called hello Naming Service.</span></span> <span data-ttu-id="b4c42-125">hello szolgáltatás megőrzi a táblázat, amely leképezhető nevű szolgáltatáspéldány toohello végpont címek azok figyelni.</span><span class="sxs-lookup"><span data-stu-id="b4c42-125">hello Naming Service maintains a table that maps named service instances toohello endpoint addresses they listen on.</span></span> <span data-ttu-id="b4c42-126">A Service Fabric összes elnevezett szolgáltatáspéldány képviselt URI-k, mint például egyedi nevük legyen `"fabric:/MyApplication/MyService"`.</span><span class="sxs-lookup"><span data-stu-id="b4c42-126">All named service instances in Service Fabric have unique names represented as URIs, for example, `"fabric:/MyApplication/MyService"`.</span></span> <span data-ttu-id="b4c42-127">hello szolgáltatás hello neve hello szolgáltatás hello élettartamuk során változatlan marad, csak hello végpont címre módosítható, ha a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b4c42-127">hello name of hello service does not change over hello lifetime of hello service, it's only hello endpoint addresses that can change when services move.</span></span> <span data-ttu-id="b4c42-128">Ez a hasonló toowebsites, amelyek állandó URL-címek, de ha hello IP-címet módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="b4c42-128">This is analogous toowebsites that have constant URLs but where hello IP address may change.</span></span> <span data-ttu-id="b4c42-129">És hasonló tooDNS hello webhely, amely kiküszöböli a webhely URL-címek tooIP címét, a Service Fabric a regisztráló, amely leképezhető szolgáltatás nevek tootheir végpont címe.</span><span class="sxs-lookup"><span data-stu-id="b4c42-129">And similar tooDNS on hello web, which resolves website URLs tooIP addresses, Service Fabric has a registrar that maps service names tootheir endpoint address.</span></span>

![Szolgáltatás-végpontok][2]

<span data-ttu-id="b4c42-131">Megoldása és a csatlakozás tooservices magában foglalja a következő ismétlődő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="b4c42-131">Resolving and connecting tooservices involves hello following steps run in a loop:</span></span>

* <span data-ttu-id="b4c42-132">**Hárítsa el**: Get hello végpontot, amely a szolgáltatás közzétette a hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4c42-132">**Resolve**: Get hello endpoint that a service has published from hello Naming Service.</span></span>
* <span data-ttu-id="b4c42-133">**Csatlakozás**: Connect toohello szolgáltatás keresztül függetlenül, hogy a végpont által használt protokoll azt.</span><span class="sxs-lookup"><span data-stu-id="b4c42-133">**Connect**: Connect toohello service over whatever protocol it uses on that endpoint.</span></span>
* <span data-ttu-id="b4c42-134">**Próbálja meg újra**: számos előnnyel, például ha hello szolgáltatás át lett helyezve, mivel hello utolsó idő hello végpontcím lett feloldva a kapcsolódási kísérlet sikertelenek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="b4c42-134">**Retry**: A connection attempt may fail for any number of reasons, for example if hello service has moved since hello last time hello endpoint address was resolved.</span></span> <span data-ttu-id="b4c42-135">Ebben az esetben hello resolve megelőző, és csatlakozzon újra megpróbálja toobe. lépést kell, és ez a ciklus ismétlődik mindaddig, amíg hello csatlakozás sikeres.</span><span class="sxs-lookup"><span data-stu-id="b4c42-135">In that case, hello preceding resolve and connect steps need toobe retried, and this cycle is repeated until hello connection succeeds.</span></span>

## <a name="connecting-tooother-services"></a><span data-ttu-id="b4c42-136">Csatlakozás tooother szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4c42-136">Connecting tooother services</span></span>
<span data-ttu-id="b4c42-137">Szolgáltatások tooeach csatlakozás a fürtben található egyéb általában közvetlenül hozzáférhet az egyéb szolgáltatások hello végpontok mert fürtben található csomópontok hello hello ugyanazon a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="b4c42-137">Services connecting tooeach other inside a cluster generally can directly access hello endpoints of other services because hello nodes in a cluster are on hello same local network.</span></span> <span data-ttu-id="b4c42-138">toomake könnyebb tooconnect szolgáltatások között, a Service Fabric nyújt kiegészítő szolgáltatásokat használó hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4c42-138">toomake is easier tooconnect between services, Service Fabric provides additional services that use hello Naming Service.</span></span> <span data-ttu-id="b4c42-139">A DNS-szolgáltatás és a fordított proxy szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b4c42-139">A DNS service and a reverse proxy service.</span></span>


### <a name="dns-service"></a><span data-ttu-id="b4c42-140">DNS-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b4c42-140">DNS service</span></span>
<span data-ttu-id="b4c42-141">Számos szolgáltatás, főleg indexelése szolgáltatások lehet egy meglévő URL-címet, mert éppen képes tooresolve ezek helyett hello szabványos DNS protokoll (hello szolgáltatás protokoll) nagyon hasznos, különösen az alkalmazás "növekedési, az eltolás mértékét megadó" forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="b4c42-141">Since many services, especially containerized services, can have an existing URL name, being able tooresolve these using hello standard DNS protocol (rather than hello Naming Service protocol) is very convenient, especially in application "lift and shift" scenarios.</span></span> <span data-ttu-id="b4c42-142">Ennek az az pontosan milyen hello DNS szolgáltatásnak nincs.</span><span class="sxs-lookup"><span data-stu-id="b4c42-142">This is exactly what hello DNS service does.</span></span> <span data-ttu-id="b4c42-143">Lehetővé teszi, hogy Ön toomap DNS-nevek tooa szolgáltatás neve, és ezért a végpont IP-címeket feloldani.</span><span class="sxs-lookup"><span data-stu-id="b4c42-143">It enables you toomap DNS names tooa service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="b4c42-144">Látható hello, az alábbi ábrán, hello DNS-szolgáltatás fut a Service Fabric-fürt hello, DNS-nevek tooservice nevek amelyből vannak majd hello szolgáltatás tooreturn hello végpont címek tooconnect való képezi le.</span><span class="sxs-lookup"><span data-stu-id="b4c42-144">As shown in hello following diagram, hello DNS service, running in hello Service Fabric cluster, maps DNS names tooservice names which are then resolved by hello Naming Service tooreturn hello endpoint addresses tooconnect to.</span></span> <span data-ttu-id="b4c42-145">hello DNS-név hello szolgáltatás létrehozása a hello időpontjában valósul meg.</span><span class="sxs-lookup"><span data-stu-id="b4c42-145">hello DNS name for hello service is provided at hello time of creation.</span></span> 

![Szolgáltatás-végpontok][9]

<span data-ttu-id="b4c42-147">Hogyan toouse hello DNS-szolgáltatás: a további részletekért [DNS-szolgáltatás az Azure Service Fabric](service-fabric-dnsservice.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4c42-147">For more details on how toouse hello DNS service see [DNS service in Azure Service Fabric](service-fabric-dnsservice.md) article.</span></span>

### <a name="reverse-proxy-service"></a><span data-ttu-id="b4c42-148">Fordított proxy szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b4c42-148">Reverse proxy service</span></span>
<span data-ttu-id="b4c42-149">fordított proxy hello hello fürt, amely elérhetővé teszi a HTTP-végpontokról, beleértve a HTTPS szolgáltatások megoldást.</span><span class="sxs-lookup"><span data-stu-id="b4c42-149">hello reverse proxy addresses services in hello cluster that exposes HTTP endpoints including HTTPS.</span></span> <span data-ttu-id="b4c42-150">hello fordított proxy jelentősen egyszerűbb hívása más szolgáltatások és azok módszerek azzal, hogy egy adott URI formátumú hello megoldásához leírók csatlakozzon, egy szolgáltatás toocommunicate egy másik használatával újra lépéseinek hello elnevezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="b4c42-150">hello reverse proxy greatly simplifies calling other services and their methods by having a specific URI format and handles hello resolve, connect, retry steps required for one service toocommunicate with another using hello Naming Serivce.</span></span> <span data-ttu-id="b4c42-151">Ez azt jelenti elrejti az Ön Naming Service hello azáltal, hogy a lehető legegyszerűbb hívja az egy URL-címet egyéb szolgáltatások hívásakor.</span><span class="sxs-lookup"><span data-stu-id="b4c42-151">In other words, it hides hello Naming Service from you when calling other services by making this as simple as calling a URL.</span></span>

![Szolgáltatás-végpontok][10]

<span data-ttu-id="b4c42-153">További információ a hogyan toouse hello fordított proxy szolgáltatás: [fordított proxy az Azure Service Fabric](service-fabric-reverseproxy.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4c42-153">For more details on how toouse hello reverse proxy service see [Reverse proxy in Azure Service Fabric](service-fabric-reverseproxy.md) article.</span></span>

## <a name="connections-from-external-clients"></a><span data-ttu-id="b4c42-154">Külső ügyfelek kapcsolódását</span><span class="sxs-lookup"><span data-stu-id="b4c42-154">Connections from external clients</span></span>
<span data-ttu-id="b4c42-155">Szolgáltatások tooeach csatlakozás a fürtben található egyéb általában közvetlenül hozzáférhet az egyéb szolgáltatások hello végpontok mert fürtben található csomópontok hello hello ugyanazon a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="b4c42-155">Services connecting tooeach other inside a cluster generally can directly access hello endpoints of other services because hello nodes in a cluster are on hello same local network.</span></span> <span data-ttu-id="b4c42-156">Bizonyos környezetekben előfordulhat azonban, fürt, amely a külső érkező forgalmat korlátozott számú portok keresztül továbbítja a terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="b4c42-156">In some environments, however, a cluster may be behind a load balancer that routes external ingress traffic through a limited set of ports.</span></span> <span data-ttu-id="b4c42-157">Ezekben az esetekben szolgáltatások továbbra is kommunikálhatnak egymással és hárítsa el a címek az hello szolgáltatás, de további lépéseket tett tooallow külső ügyfelek tooconnect tooservices kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b4c42-157">In these cases, services can still communicate with each other and resolve addresses using hello Naming Service, but extra steps must be taken tooallow external clients tooconnect tooservices.</span></span>

## <a name="service-fabric-in-azure"></a><span data-ttu-id="b4c42-158">A Service Fabric az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b4c42-158">Service Fabric in Azure</span></span>
<span data-ttu-id="b4c42-159">Az Azure Service Fabric-fürtök az Azure terheléselosztó mögött helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="b4c42-159">A Service Fabric cluster in Azure is placed behind an Azure Load Balancer.</span></span> <span data-ttu-id="b4c42-160">Minden külső forgalom toohello fürt kell haladnia hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="b4c42-160">All external traffic toohello cluster must pass through hello load balancer.</span></span> <span data-ttu-id="b4c42-161">hello terheléselosztó lesz automatikusan továbbítsa a forgalmat egy adott port tooa véletlenszerű levő *csomópont* , amely rendelkezik hello nyissa meg a ugyanazt a portot.</span><span class="sxs-lookup"><span data-stu-id="b4c42-161">hello load balancer will automatically forward traffic inbound on a given port tooa random *node* that has hello same port open.</span></span> <span data-ttu-id="b4c42-162">hello Azure Load Balancer csak ismer portok nyitva a hello *csomópontok*, portok nyitva személy nem ismer *szolgáltatások*.</span><span class="sxs-lookup"><span data-stu-id="b4c42-162">hello Azure Load Balancer only knows about ports open on hello *nodes*, it does not know about ports open by individual *services*.</span></span>

![Az Azure Load Balancer és a Service Fabric topológia][3]

<span data-ttu-id="b4c42-164">Például a rendelés tooaccept külső porton forgalom **80-as**, a következő dolgot hello úgy kell konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="b4c42-164">For example, in order tooaccept external traffic on port **80**, hello following things must be configured:</span></span>

1. <span data-ttu-id="b4c42-165">Az írási olyan szolgáltatás, amely a 80-as porton figyel.</span><span class="sxs-lookup"><span data-stu-id="b4c42-165">Write a service that listens on port 80.</span></span> <span data-ttu-id="b4c42-166">Hello szolgáltatás ServiceManifest.xml 80-as portot konfigurálja, és nyissa meg a figyelő hello szolgáltatásban, például egy önálló üzemeltetett webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b4c42-166">Configure port 80 in hello service's ServiceManifest.xml and open a listener in hello service, for example, a self-hosted web server.</span></span>

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
2. <span data-ttu-id="b4c42-167">A Service Fabric-fürt létrehozása az Azure-ban, és adja meg a port **80** hello csomóponttípus hello szolgáltatást üzemeltető egyéni végpont-portjaként működik.</span><span class="sxs-lookup"><span data-stu-id="b4c42-167">Create a Service Fabric Cluster in Azure and specify port **80** as a custom endpoint port for hello node type that will host hello service.</span></span> <span data-ttu-id="b4c42-168">Ha egynél több csomópont típusa van, beállíthat egy *elhelyezési korlátozás* hello szolgáltatás tooensure csak az hello egyéni végponti port nyitva van hello csomóponttípus fut a.</span><span class="sxs-lookup"><span data-stu-id="b4c42-168">If you have more than one node type, you can set up a *placement constraint* on hello service tooensure it only runs on hello node type that has hello custom endpoint port opened.</span></span>

    ![A csomóponttípus port megnyitása][4]
3. <span data-ttu-id="b4c42-170">Hello fürt létrehozása után adja meg Azure Load Balancer hello hello fürt erőforráscsoport tooforward forgalom 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="b4c42-170">Once hello cluster has been created, configure hello Azure Load Balancer in hello cluster's Resource Group tooforward traffic on port 80.</span></span> <span data-ttu-id="b4c42-171">Hello Azure-portálon keresztül fürt létrehozása, ha ez be van állítva automatikusan minden konfigurált egyéni végpont-porthoz.</span><span class="sxs-lookup"><span data-stu-id="b4c42-171">When creating a cluster through hello Azure portal, this is set up automatically for each custom endpoint port that was configured.</span></span>

    ![Továbbítsa a forgalmat a hello Azure Load Balancer][5]
4. <span data-ttu-id="b4c42-173">hello Azure Load Balancer használ egy mintavételi toodetermine e, vagy nem toosend forgalom tooa csomópontjában.</span><span class="sxs-lookup"><span data-stu-id="b4c42-173">hello Azure Load Balancer uses a probe toodetermine whether or not toosend traffic tooa particular node.</span></span> <span data-ttu-id="b4c42-174">hello mintavételi rendszeresen ellenőrzi a minden egyes csomópont toodetermine hello csomópont válaszol-e a végpont.</span><span class="sxs-lookup"><span data-stu-id="b4c42-174">hello probe periodically checks an endpoint on each node toodetermine whether or not hello node is responding.</span></span> <span data-ttu-id="b4c42-175">Hello mintavételi tooreceive választ után a konfigurált számú alkalommal nem sikerül, ha hello terheléselosztó leállítja a forgalom toothat csomópont küldésekor.</span><span class="sxs-lookup"><span data-stu-id="b4c42-175">If hello probe fails tooreceive a response after a configured number of times, hello load balancer stops sending traffic toothat node.</span></span> <span data-ttu-id="b4c42-176">Hello Azure-portálon keresztül a fürt létrehozásakor egy mintavételt automatikusan be van állítva minden konfigurált egyéni végpont-porthoz.</span><span class="sxs-lookup"><span data-stu-id="b4c42-176">When creating a cluster through hello Azure portal, a probe is automatically set up for each custom endpoint port that was configured.</span></span>

    ![Továbbítsa a forgalmat a hello Azure Load Balancer][8]

<span data-ttu-id="b4c42-178">Fontos tooremember hello mintavétel, valamint hello Azure Load Balancer csak tudnia: hello *csomópontok*, nem hello *szolgáltatások* hello csomópontokon futó.</span><span class="sxs-lookup"><span data-stu-id="b4c42-178">It's important tooremember that hello Azure Load Balancer and hello probe only know about hello *nodes*, not hello *services* running on hello nodes.</span></span> <span data-ttu-id="b4c42-179">hello Azure Load Balancer mindig elküldi a forgalom toonodes toohello mintavételi válaszolnak, így gondot kell fordítani tooensure szolgáltatások érhetők el, amelyek képesek toorespond toohello mintavételi hello csomópontján.</span><span class="sxs-lookup"><span data-stu-id="b4c42-179">hello Azure Load Balancer will always send traffic toonodes that respond toohello probe, so care must be taken tooensure services are available on hello nodes that are able toorespond toohello probe.</span></span>

## <a name="reliable-services-built-in-communication-api-options"></a><span data-ttu-id="b4c42-180">Megbízható szolgáltatások: Beépített kommunikációs API-beállítások</span><span class="sxs-lookup"><span data-stu-id="b4c42-180">Reliable Services: Built-in communication API options</span></span>
<span data-ttu-id="b4c42-181">hello megbízható szolgáltatások keretében számos előre elkészített kommunikációs lehetőségekről részét képező.</span><span class="sxs-lookup"><span data-stu-id="b4c42-181">hello Reliable Services framework ships with several pre-built communication options.</span></span> <span data-ttu-id="b4c42-182">hello döntés arról, hogy mely egyik működhet a legjobban az Ön programozási modell, hello kommunikációs keretrendszer és programozási nyelv, a szolgáltatások írt hello hello hello választott függ.</span><span class="sxs-lookup"><span data-stu-id="b4c42-182">hello decision about which one will work best for you depends on hello choice of hello programming model, hello communication framework, and hello programming language that your services are written in.</span></span>

* <span data-ttu-id="b4c42-183">**Nem adott protokoll:** Ha egy adott kiválasztott kommunikációs keretrendszer nincs, de szeretné tooget valami naprakész, és fut. gyorsan, akkor hello ideális megoldás [szolgáltatás távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md), amely lehetővé teszi, hogy erős típusmegadású távoli eljáráshívásnak Reliable Services és Reliable Actors hívásokat.</span><span class="sxs-lookup"><span data-stu-id="b4c42-183">**No specific protocol:**  If you don't have a particular choice of communication framework, but you want tooget something up and running quickly, then hello ideal option for you is [service remoting](service-fabric-reliable-services-communication-remoting.md), which allows strongly-typed remote procedure calls for Reliable Services and Reliable Actors.</span></span> <span data-ttu-id="b4c42-184">Ez a legegyszerűbb hello és leggyorsabb módon tooget használatába a szolgáltatások közötti kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="b4c42-184">This is hello easiest and fastest way tooget started with service communication.</span></span> <span data-ttu-id="b4c42-185">Szolgáltatás távoli eljáráshívási szolgáltatás címek, a kapcsolat, az újra gombra és a hibakezelés feloldását végzi.</span><span class="sxs-lookup"><span data-stu-id="b4c42-185">Service remoting handles resolution of service addresses, connection, retry, and error handling.</span></span> <span data-ttu-id="b4c42-186">Ez az a C# és a Java-alkalmazások számára érhető el.</span><span class="sxs-lookup"><span data-stu-id="b4c42-186">This is available for both C# and Java applications.</span></span>
* <span data-ttu-id="b4c42-187">**HTTP**: nyelvtől független kommunikációhoz HTTP biztosít egy szabványos választott nyelven is elérhető számos különböző, a Service Fabric által támogatott HTTP-kiszolgálók és eszközök.</span><span class="sxs-lookup"><span data-stu-id="b4c42-187">**HTTP**: For language-agnostic communication, HTTP provides an industry-standard choice with tools and HTTP servers available in many different languages, all supported by Service Fabric.</span></span> <span data-ttu-id="b4c42-188">Szolgáltatások bármely érhető el, beleértve a HTTP-verem használható [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) C#-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b4c42-188">Services can use any HTTP stack available, including [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) for C# applications.</span></span> <span data-ttu-id="b4c42-189">C# nyelven írt ügyfelek kihasználhatják a hello `ICommunicationClient` és `ServicePartitionClient` osztályokat, mivel a Java, használja a hello `CommunicationClient` és `FabricServicePartitionClient` osztályok, [névfeloldási szolgáltatás, a HTTP-kapcsolatoknál és az újrapróbálkozási ciklusok](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="b4c42-189">Clients written in C# can leverage hello `ICommunicationClient` and `ServicePartitionClient` classes, whereas for Java, use hello `CommunicationClient` and `FabricServicePartitionClient` classes, [for service resolution, HTTP connections, and retry loops](service-fabric-reliable-services-communication.md).</span></span>
* <span data-ttu-id="b4c42-190">**WCF**: Ha van, a kommunikáció keretrendszer WCF használó meglévő kódja, akkor használhatja a hello `WcfCommunicationListener` hello kiszolgálóoldali és `WcfCommunicationClient` és `ServicePartitionClient` osztályok hello ügyfél számára.</span><span class="sxs-lookup"><span data-stu-id="b4c42-190">**WCF**: If you have existing code that uses WCF as your communication framework, then you can use hello `WcfCommunicationListener` for hello server side and `WcfCommunicationClient` and `ServicePartitionClient` classes for hello client.</span></span> <span data-ttu-id="b4c42-191">Ez azonban lehetőség csak a C#-alkalmazást a Windows-alapú fürtök.</span><span class="sxs-lookup"><span data-stu-id="b4c42-191">This however is only available for C# applications on Windows based clusters.</span></span> <span data-ttu-id="b4c42-192">További részletekért lásd: Ez a cikk [WCF-alapú megvalósítás hello kommunikációs verem](service-fabric-reliable-services-communication-wcf.md).</span><span class="sxs-lookup"><span data-stu-id="b4c42-192">For more details, see this article about [WCF-based implementation of hello communication stack](service-fabric-reliable-services-communication-wcf.md).</span></span>

## <a name="using-custom-protocols-and-other-communication-frameworks"></a><span data-ttu-id="b4c42-193">Egyéni protokollok és egyéb kommunikációs keretrendszerek használatával</span><span class="sxs-lookup"><span data-stu-id="b4c42-193">Using custom protocols and other communication frameworks</span></span>
<span data-ttu-id="b4c42-194">Szolgáltatások használhatnak minden protokoll vagy keretrendszer kommunikációhoz, hogy egy egyéni bináris protokoll TCP-szoftvercsatornák vagy az esemény streamelését keresztül-e [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) vagy [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="b4c42-194">Services can use any protocol or framework for communication, whether its a custom binary protocol over TCP sockets, or streaming events through [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) or [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/).</span></span> <span data-ttu-id="b4c42-195">A Service Fabric biztosít a kommunikációs API-t csatlakoztathatja a kommunikációs verem be, amíg minden hello toodiscover működik, és csatlakozzon az Ön kiveszik.</span><span class="sxs-lookup"><span data-stu-id="b4c42-195">Service Fabric provides communication APIs that you can plug your communication stack into, while all hello work toodiscover and connect is abstracted from you.</span></span> <span data-ttu-id="b4c42-196">Ebben a cikkben találhat kapcsolatos hello [megbízható kommunikáció modell](service-fabric-reliable-services-communication.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="b4c42-196">See this article about hello [Reliable Service communication model](service-fabric-reliable-services-communication.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4c42-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4c42-197">Next steps</span></span>
<span data-ttu-id="b4c42-198">További információ a hello fogalmakat és API-k hello elérhető további [Reliable Services kommunikációs modellt](service-fabric-reliable-services-communication.md), majd használatának gyors megkezdése [szolgáltatás távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md) vagy mélyreható toolearn hogyan toowrite egy kommunikációs figyelővel [Web API-t önálló gazdagép OWIN](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="b4c42-198">Learn more about hello concepts and APIs available in hello [Reliable Services communication model](service-fabric-reliable-services-communication.md), then get started quickly with [service remoting](service-fabric-reliable-services-communication-remoting.md) or go in-depth toolearn how toowrite a communication listener using [Web API with OWIN self-host](service-fabric-reliable-services-communication-webapi.md).</span></span>

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
