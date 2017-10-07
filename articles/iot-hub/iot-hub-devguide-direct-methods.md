---
title: "Azure IoT Hub aaaUnderstand közvetlen módszerek |} Microsoft Docs"
description: "Fejlesztői útmutató - használható közvetlen módszerek tooinvoke kódot az eszközök egy szolgáltatás alkalmazásból."
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d15d44a0c3e1d1cda1669c1ed011c2f932e3d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a><span data-ttu-id="740e3-103">Ismertetés és az IoT-központ közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="740e3-103">Understand and invoke direct methods from IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="740e3-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="740e3-104">Overview</span></span>
<span data-ttu-id="740e3-105">Az IoT-központ lehetővé teszi az képességét tooinvoke közvetlen módszerek hello felhőből eszközökön.</span><span class="sxs-lookup"><span data-stu-id="740e3-105">IoT Hub gives you ability tooinvoke direct methods on devices from hello cloud.</span></span> <span data-ttu-id="740e3-106">Közvetlen módszerek határoz meg egy eszköz hasonló tooan HTTP hívható meg abban, hogy sikeres legyen, vagy közvetlenül (felhasználó által meghatározott időtúllépési) után sikertelen a kérelem-válasz interakció.</span><span class="sxs-lookup"><span data-stu-id="740e3-106">Direct methods represent a request-reply interaction with a device similar tooan HTTP call in that they succeed or fail immediately (after a user-specified timeout).</span></span> <span data-ttu-id="740e3-107">Ez akkor hasznos, a forgatókönyvekben, ahol hello intézkedés azonnali hello eszköz volt-e képes toorespond, például egy SMS-ébresztési tooa eszköz küldése, ha egy eszköz kapcsolat nélküli (SMS drágább, mint egy metódus hívása folyamatban) függően eltérnek.</span><span class="sxs-lookup"><span data-stu-id="740e3-107">This is useful for scenarios where hello course of immediate action is different depending on whether hello device was able toorespond, such as sending an SMS wake-up tooa device if a device is offline (SMS being more expensive than a method call).</span></span>

<span data-ttu-id="740e3-108">Minden eszköz metódus egyetlen eszközt célozza.</span><span class="sxs-lookup"><span data-stu-id="740e3-108">Each device method targets a single device.</span></span> <span data-ttu-id="740e3-109">[Feladatok] [ lnk-devguide-jobs] biztosítanak egy módon tooinvoke közvetlen módszerek több eszközön, és ütemezés szerinti metódushívás leválasztott eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="740e3-109">[Jobs][lnk-devguide-jobs] provide a way tooinvoke direct methods on multiple devices, and schedule method invocation for disconnected devices.</span></span>

<span data-ttu-id="740e3-110">Bárki, aki **service csatlakozás** IoT-központ engedélyeinek indít el egy metódust az eszközön.</span><span class="sxs-lookup"><span data-stu-id="740e3-110">Anyone with **service connect** permissions on IoT Hub may invoke a method on a device.</span></span>

### <a name="when-toouse"></a><span data-ttu-id="740e3-111">Ha toouse</span><span class="sxs-lookup"><span data-stu-id="740e3-111">When toouse</span></span>
<span data-ttu-id="740e3-112">Közvetlen módszerek hajtsa végre a kérelem-válasz mintát, és úgy van kialakítva, az eredmény, általában interaktív vezérléshez hello eszköz, például egy ventilátor a tooturn azonnali megerősítését igénylő kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="740e3-112">Direct methods follow a request-response pattern and are meant for communications that require immediate confirmation of their result, usually interactive control of hello device, for example tooturn on a fan.</span></span>

