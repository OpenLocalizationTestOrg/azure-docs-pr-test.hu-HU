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
# <a name="connect-your-device-tooyour-iot-hub-using-java"></a>Csatlakozás az eszköz tooyour IoT hub Java használatával
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Ez az oktatóanyag végén hello három Java konzol alkalmazások közül választhat:

* **Hozzon létre eszköz-identitás**, ami létrehoz egy eszközidentitás, és a biztonsági kulcsok tooconnect az eszközalkalmazás.
* **olvasási-d2c-üzenetek**, amely megjeleníti az eszköz alkalmazás által küldött hello telemetriai.
* **Szimulált eszköz**, amely tooyour IoT-központ kapcsolódik a korábban létrehozott hello eszközidentitás, és minden második használatával hello MQTT protokoll telemetriai üzenetet küld.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Utolsó lépésként, jegyezze fel a hello **elsődleges kulcs** érték. Kattintson a **végpontok** és hello **események** beépített végpont. A hello **tulajdonságok** panelen, jegyezze fel a hello **Event Hub-kompatibilis neve** és hello **Event Hub-kompatibilis végpont** cím. Erre a három értékre szüksége lesz a **read-d2c-messages** alkalmazás létrehozásakor.

![Az Azure Portal IoT Hub Üzenetküldés panelje][6]

Ezzel létrehozta az IoT Hubot. Hello IoT-központ állomásnév, IoT-központ kapcsolati karakterláncot, IoT Hub elsődleges kulcs, Event Hub-kompatibilis neve és Event Hub-kompatibilis végpont ebben az oktatóanyagban toocomplete szüksége van.

## <a name="create-a-device-identity"></a>Eszközidentitás létrehozása
Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely az IoT hub a hello identitásjegyzékhez hoz létre egy eszközidentitás. Egy eszköz nem lehet kapcsolódni a tooIoT hub, kivéve, ha azt egy bejegyzéssel rendelkezik hello identitásjegyzékhez. További információkért lásd: hello **Identitásjegyzékhez** hello szakasza [IoT Hub fejlesztői útmutató][lnk-devguide-identity]. A konzol alkalmazás futtatásakor egy eszköz egyedi Azonosítót hoz létre, és az, hogy az eszköz használhatja tooidentify magát, eszköz-felhő küld a kulcs üzenetek tooIoT Hub.

1. Hozzon létre egy iot-java-get-started nevű üres mappát. Hello iot-java-get-started mappában, úgynevezett Maven-projekt létrehozása **létrehozása eszköz-identitás** a következő parancsot a parancssorba hello segítségével. Látható, hogy ez egyetlen hosszú parancs:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorban keresse meg a toohello létrehozása eszköz-identitás mappa.

3. Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello létrehozása eszköz-identitás mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont. A függőség lehetővé teszi, hogy Ön toouse hello iot-szolgáltatás-ügyfélcsomag az alkalmazásban:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési][lnk-maven-service-search].

4. Mentse és zárja be a hello pom.xml fájlt.

5. Egy szövegszerkesztőben nyissa meg hello create-device-identity\src\main\java\com\mycompany\app\App.java fájlt.

6. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Adja hozzá a következő osztály változók toohello hello **App** osztály cseréje **{yourhubconnectionstring}** hello értékre a beállításértékeket korábban:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. Hello hello aláírása módosítása **fő** metódus tooinclude hello kivételek az alábbiak szerint:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. Adja hozzá a kódot hello hello szövege a következő hello **fő** metódust. Ez a kód egy *javadevice* nevű eszközt hoz létre az IoT Hub identitásjegyzékében, ha még nem létezik. Hello Eszközazonosító és a kulcs, amely később kell majd megjelenítése:

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

10. Mentse és zárja be hello App.java fájlt.

11. toobuild hello **létrehozása eszköz-identitás** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello létrehozása eszköz-identitás mappában hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. toorun hello **létrehozása eszköz-identitás** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello létrehozása eszköz-identitás mappában hello:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Jegyezze fel a hello **Eszközazonosító** és **eszközkulcs**. Ezek az értékek később szüksége, amely a központ eszközként tooIoT alkalmazás létrehozásakor.

