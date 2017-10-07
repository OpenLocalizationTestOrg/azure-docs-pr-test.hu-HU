---
title: "aaaCloud eszközre üzenetek Azure IoT Hub (.NET) |} Microsoft Docs"
description: "Hogyan toosend felhő eszköz üzenetek tooa eszköz regisztrációját az Azure IoT-központ hello Azure IoT SDK-k használata a .NET-hez. Módosítja egy eszközre app tooreceive felhőből eszközre küldött üzenetek, és módosítsa a háttér-alkalmazás toosend hello felhő eszközre üzeneteket."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: a31c05ed-6ec0-40f3-99ab-8fdd28b1a89a
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: f6a7618b164d95c8ddaf28943f244aeeb568217f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-messages-from-hello-cloud-tooyour-device-with-iot-hub-net"></a><span data-ttu-id="cdb81-104">Üzenetek küldése eszközről hello felhő tooyour az IoT Hub (.NET)</span><span class="sxs-lookup"><span data-stu-id="cdb81-104">Send messages from hello cloud tooyour device with IoT Hub (.NET)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="cdb81-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="cdb81-105">Introduction</span></span>
<span data-ttu-id="cdb81-106">Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere.</span><span class="sxs-lookup"><span data-stu-id="cdb81-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="cdb81-107">Hello [Ismerkedés az IoT-központ] az oktatóanyag bemutatja, hogyan toocreate az IoT-központ telepítéséhez azt egy eszközidentitás, és kód egy eszköz-alkalmazást, amelyet az eszköz a felhőbe küldött üzeneteket küld.</span><span class="sxs-lookup"><span data-stu-id="cdb81-107">hello [Get started with IoT Hub] tutorial shows how toocreate an IoT hub, provision a device identity in it, and code a device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="cdb81-108">Ez az oktatóanyag épül [Ismerkedés az IoT-központ].</span><span class="sxs-lookup"><span data-stu-id="cdb81-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="cdb81-109">Megmutatja, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="cdb81-109">It shows you how to:</span></span>

* <span data-ttu-id="cdb81-110">A megoldás háttérrendszeréhez küld felhő-eszközre küldött üzenetek tooa egyetlen eszközt az IoT-központ keresztül.</span><span class="sxs-lookup"><span data-stu-id="cdb81-110">From your solution back end, send cloud-to-device messages tooa single device through IoT Hub.</span></span>
* <span data-ttu-id="cdb81-111">Az eszközön a felhőből eszközre üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="cdb81-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="cdb81-112">A megoldás háttérrendszeréhez, a kérelem kézbesítési nyugtázási (*visszajelzés*) az üzenetek küldése tooa eszközt az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="cdb81-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent tooa device from IoT Hub.</span></span>

<span data-ttu-id="cdb81-113">További információ a felhő-eszközre küldött üzenetek található hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="cdb81-113">You can find more information on cloud-to-device messages in hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="cdb81-114">Ez az oktatóanyag végén hello két .NET konzol alkalmazások futtatása:</span><span class="sxs-lookup"><span data-stu-id="cdb81-114">At hello end of this tutorial, you run two .NET console apps:</span></span>

* <span data-ttu-id="cdb81-115">**SimulatedDevice**, hello alkalmazás létrehozott egy módosított verziója [Ismerkedés az IoT-központ], amely tooyour IoT-központ kapcsolódik, és felhő eszközre üzeneteket fogad.</span><span class="sxs-lookup"><span data-stu-id="cdb81-115">**SimulatedDevice**, a modified version of hello app created in [Get started with IoT Hub], which connects tooyour IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="cdb81-116">**SendCloudToDevice**, amely küld egy felhő-eszközre küldött üzenetek toohello eszközalkalmazás IoT-központ keresztül, és majd megkapja a kézbesítési nyugtázási.</span><span class="sxs-lookup"><span data-stu-id="cdb81-116">**SendCloudToDevice**, which sends a cloud-to-device message toohello device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="cdb81-117">Az IoT-központ rendelkezik sok eszköz platformok és nyelvek (például C, Java és Javascript) SDK támogatása keresztül [Azure IoT-eszközök SDK-k].</span><span class="sxs-lookup"><span data-stu-id="cdb81-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through [Azure IoT device SDKs].</span></span> <span data-ttu-id="cdb81-118">Hogyan tooconnect az eszköz toothis oktatóanyag kódot, és általában tooAzure IoT Hub: lépésenkénti hello [IoT Hub fejlesztői útmutató].</span><span class="sxs-lookup"><span data-stu-id="cdb81-118">For step-by-step instructions on how tooconnect your device toothis tutorial's code, and generally tooAzure IoT Hub, see hello [IoT Hub developer guide].</span></span>
> 
> 

