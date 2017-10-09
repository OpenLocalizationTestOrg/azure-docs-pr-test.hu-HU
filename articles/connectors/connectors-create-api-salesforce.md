---
title: "aaaLearn toouse hello Salesforce-összekötőt a logic Apps alkalmazásait |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása hello Salesforce összekötő biztosít egy API toowork Salesforce objektummal."
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
ms.openlocfilehash: b14b41fa8a4648b4f0090472dc0f9575bf13a2ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-salesforce-connector"></a><span data-ttu-id="eb08e-104">Hello Salesforce összekötőjével első lépései</span><span class="sxs-lookup"><span data-stu-id="eb08e-104">Get started with hello Salesforce connector</span></span>
<span data-ttu-id="eb08e-105">hello Salesforce összekötő biztosít egy API toowork Salesforce objektummal.</span><span class="sxs-lookup"><span data-stu-id="eb08e-105">hello Salesforce Connector provides an API toowork with Salesforce objects.</span></span>

<span data-ttu-id="eb08e-106">toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="eb08e-106">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="eb08e-107">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="eb08e-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosalesforce-connector"></a><span data-ttu-id="eb08e-108">Csatlakozás tooSalesforce összekötő</span><span class="sxs-lookup"><span data-stu-id="eb08e-108">Connect tooSalesforce connector</span></span>
<span data-ttu-id="eb08e-109">A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="eb08e-109">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="eb08e-110">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="eb08e-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-toosalesforce-connector"></a><span data-ttu-id="eb08e-111">Kapcsolat tooSalesforce összekötő létrehozása</span><span class="sxs-lookup"><span data-stu-id="eb08e-111">Create a connection tooSalesforce connector</span></span>
> [!INCLUDE [Steps toocreate a connection tooSalesforce Connector](../../includes/connectors-create-api-salesforce.md)]
> 
> 

## <a name="use-a-salesforce-connector-trigger"></a><span data-ttu-id="eb08e-112">Salesforce összekötő eseményindítók</span><span class="sxs-lookup"><span data-stu-id="eb08e-112">Use a Salesforce connector trigger</span></span>
<span data-ttu-id="eb08e-113">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="eb08e-113">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="eb08e-114">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="eb08e-114">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps toocreate a Salesforce trigger](../../includes/connectors-create-api-salesforce-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="eb08e-115">Feltétel hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eb08e-115">Add a condition</span></span>
> [!INCLUDE [Steps toocreate a Salesforce condition](../../includes/connectors-create-api-salesforce-condition.md)]
> 
> 

## <a name="use-a-salesforce-connector-action"></a><span data-ttu-id="eb08e-116">A Salesforce-összekötő művelettel</span><span class="sxs-lookup"><span data-stu-id="eb08e-116">Use a Salesforce connector action</span></span>
<span data-ttu-id="eb08e-117">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="eb08e-117">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="eb08e-118">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="eb08e-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps toocreate a Salesforce action](../../includes/connectors-create-api-salesforce-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="eb08e-119">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="eb08e-119">Connector-specific details</span></span>

<span data-ttu-id="eb08e-120">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/salesforce/).</span><span class="sxs-lookup"><span data-stu-id="eb08e-120">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/salesforce/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="eb08e-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eb08e-121">Next steps</span></span>
[<span data-ttu-id="eb08e-122">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="eb08e-122">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

