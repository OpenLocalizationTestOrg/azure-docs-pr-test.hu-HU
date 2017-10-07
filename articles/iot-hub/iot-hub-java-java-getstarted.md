---
title: "aaaGet Azure IoT Hub (Java) használatába |} Microsoft Docs"
description: "Ismerje meg, hogyan toosend eszköz-felhő üzenetek tooAzure IoT Hub Java IoT SDK-k használatával. Szimulált eszköz és a szolgáltatás alkalmazások tooregister létrehozása az eszköz, üzenetküldés és üzenetek olvasni az IoT-központ."
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
ms.openlocfilehash: ac954f0522b46ed2a5b4a819bc611c13be0b9a9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-tooyour-iot-hub-using-java"></a><span data-ttu-id="f818e-104">Csatlakozás az eszköz tooyour IoT hub Java használatával</span><span class="sxs-lookup"><span data-stu-id="f818e-104">Connect your device tooyour IoT hub using Java</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="f818e-105">Ez az oktatóanyag végén hello három Java konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="f818e-105">At hello end of this tutorial, you have three Java console apps:</span></span>

* <span data-ttu-id="f818e-106">**Hozzon létre eszköz-identitás**, ami létrehoz egy eszközidentitás, és a biztonsági kulcsok tooconnect az eszközalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f818e-106">**create-device-identity**, which creates a device identity and associated security key tooconnect your device app.</span></span>
* <span data-ttu-id="f818e-107">**olvasási-d2c-üzenetek**, amely megjeleníti az eszköz alkalmazás által küldött hello telemetriai.</span><span class="sxs-lookup"><span data-stu-id="f818e-107">**read-d2c-messages**, which displays hello telemetry sent by your device app.</span></span>
* <span data-ttu-id="f818e-108">**Szimulált eszköz**, amely tooyour IoT-központ kapcsolódik a korábban létrehozott hello eszközidentitás, és minden második használatával hello MQTT protokoll telemetriai üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="f818e-108">**simulated-device**, which connects tooyour IoT hub with hello device identity created earlier, and sends a telemetry message every second using hello MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="f818e-109">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="f818e-109">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both apps toorun on devices and your solution back end.</span></span>

