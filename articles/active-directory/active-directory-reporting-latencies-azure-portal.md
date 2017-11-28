---
title: "az Active Directory jelentéskészítési késések aaaAzure |} Microsoft Docs"
description: "További tudnivalók: hello mennyi ideig tart a jelentési események tooshow mentése az Azure-portálon"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
ms.openlocfilehash: eee959331262ba59b313dd038cb54699dbef48a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-latencies"></a><span data-ttu-id="b6864-103">Azure Active Directory jelentéskészítés késések</span><span class="sxs-lookup"><span data-stu-id="b6864-103">Azure Active Directory reporting latencies</span></span>

<span data-ttu-id="b6864-104">A [reporting](active-directory-preview-explainer.md) hello Azure Active Directory, az beszerzése toodetermine hogyan működik a környezetében szükséges összes hello információkat.</span><span class="sxs-lookup"><span data-stu-id="b6864-104">With [reporting](active-directory-preview-explainer.md) in hello Azure Active Directory, you get all hello information you need toodetermine how your environment is doing.</span></span> <span data-ttu-id="b6864-105">a jelentéskészítési adatok tooshow fel a hello Azure-portálon is várakozási időt hello mennyisége.</span><span class="sxs-lookup"><span data-stu-id="b6864-105">hello amount of time it takes for reporting data tooshow up in hello Azure portal is also known as latency.</span></span> 

<span data-ttu-id="b6864-106">Ez a témakör felsorolja hello késési adatok hello jelentéskészítési kategóriából hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b6864-106">This topic lists hello latency information for hello all reporting categories in hello Azure portal.</span></span> 


## <a name="activity-reports"></a><span data-ttu-id="b6864-107">Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="b6864-107">Activity reports</span></span>

<span data-ttu-id="b6864-108">Nincsenek Tevékenységjelentés két terület:</span><span class="sxs-lookup"><span data-stu-id="b6864-108">There are two areas of activity reporting:</span></span>

- <span data-ttu-id="b6864-109">**Bejelentkezési tevékenységek** – kezelt alkalmazások és a felhasználói bejelentkezési tevékenységek hello használatával kapcsolatos információért</span><span class="sxs-lookup"><span data-stu-id="b6864-109">**Sign-in activities** – Information about hello usage of managed applications and user sign-in activities</span></span>
- <span data-ttu-id="b6864-110">**Naplók** – Rendszertevékenység információk a felhasználó- és csoportfelügyeletre, valamint a felügyelt alkalmazásokra és a címtártevékenységekre vonatkozóan</span><span class="sxs-lookup"><span data-stu-id="b6864-110">**Audit logs** - System activity information about users and group management, your managed applications and directory activities</span></span>

<span data-ttu-id="b6864-111">a következő táblázat hello hello késési adatok Tevékenységjelentések sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b6864-111">hello following table lists hello latency information for activity reports.</span></span>

| <span data-ttu-id="b6864-112">Jelentés</span><span class="sxs-lookup"><span data-stu-id="b6864-112">Report</span></span> | <span data-ttu-id="b6864-113">Minimális</span><span class="sxs-lookup"><span data-stu-id="b6864-113">Minimum</span></span> | <span data-ttu-id="b6864-114">Átlagos</span><span class="sxs-lookup"><span data-stu-id="b6864-114">Average</span></span> | <span data-ttu-id="b6864-115">Maximális</span><span class="sxs-lookup"><span data-stu-id="b6864-115">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="b6864-116">Naplók</span><span class="sxs-lookup"><span data-stu-id="b6864-116">Audit logs</span></span>             | <span data-ttu-id="b6864-117">30 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-117">30 minutes</span></span>  | <span data-ttu-id="b6864-118">45 percben</span><span class="sxs-lookup"><span data-stu-id="b6864-118">45 Minutes</span></span> | <span data-ttu-id="b6864-119">1 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-119">1 hour</span></span>     |
| <span data-ttu-id="b6864-120">Bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="b6864-120">Sign-ins</span></span>               | <span data-ttu-id="b6864-121">15 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-121">15 minutes</span></span>  | <span data-ttu-id="b6864-122">15 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-122">15 minutes</span></span> | <span data-ttu-id="b6864-123">2 óra *</span><span class="sxs-lookup"><span data-stu-id="b6864-123">2 hours*</span></span>   |

