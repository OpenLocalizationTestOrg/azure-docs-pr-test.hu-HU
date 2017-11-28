---
title: "Napló riasztások tevékenység létrehozása |} Microsoft Docs"
description: "Értesítést SMS, webhook és e-mailt a műveletnaplóban bizonyos események megtörténtekor."
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
ms.date: 08/03/2017
ms.author: johnkem
ms.openlocfilehash: 3885469ec0e1fcc31386dd0ad7fe6cb5d03ab28e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-activity-log-alerts"></a><span data-ttu-id="abddd-103">Napló riasztások tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="abddd-103">Create activity log alerts</span></span>

## <a name="overview"></a><span data-ttu-id="abddd-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="abddd-104">Overview</span></span>
<span data-ttu-id="abddd-105">Tevékenység napló riasztások az éppen aktiválása, ha egy új tevékenység napló esemény történik a riasztás megadott feltételeknek megfelelő riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="abddd-105">Activity log alerts are alerts that activate when a new activity log event occurs that matches the conditions specified in the alert.</span></span> <span data-ttu-id="abddd-106">Azure-erőforrások, így azok hozhat létre Azure Resource Manager-sablonnal.</span><span class="sxs-lookup"><span data-stu-id="abddd-106">They are Azure resources, so they can be created by using an Azure Resource Manager template.</span></span> <span data-ttu-id="abddd-107">Akkor is is létrehozása, frissítése, vagy törölve az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="abddd-107">They also can be created, updated, or deleted in the Azure portal.</span></span> <span data-ttu-id="abddd-108">Ez a cikk bemutatja a napló tevékenységriasztásokat mögött.</span><span class="sxs-lookup"><span data-stu-id="abddd-108">This article introduces the concepts behind activity log alerts.</span></span> <span data-ttu-id="abddd-109">Azt ezután bemutatja, hogyan használható az Azure-portálon naplózási eseményeket a riasztás beállításához.</span><span class="sxs-lookup"><span data-stu-id="abddd-109">It then shows you how to use the Azure portal to set up an alert on activity log events.</span></span>

<span data-ttu-id="abddd-110">Általában létrehozhat tevékenység napló riasztásokat, értesítéseket során:</span><span class="sxs-lookup"><span data-stu-id="abddd-110">Typically, you create activity log alerts to receive notifications when:</span></span>

* <span data-ttu-id="abddd-111">Erőforrások az Azure-előfizetése, gyakran hatókörű adott forrás-és erőforrások adott változás.</span><span class="sxs-lookup"><span data-stu-id="abddd-111">Specific changes occur on resources in your Azure subscription, often scoped to particular resource groups or resources.</span></span> <span data-ttu-id="abddd-112">Például előfordulhat, hogy kívánt értesíti, ha bármely myProductionResourceGroup a virtuális gép törlődik.</span><span class="sxs-lookup"><span data-stu-id="abddd-112">For example, you might want to be notified when any virtual machine in myProductionResourceGroup is deleted.</span></span> <span data-ttu-id="abddd-113">Vagy előfordulhat, hogy szeretne értesítést kapni, ha új szerepköröket a felhasználó az előfizetéshez vannak rendelve.</span><span class="sxs-lookup"><span data-stu-id="abddd-113">Or, you might want to be notified if any new roles are assigned to a user in your subscription.</span></span>
* <span data-ttu-id="abddd-114">Egy szolgáltatás állapotát az esemény akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="abddd-114">A service health event occurs.</span></span> <span data-ttu-id="abddd-115">Szolgáltatás állapotával kapcsolatos események tartalmazzák az incidensek és karbantartási események erőforrást az előfizetésében vonatkozó értesítés.</span><span class="sxs-lookup"><span data-stu-id="abddd-115">Service health events include notification of incidents and maintenance events that apply to resources in your subscription.</span></span>

<span data-ttu-id="abddd-116">Mindkét esetben egy figyelmeztetés a napló csak az eseményeket az előfizetés, amelyben a riasztást hoz létre figyeli.</span><span class="sxs-lookup"><span data-stu-id="abddd-116">In either case, an activity log alert monitors only for events in the subscription in which the alert is created.</span></span>

