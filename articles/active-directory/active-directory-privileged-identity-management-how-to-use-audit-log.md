---
title: "A napló használata az Azure AD Privileged Identity Management |} Microsoft Docs"
description: "Útmutató: az auditnapló használata az Azure Privileged Identity Management bővítmény."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 5d13a6dd-1fcb-4e76-82fb-cb2f4f0e4357
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 7d9a5255a64d46c1388d328a606b3f297d61262b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-audit-log-in-pim"></a><span data-ttu-id="6c953-103">A PIM a napló használatával</span><span class="sxs-lookup"><span data-stu-id="6c953-103">Using the audit log in PIM</span></span>
<span data-ttu-id="6c953-104">A Privileged Identity Management (PIM) napló segítségével tekintse meg a felhasználó-hozzárendeléseket és aktiválások egy adott időtartamon belül.</span><span class="sxs-lookup"><span data-stu-id="6c953-104">You can use the Privileged Identity Management (PIM) audit log to see all the user assignments and activations within a given time period.</span></span> <span data-ttu-id="6c953-105">Ha meg szeretné tekinteni a teljes naplózási előzmények a bérlői rendszergazda, a végfelhasználó és a szinkronizálási tevékenység, beleértve a tevékenység használhatja a [Azure Active Directory hozzáférési és használati jelentések.](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="6c953-105">If you want to see the full audit history of activity in your tenant, including administrator, end user, and synchronization activity, you can use the [Azure Active Directory access and usage reports.](active-directory-view-access-usage-reports.md)</span></span>

## <a name="navigate-to-the-audit-log"></a><span data-ttu-id="6c953-106">Keresse meg a napló</span><span class="sxs-lookup"><span data-stu-id="6c953-106">Navigate to the audit log</span></span>
<span data-ttu-id="6c953-107">Az a [Azure-portálon](https://portal.azure.com) irányítópultot, válassza ki a **Azure AD Privileged Identity Management** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6c953-107">From the [Azure portal](https://portal.azure.com) dashboard, select the **Azure AD Privileged Identity Management** app.</span></span> <span data-ttu-id="6c953-108">Ott, hozzáférni a biztonsági naplóba kattintva **kiemelt szerepköröket kezelése** > **naplózási előzmények** a PIM irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="6c953-108">From there, access the audit log by clicking **Manage privileged roles** > **Audit history** in the PIM dashboard.</span></span>

## <a name="the-audit-log-graph"></a><span data-ttu-id="6c953-109">Az ellenőrzési napló diagramhoz</span><span class="sxs-lookup"><span data-stu-id="6c953-109">The audit log graph</span></span>
<span data-ttu-id="6c953-110">A napló segítségével tekintse meg az összes aktiválás, napi maximális aktiválások és napi átlagos aktiválások vonaldiagramon.</span><span class="sxs-lookup"><span data-stu-id="6c953-110">You can use the audit log to view the total activations, max activations per day, and average activations per day in a line graph.</span></span>  <span data-ttu-id="6c953-111">Az adatok szerepkör szűrhető Ha több szerepkör van a naplózási előzmények.</span><span class="sxs-lookup"><span data-stu-id="6c953-111">You can also filter the data by role if there is more than one role in the audit history.</span></span>

<span data-ttu-id="6c953-112">Használja a **idő**, **művelet**, és **szerepkör** gombokra kattintva rendezheti a naplót.</span><span class="sxs-lookup"><span data-stu-id="6c953-112">Use the **time**, **action**, and **role** buttons to sort the log.</span></span>

## <a name="the-audit-log-list"></a><span data-ttu-id="6c953-113">A vizsgálati naplók listája</span><span class="sxs-lookup"><span data-stu-id="6c953-113">The audit log list</span></span>
<span data-ttu-id="6c953-114">A vizsgálati naplók listája oszlopai a következők:</span><span class="sxs-lookup"><span data-stu-id="6c953-114">The columns in the audit log list are:</span></span>

* <span data-ttu-id="6c953-115">**Kérelmező** – a felhasználóknak, akik a szerepkör aktiválása vagy módosítsa a kért.</span><span class="sxs-lookup"><span data-stu-id="6c953-115">**Requestor** - the user who requested the role activation or change.</span></span>  <span data-ttu-id="6c953-116">Ha az érték "Azure rendszer", ellenőrizze a Azure naplózási további információt.</span><span class="sxs-lookup"><span data-stu-id="6c953-116">If the value is "Azure System", check the Azure audit log for more information.</span></span>
* <span data-ttu-id="6c953-117">**Felhasználói** -aktiválásával vagy az adott szerepkörhöz hozzárendelt felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6c953-117">**User** - the user who is activating or assigned to a role.</span></span>
* <span data-ttu-id="6c953-118">**Szerepkör** -a szerepkörhöz hozzárendelt, vagy a felhasználó által aktivált.</span><span class="sxs-lookup"><span data-stu-id="6c953-118">**Role** - the role assigned or activated by the user.</span></span>
* <span data-ttu-id="6c953-119">**A művelet** – a kérelmező által végrehajtott műveleteket.</span><span class="sxs-lookup"><span data-stu-id="6c953-119">**Action** - the actions taken by the requestor.</span></span> <span data-ttu-id="6c953-120">Ez magában foglalhatja hozzárendelés, helyekhez, aktiválás vagy deaktiválás.</span><span class="sxs-lookup"><span data-stu-id="6c953-120">This can include assignment, unassignment, activation, or deactivation.</span></span>
* <span data-ttu-id="6c953-121">**Idő** – Ha a művelet történt.</span><span class="sxs-lookup"><span data-stu-id="6c953-121">**Time** - when the action occurred.</span></span>
* <span data-ttu-id="6c953-122">**Mintafelismerési** -szöveg az aktiválás során az az oka mező megadása esetén megjelenik itt.</span><span class="sxs-lookup"><span data-stu-id="6c953-122">**Reasoning** - if any text was entered into the reason field during activation, it will show up here.</span></span>
* <span data-ttu-id="6c953-123">**Lejárati** – csak akkor érvényes, a szerepkörök aktiválásához.</span><span class="sxs-lookup"><span data-stu-id="6c953-123">**Expiration** - only relevant for activation of roles.</span></span>

## <a name="filter-the-audit-log"></a><span data-ttu-id="6c953-124">A napló szűrése</span><span class="sxs-lookup"><span data-stu-id="6c953-124">Filter the audit log</span></span>
<span data-ttu-id="6c953-125">A gombra kattintva megjelenik a napló információkat szűrheti is a **szűrő** gombra.</span><span class="sxs-lookup"><span data-stu-id="6c953-125">You can filter the information that shows up in the audit log by clicking the **Filter** button.</span></span>  <span data-ttu-id="6c953-126">A **frissítés diagram (paraméterek) panelen** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6c953-126">The **Update chart parameters blade** will appear.</span></span>

<span data-ttu-id="6c953-127">Miután beállította a szűrőket, kattintson a **frissítés** a naplóban szereplő adatok szűrésére.</span><span class="sxs-lookup"><span data-stu-id="6c953-127">After you set the filters, click **Update** to filter the data in the log.</span></span>  <span data-ttu-id="6c953-128">Ha az adatok nem jelennek meg azonnal, frissítse az oldalt.</span><span class="sxs-lookup"><span data-stu-id="6c953-128">If the data doesn't appear right away, refresh the page.</span></span>

### <a name="change-the-date-range"></a><span data-ttu-id="6c953-129">Módosítás dátumtartományban</span><span class="sxs-lookup"><span data-stu-id="6c953-129">Change the date range</span></span>
<span data-ttu-id="6c953-130">Használja a **Ma**, **múlt héten**, **elmúlt hónap**, vagy **egyéni** gombokkal módosíthatja az időtartományt a biztonsági napló.</span><span class="sxs-lookup"><span data-stu-id="6c953-130">Use the **Today**, **Past Week**, **Past Month**, or **Custom** buttons to change the time range of the audit log.</span></span>

<span data-ttu-id="6c953-131">Ha úgy dönt, a **egyéni** gomb, Ön által megadott egy **a** dátummezője és egy **való** dátum mezőben adja meg a napló dátumtartomány.</span><span class="sxs-lookup"><span data-stu-id="6c953-131">When you choose the **Custom** button, you will be given a **From** date field and a **To** date field to specify a range of dates for the log.</span></span>  <span data-ttu-id="6c953-132">HH/nn/éééé formátumban adja meg a dátumok, vagy kattintson a a **naptár** ikonra, és válassza ki a dátum alapján.</span><span class="sxs-lookup"><span data-stu-id="6c953-132">You can either enter the dates in MM/DD/YYYY format or click on the **calendar** icon and choose the date from a calendar.</span></span>

### <a name="change-the-roles-included-in-the-log"></a><span data-ttu-id="6c953-133">A naplóban szereplő szerepkörök módosítása</span><span class="sxs-lookup"><span data-stu-id="6c953-133">Change the roles included in the log</span></span>
<span data-ttu-id="6c953-134">Ellenőrizze, vagy törölje a jelet a **szerepkör** vagy kizárja a a naplóból szerepkörönként jelölőnégyzetét.</span><span class="sxs-lookup"><span data-stu-id="6c953-134">Check or uncheck the **Role** checkbox next to each role to include or exclude it from the log.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="6c953-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c953-135">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

