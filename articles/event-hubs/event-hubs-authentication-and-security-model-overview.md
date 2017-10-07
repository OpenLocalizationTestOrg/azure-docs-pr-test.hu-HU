---
title: "Azure Event Hubs-hitelesítés és a biztonsági modell aaaOverview |} Microsoft Docs"
description: "Event Hubs hitelesítés és a biztonsági modell – áttekintés."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 93841e30-0c5c-4719-9dc1-57a4814342e7
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: sethm;clemensv
ms.openlocfilehash: e57ccda33e5ee20e635487cf91d9e8af594d3bd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-authentication-and-security-model-overview"></a>Event Hubs hitelesítés és a biztonsági modell – áttekintés
hello Azure Event Hubs biztonsági modell megfelel a követelményeknek hello:

* Csak olyan ügyfeleket, hogy érvényes hitelesítő adatokat küldhet adatokat tooan eseményközpont.
* Egy ügyfél egy másik ügyfél nem tudja megszemélyesíteni.
* Egy támadó ügyfelet blokkolható tooan eseményközpont adatok elküldését.

## <a name="client-authentication"></a>Ügyfél-hitelesítés
hello Event Hubs biztonsági modell kombinációja alapján [közös hozzáférésű Jogosultságkód (SAS)](../service-bus-messaging/service-bus-sas.md) jogkivonatok és *esemény-közzétevők*. Egy esemény-közzétevő határozza meg egy virtuális végpont az eseményközpontban. hello publisher használt toosend üzenetek tooan eseményközpont csak lehet. Már nem lehetséges tooreceive üzeneteit közzétevő.

Az eseményközpontok általában egy publisher ügyfelenként funkcióit használja. Hello közzétevők az event hubs tooany küldött összes üzenet a várólistában levő adott eseményközpont belül. Közzétevők részletes hozzáférés-vezérlés és a szabályozás engedélyezéséhez.

Minden Event Hubs-ügyfél hozzá van rendelve egy egyedi jogkivonatot, amely a feltöltött toohello ügyfél. hello jogkivonatok úgy, hogy minden egyedi jogkivonatot biztosít hozzáférést tooa különböző egyedi publisher előállítása. Ügyfél, amely rendelkezik egy token csak küldhet tooone publisher, de nincs más közzétevő. Ha több ügyfelek megosztás hello azonos jogkivonatot, majd azok osztja meg a gyártót.

Bár nem javasolt, továbbra is lehetséges tooequip eszközök jogkivonatokkal, amely közvetlen hozzáférést tooan eseményközpont adja meg. Minden olyan eszköz, amely tárolja Ez a token közvetlenül az adott eseményközpont küldhet üzeneteket. Ilyen eszköz nem lesz tulajdonos toothrottling. Ezenkívül hello eszköz nem lehet feketelistára teszi, a küldő toothat eseményközpont.

Összes jogkivonatot egy SAS-kulcs van aláírva. Általában minden jogkivonatok aláírva hello ugyanazzal a kulccsal. Az ügyfelek nem ismerik hello kulcsot. Ez megakadályozza, hogy más ügyfelekkel gyártási jogkivonatokat.

### <a name="create-hello-sas-key"></a>Hello SAS-kulcs létrehozása

Az Event Hubs névtér létrehozásakor hello szolgáltatás állít elő, 256 bites SAS-kulcs nevű **RootManageSharedAccessKey**. A kulcs biztosít küldeni, figyelésére és jogok toohello névtér kezelésére. További kulcsok is létrehozhat. Javasoljuk, hogy a egy kulcs, biztosít küldjön engedélyek toohello adott eseményközpont eredményez. Ez a témakör hátralévő hello, feltételezzük, hogy ez a kulcs elnevezett **EventHubSendKey**.

a következő példa hello egy csak küldési kulcsot hoz létre, hello eseményközpont létrehozásakor:

