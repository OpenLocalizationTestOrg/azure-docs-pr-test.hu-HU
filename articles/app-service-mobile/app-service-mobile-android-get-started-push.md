---
title: "aaaAdd leküldéses értesítések tooyour Android-alkalmazást a Mobile Apps |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Mobile Apps toosend leküldéses értesítések tooyour Android-alkalmazás."
services: app-service\mobile
documentationcenter: android
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/12/2016
ms.author: glenga
ms.openlocfilehash: dcfb8463b395904b4bd0bf9bde2f71f259894066
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-android-app"></a><span data-ttu-id="ebb73-103">Leküldéses értesítések tooyour Android-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ebb73-103">Add push notifications tooyour Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="ebb73-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ebb73-104">Overview</span></span>
<span data-ttu-id="ebb73-105">Ebben az oktatóanyagban leküldéses értesítések toohello hozzáadása [Android gyors üzembe helyezési] projektre, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="ebb73-105">In this tutorial, you add push notifications toohello [Android quick start] project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="ebb73-106">Ha nem használ hello letöltése – első lépések, leküldéses értesítési kiterjesztési csomagot kell hello.</span><span class="sxs-lookup"><span data-stu-id="ebb73-106">If you do not use hello downloaded quick start server project, you need hello push notification extension package.</span></span> <span data-ttu-id="ebb73-107">További információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="ebb73-107">For more information, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ebb73-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ebb73-108">Prerequisites</span></span>
<span data-ttu-id="ebb73-109">Hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="ebb73-109">You need hello following:</span></span>

