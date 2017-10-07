---
title: "távoli felügyeleti megoldás hello aaaDevice információk metaadatok |} Microsoft Docs"
description: "Hello Azure IoT előre konfigurált megoldás távoli megfigyelési és az architektúra leírását."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 8387b98b8b2ae4934b0c900bc4df37dc17337c60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="device-information-metadata-in-hello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="82808-103">Eszköz információk metaadatait a távoli felügyeleti előkonfigurált megoldás hello</span><span class="sxs-lookup"><span data-stu-id="82808-103">Device information metadata in hello remote monitoring preconfigured solution</span></span>

<span data-ttu-id="82808-104">hello Azure IoT Suite távoli felügyeleti előkonfigurált megoldás egy eszköz metaadatait kezelésére szolgáló módszert mutatja be.</span><span class="sxs-lookup"><span data-stu-id="82808-104">hello Azure IoT Suite remote monitoring preconfigured solution demonstrates an approach for managing device metadata.</span></span> <span data-ttu-id="82808-105">Ez a cikk ismerteti a hello megközelítés ehhez a megoldáshoz szükséges tooenable meg toounderstand:</span><span class="sxs-lookup"><span data-stu-id="82808-105">This article outlines hello approach this solution takes tooenable you toounderstand:</span></span>

* <span data-ttu-id="82808-106">Milyen metaadatok hello megoldás tárolja.</span><span class="sxs-lookup"><span data-stu-id="82808-106">What device metadata hello solution stores.</span></span>
* <span data-ttu-id="82808-107">Hello megoldás hogyan kezeli a hello eszköz metaadatait.</span><span class="sxs-lookup"><span data-stu-id="82808-107">How hello solution manages hello device metadata.</span></span>

## <a name="context"></a><span data-ttu-id="82808-108">Környezet</span><span class="sxs-lookup"><span data-stu-id="82808-108">Context</span></span>

<span data-ttu-id="82808-109">hello távoli megfigyelési előre konfigurált megoldás által használt [Azure IoT Hub] [ lnk-iot-hub] tooenable az eszközök toosend adatok toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="82808-109">hello remote monitoring preconfigured solution uses [Azure IoT Hub][lnk-iot-hub] tooenable your devices toosend data toohello cloud.</span></span> <span data-ttu-id="82808-110">hello megoldás három különböző helyeken eszközökkel kapcsolatos információkat tárolja:</span><span class="sxs-lookup"><span data-stu-id="82808-110">hello solution stores information about devices in three different locations:</span></span>

| <span data-ttu-id="82808-111">Hely</span><span class="sxs-lookup"><span data-stu-id="82808-111">Location</span></span> | <span data-ttu-id="82808-112">Tárolt információ</span><span class="sxs-lookup"><span data-stu-id="82808-112">Information stored</span></span> | <span data-ttu-id="82808-113">Megvalósítás</span><span class="sxs-lookup"><span data-stu-id="82808-113">Implementation</span></span> |
| -------- | ------------------ | -------------- |
| <span data-ttu-id="82808-114">Identitásjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="82808-114">Identity registry</span></span> | <span data-ttu-id="82808-115">Eszközazonosító, a hitelesítési kulcsokat, engedélyezett állapota</span><span class="sxs-lookup"><span data-stu-id="82808-115">Device id, authentication keys, enabled state</span></span> | <span data-ttu-id="82808-116">A beépített tooIoT Hub</span><span class="sxs-lookup"><span data-stu-id="82808-116">Built in tooIoT Hub</span></span> |
| <span data-ttu-id="82808-117">Eszköz twins</span><span class="sxs-lookup"><span data-stu-id="82808-117">Device twins</span></span> | <span data-ttu-id="82808-118">Metaadatok: jelentett tulajdonságai, a kívánt tulajdonságokat, a címkék</span><span class="sxs-lookup"><span data-stu-id="82808-118">Metadata: reported properties, desired properties, tags</span></span> | <span data-ttu-id="82808-119">A beépített tooIoT Hub</span><span class="sxs-lookup"><span data-stu-id="82808-119">Built in tooIoT Hub</span></span> |
| <span data-ttu-id="82808-120">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="82808-120">Cosmos DB</span></span> | <span data-ttu-id="82808-121">A parancs és metódus előzményei</span><span class="sxs-lookup"><span data-stu-id="82808-121">Command and method history</span></span> | <span data-ttu-id="82808-122">Egyéni megoldás</span><span class="sxs-lookup"><span data-stu-id="82808-122">Custom for solution</span></span> |

