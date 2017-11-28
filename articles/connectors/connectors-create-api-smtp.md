---
title: "az Azure Logic Apps aaaSMTP összekötő |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása Csatlakozás tooSMTP toosend e-mailt."
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
ms.openlocfilehash: 36bb836851014d24f2e069fda8376ad7a08c943b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-smtp-connector"></a><span data-ttu-id="b1efb-104">Hello SMTP-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="b1efb-104">Get started with hello SMTP connector</span></span>
<span data-ttu-id="b1efb-105">Csatlakozás tooSMTP toosend e-mailt.</span><span class="sxs-lookup"><span data-stu-id="b1efb-105">Connect tooSMTP toosend email.</span></span>

<span data-ttu-id="b1efb-106">toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b1efb-106">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="b1efb-107">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b1efb-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosmtp"></a><span data-ttu-id="b1efb-108">Csatlakozás tooSMTP</span><span class="sxs-lookup"><span data-stu-id="b1efb-108">Connect tooSMTP</span></span>
<span data-ttu-id="b1efb-109">A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b1efb-109">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="b1efb-110">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b1efb-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="b1efb-111">Például a tooconnect tooSMTP, először egy SMTP *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="b1efb-111">For example, tooconnect tooSMTP, you first need an SMTP *connection*.</span></span> <span data-ttu-id="b1efb-112">toocreate a kapcsolat létrehozásakor, adja meg a hello hitelesítő adatokkal kell általában tooaccess hello szolgáltatás csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b1efb-112">toocreate a connection, enter hello credentials you normally use tooaccess hello service you connect to.</span></span> <span data-ttu-id="b1efb-113">Igen a hello SMTP példában adja meg az hello hitelesítő adatok tooyour kapcsolat neve, a SMTP-kiszolgáló címére és a felhasználói bejelentkezési adatokat toocreate hello kapcsolat tooSMTP.</span><span class="sxs-lookup"><span data-stu-id="b1efb-113">So, in hello SMTP example, enter hello credentials tooyour connection name, SMTP server address, and user login information toocreate hello connection tooSMTP.</span></span>  

