---
title: "Az Azure IoT Hub Java eszközökről fájlok feltöltése |} Microsoft Docs"
description: "Hogyan tölt fel az eszközről a felhőbe Javához készült Azure IoT-eszközök SDK használatával. Egy Azure blob tároló feltöltött fájlok tárolják."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: c917a3b3e16f1e84f202d6c87a04faf642266701
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a><span data-ttu-id="9e93a-104">Az eszközről a felhőbe, IoT-központ fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="9e93a-104">Upload files from your device to the cloud with IoT Hub</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="9e93a-105">Ez az oktatóanyag épít, a kód a [IoT Hub-felhő eszközre üzenetek](iot-hub-java-java-c2d.md) mutatjuk be, hogyan használható az oktatóanyag a [fájl feltöltése képességeit az IoT-központ](iot-hub-devguide-file-upload.md) feltölteni a fájlt [Azure blob tárolási](../storage/index.md).</span><span class="sxs-lookup"><span data-stu-id="9e93a-105">This tutorial builds on the code in the [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorial to show you how to use the [file upload capabilities of IoT Hub](iot-hub-devguide-file-upload.md) to upload a file to [Azure blob storage](../storage/index.md).</span></span> <span data-ttu-id="9e93a-106">Az oktatóanyag bemutatja, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="9e93a-106">The tutorial shows you how to:</span></span>

- <span data-ttu-id="9e93a-107">Biztonságosan adjon meg egy eszközt az Azure blob URI-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="9e93a-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="9e93a-108">Az IoT-központ fájl feltöltése értesítések használatával indul el, az alkalmazás háttérbeli fájl feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="9e93a-108">Use the IoT Hub file upload notifications to trigger processing the file in your app back end.</span></span>

<span data-ttu-id="9e93a-109">A [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) és [IoT Hub-felhő eszközre üzenetek](iot-hub-java-java-c2d.md) oktatóanyagok alapvető eszköz-felhő és a felhő eszközre üzenetkezelési funkcióit az IoT-központ megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="9e93a-109">The [Get started with IoT Hub](iot-hub-java-java-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorials show the basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="9e93a-110">A [folyamat eszköz felhőbe küldött üzeneteket](iot-hub-java-java-process-d2c.md) az oktatóanyag leírja, hogy megbízhatóan tárolja az eszköz a felhőbe küldött üzeneteket az Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="9e93a-110">The [Process Device-to-Cloud messages](iot-hub-java-java-process-d2c.md) tutorial describes a way to reliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="9e93a-111">Bizonyos esetekben azonban leképezése nem az eszközök elküldik üzenetbe a viszonylag kis eszközről a felhőbe, amely az IoT-központ fogadja az adatokat könnyen.</span><span class="sxs-lookup"><span data-stu-id="9e93a-111">However, in some scenarios you cannot easily map the data your devices send into the relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="9e93a-112">Példa:</span><span class="sxs-lookup"><span data-stu-id="9e93a-112">For example:</span></span>

* <span data-ttu-id="9e93a-113">Nagy fájlok, amelyek képeket</span><span class="sxs-lookup"><span data-stu-id="9e93a-113">Large files that contain images</span></span>
* <span data-ttu-id="9e93a-114">Videók</span><span class="sxs-lookup"><span data-stu-id="9e93a-114">Videos</span></span>
* <span data-ttu-id="9e93a-115">Nagy gyakoriságú lekérdező vibráció adatok</span><span class="sxs-lookup"><span data-stu-id="9e93a-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="9e93a-116">Valamilyen előre feldolgozott adatokat.</span><span class="sxs-lookup"><span data-stu-id="9e93a-116">Some form of preprocessed data.</span></span>

<span data-ttu-id="9e93a-117">Ezek a fájlok jellemzően eszközökkel, mint a felhőben feldolgozható [Azure Data Factory](../data-factory/index.md) vagy a [Hadoop](../hdinsight/index.md) verem.</span><span class="sxs-lookup"><span data-stu-id="9e93a-117">These files are typically batch processed in the cloud using tools such as [Azure Data Factory](../data-factory/index.md) or the [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="9e93a-118">Ha egy eszközről kell felvidéki fájlok, a biztonsága és megbízhatósága szempontjából az IoT-központ továbbra is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9e93a-118">When you need to upland files from a device, you can still use the security and reliability of IoT Hub.</span></span>

<span data-ttu-id="9e93a-119">Ez az oktatóanyag végén két Java konzol alkalmazások futtatása:</span><span class="sxs-lookup"><span data-stu-id="9e93a-119">At the end of this tutorial you run two Java console apps:</span></span>

* <span data-ttu-id="9e93a-120">**Szimulált eszköz**, az alkalmazás létrehozása az [IoT hubbal küldési felhőből eszközre küldött üzenetek] oktatóanyag módosított változatát.</span><span class="sxs-lookup"><span data-stu-id="9e93a-120">**simulated-device**, a modified version of the app created in the [Send Cloud-to-Device messages with IoT Hub] tutorial.</span></span> <span data-ttu-id="9e93a-121">Ez az alkalmazás az IoT hub által biztosított SAS URI-k használata tárolási feltölt egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="9e93a-121">This app uploads a file to storage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="9e93a-122">**olvasási-fájl-feltöltési-értesítési**, amely fájl feltöltése értesítéseket fogad az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9e93a-122">**read-file-upload-notification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="9e93a-123">Az IoT-központ Azure IoT-eszközök SDK-k segítségével számos eszköz platformok és nyelvek (például C, a .NET és a Javascript) támogatja.</span><span class="sxs-lookup"><span data-stu-id="9e93a-123">IoT Hub supports many device platforms and languages (including C, .NET, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="9e93a-124">Tekintse meg a [Azure IoT fejlesztői központ] az eszköz csatlakoztatása az Azure IoT Hub részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="9e93a-124">Refer to the [Azure IoT Developer Center] for step-by-step instructions on how to connect your device to Azure IoT Hub.</span></span>

<span data-ttu-id="9e93a-125">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="9e93a-125">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="9e93a-126">A legújabb [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="9e93a-126">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="9e93a-127">Maven 3</span><span class="sxs-lookup"><span data-stu-id="9e93a-127">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="9e93a-128">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="9e93a-128">An active Azure account.</span></span> <span data-ttu-id="9e93a-129">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="9e93a-129">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="9e93a-130">Egy eszköz alkalmazás-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="9e93a-130">Upload a file from a device app</span></span>

<span data-ttu-id="9e93a-131">Ebben a szakaszban módosítsa az eszköz alkalmazás létrehozott [IoT Hub-felhő eszközre üzenetek](iot-hub-java-java-c2d.md) lehet feltölteni a fájlt az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="9e93a-131">In this section, you modify the device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) to upload a file to IoT hub.</span></span>

1. <span data-ttu-id="9e93a-132">Egy képfájl másolja a `simulated-device` mappa, és nevezze át `myimage.png`.</span><span class="sxs-lookup"><span data-stu-id="9e93a-132">Copy an image file to the `simulated-device` folder and rename it `myimage.png`.</span></span>

1. <span data-ttu-id="9e93a-133">Egy szövegszerkesztőben nyissa meg a `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9e93a-133">Using a text editor, open the `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="9e93a-134">Adjon hozzá a változó; a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="9e93a-134">Add the variable declaration to the **App** class:</span></span>

    ```java
    private static String fileName = "myimage.png";
    ```

1. <span data-ttu-id="9e93a-135">Fájl feltöltése visszahívási állapotüzenetek feldolgozásához, adja hozzá a következő beágyazott osztályt a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="9e93a-135">To process file upload status callback messages, add the following nested class to the **App** class:</span></span>

    ```java
    // Define a callback method to print status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to file upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="9e93a-136">Az IoT-központ képek feltöltése, adja hozzá az alábbi metódust a **App** osztályt a képek feltöltése az IoT-központ:</span><span class="sxs-lookup"><span data-stu-id="9e93a-136">To upload images to IoT Hub, add the following method to the **App** class to upload images to IoT Hub:</span></span>

    ```java
    // Use IoT Hub to upload a file asynchronously to Azure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

1. <span data-ttu-id="9e93a-137">Módosítsa a **fő** metódust kell meghívni a **uploadFile** módszer, ahogy az a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="9e93a-137">Modify the **main** method to call the **uploadFile** method as shown in the following snippet:</span></span>

    ```java
    client.open();

    try
    {
      // Get the filename and start the upload.
      String fullFileName = System.getProperty("user.dir") + File.separator + fileName;
      uploadFile(fullFileName);
      System.out.println("File upload started with success");
    }
    catch (Exception e)
    {
      System.out.println("Exception uploading file: " + e.getCause() + " \nERROR: " + e.getMessage());
    }

    MessageSender sender = new MessageSender();
    ```

1. <span data-ttu-id="9e93a-138">A következő paranccsal hozhat létre a **szimulált eszköz** alkalmazás és a hibák:</span><span class="sxs-lookup"><span data-stu-id="9e93a-138">Use the following command to build the **simulated-device** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="9e93a-139">Fájl feltöltése értesítést</span><span class="sxs-lookup"><span data-stu-id="9e93a-139">Receive a file upload notification</span></span>

<span data-ttu-id="9e93a-140">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, hogy a fájl feltöltése értesítési üzeneteket fogad az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="9e93a-140">In this section, you create a Java console app that receives file upload notification messages from IoT Hub.</span></span>

<span data-ttu-id="9e93a-141">Van szüksége a **iothubowner** kapcsolati karakterlánc az IoT hub, ez a szakasz befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="9e93a-141">You need the **iothubowner** connection string for your IoT Hub to complete this section.</span></span> <span data-ttu-id="9e93a-142">A kapcsolati karakterlánc található a [Azure-portálon](https://portal.azure.com/) a a **megosztott hozzáférési házirend** panelen.</span><span class="sxs-lookup"><span data-stu-id="9e93a-142">You can find the connection string in the [Azure portal](https://portal.azure.com/) on the **Shared access policy** blade.</span></span>

1. <span data-ttu-id="9e93a-143">Nevű Maven-projekt létrehozása **olvasás-fájl-feltöltési-értesítési** parancsot a parancssorba az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="9e93a-143">Create a Maven project called **read-file-upload-notification** using the following command at your command prompt.</span></span> <span data-ttu-id="9e93a-144">Megjegyzés: Ez a parancs egy egyetlen, hosszú parancsot:</span><span class="sxs-lookup"><span data-stu-id="9e93a-144">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. <span data-ttu-id="9e93a-145">A parancssorban keresse meg az új `read-file-upload-notification` mappát.</span><span class="sxs-lookup"><span data-stu-id="9e93a-145">At your command prompt, navigate to the new `read-file-upload-notification` folder.</span></span>

1. <span data-ttu-id="9e93a-146">Egy szövegszerkesztőben nyissa meg a `pom.xml` fájlt a `read-file-upload-notification` mappa, és adja hozzá a következő függőség a **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="9e93a-146">Using a text editor, open the `pom.xml` file in the `read-file-upload-notification` folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="9e93a-147">A függőség hozzáadása lehetővé teszi, hogy a **IOT hubbal-java-szolgáltatásügyfél** csomag kommunikáljon az IoT-központ szolgáltatás az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="9e93a-147">Adding the dependency enables you to use the **iothub-java-service-client** package in your application to communicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="9e93a-148">Ellenőrizze, hogy a legújabb **iot-szolgáltatásügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="9e93a-148">You can check for the latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="9e93a-149">Mentse és zárja be a `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9e93a-149">Save and close the `pom.xml` file.</span></span>

1. <span data-ttu-id="9e93a-150">Egy szövegszerkesztőben nyissa meg a `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9e93a-150">Using a text editor, open the `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="9e93a-151">Adja hozzá a következő **importálási** utasításokat a fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="9e93a-151">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. <span data-ttu-id="9e93a-152">A következő osztály szintű változók hozzáadásával a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="9e93a-152">Add the following class-level variables to the **App** class:</span></span>

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. <span data-ttu-id="9e93a-153">A fájl feltöltése, a konzol adatainak nyomtatásához adja hozzá a következő beágyazott osztályt a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="9e93a-153">To print information about the file upload to the console, add the following nested class to the **App** class:</span></span>

    ```java
    // Create a thread to receive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Recieve file upload notifications...");
            FileUploadNotification fileUploadNotification = fileUploadNotificationReceiver.receive();
            if (fileUploadNotification != null) {
              System.out.println("File Upload notification received");
              System.out.println("Device Id : " + fileUploadNotification.getDeviceId());
              System.out.println("Blob Uri: " + fileUploadNotification.getBlobUri());
              System.out.println("Blob Name: " + fileUploadNotification.getBlobName());
              System.out.println("Last Updated : " + fileUploadNotification.getLastUpdatedTimeDate());
              System.out.println("Blob Size (Bytes): " + fileUploadNotification.getBlobSizeInBytes());
              System.out.println("Enqueued Time: " + fileUploadNotification.getEnqueuedTimeUtcDate());
            }
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. <span data-ttu-id="9e93a-154">Indítsa el a szál, amely figyeli a fájl feltöltése értesítések, adja hozzá az alábbi kódot a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="9e93a-154">To start the thread that listens for file upload notifications, add the following code to the **main** method:</span></span>

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from the ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start the thread to receive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER to exit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

1. <span data-ttu-id="9e93a-155">Mentse és zárja be a `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9e93a-155">Save and close the `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="9e93a-156">A következő paranccsal hozhat létre a **olvasás-fájl-feltöltési-értesítési** alkalmazás és a hibák:</span><span class="sxs-lookup"><span data-stu-id="9e93a-156">Use the following command to build the **read-file-upload-notification** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a><span data-ttu-id="9e93a-157">Az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="9e93a-157">Run the applications</span></span>

<span data-ttu-id="9e93a-158">Készen áll arra, hogy futtassa az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="9e93a-158">Now you are ready to run the applications.</span></span>

<span data-ttu-id="9e93a-159">A parancsot a parancssorba a `read-file-upload-notification` mappa, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="9e93a-159">At a command prompt in the `read-file-upload-notification` folder, run the following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="9e93a-160">A parancsot a parancssorba a `simulated-device` mappa, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="9e93a-160">At a command prompt in the `simulated-device` folder, run the following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="9e93a-161">Az alábbi képernyőfelvételen látható kimenetét a **szimulált eszköz** alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="9e93a-161">The following screenshot shows the output from the **simulated-device** app:</span></span>

![Szimulált eszköz alkalmazás kimenete](media/iot-hub-java-java-upload/simulated-device.png)

<span data-ttu-id="9e93a-163">Az alábbi képernyőfelvételen látható kimenetét a **olvasás-fájl-feltöltési-értesítési** alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="9e93a-163">The following screenshot shows the output from the **read-file-upload-notification** app:</span></span>

![Olvasási-fájl-feltöltési-értesítés alkalmazás kimenete](media/iot-hub-java-java-upload/read-file-upload-notification.png)

<span data-ttu-id="9e93a-165">A portál segítségével a tároló, konfigurálta a feltöltött fájl megtekintése:</span><span class="sxs-lookup"><span data-stu-id="9e93a-165">You can use the portal to view the uploaded file in the storage container you configured:</span></span>

![Fájl feltöltése](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a><span data-ttu-id="9e93a-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9e93a-167">Next steps</span></span>

<span data-ttu-id="9e93a-168">Ebben az oktatóprogramban megismerte fájlfeltöltéseket eszközökről leegyszerűsítése érdekében az IoT-központ fájl feltöltése funkcióinak használatát.</span><span class="sxs-lookup"><span data-stu-id="9e93a-168">In this tutorial, you learned how to use the file upload capabilities of IoT Hub to simplify file uploads from devices.</span></span> <span data-ttu-id="9e93a-169">IoT hub-szolgáltatások és a forgatókönyvet a következő cikkek az eltérések felfedezése továbbra is:</span><span class="sxs-lookup"><span data-stu-id="9e93a-169">You can continue to explore IoT hub features and scenarios with the following articles:</span></span>

* <span data-ttu-id="9e93a-170">[Programozott módon létrehoz egy IoT-központot][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="9e93a-170">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="9e93a-171">[C SDK bemutatása][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="9e93a-171">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="9e93a-172">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="9e93a-172">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="9e93a-173">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="9e93a-173">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="9e93a-174">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="9e93a-174">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png
[3]: ./media/iot-hub-csharp-csharp-file-upload/enable-file-notifications.png

<!-- Links -->



[Azure IoT fejlesztői központ]: http://www.azure.com/develop/iot

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Storage]:../storage/common/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md


