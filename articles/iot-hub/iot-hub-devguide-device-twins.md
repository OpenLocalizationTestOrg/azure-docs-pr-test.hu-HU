---
title: "Azure IoT Hub eszköz twins megértése |} Microsoft Docs"
description: "Fejlesztői útmutató - használata eszköz twins állapotot és a konfigurációs adatokat az IoT-központ és az eszközök közötti szinkronizálása"
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
ms.openlocfilehash: b316aa419d558547f90a914a22fb29935076de21
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a><span data-ttu-id="31b27-103">Ismertetés és az IoT Hub eszköz twins használata</span><span class="sxs-lookup"><span data-stu-id="31b27-103">Understand and use device twins in IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="31b27-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="31b27-104">Overview</span></span>
<span data-ttu-id="31b27-105">*Eszköz twins* tárolása eszköz állapota (a metaadatok, a konfigurációk és a feltételek) JSON-dokumentumok vannak.</span><span class="sxs-lookup"><span data-stu-id="31b27-105">*Device twins* are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="31b27-106">Az IoT Hub a rácsatlakoztatott minden egyes eszközhöz fenntart egy ikereszközt.</span><span class="sxs-lookup"><span data-stu-id="31b27-106">IoT Hub persists a device twin for each device that you connect to IoT Hub.</span></span> <span data-ttu-id="31b27-107">Ez a cikk ismerteti:</span><span class="sxs-lookup"><span data-stu-id="31b27-107">This article describes:</span></span>

* <span data-ttu-id="31b27-108">Az eszköz iker szerkezete: *címkék*, *kívánt* és *tulajdonságok jelentett*, és</span><span class="sxs-lookup"><span data-stu-id="31b27-108">The structure of the device twin: *tags*, *desired* and *reported properties*, and</span></span>
* <span data-ttu-id="31b27-109">Eszköz twins eszközeinek alkalmazásait és hátsó akkor ér véget végezhető műveletek.</span><span class="sxs-lookup"><span data-stu-id="31b27-109">The operations that device apps and back ends can perform on device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="31b27-110">Eszköz twins jelenleg csak a MQTT protokollal IoT-központ csatlakozó eszközökön érhető el.</span><span class="sxs-lookup"><span data-stu-id="31b27-110">Currently, device twins are accessible only from devices that connect to IoT Hub using the MQTT protocol.</span></span> <span data-ttu-id="31b27-111">Tekintse meg a [MQTT támogatási] [ lnk-devguide-mqtt] miként a meglévő eszköz alkalmazásának módját MQTT használatára.</span><span class="sxs-lookup"><span data-stu-id="31b27-111">Refer to the [MQTT support][lnk-devguide-mqtt] article for instructions on how to convert existing device app to use MQTT.</span></span>
> 
> 

### <a name="when-to-use"></a><span data-ttu-id="31b27-112">A következő esetekben használja</span><span class="sxs-lookup"><span data-stu-id="31b27-112">When to use</span></span>
<span data-ttu-id="31b27-113">Az eszköz twins használja:</span><span class="sxs-lookup"><span data-stu-id="31b27-113">Use device twins to:</span></span>

* <span data-ttu-id="31b27-114">A felhőben tárolt eszközre vonatkozó metaadatok.</span><span class="sxs-lookup"><span data-stu-id="31b27-114">Store device-specific metadata in the cloud.</span></span> <span data-ttu-id="31b27-115">Például a központi telepítési helye a Eladóautomata.</span><span class="sxs-lookup"><span data-stu-id="31b27-115">For example, the deployment location of a vending machine.</span></span>
* <span data-ttu-id="31b27-116">A jelentés aktuális állapotadatokat például a rendelkezésre álló lehetőségek és az eszköz alkalmazás állapotának.</span><span class="sxs-lookup"><span data-stu-id="31b27-116">Report current state information such as available capabilities and conditions from your device app.</span></span> <span data-ttu-id="31b27-117">Például egy eszköz csatlakozik az IoT hub cellás over vagy Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="31b27-117">For example, a device is connected to your IoT hub over cellular or WiFi.</span></span>
* <span data-ttu-id="31b27-118">Hosszan futó munkafolyamatok közötti eszközalkalmazás és háttér-alkalmazás állapotának szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="31b27-118">Synchronize the state of long-running workflows between device app and back-end app.</span></span> <span data-ttu-id="31b27-119">Például a megoldás biztonsági end határozza meg az új belső vezérlőprogram verziójának telepítéséhez, és az eszköz alkalmazás jelentés a frissítési folyamat különböző szakaszaiban.</span><span class="sxs-lookup"><span data-stu-id="31b27-119">For example, when the solution back end specifies the new firmware version to install, and the device app reports the various stages of the update process.</span></span>
* <span data-ttu-id="31b27-120">Az eszköz metaadatait, konfigurációs vagy az állapot lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="31b27-120">Query your device metadata, configuration, or state.</span></span>

<span data-ttu-id="31b27-121">Tekintse meg [eszközről a felhőbe kommunikációs útmutatást] [ lnk-d2c-guidance] jelentett tulajdonságok, az eszköz a felhőbe küldött üzeneteket vagy a fájl feltöltése útmutatót.</span><span class="sxs-lookup"><span data-stu-id="31b27-121">Refer to [Device-to-cloud communication guidance][lnk-d2c-guidance] for guidance on using reported properties, device-to-cloud messages, or file upload.</span></span>
<span data-ttu-id="31b27-122">Tekintse meg [felhő eszközre kommunikációs útmutatást] [ lnk-c2d-guidance] kívánt tulajdonságokkal, a közvetlen módszerek és a felhő-eszközre küldött üzenetek útmutatót.</span><span class="sxs-lookup"><span data-stu-id="31b27-122">Refer to [Cloud-to-device communication guidance][lnk-c2d-guidance] for guidance on using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="device-twins"></a><span data-ttu-id="31b27-123">Eszköz twins</span><span class="sxs-lookup"><span data-stu-id="31b27-123">Device twins</span></span>
<span data-ttu-id="31b27-124">Eszköz twins eszközzel kapcsolatos adatok tárolására, amelyek:</span><span class="sxs-lookup"><span data-stu-id="31b27-124">Device twins store device-related information that:</span></span>

