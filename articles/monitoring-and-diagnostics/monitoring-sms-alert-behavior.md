---
title: "SMS riasztási viselkedésnek a művelet csoportok |} Microsoft Docs"
description: "SMS-üzenet formátuma és ad választ az SMS-üzenetek mondhatja, resubscribe, illetve segítséget kérni."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 3e4eca174209eeb9cbce1d45111d1e5cc30af8b0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a><span data-ttu-id="78a11-103">SMS riasztási művelet csoportok működése</span><span class="sxs-lookup"><span data-stu-id="78a11-103">SMS Alert Behavior in Action Groups</span></span>
## <a name="overview"></a><span data-ttu-id="78a11-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="78a11-104">Overview</span></span> ##
<span data-ttu-id="78a11-105">A művelet csoportok lehetővé teszik a fogadók listájának konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="78a11-105">Action groups enable you to configure a list of receivers.</span></span> <span data-ttu-id="78a11-106">Ezek a csoportok majd kihasználható, napló tevékenységriasztásokat; meghatározásakor Győződjön meg arról, hogy egy adott művelet csoport értesítést kaphat a tevékenység napló figyelmeztetés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="78a11-106">These groups can then be leveraged when defining activity log alerts; ensuring that a particular action group is notified when the activity log alert is triggered.</span></span> <span data-ttu-id="78a11-107">A támogatott riasztási mechanizmus egyik SMS; a riasztások kétirányú kommunikáció támogatja.</span><span class="sxs-lookup"><span data-stu-id="78a11-107">One of the alerting mechanisms supported is SMS; the alerts support bi-directional communication.</span></span> <span data-ttu-id="78a11-108">A felhasználók válaszolhassanak riasztást:</span><span class="sxs-lookup"><span data-stu-id="78a11-108">A user can respond to an alert to:</span></span>

- <span data-ttu-id="78a11-109">**Riasztások lemondani:** A felhasználó az összes művelet csoportot, vagy egyes számú művelet csoport összes SMS riasztásokból lemondhatja.</span><span class="sxs-lookup"><span data-stu-id="78a11-109">**Unsubscribe from alerts:** A user can unsubscribe from all SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="78a11-110">**A riasztások resubscribe:** A felhasználó az összes művelet csoportot, vagy egyes számú művelet csoport összes SMS riasztásnál is resubscribe.</span><span class="sxs-lookup"><span data-stu-id="78a11-110">**Resubscribe to alerts:** A user can resubscribe to all SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="78a11-111">**Segítség kérése:** A felhasználó további információt az SMS teheti fel.</span><span class="sxs-lookup"><span data-stu-id="78a11-111">**Request help:** A user can ask for more information on the SMS.</span></span> <span data-ttu-id="78a11-112">A rendszer átirányítja a cikkhez</span><span class="sxs-lookup"><span data-stu-id="78a11-112">They will be redirected to this article</span></span>

<span data-ttu-id="78a11-113">Ez a cikk ismerteti az SMS-riasztások viselkedését, és a válasz műveletek a felhasználói vehet igénybe alapján a felhasználó területi:</span><span class="sxs-lookup"><span data-stu-id="78a11-113">This article covers the behavior of the SMS alerts and the response actions the user can take based on the locale of the user:</span></span>

