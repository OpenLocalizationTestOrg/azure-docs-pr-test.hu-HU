---
title: "Riasztási viselkedés aaaSMS művelet csoportok |} Microsoft Docs"
description: "SMS-üzenet formátuma és tooSMS üzenetek toounsubscribe válaszol, resubscribe, illetve segítséget kérni."
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
ms.openlocfilehash: 3cd09b1903e3472f6402f62b74409d97e7e7ea97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a><span data-ttu-id="a9161-103">SMS riasztási művelet csoportok működése</span><span class="sxs-lookup"><span data-stu-id="a9161-103">SMS Alert Behavior in Action Groups</span></span>
## <a name="overview"></a><span data-ttu-id="a9161-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a9161-104">Overview</span></span> ##
<span data-ttu-id="a9161-105">A művelet csoportok lehetővé teszik a tooconfigure fogadók listáját.</span><span class="sxs-lookup"><span data-stu-id="a9161-105">Action groups enable you tooconfigure a list of receivers.</span></span> <span data-ttu-id="a9161-106">Ezek a csoportok majd kihasználható, napló tevékenységriasztásokat; meghatározásakor Győződjön meg arról, hogy egy adott művelet csoport értesítést hello tevékenység napló figyelmeztetés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a9161-106">These groups can then be leveraged when defining activity log alerts; ensuring that a particular action group is notified when hello activity log alert is triggered.</span></span> <span data-ttu-id="a9161-107">Riasztási mechanizmusok támogatott hello egyik SMS; hello riasztások kétirányú kommunikáció támogatja.</span><span class="sxs-lookup"><span data-stu-id="a9161-107">One of hello alerting mechanisms supported is SMS; hello alerts support bi-directional communication.</span></span> <span data-ttu-id="a9161-108">A felhasználók válaszolhassanak tooan riasztást:</span><span class="sxs-lookup"><span data-stu-id="a9161-108">A user can respond tooan alert to:</span></span>

- <span data-ttu-id="a9161-109">**Riasztások lemondani:** A felhasználó az összes művelet csoportot, vagy egyes számú művelet csoport összes SMS riasztásokból lemondhatja.</span><span class="sxs-lookup"><span data-stu-id="a9161-109">**Unsubscribe from alerts:** A user can unsubscribe from all SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="a9161-110">**Resubscribe tooalerts:** egy felhasználó is resubscribe tooall SMS riasztások összes művelet csoportot, vagy egyes számú művelet csoport.</span><span class="sxs-lookup"><span data-stu-id="a9161-110">**Resubscribe tooalerts:** A user can resubscribe tooall SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="a9161-111">**Segítség kérése:** egy felhasználó hello SMS további információ teheti fel.</span><span class="sxs-lookup"><span data-stu-id="a9161-111">**Request help:** A user can ask for more information on hello SMS.</span></span> <span data-ttu-id="a9161-112">Átirányított toothis cikk fogja</span><span class="sxs-lookup"><span data-stu-id="a9161-112">They will be redirected toothis article</span></span>

<span data-ttu-id="a9161-113">Ez a cikk ismerteti hello SMS riasztások hello viselkedését, és hello válasz műveletek hello felhasználói vehet igénybe alapján hello területi hello felhasználó:</span><span class="sxs-lookup"><span data-stu-id="a9161-113">This article covers hello behavior of hello SMS alerts and hello response actions hello user can take based on hello locale of hello user:</span></span>

