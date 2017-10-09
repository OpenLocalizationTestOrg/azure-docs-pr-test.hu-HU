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
# <a name="how-toolog-events-tooazure-event-hubs-in-azure-api-management"></a><span data-ttu-id="34e77-103">Hogyan toolog események tooAzure Event Hubs az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="34e77-103">How toolog events tooAzure Event Hubs in Azure API Management</span></span>
<span data-ttu-id="34e77-104">Az Azure Event Hubs egy kiválóan méretezhető adatbefogadási szolgáltatás, amely több millió esemény fogadására képes, így egyszerűen feldolgozhatja és elemezheti az adatokat a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű hello.</span><span class="sxs-lookup"><span data-stu-id="34e77-104">Azure Event Hubs is a highly scalable data ingress service that can ingest millions of events per second so that you can process and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="34e77-105">Az Event Hubs úgy működik, mint egy eseményfolyamat számára "bejárati ajtón" hello, és amennyiben az eseményközpontnak összegyűjtött adatok átalakíthatók, és bármilyen valós idejű elemzési szolgáltató vagy kötegelési/tárolóadapter segítségével tárolják.</span><span class="sxs-lookup"><span data-stu-id="34e77-105">Event Hubs acts as hello "front door" for an event pipeline, and once data is collected into an event hub, it can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="34e77-106">Az Event Hubs hello éles tartozó események hello felhasználásától, az adatfolyam leválasztja az, hogy az eseményfelhasználók férhetnek hozzá a saját ütemezésüknek hello események.</span><span class="sxs-lookup"><span data-stu-id="34e77-106">Event Hubs decouples hello production of a stream of events from hello consumption of those events, so that event consumers can access hello events on their own schedule.</span></span>

