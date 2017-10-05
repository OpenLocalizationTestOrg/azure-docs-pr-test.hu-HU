---
title: "Az Azure Mobile Engagement Android SDK-integráció"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d72b5014-a22b-4a7f-a470-d2b8145b5b86
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/10/2016
ms.author: piyushjo
ms.openlocfilehash: 0282abbf44406cac89c13520bc2a4e375817ed1f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-gcm-with-mobile-engagement"></a><span data-ttu-id="d8bfa-103">GCM integrálása Mobilmarketing</span><span class="sxs-lookup"><span data-stu-id="d8bfa-103">How to Integrate GCM with Mobile Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d8bfa-104">Meg kell követnie a integrációs ismertetett a hogyan integrálhatja Engagement Android dokumentumon Ez az útmutató követése előtt.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-104">You must follow the integration procedure described in the How to Integrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="d8bfa-105">Ez a dokumentum akkor hasznos, csak akkor, ha már integrálva a Reach-modul és a Google Play-eszközök leküldéses terv.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-105">This document is useful only if you already integrated the Reach module and plan to push Google Play devices.</span></span> <span data-ttu-id="d8bfa-106">Az alkalmazás Reach-kampányokat integrálását, olvassa el először Engagement elérésével integrálni az Android.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-106">To integrate Reach campaigns in your application, please read first How to Integrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="d8bfa-107">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="d8bfa-107">Introduction</span></span>
<span data-ttu-id="d8bfa-108">GCM integrálása lehetővé teszi, hogy az alkalmazás leküldött értesítést.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-108">Integrating GCM allows your application to be pushed.</span></span>

<span data-ttu-id="d8bfa-109">GCM vonatkozó Payload van jelen az SDK leküldött mindig tartalmazza a `azme` adatobjektum kulcsban.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-109">GCM payloads pushed to the SDK always contain the `azme` key in the data object.</span></span> <span data-ttu-id="d8bfa-110">Így GCM más célra, az alkalmazás használatakor, leküldéses értesítések kulcs alapján szűrheti.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-110">Thus if you use GCM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8bfa-111">Csak az Android 2.2-es futtató eszközökön vagy újabb, Google Play telepítve, és hogy Google rendelkező háttér-kapcsolat engedélyezett továbbíthatja a GCM; használatával azonban ez a kód biztonságosan eszközökön nem támogatott (csak a leképezések használ) integrálhatja.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-111">Only devices running Android 2.2 or above, having Google Play installed and having Google background connection enabled can be pushed using GCM; however, you can integrate this code safely on unsupported devices (it just uses intents).</span></span>
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a><span data-ttu-id="d8bfa-112">Google Cloud Messaging-projekt létrehozása API-kulcs segítségével</span><span class="sxs-lookup"><span data-stu-id="d8bfa-112">Create a Google Cloud Messaging project with API key</span></span>
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a><span data-ttu-id="d8bfa-113">SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="d8bfa-113">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="d8bfa-114">Eszköz regisztrációk kezelése</span><span class="sxs-lookup"><span data-stu-id="d8bfa-114">Managing device registrations</span></span>
<span data-ttu-id="d8bfa-115">Minden eszköz regisztrációs parancsot küld a Google kiszolgálóinak, ellenkező esetben ezek nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-115">Each device must send a registration command to the Google servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="d8bfa-116">Egy eszköz is (az eszköz nem automatikusan regisztrált Ha az alkalmazás el lesz távolítva) GCM-értesítések is regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-116">A device can also unregister from GCM notifications (the device is automatically unregistered if the application is uninstalled).</span></span>

<span data-ttu-id="d8bfa-117">Ha nem adja meg [Google Play SDK] vagy nem már küldünk a regisztráció célt saját magának, az eszköz automatikus regisztrációját meg Engagement végezheti el.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-117">If you don't use [Google Play SDK] or you don't already send the registration intent yourself, you can make Engagement register the device automatically for you.</span></span>

<span data-ttu-id="d8bfa-118">Ennek engedélyezéséhez adja hozzá a következőt a `AndroidManifest.xml` fájlon belül a `<application/>` címke:</span><span class="sxs-lookup"><span data-stu-id="d8bfa-118">To enable this, add the following to your `AndroidManifest.xml` file, inside the `<application/>` tag:</span></span>

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="d8bfa-119">Regisztrációs azonosítót az Engagement leküldéses szolgáltatáshoz kommunikálnak, és értesítéseket kaphat</span><span class="sxs-lookup"><span data-stu-id="d8bfa-119">Communicate registration id to the Engagement Push service and receive notifications</span></span>
<span data-ttu-id="d8bfa-120">Ahhoz, hogy az eszköz az Engagement Push szolgáltatás regisztrációs azonosítót kommunikációhoz, és az értesítéseket, adja hozzá a következőt a `AndroidManifest.xml` fájlon belül a `<application/>` címke (még akkor is, ha a kezelt eszköz regisztrációját meg):</span><span class="sxs-lookup"><span data-stu-id="d8bfa-120">In order to communicate the registration id of the device to the Engagement Push service and receive its notifications, add the following to your `AndroidManifest.xml` file, inside the `<application/>` tag (even if you manage device registrations yourself):</span></span>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

<span data-ttu-id="d8bfa-121">Ellenőrizze, hogy a következő engedélyeket a `AndroidManifest.xml` (után a `</application>` címke).</span><span class="sxs-lookup"><span data-stu-id="d8bfa-121">Ensure you have the following permissions in your `AndroidManifest.xml` (after the `</application>` tag).</span></span>

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a><span data-ttu-id="d8bfa-122">A GCM API-kulcshoz való hozzáférés biztosítása a Mobile Engagement számára</span><span class="sxs-lookup"><span data-stu-id="d8bfa-122">Grant Mobile Engagement access to your GCM API Key</span></span>
<span data-ttu-id="d8bfa-123">Hajtsa végre a [Ez az útmutató](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) a Mobile Engagement engedélyt a GCM API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d8bfa-123">Follow [this guide](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) to grant Mobile Engagement access to your GCM API Key.</span></span>

<span data-ttu-id="d8bfa-124">[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start</span><span class="sxs-lookup"><span data-stu-id="d8bfa-124">[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start</span></span>
