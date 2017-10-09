---
title: "Megosztott hozzáférési aláírásokkal aaaAzure Service Bus hitelesítési |} Microsoft Docs"
description: "Service Bus hitelesítési megosztott hozzáférési aláírásokkal – áttekintés, SAS-hitelesítés és az Azure Service Bus adatait áttekintése."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: 773bb11720384d7245820b56dc25b8e064ffa746
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-authentication-with-shared-access-signatures"></a>A Service Bus hitelesítési megosztott hozzáférési aláírásokkal

*Közös hozzáférésű Jogosultságkód* (SAS) hello elsődleges biztonsági mechanizmus a Service Bus üzenetkezelés. Ez a cikk ismerteti, amelyek SAS, hogyan működnek, és hogyan toouse platform-független módon őket.

SAS hitelesítési lehetővé teszi, hogy az alkalmazások tooauthenticate tooService Bus hello névtér, illetve üzenetküldési entitás (üzenetsor vagy témakör) az adott jogosultságok társított hello szolgáltatást konfigurált hozzáférési kulcs használata. A kulcs toogenerate egy SAS-jogkivonatot, hogy az ügyfelek ezután használhatja tooauthenticate tooService Bus használhatja.

SAS-hitelesítési támogatás a hello Azure SDK 2.0-s és újabb verziók része.

## <a name="overview-of-sas"></a>SAS áttekintése

Megosztott hozzáférési aláírásokkal olyan hitelesítési mechanizmus biztonságos SHA-256 kivonatokkal vagy URI-k alapján. SAS, akkor ez egy nagyon erős eszköz összes Service Bus szolgáltatás által használt. Tényleges használatban van, SAS két részből áll: egy *megosztott hozzáférési házirend* és egy *közös hozzáférésű Jogosultságkód* (gyakran nevezik egy *token*).

A Service Bus SAS hitelesítési magában foglalja a hello konfigurálása a Service Bus erőforrás társított jogosultságok titkosítási kulcsot. Ügyfelek jogcím tooService Bus erőforrások eléréséhez a SAS-jogkivonat segítségével. Ez a token hello erőforrás URI férnek hozzá a áll, és aláírással rendelkező hello elévülés kulcs konfigurálva.

