---
title: "az Azure Logic Apps aaaGitHub összekötő |} Microsoft Docs"
description: "Az Azure App service Logic Apps alkalmazások létrehozása GitHub egy webalapú Git-tárház szolgáltatást tartalmazó. Összes elosztott hello változat-vezérléshez és a forrás kódot (SCM) felügyeleti funkció Git, valamint a saját szolgáltatások hozzáadására kínál."
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
ms.openlocfilehash: a75070e6371be625e5cf24a8dc0200844b7a6891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-github-connector"></a><span data-ttu-id="218f8-105">Hello GitHub-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="218f8-105">Get started with hello GitHub connector</span></span>
<span data-ttu-id="218f8-106">GitHub egy webalapú Git-tárház szolgáltatást tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="218f8-106">GitHub is a web-based Git repository hosting service.</span></span> <span data-ttu-id="218f8-107">Összes elosztott hello változat-vezérléshez és a forrás kódot (SCM) felügyeleti funkció Git, valamint a saját szolgáltatások hozzáadására kínál.</span><span class="sxs-lookup"><span data-stu-id="218f8-107">It offers all of hello distributed revision control and source code management (SCM) functionality of Git as well as adding its own features.</span></span>

<span data-ttu-id="218f8-108">Most hozzon létre egy Logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="218f8-108">You can get started by creating a Logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-toogithub"></a><span data-ttu-id="218f8-109">Egy kapcsolat tooGitHub létrehozása</span><span class="sxs-lookup"><span data-stu-id="218f8-109">Create a connection tooGitHub</span></span>
<span data-ttu-id="218f8-110">toocreate Logic apps a Githubon, létre kell hoznia egy **kapcsolat** hello részletek adja meg a következő tulajdonságok hello:</span><span class="sxs-lookup"><span data-stu-id="218f8-110">toocreate Logic apps with GitHub, you must first create a **connection** then provide hello details for hello following properties:</span></span> 

| <span data-ttu-id="218f8-111">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="218f8-111">Property</span></span> | <span data-ttu-id="218f8-112">Szükséges</span><span class="sxs-lookup"><span data-stu-id="218f8-112">Required</span></span> | <span data-ttu-id="218f8-113">Leírás</span><span class="sxs-lookup"><span data-stu-id="218f8-113">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="218f8-114">Token</span><span class="sxs-lookup"><span data-stu-id="218f8-114">Token</span></span> |<span data-ttu-id="218f8-115">Igen</span><span class="sxs-lookup"><span data-stu-id="218f8-115">Yes</span></span> |<span data-ttu-id="218f8-116">Adja meg a GitHub hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="218f8-116">Provide GitHub Credentials</span></span> |

<span data-ttu-id="218f8-117">Hello kapcsolat létrehozása után tooexecute hello műveletek használják, és a jelen cikkben ismertetett hello eseményindítók figyelésére.</span><span class="sxs-lookup"><span data-stu-id="218f8-117">After you create hello connection, you can use it tooexecute hello actions and listen for hello triggers described in this article.</span></span> 

> [!INCLUDE [Steps toocreate a connection tooGitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="218f8-118">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="218f8-118">Connector-specific details</span></span>

<span data-ttu-id="218f8-119">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/github/).</span><span class="sxs-lookup"><span data-stu-id="218f8-119">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/github/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="218f8-120">További összekötők</span><span class="sxs-lookup"><span data-stu-id="218f8-120">More connectors</span></span>
<span data-ttu-id="218f8-121">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="218f8-121">Go back toohello [APIs list](apis-list.md).</span></span>