<span data-ttu-id="cdb81-119">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="cdb81-119">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="cdb81-120">Visual Studio 2015-öt vagy a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="cdb81-120">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="cdb81-121">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="cdb81-121">An active Azure account.</span></span> <span data-ttu-id="cdb81-122">(Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)</span><span class="sxs-lookup"><span data-stu-id="cdb81-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-hello-device-app"></a><span data-ttu-id="cdb81-123">Hello eszköz alkalmazásban az üzenetek fogadásához</span><span class="sxs-lookup"><span data-stu-id="cdb81-123">Receive messages in hello device app</span></span>
<span data-ttu-id="cdb81-124">Ebben a szakaszban létrehozott hello eszközalkalmazás fogja módosítani [Ismerkedés az IoT-központ] hello IoT-központ üzeneteit tooreceive felhő eszközre.</span><span class="sxs-lookup"><span data-stu-id="cdb81-124">In this section, you'll modify hello device app you created in [Get started with IoT Hub] tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="cdb81-125">A Visual Studio, a hello **SimulatedDevice** projektre, adja hozzá a következő metódus toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="cdb81-125">In Visual Studio, in hello **SimulatedDevice** project, add hello following method toohello **Program** class.</span></span>
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud toodevice messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    <span data-ttu-id="cdb81-126">Hello `ReceiveAsync` metódus aszinkron módon adja vissza hello fogadott üzenet hello időpontban hello eszköz érkezik.</span><span class="sxs-lookup"><span data-stu-id="cdb81-126">hello `ReceiveAsync` method asynchronously returns hello received message at hello time that it is received by hello device.</span></span> <span data-ttu-id="cdb81-127">Adja vissza, *null* specifiable időtúllépés idő után (ebben az esetben egy percen hello rendszer az alapértelmezett használja).</span><span class="sxs-lookup"><span data-stu-id="cdb81-127">It returns *null* after a specifiable timeout period (in this case, hello default of one minute is used).</span></span> <span data-ttu-id="cdb81-128">Ha hello app kap egy *null*, új üzenetek toowait továbbra is.</span><span class="sxs-lookup"><span data-stu-id="cdb81-128">When hello app receives a *null*, it should continue toowait for new messages.</span></span> <span data-ttu-id="cdb81-129">Ez a követelmény oka hello hello `if (receivedMessage == null) continue` sor.</span><span class="sxs-lookup"><span data-stu-id="cdb81-129">This requirement is hello reason for hello `if (receivedMessage == null) continue` line.</span></span>
   
    <span data-ttu-id="cdb81-130">hívás túl hello`CompleteAsync()` értesíti az IoT-központ hello az üzenet feldolgozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="cdb81-130">hello call too`CompleteAsync()` notifies IoT Hub that hello message has been successfully processed.</span></span> <span data-ttu-id="cdb81-131">üdvözlőüzenetére biztonságosan hello eszköz várólista eltávolítható.</span><span class="sxs-lookup"><span data-stu-id="cdb81-131">hello message can be safely removed from hello device queue.</span></span> <span data-ttu-id="cdb81-132">Ha adott megakadályozta abban hello eszközalkalmazás hello üzenet hello feldolgozási befejezését történt, az IoT-központ továbbítja újra.</span><span class="sxs-lookup"><span data-stu-id="cdb81-132">If something happened that prevented hello device app from completing hello processing of hello message, IoT Hub delivers it again.</span></span> <span data-ttu-id="cdb81-133">Majd fontos, hogy az üzenet hello eszközalkalmazás lévő logika feldolgozási *idempotent*, hogy ugyanaz az üzenet több alkalommal hoz létre hello fogadása hello ugyanazt az eredményt.</span><span class="sxs-lookup"><span data-stu-id="cdb81-133">It is then important that message processing logic in hello device app is *idempotent*, so that receiving hello same message multiple times produces hello same result.</span></span> <span data-ttu-id="cdb81-134">Egy alkalmazás átmenetileg is abandon egy üzenetet, amely az IoT-központot, megtartja a jövőbeni felhasználásához hello várólista üdvözlőüzenetére eredményez.</span><span class="sxs-lookup"><span data-stu-id="cdb81-134">An application can also temporarily abandon a message, which results in IoT hub retaining hello message in hello queue for future consumption.</span></span> <span data-ttu-id="cdb81-135">Vagy hello alkalmazás elutasíthatja egy üzenetet, amely véglegesen eltávolítja a üdvözlőüzenetére hello sorból.</span><span class="sxs-lookup"><span data-stu-id="cdb81-135">Or, hello application can reject a message, which permanently removes hello message from hello queue.</span></span> <span data-ttu-id="cdb81-136">Hello felhő-eszközre küldött üzenetek életciklus kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="cdb81-136">For more information about hello cloud-to-device message lifecycle, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="cdb81-137">Ha HTTP helyett MQTT vagy AMQP szolgáltatást átviteli eszközként, hello `ReceiveAsync` metódus azonnal visszaadja.</span><span class="sxs-lookup"><span data-stu-id="cdb81-137">When using HTTP instead of MQTT or AMQP as a transport, hello `ReceiveAsync` method returns immediately.</span></span> <span data-ttu-id="cdb81-138">hello támogatott mintát felhő eszközre üzenetek HTTP-vel ritkán üzenetek (kevesebb mint 25 percenként) ellenőrizze, hogy időnként csatlakoztatott eszközön.</span><span class="sxs-lookup"><span data-stu-id="cdb81-138">hello supported pattern for cloud-to-device messages with HTTP is intermittently connected devices that check for messages infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="cdb81-139">Az IoT-központ szabályozási hello kérelmek eredmények további HTTP kiállító fogadása.</span><span class="sxs-lookup"><span data-stu-id="cdb81-139">Issuing more HTTP receives results in IoT Hub throttling hello requests.</span></span> <span data-ttu-id="cdb81-140">MQTT, amqp-t és HTTP-támogatás, és az IoT-központ sávszélesség-szabályozás hello különbségei kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="cdb81-140">For more information about hello differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 
