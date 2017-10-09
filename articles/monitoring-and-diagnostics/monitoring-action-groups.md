---
title: "aaaCreate és hello Azure-portálon művelet csoportok kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és hello Azure-portálon művelet csoportok kezelése."
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
ms.openlocfilehash: 97e0b22bea7787fff6856f895a7e6256c177efd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-action-groups-in-hello-azure-portal"></a><span data-ttu-id="0836c-103">Az Azure-portálon hello művelet csoportok létrehozása és kezelése</span><span class="sxs-lookup"><span data-stu-id="0836c-103">Create and manage action groups in hello Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="0836c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0836c-104">Overview</span></span> ##
<span data-ttu-id="0836c-105">Ez a cikk bemutatja, hogyan toocreate és hello Azure-portálon művelet csoportok kezelése.</span><span class="sxs-lookup"><span data-stu-id="0836c-105">This article shows you how toocreate and manage action groups in hello Azure portal.</span></span>

<span data-ttu-id="0836c-106">A művelet csoportok konfigurálható azon műveletek listáját.</span><span class="sxs-lookup"><span data-stu-id="0836c-106">You can configure a list of actions with action groups.</span></span> <span data-ttu-id="0836c-107">Ezek a csoportok majd használható napló tevékenységriasztásokat definiálásakor.</span><span class="sxs-lookup"><span data-stu-id="0836c-107">These groups then can be used when you define activity log alerts.</span></span> <span data-ttu-id="0836c-108">Ezek a csoportok majd minden egyes napló figyelmeztetés a meghatározásához, győződjön meg arról, hogy ugyanazon műveleteket hajtja végre hello figyelmeztetés a naplófájl minden egyes indításakor hello felhasználhatók.</span><span class="sxs-lookup"><span data-stu-id="0836c-108">These groups can then be reused by each activity log alert you define, ensuring that hello same actions are taken each time hello activity log alert is triggered.</span></span>

<span data-ttu-id="0836c-109">Egy művelet típusonkénti too10 legfeljebb tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="0836c-109">An action group can have up too10 of each action type.</span></span> <span data-ttu-id="0836c-110">Egyes műveletek során a következő tulajdonságok hello tevődik össze:</span><span class="sxs-lookup"><span data-stu-id="0836c-110">Each action is made up of hello following properties:</span></span>

* <span data-ttu-id="0836c-111">**Név**: hello művelet csoporton belül egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0836c-111">**Name**: A unique identifier within hello action group.</span></span>  
* <span data-ttu-id="0836c-112">**Művelet típusa**: SMS küldése, küldjön egy e-mailt, vagy hívja a webhook.</span><span class="sxs-lookup"><span data-stu-id="0836c-112">**Action type**: Send an SMS, send an email, or call a webhook.</span></span>  
* <span data-ttu-id="0836c-113">**Részletek**: hello a megfelelő telefonszám, e-mail címét vagy webhook URI.</span><span class="sxs-lookup"><span data-stu-id="0836c-113">**Details**: hello corresponding phone number, email address, or webhook URI.</span></span>