<span data-ttu-id="82808-123">Az IoT-központ tartalmazza a [eszközidentitási] [ lnk-identity-registry] toomanage tooan IoT hub és a hozzáférési [eszköz twins] [ lnk-device-twin] toomanage eszköz metaadatait .</span><span class="sxs-lookup"><span data-stu-id="82808-123">IoT Hub includes a [device identity registry][lnk-identity-registry] toomanage access tooan IoT hub and uses [device twins][lnk-device-twin] toomanage device metadata.</span></span> <span data-ttu-id="82808-124">Is van a távoli felügyeleti megoldás-specifikus *eszközbeállításjegyzékben* , amely tárolja a parancs és metódus előzményeit.</span><span class="sxs-lookup"><span data-stu-id="82808-124">There is also a remote monitoring solution-specific *device registry* that stores command and method history.</span></span> <span data-ttu-id="82808-125">hello távoli figyelési megoldást használ egy [Cosmos DB] [ lnk-docdb] adatbázis tooimplement parancs és metódus előzményekhez szükséges egyéni tárolót.</span><span class="sxs-lookup"><span data-stu-id="82808-125">hello remote monitoring solution uses a [Cosmos DB][lnk-docdb] database tooimplement a custom store for command and method history.</span></span>

> [!NOTE]
> <span data-ttu-id="82808-126">távoli felügyeleti előkonfigurált megoldás hello tartja hello eszközidentitási hello információk szinkronban hello Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="82808-126">hello remote monitoring preconfigured solution keeps hello device identity registry in sync with hello information in hello Cosmos DB database.</span></span> <span data-ttu-id="82808-127">Mindkettő használja a azonos eszköz azonosítója toouniquely azonosítása hello minden eszköz csatlakoztatva tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="82808-127">Both use hello same device id toouniquely identify each device connected tooyour IoT hub.</span></span>

## <a name="device-metadata"></a><span data-ttu-id="82808-128">Eszköz metaadatait</span><span class="sxs-lookup"><span data-stu-id="82808-128">Device metadata</span></span>

<span data-ttu-id="82808-129">Az IoT-központ tart fenn a [eszköz iker] [ lnk-device-twin] az egyes szimulált és a fizikai eszközök távoli felügyeleti megoldás tooa csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="82808-129">IoT Hub maintains a [device twin][lnk-device-twin] for each simulated and physical device connected tooa remote monitoring solution.</span></span> <span data-ttu-id="82808-130">hello megoldás az eszköz twins toomanage hello társított metaadatokat eszközök.</span><span class="sxs-lookup"><span data-stu-id="82808-130">hello solution uses device twins toomanage hello metadata associated with devices.</span></span> <span data-ttu-id="82808-131">Egy eszköz iker IoT-központ által kezelt JSON-dokumentum, és hello megoldás eszköz twins hello IoT Hub API toointeract használ.</span><span class="sxs-lookup"><span data-stu-id="82808-131">A device twin is a JSON document maintained by IoT Hub, and hello solution uses hello IoT Hub API toointeract with device twins.</span></span>

<span data-ttu-id="82808-132">Egy eszköz iker metaadatok három típusú tárolja:</span><span class="sxs-lookup"><span data-stu-id="82808-132">A device twin stores three types of metadata:</span></span>

