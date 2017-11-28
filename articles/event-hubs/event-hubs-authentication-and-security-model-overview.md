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
# <a name="event-hubs-authentication-and-security-model-overview"></a><span data-ttu-id="84d2f-103">Event Hubs hitelesítés és a biztonsági modell – áttekintés</span><span class="sxs-lookup"><span data-stu-id="84d2f-103">Event Hubs authentication and security model overview</span></span>
<span data-ttu-id="84d2f-104">hello Azure Event Hubs biztonsági modell megfelel a követelményeknek hello:</span><span class="sxs-lookup"><span data-stu-id="84d2f-104">hello Azure Event Hubs security model meets hello following requirements:</span></span>

* <span data-ttu-id="84d2f-105">Csak olyan ügyfeleket, hogy érvényes hitelesítő adatokat küldhet adatokat tooan eseményközpont.</span><span class="sxs-lookup"><span data-stu-id="84d2f-105">Only clients that present valid credentials can send data tooan event hub.</span></span>
* <span data-ttu-id="84d2f-106">Egy ügyfél egy másik ügyfél nem tudja megszemélyesíteni.</span><span class="sxs-lookup"><span data-stu-id="84d2f-106">A client cannot impersonate another client.</span></span>
* <span data-ttu-id="84d2f-107">Egy támadó ügyfelet blokkolható tooan eseményközpont adatok elküldését.</span><span class="sxs-lookup"><span data-stu-id="84d2f-107">A rogue client can be blocked from sending data tooan event hub.</span></span>

## <a name="client-authentication"></a><span data-ttu-id="84d2f-108">Ügyfél-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="84d2f-108">Client authentication</span></span>
<span data-ttu-id="84d2f-109">hello Event Hubs biztonsági modell kombinációja alapján [közös hozzáférésű Jogosultságkód (SAS)](../service-bus-messaging/service-bus-sas.md) jogkivonatok és *esemény-közzétevők*.</span><span class="sxs-lookup"><span data-stu-id="84d2f-109">hello Event Hubs security model is based on a combination of [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) tokens and *event publishers*.</span></span> <span data-ttu-id="84d2f-110">Egy esemény-közzétevő határozza meg egy virtuális végpont az eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="84d2f-110">An event publisher defines a virtual endpoint for an event hub.</span></span> <span data-ttu-id="84d2f-111">hello publisher használt toosend üzenetek tooan eseményközpont csak lehet.</span><span class="sxs-lookup"><span data-stu-id="84d2f-111">hello publisher can only be used toosend messages tooan event hub.</span></span> <span data-ttu-id="84d2f-112">Már nem lehetséges tooreceive üzeneteit közzétevő.</span><span class="sxs-lookup"><span data-stu-id="84d2f-112">It is not possible tooreceive messages from a publisher.</span></span>

<span data-ttu-id="84d2f-113">Az eseményközpontok általában egy publisher ügyfelenként funkcióit használja.</span><span class="sxs-lookup"><span data-stu-id="84d2f-113">Typically, an event hub employs one publisher per client.</span></span> <span data-ttu-id="84d2f-114">Hello közzétevők az event hubs tooany küldött összes üzenet a várólistában levő adott eseményközpont belül.</span><span class="sxs-lookup"><span data-stu-id="84d2f-114">All messages that are sent tooany of hello publishers of an event hub are enqueued within that event hub.</span></span> <span data-ttu-id="84d2f-115">Közzétevők részletes hozzáférés-vezérlés és a szabályozás engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="84d2f-115">Publishers enable fine-grained access control and throttling.</span></span>

<span data-ttu-id="84d2f-116">Minden Event Hubs-ügyfél hozzá van rendelve egy egyedi jogkivonatot, amely a feltöltött toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="84d2f-116">Each Event Hubs client is assigned a unique token, which is uploaded toohello client.</span></span> <span data-ttu-id="84d2f-117">hello jogkivonatok úgy, hogy minden egyedi jogkivonatot biztosít hozzáférést tooa különböző egyedi publisher előállítása.</span><span class="sxs-lookup"><span data-stu-id="84d2f-117">hello tokens are produced such that each unique token grants access tooa different unique publisher.</span></span> <span data-ttu-id="84d2f-118">Ügyfél, amely rendelkezik egy token csak küldhet tooone publisher, de nincs más közzétevő.</span><span class="sxs-lookup"><span data-stu-id="84d2f-118">A client that possesses a token can only send tooone publisher, but no other publisher.</span></span> <span data-ttu-id="84d2f-119">Ha több ügyfelek megosztás hello azonos jogkivonatot, majd azok osztja meg a gyártót.</span><span class="sxs-lookup"><span data-stu-id="84d2f-119">If multiple clients share hello same token, then each of them shares a publisher.</span></span>

