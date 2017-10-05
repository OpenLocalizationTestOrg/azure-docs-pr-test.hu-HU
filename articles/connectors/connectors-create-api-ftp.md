---
title: "Az FTP-összekötő használata a logic apps |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása A fájlok kezelését FTP-kiszolgálóhoz csatlakozni. Például feltöltés különféle műveleteket hajtson végre, frissítése, lekérése és FTP-kiszolgáló fájlok törlése."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d83c55fe-eb59-4b7b-a5ec-afac5c772616
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/22/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 61bfbedfd4f1e84b6976099323a32f3a720634c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-ftp-connector"></a><span data-ttu-id="62973-105">Az FTP-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="62973-105">Get started with the FTP connector</span></span>
<span data-ttu-id="62973-106">Az FTP-összekötő segítségével figyeléséhez, kezeléséhez és az FTP-kiszolgálón-fájlok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="62973-106">Use the FTP connector to monitor, manage and create files on an  FTP server.</span></span> 

<span data-ttu-id="62973-107">Használandó [a csatlakozókat](apis-list.md), először hozzon létre egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="62973-107">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="62973-108">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="62973-108">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-ftp"></a><span data-ttu-id="62973-109">FTP-csatlakozás</span><span class="sxs-lookup"><span data-stu-id="62973-109">Connect to FTP</span></span>
<span data-ttu-id="62973-110">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először hozzon létre egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="62973-110">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="62973-111">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="62973-111">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-ftp"></a><span data-ttu-id="62973-112">FTP-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="62973-112">Create a connection to FTP</span></span>
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a><span data-ttu-id="62973-113">FTP-eseményindítók</span><span class="sxs-lookup"><span data-stu-id="62973-113">Use a FTP trigger</span></span>
<span data-ttu-id="62973-114">Egy eseményindító nem egy eseményt, a logikai alkalmazás definiált munkafolyamat indításához használható.</span><span class="sxs-lookup"><span data-stu-id="62973-114">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="62973-115">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="62973-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="62973-116">Az FTP-összekötőnek szüksége van egy FTP-kiszolgálót, amely elérhető az internetről és passzív módban való működésre van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="62973-116">The FTP connector requires an FTP server that  is accessible from the Internet and is configured to operate with PASSIVE mode.</span></span> <span data-ttu-id="62973-117">Az FTP-összekötő is **implicit ftps-t (FTP szolgáltatás SSL-en keresztül) nem kompatibilis**.</span><span class="sxs-lookup"><span data-stu-id="62973-117">Also, the FTP connector is **not compatible with implicit FTPS (FTP over SSL)**.</span></span> <span data-ttu-id="62973-118">Az FTP-összekötő csak explicit ftps-t (FTP szolgáltatás SSL-en keresztül) támogatja.</span><span class="sxs-lookup"><span data-stu-id="62973-118">The FTP connector only supports explicit FTPS (FTP over SSL).</span></span>  
> 
> 

<span data-ttu-id="62973-119">Ebben a példában I bemutatja, hogyan használható a **FTP - amikor egy fájl hozzáadása vagy módosítása** eseményindítót, hogy kezdeményezhet a logic app munkafolyamat, amikor egy fájl hozzá vagy módosítja az FTP-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="62973-119">In this example, I will show you how to use the **FTP - When a file is added or modified** trigger to initiate a logic app workflow when a file is added to, or modified on, an FTP server.</span></span> <span data-ttu-id="62973-120">A vállalati például ehhez az eseményindítóhoz segítségével egy FTP-mappa az új fájlok, amelyek megfelelnek a rendeléseket ügyfelek figyelése.</span><span class="sxs-lookup"><span data-stu-id="62973-120">In an enterprise example, you could use this trigger to monitor an FTP folder for new files that represent orders from customers.</span></span>  <span data-ttu-id="62973-121">Az FTP-összekötő művelet aztán használhatja például a **fájl tartalmának lekérdezése** az rendelések adatbázisban a ahhoz, hogy további feldolgozás és a tároló tartalmának eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="62973-121">You could then use an FTP connector action such as **Get file content** to get the contents of the order for further processing and storage in your orders database.</span></span>

