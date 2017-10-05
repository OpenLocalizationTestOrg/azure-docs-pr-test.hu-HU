---
title: "Az Azure IoT-eszközök SDK c - IoTHubClient |} Microsoft Docs"
description: "Hogyan használható a IoTHubClient könyvtár, az Azure IoT eszközben C-hez készült SDK kommunikáló eszközön futó alkalmazások létrehozásához az IoT-központ számára."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: 828cf2bf-999d-4b8a-8a28-c7c901629600
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2016
ms.author: obloch
ms.openlocfilehash: 422d89014511f0d08ba57a893570ff7b253b7bc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a><span data-ttu-id="50838-103">Az Azure IoT-eszközök SDK c – további információ az IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="50838-103">Azure IoT device SDK for C – more about IoTHubClient</span></span>
<span data-ttu-id="50838-104">A [először a következő cikket:](iot-hub-device-sdk-c-intro.md) a sorozat bevezette a **C-hez készült SDK Azure IoT-eszközök**. A cikk alapján, hogy nincsenek-e két architekturális rétegek SDK.</span><span class="sxs-lookup"><span data-stu-id="50838-104">The [first article](iot-hub-device-sdk-c-intro.md) in this series introduced the **Azure IoT device SDK for C**. That article explained that there are two architectural layers in SDK.</span></span> <span data-ttu-id="50838-105">Az alap van a **IoTHubClient** könyvtárban, amely közvetlenül kezeli az IoT-központ folytatott kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="50838-105">At the base is the **IoTHubClient** library which directly manages communication with IoT Hub.</span></span> <span data-ttu-id="50838-106">Szerepel továbbá a **szerializáló** könyvtár épülő a szerializálási szolgáltatások biztosításához.</span><span class="sxs-lookup"><span data-stu-id="50838-106">There's also the **serializer** library that builds on top of that to provide serialization services.</span></span> <span data-ttu-id="50838-107">Ebben a cikkben a következőkhöz nyújtunk további részletek a a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="50838-107">In this article we'll provide additional detail on the **IoTHubClient** library.</span></span>

<span data-ttu-id="50838-108">Az előző cikkben leírt használata a **IoTHubClient** könyvtár események küldése az IoT-központ és az üzenetek fogadásához.</span><span class="sxs-lookup"><span data-stu-id="50838-108">The previous article described how to use the **IoTHubClient** library to send events to IoT Hub and receive messages.</span></span> <span data-ttu-id="50838-109">Ez a cikk e kiterjeszti pontosabban kezelése ismertető *amikor* küldött és fogadott adatok bemutatása, hogy a **alacsonyabb szintű API-k**.</span><span class="sxs-lookup"><span data-stu-id="50838-109">This article extends that discussion by explaining how to more precisely manage *when* you send and receive data, introducing you to the **lower-level APIs**.</span></span> <span data-ttu-id="50838-110">Azt is ismertetjük tulajdonságok csatolható események (és üzenetek lekérése őket) szolgáltatások kezelése tulajdonság használatával a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="50838-110">We'll also explain how to attach properties to events (and retrieve them from messages) using the property handling features in the **IoTHubClient** library.</span></span> <span data-ttu-id="50838-111">Végül az IoT-központ fogadott üzenetek kezelésének különböző módjai további magyarázata a következőkhöz nyújtunk.</span><span class="sxs-lookup"><span data-stu-id="50838-111">Finally, we'll provide additional explanation of different ways to handle messages received from IoT Hub.</span></span>

<span data-ttu-id="50838-112">A cikk arra a következtetésre jut, vegyes témaköreit, beleértve több hitelesítő adatai és működésének módosításához néhány kiterjed a **IoTHubClient** konfigurációs lehetőségek között.</span><span class="sxs-lookup"><span data-stu-id="50838-112">The article concludes by covering a couple of miscellaneous topics, including more about device credentials and how to change the behavior of the **IoTHubClient** through configuration options.</span></span>

<span data-ttu-id="50838-113">Fogjuk használni a **IoTHubClient** SDK minták az alábbi témakörök ismertetik.</span><span class="sxs-lookup"><span data-stu-id="50838-113">We'll use the **IoTHubClient** SDK samples to explain these topics.</span></span> <span data-ttu-id="50838-114">Ha azt szeretné, követéséhez, tekintse meg a **IOT hubbal\_ügyfél\_minta\_http** és **IOT hubbal\_ügyfél\_minta\_amqp** , amely az Azure IoT-eszközök SDK meg a c mindent az alábbi szakaszok ismertetik ezeket a mintákat a bemutatott alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="50838-114">If you want to follow along, see the **iothub\_client\_sample\_http** and **iothub\_client\_sample\_amqp** applications that are included in the Azure IoT device SDK for C. Everything described in the following sections is demonstrated in these samples.</span></span>

