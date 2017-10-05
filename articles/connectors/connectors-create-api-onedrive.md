---
title: "Adja hozzá a OneDrive-összekötőt a Logic Apps |} Microsoft Docs"
description: "A OneDrive-összekötő REST API-paraméterekkel rendelkező áttekintése"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 63bd33bf4e09b98aa53dcfec9fcc4a0109204952
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-onedrive-connector"></a><span data-ttu-id="92406-103">A OneDrive-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="92406-103">Get started with the OneDrive connector</span></span>
<span data-ttu-id="92406-104">Csatlakozni a onedrive-nak a fájlok kezelését, beleértve a feltöltés, get, törölje a fájlokat, és több.</span><span class="sxs-lookup"><span data-stu-id="92406-104">Connect to OneDrive to manage your files, including upload, get, delete files, and more.</span></span> 

<span data-ttu-id="92406-105">A onedrive-ról hogy:</span><span class="sxs-lookup"><span data-stu-id="92406-105">With OneDrive, you:</span></span> 

* <span data-ttu-id="92406-106">A munkafolyamat létrehozása tárolja a fájlokat a onedrive-on, vagy frissítse a meglévő fájlokat a onedrive-on.</span><span class="sxs-lookup"><span data-stu-id="92406-106">Build your workflow by storing files in OneDrive, or update existing files in OneDrive.</span></span> 
* <span data-ttu-id="92406-107">Eseményindítók segítségével indítsa el a munkafolyamat, amikor egy fájl létrehozásakor vagy frissítésekor a onedrive-on belül.</span><span class="sxs-lookup"><span data-stu-id="92406-107">Use triggers to start your workflow when a file is created or updated within your OneDrive.</span></span>
* <span data-ttu-id="92406-108">Hozzon létre egy fájlt, törölje a fájlt, és egyéb műveleteket használni.</span><span class="sxs-lookup"><span data-stu-id="92406-108">Use actions to create a file, delete a file, and more.</span></span> <span data-ttu-id="92406-109">Például egy új Office 365 e-mailt fogadásakor mellékletet (trigger) hozzon létre egy új fájlt a onedrive-on (művelet).</span><span class="sxs-lookup"><span data-stu-id="92406-109">For example, when a new Office 365 email is received with an attachment (a trigger), create a new file in OneDrive (an action).</span></span>

<span data-ttu-id="92406-110">Ez a témakör bemutatja, hogyan használható a onedrive vállalati verzió összekötő logikai alkalmazás, és eseményindítók és műveletek is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="92406-110">This topic shows you how to use the OneDrive connector in a logic app, and also lists the triggers and actions.</span></span>

