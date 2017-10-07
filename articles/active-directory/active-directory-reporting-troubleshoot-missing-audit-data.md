---
title: "Hibaelhárítás: Hiányzó adatok hello Azure Active Directory műveletnaplóban |} Microsoft Docs"
description: "Listák hello Azure Active Directory elérhető különböző jelentéseket."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 7bbec94ab42eb5b54a7e65e124060d057b4a1a34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="i-cant-find-some-actions-that-i-performed-in-hello-azure-active-directory-activity-log"></a><span data-ttu-id="c9b94-103">Nem található néhány hello Azure Active Directory tevékenységnapló végrehajtott műveletek</span><span class="sxs-lookup"><span data-stu-id="c9b94-103">I can’t find some actions that I performed in hello Azure Active Directory activity log</span></span>


## <a name="symptoms"></a><span data-ttu-id="c9b94-104">Probléma</span><span class="sxs-lookup"><span data-stu-id="c9b94-104">Symptoms</span></span>

<span data-ttu-id="c9b94-105">I végrehajtott bizonyos műveleteket a hello Azure-portálon, és a várt toosee hello naplókat a csatolási műveleteket hello `Activity logs > Audit Logs` panelen, de azokat nem található.</span><span class="sxs-lookup"><span data-stu-id="c9b94-105">I performed some actions in hello Azure portal and expected toosee hello audit logs for those actions in hello `Activity logs > Audit Logs` blade, but I can’t find them.</span></span>

 ![Jelentéskészítés](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a><span data-ttu-id="c9b94-107">Ok</span><span class="sxs-lookup"><span data-stu-id="c9b94-107">Cause</span></span>

<span data-ttu-id="c9b94-108">Műveletek nem jelennek meg azonnal hello tevékenység naplót.</span><span class="sxs-lookup"><span data-stu-id="c9b94-108">Actions don’t appear immediately in hello Activity Audit log.</span></span> <span data-ttu-id="c9b94-109">Azt is igénybe vehet tooan óra toosee hello auditnaplókat hello portálon hello művelet hello idő 15 perc.</span><span class="sxs-lookup"><span data-stu-id="c9b94-109">It can take anywhere from 15 minutes tooan hour toosee hello audit logs in hello portal from hello time hello operation is performed.</span></span>

## <a name="resolution"></a><span data-ttu-id="c9b94-110">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="c9b94-110">Resolution</span></span>

<span data-ttu-id="c9b94-111">Várjon 15 percig tooan órán keresztül, és ellenőrizze, hogy megjelennek-e hello műveletek hello naplóban.</span><span class="sxs-lookup"><span data-stu-id="c9b94-111">Wait for 15 minutes tooan hour and see if hello actions appear in hello log.</span></span> <span data-ttu-id="c9b94-112">Ha még mindig nem látja őket, hozzon létre egy támogatási jegyet, és megvizsgáljuk az ügyet.</span><span class="sxs-lookup"><span data-stu-id="c9b94-112">If you still don’t see them, please raise a support ticket with us and we will look into it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c9b94-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c9b94-113">Next steps</span></span>
<span data-ttu-id="c9b94-114">Lásd: hello [Azure Active Directory – gyakori kérdések reporting](active-directory-reporting-faq.md).</span><span class="sxs-lookup"><span data-stu-id="c9b94-114">See hello [Azure Active Directory reporting FAQ](active-directory-reporting-faq.md).</span></span>

