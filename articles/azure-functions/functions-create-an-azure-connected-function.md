---
title: "egy függvény, amely a tooAzure szolgáltatások aaaCreate |} Microsoft Docs"
description: "Használja az Azure Functions toocreate egy kiszolgáló nélküli alkalmazást, amely a tooother Azure szolgáltatások."
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
ms.openlocfilehash: 9d1f7d3b236f8d2c1a404c76aee410f6d458fb7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-functions-toocreate-a-function-that-connects-tooother-azure-services"></a><span data-ttu-id="d45fe-104">Használja az Azure Functions toocreate egy függvénynek, amely a tooother Azure szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d45fe-104">Use Azure Functions toocreate a function that connects tooother Azure services</span></span>

<span data-ttu-id="d45fe-105">Ez a témakör bemutatja, hogyan toocreate egy függvényt, amely figyeli a egy Azure Storage várólista és másolatok hello toomessages Azure Functions üzenetek toorows egy Azure Storage táblában.</span><span class="sxs-lookup"><span data-stu-id="d45fe-105">This topic shows you how toocreate a function in Azure Functions that listens toomessages on an Azure Storage queue and copies hello messages toorows in an Azure Storage table.</span></span> <span data-ttu-id="d45fe-106">Egy időzítő indított függvény használt tooload üzenetek hello várólistán.</span><span class="sxs-lookup"><span data-stu-id="d45fe-106">A timer triggered function is used tooload messages into hello queue.</span></span> <span data-ttu-id="d45fe-107">Egy második funkció hello várólistából beolvassa és üzenetek toohello tábla.</span><span class="sxs-lookup"><span data-stu-id="d45fe-107">A second function reads from hello queue and writes messages toohello table.</span></span> <span data-ttu-id="d45fe-108">Hello várólista és a hello tábla jön létre az Azure Functions hello kötés definíciók alapján.</span><span class="sxs-lookup"><span data-stu-id="d45fe-108">Both hello queue and hello table are created for you by Azure Functions based on hello binding definitions.</span></span> 

<span data-ttu-id="d45fe-109">toomake dolgot ennél is érdekesebb megoldást, egy függvény JavaScript nyelven írt van, és más hello parancsfájl C# nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="d45fe-109">toomake things more interesting, one function is written in JavaScript and hello other is written in C# script.</span></span> <span data-ttu-id="d45fe-110">Ez bemutatja, hogy egy függvényalkalmazás különféle nyelveken írt függvényekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="d45fe-110">This demonstrates how a function app can have functions in various languages.</span></span> 

