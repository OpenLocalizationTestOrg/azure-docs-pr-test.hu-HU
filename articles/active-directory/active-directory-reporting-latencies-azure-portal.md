---
title: "Azure Active Directory jelentéskészítés késések |} Microsoft Docs"
description: "További tudnivalók az jelentési eseményeket az Azure-portálon jelenik meg a szükséges idő"
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
ms.openlocfilehash: 93cb0baeab8f13f81257ed1bd32ed08561c54b72
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-reporting-latencies"></a><span data-ttu-id="ab1c4-103">Azure Active Directory jelentéskészítés késések</span><span class="sxs-lookup"><span data-stu-id="ab1c4-103">Azure Active Directory reporting latencies</span></span>

<span data-ttu-id="ab1c4-104">A [reporting](active-directory-preview-explainer.md) az Azure Active Directoryban, meg kell határoznia, hogyan működik a környezet összes információt lekérni.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-104">With [reporting](active-directory-preview-explainer.md) in the Azure Active Directory, you get all the information you need to determine how your environment is doing.</span></span> <span data-ttu-id="ab1c4-105">Mennyi ideig tart a jelentési adatok jelennek meg az Azure-portálon késés is nevezik.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-105">The amount of time it takes for reporting data to show up in the Azure portal is also known as latency.</span></span> 

<span data-ttu-id="ab1c4-106">Ez a témakör ismerteti az Azure portálon található összes jelentési kategóriák késési adatok.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-106">This topic lists the latency information for the all reporting categories in the Azure portal.</span></span> 


## <a name="activity-reports"></a><span data-ttu-id="ab1c4-107">Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="ab1c4-107">Activity reports</span></span>

<span data-ttu-id="ab1c4-108">Nincsenek Tevékenységjelentés két terület:</span><span class="sxs-lookup"><span data-stu-id="ab1c4-108">There are two areas of activity reporting:</span></span>

- <span data-ttu-id="ab1c4-109">**Bejelentkezési tevékenységek** – A felügyelt alkalmazások használatával és a felhasználók bejelentkezési tevékenységeivel kapcsolatos információk</span><span class="sxs-lookup"><span data-stu-id="ab1c4-109">**Sign-in activities** – Information about the usage of managed applications and user sign-in activities</span></span>
- <span data-ttu-id="ab1c4-110">**Naplók** – Rendszertevékenység információk a felhasználó- és csoportfelügyeletre, valamint a felügyelt alkalmazásokra és a címtártevékenységekre vonatkozóan</span><span class="sxs-lookup"><span data-stu-id="ab1c4-110">**Audit logs** - System activity information about users and group management, your managed applications and directory activities</span></span>

<span data-ttu-id="ab1c4-111">A következő táblázat a késési adatok Tevékenységjelentések.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-111">The following table lists the latency information for activity reports.</span></span>

| <span data-ttu-id="ab1c4-112">Jelentés</span><span class="sxs-lookup"><span data-stu-id="ab1c4-112">Report</span></span> | <span data-ttu-id="ab1c4-113">Minimális</span><span class="sxs-lookup"><span data-stu-id="ab1c4-113">Minimum</span></span> | <span data-ttu-id="ab1c4-114">Átlagos</span><span class="sxs-lookup"><span data-stu-id="ab1c4-114">Average</span></span> | <span data-ttu-id="ab1c4-115">Maximális</span><span class="sxs-lookup"><span data-stu-id="ab1c4-115">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="ab1c4-116">Naplók</span><span class="sxs-lookup"><span data-stu-id="ab1c4-116">Audit logs</span></span>             | <span data-ttu-id="ab1c4-117">30 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-117">30 minutes</span></span>  | <span data-ttu-id="ab1c4-118">45 percben</span><span class="sxs-lookup"><span data-stu-id="ab1c4-118">45 Minutes</span></span> | <span data-ttu-id="ab1c4-119">1 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-119">1 hour</span></span>     |
| <span data-ttu-id="ab1c4-120">Bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="ab1c4-120">Sign-ins</span></span>               | <span data-ttu-id="ab1c4-121">15 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-121">15 minutes</span></span>  | <span data-ttu-id="ab1c4-122">15 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-122">15 minutes</span></span> | <span data-ttu-id="ab1c4-123">2 óra *</span><span class="sxs-lookup"><span data-stu-id="ab1c4-123">2 hours*</span></span>   |

