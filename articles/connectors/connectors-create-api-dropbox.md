---
title: "az Azure Logic Apps aaaDropbox összekötő |} Microsoft Docs"
description: "Az Azure App service Logic Apps alkalmazások létrehozása Csatlakozás tooDropbox toomanage a fájlokat. Hajtsa végre különböző műveleteket végez, például a feltöltés, frissítése, lekérése és Dropbox fájl törlése."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cb0ae033-aba7-4ac9-beaa-be561a0f0cac
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1f307477836104c0bc0008341604a1400860987f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-dropbox-connector"></a><span data-ttu-id="43db6-105">Hello Dropbox összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="43db6-105">Get started with hello Dropbox connector</span></span>
<span data-ttu-id="43db6-106">Csatlakozás tooDropbox toomanage a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="43db6-106">Connect tooDropbox toomanage your files.</span></span> <span data-ttu-id="43db6-107">Hajtsa végre különböző műveleteket végez, például a feltöltés, frissítése, lekérése és Dropbox fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="43db6-107">You can perform various actions such as upload, update, get, and delete files in Dropbox.</span></span>

<span data-ttu-id="43db6-108">toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="43db6-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="43db6-109">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="43db6-109">You can get started by [creating a Logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toodropbox"></a><span data-ttu-id="43db6-110">Csatlakozás tooDropbox</span><span class="sxs-lookup"><span data-stu-id="43db6-110">Connect tooDropbox</span></span>
<span data-ttu-id="43db6-111">A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="43db6-111">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="43db6-112">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="43db6-112">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="43db6-113">Például a rendelés tooconnect tooDropbox, először a Dropbox *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="43db6-113">For example, in order tooconnect tooDropbox, you first need a Dropbox *connection*.</span></span> <span data-ttu-id="43db6-114">a kapcsolat toocreate, kellene tooprovide hello hitelesítő adatok általában használni kívánja a tooconnect tooaccess hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="43db6-114">toocreate a connection, you would need tooprovide hello credentials you normally use tooaccess hello service you wish tooconnect to.</span></span> <span data-ttu-id="43db6-115">Igen hello Dropbox példában kellene hello hitelesítő adatok tooyour rendelés toocreate hello kapcsolat tooDropbox Dropbox-fiókot.</span><span class="sxs-lookup"><span data-stu-id="43db6-115">So, in hello Dropbox example, you would need hello credentials tooyour Dropbox account in order toocreate hello connection tooDropbox.</span></span> [<span data-ttu-id="43db6-116">További információ a kapcsolatok száma</span><span class="sxs-lookup"><span data-stu-id="43db6-116">Learn more about connections</span></span>]()