<span data-ttu-id="84d2f-120">Bár nem javasolt, továbbra is lehetséges tooequip eszközök jogkivonatokkal, amely közvetlen hozzáférést tooan eseményközpont adja meg.</span><span class="sxs-lookup"><span data-stu-id="84d2f-120">Although not recommended, it is possible tooequip devices with tokens that grant direct access tooan event hub.</span></span> <span data-ttu-id="84d2f-121">Minden olyan eszköz, amely tárolja Ez a token közvetlenül az adott eseményközpont küldhet üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="84d2f-121">Any device that holds this token can send messages directly into that event hub.</span></span> <span data-ttu-id="84d2f-122">Ilyen eszköz nem lesz tulajdonos toothrottling.</span><span class="sxs-lookup"><span data-stu-id="84d2f-122">Such a device will not be subject toothrottling.</span></span> <span data-ttu-id="84d2f-123">Ezenkívül hello eszköz nem lehet feketelistára teszi, a küldő toothat eseményközpont.</span><span class="sxs-lookup"><span data-stu-id="84d2f-123">Furthermore, hello device cannot be blacklisted from sending toothat event hub.</span></span>

<span data-ttu-id="84d2f-124">Összes jogkivonatot egy SAS-kulcs van aláírva.</span><span class="sxs-lookup"><span data-stu-id="84d2f-124">All tokens are signed with a SAS key.</span></span> <span data-ttu-id="84d2f-125">Általában minden jogkivonatok aláírva hello ugyanazzal a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="84d2f-125">Typically, all tokens are signed with hello same key.</span></span> <span data-ttu-id="84d2f-126">Az ügyfelek nem ismerik hello kulcsot. Ez megakadályozza, hogy más ügyfelekkel gyártási jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="84d2f-126">Clients are not aware of hello key; this prevents other clients from manufacturing tokens.</span></span>

### <a name="create-hello-sas-key"></a><span data-ttu-id="84d2f-127">Hello SAS-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="84d2f-127">Create hello SAS key</span></span>

<span data-ttu-id="84d2f-128">Az Event Hubs névtér létrehozásakor hello szolgáltatás állít elő, 256 bites SAS-kulcs nevű **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="84d2f-128">When creating an Event Hubs namespace, hello service generates a 256-bit SAS key named **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="84d2f-129">A kulcs biztosít küldeni, figyelésére és jogok toohello névtér kezelésére.</span><span class="sxs-lookup"><span data-stu-id="84d2f-129">This key grants send, listen, and manage rights toohello namespace.</span></span> <span data-ttu-id="84d2f-130">További kulcsok is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="84d2f-130">You can also create additional keys.</span></span> <span data-ttu-id="84d2f-131">Javasoljuk, hogy a egy kulcs, biztosít küldjön engedélyek toohello adott eseményközpont eredményez.</span><span class="sxs-lookup"><span data-stu-id="84d2f-131">It is recommended that you produce a key that grants send permissions toohello specific event hub.</span></span> <span data-ttu-id="84d2f-132">Ez a témakör hátralévő hello, feltételezzük, hogy ez a kulcs elnevezett **EventHubSendKey**.</span><span class="sxs-lookup"><span data-stu-id="84d2f-132">For hello remainder of this topic, it is assumed that you named this key **EventHubSendKey**.</span></span>

<span data-ttu-id="84d2f-133">a következő példa hello egy csak küldési kulcsot hoz létre, hello eseményközpont létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="84d2f-133">hello following example creates a send-only key when creating hello event hub:</span></span>

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

### <a name="generate-tokens"></a><span data-ttu-id="84d2f-134">Rendszer jogkivonatokat hoz létre</span><span class="sxs-lookup"><span data-stu-id="84d2f-134">Generate tokens</span></span>

<span data-ttu-id="84d2f-135">Jogkivonatok hello SAS-kulcs használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="84d2f-135">You can generate tokens using hello SAS key.</span></span> <span data-ttu-id="84d2f-136">Csak egy token ügyfelenként kell eredményez.</span><span class="sxs-lookup"><span data-stu-id="84d2f-136">You must produce only one token per client.</span></span> <span data-ttu-id="84d2f-137">Jogkivonatok használatával a következő metódus hello majd előállított.</span><span class="sxs-lookup"><span data-stu-id="84d2f-137">Tokens can then be produced using hello following method.</span></span> <span data-ttu-id="84d2f-138">Összes jogkivonatot hello segítségével hozhatók létre **EventHubSendKey** kulcs.</span><span class="sxs-lookup"><span data-stu-id="84d2f-138">All tokens are generated using hello **EventHubSendKey** key.</span></span> <span data-ttu-id="84d2f-139">A tokenek hozzá van rendelve egy egyedi URI-t.</span><span class="sxs-lookup"><span data-stu-id="84d2f-139">Each token is assigned a unique URI.</span></span>

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

