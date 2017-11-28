---
title: "Azure IoT Hub eszköz-a-felhőbe küldött üzeneteket (Java) aaaProcess |} Microsoft Docs"
description: "Hogyan tooprocess IoT-központ eszközről a felhőbe üzenetek útválasztási szabályokat és egyéni végpontokat toodispatch üzenetek tooother háttér-szolgáltatásaihoz."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.openlocfilehash: 084e84e721ca4297c4d7d6cb06a43b0bed9bce85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-java"></a><span data-ttu-id="c76cd-103">Az IoT-központ eszközről a felhőbe üzenetek (Java)</span><span class="sxs-lookup"><span data-stu-id="c76cd-103">Process IoT Hub device-to-cloud messages (Java)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="c76cd-104">Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amely lehetővé teszi a megbízható és biztonságos kétirányú kommunikációs eszközök millióira és a megoldás közötti háttér.</span><span class="sxs-lookup"><span data-stu-id="c76cd-104">Azure IoT Hub is a fully managed service that enables reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="c76cd-105">Egyéb oktatóanyagok ([Ismerkedés az IoT-központ] és [IoT Hub-felhő eszközre üzenetek][lnk-c2d]) bemutatják, hogyan toouse hello alapvető eszköz-felhő és a felhő-eszköz az IoT-központ üzenetküldési működését.</span><span class="sxs-lookup"><span data-stu-id="c76cd-105">Other tutorials ([Get started with IoT Hub] and [Send cloud-to-device messages with IoT Hub][lnk-c2d]) show you how toouse hello basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span>

<span data-ttu-id="c76cd-106">Ez az oktatóanyag épít, hello hello kód [Ismerkedés az IoT-központ] oktatóanyag, és bemutatja, hogyan toouse üzenet skálázható módon útválasztási tooprocess eszközről a felhőbe üzenetek.</span><span class="sxs-lookup"><span data-stu-id="c76cd-106">This tutorial builds on hello code shown in hello [Get started with IoT Hub] tutorial, and shows you how toouse message routing tooprocess device-to-cloud messages in a scalable way.</span></span> <span data-ttu-id="c76cd-107">hello az oktatóanyag bemutatja, hogyan tooprocess üzenetek hello megoldásból azonnali beavatkozást igénylő háttér.</span><span class="sxs-lookup"><span data-stu-id="c76cd-107">hello tutorial illustrates how tooprocess messages that require immediate action from hello solution back end.</span></span> <span data-ttu-id="c76cd-108">Például egy eszköz előfordulhat, hogy küldése riasztás, amely elindítja a jegy beszúrása egy CRM-rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="c76cd-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="c76cd-109">Adatpont üzenetek ezzel szemben, egyszerűen hírcsatorna be egy elemzés.</span><span class="sxs-lookup"><span data-stu-id="c76cd-109">By contrast, data-point messages simply feed into an analytics engine.</span></span> <span data-ttu-id="c76cd-110">Például hőmérséklet telemetriai adatokat egy eszközről, amely tárolja a későbbi elemzési toobe az egy adatpont üzenet.</span><span class="sxs-lookup"><span data-stu-id="c76cd-110">For example, temperature telemetry from a device that is toobe stored for later analysis is a data-point message.</span></span>

<span data-ttu-id="c76cd-111">Ez az oktatóanyag végén hello három Java konzol alkalmazások futtatása:</span><span class="sxs-lookup"><span data-stu-id="c76cd-111">At hello end of this tutorial, you run three Java console apps:</span></span>

