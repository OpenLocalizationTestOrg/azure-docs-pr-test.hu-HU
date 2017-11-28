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
# <a name="get-started-with-device-management-java"></a><span data-ttu-id="7c1ec-104">Eszközkezelés (Java) az első lépései</span><span class="sxs-lookup"><span data-stu-id="7c1ec-104">Get started with device management (Java)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="7c1ec-105">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="7c1ec-106">Az Azure portál toocreate az IoT-központ hello használata, és az IoT hub hozzon létre egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-106">Use hello Azure portal toocreate an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="7c1ec-107">Létrehoz egy szimulált eszköz alkalmazást, amely megvalósítja a közvetlen módszer tooreboot hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-107">Create a simulated device app that implements a direct method tooreboot hello device.</span></span> <span data-ttu-id="7c1ec-108">Közvetlen módszerek hello felhőből hívják.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-108">Direct methods are invoked from hello cloud.</span></span>
* <span data-ttu-id="7c1ec-109">Hozzon létre egy alkalmazást, amely hívja meg hello újraindítás közvetlen módszer az IoT hub keresztül hello szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-109">Create an app that invokes hello reboot direct method in hello simulated device app through your IoT hub.</span></span> <span data-ttu-id="7c1ec-110">Ez app majd figyelők hello hello eszköz toosee jelentett tulajdonságait, ha hello újraindítás művelete befejeződött.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-110">This app then monitors hello reported properties from hello device toosee when hello reboot operation is complete.</span></span>

<span data-ttu-id="7c1ec-111">Ez az oktatóanyag végén hello két Java konzol alkalmazások közül választhat:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-111">At hello end of this tutorial, you have two Java console apps:</span></span>

<span data-ttu-id="7c1ec-112">**Szimulált eszköz**.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-112">**simulated-device**.</span></span> <span data-ttu-id="7c1ec-113">Ezt az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-113">This app:</span></span>

* <span data-ttu-id="7c1ec-114">A korábban létrehozott hello eszközidentitás tooyour IoT-központ kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-114">Connects tooyour IoT hub with hello device identity created earlier.</span></span>
* <span data-ttu-id="7c1ec-115">Újraindítás közvetlen módszer hívást kap.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-115">Receives a reboot direct method call.</span></span>
* <span data-ttu-id="7c1ec-116">A fizikai számítógép újraindítása szimulálja.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-116">Simulates a physical reboot.</span></span>
* <span data-ttu-id="7c1ec-117">Jelentések hello idő az utolsó újraindítás hello jelentett tulajdonságon keresztül.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-117">Reports hello time of hello last reboot through a reported property.</span></span>

<span data-ttu-id="7c1ec-118">**eseményindító-újraindítás**.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-118">**trigger-reboot**.</span></span> <span data-ttu-id="7c1ec-119">Ezt az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-119">This app:</span></span>

* <span data-ttu-id="7c1ec-120">Meghívja a közvetlen módszer hello szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-120">Calls a direct method in hello simulated device app.</span></span>
* <span data-ttu-id="7c1ec-121">Hello válasz toohello közvetlen metódus hívása hello szimulált eszköz által küldött jeleníti meg</span><span class="sxs-lookup"><span data-stu-id="7c1ec-121">Displays hello response toohello direct method call sent by hello simulated device</span></span>
* <span data-ttu-id="7c1ec-122">Frissített megjeleníti hello jelentett tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-122">Displays hello updated reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="7c1ec-123">Használható toobuild alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello SDK-kkal kapcsolatos információk: [Azure IoT SDK-k][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="7c1ec-123">For information about hello SDKs that you can use toobuild applications toorun on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="7c1ec-124">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-124">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="7c1ec-125">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-125">Java SE 8.</span></span> <br/> <span data-ttu-id="7c1ec-126">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Java ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-126">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="7c1ec-127">Maven 3</span><span class="sxs-lookup"><span data-stu-id="7c1ec-127">Maven 3.</span></span>  <br/> <span data-ttu-id="7c1ec-128">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall [Maven] [ lnk-maven] ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-128">[Prepare your development environment][lnk-dev-setup] describes how tooinstall [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="7c1ec-129">[NODE.js-verzió 0.10.0-s vagy újabb](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="7c1ec-129">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a><span data-ttu-id="7c1ec-130">Eseményindító közvetlen metódussal hello eszközön távoli újraindítás</span><span class="sxs-lookup"><span data-stu-id="7c1ec-130">Trigger a remote reboot on hello device using a direct method</span></span>

