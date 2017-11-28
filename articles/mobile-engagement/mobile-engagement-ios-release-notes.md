---
title: "aaaAzure a Mobile Engagement iOS SDK kibocsátási megjegyzései |} Microsoft Docs"
description: "Legújabb frissítések és az Azure Mobile Engagement SDK iOS eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a43ff0f6-90d5-4b3c-8d7a-a1db21bc776b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: ae29d200ebb1784357b29edbd1f66b71df0778cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-ios-sdk-release-notes"></a><span data-ttu-id="ba014-103">Az Azure Mobile Engagement iOS SDK kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="ba014-103">Azure Mobile Engagement iOS SDK release notes</span></span>

## <a name="410-07172017"></a><span data-ttu-id="ba014-104">4.1.0 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="ba014-104">4.1.0 (07/17/2017)</span></span>
* <span data-ttu-id="ba014-105">Rögzített jelvények háttérben törölve lett.</span><span class="sxs-lookup"><span data-stu-id="ba014-105">Fixed badges cleared in background.</span></span>
* <span data-ttu-id="ba014-106">Rögzített figyelmeztetések a XCode 9 kapcsolatos API-k nem hívja a fő várakozási sorba.</span><span class="sxs-lookup"><span data-stu-id="ba014-106">Fixed warnings on XCode 9 about APIs not called in main queue.</span></span>
* <span data-ttu-id="ba014-107">Rögzített memóriavesztés Reach lekérdezéseinél.</span><span class="sxs-lookup"><span data-stu-id="ba014-107">Fixed a memory leak in Reach polls.</span></span>
* <span data-ttu-id="ba014-108">Támogatás az iOS eldobott 6.X.</span><span class="sxs-lookup"><span data-stu-id="ba014-108">Dropped support for iOS 6.X.</span></span> <span data-ttu-id="ba014-109">A verzió hello központi telepítés célja az alkalmazás-től kezdődő kell lennie legalább iOS 7-es.</span><span class="sxs-lookup"><span data-stu-id="ba014-109">Starting from this version hello deployment target of your application must be at least iOS 7.</span></span>

## <a name="401-12132016"></a><span data-ttu-id="ba014-110">4.0.1 (12/13/2016)</span><span class="sxs-lookup"><span data-stu-id="ba014-110">4.0.1 (12/13/2016)</span></span>
* <span data-ttu-id="ba014-111">Továbbfejlesztett naplózási kézbesítési háttérben.</span><span class="sxs-lookup"><span data-stu-id="ba014-111">Improved log delivery in background.</span></span>

## <a name="400-09122016"></a><span data-ttu-id="ba014-112">4.0.0 (09/12/2016)</span><span class="sxs-lookup"><span data-stu-id="ba014-112">4.0.0 (09/12/2016)</span></span>
* <span data-ttu-id="ba014-113">Rögzített értesítési nem műveletet kiváltó iOS 10-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="ba014-113">Fixed notification not actioned on iOS 10 devices.</span></span>
* <span data-ttu-id="ba014-114">XCode 7 érvényteleníthető.</span><span class="sxs-lookup"><span data-stu-id="ba014-114">Deprecate XCode 7.</span></span>

## <a name="324-06302016"></a><span data-ttu-id="ba014-115">3.2.4 (06/30/2016)</span><span class="sxs-lookup"><span data-stu-id="ba014-115">3.2.4 (06/30/2016)</span></span>
* <span data-ttu-id="ba014-116">Rögzített összesítési műszaki naplókat, valamint további naplófájlokat között.</span><span class="sxs-lookup"><span data-stu-id="ba014-116">Fixed aggregation between technical logs and other logs.</span></span>

## <a name="323-06072016"></a><span data-ttu-id="ba014-117">3.2.3 (06/07/2016)</span><span class="sxs-lookup"><span data-stu-id="ba014-117">3.2.3 (06/07/2016)</span></span>
* <span data-ttu-id="ba014-118">Rögzített hello hiba, ahol kézbesítési visszajelzés jelentése nem hello háttérben alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="ba014-118">Fixed hello bug where delivery feedback is not reported when app is in hello background.</span></span>
* <span data-ttu-id="ba014-119">Optimalizált hello műszaki naplók küldése.</span><span class="sxs-lookup"><span data-stu-id="ba014-119">Optimized hello sending of technical logs.</span></span>

