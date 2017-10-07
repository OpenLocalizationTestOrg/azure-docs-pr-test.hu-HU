---
title: "aaaGet Azure IoT Hub kezelés (Java) használatába |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub eszköz felügyeleti tooinitiate egy távoli eszköz újraindul. A szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és hello Azure IoT szolgáltatás SDK Java tooimplement, amely hello közvetlen metódust hívja service-alkalmazást Java tooimplement hello Azure IoT-eszközök SDK használ."
services: iot-hub
documentationcenter: .java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 7aaeda9d4ff7002e5c66adfd61e2dfd5bcea964f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-java"></a>Eszközkezelés (Java) az első lépései

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Ez az oktatóanyag a következőket mutatja be:

* Az Azure portál toocreate az IoT-központ hello használata, és az IoT hub hozzon létre egy eszközidentitás.
* Létrehoz egy szimulált eszköz alkalmazást, amely megvalósítja a közvetlen módszer tooreboot hello eszköz. Közvetlen módszerek hello felhőből hívják.
* Hozzon létre egy alkalmazást, amely hívja meg hello újraindítás közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban. Ez app majd figyelők hello hello eszköz toosee jelentett tulajdonságait, ha hello újraindítás művelete befejeződött.

Ez az oktatóanyag végén hello két Java konzol alkalmazások közül választhat:

**Szimulált eszköz**. Ezt az alkalmazást:

* A korábban létrehozott hello eszközidentitás tooyour IoT-központ kapcsolódik.
* Újraindítás közvetlen módszer hívást kap.
* A fizikai számítógép újraindítása szimulálja.
* Jelentések hello idő az utolsó újraindítás hello jelentett tulajdonságon keresztül.

**eseményindító-újraindítás**. Ezt az alkalmazást:

* Meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban.
* Hello válasz toohello közvetlen metódus hívása hello szimulált eszköz által küldött jeleníti meg
* Frissített megjeleníti hello jelentett tulajdonságok.

> [!NOTE]
> Használható toobuild alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello SDK-kkal kapcsolatos információk: [Azure IoT SDK-k][lnk-hub-sdks].

toocomplete ebben az oktatóanyagban szüksége:

* Java SE 8. <br/> [A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Java ebben az oktatóanyagban a Windows vagy Linux.
* Maven 3  <br/> [A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall [Maven] [ lnk-maven] ebben az oktatóanyagban a Windows vagy Linux.
* [NODE.js-verzió 0.10.0-s vagy újabb](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a>Eseményindító közvetlen metódussal hello eszközön távoli újraindítás

Ebben a szakaszban hoz létre egy Java-Konzolalkalmazás, amely:

1. Meghívja a hello újraindítás közvetlen módszer hello szimulált eszköz alkalmazásban.
1. Hello válasz jeleníti meg.
1. Szavazások hello tulajdonságok hello eszköz toodetermine küldött hello újraindítás befejezésekor jelentett.

A Konzolalkalmazás csatlakozik az IoT-központ tooyour tooinvoke hello közvetlen módszer és olvasási hello jelentett tulajdonságait.

1. Hozzon létre egy üres nevű dm-get-started.

1. A dm-get-started hello mappában nevű Maven-projekt létrehozása **eseményindító-újraindítás** a következő parancsot a parancssorba hello segítségével. hello következő egyetlen, hosszú parancs jeleníti meg:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorban keresse meg a toohello eseményindító-újraindítás mappa.

1. Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello eseményindító-újraindítás mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont. A függőség lehetővé teszi, hogy Ön toouse hello iot-szolgáltatás-ügyfélcsomagját a app toocommunicate az IoT hubbal:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési][lnk-maven-service-search].

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

1. Mentse és zárja be a hello pom.xml fájlt.

1. Egy szövegszerkesztőben nyissa meg hello trigger-reboot\src\main\java\com\mycompany\app\App.java forrásfájl.

1. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

1. Adja hozzá a következő osztály változók toohello hello **App** osztály. Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal hello feljegyzett *létrehoz egy IoT-központot* szakasz:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. a szál hello olvasó tooimplement jelentett tulajdonságok hello eszköz kettős 10 másodpercenként hozzá hello következő beágyazott osztály toohello **App** osztály:

    ```java
    private static class ShowReportedProperties implements Runnable {
      public void run() {
        try {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          while (true) {
            System.out.println("Get reported properties from device twin");
            deviceTwins.getTwin(twinDevice);
            System.out.println(twinDevice.reportedPropertiesToString());
            Thread.sleep(10000);
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. tooinvoke hello újraindítás közvetlen módszer hello szimulált eszköz, adja hozzá a következő kód toohello hello **fő** módszert:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      System.out.println("Invoke reboot direct method");
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, null);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }
      System.out.println("Invoked reboot on device");
      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }
    ```

1. toostart hello szál toopoll hello hello szimulált eszköz jelentett tulajdonságait, adja hozzá a következő kód toohello hello **fő** módszert:

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. tooenable toostop hello alkalmazást, adja hozzá a következő kód toohello hello **fő** módszert:

    ```java
    System.out.println("Press ENTER tooexit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. Mentse és zárja be hello trigger-reboot\src\main\java\com\mycompany\app\App.java fájlt.

1. Build hello **eseményindító-újraindítás** háttér-alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban lépjen a toohello eseményindító-újraindítás mappa és a következő parancs futtatása hello:

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása

Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely az eszköz szimulálja. hello app hello újraindítás közvetlen metódus hívását az IoT hub figyeli, és azonnal válaszol toothat hívás. alkalmazást, majd egy ideig alszik hello toosimulate hello újraindítás folyamat egy jelentett tulajdonság toonotify hello használata előtt **eseményindító-újraindítás** háttér-alkalmazást, amely hello újraindítás befejeződött.

1. A dm-get-started hello mappában nevű Maven-projekt létrehozása **szimulált eszköz** a következő parancsot a parancssorba hello segítségével. hello az alábbiakban olvashat egy egyetlen, hosszú parancsot:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorban lépjen a toohello szimulált eszköz mappában.

1. Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello szimulált eszköz mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont. A függőség lehetővé teszi, hogy Ön toouse hello iot-szolgáltatás-ügyfélcsomagját a app toocommunicate az IoT hubbal:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Ellenőrizze, hogy hello legújabb verziójának **iot-eszközügyfél** használatával [Maven keresési][lnk-maven-device-search].

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

1. Mentse és zárja be a hello pom.xml fájlt.

1. Egy szövegszerkesztőben nyissa meg hello simulated-device\src\main\java\com\mycompany\app\App.java forrásfájl.

1. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    ```

1. Adja hozzá a következő osztály változók toohello hello **App** osztály. Cserélje le `{yourdeviceconnectionstring}` hello eszköz kapcsolati karakterlánccal hello feljegyzett *hozzon létre egy eszközidentitás* szakasz:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. egy visszahívás-kezelő, a közvetlen módszer állapoteseményeit, tooimplement adja hozzá a hello következő beágyazott osztály toohello **App** osztály:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. egy visszahívás-kezelő eszköz iker állapoteseményeit, a tooimplement adja hozzá a hello következő beágyazott osztály toohello **App** osztály:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
        }
    }
    ```

1. egy visszahívás-kezelő tulajdonság események tooimplement adja hozzá a hello következő beágyazott osztály toohello **App** osztály:

    ```java
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

