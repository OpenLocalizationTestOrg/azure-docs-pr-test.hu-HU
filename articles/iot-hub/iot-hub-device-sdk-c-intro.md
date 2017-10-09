---
title: "aaaThe Azure IoT-eszközök SDK C-hez |} Microsoft Docs"
description: "Ismerkedés a hello Azure IoT-eszközök SDK C-hez, és megtudhatja, hogyan toocreate eszközeinek alkalmazásait, amely kommunikálni az IoT-központ."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: e448b061-6bdd-470a-a527-15ec03cca7b9
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: obloch
ms.openlocfilehash: 9e20742e6ea513c124bfaf28f02f6fba86170daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c"></a>Az Azure IoT-eszközök SDK C-hez

Hello **Azure IoT-eszközök SDK** szalagtárak készlete tervezett toosimplify hello folyamat tooand fogadó üzenetek üzenetet küldeni a hello **Azure IoT Hub** szolgáltatás. SDK-t és egy adott platform célzó hello eltérő változata van, de ez a cikk ismerteti a hello **C-hez készült SDK Azure IoT-eszközök**.

hello Azure IoT-eszközök SDK c ANSI C (C99) toomaximize hordozhatósága nyelven van megírva. A szolgáltatás révén hello szalagtárak jól alkalmazható toooperate több platformokon és eszközökön, különösen akkor, ha minimalizálja a lemez és a memóriaigény prioritását.

