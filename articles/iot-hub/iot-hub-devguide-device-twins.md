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
# <a name="understand-and-use-device-twins-in-iot-hub"></a><span data-ttu-id="51d83-103">Ismertetés és az IoT Hub eszköz twins használata</span><span class="sxs-lookup"><span data-stu-id="51d83-103">Understand and use device twins in IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="51d83-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="51d83-104">Overview</span></span>
<span data-ttu-id="51d83-105">*Eszköz twins* tárolása eszköz állapota (a metaadatok, a konfigurációk és a feltételek) JSON-dokumentumok vannak.</span><span class="sxs-lookup"><span data-stu-id="51d83-105">*Device twins* are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="51d83-106">Az IoT-központ továbbra is fennáll, az egyes eszközök, hogy tooIoT központi csatlakozás egy eszköz iker.</span><span class="sxs-lookup"><span data-stu-id="51d83-106">IoT Hub persists a device twin for each device that you connect tooIoT Hub.</span></span> <span data-ttu-id="51d83-107">Ez a cikk ismerteti:</span><span class="sxs-lookup"><span data-stu-id="51d83-107">This article describes:</span></span>

* <span data-ttu-id="51d83-108">hello eszköz iker szerkezete hello: *címkék*, *kívánt* és *tulajdonságok jelentett*, és</span><span class="sxs-lookup"><span data-stu-id="51d83-108">hello structure of hello device twin: *tags*, *desired* and *reported properties*, and</span></span>
* <span data-ttu-id="51d83-109">hello eszközeinek alkalmazásait és hátsó végpontok eszköz twins hajthat végre műveleteket.</span><span class="sxs-lookup"><span data-stu-id="51d83-109">hello operations that device apps and back ends can perform on device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="51d83-110">Eszköz twins jelenleg csak az eszközök, tooIoT központi csatlakozás elérhető hello MQTT protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="51d83-110">Currently, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span> <span data-ttu-id="51d83-111">Tekintse meg a toohello [MQTT támogatási] [ lnk-devguide-mqtt] a cikk útmutatást tooconvert meglévő eszköz alkalmazás toouse MQTT.</span><span class="sxs-lookup"><span data-stu-id="51d83-111">Refer toohello [MQTT support][lnk-devguide-mqtt] article for instructions on how tooconvert existing device app toouse MQTT.</span></span>
> 
> 

### <a name="when-toouse"></a><span data-ttu-id="51d83-112">Ha toouse</span><span class="sxs-lookup"><span data-stu-id="51d83-112">When toouse</span></span>
<span data-ttu-id="51d83-113">Az eszköz twins használja:</span><span class="sxs-lookup"><span data-stu-id="51d83-113">Use device twins to:</span></span>

* <span data-ttu-id="51d83-114">Hello felhőben tárolt az eszközre vonatkozó metaadatok.</span><span class="sxs-lookup"><span data-stu-id="51d83-114">Store device-specific metadata in hello cloud.</span></span> <span data-ttu-id="51d83-115">Például hello egy Eladóautomata telepítési helyét.</span><span class="sxs-lookup"><span data-stu-id="51d83-115">For example, hello deployment location of a vending machine.</span></span>
* <span data-ttu-id="51d83-116">A jelentés aktuális állapotadatokat például a rendelkezésre álló lehetőségek és az eszköz alkalmazás állapotának.</span><span class="sxs-lookup"><span data-stu-id="51d83-116">Report current state information such as available capabilities and conditions from your device app.</span></span> <span data-ttu-id="51d83-117">Például egy eszköz csatlakoztatott tooyour IoT-központ cellás over vagy Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="51d83-117">For example, a device is connected tooyour IoT hub over cellular or WiFi.</span></span>
* <span data-ttu-id="51d83-118">A szinkronizálás hello állapotának hosszan futó munkafolyamatok eszközalkalmazás és háttér-alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="51d83-118">Synchronize hello state of long-running workflows between device app and back-end app.</span></span> <span data-ttu-id="51d83-119">Például hello megoldás biztonsági end megadja új belső vezérlőprogram verziója tooinstall hello és hello eszköz alkalmazás jelentések hello hello frissítési folyamat különböző szakaszaiban.</span><span class="sxs-lookup"><span data-stu-id="51d83-119">For example, when hello solution back end specifies hello new firmware version tooinstall, and hello device app reports hello various stages of hello update process.</span></span>
* <span data-ttu-id="51d83-120">Az eszköz metaadatait, konfigurációs vagy az állapot lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="51d83-120">Query your device metadata, configuration, or state.</span></span>

<span data-ttu-id="51d83-121">Tekintse meg a túl[eszközről a felhőbe kommunikációs útmutatást] [ lnk-d2c-guidance] jelentett tulajdonságok, az eszköz a felhőbe küldött üzeneteket vagy a fájl feltöltése útmutatót.</span><span class="sxs-lookup"><span data-stu-id="51d83-121">Refer too[Device-to-cloud communication guidance][lnk-d2c-guidance] for guidance on using reported properties, device-to-cloud messages, or file upload.</span></span>
<span data-ttu-id="51d83-122">Tekintse meg a túl[felhő eszközre kommunikációs útmutatást] [ lnk-c2d-guidance] kívánt tulajdonságokkal, a közvetlen módszerek és a felhő-eszközre küldött üzenetek útmutatót.</span><span class="sxs-lookup"><span data-stu-id="51d83-122">Refer too[Cloud-to-device communication guidance][lnk-c2d-guidance] for guidance on using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="device-twins"></a><span data-ttu-id="51d83-123">Eszköz twins</span><span class="sxs-lookup"><span data-stu-id="51d83-123">Device twins</span></span>
<span data-ttu-id="51d83-124">Eszköz twins eszközzel kapcsolatos adatok tárolására, amelyek:</span><span class="sxs-lookup"><span data-stu-id="51d83-124">Device twins store device-related information that:</span></span>

