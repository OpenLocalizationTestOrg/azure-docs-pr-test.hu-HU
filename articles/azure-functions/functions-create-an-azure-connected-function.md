---
title: "Az Azure-szolgáltatásokhoz csatlakozó függvény létrehozása | Microsoft Docs"
description: "Az Azure Functions segítségével létrehozhat egy kiszolgáló nélküli alkalmazást, amely más Azure-szolgáltatásokhoz kapcsolódik."
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 65964a322f0adab4f648fb350bedb77b46bf9054
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-functions-to-create-a-function-that-connects-to-other-azure-services"></a><span data-ttu-id="1d5c3-104">Az Azure Functions segítségével létrehozhat egy függvényt, amely más Azure-szolgáltatásokhoz kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-104">Use Azure Functions to create a function that connects to other Azure services</span></span>

<span data-ttu-id="1d5c3-105">Ez a témakör olyan függvény létrehozását mutatja be az Azure Functionsben, amely egy Azure Storage-üzenetsor üzeneteit figyeli, és egy Azure Storage-táblába másolja azokat.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-105">This topic shows you how to create a function in Azure Functions that listens to messages on an Azure Storage queue and copies the messages to rows in an Azure Storage table.</span></span> <span data-ttu-id="1d5c3-106">Az üzeneteknek az üzenetsorba való betöltése egy időzítő által aktivált függvénnyel történik.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-106">A timer triggered function is used to load messages into the queue.</span></span> <span data-ttu-id="1d5c3-107">Egy másik függvény beolvassa az üzeneteket az üzenetsorból, és a táblába írja azokat.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-107">A second function reads from the queue and writes messages to the table.</span></span> <span data-ttu-id="1d5c3-108">Az üzenetsort és a táblát az Azure Functions hozza létre a kötési meghatározások alapján.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-108">Both the queue and the table are created for you by Azure Functions based on the binding definitions.</span></span> 

<span data-ttu-id="1d5c3-109">A dolgokat még érdekesebbé teszi, az egyik függvény JavaScript, a másik pedig C# nyelven lett megírva.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-109">To make things more interesting, one function is written in JavaScript and the other is written in C# script.</span></span> <span data-ttu-id="1d5c3-110">Ez bemutatja, hogy egy függvényalkalmazás különféle nyelveken írt függvényekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-110">This demonstrates how a function app can have functions in various languages.</span></span> 

<span data-ttu-id="1d5c3-111">Ezt a forgatókönyvet a [Channel 9 blog egyik videója](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player) mutatja be.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-111">You can see this scenario demonstrated in a [video on Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span></span>

## <a name="create-a-function-that-writes-to-the-queue"></a><span data-ttu-id="1d5c3-112">Az üzenetsorba író függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d5c3-112">Create a function that writes to the queue</span></span>

