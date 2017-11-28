---
title: "Az Azure Service Fabric fordított proxy diagnosztika |} Microsoft Docs"
description: "Megtudhatja, hogyan figyelése és diagnosztizálása a fordított proxy kérelem feldolgozását."
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
ms.openlocfilehash: 3bc631606afbc93d5bca94f4955fd2ef816fa9fd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-and-diagnose-request-processing-at-the-reverse-proxy"></a><span data-ttu-id="d81c6-103">Figyelése és diagnosztizálása kérelem feldolgozását a fordított proxy</span><span class="sxs-lookup"><span data-stu-id="d81c6-103">Monitor and diagnose request processing at the reverse proxy</span></span>

<span data-ttu-id="d81c6-104">A Service Fabric 5.7 kiadástól kezdve, fordított proxy események állnak rendelkezésre a következő gyűjtemény számára.</span><span class="sxs-lookup"><span data-stu-id="d81c6-104">Starting with the 5.7 release of Service Fabric, reverse proxy events are available for collection.</span></span> <span data-ttu-id="d81c6-105">Az események két csatorna, egyet a kérelem feldolgozása sikertelen a fordított proxy és a bejegyzéseket a sikeres és a sikertelen kérelmek részletes eseményeket tartalmazó második csatorna kapcsolatos hibaeseményeket csak érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d81c6-105">The events are available in two channels, one with only error events related to request processing failure at the reverse proxy and second channel containing verbose events with entries for both successful and failed requests.</span></span>

<span data-ttu-id="d81c6-106">Tekintse meg [fordított proxy eseményeinek gyűjtése](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) ahhoz, hogy ezeknek a csatornáknak a helyi és az Azure Service Fabric-fürtök gyűjtését eseményekről.</span><span class="sxs-lookup"><span data-stu-id="d81c6-106">Refer to [Collect reverse proxy events](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) to enable collecting events from these channels in local and Azure Service Fabric clusters.</span></span>

## <a name="troubleshoot-using-diagnostics-logs"></a><span data-ttu-id="d81c6-107">Diagnosztikai naplók segítségével hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="d81c6-107">Troubleshoot using diagnostics logs</span></span>
<span data-ttu-id="d81c6-108">Íme néhány példa a találkozhat egy általános hiba naplók értelmezése:</span><span class="sxs-lookup"><span data-stu-id="d81c6-108">Here are some examples on how to interpret the common failure logs that one can encounter:</span></span>

1. <span data-ttu-id="d81c6-109">Fordított proxy válasz állapotkódja 504 (Timeout) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d81c6-109">Reverse proxy returns response status code 504 (Timeout).</span></span>

    <span data-ttu-id="d81c6-110">Több ok miatt a szolgáltatás válaszolni a kérés megadott időn belül nem lehet.</span><span class="sxs-lookup"><span data-stu-id="d81c6-110">One reason could be due to the service failing to reply within the request timeout period.</span></span>
