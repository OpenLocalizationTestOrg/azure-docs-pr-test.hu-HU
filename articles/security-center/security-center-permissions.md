---
title: "Engedélyek az Azure Security Centerben |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan az Azure Security Center segítségével szerepköralapú hozzáférés-vezérlési engedélyek hozzárendelése a felhasználókhoz, és az egyes szerepkörökhöz engedélyezett műveleteket azonosítja."
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: 
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: 0aaa99dda44d2020afd3e841e84020eb4ff87a85
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="permissions-in-azure-security-center"></a><span data-ttu-id="d3e2c-103">Engedélyek az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="d3e2c-103">Permissions in Azure Security Center</span></span>

<span data-ttu-id="d3e2c-104">Az Azure Security Center [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) használ, amelynek [beépített szerepköreit](../active-directory/role-based-access-built-in-roles.md) az Azure különböző csoportjaihoz, felhasználóihoz és szolgáltatásaihoz rendelheti.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-104">Azure Security Center uses [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md), which provides [built-in roles](../active-directory/role-based-access-built-in-roles.md) that can be assigned to users, groups, and services in Azure.</span></span>

<span data-ttu-id="d3e2c-105">A Security Center értékeli a konfigurációs az erőforrások biztonsági problémák és biztonsági rések azonosítása.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-105">Security Center assesses the configuration of your resources to identify security issues and vulnerabilities.</span></span> <span data-ttu-id="d3e2c-106">A biztonsági központban csak akkor jelenik meg egy erőforrás tulajdonos, közreműködő vagy olvasó szerepkört az előfizetés vagy az erőforráscsoportot, amelyhez tartozik egy erőforrás hozzárendelése esetén kapcsolatos adatokat.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-106">In Security Center, you only see information related to a resource when you are assigned the role of Owner, Contributor, or Reader for the subscription or resource group that a resource belongs to.</span></span>

<span data-ttu-id="d3e2c-107">Ezen szerepkörök mellett két speciális Security Center-szerepkör van:</span><span class="sxs-lookup"><span data-stu-id="d3e2c-107">In addition to these roles, there are two specific Security Center roles:</span></span>

* <span data-ttu-id="d3e2c-108">**Biztonsági olvasó**: ehhez a szerepkörhöz tartozó felhasználó van jogosultsága ahhoz, hogy a Security Center megtekintése.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-108">**Security Reader**: A user that belongs to this role has viewing rights to Security Center.</span></span> <span data-ttu-id="d3e2c-109">A felhasználó megtekintheti a javaslatok, a riasztások, a biztonsági házirend és a biztonsági állapotok, de nem módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-109">The user can view recommendations, alerts, a security policy, and security states, but cannot make changes.</span></span>
* <span data-ttu-id="d3e2c-110">**Biztonsági rendszergazda**: ehhez a szerepkörhöz tartozó felhasználó a biztonsági olvasó megegyező jogokkal rendelkezik és is frissíteni a biztonsági házirendet, és hagyja figyelmen kívül a riasztások és javaslatok.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-110">**Security Administrator**: A user that belongs to this role has the same rights as the Security Reader and can also update the security policy and dismiss alerts and recommendations.</span></span>

> [!NOTE]
> <span data-ttu-id="d3e2c-111">A biztonsági szerepkörök, biztonsági olvasó és a biztonsági rendszergazda, csak a Security Center rendelkezik hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-111">The security roles, Security Reader and Security Administrator, have access only in Security Center.</span></span> <span data-ttu-id="d3e2c-112">A biztonsági szerepkörök az Azure tárolási, webes & mobilalkalmazás vagy az eszközök internetes hálózatát más szolgáltatás területéhez nincs hozzáférése.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-112">The security roles do not have access to other service areas of Azure such as Storage, Web & Mobile, or Internet of Things.</span></span>
>
>

## <a name="roles-and-allowed-actions"></a><span data-ttu-id="d3e2c-113">Szerepkörök és az engedélyezett műveletek</span><span class="sxs-lookup"><span data-stu-id="d3e2c-113">Roles and allowed actions</span></span>

<span data-ttu-id="d3e2c-114">A következő táblázat megjeleníti a szerepkört, és a Security Center műveletek engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-114">The following table displays roles and allowed actions in Security Center.</span></span> <span data-ttu-id="d3e2c-115">Az X jelzi, hogy a művelet engedélyezett-e az adott szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-115">An X indicates that the action is allowed for that role.</span></span>

