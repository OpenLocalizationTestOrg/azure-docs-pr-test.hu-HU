---
title: "az Azure API Management, az Event Hubs és Runscope API-k aaaMonitor |} Microsoft Docs"
description: "Csatlakozás Azure API Management, az Azure Event Hubs és a HTTP-naplózás és figyelés Runscope hello napló eventhub házirendet, amely tartalmazza a mintaalkalmazás"
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
ms.openlocfilehash: 7456a2436f3a2d7b815b70b65fca9481d39c5fe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a><span data-ttu-id="d6ffd-103">Az API-kat az Azure API Management, az Event Hubs és Runscope figyelése</span><span class="sxs-lookup"><span data-stu-id="d6ffd-103">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>
<span data-ttu-id="d6ffd-104">Hello [API-kezelés szolgáltatás](api-management-key-concepts.md) HTTP küldött kérelmek tooyour HTTP API számos képességek tooenhance hello feldolgozása biztosít.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-104">hello [API Management service](api-management-key-concepts.md) provides many capabilities tooenhance hello processing of HTTP requests sent tooyour HTTP API.</span></span> <span data-ttu-id="d6ffd-105">Azonban hello megléte hello kérések és válaszok átmeneti.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-105">However, hello existence of hello requests and responses are transient.</span></span> <span data-ttu-id="d6ffd-106">hello kérést, és azt áthaladó hello API-kezelés szolgáltatás tooyour háttér-API.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-106">hello request is made and it flows through hello API Management service tooyour backend API.</span></span> <span data-ttu-id="d6ffd-107">Az API-hello kérést dolgoz fel, és választ vissza áthaladó toohello API fogyasztói.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-107">Your API processes hello request and a response flows back through toohello API consumer.</span></span> <span data-ttu-id="d6ffd-108">hello API-kezelés szolgáltatás tartja néhány fontos statisztikai adat kapcsolatos hello API-k megjelenítendő hello Publisher portál Irányítópultjára, de túl, hello részletek eltűnnek.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-108">hello API Management service keeps some important statistics about hello APIs for display in hello Publisher portal dashboard, but beyond that, hello details are gone.</span></span>

