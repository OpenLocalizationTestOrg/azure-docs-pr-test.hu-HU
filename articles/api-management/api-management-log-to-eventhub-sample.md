---
title: "Az Azure API Management, az Event Hubs és Runscope API-k figyelése |} Microsoft Docs"
description: "A mintaalkalmazás, amely tartalmazza a kapcsolódó Azure API Management, az Azure Event Hubs és a HTTP-naplózás és figyelés Runscope napló eventhub házirend"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 70ee752c5639c90f77dde104ce85eec0a1062300
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a><span data-ttu-id="9537b-103">Az API-kat az Azure API Management, az Event Hubs és Runscope figyelése</span><span class="sxs-lookup"><span data-stu-id="9537b-103">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>
<span data-ttu-id="9537b-104">A [API-kezelés szolgáltatás](api-management-key-concepts.md) javítása érdekében a HTTP API küldött HTTP-kérelmek feldolgozási sok képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="9537b-104">The [API Management service](api-management-key-concepts.md) provides many capabilities to enhance the processing of HTTP requests sent to your HTTP API.</span></span> <span data-ttu-id="9537b-105">Azonban a kérések és válaszok átmeneti jellegűek.</span><span class="sxs-lookup"><span data-stu-id="9537b-105">However, the existence of the requests and responses are transient.</span></span> <span data-ttu-id="9537b-106">A kérelem, és azt a háttér-API számára az API Management szolgáltatáson keresztül zajlik.</span><span class="sxs-lookup"><span data-stu-id="9537b-106">The request is made and it flows through the API Management service to your backend API.</span></span> <span data-ttu-id="9537b-107">Az API-feldolgozza a kérést, és választ áthaladó vissza az API-fogyasztó számára.</span><span class="sxs-lookup"><span data-stu-id="9537b-107">Your API processes the request and a response flows back through to the API consumer.</span></span> <span data-ttu-id="9537b-108">Az API Management szolgáltatás tartja néhány fontos statisztikai adat kapcsolatos való megjelenítéshez. az API-k Publisher portál irányítópultján, de túl eltűnnek róla, hogy a részletek.</span><span class="sxs-lookup"><span data-stu-id="9537b-108">The API Management service keeps some important statistics about the APIs for display in the Publisher portal dashboard, but beyond that, the details are gone.</span></span>

