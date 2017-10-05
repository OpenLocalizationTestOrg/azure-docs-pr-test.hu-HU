---
title: "Az Azure IoT-eszközök SDK c - szerializáló |} Microsoft Docs"
description: "Hogyan használható az Azure IoT-eszközök C-hez készült SDK a szerializáló szalagtár kommunikáló eszközön futó alkalmazások létrehozásához az IoT-központ számára."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: defbed34-de73-429c-8592-cd863a38e4dd
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2016
ms.author: obloch
ms.openlocfilehash: aa03c29c54d75538b1fdf987cac5f09d5d344f73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a><span data-ttu-id="c6e1f-103">Az Azure IoT-eszközök SDK c – további információk a szerializáló</span><span class="sxs-lookup"><span data-stu-id="c6e1f-103">Azure IoT device SDK for C – more about serializer</span></span>
<span data-ttu-id="c6e1f-104">A [először a következő cikket:](iot-hub-device-sdk-c-intro.md) a sorozat bevezette a **C-hez készült SDK Azure IoT-eszközök**. A következő cikk megadott részletes leírása a [ **IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-104">The [first article](iot-hub-device-sdk-c-intro.md) in this series introduced the **Azure IoT device SDK for C**. The next article provided a more detailed description of the [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="c6e1f-105">Ez a cikk az SDK körét befejeződik, adja meg a részletes leírását, a többi összetevő: a **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-105">This article completes coverage of the SDK by providing a more detailed description of the remaining component: the **serializer** library.</span></span>

<span data-ttu-id="c6e1f-106">A bevezető cikkben leírt használata a **szerializáló** események üzeneteket küldjön és fogadjon az IoT-központ könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-106">The introductory article described how to use the **serializer** library to send events to and receive messages from IoT Hub.</span></span> <span data-ttu-id="c6e1f-107">Ebben a cikkben tovább bővítjük ezekre a kérdésekre adott válaszokat azáltal, hogy egy több teljes magyarázatát, hogy hogyan a következő modellre: az adatok a **szerializáló** makró nyelv.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-107">In this article, we extend that discussion by providing a more complete explanation of how to model your data with the **serializer** macro language.</span></span> <span data-ttu-id="c6e1f-108">A cikk is tartalmaz, hogy a szalagtár rendezi sorba üzenetek további információkra (és egyes esetekben, hogy miként szabályozható a szerializálási viselkedés).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-108">The article also includes more detail about how the library serializes messages (and in some cases how you can control the serialization behavior).</span></span> <span data-ttu-id="c6e1f-109">Egyes paraméterek módosíthatja a modellek méretének meghatározó is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-109">We'll also describe some parameters you can modify that determine the size of the models you create.</span></span>

<span data-ttu-id="c6e1f-110">Végezetül a cikk revisits néhány témaköre például üzenet és a tulajdonság kezelési előző cikkek tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-110">Finally, the article revisits some topics covered in previous articles such as message and property handling.</span></span> <span data-ttu-id="c6e1f-111">Mint azt fogja megtudhatja, ezen szolgáltatások munkahelyi ugyanazon módon használatával a **szerializáló** könyvtár, mint az a **IoTHubClient** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-111">As we'll find out, those features work in the same way using the **serializer** library as they do with the **IoTHubClient** library.</span></span>

<span data-ttu-id="c6e1f-112">Minden ebben a cikkben leírtak alapján a **szerializáló** SDK minták.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-112">Everything described in this article is based on the **serializer** SDK samples.</span></span> <span data-ttu-id="c6e1f-113">Ha azt szeretné, követéséhez, tekintse meg a **simplesample\_amqp** és **simplesample\_http** alkalmazások az Azure IoT-eszközök SDK szerepel a c kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-113">If you want to follow along, see the **simplesample\_amqp** and **simplesample\_http** applications included in the Azure IoT device SDK for C.</span></span>

<span data-ttu-id="c6e1f-114">Megtalálhatja az [ **C-hez készült SDK Azure IoT-eszközök** ](https://github.com/Azure/azure-iot-sdk-c) GitHub tárház és nézet adatai az API-nak a [C API-referencia](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-114">You can find the [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of the API in the [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="the-modeling-language"></a><span data-ttu-id="c6e1f-115">A modellezési nyelv</span><span class="sxs-lookup"><span data-stu-id="c6e1f-115">The modeling language</span></span>
<span data-ttu-id="c6e1f-116">A [bevezető cikkben](iot-hub-device-sdk-c-intro.md) a sorozat bevezetni a **C-hez készült SDK Azure IoT-eszközök** modellezési nyelvi keresztül a megadott példa a **simplesample\_amqp** alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-116">The [introductory article](iot-hub-device-sdk-c-intro.md) in this series introduced the **Azure IoT device SDK for C** modeling language through the example provided in the **simplesample\_amqp** application:</span></span>

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

<span data-ttu-id="c6e1f-117">Ahogy látja, a modellezési nyelvi C makrók alapul.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-117">As you can see, the modeling language is based on C macros.</span></span> <span data-ttu-id="c6e1f-118">Mindig megkezdi a definíció **BEGIN\_NÉVTÉR** és mindig végződhet **END\_NÉVTÉR**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-118">You always begin your definition with **BEGIN\_NAMESPACE** and always end with **END\_NAMESPACE**.</span></span> <span data-ttu-id="c6e1f-119">A névtér nevét, a vállalat, vagy a példában a projekt, amely dolgozik közös.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-119">It's common to name the namespace for your company or, as in this example, the project that you're working on.</span></span>

<span data-ttu-id="c6e1f-120">Mi kerül a névtéren belül modell definíciókat.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-120">What goes inside the namespace are model definitions.</span></span> <span data-ttu-id="c6e1f-121">Ebben az esetben van egy anemometer egyetlen modellt.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-121">In this case, there is a single model for an anemometer.</span></span> <span data-ttu-id="c6e1f-122">Ebben az esetben a modell neve a következő lehet bármi, de általában ez nevű az eszköz vagy az IoT-központ való kívánt adatok típusát.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-122">Once again, the model can be named anything, but typically this is named for the device or type of data you want to exchange with IoT Hub.</span></span>  

<span data-ttu-id="c6e1f-123">Modellek tartalmazza a következő definícióját az események irányuló, az IoT-központ is (a *adatok*) és az IoT-központ fogadhat üzeneteket (a *műveletek*).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-123">Models contain a definition of the events you can ingress to IoT Hub (the *data*) as well as the messages you can receive from IoT Hub (the *actions*).</span></span> <span data-ttu-id="c6e1f-124">Ahogy látja, a példában, események rendelkezik-e egy típust és egy nevet; műveletek nevét és a választható paraméterek: (minden típusú) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-124">As you can see from the example, events have a type and a name; actions have a name and optional parameters (each with a type).</span></span>

<span data-ttu-id="c6e1f-125">Mi nem mutatja be ezt a mintát olyan további adatokat az SDK által támogatott.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-125">What’s not demonstrated in this sample are additional data types that are supported by the SDK.</span></span> <span data-ttu-id="c6e1f-126">Oktatóanyag a következőket ismerteti, hogy a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-126">We'll cover that next.</span></span>

> [!NOTE]
> <span data-ttu-id="c6e1f-127">Az IoT-központ hivatkozik rá küld egy eszközt az adatok *események*, míg a modellezési nyelvi hivatkozik rá *adatok* (segítségével meghatározott **WITH_DATA**).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-127">IoT Hub refers to the data a device sends to it as *events*, while the modeling language refers to it as *data* (defined using **WITH_DATA**).</span></span> <span data-ttu-id="c6e1f-128">Hasonlóképpen, az IoT-központ hivatkozik-e az eszközök és küldött adatok *üzenetek*, míg a modellezési nyelvi hivatkozik rá *műveletek* (segítségével meghatározott **WITH_ACTION**).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-128">Likewise, IoT Hub refers to the data you send to devices as *messages*, while the modeling language refers to it as *actions* (defined using **WITH_ACTION**).</span></span> <span data-ttu-id="c6e1f-129">Vegye figyelembe, hogy ezek a feltételek szabadon felcserélhetők ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-129">Be aware that these terms may be used interchangeably in this article.</span></span>
> 
> 

### <a name="supported-data-types"></a><span data-ttu-id="c6e1f-130">Támogatott adattípusokat</span><span class="sxs-lookup"><span data-stu-id="c6e1f-130">Supported data types</span></span>
<span data-ttu-id="c6e1f-131">A következő típusok támogatottak a létrehozott modelleket a **szerializáló** könyvtár:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-131">The following data types are supported in models created with the **serializer** library:</span></span>

| <span data-ttu-id="c6e1f-132">Típus</span><span class="sxs-lookup"><span data-stu-id="c6e1f-132">Type</span></span> | <span data-ttu-id="c6e1f-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="c6e1f-133">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c6e1f-134">Dupla</span><span class="sxs-lookup"><span data-stu-id="c6e1f-134">double</span></span> |<span data-ttu-id="c6e1f-135">dupla pontosságú lebegőpontos szám</span><span class="sxs-lookup"><span data-stu-id="c6e1f-135">double precision floating point number</span></span> |
| <span data-ttu-id="c6e1f-136">int</span><span class="sxs-lookup"><span data-stu-id="c6e1f-136">int</span></span> |<span data-ttu-id="c6e1f-137">32 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="c6e1f-137">32 bit integer</span></span> |
| <span data-ttu-id="c6e1f-138">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="c6e1f-138">float</span></span> |<span data-ttu-id="c6e1f-139">az egyszeres pontosságú lebegőpontos szám</span><span class="sxs-lookup"><span data-stu-id="c6e1f-139">single precision floating point number</span></span> |
| <span data-ttu-id="c6e1f-140">hosszú</span><span class="sxs-lookup"><span data-stu-id="c6e1f-140">long</span></span> |<span data-ttu-id="c6e1f-141">hosszú egész szám</span><span class="sxs-lookup"><span data-stu-id="c6e1f-141">long integer</span></span> |
| <span data-ttu-id="c6e1f-142">int8\_t</span><span class="sxs-lookup"><span data-stu-id="c6e1f-142">int8\_t</span></span> |<span data-ttu-id="c6e1f-143">8 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="c6e1f-143">8 bit integer</span></span> |
| <span data-ttu-id="c6e1f-144">Int16\_t</span><span class="sxs-lookup"><span data-stu-id="c6e1f-144">int16\_t</span></span> |<span data-ttu-id="c6e1f-145">16 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="c6e1f-145">16 bit integer</span></span> |
| <span data-ttu-id="c6e1f-146">Int32\_t</span><span class="sxs-lookup"><span data-stu-id="c6e1f-146">int32\_t</span></span> |<span data-ttu-id="c6e1f-147">32 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="c6e1f-147">32 bit integer</span></span> |
| <span data-ttu-id="c6e1f-148">Int64\_t</span><span class="sxs-lookup"><span data-stu-id="c6e1f-148">int64\_t</span></span> |<span data-ttu-id="c6e1f-149">64 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="c6e1f-149">64 bit integer</span></span> |
| <span data-ttu-id="c6e1f-150">logikai érték</span><span class="sxs-lookup"><span data-stu-id="c6e1f-150">bool</span></span> |<span data-ttu-id="c6e1f-151">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="c6e1f-151">boolean</span></span> |
| <span data-ttu-id="c6e1f-152">ASCII\_char\_ptr</span><span class="sxs-lookup"><span data-stu-id="c6e1f-152">ascii\_char\_ptr</span></span> |<span data-ttu-id="c6e1f-153">ASCII-karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c6e1f-153">ASCII string</span></span> |
| <span data-ttu-id="c6e1f-154">EDM\_DÁTUM\_IDŐ\_ELTOLÁSA</span><span class="sxs-lookup"><span data-stu-id="c6e1f-154">EDM\_DATE\_TIME\_OFFSET</span></span> |<span data-ttu-id="c6e1f-155">dátum idő eltolása</span><span class="sxs-lookup"><span data-stu-id="c6e1f-155">date time offset</span></span> |
| <span data-ttu-id="c6e1f-156">EDM\_GUID</span><span class="sxs-lookup"><span data-stu-id="c6e1f-156">EDM\_GUID</span></span> |<span data-ttu-id="c6e1f-157">GUID</span><span class="sxs-lookup"><span data-stu-id="c6e1f-157">GUID</span></span> |
| <span data-ttu-id="c6e1f-158">EDM\_BINÁRIS</span><span class="sxs-lookup"><span data-stu-id="c6e1f-158">EDM\_BINARY</span></span> |<span data-ttu-id="c6e1f-159">Bináris</span><span class="sxs-lookup"><span data-stu-id="c6e1f-159">binary</span></span> |
| <span data-ttu-id="c6e1f-160">DEKLARÁLJA\_STRUKTÚRA</span><span class="sxs-lookup"><span data-stu-id="c6e1f-160">DECLARE\_STRUCT</span></span> |<span data-ttu-id="c6e1f-161">Összetett adattípusú</span><span class="sxs-lookup"><span data-stu-id="c6e1f-161">complex data type</span></span> |

<span data-ttu-id="c6e1f-162">Kezdjük az utolsó típusát.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-162">Let’s start with the last data type.</span></span> <span data-ttu-id="c6e1f-163">A **DECLARE\_STRUCT** összetett adattípusú, amelyek csoportosításait. emellett az egyéb egyszerű típusú adható meg.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-163">The **DECLARE\_STRUCT** allows you to define complex data types, which are groupings of the other primitive types.</span></span> <span data-ttu-id="c6e1f-164">Ezek a Csoportosítások lehetővé teszik a számunkra, amely a következőképpen néz ki a modellek meghatározásához:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-164">These groupings allow us to define a model that looks like this:</span></span>

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

<span data-ttu-id="c6e1f-165">A modell típusú egyetlen eseményt tartalmaz **TestType**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-165">Our model contains a single data event of type **TestType**.</span></span> <span data-ttu-id="c6e1f-166">**TestType** több tag, amelyek együttesen bemutatása által támogatott primitív típusok tartalmazó összetett típus a **szerializáló** modeling language.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-166">**TestType** is a complex type that includes several members, which collectively demonstrate the primitive types supported by the **serializer** modeling language.</span></span>

<span data-ttu-id="c6e1f-167">Az ilyen modellek azt is írhat kódot szeretnék adatokat küldeni a IoT-központ, amely a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-167">With a model like this, we can write code to send data to IoT Hub that appears as follows:</span></span>

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

<span data-ttu-id="c6e1f-168">Alapvetően, azt még hozzárendelése értéket minden tagja a **teszt** szerkezetét, és azt, majd hívja **sendasync metódusok párhuzamosan** küldeni a **teszt** adatok esemény a felhőbe.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-168">Basically, we’re assigning a value to every member of the **Test** structure and then calling **SendAsync** to send the **Test** data event to the cloud.</span></span> <span data-ttu-id="c6e1f-169">**Sendasync metódusok párhuzamosan** küld egy egyetlen eseményt az IoT hubhoz segítő függvény:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-169">**SendAsync** is a helper function that sends a single data event to IoT Hub:</span></span>

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

<span data-ttu-id="c6e1f-170">Ez a függvény a megadott esemény rendezi sorba, és elküldi IoT-központ használatával **IoTHubClient\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-170">This function serializes the given data event and sends it to IoT Hub using **IoTHubClient\_SendEventAsync**.</span></span> <span data-ttu-id="c6e1f-171">Ez az előző cikkekben ismertetett ugyanazt a kódot (**sendasync metódusok párhuzamosan** magában foglalja a logika egy kényelmes függvénynek).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-171">This is the same code discussed in previous articles (**SendAsync** encapsulates the logic into a convenient function).</span></span>

<span data-ttu-id="c6e1f-172">Egy előző kóddal használt egyéb segítő függvény **GetDateTimeOffset**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-172">One other helper function used in the previous code is **GetDateTimeOffset**.</span></span> <span data-ttu-id="c6e1f-173">Ez a függvény a megadott időn átalakítja típusú érték a **EDM\_dátum\_idő\_ELTOLÁS**:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-173">This function transforms the given time into a value of type **EDM\_DATE\_TIME\_OFFSET**:</span></span>

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

<span data-ttu-id="c6e1f-174">Ha ezt a kódot, a következő üzenetet küld az IoT-központ:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-174">If you run this code, the following message is sent to IoT Hub:</span></span>

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

<span data-ttu-id="c6e1f-175">Vegye figyelembe, hogy a szerializálás JSON, amely formátuma az előállított által a **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-175">Note that the serialization is JSON, which is the format generated by the **serializer** library.</span></span> <span data-ttu-id="c6e1f-176">Vegye figyelembe azt is, hogy a szerializált JSON-objektum minden egyes tagjára megegyezik-e a tagjai a **TestType** , amely meghatározott a modellben.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-176">Also note that each member of the serialized JSON object matches the members of the **TestType** that we defined in our model.</span></span> <span data-ttu-id="c6e1f-177">Az értékek is pontosan megegyeznek a kódban használt.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-177">The values also exactly match those used in the code.</span></span> <span data-ttu-id="c6e1f-178">Azonban vegye figyelembe, hogy a bináris adatok base64-kódolású: "AQID" a base64 kódolása {0x01, 0x02, 0x03}.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-178">However, note that the binary data is base64-encoded: "AQID" is the base64 encoding of {0x01, 0x02, 0x03}.</span></span>

<span data-ttu-id="c6e1f-179">Ez a példa bemutatja, hogyan használatának előnye a **szerializáló** library – lehetővé teszi, hogy JSON küldjük a felhőbe, anélkül, hogy az alkalmazás a szerializálási explicit módon kezelésére.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-179">This example demonstrates the advantage of using the **serializer** library -- it enables us to send JSON to the cloud, without having to explicitly deal with serialization in our application.</span></span> <span data-ttu-id="c6e1f-180">Összes foglalkoznia kell van állítsa be az értékeket az adatok események tekinthetők, és ezeket az eseményeket a felhőbe küldése egyszerű API-k majd hívásakor.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-180">All we have to worry about is setting the values of the data events in our model and then calling simple APIs to send those events to the cloud.</span></span>

<span data-ttu-id="c6e1f-181">Az információ azt adhatja meg a modellek, amelyek közé tartozik a támogatott adattípusok, beleértve a komplex típusok (sikerült is magában foglalja az összetett típusok más összetett típusok belül) tartományán.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-181">With this information, we can define models that include the range of supported data types, including complex types (we could even include complex types within other complex types).</span></span> <span data-ttu-id="c6e1f-182">Azonban, hogy szerializálni által létrehozott JSON a fenti példában egy fontos pont megjelenik.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-182">However, he serialized JSON generated by the example above brings up an important point.</span></span> <span data-ttu-id="c6e1f-183">*Hogyan* adatok küldünk a **szerializáló** könyvtár meghatározza, hogy pontosan hogyan a JSON formátuma.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-183">*How* we send data with the **serializer** library determines exactly how the JSON is formed.</span></span> <span data-ttu-id="c6e1f-184">Hogy az adott pont áll, mi oktatóanyag a következőket ismerteti mellett.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-184">That particular point is what we'll cover next.</span></span>

## <a name="more-about-serialization"></a><span data-ttu-id="c6e1f-185">További információ a szerializálási</span><span class="sxs-lookup"><span data-stu-id="c6e1f-185">More about serialization</span></span>
<span data-ttu-id="c6e1f-186">Az előző szakaszban által létrehozott kimeneti példát kiemeli a **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-186">The previous section highlights an example of the output generated by the **serializer** library.</span></span> <span data-ttu-id="c6e1f-187">Ebben a részben azt ismertetjük, hogyan rendezi sorba a könyvtárban az adatokat, és hogy miként szabályozható, hogy a szerializálás API-k használatával viselkedését.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-187">In this section, we'll explain how the library serializes data and how you can control that behavior using the serialization APIs.</span></span>

<span data-ttu-id="c6e1f-188">Ahhoz, hogy előre a szerializálási döntéseken, dolgozunk lesz az új modell egy termosztát alapján.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-188">In order to advance the discussion on serialization, we'll work with a new model based on a thermostat.</span></span> <span data-ttu-id="c6e1f-189">Először hozzuk adja meg a bizonyos tapasztalattal a forgatókönyvön keresztül próbáljuk cím.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-189">First, let's provide some background on the scenario we're trying to address.</span></span>

<span data-ttu-id="c6e1f-190">A modell egy termosztát, és mérheti a hőmérséklet és a páratartalom szeretnénk.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-190">We want to model a thermostat that measures temperature and humidity.</span></span> <span data-ttu-id="c6e1f-191">Minden adat küldendő IoT-központ eltérő lesz.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-191">Each piece of data is going to be sent to IoT Hub differently.</span></span> <span data-ttu-id="c6e1f-192">Alapértelmezés szerint a termosztát ingresses egy hőmérséklet esemény egyszer 2 percenként; a páratartalom esemény ingressed 15 percenként egyszer.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-192">By default, the thermostat ingresses a temperature event once every 2 minutes; a humidity event is ingressed once every 15 minutes.</span></span> <span data-ttu-id="c6e1f-193">Ha bármelyik esemény ingressed, tartalmaznia kell egy Timestamp típusú, amely a időt jeleníti meg, hogy a megfelelő hőmérséklet és a páratartalom mérték.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-193">When either event is ingressed, it must include a timestamp that shows the time that the corresponding temperature or humidity was measured.</span></span>

<span data-ttu-id="c6e1f-194">Ebben a forgatókönyvben, az adatok két különböző módon lesz bemutatjuk, és azt ismertetjük, hogy a hatást, hogy a szerializált kimeneti modellezési rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-194">Given this scenario, we'll demonstrate two different ways to model the data, and we'll explain the effect that modeling has on the serialized output.</span></span>

### <a name="model-1"></a><span data-ttu-id="c6e1f-195">1 modell</span><span class="sxs-lookup"><span data-stu-id="c6e1f-195">Model 1</span></span>
<span data-ttu-id="c6e1f-196">Az első verzió-modell, amely támogatja az előző példában a következő:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-196">Here's the first version of a model that supports the previous scenario:</span></span>

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

<span data-ttu-id="c6e1f-197">Vegye figyelembe, hogy a modell tartalmaz-e a két adatok események: **hőmérséklet** és **páratartalom**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-197">Note that the model includes two data events: **Temperature** and **Humidity**.</span></span> <span data-ttu-id="c6e1f-198">Előző példák ellentétben minden esemény típus megadott struktúra **DECLARE\_STRUCT**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-198">Unlike previous examples, the type of each event is a structure defined using **DECLARE\_STRUCT**.</span></span> <span data-ttu-id="c6e1f-199">**TemperatureEvent** tartalmaz egy hőmérséklet mérési és egy időbélyegzőt adnak; **HumidityEvent** páratartalom a mérés és egy időbélyegzőt adnak tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-199">**TemperatureEvent** includes a temperature measurement and a timestamp; **HumidityEvent** contains a humidity measurement and a timestamp.</span></span> <span data-ttu-id="c6e1f-200">Ez a modell lehetőséget nyújt nekünk természetes az adatok a fent leírt forgatókönyv esetében.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-200">This model gives us a natural way to model the data for the scenario described above.</span></span> <span data-ttu-id="c6e1f-201">Ha az esemény nem küldeni a felhőben, hőmérséklet/időbélyeg vagy páratartalom/időbélyeg-párból vagy küldünk.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-201">When we send an event to the cloud, we'll either send a temperature/timestamp or a humidity/timestamp pair.</span></span>

<span data-ttu-id="c6e1f-202">Hőmérséklet-esemény elküldhetjük a felhőre, például az alábbi kód használatával:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-202">We can send a temperature event to the cloud using code such as the following:</span></span>

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="c6e1f-203">Rendszer fogja változtatható értékek használata a hőmérséklet és a páratartalom példakód, de a képzelhető el, hogy azt ténylegesen olvas be ezeket az értékeket a termosztát a megfelelő érzékelők mintavétellel.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-203">We'll use hard-coded values for temperature and humidity in the sample code, but imagine that we’re actually retrieving these values by sampling the corresponding sensors on the thermostat.</span></span>

<span data-ttu-id="c6e1f-204">A fenti által használt kódot a **GetDateTimeOffset** korábban bevezetett segítő.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-204">The code above uses the **GetDateTimeOffset** helper that was introduced previously.</span></span> <span data-ttu-id="c6e1f-205">Törölje későbbi lesz okokból ezt a kódot explicit módon elválasztja a tevékenység szerializálása során, és az esemény küldése.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-205">For reasons that will become clear later, this code explicitly separates the task of serializing and sending the event.</span></span> <span data-ttu-id="c6e1f-206">Az előző kód a hőmérséklet esemény be pufferbe rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-206">The previous code serializes the temperature event into a buffer.</span></span> <span data-ttu-id="c6e1f-207">Ezt követően **sendMessage** segítő függvény (szereplő **simplesample\_amqp**) IoT-központ, amely elküldi az esemény:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-207">Then, **sendMessage** is a helper function (included in **simplesample\_amqp**) that sends the event to IoT Hub:</span></span>

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

<span data-ttu-id="c6e1f-208">Ez a kód alkészlete a **sendasync metódusok párhuzamosan** segítő az előző szakaszban leírt, így azt nem ismerteti azt újra ide.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-208">This code is a subset of the **SendAsync** helper described in the previous section, so we won’t go over it again here.</span></span>

<span data-ttu-id="c6e1f-209">Az előző kód küldése az hőmérséklet esemény futtatásakor az esemény a szerializált képernyőn zajlik az IoT hubhoz:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-209">When we run the previous code to send the Temperature event, this serialized form of the event is sent to IoT Hub:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="c6e1f-210">Azért küldjük egy hőmérséklet, amelynek a típusa **TemperatureEvent** és, hogy a struct tartalmaz egy **hőmérséklet** és **idő** tag.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-210">We're sending a temperature which is of type **TemperatureEvent** and that struct contains a **Temperature** and **Time** member.</span></span> <span data-ttu-id="c6e1f-211">Ez közvetlenül a szerializált adatokban is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-211">This is directly reflected in the serialized data.</span></span>

<span data-ttu-id="c6e1f-212">Hasonlóképpen elküldhetjük a páratartalom esemény ezzel a kóddal:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-212">Similarly, we can send a humidity event with this code:</span></span>

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="c6e1f-213">A szerializált formában küldi el az IoT-központ a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-213">The serialized form that’s sent to IoT Hub appears as follows:</span></span>

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="c6e1f-214">Ebben az esetben ez az elvárt módon.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-214">Again, this is as expected.</span></span>

<span data-ttu-id="c6e1f-215">Ebben a modellben hogyan további események is tekinthető könnyen lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-215">With this model, you can imagine how additional events can easily be added.</span></span> <span data-ttu-id="c6e1f-216">Adhat meg további struktúrák használatával **DECLARE\_STRUCT**, és adja meg a kapcsolódó esemény a modell használatával **WITH\_adatok**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-216">You define more structures using **DECLARE\_STRUCT**, and include the corresponding event in the model using **WITH\_DATA**.</span></span>

<span data-ttu-id="c6e1f-217">Most módosítsa a modellt, hogy ugyanazokat az adatokat tartalmazza, de különböző struktúrával.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-217">Now, let’s modify the model so that it includes the same data but with a different structure.</span></span>

### <a name="model-2"></a><span data-ttu-id="c6e1f-218">A modell 2</span><span class="sxs-lookup"><span data-stu-id="c6e1f-218">Model 2</span></span>
<span data-ttu-id="c6e1f-219">Vegye figyelembe a fentiekhez alternatív modell:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-219">Consider this alternative model to the one above:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="c6e1f-220">Ebben az esetben azt biztosan nem a **DECLARE\_STRUCT** makrók és egyszerűen meghatározásakor a adatelemek a mi esetünkben a modellezési nyelven történő egyszerű típus használatával.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-220">In this case we've eliminated the **DECLARE\_STRUCT** macros and are simply defining the data items from our scenario using simple types from the modeling language.</span></span>

<span data-ttu-id="c6e1f-221">Csak a pillanatra most figyelmen kívül hagyja a **idő** esemény.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-221">Just for the moment let’s ignore the **Time** event.</span></span> <span data-ttu-id="c6e1f-222">Az adott tartalékoljon, ez a kód érkező **hőmérséklet**:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-222">With that aside, here’s the code to ingress **Temperature**:</span></span>

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="c6e1f-223">Ez a kód a következő szerializált eseményt küld az IoT-központ:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-223">This code sends the following serialized event to IoT Hub:</span></span>

```
{"Temperature":75}
```

<span data-ttu-id="c6e1f-224">És a páratartalom esemény küldése a kódját a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-224">And the code for sending the Humidity event appears as follows:</span></span>

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="c6e1f-225">Ez a kód küld a Ez az IoT-központ:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-225">This code sends this to IoT Hub:</span></span>

```
{"Humidity":45}
```

<span data-ttu-id="c6e1f-226">Amennyiben még vannak nem meglepetések számát.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-226">So far there are still no surprises.</span></span> <span data-ttu-id="c6e1f-227">Most módosítsuk a SZERIALIZÁLÁSA makró felhasználási módját.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-227">Now let's change how we use the SERIALIZE macro.</span></span>

<span data-ttu-id="c6e1f-228">A **SZERIALIZÁLÁSA** makró több adat eseményeket, mint szerepkör argumentumokban vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-228">The **SERIALIZE** macro can take multiple data events as arguments.</span></span> <span data-ttu-id="c6e1f-229">Ez lehetővé teszi, hogy szerializálni a **hőmérséklet** és **páratartalom** esemény együtt, és küldje el az IoT-központ egy hívásban:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-229">This enables us to serialize the **Temperature** and **Humidity** event together and send them to IoT Hub in one call:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="c6e1f-230">Előfordulhat, hogy kitalálja, hogy ez a kód eredménye, hogy két adatok események küldhetők az IoT-központ:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-230">You might guess that the result of this code is that two data events are sent to IoT Hub:</span></span>

<span data-ttu-id="c6e1f-231">[</span><span class="sxs-lookup"><span data-stu-id="c6e1f-231">[</span></span>

<span data-ttu-id="c6e1f-232">{"Hőmérséklet": 75},</span><span class="sxs-lookup"><span data-stu-id="c6e1f-232">{"Temperature":75},</span></span>

<span data-ttu-id="c6e1f-233">{"Páratartalom": 45}</span><span class="sxs-lookup"><span data-stu-id="c6e1f-233">{"Humidity":45}</span></span>

<span data-ttu-id="c6e1f-234">]</span><span class="sxs-lookup"><span data-stu-id="c6e1f-234">]</span></span>

<span data-ttu-id="c6e1f-235">Ez azt jelenti, előfordulhat, hogy várt, hogy ez a kód megegyezik a küldő **hőmérséklet** és **páratartalom** külön-külön.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-235">In other words, you might expect that this code is the same as sending **Temperature** and **Humidity** separately.</span></span> <span data-ttu-id="c6e1f-236">Mindkét eseményeket továbbítani csupán a könnyebb **SZERIALIZÁLÁSA** az adott hívásban.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-236">It’s just a convenience to pass both events to **SERIALIZE** in the same call.</span></span> <span data-ttu-id="c6e1f-237">Azonban, amely nem a helyzet.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-237">However, that’s not the case.</span></span> <span data-ttu-id="c6e1f-238">Ehelyett a fenti kódot az IoT hubhoz küld a egyetlen eseményt:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-238">Instead, the code above sends this single data event to IoT Hub:</span></span>

<span data-ttu-id="c6e1f-239">{"Hőmérséklet": 75, "páratartalom": 45}</span><span class="sxs-lookup"><span data-stu-id="c6e1f-239">{"Temperature":75, "Humidity":45}</span></span>

<span data-ttu-id="c6e1f-240">Ez úgy látszhat furcsa, mert a modell meghatározása **hőmérséklet** és **páratartalom** , két *külön* események:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-240">This may seem strange because our model defines **Temperature** and **Humidity** as two *separate* events:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="c6e1f-241">Több a pontra jelenleg nem a modell ezek az események hol **hőmérséklet** és **páratartalom** ugyanazon szerkezetben vannak:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-241">More to the point, we didn’t model these events where **Temperature** and **Humidity** are in the same structure:</span></span>

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

<span data-ttu-id="c6e1f-242">Ez a modell esetén könnyebben érthetőek legyenek lenne hogyan **hőmérséklet** és **páratartalom** küldheti az adatokat az azonos szerializált üzenet.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-242">If we used this model, it would be easier to understand how **Temperature** and **Humidity** would be sent in the same serialized message.</span></span> <span data-ttu-id="c6e1f-243">Azonban nem lehet egyértelmű ezért működik, hogy ha adja meg mindkét adatok események **SZERIALIZÁLÁSA** 2 modell használatával.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-243">However it may not be clear why it works that way when you pass both data events to **SERIALIZE** using model 2.</span></span>

<span data-ttu-id="c6e1f-244">Ez a viselkedés könnyebben érthetőek legyenek, ha tudja, hogy a feltételek, amelyek a **szerializáló** könyvtár okozza.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-244">This behavior is easier to understand if you know the assumptions that the **serializer** library is making.</span></span> <span data-ttu-id="c6e1f-245">Az ezen lépjen vissza a modell célszerű:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-245">To make sense of this let’s go back to our model:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="c6e1f-246">Ez a modell gondol objektumorientált feltételeit.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-246">Think of this model in object-oriented terms.</span></span> <span data-ttu-id="c6e1f-247">Ebben az esetben azt még modellezési egy fizikai eszköz (termosztát), és eszközökön is tulajdonságai, például a **hőmérséklet** és **páratartalom**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-247">In this case we’re modeling a physical device (a thermostat) and that device includes attributes like **Temperature** and **Humidity**.</span></span>

<span data-ttu-id="c6e1f-248">A modell a következő kóddal teljes állapotának kérhessen:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-248">We can send the entire state of our model with code such as the following:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="c6e1f-249">Feltéve, hogy az értékek hőmérséklet, páratartalom és az idő beállítása, azt például ez az IoT-központ küldött egy esemény jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-249">Assuming the values of Temperature, Humidity and Time are set, we would see an event like this sent to IoT Hub:</span></span>

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="c6e1f-250">Egyes esetekben előfordulhat, hogy csak szeretne küldeni *néhány* (Ez a különösen igaz, ha a modell tartalmaz számos adatok események) a felhőbe a modell tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-250">Sometimes you may only want to send *some* properties of the model to the cloud (this is especially true if your model contains a large number of data events).</span></span> <span data-ttu-id="c6e1f-251">Akkor célszerű küldendő adatok események csak egy részét, mint a korábbi példában:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-251">It’s useful to send only a subset of data events, such as in our earlier example:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="c6e1f-252">Ezt követően pontosan azonos szerializált esemény, mintha meghatározott kellett egy **TemperatureEvent** rendelkező egy **hőmérséklet** és **idő** tag, hasonló módon az azt volt a modell 1.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-252">This generates exactly the same serialized event as if we had defined a **TemperatureEvent** with a **Temperature** and **Time** member, just as we did with model 1.</span></span> <span data-ttu-id="c6e1f-253">Ebben az esetben volt pontosan azonos szerializált esemény létrehozása egy másik modell (modell 2) használatával, mert hívtuk **SZERIALIZÁLÁSA** eltérő módon.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-253">In this case we were able to generate exactly the same serialized event by using a different model (model 2) because we called **SERIALIZE** in a different way.</span></span>

<span data-ttu-id="c6e1f-254">A legfontosabb, hogy ha több adat események át **SZERIALIZÁLÁSA,** és az azt feltételezi, hogy minden esemény egy adott JSON-objektum tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-254">The important point is that if you pass multiple data events to **SERIALIZE,** then it assumes each event is a property in a single JSON object.</span></span>

<span data-ttu-id="c6e1f-255">A legjobb módszer függ, és hogyan úgy gondolja, hogy a modell kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-255">The best approach depends on you and how you think about your model.</span></span> <span data-ttu-id="c6e1f-256">Ha az "események" küldjük a felhőhöz, és minden esemény egy adott csoportján tulajdonságait tartalmazza, az első módszer logika számos tesz.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-256">If you’re sending "events" to the cloud and each event contains a defined set of properties, then the first approach makes a lot of sense.</span></span> <span data-ttu-id="c6e1f-257">Ebben az esetben használhatja **DECLARE\_STRUCT** számára adja meg minden esemény szerkezetét, és azokat a modellben az a **WITH\_adatok** makró.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-257">In that case you would use **DECLARE\_STRUCT** to define the structure of each event and then include them in your model with the **WITH\_DATA** macro.</span></span> <span data-ttu-id="c6e1f-258">Ezután küldenie minden esemény hasonlóan a fenti példában szereplő.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-258">Then you send each event as we did in the first example above.</span></span> <span data-ttu-id="c6e1f-259">Ez a módszer az lenne csak át egy egyetlen esemény **SZERIALIZÁLÓ**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-259">In this approach you would only pass a single data event to **SERIALIZER**.</span></span>

<span data-ttu-id="c6e1f-260">Ha úgy gondolja, hogy a modell csak objektumorientált módon kapcsolatos, a második megközelítés lehet, hogy megfelelően meg.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-260">If you think about your model in an object-oriented fashion, then the second approach may suit you.</span></span> <span data-ttu-id="c6e1f-261">Ebben az esetben az elemek megadott **WITH\_adatok** a "Tulajdonságok" az objektum.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-261">In this case, the elements defined using **WITH\_DATA** are the "properties" of your object.</span></span> <span data-ttu-id="c6e1f-262">Bármilyen események részhalmazát át **SZERIALIZÁLÁSA** kínálatából, attól függően, hogy mennyi a "objektum" állapota szeretne küldeni a felhőbe.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-262">You pass whatever subset of events to **SERIALIZE** that you like, depending on how much of your "object’s" state you want to send to the cloud.</span></span>

<span data-ttu-id="c6e1f-263">Nether megoldás, a megfelelő vagy nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-263">Nether approach is right or wrong.</span></span> <span data-ttu-id="c6e1f-264">Csak vegye figyelembe, hogy a **szerializáló** könyvtár működik, és mentse a modellezési megközelítés, amely a leginkább megfelel az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-264">Just be aware of how the **serializer** library works, and pick the modeling approach that best fits your needs.</span></span>

## <a name="message-handling"></a><span data-ttu-id="c6e1f-265">Üzenet kezelése</span><span class="sxs-lookup"><span data-stu-id="c6e1f-265">Message handling</span></span>
<span data-ttu-id="c6e1f-266">Ez a cikk, amennyiben rendelkezik csak az IoT hubhoz küldő események tárgyalt, és még nem címzett üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-266">So far this article has only discussed sending events to IoT Hub, and hasn't addressed receiving messages.</span></span> <span data-ttu-id="c6e1f-267">Az oka az okozza, hogy mit kell tudnia üzenetek fogadására van nagy mértékben volt szó egy [korábbi cikk](iot-hub-device-sdk-c-intro.md).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-267">The reason for this is that what we need to know about receiving messages has largely been covered in an [earlier article](iot-hub-device-sdk-c-intro.md).</span></span> <span data-ttu-id="c6e1f-268">A cikkben visszahívás, hogy Ön által feldolgozott üzenetek üzenet visszahívási függvény regisztrálásával:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-268">Recall from that article that you process messages by registering a message callback function:</span></span>

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

<span data-ttu-id="c6e1f-269">Ezután ír a visszahívási függvény, amelyet egy üzenet fogadásakor:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-269">You then write the callback function that’s invoked when a message is received:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

<span data-ttu-id="c6e1f-270">Ez a megvalósítás a **IoTHubMessage** meghívja az adott függvény a modellben minden egyes művelethez.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-270">This implementation of **IoTHubMessage** calls the specific function for each action in your model.</span></span> <span data-ttu-id="c6e1f-271">Ha például a modell határozza meg a műveletet:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-271">For example, if your model defines this action:</span></span>

```
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="c6e1f-272">Meg kell adnia a függvény a következő aláírást:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-272">You must define a function with this signature:</span></span>

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="c6e1f-273">**SetAirResistance** majd nevezzük, amikor adott üzenetet küld az eszközre.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-273">**SetAirResistance** is then called when that message is sent to your device.</span></span>

<span data-ttu-id="c6e1f-274">Mi azt még nem magyaráznak még az üzenet a szerializált verzió néz.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-274">What we haven't explained yet is what the serialized version of message looks like.</span></span> <span data-ttu-id="c6e1f-275">Ez azt jelenti Ha elküldi egy **SetAirResistance** üzenet az eszközön, what does adott néz?</span><span class="sxs-lookup"><span data-stu-id="c6e1f-275">In other words, if you want to send a **SetAirResistance** message to your device, what does that look like?</span></span>

<span data-ttu-id="c6e1f-276">Ha egy üzenetet küldünk egy eszközre, tennénk az Azure IoT SDK szolgáltatáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-276">If you're sending a message to a device, you would do so through the Azure IoT service SDK.</span></span> <span data-ttu-id="c6e1f-277">Továbbra is szeretné tudja, milyen karakterlánc küldeni egy adott művelet hívása.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-277">You still need to know what string to send to invoke a particular action.</span></span> <span data-ttu-id="c6e1f-278">Üzenetküldés az általános formátum a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-278">The general format for sending a message appears as follows:</span></span>

```
{"Name" : "", "Parameters" : "" }
```

<span data-ttu-id="c6e1f-279">Küldünk egy szerializált JSON-objektum két tulajdonságokkal: **neve** neve, a művelet (üzenet) és **paraméterek** intézkedés paramétereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-279">You're sending a serialized JSON object with two properties: **Name** is the name of the action (message) and **Parameters** contains the parameters of that action.</span></span>

<span data-ttu-id="c6e1f-280">Ahhoz például, hogy meghívása **SetAirResistance** ezt az üzenetet küld egy eszköz:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-280">For example, to invoke **SetAirResistance** you can send this message to a device:</span></span>

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

<span data-ttu-id="c6e1f-281">A művelet nevének pontosan meg kell egyeznie a modellben definiált művelet.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-281">The action name must exactly match an action defined in your model.</span></span> <span data-ttu-id="c6e1f-282">A paraméter nevét is meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-282">The parameter names must match as well.</span></span> <span data-ttu-id="c6e1f-283">Azt is vegye figyelembe a kis-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-283">Also note case sensitivity.</span></span> <span data-ttu-id="c6e1f-284">**Név** és **paraméterek** a rendszer mindig a nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-284">**Name** and **Parameters** are always uppercase.</span></span> <span data-ttu-id="c6e1f-285">Győződjön meg arról, hogy a művelet nevének és a paraméterek a modell nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-285">Make sure to match the case of your action name and parameters in your model.</span></span> <span data-ttu-id="c6e1f-286">Ebben a példában a művelet neve: "SetAirResistance", és nem "setairresistance".</span><span class="sxs-lookup"><span data-stu-id="c6e1f-286">In this example, the action name is "SetAirResistance" and not "setairresistance".</span></span>

<span data-ttu-id="c6e1f-287">A két más műveletek **TurnFanOn** és **TurnFanOff** el ezeket az üzeneteket küld egy eszköz:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-287">The two other actions **TurnFanOn** and **TurnFanOff** can be invoked by sending these messages to a device:</span></span>

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

<span data-ttu-id="c6e1f-288">Ebben a szakaszban leírt mindent, amire szüksége tudni, hogy mikor küldő események és fogadását az üzeneteket a **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-288">This section described everything you need to know when sending events and receiving messages with the **serializer** library.</span></span> <span data-ttu-id="c6e1f-289">Mielőtt továbblép, most is beállíthat néhány paraméter, a modell mekkora szabályozó foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-289">Before moving on, let's cover some parameters you can configure that control how large your model is.</span></span>

## <a name="macro-configuration"></a><span data-ttu-id="c6e1f-290">A makróban konfiguráció</span><span class="sxs-lookup"><span data-stu-id="c6e1f-290">Macro configuration</span></span>
<span data-ttu-id="c6e1f-291">Használata esetén a **szerializáló** könyvtár az SDK tisztában lenni a fontos része megtalálható az azure-c-megosztott-segédprogram kódtára.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-291">If you’re using the **Serializer** library an important part of the SDK to be aware of is found in the azure-c-shared-utility library.</span></span>
<span data-ttu-id="c6e1f-292">Ha az Azure iot-sdk--c tárház a Githubról--rekurzív funkcióval rendelkezik klónozták, majd megtalálja a megosztott segédprogram kódtára itt:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-292">If you have cloned the Azure-iot-sdk-c repository from GitHub using the --recursive option, then you will find this shared utility library here:</span></span>

```
.\\c-utility
```

<span data-ttu-id="c6e1f-293">Ha a szalagtár nem rendelkezik klónozott, megtalálhatja [Itt](https://github.com/Azure/azure-c-shared-utility).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-293">If you have not cloned the library, you can find it [here](https://github.com/Azure/azure-c-shared-utility).</span></span>

<span data-ttu-id="c6e1f-294">A megosztott segédeszközök könyvtárból talál a következő mappát:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-294">Within the shared utility library, you will find the following folder:</span></span>

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

<span data-ttu-id="c6e1f-295">Ez a mappa tartalmaz, amely a Visual Studio megoldást **makró\_utils\_h\_generator.sln**:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-295">This folder contains a Visual Studio solution called **macro\_utils\_h\_generator.sln**:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

<span data-ttu-id="c6e1f-296">Ebben a megoldásban a program létrehozza a **makró\_utils.h** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-296">The program in this solution generates the **macro\_utils.h** file.</span></span> <span data-ttu-id="c6e1f-297">Nincs alapértelmezett makró\_utils.h fájl tartalmazza az SDK-val.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-297">There’s a default macro\_utils.h file included with the SDK.</span></span> <span data-ttu-id="c6e1f-298">Ez a megoldás lehetővé teszi egyes paraméterek módosítása, és hozza létre a következő paraméterek alapján fejlécfájlt.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-298">This solution allows you to modify some parameters and then recreate the header file based on these parameters.</span></span>

<span data-ttu-id="c6e1f-299">A két fő paramétereit, és az érintett **nArithmetic** és **nMacroParameters** amely makró található két sort definiált\_utils.tt:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-299">The two key parameters to be concerned with are **nArithmetic** and **nMacroParameters** which are defined in these two lines found in macro\_utils.tt:</span></span>

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

<span data-ttu-id="c6e1f-300">Ezeket az értékeket az SDK részét képező alapértelmezett paraméterek.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-300">These values are the default parameters included with the SDK.</span></span> <span data-ttu-id="c6e1f-301">Minden paraméternek a következő jelentése:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-301">Each parameter has the following meaning:</span></span>

* <span data-ttu-id="c6e1f-302">nMacroParameters – meghatározza, hány paraméterek egy DECLARE lehet\_MODELL makró definíciója.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-302">nMacroParameters – Controls how many parameters you can have in one DECLARE\_MODEL macro definition.</span></span>
* <span data-ttu-id="c6e1f-303">nArithmetic – meghatározza a modell engedélyezett tagok száma.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-303">nArithmetic – Controls the total number of members allowed in a model.</span></span>

<span data-ttu-id="c6e1f-304">Ezek a paraméterek számára fontos oka, mivel azok vezérelni hogyan nagy is lehet a modellben.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-304">The reason these parameters are important is because they control how large your model can be.</span></span> <span data-ttu-id="c6e1f-305">Vegyük példaként a modell definíciója:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-305">For example, consider this model definition:</span></span>

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

<span data-ttu-id="c6e1f-306">Ahogy korábban említettük **DECLARE\_MODELL** csak egy C makró.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-306">As mentioned previously, **DECLARE\_MODEL** is just a C macro.</span></span> <span data-ttu-id="c6e1f-307">A modell neve és a **WITH\_adatok** utasítás (még egy másik makró) paraméterei **DECLARE\_MODELL**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-307">The names of the model and the **WITH\_DATA** statement (yet another macro) are parameters of **DECLARE\_MODEL**.</span></span> <span data-ttu-id="c6e1f-308">**nMacroParameters** határozza meg, hány paraméterek tartalmazhat **DECLARE\_MODELL**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-308">**nMacroParameters** defines how many parameters can be included in **DECLARE\_MODEL**.</span></span> <span data-ttu-id="c6e1f-309">Hatékonyan hogy hány adatok esemény és művelet nyilatkozatok, akkor is határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-309">Effectively, this defines how many data event and action declarations you can have.</span></span> <span data-ttu-id="c6e1f-310">Mint ilyen az alapértelmezett határérték 124 Ez azt jelenti, hogy definiálhat egy modell készül 60 műveletek és adatok események kombinációjával.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-310">As such, with the default limit of 124 this means that you can define a model with a combination of about 60 actions and data events.</span></span> <span data-ttu-id="c6e1f-311">Ha túllépi ezt a határt, kapni fog ilyen fordítóprogram-hibákkal:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-311">If you try to exceed this limit, you'll receive compiler errors that look similar to this:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

<span data-ttu-id="c6e1f-312">A **nArithmetic** paraméter többet az az alkalmazás a makró nyelvi belső működését.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-312">The **nArithmetic** parameter is more about the internal workings of the macro language than your application.</span></span>  <span data-ttu-id="c6e1f-313">Azt szabályozza, hogy a modell lehet tagok száma beleértve **DECLARE_STRUCT** makrókat.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-313">It controls the total number of members you can have in your model, including **DECLARE_STRUCT** macros.</span></span> <span data-ttu-id="c6e1f-314">Ha például a fordítóprogram-hibákkal jelenítse, akkor kell próbálja növelni **nArithmetic**:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-314">If you start seeing compiler errors such as this, then you should try increasing **nArithmetic**:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

<span data-ttu-id="c6e1f-315">Ha szeretné módosítani ezeket a paramétereket, módosítsa a makróban\_utils.tt fájlt, a makró újrafordítása\_utils\_h\_generator.sln megoldás, és futtassa a lefordított program.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-315">If you want to change these parameters, modify the values in the macro\_utils.tt file, recompile the macro\_utils\_h\_generator.sln solution, and run the compiled program.</span></span> <span data-ttu-id="c6e1f-316">Ha így tesz, így új makró\_utils.h fájl jön létre, és elhelyezni a.\\ közös\\inc könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-316">When you do so, a new macro\_utils.h file is generated and placed in the .\\common\\inc directory.</span></span>

<span data-ttu-id="c6e1f-317">Ahhoz, hogy az új verzió makró\_utils.h, távolítsa el a **szerializáló** NuGet-csomagot a megoldás, és helyette közé tartozik a **szerializáló** Visual Studio-projektet.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-317">In order to use the new version of macro\_utils.h, remove the **serializer** NuGet package from your solution and in its place include the **serializer** Visual Studio project.</span></span> <span data-ttu-id="c6e1f-318">Ez lehetővé teszi, hogy a kód fordítható legyen a forráskód a szerializáló szalagtár ellen.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-318">This enables your code to compile against the source code of the serializer library.</span></span> <span data-ttu-id="c6e1f-319">Ez magában foglalja a frissített makró\_utils.h.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-319">This includes the updated macro\_utils.h.</span></span> <span data-ttu-id="c6e1f-320">Ha azt szeretné, hogy erre a **simplesample\_amqp**, indítsa el a megoldás a szerializáló Library NuGet-csomag eltávolítása:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-320">If you want to do this for **simplesample\_amqp**, start by removing the NuGet package for the serializer library from the solution:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

<span data-ttu-id="c6e1f-321">Majd adja hozzá a projektet a Visual Studio megoldás:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-321">Then add this project to your Visual Studio solution:</span></span>

> <span data-ttu-id="c6e1f-322">. \\c\\szerializáló\\build\\windows\\serializer.vcxproj</span><span class="sxs-lookup"><span data-stu-id="c6e1f-322">.\\c\\serializer\\build\\windows\\serializer.vcxproj</span></span>
> 
> 

<span data-ttu-id="c6e1f-323">Amikor elkészült, a megoldás így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-323">When you're done, your solution should look like this:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

<span data-ttu-id="c6e1f-324">Ha most a a megoldás, a frissített makró fordítási\_utils.h a bináris tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-324">Now when you compile your solution, the updated macro\_utils.h is included in your binary.</span></span>

<span data-ttu-id="c6e1f-325">Vegye figyelembe, hogy ezeket az értékeket elég magas növelése lépheti túl a fordítóprogram korlátok.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-325">Note that increasing these values high enough can exceed compiler limits.</span></span> <span data-ttu-id="c6e1f-326">Erre a pontra a **nMacroParameters** a fő paraméter, amellyel érintett.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-326">To this point, the **nMacroParameters** is the main parameter with which to be concerned.</span></span> <span data-ttu-id="c6e1f-327">A C99 spec határozza meg, hogy 127 paraméterek közül legalább egy makró definition engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-327">The C99 spec specifies that a minimum of 127 parameters are allowed in a macro definition.</span></span> <span data-ttu-id="c6e1f-328">A Microsoft fordítóprogram a spec pontosan követi (és maximális hossza 127), ezért nem lehet növelheti a **nMacroParameters** meghaladja az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-328">The Microsoft compiler follows the spec exactly (and has a limit of 127), so you won't be able to increase **nMacroParameters** beyond the default.</span></span> <span data-ttu-id="c6e1f-329">Más compilers – előfordulhat, hogy lehetővé teszi (például a GNU fordító magasabb határérték támogatja).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-329">Other compilers might allow you to do so (for example, the GNU compiler supports a higher limit).</span></span>

<span data-ttu-id="c6e1f-330">Amennyiben azt már szabályozott tudni, hogyan írhat kódot a szükséges összes a **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-330">So far we've covered just about everything you need to know about how to write code with the **serializer** library.</span></span> <span data-ttu-id="c6e1f-331">Mielőtt áthaladva addig tart, most le újra az előző cikket, esetleg kíváncsi kapcsolatos témaköröket.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-331">Before concluding, let's revisit some topics from previous articles that you may be wondering about.</span></span>

## <a name="the-lower-level-apis"></a><span data-ttu-id="c6e1f-332">Az alacsonyabb szintű API-k</span><span class="sxs-lookup"><span data-stu-id="c6e1f-332">The lower-level APIs</span></span>
<span data-ttu-id="c6e1f-333">Ez a cikk arra irányul mintaalkalmazás **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-333">The sample application on which this article focused is **simplesample\_amqp**.</span></span> <span data-ttu-id="c6e1f-334">Ez a minta használja a magasabb szintű (a nem "r") API-k események küldhet és fogadhat üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-334">This sample uses the higher-level (the non-"LL") APIs to send events and receive messages.</span></span> <span data-ttu-id="c6e1f-335">Ezen API-k használatakor a háttérszálon fut, amely gondoskodik az események üzenetek küldése és fogadása.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-335">If you use these APIs, a background thread runs which takes care of both sending events and receiving messages.</span></span> <span data-ttu-id="c6e1f-336">Az alacsonyabb szintű (r) API-k használatával azonban a háttérszálon megszüntetéséhez, és tegye meg explicit szabályozhatják, amikor küldi az eseményeket, vagy a felhőbe érkező üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-336">However, you can use the lower-level (LL) APIs to eliminate this background thread and take explicit control over when you send events or receive messages from the cloud.</span></span>

<span data-ttu-id="c6e1f-337">Lásd: a [előző cikkben](iot-hub-device-sdk-c-iothubclient.md), nincs funkciók olyan készlete, amely a magasabb szintű API-k áll:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-337">As described in a [previous article](iot-hub-device-sdk-c-iothubclient.md), there is a set of functions that consists of the higher-level APIs:</span></span>

* <span data-ttu-id="c6e1f-338">IoTHubClient\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="c6e1f-338">IoTHubClient\_CreateFromConnectionString</span></span>
* <span data-ttu-id="c6e1f-339">IoTHubClient\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="c6e1f-339">IoTHubClient\_SendEventAsync</span></span>
* <span data-ttu-id="c6e1f-340">IoTHubClient\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="c6e1f-340">IoTHubClient\_SetMessageCallback</span></span>
* <span data-ttu-id="c6e1f-341">IoTHubClient\_megszüntetése</span><span class="sxs-lookup"><span data-stu-id="c6e1f-341">IoTHubClient\_Destroy</span></span>

<span data-ttu-id="c6e1f-342">Az ezen API-k egy **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-342">These APIs are demonstrated in **simplesample\_amqp**.</span></span>

<span data-ttu-id="c6e1f-343">Szerepel továbbá egy hasonló alacsonyabb szintű API-készlet.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-343">There is also an analogous set of lower-level APIs.</span></span>

* <span data-ttu-id="c6e1f-344">IoTHubClient\_inden\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="c6e1f-344">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="c6e1f-345">IoTHubClient\_inden\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="c6e1f-345">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="c6e1f-346">IoTHubClient\_inden\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="c6e1f-346">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="c6e1f-347">IoTHubClient\_inden\_megszüntetése</span><span class="sxs-lookup"><span data-stu-id="c6e1f-347">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="c6e1f-348">Vegye figyelembe, hogy az alacsonyabb szintű API-k pontosan ugyanúgy működnek, a fenti cikkekben leírt módon.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-348">Note that the lower-level APIs work exactly the same way as described in the previous articles.</span></span> <span data-ttu-id="c6e1f-349">Az első API-készlet is használhatja, ha azt szeretné, hogy a háttérszálon eseményeket küldő és fogadó üzenetek kezelésére.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-349">You can use the first set of APIs if you want a background thread to handle sending events and receiving messages.</span></span> <span data-ttu-id="c6e1f-350">Ha azt szeretné, hogy explicit szabályozhatják, amikor Ön adatokat küldeni és fogadni az IoT-központ használhatja a második API-készlet.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-350">You use the second set of APIs if you want explicit control over when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="c6e1f-351">Vagy API-k munkahelyi készlete egyaránt valamint a **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-351">Either set of APIs work equally well with the **serializer** library.</span></span>

<span data-ttu-id="c6e1f-352">Az alacsonyabb szintű API-k használata például a **szerializáló** könyvtár, tekintse meg a **simplesample\_http** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-352">For an example of how the lower-level APIs are used with the **serializer** library, see the **simplesample\_http** application.</span></span>

## <a name="additional-topics"></a><span data-ttu-id="c6e1f-353">További témakörök</span><span class="sxs-lookup"><span data-stu-id="c6e1f-353">Additional topics</span></span>
<span data-ttu-id="c6e1f-354">Érdemes megemlíteni néhány más témakörök újra szolgálnak tulajdonság kezelési, alternatív hitelesítő adatai és konfigurációs lehetőség használatával.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-354">A few other topics worth mentioning again are property handling, using alternate device credentials, and configuration options.</span></span> <span data-ttu-id="c6e1f-355">Ezek a témakörök ismertetett egy [előző cikkben](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="c6e1f-355">These are all topics covered in a [previous article](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="c6e1f-356">A fő pont, hogy ezek a funkciók működik együtt a a **szerializáló** könyvtár, mint az a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-356">The main point is that all of these features work in the same way with the **serializer** library as they do with the **IoTHubClient** library.</span></span> <span data-ttu-id="c6e1f-357">Például, ha szeretné tulajdonságok csatolása eseményhez a modellből, használhat **IoTHubMessage\_tulajdonságok** és **térkép**\_**AddorUpdate**, ugyanúgy, mint korábban leírt:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-357">For example, if you want to attach properties to an event from your model, you use **IoTHubMessage\_Properties** and **Map**\_**AddorUpdate**, the same way as described previously:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="c6e1f-358">Hogy az esemény származik a **szerializáló** könyvtár segítségével manuálisan létrehozott vagy a **IoTHubClient** könyvtár nem számít.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-358">Whether the event was generated from the **serializer** library or created manually using the **IoTHubClient** library does not matter.</span></span>

<span data-ttu-id="c6e1f-359">A hitelesítő adatok másik eszköz használatával **IoTHubClient\_inden\_létrehozása** is működik, **IoTHubClient\_CreateFromConnectionString** a rendeljen egy **IOT HUBBAL\_ügyfél\_KEZELNI**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-359">For the alternate device credentials, using **IoTHubClient\_LL\_Create** works just as well as **IoTHubClient\_CreateFromConnectionString** for allocating an **IOTHUB\_CLIENT\_HANDLE**.</span></span>

<span data-ttu-id="c6e1f-360">Végezetül használata a **szerializáló** könyvtár, állíthatja be a konfigurációs beállítások **IoTHubClient\_inden\_SetOption** csak úgy, ahogy a használatakor**IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-360">Finally, if you're using the **serializer** library, you can set configuration options with **IoTHubClient\_LL\_SetOption** just as you did when using the **IoTHubClient** library.</span></span>

<span data-ttu-id="c6e1f-361">Egy szolgáltatás, amely egyedi a **szerializáló** könyvtárban az inicializálás API-k.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-361">A feature that is unique to the **serializer** library are the initialization APIs.</span></span> <span data-ttu-id="c6e1f-362">A könyvtár használata előtt meg kell hívnia **szerializáló\_init**:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-362">Before you can start working with the library, you must call **serializer\_init**:</span></span>

```
serializer_init(NULL);
```

<span data-ttu-id="c6e1f-363">Ez történik, mielőtt meghívja a **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-363">This is done just before you call **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="c6e1f-364">Hasonlóképpen, ha elkészült dolgozik a könyvtárban, az utolsó hívás, meg kell győződnie, hogy **szerializáló\_deinit**:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-364">Similarly, when you're done working with the library, the last call you’ll make is to **serializer\_deinit**:</span></span>

```
serializer_deinit();
```

<span data-ttu-id="c6e1f-365">Ellenkező esetben a többi a fent felsorolt funkciók azonos működik a **szerializáló** könyvtár mint a **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-365">Otherwise, all of the other features listed above work the same in the **serializer** library as they do in the **IoTHubClient** library.</span></span> <span data-ttu-id="c6e1f-366">Az e témakörökkel kapcsolatos további információkért tekintse meg a [előző cikkben](iot-hub-device-sdk-c-iothubclient.md) a sorozat.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-366">For more information about any of these topics, see the [previous article](iot-hub-device-sdk-c-iothubclient.md) in this series.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6e1f-367">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c6e1f-367">Next steps</span></span>
<span data-ttu-id="c6e1f-368">Ez a cikk ismerteti részletesen a egyedi szempontjait a **szerializáló** függvénytár szerepel a **C-hez készült SDK Azure IoT-eszközök**. Az információk adni érdeme alaposan megismernie az események üzeneteket küldjön és fogadjon az IoT-központ modellek használata.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-368">This article describes in detail the unique aspects of the **serializer** library contained in the **Azure IoT device SDK for C**. With the information provided you should have a good understanding of how to use models to send events and receive messages from IoT Hub.</span></span>

<span data-ttu-id="c6e1f-369">Ezzel befejezte a három részből sorozat hogyan az alkalmazások fejlesztéséhez a **C-hez készült SDK Azure IoT-eszközök**. Ez legyen elegendő információt nem csak első lépésekhez, de Önnek az API-k működése alapos ismerete.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-369">This also concludes the three-part series on how to develop applications with the **Azure IoT device SDK for C**. This should be enough information to not only get you started but give you a thorough understanding of how the APIs work.</span></span> <span data-ttu-id="c6e1f-370">További információkért nincsenek néhány mintákat az SDK nem vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-370">For additional information, there are a few samples in the SDK not covered here.</span></span> <span data-ttu-id="c6e1f-371">Ellenkező esetben a [SDK-dokumentáció](https://github.com/Azure/azure-iot-sdk-c) helyes erőforrás további információt.</span><span class="sxs-lookup"><span data-stu-id="c6e1f-371">Otherwise, the [SDK documentation](https://github.com/Azure/azure-iot-sdk-c) is a good resource for additional information.</span></span>

<span data-ttu-id="c6e1f-372">Az IoT-központ fejlesztésével kapcsolatos további tudnivalókért tekintse meg a [Azure IoT SDK-k][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="c6e1f-372">To learn more about developing for IoT Hub, see the [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="c6e1f-373">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="c6e1f-373">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="c6e1f-374">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="c6e1f-374">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
