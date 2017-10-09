---
title: "eszközök tooAzure .NET IoT hubot aaaUpload fájlokat |} Microsoft Docs"
description: "Hogyan tooupload fájlok az Azure IoT-eszközök SDK használatával a .NET-hez eszköz toohello felhő. Egy Azure blob tároló feltöltött fájlok tárolják."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: elioda
ms.openlocfilehash: 07d555f6ba8b067bbd3233bc8eebaa220ad2388b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub-using-net"></a>Az eszköz toohello felhőből fájlok feltöltése az IoT hubbal .NET használatával

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Ez az oktatóanyag épít, hello hello kód [IoT Hub-felhő eszközre üzenetek](iot-hub-csharp-csharp-c2d.md) oktatóanyag tooshow, hogyan toouse hello fájl feltöltése képességeit az IoT-központ. Megmutatja, hogyan számára:

- Biztonságosan adjon meg egy eszközt az Azure blob URI-fájl feltöltése.
- Az IoT-központ feltöltés értesítések tootrigger feldolgozása hello fájl az alkalmazás háttérbeli hello használata.

Hello [Ismerkedés az IoT-központ](iot-hub-csharp-csharp-getstarted.md) és [IoT Hub-felhő eszközre üzenetek](iot-hub-csharp-csharp-c2d.md) oktatóanyagok hello alapvető eszköz-felhő és a felhő eszközre üzenetkezelési funkcióit az IoT-központ megjelenítése. Hello [folyamat eszköz felhőbe küldött üzeneteket](iot-hub-csharp-csharp-process-d2c.md) az oktatóanyag leírja a módon tooreliably tároló eszközről a felhőbe üzeneteket az Azure blob Storage tárolóban. Bizonyos esetekben azonban leképezése nem hello adatokat az eszközök elküldik üzenetbe hello viszonylag kis eszközről a felhőbe, amely az IoT-központ fogadja könnyen. Példa:

* Nagy fájlok, amelyek képeket
* Videók
* Nagy gyakoriságú lekérdező vibráció adatok
* Néhány előre feldolgozott adatok formája

Ezek a fájlok jellemzően hello felhő eszközökkel, mint a feldolgozott kötegelt [Azure Data Factory](../data-factory/index.md) vagy hello [Hadoop](../hdinsight/index.md) verem. Ha egy eszközről tooupload fájlok van szüksége, hello biztonsága és megbízhatósága szempontjából az IoT-központ továbbra is használhatja.

Ez az oktatóanyag végén hello két .NET konzol alkalmazások futtatása:

* **SimulatedDevice**, hello alkalmazás hello létrehozott egy módosított verziója [IoT Hub-felhő eszközre üzenetek](iot-hub-csharp-csharp-c2d.md) oktatóanyag. Ez az alkalmazás feltölt egy fájlt toostorage az IoT hub által biztosított SAS URI-k használata.
* **ReadFileUploadNotification**, amely fájl feltöltése értesítéseket fogad az IoT hub.

> [!NOTE]
> Az IoT-központ Azure IoT-eszközök SDK-k segítségével számos eszköz platformok és nyelvek (például C, Java és Javascript) támogatja. Tekintse meg a toohello [Azure IoT fejlesztői központ] részletesen ismerteti a tooconnect az eszköz tooAzure IoT-központot.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Visual Studio 2015-öt vagy a Visual Studio 2017
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Egy eszköz alkalmazás-fájl feltöltése

Ebben a szakaszban létrehozott hello eszközalkalmazás módosítása [IoT Hub-felhő eszközre üzenetek](iot-hub-csharp-csharp-c2d.md) hello IoT-központ üzeneteit tooreceive felhő eszközre.

1. A Visual Studióban, kattintson a jobb gombbal hello **SimulatedDevice** projektre, kattintson **Hozzáadás**, és kattintson a **meglévő cikk**. Keresse meg a tooan képfájl, és adja hozzá a projektben. Ez az oktatóanyag azt feltételezi, hogy hello kép nevű `image.jpg`.

1. Kattintson a jobb gombbal a hello lemezképet, és kattintson **tulajdonságok**. Győződjön meg arról, hogy **tooOutput Directory másolása** értéke túl**mindig másolása**.

    ![][1]

1. A hello **Program.cs** fájlt, adja hozzá a következő utasítások hello fájl hello tetején hello:

    ```csharp
    using System.IO;
    ```

1. Adja hozzá a következő metódus toohello hello **Program** osztály:

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time tooupload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    Hello `UploadToBlobAsync` módszer veszi a hello fájlnév, és hello fájl toobe adatfolyam forrását feltöltött és hello feltöltés toostorage kezeli. hello Konzolalkalmazás hello időt tooupload hello fájl jeleníti meg.

1. Adja hozzá a következő metódus a hello hello **fő** metódus közvetlenül előtt hello `Console.ReadLine()` sor:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].

## <a name="receive-a-file-upload-notification"></a>Fájl feltöltése értesítést

Ebben a szakaszban egy .NET-Konzolalkalmazás, hogy a fájl feltöltése értesítési üzeneteket fogad az IoT-központ írni.

1. A hello jelenlegi Visual Studio megoldás Visual C# Windows-projekt létrehozása hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **ReadFileUploadNotification**.

    ![A Visual Studio új projekt][2]

1. A Megoldáskezelőben kattintson a jobb gombbal hello **ReadFileUploadNotification** projektre, és kattintson a **NuGet-csomagok kezelése...** .

1. A hello **NuGet-Csomagkezelő** ablakban, keresse meg **Microsoft.Azure.Devices**, kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.

    Ez a művelet tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK NuGet-csomag] a hello **ReadFileUploadNotification** projekt.

1. A hello **Program.cs** fájlt, adja hozzá a következő utasítások hello fájl hello tetején hello:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. Adja hozzá a következő mezők toohello hello **Program** osztály. Helyettesítse be hello helyőrző értékének hello IoT hub kapcsolati karakterláncnak a következőről [Ismerkedés az IoT-központ]:

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. Adja hozzá a következő metódus toohello hello **Program** osztály:

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    Megjegyzés: a receive mintát van hello ugyanazon egy használt tooreceive felhő eszközre üzenetek hello eszköz alkalmazásból.

1. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter tooexit\n");
    Console.ReadLine();
    ```

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához

Most már készen áll a toorun hello alkalmazások áll.

1. A Visual Studióban, kattintson a jobb gombbal a megoldás, és válassza ki **állítsa be indítási projektek**. Válassza ki **több kezdőprojekt**, majd jelölje be hello **Start** művelet **ReadFileUploadNotification** és **SimulatedDevice**.

1. Nyomja le az **F5**. Mindkét alkalmazás elindul. Hello feltöltése befejeződött egy konzolalkalmazás és hello feltöltés értesítési üzenet által fogadott hello más Konzolalkalmazás kell megjelennie. Használhatja a hello [Azure-portálon] vagy Visual Studio Server Explorer toocheck hello hello jelenlétének feltöltött fájl az Azure Storage-fiókban.

    ![][50]

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

<!-- Links -->

[Azure-portálon]: https://portal.azure.com/

[Azure IoT fejlesztői központ]: http://www.azure.com/develop/iot

[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure IoT szolgáltatás SDK NuGet-csomag]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md
