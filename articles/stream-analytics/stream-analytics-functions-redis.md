---
title: "Adatfolyam-elemzés, valós idejű feldolgozása az Azure Functions |} Microsoft Docs"
description: "Megtudhatja, hogyan használható az Azure-függvény csatlakozott a Service Bus-üzenetsorba, egy Stream Analytics-feladat eredményének Azure Redis gyorsítótár adatokkal való feltöltése."
keywords: "adatfolyam esetében a redis cache service bus-üzenetsorba"
services: stream-analytics
author: ryancrawcour
manager: jhubbard
documentationcenter: 
ms.assetid: d428bb33-4244-4001-b93d-c77bed816527
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: ryancraw
ms.openlocfilehash: ad14cc858ff513573e2718a26a9ab5c524e1adc6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a><span data-ttu-id="4844b-104">Azure Stream Analytics adatok tárolása az az Azure Redis Cache Azure Functions használatával</span><span class="sxs-lookup"><span data-stu-id="4844b-104">How to store data from Azure Stream Analytics in an Azure Redis Cache using Azure Functions</span></span>
<span data-ttu-id="4844b-105">Az Azure Stream Analytics lehetővé teszi gyors fejlesztésére, és alacsony költségű megoldások ahhoz, hogy az eszközök, érzékelőket, infrastruktúra, és alkalmazások vagy bármilyen streamet valós idejű elemzése telepítését.</span><span class="sxs-lookup"><span data-stu-id="4844b-105">Azure Stream Analytics lets you rapidly develop and deploy low-cost solutions to gain real-time insights from devices, sensors, infrastructure, and applications, or any stream of data.</span></span> <span data-ttu-id="4844b-106">Ez lehetővé teszi a különböző használati esetek például valós idejű felügyeleti és figyelési parancs és vezérlő, csalások felderítéséhez, csatlakoztatott autók vagy sok más.</span><span class="sxs-lookup"><span data-stu-id="4844b-106">It enables various use cases such as real-time management and monitoring, command and control, fraud detection, connected cars and many more.</span></span> <span data-ttu-id="4844b-107">Ilyen helyzetekben érdemes lehet például egy Azure Redis cache-egy elosztott adattárba Azure Stream Analytics outputted adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="4844b-107">In many such scenarios, you may want to store data outputted by Azure Stream Analytics into a distributed data store such as an Azure Redis cache.</span></span>

<span data-ttu-id="4844b-108">Tegyük fel, hogy egy távközlési vállalati részét képezik.</span><span class="sxs-lookup"><span data-stu-id="4844b-108">Suppose you are part of a telecommunications company.</span></span> <span data-ttu-id="4844b-109">Ha több, ugyanazzal az identitással, ugyanazon érkező hívások idő, de különböző földrajzilag SIM csalások felderítésére kívánt helyét.</span><span class="sxs-lookup"><span data-stu-id="4844b-109">You are trying to detect SIM fraud where multiple calls coming from the same identity, at the same time, but in different geographically locations.</span></span> <span data-ttu-id="4844b-110">Az összes a potenciális csalárd telefonhívásokat tárolása az Azure Redis cache-rendszer biztosítja.</span><span class="sxs-lookup"><span data-stu-id="4844b-110">You are tasked with storing all the potential fraudulent phone calls in an Azure Redis cache.</span></span> <span data-ttu-id="4844b-111">Az ebben a blogban nyújtunk segítséget a hogyan könnyen befejezheti a feladatot.</span><span class="sxs-lookup"><span data-stu-id="4844b-111">In this blog, we provide guidance on how you can easily complete your task.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4844b-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4844b-112">Prerequisites</span></span>
<span data-ttu-id="4844b-113">Fejezze be a [valós idejű csalások felderítéséhez] [ fraud-detection] segédlet az ASA</span><span class="sxs-lookup"><span data-stu-id="4844b-113">Complete the [Real-time Fraud Detection][fraud-detection] walk-through for ASA</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="4844b-114">Architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="4844b-114">Architecture Overview</span></span>
![Képernyőkép-architektúra](./media/stream-analytics-functions-redis/architecture-overview.png)

<span data-ttu-id="4844b-116">A fenti ábrán látható, a Stream Analytics lehetővé teszi a bemeneti adatok lekérdezése és küldött egy kimeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="4844b-116">As shown in the preceding figure, Stream Analytics allows streaming input data to be queried and sent to an output.</span></span> <span data-ttu-id="4844b-117">A kimeneti alapján, az Azure Functions majd elindítható valamilyen esemény.</span><span class="sxs-lookup"><span data-stu-id="4844b-117">Based on the output, Azure Functions can then trigger some type of event.</span></span> 