<span data-ttu-id="d45fe-111">Ezt a forgatókönyvet a [Channel 9 blog egyik videója](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player) mutatja be.</span><span class="sxs-lookup"><span data-stu-id="d45fe-111">You can see this scenario demonstrated in a [video on Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span></span>

## <a name="create-a-function-that-writes-toohello-queue"></a><span data-ttu-id="d45fe-112">Egy függvény írja az toohello várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="d45fe-112">Create a function that writes toohello queue</span></span>

<span data-ttu-id="d45fe-113">Tooa storage üzenetsorába csatlakozáskor toocreate hello üzenet-várólista betöltő függvényt kell.</span><span class="sxs-lookup"><span data-stu-id="d45fe-113">Before you can connect tooa storage queue, you need toocreate a function that loads hello message queue.</span></span> <span data-ttu-id="d45fe-114">A JavaScript a funkció egy időzítő indítófeltételt toohello üzenet-várólista 10 másodpercenként írja.</span><span class="sxs-lookup"><span data-stu-id="d45fe-114">This JavaScript function uses a timer trigger that writes a message toohello queue every 10 seconds.</span></span> <span data-ttu-id="d45fe-115">Ha még nem rendelkezik Azure-fiókra, tekintse meg a hello [próbálja az Azure Functions](https://functions.azure.com/try) tapasztal, vagy [az ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="d45fe-115">If you don't already have an Azure account, check out hello [Try Azure Functions](https://functions.azure.com/try) experience, or [create your free Azure account](https://azure.microsoft.com/free/).</span></span>

1. <span data-ttu-id="d45fe-116">Nyissa meg toohello Azure-portálon, és keresse meg a függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d45fe-116">Go toohello Azure portal and locate your function app.</span></span>

2. <span data-ttu-id="d45fe-117">Kattintson a **New Function** (Új függvény)  > **TimeTrigger - JavaScript** elemre.</span><span class="sxs-lookup"><span data-stu-id="d45fe-117">Click **New Function** > **TimerTrigger-JavaScript**.</span></span> 

3. <span data-ttu-id="d45fe-118">Hello függvény neve **FunctionsBindingsDemo1**, adja meg a cron-kifejezés érték `0/10 * * * * *` a **ütemezés**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d45fe-118">Name hello function **FunctionsBindingsDemo1**, enter a cron expression value of `0/10 * * * * *` for **Schedule**, and then click **Create**.</span></span>
   
    ![Időzítő által aktivált hozzáadása](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    <span data-ttu-id="d45fe-120">Létrehozott egy időzítő által aktivált függvényt, amely 10 másodpercenként fut.</span><span class="sxs-lookup"><span data-stu-id="d45fe-120">You have now created a timer triggered function that runs every 10 seconds.</span></span>

5. <span data-ttu-id="d45fe-121">A hello **Develop** lapra, majd **naplók** és hello naplóban hello tevékenységének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="d45fe-121">On hello **Develop** tab, click **Logs** and view hello activity in hello log.</span></span> <span data-ttu-id="d45fe-122">10 másodpercenként írt naplóbejegyzéseket láthat.</span><span class="sxs-lookup"><span data-stu-id="d45fe-122">You see a log entry written every 10 seconds.</span></span>
   
    ![Hello napló tooverify hello függvény works megtekintése](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a><span data-ttu-id="d45fe-124">Üzenetsor kimeneti kötésének hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d45fe-124">Add a message queue output binding</span></span>

1. <span data-ttu-id="d45fe-125">A hello **integráció** lapra, majd **új kimeneti** > **Azure Queue Storage** > **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="d45fe-125">On hello **Integrate** tab, choose **New Output** > **Azure Queue Storage** > **Select**.</span></span>

    ![Eseményindító időzítő függvény hozzáadása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. <span data-ttu-id="d45fe-127">Adja meg `myQueueItem` a **üzenet paraméternév** és `functions-bindings` a **várólista neve**, válasszon ki egy létező **fiók tárolókapcsolat** , vagy kattintson a **új** toocreate tárolási fiók kapcsolat, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d45fe-127">Enter `myQueueItem` for **Message parameter name** and `functions-bindings` for **Queue name**, select an existing **Storage account connection** or click **new** toocreate a storage account connection, and then click **Save**.</span></span>  

    ![Hello kimeneti kötése toohello tárolási üzenetsor létrehozása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. <span data-ttu-id="d45fe-129">Vissza a hello **Develop** lapon, a következő kód toohello függvény hello hozzáfűzése:</span><span class="sxs-lookup"><span data-stu-id="d45fe-129">Back in hello **Develop** tab, append hello following code toohello function:</span></span>
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. <span data-ttu-id="d45fe-130">Keresse meg a hello *Ha* utasítás körülbelül sor 9 hello függvény, és a Beszúrás hello következő kódot adott utasítás után.</span><span class="sxs-lookup"><span data-stu-id="d45fe-130">Locate hello *if* statement around line 9 of hello function, and insert hello following code after that statement.</span></span>
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    <span data-ttu-id="d45fe-131">Ez a kód létrehoz egy **myQueueItem** , és beállítja a **idő** tulajdonság toohello aktuális időbélyeg.</span><span class="sxs-lookup"><span data-stu-id="d45fe-131">This code creates a **myQueueItem** and sets its **time** property toohello current timeStamp.</span></span> <span data-ttu-id="d45fe-132">Hozzáadja hello új várólista elem toohello környezet által **myQueueItem** kötés.</span><span class="sxs-lookup"><span data-stu-id="d45fe-132">It then adds hello new queue item toohello context's **myQueueItem** binding.</span></span>

3. <span data-ttu-id="d45fe-133">Kattintson a **Mentés és futtatás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d45fe-133">Click **Save and Run**.</span></span>

## <a name="view-storage-updates-by-using-storage-explorer"></a><span data-ttu-id="d45fe-134">A tárterület frissítéseinek áttekintése a Storage Explorerrel</span><span class="sxs-lookup"><span data-stu-id="d45fe-134">View storage updates by using Storage Explorer</span></span>
<span data-ttu-id="d45fe-135">A funkció működésének létrehozott hello várólistában lévő üzenetek megtekintésével ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="d45fe-135">You can verify that your function is working by viewing messages in hello queue you created.</span></span>  <span data-ttu-id="d45fe-136">Tooyour storage üzenetsorába Cloud Explorerben a Visual Studio használatával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="d45fe-136">You can connect tooyour storage queue by using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="d45fe-137">Azonban hello portál segítségével könnyen tooconnect tooyour storage-fiók használatával a Microsoft Azure Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="d45fe-137">However, hello portal makes it easy tooconnect tooyour storage account by using Microsoft Azure Storage Explorer.</span></span>

1. <span data-ttu-id="d45fe-138">A hello **integráció** lapra, majd a várólista kimeneti kötése > **dokumentáció**, majd a tárfiók kapcsolati karakterlánc hello felfedése és hello értéket másol.</span><span class="sxs-lookup"><span data-stu-id="d45fe-138">In hello **Integrate** tab, click your queue output binding > **Documentation**, then unhide hello Connection String for your storage account and copy hello value.</span></span> <span data-ttu-id="d45fe-139">Az érték tooconnect tooyour tárfiókot használni.</span><span class="sxs-lookup"><span data-stu-id="d45fe-139">You use this value tooconnect tooyour storage account.</span></span>

    ![Az Azure Storage Explorer letöltése](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. <span data-ttu-id="d45fe-141">Ha még nem tette meg, töltse le és telepítse a [Microsoft Azure Storage Explorert](http://storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="d45fe-141">If you haven't already done so, download and install [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span></span> 
 
3. <span data-ttu-id="d45fe-142">A Tártallózó, kattintson a hello tooAzure Kiszolgálótárhely ikon csatlakozzon, illessze be a hello kapcsolati karakterláncot hello mezőben, és hello varázsló befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d45fe-142">In Storage Explorer, click hello connect tooAzure Storage icon, paste hello connection string in hello field, and complete hello wizard.</span></span>

    ![Storage Explorer – kapcsolat hozzáadása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. <span data-ttu-id="d45fe-144">A **helyi és csatlakoztatott**, bontsa ki a **Tárfiókok** > a tárfiók > **várólisták** > **funkciók-kötések**, és ellenőrizze, hogy üzenetek írt toohello várólista.</span><span class="sxs-lookup"><span data-stu-id="d45fe-144">Under **Local and attached**, expand **Storage Accounts** > your storage account > **Queues** > **functions-bindings** and verify that messages are written toohello queue.</span></span>

    ![Hello várólistában lévő üzenetek ábrázolása](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    <span data-ttu-id="d45fe-146">Ha hello várólista nem létezik, vagy üres, valószínűleg probléma van a függvénykötés vagy kóddal.</span><span class="sxs-lookup"><span data-stu-id="d45fe-146">If hello queue does not exist or is empty, there is most likely a problem with your function binding or code.</span></span>

## <a name="create-a-function-that-reads-from-hello-queue"></a><span data-ttu-id="d45fe-147">Hello várólistából olvasó függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="d45fe-147">Create a function that reads from hello queue</span></span>

<span data-ttu-id="d45fe-148">Most, hogy rendelkezik a hozzáadni kívánt toohello várólista üzenetek, létrehozhat egy másik függvényen olvasó hello várólistából, és az írási műveletek hello üzenetek véglegesen tooan Azure Storage tábla.</span><span class="sxs-lookup"><span data-stu-id="d45fe-148">Now that you have messages being added toohello queue, you can create another function that reads from hello queue and writes hello messages permanently tooan Azure Storage table.</span></span>

1. <span data-ttu-id="d45fe-149">Kattintson a **New Function** (Új függvény) > **QueueTrigger-CSharp** elemre.</span><span class="sxs-lookup"><span data-stu-id="d45fe-149">Click **New Function** > **QueueTrigger-CSharp**.</span></span> 
 
2. <span data-ttu-id="d45fe-150">Hello függvény neve `FunctionsBindingsDemo2`, adja meg **funkciók-kötések** a hello **üzenetsornév** mezőben válasszon egy meglévő tárfiókot, vagy hozzon létre egyet, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d45fe-150">Name hello function `FunctionsBindingsDemo2`, enter **functions-bindings** in hello **Queue name** field, select an existing storage account or create one, and then click **Create**.</span></span>

    ![Kimeneti üzenetsor időzítő függvényének hozzáadása](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. <span data-ttu-id="d45fe-152">(Választható) Azt, hogy működik-e új függvény hello Tártallózó, mielőtt új várólista hello megtekintésével ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="d45fe-152">(Optional) You can verify that hello new function works by viewing hello new queue in Storage Explorer as before.</span></span> <span data-ttu-id="d45fe-153">A Visual Studio Cloud Explorer eszközét is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d45fe-153">You can also use Cloud Explorer in Visual Studio.</span></span>  

4. <span data-ttu-id="d45fe-154">(Választható) Frissítse a hello **funkciók-kötések** várólistára, és figyelje meg, hogy a cikkek hello várólistából eltávolították.</span><span class="sxs-lookup"><span data-stu-id="d45fe-154">(Optional) Refresh hello **functions-bindings** queue and notice that items have been removed from hello queue.</span></span> <span data-ttu-id="d45fe-155">hello eltávolítását okozza, hogy hello függvény kötött toohello **funkciók-kötések** várólistára, egy bemeneti eseményindító és hello függvény hello várólista olvassa be.</span><span class="sxs-lookup"><span data-stu-id="d45fe-155">hello removal occurs because hello function is bound toohello **functions-bindings** queue as an input trigger and hello function reads hello queue.</span></span> 
 
## <a name="add-a-table-output-binding"></a><span data-ttu-id="d45fe-156">Tábla kimeneti kötésének hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d45fe-156">Add a table output binding</span></span>

1. <span data-ttu-id="d45fe-157">A FunctionsBindingsDemo2 részen kattintson az **Integrate** (Integrálás) > **New Output** (Új kimenet) > **Azure Table Storage** > **Select** (Kiválasztás) elemre.</span><span class="sxs-lookup"><span data-stu-id="d45fe-157">In FunctionsBindingsDemo2, click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

    ![A kötés tooan Azure Storage tábla hozzáadása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. <span data-ttu-id="d45fe-159">Adja meg a `functionbindings` értéket a **Table name** (Tábla neve), illetve a `myTable` értéket a **Table parameter name** (Táblaparaméter neve) beállítás számára, válasszon ki egy meglévő kapcsolatot a **Storage account connection** (Tárfiókkapcsolat) mezőben, vagy hozzon létre egy új kapcsolatot, majd kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d45fe-159">Enter `functionbindings` for **Table name** and `myTable` for **Table parameter name**, choose a **Storage account connection** or create a new one, and then click **Save**.</span></span>

    ![Hello tárolási táblakötéssel konfigurálása](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. <span data-ttu-id="d45fe-161">A hello **Develop** lapon, hogy lecseréli a meglévő funkciókódot hello hello következőre:</span><span class="sxs-lookup"><span data-stu-id="d45fe-161">In hello **Develop** tab, replace hello existing function code with hello following:</span></span>
   
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
        
        // Add hello item toohello table binding collection.
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
    <span data-ttu-id="d45fe-162">Hello **TableItem** osztály hello tárolási tábla sorának jelöli, és hozzáadhat hello elem toohello `myTable` gyűjteménye **TableItem** objektumok.</span><span class="sxs-lookup"><span data-stu-id="d45fe-162">hello **TableItem** class represents a row in hello storage table, and you add hello item toohello `myTable` collection of **TableItem** objects.</span></span> <span data-ttu-id="d45fe-163">Meg kell adni a hello **PartitionKey** és **RowKey** tulajdonságok toobe képes tooinsert hello táblába.</span><span class="sxs-lookup"><span data-stu-id="d45fe-163">You must set hello **PartitionKey** and **RowKey** properties toobe able tooinsert into hello table.</span></span>

4. <span data-ttu-id="d45fe-164">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d45fe-164">Click **Save**.</span></span>  <span data-ttu-id="d45fe-165">Végezetül hello függvény works ellenőrizheti Tártallózóval vagy a Visual Studio Cloud Explorer hello tábla megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="d45fe-165">Finally, you can verify hello function works by viewing hello table in Storage explorer or Visual Studio Cloud Explorer.</span></span>

5. <span data-ttu-id="d45fe-166">(Választható) Bontsa ki a tárfiók a Tártallózó **táblák** > **functionsbindings** , és ellenőrizze, hogy a sorok hozzáadásakor toohello tábla.</span><span class="sxs-lookup"><span data-stu-id="d45fe-166">(Optional) In your storage account in Storage Explorer, expand **Tables** > **functionsbindings** and verify that rows are added toohello table.</span></span> <span data-ttu-id="d45fe-167">Mindent hello ugyanaz a Cloud Explorerben a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="d45fe-167">You can do hello same in Cloud Explorer in Visual Studio.</span></span>

    ![Hello tábla sorainak ábrázolása](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    <span data-ttu-id="d45fe-169">Ha hello tábla nem létezik, vagy üres, valószínűleg probléma van a függvénykötés vagy kóddal.</span><span class="sxs-lookup"><span data-stu-id="d45fe-169">If hello table does not exist or is empty, there is most likely a problem with your function binding or code.</span></span> 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a><span data-ttu-id="d45fe-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d45fe-170">Next steps</span></span>
<span data-ttu-id="d45fe-171">Az Azure Functions kapcsolatos további információkért tekintse meg a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="d45fe-171">For more information about Azure Functions, see hello following topics:</span></span>

* [<span data-ttu-id="d45fe-172">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="d45fe-172">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="d45fe-173">Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="d45fe-173">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="d45fe-174">Az Azure Functions tesztelése</span><span class="sxs-lookup"><span data-stu-id="d45fe-174">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="d45fe-175">Különböző függvénytesztelési eszközöket és technikákat ír le.</span><span class="sxs-lookup"><span data-stu-id="d45fe-175">Describes various tools and techniques for testing your functions.</span></span>
* [<span data-ttu-id="d45fe-176">Hogyan tooscale Azure Functions</span><span class="sxs-lookup"><span data-stu-id="d45fe-176">How tooscale Azure Functions</span></span>](functions-scale.md)  
  <span data-ttu-id="d45fe-177">Ismerteti az Azure Functions, beleértve a hello fogyasztás üzemeltetési terv, és hogyan toochoose hello megfelelő csomag elérhető service-csomagokról.</span><span class="sxs-lookup"><span data-stu-id="d45fe-177">Discusses service plans available with Azure Functions, including hello Consumption hosting plan, and how toochoose hello right plan.</span></span> 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

