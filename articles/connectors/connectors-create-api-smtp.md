---
title: "Az Azure Logic Apps összekötőt |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása Az SMTP protokoll e-mailek továbbítására szolgál."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1cf96bbf8bd215d7ddb3c99860a5cb4e668be3c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-smtp-connector"></a><span data-ttu-id="f173a-104">Az SMTP-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="f173a-104">Get started with the SMTP connector</span></span>
<span data-ttu-id="f173a-105">Az SMTP protokoll e-mailek továbbítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="f173a-105">Connect to SMTP to send email.</span></span>

<span data-ttu-id="f173a-106">Használandó [a csatlakozókat](apis-list.md), először hozzon létre egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f173a-106">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="f173a-107">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="f173a-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-smtp"></a><span data-ttu-id="f173a-108">SMTP kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="f173a-108">Connect to SMTP</span></span>
<span data-ttu-id="f173a-109">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először hozzon létre egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f173a-109">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="f173a-110">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="f173a-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="f173a-111">Például szeretne csatlakozni az SMTP, először egy SMTP *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="f173a-111">For example, to connect to SMTP, you first need an SMTP *connection*.</span></span> <span data-ttu-id="f173a-112">VPN-kapcsolat létrehozásához adja meg a hitelesítő adatok általában segítségével éri el a szolgáltatást, hogy csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="f173a-112">To create a connection, enter the credentials you normally use to access the service you connect to.</span></span> <span data-ttu-id="f173a-113">Igen a SMTP példában adja meg a hitelesítő adatokat a kapcsolat neve, az SMTP-kiszolgáló címére és a felhasználói bejelentkezésekre vonatkozó információit az SMTP-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f173a-113">So, in the SMTP example, enter the credentials to your connection name, SMTP server address, and user login information to create the connection to SMTP.</span></span>  