> [!NOTE]
> az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja. Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, és egy engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol. Ha a kell toostore más eszközre vonatkozó metaadatok, azt kell használni az alkalmazás-specifikus tárolási. További információkért lásd: hello [IoT Hub fejlesztői útmutató][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Az eszközről a felhőbe irányuló üzenetek fogadása

Ebben a szakaszban egy Java-konzolalkalmazást hoz létre, amely az eszközről a felhőbe irányuló üzeneteket olvas az IoT Hubról. Az IoT-központ mutatja egy [Eseményközpont][lnk-event-hubs-overview]-kompatibilis végpont tooenable tooread eszközről a felhőbe üzenetek meg. egyszerű tookeep dolog, ebben az oktatóanyagban egy egyszerű olvasót, amely nem alkalmas a magas teljesítmény központi telepítés hoz létre. Hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] az oktatóanyag bemutatja, hogyan tooprocess eszköz-felhő léptékű üzenetek. Hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] oktatóanyag további információkat biztosítanak a tooprocess Eseményközpontokból származó üzenetek és alkalmazható toohello IoT Hub Event Hub-kompatibilis végpontok.

> [!NOTE]
> hello Event Hub-kompatibilis végpont eszközről a felhőbe üzenetek mindig olvasásához hello AMQP protokollt használja.

1. Hello iot-java-get-started mappában létrehozott hello *hozzon létre egy eszközidentitás* területen nevű Maven-projekt létrehozása **d2c-üzenetek olvasása** a következő parancsot a parancssorba hello segítségével. Látható, hogy ez egyetlen hosszú parancs:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorban keresse meg a toohello olvasás-d2c-üzenetek mappa.

3. Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello olvasás-d2c-üzenetek mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont. A függőség toouse hello eventhubs-ügyfélcsomagját hello Event Hub-kompatibilis végpont az az alkalmazás tooread lehetővé teszi:

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. Mentse és zárja be a hello pom.xml fájlt.

5. Egy szövegszerkesztőben nyissa meg hello read-d2c-messages\src\main\java\com\mycompany\app\App.java fájlt.

6. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. Adja hozzá a következő osztály szintű változó toohello hello **App** osztály. Cserélje le **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, és **{youreventhubcompatiblename}** korábban feljegyzett hello értékekkel:

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Adja hozzá a következő hello **receiveMessages** metódus toohello **App** osztály. Ezzel a módszerrel hoz létre egy **EventHubClient** tooconnect toohello Event Hub-kompatibilis végpont példányt, és aszinkron módon létrehoz egy **PartitionReceiver** példány tooread az Eseményközpontban lévő a partíció. Folyamatosan fut a hurokban, és kiírja a hello üzenet adatai, amíg hello app véget nem ér.

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
   > Ez a módszer egy szűrőt használja, amikor hello fogadó létrehozza, hogy hello fogadó csak olvassa be küldött üzenetek tooIoT Hub után hello fogadó elindul. Ez a módszer akkor hasznos, egy tesztkörnyezetben, így hello aktuális készletében lévő üzenetek. Éles környezetben, a kódot kell győződjön meg arról, hogy feldolgozza az összes köszönőüzenetei - további információt találhat hello [hogyan tooprocess IoT Hub eszköz-a-felhőbe küldött üzeneteket] [ lnk-process-d2c-tutorial] oktatóanyag.

9. Hello hello aláírása módosítása **fő** metódus tooinclude hello kivétel az alábbiak szerint:

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. Adja hozzá a következő kód toohello hello **fő** metódus a hello **App** osztály. Ez a kód létrehoz két hello **EventHubClient** és **PartitionReceiver** példánya, és lehetővé teszi tooclose hello app üzenetek feldolgozása után:

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
    > Ez a kód azt feltételezi, hogy az IoT hub létrehozott hello F1 (ingyenes) csomagot. Az ingyenes IoT hub két, „0” és „1” nevű partícióval rendelkezik.

11. Mentse és zárja be hello App.java fájlt.

12. toobuild hello **d2c-üzenetek olvasása** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello olvasás-d2c-üzenetek mappában hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Eszközalkalmazás létrehozása
Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely szimulálja egy eszköz, amelyet az eszköz a felhőbe küldött üzeneteket tooan IoT-központ küld.

1. Hello iot-java-get-started mappában létrehozott hello *hozzon létre egy eszközidentitás* területen nevű Maven-projekt létrehozása **szimulált eszköz** a következő parancsot a parancssorba hello segítségével. Látható, hogy ez egyetlen hosszú parancs:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorban lépjen a toohello szimulált eszköz mappában.

3. Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello szimulált eszköz mappában, és adja hozzá a következő függőségek toohello hello **függőségek** csomópont. A függőség lehetővé teszi, hogy Ön toouse hello IOT hubbal-java-ügyfélcsomagját a app toocommunicate, az IoT-központ és tooserialize Java objektumok tooJSON:

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
    > Ellenőrizze, hogy hello legújabb verziójának **iot-eszközügyfél** használatával [Maven keresési][lnk-maven-device-search].

4. Mentse és zárja be a hello pom.xml fájlt.

5. Egy szövegszerkesztőben nyissa meg hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.

6. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Adja hozzá a következő osztály változók toohello hello **App** osztály. A mag cseréje **{youriothubname}** az IoT hub nevű és **{yourdevicekey}** hello eszköz kulcsérték hello hozta létre a *hozzon létre egy eszközidentitás* szakasz:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Ezen PéldaAlkalmazás használ hello **protokoll** változó, amikor elindítja a **DeviceClient** objektum. Az IoT-központ vagy hello MQTT, AMQP vagy HTTP protokollt toocommunicate használható.

8. Adja hozzá a beágyazott hello következő **TelemetryDataPoint** osztály belsejében hello **App** toospecify hello telemetriai adatokat az eszköz küld tooyour IoT-központ osztályban:

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
9. Adja hozzá a beágyazott hello következő **EventCallback** osztály belsejében hello **App** osztály toodisplay hello nyugtázási állapotát, amely az IoT-központ hello ad vissza, ha hello eszközalkalmazás üzenetét feldolgozza. Ez a módszer is értesíti hello fő szálnak hello alkalmazásban, üdvözlőüzenetére feldolgozásakor:
   
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

10. Adja hozzá a beágyazott hello következő **MessageSender** osztály belsejében hello **App** osztály. Hello **futtatása** metódus Ez az osztály a minta telemetriai adatok toosend tooyour IoT-központ állít elő, és megvárja-e a nyugtázást hello tovább üzenet küldése előtt:

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

    Ez a módszer új eszközről a felhőbe üzenetet küld egy másodperc után hello IoT-központ elismeri hello előző üzenetét. üdvözlőüzenetére hello deviceId rendelkező JSON-szerializált objektumot tartalmaz, és véletlenszerűen generált számok toosimulate hőmérséklet-érzékelő és a páratartalom érzékelő.

11. Cserélje le a hello **fő** hello kódot, amely létrehoz egy szál toosend eszköz a felhőbe küldött üzeneteket tooyour IoT-központ a következő metódust:

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

12. Mentse és zárja be hello App.java fájlt.

13. toobuild hello **szimulált eszköz** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello szimulált eszköz mappában hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása

Most már áll készen toorun hello alkalmazásokat.

1. Parancssorba hello olvasás-d2c mappában futtassa a következő parancs toobegin hello első partíció az IoT hub a figyelési hello:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT-központ szolgáltatás toomonitor eszközről a felhőbe alkalmazásüzenetek][7]

2. Parancsot egy parancssorba hello szimulált eszköz mappában futtassa a következő parancs toobegin telemetriai adatok tooyour IoT-központ küldése hello:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT Hub eszközre app toosend eszközről a felhőbe küldött üzenetek][8]

3. Hello **használati** hello csempére [Azure-portálon] [ lnk-portal] mutat be hello küldött üzenetek toohello IoT-központ száma:

    ![Az Azure portál használata csempe ábrázoló száma küldött üzenetek tooIoT Hub][43]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Az eszköz identitása tooenable hello eszköz alkalmazás toosend eszköz a felhőbe küldött üzeneteket toohello IoT-központ használta. Is létrehozott alkalmazás hello IoT-központ által fogadott hello üzeneteket jelenít meg.

első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:

* [Kapcsolódás az eszközhöz][lnk-connect-device]
* [Eszközfelügyelet – első lépések][lnk-device-management]
* [Ismerkedés az Azure IoT Edge szolgáltatással][lnk-iot-edge]

toolearn hogyan tooextend az IoT megoldás és a folyamat eszközről a felhőbe üzenetek léptékű: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag.
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