<span data-ttu-id="84d2f-140">Ez a metódus meghívásakor hello URI kell megadni: `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="84d2f-140">When calling this method, hello URI should be specified as `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span></span> <span data-ttu-id="84d2f-141">Minden jogkivonatokat hello URI megegyezik, hello kivételével `PUBLISHER_NAME`, amely nem lehet ugyanaz a minden token.</span><span class="sxs-lookup"><span data-stu-id="84d2f-141">For all tokens, hello URI is identical, with hello exception of `PUBLISHER_NAME`, which should be different for each token.</span></span> <span data-ttu-id="84d2f-142">Ideális esetben `PUBLISHER_NAME` jelöli hello azonosító hello ügyfél, amely megkapja a jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="84d2f-142">Ideally, `PUBLISHER_NAME` represents hello ID of hello client that receives that token.</span></span>

<span data-ttu-id="84d2f-143">Ez a módszer a következő struktúra hello létrehoz egy jogkivonatot:</span><span class="sxs-lookup"><span data-stu-id="84d2f-143">This method generates a token with hello following structure:</span></span>

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

<span data-ttu-id="84d2f-144">hello jogkivonat lejárati ideje másodpercben 1970. január 1. a van megadva.</span><span class="sxs-lookup"><span data-stu-id="84d2f-144">hello token expiration time is specified in seconds from Jan 1, 1970.</span></span> <span data-ttu-id="84d2f-145">hello az alábbiakban látható egy példa egy jogkivonatot:</span><span class="sxs-lookup"><span data-stu-id="84d2f-145">hello following is an example of a token:</span></span>

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

<span data-ttu-id="84d2f-146">Általában hello jogkivonatok élettartama hasonlít, vagy meghaladja a hello ügyfél hello élettartamát.</span><span class="sxs-lookup"><span data-stu-id="84d2f-146">Typically, hello tokens have a lifespan that resembles or exceeds hello lifespan of hello client.</span></span> <span data-ttu-id="84d2f-147">Ha hello ügyfél hello funkció tooobtain egy új jogkivonatot, egy rövidebb élettartamot rendelkező jogkivonatokat használható.</span><span class="sxs-lookup"><span data-stu-id="84d2f-147">If hello client has hello capability tooobtain a new token, tokens with a shorter lifespan can be used.</span></span>

### <a name="sending-data"></a><span data-ttu-id="84d2f-148">Adatok küldése</span><span class="sxs-lookup"><span data-stu-id="84d2f-148">Sending data</span></span>
<span data-ttu-id="84d2f-149">Létrehozása után a hello jogkivonatokat, minden ügyfél a saját egyedi jogkivonattal lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="84d2f-149">Once hello tokens have been created, each client is provisioned with its own unique token.</span></span>

<span data-ttu-id="84d2f-150">Hello ügyfél adatok eseményközpontnak küld, azt a küldési kérelmek hello jogkivonatok címkéket.</span><span class="sxs-lookup"><span data-stu-id="84d2f-150">When hello client sends data into an event hub, it tags its send request with hello token.</span></span> <span data-ttu-id="84d2f-151">egy támadó lehallgatás és hello jogkivonat, hello ügyfél és a hello eseményközpont hello kommunikációját ellophassák tooprevent egy titkosított csatornán keresztül kell megtörténnie.</span><span class="sxs-lookup"><span data-stu-id="84d2f-151">tooprevent an attacker from eavesdropping and stealing hello token, hello communication between hello client and hello event hub must occur over an encrypted channel.</span></span>

### <a name="blacklisting-clients"></a><span data-ttu-id="84d2f-152">Az ügyfelek letiltott</span><span class="sxs-lookup"><span data-stu-id="84d2f-152">Blacklisting clients</span></span>
<span data-ttu-id="84d2f-153">A jogkivonat támadók ellopják, hello támadó megszemélyesítheti hello ügyfél, amelynek token ellopott.</span><span class="sxs-lookup"><span data-stu-id="84d2f-153">If a token is stolen by an attacker, hello attacker can impersonate hello client whose token has been stolen.</span></span> <span data-ttu-id="84d2f-154">Letiltott ügyfél jeleníti meg, hogy az ügyfél használhatatlanná amíg nem kap egy új jogkivonatot, amely egy másik közzétevőt használ.</span><span class="sxs-lookup"><span data-stu-id="84d2f-154">Blacklisting a client renders that client unusable until it receives a new token that uses a different publisher.</span></span>

