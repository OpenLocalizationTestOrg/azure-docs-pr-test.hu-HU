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
# <a name="send-messages-from-hello-cloud-tooyour-device-with-iot-hub-net"></a>Üzenetek küldése eszközről hello felhő tooyour az IoT Hub (.NET)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Bevezetés
Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere. Hello [Ismerkedés az IoT-központ] az oktatóanyag bemutatja, hogyan toocreate az IoT-központ telepítéséhez azt egy eszközidentitás, és kód egy eszköz-alkalmazást, amelyet az eszköz a felhőbe küldött üzeneteket küld.

Ez az oktatóanyag épül [Ismerkedés az IoT-központ]. Megmutatja, hogyan számára:

* A megoldás háttérrendszeréhez küld felhő-eszközre küldött üzenetek tooa egyetlen eszközt az IoT-központ keresztül.
* Az eszközön a felhőből eszközre üzeneteket fogadni.
* A megoldás háttérrendszeréhez, a kérelem kézbesítési nyugtázási (*visszajelzés*) az üzenetek küldése tooa eszközt az IoT-központot.

További információ a felhő-eszközre küldött üzenetek található hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].

Ez az oktatóanyag végén hello két .NET konzol alkalmazások futtatása:

* **SimulatedDevice**, hello alkalmazás létrehozott egy módosított verziója [Ismerkedés az IoT-központ], amely tooyour IoT-központ kapcsolódik, és felhő eszközre üzeneteket fogad.
* **SendCloudToDevice**, amely küld egy felhő-eszközre küldött üzenetek toohello eszközalkalmazás IoT-központ keresztül, és majd megkapja a kézbesítési nyugtázási.

> [!NOTE]
> Az IoT-központ rendelkezik sok eszköz platformok és nyelvek (például C, Java és Javascript) SDK támogatása keresztül [Azure IoT-eszközök SDK-k]. Hogyan tooconnect az eszköz toothis oktatóanyag kódot, és általában tooAzure IoT Hub: lépésenkénti hello [IoT Hub fejlesztői útmutató].
> 
> 

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Visual Studio 2015-öt vagy a Visual Studio 2017
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

## <a name="receive-messages-in-hello-device-app"></a>Hello eszköz alkalmazásban az üzenetek fogadásához
Ebben a szakaszban létrehozott hello eszközalkalmazás fogja módosítani [Ismerkedés az IoT-központ] hello IoT-központ üzeneteit tooreceive felhő eszközre.

1. A Visual Studio, a hello **SimulatedDevice** projektre, adja hozzá a következő metódus toohello hello **Program** osztály.
   
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
   
    Hello `ReceiveAsync` metódus aszinkron módon adja vissza hello fogadott üzenet hello időpontban hello eszköz érkezik. Adja vissza, *null* specifiable időtúllépés idő után (ebben az esetben egy percen hello rendszer az alapértelmezett használja). Ha hello app kap egy *null*, új üzenetek toowait továbbra is. Ez a követelmény oka hello hello `if (receivedMessage == null) continue` sor.
   
    hívás túl hello`CompleteAsync()` értesíti az IoT-központ hello az üzenet feldolgozása sikeresen megtörtént. üdvözlőüzenetére biztonságosan hello eszköz várólista eltávolítható. Ha adott megakadályozta abban hello eszközalkalmazás hello üzenet hello feldolgozási befejezését történt, az IoT-központ továbbítja újra. Majd fontos, hogy az üzenet hello eszközalkalmazás lévő logika feldolgozási *idempotent*, hogy ugyanaz az üzenet több alkalommal hoz létre hello fogadása hello ugyanazt az eredményt. Egy alkalmazás átmenetileg is abandon egy üzenetet, amely az IoT-központot, megtartja a jövőbeni felhasználásához hello várólista üdvözlőüzenetére eredményez. Vagy hello alkalmazás elutasíthatja egy üzenetet, amely véglegesen eltávolítja a üdvözlőüzenetére hello sorból. Hello felhő-eszközre küldött üzenetek életciklus kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].
   
   > [!NOTE]
   > Ha HTTP helyett MQTT vagy AMQP szolgáltatást átviteli eszközként, hello `ReceiveAsync` metódus azonnal visszaadja. hello támogatott mintát felhő eszközre üzenetek HTTP-vel ritkán üzenetek (kevesebb mint 25 percenként) ellenőrizze, hogy időnként csatlakoztatott eszközön. Az IoT-központ szabályozási hello kérelmek eredmények további HTTP kiállító fogadása. MQTT, amqp-t és HTTP-támogatás, és az IoT-központ sávszélesség-szabályozás hello különbségei kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].
   > 
   > 
