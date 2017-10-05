---
title: "A SharePoint Online összekötőt használata a logic apps |} Microsoft Docs"
description: "Hozzon létre a logic apps alkalmazást a SharePoint Online Manage listák a SharePoint-összekötővel."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: e0ec3149-507a-409d-8e7b-d5fbded006ce
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 96fc1347604c0c6cc2c2463a5dbd83b560183a16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sharepoint-online-connector"></a><span data-ttu-id="f29f8-103">A SharePoint Online összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="f29f8-103">Get started with the SharePoint Online connector</span></span>
<span data-ttu-id="f29f8-104">A SharePoint Online összekötő segítségével kezelheti a SharePoint-listát.</span><span class="sxs-lookup"><span data-stu-id="f29f8-104">Use the SharePoint Online connector to manage SharePoint lists.</span></span>  

<span data-ttu-id="f29f8-105">Használandó [a csatlakozókat](apis-list.md), először hozzon létre egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f29f8-105">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="f29f8-106">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="f29f8-106">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-sharepoint-online"></a><span data-ttu-id="f29f8-107">Csatlakozás a SharePoint online szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="f29f8-107">Connect to SharePoint Online</span></span>
<span data-ttu-id="f29f8-108">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először hozzon létre egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f29f8-108">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="f29f8-109">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="f29f8-109">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-sharepoint-online"></a><span data-ttu-id="f29f8-110">Kapcsolatot létesíthet a SharePoint online-hoz</span><span class="sxs-lookup"><span data-stu-id="f29f8-110">Create a connection to SharePoint Online</span></span>
> [!INCLUDE [Steps to create a connection to SharePoint](../../includes/connectors-create-api-sharepointonline.md)]


## <a name="use-a-sharepoint-online-trigger"></a><span data-ttu-id="f29f8-111">SharePoint Online eseményindítók</span><span class="sxs-lookup"><span data-stu-id="f29f8-111">Use a SharePoint Online trigger</span></span>
<span data-ttu-id="f29f8-112">Egy eseményindító nem egy eseményt, a logikai alkalmazás definiált munkafolyamat indításához használható.</span><span class="sxs-lookup"><span data-stu-id="f29f8-112">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="f29f8-113">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="f29f8-113">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]


## <a name="use-a-sharepoint-online-action"></a><span data-ttu-id="f29f8-114">A SharePoint Online művelettel</span><span class="sxs-lookup"><span data-stu-id="f29f8-114">Use a SharePoint Online action</span></span>
<span data-ttu-id="f29f8-115">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="f29f8-115">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="f29f8-116">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="f29f8-116">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]


## <a name="connector-specific-details"></a><span data-ttu-id="f29f8-117">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="f29f8-117">Connector-specific details</span></span>

<span data-ttu-id="f29f8-118">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="f29f8-118">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sharepoint/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f29f8-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f29f8-119">Next Steps</span></span>
[<span data-ttu-id="f29f8-120">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f29f8-120">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

