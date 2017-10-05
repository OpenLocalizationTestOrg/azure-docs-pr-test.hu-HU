---
title: "Ismerkedés az Azure IoT Hub kezelés (Java) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub kezelés távoli eszköz újraindítás kezdeményezése. Az Azure IoT-eszközök SDK Java segítségével valósítja meg a szimulált eszköz alkalmazást, amely közvetlen módszer és az Azure IoT szolgáltatást megvalósítása, amely a közvetlen módszer hívja service-alkalmazást Java SDK tartalmazza."
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
ms.openlocfilehash: c75635f366f5ced4bf91792d1a905dd6aab8ed79
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-device-management-java"></a><span data-ttu-id="526f3-104">Eszközkezelés (Java) az első lépései</span><span class="sxs-lookup"><span data-stu-id="526f3-104">Get started with device management (Java)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="526f3-105">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="526f3-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="526f3-106">Az Azure-portál használatával létrehoz egy IoT-központot, és az IoT hub hozzon létre egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="526f3-106">Use the Azure portal to create an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="526f3-107">Létrehoz egy szimulált eszköz alkalmazást, amely az eszköz újraindítását közvetlen módszer.</span><span class="sxs-lookup"><span data-stu-id="526f3-107">Create a simulated device app that implements a direct method to reboot the device.</span></span> <span data-ttu-id="526f3-108">Közvetlen módszerek a felhőből hívják.</span><span class="sxs-lookup"><span data-stu-id="526f3-108">Direct methods are invoked from the cloud.</span></span>
* <span data-ttu-id="526f3-109">Hozzon létre egy alkalmazást, amely a szimulált eszköz alkalmazásának keresztül az IoT hub újraindítás közvetlen metódust hívja.</span><span class="sxs-lookup"><span data-stu-id="526f3-109">Create an app that invokes the reboot direct method in the simulated device app through your IoT hub.</span></span> <span data-ttu-id="526f3-110">Ezt az alkalmazást az eszközről, tekintse meg az újraindítás művelet befejezésekor jelentett tulajdonságok figyeli.</span><span class="sxs-lookup"><span data-stu-id="526f3-110">This app then monitors the reported properties from the device to see when the reboot operation is complete.</span></span>

<span data-ttu-id="526f3-111">Ez az oktatóanyag végén két Java konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="526f3-111">At the end of this tutorial, you have two Java console apps:</span></span>

<span data-ttu-id="526f3-112">**Szimulált eszköz**.</span><span class="sxs-lookup"><span data-stu-id="526f3-112">**simulated-device**.</span></span> <span data-ttu-id="526f3-113">Ezt az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="526f3-113">This app:</span></span>

* <span data-ttu-id="526f3-114">Az IoT hub csatlakozik a korábban létrehozott eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="526f3-114">Connects to your IoT hub with the device identity created earlier.</span></span>
* <span data-ttu-id="526f3-115">Újraindítás közvetlen módszer hívást kap.</span><span class="sxs-lookup"><span data-stu-id="526f3-115">Receives a reboot direct method call.</span></span>
* <span data-ttu-id="526f3-116">A fizikai számítógép újraindítása szimulálja.</span><span class="sxs-lookup"><span data-stu-id="526f3-116">Simulates a physical reboot.</span></span>
* <span data-ttu-id="526f3-117">A jelentés az utolsó újraindítás jelentett tulajdonságon keresztül idején.</span><span class="sxs-lookup"><span data-stu-id="526f3-117">Reports the time of the last reboot through a reported property.</span></span>

<span data-ttu-id="526f3-118">**eseményindító-újraindítás**.</span><span class="sxs-lookup"><span data-stu-id="526f3-118">**trigger-reboot**.</span></span> <span data-ttu-id="526f3-119">Ezt az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="526f3-119">This app:</span></span>