## <a name="authentication-of-back-end-applications"></a><span data-ttu-id="84d2f-155">Háttér-alkalmazások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="84d2f-155">Authentication of back-end applications</span></span>

<span data-ttu-id="84d2f-156">az Event Hubs Eseményközpontokhoz ügyfelek által generált hello adatokat használó tooauthenticate háttér-alkalmazások biztonsági modell, amely hasonló toohello modell a Service Bus-üzenettémakörök használt alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="84d2f-156">tooauthenticate back-end applications that consume hello data generated by Event Hubs clients, Event Hubs employs a security model that is similar toohello model that is used for Service Bus topics.</span></span> <span data-ttu-id="84d2f-157">Az Event Hubs fogyasztói csoportot egyenértékű tooa előfizetés tooa Service Bus-témakörbe.</span><span class="sxs-lookup"><span data-stu-id="84d2f-157">An Event Hubs consumer group is equivalent tooa subscription tooa Service Bus topic.</span></span> <span data-ttu-id="84d2f-158">Egy ügyfél egy fogyasztói csoportot hozhat létre, ha hello kérelem toocreate hello fogyasztói csoportot egy jogkivonatot biztosít hello eseményközpont tartozó jogosultságok kezelése, vagy a hello névtér toowhich hello eseményközpont tartozik.</span><span class="sxs-lookup"><span data-stu-id="84d2f-158">A client can create a consumer group if hello request toocreate hello consumer group is accompanied by a token that grants manage privileges for hello event hub, or for hello namespace toowhich hello event hub belongs.</span></span> <span data-ttu-id="84d2f-159">Egy ügyfél engedélyezett egy fogyasztói csoporton tooconsume adatait hello kérés fogadásához. Ha kíséri jogkivonatot, hogy biztosít fogadni az adott felhasználói csoportban jogok, hello eseményközpont, vagy hello névtér toowhich hello eseményközpont tartozik.</span><span class="sxs-lookup"><span data-stu-id="84d2f-159">A client is allowed tooconsume data from a consumer group if hello receive request is accompanied by a token that grants receive rights on that consumer group, hello event hub, or hello namespace toowhich hello event hub belongs.</span></span>

<span data-ttu-id="84d2f-160">a Service Bus hello jelenlegi verziója nem támogatja az egyes előfizetések SAS szabályok.</span><span class="sxs-lookup"><span data-stu-id="84d2f-160">hello current version of Service Bus does not support SAS rules for individual subscriptions.</span></span> <span data-ttu-id="84d2f-161">Ugyanez érvényes Event Hubs fogyasztói csoportok hello.</span><span class="sxs-lookup"><span data-stu-id="84d2f-161">hello same holds true for Event Hubs consumer groups.</span></span> <span data-ttu-id="84d2f-162">SAS fogja támogatni a jövőbeli hello mindkét szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="84d2f-162">SAS support will be added for both features in hello future.</span></span>

<span data-ttu-id="84d2f-163">Egyéni fogyasztói csoportok SAS hitelesítési hello hiányában használhat SAS kulcsok toosecure minden felhasználói csoport közös kulccsal.</span><span class="sxs-lookup"><span data-stu-id="84d2f-163">In hello absence of SAS authentication for individual consumer groups, you can use SAS keys toosecure all consumer groups with a common key.</span></span> <span data-ttu-id="84d2f-164">Ez a megközelítés lehetővé teszi, hogy egy alkalmazás tooconsume bármelyik hello fogyasztói csoportok az eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="84d2f-164">This approach enables an application tooconsume data from any of hello consumer groups of an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84d2f-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84d2f-165">Next steps</span></span>
<span data-ttu-id="84d2f-166">További információ az Event Hubs toolearn látogasson el a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="84d2f-166">toolearn more about Event Hubs, visit hello following topics:</span></span>

* <span data-ttu-id="84d2f-167">[Event Hubs – áttekintés]</span><span class="sxs-lookup"><span data-stu-id="84d2f-167">[Event Hubs overview]</span></span>
* <span data-ttu-id="84d2f-168">[Közös hozzáférésű Jogosultságkód áttekintése]</span><span class="sxs-lookup"><span data-stu-id="84d2f-168">[Overview of Shared Access Signatures]</span></span>
* <span data-ttu-id="84d2f-169">[Az Event Hubsot használó mintaalkalmazások]</span><span class="sxs-lookup"><span data-stu-id="84d2f-169">[Sample applications that use Event Hubs]</span></span>

[Event Hubs – áttekintés]: event-hubs-what-is-event-hubs.md
[Az Event Hubsot használó mintaalkalmazások]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Közös hozzáférésű Jogosultságkód áttekintése]: ../service-bus-messaging/service-bus-sas.md