```csharp
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create event hub with a SAS rule that enables sending toothat event hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Rendszer jogkivonatokat hoz létre

Jogkivonatok hello SAS-kulcs használatával hozhat létre. Csak egy token ügyfelenként kell eredményez. Jogkivonatok használatával a következő metódus hello majd előállított. Összes jogkivonatot hello segítségével hozhatók létre **EventHubSendKey** kulcs. A tokenek hozzá van rendelve egy egyedi URI-t.

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Ez a metódus meghívásakor hello URI kell megadni: `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Minden jogkivonatokat hello URI megegyezik, hello kivételével `PUBLISHER_NAME`, amely nem lehet ugyanaz a minden token. Ideális esetben `PUBLISHER_NAME` jelöli hello azonosító hello ügyfél, amely megkapja a jogkivonatot.

Ez a módszer a következő struktúra hello létrehoz egy jogkivonatot:

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

hello jogkivonat lejárati ideje másodpercben 1970. január 1. a van megadva. hello az alábbiakban látható egy példa egy jogkivonatot:

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Általában hello jogkivonatok élettartama hasonlít, vagy meghaladja a hello ügyfél hello élettartamát. Ha hello ügyfél hello funkció tooobtain egy új jogkivonatot, egy rövidebb élettartamot rendelkező jogkivonatokat használható.

### <a name="sending-data"></a>Adatok küldése
Létrehozása után a hello jogkivonatokat, minden ügyfél a saját egyedi jogkivonattal lett kiépítve.

Hello ügyfél adatok eseményközpontnak küld, azt a küldési kérelmek hello jogkivonatok címkéket. egy támadó lehallgatás és hello jogkivonat, hello ügyfél és a hello eseményközpont hello kommunikációját ellophassák tooprevent egy titkosított csatornán keresztül kell megtörténnie.

### <a name="blacklisting-clients"></a>Az ügyfelek letiltott
A jogkivonat támadók ellopják, hello támadó megszemélyesítheti hello ügyfél, amelynek token ellopott. Letiltott ügyfél jeleníti meg, hogy az ügyfél használhatatlanná amíg nem kap egy új jogkivonatot, amely egy másik közzétevőt használ.

## <a name="authentication-of-back-end-applications"></a>Háttér-alkalmazások hitelesítése

az Event Hubs Eseményközpontokhoz ügyfelek által generált hello adatokat használó tooauthenticate háttér-alkalmazások biztonsági modell, amely hasonló toohello modell a Service Bus-üzenettémakörök használt alkalmaz. Az Event Hubs fogyasztói csoportot egyenértékű tooa előfizetés tooa Service Bus-témakörbe. Egy ügyfél egy fogyasztói csoportot hozhat létre, ha hello kérelem toocreate hello fogyasztói csoportot egy jogkivonatot biztosít hello eseményközpont tartozó jogosultságok kezelése, vagy a hello névtér toowhich hello eseményközpont tartozik. Egy ügyfél engedélyezett egy fogyasztói csoporton tooconsume adatait hello kérés fogadásához. Ha kíséri jogkivonatot, hogy biztosít fogadni az adott felhasználói csoportban jogok, hello eseményközpont, vagy hello névtér toowhich hello eseményközpont tartozik.

a Service Bus hello jelenlegi verziója nem támogatja az egyes előfizetések SAS szabályok. Ugyanez érvényes Event Hubs fogyasztói csoportok hello. SAS fogja támogatni a jövőbeli hello mindkét szolgáltatások.

Egyéni fogyasztói csoportok SAS hitelesítési hello hiányában használhat SAS kulcsok toosecure minden felhasználói csoport közös kulccsal. Ez a megközelítés lehetővé teszi, hogy egy alkalmazás tooconsume bármelyik hello fogyasztói csoportok az eseményközpontban.

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs toolearn látogasson el a következő témakörök hello:

* [Event Hubs – áttekintés]
* [Közös hozzáférésű Jogosultságkód áttekintése]
* [Az Event Hubsot használó mintaalkalmazások]

[Event Hubs – áttekintés]: event-hubs-what-is-event-hubs.md
[Az Event Hubsot használó mintaalkalmazások]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Közös hozzáférésű Jogosultságkód áttekintése]: ../service-bus-messaging/service-bus-sas.md

