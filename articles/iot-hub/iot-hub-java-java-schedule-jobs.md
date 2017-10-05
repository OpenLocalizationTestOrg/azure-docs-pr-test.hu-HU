---
title: "Az Azure IoT Hub (Java) feladatok ütemezése |} Microsoft Docs"
description: "Hogyan kell egy Azure IoT Hub feladatot közvetlen metódus, és állítsa be a kívánt tulajdonságot több eszközön. Az Azure IoT-eszközök SDK Java segítségével valósítja meg a szimulált eszköz alkalmazások és az Azure IoT szolgáltatás Java SDK egy szolgáltatás alkalmazásnak, hogy a feladat végrehajtásához."
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
ms.openlocfilehash: 003a548ef2da2921a699df1aa9f7aee366d341ab
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="schedule-and-broadcast-jobs-java"></a><span data-ttu-id="a14a3-104">Ütemezés és a feladatok (Java)</span><span class="sxs-lookup"><span data-stu-id="a14a3-104">Schedule and broadcast jobs (Java)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="a14a3-105">Azure IoT Hub használatával ütemezése, illetve követheti nyomon, hogy az eszközök millióira frissítése feladatok.</span><span class="sxs-lookup"><span data-stu-id="a14a3-105">Use Azure IoT Hub to schedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="a14a3-106">Feladatok használata:</span><span class="sxs-lookup"><span data-stu-id="a14a3-106">Use jobs to:</span></span>

* <span data-ttu-id="a14a3-107">Eszköz kívánt tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="a14a3-107">Update desired properties</span></span>
* <span data-ttu-id="a14a3-108">Címkék frissítése</span><span class="sxs-lookup"><span data-stu-id="a14a3-108">Update tags</span></span>
* <span data-ttu-id="a14a3-109">Közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="a14a3-109">Invoke direct methods</span></span>

<span data-ttu-id="a14a3-110">Egy feladat becsomagolja az alábbi műveletek egyikét, és nyomon követi az eszközök végrehajtásának.</span><span class="sxs-lookup"><span data-stu-id="a14a3-110">A job wraps one of these actions and tracks the execution against a set of devices.</span></span> <span data-ttu-id="a14a3-111">Eszköz két lekérdezést meghatároz az eszközök a feldolgozás futtatása ellen.</span><span class="sxs-lookup"><span data-stu-id="a14a3-111">A device twin query defines the set of devices the job executes against.</span></span> <span data-ttu-id="a14a3-112">Például egy háttér-alkalmazás segítségével egy feladat meghívni egy 10 000 közvetlen metódust, amely az eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="a14a3-112">For example, a back-end app can use a job to invoke a direct method on 10,000 devices that reboots the devices.</span></span> <span data-ttu-id="a14a3-113">Adjon meg az eszközök eszköz iker lekérdezéssel, és a feladat egy későbbi időpontban futtatásának ütemezését.</span><span class="sxs-lookup"><span data-stu-id="a14a3-113">You specify the set of devices with a device twin query and schedule the job to run at a future time.</span></span> <span data-ttu-id="a14a3-114">A feladat nyomon követi előrehaladását, mint a eszközökhöz kapja meg, és a rendszer újraindítása közvetlen metódus végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="a14a3-114">The job tracks progress as each of the devices receive and execute the reboot direct method.</span></span>

