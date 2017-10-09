---
title: "a Service Fabric aaaAzure fordított proxy diagnosztika |} Microsoft Docs"
description: "Megtudhatja, hogyan toomonitor és diagnosztizálását hello fordított proxy kérelem feldolgozását."
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: kavyako
ms.openlocfilehash: 9687b9688dc26ba619cbdfab1b1f49a3035345c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-diagnose-request-processing-at-hello-reverse-proxy"></a><span data-ttu-id="07293-103">Figyelése és diagnosztizálása kérelem feldolgozását hello fordított proxy</span><span class="sxs-lookup"><span data-stu-id="07293-103">Monitor and diagnose request processing at hello reverse proxy</span></span>

<span data-ttu-id="07293-104">A Service Fabric hello 5.7 kiadástól kezdve, fordított proxy események állnak rendelkezésre a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="07293-104">Starting with hello 5.7 release of Service Fabric, reverse proxy events are available for collection.</span></span> <span data-ttu-id="07293-105">hello események állnak rendelkezésre a két csatornák toorequest feldolgozási hiba hello fordított proxy és a bejegyzéseket a sikeres és a sikertelen kérelmek részletes eseményeket tartalmazó második csatorna kapcsolatos hibaesemények csak egyet.</span><span class="sxs-lookup"><span data-stu-id="07293-105">hello events are available in two channels, one with only error events related toorequest processing failure at hello reverse proxy and second channel containing verbose events with entries for both successful and failed requests.</span></span>

<span data-ttu-id="07293-106">Tekintse meg a túl[fordított proxy eseményeinek gyűjtése](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) tooenable ezeknek a csatornáknak a helyi és az Azure Service Fabric-fürtök az események gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="07293-106">Refer too[Collect reverse proxy events](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) tooenable collecting events from these channels in local and Azure Service Fabric clusters.</span></span>

## <a name="troubleshoot-using-diagnostics-logs"></a><span data-ttu-id="07293-107">Diagnosztikai naplók segítségével hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="07293-107">Troubleshoot using diagnostics logs</span></span>
<span data-ttu-id="07293-108">Íme néhány példa a hogyan toointerpret hello közös hiba naplókat, hogy egy találkozhat:</span><span class="sxs-lookup"><span data-stu-id="07293-108">Here are some examples on how toointerpret hello common failure logs that one can encounter:</span></span>

1. <span data-ttu-id="07293-109">Fordított proxy válasz állapotkódja 504 (Timeout) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="07293-109">Reverse proxy returns response status code 504 (Timeout).</span></span>

    <span data-ttu-id="07293-110">Egyik oka toohello szolgáltatás leállása tooreply oka lehet hello kérelem időkorláton belül.</span><span class="sxs-lookup"><span data-stu-id="07293-110">One reason could be due toohello service failing tooreply within hello request timeout period.</span></span>
