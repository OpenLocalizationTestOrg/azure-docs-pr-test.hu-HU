---
title: "aaaAzure Privileged Identity Management jóváhagyási munkafolyamatok |} Microsoft Docs"
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
ms.openlocfilehash: 4afaf5c138798a803eb3d3b7905b9361d65792cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="approvals-preview"></a><span data-ttu-id="e4a80-103">A jóváhagyások (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="e4a80-103">Approvals (Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="e4a80-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e4a80-104">Overview</span></span>

<span data-ttu-id="e4a80-105">Privileged Identity Management-jóváhagyásokkal rendelkező szerepkörök toorequire jóváhagyási aktiválás konfigurálása, és válasszon egy vagy több felhasználót vagy csoportot, meghatalmazott jóváhagyóknak.</span><span class="sxs-lookup"><span data-stu-id="e4a80-105">With Approvals for Privileged Identity Management, you can configure roles toorequire approval for activation, and choose one or multiple users or groups as delegated approvers.</span></span> <span data-ttu-id="e4a80-106">Tartsa hogyan olvasása toolearn tooconfigure szerepköröket, és válassza ki a jóváhagyóknak.</span><span class="sxs-lookup"><span data-stu-id="e4a80-106">Keep reading toolearn how tooconfigure roles and select approvers.</span></span>

>[!NOTE]
<span data-ttu-id="e4a80-107">Ne feledje, a szolgáltatás fejlesztés alatt van, és hibákat tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="e4a80-107">Please keep in mind this feature is still in development, and you may encounter bugs.</span></span> <span data-ttu-id="e4a80-108">hello funkció leírásával elnevezési konvenciói változhat, és nem tekinthető végső.</span><span class="sxs-lookup"><span data-stu-id="e4a80-108">hello functionality, including text and naming conventions are subject to change, and should not be considered final.</span></span>


## <a name="key-terminology"></a><span data-ttu-id="e4a80-109">Kulcs terminológia</span><span class="sxs-lookup"><span data-stu-id="e4a80-109">Key Terminology</span></span>

<span data-ttu-id="e4a80-110">*Jogosult szerepkör felhasználói* – az jogosult szerepkör felhasználó tulajdonképpen a szervezeten belül, amely jogosult az Azure AD tooan szerepkört rendelték a felhasználó (szerepkör aktiválást igényel).</span><span class="sxs-lookup"><span data-stu-id="e4a80-110">*Eligible Role User* – An eligible role user is a user within your organization that’s been assigned tooan Azure AD role as eligible (role requires activation).</span></span>

<span data-ttu-id="e4a80-111">*Meghatalmazott jóváhagyó* – egy delegált jóváhagyó egy vagy több egyéni felhasználók számára, vagy az Azure AD a csoportokat a szerepkörök aktiválásához kérelmek jóváhagyása felelős.</span><span class="sxs-lookup"><span data-stu-id="e4a80-111">*Delegated Approver* – A delegated approver is one or multiple individuals or groups within your Azure AD who are responsible for approving requests for role activation.</span></span>

## <a name="scenarios"></a><span data-ttu-id="e4a80-112">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="e4a80-112">Scenarios</span></span>

<span data-ttu-id="e4a80-113">hello private Preview verziójára hello a következő forgatókönyveket támogatja:</span><span class="sxs-lookup"><span data-stu-id="e4a80-113">hello private preview supports hello following scenarios:</span></span>

<span data-ttu-id="e4a80-114">**A kiemelt szerepkör rendszergazda (PRA), a következőket teheti:**</span><span class="sxs-lookup"><span data-stu-id="e4a80-114">**As a Privileged Role Administrator (PRA) you can:**</span></span>

-   [<span data-ttu-id="e4a80-115">egyes szerepkörök jóváhagyás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e4a80-115">enable approval for specific roles</span></span>](#enable-approval-for-specific-roles)

-   [<span data-ttu-id="e4a80-116">Adja meg a felhasználók és/vagy csoportok jóváhagyó tooapprove kérelmek</span><span class="sxs-lookup"><span data-stu-id="e4a80-116">specify approver users and/or groups tooapprove requests</span></span>](#specify-approver-users-and/or-groups-to-approve-requests)

-   [<span data-ttu-id="e4a80-117">minden kiemelt szerepkört a kérelem és a jóváhagyási előzményeinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="e4a80-117">view request and approval history for all privileged roles</span></span>](#view-request-and-approval-history-for-all-privileged-roles)

<span data-ttu-id="e4a80-118">**Egy kijelölt jóváhagyó, mint a következő műveletek végezhetők el:**</span><span class="sxs-lookup"><span data-stu-id="e4a80-118">**As a designated approver, you can:**</span></span>

-   [<span data-ttu-id="e4a80-119">függőben lévő jóváhagyások (kérelmek) megtekintése</span><span class="sxs-lookup"><span data-stu-id="e4a80-119">view pending approvals (requests)</span></span>](#view-pending-approvals-requests)

-   [<span data-ttu-id="e4a80-120">jóváhagyhatja vagy elutasíthatja az (egyetlen és tömeges) szerepkör jogosultságszint-emelési kérések</span><span class="sxs-lookup"><span data-stu-id="e4a80-120">approve or reject requests for role elevation (single and/or bulk)</span></span>](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [<span data-ttu-id="e4a80-121">Adja meg a jóváhagyási elutasítási indokát</span><span class="sxs-lookup"><span data-stu-id="e4a80-121">provide justification for my approval/rejection</span></span>](#provide-justification-for-my-approval/rejection) 

<span data-ttu-id="e4a80-122">**Jogosult szerepkör felhasználóként a következő műveletek végezhetők el:**</span><span class="sxs-lookup"><span data-stu-id="e4a80-122">**As an Eligible Role User you can:**</span></span>

-   [<span data-ttu-id="e4a80-123">a jóváhagyást igénylő szerepkört aktiválási kérelmeinek megadása</span><span class="sxs-lookup"><span data-stu-id="e4a80-123">request activation of a role that requires approval</span></span>](#request-activation-of-a-role-that-requires-approval)

-   [<span data-ttu-id="e4a80-124">a kérelem tooactivate hello állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="e4a80-124">view hello status of your request tooactivate</span></span>](#view-the-status-of-your-request-to-activate)

-   [<span data-ttu-id="e4a80-125">a feladat befejezése az Azure AD, ha az aktiválás jóváhagyva</span><span class="sxs-lookup"><span data-stu-id="e4a80-125">complete your task in Azure AD if activation was approved</span></span>](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a><span data-ttu-id="e4a80-126">Navigációs</span><span class="sxs-lookup"><span data-stu-id="e4a80-126">Navigation</span></span>

<span data-ttu-id="e4a80-127">Frissítettük hello navigációs toosupport jóváhagyásokat</span><span class="sxs-lookup"><span data-stu-id="e4a80-127">We've updated hello navigation toosupport approvals</span></span>

![](media/azure-ad-pim-approval-workflow/image001.png)

<span data-ttu-id="e4a80-128">hello alapértelmezett kezdőlapja kényelmes hozzáférési tooinformation PIM és hello új jóváhagyások dokumentációhoz biztosít.</span><span class="sxs-lookup"><span data-stu-id="e4a80-128">hello default landing page provides convenient access tooinformation about PIM and hello new approvals documentation.</span></span>

![](media/azure-ad-pim-approval-workflow/image002.png)

<span data-ttu-id="e4a80-129">Azt is hozzáadott új szakasz PIM, "A naplózási előzmények" minden felhasználó részére.</span><span class="sxs-lookup"><span data-stu-id="e4a80-129">We’ve also added a new section for all users of PIM, ‘My Audit History’.</span></span> <span data-ttu-id="e4a80-130">Itt található összes hello információk vonatkozó tooyour identitás.</span><span class="sxs-lookup"><span data-stu-id="e4a80-130">Here you can find all hello information relevant tooyour identity.</span></span> <span data-ttu-id="e4a80-131">Ez magában foglalja a függőben lévő és befejezett kérések, bármely végrehajtott megoldása hello kérelmekkel kapcsolatos döntések, és a múltbeli szerepkör aktiválások egy tetszés szerinti helyre.</span><span class="sxs-lookup"><span data-stu-id="e4a80-131">This includes all your pending and completed requests, any decisions you’ve made about hello requests you resolve, and all your past role activations in one convenient location.</span></span>

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a><span data-ttu-id="e4a80-132">Egyes szerepkörök jóváhagyás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e4a80-132">Enable approval for specific roles</span></span>

<span data-ttu-id="e4a80-133">egy adott szerepkör tooenable jóváhagyás Directory szerepkörök először kiválasztása hello bal oldali navigációs sávon.</span><span class="sxs-lookup"><span data-stu-id="e4a80-133">tooenable approval for a specific role, first select Directory Roles from hello left navigation.</span></span>

![](media/azure-ad-pim-approval-workflow/image004.png)

<span data-ttu-id="e4a80-134">Található, és válassza a beállítások a bal oldali navigációs Directory szerepkörök hello</span><span class="sxs-lookup"><span data-stu-id="e4a80-134">Find and select settings in hello Directory Roles left navigation</span></span>

![](media/azure-ad-pim-approval-workflow/image006.png)

<span data-ttu-id="e4a80-135">Válassza ki a kiemelt szerepköröket:</span><span class="sxs-lookup"><span data-stu-id="e4a80-135">Select privileged Roles:</span></span>

![](media/azure-ad-pim-approval-workflow/image009.png)

<span data-ttu-id="e4a80-136">Válassza ki az "Engedélyezés" hello jóváhagyási szakasz megkövetelése:</span><span class="sxs-lookup"><span data-stu-id="e4a80-136">Select “Enable” in hello Require approval section:</span></span>

![](media/azure-ad-pim-approval-workflow/image011.png)

<span data-ttu-id="e4a80-137">Engedélyezve van, a hello panelen lesz bontsa ki a következő adatok tooshow hello:</span><span class="sxs-lookup"><span data-stu-id="e4a80-137">Once enabled, hello blade will expand tooshow hello following details:</span></span>

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
<span data-ttu-id="e4a80-138">Ha nem ad meg semmilyen jóváhagyóknak, a hello PRA(s) hello alapértelmezett approver(s) válik.</span><span class="sxs-lookup"><span data-stu-id="e4a80-138">If you DO NOT specify any approvers, hello PRA(s) become hello default approver(s).</span></span> <span data-ttu-id="e4a80-139">PRA(s) lenne szükséges tooapprove összes aktiválási kérelmek ehhez a szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="e4a80-139">PRA(s) would be required tooapprove ALL activation requests for this role.</span></span>

### <a name="specify-approver-users-andor-groups-tooapprove-requests"></a><span data-ttu-id="e4a80-140">Adja meg a felhasználók és/vagy csoportok jóváhagyó tooapprove kérelmek</span><span class="sxs-lookup"><span data-stu-id="e4a80-140">Specify approver users and/or groups tooapprove requests</span></span>

<span data-ttu-id="e4a80-141">toodelegate jóváhagyási hello választógomb túl "kiválasztása jóváhagyóknak":</span><span class="sxs-lookup"><span data-stu-id="e4a80-141">toodelegate approval, click hello option too“Select approvers”:</span></span>

![](media/azure-ad-pim-approval-workflow/image015.png)

<span data-ttu-id="e4a80-142">Hello válassza jóváhagyóknak panel betöltésekor, előfordulhat, hogy keresse meg olyan felhasználót vagy csoportot a hello keresősáv hello tetejéhez vagy hello előre megadott listából válassza, majd kattintson a "Select" befejezésekor:</span><span class="sxs-lookup"><span data-stu-id="e4a80-142">When hello Select approvers blade loads, you may search for a specific user or group using hello search bar at hello top, or selecting from hello pre-populated list, then click “Select” when finished:</span></span>

![](media/azure-ad-pim-approval-workflow/image017.png)

<span data-ttu-id="e4a80-143">Megjegyzés: Előfordulhat, hogy választania több felhasználó vagy csoport egyszerre.</span><span class="sxs-lookup"><span data-stu-id="e4a80-143">Note: You may select multiple users or groups at a time.</span></span>

<span data-ttu-id="e4a80-144">A kiválasztott kijelölt jóváhagyóknak hello listájában jelenik meg az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="e4a80-144">Your selection will appear in hello list of selected approvers as seen below:</span></span>

![](media/azure-ad-pim-approval-workflow/image019.png)

<span data-ttu-id="e4a80-145">tooremove jóváhagyó, egyszerűen kattintson a hello Eltávolítás gomb következő tootheir neve.</span><span class="sxs-lookup"><span data-stu-id="e4a80-145">tooremove an approver, simply click hello Remove button next tootheir name.</span></span>

<span data-ttu-id="e4a80-146">További jóváhagyóknak tooadd, ismétlődő hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="e4a80-146">tooadd additional approvers, repeat hello process.</span></span>

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a><span data-ttu-id="e4a80-147">Minden kiemelt szerepkört a kérelem és a jóváhagyási előzményeinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="e4a80-147">View request and approval history for all privileged roles</span></span>

<span data-ttu-id="e4a80-148">minden kiemelt szerepkört, a kérelem és a jóváhagyási előzmények tooview naplózási előzmények hello irányítópult közül választhat:</span><span class="sxs-lookup"><span data-stu-id="e4a80-148">tooview request and approval history for all privileged roles, select Audit History from hello dashboard:</span></span>

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
<span data-ttu-id="e4a80-149">Rendezze művelet által hello adatokat, és keressen a "Aktiválási jóváhagyott"</span><span class="sxs-lookup"><span data-stu-id="e4a80-149">You can sort hello data by Action, and look for “Activation Approved”</span></span>

### <a name="view-pending-approvals-requests"></a><span data-ttu-id="e4a80-150">Függőben lévő jóváhagyások (kérelmek) megtekintése</span><span class="sxs-lookup"><span data-stu-id="e4a80-150">View pending approvals (requests)</span></span>

<span data-ttu-id="e4a80-151">Mint egy delegált jóváhagyó kap értesítő e-mailek jóváhagyásra váró kérelem esetén.</span><span class="sxs-lookup"><span data-stu-id="e4a80-151">As a delegated approver, you’ll receive email notifications when a request is pending your approval.</span></span> <span data-ttu-id="e4a80-152">tooview ezek a kérelmek hello PIM portálon, az irányítópult (a hello új navigációs) válassza hello "függőben lévő jóváhagyási kérelmek" hello lapjának bal oldali navigációs sáv.</span><span class="sxs-lookup"><span data-stu-id="e4a80-152">tooview these requests in hello PIM portal, from the dashboard (in hello new navigation) select hello “Pending Approval Requests” tab in hello left navigation bar.</span></span>

![](media/azure-ad-pim-approval-workflow/image023.png)

<span data-ttu-id="e4a80-153">Ott függőben lévő jóváhagyási kérelmek listáját láthatja:</span><span class="sxs-lookup"><span data-stu-id="e4a80-153">From there, you’ll see a list of requests pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a><span data-ttu-id="e4a80-154">Jóváhagyhatja vagy elutasíthatja az (egyetlen és tömeges) szerepkör jogosultságszint-emelési kérések</span><span class="sxs-lookup"><span data-stu-id="e4a80-154">Approve or reject requests for role elevation (single and/or bulk)</span></span>

<span data-ttu-id="e4a80-155">Hello kérések tooapprove kívánja vagy megtagadják kiválasztása, majd kattintson a művelet található, amely megfelel a döntés hello gombra:</span><span class="sxs-lookup"><span data-stu-id="e4a80-155">Select hello requests you wish tooapprove or deny, and click hello button in the action bar that corresponds with your decision:</span></span>

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a><span data-ttu-id="e4a80-156">Adja meg a jóváhagyási elutasítási indokát</span><span class="sxs-lookup"><span data-stu-id="e4a80-156">Provide justification for my approval/rejection</span></span>

<span data-ttu-id="e4a80-157">Ezzel nyisson meg egy új panel tooapprove vagy elutasítása egyszerre több kérést.</span><span class="sxs-lookup"><span data-stu-id="e4a80-157">This will open a new blade tooapprove or deny multiple requests at once.</span></span> <span data-ttu-id="e4a80-158">Indoklásának beírásához ad helyet a döntést, és kattintson jóváhagyása (vagy megtagadása) hello alsó vagy hello panel:</span><span class="sxs-lookup"><span data-stu-id="e4a80-158">Enter a justification for your decision, and click approve (or deny) at hello bottom or hello blade:</span></span>

![](media/azure-ad-pim-approval-workflow/image029.png)

<span data-ttu-id="e4a80-159">Ha hello lekérő folyamat befejeződött, a hello állapotjelzőben fogja tartalmazni hozott döntés (ebben a példában hello döntési jóváhagyása):</span><span class="sxs-lookup"><span data-stu-id="e4a80-159">When hello request process is complete, hello status symbol will reflect the decision you made (in this example, hello decision is approve):</span></span>

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a><span data-ttu-id="e4a80-160">A jóváhagyást igénylő szerepkört aktiválási kérelmeinek megadása</span><span class="sxs-lookup"><span data-stu-id="e4a80-160">Request activation of a role that requires approval</span></span>

<span data-ttu-id="e4a80-161">A kért aktiválásának hello régi PIM navigációs, vagy az új navigációs hello kezdeményezhet jóváhagyást igénylő szerepkört, a szerepkör aktiválása marad hello folyamatként hello azonos.</span><span class="sxs-lookup"><span data-stu-id="e4a80-161">Requesting activation of a role that requires approval may be initiated from either hello old PIM navigation, or hello new navigation, as hello process for role activation remains hello same.</span></span> <span data-ttu-id="e4a80-162">Egyszerűen válassza ki a szerepkör a hello szerepkörök aktiválása:</span><span class="sxs-lookup"><span data-stu-id="e4a80-162">Simply select a role from hello list of roles to activate:</span></span>

![](media/azure-ad-pim-approval-workflow/image033.png)

<span data-ttu-id="e4a80-163">Ha egy kiemelt szerepkörhöz többtényezős hitelesítést igényel, kéri, hogy a feladat végrehajtásához először:</span><span class="sxs-lookup"><span data-stu-id="e4a80-163">If a privileged role requires Multi-Factor Authentication, you’ll be prompted to complete that task first:</span></span>

![](media/azure-ad-pim-approval-workflow/image035.png)

<span data-ttu-id="e4a80-164">A befejezést, aktiválás és a adja meg a indoklást (ha szükséges):</span><span class="sxs-lookup"><span data-stu-id="e4a80-164">Once complete, click Activate and provide a justification (if required):</span></span>

![](media/azure-ad-pim-approval-workflow/image037.png)

<span data-ttu-id="e4a80-165">hello kérelmező megjelenik egy értesítés, hogy a kérelem hello jóváhagyásra van:</span><span class="sxs-lookup"><span data-stu-id="e4a80-165">hello requestor will see a notification that hello request is pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-hello-status-of-your-request-tooactivate"></a><span data-ttu-id="e4a80-166">A kérelem tooactivate hello állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="e4a80-166">View hello status of your request tooactivate</span></span>

<span data-ttu-id="e4a80-167">A függőben lévő kérelem tooactivate hello állapotának megtekintése az új navigációs sávon kell elérni.</span><span class="sxs-lookup"><span data-stu-id="e4a80-167">Viewing hello status of a pending request tooactivate must be accessed from the new navigation.</span></span> <span data-ttu-id="e4a80-168">Hello bal oldali navigációs sávon jelölje ki hello "Saját kérések" lapon:</span><span class="sxs-lookup"><span data-stu-id="e4a80-168">From hello left navigation bar, select hello “My Requests” tab:</span></span>

![](media/azure-ad-pim-approval-workflow/image041.png)

<span data-ttu-id="e4a80-169">hello lekérési állapota alapértelmezés szerint használt érték túl "Függőben", de minden toosee visszaváltható vagy az elutasított kérelmek.</span><span class="sxs-lookup"><span data-stu-id="e4a80-169">hello request state defaults too“Pending”, but you can toggle toosee all or denied requests.</span></span>

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a><span data-ttu-id="e4a80-170">A feladat befejezése az Azure AD, ha az aktiválás jóváhagyva</span><span class="sxs-lookup"><span data-stu-id="e4a80-170">Complete your task in Azure AD if activation was approved</span></span>

<span data-ttu-id="e4a80-171">Hello kérelmet jóváhagyja a rendszer, ha hello szerepkör aktív, és folytathatja a munkát az ehhez a szerepkörhöz szükséges munka.</span><span class="sxs-lookup"><span data-stu-id="e4a80-171">Once hello request is approved, hello role is active and you may proceed with any work that requires this role.</span></span>

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a><span data-ttu-id="e4a80-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4a80-172">Next steps</span></span>

<span data-ttu-id="e4a80-173">Visszajelzése értékes toous.</span><span class="sxs-lookup"><span data-stu-id="e4a80-173">Your feedback is valuable toous.</span></span> <span data-ttu-id="e4a80-174">Adjon érzi, hogy szabad tooshare megjegyzések vagy visszajelzést szeretne küldeni betartásának vizsgálatára Itt!</span><span class="sxs-lookup"><span data-stu-id="e4a80-174">Please feel free tooshare comments or feedback with us here!</span></span>