1. <span data-ttu-id="62973-122">Adja meg *ftp* be a keresőmezőbe a logic apps designer válassza ki a **FTP - amikor egy fájl hozzáadása vagy módosítása** eseményindító</span><span class="sxs-lookup"><span data-stu-id="62973-122">Enter *ftp* in the search box on the logic apps designer then select the **FTP - When a file is added or modified**  trigger</span></span>   
   <span data-ttu-id="62973-123">![FTP eseményindító kép 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="62973-123">![FTP trigger image 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span></span>  
   <span data-ttu-id="62973-124">A **amikor egy fájl hozzáadása vagy módosítása esetén** vezérlő megnyílik.</span><span class="sxs-lookup"><span data-stu-id="62973-124">The **When a file is added or modified** control opens up</span></span>  
   <span data-ttu-id="62973-125">![FTP eseményindító kép 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="62973-125">![FTP trigger image 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span></span>  
2. <span data-ttu-id="62973-126">Válassza ki a **...**  a vezérlő jobb oldalán található.</span><span class="sxs-lookup"><span data-stu-id="62973-126">Select the **...** located on the right side of the control.</span></span> <span data-ttu-id="62973-127">Ekkor megnyílik a mappa példányválasztó vezérlő</span><span class="sxs-lookup"><span data-stu-id="62973-127">This opens the folder picker control</span></span>  
   <span data-ttu-id="62973-128">![FTP eseményindító kép 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span><span class="sxs-lookup"><span data-stu-id="62973-128">![FTP trigger image 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span></span>  
3. <span data-ttu-id="62973-129">Válassza ki a  **>**  (jobbra), és keresse meg a mappát, amely új vagy módosított fájlokat szeretné figyelni.</span><span class="sxs-lookup"><span data-stu-id="62973-129">Select the **>** (right arrow) and browse to find the folder that you want to monitor for new or modified files.</span></span> <span data-ttu-id="62973-130">Válassza ki a mappát és a mappa megjelenik a figyelmeztetés a **mappa** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="62973-130">Select the folder and notice the folder is now displayed in the **Folder** control.</span></span>  
   <span data-ttu-id="62973-131">![FTP eseményindító kép 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span><span class="sxs-lookup"><span data-stu-id="62973-131">![FTP trigger image 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span></span>   

<span data-ttu-id="62973-132">A Logic Apps alkalmazást ezen a ponton úgy van konfigurálva, az eseményindítók és műveletek a munkafolyamat futtató akkor kezdődik, amikor egy fájl módosított, vagy az adott FTP mappában létrehozott eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="62973-132">At this point, your logic app has been configured with a trigger that will begin a run of the other triggers and actions in the workflow when a file is either modified or created in the specific FTP folder.</span></span> 

> [!NOTE]
> <span data-ttu-id="62973-133">A logikai alkalmazás működéséhez legalább egy eseményindító és egy műveletet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="62973-133">For a logic app to be functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="62973-134">Kövesse a következő szakaszban művelet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="62973-134">Follow the steps in the next section to add an action.</span></span>  
> 
> 

## <a name="use-a-ftp-action"></a><span data-ttu-id="62973-135">Egy FTP művelettel</span><span class="sxs-lookup"><span data-stu-id="62973-135">Use a FTP action</span></span>
<span data-ttu-id="62973-136">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="62973-136">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="62973-137">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="62973-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="62973-138">Most, hogy hozzáadta egy eseményindítót, kövesse az alábbi lépéseket, amely megkapja a tartalmát az új vagy módosított fájl található az eseményindító által művelet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="62973-138">Now that you have added a trigger, follow these steps to add an action that will get the contents of the new or modified file found by the trigger.</span></span>    

1. <span data-ttu-id="62973-139">Válassza ki **+ új lépés** hozzáadni a a műveletet az FTP-kiszolgálón a fájl tartalmának beolvasása</span><span class="sxs-lookup"><span data-stu-id="62973-139">Select **+ New step** to add the the action to get the contents of the file on the FTP server</span></span>  
2. <span data-ttu-id="62973-140">Válassza ki a **művelet hozzáadása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="62973-140">Select the **Add an action** link.</span></span>  
   <span data-ttu-id="62973-141">![FTP-művelet kép 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span><span class="sxs-lookup"><span data-stu-id="62973-141">![FTP action image 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span></span>  
3. <span data-ttu-id="62973-142">Adja meg *FTP* FTP kapcsolatos összes műveletet kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="62973-142">Enter *FTP* to search for all actions related to FTP.</span></span>
4. <span data-ttu-id="62973-143">Válassza ki **FTP - fájl tartalmának lekérdezése** , a végrehajtandó műveletet, amikor egy új vagy módosított fájl FTP mappában található.</span><span class="sxs-lookup"><span data-stu-id="62973-143">Select **FTP - Get file content**  as the action to take when a new or modified file is found in the FTP folder.</span></span>      
   <span data-ttu-id="62973-144">![FTP-művelet kép 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span><span class="sxs-lookup"><span data-stu-id="62973-144">![FTP action image 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span></span>  
   <span data-ttu-id="62973-145">A **fájl tartalmának lekérdezése** megnyílik szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="62973-145">The **Get file content** control opens.</span></span> <span data-ttu-id="62973-146">**Megjegyzés:**: kérni fogja a Logic Apps alkalmazást az FTP-kiszolgáló fiók eléréséhez, ha még nem meg korábban engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="62973-146">**Note**: you will be prompted to authorize your logic app to access your FTP server account if you have not done so previously.</span></span>  
   <span data-ttu-id="62973-147">![FTP-művelet kép 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span><span class="sxs-lookup"><span data-stu-id="62973-147">![FTP action image 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span></span>   
5. <span data-ttu-id="62973-148">Válassza ki a **fájl** vezérlő (az üres helyet alatt található **fájl***).</span><span class="sxs-lookup"><span data-stu-id="62973-148">Select the **File** control (the white space located below **FILE***).</span></span> <span data-ttu-id="62973-149">Itt használhatja a különböző tulajdonságok valamelyikét a következő új vagy módosított fájl megtalálható-e az FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="62973-149">Here, you can use any of the various properties from the new or modified file found on the FTP server.</span></span>  
6. <span data-ttu-id="62973-150">Válassza ki a **tartalom fájl** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="62973-150">Select the **File content** option.</span></span>  
   <span data-ttu-id="62973-151">![FTP-művelet kép 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span><span class="sxs-lookup"><span data-stu-id="62973-151">![FTP action image 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span></span>   
7. <span data-ttu-id="62973-152">A vezérlő frissül, amely jelzi, hogy a **FTP - fájl tartalmának lekérdezése** művelet kap a *tartalom fájl* az új vagy módosított fájl az FTP-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="62973-152">The control is updated, indicating that the **FTP - Get file content** action will get the *file content* of the new or modified file on the FTP server.</span></span>      
   <span data-ttu-id="62973-153">![FTP-művelet kép 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span><span class="sxs-lookup"><span data-stu-id="62973-153">![FTP action image 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span></span>     
8. <span data-ttu-id="62973-154">Mentse a munkáját, majd adjon hozzá egy fájlt a munkafolyamat tesztelése az FTP-mappa.</span><span class="sxs-lookup"><span data-stu-id="62973-154">Save your work then add a file to the FTP folder to test your workflow.</span></span>    

<span data-ttu-id="62973-155">A logikai alkalmazást ezen a ponton úgy van konfigurálva, az FTP-kiszolgálón mappa megfigyelése és indítja a munkafolyamatot, ha egy új fájl vagy az FTP-kiszolgálón a módosított fájlt talál eseményindító.</span><span class="sxs-lookup"><span data-stu-id="62973-155">At this point, the logic app has been configured with a trigger to monitor a folder on an FTP server and initiate the workflow when it finds either a new file or a modified file on the FTP server.</span></span> 

<span data-ttu-id="62973-156">A logikai alkalmazás is van konfigurálva a művelet az új vagy módosított fájl tartalmának eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="62973-156">The logic app also has been configured with an action to get the contents of the new or modified file.</span></span>

<span data-ttu-id="62973-157">Mostantól hozzáadhatja azok egy másik művelet, mint a [SQL Server - sor beszúrása](connectors-create-api-sqlazure.md) lehet beszúrni az új vagy módosított fájl tartalmának az SQL-adatbázistáblában szereplő művelet.</span><span class="sxs-lookup"><span data-stu-id="62973-157">You can now add another action such as the [SQL Server - insert row](connectors-create-api-sqlazure.md) action to insert the contents of the new or modified file into a SQL database table.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="62973-158">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="62973-158">Connector-specific details</span></span>

<span data-ttu-id="62973-159">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/ftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="62973-159">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/ftpconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="62973-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62973-160">Next Steps</span></span>
[<span data-ttu-id="62973-161">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="62973-161">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

