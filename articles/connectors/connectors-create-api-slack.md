---
title: "az Azure logic Apps alkalmazásait a Slackhez összekötő aaaUse hello |} Microsoft Docs"
description: "Csatlakozás a logic apps a tooSlack"
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
ms.openlocfilehash: 6599d7b69d2147425c9fab978c5d0f93e5605f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-slack-connector"></a><span data-ttu-id="6d22d-103">Hello közzététele a Slack-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="6d22d-103">Get started with hello Slack connector</span></span>
<span data-ttu-id="6d22d-104">A Slack egy csoportos kommunikációs eszköz, amely a csoporton belüli összes kommunikációt egy helyre fogja össze. Ez a hely azonnal kereshető, és bárhonnan elérhető.</span><span class="sxs-lookup"><span data-stu-id="6d22d-104">Slack is a team communication tool, that brings together all of your team communications in one place, instantly searchable and available wherever you go.</span></span> 

<span data-ttu-id="6d22d-105">Első lépések egy logikai alkalmazás létrehozása most; Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="6d22d-105">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-tooslack"></a><span data-ttu-id="6d22d-106">Egy kapcsolat tooSlack létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d22d-106">Create a connection tooSlack</span></span>
<span data-ttu-id="6d22d-107">toouse hello Slack összekötő, először létre kell hoznia egy **kapcsolat** hello részletek adja meg ezeket a tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="6d22d-107">toouse hello Slack connector, you first create a **connection** then provide hello details for these properties:</span></span> 

| <span data-ttu-id="6d22d-108">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6d22d-108">Property</span></span> | <span data-ttu-id="6d22d-109">Szükséges</span><span class="sxs-lookup"><span data-stu-id="6d22d-109">Required</span></span> | <span data-ttu-id="6d22d-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="6d22d-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6d22d-111">Token</span><span class="sxs-lookup"><span data-stu-id="6d22d-111">Token</span></span> |<span data-ttu-id="6d22d-112">Igen</span><span class="sxs-lookup"><span data-stu-id="6d22d-112">Yes</span></span> |<span data-ttu-id="6d22d-113">Adja meg a Slack hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="6d22d-113">Provide Slack Credentials</span></span> |

<span data-ttu-id="6d22d-114">Hajtsa végre ezeket a lépéseket toosign Slackhez, és a teljes hello konfigurálása hello Slackhez **kapcsolat** a logikai alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="6d22d-114">Follow these steps toosign into Slack, and complete hello configuration of hello Slack **connection** in your logic app:</span></span>

1. <span data-ttu-id="6d22d-115">Válassza ki **ismétlődési**</span><span class="sxs-lookup"><span data-stu-id="6d22d-115">Select **Recurrence**</span></span>
2. <span data-ttu-id="6d22d-116">Válassza ki a **gyakoriság** , és írja be egy **időköz**</span><span class="sxs-lookup"><span data-stu-id="6d22d-116">Select a **Frequency** and enter an **Interval**</span></span>
3. <span data-ttu-id="6d22d-117">Válassza ki **művelet hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="6d22d-117">Select **Add an action**</span></span>  
   <span data-ttu-id="6d22d-118">![Konfigurálja a Slackhez][1]</span><span class="sxs-lookup"><span data-stu-id="6d22d-118">![Configure Slack][1]</span></span>  
4. <span data-ttu-id="6d22d-119">Hello keresőmezőbe írja be a Slackhez, majd várja meg a keresési tooreturn hello összes bejegyzést a Slackhez hello neve</span><span class="sxs-lookup"><span data-stu-id="6d22d-119">Enter Slack in hello search box and wait for hello search tooreturn all entries with Slack in hello name</span></span>
5. <span data-ttu-id="6d22d-120">Válassza ki **Slackhez - üzenet közzététele**</span><span class="sxs-lookup"><span data-stu-id="6d22d-120">Select **Slack - Post message**</span></span>
6. <span data-ttu-id="6d22d-121">Válassza ki **tooSlack bejelentkezés**:</span><span class="sxs-lookup"><span data-stu-id="6d22d-121">Select **Sign in tooSlack**:</span></span>  
   <span data-ttu-id="6d22d-122">![Konfigurálja a Slackhez][2]</span><span class="sxs-lookup"><span data-stu-id="6d22d-122">![Configure Slack][2]</span></span>
7. <span data-ttu-id="6d22d-123">Adja meg a Slack hitelesítő adatok toosign tooauthorize hello alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="6d22d-123">Provide your Slack credentials toosign in tooauthorize hello  application</span></span>    
   ![Konfigurálja a Slackhez][3]  
8. <span data-ttu-id="6d22d-125">Átirányított tooyour szervezete bejelentkezési oldalán lesz.</span><span class="sxs-lookup"><span data-stu-id="6d22d-125">You'll be redirected tooyour organization's Log in page.</span></span> <span data-ttu-id="6d22d-126">**Engedélyezi** az a logikai alkalmazás közzététele a Slack toointeract:</span><span class="sxs-lookup"><span data-stu-id="6d22d-126">**Authorize** Slack toointeract with your logic app:</span></span>      
   <span data-ttu-id="6d22d-127">![Konfigurálja a Slackhez][5]</span><span class="sxs-lookup"><span data-stu-id="6d22d-127">![Configure Slack][5]</span></span> 
9. <span data-ttu-id="6d22d-128">Hello engedélyezési befejezése után lesz átirányított tooyour logic app toocomplete azt hello konfigurálásával **Slack - összes üzenet** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6d22d-128">After hello authorization completes you'll be redirected tooyour logic app toocomplete it by configuring hello **Slack - Get all messages** section.</span></span> <span data-ttu-id="6d22d-129">Vegyen fel más eseményindítók és műveletek, amelyekre szüksége van.</span><span class="sxs-lookup"><span data-stu-id="6d22d-129">Add other triggers and actions that you need.</span></span>  
   <span data-ttu-id="6d22d-130">![Konfigurálja a Slackhez][6]</span><span class="sxs-lookup"><span data-stu-id="6d22d-130">![Configure Slack][6]</span></span>
10. <span data-ttu-id="6d22d-131">Mentse a munkáját kiválasztásával **mentése** fenti hello menüsávjában.</span><span class="sxs-lookup"><span data-stu-id="6d22d-131">Save your work by selecting **Save** on hello menu bar above.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="6d22d-132">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="6d22d-132">Connector-specific details</span></span>

<span data-ttu-id="6d22d-133">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/slack/).</span><span class="sxs-lookup"><span data-stu-id="6d22d-133">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/slack/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="6d22d-134">További összekötők</span><span class="sxs-lookup"><span data-stu-id="6d22d-134">More connectors</span></span>
<span data-ttu-id="6d22d-135">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="6d22d-135">Go back toohello [APIs list](apis-list.md).</span></span>

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
