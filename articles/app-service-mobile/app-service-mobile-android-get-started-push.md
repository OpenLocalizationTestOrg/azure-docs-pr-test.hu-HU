---
title: "Leküldéses értesítések hozzáadása Mobile Apps az Android alkalmazás |} Microsoft Docs"
description: "Megtudhatja, hogyan küldhet leküldéses értesítéseket az Android-alkalmazás a Mobile Apps segítségével."
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
ms.openlocfilehash: b89e9af55342d5d7473d848956996f846250b4b5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-android-app"></a><span data-ttu-id="88c61-103">Leküldéses értesítések Android-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="88c61-103">Add push notifications to your Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="88c61-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="88c61-104">Overview</span></span>
<span data-ttu-id="88c61-105">Ebben az oktatóanyagban leküldéses értesítések hozzáadása a [Android gyors üzembe helyezési] projektre, hogy egy leküldéses értesítést küld az eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="88c61-105">In this tutorial, you add push notifications to the [Android quick start] project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="88c61-106">Ha nem használja a letöltött gyors üzembe helyezési projekt, a leküldéses értesítési kiterjesztési csomagot kell.</span><span class="sxs-lookup"><span data-stu-id="88c61-106">If you do not use the downloaded quick start server project, you need the push notification extension package.</span></span> <span data-ttu-id="88c61-107">További információkért lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="88c61-107">For more information, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88c61-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="88c61-108">Prerequisites</span></span>
<span data-ttu-id="88c61-109">A következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="88c61-109">You need the following:</span></span>