2. Adja hozzá a következő metódus a hello hello **fő** metódus közvetlenül előtt hello `Console.ReadLine()` sor:
   
        ReceiveC2dAsync();

> [!NOTE]
> Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].
> 
> 

## <a name="send-a-cloud-to-device-message"></a>Felhő eszközre üzenet küldése
Ebben a szakaszban egy .NET-Konzolalkalmazás által a felhő-eszközre küldött üzenetek toohello eszköz alkalmazás írása.

1. Hello jelenlegi Visual Studio megoldás, hozzon létre egy Visual C# Asztalialkalmazás-projektet hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **SendCloudToDevice**.
   
    ![A Visual Studio új projekt][20]
2. A Megoldáskezelőben kattintson a jobb gombbal a hello megoldás, és kattintson **NuGet-csomagok kezelése megoldáshoz...** . 
   
    Ez a művelet megnyitja hello **NuGet-csomagok kezelése** ablak.
3. Keresse meg **Microsoft.Azure.Devices**, kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello. 
   
    Ez letölti, telepíti, és hozzáad egy hivatkozást toohello [Azure IoT szolgáltatás SDK NuGet-csomag].

4. Adja hozzá a következő hello `using` hello hello tetején utasítás **Program.cs** fájlt:
   
        using Microsoft.Azure.Devices;
5. Adja hozzá a következő mezők toohello hello **Program** osztály. Helyettesítse be hello helyőrző értékének hello IoT hub kapcsolati karakterláncnak a következőről [Ismerkedés az IoT-központ]:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Adja hozzá a következő metódus toohello hello **Program** osztály:
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud toodevice message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    Ez a módszer küldi hello azonosítójú új felhő-eszközre küldött üzenetek toohello eszköz `myFirstDevice`. Módosítsa ezt a paramétert csak akkor, ha módosította az egyik használt hello [Ismerkedés az IoT-központ].
7. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key toosend a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. A Visual studióban, kattintson a jobb gombbal a megoldás, és válassza **állítsa be indítási projektek...** . Válassza ki **több kezdőprojekt**, majd jelölje be hello **Start** művelet **ReadDeviceToCloudMessages**, **SimulatedDevice**, és **SendCloudToDevice**.
9. Nyomja le az **F5**. Mindhárom alkalmazás elindul. Jelölje be hello **SendCloudToDevice** windows, és nyomja le az **Enter**. Hello eszköz alkalmazás által fogadott üdvözlőüzenetére kell megjelennie.
   
   ![A fogadó alkalmazás üzenet][21]

## <a name="receive-delivery-feedback"></a>Kézbesítési visszajelzést kap
Már lehetséges toorequest kézbesítési (vagy lejárati) nyugták az IoT-központ minden felhő eszközre üzenethez. Ez a beállítás lehetővé teszi, hogy hello megoldás háttér tooeasily újra gombra vagy a kompenzáció logika tájékoztatja. Felhő eszközre visszajelzés kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].

Ebben a szakaszban módosítsa hello **SendCloudToDevice** alkalmazás-visszajelzési toorequest, és az IoT-központ fogadása.

1. A Visual Studio, a hello **SendCloudToDevice** projektre, adja hozzá a következő metódus toohello hello **Program** osztály.
   
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
   
    Megjegyzés: a receive mintát van hello ugyanazon egy használt tooreceive felhő eszközre üzenetek hello eszköz alkalmazásból.
2. Adja hozzá a következő metódus a hello hello **fő** metódus után hello `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` sor:
   
        ReceiveFeedbackAsync();
3. a felhő eszközre üzenet kézbesítése hello toorequest visszajelzése, vannak toospecify tulajdonság hello **SendCloudToDeviceMessageAsync** metódust. Adja hozzá a sor jobb a hello után következő hello `var commandMessage = new Message(...);` sor:
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. Hello alkalmazásainak futtatásához billentyűkombináció lenyomásával **F5**. Meg kell jelennie a start mindhárom alkalmazás. Jelölje be hello **SendCloudToDevice** windows, és nyomja le az **Enter**. Üzenet fogadása hello eszköz alkalmazás, és néhány másodpercen belül, visszajelzés üzenet beérkező hello hello kell megjelennie a **SendCloudToDevice** alkalmazás.
   
   ![A fogadó alkalmazás üzenet][22]

> [!NOTE]
> Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].
> 
> 

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban, megtudta, hogyan toosend és felhő-eszközre küldött üzenetek fogadására. 

teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite].

További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].

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