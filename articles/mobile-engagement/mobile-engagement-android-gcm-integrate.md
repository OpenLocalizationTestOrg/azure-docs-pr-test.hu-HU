---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
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
ms.openlocfilehash: e81230cbc99a209f2909cc163c4e566df67dc828
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-gcm-with-mobile-engagement"></a><span data-ttu-id="e3107-103">Hogyan tooIntegrate a GCM használata a Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="e3107-103">How tooIntegrate GCM with Mobile Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e3107-104">Hogyan tooIntegrate Engagement az Android-dokumentum Ez az útmutató követése előtt hello ismertetett hello integrációs eljárást kell követnie.</span><span class="sxs-lookup"><span data-stu-id="e3107-104">You must follow hello integration procedure described in hello How tooIntegrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="e3107-105">Ez a dokumentum akkor hasznos, csak akkor, ha már integrált hello modul és a terv toopush Google Play eszközök eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e3107-105">This document is useful only if you already integrated hello Reach module and plan toopush Google Play devices.</span></span> <span data-ttu-id="e3107-106">az alkalmazásban, olvassa el az első hogyan toointegrate Reach-kampányokat tooIntegrate Engagement Reach Android rendszeren.</span><span class="sxs-lookup"><span data-stu-id="e3107-106">toointegrate Reach campaigns in your application, please read first How tooIntegrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="e3107-107">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e3107-107">Introduction</span></span>
<span data-ttu-id="e3107-108">GCM integrálása lehetővé teszi, hogy az alkalmazás toobe leküldve.</span><span class="sxs-lookup"><span data-stu-id="e3107-108">Integrating GCM allows your application toobe pushed.</span></span>

<span data-ttu-id="e3107-109">Mindig leküldött toohello SDK GCM hasznos adatot tartalmaznak hello `azme` hello adatobjektum kulcsban.</span><span class="sxs-lookup"><span data-stu-id="e3107-109">GCM payloads pushed toohello SDK always contain hello `azme` key in hello data object.</span></span> <span data-ttu-id="e3107-110">Így GCM más célra, az alkalmazás használatakor, leküldéses értesítések kulcs alapján szűrheti.</span><span class="sxs-lookup"><span data-stu-id="e3107-110">Thus if you use GCM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3107-111">Csak az Android 2.2-es futtató eszközökön vagy újabb, Google Play telepítve, és hogy Google rendelkező háttér-kapcsolat engedélyezett továbbíthatja a GCM; használatával azonban ez a kód biztonságosan eszközökön nem támogatott (csak a leképezések használ) integrálhatja.</span><span class="sxs-lookup"><span data-stu-id="e3107-111">Only devices running Android 2.2 or above, having Google Play installed and having Google background connection enabled can be pushed using GCM; however, you can integrate this code safely on unsupported devices (it just uses intents).</span></span>
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a><span data-ttu-id="e3107-112">Google Cloud Messaging-projekt létrehozása API-kulcs segítségével</span><span class="sxs-lookup"><span data-stu-id="e3107-112">Create a Google Cloud Messaging project with API key</span></span>
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a><span data-ttu-id="e3107-113">SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="e3107-113">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="e3107-114">Eszköz regisztrációk kezelése</span><span class="sxs-lookup"><span data-stu-id="e3107-114">Managing device registrations</span></span>
<span data-ttu-id="e3107-115">Minden eszköz kell küldenie a egy regisztrációs parancs toohello Google kiszolgálóinak, ellenkező esetben ezek nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="e3107-115">Each device must send a registration command toohello Google servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="e3107-116">Eszköz is tudja törölni a GCM-értesítések (hello eszköz nem automatikusan regisztrált Ha hello alkalmazás el lesz távolítva.).</span><span class="sxs-lookup"><span data-stu-id="e3107-116">A device can also unregister from GCM notifications (hello device is automatically unregistered if hello application is uninstalled).</span></span>

<span data-ttu-id="e3107-117">Ha nem adja meg [Google Play SDK] vagy nem már küld hello regisztrációs leképezés saját kezűleg, Engagement hello eszköz automatikusan regisztrálja az Ön végezheti el.</span><span class="sxs-lookup"><span data-stu-id="e3107-117">If you don't use [Google Play SDK] or you don't already send hello registration intent yourself, you can make Engagement register hello device automatically for you.</span></span>

<span data-ttu-id="e3107-118">tooenable, adja hozzá a következő tooyour hello `AndroidManifest.xml` fájl belül hello `<application/>` címke:</span><span class="sxs-lookup"><span data-stu-id="e3107-118">tooenable this, add hello following tooyour `AndroidManifest.xml` file, inside hello `<application/>` tag:</span></span>

            <!-- If only 1 sender, don't forget hello \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="e3107-119">Eszközregisztrációs azonosító toohello Engagement leküldéses szolgáltatás kommunikálnak, és értesítéseket kaphat</span><span class="sxs-lookup"><span data-stu-id="e3107-119">Communicate registration id toohello Engagement Push service and receive notifications</span></span>
<span data-ttu-id="e3107-120">A sorrend toocommunicate hello regisztrációs azonosítója hello eszköz toohello Engagement leküldéses szolgáltatás és az értesítéseket, adja hozzá a következő tooyour hello `AndroidManifest.xml` fájl belül hello `<application/>` címke (még akkor is, ha a kezelt eszköz regisztrációját meg):</span><span class="sxs-lookup"><span data-stu-id="e3107-120">In order toocommunicate hello registration id of hello device toohello Engagement Push service and receive its notifications, add hello following tooyour `AndroidManifest.xml` file, inside hello `<application/>` tag (even if you manage device registrations yourself):</span></span>

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

<span data-ttu-id="e3107-121">Ellenőrizze, hogy a következő engedélyek szükségesek a hello a `AndroidManifest.xml` (után hello `</application>` címke).</span><span class="sxs-lookup"><span data-stu-id="e3107-121">Ensure you have hello following permissions in your `AndroidManifest.xml` (after hello `</application>` tag).</span></span>

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a><span data-ttu-id="e3107-122">Engedélyezze a Mobile Engagement hozzáférés tooyour GCM API-kulcs</span><span class="sxs-lookup"><span data-stu-id="e3107-122">Grant Mobile Engagement access tooyour GCM API Key</span></span>
<span data-ttu-id="e3107-123">Hajtsa végre a [Ez az útmutató](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) toogrant a Mobile Engagement hozzáférés tooyour GCM API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e3107-123">Follow [this guide](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) toogrant Mobile Engagement access tooyour GCM API Key.</span></span>

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