## <a name="usacanada-sms-behavior"></a><span data-ttu-id="78a11-114">Amerikai Egyesült Államok vagy Kanada SMS viselkedése</span><span class="sxs-lookup"><span data-stu-id="78a11-114">USA/Canada SMS behavior</span></span>
### <a name="receiving-an-sms-alert"></a><span data-ttu-id="78a11-115">SMS riasztást fogadása</span><span class="sxs-lookup"><span data-stu-id="78a11-115">Receiving an SMS Alert</span></span>
<span data-ttu-id="78a11-116">Az SMS fogadó, akik egy művelet csoport részeként van konfigurálva, az SMS kap, ha a riasztás akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="78a11-116">An SMS receiver, who is configured as part of an action group, will receive an SMS when an alert fires.</span></span> <span data-ttu-id="78a11-117">Az SMS fogja hajtani a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="78a11-117">The SMS will carry the following information:</span></span>
* <span data-ttu-id="78a11-118">A művelet csoport rövid_név ezt a figyelmeztetést küldött</span><span class="sxs-lookup"><span data-stu-id="78a11-118">Shortname of the action group this alert was sent to</span></span>
- <span data-ttu-id="78a11-119">A riasztás nevét</span><span class="sxs-lookup"><span data-stu-id="78a11-119">Title of the alert</span></span>

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a><span data-ttu-id="78a11-120">Az SMS riasztásokból egy művelet csoport unsubscribing</span><span class="sxs-lookup"><span data-stu-id="78a11-120">Unsubscribing from SMS alerts for one action group</span></span>
<span data-ttu-id="78a11-121">A felhasználó leiratkozhat SMS az értesítések egy-egy művelettel csoport által a shortcode 20873 a kulcsszavakkal válaszol: "LETILTÁSA &lt;művelet csoport rövid_név&gt;".</span><span class="sxs-lookup"><span data-stu-id="78a11-121">A user can unsubscribe from SMS for alerts for one action group by responding to the shortcode 20873 with the keywords: “DISABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="78a11-122">Példa</span><span class="sxs-lookup"><span data-stu-id="78a11-122">Ex.</span></span> <span data-ttu-id="78a11-123">Egy művelet csoport a rövid_név "Azure", a riasztások lemondani kívánó felhasználó SMS, amely szerint "Azure tiltsa le a" shortcode 20873 volna küldése</span><span class="sxs-lookup"><span data-stu-id="78a11-123">A user wishing to unsubscribe from alerts for an action group with the shortname “Azure”, would send an SMS to the shortcode 20873 that says “DISABLE Azure”</span></span>

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a><span data-ttu-id="78a11-124">Az SMS riasztásokból az összes művelet csoport unsubscribing</span><span class="sxs-lookup"><span data-stu-id="78a11-124">Unsubscribing from SMS alerts for all action groups</span></span>
<span data-ttu-id="78a11-125">A felhasználó válaszol a shortcode 20873 a következő kulcsszavak egyikét a leiratkozhat az összes művelet csoport összes SMS riasztás:</span><span class="sxs-lookup"><span data-stu-id="78a11-125">A user can unsubscribe from all SMS alerts for all action groups by responding to the shortcode 20873 with any of the following keywords:</span></span>
* <span data-ttu-id="78a11-126">ÁLLJ</span><span class="sxs-lookup"><span data-stu-id="78a11-126">STOP</span></span>

<span data-ttu-id="78a11-127">Példa</span><span class="sxs-lookup"><span data-stu-id="78a11-127">Ex.</span></span> <span data-ttu-id="78a11-128">A felhasználó az összes művelet csoport összes SMS riasztás lemondani kívánó, amely szerint a "STOP" shortcode 20873 SMS volna küldése</span><span class="sxs-lookup"><span data-stu-id="78a11-128">A user wishing to unsubscribe from all SMS alerts for all action groups, would send an SMS to the shortcode 20873 that says “STOP”</span></span>

>[!NOTE]
><span data-ttu-id="78a11-129">Ha egy felhasználó leiratkozott a SMS riasztást küld, de egy új művelet csoportba kerül RENDSZER SMS figyelmeztetést a művelet új csoport, hanem marad a művelet minden olyan csoportból korábbi művelet.</span><span class="sxs-lookup"><span data-stu-id="78a11-129">If a user has unsubscribed from SMS alerts, but is then added to a new action group; they WILL receive SMS alerts for that new action group, but remain unsubscribed from all previous action groups.</span></span>
>
>