## <a name="322-04072016"></a><span data-ttu-id="ba014-120">3.2.2 (04/07/2016)</span><span class="sxs-lookup"><span data-stu-id="ba014-120">3.2.2 (04/07/2016)</span></span>
* <span data-ttu-id="ba014-121">A HTTP kérelem törlését, ami időnként toocrash rögzített hiba.</span><span class="sxs-lookup"><span data-stu-id="ba014-121">Fixed bug on HTTP request cancellation which sometimes leads toocrash.</span></span>

## <a name="321-12112015"></a><span data-ttu-id="ba014-122">3.2.1 (12/11/2015)</span><span class="sxs-lookup"><span data-stu-id="ba014-122">3.2.1 (12/11/2015)</span></span>
* <span data-ttu-id="ba014-123">Ha egy új app-példány váltja ki, egy értesítés a mélyhivatkozással rögzített hello késleltetés</span><span class="sxs-lookup"><span data-stu-id="ba014-123">Fixed hello delay when a new app instance is triggered by a notification with deep links</span></span>

## <a name="320-10082015"></a><span data-ttu-id="ba014-124">3.2.0 (10/08/2015)</span><span class="sxs-lookup"><span data-stu-id="ba014-124">3.2.0 (10/08/2015)</span></span>
* <span data-ttu-id="ba014-125">Hello SDK toomake azt együttműködve Bitcode engedélyezve **Xcode 7**.</span><span class="sxs-lookup"><span data-stu-id="ba014-125">Enabled Bitcode in hello SDK toomake it work with **Xcode 7**.</span></span>
* <span data-ttu-id="ba014-126">Javított hibákról kapcsolódó tooin alkalmazáson belüli értesítések.</span><span class="sxs-lookup"><span data-stu-id="ba014-126">Fixed bugs related tooin-app notifications.</span></span>
* <span data-ttu-id="ba014-127">Alacsony töltöttségű telepre vonatkozó és egyéb ilyen forgatókönyvek esetén további megbízható végrehajtott hello az alkalmazásbeli értesítésekben.</span><span class="sxs-lookup"><span data-stu-id="ba014-127">Made hello in-app notifications more reliable in case of low battery and other such scenarios.</span></span>
* <span data-ttu-id="ba014-128">3. fél könyvtár által létrehozott további konzolnaplófájlokban eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="ba014-128">Removed extra console logs generated by 3rd party library.</span></span>

## <a name="310-08262015"></a><span data-ttu-id="ba014-129">3.1.0 (08/26/2015)</span><span class="sxs-lookup"><span data-stu-id="ba014-129">3.1.0 (08/26/2015)</span></span>
* <span data-ttu-id="ba014-130">Javítsa ki az iOS 9-es kompatibilitási hiba egy harmadik féltől származó könyvtárral.</span><span class="sxs-lookup"><span data-stu-id="ba014-130">Fix iOS 9 compatibility bug with a third party library.</span></span> <span data-ttu-id="ba014-131">Összeomlások okozott amíg küldése kérdezze le az eredményeket, az alkalmazással kapcsolatos adatok vagy a további adatokat.</span><span class="sxs-lookup"><span data-stu-id="ba014-131">It was causing crashes while sending polls results, application information or extra data.</span></span>

