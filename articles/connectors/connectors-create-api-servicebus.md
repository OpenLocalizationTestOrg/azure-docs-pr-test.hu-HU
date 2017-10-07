---
title: "aaaLearn toouse hello Azure Service Bus-összekötőt a logic Apps alkalmazásait |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása Csatlakozás a Service Bus toosend tooAzure, és üzeneteket fogadni. Például a küldési tooqueue műveleteket, tootopic küldése, várólista fogadása és előfizetés fogadása."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c815ac167c3106ade470ce139d119085558a9497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-azure-service-bus-connector"></a><span data-ttu-id="2f845-105">Ismerkedés az Azure Service Bus-összekötő hello</span><span class="sxs-lookup"><span data-stu-id="2f845-105">Get started with hello Azure Service Bus connector</span></span>
<span data-ttu-id="2f845-106">Csatlakozás a Service Bus toosend tooAzure, és üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="2f845-106">Connect tooAzure Service Bus toosend and receive messages.</span></span> <span data-ttu-id="2f845-107">Például a küldési tooqueue műveleteket, tootopic küldése, várólista fogadása és előfizetés fogadása.</span><span class="sxs-lookup"><span data-stu-id="2f845-107">You can perform actions such as send tooqueue, send tootopic, receive from queue, and receive from subscription.</span></span>

<span data-ttu-id="2f845-108">toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2f845-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="2f845-109">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="2f845-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooservice-bus"></a><span data-ttu-id="2f845-110">Csatlakozás tooService busz</span><span class="sxs-lookup"><span data-stu-id="2f845-110">Connect tooService Bus</span></span>
<span data-ttu-id="2f845-111">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először toocreate kapcsolat toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2f845-111">Before your logic app can access any service, you first need toocreate a connection toohello service.</span></span> <span data-ttu-id="2f845-112">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="2f845-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

> [!INCLUDE [Steps toocreate a connection tooAzure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a><span data-ttu-id="2f845-113">A Service Bus eseményindítók</span><span class="sxs-lookup"><span data-stu-id="2f845-113">Use a Service Bus trigger</span></span>
<span data-ttu-id="2f845-114">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="2f845-114">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="2f845-115">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="2f845-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a><span data-ttu-id="2f845-116">A Service Bus művelettel</span><span class="sxs-lookup"><span data-stu-id="2f845-116">Use a Service Bus action</span></span>
<span data-ttu-id="2f845-117">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="2f845-117">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="2f845-118">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="2f845-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

[!INCLUDE [Steps toocreate a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a><span data-ttu-id="2f845-119">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="2f845-119">Connector-specific details</span></span>

<span data-ttu-id="2f845-120">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/servicebus/).</span><span class="sxs-lookup"><span data-stu-id="2f845-120">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/servicebus/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2f845-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2f845-121">Next steps</span></span>
<span data-ttu-id="2f845-122">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="2f845-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

