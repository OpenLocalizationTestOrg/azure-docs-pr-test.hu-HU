---
title: "Bemutatják, hogyan használhatja a Salesforce-összekötőt a logic Apps alkalmazásait |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása A Salesforce-összekötő API-jával különféle műveleteket végezhet a Salesforce-objektumokon."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 54fe5af8-7d2a-4da8-94e7-15d029e029bf
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/05/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c2e2efd356382df9404f5c4ed54f24758b2cd22b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-salesforce-connector"></a><span data-ttu-id="bdffe-104">A Salesforce-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="bdffe-104">Get started with the Salesforce connector</span></span>
<span data-ttu-id="bdffe-105">A Salesforce-összekötő API-jával különféle műveleteket végezhet a Salesforce-objektumokon.</span><span class="sxs-lookup"><span data-stu-id="bdffe-105">The Salesforce Connector provides an API to work with Salesforce objects.</span></span>

<span data-ttu-id="bdffe-106">Használandó [a csatlakozókat](apis-list.md), először hozzon létre egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bdffe-106">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="bdffe-107">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="bdffe-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-salesforce-connector"></a><span data-ttu-id="bdffe-108">Kapcsolódás a Salesforce-összekötő</span><span class="sxs-lookup"><span data-stu-id="bdffe-108">Connect to Salesforce connector</span></span>
<span data-ttu-id="bdffe-109">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először hozzon létre egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="bdffe-109">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="bdffe-110">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="bdffe-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-salesforce-connector"></a><span data-ttu-id="bdffe-111">Kapcsolatot létesíthet a Salesforce-összekötő</span><span class="sxs-lookup"><span data-stu-id="bdffe-111">Create a connection to Salesforce connector</span></span>
> [!INCLUDE [Steps to create a connection to Salesforce Connector](../../includes/connectors-create-api-salesforce.md)]
> 
> 

## <a name="use-a-salesforce-connector-trigger"></a><span data-ttu-id="bdffe-112">Salesforce összekötő eseményindítók</span><span class="sxs-lookup"><span data-stu-id="bdffe-112">Use a Salesforce connector trigger</span></span>
<span data-ttu-id="bdffe-113">Egy eseményindító nem egy eseményt, a logikai alkalmazás definiált munkafolyamat indításához használható.</span><span class="sxs-lookup"><span data-stu-id="bdffe-113">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="bdffe-114">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="bdffe-114">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps to create a Salesforce trigger](../../includes/connectors-create-api-salesforce-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="bdffe-115">Feltétel hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bdffe-115">Add a condition</span></span>
> [!INCLUDE [Steps to create a Salesforce condition](../../includes/connectors-create-api-salesforce-condition.md)]
> 
> 

## <a name="use-a-salesforce-connector-action"></a><span data-ttu-id="bdffe-116">A Salesforce-összekötő művelettel</span><span class="sxs-lookup"><span data-stu-id="bdffe-116">Use a Salesforce connector action</span></span>
<span data-ttu-id="bdffe-117">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="bdffe-117">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="bdffe-118">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="bdffe-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps to create a Salesforce action](../../includes/connectors-create-api-salesforce-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="bdffe-119">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="bdffe-119">Connector-specific details</span></span>

<span data-ttu-id="bdffe-120">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/salesforce/).</span><span class="sxs-lookup"><span data-stu-id="bdffe-120">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/salesforce/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="bdffe-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bdffe-121">Next steps</span></span>
[<span data-ttu-id="bdffe-122">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="bdffe-122">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

