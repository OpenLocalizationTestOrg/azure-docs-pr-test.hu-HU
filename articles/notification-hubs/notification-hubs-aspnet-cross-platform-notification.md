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
# <a name="send-cross-platform-notifications-toousers-with-notification-hubs"></a><span data-ttu-id="386ce-103">A Notification hubs használatával platformfüggetlen bejelentéseket toousers küldése</span><span class="sxs-lookup"><span data-stu-id="386ce-103">Send cross-platform notifications toousers with Notification Hubs</span></span>
<span data-ttu-id="386ce-104">Hello előző oktatóprogram [értesítse a felhasználókat a Notification hubs használatával], megtudta, hogyan toopush értesítések tooall eszközök egy adott hitelesített felhasználó regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="386ce-104">In hello previous tutorial [Notify users with Notification Hubs], you learned how toopush notifications tooall devices registered by a specific authenticated user.</span></span> <span data-ttu-id="386ce-105">Több kérést, hogy az oktatóanyag szükséges toosend egy értesítési tooeach támogatott ügyfélplatform voltak.</span><span class="sxs-lookup"><span data-stu-id="386ce-105">In that tutorial, multiple requests were required toosend a notification tooeach supported client platform.</span></span> <span data-ttu-id="386ce-106">A Notification Hubs támogatja a sablonok, amelyek lehetővé teszik, hogy adja meg, hogyan szeretne rendelni egy adott eszköz a tooreceive értesítések.</span><span class="sxs-lookup"><span data-stu-id="386ce-106">Notification Hubs supports templates, which let you specify how a specific device wants tooreceive notifications.</span></span> <span data-ttu-id="386ce-107">Ez egyszerűbbé teszi a platformok közötti értesítések küldése.</span><span class="sxs-lookup"><span data-stu-id="386ce-107">This simplifies sending cross-platform notifications.</span></span> <span data-ttu-id="386ce-108">Ez a témakör bemutatja, hogyan sablonok toosend egyetlen kérésben, amelynek célpontja a platformfüggetlen platform-független értesítést tootake előnyeit.</span><span class="sxs-lookup"><span data-stu-id="386ce-108">This topic demonstrates how tootake advantage of templates toosend, in a single request, a platform-agnostic notification that targets all platforms.</span></span> <span data-ttu-id="386ce-109">Részletesebb információ a sablonok, lásd: [Azure Notification Hubs – áttekintés][Templates].</span><span class="sxs-lookup"><span data-stu-id="386ce-109">For more detailed information about templates, see [Azure Notification Hubs Overview][Templates].</span></span>
> [!IMPORTANT]
> <span data-ttu-id="386ce-110">Windows Phone-projektek 8.1 és korábbi verziók nem támogatottak a Visual Studio 2017 használatával.</span><span class="sxs-lookup"><span data-stu-id="386ce-110">Windows Phone projects 8.1 and earlier are not supported using Visual Studio 2017.</span></span> <span data-ttu-id="386ce-111">További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="386ce-111">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

> [!NOTE]
> <span data-ttu-id="386ce-112">A Notification Hubs lehetővé teszi, hogy egy eszköz tooregister több sablon is van hello azonos címkével.</span><span class="sxs-lookup"><span data-stu-id="386ce-112">Notification Hubs allows a device tooregister multiple templates with hello same tag.</span></span> <span data-ttu-id="386ce-113">Ebben az esetben egy bejövő célzó, hogy a címke eredményez több értesítés is érkezett kézbesíteni az üzenetet, toohello eszköz, minden sablon egy.</span><span class="sxs-lookup"><span data-stu-id="386ce-113">In this case, an incoming message targeting that tag results in multiple notifications delivered toohello device, one for each template.</span></span> <span data-ttu-id="386ce-114">Ez lehetővé teszi, hogy toodisplay hello ugyanaz az üzenet több vizuális értesítések, például egy jelvény és egy Windows Áruházbeli alkalmazásban egy bejelentési értesítést is.</span><span class="sxs-lookup"><span data-stu-id="386ce-114">This enables you toodisplay hello same message in multiple visual notifications, such as both as a badge and as a toast notification in a Windows Store app.</span></span>
> 
> 

<span data-ttu-id="386ce-115">Hajtsa végre a következő lépéseket toosend platformfüggetlen értesítések sablonokkal hello:</span><span class="sxs-lookup"><span data-stu-id="386ce-115">Complete hello following steps toosend cross-platform notifications using templates:</span></span>

