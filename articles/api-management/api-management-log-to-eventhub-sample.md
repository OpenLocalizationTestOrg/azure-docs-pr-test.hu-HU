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
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Az API-kat az Azure API Management, az Event Hubs és Runscope figyelése
Hello [API-kezelés szolgáltatás](api-management-key-concepts.md) HTTP küldött kérelmek tooyour HTTP API számos képességek tooenhance hello feldolgozása biztosít. Azonban hello megléte hello kérések és válaszok átmeneti. hello kérést, és azt áthaladó hello API-kezelés szolgáltatás tooyour háttér-API. Az API-hello kérést dolgoz fel, és választ vissza áthaladó toohello API fogyasztói. hello API-kezelés szolgáltatás tartja néhány fontos statisztikai adat kapcsolatos hello API-k megjelenítendő hello Publisher portál Irányítópultjára, de túl, hello részletek eltűnnek.

Hello segítségével [napló eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [házirend](api-management-howto-policies.md) hello API-kezelés szolgáltatás küldhet minden adatát a hello kérelem-válasz tooan [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md). Nincsenek számos oka lehet, hogy miért érdemes a HTTP-üzenetek küldése tooyour API-k toogenerate események. Például frissítések, a használatelemzés, a kivétel riasztások és a 3. fél integrációja napló.   

Ez a cikk bemutatja, hogyan toocapture hello teljes http-kérelem-válasz üzenetet elküldi a tooan Eseményközpont, és majd továbbítják az adott üzenet tooa harmadik féltől származó szolgáltatása, amely HTTP-naplózás és -szolgáltatások figyelése.

## <a name="why-send-from-api-management-service"></a>Miért küldése az API Management szolgáltatást?
Már lehetséges toowrite HTTP köztes HTTP API keretrendszerek toocapture HTTP-kérések és válaszok és a naplózás és figyelés rendszerek hírcsatorna őket. hello hátránya toothis megoldás, a hello HTTP köztes toobe hello háttér-API integrálni kell és meg kell egyeznie a hello API hello platformja. Ha több API-k minden egyes hello köztes kell telepíteni. Gyakran oka miért háttér API-k nem lehet frissíteni.

Hello Azure API Management szolgáltatás toointegrate használata naplózási infrastruktúra központi és platformfüggetlen megoldást kínál. Célszerű is méretezhető, részben miatt toohello [georeplikáció](api-management-howto-deploy-multi-region.md) Azure API-felügyeleti képességeit.

## <a name="why-send-tooan-azure-event-hub"></a>Miért küldése tooan Azure Event Hubs?
Ez egy ésszerű tooask, miért a szabályzatban meghatározott tooAzure Event Hubs van? Előfordulhat, hogy kívánt toolog saját kérések különböző helyeken van. Miért nem küldés hello kérelmek közvetlenül toohello végső rendeltetési?  Ez a beállítás. Azonban az API management szolgáltatás naplózási kérelmeinek meghozásakor szükség tooconsider hogyan befolyásolják naplózási üzeneteket az hello API hello teljesítményét. Fokozatos nő a terhelés rendszerösszetevő elérhető példányok növelésével vagy georeplikáció kihasználásával lehet kezelni. Azonban a forgalom rövid igényeiben jelentkező okozhat, ha kérelmek toologging infrastruktúra indítsa el a terhelés tooslow jelentősen késleltetett kérelmek toobe.

hello Azure Event Hubs tervezett tooingress hatalmas mennyiségű adatot, kapacitással kapcsolatos események sokkal nagyobb számú hello több HTTP-kérelmek legtöbb API-k folyamat. az Event Hubs hello kifinomult között az API management szolgáltatás és hello infrastruktúra tárolásához és feldolgozásához köszönőüzenetei egy közvetítő funkcionál. Ez biztosítja, hogy az API-teljesítmény nem romlani fog toohello naplózási infrastruktúra miatt.  

Miután hello adatok tooan tárolva, és megvárja, hogy az Event Hubs fogyasztók tooprocess az Eseményközpont adtak át. az Event Hubs hello nem ügyeljen arra, hogyan fogja feldolgozni, azt csak ügyel meggyőződött arról, hogy üdvözlőüzenetére sikeresen kézbesíti a rendszer.     

Az Event Hubs hello képességét toostream események toomultiple fogyasztói csoportok rendelkezik. Ez lehetővé teszi, hogy teljesen más rendszerek által feldolgozott események toobe. Ez lehetővé teszi több integrációs forgatókönyv támogatása nem hozzáadása késések hello hello API kérelem feldolgozása hello API Management szolgáltatáson belül csak egy eseménynapló igények toobe jön létre.

## <a name="a-policy-toosend-applicationhttp-messages"></a>Egy házirend toosend alkalmazás/HTTP-üzenetek
Az Eseményközpontok eseményadatok egyszerű karakterláncként fogad el. hello karakterláncokat tartalma teljesen tooyou fel. toobe képes toopackage HTTP-kérelem össze, és küldje el tooEvent tooformat hello karakterlánc hello kérés vagy válasz információkkal kell hubok ki. Ilyen helyzetekben, ha egy meglévő formátum újrafelhasználásához, majd előfordulhat, hogy nincs toowrite saját elemzése a kódot. Kezdetben szeretnék venni hello segítségével [HAR](http://www.softwareishard.com/blog/har-12-spec/) HTTP-kérések és válaszok küldéséhez. Azonban ez a formátum tárolására van optimalizálva HTTP-kérelmek sorozatát alapú JSON formátumban. Tartalmaz egy hello a forgatókönyvhöz az HTTP üdvözlőüzenetére áthaladó hello hálózaton keresztül szükségtelen összetettsége hozzáadott kötelező elemek száma.  

Alternatív lehetőségként lett toouse hello `application/http` médiatípus hello HTTP-specifikáció leírtak [RFC 7230](http://tools.ietf.org/html/rfc7230). A médiatípus pontos formátuma azonos, amely használt tooactually küldési HTTP-üzenetek hello hálózaton keresztül, de a teljes üdvözlőüzenetére is rendezni egy másik HTTP-kérelem törzse hello hello használja. A mi esetünkben csak fogjuk toouse hello szervezet, az üzenet toosend tooEvent hubok. Már szerepel egy elemző van kényelmesen, [Microsoft ASP.NET Web API 2.2 ügyfél](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) tárak, amelyek elemezni ezt a formátumot, majd átalakíthatja hello natív `HttpRequestMessage` és `HttpResponseMessage` objektumok.

toobe képes toocreate Ez az üzenet igazolnia kell a C#-alapú tootake kihasználásához [házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx) az Azure API Management. Íme egy HTTP kérelem üzenet tooAzure Event Hubs, amely hello házirend.

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

### <a name="policy-declaration"></a>Házirend nyilatkozat
Nincs adott szempontokat érdemes megemlíteni kapcsolatban a házirend-kifejezést. hello napló eventhub házirendben engedélyezve van a naplózó-azonosítója, amely hivatkozik hello API-kezelés szolgáltatás belül lett létrehozva naplózó toohello neve nevű attribútum. Hogyan toosetup egy Eseményközpontba naplózó hello API Management szolgáltatásban található hello dokumentum részleteit hello [hogyan toolog események tooAzure Event Hubs az Azure API Management](api-management-howto-log-event-hubs.md). hello második attribútum nem kötelező paraméter, amely arra utasítja az Event Hubs mely partíció toostore hello üzenetébe. Az Event Hubs partíciók tooenable scalabilty használja, és legalább két igényel. az üzenetek kézbesítési rendezett hello csak garantáltan a partíción belül. A Microsoft arra utasítani az Event Hubs mely partíció tooplace hello üzenetben, ha egy ciklikus multiplexelés algoritmus toodistribute hello terheléselosztási fogja használni. Azonban okozhatnak a sorrendje nem feldolgozott üzenetek toobe némelyike.  

### <a name="partitions"></a>Partíciók
az üzenetek sorrendben tooconsumers érkeznek, és kihasználhatják hello terhelés terjesztési képességet a partíciók tooensure, elfogadása toosend HTTP kérelem üzenetek tooone partíció és HTTP válasz üzenetek tooa második partíció. Ezzel biztosíthatja, hogy még akkor is, egy terheléselosztási és garantáljuk, hogy minden kérésnél fognak használni ahhoz, és minden válasz fognak használni ahhoz. Lehetséges, hogy a törlést megelőzően hello vonatkozó kérés válasz toobe, de, amely nincs probléma van, egy másik mechanizmus használatával történik a kért tooresponses, és tudjuk, hogy kérelmek mindig előznie válaszok.

### <a name="http-payloads"></a>HTTP hasznos adat található
Hello létrehozása után `requestLine` toosee ellenőrizzük, ha csonkolva lesz a hello kérés törzsében. hello kérelemtörzset csonkolt tooonly 1024. Ez növekedhet, azonban az egyes Eseményközpontban üzenetek-e korlátozott too256KB, akkor valószínű, hogy néhány HTTP-üzenet szervek elférjen egyetlen üzenetben nem lesz. Naplózás és az elemzés során jelentős mennyiségű információt származtatható csak hello HTTP kérelem-sor és fejlécekkel együtt. Is sok API-kérelmek csak kis szervek adja vissza, és így nagy szervezetek betartására értékeinek hello megszűnését viszonylag minimális összehasonlító toohello csökkenést átviteli, feldolgozása és a tárolási költségek tookeep minden szervezet tartalmát. Egy végső Megjegyzés hello törzsének feldolgozására a rendszer, hogy toopass kell `true` , toohello<string>() metódus azt olvas hello üzenettörzs tartalmát, mert de egyben kívánt hello háttér-API toobe képes tooread hello törzsében. Úgy, hogy igaz toothis metódus azt, hogy azt még egyszer olvasható pufferelt hello törzs toobe miatt. Ez egy fontos toobe tudomást Ha nagyon nagy méretű fájlok feltöltése vagy hosszú lekérdezési használ az API-k. Ezekben az esetekben lenne a legjobb tooavoid hello törzs minden olvasása.   

### <a name="http-headers"></a>HTTP-fejlécek
HTTP-fejléceket is egyszerűen továbbítja egyszerű kulcs/érték pár formátumban hello üzenet formátumba. Azt választotta ki bizonyos biztonsági toostrip időérzékeny mezőinek, tooavoid feleslegesen megakadályozására hitelesítő adatokat. Nem valószínű, hogy API-kulcsokat és más hitelesítő adatokat használni elemzés céljából. Ha toodo elemzés jelenítsük hello felhasználói és hello adott termék használják, akkor azt lehetett lekérni, amely hello `context` objektumra, és adja hozzá a toohello üzenetet.     

### <a name="message-metadata"></a>Üzenet metaadatok
Hello teljes üzenet toosend toohello eseményközpont létrehozásakor hello első sor része nem ténylegesen hello `application/http` üzenet. hello első sor üdvözlőüzenetére-e a kérés vagy válasz üzenet álló további metaadatokat, és egy üzenet azonosítója, amely használt toocorrelate tooresponses kéri. hello üzenet azonosítója, amely a következőképpen néz ki egy másik csoportházirend segítségével hozhatók létre:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

Sikerült létrehoztunk hello kérelemüzenet tárolt, hogy egy változóban, amíg hello válasz lett visszaadott és majd egyszerűen elküldve hello kérelem-válasz, egyetlen üzenetben. Azonban hello kérelem és válasz egymástól függetlenül küld, és használja egy üzenet azonosítója toocorrelate hello két, azt lekérése egy kicsit nagyobb rugalmasságot nyújt az üdvözlő üzenet mérete, hello képességét tootake előnyeit több partíciót üzenet sorrendjét és hello mellett kérelem fog megjelenni a naplózás irányítópult hamarabb. Előfordulhat, hogy is van bizonyos esetekben, ahol egy érvényes válasz nem érkezik toohello eseményközpont, valószínűleg hello API Management szolgáltatásban tooa kérelem végzetes hiba miatt, de azt továbbra is rendelkeznek egy olyan rekordot hello kérelem.

hello házirend toosend hello HTTP válaszüzenetet nagyon hasonló toohello kérés keres, és így hello végezhető házirend konfigurációs néz ki:

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

Hello `set-variable` házirendet hoz létre egy érték, amely elérhető a mindkét hello `log-to-eventhub` hello-szabályzat `<inbound>` szakasz és hello `<outbound>` szakasz.  

## <a name="receiving-events-from-event-hubs"></a>Az események fogadását az Event Hubs
Azure Event Hubs eseményeit fogadásának hello segítségével [AMQP protokoll](http://www.amqp.org/). hello Microsoft Service Bus csapatával végzett ügyfél szalagtárak elérhető toomake hello események könnyebben fel. Támogatott két különböző megközelítés, egy folyamatban van egy *közvetlen fogyasztói* , és más hello hello `EventProcessorHost` osztály. E két megközelítés példái megtalálhatók hello [Event Hubs programozási útmutató](../event-hubs/event-hubs-programming-guide.md). hello rövid hello különbségek verziószáma, `Direct Consumer` lehetőséget teljes és hello `EventProcessorHost` nem egy része hello bekötése, az azonban lehetővé teszi bizonyos feltételezéseket hogyan feldolgozza ezeket az eseményeket.  

### <a name="eventprocessorhost"></a>EventProcessorHost
Ez a példa használjuk hello `EventProcessorHost` az egyszerűség kedvéért azonban akkor lehetséges, hogy nem hello ebben a forgatókönyvben a legjobb választás. `EventProcessorHost`hello meggyőződött arról, hogy nincs kapcsolatos problémák egy adott esemény processzor osztályon belül szálkezelési tooworry rögzített munkáját. Azonban a mi esetünkben azt egyszerűen hello tooanother üzenetformátum konvertálása és átadja azt a mentén tooanother szolgáltatást egy aszinkron metódus használatával. Nincs szükség megosztott állapotot, és ezért kockázata problémák szálkezelési frissítéséhez. A legtöbb esetben `EventProcessorHost` valószínűleg hello legjobb választás, és biztosan hello egyszerűbb lehetőség.     

### <a name="ieventprocessor"></a>IEventProcessor
hello központi koncepció használatakor `EventProcessorHost` toocreate van egy hello megvalósítása `IEventProcessor` illesztőfelületet, amely tartalmazza a hello metódus `ProcessEventAsync`. Ez a módszer hello lényege itt jelenik meg:

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

Az hello metódusnak átadott EventData objektumok listáját, és azt, hogy a lista ismétlés. az egyes módszerek hello bájt elemzésének HttpMessage objektumba és, hogy az objektum átadása IHttpMessageProcessor tooan példányát.

### <a name="httpmessage"></a>HttpMessage
Hello `HttpMessage` példány három adatra adatokat tartalmazza:

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

Hello `HttpMessage` példány tartalmaz egy `MessageId` GUID, amely lehetővé teszi tooconnect hello HTTP-kérelem toohello megfelelő HTTP-válasz és egy logikai érték, amely azt jelzi, hogy hello objektum tartalmazza a HttpRequestMessage példányának és HttpResponseMessage. A HTTP-osztályok a beépített hello segítségével `System.Net.Http`, képes tootake előnyeit hello voltam `application/http` elemzése a kódot, amely része a `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
Hello `HttpMessage` példány továbbítja a tooimplementation `IHttpMessageProcessor` Ez az létrehozott toodecouple hello fogadásával és hello esemény értelmezése Azure Event Hubs és hello tényleges feldolgozása: az illesztőfelület.

## <a name="forwarding-hello-http-message"></a>Továbbító hello HTTP-üzenet
Ez a minta az I lezárását lenne érdekes toopush hello HTTP-kérelem túl keresztül[Runscope](http://www.runscope.com). Runscope egy felhőalapú szolgáltatás, amely HTTP-hibakeresés, a naplózás és figyelés. Ingyenes szint, így könnyen tootry, és lehetővé teszi velünk toosee hello HTTP-kérések valós idejű haladnak keresztül az API-kezelés szolgáltatás a rendelkeznek.

Hello `IHttpMessageProcessor` megvalósítási néz ki,

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

Képes tootake előnyeit voltam egy [meglévő ügyféloldali kódtára a Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) így később könnyen toopush `HttpRequestMessage` és `HttpResponseMessage` példányok be a szolgáltatásba. Ahhoz, tooaccess hello Runscope API szüksége lesz a fiók és API-kulcs. API-kulcs beolvasása vonatkozó utasítások megtalálhatók hello [alkalmazások létrehozása tooAccess Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) képernyőfelvétel.

## <a name="complete-sample"></a>Teljes mintát
Hello [forráskód](https://github.com/darrelmiller/ApimEventProcessor) és tesztek hello mintát a githubon. Szüksége lesz egy [API Management szolgáltatás](api-management-get-started.md), [a csatlakoztatott Eseményközpontot](api-management-howto-log-event-hubs.md), és egy [Tárfiók](../storage/common/storage-create-storage-account.md) toorun hello minta a szolgáltatást.   

hello minta értéke most egy egyszerű konzolalkalmazást, amely figyeli az Event Hubs származó események összefogásával alakítja őket egy `HttpRequestMessage` és `HttpResponseMessage` objektumokat, és ezután továbbítja őket a toohello Runscope API.

A hello animált kép a következő megtekintheti az adott kérelem benyújtásától tooan API hello fejlesztői portálra visszatérve hello konzol alkalmazás ábrázoló hello üzenet érkezett, a feldolgozott és továbbított és majd hello kérelem és válasz jelenik meg a hello Runscope forgalom Inspector.

![A kérelem tooRunscope továbbított bemutatója](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Összefoglalás
Az Azure API Management szolgáltatás biztosítja az ideális hely toocapture hello HTTP forgalom tooand utaznak az API-kat. Azure Event Hubs egy kiválóan méretezhető, alacsony költségű megoldás, hogy forgalom rögzítése és elágazó azt másodlagos feldolgozási rendszerek naplózási, figyelés, és más kifinomult analytics. Csatlakozás a like Runscope egy egyszerű, néhány dozen sornyi kód rendszerek figyelése too3rd fél forgalmat.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók az Azure Event Hubs
  * [Ismerkedés az Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Üzenetek fogadása az EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programozási útmutató](../event-hubs/event-hubs-programming-guide.md)
* További tudnivalók az API Management és az Event Hubs-integráció
  * [Hogyan toolog események tooAzure Event Hubs az Azure API Management](api-management-howto-log-event-hubs.md)
  * [Naplózó entitáshivatkozás](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [napló-eventhub szabályzatainak ismertetése](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