## <a name="usacanada-sms-behavior"></a><span data-ttu-id="a9161-114">Amerikai Egyesült Államok vagy Kanada SMS viselkedése</span><span class="sxs-lookup"><span data-stu-id="a9161-114">USA/Canada SMS behavior</span></span>
### <a name="receiving-an-sms-alert"></a><span data-ttu-id="a9161-115">SMS riasztást fogadása</span><span class="sxs-lookup"><span data-stu-id="a9161-115">Receiving an SMS Alert</span></span>
<span data-ttu-id="a9161-116">Az SMS fogadó, akik egy művelet csoport részeként van konfigurálva, az SMS kap, ha a riasztás akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="a9161-116">An SMS receiver, who is configured as part of an action group, will receive an SMS when an alert fires.</span></span> <span data-ttu-id="a9161-117">hello SMS fogja hajtani a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="a9161-117">hello SMS will carry hello following information:</span></span>
* <span data-ttu-id="a9161-118">Hello művelet csoport rövid_név ezt a figyelmeztetést küldött</span><span class="sxs-lookup"><span data-stu-id="a9161-118">Shortname of hello action group this alert was sent to</span></span>
- <span data-ttu-id="a9161-119">Cím hello riasztás</span><span class="sxs-lookup"><span data-stu-id="a9161-119">Title of hello alert</span></span>

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a><span data-ttu-id="a9161-120">Az SMS riasztásokból egy művelet csoport unsubscribing</span><span class="sxs-lookup"><span data-stu-id="a9161-120">Unsubscribing from SMS alerts for one action group</span></span>
<span data-ttu-id="a9161-121">A felhasználó leiratkozhat SMS az értesítések egy-egy művelettel csoport által válaszol toohello shortcode 20873 hello kulcsszavakkal: "LETILTÁSA &lt;művelet csoport rövid_név&gt;".</span><span class="sxs-lookup"><span data-stu-id="a9161-121">A user can unsubscribe from SMS for alerts for one action group by responding toohello shortcode 20873 with hello keywords: “DISABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="a9161-122">Példa</span><span class="sxs-lookup"><span data-stu-id="a9161-122">Ex.</span></span> <span data-ttu-id="a9161-123">Az SMS-toohello shortcode 20873, amely szerint "Azure tiltsa le a" hello rövid_név "Azure", a művelet csoportok riasztásaiból toounsubscribe kívánó felhasználó elküldése</span><span class="sxs-lookup"><span data-stu-id="a9161-123">A user wishing toounsubscribe from alerts for an action group with hello shortname “Azure”, would send an SMS toohello shortcode 20873 that says “DISABLE Azure”</span></span>

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a><span data-ttu-id="a9161-124">Az SMS riasztásokból az összes művelet csoport unsubscribing</span><span class="sxs-lookup"><span data-stu-id="a9161-124">Unsubscribing from SMS alerts for all action groups</span></span>
<span data-ttu-id="a9161-125">A felhasználó által válaszol toohello shortcode 20873 sem a következő kulcsszavak hello leiratkozhat az összes művelet csoport összes SMS riasztás:</span><span class="sxs-lookup"><span data-stu-id="a9161-125">A user can unsubscribe from all SMS alerts for all action groups by responding toohello shortcode 20873 with any of hello following keywords:</span></span>
* <span data-ttu-id="a9161-126">ÁLLJ</span><span class="sxs-lookup"><span data-stu-id="a9161-126">STOP</span></span>

<span data-ttu-id="a9161-127">Példa</span><span class="sxs-lookup"><span data-stu-id="a9161-127">Ex.</span></span> <span data-ttu-id="a9161-128">A felhasználó toounsubscribe az összes SMS riasztásokból az összes művelet csoport, amely az SMS-toohello shortcode 20873, amely szerint a "STOP" elküldése</span><span class="sxs-lookup"><span data-stu-id="a9161-128">A user wishing toounsubscribe from all SMS alerts for all action groups, would send an SMS toohello shortcode 20873 that says “STOP”</span></span>

>[!NOTE]
><span data-ttu-id="a9161-129">Ha egy felhasználó leiratkozott a SMS riasztást küld, de kerül tooa új művelet csoport; RENDSZER SMS figyelmeztetést a művelet új csoport, hanem marad a művelet minden olyan csoportból korábbi művelet.</span><span class="sxs-lookup"><span data-stu-id="a9161-129">If a user has unsubscribed from SMS alerts, but is then added tooa new action group; they WILL receive SMS alerts for that new action group, but remain unsubscribed from all previous action groups.</span></span>
>
>