<span data-ttu-id="92406-111">A Logic Apps kapcsolatos további információkért lásd: [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="92406-111">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-onedrive"></a><span data-ttu-id="92406-112">Kapcsolódás a onedrive vállalati verzió</span><span class="sxs-lookup"><span data-stu-id="92406-112">Connect to OneDrive</span></span>
<span data-ttu-id="92406-113">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="92406-113">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="92406-114">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="92406-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="92406-115">Például ha csatlakozni szeretne a OneDrive, először a onedrive vállalati verzió *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="92406-115">For example, to connect to OneDrive, you first need a OneDrive *connection*.</span></span> <span data-ttu-id="92406-116">VPN-kapcsolat létrehozásához adja meg a hitelesítő adatok általában segítségével éri el a szolgáltatást, amelyhez csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="92406-116">To create a connection, enter the credentials you normally use to access the service you wish to connect to.</span></span> <span data-ttu-id="92406-117">Igen a onedrive-on, megadja a hitelesítő adatokat a OneDrive-fiókja, a VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="92406-117">So, with OneDrive, enter the credentials to your OneDrive account  to create the connection.</span></span>

### <a name="create-the-connection"></a><span data-ttu-id="92406-118">A kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="92406-118">Create the connection</span></span>
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="92406-119">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="92406-119">Use a trigger</span></span>
<span data-ttu-id="92406-120">Egy eseményindító nem egy eseményt, a logikai alkalmazás definiált munkafolyamat indításához használható.</span><span class="sxs-lookup"><span data-stu-id="92406-120">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="92406-121">Eseményindítók "lekérdezésére" a szolgáltatás egy intervallum és a kívánt gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="92406-121">Triggers "poll" the service at an interval and frequency that you want.</span></span> <span data-ttu-id="92406-122">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="92406-122">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="92406-123">Írja be a logikai alkalmazást listáját, illetve a "onedrive":</span><span class="sxs-lookup"><span data-stu-id="92406-123">In the logic app, type "onedrive" to get a list of the triggers:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. <span data-ttu-id="92406-124">Válassza ki **fájl módosításának**.</span><span class="sxs-lookup"><span data-stu-id="92406-124">Select **When a file is modified**.</span></span> <span data-ttu-id="92406-125">Ha már létezik egy kapcsolat, majd válassza ki az objektumválasztó megjelenítése gomb egy mappa kiválasztására.</span><span class="sxs-lookup"><span data-stu-id="92406-125">If a connection already exists, then select the Show Picker button to select a folder.</span></span>
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    <span data-ttu-id="92406-126">Ha a bejelentkezéshez kéri, akkor írja be a bejelentkezési a kapcsolat létrehozásához szükséges adatok.</span><span class="sxs-lookup"><span data-stu-id="92406-126">If you are prompted to sign in, then enter the sign in details to create the connection.</span></span> <span data-ttu-id="92406-127">[A kapcsolat létrehozása](connectors-create-api-onedrive.md#create-the-connection) a témakör felsorolja azokat a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="92406-127">[Create the connection](connectors-create-api-onedrive.md#create-the-connection) in this topic lists the steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="92406-128">Ebben a példában a logikai alkalmazás fut, amikor egy fájl a mappában, ha úgy dönt, hogy a rendszer frissíti.</span><span class="sxs-lookup"><span data-stu-id="92406-128">In this example, the logic app runs when a file in the folder you choose is updated.</span></span> <span data-ttu-id="92406-129">Ehhez az eseményindítóhoz eredményeinek megtekintéséhez adja hozzá az e-mailt küld egy másik művelet.</span><span class="sxs-lookup"><span data-stu-id="92406-129">To see the results of this trigger, add another action that sends you an email.</span></span> <span data-ttu-id="92406-130">Adja hozzá például az Office 365 Outlook *egy e-mailek küldése* , hogy e-mailt küld, amikor egy fájl frissítése műveletet.</span><span class="sxs-lookup"><span data-stu-id="92406-130">For example, add the Office 365 Outlook *Send an email* action that emails you when a file is updated.</span></span> 

3. <span data-ttu-id="92406-131">Válassza ki a **szerkesztése** gombra, majd a **gyakoriság** és **időköz** értékeket.</span><span class="sxs-lookup"><span data-stu-id="92406-131">Select the **Edit** button and set the **Frequency** and **Interval** values.</span></span> <span data-ttu-id="92406-132">Például, ha azt szeretné, hogy az eseményindító, és kérdezze le a 15 percenként, majd állítsa be a **gyakoriság** való **perc**, és állítsa be a **időköz** való **15**.</span><span class="sxs-lookup"><span data-stu-id="92406-132">For example, if you want the trigger to poll every 15 minutes, then set the **Frequency** to **Minute**, and set the **Interval** to **15**.</span></span> 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. <span data-ttu-id="92406-133">**Mentés** a módosításokat (bal felső sarkában az eszköztár).</span><span class="sxs-lookup"><span data-stu-id="92406-133">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="92406-134">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="92406-134">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="92406-135">Egy művelettel</span><span class="sxs-lookup"><span data-stu-id="92406-135">Use an action</span></span>
<span data-ttu-id="92406-136">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="92406-136">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="92406-137">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="92406-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="92406-138">Kattintson a plusz ikonra.</span><span class="sxs-lookup"><span data-stu-id="92406-138">Select the plus sign.</span></span> <span data-ttu-id="92406-139">Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy az egyik a **további** beállítások.</span><span class="sxs-lookup"><span data-stu-id="92406-139">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. <span data-ttu-id="92406-140">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="92406-140">Choose **Add an action**.</span></span>
3. <span data-ttu-id="92406-141">A szövegmezőben írja be a rendelkezésre álló műveletek listájának "onedrive".</span><span class="sxs-lookup"><span data-stu-id="92406-141">In the text box, type “onedrive” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. <span data-ttu-id="92406-142">Válassza ki a fenti példában **személyes OneDrive - fájl létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="92406-142">In our example, choose **OneDrive - Create file**.</span></span> <span data-ttu-id="92406-143">Ha már létezik egy kapcsolat, majd válassza ki a **mappa elérési útja** , amelyre a fájlt, írja be a **Fájlnév**, és válassza ki a **fájl tartalma** kívánt:</span><span class="sxs-lookup"><span data-stu-id="92406-143">If a connection already exists, then select the **Folder Path** to put the file, enter the **File Name**, and choose the **File Content** you want:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    <span data-ttu-id="92406-144">Ha a kapcsolati adatokat kéri, adja meg a részleteket a VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="92406-144">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="92406-145">[A kapcsolat létrehozása](connectors-create-api-onedrive.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="92406-145">[Create the connection](connectors-create-api-onedrive.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="92406-146">Ebben a példában azt hozzon létre egy új fájlt a onedrive vállalati verzió mappában.</span><span class="sxs-lookup"><span data-stu-id="92406-146">In this example, we create a new file in a OneDrive folder.</span></span> <span data-ttu-id="92406-147">Egy másik eseményindító kimenetét a OneDrive-fájl létrehozására használhatja.</span><span class="sxs-lookup"><span data-stu-id="92406-147">You can use output from another trigger to create the OneDrive file.</span></span> <span data-ttu-id="92406-148">Adja hozzá például az Office 365 Outlook *egy új e-mailt fogadásakor* eseményindító.</span><span class="sxs-lookup"><span data-stu-id="92406-148">For example, add the Office 365 Outlook *When a new email arrives* trigger.</span></span> <span data-ttu-id="92406-149">Adja meg a onedrive vállalati verzió *létrehozás fájl* művelet, a mellékletek és a Content-Type használó mezők egy ForEach létrehozni az új fájlt a onedrive-on belül.</span><span class="sxs-lookup"><span data-stu-id="92406-149">Then add the OneDrive *Create file* action that uses the Attachments and Content-Type fields within a ForEach to create the new file in OneDrive.</span></span> 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. <span data-ttu-id="92406-150">**Mentés** a módosításokat (bal felső sarkában az eszköztár).</span><span class="sxs-lookup"><span data-stu-id="92406-150">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="92406-151">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="92406-151">Your logic app is saved and may be automatically enabled.</span></span>


## <a name="connector-specific-details"></a><span data-ttu-id="92406-152">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="92406-152">Connector-specific details</span></span>

<span data-ttu-id="92406-153">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/onedriveconnector/).</span><span class="sxs-lookup"><span data-stu-id="92406-153">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/onedriveconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="92406-154">További összekötők</span><span class="sxs-lookup"><span data-stu-id="92406-154">More connectors</span></span>
<span data-ttu-id="92406-155">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="92406-155">Go back to the [APIs list](apis-list.md).</span></span>