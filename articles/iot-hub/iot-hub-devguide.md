---
title: "Azure IoT hub fejlesztői útmutató |} Microsoft Docs"
description: "Az Azure IoT Hub fejlesztői útmutató tartalmaz beszélgetéseket végpontok, biztonsági, az identitásjegyzékhez, kezelés, közvetlen módszerek, eszköz twins, fájlfeltöltéseket, feladatok, az IoT-központ lekérdező nyelv, és üzenetkezelés."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: d534ff9d-2de5-4995-bb2d-84a02693cb2e
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: adb9a12899e9040cd83d522c734448989636fe29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-hub-developer-guide"></a><span data-ttu-id="c39d2-103">Az Azure IoT Hub fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="c39d2-103">Azure IoT Hub developer guide</span></span>

<span data-ttu-id="c39d2-104">Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere.</span><span class="sxs-lookup"><span data-stu-id="c39d2-104">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span>

<span data-ttu-id="c39d2-105">Az Azure IoT-központ biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c39d2-105">Azure IoT Hub provides you with:</span></span>

* <span data-ttu-id="c39d2-106">A biztonságos kommunikáció eszközönkénti biztonsági hitelesítő adatok használatával, és hozzáférés-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="c39d2-106">Secure communications by using per-device security credentials and access control.</span></span>
* <span data-ttu-id="c39d2-107">Több eszköz-felhő és a felhő eszközre kapacitású kommunikációs lehetőségekről.</span><span class="sxs-lookup"><span data-stu-id="c39d2-107">Multiple device-to-cloud and cloud-to-device hyper-scale communication options.</span></span>
* <span data-ttu-id="c39d2-108">Eszközönkénti állapotadatokat és a metaadatok tárolása lekérdezhető.</span><span class="sxs-lookup"><span data-stu-id="c39d2-108">Queryable storage of per-device state information and meta-data.</span></span>
* <span data-ttu-id="c39d2-109">Egyszerű eszközkapcsolatok eszköz könyvtárakkal a legnépszerűbb nyelvekhez és platformokhoz.</span><span class="sxs-lookup"><span data-stu-id="c39d2-109">Easy device connectivity with device libraries for the most popular languages and platforms.</span></span>

<span data-ttu-id="c39d2-110">Az IoT Hub fejlesztői útmutató tartalmazza a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="c39d2-110">This IoT Hub developer guide includes the following articles:</span></span>

* <span data-ttu-id="c39d2-111">[Eszköz-felhő kommunikációs útmutatást] [ lnk-d2c-guidance] segítségével választhat eszköz a felhőbe küldött üzeneteket, az eszköz iker jelentett tulajdonságok és a fájl feltöltése között.</span><span class="sxs-lookup"><span data-stu-id="c39d2-111">[Device-to-cloud communication guidance][lnk-d2c-guidance] helps you choose between device-to-cloud messages, device twin's reported properties, and file upload.</span></span>
* <span data-ttu-id="c39d2-112">[Útmutatás felhő-és eszköz közötti kommunikációt] [ lnk-c2d-guidance] segítségével választhat a közvetlen módszer eszköz iker kívánt tulajdonságokkal és felhő-eszközre küldött üzenetek között.</span><span class="sxs-lookup"><span data-stu-id="c39d2-112">[Cloud-to-device communication guidance][lnk-c2d-guidance] helps you choose between direct methods, device twin's desired properties, and cloud-to-device messages.</span></span>
* <span data-ttu-id="c39d2-113">[Eszköz-felhő és a felhő-eszközt az IoT-központ az üzenetküldési] [ devguide-messaging] üzenetkezelési szolgáltatások (eszközről a felhőbe és felhő eszközre), amely az IoT-központ elérhetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="c39d2-113">[Device-to-cloud and cloud-to-device messaging with IoT Hub][devguide-messaging] describes the messaging features (device-to-cloud and cloud-to-device) that IoT Hub exposes.</span></span>
  * <span data-ttu-id="c39d2-114">[Eszköz-felhő üzenetek küldése az IoT-központ][devguide-messages-d2c].</span><span class="sxs-lookup"><span data-stu-id="c39d2-114">[Send device-to-cloud messages to IoT Hub][devguide-messages-d2c].</span></span>
  * <span data-ttu-id="c39d2-115">[Eszköz a felhőbe küldött üzeneteket beolvasni a beépített végpont][devguide-builtin].</span><span class="sxs-lookup"><span data-stu-id="c39d2-115">[Read device-to-cloud messages from the built-in endpoint][devguide-builtin].</span></span>
  * <span data-ttu-id="c39d2-116">[Egyéni végpontokat és útválasztási szabályokat használja az eszköz a felhőbe küldött üzeneteket][devguide-custom].</span><span class="sxs-lookup"><span data-stu-id="c39d2-116">[Use custom endpoints and routing rules for device-to-cloud messages][devguide-custom].</span></span>
  * <span data-ttu-id="c39d2-117">[Felhő-eszközre küldött üzenetek küldése az IoT-központ][devguide-messages-c2d].</span><span class="sxs-lookup"><span data-stu-id="c39d2-117">[Send cloud-to-device messages from IoT Hub][devguide-messages-c2d].</span></span>
  * <span data-ttu-id="c39d2-118">[Hozzon létre, és az IoT-központ olvashatja][devguide-format].</span><span class="sxs-lookup"><span data-stu-id="c39d2-118">[Create and read IoT Hub messages][devguide-format].</span></span>
