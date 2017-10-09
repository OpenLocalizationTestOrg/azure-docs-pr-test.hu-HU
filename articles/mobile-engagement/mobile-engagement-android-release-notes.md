---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 585341f9-3f39-459a-af42-864e400a0128
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 16b098198674c49567d720d0c01d984cb763ed8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes"></a><span data-ttu-id="e89a5-103">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e89a5-103">Release notes</span></span>

## <a name="431-07172017"></a><span data-ttu-id="e89a5-104">4.3.1 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="e89a5-104">4.3.1 (07/17/2017)</span></span>
* <span data-ttu-id="e89a5-105">Javítsa ki, amelyek ritkán fordulhat elő, amikor a program meghívja crash `EngagementAgentUtils.isInDedicatedEngagementProcess`, amely is használják hello `EngagementApplication` osztály.</span><span class="sxs-lookup"><span data-stu-id="e89a5-105">Fix a crash that could rarely happen when calling `EngagementAgentUtils.isInDedicatedEngagementProcess`, which is also used by hello `EngagementApplication` class.</span></span>

## <a name="430-06272017"></a><span data-ttu-id="e89a5-106">4.3.0 (06/27/2017)</span><span class="sxs-lookup"><span data-stu-id="e89a5-106">4.3.0 (06/27/2017)</span></span>
* <span data-ttu-id="e89a5-107">Android 8 támogatása (korábbi verzióiban hello SDK Android 8 nem fog működni).</span><span class="sxs-lookup"><span data-stu-id="e89a5-107">Android 8 support (previous versions of hello SDK will not work on Android 8).</span></span>
* <span data-ttu-id="e89a5-108">Nincs több függőség támogatási függvénytára.</span><span class="sxs-lookup"><span data-stu-id="e89a5-108">No more dependency on support library.</span></span>
* <span data-ttu-id="e89a5-109">Távolítsa el `EngagementFragmentActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="e89a5-109">Remove `EngagementFragmentActivity` class.</span></span>
* <span data-ttu-id="e89a5-110">Esedékes túl[háttér végrehajtási korlátozások](https://developer.android.com/preview/features/background.html) Android 8, a háttér-naplók előfordulhat, hogy később, amíg hello felhasználó hello eszköz kommunikál, ez hatással lesz a leküldéses kampány **kézbesített** és **Rendszerértesítő jelenik meg** Ha hello eszköz alvó volt folyamatban késleltetett statisztika (hello értesítési továbbra is megjelenik, fog gyűrű és mozog hibák nélkül valós időben).</span><span class="sxs-lookup"><span data-stu-id="e89a5-110">Due too[Background Execution Limits](https://developer.android.com/preview/features/background.html) on Android 8, logs in background might be delayed until hello user interacts with hello device, this will have an impact on Push Campaign **Delivered** and **System notification displayed** statistics being delayed if hello device was sleeping (hello notification will still be displayed, will ring and vibrate in real time without issues).</span></span>
* <span data-ttu-id="e89a5-111">Esedékes túl[háttér hely korlátok](https://developer.android.com/preview/features/background-location-limits.html), háttérben hello valós idejű hely nem fog frissülni gyakran Android 8.</span><span class="sxs-lookup"><span data-stu-id="e89a5-111">Due too[Background Location Limits](https://developer.android.com/preview/features/background-location-limits.html), hello real time location in background will not be updated frequently on Android 8.</span></span>

## <a name="424-03302017"></a><span data-ttu-id="e89a5-112">4.2.4 (03/30/2017)</span><span class="sxs-lookup"><span data-stu-id="e89a5-112">4.2.4 (03/30/2017)</span></span>
* <span data-ttu-id="e89a5-113">Javítsa ki az alkalmazáson belüli értesítés szöveg színét az Android 7 toobe hello azonos régebbi Android verzióként.</span><span class="sxs-lookup"><span data-stu-id="e89a5-113">Fix in-app notification text colors on Android 7 toobe hello same as older Android versions.</span></span>

## <a name="423-08102016"></a><span data-ttu-id="e89a5-114">4.2.3 (08/10/2016)</span><span class="sxs-lookup"><span data-stu-id="e89a5-114">4.2.3 (08/10/2016)</span></span>
* <span data-ttu-id="e89a5-115">Nincsenek további Wi-Fi zárolása.</span><span class="sxs-lookup"><span data-stu-id="e89a5-115">No more WIFI lock.</span></span>
* <span data-ttu-id="e89a5-116">Javítsa ki a holtpont getDeviceId előtt init (a hiba 4.2.0 bevezetett) hívásakor.</span><span class="sxs-lookup"><span data-stu-id="e89a5-116">Fix a deadlock when calling getDeviceId before init (bug introduced in 4.2.0).</span></span>

## <a name="422-05172016"></a><span data-ttu-id="e89a5-117">4.2.2 (05/17/2016)</span><span class="sxs-lookup"><span data-stu-id="e89a5-117">4.2.2 (05/17/2016)</span></span>
* <span data-ttu-id="e89a5-118">Jobb stabilitás.</span><span class="sxs-lookup"><span data-stu-id="e89a5-118">Stability improvements.</span></span>

## <a name="421-05102016"></a><span data-ttu-id="e89a5-119">4.2.1 (05/10/2016)</span><span class="sxs-lookup"><span data-stu-id="e89a5-119">4.2.1 (05/10/2016)</span></span>
* <span data-ttu-id="e89a5-120">Biztonsági: tiltsa le a webes nézet helyi fájl hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="e89a5-120">Security: disable web view local file access.</span></span>
* <span data-ttu-id="e89a5-121">Biztonsági: távolítsa el `EngagementPreferenceActivity` elavult, és nem biztonságos osztályt `PreferenceActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="e89a5-121">Security: remove `EngagementPreferenceActivity` class that extends obsolete and unsecure `PreferenceActivity` class.</span></span>
* <span data-ttu-id="e89a5-122">Biztonsági: tevékenységek is reach dokumentált toouse `exported="false"`, ez a jelző is használható a korábbi SDK-verziókban.</span><span class="sxs-lookup"><span data-stu-id="e89a5-122">Security: reach activities are now documented toouse `exported="false"`, this flag can also be used in previous SDK versions.</span></span>