1. tooimplement szál toosimulate hello eszköz újraindítás, adja hozzá a hello következő beágyazott osztály toohello **App** osztály. hello szál öt másodpercenként alvó állapotba kerül, és ezután beállítja az hello **lastReboot** tulajdonság jelentette:

    ```java
    protected static class RebootDeviceThread implements Runnable {
      public void run() {
        try {
          System.out.println("Rebooting...");
          Thread.sleep(5000);
          Property property = new Property("lastReboot", LocalDateTime.now());
          Set<Property> properties = new HashSet<Property>();
          properties.add(property);
          client.sendReportedProperties(properties);
          System.out.println("Rebooted");
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

1. tooimplement hello közvetlen módszer hello eszközön, adja hozzá a hello következő beágyazott osztály toohello **App** osztály. Ha hello szimulált app kap egy hívás toohello **újraindítás** közvetlen módszer adja vissza egy nyugtázási toohello hívó és majd elindul egy szál tooprocess hello újraindítás:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "reboot" :
          {
            int status = METHOD_SUCCESS;
            System.out.println("Received reboot request");
            deviceMethodData = new DeviceMethodData(status, "Started reboot");
            RebootDeviceThread rebootThread = new RebootDeviceThread();
            Thread t = new Thread(rebootThread);
            t.start();
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. Hello hello aláírása módosítása **fő** metódus toothrow hello kivételek a következő:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. tooinstantiate egy **DeviceClient**, adja hozzá a következő kód toohello hello **fő** módszert:

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. figyeli a közvetlen metódushívások esetén toostart adja hozzá a következő kód toohello hello **fő** módszert:

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed toodirect methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. tooshut le hello eszköz szimulátor, adja hozzá a következő kód toohello hello **fő** módszert:

    ```java
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. Mentse és zárja be hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.

1. Build hello **szimulált eszköz** háttér-alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban lépjen a toohello szimulált eszköz mappa és a következő parancs futtatása hello:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása

Most már áll készen toorun hello alkalmazásokat.

1. Parancsot egy parancssorba hello szimulált eszköz mappában futtassa a következő parancs toobegin újraindítás metódushívások az IoT-központ figyel hello:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT Hub szimulált eszköz alkalmazás toolisten újraindítás közvetlen metódushívások][1]

1. Parancsot egy parancssorba hello eseményindító-újraindítás mappában futtassa a szimulált eszköz parancs toocall hello újraindítás metódus követően az IoT hub hello:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás app toocall hello indítsa újra a közvetlen módszer][2]

1. hello szimulált eszköz válaszol toohello újraindítás közvetlen metódus hívása:

    ![Java IoT Hub szimulált eszköz alkalmazásának válaszol toohello közvetlen metódus hívása][3]

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[1]: ./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png
[2]: ./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png
[3]: ./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png
<!-- Links -->

[lnk-maven]: https://maven.apache.org/what-is-maven.html

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22