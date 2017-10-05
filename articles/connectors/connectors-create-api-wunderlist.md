---
title: "Wunderlist összekötő i n Azure logic apps |} Microsoft Docs"
description: "Kapcsolatot létesíthet Wunderlist, és ehhez a kapcsolathoz használni a logic apps a munkafolyamat létrehozásához."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: e4773ecf-3ad3-44b4-a1b5-ee5f58baeadd
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 899110992cc52ca5edf1706320fd5570473de784
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-wunderlist-connector"></a><span data-ttu-id="bdca0-103">A Wunderlist összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="bdca0-103">Get started with the Wunderlist connector</span></span>
<span data-ttu-id="bdca0-104">Wunderlist adja meg a teendőlista lista és a feladat manager személyek saját dolgai végzett beszerzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="bdca0-104">Wunderlist provide a todo list and task manager to help people get their stuff done.</span></span>  <span data-ttu-id="bdca0-105">Egy bevásárlólista loved egy osztunk meg, hogy egy projekten dolgozik, vagy egy szabadsága tervezési Wunderlist egyszerűen rögzítése, megosztás, és végezze el a to¬dos.</span><span class="sxs-lookup"><span data-stu-id="bdca0-105">Whether you’re sharing a grocery list with a loved one, working on a project, or planning a vacation, Wunderlist makes it easy to capture, share, and complete your to¬dos.</span></span> <span data-ttu-id="bdca0-106">Wunderlist azonnal szinkronizálja a telefon, a tábla és a számítógép, között, hogy bárhonnan hozzáférhessen a feladatok.</span><span class="sxs-lookup"><span data-stu-id="bdca0-106">Wunderlist instantly syncs between your phone, tablet and computer, so you can access all your tasks from anywhere.</span></span>

<span data-ttu-id="bdca0-107">Első lépések egy logikai alkalmazás létrehozása most; Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="bdca0-107">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-wunderlist"></a><span data-ttu-id="bdca0-108">Kapcsolatot létesíthet Wunderlist</span><span class="sxs-lookup"><span data-stu-id="bdca0-108">Create a connection to Wunderlist</span></span>
<span data-ttu-id="bdca0-109">A Logic apps Wunderlist létrehozásához, létre kell hoznia egy **kapcsolat** adja meg a részleteket a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="bdca0-109">To create Logic apps with Wunderlist, you must first create a **connection** then provide the details for the following properties:</span></span>

| <span data-ttu-id="bdca0-110">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bdca0-110">Property</span></span> | <span data-ttu-id="bdca0-111">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bdca0-111">Required</span></span> | <span data-ttu-id="bdca0-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="bdca0-112">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bdca0-113">Token</span><span class="sxs-lookup"><span data-stu-id="bdca0-113">Token</span></span> |<span data-ttu-id="bdca0-114">Igen</span><span class="sxs-lookup"><span data-stu-id="bdca0-114">Yes</span></span> |<span data-ttu-id="bdca0-115">Wunderlist hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="bdca0-115">Provide Wunderlist Credentials</span></span> |

<span data-ttu-id="bdca0-116">Miután létrehozta a kapcsolatot, a műveletek végrehajtásához, és figyeljen az eseményindítók a használhatja.</span><span class="sxs-lookup"><span data-stu-id="bdca0-116">After you create the connection, you can use it to execute the actions and listen for the triggers.</span></span>

> [!INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="bdca0-117">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="bdca0-117">Connector-specific details</span></span>

<span data-ttu-id="bdca0-118">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/wunderlist/).</span><span class="sxs-lookup"><span data-stu-id="bdca0-118">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/wunderlist/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="bdca0-119">További összekötők</span><span class="sxs-lookup"><span data-stu-id="bdca0-119">More connectors</span></span>
<span data-ttu-id="bdca0-120">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="bdca0-120">Go back to the [APIs list](apis-list.md).</span></span>