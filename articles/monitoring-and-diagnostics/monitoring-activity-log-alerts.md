---
title: "napló tevékenységriasztásokat aaaCreate |} Microsoft Docs"
description: "Értesítést SMS, webhook és e-mailek bizonyos események megtörténtekor hello műveletnaplóban."
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
ms.openlocfilehash: ba0716cc12a0b3a0024ee5562a025f3f153f8982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts"></a><span data-ttu-id="e0510-103">Napló riasztások tevékenység létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0510-103">Create activity log alerts</span></span>

## <a name="overview"></a><span data-ttu-id="e0510-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e0510-104">Overview</span></span>
<span data-ttu-id="e0510-105">Tevékenység napló riasztások az éppen riasztásokat, amelyek aktiválása, ha egy új tevékenység naplózási esemény következik be, amely megfelel a hello riasztásban feltüntetett hello feltételeknek.</span><span class="sxs-lookup"><span data-stu-id="e0510-105">Activity log alerts are alerts that activate when a new activity log event occurs that matches hello conditions specified in hello alert.</span></span> <span data-ttu-id="e0510-106">Azure-erőforrások, így azok hozhat létre Azure Resource Manager-sablonnal.</span><span class="sxs-lookup"><span data-stu-id="e0510-106">They are Azure resources, so they can be created by using an Azure Resource Manager template.</span></span> <span data-ttu-id="e0510-107">Akkor is is létrehozott, frissítése, vagy törölve a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e0510-107">They also can be created, updated, or deleted in hello Azure portal.</span></span> <span data-ttu-id="e0510-108">Ez a cikk hello fogalmakat tevékenységriasztásokat napló be.</span><span class="sxs-lookup"><span data-stu-id="e0510-108">This article introduces hello concepts behind activity log alerts.</span></span> <span data-ttu-id="e0510-109">Ezután bemutatja, hogyan toouse hello Azure portál tooset naplózási eseményeket a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="e0510-109">It then shows you how toouse hello Azure portal tooset up an alert on activity log events.</span></span>

<span data-ttu-id="e0510-110">Általában létrehozhat napló tevékenységriasztásokat tooreceive értesítések során:</span><span class="sxs-lookup"><span data-stu-id="e0510-110">Typically, you create activity log alerts tooreceive notifications when:</span></span>

* <span data-ttu-id="e0510-111">Adott változások történnek az Ön Azure-előfizetéssel, gyakran hatókörön belüli tooparticular erőforráscsoportok vagy erőforrások erőforrástól függ.</span><span class="sxs-lookup"><span data-stu-id="e0510-111">Specific changes occur on resources in your Azure subscription, often scoped tooparticular resource groups or resources.</span></span> <span data-ttu-id="e0510-112">Például érdemes toobe értesíti, ha bármely myProductionResourceGroup a virtuális gép törlődik.</span><span class="sxs-lookup"><span data-stu-id="e0510-112">For example, you might want toobe notified when any virtual machine in myProductionResourceGroup is deleted.</span></span> <span data-ttu-id="e0510-113">Vagy, érdemes toobe értesítést kapnak, ha új szerepköröket rendelt tooa felhasználó az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e0510-113">Or, you might want toobe notified if any new roles are assigned tooa user in your subscription.</span></span>
* <span data-ttu-id="e0510-114">Egy szolgáltatás állapotát az esemény akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="e0510-114">A service health event occurs.</span></span> <span data-ttu-id="e0510-115">Szolgáltatás állapotával kapcsolatos események értesítési események és karbantartási események, amelyek érvényesek az előfizetésében tooresources tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e0510-115">Service health events include notification of incidents and maintenance events that apply tooresources in your subscription.</span></span>

<span data-ttu-id="e0510-116">Mindkét esetben egy figyelmeztetés a napló csak az a mely hello riasztást hoz létre hello előfizetés eseményeket figyeli.</span><span class="sxs-lookup"><span data-stu-id="e0510-116">In either case, an activity log alert monitors only for events in hello subscription in which hello alert is created.</span></span>

