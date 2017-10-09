---
title: az Azure Security Centerben aaaPermissions |} Microsoft Docs
description: "Ez a cikk azt ismerteti, hogyan használja az Azure Security Center a szerepköralapú hozzáférés-vezérlési tooassign engedélyek toousers, és azonosítja az egyes szerepkörökhöz műveletek engedélyezett hello."
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
ms.openlocfilehash: 03e16132dc3d951ef8ad9e86b9970b9e4d15c76b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-in-azure-security-center"></a><span data-ttu-id="fdd9f-103">Engedélyek az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="fdd9f-103">Permissions in Azure Security Center</span></span>

<span data-ttu-id="fdd9f-104">Az Azure Security Center által használt [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md), amely biztosítja [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md) , amely a toousers, csoportok és az Azure rendelhetők.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-104">Azure Security Center uses [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md), which provides [built-in roles](../active-directory/role-based-access-built-in-roles.md) that can be assigned toousers, groups, and services in Azure.</span></span>

<span data-ttu-id="fdd9f-105">A Security Center értékeli az erőforrások tooidentify biztonsági problémák és biztonsági rések hello konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-105">Security Center assesses hello configuration of your resources tooidentify security issues and vulnerabilities.</span></span> <span data-ttu-id="fdd9f-106">A biztonsági központban, csak akkor jelenik meg információ tooa erőforrás kapcsolatos hello szerepkör tulajdonos, közreműködő vagy olvasó hello előfizetés vagy az erőforrás csoport, amely egy erőforrás tartozik.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-106">In Security Center, you only see information related tooa resource when you are assigned hello role of Owner, Contributor, or Reader for hello subscription or resource group that a resource belongs to.</span></span>

<span data-ttu-id="fdd9f-107">Továbbá toothese szerepkörök szerepkör is létezik két adott Security Center:</span><span class="sxs-lookup"><span data-stu-id="fdd9f-107">In addition toothese roles, there are two specific Security Center roles:</span></span>

* <span data-ttu-id="fdd9f-108">**Biztonsági olvasó**: toothis szerepkörhöz tartozó felhasználó rendelkezik jogosultságokkal tooSecurity Center megtekintése.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-108">**Security Reader**: A user that belongs toothis role has viewing rights tooSecurity Center.</span></span> <span data-ttu-id="fdd9f-109">hello felhasználó megtekintheti a javaslatok, a riasztások, a biztonsági házirend és a biztonsági állapotok, de nem módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-109">hello user can view recommendations, alerts, a security policy, and security states, but cannot make changes.</span></span>
* <span data-ttu-id="fdd9f-110">**Biztonsági rendszergazda**: egy felhasználó toothis szerepkörhöz tartozó azonos hello biztonsági olvasó, tartalomvédelmi hello rendelkezik és hello biztonsági házirend frissítése, és zárja be a riasztások és javaslatok.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-110">**Security Administrator**: A user that belongs toothis role has hello same rights as hello Security Reader and can also update hello security policy and dismiss alerts and recommendations.</span></span>

> [!NOTE]
> <span data-ttu-id="fdd9f-111">hello biztonsági szerepkörök, biztonsági olvasó és a biztonsági rendszergazda, csak a Security Center rendelkezik hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-111">hello security roles, Security Reader and Security Administrator, have access only in Security Center.</span></span> <span data-ttu-id="fdd9f-112">hello biztonsági szerepkörök nem rendelkezik hozzáférési tooother szolgáltatás például a tárolóhely & webes, mobil vagy az eszközök internetes hálózatát Azure területéhez.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-112">hello security roles do not have access tooother service areas of Azure such as Storage, Web & Mobile, or Internet of Things.</span></span>
>
>

## <a name="roles-and-allowed-actions"></a><span data-ttu-id="fdd9f-113">Szerepkörök és az engedélyezett műveletek</span><span class="sxs-lookup"><span data-stu-id="fdd9f-113">Roles and allowed actions</span></span>

<span data-ttu-id="fdd9f-114">hello következő táblázat megjeleníti a szerepkört, és a Security Center műveletek engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-114">hello following table displays roles and allowed actions in Security Center.</span></span> <span data-ttu-id="fdd9f-115">Az X jelzi, hogy ez a szerepkör hello művelet engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-115">An X indicates that hello action is allowed for that role.</span></span>

