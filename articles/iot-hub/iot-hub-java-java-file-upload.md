---
title: "eszközök tooAzure IoT Hub Java aaaUpload fájlokat |} Microsoft Docs"
description: "Hogyan tooupload fájljainak eszköz toohello felhő Javához készült Azure IoT-eszközök SDK használatával. Egy Azure blob tároló feltöltött fájlok tárolják."
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
ms.openlocfilehash: e305fe61bf7ca0aeb2c092bc2c7efebdc78d4f68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub"></a><span data-ttu-id="d78eb-104">Az IoT hubbal eszköz toohello felhőből fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="d78eb-104">Upload files from your device toohello cloud with IoT Hub</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="d78eb-105">Ez az oktatóanyag hello hello kód épül [IoT Hub-felhő eszközre üzenetek](iot-hub-java-java-c2d.md) oktatóanyag tooshow, hogyan toouse hello [fájl feltöltése képességeit az IoT-központ](iot-hub-devguide-file-upload.md) tooupload fájl túl[ Az Azure blob storage](../storage/index.md).</span><span class="sxs-lookup"><span data-stu-id="d78eb-105">This tutorial builds on hello code in hello [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorial tooshow you how toouse hello [file upload capabilities of IoT Hub](iot-hub-devguide-file-upload.md) tooupload a file too[Azure blob storage](../storage/index.md).</span></span> <span data-ttu-id="d78eb-106">hello oktatóanyag bemutatja, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="d78eb-106">hello tutorial shows you how to:</span></span>

- <span data-ttu-id="d78eb-107">Biztonságosan adjon meg egy eszközt az Azure blob URI-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="d78eb-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="d78eb-108">Az IoT-központ feltöltés értesítések tootrigger feldolgozása hello fájl az alkalmazás háttérbeli hello használata.</span><span class="sxs-lookup"><span data-stu-id="d78eb-108">Use hello IoT Hub file upload notifications tootrigger processing hello file in your app back end.</span></span>

<span data-ttu-id="d78eb-109">Hello [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) és [IoT Hub-felhő eszközre üzenetek](iot-hub-java-java-c2d.md) oktatóanyagok hello alapvető eszköz-felhő és a felhő eszközre üzenetkezelési funkcióit az IoT-központ megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="d78eb-109">hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorials show hello basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="d78eb-110">Hello [folyamat eszköz felhőbe küldött üzeneteket](iot-hub-java-java-process-d2c.md) az oktatóanyag leírja a módon tooreliably tároló eszközről a felhőbe üzeneteket az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="d78eb-110">hello [Process Device-to-Cloud messages](iot-hub-java-java-process-d2c.md) tutorial describes a way tooreliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="d78eb-111">Bizonyos esetekben azonban leképezése nem hello adatokat az eszközök elküldik üzenetbe hello viszonylag kis eszközről a felhőbe, amely az IoT-központ fogadja könnyen.</span><span class="sxs-lookup"><span data-stu-id="d78eb-111">However, in some scenarios you cannot easily map hello data your devices send into hello relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="d78eb-112">Példa:</span><span class="sxs-lookup"><span data-stu-id="d78eb-112">For example:</span></span>

* <span data-ttu-id="d78eb-113">Nagy fájlok, amelyek képeket</span><span class="sxs-lookup"><span data-stu-id="d78eb-113">Large files that contain images</span></span>
* <span data-ttu-id="d78eb-114">Videók</span><span class="sxs-lookup"><span data-stu-id="d78eb-114">Videos</span></span>
* <span data-ttu-id="d78eb-115">Nagy gyakoriságú lekérdező vibráció adatok</span><span class="sxs-lookup"><span data-stu-id="d78eb-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="d78eb-116">Valamilyen előre feldolgozott adatokat.</span><span class="sxs-lookup"><span data-stu-id="d78eb-116">Some form of preprocessed data.</span></span>