* <span data-ttu-id="c76cd-112">**Szimulált eszköz**, hello alkalmazás hello létrehozott egy módosított verziója [Ismerkedés az IoT-központ] oktatóanyagban adatpont eszközről a felhőbe üzeneteket küld másodpercenként, és interaktív eszközre felhő minden 10 üzenetek másodperc.</span><span class="sxs-lookup"><span data-stu-id="c76cd-112">**simulated-device**, a modified version of hello app created in hello [Get started with IoT Hub] tutorial, sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span> <span data-ttu-id="c76cd-113">Ez az alkalmazás az IoT-központ hello AMQP protokoll toocommunicate használ.</span><span class="sxs-lookup"><span data-stu-id="c76cd-113">This app uses hello AMQP protocol toocommunicate with IoT Hub.</span></span>
* <span data-ttu-id="c76cd-114">**olvasási-d2c-üzenetek** jeleníti meg az eszköz alkalmazás által küldött hello telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="c76cd-114">**read-d2c-messages** displays hello telemetry sent by your device app.</span></span>
* <span data-ttu-id="c76cd-115">**olvasási kritikus várólista-** távolít el kritikus köszönőüzenetei a hello Service Bus csatolt várólista toohello IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="c76cd-115">**read-critical-queue** de-queues hello critical messages from hello Service Bus queue attached toohello IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="c76cd-116">Az IoT-központ rendelkezik sok eszköz platformok és nyelvek, beleértve a C, Java és JavaScript SDK támogatása.</span><span class="sxs-lookup"><span data-stu-id="c76cd-116">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="c76cd-117">Hogyan tooreplace hello eszköz ebben az oktatóanyagban egy fizikai eszközzel kapcsolatos utasításokat és hogyan tooconnect eszközök tooan IoT Hub: hello [Azure IoT fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="c76cd-117">For instructions on how tooreplace hello device in this tutorial with a physical device, and how tooconnect devices tooan IoT Hub, see hello [Azure IoT Developer Center].</span></span>

<span data-ttu-id="c76cd-118">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c76cd-118">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="c76cd-119">A teljes működő verziójához hello [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c76cd-119">A complete working version of hello [Get started with IoT Hub] tutorial.</span></span>
* <span data-ttu-id="c76cd-120">hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="c76cd-120">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="c76cd-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="c76cd-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="c76cd-122">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="c76cd-122">An active Azure account.</span></span> <span data-ttu-id="c76cd-123">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot] [lnk-ingyenes] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="c76cd-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="c76cd-124">Néhány alapvető ismerete szükséges [Azure Storage] és [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="c76cd-124">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages-from-a-device-app"></a><span data-ttu-id="c76cd-125">Interaktív üzenetek küldése egy eszköz alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="c76cd-125">Send interactive messages from a device app</span></span>
<span data-ttu-id="c76cd-126">Ebben a szakaszban létrehozott hello hello eszközalkalmazás módosítása [Ismerkedés az IoT-központ] oktatóanyag toooccasionally azonnali feldolgozást igénylő üzenetek küldése.</span><span class="sxs-lookup"><span data-stu-id="c76cd-126">In this section, you modify hello device app you created in hello [Get started with IoT Hub] tutorial toooccasionally send messages that require immediate processing.</span></span>