### <a name="resubscribing-to-sms-alerts-for-one-action-group"></a><span data-ttu-id="78a11-130">A művelet egy csoport SMS riasztásokat resubscribing</span><span class="sxs-lookup"><span data-stu-id="78a11-130">Resubscribing to SMS alerts for one action group</span></span>
<span data-ttu-id="78a11-131">A felhasználó is resubscribe SMS az értesítések egy-egy művelettel csoport által a shortcode 20873 a kulcsszavakkal válaszol: "engedélyezése &lt;művelet csoport rövid_név&gt;".</span><span class="sxs-lookup"><span data-stu-id="78a11-131">A user can resubscribe to SMS for alerts for one action group by responding to the shortcode 20873 with the keywords: “ENABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="78a11-132">Példa</span><span class="sxs-lookup"><span data-stu-id="78a11-132">Ex.</span></span> <span data-ttu-id="78a11-133">A riasztásokat a rövid_név "Azure", a művelet csoportok resubscribe kívánó felhasználó volna SMS küldése a shortcode 20873, amely szerint "Azure engedélyezése"</span><span class="sxs-lookup"><span data-stu-id="78a11-133">A user wishing to resubscribe to alerts for an action group with the shortname “Azure”, would send an SMS to the shortcode 20873 that says “ENABLE Azure”</span></span>

### <a name="resubscribing-to-sms-alerts-for-all-action-groups"></a><span data-ttu-id="78a11-134">Az összes művelet csoport SMS riasztás resubscribing</span><span class="sxs-lookup"><span data-stu-id="78a11-134">Resubscribing to SMS alerts for all action groups</span></span>
<span data-ttu-id="78a11-135">A felhasználó által válaszol a shortcode 20873 a következő kulcsszavak egyikét a riasztások az összes művelet csoport összes SMS is resubscribe:</span><span class="sxs-lookup"><span data-stu-id="78a11-135">A user can resubscribe to all SMS for alerts for all action groups by responding to the shortcode 20873 with any of the following keywords:</span></span>

* <span data-ttu-id="78a11-136">INDÍTSA EL</span><span class="sxs-lookup"><span data-stu-id="78a11-136">START</span></span>

<span data-ttu-id="78a11-137">Példa</span><span class="sxs-lookup"><span data-stu-id="78a11-137">Ex.</span></span> <span data-ttu-id="78a11-138">A felhasználó az összes művelet csoport összes SMS riasztás lemondani kívánó, amely szerint a "START" shortcode 20873 SMS volna küldése</span><span class="sxs-lookup"><span data-stu-id="78a11-138">A user wishing to unsubscribe from all SMS alerts for all action groups, would send an SMS to the shortcode 20873 that says “START”</span></span>

### <a name="requesting-help-via-sms"></a><span data-ttu-id="78a11-139">SMS segítségkérés</span><span class="sxs-lookup"><span data-stu-id="78a11-139">Requesting help via SMS</span></span>
<span data-ttu-id="78a11-140">A felhasználó további információt az SMS válaszol a shortcode 20873 a következő kulcsszavak egyikét a kaptak teheti fel:</span><span class="sxs-lookup"><span data-stu-id="78a11-140">A user can ask for more information about the SMS they have received by responding to the shortcode 20873 with any of the following keywords:</span></span>
* <span data-ttu-id="78a11-141">SEGÍTSÉG</span><span class="sxs-lookup"><span data-stu-id="78a11-141">HELP</span></span>

<span data-ttu-id="78a11-142">Választ a felhasználó egy hivatkozás, ez a cikk kapnak.</span><span class="sxs-lookup"><span data-stu-id="78a11-142">A response will be sent to the user with a link to this article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="78a11-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78a11-143">Next Steps</span></span>
<span data-ttu-id="78a11-144">Első egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md) és megtudhatja, hogyan küldjön riasztást Önnek</span><span class="sxs-lookup"><span data-stu-id="78a11-144">Get an [overview of activity log alerts](monitoring-overview-alerts.md) and learn how to get alerted</span></span>  
<span data-ttu-id="78a11-145">További információ [SMS sebesség korlátozása](monitoring-alerts-rate-limiting.md)</span><span class="sxs-lookup"><span data-stu-id="78a11-145">Learn more about [SMS rate limiting](monitoring-alerts-rate-limiting.md)</span></span>  
<span data-ttu-id="78a11-146">További információ [művelet csoportok](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="78a11-146">Learn more about [action groups](monitoring-action-groups.md)</span></span>
