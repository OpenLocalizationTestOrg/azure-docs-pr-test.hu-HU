---
title: "aaaAdd hello Twilio-összekötő az Azure Logic Apps alkalmazásait |} Microsoft Docs"
description: "Hello Twilio-összekötő REST API-paraméterekkel rendelkező áttekintése"
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
ms.openlocfilehash: b2b487f34bc76bee24b4237a71ee767d0d22ff7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twilio-connector"></a><span data-ttu-id="f88e6-103">Hello Twilio-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="f88e6-103">Get started with hello Twilio connector</span></span>
<span data-ttu-id="f88e6-104">Csatlakozás tooTwilio toosend, és globális SMS, MMS és IP-üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="f88e6-104">Connect tooTwilio toosend and receive global SMS, MMS, and IP messages.</span></span> <span data-ttu-id="f88e6-105">A Twilio a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="f88e6-105">With Twilio, you can:</span></span>

* <span data-ttu-id="f88e6-106">Hozhat létre. az üzleti folyamat kap Twilio hello adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="f88e6-106">Build your business flow based on hello data you get from Twilio.</span></span> 
* <span data-ttu-id="f88e6-107">Egy üzenet, a lista üzenetek és a további műveleteket használni.</span><span class="sxs-lookup"><span data-stu-id="f88e6-107">Use actions that get a message, list messages, and more.</span></span> <span data-ttu-id="f88e6-108">Ezeket a műveleteket válaszol, és végezze el hello kimeneti más műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="f88e6-108">These actions get a response, and then make hello output available for other actions.</span></span> <span data-ttu-id="f88e6-109">Például egy új Twilio-üzenetet kap, ha akkor igénybe ezt az üzenetet, és a Service Bus munkafolyamat használatával.</span><span class="sxs-lookup"><span data-stu-id="f88e6-109">For example, when  you get a new Twilio message, you can take this message and use it a Service Bus workflow.</span></span> 

<span data-ttu-id="f88e6-110">Hozzon létre egy logic app; első lépései Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="f88e6-110">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-tootwilio"></a><span data-ttu-id="f88e6-111">Egy kapcsolat tooTwilio létrehozása</span><span class="sxs-lookup"><span data-stu-id="f88e6-111">Create a connection tooTwilio</span></span>
<span data-ttu-id="f88e6-112">Az összekötő tooyour a logic apps hozzáadásakor adja meg a Twilio-értékeket a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f88e6-112">When you add this Connector tooyour logic apps, enter hello following Twilio values:</span></span>

| <span data-ttu-id="f88e6-113">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f88e6-113">Property</span></span> | <span data-ttu-id="f88e6-114">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f88e6-114">Required</span></span> | <span data-ttu-id="f88e6-115">Leírás</span><span class="sxs-lookup"><span data-stu-id="f88e6-115">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f88e6-116">Futtatófiók-Azonosítóvá</span><span class="sxs-lookup"><span data-stu-id="f88e6-116">Account ID</span></span> |<span data-ttu-id="f88e6-117">Igen</span><span class="sxs-lookup"><span data-stu-id="f88e6-117">Yes</span></span> |<span data-ttu-id="f88e6-118">Írja be a Twilio-fiók Azonosítóját</span><span class="sxs-lookup"><span data-stu-id="f88e6-118">Enter your Twilio account ID</span></span> |
| <span data-ttu-id="f88e6-119">Hozzáférési jogkivonat</span><span class="sxs-lookup"><span data-stu-id="f88e6-119">Access Token</span></span> |<span data-ttu-id="f88e6-120">Igen</span><span class="sxs-lookup"><span data-stu-id="f88e6-120">Yes</span></span> |<span data-ttu-id="f88e6-121">Adja meg a Twilio-hozzáférési jogkivonat</span><span class="sxs-lookup"><span data-stu-id="f88e6-121">Enter your Twilio access token</span></span> |

> [!INCLUDE [Steps toocreate a connection tooTwilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

<span data-ttu-id="f88e6-122">Ha a Twilio-hozzáférési jogkivonat nem rendelkezik, tekintse meg a [felhasználói identitás- & hozzáférési jogkivonatok](https://www.twilio.com/docs/api/chat/guides/identity).</span><span class="sxs-lookup"><span data-stu-id="f88e6-122">If you don't have a Twilio access token, see [User Identity & Access Tokens](https://www.twilio.com/docs/api/chat/guides/identity).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="f88e6-123">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="f88e6-123">Connector-specific details</span></span>

<span data-ttu-id="f88e6-124">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/twilio/).</span><span class="sxs-lookup"><span data-stu-id="f88e6-124">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/twilio/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="f88e6-125">További összekötők</span><span class="sxs-lookup"><span data-stu-id="f88e6-125">More connectors</span></span>
<span data-ttu-id="f88e6-126">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="f88e6-126">Go back toohello [APIs list](apis-list.md).</span></span>
