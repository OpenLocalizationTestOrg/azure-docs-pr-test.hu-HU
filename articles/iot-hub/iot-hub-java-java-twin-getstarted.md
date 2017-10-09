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
# <a name="get-started-with-device-twins-java"></a><span data-ttu-id="ee076-104">Ismerkedés az eszköz twins (Java)</span><span class="sxs-lookup"><span data-stu-id="ee076-104">Get started with device twins (Java)</span></span>

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="ee076-105">Ebben az oktatóanyagban létrehoz két Java-konzol alkalmazásokhoz:</span><span class="sxs-lookup"><span data-stu-id="ee076-105">In this tutorial, you create two Java console apps:</span></span>

* <span data-ttu-id="ee076-106">**Adja hozzá címkék-lekérdezés**, a Java háttér-alkalmazást, amely címkét ad hozzá, és lekérdezi az eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="ee076-106">**add-tags-query**, a Java back-end app that adds tags and queries device twins.</span></span>
* <span data-ttu-id="ee076-107">**Szimulált eszköz**, a Java eszközalkalmazás, hogy, hogy kapcsolatot tooyour IoT hub és a jelentések jelentett tulajdonsággal kapcsolat állapotát.</span><span class="sxs-lookup"><span data-stu-id="ee076-107">**simulated-device**, a Java device app that that connects tooyour IoT hub and reports its connectivity condition using a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="ee076-108">hello cikk [Azure IoT SDK-k](iot-hub-devguide-sdks.md) információkat nyújt azokról hello Azure IoT SDK-k toobuild használt eszköz és a háttér-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="ee076-108">hello article [Azure IoT SDKs](iot-hub-devguide-sdks.md) provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>

<span data-ttu-id="ee076-109">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="ee076-109">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="ee076-110">hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="ee076-110">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="ee076-111">Maven 3</span><span class="sxs-lookup"><span data-stu-id="ee076-111">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="ee076-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="ee076-112">An active Azure account.</span></span> <span data-ttu-id="ee076-113">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="ee076-113">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="ee076-114">Toocreate hello eszközidentitás programozott módon tetszés szerint olvassa el a megfelelő szakasz hello hello [csatlakozni az eszköz tooyour IoT hub Java használatával](iot-hub-java-java-getstarted.md#create-a-device-identity) cikk.</span><span class="sxs-lookup"><span data-stu-id="ee076-114">If you prefer toocreate hello device identity programmatically, read hello corresponding section in hello [Connect your device tooyour IoT hub using Java](iot-hub-java-java-getstarted.md#create-a-device-identity) article.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="ee076-115">Hello service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee076-115">Create hello service app</span></span>

<span data-ttu-id="ee076-116">Ebben a szakaszban hoz létre egy Java-alkalmazást, amely hozzáadja a hely metaadatokat egy címke toohello eszköz iker az IoT-központ kapcsolódó **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="ee076-116">In this section, you create a Java app that adds location metadata as a tag toohello device twin in IoT Hub associated with **myDeviceId**.</span></span> <span data-ttu-id="ee076-117">hello app először lekérdezi az IoT-központ hello USA található eszközök, majd jelentést mobilhálózati kapcsolat eszközök esetében.</span><span class="sxs-lookup"><span data-stu-id="ee076-117">hello app first queries IoT hub for devices located in hello US, and then for devices that report a cellular network connection.</span></span>

1. <span data-ttu-id="ee076-118">A fejlesztői gépen, hozzon létre egy üres nevű `iot-java-twin-getstarted`.</span><span class="sxs-lookup"><span data-stu-id="ee076-118">On your development machine, create an empty folder called `iot-java-twin-getstarted`.</span></span>

1. <span data-ttu-id="ee076-119">A hello `iot-java-twin-getstarted` mappa, nevű Maven-projekt létrehozása **hozzáadása-címkék-lekérdezés** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="ee076-119">In hello `iot-java-twin-getstarted` folder, create a Maven project called **add-tags-query** using hello following command at your command prompt.</span></span> <span data-ttu-id="ee076-120">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="ee076-120">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="ee076-121">A parancssorban lépjen a toohello `add-tags-query` mappa.</span><span class="sxs-lookup"><span data-stu-id="ee076-121">At your command prompt, navigate toohello `add-tags-query` folder.</span></span>

