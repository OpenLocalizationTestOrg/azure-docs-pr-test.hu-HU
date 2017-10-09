---
title: "aaaUnderstand Azure IoT Hub eszköz twins |} Microsoft Docs"
description: "Fejlesztői útmutató - eszköz használata twins toosynchronize állapotot és a konfigurációs adatokat az IoT-központ és az eszközök között"
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7dade18665108ed352ff3d18e864dc34f451bbf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>Ismertetés és az IoT Hub eszköz twins használata
## <a name="overview"></a>Áttekintés
*Eszköz twins* tárolása eszköz állapota (a metaadatok, a konfigurációk és a feltételek) JSON-dokumentumok vannak. Az IoT-központ továbbra is fennáll, az egyes eszközök, hogy tooIoT központi csatlakozás egy eszköz iker. Ez a cikk ismerteti:

* hello eszköz iker szerkezete hello: *címkék*, *kívánt* és *tulajdonságok jelentett*, és
* hello eszközeinek alkalmazásait és hátsó végpontok eszköz twins hajthat végre műveleteket.

> [!NOTE]
> Eszköz twins jelenleg csak az eszközök, tooIoT központi csatlakozás elérhető hello MQTT protokoll használatával. Tekintse meg a toohello [MQTT támogatási] [ lnk-devguide-mqtt] a cikk útmutatást tooconvert meglévő eszköz alkalmazás toouse MQTT.
> 
> 

### <a name="when-toouse"></a>Ha toouse
Az eszköz twins használja:

* Hello felhőben tárolt az eszközre vonatkozó metaadatok. Például hello egy Eladóautomata telepítési helyét.
* A jelentés aktuális állapotadatokat például a rendelkezésre álló lehetőségek és az eszköz alkalmazás állapotának. Például egy eszköz csatlakoztatott tooyour IoT-központ cellás over vagy Wi-Fi.
* A szinkronizálás hello állapotának hosszan futó munkafolyamatok eszközalkalmazás és háttér-alkalmazás között. Például hello megoldás biztonsági end megadja új belső vezérlőprogram verziója tooinstall hello és hello eszköz alkalmazás jelentések hello hello frissítési folyamat különböző szakaszaiban.
* Az eszköz metaadatait, konfigurációs vagy az állapot lekérdezése.

Tekintse meg a túl[eszközről a felhőbe kommunikációs útmutatást] [ lnk-d2c-guidance] jelentett tulajdonságok, az eszköz a felhőbe küldött üzeneteket vagy a fájl feltöltése útmutatót.
Tekintse meg a túl[felhő eszközre kommunikációs útmutatást] [ lnk-c2d-guidance] kívánt tulajdonságokkal, a közvetlen módszerek és a felhő-eszközre küldött üzenetek útmutatót.

## <a name="device-twins"></a>Eszköz twins
Eszköz twins eszközzel kapcsolatos adatok tárolására, amelyek:

* Eszköz- és biztonsági végpontok toosynchronize eszköz feltételek és a konfigurációs használhatja.
* hello megoldás háttérrendszerének tooquery használhatja és cél hosszú futású műveleteket.

egy eszköz iker hello életciklusát kapcsolódó toohello megfelelő [eszközidentitás][lnk-identity]. Eszköz twins implicit létrehozása és törlése, amikor egy új eszközidentitás jön létre, vagy az IoT-központ törölt.

Egy eszköz iker tartalmazó JSON-dokumentumhoz:

* **Címkék**. Hello JSON-dokumentum, amely a megoldás háttérrendszeréhez hello szakasz is olvasni és írni. Címkék olyan nem látható toodevice alkalmazások.
* **Szükségeskonfiguráció-tulajdonságok**. Használható együtt a jelentésben szereplő tulajdonságok toosynchronize eszközök konfigurációját és a feltételek. Kívánt tulajdonságok csak akkor állítható vissza hello megoldás end és hello eszközalkalmazás tudja olvasni. hello eszközalkalmazás is szükséges hello tulajdonságok változásainak valós időben értesítést.
* **Tulajdonságok jelentett**. Használható együtt a kívánt tulajdonságokkal toosynchronize eszközök konfigurációját és a feltételek. Jelentett tulajdonságok csak hello eszköz alkalmazás által állítható be és olvashatók és hello megoldás háttérrendszerének kellettek.

