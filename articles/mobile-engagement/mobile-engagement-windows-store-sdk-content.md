---
title: "Univerzális alkalmazások SDK tartalom aaaWindows"
description: "Az Azure Mobile Engagement univerzális Windows-alkalmazások SDK hello hello tartalmát megismerése"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8fa1b701-1c2b-4aec-940c-06c974ef5405
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: a8013d2433c0be62d737c8bc6e8360ed79bbe532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-sdk-content"></a><span data-ttu-id="903f2-103">Univerzális Windows-alkalmazások SDK tartalma</span><span class="sxs-lookup"><span data-stu-id="903f2-103">Windows Universal Apps SDK content</span></span>
<span data-ttu-id="903f2-104">Ez a dokumentum, és hello SDK az alkalmazás által telepített hello tartalom ismerteti.</span><span class="sxs-lookup"><span data-stu-id="903f2-104">This document lists and describes hello content deployed by hello SDK in your application.</span></span>

## <a name="hello-resources-folder"></a><span data-ttu-id="903f2-105">Hello `/Resources` mappa</span><span class="sxs-lookup"><span data-stu-id="903f2-105">hello `/Resources` folder</span></span>
<span data-ttu-id="903f2-106">Ez a mappa összes hello erőforrás, amelyet a Mobile Engagement tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="903f2-106">This folder contains all hello resources that Mobile Engagement needs.</span></span> <span data-ttu-id="903f2-107">Testre is szabhatja azokat toofit az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="903f2-107">You can also customize them toofit your app.</span></span>

* <span data-ttu-id="903f2-108">`EngagementConfiguration.xml`: a Mobile Engagement konfigurációs fájl hello, ez pedig, ahol testre szabhatja a Mobile Engagement-beállításokat (a Mobile Engagement kapcsolati karakterláncot, a jelentés összeomlási...).</span><span class="sxs-lookup"><span data-stu-id="903f2-108">`EngagementConfiguration.xml` : hello Mobile Engagement's configuration file, this is where you can customize Mobile Engagement settings (Mobile Engagement connection string, report crash...).</span></span>

### <a name="html-folder"></a><span data-ttu-id="903f2-109">/ HTML mappa</span><span class="sxs-lookup"><span data-stu-id="903f2-109">/html folder</span></span>
* <span data-ttu-id="903f2-110">`EngagementNotification.html`: hello `Notification` html Nézetterv alkalmazásbeli transzparensek a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="903f2-110">`EngagementNotification.html` : hello `Notification` web view html design for in-app banners.</span></span>
* <span data-ttu-id="903f2-111">`EngagementAnnouncement.html`: hello `Announcement` webalkalmazás-Nézetterv html az alkalmazáson belüli közbeszúrt nézeteket.</span><span class="sxs-lookup"><span data-stu-id="903f2-111">`EngagementAnnouncement.html` : hello `Announcement` web view html design for in-app interstitial views.</span></span>

### <a name="images-folder"></a><span data-ttu-id="903f2-112">/Images mappa</span><span class="sxs-lookup"><span data-stu-id="903f2-112">/images folder</span></span>
* <span data-ttu-id="903f2-113">`EngagementIconNotification.png`: hello márka ikon hello megjelenik egy értesítés bal oldali, helyette a márka ikon jelzi.</span><span class="sxs-lookup"><span data-stu-id="903f2-113">`EngagementIconNotification.png` : hello brand icon displayed at hello left of a notification, replace this one by your brand icon.</span></span>
* <span data-ttu-id="903f2-114">`EngagementIconOk.png`: hello `Ok` ikon hello reach tartalom az egyesített hello művelet vagy az Érvényesítés gombra.</span><span class="sxs-lookup"><span data-stu-id="903f2-114">`EngagementIconOk.png` : hello `Ok` icon of hello reach content pages for hello action or validation button.</span></span>
* <span data-ttu-id="903f2-115">`EngagementIconNOK.png`: hello `NOK` ikon használható, ha hello reach tartalom lapok hello érvényesítési gomb le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="903f2-115">`EngagementIconNOK.png` : hello `NOK` icon used when hello validation button of hello reach content pages is disabled.</span></span>
* <span data-ttu-id="903f2-116">`EngagementIconClose.png`: hello `Close` hello ikonjára eléri a értesítések, majd hello tartalmának elvetése gombra.</span><span class="sxs-lookup"><span data-stu-id="903f2-116">`EngagementIconClose.png` : hello `Close` icon of hello reach notifications and contents for hello dismiss button.</span></span>

### <a name="overlay-folder"></a><span data-ttu-id="903f2-117">/overlay mappa</span><span class="sxs-lookup"><span data-stu-id="903f2-117">/overlay folder</span></span>
* <span data-ttu-id="903f2-118">`EngagementPageOverlay.cs`: hello átfedő lap hozzáadása az Engagement reach alkalmazáson belüli felhasználói felület tooits gyermek hello felelős.</span><span class="sxs-lookup"><span data-stu-id="903f2-118">`EngagementPageOverlay.cs` : hello overlay page responsible for adding hello Engagement reach in-app UI tooits child.</span></span>

