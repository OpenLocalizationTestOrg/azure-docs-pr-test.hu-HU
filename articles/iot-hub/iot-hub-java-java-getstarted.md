---
title: "Ismerkedés az Azure IoT Hub (Java) szolgáltatással | Microsoft Docs"
description: "Megtudhatja, hogyan küldhet eszközről a felhőbe irányuló üzeneteket az Azure IoT Hubba a Javához készült IoT SDK-k használatával. Szimulált eszközt és szolgáltatásalkalmazásokat hozhat létre az eszköz regisztrálásához, üzenetek küldéséhez és üzenetek olvasásához az IoT Hubról."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 70dae4a8-0e98-4c53-b5a5-9d6963abb245
ms.service: iot-hub
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 707356a49970bcd76a55ee1b8a6fbddf6a6ba390
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-java"></a><span data-ttu-id="7ce14-104">Az eszköz csatlakoztatása az IoT Hubhoz Javával</span><span class="sxs-lookup"><span data-stu-id="7ce14-104">Connect your device to your IoT hub using Java</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="7ce14-105">Az oktatóanyag elvégzése után három Java-konzolalkalmazással fog rendelkezni:</span><span class="sxs-lookup"><span data-stu-id="7ce14-105">At the end of this tutorial, you have three Java console apps:</span></span>

* <span data-ttu-id="7ce14-106">A **create-device-identity** egy eszközidentitást, valamint egy társított biztonsági kulcsot hoz létre, amellyel csatlakozhat az eszközalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="7ce14-106">**create-device-identity**, which creates a device identity and associated security key to connect your device app.</span></span>
* <span data-ttu-id="7ce14-107">A **read-d2c-messages** megjeleníti az eszközalkalmazások által küldött telemetriát.</span><span class="sxs-lookup"><span data-stu-id="7ce14-107">**read-d2c-messages**, which displays the telemetry sent by your device app.</span></span>
* <span data-ttu-id="7ce14-108">A **simulated-device** csatlakozik az IoT Hubhoz a korábban létrehozott eszközidentitással, és az MQTT protokoll használatával másodpercenként telemetriai üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="7ce14-108">**simulated-device**, which connects to your IoT hub with the device identity created earlier, and sends a telemetry message every second using the MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="7ce14-109">Az Azure IoT SDK-kat használhatja az eszközökön és a megoldás háttérrendszerén futó alkalmazások összeállításához egyaránt. Ezekről az [Azure IoT SDK-k][lnk-hub-sdks] című témakörben talál további információt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-109">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both apps to run on devices and your solution back end.</span></span>

