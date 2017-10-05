---
title: "Tevékenység napló értesítéseket szolgáltatás értesítések |} Microsoft Docs"
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
ms.openlocfilehash: bf6a98fd7e7e11764bef174f9efd0635fa7efe9a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a><span data-ttu-id="2fab4-103">A szolgáltatás értesítések napló riasztások tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="2fab4-103">Create activity log alerts on service notifications</span></span>
## <a name="overview"></a><span data-ttu-id="2fab4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2fab4-104">Overview</span></span>
<span data-ttu-id="2fab4-105">Ez a cikk bemutatja, hogyan tevékenység napló riasztások a szolgáltatás állapotával kapcsolatos értesítésekre beállítása az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="2fab4-105">This article shows you how to set up activity log alerts for service health notifications by using the Azure portal.</span></span>  

<span data-ttu-id="2fab4-106">Akkor is figyelmezteti, ha Azure szolgáltatás állapotával kapcsolatos értesítésekre küld az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="2fab4-106">You can receive an alert when Azure sends service health notifications to your Azure subscription.</span></span> <span data-ttu-id="2fab4-107">A riasztás alapján konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="2fab4-107">You can configure the alert based on:</span></span>

- <span data-ttu-id="2fab4-108">A-osztály szolgáltatás állapota (incidens, karbantartási, stb.).</span><span class="sxs-lookup"><span data-stu-id="2fab4-108">The class of service health notification (incident, maintenance, information, etc.).</span></span>
- <span data-ttu-id="2fab4-109">Az érintett szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="2fab4-109">The service(s) affected.</span></span>
- <span data-ttu-id="2fab4-110">Az érintett régió(k) esetében.</span><span class="sxs-lookup"><span data-stu-id="2fab4-110">The region(s) affected.</span></span>
- <span data-ttu-id="2fab4-111">Az értesítés (aktív vagy megoldott) állapotát.</span><span class="sxs-lookup"><span data-stu-id="2fab4-111">The status of the notification (active vs. resolved).</span></span>
- <span data-ttu-id="2fab4-112">Az értesítések (információs, figyelmeztetési, hiba) szintjét.</span><span class="sxs-lookup"><span data-stu-id="2fab4-112">The level of the notifications (informational, warning, error).</span></span>

<span data-ttu-id="2fab4-113">Emellett konfigurálhatja ki a riasztást küldjön el:</span><span class="sxs-lookup"><span data-stu-id="2fab4-113">You also can configure who the alert should be sent to:</span></span>

- <span data-ttu-id="2fab4-114">Válasszon egy meglévő művelet csoportot.</span><span class="sxs-lookup"><span data-stu-id="2fab4-114">Select an existing action group.</span></span>
- <span data-ttu-id="2fab4-115">Hozzon létre egy új művelet (riasztást használható).</span><span class="sxs-lookup"><span data-stu-id="2fab4-115">Create a new action group (that can be used for future alerts).</span></span>