<span data-ttu-id="9537b-109">Használatával a [napló eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [házirend](api-management-howto-policies.md) az API Management szolgáltatásban küldhet minden adatát a kérelem és válasz egy [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="9537b-109">By using the [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [policy](api-management-howto-policies.md) in the API Management service you can send any details from the request and response to an [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span> <span data-ttu-id="9537b-110">Nincsenek számos okból miért érdemes lehet események generálása küldi el az API-kat a HTTP-üzenetek.</span><span class="sxs-lookup"><span data-stu-id="9537b-110">There are a variety of reasons why you may want to generate events from HTTP messages being sent to your APIs.</span></span> <span data-ttu-id="9537b-111">Például frissítések, a használatelemzés, a kivétel riasztások és a 3. fél integrációja napló.</span><span class="sxs-lookup"><span data-stu-id="9537b-111">Some examples include audit trail of updates, usage analytics, exception alerting and 3rd party integrations.</span></span>   

<span data-ttu-id="9537b-112">Ez a cikk bemutatja, hogyan rögzítése a teljes HTTP kérelem-válasz üzenet. elküldi a Eseményközpontban, majd továbbítják a harmadik fél-szolgáltatás, amely HTTP-naplózás és -szolgáltatások figyelésének üzenetet.</span><span class="sxs-lookup"><span data-stu-id="9537b-112">This article demonstrates how to capture the entire HTTP request and response message, send it to an Event Hub and then relay that message to a third party service that provides HTTP logging and monitoring services.</span></span>

## <a name="why-send-from-api-management-service"></a><span data-ttu-id="9537b-113">Miért küldése az API Management szolgáltatást?</span><span class="sxs-lookup"><span data-stu-id="9537b-113">Why Send From API Management Service?</span></span>
<span data-ttu-id="9537b-114">Akkor lehet, amely HTTP-kérések és válaszok, majd azokat a naplózás és figyelés rendszerek hírcsatorna HTTP API keretrendszerek csatlakoztatható HTTP köztes írni.</span><span class="sxs-lookup"><span data-stu-id="9537b-114">It is possible to write HTTP middleware that can plug into HTTP API frameworks to capture HTTP requests and responses and feed them into logging and monitoring systems.</span></span> <span data-ttu-id="9537b-115">A hátránya, hogy ezt a módszert használja a HTTP köztes a háttér-API integrálni kell, és meg kell egyeznie a platform API.</span><span class="sxs-lookup"><span data-stu-id="9537b-115">The downside to this approach is the HTTP middleware needs to be integrated into the backend API and must match the platform of the API.</span></span> <span data-ttu-id="9537b-116">Ha több API-k minden egyes a köztes kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="9537b-116">If there are multiple APIs then each one must deploy the middleware.</span></span> <span data-ttu-id="9537b-117">Gyakran oka miért háttér API-k nem lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="9537b-117">Often there are reasons why backend APIs cannot be updated.</span></span>

<span data-ttu-id="9537b-118">Naplózási infrastruktúra integrálása az Azure API Management szolgáltatással központosított és platformfüggetlen megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="9537b-118">Using the Azure API Management service to integrate with logging infrastructure provides a centralized and platform-independent solution.</span></span> <span data-ttu-id="9537b-119">Célszerű is méretezhető, részben oka az, hogy a [georeplikáció](api-management-howto-deploy-multi-region.md) Azure API-felügyeleti képességeit.</span><span class="sxs-lookup"><span data-stu-id="9537b-119">It is also scalable, in part due to the [geo-replication](api-management-howto-deploy-multi-region.md) capabilities of Azure API Management.</span></span>

## <a name="why-send-to-an-azure-event-hub"></a><span data-ttu-id="9537b-120">Miért küldeni az Azure Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="9537b-120">Why send to an Azure Event Hub?</span></span>
<span data-ttu-id="9537b-121">Kérje meg, miért a szabályzatban csak az Azure Event Hubs egy ésszerű?</span><span class="sxs-lookup"><span data-stu-id="9537b-121">It is a reasonable to ask, why create a policy that is specific to Azure Event Hubs?</span></span> <span data-ttu-id="9537b-122">Előfordulhat, hogy kívánt bejelentkezni a saját kérések különböző helyeken van.</span><span class="sxs-lookup"><span data-stu-id="9537b-122">There are many different places where I might want to log my requests.</span></span> <span data-ttu-id="9537b-123">Miért nem csupán a kérelmeket küldeni közvetlenül a végső rendeltetési?</span><span class="sxs-lookup"><span data-stu-id="9537b-123">Why not just send the requests directly to the final destination?</span></span>  <span data-ttu-id="9537b-124">Ez a beállítás.</span><span class="sxs-lookup"><span data-stu-id="9537b-124">That is an option.</span></span> <span data-ttu-id="9537b-125">Azonban az API management szolgáltatás naplózási kérelmeinek meghozásakor fontos figyelembe venni, hogyan befolyásolják naplózási üzeneteket az API teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="9537b-125">However, when making logging requests from an API management service, it is necessary to consider how logging messages will impact the performance of the API.</span></span> <span data-ttu-id="9537b-126">Fokozatos nő a terhelés rendszerösszetevő elérhető példányok növelésével vagy georeplikáció kihasználásával lehet kezelni.</span><span class="sxs-lookup"><span data-stu-id="9537b-126">Gradual increases in load can be handled by increasing available instances of system components or by taking advantage of geo-replication.</span></span> <span data-ttu-id="9537b-127">Azonban a forgalom rövid igényeiben jelentkező okozhat kérelmek jelentősen később, ha lassú terhelés alatt naplózási infrastruktúra kérelmek indul el.</span><span class="sxs-lookup"><span data-stu-id="9537b-127">However, short spikes in traffic can cause requests to be significantly delayed if requests to logging infrastructure start to slow under load.</span></span>

<span data-ttu-id="9537b-128">Az Azure Event Hubs célja, hogy hatalmas mennyiségű érkező adatokat, az események sokkal nagyobb számú foglalkoznak, mint a HTTP-kérések száma legtöbb API-k folyamat kapacitással rendelkező átjáróeszközt.</span><span class="sxs-lookup"><span data-stu-id="9537b-128">The Azure Event Hubs is designed to ingress huge volumes of data, with capacity for dealing with a far higher number of events than the number of HTTP requests most APIs process.</span></span> <span data-ttu-id="9537b-129">Az Event Hubs kifinomult a API management és az infrastruktúra, amely fogja tárolni, és az üzenetek feldolgozásához között egy közvetítő funkcionál.</span><span class="sxs-lookup"><span data-stu-id="9537b-129">The Event Hub acts as a kind of sophisticated buffer between your API management service and the infrastructure that will store and process the messages.</span></span> <span data-ttu-id="9537b-130">Ez biztosítja, hogy a a API teljesítménye nem érinti a naplózás infrastruktúra miatt.</span><span class="sxs-lookup"><span data-stu-id="9537b-130">This ensures that your API performance will not suffer due to the logging infrastructure.</span></span>  

<span data-ttu-id="9537b-131">Ha az adatokat adtak át az Eseményközpontok tárolva, és megvárja, hogy az Event Hubs fogyasztók feldolgozni azt.</span><span class="sxs-lookup"><span data-stu-id="9537b-131">Once the data has been passed to an Event Hub it is persisted and will wait for Event Hub consumers to process it.</span></span> <span data-ttu-id="9537b-132">Az Event Hubs nem ügyeljen arra, hogyan fogja feldolgozni, akkor csak ügyel meggyőződött arról, hogy az üzenet sikeresen kézbesíti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="9537b-132">The Event Hub does not care how it will be processed, it just cares about making sure the message will be successfully delivered.</span></span>     

<span data-ttu-id="9537b-133">Az Event Hubs képesek az adatfolyam események több felhasználói csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="9537b-133">Event Hubs have the ability to stream events to multiple consumer groups.</span></span> <span data-ttu-id="9537b-134">Ez lehetővé teszi, hogy teljesen más rendszerek által feldolgozandó események.</span><span class="sxs-lookup"><span data-stu-id="9537b-134">This allows events to be processed by completely different systems.</span></span> <span data-ttu-id="9537b-135">Ez lehetővé teszi több integrációs forgatókönyv támogatása nem helyezi hozzáadása késlelteti az API Management szolgáltatáson belül az API-kérés feldolgozása csak egy eseménynapló igények generálását.</span><span class="sxs-lookup"><span data-stu-id="9537b-135">This enables supporting many integration scenarios without putting addition delays on the processing of the API request within the API Management service as only one event needs to be generated.</span></span>

## <a name="a-policy-to-send-applicationhttp-messages"></a><span data-ttu-id="9537b-136">Alkalmazás/HTTP-üzenetek küldése egy házirend</span><span class="sxs-lookup"><span data-stu-id="9537b-136">A policy to send application/http messages</span></span>
<span data-ttu-id="9537b-137">Az Eseményközpontok eseményadatok egyszerű karakterláncként fogad el.</span><span class="sxs-lookup"><span data-stu-id="9537b-137">An Event Hub accepts event data as a simple string.</span></span> <span data-ttu-id="9537b-138">A karakterláncokat tartalma teljes mértékben Öntől függ.</span><span class="sxs-lookup"><span data-stu-id="9537b-138">The contents of that string are completely up to you.</span></span> <span data-ttu-id="9537b-139">HTTP-kérelem becsomagolhatja, és küldése az Event Hubs kell a formázó karakterlánc, a kérés vagy válasz adatokkal.</span><span class="sxs-lookup"><span data-stu-id="9537b-139">To be able to package up an HTTP request and send it off to Event Hubs we need to format the string with the request or response information.</span></span> <span data-ttu-id="9537b-140">Olyan esetekben, például egy meglévő formátum esetén újrafelhasználásához, majd saját elemzése kód írása előfordulhat, hogy nincs.</span><span class="sxs-lookup"><span data-stu-id="9537b-140">In situations like this, if there is an existing format we can reuse, then we may not have to write our own parsing code.</span></span> <span data-ttu-id="9537b-141">Kezdetben szeretnék venni használatával a [HAR](http://www.softwareishard.com/blog/har-12-spec/) HTTP-kérések és válaszok küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="9537b-141">Initially I considered using the [HAR](http://www.softwareishard.com/blog/har-12-spec/) for sending HTTP requests and responses.</span></span> <span data-ttu-id="9537b-142">Azonban ez a formátum tárolására van optimalizálva HTTP-kérelmek sorozatát alapú JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="9537b-142">However, this format is optimized for storing a sequence of HTTP requests in a JSON based format.</span></span> <span data-ttu-id="9537b-143">A forgatókönyvet a HTTP-üzenet átadja a hálózaton keresztül a szükségtelen összetettsége hozzáadott kötelező elemek számos tartalmazott.</span><span class="sxs-lookup"><span data-stu-id="9537b-143">It contained a number of mandatory elements that added unnecessary complexity for the scenario of passing the HTTP message over the wire.</span></span>  

<span data-ttu-id="9537b-144">Alternatív lehetőségként volt, hogy a `application/http` médiatípus lásd: a HTTP-specifikáció [RFC 7230](http://tools.ietf.org/html/rfc7230).</span><span class="sxs-lookup"><span data-stu-id="9537b-144">An alternative option was to use the `application/http` media type as described in the HTTP specification [RFC 7230](http://tools.ietf.org/html/rfc7230).</span></span> <span data-ttu-id="9537b-145">A médiatípus pontos ugyanazt a formátumot használja, ténylegesen a hálózaton keresztül a HTTP-üzenetek küldéséhez használt, de a teljes üzenet egy másik HTTP-kérelem törzse be lehet.</span><span class="sxs-lookup"><span data-stu-id="9537b-145">This media type uses the exact same format that is used to actually send HTTP messages over the wire, but the entire message can be put in the body of another HTTP request.</span></span> <span data-ttu-id="9537b-146">A mi esetünkben csak fogjuk a szervezet használja az üzenet küldése az Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="9537b-146">In our case we are just going to use the body as our message to send to Event Hubs.</span></span> <span data-ttu-id="9537b-147">Már szerepel egy elemző van kényelmesen, [Microsoft ASP.NET Web API 2.2 ügyfél](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) tárak, amelyek elemezni ezt a formátumot, majd átalakíthatja a natív `HttpRequestMessage` és `HttpResponseMessage` objektumok.</span><span class="sxs-lookup"><span data-stu-id="9537b-147">Conveniently, there is a parser that exists in [Microsoft ASP.NET Web API 2.2 Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) libraries that can parse this format and convert it into the native `HttpRequestMessage` and `HttpResponseMessage` objects.</span></span>

<span data-ttu-id="9537b-148">Ez az üzenet előnyeit C#-alapú kell létrehozni a [házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx) az Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="9537b-148">To be able to create this message we need to take advantage of C# based [Policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management.</span></span> <span data-ttu-id="9537b-149">Ez a házirendet, amely HTTP-kérelem üzenetet küld az Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="9537b-149">Here is the policy which sends a HTTP request message to Azure Event Hubs.</span></span>

```xml
<log-to-eventhub logger-id="conferencelogger" partition-id="0">
@{
   var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                               context.Request.Method,
                                               context.Request.Url.Path + context.Request.Url.QueryString);

   var body = context.Request.Body?.As<string>(true);
   if (body != null && body.Length > 1024)
   {
       body = body.Substring(0, 1024);
   }

   var headers = context.Request.Headers
                          .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                          .ToArray<string>();

   var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

   return "request:"   + context.Variables["message-id"] + "\n"
                       + requestLine + headerString + "\r\n" + body;
}
</log-to-eventhub>
```

### <a name="policy-declaration"></a><span data-ttu-id="9537b-150">Házirend nyilatkozat</span><span class="sxs-lookup"><span data-stu-id="9537b-150">Policy declaration</span></span>
<span data-ttu-id="9537b-151">Nincs adott szempontokat érdemes megemlíteni kapcsolatban a házirend-kifejezést.</span><span class="sxs-lookup"><span data-stu-id="9537b-151">There a few particular things worth mentioning about this policy expression.</span></span> <span data-ttu-id="9537b-152">A napló eventhub házirendben engedélyezve van a naplózó-azonosítója, amely az API Management szolgáltatáson belül lett létrehozva naplózó nevére hivatkozik nevű attribútum.</span><span class="sxs-lookup"><span data-stu-id="9537b-152">The log-to-eventhub policy has an attribute called logger-id which refers to the name of logger that has been created within the API Management service.</span></span> <span data-ttu-id="9537b-153">A dokumentum részletesen ismerteti az API Management szolgáltatásban egy Eseményközpontba naplózó beállítása található [hogyan naplózza az eseményeket az Azure Event Hubs az Azure API Management](api-management-howto-log-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="9537b-153">The details of how to setup an Event Hub logger in the API Management service can be found in the document [How to log events to Azure Event Hubs in Azure API Management](api-management-howto-log-event-hubs.md).</span></span> <span data-ttu-id="9537b-154">A második attribútumot nem kötelező paraméter, amely arra utasítja az Event Hubs, amely az üzenet tárolására partícióazonosító.</span><span class="sxs-lookup"><span data-stu-id="9537b-154">The second attribute is an optional parameter that instructs Event Hubs which partition to store the message in.</span></span> <span data-ttu-id="9537b-155">Az Event Hubs használjon partíciókat scalabilty engedélyezése, és legalább két kérheti.</span><span class="sxs-lookup"><span data-stu-id="9537b-155">Event Hubs use partitions to enable scalabilty and require a minimum of two.</span></span> <span data-ttu-id="9537b-156">Az üzenetek a rendezett kézbesítést csak garantáltan a partíción belül.</span><span class="sxs-lookup"><span data-stu-id="9537b-156">The ordered delivery of messages is only guaranteed within a partition.</span></span> <span data-ttu-id="9537b-157">A Microsoft arra utasítani az Event Hubs mely partíció helyezhető el az üzenetet, ha a terhelés elosztása a ciklikus multiplexelési algoritmus fogja használni.</span><span class="sxs-lookup"><span data-stu-id="9537b-157">If we do not instruct Event Hub in which partition to place the message, it will use a round-robin algorithm to distribute the load.</span></span> <span data-ttu-id="9537b-158">Azonban a nem megfelelő sorrendben feldolgozott üzenetek némelyike esetleg okozó.</span><span class="sxs-lookup"><span data-stu-id="9537b-158">However, that may cause some of our messages to be processed out of order.</span></span>  

### <a name="partitions"></a><span data-ttu-id="9537b-159">Partíciók</span><span class="sxs-lookup"><span data-stu-id="9537b-159">Partitions</span></span>
<span data-ttu-id="9537b-160">Az üzenetek fogyasztóknak sorrendben érkeznek, valamint partíciók terhelés terjesztési képességének kihasználása érdekében elfogadása HTTP-kérelem üzenetek küldése egy partíciót, és a HTTP-válasz üzenetek egy másik partícióra.</span><span class="sxs-lookup"><span data-stu-id="9537b-160">To ensure our messages are delivered to consumers in order and take advantage of the load distribution capability of partitions, I chose to send HTTP request messages to one partition and HTTP response messages to a second partition.</span></span> <span data-ttu-id="9537b-161">Ezzel biztosíthatja, hogy még akkor is, egy terheléselosztási és garantáljuk, hogy minden kérésnél fognak használni ahhoz, és minden válasz fognak használni ahhoz.</span><span class="sxs-lookup"><span data-stu-id="9537b-161">This will ensure an even load distribution and we can guarantee that all requests will be consumed in order and all responses will be consumed in order.</span></span> <span data-ttu-id="9537b-162">Lehetséges, hogy a vonatkozó kérés előtt fel választ, de, amely nincs probléma van a Microsoft által küldött válaszokhoz kérelmeket egy másik mechanizmus használatával történik, és tudjuk, hogy kérelmek mindig előznie válaszokat.</span><span class="sxs-lookup"><span data-stu-id="9537b-162">It is possible for a response to be consumed before the corresponding request, but as that is not a problem as we have a different mechanism for correlating requests to responses and we know that requests always come before responses.</span></span>

### <a name="http-payloads"></a><span data-ttu-id="9537b-163">HTTP hasznos adat található</span><span class="sxs-lookup"><span data-stu-id="9537b-163">HTTP payloads</span></span>
<span data-ttu-id="9537b-164">Létrehozása után a `requestLine` azt ellenőrzi, hogy ha a kérés törzsében csonkolva lesz.</span><span class="sxs-lookup"><span data-stu-id="9537b-164">After building the `requestLine` we check to see if the request body should be truncated.</span></span> <span data-ttu-id="9537b-165">A kérelem törzsében csak 1024 méretűre csonkolja.</span><span class="sxs-lookup"><span data-stu-id="9537b-165">The request body is truncated to only 1024.</span></span> <span data-ttu-id="9537b-166">Ez növekedhet, de egyes Eseményközpont üzenetek legfeljebb 256KB, akkor valószínű, hogy néhány HTTP-üzenet szervek lesz nem elférjen egyetlen üzenetben.</span><span class="sxs-lookup"><span data-stu-id="9537b-166">This could be increased, however individual Event Hub messages are limited to 256KB, so it is likely that some HTTP message bodies will not fit in a single message.</span></span> <span data-ttu-id="9537b-167">Naplózás és az elemzés során jelentős mennyiségű információt származtatható csak a HTTP-kérelem sor és a fejlécet.</span><span class="sxs-lookup"><span data-stu-id="9537b-167">When doing logging and analytics a significant amount of information can be derived from just the HTTP request line and headers.</span></span> <span data-ttu-id="9537b-168">Is sok API-kérelmek csak térjen vissza a kisebb szervezetek és nagy szervezetek betartására értékeinek megszűnését viszonylag minimális tartani minden szervezet tartalma átviteli, feldolgozása és tárolási költségek csökkentése szemben.</span><span class="sxs-lookup"><span data-stu-id="9537b-168">Also, many API requests only return small bodies and so the loss of information value by truncating large bodies is fairly minimal in comparison to the reduction in transfer, processing and storage costs to keep all body contents.</span></span> <span data-ttu-id="9537b-169">Egy végső Megjegyzés a törzsének feldolgozására a rendszer, hogy át kell `true` való az As<string>() metódus, mert azt azért olvassák el a szervezet tartalmát, de lett is szeretné, hogy a háttér-API el tudják olvasni a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="9537b-169">One final note about processing the body is that we need to pass `true` to the As<string>() method because we are reading the body contents, but was also want the backend API to be able to read the body.</span></span> <span data-ttu-id="9537b-170">Ez a módszer az IGAZ értéket történő azt miatt a szervezet számára, hogy azt még egyszer olvasható pufferbe kerüljön.</span><span class="sxs-lookup"><span data-stu-id="9537b-170">By passing true to this method we cause the body to be buffered so that it can be read a second time.</span></span> <span data-ttu-id="9537b-171">Ez azért fontos tudni, ha az API-k, amelyen a nagyon nagy méretű fájlok feltöltése vagy hosszú lekérdezési használja.</span><span class="sxs-lookup"><span data-stu-id="9537b-171">This is important to be aware of if you have an API that does uploading of very large files or uses long polling.</span></span> <span data-ttu-id="9537b-172">Ezekben az esetekben a szervezet minden olvasási elkerülése érdekében ajánlott lenne.</span><span class="sxs-lookup"><span data-stu-id="9537b-172">In these cases it would be best to avoid reading the body at all.</span></span>   

### <a name="http-headers"></a><span data-ttu-id="9537b-173">HTTP-fejlécek</span><span class="sxs-lookup"><span data-stu-id="9537b-173">HTTP headers</span></span>
<span data-ttu-id="9537b-174">HTTP-fejléceket is egyszerűen továbbítja egyszerű kulcs/érték pár formátumban üzenet formátumra alakítja.</span><span class="sxs-lookup"><span data-stu-id="9537b-174">HTTP Headers can be simply transferred over into the message format in a simple key/value pair format.</span></span> <span data-ttu-id="9537b-175">Azt választotta ki feleslegesen a hitelesítő adatok megakadályozására elkerülése érdekében bizonyos biztonsági időérzékeny mezőinek.</span><span class="sxs-lookup"><span data-stu-id="9537b-175">We have chosen to strip out certain security sensitive fields, to avoid unnecessarily leaking credential information.</span></span> <span data-ttu-id="9537b-176">Nem valószínű, hogy API-kulcsokat és más hitelesítő adatokat használni elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="9537b-176">It is unlikely that API keys and other credentials would be used for analytics purposes.</span></span> <span data-ttu-id="9537b-177">Ha azt szeretné a felhasználó és az adott termék elemzést használják, akkor azt, hogy a lehetett beolvasni a `context` objektumra, és adja hozzá, amely az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="9537b-177">If we wish to do analysis on the user and the particular product they are using then we could get that from the `context` object and add that to the message.</span></span>     

### <a name="message-metadata"></a><span data-ttu-id="9537b-178">Üzenet metaadatok</span><span class="sxs-lookup"><span data-stu-id="9537b-178">Message Metadata</span></span>
<span data-ttu-id="9537b-179">A teljes üzenetet küldeni az eseményközpont létrehozásakor az első sor nincs ténylegesen része a `application/http` üzenet.</span><span class="sxs-lookup"><span data-stu-id="9537b-179">When building the complete message to send to the event hub, the first line is not actually part of the `application/http` message.</span></span> <span data-ttu-id="9537b-180">Az első sorban álló-e az üzenetet a kérelem vagy válaszüzenetet és egy üzenetazonosítója által küldött válaszokhoz kérelmek összefüggéseket szolgáló metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="9537b-180">The first line is additional metadata consisting of whether the message is a request or response message and a message id which is used to correlate requests to responses.</span></span> <span data-ttu-id="9537b-181">Az üzenetazonosító létrejön egy másik-szabályzattal, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="9537b-181">The message id is created by using another policy that looks like this:</span></span>

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

<span data-ttu-id="9537b-182">Azt a kérelemüzenet létrehozott, egy változó tárolja, amíg a válaszban visszaadott és küldi el egyszerűen a rendszer a kérelem-válasz egy üzenet, sikerült.</span><span class="sxs-lookup"><span data-stu-id="9537b-182">We could have created the request message, stored that in a variable until the response was returned and then simply sent the request and response as a single message.</span></span> <span data-ttu-id="9537b-183">Azonban a kérés- és egymástól függetlenül küldésével, és a két összefüggéseket üzenetet azonosító használatával, azt lekérése egy kicsit nagyobb rugalmasságot biztosít az üzenet mérete, a képes több partíció, miközben megtartja üzenet rendelés és a kérelem jelennek meg a naplózási irányítópult hamarabb kihasználásához.</span><span class="sxs-lookup"><span data-stu-id="9537b-183">However, by sending the request and response independently and using a message id to correlate the two, we get a bit more flexibility in the message size, the ability to take advantage of multiple partitions whilst maintaining message order and the request will appear in our logging dashboard sooner.</span></span> <span data-ttu-id="9537b-184">Előfordulhat, hogy is van bizonyos esetekben, ahol egy érvényes válasz nem érkezik az event hubs, valószínűleg egy kérelem végzetes hiba történt az API Management szolgáltatásban, de azt továbbra is rendelkeznek egy bejegyzést a kérelem.</span><span class="sxs-lookup"><span data-stu-id="9537b-184">There also may be some scenarios where a valid response is never sent to the event hub, possibly due to a fatal request error in the API Management service, but we still will have a record of the request.</span></span>

<span data-ttu-id="9537b-185">A HTTP válaszüzenetet küldeni a házirend nagyon hasonlít-e a kérelmet, és így a teljes házirend-konfiguráció néz ki:</span><span class="sxs-lookup"><span data-stu-id="9537b-185">The policy to send the response HTTP message looks very similar to the request and so the complete policy configuration looks like this:</span></span>

```xml
<policies>
  <inbound>
      <set-variable name="message-id" value="@(Guid.NewGuid())" />
      <log-to-eventhub logger-id="conferencelogger" partition-id="0">
      @{
          var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                      context.Request.Method,
                                                      context.Request.Url.Path + context.Request.Url.QueryString);

          var body = context.Request.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Request.Headers
                               .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                               .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                               .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "request:"   + context.Variables["message-id"] + "\n"
                              + requestLine + headerString + "\r\n" + body;
      }
  </log-to-eventhub>
  </inbound>
  <backend>
      <forward-request follow-redirects="true" />
  </backend>
  <outbound>
      <log-to-eventhub logger-id="conferencelogger" partition-id="1">
      @{
          var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                              context.Response.StatusCode,
                                              context.Response.StatusReason);

          var body = context.Response.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Response.Headers
                                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                          .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "response:"  + context.Variables["message-id"] + "\n"
                              + statusLine + headerString + "\r\n" + body;
     }
  </log-to-eventhub>
  </outbound>
</policies>
```

<span data-ttu-id="9537b-186">A `set-variable` házirend által egyaránt elérhető értéket hoz létre a `log-to-eventhub` a házirend a `<inbound>` szakasz és a `<outbound>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="9537b-186">The `set-variable` policy creates a value that is accessible by both the `log-to-eventhub` policy in the `<inbound>` section and the `<outbound>` section.</span></span>  

## <a name="receiving-events-from-event-hubs"></a><span data-ttu-id="9537b-187">Az események fogadását az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="9537b-187">Receiving events from Event Hubs</span></span>
<span data-ttu-id="9537b-188">Azure Event Hubs eseményeit fogadásának használatával a [AMQP protokoll](http://www.amqp.org/).</span><span class="sxs-lookup"><span data-stu-id="9537b-188">Events from Azure Event Hub are received using the [AMQP protocol](http://www.amqp.org/).</span></span> <span data-ttu-id="9537b-189">A Microsoft Service Bus csapatával végzett ügyfél megkönnyítése érdekében a fogyasztó események elérhető szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="9537b-189">The Microsoft Service Bus team have made client libraries available to make the consuming events easier.</span></span> <span data-ttu-id="9537b-190">Támogatott két különböző megközelítés, egy folyamatban van egy *közvetlen fogyasztói* és egyéb-t használ a `EventProcessorHost` osztály.</span><span class="sxs-lookup"><span data-stu-id="9537b-190">There are two different approaches supported, one is being a *Direct Consumer* and the other is using the `EventProcessorHost` class.</span></span> <span data-ttu-id="9537b-191">E két megközelítés példái megtalálhatók a [Event Hubs programozási útmutató](../event-hubs/event-hubs-programming-guide.md).</span><span class="sxs-lookup"><span data-stu-id="9537b-191">Examples of these two approaches can be found in the [Event Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).</span></span> <span data-ttu-id="9537b-192">A különbségek rövid verziószáma, `Direct Consumer` lehetővé teszi az irányítást és a `EventProcessorHost` nem egy része a bekötése, az azonban lehetővé teszi bizonyos feltételezéseket hogyan feldolgozza ezeket az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="9537b-192">The short version of the differences is, `Direct Consumer` gives you complete control and the `EventProcessorHost` does some of the plumbing work for you but makes certain assumptions about how you will process those events.</span></span>  

### <a name="eventprocessorhost"></a><span data-ttu-id="9537b-193">EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="9537b-193">EventProcessorHost</span></span>
<span data-ttu-id="9537b-194">A példában használjuk a `EventProcessorHost` az egyszerűség kedvéért azonban akkor lehetséges, hogy ehhez a forgatókönyvhöz nem a legjobb választás.</span><span class="sxs-lookup"><span data-stu-id="9537b-194">In this sample, we will use the `EventProcessorHost` for simplicity, however it may not the best choice for this particular scenario.</span></span> <span data-ttu-id="9537b-195">`EventProcessorHost`nem meggyőződött arról, hogy nem kell foglalkoznia az egy adott esemény processzor osztályon belül problémák szálkezelési rögzített munkáját.</span><span class="sxs-lookup"><span data-stu-id="9537b-195">`EventProcessorHost` does the hard work of making sure you don't have to worry about threading issues within a particular event processor class.</span></span> <span data-ttu-id="9537b-196">Azonban a mi esetünkben azt egyszerűen az üzenet konvertálása más formátumra és átadja mentén azt egy másik szolgáltatást egy aszinkron metódus használatával.</span><span class="sxs-lookup"><span data-stu-id="9537b-196">However, in our scenario, we are simply converting the message to another format and passing it along to another service using an async method.</span></span> <span data-ttu-id="9537b-197">Nincs szükség megosztott állapotot, és ezért kockázata problémák szálkezelési frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9537b-197">There is no need for updating shared state and therefore no risk of threading issues.</span></span> <span data-ttu-id="9537b-198">A legtöbb esetben `EventProcessorHost` valószínűleg a legjobb választás, és biztosan a könnyebb beállítás.</span><span class="sxs-lookup"><span data-stu-id="9537b-198">For most scenarios, `EventProcessorHost` is probably the best choice and it is certainly the easier option.</span></span>     

### <a name="ieventprocessor"></a><span data-ttu-id="9537b-199">IEventProcessor</span><span class="sxs-lookup"><span data-stu-id="9537b-199">IEventProcessor</span></span>
<span data-ttu-id="9537b-200">A központi koncepció használatakor `EventProcessorHost` létrehozása egy megvalósítása a `IEventProcessor` illesztőfelületet, amely tartalmazza a módszert `ProcessEventAsync`.</span><span class="sxs-lookup"><span data-stu-id="9537b-200">The central concept when using `EventProcessorHost` is to create a an implementation of the `IEventProcessor` interface which contains the method `ProcessEventAsync`.</span></span> <span data-ttu-id="9537b-201">Ez a módszer lényege itt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="9537b-201">The essence of that method is shown here:</span></span>

```c#
async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
{

   foreach (EventData eventData in messages)
   {
       _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

       try
       {
           var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
           await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
       }
       catch (Exception ex)
       {
           _Logger.LogError(ex.Message);
       }
   }
    ... checkpointing code snipped ...
}
```

<span data-ttu-id="9537b-202">A metódusnak átadott EventData objektumok listáját, és azt, hogy a lista ismétlés.</span><span class="sxs-lookup"><span data-stu-id="9537b-202">A list of EventData objects are passed into the method and we iterate over that list.</span></span> <span data-ttu-id="9537b-203">Az egyes módszerek bájt elemzésének HttpMessage objektumba és, hogy az objektum egy példányát IHttpMessageProcessor objektumnak átadott.</span><span class="sxs-lookup"><span data-stu-id="9537b-203">The bytes of each method are parsed into a HttpMessage object and that object is passed to an instance of IHttpMessageProcessor.</span></span>

### <a name="httpmessage"></a><span data-ttu-id="9537b-204">HttpMessage</span><span class="sxs-lookup"><span data-stu-id="9537b-204">HttpMessage</span></span>
<span data-ttu-id="9537b-205">A `HttpMessage` példány három adatra adatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="9537b-205">The `HttpMessage` instance contains three pieces of data:</span></span>

```c#
public class HttpMessage
{
   public Guid MessageId { get; set; }
   public bool IsRequest { get; set; }
   public HttpRequestMessage HttpRequestMessage { get; set; }
   public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

<span data-ttu-id="9537b-206">A `HttpMessage` példány tartalmaz egy `MessageId` GUID, amely lehetővé teszi a HTTP-kérelem csatlakozhatnak a megfelelő HTTP-válasz és egy logikai érték, amely azt jelzi, hogy az objektum tartalmaz egy HttpRequestMessage és HttpResponseMessage példánya.</span><span class="sxs-lookup"><span data-stu-id="9537b-206">The `HttpMessage` instance contains a `MessageId` GUID that allows us to connect the HTTP request to the corresponding HTTP response and a boolean value that identifies if the object contains an instance of a HttpRequestMessage and HttpResponseMessage.</span></span> <span data-ttu-id="9537b-207">A HTTP-osztályok a beépített használatával `System.Net.Http`, sikerült előnyeit a `application/http` található kód elemzése `System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="9537b-207">By using the built in HTTP classes from `System.Net.Http`, I was able to take advantage of the `application/http` parsing code that is included in `System.Net.Http.Formatting`.</span></span>  

### <a name="ihttpmessageprocessor"></a><span data-ttu-id="9537b-208">IHttpMessageProcessor</span><span class="sxs-lookup"><span data-stu-id="9537b-208">IHttpMessageProcessor</span></span>
<span data-ttu-id="9537b-209">A `HttpMessage` példány továbbítja végrehajtásának `IHttpMessageProcessor` Ez az illesztőfelület szolgáló használata leválasztja a fogadás és az Azure Event Hubs eseményben értelmezésének és a tényleges feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="9537b-209">The `HttpMessage` instance is then forwarded to implementation of `IHttpMessageProcessor` which is an interface I created to decouple the receiving and interpretation of the event from Azure Event Hub and the actual processing of it.</span></span>

## <a name="forwarding-the-http-message"></a><span data-ttu-id="9537b-210">A HTTP üzenet továbbítása</span><span class="sxs-lookup"><span data-stu-id="9537b-210">Forwarding the HTTP message</span></span>
<span data-ttu-id="9537b-211">Ez a minta az I lezárását lenne érdekes, amelyekkel a HTTP-kérelem keresztül a [Runscope](http://www.runscope.com).</span><span class="sxs-lookup"><span data-stu-id="9537b-211">For this sample, I decided it would be interesting to push the HTTP Request over to [Runscope](http://www.runscope.com).</span></span> <span data-ttu-id="9537b-212">Runscope egy felhőalapú szolgáltatás, amely HTTP-hibakeresés, a naplózás és figyelés.</span><span class="sxs-lookup"><span data-stu-id="9537b-212">Runscope is a cloud based service that specializes in HTTP debugging, logging and monitoring.</span></span> <span data-ttu-id="9537b-213">Ingyenes szint, így könnyen próbálja, és lehetővé teszi a megállapítását, hogy a valós idejű haladnak keresztül az API-kezelés szolgáltatás a HTTP-kérelmek rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="9537b-213">They have a free tier, so it is easy to try and it allows us to see the HTTP requests in real-time flowing through our API Management service.</span></span>

<span data-ttu-id="9537b-214">A `IHttpMessageProcessor` megvalósítási néz ki,</span><span class="sxs-lookup"><span data-stu-id="9537b-214">The `IHttpMessageProcessor` implementation looks like this,</span></span>

```c#
public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
{
   private HttpClient _HttpClient;
   private ILogger _Logger;
   private string _BucketKey;
   public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
   {
       _HttpClient = httpClient;
       var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
       _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
       _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
       _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
       _Logger = logger;
   }

   public async Task ProcessHttpMessage(HttpMessage message)
   {
       var runscopeMessage = new RunscopeMessage()
       {
           UniqueIdentifier = message.MessageId
       };

       if (message.IsRequest)
       {
           _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
           runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
       }
       else
       {
           _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
           runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
       }

       var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
       messagesLink.BucketKey = _BucketKey;
       messagesLink.RunscopeMessage = runscopeMessage;
       var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
       _Logger.LogDebug("Request sent to Runscope");
   }
}
```

<span data-ttu-id="9537b-215">Sikerült kihasználása egy [meglévő ügyféloldali kódtára a Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) , amelyek segítségével könnyebben leküldéses `HttpRequestMessage` és `HttpResponseMessage` példányok be a szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="9537b-215">I was able to take advantage of an [existing client library for Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) that makes it easy to push `HttpRequestMessage` and `HttpResponseMessage` instances up into their service.</span></span> <span data-ttu-id="9537b-216">A Runscope API eléréséhez szüksége lesz a fiók és API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="9537b-216">In order to access the Runscope API you will need an account and an API Key.</span></span> <span data-ttu-id="9537b-217">API-kulcs beolvasása vonatkozó utasítások megtalálhatók a [hozzáférés Runscope API-alkalmazások létrehozása](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) képernyőfelvétel.</span><span class="sxs-lookup"><span data-stu-id="9537b-217">Instructions for getting an API key can be found in the [Creating Applications to Access Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) screencast.</span></span>

## <a name="complete-sample"></a><span data-ttu-id="9537b-218">Teljes mintát</span><span class="sxs-lookup"><span data-stu-id="9537b-218">Complete sample</span></span>
<span data-ttu-id="9537b-219">A [forráskód](https://github.com/darrelmiller/ApimEventProcessor) és meglétének ellenőrzése a mintát a Githubon.</span><span class="sxs-lookup"><span data-stu-id="9537b-219">The [source code](https://github.com/darrelmiller/ApimEventProcessor) and tests for the sample are on GitHub.</span></span> <span data-ttu-id="9537b-220">Szüksége lesz egy [API Management szolgáltatás](api-management-get-started.md), [a csatlakoztatott Eseményközpontot](api-management-howto-log-event-hubs.md), és egy [Tárfiók](../storage/common/storage-create-storage-account.md) a minta futtatásához a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9537b-220">You will need an [API Management Service](api-management-get-started.md), [a connected Event Hub](api-management-howto-log-event-hubs.md), and a [Storage Account](../storage/common/storage-create-storage-account.md) to run the sample for yourself.</span></span>   

<span data-ttu-id="9537b-221">A minta értéke csak egy egyszerű konzolalkalmazást, amely figyeli az Event Hubs származó események összefogásával alakítja őket egy `HttpRequestMessage` és `HttpResponseMessage` objektumokat, és ezután továbbítja őket a Runscope API be.</span><span class="sxs-lookup"><span data-stu-id="9537b-221">The sample is just a simple Console application that listens for events coming from Event Hub, converts them into a `HttpRequestMessage` and `HttpResponseMessage` objects and then forwards them on to the Runscope API.</span></span>

<span data-ttu-id="9537b-222">A következő animált kép tekintheti meg a kérelem egy API-t a fejlesztői portálra, az üzenet érkezett, feldolgozott és továbbított megjelenítő Konzolalkalmazás majd a kérelem és válasz jelenik meg a Runscope forgalom inspector történik.</span><span class="sxs-lookup"><span data-stu-id="9537b-222">In the following animated image, you can see a request being made to an API in the Developer Portal, the Console application showing the message being received, processed and forwarded and then the request and response showing up in the Runscope Traffic inspector.</span></span>

![A kérelem Runscope lesznek továbbítva bemutatója](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a><span data-ttu-id="9537b-224">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="9537b-224">Summary</span></span>
<span data-ttu-id="9537b-225">Az Azure API Management szolgáltatás Itt adható meg az ideális ül, illetve onnan az API-kat a HTTP-forgalom rögzítésére.</span><span class="sxs-lookup"><span data-stu-id="9537b-225">Azure API Management service provides an ideal place to capture the HTTP traffic travelling to and from your APIs.</span></span> <span data-ttu-id="9537b-226">Azure Event Hubs egy kiválóan méretezhető, alacsony költségű megoldás, hogy forgalom rögzítése és elágazó azt másodlagos feldolgozási rendszerek naplózási, figyelés, és más kifinomult analytics.</span><span class="sxs-lookup"><span data-stu-id="9537b-226">Azure Event Hubs is a highly scalable, low cost solution for capturing that traffic and feeding it into secondary processing systems for logging, monitoring and other sophisticated analytics.</span></span> <span data-ttu-id="9537b-227">3. fél forgalom a like Runscope egy egyszerű, néhány dozen sornyi kód rendszerek figyelése csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="9537b-227">Connecting to 3rd party traffic monitoring systems like Runscope is a simple as a few dozen lines of code.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9537b-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9537b-228">Next steps</span></span>
* <span data-ttu-id="9537b-229">További tudnivalók az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="9537b-229">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="9537b-230">Ismerkedés az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="9537b-230">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="9537b-231">Üzenetek fogadása az EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="9537b-231">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="9537b-232">Event Hubs programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="9537b-232">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="9537b-233">További tudnivalók az API Management és az Event Hubs-integráció</span><span class="sxs-lookup"><span data-stu-id="9537b-233">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="9537b-234">Hogyan naplózza az eseményeket az Azure Event Hubs az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="9537b-234">How to log events to Azure Event Hubs in Azure API Management</span></span>](api-management-howto-log-event-hubs.md)
  * [<span data-ttu-id="9537b-235">Naplózó entitáshivatkozás</span><span class="sxs-lookup"><span data-stu-id="9537b-235">Logger entity reference</span></span>](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [<span data-ttu-id="9537b-236">napló-eventhub szabályzatainak ismertetése</span><span class="sxs-lookup"><span data-stu-id="9537b-236">log-to-eventhub policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
