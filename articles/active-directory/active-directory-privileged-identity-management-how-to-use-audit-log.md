---
title: "az Azure AD Privileged Identity Management aaaHow toouse hello audit napló |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello napló hello Azure Privileged Identity Management bővítmény."
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
ms.openlocfilehash: 36987eaab9fe02c5dd7b4f4705e487299430745d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-audit-log-in-pim"></a><span data-ttu-id="7f277-103">A PIM hello napló használatával</span><span class="sxs-lookup"><span data-stu-id="7f277-103">Using hello audit log in PIM</span></span>
<span data-ttu-id="7f277-104">Használhatja hello Privileged Identity Management (PIM) naplózási napló toosee összes hello felhasználó hozzárendelést és aktiválások egy adott időtartamon belül.</span><span class="sxs-lookup"><span data-stu-id="7f277-104">You can use hello Privileged Identity Management (PIM) audit log toosee all hello user assignments and activations within a given time period.</span></span> <span data-ttu-id="7f277-105">Ha azt szeretné, toosee hello teljes naplózási előzmények tevékenység a bérlői rendszergazda, a végfelhasználó és a szinkronizálási tevékenység, beleértve a hello használhatja [Azure Active Directory hozzáférési és használati jelentések.](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="7f277-105">If you want toosee hello full audit history of activity in your tenant, including administrator, end user, and synchronization activity, you can use hello [Azure Active Directory access and usage reports.](active-directory-view-access-usage-reports.md)</span></span>

## <a name="navigate-toohello-audit-log"></a><span data-ttu-id="7f277-106">Keresse meg a toohello audit napló</span><span class="sxs-lookup"><span data-stu-id="7f277-106">Navigate toohello audit log</span></span>
<span data-ttu-id="7f277-107">A hello [Azure-portálon](https://portal.azure.com) irányítópultot, válassza hello **Azure AD Privileged Identity Management** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7f277-107">From hello [Azure portal](https://portal.azure.com) dashboard, select hello **Azure AD Privileged Identity Management** app.</span></span> <span data-ttu-id="7f277-108">Ott, hozzáférési napló hello kattintva **kiemelt szerepköröket kezelése** > **naplózási előzmények** hello PIM irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="7f277-108">From there, access hello audit log by clicking **Manage privileged roles** > **Audit history** in hello PIM dashboard.</span></span>

## <a name="hello-audit-log-graph"></a><span data-ttu-id="7f277-109">hello naplózási napló diagramhoz</span><span class="sxs-lookup"><span data-stu-id="7f277-109">hello audit log graph</span></span>
<span data-ttu-id="7f277-110">Hello naplózási napló tooview hello összes aktiválás, napi maximális aktiválások és napi átlagos aktiválások vonaldiagramon használható.</span><span class="sxs-lookup"><span data-stu-id="7f277-110">You can use hello audit log tooview hello total activations, max activations per day, and average activations per day in a line graph.</span></span>  <span data-ttu-id="7f277-111">Hello adatok szerepkör szűrhető Ha több szerepkör hello naplózási előzmények szerepel.</span><span class="sxs-lookup"><span data-stu-id="7f277-111">You can also filter hello data by role if there is more than one role in hello audit history.</span></span>

<span data-ttu-id="7f277-112">Használjon hello **idő**, **művelet**, és **szerepkör** gombok toosort hello napló.</span><span class="sxs-lookup"><span data-stu-id="7f277-112">Use hello **time**, **action**, and **role** buttons toosort hello log.</span></span>

## <a name="hello-audit-log-list"></a><span data-ttu-id="7f277-113">hello naplózási naplók listája</span><span class="sxs-lookup"><span data-stu-id="7f277-113">hello audit log list</span></span>
<span data-ttu-id="7f277-114">a hello napló napló listájában hello oszlopok a következők:</span><span class="sxs-lookup"><span data-stu-id="7f277-114">hello columns in hello audit log list are:</span></span>

* <span data-ttu-id="7f277-115">**Kérelmező** -hello felhasználó, aki a kért hello szerepkör aktiválása vagy módosítása.</span><span class="sxs-lookup"><span data-stu-id="7f277-115">**Requestor** - hello user who requested hello role activation or change.</span></span>  <span data-ttu-id="7f277-116">Hello értéke "Azure rendszer", ha a naplóban hello Azure naplózási további információt.</span><span class="sxs-lookup"><span data-stu-id="7f277-116">If hello value is "Azure System", check hello Azure audit log for more information.</span></span>
* <span data-ttu-id="7f277-117">**Felhasználói** -hello felhasználó az aktiválás vagy tooa szerepkörrel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="7f277-117">**User** - hello user who is activating or assigned tooa role.</span></span>
* <span data-ttu-id="7f277-118">**Szerepkör** -hello szerepkör vagy felhasználó hello aktiválására.</span><span class="sxs-lookup"><span data-stu-id="7f277-118">**Role** - hello role assigned or activated by hello user.</span></span>
* <span data-ttu-id="7f277-119">**A művelet** – hello kérelmező hello műveleteit.</span><span class="sxs-lookup"><span data-stu-id="7f277-119">**Action** - hello actions taken by hello requestor.</span></span> <span data-ttu-id="7f277-120">Ez magában foglalhatja hozzárendelés, helyekhez, aktiválás vagy deaktiválás.</span><span class="sxs-lookup"><span data-stu-id="7f277-120">This can include assignment, unassignment, activation, or deactivation.</span></span>
* <span data-ttu-id="7f277-121">**Idő** – Ha hello művelet történt.</span><span class="sxs-lookup"><span data-stu-id="7f277-121">**Time** - when hello action occurred.</span></span>
* <span data-ttu-id="7f277-122">**Mintafelismerési** -szöveg hello OK mezőbe az aktiválás során megadása esetén megjelenik itt.</span><span class="sxs-lookup"><span data-stu-id="7f277-122">**Reasoning** - if any text was entered into hello reason field during activation, it will show up here.</span></span>
* <span data-ttu-id="7f277-123">**Lejárati** – csak akkor érvényes, a szerepkörök aktiválásához.</span><span class="sxs-lookup"><span data-stu-id="7f277-123">**Expiration** - only relevant for activation of roles.</span></span>

## <a name="filter-hello-audit-log"></a><span data-ttu-id="7f277-124">Szűrő hello audit napló</span><span class="sxs-lookup"><span data-stu-id="7f277-124">Filter hello audit log</span></span>
<span data-ttu-id="7f277-125">Hello információkat megjelenő hello naplóban hello kattintva szűrheti **szűrő** gombra.</span><span class="sxs-lookup"><span data-stu-id="7f277-125">You can filter hello information that shows up in hello audit log by clicking hello **Filter** button.</span></span>  <span data-ttu-id="7f277-126">Hello **frissítés diagram (paraméterek) panelen** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7f277-126">hello **Update chart parameters blade** will appear.</span></span>

<span data-ttu-id="7f277-127">Hello szűrők beállítását követően kattintson **frissítés** toofilter hello adatok hello naplóban.</span><span class="sxs-lookup"><span data-stu-id="7f277-127">After you set hello filters, click **Update** toofilter hello data in hello log.</span></span>  <span data-ttu-id="7f277-128">Ha hello adatok nem jelennek meg azonnal, frissítse a hello lapot.</span><span class="sxs-lookup"><span data-stu-id="7f277-128">If hello data doesn't appear right away, refresh hello page.</span></span>

### <a name="change-hello-date-range"></a><span data-ttu-id="7f277-129">Hello dátumtartomány módosítása</span><span class="sxs-lookup"><span data-stu-id="7f277-129">Change hello date range</span></span>
<span data-ttu-id="7f277-130">Használjon hello **Ma**, **múlt héten**, **elmúlt hónap**, vagy **egyéni** toochange hello időtartománya hello napló gomb.</span><span class="sxs-lookup"><span data-stu-id="7f277-130">Use hello **Today**, **Past Week**, **Past Month**, or **Custom** buttons toochange hello time range of hello audit log.</span></span>

<span data-ttu-id="7f277-131">Ha úgy dönt, hogy hello **egyéni** gomb, Ön által megadott egy **a** dátummezője és egy **való** dátum mező toospecify dátumtartomány hello naplóhoz.</span><span class="sxs-lookup"><span data-stu-id="7f277-131">When you choose hello **Custom** button, you will be given a **From** date field and a **To** date field toospecify a range of dates for hello log.</span></span>  <span data-ttu-id="7f277-132">Hello dátumát éééé/hh/nn formátumban adja meg, vagy kattintson a hello **naptár** ikonra, és válassza a hello dátumát a naptárból.</span><span class="sxs-lookup"><span data-stu-id="7f277-132">You can either enter hello dates in MM/DD/YYYY format or click on hello **calendar** icon and choose hello date from a calendar.</span></span>

### <a name="change-hello-roles-included-in-hello-log"></a><span data-ttu-id="7f277-133">Hello szerepköreinek hello napló módosítása</span><span class="sxs-lookup"><span data-stu-id="7f277-133">Change hello roles included in hello log</span></span>
<span data-ttu-id="7f277-134">Ellenőrizze, vagy törölje a jelet hello **szerepkör** jelölőnégyzet következő tooeach szerepkör tooinclude vagy kizárási azt hello jelentkezzen.</span><span class="sxs-lookup"><span data-stu-id="7f277-134">Check or uncheck hello **Role** checkbox next tooeach role tooinclude or exclude it from hello log.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="7f277-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f277-135">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