<span data-ttu-id="d81c6-111">Az első esemény naplók alatt a fordított proxy fogadja a kérelem részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="d81c6-111">The first event below logs the details of the request received at the reverse proxy.</span></span> <span data-ttu-id="d81c6-112">A második esemény azt jelzi, hogy a kérelem-továbbítást a szolgáltatás, miközben miatt "belső hiba = ERROR_WINHTTP_TIMEOUT"</span><span class="sxs-lookup"><span data-stu-id="d81c6-112">The second event indicates that the request failed while forwarding to service, due to "internal error = ERROR_WINHTTP_TIMEOUT"</span></span> 

    <span data-ttu-id="d81c6-113">A tartalom tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="d81c6-113">The payload includes:</span></span>

    *  <span data-ttu-id="d81c6-114">**traceId**: A GUID egy kérelemhez tartozó összes esemény összefüggéseket használható.</span><span class="sxs-lookup"><span data-stu-id="d81c6-114">**traceId**: This GUID can be used to correlate all the events corresponding to a single request.</span></span> <span data-ttu-id="d81c6-115">Az az alábbi két esemény, a traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, úgy, hogy a kérésben tartoznak.</span><span class="sxs-lookup"><span data-stu-id="d81c6-115">In the below two events, the traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, implying they belong to the same request.</span></span>
    *  <span data-ttu-id="d81c6-116">**requestUrl**: az URL-cím (fordított proxy URL-cím), amelyhez a kérés lett elküldve.</span><span class="sxs-lookup"><span data-stu-id="d81c6-116">**requestUrl**: The URL (Reverse proxy URL) to which the request was sent.</span></span>
    *  <span data-ttu-id="d81c6-117">**művelet**: HTTP-műveletet.</span><span class="sxs-lookup"><span data-stu-id="d81c6-117">**verb**: HTTP verb.</span></span>
    *  <span data-ttu-id="d81c6-118">**remoteAddress**: a kérést küldő ügyfél címe.</span><span class="sxs-lookup"><span data-stu-id="d81c6-118">**remoteAddress**: Address of client sending the request.</span></span>
    *  <span data-ttu-id="d81c6-119">**resolvedServiceUrl**: végpont URL-címét, amelyhez a bejövő kérelem lett feloldva.</span><span class="sxs-lookup"><span data-stu-id="d81c6-119">**resolvedServiceUrl**: Service endpoint URL to which the incoming request was resolved.</span></span> 
    *  <span data-ttu-id="d81c6-120">**Hiba részletei**: a hibával kapcsolatos további információkat.</span><span class="sxs-lookup"><span data-stu-id="d81c6-120">**errorDetails**: Additional information about the failure.</span></span>

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
      "Message": "2f87b722-e254-4ac2-a802-fd315c1a0271 Error while forwarding request to service: response status code = 504, description = Reverse proxy Timeout, phase = FinishSendRequest, internal error = ERROR_WINHTTP_TIMEOUT ",
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