<span data-ttu-id="0836c-114">Információ toouse Azure Resource Manager sablonok tooconfigure művelet csoportok: [művelet csoport Resource Manager-sablonok](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="0836c-114">For information on how toouse Azure Resource Manager templates tooconfigure action groups, see [Action group Resource Manager templates](monitoring-create-action-group-with-resource-manager-template.md).</span></span>

## <a name="create-an-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="0836c-115">Egy művelet csoport létrehozása hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="0836c-115">Create an action group by using hello Azure portal</span></span> ##
1. <span data-ttu-id="0836c-116">A hello [portal](https://portal.azure.com), jelölje be **figyelő**.</span><span class="sxs-lookup"><span data-stu-id="0836c-116">In hello [portal](https://portal.azure.com), select **Monitor**.</span></span> <span data-ttu-id="0836c-117">Hello **figyelő** panel összes figyelési beállítások és adatok az egyik nézetben összesíti.</span><span class="sxs-lookup"><span data-stu-id="0836c-117">hello **Monitor** blade consolidates all your monitoring settings and data in one view.</span></span>

    ![hello "Figyelés" szolgáltatás](./media/monitoring-action-groups/home-monitor.png)
2. <span data-ttu-id="0836c-119">A hello **tevékenységnapló** szakaszban jelölje be **művelet csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0836c-119">In hello **Activity log** section, select **Action groups**.</span></span>

    ![hello "Művelet csoportok" lap](./media/monitoring-action-groups/action-groups-blade.png)
3. <span data-ttu-id="0836c-121">Válassza ki **művelet csoport hozzáadása**, és töltse ki hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="0836c-121">Select **Add action group**, and fill in hello fields.</span></span>

    ![hello "Művelet csoport hozzáadása" parancs](./media/monitoring-action-groups/add-action-group.png)
4. <span data-ttu-id="0836c-123">Adjon meg egy nevet a hello **művelet csoportnév** mezőbe, majd írjon be egy nevet a hello **rövid név** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="0836c-123">Enter a name in hello **Action group name** box, and enter a name in hello **Short name** box.</span></span> <span data-ttu-id="0836c-124">hello rövid nevét helyett egy teljes művelet felügyeleticsoport-nevet használja, ha ez a csoport értesítések küldése.</span><span class="sxs-lookup"><span data-stu-id="0836c-124">hello short name is used in place of a full action group name when notifications are sent using this group.</span></span>

      ![hello a művelet csoport hozzáadása"párbeszédpanel](./media/monitoring-action-groups/action-group-define.png)

5. <span data-ttu-id="0836c-126">Hello **előfizetés** a jelenlegi előfizetés autofills mezőben.</span><span class="sxs-lookup"><span data-stu-id="0836c-126">hello **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="0836c-127">Ez az előfizetés egy hello mely hello művelet csoport menti.</span><span class="sxs-lookup"><span data-stu-id="0836c-127">This subscription is hello one in which hello action group is saved.</span></span>

6. <span data-ttu-id="0836c-128">Jelölje be hello **erőforráscsoport** mely hello művelet csoport-t menti.</span><span class="sxs-lookup"><span data-stu-id="0836c-128">Select hello **Resource group** in which hello action group is saved.</span></span>

7. <span data-ttu-id="0836c-129">Adja meg azon műveletek listáját, adja meg a minden egyes művelethez:</span><span class="sxs-lookup"><span data-stu-id="0836c-129">Define a list of actions by providing each action's:</span></span>

    <span data-ttu-id="0836c-130">a.</span><span class="sxs-lookup"><span data-stu-id="0836c-130">a.</span></span> <span data-ttu-id="0836c-131">**Név**: Adjon meg egy egyedi azonosítót ehhez a művelethez.</span><span class="sxs-lookup"><span data-stu-id="0836c-131">**Name**: Enter a unique identifier for this action.</span></span>

    <span data-ttu-id="0836c-132">b.</span><span class="sxs-lookup"><span data-stu-id="0836c-132">b.</span></span> <span data-ttu-id="0836c-133">**Művelet típusa**: válassza ki az SMS, e-mailek vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="0836c-133">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="0836c-134">c.</span><span class="sxs-lookup"><span data-stu-id="0836c-134">c.</span></span> <span data-ttu-id="0836c-135">**Részletek**: hello művelet típusa alapján, adjon meg egy telefonszámot, az e-mail címet, illetve a webhook URI.</span><span class="sxs-lookup"><span data-stu-id="0836c-135">**Details**: Based on hello action type, enter a phone number, email address, or webhook URI.</span></span>

8. <span data-ttu-id="0836c-136">Válassza ki **OK** toocreate hello művelet csoport.</span><span class="sxs-lookup"><span data-stu-id="0836c-136">Select **OK** toocreate hello action group.</span></span>

## <a name="manage-your-action-groups"></a><span data-ttu-id="0836c-137">A művelet csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="0836c-137">Manage your action groups</span></span> ##
<span data-ttu-id="0836c-138">Egy művelet csoport létrehozása után is látható a hello **művelet csoportok** hello szakasza **figyelő** panelen.</span><span class="sxs-lookup"><span data-stu-id="0836c-138">After you create an action group, it's visible in hello **Action groups** section of hello **Monitor** blade.</span></span> <span data-ttu-id="0836c-139">Válassza ki hello művelet csoportot, hogy toomanage:</span><span class="sxs-lookup"><span data-stu-id="0836c-139">Select hello action group you want toomanage to:</span></span>

* <span data-ttu-id="0836c-140">Adja hozzá, szerkeszthet és eltávolíthat műveletek.</span><span class="sxs-lookup"><span data-stu-id="0836c-140">Add, edit, or remove actions.</span></span>
* <span data-ttu-id="0836c-141">Hello művelet csoport törlése.</span><span class="sxs-lookup"><span data-stu-id="0836c-141">Delete hello action group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0836c-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0836c-142">Next steps</span></span> ##
* <span data-ttu-id="0836c-143">További információ [SMS riasztási viselkedés](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="0836c-143">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>  
* <span data-ttu-id="0836c-144">Szerezzen egy [hello műveletnapló riasztási webhook séma megismerni](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="0836c-144">Gain an [understanding of hello activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>  
* <span data-ttu-id="0836c-145">További információ [sebességkorlátozást](monitoring-alerts-rate-limiting.md) értesítésekről.</span><span class="sxs-lookup"><span data-stu-id="0836c-145">Learn more about [rate limiting](monitoring-alerts-rate-limiting.md) on alerts.</span></span> 
* <span data-ttu-id="0836c-146">Lekérése egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md), és megtudhatja, hogyan tooreceive riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="0836c-146">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how tooreceive alerts.</span></span>  
* <span data-ttu-id="0836c-147">Ismerje meg, hogyan túl[riasztások konfigurálása, ha az állapotfigyelő szolgáltatáshoz értesítést visszaküldi](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="0836c-147">Learn how too[configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