| <span data-ttu-id="d3e2c-116">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="d3e2c-116">Role</span></span> | <span data-ttu-id="d3e2c-117">Biztonsági házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="d3e2c-117">Edit security policy</span></span> | <span data-ttu-id="d3e2c-118">Egy erőforrás biztonsági javaslatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="d3e2c-118">Apply security recommendations for a resource</span></span> | <span data-ttu-id="d3e2c-119">Hagyja figyelmen kívül a riasztások és javaslatok</span><span class="sxs-lookup"><span data-stu-id="d3e2c-119">Dismiss alerts and recommendations</span></span> | <span data-ttu-id="d3e2c-120">Riasztások megtekintése és javaslatok</span><span class="sxs-lookup"><span data-stu-id="d3e2c-120">View alerts and recommendations</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="d3e2c-121">Előfizetés tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="d3e2c-121">Subscription Owner</span></span> | <span data-ttu-id="d3e2c-122">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-122">X</span></span> | <span data-ttu-id="d3e2c-123">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-123">X</span></span> | <span data-ttu-id="d3e2c-124">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-124">X</span></span> | <span data-ttu-id="d3e2c-125">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-125">X</span></span> |
| <span data-ttu-id="d3e2c-126">Előfizetés közreműködő</span><span class="sxs-lookup"><span data-stu-id="d3e2c-126">Subscription Contributor</span></span> | <span data-ttu-id="d3e2c-127">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-127">X</span></span> | <span data-ttu-id="d3e2c-128">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-128">X</span></span> | <span data-ttu-id="d3e2c-129">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-129">X</span></span> | <span data-ttu-id="d3e2c-130">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-130">X</span></span> |
| <span data-ttu-id="d3e2c-131">Erőforráscsoport tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="d3e2c-131">Resource Group Owner</span></span> | -- | <span data-ttu-id="d3e2c-132">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-132">X</span></span> | -- | <span data-ttu-id="d3e2c-133">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-133">X</span></span> |
| <span data-ttu-id="d3e2c-134">Erőforráscsoport közreműködő</span><span class="sxs-lookup"><span data-stu-id="d3e2c-134">Resource Group Contributor</span></span> | -- | <span data-ttu-id="d3e2c-135">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-135">X</span></span> | -- | <span data-ttu-id="d3e2c-136">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-136">X</span></span> |
| <span data-ttu-id="d3e2c-137">Olvasó</span><span class="sxs-lookup"><span data-stu-id="d3e2c-137">Reader</span></span> | -- | -- | -- | <span data-ttu-id="d3e2c-138">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-138">X</span></span> |
| <span data-ttu-id="d3e2c-139">Biztonsági rendszergazda</span><span class="sxs-lookup"><span data-stu-id="d3e2c-139">Security Administrator</span></span> | <span data-ttu-id="d3e2c-140">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-140">X</span></span> | -- | <span data-ttu-id="d3e2c-141">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-141">X</span></span> | <span data-ttu-id="d3e2c-142">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-142">X</span></span> |
| <span data-ttu-id="d3e2c-143">Biztonsági olvasó</span><span class="sxs-lookup"><span data-stu-id="d3e2c-143">Security Reader</span></span> | -- | -- | -- | <span data-ttu-id="d3e2c-144">X</span><span class="sxs-lookup"><span data-stu-id="d3e2c-144">X</span></span> |

> [!NOTE]
> <span data-ttu-id="d3e2c-145">Javasoljuk, hogy a felhasználókhoz azt a lehető legalacsonyabb szintű szerepkört rendelje, amellyel még el tudják végezni feladataikat.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-145">We recommend that you assign the least permissive role needed for users to complete their tasks.</span></span> <span data-ttu-id="d3e2c-146">Például az olvasó szerepkört rendelni felhasználók, akik csak egy erőforrás biztonsági állapotával kapcsolatos információk megtekintése, de nem tesznek lépéseket, például a alkalmazhatnak javaslatokat, és módosíthatják a szabályzatokat.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-146">For example, assign the Reader role to users who only need to view information about the security health of a resource but not take action, such as applying recommendations or editing policies.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="d3e2c-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3e2c-147">Next steps</span></span>
<span data-ttu-id="d3e2c-148">Ez a cikk alapján hogyan Security Center segítségével RBAC engedélyek hozzárendelése a felhasználókhoz, és azonosítja az egyes szerepkörökhöz engedélyezett műveletek.</span><span class="sxs-lookup"><span data-stu-id="d3e2c-148">This article explained how Security Center uses RBAC to assign permissions to users and identified the allowed actions for each role.</span></span> <span data-ttu-id="d3e2c-149">Most, hogy ismeri az előfizetés biztonsági állapotának figyeléséhez szükséges szerepkör-hozzárendelések, szerkesztheti a biztonsági szabályzatokat, és a javaslatok alkalmazni, megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="d3e2c-149">Now that you're familiar with the role assignments needed to monitor the security state of your subscription, edit security policies, and apply recommendations, learn how to:</span></span>

- [<span data-ttu-id="d3e2c-150">A Security Center biztonsági házirendek beállítása</span><span class="sxs-lookup"><span data-stu-id="d3e2c-150">Set security policies in Security Center</span></span>](security-center-policies.md)
- [<span data-ttu-id="d3e2c-151">A Security Center biztonsági javaslatok kezelése</span><span class="sxs-lookup"><span data-stu-id="d3e2c-151">Manage security recommendations in Security Center</span></span>](security-center-recommendations.md)
- [<span data-ttu-id="d3e2c-152">Az Azure-erőforrások biztonsági állapotának figyelésére</span><span class="sxs-lookup"><span data-stu-id="d3e2c-152">Monitor the security health of your Azure resources</span></span>](security-center-monitoring.md)
- [<span data-ttu-id="d3e2c-153">A biztonsági riasztások kezelése és a riasztásokra való válaszadás a Security Centerben</span><span class="sxs-lookup"><span data-stu-id="d3e2c-153">Manage and respond to security alerts in Security Center</span></span>](security-center-managing-and-responding-alerts.md)
- [<span data-ttu-id="d3e2c-154">Biztonsági partnermegoldások figyelése</span><span class="sxs-lookup"><span data-stu-id="d3e2c-154">Monitor partner security solutions</span></span>](security-center-partner-solutions.md)
