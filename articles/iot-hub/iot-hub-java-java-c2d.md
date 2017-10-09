---
title: "az Azure IoT Hub (Java) aaaCloud eszközre üzenetek |} Microsoft Docs"
description: "Hogyan toosend felhő eszköz üzenetek tooa eszköz regisztrációját az Azure IoT-központ Java hello Azure IoT SDK-k használatával. A szimulált eszköz alkalmazás tooreceive felhő eszközre üzeneteket módosítja, és módosítsa a háttér-alkalmazás toosend hello felhő eszközre üzeneteket."
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
ms.openlocfilehash: 8721f18428c849ed9a04aa2e45c65605c3e38101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a><span data-ttu-id="d3992-104">Az IoT-központ (Java) felhő eszközre-üzenetek</span><span class="sxs-lookup"><span data-stu-id="d3992-104">Send cloud-to-device messages with IoT Hub (Java)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

<span data-ttu-id="d3992-105">Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere.</span><span class="sxs-lookup"><span data-stu-id="d3992-105">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="d3992-106">Hello [Ismerkedés az IoT-központ] az oktatóanyag bemutatja, hogyan toocreate az IoT-központ telepítéséhez azt egy eszközidentitás, és kód egy szimulált eszköz alkalmazást, amelyet az eszköz a felhőbe küldött üzeneteket küld.</span><span class="sxs-lookup"><span data-stu-id="d3992-106">hello [Get started with IoT Hub] tutorial shows how toocreate an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="d3992-107">Ez az oktatóanyag épül [Ismerkedés az IoT-központ].</span><span class="sxs-lookup"><span data-stu-id="d3992-107">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="d3992-108">Megmutatja, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="d3992-108">It shows you how to:</span></span>

* <span data-ttu-id="d3992-109">A megoldás háttérrendszeréhez küld felhő-eszközre küldött üzenetek tooa egyetlen eszközt az IoT-központ keresztül.</span><span class="sxs-lookup"><span data-stu-id="d3992-109">From your solution back end, send cloud-to-device messages tooa single device through IoT Hub.</span></span>
* <span data-ttu-id="d3992-110">Az eszközön a felhőből eszközre üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="d3992-110">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="d3992-111">A megoldás háttérrendszeréhez, a kérelem kézbesítési nyugtázási (*visszajelzés*) az üzenetek küldése tooa eszközt az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="d3992-111">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent tooa device from IoT Hub.</span></span>

<span data-ttu-id="d3992-112">További információ a felhő-eszközre küldött üzenetek található hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="d3992-112">You can find more information on cloud-to-device messages in hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="d3992-113">Ez az oktatóanyag végén hello két Java konzol alkalmazások futtatása:</span><span class="sxs-lookup"><span data-stu-id="d3992-113">At hello end of this tutorial, you run two Java console apps:</span></span>

* <span data-ttu-id="d3992-114">**Szimulált eszköz**, hello alkalmazás létrehozott egy módosított verziója [Ismerkedés az IoT-központ], amely tooyour IoT-központ kapcsolódik, és felhő eszközre üzeneteket fogad.</span><span class="sxs-lookup"><span data-stu-id="d3992-114">**simulated-device**, a modified version of hello app created in [Get started with IoT Hub], which connects tooyour IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="d3992-115">**küldési-c2d-üzenetek**, amely IoT-központot egy felhő-eszközre küldött üzenetek toohello szimulált eszköz alkalmazás küld, és majd megkapja a kézbesítési nyugtázási.</span><span class="sxs-lookup"><span data-stu-id="d3992-115">**send-c2d-messages**, which sends a cloud-to-device message toohello simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="d3992-116">Az IoT-központ rendelkezik sok eszköz platformok és nyelvek (például C, Java és Javascript) keresztül Azure IoT eszközoldali SDK-k SDK támogatása.</span><span class="sxs-lookup"><span data-stu-id="d3992-116">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="d3992-117">Hogyan tooconnect az eszköz toothis oktatóanyag kódot, és általában tooAzure IoT Hub: lépésenkénti hello [Azure IoT fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="d3992-117">For step-by-step instructions on how tooconnect your device toothis tutorial's code, and generally tooAzure IoT Hub, see hello [Azure IoT Developer Center].</span></span>

<span data-ttu-id="d3992-118">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d3992-118">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="d3992-119">A teljes működő verziójához hello [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) vagy [folyamat IoT Hub eszköz-a-felhőbe küldött üzeneteket](iot-hub-java-java-process-d2c.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d3992-119">A complete working version of hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) or [Process IoT Hub device-to-cloud messages](iot-hub-java-java-process-d2c.md) tutorial.</span></span>
* <span data-ttu-id="d3992-120">hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="d3992-120">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="d3992-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="d3992-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="d3992-122">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="d3992-122">An active Azure account.</span></span> <span data-ttu-id="d3992-123">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="d3992-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-hello-simulated-device-app"></a><span data-ttu-id="d3992-124">A szimulált eszköz app hello üzeneteket fogadni</span><span class="sxs-lookup"><span data-stu-id="d3992-124">Receive messages in hello simulated device app</span></span>

