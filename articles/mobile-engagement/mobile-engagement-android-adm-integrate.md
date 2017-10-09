---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a7d719ec-67b3-4be3-9d7f-0b61a57fe978
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c57132ff49cf8c335627a72b37f9b78529e84f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-adm-with-engagement"></a>Hogyan tooIntegrate az Engagement ADM
> [!IMPORTANT]
> Hogyan tooIntegrate Engagement az Android-dokumentum Ez az útmutató követése előtt hello ismertetett hello integrációs eljárást kell követnie.
> 
> Ez a dokumentum akkor hasznos, csak akkor, ha már integrált hello Reach modult, és a terv toopush Amazon eszközök. az alkalmazásban, olvassa el az első hogyan toointegrate Reach-kampányokat tooIntegrate Engagement Reach Android rendszeren.
> 
> 

## <a name="introduction"></a>Bevezetés
ADM integrációja lehetővé teszi, hogy az alkalmazás toobe leküldött, ha a célcsoport-kezelési Amazon Android-eszközök.

Mindig leküldött toohello SDK ADM hasznos adatot tartalmaznak hello `azme` hello adatobjektum kulcsban. Így ADM más célra, az alkalmazás használatakor, leküldéses értesítések kulcs alapján szűrheti.

> [!IMPORTANT]
> Csak Amazon Kindle futtató eszközökön Android 4.0.3 vagy újabb verziók támogatottak az Amazon Device Messaging; azonban ez a kód biztonságosan egyéb eszközein futó integrálhatja.
> 
> 

## <a name="sign-up-tooadm"></a>Iratkozzon fel tooADM
Ha nem tette meg, engedélyeznie kell ADM a Amazon-fiókjában.

hello eljárás részletes: [ <https://developer.amazon.com/sdk/adm/credentials.html>].

Hello eljárás befejezése után kap:

* OAuth-felhasználó hitelesítő adatait (ügyfél-azonosító és a titkos Ügyfélkulcsot) az Engagement toobe képes toopush az eszközök.
* API-kulcs, amely kell integrálni az alkalmazást.

## <a name="sdk-integration"></a>SDK-integráció
### <a name="managing-device-registrations"></a>Eszköz regisztrációk kezelése
Minden eszköz kell küldenie a egy regisztrációs parancs toohello ADM kiszolgálók, ellenkező esetben ezek nem érhető el.

Ha már használja a hello [ADM ügyféloldali kódtár], és már rendelkezik [ADM integrált] tooandroid és sdk-adm-fogadási közvetlenül lépjen.

Ha Ön nem rendelkezik integrált ADM, mégis Engagement rendelkezik egy egyszerűbb módon tooenable azt az alkalmazásban:

Szerkessze a `AndroidManifest.xml` fájlt:

* Adja hozzá a hello Amazon-névteret, hello fájl kezdjen ehhez hasonló:
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* Belső hello `<application/>` címkét, ez a szakasz hozzáadása:
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* A felvett hello amazon címke, előfordulhat, hogy egy összeállítási hibát, ha a Project Build Target Android 2.1 alatt van. Toouse rendelkezik egy **Android 2.1 +** build target (ne aggódjon, továbbra is rendelkezhet egy `minSdkVersion` too4 beállítása).
* A következő integrálását eszközként hello ADM API-kulcs [ezzel az eljárással].

Ezután kövesse a következő szakaszok hello hello utasításokat.

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a>Eszközregisztrációs azonosító toohello Engagement leküldéses szolgáltatás kommunikálnak, és értesítéseket kaphat
A sorrend toocommunicate hello regisztrációs azonosítója hello eszköz toohello Engagement leküldéses szolgáltatás és az értesítéseket, adja hozzá a következő tooyour hello `AndroidManifest.xml` fájl belül hello `<application/>` címke (Ha használja a ADM Engagement nélkül):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Ellenőrizze, hogy a következő engedélyek szükségesek a hello a `AndroidManifest.xml` (előtt hello `</application>` címke).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a>Támogatás Engagement OAuth-hitelesítőadatok
Küldje el az OAuth-hitelesítőadatok (ügyfél-azonosító és a titkos ügyfélkódot) Engagement portálon.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM ügyféloldali kódtár]:https://developer.amazon.com/sdk/adm/setup.html
[ADM integrált]:https://developer.amazon.com/sdk/adm/integrating-app.html
[ezzel az eljárással]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
