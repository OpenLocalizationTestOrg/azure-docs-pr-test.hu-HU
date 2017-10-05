---
title: "Azure IoT Hub közvetlen módszerek megértése |} Microsoft Docs"
description: "Fejlesztői útmutató - használjon közvetlen módszerek meghívni a kódot az eszközök egy szolgáltatás alkalmazásból."
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
ms.openlocfilehash: 77e788a32097edbcb1cd4faaa45f35812eabd94a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a><span data-ttu-id="12125-103">Ismertetés és az IoT-központ közvetlen metódusok</span><span class="sxs-lookup"><span data-stu-id="12125-103">Understand and invoke direct methods from IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="12125-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="12125-104">Overview</span></span>
<span data-ttu-id="12125-105">Az IoT-központ lehetővé teszi a felhőből eszközök közvetlen módszerek meghívására.</span><span class="sxs-lookup"><span data-stu-id="12125-105">IoT Hub gives you ability to invoke direct methods on devices from the cloud.</span></span> <span data-ttu-id="12125-106">Közvetlen módszerek határoz meg egy kérelem-válasz interakció egy HTTP-hívás hasonló eszközökkel abban, hogy sikeres legyen, vagy közvetlenül (felhasználó által meghatározott időtúllépési) után sikertelen.</span><span class="sxs-lookup"><span data-stu-id="12125-106">Direct methods represent a request-reply interaction with a device similar to an HTTP call in that they succeed or fail immediately (after a user-specified timeout).</span></span> <span data-ttu-id="12125-107">Ez akkor hasznos, a forgatókönyvekben, ahol azonnali lépéseket, attól függően, hogy képesek válaszolni, például egy SMS ébresztési küld egy eszközt, ha egy eszköz kapcsolat nélküli (SMS drágább, mint egy metódus hívása folyamatban) volt-e az eszköz különböző.</span><span class="sxs-lookup"><span data-stu-id="12125-107">This is useful for scenarios where the course of immediate action is different depending on whether the device was able to respond, such as sending an SMS wake-up to a device if a device is offline (SMS being more expensive than a method call).</span></span>

<span data-ttu-id="12125-108">Minden eszköz metódus egyetlen eszközt célozza.</span><span class="sxs-lookup"><span data-stu-id="12125-108">Each device method targets a single device.</span></span> <span data-ttu-id="12125-109">[Feladatok] [ lnk-devguide-jobs] nyújtanak olyan közvetlen metódusok több eszközön, és ütemezés szerinti metódushívás leválasztott eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="12125-109">[Jobs][lnk-devguide-jobs] provide a way to invoke direct methods on multiple devices, and schedule method invocation for disconnected devices.</span></span>

<span data-ttu-id="12125-110">Bárki, aki **service csatlakozás** IoT-központ engedélyeinek indít el egy metódust az eszközön.</span><span class="sxs-lookup"><span data-stu-id="12125-110">Anyone with **service connect** permissions on IoT Hub may invoke a method on a device.</span></span>

### <a name="when-to-use"></a><span data-ttu-id="12125-111">A következő esetekben használja</span><span class="sxs-lookup"><span data-stu-id="12125-111">When to use</span></span>
<span data-ttu-id="12125-112">Közvetlen módszerek hajtsa végre a kérelem-válasz mintát, és úgy van kialakítva, az eredmény, az eszköz, például egy ventilátor bekapcsolása általában interaktív vezérléshez azonnali megerősítését igénylő kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="12125-112">Direct methods follow a request-response pattern and are meant for communications that require immediate confirmation of their result, usually interactive control of the device, for example to turn on a fan.</span></span>