<span data-ttu-id="d3992-125">Ebben a szakaszban létrehozott hello szimulált eszköz alkalmazásának módosítása [Ismerkedés az IoT-központ] hello IoT-központ üzeneteit tooreceive felhő eszközre.</span><span class="sxs-lookup"><span data-stu-id="d3992-125">In this section, you modify hello simulated device app you created in [Get started with IoT Hub] tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="d3992-126">Egy szövegszerkesztőben nyissa meg hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="d3992-126">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

2. <span data-ttu-id="d3992-127">Adja hozzá a következő hello **MessageCallback** osztályhoz tartozik, mint egy beágyazott osztály belsejében hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="d3992-127">Add hello following **MessageCallback** class as a nested class inside hello **App** class.</span></span> <span data-ttu-id="d3992-128">Hello **hajtható végre** hello eszköz üzenetet kap, az IoT-központ metódus hív meg.</span><span class="sxs-lookup"><span data-stu-id="d3992-128">hello **execute** method is invoked when hello device receives a message from IoT Hub.</span></span> <span data-ttu-id="d3992-129">Ebben a példában hello eszköz mindig értesíti hello IoT-központot, hogy befejeződött-e üdvözlőüzenetére:</span><span class="sxs-lookup"><span data-stu-id="d3992-129">In this example, hello device always notifies hello IoT hub that it has completed hello message:</span></span>

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. <span data-ttu-id="d3992-130">Módosítsa a hello **fő** metódus toocreate egy **AppMessageCallback** példány és hívás hello **setMessageCallback** módszert az alábbiak szerint hello ügyfél megnyitása előtt:</span><span class="sxs-lookup"><span data-stu-id="d3992-130">Modify hello **main** method toocreate an **AppMessageCallback** instance and call hello **setMessageCallback** method before it opens hello client as follows:</span></span>

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > <span data-ttu-id="d3992-131">A HTTP használata helyett MQTT vagy AMQP hello átvitelhez, hello **DeviceClient** példány ellenőrzi az üzeneteket az IoT-központ ritkán (kevesebb mint 25 percenként).</span><span class="sxs-lookup"><span data-stu-id="d3992-131">If you use HTTP instead of MQTT or AMQP as hello transport, hello **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="d3992-132">MQTT, amqp-t és HTTP-támogatás, és az IoT-központ sávszélesség-szabályozás hello különbségei kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="d3992-132">For more information about hello differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

4. <span data-ttu-id="d3992-133">toobuild hello **szimulált eszköz** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello szimulált eszköz mappában hello:</span><span class="sxs-lookup"><span data-stu-id="d3992-133">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="d3992-134">Felhő eszközre üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="d3992-134">Send a cloud-to-device message</span></span>

