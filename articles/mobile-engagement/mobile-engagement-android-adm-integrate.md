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
# <a name="how-toointegrate-adm-with-engagement"></a><span data-ttu-id="7090c-103">Hogyan tooIntegrate az Engagement ADM</span><span class="sxs-lookup"><span data-stu-id="7090c-103">How tooIntegrate ADM with Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="7090c-104">Hogyan tooIntegrate Engagement az Android-dokumentum Ez az útmutató követése előtt hello ismertetett hello integrációs eljárást kell követnie.</span><span class="sxs-lookup"><span data-stu-id="7090c-104">You must follow hello integration procedure described in hello How tooIntegrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="7090c-105">Ez a dokumentum akkor hasznos, csak akkor, ha már integrált hello Reach modult, és a terv toopush Amazon eszközök.</span><span class="sxs-lookup"><span data-stu-id="7090c-105">This document is useful only if you already integrated hello Reach module and plan toopush Amazon devices.</span></span> <span data-ttu-id="7090c-106">az alkalmazásban, olvassa el az első hogyan toointegrate Reach-kampányokat tooIntegrate Engagement Reach Android rendszeren.</span><span class="sxs-lookup"><span data-stu-id="7090c-106">toointegrate Reach campaigns in your application, please read first How tooIntegrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="7090c-107">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="7090c-107">Introduction</span></span>
<span data-ttu-id="7090c-108">ADM integrációja lehetővé teszi, hogy az alkalmazás toobe leküldött, ha a célcsoport-kezelési Amazon Android-eszközök.</span><span class="sxs-lookup"><span data-stu-id="7090c-108">Integrating ADM allows your application toobe pushed when targeting Amazon Android devices.</span></span>

<span data-ttu-id="7090c-109">Mindig leküldött toohello SDK ADM hasznos adatot tartalmaznak hello `azme` hello adatobjektum kulcsban.</span><span class="sxs-lookup"><span data-stu-id="7090c-109">ADM payloads pushed toohello SDK always contain hello `azme` key in hello data object.</span></span> <span data-ttu-id="7090c-110">Így ADM más célra, az alkalmazás használatakor, leküldéses értesítések kulcs alapján szűrheti.</span><span class="sxs-lookup"><span data-stu-id="7090c-110">Thus if you use ADM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7090c-111">Csak Amazon Kindle futtató eszközökön Android 4.0.3 vagy újabb verziók támogatottak az Amazon Device Messaging; azonban ez a kód biztonságosan egyéb eszközein futó integrálhatja.</span><span class="sxs-lookup"><span data-stu-id="7090c-111">Only Amazon Kindle devices running Android 4.0.3 or above are supported by Amazon Device Messaging; however, you can integrate this code safely on other devices.</span></span>
> 
> 

## <a name="sign-up-tooadm"></a><span data-ttu-id="7090c-112">Iratkozzon fel tooADM</span><span class="sxs-lookup"><span data-stu-id="7090c-112">Sign up tooADM</span></span>
<span data-ttu-id="7090c-113">Ha nem tette meg, engedélyeznie kell ADM a Amazon-fiókjában.</span><span class="sxs-lookup"><span data-stu-id="7090c-113">If not already done, you must enable ADM on your Amazon account.</span></span>