<span data-ttu-id="2fab4-116">Művelet csoportokkal kapcsolatos további tudnivalókért lásd: [létrehozása és kezelése a művelet](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="2fab4-116">To learn more about action groups, see [Create and manage action groups](monitoring-action-groups.md).</span></span>

<span data-ttu-id="2fab4-117">Állapotfigyelő értesítési szolgáltatásriasztások konfigurálása Azure Resource Manager-sablonok használatával kapcsolatos információkért lásd: [Resource Manager-sablonok](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="2fab4-117">For information on how to configure service health notification alerts by using Azure Resource Manager templates, see [Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a><span data-ttu-id="2fab4-118">Egy új művelet csoport Állapotfigyelő szolgáltatás értesítést a riasztás létrehozása az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="2fab4-118">Create an alert on a service health notification for a new action group by using the Azure portal</span></span>
1. <span data-ttu-id="2fab4-119">Az a [portal](https://portal.azure.com), jelölje be **figyelő**.</span><span class="sxs-lookup"><span data-stu-id="2fab4-119">In the [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![A "Figyelés" szolgáltatás](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. <span data-ttu-id="2fab4-121">Az a **tevékenységnapló** szakaszban jelölje be **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="2fab4-121">In the **Activity log** section, select **Alerts**.</span></span>

    ![Az "Értesítések" lapon](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. <span data-ttu-id="2fab4-123">Válassza ki **Hozzáadás figyelmeztetés a napló**, és töltse ki a mezőket.</span><span class="sxs-lookup"><span data-stu-id="2fab4-123">Select **Add activity log alert**, and fill in the fields.</span></span>

    ![A "Hozzáadás napló figyelmeztetés" parancs](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. <span data-ttu-id="2fab4-125">Írjon be egy nevet a **tevékenység napló riasztás neve** mezőbe, majd adjon meg egy **leírása**.</span><span class="sxs-lookup"><span data-stu-id="2fab4-125">Enter a name in the **Activity log alert name** box, and provide a **Description**.</span></span>

    ![A "Napló figyelmeztetés hozzáadása" párbeszédpanel](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. <span data-ttu-id="2fab4-127">A **előfizetés** a jelenlegi előfizetés autofills mezőben.</span><span class="sxs-lookup"><span data-stu-id="2fab4-127">The **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="2fab4-128">Ez az előfizetés tevékenységgel menthetők a napló figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="2fab4-128">This subscription is used to save the activity log alert.</span></span> <span data-ttu-id="2fab4-129">A riasztási erőforrás ebbe az előfizetésbe telepítve van, és az események a műveletnaplóban figyeli.</span><span class="sxs-lookup"><span data-stu-id="2fab4-129">The alert resource is deployed to this subscription and monitors events in the activity log for it.</span></span>

6. <span data-ttu-id="2fab4-130">Válassza ki a **erőforráscsoport** a riasztási erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2fab4-130">Select the **Resource group** in which the alert resource is created.</span></span> <span data-ttu-id="2fab4-131">Ez az erőforráscsoport, amelyet a riasztás nem található.</span><span class="sxs-lookup"><span data-stu-id="2fab4-131">This isn't the resource group that's monitored by the alert.</span></span> <span data-ttu-id="2fab4-132">Ehelyett az erőforráscsoportot, ahol a riasztás erőforrás.</span><span class="sxs-lookup"><span data-stu-id="2fab4-132">Instead, it's the resource group where the alert resource is located.</span></span>

7. <span data-ttu-id="2fab4-133">Az a **eseménykategória** mezőben válassza **szolgáltatásának állapota**.</span><span class="sxs-lookup"><span data-stu-id="2fab4-133">In the **Event category** box, select **Service Health**.</span></span> <span data-ttu-id="2fab4-134">Bejelölheti a **szolgáltatás**, **régió**, **típus**, **állapot**, és **szint** a szeretne kapni, a szolgáltatás állapotával kapcsolatos értesítésekre.</span><span class="sxs-lookup"><span data-stu-id="2fab4-134">Optionally, select the **Service**, **Region**, **Type**, **Status**, and **Level** of service health notifications that you want to receive.</span></span>

8. <span data-ttu-id="2fab4-135">A **keresztül riasztási**, jelölje be a **új** művelet csoport gombra.</span><span class="sxs-lookup"><span data-stu-id="2fab4-135">Under **Alert via**, select the **New** action group button.</span></span> <span data-ttu-id="2fab4-136">Adjon meg egy nevet a a **művelet csoportnév** mezőbe, majd írjon be egy nevet a **rövid név** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="2fab4-136">Enter a name in the **Action group name** box, and enter a name in the **Short name** box.</span></span> <span data-ttu-id="2fab4-137">A rövid nevét a riasztás aktiválódásakor közreműködésével küldött értesítő hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="2fab4-137">The short name is referenced in the notifications that are sent when this alert fires.</span></span>

9. <span data-ttu-id="2fab4-138">Adja meg a fogadók listáját a fogadó megadásával:</span><span class="sxs-lookup"><span data-stu-id="2fab4-138">Define a list of receivers by providing the receiver's:</span></span>

    <span data-ttu-id="2fab4-139">a.</span><span class="sxs-lookup"><span data-stu-id="2fab4-139">a.</span></span> <span data-ttu-id="2fab4-140">**Név**: Adja meg a fogadó alias, név vagy azonosító.</span><span class="sxs-lookup"><span data-stu-id="2fab4-140">**Name**: Enter the receiver’s name, alias, or identifier.</span></span>

    <span data-ttu-id="2fab4-141">b.</span><span class="sxs-lookup"><span data-stu-id="2fab4-141">b.</span></span> <span data-ttu-id="2fab4-142">**Művelet típusa**: válassza ki az SMS, e-mailek vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="2fab4-142">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="2fab4-143">c.</span><span class="sxs-lookup"><span data-stu-id="2fab4-143">c.</span></span> <span data-ttu-id="2fab4-144">**Részletek**: választott művelet típusa alapján, adjon meg egy telefonszámot, az e-mail címet, illetve a webhook URI.</span><span class="sxs-lookup"><span data-stu-id="2fab4-144">**Details**: Based on the action type chosen, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="2fab4-145">Válassza ki **OK** a riasztás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2fab4-145">Select **OK** to create the alert.</span></span>

<span data-ttu-id="2fab4-146">Néhány percen belül a riasztás aktív, és elkezdi való létrehozása során a megadott feltételek alapján.</span><span class="sxs-lookup"><span data-stu-id="2fab4-146">Within a few minutes, the alert is active and begins to trigger based on the conditions you specified during creation.</span></span>

<span data-ttu-id="2fab4-147">A napló tevékenységriasztásokat webhook sémáját információkért lásd: [Webhookok Azure tevékenység naplózása riasztások](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="2fab4-147">For information on the webhook schema for activity log alerts, see [Webhooks for Azure activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="2fab4-148">A művelet csoportnak, az alábbi lépéseket az használható fel újra egy meglévő művelet csoportként jövőbeli riasztási-definíciók.</span><span class="sxs-lookup"><span data-stu-id="2fab4-148">The action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a><span data-ttu-id="2fab4-149">Hozzon létre egy riasztást az állapotfigyelő szolgáltatás értesítést meglévő művelet csoportok az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="2fab4-149">Create an alert on a service health notification for an existing action group by using the Azure portal</span></span>

1. <span data-ttu-id="2fab4-150">Végezze el az 1-7 hozhat létre a szolgáltatás állapotának értesítési az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2fab4-150">Follow steps 1 through 7 in the previous section to create your service health notification.</span></span> 

2. <span data-ttu-id="2fab4-151">A **keresztül riasztási**, jelölje be a **meglévő** művelet csoport gombra.</span><span class="sxs-lookup"><span data-stu-id="2fab4-151">Under **Alert via**, select the **Existing** action group button.</span></span> <span data-ttu-id="2fab4-152">Válassza ki a megfelelő műveletet.</span><span class="sxs-lookup"><span data-stu-id="2fab4-152">Select the appropriate action group.</span></span>

3. <span data-ttu-id="2fab4-153">Válassza ki **OK** a riasztás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2fab4-153">Select **OK** to create the alert.</span></span>

<span data-ttu-id="2fab4-154">Néhány percen belül a riasztás aktív, és elkezdi való létrehozása során a megadott feltételek alapján.</span><span class="sxs-lookup"><span data-stu-id="2fab4-154">Within a few minutes, the alert is active and begins to trigger based on the conditions you specified during creation.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="2fab4-155">A riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="2fab4-155">Manage your alerts</span></span>

<span data-ttu-id="2fab4-156">Riasztás létrehozása után is látható, az a **riasztások** szakasza a **figyelő** panelen.</span><span class="sxs-lookup"><span data-stu-id="2fab4-156">After you create an alert, it's visible in the **Alerts** section of the **Monitor** blade.</span></span> <span data-ttu-id="2fab4-157">Jelölje ki a kezelni kívánt riasztást:</span><span class="sxs-lookup"><span data-stu-id="2fab4-157">Select the alert you want to manage to:</span></span>

* <span data-ttu-id="2fab4-158">Szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="2fab4-158">Edit it.</span></span>
* <span data-ttu-id="2fab4-159">Törölje a parancsikont.</span><span class="sxs-lookup"><span data-stu-id="2fab4-159">Delete it.</span></span>
* <span data-ttu-id="2fab4-160">Tiltsa le, vagy engedélyezheti, ha azt szeretné, ideiglenesen leállítani, vagy folytassa a riasztás értesítések fogadásának.</span><span class="sxs-lookup"><span data-stu-id="2fab4-160">Disable or enable it, if you want to temporarily stop or resume receiving notifications for the alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fab4-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2fab4-161">Next steps</span></span>
- <span data-ttu-id="2fab4-162">További tudnivalók [szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="2fab4-162">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="2fab4-163">További tudnivalók [értesítési sebessége korlátozza az](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="2fab4-163">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="2fab4-164">Tekintse át a [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="2fab4-164">Review the [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="2fab4-165">Első egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md), és megtudhatja, hogyan szeretné megkapni a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="2fab4-165">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how to receive alerts.</span></span> 
- <span data-ttu-id="2fab4-166">További információ [művelet csoportok](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="2fab4-166">Learn more about [action groups](monitoring-action-groups.md).</span></span>