| <span data-ttu-id="fdd9f-116">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="fdd9f-116">Role</span></span> | <span data-ttu-id="fdd9f-117">Biztonsági házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="fdd9f-117">Edit security policy</span></span> | <span data-ttu-id="fdd9f-118">Egy erőforrás biztonsági javaslatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="fdd9f-118">Apply security recommendations for a resource</span></span> | <span data-ttu-id="fdd9f-119">Hagyja figyelmen kívül a riasztások és javaslatok</span><span class="sxs-lookup"><span data-stu-id="fdd9f-119">Dismiss alerts and recommendations</span></span> | <span data-ttu-id="fdd9f-120">Riasztások megtekintése és javaslatok</span><span class="sxs-lookup"><span data-stu-id="fdd9f-120">View alerts and recommendations</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="fdd9f-121">Előfizetés tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="fdd9f-121">Subscription Owner</span></span> | <span data-ttu-id="fdd9f-122">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-122">X</span></span> | <span data-ttu-id="fdd9f-123">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-123">X</span></span> | <span data-ttu-id="fdd9f-124">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-124">X</span></span> | <span data-ttu-id="fdd9f-125">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-125">X</span></span> |
| <span data-ttu-id="fdd9f-126">Előfizetés közreműködő</span><span class="sxs-lookup"><span data-stu-id="fdd9f-126">Subscription Contributor</span></span> | <span data-ttu-id="fdd9f-127">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-127">X</span></span> | <span data-ttu-id="fdd9f-128">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-128">X</span></span> | <span data-ttu-id="fdd9f-129">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-129">X</span></span> | <span data-ttu-id="fdd9f-130">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-130">X</span></span> |
| <span data-ttu-id="fdd9f-131">Erőforráscsoport tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="fdd9f-131">Resource Group Owner</span></span> | -- | <span data-ttu-id="fdd9f-132">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-132">X</span></span> | -- | <span data-ttu-id="fdd9f-133">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-133">X</span></span> |
| <span data-ttu-id="fdd9f-134">Erőforráscsoport közreműködő</span><span class="sxs-lookup"><span data-stu-id="fdd9f-134">Resource Group Contributor</span></span> | -- | <span data-ttu-id="fdd9f-135">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-135">X</span></span> | -- | <span data-ttu-id="fdd9f-136">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-136">X</span></span> |
| <span data-ttu-id="fdd9f-137">Olvasó</span><span class="sxs-lookup"><span data-stu-id="fdd9f-137">Reader</span></span> | -- | -- | -- | <span data-ttu-id="fdd9f-138">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-138">X</span></span> |
| <span data-ttu-id="fdd9f-139">Biztonsági rendszergazda</span><span class="sxs-lookup"><span data-stu-id="fdd9f-139">Security Administrator</span></span> | <span data-ttu-id="fdd9f-140">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-140">X</span></span> | -- | <span data-ttu-id="fdd9f-141">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-141">X</span></span> | <span data-ttu-id="fdd9f-142">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-142">X</span></span> |
| <span data-ttu-id="fdd9f-143">Biztonsági olvasó</span><span class="sxs-lookup"><span data-stu-id="fdd9f-143">Security Reader</span></span> | -- | -- | -- | <span data-ttu-id="fdd9f-144">X</span><span class="sxs-lookup"><span data-stu-id="fdd9f-144">X</span></span> |

> [!NOTE]
> <span data-ttu-id="fdd9f-145">Azt javasoljuk, hogy rendelje hello legkevésbé megengedő szerepkörre felhasználók toocomplete a feladataik ellátásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-145">We recommend that you assign hello least permissive role needed for users toocomplete their tasks.</span></span> <span data-ttu-id="fdd9f-146">Például rendeljen hello olvasó szerepkört toousers csak a tooview erőforrás biztonsági állapota hello információra van szüksége, de nem tesznek lépéseket, például a alkalmazhatnak javaslatokat, és módosíthatják a szabályzatokat.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-146">For example, assign hello Reader role toousers who only need tooview information about hello security health of a resource but not take action, such as applying recommendations or editing policies.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="fdd9f-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdd9f-147">Next steps</span></span>
<span data-ttu-id="fdd9f-148">Ez a cikk azt, hogyan használja a Security Center az RBAC tooassign engedélyek toousers, és engedélyezett műveletek az egyes szerepkörökhöz hello azonosított.</span><span class="sxs-lookup"><span data-stu-id="fdd9f-148">This article explained how Security Center uses RBAC tooassign permissions toousers and identified hello allowed actions for each role.</span></span> <span data-ttu-id="fdd9f-149">Most, hogy ismeri hello szerepkör-hozzárendelések toomonitor hello szükséges biztonsági állapotát, az előfizetés, szerkesztheti a biztonsági szabályzatokat, és a javaslatok alkalmazni, megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="fdd9f-149">Now that you're familiar with hello role assignments needed toomonitor hello security state of your subscription, edit security policies, and apply recommendations, learn how to:</span></span>

- [<span data-ttu-id="fdd9f-150">A Security Center biztonsági házirendek beállítása</span><span class="sxs-lookup"><span data-stu-id="fdd9f-150">Set security policies in Security Center</span></span>](security-center-policies.md)
- [<span data-ttu-id="fdd9f-151">A Security Center biztonsági javaslatok kezelése</span><span class="sxs-lookup"><span data-stu-id="fdd9f-151">Manage security recommendations in Security Center</span></span>](security-center-recommendations.md)
- [<span data-ttu-id="fdd9f-152">Az Azure-erőforrások biztonsági állapotát hello figyelése</span><span class="sxs-lookup"><span data-stu-id="fdd9f-152">Monitor hello security health of your Azure resources</span></span>](security-center-monitoring.md)
- [<span data-ttu-id="fdd9f-153">Kezelésének és megoldásának toosecurity riasztásokat a Security Center</span><span class="sxs-lookup"><span data-stu-id="fdd9f-153">Manage and respond toosecurity alerts in Security Center</span></span>](security-center-managing-and-responding-alerts.md)
- [<span data-ttu-id="fdd9f-154">Biztonsági partnermegoldások figyelése</span><span class="sxs-lookup"><span data-stu-id="fdd9f-154">Monitor partner security solutions</span></span>](security-center-partner-solutions.md)
