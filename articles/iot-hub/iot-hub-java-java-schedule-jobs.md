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
# <a name="schedule-and-broadcast-jobs-java"></a><span data-ttu-id="ea3ca-104">Ütemezés és a feladatok (Java)</span><span class="sxs-lookup"><span data-stu-id="ea3ca-104">Schedule and broadcast jobs (Java)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="ea3ca-105">Azure IoT Hub tooschedule és nyomon követése feladatok frissítő eszközök millióira használja.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-105">Use Azure IoT Hub tooschedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="ea3ca-106">Feladatok használata:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-106">Use jobs to:</span></span>

* <span data-ttu-id="ea3ca-107">Eszköz kívánt tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="ea3ca-107">Update desired properties</span></span>
* <span data-ttu-id="ea3ca-108">Címkék frissítése</span><span class="sxs-lookup"><span data-stu-id="ea3ca-108">Update tags</span></span>
* <span data-ttu-id="ea3ca-109">Közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="ea3ca-109">Invoke direct methods</span></span>

<span data-ttu-id="ea3ca-110">Egy feladatot az alábbi műveletek egyikét becsomagolja, és nyomon követi az eszközök végrehajtásának hello.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-110">A job wraps one of these actions and tracks hello execution against a set of devices.</span></span> <span data-ttu-id="ea3ca-111">Egy eszköz a két lekérdezést hello feladatot végre eszközök hello csoportját határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-111">A device twin query defines hello set of devices hello job executes against.</span></span> <span data-ttu-id="ea3ca-112">Például egy háttér-alkalmazást, egy feladat tooinvoke közvetlen módszer hello eszköz újraindulása 10 000 eszközökön használható.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-112">For example, a back-end app can use a job tooinvoke a direct method on 10,000 devices that reboots hello devices.</span></span> <span data-ttu-id="ea3ca-113">Adjon meg hello eszközök eszköz iker lekérdezéssel, és egy későbbi időpontban hello feladat toorun ütemezni.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-113">You specify hello set of devices with a device twin query and schedule hello job toorun at a future time.</span></span> <span data-ttu-id="ea3ca-114">hello feladat nyomon követi előrehaladását, mivel minden egyes hello eszközök kapja meg, és hello újraindítás közvetlen metódus hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-114">hello job tracks progress as each of hello devices receive and execute hello reboot direct method.</span></span>

<span data-ttu-id="ea3ca-115">További információ az egyes képességek, toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-115">toolearn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="ea3ca-116">A két eszköz és a tulajdonságok: [eszköz twins az első lépései](iot-hub-java-java-twin-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="ea3ca-116">Device twin and properties: [Get started with device twins](iot-hub-java-java-twin-getstarted.md)</span></span>
* <span data-ttu-id="ea3ca-117">A közvetlen módszer: [IoT Hub fejlesztői útmutató - közvetlen módszerek](iot-hub-devguide-direct-methods.md) és [oktatóanyag: közvetlen módszerekkel](iot-hub-java-java-direct-methods.md)</span><span class="sxs-lookup"><span data-stu-id="ea3ca-117">Direct methods: [IoT Hub developer guide - direct methods](iot-hub-devguide-direct-methods.md) and [Tutorial: Use direct methods](iot-hub-java-java-direct-methods.md)</span></span>

<span data-ttu-id="ea3ca-118">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-118">This tutorial shows you how to:</span></span>

* <span data-ttu-id="ea3ca-119">Hozzon létre egy eszköz-alkalmazást, amely egy közvetlen a hívott metódus **lockDoor**.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-119">Create a device app that implements a direct method called **lockDoor**.</span></span> <span data-ttu-id="ea3ca-120">hello eszközalkalmazás kívánt tulajdonságok módosítását is fogad hello háttér-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-120">hello device app also receives desired property changes from hello back-end app.</span></span>
* <span data-ttu-id="ea3ca-121">Hozzon létre egy háttér-alkalmazást, amely létrehoz egy feladatot toocall hello **lockDoor** közvetlen módszer több eszközön.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-121">Create a back-end app that creates a job toocall hello **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="ea3ca-122">Egy másik feladat küldi el a kívánt tulajdonság toomultiple eszközök frissíti.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-122">Another job sends desired property updates toomultiple devices.</span></span>

<span data-ttu-id="ea3ca-123">Ez az oktatóanyag végén hello hogy egy java Konzolalkalmazás eszköz és a java háttér-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-123">At hello end of this tutorial, you have a java console device app and a java console back-end app:</span></span>