<span data-ttu-id="50838-115">Megtalálhatja az [ **C-hez készült SDK Azure IoT-eszközök** ](https://github.com/Azure/azure-iot-sdk-c) GitHub tárház és nézet adatai az API-nak a [C API-referencia](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="50838-115">You can find the [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of the API in the [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="the-lower-level-apis"></a><span data-ttu-id="50838-116">Az alacsonyabb szintű API-k</span><span class="sxs-lookup"><span data-stu-id="50838-116">The lower-level APIs</span></span>
<span data-ttu-id="50838-117">Az előző cikkben leírt alapszintű működését a **IotHubClient** kontextusában a **IOT hubbal\_ügyfél\_minta\_amqp** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="50838-117">The previous article described the basic operation of the **IotHubClient** within the context of the **iothub\_client\_sample\_amqp** application.</span></span> <span data-ttu-id="50838-118">Például azt a könyvtárban, a kód használatával inicializálásával ismertetése.</span><span class="sxs-lookup"><span data-stu-id="50838-118">For example, it explained how to initialize the library using this code.</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="50838-119">Azt is ismerteti, hogyan küldhet e függvényhívás eseményeket.</span><span class="sxs-lookup"><span data-stu-id="50838-119">It also described how to send events using this function call.</span></span>

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

<span data-ttu-id="50838-120">A cikk azt is ismerteti, hogyan regisztrálja a visszahívási függvény üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="50838-120">The article also described how to receive messages by registering a callback function.</span></span>

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

<span data-ttu-id="50838-121">A cikk is bemutatta, hogyan lehet felszabadítani az erőforrásokat, például az alábbi kód használatával.</span><span class="sxs-lookup"><span data-stu-id="50838-121">The article also showed how to free resources using code such as the following.</span></span>

```
IoTHubClient_Destroy(iotHubClientHandle);
```

<span data-ttu-id="50838-122">Azonban ezen API-k mindegyikének kiegészítő funkciók vannak:</span><span class="sxs-lookup"><span data-stu-id="50838-122">However there are companion functions to each of these APIs:</span></span>

* <span data-ttu-id="50838-123">IoTHubClient\_inden\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="50838-123">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="50838-124">IoTHubClient\_inden\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="50838-124">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="50838-125">IoTHubClient\_inden\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="50838-125">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="50838-126">IoTHubClient\_inden\_megszüntetése</span><span class="sxs-lookup"><span data-stu-id="50838-126">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="50838-127">Ezek a függvények minden "R" szerepeljenek az API-név.</span><span class="sxs-lookup"><span data-stu-id="50838-127">These functions all include “LL” in the API name.</span></span> <span data-ttu-id="50838-128">Eltérő ezek a paraméterek megegyeznek mint a nem le.</span><span class="sxs-lookup"><span data-stu-id="50838-128">Other than that, the parameters of each of these functions are identical to their non-LL counterparts.</span></span> <span data-ttu-id="50838-129">Azonban ezeket a funkciókat a viselkedés eltér egy fontos módon.</span><span class="sxs-lookup"><span data-stu-id="50838-129">However, the behavior of these functions is different in one important way.</span></span>

<span data-ttu-id="50838-130">A hívás esetén **IoTHubClient\_CreateFromConnectionString**, a mögöttes tárak, amelyek a háttérben fut, új szálat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="50838-130">When you call **IoTHubClient\_CreateFromConnectionString**, the underlying libraries create a new thread that runs in the background.</span></span> <span data-ttu-id="50838-131">Ebből a szálból küldi az eseményeket, és az IoT-központ fogadja az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="50838-131">This thread sends events to, and receives messages from, IoT Hub.</span></span> <span data-ttu-id="50838-132">Nincs ilyen szál jön létre, az "R" API-k használatakor.</span><span class="sxs-lookup"><span data-stu-id="50838-132">No such thread is created when working with the "LL" APIs.</span></span> <span data-ttu-id="50838-133">A háttérben futó szál létrehozásának a fejlesztők a könnyebb elérhetőség érdekében.</span><span class="sxs-lookup"><span data-stu-id="50838-133">The creation of the background thread is a convenience to the developer.</span></span> <span data-ttu-id="50838-134">Nem rendelkezik explicit módon események üzenetek küldése és fogadása az IoT Hub – automatikusan történik a háttérben foglalkoznia.</span><span class="sxs-lookup"><span data-stu-id="50838-134">You don’t have to worry about explicitly sending events and receiving messages from IoT Hub -- it happens automatically in the background.</span></span> <span data-ttu-id="50838-135">Ezzel szemben a "r" API-k biztosítanak explicit ellenőrzése alatt tartja a kommunikáció a IoT-központot, ha esetleg szükség lenne rá.</span><span class="sxs-lookup"><span data-stu-id="50838-135">In contrast, the "LL" APIs give you explicit control over communication with IoT Hub, if you need it.</span></span>

<span data-ttu-id="50838-136">Ez jobb megértéséhez, nézzük például:</span><span class="sxs-lookup"><span data-stu-id="50838-136">To understand this better, let’s look at an example:</span></span>

<span data-ttu-id="50838-137">A hívás esetén **IoTHubClient\_SendEventAsync**, mi ténylegesen végzett helyezi az esemény a pufferben.</span><span class="sxs-lookup"><span data-stu-id="50838-137">When you call **IoTHubClient\_SendEventAsync**, what you're actually doing is putting the event in a buffer.</span></span> <span data-ttu-id="50838-138">Létre, ha meghívja a háttérszálon **IoTHubClient\_CreateFromConnectionString** folyamatosan figyeli az adott puffer és a benne található adatokat küld az IoT hubhoz.</span><span class="sxs-lookup"><span data-stu-id="50838-138">The background thread created when you call **IoTHubClient\_CreateFromConnectionString** continually monitors this buffer and sends any data that it contains to IoT Hub.</span></span> <span data-ttu-id="50838-139">Ez akkor fordul elő a háttérben, hogy a fő szálnak működik-e más feladatok egy időben.</span><span class="sxs-lookup"><span data-stu-id="50838-139">This happens in the background at the same time that the main thread is performing other work.</span></span>

<span data-ttu-id="50838-140">Hasonlóképpen, az üzenetek visszahívási függvény regisztrálásakor **IoTHubClient\_SetMessageCallback**, az SDK kell rendelkeznie a visszahívási függvény meghívása, ha egy üzenetet kapott, a fő szálnak független háttérszál most utasítja.</span><span class="sxs-lookup"><span data-stu-id="50838-140">Similarly, when you register a callback function for messages using **IoTHubClient\_SetMessageCallback**, you're instructing the SDK to have the background thread invoke the callback function when a message is received, independent of the main thread.</span></span>

<span data-ttu-id="50838-141">A "r" API-k ne hozzon létre egy háttérszálon.</span><span class="sxs-lookup"><span data-stu-id="50838-141">The "LL" APIs don’t create a background thread.</span></span> <span data-ttu-id="50838-142">Ehelyett egy új API-t meg kell hívni explicit módon adatokat küldeni és fogadni az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="50838-142">Instead, a new API must be called to explicitly send and receive data from IoT Hub.</span></span> <span data-ttu-id="50838-143">A következő példa ezt mutatják.</span><span class="sxs-lookup"><span data-stu-id="50838-143">This is demonstrated in the following example.</span></span>

<span data-ttu-id="50838-144">A **IOT hubbal\_ügyfél\_minta\_http** alkalmazás, amely tartalmazza az SDK az alacsonyabb szintű API-k mutatja be.</span><span class="sxs-lookup"><span data-stu-id="50838-144">The **iothub\_client\_sample\_http** application that’s included in the SDK demonstrates the lower-level APIs.</span></span> <span data-ttu-id="50838-145">A minta nem küldeni események IoT-központ a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="50838-145">In that sample, we send events to IoT Hub with code such as the following:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="50838-146">Az első három sort, hozza létre az üzenetet, és az utolsó sort elküldi az esemény.</span><span class="sxs-lookup"><span data-stu-id="50838-146">The first three lines create the message, and the last line sends the event.</span></span> <span data-ttu-id="50838-147">Azonban amint azt korábban említettük, "küldő" az esemény azt jelenti, hogy az adatok egyszerűen kerülnek a pufferben.</span><span class="sxs-lookup"><span data-stu-id="50838-147">However, as mentioned previously, "sending" the event means that the data is simply placed in a buffer.</span></span> <span data-ttu-id="50838-148">Nem történik adatátvitel a hálózaton, hívása **IoTHubClient\_inden\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="50838-148">Nothing is transmitted on the network when we call **IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="50838-149">Ahhoz, hogy ténylegesen az adatokat az IoT-központ érkező, meg kell hívnia **IoTHubClient\_inden\_DoWork**, a példában szereplő:</span><span class="sxs-lookup"><span data-stu-id="50838-149">In order to actually ingress the data to IoT Hub, you must call **IoTHubClient\_LL\_DoWork**, as in this example:</span></span>

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="50838-150">Ez a kód (a a **IOT hubbal\_ügyfél\_minta\_http** alkalmazás) egymás után többször hívja **IoTHubClient\_inden\_DoWork**.</span><span class="sxs-lookup"><span data-stu-id="50838-150">This code (from the **iothub\_client\_sample\_http** application) repeatedly calls **IoTHubClient\_LL\_DoWork**.</span></span> <span data-ttu-id="50838-151">Minden alkalommal, amikor **IoTHubClient\_inden\_DoWork** van neve, a puffer az IoT-központ küldené egyes események, és lekérdezi az eszközre küldött aszinkron üzenet.</span><span class="sxs-lookup"><span data-stu-id="50838-151">Each time **IoTHubClient\_LL\_DoWork** is called, it sends some events from the buffer to IoT Hub and it retrieves a queued message being sent to the device.</span></span> <span data-ttu-id="50838-152">Ez utóbbi esetben azt jelenti, hogy ha a jelenleg regisztrált üzenetek visszahívási függvény, majd a visszahívás meghívták (feltéve, hogy minden üzenet sorba vannak).</span><span class="sxs-lookup"><span data-stu-id="50838-152">The latter case means that if we registered a callback function for messages, then the callback is invoked (assuming any messages are queued up).</span></span> <span data-ttu-id="50838-153">Azt kellene regisztrált visszahívási függvény a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="50838-153">We would have registered such a callback function with code such as the following:</span></span>

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

<span data-ttu-id="50838-154">Az OK, amely **IoTHubClient\_inden\_DoWork** gyakran nevezik hurok, amely minden alkalommal, amikor azt nevezzük, elküldi *néhány* eseményeket az IoT-központ, pufferelt és beolvassa a *a következő* üzenet sorba az eszköz.</span><span class="sxs-lookup"><span data-stu-id="50838-154">The reason that **IoTHubClient\_LL\_DoWork** is often called in a loop is that each time it’s called, it sends *some* buffered events to IoT Hub and retrieves *the next* message queued up for the device.</span></span> <span data-ttu-id="50838-155">Minden hívás nem garantált is küldi az összes pufferelt eseményeket, vagy beolvasni az összes várólistára helyezett üzenetek.</span><span class="sxs-lookup"><span data-stu-id="50838-155">Each call isn’t guaranteed to send all buffered events or to retrieve all queued messages.</span></span> <span data-ttu-id="50838-156">Ha szeretné a pufferben lévő összes eseményt küldeni, és folytassa a más feldolgozás a lecserélheti Ez a ciklus a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="50838-156">If you want to send all events in the buffer and then continue on with other processing you can replace this loop with code such as the following:</span></span>

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="50838-157">Ez a kód **IoTHubClient\_inden\_DoWork** mindaddig, amíg az IoT hubhoz elküldése a pufferben lévő összes eseményt.</span><span class="sxs-lookup"><span data-stu-id="50838-157">This code calls **IoTHubClient\_LL\_DoWork** until all events in the buffer have been sent to IoT Hub.</span></span> <span data-ttu-id="50838-158">Megjegyzés: Ez nem feltétlenül is jelenti, hogy az összes várólistára helyezett üzenetek fogadott.</span><span class="sxs-lookup"><span data-stu-id="50838-158">Note this does not also imply that all queued messages have been received.</span></span> <span data-ttu-id="50838-159">Ennek oka részét képezi, hogy "all" üzenetek ellenőrzése a nem a determinisztikus művelet.</span><span class="sxs-lookup"><span data-stu-id="50838-159">Part of the reason for this is that checking for "all" messages isn’t as deterministic an action.</span></span> <span data-ttu-id="50838-160">Mi történik, ha visszaállíthatja az "összes" üzenetet, de majd egy másikat küld az eszköz után azonnal?</span><span class="sxs-lookup"><span data-stu-id="50838-160">What happens if you retrieve "all" of the messages, but then another one is sent to the device immediately after?</span></span> <span data-ttu-id="50838-161">Jobb módja foglalkozik, amely programozott időkorlát.</span><span class="sxs-lookup"><span data-stu-id="50838-161">A better way to deal with that is with a programmed timeout.</span></span> <span data-ttu-id="50838-162">Például az üzenet visszahívási függvény sikerült alaphelyzetbe állítani egy számlálót minden alkalommal, amikor meghívták.</span><span class="sxs-lookup"><span data-stu-id="50838-162">For example, the message callback function could reset a timer every time it’s invoked.</span></span> <span data-ttu-id="50838-163">Majd írhat logika folytatódik a feldolgozás, ha például nem érkeztek üzenetek a legutóbbi *X* másodperc.</span><span class="sxs-lookup"><span data-stu-id="50838-163">You can then write logic to continue processing if, for example, no messages have been received in the last *X* seconds.</span></span>

<span data-ttu-id="50838-164">Ha végzett ingressing események van, és fogadja az üzeneteket, ügyeljen arra, hogy a megfelelő függvény erőforrások karbantartása.</span><span class="sxs-lookup"><span data-stu-id="50838-164">When you’re finished ingressing events and receiving messages, be sure to call the corresponding function to clean up resources.</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="50838-165">Nincs alapvetően csak egy készletét API-k a háttérszálon és API-k, amelyet az ugyanaz a háttérszálon nélkül egy másik készlet adatokat küldeni és fogadni.</span><span class="sxs-lookup"><span data-stu-id="50838-165">Basically there’s only one set of APIs to send and receive data with a background thread and another set of APIs that does the same thing without the background thread.</span></span> <span data-ttu-id="50838-166">Nagy mennyiségű fejlesztők előfordulhat, hogy inkább a nem - inden API-k, de az alacsonyabb szintű API-k akkor hasznos, ha a fejlesztői szeretne rendelni a hálózati átvitelt explicit vezérelheti.</span><span class="sxs-lookup"><span data-stu-id="50838-166">A lot of developers may prefer the non-LL APIs, but the lower-level APIs are useful when the developer wants explicit control over network transmissions.</span></span> <span data-ttu-id="50838-167">Például bizonyos eszközök begyűjtik az adatokat idő és a csak érkező jelzések (például óra egy alkalommal vagy naponta egyszer) meghatározott időközönként.</span><span class="sxs-lookup"><span data-stu-id="50838-167">For example, some devices collect data over time and only ingress events at specified intervals (for example, once an hour or once a day).</span></span> <span data-ttu-id="50838-168">Az alacsonyabb szintű API-k tudatjuk a felhasználókkal, lehetővé teszi, explicit módon vezérlő küld és fogad adatokat az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="50838-168">The lower-level APIs give you the ability to explicitly control when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="50838-169">Mások egyszerűen, főként a az egyszerűség, amelyek az alacsonyabb szintű API-k biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="50838-169">Others will simply prefer the simplicity that the lower-level APIs provide.</span></span> <span data-ttu-id="50838-170">Minden egyes munka azonban a háttérben, hanem a fő szálnak történik.</span><span class="sxs-lookup"><span data-stu-id="50838-170">Everything happens on the main thread rather than some work happening in the background.</span></span>

<span data-ttu-id="50838-171">Bármelyik modellt választja, ne felejtse el konzisztens mely API-kat használ.</span><span class="sxs-lookup"><span data-stu-id="50838-171">Whichever model you choose, be sure to be consistent in which APIs you use.</span></span> <span data-ttu-id="50838-172">Ha először meghívásával **IoTHubClient\_inden\_CreateFromConnectionString**, győződjön meg a megfelelő alacsonyabb szintű API-kat csak használ követő munka:</span><span class="sxs-lookup"><span data-stu-id="50838-172">If you start by calling **IoTHubClient\_LL\_CreateFromConnectionString**, be sure you only use the corresponding lower-level APIs for any follow-up work:</span></span>

* <span data-ttu-id="50838-173">IoTHubClient\_inden\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="50838-173">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="50838-174">IoTHubClient\_inden\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="50838-174">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="50838-175">IoTHubClient\_inden\_megszüntetése</span><span class="sxs-lookup"><span data-stu-id="50838-175">IoTHubClient\_LL\_Destroy</span></span>
* <span data-ttu-id="50838-176">IoTHubClient\_inden\_DoWork</span><span class="sxs-lookup"><span data-stu-id="50838-176">IoTHubClient\_LL\_DoWork</span></span>

<span data-ttu-id="50838-177">Ellenkező is igaz.</span><span class="sxs-lookup"><span data-stu-id="50838-177">The opposite is true as well.</span></span> <span data-ttu-id="50838-178">Ha először **IoTHubClient\_CreateFromConnectionString**, majd használja a nem - inden API-k minden további feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="50838-178">If you start with **IoTHubClient\_CreateFromConnectionString**, then use the non-LL APIs for any additional processing.</span></span>

<span data-ttu-id="50838-179">Az Azure IoT-eszközök SDK C-hez, olvassa el a **IOT hubbal\_ügyfél\_minta\_http** alkalmazás az alacsonyabb szintű API-k átfogó példát.</span><span class="sxs-lookup"><span data-stu-id="50838-179">In the Azure IoT device SDK for C, see the **iothub\_client\_sample\_http** application for a complete example of the lower-level APIs.</span></span> <span data-ttu-id="50838-180">A **IOT hubbal\_ügyfél\_minta\_amqp** alkalmazás teljes például az a nem - inden API-k lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="50838-180">The **iothub\_client\_sample\_amqp** application can be referenced for a full example of the non-LL APIs.</span></span>

## <a name="property-handling"></a><span data-ttu-id="50838-181">Tulajdonság kezelése</span><span class="sxs-lookup"><span data-stu-id="50838-181">Property handling</span></span>
<span data-ttu-id="50838-182">Amennyiben azt korábban leírt adatokat küldő, amikor azt korábban már hivatkozik az üzenet törzsét.</span><span class="sxs-lookup"><span data-stu-id="50838-182">So far when we've described sending data, we've been referring to the body of the message.</span></span> <span data-ttu-id="50838-183">Tegyük fel ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="50838-183">For example, consider this code:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="50838-184">Ez a példa egy üzenetet küld az IoT-központ szöveget a "Hello World".</span><span class="sxs-lookup"><span data-stu-id="50838-184">This example sends a message to IoT Hub with the text "Hello World."</span></span> <span data-ttu-id="50838-185">Azonban az IoT-központ lehetővé teszi tulajdonságok az egyes üzeneteket kell csatolni.</span><span class="sxs-lookup"><span data-stu-id="50838-185">However, IoT Hub also allows properties to be attached to each message.</span></span> <span data-ttu-id="50838-186">A rendszer az üzenethez csatolt név/érték párok tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="50838-186">Properties are name/value pairs that can be attached to the message.</span></span> <span data-ttu-id="50838-187">Módosíthatja például azt az előző kód csatolni egy tulajdonság az üzenet:</span><span class="sxs-lookup"><span data-stu-id="50838-187">For example, we can modify the previous code to attach a property to the message:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="50838-188">Először meghívásával **IoTHubMessage\_tulajdonságok** , és átadja azt a leíró az üzenet.</span><span class="sxs-lookup"><span data-stu-id="50838-188">We start by calling **IoTHubMessage\_Properties** and passing it the handle of our message.</span></span> <span data-ttu-id="50838-189">Mi azt vissza van egy **térkép\_KEZELNI** hivatkozás, amely lehetővé teszi indíthatja el a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="50838-189">What we get back is a **MAP\_HANDLE** reference that enables us to start adding properties.</span></span> <span data-ttu-id="50838-190">Meghívásával elvégzéséhez **térkép\_AddOrUpdate**, így tovább is hivatkozni kell a Térképen\_leíró, a tulajdonság nevét és a tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="50838-190">The latter is accomplished by calling **Map\_AddOrUpdate**, which takes a reference to a MAP\_HANDLE, the property name, and the property value.</span></span> <span data-ttu-id="50838-191">Ez az API-t hozzá lehessen adni annyi tulajdonságai, például azt.</span><span class="sxs-lookup"><span data-stu-id="50838-191">With this API we can add as many properties as we like.</span></span>

<span data-ttu-id="50838-192">Ha az esemény olvasni **Event Hubs**, a fogadó a tulajdonságok enumerálása, és a hozzájuk tartozó értékek beolvasása.</span><span class="sxs-lookup"><span data-stu-id="50838-192">When the event is read from **Event Hubs**, the receiver can enumerate the properties and retrieve their corresponding values.</span></span> <span data-ttu-id="50838-193">Például a .NET ez szeretné végezni a fér hozzá a [tulajdonsággyűjteményében a EventData objektum](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span><span class="sxs-lookup"><span data-stu-id="50838-193">For example, in .NET this would be accomplished by accessing the [Properties collection on the EventData object](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span></span>

<span data-ttu-id="50838-194">Az előző példában azt csatol tulajdonságok nem küldeni az IoT-központ eseményre.</span><span class="sxs-lookup"><span data-stu-id="50838-194">In the previous example, we’re attaching properties to an event that we send to IoT Hub.</span></span> <span data-ttu-id="50838-195">Az IoT-központ érkező üzenetek is csatolható tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="50838-195">Properties can also be attached to messages received from IoT Hub.</span></span> <span data-ttu-id="50838-196">Tulajdonságok lekérése üzenet szeretnénk, ha azt az üzenet visszahívási függvény a következő kódot használhatja:</span><span class="sxs-lookup"><span data-stu-id="50838-196">If we want to retrieve properties from a message, we can use code such as the following in our message callback function:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

<span data-ttu-id="50838-197">A hívás **IoTHubMessage\_tulajdonságok** adja vissza a **térkép\_KEZELNI** hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="50838-197">The call to **IoTHubMessage\_Properties** returns the **MAP\_HANDLE** reference.</span></span> <span data-ttu-id="50838-198">Azt, akkor továbbítja a hivatkozó **térkép\_GetInternals** a név/érték párok (valamint a tulajdonságok száma) tömbje mutató hivatkozás beszerzése.</span><span class="sxs-lookup"><span data-stu-id="50838-198">We then pass that reference to **Map\_GetInternals** to obtain a reference to an array of the name/value pairs (as well as a count of the properties).</span></span> <span data-ttu-id="50838-199">Ezen a ponton azt szeretnénk, ha az értékek gombra tulajdonságainak egyszerű kérdése.</span><span class="sxs-lookup"><span data-stu-id="50838-199">At that point it's a simple matter of enumerating the properties to get to the values we want.</span></span>

<span data-ttu-id="50838-200">Tulajdonságok használata az alkalmazás nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="50838-200">You don't have to use properties in your application.</span></span> <span data-ttu-id="50838-201">Azonban, ha szeretné-e meg őket a események vagy kérheti le azokat az üzeneteket, a **IoTHubClient** könyvtár megkönnyíti.</span><span class="sxs-lookup"><span data-stu-id="50838-201">However, if you need to set them on events or retrieve them from messages, the **IoTHubClient** library makes it easy.</span></span>

## <a name="message-handling"></a><span data-ttu-id="50838-202">Üzenet kezelése</span><span class="sxs-lookup"><span data-stu-id="50838-202">Message handling</span></span>
<span data-ttu-id="50838-203">Ahogy korábban is hangsúlyoztuk, amikor érkező üzenetek IoT-központ a **IoTHubClient** könyvtár regisztrált visszahívási függvény meghívása válaszol.</span><span class="sxs-lookup"><span data-stu-id="50838-203">As stated previously, when messages arrive from IoT Hub the **IoTHubClient** library responds by invoking a registered callback function.</span></span> <span data-ttu-id="50838-204">Ez a funkció, amely további rövid érdemel visszatérési paraméterének van.</span><span class="sxs-lookup"><span data-stu-id="50838-204">There is a return parameter of this function that deserves some additional explanation.</span></span> <span data-ttu-id="50838-205">Ez a visszahívási függvény cikkből a **IOT hubbal\_ügyfél\_minta\_http** mintaalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="50838-205">Here’s an excerpt of the callback function in the **iothub\_client\_sample\_http** sample application:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

<span data-ttu-id="50838-206">Vegye figyelembe, hogy a visszatérési típus **IOTHUBMESSAGE\_törlése\_eredmény** és az adott esetben a rendszer visszaadja a **IOTHUBMESSAGE\_elfogadott**.</span><span class="sxs-lookup"><span data-stu-id="50838-206">Note that the return type is **IOTHUBMESSAGE\_DISPOSITION\_RESULT** and in this particular case we return **IOTHUBMESSAGE\_ACCEPTED**.</span></span> <span data-ttu-id="50838-207">Nincsenek más értékek azt adhatja vissza a függvény, amely módosíthatja az **IoTHubClient** könyvtár reagálás az üzenet visszahívási.</span><span class="sxs-lookup"><span data-stu-id="50838-207">There are other values we can return from this function that change how the **IoTHubClient** library reacts to the message callback.</span></span> <span data-ttu-id="50838-208">Az alábbiakban a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="50838-208">Here are the options.</span></span>

* <span data-ttu-id="50838-209">**IOTHUBMESSAGE\_elfogadott** – az üzenet feldolgozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="50838-209">**IOTHUBMESSAGE\_ACCEPTED** – The message has been processed successfully.</span></span> <span data-ttu-id="50838-210">A **IoTHubClient** könyvtár nem indítja el a visszahívási függvény újra ugyanazt az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="50838-210">The **IoTHubClient** library will not invoke the callback function again with the same message.</span></span>
* <span data-ttu-id="50838-211">**IOTHUBMESSAGE\_elutasítva** – az üzenet nem lett feldolgozva, és tudni az ehhez a jövőben van.</span><span class="sxs-lookup"><span data-stu-id="50838-211">**IOTHUBMESSAGE\_REJECTED** – The message was not processed and there is no desire to do so in the future.</span></span> <span data-ttu-id="50838-212">A **IoTHubClient** könyvtár nem kell meghívnia a visszahívási függvény újra ugyanazt az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="50838-212">The **IoTHubClient** library should not invoke the callback function again with the same message.</span></span>
* <span data-ttu-id="50838-213">**IOTHUBMESSAGE\_ABANDONED** – az üzenet nem feldolgozása sikeresen megtörtént, de a **IoTHubClient** könyvtárban kell meghívnia a visszahívási függvény újra ugyanazt az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="50838-213">**IOTHUBMESSAGE\_ABANDONED** – The message was not processed successfully, but the **IoTHubClient** library should invoke the callback function again with the same message.</span></span>

<span data-ttu-id="50838-214">Az első két visszatérési kódok, a **IoTHubClient** könyvtár üzenetet küld az IoT-központ, amely jelzi, hogy az üzenet törlődik az eszköz üzenetsorból kell-e, és nem kézbesíteni újra.</span><span class="sxs-lookup"><span data-stu-id="50838-214">For the first two return codes, the **IoTHubClient** library sends a message to IoT Hub indicating that the message should be deleted from the device queue and not delivered again.</span></span> <span data-ttu-id="50838-215">A nettó hatást megegyezik (az üzenet törlődik az eszköz várólista), de e a elfogadta vagy visszautasított továbbra is rögzíti.</span><span class="sxs-lookup"><span data-stu-id="50838-215">The net effect is the same (the message is deleted from the device queue), but whether the message was accepted or rejected is still recorded.</span></span>  <span data-ttu-id="50838-216">Ezt a különbséget rögzítése célszerű az üzenet feladók, akik a visszajelzés figyelésére és megtudhatja, ha egy eszköz elfogadta vagy egy üzenet visszautasította.</span><span class="sxs-lookup"><span data-stu-id="50838-216">Recording this distinction is useful to senders of the message who can listen for feedback and find out if a device has accepted or rejected a particular message.</span></span>

<span data-ttu-id="50838-217">Az utolsó esetben egy üzenet is lett van küldve IoT-központot, de azt jelzi, hogy az üzenet újból kézbesítve a kell.</span><span class="sxs-lookup"><span data-stu-id="50838-217">In the last case a message is also sent to IoT Hub, but it indicates that the message should be redelivered.</span></span> <span data-ttu-id="50838-218">Általában egy üzenetet fog abandon, ha néhány hibát, de az üzenet feldolgozására újra megpróbálhatja.</span><span class="sxs-lookup"><span data-stu-id="50838-218">Typically you’ll abandon a message if you encounter some error but want to try to process the message again.</span></span> <span data-ttu-id="50838-219">Ezzel szemben elutasítása üzenet akkor megfelelő ha helyrehozhatatlan hibát észlel (vagy egyszerűen döntenie nem tudja feldolgozni az üzenetet).</span><span class="sxs-lookup"><span data-stu-id="50838-219">In contrast, rejecting a message is appropriate when you encounter an unrecoverable error (or if you simply decide you don’t want to process the message).</span></span>

<span data-ttu-id="50838-220">Mindenképpen figyelembe vennie a különböző visszatérési kódokat, hogy a kívánt viselkedés akkor is kér a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="50838-220">In any case, be aware of the different return codes so that you can elicit the behavior you want from the **IoTHubClient** library.</span></span>

## <a name="alternate-device-credentials"></a><span data-ttu-id="50838-221">Alternatív hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="50838-221">Alternate device credentials</span></span>
<span data-ttu-id="50838-222">Ahogy korábban, az első lépés az használatakor a **IoTHubClient** könyvtár az beszerzése egy **IOT HUBBAL\_ügyfél\_KEZELNI** például a következő hívással:</span><span class="sxs-lookup"><span data-stu-id="50838-222">As explained previously, the first thing to do when working with the **IoTHubClient** library is to obtain a **IOTHUB\_CLIENT\_HANDLE** with a call such as the following:</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="50838-223">Az argumentumok **IoTHubClient\_CreateFromConnectionString** az eszköz kapcsolati karakterláncát, és egy paraméter, amely jelzi a protokoll használatával kommunikáljon az IoT hubbal.</span><span class="sxs-lookup"><span data-stu-id="50838-223">The arguments to **IoTHubClient\_CreateFromConnectionString** are the device connection string and a parameter that indicates the protocol we use to communicate with IoT Hub.</span></span> <span data-ttu-id="50838-224">Az eszköz kapcsolati karakterlánc formátuma a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="50838-224">The device connection string has a format that appears as follows:</span></span>

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

<span data-ttu-id="50838-225">Ez a karakterlánc a négy adatra van: az IoT-központ nevét, az IoT-központ utótag, Eszközazonosító és megosztott elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="50838-225">There are four pieces of information in this string: IoT Hub name, IoT Hub suffix, device ID, and shared access key.</span></span> <span data-ttu-id="50838-226">A teljesen minősített tartománynevét (FQDN), az IoT-központ kaphat az Azure-portálon az IoT hub-példány létrehozásakor – ez lehetővé teszi az IoT-központnév (első része a teljes Tartományneve) és az IoT hub utótagot (az FQDN a többi).</span><span class="sxs-lookup"><span data-stu-id="50838-226">You obtain the fully qualified domain name (FQDN) of an IoT hub when you create your IoT hub instance in the Azure portal — this gives you the IoT hub name (the first part of the FQDN) and the IoT hub suffix (the rest of the FQDN).</span></span> <span data-ttu-id="50838-227">Kapott az Eszközazonosító és a megosztott elérési kulcsot az eszköz regisztrálása az IoT-központ (lásd: a [előző cikkben](iot-hub-device-sdk-c-intro.md)).</span><span class="sxs-lookup"><span data-stu-id="50838-227">You get the device ID and the shared access key when you register your device with IoT Hub (as described in the [previous article](iot-hub-device-sdk-c-intro.md)).</span></span>

<span data-ttu-id="50838-228">**IoTHubClient\_CreateFromConnectionString** lehetővé teszi az egyik módja a kódtár inicializálása.</span><span class="sxs-lookup"><span data-stu-id="50838-228">**IoTHubClient\_CreateFromConnectionString** gives you one way to initialize the library.</span></span> <span data-ttu-id="50838-229">Ha jobban szeret, hozhat létre egy új **IOT HUBBAL\_ügyfél\_KEZELNI** az eszköz kapcsolati karakterlánc helyett ezen egyéni paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="50838-229">If you prefer, you can create a new **IOTHUB\_CLIENT\_HANDLE** by using these individual parameters rather than the device connection string.</span></span> <span data-ttu-id="50838-230">Ez úgy érhető el, az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="50838-230">This is achieved with the following code:</span></span>

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

<span data-ttu-id="50838-231">Ezt a feladatot el ugyanaz, mint **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="50838-231">This accomplishes the same thing as **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="50838-232">Nyilvánvaló, hogy volna használni kívánt tűnhet **IoTHubClient\_CreateFromConnectionString** ahelyett, hogy ez a részletesebb módszer az inicializálás.</span><span class="sxs-lookup"><span data-stu-id="50838-232">It may seem obvious that you would want to use **IoTHubClient\_CreateFromConnectionString** rather than this more verbose method of initialization.</span></span> <span data-ttu-id="50838-233">Ne feledje, azonban, hogy amikor regisztrál egy eszközt az IoT-központ tartalmával egy Eszközazonosító és eszközkulcs (kapcsolati karakterlánc).</span><span class="sxs-lookup"><span data-stu-id="50838-233">Keep in mind, however, that when you register a device in IoT Hub what you get is a device ID and device key (not a connection string).</span></span> <span data-ttu-id="50838-234">A *eszköz explorer* SDK eszköz bevezetett a [előző cikkben](iot-hub-device-sdk-c-intro.md) a-tárakat használ a **Azure IoT szolgáltatás SDK** az eszköz kapcsolati karakterlánc létrehozása a Eszközazonosító eszközkulcs és az IoT-központ állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="50838-234">The *device explorer* SDK tool introduced in the [previous article](iot-hub-device-sdk-c-intro.md) uses libraries in the **Azure IoT service SDK** to create the device connection string from the device ID, device key, and IoT Hub host name.</span></span> <span data-ttu-id="50838-235">Így hívása **IoTHubClient\_inden\_létrehozása** lehet, hogy előnyösebb egy kapcsolati karakterlánc létrehozása a lépés menti azt.</span><span class="sxs-lookup"><span data-stu-id="50838-235">So calling **IoTHubClient\_LL\_Create** may be preferable because it saves you the step of generating a connection string.</span></span> <span data-ttu-id="50838-236">Használja az kényelmes bármelyik módszert.</span><span class="sxs-lookup"><span data-stu-id="50838-236">Use whichever method is convenient.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="50838-237">Konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="50838-237">Configuration options</span></span>
<span data-ttu-id="50838-238">Amennyiben mindent ismertetett beállításairól a **IoTHubClient** könyvtár works tükrözi az alapértelmezett viselkedés.</span><span class="sxs-lookup"><span data-stu-id="50838-238">So far everything described about the way the **IoTHubClient** library works reflects its default behavior.</span></span> <span data-ttu-id="50838-239">Van azonban néhány lehetőség, amely lehet módosítani a szalagtár működése.</span><span class="sxs-lookup"><span data-stu-id="50838-239">However, there are a few options that you can set to change how the library works.</span></span> <span data-ttu-id="50838-240">Mindez, ami a **IoTHubClient\_inden\_SetOption** API.</span><span class="sxs-lookup"><span data-stu-id="50838-240">This is accomplished by leveraging the **IoTHubClient\_LL\_SetOption** API.</span></span> <span data-ttu-id="50838-241">Vegye figyelembe az ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="50838-241">Consider this example:</span></span>

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

<span data-ttu-id="50838-242">Van néhány gyakran használt beállításokat:</span><span class="sxs-lookup"><span data-stu-id="50838-242">There are a couple of options that are commonly used:</span></span>

* <span data-ttu-id="50838-243">**SetBatching** (logikai) – Ha **igaz**, majd az IoT-központ küldött adatok küldése kötegek.</span><span class="sxs-lookup"><span data-stu-id="50838-243">**SetBatching** (bool) – If **true**, then data sent to IoT Hub is sent in batches.</span></span> <span data-ttu-id="50838-244">Ha **hamis**, majd üzenetküldés külön-külön.</span><span class="sxs-lookup"><span data-stu-id="50838-244">If **false**, then messages are sent individually.</span></span> <span data-ttu-id="50838-245">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="50838-245">The default is **false**.</span></span> <span data-ttu-id="50838-246">Vegye figyelembe, hogy a **SetBatching** beállítás csak akkor érvényes, a HTTP protokollhoz, és nem a MQTT vagy az AMQP protokollt.</span><span class="sxs-lookup"><span data-stu-id="50838-246">Note that the **SetBatching** option only applies to the HTTP protocol and not to the MQTT or AMQP protocols.</span></span>
* <span data-ttu-id="50838-247">**Időtúllépés** (előjel nélküli egész) – Ez az érték ezredmásodpercben szerepel.</span><span class="sxs-lookup"><span data-stu-id="50838-247">**Timeout** (unsigned int) – This value is represented in milliseconds.</span></span> <span data-ttu-id="50838-248">Ha egy HTTP-kérés vagy válasz fogadása ennél hosszabb ideje tart, akkor a kapcsolat időtúllépés miatt küldi.</span><span class="sxs-lookup"><span data-stu-id="50838-248">If sending an HTTP request or receiving a response takes longer than this time, then the connection times out.</span></span>

<span data-ttu-id="50838-249">Fontos a kötegelési lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="50838-249">The batching option is important.</span></span> <span data-ttu-id="50838-250">Alapértelmezés szerint a szalagtár ingresses események külön-külön (egy adott eseményhez bármilyen át a **IoTHubClient\_inden\_SendEventAsync**).</span><span class="sxs-lookup"><span data-stu-id="50838-250">By default, the library ingresses events individually (a single event is whatever you pass to **IoTHubClient\_LL\_SendEventAsync**).</span></span> <span data-ttu-id="50838-251">Ha a kötegelési beállítás **igaz**, a könyvtár (akár a maximális méretét, amely elfogadja az IoT-központ) pufferből használhatja annyi eseményeit gyűjti.</span><span class="sxs-lookup"><span data-stu-id="50838-251">If the batching option is **true**, the library collects as many events as it can from the buffer (up to the maximum message size that IoT Hub will accept).</span></span>  <span data-ttu-id="50838-252">Az esemény kötegelt küldése az IoT hubhoz (az egyes események, egy JSON-tömb be vannak becsomagolva) egyetlen HTTP-hívással.</span><span class="sxs-lookup"><span data-stu-id="50838-252">The event batch is sent to IoT Hub in a single HTTP call (the individual events are bundled into a JSON array).</span></span> <span data-ttu-id="50838-253">Általában kötegelés engedélyezését eredményezi nagy teljesítménynövekedéshez óta hálózati üzenetváltások most csökkentése.</span><span class="sxs-lookup"><span data-stu-id="50838-253">Enabling batching typically results in big performance gains since you’re reducing network round-trips.</span></span> <span data-ttu-id="50838-254">Mivel az esemény-es HTTP-fejlécek egy készletét, nem pedig a minden egyes esemény fejléc készlettel küldi is jelentősen csökkenti a sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="50838-254">It also significantly reduces bandwidth since you are sending one set of HTTP headers with an event batch rather than a set of headers for each individual event.</span></span> <span data-ttu-id="50838-255">Ha valamiért másképp nincs, általában érdemes engedélyezni a kötegelés.</span><span class="sxs-lookup"><span data-stu-id="50838-255">Unless you have a specific reason to do otherwise, typically you’ll want to enable batching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="50838-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="50838-256">Next steps</span></span>
<span data-ttu-id="50838-257">Ez a cikk ismerteti részletesen a működését a **IoTHubClient** függvénytár szerepel a **C-hez készült SDK Azure IoT-eszközök**. Az információ funkcióinak beható ismerete rendelkeznie kell a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="50838-257">This article describes in detail the behavior of the **IoTHubClient** library found in the **Azure IoT device SDK for C**. With this information, you should have a good understanding of the capabilities of the **IoTHubClient** library.</span></span> <span data-ttu-id="50838-258">A [tovább cikk](iot-hub-device-sdk-c-serializer.md) hasonló részletesen ismerteti a a **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="50838-258">The [next article](iot-hub-device-sdk-c-serializer.md) provides similar detail on the **serializer** library.</span></span>

<span data-ttu-id="50838-259">Az IoT-központ fejlesztésével kapcsolatos további tudnivalókért tekintse meg a [Azure IoT SDK-k][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="50838-259">To learn more about developing for IoT Hub, see the [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="50838-260">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="50838-260">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="50838-261">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="50838-261">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
