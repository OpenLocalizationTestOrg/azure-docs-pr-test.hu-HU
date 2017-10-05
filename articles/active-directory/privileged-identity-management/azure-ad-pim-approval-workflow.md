---
title: "Azure Privileged Identity Management jóváhagyási munkafolyamatok |} Microsoft Docs"
description: "További tudnivalók a jóváhagyási munkafolyamatok Privileged Identity Management (PIM)"
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: cf6a9213fa0a1cba8725aabb42abe51b805ece7a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="approvals-preview"></a><span data-ttu-id="0699d-103">A jóváhagyások (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="0699d-103">Approvals (Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="0699d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0699d-104">Overview</span></span>

<span data-ttu-id="0699d-105">Privileged Identity Management-jóváhagyásokkal rendelkező jóváhagyás megkövetelése, az aktiváláshoz szerepkörök konfigurálása, és válasszon egy vagy több felhasználót, vagy delegált jóváhagyóknak csoportot.</span><span class="sxs-lookup"><span data-stu-id="0699d-105">With Approvals for Privileged Identity Management, you can configure roles to require approval for activation, and choose one or multiple users or groups as delegated approvers.</span></span> <span data-ttu-id="0699d-106">Megtudhatja, hogyan konfigurálhatók azok a szerepkörök, és válassza ki a jóváhagyóknak olvasási megtartása.</span><span class="sxs-lookup"><span data-stu-id="0699d-106">Keep reading to learn how to configure roles and select approvers.</span></span>

>[!NOTE]
<span data-ttu-id="0699d-107">Ne feledje, a szolgáltatás fejlesztés alatt van, és hibákat tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="0699d-107">Please keep in mind this feature is still in development, and you may encounter bugs.</span></span> <span data-ttu-id="0699d-108">A funkció leírásával elnevezési konvenciói változhat, és nem tekinthető végső.</span><span class="sxs-lookup"><span data-stu-id="0699d-108">The functionality, including text and naming conventions are subject to change, and should not be considered final.</span></span>


## <a name="key-terminology"></a><span data-ttu-id="0699d-109">Kulcs terminológia</span><span class="sxs-lookup"><span data-stu-id="0699d-109">Key Terminology</span></span>

<span data-ttu-id="0699d-110">*Jogosult szerepkör felhasználói* – az jogosult szerepkör felhasználó tulajdonképpen egy a szervezeten belül, amely szerint jogosult az Azure AD szerepkörhöz hozzárendelt felhasználó (szerepkör aktiválást igényel).</span><span class="sxs-lookup"><span data-stu-id="0699d-110">*Eligible Role User* – An eligible role user is a user within your organization that’s been assigned to an Azure AD role as eligible (role requires activation).</span></span>

<span data-ttu-id="0699d-111">*Meghatalmazott jóváhagyó* – egy delegált jóváhagyó egy vagy több egyéni felhasználók számára, vagy az Azure AD a csoportokat a szerepkörök aktiválásához kérelmek jóváhagyása felelős.</span><span class="sxs-lookup"><span data-stu-id="0699d-111">*Delegated Approver* – A delegated approver is one or multiple individuals or groups within your Azure AD who are responsible for approving requests for role activation.</span></span>

## <a name="scenarios"></a><span data-ttu-id="0699d-112">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="0699d-112">Scenarios</span></span>

<span data-ttu-id="0699d-113">A private Preview verziójára a következő szituációkat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="0699d-113">The private preview supports the following scenarios:</span></span>

<span data-ttu-id="0699d-114">**A kiemelt szerepkör rendszergazda (PRA), a következőket teheti:**</span><span class="sxs-lookup"><span data-stu-id="0699d-114">**As a Privileged Role Administrator (PRA) you can:**</span></span>

-   [<span data-ttu-id="0699d-115">egyes szerepkörök jóváhagyás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0699d-115">enable approval for specific roles</span></span>](#enable-approval-for-specific-roles)

-   [<span data-ttu-id="0699d-116">Adja meg a jóváhagyó felhasználók és/vagy a csoportok kérelmek jóváhagyása</span><span class="sxs-lookup"><span data-stu-id="0699d-116">specify approver users and/or groups to approve requests</span></span>](#specify-approver-users-and/or-groups-to-approve-requests)

-   [<span data-ttu-id="0699d-117">minden kiemelt szerepkört a kérelem és a jóváhagyási előzményeinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="0699d-117">view request and approval history for all privileged roles</span></span>](#view-request-and-approval-history-for-all-privileged-roles)

<span data-ttu-id="0699d-118">**Egy kijelölt jóváhagyó, mint a következő műveletek végezhetők el:**</span><span class="sxs-lookup"><span data-stu-id="0699d-118">**As a designated approver, you can:**</span></span>

-   [<span data-ttu-id="0699d-119">függőben lévő jóváhagyások (kérelmek) megtekintése</span><span class="sxs-lookup"><span data-stu-id="0699d-119">view pending approvals (requests)</span></span>](#view-pending-approvals-requests)

-   [<span data-ttu-id="0699d-120">jóváhagyhatja vagy elutasíthatja az (egyetlen és tömeges) szerepkör jogosultságszint-emelési kérések</span><span class="sxs-lookup"><span data-stu-id="0699d-120">approve or reject requests for role elevation (single and/or bulk)</span></span>](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [<span data-ttu-id="0699d-121">Adja meg a jóváhagyási elutasítási indokát</span><span class="sxs-lookup"><span data-stu-id="0699d-121">provide justification for my approval/rejection</span></span>](#provide-justification-for-my-approval/rejection) 

<span data-ttu-id="0699d-122">**Jogosult szerepkör felhasználóként a következő műveletek végezhetők el:**</span><span class="sxs-lookup"><span data-stu-id="0699d-122">**As an Eligible Role User you can:**</span></span>

-   [<span data-ttu-id="0699d-123">a jóváhagyást igénylő szerepkört aktiválási kérelmeinek megadása</span><span class="sxs-lookup"><span data-stu-id="0699d-123">request activation of a role that requires approval</span></span>](#request-activation-of-a-role-that-requires-approval)

-   [<span data-ttu-id="0699d-124">aktiválja a kérelem állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="0699d-124">view the status of your request to activate</span></span>](#view-the-status-of-your-request-to-activate)

-   [<span data-ttu-id="0699d-125">a feladat befejezése az Azure AD, ha az aktiválás jóváhagyva</span><span class="sxs-lookup"><span data-stu-id="0699d-125">complete your task in Azure AD if activation was approved</span></span>](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a><span data-ttu-id="0699d-126">Navigációs</span><span class="sxs-lookup"><span data-stu-id="0699d-126">Navigation</span></span>

<span data-ttu-id="0699d-127">Frissítettük a navigációs jóváhagyások támogatásához</span><span class="sxs-lookup"><span data-stu-id="0699d-127">We've updated the navigation to support approvals</span></span>

![](media/azure-ad-pim-approval-workflow/image001.png)

<span data-ttu-id="0699d-128">Az alapértelmezett kezdőlapján PIM és az új jóváhagyások dokumentációt kényelmes hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="0699d-128">The default landing page provides convenient access to information about PIM and the new approvals documentation.</span></span>

![](media/azure-ad-pim-approval-workflow/image002.png)

<span data-ttu-id="0699d-129">Azt is hozzáadott új szakasz PIM, "A naplózási előzmények" minden felhasználó részére.</span><span class="sxs-lookup"><span data-stu-id="0699d-129">We’ve also added a new section for all users of PIM, ‘My Audit History’.</span></span> <span data-ttu-id="0699d-130">Itt található összes adatot személyazonosságát kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="0699d-130">Here you can find all the information relevant to your identity.</span></span> <span data-ttu-id="0699d-131">Ez magában foglalja a függőben lévő és befejezett kérések, bármely végrehajtott megoldása a kérelmekkel kapcsolatos döntések, és a múltbeli szerepkör aktiválások egy tetszés szerinti helyre.</span><span class="sxs-lookup"><span data-stu-id="0699d-131">This includes all your pending and completed requests, any decisions you’ve made about the requests you resolve, and all your past role activations in one convenient location.</span></span>

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a><span data-ttu-id="0699d-132">Egyes szerepkörök jóváhagyás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0699d-132">Enable approval for specific roles</span></span>

<span data-ttu-id="0699d-133">Ahhoz, hogy az adott szerepkörhöz jóváhagyási, először válassza Directory szerepkörök a bal oldali navigációs sávon.</span><span class="sxs-lookup"><span data-stu-id="0699d-133">To enable approval for a specific role, first select Directory Roles from the left navigation.</span></span>

![](media/azure-ad-pim-approval-workflow/image004.png)

<span data-ttu-id="0699d-134">Keresse meg és állítsa be a Directory szerepkörök bal oldali navigációs</span><span class="sxs-lookup"><span data-stu-id="0699d-134">Find and select settings in the Directory Roles left navigation</span></span>

![](media/azure-ad-pim-approval-workflow/image006.png)

<span data-ttu-id="0699d-135">Válassza ki a kiemelt szerepköröket:</span><span class="sxs-lookup"><span data-stu-id="0699d-135">Select privileged Roles:</span></span>

![](media/azure-ad-pim-approval-workflow/image009.png)

<span data-ttu-id="0699d-136">Válassza ki az "Engedélyezés" a a jóváhagyási szakasz megkövetelése:</span><span class="sxs-lookup"><span data-stu-id="0699d-136">Select “Enable” in the Require approval section:</span></span>

![](media/azure-ad-pim-approval-workflow/image011.png)

<span data-ttu-id="0699d-137">Az engedélyezés után a panel a következő részleteket bővített:</span><span class="sxs-lookup"><span data-stu-id="0699d-137">Once enabled, the blade will expand to show the following details:</span></span>

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
<span data-ttu-id="0699d-138">Ha nem ad meg semmilyen jóváhagyóknak, a PRA(s) vált alapértelmezett leveleket a jóváhagyóknak.</span><span class="sxs-lookup"><span data-stu-id="0699d-138">If you DO NOT specify any approvers, the PRA(s) become the default approver(s).</span></span> <span data-ttu-id="0699d-139">A szerepkör összes aktiválási kéréseket jóváhagyni PRA(s) lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="0699d-139">PRA(s) would be required to approve ALL activation requests for this role.</span></span>

### <a name="specify-approver-users-andor-groups-to-approve-requests"></a><span data-ttu-id="0699d-140">Adja meg a jóváhagyó felhasználók és/vagy a csoportok kérelmek jóváhagyása</span><span class="sxs-lookup"><span data-stu-id="0699d-140">Specify approver users and/or groups to approve requests</span></span>

<span data-ttu-id="0699d-141">Jóváhagyási delegálása, kattintson a "Select jóváhagyóknak" lehetőség:</span><span class="sxs-lookup"><span data-stu-id="0699d-141">To delegate approval, click the option to “Select approvers”:</span></span>

![](media/azure-ad-pim-approval-workflow/image015.png)

<span data-ttu-id="0699d-142">A Select jóváhagyóknak panel betöltésekor, előfordulhat, hogy keresse meg egy adott felhasználó vagy csoport használja a keresési sávon a lap tetején, vagy az előre megadott listából válassza, majd kattintson a "Select" befejezésekor:</span><span class="sxs-lookup"><span data-stu-id="0699d-142">When the Select approvers blade loads, you may search for a specific user or group using the search bar at the top, or selecting from the pre-populated list, then click “Select” when finished:</span></span>

![](media/azure-ad-pim-approval-workflow/image017.png)

<span data-ttu-id="0699d-143">Megjegyzés: Előfordulhat, hogy választania több felhasználó vagy csoport egyszerre.</span><span class="sxs-lookup"><span data-stu-id="0699d-143">Note: You may select multiple users or groups at a time.</span></span>

<span data-ttu-id="0699d-144">A beállítás a kijelölt jóváhagyóknak listájában jelenik meg az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="0699d-144">Your selection will appear in the list of selected approvers as seen below:</span></span>

![](media/azure-ad-pim-approval-workflow/image019.png)

<span data-ttu-id="0699d-145">Jóváhagyó eltávolításához egyszerűen kattintson az Eltávolítás gombra a neve mellett.</span><span class="sxs-lookup"><span data-stu-id="0699d-145">To remove an approver, simply click the Remove button next to their name.</span></span>

<span data-ttu-id="0699d-146">További jóváhagyóknak hozzáadásához ismételje meg a műveletet.</span><span class="sxs-lookup"><span data-stu-id="0699d-146">To add additional approvers, repeat the process.</span></span>

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a><span data-ttu-id="0699d-147">Minden kiemelt szerepkört a kérelem és a jóváhagyási előzményeinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="0699d-147">View request and approval history for all privileged roles</span></span>

<span data-ttu-id="0699d-148">Minden kiemelt szerepkört kérelem és a jóváhagyási előzményeinek megtekintéséhez jelölje ki a naplózási előzmények az irányítópultról:</span><span class="sxs-lookup"><span data-stu-id="0699d-148">To view request and approval history for all privileged roles, select Audit History from the dashboard:</span></span>

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
<span data-ttu-id="0699d-149">Rendezze az adatokat a művelet, és keressen a "Jóváhagyott aktiválási"</span><span class="sxs-lookup"><span data-stu-id="0699d-149">You can sort the data by Action, and look for “Activation Approved”</span></span>

### <a name="view-pending-approvals-requests"></a><span data-ttu-id="0699d-150">Függőben lévő jóváhagyások (kérelmek) megtekintése</span><span class="sxs-lookup"><span data-stu-id="0699d-150">View pending approvals (requests)</span></span>

<span data-ttu-id="0699d-151">Mint egy delegált jóváhagyó kap értesítő e-mailek jóváhagyásra váró kérelem esetén.</span><span class="sxs-lookup"><span data-stu-id="0699d-151">As a delegated approver, you’ll receive email notifications when a request is pending your approval.</span></span> <span data-ttu-id="0699d-152">Ezeket a kéréseket a PIM portálon megtekintéséhez az irányítópult (az új navigációs) jelölje ki a bal oldali navigációs sávon a "Függőben lévő jóváhagyási kérelmek" lapon.</span><span class="sxs-lookup"><span data-stu-id="0699d-152">To view these requests in the PIM portal, from the dashboard (in the new navigation) select the “Pending Approval Requests” tab in the left navigation bar.</span></span>

![](media/azure-ad-pim-approval-workflow/image023.png)

<span data-ttu-id="0699d-153">Ott függőben lévő jóváhagyási kérelmek listáját láthatja:</span><span class="sxs-lookup"><span data-stu-id="0699d-153">From there, you’ll see a list of requests pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a><span data-ttu-id="0699d-154">Jóváhagyhatja vagy elutasíthatja az (egyetlen és tömeges) szerepkör jogosultságszint-emelési kérések</span><span class="sxs-lookup"><span data-stu-id="0699d-154">Approve or reject requests for role elevation (single and/or bulk)</span></span>

<span data-ttu-id="0699d-155">Válassza ki a kívánt jóváhagyásához vagy elutasításához kérelmek, és kattintson a gombra a megfelelő döntés műveletsávon:</span><span class="sxs-lookup"><span data-stu-id="0699d-155">Select the requests you wish to approve or deny, and click the button in the action bar that corresponds with your decision:</span></span>

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a><span data-ttu-id="0699d-156">Adja meg a jóváhagyási elutasítási indokát</span><span class="sxs-lookup"><span data-stu-id="0699d-156">Provide justification for my approval/rejection</span></span>

<span data-ttu-id="0699d-157">Ekkor megnyílik egy új panel jóváhagyásához vagy elutasításához egyszerre több kérést.</span><span class="sxs-lookup"><span data-stu-id="0699d-157">This will open a new blade to approve or deny multiple requests at once.</span></span> <span data-ttu-id="0699d-158">Indoklásának beírásához ad helyet a döntést, és kattintson jóváhagyása (vagy megtagadása) alsó vagy a panel:</span><span class="sxs-lookup"><span data-stu-id="0699d-158">Enter a justification for your decision, and click approve (or deny) at the bottom or the blade:</span></span>

![](media/azure-ad-pim-approval-workflow/image029.png)

<span data-ttu-id="0699d-159">Ha a kérelem befejeződött, az állapotjelzőben hozott döntés fogja tartalmazni (ebben a példában a döntést az jóváhagyása):</span><span class="sxs-lookup"><span data-stu-id="0699d-159">When the request process is complete, the status symbol will reflect the decision you made (in this example, the decision is approve):</span></span>

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a><span data-ttu-id="0699d-160">A jóváhagyást igénylő szerepkört aktiválási kérelmeinek megadása</span><span class="sxs-lookup"><span data-stu-id="0699d-160">Request activation of a role that requires approval</span></span>

<span data-ttu-id="0699d-161">A jóváhagyást igénylő szerepkört aktiválást kezdeményezhet a régi PIM navigációs, vagy az új navigációs, szerepkör-aktiválási folyamat változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="0699d-161">Requesting activation of a role that requires approval may be initiated from either the old PIM navigation, or the new navigation, as the process for role activation remains the same.</span></span> <span data-ttu-id="0699d-162">Egyszerűen jelölje ki a szerepkör aktiválása szerepkörök közül:</span><span class="sxs-lookup"><span data-stu-id="0699d-162">Simply select a role from the list of roles to activate:</span></span>

![](media/azure-ad-pim-approval-workflow/image033.png)

<span data-ttu-id="0699d-163">Ha egy kiemelt szerepkörhöz többtényezős hitelesítést igényel, kéri, hogy a feladat végrehajtásához először:</span><span class="sxs-lookup"><span data-stu-id="0699d-163">If a privileged role requires Multi-Factor Authentication, you’ll be prompted to complete that task first:</span></span>

![](media/azure-ad-pim-approval-workflow/image035.png)

<span data-ttu-id="0699d-164">A befejezést, aktiválás és a adja meg a indoklást (ha szükséges):</span><span class="sxs-lookup"><span data-stu-id="0699d-164">Once complete, click Activate and provide a justification (if required):</span></span>

![](media/azure-ad-pim-approval-workflow/image037.png)

<span data-ttu-id="0699d-165">A kérelmező megjelenik egy értesítés, amelyben a kérés jóváhagyására vár:</span><span class="sxs-lookup"><span data-stu-id="0699d-165">The requestor will see a notification that the request is pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-the-status-of-your-request-to-activate"></a><span data-ttu-id="0699d-166">Aktiválja a kérelem állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="0699d-166">View the status of your request to activate</span></span>

<span data-ttu-id="0699d-167">Aktiválja a függőben lévő kérelmek állapotának megtekintése az új navigációs sávon kell elérni.</span><span class="sxs-lookup"><span data-stu-id="0699d-167">Viewing the status of a pending request to activate must be accessed from the new navigation.</span></span> <span data-ttu-id="0699d-168">A bal oldali navigációs sávon válassza ki a "Saját kérések" lapon:</span><span class="sxs-lookup"><span data-stu-id="0699d-168">From the left navigation bar, select the “My Requests” tab:</span></span>

![](media/azure-ad-pim-approval-workflow/image041.png)

<span data-ttu-id="0699d-169">A kérelem állapotának alapértelmezett értéke: "függő"állapotba, de láthatja az összes visszaváltható vagy elutasított kérelmek.</span><span class="sxs-lookup"><span data-stu-id="0699d-169">The request state defaults to “Pending”, but you can toggle to see all or denied requests.</span></span>

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a><span data-ttu-id="0699d-170">A feladat befejezése az Azure AD, ha az aktiválás jóváhagyva</span><span class="sxs-lookup"><span data-stu-id="0699d-170">Complete your task in Azure AD if activation was approved</span></span>

<span data-ttu-id="0699d-171">Ha elfogadja a kérelmet, a szerepkör aktív, és folytathatja a munkát az ehhez a szerepkörhöz szükséges munka.</span><span class="sxs-lookup"><span data-stu-id="0699d-171">Once the request is approved, the role is active and you may proceed with any work that requires this role.</span></span>

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a><span data-ttu-id="0699d-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0699d-172">Next steps</span></span>

<span data-ttu-id="0699d-173">Visszajelzése fontos számunkra.</span><span class="sxs-lookup"><span data-stu-id="0699d-173">Your feedback is valuable to us.</span></span> <span data-ttu-id="0699d-174">Küldje el nyugodtan megosztása megjegyzések vagy visszajelzést szeretne küldeni nekünk Itt!</span><span class="sxs-lookup"><span data-stu-id="0699d-174">Please feel free to share comments or feedback with us here!</span></span>
