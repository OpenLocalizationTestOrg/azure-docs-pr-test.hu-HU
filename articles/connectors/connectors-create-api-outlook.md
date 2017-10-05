---
title: "Az Azure Logic Apps Outlook.com-os összekötő |} Microsoft Docs"
description: "Az Azure App service Logic Apps alkalmazások létrehozása Outlook.com-os összekötő lehetővé teszi a levelezés, a naptárak és a partnerek kezelését. Például a levél küldése különféle műveleteket hajtson végre, értekezletek ütemezését, adja hozzá az ügyfelek, stb."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 87113c85-d158-4dd5-9ed5-5748130003d6
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: bde1629504c97cf6706b42219570ffa6243073dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-outlookcom-connector"></a><span data-ttu-id="b10d4-105">Az Outlook.com-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="b10d4-105">Get started with the Outlook.com connector</span></span>
<span data-ttu-id="b10d4-106">Outlook.com-os összekötő lehetővé teszi a levelezés, a naptárak és a partnerek kezelését.</span><span class="sxs-lookup"><span data-stu-id="b10d4-106">Outlook.com connector allows you to manage your mail, calendars, and contacts.</span></span> <span data-ttu-id="b10d4-107">Például a levél küldése különféle műveleteket hajtson végre, értekezletek ütemezését, adja hozzá az ügyfelek, stb.</span><span class="sxs-lookup"><span data-stu-id="b10d4-107">You can perform various actions such as send mail, schedule meetings, add contacts, etc.</span></span>

<span data-ttu-id="b10d4-108">Most hozzon létre egy Logic app kezdheti, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b10d4-108">You can get started by creating a Logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-outlookcom"></a><span data-ttu-id="b10d4-109">Kapcsolatot létesíthet Outlook.com-os</span><span class="sxs-lookup"><span data-stu-id="b10d4-109">Create a connection to Outlook.com</span></span>
<span data-ttu-id="b10d4-110">A Logic apps az Outlook.com-on, akkor először hozzon létre egy **kapcsolat** adja meg a részleteket a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="b10d4-110">To create Logic apps with Outlook.com, you must first create a **connection** then provide the details for the following properties:</span></span>

| <span data-ttu-id="b10d4-111">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b10d4-111">Property</span></span> | <span data-ttu-id="b10d4-112">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b10d4-112">Required</span></span> | <span data-ttu-id="b10d4-113">Leírás</span><span class="sxs-lookup"><span data-stu-id="b10d4-113">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b10d4-114">Token</span><span class="sxs-lookup"><span data-stu-id="b10d4-114">Token</span></span> |<span data-ttu-id="b10d4-115">Igen</span><span class="sxs-lookup"><span data-stu-id="b10d4-115">Yes</span></span> |<span data-ttu-id="b10d4-116">Outlook.com-os hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="b10d4-116">Provide Outlook.com Credentials</span></span> |

<span data-ttu-id="b10d4-117">Miután létrehozta a kapcsolatot, használhatja a műveletek végrehajtása és a jelen cikkben ismertetett eseményindítók figyelni.</span><span class="sxs-lookup"><span data-stu-id="b10d4-117">After you create the connection, you can use it to execute the actions and listen for the triggers described in this article.</span></span>

> [!INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)]
>

## <a name="connector-specific-details"></a><span data-ttu-id="b10d4-118">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="b10d4-118">Connector-specific details</span></span>

<span data-ttu-id="b10d4-119">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/outlook/).</span><span class="sxs-lookup"><span data-stu-id="b10d4-119">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/outlook/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="b10d4-120">További összekötők</span><span class="sxs-lookup"><span data-stu-id="b10d4-120">More connectors</span></span>
<span data-ttu-id="b10d4-121">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="b10d4-121">Go back to the [APIs list](apis-list.md).</span></span>