---
title: "Adathozzáférési házirendek az Azure Time Series Insightsban | Microsoft Docs"
description: "Az oktatóanyagból megismerheti az adathozzáférési házirendek kezelésének módját a Time Series Insightsban"
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
ms.openlocfilehash: e975c6d8f217bc73948c0c046204b16b1a742bc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a><span data-ttu-id="46d21-103">Adathozzáférés biztosítása egy Time Series Insights-környezethez az Azure Portal segítségével</span><span class="sxs-lookup"><span data-stu-id="46d21-103">Grant data access to a Time Series Insights environment using Azure portal</span></span>

<span data-ttu-id="46d21-104">A Time Series Insights-környezetek két különböző típusú hozzáférési házirenddel rendelkeznek:</span><span class="sxs-lookup"><span data-stu-id="46d21-104">Time Series Insights environments have two independent types of access policies:</span></span>

* <span data-ttu-id="46d21-105">Felügyeleti hozzáférési házirendek</span><span class="sxs-lookup"><span data-stu-id="46d21-105">Management access policies</span></span>
* <span data-ttu-id="46d21-106">Adathozzáférési házirendek</span><span class="sxs-lookup"><span data-stu-id="46d21-106">Data access policies</span></span>

<span data-ttu-id="46d21-107">Mindkét házirend különféle engedélyeket biztosít az Azure Active Directory rendszerbiztonsági tagjai (felhasználók és alkalmazások) számára egy adott környezetre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="46d21-107">Both policies grant Azure Active Directory principals (users and apps) various permissions on a particular environment.</span></span> <span data-ttu-id="46d21-108">A rendszerbiztonsági tagoknak (felhasználóknak és alkalmazásoknak) a környezetet tartalmazó előfizetéshez társított Active Directoryhoz (vagy „Azure-bérlőhöz”) kell tartozniuk.</span><span class="sxs-lookup"><span data-stu-id="46d21-108">The principals (users and apps) must belong to the active directory (or “Azure tenant”) associated with the subscription containing the environment.</span></span>

<span data-ttu-id="46d21-109">A felügyeleti hozzáférési házirendek a környezet konfigurálásához kapcsolódó engedélyeket biztosítanak, például:</span><span class="sxs-lookup"><span data-stu-id="46d21-109">Management access policies grant permissions related to the configuration of the environment, such as</span></span>
*   <span data-ttu-id="46d21-110">A környezet létrehozása vagy törlése, eseményforrások, referencia-adatkészletek; valamint</span><span class="sxs-lookup"><span data-stu-id="46d21-110">Creation and deletion of the environment, event sources, reference data sets, and</span></span>
*   <span data-ttu-id="46d21-111">Az adathozzáférési házirendek felügyelete.</span><span class="sxs-lookup"><span data-stu-id="46d21-111">Management of the data access policies.</span></span>

<span data-ttu-id="46d21-112">Az adathozzáférési házirendek a következőkhöz biztosítanak engedélyeket: adatlekérdezések kiadása, referenciaadatok módosítása a környezetben, valamint a környezethez társított mentett lekérdezések és perspektívák megosztása.</span><span class="sxs-lookup"><span data-stu-id="46d21-112">Data access policies grant permissions to issue data queries, manipulate reference data in the environment, and share saved queries and perspectives associated with the environment.</span></span>

<span data-ttu-id="46d21-113">A házirendek két típusa lehetővé teszi a környezet felügyeletéhez történő hozzáférés és a környezetben található adatokhoz való hozzáférés teljes mértékű szétválasztását.</span><span class="sxs-lookup"><span data-stu-id="46d21-113">The two kinds of policies allow clear separation between access to the management of the environment and access to the data inside the environment.</span></span> <span data-ttu-id="46d21-114">Például beállítható egy olyan környezet, amely esetében a környezet tulajdonosa/létrehozója el van távolítva az adathozzáférésből.</span><span class="sxs-lookup"><span data-stu-id="46d21-114">For example, it is possible to setup an environment such that the owner/creator of the environment is removed from the data access.</span></span> <span data-ttu-id="46d21-115">Emellett megadható, hogy azok a felhasználók és szolgáltatások, amelyek olvashatják a környezet adatait, ne kapjanak hozzáférést a környezet konfigurációjához.</span><span class="sxs-lookup"><span data-stu-id="46d21-115">As well as users and services that are allowed to read data from the environment may be granted no access to the configuration of the environment.</span></span>

