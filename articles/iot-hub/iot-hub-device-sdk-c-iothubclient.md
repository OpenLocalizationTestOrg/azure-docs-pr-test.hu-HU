---
title: "aaaAzure IoT-eszközök SDK c - IoTHubClient |} Microsoft Docs"
description: "Hogyan toouse hello IoTHubClient szalagtár hello Azure IoT-eszközök SDK C toocreate eszköz alkalmazások, amelyek kommunikálni az IoT-központ."
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
ms.openlocfilehash: d1ece79e9ba6d1e5fd45cabb8fca393b24052e07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a><span data-ttu-id="f5f98-103">Az Azure IoT-eszközök SDK c – további információ az IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="f5f98-103">Azure IoT device SDK for C – more about IoTHubClient</span></span>
<span data-ttu-id="f5f98-104">Hello [először a következő cikket:](iot-hub-device-sdk-c-intro.md) az a sorozat bevezetett hello **C-hez készült SDK Azure IoT-eszközök**. A cikk alapján, hogy nincsenek-e két architekturális rétegek SDK.</span><span class="sxs-lookup"><span data-stu-id="f5f98-104">hello [first article](iot-hub-device-sdk-c-intro.md) in this series introduced hello **Azure IoT device SDK for C**. That article explained that there are two architectural layers in SDK.</span></span> <span data-ttu-id="f5f98-105">Alapszintű hello van hello **IoTHubClient** könyvtárban, amely közvetlenül kezeli az IoT-központ folytatott kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="f5f98-105">At hello base is hello **IoTHubClient** library which directly manages communication with IoT Hub.</span></span> <span data-ttu-id="f5f98-106">Szerepel továbbá hello **szerializáló** , mely a buildek tooprovide szerializálási szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="f5f98-106">There's also hello **serializer** library that builds on top of that tooprovide serialization services.</span></span> <span data-ttu-id="f5f98-107">Ebben a cikkben a következőkhöz nyújtunk további részletek a hello **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="f5f98-107">In this article we'll provide additional detail on hello **IoTHubClient** library.</span></span>

<span data-ttu-id="f5f98-108">hello előző cikkben leírt hogyan toouse hello **IoTHubClient** könyvtár toosend események tooIoT központ és az üzenetek fogadásához.</span><span class="sxs-lookup"><span data-stu-id="f5f98-108">hello previous article described how toouse hello **IoTHubClient** library toosend events tooIoT Hub and receive messages.</span></span> <span data-ttu-id="f5f98-109">Ez a cikk kiterjeszti ezekre a kérdésekre adott válaszokat azáltal, hogy elmagyarázza, hogyan toomore pontosan kezelése *amikor* küldött és fogadott adatok bevezeti a toohello **alacsonyabb szintű API-k**.</span><span class="sxs-lookup"><span data-stu-id="f5f98-109">This article extends that discussion by explaining how toomore precisely manage *when* you send and receive data, introducing you toohello **lower-level APIs**.</span></span> <span data-ttu-id="f5f98-110">Azt is ismertetjük, hogyan tooattach tulajdonságok tooevents (és üzenetek lekérése őket) hello szolgáltatások kezelése hello tulajdonsággal **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="f5f98-110">We'll also explain how tooattach properties tooevents (and retrieve them from messages) using hello property handling features in hello **IoTHubClient** library.</span></span> <span data-ttu-id="f5f98-111">Végezetül, a következőkhöz nyújtunk különböző módokon toohandle további magyarázata IoT-központ érkezett.</span><span class="sxs-lookup"><span data-stu-id="f5f98-111">Finally, we'll provide additional explanation of different ways toohandle messages received from IoT Hub.</span></span>

<span data-ttu-id="f5f98-112">hello cikk arra a következtetésre jut vegyes témaköreit, beleértve az bővebben a hitelesítő adatai, és hogyan toochange hello hello működésének néhány kiterjed **IoTHubClient** konfigurációs lehetőségek között.</span><span class="sxs-lookup"><span data-stu-id="f5f98-112">hello article concludes by covering a couple of miscellaneous topics, including more about device credentials and how toochange hello behavior of hello **IoTHubClient** through configuration options.</span></span>

<span data-ttu-id="f5f98-113">Hello fogjuk használni **IoTHubClient** SDK-minták tooexplain ezeket a témaköröket.</span><span class="sxs-lookup"><span data-stu-id="f5f98-113">We'll use hello **IoTHubClient** SDK samples tooexplain these topics.</span></span> <span data-ttu-id="f5f98-114">Ha azt szeretné, hogy mentén toofollow, tekintse meg a hello **IOT hubbal\_ügyfél\_minta\_http** és **IOT hubbal\_ügyfél\_minta\_amqp** hello Azure IoT-eszközök SDK szereplő minden c hello a következő részekben leírt mutatják be ezeket a mintákat az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f5f98-114">If you want toofollow along, see hello **iothub\_client\_sample\_http** and **iothub\_client\_sample\_amqp** applications that are included in hello Azure IoT device SDK for C. Everything described in hello following sections is demonstrated in these samples.</span></span>