<span data-ttu-id="7090c-114">hello eljárás részletes: [ <https://developer.amazon.com/sdk/adm/credentials.html>].</span><span class="sxs-lookup"><span data-stu-id="7090c-114">hello procedure is detailed at: [<https://developer.amazon.com/sdk/adm/credentials.html>].</span></span>

<span data-ttu-id="7090c-115">Hello eljárás befejezése után kap:</span><span class="sxs-lookup"><span data-stu-id="7090c-115">Upon completing hello procedure, you get:</span></span>

* <span data-ttu-id="7090c-116">OAuth-felhasználó hitelesítő adatait (ügyfél-azonosító és a titkos Ügyfélkulcsot) az Engagement toobe képes toopush az eszközök.</span><span class="sxs-lookup"><span data-stu-id="7090c-116">OAuth credentials (a Client ID and a Client Secret) for Engagement toobe able toopush your devices.</span></span>
* <span data-ttu-id="7090c-117">API-kulcs, amely kell integrálni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7090c-117">An API Key that must be integrated into your application.</span></span>

## <a name="sdk-integration"></a><span data-ttu-id="7090c-118">SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="7090c-118">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="7090c-119">Eszköz regisztrációk kezelése</span><span class="sxs-lookup"><span data-stu-id="7090c-119">Managing device registrations</span></span>
<span data-ttu-id="7090c-120">Minden eszköz kell küldenie a egy regisztrációs parancs toohello ADM kiszolgálók, ellenkező esetben ezek nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="7090c-120">Each device must send a registration command toohello ADM servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="7090c-121">Ha már használja a hello [ADM ügyféloldali kódtár], és már rendelkezik [ADM integrált] tooandroid és sdk-adm-fogadási közvetlenül lépjen.</span><span class="sxs-lookup"><span data-stu-id="7090c-121">If you already use hello [ADM client library], and already have [integrated ADM] you can directly go tooandroid-sdk-adm-receive.</span></span>

<span data-ttu-id="7090c-122">Ha Ön nem rendelkezik integrált ADM, mégis Engagement rendelkezik egy egyszerűbb módon tooenable azt az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="7090c-122">If you have not integrated ADM yet, Engagement has a simpler way tooenable it in your application:</span></span>

<span data-ttu-id="7090c-123">Szerkessze a `AndroidManifest.xml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="7090c-123">Edit your `AndroidManifest.xml` file:</span></span>

* <span data-ttu-id="7090c-124">Adja hozzá a hello Amazon-névteret, hello fájl kezdjen ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="7090c-124">Add hello Amazon namespace, hello file should begin like this:</span></span>
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* <span data-ttu-id="7090c-125">Belső hello `<application/>` címkét, ez a szakasz hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="7090c-125">Inside hello `<application/>` tag, add this section:</span></span>
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* <span data-ttu-id="7090c-126">A felvett hello amazon címke, előfordulhat, hogy egy összeállítási hibát, ha a Project Build Target Android 2.1 alatt van.</span><span class="sxs-lookup"><span data-stu-id="7090c-126">After adding hello amazon tag, you may have a build error if your Project Build Target is below Android 2.1.</span></span> <span data-ttu-id="7090c-127">Toouse rendelkezik egy **Android 2.1 +** build target (ne aggódjon, továbbra is rendelkezhet egy `minSdkVersion` too4 beállítása).</span><span class="sxs-lookup"><span data-stu-id="7090c-127">You have toouse an **Android 2.1+** build target (don't worry, you can still have a `minSdkVersion` set too4).</span></span>
* <span data-ttu-id="7090c-128">A következő integrálását eszközként hello ADM API-kulcs [ezzel az eljárással].</span><span class="sxs-lookup"><span data-stu-id="7090c-128">Integrate hello ADM API Key as an asset by following [this procedure].</span></span>

<span data-ttu-id="7090c-129">Ezután kövesse a következő szakaszok hello hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="7090c-129">Then follow hello instructions of hello next sections.</span></span>

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="7090c-130">Eszközregisztrációs azonosító toohello Engagement leküldéses szolgáltatás kommunikálnak, és értesítéseket kaphat</span><span class="sxs-lookup"><span data-stu-id="7090c-130">Communicate registration id toohello Engagement Push service and receive notifications</span></span>
<span data-ttu-id="7090c-131">A sorrend toocommunicate hello regisztrációs azonosítója hello eszköz toohello Engagement leküldéses szolgáltatás és az értesítéseket, adja hozzá a következő tooyour hello `AndroidManifest.xml` fájl belül hello `<application/>` címke (Ha használja a ADM Engagement nélkül):</span><span class="sxs-lookup"><span data-stu-id="7090c-131">In order toocommunicate hello registration id of hello device toohello Engagement Push service and receive its notifications, add hello following tooyour `AndroidManifest.xml` file, inside hello `<application/>` tag (even if you use ADM without Engagement):</span></span>

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

<span data-ttu-id="7090c-132">Ellenőrizze, hogy a következő engedélyek szükségesek a hello a `AndroidManifest.xml` (előtt hello `</application>` címke).</span><span class="sxs-lookup"><span data-stu-id="7090c-132">Ensure you have hello following permissions in your `AndroidManifest.xml` (before hello `</application>` tag).</span></span>

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a><span data-ttu-id="7090c-133">Támogatás Engagement OAuth-hitelesítőadatok</span><span class="sxs-lookup"><span data-stu-id="7090c-133">Grant Engagement OAuth credentials</span></span>
<span data-ttu-id="7090c-134">Küldje el az OAuth-hitelesítőadatok (ügyfél-azonosító és a titkos ügyfélkódot) Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="7090c-134">Submit your OAuth Credentials (Client ID and Client Secret) in Engagement Portal.</span></span>

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM ügyféloldali kódtár]:https://developer.amazon.com/sdk/adm/setup.html
[ADM integrált]:https://developer.amazon.com/sdk/adm/integrating-app.html
[ezzel az eljárással]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
