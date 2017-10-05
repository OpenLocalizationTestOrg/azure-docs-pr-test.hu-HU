---
title: "Univerzális Windows-alkalmazások SDK tartalma"
description: "Az Azure Mobile Engagement univerzális alkalmazások Windows SDK tartalmát megismerése"
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
ms.openlocfilehash: b28d525ab16487b963772e23fdecd11f94dcabd1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-sdk-content"></a><span data-ttu-id="ca42c-103">Univerzális Windows-alkalmazások SDK tartalma</span><span class="sxs-lookup"><span data-stu-id="ca42c-103">Windows Universal Apps SDK content</span></span>
<span data-ttu-id="ca42c-104">Ez a dokumentum, és ismerteti az SDK-t az alkalmazás által központilag telepített tartalom.</span><span class="sxs-lookup"><span data-stu-id="ca42c-104">This document lists and describes the content deployed by the SDK in your application.</span></span>

## <a name="the-resources-folder"></a><span data-ttu-id="ca42c-105">A `/Resources` mappa</span><span class="sxs-lookup"><span data-stu-id="ca42c-105">The `/Resources` folder</span></span>
<span data-ttu-id="ca42c-106">Ez a mappa tartalmaz, amelyet a Mobile Engagement összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="ca42c-106">This folder contains all the resources that Mobile Engagement needs.</span></span> <span data-ttu-id="ca42c-107">Testre is szabhatja azokat az alkalmazás megfelelően.</span><span class="sxs-lookup"><span data-stu-id="ca42c-107">You can also customize them to fit your app.</span></span>

* <span data-ttu-id="ca42c-108">`EngagementConfiguration.xml`: A Mobile Engagement konfigurációs fájlt, ahol testre szabhatja a Mobile Engagement-beállításokat (a Mobile Engagement kapcsolati karakterláncot, a jelentés összeomlási...) azt.</span><span class="sxs-lookup"><span data-stu-id="ca42c-108">`EngagementConfiguration.xml` : The Mobile Engagement's configuration file, this is where you can customize Mobile Engagement settings (Mobile Engagement connection string, report crash...).</span></span>

### <a name="html-folder"></a><span data-ttu-id="ca42c-109">/ HTML mappa</span><span class="sxs-lookup"><span data-stu-id="ca42c-109">/html folder</span></span>
* <span data-ttu-id="ca42c-110">`EngagementNotification.html`: A `Notification` html Nézetterv alkalmazásbeli transzparensek a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ca42c-110">`EngagementNotification.html` : The `Notification` web view html design for in-app banners.</span></span>
* <span data-ttu-id="ca42c-111">`EngagementAnnouncement.html`: A `Announcement` webalkalmazás-Nézetterv html az alkalmazáson belüli közbeszúrt nézeteket.</span><span class="sxs-lookup"><span data-stu-id="ca42c-111">`EngagementAnnouncement.html` : The `Announcement` web view html design for in-app interstitial views.</span></span>

### <a name="images-folder"></a><span data-ttu-id="ca42c-112">/Images mappa</span><span class="sxs-lookup"><span data-stu-id="ca42c-112">/images folder</span></span>
* <span data-ttu-id="ca42c-113">`EngagementIconNotification.png`: A márka ikonra a bal oldali, egy értesítés jelenik meg helyette a márka ikon jelzi.</span><span class="sxs-lookup"><span data-stu-id="ca42c-113">`EngagementIconNotification.png` : The brand icon displayed at the left of a notification, replace this one by your brand icon.</span></span>
* <span data-ttu-id="ca42c-114">`EngagementIconOk.png`: A `Ok` ikon a reach tartalom az egyesített a műveletet, vagy az Érvényesítés gombra.</span><span class="sxs-lookup"><span data-stu-id="ca42c-114">`EngagementIconOk.png` : The `Ok` icon of the reach content pages for the action or validation button.</span></span>
* <span data-ttu-id="ca42c-115">`EngagementIconNOK.png`: A `NOK` ikon használható, ha az Érvényesítés gombra reach tartalom oldalak le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="ca42c-115">`EngagementIconNOK.png` : The `NOK` icon used when the validation button of the reach content pages is disabled.</span></span>
* <span data-ttu-id="ca42c-116">`EngagementIconClose.png`: A `Close` ikon a reach-értesítések és a leállítási gomb tartalmát.</span><span class="sxs-lookup"><span data-stu-id="ca42c-116">`EngagementIconClose.png` : The `Close` icon of the reach notifications and contents for the dismiss button.</span></span>

### <a name="overlay-folder"></a><span data-ttu-id="ca42c-117">/overlay mappa</span><span class="sxs-lookup"><span data-stu-id="ca42c-117">/overlay folder</span></span>
* <span data-ttu-id="ca42c-118">`EngagementPageOverlay.cs`: Az átmeneti területre lap hozzáadása az Engagement felelős elérni alárendelt alkalmazáson belüli felhasználói Felületet.</span><span class="sxs-lookup"><span data-stu-id="ca42c-118">`EngagementPageOverlay.cs` : The overlay page responsible for adding the Engagement reach in-app UI to its child.</span></span>