* <span data-ttu-id="c39d2-119">[Az eszközről tölt fel] [ devguide-upload] ismerteti, hogyan tölthet fájlok az eszközről.</span><span class="sxs-lookup"><span data-stu-id="c39d2-119">[Upload files from a device][devguide-upload] describes how you can upload files from a device.</span></span> <span data-ttu-id="c39d2-120">A cikk is magában foglalja a tudnivalókat az értesítéseket küldhet a feltöltési folyamat.</span><span class="sxs-lookup"><span data-stu-id="c39d2-120">The article also includes information about topics such as the notifications the upload process can send.</span></span>
* <span data-ttu-id="c39d2-121">[Az IoT Hub eszköz identitáskezelést] [ devguide-identities] milyen információkat ismerteti, minden IoT-központ beállításjegyzék identitástárolók, és hogyan elérheti és módosíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="c39d2-121">[Manage device identities in IoT Hub][devguide-identities] describes what information each IoT hub's identity registry stores, and how you can access and modify it.</span></span>
* <span data-ttu-id="c39d2-122">[Az IoT-központ való hozzáférés szabályozása] [ devguide-security] a biztonsági modell segítségével hozzáférést biztosíthat az IoT-központ funkciót mindkét eszközökhöz és összetevők a felhőalapú ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c39d2-122">[Control access to IoT Hub][devguide-security] describes the security model used to grant access to IoT Hub functionality for both devices and cloud components.</span></span> <span data-ttu-id="c39d2-123">A cikk tokeneket és X.509-tanúsítványokat, és az engedélyek megadásához részleteit használatával kapcsolatos információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c39d2-123">The article includes information about using tokens and X.509 certificates, and details of the permissions you can grant.</span></span>
* <span data-ttu-id="c39d2-124">[Eszköz twins segítségével szinkronizálja az állapotot és konfigurációk] [ devguide-device-twins] ismerteti a *eszköz iker* koncepciót és a funkciókat, például az eszköz szinkronizálása az eszköz iker teszi elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="c39d2-124">[Use device twins to synchronize state and configurations][devguide-device-twins] describes the *device twin* concept and the functionality it exposes such as synchronizing a device with its device twin.</span></span> <span data-ttu-id="c39d2-125">A cikk egy eszköz iker tárolt adatokra vonatkozó információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c39d2-125">The article includes information about the data stored in a device twin.</span></span>
* <span data-ttu-id="c39d2-126">[Az eszközön közvetlen metódus] [ devguide-directmethods] az életciklus közvetlen módszer, információ a metódusok a háttér-alkalmazás egy eszközön, és a közvetlen módszer az eszköz kezelését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c39d2-126">[Invoke a direct method on a device][devguide-directmethods] describes the lifecycle of a direct method, information about how to invoke methods on a device from your back-end app and handle the direct method on your device.</span></span>
* <span data-ttu-id="c39d2-127">[Több eszközön feladatok ütemezése] [ devguide-jobs] ismerteti, hogyan több eszközön futó feladatot is ütemezheti.</span><span class="sxs-lookup"><span data-stu-id="c39d2-127">[Schedule jobs on multiple devices][devguide-jobs] describes how you can schedule jobs on multiple devices.</span></span> <span data-ttu-id="c39d2-128">A cikk ismerteti, hogyan, feladatokat végrehajtó egy közvetlen módszer, egy eszközt egy eszközt a két frissítési feladatok küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="c39d2-128">The article describes how to submit jobs that perform tasks as executing a direct method, updating a device using a device twin.</span></span> <span data-ttu-id="c39d2-129">Emellett bemutatja, hogyan lehet a feladat állapotának lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="c39d2-129">It also describes how to query the status of a job.</span></span>
* <span data-ttu-id="c39d2-130">[Olyan kommunikációs protokollt hivatkozhat - a] [ devguide-protocol] írja le a kommunikációs protokollt, hogy az IoT-Központ támogatja az eszközök közötti kommunikáció, és nyitva kell lennie portokat sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="c39d2-130">[Reference - choose a communication protocol][devguide-protocol] describes the communication protocols that IoT Hub supports for device communication and lists the ports that should be open.</span></span>
* <span data-ttu-id="c39d2-131">[Referencia - IoT-központok végpontjai] [ devguide-endpoints] ismerteti a különböző végpontok, amelyek minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek.</span><span class="sxs-lookup"><span data-stu-id="c39d2-131">[Reference - IoT Hub endpoints][devguide-endpoints] describes the various endpoints that each IoT hub exposes for runtime and management operations.</span></span> <span data-ttu-id="c39d2-132">A cikk is ismerteti, hogyan hozhat létre további végpontokat az IoT hub, és az IoT-központok végpontjai nem szabványos helyzetekben az eszközök kapcsolódásának engedélyezése mező átjáró használatával.</span><span class="sxs-lookup"><span data-stu-id="c39d2-132">The article also describes how you can create additional endpoints in your IoT hub, and how to use a field gateway to enable devices connectivity to your IoT Hub endpoints in non-standard scenarios.</span></span>
* <span data-ttu-id="c39d2-133">[Referencia - IoT-központ lekérdezési nyelv eszköz twins, feladatok és üzenet útválasztási] [ devguide-query] , hogy az IoT-központ lekérdező nyelv, amely lehetővé teszi az adatok lekérését a hub eszköz twins és feladatokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c39d2-133">[Reference - IoT Hub query language for device twins, jobs, and message routing][devguide-query] describes that IoT Hub query language that enables you to retrieve information from your hub about your device twins and jobs.</span></span>
* <span data-ttu-id="c39d2-134">[Referencia - kvóták és sávszélesség-szabályozási] [ devguide-quotas] foglalja össze a kvótákat, az IoT-központ szolgáltatás és a sávszélesség-szabályozási viselkedését, tekintse meg, ha több mint egy kvótát kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="c39d2-134">[Reference - quotas and throttling][devguide-quotas] summarizes the quotas set in the IoT Hub service and the throttling behavior you can expect to see when you exceed a quota.</span></span>
* <span data-ttu-id="c39d2-135">[Hivatkozás - díjszabás] [ devguide-pricing] különböző Termékváltozatai és az IoT Hub és részletek hogyan a különböző IoT-központ funkciók mérése üzenetekként IoT-központ által díjszabása kapcsolatos általános tudnivalókat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c39d2-135">[Reference - pricing][devguide-pricing] provides general information on different SKUs and pricing for IoT Hub and details on how the various IoT Hub functionalities are metered as messages by IoT Hub.</span></span>
* <span data-ttu-id="c39d2-136">[Referencia - eszköz és az SDK szolgáltatás] [ devguide-sdks] sorolja fel az Azure IoT SDK-k segítségével kommunikáló az IoT hub eszköz és a szolgáltatás alkalmazások fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="c39d2-136">[Reference - Device and service SDKs][devguide-sdks] lists the Azure IoT SDKs you can use to develop both device and service apps that interact with your IoT hub.</span></span> <span data-ttu-id="c39d2-137">A cikk online API dokumentációjára mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c39d2-137">The article includes links to online API documentation.</span></span>
* <span data-ttu-id="c39d2-138">[Referencia - IoT Hub MQTT támogatási] [ devguide-mqtt] részletes információt nyújt arról, hogyan IoT-központ a MQTT protokoll használatát támogatja.</span><span class="sxs-lookup"><span data-stu-id="c39d2-138">[Reference - IoT Hub MQTT support][devguide-mqtt] provides detailed information about how IoT Hub supports the MQTT protocol.</span></span> <span data-ttu-id="c39d2-139">A cikk ismerteti a beépített az Azure IoT SDK-k MQTT protokoll támogatását, és közvetlenül a MQTT protokollal kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="c39d2-139">The article describes the support for the MQTT protocol built-in to the Azure IoT SDKs and provides information about using the MQTT protocol directly.</span></span>
* <span data-ttu-id="c39d2-140">[Szószedet] [ devguide-glossary] általános IoT Hub-mal kapcsolatos kifejezések listája.</span><span class="sxs-lookup"><span data-stu-id="c39d2-140">[Glossary][devguide-glossary] a list of common IoT Hub-related terms.</span></span>

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[devguide-messages-d2c]: iot-hub-devguide-messages-d2c.md
[devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[devguide-custom]: iot-hub-devguide-messages-read-custom.md
[devguide-messages-c2d]: iot-hub-devguide-messages-c2d.md
[devguide-format]: iot-hub-devguide-messages-construct.md
[devguide-protocol]: iot-hub-devguide-protocols.md
