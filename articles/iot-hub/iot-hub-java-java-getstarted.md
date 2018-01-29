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
ms.openlocfilehash: 5f41dd62d3f074eebe0c996a9c14bc7811d5b365
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-java"></a>Az eszköz csatlakoztatása az IoT Hubhoz Javával
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Az oktatóanyag elvégzése után három Java-konzolalkalmazással fog rendelkezni:

* A **create-device-identity** egy eszközidentitást, valamint egy társított biztonsági kulcsot hoz létre, amellyel csatlakozhat az eszközalkalmazáshoz.
* A **read-d2c-messages** megjeleníti az eszközalkalmazások által küldött telemetriát.
* A **simulated-device** csatlakozik az IoT Hubhoz a korábban létrehozott eszközidentitással, és az MQTT protokoll használatával másodpercenként telemetriai üzenetet küld.

> [!NOTE]
> Az Azure IoT SDK-kat használhatja az eszközökön és a megoldás háttérrendszerén futó alkalmazások összeállításához egyaránt. Ezekről az [Azure IoT SDK-k][lnk-hub-sdks] című témakörben talál további információt.

Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:

* A legújabb [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Az utolsó lépésben jegyezze fel az **Elsődleges kulcs** értékét. Ezután kattintson a **Végpontok**, majd az **Események** beépített végpontra. A **Tulajdonságok** panelen jegyezze fel az **Event Hub-kompatibilis nevet** és az **Event Hub-kompatibilis végpont** címét. Erre a három értékre szüksége lesz a **read-d2c-messages** alkalmazás létrehozásakor.

![Az Azure Portal IoT Hub Üzenetküldés panelje][6]

Ezzel létrehozta az IoT Hubot. Rendelkezik az oktatóanyag teljesítéséhez szükséges IoT Hub-állomásnévvel és kapcsolati karakterlánccal, IoT Hub elsődleges kulccsal, valamint Event Hubs-kompatibilis névvel és végponttal.

## <a name="create-a-device-identity"></a>Eszközidentitás létrehozása
Ebben a szakaszban egy Java-konzolalkalmazást fog létrehozni, amely egy új eszközidentitást hoz létre az IoT Hub identitásjegyzékében. Egy eszköz csak akkor tud csatlakozni az IoT Hubhoz, ha be van jegyezve az identitásjegyzékbe. További információkért lásd az [IoT Hub fejlesztői útmutatójának][lnk-devguide-identity] **Identitásjegyzék** című szakaszát. A konzolalkalmazás egy egyedi eszközazonosítót állít elő a futtatásakor, valamint egy kulcsot, amellyel az eszköz azonosítani tudja magát, amikor az eszközről a felhőbe irányuló üzeneteket küld az IoT Hubnak.

1. Hozzon létre egy iot-java-get-started nevű üres mappát. Az iot-java-get-started mappában hozzon létre egy **create-device-identity** nevű Maven-projektet a következő parancs beírásával a parancssorba. Látható, hogy ez egyetlen hosszú parancs:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorban lépjen a create-device-identity mappára.

3. Egy szövegszerkesztővel nyissa meg a pom.xml fájlt a create-device-identity mappában, és adja hozzá a következő függőséget a **függőségek** csomóponthoz. Ezzel a függőséggel használhatja az iot-service-client csomagot az alkalmazásban:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Az **iot-service-client** legújabb verzióját a [Maven keresési funkciójával][lnk-maven-service-search] tekintheti meg.

4. Mentse és zárja be a pom.xml fájlt.

5. Egy szövegszerkesztővel nyissa meg a create-device-identity\src\main\java\com\mycompany\app\App.java fájlt.

6. Adja hozzá a következő **importálási** utasításokat a fájlhoz:

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Adja a következő osztályszintű változókat az **App** osztályhoz, lecserélve a **{yourhubconnectionstring}** helyőrzőt a korábban feljegyzett értékkel:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. Módosítsa úgy a **main** metódus aláírását, hogy tartalmazza az alábbi kivételeket:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. Adja hozzá a következő kódot a **main** metódus törzseként. Ez a kód egy *javadevice* nevű eszközt hoz létre az IoT Hub identitásjegyzékében, ha még nem létezik. Ezután megjeleníti az eszközazonosítót és a kulcsot, amelyekre később szüksége lesz:

    ```java
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
    ```

10. Mentse és zárja be az App.java fájlt.

11. Ha a **create-device-identity** alkalmazást a Maven használatával szeretné felépíteni, futtassa a következő parancsot a parancssorban a create-device-identity mappában:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. Ha a **create-device-identity** alkalmazást a Maven használatával szeretné futtatni, futtassa a következő parancsot a parancssorban a create-device-identity mappában:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Jegyezze fel az **eszköz azonosítóját** és az **eszköz kulcsát**. Ezekre az értékekre később szüksége lesz, amikor az IoT Hubhoz eszközként csatlakozó alkalmazást hoz létre.

> [!NOTE]
> Az IoT Hub-identitásjegyzék csak az IoT Hub biztonságos elérésének biztosításához tárolja az eszközidentitásokat. Az eszközazonosítókat és kulcsokat biztonsági hitelesítő adatokként tárolja, valamint tartalmaz egy engedélyezve/letiltva jelzőt, amellyel letilthatja egy adott eszköz hozzáférését. Ha az alkalmazásnak más eszközspecifikus metaadatokat kell tárolnia, egy alkalmazásspecifikus tárolót kell használnia. További információ: [IoT Hub fejlesztői útmutató][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Az eszközről a felhőbe irányuló üzenetek fogadása

Ebben a szakaszban egy Java-konzolalkalmazást hoz létre, amely az eszközről a felhőbe irányuló üzeneteket olvas az IoT Hubról. Az IoT Hub egy [Event Hubs][lnk-event-hubs-overview]-kompatibilis végpontot tesz közzé, hogy lehetővé tegye az eszközről a felhőbe irányuló üzenetek olvasását. Az egyszerűség érdekében ez az oktatóanyag egy alapszintű olvasót hoz létre, amely nem alkalmas nagy átviteli sebességű üzemelő példányokhoz. Az eszközről a felhőbe irányuló üzenetek nagy léptékű feldolgozásával kapcsolatban lásd [az eszközről a felhőbe irányuló üzenetek feldolgozását][lnk-process-d2c-tutorial] ismertető oktatóanyagot. Az Event Hubs szolgáltatástól érkező üzenetek feldolgozásával kapcsolatos további információkért lásd [az Event Hubs használatának első lépéseit][lnk-eventhubs-tutorial] ismertető oktatóanyagot. Ez az IoT Hub Event Hub-kompatibilis végpontjaira érvényes.

> [!NOTE]
> Az eszközről a felhőbe irányuló üzenetek olvasásához használt Event Hub-kompatibilis végpontok mindig az AMQP protokollt használják.

1. Az *Eszközidentitás létrehozása* szakaszban létrehozott iot-java-get-started mappában hozzon létre egy **read-d2c-messages** nevű Maven-projektet a következő parancs beírásával a parancssorba. Látható, hogy ez egyetlen hosszú parancs:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorban lépjen a read-d2c-messages mappára.

3. Egy szövegszerkesztővel nyissa meg a pom.xml fájlt a read-d2c-messages mappában, és adja hozzá a következő függőséget a **függőségek** csomóponthoz. Ezzel a függőséggel használhatja az eventhubs-client csomagot az alkalmazásban az Event Hubs-kompatibilis végpontról való olvasáshoz:

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.15.0</version> 
    </dependency>
    ```

4. Mentse és zárja be a pom.xml fájlt.

5. Egy szövegszerkesztővel nyissa meg a read-d2c-messages\src\main\java\com\mycompany\app\App.java fájlt.

6. Adja hozzá a következő **importálási** utasításokat a fájlhoz:

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. Adja hozzá a következő osztályszintű változót az **App** osztályhoz. Cserélje le a **{youriothubkey}**, **{youreventhubcompatibleendpoint}** és **{youreventhubcompatiblename}** helyőrzőket a korábban feljegyzett értékekre:

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Adja hozzá a következő **receiveMessages** metódust az **App** osztályhoz. Ez a metódus **EventHubClient** példányt hoz létre, amely az Event Hub-kompatibilis végponthoz csatlakozik, majd aszinkron módon létrehoz egy **PartitionReceiver** példányt, amely egy Event Hub-partícióról olvas. Folyamatosan hurkokat alkot, és kinyomtatja az üzenet részleteit az alkalmazás leállásáig.

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
                    System.out.println("Got some events");
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
   > Ez a metódus szűrőt használ a fogadó létrehozásakor, hogy a fogadó csak a fogadó futtatásának megkezdése után az IoT Hubra küldött üzeneteket olvassa. Ez a technika tesztkörnyezetben hasznos, mivel láthatja az aktuális üzeneteket. Éles környezetben azonban a kódnak biztosítania kell, hogy az összes üzenetet feldolgozza. További információért lásd a következő oktatóanyagot: [Eszközről a felhőbe irányuló IoT Hub-üzenetek feldolgozása][lnk-process-d2c-tutorial].

9. Módosítsa úgy a **main** metódus aláírását, hogy tartalmazza az alábbi kivételt:

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. Adja hozzá a következő kódot a **main** metódushoz az **App** osztályban. Ez a kód létrehozza a két **EventHubClient** és **PartitionReceiver** példányt, és lehetővé teszi, hogy bezárja az alkalmazást az üzenetek feldolgozásának befejezése után:

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
    } catch (Exception e) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > Ez a kód azt feltételezi, hogy az IoT hubot az F1 (ingyenes) rétegen hozta létre. Az ingyenes IoT hub két, „0” és „1” nevű partícióval rendelkezik.

11. Mentse és zárja be az App.java fájlt.

12. Ha a **read-d2c-messages** alkalmazást a Maven használatával szeretné felépíteni, futtassa a következő parancsot a parancssorban a read-d2c-messages mappában:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Eszközalkalmazás létrehozása
Ebben a szakaszban egy Java-konzolalkalmazást hoz létre, amely egy, az eszközről a felhőbe irányuló üzeneteket egy IoT Hubra küldő eszközt szimulál.

1. Az *Eszközidentitás létrehozása* szakaszban létrehozott iot-java-get-started mappában hozzon létre egy **simulated-device** nevű Maven-projektet a következő parancs beírásával a parancssorba. Látható, hogy ez egyetlen hosszú parancs:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorban lépjen a simulated-device mappára.

3. Egy szövegszerkesztővel nyissa meg a pom.xml fájlt a simulated-device mappában, és adja hozzá a következő függőségeket a **függőségek** csomóponthoz. Ezzel a függőséggel használhatja az iothub-java-client csomagot az alkalmazásban, az IoT Hubbal való kommunikációhoz és Java-objektumok JSON-objektumokká szerializálásához:

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
    > Az **iot-device-client** legújabb verzióját a [Maven keresési funkciójával][lnk-maven-device-search] tekintheti meg.

4. Mentse és zárja be a pom.xml fájlt.

5. Egy szövegszerkesztővel nyissa meg a simulated-device\src\main\java\com\mycompany\app\App.java fájlt.

6. Adja hozzá a következő **importálási** utasításokat a fájlhoz:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Adja hozzá a következő osztályszintű változókat az **App** osztályhoz. A **{youriothubname}** helyőrző cseréje az IoT Hub nevére, valamint a **{yourdevicekey}** helyőrzőé az *Eszközidentitás létrehozása* szakaszban létrehozott eszközkulcsértékre:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Ez a mintaalkalmazás a **protocol** változót használja egy **DeviceClient** objektum példányának létrehozásakor. Az IoT Hubbal folytatott kommunikációhoz MQTT, AMQP vagy HTTPS protokollt használhat.

8. Adja hozzá a következő beágyazott **TelemetryDataPoint** osztályt az **App** osztályon belül az eszköz által az IoT hubnak küldött telemetriai adatok meghatározásához:

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
9. Adja hozzá a következő beágyazott **EventCallback** osztályt az **App** osztályon belül az IoT Hub által az eszközalkalmazástól érkező üzenet feldolgozásakor visszaadott nyugtázási állapot megjelenítéséhez. Ez a metódus az alkalmazás fő szálát is értesíti, amikor üzenet fel lett dolgozva:
   
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

10. Adja hozzá a következő beágyazott **MessageSender** osztályt az **App** osztályon belül. Ezen osztály **run** metódusa minta telemetriai adatokat hoz létre, amelyeket elküld az IoT hubnak, és nyugtázásra vár a következő üzenet elküldése előtt:

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

    Ez a metódus elküld egy új, az eszközről a felhőbe irányuló üzenetet egy másodperccel azután, hogy az IoT hub nyugtázza az előző üzenetet. Az üzenet egy JSON-szerializált objektumot tartalmaz az eszköz azonosítójával és véletlenszerűen előállított számokkal, amelyek egy hőmérséklet- és egy páratartalom-érzékelőt szimulálnak.

11. Cserélje le a **main** metódust a következő kódra, amely létrehoz egy szálat az eszközről a felhőbe irányuló üzenetek elküldéséhez az IoT Hubra:

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

12. Mentse és zárja be az App.java fájlt.

13. Ha a **simulated-device** alkalmazást a Maven használatával szeretné felépíteni, futtassa a következő parancsot a parancssorban a simulated-device mappában:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> Az egyszerűség kedvéért ez az oktatóanyag nem valósít meg semmilyen újrapróbálkozási házirendet. Az éles kódban újrapróbálkozási házirendeket is meg kell valósítania (például egy exponenciális leállítást) a [tranziens hibakezelést][lnk-transient-faults] ismertető MSDN-cikkben leírtak szerint.

## <a name="run-the-apps"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatására.

1. A read-d2c mappában egy parancssorban futtassa a következő parancsot, amellyel megkezdheti az IoT Hub első partíciójának megfigyelését:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub-szolgáltatásalkalmazás az eszközről a felhőbe irányuló üzenetek figyeléséhez][7]

2. A simulated-device mappában egy parancssorban futtassa a következő parancsot, amellyel megkezdheti a telemetriai adatok küldését az IoT Hubnak:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT Hub-eszközalkalmazás az eszközről a felhőbe irányuló üzenetek küldéséhez][8]

3. Az [Azure Portal][lnk-portal] **Használat** csempéje az IoT Hubnak küldött üzenetek számát jeleníti meg:

    ![Az Azure Portalon az IoT Hubnak küldött üzenetek számát megjelenítő Használat csempe][43]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében. Ennek az eszközidentitásnak a segítségével lehetővé tette az eszközalkalmazásnak, hogy az eszközről a felhőbe irányuló üzeneteket küldjön az IoT Hubnak. Emellett létrehozott egy alkalmazást, amely megjeleníti az IoT Hub által fogadott üzeneteket.

További bevezetés az IoT Hub használatába, valamint egyéb IoT-forgatókönyvek megismerése:

* [Kapcsolódás az eszközhöz][lnk-connect-device]
* [Eszközfelügyelet – első lépések][lnk-device-management]
* [Ismerkedés az Azure IoT Edge szolgáltatással][lnk-iot-edge]

Az IoT-megoldás kibővítésével és az eszközről a felhőbe irányuló üzenetek nagy léptékű feldolgozásával kapcsolatban tekintse meg [az eszközről a felhőbe irányuló üzenetek feldolgozását][lnk-process-d2c-tutorial] ismertető oktatóanyagot.
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
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22