<span data-ttu-id="d78eb-117">Ezek a fájlok jellemzően hello felhő eszközökkel, mint a feldolgozott kötegelt [Azure Data Factory](../data-factory/index.md) vagy hello [Hadoop](../hdinsight/index.md) verem.</span><span class="sxs-lookup"><span data-stu-id="d78eb-117">These files are typically batch processed in hello cloud using tools such as [Azure Data Factory](../data-factory/index.md) or hello [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="d78eb-118">Ha egy eszközről tooupland fájlok van szüksége, hello biztonsága és megbízhatósága szempontjából az IoT-központ továbbra is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d78eb-118">When you need tooupland files from a device, you can still use hello security and reliability of IoT Hub.</span></span>

<span data-ttu-id="d78eb-119">Ez az oktatóanyag végén hello két Java konzol alkalmazások futtatása:</span><span class="sxs-lookup"><span data-stu-id="d78eb-119">At hello end of this tutorial you run two Java console apps:</span></span>

* <span data-ttu-id="d78eb-120">**Szimulált eszköz**, hello alkalmazás hello [IoT hubbal küldési felhőből eszközre küldött üzenetek] az oktatóanyagban létrehozott egy módosított verziója.</span><span class="sxs-lookup"><span data-stu-id="d78eb-120">**simulated-device**, a modified version of hello app created in hello [Send Cloud-to-Device messages with IoT Hub] tutorial.</span></span> <span data-ttu-id="d78eb-121">Ez az alkalmazás feltölt egy fájlt toostorage az IoT hub által biztosított SAS URI-k használata.</span><span class="sxs-lookup"><span data-stu-id="d78eb-121">This app uploads a file toostorage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="d78eb-122">**olvasási-fájl-feltöltési-értesítési**, amely fájl feltöltése értesítéseket fogad az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d78eb-122">**read-file-upload-notification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="d78eb-123">Az IoT-központ Azure IoT-eszközök SDK-k segítségével számos eszköz platformok és nyelvek (például C, a .NET és a Javascript) támogatja.</span><span class="sxs-lookup"><span data-stu-id="d78eb-123">IoT Hub supports many device platforms and languages (including C, .NET, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="d78eb-124">Tekintse meg a toohello [Azure IoT fejlesztői központ] részletesen ismerteti a tooconnect az eszköz tooAzure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="d78eb-124">Refer toohello [Azure IoT Developer Center] for step-by-step instructions on how tooconnect your device tooAzure IoT Hub.</span></span>

<span data-ttu-id="d78eb-125">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d78eb-125">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="d78eb-126">hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="d78eb-126">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="d78eb-127">Maven 3</span><span class="sxs-lookup"><span data-stu-id="d78eb-127">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="d78eb-128">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="d78eb-128">An active Azure account.</span></span> <span data-ttu-id="d78eb-129">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="d78eb-129">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="d78eb-130">Egy eszköz alkalmazás-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="d78eb-130">Upload a file from a device app</span></span>

<span data-ttu-id="d78eb-131">Ebben a szakaszban létrehozott hello eszközalkalmazás módosítása [IoT Hub-felhő eszközre üzenetek](iot-hub-java-java-c2d.md) tooupload fájl tooIoT hubot.</span><span class="sxs-lookup"><span data-stu-id="d78eb-131">In this section, you modify hello device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tooupload a file tooIoT hub.</span></span>

1. <span data-ttu-id="d78eb-132">Másolja át egy kép fájl toohello `simulated-device` mappa, és nevezze át `myimage.png`.</span><span class="sxs-lookup"><span data-stu-id="d78eb-132">Copy an image file toohello `simulated-device` folder and rename it `myimage.png`.</span></span>

1. <span data-ttu-id="d78eb-133">Egy szövegszerkesztőben nyissa meg hello `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d78eb-133">Using a text editor, open hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="d78eb-134">Adja hozzá a hello Változódeklaráció toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="d78eb-134">Add hello variable declaration toohello **App** class:</span></span>

    ```java
    private static String fileName = "myimage.png";
    ```

1. <span data-ttu-id="d78eb-135">tooprocess feltöltés visszahívása állapotüzenetek fájlt, adja hozzá a következő hello beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="d78eb-135">tooprocess file upload status callback messages, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Define a callback method tooprint status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toofile upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="d78eb-136">tooupload képek tooIoT Hub, adja hozzá a következő metódus toohello hello **App** osztály tooupload lemezképek tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="d78eb-136">tooupload images tooIoT Hub, add hello following method toohello **App** class tooupload images tooIoT Hub:</span></span>

    ```java
    // Use IoT Hub tooupload a file asynchronously tooAzure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

1. <span data-ttu-id="d78eb-137">Módosítsa a hello **fő** metódus toocall hello **uploadFile** módszer, ahogy az alábbi részlet hello:</span><span class="sxs-lookup"><span data-stu-id="d78eb-137">Modify hello **main** method toocall hello **uploadFile** method as shown in hello following snippet:</span></span>

    ```java
    client.open();

    try
    {
      // Get hello filename and start hello upload.
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

1. <span data-ttu-id="d78eb-138">Használjon hello következő parancsot a toobuild hello **szimulált eszköz** alkalmazás és a hibák:</span><span class="sxs-lookup"><span data-stu-id="d78eb-138">Use hello following command toobuild hello **simulated-device** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="d78eb-139">Fájl feltöltése értesítést</span><span class="sxs-lookup"><span data-stu-id="d78eb-139">Receive a file upload notification</span></span>

<span data-ttu-id="d78eb-140">Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, hogy a fájl feltöltése értesítési üzeneteket fogad az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="d78eb-140">In this section, you create a Java console app that receives file upload notification messages from IoT Hub.</span></span>

<span data-ttu-id="d78eb-141">Hello kell **iothubowner** kapcsolati karakterlánc az IoT-központ toocomplete az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="d78eb-141">You need hello **iothubowner** connection string for your IoT Hub toocomplete this section.</span></span> <span data-ttu-id="d78eb-142">Hello kapcsolati karakterlánc az hello található [Azure-portálon](https://portal.azure.com/) a hello **megosztott hozzáférési házirend** panelen.</span><span class="sxs-lookup"><span data-stu-id="d78eb-142">You can find hello connection string in hello [Azure portal](https://portal.azure.com/) on hello **Shared access policy** blade.</span></span>

1. <span data-ttu-id="d78eb-143">Nevű Maven-projekt létrehozása **olvasás-fájl-feltöltési-értesítési** a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="d78eb-143">Create a Maven project called **read-file-upload-notification** using hello following command at your command prompt.</span></span> <span data-ttu-id="d78eb-144">Megjegyzés: Ez a parancs egy egyetlen, hosszú parancsot:</span><span class="sxs-lookup"><span data-stu-id="d78eb-144">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. <span data-ttu-id="d78eb-145">A parancssorban keresse meg az új toohello `read-file-upload-notification` mappa.</span><span class="sxs-lookup"><span data-stu-id="d78eb-145">At your command prompt, navigate toohello new `read-file-upload-notification` folder.</span></span>

1. <span data-ttu-id="d78eb-146">Egy szövegszerkesztőben nyissa meg hello `pom.xml` hello fájlban `read-file-upload-notification` mappa, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="d78eb-146">Using a text editor, open hello `pom.xml` file in hello `read-file-upload-notification` folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="d78eb-147">Hello-függőség felvétele lehetővé teszi toouse hello **IOT hubbal-java-szolgáltatásügyfél** az alkalmazás toocommunicate az IoT hub szolgáltatással-csomagot:</span><span class="sxs-lookup"><span data-stu-id="d78eb-147">Adding hello dependency enables you toouse hello **iothub-java-service-client** package in your application toocommunicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="d78eb-148">Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="d78eb-148">You can check for hello latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="d78eb-149">Mentse és zárja be a hello `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d78eb-149">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="d78eb-150">Egy szövegszerkesztőben nyissa meg hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d78eb-150">Using a text editor, open hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="d78eb-151">Adja hozzá a következő hello **importálása** utasítások toohello fájlt:</span><span class="sxs-lookup"><span data-stu-id="d78eb-151">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. <span data-ttu-id="d78eb-152">Adja hozzá a következő osztály változók toohello hello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="d78eb-152">Add hello following class-level variables toohello **App** class:</span></span>

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. <span data-ttu-id="d78eb-153">hello fájl feltöltése toohello konzol tooprint információt adja hozzá a hello következő beágyazott osztály toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="d78eb-153">tooprint information about hello file upload toohello console, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Create a thread tooreceive file upload notifications.
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

1. <span data-ttu-id="d78eb-154">toostart hello szál, amely figyeli a fájl feltöltése értesítések, adja hozzá a következő kód toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="d78eb-154">toostart hello thread that listens for file upload notifications, add hello following code toohello **main** method:</span></span>

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from hello ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start hello thread tooreceive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER tooexit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

1. <span data-ttu-id="d78eb-155">Mentse és zárja be a hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d78eb-155">Save and close hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="d78eb-156">Használjon hello következő parancsot a toobuild hello **olvasás-fájl-feltöltési-értesítési** alkalmazás és a hibák:</span><span class="sxs-lookup"><span data-stu-id="d78eb-156">Use hello following command toobuild hello **read-file-upload-notification** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="d78eb-157">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="d78eb-157">Run hello applications</span></span>

<span data-ttu-id="d78eb-158">Most már készen áll a toorun hello alkalmazások áll.</span><span class="sxs-lookup"><span data-stu-id="d78eb-158">Now you are ready toorun hello applications.</span></span>

<span data-ttu-id="d78eb-159">A parancssorba a hello `read-file-upload-notification` mappa, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="d78eb-159">At a command prompt in hello `read-file-upload-notification` folder, run hello following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="d78eb-160">A parancssorba a hello `simulated-device` mappa, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="d78eb-160">At a command prompt in hello `simulated-device` folder, run hello following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="d78eb-161">hello következő képernyőfelvétel a hello hello kimeneti **szimulált eszköz** alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="d78eb-161">hello following screenshot shows hello output from hello **simulated-device** app:</span></span>

![Szimulált eszköz alkalmazás kimenete](media/iot-hub-java-java-upload/simulated-device.png)

<span data-ttu-id="d78eb-163">hello következő képernyőfelvétel a hello hello kimeneti **olvasás-fájl-feltöltési-értesítési** alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="d78eb-163">hello following screenshot shows hello output from hello **read-file-upload-notification** app:</span></span>

![Olvasási-fájl-feltöltési-értesítés alkalmazás kimenete](media/iot-hub-java-java-upload/read-file-upload-notification.png)

<span data-ttu-id="d78eb-165">Hello portál tooview hello feltöltött fájl konfigurált hello tároló használható:</span><span class="sxs-lookup"><span data-stu-id="d78eb-165">You can use hello portal tooview hello uploaded file in hello storage container you configured:</span></span>

![Fájl feltöltése](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a><span data-ttu-id="d78eb-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d78eb-167">Next steps</span></span>

<span data-ttu-id="d78eb-168">Ebben az oktatóanyagban megtanulta, hogyan toouse hello fájlfeltöltés toosimplify fájlfeltöltések eszközökről az IoT-központ képességeit.</span><span class="sxs-lookup"><span data-stu-id="d78eb-168">In this tutorial, you learned how toouse hello file upload capabilities of IoT Hub toosimplify file uploads from devices.</span></span> <span data-ttu-id="d78eb-169">A következő cikkek hello tooexplore IoT hub-szolgáltatások és a forgatókönyv folytatása:</span><span class="sxs-lookup"><span data-stu-id="d78eb-169">You can continue tooexplore IoT hub features and scenarios with hello following articles:</span></span>

* <span data-ttu-id="d78eb-170">[Programozott módon létrehoz egy IoT-központot][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="d78eb-170">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="d78eb-171">[Bevezetés tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="d78eb-171">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="d78eb-172">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="d78eb-172">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="d78eb-173">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="d78eb-173">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d78eb-174">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="d78eb-174">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

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