Emellett hello azon hello eszköz iker JSON-dokumentum tartalmaz hello megfelelő eszközidentitás hello tárolja a csak olvasható tulajdonságok hello [identitásjegyzékhez][lnk-identity].

![][img-twin]

a következő példa hello egy eszköz iker JSON-dokumentum jeleníti meg:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

A legfelső szintű objektum hello hello rendszer tulajdonságai, és a tárolóobjektumok `tags` és mindkét `reported` és `desired` tulajdonságok. Hello `properties` tároló néhány írásvédett elemet tartalmaz (`$metadata`, `$etag`, és `$version`) hello ismertetett [eszköz iker metaadatok] [ lnk-twin-metadata] és [ Egyidejű hozzáférések optimista] [ lnk-concurrency] szakaszok.

### <a name="reported-property-example"></a>Jelentett tulajdonság – példa
Hello előző példában hello eszköz iker tartalmaz egy `batteryLevel` hello eszköz alkalmazás által jelentett tulajdonság. Ez a tulajdonság lehetővé teszi lehetséges tooquery, és a hello utolsó jelentett akkumulátor szint alapján eszközökön működik. További példák lehetnek hello app jelentéskészítési eszköz eszközképességek vagy kapcsolati beállításokat.

> [!NOTE]
> Jelentett tulajdonságok forgatókönyvek, ahol hello megoldás háttérrendszerének utolsó ismert tulajdonság értékének hello iránt érdeklődik egyszerűsítése érdekében. Használjon [eszköz a felhőbe küldött üzeneteket] [ lnk-d2c] hogy hello megoldás háttérrendszerének tooprocess telemetriát eseménysorozat időbélyegzővel, például a time series hello formában kell-e.

### <a name="desired-property-example"></a>Kívánt tulajdonság – példa
Hello előző példában hello `telemetryConfig` eszköz iker szükséges, és a jelentésben szereplő tulajdonságok ehhez az eszközhöz hello megoldás háttérrendszerének és hello eszköz toosynchronize hello telemetriai Alkalmazáskonfiguráció által használt. Példa:

1. hello megoldás háttérrendszerének szükséges hello tulajdonság szükséges hello konfigurációs érték beállítása. Szükségeskonfiguráció-hello tulajdonságkészlet hello dokumentum hello része a következő:
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. hello eszközalkalmazás hello változás azonnal, ha csatlakoztatva, vagy a hello először csatlakoztassa értesítést kap. hello eszközalkalmazás majd jelentést készít hello frissített konfigurációs (vagy hello segítségével hibaállapotot `status` tulajdonság). Itt hello hello része jelentett tulajdonságok:
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. hello megoldás háttérrendszerének nyomon követheti hello eredményeit hello konfigurációs műveletet több eszközön, az [lekérdezése] [ lnk-query] eszköz twins.

> [!NOTE]
> hello előző kódtöredékek példák, olvashatóságát, egyirányú tooencode optimalizálva, egy eszköz konfigurálásának és annak állapotát. Az IoT-központ nem ír elő egy adott séma hello eszköz iker szükséges, és jelentett hello eszköz twins-tulajdonságokat.
> 
> 

Twins toosynchronize hosszú ideig futó műveletek, például a belső vezérlőprogram-frissítésekre is használhatja. További információ a hogyan toouse tulajdonságok toosynchronize és egy hosszú ideig futó művelet az eszközön, nyomon követése: [használata szükséges tulajdonságok tooconfigure eszközök][lnk-twin-properties].

## <a name="back-end-operations"></a>Háttér-műveletek
hello megoldás háttérrendszerének működik hello eszköz iker hello atomi műveletek HTTP Protokollon keresztül elérhetővé a következő használatával:

1. **Id eszköz két lekérni**. Ez a művelet visszaadja jelentett hello eszköz két dokumentumot, beleértve a címkék, és szükség esetén, és a rendszer tulajdonságai.
2. **Részlegesen frissítse az eszköz iker**. Ez a művelet lehetővé teszi a hello megoldás háttér toopartially frissítés hello címkék vagy egy eszköz iker kívánt tulajdonságokat. hello részleges frissítés, amely ad hozzá vagy bármilyen tulajdonságot JSON-dokumentumként van kifejezve. Tulajdonság értéke túl`null` törlődnek. hello alábbi példa létrehoz egy új kívánt tulajdonság értékű `{"newProperty": "newValue"}`, felülírja a meglévő értéke hello `existingProperty` rendelkező `"otherNewValue"`, és eltávolítja a `otherOldProperty`. Nincs más módosításai szükséges tooexisting tulajdonságok vagy a címkék:
   
        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }
3. **Cserélje ki a kívánt tulajdonságokkal**. Ez a művelet lehetővé teszi, hogy hello megoldás háttér toocompletely felülírja a meglévő kívánt összes tulajdonság és helyettesítse be az új JSON-dokumentum `properties/desired`.
4. **Cserélje le a címkék**. Ez a művelet lehetővé teszi, hogy hello megoldás háttér toocompletely felülírja a meglévő található összes kódcímkének és helyettesítse be az új JSON-dokumentum `tags`.
5. **A két értesítéseket**. Ez a művelet lehetővé teszi, hogy a hello megoldás háttér toobe értesíti, ha a hello iker módosul. toodo Igen, az IoT-megoldásból toocreate útvonal és kell tooset hello adatforrás értéke túl*twinChangeEvents*. Alapértelmezés szerint nincs iker értesítések küldése általában olyan, ez azt jelenti, hogy, hogy nincs ilyen útvonal létezik-e előre. Ha hello módosítási aránya túl magas, vagy egyéb okból, például a belső hibák, hello IoT-központ, amely tartalmazza az összes módosítás csak egy értesítést küldhet. Ezért, ha az alkalmazásnak megbízható vizsgálati és naplózási az összes köztes állapotok, majd továbbra is ajánlott D2C üzenetek használata. hello iker üzenet tulajdonságait és a szövegtörzs tartalmaz.

    - Tulajdonságok

    | Név | Érték |
    | --- | --- |
    $content-típus | application/json |
    $iothub-enqueuedtime |  Ha hello értesítés küldése idő |
    $iothub-üzenet-forrás | twinChangeEvents |
    $content-kódolás | az UTF-8 |
    deviceId | Hello eszköz azonosítója |
    hubName | Az IoT-központ nevét |
    operationTimestamp | [ISO8601] művelet időbélyegzője |
    IOT hubbal-üzenet-séma | deviceLifecycleNotification |
    opType | "replaceTwin" vagy "updateTwin" |

    Üzenet Rendszertulajdonságok fűzve előtagként hello `'$'` szimbólum.

    - Törzs
        
    Ez a szakasz a JSON formátumban minden hello iker módosításait tartalmazza. Hello formátuma azonos azt használja, mint a javítások, hello különbséggel, hogy minden iker szakasz tartalmazhat: címkék, properties.reported, properties.desired és hello "$metadata" elemet tartalmaz. Például:
    ```
    {
        "properties": {
            "desired": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            },
            "reported": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            }
        }
    }
    ``` 

Összes hello támogatása az előző operations [egyidejű hozzáférések optimista] [ lnk-concurrency] és hello szükséges **ServiceConnect** engedéllyel, a hello [biztonsági ] [ lnk-security] cikk.

Ezenkívül toothese műveletek, hello megoldás biztonsági célból is:

* Hello eszköz twins hello segítségével lekérdezési SQL-szerű [IoT-központ lekérdezési nyelv][lnk-query].
* Műveletek végzése eszköz twins használatával nagy mennyiségű [feladatok][lnk-jobs].

## <a name="device-operations"></a>Eszköz műveletek
hello eszközalkalmazás működik hello eszköz iker hello atomi műveletek a következő használatával:

1. **A két eszköz beolvasása**. Ez a művelet hello eszköz két dokumentumot ad vissza, (beleértve a címkék és a szükséges, a jelentett és a rendszer tulajdonságai) a hello a jelenleg csatlakoztatott eszköz.
2. **Részlegesen a jelentett tulajdonságainak frissítése**. A művelet lehetővé teszi, hogy hello részleges frissítés hello a jelentett hello jelenleg csatlakoztatott eszköz tulajdonságait. A művelet által használt hello azonos JSON frissíteni, hogy hello megoldás hátsó felhasználását egy részleges frissítés kívánt tulajdonságok formátumban.
3. **Figyelje meg a kívánt tulajdonságokkal**. hello jelenleg csatlakoztatott eszközön választható toobe fordulhat elő, akkor a frissítés szükséges toohello tulajdonságai értesítést. hello eszköz megkapja hello hello megoldás háttérrendszerének hajtja végre a frissítést (teljes vagy részleges csere) ugyanolyan formában.

Az összes fenti műveletek hello hello szükséges **DeviceConnect** engedéllyel, a hello [biztonsági] [ lnk-security] cikk.

Hello [Azure IoT-eszközök SDK-k] [ lnk-sdks] révén könnyen toouse hello megelőző sok nyelvekhez és platformokhoz műveletek. További információ az IoT-központ primitívek kívánt tulajdonságokkal szinkronizálás hello részleteit található [eszköz újracsatlakozás folyamat][lnk-reconnection].

> [!NOTE]
> Eszköz twins jelenleg csak az eszközök, tooIoT központi csatlakozás elérhető hello MQTT protokoll használatával.
> 
> 

## <a name="reference-topics"></a>Referencia-témaköreit:
hello következő referencia-témakörökre biztosít ellenőrző hozzáférés tooyour IoT-központ további információt.

## <a name="tags-and-properties-format"></a>Címkék és a Tulajdonságok
Címke, és a jelentett kívánt tulajdonság található a következő korlátozások hello JSON-objektumok:

* A JSON-objektumok összes kulcsai kis-és nagybetűket 64 bájt UTF-8 UNICODE karakterláncokat. Engedélyezett karakterek kizárása UNICODE vezérlőkaraktereket (szegmensek C0 és C1), és `'.'`, `' '`, és `'$'`.
* A következő JSON típusok hello hasznos lehet a JSON-objektumok szereplő összes érték: boolean, számot, karakterlánc, objektum. Tömbök használata nem megengedett.
* Címkék, kívánt és jelentett tulajdonságok minden JSON-objektumok maximális mélysége 5 rendelkezhet. Például a következő objektum hello érvénytelen:

        {
            ...
            "tags": {
                "one": {
                    "two": {
                        "three": {
                            "four": {
                                "five": {
                                    "property": "value"
                                }
                            }
                        }
                    }
                }
            },
            ...
        }

* Az összes karakterlánc-érték nagyobb, mint 512 bájt hosszúságú lehet.

## <a name="device-twin-size"></a>Eszköz iker mérete
Az IoT-központ kikényszeríti egy 8 KB-os méret korlátozás hello értékeit `tags`, `properties/desired`, és `properties/reported`, kivéve a csak olvasható elemeket.
hello mérete számított leltár szerint UNICODE kivételével minden karakter karakterek (szegmensek C0 és C1) és a lemezterület ellenőrzése `' '` amikor megjelenik egy karakterlánc-konstansra kívül.
Az IoT-központ volna nagyítása hello hello meghaladja a dokumentumok összes művelet elutasítja hiba történt.

