---
title: "az Azure Logic Apps-Yammer-összekötőjét aaaAdd hello |} Microsoft Docs"
description: "Hello Yammer-összekötő REST API-paraméterekkel rendelkező áttekintése"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5ae0827-fbb3-45ec-8f45-ad1cc2e7eccc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: be75df770a8062d839926dff8c0195d2647f78d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-yammer-connector"></a><span data-ttu-id="fc638-103">Hello Yammer-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="fc638-103">Get started with hello Yammer connector</span></span>
<span data-ttu-id="fc638-104">Csatlakozás tooYammer tooaccess beszélgetések a vállalati hálózaton.</span><span class="sxs-lookup"><span data-stu-id="fc638-104">Connect tooYammer tooaccess conversations in your enterprise network.</span></span> <span data-ttu-id="fc638-105">A Yammer a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="fc638-105">With Yammer, you can:</span></span>

* <span data-ttu-id="fc638-106">Hozhat létre. az üzleti folyamat Yammer kapott hello adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="fc638-106">Build your business flow based on hello data you get from Yammer.</span></span> 
* <span data-ttu-id="fc638-107">Használja a váltja ki, ha van egy új üzenet, vagy adatcsatornára a következőket.</span><span class="sxs-lookup"><span data-stu-id="fc638-107">Use triggers for when there is a new message in a group, or a feed your following.</span></span>
* <span data-ttu-id="fc638-108">Műveletek toopost üzenet használatához az összes üzenet, és több.</span><span class="sxs-lookup"><span data-stu-id="fc638-108">Use actions toopost a message, get all messages, and more.</span></span> <span data-ttu-id="fc638-109">Ezeket a műveleteket válaszol, és végezze el hello kimeneti más műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="fc638-109">These actions get a response, and then make hello output available for other actions.</span></span> <span data-ttu-id="fc638-110">Például egy új üzenet jelenik meg, ha az Office 365-tel e-mailt küldhet.</span><span class="sxs-lookup"><span data-stu-id="fc638-110">For example, when a new message appears, you can send an email using Office 365.</span></span>

<span data-ttu-id="fc638-111">Első lépések egy logikai alkalmazás létrehozása most; Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="fc638-111">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-tooyammer"></a><span data-ttu-id="fc638-112">Egy kapcsolat tooYammer létrehozása</span><span class="sxs-lookup"><span data-stu-id="fc638-112">Create a connection tooYammer</span></span>
<span data-ttu-id="fc638-113">toouse hello Yammer összekötő, először létre kell hoznia egy **kapcsolat** hello részletek adja meg ezeket a tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="fc638-113">toouse hello Yammer connector, you first create a **connection** then provide hello details for these properties:</span></span> 

| <span data-ttu-id="fc638-114">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="fc638-114">Property</span></span> | <span data-ttu-id="fc638-115">Szükséges</span><span class="sxs-lookup"><span data-stu-id="fc638-115">Required</span></span> | <span data-ttu-id="fc638-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="fc638-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fc638-117">Token</span><span class="sxs-lookup"><span data-stu-id="fc638-117">Token</span></span> |<span data-ttu-id="fc638-118">Igen</span><span class="sxs-lookup"><span data-stu-id="fc638-118">Yes</span></span> |<span data-ttu-id="fc638-119">Yammer hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="fc638-119">Provide Yammer Credentials</span></span> |

> [!INCLUDE [Steps toocreate a connection tooYammer](../../includes/connectors-create-api-yammer.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="fc638-120">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="fc638-120">Connector-specific details</span></span>

<span data-ttu-id="fc638-121">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/yammer/).</span><span class="sxs-lookup"><span data-stu-id="fc638-121">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/yammer/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="fc638-122">További összekötők</span><span class="sxs-lookup"><span data-stu-id="fc638-122">More connectors</span></span>
<span data-ttu-id="fc638-123">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="fc638-123">Go back toohello [APIs list](apis-list.md).</span></span>
