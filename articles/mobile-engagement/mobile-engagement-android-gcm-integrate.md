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
# <a name="how-toointegrate-gcm-with-mobile-engagement"></a>Hogyan tooIntegrate a GCM használata a Mobile Engagement
> [!IMPORTANT]
> Hogyan tooIntegrate Engagement az Android-dokumentum Ez az útmutató követése előtt hello ismertetett hello integrációs eljárást kell követnie.
> 
> Ez a dokumentum akkor hasznos, csak akkor, ha már integrált hello modul és a terv toopush Google Play eszközök eléréséhez. az alkalmazásban, olvassa el az első hogyan toointegrate Reach-kampányokat tooIntegrate Engagement Reach Android rendszeren.
> 
> 

## <a name="introduction"></a>Bevezetés
GCM integrálása lehetővé teszi, hogy az alkalmazás toobe leküldve.

Mindig leküldött toohello SDK GCM hasznos adatot tartalmaznak hello `azme` hello adatobjektum kulcsban. Így GCM más célra, az alkalmazás használatakor, leküldéses értesítések kulcs alapján szűrheti.

> [!IMPORTANT]
> Csak az Android 2.2-es futtató eszközökön vagy újabb, Google Play telepítve, és hogy Google rendelkező háttér-kapcsolat engedélyezett továbbíthatja a GCM; használatával azonban ez a kód biztonságosan eszközökön nem támogatott (csak a leképezések használ) integrálhatja.
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a>Google Cloud Messaging-projekt létrehozása API-kulcs segítségével
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a>SDK-integráció
### <a name="managing-device-registrations"></a>Eszköz regisztrációk kezelése
Minden eszköz kell küldenie a egy regisztrációs parancs toohello Google kiszolgálóinak, ellenkező esetben ezek nem érhető el.

Eszköz is tudja törölni a GCM-értesítések (hello eszköz nem automatikusan regisztrált Ha hello alkalmazás el lesz távolítva.).

Ha nem adja meg [Google Play SDK] vagy nem már küld hello regisztrációs leképezés saját kezűleg, Engagement hello eszköz automatikusan regisztrálja az Ön végezheti el.

tooenable, adja hozzá a következő tooyour hello `AndroidManifest.xml` fájl belül hello `<application/>` címke:

            <!-- If only 1 sender, don't forget hello \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a>Eszközregisztrációs azonosító toohello Engagement leküldéses szolgáltatás kommunikálnak, és értesítéseket kaphat
A sorrend toocommunicate hello regisztrációs azonosítója hello eszköz toohello Engagement leküldéses szolgáltatás és az értesítéseket, adja hozzá a következő tooyour hello `AndroidManifest.xml` fájl belül hello `<application/>` címke (még akkor is, ha a kezelt eszköz regisztrációját meg):

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

Ellenőrizze, hogy a következő engedélyek szükségesek a hello a `AndroidManifest.xml` (után hello `</application>` címke).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a>Engedélyezze a Mobile Engagement hozzáférés tooyour GCM API-kulcs
Hajtsa végre a [Ez az útmutató](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) toogrant a Mobile Engagement hozzáférés tooyour GCM API-kulcsot.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
