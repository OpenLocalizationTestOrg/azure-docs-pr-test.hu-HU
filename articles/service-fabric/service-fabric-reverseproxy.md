---
title: "a Service Fabric aaaAzure fordított proxy |} Microsoft Docs"
description: "A Service Fabric fordított proxy használata a belső és külső hello fürt kommunikációs toomicroservices."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: bharatn
ms.openlocfilehash: 0e7835a64ccd74293c7bdd8b41deae414c83dffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reverse-proxy-in-azure-service-fabric"></a><span data-ttu-id="9646d-103">Az Azure Service Fabric fordított proxy</span><span class="sxs-lookup"><span data-stu-id="9646d-103">Reverse proxy in Azure Service Fabric</span></span>
<span data-ttu-id="9646d-104">fordított proxy hello Azure Service Fabric beépített címek hello Service Fabric-fürt, amely elérhetővé teszi a HTTP-végpontokról mikroszolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9646d-104">hello reverse proxy that's built into Azure Service Fabric addresses microservices in hello Service Fabric cluster that exposes HTTP endpoints.</span></span>

## <a name="microservices-communication-model"></a><span data-ttu-id="9646d-105">Mikroszolgáltatások kommunikációs modellt</span><span class="sxs-lookup"><span data-stu-id="9646d-105">Microservices communication model</span></span>
<span data-ttu-id="9646d-106">A Service Fabric Mikroszolgáltatások általában futtassa a hello fürtön lévő virtuális gépek egy részét, és áthelyezheti egy virtuális gép tooanother különböző okokból.</span><span class="sxs-lookup"><span data-stu-id="9646d-106">Microservices in Service Fabric typically run on a subset of virtual machines in hello cluster and can move from one virtual machine tooanother for various reasons.</span></span> <span data-ttu-id="9646d-107">Igen dinamikusan módosítható hello végpontjainak mikroszolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9646d-107">So, hello endpoints for microservices can change dynamically.</span></span> <span data-ttu-id="9646d-108">a következő hello jellegzetes toocommunicate toohello mikroszolgáltatási hello következő oldja meg a hurok:</span><span class="sxs-lookup"><span data-stu-id="9646d-108">hello typical pattern toocommunicate toohello microservice is hello following resolve loop:</span></span>

1. <span data-ttu-id="9646d-109">Hárítsa el a hello szolgáltatás helyét kezdetben hello elnevezési szolgáltatáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="9646d-109">Resolve hello service location initially through hello naming service.</span></span>
2. <span data-ttu-id="9646d-110">Connect toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9646d-110">Connect toohello service.</span></span>
3. <span data-ttu-id="9646d-111">Kapcsolódási hibák hello okának megállapításához, és újra feloldása hello szolgáltatáskeresésre, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="9646d-111">Determine hello cause of connection failures, and resolve hello service location again when necessary.</span></span>