>[!NOTE]
> <span data-ttu-id="ab1c4-124">Egyes örökölt Office-alkalmazások bejelentkezési tevékenységeinek adatai esetében akár 8 órát is igénybe vehet, amíg a naplóadatok megjelennek.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-124">For some sign-ins activity data coming from legacy office applications, it can take to 8 hours for the reporting data to show up.</span></span> 


## <a name="security-reports"></a><span data-ttu-id="ab1c4-125">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="ab1c4-125">Security reports</span></span>

<span data-ttu-id="ab1c4-126">Nincsenek biztonsági reporting két terület:</span><span class="sxs-lookup"><span data-stu-id="ab1c4-126">There are two areas of security reporting:</span></span>

- <span data-ttu-id="ab1c4-127">**Kockázatos bejelentkezések** – A kockázatos bejelentkezés egy olyan bejelentkezési kísérletet jelöl, amelyet elképzelhető, hogy olyan személy hajtott végre, aki nem a felhasználói fiók jogos tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-127">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.</span></span> 
- <span data-ttu-id="ab1c4-128">**Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-128">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> 

<span data-ttu-id="ab1c4-129">A következő táblázat a késési adatok biztonsági jelentések.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-129">The following table lists the latency information for security reports.</span></span>

| <span data-ttu-id="ab1c4-130">Jelentés</span><span class="sxs-lookup"><span data-stu-id="ab1c4-130">Report</span></span> | <span data-ttu-id="ab1c4-131">Minimális</span><span class="sxs-lookup"><span data-stu-id="ab1c4-131">Minimum</span></span> | <span data-ttu-id="ab1c4-132">Átlagos</span><span class="sxs-lookup"><span data-stu-id="ab1c4-132">Average</span></span> | <span data-ttu-id="ab1c4-133">Maximális</span><span class="sxs-lookup"><span data-stu-id="ab1c4-133">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="ab1c4-134">Érintett felhasználók</span><span class="sxs-lookup"><span data-stu-id="ab1c4-134">Users at risk</span></span>          | <span data-ttu-id="ab1c4-135">5 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-135">5 minutes</span></span>   | <span data-ttu-id="ab1c4-136">15 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-136">15 minutes</span></span>  | <span data-ttu-id="ab1c4-137">2 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-137">2 hours</span></span>  |
| <span data-ttu-id="ab1c4-138">Kockázatos bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="ab1c4-138">Risky sign-ins</span></span>         | <span data-ttu-id="ab1c4-139">5 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-139">5 minutes</span></span>   | <span data-ttu-id="ab1c4-140">15 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-140">15 minutes</span></span>  | <span data-ttu-id="ab1c4-141">2 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-141">2 hours</span></span>  |

## <a name="risk-events"></a><span data-ttu-id="ab1c4-142">Kockázati események</span><span class="sxs-lookup"><span data-stu-id="ab1c4-142">Risk events</span></span>

<span data-ttu-id="ab1c4-143">Az Azure Active Directory adaptív gépi tanulási algoritmusok és heurisztikus használja, amely kapcsolódik a felhasználói fiókok gyanús tevékenységek észlelése.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-143">Azure Active Directory uses adaptive machine learning algorithms and heuristics to detect suspicious actions that are related to your user accounts.</span></span> <span data-ttu-id="ab1c4-144">Minden észlelt gyanús művelet egy rekord hívott kockázat esemény van tárolva.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-144">Each detected suspicious action is stored in a record called risk event.</span></span>

<span data-ttu-id="ab1c4-145">A következő táblázat a késési adatok kockázati eseményekről.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-145">The following table lists the latency information for risk events.</span></span>