1. <span data-ttu-id="ee076-122">Egy szövegszerkesztőben nyissa meg hello `pom.xml` hello fájlban `add-tags-query` mappa, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ee076-122">Using a text editor, open hello `pom.xml` file in hello `add-tags-query` folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="ee076-123">A függőség lehetővé teszi a toouse hello **iot-szolgáltatásügyfél** az alkalmazás toocommunicate és az IoT hub-csomagot:</span><span class="sxs-lookup"><span data-stu-id="ee076-123">This dependency enables you toouse hello **iot-service-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="ee076-124">Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="ee076-124">You can check for hello latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="ee076-125">Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ee076-125">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="ee076-126">Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="ee076-126">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="ee076-127">Mentse és zárja be a hello `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ee076-127">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="ee076-128">Egy szövegszerkesztőben nyissa meg hello `add-tags-query\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ee076-128">Using a text editor, open hello `add-tags-query\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="ee076-129">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="ee076-129">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. <span data-ttu-id="ee076-130">Adja hozzá a következő osztály változók toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="ee076-130">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="ee076-131">Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal hello feljegyzett *létrehoz egy IoT-központot* szakasz:</span><span class="sxs-lookup"><span data-stu-id="ee076-131">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. <span data-ttu-id="ee076-132">Frissítés hello **fő** metódus aláírása tooinclude hello következő `throws` záradékban:</span><span class="sxs-lookup"><span data-stu-id="ee076-132">Update hello **main** method signature tooinclude hello following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. <span data-ttu-id="ee076-133">Adja hozzá a következő kód toohello hello **fő** metódus toocreate hello **DeviceTwin** és **DeviceTwinDevice** objektumok.</span><span class="sxs-lookup"><span data-stu-id="ee076-133">Add hello following code toohello **main** method toocreate hello **DeviceTwin** and **DeviceTwinDevice** objects.</span></span> <span data-ttu-id="ee076-134">Hello **DeviceTwin** objektum az IoT hub hello kommunikációt kezeli.</span><span class="sxs-lookup"><span data-stu-id="ee076-134">hello **DeviceTwin** object handles hello communication with your IoT hub.</span></span> <span data-ttu-id="ee076-135">Hello **DeviceTwinDevice** objektum által jelképezett hello eszköz iker tulajdonságok és címkék:</span><span class="sxs-lookup"><span data-stu-id="ee076-135">hello **DeviceTwinDevice** object represents hello device twin with its properties and tags:</span></span>

    ```java
    // Get hello DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. <span data-ttu-id="ee076-136">Adja hozzá a következő hello `try/catch` toohello blokkolása **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="ee076-136">Add hello following `try/catch` block toohello **main** method:</span></span>

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. <span data-ttu-id="ee076-137">tooupdate hello **régió** és **gépek** az eszköz iker eszköz iker címkék hozzáadása a következő kódot a hello hello `try` letiltása:</span><span class="sxs-lookup"><span data-stu-id="ee076-137">tooupdate hello **region** and **plant** device twin tags in your device twin, add hello following code in hello `try` block:</span></span>

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

1. <span data-ttu-id="ee076-138">tooquery hello eszköz twins az IoT-központot, adja hozzá a következő kód toohello hello `try` blokk hello előző lépésben felvett hello kód után.</span><span class="sxs-lookup"><span data-stu-id="ee076-138">tooquery hello device twins in IoT hub, add hello following code toohello `try` block after hello code you added in hello previous step.</span></span> <span data-ttu-id="ee076-139">hello kódjának futtatásához két lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="ee076-139">hello code runs two queries.</span></span> <span data-ttu-id="ee076-140">Minden egyes lekérdezés visszaadja a legfeljebb 100 eszközöket:</span><span class="sxs-lookup"><span data-stu-id="ee076-140">Each query returns a maximum of 100 devices:</span></span>

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

1. <span data-ttu-id="ee076-141">Mentse és zárja be a hello `add-tags-query\src\main\java\com\mycompany\app\App.java` fájl</span><span class="sxs-lookup"><span data-stu-id="ee076-141">Save and close hello `add-tags-query\src\main\java\com\mycompany\app\App.java` file</span></span>

1. <span data-ttu-id="ee076-142">Build hello **hozzáadása-címkék-lekérdezés** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="ee076-142">Build hello **add-tags-query** app and correct any errors.</span></span> <span data-ttu-id="ee076-143">A parancssorban lépjen a toohello `add-tags-query` mappa és a következő parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="ee076-143">At your command prompt, navigate toohello `add-tags-query` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a><span data-ttu-id="ee076-144">Eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee076-144">Create a device app</span></span>

<span data-ttu-id="ee076-145">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely beállítja a központ tooIoT küldött jelentett tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="ee076-145">In this section, you create a Java console app that sets a reported property value that is sent tooIoT Hub.</span></span>

1. <span data-ttu-id="ee076-146">A hello `iot-java-twin-getstarted` mappa, úgynevezett Maven-projekt létrehozása **szimulált eszköz** használatával a következő parancsot a parancssorba hello.</span><span class="sxs-lookup"><span data-stu-id="ee076-146">In hello `iot-java-twin-getstarted` folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="ee076-147">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="ee076-147">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="ee076-148">A parancssorban lépjen a toohello `simulated-device` mappa.</span><span class="sxs-lookup"><span data-stu-id="ee076-148">At your command prompt, navigate toohello `simulated-device` folder.</span></span>

1. <span data-ttu-id="ee076-149">Egy szövegszerkesztőben nyissa meg hello `pom.xml` hello fájlban `simulated-device` mappa, és adja hozzá a következő függőségek toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ee076-149">Using a text editor, open hello `pom.xml` file in hello `simulated-device` folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="ee076-150">A függőség lehetővé teszi a toouse hello **iot-eszközügyfél** az alkalmazás toocommunicate és az IoT hub-csomagot:</span><span class="sxs-lookup"><span data-stu-id="ee076-150">This dependency enables you toouse hello **iot-device-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="ee076-151">Ellenőrizze, hogy hello legújabb verziójának **iot-eszközügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="ee076-151">You can check for hello latest version of **iot-device-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="ee076-152">Adja hozzá a következő hello **build** után hello csomópont **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ee076-152">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="ee076-153">Ez a konfiguráció arra utasítja a Maven toouse Java 1,8 toobuild hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="ee076-153">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="ee076-154">Mentse és zárja be a hello `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ee076-154">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="ee076-155">Egy szövegszerkesztőben nyissa meg hello `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ee076-155">Using a text editor, open hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="ee076-156">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="ee076-156">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="ee076-157">Adja hozzá a következő osztály változók toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="ee076-157">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="ee076-158">A mag cseréje `{youriothubname}` az IoT hub nevű és `{yourdevicekey}` hello eszköz kulcsérték hello hozta létre a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="ee076-158">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    <span data-ttu-id="ee076-159">Ezen PéldaAlkalmazás használ hello **protokoll** változó, amikor elindítja a **DeviceClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="ee076-159">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="ee076-160">Jelenleg toouse eszközök iker szolgáltatásainak hello MQTT protokollt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="ee076-160">Currently, toouse device twin features you must use hello MQTT protocol.</span></span>

