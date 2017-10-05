---
title: SendGrid |} Microsoft Docs
description: "Az Azure App service Logic Apps alkalmazások létrehozása SendGrid Kapcsolatszolgáltatója lehetővé teszi, hogy küldjenek e-maileket, és kezelheti a címzettjeinek listája."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: bc4f1fc2-824c-4ed7-8de8-e82baff3b746
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 9ff0591741899d65b8274fb14ab3f3c8db9abe36
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sendgrid-connector"></a><span data-ttu-id="80fae-104">A SendGrid szolgáltatás összekötőjével az első lépései</span><span class="sxs-lookup"><span data-stu-id="80fae-104">Get started with the SendGrid connector</span></span>
<span data-ttu-id="80fae-105">SendGrid Kapcsolatszolgáltatója lehetővé teszi, hogy küldjenek e-maileket, és kezelheti a címzettjeinek listája.</span><span class="sxs-lookup"><span data-stu-id="80fae-105">SendGrid Connection Provider lets you send email and manage recipient lists.</span></span>

<span data-ttu-id="80fae-106">Most hozzon létre egy Logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="80fae-106">You can get started by creating a Logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-sendgrid"></a><span data-ttu-id="80fae-107">Kapcsolatot létesíthet a SendGrid</span><span class="sxs-lookup"><span data-stu-id="80fae-107">Create a connection to SendGrid</span></span>
<span data-ttu-id="80fae-108">A Logic apps sendgrid létrehozásához, létre kell hoznia egy **kapcsolat** adja meg a részleteket a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="80fae-108">To create Logic apps with SendGrid, you must first create a **connection** then provide the details for the following properties:</span></span> 

| <span data-ttu-id="80fae-109">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="80fae-109">Property</span></span> | <span data-ttu-id="80fae-110">Szükséges</span><span class="sxs-lookup"><span data-stu-id="80fae-110">Required</span></span> | <span data-ttu-id="80fae-111">Leírás</span><span class="sxs-lookup"><span data-stu-id="80fae-111">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="80fae-112">apiKey</span><span class="sxs-lookup"><span data-stu-id="80fae-112">ApiKey</span></span> |<span data-ttu-id="80fae-113">Igen</span><span class="sxs-lookup"><span data-stu-id="80fae-113">Yes</span></span> |<span data-ttu-id="80fae-114">Adja meg a SendGrid Api-kulcsát</span><span class="sxs-lookup"><span data-stu-id="80fae-114">Provide Your SendGrid Api Key</span></span> |

> [!INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]
> 


<span data-ttu-id="80fae-115">Miután létrehozta a kapcsolatot, a műveletek végrehajtásához, és figyeljen az eseményindítók a használhatja.</span><span class="sxs-lookup"><span data-stu-id="80fae-115">After you create the connection, you can use it to execute the actions and listen for the triggers.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="80fae-116">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="80fae-116">Connector-specific details</span></span>

<span data-ttu-id="80fae-117">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/sendgrid/).</span><span class="sxs-lookup"><span data-stu-id="80fae-117">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sendgrid/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="80fae-118">További összekötők</span><span class="sxs-lookup"><span data-stu-id="80fae-118">More connectors</span></span>
<span data-ttu-id="80fae-119">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="80fae-119">Go back to the [APIs list](apis-list.md).</span></span>