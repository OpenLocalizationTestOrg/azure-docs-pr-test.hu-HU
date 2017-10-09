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
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub"></a>Az IoT hubbal eszköz toohello felhőből fájlok feltöltése

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Ez az oktatóanyag hello hello kód épül [IoT Hub-felhő eszközre üzenetek](iot-hub-java-java-c2d.md) oktatóanyag tooshow, hogyan toouse hello [fájl feltöltése képességeit az IoT-központ](iot-hub-devguide-file-upload.md) tooupload fájl túl[ Az Azure blob storage](../storage/index.md). hello oktatóanyag bemutatja, hogyan számára:

- Biztonságosan adjon meg egy eszközt az Azure blob URI-fájl feltöltése.
- Az IoT-központ feltöltés értesítések tootrigger feldolgozása hello fájl az alkalmazás háttérbeli hello használata.

Hello [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) és [IoT Hub-felhő eszközre üzenetek](iot-hub-java-java-c2d.md) oktatóanyagok hello alapvető eszköz-felhő és a felhő eszközre üzenetkezelési funkcióit az IoT-központ megjelenítése. Hello [folyamat eszköz felhőbe küldött üzeneteket](iot-hub-java-java-process-d2c.md) az oktatóanyag leírja a módon tooreliably tároló eszközről a felhőbe üzeneteket az Azure blob Storage tárolóban. Bizonyos esetekben azonban leképezése nem hello adatokat az eszközök elküldik üzenetbe hello viszonylag kis eszközről a felhőbe, amely az IoT-központ fogadja könnyen. Példa:

* Nagy fájlok, amelyek képeket
* Videók
* Nagy gyakoriságú lekérdező vibráció adatok
* Valamilyen előre feldolgozott adatokat.

Ezek a fájlok jellemzően hello felhő eszközökkel, mint a feldolgozott kötegelt [Azure Data Factory](../data-factory/index.md) vagy hello [Hadoop](../hdinsight/index.md) verem. Ha egy eszközről tooupland fájlok van szüksége, hello biztonsága és megbízhatósága szempontjából az IoT-központ továbbra is használhatja.

Ez az oktatóanyag végén hello két Java konzol alkalmazások futtatása:

* **Szimulált eszköz**, hello alkalmazás hello [IoT hubbal küldési felhőből eszközre küldött üzenetek] az oktatóanyagban létrehozott egy módosított verziója. Ez az alkalmazás feltölt egy fájlt toostorage az IoT hub által biztosított SAS URI-k használata.
* **olvasási-fájl-feltöltési-értesítési**, amely fájl feltöltése értesítéseket fogad az IoT hub.

> [!NOTE]
> Az IoT-központ Azure IoT-eszközök SDK-k segítségével számos eszköz platformok és nyelvek (például C, a .NET és a Javascript) támogatja. Tekintse meg a toohello [Azure IoT fejlesztői központ] részletesen ismerteti a tooconnect az eszköz tooAzure IoT-központot.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/) néhány perc alatt.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Egy eszköz alkalmazás-fájl feltöltése

Ebben a szakaszban létrehozott hello eszközalkalmazás módosítása [IoT Hub-felhő eszközre üzenetek](iot-hub-java-java-c2d.md) tooupload fájl tooIoT hubot.

1. Másolja át egy kép fájl toohello `simulated-device` mappa, és nevezze át `myimage.png`.

1. Egy szövegszerkesztőben nyissa meg hello `simulated-device\src\main\java\com\mycompany\app\App.java` fájlt.

1. Adja hozzá a hello Változódeklaráció toohello **App** osztály:

    ```java
    private static String fileName = "myimage.png";
    ```

1. tooprocess feltöltés visszahívása állapotüzenetek fájlt, adja hozzá a következő hello beágyazott osztály toohello **App** osztály:

    ```java
    // Define a callback method tooprint status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toofile upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. tooupload képek tooIoT Hub, adja hozzá a következő metódus toohello hello **App** osztály tooupload lemezképek tooIoT Hub:

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

1. Módosítsa a hello **fő** metódus toocall hello **uploadFile** módszer, ahogy az alábbi részlet hello:

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

1. Használjon hello következő parancsot a toobuild hello **szimulált eszköz** alkalmazás és a hibák:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a>Fájl feltöltése értesítést

Ebben a szakaszban hozzon létre egy Java-Konzolalkalmazás, hogy a fájl feltöltése értesítési üzeneteket fogad az IoT-központot.

Hello kell **iothubowner** kapcsolati karakterlánc az IoT-központ toocomplete az ebben a szakaszban. Hello kapcsolati karakterlánc az hello található [Azure-portálon](https://portal.azure.com/) a hello **megosztott hozzáférési házirend** panelen.

1. Nevű Maven-projekt létrehozása **olvasás-fájl-feltöltési-értesítési** a következő parancsot a parancssorba hello segítségével. Megjegyzés: Ez a parancs egy egyetlen, hosszú parancsot:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. A parancssorban keresse meg az új toohello `read-file-upload-notification` mappa.

1. Egy szövegszerkesztőben nyissa meg hello `pom.xml` hello fájlban `read-file-upload-notification` mappa, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont. Hello-függőség felvétele lehetővé teszi toouse hello **IOT hubbal-java-szolgáltatásügyfél** az alkalmazás toocommunicate az IoT hub szolgáltatással-csomagot:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Mentse és zárja be a hello `pom.xml` fájlt.

1. Egy szövegszerkesztőben nyissa meg hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fájlt.

1. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. Adja hozzá a következő osztály változók toohello hello **App** osztály:

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. hello fájl feltöltése toohello konzol tooprint információt adja hozzá a hello következő beágyazott osztály toohello **App** osztály:

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

1. toostart hello szál, amely figyeli a fájl feltöltése értesítések, adja hozzá a következő kód toohello hello **fő** módszert:

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

1. Mentse és zárja be a hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fájlt.

1. Használjon hello következő parancsot a toobuild hello **olvasás-fájl-feltöltési-értesítési** alkalmazás és a hibák:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához

Most már készen áll a toorun hello alkalmazások áll.

A parancssorba a hello `read-file-upload-notification` mappa, futtassa a következő parancs hello:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

A parancssorba a hello `simulated-device` mappa, futtassa a következő parancs hello:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

hello következő képernyőfelvétel a hello hello kimeneti **szimulált eszköz** alkalmazást:

![Szimulált eszköz alkalmazás kimenete](media/iot-hub-java-java-upload/simulated-device.png)

hello következő képernyőfelvétel a hello hello kimeneti **olvasás-fájl-feltöltési-értesítési** alkalmazást:

![Olvasási-fájl-feltöltési-értesítés alkalmazás kimenete](media/iot-hub-java-java-upload/read-file-upload-notification.png)

Hello portál tooview hello feltöltött fájl konfigurált hello tároló használható:

![Fájl feltöltése](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta, hogyan toouse hello fájlfeltöltés toosimplify fájlfeltöltések eszközökről az IoT-központ képességeit. A következő cikkek hello tooexplore IoT hub-szolgáltatások és a forgatókönyv folytatása:

* [Programozott módon létrehoz egy IoT-központot][lnk-create-hub]
* [Bevezetés tooC SDK][lnk-c-sdk]
* [Az Azure IoT SDK-k][lnk-sdks]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [Egy eszköz szimulálva IoT oldala][lnk-iotedge]

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


