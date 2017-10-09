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
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub-using-net"></a><span data-ttu-id="1c9ea-104">Az eszköz toohello felhőből fájlok feltöltése az IoT hubbal .NET használatával</span><span class="sxs-lookup"><span data-stu-id="1c9ea-104">Upload files from your device toohello cloud with IoT Hub using .NET</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="1c9ea-105">Ez az oktatóanyag épít, hello hello kód [IoT Hub-felhő eszközre üzenetek](iot-hub-csharp-csharp-c2d.md) oktatóanyag tooshow, hogyan toouse hello fájl feltöltése képességeit az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-105">This tutorial builds on hello code in hello [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial tooshow you how toouse hello file upload capabilities of IoT Hub.</span></span> <span data-ttu-id="1c9ea-106">Megmutatja, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-106">It shows you how to:</span></span>

- <span data-ttu-id="1c9ea-107">Biztonságosan adjon meg egy eszközt az Azure blob URI-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="1c9ea-108">Az IoT-központ feltöltés értesítések tootrigger feldolgozása hello fájl az alkalmazás háttérbeli hello használata.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-108">Use hello IoT Hub file upload notifications tootrigger processing hello file in your app back end.</span></span>

<span data-ttu-id="1c9ea-109">Hello [Ismerkedés az IoT-központ](iot-hub-csharp-csharp-getstarted.md) és [IoT Hub-felhő eszközre üzenetek](iot-hub-csharp-csharp-c2d.md) oktatóanyagok hello alapvető eszköz-felhő és a felhő eszközre üzenetkezelési funkcióit az IoT-központ megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-109">hello [Get started with IoT Hub](iot-hub-csharp-csharp-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorials show hello basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="1c9ea-110">Hello [folyamat eszköz felhőbe küldött üzeneteket](iot-hub-csharp-csharp-process-d2c.md) az oktatóanyag leírja a módon tooreliably tároló eszközről a felhőbe üzeneteket az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-110">hello [Process Device-to-Cloud messages](iot-hub-csharp-csharp-process-d2c.md) tutorial describes a way tooreliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="1c9ea-111">Bizonyos esetekben azonban leképezése nem hello adatokat az eszközök elküldik üzenetbe hello viszonylag kis eszközről a felhőbe, amely az IoT-központ fogadja könnyen.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-111">However, in some scenarios you cannot easily map hello data your devices send into hello relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="1c9ea-112">Példa:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-112">For example:</span></span>

* <span data-ttu-id="1c9ea-113">Nagy fájlok, amelyek képeket</span><span class="sxs-lookup"><span data-stu-id="1c9ea-113">Large files that contain images</span></span>
* <span data-ttu-id="1c9ea-114">Videók</span><span class="sxs-lookup"><span data-stu-id="1c9ea-114">Videos</span></span>
* <span data-ttu-id="1c9ea-115">Nagy gyakoriságú lekérdező vibráció adatok</span><span class="sxs-lookup"><span data-stu-id="1c9ea-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="1c9ea-116">Néhány előre feldolgozott adatok formája</span><span class="sxs-lookup"><span data-stu-id="1c9ea-116">Some form of preprocessed data</span></span>

<span data-ttu-id="1c9ea-117">Ezek a fájlok jellemzően hello felhő eszközökkel, mint a feldolgozott kötegelt [Azure Data Factory](../data-factory/index.md) vagy hello [Hadoop](../hdinsight/index.md) verem.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-117">These files are typically batch processed in hello cloud using tools such as [Azure Data Factory](../data-factory/index.md) or hello [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="1c9ea-118">Ha egy eszközről tooupload fájlok van szüksége, hello biztonsága és megbízhatósága szempontjából az IoT-központ továbbra is használhatja.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-118">When you need tooupload files from a device, you can still use hello security and reliability of IoT Hub.</span></span>

<span data-ttu-id="1c9ea-119">Ez az oktatóanyag végén hello két .NET konzol alkalmazások futtatása:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-119">At hello end of this tutorial you run two .NET console apps:</span></span>

* <span data-ttu-id="1c9ea-120">**SimulatedDevice**, hello alkalmazás hello létrehozott egy módosított verziója [IoT Hub-felhő eszközre üzenetek](iot-hub-csharp-csharp-c2d.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-120">**SimulatedDevice**, a modified version of hello app created in hello [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial.</span></span> <span data-ttu-id="1c9ea-121">Ez az alkalmazás feltölt egy fájlt toostorage az IoT hub által biztosított SAS URI-k használata.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-121">This app uploads a file toostorage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="1c9ea-122">**ReadFileUploadNotification**, amely fájl feltöltése értesítéseket fogad az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-122">**ReadFileUploadNotification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="1c9ea-123">Az IoT-központ Azure IoT-eszközök SDK-k segítségével számos eszköz platformok és nyelvek (például C, Java és Javascript) támogatja.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-123">IoT Hub supports many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="1c9ea-124">Tekintse meg a toohello [Azure IoT fejlesztői központ] részletesen ismerteti a tooconnect az eszköz tooAzure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-124">Refer toohello [Azure IoT Developer Center] for step-by-step instructions on how tooconnect your device tooAzure IoT Hub.</span></span>

<span data-ttu-id="1c9ea-125">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-125">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="1c9ea-126">Visual Studio 2015-öt vagy a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1c9ea-126">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="1c9ea-127">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-127">An active Azure account.</span></span> <span data-ttu-id="1c9ea-128">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="1c9ea-128">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="1c9ea-129">Egy eszköz alkalmazás-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="1c9ea-129">Upload a file from a device app</span></span>

<span data-ttu-id="1c9ea-130">Ebben a szakaszban létrehozott hello eszközalkalmazás módosítása [IoT Hub-felhő eszközre üzenetek](iot-hub-csharp-csharp-c2d.md) hello IoT-központ üzeneteit tooreceive felhő eszközre.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-130">In this section, you modify hello device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="1c9ea-131">A Visual Studióban, kattintson a jobb gombbal hello **SimulatedDevice** projektre, kattintson **Hozzáadás**, és kattintson a **meglévő cikk**.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-131">In Visual Studio, right-click hello **SimulatedDevice** project, click **Add**, and then click **Existing Item**.</span></span> <span data-ttu-id="1c9ea-132">Keresse meg a tooan képfájl, és adja hozzá a projektben.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-132">Navigate tooan image file and include it in your project.</span></span> <span data-ttu-id="1c9ea-133">Ez az oktatóanyag azt feltételezi, hogy hello kép nevű `image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-133">This tutorial assumes hello image is named `image.jpg`.</span></span>

1. <span data-ttu-id="1c9ea-134">Kattintson a jobb gombbal a hello lemezképet, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-134">Right-click on hello image, and then click **Properties**.</span></span> <span data-ttu-id="1c9ea-135">Győződjön meg arról, hogy **tooOutput Directory másolása** értéke túl**mindig másolása**.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-135">Make sure that **Copy tooOutput Directory** is set too**Copy always**.</span></span>

    ![][1]

1. <span data-ttu-id="1c9ea-136">A hello **Program.cs** fájlt, adja hozzá a következő utasítások hello fájl hello tetején hello:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-136">In hello **Program.cs** file, add hello following statements at hello top of hello file:</span></span>

    ```csharp
    using System.IO;
    ```

1. <span data-ttu-id="1c9ea-137">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-137">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="1c9ea-138">Hello `UploadToBlobAsync` módszer veszi a hello fájlnév, és hello fájl toobe adatfolyam forrását feltöltött és hello feltöltés toostorage kezeli.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-138">hello `UploadToBlobAsync` method takes in hello file name and stream source of hello file toobe uploaded and handles hello upload toostorage.</span></span> <span data-ttu-id="1c9ea-139">hello Konzolalkalmazás hello időt tooupload hello fájl jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-139">hello console app displays hello time it takes tooupload hello file.</span></span>

1. <span data-ttu-id="1c9ea-140">Adja hozzá a következő metódus a hello hello **fő** metódus közvetlenül előtt hello `Console.ReadLine()` sor:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-140">Add hello following method in hello **Main** method, right before hello `Console.ReadLine()` line:</span></span>

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> <span data-ttu-id="1c9ea-141">Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-141">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="1c9ea-142">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="1c9ea-142">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="1c9ea-143">Fájl feltöltése értesítést</span><span class="sxs-lookup"><span data-stu-id="1c9ea-143">Receive a file upload notification</span></span>

<span data-ttu-id="1c9ea-144">Ebben a szakaszban egy .NET-Konzolalkalmazás, hogy a fájl feltöltése értesítési üzeneteket fogad az IoT-központ írni.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-144">In this section, you write a .NET console app that receives file upload notification messages from IoT Hub.</span></span>

1. <span data-ttu-id="1c9ea-145">A hello jelenlegi Visual Studio megoldás Visual C# Windows-projekt létrehozása hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-145">In hello current Visual Studio solution, create a Visual C# Windows project by using hello **Console Application** project template.</span></span> <span data-ttu-id="1c9ea-146">Név hello projekt **ReadFileUploadNotification**.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-146">Name hello project **ReadFileUploadNotification**.</span></span>

    ![A Visual Studio új projekt][2]

1. <span data-ttu-id="1c9ea-148">A Megoldáskezelőben kattintson a jobb gombbal hello **ReadFileUploadNotification** projektre, és kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="1c9ea-148">In Solution Explorer, right-click hello **ReadFileUploadNotification** project, and then click **Manage NuGet Packages...**.</span></span>

1. <span data-ttu-id="1c9ea-149">A hello **NuGet-Csomagkezelő** ablakban, keresse meg **Microsoft.Azure.Devices**, kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-149">In hello **NuGet Package Manager** window, search for **Microsoft.Azure.Devices**, click **Install**, and accept hello terms of use.</span></span>

    <span data-ttu-id="1c9ea-150">Ez a művelet tölti le, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK NuGet-csomag] a hello **ReadFileUploadNotification** projekt.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-150">This action downloads, installs, and adds a reference toohello [Azure IoT service SDK NuGet package] in hello **ReadFileUploadNotification** project.</span></span>

1. <span data-ttu-id="1c9ea-151">A hello **Program.cs** fájlt, adja hozzá a következő utasítások hello fájl hello tetején hello:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-151">In hello **Program.cs** file, add hello following statements at hello top of hello file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. <span data-ttu-id="1c9ea-152">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-152">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="1c9ea-153">Helyettesítse be hello helyőrző értékének hello IoT hub kapcsolati karakterláncnak a következőről [Ismerkedés az IoT-központ]:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-153">Substitute hello placeholder value with hello IoT hub connection string from [Get started with IoT Hub]:</span></span>

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. <span data-ttu-id="1c9ea-154">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-154">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="1c9ea-155">Megjegyzés: a receive mintát van hello ugyanazon egy használt tooreceive felhő eszközre üzenetek hello eszköz alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-155">Note this receive pattern is hello same one used tooreceive cloud-to-device messages from hello device app.</span></span>

1. <span data-ttu-id="1c9ea-156">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-156">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter tooexit\n");
    Console.ReadLine();
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="1c9ea-157">Hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="1c9ea-157">Run hello applications</span></span>