<span data-ttu-id="4844b-118">Az ebben a blogban azt összpontosítanak az adatcsatornát az Azure Functions része, vagy pontosabban az időt. az esemény félrevezető adatokat tároló a gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="4844b-118">In this blog, we focus on the Azure Functions part of this pipeline, or more specifically the triggering of an event that stores fraudulent data into the cache.</span></span>
<span data-ttu-id="4844b-119">Miután befejezte a [valós idejű csalások felderítéséhez] [ fraud-detection] oktatóanyagban rendelkezik (az eseményközpontok) bemeneti, a lekérdezés és kimenetnek (blob-tároló) már konfigurálva és fusson.</span><span class="sxs-lookup"><span data-stu-id="4844b-119">After completing the [Real-time Fraud Detection][fraud-detection] tutorial, you have an input (an event hub), a query, and an output (blob storage) already configured and running.</span></span> <span data-ttu-id="4844b-120">Az ebben a blogban módosítjuk használja helyette a Service Bus-üzenetsorba kimenete.</span><span class="sxs-lookup"><span data-stu-id="4844b-120">In this blog, we change the output to use a Service Bus Queue instead.</span></span> <span data-ttu-id="4844b-121">Ezt követően nem csatlakozni az Azure-függvény ebből a várólistából.</span><span class="sxs-lookup"><span data-stu-id="4844b-121">After that, we connect an Azure Function to this queue.</span></span> 

