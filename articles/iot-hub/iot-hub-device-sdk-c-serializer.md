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
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a>Az Azure IoT-eszközök SDK c – további információk a szerializáló
Hello [először a következő cikket:](iot-hub-device-sdk-c-intro.md) az a sorozat bevezetett hello **C-hez készült SDK Azure IoT-eszközök**. hello a következő cikk megadott hello részletesebb leírását [ **IoTHubClient** ](iot-hub-device-sdk-c-iothubclient.md). Ez a cikk befejezése hello SDK körét, adja meg a fennmaradó összetevő hello részletesebb leírását: hello **szerializáló** könyvtár.

hello bevezető cikkben leírt hogyan toouse hello **szerializáló** könyvtár toosend események tooand üzenetek fogadása az IoT-központot. Ebben a cikkben tovább bővítjük ezekre a kérdésekre adott válaszokat azáltal, hogy részletesebb magyarázatát, hogy hogyan toomodel az adatok hello **szerializáló** makró nyelv. hello cikket is tartalmaz, hogyan hello könyvtár rendezi sorba üzenetek további információkra (és egyes esetekben szabályozásának hello szerializálási viselkedés). Egyes paraméterek módosíthatja hello modellek hello mérete meghatározó is ismerteti.

Végezetül hello cikk revisits néhány témaköre például üzenet és a tulajdonság kezelési előző cikkek tárgyalja. Módon fejeződött be, ezek a szolgáltatások munkahelyi megkeressük hello azonos módon használatával hello **szerializáló** könyvtár, mint a hello **IoTHubClient** könyvtárban.

Minden cikkben leírt hello alapul **szerializáló** SDK minták. Ha azt szeretné, hogy mentén toofollow, tekintse meg a hello **simplesample\_amqp** és **simplesample\_http** alkalmazások hello Azure IoT-eszközök SDK szerepel a c kiszolgálóra.