2. <span data-ttu-id="d81c6-121">Fordított proxy válasz állapotkódja 404-es (nem található) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d81c6-121">Reverse proxy returns response status code 404 (Not Found).</span></span> 
    
    <span data-ttu-id="d81c6-122">Íme egy példa esemény, fordított proxy ahol adja vissza a 404-es a mivel az nem található a megfelelő szolgáltatásvégpontot.</span><span class="sxs-lookup"><span data-stu-id="d81c6-122">Here is an example event where reverse proxy returns 404 since it failed to find the matching service endpoint.</span></span>
    <span data-ttu-id="d81c6-123">A tartalom itt egyik fontos a rendszer:</span><span class="sxs-lookup"><span data-stu-id="d81c6-123">The payload  entries of interest here are:</span></span>
    *  <span data-ttu-id="d81c6-124">**processRequestPhase**: azt jelzi, a fázis, a hiba történt, amikor a kérelem feldolgozása közben ***TryGetEndpoint*** Egytényezős</span><span class="sxs-lookup"><span data-stu-id="d81c6-124">**processRequestPhase**: Indicates the phase during request processing when the failure occurred, ***TryGetEndpoint*** i.e</span></span> <span data-ttu-id="d81c6-125">a szolgáltatásvégpont továbbítania beolvasása közben.</span><span class="sxs-lookup"><span data-stu-id="d81c6-125">while trying to fetch the service endpoint to forward to.</span></span> 
    *  <span data-ttu-id="d81c6-126">**Hiba részletei**: a végpont keresési feltételek sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="d81c6-126">**errorDetails**: Lists the endpoint search criteria.</span></span> <span data-ttu-id="d81c6-127">Itt láthatja, hogy a listenerName megadott = **FrontEndListener** mivel a replika végpontlistát csak figyelő, amelynek a neve tartalmaz **OldListener**.</span><span class="sxs-lookup"><span data-stu-id="d81c6-127">Here you can see that the listenerName specified = **FrontEndListener** whereas the replica endpoint list only contains a listener with the name **OldListener**.</span></span>
    
    ```
    {
      ...
      "Message": "c1cca3b7-f85d-4fef-a162-88af23604343 Error while processing request, cannot forward to service: request url = https://localhost:19081/LocationApp/LocationFEService?ListenerName=FrontEndListener&zipcode=98052, verb = GET, remote (client) address = ::1, request processing start time = 16:43:02.686271 (3,448,220.353 MSec), error = FABRIC_E_ENDPOINT_NOT_FOUND, message = , phase = TryGetEndoint, SecureOnlyMode = false, gateway protocol = https, listenerName = FrontEndListener, replica endpoint = {\"Endpoints\":{\"\":\"Https:\/\/localhost:8491\/LocationApp\/\"}} ",
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
    <span data-ttu-id="d81c6-128">Ha fordított proxy 404 adhat vissza egy másik példa van nem található: ApplicationGateway\Http konfigurációs paraméter **SecureOnlyMode** a fordított proxykiszolgálón igaz értékre kell állítani a figyelést **HTTPS**, azonban a replika végpontok összes nem biztonságos (HTTP-figyelő).</span><span class="sxs-lookup"><span data-stu-id="d81c6-128">Another example where reverse proxy can return 404 Not Found is: ApplicationGateway\Http configuration parameter **SecureOnlyMode** is set to true with the reverse proxy listening on **HTTPS**, however all of the replica endpoints are unsecure (listening on HTTP).</span></span>
    <span data-ttu-id="d81c6-129">Fordított proxy beolvasása 404, mert nem található a végpont figyel a következőn való továbbítja a kérelmet HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d81c6-129">Reverse proxy returns 404 since it cannot find an endpoint listening on HTTPS to forward the request.</span></span> <span data-ttu-id="d81c6-130">A paraméterek elemzése, abban az esetben, ha a probléma szűkítéséhez segítségével hasznos:</span><span class="sxs-lookup"><span data-stu-id="d81c6-130">Analyzing the parameters in the event payload helps to narrow down the issue:</span></span>
    
    ```
        "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
    ```

3. <span data-ttu-id="d81c6-131">A fordított proxykiszolgálóhoz kérelem időtúllépési hiba miatt sikertelen.</span><span class="sxs-lookup"><span data-stu-id="d81c6-131">Request to the reverse proxy fails with a timeout error.</span></span> 
    <span data-ttu-id="d81c6-132">Az eseménynaplók esemény tartalmaznak, a kérelem érkezett adatokkal (itt nem látható).</span><span class="sxs-lookup"><span data-stu-id="d81c6-132">The event logs contain an event with the received request details (not shown here).</span></span>
    <span data-ttu-id="d81c6-133">A következő esemény jeleníti meg, hogy a 404-es állapotkóddal válaszolt a szolgáltatás és a fordított proxy indít újra feloldani.</span><span class="sxs-lookup"><span data-stu-id="d81c6-133">The next event shows that the service responded with a 404 status code and reverse proxy initiates a re-resolve.</span></span> 

    ```
    {
      ...
      "Message": "7ac6212c-c8c4-4c98-9cf7-c187a94f141e Request to service returned: status code = 404, status description = , Reresolving ",
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
    <span data-ttu-id="d81c6-134">Gyűjtött összes eseményt, amikor a vonat események megjelenítése minden ki, és előre kísérlet megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d81c6-134">When collecting all the events, you see a train of events showing every resolve and forward attempt.</span></span>
    <span data-ttu-id="d81c6-135">Az adatsorozat utolsó esemény jeleníti meg, a kérelem feldolgozása sikertelen volt egy időkorlát együtt sikeres feloldása kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="d81c6-135">The last event in the series shows the request processing has failed with a timeout, along with the number of successful resolve attempts.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="d81c6-136">Javasoljuk, hogy alapértelmezés szerint le van tiltva a részletes csatorna eseménygyűjtés tartani, és engedélyezze a hibaelhárításhoz szükséges alapon.</span><span class="sxs-lookup"><span data-stu-id="d81c6-136">It is recommended to keep the  verbose channel event collection disabled by default and enable it for troubleshooting on a need basis.</span></span>

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
    
    <span data-ttu-id="d81c6-137">Gyűjtemény csak a kritikus hiba/események engedélyezve van, megjelenik egy esemény részleteit az időkorlát és a feloldás kísérletek száma.</span><span class="sxs-lookup"><span data-stu-id="d81c6-137">If collection is enabled for critical/error events only, you see one event with details about the timeout and the number of resolve attempts.</span></span> 
    
    <span data-ttu-id="d81c6-138">Ha a szolgáltatás visszaküldi a 404 állapotkódot a felhasználó, azt kell csatolni kell egy "X-ServiceFabric" fejlécet.</span><span class="sxs-lookup"><span data-stu-id="d81c6-138">If the service intends to send a 404 status code back to the user, it should be accompanied by an "X-ServiceFabric" header.</span></span> <span data-ttu-id="d81c6-139">Miután úgy javította ki ez, jelenik meg, hogy fordított proxy állapotkód továbbítja az ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="d81c6-139">After fixing this, you will see that reverse proxy forwards the status code back to the client.</span></span>  

4. <span data-ttu-id="d81c6-140">Olyan esetekben, amikor az ügyfél a kérés megszakadt.</span><span class="sxs-lookup"><span data-stu-id="d81c6-140">Cases when the client has disconnected the request.</span></span>

    <span data-ttu-id="d81c6-141">Az alábbi esemény rögzítése Ha fordított proxy továbbító ügyfél válasz, azonban az ügyfél kapcsolata megszakad:</span><span class="sxs-lookup"><span data-stu-id="d81c6-141">The below event is recorded when reverse proxy is forwarding the response to client but the client disconnects:</span></span>

    ```
    {
      ...
      "Message": "6e2571a3-14a8-4fc7-93bb-c202c23b50b8 Unable to send response to client: phase = SendResponseHeaders, error = -805306367, internal error = ERROR_SUCCESS ",
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
> <span data-ttu-id="d81c6-142">A websocket-kérelem feldolgozásra kapcsolatos események nem jelenleg bejelentkezett.</span><span class="sxs-lookup"><span data-stu-id="d81c6-142">Events related to websocket request processing are not currently logged.</span></span> <span data-ttu-id="d81c6-143">Ez a következő kiadásban lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="d81c6-143">This will be added in the next release.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d81c6-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d81c6-144">Next steps</span></span>
* <span data-ttu-id="d81c6-145">[Esemény összesítésére és az adatgyűjtést, a Windows Azure diagnosztikai](service-fabric-diagnostics-event-aggregation-wad.md) naplógyűjtést Azure fürtök engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d81c6-145">[Event aggregation and collection using Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) for enabling log collection in Azure clusters.</span></span>
* <span data-ttu-id="d81c6-146">Service Fabric-események megtekintése a Visual Studio: [figyelése és diagnosztizálása helyileg](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="d81c6-146">To view Service Fabric events in Visual Studio, see [Monitor and diagnose locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span>
* <span data-ttu-id="d81c6-147">Tekintse meg [fordított proxy konfigurálása a biztonságos szolgáltatásokhoz](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) az Azure Resource Manager sablon minták konfigurálása biztonságos különböző szolgáltatástanúsítvány fordított proxy ellenőrzési beállítások.</span><span class="sxs-lookup"><span data-stu-id="d81c6-147">Refer to [Configure reverse proxy to connect to secure services](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) for Azure Resource Manager template samples to configure secure reverse proxy with the different service certificate validation options.</span></span>
* <span data-ttu-id="d81c6-148">Olvasási [Service Fabric fordított proxy](service-fabric-reverseproxy.md) további.</span><span class="sxs-lookup"><span data-stu-id="d81c6-148">Read [Service Fabric reverse proxy](service-fabric-reverseproxy.md) to learn more.</span></span>
