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
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Az Azure IoT-eszközök SDK c – további információ az IoTHubClient
Hello [először a következő cikket:](iot-hub-device-sdk-c-intro.md) az a sorozat bevezetett hello **C-hez készült SDK Azure IoT-eszközök**. A cikk alapján, hogy nincsenek-e két architekturális rétegek SDK. Alapszintű hello van hello **IoTHubClient** könyvtárban, amely közvetlenül kezeli az IoT-központ folytatott kommunikáció. Szerepel továbbá hello **szerializáló** , mely a buildek tooprovide szerializálási szolgáltatások. Ebben a cikkben a következőkhöz nyújtunk további részletek a hello **IoTHubClient** könyvtárban.

hello előző cikkben leírt hogyan toouse hello **IoTHubClient** könyvtár toosend események tooIoT központ és az üzenetek fogadásához. Ez a cikk kiterjeszti ezekre a kérdésekre adott válaszokat azáltal, hogy elmagyarázza, hogyan toomore pontosan kezelése *amikor* küldött és fogadott adatok bevezeti a toohello **alacsonyabb szintű API-k**. Azt is ismertetjük, hogyan tooattach tulajdonságok tooevents (és üzenetek lekérése őket) hello szolgáltatások kezelése hello tulajdonsággal **IoTHubClient** könyvtárban. Végezetül, a következőkhöz nyújtunk különböző módokon toohandle további magyarázata IoT-központ érkezett.

hello cikk arra a következtetésre jut vegyes témaköreit, beleértve az bővebben a hitelesítő adatai, és hogyan toochange hello hello működésének néhány kiterjed **IoTHubClient** konfigurációs lehetőségek között.

Hello fogjuk használni **IoTHubClient** SDK-minták tooexplain ezeket a témaköröket. Ha azt szeretné, hogy mentén toofollow, tekintse meg a hello **IOT hubbal\_ügyfél\_minta\_http** és **IOT hubbal\_ügyfél\_minta\_amqp** hello Azure IoT-eszközök SDK szereplő minden c hello a következő részekben leírt mutatják be ezeket a mintákat az alkalmazások.