### <a name="create-a-connection-to-smtp"></a><span data-ttu-id="f173a-114">Kapcsolatot létesíthet SMTP</span><span class="sxs-lookup"><span data-stu-id="f173a-114">Create a connection to SMTP</span></span>
> [!INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a><span data-ttu-id="f173a-115">Az SMTP-eseményindító használata</span><span class="sxs-lookup"><span data-stu-id="f173a-115">Use an SMTP trigger</span></span>
<span data-ttu-id="f173a-116">Egy eseményindító nem egy eseményt, a logikai alkalmazás definiált munkafolyamat indításához használható.</span><span class="sxs-lookup"><span data-stu-id="f173a-116">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="f173a-117">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="f173a-117">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="f173a-118">Ebben a példában, mert SMTP nem rendelkezik saját, eseményindító fogjuk használni a **Salesforce - amikor létrejön egy objektum** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="f173a-118">In this example, because SMTP does not have a trigger of its own, we'll use the **Salesforce - When an object is created** trigger.</span></span> <span data-ttu-id="f173a-119">Ehhez az eseményindítóhoz akkor aktiválódik, amikor új objektumot hoz létre a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="f173a-119">This trigger activates when a new object is created in Salesforce.</span></span> <span data-ttu-id="f173a-120">A példa kedvéért be azt, hogy minden alkalommal új vezető jön létre a Salesforce, egy *e-mailek küldése* , egy értesítés, amely a létrehozandó új vezető az SMTP-összekötőn keresztül valósul meg.</span><span class="sxs-lookup"><span data-stu-id="f173a-120">For our example, we'll set it up such that every time a new lead is created in Salesforce, a *send email* action occurs via the SMTP connector with a notification of the new lead being created.</span></span>

1. <span data-ttu-id="f173a-121">Adja meg *salesforce* be a keresőmezőbe a logic apps designer válassza ki a **Salesforce - amikor létrejön egy objektum** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="f173a-121">Enter *salesforce* in the search box on the logic apps designer then select the **Salesforce - When an object is created** trigger.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. <span data-ttu-id="f173a-122">A **egy objektumának létrejöttekor** vezérlő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f173a-122">The **When an object is created** control is displayed.</span></span>
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. <span data-ttu-id="f173a-123">Válassza ki a **objektumtípus** válassza *vezethet* objektumok közül.</span><span class="sxs-lookup"><span data-stu-id="f173a-123">Select the **Object Type** then select *Lead* from the list of objects.</span></span> <span data-ttu-id="f173a-124">Ebben a lépésben vannak jelzi, hogy hoz létre egy eseményindítót, amely értesíti a Logic Apps alkalmazást, amikor egy új vezető jön létre a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="f173a-124">In this step you are indicating that you are creating a trigger that will notify your logic app whenever a new lead is created in Salesforce.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. <span data-ttu-id="f173a-125">Az eseményindító létrehozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f173a-125">The trigger has been created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a><span data-ttu-id="f173a-126">Az SMTP-művelethez használata</span><span class="sxs-lookup"><span data-stu-id="f173a-126">Use an SMTP action</span></span>
<span data-ttu-id="f173a-127">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="f173a-127">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="f173a-128">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="f173a-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="f173a-129">Most, hogy hozzá lett adva az eseményindítót, kövesse az alábbi lépéseket egy SMTP-művelethez egy új vezető a Salesforce-ban létrehozásakor előforduló hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="f173a-129">Now that the trigger has been added, follow these steps to add an SMTP action that will occur when a new lead is created in Salesforce.</span></span>

1. <span data-ttu-id="f173a-130">Válassza ki **+ új lépés** hozzáadása a műveletet hajtson végre egy új vezető jön létre.</span><span class="sxs-lookup"><span data-stu-id="f173a-130">Select **+ New Step** to add the action you would like to take when a new lead is created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. <span data-ttu-id="f173a-131">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f173a-131">Select **Add an action**.</span></span> <span data-ttu-id="f173a-132">Ez a keresési mezőbe, ahol kereshet bármilyen műveletet meg szeretné igénybe megnyílik.</span><span class="sxs-lookup"><span data-stu-id="f173a-132">This opens the search box where you can search for any action you would like to take.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. <span data-ttu-id="f173a-133">Adja meg *smtp* SMTP kapcsolatos műveletek kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="f173a-133">Enter *smtp* to search for actions related to SMTP.</span></span>  
4. <span data-ttu-id="f173a-134">Válassza ki **SMTP - E-mail küldése** , a végrehajtandó műveletet, amikor az új vezető jön létre.</span><span class="sxs-lookup"><span data-stu-id="f173a-134">Select **SMTP - Send Email** as the action to take when the new lead is created.</span></span> <span data-ttu-id="f173a-135">A művelet blokk nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="f173a-135">The action control block opens.</span></span> <span data-ttu-id="f173a-136">A Tervező blokkban az smtp-kapcsolatot létesíteni, ha még nem meg korábban fog.</span><span class="sxs-lookup"><span data-stu-id="f173a-136">You will have to establish your smtp connection in the designer block if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. <span data-ttu-id="f173a-137">Adjon meg a kívánt e-mailek adataihoz a **SMTP - E-mail küldése** blokkot.</span><span class="sxs-lookup"><span data-stu-id="f173a-137">Input your desired email information in the **SMTP - Send Email** block.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. <span data-ttu-id="f173a-138">Mentse a munkáját ahhoz, hogy aktiválja a munkafolyamatot.</span><span class="sxs-lookup"><span data-stu-id="f173a-138">Save your work in order to activate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="f173a-139">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="f173a-139">Connector-specific details</span></span>

<span data-ttu-id="f173a-140">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/smtpconnector/).</span><span class="sxs-lookup"><span data-stu-id="f173a-140">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/smtpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="f173a-141">További összekötők</span><span class="sxs-lookup"><span data-stu-id="f173a-141">More connectors</span></span>
<span data-ttu-id="f173a-142">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="f173a-142">Go back to the [APIs list](apis-list.md).</span></span>