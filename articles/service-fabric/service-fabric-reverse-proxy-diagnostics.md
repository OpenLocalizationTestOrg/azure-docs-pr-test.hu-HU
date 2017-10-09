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
# <a name="monitor-and-diagnose-request-processing-at-hello-reverse-proxy"></a>Figyelése és diagnosztizálása kérelem feldolgozását hello fordított proxy

A Service Fabric hello 5.7 kiadástól kezdve, fordított proxy események állnak rendelkezésre a gyűjteményhez. hello események állnak rendelkezésre a két csatornák toorequest feldolgozási hiba hello fordított proxy és a bejegyzéseket a sikeres és a sikertelen kérelmek részletes eseményeket tartalmazó második csatorna kapcsolatos hibaesemények csak egyet.

Tekintse meg a túl[fordított proxy eseményeinek gyűjtése](service-fabric-diagnostics-event-aggregation-wad.md#collect-reverse-proxy-events) tooenable ezeknek a csatornáknak a helyi és az Azure Service Fabric-fürtök az események gyűjtése.

## <a name="troubleshoot-using-diagnostics-logs"></a>Diagnosztikai naplók segítségével hibaelhárítása
Íme néhány példa a hogyan toointerpret hello közös hiba naplókat, hogy egy találkozhat:

1. Fordított proxy válasz állapotkódja 504 (Timeout) adja vissza.

    Egyik oka toohello szolgáltatás leállása tooreply oka lehet hello kérelem időkorláton belül.
hello első eseményt naplózza a hello kérelem érkezett: hello fordított proxy hello részleteit. hello második esemény azt jelzi, hogy hello kérelem sikertelen volt továbbítási tooservice, miközben kellő túl "belső hiba = ERROR_WINHTTP_TIMEOUT" 

    hello hasznos tartalmazza:

    *  **traceId**: A GUID lehet használt toocorrelate tooa egy kérelemhez tartozó összes hello eseményt. Hello alább két esemény, a hello traceId = **2f87b722-e254-4ac2-a802-fd315c1a0271**, ami azt jelenti, toohello tartoznak egyazon kérelemben.
    *  **requestUrl**: hello URL-címe (fordított proxy URL-cím) toowhich hello kérés elküldése megtörtént.
    *  **művelet**: HTTP-műveletet.
    *  **remoteAddress**: hello kérést küldő ügyfél címe.
    *  **resolvedServiceUrl**: szolgáltatási végpont URL-cím toowhich hello bejövő kérelem lett feloldva. 
    *  **Hiba részletei**: hello hibával kapcsolatos további információkat.

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

2. Fordított proxy válasz állapotkódja 404-es (nem található) adja vissza. 
    
    Íme egy példa esemény, fordított proxy ahol adja vissza a 404-es a óta toofind hello megfelelő szolgáltatási végpont nem sikerült.
    hello hasznos itt egyik fontos a rendszer:
    *  **processRequestPhase**: azt jelzi, hello fázis hello hiba történt, amikor a kérelem feldolgozása közben ***TryGetEndpoint*** Egytényezős miközben közben toofetch hello szolgáltatás végpont tooforward számára. 
    *  **Hiba részletei**: hello végpont keresési feltételek sorolja fel. Itt láthatja, hogy a megadott hello listenerName = **FrontEndListener** mivel hello replika végpont lista csak tartalmaz egy figyelő hello nevű **OldListener**.
    
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
    Ha fordított proxy 404 adhat vissza egy másik példa van nem található: ApplicationGateway\Http konfigurációs paraméter **SecureOnlyMode** hello fordított proxy a figyelést a következő tootrue beállított **HTTPS**, azonban hello replika végpontok összes nem biztonságos (HTTP-figyelő).
    Fordított proxy beolvasása 404, mivel figyeli a HTTPS-tooforward hello kérést a végpont nem található. Hello eseménytartalom hello paramétereiben elemzésével segít toonarrow hello probléma le:
    
    ```
        "errorDetails": "SecureOnlyMode = true, gateway protocol = https, listenerName = NewListener, replica endpoint = {\"Endpoints\":{\"OldListener\":\"Http:\/\/localhost:8491\/LocationApp\/\", \"NewListener\":\"Http:\/\/localhost:8492\/LocationApp\/\"}}"
    ```

3. Kérelem toohello fordított proxy időtúllépési hiba miatt sikertelen. 
    hello eseménynaplók tartalmazhatnak esemény érkezett hello szolgáltatáskérés részleteinek (itt nem látható).
    hello következő esemény jeleníti meg, hogy a 404-es állapotkóddal válaszolt hello szolgáltatást és a fordított proxy indít újra feloldani. 

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
    Minden hello események gyűjtése, amikor a vonat minden ki, és előre kísérlet megjelenítő események láthatja.
    hello utolsó esemény hello sorozat bemutatja hello kérelem feldolgozása sikertelen volt egy időkorlát együtt hello sikeres feloldása kísérletek száma.
    
    > [!NOTE]
    > Ajánlott tookeep hello részletes csatorna eseménygyűjtés alapértelmezés szerint le van tiltva, és engedélyezze a hibaelhárításhoz szükséges alapon.

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
    
    Gyűjtemény csak a kritikus hiba/események engedélyezve van, megjelenik egy esemény részleteit hello időtúllépés és hello resolve kísérletek száma. 
    
    Ha hello szolgáltatás toosend a 404-es állapot kód hátsó toohello felhasználó, azt kell csatolni kell egy "X-ServiceFabric" fejlécet. Miután úgy javította ki ez, látni fogja, hogy fordított proxy továbbítást hello állapot kód hátsó toohello ügyfél.  

4. Olyan esetekben, amikor a hello ügyfélkapcsolat megszakadt hello kérelem.

    hello alatt esemény jön létre, amikor fordított proxy továbbító hello válasz tooclient, de hello ügyfél bontja a kapcsolatot:

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
> Események kapcsolódó toowebsocket kérelemfeldolgozás nem jelenleg bejelentkezett. Ez a következő verzióra hello lesz hozzáadva.

## <a name="next-steps"></a>Következő lépések
* [Esemény összesítésére és az adatgyűjtést, a Windows Azure diagnosztikai](service-fabric-diagnostics-event-aggregation-wad.md) naplógyűjtést Azure fürtök engedélyezéséhez.
* tooview Service Fabric események a Visual Studio alkalmazásban, lásd: [figyelése és diagnosztizálása helyileg](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).
* Tekintse meg a túl[fordított proxy tooconnect toosecure szolgáltatások konfigurálása](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) az Azure Resource Manager-sablon minták tooconfigure biztonságos fordított proxy hello eltérő tanúsítvány érvényesítése beállításokkal.
* Olvasási [Service Fabric fordított proxy](service-fabric-reverseproxy.md) további toolearn.
