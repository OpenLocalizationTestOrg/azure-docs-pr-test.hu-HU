---
title: "Adja hozzá a Facebook-összekötőt a Logic Apps |} Microsoft Docs"
description: "A REST API-paraméterekkel rendelkező Facebook-összekötő áttekintése"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f4d6f0ed-c09b-488c-be1c-8cf2b5b1d4b8
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia; ladocs
ms.openlocfilehash: e10a30ccef3e81cb3d7749696453d82b8958d076
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-facebook-connector"></a><span data-ttu-id="8a665-103">A Facebook-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="8a665-103">Get started with the Facebook connector</span></span>
<span data-ttu-id="8a665-104">Csatlakozni a Facebookhoz és idővonalon, hírcsatorna oldal, és több.</span><span class="sxs-lookup"><span data-stu-id="8a665-104">Connect to Facebook and post to a timeline, get a page feed, and more.</span></span> <span data-ttu-id="8a665-105">A Facebook a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="8a665-105">With Facebook, you can:</span></span>

* <span data-ttu-id="8a665-106">Az üzleti folyamat Facebook származó adatok alapján történő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8a665-106">Build your business flow based on the data you get from Facebook.</span></span> 
* <span data-ttu-id="8a665-107">Egy eseményindító használni, új post fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="8a665-107">Use a trigger when a new post is received.</span></span>
* <span data-ttu-id="8a665-108">Küldje el a ütemterv használatát műveleteket hírcsatorna oldal, és több kapják meg.</span><span class="sxs-lookup"><span data-stu-id="8a665-108">Use actions that post to your timeline, get a page feed, and more.</span></span> <span data-ttu-id="8a665-109">Ezeket a műveleteket válaszol, és végezze el a kimeneti más műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="8a665-109">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="8a665-110">Például, ha van egy új post az ütemterven, igénybe a feladás egy vagy több, és hogy a Twitter hírcsatorna.</span><span class="sxs-lookup"><span data-stu-id="8a665-110">For example, when there is a new post on your timeline, you can take that post and push it to your Twitter feed.</span></span> 

<span data-ttu-id="8a665-111">Most hozzon létre egy logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="8a665-111">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-facebook"></a><span data-ttu-id="8a665-112">Kapcsolatot létesíthet a Facebook-on</span><span class="sxs-lookup"><span data-stu-id="8a665-112">Create a connection to Facebook</span></span>
<span data-ttu-id="8a665-113">Ezt az összekötőt a logic apps hozzáadásakor engedélyeznie kell a logic apps csatlakozni a Facebookhoz.</span><span class="sxs-lookup"><span data-stu-id="8a665-113">When you add this connector to your logic apps, you must authorize logic apps to connect to your Facebook.</span></span>

1. <span data-ttu-id="8a665-114">Jelentkezzen be a Facebook-fiókba</span><span class="sxs-lookup"><span data-stu-id="8a665-114">Sign in to your Facebook account</span></span>
2. <span data-ttu-id="8a665-115">Válassza ki **engedélyezés**, és teszi lehetővé a logic Apps alkalmazásokat csatlakozzon, és használja a Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="8a665-115">Select **Authorize**, and allow your logic apps to connect and use your Facebook.</span></span> 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 


## <a name="connector-specific-details"></a><span data-ttu-id="8a665-116">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="8a665-116">Connector-specific details</span></span>

<span data-ttu-id="8a665-117">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/facebook/).</span><span class="sxs-lookup"><span data-stu-id="8a665-117">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/facebook/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="8a665-118">További összekötők</span><span class="sxs-lookup"><span data-stu-id="8a665-118">More connectors</span></span>
<span data-ttu-id="8a665-119">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="8a665-119">Go back to the [APIs list](apis-list.md).</span></span>