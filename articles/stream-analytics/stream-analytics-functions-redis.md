---
title: "aaaStream elemzés valós idejű feldolgozással az Azure Functions |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse egy Azure-függvény csatlakozott a Service Bus-üzenetsorba, toopopulate egy egy Stream Analytics-feladat eredményének hello Azure Redis Cache."
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
ms.openlocfilehash: 5ef4fe76c2cadf896a80eeaf421f010c315918af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostore-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a><span data-ttu-id="34386-104">Hogyan Azure Stream Analytics egy Azure Redis Cache használata az Azure Functions a toostore adatait</span><span class="sxs-lookup"><span data-stu-id="34386-104">How toostore data from Azure Stream Analytics in an Azure Redis Cache using Azure Functions</span></span>
<span data-ttu-id="34386-105">Az Azure Stream Analytics lehetővé teszi gyors fejlesztésére és központi telepítése az alacsony költségű megoldások toogain valós idejű elemzése eszközök, érzékelőket, infrastruktúra, és alkalmazások vagy bármilyen streamet.</span><span class="sxs-lookup"><span data-stu-id="34386-105">Azure Stream Analytics lets you rapidly develop and deploy low-cost solutions toogain real-time insights from devices, sensors, infrastructure, and applications, or any stream of data.</span></span> <span data-ttu-id="34386-106">Ez lehetővé teszi a különböző használati esetek például valós idejű felügyeleti és figyelési parancs és vezérlő, csalások felderítéséhez, csatlakoztatott autók vagy sok más.</span><span class="sxs-lookup"><span data-stu-id="34386-106">It enables various use cases such as real-time management and monitoring, command and control, fraud detection, connected cars and many more.</span></span> <span data-ttu-id="34386-107">Ilyen helyzetekben érdemes lehet egy Azure Redis Cache-gyorsítótár például egy elosztott adattárba Azure Stream Analytics outputted toostore adatok.</span><span class="sxs-lookup"><span data-stu-id="34386-107">In many such scenarios, you may want toostore data outputted by Azure Stream Analytics into a distributed data store such as an Azure Redis cache.</span></span>

<span data-ttu-id="34386-108">Tegyük fel, hogy egy távközlési vállalati részét képezik.</span><span class="sxs-lookup"><span data-stu-id="34386-108">Suppose you are part of a telecommunications company.</span></span> <span data-ttu-id="34386-109">Ahol több érkező hívást hello ugyanazzal az identitással toodetect SIM csalás próbál, a hello azonos időben, de különböző földrajzi helyeken.</span><span class="sxs-lookup"><span data-stu-id="34386-109">You are trying toodetect SIM fraud where multiple calls coming from hello same identity, at hello same time, but in different geographically locations.</span></span> <span data-ttu-id="34386-110">Az összes hello lehetséges csalárd telefonhívásokat tárolása az Azure Redis cache-rendszer biztosítja.</span><span class="sxs-lookup"><span data-stu-id="34386-110">You are tasked with storing all hello potential fraudulent phone calls in an Azure Redis cache.</span></span> <span data-ttu-id="34386-111">Az ebben a blogban nyújtunk segítséget a hogyan könnyen befejezheti a feladatot.</span><span class="sxs-lookup"><span data-stu-id="34386-111">In this blog, we provide guidance on how you can easily complete your task.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="34386-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="34386-112">Prerequisites</span></span>
<span data-ttu-id="34386-113">Teljes hello [valós idejű csalások felderítéséhez] [ fraud-detection] segédlet az ASA</span><span class="sxs-lookup"><span data-stu-id="34386-113">Complete hello [Real-time Fraud Detection][fraud-detection] walk-through for ASA</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="34386-114">Architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="34386-114">Architecture Overview</span></span>
![Képernyőkép-architektúra](./media/stream-analytics-functions-redis/architecture-overview.png)

<span data-ttu-id="34386-116">Ahogy az ábra megelőző hello, Stream Analytics lehetővé teszi a bemeneti adatok toobe streaming lekérdezett és tooan kimeneti küldött.</span><span class="sxs-lookup"><span data-stu-id="34386-116">As shown in hello preceding figure, Stream Analytics allows streaming input data toobe queried and sent tooan output.</span></span> <span data-ttu-id="34386-117">Hello kimeneti alapján, az Azure Functions majd elindítható valamilyen esemény.</span><span class="sxs-lookup"><span data-stu-id="34386-117">Based on hello output, Azure Functions can then trigger some type of event.</span></span> 