Hello található [ **C-hez készült SDK Azure IoT-eszközök** ](https://github.com/Azure/azure-iot-sdk-c) hello API hello a GitHub tárház és nézet adatai [C API-referencia](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="hello-lower-level-apis"></a>hello alacsonyabb szintű API-k
hello előző cikkben leírt hello alapszintű hello működésének **IotHubClient** belül hello hello kontextusában **IOT hubbal\_ügyfél\_minta\_amqp** az alkalmazás. Például azt, hogyan tooinitialize hello könyvtár használja ezt a kódot.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Azt is ismerteti, hogyan függvényhívás használatát az toosend események.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

hello a cikk azt is ismerteti, hogyan tooreceive üzenetek, ha regisztrálja a visszahívási függvény.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

hello cikket is bemutatta, hogyan toofree-erőforrások hello alábbi kód.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Vannak azonban olyan API-e kiegészítő funkciók tooeach:

* IoTHubClient\_inden\_CreateFromConnectionString
* IoTHubClient\_inden\_SendEventAsync
* IoTHubClient\_inden\_SetMessageCallback
* IoTHubClient\_inden\_megszüntetése

Ezek a függvények minden "R" foglalandó hello API-név. Eltérő hello ezek paraméterei azonos tootheir nem inden megfelelők esetében. Azonban hello ezek a funkciók működése eltérő módon, egy fontos.

A hívás esetén **IoTHubClient\_CreateFromConnectionString**, alapul szolgáló szalagtárak hello hello háttérben futó új szálat létrehozni. Ebből a szálból küldi az eseményeket, és az IoT-központ fogadja az üzeneteket. Nincs ilyen szálhoz létrehoz az hello "R" API-k használatakor. hello háttérszál hello létrehozását a fejlesztők kényelme toohello. Nincs tooworry kapcsolatos explicit módon események üzenetek küldése és fogadása az IoT Hub – hello háttérben automatikusan történik. Ezzel szemben hello "R" API-k biztosítanak explicit ellenőrzése alatt tartja a kommunikáció a IoT-központot, ha esetleg szükség lenne rá.

toounderstand a megfelelőbb nézzük például:

A hívás esetén **IoTHubClient\_SendEventAsync**, mi ténylegesen végzett helyezi hello esemény a pufferben. létre, ha meghívja a háttérben futó szál hello **IoTHubClient\_CreateFromConnectionString** folyamatosan figyeli az adott puffer és adatokat küld, hogy az tartalmazza-e tooIoT központ. Ez akkor fordul elő: hello hello háttérben azonos ideje, hogy hello fő szálnak működik-e más feladatok.

Hasonlóképpen, az üzenetek visszahívási függvény regisztrálásakor **IoTHubClient\_SetMessageCallback**, hello SDK toohave hello háttér most utasítja szál a hello visszahívási függvény meghívása az üzenet esetén fogadott, független hello fő szálnak.

hello "R" API-k ne hozzon létre egy háttérszálon. Ehelyett egy olyan új API tooexplicitly küldési kell meghívni, és fogadhat adatokat az IoT-központ. Ezt mutatják be a következő példa hello.

Hello **IOT hubbal\_ügyfél\_minta\_http** alkalmazás, amely azt mutatja be, SDK hello megtalálható hello alacsonyabb szintű API-k. A mintában szereplő küldünk események tooIoT Hub például hello következő kóddal:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

hello első három sorok hello üzenet létrehozása, és hello utolsó sora küldi hello esemény. Azonban amint azt korábban említettük, "küldése a" hello esemény azt jelenti, hogy egyszerűen hello adatok kerülnek a pufferben. Nem történik adatátvitel hello hálózaton, hívása **IoTHubClient\_inden\_SendEventAsync**. A sorrend tooactually érkező hello adatok tooIoT Hub, meg kell hívnia **IoTHubClient\_inden\_DoWork**, a példában szereplő:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Ez a kód (a hello **IOT hubbal\_ügyfél\_minta\_http** alkalmazás) egymás után többször hívja **IoTHubClient\_inden\_DoWork** . Minden alkalommal, amikor **IoTHubClient\_inden\_DoWork** van neve, az egyes események küldi hello puffer tooIoT Hub, és lekérdezi a egy aszinkron küldés alatt álló toohello eszköz. hello ez utóbbi esetben azt jelenti, hogy ha a jelenleg regisztrált visszahívási függvény üzenetek, majd hello visszahívási meghívták (feltéve, hogy minden üzenet sorba vannak). Azt kellene regisztrált visszahívási függvény például hello következő kóddal:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

hello OK, amely **IoTHubClient\_inden\_DoWork** gyakran nevezik hurok, amely minden alkalommal, amikor azt nevezzük, elküldi *néhány* pufferelt eseményeket tooIoT Hub és lekéri *következő hello* üzenet sorba hello eszközhöz. Minden hívás nem garantált toosend összes pufferelt eseményeket vagy tooretrieve összes sorba állított üzenetek. Ha azt szeretné, hogy toosend események kerülnek egy hello puffer, és folytassa a más feldolgozás a lecserélheti a hurok hello alábbi kódot:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Ez a kód **IoTHubClient\_inden\_DoWork** amíg hello pufferben lévő összes esemény elküldött tooIoT központ. Megjegyzés: Ez nem feltétlenül is jelenti, hogy az összes várólistára helyezett üzenetek fogadott. Ennek oka hello részét képezi, hogy "all" üzenetek ellenőrzése a nem a determinisztikus művelet. Mi történik, ha visszaállíthatja az "összes" köszönőüzenetei, de majd egy másikat toohello eszköz küldött után azonnal? A jobb módon toodeal vele programozott időtúllépés jelenti. Például hello üzenet visszahívási függvény sikerült alaphelyzetbe állítani egy számlálót minden alkalommal, amikor meghívták. Majd írhat logika toocontinue feldolgozási, ha például nem érkeztek üzenetek hello a legutóbbi *X* másodperc.

Ha végzett ingressing események van, és fogadja az üzeneteket, lehet, hogy toocall hello megfelelő függvény tooclean erőforrásait.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Alapvetően az API-k toosend csak egy készletét, és az API-t hello nélkül hello háttérszál ugyanaz a háttérszálon és más adatok fogadására. Nagy mennyiségű fejlesztők számára előnyös lehet hello nem - inden API-k, de hello alacsonyabb szintű API-k akkor hasznos, ha hello fejlesztői szeretne rendelni a hálózati átvitelt explicit vezérelheti. Például bizonyos eszközök begyűjtik az adatokat idő és a csak érkező jelzések (például óra egy alkalommal vagy naponta egyszer) meghatározott időközönként. alacsonyabb szintű API-kat biztosít lehetőséget tooexplicitly vezérlő hello, amikor Ön adatokat küldeni és fogadni az IoT-központ hello. Mások egyszerűen, főként a hello egyszerűség érdekében, hogy alacsonyabb szintű API-k olyan hello. Minden egyes munka azonban hello háttérben helyett hello fő szálnak történik.

Bármelyik modellt választja, lehet, hogy toobe konzisztens mely API-kat használ. Ha először meghívásával **IoTHubClient\_inden\_CreateFromConnectionString**, alacsonyabb szintű API-khoz, követési munka megfelelő hello csak használnia kell:

* IoTHubClient\_inden\_SendEventAsync
* IoTHubClient\_inden\_SetMessageCallback
* IoTHubClient\_inden\_megszüntetése
* IoTHubClient\_inden\_DoWork

Ellenkező hello is igaz. Ha először **IoTHubClient\_CreateFromConnectionString**, akkor használjon hello nem - inden API-k minden további feldolgozásra.

Hello Azure IoT-eszközök SDK C-hez, a következő látható: hello **IOT hubbal\_ügyfél\_minta\_http** hello átfogó példát alkalmazás alacsonyabb szintű API-k. Hello **IOT hubbal\_ügyfél\_minta\_amqp** alkalmazás teljes például a hello nem - inden API-k lehet hivatkozni.

## <a name="property-handling"></a>Tulajdonság kezelése
Amennyiben azt korábban leírt adatokat küldő, ha azt már lett utaló toohello hello üzenet törzsét. Tegyük fel ezt a kódot:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Ez a példa küld egy üzenet tooIoT Hub hello szövegre "Hello World". Azonban az IoT-központ lehetővé teszi tulajdonságok toobe csatolt tooeach üzenet. Tulajdonságok olyan név/érték párok, amelyek csatlakoztatott toohello üzenet lehetnek. Módosíthatja például a Microsoft hello előző kód tooattach tulajdonság toohello üzenet:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Először meghívásával **IoTHubMessage\_tulajdonságok** , és átadja azt a üzenet hello leíróját. Mi azt vissza van egy **térkép\_KEZELNI** hivatkozás, amely lehetővé teszi velünk toostart tulajdonságokat. hívása úgy érhető el, ez utóbbi hello **térkép\_AddOrUpdate**, amely fogad egy hivatkozás tooa térkép\_leíró hello tulajdonságnév és hello tulajdonság értéke. Ez az API-t hozzá lehessen adni annyi tulajdonságai, például azt.

Ha hello esemény olvasni **Event Hubs**, hello fogadó hello tulajdonságlistázás, és a hozzájuk tartozó értékek beolvasása. Például a .NET ez lenne elvégezhető hello elérésével [tulajdonsággyűjteményében hello EventData objektum](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

Hello előző példában csatol azt, hogy küldünk tooIoT Hub tulajdonságainak tooan esemény. Tulajdonságok is csatolt toomessages IoT-központ kapott. Tooretrieve tulajdonságok üzenetből szeretnénk, ha azt az üzenet visszahívási függvény például hello következő kódot használhatja:

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

hívás túl hello**IoTHubMessage\_tulajdonságok** értéket ad vissza hello **térkép\_KEZELNI** hivatkozás. Jelenleg akkor továbbítja a hivatkozás túl**térkép\_GetInternals** tooobtain egy hivatkozás tooan tömbje hello név-érték párokat (valamint hello tulajdonságok száma). Ezen a ponton számbavétele hello tulajdonságok tooget toohello értékek azt szeretnénk, ha egyszerű kérdése.

Az alkalmazás nem rendelkezik toouse tulajdonságait. Azonban ha tooset kell őket események vagy kérheti le azokat az üzeneteket, hello **IoTHubClient** könyvtár megkönnyíti.

## <a name="message-handling"></a>Üzenet kezelése
Ahogy korábban is hangsúlyoztuk, amikor érkező üzenetek IoT-központ hello **IoTHubClient** könyvtár regisztrált visszahívási függvény meghívása válaszol. Ez a funkció, amely további rövid érdemel visszatérési paraméterének van. Ez egy olyan hello visszahívási függvény hello **IOT hubbal\_ügyfél\_minta\_http** mintaalkalmazást:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Vegye figyelembe, hogy hello visszatérési típusa **IOTHUBMESSAGE\_törlése\_eredmény** és az adott esetben a rendszer visszaadja a **IOTHUBMESSAGE\_elfogadott**. Nincsenek más értékek azt adhatja vissza a függvény módosítása hogyan hello **IoTHubClient** könyvtár reagál toohello üzenet visszahívási. Az alábbiakban hello-beállítások.

* **IOTHUBMESSAGE\_elfogadott** – hello üzenet feldolgozása sikeresen megtörtént. Hello **IoTHubClient** könyvtár nem indítja el hello visszahívási függvényt újra hello ugyanazt az üzenetet.
* **IOTHUBMESSAGE\_elutasítva** – hello üzenet nem lett feldolgozva, és nem desire toodo ezért hello jövőbeli van. Hello **IoTHubClient** könyvtár nem kell meghívnia hello visszahívási függvényt újra hello ugyanazt az üzenetet.
* **IOTHUBMESSAGE\_ABANDONED** – üdvözlőüzenetére nem feldolgozása sikeresen megtörtént, de hello **IoTHubClient** könyvtárban kell meghívnia hello visszahívási függvényt újra hello ugyanazt az üzenetet.

Hello az első két visszatérési kódok, hello **IoTHubClient** könyvtár küld egy üzenet tooIoT Hub, amely jelzi, hogy üdvözlőüzenetére törli hello eszköz várólistáról legyen, és nem érkezik újra. hello nettó hatása van hello azonos (üdvözlőüzenetére hello eszköz várólista törlődik), de e hello elfogadta vagy visszautasított továbbra is rögzíti.  Felvétel a különbség üdvözlőüzenetére hasznos toosenders, akik a visszajelzés figyelésére és megtudhatja, ha egy eszköz elfogadta vagy egy üzenet visszautasította.

Hello utolsó esetben egy üzenetet is kap tooIoT Hub, de azt jelzi, hogy hello üzenet újbóli kézbesítése kell lennie. Általában egy üzenetet fog abandon, ha valamilyen hiba előforduló, de tootry tooprocess hello üzenet újra. Ezzel szemben egy üzenet elutasítása akkor megfelelő ha helyrehozhatatlan hibát észlel (vagy, egyszerűen dönthet úgy, hogy tooprocess üdvözlőüzenetére).

Mindenképpen figyelembe hello különböző visszatérési kódokat, hogy akkor is kér hello viselkedés hello a kívánt **IoTHubClient** könyvtárban.

## <a name="alternate-device-credentials"></a>Alternatív hitelesítő adatai
Ahogy korábban, hello először thing toodo az hello használatakor **IoTHubClient** könyvtárban tooobtain egy **IOT HUBBAL\_ügyfél\_KEZELNI** hello például a következőt hívja a következő:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

argumentum túl hello**IoTHubClient\_CreateFromConnectionString** hello eszköz kapcsolati karakterláncot és a paraméter, amely jelzi az IoT hubbal használjuk toocommunicate hello protokoll. hello eszköz kapcsolati karakterlánc formátuma a következőképpen jelenik meg:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Ez a karakterlánc a négy adatra van: az IoT-központ nevét, az IoT-központ utótag, Eszközazonosító és megosztott elérési kulcsot. Hello teljesen minősített tartománynevét (FQDN) az IoT-központ az beszerzése az hello Azure-portálon az IoT hub-példány létrehozásakor – így hello IoT hub name (hello első része hello teljes Tartományneve) és hello IoT hub utótag (hello többi hello FQDN). Kapott hello Eszközazonosító és hello megosztott elérési kulcsot az eszköz regisztrálása az IoT-központ (lásd: hello [előző cikkben](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** egyirányú tooinitialize hello library lehetővé teszi. Ha jobban szeret, hozhat létre egy új **IOT HUBBAL\_ügyfél\_KEZELNI** hello eszköz kapcsolati karakterlánc helyett ezeket az egyes paraméterek használatával. Ez a kód a következő hello érhető el:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Ezt a feladatot el hello ugyanaz, mint **IoTHubClient\_CreateFromConnectionString**.

Nyilvánvaló, hogy érdemes-e toouse tűnhet **IoTHubClient\_CreateFromConnectionString** ahelyett, hogy ez a részletesebb módszer az inicializálás. Ne feledje, azonban, hogy amikor regisztrál egy eszközt az IoT-központ tartalmával egy Eszközazonosító és eszközkulcs (kapcsolati karakterlánc). Hello *eszköz explorer* hello bevezetett SDK eszköz [előző cikkben](iot-hub-device-sdk-c-intro.md) hello-tárakat használ **Azure IoT szolgáltatás SDK** származó toocreate hello eszköz kapcsolati karakterlánc hello Eszközazonosító eszközkulcs és az IoT-központ állomásnevet. Így hívása **IoTHubClient\_inden\_létrehozása** oka menti azt előnyösebb lehet hello lépésében létrehozása egy kapcsolati karakterláncot. Használja az kényelmes bármelyik módszert.

## <a name="configuration-options"></a>Konfigurációs beállítások
Amennyiben minden leírt hello módon hello kapcsolatos **IoTHubClient** könyvtár works tükrözi az alapértelmezett viselkedés. Van azonban néhány lehetőség, amely lehet toochange hello szalagtár működése. Mindez, ami hello **IoTHubClient\_inden\_SetOption** API. Vegye figyelembe az ebben a példában:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Van néhány gyakran használt beállításokat:

* **SetBatching** (logikai) – Ha **igaz**, majd a küldött adatok tooIoT Hub rendszer kötegekben. Ha **hamis**, majd üzenetküldés külön-külön. hello alapértelmezett érték a **hamis**. Vegye figyelembe, hogy hello **SetBatching** beállítás csak akkor alkalmazható, toohello HTTP protokollt, és nem toohello MQTT vagy AMQP protokollt.
* **Időtúllépés** (előjel nélküli egész) – Ez az érték ezredmásodpercben szerepel. Ha egy HTTP-kérés vagy válasz fogadása ennél hosszabb ideje tart, majd hello kapcsolat időtúllépése küldi.

a beállítás kötegelés hello fontos. Alapértelmezés szerint hello könyvtár ingresses események külön-külön (egy adott eseményhez bármilyen át túl**IoTHubClient\_inden\_SendEventAsync**). Ha kötegelés beállítás hello **igaz**, hello könyvtár eseményeit annyi hello pufferből (felfelé toohello üzenetek maximális méretét, amely elfogadja az IoT-központ) használhatja.  hello esemény kötegelt küldött tooIoT központ egyetlen HTTP hívás (hello események, egy JSON-tömb be vannak becsomagolva). Általában kötegelés engedélyezését eredményezi nagy teljesítménynövekedéshez óta hálózati üzenetváltások most csökkentése. Mivel az esemény-es HTTP-fejlécek egy készletét, nem pedig a minden egyes esemény fejléc készlettel küldi is jelentősen csökkenti a sávszélesség. Ha egy konkrét Okkód toodo egyébként nincs, általában érdemes tooenable kötegelés.

## <a name="next-steps"></a>Következő lépések
Ez a cikk ismerteti a részletes hello viselkedését hello **IoTHubClient** függvénytár szerepel a hello **C-hez készült SDK Azure IoT-eszközök**. Az információ, rendelkeznie kell hello hello funkcióinak beható ismerete **IoTHubClient** könyvtárban. Hello [tovább cikk](iot-hub-device-sdk-c-serializer.md) hasonló részletesen ismerteti a hello **szerializáló** könyvtár.

az IoT-központ fejlesztésével kapcsolatos további toolearn lásd: hello [Azure IoT SDK-k][lnk-sdks].

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