<span data-ttu-id="07293-111">hello első eseményt naplózza a hello kérelem érkezett: hello fordított proxy hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="07293-111">hello first event below logs hello details of hello request received at hello reverse proxy.</span></span> <span data-ttu-id="07293-112">hello második esemény azt jelzi, hogy hello kérelem sikertelen volt továbbítási tooservice, miközben kellő túl "belső hiba = ERROR_WINHTTP_TIMEOUT"</span><span class="sxs-lookup"><span data-stu-id="07293-112">hello second event indicates that hello request failed while forwarding tooservice, due too"internal error = ERROR_WINHTTP_TIMEOUT"</span></span> 

    <span data-ttu-id="07293-113">hello hasznos tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="07293-113">hello payload includes:</span></span>

    *  <span data-ttu-id="07293-114">**traceId**: A GUID lehet használt toocorrelate tooa egy kérelemhez tartozó összes hello eseményt.</span><span class="sxs-lookup"><span data-stu-id="07293-114">**traceId**: This GUID can be used toocorrelate all hello events corresponding tooa single request.</span></span> <span data-ttu-id="07293-115">Hello alább két esemény, a hello traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, ami azt jelenti, toohello tartoznak egyazon kérelemben.</span><span class="sxs-lookup"><span data-stu-id="07293-115">In hello below two events, hello traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, implying they belong toohello same request.</span></span>
    *  <span data-ttu-id="07293-116">**requestUrl**: hello URL-címe (fordított proxy URL-cím) toowhich hello kérés elküldése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="07293-116">**requestUrl**: hello URL (Reverse proxy URL) toowhich hello request was sent.</span></span>
    *  <span data-ttu-id="07293-117">**művelet**: HTTP-műveletet.</span><span class="sxs-lookup"><span data-stu-id="07293-117">**verb**: HTTP verb.</span></span>
    *  <span data-ttu-id="07293-118">**remoteAddress**: hello kérést küldő ügyfél címe.</span><span class="sxs-lookup"><span data-stu-id="07293-118">**remoteAddress**: Address of client sending hello request.</span></span>
    *  <span data-ttu-id="07293-119">**resolvedServiceUrl**: szolgáltatási végpont URL-cím toowhich hello bejövő kérelem lett feloldva.</span><span class="sxs-lookup"><span data-stu-id="07293-119">**resolvedServiceUrl**: Service endpoint URL toowhich hello incoming request was resolved.</span></span> 
    *  <span data-ttu-id="07293-120">**Hiba részletei**: hello hibával kapcsolatos további információkat.</span><span class="sxs-lookup"><span data-stu-id="07293-120">**errorDetails**: Additional information about hello failure.</span></span>

    ```
    {
      "Timestamp": "2017-07-20T15:57:59.9871163-07:00",
      "ProviderName": "Microsoft-ServiceFabric",
      "Id": 51477,
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Request url = https://localhost:19081/LocationApp/LocationFEService?zipcode=98052, verb = GET, remote (client) address = ::1, resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052, request processing start time =     15:58:00.074114 (745,608.196 MSec) ",
      "ProcessId": 57696,
      "Level": "Informational",
      "Keywords": "0x1000000000000021",
      "EventName": "ReverseProxy",
      "ActivityID": null,
      "RelatedActivityID": null,
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?zipcode=98052",
        "verb": "GET",
        "remoteAddress": "::1",
        "resolvedServiceUrl": "Https://localhost:8491/LocationApp/?zipcode=98052",
        "requestStartTime": "2017-07-20T15:58:00.0741142-07:00"
      }
    }

    {
      "Timestamp": "2017-07-20T16:00:01.3173605-07:00",
      ...
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Error while forwarding request tooservice: response status code = 504, description = Reverse proxy Timeout, phase = FinishSendRequest, internal error = ERROR_WINHTTP_TIMEOUT ",
      ...
      "Payload": {
        "traceId": "2f87b722-e254-4ac2-a802-fd315c1a0271",
        "statusCode": 504,
        "description": "Reverse Proxy Timeout",
        "sendRequestPhase": "FinishSendRequest",
        "errorDetails": "internal error = ERROR_WINHTTP_TIMEOUT"
      }
    }
    ```