1. <span data-ttu-id="ee076-161">Adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="ee076-161">Add hello following code toohello **main** method to:</span></span>
    * <span data-ttu-id="ee076-162">Hozzon létre egy eszköz ügyfél toocommunicate IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="ee076-162">Create a device client toocommunicate with IoT Hub.</span></span>
    * <span data-ttu-id="ee076-163">Hozzon létre egy **eszköz** toostore hello eszköztulajdonságok a két objektum.</span><span class="sxs-lookup"><span data-stu-id="ee076-163">Create a **Device** object toostore hello device twin properties.</span></span>

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

1. <span data-ttu-id="ee076-164">Adja hozzá a következő kód toohello hello **fő** metódus toocreate egy **connectivityType** tulajdonság jelentette, és elküldi a tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="ee076-164">Add hello following code toohello **main** method toocreate a **connectivityType** reported property and send it tooIoT Hub:</span></span>

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

1. <span data-ttu-id="ee076-165">Adja hozzá a következő kód toohello vége hello hello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="ee076-165">Add hello following code toohello end of hello **main** method.</span></span> <span data-ttu-id="ee076-166">Várakozás a hello **Enter** kulcs lehetővé teszi, hogy az IoT-központ tooreport hello hello eszköz iker műveletek állapotának ideje:</span><span class="sxs-lookup"><span data-stu-id="ee076-166">Waiting for hello **Enter** key allows time for IoT Hub tooreport hello status of hello device twin operations:</span></span>

    ```java
    System.out.println("Press any key tooexit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. <span data-ttu-id="ee076-167">Mentse és zárja be a hello `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="ee076-167">Save and close hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="ee076-168">Build hello **szimulált eszköz** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="ee076-168">Build hello **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="ee076-169">A parancssorban lépjen a toohello `simulated-device` mappa és a következő parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="ee076-169">At your command prompt, navigate toohello `simulated-device` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="ee076-170">Hello alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="ee076-170">Run hello apps</span></span>