## <a name="420-03112016"></a><span data-ttu-id="e89a5-123">4.2.0 (03/11/2016)</span><span class="sxs-lookup"><span data-stu-id="e89a5-123">4.2.0 (03/11/2016)</span></span>
* <span data-ttu-id="e89a5-124">hello SDK MIT most licencbe.</span><span class="sxs-lookup"><span data-stu-id="e89a5-124">hello SDK is now licensed under MIT.</span></span>
* <span data-ttu-id="e89a5-125">Lehetővé teszi egyéni eszköz azonosító megadása SDK inicializálása során.</span><span class="sxs-lookup"><span data-stu-id="e89a5-125">Allow specifying a custom device identifier at SDK initialization time.</span></span>

## <a name="415-02012016"></a><span data-ttu-id="e89a5-126">4.1.5 (02/01/2016)</span><span class="sxs-lookup"><span data-stu-id="e89a5-126">4.1.5 (02/01/2016)</span></span>
* <span data-ttu-id="e89a5-127">Jobb stabilitás.</span><span class="sxs-lookup"><span data-stu-id="e89a5-127">Stability improvements.</span></span>

## <a name="414-01262016"></a><span data-ttu-id="e89a5-128">4.1.4 (01/26/2016)</span><span class="sxs-lookup"><span data-stu-id="e89a5-128">4.1.4 (01/26/2016)</span></span>
* <span data-ttu-id="e89a5-129">Jobb stabilitás.</span><span class="sxs-lookup"><span data-stu-id="e89a5-129">Stability improvements.</span></span>

## <a name="413-1292015"></a><span data-ttu-id="e89a5-130">4.1.3 (12/9/2015)</span><span class="sxs-lookup"><span data-stu-id="e89a5-130">4.1.3 (12/9/2015)</span></span>
* <span data-ttu-id="e89a5-131">Jobb stabilitás.</span><span class="sxs-lookup"><span data-stu-id="e89a5-131">Stability improvements.</span></span>

## <a name="412-11252015"></a><span data-ttu-id="e89a5-132">4.1.2 (11/25/2015)</span><span class="sxs-lookup"><span data-stu-id="e89a5-132">4.1.2 (11/25/2015)</span></span>
* <span data-ttu-id="e89a5-133">Jobb stabilitás.</span><span class="sxs-lookup"><span data-stu-id="e89a5-133">Stability improvements.</span></span>

## <a name="411-11042015"></a><span data-ttu-id="e89a5-134">4.1.1 (11/04/2015)</span><span class="sxs-lookup"><span data-stu-id="e89a5-134">4.1.1 (11/04/2015)</span></span>
* <span data-ttu-id="e89a5-135">Jobb stabilitás.</span><span class="sxs-lookup"><span data-stu-id="e89a5-135">Stability improvements.</span></span>