2. <span data-ttu-id="07293-121">Fordított proxy válasz állapotkódja 404-es (nem található) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="07293-121">Reverse proxy returns response status code 404 (Not Found).</span></span> 
    
    <span data-ttu-id="07293-122">Íme egy példa esemény, fordított proxy ahol adja vissza a 404-es a óta toofind hello megfelelő szolgáltatási végpont nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="07293-122">Here is an example event where reverse proxy returns 404 since it failed toofind hello matching service endpoint.</span></span>
    <span data-ttu-id="07293-123">hello hasznos itt egyik fontos a rendszer:</span><span class="sxs-lookup"><span data-stu-id="07293-123">hello payload  entries of interest here are:</span></span>
    *  <span data-ttu-id="07293-124">**processRequestPhase**: azt jelzi, hello fázis hello hiba történt, amikor a kérelem feldolgozása közben ***TryGetEndpoint*** Egytényezős</span><span class="sxs-lookup"><span data-stu-id="07293-124">**processRequestPhase**: Indicates hello phase during request processing when hello failure occurred, ***TryGetEndpoint*** i.e</span></span> <span data-ttu-id="07293-125">miközben közben toofetch hello szolgáltatás végpont tooforward számára.</span><span class="sxs-lookup"><span data-stu-id="07293-125">while trying toofetch hello service endpoint tooforward to.</span></span> 
    *  <span data-ttu-id="07293-126">**Hiba részletei**: hello végpont keresési feltételek sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="07293-126">**errorDetails**: Lists hello endpoint search criteria.</span></span> <span data-ttu-id="07293-127">Itt láthatja, hogy a megadott hello listenerName = **FrontEndListener** mivel hello replika végpont lista csak tartalmaz egy figyelő hello nevű **OldListener**.</span><span class="sxs-lookup"><span data-stu-id="07293-127">Here you can see that hello listenerName specified = **FrontEndListener** whereas hello replica endpoint list only contains a listener with hello name **OldListener**.</span></span>
    
    ```
    {
      ...
      "Message": "c1cca3b7-f85d-4fef-a162-88af23604343 Error while processing request, cannot forward tooservice: request url = https://localhost:19081/LocationApp/LocationFEService?ListenerName=FrontEndListener&zipcode=98052, verb = GET, remote (client) address = ::1, request processing start time = 16:43:02.686271 (3,448,220.353 MSec), error = FABRIC_E_ENDPOINT_NOT_FOUND, message = , phase = TryGetEndoint, SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"\":\"Https:\/\/localhost:8491\/LocationApp\/\"}} ",
      "ProcessId": 57696,
      "Level": "Warning",
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "c1cca3b7-f85d-4fef-a162-88af23604343",
        "requestUrl": "https://localhost:19081/LocationApp/LocationFEService?ListenerName=NewListener&zipcode=98052",
        ...
        "processRequestPhase": "TryGetEndoint",
        "errorDetails": "SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Https:\/\/localhost:8491\/LocationApp\/\"}}"
      }
    }
    ```
    <span data-ttu-id="07293-128">Ha fordított proxy 404 adhat vissza egy másik példa van nem található: ApplicationGateway\Http konfigurációs paraméter **SecureOnlyMode** hello fordított proxy a figyelést a következő tootrue beállított **HTTPS**, azonban hello replika végpontok összes nem biztonságos (HTTP-figyelő).</span><span class="sxs-lookup"><span data-stu-id="07293-128">Another example where reverse proxy can return 404 Not Found is: ApplicationGateway\Http configuration parameter **SecureOnlyMode** is set tootrue with hello reverse proxy listening on **HTTPS**, however all of hello replica endpoints are unsecure (listening on HTTP).</span></span>
    <span data-ttu-id="07293-129">Fordított proxy beolvasása 404, mivel figyeli a HTTPS-tooforward hello kérést a végpont nem található.</span><span class="sxs-lookup"><span data-stu-id="07293-129">Reverse proxy returns 404 since it cannot find an endpoint listening on HTTPS tooforward hello request.</span></span> <span data-ttu-id="07293-130">Hello eseménytartalom hello paramétereiben elemzésével segít toonarrow hello probléma le:</span><span class="sxs-lookup"><span data-stu-id="07293-130">Analyzing hello parameters in hello event payload helps toonarrow down hello issue:</span></span>
    
    ```
        "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
    ```

3. <span data-ttu-id="07293-131">Kérelem toohello fordított proxy időtúllépési hiba miatt sikertelen.</span><span class="sxs-lookup"><span data-stu-id="07293-131">Request toohello reverse proxy fails with a timeout error.</span></span> 
    <span data-ttu-id="07293-132">hello eseménynaplók tartalmazhatnak esemény érkezett hello szolgáltatáskérés részleteinek (itt nem látható).</span><span class="sxs-lookup"><span data-stu-id="07293-132">hello event logs contain an event with hello received request details (not shown here).</span></span>
    <span data-ttu-id="07293-133">hello következő esemény jeleníti meg, hogy a 404-es állapotkóddal válaszolt hello szolgáltatást és a fordított proxy indít újra feloldani.</span><span class="sxs-lookup"><span data-stu-id="07293-133">hello next event shows that hello service responded with a 404 status code and reverse proxy initiates a re-resolve.</span></span> 

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Request tooservice returned: status code = 404, status description = , Reresolving ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "statusCode": 404,
        "statusDescription": ""
      }
    }
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Re-resolved service url = Https://localhost:8491/LocationApp/?zipcode=98052 ",
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "requestUrl": "Https://localhost:8491/LocationApp/?zipcode=98052"
      }
    }
    ```
    <span data-ttu-id="07293-134">Minden hello események gyűjtése, amikor a vonat minden ki, és előre kísérlet megjelenítő események láthatja.</span><span class="sxs-lookup"><span data-stu-id="07293-134">When collecting all hello events, you see a train of events showing every resolve and forward attempt.</span></span>
    <span data-ttu-id="07293-135">hello utolsó esemény hello sorozat bemutatja hello kérelem feldolgozása sikertelen volt egy időkorlát együtt hello sikeres feloldása kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="07293-135">hello last event in hello series shows hello request processing has failed with a timeout, along with hello number of successful resolve attempts.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="07293-136">Ajánlott tookeep hello részletes csatorna eseménygyűjtés alapértelmezés szerint le van tiltva, és engedélyezze a hibaelhárításhoz szükséges alapon.</span><span class="sxs-lookup"><span data-stu-id="07293-136">It is recommended tookeep hello  verbose channel event collection disabled by default and enable it for troubleshooting on a need basis.</span></span>

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Error while processing request: number of successful resolve attempts = 12, error = FABRIC_E_TIMEOUT, message = , phase = ResolveServicePartition,  ",
      "EventName": "ReverseProxy",
      ...
      "Payload": {
        "traceId": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e",
        "resolveCount": 12,
        "errorval": -2147017729,
        "errorMessage": "",
        "processRequestPhase": "ResolveServicePartition",
        "errorDetails": ""
      }
    }
    ```
    
    <span data-ttu-id="07293-137">Gyűjtemény csak a kritikus hiba/események engedélyezve van, megjelenik egy esemény részleteit hello időtúllépés és hello resolve kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="07293-137">If collection is enabled for critical/error events only, you see one event with details about hello timeout and hello number of resolve attempts.</span></span> 
    
    <span data-ttu-id="07293-138">Ha hello szolgáltatás toosend a 404-es állapot kód hátsó toohello felhasználó, azt kell csatolni kell egy "X-ServiceFabric" fejlécet.</span><span class="sxs-lookup"><span data-stu-id="07293-138">If hello service intends toosend a 404 status code back toohello user, it should be accompanied by an "X-ServiceFabric" header.</span></span> <span data-ttu-id="07293-139">Miután úgy javította ki ez, látni fogja, hogy fordított proxy továbbítást hello állapot kód hátsó toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="07293-139">After fixing this, you will see that reverse proxy forwards hello status code back toohello client.</span></span>  