<span data-ttu-id="34386-118">A blogban található Microsoft hello Azure Functions részét, ez az adatcsatorna összpontosítson, vagy pontosabban hello a csalárd adatokat tároló hello gyorsítótárba esemény váltanak.</span><span class="sxs-lookup"><span data-stu-id="34386-118">In this blog, we focus on hello Azure Functions part of this pipeline, or more specifically hello triggering of an event that stores fraudulent data into hello cache.</span></span>
<span data-ttu-id="34386-119">Hello befejezése után [valós idejű csalások felderítéséhez] [ fraud-detection] oktatóanyagban rendelkezik (az eseményközpontok) bemeneti, a lekérdezés és kimenetnek (blob-tároló) már konfigurálva és fusson.</span><span class="sxs-lookup"><span data-stu-id="34386-119">After completing hello [Real-time Fraud Detection][fraud-detection] tutorial, you have an input (an event hub), a query, and an output (blob storage) already configured and running.</span></span> <span data-ttu-id="34386-120">Az ebben a blogban módosítjuk hello kimeneti toouse a Service Bus-üzenetsorba helyette.</span><span class="sxs-lookup"><span data-stu-id="34386-120">In this blog, we change hello output toouse a Service Bus Queue instead.</span></span> <span data-ttu-id="34386-121">Ezt követően csatlakoztassa azt egy Azure-függvény toothis várólista.</span><span class="sxs-lookup"><span data-stu-id="34386-121">After that, we connect an Azure Function toothis queue.</span></span> 

