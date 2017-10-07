---
title: "Hibáinak elhárítása: Hiányzó adatok hello letöltött Azure Active Directory tevékenységi naplóit |} Microsoft Docs"
description: "A letöltött Azure Active Directory tevékenységi naplóit feloldási toomissing adatokat biztosít."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 027b70e6efc570f81d3c836f50ee52aaa89be71a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="i-cant-find-any-data-in-hello-azure-active-directory-activity-logs-i-have-downloaded"></a><span data-ttu-id="a75bf-103">Nem található adatok hello Azure Active Directory tevékenységi naplóit le van töltve</span><span class="sxs-lookup"><span data-stu-id="a75bf-103">I can’t find any data in hello Azure Active Directory activity logs I have downloaded</span></span>


## <a name="symptoms"></a><span data-ttu-id="a75bf-104">Probléma</span><span class="sxs-lookup"><span data-stu-id="a75bf-104">Symptoms</span></span>

<span data-ttu-id="a75bf-105">Le hello tevékenységi naplóit (naplózási vagy bejelentkezéseket) töltve, és nem szerepel az összes hello rekord elfogadása hello ideje.</span><span class="sxs-lookup"><span data-stu-id="a75bf-105">I downloaded hello activity logs (audit or sign-ins) and I don’t see all hello records for hello time I chose.</span></span> <span data-ttu-id="a75bf-106">Hogy miért?</span><span class="sxs-lookup"><span data-stu-id="a75bf-106">Why?</span></span> 

 ![Jelentéskészítés](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a><span data-ttu-id="a75bf-108">Ok</span><span class="sxs-lookup"><span data-stu-id="a75bf-108">Cause</span></span>

<span data-ttu-id="a75bf-109">Ha le az Azure-portálon hello tevékenységi naplóit, azt hello méretezési too120K rekordok, legtöbb rendezve korlátozására legutóbbi.</span><span class="sxs-lookup"><span data-stu-id="a75bf-109">When you download activity logs in hello Azure portal, we limit hello scale too120K records, sorted by most recent.</span></span> 

## <a name="resolution"></a><span data-ttu-id="a75bf-110">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="a75bf-110">Resolution</span></span>

<span data-ttu-id="a75bf-111">Kihasználhatja [az Azure AD Reporting API-k](active-directory-reporting-api-getting-started.md) toofetch tooa millió rekordok álljon.</span><span class="sxs-lookup"><span data-stu-id="a75bf-111">You can leverage [Azure AD Reporting APIs](active-directory-reporting-api-getting-started.md) toofetch up tooa million records at any given point.</span></span> <span data-ttu-id="a75bf-112">Az ajánlott megoldás, egy meghatározott időtartamra vonatkozóan egy egyedi módon rögzíti, amely behívja hello jelentéskészítési API-k toofetch ütemezés szerint parancsfájlt toorun, (például naponta vagy hetente).</span><span class="sxs-lookup"><span data-stu-id="a75bf-112">Our recommended approach is toorun a script on a scheduled basis that calls hello reporting APIs toofetch records in an incremental fashion over a period of time (e.g., daily or weekly).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a75bf-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a75bf-113">Next steps</span></span>
<span data-ttu-id="a75bf-114">Lásd: hello [Azure Active Directory – gyakori kérdések reporting](active-directory-reporting-faq.md).</span><span class="sxs-lookup"><span data-stu-id="a75bf-114">See hello [Azure Active Directory reporting FAQ](active-directory-reporting-faq.md).</span></span>