## <a name="410-08252015"></a><span data-ttu-id="e89a5-136">4.1.0 (08/25/2015)</span><span class="sxs-lookup"><span data-stu-id="e89a5-136">4.1.0 (08/25/2015)</span></span>
* <span data-ttu-id="e89a5-137">Új engedélyezési modelljének Android M kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="e89a5-137">Handle new permission model for Android M.</span></span>
* <span data-ttu-id="e89a5-138">Segítségével mostantól beállíthatja hely funkciók használata helyett futásidőben `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="e89a5-138">Can now configure location features at runtime instead of using  `AndroidManifest.xml`.</span></span>
* <span data-ttu-id="e89a5-139">Javítsa ki az engedély Programhiba: Ha `ACCESS_FINE_LOCATION`, majd `ACCESS_COARSE_LOCATION` már nincs rá szükség.</span><span class="sxs-lookup"><span data-stu-id="e89a5-139">Fix a permission bug: if you use `ACCESS_FINE_LOCATION`, then `ACCESS_COARSE_LOCATION` is not needed anymore.</span></span>
* <span data-ttu-id="e89a5-140">Jobb stabilitás.</span><span class="sxs-lookup"><span data-stu-id="e89a5-140">Stability improvements.</span></span>

## <a name="400-07062015"></a><span data-ttu-id="e89a5-141">4.0.0 (07/06/2015)</span><span class="sxs-lookup"><span data-stu-id="e89a5-141">4.0.0 (07/06/2015)</span></span>
* <span data-ttu-id="e89a5-142">Belső protokoll toomake elemzések és leküldéses megbízhatóbb változik.</span><span class="sxs-lookup"><span data-stu-id="e89a5-142">Internal protocol changes toomake analytics and push more reliable.</span></span>
* <span data-ttu-id="e89a5-143">Natív leküldéses (GCM/ADM) most is szolgál az alkalmazáson belüli értesítések, konfigurálnia kell a hello hitelesítő adatokat a natív leküldéses kampány bármilyen típusú.</span><span class="sxs-lookup"><span data-stu-id="e89a5-143">Native push (GCM/ADM) is now also used for in app notifications so you must configure hello native push credentials for any type of push campaign.</span></span>
* <span data-ttu-id="e89a5-144">Javítsa ki a nagy vonalakban tekinti értesítési: után leküldött éppen megjelenített csak 10 egység voltak.</span><span class="sxs-lookup"><span data-stu-id="e89a5-144">Fix big picture notification: they were displayed only 10s after being pushed.</span></span>
* <span data-ttu-id="e89a5-145">Hárítsa el a hiba a webes nézet: a hivatkozásra kattintva is végrehajtása hello alapértelmezett műveleti URL-cím.</span><span class="sxs-lookup"><span data-stu-id="e89a5-145">Fix a bug in web view: clicking on a link was also executing hello default action URL.</span></span>
* <span data-ttu-id="e89a5-146">Hárítsa el a ritka összeomlási kapcsolódó toolocal tárolók kezelése.</span><span class="sxs-lookup"><span data-stu-id="e89a5-146">Fix a rare crash related toolocal storage management.</span></span>
* <span data-ttu-id="e89a5-147">Javítsa ki a dinamikus konfigurációkezelés karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="e89a5-147">Fix dynamic configuration string management.</span></span>
* <span data-ttu-id="e89a5-148">Frissítse a végfelhasználói LICENCSZERZŐDÉST.</span><span class="sxs-lookup"><span data-stu-id="e89a5-148">Update EULA.</span></span>

## <a name="300-02172015"></a><span data-ttu-id="e89a5-149">3.0.0 (02/17/2015)</span><span class="sxs-lookup"><span data-stu-id="e89a5-149">3.0.0 (02/17/2015)</span></span>
* <span data-ttu-id="e89a5-150">Az Azure Mobile Engagement eredeti kiadás</span><span class="sxs-lookup"><span data-stu-id="e89a5-150">Initial Release of Azure Mobile Engagement</span></span>
* <span data-ttu-id="e89a5-151">a kapcsolódási karakterlánc konfigurációs appId konfigurációs cseréli le.</span><span class="sxs-lookup"><span data-stu-id="e89a5-151">appId configuration is replaced by a connection string configuration.</span></span>
* <span data-ttu-id="e89a5-152">API toosend és a tetszőleges XMPP-üzeneteket fogadjon tetszőleges XMPP-entitások.</span><span class="sxs-lookup"><span data-stu-id="e89a5-152">Removed API toosend and receive arbitrary XMPP messages from arbitrary XMPP entities.</span></span>
* <span data-ttu-id="e89a5-153">API toosend eltávolítva és eszközök közötti üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="e89a5-153">Removed API toosend and receive messages between devices.</span></span>
* <span data-ttu-id="e89a5-154">Biztonsági fejlesztések.</span><span class="sxs-lookup"><span data-stu-id="e89a5-154">Security improvements.</span></span>
* <span data-ttu-id="e89a5-155">Google Play és SmartAd követési eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="e89a5-155">Google Play and SmartAd tracking removed.</span></span>