<span data-ttu-id="740e3-113">Tekintse meg a túl[felhő eszközre kommunikációs útmutatást] [ lnk-c2d-guidance] bizonytalan kívánt tulajdonságai között, ha a közvetlen módszer vagy a felhő-eszközre küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="740e3-113">Refer too[Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="method-lifecycle"></a><span data-ttu-id="740e3-114">Módszer életciklusa</span><span class="sxs-lookup"><span data-stu-id="740e3-114">Method lifecycle</span></span>
<span data-ttu-id="740e3-115">Közvetlen módszerek hello eszközön megvalósított, és előfordulhat, hogy nulla vagy több bemenetében benne hello módszer hasznos toocorrectly hozható létre.</span><span class="sxs-lookup"><span data-stu-id="740e3-115">Direct methods are implemented on hello device and may require zero or more inputs in hello method payload toocorrectly instantiate.</span></span> <span data-ttu-id="740e3-116">A közvetlen módszer keresztül elérhető szolgáltatás URI indításakor (`{iot hub}/twins/{device id}/methods/`).</span><span class="sxs-lookup"><span data-stu-id="740e3-116">You invoke a direct method through a service-facing URI (`{iot hub}/twins/{device id}/methods/`).</span></span> <span data-ttu-id="740e3-117">Egy eszköz megkapja közvetlen metódusok eszközspecifikus MQTT témakör keresztül (`$iothub/methods/POST/{method name}/`).</span><span class="sxs-lookup"><span data-stu-id="740e3-117">A device receives direct methods through a device-specific MQTT topic (`$iothub/methods/POST/{method name}/`).</span></span> <span data-ttu-id="740e3-118">A további eszközoldali hálózati protokollok a jövőbeli hello közvetlen módszerek előfordulhat, hogy támogatott.</span><span class="sxs-lookup"><span data-stu-id="740e3-118">We may support direct methods on additional device-side networking protocols in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="740e3-119">Az eszközön közvetlen metódus meghívásakor nevét és értékeit csak tartalmazhat US-ASCII nyomtatható alfanumerikus, kivéve a következő set hello: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="740e3-119">When you invoke a direct method on a device, property names and values can only contain US-ASCII printable alphanumeric, except any in hello following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

<span data-ttu-id="740e3-120">Szinkron módszerek és vagy sikeres közvetlen vagy követően hello időkorlát (alapértelmezett: 30 másodperces, állítható be too3600 másodperc).</span><span class="sxs-lookup"><span data-stu-id="740e3-120">Direct methods are synchronous and either succeed or fail after hello timeout period (default: 30 seconds, settable up too3600 seconds).</span></span> <span data-ttu-id="740e3-121">Közvetlen módszereket kívánt egy eszköz tooact csak ha hello eszköz online és a fogadó parancsok, például egy világos telefonos bekapcsolásával interaktív esetekben hasznos.</span><span class="sxs-lookup"><span data-stu-id="740e3-121">Direct methods are useful in interactive scenarios where you want a device tooact if and only if hello device is online and receiving commands, such as turning on a light from a phone.</span></span> <span data-ttu-id="740e3-122">Ezekben az esetekben érdemes toosee egy azonnali sikeres vagy sikertelen volt, hello felhőalapú szolgáltatás működhet hello eredménye a lehető leghamarabb.</span><span class="sxs-lookup"><span data-stu-id="740e3-122">In these scenarios, you want toosee an immediate success or failure so hello cloud service can act on hello result as soon as possible.</span></span> <span data-ttu-id="740e3-123">hello eszköz térhetnek vissza néhány üzenettörzs hello metódus miatt, de ez nem szükséges hello metódus toodo így.</span><span class="sxs-lookup"><span data-stu-id="740e3-123">hello device may return some message body as a result of hello method, but it isn't required for hello method toodo so.</span></span> <span data-ttu-id="740e3-124">Van a rendezés nem garantálja, vagy a metódushívások bármely párhuzamossági szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="740e3-124">There is no guarantee on ordering or any concurrency semantics on method calls.</span></span>

<span data-ttu-id="740e3-125">Közvetlen módszer a következők: csak HTTP hello felhő oldaláról, és csak MQTT hello eszköz oldaláról.</span><span class="sxs-lookup"><span data-stu-id="740e3-125">Direct method are HTTP-only from hello cloud side, and MQTT-only from hello device side.</span></span>

<span data-ttu-id="740e3-126">hello hasznos módszer kérelem és válasz egy JSON-dokumentum mentése too8KB.</span><span class="sxs-lookup"><span data-stu-id="740e3-126">hello payload for method requests and responses is a JSON document up too8KB.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="740e3-127">Referencia-témaköreit:</span><span class="sxs-lookup"><span data-stu-id="740e3-127">Reference topics:</span></span>
<span data-ttu-id="740e3-128">hello következő referencia-témakörökre biztosít közvetlen módszerek használatáról további információkat.</span><span class="sxs-lookup"><span data-stu-id="740e3-128">hello following reference topics provide you with more information about using direct methods.</span></span>

## <a name="invoke-a-direct-method-from-a-back-end-app"></a><span data-ttu-id="740e3-129">A háttér-alkalmazásból közvetlen metódus</span><span class="sxs-lookup"><span data-stu-id="740e3-129">Invoke a direct method from a back-end app</span></span>
### <a name="method-invocation"></a><span data-ttu-id="740e3-130">A metódushívás</span><span class="sxs-lookup"><span data-stu-id="740e3-130">Method invocation</span></span>
<span data-ttu-id="740e3-131">Az eszközön a közvetlen módszer meghívásához HTTP hívások, amely tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="740e3-131">Direct method invocations on a device are HTTP calls which comprise:</span></span>

* <span data-ttu-id="740e3-132">Hello *URI* adott toohello eszköz (`{iot hub}/twins/{device id}/methods/`)</span><span class="sxs-lookup"><span data-stu-id="740e3-132">hello *URI* specific toohello device (`{iot hub}/twins/{device id}/methods/`)</span></span>
* <span data-ttu-id="740e3-133">hello POST *módszer*</span><span class="sxs-lookup"><span data-stu-id="740e3-133">hello POST *method*</span></span>
* <span data-ttu-id="740e3-134">*Fejlécek* amely tartalmaz hello engedélyezési, a kérelem azonosítója, tartalomtípus, és a tartalom kódolása</span><span class="sxs-lookup"><span data-stu-id="740e3-134">*Headers* which contain hello authorization, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="740e3-135">Transzparens JSON *törzs* a hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="740e3-135">A transparent JSON *body* in hello following format:</span></span>

```
{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

<span data-ttu-id="740e3-136">Időtúllépés másodpercben van.</span><span class="sxs-lookup"><span data-stu-id="740e3-136">Timeout is in seconds.</span></span> <span data-ttu-id="740e3-137">Ha időtúllépési nincs beállítva, alapértelmezett too30 másodperc.</span><span class="sxs-lookup"><span data-stu-id="740e3-137">If timeout is not set, it defaults too30 seconds.</span></span>

### <a name="response"></a><span data-ttu-id="740e3-138">Válasz</span><span class="sxs-lookup"><span data-stu-id="740e3-138">Response</span></span>
<span data-ttu-id="740e3-139">hello háttér-alkalmazást magában foglaló választ kap:</span><span class="sxs-lookup"><span data-stu-id="740e3-139">hello back-end app receives a response which comprises:</span></span>

* <span data-ttu-id="740e3-140">*HTTP-állapotkód*, amellyel az IoT-központ hello érkező hibák, beleértve az eszközök 404 hibaüzenetet jelenleg nem csatlakoztatott</span><span class="sxs-lookup"><span data-stu-id="740e3-140">*HTTP status code*, which is used for errors coming from hello IoT Hub, including a 404 error for devices not currently connected</span></span>
* <span data-ttu-id="740e3-141">*Fejlécek* amely hello ETag tartalmaz, a kérelem azonosítója, tartalomtípus, és a tartalom kódolása</span><span class="sxs-lookup"><span data-stu-id="740e3-141">*Headers* which contain hello ETag, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="740e3-142">A JSON *törzs* a hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="740e3-142">A JSON *body* in hello following format:</span></span>

```
{
    "status" : 201,
    "payload" : {...}
}
```

   <span data-ttu-id="740e3-143">Mindkét `status` és `body` hello eszköz által biztosított és toorespond használt hello eszköz saját állapotkód és/vagy leírása.</span><span class="sxs-lookup"><span data-stu-id="740e3-143">Both `status` and `body` are provided by hello device and used toorespond with hello device's own status code and/or description.</span></span>

## <a name="handle-a-direct-method-on-a-device"></a><span data-ttu-id="740e3-144">Kezeli az eszközön a közvetlen módszer</span><span class="sxs-lookup"><span data-stu-id="740e3-144">Handle a direct method on a device</span></span>
### <a name="method-invocation"></a><span data-ttu-id="740e3-145">A metódushívás</span><span class="sxs-lookup"><span data-stu-id="740e3-145">Method invocation</span></span>
<span data-ttu-id="740e3-146">Eszköz megkapja a közvetlen módszer kérelmeket a hello MQTT témakör:`$iothub/methods/POST/{method name}/?$rid={request id}`</span><span class="sxs-lookup"><span data-stu-id="740e3-146">Devices receive direct method requests on hello MQTT topic: `$iothub/methods/POST/{method name}/?$rid={request id}`</span></span>

<span data-ttu-id="740e3-147">mely hello eszköz megkapja hello törzs hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="740e3-147">hello body which hello device receives is in hello following format:</span></span>

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

<span data-ttu-id="740e3-148">A metóduskérelmek QoS 0.</span><span class="sxs-lookup"><span data-stu-id="740e3-148">Method requests are QoS 0.</span></span>

### <a name="response"></a><span data-ttu-id="740e3-149">Válasz</span><span class="sxs-lookup"><span data-stu-id="740e3-149">Response</span></span>
<span data-ttu-id="740e3-150">hello eszköz küldi a válaszokat túl`$iothub/methods/res/{status}/?$rid={request id}`, ahol:</span><span class="sxs-lookup"><span data-stu-id="740e3-150">hello device sends responses too`$iothub/methods/res/{status}/?$rid={request id}`, where:</span></span>

* <span data-ttu-id="740e3-151">Hello `status` tulajdonsága hello metódus végrehajtása a megadott eszköz állapotát.</span><span class="sxs-lookup"><span data-stu-id="740e3-151">hello `status` property is hello device-supplied status of method execution.</span></span>
* <span data-ttu-id="740e3-152">Hello `$rid` tulajdonsága az IoT-központ kapott hello metódushívás hello Kérelemazonosító.</span><span class="sxs-lookup"><span data-stu-id="740e3-152">hello `$rid` property is hello request ID from hello method invocation received from IoT Hub.</span></span>

<span data-ttu-id="740e3-153">hello szervezet hello eszköz állítja be, és bármely állapota lehet.</span><span class="sxs-lookup"><span data-stu-id="740e3-153">hello body is set by hello device and can be any status.</span></span>

## <a name="additional-reference-material"></a><span data-ttu-id="740e3-154">További referenciaanyag</span><span class="sxs-lookup"><span data-stu-id="740e3-154">Additional reference material</span></span>
<span data-ttu-id="740e3-155">Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="740e3-155">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="740e3-156">[IoT-központok végpontjai] [ lnk-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello.</span><span class="sxs-lookup"><span data-stu-id="740e3-156">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="740e3-157">[Sávszélesség-szabályozási és kvóták] [ lnk-quotas] toohello IoT-központ szolgáltatás és szabályozási viselkedés tooexpect hello hello szolgáltatás használatakor hello kvóták ismerteti.</span><span class="sxs-lookup"><span data-stu-id="740e3-157">[Throttling and quotas][lnk-quotas] describes hello quotas that apply toohello IoT Hub service and hello throttling behavior tooexpect when you use hello service.</span></span>
* <span data-ttu-id="740e3-158">[Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] listák hello különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.</span><span class="sxs-lookup"><span data-stu-id="740e3-158">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="740e3-159">[Az IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] hello tooretrieve IoT Hub-ből származó adataival az eszköz twins és feladatok is használhatja az IoT-központ lekérdezési nyelv ismerteti.</span><span class="sxs-lookup"><span data-stu-id="740e3-159">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes hello IoT Hub query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="740e3-160">[Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] hello MQTT protokoll IoT-központ támogatásával kapcsolatos további információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="740e3-160">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="740e3-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="740e3-161">Next steps</span></span>
<span data-ttu-id="740e3-162">Most megtanulta, hogyan toouse közvetlen módszerek, esetleg következő IoT Hub fejlesztői útmutató témakör hello iránt érdeklődik:</span><span class="sxs-lookup"><span data-stu-id="740e3-162">Now you have learned how toouse direct methods, you may be interested in hello following IoT Hub developer guide topic:</span></span>

* <span data-ttu-id="740e3-163">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="740e3-163">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="740e3-164">Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ az oktatóanyag következő hello iránt érdeklődik:</span><span class="sxs-lookup"><span data-stu-id="740e3-164">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorial:</span></span>

* <span data-ttu-id="740e3-165">[Közvetlen módszerekkel][lnk-methods-tutorial]</span><span class="sxs-lookup"><span data-stu-id="740e3-165">[Use direct methods][lnk-methods-tutorial]</span></span>

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