### <a name="resubscribing-toosms-alerts-for-one-action-group"></a><span data-ttu-id="a9161-130">Egy művelet csoport resubscribing tooSMS riasztások</span><span class="sxs-lookup"><span data-stu-id="a9161-130">Resubscribing tooSMS alerts for one action group</span></span>
<span data-ttu-id="a9161-131">A felhasználó által válaszol toohello shortcode 20873 hello kulcsszavakkal is resubscribe tooSMS az értesítések egy-egy művelettel csoport: "engedélyezése &lt;művelet csoport rövid_név&gt;".</span><span class="sxs-lookup"><span data-stu-id="a9161-131">A user can resubscribe tooSMS for alerts for one action group by responding toohello shortcode 20873 with hello keywords: “ENABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="a9161-132">Példa</span><span class="sxs-lookup"><span data-stu-id="a9161-132">Ex.</span></span> <span data-ttu-id="a9161-133">A felhasználó tooresubscribe tooalerts egy művelet csoport a hello rövid_név "Azure", amely az SMS-toohello shortcode 20873, amely szerint "Engedélyezése Azure" elküldése</span><span class="sxs-lookup"><span data-stu-id="a9161-133">A user wishing tooresubscribe tooalerts for an action group with hello shortname “Azure”, would send an SMS toohello shortcode 20873 that says “ENABLE Azure”</span></span>

### <a name="resubscribing-toosms-alerts-for-all-action-groups"></a><span data-ttu-id="a9161-134">Az összes művelet csoport resubscribing tooSMS riasztások</span><span class="sxs-lookup"><span data-stu-id="a9161-134">Resubscribing tooSMS alerts for all action groups</span></span>
<span data-ttu-id="a9161-135">A felhasználó által válaszol toohello shortcode 20873 sem a következő kulcsszavak hello is resubscribe tooall SMS az összes művelet csoport riasztásokhoz:</span><span class="sxs-lookup"><span data-stu-id="a9161-135">A user can resubscribe tooall SMS for alerts for all action groups by responding toohello shortcode 20873 with any of hello following keywords:</span></span>

* <span data-ttu-id="a9161-136">INDÍTSA EL</span><span class="sxs-lookup"><span data-stu-id="a9161-136">START</span></span>

<span data-ttu-id="a9161-137">Példa</span><span class="sxs-lookup"><span data-stu-id="a9161-137">Ex.</span></span> <span data-ttu-id="a9161-138">A felhasználó toounsubscribe az összes SMS riasztásokból az összes művelet csoport, amely az SMS-toohello shortcode 20873, amely szerint a "START" elküldése</span><span class="sxs-lookup"><span data-stu-id="a9161-138">A user wishing toounsubscribe from all SMS alerts for all action groups, would send an SMS toohello shortcode 20873 that says “START”</span></span>

### <a name="requesting-help-via-sms"></a><span data-ttu-id="a9161-139">SMS segítségkérés</span><span class="sxs-lookup"><span data-stu-id="a9161-139">Requesting help via SMS</span></span>
<span data-ttu-id="a9161-140">A felhasználó hello SMS további információ teheti fel, kaptak a következő kulcsszavak hello egyik válaszol toohello shortcode 20873 által:</span><span class="sxs-lookup"><span data-stu-id="a9161-140">A user can ask for more information about hello SMS they have received by responding toohello shortcode 20873 with any of hello following keywords:</span></span>
* <span data-ttu-id="a9161-141">SEGÍTSÉG</span><span class="sxs-lookup"><span data-stu-id="a9161-141">HELP</span></span>

<span data-ttu-id="a9161-142">A hivatkozás toothis cikk toohello felhasználó küld egy választ.</span><span class="sxs-lookup"><span data-stu-id="a9161-142">A response will be sent toohello user with a link toothis article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9161-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9161-143">Next Steps</span></span>
<span data-ttu-id="a9161-144">Első egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md) és megtudhatja, hogyan tooget riasztást</span><span class="sxs-lookup"><span data-stu-id="a9161-144">Get an [overview of activity log alerts](monitoring-overview-alerts.md) and learn how tooget alerted</span></span>  
<span data-ttu-id="a9161-145">További információ [SMS sebesség korlátozása](monitoring-alerts-rate-limiting.md)</span><span class="sxs-lookup"><span data-stu-id="a9161-145">Learn more about [SMS rate limiting](monitoring-alerts-rate-limiting.md)</span></span>  
<span data-ttu-id="a9161-146">További információ [művelet csoportok](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="a9161-146">Learn more about [action groups](monitoring-action-groups.md)</span></span>