2. <span data-ttu-id="cdb81-141">Adja hozzá a következő metódus a hello hello **fő** metódus közvetlenül előtt hello `Console.ReadLine()` sor:</span><span class="sxs-lookup"><span data-stu-id="cdb81-141">Add hello following method in hello **Main** method, right before hello `Console.ReadLine()` line:</span></span>
   
        ReceiveC2dAsync();

> [!NOTE]
> <span data-ttu-id="cdb81-142">Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="cdb81-142">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="cdb81-143">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="cdb81-143">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="cdb81-144">Felhő eszközre üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="cdb81-144">Send a cloud-to-device message</span></span>
<span data-ttu-id="cdb81-145">Ebben a szakaszban egy .NET-Konzolalkalmazás által a felhő-eszközre küldött üzenetek toohello eszköz alkalmazás írása.</span><span class="sxs-lookup"><span data-stu-id="cdb81-145">In this section, you write a .NET console app that sends cloud-to-device messages toohello device app.</span></span>

1. <span data-ttu-id="cdb81-146">Hello jelenlegi Visual Studio megoldás, hozzon létre egy Visual C# Asztalialkalmazás-projektet hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="cdb81-146">In hello current Visual Studio solution, create a Visual C# Desktop App project by using hello **Console Application** project template.</span></span> <span data-ttu-id="cdb81-147">Név hello projekt **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="cdb81-147">Name hello project **SendCloudToDevice**.</span></span>
   
    ![A Visual Studio új projekt][20]
2. <span data-ttu-id="cdb81-149">A Megoldáskezelőben kattintson a jobb gombbal a hello megoldás, és kattintson **NuGet-csomagok kezelése megoldáshoz...** .</span><span class="sxs-lookup"><span data-stu-id="cdb81-149">In Solution Explorer, right-click hello solution, and then click **Manage NuGet Packages for Solution...**.</span></span> 
   
    <span data-ttu-id="cdb81-150">Ez a művelet megnyitja hello **NuGet-csomagok kezelése** ablak.</span><span class="sxs-lookup"><span data-stu-id="cdb81-150">This action opens hello **Manage NuGet Packages** window.</span></span>