* <span data-ttu-id="51d83-125">Eszköz- és biztonsági végpontok toosynchronize eszköz feltételek és a konfigurációs használhatja.</span><span class="sxs-lookup"><span data-stu-id="51d83-125">Device and back ends can use toosynchronize device conditions and configuration.</span></span>
* <span data-ttu-id="51d83-126">hello megoldás háttérrendszerének tooquery használhatja és cél hosszú futású műveleteket.</span><span class="sxs-lookup"><span data-stu-id="51d83-126">hello solution back end can use tooquery and target long-running operations.</span></span>

<span data-ttu-id="51d83-127">egy eszköz iker hello életciklusát kapcsolódó toohello megfelelő [eszközidentitás][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="51d83-127">hello lifecycle of a device twin is linked toohello corresponding [device identity][lnk-identity].</span></span> <span data-ttu-id="51d83-128">Eszköz twins implicit létrehozása és törlése, amikor egy új eszközidentitás jön létre, vagy az IoT-központ törölt.</span><span class="sxs-lookup"><span data-stu-id="51d83-128">Device twins are implicitly created and deleted when a new device identity is created or deleted in IoT Hub.</span></span>

<span data-ttu-id="51d83-129">Egy eszköz iker tartalmazó JSON-dokumentumhoz:</span><span class="sxs-lookup"><span data-stu-id="51d83-129">A device twin is a JSON document that includes:</span></span>

* <span data-ttu-id="51d83-130">**Címkék**.</span><span class="sxs-lookup"><span data-stu-id="51d83-130">**Tags**.</span></span> <span data-ttu-id="51d83-131">Hello JSON-dokumentum, amely a megoldás háttérrendszeréhez hello szakasz is olvasni és írni.</span><span class="sxs-lookup"><span data-stu-id="51d83-131">A section of hello JSON document that hello solution back end can read from and write to.</span></span> <span data-ttu-id="51d83-132">Címkék olyan nem látható toodevice alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="51d83-132">Tags are not visible toodevice apps.</span></span>
* <span data-ttu-id="51d83-133">**Szükségeskonfiguráció-tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="51d83-133">**Desired properties**.</span></span> <span data-ttu-id="51d83-134">Használható együtt a jelentésben szereplő tulajdonságok toosynchronize eszközök konfigurációját és a feltételek.</span><span class="sxs-lookup"><span data-stu-id="51d83-134">Used along with reported properties toosynchronize device configuration or conditions.</span></span> <span data-ttu-id="51d83-135">Kívánt tulajdonságok csak akkor állítható vissza hello megoldás end és hello eszközalkalmazás tudja olvasni.</span><span class="sxs-lookup"><span data-stu-id="51d83-135">Desired properties can only be set by hello solution back end and can be read by hello device app.</span></span> <span data-ttu-id="51d83-136">hello eszközalkalmazás is szükséges hello tulajdonságok változásainak valós időben értesítést.</span><span class="sxs-lookup"><span data-stu-id="51d83-136">hello device app can also be notified in real time of changes in hello desired properties.</span></span>
* <span data-ttu-id="51d83-137">**Tulajdonságok jelentett**.</span><span class="sxs-lookup"><span data-stu-id="51d83-137">**Reported properties**.</span></span> <span data-ttu-id="51d83-138">Használható együtt a kívánt tulajdonságokkal toosynchronize eszközök konfigurációját és a feltételek.</span><span class="sxs-lookup"><span data-stu-id="51d83-138">Used along with desired properties toosynchronize device configuration or conditions.</span></span> <span data-ttu-id="51d83-139">Jelentett tulajdonságok csak hello eszköz alkalmazás által állítható be és olvashatók és hello megoldás háttérrendszerének kellettek.</span><span class="sxs-lookup"><span data-stu-id="51d83-139">Reported properties can only be set by hello device app and can be read and queried by hello solution back end.</span></span>

<span data-ttu-id="51d83-140">Emellett hello azon hello eszköz iker JSON-dokumentum tartalmaz hello megfelelő eszközidentitás hello tárolja a csak olvasható tulajdonságok hello [identitásjegyzékhez][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="51d83-140">Additionally, hello root of hello device twin JSON document contains hello read-only properties from hello corresponding device identity stored in hello [identity registry][lnk-identity].</span></span>

![][img-twin]

<span data-ttu-id="51d83-141">a következő példa hello egy eszköz iker JSON-dokumentum jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="51d83-141">hello following example shows a device twin JSON document:</span></span>

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

<span data-ttu-id="51d83-142">A legfelső szintű objektum hello hello rendszer tulajdonságai, és a tárolóobjektumok `tags` és mindkét `reported` és `desired` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="51d83-142">In hello root object, are hello system properties, and container objects for `tags` and both `reported` and `desired` properties.</span></span> <span data-ttu-id="51d83-143">Hello `properties` tároló néhány írásvédett elemet tartalmaz (`$metadata`, `$etag`, és `$version`) hello ismertetett [eszköz iker metaadatok] [ lnk-twin-metadata] és [ Egyidejű hozzáférések optimista] [ lnk-concurrency] szakaszok.</span><span class="sxs-lookup"><span data-stu-id="51d83-143">hello `properties` container contains some read-only elements (`$metadata`, `$etag`, and `$version`) described in hello [Device twin metadata][lnk-twin-metadata] and [Optimistic concurrency][lnk-concurrency] sections.</span></span>

### <a name="reported-property-example"></a><span data-ttu-id="51d83-144">Jelentett tulajdonság – példa</span><span class="sxs-lookup"><span data-stu-id="51d83-144">Reported property example</span></span>
<span data-ttu-id="51d83-145">Hello előző példában hello eszköz iker tartalmaz egy `batteryLevel` hello eszköz alkalmazás által jelentett tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="51d83-145">In hello previous example, hello device twin contains a `batteryLevel` property that is reported by hello device app.</span></span> <span data-ttu-id="51d83-146">Ez a tulajdonság lehetővé teszi lehetséges tooquery, és a hello utolsó jelentett akkumulátor szint alapján eszközökön működik.</span><span class="sxs-lookup"><span data-stu-id="51d83-146">This property makes it possible tooquery and operate on devices based on hello last reported battery level.</span></span> <span data-ttu-id="51d83-147">További példák lehetnek hello app jelentéskészítési eszköz eszközképességek vagy kapcsolati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="51d83-147">Other examples include hello device app reporting device capabilities or connectivity options.</span></span>

> [!NOTE]
> <span data-ttu-id="51d83-148">Jelentett tulajdonságok forgatókönyvek, ahol hello megoldás háttérrendszerének utolsó ismert tulajdonság értékének hello iránt érdeklődik egyszerűsítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="51d83-148">Reported properties simplify scenarios where hello solution back end is interested in hello last known value of a property.</span></span> <span data-ttu-id="51d83-149">Használjon [eszköz a felhőbe küldött üzeneteket] [ lnk-d2c] hogy hello megoldás háttérrendszerének tooprocess telemetriát eseménysorozat időbélyegzővel, például a time series hello formában kell-e.</span><span class="sxs-lookup"><span data-stu-id="51d83-149">Use [device-to-cloud messages][lnk-d2c] if hello solution back end needs tooprocess device telemetry in hello form of sequences of timestamped events, such as time series.</span></span>

### <a name="desired-property-example"></a><span data-ttu-id="51d83-150">Kívánt tulajdonság – példa</span><span class="sxs-lookup"><span data-stu-id="51d83-150">Desired property example</span></span>
<span data-ttu-id="51d83-151">Hello előző példában hello `telemetryConfig` eszköz iker szükséges, és a jelentésben szereplő tulajdonságok ehhez az eszközhöz hello megoldás háttérrendszerének és hello eszköz toosynchronize hello telemetriai Alkalmazáskonfiguráció által használt.</span><span class="sxs-lookup"><span data-stu-id="51d83-151">In hello previous example, hello `telemetryConfig` device twin desired and reported properties are used by hello solution back end and hello device app toosynchronize hello telemetry configuration for this device.</span></span> <span data-ttu-id="51d83-152">Példa:</span><span class="sxs-lookup"><span data-stu-id="51d83-152">For example:</span></span>

1. <span data-ttu-id="51d83-153">hello megoldás háttérrendszerének szükséges hello tulajdonság szükséges hello konfigurációs érték beállítása.</span><span class="sxs-lookup"><span data-stu-id="51d83-153">hello solution back end sets hello desired property with hello desired configuration value.</span></span> <span data-ttu-id="51d83-154">Szükségeskonfiguráció-hello tulajdonságkészlet hello dokumentum hello része a következő:</span><span class="sxs-lookup"><span data-stu-id="51d83-154">Here is hello portion of hello document with hello desired property set:</span></span>
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. <span data-ttu-id="51d83-155">hello eszközalkalmazás hello változás azonnal, ha csatlakoztatva, vagy a hello először csatlakoztassa értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="51d83-155">hello device app is notified of hello change immediately if connected, or at hello first reconnect.</span></span> <span data-ttu-id="51d83-156">hello eszközalkalmazás majd jelentést készít hello frissített konfigurációs (vagy hello segítségével hibaállapotot `status` tulajdonság).</span><span class="sxs-lookup"><span data-stu-id="51d83-156">hello device app then reports hello updated configuration (or an error condition using hello `status` property).</span></span> <span data-ttu-id="51d83-157">Itt hello hello része jelentett tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="51d83-157">Here is hello portion of hello reported properties:</span></span>
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. <span data-ttu-id="51d83-158">hello megoldás háttérrendszerének nyomon követheti hello eredményeit hello konfigurációs műveletet több eszközön, az [lekérdezése] [ lnk-query] eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="51d83-158">hello solution back end can track hello results of hello configuration operation across many devices, by [querying][lnk-query] device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="51d83-159">hello előző kódtöredékek példák, olvashatóságát, egyirányú tooencode optimalizálva, egy eszköz konfigurálásának és annak állapotát.</span><span class="sxs-lookup"><span data-stu-id="51d83-159">hello preceding snippets are examples, optimized for readability, of one way tooencode a device configuration and its status.</span></span> <span data-ttu-id="51d83-160">Az IoT-központ nem ír elő egy adott séma hello eszköz iker szükséges, és jelentett hello eszköz twins-tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="51d83-160">IoT Hub does not impose a specific schema for hello device twin desired and reported properties in hello device twins.</span></span>
> 
> 

<span data-ttu-id="51d83-161">Twins toosynchronize hosszú ideig futó műveletek, például a belső vezérlőprogram-frissítésekre is használhatja.</span><span class="sxs-lookup"><span data-stu-id="51d83-161">You can use twins toosynchronize long-running operations such as firmware updates.</span></span> <span data-ttu-id="51d83-162">További információ a hogyan toouse tulajdonságok toosynchronize és egy hosszú ideig futó művelet az eszközön, nyomon követése: [használata szükséges tulajdonságok tooconfigure eszközök][lnk-twin-properties].</span><span class="sxs-lookup"><span data-stu-id="51d83-162">For more information on how toouse properties toosynchronize and track a long running operation across devices, see [Use desired properties tooconfigure devices][lnk-twin-properties].</span></span>

## <a name="back-end-operations"></a><span data-ttu-id="51d83-163">Háttér-műveletek</span><span class="sxs-lookup"><span data-stu-id="51d83-163">Back-end operations</span></span>
<span data-ttu-id="51d83-164">hello megoldás háttérrendszerének működik hello eszköz iker hello atomi műveletek HTTP Protokollon keresztül elérhetővé a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="51d83-164">hello solution back end operates on hello device twin using hello following atomic operations, exposed through HTTP:</span></span>

1. <span data-ttu-id="51d83-165">**Id eszköz két lekérni**. Ez a művelet visszaadja jelentett hello eszköz két dokumentumot, beleértve a címkék, és szükség esetén, és a rendszer tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="51d83-165">**Retrieve device twin by id**. This operation returns hello device twin document, including tags and desired, reported, and system properties.</span></span>
2. <span data-ttu-id="51d83-166">**Részlegesen frissítse az eszköz iker**.</span><span class="sxs-lookup"><span data-stu-id="51d83-166">**Partially update device twin**.</span></span> <span data-ttu-id="51d83-167">Ez a művelet lehetővé teszi a hello megoldás háttér toopartially frissítés hello címkék vagy egy eszköz iker kívánt tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="51d83-167">This operation enables hello solution back end toopartially update hello tags or desired properties in a device twin.</span></span> <span data-ttu-id="51d83-168">hello részleges frissítés, amely ad hozzá vagy bármilyen tulajdonságot JSON-dokumentumként van kifejezve.</span><span class="sxs-lookup"><span data-stu-id="51d83-168">hello partial update is expressed as a JSON document that adds or updates any property.</span></span> <span data-ttu-id="51d83-169">Tulajdonság értéke túl`null` törlődnek.</span><span class="sxs-lookup"><span data-stu-id="51d83-169">Properties set too`null` are removed.</span></span> <span data-ttu-id="51d83-170">hello alábbi példa létrehoz egy új kívánt tulajdonság értékű `{"newProperty": "newValue"}`, felülírja a meglévő értéke hello `existingProperty` rendelkező `"otherNewValue"`, és eltávolítja a `otherOldProperty`.</span><span class="sxs-lookup"><span data-stu-id="51d83-170">hello following example creates a new desired property with value `{"newProperty": "newValue"}`, overwrites hello existing value of `existingProperty` with `"otherNewValue"`, and removes `otherOldProperty`.</span></span> <span data-ttu-id="51d83-171">Nincs más módosításai szükséges tooexisting tulajdonságok vagy a címkék:</span><span class="sxs-lookup"><span data-stu-id="51d83-171">No other changes are made tooexisting desired properties or tags:</span></span>
   
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
3. <span data-ttu-id="51d83-172">**Cserélje ki a kívánt tulajdonságokkal**.</span><span class="sxs-lookup"><span data-stu-id="51d83-172">**Replace desired properties**.</span></span> <span data-ttu-id="51d83-173">Ez a művelet lehetővé teszi, hogy hello megoldás háttér toocompletely felülírja a meglévő kívánt összes tulajdonság és helyettesítse be az új JSON-dokumentum `properties/desired`.</span><span class="sxs-lookup"><span data-stu-id="51d83-173">This operation enables hello solution back end toocompletely overwrite all existing desired properties and substitute a new JSON document for `properties/desired`.</span></span>
4. <span data-ttu-id="51d83-174">**Cserélje le a címkék**.</span><span class="sxs-lookup"><span data-stu-id="51d83-174">**Replace tags**.</span></span> <span data-ttu-id="51d83-175">Ez a művelet lehetővé teszi, hogy hello megoldás háttér toocompletely felülírja a meglévő található összes kódcímkének és helyettesítse be az új JSON-dokumentum `tags`.</span><span class="sxs-lookup"><span data-stu-id="51d83-175">This operation enables hello solution back end toocompletely overwrite all existing tags and substitute a new JSON document for `tags`.</span></span>
5. <span data-ttu-id="51d83-176">**A két értesítéseket**.</span><span class="sxs-lookup"><span data-stu-id="51d83-176">**Receive twin notifications**.</span></span> <span data-ttu-id="51d83-177">Ez a művelet lehetővé teszi, hogy a hello megoldás háttér toobe értesíti, ha a hello iker módosul.</span><span class="sxs-lookup"><span data-stu-id="51d83-177">This operation allows hello solution back end toobe notified when hello twin is modified.</span></span> <span data-ttu-id="51d83-178">toodo Igen, az IoT-megoldásból toocreate útvonal és kell tooset hello adatforrás értéke túl*twinChangeEvents*.</span><span class="sxs-lookup"><span data-stu-id="51d83-178">toodo so, your IoT solution needs toocreate a route and tooset hello Data Source equal too*twinChangeEvents*.</span></span> <span data-ttu-id="51d83-179">Alapértelmezés szerint nincs iker értesítések küldése általában olyan, ez azt jelenti, hogy, hogy nincs ilyen útvonal létezik-e előre.</span><span class="sxs-lookup"><span data-stu-id="51d83-179">By default, no twin notifications are sent, that is, no such routes pre-exist.</span></span> <span data-ttu-id="51d83-180">Ha hello módosítási aránya túl magas, vagy egyéb okból, például a belső hibák, hello IoT-központ, amely tartalmazza az összes módosítás csak egy értesítést küldhet.</span><span class="sxs-lookup"><span data-stu-id="51d83-180">If hello rate of change is too high, or for other reasons, such as internal failures, hello IoT Hub might send only one notification that contains all changes.</span></span> <span data-ttu-id="51d83-181">Ezért, ha az alkalmazásnak megbízható vizsgálati és naplózási az összes köztes állapotok, majd továbbra is ajánlott D2C üzenetek használata.</span><span class="sxs-lookup"><span data-stu-id="51d83-181">So, if your application needs reliable auditing and logging of all intermediate states, then it is still recommended that you use D2C messages.</span></span> <span data-ttu-id="51d83-182">hello iker üzenet tulajdonságait és a szövegtörzs tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="51d83-182">hello twin notification message includes properties, and body.</span></span>

    - <span data-ttu-id="51d83-183">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="51d83-183">Properties</span></span>

    | <span data-ttu-id="51d83-184">Név</span><span class="sxs-lookup"><span data-stu-id="51d83-184">Name</span></span> | <span data-ttu-id="51d83-185">Érték</span><span class="sxs-lookup"><span data-stu-id="51d83-185">Value</span></span> |
    | --- | --- |
    <span data-ttu-id="51d83-186">$content-típus</span><span class="sxs-lookup"><span data-stu-id="51d83-186">$content-type</span></span> | <span data-ttu-id="51d83-187">application/json</span><span class="sxs-lookup"><span data-stu-id="51d83-187">application/json</span></span> |
    <span data-ttu-id="51d83-188">$iothub-enqueuedtime</span><span class="sxs-lookup"><span data-stu-id="51d83-188">$iothub-enqueuedtime</span></span> |  <span data-ttu-id="51d83-189">Ha hello értesítés küldése idő</span><span class="sxs-lookup"><span data-stu-id="51d83-189">Time when hello notification was sent</span></span> |
    <span data-ttu-id="51d83-190">$iothub-üzenet-forrás</span><span class="sxs-lookup"><span data-stu-id="51d83-190">$iothub-message-source</span></span> | <span data-ttu-id="51d83-191">twinChangeEvents</span><span class="sxs-lookup"><span data-stu-id="51d83-191">twinChangeEvents</span></span> |
    <span data-ttu-id="51d83-192">$content-kódolás</span><span class="sxs-lookup"><span data-stu-id="51d83-192">$content-encoding</span></span> | <span data-ttu-id="51d83-193">az UTF-8</span><span class="sxs-lookup"><span data-stu-id="51d83-193">utf-8</span></span> |
    <span data-ttu-id="51d83-194">deviceId</span><span class="sxs-lookup"><span data-stu-id="51d83-194">deviceId</span></span> | <span data-ttu-id="51d83-195">Hello eszköz azonosítója</span><span class="sxs-lookup"><span data-stu-id="51d83-195">Id of hello device</span></span> |
    <span data-ttu-id="51d83-196">hubName</span><span class="sxs-lookup"><span data-stu-id="51d83-196">hubName</span></span> | <span data-ttu-id="51d83-197">Az IoT-központ nevét</span><span class="sxs-lookup"><span data-stu-id="51d83-197">Name of IoT Hub</span></span> |
    <span data-ttu-id="51d83-198">operationTimestamp</span><span class="sxs-lookup"><span data-stu-id="51d83-198">operationTimestamp</span></span> | <span data-ttu-id="51d83-199">[ISO8601] művelet időbélyegzője</span><span class="sxs-lookup"><span data-stu-id="51d83-199">[ISO8601] timestamp of operation</span></span> |
    <span data-ttu-id="51d83-200">IOT hubbal-üzenet-séma</span><span class="sxs-lookup"><span data-stu-id="51d83-200">iothub-message-schema</span></span> | <span data-ttu-id="51d83-201">deviceLifecycleNotification</span><span class="sxs-lookup"><span data-stu-id="51d83-201">deviceLifecycleNotification</span></span> |
    <span data-ttu-id="51d83-202">opType</span><span class="sxs-lookup"><span data-stu-id="51d83-202">opType</span></span> | <span data-ttu-id="51d83-203">"replaceTwin" vagy "updateTwin"</span><span class="sxs-lookup"><span data-stu-id="51d83-203">"replaceTwin" or "updateTwin"</span></span> |

    <span data-ttu-id="51d83-204">Üzenet Rendszertulajdonságok fűzve előtagként hello `'$'` szimbólum.</span><span class="sxs-lookup"><span data-stu-id="51d83-204">Message system properties are prefixed with hello `'$'` symbol.</span></span>

    - <span data-ttu-id="51d83-205">Törzs</span><span class="sxs-lookup"><span data-stu-id="51d83-205">Body</span></span>
        
    <span data-ttu-id="51d83-206">Ez a szakasz a JSON formátumban minden hello iker módosításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="51d83-206">This section includes all hello twin changes in a JSON format.</span></span> <span data-ttu-id="51d83-207">Hello formátuma azonos azt használja, mint a javítások, hello különbséggel, hogy minden iker szakasz tartalmazhat: címkék, properties.reported, properties.desired és hello "$metadata" elemet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="51d83-207">It uses hello same format as a patch, with hello difference that it can contain all twin sections: tags, properties.reported, properties.desired, and that it contains hello “$metadata” elements.</span></span> <span data-ttu-id="51d83-208">Például:</span><span class="sxs-lookup"><span data-stu-id="51d83-208">For example,</span></span>
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

<span data-ttu-id="51d83-209">Összes hello támogatása az előző operations [egyidejű hozzáférések optimista] [ lnk-concurrency] és hello szükséges **ServiceConnect** engedéllyel, a hello [biztonsági ] [ lnk-security] cikk.</span><span class="sxs-lookup"><span data-stu-id="51d83-209">All hello preceding operations support [Optimistic concurrency][lnk-concurrency] and require hello **ServiceConnect** permission, as defined in hello [Security][lnk-security] article.</span></span>

<span data-ttu-id="51d83-210">Ezenkívül toothese műveletek, hello megoldás biztonsági célból is:</span><span class="sxs-lookup"><span data-stu-id="51d83-210">In addition toothese operations, hello solution back end can:</span></span>

* <span data-ttu-id="51d83-211">Hello eszköz twins hello segítségével lekérdezési SQL-szerű [IoT-központ lekérdezési nyelv][lnk-query].</span><span class="sxs-lookup"><span data-stu-id="51d83-211">Query hello device twins using hello SQL-like [IoT Hub query language][lnk-query].</span></span>
* <span data-ttu-id="51d83-212">Műveletek végzése eszköz twins használatával nagy mennyiségű [feladatok][lnk-jobs].</span><span class="sxs-lookup"><span data-stu-id="51d83-212">Perform operations on large sets of device twins using [jobs][lnk-jobs].</span></span>

## <a name="device-operations"></a><span data-ttu-id="51d83-213">Eszköz műveletek</span><span class="sxs-lookup"><span data-stu-id="51d83-213">Device operations</span></span>
<span data-ttu-id="51d83-214">hello eszközalkalmazás működik hello eszköz iker hello atomi műveletek a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="51d83-214">hello device app operates on hello device twin using hello following atomic operations:</span></span>

1. <span data-ttu-id="51d83-215">**A két eszköz beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="51d83-215">**Retrieve device twin**.</span></span> <span data-ttu-id="51d83-216">Ez a művelet hello eszköz két dokumentumot ad vissza, (beleértve a címkék és a szükséges, a jelentett és a rendszer tulajdonságai) a hello a jelenleg csatlakoztatott eszköz.</span><span class="sxs-lookup"><span data-stu-id="51d83-216">This operation returns hello device twin document (including tags and desired, reported and system properties) for hello currently connected device.</span></span>
2. <span data-ttu-id="51d83-217">**Részlegesen a jelentett tulajdonságainak frissítése**.</span><span class="sxs-lookup"><span data-stu-id="51d83-217">**Partially update reported properties**.</span></span> <span data-ttu-id="51d83-218">A művelet lehetővé teszi, hogy hello részleges frissítés hello a jelentett hello jelenleg csatlakoztatott eszköz tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="51d83-218">This operation enables hello partial update of hello reported properties of hello currently connected device.</span></span> <span data-ttu-id="51d83-219">A művelet által használt hello azonos JSON frissíteni, hogy hello megoldás hátsó felhasználását egy részleges frissítés kívánt tulajdonságok formátumban.</span><span class="sxs-lookup"><span data-stu-id="51d83-219">This operation uses hello same JSON update format that hello solution back end uses for a partial update of desired properties.</span></span>
3. <span data-ttu-id="51d83-220">**Figyelje meg a kívánt tulajdonságokkal**.</span><span class="sxs-lookup"><span data-stu-id="51d83-220">**Observe desired properties**.</span></span> <span data-ttu-id="51d83-221">hello jelenleg csatlakoztatott eszközön választható toobe fordulhat elő, akkor a frissítés szükséges toohello tulajdonságai értesítést.</span><span class="sxs-lookup"><span data-stu-id="51d83-221">hello currently connected device can choose toobe notified of updates toohello desired properties when they happen.</span></span> <span data-ttu-id="51d83-222">hello eszköz megkapja hello hello megoldás háttérrendszerének hajtja végre a frissítést (teljes vagy részleges csere) ugyanolyan formában.</span><span class="sxs-lookup"><span data-stu-id="51d83-222">hello device receives hello same form of update (partial or full replacement) executed by hello solution back end.</span></span>

<span data-ttu-id="51d83-223">Az összes fenti műveletek hello hello szükséges **DeviceConnect** engedéllyel, a hello [biztonsági] [ lnk-security] cikk.</span><span class="sxs-lookup"><span data-stu-id="51d83-223">All hello preceding operations require hello **DeviceConnect** permission, as defined in hello [Security][lnk-security] article.</span></span>

<span data-ttu-id="51d83-224">Hello [Azure IoT-eszközök SDK-k] [ lnk-sdks] révén könnyen toouse hello megelőző sok nyelvekhez és platformokhoz műveletek.</span><span class="sxs-lookup"><span data-stu-id="51d83-224">hello [Azure IoT device SDKs][lnk-sdks] make it easy toouse hello preceding operations from many languages and platforms.</span></span> <span data-ttu-id="51d83-225">További információ az IoT-központ primitívek kívánt tulajdonságokkal szinkronizálás hello részleteit található [eszköz újracsatlakozás folyamat][lnk-reconnection].</span><span class="sxs-lookup"><span data-stu-id="51d83-225">More information on hello details of IoT Hub primitives for desired properties synchronization can be found in [Device reconnection flow][lnk-reconnection].</span></span>

> [!NOTE]
> <span data-ttu-id="51d83-226">Eszköz twins jelenleg csak az eszközök, tooIoT központi csatlakozás elérhető hello MQTT protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="51d83-226">Currently, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="51d83-227">Referencia-témaköreit:</span><span class="sxs-lookup"><span data-stu-id="51d83-227">Reference topics:</span></span>
<span data-ttu-id="51d83-228">hello következő referencia-témakörökre biztosít ellenőrző hozzáférés tooyour IoT-központ további információt.</span><span class="sxs-lookup"><span data-stu-id="51d83-228">hello following reference topics provide you with more information about controlling access tooyour IoT hub.</span></span>

## <a name="tags-and-properties-format"></a><span data-ttu-id="51d83-229">Címkék és a Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="51d83-229">Tags and properties format</span></span>
<span data-ttu-id="51d83-230">Címke, és a jelentett kívánt tulajdonság található a következő korlátozások hello JSON-objektumok:</span><span class="sxs-lookup"><span data-stu-id="51d83-230">Tags, desired, and reported properties are JSON objects with hello following restrictions:</span></span>

* <span data-ttu-id="51d83-231">A JSON-objektumok összes kulcsai kis-és nagybetűket 64 bájt UTF-8 UNICODE karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="51d83-231">All keys in JSON objects are case-sensitive 64 bytes UTF-8 UNICODE strings.</span></span> <span data-ttu-id="51d83-232">Engedélyezett karakterek kizárása UNICODE vezérlőkaraktereket (szegmensek C0 és C1), és `'.'`, `' '`, és `'$'`.</span><span class="sxs-lookup"><span data-stu-id="51d83-232">Allowed characters exclude UNICODE control characters (segments C0 and C1), and `'.'`, `' '`, and `'$'`.</span></span>
* <span data-ttu-id="51d83-233">A következő JSON típusok hello hasznos lehet a JSON-objektumok szereplő összes érték: boolean, számot, karakterlánc, objektum.</span><span class="sxs-lookup"><span data-stu-id="51d83-233">All values in JSON objects can be of hello following JSON types: boolean, number, string, object.</span></span> <span data-ttu-id="51d83-234">Tömbök használata nem megengedett.</span><span class="sxs-lookup"><span data-stu-id="51d83-234">Arrays are not allowed.</span></span>
* <span data-ttu-id="51d83-235">Címkék, kívánt és jelentett tulajdonságok minden JSON-objektumok maximális mélysége 5 rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="51d83-235">All JSON objects in tags, desired, and reported properties can have a maximum depth of 5.</span></span> <span data-ttu-id="51d83-236">Például a következő objektum hello érvénytelen:</span><span class="sxs-lookup"><span data-stu-id="51d83-236">For instance, hello following object is valid:</span></span>

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

* <span data-ttu-id="51d83-237">Az összes karakterlánc-érték nagyobb, mint 512 bájt hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="51d83-237">All string values can be at most 512 bytes in length.</span></span>

## <a name="device-twin-size"></a><span data-ttu-id="51d83-238">Eszköz iker mérete</span><span class="sxs-lookup"><span data-stu-id="51d83-238">Device twin size</span></span>
<span data-ttu-id="51d83-239">Az IoT-központ kikényszeríti egy 8 KB-os méret korlátozás hello értékeit `tags`, `properties/desired`, és `properties/reported`, kivéve a csak olvasható elemeket.</span><span class="sxs-lookup"><span data-stu-id="51d83-239">IoT Hub enforces an 8KB size limitation on hello values of `tags`, `properties/desired`, and `properties/reported`, excluding read-only elements.</span></span>
<span data-ttu-id="51d83-240">hello mérete számított leltár szerint UNICODE kivételével minden karakter karakterek (szegmensek C0 és C1) és a lemezterület ellenőrzése `' '` amikor megjelenik egy karakterlánc-konstansra kívül.</span><span class="sxs-lookup"><span data-stu-id="51d83-240">hello size is computed by counting all characters excluding UNICODE control characters (segments C0 and C1) and space `' '` when it appears outside of a string constant.</span></span>
<span data-ttu-id="51d83-241">Az IoT-központ volna nagyítása hello hello meghaladja a dokumentumok összes művelet elutasítja hiba történt.</span><span class="sxs-lookup"><span data-stu-id="51d83-241">IoT Hub rejects with an error all operations that would increase hello size of those documents above hello limit.</span></span>

## <a name="device-twin-metadata"></a><span data-ttu-id="51d83-242">Eszköz iker metaadatok</span><span class="sxs-lookup"><span data-stu-id="51d83-242">Device twin metadata</span></span>
<span data-ttu-id="51d83-243">Az IoT-központ tárolja a hello időbélyeg hello minden JSON-objektumból, az eszköz a két utolsó frissítésének szükséges tulajdonságok jelentett.</span><span class="sxs-lookup"><span data-stu-id="51d83-243">IoT Hub maintains hello timestamp of hello last update for each JSON object in device twin desired and reported properties.</span></span> <span data-ttu-id="51d83-244">hello időbélyegeket UTC és hello kódolású [ISO8601] formátum `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span><span class="sxs-lookup"><span data-stu-id="51d83-244">hello timestamps are in UTC and encoded in hello [ISO8601] format `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span></span>
<span data-ttu-id="51d83-245">Példa:</span><span class="sxs-lookup"><span data-stu-id="51d83-245">For example:</span></span>

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

<span data-ttu-id="51d83-246">Ez az információ minden szintű (ne csak hello levelek, JSON struktúrában hello) toopreserve frissítések objektum bejegyzéseinek eltávolítása kell tartani.</span><span class="sxs-lookup"><span data-stu-id="51d83-246">This information is kept at every level (not just hello leaves of hello JSON structure) toopreserve updates that remove object keys.</span></span>

## <a name="optimistic-concurrency"></a><span data-ttu-id="51d83-247">Egyidejű hozzáférések optimista</span><span class="sxs-lookup"><span data-stu-id="51d83-247">Optimistic concurrency</span></span>
<span data-ttu-id="51d83-248">Címkék, szükséges, és a Tulajdonságok jelentett összes támogatási hozzáférések optimista.</span><span class="sxs-lookup"><span data-stu-id="51d83-248">Tags, desired, and reported properties all support optimistic concurrency.</span></span>
<span data-ttu-id="51d83-249">Címkék rendelkezik egy ETag megfelelően [RFC7232], amely JSON-megjelenítés hello címke jelöli.</span><span class="sxs-lookup"><span data-stu-id="51d83-249">Tags have an ETag, as per [RFC7232], that represents hello tag's JSON representation.</span></span> <span data-ttu-id="51d83-250">A feltételes frissítési műveletekben hello megoldás háttér tooensure konzisztencia az ETag-EK is használhatja.</span><span class="sxs-lookup"><span data-stu-id="51d83-250">You can use ETags in conditional update operations from hello solution back end tooensure consistency.</span></span>

<span data-ttu-id="51d83-251">Eszköz iker szükséges, és a Tulajdonságok jelentett ETag-EK nem rendelkezik, de rendelkezik egy `$version` érték, amely garantáltan toobe növekményes.</span><span class="sxs-lookup"><span data-stu-id="51d83-251">Device twin desired and reported properties do not have ETags, but have a `$version` value that is guaranteed toobe incremental.</span></span> <span data-ttu-id="51d83-252">Hasonlóképpen tooan ETag, hello verzió frissítése fél tooenforce konzisztencia frissítések hello használható.</span><span class="sxs-lookup"><span data-stu-id="51d83-252">Similarly tooan ETag, hello version can be used by hello updating party tooenforce consistency of updates.</span></span> <span data-ttu-id="51d83-253">Például egy eszköz-alkalmazás egy jelentett tulajdonság vagy hello megoldás háttérrendszerének kívánt tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="51d83-253">For example, a device app for a reported property or hello solution back end for a desired property.</span></span>

<span data-ttu-id="51d83-254">Verziók is hasznosak, ha (például hello eszközalkalmazás szükséges hello tulajdonságok betartásával) observing ügynök fajok lekérése művelet eredménye hello és frissítési értesítést között kell feloldani.</span><span class="sxs-lookup"><span data-stu-id="51d83-254">Versions are also useful when an observing agent (such as hello device app observing hello desired properties) must reconcile races between hello result of a retrieve operation and an update notification.</span></span> <span data-ttu-id="51d83-255">a szakasz hello [eszköz újracsatlakozás folyamat] [ lnk-reconnection] nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="51d83-255">hello section [Device reconnection flow][lnk-reconnection] provides more information.</span></span>

## <a name="device-reconnection-flow"></a><span data-ttu-id="51d83-256">Eszköz újracsatlakozás folyamata</span><span class="sxs-lookup"><span data-stu-id="51d83-256">Device reconnection flow</span></span>
<span data-ttu-id="51d83-257">Az IoT-központ nem őrzi meg a kívánt tulajdonságokkal frissítési értesítések leválasztott eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="51d83-257">IoT Hub does not preserve desired properties update notifications for disconnected devices.</span></span> <span data-ttu-id="51d83-258">Ez azt jelenti, hogy a csatlakozó eszköz hello teljes kívánt tulajdonságokat tartalmazó dokumentum, a frissítési értesítések továbbá toosubscribing kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="51d83-258">It follows that a device that is connecting must retrieve hello full desired properties document, in addition toosubscribing for update notifications.</span></span> <span data-ttu-id="51d83-259">Frissítési értesítések és a teljes visszaállításhoz közötti fajok hello lehetőséget biztosítani, a következő folyamat hello biztosítani kell:</span><span class="sxs-lookup"><span data-stu-id="51d83-259">Given hello possibility of races between update notifications and full retrieval, hello following flow must be ensured:</span></span>

1. <span data-ttu-id="51d83-260">Eszköz alkalmazásának tooan IoT-központ csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="51d83-260">Device app connects tooan IoT hub.</span></span>
2. <span data-ttu-id="51d83-261">Eszköz alkalmazás frissítési értesítések előfizet a kívánt tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="51d83-261">Device app subscribes for desired properties update notifications.</span></span>
3. <span data-ttu-id="51d83-262">Eszköz alkalmazás lekéri a hello teljes dokumentumot a kívánt tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="51d83-262">Device app retrieves hello full document for desired properties.</span></span>

<span data-ttu-id="51d83-263">hello eszközalkalmazás figyelmen kívül hagyhatja az összes értesítésben `$version` kisebb vagy egyenlő, mint hello teljes lekérdezése dokumentum hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="51d83-263">hello device app can ignore all notifications with `$version` less or equal than hello version of hello full retrieved document.</span></span> <span data-ttu-id="51d83-264">Ez a megközelítés lehetőség, mert az IoT-központ biztosítja, hogy verziók mindig növelhető.</span><span class="sxs-lookup"><span data-stu-id="51d83-264">This approach is possible because IoT Hub guarantees that versions always increment.</span></span>

> [!NOTE]
> <span data-ttu-id="51d83-265">A működési elvet tagot már megvalósította a hello [Azure IoT-eszközök SDK-k][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="51d83-265">This logic is already implemented in hello [Azure IoT device SDKs][lnk-sdks].</span></span> <span data-ttu-id="51d83-266">A leírást akkor hasznos, csak akkor, ha hello eszköz alkalmazás az Azure IoT-eszközök SDK-k nem használható, és közvetlenül kell program hello MQTT felületet.</span><span class="sxs-lookup"><span data-stu-id="51d83-266">This description is useful only if hello device app cannot use any of Azure IoT device SDKs and must program hello MQTT interface directly.</span></span>
> 
> 

## <a name="additional-reference-material"></a><span data-ttu-id="51d83-267">További referenciaanyag</span><span class="sxs-lookup"><span data-stu-id="51d83-267">Additional reference material</span></span>
<span data-ttu-id="51d83-268">Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="51d83-268">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="51d83-269">Hello [IoT-központok végpontjai] [ lnk-endpoints] cikkből hello minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontokhoz.</span><span class="sxs-lookup"><span data-stu-id="51d83-269">hello [IoT Hub endpoints][lnk-endpoints] article describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="51d83-270">Hello [sávszélesség-szabályozási és kvóták] [ lnk-quotas] cikkből hello kvóták toohello IoT-központ szolgáltatás és szabályozási viselkedés tooexpect hello hello szolgáltatás használatakor.</span><span class="sxs-lookup"><span data-stu-id="51d83-270">hello [Throttling and quotas][lnk-quotas] article describes hello quotas that apply toohello IoT Hub service and hello throttling behavior tooexpect when you use hello service.</span></span>
* <span data-ttu-id="51d83-271">Hello [Azure IoT-eszközök és szolgáltatások SDK] [ lnk-sdks] cikk listák hello használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal való fejlesztésekor SDK különböző nyelvi csomagok.</span><span class="sxs-lookup"><span data-stu-id="51d83-271">hello [Azure IoT device and service SDKs][lnk-sdks] article lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="51d83-272">Hello [IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] cikkből hello tooretrieve IoT Hub-ből származó adataival az eszköz twins és feladatok is használhatja az IoT-központ lekérdezési nyelv .</span><span class="sxs-lookup"><span data-stu-id="51d83-272">hello [IoT Hub query language for device twins, jobs, and message routing][lnk-query] article describes hello IoT Hub query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="51d83-273">Hello [IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] cikk hello MQTT protokoll IoT Hub-támogatással kapcsolatos további információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="51d83-273">hello [IoT Hub MQTT support][lnk-devguide-mqtt] article provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51d83-274">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51d83-274">Next steps</span></span>
<span data-ttu-id="51d83-275">Most eszköz twins megismerte esetleg érdekli hello IoT Hub fejlesztői útmutató témakörei a következő:</span><span class="sxs-lookup"><span data-stu-id="51d83-275">Now you have learned about device twins, you may be interested in hello following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="51d83-276">[Az eszközön közvetlen metódus][lnk-methods]</span><span class="sxs-lookup"><span data-stu-id="51d83-276">[Invoke a direct method on a device][lnk-methods]</span></span>
* <span data-ttu-id="51d83-277">[Több eszközön feladatok ütemezése][lnk-jobs]</span><span class="sxs-lookup"><span data-stu-id="51d83-277">[Schedule jobs on multiple devices][lnk-jobs]</span></span>

<span data-ttu-id="51d83-278">Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ oktatóanyagok következő hello iránt érdeklődik:</span><span class="sxs-lookup"><span data-stu-id="51d83-278">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorials:</span></span>

* <span data-ttu-id="51d83-279">[Hogyan toouse hello eszköz iker][lnk-twin-tutorial]</span><span class="sxs-lookup"><span data-stu-id="51d83-279">[How toouse hello device twin][lnk-twin-tutorial]</span></span>
* <span data-ttu-id="51d83-280">[Hogyan toouse eszköz iker tulajdonságai][lnk-twin-properties]</span><span class="sxs-lookup"><span data-stu-id="51d83-280">[How toouse device twin properties][lnk-twin-properties]</span></span>

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
