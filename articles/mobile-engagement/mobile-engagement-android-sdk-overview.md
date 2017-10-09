---
title: "Azure Mobile Engagement SDK-integráció aaaAndroid"
description: "Ismerteti, hogyan toointegrate Azure Mobile Engagement SDK az Android-alkalmazások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a91ed04f-f3ce-4692-a6dd-b56a28d7dee8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo;ricksal
ms.openlocfilehash: 0c63bfaf673abbda7ea498390f8282c43e2fb8df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="android-sdk-integration-for-azure-mobile-engagement"></a><span data-ttu-id="587fa-103">Az Azure Mobile Engagement Android SDK-integráció</span><span class="sxs-lookup"><span data-stu-id="587fa-103">Android SDK Integration for Azure Mobile Engagement</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="587fa-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="587fa-104">Universal Windows</span></span>](mobile-engagement-windows-store-sdk-overview.md)
> * [<span data-ttu-id="587fa-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="587fa-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-sdk-overview.md)
> * [<span data-ttu-id="587fa-106">iOS</span><span class="sxs-lookup"><span data-stu-id="587fa-106">iOS</span></span>](mobile-engagement-ios-sdk-overview.md)
> * [<span data-ttu-id="587fa-107">Android</span><span class="sxs-lookup"><span data-stu-id="587fa-107">Android</span></span>](mobile-engagement-android-sdk-overview.md)
> 
> 

<span data-ttu-id="587fa-108">Ez a dokumentum ismerteti az összes hello integrációs és konfigurációs lehetőségek érhető el az Azure Mobile Engagement Android SDK-t.</span><span class="sxs-lookup"><span data-stu-id="587fa-108">This document describes all hello integration and configuration options available for Azure Mobile Engagement Android SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="587fa-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="587fa-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a><span data-ttu-id="587fa-110">A speciális funkciók</span><span class="sxs-lookup"><span data-stu-id="587fa-110">Advanced Features</span></span>
### <a name="reporting-features"></a><span data-ttu-id="587fa-111">Jelentési szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="587fa-111">Reporting Features</span></span>
<span data-ttu-id="587fa-112">Ezeket a szolgáltatásokat is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="587fa-112">You can add these features:</span></span>

