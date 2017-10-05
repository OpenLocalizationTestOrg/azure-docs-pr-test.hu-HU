---
title: "Adja hozzá a Yammer-összekötő az Azure Logic Apps |} Microsoft Docs"
description: "A REST API-paraméterekkel rendelkező Yammer-összekötő áttekintése"
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
ms.openlocfilehash: c7a213343b4fb2b5a89a5052a459061b404a431c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-yammer-connector"></a><span data-ttu-id="34da1-103">A Yammer-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="34da1-103">Get started with the Yammer connector</span></span>
<span data-ttu-id="34da1-104">A Yammer csatlakozni hozzáférés beszélgetések a vállalati hálózaton.</span><span class="sxs-lookup"><span data-stu-id="34da1-104">Connect to Yammer to access conversations in your enterprise network.</span></span> <span data-ttu-id="34da1-105">A Yammer a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="34da1-105">With Yammer, you can:</span></span>

* <span data-ttu-id="34da1-106">Az üzleti folyamat Yammer származó adatok alapján történő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="34da1-106">Build your business flow based on the data you get from Yammer.</span></span> 
* <span data-ttu-id="34da1-107">Használja a váltja ki, ha van egy új üzenet, vagy adatcsatornára a következőket.</span><span class="sxs-lookup"><span data-stu-id="34da1-107">Use triggers for when there is a new message in a group, or a feed your following.</span></span>
* <span data-ttu-id="34da1-108">Műveletek segítségével üzenetet, az összes üzenetet, és több.</span><span class="sxs-lookup"><span data-stu-id="34da1-108">Use actions to post a message, get all messages, and more.</span></span> <span data-ttu-id="34da1-109">Ezeket a műveleteket válaszol, és végezze el a kimeneti más műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="34da1-109">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="34da1-110">Például egy új üzenet jelenik meg, ha az Office 365-tel e-mailt küldhet.</span><span class="sxs-lookup"><span data-stu-id="34da1-110">For example, when a new message appears, you can send an email using Office 365.</span></span>

<span data-ttu-id="34da1-111">Első lépések egy logikai alkalmazás létrehozása most; Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="34da1-111">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-yammer"></a><span data-ttu-id="34da1-112">Kapcsolatot létesíthet Yammer</span><span class="sxs-lookup"><span data-stu-id="34da1-112">Create a connection to Yammer</span></span>
<span data-ttu-id="34da1-113">A Yammer-összekötő használatához először létre kell hoznia egy **kapcsolat** adja meg a részleteket a tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="34da1-113">To use the Yammer connector, you first create a **connection** then provide the details for these properties:</span></span> 

| <span data-ttu-id="34da1-114">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="34da1-114">Property</span></span> | <span data-ttu-id="34da1-115">Szükséges</span><span class="sxs-lookup"><span data-stu-id="34da1-115">Required</span></span> | <span data-ttu-id="34da1-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="34da1-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="34da1-117">Token</span><span class="sxs-lookup"><span data-stu-id="34da1-117">Token</span></span> |<span data-ttu-id="34da1-118">Igen</span><span class="sxs-lookup"><span data-stu-id="34da1-118">Yes</span></span> |<span data-ttu-id="34da1-119">Yammer hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="34da1-119">Provide Yammer Credentials</span></span> |

> [!INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="34da1-120">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="34da1-120">Connector-specific details</span></span>

<span data-ttu-id="34da1-121">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/yammer/).</span><span class="sxs-lookup"><span data-stu-id="34da1-121">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/yammer/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="34da1-122">További összekötők</span><span class="sxs-lookup"><span data-stu-id="34da1-122">More connectors</span></span>
<span data-ttu-id="34da1-123">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="34da1-123">Go back to the [APIs list](apis-list.md).</span></span>