4. <span data-ttu-id="07293-140">Olyan esetekben, amikor a hello ügyfélkapcsolat megszakadt hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="07293-140">Cases when hello client has disconnected hello request.</span></span>

    <span data-ttu-id="07293-141">hello alatt esemény jön létre, amikor fordított proxy továbbító hello válasz tooclient, de hello ügyfél bontja a kapcsolatot:</span><span class="sxs-lookup"><span data-stu-id="07293-141">hello below event is recorded when reverse proxy is forwarding hello response tooclient but hello client disconnects:</span></span>

    ```
    {
      ...
      "Message": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8 Unable toosend response tooclient: phase = SendResponseHeaders, error = -805306367, internal error = ERROR_SUCCESS ",
      "ProcessId": 57696,
      "Level": "Warning",
      ...
      "EventName": "ReverseProxy",
      "Payload": {
        "traceId": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8",
        "sendResponsePhase": "SendResponseHeaders",
        "errorval": -805306367,
        "winHttpError": "ERROR_SUCCESS"
      }
    }
    ```

> [!NOTE]
> <span data-ttu-id="07293-142">Események kapcsolódó toowebsocket kérelemfeldolgozás nem jelenleg bejelentkezett.</span><span class="sxs-lookup"><span data-stu-id="07293-142">Events related toowebsocket request processing are not currently logged.</span></span> <span data-ttu-id="07293-143">Ez a következő verzióra hello lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="07293-143">This will be added in hello next release.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07293-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="07293-144">Next steps</span></span>
* <span data-ttu-id="07293-145">[Esemény összesítésére és az adatgyűjtést, a Windows Azure diagnosztikai](service-fabric-diagnostics-event-aggregation-wad.md) naplógyűjtést Azure fürtök engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="07293-145">[Event aggregation and collection using Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) for enabling log collection in Azure clusters.</span></span>
* <span data-ttu-id="07293-146">tooview Service Fabric események a Visual Studio alkalmazásban, lásd: [figyelése és diagnosztizálása helyileg](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="07293-146">tooview Service Fabric events in Visual Studio, see [Monitor and diagnose locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>
* <span data-ttu-id="07293-147">Tekintse meg a túl[fordított proxy tooconnect toosecure szolgáltatások konfigurálása](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) az Azure Resource Manager-sablon minták tooconfigure biztonságos fordított proxy hello eltérő tanúsítvány érvényesítése beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="07293-147">Refer too[Configure reverse proxy tooconnect toosecure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) for Azure Resource Manager template samples tooconfigure secure reverse proxy with hello different service certificate validation options.</span></span>
* <span data-ttu-id="07293-148">Olvasási [Service Fabric fordított proxy](service-fabric-reverseproxy.md) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="07293-148">Read [Service Fabric reverse proxy](service-fabric-reverseproxy.md) toolearn more.</span></span>
