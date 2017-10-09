---
title: "aaaAdd hello OneDrive-összekötőt a Logic Apps a |} Microsoft Docs"
description: "Hello OneDrive összekötő REST API-paraméterekkel rendelkező áttekintése"
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
ms.openlocfilehash: 8303794bb3c2844de288f87f40639abb84c160fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-onedrive-connector"></a><span data-ttu-id="da73c-103">Hello OneDrive összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="da73c-103">Get started with hello OneDrive connector</span></span>
<span data-ttu-id="da73c-104">Csatlakozás tooOneDrive toomanage a fájlokat, például az feltöltési, get, törölje a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="da73c-104">Connect tooOneDrive toomanage your files, including upload, get, delete files, and more.</span></span> 

<span data-ttu-id="da73c-105">A onedrive-ról hogy:</span><span class="sxs-lookup"><span data-stu-id="da73c-105">With OneDrive, you:</span></span> 

* <span data-ttu-id="da73c-106">A munkafolyamat létrehozása tárolja a fájlokat a onedrive-on, vagy frissítse a meglévő fájlokat a onedrive-on.</span><span class="sxs-lookup"><span data-stu-id="da73c-106">Build your workflow by storing files in OneDrive, or update existing files in OneDrive.</span></span> 
* <span data-ttu-id="da73c-107">Felhasználhatja a eseményindítók toostart a munkafolyamat egy fájl létrehozásakor vagy frissítésekor a onedrive-on belül.</span><span class="sxs-lookup"><span data-stu-id="da73c-107">Use triggers toostart your workflow when a file is created or updated within your OneDrive.</span></span>
* <span data-ttu-id="da73c-108">Műveletek toocreate fájlt használja, és törölje a fájlt, és több.</span><span class="sxs-lookup"><span data-stu-id="da73c-108">Use actions toocreate a file, delete a file, and more.</span></span> <span data-ttu-id="da73c-109">Például egy új Office 365 e-mailt fogadásakor mellékletet (trigger) hozzon létre egy új fájlt a onedrive-on (művelet).</span><span class="sxs-lookup"><span data-stu-id="da73c-109">For example, when a new Office 365 email is received with an attachment (a trigger), create a new file in OneDrive (an action).</span></span>

<span data-ttu-id="da73c-110">Ez a témakör bemutatja, hogyan toouse hello OneDrive-összekötőt a logikai alkalmazás, és is listák hello eseményindítók és műveletek.</span><span class="sxs-lookup"><span data-stu-id="da73c-110">This topic shows you how toouse hello OneDrive connector in a logic app, and also lists hello triggers and actions.</span></span>

<span data-ttu-id="da73c-111">További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="da73c-111">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooonedrive"></a><span data-ttu-id="da73c-112">Csatlakozás tooOneDrive</span><span class="sxs-lookup"><span data-stu-id="da73c-112">Connect tooOneDrive</span></span>
<span data-ttu-id="da73c-113">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="da73c-113">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="da73c-114">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="da73c-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="da73c-115">Például a tooconnect tooOneDrive, először a onedrive vállalati verzió *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="da73c-115">For example, tooconnect tooOneDrive, you first need a OneDrive *connection*.</span></span> <span data-ttu-id="da73c-116">toocreate a kapcsolat létrehozásakor, adja meg a hello hitelesítő adatok általában használni kívánja a tooconnect tooaccess hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="da73c-116">toocreate a connection, enter hello credentials you normally use tooaccess hello service you wish tooconnect to.</span></span> <span data-ttu-id="da73c-117">Igen a onedrive-on, adja meg a hello hitelesítő adatok tooyour OneDrive fiók toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="da73c-117">So, with OneDrive, enter hello credentials tooyour OneDrive account  toocreate hello connection.</span></span>

