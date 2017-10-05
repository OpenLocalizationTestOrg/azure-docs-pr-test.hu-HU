---
title: "Azure Event Hubs-hitelesítés és a biztonsági modell áttekintése |} Microsoft Docs"
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
ms.openlocfilehash: 5abdbf70d4fdb2c7feb0f3537ecc0f2abf0775a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="event-hubs-authentication-and-security-model-overview"></a><span data-ttu-id="5d816-103">Event Hubs hitelesítés és a biztonsági modell – áttekintés</span><span class="sxs-lookup"><span data-stu-id="5d816-103">Event Hubs authentication and security model overview</span></span>
<span data-ttu-id="5d816-104">Az Azure Event Hubs biztonsági modell megfelel-e a következő követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="5d816-104">The Azure Event Hubs security model meets the following requirements:</span></span>

* <span data-ttu-id="5d816-105">Csak ügyfeleket, hogy érvényes hitelesítő adatokat küldhet egy eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="5d816-105">Only clients that present valid credentials can send data to an event hub.</span></span>
* <span data-ttu-id="5d816-106">Egy ügyfél egy másik ügyfél nem tudja megszemélyesíteni.</span><span class="sxs-lookup"><span data-stu-id="5d816-106">A client cannot impersonate another client.</span></span>
* <span data-ttu-id="5d816-107">Egy támadó ügyfelet is letiltja a adatok küldése az eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="5d816-107">A rogue client can be blocked from sending data to an event hub.</span></span>

## <a name="client-authentication"></a><span data-ttu-id="5d816-108">Ügyfél-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="5d816-108">Client authentication</span></span>
<span data-ttu-id="5d816-109">Az Event Hubs biztonsági modell kombinációja alapján [közös hozzáférésű Jogosultságkód (SAS)](../service-bus-messaging/service-bus-sas.md) jogkivonatok és *esemény-közzétevők*.</span><span class="sxs-lookup"><span data-stu-id="5d816-109">The Event Hubs security model is based on a combination of [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) tokens and *event publishers*.</span></span> <span data-ttu-id="5d816-110">Egy esemény-közzétevő határozza meg egy virtuális végpont az eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="5d816-110">An event publisher defines a virtual endpoint for an event hub.</span></span> <span data-ttu-id="5d816-111">A közzétevő csak használható küldhet üzeneteket az eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="5d816-111">The publisher can only be used to send messages to an event hub.</span></span> <span data-ttu-id="5d816-112">Nincs lehetőség a közzétevők érkező üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="5d816-112">It is not possible to receive messages from a publisher.</span></span>

<span data-ttu-id="5d816-113">Az eseményközpontok általában egy publisher ügyfelenként funkcióit használja.</span><span class="sxs-lookup"><span data-stu-id="5d816-113">Typically, an event hub employs one publisher per client.</span></span> <span data-ttu-id="5d816-114">Az eseményközpontok a közzétevők bármelyikét küldött összes üzenet a várólistában levő adott eseményközpont belül.</span><span class="sxs-lookup"><span data-stu-id="5d816-114">All messages that are sent to any of the publishers of an event hub are enqueued within that event hub.</span></span> <span data-ttu-id="5d816-115">Közzétevők részletes hozzáférés-vezérlés és a szabályozás engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5d816-115">Publishers enable fine-grained access control and throttling.</span></span>

<span data-ttu-id="5d816-116">Minden Event Hubs-ügyfél hozzá van rendelve egy egyedi jogkivonatot, amely az ügyfél feltöltődött.</span><span class="sxs-lookup"><span data-stu-id="5d816-116">Each Event Hubs client is assigned a unique token, which is uploaded to the client.</span></span> <span data-ttu-id="5d816-117">A jogkivonatok úgy, hogy mindegyik egyedi token engedélyezi a hozzáférést egy másik egyedi publisher előállítása.</span><span class="sxs-lookup"><span data-stu-id="5d816-117">The tokens are produced such that each unique token grants access to a different unique publisher.</span></span> <span data-ttu-id="5d816-118">Ügyfél, amely rendelkezik egy token csak egy publisher, de nincs más publisher küldhet.</span><span class="sxs-lookup"><span data-stu-id="5d816-118">A client that possesses a token can only send to one publisher, but no other publisher.</span></span> <span data-ttu-id="5d816-119">Több ügyfelek osztják meg ugyanezt a tokent, ha azok a közzétevők osztja ki.</span><span class="sxs-lookup"><span data-stu-id="5d816-119">If multiple clients share the same token, then each of them shares a publisher.</span></span>

