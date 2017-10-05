---
title: "Hibaelhárítás: Hiányzó adatok a letöltött Azure Active Directory-tevékenységnaplókban | Microsoft Docs"
description: "A letöltött Azure Active Directory-tevékenységnaplókból hiányzó adatok problémájára nyújt megoldást."
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
ms.openlocfilehash: 3d56f89035da4d1a0074256b165663f81fc2b01e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded"></a><span data-ttu-id="6e767-103">Nem találhatók adatok az Azure Active Directory letöltött tevékenységnaplóiban</span><span class="sxs-lookup"><span data-stu-id="6e767-103">I can’t find any data in the Azure Active Directory activity logs I have downloaded</span></span>


## <a name="symptoms"></a><span data-ttu-id="6e767-104">Probléma</span><span class="sxs-lookup"><span data-stu-id="6e767-104">Symptoms</span></span>

<span data-ttu-id="6e767-105">Letöltöttem a tevékenységnaplókat (audit vagy bejelentkezési), és nem látom a kiválasztott időre vonatkozó összes rekordot.</span><span class="sxs-lookup"><span data-stu-id="6e767-105">I downloaded the activity logs (audit or sign-ins) and I don’t see all the records for the time I chose.</span></span> <span data-ttu-id="6e767-106">Hogy miért?</span><span class="sxs-lookup"><span data-stu-id="6e767-106">Why?</span></span> 

 ![Jelentéskészítés](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a><span data-ttu-id="6e767-108">Ok</span><span class="sxs-lookup"><span data-stu-id="6e767-108">Cause</span></span>

<span data-ttu-id="6e767-109">Amikor tevékenységnaplókat tölt le az Azure Portalon, 120 ezer rekordra korlátozzuk a letöltött tartományt, és a legfrissebb elemek kerülnek előre.</span><span class="sxs-lookup"><span data-stu-id="6e767-109">When you download activity logs in the Azure portal, we limit the scale to 120K records, sorted by most recent.</span></span> 

## <a name="resolution"></a><span data-ttu-id="6e767-110">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="6e767-110">Resolution</span></span>

<span data-ttu-id="6e767-111">Az [Azure AD Reporting API-kkal](active-directory-reporting-api-getting-started.md) akár egymillió rekordot is lekérdezhet.</span><span class="sxs-lookup"><span data-stu-id="6e767-111">You can leverage [Azure AD Reporting APIs](active-directory-reporting-api-getting-started.md) to fetch up to a million records at any given point.</span></span> <span data-ttu-id="6e767-112">Azt ajánljuk, hogy futtasson egy ütemezett szkriptet, amely meghívja a jelentéskészítő API-kat, hogy növekményes módon kérdezzék le az egy adott időszakra (például egy napra vagy hétre) vonatkozó rekordokat.</span><span class="sxs-lookup"><span data-stu-id="6e767-112">Our recommended approach is to run a script on a scheduled basis that calls the reporting APIs to fetch records in an incremental fashion over a period of time (e.g., daily or weekly).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e767-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e767-113">Next steps</span></span>
<span data-ttu-id="6e767-114">További információ: [Jelentéskészítés az Azure Active Directoryban – gyakori kérdések](active-directory-reporting-faq.md).</span><span class="sxs-lookup"><span data-stu-id="6e767-114">See the [Azure Active Directory reporting FAQ](active-directory-reporting-faq.md).</span></span>