>[!NOTE]
> <span data-ttu-id="b6864-124">Néhány bejelentkezések tevékenységre vonatkozó adatok az örökölt office-alkalmazások érkező hello adatok tooshow végzi a jelentéseket a too8 óráig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="b6864-124">For some sign-ins activity data coming from legacy office applications, it can take too8 hours for hello reporting data tooshow up.</span></span> 


## <a name="security-reports"></a><span data-ttu-id="b6864-125">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="b6864-125">Security reports</span></span>

<span data-ttu-id="b6864-126">Nincsenek biztonsági reporting két terület:</span><span class="sxs-lookup"><span data-stu-id="b6864-126">There are two areas of security reporting:</span></span>

- <span data-ttu-id="b6864-127">**Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója.</span><span class="sxs-lookup"><span data-stu-id="b6864-127">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> 
- <span data-ttu-id="b6864-128">**Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="b6864-128">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> 

<span data-ttu-id="b6864-129">a következő táblázat hello hello késési adatok biztonsági jelentések sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b6864-129">hello following table lists hello latency information for security reports.</span></span>

| <span data-ttu-id="b6864-130">Jelentés</span><span class="sxs-lookup"><span data-stu-id="b6864-130">Report</span></span> | <span data-ttu-id="b6864-131">Minimális</span><span class="sxs-lookup"><span data-stu-id="b6864-131">Minimum</span></span> | <span data-ttu-id="b6864-132">Átlagos</span><span class="sxs-lookup"><span data-stu-id="b6864-132">Average</span></span> | <span data-ttu-id="b6864-133">Maximális</span><span class="sxs-lookup"><span data-stu-id="b6864-133">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="b6864-134">Érintett felhasználók</span><span class="sxs-lookup"><span data-stu-id="b6864-134">Users at risk</span></span>          | <span data-ttu-id="b6864-135">5 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-135">5 minutes</span></span>   | <span data-ttu-id="b6864-136">15 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-136">15 minutes</span></span>  | <span data-ttu-id="b6864-137">2 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-137">2 hours</span></span>  |
| <span data-ttu-id="b6864-138">Kockázatos bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="b6864-138">Risky sign-ins</span></span>         | <span data-ttu-id="b6864-139">5 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-139">5 minutes</span></span>   | <span data-ttu-id="b6864-140">15 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-140">15 minutes</span></span>  | <span data-ttu-id="b6864-141">2 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-141">2 hours</span></span>  |

## <a name="risk-events"></a><span data-ttu-id="b6864-142">Kockázati események</span><span class="sxs-lookup"><span data-stu-id="b6864-142">Risk events</span></span>

<span data-ttu-id="b6864-143">Az Azure Active Directory adaptív gépi tanulási a algoritmusok és heurisztikus toodetect gyanús műveleteket kapcsolódó tooyour felhasználói fiókokat használ.</span><span class="sxs-lookup"><span data-stu-id="b6864-143">Azure Active Directory uses adaptive machine learning algorithms and heuristics toodetect suspicious actions that are related tooyour user accounts.</span></span> <span data-ttu-id="b6864-144">Minden észlelt gyanús művelet egy rekord hívott kockázat esemény van tárolva.</span><span class="sxs-lookup"><span data-stu-id="b6864-144">Each detected suspicious action is stored in a record called risk event.</span></span>

<span data-ttu-id="b6864-145">a következő táblázat hello hello késési adatok kockázati események sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b6864-145">hello following table lists hello latency information for risk events.</span></span>

