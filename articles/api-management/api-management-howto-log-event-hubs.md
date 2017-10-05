---
title: "Hogyan naplózza az eseményeket az Azure Event Hubs az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan naplózza az eseményeket az Azure Event Hubs az Azure API Management."
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
ms.openlocfilehash: a310236179677046ec49930b07cfdffdadc37974
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a><span data-ttu-id="5c409-103">Hogyan naplózza az eseményeket az Azure Event Hubs az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="5c409-103">How to log events to Azure Event Hubs in Azure API Management</span></span>
<span data-ttu-id="5c409-104">Az Azure Event Hubs egy kiválóan méretezhető adatbefogadási szolgáltatás, amely másodpercenként több millió esemény fogadására képes, így a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű adatot egyszerűen feldolgozhatja és elemezheti.</span><span class="sxs-lookup"><span data-stu-id="5c409-104">Azure Event Hubs is a highly scalable data ingress service that can ingest millions of events per second so that you can process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="5c409-105">Az Event Hubs úgy működik, mint a "bejárati ajtón" egy eseményfolyamat számára, és amennyiben az eseményközpontnak összegyűjtött adatok átalakíthatók, és bármilyen valós idejű elemzési szolgáltató vagy kötegelési/tárolóadapter segítségével tárolják.</span><span class="sxs-lookup"><span data-stu-id="5c409-105">Event Hubs acts as the "front door" for an event pipeline, and once data is collected into an event hub, it can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="5c409-106">Az Event Hubs elválasztja az eseménystreamek létrehozását azok felhasználásától, így az események felhasználói a saját ütemezésüknek megfelelően férhetnek hozzá az eseményekhez.</span><span class="sxs-lookup"><span data-stu-id="5c409-106">Event Hubs decouples the production of a stream of events from the consumption of those events, so that event consumers can access the events on their own schedule.</span></span>

