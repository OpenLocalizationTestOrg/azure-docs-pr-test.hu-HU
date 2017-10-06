---
title: "aaaAdd leküldéses értesítések tooyour Xamarin.Android-alkalmazásokhoz |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure App Service és az Azure Notification Hubs toosend leküldéses értesítések tooyour Xamarin.Android-alkalmazás"
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
ms.openlocfilehash: c93d1d0cae06ab15e3e3e5c4b342808b2cd49113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinandroid-app"></a><span data-ttu-id="95dda-103">Leküldéses értesítések tooyour Xamarin.Android-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="95dda-103">Add push notifications tooyour Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="95dda-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="95dda-104">Overview</span></span>
<span data-ttu-id="95dda-105">Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [Xamarin.Android gyors üzembe helyezési](app-service-mobile-windows-store-dotnet-get-started.md) projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="95dda-105">In this tutorial, you add push notifications toohello [Xamarin.Android quick start](app-service-mobile-windows-store-dotnet-get-started.md) project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="95dda-106">Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában.</span><span class="sxs-lookup"><span data-stu-id="95dda-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="95dda-107">Lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="95dda-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95dda-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="95dda-108">Prerequisites</span></span>
<span data-ttu-id="95dda-109">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="95dda-109">This tutorial requires hello following:</span></span>

* <span data-ttu-id="95dda-110">Aktív Google-fiók.</span><span class="sxs-lookup"><span data-stu-id="95dda-110">An active Google account.</span></span> <span data-ttu-id="95dda-111">A Google-fiókja, regisztrálhat [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span><span class="sxs-lookup"><span data-stu-id="95dda-111">You can sign up for a Google account at [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>
* <span data-ttu-id="95dda-112">[A Google Cloud Messaging Client összetevő](http://components.xamarin.com/view/GCMClient/).</span><span class="sxs-lookup"><span data-stu-id="95dda-112">[Google Cloud Messaging Client Component](http://components.xamarin.com/view/GCMClient/).</span></span>

## <span data-ttu-id="95dda-113"><a name="configure-hub"></a>Egy értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="95dda-113"><a name="configure-hub"></a>Configure a Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="95dda-114"><a id="register"></a>Engedélyezze a Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="95dda-114"><a id="register"></a>Enable Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-toosend-push-requests"></a><span data-ttu-id="95dda-115">Konfigurálhatja az Azure toosend leküldéses kérések</span><span class="sxs-lookup"><span data-stu-id="95dda-115">Configure Azure toosend push requests</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <span data-ttu-id="95dda-116"><a id="update-server"></a>Frissítési hello server projekt toosend leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="95dda-116"><a id="update-server"></a>Update hello server project toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="95dda-117"><a id="configure-app"></a>A leküldéses értesítések hello ügyfél-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="95dda-117"><a id="configure-app"></a>Configure hello client project for push notifications</span></span>
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <span data-ttu-id="95dda-118"><a id="add-push"></a>Leküldéses értesítések kód tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="95dda-118"><a id="add-push"></a>Add push notifications code tooyour app</span></span>
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <span data-ttu-id="95dda-119"><a name="test"></a>Teszt leküldéses értesítések az alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="95dda-119"><a name="test"></a>Test push notifications in your app</span></span>
<span data-ttu-id="95dda-120">Egy virtuális eszközzel hello emulátorban hello alkalmazást tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="95dda-120">You can test hello app by using a virtual device in hello emulator.</span></span> <span data-ttu-id="95dda-121">Nincsenek az emulátor futtatásához szükséges további konfigurációs lépéseket.</span><span class="sxs-lookup"><span data-stu-id="95dda-121">There are additional configuration steps required when running on an emulator.</span></span>

1. <span data-ttu-id="95dda-122">Győződjön meg arról, hogy telepít egy virtuális eszközön, amely rendelkezik a Google API-k hello célként beállítva hello Android virtuális eszközt (AVD) kezelő az alábbiak szerint hibakeresés tooor.</span><span class="sxs-lookup"><span data-stu-id="95dda-122">Make sure that you are deploying tooor debugging on a virtual device that has Google APIs set as hello target, as shown below in hello Android Virtual Device (AVD) manager.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. <span data-ttu-id="95dda-123">Google fiók toohello Android-eszköz hozzáadásához kattintson **alkalmazások** > **beállítások** > **fiók hozzáadása**, majd kövesse a hello megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="95dda-123">Add a Google account toohello Android device by clicking **Apps** > **Settings** > **Add account**, then follow hello prompts.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. <span data-ttu-id="95dda-124">Futtassa a hello todolist alkalmazást előtt, és egy új teendő elem beszúrására.</span><span class="sxs-lookup"><span data-stu-id="95dda-124">Run hello todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="95dda-125">Megadott idő hello értesítési területén egy értesítési ikon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="95dda-125">This time, a notification icon is displayed in hello notification area.</span></span> <span data-ttu-id="95dda-126">Hello értesítési fiók tooview hello teljes szöveges hello értesítés nyithatja meg.</span><span class="sxs-lookup"><span data-stu-id="95dda-126">You can open hello notification drawer tooview hello full text of hello notification.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
