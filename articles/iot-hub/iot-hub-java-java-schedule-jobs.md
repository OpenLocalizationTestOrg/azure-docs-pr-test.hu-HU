---
title: az Azure IoT Hub (Java) aaaSchedule feladatok |} Microsoft Docs
description: "Az Azure IoT-központ tooschedule hogyan tooinvoke közvetlen módszer feladat, és állítsa be a kívánt tulajdonságot több eszközön. Hello Azure IoT-eszközök SDK használata Java tooimplement hello szimulált eszköz alkalmazások és hello Azure IoT szolgáltatás SDK Java tooimplement a app toorun hello feladata."
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
ms.date: 07/10/2017
ms.author: dobett
ms.openlocfilehash: b1b05fa56c3ce96af0b33d4cca0dd54da0f4e927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-java"></a>Ütemezés és a feladatok (Java)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub tooschedule és nyomon követése feladatok frissítő eszközök millióira használja. Feladatok használata:

* Eszköz kívánt tulajdonságainak frissítése
* Címkék frissítése
* Közvetlen metódusok

Egy feladatot az alábbi műveletek egyikét becsomagolja, és nyomon követi az eszközök végrehajtásának hello. Egy eszköz a két lekérdezést hello feladatot végre eszközök hello csoportját határozza meg. Például egy háttér-alkalmazást, egy feladat tooinvoke közvetlen módszer hello eszköz újraindulása 10 000 eszközökön használható. Adjon meg hello eszközök eszköz iker lekérdezéssel, és egy későbbi időpontban hello feladat toorun ütemezni. hello feladat nyomon követi előrehaladását, mivel minden egyes hello eszközök kapja meg, és hello újraindítás közvetlen metódus hajtható végre.

További információ az egyes képességek, toolearn lásd:

* A két eszköz és a tulajdonságok: [eszköz twins az első lépései](iot-hub-java-java-twin-getstarted.md)
* A közvetlen módszer: [IoT Hub fejlesztői útmutató - közvetlen módszerek](iot-hub-devguide-direct-methods.md) és [oktatóanyag: közvetlen módszerekkel](iot-hub-java-java-direct-methods.md)

Ez az oktatóanyag a következőket mutatja be:

* Hozzon létre egy eszköz-alkalmazást, amely egy közvetlen a hívott metódus **lockDoor**. hello eszközalkalmazás kívánt tulajdonságok módosítását is fogad hello háttér-alkalmazást.
* Hozzon létre egy háttér-alkalmazást, amely létrehoz egy feladatot toocall hello **lockDoor** közvetlen módszer több eszközön. Egy másik feladat küldi el a kívánt tulajdonság toomultiple eszközök frissíti.

Ez az oktatóanyag végén hello hogy egy java Konzolalkalmazás eszköz és a java háttér-Konzolalkalmazás:

**Szimulált eszköz** , hogy csatlakozik az IoT-központ tooyour, hello megvalósítja **lockDoor** közvetlen metódust, és kezeli a szükséges tulajdonságok módosítását.

**a feladatok ütemezése** használó feladatok toocall hello **lockDoor** közvetlen módszer és a frissítés hello eszköz iker szükségeskonfiguráció-tulajdonságok több eszközön.

> [!NOTE]
> hello cikk [Azure IoT SDK-k](iot-hub-devguide-sdks.md) információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.

## <a name="prerequisites"></a>Előfeltételek

toocomplete ebben az oktatóanyagban szüksége:

* hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) néhány perc alatt.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Toocreate hello eszközidentitás programozott módon tetszés szerint olvassa el a megfelelő szakasz hello hello [csatlakozni az eszköz tooyour IoT hub Java használatával](iot-hub-java-java-getstarted.md#create-a-device-identity) cikk. Is használhatja a hello [IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) eszköz tooadd eszköz tooyour IoT-központot.

## <a name="create-hello-service-app"></a>Hello service-alkalmazás létrehozása

Ebben a szakaszban egy feladatok használó Java-Konzolalkalmazás létrehozása:

* Hello hívás **lockDoor** közvetlen módszer több eszközön.
* Elküldeni kívánt tulajdonságokkal toomultiple eszközök.

toocreate hello alkalmazást:

1. A fejlesztői gépen, hozzon létre egy üres nevű `iot-java-schedule-jobs`.

1. A hello `iot-java-schedule-jobs` mappa, úgynevezett Maven-projekt létrehozása **feladatok ütemezése** használatával a következő parancsot a parancssorba hello. Látható, hogy ez egyetlen hosszú parancs:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. A parancssorban lépjen a toohello `schedule-jobs` mappa.

1. Egy szövegszerkesztőben nyissa meg hello `pom.xml` hello fájlban `schedule-jobs` mappa, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont. A függőség lehetővé teszi a toouse hello **iot-szolgáltatásügyfél** az alkalmazás toocommunicate és az IoT hub-csomagot:

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

1. Egy szövegszerkesztőben nyissa meg hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` fájlt.

1. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Pair;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Query;
    import com.microsoft.azure.sdk.iot.service.devicetwin.SqlQuery;
    import com.microsoft.azure.sdk.iot.service.jobs.JobClient;
    import com.microsoft.azure.sdk.iot.service.jobs.JobResult;
    import com.microsoft.azure.sdk.iot.service.jobs.JobStatus;

    import java.util.Date;
    import java.time.Instant;
    import java.util.HashSet;
    import java.util.Set;
    import java.util.UUID;
    ```

1. Adja hozzá a következő osztály változók toohello hello **App** osztály. Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal hello feljegyzett *létrehoz egy IoT-központot* szakasz:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long hello job is permitted toorun without
    // completing its work on hello set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. Adja hozzá a következő metódus toohello hello **App** osztály tooschedule egy feladat tooupdate hello **épület** és **emelet** hello eszköz iker tulajdonság szükséges:

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule hello update twin job toorun now
      // against a single device
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleUpdateTwin(jobId, 
          "deviceId='" + deviceId + "'",
          twin,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling desired properties job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    }
    ```

1. egy feladat toocall hello tooschedule **lockDoor** módszer, adja hozzá a következő metódus toohello hello **App** osztály:

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now toocall hello lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set too5 seconds.
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleDeviceMethod(jobId,
          "deviceId='" + deviceId + "'",
          "lockDoor",
          5L, 5L, null,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling direct method job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    };
    ```

1. toomonitor egy feladatot, adja hozzá a következő metódus toohello hello **App** osztály:

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check hello job result until it's completed
        while(jobResult.getJobStatus() != JobStatus.completed)
        {
          Thread.sleep(100);
          jobResult = jobClient.getJob(jobId);
          System.out.println("Status " + jobResult.getJobStatus() + " for job " + jobId);
        }
        System.out.println("Final status " + jobResult.getJobStatus() + " for job " + jobId);
      } catch (Exception e) {
        System.out.println("Exception monitoring job: " + jobId);
        System.out.println(e.getMessage());
        return;
      }
    }
    ```

1. tooquery hello részletes hello feladatok futtatása, a következő metódus hello hozzáadása:

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using hello time hello jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over hello list of jobs and print hello details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

1. Frissítés hello **fő** metódus aláírása tooinclude hello következő `throws` záradékban:

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. toorun és a figyelő két feladatok egymás után, adja hozzá a következő kód toohello hello **fő** módszert:

    ```java
    // Record hello start time
    String start = Instant.now().toString();

    // Create JobClient
    JobClient jobClient = JobClient.createFromConnectionString(iotHubConnectionString);
    System.out.println("JobClient created with success");

    // Schedule twin job desired properties
    // Maximum concurrent jobs is 1 for Free and S1 tiers
    String desiredPropertiesJobId = "DPCMD" + UUID.randomUUID();
    scheduleJobSetDesiredProperties(jobClient, desiredPropertiesJobId);
    monitorJob(jobClient, desiredPropertiesJobId);

    // Schedule twin job direct method
    String directMethodJobId = "DMCMD" + UUID.randomUUID();
    scheduleJobCallDirectMethod(jobClient, directMethodJobId);
    monitorJob(jobClient, directMethodJobId);

    // Run a query tooshow hello job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

