---
title: "Leküldéses értesítések tooiOS alkalmazást az Azure Mobile Apps aaaAdd"
description: "Ismerje meg, hogyan toouse Azure Mobile Apps toosend leküldéses értesítések tooyour iOS-alkalmazás."
services: app-service\mobile
documentationcenter: ios
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/10/2016
ms.author: glenga
ms.openlocfilehash: 11588c56bc8987a71257dbad4fcdebf1b3209b1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-ios-app"></a><span data-ttu-id="958af-103">Leküldéses értesítések tooyour iOS-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="958af-103">Add Push Notifications tooyour iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="958af-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="958af-104">Overview</span></span>
<span data-ttu-id="958af-105">Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [iOS quick start] projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="958af-105">In this tutorial, you add push notifications toohello [iOS quick start] project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="958af-106">Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában.</span><span class="sxs-lookup"><span data-stu-id="958af-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="958af-107">Lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="958af-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

<span data-ttu-id="958af-108">Hello [iOS-szimulátorban nem támogatja a leküldéses értesítések](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="958af-108">hello [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span> <span data-ttu-id="958af-109">Egy fizikai iOS-eszközön van szüksége, és egy [Apple Fejlesztőprogrambeli tagság](https://developer.apple.com/programs/ios/).</span><span class="sxs-lookup"><span data-stu-id="958af-109">You need a physical iOS device and an [Apple Developer Program membership](https://developer.apple.com/programs/ios/).</span></span>

## <span data-ttu-id="958af-110"><a name="configure-hub"></a>Értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="958af-110"><a name="configure-hub"></a>Configure Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="958af-111"><a id="register"></a>Regisztrálja az alkalmazást a leküldéses értesítésekre</span><span class="sxs-lookup"><span data-stu-id="958af-111"><a id="register"></a>Register app for push notifications</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-toosend-push-notifications"></a><span data-ttu-id="958af-112">Az Azure toosend leküldéses értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="958af-112">Configure Azure toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <span data-ttu-id="958af-113"><a id="update-server"></a>Frissítési háttér toosend leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="958af-113"><a id="update-server"></a>Update backend toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <span data-ttu-id="958af-114"><a id="add-push"></a>Leküldéses értesítések tooapp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="958af-114"><a id="add-push"></a>Add push notifications tooapp</span></span>
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <span data-ttu-id="958af-115"><a id="test"></a>Teszt leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="958af-115"><a id="test"></a>Test push notifications</span></span>
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <span data-ttu-id="958af-116"><a id="more"></a>Több</span><span class="sxs-lookup"><span data-stu-id="958af-116"><a id="more"></a>More</span></span>
* <span data-ttu-id="958af-117">Sablonok segítenek rugalmasságot toosend platformok közötti leküldéses értesítések és honosított leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="958af-117">Templates give you flexibility toosend cross-platform pushes and localized pushes.</span></span> <span data-ttu-id="958af-118">[Hogyan tooUse iOS ügyféloldali kódtára a Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) bemutatja, hogyan tooregister sablonok.</span><span class="sxs-lookup"><span data-stu-id="958af-118">[How tooUse iOS Client Library for Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) shows you how tooregister templates.</span></span>

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS quick start]: app-service-mobile-ios-get-started.md