3. <span data-ttu-id="cdb81-151">Keresse meg **Microsoft.Azure.Devices**, kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="cdb81-151">Search for **Microsoft.Azure.Devices**, click **Install**, and accept hello terms of use.</span></span> 
   
    <span data-ttu-id="cdb81-152">Ez letölti, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK NuGet-csomag].</span><span class="sxs-lookup"><span data-stu-id="cdb81-152">This downloads, installs, and adds a reference toohello [Azure IoT service SDK NuGet package].</span></span>

4. <span data-ttu-id="cdb81-153">Adja hozzá a következő hello `using` hello hello tetején utasítás **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="cdb81-153">Add hello following `using` statement at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="cdb81-154">Adja hozzá a következő mezők toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="cdb81-154">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="cdb81-155">Helyettesítse be hello helyőrző értékének hello IoT hub kapcsolati karakterláncnak a következőről [Ismerkedés az IoT-központ]:</span><span class="sxs-lookup"><span data-stu-id="cdb81-155">Substitute hello placeholder value with hello IoT hub connection string from [Get started with IoT Hub]:</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="cdb81-156">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="cdb81-156">Add hello following method toohello **Program** class:</span></span>
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud toodevice message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    <span data-ttu-id="cdb81-157">Ez a módszer küldi hello azonosítójú új felhő-eszközre küldött üzenetek toohello eszköz `myFirstDevice`.</span><span class="sxs-lookup"><span data-stu-id="cdb81-157">This method sends a new cloud-to-device message toohello device with hello ID, `myFirstDevice`.</span></span> <span data-ttu-id="cdb81-158">Módosítsa ezt a paramétert csak akkor, ha módosította az egyik használt hello [Ismerkedés az IoT-központ].</span><span class="sxs-lookup"><span data-stu-id="cdb81-158">Change this parameter only if you modified it from hello one used in [Get started with IoT Hub].</span></span>
7. <span data-ttu-id="cdb81-159">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="cdb81-159">Finally, add hello following lines toohello **Main** method:</span></span>
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key toosend a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="cdb81-160">A Visual studióban, kattintson a jobb gombbal a megoldás, és válassza **állítsa be indítási projektek...** . Válassza ki **több kezdőprojekt**, majd jelölje be hello **Start** művelet **ReadDeviceToCloudMessages**, **SimulatedDevice**, és **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="cdb81-160">From within Visual Studio, right-click your solution, and select **Set StartUp projects...**. Select **Multiple startup projects**, then select hello **Start** action for **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **SendCloudToDevice**.</span></span>
9. <span data-ttu-id="cdb81-161">Nyomja le az **F5**.</span><span class="sxs-lookup"><span data-stu-id="cdb81-161">Press **F5**.</span></span> <span data-ttu-id="cdb81-162">Mindhárom alkalmazás elindul.</span><span class="sxs-lookup"><span data-stu-id="cdb81-162">All three applications should start.</span></span> <span data-ttu-id="cdb81-163">Jelölje be hello **SendCloudToDevice** windows, és nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="cdb81-163">Select hello **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="cdb81-164">Hello eszköz alkalmazás által fogadott üdvözlőüzenetére kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="cdb81-164">You should see hello message being received by hello device app.</span></span>
   
   ![A fogadó alkalmazás üzenet][21]