| <span data-ttu-id="ab1c4-146">Jelentés</span><span class="sxs-lookup"><span data-stu-id="ab1c4-146">Report</span></span> | <span data-ttu-id="ab1c4-147">Minimális</span><span class="sxs-lookup"><span data-stu-id="ab1c4-147">Minimum</span></span> | <span data-ttu-id="ab1c4-148">Átlagos</span><span class="sxs-lookup"><span data-stu-id="ab1c4-148">Average</span></span> | <span data-ttu-id="ab1c4-149">Maximális</span><span class="sxs-lookup"><span data-stu-id="ab1c4-149">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="ab1c4-150">Névtelen IP-címről történő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="ab1c4-150">Sign-ins from anonymous IP addresses</span></span> |<span data-ttu-id="ab1c4-151">5 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-151">5 minutes</span></span> |<span data-ttu-id="ab1c4-152">15 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-152">15 Minutes</span></span> |<span data-ttu-id="ab1c4-153">2 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-153">2 hours</span></span> |
| <span data-ttu-id="ab1c4-154">Ismeretlen helyekről történt bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="ab1c4-154">Sign-ins from unfamiliar locations</span></span> |<span data-ttu-id="ab1c4-155">5 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-155">5 minutes</span></span> |<span data-ttu-id="ab1c4-156">15 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-156">15 Minutes</span></span> |<span data-ttu-id="ab1c4-157">2 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-157">2 hours</span></span> |
| <span data-ttu-id="ab1c4-158">Felhasználók, akiknek kiszivárogtak a hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="ab1c4-158">Users with leaked credentials</span></span> |<span data-ttu-id="ab1c4-159">2 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-159">2 hours</span></span> |<span data-ttu-id="ab1c4-160">4 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-160">4 hours</span></span> |<span data-ttu-id="ab1c4-161">8 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-161">8 hours</span></span> |
| <span data-ttu-id="ab1c4-162">Bejelentkezés szokatlan helyekről</span><span class="sxs-lookup"><span data-stu-id="ab1c4-162">Impossible travel to atypical locations</span></span> |<span data-ttu-id="ab1c4-163">5 perc</span><span class="sxs-lookup"><span data-stu-id="ab1c4-163">5 minutes</span></span> |<span data-ttu-id="ab1c4-164">1 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-164">1 hour</span></span> |<span data-ttu-id="ab1c4-165">8 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-165">8 hours</span></span>  |
| <span data-ttu-id="ab1c4-166">Bejelentkezések fertőzött eszközökről</span><span class="sxs-lookup"><span data-stu-id="ab1c4-166">Sign-ins from infected devices</span></span> |<span data-ttu-id="ab1c4-167">2 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-167">2 hours</span></span> |<span data-ttu-id="ab1c4-168">4 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-168">4 hours</span></span> |<span data-ttu-id="ab1c4-169">8 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-169">8 hours</span></span>  |
| <span data-ttu-id="ab1c4-170">Bejelentkezések gyanús tevékenységeket mutató IP-címekkel</span><span class="sxs-lookup"><span data-stu-id="ab1c4-170">Sign-ins from IP addresses with suspicious activity</span></span> |<span data-ttu-id="ab1c4-171">2 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-171">2 hours</span></span> |<span data-ttu-id="ab1c4-172">4 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-172">4 hours</span></span> |<span data-ttu-id="ab1c4-173">8 óra</span><span class="sxs-lookup"><span data-stu-id="ab1c4-173">8 hours</span></span>  |



## <a name="next-steps"></a><span data-ttu-id="ab1c4-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab1c4-174">Next steps</span></span>

<span data-ttu-id="ab1c4-175">Ha azt szeretné, további információkat az Azure-portálon a Tevékenységjelentések, lásd:</span><span class="sxs-lookup"><span data-stu-id="ab1c4-175">If you want to know more about the activity reports in the Azure portal, see:</span></span>

- [<span data-ttu-id="ab1c4-176">Bejelentkezési tevékenység jelentések az Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="ab1c4-176">Sign-in activity reports in the Azure Active Directory portal</span></span>](active-directory-reporting-activity-sign-ins.md)
- [<span data-ttu-id="ab1c4-177">Naplózási Tevékenységjelentések az Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="ab1c4-177">Audit activity reports in the Azure Active Directory portal</span></span>](active-directory-reporting-activity-audit-logs.md)

<span data-ttu-id="ab1c4-178">Ha azt szeretné, további információkat a biztonsági jelentések az Azure portálon, lásd:</span><span class="sxs-lookup"><span data-stu-id="ab1c4-178">If you want to know more about the security reports in the Azure portal, see:</span></span>

- [<span data-ttu-id="ab1c4-179">Kockázati biztonsági jelentést az Azure Active Directory portálon a felhasználók</span><span class="sxs-lookup"><span data-stu-id="ab1c4-179">Users at risk security report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="ab1c4-180">Az Azure Active Directory portálon kockázatos bejelentkezések jelentés</span><span class="sxs-lookup"><span data-stu-id="ab1c4-180">Risky sign-ins report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)

<span data-ttu-id="ab1c4-181">Ha azt szeretné, további információkat a kockázati eseményekről, lásd: [Azure Active Directory kockázati események](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="ab1c4-181">If you want to know more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>
