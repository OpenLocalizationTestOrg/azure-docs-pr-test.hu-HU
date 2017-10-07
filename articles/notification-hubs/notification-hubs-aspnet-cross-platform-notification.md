---
title: "aaaSend platformfüggetlen bejelentéseket toousers a Notification Hubs (ASP.NET)"
description: "Megtudhatja, hogyan toouse Notification Hubs sablonok toosend, egyetlen kérésben, amelynek célpontja a platformfüggetlen platform-független értesítést."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: f105b871b809e739dd5c05ea819ad135e842ebb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-cross-platform-notifications-toousers-with-notification-hubs"></a>A Notification hubs használatával platformfüggetlen bejelentéseket toousers küldése
Hello előző oktatóprogram [értesítse a felhasználókat a Notification hubs használatával], megtudta, hogyan toopush értesítések tooall eszközök egy adott hitelesített felhasználó regisztrálja. Több kérést, hogy az oktatóanyag szükséges toosend egy értesítési tooeach támogatott ügyfélplatform voltak. A Notification Hubs támogatja a sablonok, amelyek lehetővé teszik, hogy adja meg, hogyan szeretne rendelni egy adott eszköz a tooreceive értesítések. Ez egyszerűbbé teszi a platformok közötti értesítések küldése. Ez a témakör bemutatja, hogyan sablonok toosend egyetlen kérésben, amelynek célpontja a platformfüggetlen platform-független értesítést tootake előnyeit. Részletesebb információ a sablonok, lásd: [Azure Notification Hubs – áttekintés][Templates].
> [!IMPORTANT]
> Windows Phone-projektek 8.1 és korábbi verziók nem támogatottak a Visual Studio 2017 használatával. További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> A Notification Hubs lehetővé teszi, hogy egy eszköz tooregister több sablon is van hello azonos címkével. Ebben az esetben egy bejövő célzó, hogy a címke eredményez több értesítés is érkezett kézbesíteni az üzenetet, toohello eszköz, minden sablon egy. Ez lehetővé teszi, hogy toodisplay hello ugyanaz az üzenet több vizuális értesítések, például egy jelvény és egy Windows Áruházbeli alkalmazásban egy bejelentési értesítést is.
> 
> 

Hajtsa végre a következő lépéseket toosend platformfüggetlen értesítések sablonokkal hello:

1. A Visual Studio Solution Explorer hello, bontsa ki a hello **tartományvezérlők** mappát, majd nyissa meg hello RegisterController.cs fájlt.
2. Hello kódblokkot található hello **Put** módszerrel, amely létrehoz egy új regisztrációs cserélje le a hello `switch` tartalom a következő kód hello:
   
        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }
   
    Ez a kód meghívja hello platform-specifikus metódus toocreate helyett egy natív regisztrációjának sablon regisztráció. Létező regisztráció nem kell módosítani, mert a sablon regisztrációk natív regisztrációk származik.
3. A hello **értesítések** tartományvezérlő, a név felülírandó hello **sendNotification** hello kód a következő metódust:
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    Ez a kód értesítést küld tooall platformok: hello azonos időben, és anélkül, hogy toospecify a natív hasznos adatok között. A Notification Hubs alapszik, és kézbesíti hello megfelelő hasznos tooevery eszköz a megadott hello *címke* érték, a regisztrált hello sablonok.
4. Újra tegye közzé a WebApi háttérrendszerből projekthez.
5. Futtassa újra a hello ügyfélalkalmazás, és győződjön meg arról, hogy a regisztráció sikeres lesz.
6. (Választható) Hello ügyfél app tooa második eszköz üzembe helyezése, majd futtassa a hello alkalmazást.
   
    Vegye figyelembe, hogy minden egyes eszközön megjelenik egy értesítés.

## <a name="next-steps"></a>Következő lépések
Most, hogy ez az oktatóanyag befejezése többet szeretne tudni a Notification Hubs és a sablonok a következő témakörökben talál:

* **[Használja a Notification Hubs toosend legfrissebb hírek]** <br/>Azt mutatja be egy másik forgatókönyv a sablonok használatával
* **[Az Azure Notification Hubs – áttekintés][Templates]**<br/>Összefoglaló téma részletesebb információkat a sablonok.

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push toousers ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push toousers Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Használja a Notification Hubs toosend legfrissebb hírek]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[értesítse a felhasználókat a Notification hubs használatával]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How toofor Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