* <span data-ttu-id="88c61-110">Egy IDE, attól függően, hogy a projekt háttér:</span><span class="sxs-lookup"><span data-stu-id="88c61-110">An IDE, depending on your project's back end:</span></span>

  * <span data-ttu-id="88c61-111">[Android Studio](https://developer.android.com/sdk/index.html) a Node.js háttérből az alkalmazás-e.</span><span class="sxs-lookup"><span data-stu-id="88c61-111">[Android Studio](https://developer.android.com/sdk/index.html) if this app has a Node.js back end.</span></span>
  * <span data-ttu-id="88c61-112">[A Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) vagy újabb verzióját, ha az alkalmazás a Microsoft .NET-háttérből.</span><span class="sxs-lookup"><span data-stu-id="88c61-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) or later if this app has a Microsoft .NET back end.</span></span>
* <span data-ttu-id="88c61-113">Android 2.3-as vagy újabb verzió, a Google-tárház változat 27 vagy újabb és a Google Play-szolgáltatásokra 9.0.2 vagy újabb Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="88c61-113">Android 2.3 or later, Google Repository revision 27 or later, and Google Play Services 9.0.2 or later for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="88c61-114">Fejezze be a [Android gyors üzembe helyezési].</span><span class="sxs-lookup"><span data-stu-id="88c61-114">Complete the [Android quick start].</span></span>

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a><span data-ttu-id="88c61-115">A Firebase Cloud Messaginget támogató projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="88c61-115">Create a project that supports Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a><span data-ttu-id="88c61-116">Egy értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="88c61-116">Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a><span data-ttu-id="88c61-117">Leküldéses értesítések küldéséhez Azure konfigurálása</span><span class="sxs-lookup"><span data-stu-id="88c61-117">Configure Azure to send push notifications</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a><span data-ttu-id="88c61-118">A kiszolgáló projekt leküldéses értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="88c61-118">Enable push notifications for the server project</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a><span data-ttu-id="88c61-119">Leküldéses értesítések hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="88c61-119">Add push notifications to your app</span></span>
<span data-ttu-id="88c61-120">Ebben a szakaszban frissíti az ügyfél Android-alkalmazás leküldéses értesítések kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="88c61-120">In this section, you update your client Android app to handle push notifications.</span></span>

### <a name="verify-android-sdk-version"></a><span data-ttu-id="88c61-121">Android SDK-verziójának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="88c61-121">Verify Android SDK version</span></span>
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

<span data-ttu-id="88c61-122">A következő lépés, hogy telepítse a Google Play-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="88c61-122">Your next step is to install Google Play services.</span></span> <span data-ttu-id="88c61-123">Google Cloud Messaging rendelkezik néhány minimális API szintre vonatkozó követelményeinek fejlesztéshez és teszteléshez, amely a **minSdkVersion** tulajdonság a jegyzékben meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="88c61-123">Google Cloud Messaging has some minimum API level requirements for development and testing, which the **minSdkVersion** property in the manifest must conform to.</span></span>

<span data-ttu-id="88c61-124">Ha egy régebbi eszközzel rendelkező teszteli, tekintse meg [beállítva fel Google Play Services SDK] annak meghatározásához, hogyan alacsony értékre állítja, és állítsa be megfelelően.</span><span class="sxs-lookup"><span data-stu-id="88c61-124">If you are testing with an older device, consult [Set Up Google Play Services SDK] to determine how low you can set this value, and set it appropriately.</span></span>

### <a name="add-google-play-services-to-the-project"></a><span data-ttu-id="88c61-125">Google Play-szolgáltatások felvétele a projektbe</span><span class="sxs-lookup"><span data-stu-id="88c61-125">Add Google Play services to the project</span></span>
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a><span data-ttu-id="88c61-126">Adja hozzá a kódot</span><span class="sxs-lookup"><span data-stu-id="88c61-126">Add code</span></span>
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a><span data-ttu-id="88c61-127">A közzétett mobilszolgáltatás alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="88c61-127">Test the app against the published mobile service</span></span>
<span data-ttu-id="88c61-128">Az alkalmazás közvetlen csatlakoztatása az USB-kábellel Androidos telefonnal, vagy az emulátorban a virtuális eszköz segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="88c61-128">You can test the app by directly attaching an Android phone with a USB cable, or by using a virtual device in the emulator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="88c61-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="88c61-129">Next steps</span></span>
<span data-ttu-id="88c61-130">Most, hogy ez az oktatóanyag befejezése fontolja meg valamelyik az alábbi oktatóanyagok folytatása:</span><span class="sxs-lookup"><span data-stu-id="88c61-130">Now that you completed this tutorial, consider continuing on to one of the following tutorials:</span></span>

* <span data-ttu-id="88c61-131">[Hitelesítés hozzáadása az Android-alkalmazás](app-service-mobile-android-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="88c61-131">[Add authentication to your Android app](app-service-mobile-android-get-started-users.md).</span></span>
  <span data-ttu-id="88c61-132">Útmutató: hitelesítés hozzáadása a todolist gyorsútmutató-projekt az Android támogatott identitásszolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="88c61-132">Learn how to add authentication to the todolist quickstart project on Android using a supported identity provider.</span></span>
* <span data-ttu-id="88c61-133">[Az Android-alkalmazás kapcsolat nélküli szinkronizálásának engedélyezése](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="88c61-133">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="88c61-134">Megtudhatja, hogyan adhat offline támogatást az alkalmazást a Mobile Apps háttérből segítségével.</span><span class="sxs-lookup"><span data-stu-id="88c61-134">Learn how to add offline support to your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="88c61-135">Kapcsolat nélküli szinkronizálás, a felhasználók használhatják a mobilalkalmazás&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="88c61-135">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs -->
<span data-ttu-id="88c61-136">[Android gyors üzembe helyezési]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="88c61-136">[Android quick start]: app-service-mobile-android-get-started.md</span></span>

<span data-ttu-id="88c61-137">[beállítva fel Google Play Services SDK]:https://developers.google.com/android/guides/setup</span><span class="sxs-lookup"><span data-stu-id="88c61-137">[Set Up Google Play Services SDK]:https://developers.google.com/android/guides/setup</span></span>
