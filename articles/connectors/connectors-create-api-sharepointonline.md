---
title: "aaaLearn hogyan toouse hello SharePoint Online-összekötőt a logic apps |} Microsoft Docs"
description: "Hozzon létre a logic apps hello SharePoint online-hoz csatlakozó toomange listák SharePoint-webhelyre."
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
ms.openlocfilehash: fc2f2745f0d174eae6165e84fd8b6656b0aac44b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sharepoint-online-connector"></a><span data-ttu-id="9fabd-103">SharePoint Online hello összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="9fabd-103">Get started with hello SharePoint Online connector</span></span>
<span data-ttu-id="9fabd-104">Hello SharePoint-listák SharePoint online-hoz csatlakozó toomanage használja.</span><span class="sxs-lookup"><span data-stu-id="9fabd-104">Use hello SharePoint Online connector toomanage SharePoint lists.</span></span>  

<span data-ttu-id="9fabd-105">toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9fabd-105">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="9fabd-106">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9fabd-106">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosharepoint-online"></a><span data-ttu-id="9fabd-107">TooSharePoint Online csatlakozás</span><span class="sxs-lookup"><span data-stu-id="9fabd-107">Connect tooSharePoint Online</span></span>
<span data-ttu-id="9fabd-108">A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9fabd-108">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="9fabd-109">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="9fabd-109">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-toosharepoint-online"></a><span data-ttu-id="9fabd-110">Egy kapcsolat tooSharePoint Online létrehozása</span><span class="sxs-lookup"><span data-stu-id="9fabd-110">Create a connection tooSharePoint Online</span></span>
> [!INCLUDE [Steps toocreate a connection tooSharePoint](../../includes/connectors-create-api-sharepointonline.md)]


## <a name="use-a-sharepoint-online-trigger"></a><span data-ttu-id="9fabd-111">SharePoint Online eseményindítók</span><span class="sxs-lookup"><span data-stu-id="9fabd-111">Use a SharePoint Online trigger</span></span>
<span data-ttu-id="9fabd-112">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="9fabd-112">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="9fabd-113">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9fabd-113">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]


## <a name="use-a-sharepoint-online-action"></a><span data-ttu-id="9fabd-114">A SharePoint Online művelettel</span><span class="sxs-lookup"><span data-stu-id="9fabd-114">Use a SharePoint Online action</span></span>
<span data-ttu-id="9fabd-115">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="9fabd-115">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="9fabd-116">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9fabd-116">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]


## <a name="connector-specific-details"></a><span data-ttu-id="9fabd-117">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="9fabd-117">Connector-specific details</span></span>

<span data-ttu-id="9fabd-118">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="9fabd-118">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sharepoint/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9fabd-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9fabd-119">Next Steps</span></span>
[<span data-ttu-id="9fabd-120">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9fabd-120">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