<span data-ttu-id="12125-113">Tekintse meg [felhő eszközre kommunikációs útmutatást] [ lnk-c2d-guidance] bizonytalan kívánt tulajdonságai között, ha a közvetlen módszer vagy a felhő-eszközre küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="12125-113">Refer to [Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="method-lifecycle"></a><span data-ttu-id="12125-114">Módszer életciklusa</span><span class="sxs-lookup"><span data-stu-id="12125-114">Method lifecycle</span></span>
<span data-ttu-id="12125-115">Közvetlen módszerek valósíthatók meg az eszközön, és előfordulhat, hogy a módszer hasznos megfelelően példányosítani nulla vagy több bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="12125-115">Direct methods are implemented on the device and may require zero or more inputs in the method payload to correctly instantiate.</span></span> <span data-ttu-id="12125-116">A közvetlen módszer keresztül elérhető szolgáltatás URI indításakor (`{iot hub}/twins/{device id}/methods/`).</span><span class="sxs-lookup"><span data-stu-id="12125-116">You invoke a direct method through a service-facing URI (`{iot hub}/twins/{device id}/methods/`).</span></span> <span data-ttu-id="12125-117">Egy eszköz megkapja közvetlen metódusok eszközspecifikus MQTT témakör keresztül (`$iothub/methods/POST/{method name}/`).</span><span class="sxs-lookup"><span data-stu-id="12125-117">A device receives direct methods through a device-specific MQTT topic (`$iothub/methods/POST/{method name}/`).</span></span> <span data-ttu-id="12125-118">Előfordulhat, hogy támogatjuk közvetlen módszerek a további eszközoldali hálózati protokollok a jövőben.</span><span class="sxs-lookup"><span data-stu-id="12125-118">We may support direct methods on additional device-side networking protocols in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="12125-119">Az eszközön közvetlen metódus meghívásakor nevét és értékeit csak tartalmazhat US-ASCII nyomtatható alfanumerikus, kivéve a következő set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="12125-119">When you invoke a direct method on a device, property names and values can only contain US-ASCII printable alphanumeric, except any in the following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

<span data-ttu-id="12125-120">Szinkron módszerek és vagy sikeres közvetlen vagy követően az időkorlát (alapértelmezett: 30 másodperces, állítható be 3600 másodperc).</span><span class="sxs-lookup"><span data-stu-id="12125-120">Direct methods are synchronous and either succeed or fail after the timeout period (default: 30 seconds, settable up to 3600 seconds).</span></span> <span data-ttu-id="12125-121">Közvetlen módszereket hasznos interaktív olyan esetekben, ahol azt szeretné, hogy az eszköz csak, ha az eszköz nem online és a fogadó parancsok, például egy világos telefonos bekapcsolásával jár el.</span><span class="sxs-lookup"><span data-stu-id="12125-121">Direct methods are useful in interactive scenarios where you want a device to act if and only if the device is online and receiving commands, such as turning on a light from a phone.</span></span> <span data-ttu-id="12125-122">Ezekben az esetekben meg szeretné tekinteni az azonnali sikeres vagy sikertelen volt, a felhőalapú szolgáltatás működhet-e az eredmény a lehető leghamarabb.</span><span class="sxs-lookup"><span data-stu-id="12125-122">In these scenarios, you want to see an immediate success or failure so the cloud service can act on the result as soon as possible.</span></span> <span data-ttu-id="12125-123">Az eszköz néhány üzenettörzs metódus miatt előfordulhat, hogy vissza, de ez nem szükséges ehhez a metódushoz.</span><span class="sxs-lookup"><span data-stu-id="12125-123">The device may return some message body as a result of the method, but it isn't required for the method to do so.</span></span> <span data-ttu-id="12125-124">Van a rendezés nem garantálja, vagy a metódushívások bármely párhuzamossági szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="12125-124">There is no guarantee on ordering or any concurrency semantics on method calls.</span></span>

<span data-ttu-id="12125-125">Közvetlen módszer a következők: a felhő oldal csak HTTP és MQTT csak az eszköz oldaláról.</span><span class="sxs-lookup"><span data-stu-id="12125-125">Direct method are HTTP-only from the cloud side, and MQTT-only from the device side.</span></span>

<span data-ttu-id="12125-126">A hasznos módszer kérelmeit és válaszait, egy JSON-dokumentuma legfeljebb 8 KB-os.</span><span class="sxs-lookup"><span data-stu-id="12125-126">The payload for method requests and responses is a JSON document up to 8KB.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="12125-127">Referencia-témaköreit:</span><span class="sxs-lookup"><span data-stu-id="12125-127">Reference topics:</span></span>
<span data-ttu-id="12125-128">A következő témaköröket közvetlen módszerekkel kapcsolatos további információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="12125-128">The following reference topics provide you with more information about using direct methods.</span></span>

## <a name="invoke-a-direct-method-from-a-back-end-app"></a><span data-ttu-id="12125-129">A háttér-alkalmazásból közvetlen metódus</span><span class="sxs-lookup"><span data-stu-id="12125-129">Invoke a direct method from a back-end app</span></span>
### <a name="method-invocation"></a><span data-ttu-id="12125-130">A metódushívás</span><span class="sxs-lookup"><span data-stu-id="12125-130">Method invocation</span></span>
<span data-ttu-id="12125-131">Az eszközön a közvetlen módszer meghívásához HTTP hívások, amely tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="12125-131">Direct method invocations on a device are HTTP calls which comprise:</span></span>

* <span data-ttu-id="12125-132">A *URI* eszközhöz tartozó (`{iot hub}/twins/{device id}/methods/`)</span><span class="sxs-lookup"><span data-stu-id="12125-132">The *URI* specific to the device (`{iot hub}/twins/{device id}/methods/`)</span></span>
* <span data-ttu-id="12125-133">A POST *módszer*</span><span class="sxs-lookup"><span data-stu-id="12125-133">The POST *method*</span></span>
* <span data-ttu-id="12125-134">*Fejlécek* amely tartalmaz az engedélyezési, a kérelem azonosítója, tartalomtípus, és a tartalom kódolása</span><span class="sxs-lookup"><span data-stu-id="12125-134">*Headers* which contain the authorization, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="12125-135">A transzparens JSON *törzs* a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="12125-135">A transparent JSON *body* in the following format:</span></span>

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

<span data-ttu-id="12125-136">Időtúllépés másodpercben van.</span><span class="sxs-lookup"><span data-stu-id="12125-136">Timeout is in seconds.</span></span> <span data-ttu-id="12125-137">Ha időtúllépési nincs megadva, alapértelmezés szerint 30 másodperc.</span><span class="sxs-lookup"><span data-stu-id="12125-137">If timeout is not set, it defaults to 30 seconds.</span></span>

### <a name="response"></a><span data-ttu-id="12125-138">Válasz</span><span class="sxs-lookup"><span data-stu-id="12125-138">Response</span></span>
<span data-ttu-id="12125-139">A háttér-alkalmazást magában foglaló választ kap:</span><span class="sxs-lookup"><span data-stu-id="12125-139">The back-end app receives a response which comprises:</span></span>

* <span data-ttu-id="12125-140">*HTTP-állapotkód*, amellyel az IoT Hub érkező hibák, beleértve az eszközök 404 hibaüzenetet jelenleg nem csatlakoztatott</span><span class="sxs-lookup"><span data-stu-id="12125-140">*HTTP status code*, which is used for errors coming from the IoT Hub, including a 404 error for devices not currently connected</span></span>
* <span data-ttu-id="12125-141">*Fejlécek* amely tartalmazza az ETag, a kérelem azonosítója, tartalomtípus, és a tartalom kódolása</span><span class="sxs-lookup"><span data-stu-id="12125-141">*Headers* which contain the ETag, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="12125-142">A JSON *törzs* a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="12125-142">A JSON *body* in the following format:</span></span>

```
{
    "status" : 201,
    "payload" : {...}
}
```

   <span data-ttu-id="12125-143">Mindkét `status` és `body` az eszköz által biztosított és a használt szeretne válaszolni az eszköz saját állapotkód és/vagy a leírását.</span><span class="sxs-lookup"><span data-stu-id="12125-143">Both `status` and `body` are provided by the device and used to respond with the device's own status code and/or description.</span></span>

## <a name="handle-a-direct-method-on-a-device"></a><span data-ttu-id="12125-144">Kezeli az eszközön a közvetlen módszer</span><span class="sxs-lookup"><span data-stu-id="12125-144">Handle a direct method on a device</span></span>
### <a name="method-invocation"></a><span data-ttu-id="12125-145">A metódushívás</span><span class="sxs-lookup"><span data-stu-id="12125-145">Method invocation</span></span>
<span data-ttu-id="12125-146">Eszköz megkapja a MQTT témakör közvetlen metódusú kérelmeket:`$iothub/methods/POST/{method name}/?$rid={request id}`</span><span class="sxs-lookup"><span data-stu-id="12125-146">Devices receive direct method requests on the MQTT topic: `$iothub/methods/POST/{method name}/?$rid={request id}`</span></span>

<span data-ttu-id="12125-147">A szervezet, amely az eszköz megkapja a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="12125-147">The body which the device receives is in the following format:</span></span>

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

<span data-ttu-id="12125-148">A metóduskérelmek QoS 0.</span><span class="sxs-lookup"><span data-stu-id="12125-148">Method requests are QoS 0.</span></span>

### <a name="response"></a><span data-ttu-id="12125-149">Válasz</span><span class="sxs-lookup"><span data-stu-id="12125-149">Response</span></span>
<span data-ttu-id="12125-150">Az eszköz küld válaszokat `$iothub/methods/res/{status}/?$rid={request id}`, ahol:</span><span class="sxs-lookup"><span data-stu-id="12125-150">The device sends responses to `$iothub/methods/res/{status}/?$rid={request id}`, where:</span></span>

* <span data-ttu-id="12125-151">A `status` tulajdonság metódus végrehajtása a megadott eszköz állapotát.</span><span class="sxs-lookup"><span data-stu-id="12125-151">The `status` property is the device-supplied status of method execution.</span></span>
* <span data-ttu-id="12125-152">A `$rid` tulajdonsága az IoT-központ kapott metódushívás a kérelem azonosítója.</span><span class="sxs-lookup"><span data-stu-id="12125-152">The `$rid` property is the request ID from the method invocation received from IoT Hub.</span></span>

<span data-ttu-id="12125-153">A szervezet az eszköz állítja be, és bármely állapota lehet.</span><span class="sxs-lookup"><span data-stu-id="12125-153">The body is set by the device and can be any status.</span></span>

## <a name="additional-reference-material"></a><span data-ttu-id="12125-154">További referenciaanyag</span><span class="sxs-lookup"><span data-stu-id="12125-154">Additional reference material</span></span>
<span data-ttu-id="12125-155">Az IoT Hub fejlesztői útmutató más hivatkozás témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="12125-155">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="12125-156">[IoT-központok végpontjai] [ lnk-endpoints] ismerteti a különböző végpontok, amelyek minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek.</span><span class="sxs-lookup"><span data-stu-id="12125-156">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="12125-157">[Sávszélesség-szabályozási és kvóták] [ lnk-quotas] ismerteti a kvótákat, az IoT-központ szolgáltatás és a sávszélesség-szabályozási viselkedését történik, ha a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="12125-157">[Throttling and quotas][lnk-quotas] describes the quotas that apply to the IoT Hub service and the throttling behavior to expect when you use the service.</span></span>
* <span data-ttu-id="12125-158">[Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] felsorolja a különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.</span><span class="sxs-lookup"><span data-stu-id="12125-158">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="12125-159">[Az IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ lnk-query] az IoT-központ lekérdezési nyelv segítségével adatok lekérését az IoT-központ az eszköz twins és feladatok ismertetése.</span><span class="sxs-lookup"><span data-stu-id="12125-159">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes the IoT Hub query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="12125-160">[Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] IoT-központ támogatásával kapcsolatos további információkat biztosít a MQTT protokoll.</span><span class="sxs-lookup"><span data-stu-id="12125-160">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12125-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12125-161">Next steps</span></span>
<span data-ttu-id="12125-162">Most közvetlen módszerek használatával megtanulhatta, esetleg megváltozása a következő IoT Hub fejlesztői útmutató témakörében:</span><span class="sxs-lookup"><span data-stu-id="12125-162">Now you have learned how to use direct methods, you may be interested in the following IoT Hub developer guide topic:</span></span>

* <span data-ttu-id="12125-163">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="12125-163">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="12125-164">Ha azt szeretné, hogy próbálja ki azokat a jelen cikkben ismertetett fogalmakat, esetleg megváltozása a következő IoT Hub-oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="12125-164">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorial:</span></span>

* <span data-ttu-id="12125-165">[Közvetlen módszerekkel][lnk-methods-tutorial]</span><span class="sxs-lookup"><span data-stu-id="12125-165">[Use direct methods][lnk-methods-tutorial]</span></span>

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