<span data-ttu-id="a14a3-115">Az egyes képességek kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="a14a3-115">To learn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="a14a3-116">A két eszköz és a tulajdonságok: [eszköz twins az első lépései](iot-hub-java-java-twin-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="a14a3-116">Device twin and properties: [Get started with device twins](iot-hub-java-java-twin-getstarted.md)</span></span>
* <span data-ttu-id="a14a3-117">A közvetlen módszer: [IoT Hub fejlesztői útmutató - közvetlen módszerek](iot-hub-devguide-direct-methods.md) és [oktatóanyag: közvetlen módszerekkel](iot-hub-java-java-direct-methods.md)</span><span class="sxs-lookup"><span data-stu-id="a14a3-117">Direct methods: [IoT Hub developer guide - direct methods](iot-hub-devguide-direct-methods.md) and [Tutorial: Use direct methods](iot-hub-java-java-direct-methods.md)</span></span>

<span data-ttu-id="a14a3-118">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="a14a3-118">This tutorial shows you how to:</span></span>

* <span data-ttu-id="a14a3-119">Hozzon létre egy eszköz-alkalmazást, amely egy közvetlen a hívott metódus **lockDoor**.</span><span class="sxs-lookup"><span data-stu-id="a14a3-119">Create a device app that implements a direct method called **lockDoor**.</span></span> <span data-ttu-id="a14a3-120">Az eszköz alkalmazás is fogad kívánt tulajdonságok módosítását a háttér-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a14a3-120">The device app also receives desired property changes from the back-end app.</span></span>
* <span data-ttu-id="a14a3-121">Hozzon létre egy háttér-alkalmazást, amely létrehoz egy feladatot, amely hívja az **lockDoor** közvetlen módszer több eszközön.</span><span class="sxs-lookup"><span data-stu-id="a14a3-121">Create a back-end app that creates a job to call the **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="a14a3-122">Egy másik feladat kívánt tulajdonság frissítései több eszközre elküldi.</span><span class="sxs-lookup"><span data-stu-id="a14a3-122">Another job sends desired property updates to multiple devices.</span></span>

<span data-ttu-id="a14a3-123">Ez az oktatóanyag végén akkor egy java Konzolalkalmazás eszköz és a java háttér-Konzolalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="a14a3-123">At the end of this tutorial, you have a java console device app and a java console back-end app:</span></span>

<span data-ttu-id="a14a3-124">**Szimulált eszköz** , amely csatlakozik az IoT hub valósít meg a **lockDoor** közvetlen metódust, és kezeli a szükséges tulajdonságok módosítását.</span><span class="sxs-lookup"><span data-stu-id="a14a3-124">**simulated-device** that connects to your IoT hub, implements the **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="a14a3-125">**feladatok ütemezése** feladatok segítségével, amely hívja az **lockDoor** közvetlen módszer, és frissítse a eszköztulajdonságok iker szükséges több eszközön.</span><span class="sxs-lookup"><span data-stu-id="a14a3-125">**schedule-jobs** that use jobs to call the **lockDoor** direct method and update the device twin desired properties on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="a14a3-126">A cikk [Azure IoT SDK-k](iot-hub-devguide-sdks.md) használható eszközt és a háttér-alkalmazások az Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="a14a3-126">The article [Azure IoT SDKs](iot-hub-devguide-sdks.md) provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a14a3-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a14a3-127">Prerequisites</span></span>

<span data-ttu-id="a14a3-128">Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="a14a3-128">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="a14a3-129">A legújabb [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="a14a3-129">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="a14a3-130">Maven 3</span><span class="sxs-lookup"><span data-stu-id="a14a3-130">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="a14a3-131">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a14a3-131">An active Azure account.</span></span> <span data-ttu-id="a14a3-132">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="a14a3-132">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="a14a3-133">Ha inkább hozzon létre az eszközidentitást programozott módon, olvassa el a megfelelő részt a [csatlakoztassa az eszközt az IoT hub Java használatával](iot-hub-java-java-getstarted.md#create-a-device-identity) cikk.</span><span class="sxs-lookup"><span data-stu-id="a14a3-133">If you prefer to create the device identity programmatically, read the corresponding section in the [Connect your device to your IoT hub using Java](iot-hub-java-java-getstarted.md#create-a-device-identity) article.</span></span> <span data-ttu-id="a14a3-134">Használhatja a [IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) hozzáad egy eszközt az IoT hub eszköz.</span><span class="sxs-lookup"><span data-stu-id="a14a3-134">You can also use the [iothub-explorer](https://github.com/Azure/iothub-explorer) tool to add a device to your IoT hub.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="a14a3-135">A service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a14a3-135">Create the service app</span></span>

<span data-ttu-id="a14a3-136">Ebben a szakaszban egy feladatok használó Java-Konzolalkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="a14a3-136">In this section, you create a Java console app that uses jobs to:</span></span>

* <span data-ttu-id="a14a3-137">Hívja a **lockDoor** közvetlen módszer több eszközön.</span><span class="sxs-lookup"><span data-stu-id="a14a3-137">Call the **lockDoor** direct method on multiple devices.</span></span>
* <span data-ttu-id="a14a3-138">Kívánt tulajdonságokkal küldése több eszközre.</span><span class="sxs-lookup"><span data-stu-id="a14a3-138">Send desired properties to multiple devices.</span></span>

<span data-ttu-id="a14a3-139">Az alkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="a14a3-139">To create the app:</span></span>

1. <span data-ttu-id="a14a3-140">A fejlesztői gépen, hozzon létre egy üres nevű `iot-java-schedule-jobs`.</span><span class="sxs-lookup"><span data-stu-id="a14a3-140">On your development machine, create an empty folder called `iot-java-schedule-jobs`.</span></span>

1. <span data-ttu-id="a14a3-141">Az a `iot-java-schedule-jobs` mappa, úgynevezett Maven-projekt létrehozása **feladatok ütemezése** parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="a14a3-141">In the `iot-java-schedule-jobs` folder, create a Maven project called **schedule-jobs** using the following command at your command prompt.</span></span> <span data-ttu-id="a14a3-142">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="a14a3-142">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="a14a3-143">A parancssorban navigáljon a `schedule-jobs` mappát.</span><span class="sxs-lookup"><span data-stu-id="a14a3-143">At your command prompt, navigate to the `schedule-jobs` folder.</span></span>

1. <span data-ttu-id="a14a3-144">Egy szövegszerkesztőben nyissa meg a `pom.xml` fájlt a `schedule-jobs` mappa, és adja hozzá a következő függőség a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="a14a3-144">Using a text editor, open the `pom.xml` file in the `schedule-jobs` folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="a14a3-145">A függőség lehetővé teszi, hogy a **iot-szolgáltatásügyfél** csomag kommunikáljon az IoT hub az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="a14a3-145">This dependency enables you to use the **iot-service-client** package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="a14a3-146">Ellenőrizze, hogy a legújabb **iot-szolgáltatásügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="a14a3-146">You can check for the latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="a14a3-147">Adja hozzá a következő **build** csomópont után a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="a14a3-147">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="a14a3-148">Ez a konfiguráció arra utasítja a Java 1.8 az lehetővé teszi az alkalmazás Maven:</span><span class="sxs-lookup"><span data-stu-id="a14a3-148">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="a14a3-149">Mentse és zárja be a `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a14a3-149">Save and close the `pom.xml` file.</span></span>

1. <span data-ttu-id="a14a3-150">Egy szövegszerkesztőben nyissa meg a `schedule-jobs\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a14a3-150">Using a text editor, open the `schedule-jobs\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="a14a3-151">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="a14a3-151">Add the following **import** statements to the file:</span></span>

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

1. <span data-ttu-id="a14a3-152">Adja hozzá a következő osztályszintű változókat az **App** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="a14a3-152">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="a14a3-153">Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal feljegyzett a *létrehoz egy IoT-központot* szakasz:</span><span class="sxs-lookup"><span data-stu-id="a14a3-153">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in the *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long the job is permitted to run without
    // completing its work on the set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. <span data-ttu-id="a14a3-154">Adja hozzá a következő metódust a **App** osztály frissítése feladat ütemezése a **épület** és **emelet** az eszköz a két tulajdonság szükséges:</span><span class="sxs-lookup"><span data-stu-id="a14a3-154">Add the following method to the **App** class to schedule a job to update the **Building** and **Floor** desired properties in the device twin:</span></span>

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule the update twin job to run now
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

1. <span data-ttu-id="a14a3-155">Hívása a feladat ütemezése a **lockDoor** módszer, adja hozzá a következő metódust a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="a14a3-155">To schedule a job to call the **lockDoor** method, add the following method to the **App** class:</span></span>

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now to call the lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set to 5 seconds.
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

1. <span data-ttu-id="a14a3-156">Egy feladat figyeléséhez, adja hozzá a következő metódust a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="a14a3-156">To monitor a job, add the following method to the **App** class:</span></span>

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check the job result until it's completed
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

1. <span data-ttu-id="a14a3-157">A futtatott feladatok részletes adatainak lekérdezéséhez adja hozzá a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="a14a3-157">To query for the details of the jobs you ran, add the following method:</span></span>

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using the time the jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over the list of jobs and print the details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

1. <span data-ttu-id="a14a3-158">Frissítés a **fő** tartalmazza a következő metódus-aláírás `throws` záradékban:</span><span class="sxs-lookup"><span data-stu-id="a14a3-158">Update the **main** method signature to include the following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. <span data-ttu-id="a14a3-159">Futtathatja, és figyelheti a két feladatok egymás után, adja hozzá az alábbi kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="a14a3-159">To run and monitor two jobs sequentially, add the following code to the **main** method:</span></span>

    ```java
    // Record the start time
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

    // Run a query to show the job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

1. <span data-ttu-id="a14a3-160">Mentse és zárja be a `schedule-jobs\src\main\java\com\mycompany\app\App.java` fájl</span><span class="sxs-lookup"><span data-stu-id="a14a3-160">Save and close the `schedule-jobs\src\main\java\com\mycompany\app\App.java` file</span></span>

1. <span data-ttu-id="a14a3-161">Build a **feladatok ütemezése** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="a14a3-161">Build the **schedule-jobs** app and correct any errors.</span></span> <span data-ttu-id="a14a3-162">A parancssorban navigáljon a `schedule-jobs` mappa, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a14a3-162">At your command prompt, navigate to the `schedule-jobs` folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a><span data-ttu-id="a14a3-163">Eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a14a3-163">Create a device app</span></span>

<span data-ttu-id="a14a3-164">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely a kívánt tulajdonságokkal, az IoT-központ által küldött kezeli, és megvalósítja az közvetlen metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="a14a3-164">In this section, you create a Java console app that handles the desired properties sent from IoT Hub and implements the direct method call.</span></span>

1. <span data-ttu-id="a14a3-165">Az a `iot-java-schedule-jobs` mappa, úgynevezett Maven-projekt létrehozása **szimulált eszköz** parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="a14a3-165">In the `iot-java-schedule-jobs` folder, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="a14a3-166">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="a14a3-166">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="a14a3-167">A parancssorban navigáljon a `simulated-device` mappát.</span><span class="sxs-lookup"><span data-stu-id="a14a3-167">At your command prompt, navigate to the `simulated-device` folder.</span></span>

1. <span data-ttu-id="a14a3-168">Egy szövegszerkesztőben nyissa meg a `pom.xml` fájlt a `simulated-device` mappa, és adja hozzá az alábbi függőségeket a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="a14a3-168">Using a text editor, open the `pom.xml` file in the `simulated-device` folder and add the following dependencies to the **dependencies** node.</span></span> <span data-ttu-id="a14a3-169">A függőség lehetővé teszi, hogy a **iot-eszközügyfél** csomag kommunikáljon az IoT hub az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="a14a3-169">This dependency enables you to use the **iot-device-client** package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="a14a3-170">Ellenőrizze, hogy a legújabb **iot-eszközügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="a14a3-170">You can check for the latest version of **iot-device-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="a14a3-171">Adja hozzá a következő **build** csomópont után a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="a14a3-171">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="a14a3-172">Ez a konfiguráció arra utasítja a Java 1.8 az lehetővé teszi az alkalmazás Maven:</span><span class="sxs-lookup"><span data-stu-id="a14a3-172">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="a14a3-173">Mentse és zárja be a `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a14a3-173">Save and close the `pom.xml` file.</span></span>

1. <span data-ttu-id="a14a3-174">Egy szövegszerkesztőben nyissa meg a `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a14a3-174">Using a text editor, open the `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="a14a3-175">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="a14a3-175">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="a14a3-176">Adja hozzá a következő osztályszintű változókat az **App** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="a14a3-176">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="a14a3-177">A mag cseréje `{youriothubname}` az IoT hub nevű és `{yourdevicekey}` a eszközkulcs értékre hozott létre a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="a14a3-177">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with the device key value you generated in the *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="a14a3-178">Ez a mintaalkalmazás a **protocol** változót használja egy **DeviceClient** objektum példányának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a14a3-178">This sample app uses the **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="a14a3-179">Jelenleg eszköz iker szolgáltatásait is használni kell használnia a MQTT protokoll.</span><span class="sxs-lookup"><span data-stu-id="a14a3-179">Currently, to use device twin features you must use the MQTT protocol.</span></span>

1. <span data-ttu-id="a14a3-180">A konzol eszköz iker értesítések nyomtatni, adja hozzá a következő beágyazott osztály a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="a14a3-180">To print device twin notifications to the console, add the following nested class to the **App** class:</span></span>

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="a14a3-181">A konzol közvetlen módszer értesítések nyomtatni, adja hozzá a következő beágyazott osztályt a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="a14a3-181">To print direct method notifications to the console, add the following nested class to the **App** class:</span></span>

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to direct method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="a14a3-182">Az IoT-központ közvetlen metódushívások kezeli, vegye fel a következő beágyazott osztály a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="a14a3-182">To handle direct method calls from IoT Hub, add the following nested class to the **App** class:</span></span>

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

1. <span data-ttu-id="a14a3-183">Frissítés a **fő** tartalmazza a következő metódus-aláírás `throws` záradékban:</span><span class="sxs-lookup"><span data-stu-id="a14a3-183">Update the **main** method signature to include the following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="a14a3-184">Adja hozzá a következő kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="a14a3-184">Add the following code to the **main** method to:</span></span>
    * <span data-ttu-id="a14a3-185">Hozzon létre egy ügyfél-kommunikáljon az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a14a3-185">Create a device client to communicate with IoT Hub.</span></span>
    * <span data-ttu-id="a14a3-186">Hozzon létre egy **eszköz** objektum iker eszköztulajdonságok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="a14a3-186">Create a **Device** object to store the device twin properties.</span></span>

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object to manage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

1. <span data-ttu-id="a14a3-187">Indítsa el az eszközt tartalmazó ügyfélszolgáltatások, adja hozzá az alábbi kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="a14a3-187">To start the device client services, add the following code to the **main** method:</span></span>

    ```java
    try {
      // Open the DeviceClient
      // Start the device twin services
      // Subscribe to direct method calls
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

1. <span data-ttu-id="a14a3-188">Várja meg, hogy nyomja meg a felhasználó a **Enter** leállítása előtt kulcsban, adja hozzá a következő kódot végén a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="a14a3-188">To wait for the user to press the **Enter** key before shutting down, add the following code to the end of the **main** method:</span></span>

    ```java
    // Close the app
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. <span data-ttu-id="a14a3-189">Mentse és zárja be a `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a14a3-189">Save and close the `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="a14a3-190">Build a **szimulált eszköz** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="a14a3-190">Build the **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="a14a3-191">A parancssorban navigáljon a `simulated-device` mappa, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a14a3-191">At your command prompt, navigate to the `simulated-device` folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a><span data-ttu-id="a14a3-192">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="a14a3-192">Run the apps</span></span>

<span data-ttu-id="a14a3-193">Most már készen áll a konzol alkalmazások futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a14a3-193">You are now ready to run the console apps.</span></span>

1. <span data-ttu-id="a14a3-194">A parancsot a parancssorba a `simulated-device` mappát, az eszköz alkalmazás figyel a kívánt tulajdonságok módosítását és a közvetlen metódushívások indításához a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a14a3-194">At a command prompt in the `simulated-device` folder, run the following command to start the device app listening for desired property changes and direct method calls:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Az eszközügyfél kezdődik](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. <span data-ttu-id="a14a3-196">A parancsot a parancssorba a `schedule-jobs` mappa futtatásához a következő parancsot a **feladatok ütemezése** service alkalmazás két feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a14a3-196">At a command prompt in the `schedule-jobs` folder, run the following command to run the **schedule-jobs** service app to run two jobs.</span></span> <span data-ttu-id="a14a3-197">Az első állítja be a kívánt tulajdonság értékével, a második meghívja a közvetlen módszer:</span><span class="sxs-lookup"><span data-stu-id="a14a3-197">The first sets the desired property values, the second calls the direct method:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás alkalmazás két feladatot hoz létre.](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. <span data-ttu-id="a14a3-199">Az eszköz alkalmazás kezeli a kívánt tulajdonság módosításának és a közvetlen metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="a14a3-199">The device app handles the desired property change and the direct method call:</span></span>

    ![Az eszközügyfél válaszol-e a módosításokat](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a><span data-ttu-id="a14a3-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a14a3-201">Next steps</span></span>

<span data-ttu-id="a14a3-202">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="a14a3-202">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="a14a3-203">Létrehozott egy háttér-alkalmazás két feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a14a3-203">You created a back-end app to run two jobs.</span></span> <span data-ttu-id="a14a3-204">Az első munkahelye kívánt tulajdonságértékei állíthatók, és a második feladat közvetlen metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="a14a3-204">The first job set desired property values, and the second job called a direct method.</span></span>

<span data-ttu-id="a14a3-205">A következő források segítségével megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="a14a3-205">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="a14a3-206">Telemetriai adatokat küldhet az eszközökről a [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a14a3-206">Send telemetry from devices with the [Get started with IoT Hub](iot-hub-java-java-getstarted.md) tutorial.</span></span>
* <span data-ttu-id="a14a3-207">Az interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a [közvetlen módszerekkel](iot-hub-java-java-direct-methods.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a14a3-207">Control devices interactively (such as turning on a fan from a user-controlled app) with the [Use direct methods](iot-hub-java-java-direct-methods.md) tutorial.</span></span>