- <span data-ttu-id="82808-133">*Tulajdonságok jelentett* tooan IoT hub eszköz által küldött.</span><span class="sxs-lookup"><span data-stu-id="82808-133">*Reported properties* are sent tooan IoT hub by a device.</span></span> <span data-ttu-id="82808-134">Hello távoli felügyeleti megoldás, a szimulált eszköz küldött jelentésben szereplő tulajdonságok indításkor és a válasz túl**módosította az eszköz állapotát,** parancsok és módszerek.</span><span class="sxs-lookup"><span data-stu-id="82808-134">In hello remote monitoring solution, simulated devices send reported properties at start-up and in response too**Change device state** commands and methods.</span></span> <span data-ttu-id="82808-135">Jelentett tulajdonságok megtekintheti a hello **eszközlista** és **eszközadatok** hello megoldás portálon.</span><span class="sxs-lookup"><span data-stu-id="82808-135">You can view reported properties in hello **Device list** and **Device details** in hello solution portal.</span></span> <span data-ttu-id="82808-136">Jelentett tulajdonságok csak olvashatók.</span><span class="sxs-lookup"><span data-stu-id="82808-136">Reported properties are read only.</span></span>
- <span data-ttu-id="82808-137">*Szükségeskonfiguráció-tulajdonságok* lekért hello IoT hub-eszközök által.</span><span class="sxs-lookup"><span data-stu-id="82808-137">*Desired properties* are retrieved from hello IoT hub by devices.</span></span> <span data-ttu-id="82808-138">Hello felelőssége hello eszköz toomake bármely szükséges konfigurációs változásokat hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="82808-138">It is hello responsibility of hello device toomake any necessary configuration change on hello device.</span></span> <span data-ttu-id="82808-139">Feladata is hello hello eszköz tooreport hello módosítás vissza toohello központ jelentett tulajdonságainál.</span><span class="sxs-lookup"><span data-stu-id="82808-139">It is also hello responsibility of hello device tooreport hello change back toohello hub as a reported property.</span></span> <span data-ttu-id="82808-140">Beállíthatja a kívánt tulajdonság értéke hello megoldás portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="82808-140">You can set a desired property value through hello solution portal.</span></span>
- <span data-ttu-id="82808-141">*Címkék* hello eszköz iker csak szerepelnek, és egy eszköz nem lesznek szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="82808-141">*Tags* only exist in hello device twin and are never synchronized with a device.</span></span> <span data-ttu-id="82808-142">Állítsa be az értékeket hello megoldás portálon, és az eszközök listájának hello szűrésekor használja őket.</span><span class="sxs-lookup"><span data-stu-id="82808-142">You can set tag values in hello solution portal and use them when you filter hello list of devices.</span></span> <span data-ttu-id="82808-143">hello megoldás is használ egy címke tooidentify hello ikon toodisplay hello megoldás portálon egy eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="82808-143">hello solution also uses a tag tooidentify hello icon toodisplay for a device in hello solution portal.</span></span>

<span data-ttu-id="82808-144">Példa jelentett, a szimulált hello eszközökről tulajdonságai közé tartozik a gyártó, a modell számának, a szélességi és hosszúsági.</span><span class="sxs-lookup"><span data-stu-id="82808-144">Example reported properties from hello simulated devices include manufacturer, model number, latitude, and longitude.</span></span> <span data-ttu-id="82808-145">Szimulált eszköz is visszatérési hello támogatott módszerek listája jelentett tulajdonságainál.</span><span class="sxs-lookup"><span data-stu-id="82808-145">Simulated devices also return hello list of supported methods as a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="82808-146">hello szimulált eszköz kód csak akkor használja, hello **Desired.Config.TemperatureMeanValue** és **Desired.Config.TelemetryInterval** kívánt tulajdonságokkal tooupdate hello jelentett küldött vissza tulajdonságai tooIoT központ.</span><span class="sxs-lookup"><span data-stu-id="82808-146">hello simulated device code only uses hello **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties tooupdate hello reported properties sent back tooIoT Hub.</span></span> <span data-ttu-id="82808-147">Az összes többi kívánt tulajdonság változáskérések figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="82808-147">All other desired property change requests are ignored.</span></span>

<span data-ttu-id="82808-148">Egy eszköz információk metaadatok JSON-dokumentum hello eszköz beállításjegyzék Cosmos DB adatbázisban tárolt struktúra a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="82808-148">A device information metadata JSON document stored in hello device registry Cosmos DB database has hello following structure:</span></span>

