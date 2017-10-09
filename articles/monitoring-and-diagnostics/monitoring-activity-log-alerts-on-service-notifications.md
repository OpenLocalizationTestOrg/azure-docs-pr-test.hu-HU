---
title: "napló tevékenységriasztásokat aaaReceive szolgáltatás értesítések |} Microsoft Docs"
description: "Értesítés SMS, e-mailben vagy webhook Azure-szolgáltatás esetén."
author: johnkemnetz
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
ms.author: johnkem
ms.openlocfilehash: dd35e8f39d2a522efdae4dfed20779c992c1dd27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a><span data-ttu-id="0d761-103">A szolgáltatás értesítések napló riasztások tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d761-103">Create activity log alerts on service notifications</span></span>
## <a name="overview"></a><span data-ttu-id="0d761-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0d761-104">Overview</span></span>
<span data-ttu-id="0d761-105">Ez a cikk bemutatja, hogyan tooset tevékenységnapló be riasztásokat a szolgáltatás állapotával kapcsolatos értesítésekre hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="0d761-105">This article shows you how tooset up activity log alerts for service health notifications by using hello Azure portal.</span></span>  

<span data-ttu-id="0d761-106">Akkor is figyelmezteti, ha Azure szolgáltatás állapota értesítések tooyour Azure-előfizetés küld.</span><span class="sxs-lookup"><span data-stu-id="0d761-106">You can receive an alert when Azure sends service health notifications tooyour Azure subscription.</span></span> <span data-ttu-id="0d761-107">Hello riasztás alapján konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="0d761-107">You can configure hello alert based on:</span></span>

- <span data-ttu-id="0d761-108">hello-osztály szolgáltatás állapota (incidens, karbantartási, stb.).</span><span class="sxs-lookup"><span data-stu-id="0d761-108">hello class of service health notification (incident, maintenance, information, etc.).</span></span>
- <span data-ttu-id="0d761-109">hello Services-szolgáltatás(ok) hatással.</span><span class="sxs-lookup"><span data-stu-id="0d761-109">hello service(s) affected.</span></span>
- <span data-ttu-id="0d761-110">hello régió(k) hatással.</span><span class="sxs-lookup"><span data-stu-id="0d761-110">hello region(s) affected.</span></span>
- <span data-ttu-id="0d761-111">hello állapota hello értesítés (aktív vagy megoldott).</span><span class="sxs-lookup"><span data-stu-id="0d761-111">hello status of hello notification (active vs. resolved).</span></span>
- <span data-ttu-id="0d761-112">hello szintje hello értesítések (információs, figyelmeztetési, hiba).</span><span class="sxs-lookup"><span data-stu-id="0d761-112">hello level of hello notifications (informational, warning, error).</span></span>

<span data-ttu-id="0d761-113">Is konfigurálhatja, akik hello riasztást küldjön el:</span><span class="sxs-lookup"><span data-stu-id="0d761-113">You also can configure who hello alert should be sent to:</span></span>

- <span data-ttu-id="0d761-114">Válasszon egy meglévő művelet csoportot.</span><span class="sxs-lookup"><span data-stu-id="0d761-114">Select an existing action group.</span></span>
- <span data-ttu-id="0d761-115">Hozzon létre egy új művelet (riasztást használható).</span><span class="sxs-lookup"><span data-stu-id="0d761-115">Create a new action group (that can be used for future alerts).</span></span>