<span data-ttu-id="5c409-107">Ez a cikk egy kiegészítő, hogy a [integrálni Azure API Management az Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) videó ismerteti, hogyan lehet az Azure Event Hubs API Management naplózása és.</span><span class="sxs-lookup"><span data-stu-id="5c409-107">This article is a companion to the [Integrate Azure API Management with Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video and describes how to log API Management events using Azure Event Hubs.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="5c409-108">Hozzon létre egy Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5c409-108">Create an Azure Event Hub</span></span>
<span data-ttu-id="5c409-109">Egy új Eseményközpont létrehozásához jelentkezzen be a [a klasszikus Azure portálon](https://manage.windowsazure.com) kattintson **új**->**alkalmazásszolgáltatások**->**Service Bus**->**Eseményközpont**->**Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="5c409-109">To create a new Event Hub, sign-in to the [Azure classic portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Service Bus**->**Event Hub**->**Quick Create**.</span></span> <span data-ttu-id="5c409-110">Adja meg az Eseményközpont nevét régióban, válasszon egy előfizetést, és jelöljön ki egy névteret.</span><span class="sxs-lookup"><span data-stu-id="5c409-110">Enter an Event Hub name, region, select a subscription, and select a namespace.</span></span> <span data-ttu-id="5c409-111">Ha korábban még nem létrehozott névtér létrehozhat egyet, írja be a nevet adja meg a **Namespace** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="5c409-111">If you haven't previously created a namespace you can create one by typing a name in the **Namespace** textbox.</span></span> <span data-ttu-id="5c409-112">Az összes tulajdonság konfigurálása után kattintson **hozzon létre egy új Eseményközpont** az Eseményközpont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5c409-112">Once all properties are configured, click **Create a new Event Hub** to create the Event Hub.</span></span>

![Eseményközpont létrehozása][create-event-hub]

<span data-ttu-id="5c409-114">Ezután keresse meg a **konfigurálása** az új Eseményközpont lapra, és hozzon létre két **megosztott hozzáférési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="5c409-114">Next, navigate to the **Configure** tab for your new Event Hub and create two **shared access policies**.</span></span> <span data-ttu-id="5c409-115">Neve az elsőt **küldő** , és adjon neki **küldése** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="5c409-115">Name the first one **Sending** and give it **Send** permissions.</span></span>

![Küldő házirend][sending-policy]

<span data-ttu-id="5c409-117">Neve a másikat **fogadása**, adjon neki **figyelésére** engedélyeket, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5c409-117">Name the second one **Receiving**, give it **Listen** permissions, and click **Save**.</span></span>

![A fogadó házirend][receiving-policy]

<span data-ttu-id="5c409-119">Minden megosztott elérési házirend lehetővé teszi, hogy az alkalmazások és az Event Hubs az események küldéséhez és fogadásához.</span><span class="sxs-lookup"><span data-stu-id="5c409-119">Each shared access policy allows applications to send and receive events to and from the Event Hub.</span></span> <span data-ttu-id="5c409-120">Ezek a házirendek a kapcsolati karakterláncok szeretne használni, keresse meg a **irányítópult** a Eseményközpont, majd kattintson a lap **kapcsolatadatok**.</span><span class="sxs-lookup"><span data-stu-id="5c409-120">To access the connection strings for these policies, navigate to the **Dashboard** tab of the Event Hub and click **Connection information**.</span></span>

![Kapcsolati karakterlánc][event-hub-dashboard]

<span data-ttu-id="5c409-122">A **küldő** kapcsolati karakterlánc használatos naplózási eseményeket, és a **fogadása** az Event Hubs-események letöltésekor használt kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="5c409-122">The **Sending** connection string is used when logging events, and the **Receiving** connection string is used when downloading events from the Event Hub.</span></span>

![Kapcsolati karakterlánc][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a><span data-ttu-id="5c409-124">Az API Management naplózó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c409-124">Create an API Management logger</span></span>
<span data-ttu-id="5c409-125">Most, hogy egy Eseményközpontot,-e a következő lépéssel konfigurálhatja a [naplózó](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) az API-kezelés szolgáltatást, hogy az események bejelentkezhetnek az Eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="5c409-125">Now that you have an Event Hub, the next step is to configure a [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) in your API Management service so that it can log events to the Event Hub.</span></span>

<span data-ttu-id="5c409-126">Az API Management-figyelő szoftverek használatával vannak konfigurálva a [API Management REST API](http://aka.ms/smapi).</span><span class="sxs-lookup"><span data-stu-id="5c409-126">API Management loggers are configured using the [API Management REST API](http://aka.ms/smapi).</span></span> <span data-ttu-id="5c409-127">Előtt először a REST API használatával, olvassa el a [Előfeltételek](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) , és győződjön meg arról, hogy [engedélyezve van a REST API eléréséhez](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span><span class="sxs-lookup"><span data-stu-id="5c409-127">Before using the REST API for the first time, review the [prerequisites](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) and ensure that you have [enabled access to the REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span></span>

<span data-ttu-id="5c409-128">Hozzon létre egy naplózó, hajtsa végre egy HTTP PUT-kérelmet a következő URL-cím sablonja segítségével.</span><span class="sxs-lookup"><span data-stu-id="5c409-128">To create a logger, make an HTTP PUT request using the following URL template.</span></span>

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* <span data-ttu-id="5c409-129">Cserélje le `{your service}` az API Management szolgáltatáspéldány nevét.</span><span class="sxs-lookup"><span data-stu-id="5c409-129">Replace `{your service}` with the name of your API Management service instance.</span></span>
* <span data-ttu-id="5c409-130">Cserélje le `{new logger name}` az új naplózó a kívánt néven.</span><span class="sxs-lookup"><span data-stu-id="5c409-130">Replace `{new logger name}` with the desired name for your new logger.</span></span> <span data-ttu-id="5c409-131">Ez a név fog hivatkozni, amikor konfigurálja a [napló eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) házirend</span><span class="sxs-lookup"><span data-stu-id="5c409-131">You will reference this name when you configure the [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) policy</span></span>

<span data-ttu-id="5c409-132">A következő fejlécek hozzáadása a kéréshez.</span><span class="sxs-lookup"><span data-stu-id="5c409-132">Add the following headers to the request.</span></span>

* <span data-ttu-id="5c409-133">Content-Type: az application/json</span><span class="sxs-lookup"><span data-stu-id="5c409-133">Content-Type : application/json</span></span>
* <span data-ttu-id="5c409-134">Engedélyezési: SharedAccessSignature 58...</span><span class="sxs-lookup"><span data-stu-id="5c409-134">Authorization : SharedAccessSignature 58...</span></span>
  * <span data-ttu-id="5c409-135">Generálása kapcsolatos utasításokat a `SharedAccessSignature` lásd [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span><span class="sxs-lookup"><span data-stu-id="5c409-135">For instructions on generating the `SharedAccessSignature` see [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span></span>

<span data-ttu-id="5c409-136">Adja meg a kérelem törzsében, az alábbi sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="5c409-136">Specify the request body using the following template.</span></span>

```json
{
  "type" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* <span data-ttu-id="5c409-137">`type`meg kell `AzureEventHub`.</span><span class="sxs-lookup"><span data-stu-id="5c409-137">`type` must be set to `AzureEventHub`.</span></span>
* <span data-ttu-id="5c409-138">`description`egy leírást a naplózó biztosít, és szükség esetén egy nulla hosszúságú karakterlánc lehet.</span><span class="sxs-lookup"><span data-stu-id="5c409-138">`description` provides an optional description of the logger and can be a zero length string if desired.</span></span>
* <span data-ttu-id="5c409-139">`credentials`tartalmazza a `name` és `connectionString` az Azure Event hubs.</span><span class="sxs-lookup"><span data-stu-id="5c409-139">`credentials` contains the `name` and `connectionString` of your Azure Event Hub.</span></span>

<span data-ttu-id="5c409-140">Ha elvégezte a kérelmet, ha a naplózó létrejön egy állapotkódját `201 Created` adja vissza.</span><span class="sxs-lookup"><span data-stu-id="5c409-140">When you make the request, if the logger is created a status code of `201 Created` is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="5c409-141">Más lehetséges visszatérési kódok és azok okok: [hozzon létre egy naplózó](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span><span class="sxs-lookup"><span data-stu-id="5c409-141">For other possible return codes and their reasons, see [Create a Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span></span> <span data-ttu-id="5c409-142">Hogy hogyan hajtsa végre az egyéb műveletek, például a listában, update és delete, lásd: a [naplózó](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entitás dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5c409-142">To see how perform other operations such as list, update, and delete, see the [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entity documentation.</span></span>
>
>

## <a name="configure-log-to-eventhubs-policies"></a><span data-ttu-id="5c409-143">Napló-eventhubs-szabályzatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5c409-143">Configure log-to-eventhubs policies</span></span>
<span data-ttu-id="5c409-144">Miután beállította a naplózó az API Management, beállíthatja a napló-eventhubs a kívánt események naplózása.</span><span class="sxs-lookup"><span data-stu-id="5c409-144">Once your logger is configured in API Management, you can configure your log-to-eventhubs policies to log the desired events.</span></span> <span data-ttu-id="5c409-145">A napló-eventhubs házirend a bejövő házirend szakaszban vagy a kimenő házirend szakaszban használható.</span><span class="sxs-lookup"><span data-stu-id="5c409-145">The log-to-eventhubs policy can be used in either the inbound policy section or the outbound policy section.</span></span>

<span data-ttu-id="5c409-146">Házirendek konfigurálásához, jelentkezzen be a [Azure-portálon](https://portal.azure.com), keresse meg az API Management szolgáltatást, és kattintson **Publisher portal** közzétevő-portál eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5c409-146">To configure policies, sign-in to the [Azure portal](https://portal.azure.com), navigate to your API Management service, and click **Publisher portal** to access the publisher portal.</span></span>

![Közzétevő portál][publisher-portal]

<span data-ttu-id="5c409-148">Kattintson a **házirendek** a bal oldali API Management menüben válasszon ki a kívánt termék és API-t, és kattintson a **házirend hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5c409-148">Click **Policies** in the API Management menu on the left, select the desired product and API, and click **Add policy**.</span></span> <span data-ttu-id="5c409-149">Ebben a példában egy házirendet, amellyel visszaigazolása azt a **Echo API** a a **korlátlan** termék.</span><span class="sxs-lookup"><span data-stu-id="5c409-149">In this example we're adding a policy to the **Echo API** in the **Unlimited** product.</span></span>

![Házirend hozzáadása][add-policy]

<span data-ttu-id="5c409-151">Vigye a kurzort a a `inbound` házirend szakaszban, és kattintson a **EventHub napló** házirend beszúrására a `log-to-eventhub` Házirendsablon utasítás.</span><span class="sxs-lookup"><span data-stu-id="5c409-151">Position your cursor in the `inbound` policy section and click the **Log to EventHub** policy to insert the `log-to-eventhub` policy statement template.</span></span>

![Házirendszerkesztő][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

<span data-ttu-id="5c409-153">Cserélje le `logger-id` az API Management-naplózó az előző lépésben konfigurált nevével.</span><span class="sxs-lookup"><span data-stu-id="5c409-153">Replace `logger-id` with the name of the API Management logger you configured in the previous step.</span></span>

<span data-ttu-id="5c409-154">Használhat egy kifejezést, amely egy karakterláncot ad vissza, az értéknek a `log-to-eventhub` elemet.</span><span class="sxs-lookup"><span data-stu-id="5c409-154">You can use any expression that returns a string as the value for the `log-to-eventhub` element.</span></span> <span data-ttu-id="5c409-155">Ebben a példában a dátum és idő, szolgáltatásnév, kérelemazonosító, kérelem IP-cím és műveletnév tartalmazó karakterláncot a rendszer naplózza.</span><span class="sxs-lookup"><span data-stu-id="5c409-155">In this example a string containing the date and time, service name, request id, request ip address, and operation name is logged.</span></span>

<span data-ttu-id="5c409-156">Kattintson a **mentése** a frissített házirend konfigurációjának mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="5c409-156">Click **Save** to save the updated policy configuration.</span></span> <span data-ttu-id="5c409-157">Amint a rendszer menti a házirend és aktív eseményeket naplózza a kijelölt eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="5c409-157">As soon as it is saved the policy is active and events are logged to the designated Event Hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c409-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c409-158">Next steps</span></span>
* <span data-ttu-id="5c409-159">További tudnivalók az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5c409-159">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="5c409-160">Ismerkedés az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5c409-160">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="5c409-161">Üzenetek fogadása az EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="5c409-161">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="5c409-162">Event Hubs programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="5c409-162">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="5c409-163">További tudnivalók az API Management és az Event Hubs-integráció</span><span class="sxs-lookup"><span data-stu-id="5c409-163">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="5c409-164">Naplózó entitáshivatkozás</span><span class="sxs-lookup"><span data-stu-id="5c409-164">Logger entity reference</span></span>](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [<span data-ttu-id="5c409-165">napló-eventhub szabályzatainak ismertetése</span><span class="sxs-lookup"><span data-stu-id="5c409-165">log-to-eventhub policy reference</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [<span data-ttu-id="5c409-166">Az API-kat az Azure API Management, az Event Hubs és Runscope figyelése</span><span class="sxs-lookup"><span data-stu-id="5c409-166">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a><span data-ttu-id="5c409-167">Bemutató videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="5c409-167">Watch a video walkthrough</span></span>
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