A Service Bus közös hozzáférésű Jogosultságkód-engedélyezési szabályok konfigurálásával [továbbítja](service-bus-fundamentals-hybrid-solutions.md#relays), [várólisták](service-bus-fundamentals-hybrid-solutions.md#queues), és [témakörök](service-bus-fundamentals-hybrid-solutions.md#topics).

SAS hitelesítési hello a következő elemeket használja:

* [Megosztott hozzáférés-engedélyezési szabály](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule): A 256 bites Base64 ábrázolás elsődleges titkosítási kulcs, egy nem kötelező másodlagos kulcsot, és a kulcs nevét és társított jogosultságok (gyűjteménye *figyelésére*, *küldése*, vagy *kezelése* jogosultságok).
* [Közös hozzáférésű Jogosultságkód](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) token: generálta hello HMAC-SHA256 hello URI-azonosítója, amelyhez a hello erőforrás álló erőforrás-karakterlánc, és egy lejárati hello titkosítási kulccsal. hello aláírás és egyéb hello a következő részekben leírt elemeket egy karakterlánc tooform hello SAS-jogkivonat a formátumát.

## <a name="shared-access-policy"></a>Megosztott elérési házirendet

Egy SAS kapcsolatos fontos toounderstand, hogy egy házirend kezdődik. Minden egyes házirend mellett dönt három adatra: **neve**, **hatókör**, és **engedélyek**. Hello **neve** most, hogy; adott hatókörén belül egyedi nevet. hello hatóköre elég egyszerűen: fennállt hello szóban forgó hello erőforrás URI Azonosítóját. A Service Bus-névtér hello hatóköre hello teljesen minősített tartománynevét (FQDN), például a `https://<yournamespace>.servicebus.windows.net/`.

hello elérhető engedélyek a házirend magától értetődő-nagymértékben:

* Küldés
* Figyelés
* Kezelés

Hello házirend létrehozása után van hozzárendelve egy *elsődleges kulcs* és egy *másodlagos kulcs*. Ezek a kriptográfiai erős kulcsokat. Nem elvesznek vagy szivárgás lépett fel őket – azok mindig megtalálhatók a hello [Azure-portálon][Azure portal]. Generált hello kulcsok bármelyikét használhatja, és bármikor helyreállíthatók. Azonban ha újragenerálja vagy hello elsődleges kulcs hello házirendben módosítja, a megosztott hozzáférési aláírásokkal létre belőle a rendszer érvényteleníti.

A Service Bus-névtér létrehozásakor hello teljes névtér neve automatikusan létrehozott egy házirend **RootManageSharedAccessKey**, és ez a házirend az összes jogosult. Nem bejelentkezni **legfelső szintű**, így nem használja ezt a házirendet, kivéve, ha valóban okunk. További házirendeket is létrehozhat a hello **konfigurálása** hello névtér hello portálon lapon. Fontos toonote, amely egy Service Bus (névtér, várólista stb.) egy fa szintjének csak too12 csatolt házirendek tooit fel.

## <a name="configuration-for-shared-access-signature-authentication"></a>Közös hozzáférésű Jogosultságkód-hitelesítés konfigurációja
Konfigurálhatja a hello [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) a Szolgáltatásbusz-névterek, a várólisták vagy a témakörök szabály. Konfigurálása egy [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) egy Service Bus az előfizetés jelenleg nem támogatott, de használhatja a névtér vagy témakör toosecure hozzáférés toosubscriptions konfigurált szabályokat. Működő minta azt mutatja be ezt az eljárást, lásd: hello [Service Bus előfizetések használata közös hozzáférésű Jogosultságkód (SAS) hitelesítési](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) minta.

Ezek a szabályok 12 legfeljebb egy Service Bus-névtér, üzenetsor vagy témakör is lehet konfigurálni. Szabályok konfigurálása a Service Bus-névtér tooall bejegyzései szerepelnek a névtér alkalmazni.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

Az ábrán hello *manageRuleNS*, *sendRuleNS*, és *listenRuleNS* engedélyezési szabályok vonatkoznak V1 tooboth várólista és ez a témakör a T1, amíg a *listenRuleQ*  és *sendRuleQ* csak V1 tooqueue alkalmazása és *sendRuleT* csak tootopic T1 vonatkozik.

a paramétereket hello egy [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) a következők:

| Paraméter | Leírás |
| --- | --- |
| *Kulcsnév* |Egy karakterlánc, amely hello engedélyezési szabályt. |
| *PrimaryKey* |A base64-kódolású 256 bites elsődleges kulcs aláírása és hello SAS-token ellenőrzése. |
| *Másodlagos kulcs* |A base64-kódolású 256 bites másodlagos kulcs aláírása és hello SAS-token ellenőrzése. |
| *AccessRights* |Hello engedélyezési szabály által megadott hozzáférési jogok listáját. Ezek a jogosultságok bármely gyűjtemény figyelés, a Küldés és a kezelés jogok lehet. |

A Service Bus-névtér ki van építve, amikor egy [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), a [kulcsnév](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) túl beállítása**RootManageSharedAccessKey**, alapértelmezés szerint létrejön.

## <a name="generate-a-shared-access-signature-token"></a>Egy közös hozzáférésű Jogosultságkód (jogkivonat) létrehozása

hello házirend maga nincs Service Bus hello hozzáférési jogkivonat. Mely hello a létrehozott hozzáférési jogkivonat - vagy hello elsődleges vagy másodlagos kulcs használatával hello objektum. Bármely ügyfél, amely aláírási kulcsok hello közös hozzáférésű engedélyezési szabályban megadott hozzáférési toohello hello SAS-jogkivonat hozhat létre. hello token jön létre, gondosan létrehozásával hello formátuma a következő karakterláncot:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Ahol `signature-string` hello hatókör hello jogkivonat hello SHA-256-kivonata (**hatókör** hello előző szakaszban leírtak szerint) a CR LF Karakterpár fűzött és a lejárati idő (másodpercben hello epoch óta: `00:00:00 UTC` 1970. január 1. a). 

> [!NOTE]
> tooavoid egy rövid jogkivonat lejárati ideje, javasoljuk, hogy legalább egy 32 bites előjel nélküli egész számokat, vagy lehetőleg egy hosszú (64 bites) egész szám hello lejárati idő értéknek megfelelően kódolja.  
> 
> 

hello kivonatoló pszeudo kód a következő hasonló toohello és 32 bájt visszaadása.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

hello nem kivonatolt értékei hello **SharedAccessSignature** úgy, hogy hello címzett számíthatja hello kivonatoló hello az ugyanezen paraméterekkel, toobe meg arról, hogy vissza hello ugyanazt az eredményt. hello URI hello hatókör határozza meg, és hello kulcsnév hello házirend használt toobe toocompute hello kivonatoló azonosítja. Ez fontos biztonsági szempontból. Hello aláírása nem egyezik a mely hello címzett (Service Bus) számítja ki, ha a hozzáférés megtagadva. Ezen a ponton biztos lehet, hogy hello küldő toohello hívóbetűt kellett, és kell jogosultságokat hello hello házirendben megadott.

Vegye figyelembe, hogy kell-e használnia hello kódolt erőforrás URI ehhez a művelethez. hello erőforrás URI hello teljes URI-címe hello Service Bus erőforrás toowhich hozzáférést igényelnek. Például `http://<namespace>.servicebus.windows.net/<entityPath>` vagy `sb://<namespace>.servicebus.windows.net/<entityPath>`; Ez azt jelenti, hogy `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`.

az aláíráshoz használt hello közös hozzáférésű engedélyezési szabály megadott ezt az URI, vagy egyik szülőobjektumtól hierarchikus hello entitás kell konfigurálni. Például `http://contoso.servicebus.windows.net/contosoTopics/T1` vagy `http://contoso.servicebus.windows.net` hello előző példában.

A SAS-jogkivonat esetén érvényes hello tartozó összes erőforrást `<resourceURI>` hello használt `signature-string`.

Hello [kulcsnév](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) hello SAS tokent jelenti toohello **kulcsnév** közös hozzáférésű engedélyezési szabály hello toogenerate hello token használjuk.

Hello *URL-kódolású-resourceURI* kell lennie hello megegyeznek a hello hello számítási hello aláírás során használt hello karakterlánc bejelentkezési URI. Meg kell [százalék-kódolású](https://msdn.microsoft.com/library/4fkewx0t.aspx).

Ajánlott hello használt hello kulcsok rendszeres újragenerálása [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objektum. Alkalmazások általában használandó hello [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) toogenerate egy SAS-jogkivonatot. Hello kulcsok újragenerálása, ha le kell cserélni hello [másodlagos kulcs](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) a hello régi elsődleges kulcs, és hozzon létre egy új kulcsot hello új elsődleges kulcsaként. Ez lehetővé teszi toocontinue tokenek használatára a hitelesítéshez, amely kiállított hello régi elsődleges kulccsal, és még nem lejárt.

Ha a kulcs biztonsága sérül, és toorevoke hello kulcsot tartalmaz, újragenerálás mindkét hello [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) és hello [másodlagos kulcs](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) , egy [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), Felülírás új kulcsokkal. Ez az eljárás érvényteleníti összes jogkivonatot hello régi kulccsal aláírva.

## <a name="how-toouse-shared-access-signature-authentication-with-service-bus"></a>Hogyan toouse közös hozzáférésű Jogosultságkód hitelesítés a Service busszal

a következő forgatókönyvek hello tartalmazza az engedélyezési szabályok konfigurálása, SAS-tokenje és ügyfél-hitelesítés létrehozását.

A teljes minta a Service Bus-alkalmazás, amely bemutatja a hello konfigurációs és a SAS engedélyezési, lásd: [közös hozzáférésű Jogosultságkód hitelesítés a Service busszal](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Kapcsolódó minta azt mutatja be a SAS-engedélyezési szabályok konfigurálva a névterek vagy témakörök toosecure Service Bus előfizetések hello használata érhető el itt: [használatával közös hozzáférésű Jogosultságkód (SAS) hitelesítés a Service Bus-előfizetések ](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-a-namespace"></a>Egy névtér hozzáférés megosztott hozzáférés-engedélyezési szabályok

Műveletek hello Service Bus-névtér legfelső szintű tanúsítvány alapú hitelesítést igényelnek. Azure-előfizetése egy felügyeleti tanúsítványt kell feltöltenie. egy felügyeleti tanúsítványt tooupload hello lépésekkel [Itt](../cloud-services/cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate), hello segítségével [Azure-portálon][Azure portal]. Az Azure felügyeleti tanúsítványok kapcsolatos további információkért lásd: hello [Azure tanúsítványok – áttekintés](../cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

megosztott hozzáférés-engedélyezési szabályok a Service Bus-névtér eléréséhez hello végpont a következőképpen történik:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/
```

toocreate egy [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objektum a Service Bus-névtér, végrehajtani a POST műveletet ezen a végponton JSON vagy XML szerializált hello szabály adatokkal. Példa:

```csharp
// Base address for accessing authorization rules on a namespace
string baseAddress = @"https://management.core.windows.net/<subscriptionId>/services/ServiceBus/namespaces/<namespace>/AuthorizationRules/";

// Configure authorization rule with base64-encoded 256-bit key and Send rights
var sendRule = new SharedAccessAuthorizationRule("contosoSendAll",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send });

// Operations on hello Service Bus namespace root require certificate authentication.
WebRequestHandler handler = new WebRequestHandler
{
    ClientCertificateOptions = ClientCertificateOption.Manual
};
// Access hello management certificate by subject name
handler.ClientCertificates.Add(GetCertificate(<certificateSN>));

HttpClient httpClient = new HttpClient(handler)
{
    BaseAddress = new Uri(baseAddress)
};
httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
httpClient.DefaultRequestHeaders.Add("x-ms-version", "2015-01-01");

// Execute a POST operation on hello baseAddress above toocreate an auth rule
var postResult = httpClient.PostAsJsonAsync("", sendRule).Result;
```

Ehhez hasonlóan használja a GET műveletet hello végpont tooread hello engedélyezési szabályok hello névtér konfigurálva.

tooupdate vagy egy adott engedélyezési szabály törlése használja a következő végpont hello:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/{KeyName}
```

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Egy entitás hozzáférés megosztott hozzáférés-engedélyezési szabályok

Próbál hozzáférni egy [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) konfigurálva a Service Bus-üzenetsor vagy témakör keresztül hello objektum [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) hello gyűjtemény megfelelő [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) vagy [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

hello a következő kód bemutatja, hogyan tooadd engedélyezési szabályok várólista.

```csharp
// Create an instance of NamespaceManager for hello operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it toohello queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it toohello queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it toohello queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create hello queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Közös hozzáférésű Jogosultságkód engedélyezési használata

Hello Service Bus .NET-kódtárakra hello Azure .NET SDK használata alkalmazások használhatják hello SAS engedélyezést [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) osztály. hello alábbi kód bemutatja, hello jogkivonat-szolgáltató toosend üzenetek tooa Service Bus-üzenetsorba hello használatát.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message tooqueue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Alkalmazások is használható SAS hitelesítési módszereket, amelyek fogadja el a kapcsolati karakterláncok egy SAS-kapcsolati karakterlánc használatával.

Vegye figyelembe, hogy toouse SAS engedélyezési a Service Bus-továbbítók, SAS-kulcsok hello Service Bus-névtér konfigurált használja. A hello névtér explicit módon létrehozott egy továbbítót ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) rendelkező egy [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription)) objektum számára, hogy a továbbító hello SAS szabályainak beállítása. toouse SAS engedélyezési Service Bus-előfizetések, a Service Bus-névtér és a témakör a SAS-kulcsok is használhatja.

## <a name="use-hello-shared-access-signature-at-http-level"></a>A közös hozzáférésű Jogosultságkód hello használata (HTTP szinten)

Most, hogy tudjuk, hogyan toocreate megosztott hozzáférési aláírásokkal bármely entitás a Service Bus, akkor készen áll tooperform HTTP POST:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Ne feledje, hogy ez minden működik. Az üzenetsor, témakör vagy előfizetés SAS hozhat létre. 

Ha ad a feladó, vagy az ügyfél egy SAS-jogkivonatot, nem rendelkeznek hello kulcs közvetlenül, és azok nem fordított hello kivonatoló tooobtain azt. Hozzáférése van keresztül mi eléréséhez, és mennyi ideig. Egy fontos tooremember, hogy ha hello elsődleges kulcs hello házirendben módosítja, a megosztott hozzáférési aláírásokkal létre belőle a rendszer érvényteleníti.

## <a name="use-hello-shared-access-signature-at-amqp-level"></a>A közös hozzáférésű Jogosultságkód hello használata (AMQP szinten)

Hello előző szakaszban megtudhatta, hogyan toouse hello SAS-jogkivonatot az adatok toohello Service Bus küldési HTTP POST-kérelmet. Mint tudja, érheti el a Service Bus használatával hello speciális Message Queuing protokoll (AMQP), amely elsődleges hello protokoll toouse teljesítményének javítására szolgál, számos forgatókönyvben. SAS-token használatát a AMQP hello hello dokumentumban ismertetett [AMQP Claim-Based biztonsági 1.0-s verziójának](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) , amely a munkát vázlat 2013 jól támogatott az Azure-ban, de még ma óta.

Toosend adatok tooService Bus megkezdése előtt hello publisher kell küldenie egy AMQP üzenet tooa jól meghatározott AMQP nevű csomópont belül hello SAS-jogkivonat **$cbs** (lásd a "speciális" várólista hello szolgáltatás tooacquire által használt és az összes ellenőrzése hello SAS-tokenje). hello publisher kell megadnia a hello **ReplyTo** AMQP üdvözlőüzenetére belül mezőben; ez hello csomópont, mely hello szolgáltatás hello jogkivonatok érvényesség-ellenőrzése (egyszerű kérelem/válasz mintát közötti hello eredményét toohello közzétevő ügyfélkéréseire válaszol. közzétevő és szolgáltatás). Létrejön a válasz csomópont "on hello menet közben,", és beszéljen "távoli csomópont dinamikus létrehozása" hello AMQP 1.0-specifikáció szerint. Adott hello SAS-jogkivonat érvényességének ellenőrzése, miután hello publisher előre képesek haladni, és toosend adatok toohello szolgáltatás elindítása.

hello következő lépések bemutatják, hogyan toosend hello-SAS-jogkivonat AMQP protokollt használnak a hello [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) könyvtár. Ez akkor hasznos, ha nem használ hello hivatalos Service Bus SDK (például a WinRT, .net Compact Framework, a .net keretrendszer Micro és monó) c. fejlesztése\#. Természetesen érdemes használni ezt a szalagtárat toohelp hogyan jogcímalapú biztonsági eljárásainak megértését hello AMQP szinten működik, mint megtudhatta, hogyan működik szintű hello HTTP (egy HTTP POST kérelem és hello SAS tokent küldött belül hello "Engedélyezés" fejlécet). Ha nem kívánja ilyen AMQP átfogó ismerete, használhatja a hello hivatalos Service Bus SDK .net Framework alkalmazások, amelyek fog ő elvégezze ezt.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) toosend</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct hello put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive hello response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // hello sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Hello `PutCbsToken()` metódus kap hello *kapcsolat* (AMQP kapcsolat osztálypéldány hello által biztosított [AMQP .NET Lite könyvtár](https://github.com/Azure/amqpnetlite)), amely hello TCP kapcsolat toohello szolgáltatás és a hello jelöli *sasToken* hello SAS-token toosend paraméter. 

> [!NOTE]
> Fontos, hogy hello kapcsolat jön létre **SASL hitelesítési módszer beállítása tooEXTERNAL** (és nem az alapértelmezett egyszerű felhasználónévvel és jelszóval, ha már nincs szükség toosend hello SAS-jogkivonat használt hello).
> 
> 

A következő hello publisher két AMQP hivatkozásokat hoz hello SAS-jogkivonat küldését és fogadását a hello válasz (hello jogkivonat érvényesítésére eredmény) hello szolgáltatásból.

hello AMQP üzenet tulajdonságait, és egy egyszerű üzenet több információt tartalmazza. hello SAS-jogkivonat neve (a saját konstruktoraikban használatával) üdvözlőüzenetére hello törzsét. Hello **"ReplyTo"** tulajdonsága toohello csomópontnév hello ellenőrzési eredmény hello fogadó hivatkozásra (módosíthatja a nevét, és a rendszer automatikusan létrehozza dinamikusan hello szolgáltatás) fogadására. hello utolsó három alkalmazáshoz vagy egyéni tulajdonságok által használt hello szolgáltatás tooindicate milyen művelet tooexecute rendelkezik. Hello CBS vázlat specifikáció szerint, fel kell-e az hello **műveletnév** ("put-token"), hello **jogkivonattípusnak** (az ebben az esetben a "servicebus.windows.net:sastoken"), és hello **" neve"hello célközönség** toowhich hello token (hello teljes entitás) érvényes.

Hello SAS-jogkivonat elküldése hello küldő hivatkozásra, után hello publisher hello válasz hello fogadó hivatkozásra kell olvasni. hello válasz egy alkalmazás tulajdonsággal nevű egyszerű AMQP üzenet **"állapot-kód"** , amely ugyanaz, mint a HTTP-állapotkódot értékek hello tartalmazhat.

## <a name="rights-required-for-service-bus-operations"></a>A Service Bus műveletekhez szükséges jogok

hello alábbi táblázat ismerteti a Service Bus erőforrásainak különböző műveleteihez szükséges hello hozzáférési jogok.

| Művelet | Jogcím szükséges | Jogcím-hatókör |
| --- | --- | --- |
| **Namespace** | | |
| A névtér engedélyezési szabály konfigurálása |Kezelés |Minden névtér cím |
| **Szolgáltatás-beállításjegyzék** | | |
| Magánfelhő házirendek felsorolása |Kezelés |Minden névtér cím |
| A névtér figyelését |Figyelés |Minden névtér cím |
| Tooa figyelő üzenetek küldése egy névtérben |Küldés |Minden névtér cím |
| **Várólista** | | |
| Üzenetsor létrehozása |Kezelés |Minden névtér cím |
| Üzenetsor törlése |Kezelés |Bármilyen érvényes várósor címe |
| A várólisták számbavétele |Kezelés |$ Erőforrások/várólisták |
| Első hello várólista leírása |Kezelés |Bármilyen érvényes várósor címe |
| A várólisták engedélyezési szabály konfigurálása |Kezelés |Bármilyen érvényes várósor címe |
| Toohello várólistán küldése |Küldés |Bármilyen érvényes várósor címe |
| Üzenetek fogadása egy üzenetsorból |Figyelés |Bármilyen érvényes várósor címe |
| Szakítsa vagy üzenetek befejezése után üdvözlőüzenetére fogadásának betekintés-zárolási mód |Figyelés |Bármilyen érvényes várósor címe |
| Késlelteti a későbbi beolvasásához üzenet |Figyelés |Bármilyen érvényes várósor címe |
| Kézbesítetlen levelek üzenet |Figyelés |Bármilyen érvényes várósor címe |
| Egy üzenet-várólista munkamenethez társított hello állapot beolvasása |Figyelés |Bármilyen érvényes várósor címe |
| Egy üzenet-várólista munkamenethez társított hello állapotának beállítása |Figyelés |Bármilyen érvényes várósor címe |
| **A témakör** | | |
| Üzenettémakör létrehozása |Kezelés |Minden névtér cím |
| Egy témakör törlése |Kezelés |Bármilyen érvényes témakör cím |
| Témakörök számbavétele |Kezelés |$ Erőforrások/kapcsolatos témakörök |
| Első hello témakör leírása |Kezelés |Bármilyen érvényes témakör cím |
| A témakör az engedélyezési szabály konfigurálása |Kezelés |Bármilyen érvényes témakör cím |
| Toohello témakör küldése |Küldés |Bármilyen érvényes témakör cím |
| **Előfizetés** | | |
| Előfizetés létrehozása |Kezelés |Minden névtér cím |
| Előfizetés törlése |Kezelés |.. /myTopic/Subscriptions/mySubscription |
| Előfizetések számbavétele |Kezelés |.. / myTopic/előfizetések |
| Előfizetés leírását beolvasása |Kezelés |.. /myTopic/Subscriptions/mySubscription |
| Szakítsa vagy üzenetek befejezése után üdvözlőüzenetére fogadásának betekintés-zárolási mód |Figyelés |.. /myTopic/Subscriptions/mySubscription |
| Késlelteti a későbbi beolvasásához üzenet |Figyelés |.. /myTopic/Subscriptions/mySubscription |
| Kézbesítetlen levelek üzenet |Figyelés |.. /myTopic/Subscriptions/mySubscription |
| A témakör munkamenethez tartozó hello állapot beolvasása |Figyelés |.. /myTopic/Subscriptions/mySubscription |
| A témakör munkamenethez tartozó hello állapotának beállítása |Figyelés |.. /myTopic/Subscriptions/mySubscription |
| **Szabályok** | | |
| Szabály létrehozása |Kezelés |.. /myTopic/Subscriptions/mySubscription |
| Szabály törlése |Kezelés |.. /myTopic/Subscriptions/mySubscription |
| Szabályok számbavétele |Kezelése vagy figyelésére |.. /myTopic/Subscriptions/mySubscription/Rules 

## <a name="next-steps"></a>Következő lépések

toolearn Service Bus üzenetkezelés, kapcsolatos további információkért tekintse meg a következő témakörök hello.

* [A Service Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-üzenetsorok, -témakörök és -előfizetések](service-bus-queues-topics-subscriptions.md)
* [Hogyan toouse Service Bus-üzenetsorok](service-bus-dotnet-get-started-with-queues.md)
* [Hogyan toouse Service Bus üzenettémák és előfizetések](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com