* <span data-ttu-id="526f3-120">A közvetlen módszer meghívja a szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="526f3-120">Calls a direct method in the simulated device app.</span></span>
* <span data-ttu-id="526f3-121">Megjeleníti a közvetlen módszer hívásához a szimulált eszköz által küldött válasz</span><span class="sxs-lookup"><span data-stu-id="526f3-121">Displays the response to the direct method call sent by the simulated device</span></span>
* <span data-ttu-id="526f3-122">Megjeleníti a módosított tulajdonságok jelentett.</span><span class="sxs-lookup"><span data-stu-id="526f3-122">Displays the updated reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="526f3-123">Az SDK-k használó alkalmazások futtatásához eszközökön és a megoldás háttérrendszeréhez kapcsolatos információkért lásd: [Azure IoT SDK-k][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="526f3-123">For information about the SDKs that you can use to build applications to run on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="526f3-124">Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="526f3-124">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="526f3-125">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="526f3-125">Java SE 8.</span></span> <br/> <span data-ttu-id="526f3-126">[A fejlesztési környezet előkészítését][lnk-dev-setup] ismertető cikk leírja, hogyan telepítheti a Javát ehhez az oktatóanyaghoz Windows vagy Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="526f3-126">[Prepare your development environment][lnk-dev-setup] describes how to install Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="526f3-127">Maven 3</span><span class="sxs-lookup"><span data-stu-id="526f3-127">Maven 3.</span></span>  <br/> <span data-ttu-id="526f3-128">[A fejlesztési környezet előkészítését][lnk-dev-setup] ismertető cikk leírja, hogyan telepítheti a [Mavent][lnk-maven] ehhez az oktatóanyaghoz Windows vagy Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="526f3-128">[Prepare your development environment][lnk-dev-setup] describes how to install [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="526f3-129">[NODE.js-verzió 0.10.0-s vagy újabb](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="526f3-129">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a><span data-ttu-id="526f3-130">Az eszközön, a közvetlen módszer használatával távoli újraindítás eseményindító</span><span class="sxs-lookup"><span data-stu-id="526f3-130">Trigger a remote reboot on the device using a direct method</span></span>

<span data-ttu-id="526f3-131">Ebben a szakaszban hoz létre egy Java-Konzolalkalmazás, amely:</span><span class="sxs-lookup"><span data-stu-id="526f3-131">In this section, you create a Java console app that:</span></span>

1. <span data-ttu-id="526f3-132">Meghívja a szimulált eszköz alkalmazásának újraindítás közvetlen metódust.</span><span class="sxs-lookup"><span data-stu-id="526f3-132">Invokes the reboot direct method in the simulated device app.</span></span>
1. <span data-ttu-id="526f3-133">A válasz megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="526f3-133">Displays the response.</span></span>
1. <span data-ttu-id="526f3-134">Szavazások a jelentésben szereplő tulajdonságok az eszköz számára küldött határozza meg, ha az újraindítás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="526f3-134">Polls the reported properties sent from the device to determine when the reboot is complete.</span></span>

<span data-ttu-id="526f3-135">A Konzolalkalmazás csatlakozik az IoT Hub meghívni a közvetlen metódust, és a jelentett tulajdonságainak olvasása.</span><span class="sxs-lookup"><span data-stu-id="526f3-135">This console app connects to your IoT Hub to invoke the direct method and read the reported properties.</span></span>

1. <span data-ttu-id="526f3-136">Hozzon létre egy üres nevű dm-get-started.</span><span class="sxs-lookup"><span data-stu-id="526f3-136">Create an empty folder called dm-get-started.</span></span>

1. <span data-ttu-id="526f3-137">A dm-get-started mappában hozzon létre egy Maven project nevű **eseményindító-újraindítás** parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="526f3-137">In the dm-get-started folder, create a Maven project called **trigger-reboot** using the following command at your command prompt.</span></span> <span data-ttu-id="526f3-138">Az alábbiakban látható egy egyetlen, hosszú parancsot:</span><span class="sxs-lookup"><span data-stu-id="526f3-138">The following shows a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="526f3-139">A parancssorban keresse meg az eseményindító-újraindítás mappát.</span><span class="sxs-lookup"><span data-stu-id="526f3-139">At your command prompt, navigate to the trigger-reboot folder.</span></span>

1. <span data-ttu-id="526f3-140">Egy szövegszerkesztővel nyissa meg a pom.xml fájlt az eseményindító-újraindítás mappában, és adja hozzá a következő függőség a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="526f3-140">Using a text editor, open the pom.xml file in the trigger-reboot folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="526f3-141">A függőség lehetővé teszi, hogy az iot-szolgáltatás-ügyfélcsomag az alkalmazás kommunikáljon az IoT hub:</span><span class="sxs-lookup"><span data-stu-id="526f3-141">This dependency enables you to use the iot-service-client package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="526f3-142">Az **iot-service-client** legújabb verzióját a [Maven keresési funkciójával][lnk-maven-service-search] tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="526f3-142">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="526f3-143">Adja hozzá a következő **build** csomópont után a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="526f3-143">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="526f3-144">Ez a konfiguráció arra utasítja a Java 1.8 az lehetővé teszi az alkalmazás Maven:</span><span class="sxs-lookup"><span data-stu-id="526f3-144">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="526f3-145">Mentse és zárja be a pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="526f3-145">Save and close the pom.xml file.</span></span>

1. <span data-ttu-id="526f3-146">Egy szövegszerkesztőben nyissa meg a trigger-reboot\src\main\java\com\mycompany\app\App.java forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="526f3-146">Using a text editor, open the trigger-reboot\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="526f3-147">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="526f3-147">Add the following **import** statements to the file:</span></span>

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

1. <span data-ttu-id="526f3-148">Adja hozzá a következő osztályszintű változókat az **App** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="526f3-148">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="526f3-149">Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal feljegyzett a *létrehoz egy IoT-központot* szakasz:</span><span class="sxs-lookup"><span data-stu-id="526f3-149">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in the *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. <span data-ttu-id="526f3-150">A szál, amely a jelentésben szereplő tulajdonságok beolvassa az eszköz kettős 10 másodpercenként alkalmazásához adja hozzá a következő beágyazott osztály a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="526f3-150">To implement a thread that reads the reported properties from the device twin every 10 seconds, add the following nested class to the **App** class:</span></span>

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

1. <span data-ttu-id="526f3-151">A rendszer újraindítása a szimulált eszköz közvetlen metódus meghívásához, adja hozzá az alábbi kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="526f3-151">To invoke the reboot direct method on the simulated device, add the following code to the **main** method:</span></span>

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

1. <span data-ttu-id="526f3-152">És kérdezze le a jelentett tulajdonságok a szimulált eszköz eltávolítása a szálat indítani, adja hozzá az alábbi kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="526f3-152">To start the thread to poll the reported properties from the simulated device, add the following code to the **main** method:</span></span>

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. <span data-ttu-id="526f3-153">Ahhoz, hogy állítsa le az alkalmazást, adja hozzá a következő kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="526f3-153">To enable you to stop the app, add the following code to the **main** method:</span></span>

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. <span data-ttu-id="526f3-154">Mentse és zárja be a trigger-reboot\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="526f3-154">Save and close the trigger-reboot\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="526f3-155">Build a **eseményindító-újraindítás** háttér-alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="526f3-155">Build the **trigger-reboot** back-end app and correct any errors.</span></span> <span data-ttu-id="526f3-156">A parancssorban keresse meg az eseményindító-újraindítás mappát, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="526f3-156">At your command prompt, navigate to the trigger-reboot folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="526f3-157">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="526f3-157">Create a simulated device app</span></span>

<span data-ttu-id="526f3-158">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely az eszköz szimulálja.</span><span class="sxs-lookup"><span data-stu-id="526f3-158">In this section, you create a Java console app that simulates a device.</span></span> <span data-ttu-id="526f3-159">Az alkalmazás figyeli, az újraindítást követően a közvetlen metódus hívása az IoT hub, és azonnal válaszol-e, hogy a hívást.</span><span class="sxs-lookup"><span data-stu-id="526f3-159">The app listens for the reboot direct method call from your IoT hub and immediately responds to that call.</span></span> <span data-ttu-id="526f3-160">A alkalmazást, majd a alszik egy ideig, hogy újraindítás szimulálja értesíteni jelentett tulajdonság használata előtt a **eseményindító-újraindítás** háttér-alkalmazást, hogy az újraindítás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="526f3-160">The app then sleeps for a while to simulate the reboot process before it uses a reported property to notify the **trigger-reboot** back-end app that the reboot is complete.</span></span>

1. <span data-ttu-id="526f3-161">A dm-get-started mappában hozzon létre egy Maven project nevű **szimulált eszköz** parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="526f3-161">In the dm-get-started folder, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="526f3-162">A következő egy olyan nagyméretű, sok parancs:</span><span class="sxs-lookup"><span data-stu-id="526f3-162">The following is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="526f3-163">A parancssorban lépjen a simulated-device mappára.</span><span class="sxs-lookup"><span data-stu-id="526f3-163">At your command prompt, navigate to the simulated-device folder.</span></span>

1. <span data-ttu-id="526f3-164">Egy szövegszerkesztővel nyissa meg a pom.xml fájlt a szimulált eszköz mappában, és adja hozzá a következő függőség a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="526f3-164">Using a text editor, open the pom.xml file in the simulated-device folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="526f3-165">A függőség lehetővé teszi, hogy az iot-szolgáltatás-ügyfélcsomag az alkalmazás kommunikáljon az IoT hub:</span><span class="sxs-lookup"><span data-stu-id="526f3-165">This dependency enables you to use the iot-service-client package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="526f3-166">Az **iot-device-client** legújabb verzióját a [Maven keresési funkciójával][lnk-maven-device-search] tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="526f3-166">You can check for the latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="526f3-167">Adja hozzá a következő **build** csomópont után a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="526f3-167">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="526f3-168">Ez a konfiguráció arra utasítja a Java 1.8 az lehetővé teszi az alkalmazás Maven:</span><span class="sxs-lookup"><span data-stu-id="526f3-168">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="526f3-169">Mentse és zárja be a pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="526f3-169">Save and close the pom.xml file.</span></span>

1. <span data-ttu-id="526f3-170">Egy szövegszerkesztőben nyissa meg a simulated-device\src\main\java\com\mycompany\app\App.java forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="526f3-170">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="526f3-171">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="526f3-171">Add the following **import** statements to the file:</span></span>

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

1. <span data-ttu-id="526f3-172">Adja hozzá a következő osztályszintű változókat az **App** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="526f3-172">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="526f3-173">Cserélje le `{yourdeviceconnectionstring}` feljegyzett eszköz kapcsolati karakterlánccal rendelkező a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="526f3-173">Replace `{yourdeviceconnectionstring}` with the device connection string you noted in the *Create a device identity* section:</span></span>

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. <span data-ttu-id="526f3-174">Egy visszahívás-kezelő, a közvetlen módszer állapoteseményeit alkalmazásához adja hozzá a következő beágyazott osztályt a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="526f3-174">To implement a callback handler for direct method status events, add the following nested class to the **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="526f3-175">Egy visszahívás-kezelő eszköz iker állapoteseményeit a alkalmazásához adja hozzá a következő beágyazott osztályt a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="526f3-175">To implement a callback handler for device twin status events, add the following nested class to the **App** class:</span></span>

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

1. <span data-ttu-id="526f3-176">Egy visszahívás-kezelő tulajdonság események alkalmazásához adja hozzá a következő beágyazott osztályt a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="526f3-176">To implement a callback handler for property events, add the following nested class to the **App** class:</span></span>

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

1. <span data-ttu-id="526f3-177">A szál az eszköz újraindítás szimulálásához alkalmazásához adja hozzá a következő beágyazott osztályt a **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="526f3-177">To implement a thread to simulate the device reboot, add the following nested class to the **App** class.</span></span> <span data-ttu-id="526f3-178">A szál öt másodpercenként alvó állapotba kerül, és ezután beállítja a **lastReboot** tulajdonság jelentette:</span><span class="sxs-lookup"><span data-stu-id="526f3-178">The thread sleeps for five seconds and then sets the **lastReboot** reported property:</span></span>

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

1. <span data-ttu-id="526f3-179">Az eszközön a közvetlen módszer alkalmazásához adja hozzá a következő beágyazott osztály a **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="526f3-179">To implement the direct method on the device, add the following nested class to the **App** class.</span></span> <span data-ttu-id="526f3-180">Ha a szimulált alkalmazások a hívást kap a **újraindítás** közvetlen módszer azt nyugtázást visszatér a hívó, majd elindítja az újraindítás feldolgozására szolgáló szálat:</span><span class="sxs-lookup"><span data-stu-id="526f3-180">When the simulated app receives a call to the **reboot** direct method, it returns an acknowledgement to the caller and then starts a thread to process the reboot:</span></span>

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

1. <span data-ttu-id="526f3-181">Aláírását, valamint módosíthatja a **fő** metódust kell küldeni a következő kivételekkel:</span><span class="sxs-lookup"><span data-stu-id="526f3-181">Modify the signature of the **main** method to throw the following exceptions:</span></span>

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="526f3-182">Példányt létrehozni egy **DeviceClient**, adja hozzá a következő kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="526f3-182">To instantiate a **DeviceClient**, add the following code to the **main** method:</span></span>

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. <span data-ttu-id="526f3-183">Megkezdeni a figyelést közvetlen metódushívások, adja hozzá a következő kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="526f3-183">To start listening for direct method calls, add the following code to the **main** method:</span></span>

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed to direct methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. <span data-ttu-id="526f3-184">Állítsa le az eszköz szimulátor, adja hozzá az alábbi kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="526f3-184">To shut down the device simulator, add the following code to the **main** method:</span></span>

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. <span data-ttu-id="526f3-185">Mentse és zárja be a simulated-device\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="526f3-185">Save and close the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="526f3-186">Build a **szimulált eszköz** háttér-alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="526f3-186">Build the **simulated-device** back-end app and correct any errors.</span></span> <span data-ttu-id="526f3-187">A parancssorban keresse meg a szimulált eszköz mappát, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="526f3-187">At your command prompt, navigate to the simulated-device folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a><span data-ttu-id="526f3-188">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="526f3-188">Run the apps</span></span>

<span data-ttu-id="526f3-189">Most már készen áll az alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="526f3-189">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="526f3-190">Parancsot egy parancssorba a szimulált eszköz mappában futtassa a következő parancsot az IoT hub az újraindítás metódushívások figyelését:</span><span class="sxs-lookup"><span data-stu-id="526f3-190">At a command prompt in the simulated-device folder, run the following command to begin listening for reboot method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT Hub szimulált eszköz alkalmazás figyelését, indítsa újra közvetlen metódushívások][1]

1. <span data-ttu-id="526f3-192">A parancssorba az eseményindító-újraindítás mappában futtassa a következő parancsot a rendszer újraindítása metódusát hívja a szimulált eszköz az IoT hub:</span><span class="sxs-lookup"><span data-stu-id="526f3-192">At a command prompt in the trigger-reboot folder, run the following command to call the reboot method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás alkalmazásnak, hogy a rendszer újraindítása közvetlen metódus hívása][2]

1. <span data-ttu-id="526f3-194">A szimulált eszköz válaszol-e a rendszer újraindítása közvetlen metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="526f3-194">The simulated device responds to the reboot direct method call:</span></span>

    ![Java IoT Hub szimulált eszköz alkalmazásának válaszol-e a közvetlen metódus hívása][3]

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