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
# <a name="use-direct-methods-java"></a><span data-ttu-id="09ef5-104">Közvetlen módszerekkel (Java)</span><span class="sxs-lookup"><span data-stu-id="09ef5-104">Use direct methods (Java)</span></span>

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="09ef5-105">Ebben az oktatóanyagban létrehoz két Java-konzol alkalmazásokhoz:</span><span class="sxs-lookup"><span data-stu-id="09ef5-105">In this tutorial, you create two Java console apps:</span></span>

* <span data-ttu-id="09ef5-106">**Invoke-közvetlen-method**, a Java háttér-alkalmazást, amely metódus meghívja a szimulált eszköz app hello és hello válasz jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="09ef5-106">**invoke-direct-method**, a Java back-end app that calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="09ef5-107">**Szimulált eszköz**, szimulálja egy eszközt az IoT-központ tooyour összekapcsolása hello eszközidentitás hoz létre egy Java-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="09ef5-107">**simulated-device**, a Java app that simulates a device connecting tooyour IoT hub with hello device identity you create.</span></span> <span data-ttu-id="09ef5-108">Ez az alkalmazás közvetlen meghívása hello háttérből toohello válaszol.</span><span class="sxs-lookup"><span data-stu-id="09ef5-108">This app responds toohello direct invoked from hello back end.</span></span>

