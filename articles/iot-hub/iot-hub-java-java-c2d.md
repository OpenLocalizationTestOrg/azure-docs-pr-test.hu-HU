---
title: "Az Azure IoT Hub (Java) felhő eszközre üzenetek |} Microsoft Docs"
description: "Hogyan küldhetők felhő-eszközre küldött üzenetek egy eszközre egy Azure IoT hub Java SDK az Azure IoT-k használatával. A szimulált eszköz alkalmazásnak, hogy a felhő-eszközre küldött üzenetek fogadására és módosíthat egy háttér-alkalmazást a felhőből eszközre küldéséhez módosítása."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7f785ea8-e7c2-40c5-87ef-96525e9b9e1e
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: f5e3ac46f4d144b12e2ab7fcfb456665ff6cc68f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a><span data-ttu-id="a772e-104">Az IoT-központ (Java) felhő eszközre-üzenetek</span><span class="sxs-lookup"><span data-stu-id="a772e-104">Send cloud-to-device messages with IoT Hub (Java)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

<span data-ttu-id="a772e-105">Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere.</span><span class="sxs-lookup"><span data-stu-id="a772e-105">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="a772e-106">A [Ismerkedés az IoT-központ] oktatóanyag bemutatja, hogyan létrehoz egy IoT-központot, azt egy eszközidentitás kiépítéséhez és kód egy szimulált eszköz alkalmazást, amelyet az eszköz a felhőbe küldött üzeneteket küld.</span><span class="sxs-lookup"><span data-stu-id="a772e-106">The [Get started with IoT Hub] tutorial shows how to create an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="a772e-107">Ez az oktatóanyag épül [Ismerkedés az IoT-központ].</span><span class="sxs-lookup"><span data-stu-id="a772e-107">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="a772e-108">Megmutatja, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="a772e-108">It shows you how to:</span></span>

* <span data-ttu-id="a772e-109">A megoldás háttérrendszeréhez, az IoT-központ keresztül egyetlen eszközt felhő eszközre üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="a772e-109">From your solution back end, send cloud-to-device messages to a single device through IoT Hub.</span></span>
* <span data-ttu-id="a772e-110">Az eszközön a felhőből eszközre üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="a772e-110">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="a772e-111">A megoldás háttérrendszeréhez, a kérelem kézbesítési nyugtázási (*visszajelzés*) IoT-központot egy eszközre küldött üzenetek esetében.</span><span class="sxs-lookup"><span data-stu-id="a772e-111">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent to a device from IoT Hub.</span></span>