### <a name="create-a-connection-toosmtp"></a><span data-ttu-id="b1efb-114">Egy kapcsolat tooSMTP létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1efb-114">Create a connection tooSMTP</span></span>
> [!INCLUDE [Steps toocreate a connection tooSMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a><span data-ttu-id="b1efb-115">Az SMTP-eseményindító használata</span><span class="sxs-lookup"><span data-stu-id="b1efb-115">Use an SMTP trigger</span></span>
<span data-ttu-id="b1efb-116">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="b1efb-116">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="b1efb-117">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="b1efb-117">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="b1efb-118">Ebben a példában, mert SMTP nem rendelkezik saját, eseményindító fogjuk használni hello **Salesforce - amikor létrejön egy objektum** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="b1efb-118">In this example, because SMTP does not have a trigger of its own, we'll use hello **Salesforce - When an object is created** trigger.</span></span> <span data-ttu-id="b1efb-119">Ehhez az eseményindítóhoz akkor aktiválódik, amikor új objektumot hoz létre a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="b1efb-119">This trigger activates when a new object is created in Salesforce.</span></span> <span data-ttu-id="b1efb-120">A példa kedvéért be azt, hogy minden alkalommal új vezető jön létre a Salesforce, egy *e-mailek küldése* egy értesítés, amely hello új vezető létrehozásra a hello SMTP-összekötőn keresztül valósul meg.</span><span class="sxs-lookup"><span data-stu-id="b1efb-120">For our example, we'll set it up such that every time a new lead is created in Salesforce, a *send email* action occurs via hello SMTP connector with a notification of hello new lead being created.</span></span>

1. <span data-ttu-id="b1efb-121">Adja meg *salesforce* hello keresőmezőbe hello logic apps designer válassza hello **Salesforce - amikor létrejön egy objektum** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="b1efb-121">Enter *salesforce* in hello search box on hello logic apps designer then select hello **Salesforce - When an object is created** trigger.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. <span data-ttu-id="b1efb-122">Hello **egy objektumának létrejöttekor** vezérlő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b1efb-122">hello **When an object is created** control is displayed.</span></span>
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. <span data-ttu-id="b1efb-123">SELECT hello **objektumtípus** válassza *vezethet* hello objektumok listája.</span><span class="sxs-lookup"><span data-stu-id="b1efb-123">Select hello **Object Type** then select *Lead* from hello list of objects.</span></span> <span data-ttu-id="b1efb-124">Ebben a lépésben vannak jelzi, hogy hoz létre egy eseményindítót, amely értesíti a Logic Apps alkalmazást, amikor egy új vezető jön létre a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="b1efb-124">In this step you are indicating that you are creating a trigger that will notify your logic app whenever a new lead is created in Salesforce.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. <span data-ttu-id="b1efb-125">hello eseményindító létrehozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b1efb-125">hello trigger has been created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a><span data-ttu-id="b1efb-126">Az SMTP-művelethez használata</span><span class="sxs-lookup"><span data-stu-id="b1efb-126">Use an SMTP action</span></span>
<span data-ttu-id="b1efb-127">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="b1efb-127">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="b1efb-128">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="b1efb-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="b1efb-129">Most, hogy hello eseményindító hozzá lett adva, kövesse ezeket, hogy történjen a Salesforce jön létre egy új vezető lépéseket tooadd egy SMTP műveletet.</span><span class="sxs-lookup"><span data-stu-id="b1efb-129">Now that hello trigger has been added, follow these steps tooadd an SMTP action that will occur when a new lead is created in Salesforce.</span></span>

1. <span data-ttu-id="b1efb-130">Válassza ki **+ új lépés** tooadd hello művelet szeretné tootake egy új vezető létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b1efb-130">Select **+ New Step** tooadd hello action you would like tootake when a new lead is created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. <span data-ttu-id="b1efb-131">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b1efb-131">Select **Add an action**.</span></span> <span data-ttu-id="b1efb-132">A megnyíló hello keresőmezőbe ahol kereshet bármely művelet, tootake szeretné.</span><span class="sxs-lookup"><span data-stu-id="b1efb-132">This opens hello search box where you can search for any action you would like tootake.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. <span data-ttu-id="b1efb-133">Adja meg *smtp* toosearch kapcsolódó tooSMTP műveletek számára.</span><span class="sxs-lookup"><span data-stu-id="b1efb-133">Enter *smtp* toosearch for actions related tooSMTP.</span></span>  
4. <span data-ttu-id="b1efb-134">Válassza ki **SMTP - E-mail küldése** , hello művelet tootake hello új vezető létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b1efb-134">Select **SMTP - Send Email** as hello action tootake when hello new lead is created.</span></span> <span data-ttu-id="b1efb-135">Megnyílik a hello művelet blokk.</span><span class="sxs-lookup"><span data-stu-id="b1efb-135">hello action control block opens.</span></span> <span data-ttu-id="b1efb-136">Hogy tooestablish az smtp-kapcsolat hello Tervező blokkban Ha még nem meg korábban.</span><span class="sxs-lookup"><span data-stu-id="b1efb-136">You will have tooestablish your smtp connection in hello designer block if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. <span data-ttu-id="b1efb-137">Adjon meg a kívánt e-mail adatokkal hello **SMTP - E-mail küldése** blokkot.</span><span class="sxs-lookup"><span data-stu-id="b1efb-137">Input your desired email information in hello **SMTP - Send Email** block.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. <span data-ttu-id="b1efb-138">Mentse a munkáját a rendelés tooactivate a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="b1efb-138">Save your work in order tooactivate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="b1efb-139">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="b1efb-139">Connector-specific details</span></span>

<span data-ttu-id="b1efb-140">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/smtpconnector/).</span><span class="sxs-lookup"><span data-stu-id="b1efb-140">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/smtpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="b1efb-141">További összekötők</span><span class="sxs-lookup"><span data-stu-id="b1efb-141">More connectors</span></span>
<span data-ttu-id="b1efb-142">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="b1efb-142">Go back toohello [APIs list](apis-list.md).</span></span>