<span data-ttu-id="5d816-120">Bár nem javasolt, akkor lehet jogkivonatokkal eseményközpontokba való közvetlen hozzáférést biztosító eszközök ellátására.</span><span class="sxs-lookup"><span data-stu-id="5d816-120">Although not recommended, it is possible to equip devices with tokens that grant direct access to an event hub.</span></span> <span data-ttu-id="5d816-121">Minden olyan eszköz, amely tárolja Ez a token közvetlenül az adott eseményközpont küldhet üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="5d816-121">Any device that holds this token can send messages directly into that event hub.</span></span> <span data-ttu-id="5d816-122">Ilyen eszköz sávszélesség-szabályozás nem függvénye.</span><span class="sxs-lookup"><span data-stu-id="5d816-122">Such a device will not be subject to throttling.</span></span> <span data-ttu-id="5d816-123">Továbbá az eszköz nem lehet feketelistára teszi, az adott eseményközpont küld.</span><span class="sxs-lookup"><span data-stu-id="5d816-123">Furthermore, the device cannot be blacklisted from sending to that event hub.</span></span>

<span data-ttu-id="5d816-124">Összes jogkivonatot egy SAS-kulcs van aláírva.</span><span class="sxs-lookup"><span data-stu-id="5d816-124">All tokens are signed with a SAS key.</span></span> <span data-ttu-id="5d816-125">Általában minden jogkivonatok aláírt ugyanazzal a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="5d816-125">Typically, all tokens are signed with the same key.</span></span> <span data-ttu-id="5d816-126">Az ügyfelek nem ismerik a kulcsot. Ez megakadályozza, hogy más ügyfelekkel gyártási jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="5d816-126">Clients are not aware of the key; this prevents other clients from manufacturing tokens.</span></span>

### <a name="create-the-sas-key"></a><span data-ttu-id="5d816-127">A SAS-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d816-127">Create the SAS key</span></span>

<span data-ttu-id="5d816-128">Az Event Hubs névtér létrehozása, ha a szolgáltatás állít elő, 256 bites SAS-kulcs nevű **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="5d816-128">When creating an Event Hubs namespace, the service generates a 256-bit SAS key named **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="5d816-129">A kulcs biztosít küldése, figyelésére és a névtérhez jogosultságaik kezelését.</span><span class="sxs-lookup"><span data-stu-id="5d816-129">This key grants send, listen, and manage rights to the namespace.</span></span> <span data-ttu-id="5d816-130">További kulcsok is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="5d816-130">You can also create additional keys.</span></span> <span data-ttu-id="5d816-131">Javasoljuk, hogy a egy kulcs, küldjön engedélyeket biztosít az adott event hubs eredményez.</span><span class="sxs-lookup"><span data-stu-id="5d816-131">It is recommended that you produce a key that grants send permissions to the specific event hub.</span></span> <span data-ttu-id="5d816-132">Ez a témakör hátralévő, feltételezzük, hogy ez a kulcs elnevezett **EventHubSendKey**.</span><span class="sxs-lookup"><span data-stu-id="5d816-132">For the remainder of this topic, it is assumed that you named this key **EventHubSendKey**.</span></span>

<span data-ttu-id="5d816-133">Az alábbi példa létrehoz egy csak küldési kulcsot, az eseményközpont létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="5d816-133">The following example creates a send-only key when creating the event hub:</span></span>