<span data-ttu-id="a772e-112">Felhő eszközre üzenetek további információt a [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="a772e-112">You can find more information on cloud-to-device messages in the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="a772e-113">Ez az oktatóanyag végén két Java konzol alkalmazások futtatása:</span><span class="sxs-lookup"><span data-stu-id="a772e-113">At the end of this tutorial, you run two Java console apps:</span></span>

* <span data-ttu-id="a772e-114">**Szimulált eszköz**, az alkalmazás létrehozása a módosított változatát [Ismerkedés az IoT-központ], amely csatlakozik az IoT hub és felhő eszközre üzeneteket fogad.</span><span class="sxs-lookup"><span data-stu-id="a772e-114">**simulated-device**, a modified version of the app created in [Get started with IoT Hub], which connects to your IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="a772e-115">**küldési-c2d-üzenetek**, melyik felhőalapú eszközre üzenetet küld az IoT-központ szimulált eszköz alkalmazás, és a szállítási nyugtázási majd megkapja.</span><span class="sxs-lookup"><span data-stu-id="a772e-115">**send-c2d-messages**, which sends a cloud-to-device message to the simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="a772e-116">Az IoT-központ rendelkezik sok eszköz platformok és nyelvek (például C, Java és Javascript) keresztül Azure IoT eszközoldali SDK-k SDK támogatása.</span><span class="sxs-lookup"><span data-stu-id="a772e-116">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="a772e-117">Csatlakoztassa az eszközt, az oktatóanyag kódot, és általában Azure IoT Hub kapcsolatos lépésenkénti útmutatót lásd: a [Azure IoT fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="a772e-117">For step-by-step instructions on how to connect your device to this tutorial's code, and generally to Azure IoT Hub, see the [Azure IoT Developer Center].</span></span>

<span data-ttu-id="a772e-118">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="a772e-118">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="a772e-119">A teljes működő verziójához a [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) vagy [folyamat IoT Hub eszköz-a-felhőbe küldött üzeneteket](iot-hub-java-java-process-d2c.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a772e-119">A complete working version of the [Get started with IoT Hub](iot-hub-java-java-getstarted.md) or [Process IoT Hub device-to-cloud messages](iot-hub-java-java-process-d2c.md) tutorial.</span></span>
* <span data-ttu-id="a772e-120">A legújabb [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="a772e-120">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="a772e-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="a772e-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="a772e-122">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a772e-122">An active Azure account.</span></span> <span data-ttu-id="a772e-123">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="a772e-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-the-simulated-device-app"></a><span data-ttu-id="a772e-124">A szimulált eszköz alkalmazás üzeneteket fogadni</span><span class="sxs-lookup"><span data-stu-id="a772e-124">Receive messages in the simulated device app</span></span>

<span data-ttu-id="a772e-125">Ebben a szakaszban módosítsa a szimulált eszköz alkalmazás létrehozott [Ismerkedés az IoT-központ] felhő eszközre üzenetek fogadása az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a772e-125">In this section, you modify the simulated device app you created in [Get started with IoT Hub] to receive cloud-to-device messages from the IoT hub.</span></span>

1. <span data-ttu-id="a772e-126">Egy szövegszerkesztővel nyissa meg a simulated-device\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="a772e-126">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

2. <span data-ttu-id="a772e-127">Adja hozzá a következő **MessageCallback** osztályhoz tartozik, mint egy beágyazott osztály belsejében a **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="a772e-127">Add the following **MessageCallback** class as a nested class inside the **App** class.</span></span> <span data-ttu-id="a772e-128">A **hajtható végre** metódus van meghívva, amikor az eszköz megkapja egy üzenetet az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="a772e-128">The **execute** method is invoked when the device receives a message from IoT Hub.</span></span> <span data-ttu-id="a772e-129">Ebben a példában az eszköz mindig értesíti az IoT-központot, hogy befejeződött-e az üzenetet:</span><span class="sxs-lookup"><span data-stu-id="a772e-129">In this example, the device always notifies the IoT hub that it has completed the message:</span></span>

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. <span data-ttu-id="a772e-130">Módosítsa a **fő** metódus létrehozása egy **AppMessageCallback** példány és a hívás a **setMessageCallback** módszert az alábbiak szerint az ügyfél megnyitása előtt:</span><span class="sxs-lookup"><span data-stu-id="a772e-130">Modify the **main** method to create an **AppMessageCallback** instance and call the **setMessageCallback** method before it opens the client as follows:</span></span>

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > <span data-ttu-id="a772e-131">Ha a átvitelhez MQTT vagy AMQP helyett a HTTP Protokollt használja. a **DeviceClient** példány ellenőrzi az üzeneteket az IoT-központ ritkán (kevesebb mint 25 percenként).</span><span class="sxs-lookup"><span data-stu-id="a772e-131">If you use HTTP instead of MQTT or AMQP as the transport, the **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="a772e-132">MQTT, amqp-t és HTTP-támogatás, és az IoT-központ sávszélesség-szabályozás közötti különbségekről további információkért lásd: a [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="a772e-132">For more information about the differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

4. <span data-ttu-id="a772e-133">Ha a **simulated-device** alkalmazást a Maven használatával szeretné felépíteni, futtassa a következő parancsot a parancssorban a simulated-device mappában:</span><span class="sxs-lookup"><span data-stu-id="a772e-133">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="a772e-134">Felhő eszközre üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="a772e-134">Send a cloud-to-device message</span></span>

<span data-ttu-id="a772e-135">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amelyek a felhőből eszközre üzeneteket küld a szimulált eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a772e-135">In this section, you create a Java console app that sends cloud-to-device messages to the simulated device app.</span></span> <span data-ttu-id="a772e-136">A hozzáadott eszköz az Eszközazonosítót van szüksége a [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a772e-136">You need the device ID of the device you added in the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="a772e-137">Az IoT-központ kapcsolati karakterlánc, amely megtalálható a központ is kell a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="a772e-137">You also need the IoT Hub connection string for your hub that you can find in the [Azure portal].</span></span>

1. <span data-ttu-id="a772e-138">Nevű Maven-projekt létrehozása **c2d-üzenetküldés** parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="a772e-138">Create a Maven project called **send-c2d-messages** using the following command at your command prompt.</span></span> <span data-ttu-id="a772e-139">Megjegyzés: Ez a parancs egy egyetlen, hosszú parancsot:</span><span class="sxs-lookup"><span data-stu-id="a772e-139">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="a772e-140">A parancssorban keresse meg az új c2d-üzenetküldés mappát.</span><span class="sxs-lookup"><span data-stu-id="a772e-140">At your command prompt, navigate to the new send-c2d-messages folder.</span></span>

3. <span data-ttu-id="a772e-141">Egy szövegszerkesztővel nyissa meg a pom.xml fájlt a küldési-c2d-üzenetek mappában, és adja hozzá a következő függőség a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="a772e-141">Using a text editor, open the pom.xml file in the send-c2d-messages folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="a772e-142">A függőség hozzáadása lehetővé teszi, hogy a **IOT hubbal-java-szolgáltatásügyfél** csomag kommunikáljon az IoT-központ szolgáltatás az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="a772e-142">Adding the dependency enables you to use the **iothub-java-service-client** package in your application to communicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="a772e-143">Az **iot-service-client** legújabb verzióját a [Maven keresési funkciójával][lnk-maven-service-search] tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="a772e-143">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="a772e-144">Mentse és zárja be a pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="a772e-144">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="a772e-145">Egy szövegszerkesztőben nyissa meg a send-c2d-messages\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="a772e-145">Using a text editor, open the send-c2d-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="a772e-146">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="a772e-146">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="a772e-147">A következő osztály szintű változók hozzáadásával a **App** osztály cseréje **{yourhubconnectionstring}** és **{yourdeviceid}** értékekkel a beállításértékeket korábban:</span><span class="sxs-lookup"><span data-stu-id="a772e-147">Add the following class-level variables to the **App** class, replacing **{yourhubconnectionstring}** and **{yourdeviceid}** with the values your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. <span data-ttu-id="a772e-148">Cserélje le a **fő** metódus a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="a772e-148">Replace the **main** method with the following code.</span></span> <span data-ttu-id="a772e-149">Ez a kód csatlakozik az IoT hub, egy üzenetet küld az eszköz és majd megvárja, hogy az eszköz kapott, és az üzenet feldolgozása nyugtázás:</span><span class="sxs-lookup"><span data-stu-id="a772e-149">This code connects to your IoT hub, sends a message to your device, and then waits for an acknowledgment that the device received and processed the message:</span></span>
   
    ```java
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
   
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver();
        if (feedbackReceiver != null) feedbackReceiver.open();
   
        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
   
        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");
   
        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }
   
        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="a772e-150">Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="a772e-150">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="a772e-151">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), az MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="a772e-151">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>


9. <span data-ttu-id="a772e-152">Ha a **simulated-device** alkalmazást a Maven használatával szeretné felépíteni, futtassa a következő parancsot a parancssorban a simulated-device mappában:</span><span class="sxs-lookup"><span data-stu-id="a772e-152">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a><span data-ttu-id="a772e-153">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="a772e-153">Run the applications</span></span>

<span data-ttu-id="a772e-154">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="a772e-154">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="a772e-155">A parancssorba a szimulált eszköz mappában futtassa a következő parancsot, telemetriai adatok küldése az IoT hub megkezdéséhez és a felhő eszközre üzenetek a központ által küldött figyelésére:</span><span class="sxs-lookup"><span data-stu-id="a772e-155">At a command prompt in the simulated-device folder, run the following command to begin sending telemetry to your IoT hub and to listen for cloud-to-device messages sent from your hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![A szimulált eszköz alkalmazás futtatása][img-simulated-device]

2. <span data-ttu-id="a772e-157">A parancssorba a küldési-c2d-üzenetek mappában futtassa a felhőből eszközre küldött és a visszajelzés visszaigazolás várja meg a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a772e-157">At a command prompt in the send-c2d-messages folder, run the following command to send a cloud-to-device message and wait for a feedback acknowledgment:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Futtassa a parancsot a felhőből eszközre üzenet küldése][img-send-command]

## <a name="next-steps"></a><span data-ttu-id="a772e-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a772e-159">Next steps</span></span>

<span data-ttu-id="a772e-160">Ebben az oktatóprogramban megismerte felhő eszközre üzeneteket küldjön és fogadjon.</span><span class="sxs-lookup"><span data-stu-id="a772e-160">In this tutorial, you learned how to send and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="a772e-161">Példák teljes végpontok közötti megoldások, amelyek használják az IoT-központot, lásd: [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="a772e-161">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="a772e-162">Az IoT hubbal megoldások fejlesztésével kapcsolatos további tudnivalókért tekintse meg a [IoT Hub fejlesztői útmutató].</span><span class="sxs-lookup"><span data-stu-id="a772e-162">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

<span data-ttu-id="a772e-163">[Ismerkedés az IoT-központ]: iot-hub-java-java-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="a772e-163">[Get started with IoT Hub]: iot-hub-java-java-getstarted.md</span></span>
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
<span data-ttu-id="a772e-164">[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="a772e-164">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
<span data-ttu-id="a772e-165">[Azure IoT fejlesztői központ]: http://www.azure.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="a772e-165">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span></span>
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
<span data-ttu-id="a772e-166">[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="a772e-166">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
<span data-ttu-id="a772e-167">[Azure-portálon]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="a772e-167">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="a772e-168">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span><span class="sxs-lookup"><span data-stu-id="a772e-168">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span></span>
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22