<span data-ttu-id="ea3ca-124">**Szimulált eszköz** , hogy csatlakozik az IoT-központ tooyour, hello megvalósítja **lockDoor** közvetlen metódust, és kezeli a szükséges tulajdonságok módosítását.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-124">**simulated-device** that connects tooyour IoT hub, implements hello **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="ea3ca-125">**a feladatok ütemezése** használó feladatok toocall hello **lockDoor** közvetlen módszer és a frissítés hello eszköz iker szükségeskonfiguráció-tulajdonságok több eszközön.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-125">**schedule-jobs** that use jobs toocall hello **lockDoor** direct method and update hello device twin desired properties on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="ea3ca-126">hello cikk [Azure IoT SDK-k](iot-hub-devguide-sdks.md) információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-126">hello article [Azure IoT SDKs](iot-hub-devguide-sdks.md) provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea3ca-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ea3ca-127">Prerequisites</span></span>

<span data-ttu-id="ea3ca-128">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-128">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="ea3ca-129">hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="ea3ca-129">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="ea3ca-130">Maven 3</span><span class="sxs-lookup"><span data-stu-id="ea3ca-130">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="ea3ca-131">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-131">An active Azure account.</span></span> <span data-ttu-id="ea3ca-132">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="ea3ca-132">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="ea3ca-133">Toocreate hello eszközidentitás programozott módon tetszés szerint olvassa el a megfelelő szakasz hello hello [csatlakozni az eszköz tooyour IoT hub Java használatával](iot-hub-java-java-getstarted.md#create-a-device-identity) cikk.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-133">If you prefer toocreate hello device identity programmatically, read hello corresponding section in hello [Connect your device tooyour IoT hub using Java](iot-hub-java-java-getstarted.md#create-a-device-identity) article.</span></span> <span data-ttu-id="ea3ca-134">Is használhatja a hello [IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) eszköz tooadd eszköz tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-134">You can also use hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tool tooadd a device tooyour IoT hub.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="ea3ca-135">Hello service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea3ca-135">Create hello service app</span></span>

<span data-ttu-id="ea3ca-136">Ebben a szakaszban egy feladatok használó Java-Konzolalkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-136">In this section, you create a Java console app that uses jobs to:</span></span>

* <span data-ttu-id="ea3ca-137">Hello hívás **lockDoor** közvetlen módszer több eszközön.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-137">Call hello **lockDoor** direct method on multiple devices.</span></span>
* <span data-ttu-id="ea3ca-138">Elküldeni kívánt tulajdonságokkal toomultiple eszközök.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-138">Send desired properties toomultiple devices.</span></span>

<span data-ttu-id="ea3ca-139">toocreate hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-139">toocreate hello app:</span></span>

1. <span data-ttu-id="ea3ca-140">A fejlesztői gépen, hozzon létre egy üres nevű `iot-java-schedule-jobs`.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-140">On your development machine, create an empty folder called `iot-java-schedule-jobs`.</span></span>

1. <span data-ttu-id="ea3ca-141">A hello `iot-java-schedule-jobs` mappa, úgynevezett Maven-projekt létrehozása **feladatok ütemezése** használatával a következő parancsot a parancssorba hello.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-141">In hello `iot-java-schedule-jobs` folder, create a Maven project called **schedule-jobs** using hello following command at your command prompt.</span></span> <span data-ttu-id="ea3ca-142">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-142">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="ea3ca-143">A parancssorban lépjen a toohello `schedule-jobs` mappa.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-143">At your command prompt, navigate toohello `schedule-jobs` folder.</span></span>

1. <span data-ttu-id="ea3ca-144">Egy szövegszerkesztőben nyissa meg hello `pom.xml` hello fájlban `schedule-jobs` mappa, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-144">Using a text editor, open hello `pom.xml` file in hello `schedule-jobs` folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="ea3ca-145">A függőség lehetővé teszi a toouse hello **iot-szolgáltatásügyfél** az alkalmazás toocommunicate és az IoT hub-csomagot:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-145">This dependency enables you toouse hello **iot-service-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="ea3ca-146">Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="ea3ca-146">You can check for hello latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="ea3ca-147">Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-147">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="ea3ca-148">Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-148">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="ea3ca-149">Mentse és zárja be a hello `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-149">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="ea3ca-150">Egy szövegszerkesztőben nyissa meg hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-150">Using a text editor, open hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="ea3ca-151">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-151">Add hello following **import** statements toohello file:</span></span>

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

1. <span data-ttu-id="ea3ca-152">Adja hozzá a következő osztály változók toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-152">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="ea3ca-153">Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal hello feljegyzett *létrehoz egy IoT-központot* szakasz:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-153">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long hello job is permitted toorun without
    // completing its work on hello set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. <span data-ttu-id="ea3ca-154">Adja hozzá a következő metódus toohello hello **App** osztály tooschedule egy feladat tooupdate hello **épület** és **emelet** hello eszköz iker tulajdonság szükséges:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-154">Add hello following method toohello **App** class tooschedule a job tooupdate hello **Building** and **Floor** desired properties in hello device twin:</span></span>

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

1. <span data-ttu-id="ea3ca-155">egy feladat toocall hello tooschedule **lockDoor** módszer, adja hozzá a következő metódus toohello hello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-155">tooschedule a job toocall hello **lockDoor** method, add hello following method toohello **App** class:</span></span>

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

1. <span data-ttu-id="ea3ca-156">toomonitor egy feladatot, adja hozzá a következő metódus toohello hello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-156">toomonitor a job, add hello following method toohello **App** class:</span></span>

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

1. <span data-ttu-id="ea3ca-157">tooquery hello részletes hello feladatok futtatása, a következő metódus hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-157">tooquery for hello details of hello jobs you ran, add hello following method:</span></span>

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

1. <span data-ttu-id="ea3ca-158">Frissítés hello **fő** metódus aláírása tooinclude hello következő `throws` záradékban:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-158">Update hello **main** method signature tooinclude hello following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. <span data-ttu-id="ea3ca-159">toorun és a figyelő két feladatok egymás után, adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-159">toorun and monitor two jobs sequentially, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="ea3ca-160">Mentse és zárja be a hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` fájl</span><span class="sxs-lookup"><span data-stu-id="ea3ca-160">Save and close hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` file</span></span>

1. <span data-ttu-id="ea3ca-161">Build hello **feladatok ütemezése** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-161">Build hello **schedule-jobs** app and correct any errors.</span></span> <span data-ttu-id="ea3ca-162">A parancssorban lépjen a toohello `schedule-jobs` mappa és a következő parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-162">At your command prompt, navigate toohello `schedule-jobs` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a><span data-ttu-id="ea3ca-163">Eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea3ca-163">Create a device app</span></span>

<span data-ttu-id="ea3ca-164">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely kezeli az IoT-központot, és megvalósít hello közvetlen metódushívás által küldött kívánt tulajdonságokkal hello.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-164">In this section, you create a Java console app that handles hello desired properties sent from IoT Hub and implements hello direct method call.</span></span>

1. <span data-ttu-id="ea3ca-165">A hello `iot-java-schedule-jobs` mappa, úgynevezett Maven-projekt létrehozása **szimulált eszköz** használatával a következő parancsot a parancssorba hello.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-165">In hello `iot-java-schedule-jobs` folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="ea3ca-166">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-166">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="ea3ca-167">A parancssorban lépjen a toohello `simulated-device` mappa.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-167">At your command prompt, navigate toohello `simulated-device` folder.</span></span>

1. <span data-ttu-id="ea3ca-168">Egy szövegszerkesztőben nyissa meg hello `pom.xml` hello fájlban `simulated-device` mappa, és adja hozzá a következő függőségek toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-168">Using a text editor, open hello `pom.xml` file in hello `simulated-device` folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="ea3ca-169">A függőség lehetővé teszi a toouse hello **iot-eszközügyfél** az alkalmazás toocommunicate és az IoT hub-csomagot:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-169">This dependency enables you toouse hello **iot-device-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="ea3ca-170">Ellenőrizze, hogy hello legújabb verziójának **iot-eszközügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="ea3ca-170">You can check for hello latest version of **iot-device-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="ea3ca-171">Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-171">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="ea3ca-172">Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-172">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="ea3ca-173">Mentse és zárja be a hello `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-173">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="ea3ca-174">Egy szövegszerkesztőben nyissa meg hello `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-174">Using a text editor, open hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="ea3ca-175">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-175">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="ea3ca-176">Adja hozzá a következő osztály változók toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-176">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="ea3ca-177">A mag cseréje `{youriothubname}` az IoT hub nevű és `{yourdevicekey}` hello eszköz kulcsérték hello hozta létre a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-177">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="ea3ca-178">Ezen PéldaAlkalmazás használ hello **protokoll** változó, amikor elindítja a **DeviceClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-178">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="ea3ca-179">Jelenleg toouse eszközök iker szolgáltatásainak hello MQTT protokollt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-179">Currently, toouse device twin features you must use hello MQTT protocol.</span></span>

1. <span data-ttu-id="ea3ca-180">tooprint eszköz iker értesítések toohello konzolról, adja hozzá a következő hello beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-180">tooprint device twin notifications toohello console, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="ea3ca-181">a közvetlen módszer értesítések toohello tooprint konzolról, adja hozzá a következő hello osztály toohello beágyazott **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-181">tooprint direct method notifications toohello console, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodirect method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="ea3ca-182">toohandle közvetlen metódushívások az IoT-központot, adja hozzá a hello következő beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-182">toohandle direct method calls from IoT Hub, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="ea3ca-183">Frissítés hello **fő** metódus aláírása tooinclude hello következő `throws` záradékban:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-183">Update hello **main** method signature tooinclude hello following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="ea3ca-184">Adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-184">Add hello following code toohello **main** method to:</span></span>
    * <span data-ttu-id="ea3ca-185">Hozzon létre egy eszköz ügyfél toocommunicate IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-185">Create a device client toocommunicate with IoT Hub.</span></span>
    * <span data-ttu-id="ea3ca-186">Hozzon létre egy **eszköz** toostore hello eszköztulajdonságok a két objektum.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-186">Create a **Device** object toostore hello device twin properties.</span></span>

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

1. <span data-ttu-id="ea3ca-187">toostart hello eszköz ügyfélszolgáltatások, adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-187">toostart hello device client services, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="ea3ca-188">a hello felhasználói toopress hello toowait **Enter** leállítása előtt kulcsban, adja hozzá a következő kód toohello vége hello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-188">toowait for hello user toopress hello **Enter** key before shutting down, add hello following code toohello end of hello **main** method:</span></span>

    ```java
    // Close hello app
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. <span data-ttu-id="ea3ca-189">Mentse és zárja be a hello `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-189">Save and close hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="ea3ca-190">Build hello **szimulált eszköz** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-190">Build hello **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="ea3ca-191">A parancssorban lépjen a toohello `simulated-device` mappa és a következő parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-191">At your command prompt, navigate toohello `simulated-device` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="ea3ca-192">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="ea3ca-192">Run hello apps</span></span>

<span data-ttu-id="ea3ca-193">Most már áll készen toorun hello konzol alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-193">You are now ready toorun hello console apps.</span></span>

1. <span data-ttu-id="ea3ca-194">A parancssorba a hello `simulated-device` mappa, futtassa a következő parancs toostart hello eszköz alkalmazás figyel a kívánt tulajdonságok módosítását és a közvetlen metódushívások hello:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-194">At a command prompt in hello `simulated-device` folder, run hello following command toostart hello device app listening for desired property changes and direct method calls:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![hello eszközügyfél kezdődik](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. <span data-ttu-id="ea3ca-196">A hello parancsot a parancssorba `schedule-jobs` mappa, futtassa a következő parancs toorun hello hello **feladatok ütemezése** szolgáltatás app toorun két feladatot.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-196">At a command prompt in hello `schedule-jobs` folder, run hello following command toorun hello **schedule-jobs** service app toorun two jobs.</span></span> <span data-ttu-id="ea3ca-197">hello először állítja be a kívánt hello tulajdonságértékek, második hívás hello hello közvetlen módszer:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-197">hello first sets hello desired property values, hello second calls hello direct method:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás alkalmazás két feladatot hoz létre.](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. <span data-ttu-id="ea3ca-199">hello eszközalkalmazás kezeli kívánt hello tulajdonság módosításának és hello közvetlen metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-199">hello device app handles hello desired property change and hello direct method call:</span></span>

    ![hello ügyfél válaszol toohello módosítások](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a><span data-ttu-id="ea3ca-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea3ca-201">Next steps</span></span>

<span data-ttu-id="ea3ca-202">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-202">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="ea3ca-203">A háttér-alkalmazások létrehozott toorun két feladatot.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-203">You created a back-end app toorun two jobs.</span></span> <span data-ttu-id="ea3ca-204">hello első feladat beállítani kívánt tulajdonságértékek, és egy közvetlen metódust hívott hello második feladat.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-204">hello first job set desired property values, and hello second job called a direct method.</span></span>

<span data-ttu-id="ea3ca-205">A következő erőforrások toolearn hogyan használja hello számára:</span><span class="sxs-lookup"><span data-stu-id="ea3ca-205">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="ea3ca-206">Telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-206">Send telemetry from devices with hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) tutorial.</span></span>
* <span data-ttu-id="ea3ca-207">Interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a hello [közvetlen módszerekkel](iot-hub-java-java-direct-methods.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="ea3ca-207">Control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods](iot-hub-java-java-direct-methods.md) tutorial.</span></span>