## <a name="receive-delivery-feedback"></a><span data-ttu-id="cdb81-166">Kézbesítési visszajelzést kap</span><span class="sxs-lookup"><span data-stu-id="cdb81-166">Receive delivery feedback</span></span>
<span data-ttu-id="cdb81-167">Már lehetséges toorequest kézbesítési (vagy lejárati) nyugták az IoT-központ minden felhő eszközre üzenethez.</span><span class="sxs-lookup"><span data-stu-id="cdb81-167">It is possible toorequest delivery (or expiration) acknowledgements from IoT Hub for each cloud-to-device message.</span></span> <span data-ttu-id="cdb81-168">Ez a beállítás lehetővé teszi, hogy hello megoldás háttér tooeasily újra gombra vagy a kompenzáció logika tájékoztatja.</span><span class="sxs-lookup"><span data-stu-id="cdb81-168">This option enables hello solution back end tooeasily inform retry or compensation logic.</span></span> <span data-ttu-id="cdb81-169">Felhő eszközre visszajelzés kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="cdb81-169">For more information about cloud-to-device feedback, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="cdb81-170">Ebben a szakaszban módosítsa hello **SendCloudToDevice** alkalmazás-visszajelzési toorequest, és az IoT-központ fogadása.</span><span class="sxs-lookup"><span data-stu-id="cdb81-170">In this section, you modify hello **SendCloudToDevice** app toorequest feedback, and receive it from IoT Hub.</span></span>

1. <span data-ttu-id="cdb81-171">A Visual Studio, a hello **SendCloudToDevice** projektre, adja hozzá a következő metódus toohello hello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="cdb81-171">In Visual Studio, in hello **SendCloudToDevice** project, add hello following method toohello **Program** class.</span></span>
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    <span data-ttu-id="cdb81-172">Megjegyzés: a receive mintát van hello ugyanazon egy használt tooreceive felhő eszközre üzenetek hello eszköz alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="cdb81-172">Note this receive pattern is hello same one used tooreceive cloud-to-device messages from hello device app.</span></span>
2. <span data-ttu-id="cdb81-173">Adja hozzá a következő metódus a hello hello **fő** metódus után hello `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` sor:</span><span class="sxs-lookup"><span data-stu-id="cdb81-173">Add hello following method in hello **Main** method, right after hello `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` line:</span></span>
   
        ReceiveFeedbackAsync();
3. <span data-ttu-id="cdb81-174">a felhő eszközre üzenet kézbesítése hello toorequest visszajelzése, vannak toospecify tulajdonság hello **SendCloudToDeviceMessageAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="cdb81-174">toorequest feedback for hello delivery of your cloud-to-device message, you have toospecify a property in hello **SendCloudToDeviceMessageAsync** method.</span></span> <span data-ttu-id="cdb81-175">Adja hozzá a sor jobb a hello után következő hello `var commandMessage = new Message(...);` sor:</span><span class="sxs-lookup"><span data-stu-id="cdb81-175">Add hello following line, right after hello `var commandMessage = new Message(...);` line:</span></span>
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. <span data-ttu-id="cdb81-176">Hello alkalmazásainak futtatásához billentyűkombináció lenyomásával **F5**.</span><span class="sxs-lookup"><span data-stu-id="cdb81-176">Run hello apps by pressing **F5**.</span></span> <span data-ttu-id="cdb81-177">Meg kell jelennie a start mindhárom alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cdb81-177">You should see all three applications start.</span></span> <span data-ttu-id="cdb81-178">Jelölje be hello **SendCloudToDevice** windows, és nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="cdb81-178">Select hello **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="cdb81-179">Üzenet fogadása hello eszköz alkalmazás, és néhány másodpercen belül, visszajelzés üzenet beérkező hello hello kell megjelennie a **SendCloudToDevice** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cdb81-179">You should see hello message being received by hello device app, and after a few seconds, hello feedback message being received by your **SendCloudToDevice** application.</span></span>
   
   ![A fogadó alkalmazás üzenet][22]

> [!NOTE]
> <span data-ttu-id="cdb81-181">Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="cdb81-181">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="cdb81-182">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].</span><span class="sxs-lookup"><span data-stu-id="cdb81-182">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="cdb81-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cdb81-183">Next steps</span></span>
<span data-ttu-id="cdb81-184">Ebben az oktatóanyagban, megtudta, hogyan toosend és felhő-eszközre küldött üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="cdb81-184">In this tutorial, you learned how toosend and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="cdb81-185">teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="cdb81-185">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="cdb81-186">További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].</span><span class="sxs-lookup"><span data-stu-id="cdb81-186">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT szolgáltatás SDK NuGet-csomag]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md

[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md
[Ismerkedés az IoT-központ]: iot-hub-csharp-csharp-getstarted.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://docs.microsoft.com/en-us/azure/iot-suite/
[Azure IoT-eszközök SDK-k]: iot-hub-devguide-sdks.md