```json
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

> [!NOTE]
> <span data-ttu-id="82808-149">Eszközadatok tartalmazhat metaadatok toodescribe hello telemetriai hello eszköz küld tooIoT Hub is.</span><span class="sxs-lookup"><span data-stu-id="82808-149">Device information can also include metadata toodescribe hello telemetry hello device sends tooIoT Hub.</span></span> <span data-ttu-id="82808-150">hello távoli figyelési megoldást használja a telemetriai adatok metaadatok toocustomize hello irányítópult megjelenítésének [dinamikus telemetriai][lnk-dynamic-telemetry].</span><span class="sxs-lookup"><span data-stu-id="82808-150">hello remote monitoring solution uses this telemetry metadata toocustomize how hello dashboard displays [dynamic telemetry][lnk-dynamic-telemetry].</span></span>

## <a name="lifecycle"></a><span data-ttu-id="82808-151">Életciklus</span><span class="sxs-lookup"><span data-stu-id="82808-151">Lifecycle</span></span>

<span data-ttu-id="82808-152">Ha először eszköz hello megoldás portálon, hello megoldás hoz létre egy Cosmos DB adatbázis toostore parancs hello bejegyzést és metódus előzményei.</span><span class="sxs-lookup"><span data-stu-id="82808-152">When you first create a device in hello solution portal, hello solution creates an entry in hello Cosmos DB database toostore command and method history.</span></span> <span data-ttu-id="82808-153">Ezen a ponton hello megoldás is létrehoz egy tételt hello eszköz hello eszközidentitási, amely hello kulcsok hello eszköz által használt tooauthenticate IoT-központ állít elő.</span><span class="sxs-lookup"><span data-stu-id="82808-153">At this point, hello solution also creates an entry for hello device in hello device identity registry, which generates hello keys hello device uses tooauthenticate with IoT Hub.</span></span> <span data-ttu-id="82808-154">Egy eszköz iker is létrehoz.</span><span class="sxs-lookup"><span data-stu-id="82808-154">It also creates a device twin.</span></span>

<span data-ttu-id="82808-155">Ha egy eszköz először csatlakozik toohello megoldás, és küldi el jelentett tulajdonságai egy eszköz tájékoztató üzenetet.</span><span class="sxs-lookup"><span data-stu-id="82808-155">When a device first connects toohello solution, it sends reported properties and a device information message.</span></span> <span data-ttu-id="82808-156">hello jelentett tulajdonságértékek automatikusan megtörténik az eszköz a két hello.</span><span class="sxs-lookup"><span data-stu-id="82808-156">hello reported property values are automatically saved in hello device twin.</span></span> <span data-ttu-id="82808-157">hello jelentett tulajdonságai közé tartozik a hello gyártó modellszámát, sorozatszám és támogatott módszerek listája.</span><span class="sxs-lookup"><span data-stu-id="82808-157">hello reported properties include hello device manufacturer, model number, serial number, and a list of supported methods.</span></span> <span data-ttu-id="82808-158">eszköz információk üdvözlőüzenetére hello listán szereplő hello parancsok hello eszköz támogatja, beleértve bármilyen parancs paraméterei kapcsolatos információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="82808-158">hello device information message includes hello list of hello commands hello device supports including information about any command parameters.</span></span> <span data-ttu-id="82808-159">Amikor hello megoldás ezt az üzenetet kap, frissíti hello eszközadatokat hello Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="82808-159">When hello solution receives this message, it updates hello device information in hello Cosmos DB database.</span></span>

### <a name="view-and-edit-device-information-in-hello-solution-portal"></a><span data-ttu-id="82808-160">Hello megoldás portálon eszköz adatainak megtekintése és szerkesztése</span><span class="sxs-lookup"><span data-stu-id="82808-160">View and edit device information in hello solution portal</span></span>

<span data-ttu-id="82808-161">hello hello megoldás portálon eszközök listáját jeleníti meg eszköztulajdonságok alábbi oszlopokként alapértelmezés szerint hello: **állapot**, **DeviceId**, **gyártó**, **Típusszámát**, **sorozatszám**, **belső vezérlőprogram**, **Platform**, **processzor**, és  **RAM telepített**.</span><span class="sxs-lookup"><span data-stu-id="82808-161">hello device list in hello solution portal displays hello following device properties as columns by default: **Status**, **DeviceId**, **Manufacturer**, **Model Number**, **Serial Number**, **Firmware**, **Platform**, **Processor**, and **Installed RAM**.</span></span> <span data-ttu-id="82808-162">Testre szabhatja hello oszlopok kattintva **oszlop szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="82808-162">You can customize hello columns by clicking **Column editor**.</span></span> <span data-ttu-id="82808-163">Eszköztulajdonságok hello **szélesség** és **hosszúság** hello helyre a Bing térképek hello meghajtó hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="82808-163">hello device properties **Latitude** and **Longitude** drive hello location in hello Bing Map on hello dashboard.</span></span>

![Oszlop-szerkesztőt az eszközök listája][img-device-list]

<span data-ttu-id="82808-165">A hello **eszközadatok** ablaktáblán hello megoldás portálon, szerkesztheti a kívánt tulajdonságokkal és címkék (csak olvasható tulajdonságok jelentette).</span><span class="sxs-lookup"><span data-stu-id="82808-165">In hello **Device Details** pane in hello solution portal, you can edit desired properties and tags (reported properties are read only).</span></span>

![Eszköz részletező ablaktábláján][img-device-edit]

<span data-ttu-id="82808-167">A megoldás hello megoldás portál tooremove egy eszközt használhatja.</span><span class="sxs-lookup"><span data-stu-id="82808-167">You can use hello solution portal tooremove a device from your solution.</span></span> <span data-ttu-id="82808-168">Egy eszköz eltávolításakor hello megoldás identitásjegyzékhez eltávolítja a hello eszköz bejegyzést, és majd törli az eszköz a két hello.</span><span class="sxs-lookup"><span data-stu-id="82808-168">When you remove a device, hello solution removes hello device entry from identity registry and then deletes hello device twin.</span></span> <span data-ttu-id="82808-169">hello megoldás is eltávolítja az adatokat kapcsolódó toohello eszköz hello Cosmos DB adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="82808-169">hello solution also removes information related toohello device from hello Cosmos DB database.</span></span> <span data-ttu-id="82808-170">Eszköz eltávolítása előtt le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="82808-170">Before you can remove a device, you must disable it.</span></span>

![Eszköz eltávolítása][img-device-remove]

## <a name="device-information-message-processing"></a><span data-ttu-id="82808-172">Eszköz információk üzenet feldolgozása</span><span class="sxs-lookup"><span data-stu-id="82808-172">Device information message processing</span></span>

<span data-ttu-id="82808-173">Eszköz információs üzenetet küldött egy eszköz különbözőek a telemetria-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="82808-173">Device information messages sent by a device are distinct from telemetry messages.</span></span> <span data-ttu-id="82808-174">Eszköz információk üzenetekben eszköz válaszolhassanak hello parancsok, és a parancselőzményekben.</span><span class="sxs-lookup"><span data-stu-id="82808-174">Device information messages include hello commands a device can respond to, and any command history.</span></span> <span data-ttu-id="82808-175">Maga az IoT-központ nincsenek saját ismeretei az eszköz információk üzenet tárolt hello metaadatok és a folyamatok hello hello üzenet ugyanúgy bármely eszközről a felhőbe üzenetet feldolgozza.</span><span class="sxs-lookup"><span data-stu-id="82808-175">IoT Hub itself has no knowledge of hello metadata contained in a device information message and processes hello message in hello same way it processes any device-to-cloud message.</span></span> <span data-ttu-id="82808-176">A távoli felügyeleti megoldás, hello egy [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) feladat köszönőüzenetei olvassa be az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="82808-176">In hello remote monitoring solution, an [Azure Stream Analytics][lnk-stream-analytics] (ASA) job reads hello messages from IoT Hub.</span></span> <span data-ttu-id="82808-177">Hello **deviceinfo információja** stream analytics-feladat tartalmazó üzenetek szűrők **"Objektumtípus": "Deviceinfo információja"** , majd továbbítja azokat toohello **EventProcessorHost** állomás a webes projekt futtató példányhoz.</span><span class="sxs-lookup"><span data-stu-id="82808-177">hello **DeviceInfo** stream analytics job filters for messages that contain **"ObjectType": "DeviceInfo"** and forwards them toohello **EventProcessorHost** host instance that runs in a web job.</span></span> <span data-ttu-id="82808-178">A logikai hello **EventProcessorHost** példány hello adott eszköz és a frissítés hello rekord hello azonosító toofind hello Cosmos DB rekordokat használ.</span><span class="sxs-lookup"><span data-stu-id="82808-178">Logic in hello **EventProcessorHost** instance uses hello device id toofind hello Cosmos DB record for hello specific device and update hello record.</span></span>

> [!NOTE]
> <span data-ttu-id="82808-179">Egy eszköz információs üzenet jelenik meg egy szabványos eszközről a felhőbe üzenet.</span><span class="sxs-lookup"><span data-stu-id="82808-179">A device information message is a standard device-to-cloud message.</span></span> <span data-ttu-id="82808-180">hello megoldás különböztet eszköz információs üzenetet és telemetriai üzenetek ASA lekérdezések használatával.</span><span class="sxs-lookup"><span data-stu-id="82808-180">hello solution distinguishes between device information messages and telemetry messages by using ASA queries.</span></span>

## <a name="next-steps"></a><span data-ttu-id="82808-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82808-181">Next steps</span></span>

<span data-ttu-id="82808-182">Miután befejezte a tanulási, hogyan szabhatja testre előre konfigurált hello megoldások, ismerje meg a néhány hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:</span><span class="sxs-lookup"><span data-stu-id="82808-182">Now you've finished learning how you can customize hello preconfigured solutions, you can explore some of hello other features and capabilities of hello IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="82808-183">[Előre konfigurált prediktív karbantartási megoldás áttekintése][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="82808-183">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* <span data-ttu-id="82808-184">[Gyakran ismételt kérdések az IoT Suite-ról][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="82808-184">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="82808-185">[A hello IoT biztonsági szabad][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="82808-185">[IoT security from hello ground up][lnk-security-groundup]</span></span>

<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
