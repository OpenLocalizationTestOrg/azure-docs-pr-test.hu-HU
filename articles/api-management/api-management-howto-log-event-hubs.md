---
title: "aaaHow toolog események tooAzure Event Hubs az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan toolog események tooAzure Event Hubs az Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 09ca65fc48a874467c6662858f7594e9b19fcdb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toolog-events-tooazure-event-hubs-in-azure-api-management"></a>Hogyan toolog események tooAzure Event Hubs az Azure API Management
Az Azure Event Hubs egy kiválóan méretezhető adatbefogadási szolgáltatás, amely több millió esemény fogadására képes, így egyszerűen feldolgozhatja és elemezheti az adatokat a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű hello. Az Event Hubs úgy működik, mint egy eseményfolyamat számára "bejárati ajtón" hello, és amennyiben az eseményközpontnak összegyűjtött adatok átalakíthatók, és bármilyen valós idejű elemzési szolgáltató vagy kötegelési/tárolóadapter segítségével tárolják. Az Event Hubs hello éles tartozó események hello felhasználásától, az adatfolyam leválasztja az, hogy az eseményfelhasználók férhetnek hozzá a saját ütemezésüknek hello események.

Ez a cikk egy kiegészítő toohello [integrálni Azure API Management az Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) videót, és bemutatja, hogyan toolog API Management eseményeket az Azure Event Hubs.

## <a name="create-an-azure-event-hub"></a>Hozzon létre egy Azure Event Hubs
egy új Eseményközpont, bejelentkezési toohello toocreate [a klasszikus Azure portálon](https://manage.windowsazure.com) kattintson **új**->**alkalmazásszolgáltatások**->**Service Bus**  -> **Eseményközpont**->**Gyorslétrehozás**. Adja meg az Eseményközpont nevét régióban, válasszon egy előfizetést, és jelöljön ki egy névteret. Ha korábban még nem létrehozott névtér létrehozhat egyet, írja be egy nevet a hello **Namespace** szövegmező. Az összes tulajdonság konfigurálása után kattintson **hozzon létre egy új Eseményközpont** toocreate hello Eseményközpontot.

![Eseményközpont létrehozása][create-event-hub]

Ezután keresse meg a toohello **konfigurálása** az új Eseményközpont lapra, és hozzon létre két **megosztott hozzáférési házirendek**. Name hello első **küldő** , és adjon neki **küldése** engedélyek.

![Küldő házirend][sending-policy]

Name hello második **fogadása**, adjon neki **figyelésére** engedélyeket, majd kattintson **mentése**.

![A fogadó házirend][receiving-policy]

Minden megosztott elérési házirend lehetővé teszi az alkalmazások toosend és események tooand fogadása hello Eseményközpontot. Ezek a házirendek tooaccess hello kapcsolati karakterláncainak toohello keresse meg **irányítópult** hello Eseményközpont, és kattintson a lap **kapcsolatadatok**.

![Kapcsolati karakterlánc][event-hub-dashboard]

Hello **küldő** kapcsolati karakterlánc használatos naplózási eseményeket, és hello **fogadása** hello Eseményközpont-események letöltésekor használt kapcsolati karakterlánc.

![Kapcsolati karakterlánc][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Az API Management naplózó létrehozása
Most, hogy egy Eseményközpontot, hello következő lépésre-e tooconfigure egy [naplózó](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) az API-kezelés szolgáltatást, hogy az események toohello Eseményközpont írhasson a naplóba.

Az API Management-figyelő szoftverek hello használatával konfigurálhatók [API Management REST API](http://aka.ms/smapi). A használata előtt hello REST API hello először, tekintse át a hello [Előfeltételek](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) , és győződjön meg arról, hogy [hozzáférés toohello REST API engedélyezve](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

egy naplózó toocreate egy HTTP PUT-kérelmet a következő URL-cím sablon hello segítségével ellenőrizze.

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* Cserélje le `{your service}` hello nevű az API Management service-példány.
* Cserélje le `{new logger name}` hello kívánt nevet az új naplózó együtt. Ez a név fog hivatkozni, hello konfigurálásakor [napló eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) házirend

Adja hozzá a következő fejlécek toohello kérelem hello.

* Content-Type: az application/json
* Engedélyezési: SharedAccessSignature 58...
  * Hello generálása kapcsolatos utasításokat `SharedAccessSignature` lásd [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Adja meg a sablon a következő hello segítségével hello kérés törzsében.

```json
{
  "type" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of hello Event Hub from hello Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `type`be kell állítani túl`AzureEventHub`.
* `description`hello naplózó leírását biztosít, és szükség esetén egy nulla hosszúságú karakterlánc lehet.
* `credentials`hello tartalmaz `name` és `connectionString` az Azure Event hubs.

Ha elvégezte hello kérelmet, ha a hello naplózó létrejön egy állapotkódját `201 Created` adja vissza.

> [!NOTE]
> Más lehetséges visszatérési kódok és azok okok: [hozzon létre egy naplózó](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). toosee hogyan végezhet el más műveleteket, például a listában, update és delete, lásd: hello [naplózó](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entitás dokumentációját.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Napló-eventhubs-szabályzatok konfigurálása
Után a naplózó az API Management van konfigurálva, a napló-eventhubs házirendek toolog szükséges hello események állíthatja be. hello napló-eventhubs házirend használható vagy hello a bejövő házirend szakasz vagy hello kimenő házirend szakaszban.

tooconfigure házirendek, bejelentkezési toohello [Azure-portálon](https://portal.azure.com), keresse meg a tooyour API Management szolgáltatást, és kattintson **Publisher portal** tooaccess hello publisher portálon.

![Közzétevő portál][publisher-portal]

Kattintson a **házirendek** hello balra hello API-kezelés parancsára, válassza ki a kívánt termék hello és API-t, és kattintson **házirend hozzáadása**. Ebben a példában azt adja hozzá a házirend toohello **Echo API** a hello **korlátlan** termék.

![Házirend hozzáadása][add-policy]

Vigye a kurzort hello `inbound` házirend szakaszt, és kattintson a hello **napló tooEventHub** házirend tooinsert hello `log-to-eventhub` Házirendsablon utasítást.

![Házirendszerkesztő][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

Cserélje le `logger-id` hello API Management naplózó hello előző lépésben konfigurált hello nevére.

Bármely kifejezés, amely egy karakterláncot ad vissza, hello hello értékként használható `log-to-eventhub` elemet. Ebben a példában a karakterlánc hello dátum és az idő, a szolgáltatás neve, a kérelem azonosítója, a kérelem IP-cím és a művelet neve kerül.

Kattintson a **mentése** toosave hello házirend beállításainak frissítése. Amint azok mentésekor hello házirend aktív, és eseményeket naplózott toohello Eseményközpont kijelölt is.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók az Azure Event Hubs
  * [Ismerkedés az Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Üzenetek fogadása az EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programozási útmutató](../event-hubs/event-hubs-programming-guide.md)
* További tudnivalók az API Management és az Event Hubs-integráció
  * [Naplózó entitáshivatkozás](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [napló-eventhub szabályzatainak ismertetése](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Az API-kat az Azure API Management, az Event Hubs és Runscope figyelése](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Bemutató videó megtekintése
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Integrate-Azure-API-Management-with-Event-Hubs/player]
>
>

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