<span data-ttu-id="d3992-135">Ebben a szakaszban egy Java-Konzolalkalmazás által a felhő-eszközre küldött üzenetek toohello szimulált eszköz alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d3992-135">In this section, you create a Java console app that sends cloud-to-device messages toohello simulated device app.</span></span> <span data-ttu-id="d3992-136">Eszközazonosító hello hozzáadott hello eszköz kell hello [Ismerkedés az IoT-központ] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d3992-136">You need hello device ID of hello device you added in hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="d3992-137">Akkor is kell hello IoT-központ kapcsolati karakterláncot a központ, amely az hello található [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="d3992-137">You also need hello IoT Hub connection string for your hub that you can find in hello [Azure portal].</span></span>

1. <span data-ttu-id="d3992-138">Nevű Maven-projekt létrehozása **c2d-üzenetküldés** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="d3992-138">Create a Maven project called **send-c2d-messages** using hello following command at your command prompt.</span></span> <span data-ttu-id="d3992-139">Megjegyzés: Ez a parancs egy egyetlen, hosszú parancsot:</span><span class="sxs-lookup"><span data-stu-id="d3992-139">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="d3992-140">A parancssorban lépjen a toohello új küldési-c2d-üzenetek mappa.</span><span class="sxs-lookup"><span data-stu-id="d3992-140">At your command prompt, navigate toohello new send-c2d-messages folder.</span></span>

3. <span data-ttu-id="d3992-141">Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello küldési-c2d-üzenetek mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="d3992-141">Using a text editor, open hello pom.xml file in hello send-c2d-messages folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="d3992-142">Hello-függőség felvétele lehetővé teszi toouse hello **IOT hubbal-java-szolgáltatásügyfél** az alkalmazás toocommunicate az IoT hub szolgáltatással-csomagot:</span><span class="sxs-lookup"><span data-stu-id="d3992-142">Adding hello dependency enables you toouse hello **iothub-java-service-client** package in your application toocommunicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="d3992-143">Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="d3992-143">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="d3992-144">Mentse és zárja be a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="d3992-144">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="d3992-145">Egy szövegszerkesztőben nyissa meg hello send-c2d-messages\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="d3992-145">Using a text editor, open hello send-c2d-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="d3992-146">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="d3992-146">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="d3992-147">Adja hozzá a következő osztály változók toohello hello **App** osztály cseréje **{yourhubconnectionstring}** és **{yourdeviceid}** hello értékeit a beállításértékeket korábban:</span><span class="sxs-lookup"><span data-stu-id="d3992-147">Add hello following class-level variables toohello **App** class, replacing **{yourhubconnectionstring}** and **{yourdeviceid}** with hello values your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. <span data-ttu-id="d3992-148">Cserélje le a hello **fő** hello kód a következő metódust.</span><span class="sxs-lookup"><span data-stu-id="d3992-148">Replace hello **main** method with hello following code.</span></span> <span data-ttu-id="d3992-149">Ez a kód tooyour IoT-központ csatlakozik, küld egy üzenet tooyour eszköz, valamint a majd megvárja-e a nyugtázás hello eszköz fogadott és a feldolgozott hello üzenet:</span><span class="sxs-lookup"><span data-stu-id="d3992-149">This code connects tooyour IoT hub, sends a message tooyour device, and then waits for an acknowledgment that hello device received and processed hello message:</span></span>
   
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
   
        Message messageToSend = new Message("Cloud toodevice message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
   
        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent toodevice");
   
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
    > <span data-ttu-id="d3992-150">Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="d3992-150">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="d3992-151">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="d3992-151">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>


9. <span data-ttu-id="d3992-152">toobuild hello **szimulált eszköz** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello szimulált eszköz mappában hello:</span><span class="sxs-lookup"><span data-stu-id="d3992-152">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="d3992-153">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="d3992-153">Run hello applications</span></span>

<span data-ttu-id="d3992-154">Most már áll készen toorun hello alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="d3992-154">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="d3992-155">Parancsot egy parancssorba hello szimulált eszköz mappában futtassa a következő parancs toobegin telemetriai tooyour IoT hub és toolisten a hubról felhő eszközre üzeneteket küld hello:</span><span class="sxs-lookup"><span data-stu-id="d3992-155">At a command prompt in hello simulated-device folder, run hello following command toobegin sending telemetry tooyour IoT hub and toolisten for cloud-to-device messages sent from your hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Hello szimulált eszköz alkalmazás futtatása][img-simulated-device]

2. <span data-ttu-id="d3992-157">Parancssorba hello küldési-c2d-üzenetek mappában futtassa a következő parancs toosend hello egy felhő-eszközre küldött üzenetek és a visszajelzés visszaigazolás Várakozás:</span><span class="sxs-lookup"><span data-stu-id="d3992-157">At a command prompt in hello send-c2d-messages folder, run hello following command toosend a cloud-to-device message and wait for a feedback acknowledgment:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Hello felhő eszközre parancs toosend üdvözlőüzenetére futtatása][img-send-command]

## <a name="next-steps"></a><span data-ttu-id="d3992-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3992-159">Next steps</span></span>

<span data-ttu-id="d3992-160">Ebben az oktatóanyagban, megtudta, hogyan toosend és felhő-eszközre küldött üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="d3992-160">In this tutorial, you learned how toosend and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="d3992-161">teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="d3992-161">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="d3992-162">További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].</span><span class="sxs-lookup"><span data-stu-id="d3992-162">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Ismerkedés az IoT-központ]: iot-hub-java-java-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md
[Azure IoT fejlesztői központ]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portálon]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22