```csharp
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create event hub with a SAS rule that enables sending to that event hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a><span data-ttu-id="5d816-134">Rendszer jogkivonatokat hoz létre</span><span class="sxs-lookup"><span data-stu-id="5d816-134">Generate tokens</span></span>

<span data-ttu-id="5d816-135">Az SAS-kulcsot használó tokenek hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="5d816-135">You can generate tokens using the SAS key.</span></span> <span data-ttu-id="5d816-136">Csak egy token ügyfelenként kell eredményez.</span><span class="sxs-lookup"><span data-stu-id="5d816-136">You must produce only one token per client.</span></span> <span data-ttu-id="5d816-137">Jogkivonatok az alábbi módszerrel majd előállított.</span><span class="sxs-lookup"><span data-stu-id="5d816-137">Tokens can then be produced using the following method.</span></span> <span data-ttu-id="5d816-138">Minden jogkivonatok segítségével hozhatók létre a **EventHubSendKey** kulcs.</span><span class="sxs-lookup"><span data-stu-id="5d816-138">All tokens are generated using the **EventHubSendKey** key.</span></span> <span data-ttu-id="5d816-139">A tokenek hozzá van rendelve egy egyedi URI-t.</span><span class="sxs-lookup"><span data-stu-id="5d816-139">Each token is assigned a unique URI.</span></span>

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

<span data-ttu-id="5d816-140">Ez a metódus hívásakor az URI Azonosítót kell megadni: `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="5d816-140">When calling this method, the URI should be specified as `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span></span> <span data-ttu-id="5d816-141">Minden jogkivonatokat URI megegyezik, kivéve a `PUBLISHER_NAME`, amely nem lehet ugyanaz a minden token.</span><span class="sxs-lookup"><span data-stu-id="5d816-141">For all tokens, the URI is identical, with the exception of `PUBLISHER_NAME`, which should be different for each token.</span></span> <span data-ttu-id="5d816-142">Ideális esetben `PUBLISHER_NAME` , amely megkapja a jogkivonatot az ügyfél Azonosítójának jelöli.</span><span class="sxs-lookup"><span data-stu-id="5d816-142">Ideally, `PUBLISHER_NAME` represents the ID of the client that receives that token.</span></span>

<span data-ttu-id="5d816-143">Ez a módszer létrehoz egy jogkivonatot az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="5d816-143">This method generates a token with the following structure:</span></span>

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

<span data-ttu-id="5d816-144">A jogkivonat lejárati ideje másodpercben 1970. január 1. a van megadva.</span><span class="sxs-lookup"><span data-stu-id="5d816-144">The token expiration time is specified in seconds from Jan 1, 1970.</span></span> <span data-ttu-id="5d816-145">A következő egy példa egy jogkivonatot:</span><span class="sxs-lookup"><span data-stu-id="5d816-145">The following is an example of a token:</span></span>

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

<span data-ttu-id="5d816-146">Általában a jogkivonatok élettartama hasonlít, vagy meghaladja az ügyfél élettartamát.</span><span class="sxs-lookup"><span data-stu-id="5d816-146">Typically, the tokens have a lifespan that resembles or exceeds the lifespan of the client.</span></span> <span data-ttu-id="5d816-147">Ha az ügyfél új jogkivonat beszerzése képességgel rendelkezik, egy rövidebb élettartamot rendelkező jogkivonatokat használható.</span><span class="sxs-lookup"><span data-stu-id="5d816-147">If the client has the capability to obtain a new token, tokens with a shorter lifespan can be used.</span></span>

### <a name="sending-data"></a><span data-ttu-id="5d816-148">Adatok küldése</span><span class="sxs-lookup"><span data-stu-id="5d816-148">Sending data</span></span>
<span data-ttu-id="5d816-149">A jogkivonatok létrehozása után, minden ügyfél a saját egyedi jogkivonattal lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="5d816-149">Once the tokens have been created, each client is provisioned with its own unique token.</span></span>

<span data-ttu-id="5d816-150">Az ügyfél eseményközpontnak adatokat küld, azt a küldési kérelem a token címkét.</span><span class="sxs-lookup"><span data-stu-id="5d816-150">When the client sends data into an event hub, it tags its send request with the token.</span></span> <span data-ttu-id="5d816-151">Megakadályozza a lehallgatást, és a token ellophassák támadók, hogy az az ügyfél és az event hubs közötti kommunikációra egy titkosított csatornán keresztül.</span><span class="sxs-lookup"><span data-stu-id="5d816-151">To prevent an attacker from eavesdropping and stealing the token, the communication between the client and the event hub must occur over an encrypted channel.</span></span>

### <a name="blacklisting-clients"></a><span data-ttu-id="5d816-152">Az ügyfelek letiltott</span><span class="sxs-lookup"><span data-stu-id="5d816-152">Blacklisting clients</span></span>
<span data-ttu-id="5d816-153">A jogkivonat támadók ellopják, a támadó megszemélyesíthet-e az ügyfél, amelynek token ellopott.</span><span class="sxs-lookup"><span data-stu-id="5d816-153">If a token is stolen by an attacker, the attacker can impersonate the client whose token has been stolen.</span></span> <span data-ttu-id="5d816-154">Letiltott ügyfél jeleníti meg, hogy az ügyfél használhatatlanná amíg nem kap egy új jogkivonatot, amely egy másik közzétevőt használ.</span><span class="sxs-lookup"><span data-stu-id="5d816-154">Blacklisting a client renders that client unusable until it receives a new token that uses a different publisher.</span></span>

## <a name="authentication-of-back-end-applications"></a><span data-ttu-id="5d816-155">Háttér-alkalmazások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="5d816-155">Authentication of back-end applications</span></span>

<span data-ttu-id="5d816-156">Hitelesítést végezni, amely az adatokat az Event Hubs-ügyfelek által generált háttér-alkalmazások, az Event Hubs egy Service Bus-témakörök használt modell hasonló biztonsági modellt alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="5d816-156">To authenticate back-end applications that consume the data generated by Event Hubs clients, Event Hubs employs a security model that is similar to the model that is used for Service Bus topics.</span></span> <span data-ttu-id="5d816-157">Az Event Hubs fogyasztói csoportot megegyezik egy Service Bus-témakörbe való előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5d816-157">An Event Hubs consumer group is equivalent to a subscription to a Service Bus topic.</span></span> <span data-ttu-id="5d816-158">Egy ügyfél egy fogyasztói csoportot hozhat létre, ha a felhasználói csoport létrehozására vonatkozó kérelem kíséri jogkivonatot, hogy biztosít kezelése jogosultságokkal az event hubs és a névteret, amelyhez az event hubs tartozik.</span><span class="sxs-lookup"><span data-stu-id="5d816-158">A client can create a consumer group if the request to create the consumer group is accompanied by a token that grants manage privileges for the event hub, or for the namespace to which the event hub belongs.</span></span> <span data-ttu-id="5d816-159">Egy ügyfél egy felhasználói csoport származó adatok felhasználásához, ha a fogadási kérelem egy jogkivonatot, amely megadja az adott felhasználói csoportban, az event hubs vagy a névteret, amelyhez az event hubs tartozik fogadási jogosultságot kap.</span><span class="sxs-lookup"><span data-stu-id="5d816-159">A client is allowed to consume data from a consumer group if the receive request is accompanied by a token that grants receive rights on that consumer group, the event hub, or the namespace to which the event hub belongs.</span></span>

<span data-ttu-id="5d816-160">A Service Bus jelenlegi verziója nem támogatja a SAS-szabályokat az egyes előfizetések.</span><span class="sxs-lookup"><span data-stu-id="5d816-160">The current version of Service Bus does not support SAS rules for individual subscriptions.</span></span> <span data-ttu-id="5d816-161">Ugyanez érvényes Event Hubs fogyasztói csoportok.</span><span class="sxs-lookup"><span data-stu-id="5d816-161">The same holds true for Event Hubs consumer groups.</span></span> <span data-ttu-id="5d816-162">SAS-támogatás a jövőben is a szolgáltatások lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="5d816-162">SAS support will be added for both features in the future.</span></span>

<span data-ttu-id="5d816-163">Egyéni fogyasztói csoportok SAS hitelesítési hiányában a SAS-kulcs segítségével biztonságos minden felhasználói csoport egy közös kulccsal.</span><span class="sxs-lookup"><span data-stu-id="5d816-163">In the absence of SAS authentication for individual consumer groups, you can use SAS keys to secure all consumer groups with a common key.</span></span> <span data-ttu-id="5d816-164">Ez a megközelítés lehetővé teszi az alkalmazás bármelyik a fogyasztói csoportok eseményközpontban az adatokat.</span><span class="sxs-lookup"><span data-stu-id="5d816-164">This approach enables an application to consume data from any of the consumer groups of an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d816-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d816-165">Next steps</span></span>
<span data-ttu-id="5d816-166">Az Event Hubs kapcsolatos további információkért látogasson el a következő témaköröket:</span><span class="sxs-lookup"><span data-stu-id="5d816-166">To learn more about Event Hubs, visit the following topics:</span></span>

* <span data-ttu-id="5d816-167">[Event Hubs – áttekintés]</span><span class="sxs-lookup"><span data-stu-id="5d816-167">[Event Hubs overview]</span></span>
* <span data-ttu-id="5d816-168">[Közös hozzáférésű Jogosultságkód áttekintése]</span><span class="sxs-lookup"><span data-stu-id="5d816-168">[Overview of Shared Access Signatures]</span></span>
* <span data-ttu-id="5d816-169">[Az Event Hubsot használó mintaalkalmazások]</span><span class="sxs-lookup"><span data-stu-id="5d816-169">[Sample applications that use Event Hubs]</span></span>

<span data-ttu-id="5d816-170">[Event Hubs – áttekintés]: event-hubs-what-is-event-hubs.md</span><span class="sxs-lookup"><span data-stu-id="5d816-170">[Event Hubs overview]: event-hubs-what-is-event-hubs.md</span></span>
<span data-ttu-id="5d816-171">[Az Event Hubsot használó mintaalkalmazások]: https://github.com/Azure/azure-event-hubs/tree/master/samples</span><span class="sxs-lookup"><span data-stu-id="5d816-171">[Sample applications that use Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples</span></span>
<span data-ttu-id="5d816-172">[Közös hozzáférésű Jogosultságkód áttekintése]: ../service-bus-messaging/service-bus-sas.md</span><span class="sxs-lookup"><span data-stu-id="5d816-172">[Overview of Shared Access Signatures]: ../service-bus-messaging/service-bus-sas.md</span></span>

