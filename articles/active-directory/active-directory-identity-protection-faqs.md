---
title: "aaaAzure Active Directory Identity Protection – gyakori kérdések |} Microsoft Docs"
description: "Az Azure AD Identity Protection kapcsolatos gyakori kérdések"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6a053ce02bf7a248a284f482f20f96a312f1c204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-faq"></a><span data-ttu-id="66316-103">Az Azure Active Directory Identity Protection – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="66316-103">Azure Active Directory Identity Protection FAQ</span></span>


## <a name="why-do-some-risk-events-have-closed-system-status"></a><span data-ttu-id="66316-104">Miért van néhány kockázati események "(rendszer) Lezárva" állapota?</span><span class="sxs-lookup"><span data-stu-id="66316-104">Why do some risk events have “Closed (system)” status?</span></span>

<span data-ttu-id="66316-105">**V:** ezek olyan eseményeket, amelyek az Azure Active Directory Identity Protection észlelt, és később bezárva, mert hello események már nem vették kockázatos.</span><span class="sxs-lookup"><span data-stu-id="66316-105">**A:** These are events that Azure Active Directory Identity Protection detected and later closed because hello events were no longer considered risky.</span></span> <span data-ttu-id="66316-106">Ezek az események nem számítanak bele hello felhasználói kockázati szintjét.</span><span class="sxs-lookup"><span data-stu-id="66316-106">These events do not count towards hello user’s risk level.</span></span> 

---

## <a name="do-i-need-toobe-a-global-admin-toouse-identity-protection-in-hello-azure-portal"></a><span data-ttu-id="66316-107">Egy globális rendszergazdai toouse Identity Protection az Azure-portálon hello toobe kell?</span><span class="sxs-lookup"><span data-stu-id="66316-107">Do I need toobe a global admin toouse Identity Protection in hello Azure portal?</span></span>
<span data-ttu-id="66316-108">**V:** **nem**.</span><span class="sxs-lookup"><span data-stu-id="66316-108">**A:** **No**.</span></span> <span data-ttu-id="66316-109">A biztonsági olvasó, a biztonsági rendszergazda vagy egy globális rendszergazdai toouse Identity Protection értéke lehet.</span><span class="sxs-lookup"><span data-stu-id="66316-109">You can either be a Security Reader, a Security Admin or a Global Admin toouse Identity Protection.</span></span>

---

## <a name="how-do-i-get-identity-protection"></a><span data-ttu-id="66316-110">Hogyan szerezhetek Identity Protection?</span><span class="sxs-lookup"><span data-stu-id="66316-110">How do I get Identity Protection?</span></span>
<span data-ttu-id="66316-111">**V:** lásd [Ismerkedés az Azure Active Directory Premium](active-directory-get-started-premium.md) egy válasz toothis kérdés.</span><span class="sxs-lookup"><span data-stu-id="66316-111">**A:** See [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md) for an answer toothis question.</span></span>

---

## <a name="what-happens-when-your-azure-active-directory-premium-2-trial-expires"></a><span data-ttu-id="66316-112">Mi történik, ha az Azure Active Directory Premium 2 próbaverzió lejárta?</span><span class="sxs-lookup"><span data-stu-id="66316-112">What happens when your Azure Active Directory Premium 2 trial expires?</span></span>

<span data-ttu-id="66316-113">**V:** Ha az Azure Active Directory ingyenes, alapszintű vagy Premium 1 kiadásán az Azure Active Directory Premium 2 próbaverziója lejárt, és az Identity Protection engedélyezve volt, továbbra is meg kell hozzáférést tooit annak ellenére, hogy Ön nem megfelelt, Licencelés.</span><span class="sxs-lookup"><span data-stu-id="66316-113">**A:** If your Azure Active Directory Premium 2 trial edition has expired on your Azure Active Directory Free, Basic or Premium 1 edition, and you had Identity Protection enabled, you still have access tooit even though you are not in compliance with licensing.</span></span>

---