* <span data-ttu-id="ebb73-110">Egy IDE, attól függően, hogy a projekt háttér:</span><span class="sxs-lookup"><span data-stu-id="ebb73-110">An IDE, depending on your project's back end:</span></span>

  * <span data-ttu-id="ebb73-111">[Android Studio](https://developer.android.com/sdk/index.html) a Node.js háttérből az alkalmazás-e.</span><span class="sxs-lookup"><span data-stu-id="ebb73-111">[Android Studio](https://developer.android.com/sdk/index.html) if this app has a Node.js back end.</span></span>
  * <span data-ttu-id="ebb73-112">[A Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) vagy újabb verzióját, ha az alkalmazás a Microsoft .NET-háttérből.</span><span class="sxs-lookup"><span data-stu-id="ebb73-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) or later if this app has a Microsoft .NET back end.</span></span>
* <span data-ttu-id="ebb73-113">Android 2.3-as vagy újabb verzió, a Google-tárház változat 27 vagy újabb és a Google Play-szolgáltatásokra 9.0.2 vagy újabb Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="ebb73-113">Android 2.3 or later, Google Repository revision 27 or later, and Google Play Services 9.0.2 or later for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="ebb73-114">Teljes hello [Android gyors üzembe helyezési].</span><span class="sxs-lookup"><span data-stu-id="ebb73-114">Complete hello [Android quick start].</span></span>

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a><span data-ttu-id="ebb73-115">A Firebase Cloud Messaginget támogató projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebb73-115">Create a project that supports Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a><span data-ttu-id="ebb73-116">Egy értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebb73-116">Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-toosend-push-notifications"></a><span data-ttu-id="ebb73-117">Az Azure toosend leküldéses értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebb73-117">Configure Azure toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-hello-server-project"></a><span data-ttu-id="ebb73-118">Engedélyezze a leküldéses értesítéseket hello server projekthez</span><span class="sxs-lookup"><span data-stu-id="ebb73-118">Enable push notifications for hello server project</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-tooyour-app"></a><span data-ttu-id="ebb73-119">Leküldéses értesítések tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ebb73-119">Add push notifications tooyour app</span></span>
<span data-ttu-id="ebb73-120">Ebben a szakaszban frissíti az ügyfél Android-alkalmazás toohandle leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="ebb73-120">In this section, you update your client Android app toohandle push notifications.</span></span>

### <a name="verify-android-sdk-version"></a><span data-ttu-id="ebb73-121">Android SDK-verziójának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="ebb73-121">Verify Android SDK version</span></span>
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

<span data-ttu-id="ebb73-122">A következő lépés tooinstall Google Play-szolgáltatásokra.</span><span class="sxs-lookup"><span data-stu-id="ebb73-122">Your next step is tooinstall Google Play services.</span></span> <span data-ttu-id="ebb73-123">Google Cloud Messaging rendelkezik néhány minimális API szintre vonatkozó követelményeinek fejlesztéshez és teszteléshez, mely hello **minSdkVersion** tulajdonság hello jegyzékben meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="ebb73-123">Google Cloud Messaging has some minimum API level requirements for development and testing, which hello **minSdkVersion** property in hello manifest must conform to.</span></span>

<span data-ttu-id="ebb73-124">Ha egy régebbi eszközzel rendelkező teszteli, tekintse meg [beállítva fel Google Play Services SDK] toodetermine hogyan alacsony értékre állítja, és állítsa be megfelelően.</span><span class="sxs-lookup"><span data-stu-id="ebb73-124">If you are testing with an older device, consult [Set Up Google Play Services SDK] toodetermine how low you can set this value, and set it appropriately.</span></span>

### <a name="add-google-play-services-toohello-project"></a><span data-ttu-id="ebb73-125">Google Play services toohello projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ebb73-125">Add Google Play services toohello project</span></span>
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a><span data-ttu-id="ebb73-126">Adja hozzá a kódot</span><span class="sxs-lookup"><span data-stu-id="ebb73-126">Add code</span></span>
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-hello-app-against-hello-published-mobile-service"></a><span data-ttu-id="ebb73-127">Teszt hello alkalmazás hello mobilszolgáltatás közzététele</span><span class="sxs-lookup"><span data-stu-id="ebb73-127">Test hello app against hello published mobile service</span></span>
<span data-ttu-id="ebb73-128">USB-kábellel Androidos telefonnal közvetlenül csatolásával, vagy egy virtuális eszközzel hello emulátorban hello alkalmazást tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="ebb73-128">You can test hello app by directly attaching an Android phone with a USB cable, or by using a virtual device in hello emulator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebb73-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ebb73-129">Next steps</span></span>
<span data-ttu-id="ebb73-130">Most, hogy ez az oktatóanyag befejezése vegye figyelembe az alábbi oktatóanyagok hello tooone a folytatása:</span><span class="sxs-lookup"><span data-stu-id="ebb73-130">Now that you completed this tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* <span data-ttu-id="ebb73-131">[Hitelesítési tooyour Android-alkalmazás hozzáadása](app-service-mobile-android-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="ebb73-131">[Add authentication tooyour Android app](app-service-mobile-android-get-started-users.md).</span></span>
  <span data-ttu-id="ebb73-132">Ismerje meg, hogyan tooadd hitelesítési toohello todolist gyors üzembe helyezési projekt támogatott identitásszolgáltató használatával Android rendszeren.</span><span class="sxs-lookup"><span data-stu-id="ebb73-132">Learn how tooadd authentication toohello todolist quickstart project on Android using a supported identity provider.</span></span>
* <span data-ttu-id="ebb73-133">[Az Android-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="ebb73-133">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="ebb73-134">Ismerje meg, hogyan tooadd offline tooyour alkalmazás használatával támogatják a Mobile Apps háttérből.</span><span class="sxs-lookup"><span data-stu-id="ebb73-134">Learn how tooadd offline support tooyour app by using a Mobile Apps back end.</span></span> <span data-ttu-id="ebb73-135">Kapcsolat nélküli szinkronizálás, a felhasználók használhatják a mobilalkalmazás&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ebb73-135">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs -->
[Android gyors üzembe helyezési]: app-service-mobile-android-get-started.md

[beállítva fel Google Play Services SDK]:https://developers.google.com/android/guides/setup