## <a name="create-and-connect-a-service-bus-queue-output"></a><span data-ttu-id="4844b-122">Hozzon létre, és csatlakozzon a Service Bus-üzenetsorba kimenete</span><span class="sxs-lookup"><span data-stu-id="4844b-122">Create and connect a Service Bus Queue output</span></span>
<span data-ttu-id="4844b-123">Hozzon létre egy Service Bus-üzenetsorba, kövesse a lépéseket, 1. és 2. a .NET részt [Ismerkedés a Service Bus-üzenetsorok][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="4844b-123">To create a Service Bus Queue, follow steps 1 and 2 of the .NET section in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span>
<span data-ttu-id="4844b-124">Most tegyük a várólista csatlakozni a Stream Analytics-feladat, amely a korábbi csalások észlelése segédlet jött létre.</span><span class="sxs-lookup"><span data-stu-id="4844b-124">Now let's connect the queue to the Stream Analytics job that was created in the earlier fraud detection walk-through.</span></span>

1. <span data-ttu-id="4844b-125">Az Azure-portálon lépjen a **kimenetek** a feladat, és válassza ki a panel **Hozzáadás** az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="4844b-125">In the Azure portal, go to the **Outputs** blade of your job and select **Add** at the top of the page.</span></span>
   
    ![Kimenet hozzáadása](./media/stream-analytics-functions-redis/adding-outputs.png)
2. <span data-ttu-id="4844b-127">Válasszon **Service Bus-üzenetsorba** , a **gyűjtése** és kövesse a képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="4844b-127">Choose **Service Bus Queue** as the **Sink** and follow the instructions on the screen.</span></span> <span data-ttu-id="4844b-128">Ügyeljen arra, hogy válassza ki a Service Bus-üzenetsorba, a létrehozott névtér [Ismerkedés a Service Bus-üzenetsorok][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="4844b-128">Be sure to choose the namespace of the Service Bus Queue you created in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span> <span data-ttu-id="4844b-129">Ha elkészült, kattintson a "jobb oldali" gombra.</span><span class="sxs-lookup"><span data-stu-id="4844b-129">Click the "right" button when you are finished.</span></span>
3. <span data-ttu-id="4844b-130">Adja meg a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="4844b-130">Specify the following values:</span></span>
   
   * <span data-ttu-id="4844b-131">**Esemény szerializáló formátum**: JSON-ban</span><span class="sxs-lookup"><span data-stu-id="4844b-131">**Event Serializer Format**: JSON</span></span>
   * <span data-ttu-id="4844b-132">**Kódolás**: UTF8</span><span class="sxs-lookup"><span data-stu-id="4844b-132">**Encoding**: UTF8</span></span>
   * <span data-ttu-id="4844b-133">**FORMÁTUM**: sorral elválasztott</span><span class="sxs-lookup"><span data-stu-id="4844b-133">**FORMAT**: Line separated</span></span>
4. <span data-ttu-id="4844b-134">Kattintson a **létrehozása** gombra, adja hozzá a forrás, és győződjön meg arról, hogy a Stream Analytics sikeresen csatlakozott-e a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="4844b-134">Click the **Create** button to add this source and to verify that Stream Analytics can successfully connect to the storage account.</span></span>
5. <span data-ttu-id="4844b-135">Az a **lekérdezés** lapon, az aktuális lekérdezés cserélje le a következő.</span><span class="sxs-lookup"><span data-stu-id="4844b-135">In the **Query** tab, replace the current query with the following.</span></span> <span data-ttu-id="4844b-136">Cserélje le * [YOUR SERVICE BUS NAME] * a 3. lépésben létrehozott kimeneti névvel.</span><span class="sxs-lookup"><span data-stu-id="4844b-136">Replace *[YOUR SERVICE BUS NAME] * with the output name you created in step 3.</span></span> 
   
    ```    
   
        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
   
        INTO [YOUR SERVICE BUS NAME]
   
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
   
        WHERE CS1.SwitchNum != CS2.SwitchNum
   
    ```

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="4844b-137">Azure Redis Cache létrehozása</span><span class="sxs-lookup"><span data-stu-id="4844b-137">Create an Azure Redis Cache</span></span>
<span data-ttu-id="4844b-138">Hozzon létre egy Azure Redis Cache-gyorsítótár a .NET részt [használata Azure Redis Cache hogyan] [ use-rediscache] mindaddig, amíg a szakasz nevű ***a gyorsítótár-ügyfelek konfigurálása***.</span><span class="sxs-lookup"><span data-stu-id="4844b-138">Create an Azure Redis cache by following the .NET section in [How to Use Azure Redis Cache][use-rediscache] until the section called ***Configure the cache clients***.</span></span>
<span data-ttu-id="4844b-139">Művelet befejeződése után egy új Redis Cache rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4844b-139">Once complete, you have a new Redis Cache.</span></span> <span data-ttu-id="4844b-140">A **összes beállítás**, jelölje be **hívóbetűk** és jegyezze fel a ***elsődleges kapcsolódási karakterlánc***.</span><span class="sxs-lookup"><span data-stu-id="4844b-140">Under **All settings**, select **Access keys** and note down the ***Primary connection string***.</span></span>

![Képernyőkép-architektúra](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a><span data-ttu-id="4844b-142">Egy Azure-függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="4844b-142">Create an Azure Function</span></span>
<span data-ttu-id="4844b-143">Hajtsa végre a [az első Azure-függvény létrehozása] [ functions-getstarted] oktatóanyag az Azure Functions első lépéseiben.</span><span class="sxs-lookup"><span data-stu-id="4844b-143">Follow [Create your first Azure Function][functions-getstarted] tutorial to get started with Azure Functions.</span></span> <span data-ttu-id="4844b-144">Ha már rendelkezik egy Azure függvény használni szeretné, majd ugorjon előre [Redis Cache írása](#Writing-to-Redis-Cache)</span><span class="sxs-lookup"><span data-stu-id="4844b-144">If you already have an Azure function you would like to use, then skip ahead to [Writing to Redis Cache](#Writing-to-Redis-Cache)</span></span>

1. <span data-ttu-id="4844b-145">A portál a alkalmazásszolgáltatások válassza a bal oldali navigációs sávon, majd az Azure-függvény alkalmazásnév, a függvény app webhely eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4844b-145">In the portal, select App Services from the left-hand navigation, then click your Azure function app name to get to the Function's app website.</span></span>
    <span data-ttu-id="4844b-146">![Képernyőkép a szolgáltatások függvény alkalmazáslistájában](./media/stream-analytics-functions-redis/app-services-function-list.png)</span><span class="sxs-lookup"><span data-stu-id="4844b-146">![Screenshot of app services function list](./media/stream-analytics-functions-redis/app-services-function-list.png)</span></span>
2. <span data-ttu-id="4844b-147">Kattintson a **új függvény > ServiceBusQueueTrigger – C#**.</span><span class="sxs-lookup"><span data-stu-id="4844b-147">Click **New Function > ServiceBusQueueTrigger – C#**.</span></span> <span data-ttu-id="4844b-148">Az alábbi mezők kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="4844b-148">For the following fields, follow these instructions:</span></span>
   
   * <span data-ttu-id="4844b-149">**Várólista neve**: neve megegyezik a sor létrehozásakor megadott név [Ismerkedés a Service Bus-üzenetsorok] [ servicebus-getstarted] (nem a service bus neve).</span><span class="sxs-lookup"><span data-stu-id="4844b-149">**Queue name**: The same name as the name you entered when you created the queue in [Get Started with Service Bus Queues][servicebus-getstarted] (not the name of the service bus).</span></span> <span data-ttu-id="4844b-150">Ellenőrizze, hogy csatlakozik-e a Stream Analytics kimeneti várólista használja.</span><span class="sxs-lookup"><span data-stu-id="4844b-150">Make sure you use the queue that is connected to the Stream Analytics output.</span></span>
   * <span data-ttu-id="4844b-151">**A Service Bus kapcsolati**: válasszon **a kapcsolati karakterlánc hozzáadásának**.</span><span class="sxs-lookup"><span data-stu-id="4844b-151">**Service Bus connection**: Select **Add a connection string**.</span></span> <span data-ttu-id="4844b-152">Keresse meg a kapcsolati karakterláncot, lépjen a klasszikus portálra, válassza ki **Service Bus**, a service bus hozott létre, és **KAPCSOLATADATOK** a képernyő alján.</span><span class="sxs-lookup"><span data-stu-id="4844b-152">To find the connection string, go to the classic portal, select **Service Bus**, the service bus you created, and **CONNECTION INFORMATION** at the bottom of the screen.</span></span> <span data-ttu-id="4844b-153">Győződjön meg arról, hogy a fő képernyőn ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="4844b-153">Make sure you are on the main screen on this page.</span></span> <span data-ttu-id="4844b-154">Másolja és illessze be a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="4844b-154">Copy and paste the connection string.</span></span> <span data-ttu-id="4844b-155">Nyugodtan adjon meg a kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="4844b-155">Feel free to enter any connection name.</span></span>
     
       ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * <span data-ttu-id="4844b-157">**AccessRights**: válasszon **kezelése**</span><span class="sxs-lookup"><span data-stu-id="4844b-157">**AccessRights**: Choose **Manage**</span></span>
3. <span data-ttu-id="4844b-158">Kattintson a **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="4844b-158">Click **Create**</span></span>

## <a name="writing-to-redis-cache"></a><span data-ttu-id="4844b-159">Redis gyorsítótár írása</span><span class="sxs-lookup"><span data-stu-id="4844b-159">Writing to Redis Cache</span></span>
<span data-ttu-id="4844b-160">Létrehoztunk egy Azure-függvény, amely egy Service Bus-üzenetsorba olvassa be most.</span><span class="sxs-lookup"><span data-stu-id="4844b-160">We have now created an Azure Function that reads from a Service Bus Queue.</span></span> <span data-ttu-id="4844b-161">Ehhez marad csak a funkcióval a Redis Cache ezeket az adatokat írni.</span><span class="sxs-lookup"><span data-stu-id="4844b-161">All that is left to do is use our Function to write this data to the Redis Cache.</span></span> 

1. <span data-ttu-id="4844b-162">Válassza ki az újonnan létrehozott **ServiceBusQueueTrigger**, és kattintson a **Alkalmazásbeállítások működéséhez** a jobb felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="4844b-162">Select your newly created **ServiceBusQueueTrigger**, and click **Function app settings** on the top right corner.</span></span> <span data-ttu-id="4844b-163">Válassza ki **az App Service-beállítások > Beállítások > Alkalmazásbeállítások**</span><span class="sxs-lookup"><span data-stu-id="4844b-163">Select **Go to App Service Settings > Settings > Application settings**</span></span>
2. <span data-ttu-id="4844b-164">A kapcsolati karakterláncok szakaszban, hozzon létre egy nevet a a **neve** szakasz.</span><span class="sxs-lookup"><span data-stu-id="4844b-164">In the Connection strings section, create a name in the **Name** section.</span></span> <span data-ttu-id="4844b-165">Illessze be a található a elsődleges kapcsolati karakterláncot a **Redis gyorsítótárat létrehozni** lépjen be a **érték** szakasz.</span><span class="sxs-lookup"><span data-stu-id="4844b-165">Paste the primary connection string you found in the **Create a Redis Cache** step into the **Value** section.</span></span> <span data-ttu-id="4844b-166">Válassza ki **egyéni** ahol felirat látható **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="4844b-166">Select **Custom** where it says **SQL Database**.</span></span>
3. <span data-ttu-id="4844b-167">Kattintson a **mentése** tetején.</span><span class="sxs-lookup"><span data-stu-id="4844b-167">Click **Save** at the top.</span></span>
   
    ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/function-connection-string.png)
4. <span data-ttu-id="4844b-169">Most lépjen vissza a alkalmazás Service beállításai, és válassza ki **eszközök > App Service-szerkesztő (előzetes verzió) > a > Lépjen**.</span><span class="sxs-lookup"><span data-stu-id="4844b-169">Now go back to the App Service Settings and select **Tools > App Service Editor (Preview) > On > Go**.</span></span>
   
    ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/app-service-editor.png)
5. <span data-ttu-id="4844b-171">Egy tetszőleges szerkesztővel, hozzon létre egy JSON fájlt **project.json** a következőre, és a helyi lemezre menteni.</span><span class="sxs-lookup"><span data-stu-id="4844b-171">In an editor of your choice, create a JSON file named **project.json** with the following and save it to your local disk.</span></span>
   
        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }
6. <span data-ttu-id="4844b-172">Ez a fájl feltöltése a legfelső szintű könyvtárba, a függvény (nem WWWROOT).</span><span class="sxs-lookup"><span data-stu-id="4844b-172">Upload this file into the root directory of your function (not WWWROOT).</span></span> <span data-ttu-id="4844b-173">Nevű fájlba kell megjelennie **project.lock.json** jelennek meg automatikusan, erősítse meg, hogy a Nuget-csomagok "StackExchange.Redis" és "Newtonsoft.Json" importálta-e.</span><span class="sxs-lookup"><span data-stu-id="4844b-173">You should see a file named **project.lock.json** automatically appear, confirming that the Nuget packages “StackExchange.Redis” and “Newtonsoft.Json” have been imported.</span></span>
7. <span data-ttu-id="4844b-174">Az a **run.csx** fájlt, cserélje le az előre generált kódot az alábbira.</span><span class="sxs-lookup"><span data-stu-id="4844b-174">In the **run.csx** file, replace the pre-generated code with the following code.</span></span> <span data-ttu-id="4844b-175">A lazyConnection függvényben cserélje le az "Kapcs NAME" 2. lépésében létrehozott nevű **adatok tárolásához a Redis gyorsítótárba**.</span><span class="sxs-lookup"><span data-stu-id="4844b-175">In the lazyConnection function, replace “CONN NAME” with the name you created in step 2 of **Store data into the Redis cache**.</span></span>

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a><span data-ttu-id="4844b-176">A Stream Analytics-feladat indítása</span><span class="sxs-lookup"><span data-stu-id="4844b-176">Start the Stream Analytics job</span></span>
1. <span data-ttu-id="4844b-177">A telcodatagen.exe alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="4844b-177">Start the telcodatagen.exe application.</span></span> <span data-ttu-id="4844b-178">A használati a következőképpen történik:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span><span class="sxs-lookup"><span data-stu-id="4844b-178">The usage is as follows: ````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span></span>
2. <span data-ttu-id="4844b-179">A Stream Analytics-feladat paneljéről a portálon, kattintson a **Start** az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="4844b-179">From the Stream Analytics Job blade in the portal, click **Start** at the top of the page.</span></span>
   
    ![Képernyőkép a indítási feladat](./media/stream-analytics-functions-redis/starting-job.png)
3. <span data-ttu-id="4844b-181">Az a **indítási feladat** panelen megjelenő, válassza ki **most** , majd a **Start** gomb a képernyő alján.</span><span class="sxs-lookup"><span data-stu-id="4844b-181">In the **Start job** blade that appears, select **Now** and then click the **Start** button at the bottom of the screen.</span></span> <span data-ttu-id="4844b-182">A feladat állapotmódosítások indítása és futtatásának ideje módosításai után.</span><span class="sxs-lookup"><span data-stu-id="4844b-182">The job status changes to Starting and after some time changes to Running.</span></span>
   
    ![Képernyőkép a start feladat idő kiválasztása](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a><span data-ttu-id="4844b-184">Futtassa a megoldás és eredmények ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4844b-184">Run solution and check results</span></span>
<span data-ttu-id="4844b-185">Visszatér a **ServiceBusQueueTrigger** lapon meg kell jelennie jelentkezzen utasításokat.</span><span class="sxs-lookup"><span data-stu-id="4844b-185">Going back to your **ServiceBusQueueTrigger** page, you should now see log statements.</span></span> <span data-ttu-id="4844b-186">Ezek a naplók megjelenítése a Service Bus-üzenetsorba, tegye közzé az adatbázis kapott azonosítóértékeket valami, és be szeretné olvasni, az idő használata a kulcs!</span><span class="sxs-lookup"><span data-stu-id="4844b-186">These logs show that you got something from the Service Bus Queue, put it into the database, and fetched it out using the time as the key!</span></span>

<span data-ttu-id="4844b-187">Győződjön meg arról, hogy az adatok a Redis gyorsítótárt, lépjen a Redis gyorsítótár lap az új portálon (ahogy az előző [hozzon létre egy Azure Redis Cache](#Create-an-Azure-Redis-Cache) lépés), és válassza a konzolt.</span><span class="sxs-lookup"><span data-stu-id="4844b-187">To verify that your data is in your Redis cache, go to your Redis cache page in the new portal (as shown in the preceding [Create an Azure Redis Cache](#Create-an-Azure-Redis-Cache) step) and select Console.</span></span>

<span data-ttu-id="4844b-188">Ezután írhat Redis-parancsok futtatásával győződjön meg arról, hogy adatokat valójában a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="4844b-188">Now you can write Redis commands to confirm that data is in fact in the cache.</span></span>

![Képernyőkép a Redis-konzol](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a><span data-ttu-id="4844b-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4844b-190">Next steps</span></span>
<span data-ttu-id="4844b-191">A rendszer az Azure Functions új szempontot várakozással, és együtt teheti meg a Stream analytics Reméljük, ez új lehetőségek oldja meg.</span><span class="sxs-lookup"><span data-stu-id="4844b-191">We’re excited about the new things Azure Functions and Stream analytics can do together, and we hope this unlocks new possibilities for you.</span></span> <span data-ttu-id="4844b-192">Ha olyan visszajelzést, amit meg akar mellett a, szabadon használhatja a [Azure UserVoice webhelyén](https://feedback.azure.com/forums/270577-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="4844b-192">If you have any feedback on what you want next, feel free to use the [Azure UserVoice site](https://feedback.azure.com/forums/270577-stream-analytics).</span></span>

<span data-ttu-id="4844b-193">Ha új Microsoft Azure, azt hívhat meg, hogy próbálja ki által regisztrációt egy [ingyenes Azure próba-fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4844b-193">If you are new Microsoft Azure, we invite you to try it out by signing up for a [free Azure trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="4844b-194">Ha még nem ismeri a Stream Analytics, akkor kérjük [az első Stream Analytics-feladat létrehozása](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="4844b-194">If you are new to Stream Analytics, then we invite you to [create your first Stream Analytics job](stream-analytics-create-a-job.md).</span></span>

<span data-ttu-id="4844b-195">Ha bármelyik segítséget vagy kérdése van, írjon [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) vagy [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fórumok.</span><span class="sxs-lookup"><span data-stu-id="4844b-195">If you need any help or have questions, post on [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) or [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forums.</span></span> 

<span data-ttu-id="4844b-196">Megtekintheti a következőket is:</span><span class="sxs-lookup"><span data-stu-id="4844b-196">You can also see the following resources:</span></span>

* [<span data-ttu-id="4844b-197">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="4844b-197">Azure Functions developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="4844b-198">Az Azure Functions C# fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="4844b-198">Azure Functions C# developer reference</span></span>](../azure-functions/functions-reference-csharp.md)
* [<span data-ttu-id="4844b-199">Az Azure Functions F # fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="4844b-199">Azure Functions F# developer reference</span></span>](../azure-functions/functions-reference-fsharp.md)
* [<span data-ttu-id="4844b-200">Az Azure Functions NodeJS fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="4844b-200">Azure Functions NodeJS developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="4844b-201">Az Azure Functions eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="4844b-201">Azure Functions triggers and bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
* [<span data-ttu-id="4844b-202">Azure Redis Cache figyelése</span><span class="sxs-lookup"><span data-stu-id="4844b-202">How to monitor Azure Redis Cache</span></span>](../redis-cache/cache-how-to-monitor.md)

<span data-ttu-id="4844b-203">Ezek naprakész állapotát az összes a legújabb híreket és szolgáltatásokat, hajtsa végre a [ @AzureStreaming ](https://twitter.com/AzureStreaming) a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="4844b-203">To stay up-to-date on all the latest news and features, follow [@AzureStreaming](https://twitter.com/AzureStreaming) on Twitter.</span></span>

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