## <a name="grant-data-access"></a><span data-ttu-id="46d21-116">Adathozzáférés biztosítása</span><span class="sxs-lookup"><span data-stu-id="46d21-116">Grant data access</span></span>
<span data-ttu-id="46d21-117">A következő lépések bemutatják, hogyan biztosítható adathozzáférés egy felhasználó rendszerbiztonsági tag számára:</span><span class="sxs-lookup"><span data-stu-id="46d21-117">The following steps show how to grant data access for a user principal:</span></span>

1.  <span data-ttu-id="46d21-118">Jelentkezzen be az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="46d21-118">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="46d21-119">Az Azure Portal bal oldali menüjében kattintson a „Minden erőforrás” lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="46d21-119">Click “All resources” in the menu on the left side of the Azure portal.</span></span>
3.  <span data-ttu-id="46d21-120">Válassza ki az Azure Time Series Insights-környezetet.</span><span class="sxs-lookup"><span data-stu-id="46d21-120">Select your Time Series Insights environment.</span></span>

  ![A Time Series Insights-forrás felügyelete – környezet](media/data-access/getstarted-grant-data-access1.png)

4.  <span data-ttu-id="46d21-122">Válassza az „Adatsík-hozzáférés” lehetőséget, majd kattintson a „Hozzáadás” gombra.</span><span class="sxs-lookup"><span data-stu-id="46d21-122">Select “Data Plane Access”, click “Add”</span></span>

  ![A Time Series Insights-forrás felügyelete – hozzáadás](media/data-access/getstarted-grant-data-access2.png)

5.  <span data-ttu-id="46d21-124">Kattintson a „Felhasználó kiválasztása” gombra.</span><span class="sxs-lookup"><span data-stu-id="46d21-124">Click “Select user”.</span></span>
6.  <span data-ttu-id="46d21-125">Keresse meg és válassza ki a felhasználót e-mail-cím alapján.</span><span class="sxs-lookup"><span data-stu-id="46d21-125">Search and select user by the email.</span></span>
7.  <span data-ttu-id="46d21-126">Kattintson a „Kiválasztás” gombra a „Felhasználó kiválasztása” panelen.</span><span class="sxs-lookup"><span data-stu-id="46d21-126">Click “Select” in “Select User” blade.</span></span>

  ![A Time Series Insights-forrás felügyelete – felhasználó kiválasztása](media/data-access/getstarted-grant-data-access3.png)

8.  <span data-ttu-id="46d21-128">Kattintson a „Szerepkör kiválasztása” gombra.</span><span class="sxs-lookup"><span data-stu-id="46d21-128">Click “Select role”.</span></span>
9.  <span data-ttu-id="46d21-129">Válassza a „Közreműködő” lehetőséget, ha engedélyezni szeretné a felhasználó számára, hogy módosítsa a referenciaadatokat és megoszthassa a mentett lekérdezéseket és perspektívákat a környezet más felhasználóival.</span><span class="sxs-lookup"><span data-stu-id="46d21-129">Select “Contributor” if you want to allow user to change reference data and share saved queries and perspectives with other users of the environment.</span></span> <span data-ttu-id="46d21-130">Egyéb esetben válassza az „Olvasó” lehetőséget. Ekkor a felhasználó lekérdezheti a környezet adatait, és személyes (nem megosztott) lekérdezéseket menthet a környezetben.</span><span class="sxs-lookup"><span data-stu-id="46d21-130">Otherwise select “Reader” to allow user query data in the environment and save personal (not shared) queries in the environment.</span></span>
10. <span data-ttu-id="46d21-131">Kattintson az „OK” gombra a „Szerepkör kiválasztása” panelen.</span><span class="sxs-lookup"><span data-stu-id="46d21-131">Click “Ok” in the “Select Role” blade.</span></span>

  ![A Time Series Insights-forrás felügyelete – szerepkör kiválasztása](media/data-access/getstarted-grant-data-access4.png)

11. <span data-ttu-id="46d21-133">Kattintson az „OK” gombra a „Felhasználói szerepkör kiválasztása” panelen.</span><span class="sxs-lookup"><span data-stu-id="46d21-133">Click “Ok” in the “Select User Role” blade.</span></span>
12. <span data-ttu-id="46d21-134">A következőnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="46d21-134">You should see:</span></span>

  ![A Time Series Insights-forrás felügyelete – eredmények](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a><span data-ttu-id="46d21-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46d21-136">Next steps</span></span>

* [<span data-ttu-id="46d21-137">Eseményforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="46d21-137">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="46d21-138">[Események küldése](time-series-insights-send-events.md) az eseményforrásnak</span><span class="sxs-lookup"><span data-stu-id="46d21-138">[Send events](time-series-insights-send-events.md) to the event source</span></span>
* <span data-ttu-id="46d21-139">A környezet megtekintése a [Time Series Insights portálon](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="46d21-139">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
