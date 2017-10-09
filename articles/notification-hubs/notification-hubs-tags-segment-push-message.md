---
title: "aaaRouting és címke kifejezések"
description: "Ez a témakör ismerteti az Azure notification hubs használatával az Útválasztás és a címke a kifejezéseket."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c2c60500f7469f1cb1a73a5cf63c221a9ad6cbb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="routing-and-tag-expressions"></a><span data-ttu-id="a0299-103">Az Útválasztás és címke kifejezések</span><span class="sxs-lookup"><span data-stu-id="a0299-103">Routing and tag expressions</span></span>
## <a name="overview"></a><span data-ttu-id="a0299-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a0299-104">Overview</span></span>
<span data-ttu-id="a0299-105">Címke kifejezések lehetővé teszik, eszközök, illetve pontosabban regisztrációk adott készleteinek tootarget keresztül a Notification Hubs leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="a0299-105">Tag expressions enable you tootarget specific sets of devices, or more specifically registrations, when sending a push notification through Notification Hubs.</span></span>

## <a name="targeting-specific-registrations"></a><span data-ttu-id="a0299-106">Célcsoport-kezelési adott regisztrációk</span><span class="sxs-lookup"><span data-stu-id="a0299-106">Targeting specific registrations</span></span>
<span data-ttu-id="a0299-107">hello csak úgy tootarget adott értesítést regisztrációk tooassociate címkék velük, majd jelölje ki ezen címkék.</span><span class="sxs-lookup"><span data-stu-id="a0299-107">hello only way tootarget specific notification registrations is tooassociate tags with them, then target those tags.</span></span> <span data-ttu-id="a0299-108">A bemutatott [regisztrációs felügyeleti](notification-hubs-push-notification-registration-management.md), a rendelés tooreceive leküldéses értesítések az alkalmazás rendelkezik tooregister eszköz kezelni egy értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="a0299-108">As discussed in [Registration Management](notification-hubs-push-notification-registration-management.md), in order tooreceive push notifications an app has tooregister a device handle on a notification hub.</span></span> <span data-ttu-id="a0299-109">A regisztrációs létrejön egy értesítési központot, hello alkalmazás háttér küldhet leküldéses értesítések tooit.</span><span class="sxs-lookup"><span data-stu-id="a0299-109">Once a registration is created on a notification hub, hello application backend can send push notifications tooit.</span></span>
<span data-ttu-id="a0299-110">hello alkalmazás háttér hello regisztrációk tootarget egy adott értesítéssel választhatja ki a következő módokon hello:</span><span class="sxs-lookup"><span data-stu-id="a0299-110">hello application backend can choose hello registrations tootarget with a specific notification in hello following ways:</span></span>

1. <span data-ttu-id="a0299-111">**Szórási**: hello értesítési központban az összes regisztrációk hello értesítést kapni.</span><span class="sxs-lookup"><span data-stu-id="a0299-111">**Broadcast**: all registrations in hello notification hub receive hello notification.</span></span>
2. <span data-ttu-id="a0299-112">**Címke**: hello megadott tartalmazó összes regisztrációk címke hello értesítést kapni.</span><span class="sxs-lookup"><span data-stu-id="a0299-112">**Tag**: all registrations that contain hello specified tag receive hello notification.</span></span>
3. <span data-ttu-id="a0299-113">**Címke kifejezés**: összes regisztrációját, amelynek set címkék megfelelő hello megadott kifejezés hello értesítést kapni.</span><span class="sxs-lookup"><span data-stu-id="a0299-113">**Tag expression**: all registrations whose set of tags match hello specified expression receive hello notification.</span></span>

## <a name="tags"></a><span data-ttu-id="a0299-114">Címkék</span><span class="sxs-lookup"><span data-stu-id="a0299-114">Tags</span></span>
<span data-ttu-id="a0299-115">Egy címke bármilyen karakterlánc too120 karaktereket tartalmazó be alfanumerikus karakterekből állhat, és a hello a következő nem alfanumerikus karaktereket: "_", "@", '#', '. ',': ","-".</span><span class="sxs-lookup"><span data-stu-id="a0299-115">A tag can be any string, up too120 characters, containing alphanumeric and hello following non-alphanumeric characters: ‘_’, ‘@’, ‘#’, ‘.’, ‘:’, ‘-’.</span></span> <span data-ttu-id="a0299-116">hello következő példa bemutatja egy alkalmazás, amelyből adott zene csoportok bejelentési értesítést kaphat.</span><span class="sxs-lookup"><span data-stu-id="a0299-116">hello following example shows an application from which you can receive toast notifications about specific music groups.</span></span> <span data-ttu-id="a0299-117">Ebben a forgatókönyvben egy egyszerű módon tooroute értesítések toolabel regisztrációk hello különböző szalagokhoz, mint a következő képen hello jelölő címkékkel.</span><span class="sxs-lookup"><span data-stu-id="a0299-117">In this scenario, a simple way tooroute notifications is toolabel registrations with tags that represent hello different bands, as in hello following picture.</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