1. <span data-ttu-id="386ce-116">A Visual Studio Solution Explorer hello, bontsa ki a hello **tartományvezérlők** mappát, majd nyissa meg hello RegisterController.cs fájlt.</span><span class="sxs-lookup"><span data-stu-id="386ce-116">In hello Solution Explorer in Visual Studio, expand hello **Controllers** folder, then open hello RegisterController.cs file.</span></span>
2. <span data-ttu-id="386ce-117">Hello kódblokkot található hello **Put** módszerrel, amely létrehoz egy új regisztrációs cserélje le a hello `switch` tartalom a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="386ce-117">Locate hello block of code in hello **Put** method that creates a new registration replace hello `switch` content with hello following code:</span></span>
   
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
   
    <span data-ttu-id="386ce-118">Ez a kód meghívja hello platform-specifikus metódus toocreate helyett egy natív regisztrációjának sablon regisztráció.</span><span class="sxs-lookup"><span data-stu-id="386ce-118">This code calls hello platform-specific method toocreate a template registration instead of a native registration.</span></span> <span data-ttu-id="386ce-119">Létező regisztráció nem kell módosítani, mert a sablon regisztrációk natív regisztrációk származik.</span><span class="sxs-lookup"><span data-stu-id="386ce-119">Existing registrations need not be modified because template registrations derive from native registrations.</span></span>
3. <span data-ttu-id="386ce-120">A hello **értesítések** tartományvezérlő, a név felülírandó hello **sendNotification** hello kód a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="386ce-120">In hello **Notifications** controller, replace hello **sendNotification** method with hello following code:</span></span>
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    <span data-ttu-id="386ce-121">Ez a kód értesítést küld tooall platformok: hello azonos időben, és anélkül, hogy toospecify a natív hasznos adatok között.</span><span class="sxs-lookup"><span data-stu-id="386ce-121">This code sends a notification tooall platforms at hello same time and without having toospecify a native payload.</span></span> <span data-ttu-id="386ce-122">A Notification Hubs alapszik, és kézbesíti hello megfelelő hasznos tooevery eszköz a megadott hello *címke* érték, a regisztrált hello sablonok.</span><span class="sxs-lookup"><span data-stu-id="386ce-122">Notification Hubs builds and delivers hello correct payload tooevery device with hello provided *tag* value, as specified in hello registered templates.</span></span>
4. <span data-ttu-id="386ce-123">Újra tegye közzé a WebApi háttérrendszerből projekthez.</span><span class="sxs-lookup"><span data-stu-id="386ce-123">Re-publish your WebApi back-end project.</span></span>
5. <span data-ttu-id="386ce-124">Futtassa újra a hello ügyfélalkalmazás, és győződjön meg arról, hogy a regisztráció sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="386ce-124">Run hello client app again and verify that registration succeeds.</span></span>
6. <span data-ttu-id="386ce-125">(Választható) Hello ügyfél app tooa második eszköz üzembe helyezése, majd futtassa a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="386ce-125">(Optional) Deploy hello client app tooa second device, then run hello app.</span></span>
   
    <span data-ttu-id="386ce-126">Vegye figyelembe, hogy minden egyes eszközön megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="386ce-126">Note that a notification is displayed on each device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="386ce-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="386ce-127">Next Steps</span></span>
<span data-ttu-id="386ce-128">Most, hogy ez az oktatóanyag befejezése többet szeretne tudni a Notification Hubs és a sablonok a következő témakörökben talál:</span><span class="sxs-lookup"><span data-stu-id="386ce-128">Now that you have completed this tutorial, find out more about Notification Hubs and templates in these topics:</span></span>

* <span data-ttu-id="386ce-129">**[Használja a Notification Hubs toosend legfrissebb hírek]**</span><span class="sxs-lookup"><span data-stu-id="386ce-129">**[Use Notification Hubs toosend breaking news]**</span></span> <br/><span data-ttu-id="386ce-130">Azt mutatja be egy másik forgatókönyv a sablonok használatával</span><span class="sxs-lookup"><span data-stu-id="386ce-130">Demonstrates another scenario for using templates</span></span>
* <span data-ttu-id="386ce-131">**[Az Azure Notification Hubs – áttekintés][Templates]**</span><span class="sxs-lookup"><span data-stu-id="386ce-131">**[Azure Notification Hubs Overview][Templates]**</span></span><br/><span data-ttu-id="386ce-132">Összefoglaló téma részletesebb információkat a sablonok.</span><span class="sxs-lookup"><span data-stu-id="386ce-132">Overview topic has more detailed information on templates.</span></span>

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