<span data-ttu-id="7c1ec-131">Ebben a szakaszban hoz létre egy Java-Konzolalkalmazás, amely:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-131">In this section, you create a Java console app that:</span></span>

1. <span data-ttu-id="7c1ec-132">Meghívja a hello újraindítás közvetlen módszer hello szimulált eszköz alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-132">Invokes hello reboot direct method in hello simulated device app.</span></span>
1. <span data-ttu-id="7c1ec-133">Hello válasz jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-133">Displays hello response.</span></span>
1. <span data-ttu-id="7c1ec-134">Szavazások hello tulajdonságok hello eszköz toodetermine küldött hello újraindítás befejezésekor jelentett.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-134">Polls hello reported properties sent from hello device toodetermine when hello reboot is complete.</span></span>

<span data-ttu-id="7c1ec-135">A Konzolalkalmazás csatlakozik az IoT-központ tooyour tooinvoke hello közvetlen módszer és olvasási hello jelentett tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-135">This console app connects tooyour IoT Hub tooinvoke hello direct method and read hello reported properties.</span></span>

1. <span data-ttu-id="7c1ec-136">Hozzon létre egy üres nevű dm-get-started.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-136">Create an empty folder called dm-get-started.</span></span>

1. <span data-ttu-id="7c1ec-137">A dm-get-started hello mappában nevű Maven-projekt létrehozása **eseményindító-újraindítás** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-137">In hello dm-get-started folder, create a Maven project called **trigger-reboot** using hello following command at your command prompt.</span></span> <span data-ttu-id="7c1ec-138">hello következő egyetlen, hosszú parancs jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-138">hello following shows a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="7c1ec-139">A parancssorban keresse meg a toohello eseményindító-újraindítás mappa.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-139">At your command prompt, navigate toohello trigger-reboot folder.</span></span>

1. <span data-ttu-id="7c1ec-140">Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello eseményindító-újraindítás mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-140">Using a text editor, open hello pom.xml file in hello trigger-reboot folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="7c1ec-141">A függőség lehetővé teszi, hogy Ön toouse hello iot-szolgáltatás-ügyfélcsomagját a app toocommunicate az IoT hubbal:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-141">This dependency enables you toouse hello iot-service-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="7c1ec-142">Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="7c1ec-142">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="7c1ec-143">Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-143">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="7c1ec-144">Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-144">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="7c1ec-145">Mentse és zárja be a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-145">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="7c1ec-146">Egy szövegszerkesztőben nyissa meg hello trigger-reboot\src\main\java\com\mycompany\app\App.java forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-146">Using a text editor, open hello trigger-reboot\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="7c1ec-147">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-147">Add hello following **import** statements toohello file:</span></span>

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