Nincsenek mely hello az SDK-t tesztelték platformok széles körének (lásd: hello [Azure IoT eszköz katalógus hitelesített](https://catalog.azureiotsuite.com/) részletekért). Bár ez a cikk a Windows hello platformon futó mintakód forgatókönyvek tartalmaz, akkor hello kód cikkben leírt megegyezik hello támogatott platformok tartományán keresztül.

Ez a cikk bemutatja a toohello architektúra az hello Azure IoT-eszközök SDK a c kiszolgálóra. Azt mutatja be, hogyan tooinitialize hello hálózatieszköz-könyvtár adatok tooIoT Hub, üzeneteket küldjön és fogadjon abból. a cikkben szereplő információkat hello legyen elég tooget hello SDK használatának, de hello szalagtárak mutatók tooadditional adatait is tartalmazza.

## <a name="sdk-architecture"></a>SDK-architektúra

Hello található [ **C-hez készült SDK Azure IoT-eszközök** ](https://github.com/Azure/azure-iot-sdk-c) hello API hello a GitHub tárház és nézet adatai [C API-referencia](https://azure.github.io/azure-iot-sdk-c/index.html).

hello hello könyvtárak legújabb verziója található hello **fő** hello tárház főágába:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

* hello hello SDK alapvető végrehajtásának van hello **IOT hubbal\_ügyfél** hello legalacsonyabb API réteg hello SDK hello végrehajtásának tartalmazó mappa: hello **IoTHubClient** könyvtárban. Hello **IoTHubClient** könyvtár végrehajtási üzenetek tooIoT Hub üzenetek küldése és fogadása az IoT-központ az üzenetküldési nyers API-kat tartalmaz. Ha ezt a szalagtárat, üzenet szerializálási végrehajtásáért felelős, de egyéb adatainak kommunikál az IoT-központ történik meg.
* Hello **szerializáló** mappája súgófunkciókat és minták, amelyek bemutatják, hogyan tooserialize adatok küldése a tooAzure IoT Hub használata előtt hello ügyféloldali kódtára tartalmaz. hello szerializáló hello használata nem kötelező, és a könnyebb biztosítja. toouse hello **szerializáló** könyvtár, egy modell, amely meghatározza az adatok toosend tooIoT Hub és hello köszönőüzenetei tooreceive származó várt megadása. Miután hello modell van definiálva, hello SDK használható biztosít, amely lehetővé teszi API felületéhez tooeasily az eszközről a felhőbe, és anélkül, hogy a felhő eszközre üzenetek hello szerializálási részleteit. hello könyvtár más nyílt forráskódú tárak, amelyek megvalósítják az átviteli protokollal például MQTT és AMQP függ.
* Hello **IoTHubClient** könyvtár más nyílt forráskódú szalagtárak függ:
  * Hello [Azure C megosztott segédprogram](https://github.com/Azure/azure-c-shared-utility) könyvtárban, amely több Azure-hoz kapcsolódó C SDK között szükséges alapvető feladatokhoz (például karakterláncok, lista manipuláció és IO) közös funkciókat biztosít.
  * Hello [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) könyvtárban, és azt egy AMQP korlátozott erőforrás eszközök optimalizálva ügyféloldali megvalósítása.
  * Hello [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) könyvtárba, amely egy általános célú függvénytár végrehajtási hello MQTT protokoll, és korlátozott erőforrás eszközök optimalizálva.

Ezek a könyvtárak használata könnyebb toounderstand kódpéldákat megnézzük. a következő szakaszok hello végigvezetik Önt hello alkalmazásokra, amelyek szerepelnek a hello SDK számos. Ez a forgatókönyv adjon meg helyes érzi, hogy a hello hello architekturális rétegek hello SDK és az API-k működnek bemutatása toohow hello különböző képességeire.

## <a name="before-you-run-hello-samples"></a>Mielőtt újra lefuttatja hello minták

Hello Azure IoT-eszközök SDK hello minták c futtathat, előtt [hello IoT-központ szolgáltatás példányának létrehozása](iot-hub-create-through-portal.md) az Azure-előfizetésben. Fejezze be a következő feladatok hello:

* A fejlesztőkörnyezet előkészítése
* Szerezze be a hitelesítő adatai.

### <a name="prepare-your-development-environment"></a>A fejlesztőkörnyezet előkészítése

Csomagok (például NuGet Windows vagy apt_get Debian és Ubuntu) közös platformok kapnak, és hello minták használja ezeket a csomagokat, ha elérhető. Bizonyos esetekben kell toocompile hello SDK számára, vagy az eszközön. Ha toocompile hello SDK van szüksége, tekintse meg [a fejlesztőkörnyezet előkészítése](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) hello GitHub-tárházban.

tooobtain hello alkalmazás kódmintában letöltési hello SDK a Githubon verzióját. Hello forrás példányának lekérése hello **fő** hello ága [GitHub-tárházban](https://github.com/Azure/azure-iot-sdk-c).


### <a name="obtain-hello-device-credentials"></a>Hello eszköz hitelesítő adatok beszerzése

Most, hogy hello minta forráskódját, hello következő lépésként toodo tooget eszköz hitelesítő adatokat. Egy eszköz toobe képes tooaccess az IoT-központ, a hozzá kell adnia hello eszköz toohello IoT-központ identitásjegyzékhez. Az eszköz hozzáadásakor kap az hello eszköz toobe képes tooconnect toohello IoT-központ szükséges eszköz hitelesítő adatokat. hello a következő szakaszban tárgyalt hello mintaalkalmazások ezeket a hitelesítő adatokat várt hello formája egy **eszköz kapcsolati karakterlánc**.

Számos nyílt forráskódú eszközök toohelp kezelheti az IoT hub van.

* Egy nevű Windows-alkalmazás [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).
* A platformok közötti node.js parancssori eszköz neve [IOT hubbal-explorer](https://github.com/azure/iothub-explorer).

Ez az oktatóanyag használja grafikus hello *eszköz explorer* eszköz. Is használhatja a hello *IOT hubbal-explorer* eszköze, ha jobban szeret toouse parancssori eszközt.

hello eszköz explorer eszköz különböző funkciókat használ hello Azure IoT szolgáltatás szalagtárak tooperform az IoT-központot, beleértve az eszközök. Hello eszköz explorer eszköz tooadd eszköz használatakor az eszközhöz kapott egy kapcsolati karakterláncot. A kapcsolati karakterlánc toorun hello mintaalkalmazások van szüksége.

Ha nem ismeri a hello eszköz explorer eszközzel, a következő eljárás hello ismerteti hogyan toouse azt tooadd egy eszköz, és szerezze be az eszköz kapcsolati karakterláncot.

tooinstall hello eszköz explorer eszköz, lásd: [toouse hogyan eszköz Explorer hello IoT Hub-eszközöknek](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).

Hello program futtatásakor ez az interfész jelenik meg:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Adja meg a **IoT Hub kapcsolati karakterlánc** hello az első mezőben, majd kattintson **frissítés**. Ez a lépés hello eszköz konfigurálja, hogy kommunikáljon az IoT-központ.

Az IoT-központ kapcsolati karakterlánc hello konfigurálását, kattintson hello **felügyeleti** lapon:

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Ez a lap majdnem, amelyeken kezelheti az IoT hub-ben regisztrált hello eszközök.

Hello kattintva hozhat létre egy eszköz **létrehozása** gombra. Olyan előre megadott kulcsokat (elsődleges és másodlagos) egy párbeszédpanelt jelenít meg. Adjon meg egy **Eszközazonosító** majd **létrehozása**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Hello eszköz létrehozásakor hello eszközök frissítések minden hello regisztrált eszközökkel, például egy újonnan létrehozott hello listában. Az új eszköz a jobb gombbal, az ebben a menüben jelenik meg:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Ha úgy dönt, **másolja a kijelölt eszközre vonatkozóan a kapcsolati karakterlánc**, hello eszköz kapcsolati karakterláncot a vágólapra másolt toohello. Hello eszköz kapcsolati karakterlánc egy példányát megőrizni. Meg kell hello a következő részekben leírt hello mintaalkalmazások futtatásakor.

Hello fenti lépések végrehajtását, hamarosan kész toostart bizonyos kód futtatásával. Mindkét minták állandó rendelkezik, amely lehetővé teszi egy kapcsolati karakterláncot tooenter hello fő forrásfájl hello tetején. Például a megfelelő sor a hello hello **IOT hubbal\_ügyfél\_minta\_mqtt** alkalmazás a következőképpen jelenik meg.

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-hello-iothubclient-library"></a>Hello IoTHubClient könyvtárban

Hello belül **IOT hubbal\_ügyfél** hello mappájában [azure iot-sdk--c](https://github.com/azure/azure-iot-sdk-c) -tárházban, van egy **minták** mappába, amelyben egy alkalmazás nevű **IOT hubbal\_ügyfél\_minta\_mqtt**.

Windows-verzión hello hello **IOT hubbal\_ügyfél\_minta\_mqtt** alkalmazás tartalmazza a Visual Studio megoldás a következő hello:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-mqtt.PNG)

> [!NOTE]
> Ha megnyitja a projektet a Visual Studio 2017, fogadja el a hello kér tooretarget hello projekt toohello legújabb verziójára.

Ez a megoldás egyetlen projektet tartalmaz. Nincsenek telepítve az ebben a megoldásban négy NuGet-csomagok:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.umqtt

Folyamatosan hello **Microsoft.Azure.C.SharedUtility** amikor dolgozunk hello SDK csomagot. Ez a minta hello MQTT protokollt használja, ezért meg kell adni hello **Microsoft.Azure.umqtt** és **Microsoft.Azure.IoTHub.MqttTransport** (nincsenek egyenértékű csomagok AMQP és HTTP csomagok ). Mivel hello mintát használ hello **IoTHubClient** könyvtár, is magában kell foglalnia hello **Microsoft.Azure.IoTHub.IoTHubClient** csomag a megoldásban.

Hello megvalósítási hello mintaalkalmazás hello található **IOT hubbal\_ügyfél\_minta\_mqtt.c** forrásfájl.

hello következő lépések használják a minta alkalmazás toowalk le toouse hello mi szükséges a **IoTHubClient** könyvtárban.

### <a name="initialize-hello-library"></a>Hello kódtár inicializálása

> [!NOTE]
> Mielőtt az hello szalagtárak használatának megkezdése, szükséges tooperform néhány platform-specifikus inicializálása. Ha azt tervezi, toouse AMQP Linux például kell hello OpenSSL kódtár inicializálása. hello minták hello [GitHub-tárházban](https://github.com/Azure/azure-iot-sdk-c) hello segédprogram függvény **platform\_init** amikor hello ügyfél elindul, és hívja meg hello **platform\_deinit**  függvény való kilépés előtt. Ezek a funkciók hello platform.h fejlécfájlt van deklarálva. Vizsgálja meg ezeket a funkciókat a célplatformot a hello hello definíciók [tárház](https://github.com/Azure/azure-iot-sdk-c) toodetermine e szükséges tooinclude bármely platform-specifikus inicializálási kód az ügyfél.

hello könyvtárakkal toostart először lefoglalni egy IoT-központ ügyfél leíró:

```c
if ((iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

Hello eszköz kapcsolati karakterláncot kapott, hello eszköz explorer eszköz toothis függvény másolatát át. Is megjelölhetők hello kommunikációs protokollt toouse. Ez a példa MQTT, de amqp-t és HTTP is lehetősége.

Ha rendelkezik olyan érvényes **IOT HUBBAL\_ügyfél\_KEZELNI**, hívja az API-k toosend hello elindíthatja és tooand üzenetek fogadása az IoT-központ.

### <a name="send-messages"></a>Üzenetek küldése

hello mintaalkalmazás állít be egy hurok toosend üzenetek tooyour IoT-központot. a következő kódrészletet hello:

- Létrehoz egy üzenetet.
- Hozzáad egy tulajdonság toohello üzenetet.
- Üzenet küldése.

Először hozzon létre egy üzenetet:

```c
size_t iterator = 0;
do
{
    if (iterator < MESSAGE_COUNT)
    {
        sprintf_s(msgText, sizeof(msgText), "{\"deviceId\":\"myFirstDevice\",\"windSpeed\":%.2f}", avgWindSpeed + (rand() % 4 + 2));
        if ((messages[iterator].messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText))) == NULL)
        {
            (void)printf("ERROR: iotHubMessageHandle is NULL!\r\n");
        }
        else
        {
            messages[iterator].messageTrackingId = iterator;
            MAP_HANDLE propMap = IoTHubMessage_Properties(messages[iterator].messageHandle);
            (void)sprintf_s(propText, sizeof(propText), "PropMsg_%zu", iterator);
            if (Map_AddOrUpdate(propMap, "PropName", propText) != MAP_OK)
            {
                (void)printf("ERROR: Map_AddOrUpdate Failed!\r\n");
            }

            if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messages[iterator].messageHandle, SendConfirmationCallback, &messages[iterator]) != IOTHUB_CLIENT_OK)
            {
                (void)printf("ERROR: IoTHubClient_LL_SendEventAsync..........FAILED!\r\n");
            }
            else
            {
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission tooIoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

Minden alkalommal, amikor egy üzenetet küld, meg kell adnia egy referencia tooa visszahívási függvény hello adatküldést hív meg. Ebben a példában a hello visszahívási függvény hívása esetén **SendConfirmationCallback**. a következő kódrészletet hello a visszahívási függvény jeleníti meg:

```c
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %zu with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Vegye figyelembe a hello hívás toohello **IoTHubMessage\_Destroy** működik, ha üdvözlőüzenetére végzett. Ez a funkció ennyi helyet szabadít hello erőforrások lefoglalt üdvözlőüzenetére létrehozásakor.

### <a name="receive-messages"></a>Hibaüzenetek

Egy üzenet fogadását egy aszinkron művelet. Először hello visszahívási tooinvoke regisztrálása, amikor hello eszköz üzenetet kapja:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext) != IOTHUB_CLIENT_OK)
{
    (void)printf("ERROR: IoTHubClient_LL_SetMessageCallback..........FAILED!\r\n");
}
else
{
    (void)printf("IoTHubClient_LL_SetMessageCallback...successful.\r\n");
...
```

hello utolsó paraméter értéke a "void" mutató toowhatever szeretné. Hello mintát, az egész számnak mutató tooan is, de ez lehet egy mutató tooa összetettebb adatstruktúra. Ez a paraméter lehetővé teszi, hogy a hello visszahívási függvény toooperate a függvény hello hívó rendelkező megosztott állapot.

Hello eszköz üzenetet kap, amikor hello regisztrált visszahívási függvényt hívják. A visszahívási függvény kéri le:

* hello üzenetazonosítója és hello üzenet korrelációs azonosítója.
* hello üzenet tartalma.
* Egyéni tulajdonságok hello üzenetből.

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    MAP_HANDLE mapProperties;
    const char* messageId;
    const char* correlationId;

    // Message properties
    if ((messageId = IoTHubMessage_GetMessageId(message)) == NULL)
    {
        messageId = "<null>";
    }

    if ((correlationId = IoTHubMessage_GetCorrelationId(message)) == NULL)
    {
        correlationId = "<null>";
    }

    // Message content
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        (void)printf("unable tooretrieve hello message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive hello work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from hello message
    mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                size_t index;

                printf(" Message Properties:\r\n");
                for (index = 0; index < propertyCount; index++)
                {
                    (void)printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                (void)printf("\r\n");
            }
        }
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Használjon hello **IoTHubMessage\_GetByteArray** függvény tooretrieve hello üzenet, amely ebben a példában a karakterlánc.

### <a name="uninitialize-hello-library"></a>Meghívná hello könyvtár

Ha elkészült a küldő események, és fogadja az üzeneteket, hello IoT könyvtár is meghívná. toodo Igen, a következő függvény hívásához szükséges hello adja ki:

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

A hívás szabaddá korábban le van foglalva hello hello erőforrások **IoTHubClient\_CreateFromConnectionString** függvény.

Ahogy látja, könnyen toosend és üzenetek fogadása az hello **IoTHubClient** könyvtárban. hello könyvtár kezeli az IoT-központot, mely protokoll toouse beleértve folytatott kommunikáció hello részleteit (hello fejlesztői hello szempontjából, ez a lehetőség egy egyszerű konfiguráció).

Hello **IoTHubClient** kódtár is biztosít a pontos szabályozhatják, hogyan tooserialize hello adatok az eszköz küld tooIoT központ. Egyes esetekben ez az érték előnyös, de az mások számára nem szeretné, hogy az érintett toobe egy megvalósítási részletek is. Ha hello esetben érdemes, a hello **szerializáló** hello a következő szakaszban ismertetett könyvtárban.

## <a name="use-hello-serializer-library"></a>Hello szerializáló könyvtárban

Fogalmilag hello **szerializáló** könyvtárban található fölött hello **IoTHubClient** hello SDK szalagtár. Hello használ **IoTHubClient** könyvtár a kommunikáció az IoT-központ, de a mögöttes hello hello terheket üzenet szerializálási való eltávolításához hello fejlesztőtől származó modellezési lényeges képességét biztosítja. Hogyan a szalagtár works legjobb bemutatott példa szerint.

Belső hello **szerializáló** hello mappájában [azure iot-sdk--c tárház](https://github.com/Azure/azure-iot-sdk-c), van egy **minták** nevű mappát, amely tartalmazza az alkalmazás **simplesample \_mqtt**. Ez a minta hello Windows verzió magában foglalja a Visual Studio megoldás a következő hello:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_mqtt.PNG)

> [!NOTE]
> Ha megnyitja a projektet a Visual Studio 2017, fogadja el a hello kér tooretarget hello projekt toohello legújabb verziójára.

Csakúgy, mint a korábbi minta hello, ez egy több NuGet-csomagot tartalmazza:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.IoTHub.Serializer
* Microsoft.Azure.umqtt

Most láthatta, ezeket a csomagokat hello korábbi minta a legtöbb, de **Microsoft.Azure.IoTHub.Serializer** új. Ez a csomag szükséges használata esetén hello **szerializáló** könyvtár.

Hello mintaalkalmazás hello végrehajtásának hello található **simplesample\_mqtt.c** fájlt.

hello alábbiakban ismerteti a minta hello kulcs részei között.

### <a name="initialize-hello-library"></a>Hello kódtár inicializálása

hello használata toostart **szerializáló** könyvtár, hívás hello inicializálási API-kat:

```c
if (serializer_init(NULL) != SERIALIZER_OK)
{
    (void)printf("Failed on serializer_init\r\n");
}
else
{
    IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol);
    srand((unsigned int)time(NULL));
    int avgWindSpeed = 10;

    if (iotHubClientHandle == NULL)
    {
        (void)printf("Failed on IoTHubClient_LL_Create\r\n");
    }
    else
    {
        ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
        if (myWeather == NULL)
        {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
        }
        else
        {
...
```

hello hívás toohello **szerializáló\_init** függvény egyszeri hívás és inicializál hello alapul szolgáló könyvtár. Ezután meghívja a hello **IoTHubClient\_inden\_CreateFromConnectionString** függvénynek, amely van hello hello hasonlóan azonos API **IoTHubClient** minta. Ez a hívás beállítja az eszköz kapcsolati karakterlánc (Ez a hívás is, ha úgy dönt, hogy hello protokoll toouse szeretné). Ez a minta hello átvitelhez MQTT használ, de AMQP vagy HTTP.

Végezetül hívás hello **létrehozása\_MODELL\_példány** függvény. **WeatherStation** hello névtér hello modell és **ContosoAnemometer** hello modell hello neve van. Hello modell példány létrehozása után is használhatja az üzenetek toostart küldése és fogadása. Azt azonban fontos toounderstand milyen modell.

### <a name="define-hello-model"></a>Hello modellt

Egy modell a(z) hello **szerializáló** könyvtár határozza meg, amely az eszköz küld tooIoT Hub és hello nevezett üzenetváltással kezdődnek, köszönőüzenetei *műveletek* hello modellezési nyelv, amely fogadja a. A modellek, mint hello C makrók használatával meghatározásához **simplesample\_mqtt** mintaalkalmazást:

```c
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(int, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Hello **MEGKEZDÉSÉHEZ\_NÉVTÉR** és **END\_NÉVTÉR** mindkét makrók hello névtér hello modell argumentumként igénybe vehet. Valószínű, hogy semmit, ezek a makrók között-e a modell vagy a modellek és a hello adatstruktúrák hello modellek használó hello meghatározása.

Ebben a példában nincs nevű egyetlen modellt **ContosoAnemometer**. Ez a modell határozza meg, hogy az eszköz küldhet tooIoT Hub adatok kétféle: **DeviceId** és **szélsebesség**. Az eszköz megkaphatja három műveleteket (üzenet) is definiálja: **TurnFanOn**, **TurnFanOff**, és **SetAirResistance**. Minden adatelemre típussal rendelkezik, és minden művelet nevét (és nem kötelező paraméterek).

hello adatok és a hello modellben definiált műveletek határozza meg, hogy toosend üzenetek tooIoT Hub használja, és beállíthatja küldött toomessages toohello eszköz válaszára API felületéhez. Ez a modell használatának legjobb értendő egy példán keresztül.

### <a name="send-messages"></a>Üzenetek küldése

hello modell hello adatokat küldhet a központ tooIoT határozza meg. Ebben a példában, amely azt jelenti, hogy egy hello hello segítségével meghatározott két adatelemek **WITH_DATA** makró. Van több lépéseket szükséges toosend **DeviceId** és **szélsebesség** értékek tooan IoT-központot. hello először tooset hello adatokat toosend:

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

hello korábban meghatározott modell lehetővé teszi tooset hello értékek úgy, hogy tagjai egy **struct**. A következő szerializálni kívánt toosend üdvözlőüzenetére:

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed tooserialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

Ez a kód rendezi sorba hello eszközről a felhőbe tooa puffer (által hivatkozott **cél**). hello kódot, majd meghívja hello **sendMessage** toosend hello üzenet tooIoT Központ működéséhez:

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable toocreate a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed toohand over hello message tooIoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted hello message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```


második toolast paramétere hello **IoTHubClient\_inden\_SendEventAsync** hivatkozás tooa visszahívási függvény nevezzük, amikor hello adatok elküldése megtörtént. Íme hello visszahívási függvény hello mintában:

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

hello második paramétere mutató toouser kontextusban; azonos kapott túl hello**IoTHubClient\_inden\_SendEventAsync**. Ebben az esetben hello környezetben egy egyszerű számlálót, de bármilyen lehet.

Ez minden toosending eszközről a felhőbe üzenetek van. hello csak a bal oldali toocover dolog van hogyan tooreceive üzeneteket.

### <a name="receive-messages"></a>Hibaüzenetek

Egy üzenet fogadását működik, hasonlóképpen toohello üzenetek működéséhez a hello **IoTHubClient** könyvtárban. Először regisztrálnia üzenet visszahívási függvény:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable tooIoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

Ezután ír hello visszahívási függvény, amelyet ha egy üzenet jelenik meg:

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable tooIoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed toomalloc\r\n");
            result = IOTHUBMESSAGE_ABANDONED;
        }
        else
        {
            (void)memcpy(temp, buffer, size);
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

Ez a kód bolierplate--azt rendelkezik hello azonos bármely, a megoldás. Ez a funkció hello üzenetet kap, és gondoskodik összegzésére, valamint megfelelő funkciót toohello hello hívás túl**EXECUTE\_parancs**. hello függvény hívása ezen a ponton a modell hello műveletek hello meghatározása függ.

A modell műveletet ad meg, amikor Ön szükséges tooimplement nevezzük, amikor az eszköz megkapja a megfelelő üdvözlőüzenetére függvényt. Ha például a modell határozza meg a műveletet:

```c
WITH_ACTION(SetAirResistance, int, Position)
```

Adja meg a függvény a következő aláírást:

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Vegye figyelembe, hogyan hello hello függvény neve megegyezik hello hello művelet hello modellben és, hogy hello hello függvény paraméterei egyeznek-e a hello hello a művelethez megadott paraméterek közül. hello első paraméter megadása mindig kötelező, és a modell mutató toohello példányát tartalmazza.

Hello eszköz az aláírásnak megfelelő üzenetet kap, hello megfelelő függvény neve. Ezért vezérelt tooinclude hello bolierplate kóddal a rendelkező **IoTHubMessage**, üzenetek fogadására csak egy egyszerű függvény a modellben megadott műveleteket meghatározása.

### <a name="uninitialize-hello-library"></a>Meghívná hello könyvtár

Amikor elkészült adatokat küldő, és fogadja az üzeneteket, hello IoT könyvtár is meghívná:

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Mindegyik funkcióhoz három hello a fentiekben ismertetett három inicializálási funkciók igazodik. Ezen API-k hívása biztosítja, hogy a korábban kiosztott erőforrásokat ingyenes.

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben szereplő hello hello hello tárak használatának alapjaival **C-hez készült SDK Azure IoT-eszközök**. Az rendelkezik külön elegendő információt toounderstand hello SDK, az architektúra, és hogyan tooget hello használata Windows minták tartalmát. hello a következő cikk alapján, amely ismerteti, hogy továbbra is hello SDK hello leírása [további: hello IoTHubClient library](iot-hub-device-sdk-c-iothubclient.md).

az IoT-központ fejlesztésével kapcsolatos további toolearn lásd: hello [Azure IoT SDK-k][lnk-sdks].

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
