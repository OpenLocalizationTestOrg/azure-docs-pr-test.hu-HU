---
title: "Az Azure portálon művelet csoportok létrehozása és kezelése |} Microsoft Docs"
description: "Útmutató az Azure portálon művelet csoportok létrehozásához és kezeléséhez."
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
ms.date: 05/15/2017
ms.author: ancav
ms.openlocfilehash: ea15705bf02d9773507c6cb59f2da4c1dd0f9d77
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a><span data-ttu-id="f3bbb-103">Az Azure portálon művelet csoportok létrehozása és kezelése</span><span class="sxs-lookup"><span data-stu-id="f3bbb-103">Create and manage action groups in the Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="f3bbb-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f3bbb-104">Overview</span></span> ##
<span data-ttu-id="f3bbb-105">Ez a cikk bemutatja, hogyan az Azure portálon művelet csoportok létrehozásához és kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-105">This article shows you how to create and manage action groups in the Azure portal.</span></span>

<span data-ttu-id="f3bbb-106">A művelet csoportok konfigurálható azon műveletek listáját.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-106">You can configure a list of actions with action groups.</span></span> <span data-ttu-id="f3bbb-107">Ezek a csoportok majd használható napló tevékenységriasztásokat definiálásakor.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-107">These groups then can be used when you define activity log alerts.</span></span> <span data-ttu-id="f3bbb-108">Ezek a csoportok majd minden egyes napló figyelmeztetés a meghatározásához, biztosítva, hogy ugyanazokat a műveleteket a rendszer minden alkalommal, amikor a tevékenység napló figyelmeztetés felhasználhatók.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-108">These groups can then be reused by each activity log alert you define, ensuring that the same actions are taken each time the activity log alert is triggered.</span></span>

<span data-ttu-id="f3bbb-109">Egy legfeljebb 10 minden művelet típusú lehet.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-109">An action group can have up to 10 of each action type.</span></span> <span data-ttu-id="f3bbb-110">Egyes műveletek során a következő tulajdonságok tevődik össze:</span><span class="sxs-lookup"><span data-stu-id="f3bbb-110">Each action is made up of the following properties:</span></span>

* <span data-ttu-id="f3bbb-111">**Név**: a művelet csoporton belül egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-111">**Name**: A unique identifier within the action group.</span></span>  
* <span data-ttu-id="f3bbb-112">**Művelet típusa**: SMS küldése, küldjön egy e-mailt, vagy hívja a webhook.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-112">**Action type**: Send an SMS, send an email, or call a webhook.</span></span>  
* <span data-ttu-id="f3bbb-113">**Részletek**: A megfelelő telefonszám, számot, e-mail címét vagy webhook URI.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-113">**Details**: The corresponding phone number, email address, or webhook URI.</span></span>