| <span data-ttu-id="b6864-146">Jelentés</span><span class="sxs-lookup"><span data-stu-id="b6864-146">Report</span></span> | <span data-ttu-id="b6864-147">Minimális</span><span class="sxs-lookup"><span data-stu-id="b6864-147">Minimum</span></span> | <span data-ttu-id="b6864-148">Átlagos</span><span class="sxs-lookup"><span data-stu-id="b6864-148">Average</span></span> | <span data-ttu-id="b6864-149">Maximális</span><span class="sxs-lookup"><span data-stu-id="b6864-149">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="b6864-150">Névtelen IP-címről történő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="b6864-150">Sign-ins from anonymous IP addresses</span></span> |<span data-ttu-id="b6864-151">5 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-151">5 minutes</span></span> |<span data-ttu-id="b6864-152">15 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-152">15 Minutes</span></span> |<span data-ttu-id="b6864-153">2 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-153">2 hours</span></span> |
| <span data-ttu-id="b6864-154">Ismeretlen helyekről történt bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="b6864-154">Sign-ins from unfamiliar locations</span></span> |<span data-ttu-id="b6864-155">5 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-155">5 minutes</span></span> |<span data-ttu-id="b6864-156">15 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-156">15 Minutes</span></span> |<span data-ttu-id="b6864-157">2 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-157">2 hours</span></span> |
| <span data-ttu-id="b6864-158">Felhasználók, akiknek kiszivárogtak a hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="b6864-158">Users with leaked credentials</span></span> |<span data-ttu-id="b6864-159">2 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-159">2 hours</span></span> |<span data-ttu-id="b6864-160">4 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-160">4 hours</span></span> |<span data-ttu-id="b6864-161">8 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-161">8 hours</span></span> |
| <span data-ttu-id="b6864-162">Lehetetlen odautazás tooatypical helyek</span><span class="sxs-lookup"><span data-stu-id="b6864-162">Impossible travel tooatypical locations</span></span> |<span data-ttu-id="b6864-163">5 perc</span><span class="sxs-lookup"><span data-stu-id="b6864-163">5 minutes</span></span> |<span data-ttu-id="b6864-164">1 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-164">1 hour</span></span> |<span data-ttu-id="b6864-165">8 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-165">8 hours</span></span>  |
| <span data-ttu-id="b6864-166">Bejelentkezések fertőzött eszközökről</span><span class="sxs-lookup"><span data-stu-id="b6864-166">Sign-ins from infected devices</span></span> |<span data-ttu-id="b6864-167">2 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-167">2 hours</span></span> |<span data-ttu-id="b6864-168">4 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-168">4 hours</span></span> |<span data-ttu-id="b6864-169">8 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-169">8 hours</span></span>  |
| <span data-ttu-id="b6864-170">Bejelentkezések gyanús tevékenységeket mutató IP-címekkel</span><span class="sxs-lookup"><span data-stu-id="b6864-170">Sign-ins from IP addresses with suspicious activity</span></span> |<span data-ttu-id="b6864-171">2 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-171">2 hours</span></span> |<span data-ttu-id="b6864-172">4 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-172">4 hours</span></span> |<span data-ttu-id="b6864-173">8 óra</span><span class="sxs-lookup"><span data-stu-id="b6864-173">8 hours</span></span>  |



## <a name="next-steps"></a><span data-ttu-id="b6864-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6864-174">Next steps</span></span>

<span data-ttu-id="b6864-175">Ha azt szeretné, hogy az Azure-portálon hello hello tevékenység jelentésekkel kapcsolatos további tooknow, lásd:</span><span class="sxs-lookup"><span data-stu-id="b6864-175">If you want tooknow more about hello activity reports in hello Azure portal, see:</span></span>

- [<span data-ttu-id="b6864-176">Bejelentkezési tevékenység jelentések hello Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="b6864-176">Sign-in activity reports in hello Azure Active Directory portal</span></span>](active-directory-reporting-activity-sign-ins.md)
- [<span data-ttu-id="b6864-177">Naplózási Tevékenységjelentések hello Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="b6864-177">Audit activity reports in hello Azure Active Directory portal</span></span>](active-directory-reporting-activity-audit-logs.md)

<span data-ttu-id="b6864-178">Ha azt szeretné, hogy az Azure-portálon hello hello biztonsági jelentésekkel kapcsolatos további tooknow, lásd:</span><span class="sxs-lookup"><span data-stu-id="b6864-178">If you want tooknow more about hello security reports in hello Azure portal, see:</span></span>

- [<span data-ttu-id="b6864-179">A kockázat biztonsági jelentés hello Azure Active Directory portálon felhasználója</span><span class="sxs-lookup"><span data-stu-id="b6864-179">Users at risk security report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="b6864-180">Kockázatos bejelentkezések jelentés hello Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="b6864-180">Risky sign-ins report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)

<span data-ttu-id="b6864-181">Ha azt szeretné, hogy a kockázati eseményekről további tooknow, [Azure Active Directory kockázati események](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="b6864-181">If you want tooknow more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>