### <a name="create-hello-connection"></a><span data-ttu-id="da73c-118">Hello kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="da73c-118">Create hello connection</span></span>
> [!INCLUDE [Steps toocreate a connection tooOneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="da73c-119">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="da73c-119">Use a trigger</span></span>
<span data-ttu-id="da73c-120">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="da73c-120">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="da73c-121">Eseményindítók "lekérdezésére" hello szolgáltatást egy intervallum és a kívánt gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="da73c-121">Triggers "poll" hello service at an interval and frequency that you want.</span></span> <span data-ttu-id="da73c-122">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="da73c-122">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="da73c-123">A hello logikai alkalmazás írja be a "onedrive" tooget hello eseményindítók listáját:</span><span class="sxs-lookup"><span data-stu-id="da73c-123">In hello logic app, type "onedrive" tooget a list of hello triggers:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. <span data-ttu-id="da73c-124">Válassza ki **fájl módosításának**.</span><span class="sxs-lookup"><span data-stu-id="da73c-124">Select **When a file is modified**.</span></span> <span data-ttu-id="da73c-125">Ha már létezik egy kapcsolat, majd válassza ki a hello objektumválasztó megjelenítése gomb tooselect egy mappát.</span><span class="sxs-lookup"><span data-stu-id="da73c-125">If a connection already exists, then select hello Show Picker button tooselect a folder.</span></span>
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    <span data-ttu-id="da73c-126">Ha a kért toosign, majd adja meg az hello bejelentkezési részletek toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="da73c-126">If you are prompted toosign in, then enter hello sign in details toocreate hello connection.</span></span> <span data-ttu-id="da73c-127">[Hello kapcsolat létrehozása](connectors-create-api-onedrive.md#create-the-connection) ebben a témakörben hello lépéseket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="da73c-127">[Create hello connection](connectors-create-api-onedrive.md#create-the-connection) in this topic lists hello steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="da73c-128">Ebben a példában hello logikai alkalmazás fut, amikor egy fájlt úgy dönt, hogy a rendszer frissíti hello mappában.</span><span class="sxs-lookup"><span data-stu-id="da73c-128">In this example, hello logic app runs when a file in hello folder you choose is updated.</span></span> <span data-ttu-id="da73c-129">Ehhez az eseményindítóhoz toosee hello eredményeit hozzáadása, amely egy e-mailt küld egy másik művelet.</span><span class="sxs-lookup"><span data-stu-id="da73c-129">toosee hello results of this trigger, add another action that sends you an email.</span></span> <span data-ttu-id="da73c-130">Adja hozzá például az Office 365 Outlook hello *egy e-mailek küldése* , hogy e-mailt küld, amikor egy fájl frissítése műveletet.</span><span class="sxs-lookup"><span data-stu-id="da73c-130">For example, add hello Office 365 Outlook *Send an email* action that emails you when a file is updated.</span></span> 

3. <span data-ttu-id="da73c-131">Jelölje be hello **szerkesztése** gombra, majd hello **gyakoriság** és **időköz** értékeket.</span><span class="sxs-lookup"><span data-stu-id="da73c-131">Select hello **Edit** button and set hello **Frequency** and **Interval** values.</span></span> <span data-ttu-id="da73c-132">Például, ha azt szeretné, hello eseményindító toopoll 15 percenként, majd állítsa be hello **gyakoriság** túl**perc**, és a set hello **időköz** túl**15**.</span><span class="sxs-lookup"><span data-stu-id="da73c-132">For example, if you want hello trigger toopoll every 15 minutes, then set hello **Frequency** too**Minute**, and set hello **Interval** too**15**.</span></span> 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. <span data-ttu-id="da73c-133">**Mentés** a módosításokat (bal felső sarkában hello eszköztár).</span><span class="sxs-lookup"><span data-stu-id="da73c-133">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="da73c-134">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="da73c-134">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="da73c-135">Egy művelettel</span><span class="sxs-lookup"><span data-stu-id="da73c-135">Use an action</span></span>
<span data-ttu-id="da73c-136">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="da73c-136">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="da73c-137">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="da73c-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="da73c-138">Válassza ki a hello plusz jel.</span><span class="sxs-lookup"><span data-stu-id="da73c-138">Select hello plus sign.</span></span> <span data-ttu-id="da73c-139">Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy egy hello **további** beállítások.</span><span class="sxs-lookup"><span data-stu-id="da73c-139">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. <span data-ttu-id="da73c-140">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="da73c-140">Choose **Add an action**.</span></span>
3. <span data-ttu-id="da73c-141">Hello szövegmezőbe írja be a "onedrive" tooget összes hello elérhető műveletek listájának.</span><span class="sxs-lookup"><span data-stu-id="da73c-141">In hello text box, type “onedrive” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. <span data-ttu-id="da73c-142">Válassza ki a fenti példában **személyes OneDrive - fájl létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="da73c-142">In our example, choose **OneDrive - Create file**.</span></span> <span data-ttu-id="da73c-143">Ha már létezik egy kapcsolat, majd válassza ki hello **mappa elérési útja** tooput hello hello adja meg **Fájlnév**, és válassza ki a hello **fájl tartalma** kívánt:</span><span class="sxs-lookup"><span data-stu-id="da73c-143">If a connection already exists, then select hello **Folder Path** tooput hello file, enter hello **File Name**, and choose hello **File Content** you want:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    <span data-ttu-id="da73c-144">Ha hello kapcsolati adatokat kéri, adja meg hello részletek toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="da73c-144">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="da73c-145">[Hello kapcsolat létrehozása](connectors-create-api-onedrive.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="da73c-145">[Create hello connection](connectors-create-api-onedrive.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="da73c-146">Ebben a példában azt hozzon létre egy új fájlt a onedrive vállalati verzió mappában.</span><span class="sxs-lookup"><span data-stu-id="da73c-146">In this example, we create a new file in a OneDrive folder.</span></span> <span data-ttu-id="da73c-147">Kimeneti fájlból egy másik eseményindító toocreate hello onedrive vállalati verzió is használhatja.</span><span class="sxs-lookup"><span data-stu-id="da73c-147">You can use output from another trigger toocreate hello OneDrive file.</span></span> <span data-ttu-id="da73c-148">Adja hozzá például az Office 365 Outlook hello *egy új e-mailt fogadásakor* eseményindító.</span><span class="sxs-lookup"><span data-stu-id="da73c-148">For example, add hello Office 365 Outlook *When a new email arrives* trigger.</span></span> <span data-ttu-id="da73c-149">Majd adja hozzá a onedrive vállalati verzió hello *létrehozás fájl* művelet hello mellékletek és a Content-Type mezők használó ForEach toocreate hello új fájlt a onedrive-on belül.</span><span class="sxs-lookup"><span data-stu-id="da73c-149">Then add hello OneDrive *Create file* action that uses hello Attachments and Content-Type fields within a ForEach toocreate hello new file in OneDrive.</span></span> 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. <span data-ttu-id="da73c-150">**Mentés** a módosításokat (bal felső sarkában hello eszköztár).</span><span class="sxs-lookup"><span data-stu-id="da73c-150">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="da73c-151">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="da73c-151">Your logic app is saved and may be automatically enabled.</span></span>


## <a name="connector-specific-details"></a><span data-ttu-id="da73c-152">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="da73c-152">Connector-specific details</span></span>

<span data-ttu-id="da73c-153">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/onedriveconnector/).</span><span class="sxs-lookup"><span data-stu-id="da73c-153">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/onedriveconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="da73c-154">További összekötők</span><span class="sxs-lookup"><span data-stu-id="da73c-154">More connectors</span></span>
<span data-ttu-id="da73c-155">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="da73c-155">Go back toohello [APIs list](apis-list.md).</span></span>