> [!NOTE]
> <span data-ttu-id="09ef5-109">Használható toobuild alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello SDK-kkal kapcsolatos információk: [Azure IoT SDK-k][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="09ef5-109">For information about hello SDKs that you can use toobuild applications toorun on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="09ef5-110">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="09ef5-110">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="09ef5-111">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="09ef5-111">Java SE 8.</span></span> <br/> <span data-ttu-id="09ef5-112">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall Java ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="09ef5-112">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="09ef5-113">Maven 3</span><span class="sxs-lookup"><span data-stu-id="09ef5-113">Maven 3.</span></span>  <br/> <span data-ttu-id="09ef5-114">[A fejlesztőkörnyezet előkészítése] [ lnk-dev-setup] ismerteti, hogyan tooinstall [Maven] [ lnk-maven] ebben az oktatóanyagban a Windows vagy Linux.</span><span class="sxs-lookup"><span data-stu-id="09ef5-114">[Prepare your development environment][lnk-dev-setup] describes how tooinstall [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="09ef5-115">[NODE.js-verzió 0.10.0-s vagy újabb](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="09ef5-115">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="09ef5-116">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="09ef5-116">Create a simulated device app</span></span>

<span data-ttu-id="09ef5-117">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely válaszol a hello megoldás háttérrendszere által meghívott end tooa metódust.</span><span class="sxs-lookup"><span data-stu-id="09ef5-117">In this section, you create a Java console app that responds tooa method called by hello solution back end.</span></span>

1. <span data-ttu-id="09ef5-118">Hozzon létre egy üres nevű iot-java-közvetlen-metódus.</span><span class="sxs-lookup"><span data-stu-id="09ef5-118">Create an empty folder called iot-java-direct-method.</span></span>

1. <span data-ttu-id="09ef5-119">Hello iot-java-közvetlen-metódus mappában hozzon létre egy Maven project nevű **szimulált eszköz** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="09ef5-119">In hello iot-java-direct-method folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="09ef5-120">a következő parancs hello egyetlen, hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="09ef5-120">hello following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="09ef5-121">A parancssorban lépjen a toohello szimulált eszköz mappában.</span><span class="sxs-lookup"><span data-stu-id="09ef5-121">At your command prompt, navigate toohello simulated-device folder.</span></span>

1. <span data-ttu-id="09ef5-122">Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello szimulált eszköz mappában, és adja hozzá a következő függőségek toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="09ef5-122">Using a text editor, open hello pom.xml file in hello simulated-device folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="09ef5-123">A függőség lehetővé teszi, hogy Ön toouse hello iot-eszközök-ügyfélcsomagját az alkalmazás toocommunicate az IoT hubbal:</span><span class="sxs-lookup"><span data-stu-id="09ef5-123">This dependency enables you toouse hello iot-device-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="09ef5-124">Ellenőrizze, hogy hello legújabb verziójának **iot-eszközügyfél** használatával [Maven keresési][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="09ef5-124">You can check for hello latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="09ef5-125">Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="09ef5-125">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="09ef5-126">Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="09ef5-126">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="09ef5-127">Mentse és zárja be a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="09ef5-127">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="09ef5-128">Egy szövegszerkesztőben nyissa meg hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="09ef5-128">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="09ef5-129">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="09ef5-129">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="09ef5-130">Adja hozzá a következő osztály változók toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="09ef5-130">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="09ef5-131">A mag cseréje `{youriothubname}` az IoT hub nevű és `{yourdevicekey}` hello eszköz kulcsérték hello hozta létre a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="09ef5-131">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="09ef5-132">Ezen PéldaAlkalmazás használ hello **protokoll** változó, amikor elindítja a **DeviceClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="09ef5-132">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="09ef5-133">Jelenleg toouse közvetlen módszerek hello MQTT protokollt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="09ef5-133">Currently, toouse direct methods you must use hello MQTT protocol.</span></span>

1. <span data-ttu-id="09ef5-134">tooreturn állapot kód tooyour IoT hubot, adja hozzá a hello következő beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="09ef5-134">tooreturn a status code tooyour IoT hub, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="09ef5-135">toohandle hello közvetlen módszer meghívásához hello megoldás háttérből, adja hozzá a hello következő beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="09ef5-135">toohandle hello direct method invocations from hello solution back end, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="09ef5-136">toocreate egy **DeviceClient** és a közvetlen módszer meghívásához figyelésére, vegye fel a **fő** metódus toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="09ef5-136">toocreate a **DeviceClient** and listen for direct method invocations, add a **main** method toohello **App** class:</span></span>

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

1. <span data-ttu-id="09ef5-137">Mentse és zárja be a hello simulated-device\src\main\java\com\mycompany\app\App.java fájl</span><span class="sxs-lookup"><span data-stu-id="09ef5-137">Save and close hello simulated-device\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="09ef5-138">Build hello **szimulált eszköz** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="09ef5-138">Build hello **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="09ef5-139">A parancssorban lépjen a toohello szimulált eszköz mappa és a következő parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="09ef5-139">At your command prompt, navigate toohello simulated-device folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="09ef5-140">A közvetlen metódus hívása az eszközön</span><span class="sxs-lookup"><span data-stu-id="09ef5-140">Call a direct method on a device</span></span>

<span data-ttu-id="09ef5-141">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely meghívja a közvetlen módszer és hello válasz megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="09ef5-141">In this section, you create a Java console app that invokes a direct method and then displays hello response.</span></span> <span data-ttu-id="09ef5-142">A Konzolalkalmazás tooyour IoT-központ tooinvoke hello közvetlen módszer csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="09ef5-142">This console app connects tooyour IoT Hub tooinvoke hello direct method.</span></span>

1. <span data-ttu-id="09ef5-143">Hello iot-java-közvetlen-metódus mappában hozzon létre egy Maven project nevű **meghívása közvetlen-metódus** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="09ef5-143">In hello iot-java-direct-method folder, create a Maven project called **invoke-direct-method** using hello following command at your command prompt.</span></span> <span data-ttu-id="09ef5-144">a következő parancs hello egyetlen, hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="09ef5-144">hello following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="09ef5-145">A parancssorba keresse meg a toohello meghívása közvetlen-metódus mappa.</span><span class="sxs-lookup"><span data-stu-id="09ef5-145">At your command prompt, navigate toohello invoke-direct-method folder.</span></span>

1. <span data-ttu-id="09ef5-146">Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello meghívása közvetlen-metódus mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="09ef5-146">Using a text editor, open hello pom.xml file in hello invoke-direct-method folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="09ef5-147">A függőség lehetővé teszi, hogy Ön toouse hello iot-szolgáltatás-ügyfélcsomagját a app toocommunicate az IoT hubbal:</span><span class="sxs-lookup"><span data-stu-id="09ef5-147">This dependency enables you toouse hello iot-service-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="09ef5-148">Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="09ef5-148">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="09ef5-149">Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="09ef5-149">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="09ef5-150">Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="09ef5-150">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="09ef5-151">Mentse és zárja be a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="09ef5-151">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="09ef5-152">Egy szövegszerkesztőben nyissa meg hello invoke-direct-method\src\main\java\com\mycompany\app\App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="09ef5-152">Using a text editor, open hello invoke-direct-method\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="09ef5-153">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="09ef5-153">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. <span data-ttu-id="09ef5-154">Adja hozzá a következő osztály változók toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="09ef5-154">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="09ef5-155">Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal hello feljegyzett *létrehoz egy IoT-központot* szakasz:</span><span class="sxs-lookup"><span data-stu-id="09ef5-155">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line toobe written";
    ```

1. <span data-ttu-id="09ef5-156">tooinvoke hello metódus hello szimulált eszköz, adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="09ef5-156">tooinvoke hello method on hello simulated device, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="09ef5-157">Mentse és zárja be a hello invoke-direct-method\src\main\java\com\mycompany\app\App.java fájl</span><span class="sxs-lookup"><span data-stu-id="09ef5-157">Save and close hello invoke-direct-method\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="09ef5-158">Build hello **meghívása közvetlen-metódus** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="09ef5-158">Build hello **invoke-direct-method** app and correct any errors.</span></span> <span data-ttu-id="09ef5-159">A parancssorban keresse meg a toohello meghívása közvetlen-metódus mappa és a következő parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="09ef5-159">At your command prompt, navigate toohello invoke-direct-method folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="09ef5-160">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="09ef5-160">Run hello apps</span></span>

<span data-ttu-id="09ef5-161">Most már áll készen toorun hello konzol alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="09ef5-161">You are now ready toorun hello console apps.</span></span>

1. <span data-ttu-id="09ef5-162">Parancsot egy parancssorba hello szimulált eszköz mappában futtassa a következő parancs toobegin metódushívások az IoT-központ figyel hello:</span><span class="sxs-lookup"><span data-stu-id="09ef5-162">At a command prompt in hello simulated-device folder, run hello following command toobegin listening for method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT Hub szimulált eszköz alkalmazás toolisten közvetlen metódushívások][8]

1. <span data-ttu-id="09ef5-164">A parancssorba hello meghívása közvetlen-metódus mappában futtassa a következő parancs toocall hello metódus a szimulált eszköz az IoT hub:</span><span class="sxs-lookup"><span data-stu-id="09ef5-164">At a command prompt in hello invoke-direct-method folder, run hello following command toocall a method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás app toocall közvetlen módszer][7]

1. <span data-ttu-id="09ef5-166">hello szimulált eszköz válaszol toohello közvetlen metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="09ef5-166">hello simulated device responds toohello direct method call:</span></span>

    ![Java IoT Hub szimulált eszköz alkalmazásának válaszol toohello közvetlen metódus hívása][9]

## <a name="next-steps"></a><span data-ttu-id="09ef5-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09ef5-168">Next steps</span></span>

<span data-ttu-id="09ef5-169">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="09ef5-169">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="09ef5-170">Az eszköz identitás tooenable hello szimulált eszköz alkalmazás tooreact toomethods hello felhő által meghívott használta.</span><span class="sxs-lookup"><span data-stu-id="09ef5-170">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="09ef5-171">Is létrehozott egy alkalmazást, amely hello eszközön módszereket hív, és megjeleníti hello válasz hello eszközről.</span><span class="sxs-lookup"><span data-stu-id="09ef5-171">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span>

<span data-ttu-id="09ef5-172">tooexplore más IoT-forgatókönyvek esetén, lásd: [több eszközön feladatok ütemezése][lnk-devguide-jobs].</span><span class="sxs-lookup"><span data-stu-id="09ef5-172">tooexplore other IoT scenarios, see [Schedule jobs on multiple devices][lnk-devguide-jobs].</span></span>

<span data-ttu-id="09ef5-173">toolearn hogyan tooextend az IoT megoldás és az ütemezések metódus meghívja a több eszközön, tekintse meg a hello [ütemezés és a szórásos feladatok] [ lnk-tutorial-jobs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="09ef5-173">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