<span data-ttu-id="1d5c3-113">Mielőtt egy tárterületi üzenetsorhoz kapcsolódna, létre kell hoznia egy függvényt, amely betölti az üzenetsort.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-113">Before you can connect to a storage queue, you need to create a function that loads the message queue.</span></span> <span data-ttu-id="1d5c3-114">Ez a JavaScript-függvény időzítő aktiválót használ, amely 10 másodpercenként üzenetet ír a sorba.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-114">This JavaScript function uses a timer trigger that writes a message to the queue every 10 seconds.</span></span> <span data-ttu-id="1d5c3-115">Ha még nem rendelkezik Azure-fiókkal, keresse fel az [Azure Functions kipróbálását](https://functions.azure.com/try) lehetővé tévő webhelyet, vagy [hozzon létre egy ingyenes Azure-fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1d5c3-115">If you don't already have an Azure account, check out the [Try Azure Functions](https://functions.azure.com/try) experience, or [create your free Azure account](https://azure.microsoft.com/free/).</span></span>

1. <span data-ttu-id="1d5c3-116">Nyissa meg az Azure Portalt, és keresse meg a függvényalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-116">Go to the Azure portal and locate your function app.</span></span>

2. <span data-ttu-id="1d5c3-117">Kattintson a **New Function** (Új függvény)  > **TimeTrigger - JavaScript** elemre.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-117">Click **New Function** > **TimerTrigger-JavaScript**.</span></span> 

3. <span data-ttu-id="1d5c3-118">Adja a függvénynek a **FunctionsBindingsDemo1** nevet, adja meg a `0/10 * * * * *` cron kifejezésértéket a **Schedule** (Ütemezés) számára, és kattintson a **Create** (Létrehozás) elemre.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-118">Name the function **FunctionsBindingsDemo1**, enter a cron expression value of `0/10 * * * * *` for **Schedule**, and then click **Create**.</span></span>
   
    ![Időzítő által aktivált hozzáadása](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    <span data-ttu-id="1d5c3-120">Létrehozott egy időzítő által aktivált függvényt, amely 10 másodpercenként fut.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-120">You have now created a timer triggered function that runs every 10 seconds.</span></span>

5. <span data-ttu-id="1d5c3-121">A **Develop** (Fejlesztés) lapon kattintson a **Logs** (Naplók) elemre, és tekintse meg a tevékenységet a naplóban.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-121">On the **Develop** tab, click **Logs** and view the activity in the log.</span></span> <span data-ttu-id="1d5c3-122">10 másodpercenként írt naplóbejegyzéseket láthat.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-122">You see a log entry written every 10 seconds.</span></span>
   
    ![A napló megtekintésével ellenőrizze a függvény működését](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a><span data-ttu-id="1d5c3-124">Üzenetsor kimeneti kötésének hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d5c3-124">Add a message queue output binding</span></span>

1. <span data-ttu-id="1d5c3-125">Az **Integrate** (Integrálás) lapon válassza a **New Output** (Új kimenet) > **Azure Queue Storage** > **Select** (Kiválasztás) elemet.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-125">On the **Integrate** tab, choose **New Output** > **Azure Queue Storage** > **Select**.</span></span>

    ![Eseményindító időzítő függvény hozzáadása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. <span data-ttu-id="1d5c3-127">Adja meg a `myQueueItem` értéket a **Message parameter name** (Üzenetparaméter neve), illetve a `functions-bindings` értéket a **Queue name** (Üzenetsor neve) beállítás számára, válasszon ki egy meglévő kapcsolatot a **Storage account connection** (Tárfiókkapcsolat) mezőben, vagy kattintson a **new** (új) elemre egy tárfiókkapcsolat létrehozásához, majd kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-127">Enter `myQueueItem` for **Message parameter name** and `functions-bindings` for **Queue name**, select an existing **Storage account connection** or click **new** to create a storage account connection, and then click **Save**.</span></span>  

    ![Kimeneti kötés létrehozása a tárterület üzenetsorával](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. <span data-ttu-id="1d5c3-129">A **Develop** (Fejlesztés) lapon egészítse ki a függvényt a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="1d5c3-129">Back in the **Develop** tab, append the following code to the function:</span></span>
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. <span data-ttu-id="1d5c3-130">Keresse meg az *if* utasítást a függvény 9. sorának közelében, és szúrja be az alábbi kódot az utasítás után.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-130">Locate the *if* statement around line 9 of the function, and insert the following code after that statement.</span></span>
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    <span data-ttu-id="1d5c3-131">Ez a kód egy **myQueueItem** elemet hoz létre, és az **idő** tulajdonságot az aktuális timeStamp értékére állítja.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-131">This code creates a **myQueueItem** and sets its **time** property to the current timeStamp.</span></span> <span data-ttu-id="1d5c3-132">Ezután az új üzenetsorelemet a kontextus **myQueueItem** kötéséhez adja.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-132">It then adds the new queue item to the context's **myQueueItem** binding.</span></span>

3. <span data-ttu-id="1d5c3-133">Kattintson a **Mentés és futtatás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-133">Click **Save and Run**.</span></span>

## <a name="view-storage-updates-by-using-storage-explorer"></a><span data-ttu-id="1d5c3-134">A tárterület frissítéseinek áttekintése a Storage Explorerrel</span><span class="sxs-lookup"><span data-stu-id="1d5c3-134">View storage updates by using Storage Explorer</span></span>
<span data-ttu-id="1d5c3-135">A függvény működését a létrehozott sorban található üzenetek megtekintésével ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-135">You can verify that your function is working by viewing messages in the queue you created.</span></span>  <span data-ttu-id="1d5c3-136">A Visual Studio Cloud Explorer eszközével kapcsolódhat a tárterület üzenetsorához.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-136">You can connect to your storage queue by using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="1d5c3-137">A portálon azonban egyszerűen kapcsolódhat a tárfiókhoz a Microsoft Azure Storage Explorerrel.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-137">However, the portal makes it easy to connect to your storage account by using Microsoft Azure Storage Explorer.</span></span>

1. <span data-ttu-id="1d5c3-138">Az **Integrate** (Integrálás) lapon kattintson az üzenetsor kimeneti kötésére, majd a **Documentation** (Dokumentáció) elemre, jelenítse meg tárfiók kapcsolati karakterláncát, és másolja az értéket.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-138">In the **Integrate** tab, click your queue output binding > **Documentation**, then unhide the Connection String for your storage account and copy the value.</span></span> <span data-ttu-id="1d5c3-139">Ezt az értéket használja a tárfiókhoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-139">You use this value to connect to your storage account.</span></span>

    ![Az Azure Storage Explorer letöltése](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. <span data-ttu-id="1d5c3-141">Ha még nem tette meg, töltse le és telepítse a [Microsoft Azure Storage Explorert](http://storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="1d5c3-141">If you haven't already done so, download and install [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span></span> 
 
3. <span data-ttu-id="1d5c3-142">A Storage Explorerben kattintson az Azure Storage-hoz való kapcsolódásra szolgáló ikonra, illessze be a kapcsolati karakterláncot a mezőbe, és haladjon végig a varázslón.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-142">In Storage Explorer, click the connect to Azure Storage icon, paste the connection string in the field, and complete the wizard.</span></span>

    ![Storage Explorer – kapcsolat hozzáadása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. <span data-ttu-id="1d5c3-144">A **Local and attached** (Helyi és csatolt) részen bontsa ki a **Storage Accounts** (Tárfiókok) > tárfiók > **Queues** (Üzenetsorok)  > **functions-bindings** csomópontot, és ellenőrizze, hogy az üzenetek írása az üzenetsorba megtörténik-e.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-144">Under **Local and attached**, expand **Storage Accounts** > your storage account > **Queues** > **functions-bindings** and verify that messages are written to the queue.</span></span>

    ![Az üzenetsorban szereplő üzenetek nézete](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    <span data-ttu-id="1d5c3-146">Ha az üzenetsor nem létezik vagy üres, nagy valószínűséggel a függvénykötéssel vagy a kóddal van probléma.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-146">If the queue does not exist or is empty, there is most likely a problem with your function binding or code.</span></span>

## <a name="create-a-function-that-reads-from-the-queue"></a><span data-ttu-id="1d5c3-147">Az üzenetsorból olvasó függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d5c3-147">Create a function that reads from the queue</span></span>

<span data-ttu-id="1d5c3-148">Most, hogy rendelkezik az üzenetsorhoz adott üzenetekkel, létrehozhat egy másik függvényt, amely az üzenetsorból olvas, és az üzeneteket véglegesen egy Azure Storage-táblába írja.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-148">Now that you have messages being added to the queue, you can create another function that reads from the queue and writes the messages permanently to an Azure Storage table.</span></span>

1. <span data-ttu-id="1d5c3-149">Kattintson a **New Function** (Új függvény) > **QueueTrigger-CSharp** elemre.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-149">Click **New Function** > **QueueTrigger-CSharp**.</span></span> 
 
2. <span data-ttu-id="1d5c3-150">Adja a függvénynek a `FunctionsBindingsDemo2` nevet, írja be a **functions-bindings** szöveget a **Queue name** (Üzenetsor neve) mezőbe, válasszon ki egy meglévő tárfiókot, vagy hozzon létre egyet, majd kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-150">Name the function `FunctionsBindingsDemo2`, enter **functions-bindings** in the **Queue name** field, select an existing storage account or create one, and then click **Create**.</span></span>

    ![Kimeneti üzenetsor időzítő függvényének hozzáadása](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. <span data-ttu-id="1d5c3-152">(Nem kötelező) Az új függvény működését az új üzenetsornak a Storage Explorerben való megtekintésével ellenőrizheti, ahogy korábban is.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-152">(Optional) You can verify that the new function works by viewing the new queue in Storage Explorer as before.</span></span> <span data-ttu-id="1d5c3-153">A Visual Studio Cloud Explorer eszközét is használhatja.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-153">You can also use Cloud Explorer in Visual Studio.</span></span>  

4. <span data-ttu-id="1d5c3-154">(Nem kötelező) Frissítse a **functions-bindings** üzenetsort, és figyelje meg, hogy elemek lettek eltávolítva az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-154">(Optional) Refresh the **functions-bindings** queue and notice that items have been removed from the queue.</span></span> <span data-ttu-id="1d5c3-155">Az eltávolítás azért történt, mert a függvény a **functions-bindings** üzenetsorhoz van kötve bemeneti eseményindítóként, és a függvény az üzenetsort olvassa.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-155">The removal occurs because the function is bound to the **functions-bindings** queue as an input trigger and the function reads the queue.</span></span> 
 
## <a name="add-a-table-output-binding"></a><span data-ttu-id="1d5c3-156">Tábla kimeneti kötésének hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d5c3-156">Add a table output binding</span></span>

1. <span data-ttu-id="1d5c3-157">A FunctionsBindingsDemo2 részen kattintson az **Integrate** (Integrálás) > **New Output** (Új kimenet) > **Azure Table Storage** > **Select** (Kiválasztás) elemre.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-157">In FunctionsBindingsDemo2, click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

    ![Kötés hozzáadása Azure Storage-táblához](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. <span data-ttu-id="1d5c3-159">Adja meg a `functionbindings` értéket a **Table name** (Tábla neve), illetve a `myTable` értéket a **Table parameter name** (Táblaparaméter neve) beállítás számára, válasszon ki egy meglévő kapcsolatot a **Storage account connection** (Tárfiókkapcsolat) mezőben, vagy hozzon létre egy új kapcsolatot, majd kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-159">Enter `functionbindings` for **Table name** and `myTable` for **Table parameter name**, choose a **Storage account connection** or create a new one, and then click **Save**.</span></span>

    ![A Storage-tábla kötésének konfigurálása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. <span data-ttu-id="1d5c3-161">A **Develop** (Fejlesztés) lapon cserélje a meglévő függvénykódot az alábbira:</span><span class="sxs-lookup"><span data-stu-id="1d5c3-161">In the **Develop** tab, replace the existing function code with the following:</span></span>
   
    ```cs
    
    using System;
    
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
        TableItem myItem = new TableItem
        {
            PartitionKey = "key",
            RowKey = Guid.NewGuid().ToString(),
            Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
            Msg = myQueueItem.Msg,
            OriginalTime = myQueueItem.Time    
        };
        
        // Add the item to the table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
    }
    
    public class TableItem
    {
        public string PartitionKey {get; set;}
        public string RowKey {get; set;}
        public string Time {get; set;}
        public string Msg {get; set;}
        public string OriginalTime {get; set;}
    }
    
    public class QItem
    {
        public string Msg { get; set;}
        public string Time { get; set;}
    }
    ```
    <span data-ttu-id="1d5c3-162">A **TableItem** osztály egy sort képvisel a tárolási táblában, és az elemet adja hozzá a **TableItem** objektumok `myTable` gyűjteményéhez.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-162">The **TableItem** class represents a row in the storage table, and you add the item to the `myTable` collection of **TableItem** objects.</span></span> <span data-ttu-id="1d5c3-163">A **PartitionKey** és a **RowKey** tulajdonságot be kell állítania a táblába való beszúrás lehetővé tételéhez.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-163">You must set the **PartitionKey** and **RowKey** properties to be able to insert into the table.</span></span>

4. <span data-ttu-id="1d5c3-164">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-164">Click **Save**.</span></span>  <span data-ttu-id="1d5c3-165">Végül, ellenőrizheti a függvény működését a Storage Explorerben vagy a Visual Studio Cloud Explorer eszközében.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-165">Finally, you can verify the function works by viewing the table in Storage explorer or Visual Studio Cloud Explorer.</span></span>

5. <span data-ttu-id="1d5c3-166">(Nem kötelező) A Storage Explorerben található tárfiókban bontsa ki a **Tables** (Táblák) > **functionsbindings** csomópontot, és ellenőrizze, hogy a sorok hozzáadása a táblához megtörtént-e.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-166">(Optional) In your storage account in Storage Explorer, expand **Tables** > **functionsbindings** and verify that rows are added to the table.</span></span> <span data-ttu-id="1d5c3-167">Ugyanezt megteheti a Visual Studio Cloud Explorer eszközében is.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-167">You can do the same in Cloud Explorer in Visual Studio.</span></span>

    ![A tábla sorainak nézete](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    <span data-ttu-id="1d5c3-169">Ha a tábla nem létezik vagy üres, nagy valószínűséggel a függvénykötéssel vagy a kóddal van probléma.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-169">If the table does not exist or is empty, there is most likely a problem with your function binding or code.</span></span> 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a><span data-ttu-id="1d5c3-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d5c3-170">Next steps</span></span>
<span data-ttu-id="1d5c3-171">Az Azure Functions használatával kapcsolatos további tudnivalókért tekintse át az alábbi témaköröket:</span><span class="sxs-lookup"><span data-stu-id="1d5c3-171">For more information about Azure Functions, see the following topics:</span></span>

* [<span data-ttu-id="1d5c3-172">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="1d5c3-172">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="1d5c3-173">Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-173">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="1d5c3-174">Az Azure Functions tesztelése</span><span class="sxs-lookup"><span data-stu-id="1d5c3-174">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="1d5c3-175">Különböző függvénytesztelési eszközöket és technikákat ír le.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-175">Describes various tools and techniques for testing your functions.</span></span>
* [<span data-ttu-id="1d5c3-176">Az Azure Functions méretezése</span><span class="sxs-lookup"><span data-stu-id="1d5c3-176">How to scale Azure Functions</span></span>](functions-scale.md)  
  <span data-ttu-id="1d5c3-177">Az Azure Functions szolgáltatáshoz elérhető szolgáltatáscsomagokat ismerteti, köztük a Használatalapú futtatási csomagot, és segít a megfelelő csomag kiválasztásában.</span><span class="sxs-lookup"><span data-stu-id="1d5c3-177">Discusses service plans available with Azure Functions, including the Consumption hosting plan, and how to choose the right plan.</span></span> 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