1. <span data-ttu-id="7c1ec-148">Adja hozzá a következő osztály változók toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-148">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="7c1ec-149">Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal hello feljegyzett *létrehoz egy IoT-központot* szakasz:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-149">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. <span data-ttu-id="7c1ec-150">a szál hello olvasó tooimplement jelentett tulajdonságok hello eszköz kettős 10 másodpercenként hozzá hello következő beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-150">tooimplement a thread that reads hello reported properties from hello device twin every 10 seconds, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="7c1ec-151">tooinvoke hello újraindítás közvetlen módszer hello szimulált eszköz, adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-151">tooinvoke hello reboot direct method on hello simulated device, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="7c1ec-152">toostart hello szál toopoll hello hello szimulált eszköz jelentett tulajdonságait, adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-152">toostart hello thread toopoll hello reported properties from hello simulated device, add hello following code toohello **main** method:</span></span>

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. <span data-ttu-id="7c1ec-153">tooenable toostop hello alkalmazást, adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-153">tooenable you toostop hello app, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Press ENTER tooexit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. <span data-ttu-id="7c1ec-154">Mentse és zárja be hello trigger-reboot\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-154">Save and close hello trigger-reboot\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="7c1ec-155">Build hello **eseményindító-újraindítás** háttér-alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-155">Build hello **trigger-reboot** back-end app and correct any errors.</span></span> <span data-ttu-id="7c1ec-156">A parancssorban lépjen a toohello eseményindító-újraindítás mappa és a következő parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-156">At your command prompt, navigate toohello trigger-reboot folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="7c1ec-157">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c1ec-157">Create a simulated device app</span></span>

<span data-ttu-id="7c1ec-158">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely az eszköz szimulálja.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-158">In this section, you create a Java console app that simulates a device.</span></span> <span data-ttu-id="7c1ec-159">hello app hello újraindítás közvetlen metódus hívását az IoT hub figyeli, és azonnal válaszol toothat hívás.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-159">hello app listens for hello reboot direct method call from your IoT hub and immediately responds toothat call.</span></span> <span data-ttu-id="7c1ec-160">alkalmazást, majd egy ideig alszik hello toosimulate hello újraindítás folyamat egy jelentett tulajdonság toonotify hello használata előtt **eseményindító-újraindítás** háttér-alkalmazást, amely hello újraindítás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-160">hello app then sleeps for a while toosimulate hello reboot process before it uses a reported property toonotify hello **trigger-reboot** back-end app that hello reboot is complete.</span></span>

1. <span data-ttu-id="7c1ec-161">A dm-get-started hello mappában nevű Maven-projekt létrehozása **szimulált eszköz** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-161">In hello dm-get-started folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="7c1ec-162">hello az alábbiakban olvashat egy egyetlen, hosszú parancsot:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-162">hello following is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="7c1ec-163">A parancssorban lépjen a toohello szimulált eszköz mappában.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-163">At your command prompt, navigate toohello simulated-device folder.</span></span>