1. Mentse és zárja be a hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` fájl

1. Build hello **feladatok ütemezése** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban lépjen a toohello `schedule-jobs` mappa és a következő parancs futtatása hello:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Eszközalkalmazás létrehozása

Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely kezeli az IoT-központot, és megvalósít hello közvetlen metódushívás által küldött kívánt tulajdonságokkal hello.

1. A hello `iot-java-schedule-jobs` mappa, úgynevezett Maven-projekt létrehozása **szimulált eszköz** használatával a következő parancsot a parancssorba hello. Látható, hogy ez egyetlen hosszú parancs:

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
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Ezen PéldaAlkalmazás használ hello **protokoll** változó, amikor elindítja a **DeviceClient** objektum. Jelenleg toouse eszközök iker szolgáltatásainak hello MQTT protokollt kell használnia.

1. tooprint eszköz iker értesítések toohello konzolról, adja hozzá a következő hello beágyazott osztály toohello **App** osztály:

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
      }
    }
    ```

1. a közvetlen módszer értesítések toohello tooprint konzolról, adja hozzá a következő hello osztály toohello beágyazott **App** osztály:

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodirect method operation with status " + status.name());
      }
    }
    ```

1. toohandle közvetlen metódushívások az IoT-központot, adja hozzá a hello következő beágyazott osztály toohello **App** osztály:

    ```java
    // Handler for direct method calls from IoT Hub
    protected static class DirectMethodCallback
        implements DeviceMethodCallback {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context) {
        DeviceMethodData deviceMethodData;
        switch (methodName) {
          case "lockDoor": {
            System.out.println("Executing direct method: " + methodName);
            deviceMethodData = new DeviceMethodData(METHOD_SUCCESS, "Executed direct method " + methodName);
            break;
          }
          default: {
            deviceMethodData = new DeviceMethodData(METHOD_NOT_DEFINED, "Not defined direct method " + methodName);
          }
        }
        // Notify IoT Hub of result
        return deviceMethodData;
      }
    }
    ```

1. Frissítés hello **fő** metódus aláírása tooinclude hello következő `throws` záradékban:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. Adja hozzá a következő kód toohello hello **fő** módszert:
    * Hozzon létre egy eszköz ügyfél toocommunicate IoT-központot.
    * Hozzon létre egy **eszköz** toostore hello eszköztulajdonságok a két objektum.

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object toomanage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

1. toostart hello eszköz ügyfélszolgáltatások, adja hozzá a következő kód toohello hello **fő** módszert:

    ```java
    try {
      // Open hello DeviceClient
      // Start hello device twin services
      // Subscribe toodirect method calls
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
    } catch (Exception e) {
      System.out.println("Exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

1. a hello felhasználói toopress hello toowait **Enter** leállítása előtt kulcsban, adja hozzá a következő kód toohello vége hello hello **fő** módszert:

    ```java
    // Close hello app
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. Mentse és zárja be a hello `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.

1. Build hello **szimulált eszköz** alkalmazást, és kijavíthatja az esetleges hibákat. A parancssorban lépjen a toohello `simulated-device` mappa és a következő parancs futtatása hello:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Hello alkalmazások futtatása

Most már áll készen toorun hello konzol alkalmazásokat.

1. A parancssorba a hello `simulated-device` mappa, futtassa a következő parancs toostart hello eszköz alkalmazás figyel a kívánt tulajdonságok módosítását és a közvetlen metódushívások hello:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![hello eszközügyfél kezdődik](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. A hello parancsot a parancssorba `schedule-jobs` mappa, futtassa a következő parancs toorun hello hello **feladatok ütemezése** szolgáltatás app toorun két feladatot. hello először állítja be a kívánt hello tulajdonságértékek, második hívás hello hello közvetlen módszer:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás alkalmazás két feladatot hoz létre.](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. hello eszközalkalmazás kezeli kívánt hello tulajdonság módosításának és hello közvetlen metódus hívása:

    ![hello ügyfél válaszol toohello módosítások](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. A háttér-alkalmazások létrehozott toorun két feladatot. hello első feladat beállítani kívánt tulajdonságértékek, és egy közvetlen metódust hívott hello második feladat.

A következő erőforrások toolearn hogyan használja hello számára:

* Telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) oktatóanyag.
* Interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a hello [közvetlen módszerekkel](iot-hub-java-java-direct-methods.md) oktatóanyag.
