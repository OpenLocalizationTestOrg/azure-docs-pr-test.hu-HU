---
title: "aaaAzure IoT-eszközök SDK c - szerializáló |} Microsoft Docs"
description: "Hogyan toouse hello szerializáló szalagtár hello Azure IoT-eszközök SDK C toocreate eszköz alkalmazások, amelyek kommunikálni az IoT-központ."
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
ms.openlocfilehash: c5776e9b50ffea71df96cb2d342ea2fc045f5a0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a><span data-ttu-id="8cfb2-103">Az Azure IoT-eszközök SDK c – további információk a szerializáló</span><span class="sxs-lookup"><span data-stu-id="8cfb2-103">Azure IoT device SDK for C – more about serializer</span></span>
<span data-ttu-id="8cfb2-104">Hello [először a következő cikket:](iot-hub-device-sdk-c-intro.md) az a sorozat bevezetett hello **C-hez készült SDK Azure IoT-eszközök**. hello a következő cikk megadott hello részletesebb leírását [ **IoTHubClient** ](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-104">hello [first article](iot-hub-device-sdk-c-intro.md) in this series introduced hello **Azure IoT device SDK for C**. hello next article provided a more detailed description of hello [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="8cfb2-105">Ez a cikk befejezése hello SDK körét, adja meg a fennmaradó összetevő hello részletesebb leírását: hello **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-105">This article completes coverage of hello SDK by providing a more detailed description of hello remaining component: hello **serializer** library.</span></span>

<span data-ttu-id="8cfb2-106">hello bevezető cikkben leírt hogyan toouse hello **szerializáló** könyvtár toosend események tooand üzenetek fogadása az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-106">hello introductory article described how toouse hello **serializer** library toosend events tooand receive messages from IoT Hub.</span></span> <span data-ttu-id="8cfb2-107">Ebben a cikkben tovább bővítjük ezekre a kérdésekre adott válaszokat azáltal, hogy részletesebb magyarázatát, hogy hogyan toomodel az adatok hello **szerializáló** makró nyelv.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-107">In this article, we extend that discussion by providing a more complete explanation of how toomodel your data with hello **serializer** macro language.</span></span> <span data-ttu-id="8cfb2-108">hello cikket is tartalmaz, hogyan hello könyvtár rendezi sorba üzenetek további információkra (és egyes esetekben szabályozásának hello szerializálási viselkedés).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-108">hello article also includes more detail about how hello library serializes messages (and in some cases how you can control hello serialization behavior).</span></span> <span data-ttu-id="8cfb2-109">Egyes paraméterek módosíthatja hello modellek hello mérete meghatározó is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-109">We'll also describe some parameters you can modify that determine hello size of hello models you create.</span></span>

<span data-ttu-id="8cfb2-110">Végezetül hello cikk revisits néhány témaköre például üzenet és a tulajdonság kezelési előző cikkek tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-110">Finally, hello article revisits some topics covered in previous articles such as message and property handling.</span></span> <span data-ttu-id="8cfb2-111">Módon fejeződött be, ezek a szolgáltatások munkahelyi megkeressük hello azonos módon használatával hello **szerializáló** könyvtár, mint a hello **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-111">As we'll find out, those features work in hello same way using hello **serializer** library as they do with hello **IoTHubClient** library.</span></span>

<span data-ttu-id="8cfb2-112">Minden cikkben leírt hello alapul **szerializáló** SDK minták.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-112">Everything described in this article is based on hello **serializer** SDK samples.</span></span> <span data-ttu-id="8cfb2-113">Ha azt szeretné, hogy mentén toofollow, tekintse meg a hello **simplesample\_amqp** és **simplesample\_http** alkalmazások hello Azure IoT-eszközök SDK szerepel a c kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-113">If you want toofollow along, see hello **simplesample\_amqp** and **simplesample\_http** applications included in hello Azure IoT device SDK for C.</span></span>

<span data-ttu-id="8cfb2-114">Hello található [ **C-hez készült SDK Azure IoT-eszközök** ](https://github.com/Azure/azure-iot-sdk-c) hello API hello a GitHub tárház és nézet adatai [C API-referencia](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-114">You can find hello [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of hello API in hello [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="hello-modeling-language"></a><span data-ttu-id="8cfb2-115">hello modellezési nyelv</span><span class="sxs-lookup"><span data-stu-id="8cfb2-115">hello modeling language</span></span>
<span data-ttu-id="8cfb2-116">Hello [bevezető cikkben](iot-hub-device-sdk-c-intro.md) az a sorozat bevezetett hello **C-hez készült SDK Azure IoT-eszközök** hello példa hello megadott nyelv modellezési **simplesample\_amqp**  alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-116">hello [introductory article](iot-hub-device-sdk-c-intro.md) in this series introduced hello **Azure IoT device SDK for C** modeling language through hello example provided in hello **simplesample\_amqp** application:</span></span>

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

<span data-ttu-id="8cfb2-117">Ahogy látja, nyelvi modellezési hello C makrók alapul.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-117">As you can see, hello modeling language is based on C macros.</span></span> <span data-ttu-id="8cfb2-118">Mindig megkezdi a definíció **BEGIN\_NÉVTÉR** és mindig végződhet **END\_NÉVTÉR**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-118">You always begin your definition with **BEGIN\_NAMESPACE** and always end with **END\_NAMESPACE**.</span></span> <span data-ttu-id="8cfb2-119">Közös tooname hello névtér a vállalat vagy a példában, amelyet használ a hello projekt legyen.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-119">It's common tooname hello namespace for your company or, as in this example, hello project that you're working on.</span></span>

<span data-ttu-id="8cfb2-120">Mi kerül hello névtéren belül modell definíciókat.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-120">What goes inside hello namespace are model definitions.</span></span> <span data-ttu-id="8cfb2-121">Ebben az esetben van egy anemometer egyetlen modellt.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-121">In this case, there is a single model for an anemometer.</span></span> <span data-ttu-id="8cfb2-122">Ebben az esetben hello modell elnevezheti semmit, de általában ez nevű hello eszköz vagy az adattípus az IoT hubbal kívánt tooexchange.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-122">Once again, hello model can be named anything, but typically this is named for hello device or type of data you want tooexchange with IoT Hub.</span></span>  

<span data-ttu-id="8cfb2-123">Modellek tartalmazza a következő definícióját hello események érkező tooIoT Hub is (hello *adatok*) továbbá az IoT-központ fogadhat köszönőüzenetei (hello *műveletek*).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-123">Models contain a definition of hello events you can ingress tooIoT Hub (hello *data*) as well as hello messages you can receive from IoT Hub (hello *actions*).</span></span> <span data-ttu-id="8cfb2-124">Mint hello példa látható, események rendelkezik-e egy típust és egy nevet; műveletek nevét és a választható paraméterek: (minden típusú) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-124">As you can see from hello example, events have a type and a name; actions have a name and optional parameters (each with a type).</span></span>

<span data-ttu-id="8cfb2-125">Mi nem mutatja be ezt a mintát hello SDK által támogatott további adattípusok.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-125">What’s not demonstrated in this sample are additional data types that are supported by hello SDK.</span></span> <span data-ttu-id="8cfb2-126">Oktatóanyag a következőket ismerteti, hogy a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-126">We'll cover that next.</span></span>

> [!NOTE]
> <span data-ttu-id="8cfb2-127">Az IoT-központ hivatkozik egy eszköz küld, tooit toohello adatok *események*, míg a modellezési nyelvi hello hivatkozik, tooit *adatok* (segítségével meghatározott **WITH_DATA**).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-127">IoT Hub refers toohello data a device sends tooit as *events*, while hello modeling language refers tooit as *data* (defined using **WITH_DATA**).</span></span> <span data-ttu-id="8cfb2-128">Hasonlóképpen, az IoT-központ hivatkozik, toodevices küldött toohello adatait *üzenetek*, míg a modellezési nyelvi hello hivatkozik, tooit *műveletek* (segítségével meghatározott **WITH_ACTION**).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-128">Likewise, IoT Hub refers toohello data you send toodevices as *messages*, while hello modeling language refers tooit as *actions* (defined using **WITH_ACTION**).</span></span> <span data-ttu-id="8cfb2-129">Vegye figyelembe, hogy ezek a feltételek szabadon felcserélhetők ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-129">Be aware that these terms may be used interchangeably in this article.</span></span>
> 
> 

### <a name="supported-data-types"></a><span data-ttu-id="8cfb2-130">Támogatott adattípusokat</span><span class="sxs-lookup"><span data-stu-id="8cfb2-130">Supported data types</span></span>
<span data-ttu-id="8cfb2-131">a következő adattípusokat hello tartalmazó hello modellek támogatottak **szerializáló** könyvtár:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-131">hello following data types are supported in models created with hello **serializer** library:</span></span>

| <span data-ttu-id="8cfb2-132">Típus</span><span class="sxs-lookup"><span data-stu-id="8cfb2-132">Type</span></span> | <span data-ttu-id="8cfb2-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="8cfb2-133">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8cfb2-134">Dupla</span><span class="sxs-lookup"><span data-stu-id="8cfb2-134">double</span></span> |<span data-ttu-id="8cfb2-135">dupla pontosságú lebegőpontos szám</span><span class="sxs-lookup"><span data-stu-id="8cfb2-135">double precision floating point number</span></span> |
| <span data-ttu-id="8cfb2-136">int</span><span class="sxs-lookup"><span data-stu-id="8cfb2-136">int</span></span> |<span data-ttu-id="8cfb2-137">32 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="8cfb2-137">32 bit integer</span></span> |
| <span data-ttu-id="8cfb2-138">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="8cfb2-138">float</span></span> |<span data-ttu-id="8cfb2-139">az egyszeres pontosságú lebegőpontos szám</span><span class="sxs-lookup"><span data-stu-id="8cfb2-139">single precision floating point number</span></span> |
| <span data-ttu-id="8cfb2-140">hosszú</span><span class="sxs-lookup"><span data-stu-id="8cfb2-140">long</span></span> |<span data-ttu-id="8cfb2-141">hosszú egész szám</span><span class="sxs-lookup"><span data-stu-id="8cfb2-141">long integer</span></span> |
| <span data-ttu-id="8cfb2-142">int8\_t</span><span class="sxs-lookup"><span data-stu-id="8cfb2-142">int8\_t</span></span> |<span data-ttu-id="8cfb2-143">8 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="8cfb2-143">8 bit integer</span></span> |
| <span data-ttu-id="8cfb2-144">Int16\_t</span><span class="sxs-lookup"><span data-stu-id="8cfb2-144">int16\_t</span></span> |<span data-ttu-id="8cfb2-145">16 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="8cfb2-145">16 bit integer</span></span> |
| <span data-ttu-id="8cfb2-146">Int32\_t</span><span class="sxs-lookup"><span data-stu-id="8cfb2-146">int32\_t</span></span> |<span data-ttu-id="8cfb2-147">32 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="8cfb2-147">32 bit integer</span></span> |
| <span data-ttu-id="8cfb2-148">Int64\_t</span><span class="sxs-lookup"><span data-stu-id="8cfb2-148">int64\_t</span></span> |<span data-ttu-id="8cfb2-149">64 bites egész szám</span><span class="sxs-lookup"><span data-stu-id="8cfb2-149">64 bit integer</span></span> |
| <span data-ttu-id="8cfb2-150">logikai érték</span><span class="sxs-lookup"><span data-stu-id="8cfb2-150">bool</span></span> |<span data-ttu-id="8cfb2-151">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="8cfb2-151">boolean</span></span> |
| <span data-ttu-id="8cfb2-152">ASCII\_char\_ptr</span><span class="sxs-lookup"><span data-stu-id="8cfb2-152">ascii\_char\_ptr</span></span> |<span data-ttu-id="8cfb2-153">ASCII-karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8cfb2-153">ASCII string</span></span> |
| <span data-ttu-id="8cfb2-154">EDM\_DÁTUM\_IDŐ\_ELTOLÁSA</span><span class="sxs-lookup"><span data-stu-id="8cfb2-154">EDM\_DATE\_TIME\_OFFSET</span></span> |<span data-ttu-id="8cfb2-155">dátum idő eltolása</span><span class="sxs-lookup"><span data-stu-id="8cfb2-155">date time offset</span></span> |
| <span data-ttu-id="8cfb2-156">EDM\_GUID</span><span class="sxs-lookup"><span data-stu-id="8cfb2-156">EDM\_GUID</span></span> |<span data-ttu-id="8cfb2-157">GUID</span><span class="sxs-lookup"><span data-stu-id="8cfb2-157">GUID</span></span> |
| <span data-ttu-id="8cfb2-158">EDM\_BINÁRIS</span><span class="sxs-lookup"><span data-stu-id="8cfb2-158">EDM\_BINARY</span></span> |<span data-ttu-id="8cfb2-159">Bináris</span><span class="sxs-lookup"><span data-stu-id="8cfb2-159">binary</span></span> |
| <span data-ttu-id="8cfb2-160">DEKLARÁLJA\_STRUKTÚRA</span><span class="sxs-lookup"><span data-stu-id="8cfb2-160">DECLARE\_STRUCT</span></span> |<span data-ttu-id="8cfb2-161">Összetett adattípusú</span><span class="sxs-lookup"><span data-stu-id="8cfb2-161">complex data type</span></span> |

<span data-ttu-id="8cfb2-162">Kezdjük hello utolsó adattípus.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-162">Let’s start with hello last data type.</span></span> <span data-ttu-id="8cfb2-163">Hello **DECLARE\_STRUCT** lehetővé teszi a toodefine összetett adattípusú, amelyek a hello csoportosításait. Emellett más egyszerű típusokhoz.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-163">hello **DECLARE\_STRUCT** allows you toodefine complex data types, which are groupings of hello other primitive types.</span></span> <span data-ttu-id="8cfb2-164">Ezek a Csoportosítások lehetővé teszik a számunkra toodefine modell, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-164">These groupings allow us toodefine a model that looks like this:</span></span>

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

<span data-ttu-id="8cfb2-165">A modell típusú egyetlen eseményt tartalmaz **TestType**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-165">Our model contains a single data event of type **TestType**.</span></span> <span data-ttu-id="8cfb2-166">**TestType** több tag, amelyek együttesen bemutatása hello hello által támogatott primitív típusok tartalmazó összetett típus **szerializáló** modeling language.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-166">**TestType** is a complex type that includes several members, which collectively demonstrate hello primitive types supported by hello **serializer** modeling language.</span></span>

<span data-ttu-id="8cfb2-167">Az ilyen modellek azt írhat kódot toosend adatok tooIoT Hub, amely a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-167">With a model like this, we can write code toosend data tooIoT Hub that appears as follows:</span></span>

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

<span data-ttu-id="8cfb2-168">Alapvetően, azt még hozzárendelése érték tooevery tagja hello **teszt** szerkezetét, és azt, majd hívja **sendasync metódusok párhuzamosan** toosend hello **teszt** adatok esemény toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-168">Basically, we’re assigning a value tooevery member of hello **Test** structure and then calling **SendAsync** toosend hello **Test** data event toohello cloud.</span></span> <span data-ttu-id="8cfb2-169">**Sendasync metódusok párhuzamosan** küld egy egyetlen esemény tooIoT Hub segítő függvény:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-169">**SendAsync** is a helper function that sends a single data event tooIoT Hub:</span></span>

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate hello string
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

<span data-ttu-id="8cfb2-170">Ez a függvény megadott adatok esemény hello rendezi sorba, és elküldi azt tooIoT központ használatával **IoTHubClient\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-170">This function serializes hello given data event and sends it tooIoT Hub using **IoTHubClient\_SendEventAsync**.</span></span> <span data-ttu-id="8cfb2-171">Ez az előző cikkekben ismertetett ugyanazt a kódot hello (**sendasync metódusok párhuzamosan** magában foglalja a hello logika egy kényelmes függvénynek).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-171">This is hello same code discussed in previous articles (**SendAsync** encapsulates hello logic into a convenient function).</span></span>

<span data-ttu-id="8cfb2-172">Egy korábbi kód hello használt egyéb segítő függvény **GetDateTimeOffset**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-172">One other helper function used in hello previous code is **GetDateTimeOffset**.</span></span> <span data-ttu-id="8cfb2-173">Ez a funkció átalakítja az idő típusú érték a megadott hello **EDM\_dátum\_idő\_ELTOLÁS**:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-173">This function transforms hello given time into a value of type **EDM\_DATE\_TIME\_OFFSET**:</span></span>

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

<span data-ttu-id="8cfb2-174">Ha futtatja ezt a kódot, a következő üzenet hello tooIoT Hub küld:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-174">If you run this code, hello following message is sent tooIoT Hub:</span></span>

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

<span data-ttu-id="8cfb2-175">Vegye figyelembe, hogy hello szerializálási JSON, amely hello által generált hello formátum **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-175">Note that hello serialization is JSON, which is hello format generated by hello **serializer** library.</span></span> <span data-ttu-id="8cfb2-176">Is vegye figyelembe, hogy minden egyes tagjára hello szerializált JSON-objektum megegyezik-e hello hello tagjai **TestType** , amely meghatározott a modellben.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-176">Also note that each member of hello serialized JSON object matches hello members of hello **TestType** that we defined in our model.</span></span> <span data-ttu-id="8cfb2-177">hello is pontosan megegyeznek a hello kód.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-177">hello values also exactly match those used in hello code.</span></span> <span data-ttu-id="8cfb2-178">Azonban vegye figyelembe, hogy hello bináris adatok base64-kódolású: "AQID" hello van az alkalmazás base64 kódolást {0x01, 0x02, 0x03}.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-178">However, note that hello binary data is base64-encoded: "AQID" is hello base64 encoding of {0x01, 0x02, 0x03}.</span></span>

<span data-ttu-id="8cfb2-179">Ez a példa bemutatja, hello használatának előnye hello **szerializáló** könyvtár--lehetővé velünk toosend JSON toohello felhő, anélkül, hogy az alkalmazás a szerializálási foglalkozik tooexplicitly.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-179">This example demonstrates hello advantage of using hello **serializer** library -- it enables us toosend JSON toohello cloud, without having tooexplicitly deal with serialization in our application.</span></span> <span data-ttu-id="8cfb2-180">Összes tooworry tudunk kapcsolatos van hello értékeinek beállítása a hello adatok események tekinthetők és egyszerű API-k toosend ezen események toohello felhő majd hívja.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-180">All we have tooworry about is setting hello values of hello data events in our model and then calling simple APIs toosend those events toohello cloud.</span></span>

<span data-ttu-id="8cfb2-181">Az információ azt adhatja meg a modellek, amelyek közé tartozik a támogatott adattípusok, beleértve a komplex típusok (sikerült is magában foglalja az összetett típusok más összetett típusok belül) hello tartományán.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-181">With this information, we can define models that include hello range of supported data types, including complex types (we could even include complex types within other complex types).</span></span> <span data-ttu-id="8cfb2-182">Azonban úgy szerializált létrehozott JSON hello által a fenti példában egy fontos pont megjelenik.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-182">However, he serialized JSON generated by hello example above brings up an important point.</span></span> <span data-ttu-id="8cfb2-183">*Hogyan* hello adatok küldünk **szerializáló** könyvtár meghatározza, hogy pontosan hogyan hello JSON formátuma.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-183">*How* we send data with hello **serializer** library determines exactly how hello JSON is formed.</span></span> <span data-ttu-id="8cfb2-184">Hogy az adott pont áll, mi oktatóanyag a következőket ismerteti mellett.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-184">That particular point is what we'll cover next.</span></span>

## <a name="more-about-serialization"></a><span data-ttu-id="8cfb2-185">További információ a szerializálási</span><span class="sxs-lookup"><span data-stu-id="8cfb2-185">More about serialization</span></span>
<span data-ttu-id="8cfb2-186">hello előző szakaszban kiemeli a hello által generált hello kimeneti példát **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-186">hello previous section highlights an example of hello output generated by hello **serializer** library.</span></span> <span data-ttu-id="8cfb2-187">Ebben a részben azt ismertetjük, hogyan hello könyvtár rendezi sorba adatok, és hogy miként szabályozható viselkedésmódot hello szerializálási API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-187">In this section, we'll explain how hello library serializes data and how you can control that behavior using hello serialization APIs.</span></span>

<span data-ttu-id="8cfb2-188">Rendelés tooadvance hello döntéseken szerializálási, a dolgozunk lesz az új modell egy termosztát alapján.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-188">In order tooadvance hello discussion on serialization, we'll work with a new model based on a thermostat.</span></span> <span data-ttu-id="8cfb2-189">Először hozzuk biztosít bizonyos tapasztalattal hello forgatókönyvön keresztül próbáljuk tooaddress.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-189">First, let's provide some background on hello scenario we're trying tooaddress.</span></span>

<span data-ttu-id="8cfb2-190">Azt szeretnénk, ha egy termosztát, és mérheti a hőmérséklet és a páratartalom toomodel.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-190">We want toomodel a thermostat that measures temperature and humidity.</span></span> <span data-ttu-id="8cfb2-191">Minden adat tooIoT Hub másképp küldött toobe lesz.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-191">Each piece of data is going toobe sent tooIoT Hub differently.</span></span> <span data-ttu-id="8cfb2-192">Alapértelmezés szerint hello termosztát ingresses hőmérséklet-esemény egyszer 2 percenként; a páratartalom esemény ingressed 15 percenként egyszer.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-192">By default, hello thermostat ingresses a temperature event once every 2 minutes; a humidity event is ingressed once every 15 minutes.</span></span> <span data-ttu-id="8cfb2-193">Ha bármelyik esemény ingressed, tartalmaznia kell egy Timestamp típusú, amely hello időt jeleníti meg, hogy hello megfelelő hőmérséklet vagy páratartalom mérték.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-193">When either event is ingressed, it must include a timestamp that shows hello time that hello corresponding temperature or humidity was measured.</span></span>

<span data-ttu-id="8cfb2-194">Ebben a forgatókönyvben tekintve lesz bemutatjuk, két különböző módon toomodel hello adatokat, és azt ismertetjük, hogy modellezési rendelkezik a hello szerializált kimeneti hello hatása.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-194">Given this scenario, we'll demonstrate two different ways toomodel hello data, and we'll explain hello effect that modeling has on hello serialized output.</span></span>

### <a name="model-1"></a><span data-ttu-id="8cfb2-195">1 modell</span><span class="sxs-lookup"><span data-stu-id="8cfb2-195">Model 1</span></span>
<span data-ttu-id="8cfb2-196">Hello első verzió-modell, hogy támogatja hello az előző példában a következő:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-196">Here's hello first version of a model that supports hello previous scenario:</span></span>

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

<span data-ttu-id="8cfb2-197">Vegye figyelembe, hogy hello modell az két adatok eseményeket is tartalmazza: **hőmérséklet** és **páratartalom**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-197">Note that hello model includes two data events: **Temperature** and **Humidity**.</span></span> <span data-ttu-id="8cfb2-198">Ellentétben a korábbi példákhoz hello a minden esemény típus megadott struktúra **DECLARE\_STRUCT**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-198">Unlike previous examples, hello type of each event is a structure defined using **DECLARE\_STRUCT**.</span></span> <span data-ttu-id="8cfb2-199">**TemperatureEvent** tartalmaz egy hőmérséklet mérési és egy időbélyegzőt adnak; **HumidityEvent** páratartalom a mérés és egy időbélyegzőt adnak tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-199">**TemperatureEvent** includes a temperature measurement and a timestamp; **HumidityEvent** contains a humidity measurement and a timestamp.</span></span> <span data-ttu-id="8cfb2-200">Ez a modell biztosítanak számunkra a természetes módon toomodel hello adatokat a fent leírt hello forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-200">This model gives us a natural way toomodel hello data for hello scenario described above.</span></span> <span data-ttu-id="8cfb2-201">Egy esemény toohello felhőben kapni, amikor hőmérséklet/időbélyeg vagy páratartalom/időbélyeg-párból vagy küldünk.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-201">When we send an event toohello cloud, we'll either send a temperature/timestamp or a humidity/timestamp pair.</span></span>

<span data-ttu-id="8cfb2-202">Elküldhetjük hőmérséklet esemény toohello felhő hello alábbi kód használatával:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-202">We can send a temperature event toohello cloud using code such as hello following:</span></span>

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

<span data-ttu-id="8cfb2-203">Rendszer fogja hőmérséklet és a páratartalom hello mintakód a kódolt értékeket használja, de a képzelhető el, hogy azt ténylegesen olvas be ezeket az értékeket hello megfelelő érzékelők hello termosztát a mintavétellel.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-203">We'll use hard-coded values for temperature and humidity in hello sample code, but imagine that we’re actually retrieving these values by sampling hello corresponding sensors on hello thermostat.</span></span>

<span data-ttu-id="8cfb2-204">a fenti hello kódot használja hello **GetDateTimeOffset** korábban bevezetett segítő.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-204">hello code above uses hello **GetDateTimeOffset** helper that was introduced previously.</span></span> <span data-ttu-id="8cfb2-205">Törölje későbbi lesz okokból ezt a kódot explicit módon elválasztja hello tevékenység szerializálása hello esemény küldése.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-205">For reasons that will become clear later, this code explicitly separates hello task of serializing and sending hello event.</span></span> <span data-ttu-id="8cfb2-206">hello előző kód hello hőmérséklet esemény be pufferbe rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-206">hello previous code serializes hello temperature event into a buffer.</span></span> <span data-ttu-id="8cfb2-207">Ezt követően **sendMessage** segítő függvény (szereplő **simplesample\_amqp**), hogy küld hello esemény tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-207">Then, **sendMessage** is a helper function (included in **simplesample\_amqp**) that sends hello event tooIoT Hub:</span></span>

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

<span data-ttu-id="8cfb2-208">Ez a kód része a hello **sendasync metódusok párhuzamosan** segédlet az előző szakaszban leírt hello, így azt nem ismerteti azt újra ide.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-208">This code is a subset of hello **SendAsync** helper described in hello previous section, so we won’t go over it again here.</span></span>

<span data-ttu-id="8cfb2-209">Hello előző kód toosend hello hőmérséklet esemény futtatásakor a szerializált képernyőn hello esemény érkezik tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-209">When we run hello previous code toosend hello Temperature event, this serialized form of hello event is sent tooIoT Hub:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="8cfb2-210">Azért küldjük egy hőmérséklet, amelynek a típusa **TemperatureEvent** és, hogy a struct tartalmaz egy **hőmérséklet** és **idő** tag.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-210">We're sending a temperature which is of type **TemperatureEvent** and that struct contains a **Temperature** and **Time** member.</span></span> <span data-ttu-id="8cfb2-211">Ez közvetlenül a hello szerializált adatok megjelennek.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-211">This is directly reflected in hello serialized data.</span></span>

<span data-ttu-id="8cfb2-212">Hasonlóképpen elküldhetjük a páratartalom esemény ezzel a kóddal:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-212">Similarly, we can send a humidity event with this code:</span></span>

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="8cfb2-213">elküldte a központ tooIoT szerializált hello űrlapot a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-213">hello serialized form that’s sent tooIoT Hub appears as follows:</span></span>

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="8cfb2-214">Ebben az esetben ez az elvárt módon.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-214">Again, this is as expected.</span></span>

<span data-ttu-id="8cfb2-215">Ebben a modellben hogyan további események is tekinthető könnyen lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-215">With this model, you can imagine how additional events can easily be added.</span></span> <span data-ttu-id="8cfb2-216">Adhat meg további struktúrák használatával **DECLARE\_STRUCT**, hello kapcsolódó esemény hello modell használatával is **WITH\_adatok**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-216">You define more structures using **DECLARE\_STRUCT**, and include hello corresponding event in hello model using **WITH\_DATA**.</span></span>

<span data-ttu-id="8cfb2-217">Most, hogy tartalmazzon hello módosítsa hello modell ugyanazokat az adatokat, de különböző struktúrával.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-217">Now, let’s modify hello model so that it includes hello same data but with a different structure.</span></span>

### <a name="model-2"></a><span data-ttu-id="8cfb2-218">A modell 2</span><span class="sxs-lookup"><span data-stu-id="8cfb2-218">Model 2</span></span>
<span data-ttu-id="8cfb2-219">Vegye figyelembe a egy másik modell toohello fent:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-219">Consider this alternative model toohello one above:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="8cfb2-220">Ebben az esetben azt biztosan nem hello **DECLARE\_STRUCT** makrók és egyszerűen meghatározásakor hello adatelemek a mi esetünkben a modellezési nyelvi hello egyszerű típus használatával.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-220">In this case we've eliminated hello **DECLARE\_STRUCT** macros and are simply defining hello data items from our scenario using simple types from hello modeling language.</span></span>

<span data-ttu-id="8cfb2-221">Hello pillanatra csak most figyelmen kívül hagyása hello **idő** esemény.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-221">Just for hello moment let’s ignore hello **Time** event.</span></span> <span data-ttu-id="8cfb2-222">Az adott tartalékoljon ez hello kód tooingress **hőmérséklet**:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-222">With that aside, here’s hello code tooingress **Temperature**:</span></span>

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

<span data-ttu-id="8cfb2-223">Ez a kód küldi hello következő szerializált esemény tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-223">This code sends hello following serialized event tooIoT Hub:</span></span>

```
{"Temperature":75}
```

<span data-ttu-id="8cfb2-224">És hello kódját hello páratartalom esemény küldése a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-224">And hello code for sending hello Humidity event appears as follows:</span></span>

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="8cfb2-225">Ez a kód elküldi a tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-225">This code sends this tooIoT Hub:</span></span>

```
{"Humidity":45}
```

<span data-ttu-id="8cfb2-226">Amennyiben még vannak nem meglepetések számát.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-226">So far there are still no surprises.</span></span> <span data-ttu-id="8cfb2-227">Most módosítsuk a hello SZERIALIZÁLÁSA makró felhasználási módját.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-227">Now let's change how we use hello SERIALIZE macro.</span></span>

<span data-ttu-id="8cfb2-228">Hello **SZERIALIZÁLÁSA** makró több adat eseményeket, mint szerepkör argumentumokban vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-228">hello **SERIALIZE** macro can take multiple data events as arguments.</span></span> <span data-ttu-id="8cfb2-229">Ez lehetővé teszi a us tooserialize hello **hőmérséklet** és **páratartalom** esemény együtt, és küldje el egy hívásban tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-229">This enables us tooserialize hello **Temperature** and **Humidity** event together and send them tooIoT Hub in one call:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="8cfb2-230">Előfordulhat, hogy kitalálja, hogy hello ezt a kódot eredménye két adatok események küldött tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-230">You might guess that hello result of this code is that two data events are sent tooIoT Hub:</span></span>

<span data-ttu-id="8cfb2-231">[</span><span class="sxs-lookup"><span data-stu-id="8cfb2-231">[</span></span>

<span data-ttu-id="8cfb2-232">{"Hőmérséklet": 75},</span><span class="sxs-lookup"><span data-stu-id="8cfb2-232">{"Temperature":75},</span></span>

<span data-ttu-id="8cfb2-233">{"Páratartalom": 45}</span><span class="sxs-lookup"><span data-stu-id="8cfb2-233">{"Humidity":45}</span></span>

<span data-ttu-id="8cfb2-234">]</span><span class="sxs-lookup"><span data-stu-id="8cfb2-234">]</span></span>

<span data-ttu-id="8cfb2-235">Ez azt jelenti, előfordulhat, hogy várt, hogy ez a kód van hello azonos elküldeni **hőmérséklet** és **páratartalom** külön-külön.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-235">In other words, you might expect that this code is hello same as sending **Temperature** and **Humidity** separately.</span></span> <span data-ttu-id="8cfb2-236">Csupán kényelmi toopass mindkét események túl**SZERIALIZÁLÁSA** hello azonos hívja.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-236">It’s just a convenience toopass both events too**SERIALIZE** in hello same call.</span></span> <span data-ttu-id="8cfb2-237">Azonban ez nem hello eset.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-237">However, that’s not hello case.</span></span> <span data-ttu-id="8cfb2-238">Ehelyett a fenti hello kódot küld a egyetlen esemény tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-238">Instead, hello code above sends this single data event tooIoT Hub:</span></span>

<span data-ttu-id="8cfb2-239">{"Hőmérséklet": 75, "páratartalom": 45}</span><span class="sxs-lookup"><span data-stu-id="8cfb2-239">{"Temperature":75, "Humidity":45}</span></span>

<span data-ttu-id="8cfb2-240">Ez úgy látszhat furcsa, mert a modell meghatározása **hőmérséklet** és **páratartalom** , két *külön* események:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-240">This may seem strange because our model defines **Temperature** and **Humidity** as two *separate* events:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="8cfb2-241">További toohello pont, azt nem a modell ezek az események hol **hőmérséklet** és **páratartalom** hello lévő azonos struktúra:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-241">More toohello point, we didn’t model these events where **Temperature** and **Humidity** are in hello same structure:</span></span>

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

<span data-ttu-id="8cfb2-242">Ez a modell esetén lenne könnyebb toounderstand hogyan **hőmérséklet** és **páratartalom** küldheti az adatokat a hello azonos szerializált üzenet.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-242">If we used this model, it would be easier toounderstand how **Temperature** and **Humidity** would be sent in hello same serialized message.</span></span> <span data-ttu-id="8cfb2-243">Azonban nem lehet egyértelmű ezért működik, hogy ha adja meg mindkét adatok események túl**SZERIALIZÁLÁSA** 2 modell használatával.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-243">However it may not be clear why it works that way when you pass both data events too**SERIALIZE** using model 2.</span></span>

<span data-ttu-id="8cfb2-244">Ez a viselkedés könnyebb toounderstand, ha adott hello tudja hello feltételezéseket **szerializáló** könyvtár okozza.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-244">This behavior is easier toounderstand if you know hello assumptions that hello **serializer** library is making.</span></span> <span data-ttu-id="8cfb2-245">Ezen toomake logika ugorjunk vissza tooour modell:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-245">toomake sense of this let’s go back tooour model:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="8cfb2-246">Ez a modell gondol objektumorientált feltételeit.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-246">Think of this model in object-oriented terms.</span></span> <span data-ttu-id="8cfb2-247">Ebben az esetben azt még modellezési egy fizikai eszköz (termosztát), és eszközökön is tulajdonságai, például a **hőmérséklet** és **páratartalom**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-247">In this case we’re modeling a physical device (a thermostat) and that device includes attributes like **Temperature** and **Humidity**.</span></span>

<span data-ttu-id="8cfb2-248">A modell hello alábbi kóddal teljes állapotának hello kérhessen:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-248">We can send hello entire state of our model with code such as hello following:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="8cfb2-249">Ha hello értékeknek, páratartalom és időtartam, az esemény például a elküldött tooIoT Hub volna látható:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-249">Assuming hello values of Temperature, Humidity and Time are set, we would see an event like this sent tooIoT Hub:</span></span>

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="8cfb2-250">Egyes esetekben előfordulhat, hogy csak szeretné toosend *néhány* hello modell toohello felhő (Ez a különösen igaz, ha a modell tartalmaz számos adatok események) tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-250">Sometimes you may only want toosend *some* properties of hello model toohello cloud (this is especially true if your model contains a large number of data events).</span></span> <span data-ttu-id="8cfb2-251">Hasznos toosend adatok események csak egy részét, mint a korábbi példában:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-251">It’s useful toosend only a subset of data events, such as in our earlier example:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="8cfb2-252">Ezt követően, hogy pontosan hello azonos szerializált esemény, mintha meghatározott kellett egy **TemperatureEvent** rendelkező egy **hőmérséklet** és **idő** tag, hasonló módon az azt volt a modell 1.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-252">This generates exactly hello same serialized event as if we had defined a **TemperatureEvent** with a **Temperature** and **Time** member, just as we did with model 1.</span></span> <span data-ttu-id="8cfb2-253">Ebben az esetben a rendszer pontosan hello azonos szerializált esemény egy másik modell (modell 2) használatával, mert hívtuk képes toogenerate **SZERIALIZÁLÁSA** eltérő módon.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-253">In this case we were able toogenerate exactly hello same serialized event by using a different model (model 2) because we called **SERIALIZE** in a different way.</span></span>

<span data-ttu-id="8cfb2-254">hello legfontosabb, hogy ha túl át az adatokat több esemény**SZERIALIZÁLÁSA,** és az azt feltételezi, hogy minden esemény egy adott JSON-objektum tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-254">hello important point is that if you pass multiple data events too**SERIALIZE,** then it assumes each event is a property in a single JSON object.</span></span>

<span data-ttu-id="8cfb2-255">hello ajánlott módszert, és hogyan úgy gondolja, hogy a modell kapcsolatos függ.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-255">hello best approach depends on you and how you think about your model.</span></span> <span data-ttu-id="8cfb2-256">Ha küldünk "események" toohello felhő, és minden esemény egy adott csoportján tulajdonságait tartalmazza, hello első módszer teszi nagy mennyiségű logika.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-256">If you’re sending "events" toohello cloud and each event contains a defined set of properties, then hello first approach makes a lot of sense.</span></span> <span data-ttu-id="8cfb2-257">Ebben az esetben használhatja **DECLARE\_STRUCT** toodefine hello minden esemény szerkezetét, és azokat a modellben az hello **WITH\_adatok** makró.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-257">In that case you would use **DECLARE\_STRUCT** toodefine hello structure of each event and then include them in your model with hello **WITH\_DATA** macro.</span></span> <span data-ttu-id="8cfb2-258">Ezután küldenie minden esemény hasonlóan az első példában hello fenti.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-258">Then you send each event as we did in hello first example above.</span></span> <span data-ttu-id="8cfb2-259">Ez a módszer az lenne csak át egy egyetlen esemény túl**SZERIALIZÁLÓ**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-259">In this approach you would only pass a single data event too**SERIALIZER**.</span></span>

<span data-ttu-id="8cfb2-260">Ha úgy gondolja, hogy a modell csak objektumorientált módon kapcsolatos, hello második megközelítés lehet, hogy megfelelően meg.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-260">If you think about your model in an object-oriented fashion, then hello second approach may suit you.</span></span> <span data-ttu-id="8cfb2-261">Ebben az esetben a megadott elemek hello **WITH\_adatok** hello "Tulajdonságok" az objektum.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-261">In this case, hello elements defined using **WITH\_DATA** are hello "properties" of your object.</span></span> <span data-ttu-id="8cfb2-262">Bármilyen események részhalmazát túl át**SZERIALIZÁLÁSA** , hogy van lehetősége, attól függően, hogy mennyi a "objektum" állapota toosend toohello felhő szeretné.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-262">You pass whatever subset of events too**SERIALIZE** that you like, depending on how much of your "object’s" state you want toosend toohello cloud.</span></span>

<span data-ttu-id="8cfb2-263">Nether megoldás, a megfelelő vagy nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-263">Nether approach is right or wrong.</span></span> <span data-ttu-id="8cfb2-264">Arról, hogyan fog hello **szerializáló** könyvtár működik, és mentse hello modellezési módszert alkalmaz, amely a leginkább megfelel az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-264">Just be aware of how hello **serializer** library works, and pick hello modeling approach that best fits your needs.</span></span>

## <a name="message-handling"></a><span data-ttu-id="8cfb2-265">Üzenet kezelése</span><span class="sxs-lookup"><span data-stu-id="8cfb2-265">Message handling</span></span>
<span data-ttu-id="8cfb2-266">Ebben a cikkben eddig csak rendelkezik tárgyalt küldő események tooIoT Hub, és még nem címzett üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-266">So far this article has only discussed sending events tooIoT Hub, and hasn't addressed receiving messages.</span></span> <span data-ttu-id="8cfb2-267">hello ok az okozza, hogy mit kell üzenetek fogadása tooknow nagy mértékben volt szó a egy [korábbi cikk](iot-hub-device-sdk-c-intro.md).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-267">hello reason for this is that what we need tooknow about receiving messages has largely been covered in an [earlier article](iot-hub-device-sdk-c-intro.md).</span></span> <span data-ttu-id="8cfb2-268">A cikkben visszahívás, hogy Ön által feldolgozott üzenetek üzenet visszahívási függvény regisztrálásával:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-268">Recall from that article that you process messages by registering a message callback function:</span></span>

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

<span data-ttu-id="8cfb2-269">Majd írási hello visszahívási függvény, amelyet ha egy üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-269">You then write hello callback function that’s invoked when a message is received:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable tooIoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed toomalloc\r\n");
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

<span data-ttu-id="8cfb2-270">Ez a megvalósítás a **IoTHubMessage** hívások hello adott függvény a modellben minden egyes művelethez.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-270">This implementation of **IoTHubMessage** calls hello specific function for each action in your model.</span></span> <span data-ttu-id="8cfb2-271">Ha például a modell határozza meg a műveletet:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-271">For example, if your model defines this action:</span></span>

```
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="8cfb2-272">Meg kell adnia a függvény a következő aláírást:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-272">You must define a function with this signature:</span></span>

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="8cfb2-273">**SetAirResistance** majd van meghívva, amikor az adott üzenettel tooyour eszköz.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-273">**SetAirResistance** is then called when that message is sent tooyour device.</span></span>

<span data-ttu-id="8cfb2-274">Mi azt még nem magyaráznak még verziója telepítve milyen hello szerializált üzenet tűnik.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-274">What we haven't explained yet is what hello serialized version of message looks like.</span></span> <span data-ttu-id="8cfb2-275">Ez azt jelenti Ha azt szeretné, hogy toosend egy **SetAirResistance** üzenet tooyour eszköz, what does adott néz?</span><span class="sxs-lookup"><span data-stu-id="8cfb2-275">In other words, if you want toosend a **SetAirResistance** message tooyour device, what does that look like?</span></span>

<span data-ttu-id="8cfb2-276">Ha egy üzenet tooa eszköz küldjük, tennénk hello Azure IoT Service SDK.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-276">If you're sending a message tooa device, you would do so through hello Azure IoT service SDK.</span></span> <span data-ttu-id="8cfb2-277">Szüksége van az tooknow mi karakterlánc toosend tooinvoke egy adott művelet.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-277">You still need tooknow what string toosend tooinvoke a particular action.</span></span> <span data-ttu-id="8cfb2-278">általános formát hello üzenetküldés a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-278">hello general format for sending a message appears as follows:</span></span>

```
{"Name" : "", "Parameters" : "" }
```

<span data-ttu-id="8cfb2-279">Küldünk egy szerializált JSON-objektum két tulajdonságokkal: **neve** hello művelet (üzenet) hello neve és **paraméterek** intézkedés hello paramétereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-279">You're sending a serialized JSON object with two properties: **Name** is hello name of hello action (message) and **Parameters** contains hello parameters of that action.</span></span>

<span data-ttu-id="8cfb2-280">Például tooinvoke **SetAirResistance** küldhet üzenetet tooa eszköz:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-280">For example, tooinvoke **SetAirResistance** you can send this message tooa device:</span></span>

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

<span data-ttu-id="8cfb2-281">hello művelet nevének pontosan meg kell egyeznie a modellben definiált művelet.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-281">hello action name must exactly match an action defined in your model.</span></span> <span data-ttu-id="8cfb2-282">hello paraméterének neve is meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-282">hello parameter names must match as well.</span></span> <span data-ttu-id="8cfb2-283">Azt is vegye figyelembe a kis-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-283">Also note case sensitivity.</span></span> <span data-ttu-id="8cfb2-284">**Név** és **paraméterek** a rendszer mindig a nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-284">**Name** and **Parameters** are always uppercase.</span></span> <span data-ttu-id="8cfb2-285">Győződjön meg arról, hogy toomatch hello kis-és a művelet nevének és a paraméterek a modellben.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-285">Make sure toomatch hello case of your action name and parameters in your model.</span></span> <span data-ttu-id="8cfb2-286">Ebben a példában hello művelet neve: "SetAirResistance", és nem "setairresistance".</span><span class="sxs-lookup"><span data-stu-id="8cfb2-286">In this example, hello action name is "SetAirResistance" and not "setairresistance".</span></span>

<span data-ttu-id="8cfb2-287">két más műveletek hello **TurnFanOn** és **TurnFanOff** is elindítható a üzenetek tooa eszköz elküldésével:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-287">hello two other actions **TurnFanOn** and **TurnFanOff** can be invoked by sending these messages tooa device:</span></span>

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

<span data-ttu-id="8cfb2-288">Ebben a szakaszban leírt mindent, amire szüksége tooknow, amikor az üzenetek események küldése és fogadása hello **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-288">This section described everything you need tooknow when sending events and receiving messages with hello **serializer** library.</span></span> <span data-ttu-id="8cfb2-289">Mielőtt továbblép, most is beállíthat néhány paraméter, a modell mekkora szabályozó foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-289">Before moving on, let's cover some parameters you can configure that control how large your model is.</span></span>

## <a name="macro-configuration"></a><span data-ttu-id="8cfb2-290">A makróban konfiguráció</span><span class="sxs-lookup"><span data-stu-id="8cfb2-290">Macro configuration</span></span>
<span data-ttu-id="8cfb2-291">Hello használata **szerializáló** könyvtár hello SDK toobe tudomást fontos része megtalálható hello azure-c-megosztott-segédprogram kódtára.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-291">If you’re using hello **Serializer** library an important part of hello SDK toobe aware of is found in hello azure-c-shared-utility library.</span></span>
<span data-ttu-id="8cfb2-292">Ha rendelkezik klónozott hello Azure iot-sdk--c tárház a Githubról hello – rekurzív lehetőség használatával, majd megtalálja a megosztott segédprogram kódtára itt:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-292">If you have cloned hello Azure-iot-sdk-c repository from GitHub using hello --recursive option, then you will find this shared utility library here:</span></span>

```
.\\c-utility
```

<span data-ttu-id="8cfb2-293">Ha nem rendelkezik klónozott hello könyvtár, megtalálhatja [Itt](https://github.com/Azure/azure-c-shared-utility).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-293">If you have not cloned hello library, you can find it [here](https://github.com/Azure/azure-c-shared-utility).</span></span>

<span data-ttu-id="8cfb2-294">Hello megosztott segédprogram kódtára, belül található a következő mappa hello:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-294">Within hello shared utility library, you will find hello following folder:</span></span>

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

<span data-ttu-id="8cfb2-295">Ez a mappa tartalmaz, amely a Visual Studio megoldást **makró\_utils\_h\_generator.sln**:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-295">This folder contains a Visual Studio solution called **macro\_utils\_h\_generator.sln**:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

<span data-ttu-id="8cfb2-296">Ebben a megoldásban hello program állít elő, hello **makró\_utils.h** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-296">hello program in this solution generates hello **macro\_utils.h** file.</span></span> <span data-ttu-id="8cfb2-297">Nincs alapértelmezett makró\_hello SDK utils.h fájl.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-297">There’s a default macro\_utils.h file included with hello SDK.</span></span> <span data-ttu-id="8cfb2-298">Ez a megoldás lehetővé teszi bizonyos paraméterek, és ezután hozza létre újra hello ezen paraméterek alapján fejlécfájlt toomodify.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-298">This solution allows you toomodify some parameters and then recreate hello header file based on these parameters.</span></span>

<span data-ttu-id="8cfb2-299">az érintett toobe hello két fő paraméterek **nArithmetic** és **nMacroParameters** amely makró található két sort definiált\_utils.tt:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-299">hello two key parameters toobe concerned with are **nArithmetic** and **nMacroParameters** which are defined in these two lines found in macro\_utils.tt:</span></span>

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

<span data-ttu-id="8cfb2-300">Ezek az értékek hello alapértelmezett paraméterek hello SDK része.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-300">These values are hello default parameters included with hello SDK.</span></span> <span data-ttu-id="8cfb2-301">Minden paraméternek hello jelentése a következő:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-301">Each parameter has hello following meaning:</span></span>

* <span data-ttu-id="8cfb2-302">nMacroParameters – meghatározza, hány paraméterek egy DECLARE lehet\_MODELL makró definíciója.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-302">nMacroParameters – Controls how many parameters you can have in one DECLARE\_MODEL macro definition.</span></span>
* <span data-ttu-id="8cfb2-303">nArithmetic – vezérlők hello tagok összes száma a modellben engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-303">nArithmetic – Controls hello total number of members allowed in a model.</span></span>

<span data-ttu-id="8cfb2-304">Ezek a paraméterek számára fontos hello oka, mivel azok vezérelni hogyan nagy is lehet a modellben.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-304">hello reason these parameters are important is because they control how large your model can be.</span></span> <span data-ttu-id="8cfb2-305">Vegyük példaként a modell definíciója:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-305">For example, consider this model definition:</span></span>

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

<span data-ttu-id="8cfb2-306">Ahogy korábban említettük **DECLARE\_MODELL** csak egy C makró.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-306">As mentioned previously, **DECLARE\_MODEL** is just a C macro.</span></span> <span data-ttu-id="8cfb2-307">hello modell és hello hello **WITH\_adatok** utasítás (még egy másik makró) paraméterei **DECLARE\_MODELL**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-307">hello names of hello model and hello **WITH\_DATA** statement (yet another macro) are parameters of **DECLARE\_MODEL**.</span></span> <span data-ttu-id="8cfb2-308">**nMacroParameters** határozza meg, hány paraméterek tartalmazhat **DECLARE\_MODELL**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-308">**nMacroParameters** defines how many parameters can be included in **DECLARE\_MODEL**.</span></span> <span data-ttu-id="8cfb2-309">Hatékonyan hogy hány adatok esemény és művelet nyilatkozatok, akkor is határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-309">Effectively, this defines how many data event and action declarations you can have.</span></span> <span data-ttu-id="8cfb2-310">És a hello alapértelmezett korlátja 124 Ez azt jelenti, hogy definiálhat egy modell készül 60 műveletek és adatok események kombinációjával.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-310">As such, with hello default limit of 124 this means that you can define a model with a combination of about 60 actions and data events.</span></span> <span data-ttu-id="8cfb2-311">Ha tooexceed ezt a határt, kapni fog a hely hasonló toothis fordítóprogram-hibákkal:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-311">If you try tooexceed this limit, you'll receive compiler errors that look similar toothis:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

<span data-ttu-id="8cfb2-312">Hello **nArithmetic** többet az az alkalmazás belső működését hello hello makró nyelvi paraméter.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-312">hello **nArithmetic** parameter is more about hello internal workings of hello macro language than your application.</span></span>  <span data-ttu-id="8cfb2-313">Azt szabályozza, hogy is szerepelhet a modell tagok összes száma hello beleértve **DECLARE_STRUCT** makrókat.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-313">It controls hello total number of members you can have in your model, including **DECLARE_STRUCT** macros.</span></span> <span data-ttu-id="8cfb2-314">Ha például a fordítóprogram-hibákkal jelenítse, akkor kell próbálja növelni **nArithmetic**:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-314">If you start seeing compiler errors such as this, then you should try increasing **nArithmetic**:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

<span data-ttu-id="8cfb2-315">Ha azt szeretné, toochange ezeket a paramétereket, módosítsa a hello értékeket hello makróban\_utils.tt fájl, a recompile hello makró\_utils\_h\_generator.sln megoldás, és futtassa hello lefordított program.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-315">If you want toochange these parameters, modify hello values in hello macro\_utils.tt file, recompile hello macro\_utils\_h\_generator.sln solution, and run hello compiled program.</span></span> <span data-ttu-id="8cfb2-316">Ha így tesz, így új makró\_utils.h fájl jön létre, ezért hello helyezett.\\ közös\\inc könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-316">When you do so, a new macro\_utils.h file is generated and placed in hello .\\common\\inc directory.</span></span>

<span data-ttu-id="8cfb2-317">Rendelés toouse hello új verziójában makró\_utils.h, eltávolítás hello **szerializáló** NuGet-csomagot a megoldás, és helyette tartalmaznak hello **szerializáló** Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-317">In order toouse hello new version of macro\_utils.h, remove hello **serializer** NuGet package from your solution and in its place include hello **serializer** Visual Studio project.</span></span> <span data-ttu-id="8cfb2-318">Ez lehetővé teszi, hogy a kód toocompile hello forráskódját hello szerializáló könyvtár ellen.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-318">This enables your code toocompile against hello source code of hello serializer library.</span></span> <span data-ttu-id="8cfb2-319">Ez magában foglalja a frissített hello makró\_utils.h.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-319">This includes hello updated macro\_utils.h.</span></span> <span data-ttu-id="8cfb2-320">Ha azt szeretné, toodo ezt **simplesample\_amqp**, első lépésként hello megoldás NuGet-csomag hello hello szerializáló szalagtár eltávolítása:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-320">If you want toodo this for **simplesample\_amqp**, start by removing hello NuGet package for hello serializer library from hello solution:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

<span data-ttu-id="8cfb2-321">Majd adja hozzá a projekt tooyour Visual Studio megoldás:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-321">Then add this project tooyour Visual Studio solution:</span></span>

> <span data-ttu-id="8cfb2-322">. \\c\\szerializáló\\build\\windows\\serializer.vcxproj</span><span class="sxs-lookup"><span data-stu-id="8cfb2-322">.\\c\\serializer\\build\\windows\\serializer.vcxproj</span></span>
> 
> 

<span data-ttu-id="8cfb2-323">Amikor elkészült, a megoldás így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-323">When you're done, your solution should look like this:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

<span data-ttu-id="8cfb2-324">Ha fordítása a megoldás hello makró frissítése most\_utils.h a bináris tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-324">Now when you compile your solution, hello updated macro\_utils.h is included in your binary.</span></span>

<span data-ttu-id="8cfb2-325">Vegye figyelembe, hogy ezeket az értékeket elég magas növelése lépheti túl a fordítóprogram korlátok.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-325">Note that increasing these values high enough can exceed compiler limits.</span></span> <span data-ttu-id="8cfb2-326">toothis mutasson, hello **nMacroParameters** hello fő paraméter mely érintett toobe van.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-326">toothis point, hello **nMacroParameters** is hello main parameter with which toobe concerned.</span></span> <span data-ttu-id="8cfb2-327">hello C99 spec határozza meg, hogy 127 paraméterek közül legalább egy makró definition engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-327">hello C99 spec specifies that a minimum of 127 parameters are allowed in a macro definition.</span></span> <span data-ttu-id="8cfb2-328">hello Microsoft fordító hello spec pontosan követi (és a maximális hossza 127), ezért nem fogja tudni tooincrease **nMacroParameters** hello alapértelmezett felett.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-328">hello Microsoft compiler follows hello spec exactly (and has a limit of 127), so you won't be able tooincrease **nMacroParameters** beyond hello default.</span></span> <span data-ttu-id="8cfb2-329">Más compilers – előfordulhat, hogy engedélyezi toodo így (például hello GNU fordító magasabb határérték támogatja).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-329">Other compilers might allow you toodo so (for example, hello GNU compiler supports a higher limit).</span></span>

<span data-ttu-id="8cfb2-330">Eddig azt már szabályozott szinte minden tooknow kapcsolatos hogyan toowrite hello a kód szükséges **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-330">So far we've covered just about everything you need tooknow about how toowrite code with hello **serializer** library.</span></span> <span data-ttu-id="8cfb2-331">Mielőtt áthaladva addig tart, most le újra az előző cikket, esetleg kíváncsi kapcsolatos témaköröket.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-331">Before concluding, let's revisit some topics from previous articles that you may be wondering about.</span></span>

## <a name="hello-lower-level-apis"></a><span data-ttu-id="8cfb2-332">hello alacsonyabb szintű API-k</span><span class="sxs-lookup"><span data-stu-id="8cfb2-332">hello lower-level APIs</span></span>
<span data-ttu-id="8cfb2-333">Ez a cikk arra irányul hello mintaalkalmazás **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-333">hello sample application on which this article focused is **simplesample\_amqp**.</span></span> <span data-ttu-id="8cfb2-334">Ez a minta hello magasabb szintű (hello nem "r") API-k toosend eseményeket használ, és üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-334">This sample uses hello higher-level (hello non-"LL") APIs toosend events and receive messages.</span></span> <span data-ttu-id="8cfb2-335">Ezen API-k használatakor a háttérszálon fut, amely gondoskodik az események üzenetek küldése és fogadása.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-335">If you use these APIs, a background thread runs which takes care of both sending events and receiving messages.</span></span> <span data-ttu-id="8cfb2-336">Azonban hello alacsonyabb szintű (r) API-k tooeliminate használja a háttérszálon, és ha küldi az eseményeket, vagy-üzeneteket fogadjon hello felhő explicit vezérlését.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-336">However, you can use hello lower-level (LL) APIs tooeliminate this background thread and take explicit control over when you send events or receive messages from hello cloud.</span></span>

<span data-ttu-id="8cfb2-337">A egy [előző cikkben](iot-hub-device-sdk-c-iothubclient.md), nincs olyan hello áll funkciók összessége magasabb szintű API-kat:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-337">As described in a [previous article](iot-hub-device-sdk-c-iothubclient.md), there is a set of functions that consists of hello higher-level APIs:</span></span>

* <span data-ttu-id="8cfb2-338">IoTHubClient\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="8cfb2-338">IoTHubClient\_CreateFromConnectionString</span></span>
* <span data-ttu-id="8cfb2-339">IoTHubClient\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="8cfb2-339">IoTHubClient\_SendEventAsync</span></span>
* <span data-ttu-id="8cfb2-340">IoTHubClient\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="8cfb2-340">IoTHubClient\_SetMessageCallback</span></span>
* <span data-ttu-id="8cfb2-341">IoTHubClient\_megszüntetése</span><span class="sxs-lookup"><span data-stu-id="8cfb2-341">IoTHubClient\_Destroy</span></span>

<span data-ttu-id="8cfb2-342">Az ezen API-k egy **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-342">These APIs are demonstrated in **simplesample\_amqp**.</span></span>

<span data-ttu-id="8cfb2-343">Szerepel továbbá egy hasonló alacsonyabb szintű API-készlet.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-343">There is also an analogous set of lower-level APIs.</span></span>

* <span data-ttu-id="8cfb2-344">IoTHubClient\_inden\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="8cfb2-344">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="8cfb2-345">IoTHubClient\_inden\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="8cfb2-345">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="8cfb2-346">IoTHubClient\_inden\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="8cfb2-346">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="8cfb2-347">IoTHubClient\_inden\_megszüntetése</span><span class="sxs-lookup"><span data-stu-id="8cfb2-347">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="8cfb2-348">Vegye figyelembe, hogy hello alacsonyabb szintű API-k munkahelyi pontosan hello azonos módon hello előző cikkeiben.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-348">Note that hello lower-level APIs work exactly hello same way as described in hello previous articles.</span></span> <span data-ttu-id="8cfb2-349">Hello első API-készlet is használhatja, ha azt szeretné, hogy a háttér szál toohandle üzenetek események küldése és fogadása.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-349">You can use hello first set of APIs if you want a background thread toohandle sending events and receiving messages.</span></span> <span data-ttu-id="8cfb2-350">Ha azt szeretné, hogy explicit szabályozhatják, amikor Ön adatokat küldeni és fogadni az IoT-központ használhatja hello második API-készlet.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-350">You use hello second set of APIs if you want explicit control over when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="8cfb2-351">API-k munkahelyi vagy készletét a hello is egyaránt jól **szerializáló** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-351">Either set of APIs work equally well with hello **serializer** library.</span></span>

<span data-ttu-id="8cfb2-352">A példa bemutatja, hogyan hello alacsonyabb szintű API-k használt hello **szerializáló** könyvtár, lásd: hello **simplesample\_http** alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-352">For an example of how hello lower-level APIs are used with hello **serializer** library, see hello **simplesample\_http** application.</span></span>

## <a name="additional-topics"></a><span data-ttu-id="8cfb2-353">További témakörök</span><span class="sxs-lookup"><span data-stu-id="8cfb2-353">Additional topics</span></span>
<span data-ttu-id="8cfb2-354">Érdemes megemlíteni néhány más témakörök újra szolgálnak tulajdonság kezelési, alternatív hitelesítő adatai és konfigurációs lehetőség használatával.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-354">A few other topics worth mentioning again are property handling, using alternate device credentials, and configuration options.</span></span> <span data-ttu-id="8cfb2-355">Ezek a témakörök ismertetett egy [előző cikkben](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="8cfb2-355">These are all topics covered in a [previous article](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="8cfb2-356">hello fő pont, hogy ezek a funkciók működni hello azonos módon a hello **szerializáló** könyvtár, mint a hello **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-356">hello main point is that all of these features work in hello same way with hello **serializer** library as they do with hello **IoTHubClient** library.</span></span> <span data-ttu-id="8cfb2-357">Például, ha tooattach tulajdonságok tooan esemény a modellből, akkor használja a **IoTHubMessage\_tulajdonságok** és **térkép**\_**AddorUpdate**, hasonlóan a fentiekben ismertetett hello:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-357">For example, if you want tooattach properties tooan event from your model, you use **IoTHubMessage\_Properties** and **Map**\_**AddorUpdate**, hello same way as described previously:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="8cfb2-358">E hello esemény amelyből létre lett hozva hello **szerializáló** könyvtárban vagy hello segítségével manuálisan létrehozott **IoTHubClient** könyvtár nem számít.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-358">Whether hello event was generated from hello **serializer** library or created manually using hello **IoTHubClient** library does not matter.</span></span>

<span data-ttu-id="8cfb2-359">Hello a alternatív hitelesítő adatai, használatával **IoTHubClient\_inden\_létrehozása** is működik, **IoTHubClient\_CreateFromConnectionString** a rendeljen egy **IOT HUBBAL\_ügyfél\_KEZELNI**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-359">For hello alternate device credentials, using **IoTHubClient\_LL\_Create** works just as well as **IoTHubClient\_CreateFromConnectionString** for allocating an **IOTHUB\_CLIENT\_HANDLE**.</span></span>

<span data-ttu-id="8cfb2-360">Végezetül hello használata **szerializáló** könyvtár, állíthatja be a konfigurációs beállítások **IoTHubClient\_inden\_SetOption** csak úgy, ahogy hello használatakor**IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-360">Finally, if you're using hello **serializer** library, you can set configuration options with **IoTHubClient\_LL\_SetOption** just as you did when using hello **IoTHubClient** library.</span></span>

<span data-ttu-id="8cfb2-361">Egy szolgáltatás, amely egyedi toohello **szerializáló** könyvtárban hello inicializálási API-k.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-361">A feature that is unique toohello **serializer** library are hello initialization APIs.</span></span> <span data-ttu-id="8cfb2-362">Hello könyvtár használata előtt meg kell hívnia **szerializáló\_init**:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-362">Before you can start working with hello library, you must call **serializer\_init**:</span></span>

```
serializer_init(NULL);
```

<span data-ttu-id="8cfb2-363">Ez történik, mielőtt meghívja a **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-363">This is done just before you call **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="8cfb2-364">Hasonlóképpen, ha elkészült hello szalagtár használata esetén az túl van hello utolsó hívás meg kell nyitnia a**szerializáló\_deinit**:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-364">Similarly, when you're done working with hello library, hello last call you’ll make is too**serializer\_deinit**:</span></span>

```
serializer_deinit();
```

<span data-ttu-id="8cfb2-365">Ellenkező esetben az összes hello más a fent felsorolt szolgáltatások munkahelyi hello ugyanaz a hello **szerializáló** könyvtár mint hello **IoTHubClient** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-365">Otherwise, all of hello other features listed above work hello same in hello **serializer** library as they do in hello **IoTHubClient** library.</span></span> <span data-ttu-id="8cfb2-366">Az e témakörökkel kapcsolatos további információkért lásd: hello [előző cikkben](iot-hub-device-sdk-c-iothubclient.md) a sorozat.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-366">For more information about any of these topics, see hello [previous article](iot-hub-device-sdk-c-iothubclient.md) in this series.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8cfb2-367">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8cfb2-367">Next steps</span></span>
<span data-ttu-id="8cfb2-368">Ez a cikk ismerteti a részletes hello egyedi szempontjait hello **szerializáló** hello szereplő könyvtár **C-hez készült SDK Azure IoT-eszközök**. Hello információk az alapos megértése, hogyan toouse modellek toosend események és üzenetek fogadása az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-368">This article describes in detail hello unique aspects of hello **serializer** library contained in hello **Azure IoT device SDK for C**. With hello information provided you should have a good understanding of how toouse models toosend events and receive messages from IoT Hub.</span></span>

<span data-ttu-id="8cfb2-369">Ennyi lenne hello háromrészes series hogyan toodevelop alkalmazások hello **C-hez készült SDK Azure IoT-eszközök**. Ez a kell elegendő információt toonot csak get használatba lehet, de alapos ismerete, hogyan hello API-k biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-369">This also concludes hello three-part series on how toodevelop applications with hello **Azure IoT device SDK for C**. This should be enough information toonot only get you started but give you a thorough understanding of how hello APIs work.</span></span> <span data-ttu-id="8cfb2-370">További információkért nincsenek néhány minták hello SDK nem vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-370">For additional information, there are a few samples in hello SDK not covered here.</span></span> <span data-ttu-id="8cfb2-371">Ellenkező esetben hello [SDK-dokumentáció](https://github.com/Azure/azure-iot-sdk-c) helyes erőforrás további információt.</span><span class="sxs-lookup"><span data-stu-id="8cfb2-371">Otherwise, hello [SDK documentation](https://github.com/Azure/azure-iot-sdk-c) is a good resource for additional information.</span></span>

<span data-ttu-id="8cfb2-372">az IoT-központ fejlesztésével kapcsolatos további toolearn lásd: hello [Azure IoT SDK-k][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="8cfb2-372">toolearn more about developing for IoT Hub, see hello [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="8cfb2-373">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="8cfb2-373">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="8cfb2-374">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="8cfb2-374">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