<span data-ttu-id="f3bbb-114">Művelet csoportok konfigurálása Azure Resource Manager-sablonok használatával kapcsolatos információkért lásd: [művelet csoport Resource Manager-sablonok](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="f3bbb-114">For information on how to use Azure Resource Manager templates to configure action groups, see [Action group Resource Manager templates](monitoring-create-action-group-with-resource-manager-template.md).</span></span>

## <a name="create-an-action-group-by-using-the-azure-portal"></a><span data-ttu-id="f3bbb-115">Egy művelet csoport létrehozása az Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="f3bbb-115">Create an action group by using the Azure portal</span></span> ##
1. <span data-ttu-id="f3bbb-116">Az a [portal](https://portal.azure.com), jelölje be **figyelő**.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-116">In the [portal](https://portal.azure.com), select **Monitor**.</span></span> <span data-ttu-id="f3bbb-117">A **figyelő** panel összes figyelési beállítások és adatok az egyik nézetben összesíti.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-117">The **Monitor** blade consolidates all your monitoring settings and data in one view.</span></span>

    ![A "Figyelés" szolgáltatás](./media/monitoring-action-groups/home-monitor.png)
2. <span data-ttu-id="f3bbb-119">Az a **tevékenységnapló** szakaszban jelölje be **művelet csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-119">In the **Activity log** section, select **Action groups**.</span></span>

    ![A "Művelet csoportok" lap](./media/monitoring-action-groups/action-groups-blade.png)
3. <span data-ttu-id="f3bbb-121">Válassza ki **művelet csoport hozzáadása**, és töltse ki a mezőket.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-121">Select **Add action group**, and fill in the fields.</span></span>

    ![A "Művelet csoport hozzáadása" parancs](./media/monitoring-action-groups/add-action-group.png)
4. <span data-ttu-id="f3bbb-123">Adjon meg egy nevet a a **művelet csoportnév** mezőbe, majd írjon be egy nevet a **rövid név** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-123">Enter a name in the **Action group name** box, and enter a name in the **Short name** box.</span></span> <span data-ttu-id="f3bbb-124">A rövid nevét használja a teljes műveletet csoport neve helyett amikor ez a csoport értesítések küldése.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-124">The short name is used in place of a full action group name when notifications are sent using this group.</span></span>

      ![A művelet csoport hozzáadása"párbeszédpanel](./media/monitoring-action-groups/action-group-define.png)

5. <span data-ttu-id="f3bbb-126">A **előfizetés** a jelenlegi előfizetés autofills mezőben.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-126">The **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="f3bbb-127">Ez az előfizetés, amelyben a művelet csoport mentett lesz.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-127">This subscription is the one in which the action group is saved.</span></span>

6. <span data-ttu-id="f3bbb-128">Válassza ki a **erőforráscsoport** az a művelet csoport mentve.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-128">Select the **Resource group** in which the action group is saved.</span></span>

7. <span data-ttu-id="f3bbb-129">Adja meg azon műveletek listáját, adja meg a minden egyes művelethez:</span><span class="sxs-lookup"><span data-stu-id="f3bbb-129">Define a list of actions by providing each action's:</span></span>

    <span data-ttu-id="f3bbb-130">a.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-130">a.</span></span> <span data-ttu-id="f3bbb-131">**Név**: Adjon meg egy egyedi azonosítót ehhez a művelethez.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-131">**Name**: Enter a unique identifier for this action.</span></span>

    <span data-ttu-id="f3bbb-132">b.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-132">b.</span></span> <span data-ttu-id="f3bbb-133">**Művelet típusa**: válassza ki az SMS, e-mailek vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-133">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="f3bbb-134">c.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-134">c.</span></span> <span data-ttu-id="f3bbb-135">**Részletek**: a művelet típusa alapján, adjon meg egy telefonszám, e-mail címét vagy webhook URI.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-135">**Details**: Based on the action type, enter a phone number, email address, or webhook URI.</span></span>

8. <span data-ttu-id="f3bbb-136">Válassza ki **OK** a művelet csoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-136">Select **OK** to create the action group.</span></span>

## <a name="manage-your-action-groups"></a><span data-ttu-id="f3bbb-137">A művelet csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="f3bbb-137">Manage your action groups</span></span> ##
<span data-ttu-id="f3bbb-138">Egy művelet csoport létrehozása után is látható, az a **művelet csoportok** szakasza a **figyelő** panelen.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-138">After you create an action group, it's visible in the **Action groups** section of the **Monitor** blade.</span></span> <span data-ttu-id="f3bbb-139">Válassza ki a kezelni kívánt:</span><span class="sxs-lookup"><span data-stu-id="f3bbb-139">Select the action group you want to manage to:</span></span>

* <span data-ttu-id="f3bbb-140">Adja hozzá, szerkeszthet és eltávolíthat műveletek.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-140">Add, edit, or remove actions.</span></span>
* <span data-ttu-id="f3bbb-141">A művelet csoport törlése.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-141">Delete the action group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3bbb-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3bbb-142">Next steps</span></span> ##
* <span data-ttu-id="f3bbb-143">További információ [SMS riasztási viselkedés](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="f3bbb-143">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>  
* <span data-ttu-id="f3bbb-144">Szerezzen egy [megismerni a műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="f3bbb-144">Gain an [understanding of the activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>  
* <span data-ttu-id="f3bbb-145">További információ [sebességkorlátozást](monitoring-alerts-rate-limiting.md) értesítésekről.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-145">Learn more about [rate limiting](monitoring-alerts-rate-limiting.md) on alerts.</span></span> 
* <span data-ttu-id="f3bbb-146">Első egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md), és megtudhatja, hogyan szeretné megkapni a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="f3bbb-146">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how to receive alerts.</span></span>  
* <span data-ttu-id="f3bbb-147">Megtudhatja, hogyan [riasztások konfigurálása, ha az állapotfigyelő szolgáltatáshoz értesítést visszaküldi](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="f3bbb-147">Learn how to [configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