<span data-ttu-id="d6ffd-109">Hello segítségével [napló eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [házirend](api-management-howto-policies.md) hello API-kezelés szolgáltatás küldhet minden adatát a hello kérelem-válasz tooan [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="d6ffd-109">By using hello [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [policy](api-management-howto-policies.md) in hello API Management service you can send any details from hello request and response tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span> <span data-ttu-id="d6ffd-110">Nincsenek számos oka lehet, hogy miért érdemes a HTTP-üzenetek küldése tooyour API-k toogenerate események.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-110">There are a variety of reasons why you may want toogenerate events from HTTP messages being sent tooyour APIs.</span></span> <span data-ttu-id="d6ffd-111">Például frissítések, a használatelemzés, a kivétel riasztások és a 3. fél integrációja napló.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-111">Some examples include audit trail of updates, usage analytics, exception alerting and 3rd party integrations.</span></span>   

<span data-ttu-id="d6ffd-112">Ez a cikk bemutatja, hogyan toocapture hello teljes http-kérelem-válasz üzenetet elküldi a tooan Eseményközpont, és majd továbbítják az adott üzenet tooa harmadik féltől származó szolgáltatása, amely HTTP-naplózás és -szolgáltatások figyelése.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-112">This article demonstrates how toocapture hello entire HTTP request and response message, send it tooan Event Hub and then relay that message tooa third party service that provides HTTP logging and monitoring services.</span></span>

## <a name="why-send-from-api-management-service"></a><span data-ttu-id="d6ffd-113">Miért küldése az API Management szolgáltatást?</span><span class="sxs-lookup"><span data-stu-id="d6ffd-113">Why Send From API Management Service?</span></span>
<span data-ttu-id="d6ffd-114">Már lehetséges toowrite HTTP köztes HTTP API keretrendszerek toocapture HTTP-kérések és válaszok és a naplózás és figyelés rendszerek hírcsatorna őket.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-114">It is possible toowrite HTTP middleware that can plug into HTTP API frameworks toocapture HTTP requests and responses and feed them into logging and monitoring systems.</span></span> <span data-ttu-id="d6ffd-115">hello hátránya toothis megoldás, a hello HTTP köztes toobe hello háttér-API integrálni kell és meg kell egyeznie a hello API hello platformja.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-115">hello downside toothis approach is hello HTTP middleware needs toobe integrated into hello backend API and must match hello platform of hello API.</span></span> <span data-ttu-id="d6ffd-116">Ha több API-k minden egyes hello köztes kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-116">If there are multiple APIs then each one must deploy hello middleware.</span></span> <span data-ttu-id="d6ffd-117">Gyakran oka miért háttér API-k nem lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-117">Often there are reasons why backend APIs cannot be updated.</span></span>

<span data-ttu-id="d6ffd-118">Hello Azure API Management szolgáltatás toointegrate használata naplózási infrastruktúra központi és platformfüggetlen megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-118">Using hello Azure API Management service toointegrate with logging infrastructure provides a centralized and platform-independent solution.</span></span> <span data-ttu-id="d6ffd-119">Célszerű is méretezhető, részben miatt toohello [georeplikáció](api-management-howto-deploy-multi-region.md) Azure API-felügyeleti képességeit.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-119">It is also scalable, in part due toohello [geo-replication](api-management-howto-deploy-multi-region.md) capabilities of Azure API Management.</span></span>

## <a name="why-send-tooan-azure-event-hub"></a><span data-ttu-id="d6ffd-120">Miért küldése tooan Azure Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="d6ffd-120">Why send tooan Azure Event Hub?</span></span>
<span data-ttu-id="d6ffd-121">Ez egy ésszerű tooask, miért a szabályzatban meghatározott tooAzure Event Hubs van?</span><span class="sxs-lookup"><span data-stu-id="d6ffd-121">It is a reasonable tooask, why create a policy that is specific tooAzure Event Hubs?</span></span> <span data-ttu-id="d6ffd-122">Előfordulhat, hogy kívánt toolog saját kérések különböző helyeken van.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-122">There are many different places where I might want toolog my requests.</span></span> <span data-ttu-id="d6ffd-123">Miért nem küldés hello kérelmek közvetlenül toohello végső rendeltetési?</span><span class="sxs-lookup"><span data-stu-id="d6ffd-123">Why not just send hello requests directly toohello final destination?</span></span>  <span data-ttu-id="d6ffd-124">Ez a beállítás.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-124">That is an option.</span></span> <span data-ttu-id="d6ffd-125">Azonban az API management szolgáltatás naplózási kérelmeinek meghozásakor szükség tooconsider hogyan befolyásolják naplózási üzeneteket az hello API hello teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-125">However, when making logging requests from an API management service, it is necessary tooconsider how logging messages will impact hello performance of hello API.</span></span> <span data-ttu-id="d6ffd-126">Fokozatos nő a terhelés rendszerösszetevő elérhető példányok növelésével vagy georeplikáció kihasználásával lehet kezelni.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-126">Gradual increases in load can be handled by increasing available instances of system components or by taking advantage of geo-replication.</span></span> <span data-ttu-id="d6ffd-127">Azonban a forgalom rövid igényeiben jelentkező okozhat, ha kérelmek toologging infrastruktúra indítsa el a terhelés tooslow jelentősen késleltetett kérelmek toobe.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-127">However, short spikes in traffic can cause requests toobe significantly delayed if requests toologging infrastructure start tooslow under load.</span></span>

<span data-ttu-id="d6ffd-128">hello Azure Event Hubs tervezett tooingress hatalmas mennyiségű adatot, kapacitással kapcsolatos események sokkal nagyobb számú hello több HTTP-kérelmek legtöbb API-k folyamat.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-128">hello Azure Event Hubs is designed tooingress huge volumes of data, with capacity for dealing with a far higher number of events than hello number of HTTP requests most APIs process.</span></span> <span data-ttu-id="d6ffd-129">az Event Hubs hello kifinomult között az API management szolgáltatás és hello infrastruktúra tárolásához és feldolgozásához köszönőüzenetei egy közvetítő funkcionál.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-129">hello Event Hub acts as a kind of sophisticated buffer between your API management service and hello infrastructure that will store and process hello messages.</span></span> <span data-ttu-id="d6ffd-130">Ez biztosítja, hogy az API-teljesítmény nem romlani fog toohello naplózási infrastruktúra miatt.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-130">This ensures that your API performance will not suffer due toohello logging infrastructure.</span></span>  

<span data-ttu-id="d6ffd-131">Miután hello adatok tooan tárolva, és megvárja, hogy az Event Hubs fogyasztók tooprocess az Eseményközpont adtak át.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-131">Once hello data has been passed tooan Event Hub it is persisted and will wait for Event Hub consumers tooprocess it.</span></span> <span data-ttu-id="d6ffd-132">az Event Hubs hello nem ügyeljen arra, hogyan fogja feldolgozni, azt csak ügyel meggyőződött arról, hogy üdvözlőüzenetére sikeresen kézbesíti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-132">hello Event Hub does not care how it will be processed, it just cares about making sure hello message will be successfully delivered.</span></span>     

<span data-ttu-id="d6ffd-133">Az Event Hubs hello képességét toostream események toomultiple fogyasztói csoportok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-133">Event Hubs have hello ability toostream events toomultiple consumer groups.</span></span> <span data-ttu-id="d6ffd-134">Ez lehetővé teszi, hogy teljesen más rendszerek által feldolgozott események toobe.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-134">This allows events toobe processed by completely different systems.</span></span> <span data-ttu-id="d6ffd-135">Ez lehetővé teszi több integrációs forgatókönyv támogatása nem hozzáadása késések hello hello API kérelem feldolgozása hello API Management szolgáltatáson belül csak egy eseménynapló igények toobe jön létre.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-135">This enables supporting many integration scenarios without putting addition delays on hello processing of hello API request within hello API Management service as only one event needs toobe generated.</span></span>

## <a name="a-policy-toosend-applicationhttp-messages"></a><span data-ttu-id="d6ffd-136">Egy házirend toosend alkalmazás/HTTP-üzenetek</span><span class="sxs-lookup"><span data-stu-id="d6ffd-136">A policy toosend application/http messages</span></span>
<span data-ttu-id="d6ffd-137">Az Eseményközpontok eseményadatok egyszerű karakterláncként fogad el.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-137">An Event Hub accepts event data as a simple string.</span></span> <span data-ttu-id="d6ffd-138">hello karakterláncokat tartalma teljesen tooyou fel.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-138">hello contents of that string are completely up tooyou.</span></span> <span data-ttu-id="d6ffd-139">toobe képes toopackage HTTP-kérelem össze, és küldje el tooEvent tooformat hello karakterlánc hello kérés vagy válasz információkkal kell hubok ki.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-139">toobe able toopackage up an HTTP request and send it off tooEvent Hubs we need tooformat hello string with hello request or response information.</span></span> <span data-ttu-id="d6ffd-140">Ilyen helyzetekben, ha egy meglévő formátum újrafelhasználásához, majd előfordulhat, hogy nincs toowrite saját elemzése a kódot.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-140">In situations like this, if there is an existing format we can reuse, then we may not have toowrite our own parsing code.</span></span> <span data-ttu-id="d6ffd-141">Kezdetben szeretnék venni hello segítségével [HAR](http://www.softwareishard.com/blog/har-12-spec/) HTTP-kérések és válaszok küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-141">Initially I considered using hello [HAR](http://www.softwareishard.com/blog/har-12-spec/) for sending HTTP requests and responses.</span></span> <span data-ttu-id="d6ffd-142">Azonban ez a formátum tárolására van optimalizálva HTTP-kérelmek sorozatát alapú JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-142">However, this format is optimized for storing a sequence of HTTP requests in a JSON based format.</span></span> <span data-ttu-id="d6ffd-143">Tartalmaz egy hello a forgatókönyvhöz az HTTP üdvözlőüzenetére áthaladó hello hálózaton keresztül szükségtelen összetettsége hozzáadott kötelező elemek száma.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-143">It contained a number of mandatory elements that added unnecessary complexity for hello scenario of passing hello HTTP message over hello wire.</span></span>  

<span data-ttu-id="d6ffd-144">Alternatív lehetőségként lett toouse hello `application/http` médiatípus hello HTTP-specifikáció leírtak [RFC 7230](http://tools.ietf.org/html/rfc7230).</span><span class="sxs-lookup"><span data-stu-id="d6ffd-144">An alternative option was toouse hello `application/http` media type as described in hello HTTP specification [RFC 7230](http://tools.ietf.org/html/rfc7230).</span></span> <span data-ttu-id="d6ffd-145">A médiatípus pontos formátuma azonos, amely használt tooactually küldési HTTP-üzenetek hello hálózaton keresztül, de a teljes üdvözlőüzenetére is rendezni egy másik HTTP-kérelem törzse hello hello használja.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-145">This media type uses hello exact same format that is used tooactually send HTTP messages over hello wire, but hello entire message can be put in hello body of another HTTP request.</span></span> <span data-ttu-id="d6ffd-146">A mi esetünkben csak fogjuk toouse hello szervezet, az üzenet toosend tooEvent hubok.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-146">In our case we are just going toouse hello body as our message toosend tooEvent Hubs.</span></span> <span data-ttu-id="d6ffd-147">Már szerepel egy elemző van kényelmesen, [Microsoft ASP.NET Web API 2.2 ügyfél](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) tárak, amelyek elemezni ezt a formátumot, majd átalakíthatja hello natív `HttpRequestMessage` és `HttpResponseMessage` objektumok.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-147">Conveniently, there is a parser that exists in [Microsoft ASP.NET Web API 2.2 Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) libraries that can parse this format and convert it into hello native `HttpRequestMessage` and `HttpResponseMessage` objects.</span></span>

<span data-ttu-id="d6ffd-148">toobe képes toocreate Ez az üzenet igazolnia kell a C#-alapú tootake kihasználásához [házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx) az Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-148">toobe able toocreate this message we need tootake advantage of C# based [Policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management.</span></span> <span data-ttu-id="d6ffd-149">Íme egy HTTP kérelem üzenet tooAzure Event Hubs, amely hello házirend.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-149">Here is hello policy which sends a HTTP request message tooAzure Event Hubs.</span></span>

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

### <a name="policy-declaration"></a><span data-ttu-id="d6ffd-150">Házirend nyilatkozat</span><span class="sxs-lookup"><span data-stu-id="d6ffd-150">Policy declaration</span></span>
<span data-ttu-id="d6ffd-151">Nincs adott szempontokat érdemes megemlíteni kapcsolatban a házirend-kifejezést.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-151">There a few particular things worth mentioning about this policy expression.</span></span> <span data-ttu-id="d6ffd-152">hello napló eventhub házirendben engedélyezve van a naplózó-azonosítója, amely hivatkozik hello API-kezelés szolgáltatás belül lett létrehozva naplózó toohello neve nevű attribútum.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-152">hello log-to-eventhub policy has an attribute called logger-id which refers toohello name of logger that has been created within hello API Management service.</span></span> <span data-ttu-id="d6ffd-153">Hogyan toosetup egy Eseményközpontba naplózó hello API Management szolgáltatásban található hello dokumentum részleteit hello [hogyan toolog események tooAzure Event Hubs az Azure API Management](api-management-howto-log-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="d6ffd-153">hello details of how toosetup an Event Hub logger in hello API Management service can be found in hello document [How toolog events tooAzure Event Hubs in Azure API Management](api-management-howto-log-event-hubs.md).</span></span> <span data-ttu-id="d6ffd-154">hello második attribútum nem kötelező paraméter, amely arra utasítja az Event Hubs mely partíció toostore hello üzenetébe.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-154">hello second attribute is an optional parameter that instructs Event Hubs which partition toostore hello message in.</span></span> <span data-ttu-id="d6ffd-155">Az Event Hubs partíciók tooenable scalabilty használja, és legalább két igényel.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-155">Event Hubs use partitions tooenable scalabilty and require a minimum of two.</span></span> <span data-ttu-id="d6ffd-156">az üzenetek kézbesítési rendezett hello csak garantáltan a partíción belül.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-156">hello ordered delivery of messages is only guaranteed within a partition.</span></span> <span data-ttu-id="d6ffd-157">A Microsoft arra utasítani az Event Hubs mely partíció tooplace hello üzenetben, ha egy ciklikus multiplexelés algoritmus toodistribute hello terheléselosztási fogja használni.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-157">If we do not instruct Event Hub in which partition tooplace hello message, it will use a round-robin algorithm toodistribute hello load.</span></span> <span data-ttu-id="d6ffd-158">Azonban okozhatnak a sorrendje nem feldolgozott üzenetek toobe némelyike.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-158">However, that may cause some of our messages toobe processed out of order.</span></span>  

### <a name="partitions"></a><span data-ttu-id="d6ffd-159">Partíciók</span><span class="sxs-lookup"><span data-stu-id="d6ffd-159">Partitions</span></span>
<span data-ttu-id="d6ffd-160">az üzenetek sorrendben tooconsumers érkeznek, és kihasználhatják hello terhelés terjesztési képességet a partíciók tooensure, elfogadása toosend HTTP kérelem üzenetek tooone partíció és HTTP válasz üzenetek tooa második partíció.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-160">tooensure our messages are delivered tooconsumers in order and take advantage of hello load distribution capability of partitions, I chose toosend HTTP request messages tooone partition and HTTP response messages tooa second partition.</span></span> <span data-ttu-id="d6ffd-161">Ezzel biztosíthatja, hogy még akkor is, egy terheléselosztási és garantáljuk, hogy minden kérésnél fognak használni ahhoz, és minden válasz fognak használni ahhoz.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-161">This will ensure an even load distribution and we can guarantee that all requests will be consumed in order and all responses will be consumed in order.</span></span> <span data-ttu-id="d6ffd-162">Lehetséges, hogy a törlést megelőzően hello vonatkozó kérés válasz toobe, de, amely nincs probléma van, egy másik mechanizmus használatával történik a kért tooresponses, és tudjuk, hogy kérelmek mindig előznie válaszok.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-162">It is possible for a response toobe consumed before hello corresponding request, but as that is not a problem as we have a different mechanism for correlating requests tooresponses and we know that requests always come before responses.</span></span>

### <a name="http-payloads"></a><span data-ttu-id="d6ffd-163">HTTP hasznos adat található</span><span class="sxs-lookup"><span data-stu-id="d6ffd-163">HTTP payloads</span></span>
<span data-ttu-id="d6ffd-164">Hello létrehozása után `requestLine` toosee ellenőrizzük, ha csonkolva lesz a hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-164">After building hello `requestLine` we check toosee if hello request body should be truncated.</span></span> <span data-ttu-id="d6ffd-165">hello kérelemtörzset csonkolt tooonly 1024.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-165">hello request body is truncated tooonly 1024.</span></span> <span data-ttu-id="d6ffd-166">Ez növekedhet, azonban az egyes Eseményközpontban üzenetek-e korlátozott too256KB, akkor valószínű, hogy néhány HTTP-üzenet szervek elférjen egyetlen üzenetben nem lesz.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-166">This could be increased, however individual Event Hub messages are limited too256KB, so it is likely that some HTTP message bodies will not fit in a single message.</span></span> <span data-ttu-id="d6ffd-167">Naplózás és az elemzés során jelentős mennyiségű információt származtatható csak hello HTTP kérelem-sor és fejlécekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-167">When doing logging and analytics a significant amount of information can be derived from just hello HTTP request line and headers.</span></span> <span data-ttu-id="d6ffd-168">Is sok API-kérelmek csak kis szervek adja vissza, és így nagy szervezetek betartására értékeinek hello megszűnését viszonylag minimális összehasonlító toohello csökkenést átviteli, feldolgozása és a tárolási költségek tookeep minden szervezet tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-168">Also, many API requests only return small bodies and so hello loss of information value by truncating large bodies is fairly minimal in comparison toohello reduction in transfer, processing and storage costs tookeep all body contents.</span></span> <span data-ttu-id="d6ffd-169">Egy végső Megjegyzés hello törzsének feldolgozására a rendszer, hogy toopass kell `true` , toohello<string>() metódus azt olvas hello üzenettörzs tartalmát, mert de egyben kívánt hello háttér-API toobe képes tooread hello törzsében.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-169">One final note about processing hello body is that we need toopass `true` toohello As<string>() method because we are reading hello body contents, but was also want hello backend API toobe able tooread hello body.</span></span> <span data-ttu-id="d6ffd-170">Úgy, hogy igaz toothis metódus azt, hogy azt még egyszer olvasható pufferelt hello törzs toobe miatt.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-170">By passing true toothis method we cause hello body toobe buffered so that it can be read a second time.</span></span> <span data-ttu-id="d6ffd-171">Ez egy fontos toobe tudomást Ha nagyon nagy méretű fájlok feltöltése vagy hosszú lekérdezési használ az API-k.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-171">This is important toobe aware of if you have an API that does uploading of very large files or uses long polling.</span></span> <span data-ttu-id="d6ffd-172">Ezekben az esetekben lenne a legjobb tooavoid hello törzs minden olvasása.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-172">In these cases it would be best tooavoid reading hello body at all.</span></span>   

### <a name="http-headers"></a><span data-ttu-id="d6ffd-173">HTTP-fejlécek</span><span class="sxs-lookup"><span data-stu-id="d6ffd-173">HTTP headers</span></span>
<span data-ttu-id="d6ffd-174">HTTP-fejléceket is egyszerűen továbbítja egyszerű kulcs/érték pár formátumban hello üzenet formátumba.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-174">HTTP Headers can be simply transferred over into hello message format in a simple key/value pair format.</span></span> <span data-ttu-id="d6ffd-175">Azt választotta ki bizonyos biztonsági toostrip időérzékeny mezőinek, tooavoid feleslegesen megakadályozására hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-175">We have chosen toostrip out certain security sensitive fields, tooavoid unnecessarily leaking credential information.</span></span> <span data-ttu-id="d6ffd-176">Nem valószínű, hogy API-kulcsokat és más hitelesítő adatokat használni elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-176">It is unlikely that API keys and other credentials would be used for analytics purposes.</span></span> <span data-ttu-id="d6ffd-177">Ha toodo elemzés jelenítsük hello felhasználói és hello adott termék használják, akkor azt lehetett lekérni, amely hello `context` objektumra, és adja hozzá a toohello üzenetet.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-177">If we wish toodo analysis on hello user and hello particular product they are using then we could get that from hello `context` object and add that toohello message.</span></span>     

### <a name="message-metadata"></a><span data-ttu-id="d6ffd-178">Üzenet metaadatok</span><span class="sxs-lookup"><span data-stu-id="d6ffd-178">Message Metadata</span></span>
<span data-ttu-id="d6ffd-179">Hello teljes üzenet toosend toohello eseményközpont létrehozásakor hello első sor része nem ténylegesen hello `application/http` üzenet.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-179">When building hello complete message toosend toohello event hub, hello first line is not actually part of hello `application/http` message.</span></span> <span data-ttu-id="d6ffd-180">hello első sor üdvözlőüzenetére-e a kérés vagy válasz üzenet álló további metaadatokat, és egy üzenet azonosítója, amely használt toocorrelate tooresponses kéri.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-180">hello first line is additional metadata consisting of whether hello message is a request or response message and a message id which is used toocorrelate requests tooresponses.</span></span> <span data-ttu-id="d6ffd-181">hello üzenet azonosítója, amely a következőképpen néz ki egy másik csoportházirend segítségével hozhatók létre:</span><span class="sxs-lookup"><span data-stu-id="d6ffd-181">hello message id is created by using another policy that looks like this:</span></span>

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

<span data-ttu-id="d6ffd-182">Sikerült létrehoztunk hello kérelemüzenet tárolt, hogy egy változóban, amíg hello válasz lett visszaadott és majd egyszerűen elküldve hello kérelem-válasz, egyetlen üzenetben.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-182">We could have created hello request message, stored that in a variable until hello response was returned and then simply sent hello request and response as a single message.</span></span> <span data-ttu-id="d6ffd-183">Azonban hello kérelem és válasz egymástól függetlenül küld, és használja egy üzenet azonosítója toocorrelate hello két, azt lekérése egy kicsit nagyobb rugalmasságot nyújt az üdvözlő üzenet mérete, hello képességét tootake előnyeit több partíciót üzenet sorrendjét és hello mellett kérelem fog megjelenni a naplózás irányítópult hamarabb.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-183">However, by sending hello request and response independently and using a message id toocorrelate hello two, we get a bit more flexibility in hello message size, hello ability tootake advantage of multiple partitions whilst maintaining message order and hello request will appear in our logging dashboard sooner.</span></span> <span data-ttu-id="d6ffd-184">Előfordulhat, hogy is van bizonyos esetekben, ahol egy érvényes válasz nem érkezik toohello eseményközpont, valószínűleg hello API Management szolgáltatásban tooa kérelem végzetes hiba miatt, de azt továbbra is rendelkeznek egy olyan rekordot hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-184">There also may be some scenarios where a valid response is never sent toohello event hub, possibly due tooa fatal request error in hello API Management service, but we still will have a record of hello request.</span></span>

<span data-ttu-id="d6ffd-185">hello házirend toosend hello HTTP válaszüzenetet nagyon hasonló toohello kérés keres, és így hello végezhető házirend konfigurációs néz ki:</span><span class="sxs-lookup"><span data-stu-id="d6ffd-185">hello policy toosend hello response HTTP message looks very similar toohello request and so hello complete policy configuration looks like this:</span></span>

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

<span data-ttu-id="d6ffd-186">Hello `set-variable` házirendet hoz létre egy érték, amely elérhető a mindkét hello `log-to-eventhub` hello-szabályzat `<inbound>` szakasz és hello `<outbound>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-186">hello `set-variable` policy creates a value that is accessible by both hello `log-to-eventhub` policy in hello `<inbound>` section and hello `<outbound>` section.</span></span>  

## <a name="receiving-events-from-event-hubs"></a><span data-ttu-id="d6ffd-187">Az események fogadását az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d6ffd-187">Receiving events from Event Hubs</span></span>
<span data-ttu-id="d6ffd-188">Azure Event Hubs eseményeit fogadásának hello segítségével [AMQP protokoll](http://www.amqp.org/).</span><span class="sxs-lookup"><span data-stu-id="d6ffd-188">Events from Azure Event Hub are received using hello [AMQP protocol](http://www.amqp.org/).</span></span> <span data-ttu-id="d6ffd-189">hello Microsoft Service Bus csapatával végzett ügyfél szalagtárak elérhető toomake hello események könnyebben fel.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-189">hello Microsoft Service Bus team have made client libraries available toomake hello consuming events easier.</span></span> <span data-ttu-id="d6ffd-190">Támogatott két különböző megközelítés, egy folyamatban van egy *közvetlen fogyasztói* , és más hello hello `EventProcessorHost` osztály.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-190">There are two different approaches supported, one is being a *Direct Consumer* and hello other is using hello `EventProcessorHost` class.</span></span> <span data-ttu-id="d6ffd-191">E két megközelítés példái megtalálhatók hello [Event Hubs programozási útmutató](../event-hubs/event-hubs-programming-guide.md).</span><span class="sxs-lookup"><span data-stu-id="d6ffd-191">Examples of these two approaches can be found in hello [Event Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).</span></span> <span data-ttu-id="d6ffd-192">hello rövid hello különbségek verziószáma, `Direct Consumer` lehetőséget teljes és hello `EventProcessorHost` nem egy része hello bekötése, az azonban lehetővé teszi bizonyos feltételezéseket hogyan feldolgozza ezeket az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-192">hello short version of hello differences is, `Direct Consumer` gives you complete control and hello `EventProcessorHost` does some of hello plumbing work for you but makes certain assumptions about how you will process those events.</span></span>  

### <a name="eventprocessorhost"></a><span data-ttu-id="d6ffd-193">EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="d6ffd-193">EventProcessorHost</span></span>
<span data-ttu-id="d6ffd-194">Ez a példa használjuk hello `EventProcessorHost` az egyszerűség kedvéért azonban akkor lehetséges, hogy nem hello ebben a forgatókönyvben a legjobb választás.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-194">In this sample, we will use hello `EventProcessorHost` for simplicity, however it may not hello best choice for this particular scenario.</span></span> <span data-ttu-id="d6ffd-195">`EventProcessorHost`hello meggyőződött arról, hogy nincs kapcsolatos problémák egy adott esemény processzor osztályon belül szálkezelési tooworry rögzített munkáját.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-195">`EventProcessorHost` does hello hard work of making sure you don't have tooworry about threading issues within a particular event processor class.</span></span> <span data-ttu-id="d6ffd-196">Azonban a mi esetünkben azt egyszerűen hello tooanother üzenetformátum konvertálása és átadja azt a mentén tooanother szolgáltatást egy aszinkron metódus használatával.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-196">However, in our scenario, we are simply converting hello message tooanother format and passing it along tooanother service using an async method.</span></span> <span data-ttu-id="d6ffd-197">Nincs szükség megosztott állapotot, és ezért kockázata problémák szálkezelési frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-197">There is no need for updating shared state and therefore no risk of threading issues.</span></span> <span data-ttu-id="d6ffd-198">A legtöbb esetben `EventProcessorHost` valószínűleg hello legjobb választás, és biztosan hello egyszerűbb lehetőség.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-198">For most scenarios, `EventProcessorHost` is probably hello best choice and it is certainly hello easier option.</span></span>     

### <a name="ieventprocessor"></a><span data-ttu-id="d6ffd-199">IEventProcessor</span><span class="sxs-lookup"><span data-stu-id="d6ffd-199">IEventProcessor</span></span>
<span data-ttu-id="d6ffd-200">hello központi koncepció használatakor `EventProcessorHost` toocreate van egy hello megvalósítása `IEventProcessor` illesztőfelületet, amely tartalmazza a hello metódus `ProcessEventAsync`.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-200">hello central concept when using `EventProcessorHost` is toocreate a an implementation of hello `IEventProcessor` interface which contains hello method `ProcessEventAsync`.</span></span> <span data-ttu-id="d6ffd-201">Ez a módszer hello lényege itt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d6ffd-201">hello essence of that method is shown here:</span></span>

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

<span data-ttu-id="d6ffd-202">Az hello metódusnak átadott EventData objektumok listáját, és azt, hogy a lista ismétlés.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-202">A list of EventData objects are passed into hello method and we iterate over that list.</span></span> <span data-ttu-id="d6ffd-203">az egyes módszerek hello bájt elemzésének HttpMessage objektumba és, hogy az objektum átadása IHttpMessageProcessor tooan példányát.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-203">hello bytes of each method are parsed into a HttpMessage object and that object is passed tooan instance of IHttpMessageProcessor.</span></span>

### <a name="httpmessage"></a><span data-ttu-id="d6ffd-204">HttpMessage</span><span class="sxs-lookup"><span data-stu-id="d6ffd-204">HttpMessage</span></span>
<span data-ttu-id="d6ffd-205">Hello `HttpMessage` példány három adatra adatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="d6ffd-205">hello `HttpMessage` instance contains three pieces of data:</span></span>

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

<span data-ttu-id="d6ffd-206">Hello `HttpMessage` példány tartalmaz egy `MessageId` GUID, amely lehetővé teszi tooconnect hello HTTP-kérelem toohello megfelelő HTTP-válasz és egy logikai érték, amely azt jelzi, hogy hello objektum tartalmazza a HttpRequestMessage példányának és HttpResponseMessage.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-206">hello `HttpMessage` instance contains a `MessageId` GUID that allows us tooconnect hello HTTP request toohello corresponding HTTP response and a boolean value that identifies if hello object contains an instance of a HttpRequestMessage and HttpResponseMessage.</span></span> <span data-ttu-id="d6ffd-207">A HTTP-osztályok a beépített hello segítségével `System.Net.Http`, képes tootake előnyeit hello voltam `application/http` elemzése a kódot, amely része a `System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-207">By using hello built in HTTP classes from `System.Net.Http`, I was able tootake advantage of hello `application/http` parsing code that is included in `System.Net.Http.Formatting`.</span></span>  

### <a name="ihttpmessageprocessor"></a><span data-ttu-id="d6ffd-208">IHttpMessageProcessor</span><span class="sxs-lookup"><span data-stu-id="d6ffd-208">IHttpMessageProcessor</span></span>
<span data-ttu-id="d6ffd-209">Hello `HttpMessage` példány továbbítja a tooimplementation `IHttpMessageProcessor` Ez az létrehozott toodecouple hello fogadásával és hello esemény értelmezése Azure Event Hubs és hello tényleges feldolgozása: az illesztőfelület.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-209">hello `HttpMessage` instance is then forwarded tooimplementation of `IHttpMessageProcessor` which is an interface I created toodecouple hello receiving and interpretation of hello event from Azure Event Hub and hello actual processing of it.</span></span>

## <a name="forwarding-hello-http-message"></a><span data-ttu-id="d6ffd-210">Továbbító hello HTTP-üzenet</span><span class="sxs-lookup"><span data-stu-id="d6ffd-210">Forwarding hello HTTP message</span></span>
<span data-ttu-id="d6ffd-211">Ez a minta az I lezárását lenne érdekes toopush hello HTTP-kérelem túl keresztül[Runscope](http://www.runscope.com).</span><span class="sxs-lookup"><span data-stu-id="d6ffd-211">For this sample, I decided it would be interesting toopush hello HTTP Request over too[Runscope](http://www.runscope.com).</span></span> <span data-ttu-id="d6ffd-212">Runscope egy felhőalapú szolgáltatás, amely HTTP-hibakeresés, a naplózás és figyelés.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-212">Runscope is a cloud based service that specializes in HTTP debugging, logging and monitoring.</span></span> <span data-ttu-id="d6ffd-213">Ingyenes szint, így könnyen tootry, és lehetővé teszi velünk toosee hello HTTP-kérések valós idejű haladnak keresztül az API-kezelés szolgáltatás a rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-213">They have a free tier, so it is easy tootry and it allows us toosee hello HTTP requests in real-time flowing through our API Management service.</span></span>

<span data-ttu-id="d6ffd-214">Hello `IHttpMessageProcessor` megvalósítási néz ki,</span><span class="sxs-lookup"><span data-stu-id="d6ffd-214">hello `IHttpMessageProcessor` implementation looks like this,</span></span>

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
       _Logger.LogDebug("Request sent tooRunscope");
   }
}
```

<span data-ttu-id="d6ffd-215">Képes tootake előnyeit voltam egy [meglévő ügyféloldali kódtára a Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) így később könnyen toopush `HttpRequestMessage` és `HttpResponseMessage` példányok be a szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-215">I was able tootake advantage of an [existing client library for Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) that makes it easy toopush `HttpRequestMessage` and `HttpResponseMessage` instances up into their service.</span></span> <span data-ttu-id="d6ffd-216">Ahhoz, tooaccess hello Runscope API szüksége lesz a fiók és API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-216">In order tooaccess hello Runscope API you will need an account and an API Key.</span></span> <span data-ttu-id="d6ffd-217">API-kulcs beolvasása vonatkozó utasítások megtalálhatók hello [alkalmazások létrehozása tooAccess Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) képernyőfelvétel.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-217">Instructions for getting an API key can be found in hello [Creating Applications tooAccess Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) screencast.</span></span>

## <a name="complete-sample"></a><span data-ttu-id="d6ffd-218">Teljes mintát</span><span class="sxs-lookup"><span data-stu-id="d6ffd-218">Complete sample</span></span>
<span data-ttu-id="d6ffd-219">Hello [forráskód](https://github.com/darrelmiller/ApimEventProcessor) és tesztek hello mintát a githubon.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-219">hello [source code](https://github.com/darrelmiller/ApimEventProcessor) and tests for hello sample are on GitHub.</span></span> <span data-ttu-id="d6ffd-220">Szüksége lesz egy [API Management szolgáltatás](api-management-get-started.md), [a csatlakoztatott Eseményközpontot](api-management-howto-log-event-hubs.md), és egy [Tárfiók](../storage/common/storage-create-storage-account.md) toorun hello minta a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-220">You will need an [API Management Service](api-management-get-started.md), [a connected Event Hub](api-management-howto-log-event-hubs.md), and a [Storage Account](../storage/common/storage-create-storage-account.md) toorun hello sample for yourself.</span></span>   

<span data-ttu-id="d6ffd-221">hello minta értéke most egy egyszerű konzolalkalmazást, amely figyeli az Event Hubs származó események összefogásával alakítja őket egy `HttpRequestMessage` és `HttpResponseMessage` objektumokat, és ezután továbbítja őket a toohello Runscope API.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-221">hello sample is just a simple Console application that listens for events coming from Event Hub, converts them into a `HttpRequestMessage` and `HttpResponseMessage` objects and then forwards them on toohello Runscope API.</span></span>

<span data-ttu-id="d6ffd-222">A hello animált kép a következő megtekintheti az adott kérelem benyújtásától tooan API hello fejlesztői portálra visszatérve hello konzol alkalmazás ábrázoló hello üzenet érkezett, a feldolgozott és továbbított és majd hello kérelem és válasz jelenik meg a hello Runscope forgalom Inspector.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-222">In hello following animated image, you can see a request being made tooan API in hello Developer Portal, hello Console application showing hello message being received, processed and forwarded and then hello request and response showing up in hello Runscope Traffic inspector.</span></span>

![A kérelem tooRunscope továbbított bemutatója](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a><span data-ttu-id="d6ffd-224">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="d6ffd-224">Summary</span></span>
<span data-ttu-id="d6ffd-225">Az Azure API Management szolgáltatás biztosítja az ideális hely toocapture hello HTTP forgalom tooand utaznak az API-kat.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-225">Azure API Management service provides an ideal place toocapture hello HTTP traffic travelling tooand from your APIs.</span></span> <span data-ttu-id="d6ffd-226">Azure Event Hubs egy kiválóan méretezhető, alacsony költségű megoldás, hogy forgalom rögzítése és elágazó azt másodlagos feldolgozási rendszerek naplózási, figyelés, és más kifinomult analytics.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-226">Azure Event Hubs is a highly scalable, low cost solution for capturing that traffic and feeding it into secondary processing systems for logging, monitoring and other sophisticated analytics.</span></span> <span data-ttu-id="d6ffd-227">Csatlakozás a like Runscope egy egyszerű, néhány dozen sornyi kód rendszerek figyelése too3rd fél forgalmat.</span><span class="sxs-lookup"><span data-stu-id="d6ffd-227">Connecting too3rd party traffic monitoring systems like Runscope is a simple as a few dozen lines of code.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6ffd-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6ffd-228">Next steps</span></span>
* <span data-ttu-id="d6ffd-229">További tudnivalók az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d6ffd-229">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="d6ffd-230">Ismerkedés az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d6ffd-230">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="d6ffd-231">Üzenetek fogadása az EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="d6ffd-231">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="d6ffd-232">Event Hubs programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="d6ffd-232">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="d6ffd-233">További tudnivalók az API Management és az Event Hubs-integráció</span><span class="sxs-lookup"><span data-stu-id="d6ffd-233">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="d6ffd-234">Hogyan toolog események tooAzure Event Hubs az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d6ffd-234">How toolog events tooAzure Event Hubs in Azure API Management</span></span>](api-management-howto-log-event-hubs.md)
  * [<span data-ttu-id="d6ffd-235">Naplózó entitáshivatkozás</span><span class="sxs-lookup"><span data-stu-id="d6ffd-235">Logger entity reference</span></span>](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [<span data-ttu-id="d6ffd-236">napló-eventhub szabályzatainak ismertetése</span><span class="sxs-lookup"><span data-stu-id="d6ffd-236">log-to-eventhub policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