1. [<span data-ttu-id="587fa-113">Speciális jelentéskészítési beállítások</span><span class="sxs-lookup"><span data-stu-id="587fa-113">Advanced reporting options</span></span>](mobile-engagement-android-advanced-reporting.md)
2. [<span data-ttu-id="587fa-114">Hely jelentéskészítési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="587fa-114">Location Reporting options</span></span>](mobile-engagement-android-location-reporting.md)
3. [<span data-ttu-id="587fa-115">Speciális konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="587fa-115">Advanced Configuration options</span></span>](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a><span data-ttu-id="587fa-116">Értesítések:</span><span class="sxs-lookup"><span data-stu-id="587fa-116">Notifications:</span></span>
[<span data-ttu-id="587fa-117">Hogyan toointegrate Reach (értesítések) a Android-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="587fa-117">How toointegrate Reach (Notifications) in your Android app</span></span>](mobile-engagement-android-integrate-engagement-reach.md)

1. <span data-ttu-id="587fa-118">Google Cloud Messaging (GCM): [hogyan tooIntegrate a GCM használata a Mobile Engagement](mobile-engagement-android-gcm-integrate.md)</span><span class="sxs-lookup"><span data-stu-id="587fa-118">Google Cloud Messaging (GCM): [How tooIntegrate GCM with Mobile Engagement](mobile-engagement-android-gcm-integrate.md)</span></span>
2. <span data-ttu-id="587fa-119">Amazon eszköz Messaging (ADM): [hogyan tooIntegrate ADM a Mobile Engagement](mobile-engagement-android-adm-integrate.md)</span><span class="sxs-lookup"><span data-stu-id="587fa-119">Amazon Device Messaging (ADM): [How tooIntegrate ADM with Mobile Engagement](mobile-engagement-android-adm-integrate.md)</span></span>

### <a name="tag-plan-implementation"></a><span data-ttu-id="587fa-120">Címke terv végrehajtására:</span><span class="sxs-lookup"><span data-stu-id="587fa-120">Tag plan implementation:</span></span>
[<span data-ttu-id="587fa-121">Hogyan toouse hello speciális a Mobile Engagement az Android-alkalmazás szerinti címkézését API</span><span class="sxs-lookup"><span data-stu-id="587fa-121">How toouse hello advanced Mobile Engagement tagging API in your Android app</span></span>](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a><span data-ttu-id="587fa-122">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="587fa-122">Release notes</span></span>

### <a name="431-07172017"></a><span data-ttu-id="587fa-123">4.3.1 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="587fa-123">4.3.1 (07/17/2017)</span></span>
* <span data-ttu-id="587fa-124">Javítsa ki, amelyek ritkán fordulhat elő, amikor a program meghívja crash `EngagementAgentUtils.isInDedicatedEngagementProcess`, amely is használják hello `EngagementApplication` osztály.</span><span class="sxs-lookup"><span data-stu-id="587fa-124">Fix a crash that could rarely happen when calling `EngagementAgentUtils.isInDedicatedEngagementProcess`, which is also used by hello `EngagementApplication` class.</span></span>

### <a name="430-06272017"></a><span data-ttu-id="587fa-125">4.3.0 (06/27/2017)</span><span class="sxs-lookup"><span data-stu-id="587fa-125">4.3.0 (06/27/2017)</span></span>
* <span data-ttu-id="587fa-126">Android 8 támogatása (korábbi verzióiban hello SDK Android 8 nem fog működni).</span><span class="sxs-lookup"><span data-stu-id="587fa-126">Android 8 support (previous versions of hello SDK will not work on Android 8).</span></span>
* <span data-ttu-id="587fa-127">Nincs több függőség támogatási függvénytára.</span><span class="sxs-lookup"><span data-stu-id="587fa-127">No more dependency on support library.</span></span>
* <span data-ttu-id="587fa-128">Távolítsa el `EngagementFragmentActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="587fa-128">Remove `EngagementFragmentActivity` class.</span></span>
* <span data-ttu-id="587fa-129">Esedékes túl[háttér végrehajtási korlátozások](https://developer.android.com/preview/features/background.html) Android 8, a háttér-naplók előfordulhat, hogy később, amíg hello felhasználó hello eszköz kommunikál, ez hatással lesz a leküldéses kampány **kézbesített** és **Rendszerértesítő jelenik meg** Ha hello eszköz alvó volt folyamatban késleltetett statisztika (hello értesítési továbbra is megjelenik, fog gyűrű és mozog hibák nélkül valós időben).</span><span class="sxs-lookup"><span data-stu-id="587fa-129">Due too[Background Execution Limits](https://developer.android.com/preview/features/background.html) on Android 8, logs in background might be delayed until hello user interacts with hello device, this will have an impact on Push Campaign **Delivered** and **System notification displayed** statistics being delayed if hello device was sleeping (hello notification will still be displayed, will ring and vibrate in real time without issues).</span></span>
* <span data-ttu-id="587fa-130">Esedékes túl[háttér hely korlátok](https://developer.android.com/preview/features/background-location-limits.html), háttérben hello valós idejű hely nem fog frissülni gyakran Android 8.</span><span class="sxs-lookup"><span data-stu-id="587fa-130">Due too[Background Location Limits](https://developer.android.com/preview/features/background-location-limits.html), hello real time location in background will not be updated frequently on Android 8.</span></span>

<span data-ttu-id="587fa-131">Minden verzióiért lásd: hello [végezze el a kibocsátási megjegyzések](mobile-engagement-android-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="587fa-131">For all versions, see hello [complete release notes](mobile-engagement-android-release-notes.md).</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="587fa-132">Frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="587fa-132">Upgrade procedures</span></span>
<span data-ttu-id="587fa-133">Ha Ön már rendelkezik integrálva egy régebbi verzióját az SDK az alkalmazás, tekintse meg [frissítése eljárások](mobile-engagement-android-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="587fa-133">If you already have integrated an older version of our SDK into your application, consult [Upgrade Procedures](mobile-engagement-android-upgrade-procedure.md).</span></span>