<span data-ttu-id="34e77-107">Ez a cikk egy kiegészítő toohello [integrálni Azure API Management az Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) videót, és bemutatja, hogyan toolog API Management eseményeket az Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="34e77-107">This article is a companion toohello [Integrate Azure API Management with Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video and describes how toolog API Management events using Azure Event Hubs.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="34e77-108">Hozzon létre egy Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="34e77-108">Create an Azure Event Hub</span></span>
<span data-ttu-id="34e77-109">egy új Eseményközpont, bejelentkezési toohello toocreate [a klasszikus Azure portálon](https://manage.windowsazure.com) kattintson **új**->**alkalmazásszolgáltatások**->**Service Bus**  -> **Eseményközpont**->**Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="34e77-109">toocreate a new Event Hub, sign-in toohello [Azure classic portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Service Bus**->**Event Hub**->**Quick Create**.</span></span> <span data-ttu-id="34e77-110">Adja meg az Eseményközpont nevét régióban, válasszon egy előfizetést, és jelöljön ki egy névteret.</span><span class="sxs-lookup"><span data-stu-id="34e77-110">Enter an Event Hub name, region, select a subscription, and select a namespace.</span></span> <span data-ttu-id="34e77-111">Ha korábban még nem létrehozott névtér létrehozhat egyet, írja be egy nevet a hello **Namespace** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="34e77-111">If you haven't previously created a namespace you can create one by typing a name in hello **Namespace** textbox.</span></span> <span data-ttu-id="34e77-112">Az összes tulajdonság konfigurálása után kattintson **hozzon létre egy új Eseményközpont** toocreate hello Eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="34e77-112">Once all properties are configured, click **Create a new Event Hub** toocreate hello Event Hub.</span></span>

![Eseményközpont létrehozása][create-event-hub]

<span data-ttu-id="34e77-114">Ezután keresse meg a toohello **konfigurálása** az új Eseményközpont lapra, és hozzon létre két **megosztott hozzáférési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="34e77-114">Next, navigate toohello **Configure** tab for your new Event Hub and create two **shared access policies**.</span></span> <span data-ttu-id="34e77-115">Name hello első **küldő** , és adjon neki **küldése** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="34e77-115">Name hello first one **Sending** and give it **Send** permissions.</span></span>

![Küldő házirend][sending-policy]

<span data-ttu-id="34e77-117">Name hello második **fogadása**, adjon neki **figyelésére** engedélyeket, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="34e77-117">Name hello second one **Receiving**, give it **Listen** permissions, and click **Save**.</span></span>

![A fogadó házirend][receiving-policy]

<span data-ttu-id="34e77-119">Minden megosztott elérési házirend lehetővé teszi az alkalmazások toosend és események tooand fogadása hello Eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="34e77-119">Each shared access policy allows applications toosend and receive events tooand from hello Event Hub.</span></span> <span data-ttu-id="34e77-120">Ezek a házirendek tooaccess hello kapcsolati karakterláncainak toohello keresse meg **irányítópult** hello Eseményközpont, és kattintson a lap **kapcsolatadatok**.</span><span class="sxs-lookup"><span data-stu-id="34e77-120">tooaccess hello connection strings for these policies, navigate toohello **Dashboard** tab of hello Event Hub and click **Connection information**.</span></span>

![Kapcsolati karakterlánc][event-hub-dashboard]

<span data-ttu-id="34e77-122">Hello **küldő** kapcsolati karakterlánc használatos naplózási eseményeket, és hello **fogadása** hello Eseményközpont-események letöltésekor használt kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="34e77-122">hello **Sending** connection string is used when logging events, and hello **Receiving** connection string is used when downloading events from hello Event Hub.</span></span>

![Kapcsolati karakterlánc][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a><span data-ttu-id="34e77-124">Az API Management naplózó létrehozása</span><span class="sxs-lookup"><span data-stu-id="34e77-124">Create an API Management logger</span></span>
<span data-ttu-id="34e77-125">Most, hogy egy Eseményközpontot, hello következő lépésre-e tooconfigure egy [naplózó](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) az API-kezelés szolgáltatást, hogy az események toohello Eseményközpont írhasson a naplóba.</span><span class="sxs-lookup"><span data-stu-id="34e77-125">Now that you have an Event Hub, hello next step is tooconfigure a [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) in your API Management service so that it can log events toohello Event Hub.</span></span>

<span data-ttu-id="34e77-126">Az API Management-figyelő szoftverek hello használatával konfigurálhatók [API Management REST API](http://aka.ms/smapi).</span><span class="sxs-lookup"><span data-stu-id="34e77-126">API Management loggers are configured using hello [API Management REST API](http://aka.ms/smapi).</span></span> <span data-ttu-id="34e77-127">A használata előtt hello REST API hello először, tekintse át a hello [Előfeltételek](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) , és győződjön meg arról, hogy [hozzáférés toohello REST API engedélyezve](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span><span class="sxs-lookup"><span data-stu-id="34e77-127">Before using hello REST API for hello first time, review hello [prerequisites](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) and ensure that you have [enabled access toohello REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span></span>

<span data-ttu-id="34e77-128">egy naplózó toocreate egy HTTP PUT-kérelmet a következő URL-cím sablon hello segítségével ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="34e77-128">toocreate a logger, make an HTTP PUT request using hello following URL template.</span></span>

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* <span data-ttu-id="34e77-129">Cserélje le `{your service}` hello nevű az API Management service-példány.</span><span class="sxs-lookup"><span data-stu-id="34e77-129">Replace `{your service}` with hello name of your API Management service instance.</span></span>
* <span data-ttu-id="34e77-130">Cserélje le `{new logger name}` hello kívánt nevet az új naplózó együtt.</span><span class="sxs-lookup"><span data-stu-id="34e77-130">Replace `{new logger name}` with hello desired name for your new logger.</span></span> <span data-ttu-id="34e77-131">Ez a név fog hivatkozni, hello konfigurálásakor [napló eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) házirend</span><span class="sxs-lookup"><span data-stu-id="34e77-131">You will reference this name when you configure hello [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) policy</span></span>

<span data-ttu-id="34e77-132">Adja hozzá a következő fejlécek toohello kérelem hello.</span><span class="sxs-lookup"><span data-stu-id="34e77-132">Add hello following headers toohello request.</span></span>

* <span data-ttu-id="34e77-133">Content-Type: az application/json</span><span class="sxs-lookup"><span data-stu-id="34e77-133">Content-Type : application/json</span></span>
* <span data-ttu-id="34e77-134">Engedélyezési: SharedAccessSignature 58...</span><span class="sxs-lookup"><span data-stu-id="34e77-134">Authorization : SharedAccessSignature 58...</span></span>
  * <span data-ttu-id="34e77-135">Hello generálása kapcsolatos utasításokat `SharedAccessSignature` lásd [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span><span class="sxs-lookup"><span data-stu-id="34e77-135">For instructions on generating hello `SharedAccessSignature` see [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span></span>

<span data-ttu-id="34e77-136">Adja meg a sablon a következő hello segítségével hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="34e77-136">Specify hello request body using hello following template.</span></span>

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

* <span data-ttu-id="34e77-137">`type`be kell állítani túl`AzureEventHub`.</span><span class="sxs-lookup"><span data-stu-id="34e77-137">`type` must be set too`AzureEventHub`.</span></span>
* <span data-ttu-id="34e77-138">`description`hello naplózó leírását biztosít, és szükség esetén egy nulla hosszúságú karakterlánc lehet.</span><span class="sxs-lookup"><span data-stu-id="34e77-138">`description` provides an optional description of hello logger and can be a zero length string if desired.</span></span>
* <span data-ttu-id="34e77-139">`credentials`hello tartalmaz `name` és `connectionString` az Azure Event hubs.</span><span class="sxs-lookup"><span data-stu-id="34e77-139">`credentials` contains hello `name` and `connectionString` of your Azure Event Hub.</span></span>

<span data-ttu-id="34e77-140">Ha elvégezte hello kérelmet, ha a hello naplózó létrejön egy állapotkódját `201 Created` adja vissza.</span><span class="sxs-lookup"><span data-stu-id="34e77-140">When you make hello request, if hello logger is created a status code of `201 Created` is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="34e77-141">Más lehetséges visszatérési kódok és azok okok: [hozzon létre egy naplózó](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span><span class="sxs-lookup"><span data-stu-id="34e77-141">For other possible return codes and their reasons, see [Create a Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span></span> <span data-ttu-id="34e77-142">toosee hogyan végezhet el más műveleteket, például a listában, update és delete, lásd: hello [naplózó](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entitás dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="34e77-142">toosee how perform other operations such as list, update, and delete, see hello [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entity documentation.</span></span>
>
>

## <a name="configure-log-to-eventhubs-policies"></a><span data-ttu-id="34e77-143">Napló-eventhubs-szabályzatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="34e77-143">Configure log-to-eventhubs policies</span></span>
<span data-ttu-id="34e77-144">Után a naplózó az API Management van konfigurálva, a napló-eventhubs házirendek toolog szükséges hello események állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="34e77-144">Once your logger is configured in API Management, you can configure your log-to-eventhubs policies toolog hello desired events.</span></span> <span data-ttu-id="34e77-145">hello napló-eventhubs házirend használható vagy hello a bejövő házirend szakasz vagy hello kimenő házirend szakaszban.</span><span class="sxs-lookup"><span data-stu-id="34e77-145">hello log-to-eventhubs policy can be used in either hello inbound policy section or hello outbound policy section.</span></span>

<span data-ttu-id="34e77-146">tooconfigure házirendek, bejelentkezési toohello [Azure-portálon](https://portal.azure.com), keresse meg a tooyour API Management szolgáltatást, és kattintson **Publisher portal** tooaccess hello publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="34e77-146">tooconfigure policies, sign-in toohello [Azure portal](https://portal.azure.com), navigate tooyour API Management service, and click **Publisher portal** tooaccess hello publisher portal.</span></span>

![Közzétevő portál][publisher-portal]

<span data-ttu-id="34e77-148">Kattintson a **házirendek** hello balra hello API-kezelés parancsára, válassza ki a kívánt termék hello és API-t, és kattintson **házirend hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="34e77-148">Click **Policies** in hello API Management menu on hello left, select hello desired product and API, and click **Add policy**.</span></span> <span data-ttu-id="34e77-149">Ebben a példában azt adja hozzá a házirend toohello **Echo API** a hello **korlátlan** termék.</span><span class="sxs-lookup"><span data-stu-id="34e77-149">In this example we're adding a policy toohello **Echo API** in hello **Unlimited** product.</span></span>

![Házirend hozzáadása][add-policy]

<span data-ttu-id="34e77-151">Vigye a kurzort hello `inbound` házirend szakaszt, és kattintson a hello **napló tooEventHub** házirend tooinsert hello `log-to-eventhub` Házirendsablon utasítást.</span><span class="sxs-lookup"><span data-stu-id="34e77-151">Position your cursor in hello `inbound` policy section and click hello **Log tooEventHub** policy tooinsert hello `log-to-eventhub` policy statement template.</span></span>

![Házirendszerkesztő][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

<span data-ttu-id="34e77-153">Cserélje le `logger-id` hello API Management naplózó hello előző lépésben konfigurált hello nevére.</span><span class="sxs-lookup"><span data-stu-id="34e77-153">Replace `logger-id` with hello name of hello API Management logger you configured in hello previous step.</span></span>

<span data-ttu-id="34e77-154">Bármely kifejezés, amely egy karakterláncot ad vissza, hello hello értékként használható `log-to-eventhub` elemet.</span><span class="sxs-lookup"><span data-stu-id="34e77-154">You can use any expression that returns a string as hello value for hello `log-to-eventhub` element.</span></span> <span data-ttu-id="34e77-155">Ebben a példában a karakterlánc hello dátum és az idő, a szolgáltatás neve, a kérelem azonosítója, a kérelem IP-cím és a művelet neve kerül.</span><span class="sxs-lookup"><span data-stu-id="34e77-155">In this example a string containing hello date and time, service name, request id, request ip address, and operation name is logged.</span></span>

<span data-ttu-id="34e77-156">Kattintson a **mentése** toosave hello házirend beállításainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="34e77-156">Click **Save** toosave hello updated policy configuration.</span></span> <span data-ttu-id="34e77-157">Amint azok mentésekor hello házirend aktív, és eseményeket naplózott toohello Eseményközpont kijelölt is.</span><span class="sxs-lookup"><span data-stu-id="34e77-157">As soon as it is saved hello policy is active and events are logged toohello designated Event Hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34e77-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34e77-158">Next steps</span></span>
* <span data-ttu-id="34e77-159">További tudnivalók az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="34e77-159">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="34e77-160">Ismerkedés az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="34e77-160">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="34e77-161">Üzenetek fogadása az EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="34e77-161">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="34e77-162">Event Hubs programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="34e77-162">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="34e77-163">További tudnivalók az API Management és az Event Hubs-integráció</span><span class="sxs-lookup"><span data-stu-id="34e77-163">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="34e77-164">Naplózó entitáshivatkozás</span><span class="sxs-lookup"><span data-stu-id="34e77-164">Logger entity reference</span></span>](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [<span data-ttu-id="34e77-165">napló-eventhub szabályzatainak ismertetése</span><span class="sxs-lookup"><span data-stu-id="34e77-165">log-to-eventhub policy reference</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [<span data-ttu-id="34e77-166">Az API-kat az Azure API Management, az Event Hubs és Runscope figyelése</span><span class="sxs-lookup"><span data-stu-id="34e77-166">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a><span data-ttu-id="34e77-167">Bemutató videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="34e77-167">Watch a video walkthrough</span></span>
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