<span data-ttu-id="abddd-117">A JSON-objektumokat az tevékenység naplóesemény legfelső szintű tulajdonságnak sem alapján tevékenység napló riasztásokat lehet beállítani.</span><span class="sxs-lookup"><span data-stu-id="abddd-117">You can configure an activity log alert based on any top-level property in the JSON object for an activity log event.</span></span> <span data-ttu-id="abddd-118">A portál azonban a leggyakoribb beállítások láthatók:</span><span class="sxs-lookup"><span data-stu-id="abddd-118">However, the portal shows the most common options:</span></span>

- <span data-ttu-id="abddd-119">**Kategória**: felügyeleti, a szolgáltatás állapotát, automatikus skálázás és javaslat.</span><span class="sxs-lookup"><span data-stu-id="abddd-119">**Category**: Administrative, Service Health, Autoscale, and Recommendation.</span></span> <span data-ttu-id="abddd-120">További információkért lásd: [az Azure tevékenységnapló áttekintése](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span><span class="sxs-lookup"><span data-stu-id="abddd-120">For more information, see [Overview of the Azure activity log](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span></span> <span data-ttu-id="abddd-121">A szolgáltatás állapotával kapcsolatos események kapcsolatos további információkért lásd: [szolgáltatáshoz értesítést tevékenység napló riasztásokat fogadhat](./monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="abddd-121">To learn more about service health events, see [Receive activity log alerts on service notifications](./monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
- <span data-ttu-id="abddd-122">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="abddd-122">**Resource group**</span></span>
- <span data-ttu-id="abddd-123">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="abddd-123">**Resource**</span></span>
- <span data-ttu-id="abddd-124">**Erőforrástípus**</span><span class="sxs-lookup"><span data-stu-id="abddd-124">**Resource type**</span></span>
- <span data-ttu-id="abddd-125">**A művelet neve**: hozzáférés-vezérlés The Resource Manager Role-Based művelet neve.</span><span class="sxs-lookup"><span data-stu-id="abddd-125">**Operation name**: The Resource Manager Role-Based Access Control operation name.</span></span>
- <span data-ttu-id="abddd-126">**Szint**: A súlyossági szintet az esemény (részletes, tájékoztatás, figyelmeztetés, hiba vagy kritikus).</span><span class="sxs-lookup"><span data-stu-id="abddd-126">**Level**: The severity level of the event (Verbose, Informational, Warning, Error, or Critical).</span></span>
- <span data-ttu-id="abddd-127">**Állapot**: az esemény, általában elindult, sikertelen vagy sikeres állapotát.</span><span class="sxs-lookup"><span data-stu-id="abddd-127">**Status**: The status of the event, typically Started, Failed, or Succeeded.</span></span>
- <span data-ttu-id="abddd-128">**Az esemény által kezdeményezett**: más néven a "hívó."</span><span class="sxs-lookup"><span data-stu-id="abddd-128">**Event initiated by**: Also known as the "caller."</span></span> <span data-ttu-id="abddd-129">Az e-mail cím vagy a műveletet a felhasználó Azure Active Directory azonosítója.</span><span class="sxs-lookup"><span data-stu-id="abddd-129">The email address or Azure Active Directory identifier of the user who performed the operation.</span></span>

>[!NOTE]
><span data-ttu-id="abddd-130">Meg kell adnia a fenti feltételek közül legalább kettő kattintson a riasztásra, egy a kategória alatt.</span><span class="sxs-lookup"><span data-stu-id="abddd-130">You must specify at least two of the preceding criteria in your alert, with one being the category.</span></span> <span data-ttu-id="abddd-131">Nem hozható létre egy riasztást, amely minden alkalommal, amikor egy esemény jön létre a tevékenységi naplóit aktiválja.</span><span class="sxs-lookup"><span data-stu-id="abddd-131">You may not create an alert that activates every time an event is created in the activity logs.</span></span>
>
>

<span data-ttu-id="abddd-132">Amikor egy tevékenység napló riasztás aktív, az egy műveletek vagy értesítések.</span><span class="sxs-lookup"><span data-stu-id="abddd-132">When an activity log alert is activated, it uses an action group to generate actions or notifications.</span></span> <span data-ttu-id="abddd-133">Egy olyan újrafelhasználható értesítési fogadók, például az e-mail címeket, a webhook URL-címek vagy SMS telefonszámokat.</span><span class="sxs-lookup"><span data-stu-id="abddd-133">An action group is a reusable set of notification receivers, such as email addresses, webhook URLs, or SMS phone numbers.</span></span> <span data-ttu-id="abddd-134">A fogadók központosítása és az értesítési csatornák csoport több riasztás lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="abddd-134">The receivers can be referenced from multiple alerts to centralize and group your notification channels.</span></span> <span data-ttu-id="abddd-135">Ha a napló figyelmeztetés, két választási lehetősége van.</span><span class="sxs-lookup"><span data-stu-id="abddd-135">When you define your activity log alert, you have two options.</span></span> <span data-ttu-id="abddd-136">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="abddd-136">You can:</span></span>

* <span data-ttu-id="abddd-137">A napló figyelmeztetés művelet meglévő csoport használata</span><span class="sxs-lookup"><span data-stu-id="abddd-137">Use an existing action group in your activity log alert.</span></span> 
* <span data-ttu-id="abddd-138">Hozzon létre egy új művelet.</span><span class="sxs-lookup"><span data-stu-id="abddd-138">Create a new action group.</span></span> 

<span data-ttu-id="abddd-139">Művelet csoportokkal kapcsolatos további tudnivalókért lásd: [létrehozása és kezelése az Azure portálon művelet csoportok](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="abddd-139">To learn more about action groups, see [Create and manage action groups in the Azure portal](monitoring-action-groups.md).</span></span>

<span data-ttu-id="abddd-140">A szolgáltatás állapotával kapcsolatos értesítésekre kapcsolatos további információkért lásd: [tevékenység napló értesítést a szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="abddd-140">To learn more about service health notifications, see [Receive activity log alerts on service health notifications](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a><span data-ttu-id="abddd-141">A művelet új csoport tevékenység napló esemény a riasztás létrehozása az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="abddd-141">Create an alert on an activity log event with a new action group by using the Azure portal</span></span>
1. <span data-ttu-id="abddd-142">Az a [portal](https://portal.azure.com), jelölje be **figyelő**.</span><span class="sxs-lookup"><span data-stu-id="abddd-142">In the [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![A "Figyelés" szolgáltatás](./media/monitoring-activity-log-alerts/home-monitor.png)
2. <span data-ttu-id="abddd-144">Az a **tevékenységnapló** szakaszban jelölje be **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="abddd-144">In the **Activity log** section, select **Alerts**.</span></span>

    ![Az "Értesítések" lapon](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. <span data-ttu-id="abddd-146">Válassza ki **Hozzáadás figyelmeztetés a napló**, és töltse ki a mezőket.</span><span class="sxs-lookup"><span data-stu-id="abddd-146">Select **Add activity log alert**, and fill in the fields.</span></span>

4. <span data-ttu-id="abddd-147">Írjon be egy nevet a **tevékenység napló riasztás neve** mezőbe, majd válassza ki a **leírás**.</span><span class="sxs-lookup"><span data-stu-id="abddd-147">Enter a name in the **Activity log alert name** box, and select a **Description**.</span></span>

    ![A "Hozzáadás napló figyelmeztetés" parancs](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. <span data-ttu-id="abddd-149">A **előfizetés** a jelenlegi előfizetés autofills mezőben.</span><span class="sxs-lookup"><span data-stu-id="abddd-149">The **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="abddd-150">Ez az előfizetés, amelyben a művelet csoport mentett lesz.</span><span class="sxs-lookup"><span data-stu-id="abddd-150">This subscription is the one in which the action group is saved.</span></span> <span data-ttu-id="abddd-151">A riasztási erőforrás ebbe az előfizetésbe telepítve van, és figyeli a naplózási eseményeket belőle.</span><span class="sxs-lookup"><span data-stu-id="abddd-151">The alert resource is deployed to this subscription and monitors activity log events from it.</span></span>

    ![A "Napló figyelmeztetés hozzáadása" párbeszédpanel](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. <span data-ttu-id="abddd-153">Válassza ki a **erőforráscsoport** a riasztási erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="abddd-153">Select the **Resource group** in which the alert resource is created.</span></span> <span data-ttu-id="abddd-154">Ez nem az erőforráscsoport, amelyet a riasztást.</span><span class="sxs-lookup"><span data-stu-id="abddd-154">This is not the resource group that's monitored by the alert.</span></span> <span data-ttu-id="abddd-155">Ehelyett az erőforráscsoportot, ahol a riasztás erőforrás.</span><span class="sxs-lookup"><span data-stu-id="abddd-155">Instead, it's the resource group where the alert resource is located.</span></span>

7. <span data-ttu-id="abddd-156">Ha kijelöl egy **eseménykategória** módosítása a további szűrőket látható.</span><span class="sxs-lookup"><span data-stu-id="abddd-156">Optionally, select an **Event category** to modify the additional filters that are shown.</span></span> <span data-ttu-id="abddd-157">A felügyeleti események, a szűrők a következők **erőforráscsoport**, **erőforrás**, **erőforrástípus**, **műveletnév**, **Szint**, **állapot**, és **esemény által kezdeményezett**.</span><span class="sxs-lookup"><span data-stu-id="abddd-157">For Administrative events, the filters include **Resource group**, **Resource**, **Resource type**, **Operation name**, **Level**, **Status**, and **Event initiated by**.</span></span> <span data-ttu-id="abddd-158">Ezeket az értékeket a riasztás célszerű figyelemmel kísérni események azonosítása.</span><span class="sxs-lookup"><span data-stu-id="abddd-158">These values identify which events this alert should monitor.</span></span>

    >[!NOTE]
    ><span data-ttu-id="abddd-159">Meg kell adnia legalább egy a fenti feltételek közül a riasztásban.</span><span class="sxs-lookup"><span data-stu-id="abddd-159">You must specify at least one of the preceding criteria in your alert.</span></span> <span data-ttu-id="abddd-160">Nem hozható létre egy riasztást, amely minden alkalommal, amikor egy esemény jön létre a tevékenységi naplóit aktiválja.</span><span class="sxs-lookup"><span data-stu-id="abddd-160">You may not create an alert that activates every time an event is created in the activity logs.</span></span>
    >
    >

8. <span data-ttu-id="abddd-161">Adjon meg egy nevet a a **művelet csoportnév** mezőbe, majd írjon be egy nevet a **rövid név** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="abddd-161">Enter a name in the **Action group name** box, and enter a name in the **Short name** box.</span></span> <span data-ttu-id="abddd-162">A rövid nevét használja a teljes műveletet csoport neve helyett amikor ez a csoport értesítések küldése.</span><span class="sxs-lookup"><span data-stu-id="abddd-162">The short name is used in place of a full action group name when notifications are sent using this group.</span></span>

9.  <span data-ttu-id="abddd-163">A művelet megadásával határozza meg azon műveletek listáját:</span><span class="sxs-lookup"><span data-stu-id="abddd-163">Define a list of actions by providing the action’s:</span></span>

    <span data-ttu-id="abddd-164">a.</span><span class="sxs-lookup"><span data-stu-id="abddd-164">a.</span></span> <span data-ttu-id="abddd-165">**Név**: Adja meg a művelet neve, az alias vagy a azonosítója.</span><span class="sxs-lookup"><span data-stu-id="abddd-165">**Name**: Enter the action’s name, alias, or identifier.</span></span>

    <span data-ttu-id="abddd-166">b.</span><span class="sxs-lookup"><span data-stu-id="abddd-166">b.</span></span> <span data-ttu-id="abddd-167">**Művelet típusa**: válassza ki az SMS, e-mailek vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="abddd-167">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="abddd-168">c.</span><span class="sxs-lookup"><span data-stu-id="abddd-168">c.</span></span> <span data-ttu-id="abddd-169">**Részletek**: a művelet típusa alapján, adjon meg egy telefonszám, e-mail címét vagy webhook URI.</span><span class="sxs-lookup"><span data-stu-id="abddd-169">**Details**: Based on the action type, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="abddd-170">Válassza ki **OK** a riasztás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="abddd-170">Select **OK** to create the alert.</span></span>

<span data-ttu-id="abddd-171">A riasztás teljesen propagálása és válik aktívvá néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="abddd-171">The alert takes a few minutes to fully propagate and then become active.</span></span> <span data-ttu-id="abddd-172">Amikor új események feltételeknek a riasztás elindítja.</span><span class="sxs-lookup"><span data-stu-id="abddd-172">It triggers when new events match the alert's criteria.</span></span>

<span data-ttu-id="abddd-173">További információkért lásd: [megérteni a webhook séma napló tevékenységriasztásokat használt](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="abddd-173">For more information, see [Understand the webhook schema used in activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="abddd-174">A művelet csoportnak, az alábbi lépéseket az használható fel újra egy meglévő művelet csoportként jövőbeli riasztási-definíciók.</span><span class="sxs-lookup"><span data-stu-id="abddd-174">The action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a><span data-ttu-id="abddd-175">Az Azure portál segítségével hozzon létre egy riasztást az tevékenység naplóesemény meglévő művelet csoportok</span><span class="sxs-lookup"><span data-stu-id="abddd-175">Create an alert on an activity log event for an existing action group by using the Azure portal</span></span>
1. <span data-ttu-id="abddd-176">Végezze el az 1 – 7 a napló figyelmeztetés létrehozásához az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="abddd-176">Follow steps 1 through 7 in the previous section to create your activity log alert.</span></span>

2. <span data-ttu-id="abddd-177">A **keresztül értesíti**, jelölje be a **meglévő** művelet csoport gombra.</span><span class="sxs-lookup"><span data-stu-id="abddd-177">Under **Notify via**, select the **Existing** action group button.</span></span> <span data-ttu-id="abddd-178">Válasszon egy meglévő művelet csoportot a listából.</span><span class="sxs-lookup"><span data-stu-id="abddd-178">Select an existing action group from the list.</span></span>

3. <span data-ttu-id="abddd-179">Válassza ki **OK** a riasztás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="abddd-179">Select **OK** to create the alert.</span></span>

<span data-ttu-id="abddd-180">A riasztás teljesen propagálása és válik aktívvá néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="abddd-180">The alert takes a few minutes to fully propagate and then become active.</span></span> <span data-ttu-id="abddd-181">Amikor új események feltételeknek a riasztás elindítja.</span><span class="sxs-lookup"><span data-stu-id="abddd-181">It triggers when new events match the alert's criteria.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="abddd-182">A riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="abddd-182">Manage your alerts</span></span>

<span data-ttu-id="abddd-183">Riasztás létrehozása után már látható a riasztások szakaszban, a figyelő panelről.</span><span class="sxs-lookup"><span data-stu-id="abddd-183">After you create an alert, it's visible in the Alerts section of the Monitor blade.</span></span> <span data-ttu-id="abddd-184">Jelölje ki a kezelni kívánt riasztást:</span><span class="sxs-lookup"><span data-stu-id="abddd-184">Select the alert you want to manage to:</span></span>

* <span data-ttu-id="abddd-185">Szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="abddd-185">Edit it.</span></span>
* <span data-ttu-id="abddd-186">Törölje a parancsikont.</span><span class="sxs-lookup"><span data-stu-id="abddd-186">Delete it.</span></span>
* <span data-ttu-id="abddd-187">Tiltsa le, vagy engedélyezheti, ha azt szeretné, ideiglenesen leállítani, vagy folytassa a riasztás értesítések fogadásának.</span><span class="sxs-lookup"><span data-stu-id="abddd-187">Disable or enable it, if you want to temporarily stop or resume receiving notifications for the alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="abddd-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="abddd-188">Next steps</span></span>
- <span data-ttu-id="abddd-189">Első egy [riasztások áttekintése](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="abddd-189">Get an [overview of alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="abddd-190">További tudnivalók [értesítési sebessége korlátozza az](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="abddd-190">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="abddd-191">Tekintse át a [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="abddd-191">Review the [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="abddd-192">További információ [művelet csoportok](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="abddd-192">Learn more about [action groups](monitoring-action-groups.md).</span></span>  
- <span data-ttu-id="abddd-193">További tudnivalók [szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="abddd-193">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="abddd-194">Hozzon létre egy [napló figyelmeztetés a figyelheti az előfizetés minden automatikus skálázási motor műveletek](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="abddd-194">Create an [activity log alert to monitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="abddd-195">Hozzon létre egy [napló figyelmeztetés a figyelheti az előfizetés összes sikertelen automatikus skálázás méretezési-a/kibővített művelete](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="abddd-195">Create an [activity log alert to monitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