## <a name="300-06192015"></a><span data-ttu-id="ba014-132">3.0.0 (06/19/2015)</span><span class="sxs-lookup"><span data-stu-id="ba014-132">3.0.0 (06/19/2015)</span></span>
* <span data-ttu-id="ba014-133">A Mobile Engagement csendes leküldéses értesítések használja.</span><span class="sxs-lookup"><span data-stu-id="ba014-133">Mobile Engagement uses Silent Push Notifications.</span></span>
* <span data-ttu-id="ba014-134">Támogatás az iOS eldobott 4.X.</span><span class="sxs-lookup"><span data-stu-id="ba014-134">Dropped support for iOS 4.X.</span></span> <span data-ttu-id="ba014-135">A verzió hello központi telepítés célja az alkalmazás-től kezdődő kell lennie legalább iOS 6.</span><span class="sxs-lookup"><span data-stu-id="ba014-135">Starting from this version hello deployment target of your application must be at least iOS 6.</span></span>

## <a name="220-05212015"></a><span data-ttu-id="ba014-136">2.2.0 (05/21/2015)</span><span class="sxs-lookup"><span data-stu-id="ba014-136">2.2.0 (05/21/2015)</span></span>
* <span data-ttu-id="ba014-137">hello Mobile Engagement-eszközazonosítót az eszközök < iOS 6 alapján a telepítéskor létrehozott GUID.</span><span class="sxs-lookup"><span data-stu-id="ba014-137">hello Mobile Engagement device id for devices < iOS 6 is now based on a GUID generated at installation time.</span></span>

## <a name="210-04242015"></a><span data-ttu-id="ba014-138">2.1.0 (04/24/2015)</span><span class="sxs-lookup"><span data-stu-id="ba014-138">2.1.0 (04/24/2015)</span></span>
* <span data-ttu-id="ba014-139">A hozzáadott Swift kompatibilitási.</span><span class="sxs-lookup"><span data-stu-id="ba014-139">Added Swift compatibility.</span></span>
* <span data-ttu-id="ba014-140">Kattintva egy értesítést, ha az URL-cím már hello művelet végre jobb, hello alkalmazás megnyitása után.</span><span class="sxs-lookup"><span data-stu-id="ba014-140">When clicking on a notification, hello action URL is now executed right after hello application is opened.</span></span>
* <span data-ttu-id="ba014-141">A hozzáadott hiányzó fejlécfájlt az SDK-csomagot.</span><span class="sxs-lookup"><span data-stu-id="ba014-141">Added missing header file in SDK package.</span></span>
* <span data-ttu-id="ba014-142">Megtörtént egy probléma javítása amikor hello a Mobile Engagement összeomlási jelentéskészítői le lett tiltva.</span><span class="sxs-lookup"><span data-stu-id="ba014-142">Fixed an issue when hello Mobile Engagement crash reporter was disabled.</span></span>

## <a name="200-02172015"></a><span data-ttu-id="ba014-143">2.0.0 (02/17/2015)</span><span class="sxs-lookup"><span data-stu-id="ba014-143">2.0.0 (02/17/2015)</span></span>
* <span data-ttu-id="ba014-144">Az Azure Mobile Engagement eredeti kiadás</span><span class="sxs-lookup"><span data-stu-id="ba014-144">Initial Release of Azure Mobile Engagement</span></span>
* <span data-ttu-id="ba014-145">a kapcsolódási karakterlánc konfigurációs appId/sdkKey konfigurációs cseréli le.</span><span class="sxs-lookup"><span data-stu-id="ba014-145">appId/sdkKey configuration is replaced by a connection string configuration.</span></span>
* <span data-ttu-id="ba014-146">API toosend és a tetszőleges XMPP-üzeneteket fogadjon tetszőleges XMPP-entitások.</span><span class="sxs-lookup"><span data-stu-id="ba014-146">Removed API toosend and receive arbitrary XMPP messages from arbitrary XMPP entities.</span></span>
* <span data-ttu-id="ba014-147">API toosend eltávolítva és eszközök közötti üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="ba014-147">Removed API toosend and receive messages between devices.</span></span>
* <span data-ttu-id="ba014-148">Biztonsági fejlesztések.</span><span class="sxs-lookup"><span data-stu-id="ba014-148">Security improvements.</span></span>
* <span data-ttu-id="ba014-149">SmartAd követési eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="ba014-149">SmartAd tracking removed.</span></span>