Hello található [ **C-hez készült SDK Azure IoT-eszközök** ](https://github.com/Azure/azure-iot-sdk-c) hello API hello a GitHub tárház és nézet adatai [C API-referencia](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="hello-modeling-language"></a>hello modellezési nyelv
Hello [bevezető cikkben](iot-hub-device-sdk-c-intro.md) az a sorozat bevezetett hello **C-hez készült SDK Azure IoT-eszközök** hello példa hello megadott nyelv modellezési **simplesample\_amqp**  alkalmazás:

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

Ahogy látja, nyelvi modellezési hello C makrók alapul. Mindig megkezdi a definíció **BEGIN\_NÉVTÉR** és mindig végződhet **END\_NÉVTÉR**. Közös tooname hello névtér a vállalat vagy a példában, amelyet használ a hello projekt legyen.

Mi kerül hello névtéren belül modell definíciókat. Ebben az esetben van egy anemometer egyetlen modellt. Ebben az esetben hello modell elnevezheti semmit, de általában ez nevű hello eszköz vagy az adattípus az IoT hubbal kívánt tooexchange.  

Modellek tartalmazza a következő definícióját hello események érkező tooIoT Hub is (hello *adatok*) továbbá az IoT-központ fogadhat köszönőüzenetei (hello *műveletek*). Mint hello példa látható, események rendelkezik-e egy típust és egy nevet; műveletek nevét és a választható paraméterek: (minden típusú) rendelkezik.

Mi nem mutatja be ezt a mintát hello SDK által támogatott további adattípusok. Oktatóanyag a következőket ismerteti, hogy a Tovább gombra.

> [!NOTE]
> Az IoT-központ hivatkozik egy eszköz küld, tooit toohello adatok *események*, míg a modellezési nyelvi hello hivatkozik, tooit *adatok* (segítségével meghatározott **WITH_DATA**). Hasonlóképpen, az IoT-központ hivatkozik, toodevices küldött toohello adatait *üzenetek*, míg a modellezési nyelvi hello hivatkozik, tooit *műveletek* (segítségével meghatározott **WITH_ACTION**). Vegye figyelembe, hogy ezek a feltételek szabadon felcserélhetők ebben a cikkben.
> 
> 

### <a name="supported-data-types"></a>Támogatott adattípusokat
a következő adattípusokat hello tartalmazó hello modellek támogatottak **szerializáló** könyvtár:

| Típus | Leírás |
| --- | --- |
| Dupla |dupla pontosságú lebegőpontos szám |
| int |32 bites egész szám |
| Lebegőpontos |az egyszeres pontosságú lebegőpontos szám |
| hosszú |hosszú egész szám |
| int8\_t |8 bites egész szám |
| Int16\_t |16 bites egész szám |
| Int32\_t |32 bites egész szám |
| Int64\_t |64 bites egész szám |
| logikai érték |Logikai érték |
| ASCII\_char\_ptr |ASCII-karakterlánc |
| EDM\_DÁTUM\_IDŐ\_ELTOLÁSA |dátum idő eltolása |
| EDM\_GUID |GUID |
| EDM\_BINÁRIS |Bináris |
| DEKLARÁLJA\_STRUKTÚRA |Összetett adattípusú |

Kezdjük hello utolsó adattípus. Hello **DECLARE\_STRUCT** lehetővé teszi a toodefine összetett adattípusú, amelyek a hello csoportosításait. Emellett más egyszerű típusokhoz. Ezek a Csoportosítások lehetővé teszik a számunkra toodefine modell, amely a következőképpen néz ki:

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

A modell típusú egyetlen eseményt tartalmaz **TestType**. **TestType** több tag, amelyek együttesen bemutatása hello hello által támogatott primitív típusok tartalmazó összetett típus **szerializáló** modeling language.

Az ilyen modellek azt írhat kódot toosend adatok tooIoT Hub, amely a következőképpen jelenik meg:

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

Alapvetően, azt még hozzárendelése érték tooevery tagja hello **teszt** szerkezetét, és azt, majd hívja **sendasync metódusok párhuzamosan** toosend hello **teszt** adatok esemény toohello felhő. **Sendasync metódusok párhuzamosan** küld egy egyetlen esemény tooIoT Hub segítő függvény:

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

Ez a függvény megadott adatok esemény hello rendezi sorba, és elküldi azt tooIoT központ használatával **IoTHubClient\_SendEventAsync**. Ez az előző cikkekben ismertetett ugyanazt a kódot hello (**sendasync metódusok párhuzamosan** magában foglalja a hello logika egy kényelmes függvénynek).

Egy korábbi kód hello használt egyéb segítő függvény **GetDateTimeOffset**. Ez a funkció átalakítja az idő típusú érték a megadott hello **EDM\_dátum\_idő\_ELTOLÁS**:

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

Ha futtatja ezt a kódot, a következő üzenet hello tooIoT Hub küld:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Vegye figyelembe, hogy hello szerializálási JSON, amely hello által generált hello formátum **szerializáló** könyvtár. Is vegye figyelembe, hogy minden egyes tagjára hello szerializált JSON-objektum megegyezik-e hello hello tagjai **TestType** , amely meghatározott a modellben. hello is pontosan megegyeznek a hello kód. Azonban vegye figyelembe, hogy hello bináris adatok base64-kódolású: "AQID" hello van az alkalmazás base64 kódolást {0x01, 0x02, 0x03}.

Ez a példa bemutatja, hello használatának előnye hello **szerializáló** könyvtár--lehetővé velünk toosend JSON toohello felhő, anélkül, hogy az alkalmazás a szerializálási foglalkozik tooexplicitly. Összes tooworry tudunk kapcsolatos van hello értékeinek beállítása a hello adatok események tekinthetők és egyszerű API-k toosend ezen események toohello felhő majd hívja.

Az információ azt adhatja meg a modellek, amelyek közé tartozik a támogatott adattípusok, beleértve a komplex típusok (sikerült is magában foglalja az összetett típusok más összetett típusok belül) hello tartományán. Azonban úgy szerializált létrehozott JSON hello által a fenti példában egy fontos pont megjelenik. *Hogyan* hello adatok küldünk **szerializáló** könyvtár meghatározza, hogy pontosan hogyan hello JSON formátuma. Hogy az adott pont áll, mi oktatóanyag a következőket ismerteti mellett.

## <a name="more-about-serialization"></a>További információ a szerializálási
hello előző szakaszban kiemeli a hello által generált hello kimeneti példát **szerializáló** könyvtár. Ebben a részben azt ismertetjük, hogyan hello könyvtár rendezi sorba adatok, és hogy miként szabályozható viselkedésmódot hello szerializálási API-k használatával.

Rendelés tooadvance hello döntéseken szerializálási, a dolgozunk lesz az új modell egy termosztát alapján. Először hozzuk biztosít bizonyos tapasztalattal hello forgatókönyvön keresztül próbáljuk tooaddress.

Azt szeretnénk, ha egy termosztát, és mérheti a hőmérséklet és a páratartalom toomodel. Minden adat tooIoT Hub másképp küldött toobe lesz. Alapértelmezés szerint hello termosztát ingresses hőmérséklet-esemény egyszer 2 percenként; a páratartalom esemény ingressed 15 percenként egyszer. Ha bármelyik esemény ingressed, tartalmaznia kell egy Timestamp típusú, amely hello időt jeleníti meg, hogy hello megfelelő hőmérséklet vagy páratartalom mérték.

Ebben a forgatókönyvben tekintve lesz bemutatjuk, két különböző módon toomodel hello adatokat, és azt ismertetjük, hogy modellezési rendelkezik a hello szerializált kimeneti hello hatása.

### <a name="model-1"></a>1 modell
Hello első verzió-modell, hogy támogatja hello az előző példában a következő:

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

Vegye figyelembe, hogy hello modell az két adatok eseményeket is tartalmazza: **hőmérséklet** és **páratartalom**. Ellentétben a korábbi példákhoz hello a minden esemény típus megadott struktúra **DECLARE\_STRUCT**. **TemperatureEvent** tartalmaz egy hőmérséklet mérési és egy időbélyegzőt adnak; **HumidityEvent** páratartalom a mérés és egy időbélyegzőt adnak tartalmazza. Ez a modell biztosítanak számunkra a természetes módon toomodel hello adatokat a fent leírt hello forgatókönyvhöz. Egy esemény toohello felhőben kapni, amikor hőmérséklet/időbélyeg vagy páratartalom/időbélyeg-párból vagy küldünk.

Elküldhetjük hőmérséklet esemény toohello felhő hello alábbi kód használatával:

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

Rendszer fogja hőmérséklet és a páratartalom hello mintakód a kódolt értékeket használja, de a képzelhető el, hogy azt ténylegesen olvas be ezeket az értékeket hello megfelelő érzékelők hello termosztát a mintavétellel.

a fenti hello kódot használja hello **GetDateTimeOffset** korábban bevezetett segítő. Törölje későbbi lesz okokból ezt a kódot explicit módon elválasztja hello tevékenység szerializálása hello esemény küldése. hello előző kód hello hőmérséklet esemény be pufferbe rendezi sorba. Ezt követően **sendMessage** segítő függvény (szereplő **simplesample\_amqp**), hogy küld hello esemény tooIoT Hub:

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

Ez a kód része a hello **sendasync metódusok párhuzamosan** segédlet az előző szakaszban leírt hello, így azt nem ismerteti azt újra ide.

Hello előző kód toosend hello hőmérséklet esemény futtatásakor a szerializált képernyőn hello esemény érkezik tooIoT Hub:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Azért küldjük egy hőmérséklet, amelynek a típusa **TemperatureEvent** és, hogy a struct tartalmaz egy **hőmérséklet** és **idő** tag. Ez közvetlenül a hello szerializált adatok megjelennek.

Hasonlóképpen elküldhetjük a páratartalom esemény ezzel a kóddal:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

elküldte a központ tooIoT szerializált hello űrlapot a következőképpen jelenik meg:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Ebben az esetben ez az elvárt módon.

Ebben a modellben hogyan további események is tekinthető könnyen lehet hozzáadni. Adhat meg további struktúrák használatával **DECLARE\_STRUCT**, hello kapcsolódó esemény hello modell használatával is **WITH\_adatok**.

Most, hogy tartalmazzon hello módosítsa hello modell ugyanazokat az adatokat, de különböző struktúrával.

### <a name="model-2"></a>A modell 2
Vegye figyelembe a egy másik modell toohello fent:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Ebben az esetben azt biztosan nem hello **DECLARE\_STRUCT** makrók és egyszerűen meghatározásakor hello adatelemek a mi esetünkben a modellezési nyelvi hello egyszerű típus használatával.

Hello pillanatra csak most figyelmen kívül hagyása hello **idő** esemény. Az adott tartalékoljon ez hello kód tooingress **hőmérséklet**:

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

Ez a kód küldi hello következő szerializált esemény tooIoT Hub:

```
{"Temperature":75}
```

És hello kódját hello páratartalom esemény küldése a következőképpen jelenik meg:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Ez a kód elküldi a tooIoT Hub:

```
{"Humidity":45}
```

Amennyiben még vannak nem meglepetések számát. Most módosítsuk a hello SZERIALIZÁLÁSA makró felhasználási módját.

Hello **SZERIALIZÁLÁSA** makró több adat eseményeket, mint szerepkör argumentumokban vehet igénybe. Ez lehetővé teszi a us tooserialize hello **hőmérséklet** és **páratartalom** esemény együtt, és küldje el egy hívásban tooIoT Hub:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Előfordulhat, hogy kitalálja, hogy hello ezt a kódot eredménye két adatok események küldött tooIoT Hub:

[

{"Hőmérséklet": 75},

{"Páratartalom": 45}

]

Ez azt jelenti, előfordulhat, hogy várt, hogy ez a kód van hello azonos elküldeni **hőmérséklet** és **páratartalom** külön-külön. Csupán kényelmi toopass mindkét események túl**SZERIALIZÁLÁSA** hello azonos hívja. Azonban ez nem hello eset. Ehelyett a fenti hello kódot küld a egyetlen esemény tooIoT Hub:

{"Hőmérséklet": 75, "páratartalom": 45}

Ez úgy látszhat furcsa, mert a modell meghatározása **hőmérséklet** és **páratartalom** , két *külön* események:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

További toohello pont, azt nem a modell ezek az események hol **hőmérséklet** és **páratartalom** hello lévő azonos struktúra:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Ez a modell esetén lenne könnyebb toounderstand hogyan **hőmérséklet** és **páratartalom** küldheti az adatokat a hello azonos szerializált üzenet. Azonban nem lehet egyértelmű ezért működik, hogy ha adja meg mindkét adatok események túl**SZERIALIZÁLÁSA** 2 modell használatával.

Ez a viselkedés könnyebb toounderstand, ha adott hello tudja hello feltételezéseket **szerializáló** könyvtár okozza. Ezen toomake logika ugorjunk vissza tooour modell:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Ez a modell gondol objektumorientált feltételeit. Ebben az esetben azt még modellezési egy fizikai eszköz (termosztát), és eszközökön is tulajdonságai, például a **hőmérséklet** és **páratartalom**.

A modell hello alábbi kóddal teljes állapotának hello kérhessen:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Ha hello értékeknek, páratartalom és időtartam, az esemény például a elküldött tooIoT Hub volna látható:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Egyes esetekben előfordulhat, hogy csak szeretné toosend *néhány* hello modell toohello felhő (Ez a különösen igaz, ha a modell tartalmaz számos adatok események) tulajdonságait. Hasznos toosend adatok események csak egy részét, mint a korábbi példában:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Ezt követően, hogy pontosan hello azonos szerializált esemény, mintha meghatározott kellett egy **TemperatureEvent** rendelkező egy **hőmérséklet** és **idő** tag, hasonló módon az azt volt a modell 1. Ebben az esetben a rendszer pontosan hello azonos szerializált esemény egy másik modell (modell 2) használatával, mert hívtuk képes toogenerate **SZERIALIZÁLÁSA** eltérő módon.

hello legfontosabb, hogy ha túl át az adatokat több esemény**SZERIALIZÁLÁSA,** és az azt feltételezi, hogy minden esemény egy adott JSON-objektum tulajdonság.

hello ajánlott módszert, és hogyan úgy gondolja, hogy a modell kapcsolatos függ. Ha küldünk "események" toohello felhő, és minden esemény egy adott csoportján tulajdonságait tartalmazza, hello első módszer teszi nagy mennyiségű logika. Ebben az esetben használhatja **DECLARE\_STRUCT** toodefine hello minden esemény szerkezetét, és azokat a modellben az hello **WITH\_adatok** makró. Ezután küldenie minden esemény hasonlóan az első példában hello fenti. Ez a módszer az lenne csak át egy egyetlen esemény túl**SZERIALIZÁLÓ**.

Ha úgy gondolja, hogy a modell csak objektumorientált módon kapcsolatos, hello második megközelítés lehet, hogy megfelelően meg. Ebben az esetben a megadott elemek hello **WITH\_adatok** hello "Tulajdonságok" az objektum. Bármilyen események részhalmazát túl át**SZERIALIZÁLÁSA** , hogy van lehetősége, attól függően, hogy mennyi a "objektum" állapota toosend toohello felhő szeretné.

Nether megoldás, a megfelelő vagy nem megfelelő. Arról, hogyan fog hello **szerializáló** könyvtár működik, és mentse hello modellezési módszert alkalmaz, amely a leginkább megfelel az igényeinek.

## <a name="message-handling"></a>Üzenet kezelése
Ebben a cikkben eddig csak rendelkezik tárgyalt küldő események tooIoT Hub, és még nem címzett üzenetek fogadására. hello ok az okozza, hogy mit kell üzenetek fogadása tooknow nagy mértékben volt szó a egy [korábbi cikk](iot-hub-device-sdk-c-intro.md). A cikkben visszahívás, hogy Ön által feldolgozott üzenetek üzenet visszahívási függvény regisztrálásával:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Majd írási hello visszahívási függvény, amelyet ha egy üzenet jelenik meg:

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

Ez a megvalósítás a **IoTHubMessage** hívások hello adott függvény a modellben minden egyes művelethez. Ha például a modell határozza meg a műveletet:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Meg kell adnia a függvény a következő aláírást:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** majd van meghívva, amikor az adott üzenettel tooyour eszköz.

Mi azt még nem magyaráznak még verziója telepítve milyen hello szerializált üzenet tűnik. Ez azt jelenti Ha azt szeretné, hogy toosend egy **SetAirResistance** üzenet tooyour eszköz, what does adott néz?

Ha egy üzenet tooa eszköz küldjük, tennénk hello Azure IoT Service SDK. Szüksége van az tooknow mi karakterlánc toosend tooinvoke egy adott művelet. általános formát hello üzenetküldés a következőképpen jelenik meg:

```
{"Name" : "", "Parameters" : "" }
```

Küldünk egy szerializált JSON-objektum két tulajdonságokkal: **neve** hello művelet (üzenet) hello neve és **paraméterek** intézkedés hello paramétereket tartalmaz.

Például tooinvoke **SetAirResistance** küldhet üzenetet tooa eszköz:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

hello művelet nevének pontosan meg kell egyeznie a modellben definiált művelet. hello paraméterének neve is meg kell egyeznie. Azt is vegye figyelembe a kis-és nagybetűk. **Név** és **paraméterek** a rendszer mindig a nagybetűk. Győződjön meg arról, hogy toomatch hello kis-és a művelet nevének és a paraméterek a modellben. Ebben a példában hello művelet neve: "SetAirResistance", és nem "setairresistance".

két más műveletek hello **TurnFanOn** és **TurnFanOff** is elindítható a üzenetek tooa eszköz elküldésével:

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

Ebben a szakaszban leírt mindent, amire szüksége tooknow, amikor az üzenetek események küldése és fogadása hello **szerializáló** könyvtár. Mielőtt továbblép, most is beállíthat néhány paraméter, a modell mekkora szabályozó foglalkozik.

## <a name="macro-configuration"></a>A makróban konfiguráció
Hello használata **szerializáló** könyvtár hello SDK toobe tudomást fontos része megtalálható hello azure-c-megosztott-segédprogram kódtára.
Ha rendelkezik klónozott hello Azure iot-sdk--c tárház a Githubról hello – rekurzív lehetőség használatával, majd megtalálja a megosztott segédprogram kódtára itt:

```
.\\c-utility
```

Ha nem rendelkezik klónozott hello könyvtár, megtalálhatja [Itt](https://github.com/Azure/azure-c-shared-utility).

Hello megosztott segédprogram kódtára, belül található a következő mappa hello:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Ez a mappa tartalmaz, amely a Visual Studio megoldást **makró\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Ebben a megoldásban hello program állít elő, hello **makró\_utils.h** fájlt. Nincs alapértelmezett makró\_hello SDK utils.h fájl. Ez a megoldás lehetővé teszi bizonyos paraméterek, és ezután hozza létre újra hello ezen paraméterek alapján fejlécfájlt toomodify.

az érintett toobe hello két fő paraméterek **nArithmetic** és **nMacroParameters** amely makró található két sort definiált\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Ezek az értékek hello alapértelmezett paraméterek hello SDK része. Minden paraméternek hello jelentése a következő:

* nMacroParameters – meghatározza, hány paraméterek egy DECLARE lehet\_MODELL makró definíciója.
* nArithmetic – vezérlők hello tagok összes száma a modellben engedélyezett.

Ezek a paraméterek számára fontos hello oka, mivel azok vezérelni hogyan nagy is lehet a modellben. Vegyük példaként a modell definíciója:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Ahogy korábban említettük **DECLARE\_MODELL** csak egy C makró. hello modell és hello hello **WITH\_adatok** utasítás (még egy másik makró) paraméterei **DECLARE\_MODELL**. **nMacroParameters** határozza meg, hány paraméterek tartalmazhat **DECLARE\_MODELL**. Hatékonyan hogy hány adatok esemény és művelet nyilatkozatok, akkor is határozza meg. És a hello alapértelmezett korlátja 124 Ez azt jelenti, hogy definiálhat egy modell készül 60 műveletek és adatok események kombinációjával. Ha tooexceed ezt a határt, kapni fog a hely hasonló toothis fordítóprogram-hibákkal:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

Hello **nArithmetic** többet az az alkalmazás belső működését hello hello makró nyelvi paraméter.  Azt szabályozza, hogy is szerepelhet a modell tagok összes száma hello beleértve **DECLARE_STRUCT** makrókat. Ha például a fordítóprogram-hibákkal jelenítse, akkor kell próbálja növelni **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Ha azt szeretné, toochange ezeket a paramétereket, módosítsa a hello értékeket hello makróban\_utils.tt fájl, a recompile hello makró\_utils\_h\_generator.sln megoldás, és futtassa hello lefordított program. Ha így tesz, így új makró\_utils.h fájl jön létre, ezért hello helyezett.\\ közös\\inc könyvtár.

Rendelés toouse hello új verziójában makró\_utils.h, eltávolítás hello **szerializáló** NuGet-csomagot a megoldás, és helyette tartalmaznak hello **szerializáló** Visual Studio-projekt. Ez lehetővé teszi, hogy a kód toocompile hello forráskódját hello szerializáló könyvtár ellen. Ez magában foglalja a frissített hello makró\_utils.h. Ha azt szeretné, toodo ezt **simplesample\_amqp**, első lépésként hello megoldás NuGet-csomag hello hello szerializáló szalagtár eltávolítása:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Majd adja hozzá a projekt tooyour Visual Studio megoldás:

> . \\c\\szerializáló\\build\\windows\\serializer.vcxproj
> 
> 

Amikor elkészült, a megoldás így kell kinéznie:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Ha fordítása a megoldás hello makró frissítése most\_utils.h a bináris tartalmazza.

Vegye figyelembe, hogy ezeket az értékeket elég magas növelése lépheti túl a fordítóprogram korlátok. toothis mutasson, hello **nMacroParameters** hello fő paraméter mely érintett toobe van. hello C99 spec határozza meg, hogy 127 paraméterek közül legalább egy makró definition engedélyezettek. hello Microsoft fordító hello spec pontosan követi (és a maximális hossza 127), ezért nem fogja tudni tooincrease **nMacroParameters** hello alapértelmezett felett. Más compilers – előfordulhat, hogy engedélyezi toodo így (például hello GNU fordító magasabb határérték támogatja).

Eddig azt már szabályozott szinte minden tooknow kapcsolatos hogyan toowrite hello a kód szükséges **szerializáló** könyvtár. Mielőtt áthaladva addig tart, most le újra az előző cikket, esetleg kíváncsi kapcsolatos témaköröket.

## <a name="hello-lower-level-apis"></a>hello alacsonyabb szintű API-k
Ez a cikk arra irányul hello mintaalkalmazás **simplesample\_amqp**. Ez a minta hello magasabb szintű (hello nem "r") API-k toosend eseményeket használ, és üzeneteket fogadni. Ezen API-k használatakor a háttérszálon fut, amely gondoskodik az események üzenetek küldése és fogadása. Azonban hello alacsonyabb szintű (r) API-k tooeliminate használja a háttérszálon, és ha küldi az eseményeket, vagy-üzeneteket fogadjon hello felhő explicit vezérlését.

A egy [előző cikkben](iot-hub-device-sdk-c-iothubclient.md), nincs olyan hello áll funkciók összessége magasabb szintű API-kat:

* IoTHubClient\_CreateFromConnectionString
* IoTHubClient\_SendEventAsync
* IoTHubClient\_SetMessageCallback
* IoTHubClient\_megszüntetése

Az ezen API-k egy **simplesample\_amqp**.

Szerepel továbbá egy hasonló alacsonyabb szintű API-készlet.

* IoTHubClient\_inden\_CreateFromConnectionString
* IoTHubClient\_inden\_SendEventAsync
* IoTHubClient\_inden\_SetMessageCallback
* IoTHubClient\_inden\_megszüntetése

Vegye figyelembe, hogy hello alacsonyabb szintű API-k munkahelyi pontosan hello azonos módon hello előző cikkeiben. Hello első API-készlet is használhatja, ha azt szeretné, hogy a háttér szál toohandle üzenetek események küldése és fogadása. Ha azt szeretné, hogy explicit szabályozhatják, amikor Ön adatokat küldeni és fogadni az IoT-központ használhatja hello második API-készlet. API-k munkahelyi vagy készletét a hello is egyaránt jól **szerializáló** könyvtár.

A példa bemutatja, hogyan hello alacsonyabb szintű API-k használt hello **szerializáló** könyvtár, lásd: hello **simplesample\_http** alkalmazás.

## <a name="additional-topics"></a>További témakörök
Érdemes megemlíteni néhány más témakörök újra szolgálnak tulajdonság kezelési, alternatív hitelesítő adatai és konfigurációs lehetőség használatával. Ezek a témakörök ismertetett egy [előző cikkben](iot-hub-device-sdk-c-iothubclient.md). hello fő pont, hogy ezek a funkciók működni hello azonos módon a hello **szerializáló** könyvtár, mint a hello **IoTHubClient** könyvtárban. Például, ha tooattach tulajdonságok tooan esemény a modellből, akkor használja a **IoTHubMessage\_tulajdonságok** és **térkép**\_**AddorUpdate**, hasonlóan a fentiekben ismertetett hello:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

E hello esemény amelyből létre lett hozva hello **szerializáló** könyvtárban vagy hello segítségével manuálisan létrehozott **IoTHubClient** könyvtár nem számít.

Hello a alternatív hitelesítő adatai, használatával **IoTHubClient\_inden\_létrehozása** is működik, **IoTHubClient\_CreateFromConnectionString** a rendeljen egy **IOT HUBBAL\_ügyfél\_KEZELNI**.

Végezetül hello használata **szerializáló** könyvtár, állíthatja be a konfigurációs beállítások **IoTHubClient\_inden\_SetOption** csak úgy, ahogy hello használatakor**IoTHubClient** könyvtárban.

Egy szolgáltatás, amely egyedi toohello **szerializáló** könyvtárban hello inicializálási API-k. Hello könyvtár használata előtt meg kell hívnia **szerializáló\_init**:

```
serializer_init(NULL);
```

Ez történik, mielőtt meghívja a **IoTHubClient\_CreateFromConnectionString**.

Hasonlóképpen, ha elkészült hello szalagtár használata esetén az túl van hello utolsó hívás meg kell nyitnia a**szerializáló\_deinit**:

```
serializer_deinit();
```

Ellenkező esetben az összes hello más a fent felsorolt szolgáltatások munkahelyi hello ugyanaz a hello **szerializáló** könyvtár mint hello **IoTHubClient** könyvtárban. Az e témakörökkel kapcsolatos további információkért lásd: hello [előző cikkben](iot-hub-device-sdk-c-iothubclient.md) a sorozat.

## <a name="next-steps"></a>Következő lépések
Ez a cikk ismerteti a részletes hello egyedi szempontjait hello **szerializáló** hello szereplő könyvtár **C-hez készült SDK Azure IoT-eszközök**. Hello információk az alapos megértése, hogyan toouse modellek toosend események és üzenetek fogadása az IoT-központ.

Ennyi lenne hello háromrészes series hogyan toodevelop alkalmazások hello **C-hez készült SDK Azure IoT-eszközök**. Ez a kell elegendő információt toonot csak get használatba lehet, de alapos ismerete, hogyan hello API-k biztosítanak. További információkért nincsenek néhány minták hello SDK nem vonatkozik. Ellenkező esetben hello [SDK-dokumentáció](https://github.com/Azure/azure-iot-sdk-c) helyes erőforrás további információt.

az IoT-központ fejlesztésével kapcsolatos további toolearn lásd: hello [Azure IoT SDK-k][lnk-sdks].

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