<span data-ttu-id="ee076-171">Most már áll készen toorun hello konzol alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="ee076-171">You are now ready toorun hello console apps.</span></span>

1. <span data-ttu-id="ee076-172">A hello parancsot a parancssorba `add-tags-query` mappa, futtassa a következő parancs toorun hello hello **hozzáadása-címkék-lekérdezés** service-alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="ee076-172">At a command prompt in hello `add-tags-query` folder, run hello following command toorun hello **add-tags-query** service app:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás app tooupdate értékek címkét, és eszköz-lekérdezések futtatása](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    <span data-ttu-id="ee076-174">Megtekintheti a hello **gépek** és **régió** címkék hozzáadott toohello eszköz iker.</span><span class="sxs-lookup"><span data-stu-id="ee076-174">You can see hello **plant** and **region** tags added toohello device twin.</span></span> <span data-ttu-id="ee076-175">hello első lekérdezés visszaadja az eszközt, de hello második viszont nem.</span><span class="sxs-lookup"><span data-stu-id="ee076-175">hello first query returns your device, but hello second does not.</span></span>

1. <span data-ttu-id="ee076-176">A hello parancsot a parancssorba `simulated-device` mappa, futtassa a következő parancs tooadd hello hello **connectivityType** tulajdonság toohello eszköz iker jelentette:</span><span class="sxs-lookup"><span data-stu-id="ee076-176">At a command prompt in hello `simulated-device` folder, run hello following command tooadd hello **connectivityType** reported property toohello device twin:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![hello eszköz client ad hello ** connectivityType ** jelentett tulajdonság](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. <span data-ttu-id="ee076-178">A hello parancsot a parancssorba `add-tags-query` mappa, futtassa a következő parancs toorun hello hello **hozzáadása-címkék-lekérdezés** service-alkalmazást még egyszer:</span><span class="sxs-lookup"><span data-stu-id="ee076-178">At a command prompt in hello `add-tags-query` folder, run hello following command toorun hello **add-tags-query** service app a second time:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás app tooupdate értékek címkét, és eszköz-lekérdezések futtatása](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    <span data-ttu-id="ee076-180">Miután az eszköz küldött hello **connectivityType** tulajdonság tooIoT Hub, hello második lekérdezés visszaadja az eszközt.</span><span class="sxs-lookup"><span data-stu-id="ee076-180">Now your device has sent hello **connectivityType** property tooIoT Hub, hello second query returns your device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee076-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee076-181">Next steps</span></span>

<span data-ttu-id="ee076-182">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="ee076-182">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="ee076-183">Eszköz metaadatait címkeként felvett egy háttér-alkalmazást, és egy eszköz kapcsolat app tooreport eszközadatokat megírt hello eszköz iker a.</span><span class="sxs-lookup"><span data-stu-id="ee076-183">You added device metadata as tags from a back-end app, and wrote a device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="ee076-184">Is megtanulta, hogyan tooquery hello hello SQL-szerű IoT Hub lekérdezési nyelv használatával kettős eszközadatokat.</span><span class="sxs-lookup"><span data-stu-id="ee076-184">You also learned how tooquery hello device twin information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="ee076-185">A következő erőforrások toolearn hogyan használja hello számára:</span><span class="sxs-lookup"><span data-stu-id="ee076-185">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="ee076-186">Telemetriai adatokat küldhet a hello eszközökről [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="ee076-186">Send telemetry from devices with hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) tutorial.</span></span>
* <span data-ttu-id="ee076-187">Interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a hello [közvetlen módszerekkel](iot-hub-java-java-direct-methods.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="ee076-187">Control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods](iot-hub-java-java-direct-methods.md) tutorial.</span></span>

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