## <a name="create-and-connect-a-service-bus-queue-output"></a><span data-ttu-id="34386-122">Hozzon létre, és csatlakozzon a Service Bus-üzenetsorba kimenete</span><span class="sxs-lookup"><span data-stu-id="34386-122">Create and connect a Service Bus Queue output</span></span>
<span data-ttu-id="34386-123">toocreate a Service Bus-üzenetsorba, hajtsa végre az 1. és 2 hello .NET szakasz [Ismerkedés a Service Bus-üzenetsorok][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="34386-123">toocreate a Service Bus Queue, follow steps 1 and 2 of hello .NET section in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span>
<span data-ttu-id="34386-124">Most tegyük csatlakozás hello várólista toohello Stream Analytics-feladat a hello létrehozott korábbi csalások észlelése segédlet.</span><span class="sxs-lookup"><span data-stu-id="34386-124">Now let's connect hello queue toohello Stream Analytics job that was created in hello earlier fraud detection walk-through.</span></span>

1. <span data-ttu-id="34386-125">A hello Azure-portálon, válassza a toohello **kimenetek** panel a feladatot, és válassza ki a **hozzáadása** hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="34386-125">In hello Azure portal, go toohello **Outputs** blade of your job and select **Add** at hello top of hello page.</span></span>
   
    ![Kimenet hozzáadása](./media/stream-analytics-functions-redis/adding-outputs.png)
2. <span data-ttu-id="34386-127">Válasszon **Service Bus-üzenetsorba** , hello **gyűjtése** és az üdvözlő képernyőt hello útmutatás.</span><span class="sxs-lookup"><span data-stu-id="34386-127">Choose **Service Bus Queue** as hello **Sink** and follow hello instructions on hello screen.</span></span> <span data-ttu-id="34386-128">A Service Bus-üzenetsorba hello meg arról, hogy toochoose hello névterekkel létrehozott [Ismerkedés a Service Bus-üzenetsorok][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="34386-128">Be sure toochoose hello namespace of hello Service Bus Queue you created in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span> <span data-ttu-id="34386-129">Ha elkészült, kattintson a hello "jobb oldali" gombra.</span><span class="sxs-lookup"><span data-stu-id="34386-129">Click hello "right" button when you are finished.</span></span>
3. <span data-ttu-id="34386-130">Adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="34386-130">Specify hello following values:</span></span>
   
   * <span data-ttu-id="34386-131">**Esemény szerializáló formátum**: JSON-ban</span><span class="sxs-lookup"><span data-stu-id="34386-131">**Event Serializer Format**: JSON</span></span>
   * <span data-ttu-id="34386-132">**Kódolás**: UTF8</span><span class="sxs-lookup"><span data-stu-id="34386-132">**Encoding**: UTF8</span></span>
   * <span data-ttu-id="34386-133">**FORMÁTUM**: sorral elválasztott</span><span class="sxs-lookup"><span data-stu-id="34386-133">**FORMAT**: Line separated</span></span>
4. <span data-ttu-id="34386-134">Kattintson a hello **létrehozása** tooadd gombra kattint, a forrás- és, hogy a Stream Analytics képes csatlakozni toohello tárfiók tooverify.</span><span class="sxs-lookup"><span data-stu-id="34386-134">Click hello **Create** button tooadd this source and tooverify that Stream Analytics can successfully connect toohello storage account.</span></span>
5. <span data-ttu-id="34386-135">A hello **lekérdezés** lapra, cserélje ki hello aktuális lekérdezés hello következőre.</span><span class="sxs-lookup"><span data-stu-id="34386-135">In hello **Query** tab, replace hello current query with hello following.</span></span> <span data-ttu-id="34386-136">Cserélje le * [YOUR SERVICE BUS NAME] * a 3. lépésben létrehozott hello kimeneti nevű.</span><span class="sxs-lookup"><span data-stu-id="34386-136">Replace *[YOUR SERVICE BUS NAME] * with hello output name you created in step 3.</span></span> 
   
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

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="34386-137">Azure Redis Cache létrehozása</span><span class="sxs-lookup"><span data-stu-id="34386-137">Create an Azure Redis Cache</span></span>
<span data-ttu-id="34386-138">Hozzon létre egy Azure Redis cache-hello .NET szakasz [hogyan tooUse Azure Redis Cache] [ use-rediscache] amíg hello szakasz nevű ***hello gyorsítótár-ügyfelek konfigurálása***.</span><span class="sxs-lookup"><span data-stu-id="34386-138">Create an Azure Redis cache by following hello .NET section in [How tooUse Azure Redis Cache][use-rediscache] until hello section called ***Configure hello cache clients***.</span></span>
<span data-ttu-id="34386-139">Művelet befejeződése után egy új Redis Cache rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="34386-139">Once complete, you have a new Redis Cache.</span></span> <span data-ttu-id="34386-140">A **összes beállítás**, jelölje be **hívóbetűk** és jegyezze fel a hello ***elsődleges kapcsolódási karakterlánc***.</span><span class="sxs-lookup"><span data-stu-id="34386-140">Under **All settings**, select **Access keys** and note down hello ***Primary connection string***.</span></span>

![Képernyőkép-architektúra](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a><span data-ttu-id="34386-142">Egy Azure-függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="34386-142">Create an Azure Function</span></span>
<span data-ttu-id="34386-143">Hajtsa végre a [az első Azure-függvény létrehozása] [ functions-getstarted] az Azure Functions használatába oktatóanyag tooget.</span><span class="sxs-lookup"><span data-stu-id="34386-143">Follow [Create your first Azure Function][functions-getstarted] tutorial tooget started with Azure Functions.</span></span> <span data-ttu-id="34386-144">Ha már rendelkezik egy Azure függvény volna, például toouse, akkor hagyja ki azokat, amelyek túl[tooRedis gyorsítótár írása](#Writing-to-Redis-Cache)</span><span class="sxs-lookup"><span data-stu-id="34386-144">If you already have an Azure function you would like toouse, then skip ahead too[Writing tooRedis Cache](#Writing-to-Redis-Cache)</span></span>

1. <span data-ttu-id="34386-145">Hello portálon alkalmazásszolgáltatások hello bal oldali navigációs sávon válassza az Azure-függvény app name tooget toohello függvény app webhelyet.</span><span class="sxs-lookup"><span data-stu-id="34386-145">In hello portal, select App Services from hello left-hand navigation, then click your Azure function app name tooget toohello Function's app website.</span></span>
    <span data-ttu-id="34386-146">![Képernyőkép a szolgáltatások függvény alkalmazáslistájában](./media/stream-analytics-functions-redis/app-services-function-list.png)</span><span class="sxs-lookup"><span data-stu-id="34386-146">![Screenshot of app services function list](./media/stream-analytics-functions-redis/app-services-function-list.png)</span></span>
2. <span data-ttu-id="34386-147">Kattintson a **új függvény > ServiceBusQueueTrigger – C#**.</span><span class="sxs-lookup"><span data-stu-id="34386-147">Click **New Function > ServiceBusQueueTrigger – C#**.</span></span> <span data-ttu-id="34386-148">Hello a következő mezőket, kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="34386-148">For hello following fields, follow these instructions:</span></span>
   
   * <span data-ttu-id="34386-149">**Várólista neve**: hello azonos nevet a hello sor létrehozásakor megadott hello névként [Ismerkedés a Service Bus-üzenetsorok] [ servicebus-getstarted] (nem hello neve hello a service bus).</span><span class="sxs-lookup"><span data-stu-id="34386-149">**Queue name**: hello same name as hello name you entered when you created hello queue in [Get Started with Service Bus Queues][servicebus-getstarted] (not hello name of hello service bus).</span></span> <span data-ttu-id="34386-150">Győződjön meg arról, amely a Stream Analytics kimeneti csatlakoztatott toohello hello várólista használja.</span><span class="sxs-lookup"><span data-stu-id="34386-150">Make sure you use hello queue that is connected toohello Stream Analytics output.</span></span>
   * <span data-ttu-id="34386-151">**A Service Bus kapcsolati**: válasszon **a kapcsolati karakterlánc hozzáadásának**.</span><span class="sxs-lookup"><span data-stu-id="34386-151">**Service Bus connection**: Select **Add a connection string**.</span></span> <span data-ttu-id="34386-152">toofind hello kapcsolati karakterláncot, nyissa meg toohello klasszikus portálra, válassza ki **Service Bus**, hozott létre, a service bus hello és **KAPCSOLATADATOK** üdvözlő képernyőt hello alján.</span><span class="sxs-lookup"><span data-stu-id="34386-152">toofind hello connection string, go toohello classic portal, select **Service Bus**, hello service bus you created, and **CONNECTION INFORMATION** at hello bottom of hello screen.</span></span> <span data-ttu-id="34386-153">Győződjön meg arról, hogy a fő képernyőn hello ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="34386-153">Make sure you are on hello main screen on this page.</span></span> <span data-ttu-id="34386-154">Másolja és illessze be hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="34386-154">Copy and paste hello connection string.</span></span> <span data-ttu-id="34386-155">Érzi, hogy szabad tooenter bármely kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="34386-155">Feel free tooenter any connection name.</span></span>
     
       ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * <span data-ttu-id="34386-157">**AccessRights**: válasszon **kezelése**</span><span class="sxs-lookup"><span data-stu-id="34386-157">**AccessRights**: Choose **Manage**</span></span>
3. <span data-ttu-id="34386-158">Kattintson a **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="34386-158">Click **Create**</span></span>

## <a name="writing-tooredis-cache"></a><span data-ttu-id="34386-159">Írás tooRedis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="34386-159">Writing tooRedis Cache</span></span>
<span data-ttu-id="34386-160">Létrehoztunk egy Azure-függvény, amely egy Service Bus-üzenetsorba olvassa be most.</span><span class="sxs-lookup"><span data-stu-id="34386-160">We have now created an Azure Function that reads from a Service Bus Queue.</span></span> <span data-ttu-id="34386-161">Összes toodo marad, a függvény toowrite ezen adatok toohello Redis Cache használja.</span><span class="sxs-lookup"><span data-stu-id="34386-161">All that is left toodo is use our Function toowrite this data toohello Redis Cache.</span></span> 

1. <span data-ttu-id="34386-162">Válassza ki az újonnan létrehozott **ServiceBusQueueTrigger**, és kattintson a **Alkalmazásbeállítások működik** a hello jobb felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="34386-162">Select your newly created **ServiceBusQueueTrigger**, and click **Function app settings** on hello top right corner.</span></span> <span data-ttu-id="34386-163">Válassza ki **Ugrás tooApp szolgáltatás beállításaira > Beállítások > Alkalmazásbeállítások**</span><span class="sxs-lookup"><span data-stu-id="34386-163">Select **Go tooApp Service Settings > Settings > Application settings**</span></span>
2. <span data-ttu-id="34386-164">Kapcsolati karakterláncok szakasz hello, hozzon létre egy nevet a hello **neve** szakasz.</span><span class="sxs-lookup"><span data-stu-id="34386-164">In hello Connection strings section, create a name in hello **Name** section.</span></span> <span data-ttu-id="34386-165">Illessze be a hello talált hello elsődleges kapcsolati karakterláncot **Redis gyorsítótárat létrehozni** történő hello lépés **érték** szakasz.</span><span class="sxs-lookup"><span data-stu-id="34386-165">Paste hello primary connection string you found in hello **Create a Redis Cache** step into hello **Value** section.</span></span> <span data-ttu-id="34386-166">Válassza ki **egyéni** ahol felirat látható **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="34386-166">Select **Custom** where it says **SQL Database**.</span></span>
3. <span data-ttu-id="34386-167">Kattintson a **mentése** hello tetején.</span><span class="sxs-lookup"><span data-stu-id="34386-167">Click **Save** at hello top.</span></span>
   
    ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/function-connection-string.png)
4. <span data-ttu-id="34386-169">Most lépjen vissza toohello App Service-beállításokat, és válassza ki **eszközök > App Service-szerkesztő (előzetes verzió) > a > Lépjen**.</span><span class="sxs-lookup"><span data-stu-id="34386-169">Now go back toohello App Service Settings and select **Tools > App Service Editor (Preview) > On > Go**.</span></span>
   
    ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/app-service-editor.png)
5. <span data-ttu-id="34386-171">Egy tetszőleges szerkesztővel, hozzon létre egy JSON fájlt **project.json** a következő hello és tooyour helyi lemezre menti.</span><span class="sxs-lookup"><span data-stu-id="34386-171">In an editor of your choice, create a JSON file named **project.json** with hello following and save it tooyour local disk.</span></span>
   
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
6. <span data-ttu-id="34386-172">A feltöltés a függvény (nem WWWROOT) hello legfelső szintű könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="34386-172">Upload this file into hello root directory of your function (not WWWROOT).</span></span> <span data-ttu-id="34386-173">Nevű fájlba kell megjelennie **project.lock.json** jelennek meg automatikusan, erősítse meg, hogy Nuget hello csomagok "StackExchange.Redis" és "Newtonsoft.Json" importálva.</span><span class="sxs-lookup"><span data-stu-id="34386-173">You should see a file named **project.lock.json** automatically appear, confirming that hello Nuget packages “StackExchange.Redis” and “Newtonsoft.Json” have been imported.</span></span>
7. <span data-ttu-id="34386-174">A hello **run.csx** fájlt, cserélje le a következő kód hello hello előre generált kódot.</span><span class="sxs-lookup"><span data-stu-id="34386-174">In hello **run.csx** file, replace hello pre-generated code with hello following code.</span></span> <span data-ttu-id="34386-175">Hello lazyConnection függvényben, cserélje le az "Kapcs NAME" 2. lépésében létrehozott hello nevű **adattárolásra való hello Redis gyorsítótár**.</span><span class="sxs-lookup"><span data-stu-id="34386-175">In hello lazyConnection function, replace “CONN NAME” with hello name you created in step 2 of **Store data into hello Redis cache**.</span></span>

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers tooa property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract hello time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using hello cache object...
        // Simple put of integral data types into hello cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from hello cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect toohello Service Bus
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

## <a name="start-hello-stream-analytics-job"></a><span data-ttu-id="34386-176">Hello Stream Analytics-feladat indítása</span><span class="sxs-lookup"><span data-stu-id="34386-176">Start hello Stream Analytics job</span></span>
1. <span data-ttu-id="34386-177">Hello telcodatagen.exe alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="34386-177">Start hello telcodatagen.exe application.</span></span> <span data-ttu-id="34386-178">hello használata a következőképpen történik:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span><span class="sxs-lookup"><span data-stu-id="34386-178">hello usage is as follows: ````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span></span>
2. <span data-ttu-id="34386-179">A Stream Analytics-feladat panelen hello hello portálon kattintson az **Start** hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="34386-179">From hello Stream Analytics Job blade in hello portal, click **Start** at hello top of hello page.</span></span>
   
    ![Képernyőkép a indítási feladat](./media/stream-analytics-functions-redis/starting-job.png)
3. <span data-ttu-id="34386-181">A hello **indítási feladat** panelen megjelenő, válassza ki **most** majd hello **Start** üdvözlő képernyőt hello alján gombra.</span><span class="sxs-lookup"><span data-stu-id="34386-181">In hello **Start job** blade that appears, select **Now** and then click hello **Start** button at hello bottom of hello screen.</span></span> <span data-ttu-id="34386-182">hello feladat állapota tooStarting és egyes módosítások tooRunning időpont után.</span><span class="sxs-lookup"><span data-stu-id="34386-182">hello job status changes tooStarting and after some time changes tooRunning.</span></span>
   
    ![Képernyőkép a start feladat idő kiválasztása](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a><span data-ttu-id="34386-184">Futtassa a megoldás és eredmények ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="34386-184">Run solution and check results</span></span>
<span data-ttu-id="34386-185">Ha visszalép tooyour **ServiceBusQueueTrigger** lapon meg kell jelennie jelentkezzen utasításokat.</span><span class="sxs-lookup"><span data-stu-id="34386-185">Going back tooyour **ServiceBusQueueTrigger** page, you should now see log statements.</span></span> <span data-ttu-id="34386-186">Ezek a naplók megjelenítése hello Service Bus-üzenetsorba, tegye közzé hello adatbázis kapott azonosítóértékeket valami, és be szeretné olvasni hello időparaméterrel hello kulcsként ki!</span><span class="sxs-lookup"><span data-stu-id="34386-186">These logs show that you got something from hello Service Bus Queue, put it into hello database, and fetched it out using hello time as hello key!</span></span>

<span data-ttu-id="34386-187">amely az adatok a Redis gyorsítótár tooverify tooyour Redis gyorsítótár oldal hello új portálon lépjen (ahogy az előző hello [hozzon létre egy Azure Redis Cache](#Create-an-Azure-Redis-Cache) lépés), és válassza a konzolt.</span><span class="sxs-lookup"><span data-stu-id="34386-187">tooverify that your data is in your Redis cache, go tooyour Redis cache page in hello new portal (as shown in hello preceding [Create an Azure Redis Cache](#Create-an-Azure-Redis-Cache) step) and select Console.</span></span>

<span data-ttu-id="34386-188">Ezután írhat Redis parancsok tooconfirm, hogy adatokat valójában hello gyorsítótárában.</span><span class="sxs-lookup"><span data-stu-id="34386-188">Now you can write Redis commands tooconfirm that data is in fact in hello cache.</span></span>

![Képernyőkép a Redis-konzol](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a><span data-ttu-id="34386-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34386-190">Next steps</span></span>
<span data-ttu-id="34386-191">Hello új szempontot Azure Functions Izgatottan várja el a Stream analytics együtt teheti meg, és Reméljük, ez új lehetőségek oldja meg.</span><span class="sxs-lookup"><span data-stu-id="34386-191">We’re excited about hello new things Azure Functions and Stream analytics can do together, and we hope this unlocks new possibilities for you.</span></span> <span data-ttu-id="34386-192">Ha olyan visszajelzést, amit meg akar mellett a, érzi, hogy szabad toouse hello [Azure UserVoice webhelyén](https://feedback.azure.com/forums/270577-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="34386-192">If you have any feedback on what you want next, feel free toouse hello [Azure UserVoice site](https://feedback.azure.com/forums/270577-stream-analytics).</span></span>

<span data-ttu-id="34386-193">Ha új Microsoft Azure, a Microsoft meghívhatjuk tootry által végzett regisztrációt azt egy [ingyenes Azure próba-fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="34386-193">If you are new Microsoft Azure, we invite you tootry it out by signing up for a [free Azure trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="34386-194">Ha új tooStream Analytics áll, akkor azt meghívhatjuk túl[az első Stream Analytics-feladat létrehozása](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="34386-194">If you are new tooStream Analytics, then we invite you too[create your first Stream Analytics job](stream-analytics-create-a-job.md).</span></span>

<span data-ttu-id="34386-195">Ha bármelyik segítséget vagy kérdése van, írjon [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) vagy [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fórumok.</span><span class="sxs-lookup"><span data-stu-id="34386-195">If you need any help or have questions, post on [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) or [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forums.</span></span> 

<span data-ttu-id="34386-196">Azt is láthatja, hogy a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="34386-196">You can also see hello following resources:</span></span>

* [<span data-ttu-id="34386-197">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="34386-197">Azure Functions developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="34386-198">Az Azure Functions C# fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="34386-198">Azure Functions C# developer reference</span></span>](../azure-functions/functions-reference-csharp.md)
* [<span data-ttu-id="34386-199">Az Azure Functions F # fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="34386-199">Azure Functions F# developer reference</span></span>](../azure-functions/functions-reference-fsharp.md)
* [<span data-ttu-id="34386-200">Az Azure Functions NodeJS fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="34386-200">Azure Functions NodeJS developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="34386-201">Az Azure Functions eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="34386-201">Azure Functions triggers and bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
* [<span data-ttu-id="34386-202">Hogyan toomonitor Azure Redis Cache-gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="34386-202">How toomonitor Azure Redis Cache</span></span>](../redis-cache/cache-how-to-monitor.md)

<span data-ttu-id="34386-203">minden hello legújabb híreket és szolgáltatásokat, naprakész toostay kövesse [ @AzureStreaming ](https://twitter.com/AzureStreaming) a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="34386-203">toostay up-to-date on all hello latest news and features, follow [@AzureStreaming](https://twitter.com/AzureStreaming) on Twitter.</span></span>

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