### <a name="create-a-connection-toodropbox"></a><span data-ttu-id="43db6-117">Egy kapcsolat tooDropbox létrehozása</span><span class="sxs-lookup"><span data-stu-id="43db6-117">Create a connection tooDropbox</span></span>
> [!INCLUDE [Steps toocreate a connection tooDropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a><span data-ttu-id="43db6-118">A Dropbox eseményindító használata</span><span class="sxs-lookup"><span data-stu-id="43db6-118">Use a Dropbox trigger</span></span>
<span data-ttu-id="43db6-119">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="43db6-119">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="43db6-120">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="43db6-120">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="43db6-121">A jelen példában használjuk hello **fájl létrehozásának** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="43db6-121">In this example, we will use hello **When a file is created** trigger.</span></span> <span data-ttu-id="43db6-122">Ehhez az eseményindítóhoz akkor fordul elő, ha azt telefonhívásokhoz hello **elérési út használatával fájl tartalmának lekérdezése** Dropbox művelet.</span><span class="sxs-lookup"><span data-stu-id="43db6-122">When this trigger occurs, we will call hello **Get file content using path** Dropbox action.</span></span> 

1. <span data-ttu-id="43db6-123">Adja meg *dropbox* a hello Logic Apps designer hello a keresési mezőbe, majd válassza ki hello **Dropbox - fájl létrehozásakor** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="43db6-123">Enter *dropbox* in hello search box on hello Logic Apps designer, then select hello **Dropbox - When a file is created** trigger.</span></span>      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. <span data-ttu-id="43db6-124">Válassza ki a kívánt tootrack fájllétrehozást hello mappát.</span><span class="sxs-lookup"><span data-stu-id="43db6-124">Select hello folder in which you want tootrack file creation.</span></span> <span data-ttu-id="43db6-125">Válasszon... (piros hello mezőben azonosított) és a Tallózás toohello mappa hello eseményindító tooselect bemeneti kívánja.</span><span class="sxs-lookup"><span data-stu-id="43db6-125">Select ... (identified in hello red box) and browse toohello folder you wish tooselect for hello trigger's input.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a><span data-ttu-id="43db6-126">A Dropbox művelettel</span><span class="sxs-lookup"><span data-stu-id="43db6-126">Use a Dropbox action</span></span>
<span data-ttu-id="43db6-127">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="43db6-127">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="43db6-128">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="43db6-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="43db6-129">Most, hogy hello eseményindító hozzá lett adva, kövesse ezeket a lépéseket tooadd egy műveletet, amely megkapja a hello új fájl tartalma.</span><span class="sxs-lookup"><span data-stu-id="43db6-129">Now that hello trigger has been added, follow these steps tooadd an action that will get hello new file's content.</span></span>

1. <span data-ttu-id="43db6-130">Válassza ki **+ új lépés** tooadd hello művelet szeretné tootake amikor egy új fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="43db6-130">Select **+ New Step** tooadd hello action you would like tootake when a new file is created.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. <span data-ttu-id="43db6-131">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="43db6-131">Select **Add an action**.</span></span> <span data-ttu-id="43db6-132">A megnyíló hello keresőmezőbe ahol kereshet bármely művelet, tootake szeretné.</span><span class="sxs-lookup"><span data-stu-id="43db6-132">This opens hello search box where you can search for any action you would like tootake.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. <span data-ttu-id="43db6-133">Adja meg *dropbox* toosearch kapcsolódó tooDropbox műveletek számára.</span><span class="sxs-lookup"><span data-stu-id="43db6-133">Enter *dropbox* toosearch for actions related tooDropbox.</span></span>  
4. <span data-ttu-id="43db6-134">Válassza ki **Dropbox - fájl tartalmának lekérése az elérési út használatával** művelet tootake hello, amikor egy új fájl jön létre hello kijelölt a Dropbox mappa.</span><span class="sxs-lookup"><span data-stu-id="43db6-134">Select **Dropbox - Get file content using path** as hello action tootake when a new file is created in hello selected Dropbox folder.</span></span> <span data-ttu-id="43db6-135">Megnyílik a hello művelet blokk.</span><span class="sxs-lookup"><span data-stu-id="43db6-135">hello action control block opens.</span></span> <span data-ttu-id="43db6-136">Akkor lesz felszólító tooauthorize a logic app tooaccess a Dropbox fiókot, ha még nem meg korábban.</span><span class="sxs-lookup"><span data-stu-id="43db6-136">You will be prompted tooauthorize your logic app tooaccess your Dropbox account if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. <span data-ttu-id="43db6-137">Válasszon... (hello hello jobb oldalán található **fájl elérési útját** vezérlő), és keresse meg azt szeretné, hogy toouse toohello-fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="43db6-137">Select ... (located at hello right side of hello **File Path** control) and browse toohello file path you would like toouse.</span></span> <span data-ttu-id="43db6-138">Másik lehetőségként hello **fájl elérési útját** token toospeed fel a logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="43db6-138">Or, use hello **file path** token toospeed up your logic app creation.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. <span data-ttu-id="43db6-139">Mentse a munkáját, és hozzon létre egy új fájlt a Dropbox tooactivate a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="43db6-139">Save your work and create a new file in Dropbox tooactivate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="43db6-140">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="43db6-140">Connector-specific details</span></span>

<span data-ttu-id="43db6-141">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/dropbox/).</span><span class="sxs-lookup"><span data-stu-id="43db6-141">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/dropbox/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="43db6-142">További összekötők</span><span class="sxs-lookup"><span data-stu-id="43db6-142">More connectors</span></span>
<span data-ttu-id="43db6-143">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="43db6-143">Go back toohello [APIs list](apis-list.md).</span></span>