<span data-ttu-id="a0299-118">A képen látható üdvözlőüzenetére címkézett **Beatles** eléri csak hello tablet hello címkével ellátott regisztrált **Beatles**.</span><span class="sxs-lookup"><span data-stu-id="a0299-118">In this picture, hello message tagged **Beatles** reaches only hello tablet that registered with hello tag **Beatles**.</span></span>

<span data-ttu-id="a0299-119">Címkék regisztrációinak létrehozásával kapcsolatos további információkért lásd: [regisztrációs felügyeleti](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="a0299-119">For more information about creating registrations for tags, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

<span data-ttu-id="a0299-120">Értesítések tootags hello segítségével hello módszerek értesítések küldése küldhet `Microsoft.Azure.NotificationHubs.NotificationHubClient` hello osztály [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK.</span><span class="sxs-lookup"><span data-stu-id="a0299-120">You can send notifications tootags using hello send notifications methods of hello `Microsoft.Azure.NotificationHubs.NotificationHubClient` class in hello [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK.</span></span> <span data-ttu-id="a0299-121">Node.js használja, vagy hello leküldéses értesítések REST API-kat is.</span><span class="sxs-lookup"><span data-stu-id="a0299-121">You can also use Node.js, or hello Push Notifications REST APIs.</span></span>  <span data-ttu-id="a0299-122">Íme egy példa a hello SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="a0299-122">Here's an example using hello SDK.</span></span>

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




<span data-ttu-id="a0299-123">Címkék előzetes kiosztása toobe nem rendelkezik, és toomultiple alkalmazásspecifikus fogalmak hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="a0299-123">Tags do not have toobe pre-provisioned and can refer toomultiple app-specific concepts.</span></span> <span data-ttu-id="a0299-124">Felhasználók például alkalmazás például sávok fűzni, és a kívánt tooreceive toasts, nem csak a kedvenc sávok hello megjegyzéseket, de is az ismerősei, függetlenül attól, amelyen megjegyzések hello sávon az összes megjegyzést.</span><span class="sxs-lookup"><span data-stu-id="a0299-124">For example, users of this example application can comment on bands and want tooreceive toasts, not only for hello comments on their favorite bands, but also for all comments from their friends, regardless of hello band on which they are commenting.</span></span> <span data-ttu-id="a0299-125">a következő kép hello ebben a forgatókönyvben példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="a0299-125">hello following picture shows an example of this scenario:</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

<span data-ttu-id="a0299-126">A képen látható Alice olyan hello Beatles frissítései iránt érdeklődik, és Bob hello Wailers frissítései.</span><span class="sxs-lookup"><span data-stu-id="a0299-126">In this picture, Alice is interested in updates for hello Beatles, and Bob is interested in updates for hello Wailers.</span></span> <span data-ttu-id="a0299-127">Bob is Charlie tartozó megjegyzések iránt érdeklődik, és Charlie hello Wailers érdeklődik.</span><span class="sxs-lookup"><span data-stu-id="a0299-127">Bob is also interested in Charlie’s comments, and Charlie is in interested in hello Wailers.</span></span> <span data-ttu-id="a0299-128">Hello Beatles Charlie tartozó megjegyzés értesítést elküldésekor Alice és a Bob megkapni.</span><span class="sxs-lookup"><span data-stu-id="a0299-128">When a notification is sent for Charlie’s comment on hello Beatles, both Alice and Bob receive it.</span></span>

<span data-ttu-id="a0299-129">Több aggályokat címkéket (például "band_Beatles" vagy "follows_Charlie") is kódolására, amíg a címke található egyszerű karakterláncok és a nem értékkel rendelkező tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="a0299-129">While you can encode multiple concerns in tags (for example, “band_Beatles” or “follows_Charlie”), tags are simple strings and not properties with values.</span></span> <span data-ttu-id="a0299-130">Regisztráció csak a hello megléte vagy hiánya, egy adott kód egyezik.</span><span class="sxs-lookup"><span data-stu-id="a0299-130">A registration is matched only on hello presence or absence of a specific tag.</span></span>

<span data-ttu-id="a0299-131">A teljes részletes útmutatót a hogyan toouse címkék toointerest küldése, lásd: [Megtörje hírek](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).</span><span class="sxs-lookup"><span data-stu-id="a0299-131">For a full step-by-step tutorial on how toouse tags for sending toointerest groups, see [Breaking News](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).</span></span>

## <a name="using-tags-tootarget-users"></a><span data-ttu-id="a0299-132">Címkék tootarget felhasználók használatával</span><span class="sxs-lookup"><span data-stu-id="a0299-132">Using tags tootarget users</span></span>
<span data-ttu-id="a0299-133">Egy másik módja toouse címkéket tooidentify, ha egy adott felhasználó összes hello eszköz van.</span><span class="sxs-lookup"><span data-stu-id="a0299-133">Another way toouse tags is tooidentify all hello devices of a particular user.</span></span> <span data-ttu-id="a0299-134">Regisztráció prioritáscímkékkel való ellátását, amely tartalmazza a felhasználói azonosítóval, mint hello kép a következő címke:</span><span class="sxs-lookup"><span data-stu-id="a0299-134">Registrations can be tagged with a tag that contains a user id, as in hello following picture:</span></span>

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

<span data-ttu-id="a0299-135">A képen látható hello címkézett üzenet uid:Alice eléri összes regisztrációk címkézett uid:Alice; ezért Ágnes eszközeiről.</span><span class="sxs-lookup"><span data-stu-id="a0299-135">In this picture, hello message tagged uid:Alice reaches all registrations tagged uid:Alice; hence, all of Alice’s devices.</span></span>

## <a name="tag-expressions"></a><span data-ttu-id="a0299-136">Címke kifejezések</span><span class="sxs-lookup"><span data-stu-id="a0299-136">Tag expressions</span></span>
<span data-ttu-id="a0299-137">Nincsenek olyan esetekben, ahol egy értesítés tootarget regisztrációk csoportja, amely nem egy egyetlen címkét, hanem egy logikai kifejezés címkékre azonosítja.</span><span class="sxs-lookup"><span data-stu-id="a0299-137">There are cases in which a notification has tootarget a set of registrations that is identified not by a single tag, but by a Boolean expression on tags.</span></span>

<span data-ttu-id="a0299-138">Vegye figyelembe a sport alkalmazást, amely egy emlékeztető tooeveryone a Boston kapcsolatos játék hello piros Sox és Cardinals között.</span><span class="sxs-lookup"><span data-stu-id="a0299-138">Consider a sports application that sends a reminder tooeveryone in Boston about a game between hello Red Sox and Cardinals.</span></span> <span data-ttu-id="a0299-139">Ha hello ügyfélalkalmazás regisztrálja a csoportok és a hely érdeklődik kapcsolatos címkék, hello értesítési Boston, akik hello piros Sox vagy hello Cardinals a célzott tooeveryone kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a0299-139">If hello client app registers tags about interest in teams and location, then hello notification should be targeted tooeveryone in Boston who is interested in either hello Red Sox or hello Cardinals.</span></span> <span data-ttu-id="a0299-140">Ez a feltétel a következő logikai kifejezés hello lehet megadni:</span><span class="sxs-lookup"><span data-stu-id="a0299-140">This condition can be expressed with hello following Boolean expression:</span></span>

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

<span data-ttu-id="a0299-141">Címke kifejezések például tartalmazhat összes logikai operátorok, és (& &), vagy (|), és nem (!).</span><span class="sxs-lookup"><span data-stu-id="a0299-141">Tag expressions can contain all Boolean operators, such as AND (&&), OR (||), and NOT (!).</span></span> <span data-ttu-id="a0299-142">Kerek zárójeleket tartalmazhatnak is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="a0299-142">They can also contain parentheses.</span></span> <span data-ttu-id="a0299-143">Címke kifejezések szerepelnek a korlátozott too20 címkék, ha tartalmaznak csak ORs; Ellenkező esetben a korlátozott too6 címkék.</span><span class="sxs-lookup"><span data-stu-id="a0299-143">Tag expressions are limited too20 tags if they contain only ORs; otherwise they are limited too6 tags.</span></span>

<span data-ttu-id="a0299-144">Íme egy példa az értesítések küldésével címke kifejezésekkel hello SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="a0299-144">Here's an example for sending notifications with tag expressions using hello SDK.</span></span>

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)";    

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on hello Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on hello Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