<span data-ttu-id="f818e-110">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f818e-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="f818e-111">hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="f818e-111">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span> 
* [<span data-ttu-id="f818e-112">Maven 3</span><span class="sxs-lookup"><span data-stu-id="f818e-112">Maven 3</span></span>](https://maven.apache.org/install.html) 
* <span data-ttu-id="f818e-113">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="f818e-113">An active Azure account.</span></span> <span data-ttu-id="f818e-114">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="f818e-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="f818e-115">Utolsó lépésként, jegyezze fel a hello **elsődleges kulcs** érték.</span><span class="sxs-lookup"><span data-stu-id="f818e-115">As a final step, make a note of hello **Primary key** value.</span></span> <span data-ttu-id="f818e-116">Kattintson a **végpontok** és hello **események** beépített végpont.</span><span class="sxs-lookup"><span data-stu-id="f818e-116">Then click **Endpoints** and hello **Events** built-in endpoint.</span></span> <span data-ttu-id="f818e-117">A hello **tulajdonságok** panelen, jegyezze fel a hello **Event Hub-kompatibilis neve** és hello **Event Hub-kompatibilis végpont** cím.</span><span class="sxs-lookup"><span data-stu-id="f818e-117">On hello **Properties** blade, make a note of hello **Event Hub-compatible name** and hello **Event Hub-compatible endpoint** address.</span></span> <span data-ttu-id="f818e-118">Erre a három értékre szüksége lesz a **read-d2c-messages** alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f818e-118">You need these three values when you create your **read-d2c-messages** app.</span></span>

![Az Azure Portal IoT Hub Üzenetküldés panelje][6]

<span data-ttu-id="f818e-120">Ezzel létrehozta az IoT Hubot.</span><span class="sxs-lookup"><span data-stu-id="f818e-120">You have now created your IoT hub.</span></span> <span data-ttu-id="f818e-121">Hello IoT-központ állomásnév, IoT-központ kapcsolati karakterláncot, IoT Hub elsődleges kulcs, Event Hub-kompatibilis neve és Event Hub-kompatibilis végpont ebben az oktatóanyagban toocomplete szüksége van.</span><span class="sxs-lookup"><span data-stu-id="f818e-121">You have hello IoT Hub host name, IoT Hub connection string, IoT Hub Primary Key, Event Hub-compatible name, and Event Hub-compatible endpoint you need toocomplete this tutorial.</span></span>

## <a name="create-a-device-identity"></a><span data-ttu-id="f818e-122">Eszközidentitás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f818e-122">Create a device identity</span></span>
<span data-ttu-id="f818e-123">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely az IoT hub a hello identitásjegyzékhez hoz létre egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="f818e-123">In this section, you create a Java console app that creates a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="f818e-124">Egy eszköz nem lehet kapcsolódni a tooIoT hub, kivéve, ha azt egy bejegyzéssel rendelkezik hello identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="f818e-124">A device cannot connect tooIoT hub unless it has an entry in hello identity registry.</span></span> <span data-ttu-id="f818e-125">További információkért lásd: hello **Identitásjegyzékhez** hello szakasza [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="f818e-125">For more information, see hello **Identity Registry** section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="f818e-126">A konzol alkalmazás futtatásakor egy eszköz egyedi Azonosítót hoz létre, és az, hogy az eszköz használhatja tooidentify magát, eszköz-felhő küld a kulcs üzenetek tooIoT Hub.</span><span class="sxs-lookup"><span data-stu-id="f818e-126">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span>

1. <span data-ttu-id="f818e-127">Hozzon létre egy iot-java-get-started nevű üres mappát.</span><span class="sxs-lookup"><span data-stu-id="f818e-127">Create an empty folder called iot-java-get-started.</span></span> <span data-ttu-id="f818e-128">Hello iot-java-get-started mappában, úgynevezett Maven-projekt létrehozása **létrehozása eszköz-identitás** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="f818e-128">In hello iot-java-get-started folder, create a Maven project called **create-device-identity** using hello following command at your command prompt.</span></span> <span data-ttu-id="f818e-129">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="f818e-129">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="f818e-130">A parancssorban keresse meg a toohello létrehozása eszköz-identitás mappa.</span><span class="sxs-lookup"><span data-stu-id="f818e-130">At your command prompt, navigate toohello create-device-identity folder.</span></span>

3. <span data-ttu-id="f818e-131">Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello létrehozása eszköz-identitás mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="f818e-131">Using a text editor, open hello pom.xml file in hello create-device-identity folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="f818e-132">A függőség lehetővé teszi, hogy Ön toouse hello iot-szolgáltatás-ügyfélcsomag az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="f818e-132">This dependency enables you toouse hello iot-service-client package in your app:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="f818e-133">Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="f818e-133">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="f818e-134">Mentse és zárja be a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="f818e-134">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="f818e-135">Egy szövegszerkesztőben nyissa meg hello create-device-identity\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="f818e-135">Using a text editor, open hello create-device-identity\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="f818e-136">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="f818e-136">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="f818e-137">Adja hozzá a következő osztály változók toohello hello **App** osztály cseréje **{yourhubconnectionstring}** hello értékre a beállításértékeket korábban:</span><span class="sxs-lookup"><span data-stu-id="f818e-137">Add hello following class-level variables toohello **App** class, replacing **{yourhubconnectionstring}** with hello value your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. <span data-ttu-id="f818e-138">Hello hello aláírása módosítása **fő** metódus tooinclude hello kivételek az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f818e-138">Modify hello signature of hello **main** method tooinclude hello exceptions as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. <span data-ttu-id="f818e-139">Adja hozzá a kódot hello hello szövege a következő hello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="f818e-139">Add hello following code as hello body of hello **main** method.</span></span> <span data-ttu-id="f818e-140">Ez a kód egy *javadevice* nevű eszközt hoz létre az IoT Hub identitásjegyzékében, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="f818e-140">This code creates a device called *javadevice* in your IoT Hub identity registry if doesn't already exist.</span></span> <span data-ttu-id="f818e-141">Hello Eszközazonosító és a kulcs, amely később kell majd megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="f818e-141">It then displays hello device ID and key that you need later:</span></span>

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If hello device already exists.
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
      // If hello device already exists.
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

10. <span data-ttu-id="f818e-142">Mentse és zárja be hello App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="f818e-142">Save and close hello App.java file.</span></span>

11. <span data-ttu-id="f818e-143">toobuild hello **létrehozása eszköz-identitás** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello létrehozása eszköz-identitás mappában hello:</span><span class="sxs-lookup"><span data-stu-id="f818e-143">toobuild hello **create-device-identity** app using Maven, execute hello following command at hello command prompt in hello create-device-identity folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. <span data-ttu-id="f818e-144">toorun hello **létrehozása eszköz-identitás** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello létrehozása eszköz-identitás mappában hello:</span><span class="sxs-lookup"><span data-stu-id="f818e-144">toorun hello **create-device-identity** app using Maven, execute hello following command at hello command prompt in hello create-device-identity folder:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. <span data-ttu-id="f818e-145">Jegyezze fel a hello **Eszközazonosító** és **eszközkulcs**.</span><span class="sxs-lookup"><span data-stu-id="f818e-145">Make a note of hello **Device ID** and **Device key**.</span></span> <span data-ttu-id="f818e-146">Ezek az értékek később szüksége, amely a központ eszközként tooIoT alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f818e-146">You need these values later when you create an app that connects tooIoT Hub as a device.</span></span>

> [!NOTE]
> <span data-ttu-id="f818e-147">az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja.</span><span class="sxs-lookup"><span data-stu-id="f818e-147">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="f818e-148">Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, és egy engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol.</span><span class="sxs-lookup"><span data-stu-id="f818e-148">It stores device IDs and keys toouse as security credentials and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="f818e-149">Ha a kell toostore más eszközre vonatkozó metaadatok, azt kell használni az alkalmazás-specifikus tárolási.</span><span class="sxs-lookup"><span data-stu-id="f818e-149">If your app needs toostore other device-specific metadata, it should use an app-specific store.</span></span> <span data-ttu-id="f818e-150">További információkért lásd: hello [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="f818e-150">For more information, see hello [IoT Hub developer guide][lnk-devguide-identity].</span></span>

## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="f818e-151">Az eszközről a felhőbe irányuló üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="f818e-151">Receive device-to-cloud messages</span></span>

<span data-ttu-id="f818e-152">Ebben a szakaszban egy Java-konzolalkalmazást hoz létre, amely az eszközről a felhőbe irányuló üzeneteket olvas az IoT Hubról.</span><span class="sxs-lookup"><span data-stu-id="f818e-152">In this section, you create a Java console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="f818e-153">Az IoT-központ mutatja egy [Eseményközpont][lnk-event-hubs-overview]-kompatibilis végpont tooenable tooread eszközről a felhőbe üzenetek meg.</span><span class="sxs-lookup"><span data-stu-id="f818e-153">An IoT hub exposes an [Event Hub][lnk-event-hubs-overview]-compatible endpoint tooenable you tooread device-to-cloud messages.</span></span> <span data-ttu-id="f818e-154">egyszerű tookeep dolog, ebben az oktatóanyagban egy egyszerű olvasót, amely nem alkalmas a magas teljesítmény központi telepítés hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f818e-154">tookeep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="f818e-155">Hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] az oktatóanyag bemutatja, hogyan tooprocess eszköz-felhő léptékű üzenetek.</span><span class="sxs-lookup"><span data-stu-id="f818e-155">hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial shows you how tooprocess device-to-cloud messages at scale.</span></span> <span data-ttu-id="f818e-156">Hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] oktatóanyag további információkat biztosítanak a tooprocess Eseményközpontokból származó üzenetek és alkalmazható toohello IoT Hub Event Hub-kompatibilis végpontok.</span><span class="sxs-lookup"><span data-stu-id="f818e-156">hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial provides further information on how tooprocess messages from Event Hubs and is applicable toohello IoT Hub Event Hub-compatible endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="f818e-157">hello Event Hub-kompatibilis végpont eszközről a felhőbe üzenetek mindig olvasásához hello AMQP protokollt használja.</span><span class="sxs-lookup"><span data-stu-id="f818e-157">hello Event Hub-compatible endpoint for reading device-to-cloud messages always uses hello AMQP protocol.</span></span>

1. <span data-ttu-id="f818e-158">Hello iot-java-get-started mappában létrehozott hello *hozzon létre egy eszközidentitás* területen nevű Maven-projekt létrehozása **d2c-üzenetek olvasása** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="f818e-158">In hello iot-java-get-started folder you created in hello *Create a device identity* section, create a Maven project called **read-d2c-messages** using hello following command at your command prompt.</span></span> <span data-ttu-id="f818e-159">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="f818e-159">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="f818e-160">A parancssorban keresse meg a toohello olvasás-d2c-üzenetek mappa.</span><span class="sxs-lookup"><span data-stu-id="f818e-160">At your command prompt, navigate toohello read-d2c-messages folder.</span></span>

3. <span data-ttu-id="f818e-161">Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello olvasás-d2c-üzenetek mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="f818e-161">Using a text editor, open hello pom.xml file in hello read-d2c-messages folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="f818e-162">A függőség toouse hello eventhubs-ügyfélcsomagját hello Event Hub-kompatibilis végpont az az alkalmazás tooread lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="f818e-162">This dependency enables you toouse hello eventhubs-client package in your app tooread from hello Event Hub-compatible endpoint:</span></span>

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. <span data-ttu-id="f818e-163">Mentse és zárja be a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="f818e-163">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="f818e-164">Egy szövegszerkesztőben nyissa meg hello read-d2c-messages\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="f818e-164">Using a text editor, open hello read-d2c-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="f818e-165">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="f818e-165">Add hello following **import** statements toohello file:</span></span>

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. <span data-ttu-id="f818e-166">Adja hozzá a következő osztály szintű változó toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="f818e-166">Add hello following class-level variable toohello **App** class.</span></span> <span data-ttu-id="f818e-167">Cserélje le **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, és **{youreventhubcompatiblename}** korábban feljegyzett hello értékekkel:</span><span class="sxs-lookup"><span data-stu-id="f818e-167">Replace **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, and **{youreventhubcompatiblename}** with hello values you noted previously:</span></span>

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. <span data-ttu-id="f818e-168">Adja hozzá a következő hello **receiveMessages** metódus toohello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="f818e-168">Add hello following **receiveMessages** method toohello **App** class.</span></span> <span data-ttu-id="f818e-169">Ezzel a módszerrel hoz létre egy **EventHubClient** tooconnect toohello Event Hub-kompatibilis végpont példányt, és aszinkron módon létrehoz egy **PartitionReceiver** példány tooread az Eseményközpontban lévő a partíció.</span><span class="sxs-lookup"><span data-stu-id="f818e-169">This method creates an **EventHubClient** instance tooconnect toohello Event Hub-compatible endpoint and then asynchronously creates a **PartitionReceiver** instance tooread from an Event Hub partition.</span></span> <span data-ttu-id="f818e-170">Folyamatosan fut a hurokban, és kiírja a hello üzenet adatai, amíg hello app véget nem ér.</span><span class="sxs-lookup"><span data-stu-id="f818e-170">It loops continuously and prints hello message details until hello app terminates.</span></span>

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed toocreate client: " + e.getMessage());
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
                System.out.println("Failed tooreceive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed toocreate receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="f818e-171">Ez a módszer egy szűrőt használja, amikor hello fogadó létrehozza, hogy hello fogadó csak olvassa be küldött üzenetek tooIoT Hub után hello fogadó elindul.</span><span class="sxs-lookup"><span data-stu-id="f818e-171">This method uses a filter when it creates hello receiver so that hello receiver only reads messages sent tooIoT Hub after hello receiver starts running.</span></span> <span data-ttu-id="f818e-172">Ez a módszer akkor hasznos, egy tesztkörnyezetben, így hello aktuális készletében lévő üzenetek.</span><span class="sxs-lookup"><span data-stu-id="f818e-172">This technique is useful in a test environment so you can see hello current set of messages.</span></span> <span data-ttu-id="f818e-173">Éles környezetben, a kódot kell győződjön meg arról, hogy feldolgozza az összes köszönőüzenetei - további információt találhat hello [hogyan tooprocess IoT Hub eszköz-a-felhőbe küldött üzeneteket] [ lnk-process-d2c-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f818e-173">In a production environment, your code should make sure that it processes all hello messages - for more information, see hello [How tooprocess IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

9. <span data-ttu-id="f818e-174">Hello hello aláírása módosítása **fő** metódus tooinclude hello kivétel az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f818e-174">Modify hello signature of hello **main** method tooinclude hello exception as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. <span data-ttu-id="f818e-175">Adja hozzá a következő kód toohello hello **fő** metódus a hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="f818e-175">Add hello following code toohello **main** method in hello **App** class.</span></span> <span data-ttu-id="f818e-176">Ez a kód létrehoz két hello **EventHubClient** és **PartitionReceiver** példánya, és lehetővé teszi tooclose hello app üzenetek feldolgozása után:</span><span class="sxs-lookup"><span data-stu-id="f818e-176">This code creates hello two **EventHubClient** and **PartitionReceiver** instances and enables you tooclose hello app when you have finished processing messages:</span></span>

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER tooexit.");
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
    > <span data-ttu-id="f818e-177">Ez a kód azt feltételezi, hogy az IoT hub létrehozott hello F1 (ingyenes) csomagot.</span><span class="sxs-lookup"><span data-stu-id="f818e-177">This code assumes you created your IoT hub in hello F1 (free) tier.</span></span> <span data-ttu-id="f818e-178">Az ingyenes IoT hub két, „0” és „1” nevű partícióval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f818e-178">A free IoT hub has two partitions named "0" and "1".</span></span>

11. <span data-ttu-id="f818e-179">Mentse és zárja be hello App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="f818e-179">Save and close hello App.java file.</span></span>

12. <span data-ttu-id="f818e-180">toobuild hello **d2c-üzenetek olvasása** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello olvasás-d2c-üzenetek mappában hello:</span><span class="sxs-lookup"><span data-stu-id="f818e-180">toobuild hello **read-d2c-messages** app using Maven, execute hello following command at hello command prompt in hello read-d2c-messages folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a><span data-ttu-id="f818e-181">Eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f818e-181">Create a device app</span></span>
<span data-ttu-id="f818e-182">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely szimulálja egy eszköz, amelyet az eszköz a felhőbe küldött üzeneteket tooan IoT-központ küld.</span><span class="sxs-lookup"><span data-stu-id="f818e-182">In this section, you create a Java console app that simulates a device that sends device-to-cloud messages tooan IoT hub.</span></span>

1. <span data-ttu-id="f818e-183">Hello iot-java-get-started mappában létrehozott hello *hozzon létre egy eszközidentitás* területen nevű Maven-projekt létrehozása **szimulált eszköz** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="f818e-183">In hello iot-java-get-started folder you created in hello *Create a device identity* section, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="f818e-184">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="f818e-184">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="f818e-185">A parancssorban lépjen a toohello szimulált eszköz mappában.</span><span class="sxs-lookup"><span data-stu-id="f818e-185">At your command prompt, navigate toohello simulated-device folder.</span></span>

3. <span data-ttu-id="f818e-186">Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello szimulált eszköz mappában, és adja hozzá a következő függőségek toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="f818e-186">Using a text editor, open hello pom.xml file in hello simulated-device folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="f818e-187">A függőség lehetővé teszi, hogy Ön toouse hello IOT hubbal-java-ügyfélcsomagját a app toocommunicate, az IoT-központ és tooserialize Java objektumok tooJSON:</span><span class="sxs-lookup"><span data-stu-id="f818e-187">This dependency enables you toouse hello iothub-java-client package in your app toocommunicate with your IoT hub and tooserialize Java objects tooJSON:</span></span>

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
    > <span data-ttu-id="f818e-188">Ellenőrizze, hogy hello legújabb verziójának **iot-eszközügyfél** használatával [Maven keresési][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="f818e-188">You can check for hello latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

4. <span data-ttu-id="f818e-189">Mentse és zárja be a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="f818e-189">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="f818e-190">Egy szövegszerkesztőben nyissa meg hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="f818e-190">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="f818e-191">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="f818e-191">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. <span data-ttu-id="f818e-192">Adja hozzá a következő osztály változók toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="f818e-192">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="f818e-193">A mag cseréje **{youriothubname}** az IoT hub nevű és **{yourdevicekey}** hello eszköz kulcsérték hello hozta létre a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="f818e-193">Replacing **{youriothubname}** with your IoT hub name, and **{yourdevicekey}** with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    <span data-ttu-id="f818e-194">Ezen PéldaAlkalmazás használ hello **protokoll** változó, amikor elindítja a **DeviceClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="f818e-194">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="f818e-195">Az IoT-központ vagy hello MQTT, AMQP vagy HTTP protokollt toocommunicate használható.</span><span class="sxs-lookup"><span data-stu-id="f818e-195">You can use either hello MQTT, AMQP, or HTTP protocol toocommunicate with IoT Hub.</span></span>

8. <span data-ttu-id="f818e-196">Adja hozzá a beágyazott hello következő **TelemetryDataPoint** osztály belsejében hello **App** toospecify hello telemetriai adatokat az eszköz küld tooyour IoT-központ osztályban:</span><span class="sxs-lookup"><span data-stu-id="f818e-196">Add hello following nested **TelemetryDataPoint** class inside hello **App** class toospecify hello telemetry data your device sends tooyour IoT hub:</span></span>

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
9. <span data-ttu-id="f818e-197">Adja hozzá a beágyazott hello következő **EventCallback** osztály belsejében hello **App** osztály toodisplay hello nyugtázási állapotát, amely az IoT-központ hello ad vissza, ha hello eszközalkalmazás üzenetét feldolgozza.</span><span class="sxs-lookup"><span data-stu-id="f818e-197">Add hello following nested **EventCallback** class inside hello **App** class toodisplay hello acknowledgement status that hello IoT hub returns when it processes a message from hello device app.</span></span> <span data-ttu-id="f818e-198">Ez a módszer is értesíti hello fő szálnak hello alkalmazásban, üdvözlőüzenetére feldolgozásakor:</span><span class="sxs-lookup"><span data-stu-id="f818e-198">This method also notifies hello main thread in hello app when hello message has been processed:</span></span>
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toomessage with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. <span data-ttu-id="f818e-199">Adja hozzá a beágyazott hello következő **MessageSender** osztály belsejében hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="f818e-199">Add hello following nested **MessageSender** class inside hello **App** class.</span></span> <span data-ttu-id="f818e-200">Hello **futtatása** metódus Ez az osztály a minta telemetriai adatok toosend tooyour IoT-központ állít elő, és megvárja-e a nyugtázást hello tovább üzenet küldése előtt:</span><span class="sxs-lookup"><span data-stu-id="f818e-200">hello **run** method in this class generates sample telemetry data toosend tooyour IoT hub and waits for an acknowledgement before sending hello next message:</span></span>

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

    <span data-ttu-id="f818e-201">Ez a módszer új eszközről a felhőbe üzenetet küld egy másodperc után hello IoT-központ elismeri hello előző üzenetét.</span><span class="sxs-lookup"><span data-stu-id="f818e-201">This method sends a new device-to-cloud message one second after hello IoT hub acknowledges hello previous message.</span></span> <span data-ttu-id="f818e-202">üdvözlőüzenetére hello deviceId rendelkező JSON-szerializált objektumot tartalmaz, és véletlenszerűen generált számok toosimulate hőmérséklet-érzékelő és a páratartalom érzékelő.</span><span class="sxs-lookup"><span data-stu-id="f818e-202">hello message contains a JSON-serialized object with hello deviceId and randomly generated numbers toosimulate a temperature sensor, and a humidity sensor.</span></span>

11. <span data-ttu-id="f818e-203">Cserélje le a hello **fő** hello kódot, amely létrehoz egy szál toosend eszköz a felhőbe küldött üzeneteket tooyour IoT-központ a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="f818e-203">Replace hello **main** method with hello following code that creates a thread toosend device-to-cloud messages tooyour IoT hub:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER tooexit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. <span data-ttu-id="f818e-204">Mentse és zárja be hello App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="f818e-204">Save and close hello App.java file.</span></span>

13. <span data-ttu-id="f818e-205">toobuild hello **szimulált eszköz** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello szimulált eszköz mappában hello:</span><span class="sxs-lookup"><span data-stu-id="f818e-205">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> <span data-ttu-id="f818e-206">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="f818e-206">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="f818e-207">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="f818e-207">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="f818e-208">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="f818e-208">Run hello apps</span></span>

<span data-ttu-id="f818e-209">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="f818e-209">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="f818e-210">Parancssorba hello olvasás-d2c mappában futtassa a következő parancs toobegin hello első partíció az IoT hub a figyelési hello:</span><span class="sxs-lookup"><span data-stu-id="f818e-210">At a command prompt in hello read-d2c folder, run hello following command toobegin monitoring hello first partition in your IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT-központ szolgáltatás toomonitor eszközről a felhőbe alkalmazásüzenetek][7]

2. <span data-ttu-id="f818e-212">Parancsot egy parancssorba hello szimulált eszköz mappában futtassa a következő parancs toobegin telemetriai adatok tooyour IoT-központ küldése hello:</span><span class="sxs-lookup"><span data-stu-id="f818e-212">At a command prompt in hello simulated-device folder, run hello following command toobegin sending telemetry data tooyour IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT Hub eszközre app toosend eszközről a felhőbe küldött üzenetek][8]

3. <span data-ttu-id="f818e-214">Hello **használati** hello csempére [Azure-portálon] [ lnk-portal] mutat be hello küldött üzenetek toohello IoT-központ száma:</span><span class="sxs-lookup"><span data-stu-id="f818e-214">hello **Usage** tile in hello [Azure portal][lnk-portal] shows hello number of messages sent toohello IoT hub:</span></span>

    ![Az Azure portál használata csempe ábrázoló száma küldött üzenetek tooIoT Hub][43]

## <a name="next-steps"></a><span data-ttu-id="f818e-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f818e-216">Next steps</span></span>
<span data-ttu-id="f818e-217">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="f818e-217">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="f818e-218">Az eszköz identitása tooenable hello eszköz alkalmazás toosend eszköz a felhőbe küldött üzeneteket toohello IoT-központ használta.</span><span class="sxs-lookup"><span data-stu-id="f818e-218">You used this device identity tooenable hello device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="f818e-219">Is létrehozott alkalmazás hello IoT-központ által fogadott hello üzeneteket jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="f818e-219">You also created an app that displays hello messages received by hello IoT hub.</span></span>

<span data-ttu-id="f818e-220">első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:</span><span class="sxs-lookup"><span data-stu-id="f818e-220">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="f818e-221">[Kapcsolódás az eszközhöz][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="f818e-221">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="f818e-222">[Eszközfelügyelet – első lépések][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="f818e-222">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="f818e-223">[Ismerkedés az Azure IoT Edge szolgáltatással][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="f818e-223">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="f818e-224">toolearn hogyan tooextend az IoT megoldás és a folyamat eszközről a felhőbe üzenetek léptékű: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f818e-224">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
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