<span data-ttu-id="9646d-112">Ez a folyamat általában ügyféloldali kommunikációs szalagtárak újrapróbálkozási hurokba került, amely megvalósítja az hello szolgáltatás megoldás, és próbálkozzon újra a házirendek alkalmazásburkoló foglal magában.</span><span class="sxs-lookup"><span data-stu-id="9646d-112">This process generally involves wrapping client-side communication libraries in a retry loop that implements hello service resolution and retry policies.</span></span>
<span data-ttu-id="9646d-113">További információkért lásd: [Connect és szolgáltatásokkal kommunikálni](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="9646d-113">For more information, see [Connect and communicate with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

### <a name="communicating-by-using-hello-reverse-proxy"></a><span data-ttu-id="9646d-114">Hello fordított proxy segítségével történő kommunikációhoz</span><span class="sxs-lookup"><span data-stu-id="9646d-114">Communicating by using hello reverse proxy</span></span>
<span data-ttu-id="9646d-115">fordított proxy hello a Service Fabric hello fürt összes csomópontjának hello futtatja.</span><span class="sxs-lookup"><span data-stu-id="9646d-115">hello reverse proxy in Service Fabric runs on all hello nodes in hello cluster.</span></span> <span data-ttu-id="9646d-116">Hello teljes szolgáltatási névfeloldási folyamat az ügyfél nevében végez, és ezután továbbítja a hello ügyfélkérés.</span><span class="sxs-lookup"><span data-stu-id="9646d-116">It performs hello entire service resolution process on a client's behalf and then forwards hello client request.</span></span> <span data-ttu-id="9646d-117">Igen hello fürtön futó bármely ügyféloldali HTTP-kommunikáció szalagtárak tootalk toohello célként megadott szolgáltatás mezővel használhat hello fordított proxy, hogy fut a helyi hello ugyanahhoz a csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="9646d-117">So, clients that run on hello cluster can use any client-side HTTP communication libraries tootalk toohello target service by using hello reverse proxy that runs locally on hello same node.</span></span>

![Belső kommunikációs][1]

## <a name="reaching-microservices-from-outside-hello-cluster"></a><span data-ttu-id="9646d-119">Külső hello fürtből mikroszolgáltatások elérése</span><span class="sxs-lookup"><span data-stu-id="9646d-119">Reaching microservices from outside hello cluster</span></span>
<span data-ttu-id="9646d-120">hello alapértelmezett külső kommunikáció modell mikroszolgáltatások egy egy választható modell, ahol minden szolgáltatás nem érhető el közvetlenül a külső ügyfeleknek.</span><span class="sxs-lookup"><span data-stu-id="9646d-120">hello default external communication model for microservices is an opt-in model where each service cannot be accessed directly from external clients.</span></span> <span data-ttu-id="9646d-121">[Az Azure Load Balancer](../load-balancer/load-balancer-overview.md), ez egy olyan hálózathatárhoz mikroszolgáltatások létrehozására és a külső ügyfelek közötti hálózati címfordítás végez, és továbbítja külső kérelmek toointernal IP:port végpontok.</span><span class="sxs-lookup"><span data-stu-id="9646d-121">[Azure Load Balancer](../load-balancer/load-balancer-overview.md), which is a network boundary between microservices and external clients, performs network address translation and forwards external requests toointernal IP:port endpoints.</span></span> <span data-ttu-id="9646d-122">toomake egy mikroszolgáltatási végpont közvetlenül elérhető tooexternal ügyfelek, először konfigurálnia kell tooforward forgalom tooeach portot, amelyet hello szolgáltatás használ a Load Balancer hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="9646d-122">toomake a microservice's endpoint directly accessible tooexternal clients, you must first configure Load Balancer tooforward traffic tooeach port that hello service uses in hello cluster.</span></span> <span data-ttu-id="9646d-123">Továbbá a legtöbb mikroszolgáltatások, különösen akkor állapot-nyilvántartó mikroszolgáltatások nem élő hello fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="9646d-123">Furthermore, most microservices, especially stateful microservices, don't live on all nodes of hello cluster.</span></span> <span data-ttu-id="9646d-124">hello mikroszolgáltatások áthelyezheti a feladatátvételi csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="9646d-124">hello microservices can move between nodes on failover.</span></span> <span data-ttu-id="9646d-125">Ebben az esetben, Load Balancer nem hatékony hello helyének meghatározásához hello cél csomópontjának hello replikák toowhich továbbítsa a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="9646d-125">In such cases, Load Balancer cannot effectively determine hello location of hello target node of hello replicas toowhich it should forward traffic.</span></span>

### <a name="reaching-microservices-via-hello-reverse-proxy-from-outside-hello-cluster"></a><span data-ttu-id="9646d-126">Mikroszolgáltatások külső hello fürtből a hello fordított proxyn keresztül történő elérése</span><span class="sxs-lookup"><span data-stu-id="9646d-126">Reaching microservices via hello reverse proxy from outside hello cluster</span></span>
<span data-ttu-id="9646d-127">Egy szolgáltatás portja hello konfigurálására a terheléselosztóhoz, port is beállítható csak hello hello fordított proxy a terheléselosztó hasonló adataival.</span><span class="sxs-lookup"><span data-stu-id="9646d-127">Instead of configuring hello port of an individual service in Load Balancer, you can configure just hello port of hello reverse proxy in Load Balancer.</span></span> <span data-ttu-id="9646d-128">Ez a konfiguráció lehetővé teszi az ügyfelek számára hello fürtön kívüli hello fordított proxy további konfiguráció nélkül használatával hello fürtben található szolgáltatások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9646d-128">This configuration lets clients outside hello cluster reach services inside hello cluster by using hello reverse proxy without additional configuration.</span></span>

![Külső kommunikáció][0]

> [!WARNING]
> <span data-ttu-id="9646d-130">Hello fordított proxy portja a Load Balancer konfigurálásakor hello fürt összes HTTP-végponttal visszaállítását mikroszolgáltatások hello fürtön kívüli megcímezhető.</span><span class="sxs-lookup"><span data-stu-id="9646d-130">When you configure hello reverse proxy's port in Load Balancer, all microservices in hello cluster that expose an HTTP endpoint are addressable from outside hello cluster.</span></span>
>
>


## <a name="uri-format-for-addressing-services-by-using-hello-reverse-proxy"></a><span data-ttu-id="9646d-131">Szolgáltatások címzéshez hello fordított proxy használatával az URI-formátum</span><span class="sxs-lookup"><span data-stu-id="9646d-131">URI format for addressing services by using hello reverse proxy</span></span>
<span data-ttu-id="9646d-132">fordított proxy használ a megadott egységes erőforrás-azonosító (URI) formátumban tooidentify hello partíció toowhich hello bejövő szolgáltatáskérés továbbítani hello:</span><span class="sxs-lookup"><span data-stu-id="9646d-132">hello reverse proxy uses a specific uniform resource identifier (URI) format tooidentify hello service partition toowhich hello incoming request should be forwarded:</span></span>

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* <span data-ttu-id="9646d-133">**http (s):** hello fordított proxy lehet konfigurált tooaccept HTTP vagy HTTPS-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="9646d-133">**http(s):** hello reverse proxy can be configured tooaccept HTTP or HTTPS traffic.</span></span> <span data-ttu-id="9646d-134">HTTPS-továbbítást, tekintse meg túl[tooa biztonságos szolgáltatás kapcsolattartásnak hello fordított proxy](service-fabric-reverseproxy-configure-secure-communication.md) után fordított proxy telepítő toolisten HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9646d-134">For HTTPS forwarding, refer too[Connect tooa secure service with hello reverse proxy](service-fabric-reverseproxy-configure-secure-communication.md) once you have reverse proxy setup toolisten on HTTPS.</span></span>
* <span data-ttu-id="9646d-135">**Fürt teljesen minősített tartománynevét (FQDN) |} belső IP:** külső ügyfelek esetében konfigurálhatja hello fordított proxy úgy, hogy hello fürt tartományban, például mycluster.eastus.cloudapp.azure.com keresztül érhető el. Alapértelmezés szerint a hello fordított proxy minden csomóponton fut.</span><span class="sxs-lookup"><span data-stu-id="9646d-135">**Cluster fully qualified domain name (FQDN) | internal IP:** For external clients, you can configure hello reverse proxy so that it is reachable through hello cluster domain, such as mycluster.eastus.cloudapp.azure.com. By default, hello reverse proxy runs on every node.</span></span> <span data-ttu-id="9646d-136">A belső forgalom hello fordított proxy elérhető localhost vagy minden csomópont belső IP-, például 10.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="9646d-136">For internal traffic, hello reverse proxy can be reached on localhost or on any internal node IP, such as 10.0.0.1.</span></span>
* <span data-ttu-id="9646d-137">**Port:** hello portja, például a 19081, amely hello fordított proxy lett meghatározva.</span><span class="sxs-lookup"><span data-stu-id="9646d-137">**Port:** This is hello port, such as 19081, that has been specified for hello reverse proxy.</span></span>
* <span data-ttu-id="9646d-138">**ServiceInstanceName:** tooreach hello nélkül próbálja telepíteni hello szolgáltatáspéldány hello teljesen minősített neve "fabric: /" sémával.</span><span class="sxs-lookup"><span data-stu-id="9646d-138">**ServiceInstanceName:** This is hello fully-qualified name of hello deployed service instance that you are trying tooreach without hello "fabric:/" scheme.</span></span> <span data-ttu-id="9646d-139">Például tooreach hello *fabric: / myapp/myservice/* szolgáltatást szeretné használni *myapp/myservice*.</span><span class="sxs-lookup"><span data-stu-id="9646d-139">For example, tooreach hello *fabric:/myapp/myservice/* service, you would use *myapp/myservice*.</span></span>

    <span data-ttu-id="9646d-140">hello szolgáltatás példány neve megkülönbözteti a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="9646d-140">hello service instance name is case-sensitive.</span></span> <span data-ttu-id="9646d-141">Egy másik kis-és nagybetűk használata hello szolgáltatás neve hello URL-címben azt eredményezi, hello kérelmek toofail a 404-es (nem található).</span><span class="sxs-lookup"><span data-stu-id="9646d-141">Using a different casing for hello service instance name in hello URL causes hello requests toofail with 404 (Not Found).</span></span>
* <span data-ttu-id="9646d-142">**Utótag elérési út:** Ez az hello tényleges URL-címe, mint például az *myapi/értékek/hozzáadása/3*, tooconnect a kívánt hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9646d-142">**Suffix path:** This is hello actual URL path, such as *myapi/values/add/3*, for hello service that you want tooconnect to.</span></span>
* <span data-ttu-id="9646d-143">**PartitionKey:** egy particionált szolgáltatást, ez pedig hello számított partíciós kulcs, amelyet az tooreach hello partíció.</span><span class="sxs-lookup"><span data-stu-id="9646d-143">**PartitionKey:** For a partitioned service, this is hello computed partition key of hello partition that you want tooreach.</span></span> <span data-ttu-id="9646d-144">Vegye figyelembe, hogy ez *nem* hello partíció GUID azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9646d-144">Note that this is *not* hello partition ID GUID.</span></span> <span data-ttu-id="9646d-145">Ez a paraméter nincs szükség a hello egypéldányos partícióséma használó szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="9646d-145">This parameter is not required for services that use hello singleton partition scheme.</span></span>
* <span data-ttu-id="9646d-146">**PartitionKind:** hello szolgáltatás partícióséma azt.</span><span class="sxs-lookup"><span data-stu-id="9646d-146">**PartitionKind:** This is hello service partition scheme.</span></span> <span data-ttu-id="9646d-147">Ez lehet "Int64Range" vagy "Nevű".</span><span class="sxs-lookup"><span data-stu-id="9646d-147">This can be 'Int64Range' or 'Named'.</span></span> <span data-ttu-id="9646d-148">Ez a paraméter nincs szükség a hello egypéldányos partícióséma használó szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="9646d-148">This parameter is not required for services that use hello singleton partition scheme.</span></span>
* <span data-ttu-id="9646d-149">**ListenerName** hello szolgáltatás hello végpontok hello formában vannak {"Végpont": {"Listener1": "Endpoint1", "Listener2": "Endpoint2" …}}.</span><span class="sxs-lookup"><span data-stu-id="9646d-149">**ListenerName** hello endpoints from hello service are of hello form {"Endpoints":{"Listener1":"Endpoint1","Listener2":"Endpoint2" ...}}.</span></span> <span data-ttu-id="9646d-150">Amikor hello szolgáltatás elérhetővé teszi a több végpontot, hello végpont azonosítja, hogy hello ügyfélkérés továbbítani kell.</span><span class="sxs-lookup"><span data-stu-id="9646d-150">When hello service exposes multiple endpoints, this identifies hello endpoint that hello client request should be forwarded to.</span></span> <span data-ttu-id="9646d-151">Ez csak egy figyelő hello szolgáltatás-e elhagyható.</span><span class="sxs-lookup"><span data-stu-id="9646d-151">This can be omitted if hello service has only one listener.</span></span>
* <span data-ttu-id="9646d-152">**TargetReplicaSelector** azt határozza meg, hogyan hello cél replika- vagy kell kiválasztani.</span><span class="sxs-lookup"><span data-stu-id="9646d-152">**TargetReplicaSelector** This specifies how hello target replica or instance should be selected.</span></span>
  * <span data-ttu-id="9646d-153">Állapot-nyilvántartó hello célszolgáltatás esetén hello TargetReplicaSelector hello következő lehet: "PrimaryReplica", "RandomSecondaryReplica" vagy "RandomReplica".</span><span class="sxs-lookup"><span data-stu-id="9646d-153">When hello target service is stateful, hello TargetReplicaSelector can be one of hello following:  'PrimaryReplica', 'RandomSecondaryReplica', or 'RandomReplica'.</span></span> <span data-ttu-id="9646d-154">Ha ez a paraméter nincs megadva, a hello alapértelmezett érték "PrimaryReplica".</span><span class="sxs-lookup"><span data-stu-id="9646d-154">When this parameter is not specified, hello default is 'PrimaryReplica'.</span></span>
  * <span data-ttu-id="9646d-155">Állapot nélküli hello célként megadott szolgáltatás esetén a fordított proxy szerzi hello szolgáltatás partíció tooforward hello kérelem véletlenszerű példányát.</span><span class="sxs-lookup"><span data-stu-id="9646d-155">When hello target service is stateless, reverse proxy picks a random instance of hello service partition tooforward hello request to.</span></span>
* <span data-ttu-id="9646d-156">**Időtúllépés:** azt határozza meg, hogy hello időtúllépés hello HTTP-kérelem hello fordított proxy toohello szolgáltatás nevében hello ügyfél kérelmet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9646d-156">**Timeout:**  This specifies hello timeout for hello HTTP request created by hello reverse proxy toohello service on behalf of hello client request.</span></span> <span data-ttu-id="9646d-157">hello alapértelmezett értéke 60 másodperc.</span><span class="sxs-lookup"><span data-stu-id="9646d-157">hello default value is 60 seconds.</span></span> <span data-ttu-id="9646d-158">Ez nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="9646d-158">This is an optional parameter.</span></span>

### <a name="example-usage"></a><span data-ttu-id="9646d-159">Példa használati</span><span class="sxs-lookup"><span data-stu-id="9646d-159">Example usage</span></span>
<span data-ttu-id="9646d-160">Példaként vegyünk hello *fabric: / MyApp/MyService* , amely megnyitja a hello URL-cím a következő HTTP-figyelő szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="9646d-160">As an example, let's take hello *fabric:/MyApp/MyService* service that opens an HTTP listener on hello following URL:</span></span>

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

<span data-ttu-id="9646d-161">Az alábbiakban hello erőforrásait hello szolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="9646d-161">Following are hello resources for hello service:</span></span>

* `/index.html`
* `/api/users/<userId>`

<span data-ttu-id="9646d-162">Hello szolgáltatás hello egypéldányos particionálási sémát használja, ha hello *PartitionKey* és *PartitionKind* lekérdezési karakterlánc paraméterek esetén nincs szükség, és hello szolgáltatás elérhetőségének hello átjáró mint használatával:</span><span class="sxs-lookup"><span data-stu-id="9646d-162">If hello service uses hello singleton partitioning scheme, hello *PartitionKey* and *PartitionKind* query string parameters are not required, and hello service can be reached by using hello gateway as:</span></span>

* <span data-ttu-id="9646d-163">Külsőleg:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="9646d-163">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`</span></span>
* <span data-ttu-id="9646d-164">Belső:`http://localhost:19081/MyApp/MyService`</span><span class="sxs-lookup"><span data-stu-id="9646d-164">Internally: `http://localhost:19081/MyApp/MyService`</span></span>

<span data-ttu-id="9646d-165">Hello szolgáltatás hello egységes Int64 particionálási sémát használja, ha hello *PartitionKey* és *PartitionKind* lekérdezési karakterlánc-paraméterrel használt tooreach hello szolgáltatás partíciójának kell lennie:</span><span class="sxs-lookup"><span data-stu-id="9646d-165">If hello service uses hello Uniform Int64 partitioning scheme, hello *PartitionKey* and *PartitionKind* query string parameters must be used tooreach a partition of hello service:</span></span>

* <span data-ttu-id="9646d-166">Külsőleg:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="9646d-166">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="9646d-167">Belső:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="9646d-167">Internally: `http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="9646d-168">tooreach hello erőforrások elérhetővé tévő hello szolgáltatást, egyszerűen után hello szolgáltatás neve hello URL-címben helyezze el hello erőforrás elérési útja:</span><span class="sxs-lookup"><span data-stu-id="9646d-168">tooreach hello resources that hello service exposes, simply place hello resource path after hello service name in hello URL:</span></span>

* <span data-ttu-id="9646d-169">Külsőleg:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="9646d-169">Externally: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`</span></span>
* <span data-ttu-id="9646d-170">Belső:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span><span class="sxs-lookup"><span data-stu-id="9646d-170">Internally: `http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`</span></span>

<span data-ttu-id="9646d-171">hello átjáró ezután továbbítja a kérelmek toohello szolgáltatás URL-címe:</span><span class="sxs-lookup"><span data-stu-id="9646d-171">hello gateway will then forward these requests toohello service's URL:</span></span>

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a><span data-ttu-id="9646d-172">A port megosztása különleges kezelést szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9646d-172">Special handling for port-sharing services</span></span>
<span data-ttu-id="9646d-173">Azure Application Gateway megpróbál tooresolve szolgáltatás cím újra, majd próbálja megismételni a hello kérelem, ha a szolgáltatás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="9646d-173">Azure Application Gateway attempts tooresolve a service address again and retry hello request when a service cannot be reached.</span></span> <span data-ttu-id="9646d-174">Ennek oka egy fő előnye, hogy az Alkalmazásátjáró Ügyfélkód kell tooimplement saját service megoldás, és nem oldja meg a hurok.</span><span class="sxs-lookup"><span data-stu-id="9646d-174">This is a major benefit of Application Gateway because client code does not need tooimplement its own service resolution and resolve loop.</span></span>

<span data-ttu-id="9646d-175">Általában a szolgáltatás nem érhető el, amikor hello szolgáltatáspéldány vagy replika áthelyezte tooa másik csomópont, a normál életciklusának.</span><span class="sxs-lookup"><span data-stu-id="9646d-175">Generally, when a service cannot be reached, hello service instance or replica has moved tooa different node as part of its normal lifecycle.</span></span> <span data-ttu-id="9646d-176">Ez akkor fordul elő, amikor Alkalmazásátjáró fordulhat elő, egy hálózati kapcsolat hiba arról, hogy a végpont nem hosszabb nyissa meg a hello eredetileg feloldva cím.</span><span class="sxs-lookup"><span data-stu-id="9646d-176">When this happens, Application Gateway might receive a network connection error indicating that an endpoint is no longer open on hello originally resolved address.</span></span>

<span data-ttu-id="9646d-177">Azonban replikák és a szolgáltatáspéldány egy gazdafolyamaton megoszthatnak, és előfordulhat, hogy is megoszthat egy portot, ha azt egy http.sys alapú webkiszolgálóhoz, beleértve:</span><span class="sxs-lookup"><span data-stu-id="9646d-177">However, replicas or service instances can share a host process and might also share a port when hosted by an http.sys-based web server, including:</span></span>

* [<span data-ttu-id="9646d-178">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="9646d-178">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [<span data-ttu-id="9646d-179">Az ASP.NET Core WebListener</span><span class="sxs-lookup"><span data-stu-id="9646d-179">ASP.NET Core WebListener</span></span>](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [<span data-ttu-id="9646d-180">Katana</span><span class="sxs-lookup"><span data-stu-id="9646d-180">Katana</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

<span data-ttu-id="9646d-181">Ebben az esetben valószínű hello webkiszolgálóra hello gazdafolyamat és érhető toorequests, de hello feloldása szolgáltatáspéldány, és a replika már nem érhető el a gazdagépen hello válaszol.</span><span class="sxs-lookup"><span data-stu-id="9646d-181">In this situation, it is likely that hello web server is available in hello host process and responding toorequests, but hello resolved service instance or replica is no longer available on hello host.</span></span> <span data-ttu-id="9646d-182">Ebben az esetben hello átjáró kap egy HTTP 404-es választ hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="9646d-182">In this case, hello gateway will receive an HTTP 404 response from hello web server.</span></span> <span data-ttu-id="9646d-183">HTTP 404-es így két különböző jelentése van:</span><span class="sxs-lookup"><span data-stu-id="9646d-183">Thus, an HTTP 404 has two distinct meanings:</span></span>

- <span data-ttu-id="9646d-184">#1. eset: hello szolgáltatás címe helyes, de, amely a kért felhasználót hello hello erőforrás nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9646d-184">Case #1: hello service address is correct, but hello resource that hello user requested does not exist.</span></span>
- <span data-ttu-id="9646d-185">#2. eset: hello szolgáltatás címe nem megfelelő, és, hogy a felhasználó hello hello erőforrás előfordulhat, hogy létezik egy másik csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="9646d-185">Case #2: hello service address is incorrect, and hello resource that hello user requested might exist on a different node.</span></span>

<span data-ttu-id="9646d-186">hello első esetben egy normál HTTP 404-es, amely felhasználói hiba történt.</span><span class="sxs-lookup"><span data-stu-id="9646d-186">hello first case is a normal HTTP 404, which is considered a user error.</span></span> <span data-ttu-id="9646d-187">Hello második esetben azonban hello felhasználói kérte a erőforrása, amely létezik.</span><span class="sxs-lookup"><span data-stu-id="9646d-187">However, in hello second case, hello user has requested a resource that does exist.</span></span> <span data-ttu-id="9646d-188">Alkalmazásátjáró nem toolocate, mert hello szolgáltatás maga átkerült volt.</span><span class="sxs-lookup"><span data-stu-id="9646d-188">Application Gateway was unable toolocate it because hello service itself has moved.</span></span> <span data-ttu-id="9646d-189">Alkalmazás igényeinek tooresolve hello átjárócímet újra, és ismételje meg a hello kérést.</span><span class="sxs-lookup"><span data-stu-id="9646d-189">Application Gateway needs tooresolve hello address again and retry hello request.</span></span>

<span data-ttu-id="9646d-190">Alkalmazásátjáró így kell egy módon toodistinguish két esetben között.</span><span class="sxs-lookup"><span data-stu-id="9646d-190">Application Gateway thus needs a way toodistinguish between these two cases.</span></span> <span data-ttu-id="9646d-191">toomake, hogy megkülönböztetés, egy hello kiszolgálóról megadása szükséges.</span><span class="sxs-lookup"><span data-stu-id="9646d-191">toomake that distinction, a hint from hello server is required.</span></span>

* <span data-ttu-id="9646d-192">Alapértelmezés szerint az Application Gateway azt feltételezi, hogy a #2. eset, és újra megpróbál tooresolve és probléma hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="9646d-192">By default, Application Gateway assumes case #2 and attempts tooresolve and issue hello request again.</span></span>
* <span data-ttu-id="9646d-193">tooindicate #1. eset tooApplication átjáró hello szolgáltatást a következő HTTP-válaszfejléc hello kell visszaadnia:</span><span class="sxs-lookup"><span data-stu-id="9646d-193">tooindicate case #1 tooApplication Gateway, hello service should return hello following HTTP response header:</span></span>

  `X-ServiceFabric : ResourceNotFound`

<span data-ttu-id="9646d-194">A HTTP-válaszfejléc jelzi egy normál HTTP 404-es helyzetben melyik hello a kért erőforrás nem létezik, és Alkalmazásátjáró nem kísérli meg tooresolve hello szolgáltatás címe újra.</span><span class="sxs-lookup"><span data-stu-id="9646d-194">This HTTP response header indicates a normal HTTP 404 situation in which hello requested resource does not exist, and Application Gateway will not attempt tooresolve hello service address again.</span></span>

## <a name="setup-and-configuration"></a><span data-ttu-id="9646d-195">Beállítás és konfiguráció</span><span class="sxs-lookup"><span data-stu-id="9646d-195">Setup and configuration</span></span>

### <a name="enable-reverse-proxy-via-azure-portal"></a><span data-ttu-id="9646d-196">Azure-portálon fordított proxy engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9646d-196">Enable reverse proxy via Azure portal</span></span>

<span data-ttu-id="9646d-197">Azure portál egy beállítás tooenable fordított proxy biztosít egy új Service Fabric-fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="9646d-197">Azure portal provides an option tooenable Reverse proxy while creating a new Service Fabric cluster.</span></span>
<span data-ttu-id="9646d-198">A **létrehozása a Service Fabric-fürt**, 2. lépés: fürtkonfiguráció esetén a csomópont-konfiguráció típusát, jelölje be a hello négyzetet túl "Enable fordított proxy".</span><span class="sxs-lookup"><span data-stu-id="9646d-198">Under **Create Service Fabric cluster**, Step 2: Cluster Configuration, Node type configuration, select hello checkbox too"Enable reverse proxy".</span></span>
<span data-ttu-id="9646d-199">Biztonságos fordított proxy konfigurálása az SSL-tanúsítvány adható meg a 3. lépés: biztonsági, fürt biztonsági beállításainak konfigurálása, jelölje be hello jelölőnégyzet túl "Közé tartozik egy SSL-tanúsítvány a fordított proxyhoz", és írja be a hello tanúsítvány adatait.</span><span class="sxs-lookup"><span data-stu-id="9646d-199">For configuring secure reverse proxy, SSL certificate can be specified in Step 3: Security, Configure cluster security settings, select hello checkbox too"Include a SSL certificate for reverse proxy" and enter hello certificate details.</span></span>

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a><span data-ttu-id="9646d-200">Fordított proxyn keresztül Azure Resource Manager-sablonok engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9646d-200">Enable reverse proxy via Azure Resource Manager templates</span></span>

<span data-ttu-id="9646d-201">Használhatja a hello [Azure Resource Manager sablon](service-fabric-cluster-creation-via-arm.md) tooenable hello fordított proxy a Service Fabric hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="9646d-201">You can use hello [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) tooenable hello reverse proxy in Service Fabric for hello cluster.</span></span>

<span data-ttu-id="9646d-202">Tekintse meg a túl[beállítása HTTPS fordított Proxy biztonságos fürtben](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) az Azure Resource Manager-sablon minták tooconfigure biztonságos fordított proxy tanúsítvány és kezelési tanúsítványok leváltása.</span><span class="sxs-lookup"><span data-stu-id="9646d-202">Refer too[Configure HTTPS Reverse Proxy in a secure cluster](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) for Azure Resource Manager template samples tooconfigure secure reverse proxy with a certificate and handling certificate rollover.</span></span>

<span data-ttu-id="9646d-203">Első lépésként elérhetővé hello sablon hello fürt, amelyet az toodeploy.</span><span class="sxs-lookup"><span data-stu-id="9646d-203">First, you get hello template for hello cluster that you want toodeploy.</span></span> <span data-ttu-id="9646d-204">Hello minta sablonok, vagy hozzon létre egy egyéni erőforrás-kezelő sablont.</span><span class="sxs-lookup"><span data-stu-id="9646d-204">You can either use hello sample templates or create a custom Resource Manager template.</span></span> <span data-ttu-id="9646d-205">Ezt követően engedélyezheti hello fordított proxy hello lépések segítségével:</span><span class="sxs-lookup"><span data-stu-id="9646d-205">Then, you can enable hello reverse proxy by using hello following steps:</span></span>

1. <span data-ttu-id="9646d-206">Adjon meg egy hello fordított proxy portja hello [paraméterek szakaszban](../azure-resource-manager/resource-group-authoring-templates.md) hello sablon.</span><span class="sxs-lookup"><span data-stu-id="9646d-206">Define a port for hello reverse proxy in hello [Parameters section](../azure-resource-manager/resource-group-authoring-templates.md) of hello template.</span></span>

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. <span data-ttu-id="9646d-207">Adja meg a hello portot az egyes hello nodetype objektumok hello **fürt** [erőforrás típushoz című rész](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9646d-207">Specify hello port for each of hello nodetype objects in hello **Cluster** [Resource type section](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

    <span data-ttu-id="9646d-208">hello port hello paraméter neve, reverseProxyEndpointPort azonosít.</span><span class="sxs-lookup"><span data-stu-id="9646d-208">hello port is identified by hello parameter name, reverseProxyEndpointPort.</span></span>

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. <span data-ttu-id="9646d-209">tooaddress hello fordított proxy a külső hello Azure-fürttel, az 1. lépésben megadott hello port hello Azure Load Balancer szabályokat.</span><span class="sxs-lookup"><span data-stu-id="9646d-209">tooaddress hello reverse proxy from outside hello Azure cluster, set up hello Azure Load Balancer rules for hello port that you specified in step 1.</span></span>

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. <span data-ttu-id="9646d-210">SSL-tanúsítványok tooconfigure hello porton hello fordított proxy, vegye fel a hello tanúsítvány toohello ***reverseProxyCertificate*** hello tulajdonság **fürt** [erőforrás típushoz című rész](../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9646d-210">tooconfigure SSL certificates on hello port for hello reverse proxy, add hello certificate toohello ***reverseProxyCertificate*** property in hello **Cluster** [Resource type section](../resource-group-authoring-templates.md).</span></span>

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-hello-cluster-certificate"></a><span data-ttu-id="9646d-211">A fordított proxy tanúsítvány, amely eltér a fürt tanúsítvány hello támogatása</span><span class="sxs-lookup"><span data-stu-id="9646d-211">Supporting a reverse proxy certificate that's different from hello cluster certificate</span></span>
 <span data-ttu-id="9646d-212">Ha hello fordított proxy tanúsítvány nem azonos a hello tanúsítvány, amely biztosítja a hello fürt, majd hello korábban megadott tanúsítvány hello virtuális gépen kell telepíteni, és hozzá toohello hozzáférés-vezérlési lista (ACL), hogy a Service Fabric is -e férni.</span><span class="sxs-lookup"><span data-stu-id="9646d-212">If hello reverse proxy certificate is different from hello certificate that secures hello cluster, then hello previously specified certificate should be installed on hello virtual machine and added toohello access control list (ACL) so that Service Fabric can access it.</span></span> <span data-ttu-id="9646d-213">Ezt megteheti a hello **virtualMachineScaleSets** [erőforrás típushoz című rész](../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9646d-213">This can be done in hello **virtualMachineScaleSets** [Resource type section](../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="9646d-214">A telepítéshez adja hozzá az adott tanúsítvány toohello osProfile.</span><span class="sxs-lookup"><span data-stu-id="9646d-214">For installation, add that certificate toohello osProfile.</span></span> <span data-ttu-id="9646d-215">hello kiterjesztésben megadottaknak hello sablon frissítheti hello hello ACL-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="9646d-215">hello extension section of hello template can update hello certificate in hello ACL.</span></span>

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> <span data-ttu-id="9646d-216">Tanúsítványokat, amelyek eltérnek a hello fürt tanúsítvány tooenable hello fordított proxy egy meglévő fürt használatakor hello fordított proxy tanúsítvány telepítéséhez, és frissítse a hello ACL hello fürtön, hello fordított proxy engedélyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="9646d-216">When you use certificates that are different from hello cluster certificate tooenable hello reverse proxy on an existing cluster, install hello reverse proxy certificate and update hello ACL on hello cluster before you enable hello reverse proxy.</span></span> <span data-ttu-id="9646d-217">Teljes hello [Azure Resource Manager sablon](service-fabric-cluster-creation-via-arm.md) említett hello beállításokkal telepítési korábban egy központi telepítési tooenable hello fordított proxy megkezdése előtt a lépések 1-4.</span><span class="sxs-lookup"><span data-stu-id="9646d-217">Complete hello [Azure Resource Manager template](service-fabric-cluster-creation-via-arm.md) deployment by using hello settings mentioned previously before you start a deployment tooenable hello reverse proxy in steps 1-4.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9646d-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9646d-218">Next steps</span></span>
* <span data-ttu-id="9646d-219">Példa a szolgáltatások közötti HTTP-kommunikációt egy [mintaprojektet a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="9646d-219">See an example of HTTP communication between services in a [sample project on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).</span></span>
* [<span data-ttu-id="9646d-220">A fordított proxy hello toosecure HTTP-szolgáltatás továbbítás</span><span class="sxs-lookup"><span data-stu-id="9646d-220">Forwarding toosecure HTTP service with hello reverse proxy</span></span>](service-fabric-reverseproxy-configure-secure-communication.md)
* [<span data-ttu-id="9646d-221">Távoli eljáráshívások a Reliable Services távoli eljáráshívás</span><span class="sxs-lookup"><span data-stu-id="9646d-221">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="9646d-222">Webes API-t használó OWIN Reliable Services</span><span class="sxs-lookup"><span data-stu-id="9646d-222">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="9646d-223">WCF-kommunikáció Reliable Services használatával</span><span class="sxs-lookup"><span data-stu-id="9646d-223">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
* <span data-ttu-id="9646d-224">A további fordított proxy konfigurációs lehetőségeket, tekintse meg a alapú/Http című [testre szabhatja a Service Fabric-fürt beállítások](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="9646d-224">For additional reverse proxy configuration options, refer ApplicationGateway/Http section in [Customize Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
