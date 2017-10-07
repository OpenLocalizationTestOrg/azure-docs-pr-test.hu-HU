---
title: "aaaData hozzáférési szabályzatokat az Azure idő adatsorozat Insights |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja toomanage adat-hozzáférési házirendjeit idő adatsorozat insightsban"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: f286d26c8c5c851c523e9384760dc4b10711fa3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="grant-data-access-tooa-time-series-insights-environment-using-azure-portal"></a><span data-ttu-id="7814f-103">Adja meg a data access tooa idő adatsorozat Insights környezet az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="7814f-103">Grant data access tooa Time Series Insights environment using Azure portal</span></span>

<span data-ttu-id="7814f-104">A Time Series Insights-környezetek két különböző típusú hozzáférési házirenddel rendelkeznek:</span><span class="sxs-lookup"><span data-stu-id="7814f-104">Time Series Insights environments have two independent types of access policies:</span></span>

* <span data-ttu-id="7814f-105">Felügyeleti hozzáférési házirendek</span><span class="sxs-lookup"><span data-stu-id="7814f-105">Management access policies</span></span>
* <span data-ttu-id="7814f-106">Adathozzáférési házirendek</span><span class="sxs-lookup"><span data-stu-id="7814f-106">Data access policies</span></span>

<span data-ttu-id="7814f-107">Mindkét házirend különféle engedélyeket biztosít az Azure Active Directory rendszerbiztonsági tagjai (felhasználók és alkalmazások) számára egy adott környezetre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="7814f-107">Both policies grant Azure Active Directory principals (users and apps) various permissions on a particular environment.</span></span> <span data-ttu-id="7814f-108">hello rendszerbiztonsági tagok (felhasználók és alkalmazások) kell tartozniuk aktív toohello hello környezet tartalmazó hello előfizetéshez tartozó címtár (vagy "Azure-bérlőhöz").</span><span class="sxs-lookup"><span data-stu-id="7814f-108">hello principals (users and apps) must belong toohello active directory (or “Azure tenant”) associated with hello subscription containing hello environment.</span></span>

<span data-ttu-id="7814f-109">Felügyeleti hozzáférési házirendek engedélyek kapcsolódó toohello konfigurációs hello környezet, például a engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7814f-109">Management access policies grant permissions related toohello configuration of hello environment, such as</span></span>
*   <span data-ttu-id="7814f-110">A környezet hello eseményforrások, létrehozását és törlését hivatkozási adatkészletek, és</span><span class="sxs-lookup"><span data-stu-id="7814f-110">Creation and deletion of hello environment, event sources, reference data sets, and</span></span>
*   <span data-ttu-id="7814f-111">Adat-hozzáférési házirendjeit hello kezelése.</span><span class="sxs-lookup"><span data-stu-id="7814f-111">Management of hello data access policies.</span></span>

<span data-ttu-id="7814f-112">Adat-hozzáférési házirendjeit engedélyeket tooissue lekérdezések, kezelheti a referenciaadatok hello környezetben és lekérdezések és hello környezet perspektívákat.</span><span class="sxs-lookup"><span data-stu-id="7814f-112">Data access policies grant permissions tooissue data queries, manipulate reference data in hello environment, and share saved queries and perspectives associated with hello environment.</span></span>

<span data-ttu-id="7814f-113">házirendek hello kétféle engedélyezése egyértelmű elkülönítése is megvalósuljon toohello kezelési hello környezet és a hozzáférés toohello adatok hello környezeten belül.</span><span class="sxs-lookup"><span data-stu-id="7814f-113">hello two kinds of policies allow clear separation between access toohello management of hello environment and access toohello data inside hello environment.</span></span> <span data-ttu-id="7814f-114">Például is lehetséges toosetup olyan környezetet úgy, hogy hello tulajdonos/létrehozó hello környezet hello adatelérési törlődik.</span><span class="sxs-lookup"><span data-stu-id="7814f-114">For example, it is possible toosetup an environment such that hello owner/creator of hello environment is removed from hello data access.</span></span> <span data-ttu-id="7814f-115">Továbbá a felhasználók és a szolgáltatások, amelyek számára engedélyezett tooread adatok hello környezetből adható nem toohello konfiguráció hello környezet.</span><span class="sxs-lookup"><span data-stu-id="7814f-115">As well as users and services that are allowed tooread data from hello environment may be granted no access toohello configuration of hello environment.</span></span>