<span data-ttu-id="0d761-116">További információk a művelet csoportok toolearn lásd: [létrehozása és művelet csoportok kezelése](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="0d761-116">toolearn more about action groups, see [Create and manage action groups](monitoring-action-groups.md).</span></span>

<span data-ttu-id="0d761-117">Hogyan tooconfigure szolgáltatás állapotának értesítési riasztást küld, Azure Resource Manager-sablonok használatával kapcsolatos tudnivalókért lásd: [Resource Manager-sablonok](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="0d761-117">For information on how tooconfigure service health notification alerts by using Azure Resource Manager templates, see [Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="0d761-118">Riasztás létrehozása egy új művelet csoport Állapotfigyelő szolgáltatás értesítést a hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="0d761-118">Create an alert on a service health notification for a new action group by using hello Azure portal</span></span>
1. <span data-ttu-id="0d761-119">A hello [portal](https://portal.azure.com), jelölje be **figyelő**.</span><span class="sxs-lookup"><span data-stu-id="0d761-119">In hello [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![hello "Figyelés" szolgáltatás](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. <span data-ttu-id="0d761-121">A hello **tevékenységnapló** szakaszban jelölje be **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="0d761-121">In hello **Activity log** section, select **Alerts**.</span></span>

    ![hello "Értesítések" lapon](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. <span data-ttu-id="0d761-123">Válassza ki **Hozzáadás figyelmeztetés a napló**, és töltse ki hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="0d761-123">Select **Add activity log alert**, and fill in hello fields.</span></span>

    ![hello "Figyelmeztetés a napló hozzáadása" parancs](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. <span data-ttu-id="0d761-125">Adjon meg egy nevet a hello **tevékenység napló riasztás neve** mezőbe, majd adjon meg egy **leírás**.</span><span class="sxs-lookup"><span data-stu-id="0d761-125">Enter a name in hello **Activity log alert name** box, and provide a **Description**.</span></span>

    ![hello "Figyelmeztetés a napló hozzáadása" párbeszédpanel](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. <span data-ttu-id="0d761-127">Hello **előfizetés** a jelenlegi előfizetés autofills mezőben.</span><span class="sxs-lookup"><span data-stu-id="0d761-127">hello **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="0d761-128">Ez az előfizetés használt toosave hello napló figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="0d761-128">This subscription is used toosave hello activity log alert.</span></span> <span data-ttu-id="0d761-129">hello riasztási erőforrás hello műveletnaplóban az előfizetés és a figyelők események telepített toothis.</span><span class="sxs-lookup"><span data-stu-id="0d761-129">hello alert resource is deployed toothis subscription and monitors events in hello activity log for it.</span></span>

6. <span data-ttu-id="0d761-130">Jelölje be hello **erőforráscsoport** mely hello a riasztás létrehozása történt meg.</span><span class="sxs-lookup"><span data-stu-id="0d761-130">Select hello **Resource group** in which hello alert resource is created.</span></span> <span data-ttu-id="0d761-131">Ez nem hello erőforráscsoport, amelyet az hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="0d761-131">This isn't hello resource group that's monitored by hello alert.</span></span> <span data-ttu-id="0d761-132">Ehelyett hello erőforráscsoport, ahol hello riasztási erőforrás is található.</span><span class="sxs-lookup"><span data-stu-id="0d761-132">Instead, it's hello resource group where hello alert resource is located.</span></span>

7. <span data-ttu-id="0d761-133">A hello **eseménykategória** mezőben válassza **szolgáltatásának állapota**.</span><span class="sxs-lookup"><span data-stu-id="0d761-133">In hello **Event category** box, select **Service Health**.</span></span> <span data-ttu-id="0d761-134">Bejelölheti a hello **szolgáltatás**, **régió**, **típus**, **állapot**, és **szint** szolgáltatás állapotával kapcsolatos értesítésekre, amelyet az tooreceive.</span><span class="sxs-lookup"><span data-stu-id="0d761-134">Optionally, select hello **Service**, **Region**, **Type**, **Status**, and **Level** of service health notifications that you want tooreceive.</span></span>

8. <span data-ttu-id="0d761-135">A **keresztül riasztási**, jelölje be hello **új** művelet csoport gombra.</span><span class="sxs-lookup"><span data-stu-id="0d761-135">Under **Alert via**, select hello **New** action group button.</span></span> <span data-ttu-id="0d761-136">Adjon meg egy nevet a hello **művelet csoportnév** mezőbe, majd írjon be egy nevet a hello **rövid név** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="0d761-136">Enter a name in hello **Action group name** box, and enter a name in hello **Short name** box.</span></span> <span data-ttu-id="0d761-137">a riasztás aktiválódásakor közreműködésével küldött értesítő hello hivatkozik hello rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="0d761-137">hello short name is referenced in hello notifications that are sent when this alert fires.</span></span>

9. <span data-ttu-id="0d761-138">Adja meg a fogadók listáját hello fogadó megadásával:</span><span class="sxs-lookup"><span data-stu-id="0d761-138">Define a list of receivers by providing hello receiver's:</span></span>

    <span data-ttu-id="0d761-139">a.</span><span class="sxs-lookup"><span data-stu-id="0d761-139">a.</span></span> <span data-ttu-id="0d761-140">**Név**: Adja meg a hello fogadó alias, név vagy azonosító.</span><span class="sxs-lookup"><span data-stu-id="0d761-140">**Name**: Enter hello receiver’s name, alias, or identifier.</span></span>

    <span data-ttu-id="0d761-141">b.</span><span class="sxs-lookup"><span data-stu-id="0d761-141">b.</span></span> <span data-ttu-id="0d761-142">**Művelet típusa**: válassza ki az SMS, e-mailek vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="0d761-142">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="0d761-143">c.</span><span class="sxs-lookup"><span data-stu-id="0d761-143">c.</span></span> <span data-ttu-id="0d761-144">**Részletek**: hello művelet CustomProperty alapján, adjon meg egy telefonszám, e-mail címét vagy webhook URI.</span><span class="sxs-lookup"><span data-stu-id="0d761-144">**Details**: Based on hello action type chosen, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="0d761-145">Válassza ki **OK** toocreate hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="0d761-145">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="0d761-146">Néhány percen belül hello aktív és riasztás létrehozása során megadott hello feltételek alapján tootrigger kezdődik.</span><span class="sxs-lookup"><span data-stu-id="0d761-146">Within a few minutes, hello alert is active and begins tootrigger based on hello conditions you specified during creation.</span></span>

<span data-ttu-id="0d761-147">A napló tevékenységriasztásokat hello webhook sémája információkért lásd: [Webhookok Azure tevékenység naplózása riasztások](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="0d761-147">For information on hello webhook schema for activity log alerts, see [Webhooks for Azure activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="0d761-148">Ezeket a lépéseket definiált hello művelet csoport újrafelhasználható, az összes jövőbeni riasztási definíciók egy meglévő művelet csoportként.</span><span class="sxs-lookup"><span data-stu-id="0d761-148">hello action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="0d761-149">Hozzon létre egy riasztást az állapotfigyelő szolgáltatás értesítést meglévő művelet csoportok hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="0d761-149">Create an alert on a service health notification for an existing action group by using hello Azure portal</span></span>

1. <span data-ttu-id="0d761-150">Végezze el az 1-7 hello előző szakasz toocreate a szolgáltatás állapotának értesítési.</span><span class="sxs-lookup"><span data-stu-id="0d761-150">Follow steps 1 through 7 in hello previous section toocreate your service health notification.</span></span> 

2. <span data-ttu-id="0d761-151">A **keresztül riasztási**, jelölje be hello **meglévő** művelet csoport gombra.</span><span class="sxs-lookup"><span data-stu-id="0d761-151">Under **Alert via**, select hello **Existing** action group button.</span></span> <span data-ttu-id="0d761-152">Válassza ki hello megfelelő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0d761-152">Select hello appropriate action group.</span></span>

3. <span data-ttu-id="0d761-153">Válassza ki **OK** toocreate hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="0d761-153">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="0d761-154">Néhány percen belül hello aktív és riasztás létrehozása során megadott hello feltételek alapján tootrigger kezdődik.</span><span class="sxs-lookup"><span data-stu-id="0d761-154">Within a few minutes, hello alert is active and begins tootrigger based on hello conditions you specified during creation.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="0d761-155">A riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="0d761-155">Manage your alerts</span></span>

<span data-ttu-id="0d761-156">Riasztás létrehozása után is látható a hello **riasztások** hello szakasza **figyelő** panelen.</span><span class="sxs-lookup"><span data-stu-id="0d761-156">After you create an alert, it's visible in hello **Alerts** section of hello **Monitor** blade.</span></span> <span data-ttu-id="0d761-157">Jelölje ki azt szeretné, hogy toomanage hello riasztást:</span><span class="sxs-lookup"><span data-stu-id="0d761-157">Select hello alert you want toomanage to:</span></span>

* <span data-ttu-id="0d761-158">Szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="0d761-158">Edit it.</span></span>
* <span data-ttu-id="0d761-159">Törölje a parancsikont.</span><span class="sxs-lookup"><span data-stu-id="0d761-159">Delete it.</span></span>
* <span data-ttu-id="0d761-160">Tiltsa le, vagy engedélyezheti, ha folytatni hello riasztás értesítések fogadásának vagy tootemporarily le szeretné.</span><span class="sxs-lookup"><span data-stu-id="0d761-160">Disable or enable it, if you want tootemporarily stop or resume receiving notifications for hello alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d761-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d761-161">Next steps</span></span>
- <span data-ttu-id="0d761-162">További tudnivalók [szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="0d761-162">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="0d761-163">További tudnivalók [értesítési sebessége korlátozza az](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="0d761-163">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="0d761-164">Felülvizsgálati hello [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="0d761-164">Review hello [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="0d761-165">Lekérése egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md), és megtudhatja, hogyan tooreceive riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="0d761-165">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how tooreceive alerts.</span></span> 
- <span data-ttu-id="0d761-166">További információ [művelet csoportok](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="0d761-166">Learn more about [action groups](monitoring-action-groups.md).</span></span>
