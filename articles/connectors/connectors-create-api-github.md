---
title: "Az Azure Logic Apps GitHub összekötő |} Microsoft Docs"
description: "Az Azure App service Logic Apps alkalmazások létrehozása GitHub egy webalapú Git-tárház szolgáltatást tartalmazó. Összes elosztott változat-vezérléshez és a forrás kódot (SCM) felügyeleti funkció Git, valamint a saját szolgáltatások hozzáadására kínál."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: d4614b0ceff0ec0d36dbb1a136551f985f2fc1a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-github-connector"></a><span data-ttu-id="1ac87-105">A GitHub-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="1ac87-105">Get started with the GitHub connector</span></span>
<span data-ttu-id="1ac87-106">GitHub egy webalapú Git-tárház szolgáltatást tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="1ac87-106">GitHub is a web-based Git repository hosting service.</span></span> <span data-ttu-id="1ac87-107">Összes elosztott változat-vezérléshez és a forrás kódot (SCM) felügyeleti funkció Git, valamint a saját szolgáltatások hozzáadására kínál.</span><span class="sxs-lookup"><span data-stu-id="1ac87-107">It offers all of the distributed revision control and source code management (SCM) functionality of Git as well as adding its own features.</span></span>

<span data-ttu-id="1ac87-108">Most hozzon létre egy Logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1ac87-108">You can get started by creating a Logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-github"></a><span data-ttu-id="1ac87-109">Kapcsolatot létesíthet a Githubon</span><span class="sxs-lookup"><span data-stu-id="1ac87-109">Create a connection to GitHub</span></span>
<span data-ttu-id="1ac87-110">A Logic apps GitHub létrehozásához, létre kell hoznia egy **kapcsolat** adja meg a részleteket a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="1ac87-110">To create Logic apps with GitHub, you must first create a **connection** then provide the details for the following properties:</span></span> 

| <span data-ttu-id="1ac87-111">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1ac87-111">Property</span></span> | <span data-ttu-id="1ac87-112">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1ac87-112">Required</span></span> | <span data-ttu-id="1ac87-113">Leírás</span><span class="sxs-lookup"><span data-stu-id="1ac87-113">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1ac87-114">Token</span><span class="sxs-lookup"><span data-stu-id="1ac87-114">Token</span></span> |<span data-ttu-id="1ac87-115">Igen</span><span class="sxs-lookup"><span data-stu-id="1ac87-115">Yes</span></span> |<span data-ttu-id="1ac87-116">Adja meg a GitHub hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="1ac87-116">Provide GitHub Credentials</span></span> |

<span data-ttu-id="1ac87-117">Miután létrehozta a kapcsolatot, használhatja a műveletek végrehajtása és a jelen cikkben ismertetett eseményindítók figyelni.</span><span class="sxs-lookup"><span data-stu-id="1ac87-117">After you create the connection, you can use it to execute the actions and listen for the triggers described in this article.</span></span> 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="1ac87-118">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="1ac87-118">Connector-specific details</span></span>

<span data-ttu-id="1ac87-119">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/github/).</span><span class="sxs-lookup"><span data-stu-id="1ac87-119">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/github/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="1ac87-120">További összekötők</span><span class="sxs-lookup"><span data-stu-id="1ac87-120">More connectors</span></span>
<span data-ttu-id="1ac87-121">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="1ac87-121">Go back to the [APIs list](apis-list.md).</span></span>