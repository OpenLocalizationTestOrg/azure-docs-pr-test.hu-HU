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
# <a name="routing-and-tag-expressions"></a>Az Útválasztás és címke kifejezések
## <a name="overview"></a>Áttekintés
Címke kifejezések lehetővé teszik, eszközök, illetve pontosabban regisztrációk adott készleteinek tootarget keresztül a Notification Hubs leküldéses értesítés küldéséhez.

## <a name="targeting-specific-registrations"></a>Célcsoport-kezelési adott regisztrációk
hello csak úgy tootarget adott értesítést regisztrációk tooassociate címkék velük, majd jelölje ki ezen címkék. A bemutatott [regisztrációs felügyeleti](notification-hubs-push-notification-registration-management.md), a rendelés tooreceive leküldéses értesítések az alkalmazás rendelkezik tooregister eszköz kezelni egy értesítési központot. A regisztrációs létrejön egy értesítési központot, hello alkalmazás háttér küldhet leküldéses értesítések tooit.
hello alkalmazás háttér hello regisztrációk tootarget egy adott értesítéssel választhatja ki a következő módokon hello:

1. **Szórási**: hello értesítési központban az összes regisztrációk hello értesítést kapni.
2. **Címke**: hello megadott tartalmazó összes regisztrációk címke hello értesítést kapni.
3. **Címke kifejezés**: összes regisztrációját, amelynek set címkék megfelelő hello megadott kifejezés hello értesítést kapni.

## <a name="tags"></a>Címkék
Egy címke bármilyen karakterlánc too120 karaktereket tartalmazó be alfanumerikus karakterekből állhat, és a hello a következő nem alfanumerikus karaktereket: "_", "@", '#', '. ',': ","-". hello következő példa bemutatja egy alkalmazás, amelyből adott zene csoportok bejelentési értesítést kaphat. Ebben a forgatókönyvben egy egyszerű módon tooroute értesítések toolabel regisztrációk hello különböző szalagokhoz, mint a következő képen hello jelölő címkékkel.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

A képen látható üdvözlőüzenetére címkézett **Beatles** eléri csak hello tablet hello címkével ellátott regisztrált **Beatles**.

Címkék regisztrációinak létrehozásával kapcsolatos további információkért lásd: [regisztrációs felügyeleti](notification-hubs-push-notification-registration-management.md).

Értesítések tootags hello segítségével hello módszerek értesítések küldése küldhet `Microsoft.Azure.NotificationHubs.NotificationHubClient` hello osztály [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Node.js használja, vagy hello leküldéses értesítések REST API-kat is.  Íme egy példa a hello SDK használatával.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Címkék előzetes kiosztása toobe nem rendelkezik, és toomultiple alkalmazásspecifikus fogalmak hivatkozhat. Felhasználók például alkalmazás például sávok fűzni, és a kívánt tooreceive toasts, nem csak a kedvenc sávok hello megjegyzéseket, de is az ismerősei, függetlenül attól, amelyen megjegyzések hello sávon az összes megjegyzést. a következő kép hello ebben a forgatókönyvben példáját mutatja be:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

A képen látható Alice olyan hello Beatles frissítései iránt érdeklődik, és Bob hello Wailers frissítései. Bob is Charlie tartozó megjegyzések iránt érdeklődik, és Charlie hello Wailers érdeklődik. Hello Beatles Charlie tartozó megjegyzés értesítést elküldésekor Alice és a Bob megkapni.

Több aggályokat címkéket (például "band_Beatles" vagy "follows_Charlie") is kódolására, amíg a címke található egyszerű karakterláncok és a nem értékkel rendelkező tulajdonságok. Regisztráció csak a hello megléte vagy hiánya, egy adott kód egyezik.

A teljes részletes útmutatót a hogyan toouse címkék toointerest küldése, lásd: [Megtörje hírek](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

## <a name="using-tags-tootarget-users"></a>Címkék tootarget felhasználók használatával
Egy másik módja toouse címkéket tooidentify, ha egy adott felhasználó összes hello eszköz van. Regisztráció prioritáscímkékkel való ellátását, amely tartalmazza a felhasználói azonosítóval, mint hello kép a következő címke:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

A képen látható hello címkézett üzenet uid:Alice eléri összes regisztrációk címkézett uid:Alice; ezért Ágnes eszközeiről.

## <a name="tag-expressions"></a>Címke kifejezések
Nincsenek olyan esetekben, ahol egy értesítés tootarget regisztrációk csoportja, amely nem egy egyetlen címkét, hanem egy logikai kifejezés címkékre azonosítja.

Vegye figyelembe a sport alkalmazást, amely egy emlékeztető tooeveryone a Boston kapcsolatos játék hello piros Sox és Cardinals között. Ha hello ügyfélalkalmazás regisztrálja a csoportok és a hely érdeklődik kapcsolatos címkék, hello értesítési Boston, akik hello piros Sox vagy hello Cardinals a célzott tooeveryone kell lennie. Ez a feltétel a következő logikai kifejezés hello lehet megadni:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Címke kifejezések például tartalmazhat összes logikai operátorok, és (& &), vagy (|), és nem (!). Kerek zárójeleket tartalmazhatnak is tartalmazhat. Címke kifejezések szerepelnek a korlátozott too20 címkék, ha tartalmaznak csak ORs; Ellenkező esetben a korlátozott too6 címkék.

Íme egy példa az értesítések küldésével címke kifejezésekkel hello SDK használatával.

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