* <span data-ttu-id="31b27-125">Eszköz- és biztonsági végpontok használatával szinkronizálhatja a eszköz feltételek és a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="31b27-125">Device and back ends can use to synchronize device conditions and configuration.</span></span>
* <span data-ttu-id="31b27-126">A megoldás háttérrendszeréhez segítségével lekérdezés és a cél hosszú futású műveleteket.</span><span class="sxs-lookup"><span data-stu-id="31b27-126">The solution back end can use to query and target long-running operations.</span></span>

<span data-ttu-id="31b27-127">Egy eszköz iker életciklusát kapcsolódik a megfelelő [eszközidentitás][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="31b27-127">The lifecycle of a device twin is linked to the corresponding [device identity][lnk-identity].</span></span> <span data-ttu-id="31b27-128">Eszköz twins implicit létrehozása és törlése, amikor egy új eszközidentitás jön létre, vagy az IoT-központ törölt.</span><span class="sxs-lookup"><span data-stu-id="31b27-128">Device twins are implicitly created and deleted when a new device identity is created or deleted in IoT Hub.</span></span>

<span data-ttu-id="31b27-129">Egy eszköz iker tartalmazó JSON-dokumentumhoz:</span><span class="sxs-lookup"><span data-stu-id="31b27-129">A device twin is a JSON document that includes:</span></span>

* <span data-ttu-id="31b27-130">**Címkék**.</span><span class="sxs-lookup"><span data-stu-id="31b27-130">**Tags**.</span></span> <span data-ttu-id="31b27-131">A JSON-dokumentum, amely a megoldás háttérrendszeréhez olvasni és írni egy része.</span><span class="sxs-lookup"><span data-stu-id="31b27-131">A section of the JSON document that the solution back end can read from and write to.</span></span> <span data-ttu-id="31b27-132">Címkék nem láthatók az eszközön futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="31b27-132">Tags are not visible to device apps.</span></span>
* <span data-ttu-id="31b27-133">**Szükségeskonfiguráció-tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="31b27-133">**Desired properties**.</span></span> <span data-ttu-id="31b27-134">Eszközök konfigurálása és a feltételek szinkronizálásához használt jelentett tulajdonságok együtt.</span><span class="sxs-lookup"><span data-stu-id="31b27-134">Used along with reported properties to synchronize device configuration or conditions.</span></span> <span data-ttu-id="31b27-135">Kívánt tulajdonságok csak akkor állítható vissza a megoldás end, és az eszköz alkalmazás által is olvasható.</span><span class="sxs-lookup"><span data-stu-id="31b27-135">Desired properties can only be set by the solution back end and can be read by the device app.</span></span> <span data-ttu-id="31b27-136">Az eszköz alkalmazás is a kívánt tulajdonságokkal változásainak valós időben értesítést.</span><span class="sxs-lookup"><span data-stu-id="31b27-136">The device app can also be notified in real time of changes in the desired properties.</span></span>
* <span data-ttu-id="31b27-137">**Tulajdonságok jelentett**.</span><span class="sxs-lookup"><span data-stu-id="31b27-137">**Reported properties**.</span></span> <span data-ttu-id="31b27-138">Eszközök konfigurálása és feltételek szinkronizálásához használni kívánt tulajdonságokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="31b27-138">Used along with desired properties to synchronize device configuration or conditions.</span></span> <span data-ttu-id="31b27-139">Jelentett tulajdonságok csak az eszköz alkalmazás által állítható be és olvashatók és a megoldás háttérrendszeréhez kellettek.</span><span class="sxs-lookup"><span data-stu-id="31b27-139">Reported properties can only be set by the device app and can be read and queried by the solution back end.</span></span>

<span data-ttu-id="31b27-140">Emellett az eszköz iker JSON-dokumentum gyökerébe tartalmazza a megfelelő eszközidentitás tárolja a csak olvasható tulajdonságok a [identitásjegyzékhez][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="31b27-140">Additionally, the root of the device twin JSON document contains the read-only properties from the corresponding device identity stored in the [identity registry][lnk-identity].</span></span>

![][img-twin]

<span data-ttu-id="31b27-141">A következő példa bemutatja egy eszköz iker JSON-dokumentum:</span><span class="sxs-lookup"><span data-stu-id="31b27-141">The following example shows a device twin JSON document:</span></span>

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

<span data-ttu-id="31b27-142">A legfelső szintű objektum, a rendszer tulajdonságai, és a tárolóobjektumok `tags` és mindkét `reported` és `desired` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="31b27-142">In the root object, are the system properties, and container objects for `tags` and both `reported` and `desired` properties.</span></span> <span data-ttu-id="31b27-143">A `properties` tároló néhány írásvédett elemet tartalmaz (`$metadata`, `$etag`, és `$version`) ismertetett a [eszköz iker metaadatok] [ lnk-twin-metadata] és [egyidejű hozzáférések optimista] [ lnk-concurrency] szakaszok.</span><span class="sxs-lookup"><span data-stu-id="31b27-143">The `properties` container contains some read-only elements (`$metadata`, `$etag`, and `$version`) described in the [Device twin metadata][lnk-twin-metadata] and [Optimistic concurrency][lnk-concurrency] sections.</span></span>

### <a name="reported-property-example"></a><span data-ttu-id="31b27-144">Jelentett tulajdonság – példa</span><span class="sxs-lookup"><span data-stu-id="31b27-144">Reported property example</span></span>
<span data-ttu-id="31b27-145">Az előző példában az eszköz iker tartalmaz egy `batteryLevel` az eszköz alkalmazás által jelentett tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="31b27-145">In the previous example, the device twin contains a `batteryLevel` property that is reported by the device app.</span></span> <span data-ttu-id="31b27-146">Ez a tulajdonság lekérdezése, és az utolsó jelentett akkumulátor szint alapján eszközökön működik lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="31b27-146">This property makes it possible to query and operate on devices based on the last reported battery level.</span></span> <span data-ttu-id="31b27-147">További példák lehetnek az alkalmazás jelentéskészítési eszköz eszközképességek vagy kapcsolati lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="31b27-147">Other examples include the device app reporting device capabilities or connectivity options.</span></span>

> [!NOTE]
> <span data-ttu-id="31b27-148">Jelentett tulajdonságok forgatókönyvek, amelyben az utolsó ismert tulajdonság értéke a megoldás háttérrendszeréhez iránt érdeklődik egyszerűsítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="31b27-148">Reported properties simplify scenarios where the solution back end is interested in the last known value of a property.</span></span> <span data-ttu-id="31b27-149">Használjon [eszköz a felhőbe küldött üzeneteket] [ lnk-d2c] hogy kell-e a megoldás háttérrendszeréhez telemetriát eseménysorozat időbélyegzővel, például a time series formájában feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="31b27-149">Use [device-to-cloud messages][lnk-d2c] if the solution back end needs to process device telemetry in the form of sequences of timestamped events, such as time series.</span></span>

### <a name="desired-property-example"></a><span data-ttu-id="31b27-150">Kívánt tulajdonság – példa</span><span class="sxs-lookup"><span data-stu-id="31b27-150">Desired property example</span></span>
<span data-ttu-id="31b27-151">Az előző példában a `telemetryConfig` eszköz iker szükséges, és a jelentett tulajdonságok szinkronizálni a telemetria-konfigurációt, az eszköz a megoldás háttérrendszeréhez, és az eszköz alkalmazás által használt.</span><span class="sxs-lookup"><span data-stu-id="31b27-151">In the previous example, the `telemetryConfig` device twin desired and reported properties are used by the solution back end and the device app to synchronize the telemetry configuration for this device.</span></span> <span data-ttu-id="31b27-152">Példa:</span><span class="sxs-lookup"><span data-stu-id="31b27-152">For example:</span></span>

1. <span data-ttu-id="31b27-153">A megoldás háttérrendszeréhez állítja be a kívánt tulajdonságot a szükségeskonfiguráció-értékkel.</span><span class="sxs-lookup"><span data-stu-id="31b27-153">The solution back end sets the desired property with the desired configuration value.</span></span> <span data-ttu-id="31b27-154">Ez a dokumentum a kívánt tulajdonság értéke:</span><span class="sxs-lookup"><span data-stu-id="31b27-154">Here is the portion of the document with the desired property set:</span></span>
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. <span data-ttu-id="31b27-155">Az eszköz alkalmazásának a változás azonnal, ha létrejött a kapcsolat, vagy első újbóli értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="31b27-155">The device app is notified of the change immediately if connected, or at the first reconnect.</span></span> <span data-ttu-id="31b27-156">Az eszköz alkalmazás majd jelentést készít a frissített konfigurációt (vagy egy hiba feltétel használatával a `status` tulajdonság).</span><span class="sxs-lookup"><span data-stu-id="31b27-156">The device app then reports the updated configuration (or an error condition using the `status` property).</span></span> <span data-ttu-id="31b27-157">A jelentésben szereplő tulajdonságok része a következő:</span><span class="sxs-lookup"><span data-stu-id="31b27-157">Here is the portion of the reported properties:</span></span>
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. <span data-ttu-id="31b27-158">A megoldás háttérrendszeréhez nyomon követheti a konfigurációs művelet eredménye számos eszközön, az [lekérdezése] [ lnk-query] eszköz twins.</span><span class="sxs-lookup"><span data-stu-id="31b27-158">The solution back end can track the results of the configuration operation across many devices, by [querying][lnk-query] device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="31b27-159">Az előző kódtöredékek példák, olvashatóságát, egyik módja egy eszköz konfigurálásának és annak állapotát kódolása optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="31b27-159">The preceding snippets are examples, optimized for readability, of one way to encode a device configuration and its status.</span></span> <span data-ttu-id="31b27-160">Az IoT-központ nem ír elő egy adott séma, az eszköz iker szükséges, és az eszköz twins tulajdonságok jelentett.</span><span class="sxs-lookup"><span data-stu-id="31b27-160">IoT Hub does not impose a specific schema for the device twin desired and reported properties in the device twins.</span></span>
> 
> 

<span data-ttu-id="31b27-161">Twins segítségével szinkronizálása hosszú ideig futó műveletek, például a belső vezérlőprogram-frissítésekre.</span><span class="sxs-lookup"><span data-stu-id="31b27-161">You can use twins to synchronize long-running operations such as firmware updates.</span></span> <span data-ttu-id="31b27-162">Szinkronizálja, és nyomon követéséhez egy hosszú ideig futó művelet az eszközön tulajdonságok használatával további információkért lásd: [használata szükséges eszközök tulajdonságok][lnk-twin-properties].</span><span class="sxs-lookup"><span data-stu-id="31b27-162">For more information on how to use properties to synchronize and track a long running operation across devices, see [Use desired properties to configure devices][lnk-twin-properties].</span></span>

## <a name="back-end-operations"></a><span data-ttu-id="31b27-163">Háttér-műveletek</span><span class="sxs-lookup"><span data-stu-id="31b27-163">Back-end operations</span></span>
<span data-ttu-id="31b27-164">A megoldás háttérrendszeréhez a használatával a következő atomi műveletek, HTTP Protokollon keresztül elérhetővé tett eszközök iker működik:</span><span class="sxs-lookup"><span data-stu-id="31b27-164">The solution back end operates on the device twin using the following atomic operations, exposed through HTTP:</span></span>

1. <span data-ttu-id="31b27-165">**Id eszköz két lekérni**. Ez a művelet adja vissza a eszköz két dokumentumot, beleértve a címkék, és szükség esetén jelentette, és a rendszer tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="31b27-165">**Retrieve device twin by id**. This operation returns the device twin document, including tags and desired, reported, and system properties.</span></span>
2. <span data-ttu-id="31b27-166">**Részlegesen frissítse az eszköz iker**.</span><span class="sxs-lookup"><span data-stu-id="31b27-166">**Partially update device twin**.</span></span> <span data-ttu-id="31b27-167">Ez a művelet lehetővé teszi, hogy a megoldás háttérrendszeréhez részben frissíteni a címkék vagy egy eszköz iker kívánt tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="31b27-167">This operation enables the solution back end to partially update the tags or desired properties in a device twin.</span></span> <span data-ttu-id="31b27-168">A részleges frissítés, amely ad hozzá vagy bármilyen tulajdonságot JSON-dokumentumként van kifejezve.</span><span class="sxs-lookup"><span data-stu-id="31b27-168">The partial update is expressed as a JSON document that adds or updates any property.</span></span> <span data-ttu-id="31b27-169">Tulajdonságok beállítása `null` törlődnek.</span><span class="sxs-lookup"><span data-stu-id="31b27-169">Properties set to `null` are removed.</span></span> <span data-ttu-id="31b27-170">Az alábbi példa hoz létre egy új kívánt tulajdonság értéke `{"newProperty": "newValue"}`, felülírja a meglévő értéket az `existingProperty` rendelkező `"otherNewValue"`, és eltávolítja a `otherOldProperty`.</span><span class="sxs-lookup"><span data-stu-id="31b27-170">The following example creates a new desired property with value `{"newProperty": "newValue"}`, overwrites the existing value of `existingProperty` with `"otherNewValue"`, and removes `otherOldProperty`.</span></span> <span data-ttu-id="31b27-171">Nincs más módosításai meglévő kívánt tulajdonságok vagy a címkék:</span><span class="sxs-lookup"><span data-stu-id="31b27-171">No other changes are made to existing desired properties or tags:</span></span>
   
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
3. <span data-ttu-id="31b27-172">**Cserélje ki a kívánt tulajdonságokkal**.</span><span class="sxs-lookup"><span data-stu-id="31b27-172">**Replace desired properties**.</span></span> <span data-ttu-id="31b27-173">Ez a művelet lehetővé teszi, hogy a megoldás háttérrendszeréhez teljesen felülírja az összes meglévő kívánt tulajdonságokat, és helyettesítse be az új JSON-dokumentum `properties/desired`.</span><span class="sxs-lookup"><span data-stu-id="31b27-173">This operation enables the solution back end to completely overwrite all existing desired properties and substitute a new JSON document for `properties/desired`.</span></span>
4. <span data-ttu-id="31b27-174">**Cserélje le a címkék**.</span><span class="sxs-lookup"><span data-stu-id="31b27-174">**Replace tags**.</span></span> <span data-ttu-id="31b27-175">Ez a művelet lehetővé teszi, hogy a megoldás háttérrendszeréhez teljesen felülírja az összes meglévő címkék és helyettesítse be az új JSON-dokumentum `tags`.</span><span class="sxs-lookup"><span data-stu-id="31b27-175">This operation enables the solution back end to completely overwrite all existing tags and substitute a new JSON document for `tags`.</span></span>
5. <span data-ttu-id="31b27-176">**A két értesítéseket**.</span><span class="sxs-lookup"><span data-stu-id="31b27-176">**Receive twin notifications**.</span></span> <span data-ttu-id="31b27-177">Ez a művelet lehetővé teszi, hogy a megoldás háttérrendszeréhez értesíti, ha a két módosul.</span><span class="sxs-lookup"><span data-stu-id="31b27-177">This operation allows the solution back end to be notified when the twin is modified.</span></span> <span data-ttu-id="31b27-178">Ehhez az IoT-megoldásból egy útvonal létrehozása és kell beállítani az adatforrás egyenlő *twinChangeEvents*.</span><span class="sxs-lookup"><span data-stu-id="31b27-178">To do so, your IoT solution needs to create a route and to set the Data Source equal to *twinChangeEvents*.</span></span> <span data-ttu-id="31b27-179">Alapértelmezés szerint nincs iker értesítések küldése általában olyan, ez azt jelenti, hogy, hogy nincs ilyen útvonal létezik-e előre.</span><span class="sxs-lookup"><span data-stu-id="31b27-179">By default, no twin notifications are sent, that is, no such routes pre-exist.</span></span> <span data-ttu-id="31b27-180">Ha a módosítási aránya túl magas, vagy más okok miatt, például a belső hibák, az IoT Hub el tudja küldeni csak egy értesítést, amely tartalmazza az összes módosítást.</span><span class="sxs-lookup"><span data-stu-id="31b27-180">If the rate of change is too high, or for other reasons, such as internal failures, the IoT Hub might send only one notification that contains all changes.</span></span> <span data-ttu-id="31b27-181">Ezért, ha az alkalmazásnak megbízható vizsgálati és naplózási az összes köztes állapotok, majd továbbra is ajánlott D2C üzenetek használata.</span><span class="sxs-lookup"><span data-stu-id="31b27-181">So, if your application needs reliable auditing and logging of all intermediate states, then it is still recommended that you use D2C messages.</span></span> <span data-ttu-id="31b27-182">A két üzenet tulajdonságait és a szövegtörzs tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="31b27-182">The twin notification message includes properties, and body.</span></span>

    - <span data-ttu-id="31b27-183">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="31b27-183">Properties</span></span>

    | <span data-ttu-id="31b27-184">Név</span><span class="sxs-lookup"><span data-stu-id="31b27-184">Name</span></span> | <span data-ttu-id="31b27-185">Érték</span><span class="sxs-lookup"><span data-stu-id="31b27-185">Value</span></span> |
    | --- | --- |
    <span data-ttu-id="31b27-186">$content-típus</span><span class="sxs-lookup"><span data-stu-id="31b27-186">$content-type</span></span> | <span data-ttu-id="31b27-187">application/json</span><span class="sxs-lookup"><span data-stu-id="31b27-187">application/json</span></span> |
    <span data-ttu-id="31b27-188">$iothub-enqueuedtime</span><span class="sxs-lookup"><span data-stu-id="31b27-188">$iothub-enqueuedtime</span></span> |  <span data-ttu-id="31b27-189">Ha az értesítés küldése idő</span><span class="sxs-lookup"><span data-stu-id="31b27-189">Time when the notification was sent</span></span> |
    <span data-ttu-id="31b27-190">$iothub-üzenet-forrás</span><span class="sxs-lookup"><span data-stu-id="31b27-190">$iothub-message-source</span></span> | <span data-ttu-id="31b27-191">twinChangeEvents</span><span class="sxs-lookup"><span data-stu-id="31b27-191">twinChangeEvents</span></span> |
    <span data-ttu-id="31b27-192">$content-kódolás</span><span class="sxs-lookup"><span data-stu-id="31b27-192">$content-encoding</span></span> | <span data-ttu-id="31b27-193">az UTF-8</span><span class="sxs-lookup"><span data-stu-id="31b27-193">utf-8</span></span> |
    <span data-ttu-id="31b27-194">deviceId</span><span class="sxs-lookup"><span data-stu-id="31b27-194">deviceId</span></span> | <span data-ttu-id="31b27-195">Az eszköz azonosítója</span><span class="sxs-lookup"><span data-stu-id="31b27-195">Id of the device</span></span> |
    <span data-ttu-id="31b27-196">hubName</span><span class="sxs-lookup"><span data-stu-id="31b27-196">hubName</span></span> | <span data-ttu-id="31b27-197">Az IoT-központ nevét</span><span class="sxs-lookup"><span data-stu-id="31b27-197">Name of IoT Hub</span></span> |
    <span data-ttu-id="31b27-198">operationTimestamp</span><span class="sxs-lookup"><span data-stu-id="31b27-198">operationTimestamp</span></span> | <span data-ttu-id="31b27-199">[ISO8601] művelet időbélyegzője</span><span class="sxs-lookup"><span data-stu-id="31b27-199">[ISO8601] timestamp of operation</span></span> |
    <span data-ttu-id="31b27-200">IOT hubbal-üzenet-séma</span><span class="sxs-lookup"><span data-stu-id="31b27-200">iothub-message-schema</span></span> | <span data-ttu-id="31b27-201">deviceLifecycleNotification</span><span class="sxs-lookup"><span data-stu-id="31b27-201">deviceLifecycleNotification</span></span> |
    <span data-ttu-id="31b27-202">opType</span><span class="sxs-lookup"><span data-stu-id="31b27-202">opType</span></span> | <span data-ttu-id="31b27-203">"replaceTwin" vagy "updateTwin"</span><span class="sxs-lookup"><span data-stu-id="31b27-203">"replaceTwin" or "updateTwin"</span></span> |

    <span data-ttu-id="31b27-204">Üzenet Rendszertulajdonságok fűzve előtagként a `'$'` szimbólum.</span><span class="sxs-lookup"><span data-stu-id="31b27-204">Message system properties are prefixed with the `'$'` symbol.</span></span>

    - <span data-ttu-id="31b27-205">Törzs</span><span class="sxs-lookup"><span data-stu-id="31b27-205">Body</span></span>
        
    <span data-ttu-id="31b27-206">Ez a szakasz a kettős módosításokat tartalmazza JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="31b27-206">This section includes all the twin changes in a JSON format.</span></span> <span data-ttu-id="31b27-207">Ugyanazt a formátumot használja, a javítás, azzal a különbséggel, hogy minden iker szakasz tartalmazhat: címkék, properties.reported, properties.desired és, hogy az tartalmazza-e a "$metadata" elemek.</span><span class="sxs-lookup"><span data-stu-id="31b27-207">It uses the same format as a patch, with the difference that it can contain all twin sections: tags, properties.reported, properties.desired, and that it contains the “$metadata” elements.</span></span> <span data-ttu-id="31b27-208">Például:</span><span class="sxs-lookup"><span data-stu-id="31b27-208">For example,</span></span>
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

<span data-ttu-id="31b27-209">Az előző műveletei támogatják [egyidejű hozzáférések optimista] [ lnk-concurrency] , és van szüksége a **ServiceConnect** engedéllyel, akkor az a [biztonsági] [ lnk-security] cikk.</span><span class="sxs-lookup"><span data-stu-id="31b27-209">All the preceding operations support [Optimistic concurrency][lnk-concurrency] and require the **ServiceConnect** permission, as defined in the [Security][lnk-security] article.</span></span>

<span data-ttu-id="31b27-210">Ezek a műveletek mellett a megoldás háttérrendszeréhez is:</span><span class="sxs-lookup"><span data-stu-id="31b27-210">In addition to these operations, the solution back end can:</span></span>

* <span data-ttu-id="31b27-211">Az eszköz twins használata az SQL-szerű lekérdezése [IoT-központ lekérdezési nyelv][lnk-query].</span><span class="sxs-lookup"><span data-stu-id="31b27-211">Query the device twins using the SQL-like [IoT Hub query language][lnk-query].</span></span>
* <span data-ttu-id="31b27-212">Műveletek végzése eszköz twins használatával nagy mennyiségű [feladatok][lnk-jobs].</span><span class="sxs-lookup"><span data-stu-id="31b27-212">Perform operations on large sets of device twins using [jobs][lnk-jobs].</span></span>

## <a name="device-operations"></a><span data-ttu-id="31b27-213">Eszköz műveletek</span><span class="sxs-lookup"><span data-stu-id="31b27-213">Device operations</span></span>
<span data-ttu-id="31b27-214">Az eszköz alkalmazás végez műveleteket ezeken az eszköz a két használatával a következő atomi műveletek:</span><span class="sxs-lookup"><span data-stu-id="31b27-214">The device app operates on the device twin using the following atomic operations:</span></span>

1. <span data-ttu-id="31b27-215">**A két eszköz beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="31b27-215">**Retrieve device twin**.</span></span> <span data-ttu-id="31b27-216">Ez a művelet a két eszköz dokumentumát adja vissza (beleértve a címkék és a szükséges, a jelentett és a rendszer tulajdonságai) a jelenleg csatlakoztatott eszköz.</span><span class="sxs-lookup"><span data-stu-id="31b27-216">This operation returns the device twin document (including tags and desired, reported and system properties) for the currently connected device.</span></span>
2. <span data-ttu-id="31b27-217">**Részlegesen a jelentett tulajdonságainak frissítése**.</span><span class="sxs-lookup"><span data-stu-id="31b27-217">**Partially update reported properties**.</span></span> <span data-ttu-id="31b27-218">Ez a művelet lehetővé teszi, hogy a jelenleg csatlakoztatott eszköz jelentett tulajdonságait részleges frissítése.</span><span class="sxs-lookup"><span data-stu-id="31b27-218">This operation enables the partial update of the reported properties of the currently connected device.</span></span> <span data-ttu-id="31b27-219">Ez a művelet formátuma JSON frissítés, hogy a megoldás biztonsági egy részleges frissítés kívánt tulajdonságok felhasználását.</span><span class="sxs-lookup"><span data-stu-id="31b27-219">This operation uses the same JSON update format that the solution back end uses for a partial update of desired properties.</span></span>
3. <span data-ttu-id="31b27-220">**Figyelje meg a kívánt tulajdonságokkal**.</span><span class="sxs-lookup"><span data-stu-id="31b27-220">**Observe desired properties**.</span></span> <span data-ttu-id="31b27-221">A jelenleg csatlakoztatott eszközre választhat, ha értesítést szeretne kapni a kívánt tulajdonságokkal frissítések akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="31b27-221">The currently connected device can choose to be notified of updates to the desired properties when they happen.</span></span> <span data-ttu-id="31b27-222">Az eszköz megkapja a frissítés (teljes vagy részleges csere) hajtja végre a megoldás háttérrendszeréhez ugyanolyan formában.</span><span class="sxs-lookup"><span data-stu-id="31b27-222">The device receives the same form of update (partial or full replacement) executed by the solution back end.</span></span>

<span data-ttu-id="31b27-223">Az előző működéséhez szükségesek a **DeviceConnect** engedéllyel, akkor az a [biztonsági] [ lnk-security] cikk.</span><span class="sxs-lookup"><span data-stu-id="31b27-223">All the preceding operations require the **DeviceConnect** permission, as defined in the [Security][lnk-security] article.</span></span>

<span data-ttu-id="31b27-224">A [Azure IoT-eszközök SDK-k] [ lnk-sdks] megkönnyítik a fenti műveleteket az sok nyelvekhez és platformokhoz használatára.</span><span class="sxs-lookup"><span data-stu-id="31b27-224">The [Azure IoT device SDKs][lnk-sdks] make it easy to use the preceding operations from many languages and platforms.</span></span> <span data-ttu-id="31b27-225">További információ az IoT-központ primitívek kívánt tulajdonságokkal szinkronizálás részleteit található [eszköz újracsatlakozás folyamat][lnk-reconnection].</span><span class="sxs-lookup"><span data-stu-id="31b27-225">More information on the details of IoT Hub primitives for desired properties synchronization can be found in [Device reconnection flow][lnk-reconnection].</span></span>

> [!NOTE]
> <span data-ttu-id="31b27-226">Eszköz twins jelenleg csak a MQTT protokollal IoT-központ csatlakozó eszközökön érhető el.</span><span class="sxs-lookup"><span data-stu-id="31b27-226">Currently, device twins are accessible only from devices that connect to IoT Hub using the MQTT protocol.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="31b27-227">Referencia-témaköreit:</span><span class="sxs-lookup"><span data-stu-id="31b27-227">Reference topics:</span></span>
<span data-ttu-id="31b27-228">A következő témaköröket további tudnivalókat az IoT hub hozzáférést biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="31b27-228">The following reference topics provide you with more information about controlling access to your IoT hub.</span></span>

## <a name="tags-and-properties-format"></a><span data-ttu-id="31b27-229">Címkék és a Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="31b27-229">Tags and properties format</span></span>
<span data-ttu-id="31b27-230">Címkék, kívánt és jelentett tulajdonságainak JSON-objektumok a következő korlátozásokkal:</span><span class="sxs-lookup"><span data-stu-id="31b27-230">Tags, desired, and reported properties are JSON objects with the following restrictions:</span></span>

* <span data-ttu-id="31b27-231">A JSON-objektumok összes kulcsai kis-és nagybetűket 64 bájt UTF-8 UNICODE karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="31b27-231">All keys in JSON objects are case-sensitive 64 bytes UTF-8 UNICODE strings.</span></span> <span data-ttu-id="31b27-232">Engedélyezett karakterek kizárása UNICODE vezérlőkaraktereket (szegmensek C0 és C1), és `'.'`, `' '`, és `'$'`.</span><span class="sxs-lookup"><span data-stu-id="31b27-232">Allowed characters exclude UNICODE control characters (segments C0 and C1), and `'.'`, `' '`, and `'$'`.</span></span>
* <span data-ttu-id="31b27-233">JSON-objektumok összes értéke lehet a következő JSON-típusok: boolean, számot, karakterlánc, objektum.</span><span class="sxs-lookup"><span data-stu-id="31b27-233">All values in JSON objects can be of the following JSON types: boolean, number, string, object.</span></span> <span data-ttu-id="31b27-234">Tömbök használata nem megengedett.</span><span class="sxs-lookup"><span data-stu-id="31b27-234">Arrays are not allowed.</span></span>
* <span data-ttu-id="31b27-235">Címkék, kívánt és jelentett tulajdonságok minden JSON-objektumok maximális mélysége 5 rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="31b27-235">All JSON objects in tags, desired, and reported properties can have a maximum depth of 5.</span></span> <span data-ttu-id="31b27-236">Például a következő objektum érvénytelen:</span><span class="sxs-lookup"><span data-stu-id="31b27-236">For instance, the following object is valid:</span></span>

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

* <span data-ttu-id="31b27-237">Az összes karakterlánc-érték nagyobb, mint 512 bájt hosszúságú lehet.</span><span class="sxs-lookup"><span data-stu-id="31b27-237">All string values can be at most 512 bytes in length.</span></span>

## <a name="device-twin-size"></a><span data-ttu-id="31b27-238">Eszköz iker mérete</span><span class="sxs-lookup"><span data-stu-id="31b27-238">Device twin size</span></span>
<span data-ttu-id="31b27-239">Az IoT-központ kikényszeríti egy 8 KB-os méret korlátozás értékeit `tags`, `properties/desired`, és `properties/reported`, kivéve a csak olvasható elemeket.</span><span class="sxs-lookup"><span data-stu-id="31b27-239">IoT Hub enforces an 8KB size limitation on the values of `tags`, `properties/desired`, and `properties/reported`, excluding read-only elements.</span></span>
<span data-ttu-id="31b27-240">A méret számított leltár szerint UNICODE kivételével minden karakter karakterek (szegmensek C0 és C1) és a lemezterület ellenőrzése `' '` amikor megjelenik egy karakterlánc-konstansra kívül.</span><span class="sxs-lookup"><span data-stu-id="31b27-240">The size is computed by counting all characters excluding UNICODE control characters (segments C0 and C1) and space `' '` when it appears outside of a string constant.</span></span>
<span data-ttu-id="31b27-241">Az IoT-központ hibával elutasítja a összes művelet lenne növelje a mérete meghaladja a közzétett dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="31b27-241">IoT Hub rejects with an error all operations that would increase the size of those documents above the limit.</span></span>

## <a name="device-twin-metadata"></a><span data-ttu-id="31b27-242">Eszköz iker metaadatok</span><span class="sxs-lookup"><span data-stu-id="31b27-242">Device twin metadata</span></span>
<span data-ttu-id="31b27-243">Az IoT-központ tárolja a minden JSON-objektumból, az eszköz a két utolsó frissítésének időbélyeg szükséges tulajdonságok jelentett.</span><span class="sxs-lookup"><span data-stu-id="31b27-243">IoT Hub maintains the timestamp of the last update for each JSON object in device twin desired and reported properties.</span></span> <span data-ttu-id="31b27-244">Az időbélyeg helyi időre UTC és kódolású a [ISO8601] formátum `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span><span class="sxs-lookup"><span data-stu-id="31b27-244">The timestamps are in UTC and encoded in the [ISO8601] format `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span></span>
<span data-ttu-id="31b27-245">Példa:</span><span class="sxs-lookup"><span data-stu-id="31b27-245">For example:</span></span>

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

<span data-ttu-id="31b27-246">Ezek az információk megőrzése érdekében távolítsa el a objektum kulcsok frissítések tartják minden szinten (nem csak a JSON struktúrában leaves).</span><span class="sxs-lookup"><span data-stu-id="31b27-246">This information is kept at every level (not just the leaves of the JSON structure) to preserve updates that remove object keys.</span></span>

## <a name="optimistic-concurrency"></a><span data-ttu-id="31b27-247">Egyidejű hozzáférések optimista</span><span class="sxs-lookup"><span data-stu-id="31b27-247">Optimistic concurrency</span></span>
<span data-ttu-id="31b27-248">Címkék, szükséges, és a Tulajdonságok jelentett összes támogatási hozzáférések optimista.</span><span class="sxs-lookup"><span data-stu-id="31b27-248">Tags, desired, and reported properties all support optimistic concurrency.</span></span>
<span data-ttu-id="31b27-249">Címkék rendelkezik egy ETag megfelelően [RFC7232], amely jelzi, hogy a címke JSON-megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="31b27-249">Tags have an ETag, as per [RFC7232], that represents the tag's JSON representation.</span></span> <span data-ttu-id="31b27-250">A megoldás háttérrendszeréhez feltételes frissítési műveletek ETag-EK használatával biztosítják a konzisztenciát.</span><span class="sxs-lookup"><span data-stu-id="31b27-250">You can use ETags in conditional update operations from the solution back end to ensure consistency.</span></span>

<span data-ttu-id="31b27-251">Eszköz iker szükséges, és a Tulajdonságok jelentett ETag-EK nem rendelkezik, de rendelkezik egy `$version` érték, amely garantáltan növekményes.</span><span class="sxs-lookup"><span data-stu-id="31b27-251">Device twin desired and reported properties do not have ETags, but have a `$version` value that is guaranteed to be incremental.</span></span> <span data-ttu-id="31b27-252">Ehhez hasonlóan az egy ETag-gel, a verziója használható a frissítési entitás frissítések konzisztencia érvényesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="31b27-252">Similarly to an ETag, the version can be used by the updating party to enforce consistency of updates.</span></span> <span data-ttu-id="31b27-253">Például egy eszköz alkalmazás jelentett tulajdonság vagy a megoldás háttérrendszeréhez kívánt tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="31b27-253">For example, a device app for a reported property or the solution back end for a desired property.</span></span>

<span data-ttu-id="31b27-254">Verziók is hasznosak, ha (például az eszköz alkalmazás tartják be a kívánt tulajdonságokkal) observing ügynök fajok között egy lekérése művelet eredményét, és a frissítési értesítést kell feloldani.</span><span class="sxs-lookup"><span data-stu-id="31b27-254">Versions are also useful when an observing agent (such as the device app observing the desired properties) must reconcile races between the result of a retrieve operation and an update notification.</span></span> <span data-ttu-id="31b27-255">A szakasz [eszköz újracsatlakozás folyamat] [ lnk-reconnection] nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="31b27-255">The section [Device reconnection flow][lnk-reconnection] provides more information.</span></span>

## <a name="device-reconnection-flow"></a><span data-ttu-id="31b27-256">Eszköz újracsatlakozás folyamata</span><span class="sxs-lookup"><span data-stu-id="31b27-256">Device reconnection flow</span></span>
<span data-ttu-id="31b27-257">Az IoT-központ nem őrzi meg a kívánt tulajdonságokkal frissítési értesítések leválasztott eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="31b27-257">IoT Hub does not preserve desired properties update notifications for disconnected devices.</span></span> <span data-ttu-id="31b27-258">Ez azt jelenti, hogy egy eszköz, amely kapcsolódik a teljes kívánt tulajdonságokat tartalmazó dokumentum, frissítési értesítések való előfizetés mellett kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="31b27-258">It follows that a device that is connecting must retrieve the full desired properties document, in addition to subscribing for update notifications.</span></span> <span data-ttu-id="31b27-259">Frissítési értesítések és a teljes visszaállításhoz közötti fajok lehetőséget biztosítani a következő folyamatot kell biztosítani:</span><span class="sxs-lookup"><span data-stu-id="31b27-259">Given the possibility of races between update notifications and full retrieval, the following flow must be ensured:</span></span>

1. <span data-ttu-id="31b27-260">Eszköz alkalmazásának csatlakozik az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="31b27-260">Device app connects to an IoT hub.</span></span>
2. <span data-ttu-id="31b27-261">Eszköz alkalmazás frissítési értesítések előfizet a kívánt tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="31b27-261">Device app subscribes for desired properties update notifications.</span></span>
3. <span data-ttu-id="31b27-262">Eszköz alkalmazás lekéri a teljes dokumentumot a kívánt tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="31b27-262">Device app retrieves the full document for desired properties.</span></span>

<span data-ttu-id="31b27-263">Az eszköz alkalmazás figyelmen kívül hagyhatja az összes értesítésben `$version` kisebb vagy egyenlő, mint a teljes lekérdezése dokumentum verziója.</span><span class="sxs-lookup"><span data-stu-id="31b27-263">The device app can ignore all notifications with `$version` less or equal than the version of the full retrieved document.</span></span> <span data-ttu-id="31b27-264">Ez a megközelítés lehetőség, mert az IoT-központ biztosítja, hogy verziók mindig növelhető.</span><span class="sxs-lookup"><span data-stu-id="31b27-264">This approach is possible because IoT Hub guarantees that versions always increment.</span></span>

> [!NOTE]
> <span data-ttu-id="31b27-265">Ezt a tagot már megvalósította a [Azure IoT-eszközök SDK-k][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="31b27-265">This logic is already implemented in the [Azure IoT device SDKs][lnk-sdks].</span></span> <span data-ttu-id="31b27-266">Ez a megnevezés akkor hasznos, csak akkor, ha az eszköz alkalmazás az Azure IoT-eszközök SDK-k nem használható, és programot kell közvetlenül a MQTT felületet.</span><span class="sxs-lookup"><span data-stu-id="31b27-266">This description is useful only if the device app cannot use any of Azure IoT device SDKs and must program the MQTT interface directly.</span></span>
> 
> 

## <a name="additional-reference-material"></a><span data-ttu-id="31b27-267">További referenciaanyag</span><span class="sxs-lookup"><span data-stu-id="31b27-267">Additional reference material</span></span>
<span data-ttu-id="31b27-268">Az IoT Hub fejlesztői útmutató más hivatkozás témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="31b27-268">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="31b27-269">A [IoT-központok végpontjai] [ lnk-endpoints] a cikk ismerteti a különböző végpontok, amelyek minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek.</span><span class="sxs-lookup"><span data-stu-id="31b27-269">The [IoT Hub endpoints][lnk-endpoints] article describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="31b27-270">A [sávszélesség-szabályozási és kvóták] [ lnk-quotas] a cikk ismerteti a kvótákat, az IoT-központ szolgáltatás és a sávszélesség-szabályozási viselkedését történik, ha a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="31b27-270">The [Throttling and quotas][lnk-quotas] article describes the quotas that apply to the IoT Hub service and the throttling behavior to expect when you use the service.</span></span>
* <span data-ttu-id="31b27-271">A [Azure IoT-eszközök és szolgáltatások SDK] [ lnk-sdks] cikk felsorolja a különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.</span><span class="sxs-lookup"><span data-stu-id="31b27-271">The [Azure IoT device and service SDKs][lnk-sdks] article lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="31b27-272">A [IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] a cikk ismerteti az IoT-központ lekérdezési nyelv segítségével IoT Hub eszköz twins és feladatok kapcsolatos adatok lekérését.</span><span class="sxs-lookup"><span data-stu-id="31b27-272">The [IoT Hub query language for device twins, jobs, and message routing][lnk-query] article describes the IoT Hub query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="31b27-273">A [IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] cikkben olvashat az IoT-Központ támogatja a MQTT protokollt.</span><span class="sxs-lookup"><span data-stu-id="31b27-273">The [IoT Hub MQTT support][lnk-devguide-mqtt] article provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31b27-274">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31b27-274">Next steps</span></span>
<span data-ttu-id="31b27-275">Most már rendelkezik megismerte eszköz twins, előfordulhat, hogy érdeklődik a következő IoT Hub fejlesztői útmutató témakörei:</span><span class="sxs-lookup"><span data-stu-id="31b27-275">Now you have learned about device twins, you may be interested in the following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="31b27-276">[Az eszközön közvetlen metódus][lnk-methods]</span><span class="sxs-lookup"><span data-stu-id="31b27-276">[Invoke a direct method on a device][lnk-methods]</span></span>
* <span data-ttu-id="31b27-277">[Több eszközön feladatok ütemezése][lnk-jobs]</span><span class="sxs-lookup"><span data-stu-id="31b27-277">[Schedule jobs on multiple devices][lnk-jobs]</span></span>

<span data-ttu-id="31b27-278">Ha azt szeretné, hogy próbálja ki azokat a jelen cikkben ismertetett fogalmakat, esetleg érdekli az IoT-központ anyagra:</span><span class="sxs-lookup"><span data-stu-id="31b27-278">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorials:</span></span>

* <span data-ttu-id="31b27-279">[A két eszköz használata][lnk-twin-tutorial]</span><span class="sxs-lookup"><span data-stu-id="31b27-279">[How to use the device twin][lnk-twin-tutorial]</span></span>
* <span data-ttu-id="31b27-280">[A két eszköztulajdonságok használata][lnk-twin-properties]</span><span class="sxs-lookup"><span data-stu-id="31b27-280">[How to use device twin properties][lnk-twin-properties]</span></span>

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