1. <span data-ttu-id="c76cd-127">Szerkesztő tooopen hello simulated-device\src\main\java\com\mycompany\app\App.java szövegfájl használata.</span><span class="sxs-lookup"><span data-stu-id="c76cd-127">Use a text editor tooopen hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span> <span data-ttu-id="c76cd-128">Ez a fájl tartalmazza a hello hello kódját **szimulált eszköz** hello létrehozott alkalmazás [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c76cd-128">This file contains hello code for hello **simulated-device** app you created in hello [Get started with IoT Hub] tutorial.</span></span>

2. <span data-ttu-id="c76cd-129">Cserélje le a hello **MessageSender** hello kód a következő osztályra:</span><span class="sxs-lookup"><span data-stu-id="c76cd-129">Replace hello **MessageSender** class with hello following code:</span></span>

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        msgStr = "This is a critical message.";
                        msg = new Message(msgStr);
                        msg.setProperty("level", "critical");
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }
                    
                    System.out.println("Sending: " + msgStr);

                    Object lockobj = new Object();
                    EventCallback callback = new EventCallback();
                    client.sendEventAsync(msg, callback, lockobj);

                    synchronized (lockobj) {
                        lockobj.wait();
                    }
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                System.out.println("Finished.");
            }
        }
    }
    ```
   
    <span data-ttu-id="c76cd-130">Ez a metódus véletlenszerűen ad hello tulajdonság `"level": "critical"` hello eszközét, ezzel szimulálja egy üzenetet, amely azonnali beavatkozást igényel a háttérkiszolgálók alkalmazás hello által küldött toomessages.</span><span class="sxs-lookup"><span data-stu-id="c76cd-130">This method randomly adds hello property `"level": "critical"` toomessages sent by hello device, which simulates a message that requires immediate action by hello application back-end.</span></span> <span data-ttu-id="c76cd-131">hello alkalmazás információt átadja a hello üzenet tulajdonságai, ahelyett, hogy hello üzenet törzsében, úgy, hogy az IoT-központ megfelelő üzenet toohello hello üzenetcél irányíthatja.</span><span class="sxs-lookup"><span data-stu-id="c76cd-131">hello application passes this information in hello message properties, instead of in hello message body, so that IoT Hub can route hello message toohello proper message destination.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c76cd-132">Üzenet tulajdonságai tooroute üzenetek feldolgozásához, továbbá itt látható példa az toohello kiemelt útvonalra cold-elérési utat is több, különböző esetekre is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c76cd-132">You can use message properties tooroute messages for various scenarios including cold-path processing, in addition toohello hot path example shown here.</span></span>

2. <span data-ttu-id="c76cd-133">Mentse és zárja be hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="c76cd-133">Save and close hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c76cd-134">Hello szakét az egyszerűség Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="c76cd-134">For hello sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="c76cd-135">Az éles kódban, meg kell valósítania egy újrapróbálkozási házirendje, például az exponenciális leállítási hello MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="c76cd-135">In production code, you should implement a retry policy such as exponential backoff, as suggested in hello MSDN article [Transient Fault Handling].</span></span>

3. <span data-ttu-id="c76cd-136">toobuild hello **szimulált eszköz** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello szimulált eszköz mappában hello:</span><span class="sxs-lookup"><span data-stu-id="c76cd-136">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-tooyour-iot-hub-and-route-messages-tooit"></a><span data-ttu-id="c76cd-137">A várólista tooyour IoT hub és útvonal üzenetek tooit hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c76cd-137">Add a queue tooyour IoT hub and route messages tooit</span></span>

<span data-ttu-id="c76cd-138">Ebben a szakaszban hozzon létre egy Service Bus-üzenetsorba, csatlakoztassa tooyour IoT-központot, és konfigurálja az IoT hub toosend üzenetek toohello várólista hello jelenléte ügyfélkulcscsere-tulajdonságok alapján.</span><span class="sxs-lookup"><span data-stu-id="c76cd-138">In this section, you create a Service Bus queue, connect it tooyour IoT hub, and configure your IoT hub toosend messages toohello queue based on hello presence of a property on hello message.</span></span> <span data-ttu-id="c76cd-139">Hogyan tooprocess a Service Bus-üzenetsorok érkező üzenetek kapcsolatos további információkért lásd: [Ismerkedés a várólisták][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="c76cd-139">For more information about how tooprocess messages from Service Bus queues, see [Get started with queues][lnk-sb-queues-java].</span></span>

1. <span data-ttu-id="c76cd-140">Hozzon létre egy Service Bus-üzenetsorba, a [Ismerkedés a várólisták][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="c76cd-140">Create a Service Bus queue as described in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="c76cd-141">Jegyezze fel a hello névtér és a várólista nevét.</span><span class="sxs-lookup"><span data-stu-id="c76cd-141">Make a note of hello namespace and queue name.</span></span>

2. <span data-ttu-id="c76cd-142">A hello Azure-portálon, nyissa meg az IoT hub, és kattintson a **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="c76cd-142">In hello Azure portal, open your IoT hub and click **Endpoints**.</span></span>

    ![Az IoT-központ végpontok][30]

3. <span data-ttu-id="c76cd-144">A hello **végpontok** panelen kattintson a **hozzáadása** : hello felső tooadd a várólista tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="c76cd-144">In hello **Endpoints** blade, click **Add** at hello top tooadd your queue tooyour IoT hub.</span></span> <span data-ttu-id="c76cd-145">Név hello végpont **CriticalQueue** és hello legördülő listákat tooselect **Service Bus-üzenetsorba**, Service Bus-névtér, amelyben a várólista található hello és hello a várólista neve.</span><span class="sxs-lookup"><span data-stu-id="c76cd-145">Name hello endpoint **CriticalQueue** and use hello drop-downs tooselect **Service Bus queue**, hello Service Bus namespace in which your queue resides, and hello name of your queue.</span></span> <span data-ttu-id="c76cd-146">Amikor elkészült, kattintson a **mentése** hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c76cd-146">When you are done, click **Save** at hello bottom.</span></span>

    ![A végpont hozzáadása][31]

4. <span data-ttu-id="c76cd-148">Most kattintson **útvonalak** az IoT Hub a.</span><span class="sxs-lookup"><span data-stu-id="c76cd-148">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="c76cd-149">Kattintson a **Hozzáadás** hello panel toocreate toohello csak várólistára meg üzeneteit útválasztási szabályainak hozzáadott hello tetején.</span><span class="sxs-lookup"><span data-stu-id="c76cd-149">Click **Add** at hello top of hello blade toocreate a routing rule that routes messages toohello queue you just added.</span></span> <span data-ttu-id="c76cd-150">Válassza ki **DeviceTelemetry** hello adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="c76cd-150">Select **DeviceTelemetry** as hello source of data.</span></span> <span data-ttu-id="c76cd-151">Adja meg `level="critical"` hello feltételként, és válassza ki az előzőekben adott hozzá, útválasztási szabály végpont hello egyéni végpontként hello várólista.</span><span class="sxs-lookup"><span data-stu-id="c76cd-151">Enter `level="critical"` as hello condition, and choose hello queue you just added as a custom endpoint as hello routing rule endpoint.</span></span> <span data-ttu-id="c76cd-152">Amikor elkészült, kattintson a **mentése** hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c76cd-152">When you are done, click **Save** at hello bottom.</span></span>

    ![Egy útvonal hozzáadása][32]

    <span data-ttu-id="c76cd-154">Ellenőrizze, hogy hello tartalék útvonal túl van-e állítva**ON**.</span><span class="sxs-lookup"><span data-stu-id="c76cd-154">Make sure hello fallback route is set too**ON**.</span></span> <span data-ttu-id="c76cd-155">Ez a beállítás akkor hello alapértelmezett beállítása az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="c76cd-155">This setting is hello default configuration of an IoT hub.</span></span>

    ![Tartalék útvonal][33]

## <a name="optional-read-from-hello-queue-endpoint"></a><span data-ttu-id="c76cd-157">(Választható) Hello várólista végpont olvasásakor</span><span class="sxs-lookup"><span data-stu-id="c76cd-157">(Optional) Read from hello queue endpoint</span></span>

<span data-ttu-id="c76cd-158">Opcionálisan olvasható köszönőüzenetei hello várólista végpont a hello utasításait követve [Ismerkedés a várólisták][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="c76cd-158">You can optionally read hello messages from hello queue endpoint by following hello instructions in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="c76cd-159">Name hello app **olvasási kritikus várólista-**.</span><span class="sxs-lookup"><span data-stu-id="c76cd-159">Name hello app **read-critical-queue**.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="c76cd-160">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="c76cd-160">Run hello applications</span></span>

<span data-ttu-id="c76cd-161">Most már készen áll a toorun hello három alkalmazások áll.</span><span class="sxs-lookup"><span data-stu-id="c76cd-161">Now you are ready toorun hello three applications.</span></span>

1. <span data-ttu-id="c76cd-162">toorun hello **d2c-üzenetek olvasása** alkalmazás, a parancssor vagy a rendszerhéj nyissa meg a toohello olvasás-d2c mappa, és hajtsa végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c76cd-162">toorun hello **read-d2c-messages** application, in a command prompt or shell navigate toohello read-d2c folder and execute hello following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Futtassa a üzenetek d2c olvasása][readd2c]

2. <span data-ttu-id="c76cd-164">toorun hello **olvasási kritikus várólista-** alkalmazás, a parancssor vagy a rendszerhéj nyissa meg a toohello olvasási kritikus várólista-mappa, és hajtsa végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c76cd-164">toorun hello **read-critical-queue** application, in a command prompt or shell navigate toohello read-critical-queue folder and execute hello following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Olvasási kritikus üzeneteihez futtatása][readqueue]

3. <span data-ttu-id="c76cd-166">toorun hello **szimulált eszköz** alkalmazást, a parancssor vagy a rendszerhéj toohello szimulált eszköz mappában nyissa meg, és hajtsa végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c76cd-166">toorun hello **simulated-device** app, in a command prompt or shell navigate toohello simulated-device folder and execute hello following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Szimulált eszköz futtatása][simulateddevice]

## <a name="next-steps"></a><span data-ttu-id="c76cd-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c76cd-168">Next steps</span></span>

<span data-ttu-id="c76cd-169">Ebben az oktatóanyagban megtanulta, hogyan tooreliably mennyi funkcióval hello üzenet útválasztási IoT hub eszköz a felhőbe küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="c76cd-169">In this tutorial, you learned how tooreliably dispatch device-to-cloud messages by using hello message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="c76cd-170">Hello [hogyan toosend felhőből eszközre küldött üzenetek IoT-központ] [ lnk-c2d] bemutatja, hogyan toosend üzenetek tooyour eszközöket a megoldás háttérből.</span><span class="sxs-lookup"><span data-stu-id="c76cd-170">hello [How toosend cloud-to-device messages with IoT Hub][lnk-c2d] shows you how toosend messages tooyour devices from your solution back end.</span></span>

<span data-ttu-id="c76cd-171">teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="c76cd-171">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="c76cd-172">További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].</span><span class="sxs-lookup"><span data-stu-id="c76cd-172">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<span data-ttu-id="c76cd-173">toolearn üzenetet az IoT hubon útválasztási kapcsolatos további információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="c76cd-173">toolearn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Ismerkedés az IoT-központ]: iot-hub-java-java-getstarted.md
[Azure IoT fejlesztői központ]: https://azure.microsoft.com/develop/iot
[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
