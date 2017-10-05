---
title: "Ismerkedés az Azure IoT Hub eszköz twins (Java) |} Microsoft Docs"
description: "Hogyan használható az Azure IoT Hub eszköz twins címkéket, majd az IoT Hub-lekérdezést. Az Azure IoT-eszközök SDK Java segítségével valósítja meg az eszköz alkalmazás és az Azure IoT szolgáltatás Java SDK egy szolgáltatás-alkalmazást, amely hozzáadja a címkéket és az IoT Hub-lekérdezés futtatása végrehajtásához."
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
ms.openlocfilehash: 583cfcb9442c7be0a41aa1bc0f743bf8cf863665
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-device-twins-java"></a><span data-ttu-id="1b37c-104">Ismerkedés az eszköz twins (Java)</span><span class="sxs-lookup"><span data-stu-id="1b37c-104">Get started with device twins (Java)</span></span>

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="1b37c-105">Ebben az oktatóanyagban létrehoz két Java-konzol alkalmazásokhoz:</span><span class="sxs-lookup"><span data-stu-id="1b37c-105">In this tutorial, you create two Java console apps:</span></span>

* <span data-ttu-id="1b37c-106">**Adja hozzá címkék-lekérdezés**, a Java háttér-alkalmazást, amely címkét ad hozzá, és lekérdezi az eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="1b37c-106">**add-tags-query**, a Java back-end app that adds tags and queries device twins.</span></span>
* <span data-ttu-id="1b37c-107">**Szimulált eszköz**, egy Java eszköz-alkalmazást, amely kapcsolódik az IoT hub jelentést készít a kapcsolatát feltétel jelentett tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="1b37c-107">**simulated-device**, a Java device app that that connects to your IoT hub and reports its connectivity condition using a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="1b37c-108">A cikk [Azure IoT SDK-k](iot-hub-devguide-sdks.md) használható eszközt és a háttér-alkalmazások az Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="1b37c-108">The article [Azure IoT SDKs](iot-hub-devguide-sdks.md) provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>

