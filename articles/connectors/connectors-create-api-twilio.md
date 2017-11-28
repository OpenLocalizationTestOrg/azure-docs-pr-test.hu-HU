---
title: "Adja hozzá a Twilio-összekötő az Azure Logic apps |} Microsoft Docs"
description: "A REST API-paraméterekkel rendelkező Twilio-összekötő áttekintése"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a790ac51b0fea7e3fa379d20e0e094e7ce0d7696
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-twilio-connector"></a><span data-ttu-id="84194-103">A Twilio-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="84194-103">Get started with the Twilio connector</span></span>
<span data-ttu-id="84194-104">A globális SMS, MMS és IP-üzeneteket küldjön és fogadjon Twilio csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="84194-104">Connect to Twilio to send and receive global SMS, MMS, and IP messages.</span></span> <span data-ttu-id="84194-105">A Twilio a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="84194-105">With Twilio, you can:</span></span>

* <span data-ttu-id="84194-106">Az üzleti folyamat Twilio származó adatok alapján történő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="84194-106">Build your business flow based on the data you get from Twilio.</span></span> 
* <span data-ttu-id="84194-107">Egy üzenet, a lista üzenetek és a további műveleteket használni.</span><span class="sxs-lookup"><span data-stu-id="84194-107">Use actions that get a message, list messages, and more.</span></span> <span data-ttu-id="84194-108">Ezeket a műveleteket válaszol, és végezze el a kimeneti más műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="84194-108">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="84194-109">Például egy új Twilio-üzenetet kap, ha akkor igénybe ezt az üzenetet, és a Service Bus munkafolyamat használatával.</span><span class="sxs-lookup"><span data-stu-id="84194-109">For example, when  you get a new Twilio message, you can take this message and use it a Service Bus workflow.</span></span> 

<span data-ttu-id="84194-110">Hozzon létre egy logic app; első lépései Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="84194-110">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-twilio"></a><span data-ttu-id="84194-111">Kapcsolatot létesíthet Twilio</span><span class="sxs-lookup"><span data-stu-id="84194-111">Create a connection to Twilio</span></span>
<span data-ttu-id="84194-112">Ezt az összekötőt a logic apps hozzáadásakor adja meg a következő Twilio-értékeket:</span><span class="sxs-lookup"><span data-stu-id="84194-112">When you add this Connector to your logic apps, enter the following Twilio values:</span></span>

| <span data-ttu-id="84194-113">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="84194-113">Property</span></span> | <span data-ttu-id="84194-114">Szükséges</span><span class="sxs-lookup"><span data-stu-id="84194-114">Required</span></span> | <span data-ttu-id="84194-115">Leírás</span><span class="sxs-lookup"><span data-stu-id="84194-115">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="84194-116">Futtatófiók-Azonosítóvá</span><span class="sxs-lookup"><span data-stu-id="84194-116">Account ID</span></span> |<span data-ttu-id="84194-117">Igen</span><span class="sxs-lookup"><span data-stu-id="84194-117">Yes</span></span> |<span data-ttu-id="84194-118">Írja be a Twilio-fiók Azonosítóját</span><span class="sxs-lookup"><span data-stu-id="84194-118">Enter your Twilio account ID</span></span> |
| <span data-ttu-id="84194-119">Hozzáférési jogkivonat</span><span class="sxs-lookup"><span data-stu-id="84194-119">Access Token</span></span> |<span data-ttu-id="84194-120">Igen</span><span class="sxs-lookup"><span data-stu-id="84194-120">Yes</span></span> |<span data-ttu-id="84194-121">Adja meg a Twilio-hozzáférési jogkivonat</span><span class="sxs-lookup"><span data-stu-id="84194-121">Enter your Twilio access token</span></span> |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

<span data-ttu-id="84194-122">Ha a Twilio-hozzáférési jogkivonat nem rendelkezik, tekintse meg a [felhasználói identitás- & hozzáférési jogkivonatok](https://www.twilio.com/docs/api/chat/guides/identity).</span><span class="sxs-lookup"><span data-stu-id="84194-122">If you don't have a Twilio access token, see [User Identity & Access Tokens](https://www.twilio.com/docs/api/chat/guides/identity).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="84194-123">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="84194-123">Connector-specific details</span></span>

<span data-ttu-id="84194-124">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/twilio/).</span><span class="sxs-lookup"><span data-stu-id="84194-124">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/twilio/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="84194-125">További összekötők</span><span class="sxs-lookup"><span data-stu-id="84194-125">More connectors</span></span>
<span data-ttu-id="84194-126">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="84194-126">Go back to the [APIs list](apis-list.md).</span></span>