<span data-ttu-id="7ce14-110">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="7ce14-110">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="7ce14-111">A legújabb [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="7ce14-111">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span> 
* [<span data-ttu-id="7ce14-112">Maven 3</span><span class="sxs-lookup"><span data-stu-id="7ce14-112">Maven 3</span></span>](https://maven.apache.org/install.html) 
* <span data-ttu-id="7ce14-113">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="7ce14-113">An active Azure account.</span></span> <span data-ttu-id="7ce14-114">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="7ce14-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="7ce14-115">Az utolsó lépésben jegyezze fel az **Elsődleges kulcs** értékét.</span><span class="sxs-lookup"><span data-stu-id="7ce14-115">As a final step, make a note of the **Primary key** value.</span></span> <span data-ttu-id="7ce14-116">Ezután kattintson a **Végpontok**, majd az **Események** beépített végpontra.</span><span class="sxs-lookup"><span data-stu-id="7ce14-116">Then click **Endpoints** and the **Events** built-in endpoint.</span></span> <span data-ttu-id="7ce14-117">A **Tulajdonságok** panelen jegyezze fel az **Event Hub-kompatibilis nevet** és az **Event Hub-kompatibilis végpont** címét.</span><span class="sxs-lookup"><span data-stu-id="7ce14-117">On the **Properties** blade, make a note of the **Event Hub-compatible name** and the **Event Hub-compatible endpoint** address.</span></span> <span data-ttu-id="7ce14-118">Erre a három értékre szüksége lesz a **read-d2c-messages** alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7ce14-118">You need these three values when you create your **read-d2c-messages** app.</span></span>

![Az Azure Portal IoT Hub Üzenetküldés panelje][6]

<span data-ttu-id="7ce14-120">Ezzel létrehozta az IoT Hubot.</span><span class="sxs-lookup"><span data-stu-id="7ce14-120">You have now created your IoT hub.</span></span> <span data-ttu-id="7ce14-121">Rendelkezik az oktatóanyag teljesítéséhez szükséges IoT Hub-állomásnévvel és kapcsolati karakterlánccal, IoT Hub elsődleges kulccsal, valamint Event Hubs-kompatibilis névvel és végponttal.</span><span class="sxs-lookup"><span data-stu-id="7ce14-121">You have the IoT Hub host name, IoT Hub connection string, IoT Hub Primary Key, Event Hub-compatible name, and Event Hub-compatible endpoint you need to complete this tutorial.</span></span>

## <a name="create-a-device-identity"></a><span data-ttu-id="7ce14-122">Eszközidentitás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ce14-122">Create a device identity</span></span>
<span data-ttu-id="7ce14-123">Ebben a szakaszban egy Java-konzolalkalmazást fog létrehozni, amely egy új eszközidentitást hoz létre az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="7ce14-123">In this section, you create a Java console app that creates a device identity in the identity registry in your IoT hub.</span></span> <span data-ttu-id="7ce14-124">Egy eszköz csak akkor tud csatlakozni az IoT Hubhoz, ha be van jegyezve az identitásjegyzékbe.</span><span class="sxs-lookup"><span data-stu-id="7ce14-124">A device cannot connect to IoT hub unless it has an entry in the identity registry.</span></span> <span data-ttu-id="7ce14-125">További információkért lásd az [IoT Hub fejlesztői útmutatójának][lnk-devguide-identity] **Identitásjegyzék** című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="7ce14-125">For more information, see the **Identity Registry** section of the [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="7ce14-126">A konzolalkalmazás egy egyedi eszközazonosítót állít elő a futtatásakor, valamint egy kulcsot, amellyel az eszköz azonosítani tudja magát, amikor az eszközről a felhőbe irányuló üzeneteket küld az IoT Hubnak.</span><span class="sxs-lookup"><span data-stu-id="7ce14-126">When you run this console app, it generates a unique device ID and key that your device can use to identify itself when it sends device-to-cloud messages to IoT Hub.</span></span>

1. <span data-ttu-id="7ce14-127">Hozzon létre egy iot-java-get-started nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="7ce14-127">Create an empty folder called iot-java-get-started.</span></span> <span data-ttu-id="7ce14-128">Az iot-java-get-started mappában hozzon létre egy **create-device-identity** nevű Maven-projektet a következő parancs beírásával a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="7ce14-128">In the iot-java-get-started folder, create a Maven project called **create-device-identity** using the following command at your command prompt.</span></span> <span data-ttu-id="7ce14-129">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="7ce14-129">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="7ce14-130">A parancssorban lépjen a create-device-identity mappára.</span><span class="sxs-lookup"><span data-stu-id="7ce14-130">At your command prompt, navigate to the create-device-identity folder.</span></span>

3. <span data-ttu-id="7ce14-131">Egy szövegszerkesztővel nyissa meg a pom.xml fájlt a create-device-identity mappában, és adja hozzá a következő függőséget a **függőségek** csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="7ce14-131">Using a text editor, open the pom.xml file in the create-device-identity folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="7ce14-132">Ezzel a függőséggel használhatja az iot-service-client csomagot az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="7ce14-132">This dependency enables you to use the iot-service-client package in your app:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="7ce14-133">Az **iot-service-client** legújabb verzióját a [Maven keresési funkciójával][lnk-maven-service-search] tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="7ce14-133">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="7ce14-134">Mentse és zárja be a pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-134">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="7ce14-135">Egy szövegszerkesztővel nyissa meg a create-device-identity\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-135">Using a text editor, open the create-device-identity\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="7ce14-136">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="7ce14-136">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="7ce14-137">Adja a következő osztályszintű változókat az **App** osztályhoz, lecserélve a **{yourhubconnectionstring}** helyőrzőt a korábban feljegyzett értékkel:</span><span class="sxs-lookup"><span data-stu-id="7ce14-137">Add the following class-level variables to the **App** class, replacing **{yourhubconnectionstring}** with the value your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. <span data-ttu-id="7ce14-138">Módosítsa úgy a **main** metódus aláírását, hogy tartalmazza az alábbi kivételeket:</span><span class="sxs-lookup"><span data-stu-id="7ce14-138">Modify the signature of the **main** method to include the exceptions as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. <span data-ttu-id="7ce14-139">Adja hozzá a következő kódot a **main** metódus törzseként.</span><span class="sxs-lookup"><span data-stu-id="7ce14-139">Add the following code as the body of the **main** method.</span></span> <span data-ttu-id="7ce14-140">Ez a kód egy *javadevice* nevű eszközt hoz létre az IoT Hub identitásjegyzékében, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="7ce14-140">This code creates a device called *javadevice* in your IoT Hub identity registry if doesn't already exist.</span></span> <span data-ttu-id="7ce14-141">Ezután megjeleníti az eszközazonosítót és a kulcsot, amelyekre később szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="7ce14-141">It then displays the device ID and key that you need later:</span></span>

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. <span data-ttu-id="7ce14-142">Mentse és zárja be az App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-142">Save and close the App.java file.</span></span>

11. <span data-ttu-id="7ce14-143">Ha a **create-device-identity** alkalmazást a Maven használatával szeretné felépíteni, futtassa a következő parancsot a parancssorban a create-device-identity mappában:</span><span class="sxs-lookup"><span data-stu-id="7ce14-143">To build the **create-device-identity** app using Maven, execute the following command at the command prompt in the create-device-identity folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. <span data-ttu-id="7ce14-144">Ha a **create-device-identity** alkalmazást a Maven használatával szeretné futtatni, futtassa a következő parancsot a parancssorban a create-device-identity mappában:</span><span class="sxs-lookup"><span data-stu-id="7ce14-144">To run the **create-device-identity** app using Maven, execute the following command at the command prompt in the create-device-identity folder:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. <span data-ttu-id="7ce14-145">Jegyezze fel az **eszköz azonosítóját** és az **eszköz kulcsát**.</span><span class="sxs-lookup"><span data-stu-id="7ce14-145">Make a note of the **Device ID** and **Device key**.</span></span> <span data-ttu-id="7ce14-146">Ezekre az értékekre később szüksége lesz, amikor az IoT Hubhoz eszközként csatlakozó alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7ce14-146">You need these values later when you create an app that connects to IoT Hub as a device.</span></span>

> [!NOTE]
> <span data-ttu-id="7ce14-147">Az IoT Hub-identitásjegyzék csak az IoT Hub biztonságos elérésének biztosításához tárolja az eszközidentitásokat.</span><span class="sxs-lookup"><span data-stu-id="7ce14-147">The IoT Hub identity registry only stores device identities to enable secure access to the IoT hub.</span></span> <span data-ttu-id="7ce14-148">Az eszközazonosítókat és kulcsokat biztonsági hitelesítő adatokként tárolja, valamint tartalmaz egy engedélyezve/letiltva jelzőt, amellyel letilthatja egy adott eszköz hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="7ce14-148">It stores device IDs and keys to use as security credentials and an enabled/disabled flag that you can use to disable access for an individual device.</span></span> <span data-ttu-id="7ce14-149">Ha az alkalmazásnak más eszközspecifikus metaadatokat kell tárolnia, egy alkalmazásspecifikus tárolót kell használnia.</span><span class="sxs-lookup"><span data-stu-id="7ce14-149">If your app needs to store other device-specific metadata, it should use an app-specific store.</span></span> <span data-ttu-id="7ce14-150">További információ: [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="7ce14-150">For more information, see the [IoT Hub developer guide][lnk-devguide-identity].</span></span>

## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="7ce14-151">Az eszközről a felhőbe irányuló üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="7ce14-151">Receive device-to-cloud messages</span></span>

<span data-ttu-id="7ce14-152">Ebben a szakaszban egy Java-konzolalkalmazást hoz létre, amely az eszközről a felhőbe irányuló üzeneteket olvas az IoT Hubról.</span><span class="sxs-lookup"><span data-stu-id="7ce14-152">In this section, you create a Java console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="7ce14-153">Az IoT Hub egy [Event Hubs][lnk-event-hubs-overview]-kompatibilis végpontot tesz közzé, hogy lehetővé tegye az eszközről a felhőbe irányuló üzenetek olvasását.</span><span class="sxs-lookup"><span data-stu-id="7ce14-153">An IoT hub exposes an [Event Hub][lnk-event-hubs-overview]-compatible endpoint to enable you to read device-to-cloud messages.</span></span> <span data-ttu-id="7ce14-154">Az egyszerűség érdekében ez az oktatóanyag egy alapszintű olvasót hoz létre, amely nem alkalmas nagy átviteli sebességű üzemelő példányokhoz.</span><span class="sxs-lookup"><span data-stu-id="7ce14-154">To keep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="7ce14-155">Az eszközről a felhőbe irányuló üzenetek nagy léptékű feldolgozásával kapcsolatban lásd [az eszközről a felhőbe irányuló üzenetek feldolgozását][lnk-process-d2c-tutorial] ismertető oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="7ce14-155">The [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial shows you how to process device-to-cloud messages at scale.</span></span> <span data-ttu-id="7ce14-156">Az Event Hubs szolgáltatástól érkező üzenetek feldolgozásával kapcsolatos további információkért lásd [az Event Hubs használatának első lépéseit][lnk-eventhubs-tutorial] ismertető oktatóanyagot. Ez az IoT Hub Event Hub-kompatibilis végpontjaira érvényes.</span><span class="sxs-lookup"><span data-stu-id="7ce14-156">The [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial provides further information on how to process messages from Event Hubs and is applicable to the IoT Hub Event Hub-compatible endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="7ce14-157">Az eszközről a felhőbe irányuló üzenetek olvasásához használt Event Hub-kompatibilis végpontok mindig az AMQP protokollt használják.</span><span class="sxs-lookup"><span data-stu-id="7ce14-157">The Event Hub-compatible endpoint for reading device-to-cloud messages always uses the AMQP protocol.</span></span>

1. <span data-ttu-id="7ce14-158">Az *Eszközidentitás létrehozása* szakaszban létrehozott iot-java-get-started mappában hozzon létre egy **read-d2c-messages** nevű Maven-projektet a következő parancs beírásával a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="7ce14-158">In the iot-java-get-started folder you created in the *Create a device identity* section, create a Maven project called **read-d2c-messages** using the following command at your command prompt.</span></span> <span data-ttu-id="7ce14-159">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="7ce14-159">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="7ce14-160">A parancssorban lépjen a read-d2c-messages mappára.</span><span class="sxs-lookup"><span data-stu-id="7ce14-160">At your command prompt, navigate to the read-d2c-messages folder.</span></span>

3. <span data-ttu-id="7ce14-161">Egy szövegszerkesztővel nyissa meg a pom.xml fájlt a read-d2c-messages mappában, és adja hozzá a következő függőséget a **függőségek** csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="7ce14-161">Using a text editor, open the pom.xml file in the read-d2c-messages folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="7ce14-162">Ezzel a függőséggel használhatja az eventhubs-client csomagot az alkalmazásban az Event Hubs-kompatibilis végpontról való olvasáshoz:</span><span class="sxs-lookup"><span data-stu-id="7ce14-162">This dependency enables you to use the eventhubs-client package in your app to read from the Event Hub-compatible endpoint:</span></span>

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. <span data-ttu-id="7ce14-163">Mentse és zárja be a pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-163">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="7ce14-164">Egy szövegszerkesztővel nyissa meg a read-d2c-messages\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-164">Using a text editor, open the read-d2c-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="7ce14-165">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="7ce14-165">Add the following **import** statements to the file:</span></span>

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. <span data-ttu-id="7ce14-166">Adja hozzá a következő osztályszintű változót az **App** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="7ce14-166">Add the following class-level variable to the **App** class.</span></span> <span data-ttu-id="7ce14-167">Cserélje le a **{youriothubkey}**, **{youreventhubcompatibleendpoint}** és **{youreventhubcompatiblename}** helyőrzőket a korábban feljegyzett értékekre:</span><span class="sxs-lookup"><span data-stu-id="7ce14-167">Replace **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, and **{youreventhubcompatiblename}** with the values you noted previously:</span></span>

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. <span data-ttu-id="7ce14-168">Adja hozzá a következő **receiveMessages** metódust az **App** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="7ce14-168">Add the following **receiveMessages** method to the **App** class.</span></span> <span data-ttu-id="7ce14-169">Ez a metódus **EventHubClient** példányt hoz létre, amely az Event Hub-kompatibilis végponthoz csatlakozik, majd aszinkron módon létrehoz egy **PartitionReceiver** példányt, amely egy Event Hub-partícióról olvas.</span><span class="sxs-lookup"><span data-stu-id="7ce14-169">This method creates an **EventHubClient** instance to connect to the Event Hub-compatible endpoint and then asynchronously creates a **PartitionReceiver** instance to read from an Event Hub partition.</span></span> <span data-ttu-id="7ce14-170">Folyamatosan hurkokat alkot, és kinyomtatja az üzenet részleteit az alkalmazás leállásáig.</span><span class="sxs-lookup"><span data-stu-id="7ce14-170">It loops continuously and prints the message details until the app terminates.</span></span>

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        // Create a receiver using the
        // default Event Hubs consumer group
        // that listens for messages from now on.
        client.createReceiver(EventHubClient.DEFAULT_CONSUMER_GROUP_NAME, partitionId, Instant.now())
          .thenAccept(new Consumer<PartitionReceiver>() {
            public void accept(PartitionReceiver receiver) {
              System.out.println("** Created receiver on partition " + partitionId);
              try {
                while (true) {
                  Iterable<EventData> receivedEvents = receiver.receive(100).get();
                  int batchSize = 0;
                  if (receivedEvents != null) {
                    System.out.println("Got some evenst");
                    for (EventData receivedEvent : receivedEvents) {
                      System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s",
                        receivedEvent.getSystemProperties().getOffset(),
                        receivedEvent.getSystemProperties().getSequenceNumber(),
                        receivedEvent.getSystemProperties().getEnqueuedTime()));
                      System.out.println(String.format("| Device ID: %s",
                        receivedEvent.getSystemProperties().get("iothub-connection-device-id")));
                      System.out.println(String.format("| Message Payload: %s",
                        new String(receivedEvent.getBytes(), Charset.defaultCharset())));
                      batchSize++;
                    }
                  }
                  System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId, batchSize));
                }
              } catch (Exception e) {
                System.out.println("Failed to receive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="7ce14-171">Ez a metódus szűrőt használ a fogadó létrehozásakor, hogy a fogadó csak a fogadó futtatásának megkezdése után az IoT Hubra küldött üzeneteket olvassa.</span><span class="sxs-lookup"><span data-stu-id="7ce14-171">This method uses a filter when it creates the receiver so that the receiver only reads messages sent to IoT Hub after the receiver starts running.</span></span> <span data-ttu-id="7ce14-172">Ez a technika tesztkörnyezetben hasznos, mivel láthatja az aktuális üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="7ce14-172">This technique is useful in a test environment so you can see the current set of messages.</span></span> <span data-ttu-id="7ce14-173">Éles környezetben azonban a kódnak biztosítania kell, hogy az összes üzenetet feldolgozza. További információért lásd a következő oktatóanyagot: [Eszközről a felhőbe irányuló IoT Hub-üzenetek feldolgozása][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="7ce14-173">In a production environment, your code should make sure that it processes all the messages - for more information, see the [How to process IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

9. <span data-ttu-id="7ce14-174">Módosítsa úgy a **main** metódus aláírását, hogy tartalmazza az alábbi kivételt:</span><span class="sxs-lookup"><span data-stu-id="7ce14-174">Modify the signature of the **main** method to include the exception as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. <span data-ttu-id="7ce14-175">Adja hozzá a következő kódot a **main** metódushoz az **App** osztályban.</span><span class="sxs-lookup"><span data-stu-id="7ce14-175">Add the following code to the **main** method in the **App** class.</span></span> <span data-ttu-id="7ce14-176">Ez a kód létrehozza a két **EventHubClient** és **PartitionReceiver** példányt, és lehetővé teszi, hogy bezárja az alkalmazást az üzenetek feldolgozásának befejezése után:</span><span class="sxs-lookup"><span data-stu-id="7ce14-176">This code creates the two **EventHubClient** and **PartitionReceiver** instances and enables you to close the app when you have finished processing messages:</span></span>

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    } catch (ServiceBusException sbe) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="7ce14-177">Ez a kód azt feltételezi, hogy az IoT hubot az F1 (ingyenes) rétegen hozta létre.</span><span class="sxs-lookup"><span data-stu-id="7ce14-177">This code assumes you created your IoT hub in the F1 (free) tier.</span></span> <span data-ttu-id="7ce14-178">Az ingyenes IoT hub két, „0” és „1” nevű partícióval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7ce14-178">A free IoT hub has two partitions named "0" and "1".</span></span>

11. <span data-ttu-id="7ce14-179">Mentse és zárja be az App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-179">Save and close the App.java file.</span></span>

12. <span data-ttu-id="7ce14-180">Ha a **read-d2c-messages** alkalmazást a Maven használatával szeretné felépíteni, futtassa a következő parancsot a parancssorban a read-d2c-messages mappában:</span><span class="sxs-lookup"><span data-stu-id="7ce14-180">To build the **read-d2c-messages** app using Maven, execute the following command at the command prompt in the read-d2c-messages folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a><span data-ttu-id="7ce14-181">Eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ce14-181">Create a device app</span></span>
<span data-ttu-id="7ce14-182">Ebben a szakaszban egy Java-konzolalkalmazást hoz létre, amely egy, az eszközről a felhőbe irányuló üzeneteket egy IoT Hubra küldő eszközt szimulál.</span><span class="sxs-lookup"><span data-stu-id="7ce14-182">In this section, you create a Java console app that simulates a device that sends device-to-cloud messages to an IoT hub.</span></span>

1. <span data-ttu-id="7ce14-183">Az *Eszközidentitás létrehozása* szakaszban létrehozott iot-java-get-started mappában hozzon létre egy **simulated-device** nevű Maven-projektet a következő parancs beírásával a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="7ce14-183">In the iot-java-get-started folder you created in the *Create a device identity* section, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="7ce14-184">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="7ce14-184">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="7ce14-185">A parancssorban lépjen a simulated-device mappára.</span><span class="sxs-lookup"><span data-stu-id="7ce14-185">At your command prompt, navigate to the simulated-device folder.</span></span>

3. <span data-ttu-id="7ce14-186">Egy szövegszerkesztővel nyissa meg a pom.xml fájlt a simulated-device mappában, és adja hozzá a következő függőségeket a **függőségek** csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="7ce14-186">Using a text editor, open the pom.xml file in the simulated-device folder and add the following dependencies to the **dependencies** node.</span></span> <span data-ttu-id="7ce14-187">Ezzel a függőséggel használhatja az iothub-java-client csomagot az alkalmazásban, az IoT Hubbal való kommunikációhoz és Java-objektumok JSON-objektumokká szerializálásához:</span><span class="sxs-lookup"><span data-stu-id="7ce14-187">This dependency enables you to use the iothub-java-client package in your app to communicate with your IoT hub and to serialize Java objects to JSON:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="7ce14-188">Az **iot-device-client** legújabb verzióját a [Maven keresési funkciójával][lnk-maven-device-search] tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="7ce14-188">You can check for the latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

4. <span data-ttu-id="7ce14-189">Mentse és zárja be a pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-189">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="7ce14-190">Egy szövegszerkesztővel nyissa meg a simulated-device\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-190">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="7ce14-191">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="7ce14-191">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. <span data-ttu-id="7ce14-192">Adja hozzá a következő osztályszintű változókat az **App** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="7ce14-192">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="7ce14-193">A **{youriothubname}** helyőrző cseréje az IoT Hub nevére, valamint a **{yourdevicekey}** helyőrzőé az *Eszközidentitás létrehozása* szakaszban létrehozott eszközkulcsértékre:</span><span class="sxs-lookup"><span data-stu-id="7ce14-193">Replacing **{youriothubname}** with your IoT hub name, and **{yourdevicekey}** with the device key value you generated in the *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    <span data-ttu-id="7ce14-194">Ez a mintaalkalmazás a **protocol** változót használja egy **DeviceClient** objektum példányának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7ce14-194">This sample app uses the **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="7ce14-195">Az IoT Hubbal való kommunikációhoz MQTT, AMQP vagy HTTP protokollt használhat.</span><span class="sxs-lookup"><span data-stu-id="7ce14-195">You can use either the MQTT, AMQP, or HTTP protocol to communicate with IoT Hub.</span></span>

8. <span data-ttu-id="7ce14-196">Adja hozzá a következő beágyazott **TelemetryDataPoint** osztályt az **App** osztályon belül az eszköz által az IoT hubnak küldött telemetriai adatok meghatározásához:</span><span class="sxs-lookup"><span data-stu-id="7ce14-196">Add the following nested **TelemetryDataPoint** class inside the **App** class to specify the telemetry data your device sends to your IoT hub:</span></span>

    ```java
    private static class TelemetryDataPoint {
      public String deviceId;
      public double temperature;
      public double humidity;
   
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```
9. <span data-ttu-id="7ce14-197">Adja hozzá a következő beágyazott **EventCallback** osztályt az **App** osztályon belül az IoT Hub által az eszközalkalmazástól érkező üzenet feldolgozásakor visszaadott nyugtázási állapot megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7ce14-197">Add the following nested **EventCallback** class inside the **App** class to display the acknowledgement status that the IoT hub returns when it processes a message from the device app.</span></span> <span data-ttu-id="7ce14-198">Ez a metódus az alkalmazás fő szálát is értesíti, amikor üzenet fel lett dolgozva:</span><span class="sxs-lookup"><span data-stu-id="7ce14-198">This method also notifies the main thread in the app when the message has been processed:</span></span>
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. <span data-ttu-id="7ce14-199">Adja hozzá a következő beágyazott **MessageSender** osztályt az **App** osztályon belül.</span><span class="sxs-lookup"><span data-stu-id="7ce14-199">Add the following nested **MessageSender** class inside the **App** class.</span></span> <span data-ttu-id="7ce14-200">Ezen osztály **run** metódusa minta telemetriai adatokat hoz létre, amelyeket elküld az IoT hubnak, és nyugtázásra vár a következő üzenet elküldése előtt:</span><span class="sxs-lookup"><span data-stu-id="7ce14-200">The **run** method in this class generates sample telemetry data to send to your IoT hub and waits for an acknowledgement before sending the next message:</span></span>

    ```java
    private static class MessageSender implements Runnable {
      public void run()  {
        try {
          double minTemperature = 20;
          double minHumidity = 60;
          Random rand = new Random();
    
          while (true) {
            double currentTemperature = minTemperature + rand.nextDouble() * 15;
            double currentHumidity = minHumidity + rand.nextDouble() * 20;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.temperature = currentTemperature;
            telemetryDataPoint.humidity = currentHumidity;
    
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            msg.setProperty("temperatureAlert", (currentTemperature > 30) ? "true" : "false");
            msg.setMessageId(java.util.UUID.randomUUID().toString()); 
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

    <span data-ttu-id="7ce14-201">Ez a metódus elküld egy új, az eszközről a felhőbe irányuló üzenetet egy másodperccel azután, hogy az IoT hub nyugtázza az előző üzenetet.</span><span class="sxs-lookup"><span data-stu-id="7ce14-201">This method sends a new device-to-cloud message one second after the IoT hub acknowledges the previous message.</span></span> <span data-ttu-id="7ce14-202">Az üzenet egy JSON-szerializált objektumot tartalmaz az eszköz azonosítójával és véletlenszerűen előállított számokkal, amelyek egy hőmérséklet- és egy páratartalom-érzékelőt szimulálnak.</span><span class="sxs-lookup"><span data-stu-id="7ce14-202">The message contains a JSON-serialized object with the deviceId and randomly generated numbers to simulate a temperature sensor, and a humidity sensor.</span></span>

11. <span data-ttu-id="7ce14-203">Cserélje le a **main** metódust a következő kódra, amely létrehoz egy szálat az eszközről a felhőbe irányuló üzenetek elküldéséhez az IoT Hubra:</span><span class="sxs-lookup"><span data-stu-id="7ce14-203">Replace the **main** method with the following code that creates a thread to send device-to-cloud messages to your IoT hub:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. <span data-ttu-id="7ce14-204">Mentse és zárja be az App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce14-204">Save and close the App.java file.</span></span>

13. <span data-ttu-id="7ce14-205">Ha a **simulated-device** alkalmazást a Maven használatával szeretné felépíteni, futtassa a következő parancsot a parancssorban a simulated-device mappában:</span><span class="sxs-lookup"><span data-stu-id="7ce14-205">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> <span data-ttu-id="7ce14-206">Az egyszerűség kedvéért ez az oktatóanyag nem valósít meg semmilyen újrapróbálkozási házirendet.</span><span class="sxs-lookup"><span data-stu-id="7ce14-206">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="7ce14-207">Az éles kódban újrapróbálkozási házirendeket is meg kell valósítania (például egy exponenciális leállítást) a [tranziens hibakezelést][lnk-transient-faults] ismertető MSDN-cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="7ce14-207">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="7ce14-208">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="7ce14-208">Run the apps</span></span>

<span data-ttu-id="7ce14-209">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="7ce14-209">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="7ce14-210">A read-d2c mappában egy parancssorban futtassa a következő parancsot, amellyel megkezdheti az IoT Hub első partíciójának megfigyelését:</span><span class="sxs-lookup"><span data-stu-id="7ce14-210">At a command prompt in the read-d2c folder, run the following command to begin monitoring the first partition in your IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub-szolgáltatásalkalmazás az eszközről a felhőbe irányuló üzenetek figyeléséhez][7]

2. <span data-ttu-id="7ce14-212">A simulated-device mappában egy parancssorban futtassa a következő parancsot, amellyel megkezdheti a telemetriai adatok küldését az IoT Hubnak:</span><span class="sxs-lookup"><span data-stu-id="7ce14-212">At a command prompt in the simulated-device folder, run the following command to begin sending telemetry data to your IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT Hub-eszközalkalmazás az eszközről a felhőbe irányuló üzenetek küldéséhez][8]

3. <span data-ttu-id="7ce14-214">Az [Azure Portal][lnk-portal] **Használat** csempéje az IoT Hubnak küldött üzenetek számát jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="7ce14-214">The **Usage** tile in the [Azure portal][lnk-portal] shows the number of messages sent to the IoT hub:</span></span>

    ![Az Azure Portalon az IoT Hubnak küldött üzenetek számát megjelenítő Használat csempe][43]

## <a name="next-steps"></a><span data-ttu-id="7ce14-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ce14-216">Next steps</span></span>
<span data-ttu-id="7ce14-217">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="7ce14-217">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="7ce14-218">Ennek az eszközidentitásnak a segítségével lehetővé tette az eszközalkalmazásnak, hogy az eszközről a felhőbe irányuló üzeneteket küldjön az IoT Hubnak.</span><span class="sxs-lookup"><span data-stu-id="7ce14-218">You used this device identity to enable the device app to send device-to-cloud messages to the IoT hub.</span></span> <span data-ttu-id="7ce14-219">Emellett létrehozott egy alkalmazást, amely megjeleníti az IoT Hub által fogadott üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="7ce14-219">You also created an app that displays the messages received by the IoT hub.</span></span>

<span data-ttu-id="7ce14-220">További bevezetés az IoT Hub használatába, valamint egyéb IoT-forgatókönyvek megismerése:</span><span class="sxs-lookup"><span data-stu-id="7ce14-220">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="7ce14-221">[Kapcsolódás az eszközhöz][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="7ce14-221">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="7ce14-222">[Eszközfelügyelet – első lépések][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="7ce14-222">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="7ce14-223">[Ismerkedés az Azure IoT Edge szolgáltatással][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="7ce14-223">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="7ce14-224">Az IoT-megoldás kibővítésével és az eszközről a felhőbe irányuló üzenetek nagy léptékű feldolgozásával kapcsolatban tekintse meg [az eszközről a felhőbe irányuló üzenetek feldolgozását][lnk-process-d2c-tutorial] ismertető oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="7ce14-224">To learn how to extend your IoT solution and process device-to-cloud messages at scale, see the [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22