<span data-ttu-id="1b37c-109">Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="1b37c-109">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="1b37c-110">A legújabb [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="1b37c-110">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="1b37c-111">Maven 3</span><span class="sxs-lookup"><span data-stu-id="1b37c-111">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="1b37c-112">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="1b37c-112">An active Azure account.</span></span> <span data-ttu-id="1b37c-113">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="1b37c-113">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="1b37c-114">Ha inkább hozzon létre az eszközidentitást programozott módon, olvassa el a megfelelő részt a [csatlakoztassa az eszközt az IoT hub Java használatával](iot-hub-java-java-getstarted.md#create-a-device-identity) cikk.</span><span class="sxs-lookup"><span data-stu-id="1b37c-114">If you prefer to create the device identity programmatically, read the corresponding section in the [Connect your device to your IoT hub using Java](iot-hub-java-java-getstarted.md#create-a-device-identity) article.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="1b37c-115">A service-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b37c-115">Create the service app</span></span>

<span data-ttu-id="1b37c-116">Ebben a szakaszban hoz létre egy Java-alkalmazást, amely hozzáadja a hely metaadatokat, az IoT-központ az eszköz a két címkét társított **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="1b37c-116">In this section, you create a Java app that adds location metadata as a tag to the device twin in IoT Hub associated with **myDeviceId**.</span></span> <span data-ttu-id="1b37c-117">Az alkalmazás első lekérdezi az IoT-központ az Egyesült Államokban lévő eszközök, majd az eszközök mobilhálózati kapcsolat hoznak.</span><span class="sxs-lookup"><span data-stu-id="1b37c-117">The app first queries IoT hub for devices located in the US, and then for devices that report a cellular network connection.</span></span>

1. <span data-ttu-id="1b37c-118">A fejlesztői gépen, hozzon létre egy üres nevű `iot-java-twin-getstarted`.</span><span class="sxs-lookup"><span data-stu-id="1b37c-118">On your development machine, create an empty folder called `iot-java-twin-getstarted`.</span></span>

1. <span data-ttu-id="1b37c-119">A a `iot-java-twin-getstarted` mappa, úgynevezett Maven-projekt létrehozása **hozzáadása-címkék-lekérdezés** parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="1b37c-119">In the `iot-java-twin-getstarted` folder, create a Maven project called **add-tags-query** using the following command at your command prompt.</span></span> <span data-ttu-id="1b37c-120">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="1b37c-120">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="1b37c-121">A parancssorban navigáljon a `add-tags-query` mappát.</span><span class="sxs-lookup"><span data-stu-id="1b37c-121">At your command prompt, navigate to the `add-tags-query` folder.</span></span>

1. <span data-ttu-id="1b37c-122">Egy szövegszerkesztőben nyissa meg a `pom.xml` fájlt a `add-tags-query` mappa, és adja hozzá a következő függőség a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="1b37c-122">Using a text editor, open the `pom.xml` file in the `add-tags-query` folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="1b37c-123">A függőség lehetővé teszi, hogy a **iot-szolgáltatásügyfél** csomag kommunikáljon az IoT hub az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="1b37c-123">This dependency enables you to use the **iot-service-client** package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="1b37c-124">Ellenőrizze, hogy a legújabb **iot-szolgáltatásügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="1b37c-124">You can check for the latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="1b37c-125">Adja hozzá a következő **build** csomópont után a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="1b37c-125">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="1b37c-126">Ez a konfiguráció arra utasítja a Java 1.8 az lehetővé teszi az alkalmazás Maven:</span><span class="sxs-lookup"><span data-stu-id="1b37c-126">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="1b37c-127">Mentse és zárja be a `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="1b37c-127">Save and close the `pom.xml` file.</span></span>

1. <span data-ttu-id="1b37c-128">Egy szövegszerkesztőben nyissa meg a `add-tags-query\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="1b37c-128">Using a text editor, open the `add-tags-query\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="1b37c-129">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="1b37c-129">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. <span data-ttu-id="1b37c-130">Adja hozzá a következő osztályszintű változókat az **App** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="1b37c-130">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="1b37c-131">Cserélje le `{youriothubconnectionstring}` az IoT hub kapcsolati karakterlánccal feljegyzett a *létrehoz egy IoT-központot* szakasz:</span><span class="sxs-lookup"><span data-stu-id="1b37c-131">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in the *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. <span data-ttu-id="1b37c-132">Frissítés a **fő** tartalmazza a következő metódus-aláírás `throws` záradékban:</span><span class="sxs-lookup"><span data-stu-id="1b37c-132">Update the **main** method signature to include the following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. <span data-ttu-id="1b37c-133">Adja hozzá a következő kódot a **fő** metódus létrehozása a **DeviceTwin** és **DeviceTwinDevice** objektumok.</span><span class="sxs-lookup"><span data-stu-id="1b37c-133">Add the following code to the **main** method to create the **DeviceTwin** and **DeviceTwinDevice** objects.</span></span> <span data-ttu-id="1b37c-134">A **DeviceTwin** objektum az IoT hub kommunikációt kezeli.</span><span class="sxs-lookup"><span data-stu-id="1b37c-134">The **DeviceTwin** object handles the communication with your IoT hub.</span></span> <span data-ttu-id="1b37c-135">A **DeviceTwinDevice** objektum által jelképezett tulajdonságok és címkék az eszköz iker:</span><span class="sxs-lookup"><span data-stu-id="1b37c-135">The **DeviceTwinDevice** object represents the device twin with its properties and tags:</span></span>

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. <span data-ttu-id="1b37c-136">Adja hozzá a következő `try/catch` megakadályozza a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="1b37c-136">Add the following `try/catch` block to the **main** method:</span></span>

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. <span data-ttu-id="1b37c-137">Frissítése a **régió** és **gépek** eszköz iker címkék az eszköz iker adja hozzá az alábbi kódot a a `try` letiltása:</span><span class="sxs-lookup"><span data-stu-id="1b37c-137">To update the **region** and **plant** device twin tags in your device twin, add the following code in the `try` block:</span></span>

    ```java
    // Get the device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from the existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create the tags and attach them to the DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update the device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve the device twin with the tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

1. <span data-ttu-id="1b37c-138">Az eszköz twins az IoT hubon lekérdezéséhez adja hozzá az alábbi kódot a `try` blokk az előző lépésben felvett kód után.</span><span class="sxs-lookup"><span data-stu-id="1b37c-138">To query the device twins in IoT hub, add the following code to the `try` block after the code you added in the previous step.</span></span> <span data-ttu-id="1b37c-139">A kód lefut két lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="1b37c-139">The code runs two queries.</span></span> <span data-ttu-id="1b37c-140">Minden egyes lekérdezés visszaadja a legfeljebb 100 eszközöket:</span><span class="sxs-lookup"><span data-stu-id="1b37c-140">Each query returns a maximum of 100 devices:</span></span>

    ```java
    // Query the device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct the query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run the query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct the query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run the query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

1. <span data-ttu-id="1b37c-141">Mentse és zárja be a `add-tags-query\src\main\java\com\mycompany\app\App.java` fájl</span><span class="sxs-lookup"><span data-stu-id="1b37c-141">Save and close the `add-tags-query\src\main\java\com\mycompany\app\App.java` file</span></span>

1. <span data-ttu-id="1b37c-142">Build a **hozzáadása-címkék-lekérdezés** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="1b37c-142">Build the **add-tags-query** app and correct any errors.</span></span> <span data-ttu-id="1b37c-143">A parancssorban navigáljon a `add-tags-query` mappa, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="1b37c-143">At your command prompt, navigate to the `add-tags-query` folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a><span data-ttu-id="1b37c-144">Eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b37c-144">Create a device app</span></span>

<span data-ttu-id="1b37c-145">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, amely beállítja a küldi el az IoT-központ jelentett tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="1b37c-145">In this section, you create a Java console app that sets a reported property value that is sent to IoT Hub.</span></span>

1. <span data-ttu-id="1b37c-146">Az a `iot-java-twin-getstarted` mappa, úgynevezett Maven-projekt létrehozása **szimulált eszköz** parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="1b37c-146">In the `iot-java-twin-getstarted` folder, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="1b37c-147">Látható, hogy ez egyetlen hosszú parancs:</span><span class="sxs-lookup"><span data-stu-id="1b37c-147">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="1b37c-148">A parancssorban navigáljon a `simulated-device` mappát.</span><span class="sxs-lookup"><span data-stu-id="1b37c-148">At your command prompt, navigate to the `simulated-device` folder.</span></span>

1. <span data-ttu-id="1b37c-149">Egy szövegszerkesztőben nyissa meg a `pom.xml` fájlt a `simulated-device` mappa, és adja hozzá az alábbi függőségeket a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="1b37c-149">Using a text editor, open the `pom.xml` file in the `simulated-device` folder and add the following dependencies to the **dependencies** node.</span></span> <span data-ttu-id="1b37c-150">A függőség lehetővé teszi, hogy a **iot-eszközügyfél** csomag kommunikáljon az IoT hub az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="1b37c-150">This dependency enables you to use the **iot-device-client** package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="1b37c-151">Ellenőrizze, hogy a legújabb **iot-eszközügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="1b37c-151">You can check for the latest version of **iot-device-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="1b37c-152">Adja hozzá a következő **build** csomópont után a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="1b37c-152">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="1b37c-153">Ez a konfiguráció arra utasítja a Java 1.8 az lehetővé teszi az alkalmazás Maven:</span><span class="sxs-lookup"><span data-stu-id="1b37c-153">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="1b37c-154">Mentse és zárja be a `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="1b37c-154">Save and close the `pom.xml` file.</span></span>

1. <span data-ttu-id="1b37c-155">Egy szövegszerkesztőben nyissa meg a `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="1b37c-155">Using a text editor, open the `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="1b37c-156">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="1b37c-156">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="1b37c-157">Adja hozzá a következő osztályszintű változókat az **App** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="1b37c-157">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="1b37c-158">A mag cseréje `{youriothubname}` az IoT hub nevű és `{yourdevicekey}` a eszközkulcs értékre hozott létre a *hozzon létre egy eszközidentitás* szakasz:</span><span class="sxs-lookup"><span data-stu-id="1b37c-158">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with the device key value you generated in the *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    <span data-ttu-id="1b37c-159">Ez a mintaalkalmazás a **protocol** változót használja egy **DeviceClient** objektum példányának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1b37c-159">This sample app uses the **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="1b37c-160">Jelenleg eszköz iker szolgáltatásait is használni kell használnia a MQTT protokoll.</span><span class="sxs-lookup"><span data-stu-id="1b37c-160">Currently, to use device twin features you must use the MQTT protocol.</span></span>

1. <span data-ttu-id="1b37c-161">Adja hozzá a következő kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="1b37c-161">Add the following code to the **main** method to:</span></span>
    * <span data-ttu-id="1b37c-162">Hozzon létre egy ügyfél-kommunikáljon az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="1b37c-162">Create a device client to communicate with IoT Hub.</span></span>
    * <span data-ttu-id="1b37c-163">Hozzon létre egy **eszköz** objektum iker eszköztulajdonságok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="1b37c-163">Create a **Device** object to store the device twin properties.</span></span>

    ```java
    DeviceClient client = new DeviceClient(connString, protocol);

    // Create a Device object to store the device twin properties
    Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed to " + propertyValue);
      }
    };
    ```

1. <span data-ttu-id="1b37c-164">Adja hozzá a következő kódot a **fő** metódussal hozzon létre egy **connectivityType** tulajdonság jelentette, és elküldi a IoT-központ:</span><span class="sxs-lookup"><span data-stu-id="1b37c-164">Add the following code to the **main** method to create a **connectivityType** reported property and send it to IoT Hub:</span></span>

    ```java
    try {
      // Open the DeviceClient and start the device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it to your IoT hub.
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

1. <span data-ttu-id="1b37c-165">Adja hozzá a következő kódot végén a **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="1b37c-165">Add the following code to the end of the **main** method.</span></span> <span data-ttu-id="1b37c-166">Várakozás a **Enter** kulcs lehetővé teszi, hogy az eszköz iker műveletek állapotának jelentésének IoT-központ számára:</span><span class="sxs-lookup"><span data-stu-id="1b37c-166">Waiting for the **Enter** key allows time for IoT Hub to report the status of the device twin operations:</span></span>

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. <span data-ttu-id="1b37c-167">Mentse és zárja be a `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="1b37c-167">Save and close the `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="1b37c-168">Build a **szimulált eszköz** alkalmazást, és kijavíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="1b37c-168">Build the **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="1b37c-169">A parancssorban navigáljon a `simulated-device` mappa, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="1b37c-169">At your command prompt, navigate to the `simulated-device` folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a><span data-ttu-id="1b37c-170">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="1b37c-170">Run the apps</span></span>

<span data-ttu-id="1b37c-171">Most már készen áll a konzol alkalmazások futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1b37c-171">You are now ready to run the console apps.</span></span>

1. <span data-ttu-id="1b37c-172">A parancsot a parancssorba a `add-tags-query` mappa futtatásához a következő parancsot a **hozzáadása-címkék-lekérdezés** service-alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="1b37c-172">At a command prompt in the `add-tags-query` folder, run the following command to run the **add-tags-query** service app:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás alkalmazás címke értékeinek frissítéséhez és eszköz-lekérdezések futtatása](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    <span data-ttu-id="1b37c-174">Megtekintheti a **gépek** és **régió** címkéket, az eszköz iker hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="1b37c-174">You can see the **plant** and **region** tags added to the device twin.</span></span> <span data-ttu-id="1b37c-175">Az első lekérdezés visszaadja az eszközt, de a második viszont nem.</span><span class="sxs-lookup"><span data-stu-id="1b37c-175">The first query returns your device, but the second does not.</span></span>

1. <span data-ttu-id="1b37c-176">A parancsot a parancssorba a `simulated-device` mappa, a következő parancs futtatásával adja hozzá a **connectivityType** jelentett tulajdonság az eszköz iker:</span><span class="sxs-lookup"><span data-stu-id="1b37c-176">At a command prompt in the `simulated-device` folder, run the following command to add the **connectivityType** reported property to the device twin:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Az eszközügyfél hozzáadja a ** connectivityType ** jelentett tulajdonság](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. <span data-ttu-id="1b37c-178">A parancsot a parancssorba a `add-tags-query` mappa futtatásához a következő parancsot a **hozzáadása-címkék-lekérdezés** service-alkalmazást még egyszer:</span><span class="sxs-lookup"><span data-stu-id="1b37c-178">At a command prompt in the `add-tags-query` folder, run the following command to run the **add-tags-query** service app a second time:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-központ szolgáltatás alkalmazás címke értékeinek frissítéséhez és eszköz-lekérdezések futtatása](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    <span data-ttu-id="1b37c-180">Miután elküldte az eszköz a **connectivityType** IoT-központ tulajdonságot, a második lekérdezés visszaadja az eszközt.</span><span class="sxs-lookup"><span data-stu-id="1b37c-180">Now your device has sent the **connectivityType** property to IoT Hub, the second query returns your device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b37c-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b37c-181">Next steps</span></span>

<span data-ttu-id="1b37c-182">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="1b37c-182">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="1b37c-183">Fel van véve eszköz metaadatait címkék egy háttér-alkalmazásból, és egy eszköz alkalmazásának megírt az eszköz a két jelentés eszköz kapcsolódási adatok.</span><span class="sxs-lookup"><span data-stu-id="1b37c-183">You added device metadata as tags from a back-end app, and wrote a device app to report device connectivity information in the device twin.</span></span> <span data-ttu-id="1b37c-184">Is megismerte az SQL-szerű IoT Hub lekérdezési nyelv használatával kettős eszközadatokat lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="1b37c-184">You also learned how to query the device twin information using the SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="1b37c-185">A következő források segítségével megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="1b37c-185">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="1b37c-186">Telemetriai adatokat küldhet az eszközökről a [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="1b37c-186">Send telemetry from devices with the [Get started with IoT Hub](iot-hub-java-java-getstarted.md) tutorial.</span></span>
* <span data-ttu-id="1b37c-187">Az interaktív (például egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása) eszközeinek vezérléséhez a [közvetlen módszerekkel](iot-hub-java-java-direct-methods.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="1b37c-187">Control devices interactively (such as turning on a fan from a user-controlled app) with the [Use direct methods](iot-hub-java-java-direct-methods.md) tutorial.</span></span>

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