## <a name="grant-data-access"></a><span data-ttu-id="7814f-116">Adathozzáférés biztosítása</span><span class="sxs-lookup"><span data-stu-id="7814f-116">Grant data access</span></span>
<span data-ttu-id="7814f-117">hello következő lépések bemutatják, hogyan toogrant adatok elérését egy egyszerű:</span><span class="sxs-lookup"><span data-stu-id="7814f-117">hello following steps show how toogrant data access for a user principal:</span></span>

1.  <span data-ttu-id="7814f-118">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7814f-118">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="7814f-119">Kattintson az "Összes erőforrás" hello menü hello hello Azure-portálon a bal oldalán.</span><span class="sxs-lookup"><span data-stu-id="7814f-119">Click “All resources” in hello menu on hello left side of hello Azure portal.</span></span>
3.  <span data-ttu-id="7814f-120">Válassza ki az Azure Time Series Insights-környezetet.</span><span class="sxs-lookup"><span data-stu-id="7814f-120">Select your Time Series Insights environment.</span></span>

  ![Kezelheti a hello idő adatsorozat Insights forrás - környezet](media/data-access/getstarted-grant-data-access1.png)

4.  <span data-ttu-id="7814f-122">Válassza az „Adatsík-hozzáférés” lehetőséget, majd kattintson a „Hozzáadás” gombra.</span><span class="sxs-lookup"><span data-stu-id="7814f-122">Select “Data Plane Access”, click “Add”</span></span>

  ![Hello idő adatsorozat Insights forrás kezelése – hozzáadása](media/data-access/getstarted-grant-data-access2.png)

5.  <span data-ttu-id="7814f-124">Kattintson a „Felhasználó kiválasztása” gombra.</span><span class="sxs-lookup"><span data-stu-id="7814f-124">Click “Select user”.</span></span>
6.  <span data-ttu-id="7814f-125">Keresse meg és válassza ki a felhasználói hello e-mailben.</span><span class="sxs-lookup"><span data-stu-id="7814f-125">Search and select user by hello email.</span></span>
7.  <span data-ttu-id="7814f-126">Kattintson a „Kiválasztás” gombra a „Felhasználó kiválasztása” panelen.</span><span class="sxs-lookup"><span data-stu-id="7814f-126">Click “Select” in “Select User” blade.</span></span>

  ![Hello idő adatsorozat Insights forrás - felhasználó kijelölése kezelése](media/data-access/getstarted-grant-data-access3.png)

8.  <span data-ttu-id="7814f-128">Kattintson a „Szerepkör kiválasztása” gombra.</span><span class="sxs-lookup"><span data-stu-id="7814f-128">Click “Select role”.</span></span>
9.  <span data-ttu-id="7814f-129">Válassza ki a "Közreműködői", ha tooallow felhasználói toochange referenciaadatok és perspektívák és lekérdezések megosztása más felhasználókkal hello környezet.</span><span class="sxs-lookup"><span data-stu-id="7814f-129">Select “Contributor” if you want tooallow user toochange reference data and share saved queries and perspectives with other users of hello environment.</span></span> <span data-ttu-id="7814f-130">Ellenkező esetben válassza a "Olvasó" tooallow felhasználói adatait hello környezetben, és mentse a személyes (megosztott) lekérdezések hello környezetben.</span><span class="sxs-lookup"><span data-stu-id="7814f-130">Otherwise select “Reader” tooallow user query data in hello environment and save personal (not shared) queries in hello environment.</span></span>
10. <span data-ttu-id="7814f-131">Kattintson az "Ok" hello "Szerepkör kiválasztása" panelen.</span><span class="sxs-lookup"><span data-stu-id="7814f-131">Click “Ok” in hello “Select Role” blade.</span></span>

  ![Hello idő adatsorozat Insights forrás - válassza szerepkör kezelése](media/data-access/getstarted-grant-data-access4.png)

11. <span data-ttu-id="7814f-133">Kattintson az "Ok" hello "A felhasználói szerepkör kiválasztása" panelen.</span><span class="sxs-lookup"><span data-stu-id="7814f-133">Click “Ok” in hello “Select User Role” blade.</span></span>
12. <span data-ttu-id="7814f-134">A következőnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="7814f-134">You should see:</span></span>

  ![Hello idő adatsorozat Insights forrás - eredmények kezelése](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a><span data-ttu-id="7814f-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7814f-136">Next steps</span></span>

* [<span data-ttu-id="7814f-137">Eseményforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7814f-137">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="7814f-138">[Események küldése](time-series-insights-send-events.md) toohello eseményforrás</span><span class="sxs-lookup"><span data-stu-id="7814f-138">[Send events](time-series-insights-send-events.md) toohello event source</span></span>
* <span data-ttu-id="7814f-139">A környezet megtekintése a [Time Series Insights portálon](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="7814f-139">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