<span data-ttu-id="1c9ea-158">Most már készen áll a toorun hello alkalmazások áll.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-158">Now you are ready toorun hello applications.</span></span>

1. <span data-ttu-id="1c9ea-159">A Visual Studióban, kattintson a jobb gombbal a megoldás, és válassza ki **állítsa be indítási projektek**.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-159">In Visual Studio, right-click your solution, and select **Set StartUp projects**.</span></span> <span data-ttu-id="1c9ea-160">Válassza ki **több kezdőprojekt**, majd jelölje be hello **Start** művelet **ReadFileUploadNotification** és **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-160">Select **Multiple startup projects**, then select hello **Start** action for **ReadFileUploadNotification** and **SimulatedDevice**.</span></span>

1. <span data-ttu-id="1c9ea-161">Nyomja le az **F5**.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-161">Press **F5**.</span></span> <span data-ttu-id="1c9ea-162">Mindkét alkalmazás elindul.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-162">Both applications should start.</span></span> <span data-ttu-id="1c9ea-163">Hello feltöltése befejeződött egy konzolalkalmazás és hello feltöltés értesítési üzenet által fogadott hello más Konzolalkalmazás kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-163">You should see hello upload completed in one console app and hello upload notification message received by hello other console app.</span></span> <span data-ttu-id="1c9ea-164">Használhatja a hello [Azure-portálon] vagy Visual Studio Server Explorer toocheck hello hello jelenlétének feltöltött fájl az Azure Storage-fiókban.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-164">You can use hello [Azure portal] or Visual Studio Server Explorer toocheck for hello presence of hello uploaded file in your Azure Storage account.</span></span>

    ![][50]

## <a name="next-steps"></a><span data-ttu-id="1c9ea-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1c9ea-165">Next steps</span></span>

<span data-ttu-id="1c9ea-166">Ebben az oktatóanyagban megtanulta, hogyan toouse hello fájlfeltöltés toosimplify fájlfeltöltések eszközökről az IoT-központ képességeit.</span><span class="sxs-lookup"><span data-stu-id="1c9ea-166">In this tutorial, you learned how toouse hello file upload capabilities of IoT Hub toosimplify file uploads from devices.</span></span> <span data-ttu-id="1c9ea-167">A következő cikkek hello tooexplore IoT hub-szolgáltatások és a forgatókönyv folytatása:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-167">You can continue tooexplore IoT hub features and scenarios with hello following articles:</span></span>

* <span data-ttu-id="1c9ea-168">[Programozott módon létrehoz egy IoT-központot][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="1c9ea-168">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="1c9ea-169">[Bevezetés tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="1c9ea-169">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="1c9ea-170">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="1c9ea-170">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="1c9ea-171">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="1c9ea-171">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="1c9ea-172">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="1c9ea-172">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

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