1. <span data-ttu-id="7c1ec-164">Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello szimulált eszköz mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-164">Using a text editor, open hello pom.xml file in hello simulated-device folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="7c1ec-165">A függőség lehetővé teszi, hogy Ön toouse hello iot-szolgáltatás-ügyfélcsomagját a app toocommunicate az IoT hubbal:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-165">This dependency enables you toouse hello iot-service-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="7c1ec-166">Ellenőrizze, hogy hello legújabb verziójának **iot-eszközügyfél** használatával [Maven keresési][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="7c1ec-166">You can check for hello latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="7c1ec-167">Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-167">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="7c1ec-168">Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-168">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="7c1ec-169">Mentse és zárja be a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-169">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="7c1ec-170">Egy szövegszerkesztőben nyissa meg hello simulated-device\src\main\java\com\mycompany\app\App.java forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-170">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="7c1ec-171">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-171">Add hello following **import** statements toohello file:</span></span>

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

1. <span data-ttu-id="7c1ec-172">Adja hozzá a következő osztály változók toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-172">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="7c1ec-173">Cserélje le `{yourdeviceconnectionstring}` hello eszköz kapcsolati karakterlánccal hello feljegyzett *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-173">Replace `{yourdeviceconnectionstring}` with hello device connection string you noted in hello *Create a device identity* section:</span></span>

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. <span data-ttu-id="7c1ec-174">egy visszahívás-kezelő, a közvetlen módszer állapoteseményeit, tooimplement adja hozzá a hello következő beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-174">tooimplement a callback handler for direct method status events, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="7c1ec-175">egy visszahívás-kezelő eszköz iker állapoteseményeit, a tooimplement adja hozzá a hello következő beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-175">tooimplement a callback handler for device twin status events, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
        }
    }
    ```

1. <span data-ttu-id="7c1ec-176">egy visszahívás-kezelő tulajdonság események tooimplement adja hozzá a hello következő beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-176">tooimplement a callback handler for property events, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="7c1ec-177">tooimplement szál toosimulate hello eszköz újraindítás, adja hozzá a hello következő beágyazott osztály toohello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-177">tooimplement a thread toosimulate hello device reboot, add hello following nested class toohello **App** class.</span></span> <span data-ttu-id="7c1ec-178">hello szál öt másodpercenként alvó állapotba kerül, és ezután beállítja az hello **lastReboot** tulajdonság jelentette:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-178">hello thread sleeps for five seconds and then sets hello **lastReboot** reported property:</span></span>

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

1. <span data-ttu-id="7c1ec-179">tooimplement hello közvetlen módszer hello eszközön, adja hozzá a hello következő beágyazott osztály toohello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-179">tooimplement hello direct method on hello device, add hello following nested class toohello **App** class.</span></span> <span data-ttu-id="7c1ec-180">Ha hello szimulált app kap egy hívás toohello **újraindítás** közvetlen módszer adja vissza egy nyugtázási toohello hívó és majd elindul egy szál tooprocess hello újraindítás:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-180">When hello simulated app receives a call toohello **reboot** direct method, it returns an acknowledgement toohello caller and then starts a thread tooprocess hello reboot:</span></span>

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

1. <span data-ttu-id="7c1ec-181">Hello hello aláírása módosítása **fő** metódus toothrow hello kivételek a következő:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-181">Modify hello signature of hello **main** method toothrow hello following exceptions:</span></span>

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="7c1ec-182">tooinstantiate egy **DeviceClient**, adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-182">tooinstantiate a **DeviceClient**, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. <span data-ttu-id="7c1ec-183">figyeli a közvetlen metódushívások esetén toostart adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-183">toostart listening for direct method calls, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="7c1ec-184">tooshut le hello eszköz szimulátor, adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-184">tooshut down hello device simulator, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. <span data-ttu-id="7c1ec-185">Mentse és zárja be hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-185">Save and close hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="7c1ec-186">Build hello **szimulált eszköz** háttér-alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-186">Build hello **simulated-device** back-end app and correct any errors.</span></span> <span data-ttu-id="7c1ec-187">A parancssorban lépjen a toohello szimulált eszköz mappa és a következő parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-187">At your command prompt, navigate toohello simulated-device folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="7c1ec-188">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="7c1ec-188">Run hello apps</span></span>

<span data-ttu-id="7c1ec-189">Most már áll készen toorun hello alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="7c1ec-189">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="7c1ec-190">Parancsot egy parancssorba hello szimulált eszköz mappában futtassa a következő parancs toobegin újraindítás metódushívások az IoT-központ figyel hello:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-190">At a command prompt in hello simulated-device folder, run hello following command toobegin listening for reboot method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT Hub szimulált eszköz alkalmazás toolisten újraindítás közvetlen metódushívások][1]

1. <span data-ttu-id="7c1ec-192">Parancsot egy parancssorba hello eseményindító-újraindítás mappában futtassa a szimulált eszköz parancs toocall hello újraindítás metódus követően az IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-192">At a command prompt in hello trigger-reboot folder, run hello following command toocall hello reboot method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás app toocall hello indítsa újra a közvetlen módszer][2]

1. <span data-ttu-id="7c1ec-194">hello szimulált eszköz válaszol toohello újraindítás közvetlen metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="7c1ec-194">hello simulated device responds toohello reboot direct method call:</span></span>

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