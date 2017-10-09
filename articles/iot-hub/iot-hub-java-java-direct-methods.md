---
title: "Azure IoT Hub aaaUse közvetlen módszerek (Java) |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub közvetlen módszerek. A szimulált eszköz alkalmazást, amely magában foglalja a közvetlen módszer és hello Azure IoT szolgáltatás SDK Java tooimplement, amely hello közvetlen metódust hívja service-alkalmazást Java tooimplement hello Azure IoT-eszközök SDK használ."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: b6f2f4a64535ab649a3965cd9c5a19bebaf88eef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-java"></a>Közvetlen módszerekkel (Java)

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Ebben az oktatóanyagban létrehoz két Java-konzol alkalmazásokhoz:

* **Invoke-közvetlen-method**, a Java háttér-alkalmazást, amely metódus meghívja a szimulált eszköz app hello és hello válasz jeleníti meg.
* **Szimulált eszköz**, szimulálja egy eszközt az IoT-központ tooyour összekapcsolása hello eszközidentitás hoz létre egy Java-alkalmazást. Ez az alkalmazás közvetlen meghívása hello háttérből toohello válaszol.

> [!NOTE]
> Használható toobuild alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello SDK-kkal kapcsolatos információk: [Azure IoT SDK-k][lnk-hub-sdks].

toocomplete ebben az oktatóanyagban szüksége:

* Java SE 8. <br/> [A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Java ebben az oktatóanyagban a Windows vagy Linux.
* Maven 3  <br/> [A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall [Maven] [ lnk-maven] ebben az oktatóanyagban a Windows vagy Linux.
* [NODE.js-verzió 0.10.0-s vagy újabb](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása

Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely válaszol a hello megoldás háttérrendszere által meghívott end tooa metódust.

1. Hozzon létre egy üres nevű iot-java-közvetlen-metódus.

1. Hello iot-java-közvetlen-metódus mappában hozzon létre egy Maven project nevű **szimulált eszköz** a következő parancsot a parancssorba hello segítségével. a következő parancs hello egyetlen, hosszú parancs:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorban lépjen a toohello szimulált eszköz mappában.

1. Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello szimulált eszköz mappában, és adja hozzá a következő függőségek toohello hello **függőségek** csomópont. A függőség lehetővé teszi, hogy Ön toouse hello iot-eszközök-ügyfélcsomagját az alkalmazás toocommunicate az IoT hubbal:

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

1. Egy szövegszerkesztőben nyissa meg hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.

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
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Ezen PéldaAlkalmazás használ hello **protokoll** változó, amikor elindítja a **DeviceClient** objektum. Jelenleg toouse közvetlen módszerek hello MQTT protokollt kell használnia.

1. tooreturn állapot kód tooyour IoT hubot, adja hozzá a hello következő beágyazott osztály toohello **App** osztály:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. toohandle hello közvetlen módszer meghívásához hello megoldás háttérből, adja hozzá a hello következő beágyazott osztály toohello **App** osztály:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "writeLine" :
          {
            int status = METHOD_SUCCESS;
            System.out.println(new String((byte[])methodData));
            deviceMethodData = new DeviceMethodData(status, "Executed direct method " + methodName);
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

1. toocreate egy **DeviceClient** és a közvetlen módszer meghívásához figyelésére, vegye fel a **fő** metódus toohello **App** osztály:

    ```java
    public static void main(String[] args)
      throws IOException, URISyntaxException
    {
      System.out.println("Starting device sample...");

      DeviceClient client = new DeviceClient(connString, protocol);

      try
      {
        client.open();
        client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
        System.out.println("Subscribed toodirect methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key tooexit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Mentse és zárja be a hello simulated-device\src\main\java\com\mycompany\app\App.java fájl

1. Build hello **szimulált eszköz** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban lépjen a toohello szimulált eszköz mappa és a következő parancs futtatása hello:

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a>A közvetlen metódus hívása az eszközön

Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely meghívja a közvetlen módszer és hello válasz megjeleníti. A Konzolalkalmazás tooyour IoT-központ tooinvoke hello közvetlen módszer csatlakozik.

1. Hello iot-java-közvetlen-metódus mappában hozzon létre egy Maven project nevű **meghívása közvetlen-metódus** a következő parancsot a parancssorba hello segítségével. a következő parancs hello egyetlen, hosszú parancs:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorba keresse meg a toohello meghívása közvetlen-metódus mappa.

1. Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello meghívása közvetlen-metódus mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont. A függőség lehetővé teszi, hogy Ön toouse hello iot-szolgáltatás-ügyfélcsomagját a app toocommunicate az IoT hubbal:

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

1. Egy szövegszerkesztőben nyissa meg hello invoke-direct-method\src\main\java\com\mycompany\app\App.java fájlt.

1. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Adja hozzá a következő osztály változók toohello hello **App** osztály. Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal hello feljegyzett *létrehoz egy IoT-központot* szakasz:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line toobe written";
    ```

1. tooinvoke hello metódus hello szimulált eszköz, adja hozzá a következő kód toohello hello **fő** módszert:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
        System.out.println("Invoke direct method");
        MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

        if(result == null)
        {
            throw new IOException("Direct method invoke returns null");
        }
        System.out.println("Status=" + result.getStatus());
        System.out.println("Payload=" + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }

    System.out.println("Shutting down sample...");
    ```

1. Mentse és zárja be a hello invoke-direct-method\src\main\java\com\mycompany\app\App.java fájl

1. Build hello **meghívása közvetlen-metódus** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban keresse meg a toohello meghívása közvetlen-metódus mappa és a következő parancs futtatása hello:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása

Most már áll készen toorun hello konzol alkalmazásokat.

1. Parancsot egy parancssorba hello szimulált eszköz mappában futtassa a következő parancs toobegin metódushívások az IoT-központ figyel hello:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT Hub szimulált eszköz alkalmazás toolisten közvetlen metódushívások][8]

1. A parancssorba hello meghívása közvetlen-metódus mappában futtassa a következő parancs toocall hello metódus a szimulált eszköz az IoT hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás app toocall közvetlen módszer][7]

1. hello szimulált eszköz válaszol toohello közvetlen metódus hívása:

    ![Java IoT Hub szimulált eszköz alkalmazásának válaszol toohello közvetlen metódus hívása][9]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Az eszköz identitás tooenable hello szimulált eszköz alkalmazás tooreact toomethods hello felhő által meghívott használta. Is létrehozott egy alkalmazást, amely hello eszközön módszereket hív, és megjeleníti hello válasz hello eszközről.

tooexplore más IoT-forgatókönyvek esetén, lásd: [több eszközön feladatok ütemezése][lnk-devguide-jobs].

toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-direct-methods/invoke-method.png
[8]: ./media/iot-hub-java-java-direct-methods/device-listen.png
[9]: ./media/iot-hub-java-java-direct-methods/device-respond.png

<!-- Links -->
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md
[lnk-maven]: https://maven.apache.org/what-is-maven.html
[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