<span data-ttu-id="e0510-117">A legfelső szintű tulajdonság a hello JSON-objektumból az tevékenység naplóesemény alapján tevékenység napló riasztásokat lehet beállítani.</span><span class="sxs-lookup"><span data-stu-id="e0510-117">You can configure an activity log alert based on any top-level property in hello JSON object for an activity log event.</span></span> <span data-ttu-id="e0510-118">Azonban a hello portal hello leggyakoribb lehetőségeket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="e0510-118">However, hello portal shows hello most common options:</span></span>

- <span data-ttu-id="e0510-119">**Kategória**: felügyeleti, a szolgáltatás állapotát, automatikus skálázás és javaslat.</span><span class="sxs-lookup"><span data-stu-id="e0510-119">**Category**: Administrative, Service Health, Autoscale, and Recommendation.</span></span> <span data-ttu-id="e0510-120">További információkért lásd: [hello Azure tevékenységnapló áttekintése](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span><span class="sxs-lookup"><span data-stu-id="e0510-120">For more information, see [Overview of hello Azure activity log](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span></span> <span data-ttu-id="e0510-121">További információ a szolgáltatás állapotának események, toolearn lásd: [szolgáltatáshoz értesítést tevékenység napló riasztásokat fogadhat](./monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="e0510-121">toolearn more about service health events, see [Receive activity log alerts on service notifications](./monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
- <span data-ttu-id="e0510-122">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="e0510-122">**Resource group**</span></span>
- <span data-ttu-id="e0510-123">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="e0510-123">**Resource**</span></span>
- <span data-ttu-id="e0510-124">**Erőforrástípus**</span><span class="sxs-lookup"><span data-stu-id="e0510-124">**Resource type**</span></span>
- <span data-ttu-id="e0510-125">**A művelet neve**: hello Resource Manager szerepköralapú hozzáférés-vezérlés művelet neve.</span><span class="sxs-lookup"><span data-stu-id="e0510-125">**Operation name**: hello Resource Manager Role-Based Access Control operation name.</span></span>
- <span data-ttu-id="e0510-126">**Szint**: hello súlyossági szint (részletes, tájékoztatás, figyelmeztetés, hiba vagy kritikus) hello esemény.</span><span class="sxs-lookup"><span data-stu-id="e0510-126">**Level**: hello severity level of hello event (Verbose, Informational, Warning, Error, or Critical).</span></span>
- <span data-ttu-id="e0510-127">**Állapot**: hello állapotának hello esemény – általában elindult, sikertelen vagy sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="e0510-127">**Status**: hello status of hello event, typically Started, Failed, or Succeeded.</span></span>
- <span data-ttu-id="e0510-128">**Az esemény által kezdeményezett**: néven is ismert hello "hívó."</span><span class="sxs-lookup"><span data-stu-id="e0510-128">**Event initiated by**: Also known as hello "caller."</span></span> <span data-ttu-id="e0510-129">hello e-mail címet, vagy Azure Active Directory hello műveletet végre hello felhasználó azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e0510-129">hello email address or Azure Active Directory identifier of hello user who performed hello operation.</span></span>

>[!NOTE]
><span data-ttu-id="e0510-130">Meg kell adnia legalább két megelőző feltételeknek a riasztás az egy folyamatban lévő hello hello kategóriát.</span><span class="sxs-lookup"><span data-stu-id="e0510-130">You must specify at least two of hello preceding criteria in your alert, with one being hello category.</span></span> <span data-ttu-id="e0510-131">Nem hozható létre egy riasztást, amely minden alkalommal, amikor egy esemény jön létre hello tevékenységi naplóit aktiválja.</span><span class="sxs-lookup"><span data-stu-id="e0510-131">You may not create an alert that activates every time an event is created in hello activity logs.</span></span>
>
>

<span data-ttu-id="e0510-132">Egy tevékenység napló riasztás aktiválásakor használja-e művelet toogenerate műveletek vagy értesítések csoportban.</span><span class="sxs-lookup"><span data-stu-id="e0510-132">When an activity log alert is activated, it uses an action group toogenerate actions or notifications.</span></span> <span data-ttu-id="e0510-133">Egy olyan újrafelhasználható értesítési fogadók, például az e-mail címeket, a webhook URL-címek vagy SMS telefonszámokat.</span><span class="sxs-lookup"><span data-stu-id="e0510-133">An action group is a reusable set of notification receivers, such as email addresses, webhook URLs, or SMS phone numbers.</span></span> <span data-ttu-id="e0510-134">hello fogadók több riasztások toocentralize lehet hivatkozni, és csoportosítsa az értesítési csatornák.</span><span class="sxs-lookup"><span data-stu-id="e0510-134">hello receivers can be referenced from multiple alerts toocentralize and group your notification channels.</span></span> <span data-ttu-id="e0510-135">Ha a napló figyelmeztetés, két választási lehetősége van.</span><span class="sxs-lookup"><span data-stu-id="e0510-135">When you define your activity log alert, you have two options.</span></span> <span data-ttu-id="e0510-136">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="e0510-136">You can:</span></span>

* <span data-ttu-id="e0510-137">A napló figyelmeztetés művelet meglévő csoport használata</span><span class="sxs-lookup"><span data-stu-id="e0510-137">Use an existing action group in your activity log alert.</span></span> 
* <span data-ttu-id="e0510-138">Hozzon létre egy új művelet.</span><span class="sxs-lookup"><span data-stu-id="e0510-138">Create a new action group.</span></span> 

<span data-ttu-id="e0510-139">További információk a művelet csoportok toolearn lásd: [létrehozása és az Azure-portálon hello művelet csoportok kezelése](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="e0510-139">toolearn more about action groups, see [Create and manage action groups in hello Azure portal](monitoring-action-groups.md).</span></span>

<span data-ttu-id="e0510-140">További információ a szolgáltatás állapotával kapcsolatos értesítésekre toolearn lásd [tevékenység napló értesítést a szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="e0510-140">toolearn more about service health notifications, see [Receive activity log alerts on service health notifications](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="e0510-141">Riasztás létrehozása a művelet új csoportot a tevékenység napló esemény hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="e0510-141">Create an alert on an activity log event with a new action group by using hello Azure portal</span></span>
1. <span data-ttu-id="e0510-142">A hello [portal](https://portal.azure.com), jelölje be **figyelő**.</span><span class="sxs-lookup"><span data-stu-id="e0510-142">In hello [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![hello "Figyelés" szolgáltatás](./media/monitoring-activity-log-alerts/home-monitor.png)
2. <span data-ttu-id="e0510-144">A hello **tevékenységnapló** szakaszban jelölje be **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="e0510-144">In hello **Activity log** section, select **Alerts**.</span></span>

    ![hello "Értesítések" lapon](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. <span data-ttu-id="e0510-146">Válassza ki **Hozzáadás figyelmeztetés a napló**, és töltse ki hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="e0510-146">Select **Add activity log alert**, and fill in hello fields.</span></span>

4. <span data-ttu-id="e0510-147">Adjon meg egy nevet a hello **tevékenység napló riasztás neve** mezőbe, majd válassza ki a **leírás**.</span><span class="sxs-lookup"><span data-stu-id="e0510-147">Enter a name in hello **Activity log alert name** box, and select a **Description**.</span></span>

    ![hello "Figyelmeztetés a napló hozzáadása" parancs](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. <span data-ttu-id="e0510-149">Hello **előfizetés** a jelenlegi előfizetés autofills mezőben.</span><span class="sxs-lookup"><span data-stu-id="e0510-149">hello **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="e0510-150">Ez az előfizetés egy hello mely hello művelet csoport menti.</span><span class="sxs-lookup"><span data-stu-id="e0510-150">This subscription is hello one in which hello action group is saved.</span></span> <span data-ttu-id="e0510-151">hello riasztási erőforrás telepített toothis előfizetés és a figyelők tevékenység naplóeseményeket belőle.</span><span class="sxs-lookup"><span data-stu-id="e0510-151">hello alert resource is deployed toothis subscription and monitors activity log events from it.</span></span>

    ![hello "Figyelmeztetés a napló hozzáadása" párbeszédpanel](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. <span data-ttu-id="e0510-153">Jelölje be hello **erőforráscsoport** mely hello a riasztás létrehozása történt meg.</span><span class="sxs-lookup"><span data-stu-id="e0510-153">Select hello **Resource group** in which hello alert resource is created.</span></span> <span data-ttu-id="e0510-154">Ez az hello erőforráscsoport, amelyet az hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="e0510-154">This is not hello resource group that's monitored by hello alert.</span></span> <span data-ttu-id="e0510-155">Ehelyett hello erőforráscsoport, ahol hello riasztási erőforrás is található.</span><span class="sxs-lookup"><span data-stu-id="e0510-155">Instead, it's hello resource group where hello alert resource is located.</span></span>

7. <span data-ttu-id="e0510-156">Ha kijelöl egy **eseménykategória** toomodify hello további szűrőket látható.</span><span class="sxs-lookup"><span data-stu-id="e0510-156">Optionally, select an **Event category** toomodify hello additional filters that are shown.</span></span> <span data-ttu-id="e0510-157">A felügyeleti események hello szűrők a következők **erőforráscsoport**, **erőforrás**, **erőforrástípus**, **műveletnév**, **Szint**, **állapot**, és **esemény által kezdeményezett**.</span><span class="sxs-lookup"><span data-stu-id="e0510-157">For Administrative events, hello filters include **Resource group**, **Resource**, **Resource type**, **Operation name**, **Level**, **Status**, and **Event initiated by**.</span></span> <span data-ttu-id="e0510-158">Ezeket az értékeket a riasztás célszerű figyelemmel kísérni események azonosítása.</span><span class="sxs-lookup"><span data-stu-id="e0510-158">These values identify which events this alert should monitor.</span></span>

    >[!NOTE]
    ><span data-ttu-id="e0510-159">Meg kell adnia legalább egy hello megelőző feltételeknek a riasztás.</span><span class="sxs-lookup"><span data-stu-id="e0510-159">You must specify at least one of hello preceding criteria in your alert.</span></span> <span data-ttu-id="e0510-160">Nem hozható létre egy riasztást, amely minden alkalommal, amikor egy esemény jön létre hello tevékenységi naplóit aktiválja.</span><span class="sxs-lookup"><span data-stu-id="e0510-160">You may not create an alert that activates every time an event is created in hello activity logs.</span></span>
    >
    >

8. <span data-ttu-id="e0510-161">Adjon meg egy nevet a hello **művelet csoportnév** mezőbe, majd írjon be egy nevet a hello **rövid név** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="e0510-161">Enter a name in hello **Action group name** box, and enter a name in hello **Short name** box.</span></span> <span data-ttu-id="e0510-162">hello rövid nevét helyett egy teljes művelet felügyeleticsoport-nevet használja, ha ez a csoport értesítések küldése.</span><span class="sxs-lookup"><span data-stu-id="e0510-162">hello short name is used in place of a full action group name when notifications are sent using this group.</span></span>

9.  <span data-ttu-id="e0510-163">Adja meg azon műveletek listáját, adja meg a hello művelet:</span><span class="sxs-lookup"><span data-stu-id="e0510-163">Define a list of actions by providing hello action’s:</span></span>

    <span data-ttu-id="e0510-164">a.</span><span class="sxs-lookup"><span data-stu-id="e0510-164">a.</span></span> <span data-ttu-id="e0510-165">**Név**: Adja meg a hello művelet alias, név vagy azonosító.</span><span class="sxs-lookup"><span data-stu-id="e0510-165">**Name**: Enter hello action’s name, alias, or identifier.</span></span>

    <span data-ttu-id="e0510-166">b.</span><span class="sxs-lookup"><span data-stu-id="e0510-166">b.</span></span> <span data-ttu-id="e0510-167">**Művelet típusa**: válassza ki az SMS, e-mailek vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="e0510-167">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="e0510-168">c.</span><span class="sxs-lookup"><span data-stu-id="e0510-168">c.</span></span> <span data-ttu-id="e0510-169">**Részletek**: hello művelet típusa alapján, adjon meg egy telefonszámot, az e-mail címet, illetve a webhook URI.</span><span class="sxs-lookup"><span data-stu-id="e0510-169">**Details**: Based on hello action type, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="e0510-170">Válassza ki **OK** toocreate hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="e0510-170">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="e0510-171">hello riasztás néhány percet vesz igénybe toofully propagálására, és majd aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="e0510-171">hello alert takes a few minutes toofully propagate and then become active.</span></span> <span data-ttu-id="e0510-172">Amikor új események hello riasztás feltételeknek elindítja.</span><span class="sxs-lookup"><span data-stu-id="e0510-172">It triggers when new events match hello alert's criteria.</span></span>

<span data-ttu-id="e0510-173">További információkért lásd: [megértése hello webhook séma napló tevékenységriasztásokat használt](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="e0510-173">For more information, see [Understand hello webhook schema used in activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="e0510-174">Ezeket a lépéseket definiált hello művelet csoport újrafelhasználható, az összes jövőbeni riasztási definíciók egy meglévő művelet csoportként.</span><span class="sxs-lookup"><span data-stu-id="e0510-174">hello action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="e0510-175">Riasztás létrehozása a meglévő művelet csoportok az tevékenység naplóesemény hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="e0510-175">Create an alert on an activity log event for an existing action group by using hello Azure portal</span></span>
1. <span data-ttu-id="e0510-176">Végezze el az 1-7 hello előző szakasz toocreate a napló figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="e0510-176">Follow steps 1 through 7 in hello previous section toocreate your activity log alert.</span></span>

2. <span data-ttu-id="e0510-177">A **keresztül értesíti**, jelölje be hello **meglévő** művelet csoport gombra.</span><span class="sxs-lookup"><span data-stu-id="e0510-177">Under **Notify via**, select hello **Existing** action group button.</span></span> <span data-ttu-id="e0510-178">Válasszon egy meglévő művelet csoportot hello listából.</span><span class="sxs-lookup"><span data-stu-id="e0510-178">Select an existing action group from hello list.</span></span>

3. <span data-ttu-id="e0510-179">Válassza ki **OK** toocreate hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="e0510-179">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="e0510-180">hello riasztás néhány percet vesz igénybe toofully propagálására, és majd aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="e0510-180">hello alert takes a few minutes toofully propagate and then become active.</span></span> <span data-ttu-id="e0510-181">Amikor új események hello riasztás feltételeknek elindítja.</span><span class="sxs-lookup"><span data-stu-id="e0510-181">It triggers when new events match hello alert's criteria.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="e0510-182">A riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="e0510-182">Manage your alerts</span></span>

<span data-ttu-id="e0510-183">Riasztás létrehozása után már látható hello riasztások szakaszban hello figyelő panelről.</span><span class="sxs-lookup"><span data-stu-id="e0510-183">After you create an alert, it's visible in hello Alerts section of hello Monitor blade.</span></span> <span data-ttu-id="e0510-184">Jelölje ki azt szeretné, hogy toomanage hello riasztást:</span><span class="sxs-lookup"><span data-stu-id="e0510-184">Select hello alert you want toomanage to:</span></span>

* <span data-ttu-id="e0510-185">Szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="e0510-185">Edit it.</span></span>
* <span data-ttu-id="e0510-186">Törölje a parancsikont.</span><span class="sxs-lookup"><span data-stu-id="e0510-186">Delete it.</span></span>
* <span data-ttu-id="e0510-187">Tiltsa le, vagy engedélyezheti, ha folytatni hello riasztás értesítések fogadásának vagy tootemporarily le szeretné.</span><span class="sxs-lookup"><span data-stu-id="e0510-187">Disable or enable it, if you want tootemporarily stop or resume receiving notifications for hello alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0510-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0510-188">Next steps</span></span>
- <span data-ttu-id="e0510-189">Első egy [riasztások áttekintése](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="e0510-189">Get an [overview of alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="e0510-190">További tudnivalók [értesítési sebessége korlátozza az](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="e0510-190">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="e0510-191">Felülvizsgálati hello [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="e0510-191">Review hello [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="e0510-192">További információ [művelet csoportok](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="e0510-192">Learn more about [action groups](monitoring-action-groups.md).</span></span>  
- <span data-ttu-id="e0510-193">További tudnivalók [szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="e0510-193">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="e0510-194">Hozzon létre egy [tevékenységet naplózni riasztási toomonitor az előfizetés minden automatikus skálázási motor műveletek](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="e0510-194">Create an [activity log alert toomonitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="e0510-195">Hozzon létre egy [tevékenységet naplózni riasztási toomonitor az előfizetés összes sikertelen automatikus skálázás méretezési-a/kibővített művelete](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="e0510-195">Create an [activity log alert toomonitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
