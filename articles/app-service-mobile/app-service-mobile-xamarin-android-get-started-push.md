---
title: "Leküldéses értesítések hozzáadása Xamarin.Android-alkalmazáshoz |} Microsoft Docs"
description: "Azure App Service és az Azure Notification Hubs használata leküldéses értesítések küldésére a Xamarin.Android-alkalmazás"
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: c3757d56fb1792092710740dc5ffbd64f18730cf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a><span data-ttu-id="b0333-103">Leküldéses értesítések hozzáadása Xamarin.Android-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="b0333-103">Add push notifications to your Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="b0333-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b0333-104">Overview</span></span>
<span data-ttu-id="b0333-105">Ebben az oktatóanyagban leküldéses értesítések hozzáadása a [Xamarin.Android gyors üzembe helyezési](app-service-mobile-windows-store-dotnet-get-started.md) projektre, hogy egy leküldéses értesítést küld az eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="b0333-105">In this tutorial, you add push notifications to the [Xamarin.Android quick start](app-service-mobile-windows-store-dotnet-get-started.md) project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="b0333-106">Ha nem használja a letöltött gyors üzembe helyezési kiszolgálóprojektet, szüksége lesz a leküldéses értesítési kiterjesztési csomagot.</span><span class="sxs-lookup"><span data-stu-id="b0333-106">If you do not use the downloaded quick start server project, you will need the push notification extension package.</span></span> <span data-ttu-id="b0333-107">Lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="b0333-107">See [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0333-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b0333-108">Prerequisites</span></span>
<span data-ttu-id="b0333-109">Az oktatóanyaghoz az alábbiakra lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="b0333-109">This tutorial requires the following:</span></span>

* <span data-ttu-id="b0333-110">Aktív Google-fiók.</span><span class="sxs-lookup"><span data-stu-id="b0333-110">An active Google account.</span></span> <span data-ttu-id="b0333-111">A Google-fiókja, regisztrálhat [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span><span class="sxs-lookup"><span data-stu-id="b0333-111">You can sign up for a Google account at [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>
* <span data-ttu-id="b0333-112">[A Google Cloud Messaging Client összetevő](http://components.xamarin.com/view/GCMClient/).</span><span class="sxs-lookup"><span data-stu-id="b0333-112">[Google Cloud Messaging Client Component](http://components.xamarin.com/view/GCMClient/).</span></span>

## <span data-ttu-id="b0333-113"><a name="configure-hub"></a>Egy értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b0333-113"><a name="configure-hub"></a>Configure a Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="b0333-114"><a id="register"></a>Engedélyezze a Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="b0333-114"><a id="register"></a>Enable Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a><span data-ttu-id="b0333-115">Leküldéses kérelmek küldésére Azure konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b0333-115">Configure Azure to send push requests</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <span data-ttu-id="b0333-116"><a id="update-server"></a>Frissítés a kiszolgáló projekt leküldéses értesítések küldéséhez</span><span class="sxs-lookup"><span data-stu-id="b0333-116"><a id="update-server"></a>Update the server project to send push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="b0333-117"><a id="configure-app"></a>Az ügyfél-projektjét, amely a leküldéses értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b0333-117"><a id="configure-app"></a>Configure the client project for push notifications</span></span>
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <span data-ttu-id="b0333-118"><a id="add-push"></a>Leküldéses értesítések kód hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="b0333-118"><a id="add-push"></a>Add push notifications code to your app</span></span>
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <span data-ttu-id="b0333-119"><a name="test"></a>Teszt leküldéses értesítések az alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="b0333-119"><a name="test"></a>Test push notifications in your app</span></span>
<span data-ttu-id="b0333-120">Az alkalmazást az emulátorban a virtuális eszköz segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="b0333-120">You can test the app by using a virtual device in the emulator.</span></span> <span data-ttu-id="b0333-121">Nincsenek az emulátor futtatásához szükséges további konfigurációs lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b0333-121">There are additional configuration steps required when running on an emulator.</span></span>

1. <span data-ttu-id="b0333-122">Győződjön meg arról, üzembe vagy hibakeresés egy virtuális eszközön, amely rendelkezik a Google API-k, célként beállítva, az Android virtuális eszközt (AVD) kezelő az alábbiak szerint vannak.</span><span class="sxs-lookup"><span data-stu-id="b0333-122">Make sure that you are deploying to or debugging on a virtual device that has Google APIs set as the target, as shown below in the Android Virtual Device (AVD) manager.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. <span data-ttu-id="b0333-123">A Google-fiók kattintva vegyen fel új Android-eszköz **alkalmazások** > **beállítások** > **fiók hozzáadása**, majd kövesse a megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="b0333-123">Add a Google account to the Android device by clicking **Apps** > **Settings** > **Add account**, then follow the prompts.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. <span data-ttu-id="b0333-124">Futtassa a todolist alkalmazást előtt, és egy új teendő elem beszúrása.</span><span class="sxs-lookup"><span data-stu-id="b0333-124">Run the todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="b0333-125">Ebben az esetben egy értesítési ikon jelenik meg az értesítési területen.</span><span class="sxs-lookup"><span data-stu-id="b0333-125">This time, a notification icon is displayed in the notification area.</span></span> <span data-ttu-id="b0333-126">Úgy is megnyithatja az értesítési fiókot, az értesítés a teljes szöveg megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b0333-126">You can open the notification drawer to view the full text of the notification.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
