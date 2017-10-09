---
title: "aaaGet Azure IoT Hub eszköz twins (Java) használatába |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz twins tooadd címkéket, majd az IoT Hub-lekérdezést. Hello Azure IoT-eszközök SDK használata Java tooimplement hello eszközalkalmazás és hello Azure IoT szolgáltatás SDK Java tooimplement egy szolgáltatás-alkalmazást, amely hello címkék hozzáadása, és az IoT-központ lekérdezés hello futtatja."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: dobett
ms.openlocfilehash: 25f6fc81471d59c62bcdc3766bb5c33f5733c930
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-java"></a>Ismerkedés az eszköz twins (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Ebben az oktatóanyagban létrehoz két Java-konzol alkalmazásokhoz:

* **Adja hozzá címkék-lekérdezés**, a Java háttér-alkalmazást, amely címkét ad hozzá, és lekérdezi az eszköz twins.
* **Szimulált eszköz**, a Java eszközalkalmazás, hogy, hogy kapcsolatot tooyour IoT hub és a jelentések jelentett tulajdonsággal kapcsolat állapotát.

> [!NOTE]
> hello cikk [Azure IoT SDK-k](iot-hub-devguide-sdks.md) információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.

toocomplete ebben az oktatóanyagban szüksége:

* hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Toocreate hello eszközidentitás programozott módon tetszés szerint olvassa el a megfelelő szakasz hello hello [csatlakozni az eszköz tooyour IoT hub Java használatával](iot-hub-java-java-getstarted.md#create-a-device-identity) cikk.

## <a name="create-hello-service-app"></a>Hello service-alkalmazás létrehozása

Ebben a szakaszban hoz létre egy Java-alkalmazást, amely hozzáadja a hely metaadatokat egy címke toohello eszköz iker az IoT-központ kapcsolódó **myDeviceId**. hello app először lekérdezi az IoT-központ hello USA található eszközök, majd jelentést mobilhálózati kapcsolat eszközök esetében.

1. A fejlesztői gépen, hozzon létre egy üres nevű `iot-java-twin-getstarted`.

1. A hello `iot-java-twin-getstarted` mappa, nevű Maven-projekt létrehozása **hozzáadása-címkék-lekérdezés** a következő parancsot a parancssorba hello segítségével. Látható, hogy ez egyetlen hosszú parancs:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorban lépjen a toohello `add-tags-query` mappa.

1. Egy szövegszerkesztőben nyissa meg hello `pom.xml` hello fájlban `add-tags-query` mappa, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont. A függőség lehetővé teszi a toouse hello **iot-szolgáltatásügyfél** az alkalmazás toocommunicate és az IoT hub-csomagot:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont. Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Mentse és zárja be a hello `pom.xml` fájlt.

1. Egy szövegszerkesztőben nyissa meg hello `add-tags-query\src\main\java\com\mycompany\app\App.java` fájlt.

1. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Adja hozzá a következő osztály változók toohello hello **App** osztály. Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal hello feljegyzett *létrehoz egy IoT-központot* szakasz:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. Frissítés hello **fő** metódus aláírása tooinclude hello következő `throws` záradékban:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Adja hozzá a következő kód toohello hello **fő** metódus toocreate hello **DeviceTwin** és **DeviceTwinDevice** objektumok. Hello **DeviceTwin** objektum az IoT hub hello kommunikációt kezeli. Hello **DeviceTwinDevice** objektum által jelképezett hello eszköz iker tulajdonságok és címkék:

    ```java
    // Get hello DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. Adja hozzá a következő hello `try/catch` toohello blokkolása **fő** módszert:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. tooupdate hello **régió** és **gépek** az eszköz iker eszköz iker címkék hozzáadása a következő kódot a hello hello `try` letiltása:

    ```java
    // Get hello device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from hello existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create hello tags and attach them toohello DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update hello device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve hello device twin with hello tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

1. tooquery hello eszköz twins az IoT-központot, adja hozzá a következő kód toohello hello `try` blokk hello előző lépésben felvett hello kód után. hello kódjának futtatásához két lekérdezést. Minden egyes lekérdezés visszaadja a legfeljebb 100 eszközöket:

    ```java
    // Query hello device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct hello query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run hello query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct hello query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run hello query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

1. Mentse és zárja be a hello `add-tags-query\src\main\java\com\mycompany\app\App.java` fájl

1. Build hello **hozzáadása-címkék-lekérdezés** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban lépjen a toohello `add-tags-query` mappa és a következő parancs futtatása hello:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Eszközalkalmazás létrehozása

Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely beállítja a központ tooIoT küldött jelentett tulajdonság értéke.

1. A hello `iot-java-twin-getstarted` mappa, úgynevezett Maven-projekt létrehozása **szimulált eszköz** használatával a következő parancsot a parancssorba hello. Látható, hogy ez egyetlen hosszú parancs:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorban lépjen a toohello `simulated-device` mappa.

1. Egy szövegszerkesztőben nyissa meg hello `pom.xml` hello fájlban `simulated-device` mappa, és adja hozzá a következő függőségek toohello hello **függőségek** csomópont. A függőség lehetővé teszi a toouse hello **iot-eszközügyfél** az alkalmazás toocommunicate és az IoT hub-csomagot:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Ellenőrizze, hogy hello legújabb verziójának **iot-eszközügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont. Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Mentse és zárja be a hello `pom.xml` fájlt.

1. Egy szövegszerkesztőben nyissa meg hello `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.

1. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Adja hozzá a következő osztály változók toohello hello **App** osztály. A mag cseréje `{youriothubname}` az IoT hub nevű és `{yourdevicekey}` hello eszköz kulcsérték hello hozta létre a *hozzon létre egy eszközidentitás* szakasz:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    Ezen PéldaAlkalmazás használ hello **protokoll** változó, amikor elindítja a **DeviceClient** objektum. Jelenleg toouse eszközök iker szolgáltatásainak hello MQTT protokollt kell használnia.

1. Adja hozzá a következő kód toohello hello **fő** módszert:
    * Hozzon létre egy eszköz ügyfél toocommunicate IoT-központot.
    * Hozzon létre egy **eszköz** toostore hello eszköztulajdonságok a két objektum.

    ```java
    DeviceClient client = new DeviceClient(connString, protocol);

    // Create a Device object toostore hello device twin properties
    Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed too" + propertyValue);
      }
    };
    ```

1. Adja hozzá a következő kód toohello hello **fő** metódus toocreate egy **connectivityType** tulajdonság jelentette, és elküldi a tooIoT Hub:

    ```java
    try {
      // Open hello DeviceClient and start hello device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it tooyour IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Adja hozzá a következő kód toohello vége hello hello **fő** metódust. Várakozás a hello **Enter** kulcs lehetővé teszi, hogy az IoT-központ tooreport hello hello eszköz iker műveletek állapotának ideje:

    ```java
    System.out.println("Press any key tooexit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Mentse és zárja be a hello `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.

1. Build hello **szimulált eszköz** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban lépjen a toohello `simulated-device` mappa és a következő parancs futtatása hello:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása

Most már áll készen toorun hello konzol alkalmazásokat.

1. A hello parancsot a parancssorba `add-tags-query` mappa, futtassa a következő parancs toorun hello hello **hozzáadása-címkék-lekérdezés** service-alkalmazást:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás app tooupdate értékek címkét, és eszköz-lekérdezések futtatása](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Megtekintheti a hello **gépek** és **régió** címkék hozzáadott toohello eszköz iker. hello első lekérdezés visszaadja az eszközt, de hello második viszont nem.

1. A hello parancsot a parancssorba `simulated-device` mappa, futtassa a következő parancs tooadd hello hello **connectivityType** tulajdonság toohello eszköz iker jelentette:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![hello eszköz client ad hello ** connectivityType ** jelentett tulajdonság](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. A hello parancsot a parancssorba `add-tags-query` mappa, futtassa a következő parancs toorun hello hello **hozzáadása-címkék-lekérdezés** service-alkalmazást még egyszer:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás app tooupdate értékek címkét, és eszköz-lekérdezések futtatása](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Miután az eszköz küldött hello **connectivityType** tulajdonság tooIoT Hub, hello második lekérdezés visszaadja az eszközt.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Eszköz metaadatait címkeként felvett egy háttér-alkalmazást, és egy eszköz kapcsolat app tooreport eszközadatokat megírt hello eszköz iker a. Is megtanulta, hogyan tooquery hello hello SQL-szerű IoT Hub lekérdezési nyelv használatával kettős eszközadatokat.

A következő erőforrások toolearn hogyan használja hello számára:

* Telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) oktatóanyag.
* Interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a hello [közvetlen módszerekkel](iot-hub-java-java-direct-methods.md) oktatóanyag.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
