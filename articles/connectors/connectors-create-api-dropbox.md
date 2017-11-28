---
title: "Az Azure Logic Apps Dropbox összekötő |} Microsoft Docs"
description: "Az Azure App service Logic Apps alkalmazások létrehozása A fájlok kezelését Dropbox csatlakozni. Hajtsa végre különböző műveleteket végez, például a feltöltés, frissítése, lekérése és Dropbox fájl törlése."
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
ms.openlocfilehash: 0d09580c60fd620811b539147439d0922839fe7e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-dropbox-connector"></a><span data-ttu-id="551a3-105">A Dropbox-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="551a3-105">Get started with the Dropbox connector</span></span>
<span data-ttu-id="551a3-106">A fájlok kezelését Dropbox csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="551a3-106">Connect to Dropbox to manage your files.</span></span> <span data-ttu-id="551a3-107">Hajtsa végre különböző műveleteket végez, például a feltöltés, frissítése, lekérése és Dropbox fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="551a3-107">You can perform various actions such as upload, update, get, and delete files in Dropbox.</span></span>

<span data-ttu-id="551a3-108">Használandó [a csatlakozókat](apis-list.md), először hozzon létre egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="551a3-108">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="551a3-109">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="551a3-109">You can get started by [creating a Logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-dropbox"></a><span data-ttu-id="551a3-110">Csatlakozás a dropbox alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="551a3-110">Connect to Dropbox</span></span>
<span data-ttu-id="551a3-111">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először hozzon létre egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="551a3-111">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="551a3-112">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="551a3-112">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="551a3-113">Például használata esetén csatlakozhassanak a dropbox alkalmazásba, először a Dropbox *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="551a3-113">For example, in order to connect to Dropbox, you first need a Dropbox *connection*.</span></span> <span data-ttu-id="551a3-114">VPN-kapcsolat létrehozásához kellene általában csatlakozni szeretne a szolgáltatás eléréséhez használt hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="551a3-114">To create a connection, you would need to provide the credentials you normally use to access the service you wish to connect to.</span></span> <span data-ttu-id="551a3-115">Igen a Dropbox példában kellene a hitelesítő adatokat a Dropbox-fiók a Dropbox kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="551a3-115">So, in the Dropbox example, you would need the credentials to your Dropbox account in order to create the connection to Dropbox.</span></span> [<span data-ttu-id="551a3-116">További információ a kapcsolatok száma</span><span class="sxs-lookup"><span data-stu-id="551a3-116">Learn more about connections</span></span>]()

### <a name="create-a-connection-to-dropbox"></a><span data-ttu-id="551a3-117">A dropbox alkalmazásba kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="551a3-117">Create a connection to Dropbox</span></span>
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a><span data-ttu-id="551a3-118">A Dropbox eseményindító használata</span><span class="sxs-lookup"><span data-stu-id="551a3-118">Use a Dropbox trigger</span></span>
<span data-ttu-id="551a3-119">Egy eseményindító nem egy eseményt, a logikai alkalmazás definiált munkafolyamat indításához használható.</span><span class="sxs-lookup"><span data-stu-id="551a3-119">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="551a3-120">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="551a3-120">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="551a3-121">A jelen példában használjuk a **fájl létrehozásának** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="551a3-121">In this example, we will use the **When a file is created** trigger.</span></span> <span data-ttu-id="551a3-122">Ehhez az eseményindítóhoz akkor fordul elő, amikor ezt nevezik a **elérési út használatával fájl tartalmának lekérdezése** Dropbox művelet.</span><span class="sxs-lookup"><span data-stu-id="551a3-122">When this trigger occurs, we will call the **Get file content using path** Dropbox action.</span></span> 

1. <span data-ttu-id="551a3-123">Adja meg *dropbox* be a keresőmezőbe a Logic Apps-tervezőben, majd válassza ki a **Dropbox - fájl létrehozásakor** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="551a3-123">Enter *dropbox* in the search box on the Logic Apps designer, then select the **Dropbox - When a file is created** trigger.</span></span>      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. <span data-ttu-id="551a3-124">Válassza ki a mappát, ahol alkalmazni kívánja nyomon követését a fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="551a3-124">Select the folder in which you want to track file creation.</span></span> <span data-ttu-id="551a3-125">Válasszon... (a vörös téglalappal azonosított), és keresse meg azt a mappát, válassza ki van adva az eseményindítót kíván.</span><span class="sxs-lookup"><span data-stu-id="551a3-125">Select ... (identified in the red box) and browse to the folder you wish to select for the trigger's input.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a><span data-ttu-id="551a3-126">A Dropbox művelettel</span><span class="sxs-lookup"><span data-stu-id="551a3-126">Use a Dropbox action</span></span>
<span data-ttu-id="551a3-127">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="551a3-127">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="551a3-128">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="551a3-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="551a3-129">Most, hogy hozzá lett adva az eseményindítót, kövesse az alábbi lépéseket, amelyek megkapják az új fájl tartalma művelet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="551a3-129">Now that the trigger has been added, follow these steps to add an action that will get the new file's content.</span></span>

1. <span data-ttu-id="551a3-130">Válassza ki **+ új lépés** hozzáadása a műveletet hajtson végre egy új fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="551a3-130">Select **+ New Step** to add the action you would like to take when a new file is created.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. <span data-ttu-id="551a3-131">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="551a3-131">Select **Add an action**.</span></span> <span data-ttu-id="551a3-132">Ez a keresési mezőbe, ahol kereshet bármilyen műveletet meg szeretné igénybe megnyílik.</span><span class="sxs-lookup"><span data-stu-id="551a3-132">This opens the search box where you can search for any action you would like to take.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. <span data-ttu-id="551a3-133">Adja meg *dropbox* Dropbox kapcsolatos műveletek kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="551a3-133">Enter *dropbox* to search for actions related to Dropbox.</span></span>  
4. <span data-ttu-id="551a3-134">Válassza ki **Dropbox - fájl tartalmának lekérése az elérési út használatával** esetén végrehajtandó műveletet, egy új fájl jön létre a kiválasztott Dropbox mappában.</span><span class="sxs-lookup"><span data-stu-id="551a3-134">Select **Dropbox - Get file content using path** as the action to take when a new file is created in the selected Dropbox folder.</span></span> <span data-ttu-id="551a3-135">A művelet blokk nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="551a3-135">The action control block opens.</span></span> <span data-ttu-id="551a3-136">Kérni fogja a Logic Apps alkalmazást a Dropbox-fiók eléréséhez, ha még nem meg korábban engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="551a3-136">You will be prompted to authorize your logic app to access your Dropbox account if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. <span data-ttu-id="551a3-137">Válasszon... (jobb oldalán található a **fájl elérési útját** vezérlő), és keresse meg a használni kívánt fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="551a3-137">Select ... (located at the right side of the **File Path** control) and browse to the file path you would like to use.</span></span> <span data-ttu-id="551a3-138">Másik lehetőségként a **fájl elérési útját** felgyorsítható a logic app létrehozásának token.</span><span class="sxs-lookup"><span data-stu-id="551a3-138">Or, use the **file path** token to speed up your logic app creation.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. <span data-ttu-id="551a3-139">Mentse a munkáját, és hozzon létre egy új fájlt a dropbox-ba, a munkafolyamat aktiválásához.</span><span class="sxs-lookup"><span data-stu-id="551a3-139">Save your work and create a new file in Dropbox to activate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="551a3-140">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="551a3-140">Connector-specific details</span></span>

<span data-ttu-id="551a3-141">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/dropbox/).</span><span class="sxs-lookup"><span data-stu-id="551a3-141">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/dropbox/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="551a3-142">További összekötők</span><span class="sxs-lookup"><span data-stu-id="551a3-142">More connectors</span></span>
<span data-ttu-id="551a3-143">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="551a3-143">Go back to the [APIs list](apis-list.md).</span></span>