## <a name="device-twin-metadata"></a>Eszköz iker metaadatok
Az IoT-központ tárolja a hello időbélyeg hello minden JSON-objektumból, az eszköz a két utolsó frissítésének szükséges tulajdonságok jelentett. hello időbélyegeket UTC és hello kódolású [ISO8601] formátum `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Példa:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Ez az információ minden szintű (ne csak hello levelek, JSON struktúrában hello) toopreserve frissítések objektum bejegyzéseinek eltávolítása kell tartani.

## <a name="optimistic-concurrency"></a>Egyidejű hozzáférések optimista
Címkék, szükséges, és a Tulajdonságok jelentett összes támogatási hozzáférések optimista.
Címkék rendelkezik egy ETag megfelelően [RFC7232], amely JSON-megjelenítés hello címke jelöli. A feltételes frissítési műveletekben hello megoldás háttér tooensure konzisztencia az ETag-EK is használhatja.

Eszköz iker szükséges, és a Tulajdonságok jelentett ETag-EK nem rendelkezik, de rendelkezik egy `$version` érték, amely garantáltan toobe növekményes. Hasonlóképpen tooan ETag, hello verzió frissítése fél tooenforce konzisztencia frissítések hello használható. Például egy eszköz-alkalmazás egy jelentett tulajdonság vagy hello megoldás háttérrendszerének kívánt tulajdonság.

Verziók is hasznosak, ha (például hello eszközalkalmazás szükséges hello tulajdonságok betartásával) observing ügynök fajok lekérése művelet eredménye hello és frissítési értesítést között kell feloldani. a szakasz hello [eszköz újracsatlakozás folyamat] [ lnk-reconnection] nyújt részletesebb információt.

## <a name="device-reconnection-flow"></a>Eszköz újracsatlakozás folyamata
Az IoT-központ nem őrzi meg a kívánt tulajdonságokkal frissítési értesítések leválasztott eszközökhöz. Ez azt jelenti, hogy a csatlakozó eszköz hello teljes kívánt tulajdonságokat tartalmazó dokumentum, a frissítési értesítések továbbá toosubscribing kell olvasni. Frissítési értesítések és a teljes visszaállításhoz közötti fajok hello lehetőséget biztosítani, a következő folyamat hello biztosítani kell:

1. Eszköz alkalmazásának tooan IoT-központ csatlakozik.
2. Eszköz alkalmazás frissítési értesítések előfizet a kívánt tulajdonságokat.
3. Eszköz alkalmazás lekéri a hello teljes dokumentumot a kívánt tulajdonságokat.

hello eszközalkalmazás figyelmen kívül hagyhatja az összes értesítésben `$version` kisebb vagy egyenlő, mint hello teljes lekérdezése dokumentum hello verzióját. Ez a megközelítés lehetőség, mert az IoT-központ biztosítja, hogy verziók mindig növelhető.

> [!NOTE]
> A működési elvet tagot már megvalósította a hello [Azure IoT-eszközök SDK-k][lnk-sdks]. A leírást akkor hasznos, csak akkor, ha hello eszköz alkalmazás az Azure IoT-eszközök SDK-k nem használható, és közvetlenül kell program hello MQTT felületet.
> 
> 

## <a name="additional-reference-material"></a>További referenciaanyag
Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:

* Hello [IoT-központok végpontjai] [ lnk-endpoints] cikkből hello minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontokhoz.
* Hello [sávszélesség-szabályozási és kvóták] [ lnk-quotas] cikkből hello kvóták toohello IoT-központ szolgáltatás és szabályozási viselkedés tooexpect hello hello szolgáltatás használatakor.
* Hello [Azure IoT-eszközök és szolgáltatások SDK] [ lnk-sdks] cikk listák hello használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal való fejlesztésekor SDK különböző nyelvi csomagok.
* Hello [IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] cikkből hello tooretrieve IoT Hub-ből származó adataival az eszköz twins és feladatok is használhatja az IoT-központ lekérdezési nyelv .
* Hello [IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] cikk hello MQTT protokoll IoT Hub-támogatással kapcsolatos további információkat tartalmazza.

## <a name="next-steps"></a>Következő lépések
Most eszköz twins megismerte esetleg érdekli hello IoT Hub fejlesztői útmutató témakörei a következő:

* [Az eszközön közvetlen metódus][lnk-methods]
* [Több eszközön feladatok ütemezése][lnk-jobs]

Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ oktatóanyagok következő hello iránt érdeklődik:

* [Hogyan toouse hello eszköz iker][lnk-twin-tutorial]
* [Hogyan toouse eszköz iker tulajdonságai][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#device-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png
