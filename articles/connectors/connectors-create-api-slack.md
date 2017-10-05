---
title: "A Slackhez Connector használata az Azure logic Apps alkalmazásait |} Microsoft Docs"
description: "A logic apps a csatlakozás a Slackhez"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: fc5fc128efe01bd0727e3ff30d8938918e89ac3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-slack-connector"></a><span data-ttu-id="be0b5-103">A Slack-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="be0b5-103">Get started with the Slack connector</span></span>
<span data-ttu-id="be0b5-104">A Slack egy csoportos kommunikációs eszköz, amely a csoporton belüli összes kommunikációt egy helyre fogja össze. Ez a hely azonnal kereshető, és bárhonnan elérhető.</span><span class="sxs-lookup"><span data-stu-id="be0b5-104">Slack is a team communication tool, that brings together all of your team communications in one place, instantly searchable and available wherever you go.</span></span> 

<span data-ttu-id="be0b5-105">Első lépések egy logikai alkalmazás létrehozása most; Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="be0b5-105">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-slack"></a><span data-ttu-id="be0b5-106">Kapcsolatot létesíthet a Slackhez</span><span class="sxs-lookup"><span data-stu-id="be0b5-106">Create a connection to Slack</span></span>
<span data-ttu-id="be0b5-107">A Slack-összekötő használatához először létre kell hoznia egy **kapcsolat** adja meg a részleteket a tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="be0b5-107">To use the Slack connector, you first create a **connection** then provide the details for these properties:</span></span> 

| <span data-ttu-id="be0b5-108">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="be0b5-108">Property</span></span> | <span data-ttu-id="be0b5-109">Szükséges</span><span class="sxs-lookup"><span data-stu-id="be0b5-109">Required</span></span> | <span data-ttu-id="be0b5-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="be0b5-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="be0b5-111">Token</span><span class="sxs-lookup"><span data-stu-id="be0b5-111">Token</span></span> |<span data-ttu-id="be0b5-112">Igen</span><span class="sxs-lookup"><span data-stu-id="be0b5-112">Yes</span></span> |<span data-ttu-id="be0b5-113">Adja meg a Slack hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="be0b5-113">Provide Slack Credentials</span></span> |

<span data-ttu-id="be0b5-114">Kövesse az alábbi lépéseket követve jelentkezzen be a Slackhez, és konfigurációjához a Slackhez, **kapcsolat** a logikai alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="be0b5-114">Follow these steps to sign into Slack, and complete the configuration of the Slack **connection** in your logic app:</span></span>

1. <span data-ttu-id="be0b5-115">Válassza ki **ismétlődési**</span><span class="sxs-lookup"><span data-stu-id="be0b5-115">Select **Recurrence**</span></span>
2. <span data-ttu-id="be0b5-116">Válassza ki a **gyakoriság** , és írja be egy **időköz**</span><span class="sxs-lookup"><span data-stu-id="be0b5-116">Select a **Frequency** and enter an **Interval**</span></span>
3. <span data-ttu-id="be0b5-117">Válassza ki **művelet hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="be0b5-117">Select **Add an action**</span></span>  
   <span data-ttu-id="be0b5-118">![Konfigurálja a Slackhez][1]</span><span class="sxs-lookup"><span data-stu-id="be0b5-118">![Configure Slack][1]</span></span>  
4. <span data-ttu-id="be0b5-119">A keresőmezőbe írja be a Slackhez, és várja meg, a Keresés a Slackhez összes bejegyzés a nevét adja vissza</span><span class="sxs-lookup"><span data-stu-id="be0b5-119">Enter Slack in the search box and wait for the search to return all entries with Slack in the name</span></span>
5. <span data-ttu-id="be0b5-120">Válassza ki **Slackhez - üzenet közzététele**</span><span class="sxs-lookup"><span data-stu-id="be0b5-120">Select **Slack - Post message**</span></span>
6. <span data-ttu-id="be0b5-121">Válassza ki **jelentkezzen be a Slackhez**:</span><span class="sxs-lookup"><span data-stu-id="be0b5-121">Select **Sign in to Slack**:</span></span>  
   <span data-ttu-id="be0b5-122">![Konfigurálja a Slackhez][2]</span><span class="sxs-lookup"><span data-stu-id="be0b5-122">![Configure Slack][2]</span></span>
7. <span data-ttu-id="be0b5-123">Jelentkezzen be az alkalmazás a Slack hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="be0b5-123">Provide your Slack credentials to sign in to authorize the  application</span></span>    
   ![Konfigurálja a Slackhez][3]  
8. <span data-ttu-id="be0b5-125">Meg kell átirányítani a szervezet bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="be0b5-125">You'll be redirected to your organization's Log in page.</span></span> <span data-ttu-id="be0b5-126">**Engedélyezi** Slackhez, amellyel kommunikálni tud a Logic Apps alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="be0b5-126">**Authorize** Slack to interact with your logic app:</span></span>      
   <span data-ttu-id="be0b5-127">![Konfigurálja a Slackhez][5]</span><span class="sxs-lookup"><span data-stu-id="be0b5-127">![Configure Slack][5]</span></span> 
9. <span data-ttu-id="be0b5-128">Az engedélyezés befejezése után meg fogja átirányítani a Logic Apps alkalmazást befejezéseként konfigurálja a **Slack - összes üzenet** szakasz.</span><span class="sxs-lookup"><span data-stu-id="be0b5-128">After the authorization completes you'll be redirected to your logic app to complete it by configuring the **Slack - Get all messages** section.</span></span> <span data-ttu-id="be0b5-129">Vegyen fel más eseményindítók és műveletek, amelyekre szüksége van.</span><span class="sxs-lookup"><span data-stu-id="be0b5-129">Add other triggers and actions that you need.</span></span>  
   <span data-ttu-id="be0b5-130">![Konfigurálja a Slackhez][6]</span><span class="sxs-lookup"><span data-stu-id="be0b5-130">![Configure Slack][6]</span></span>
10. <span data-ttu-id="be0b5-131">Mentse a munkáját kiválasztásával **mentése** fenti menüsávjában.</span><span class="sxs-lookup"><span data-stu-id="be0b5-131">Save your work by selecting **Save** on the menu bar above.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="be0b5-132">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="be0b5-132">Connector-specific details</span></span>

<span data-ttu-id="be0b5-133">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/slack/).</span><span class="sxs-lookup"><span data-stu-id="be0b5-133">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/slack/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="be0b5-134">További összekötők</span><span class="sxs-lookup"><span data-stu-id="be0b5-134">More connectors</span></span>
<span data-ttu-id="be0b5-135">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="be0b5-135">Go back to the [APIs list](apis-list.md).</span></span>

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