<span data-ttu-id="f5f98-115">Hello található [ **C-hez készült SDK Azure IoT-eszközök** ](https://github.com/Azure/azure-iot-sdk-c) hello API hello a GitHub tárház és nézet adatai [C API-referencia](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="f5f98-115">You can find hello [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of hello API in hello [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="hello-lower-level-apis"></a><span data-ttu-id="f5f98-116">hello alacsonyabb szintű API-k</span><span class="sxs-lookup"><span data-stu-id="f5f98-116">hello lower-level APIs</span></span>
<span data-ttu-id="f5f98-117">hello előző cikkben leírt hello alapszintű hello működésének **IotHubClient** belül hello hello kontextusában **IOT hubbal\_ügyfél\_minta\_amqp** az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f5f98-117">hello previous article described hello basic operation of hello **IotHubClient** within hello context of hello **iothub\_client\_sample\_amqp** application.</span></span> <span data-ttu-id="f5f98-118">Például azt, hogyan tooinitialize hello könyvtár használja ezt a kódot.</span><span class="sxs-lookup"><span data-stu-id="f5f98-118">For example, it explained how tooinitialize hello library using this code.</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="f5f98-119">Azt is ismerteti, hogyan függvényhívás használatát az toosend események.</span><span class="sxs-lookup"><span data-stu-id="f5f98-119">It also described how toosend events using this function call.</span></span>

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

<span data-ttu-id="f5f98-120">hello a cikk azt is ismerteti, hogyan tooreceive üzenetek, ha regisztrálja a visszahívási függvény.</span><span class="sxs-lookup"><span data-stu-id="f5f98-120">hello article also described how tooreceive messages by registering a callback function.</span></span>

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

<span data-ttu-id="f5f98-121">hello cikket is bemutatta, hogyan toofree-erőforrások hello alábbi kód.</span><span class="sxs-lookup"><span data-stu-id="f5f98-121">hello article also showed how toofree resources using code such as hello following.</span></span>

```
IoTHubClient_Destroy(iotHubClientHandle);
```

<span data-ttu-id="f5f98-122">Vannak azonban olyan API-e kiegészítő funkciók tooeach:</span><span class="sxs-lookup"><span data-stu-id="f5f98-122">However there are companion functions tooeach of these APIs:</span></span>

* <span data-ttu-id="f5f98-123">IoTHubClient\_inden\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="f5f98-123">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="f5f98-124">IoTHubClient\_inden\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="f5f98-124">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="f5f98-125">IoTHubClient\_inden\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="f5f98-125">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="f5f98-126">IoTHubClient\_inden\_megszüntetése</span><span class="sxs-lookup"><span data-stu-id="f5f98-126">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="f5f98-127">Ezek a függvények minden "R" foglalandó hello API-név.</span><span class="sxs-lookup"><span data-stu-id="f5f98-127">These functions all include “LL” in hello API name.</span></span> <span data-ttu-id="f5f98-128">Eltérő hello ezek paraméterei azonos tootheir nem inden megfelelők esetében.</span><span class="sxs-lookup"><span data-stu-id="f5f98-128">Other than that, hello parameters of each of these functions are identical tootheir non-LL counterparts.</span></span> <span data-ttu-id="f5f98-129">Azonban hello ezek a funkciók működése eltérő módon, egy fontos.</span><span class="sxs-lookup"><span data-stu-id="f5f98-129">However, hello behavior of these functions is different in one important way.</span></span>

<span data-ttu-id="f5f98-130">A hívás esetén **IoTHubClient\_CreateFromConnectionString**, alapul szolgáló szalagtárak hello hello háttérben futó új szálat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f5f98-130">When you call **IoTHubClient\_CreateFromConnectionString**, hello underlying libraries create a new thread that runs in hello background.</span></span> <span data-ttu-id="f5f98-131">Ebből a szálból küldi az eseményeket, és az IoT-központ fogadja az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="f5f98-131">This thread sends events to, and receives messages from, IoT Hub.</span></span> <span data-ttu-id="f5f98-132">Nincs ilyen szálhoz létrehoz az hello "R" API-k használatakor.</span><span class="sxs-lookup"><span data-stu-id="f5f98-132">No such thread is created when working with hello "LL" APIs.</span></span> <span data-ttu-id="f5f98-133">hello háttérszál hello létrehozását a fejlesztők kényelme toohello.</span><span class="sxs-lookup"><span data-stu-id="f5f98-133">hello creation of hello background thread is a convenience toohello developer.</span></span> <span data-ttu-id="f5f98-134">Nincs tooworry kapcsolatos explicit módon események üzenetek küldése és fogadása az IoT Hub – hello háttérben automatikusan történik.</span><span class="sxs-lookup"><span data-stu-id="f5f98-134">You don’t have tooworry about explicitly sending events and receiving messages from IoT Hub -- it happens automatically in hello background.</span></span> <span data-ttu-id="f5f98-135">Ezzel szemben hello "R" API-k biztosítanak explicit ellenőrzése alatt tartja a kommunikáció a IoT-központot, ha esetleg szükség lenne rá.</span><span class="sxs-lookup"><span data-stu-id="f5f98-135">In contrast, hello "LL" APIs give you explicit control over communication with IoT Hub, if you need it.</span></span>

<span data-ttu-id="f5f98-136">toounderstand a megfelelőbb nézzük például:</span><span class="sxs-lookup"><span data-stu-id="f5f98-136">toounderstand this better, let’s look at an example:</span></span>

<span data-ttu-id="f5f98-137">A hívás esetén **IoTHubClient\_SendEventAsync**, mi ténylegesen végzett helyezi hello esemény a pufferben.</span><span class="sxs-lookup"><span data-stu-id="f5f98-137">When you call **IoTHubClient\_SendEventAsync**, what you're actually doing is putting hello event in a buffer.</span></span> <span data-ttu-id="f5f98-138">létre, ha meghívja a háttérben futó szál hello **IoTHubClient\_CreateFromConnectionString** folyamatosan figyeli az adott puffer és adatokat küld, hogy az tartalmazza-e tooIoT központ.</span><span class="sxs-lookup"><span data-stu-id="f5f98-138">hello background thread created when you call **IoTHubClient\_CreateFromConnectionString** continually monitors this buffer and sends any data that it contains tooIoT Hub.</span></span> <span data-ttu-id="f5f98-139">Ez akkor fordul elő: hello hello háttérben azonos ideje, hogy hello fő szálnak működik-e más feladatok.</span><span class="sxs-lookup"><span data-stu-id="f5f98-139">This happens in hello background at hello same time that hello main thread is performing other work.</span></span>

<span data-ttu-id="f5f98-140">Hasonlóképpen, az üzenetek visszahívási függvény regisztrálásakor **IoTHubClient\_SetMessageCallback**, hello SDK toohave hello háttér most utasítja szál a hello visszahívási függvény meghívása az üzenet esetén fogadott, független hello fő szálnak.</span><span class="sxs-lookup"><span data-stu-id="f5f98-140">Similarly, when you register a callback function for messages using **IoTHubClient\_SetMessageCallback**, you're instructing hello SDK toohave hello background thread invoke hello callback function when a message is received, independent of hello main thread.</span></span>

<span data-ttu-id="f5f98-141">hello "R" API-k ne hozzon létre egy háttérszálon.</span><span class="sxs-lookup"><span data-stu-id="f5f98-141">hello "LL" APIs don’t create a background thread.</span></span> <span data-ttu-id="f5f98-142">Ehelyett egy olyan új API tooexplicitly küldési kell meghívni, és fogadhat adatokat az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="f5f98-142">Instead, a new API must be called tooexplicitly send and receive data from IoT Hub.</span></span> <span data-ttu-id="f5f98-143">Ezt mutatják be a következő példa hello.</span><span class="sxs-lookup"><span data-stu-id="f5f98-143">This is demonstrated in hello following example.</span></span>

<span data-ttu-id="f5f98-144">Hello **IOT hubbal\_ügyfél\_minta\_http** alkalmazás, amely azt mutatja be, SDK hello megtalálható hello alacsonyabb szintű API-k.</span><span class="sxs-lookup"><span data-stu-id="f5f98-144">hello **iothub\_client\_sample\_http** application that’s included in hello SDK demonstrates hello lower-level APIs.</span></span> <span data-ttu-id="f5f98-145">A mintában szereplő küldünk események tooIoT Hub például hello következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="f5f98-145">In that sample, we send events tooIoT Hub with code such as hello following:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="f5f98-146">hello első három sorok hello üzenet létrehozása, és hello utolsó sora küldi hello esemény.</span><span class="sxs-lookup"><span data-stu-id="f5f98-146">hello first three lines create hello message, and hello last line sends hello event.</span></span> <span data-ttu-id="f5f98-147">Azonban amint azt korábban említettük, "küldése a" hello esemény azt jelenti, hogy egyszerűen hello adatok kerülnek a pufferben.</span><span class="sxs-lookup"><span data-stu-id="f5f98-147">However, as mentioned previously, "sending" hello event means that hello data is simply placed in a buffer.</span></span> <span data-ttu-id="f5f98-148">Nem történik adatátvitel hello hálózaton, hívása **IoTHubClient\_inden\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="f5f98-148">Nothing is transmitted on hello network when we call **IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="f5f98-149">A sorrend tooactually érkező hello adatok tooIoT Hub, meg kell hívnia **IoTHubClient\_inden\_DoWork**, a példában szereplő:</span><span class="sxs-lookup"><span data-stu-id="f5f98-149">In order tooactually ingress hello data tooIoT Hub, you must call **IoTHubClient\_LL\_DoWork**, as in this example:</span></span>

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="f5f98-150">Ez a kód (a hello **IOT hubbal\_ügyfél\_minta\_http** alkalmazás) egymás után többször hívja **IoTHubClient\_inden\_DoWork** .</span><span class="sxs-lookup"><span data-stu-id="f5f98-150">This code (from hello **iothub\_client\_sample\_http** application) repeatedly calls **IoTHubClient\_LL\_DoWork**.</span></span> <span data-ttu-id="f5f98-151">Minden alkalommal, amikor **IoTHubClient\_inden\_DoWork** van neve, az egyes események küldi hello puffer tooIoT Hub, és lekérdezi a egy aszinkron küldés alatt álló toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="f5f98-151">Each time **IoTHubClient\_LL\_DoWork** is called, it sends some events from hello buffer tooIoT Hub and it retrieves a queued message being sent toohello device.</span></span> <span data-ttu-id="f5f98-152">hello ez utóbbi esetben azt jelenti, hogy ha a jelenleg regisztrált visszahívási függvény üzenetek, majd hello visszahívási meghívták (feltéve, hogy minden üzenet sorba vannak).</span><span class="sxs-lookup"><span data-stu-id="f5f98-152">hello latter case means that if we registered a callback function for messages, then hello callback is invoked (assuming any messages are queued up).</span></span> <span data-ttu-id="f5f98-153">Azt kellene regisztrált visszahívási függvény például hello következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="f5f98-153">We would have registered such a callback function with code such as hello following:</span></span>

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

<span data-ttu-id="f5f98-154">hello OK, amely **IoTHubClient\_inden\_DoWork** gyakran nevezik hurok, amely minden alkalommal, amikor azt nevezzük, elküldi *néhány* pufferelt eseményeket tooIoT Hub és lekéri *következő hello* üzenet sorba hello eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="f5f98-154">hello reason that **IoTHubClient\_LL\_DoWork** is often called in a loop is that each time it’s called, it sends *some* buffered events tooIoT Hub and retrieves *hello next* message queued up for hello device.</span></span> <span data-ttu-id="f5f98-155">Minden hívás nem garantált toosend összes pufferelt eseményeket vagy tooretrieve összes sorba állított üzenetek.</span><span class="sxs-lookup"><span data-stu-id="f5f98-155">Each call isn’t guaranteed toosend all buffered events or tooretrieve all queued messages.</span></span> <span data-ttu-id="f5f98-156">Ha azt szeretné, hogy toosend események kerülnek egy hello puffer, és folytassa a más feldolgozás a lecserélheti a hurok hello alábbi kódot:</span><span class="sxs-lookup"><span data-stu-id="f5f98-156">If you want toosend all events in hello buffer and then continue on with other processing you can replace this loop with code such as hello following:</span></span>

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="f5f98-157">Ez a kód **IoTHubClient\_inden\_DoWork** amíg hello pufferben lévő összes esemény elküldött tooIoT központ.</span><span class="sxs-lookup"><span data-stu-id="f5f98-157">This code calls **IoTHubClient\_LL\_DoWork** until all events in hello buffer have been sent tooIoT Hub.</span></span> <span data-ttu-id="f5f98-158">Megjegyzés: Ez nem feltétlenül is jelenti, hogy az összes várólistára helyezett üzenetek fogadott.</span><span class="sxs-lookup"><span data-stu-id="f5f98-158">Note this does not also imply that all queued messages have been received.</span></span> <span data-ttu-id="f5f98-159">Ennek oka hello részét képezi, hogy "all" üzenetek ellenőrzése a nem a determinisztikus művelet.</span><span class="sxs-lookup"><span data-stu-id="f5f98-159">Part of hello reason for this is that checking for "all" messages isn’t as deterministic an action.</span></span> <span data-ttu-id="f5f98-160">Mi történik, ha visszaállíthatja az "összes" köszönőüzenetei, de majd egy másikat toohello eszköz küldött után azonnal?</span><span class="sxs-lookup"><span data-stu-id="f5f98-160">What happens if you retrieve "all" of hello messages, but then another one is sent toohello device immediately after?</span></span> <span data-ttu-id="f5f98-161">A jobb módon toodeal vele programozott időtúllépés jelenti.</span><span class="sxs-lookup"><span data-stu-id="f5f98-161">A better way toodeal with that is with a programmed timeout.</span></span> <span data-ttu-id="f5f98-162">Például hello üzenet visszahívási függvény sikerült alaphelyzetbe állítani egy számlálót minden alkalommal, amikor meghívták.</span><span class="sxs-lookup"><span data-stu-id="f5f98-162">For example, hello message callback function could reset a timer every time it’s invoked.</span></span> <span data-ttu-id="f5f98-163">Majd írhat logika toocontinue feldolgozási, ha például nem érkeztek üzenetek hello a legutóbbi *X* másodperc.</span><span class="sxs-lookup"><span data-stu-id="f5f98-163">You can then write logic toocontinue processing if, for example, no messages have been received in hello last *X* seconds.</span></span>

<span data-ttu-id="f5f98-164">Ha végzett ingressing események van, és fogadja az üzeneteket, lehet, hogy toocall hello megfelelő függvény tooclean erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="f5f98-164">When you’re finished ingressing events and receiving messages, be sure toocall hello corresponding function tooclean up resources.</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="f5f98-165">Alapvetően az API-k toosend csak egy készletét, és az API-t hello nélkül hello háttérszál ugyanaz a háttérszálon és más adatok fogadására.</span><span class="sxs-lookup"><span data-stu-id="f5f98-165">Basically there’s only one set of APIs toosend and receive data with a background thread and another set of APIs that does hello same thing without hello background thread.</span></span> <span data-ttu-id="f5f98-166">Nagy mennyiségű fejlesztők számára előnyös lehet hello nem - inden API-k, de hello alacsonyabb szintű API-k akkor hasznos, ha hello fejlesztői szeretne rendelni a hálózati átvitelt explicit vezérelheti.</span><span class="sxs-lookup"><span data-stu-id="f5f98-166">A lot of developers may prefer hello non-LL APIs, but hello lower-level APIs are useful when hello developer wants explicit control over network transmissions.</span></span> <span data-ttu-id="f5f98-167">Például bizonyos eszközök begyűjtik az adatokat idő és a csak érkező jelzések (például óra egy alkalommal vagy naponta egyszer) meghatározott időközönként.</span><span class="sxs-lookup"><span data-stu-id="f5f98-167">For example, some devices collect data over time and only ingress events at specified intervals (for example, once an hour or once a day).</span></span> <span data-ttu-id="f5f98-168">alacsonyabb szintű API-kat biztosít lehetőséget tooexplicitly vezérlő hello, amikor Ön adatokat küldeni és fogadni az IoT-központ hello.</span><span class="sxs-lookup"><span data-stu-id="f5f98-168">hello lower-level APIs give you hello ability tooexplicitly control when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="f5f98-169">Mások egyszerűen, főként a hello egyszerűség érdekében, hogy alacsonyabb szintű API-k olyan hello.</span><span class="sxs-lookup"><span data-stu-id="f5f98-169">Others will simply prefer hello simplicity that hello lower-level APIs provide.</span></span> <span data-ttu-id="f5f98-170">Minden egyes munka azonban hello háttérben helyett hello fő szálnak történik.</span><span class="sxs-lookup"><span data-stu-id="f5f98-170">Everything happens on hello main thread rather than some work happening in hello background.</span></span>

<span data-ttu-id="f5f98-171">Bármelyik modellt választja, lehet, hogy toobe konzisztens mely API-kat használ.</span><span class="sxs-lookup"><span data-stu-id="f5f98-171">Whichever model you choose, be sure toobe consistent in which APIs you use.</span></span> <span data-ttu-id="f5f98-172">Ha először meghívásával **IoTHubClient\_inden\_CreateFromConnectionString**, alacsonyabb szintű API-khoz, követési munka megfelelő hello csak használnia kell:</span><span class="sxs-lookup"><span data-stu-id="f5f98-172">If you start by calling **IoTHubClient\_LL\_CreateFromConnectionString**, be sure you only use hello corresponding lower-level APIs for any follow-up work:</span></span>

* <span data-ttu-id="f5f98-173">IoTHubClient\_inden\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="f5f98-173">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="f5f98-174">IoTHubClient\_inden\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="f5f98-174">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="f5f98-175">IoTHubClient\_inden\_megszüntetése</span><span class="sxs-lookup"><span data-stu-id="f5f98-175">IoTHubClient\_LL\_Destroy</span></span>
* <span data-ttu-id="f5f98-176">IoTHubClient\_inden\_DoWork</span><span class="sxs-lookup"><span data-stu-id="f5f98-176">IoTHubClient\_LL\_DoWork</span></span>

<span data-ttu-id="f5f98-177">Ellenkező hello is igaz.</span><span class="sxs-lookup"><span data-stu-id="f5f98-177">hello opposite is true as well.</span></span> <span data-ttu-id="f5f98-178">Ha először **IoTHubClient\_CreateFromConnectionString**, akkor használjon hello nem - inden API-k minden további feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="f5f98-178">If you start with **IoTHubClient\_CreateFromConnectionString**, then use hello non-LL APIs for any additional processing.</span></span>

<span data-ttu-id="f5f98-179">Hello Azure IoT-eszközök SDK C-hez, a következő látható: hello **IOT hubbal\_ügyfél\_minta\_http** hello átfogó példát alkalmazás alacsonyabb szintű API-k.</span><span class="sxs-lookup"><span data-stu-id="f5f98-179">In hello Azure IoT device SDK for C, see hello **iothub\_client\_sample\_http** application for a complete example of hello lower-level APIs.</span></span> <span data-ttu-id="f5f98-180">Hello **IOT hubbal\_ügyfél\_minta\_amqp** alkalmazás teljes például a hello nem - inden API-k lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="f5f98-180">hello **iothub\_client\_sample\_amqp** application can be referenced for a full example of hello non-LL APIs.</span></span>

## <a name="property-handling"></a><span data-ttu-id="f5f98-181">Tulajdonság kezelése</span><span class="sxs-lookup"><span data-stu-id="f5f98-181">Property handling</span></span>
<span data-ttu-id="f5f98-182">Amennyiben azt korábban leírt adatokat küldő, ha azt már lett utaló toohello hello üzenet törzsét.</span><span class="sxs-lookup"><span data-stu-id="f5f98-182">So far when we've described sending data, we've been referring toohello body of hello message.</span></span> <span data-ttu-id="f5f98-183">Tegyük fel ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="f5f98-183">For example, consider this code:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="f5f98-184">Ez a példa küld egy üzenet tooIoT Hub hello szövegre "Hello World".</span><span class="sxs-lookup"><span data-stu-id="f5f98-184">This example sends a message tooIoT Hub with hello text "Hello World."</span></span> <span data-ttu-id="f5f98-185">Azonban az IoT-központ lehetővé teszi tulajdonságok toobe csatolt tooeach üzenet.</span><span class="sxs-lookup"><span data-stu-id="f5f98-185">However, IoT Hub also allows properties toobe attached tooeach message.</span></span> <span data-ttu-id="f5f98-186">Tulajdonságok olyan név/érték párok, amelyek csatlakoztatott toohello üzenet lehetnek.</span><span class="sxs-lookup"><span data-stu-id="f5f98-186">Properties are name/value pairs that can be attached toohello message.</span></span> <span data-ttu-id="f5f98-187">Módosíthatja például a Microsoft hello előző kód tooattach tulajdonság toohello üzenet:</span><span class="sxs-lookup"><span data-stu-id="f5f98-187">For example, we can modify hello previous code tooattach a property toohello message:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="f5f98-188">Először meghívásával **IoTHubMessage\_tulajdonságok** , és átadja azt a üzenet hello leíróját.</span><span class="sxs-lookup"><span data-stu-id="f5f98-188">We start by calling **IoTHubMessage\_Properties** and passing it hello handle of our message.</span></span> <span data-ttu-id="f5f98-189">Mi azt vissza van egy **térkép\_KEZELNI** hivatkozás, amely lehetővé teszi velünk toostart tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="f5f98-189">What we get back is a **MAP\_HANDLE** reference that enables us toostart adding properties.</span></span> <span data-ttu-id="f5f98-190">hívása úgy érhető el, ez utóbbi hello **térkép\_AddOrUpdate**, amely fogad egy hivatkozás tooa térkép\_leíró hello tulajdonságnév és hello tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="f5f98-190">hello latter is accomplished by calling **Map\_AddOrUpdate**, which takes a reference tooa MAP\_HANDLE, hello property name, and hello property value.</span></span> <span data-ttu-id="f5f98-191">Ez az API-t hozzá lehessen adni annyi tulajdonságai, például azt.</span><span class="sxs-lookup"><span data-stu-id="f5f98-191">With this API we can add as many properties as we like.</span></span>

<span data-ttu-id="f5f98-192">Ha hello esemény olvasni **Event Hubs**, hello fogadó hello tulajdonságlistázás, és a hozzájuk tartozó értékek beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f5f98-192">When hello event is read from **Event Hubs**, hello receiver can enumerate hello properties and retrieve their corresponding values.</span></span> <span data-ttu-id="f5f98-193">Például a .NET ez lenne elvégezhető hello elérésével [tulajdonsággyűjteményében hello EventData objektum](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span><span class="sxs-lookup"><span data-stu-id="f5f98-193">For example, in .NET this would be accomplished by accessing hello [Properties collection on hello EventData object](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span></span>

<span data-ttu-id="f5f98-194">Hello előző példában csatol azt, hogy küldünk tooIoT Hub tulajdonságainak tooan esemény.</span><span class="sxs-lookup"><span data-stu-id="f5f98-194">In hello previous example, we’re attaching properties tooan event that we send tooIoT Hub.</span></span> <span data-ttu-id="f5f98-195">Tulajdonságok is csatolt toomessages IoT-központ kapott.</span><span class="sxs-lookup"><span data-stu-id="f5f98-195">Properties can also be attached toomessages received from IoT Hub.</span></span> <span data-ttu-id="f5f98-196">Tooretrieve tulajdonságok üzenetből szeretnénk, ha azt az üzenet visszahívási függvény például hello következő kódot használhatja:</span><span class="sxs-lookup"><span data-stu-id="f5f98-196">If we want tooretrieve properties from a message, we can use code such as hello following in our message callback function:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from hello message
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

<span data-ttu-id="f5f98-197">hívás túl hello**IoTHubMessage\_tulajdonságok** értéket ad vissza hello **térkép\_KEZELNI** hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="f5f98-197">hello call too**IoTHubMessage\_Properties** returns hello **MAP\_HANDLE** reference.</span></span> <span data-ttu-id="f5f98-198">Jelenleg akkor továbbítja a hivatkozás túl**térkép\_GetInternals** tooobtain egy hivatkozás tooan tömbje hello név-érték párokat (valamint hello tulajdonságok száma).</span><span class="sxs-lookup"><span data-stu-id="f5f98-198">We then pass that reference too**Map\_GetInternals** tooobtain a reference tooan array of hello name/value pairs (as well as a count of hello properties).</span></span> <span data-ttu-id="f5f98-199">Ezen a ponton számbavétele hello tulajdonságok tooget toohello értékek azt szeretnénk, ha egyszerű kérdése.</span><span class="sxs-lookup"><span data-stu-id="f5f98-199">At that point it's a simple matter of enumerating hello properties tooget toohello values we want.</span></span>

<span data-ttu-id="f5f98-200">Az alkalmazás nem rendelkezik toouse tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f5f98-200">You don't have toouse properties in your application.</span></span> <span data-ttu-id="f5f98-201">Azonban ha tooset kell őket események vagy kérheti le azokat az üzeneteket, hello **IoTHubClient** könyvtár megkönnyíti.</span><span class="sxs-lookup"><span data-stu-id="f5f98-201">However, if you need tooset them on events or retrieve them from messages, hello **IoTHubClient** library makes it easy.</span></span>

## <a name="message-handling"></a><span data-ttu-id="f5f98-202">Üzenet kezelése</span><span class="sxs-lookup"><span data-stu-id="f5f98-202">Message handling</span></span>
<span data-ttu-id="f5f98-203">Ahogy korábban is hangsúlyoztuk, amikor érkező üzenetek IoT-központ hello **IoTHubClient** könyvtár regisztrált visszahívási függvény meghívása válaszol.</span><span class="sxs-lookup"><span data-stu-id="f5f98-203">As stated previously, when messages arrive from IoT Hub hello **IoTHubClient** library responds by invoking a registered callback function.</span></span> <span data-ttu-id="f5f98-204">Ez a funkció, amely további rövid érdemel visszatérési paraméterének van.</span><span class="sxs-lookup"><span data-stu-id="f5f98-204">There is a return parameter of this function that deserves some additional explanation.</span></span> <span data-ttu-id="f5f98-205">Ez egy olyan hello visszahívási függvény hello **IOT hubbal\_ügyfél\_minta\_http** mintaalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="f5f98-205">Here’s an excerpt of hello callback function in hello **iothub\_client\_sample\_http** sample application:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

<span data-ttu-id="f5f98-206">Vegye figyelembe, hogy hello visszatérési típusa **IOTHUBMESSAGE\_törlése\_eredmény** és az adott esetben a rendszer visszaadja a **IOTHUBMESSAGE\_elfogadott**.</span><span class="sxs-lookup"><span data-stu-id="f5f98-206">Note that hello return type is **IOTHUBMESSAGE\_DISPOSITION\_RESULT** and in this particular case we return **IOTHUBMESSAGE\_ACCEPTED**.</span></span> <span data-ttu-id="f5f98-207">Nincsenek más értékek azt adhatja vissza a függvény módosítása hogyan hello **IoTHubClient** könyvtár reagál toohello üzenet visszahívási.</span><span class="sxs-lookup"><span data-stu-id="f5f98-207">There are other values we can return from this function that change how hello **IoTHubClient** library reacts toohello message callback.</span></span> <span data-ttu-id="f5f98-208">Az alábbiakban hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="f5f98-208">Here are hello options.</span></span>

* <span data-ttu-id="f5f98-209">**IOTHUBMESSAGE\_elfogadott** – hello üzenet feldolgozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="f5f98-209">**IOTHUBMESSAGE\_ACCEPTED** – hello message has been processed successfully.</span></span> <span data-ttu-id="f5f98-210">Hello **IoTHubClient** könyvtár nem indítja el hello visszahívási függvényt újra hello ugyanazt az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="f5f98-210">hello **IoTHubClient** library will not invoke hello callback function again with hello same message.</span></span>
* <span data-ttu-id="f5f98-211">**IOTHUBMESSAGE\_elutasítva** – hello üzenet nem lett feldolgozva, és nem desire toodo ezért hello jövőbeli van.</span><span class="sxs-lookup"><span data-stu-id="f5f98-211">**IOTHUBMESSAGE\_REJECTED** – hello message was not processed and there is no desire toodo so in hello future.</span></span> <span data-ttu-id="f5f98-212">Hello **IoTHubClient** könyvtár nem kell meghívnia hello visszahívási függvényt újra hello ugyanazt az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="f5f98-212">hello **IoTHubClient** library should not invoke hello callback function again with hello same message.</span></span>
* <span data-ttu-id="f5f98-213">**IOTHUBMESSAGE\_ABANDONED** – üdvözlőüzenetére nem feldolgozása sikeresen megtörtént, de hello **IoTHubClient** könyvtárban kell meghívnia hello visszahívási függvényt újra hello ugyanazt az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="f5f98-213">**IOTHUBMESSAGE\_ABANDONED** – hello message was not processed successfully, but hello **IoTHubClient** library should invoke hello callback function again with hello same message.</span></span>

<span data-ttu-id="f5f98-214">Hello az első két visszatérési kódok, hello **IoTHubClient** könyvtár küld egy üzenet tooIoT Hub, amely jelzi, hogy üdvözlőüzenetére törli hello eszköz várólistáról legyen, és nem érkezik újra.</span><span class="sxs-lookup"><span data-stu-id="f5f98-214">For hello first two return codes, hello **IoTHubClient** library sends a message tooIoT Hub indicating that hello message should be deleted from hello device queue and not delivered again.</span></span> <span data-ttu-id="f5f98-215">hello nettó hatása van hello azonos (üdvözlőüzenetére hello eszköz várólista törlődik), de e hello elfogadta vagy visszautasított továbbra is rögzíti.</span><span class="sxs-lookup"><span data-stu-id="f5f98-215">hello net effect is hello same (hello message is deleted from hello device queue), but whether hello message was accepted or rejected is still recorded.</span></span>  <span data-ttu-id="f5f98-216">Felvétel a különbség üdvözlőüzenetére hasznos toosenders, akik a visszajelzés figyelésére és megtudhatja, ha egy eszköz elfogadta vagy egy üzenet visszautasította.</span><span class="sxs-lookup"><span data-stu-id="f5f98-216">Recording this distinction is useful toosenders of hello message who can listen for feedback and find out if a device has accepted or rejected a particular message.</span></span>

<span data-ttu-id="f5f98-217">Hello utolsó esetben egy üzenetet is kap tooIoT Hub, de azt jelzi, hogy hello üzenet újbóli kézbesítése kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f5f98-217">In hello last case a message is also sent tooIoT Hub, but it indicates that hello message should be redelivered.</span></span> <span data-ttu-id="f5f98-218">Általában egy üzenetet fog abandon, ha valamilyen hiba előforduló, de tootry tooprocess hello üzenet újra.</span><span class="sxs-lookup"><span data-stu-id="f5f98-218">Typically you’ll abandon a message if you encounter some error but want tootry tooprocess hello message again.</span></span> <span data-ttu-id="f5f98-219">Ezzel szemben egy üzenet elutasítása akkor megfelelő ha helyrehozhatatlan hibát észlel (vagy, egyszerűen dönthet úgy, hogy tooprocess üdvözlőüzenetére).</span><span class="sxs-lookup"><span data-stu-id="f5f98-219">In contrast, rejecting a message is appropriate when you encounter an unrecoverable error (or if you simply decide you don’t want tooprocess hello message).</span></span>

<span data-ttu-id="f5f98-220">Mindenképpen figyelembe hello különböző visszatérési kódokat, hogy akkor is kér hello viselkedés hello a kívánt **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="f5f98-220">In any case, be aware of hello different return codes so that you can elicit hello behavior you want from hello **IoTHubClient** library.</span></span>

## <a name="alternate-device-credentials"></a><span data-ttu-id="f5f98-221">Alternatív hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="f5f98-221">Alternate device credentials</span></span>
<span data-ttu-id="f5f98-222">Ahogy korábban, hello először thing toodo az hello használatakor **IoTHubClient** könyvtárban tooobtain egy **IOT HUBBAL\_ügyfél\_KEZELNI** hello például a következőt hívja a következő:</span><span class="sxs-lookup"><span data-stu-id="f5f98-222">As explained previously, hello first thing toodo when working with hello **IoTHubClient** library is tooobtain a **IOTHUB\_CLIENT\_HANDLE** with a call such as hello following:</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="f5f98-223">argumentum túl hello**IoTHubClient\_CreateFromConnectionString** hello eszköz kapcsolati karakterláncot és a paraméter, amely jelzi az IoT hubbal használjuk toocommunicate hello protokoll.</span><span class="sxs-lookup"><span data-stu-id="f5f98-223">hello arguments too**IoTHubClient\_CreateFromConnectionString** are hello device connection string and a parameter that indicates hello protocol we use toocommunicate with IoT Hub.</span></span> <span data-ttu-id="f5f98-224">hello eszköz kapcsolati karakterlánc formátuma a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="f5f98-224">hello device connection string has a format that appears as follows:</span></span>

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

<span data-ttu-id="f5f98-225">Ez a karakterlánc a négy adatra van: az IoT-központ nevét, az IoT-központ utótag, Eszközazonosító és megosztott elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f5f98-225">There are four pieces of information in this string: IoT Hub name, IoT Hub suffix, device ID, and shared access key.</span></span> <span data-ttu-id="f5f98-226">Hello teljesen minősített tartománynevét (FQDN) az IoT-központ az beszerzése az hello Azure-portálon az IoT hub-példány létrehozásakor – így hello IoT hub name (hello első része hello teljes Tartományneve) és hello IoT hub utótag (hello többi hello FQDN).</span><span class="sxs-lookup"><span data-stu-id="f5f98-226">You obtain hello fully qualified domain name (FQDN) of an IoT hub when you create your IoT hub instance in hello Azure portal — this gives you hello IoT hub name (hello first part of hello FQDN) and hello IoT hub suffix (hello rest of hello FQDN).</span></span> <span data-ttu-id="f5f98-227">Kapott hello Eszközazonosító és hello megosztott elérési kulcsot az eszköz regisztrálása az IoT-központ (lásd: hello [előző cikkben](iot-hub-device-sdk-c-intro.md)).</span><span class="sxs-lookup"><span data-stu-id="f5f98-227">You get hello device ID and hello shared access key when you register your device with IoT Hub (as described in hello [previous article](iot-hub-device-sdk-c-intro.md)).</span></span>

<span data-ttu-id="f5f98-228">**IoTHubClient\_CreateFromConnectionString** egyirányú tooinitialize hello library lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="f5f98-228">**IoTHubClient\_CreateFromConnectionString** gives you one way tooinitialize hello library.</span></span> <span data-ttu-id="f5f98-229">Ha jobban szeret, hozhat létre egy új **IOT HUBBAL\_ügyfél\_KEZELNI** hello eszköz kapcsolati karakterlánc helyett ezeket az egyes paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="f5f98-229">If you prefer, you can create a new **IOTHUB\_CLIENT\_HANDLE** by using these individual parameters rather than hello device connection string.</span></span> <span data-ttu-id="f5f98-230">Ez a kód a következő hello érhető el:</span><span class="sxs-lookup"><span data-stu-id="f5f98-230">This is achieved with hello following code:</span></span>

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

<span data-ttu-id="f5f98-231">Ezt a feladatot el hello ugyanaz, mint **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="f5f98-231">This accomplishes hello same thing as **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="f5f98-232">Nyilvánvaló, hogy érdemes-e toouse tűnhet **IoTHubClient\_CreateFromConnectionString** ahelyett, hogy ez a részletesebb módszer az inicializálás.</span><span class="sxs-lookup"><span data-stu-id="f5f98-232">It may seem obvious that you would want toouse **IoTHubClient\_CreateFromConnectionString** rather than this more verbose method of initialization.</span></span> <span data-ttu-id="f5f98-233">Ne feledje, azonban, hogy amikor regisztrál egy eszközt az IoT-központ tartalmával egy Eszközazonosító és eszközkulcs (kapcsolati karakterlánc).</span><span class="sxs-lookup"><span data-stu-id="f5f98-233">Keep in mind, however, that when you register a device in IoT Hub what you get is a device ID and device key (not a connection string).</span></span> <span data-ttu-id="f5f98-234">Hello *eszköz explorer* hello bevezetett SDK eszköz [előző cikkben](iot-hub-device-sdk-c-intro.md) hello-tárakat használ **Azure IoT szolgáltatás SDK** származó toocreate hello eszköz kapcsolati karakterlánc hello Eszközazonosító eszközkulcs és az IoT-központ állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="f5f98-234">hello *device explorer* SDK tool introduced in hello [previous article](iot-hub-device-sdk-c-intro.md) uses libraries in hello **Azure IoT service SDK** toocreate hello device connection string from hello device ID, device key, and IoT Hub host name.</span></span> <span data-ttu-id="f5f98-235">Így hívása **IoTHubClient\_inden\_létrehozása** oka menti azt előnyösebb lehet hello lépésében létrehozása egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="f5f98-235">So calling **IoTHubClient\_LL\_Create** may be preferable because it saves you hello step of generating a connection string.</span></span> <span data-ttu-id="f5f98-236">Használja az kényelmes bármelyik módszert.</span><span class="sxs-lookup"><span data-stu-id="f5f98-236">Use whichever method is convenient.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="f5f98-237">Konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="f5f98-237">Configuration options</span></span>
<span data-ttu-id="f5f98-238">Amennyiben minden leírt hello módon hello kapcsolatos **IoTHubClient** könyvtár works tükrözi az alapértelmezett viselkedés.</span><span class="sxs-lookup"><span data-stu-id="f5f98-238">So far everything described about hello way hello **IoTHubClient** library works reflects its default behavior.</span></span> <span data-ttu-id="f5f98-239">Van azonban néhány lehetőség, amely lehet toochange hello szalagtár működése.</span><span class="sxs-lookup"><span data-stu-id="f5f98-239">However, there are a few options that you can set toochange how hello library works.</span></span> <span data-ttu-id="f5f98-240">Mindez, ami hello **IoTHubClient\_inden\_SetOption** API.</span><span class="sxs-lookup"><span data-stu-id="f5f98-240">This is accomplished by leveraging hello **IoTHubClient\_LL\_SetOption** API.</span></span> <span data-ttu-id="f5f98-241">Vegye figyelembe az ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="f5f98-241">Consider this example:</span></span>

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

<span data-ttu-id="f5f98-242">Van néhány gyakran használt beállításokat:</span><span class="sxs-lookup"><span data-stu-id="f5f98-242">There are a couple of options that are commonly used:</span></span>

* <span data-ttu-id="f5f98-243">**SetBatching** (logikai) – Ha **igaz**, majd a küldött adatok tooIoT Hub rendszer kötegekben.</span><span class="sxs-lookup"><span data-stu-id="f5f98-243">**SetBatching** (bool) – If **true**, then data sent tooIoT Hub is sent in batches.</span></span> <span data-ttu-id="f5f98-244">Ha **hamis**, majd üzenetküldés külön-külön.</span><span class="sxs-lookup"><span data-stu-id="f5f98-244">If **false**, then messages are sent individually.</span></span> <span data-ttu-id="f5f98-245">hello alapértelmezett érték a **hamis**.</span><span class="sxs-lookup"><span data-stu-id="f5f98-245">hello default is **false**.</span></span> <span data-ttu-id="f5f98-246">Vegye figyelembe, hogy hello **SetBatching** beállítás csak akkor alkalmazható, toohello HTTP protokollt, és nem toohello MQTT vagy AMQP protokollt.</span><span class="sxs-lookup"><span data-stu-id="f5f98-246">Note that hello **SetBatching** option only applies toohello HTTP protocol and not toohello MQTT or AMQP protocols.</span></span>
* <span data-ttu-id="f5f98-247">**Időtúllépés** (előjel nélküli egész) – Ez az érték ezredmásodpercben szerepel.</span><span class="sxs-lookup"><span data-stu-id="f5f98-247">**Timeout** (unsigned int) – This value is represented in milliseconds.</span></span> <span data-ttu-id="f5f98-248">Ha egy HTTP-kérés vagy válasz fogadása ennél hosszabb ideje tart, majd hello kapcsolat időtúllépése küldi.</span><span class="sxs-lookup"><span data-stu-id="f5f98-248">If sending an HTTP request or receiving a response takes longer than this time, then hello connection times out.</span></span>

<span data-ttu-id="f5f98-249">a beállítás kötegelés hello fontos.</span><span class="sxs-lookup"><span data-stu-id="f5f98-249">hello batching option is important.</span></span> <span data-ttu-id="f5f98-250">Alapértelmezés szerint hello könyvtár ingresses események külön-külön (egy adott eseményhez bármilyen át túl**IoTHubClient\_inden\_SendEventAsync**).</span><span class="sxs-lookup"><span data-stu-id="f5f98-250">By default, hello library ingresses events individually (a single event is whatever you pass too**IoTHubClient\_LL\_SendEventAsync**).</span></span> <span data-ttu-id="f5f98-251">Ha kötegelés beállítás hello **igaz**, hello könyvtár eseményeit annyi hello pufferből (felfelé toohello üzenetek maximális méretét, amely elfogadja az IoT-központ) használhatja.</span><span class="sxs-lookup"><span data-stu-id="f5f98-251">If hello batching option is **true**, hello library collects as many events as it can from hello buffer (up toohello maximum message size that IoT Hub will accept).</span></span>  <span data-ttu-id="f5f98-252">hello esemény kötegelt küldött tooIoT központ egyetlen HTTP hívás (hello események, egy JSON-tömb be vannak becsomagolva).</span><span class="sxs-lookup"><span data-stu-id="f5f98-252">hello event batch is sent tooIoT Hub in a single HTTP call (hello individual events are bundled into a JSON array).</span></span> <span data-ttu-id="f5f98-253">Általában kötegelés engedélyezését eredményezi nagy teljesítménynövekedéshez óta hálózati üzenetváltások most csökkentése.</span><span class="sxs-lookup"><span data-stu-id="f5f98-253">Enabling batching typically results in big performance gains since you’re reducing network round-trips.</span></span> <span data-ttu-id="f5f98-254">Mivel az esemény-es HTTP-fejlécek egy készletét, nem pedig a minden egyes esemény fejléc készlettel küldi is jelentősen csökkenti a sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="f5f98-254">It also significantly reduces bandwidth since you are sending one set of HTTP headers with an event batch rather than a set of headers for each individual event.</span></span> <span data-ttu-id="f5f98-255">Ha egy konkrét Okkód toodo egyébként nincs, általában érdemes tooenable kötegelés.</span><span class="sxs-lookup"><span data-stu-id="f5f98-255">Unless you have a specific reason toodo otherwise, typically you’ll want tooenable batching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5f98-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f5f98-256">Next steps</span></span>
<span data-ttu-id="f5f98-257">Ez a cikk ismerteti a részletes hello viselkedését hello **IoTHubClient** függvénytár szerepel a hello **C-hez készült SDK Azure IoT-eszközök**. Az információ, rendelkeznie kell hello hello funkcióinak beható ismerete **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="f5f98-257">This article describes in detail hello behavior of hello **IoTHubClient** library found in hello **Azure IoT device SDK for C**. With this information, you should have a good understanding of hello capabilities of hello **IoTHubClient** library.</span></span> <span data-ttu-id="f5f98-258">Hello [tovább cikk](iot-hub-device-sdk-c-serializer.md) hasonló részletesen ismerteti a hello **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="f5f98-258">hello [next article](iot-hub-device-sdk-c-serializer.md) provides similar detail on hello **serializer** library.</span></span>

<span data-ttu-id="f5f98-259">az IoT-központ fejlesztésével kapcsolatos további toolearn lásd: hello [Azure IoT SDK-k][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="f5f98-259">toolearn more about developing for IoT Hub, see hello [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="f5f98-260">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="f5f98-260">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="f5f98-261">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="f5f98-261">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
